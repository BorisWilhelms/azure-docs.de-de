---
title: Pools für elastische Datenbanken mit DTU-Ressourcenlimits
description: Diese Seite beschreibt einige allgemeine DTU-Ressourcenlimits für Pools für elastische Datenbanken in Azure SQL-Datenbank.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: seo-lt-2019
ms.devlang: ''
ms.topic: conceptual
author: sachinpMSFT
ms.author: sachinp
ms.reviewer: carlrab
ms.date: 03/14/2019
ms.openlocfilehash: 76085dc29d40944cf704dbc5efc578b3314f499a
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81419237"
---
# <a name="resources-limits-for-elastic-pools-using-the-dtu-purchasing-model"></a>Grenzwerte für Ressourcen für Pools für elastische Datenbanken, die das DTU-Kaufmodell verwenden

Dieser Artikel enthält ausführliche Angaben zu Ressourcenlimits für Pools für elastische Datenbanken und Pooldatenbanken in Azure SQL-Datenbank, die das DTU-Kaufmodell verwenden.

Informationen zu Ressourcenlimits für einzelne Datenbanken beim DTU-Kaufmodell finden Sie unter [Ressourcenlimits für Pools für elastische Datenbanken, die das vCore-basierte Kaufmodell verwenden](sql-database-vcore-resource-limits-elastic-pools.md). Informationen zu V-Kern-Ressourcenlimits finden Sie unter [V-Kern-Ressourcenlimits – Einzeldatenbanken](sql-database-vcore-resource-limits-single-databases.md) und [V-Kern-Ressourcenlimits – Pools für elastische Datenbanken](sql-database-vcore-resource-limits-elastic-pools.md).

## <a name="elastic-pool-storage-sizes-and-compute-sizes"></a>Pool für elastische Datenbanken: Speicher- und Computegrößen

