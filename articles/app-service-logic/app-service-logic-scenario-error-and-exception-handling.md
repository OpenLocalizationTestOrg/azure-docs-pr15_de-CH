<properties
    pageTitle="Protokollierung und Fehlerbehandlung in Logik-Apps | Microsoft Azure"
    description="Zeigen Sie einen realen Anwendungsfall erweiterten Fehler Behandlung und Protokollierung mit Logik Apps an"
    keywords=""
    services="logic-apps"
    authors="hedidin"
    manager="anneta"
    editor=""
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="b-hoedid"/>

# <a name="logging-and-error-handling-in-logic-apps"></a>Protokollierung und Fehlerbehandlung in Logik-Apps

Dieser Artikel beschreibt, wie eine Anwendung Logik zur besseren Unterstützung der Ausnahmebehandlung erweitern können. Es ist einem realen Anwendungsfall und unsere Antwort auf die Frage, "Logik Apps Ausnahme- und Fehlerbehandlung unterstützt?"

>[AZURE.NOTE]Die aktuelle Version von Logik-Apps-Feature von Microsoft Azure App Service bietet eine Standardvorlage für Aktion Antworten.
>Dazu gehören interne Validierung und Fehlerantworten von API-app zurückgegeben.

## <a name="overview-of-the-use-case-and-scenario"></a>Übersicht über die Verwendung und Szenario

