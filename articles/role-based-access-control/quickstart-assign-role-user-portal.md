---
title: 'Tutorial: Gewähren des Zugriffs auf Azure-Ressourcen für einen Benutzer mit dem Azure-Portal – Azure RBAC'
description: In diesem Tutorial erfahren Sie, wie Sie einem Benutzer mit dem Azure-Portal und der rollenbasierten Zugriffssteuerung in Azure (Azure Role-Based Access Control, Azure RBAC) Zugriff auf Azure-Ressourcen gewähren.
services: role-based-access-control
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: tutorial
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 02/22/2019
ms.author: rolyon
ms.openlocfilehash: e70a3a1eedbc56754ad46fd99dddc0c7a3a6cf2f
ms.sourcegitcommit: 4499035f03e7a8fb40f5cff616eb01753b986278
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2020
ms.locfileid: "82735843"
---
# <a name="tutorial-grant-a-user-access-to-azure-resources-using-the-azure-portal"></a>Tutorial: Gewähren des Zugriffs auf Azure-Ressourcen für einen Benutzer mit dem Azure-Portal

Der Zugriff auf Azure-Ressourcen wird mithilfe der [rollenbasierten Zugriffssteuerung in Azure (Azure RBAC)](overview.md) verwaltet. In diesem Tutorial gewähren Sie einem Benutzer Zugriff, um virtuelle Computer in einer Ressourcengruppe zu erstellen und zu verwalten.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Gewähren des Zugriffs für einen Benutzer im Ressourcengruppenbereich
> * Zugriff entfernen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

1. Klicken Sie in der Navigationsliste auf **Ressourcengruppen**.

1. Klicken Sie auf **Hinzufügen**, um das Blatt **Ressourcengruppe** zu öffnen.

   ![Hinzufügen einer neuen Ressourcengruppe](./media/quickstart-assign-role-user-portal/resource-group.png)

1. Geben Sie für **Ressourcengruppenname** die Zeichenfolge **rbac-resource-group** ein.

1. Wählen Sie ein Abonnement und einen Standort aus.

1. Klicken Sie auf **Erstellen** , um die Ressourcengruppe zu erstellen.

1. Klicken Sie auf **Aktualisieren**, um die Liste der Ressourcengruppen zu aktualisieren.

   Die neue Ressourcengruppe wird in der Liste Ihrer Ressourcengruppen angezeigt.

   ![Ressourcengruppenliste](./media/quickstart-assign-role-user-portal/resource-group-list.png)

## <a name="grant-access"></a>Gewähren von Zugriff

In Azure RBAC erstellen Sie zum Gewähren des Zugriffs eine Rollenzuweisung.

1. Klicken Sie in der Liste der **Ressourcengruppen** auf die neue Ressourcengruppe **rbac-resource-group**.

1. Klicken Sie auf **Zugriffssteuerung (IAM)** .

1. Klicken Sie auf die Registerkarte **Rollenzuweisungen**, um die aktuelle Liste mit den Rollenzuweisungen anzuzeigen.

   ![Blatt „Zugriffssteuerung (IAM)“ für Ressourcengruppe](./media/quickstart-assign-role-user-portal/access-control.png)

1. Klicken Sie auf **Hinzufügen** > **Rollenzuweisung hinzufügen**, um den Bereich „Rollenzuweisung hinzufügen“ zu öffnen.

   Wenn Sie keine Berechtigungen zum Zuweisen von Rollen haben, ist die Option „Rollenzuweisung hinzufügen“ deaktiviert.

   ![Menü „Hinzufügen“](./media/role-assignments-portal/add-menu.png)

   ![Bereich „Rollenzuweisung hinzufügen“](./media/quickstart-assign-role-user-portal/add-role-assignment.png)

1. Wählen Sie in der Dropdownliste **Rolle** die Rolle **Mitwirkender für virtuelle Computer** aus.

1. Wählen Sie in der Liste **Auswählen** sich selbst oder einen anderen Benutzer aus.

1. Klicken Sie auf **Speichern**, um die Rollenzuweisung zu erstellen.

   Nach einigen Augenblicken wird dem Benutzer die Rolle „Mitwirkender für virtuelle Computer“ im Bereich der Ressourcengruppe „rbac-resource-group“ zugewiesen.

   ![Rollenzuweisung: Mitwirkender von virtuellen Computern](./media/quickstart-assign-role-user-portal/vm-contributor-assignment.png)

## <a name="remove-access"></a>Zugriff entfernen

In Azure RBAC entfernen Sie eine Rollenzuweisung, um den Zugriff zu entfernen.

1. Fügen Sie in der Liste der Rollenzuweisungen ein Häkchen neben dem Benutzer mit der Rolle „Mitwirkender für virtuelle Computer“ hinzu.

1. Klicken Sie auf **Entfernen**.

   ![Nachricht zum Entfernen der Rollenzuweisung](./media/quickstart-assign-role-user-portal/remove-role-assignment.png)

1. Klicken Sie in der angezeigten Meldung zum Entfernen der Rollenzuweisung auf **Ja**.

## <a name="clean-up"></a>Bereinigung

1. Klicken Sie in der Navigationsliste auf **Ressourcengruppen**.

1. Klicken Sie auf **rbac-resource-group**, um die Ressourcengruppe zu öffnen.

1. Klicken Sie auf **Ressourcengruppe löschen**, um die Ressourcengruppe zu löschen.

   ![Ressourcengruppe löschen](./media/quickstart-assign-role-user-portal/delete-resource-group.png)

1. Geben Sie auf dem Blatt **Löschvorgang bestätigen**  den Namen der Ressourcengruppe ein: **rbac-resource-group**.

1. Klicken Sie auf **Löschen**, um die Ressourcengruppe zu löschen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: Gewähren des Zugriffs auf Azure-Ressourcen für einen Benutzer mit RBAC und Azure PowerShell](tutorial-role-assignments-user-powershell.md)
