---
title: Anpassen der Übersetzung – Translator
titleSuffix: Azure Cognitive Services
description: Verwenden Sie den Microsoft Translator-Hub zum Erstellen Ihres eigenen Systems für die maschinelle Übersetzung mit der Terminologie und dem Stil, den Sie bevorzugen.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: c1928f9aa2c2b011e1cf54b4ef6cfc5cb8251904
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83588648"
---
# <a name="customize-your-text-translations"></a>Anpassen von Textübersetzungen

Benutzerdefinierter Translator ist ein Feature des Translator-Diensts, mit dem Sie die erweiterte neuronale maschinelle Übersetzung von Microsoft Translator anpassen können, wenn Sie Translator (nur Version 3) zum Übersetzen verwenden.

Das Feature kann mit [Cognitive Services Speech](https://docs.microsoft.com/azure/cognitive-services/speech-service/) auch zum Anpassen der Sprachübersetzung verwendet werden.

## <a name="custom-translator"></a>Benutzerdefinierter Translator

Mit dem benutzerdefinierten Translator können Sie neuronale Übersetzungssysteme erstellen, die die Terminologie verstehen, die in Ihrem Unternehmen und der Branche verwendet wird. Das angepasste Übersetzungssystem wird dann in vorhandene Anwendungen, Workflows und Websites integriert.

### <a name="how-does-it-work"></a>Wie funktioniert dies?

Verwenden Sie zuvor übersetzte Dokumente (Broschüren, Webseiten, Dokumentationen usw.), um ein Übersetzungssystem zu erstellen, das die domänenspezifische Terminologie und den Stil besser als ein Standardübersetzungssystem widerspiegelt. Benutzer können Dokumente in den folgenden Dateiformaten hochladen: TMX, XLIFF, TXT, DOCX und XLSX.  

Das System akzeptiert auch Daten, die parallel auf Dokumentebene parallel sind, jedoch noch nicht auf Satzebene ausgerichtet wurden. Wenn Sie über Zugriff auf Versionen mit dem gleichen Inhalt in mehreren Sprachen verfügen, die sich jedoch in separaten Dokumenten befinden, kann der benutzerdefinierte Translator Sätze über mehrere Dokumente hinweg automatisch abgleichen.  Das System kann auch einsprachige Daten in einer oder beiden Sprachen verwenden, um die parallelen Trainingsdaten zu ergänzen, um die Übersetzungen zu verbessern.

Das benutzerdefinierte System ist dann über einen regulären Aufruf von Translator mit dem Category-Parameter verfügbar.

Mit dem entsprechenden Typ und der Menge von Trainingsdaten ist es nicht ungewöhnlich, unter Verwendung des benutzerdefinierten Translators Gewinne von fünf bis zehn oder sogar mehr BLEU-Punkten für die Übersetzungsqualität zu erwarten.

Weitere Informationen über die verschiedenen Anpassungsstufen basierend auf verfügbaren Daten finden Sie im [Benutzerleitfaden für den benutzerdefinierten Translator](https://aka.ms/CustomTranslatorDocs).


## <a name="microsoft-translator-hub"></a>Microsoft Translator-Hub

> [!NOTE]
> Der veraltete Microsoft Translator-Hub wird am 17. April 2019 eingestellt. [Anzeigen von wichtigen Migrationsinformationen und Datumsangaben](https://www.microsoft.com/translator/business/hub/).  

## <a name="custom-translator-versus-hub"></a>Benutzerdefinierter Translator im Vergleich zum Hub

|   | **Hub** | **Benutzerdefinierter Translator**|
|:-----|:----:|:----:|
|Status des Anpassungsfeatures   | Allgemeine Verfügbarkeit  | Allgemeine Verfügbarkeit |
| Version der Text-API  | Nur V2   | Nur V3 |
| SMT-Anpassung | Ja   | Nein |
| NMT-Anpassung | Nein    | Ja |
| Neue einheitliche Anpassung der Sprachdienste | Nein    | Ja |
| [No Trace (Keine Ablaufverfolgung)](https://www.aka.ms/notrace) | Ja  | Ja |

## <a name="collaborative-translations-framework"></a>Framework für kollaborative Übersetzungen

> [!NOTE]
> Seit dem 1. Februar 2018 sind „AddTranslation()“ und „AddTranslationArray()“ nicht mehr für die Verwendung mit Translator v2.0 verfügbar. Diese Methoden schlagen fehl, und nichts wird geschrieben. Translator v3.0 unterstützt diese Methoden nicht.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Set up a customized language system using Custom Translator (Einrichten eines angepassten Sprachsystems mit dem benutzerdefinierten Translator)](https://aka.ms/CustomTranslatorDocs)
