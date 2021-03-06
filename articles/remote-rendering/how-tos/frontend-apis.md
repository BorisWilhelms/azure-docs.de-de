---
title: Azure-Front-End-APIs für die Authentifizierung
description: Erläutert die Verwendung der C#-Front-End-API für die Authentifizierung
author: florianborn71
ms.author: flborn
ms.date: 02/12/2010
ms.topic: how-to
ms.openlocfilehash: 04296a3dab61fdb569126abc1bc1f975d69e226d
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2020
ms.locfileid: "80679248"
---
# <a name="use-the-azure-frontend-apis-for-authentication"></a>Verwenden der Azure-Front-End-APIs für die Authentifizierung

In diesem Abschnitt wird die Verwendung der C#-API für die Authentifizierung beschrieben.

## <a name="azurefrontendaccountinfo"></a>AzureFrontendAccountInfo

„AzureFrontendAccountInfo“ wird verwendet, um die Authentifizierungsinformationen für eine ```AzureFrontend```-Instanz im SDK einzurichten.

Die wichtigen Felder lauten:

```cs

    public class AzureFrontendAccountInfo
    {
        // Something akin to "<region>.mixedreality.azure.com"
        public string AccountDomain;

        // Can use one of:
        // 1) ID and Key.
        // 2) AuthenticationToken.
        // 3) AccessToken.
        public string AccountId = Guid.Empty.ToString();
        public string AccountKey = string.Empty;
        public string AuthenticationToken = string.Empty;
        public string AccessToken = string.Empty;
    }

```

Verwenden Sie für den Teil _region_ in der Domäne eine [Region in Ihrer Nähe](../reference/regions.md).