Die folgenden Tabellen enthalten die verfügbaren Ressourcen für Pools für elastische SQL-Datenbank-Instanzen für alle Dienstebenen und Computegrößen. Sie können im [Azure-Portal](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), mit [PowerShell](sql-database-elastic-pool-manage.md#powershell-manage-elastic-pools-and-pooled-databases), mit der [Azure CLI](sql-database-elastic-pool-manage.md#azure-cli-manage-elastic-pools-and-pooled-databases) oder der [REST-API](sql-database-elastic-pool-manage.md#rest-api-manage-elastic-pools-and-pooled-databases) Dienstebene, Computegröße und Speichermenge festlegen.

> [!IMPORTANT]
> Anleitungen und Überlegungen zur Skalierung finden Sie unter [Skalieren eines Pools für elastische Datenbanken](sql-database-elastic-pool-scale.md).
> [!NOTE]
> Die Ressourcengrenzwerte einzelner Datenbanken in Pools für elastische Datenbanken entsprechen im Allgemeinen hinsichtlich DTU-Anzahl und Dienstebene denen einzelner Datenbanken außerhalb von Pools. Auf eine S2-Datenbank können z.B. max. 120 Mitarbeiter gleichzeitig zugreifen. Also können auch 120 Mitarbeiter gleichzeitig auf eine Datenbank in einem Standard-Pool zugreifen, wenn max. 50 DTUs pro Datenbank im Pool zulässig sind (entspricht S2).

> [!NOTE]
> Das Speicherressourcenlimit pro Pool in den folgenden Tabellen gilt nicht für die Speicher „tempdb“ und „log“.

### <a name="basic-elastic-pool-limits"></a>Grenzwerte für Pools für elastische Datenbanken – Basic

| eDTUs pro Pool | **50** | **100** | **200** | **300** | **400** | **800** | **1200** | **1600** |
|:---|---:|---:|---:| ---: | ---: | ---: | ---: | ---: |
| Inbegriffener Speicher pro Pool (GB) | 5 | 10 | 20 | 29 | 39 | 78 | 117 | 156 |
| Max. Speicherlösungen pro Pool (GB) | 5 | 10 | 20 | 29 | 39 | 78 | 117 | 156 |
| Max. In-Memory-OLTP-Speicher pro Pool (GB) | – | – | – | – | – | – | – | – |
| Max. Anzahl Datenbanken pro Pool <sup>1</sup> | 100 | 200 | 500 | 500 | 500 | 500 | 500 | 500 |
| Max. gleichzeitige Worker (Anforderungen) pro Pool <sup>2</sup> | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Max. gleichzeitige Sitzungen pro Pool <sup>2</sup> | 30.000 | 30.000 | 30.000 | 30.000 |30.000 | 30.000 | 30.000 | 30.000 |
| Min. Anzahl von eDTU-Lösungen pro Datenbank | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 |
| Max. Anzahl von eDTU-Lösungen pro Datenbank | 5 | 5 | 5 | 5 | 5 | 5 | 5 | 5 |
| Max. Speicherkapazität pro Datenbank (GB) | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 |
||||||||

<sup>1</sup> Weitere Überlegungen finden Sie unter [Ressourcenverwaltung in umfangreichen Pools für elastische Datenbanken](sql-database-elastic-pool-resource-management.md).

<sup>2</sup> Den Wert für „Max. gleichzeitige Worker (Anforderungen)“ für einzelne Datenbanken finden Sie unter [Ressourcenlimits für Singletons mit dem auf virtuellen Kernen (V-Kernen) basierenden Kaufmodell](sql-database-vcore-resource-limits-single-databases.md). Wenn für den Pool für elastische Datenbanken beispielsweise Gen5 verwendet wird und die maximale Anzahl von V-Kernen pro Datenbank auf 2 festgelegt ist, ist der Wert für die Anzahl von gleichzeitigen Workern 200.  Wenn die maximale Anzahl von V-Kernen pro Datenbank auf 0,5 festgelegt ist, beträgt der Wert für die Anzahl von gleichzeitigen Workern 50, da unter Gen5 maximal 100 gleichzeitige Worker pro V-Kern verwendet werden. Für andere Einstellungen zur maximalen Anzahl von V-Kernen pro Datenbank (1 V-Kern oder weniger), wird die maximale Anzahl von gleichzeitigen Workern auf ähnliche Weise neu skaliert.

### <a name="standard-elastic-pool-limits"></a>Grenzwerte für Pools für elastische Datenbanken – Standard

| eDTUs pro Pool | **50** | **100** | **200** | **300** | **400** | **800**|
|:---|---:|---:|---:| ---: | ---: | ---: |
| Inbegriffener Speicher pro Pool (GB) | 50 | 100 | 200 | 300 | 400 | 800 |
| Max. Speicherlösungen pro Pool (GB) | 50, 250, 500 | 100, 250, 500, 750 | 200, 250, 500, 750, 1024 | 300, 500, 750, 1024, 1280 | 400, 500, 750, 1024, 1280, 1536 | 800, 1024, 1280, 1536, 1792, 2048 |
| Max. In-Memory-OLTP-Speicher pro Pool (GB) | – | – | – | – | – | – |
| Max. Anzahl Datenbanken pro Pool <sup>1</sup> | 100 | 200 | 500 | 500 | 500 | 500 |
| Max. gleichzeitige Worker (Anforderungen) pro Pool <sup>2</sup> | 100 | 200 | 400 | 600 | 800 | 1600 |
| Max. gleichzeitige Sitzungen pro Pool <sup>2</sup> | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 |
| Min. Anzahl von eDTU-Lösungen pro Datenbank | 0, 10, 20, 50 | 0, 10, 20, 50, 100 | 0, 10, 20, 50, 100, 200 | 0, 10, 20, 50, 100, 200, 300 | 0, 10, 20, 50, 100, 200, 300, 400 | 0, 10, 20, 50, 100, 200, 300, 400, 800 |
| Max. Anzahl von eDTU-Lösungen pro Datenbank | 10, 20, 50 | 10, 20, 50, 100 | 10, 20, 50, 100, 200 | 10, 20, 50, 100, 200, 300 | 10, 20, 50, 100, 200, 300, 400 | 10, 20, 50, 100, 200, 300, 400, 800 |
| Max. Speicherkapazität pro Datenbank (GB) | 500 | 750 | 1024 | 1024 | 1024 | 1024 |
||||||||

<sup>1</sup> Weitere Überlegungen finden Sie unter [Ressourcenverwaltung in umfangreichen Pools für elastische Datenbanken](sql-database-elastic-pool-resource-management.md).

<sup>2</sup> Den Wert für „Max. gleichzeitige Worker (Anforderungen)“ für einzelne Datenbanken finden Sie unter [Ressourcenlimits für Singletons mit dem auf virtuellen Kernen (V-Kernen) basierenden Kaufmodell](sql-database-vcore-resource-limits-single-databases.md). Wenn für den Pool für elastische Datenbanken beispielsweise Gen5 verwendet wird und die maximale Anzahl von V-Kernen pro Datenbank auf 2 festgelegt ist, ist der Wert für die Anzahl von gleichzeitigen Workern 200.  Wenn die maximale Anzahl von V-Kernen pro Datenbank auf 0,5 festgelegt ist, beträgt der Wert für die Anzahl von gleichzeitigen Workern 50, da unter Gen5 maximal 100 gleichzeitige Worker pro V-Kern verwendet werden. Für andere Einstellungen zur maximalen Anzahl von V-Kernen pro Datenbank (1 V-Kern oder weniger), wird die maximale Anzahl von gleichzeitigen Workern auf ähnliche Weise neu skaliert.

### <a name="standard-elastic-pool-limits-continued"></a>Grenzwerte für Pools für elastische Datenbanken – Standard (Fortsetzung)

| eDTUs pro Pool | **1200** | **1600** | **2000** | **2500** | **3000** |
|:---|---:|---:|---:| ---: | ---: |
| Inbegriffener Speicher pro Pool (GB) | 1200 | 1600 | 2000 | 2500 | 3000 |
| Max. Speicherlösungen pro Pool (GB) | 1200, 1280, 1536, 1792, 2048, 2304, 2560 | 1600, 1792, 2048, 2304, 2560, 2816, 3072 | 2000, 2048, 2304, 2560, 2816, 3072, 3328, 3584 | 2500, 2560, 2816, 3072, 3328, 3584, 3840, 4096 | 3000, 3072, 3328, 3584, 3840, 4096 |
| Max. In-Memory-OLTP-Speicher pro Pool (GB) | – | – | – | – | – |
| Max. Anzahl Datenbanken pro Pool <sup>1</sup> | 500 | 500 | 500 | 500 | 500 |
| Max. gleichzeitige Worker (Anforderungen) pro Pool <sup>2</sup> | 2400 | 3200 | 4000 | 5\.000 | 6000 |
| Max. gleichzeitige Sitzungen pro Pool <sup>2</sup> | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 |
| Min. Anzahl von eDTU-Lösungen pro Datenbank | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 |
| Max. Anzahl von eDTU-Lösungen pro Datenbank | 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 |
| Max. Speicherlösungen pro Datenbank (GB) | 1024 | 1024 | 1024 | 1024 | 1024 |
|||||||

<sup>1</sup> Weitere Überlegungen finden Sie unter [Ressourcenverwaltung in umfangreichen Pools für elastische Datenbanken](sql-database-elastic-pool-resource-management.md).

<sup>2</sup> Den Wert für „Max. gleichzeitige Worker (Anforderungen)“ für einzelne Datenbanken finden Sie unter [Ressourcenlimits für Singletons mit dem auf virtuellen Kernen (V-Kernen) basierenden Kaufmodell](sql-database-vcore-resource-limits-single-databases.md). Wenn für den Pool für elastische Datenbanken beispielsweise Gen5 verwendet wird und die maximale Anzahl von V-Kernen pro Datenbank auf 2 festgelegt ist, ist der Wert für die Anzahl von gleichzeitigen Workern 200.  Wenn die maximale Anzahl von V-Kernen pro Datenbank auf 0,5 festgelegt ist, beträgt der Wert für die Anzahl von gleichzeitigen Workern 50, da unter Gen5 maximal 100 gleichzeitige Worker pro V-Kern verwendet werden. Für andere Einstellungen zur maximalen Anzahl von V-Kernen pro Datenbank (1 V-Kern oder weniger), wird die maximale Anzahl von gleichzeitigen Workern auf ähnliche Weise neu skaliert.

### <a name="premium-elastic-pool-limits"></a>Grenzwerte für Pools für elastische Datenbanken – Premium

| eDTUs pro Pool | **125** | **250** | **500** | **1000** | **1500**|
|:---|---:|---:|---:| ---: | ---: |
| Inbegriffener Speicher pro Pool (GB) | 250 | 500 | 750 | 1024 | 1536 |
| Max. Speicherlösungen pro Pool (GB) | 250, 500, 750, 1024 | 500, 750, 1024 | 750, 1024 | 1024 | 1536 |
| Max. In-Memory-OLTP-Speicher pro Pool (GB) | 1 | 2 | 4 | 10 | 12 |
| Max. Anzahl Datenbanken pro Pool <sup>1</sup> | 50 | 100 | 100 | 100 | 100 |
| Max. gleichzeitige Worker pro Pool (Anforderungen)<sup>2</sup> | 200 | 400 | 800 | 1600 | 2400 |
| Max. gleichzeitige Sitzungen pro Pool <sup>2</sup> | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 |
| Min. Anz. von eDTUs pro Datenbank | 0, 25, 50, 75, 125 | 0, 25, 50, 75, 125, 250 | 0, 25, 50, 75, 125, 250, 500 | 0, 25, 50, 75, 125, 250, 500, 1000 | 0, 25, 50, 75, 125, 250, 500, 1000|
| Max. Anz. von eDTUs pro Datenbank | 25, 50, 75, 125 | 25, 50, 75, 125, 250 | 25, 50, 75, 125, 250, 500 | 25, 50, 75, 125, 250, 500, 1000 | 25, 50, 75, 125, 250, 500, 1000|
| Max. Speicherkapazität pro Datenbank (GB) | 1024 | 1024 | 1024 | 1024 | 1024 |
|||||||

<sup>1</sup> Weitere Überlegungen finden Sie unter [Ressourcenverwaltung in umfangreichen Pools für elastische Datenbanken](sql-database-elastic-pool-resource-management.md).

<sup>2</sup> Den Wert für „Max. gleichzeitige Worker (Anforderungen)“ für einzelne Datenbanken finden Sie unter [Ressourcenlimits für Singletons mit dem auf virtuellen Kernen (V-Kernen) basierenden Kaufmodell](sql-database-vcore-resource-limits-single-databases.md). Wenn für den Pool für elastische Datenbanken beispielsweise Gen5 verwendet wird und die maximale Anzahl von V-Kernen pro Datenbank auf 2 festgelegt ist, ist der Wert für die Anzahl von gleichzeitigen Workern 200.  Wenn die maximale Anzahl von V-Kernen pro Datenbank auf 0,5 festgelegt ist, beträgt der Wert für die Anzahl von gleichzeitigen Workern 50, da unter Gen5 maximal 100 gleichzeitige Worker pro V-Kern verwendet werden. Für andere Einstellungen zur maximalen Anzahl von V-Kernen pro Datenbank (1 V-Kern oder weniger), wird die maximale Anzahl von gleichzeitigen Workern auf ähnliche Weise neu skaliert.

### <a name="premium-elastic-pool-limits-continued"></a>Grenzwerte für Pools für elastische Datenbanken – Premium (Fortsetzung)

| eDTUs pro Pool | **2000** | **2500** | **3000** | **3500** | **4000**|
|:---|---:|---:|---:| ---: | ---: |
| Inbegriffener Speicher pro Pool (GB) | 2048 | 2560 | 3072 | 3548 | 4096 |
| Max. Speicherlösungen pro Pool (GB) | 2048 | 2560 | 3072 | 3548 | 4096|
| Max. In-Memory-OLTP-Speicher pro Pool (GB) | 16 | 20 | 24 | 28 | 32 |
| Max. Anzahl Datenbanken pro Pool <sup>1</sup> | 100 | 100 | 100 | 100 | 100 |
| Max. gleichzeitige Worker (Anforderungen) pro Pool <sup>2</sup> | 3200 | 4000 | 4800 | 5600 | 6400 |
| Max. gleichzeitige Sitzungen pro Pool <sup>2</sup> | 30.000 | 30.000 | 30.000 | 30.000 | 30.000 |
| Min. Anzahl von eDTU-Lösungen pro Datenbank | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 |
| Max. Anzahl von eDTU-Lösungen pro Datenbank | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 |
| Max. Speicherkapazität pro Datenbank (GB) | 1024 | 1024 | 1024 | 1024 | 1024 |
|||||||

<sup>1</sup> Weitere Überlegungen finden Sie unter [Ressourcenverwaltung in umfangreichen Pools für elastische Datenbanken](sql-database-elastic-pool-resource-management.md).

<sup>2</sup> Den Wert für „Max. gleichzeitige Worker (Anforderungen)“ für einzelne Datenbanken finden Sie unter [Ressourcenlimits für Singletons mit dem auf virtuellen Kernen (V-Kernen) basierenden Kaufmodell](sql-database-vcore-resource-limits-single-databases.md). Wenn für den Pool für elastische Datenbanken beispielsweise Gen5 verwendet wird und die maximale Anzahl von V-Kernen pro Datenbank auf 2 festgelegt ist, ist der Wert für die Anzahl von gleichzeitigen Workern 200.  Wenn die maximale Anzahl von V-Kernen pro Datenbank auf 0,5 festgelegt ist, beträgt der Wert für die Anzahl von gleichzeitigen Workern 50, da unter Gen5 maximal 100 gleichzeitige Worker pro V-Kern verwendet werden. Für andere Einstellungen zur maximalen Anzahl von V-Kernen pro Datenbank (1 V-Kern oder weniger), wird die maximale Anzahl von gleichzeitigen Workern auf ähnliche Weise neu skaliert.

> [!IMPORTANT]
> In allen Regionen außer den folgenden ist im Premium-Tarif derzeit mehr als 1 TB Speicher verfügbar: Regionen „China, Osten“, „China, Norden“, „Deutschland, Mitte“, „Deutschland, Nordosten“, „USA, Westen-Mitte“, „US DoD“ und „US Government, Mitte“. In diesen Regionen ist der Speicher im Tarif „Premium“ auf 1 TB begrenzt.  Weitere Informationen finden Sie unter [Einschränkungen von P11 und P15](sql-database-single-database-scale.md#p11-and-p15-constraints-when-max-size-greater-than-1-tb).

Wenn alle DTUs eines Pools für elastische Datenbanken verwendet werden, erhält jede Datenbank im Pool gleich viel Ressourcen zum Verarbeiten von Abfragen. Der SQL-Datenbank-Dienst bietet eine faire gemeinsame Nutzung von Ressourcen durch Datenbanken, indem gleiche Slices an Computezeit zugesichert werden. Diese faire gemeinsame Nutzung in Pools für elastische Datenbanken ergänzt jegliche Ressourcen, die jeder Datenbank auf andere Weise garantiert werden, wenn das DTU-Minimum pro Datenbank auf einen Wert ungleich null festgelegt ist.

> [!NOTE]
> Weitere Informationen zu `tempdb`-Einschränkungen finden Sie unter [tempdb-Grenzwerte](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database?view=sql-server-2017#tempdb-database-in-sql-database).

### <a name="database-properties-for-pooled-databases"></a>Eigenschaften von Pooldatenbanken

Die folgende Tabelle beschreibt die Eigenschaften von Pooldatenbanken.

| Eigenschaft | BESCHREIBUNG |
|:--- |:--- |
| Max. Anz. von eDTUs pro Datenbank |Die maximale Anzahl von eDTUs, die jede Datenbank im Pool verwenden kann, sofern basierend auf der Nutzung durch andere Datenbanken im Pool verfügbar. Die maximale Anzahl der eDTUs pro Datenbank ist keine Ressourcengarantie für eine Datenbank. Dies ist eine globale Einstellung, die für alle Datenbanken im Pool gilt. Legen Sie die maximale Anzahl der eDTUs pro Datenbank hoch genug fest, sodass Spitzen bei der Datenbanknutzung verarbeitet werden können. Sie sollten ein gewisses Maß an Mehrlast einplanen, da für den Pool im Allgemeinen von Nutzungsmustern starker und schwacher Auslastung ausgegangen wird, bei der aber nicht alle Datenbanken gleichzeitig stark ausgelastet sind. Angenommen, die Spitzenauslastung pro Datenbank beträgt 20 eDTUs und betrifft nur 20 % der 100 Datenbanken im Pool. Wenn die eDTU-Höchstanzahl pro Datenbank auf 20 eDTUs festgelegt ist, ist es sinnvoll, die fünffache Mehrlast für den Pool einzuplanen und die eDTUs pro Pool auf 400 festzulegen. |
| Min. Anz. von eDTUs pro Datenbank |Die minimale Anzahl der eDTUs, die für jede Datenbank im Pool garantiert werden können. Dies ist eine globale Einstellung, die für alle Datenbanken im Pool gilt. Die Mindestanzahl der eDTUs pro Datenbank kann auf 0 festgelegt werden. Dies ist auch der Standardwert. Diese Eigenschaft ist auf einen Wert zwischen 0 und der durchschnittlichen eDTU-Nutzung pro Datenbank festgelegt. Das Produkt aus der Anzahl von Datenbanken im Pool und der Mindestzahl von eDTUs pro Datenbank darf die tatsächliche Anzahl der eDTUs pro Pool nicht übersteigen. Wenn ein Pool beispielsweise 20 Datenbanken umfasst und die Mindestanzahl der eDTUs pro Datenbank auf 10 eDTUs festgelegt hat, müssen mindestens 200 eDTUs pro Pool festgelegt sein. |
| Max. Speicherkapazität pro Datenbank |Die maximale Datenbankgröße, die vom Benutzer für eine Datenbank in einem Pool festgelegt wird. Pooldatenbanken teilen sich den zugeordneten Poolspeicher. Auch wenn für die gesamte maximale Speicherkapazität *pro Datenbank* ein größerer Wert als für den gesamten verfügbaren *Speicherplatz des Pools* festgelegt wird, kann der von allen Datenbanken verwendete Gesamtspeicherplatz den verfügbaren Poolgrenzwert nicht überschreiten. Die maximale Datenbankgröße bezieht sich auf die maximale Größe der Datendateien und umfasst nicht den von Protokolldateien belegten Speicherplatz. |
|||

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu V-Kern-Ressourcenlimits für eine Einzeldatenbank finden Sie unter [Ressourcenlimits für Einzeldatenbanken, die das V-Kern-Kaufmodell verwenden](sql-database-vcore-resource-limits-single-databases.md).
- Informationen zu DTU-Ressourcenlimits für einen Singleton finden Sie unter [Ressourcenlimits für Singletons, die das DTU-Kaufmodell verwenden](sql-database-dtu-resource-limits-single-databases.md).
- Informationen zu V-Kern-Ressourcenlimits für Pools für elastische Datenbanken finden Sie unter [Ressourcenlimits für Pools für elastische Datenbanken, die das V-Kern-Kaufmodell verwenden](sql-database-vcore-resource-limits-elastic-pools.md).
- Informationen zu den Ressourcenlimits für verwaltete Instanzen finden Sie unter [Ressourcenlimits bei verwalteten Instanzen](sql-database-managed-instance-resource-limits.md).
- Informationen zu allgemeinen Azure-Einschränkungen finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-resource-manager/management/azure-subscription-service-limits.md).
- Informationen zu Ressourcenlimits auf Server- und Abonnementebene auf einem Datenbankserver finden Sie unter [Übersicht über Ressourcenlimits für einen SQL-Datenbank-Server](sql-database-resource-limits-database-server.md).
