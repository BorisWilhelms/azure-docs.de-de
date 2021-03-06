---
title: Azure Deutschland im Überblick | Microsoft-Dokumentation
description: Dieser Artikel enthält einen Überblick über Azure Deutschland-Cloudfunktionen, den vertrauenswürdigen Aufbau und die Sicherheitsmerkmale, die die Complianceanforderungen deutscher Datenschutzrichtlinien unterstützen.
services: germany
cloud: na
documentationcenter: na
author: gitralf
manager: rainerst
ms.assetid: na
ms.service: germany
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2019
ms.author: ralfwi
ms.openlocfilehash: 62e4e12097bbf705bb83ea90cc728105503e6188
ms.sourcegitcommit: fe6c9a35e75da8a0ec8cea979f9dec81ce308c0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "78675309"
---
# <a name="welcome-to-azure-germany"></a>Willkommen bei Azure Deutschland

> [!IMPORTANT]
> Seit [August 2018](https://news.microsoft.com/europe/2018/08/31/microsoft-to-deliver-cloud-services-from-new-datacentres-in-germany-in-2019-to-meet-evolving-customer-needs/) haben wir keine neuen Kunden akzeptiert und keine neuen Features und Dienste an den ursprünglichen Microsoft Cloud Deutschland-Standorten bereitgestellt.
>
> Aufgrund der Weiterentwicklung der Kundenbedürfnisse haben wir vor Kurzem zwei neue Rechenzentrumsregionen in Deutschland [gestartet](https://azure.microsoft.com/blog/microsoft-azure-available-from-new-cloud-regions-in-germany/), die Data Residency für Kundendaten, umfassende Konnektivität mit dem globalen Cloudnetzwerk von Microsoft sowie wettbewerbsfähige Preise bieten. 
>
> Profitieren Sie von der Vielfalt der Funktionen, Sicherheit auf Unternehmensniveau und den umfangreichen Features, die in unseren neuen deutschen Rechenzentrumsregionen zur Verfügung stehen, und [migrieren](germany-migration-main.md) Sie noch heute.

## <a name="overview"></a>Übersicht
Microsoft Azure Deutschland bietet eine Cloudplattform, die auf den [grundlegenden Prinzipien von Sicherheit, Datenschutz, Compliance und Transparenz](https://azure.microsoft.com/overview/clouds/germany/) beruht. Azure Deutschland ist eine physisch isolierte Instanz von Microsoft Azure. Es basiert auf Sicherheits- und [Compliancediensten](https://azure.microsoft.com/support/trust-center/compliance/), die für alle auf dieser Architektur beruhenden Systeme und Anwendungen hinsichtlich deutscher Datenschutzbestimmungen von entscheidender Bedeutung sind. Azure Deutschland wird von einem Datentreuhänder überwacht und unterstützt verschiedene Hybridszenarien für die Erstellung und Bereitstellung von Lösungen lokal oder in der Cloud. Sie können auch die sofortige Skalierbarkeit und garantierte Verfügbarkeit eines überaus skalierbaren Clouddiensts nutzen.

Es bietet Datenhoheit bei der Übertragung und Speicherung in Deutschland und gewährleistet die Geschäftskontinuität per Datenreplikation zwischen deutschen Rechenzentren. Kundendaten in beiden Rechenzentren werden unter der Kontrolle eines Datentreuhänders, T-Systems International, verwaltet. Bei diesem Treuhänder handelt es sich um ein unabhängiges deutsches Unternehmen, eine Tochtergesellschaft der Deutschen Telekom. Dieser sorgt für eine umfassendere Kontrolle über Kundendaten, da der Zugriff nur mit der Zustimmung des Kunden oder des Datentreuhänders gewährt wird.

Die kommerziellen Clouddienste von Microsoft in diesen neuen Rechenzentren erfüllen die deutschen Gesetze zur Datenverarbeitung und bieten Kunden eine größere Auswahl an Möglichkeiten, wie und wo die Daten verarbeitet werden.

Azure Deutschland umfasst die Hauptkomponenten von Infrastructure-as-a-Service (IaaS), Platform-as-a-Service (PaaS) und Software-as-a-Service (SaaS). Zu diesen Komponenten gehören Infrastruktur, Netzwerk, Speicher, Datenverwaltung, Identitätsverwaltung und viele andere Dienste.

Azure Deutschland unterstützt im Wesentlichen dieselben hervorragenden Funktionen, die von Kunden der globalen Azure-Umgebung genutzt wurden, wie etwa geografisch synchrone Datenreplikation und automatische Skalierung. 

## <a name="azure-germany-documentation"></a>Azure Deutschland-Dokumentation
Diese Website beschreibt den Funktionsumfang der Dienste von [Microsoft Azure Deutschland](https://azure.microsoft.com/overview/clouds/germany/) und enthält allgemeine Informationen für alle Kunden. Bevor Sie speziell regulierte Daten in Ihr Azure Deutschland-Abonnement einfügen, sollten Sie sich mit den Funktionen von Azure Deutschland vertraut machen.

Auf der [Complianceseite von Microsoft Azure Trust Center](https://www.microsoft.com/TrustCenter/Compliance/default.aspx) finden Sie aktuelle Informationen zu den Azure Deutschland-Diensten, die spezifischen Akkreditierungen und Regulierungen unterliegen. Gegebenenfalls sind weitere Microsoft-Dienste verfügbar, die jedoch nicht zu den in Azure Deutschland vorhandenen Diensten gehören oder Gegenstand dieser Dokumentation sind. Azure Deutschland-Dienste ermöglichen unter anderem die Verwendung verschiedener zusätzlicher Ressourcen, Anwendungen oder Dienste, die von Drittanbietern (oder von Microsoft im Rahmen separater Nutzungsbedingungen und Datenschutzrichtlinien) bereitgestellt werden. Auch diese sind nicht Gegenstand dieser Dokumentation. Die Bedingungen solcher (etwa über den Azure Marketplace angebotenen) Zusatzangebote müssen von Ihnen eigenverantwortlich geprüft werden, um sicherzustellen, dass sie Ihre Complianceanforderungen erfüllen.

Azure Deutschland ist für Kunden und Partner weltweit verfügbar, die in der EU/EFTA, einschließlich Großbritannien, geschäftlich tätig sind.

## <a name="general-guidance-for-customers"></a>Allgemeine Richtlinien für Kunden
Bei den meisten der derzeit verfügbaren technischen Inhalte wird davon ausgegangen, dass Anwendungen für die globale Azure-Umgebung und nicht für Azure Deutschland entwickelt werden. Es ist unbedingt sicherzustellen, dass sich Entwickler über die wichtigsten Unterschiede der Anwendungen, die für das Hosting in Azure Deutschland entwickelt wurden, im Klaren sind:

* Bestimmte Dienste und Funktionen, die in bestimmten Regionen der globalen Azure-Umgebung vorhanden sind, sind möglicherweise in Azure Deutschland nicht verfügbar. Auf der Seite [Regionen](https://azure.microsoft.com/regions/services) finden Sie aktuelle Dienste, die allgemein verfügbar sind. 
* Für Funktionen in Azure Deutschland gibt es zudem hinsichtlich der Konfiguration Unterschiede zur globalen Azure-Umgebung. Aus diesem Grund sollten Sie Ihren Beispielcode, Ihre Konfigurationen und Schritte überprüfen, um sicherzustellen, dass Sie diese innerhalb der Azure Deutschland-Umgebung erstellen und ausführen.
* Informationen zur Abgrenzung von Azure Deutschland sowie Anleitungen und bewährte Methoden zu den durch Kunden regulierten und/oder gesteuerten Daten finden Sie in der technischen Dienstdokumentation zu Azure Deutschland auf dieser Website.

## <a name="next-steps"></a>Nächste Schritte
Abonnieren Sie den [Azure Deutschland-Blog](https://blogs.msdn.microsoft.com/azuregermany/), um weitere Informationen und Updates zu erhalten.

Wenn Sie mehr über Azure Deutschland erfahren möchten, verwenden Sie die folgenden Links:

* [Registrieren für eine Testversion](https://azure.microsoft.com/free/germany/)
* [Anmelden](https://portal.microsoftazure.de/) (wenn Sie bereits ein Azure Deutschland-Konto besitzen)
* [Erwerben von und Zugreifen auf Azure Deutschland](https://azure.microsoft.com/overview/clouds/germany/) 