Geschichte ist bei Verwendung dieses Artikels.
Einem bekannten Unternehmen beschäftigt Azure-Lösung entwickeln, die mit Microsoft Dynamics CRM Online Patienten Portal erstellen würden. Termin Datensätze zwischen dem Dynamics CRM Online Patienten Portal und Salesforce werden mussten.  Wir mussten die [HL7-FHIR](http://www.hl7.org/implement/standards/fhir/) für alle Datensätze verwendet.

Das Projekt hat zwei wichtige Vorschriften:  

 -  Eine Methode zum aufgezeichnet von Dynamics CRM Online Portal gesendet
 -  Fehler angezeigt, die innerhalb des Workflows


## <a name="how-we-solved-the-problem"></a>Wie wir das Problem gelöst

>[AZURE.TIP] Sie können allgemeine Video des Projekts [Benutzergruppe Integration](http://www.integrationusergroup.com/do-logic-apps-support-error-handling/ "Integration Benutzergruppe")anzeigen.

Wir haben [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/ "Azure DocumentDB") als Repository für (DocumentDB bezieht sich auf Dokumente Datensätze) und Datensätze. Da Logik Programme eine Standardvorlage für Antworten hat, würden wir kein benutzerdefiniertes Schema erstellen. Eine API-app **und **Abfrage** ** Fehler und Protokolldatei konnte erstellt werden. Ein Schema kann auch für jede API-App definiert.  

Weitere wurde Datensätze nach einem bestimmten Datum gelöscht. DocumentDB enthält eine Eigenschaft namens [Gültigkeitsdauer](https://azure.microsoft.com/blog/documentdb-now-supports-time-to-live-ttl/ "Gültigkeitsdauer") (TTL), erlaubt uns eine **Gültigkeitsdauer** für jeden Datensatz oder Auflistung festgelegt. Dies eliminiert die Notwendigkeit zum Löschen von Datensätzen in DocumentDB.

### <a name="creation-of-the-logic-app"></a>Erstellung der Logik-app

Der erste Schritt ist zu Logik app im Designer geladen. In diesem Beispiel verwenden wir hierarchische Logik apps. Angenommen, wir bereits das übergeordnete haben und untergeordnete Logik app erstellen.

Da wir aus Dynamics CRM Online Datensatz anmelden möchten, beginnen Sie oben. Wir müssen einen Anforderung Trigger verwenden, da übergeordnete Logik app dieses untergeordnete Element ausgelöst.

> [AZURE.IMPORTANT] Um dieses Lernprogramm müssen Sie zum Erstellen einer DocumentDB und zwei Sammlungen (Protokollierung und Fehler).

### <a name="logic-app-trigger"></a>Logik app trigger

Wir verwenden einen Anforderung Trigger wie im folgenden Beispiel gezeigt.

```` json
"triggers": {
        "request": {
          "type": "request",
          "kind": "http",
          "inputs": {
            "schema": {
              "properties": {
                "CRMid": {
                  "type": "string"
                },
                "recordType": {
                  "type": "string"
                },
                "salesforceID": {
                  "type": "string"
                },
                "update": {
                  "type": "boolean"
                }
              },
              "required": [
                "CRMid",
                "recordType",
                "salesforceID",
                "update"
              ],
              "type": "object"
            }
          }
        }
      },

````


### <a name="steps"></a>Schritte

Wir müssen der Datenquelle (Anforderung) der Patienten von Dynamics CRM Online-Portal anmelden.

1. Wir benötigen eine neue Termin von Dynamics CRM Online.
    Der Trigger von CRM bietet die **CRM-PatentId**, **Datensatz** **neu oder aktualisiert** (neues oder booleschen Wert aktualisieren), und **SalesforceId**. Die **SalesforceId** kann null sein, da sie nur für eine Aktualisierung verwendet wird.
    CRM-Datensatz erhalten mit CRM **Patienten-ID** und den **Datensatztyp**.
1. Als Nächstes müssen wir DocumentDB API-app **InsertLogEntry** Vorgang hinzufügen, wie in der folgenden Abbildung dargestellt.


#### <a name="insert-log-entry-designer-view"></a>Protokoll Eintrag Entwurfsansicht einfügen

![Eintrag einfügen](./media/app-service-logic-scenario-error-and-exception-handling/lognewpatient.png)

#### <a name="insert-error-entry-designer-view"></a>Fehler Eintrag Entwurfsansicht einfügen
![Eintrag einfügen](./media/app-service-logic-scenario-error-and-exception-handling/insertlogentry.png)

#### <a name="check-for-create-record-failure"></a>Kontrollkästchen für Datensatz Fehler beim Erstellen

![Bedingung](./media/app-service-logic-scenario-error-and-exception-handling/condition.png)


## <a name="logic-app-source-code"></a>Quellcode der Synchronisationslogik app

>[AZURE.NOTE]  Im folgenden sind nur Beispiele. Da dieses Lernprogramm eine Implementierung in Produktion basiert, kann Wert **Quellknoten** nicht Eigenschaften anzeigen, die Planen eines Termins zusammenhängen.

### <a name="logging"></a>Protokollierung
Das folgende Logik app-Codebeispiel veranschaulicht die Protokollierung.

#### <a name="log-entry"></a>Protokolleintrag
Dies ist der Logik app-Quellcode für einen Protokolleintrag einfügen.

``` json
"InsertLogEntry": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "date": "@{outputs('Gets_NewPatientRecord')['headers']['Date']}",
            "operation": "New Patient",
            "patientId": "@{triggerBody()['CRMid']}",
            "providerId": "@{triggerBody()['providerID']}",
            "source": "@{outputs('Gets_NewPatientRecord')['headers']}"
        },
        "method": "post",
        "uri": "https://.../api/Log"
        },
        "runAfter":    {
            "Gets_NewPatientecord": ["Succeeded"]
        }
}
```

#### <a name="log-request"></a>Protokoll-Anforderung

Dies ist Protokoll Anforderungsnachricht API-app bereitgestellt.

``` json
    {
    "uri": "https://.../api/Log",
    "method": "post",
    "body": {
        "date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "operation": "New Patient",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "providerId": "",
        "source": "{\"Pragma\":\"no-cache\",\"x-ms-request-id\":\"e750c9a9-bd48-44c4-bbba-1688b6f8a132\",\"OData-Version\":\"4.0\",\"Cache-Control\":\"no-cache\",\"Date\":\"Fri, 10 Jun 2016 22:31:56 GMT\",\"Set-Cookie\":\"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1\",\"Server\":\"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0\",\"X-AspNet-Version\":\"4.0.30319\",\"X-Powered-By\":\"ASP.NET\",\"Content-Length\":\"1935\",\"Content-Type\":\"application/json; odata.metadata=minimal; odata.streaming=true\",\"Expires\":\"-1\"}"
        }
    }

```


#### <a name="log-response"></a>Protokollantwort

Protokoll-Antwortnachricht von API-app ist

``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:32:17 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "964",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "ttl": 2592000,
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0_1465597937",
        "_rid": "XngRAOT6IQEHAAAAAAAAAA==",
        "_self": "dbs/XngRAA==/colls/XngRAOT6IQE=/docs/XngRAOT6IQEHAAAAAAAAAA==/",
        "_ts": 1465597936,
        "_etag": "\"0400fc2f-0000-0000-0000-575b3ff00000\"",
        "patientID": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:56Z",
        "source": "{\"Pragma\":\"no-cache\",\"x-ms-request-id\":\"e750c9a9-bd48-44c4-bbba-1688b6f8a132\",\"OData-Version\":\"4.0\",\"Cache-Control\":\"no-cache\",\"Date\":\"Fri, 10 Jun 2016 22:31:56 GMT\",\"Set-Cookie\":\"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1\",\"Server\":\"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0\",\"X-AspNet-Version\":\"4.0.30319\",\"X-Powered-By\":\"ASP.NET\",\"Content-Length\":\"1935\",\"Content-Type\":\"application/json; odata.metadata=minimal; odata.streaming=true\",\"Expires\":\"-1\"}",
        "operation": "New Patient",
        "salesforceId": "",
        "expired": false
    }
}

```

Jetzt sehen wir uns die Schritte für die Fehlerbehandlung.


### <a name="error-handling"></a>Fehlerbehandlung

Das folgende Logik Apps Codebeispiel zeigt Implementierung Fehlerbehandlung.

#### <a name="create-error-record"></a>Fehlerdatensatz erstellen

Dies ist der Logik Apps Quellcode für einen Fehlerbericht erstellen.

``` json
"actions": {
    "CreateErrorRecord": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "action": "New_Patient",
            "isError": true,
            "crmId": "@{triggerBody()['CRMid']}",
            "patientID": "@{triggerBody()['CRMid']}",
            "message": "@{body('Create_NewPatientRecord')['message']}",
            "providerId": "@{triggerBody()['providerId']}",
            "severity": 4,
            "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
            "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
            "salesforceId": "",
            "update": false
        },
        "method": "post",
        "uri": "https://.../api/CrMtoSfError"
        },
        "runAfter":
        {
            "Create_NewPatientRecord": ["Failed" ]
        }
    }
}          
```

#### <a name="insert-error-into-documentdb--request"></a>Fehler beim Einfügen in DocumentDB - Anforderung

``` json

