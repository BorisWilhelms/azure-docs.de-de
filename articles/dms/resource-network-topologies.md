---
title: Netzwerktopologien für Migrationen vom Typ „Verwaltete SQL-Instanz“
titleSuffix: Azure Database Migration Service
description: Lernen Sie die Quell- und Zielkonfigurationen für Migrationen von verwalteten Azure SQL-Datenbank-Instanzen mithilfe von Azure Database Migration Service kennen.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: article
ms.date: 01/08/2020
ms.openlocfilehash: 48485b7ba0f846afa737454b092a6c1ee986b737
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78254954"
---
# <a name="network-topologies-for-azure-sql-db-managed-instance-migrations-using-azure-database-migration-service"></a>Netzwerktopologien für Migrationen von verwalteten Azure SQL-Datenbank-Instanzen mithilfe von Azure Database Migration Service

In diesem Artikel werden verschiedene Netzwerktopologien erläutert, die Azure Database Migration Service verwenden kann, um eine umfassende Migration von lokalen SQL Server-Instanzen zu einer verwalteten Azure SQL-Datenbank-Instanz zu ermöglichen.

## <a name="azure-sql-database-managed-instance-configured-for-hybrid-workloads"></a>Verwaltete Azure SQL-Datenbank-Instanz konfiguriert für Hybridworkloads 

Verwenden Sie diese Topologie, wenn die verwaltete Azure SQL-Datenbank-Instanz mit Ihrem lokalen Netzwerk verbunden ist. Dieser Ansatz nutzt das einfachste Netzwerkrouting bei maximalem Datendurchsatz während der Migration.

![Netzwerktopologie für Hybridworkloads](media/resource-network-topologies/hybrid-workloads.png)

**Anforderungen**

- In diesem Szenario werden die verwaltete Azure SQL-Datenbank-Instanz und die Azure Database Migration Service-Instanz in derselben Microsoft Azure Virtual Network-Instanz erstellt, sie nutzen jedoch unterschiedliche Subnetze.  
- Das in diesem Szenario verwendete virtuelle Netzwerk ist über [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) oder [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) auch mit dem lokalen Netzwerk verbunden.

## <a name="azure-sql-database-managed-instance-isolated-from-the-on-premises-network"></a>Verwaltete Azure SQL-Datenbank-Instanz isoliert vom lokalen Netzwerk

Verwenden Sie diese Netzwerktopologie, wenn Ihre Umgebung mindestens eines der folgenden Szenarien erfordert:

- Die verwaltete Azure SQL-Datenbank-Instanz ist von der lokalen Konnektivität isoliert, aber die Azure Database Migration Service-Instanz ist mit dem lokalen Netzwerk verbunden.
- Es gelten RBAC-Richtlinien (rollenbasierte Zugriffssteuerung), und Sie müssen den Benutzerzugriff auf das Abonnement beschränken, unter dem die verwaltete Azure SQL-Datenbank-Instanz gehostet wird.
- Die für die verwaltete Azure SQL-Datenbank-Instanz und Azure Database Migration Service verwendeten virtuellen Netzwerke befinden sich in unterschiedlichen Abonnements.

![Netzwerktopologie für eine vom lokalen Netzwerk isolierte verwaltete Instanz](media/resource-network-topologies/mi-isolated-workload.png)

**Anforderungen**

