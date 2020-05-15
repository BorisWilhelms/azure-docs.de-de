---
title: include file
description: include file
services: azure-monitor
author: rboucher
tags: azure-service-management
ms.topic: include
ms.date: 02/07/2019
ms.author: robb
ms.custom: include file
ms.openlocfilehash: 864b37c9e59786546ad2c29faf8457cfc3a21f6b
ms.sourcegitcommit: be32c9a3f6ff48d909aabdae9a53bd8e0582f955
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/26/2020
ms.locfileid: "82161165"
---
**Umfang und Aufbewahrung der Datensammlung** 

| Tarif | Grenzwert pro Tag | Beibehaltung von Daten | Comment |
|:---|:---|:---|:---|
| Current Per GB-Tarif<br>(Einführung: April 2018) | Keine Begrenzung | 30–730 Tage | Für eine Datenaufbewahrung von mehr als 31 Tagen fallen zusätzliche Gebühren an. Erfahren Sie mehr über die Preise für Azure Monitor. |
| Legacy Free-Tarife<br>(Einführung: April 2016) | 500 MB | 7 Tage | Wenn Ihr Arbeitsbereich das Tageslimit von 500 MB erreicht, wird die Datenerfassung beendet und am nächsten Morgen fortgesetzt. Ein Tag wird in UTC-Zeit ausgegeben. Beachten Sie, dass Daten, die von Azure Security Center erfasst werden, nicht im Tageslimit von 500 MB enthalten sind und auch noch darüber hinaus weiter erfasst werden.  |
| Legacy Standalone Per GB-Tarif<br>(Einführung: April 2016) | Keine Begrenzung | 30 bis 730 Tage | Für eine Datenaufbewahrung von mehr als 31 Tagen fallen zusätzliche Gebühren an. Erfahren Sie mehr über die Preise für Azure Monitor. |
| Legacy Per Node (OMS)<br>(Einführung: April 2016) | Keine Begrenzung | 30 bis 730 Tage | Für eine Datenaufbewahrung von mehr als 31 Tagen fallen zusätzliche Gebühren an. Erfahren Sie mehr über die Preise für Azure Monitor. |
| Legacy Standard-Tarif | Keine Begrenzung | 30 Tage  | Die Aufbewahrungsdauer kann nicht angepasst werden |
| Legacy Premium-Tarif | Keine Begrenzung | 365 Tage  | Die Aufbewahrungsdauer kann nicht angepasst werden |

**Anzahl von Arbeitsbereichen pro Abonnement**

| Tarif    | Arbeitsbereichsbegrenzung | Kommentare
|:---|:---|:---|
| Free-Tarif  | 10 | Dieser Grenzwert kann nicht erhöht werden. |
| Alle anderen Tarife | Keine Begrenzung | Sie sind durch die Anzahl der Ressourcen innerhalb einer Ressourcengruppe und die Anzahl der Ressourcengruppen pro Abonnement eingeschränkt. |

**Azure portal**

| Category | Begrenzung | Kommentare |
|:---|:---|:---|
| Maximale Anzahl von Datensätzen, die von einer Protokollabfrage zurückgegebenen werden | 10.000 | Reduziert die Ergebnisse durch die Verwendung eines Abfragebereichs, eines Zeitbereichs und von Filtern in der Abfrage. |


**Datensammler-API**

| Category | Begrenzung | Kommentare |
|:---|:---|:---|
| Maximale Größe für einen einzelnen Beitrag | 30 MB | Teilen Sie größere Volumen auf mehrere Beiträge auf. |
| Maximale Größe für Feldwerte  | 32 KB | Felder mit einer Länge von mehr als 32 KB werden abgeschnitten. |

**Search-API**

| Category | Begrenzung | Kommentare |
|:---|:---|:---|
| Maximale Anzahl von Datensätzen, die bei einer einzelnen Abfrage zurückgegeben werden | 500.000 | |
| Maximale Größe der zurückgegebenen Daten | 64.000.000 Byte (~61 MiB)| |
| Maximale Ausführungszeit der Abfrage | 10 Minuten | Weitere Informationen finden Sie unter [Timeouts (Zeitlimit)](https://dev.loganalytics.io/documentation/Using-the-API/Timeouts).  |
| Maximale Anforderungsrate | 200 Anforderungen pro 30 Sekunden und AAD-Benutzer oder Client-IP-Adresse | Weitere Informationen finden Sie unter [Rate limits (Ratenlimits)](https://dev.loganalytics.io/documentation/Using-the-API/Limits). |

**Allgemeine Grenzwerte für Arbeitsbereiche**

| Category | Begrenzung | Kommentare |
|:---|:---|:---|
| Maximale Anzahl von Spalten in einer Tabelle         | 500 | |
| Maximale Anzahl an Zeichen für einen Spaltennamen | 500 | |
| Datenexport | Derzeit nicht verfügbar | Verwenden Sie Azure Functions oder Logic Apps, um Daten zu aggregieren und zu exportieren. | 

**Rate für Datenerfassungsvolumen**


Azure Monitor ist ein Hochleistungs-Datendienst, der Tausende Kunden bedient, die mit zunehmender Tendenz jeden Monat Terabytes von Daten senden. Das Standardratenlimit für das Erfassungsvolumen für Daten, die von Azure-Ressourcen mit [Diagnoseeinstellungen](../articles/azure-monitor/platform/diagnostic-settings.md) gesendet werden, beträgt ungefähr **6 GB/Minute** pro Arbeitsbereich. Dies ist ein ungefährer Wert, da die tatsächliche Größe je nach Protokolllänge und Komprimierungsverhältnis zwischen den Datentypen variieren kann. Dieses Limit gilt nicht für Daten, die von Agents oder der [Data Collector-API](../articles/azure-monitor/platform/data-collector-api.md) gesendet werden.

Wenn Sie Daten mit einer höheren Rate an einen einzelnen Arbeitsbereich senden, werden einige Daten gelöscht, und es wird alle sechs Stunden ein Ereignis an die Tabelle *Vorgang* im Arbeitsbereich gesendet, während der Schwellenwert weiterhin überschritten wird. Wenn das Datenerfassungsvolumen weiterhin das Ratenlimit überschreitet oder Sie es wahrscheinlich in Kürze erreichen werden, können Sie eine Erhöhung für Ihren Arbeitsbereich anfordern, indem Sie eine E-Mail an LAIngestionRate@microsoft.com senden oder eine Supportanfrage öffnen.
 
Damit Sie bei einem solchen Ereignis in Ihrem Arbeitsbereich benachrichtigt werden, erstellen Sie eine [Protokollwarnungsregel](../articles/azure-monitor/platform/alerts-log.md). Dazu verwenden Sie die folgende Abfrage mit der Warnungslogik basierend auf der Anzahl von Ergebnissen größer null.

``` Kusto
Operation
|where OperationCategory == "Ingestion"
|where Detail startswith "The rate of data crossed the threshold"
``` 


>[!NOTE]
>Abhängig von Ihrer Log Analytics-Nutzungsdauer haben Sie ggf. Zugang zu Legacytarifen. Weitere Informationen zu Legacytarifen von Log Analytics finden Sie [hier](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#legacy-pricing-tiers). 
