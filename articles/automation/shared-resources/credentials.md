---
title: Verwalten von Anmeldeinformationen in Azure Automation
description: Anmeldeinformationsobjekte in Azure Automation enthalten Sicherheitsanmeldeinformationen, die zur Authentifizierung von Ressourcen verwendet werden können, auf die über das Runbook oder die DSC-Konfiguration zugegriffen wird. Dieser Artikel beschreibt, wie Sie Anmeldeinformationsobjekte erstellen und in einem Runbook oder einer DSC-Konfiguration verwenden.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: mgoedtel
ms.author: magoedte
ms.date: 01/31/2020
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 16b92108bcb4e5185a1990b0ed8f1278bfe44921
ms.sourcegitcommit: d662eda7c8eec2a5e131935d16c80f1cf298cb6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2020
ms.locfileid: "82652830"
---
# <a name="manage-credentials-in-azure-automation"></a>Verwalten von Anmeldeinformationen in Azure Automation

Ein Automation-Anmeldeinformationsobjekt enthält ein Objekt, das Sicherheitsanmeldeinformationen wie beispielsweise einen Benutzernamen und ein Kennwort umfasst. Runbooks und DSC-Konfigurationen verwenden Cmdlets, die ein [PSCredential-](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?view=pscore-6.2.0)-Objekt für die Authentifizierung akzeptieren. Alternativ können sie den Benutzernamen und das Kennwort des `PSCredential`-Objekts extrahieren, um diese einer Anwendung oder einem Dienst bereitzustellen, die eine Authentifizierung erfordern. 

>[!NOTE]
>Zu den sicheren Objekten in Azure Automation gehören Anmeldeinformationen, Zertifikate, Verbindungen und verschlüsselte Variablen. Diese Objekte werden mithilfe eines eindeutigen Schlüssels verschlüsselt und in Azure Automation gespeichert, der für jedes Automation-Konto generiert wird. Azure Automation speichert den Schlüssel im vom System verwalteten Schlüsseltresor. Vor dem Speichern einer sicheren Ressource lädt Azure Automation den Schlüssel aus dem Schlüsseltresor und verwendet ihn dann zum Verschlüsseln der Ressource. 

>[!NOTE]
>Dieser Artikel wurde aktualisiert und beinhaltet jetzt das neue Az-Modul von Azure PowerShell. Sie können das AzureRM-Modul weiterhin verwenden, das bis mindestens Dezember 2020 weiterhin Fehlerbehebungen erhält. Weitere Informationen zum neuen Az-Modul und zur Kompatibilität mit AzureRM finden Sie unter [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Einführung in das neue Az-Modul von Azure PowerShell). Installationsanweisungen für das Az-Modul auf Ihrem Hybrid Runbook Worker finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). In Ihrem Automation-Konto können Sie die Module mithilfe der Informationen unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](../automation-update-azure-modules.md) auf die neueste Version aktualisieren.