- Das virtuelle Netzwerk, das Azure Database Migration Service für dieses Szenario verwendet, muss über ExpressRoute (https://docs.microsoft.com/azure/expressroute/expressroute-introduction) oder [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) auch mit dem lokalen Netzwerk verbunden sein.
- Richten Sie ein [VNET-Netzwerkpeering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) zwischen dem virtuellen Netzwerk für die verwaltete Azure SQL-Datenbank-Instanz und Azure Database Migration Service ein.

## <a name="cloud-to-cloud-migrations-shared-virtual-network"></a>Cloud-zu-Cloud-Migrationen: gemeinsam genutztes virtuelles Netzwerk

Verwenden Sie diese Topologie, wenn die SQL Server-Quelle auf einer Azure-VM gehostet wird und dasselbe virtuelle Netzwerk wie die verwaltete Azure SQL-Datenbank-Instanz und Azure Database Migration Service nutzt.

![Netzwerktopologie für Cloud-zu-Cloud-Migrationen mit einem gemeinsam genutzten VNet](media/resource-network-topologies/cloud-to-cloud.png)

**Anforderungen**

- Es bestehen keine weiteren Anforderungen.

## <a name="cloud-to-cloud-migrations-isolated-virtual-network"></a>Cloud-zu-Cloud-Migrationen: isoliertes virtuelles Netzwerk

Verwenden Sie diese Netzwerktopologie, wenn Ihre Umgebung mindestens eines der folgenden Szenarien erfordert:

- Die verwaltete Azure SQL-Datenbank-Instanz wird in einem isolierten virtuellen Netzwerk bereitgestellt.
- Es gelten RBAC-Richtlinien (rollenbasierte Zugriffssteuerung), und Sie müssen den Benutzerzugriff auf das Abonnement beschränken, unter dem die verwaltete Azure SQL-Datenbank-Instanz gehostet wird.
- Die für die verwaltete Azure SQL-Datenbank-Instanz und Azure Database Migration Service verwendeten virtuellen Netzwerke befinden sich in unterschiedlichen Abonnements.

![Netzwerktopologie für Cloud-zu-Cloud-Migrationen mit einem isolierten VNet](media/resource-network-topologies/cloud-to-cloud-isolated.png)

**Anforderungen**

- Richten Sie ein [VNET-Netzwerkpeering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) zwischen dem virtuellen Netzwerk für die verwaltete Azure SQL-Datenbank-Instanz und Azure Database Migration Service ein.

## <a name="inbound-security-rules"></a>Eingangssicherheitsregeln

| **NAME**   | **PORT** | **PROTOKOLL** | **QUELLE** | **ZIEL** | **AKTION** |
|------------|----------|--------------|------------|-----------------|------------|
| DMS_subnet | Any      | Any          | DMS-SUBNETZ | Any             | Allow      |

## <a name="outbound-security-rules"></a>Ausgangssicherheitsregeln

| **NAME**                  | **PORT**                                              | **PROTOKOLL** | **QUELLE** | **ZIEL**           | **AKTION** | **Grund für die Regel**                                                                                                                                                                              |
|---------------------------|-------------------------------------------------------|--------------|------------|---------------------------|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| management                | 443, 9354                                              | TCP          | Any        | Any                       | Allow      | Kommunikation auf Verwaltungsebene über Service Bus und Azure Blob Storage. <br/>(Wenn Microsoft-Peering aktiviert ist, benötigen Sie diese Regel möglicherweise nicht.)                                                             |
| Diagnose               | 12000                                                 | TCP          | Any        | Any                       | Allow      | DMS verwendet diese Regel zum Sammeln von Diagnoseinformationen für die Problembehandlung.                                                                                                                      |
| SQL-Quellserver         | 1433 (oder TCP-IP-Port, an dem SQL Server lauscht) | TCP          | Any        | Lokaler Adressraum | Allow      | SQL Server-Quellkonnektivität von DMS <br/>(Wenn Sie über Site-to-Site-Konnektivität verfügen, benötigen Sie diese Regel möglicherweise nicht.)                                                                                       |
| Benannte SQL Server-Instanz | 1434                                                  | UDP          | Any        | Lokaler Adressraum | Allow      | Quellkonnektivität der benannten SQL Server-Instanz von DMS <br/>(Wenn Sie über Site-to-Site-Konnektivität verfügen, benötigen Sie diese Regel möglicherweise nicht.)                                                                        |
| SMB-Freigabe                 | 445                                                   | TCP          | Any        | Lokaler Adressraum | Allow      | SMB-Netzwerkfreigabe, in der DMS Datenbank-Sicherungsdateien für Migrationen zu verwalteten Azure SQL-Datenbank-Instanzen und SQL Server-Instanzen auf virtuellen Azure-Computern speichert <br/>(Wenn Sie über Site-to-Site-Konnektivität verfügen, benötigen Sie diese Regel möglicherweise nicht.) |
| DMS_subnet                | Any                                                   | Any          | Any        | DMS_Subnet                | Allow      |                                                                                                                                                                                                  |

## <a name="see-also"></a>Weitere Informationen

- [Migrieren von SQL Server zu einer verwalteten Azure SQL-Datenbank-Instanz](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance)
- [Übersicht über die Voraussetzungen für die Verwendung von Azure Database Migration Service](https://docs.microsoft.com/azure/dms/pre-reqs)
- [Erstellen eines virtuellen Netzwerks im Azure-Portal](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über Azure Database Migration Service finden Sie im Artikel [Was ist Azure Database Migration Service?](dms-overview.md).
- Neueste Informationen zur regionalen Verfügbarkeit von Azure Database Migration Service finden Sie auf der Seite [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=database-migration).
