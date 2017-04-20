<properties
 pageTitle="Entwicklerhandbuch - Abfragesprache | Microsoft Azure"
 description="Azure IoT Hub-Entwicklerhandbuch - Beschreibung der Abfragesprache zum Abrufen von Informationen im Vergleich, Methoden und Aufträge von IoT hub"
 services="iot-hub"
 documentationCenter=".net"
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="elioda"/>

# <a name="reference---query-language-for-twins-and-jobs"></a>Referenz - Abfragesprache im Vergleich zu Aufträgen

## <a name="overview"></a>Übersicht

IoT Hub bietet eine leistungsstarke SQL-ähnliche Sprache zum Abrufen von Informationen über [Geräte im Vergleich] [ lnk-twins] und [Aufträge][lnk-jobs]. Dieser Artikel erläutert:

* Eine Einführung in die wichtigsten Features der Abfragesprache IoT Hub und
* Die detaillierte Beschreibung der Sprache.

## <a name="getting-started-with-twin-queries"></a>Erste Schritte mit zwei Abfragen

[Gerät im Vergleich] [ lnk-twins] können beliebige JSON-Objekte als Tags und Eigenschaften enthalten. IoT Hub ermöglicht Abfrage Gerät im Vergleich als einzelnes JSON-Dokument mit zwei Informationen.
Angenommen Sie, beispielsweise Ihre IoT Hub im Vergleich haben folgende Struktur:

        {                                                                      
            "deviceId": "myDeviceId",                                            
            "etag": "AAAAAAAAAAc=",                                              
            "tags": {                                                            
                "location": {                                                      
                    "region": "US",                                                  
                    "plant": "Redmond43"                                             
                }                                                                  
            },                                                                   
            "properties": {                                                      
                "desired": {                                                       
                    "telemetryConfig": {                                             
                        "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",            
                        "sendFrequencyInSecs": 300                                          
                    },                                                               
                    "$metadata": {                                                   
                    ...                                                     
                    },                                                               
                    "$version": 4                                                    
                },                                                                 
                "reported": {                                                      
                    "connectivity": {                                                
                        "type": "cellular"                            
                    },                                                               
                    "telemetryConfig": {                                             
                        "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",            
                        "sendFrequencyInSecs": 300,                                         
                        "status": "Success"                                            
                    },                                                               
                    "$metadata": {                                                   
                    ...                                                
                    },                                                               
                    "$version": 7                                                    
                }                                                                  
            }                                                                    
        }

IoT Hub macht das Gerät im Vergleich als Dokumentensammlung **Geräte**.
Die folgende Abfrage ruft so die Gesamtmenge der Geräte im Vergleich:

        SELECT * FROM devices

> [AZURE.NOTE] [IoT Hub SDKs] [ lnk-hub-sdks] unterstützen Paging umfangreicher Ergebnisse.

IoT Hub können im Vergleich mit beliebigen Filtern abrufen. Zum Beispiel

        SELECT * FROM devices
        WHERE tags.location.region = 'US'

Ruft die im Vergleich mit den **location.region** Tag für **uns**.
Boolesche Operatoren und arithmetische Vergleiche, z.B. unterstützt

        SELECT * FROM devices
        WHERE tags.location.region = 'US'
            AND properties.reported.telemetryConfig.sendFrequencyInSecs >= 60

Ruft alle im Vergleich befindet sich in den USA Telemetrie als jede Minute senden konfiguriert. Zur leichteren Handhabung kann auch Konstanten in Verbindung mit **IN** **NIN** (in) Operatoren verwenden. Zum Beispiel

        SELECT * FROM devices
        WHERE property.reported.connectivity IN ['wired', 'wifi']

Ruft alle im Vergleich, die Wifi gemeldet oder verkabelte Verbindung ab. Die [WHERE-Klausel] finden Sie unter[ lnk-query-where] Teil der Filterfunktionen für die vollständige Referenz.

Gruppierung und Aggregationen werden ebenfalls unterstützt. Zum Beispiel

        SELECT properties.reported.telemetryConfig.status AS status,
            COUNT() AS numberOfDevices
        FROM devices
        GROUP BY properties.reported.telemetryConfig.status

Gibt die Anzahl der Geräte in jeder Telemetrie-Konfigurationsstatus zurück.

        [
            {
                "numberOfDevices": 3,
                "status": "Success"
            },
            {
                "numberOfDevices": 2,
                "status": "Pending"
            },
            {
                "numberOfDevices": 1,
                "status": "Error"
            }
        ]

