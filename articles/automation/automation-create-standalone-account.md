---
title: Erstellen eines eigenständigen Azure Automation-Kontos
description: Dieser Artikel führt Sie anhand eines Beispiels durch die Schritte zum Erstellen, Testen und Verwenden der Authentifizierung per Sicherheitsprinzipal in Azure Automation.
services: automation
ms.subservice: process-automation
ms.date: 01/15/2019
ms.topic: conceptual
ms.openlocfilehash: 72a40363edf0e83eea26ee697ce992226da0db4f
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/15/2020
ms.locfileid: "81392290"
---
# <a name="create-a-standalone-azure-automation-account"></a>Erstellen eines eigenständigen Azure Automation-Kontos

In diesem Artikel erfahren Sie, wie Sie ein Azure Automation-Konto im Azure-Portal erstellen. Mit dem Automation-Konto im Portal können Sie Automation auswerten und sich damit vertraut machen, ohne zusätzliche Verwaltungslösungen oder eine Azure Monitor-Protokolle-Integration zu verwenden. Wenn Sie Runbookaufträge später genauer überwachen möchten, können Sie jederzeit entsprechende Verwaltungslösungen hinzufügen oder eine Azure Monitor-Protokolle-Integration durchführen.

Mit einem Automation-Konto können Sie Runbooks authentifizieren, indem Sie Ressourcen im Azure Resource Manager-Bereitstellungsmodell oder im klassischen Bereitstellungsmodell verwalten. Ein Automation-Konto kann Ressourcen in allen Regionen und Abonnements für einen bestimmten Mandanten verwalten.

Wenn Sie im Azure-Portal ein Automation-Konto erstellen, wird automatisch ein **ausführendes Konto** erstellt. Das Konto führt die folgenden Aufgaben aus:

* Erstellen eines Dienstprinzipals in Azure Active Directory (Azure AD)
* Erstellen eines Zertifikats
* Zuweisen der Rolle „Mitwirkender“ der rollenbasierten Zugriffssteuerung (RBAC), die zum Verwalten von Azure Resource Manager-Ressourcen mit Runbooks verwendet wird

Dank der automatischen Erstellung dieses Kontos können Sie schnell Runbooks für Ihre Automatisierungsanforderungen erstellen und bereitstellen.

## <a name="permissions-required-to-create-an-automation-account"></a>Erforderliche Berechtigungen zum Erstellen eines Automation-Kontos

Zum Erstellen oder Aktualisieren eines Automation-Kontos und zum Abschließen der Aufgaben in diesem Artikel müssen Sie über die folgenden Berechtigungen verfügen:

