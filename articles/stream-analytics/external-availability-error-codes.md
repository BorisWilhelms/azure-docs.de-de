---
title: Fehlercodes für externe Verfügbarkeit – Azure Stream Analytics
description: Beheben Sie Probleme in Azure Stream Analytics mit Fehlercodes für externe Verfügbarkeit.
ms.author: mamccrea
author: mamccrea
ms.topic: conceptual
ms.date: 05/07/2020
ms.service: stream-analytics
ms.openlocfilehash: 092b548e73330fe80ef10b2e25d9ab7883b256ac
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83595199"
---
# <a name="azure-stream-analytics-external-availability-error-codes"></a>Fehlercodes für externe Verfügbarkeit in Azure Stream Analytics

Unerwartetes Verhalten Ihres Azure Stream Analytics-Auftrags können Sie mithilfe von Aktivitätsprotokollen und Ressourcenprotokollen debuggen. In diesem Artikel ist die Beschreibung für jeden Fehlercode für externe Verfügbarkeit aufgeführt. Fehler der externen Verfügbarkeit treten auf, wenn ein abhängiger Dienst nicht verfügbar ist.

## <a name="externalserviceunavailable"></a>ExternalServiceUnavailable

* **Ursache:** Ein Dienst ist vorübergehend nicht verfügbar.
* **Empfehlung**: Stream Analytics versucht weiterhin, den Dienst zu erreichen. 

## <a name="kafkaservernotavailable"></a>KafkaServerNotAvailable

* **Ursache:** Der Kafka-Server ist nicht verfügbar.

## <a name="eventhubmessagingerror"></a>EventHubMessagingError

* **Ursache:** Stream Analytics hat bei der Kommunikation mit EventHub einen Fehler festgestellt. 


## <a name="next-steps"></a>Nächste Schritte

* [Problembehandlung für Eingangsverbindungen](stream-analytics-troubleshoot-input.md)
* [Problembehandlung von Azure Stream Analytics-Ausgaben](stream-analytics-troubleshoot-output.md)
* [Problembehandlung von Azure Stream Analytics-Abfragen](stream-analytics-troubleshoot-query.md)
* [Problembehandlung von Azure Stream Analytics mit Ressourcenprotokollen](stream-analytics-job-diagnostic-logs.md)
* [Datenfehler in Azure Stream Analytics](data-errors.md)