{
    "uri": "https://.../api/CrMtoSfError",
    "method": "post",
    "body": {
        "action": "New_Patient",
        "isError": true,
        "crmId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "providerId": "",
        "severity": 4,
        "salesforceId": "",
        "update": false,
        "source": "{\"Account_Class_vod__c\":\"PRAC\",\"Account_Status_MED__c\":\"I\",\"CRM_HUB_ID__c\":\"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0\",\"Credentials_vod__c\",\"DTC_ID_MED__c\":\"\",\"Fax\":\"\",\"FirstName\":\"A\",\"Gender_vod__c\":\"\",\"IMS_ID__c\":\"\",\"LastName\":\"BAILEY\",\"MasterID_mp__c\":\"\",\"C_ID_MED__c\":\"851588\",\"Middle_vod__c\":\"\",\"NPI_vod__c\":\"\",\"PDRP_MED__c\":false,\"PersonDoNotCall\":false,\"PersonEmail\":\"\",\"PersonHasOptedOutOfEmail\":false,\"PersonHasOptedOutOfFax\":false,\"PersonMobilePhone\":\"\",\"Phone\":\"\",\"Practicing_Specialty__c\":\"FM - FAMILY MEDICINE\",\"Primary_City__c\":\"\",\"Primary_State__c\":\"\",\"Primary_Street_Line2__c\":\"\",\"Primary_Street__c\":\"\",\"Primary_Zip__c\":\"\",\"RecordTypeId\":\"012U0000000JaPWIA0\",\"Request_Date__c\":\"2016-06-10T22:31:55.9647467Z\",\"ONY_ID__c\":\"\",\"Specialty_1_vod__c\":\"\",\"Suffix_vod__c\":\"\",\"Website\":\"\"}",
        "statusCode": "400"
    }
}
```

#### <a name="insert-error-into-documentdb--response"></a>Fügen Sie Fehler beim in DocumentDB - Antwort ein


``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:57 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "1561",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0-1465597917",
        "_rid": "sQx2APhVzAA8AAAAAAAAAA==",
        "_self": "dbs/sQx2AA==/colls/sQx2APhVzAA=/docs/sQx2APhVzAA8AAAAAAAAAA==/",
        "_ts": 1465597912,
        "_etag": "\"0c00eaac-0000-0000-0000-575b3fdc0000\"",
        "prescriberId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:57.3651027Z",
        "action": "New_Patient",
        "salesforceId": "",
        "update": false,
        "body": "CRM failed to complete task: Message: duplicate value found: CRM_HUB_ID__c duplicates value on record with id: 001U000001c83gK",
        "source": "{\"Account_Class_vod__c\":\"PRAC\",\"Account_Status_MED__c\":\"I\",\"CRM_HUB_ID__c\":\"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0\",\"Credentials_vod__c\":\"DO - Degree level is DO\",\"DTC_ID_MED__c\":\"\",\"Fax\":\"\",\"FirstName\":\"A\",\"Gender_vod__c\":\"\",\"IMS_ID__c\":\"\",\"LastName\":\"BAILEY\",\"MterID_mp__c\":\"\",\"Medicis_ID_MED__c\":\"851588\",\"Middle_vod__c\":\"\",\"NPI_vod__c\":\"\",\"PDRP_MED__c\":false,\"PersonDoNotCall\":false,\"PersonEmail\":\"\",\"PersonHasOptedOutOfEmail\":false,\"PersonHasOptedOutOfFax\":false,\"PersonMobilePhone\":\"\",\"Phone\":\"\",\"Practicing_Specialty__c\":\"FM - FAMILY MEDICINE\",\"Primary_City__c\":\"\",\"Primary_State__c\":\"\",\"Primary_Street_Line2__c\":\"\",\"Primary_Street__c\":\"\",\"Primary_Zip__c\":\"\",\"RecordTypeId\":\"012U0000000JaPWIA0\",\"Request_Date__c\":\"2016-06-10T22:31:55.9647467Z\",\"XXXXXXX\":\"\",\"Specialty_1_vod__c\":\"\",\"Suffix_vod__c\":\"\",\"Website\":\"\"}",
        "code": 400,
        "errors": null,
        "isError": true,
        "severity": 4,
        "notes": null,
        "resolved": 0
        }
}
```