Im Beispiel oben zeigt eine Situation, drei Geräte gemeldet erfolgreichen Konfiguration zwei noch die Konfiguration anwenden und eine hat einen Fehler gemeldet.

### <a name="c-example"></a>C#-Beispiel

Die Funktionalität von [C# Service SDK] verfügbar gemacht wird[ lnk-hub-sdks] in der **RegistryManager** -Klasse.
Hier ist ein Beispiel für eine einfache Abfrage ein:

        var query = registryManager.CreateQuery("SELECT * FROM devices", 100);
        while (query.HasMoreResults)
        {
            var page = await query.GetNextAsTwinAsync();
            foreach (var twin in page) 
            {
                // do work on twin object
            }
        }

Beachten Sie, wie **das Abfrageobjekt** Seitengröße (bis 1000) instanziiert wird und mehrere Seiten können dann durch Aufrufen der **GetNextAsTwinAsync** -Methode mehrmals abgerufen werden.
Es ist zu beachten, dass das Abfrageobjekt mehrere macht **Weiter\***, d. h. je nach der Deserialisierung der Abfrage erforderlichen twin oder Aufgaben Objekte oder einfachen Json Verwendung Projektionen verwendet werden.

### <a name="node-example"></a>Knoten-Beispiel

Die Abfragefunktion vom [Knoten Service SDK] bereitgestellten[ lnk-hub-sdks] in das **Registrierungsobjekt** .
Hier ist ein Beispiel für eine einfache Abfrage ein:

        var query = registry.createQuery('SELECT * FROM devices', 100);
        var onResults = function(err, results) {
            if (err) {
                console.error('Failed to fetch the results: ' + err.message);
            } else {
                // Do something with the results
                results.forEach(function(twin) {
                    console.log(twin.deviceId);
                });

                if (query.hasMoreResults) {
                    query.nextAsTwin(onResults);
                }
            }
        };
        query.nextAsTwin(onResults);

Beachten Sie, wie **das Abfrageobjekt** Seitengröße (bis 1000) instanziiert wird und mehrere Seiten können dann durch Aufrufen der **NextAsTwin** -Methode mehrmals abgerufen werden.
Es ist zu beachten, dass das Abfrageobjekt mehrere macht **Weiter\***, d. h. je nach der Deserialisierung der Abfrage erforderlichen twin oder Aufgaben Objekte oder einfachen Json Verwendung Projektionen verwendet werden.

### <a name="limitations"></a>Grenzen

Derzeit Projektionen werden nur unterstützt, wenn Aggregationen verwenden, d. h. nicht aggregierte Abfragen können nur `SELECT *`. Auch Aggregation werden nur unterstützt zusammen gruppieren.

## <a name="getting-started-with-jobs-queries"></a>Erste Schritte mit Aufträgen Abfragen

[Aufträge] [ lnk-jobs] bieten eine Möglichkeit zum Ausführen von Vorgängen auf Geräte. Jedes Gerät und Informationen die Aufträge der Sammlung **Aufträge**an ist.
Logisch

        {                                                                      
            "deviceId": "myDeviceId",                                            
            "etag": "AAAAAAAAAAc=",                                              
            "tags": {                                                            
                ...                                                              
            },                                                                   
            "properties": {                                                      
                ...                                                                 
            },
            "jobs": [
                { 
                    "deviceId": "myDeviceId",
                    "jobId": "myJobId",    
                    "jobType": "scheduleTwinUpdate",            
                    "status": "completed",                    
                    "startTimeUtc": "2016-09-29T18:18:52.7418462",
                    "endTimeUtc": "2016-09-29T18:20:52.7418462",
                    "createdDateTimeUtc": "2016-09-29T18:18:56.7787107Z",
                    "lastUpdatedDateTimeUtc": "2016-09-29T18:18:56.8894408Z",
                    "outcome": {
                        "deviceMethodResponse": null   
                    }                                         
                },
                ...
            ]                                                             
        }

Diese Auflistung ist als **devices.jobs** in der Abfragesprache IoT Hub abfragbare.

Beispielsweise können Sie zu alle Aufträge (vergangenen und geplanten), die ein einzelnes Gerät beeinflussen, die folgende Abfrage verwenden:

        SELECT * FROM devices.jobs
        WHERE devices.jobs.deviceId = 'myDeviceId'

Beachten Sie, wie diese Abfrage gerätespezifische Status und möglicherweise die Antwort direkte Methode Auftrags zurückgegeben bereitstellt.
Es ist möglich, mit beliebigen booleschen alle Eigenschaften der Objekte in der **devices.jobs** -Auflistung.
Beispielsweise die folgende Abfrage:

        SELECT * FROM devices.jobs
        WHERE devices.jobs.deviceId = 'myDeviceId'
            AND devices.jobs.jobType = 'scheduleTwinUpdate'
            AND devices.jobs.status = 'completed'
            AND devices.jobs.createdTimeUtc > '2016-09-01'

Ruft alle abgeschlossenen und Aktualisierungsvorgänge für Gerät **MyDeviceId** , die nach September 2016 erstellt wurden.

Es kann auch pro Gerät Ergebnisse eines Projekts abrufen.

        SELECT * FROM devices.jobs
        WHERE devices.jobs.jobId = 'myJobId'

### <a name="limitations"></a>Grenzen
Abfragen zu **devices.jobs** können derzeit nicht unterstützt werden:

* Prognosen, d.h. `SELECT *` möglich ist;
* Regeln, die zu dem Gerät neben Auftragseigenschaften obigen verweisen.
* Unterschiedlichsten Aggregationen, z. B. Count, Avg, gruppieren.

## <a name="basics-of-an-iot-hub-query"></a>Grundlagen einer Abfrage IoT Hub

Jede Abfrage IoT Hub besteht aus einer SELECT und FROM-Klauseln und optionale WHERE und GROUP BY Klauseln. Jede Abfrage wird auf eine Auflistung von JSON-Dokumente, z.B. Gerät im Vergleich ausgeführt. Die FROM-Klausel gibt die Dokumentgruppe (**Geräte** oder **devices.jobs**) durchlaufen werden. Dann wird der Filter in der WHERE-Klausel angewendet. Bei Aggregationen, die Ergebnisse dieses Schritts werden als gruppiert in der GROUP BY-Klausel und für jede Gruppe angegeben, wird eine Zeile generiert in der SELECT-Klausel angegeben.

        SELECT <select_list>
        FROM <from_specification>
        [WHERE <filter_condition>]
        [GROUP BY <group_specification>]

## <a name="from-clause"></a>FROM-Klausel

**Von < From_specification >** -Klausel kann nur zwei Werte annehmen: **von Geräten**zu Abfrage Gerät im Vergleich **von devices.jobs**auf Abfrage pro Gerät Auftragsdetails.

## <a name="where-clause"></a>WHERE-Klausel

Die **, < Filter_condition >** -Klausel ist optional. Es gibt die Bedingung(en), die JSON-Dokumente in der Auflistung von erfüllen muss, um das Ergebnis enthalten sein. Ein JSON-Dokument muss der angegebenen Bedingung "true" in das Ergebnis eingeschlossen werden ausgewertet.

Zulässigen Vorschriften gemäß Abschnitt [Ausdrücke und Geschäftsbedingungen][lnk-query-expressions].

## <a name="select-clause"></a>SELECT-Klausel

Die SELECT-Klausel (**< Select_list > auswählen**) ist obligatorisch und gibt an, welche Werte werden von der Abfrage abgerufen. Es gibt die JSON-Werte, neue JSON-Objekte generiert für jedes Element der gefilterten (und optional gruppierte) Teil der Auflistung von der Projektionsphase generiert ein JSON-Objekt mit den Angaben in der SELECT-Klausel erstellt.

Dies ist die Grammatik der SELECT-Klausel:

        SELECT [TOP <max number>] <projection list>

        <projection_list> ::=
            '*'
            | <projection_element> AS alias [, <projection_element> AS alias]+

        <projection_element> :==
            attribute_name
            | <projection_element> '.' attribute_name
            | <aggregate>

        <aggregate> :==
            count(<projection_element>) | count()
            | avg(<projection_element>) | avg()
            | sum(<projection_element>) | sum()
            | min(<projection_element>) | min()
            | max(<projection_element>) | max()

Jede Eigenschaft in der Auflistung von JSON-Dokument, wobei **Attribute_name** bezeichnet. Beispiele für SELECT-Klauseln finden Sie unter [Erste Schritte mit zwei Abfragen] [ lnk-query-getstarted] Abschnitt.

Derzeit Auswahl Klauseln anders als **auswählen \* ** nur in Aggregatabfragen im Vergleich unterstützt.

## <a name="group-by-clause"></a>GROUP BY-Klausel

Die **Gruppe von < Group_specification >** -Klausel ist ein optionaler Schritt, der nach dem Filter in der WHERE-Klausel und vor der Projektion angegeben aus ausgeführt werden kann. Dokumente basierend auf dem Wert eines Attributs gruppiert. Diese Gruppen werden zu aggregierten Werte in der SELECT-Klausel verwendet.

Ein Beispiel für eine Abfrage mit GROUP BY ist:

        SELECT properties.reported.telemetryConfig.status AS status,
            COUNT() AS numberOfDevices
        FROM devices
        GROUP BY properties.reported.telemetryConfig.status

GRUPPIEREN nach die formale Syntax lautet:

        GROUP BY <group_by_element>
        <group_by_element> :==
            attribute_name
            | < group_by_element > '.' attribute_name

Jede Eigenschaft in der Auflistung von JSON-Dokument, wobei **Attribute_name** bezeichnet. 

Die GROUP BY-Klausel wird zurzeit nur unterstützt, wenn Abfragen im Vergleich.

## <a name="expressions-and-conditions"></a>Ausdrücke und Geschäftsbedingungen

Auf einer hohen Ebene *Ausdruck*:

* Ergibt eine Instanz eines Typs JSON (d. h. Boolean, Zahl, Zeichenfolge, Array oder Objekt), und
* Wird durch Bearbeiten von Daten vom Gerät JSON-Dokument und Konstanten integrierten Operatoren und Funktionen definiert.

*Sind Ausdrücke, die einen booleschen Wert, d.h. eine Konstante anders als booleschen Wert **true** , **false** (einschließlich **null**, **undefined**, Object oder Array-Instanz, eine beliebige Zeichenfolge und deutlich die booleschen **false gilt**) ergibt.*

Die Syntax für Ausdrücke lautet:

        <expression> ::=
            <constant> |
            attribute_name |
            unary_operator <expression> |
            <expression> binary_operator <expression> |
            <create_array_expression> |
            '(' <expression> ')'

        <constant> ::=
            <undefined_constant>
            | <null_constant> 
            | <number_constant> 
            | <string_constant> 
            | <array_constant> 

        <undefined_constant> ::= undefined
        <null_constant> ::= null
        <number_constant> ::= decimal_literal | hexadecimal_literal
        <string_constant> ::= string_literal
        <array_constant> ::= '[' <constant> [, <constant>]+ ']'

Wo:

| Symbol | Definition |
| -------------- | -----------------|
| Attributname | Jede Eigenschaft in der Auflistung von JSON-Dokument. |
| UNARY_OPERATOR | Unäre Operatoren gemäß Abschnitt Operatoren. |
| binary_operator | Alle binären Operator gemäß Abschnitt Operatoren. |
| decimal_literal | Ein Float in dezimaler Schreibweise ausgedrückt. |
| hexadecimal_literal | Eine Zahl, durch die Zeichenfolge "0 X" gefolgt von einer Zeichenfolge von Hexadezimalziffern ausgedrückt. |
| string_literal | Zeichenfolgenliterale werden durch eine Folge von NULL oder mehr Unicode-Zeichen oder Escapesequenzen dargestellt Unicode-Zeichenfolgen. Zeichenfolgenliterale werden in einfache Anführungszeichen eingeschlossen (Apostroph: ') oder Anführungszeichen (Anführungszeichen ein: "). Escapes zulässig: `\'`, `\"`, `\\`, `\uXXXX` für Unicode-Zeichen durch 4 Hexadezimalzahlen definiert. |

### <a name="operators"></a>Operatoren

Die folgenden Operatoren werden unterstützt:

| Familie | Operatoren |
| -------------- | -----------------|
| Arithmetische Operationen | +,-,*,/,% |
| Logische | UND, ODER, NICHT |
| Vergleich |  =,! =, <>,, < =, > = <> |

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Ausführen von Abfragen in Ihren apps [IoT Hub SDKs]mit[lnk-hub-sdks].

[lnk-query-where]: iot-hub-devguide-query-language.md#where-clause
[lnk-query-expressions]: iot-hub-devguide-query-language.md#expressions-and-conditions
[lnk-query-getstarted]: iot-hub-devguide-query-language.md#getting-started-with-twin-queries

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md