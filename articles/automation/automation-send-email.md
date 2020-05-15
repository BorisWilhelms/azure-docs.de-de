---
title: Senden einer E-Mail aus einem Azure Automation-Runbook
description: Hier erfahren Sie, wie Sie mithilfe von SendGrid eine E-Mail aus einem Runbook senden.
services: automation
ms.subservice: process-automation
ms.date: 07/15/2019
ms.topic: tutorial
ms.openlocfilehash: 4d825dee469497cbb56a91c913ff3ac51963058b
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2020
ms.locfileid: "82855691"
---
# <a name="tutorial-send-an-email-from-an-azure-automation-runbook"></a>Tutorial: Senden einer E-Mail aus einem Azure Automation-Runbook

Sie können unter Verwendung von PowerShell und [SendGrid](https://sendgrid.com/solutions) eine E-Mail aus einem Runbook senden. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
>
> * Erstellen Sie eine Azure Key Vault-Instanz.
> * Speichern Sie Ihren `SendGrid`-API-Schlüssel im Schlüsseltresor.
> * Erstellen Sie ein wiederverwendbares Runbook, das mithilfe eines in [Azure KeyVault](/azure/key-vault/) gespeicherten API-Schlüssels Ihren API-Schlüssel abruft und eine E-Mail sendet.

>[!NOTE]
>Dieser Artikel wurde aktualisiert und beinhaltet jetzt das neue Az-Modul von Azure PowerShell. Sie können das AzureRM-Modul weiterhin verwenden, das bis mindestens Dezember 2020 weiterhin Fehlerbehebungen erhält. Weitere Informationen zum neuen Az-Modul und zur Kompatibilität mit AzureRM finden Sie unter [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Einführung in das neue Az-Modul von Azure PowerShell). Installationsanweisungen für das Az-Modul auf Ihrem Hybrid Runbook Worker finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). In Ihrem Automation-Konto können Sie die Module mithilfe der Informationen unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](automation-update-azure-modules.md) auf die neueste Version aktualisieren.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorials müssen folgende Voraussetzungen erfüllt sein:

* Azure-Abonnement: Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich für ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) registrieren.
* [Erstellen eines SendGrid-Kontos](/azure/sendgrid-dotnet-how-to-send-email#create-a-sendgrid-account)
* [Automation-Konto](automation-offering-get-started.md) mit **Az**-Modulen und [ausführender Verbindung](automation-create-runas-account.md) zum Speichern und Ausführen des Runbooks

## <a name="create-an-azure-key-vault"></a>Erstellen einer Azure Key Vault-Instanz

Sie können eine Azure Key Vault-Instanz mit dem folgenden PowerShell-Skript erstellen. Ersetzen Sie die Variablenwerte durch die Werte für Ihre Umgebung. Verwenden Sie die eingebettete Azure Cloud Shell-Instanz, indem Sie die Schaltfläche **Ausprobieren** in der oberen rechten Ecke des Codeblocks nutzen. Sie können den Code auch lokal kopieren und ausführen, wenn das [Azure PowerShell-Modul](/powershell/azure/install-az-ps) auf dem lokalen Computer installiert ist.

> [!NOTE]
> Führen Sie zum Abrufen Ihres API-Schlüssels die Schritte unter [Senden von E-Mails in Azure mit SendGrid](/azure/sendgrid-dotnet-how-to-send-email#to-find-your-sendgrid-api-key) aus.

```azurepowershell-interactive
$SubscriptionId  =  "<subscription ID>"

# Sign in to your Azure account and select your subscription
# If you omit the SubscriptionId parameter, the default subscription is selected.
Connect-AzAccount -SubscriptionId $SubscriptionId

# Use Get-AzLocation to see your available locations.
$region = "southcentralus"
$KeyVaultResourceGroupName  = "mykeyvaultgroup"
$VaultName = "<Enter a universally unique vault name>"
$SendGridAPIKey = "<SendGrid API key>"
$AutomationAccountName = "testaa"

# Create new Resource Group, or omit this step if you already have a resource group.
New-AzResourceGroup -Name $KeyVaultResourceGroupName -Location $region

# Create the new key vault
$newKeyVault = New-AzKeyVault -VaultName $VaultName -ResourceGroupName $KeyVaultResourceGroupName -Location $region
$resourceId = $newKeyVault.ResourceId

# Convert the SendGrid API key into a SecureString
$Secret = ConvertTo-SecureString -String $SendGridAPIKey -AsPlainText -Force
Set-AzKeyVaultSecret -VaultName $VaultName -Name 'SendGridAPIKey' -SecretValue $Secret

# Grant access to the Key Vault to the Automation Run As account.
$connection = Get-AzAutomationConnection -ResourceGroupName $KeyVaultResourceGroupName -AutomationAccountName $AutomationAccountName -Name AzureRunAsConnection
$appID = $connection.FieldDefinitionValues.ApplicationId
Set-AzKeyVaultAccessPolicy -VaultName $VaultName -ServicePrincipalName $appID -PermissionsToSecrets Set, Get
```

Weitere Möglichkeiten zum Erstellen einer Azure Key Vault-Instanz und zum Speichern eines Geheimnisses finden Sie unter [Schnellstarts zu Key Vault](/azure/key-vault/).

## <a name="import-required-modules-to-your-automation-account"></a>Importieren erforderlicher Module in Ihr Automation-Konto

Für die Verwendung von Azure Key Vault innerhalb eines Runbooks benötigt Ihr Automation-Konto die folgenden Module:

* [Az.Profile](https://www.powershellgallery.com/packages/Az.Profile)
* [Az.KeyVault](https://www.powershellgallery.com/packages/Az.KeyVault)

Klicken Sie auf der Registerkarte „Azure Automation“ unter **Installationsoptionen** auf **Deploy to Azure Automation** (In Azure Automation bereitstellen). Das Azure-Portal wird geöffnet. Wählen Sie auf der Seite „Importieren“ Ihr Automation-Konto aus, und klicken Sie auf **OK**.

Weitere Methoden zum Hinzufügen der erforderlichen Module finden Sie unter [Verwenden von Modulen in Azure Automation](/azure/automation/shared-resources/modules#importing-modules).

## <a name="create-the-runbook-to-send-an-email"></a>Erstellen des Runbooks zum Senden einer E-Mail

Wenn Sie einen Schlüsseltresor erstellt und Ihren `SendGrid`-API-Schlüssel gespeichert haben, können Sie das Runbook erstellen, das den API-Schlüssel abruft und eine E-Mail sendet.

Dieses Runbook verwendet `AzureRunAsConnection` als [ausführendes Konto](automation-create-runas-account.md) für die Authentifizierung bei Azure, um das Geheimnis aus Azure Key Vault abzurufen.

Verwenden Sie dieses Beispiel, um ein Runbook mit dem Namen **Send-GridMailMessage** zu erstellen. Sie können das PowerShell-Skript ändern und für verschiedene Szenarien wiederverwenden.

1. Wechseln Sie zu Ihrem Azure Automation-Konto.
2. Wählen Sie unter **Prozessautomatisierung** die Option **Runbooks** aus.
3. Wählen Sie oben in der Liste mit den Runbooks die Option **+ Runbook erstellen** aus.
4. Geben Sie auf der Seite **Runbook hinzufügen** den Namen **Send-GridMailMessage** für das Runbook ein. Wählen Sie als Runbooktyp **PowerShell** aus. Wählen Sie anschließend **Erstellen**.
   ![Erstellen eines Runbooks](./media/automation-send-email/automation-send-email-runbook.png)
5. Das Runbook wird erstellt, und die Seite **PowerShell-Runbook bearbeiten** wird geöffnet.
   ![Bearbeiten des Runbooks](./media/automation-send-email/automation-send-email-edit.png)
6. Kopieren Sie das folgende PowerShell-Beispiel auf die Seite **Bearbeiten**. Stellen Sie sicher, dass `$VaultName` der Name ist, den Sie beim Erstellen des Schlüsseltresors angegeben haben.

    ```powershell-interactive
    Param(
      [Parameter(Mandatory=$True)]
      [String] $destEmailAddress,
      [Parameter(Mandatory=$True)]
      [String] $fromEmailAddress,
      [Parameter(Mandatory=$True)]
      [String] $subject,
      [Parameter(Mandatory=$True)]
      [String] $content
    )

    $Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Connect-AzAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Out-Null
    $VaultName = "<Enter your vault name>"
    $SENDGRID_API_KEY = (Get-AzKeyVaultSecret -VaultName $VaultName -Name "SendGridAPIKey").SecretValueText
    $headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
    $headers.Add("Authorization", "Bearer " + $SENDGRID_API_KEY)
    $headers.Add("Content-Type", "application/json")

    $body = @{
    personalizations = @(
        @{
            to = @(
                    @{
                        email = $destEmailAddress
                    }
            )
        }
    )
    from = @{
        email = $fromEmailAddress
    }
    subject = $subject
    content = @(
        @{
            type = "text/plain"
            value = $content
        }
    )
    }

    $bodyJson = $body | ConvertTo-Json -Depth 4

    $response = Invoke-RestMethod -Uri https://api.sendgrid.com/v3/mail/send -Method Post -Headers $headers -Body $bodyJson
    ```

7. Wählen Sie die Option **Veröffentlichen**, um das Runbook zu speichern und zu veröffentlichen.

Führen Sie die Schritte unter [Verwalten von Runbooks in Azure Automation](manage-runbooks.md#test-a-runbook) oder [Starten eines Runbooks in Azure Automation](start-runbooks.md) aus, um zu überprüfen, ob das Runbook ausgeführt wird.
Wird die Test-E-Mail zuerst nicht angezeigt, überprüfen Sie den **Junk-** und **Spam**-Ordner.

## <a name="clean-up"></a>Bereinigen

Löschen Sie das Runbook, falls es nicht mehr benötigt wird. Wählen Sie das Runbook hierzu in der Runbookliste aus, und klicken Sie auf **Löschen**.

Löschen Sie den Schlüsseltresor mithilfe des Cmdlets [Remove-AzKeyVault](https://docs.microsoft.com/powershell/module/az.keyvault/remove-azkeyvault?view=azps-3.7.0).

```azurepowershell-interactive
$VaultName = "<your KeyVault name>"
$ResourceGroupName = "<your ResourceGroup name>"
Remove-AzKeyVault -VaultName $VaultName -ResourceGroupName $ResourceGroupName
```

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu Problemen beim Erstellen oder Starten des Runbooks finden Sie unter [Beheben von Fehlern bei Runbooks](./troubleshoot/runbooks.md).
* Informationen zum Aktualisieren von Modulen in Ihrem Automation-Konto finden Sie unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](automation-update-azure-modules.md).
* Informationen zum Überwachen der Runbookausführung finden Sie unter [Weiterleiten von Auftragsstatus und Auftragsdatenströmen von Automation an Azure Monitor-Protokolle](automation-manage-send-joblogs-log-analytics.md).
* Informationen zum Auslösen eines Runbooks mithilfe einer Warnung finden Sie unter [Verwenden einer Warnung zum Auslösen eines Azure Automation-Runbooks](automation-create-alert-triggered-runbook.md).