* Für die Erstellung eines Automation-Kontos muss Ihr Azure AD-Benutzerkonto einer Rolle mit Berechtigungen hinzugefügt werden, die der Rolle „Besitzer“ für `Microsoft.Automation`-Ressourcen entsprechen. Weitere Informationen finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure Automation](automation-role-based-access-control.md).
* Wenn im Azure-Portal unter **Azure Active Directory** > **VERWALTEN** > **Benutzereinstellungen** die Option **App-Registrierungen** auf **Ja** festgelegt ist, können Benutzer ohne Administratorrechte in Ihrem Azure AD-Mandanten [Active Directory-Anwendungen registrieren](../active-directory/develop/howto-create-service-principal-portal.md#check-azure-subscription-permissions). Wenn **App-Registrierungen** auf **Nein** festgelegt ist, muss ein Benutzer, der diese Aktion ausführt, ein globaler Administrator in Azure AD sein.

Wenn Sie kein Mitglied der Active Directory-Instanz des Abonnements sind, werden Sie, bevor Sie der Rolle „Globaler Administrator“/„Co-Administrator“ des Abonnements hinzugefügt werden, zunächst in Active Directory als Gast hinzugefügt. In diesem Szenario wird im Bereich „Automation-Konto hinzufügen“ die folgende Meldung angezeigt: `You do not have permissions to create.`

Wird ein Benutzer zuerst der Rolle „Globaler Administrator“/„Co-Administrator“ hinzugefügt, können Sie diesen aus der Active Directory-Instanz des Abonnements entfernen. Sie können den Benutzer der Rolle „Benutzer“ in Active Directory erneut hinzufügen.

So überprüfen Sie Benutzerrollen

1. Navigieren Sie im Azure-Portal zum Bereich „Azure Active Directory“.
1. Wählen Sie **Benutzer und Gruppen**.
1. Wählen Sie **Alle Benutzer**.
1. Wählen Sie nach der Auswahl eines bestimmten Benutzers **Profil** aus. Als Wert des Attributs **Benutzertyp** im Benutzerprofil darf nicht **Gast** angegeben sein.

## <a name="create-a-new-automation-account-in-the-azure-portal"></a>Erstellen eines neuen Automation-Kontos über das Azure-Portal

Führen Sie die folgenden Schritte aus, um ein Azure Automation-Konto über das Azure-Portal zu erstellen:

1. Melden Sie sich mit einem Konto, das Mitglied der Rolle „Abonnement-Administratoren“ und Co-Administrator des Abonnements ist, beim Azure-Portal an.
1. Klicken Sie auf **+ Ressource erstellen**.
1. Suchen Sie nach **Automation**. Klicken Sie in den Suchergebnissen auf **Automation**.

   ![„Automation & Control“ im Azure Marketplace suchen und auswählen](media/automation-create-standalone-account/automation-marketplace-select-create-automationacct.png)

1. Wählen Sie auf dem nächsten Bildschirm die Option **Neu erstellen** aus.

   ![Automation-Konto hinzufügen](media/automation-create-standalone-account/automation-create-automationacct-properties.png)

   > [!NOTE]
   > Sollte im Bereich „Automation-Konto hinzufügen“ die folgende Meldung angezeigt werden, ist Ihr Konto kein Mitglied der Rolle „Administratoren“ des Abonnements und kein Co-Administrator des Abonnements.
   >
   > ![Warnung beim Hinzufügen des Automation-Kontos](media/automation-create-standalone-account/create-account-without-perms.png)

1. Geben Sie im Bereich „Automation-Konto hinzufügen“ im Feld **Name** einen Namen für das neue Automation-Konto ein. Sie können diesen Namen nach der Auswahl nicht mehr ändern. 

    > [!NOTE]
    > Automation-Kontonamen sind für jede Region und Ressourcengruppe eindeutig. Namen gelöschter Automation-Konten sind möglicherweise nicht sofort wieder verfügbar.

1. Falls Sie über mehrere Abonnements verfügen, geben Sie im Feld **Abonnement** das Abonnement an, das Sie für das neue Konto verwenden möchten.
1. Geben Sie unter **Ressourcengruppe** eine neue oder vorhandene Ressourcengruppe ein, oder wählen Sie eine Ressourcengruppe aus.
1. Wählen Sie unter **Standort** den Standort eines Azure-Datencenters aus.
1. Vergewissern Sie sich, dass die Option **Ausführendes Azure-Konto erstellen** auf **Ja** festgelegt ist, und klicken Sie anschließend auf **Erstellen**.

   > [!NOTE]
   > Wenn Sie die Option **Ausführendes Azure-Konto erstellen** auf **Nein** festlegen, wird im Bereich „Automation-Konto hinzufügen“ eine Meldung angezeigt. Das Konto wird zwar im Azure-Portal erstellt, verfügt aber im Verzeichnisdienst für Ihr Abonnement (klassisches Bereitstellungsmodell oder Azure Resource Manager-Bereitstellungsmodell) über keine entsprechende Authentifizierungsidentität. Dadurch hat das Automation-Konto auch keinen Zugriff auf Ressourcen in Ihrem Abonnement. Runbooks, die auf dieses Konto verweisen, können in diesem Fall nicht authentifiziert werden und keine Aufgaben für Ressourcen in diesen Bereitstellungsmodellen durchführen.
   >
   > ![Warnung beim Hinzufügen des Automation-Kontos](media/automation-create-standalone-account/create-account-decline-create-runas-msg.png)
   >
   > Wenn der Dienstprinzipal nicht erstellt wird, wird die Rolle „Mitwirkender“ nicht zugewiesen.
   >

1. Den Status der Automation-Kontoerstellung können Sie über das Menü unter **Benachrichtigungen** verfolgen.

### <a name="resources-included"></a>Enthaltene Ressourcen

Nach der erfolgreichen Erstellung des Automation-Kontos werden automatisch verschiedene Ressourcen erstellt. Nachdem diese Ressourcen erstellt sind, können Runbooks gefahrlos gelöscht, wenn Sie diese nicht behalten möchten. Die ausführenden Konten können dazu verwendet werden, sich bei Ihrem Konto in einem Runbook zu authentifizieren, und sollten erhalten bleiben, es sei denn, Sie erstellen ein anderes oder benötigen sie nicht mehr. In der folgenden Tabelle sind die Ressourcen für das ausführende Konto zusammengefasst.

| Resource | BESCHREIBUNG |
| --- | --- |
| AzureAutomationTutorial-Runbook |Ein grafisches Beispielrunbook, das die Authentifizierung mithilfe des ausführenden Kontos veranschaulicht. Das Runbook ruft alle Resource Manager-Ressourcen ab. |
| AzureAutomationTutorialScript-Runbook |Ein PowerShell-Beispielrunbook, das die Authentifizierung mithilfe des ausführenden Kontos veranschaulicht. Das Runbook ruft alle Resource Manager-Ressourcen ab. |
| AzureAutomationTutorialPython2-Runbook |Ein Python-Beispielrunbook, das die Authentifizierung mithilfe des ausführenden Kontos veranschaulicht. Das Runbook führt alle im Abonnement vorhandenen Ressourcengruppen auf. |
| AzureRunAsCertificate |Eine Zertifikatressource, die automatisch zusammen mit dem Automation-Konto oder mithilfe eines PowerShell-Skripts für ein vorhandenes Konto erstellt wird. Das Zertifikat dient zur Authentifizierung bei Azure, um Azure Resource Manager-Ressourcen über Runbooks verwalten zu können. Dieses Zertifikat ist ein Jahr lang gültig. |
| AzureRunAsConnection |Eine Verbindungsressource, die automatisch zusammen mit dem Automation-Konto oder mithilfe eines PowerShell-Skripts für ein vorhandenes Konto erstellt wird. |

## <a name="create-a-classic-run-as-account"></a>Erstellen eines klassischen ausführenden Kontos

Klassische ausführende Konten werden nicht mehr standardmäßig erstellt, wenn Sie ein Azure Automation-Konto erstellen. Falls Sie dennoch ein klassisches ausführendes Konto benötigen:

1. Wählen Sie in Ihrem Automation-Konto unter **Kontoeinstellungen** die Option **Ausführende Konten** aus.
2. Wählen Sie **Klassisches ausführendes Azure-Konto** aus.
3. Klicken Sie auf **Erstellen**, um mit dem Erstellen des klassischen ausführenden Kontos fortzufahren.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur grafischen Inhaltserstellung finden Sie unter [Grafische Erstellung in Azure Automation](automation-graphical-authoring-intro.md).
* Erste Schritte mit PowerShell-Runbooks werden unter [Mein erstes PowerShell-Runbook](automation-first-runbook-textual-powershell.md) beschrieben.
* Die ersten Schritte mit PowerShell-Workflow-Runbooks sind unter [Mein erstes PowerShell-Workflow-Runbook](automation-first-runbook-textual.md)beschrieben.
* Eine Einführung in Python 2-Runbooks finden Sie unter [Mein erstes Python 2-Runbook](automation-first-runbook-textual-python2.md).
* Eine Referenz zu den PowerShell-Cmdlets finden Sie unter [Az.Automation](https://docs.microsoft.com/powershell/module/az.automation/?view=azps-3.7.0#automation
).
