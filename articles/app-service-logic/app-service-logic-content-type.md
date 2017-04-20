<properties
   pageTitle="Logik apps Inhalt geben Behandlung | Microsoft Azure"
   description="Verstehen Sie, wie Logik Apps Inhaltstypen zur Entwurfs- und Laufzeit behandelt"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="logic-apps-content-type-handling"></a>Logik Apps Inhaltstyp Behandlung

Es gibt viele verschiedene Arten von Inhalten, die durch eine Anwendung Logik - einschließlich JSON, XML-Dateien und binäre Daten fließen können.  Während alle Inhaltstypen unterstützt, einige werden direkt vom Modul Apps Logik verstanden und andere Umwandlung oder Konvertierung Bedarf erfordern.  Im folgende Artikel wird beschrieben, wie das Modul verschiedene Inhaltstypen behandelt und wie sie ordnungsgemäß behandelt werden können wie erforderlich.

## <a name="content-type-header"></a>Content-Type-Header

Starten Sie einfach, betrachten wir die beiden `Content-Types` keine Konvertierung oder Umwandlung innerhalb einer Anwendung Logik - erfordern `application/json` und `text/plain`.

### <a name="applicationjson"></a>Anwendung/json

Das Workflow-System basiert auf der `Content-Type` von HTTP Header bestimmen die geeignete Behandlung aufruft.  Jede Anforderung mit dem Inhaltstyp `application/json` gespeichert und als JSON-Objekt behandelt werden.  Darüber hinaus kann JSON Inhalt standardmäßig analysiert werden, ohne Umwandlung.  Damit eine Anforderung mit der Content-Type-Header `application/json ` wie folgt:

```
{
    "data": "a",
    "foo": [
        "bar"
    ]
}
```

kann in einem Workflow mit Ausdruck analysiert `@body('myAction')['foo'][0]` auf einen Wert (in diesem Fall `bar`).  Keine weitere Umwandlung ist erforderlich.  Wenn Sie mit Daten, das JSON aber keinen Header angegeben arbeiten, Sie können manuell Umwandlung in JSON mit dem `@json()` Funktion (Beispiel: `@json(triggerBody())['foo']`).

### <a name="textplain"></a>Text/plain

Ähnlich wie `application/json`, HTTP-Nachrichten mit den `Content-Type` Header `text/plain` in seiner Rohform gespeichert.  Außerdem, wenn in nachfolgenden Aktionen ohne Umwandlung die Anforderung erlischt mit einem `Content-Type`: `text/plain` Header.  Beispielsweise wenn eine Flatfile erhalten folgenden HTTP-Inhalt Sie:

```
Date,Name,Address
Oct-1,Frank,123 Ave.
```

as `text/plain`.  In der nächsten Aktion Sie es als Teil einer anderen Anforderung gesendet (`@body('flatfile')`), normalerweise eine `text/plain` Content-Type-Header.  Wenn Sie mit Daten, das nur-Text aber keinen Header angegeben arbeiten, Sie können manuell Umwandlung in Text mit der `@string()` Funktion (Beispiel: `@string(triggerBody())`)

### <a name="applicationxml-and-applicationoctet-stream-and-converter-functions"></a>Anwendung/Xml und Application/Octet-Stream und Konverter Funktionen

Logik App Engine behält immer die `Content-Type` für die HTTP-Anforderung oder die Antwort empfangen wurde.  Daher ist ein Inhalts eingeht `Content-Type` von `application/octet-stream`, einschließlich Folgemaßnahmen mit keine führt zu einer ausgehenden Anforderung mit `Content-Type`: `application/octet-stream`.  Auf diese Weise das Modul können Guaruntee Daten nicht verloren, wie im Workflow verschoben.  Jedoch Aktionszustand (Eingaben und Ausgaben) befinden sich in einem JSON-Objekt im Workflow fließt.  Dies bedeutet um einige Datentypen zu erhalten, das Modul konvertiert den Inhalt in ein binary base64-codierte Zeichenfolge mit entsprechenden Metadaten, die beide behält `$content` und `$content-type` -die automatisch konvertiert werden.  Sie können auch manuell konvertieren zwischen Inhaltstypen mit integrierten Typkonverter funktioniert:

* `@json()`-Daten umwandelt`application/json`
* `@xml()`-Daten umwandelt`application/xml`
* `@binary()`-Daten umwandelt`application/octet-stream`
* `@string()`-Daten umwandelt`text/plain`
* `@base64()`-Inhalte in eine base64-Zeichenfolge konvertiert
* `@base64toString()`-eine base64-codierte Zeichenfolge konvertiert`text/plain`
* `@base64toBinary()`-eine base64-codierte Zeichenfolge konvertiert`application/octet-stream`
* `@encodeDataUri()`-codiert eine Zeichenfolge als Bytearray DataUri
* `@decodeDataUri()`-ein DataUri in einem Bytearray decodiert

Beispielsweise erhalten eine HTTP-Anforderung mit `Content-Type`: `application/xml` von:

```
<?xml version="1.0" encoding="UTF-8" ?>
<CustomerName>Frank</CustomerName>
```

Ich konnte und später mit `@xml(triggerBody())`, oder in einer Funktion wie `@xpath(xml(triggerBody()), '/CustomerName')`.

### <a name="other-content-types"></a>Andere Inhaltstypen

Andere Inhaltstypen werden unterstützt und Arbeiten mit einer Anwendung Logik jedoch möglicherweise manuell abrufen Nachrichtentext Decodieren der `$content`.  Angenommen, ich wurden von Auslösen einer `application/x-www-url-formencoded` wie folgende Anforderung:

```
CustomerName=Frank&Address=123+Avenue
```

Da dies nicht nur Text oder JSON in Aktion als gespeichert:

```
...
"body": {
    "$content-type": "application/x-www-url-formencoded",
    "$content": "AAB1241BACDFA=="
}
```

Wo `$content` ist die Nutzlast als base64-Zeichenfolge alle Daten erhalten.  Da derzeit keine systemeigene Funktion für Formulardaten, kann weiterhin verwendet werden diese Daten in einem Workflow manuell Datenzugriff mit einer Funktion wie `@string(body('formdataAction'))`.  Wenn ich meine ausgehenden Anforderung auch wollte die `application/x-www-url-formencoded` Content-Type-Header kann nur hinzugefügt sie Aktionskörper ohne Umwandlung wie `@body('formdataAction')`.  Aber dies funktioniert nur wenn der einzige Parameter in ist der `body` input.  Wenn Sie versuchen, `@body('formdataAction')` in einem `application/json` anfordern, erhalten Sie einen Laufzeitfehler codierten Text senden wird.
