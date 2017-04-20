<properties
    pageTitle="Azure Media Services Fehlercodes | Microsoft Azure"
    description="Das Thema Überblick Azure Media Services-Fehlercodes."
    authors="Juliako"
    manager="erikre"
    editor=""
    services="media-services"
    documentationCenter=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016" 
    ms.author="juliako"/>

# <a name="azure-media-services-error-codes"></a>Azure Media Services-Fehlercodes

Bei Microsoft Azure Media Services erhalten Sie HTTP-Fehlercodes aus dem Dienst je nach Themen Authentifizierungstokens ablaufenden Aktionen in Media Services nicht unterstützt. Folgendes ist eine Liste der **http-Fehlercodes** , die von Media Services und die möglichen Ursachen für diese zurückgegeben werden kann.  
  
## <a name="400-bad-request"></a>400 fehlerhafte Anforderung.

Die Anforderung enthält ungültige Daten und ist aus folgenden Gründen abgelehnt:

- Eine nicht unterstützte API-Version angegeben. Die aktuelle Version finden Sie unter [Setup für Media Services REST API-Entwicklung](media-services-rest-how-to-use.md).
- API-Version von Media Services wurde nicht angegeben. Informationen an die API-Version finden Sie unter [Herstellen einer Verbindung mit Media Services mit Media Services REST-API](media-services-rest-connect-programmatically.md). 
   
    >[AZURE.NOTE] Wenn Sie .NET oder Java SDKs für die Verbindung mit Media Services verwenden, wird die API-Version vorgegeben, versuchen und Maßnahmen gegen Media Services ausführen.