Die Kontoinformationen können aus dem Portal abgerufen werden, wie im Abschnitt [Abrufen von Kontoinformationen](create-an-account.md#retrieve-the-account-information) beschrieben.

## <a name="azure-frontend"></a>Azure-Front-End

Die relevanten Klassen lauten ```AzureFrontend``` und ```AzureSession```. ```AzureFrontend``` wird für die Kontoverwaltung und für Funktionalität auf Kontoebene verwendet, einschließlich der Ressourcenkonvertierung und der Erstellung von Renderingsitzungen. ```AzureSession``` wird für Funktionen auf Sitzungsebene verwendet, einschließlich Sitzungsaktualisierung, Abfragen, Erneuern und Außerbetriebsetzung.

Jede geöffnete/erstellte ```AzureSession``` verfügt über einen Verweis auf das Front-End, von dem sie erstellt wurde. Zum ordnungsgemäßen Herunterfahren muss die Zuordnung aller Sitzungen aufgehoben werden, bevor die Zuordnung des Front-Ends aufgehoben wird.

Durch Aufheben der Zuordnung einer Sitzung wird die VM in Azure nicht beendet; `AzureSession.StopAsync` muss explizit aufgerufen werden.

Nachdem eine Sitzung erstellt und ihr Status als bereit markiert wurde, kann sie mittels `AzureSession.ConnectToRuntime` mit der Remote Rendering-Runtime verbunden werden.

### <a name="threading"></a>Threading

Alle asynchronen Aufrufe von „AzureSession“ und „AzureFrontend“ werden in einem Hintergrundthread und nicht im Hauptanwendungsthread ausgeführt.

### <a name="conversion-apis"></a>Konvertierungs-APIs

Weitere Informationen zum Konvertierungsdienst finden Sie unter [Verwenden der REST-API für die Modellkonvertierung](conversion/conversion-rest-api.md).

#### <a name="start-asset-conversion"></a>Startet die Objektkonvertierung.

``` cs
private StartConversionAsync _pendingAsync = null;

void StartAssetConversion(AzureFrontend frontend, string modelName, string modelUrl, string assetContainerUrl)
{
    _pendingAsync = frontend.StartConversionAsync(
        new AssetConversionParams(modelName, modelUrl, assetContainerUrl));
    _pendingAsync.Completed +=
        (StartConversionAsync res) =>
        {
            if (res.IsRanToCompletion)
            {
                //use res.Result
            }
            else
            {
                Console.WriteLine("Failed to start asset conversion!");
            }
        };

        _pendingAsync = null;
}
```

#### <a name="get-conversion-status"></a>Abrufen des Konvertierungsstatus

``` cs
private ConversionStatusAsync _pendingAsync = null
void GetConversionStatus(AzureFrontend frontend, string assetId)
{
    _pendingAsync = frontend.GetConversionStatusAsync(assetId);
    _pendingAsync.Completed +=
        (ConversionStatusAsync res) =>
        {
            if (res.IsRanToCompletion)
            {
                //use res.Result
            }
            else
            {
                Console.WriteLine("Failed to get status of asset conversion!");
            }

            _pendingAsync = null;
        };
}
```

### <a name="rendering-apis"></a>Rendering-APIs

Ausführliche Informationen zur Sitzungsverwaltung finden Sie unter [REST-API für die Sitzungsverwaltung](session-rest-api.md).

Eine Renderingsitzung kann entweder dynamisch für den Dienst erstellt werden, oder eine Sitzung mit einer bereits vorhandenen Sitzungs-ID kann in einem AzureSession-Objekt geöffnet werden.

#### <a name="create-rendering-session"></a>Erstellen einer Renderingsitzung

``` cs
private CreateSessionAsync _pendingAsync = null;
void CreateRenderingSession(AzureFrontend frontend, RenderingSessionVmSize vmSize, ARRTimeSpan maxLease)
{
    _pendingAsync = frontend.CreateNewRenderingSessionAsync(
        new RenderingSessionCreationParams(vmSize, maxLease));

    _pendingAsync.Completed +=
        (CreateSessionAsync res) =>
        {
            if (res.IsRanToCompletion)
            {
                //use res.Result
            }
            else
            {
                Console.WriteLine("Failed to create session!");
            }
            _pendingAsync = null;
        };
}
```

#### <a name="open-an-existing-rendering-session"></a>Öffnen einer vorhandenen Renderingsitzung

Das Öffnen einer vorhandenen Sitzung ist ein synchroner Aufruf.

``` cs
void CreateRenderingSession(AzureFrontend frontend, string sessionId)
{
    AzureSession session = frontend.OpenRenderingSession(sessionId);
    // Query session status, etc.
}
```

#### <a name="get-current-rendering-sessions"></a>Abrufen aktueller Renderingsitzungen

``` cs
private SessionPropertiesArrayAsync _pendingAsync = null;
void GetCurrentRenderingSessions(AzureFrontend frontend)
{
    _pendingAsync = frontend.GetCurrentRenderingSessionsAsync();
    _pendingAsync.Completed +=
        (SessionPropertiesArrayAsync res) =>
        {
            if (res.IsRanToCompletion)
            {
                //use res.Result
            }
            else
            {
                Console.WriteLine("Failed to get current rendering sessions!");
            }
            _pendingAsync = null;
        };
}
```

### <a name="session-apis"></a>Sitzungs-APIs

#### <a name="get-rendering-session-properties"></a>Abrufen von Renderingsitzungseigenschaften

``` cs
private SessionPropertiesAsync _pendingAsync = null;
void GetRenderingSessionProperties(AzureSession session)
{
    _pendingAsync = session.GetPropertiesAsync();
    _pendingAsync.Completed +=
        (SessionPropertiesAsync res) =>
        {
            if (res.IsRanToCompletion)
            {
                //use res.Result
            }
            else
            {
                Console.WriteLine("Failed to get properties of session!");
            }
            _pendingAsync = null;
        };
}
```

#### <a name="update-rendering-session"></a>Aktualisieren einer Renderingsitzung

``` cs
private SessionAsync _pendingAsync;
void UpdateRenderingSession(AzureSession session, ARRTimeSpan updatedLease)
{
    _pendingAsync = session.RenewAsync(
        new RenderingSessionUpdateParams(updatedLease));
    _pendingAsync.Completed +=
        (SessionAsync res) =>
        {
            if (res.IsRanToCompletion)
            {
                Console.WriteLine("Rendering session renewed succeeded!");
            }
            else
            {
                Console.WriteLine("Failed to renew rendering session!");
            }
            _pendingAsync = null;
        };
}
```

#### <a name="stop-rendering-session"></a>Beenden einer Renderingsitzung

``` cs
private SessionAsync _pendingAsync;
void StopRenderingSession(AzureSession session)
{
    _pendingAsync = session.StopAsync();
    _pendingAsync.Completed +=
        (SessionAsync res) =>
        {
            if (res.IsRanToCompletion)
            {
                Console.WriteLine("Rendering session stopped successfully!");
            }
            else
            {
                Console.WriteLine("Failed to stop rendering session!");
            }
            _pendingAsync = null;
        };
}
```

#### <a name="connect-to-arr-inspector"></a>Verbinden mit ARRInspector

``` cs
private ArrInspectorAsync _pendingAsync = null;
void ConnectToArrInspector(AzureSession session, string hostname)
{
    _pendingAsync = session.ConnectToArrInspectorAsync(hostname);
    _pendingAsync.Completed +=
        (ArrInspectorAsync res) =>
        {
            if (res.IsRanToCompletion)
            {
                // Launch the html file with default browser
                string htmlPath = res.Result;
#if WINDOWS_UWP
                UnityEngine.WSA.Application.InvokeOnUIThread(async () =>
                {
                    var file = await Windows.Storage.StorageFile.GetFileFromPathAsync(htmlPath);
                    await Windows.System.Launcher.LaunchFileAsync(file);
                }, true);
#else
                InvokeOnAppThreadAsync(() =>
                {
                    System.Diagnostics.Process.Start("file:///" + htmlPath);
                });
#endif
            }
            else
            {
                Console.WriteLine("Failed to connect to ARR inspector!");
            }
        };
}
```

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen eines Kontos](create-an-account.md)
* [PowerShell-Beispielskripts](../samples/powershell-example-scripts.md)