#### <a name="salesforce-error-response"></a>Salesforce-Fehlermeldung

``` json
{
    "statusCode": 400,
    "headers": {
        "Pragma": "no-cache",
        "x-ms-request-id": "3e8e4884-288e-4633-972c-8271b2cc912c",
        "X-Content-Type-Options": "nosniff",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "Set-Cookie": "ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1",
        "Server": "Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "205",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "status": 400,
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "source": "Salesforce.Common",
        "errors": []
    }
}

```

### <a name="returning-the-response-back-to-the-parent-logic-app"></a>Die Antwort auf die übergeordnete Logik app zurückgeben

Nachdem Sie die Antwort haben, können Sie übergeordnete Logik App übergeben werden.

#### <a name="return-success-response-to-the-parent-logic-app"></a>Erfolg auf übergeordnete Logik app zurück

``` json
"SuccessResponse": {
    "runAfter":
        {
            "UpdateNew_CRMPatientResponse": ["Succeeded"]
        },
    "inputs": {
        "body": {
            "status": "Success"
    },
    "headers": {
    "   Content-type": "application/json",
        "x-ms-date": "@utcnow()"
    },
    "statusCode": 200
    },
    "type": "Response"
}
```

#### <a name="return-error-response-to-the-parent-logic-app"></a>Fehler auf der übergeordneten Logik app

``` json
"ErrorResponse": {
    "runAfter":
        {
            "Create_NewPatientRecord": ["Failed"]
        },
    "inputs": {
        "body": {
            "status": "BadRequest"
        },
        "headers": {
            "Content-type": "application/json",
            "x-ms-date": "@utcnow()"
        },
        "statusCode": 400
    },
    "type": "Response"
}

```


## <a name="documentdb-repository-and-portal"></a>DocumentDB Repository und portal

