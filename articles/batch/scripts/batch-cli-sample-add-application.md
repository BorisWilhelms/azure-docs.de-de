---
title: 'Azure CLI-Skriptbeispiel: Hinzufügen einer Anwendung in Batch'
description: Dieses Beispielskript veranschaulicht das Hinzufügen einer Anwendung für die Verwendung mit einem Azure Batch-Pool oder einer Aufgabe.
services: batch
documentationcenter: ''
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: ''
ms.service: batch
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/29/2018
ms.author: labrenne
ms.openlocfilehash: b19f5dbe27ba0ecabdca6557e4e8e1996fbef78a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "77017121"
---
# <a name="cli-example-add-an-application-to-an-azure-batch-account"></a>CLI-Beispiel: Hinzufügen einer Anwendung zu einem Azure Batch-Konto

Dieses Skript veranschaulicht das Hinzufügen einer Anwendung für die Verwendung mit einem Azure Batch-Pool oder einer Azure Batch-Aufgabe. Um eine Anwendung einzurichten, die Sie Ihrem Batch-Konto hinzufügen können, fassen Sie die ausführbare Datei mit sämtlichen Abhängigkeiten in einer ZIP-Datei zusammen. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für diesen Artikel mindestens die Azure CLI-Version 2.0.20 verwenden. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/batch/add-application/add-application.sh "Add Application")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe und alle dazugehörigen Ressourcen zu entfernen:

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle.
Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az storage account create](/cli/azure/storage/account#az-storage-account-create) | Erstellt ein Speicherkonto. |
| [az batch account create](/cli/azure/batch/account#az-batch-account-create) | Erstellt das Batch-Konto. |
| [az batch account login](/cli/azure/batch/account#az-batch-account-login) | Authentifiziert das angegebene Batch-Konto zur weiteren CLI-Interaktion.  |
| [az batch application create](/cli/azure/batch/application#az-batch-application-create) | Erstellt eine Anwendung  |
| [az batch application package create](/cli/azure/batch/application/package#az-batch-application-package-create) | Fügt der angegebenen Anwendung ein Anwendungspaket hinzu  |
| [az batch application set](/cli/azure/batch/application#az-batch-application-set) | Aktualisiert die Eigenschaften einer Anwendung  |
| [az group delete](/cli/azure/group#az-group-delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).
