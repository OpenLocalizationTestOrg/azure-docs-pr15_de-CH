<properties
    pageTitle="Microsoft Translator Logik Apps hinzufügen | Microsoft Azure"
    description="Übersicht über Microsoft Translator Connector mit REST-API"
    services=""
    suite=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="08/18/2016"
   ms.author="mandia"/>

# <a name="get-started-with-the-microsoft-translator-connector"></a>Erste Schritte mit Microsoft Translator connector
Verbinden Sie mit Microsoft Translator übersetzen Text, Sprache und mehr erkannt. Mit Microsoft Translator können Sie: 

- Erstellen Sie Ihr Geschäftsablauf basierend auf den Daten von Microsoft Translator erhalten. 
- Verwenden Sie Aktionen zum Übersetzen von Text, Sprache und mehr erkannt. Diese Aktionen eine Antwort erhalten und dann die Ausgabe für andere Aktionen verfügbar machen. Beim Erstellen eine neue Datei in Ablage können Sie den Text in der Datei in einer anderen Sprache mit Microsoft Translator übersetzen.

Ein Vorgang in Logik-apps finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen
Microsoft Translator umfasst die folgenden Aktionen. Es gibt keine Trigger.

Trigger | Aktionen
--- | ---
Keine | <ul><li>Sprache erkennen</li><li>Sprachausgabe</li><li>Text übersetzen</li><li>Abrufen von Sprachen</li><li>Sprache Sprachen erhalten</li></ul>

Alle Connectors unterstützen in JSON und XML-Daten.


## <a name="create-a-connection-to-microsoft-translator"></a>Erstellen einer Verbindung mit Microsoft Translator

>[AZURE.INCLUDE [Steps to create a connection to Microsoft Translator](../../includes/connectors-create-api-microsofttranslator.md)]


## <a name="swagger-rest-api-reference"></a>Stolz REST-API-Referenz
Gilt für Version: 1.0.

### <a name="detect-language"></a>Sprache erkennen    
Erkennt die Ausgangssprache des angegebenen Text.  
```GET: /Detect```

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Abfrage|Zeichenfolge|Ja|Abfrage|keine |Text, dessen Sprache identifiziert werden|

#### <a name="response"></a>Antwort
|Name|Beschreibung|
|---|---|
|200|Okay|
|Standard|Vorgang ist fehlgeschlagen.|


### <a name="text-to-speech"></a>Sprachausgabe    
Konvertiert einen angegebenen Text in Sprache als Audiostream in Wave-Format.  
```GET: /Speak```

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Abfrage|Zeichenfolge|Ja|Abfrage|keine |Text konvertieren|
|Sprache|Zeichenfolge|Ja|Abfrage|keine |Sprachcode Sprache generiert (Beispiel: "En-us)|

#### <a name="response"></a>Antwort
|Name|Beschreibung|
|---|---|
|200|Okay|
|Standard|Vorgang ist fehlgeschlagen.|


### <a name="translate-text"></a>Text übersetzen    
Text in einer bestimmten Sprache mit Microsoft Translator übersetzt.  
```GET: /Translate```

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Abfrage|Zeichenfolge|Ja|Abfrage|keine |Zu übersetzender Text|
|languageTo|Zeichenfolge|Ja|Abfrage| keine|Zielsprache Code (Beispiel: "fr")|
|languageFrom|Zeichenfolge|Nein|Abfrage|keine |Ausgangssprache; Wenn nicht angegeben, versucht Microsoft Translator automatisch erkennen. (Beispiel: En)|
|Kategorie|Zeichenfolge|Nein|Abfrage|Allgemein |Übersetzung Kategorie (Standard: "Allgemein")|

#### <a name="response"></a>Antwort
|Name|Beschreibung|
|---|---|
|200|Okay|
|Standard|Vorgang ist fehlgeschlagen.|


### <a name="get-languages"></a>Abrufen von Sprachen    
Ruft alle Sprachen, die Microsoft Translator unterstützt.  
```GET: /TranslatableLanguages```

Es sind keine Parameter für diesen Aufruf. 

#### <a name="response"></a>Antwort
|Name|Beschreibung|
|---|---|
|200|Okay|
|Standard|Vorgang ist fehlgeschlagen.|


### <a name="get-speech-languages"></a>Sprache Sprachen erhalten    
Ruft die Sprachen für die Sprachsynthese.  
```GET: /SpeakLanguages``` 

Es sind keine Parameter für diesen Aufruf.

#### <a name="response"></a>Antwort
|Name|Beschreibung|
|---|---|
|200|Okay|
|Standard|Vorgang ist fehlgeschlagen.|

## <a name="object-definitions"></a>Objektdefinitionen

#### <a name="language-language-model-for-microsoft-translator-translatable-languages"></a>Sprache: Language-Modell übersetzt bei Microsoft Translator

|Eigenschaftenname | Datentyp | Erforderlich|
|---|---|---|
|Code|Zeichenfolge|Nein|
|Name|Zeichenfolge|Nein|


## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer Anwendung Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

Zurück zur [Liste der APIs](apis-list.md).


<!--References-->
[5]: https://datamarket.azure.com/developer/applications/
[6]: ./media/connectors-create-api-microsofttranslator/register-your-application.png