- Eine nicht definierte Eigenschaft es wurde angegeben. Der Eigenschaftenname ist in der Fehlermeldung. Nur diejenigen Eigenschaften, die eine bestimmte Entität gehören können angegeben werden. Eine Liste der Entitäten und deren Eigenschaften finden Sie unter [Azure Media Services REST-API-Referenz](http://msdn.microsoft.com/library/azure/hh973617.aspx) .
- Ein ungültiger Eigenschaftswert wurde angegeben. Der Eigenschaftenname ist in der Fehlermeldung. Den vorherigen Link für gültige und ihre Werte anzeigen
- Ein Eigenschaftswert fehlt und ist erforderlich.
- Teil der angegebenen URL enthält einen ungültigen Wert.
- Versuch, WriteOnce-Eigenschaft zu aktualisieren.
- Wurde versucht, einen Auftrag erstellen, input Vermögenswert mit primären AssetFile die wurde nicht angegeben oder konnte nicht ermittelt werden.
- Versuch ein SAS-Locator aktualisieren. SAS-Locators können nur erstellt oder gelöscht werden. Streaming-Locators kann aktualisiert werden. Weitere Informationen finden Sie unter [Locators](http://msdn.microsoft.com/library/azure/hh974308.aspx).
- Ein nicht unterstützter Vorgang oder die Abfrage wurde übermittelt. 

## <a name="401-unauthorized"></a>401 nicht autorisiert

Die Anforderung konnte nicht authentifiziert werden (bevor sie autorisiert werden kann) die folgenden Gründe:

- Fehlende Authentifizierungsheader.
- Ungültiger Wert.
    - Das Token ist abgelaufen. Wenn die REST-APIs direkt verwenden, finden Sie unter [Herstellen einer Verbindung mit Media Services mit Media Services REST API](media-services-rest-connect_programmatically.md) Informationen zum Generieren einer neuen Authentifizierungstoken. Wenn Sie .NET oder Java SDKs verwenden, erstellen Sie ein Objekt CloudMediaContext oder MediaContract um das Token zu generieren. Weitere Informationen hierzu finden Sie unter [Herstellen einer Verbindung mit Media Services mit Media Services SDK für .NET](media-services-dotnet-connect-programmatically.md).
    - Das Token enthält eine ungültige Signatur.</li></ul></li></ul>

## <a name="403-forbidden"></a>403 Forbidden

Die Anforderung ist aus folgenden Gründen nicht zulässig:

- Media Services-Konto kann nicht gefunden werden oder wurde gelöscht.
- Media Services-Konto ist deaktiviert, und der Anforderungstyp ist nicht HTTP GET. Dienstvorgänge werden sowie eine 403 Antwort zurückgeben.
- Das Authentifizierungstoken enthält keine Anmeldeinformationen des Benutzers: Kontoname bzw. SubscriptionId. Diese Informationen finden in der Media-Benutzeroberflächenerweiterung für Ihr Konto Media Services in Azure-Verwaltungsportal Sie.
- Die Ressource kann nicht zugegriffen werden.
    - Es wurde versucht, eine MediaProcessor zu verwenden, die für Ihr Konto Media Services nicht verfügbar ist.
    - Versuch eine Auftragsvorlage Media Services entsprechend aktualisieren.
    - Es wurde versucht, einige andere Media Services Firma Locator überschreiben.
    - Es wurde versucht, einige andere Media Services-Konto des ContentKey überschrieben.

- Die Ressource konnte nicht durch ein Service-Kontingent erstellt werden, die für das Media Services-Konto erreicht wurde. Weitere Informationen zu Kontingenten Service finden Sie unter [Kontingente und Einschränkungen](media-services-quotas-and-limitations.md).

## <a name="404-not-found"></a>404 nicht gefunden

Die Anforderung darf nicht auf eine Ressource aus einem der folgenden Gründe:

- Versuch, eine Entität aktualisieren, die nicht vorhanden ist.
- Es wurde versucht, ein Element zu löschen, die nicht vorhanden ist.
- Versuch, eine Entität mit einem Link zu einer Entität erstellen, die nicht vorhanden ist.
- Es wurde versucht, zu einer Entität, die nicht vorhanden ist.
- Es wurde versucht, ein Speicherkonto angeben, die nicht mit Media Services-Konto verknüpft ist.  

## <a name="409-conflict"></a>409 conflict

Die Anforderung ist aus folgenden Gründen nicht zulässig:

- Mehrere AssetFile hat den angegebenen Namen in der Anlage.
- Versuch einer zweiten primären AssetFile in der Anlage zu erstellen.
- Versuch einer ContentKey mit der angegebenen Id bereits erstellt.
- Versuch mit der angegebenen Id bereits einen Locator erstellen.
- Mehrere IngestManifestFile hat den angegebenen Namen in der IngestManifest.
- Es wurde versucht, eine zweite speicherverschlüsselung ContentKey Speicher verschlüsselt Anlage verknüpfen.
- Es wurde versucht, dieselbe ContentKey Anlage verknüpfen.
- Versuch einen Locator für eine Anlage erstellt, deren Behälter fehlt oder ist nicht mehr mit der Anlage.
- Versuch einen Locator für eine Anlage erstellen bereits 5 Locators verwendet. (Azure-Speicher erzwingt den Grenzwert von fünf Richtlinien für gemeinsamen Zugriff auf einen Speichercontainer.)
- Speicherkonto einer Anlage mit einem IngestManifestAsset verknüpfen ist nicht vom übergeordneten IngestManifest verwendet Storage-Konto.  

## <a name="500-internal-server-error"></a>500 Interner Serverfehler

Media Services findet während der Verarbeitung der Anforderung einen Fehler, der verhindert, die Verarbeitung fortgesetzt dass. Dies kann einen der folgenden Gründe haben:

- Erstellen einer Anlage oder Auftrag schlägt fehl, weil Media Services Account Service Kontingentinformationen vorübergehend nicht verfügbar.
- Erstellen einer Anlage oder IngestManifest BLOB-Speichercontainer schlägt fehl, weil das Konto speicherkontoinformationen vorübergehend nicht verfügbar.
- Sonstige unerwartete Fehler. 

## <a name="503-service-unavailable"></a>503 Dienst nicht verfügbar

Der Server ist zurzeit nicht auf Anfragen. Dieser Fehler kann durch übermäßige Abfragen an den Dienst. Media Services Drosselungsmechanismus schränkt die Ressourcenverwendung für Programme, die den Dienst übermäßige anfordern.

>[AZURE.NOTE]Überprüfen Sie die Fehlermeldung und Fehlercodezeichenfolge, um ausführliche Informationen über die Ursache der Fehler 503 habe. Dieser Fehler bedeutet nicht immer Drosselung.

Mögliche Status sind:

- "Der Server ist ausgelastet. Früher für diese Art von Anforderung dauerte länger als {0} Sekunden."
- "Der Server ist ausgelastet. Mehr als {0} Anfragen pro Sekunde können eingeschränkt."
- "Der Server ist ausgelastet. Mehr als {0} Anfragen innerhalb von {1} Sekunden gedrosselt werden können."

Zur Behandlung dieses Fehlers empfiehlt exponentielle Backoff-Wiederholungslogik. Das heißt mit zunehmend länger wartet zwischen Wiederholungsversuchen für aufeinander folgende Fehlerantworten.  Weitere Informationen finden Sie unter [Transiente Fehler Handling Application Block](https://msdn.microsoft.com/library/hh680905.aspx). 

>[AZURE.NOTE]Bei Verwendung von [Azure Media Services SDK für .net](https://github.com/Azure/azure-sdk-for-media-services/tree/master)wurde die Wiederholungslogik Fehler 503 vom SDK implementiert.  
  
## <a name="see-also"></a>Siehe auch  

[Medien-Management-Fehlercodes](http://msdn.microsoft.com/library/windowsazure/dn167016.aspx)

## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