Unsere Lösung hinzugefügt zusätzliche Funktionen mit [DocumentDB](https://azure.microsoft.com/services/documentdb).

### <a name="error-management-portal"></a>Fehler-Verwaltungsportal

Um den Fehler anzuzeigen, erstellen Sie eine MVC Web app Fehlerdatensätze aus DocumentDB angezeigt. **Liste**, **Details**, **Bearbeiten**und **Löschen** von Vorgängen werden in der aktuellen Version aufgenommen.

> [AZURE.NOTE]Bearbeitungsvorgang: DocumentDB ersetzt des gesamten Dokuments.
> Die Datensätze in der **Liste** und **Detailansichten** angezeigt sind nur Beispiele. Sie sind nicht Patienten Termindatensätze.

Es folgen Beispiele für unsere MVC app Details mit dem oben beschriebenen Ansatz erstellt.

#### <a name="error-management-list"></a>Fehlerliste management

![Fehlerliste](./media/app-service-logic-scenario-error-and-exception-handling/errorlist.png)

#### <a name="error-management-detail-view"></a>Fehler-Management-Detailansicht

![Fehlerdetails](./media/app-service-logic-scenario-error-and-exception-handling/errordetails.png)

### <a name="log-management-portal"></a>Protokoll-Verwaltungsportal

Zum Anzeigen der Protokolle haben wir auch eine MVC Web app.  Es folgen Beispiele für unsere MVC app Details mit dem oben beschriebenen Ansatz erstellt.

#### <a name="sample-log-detail-view"></a>Beispiel-Protokoll-Detailansicht

![Protokoll-Detailansicht](./media/app-service-logic-scenario-error-and-exception-handling/samplelogdetail.png)

### <a name="api-app-details"></a>API-app-details

#### <a name="logic-apps-exception-management-api"></a>Logik Apps ausnahmemanagement-API

Unsere Opensource-Logik Apps Ausnahme Management API-app bietet die folgenden Funktionen.

Es gibt zwei Controller:

- **ErrorController** Fügt ein Fehlerbericht (Dokument) in eine DocumentDB-Auflistung.
- **LogController** Fügt einen Protokolldatensatz (Dokument) in eine DocumentDB-Auflistung.

> [AZURE.TIP] Verwenden Sie beide Controller `async Task<dynamic>` Operationen. Dadurch können zur Laufzeit aufgelöst werden, sodass das Schema DocumentDB im Textkörper des Vorgangs.

Jedes Dokument in DocumentDB muss eine eindeutige ID haben. Wir verwenden `PatientId` und einen Zeitstempel, der in einem Unix-Timestamp-Wert (Double) konvertiert. Wir kürzen den anteilige Wert entfernen.

Sie können den Quellcode unserer Fehler Controller API [von GitHub](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi/blob/master/Logic App Exception Management API/Controllers/ErrorController.cs)anzeigen.

Wir rufen die API eine Anwendung Logik mit der folgenden Syntax.

``` json
 "actions": {
        "CreateErrorRecord": {
          "metadata": {
            "apiDefinitionUrl": "https://.../swagger/docs/v1",
            "swaggerSource": "website"
          },
          "type": "Http",
          "inputs": {
            "body": {
              "action": "New_Patient",
              "isError": true,
              "crmId": "@{triggerBody()['CRMid']}",
              "prescriberId": "@{triggerBody()['CRMid']}",
              "message": "@{body('Create_NewPatientRecord')['message']}",
              "salesforceId": "@{triggerBody()['salesforceID']}",
              "severity": 4,
              "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
              "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
              "update": false
            },
            "method": "post",
            "uri": "https://.../api/CrMtoSfError"
          },
          "runAfter": {
              "Create_NewPatientRecord": ["Failed"]
            }
        }
 }
```

Der Ausdruck im vorangehenden Beispiel ist *Create_NewPatientRecord* Status **Fehler**überprüft.

## <a name="summary"></a>Zusammenfassung

- Sie können problemlos Protokollierung und Fehlerbehandlung in einer Anwendung Logik implementieren.
- Sie können DocumentDB als Repository für und Datensätze (Dokumente).
- MVC können Sie um ein Portal anzeigen und Datensätze zu erstellen.

### <a name="source-code"></a>Quellcode
Der Quellcode für die Logik Apps ausnahmemanagement API-Anwendung steht in [GitHub Repository](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi "Logik App Ausnahme Management-API").


## <a name="next-steps"></a>Nächste Schritte
- [Weitere Logik Apps Beispiele und Szenarien](app-service-logic-examples-and-scenarios.md)
- [Erfahren Sie mehr über Logik Apps Überwachungstools](app-service-logic-monitor-your-logic-apps.md)
- [Erstellen einer Vorlage Logik App automatisierte Bereitstellung](app-service-logic-create-deploy-template.md)