[!INCLUDE [gdpr-dsr-and-stp-note.md](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="powershell-cmdlets-used-to-access-credentials"></a>PowerShell-Cmdlets, die für den Zugriff auf Anmeldeinformationen verwendet werden

Über die Cmdlets in der folgenden Tabelle können Sie Anmeldeinformation für Automation mit PowerShell erstellen und verwalten. Diese sind im Lieferumfang der [Az-Module](modules.md#az-modules) enthalten.

| Cmdlet | BESCHREIBUNG |
|:--- |:--- |
| [Get-AzAutomationCredential](/powershell/module/az.automation/get-azautomationcredential?view=azps-3.3.0) |Ruft ein [CredentialInfo](https://docs.microsoft.com/dotnet/api/microsoft.azure.commands.automation.model.credentialinfo?view=azurerm-ps)-Objekt ab, das Metadaten über die Anmeldeinformationen enthält. Das Cmdlet ruft nicht das `PSCredential`-Objekt selbst ab.  |
| [New-AzAutomationCredential](/powershell/module/az.automation/new-azautomationcredential?view=azps-3.3.0) |Erstellt ein neues Anmeldeinformationsobjekt. |
| [Remove-AzAutomationCredential](/powershell/module/az.automation/remove-azautomationcredential?view=azps-3.3.0) |Entfernt ein Anmeldeinformationsobjekt. |
| [Set-AzAutomationCredential](/powershell/module/az.automation/set-azautomationcredential?view=azps-3.3.0) |Legt die Eigenschaften für ein vorhandenes Anmeldeinformationsobjekt fest. |

## <a name="other-cmdlets-used-to-access-credentials"></a>Andere Cmdlets, die für den Zugriff auf Anmeldeinformationen verwendet werden

Die Cmdlets in der folgenden Tabelle werden für den Zugriff auf Anmeldeinformationen in Runbooks und DSC-Konfigurationen verwendet. 

| Cmdlet | BESCHREIBUNG |
|:--- |:--- |
| `Get-AutomationPSCredential` |Ruft ein `PSCredential`-Objekt zur Verwendung in einem Runbook oder einer DSC-Konfiguration ab. In den meisten Fällen sollten Sie dieses [interne Cmdlet](modules.md#internal-cmdlets) anstelle des `Get-AzAutomationCredential`-Cmdlets verwenden, da dieses nur Anmeldeinformationen abruft. Es ist in der Regel nicht hilfreich, diese Informationen an ein anderes Cmdlet zu übergeben. |
| [Get-Credential](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-credential?view=powershell-7) |Ruft Anmeldeinformationen mit einer Abfrage von Benutzername und Kennwort ab. Dieses Cmdlet ist Teil des Standardmoduls von Microsoft.PowerShell.Security. Weitere Informationen finden Sie unter [Standardmodule](modules.md#default-modules).|
| [New-AzureAutomationCredential](https://docs.microsoft.com/powershell/module/servicemanagement/azure/new-azureautomationcredential?view=azuresmps-4.0.0) | Erstellt ein Anmeldeinformationsobjekt. Dieses Cmdlet ist Teil des Azure-Standardmoduls. Weitere Informationen finden Sie unter [Standardmodule](modules.md#default-modules).|

Zum Abrufen von `PSCredential`-Objekten in Ihrem Code müssen Sie das `Orchestrator.AssetManagement.Cmdlets`-Modul importieren. Weitere Informationen finden Sie unter [Verwalten von Modulen in Azure Automation](modules.md).

```azurepowershell
Import-Module Orchestrator.AssetManagement.Cmdlets -ErrorAction SilentlyContinue
```

> [!NOTE]
> Vermeiden Sie die Verwendung von Variablen im Parameter `Name` von `Get-AutomationPSCredential`. Durch deren Verwendung kann die Ermittlung von Abhängigkeiten zwischen Runbooks oder DSC-Konfigurationen und Anmeldeinformationsobjekten zur Entwurfszeit erschwert werden.

## <a name="python-2-functions-that-access-credentials"></a>Python 2-Funktionen, die auf Anmeldeinformationen zugreifen

Mit der Funktion in der folgenden Tabelle wird auf Anmeldeinformationen in einem Python 2-Runbook zugegriffen.

| Funktion | BESCHREIBUNG |
|:---|:---|
| `automationassets.get_automation_credential` | Ruft Informationen zu einem Anmeldeinformationsobjekt ab. |

> [!NOTE]
> Importieren Sie das Modul `automationassets` oben im Python-Runbook, um auf die Ressourcenfunktionen zugreifen zu können.

## <a name="create-a-new-credential-asset"></a>Erstellen von neuen Anmeldeinformationsobjekten

Sie können ein neues Anmeldeinformationsobjekt im Azure-Portal oder mit Windows PowerShell erstellen.

### <a name="create-a-new-credential-asset-with-the-azure-portal"></a>Erstellen eines neuen Anmeldeinformationsobjekts über das Azure-Portal

1. Wählen Sie in Ihrem Automation-Konto unter **Freigegebene Ressourcen** die Option **Anmeldeinformationen** aus.
1. Wählen Sie **Anmeldeinformationen hinzufügen** aus.
2. Geben Sie im Bereich „Neue Anmeldeinformationen“ einen geeigneten Anmeldeinformationsnamen ein, der Ihre Benennungsstandards einhält. 
3. Geben Sie Ihre Zugriffs-ID in das Feld **Benutzername** ein. 
4. Geben Sie für beide Kennwortfelder Ihren geheimen Zugriffsschlüssel ein.

    ![Erstellen neuer Anmeldeinformationen](../media/credentials/credential-create.png)

5. Wenn das Kontrollkästchen für mehrstufige Authentifizierung aktiviert ist, deaktivieren Sie es. 
6. Klicken Sie auf **Erstellen**, um das neue Anmeldeinformationsobjekt zu speichern.

> [!NOTE]
> Azure Automation unterstützt keine Benutzerkonten, die mehrstufige Authentifizierung verwenden.

### <a name="create-a-new-credential-asset-with-windows-powershell"></a>Erstellen eines neuen Anmeldeinformationsobjekts mithilfe von Windows PowerShell

Das folgenden Beispiel zeigt, wie Sie ein neues Automation-Anmeldeinformationsobjekt erstellen. Es wird zunächst ein `PSCredential`-Objekt mit Name und Kennwort erstellt, anschließend werden diese Informationen zum Erstellen des Anmeldeinformationsobjekts verwendet. Stattdessen können Sie das Cmdlet `Get-Credential` verwenden, um den Benutzer zur Eingabe eines Namens und Kennworts aufzufordern.

```powershell
$user = "MyDomain\MyUser"
$pw = ConvertTo-SecureString "PassWord!" -AsPlainText -Force
$cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $user, $pw
New-AzureAutomationCredential -AutomationAccountName "MyAutomationAccount" -Name "MyCredential" -Value $cred
```

## <a name="get-a-credential-asset"></a>Abrufen von Anmeldeinformationsobjekten

Ein Runbook oder eine DSC-Konfiguration ruft Anmeldeinformationsobjekte mit dem internen `Get-AutomationPSCredential`-Cmdlet ab. Dieses Cmdlet ruft ein `PSCredential`-Objekt ab, das Sie mit einem Cmdlet verwenden können, für das Anmeldeinformationen benötigt werden. Sie können auch die Eigenschaften des Anmeldeinformationsobjekts abrufen, um diese einzeln zu verwenden. Das Objekt verfügt über Eigenschaften für den Benutzernamen und das sichere Kennwort. 

> [!NOTE]
> Das `Get-AzAutomationCredential`-Cmdlet ruft kein `PSCredential`-Objekt ab, das für die Authentifizierung verwendet werden kann. Es bietet lediglich Informationen zu den Anmeldeinformationen. Wenn Sie ein Anmeldeinformationsobjekt in einem Runbook verwenden müssen, müssen Sie dieses mithilfe von `Get-AutomationPSCredential` als `PSCredential`-Objekt abrufen.

Alternativ können Sie die Methode [GetNetworkCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential.getnetworkcredential?view=pscore-6.2.0) verwenden, um ein [NetworkCredential](/dotnet/api/system.net.networkcredential)-Objekt abzurufen, das eine unsichere Version des Kennworts darstellt.

### <a name="textual-runbook-example"></a>Beispiel für ein Textrunbook

Das folgende Beispiel zeigt, wie Sie ein PowerShell-Anmeldeinformationsobjekt in einem Runbook verwenden. Es ruft das Anmeldeinformationsobjekt ab und weist seinen Benutzernamen und das Kennwort Variablen zu.


```azurepowershell
$myCredential = Get-AutomationPSCredential -Name 'MyCredential'
$userName = $myCredential.UserName
$securePassword = $myCredential.Password
$password = $myCredential.GetNetworkCredential().Password
```

Sie können Anmeldeinformationen auch zum Authentifizieren mit [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount?view=azps-3.3.0) in Azure verwenden. In den meisten Fällen sollten Sie ein [ausführendes Konto](../manage-runas-account.md) verwenden und die Verbindung mit [Get-AzAutomationConnection](../automation-connections.md) abrufen.


```azurepowershell
$myCred = Get-AutomationPSCredential -Name 'MyCredential'
$userName = $myCred.UserName
$securePassword = $myCred.Password
$password = $myCred.GetNetworkCredential().Password

$myPsCred = New-Object System.Management.Automation.PSCredential ($userName,$password)

Connect-AzAccount -Credential $myPsCred
```

### <a name="graphical-runbook-example"></a>Beispiel für ein grafisches Runbook

Sie können einem grafischen Runbook eine Aktivität für das interne `Get-AutomationPSCredential`-Cmdlet hinzufügen, indem Sie im Bibliotheksbereich des grafischen Editors erst mit der rechten Maustaste auf die Anmeldeinformationen und anschließend mit der linken auf **Zur Canvas hinzufügen** klicken.

![Anmeldeinformationen zum Zeichenbereich hinzufügen](../media/credentials/credential-add-canvas.png)

Die folgende Abbildung zeigt ein Beispiel für die Verwendung eines Anmeldeinformationsobjekts in einem grafischen Runbook. In diesem Fall stellt das Anmeldeinformationsobjekt Authentifizierungsinformationen für ein Runbook in Azure-Ressourcen bereit, wie unter [Verwenden von Azure AD in Azure Automation für die Authentifizierung bei Azure](../automation-use-azure-ad.md) beschrieben. Die erste Aktivität ruft die Anmeldeinformationen ab, die Zugriff auf das Azure-Abonnement haben. Die Kontoverbindungsaktivität verwendet diese Anmeldeinformationen anschließend, um eine Authentifizierung für alle nachfolgenden Aktivitäten bereitzustellen. Es wird eine [Pipelineverknüpfung](../automation-graphical-authoring-intro.md#links-and-workflow) verwendet, da `Get-AutomationPSCredential` ein einzelnes Objekt erwartet.  

![Anmeldeinformationen zum Zeichenbereich hinzufügen](../media/credentials/get-credential.png)

## <a name="use-credentials-in-a-dsc-configuration"></a>Verwenden von Anmeldeinformationen in einer DSC-Konfiguration

DSC-Konfigurationen in Azure Automation können mithilfe von `Get-AutomationPSCredential` mit Anmeldeinformationsobjekten arbeiten, können aber gleichzeitig auch Anmeldeinformationsobjekte über Parameter übergeben. Weitere Informationen finden Sie unter [Kompilieren von Konfigurationen in Azure Automation DSC](../automation-dsc-compile.md#credential-assets).

## <a name="use-credentials-in-a-python-2-runbook"></a>Verwenden von Anmeldeinformationen in einem Python 2-Runbook

Im folgenden Beispiel wird der Zugriff auf Anmeldeinformationen in Python 2-Runbooks veranschaulicht.


```python
import automationassets
from automationassets import AutomationAssetNotFound

# get a credential
cred = automationassets.get_automation_credential("credtest")
print cred["username"]
print cred["password"]
```

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu den Cmdlets, die für den Zugriff auf Anmeldeinformationen verwendet werden, finden Sie unter [Verwalten von Modulen in Azure Automation](modules.md).
* Allgemeine Informationen zu Runbooks finden Sie unter [Ausführen von Runbooks in Azure Automation](../automation-runbook-execution.md).
* Ausführliche Informationen zu DSC-Konfigurationen finden Sie in der [Übersicht über State Configuration](../automation-dsc-overview.md).