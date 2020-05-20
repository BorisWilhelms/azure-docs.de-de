---
title: Erste Schritte – Textübersetzungs-API
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erfahren Sie, wie Sie sich für die Textübersetzungs-API von Azure Cognitive Services anmelden und einen Abonnementschlüssel erhalten.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.custom: seodec18
ms.openlocfilehash: cfae5318213e8af164c8d0cc9ed378705a5644b0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "73835704"
---
# <a name="how-to-sign-up-for-the-translator-text-api"></a>Registrieren für die Textübersetzungs-API

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

- Sie haben kein Konto? Sie können ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, um kostenlos zu experimentieren.
- Sie verfügen bereits über ein Konto? [Anmelden](https://ms.portal.azure.com/)

## <a name="create-a-subscription-to-the-translator-text-api"></a>Erstellen eines Abonnements für die Textübersetzungs-API

Nachdem Sie sich beim Portal angemeldet haben, können Sie wie folgt ein Abonnement für die Textübersetzungs-API erstellen:

1. Wählen Sie **+ Ressource erstellen**.
1. Geben Sie im Suchfeld **Marketplace durchsuchen** den Suchbegriff **Textübersetzung** ein, und wählen Sie die Textübersetzungs-API in den Ergebnissen aus.
1. Wählen Sie **Erstellen** aus, um Details für das Abonnement zu definieren.
1. Wählen Sie in der Liste **Tarif** den Tarif aus, der Ihren Anforderungen am besten entspricht.
    1. Jedes Abonnement verfügt über einen Free-Tarif. Der Free-Tarif bietet dieselben Features und Funktionen wie die kostenpflichtigen Tarife, und er läuft nicht ab.
    1. Sie können nur ein kostenloses Abonnement für Ihr Konto verwenden.
1. Wählen Sie **Erstellen** aus, um das Erstellen des Abonnements abzuschließen.

## <a name="authentication-key"></a>Authentifizierungsschlüssel

Wenn Sie sich bei der Textübersetzungs-API registrieren, erhalten Sie einen personalisierten Zugriffsschlüssel, der nur für Ihr Abonnement gilt. Dieser Schlüssel ist für jeden Aufruf an die Textübersetzungs-API erforderlich.

1. Sie rufen Ihren Authentifizierungsschlüssel ab, indem Sie zunächst das zugehörige Abonnement auswählen.
1. Wählen Sie im Abschnitt **Ressourcenverwaltung** Ihres Abonnements **Schlüssel** aus.
1. Kopieren Sie einen der für Ihr Abonnement aufgeführten Schlüssel.

## <a name="learn-test-and-get-support"></a>Lernen, testen und Hilfe erhalten

- [Codebeispiele auf GitHub](https://github.com/MicrosoftTranslator)
- [Supportforum zur Microsoft-Sprachübersetzung](https://www.aka.ms/TranslatorForum)

Microsoft Translator erlaubt in der Regel einige erste Anforderungen, bevor der Status des Abonnementkontos überprüft wurde. Die ersten Microsoft Translator-API-Anforderungen sind erfolgreich, während nachfolgende Aufrufe zu Fehlern führen. Die Ursache wird in der Fehlermeldung angegeben. Protokollieren Sie die API-Antwort, damit Sie die Ursache sehen können.

## <a name="pricing-options"></a>Preisoptionen

- [Textübersetzungs-API](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/)

## <a name="customization"></a>Anpassung

Verwenden Sie Custom Translator zum Anpassen Ihrer Übersetzungen, und erstellen Sie basierend auf den generischen Systemen von Microsoft Translator für neuronale maschinelle Übersetzungen ein Übersetzungssystem, das an Ihre eigene Terminologie und Ihren Stil angepasst ist. [Weitere Informationen](customization.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Erste Schritte mit Azure (3-Minuten-Video)](https://azure.microsoft.com/get-started/?b=16.24)
- [Bezahlen auf Rechnung](https://azure.microsoft.com/pricing/invoicing/)
