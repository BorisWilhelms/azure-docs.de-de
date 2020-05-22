---
title: 'Tutorial: Veröffentlichungseinstellung – LUIS'
description: In diesem Tutorial ändern Sie die Veröffentlichungseinstellungen, um bessere Vorhersagen zu erzielen.
ms.topic: tutorial
ms.date: 04/01/2020
ms.openlocfilehash: d976d06fbf20ea0e64fa01bd94658a27d680bc86
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83588780"
---
# <a name="tutorial--add-sentiment-analysis-as-a-publishing-setting"></a>Tutorial:  Hinzufügen der Standpunktanalyse als Veröffentlichungseinstellung

In diesem Tutorial ändern Sie die Veröffentlichungseinstellungen, um die Stimmungsanalyse zu extrahieren. Fragen Sie anschließend den LUIS-Endpunkt ab, um die zurückgegebene Stimmung einer Benutzeräußerung anzuzeigen.

**In diesem Tutorial lernen Sie Folgendes:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Hinzufügen der Standpunktanalyse als Veröffentlichungseinstellung
> * Abrufen der Stimmung einer Äußerung vom veröffentlichten Endpunkt

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="sentiment-analysis-is-a-publish-setting"></a>Standpunktanalyse ist eine Veröffentlichungseinstellung

Die folgenden Äußerungen sind Beispiele für die Stimmung:

|Stimmung|Ergebnis|Äußerung|
|:--|:--|:--|
|Negativ|0.01 |Die Pizza war furchtbar.|
|Positiv|0,97 |Die Käsepizza war sehr lecker.|

Die Standpunktanalyse ist eine Veröffentlichungseinstellung, die auf jede Äußerung angewendet wird. Nach ihrer Festlegung gibt die App die Stimmung einer Äußerung zurück, ohne dass Daten bezeichnet werden müssen.

Da es sich um eine Veröffentlichungseinstellung handelt, wird diese nicht auf den Seiten mit Absichten oder Entitäten bezeichnet. Sie wird aber im Bereich für den [interaktiven Test](luis-interactive-test.md#view-sentiment-results) oder beim Testen an der Endpunkt-URL angezeigt.

## <a name="download-json-file-for-app"></a>Herunterladen der JSON-Datei für die App

Laden Sie die [App-JSON-Datei](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-language-understanding/master/documentation-samples/tutorials/machine-learned-entity/pizza-tutorial-with-entities.json) herunter, und speichern Sie sie.

## <a name="import-json-file-for-app"></a>Importieren der JSON-Datei für die App

[!INCLUDE [Import app steps](includes/import-app-steps.md)]

## <a name="train-the-app"></a>Trainieren der App

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="configure-app-to-include-sentiment-analysis"></a>Konfigurieren der App zur Aufnahme der Stimmungsanalyse

1. Wählen Sie im Menü oben die Option **Veröffentlichen** aus. Die Standpunktanalyse ist eine Veröffentlichungseinstellung.

1. Wählen Sie **Produktionsslot** und anschließend **Einstellungen ändern** aus.
1. Legen Sie die Einstellung für die Standpunktanalyse auf **Ein** fest.

    ![Aktivieren der Standpunktanalyse als Veröffentlichungseinstellung](./media/luis-quickstart-intent-and-sentiment-analysis/select-sentiment-publishing-setting.png)

## <a name="get-the-sentiment-of-an-utterance-from-the-endpoint"></a>Abrufen der Stimmung einer Äußerung vom Endpunkt

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]

1. Ersetzen Sie in der Adressleiste am Ende der URL die Angabe _YOUR_QUERY_HERE_ durch Folgendes:

    `Deliver 2 of the best cheese pizzas ever!!!`

    Diese Äußerung entspricht keiner der bezeichneten Äußerungen. Sie ist daher ein guter Test und sollte die Absicht `OrderPizza` mit der extrahierten Stimmungsanalyse zurückgeben.

    ```json
    {
        "query": "Deliver 2 of the best cheese pizzas ever!!!",
        "prediction": {
            "topIntent": "OrderPizza",
            "intents": {
                "OrderPizza": {
                    "score": 0.4659952
                },
                "None": {
                    "score": 0.16901511
                },
                "Confirm": {
                    "score": 0.00421415
                },
                "Greeting": {
                    "score": 0.00109914376
                }
            },
            "entities": {
                "number": [
                    2
                ],
                "$instance": {
                    "number": [
                        {
                            "type": "builtin.number",
                            "text": "2",
                            "startIndex": 8,
                            "length": 1,
                            "modelTypeId": 2,
                            "modelType": "Prebuilt Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ]
                }
            },
            "sentiment": {
                "label": "positive",
                "score": 0.861665964
            }
        }
    }
    ```

    Die Standpunktanalyse ist positiv (Wert: 86 %).

[!INCLUDE [LUIS How to clean up resources](includes/quickstart-tutorial-cleanup-resources.md)]

## <a name="related-information"></a>Verwandte Informationen

* Die Stimmungsanalyse wird von der Cognitive Service-[Textanalyse](../Text-Analytics/index.yml) bereitgestellt. Das Feature ist auf die von der Textanalyse [unterstützten Sprachen](luis-language-support.md#languages-supported) beschränkt.
* [Informationen zum Trainieren](luis-how-to-train.md)
* [Informationen zum Veröffentlichen](luis-how-to-publish-app.md)
* [Informationen zum Testen im LUIS-Portal](luis-interactive-test.md)


## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial wird die Standpunktanalyse als Veröffentlichungseinstellung zum Extrahieren von Stimmungswerten aus der gesamten Äußerung hinzugefügt.

> [!div class="nextstepaction"]
> [Überprüfen von Endpunktäußerungen in der Personal-App](luis-tutorial-review-endpoint-utterances.md)

