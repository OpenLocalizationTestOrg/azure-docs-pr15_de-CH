<properties
 pageTitle="Ausgehende Authentifizierung Planer"
 description="Ausgehende Authentifizierung Planer"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/15/2016"
 ms.author="deli"/>

# <a name="scheduler-outbound-authentication"></a>Ausgehende Authentifizierung Planer

Scheduler-Jobs müssen Dienste aufrufen, die eine Authentifizierung erfordern. Auf diese Weise kann aufgerufenen Dienst ermitteln, ob des Steuerprogrammauftrags Ressourcen zugreifen kann. Einige dieser Dienste sind andere Azure Services, Salesforce.com, Facebook und sichere benutzerdefinierten Websites.

## <a name="adding-and-removing-authentication"></a>Hinzufügen und Entfernen von Authentifizierung

Authentifizierung für eine Steuerprogrammauftrag ist einfach, JSON untergeordnetes Element hinzufügen `authentication` , die `request` Element beim Erstellen oder Aktualisieren eines Auftrags. Geheime Schlüssel übergeben der Auftragsplanungsdienst PUT, PATCH oder POST-Anforderung – als Teil der `authentication` Objekt – nie Antworten zurückgegeben. Antworten, geheimer Informationen soll null oder möglicherweise ein öffentliches Token, die authentifizierte Entität darstellt.

Entfernen Sie Authentifizierung PLATZIEREN oder PATCH für das Projekt explizit festlegen der `authentication` Objekt auf null. Alle Eigenschaften in Antwort wird nicht angezeigt.

Die einzigen unterstützten Authentifizierungstypen derzeit die `ClientCertificate` Modell (für die Verwendung von Clientzertifikaten für SSL/TLS), die `Basic` (für Standardauthentifizierung) und `ActiveDirectoryOAuth` (für Active Directory OAuth Authentifizierung.)

## <a name="request-body-for-clientcertificate-authentication"></a>Authentifizierungsanfrage Körper ClientCertificate

Beim Hinzufügen von Authentifizierung mit der `ClientCertificate` Modell, andere Elemente im Hauptteil Anforderung angeben.  

|Element|Beschreibung|
|:---|:---|
|_Authentifizierung (übergeordnetes Element)_|Authentifizierungsobjekt für die Verwendung von SSL-Clientzertifikat.|
|_Typ_|Erforderlich. Art der Authentifizierung. Für SSL Clientzertifikate muss der Wert `ClientCertificate`.|
|_PFX_|Erforderlich. Base64-codierten Inhalt der PFX-Datei.|
|_Kennwort_|Erforderlich. Kennwort für die PFX-Datei zugreifen.|


## <a name="response-body-for-clientcertificate-authentication"></a>Antworttext für die ClientCertificate-Authentifizierung

Wenn eine Anforderung mit Authentifizierungsinformationen gesendet wird, enthält die Antwort die folgenden Authentifizierung Elemente.

|Element |Beschreibung |
|:--|:--|
|_Authentifizierung (übergeordnetes Element)_ |Authentifizierungsobjekt für die Verwendung von SSL-Clientzertifikat.|
|_Typ_ |Art der Authentifizierung. Für SSL Clientzertifikate ist der Wert `ClientCertificate`.|
|_certificateThumbprint_ |Der Fingerabdruck des Zertifikats.|
|_certificateSubjectName_ |Der distinguished Name des Antragstellers des Zertifikats.|
|_certificateExpiration_ |Das Ablaufdatum des Zertifikats.|

## <a name="sample-rest-request-for-clientcertificate-authentication"></a>ANDEREN Beispielanfrage-ClientCertificate Authentifizierung

```
PUT https://management.azure.com/subscriptions/1fe0abdf-581e-4dfe-9ec7-e5cb8e7b205e/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "clientcertificate",
          "password": "password",
          "pfx": "pfx key"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-clientcertificate-authentication"></a>REST Beispielantwort ClientCertificate Authentifizierung

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 858
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 56c7b40e-721a-437e-88e6-f68562a73aa8
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 1075219e-e879-4030-bc81-094e54fbabce
x-ms-routing-request-id: WESTUS:20160316T190424Z:1075219e-e879-4030-bc81-094e54fbabce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:04:23 GMT

{
  "id": "/subscriptions/1fe0abdf-581e-4dfe-9ec7-e5cb8e7b205e/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
  "type": "Microsoft.Scheduler/jobCollections/jobs",
  "name": "southeastasiajc/httpjob",
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "certificateThumbprint": "88105CG9DF9ADE75B835711D899296CB217D7055",
          "certificateExpiration": "2021-01-01T07:00:00Z",
          "certificateSubjectName": "CN=Scheduler Mgmt",
          "type": "ClientCertificate"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
    "status": {
      "nextExecutionTime": "2016-03-16T19:05:00Z",
      "executionCount": 0,
      "failureCount": 0,
      "faultedCount": 0
    }
  }
}
```

## <a name="request-body-for-basic-authentication"></a>Anfragetext für die Standardauthentifizierung

Beim Hinzufügen von Authentifizierung mit der `Basic` Modell, andere Elemente im Hauptteil Anforderung angeben.

|Element|Beschreibung|
|:--|:--|
|_Authentifizierung (übergeordnetes Element)_ |Authentifizierungsobjekt für die Standardauthentifizierung.|
|_Typ_ |Erforderlich. Art der Authentifizierung. Bei der Standardauthentifizierung muss der Wert `Basic`.|
|_Benutzername_ |Erforderlich. Der Benutzername authentifiziert.|
|_Kennwort_ |Erforderlich. Kennwort authentifizieren.|

## <a name="response-body-for-basic-authentication"></a>Antworttext für die Standardauthentifizierung

Wenn eine Anforderung mit Authentifizierungsinformationen gesendet wird, enthält die Antwort die folgenden Authentifizierung Elemente.

|Element|Beschreibung|
|:--|:--|
|_Authentifizierung (übergeordnetes Element)_ |Authentifizierungsobjekt für die Standardauthentifizierung.|
|_Typ_ |Art der Authentifizierung. Bei der Standardauthentifizierung der Wert `Basic`.|
|_Benutzername_ |Der authentifizierte Benutzername.|

## <a name="sample-rest-request-for-basic-authentication"></a>ANDEREN Beispielanfrage-für die Standardauthentifizierung

```
PUT https://management.azure.com/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 562
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "basic",
          "username": "user",
          "password": "password"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-basic-authentication"></a>REST Beispielantwort für die Standardauthentifizierung

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 701
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: a2dcb9cd-1aea-4887-8893-d81273a8cf04
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 7816f222-6ea7-468d-b919-e6ddebbd7e95
x-ms-routing-request-id: WESTUS:20160316T190506Z:7816f222-6ea7-468d-b919-e6ddebbd7e95
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:05:06 GMT

{  
   "id":"/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type":"Microsoft.Scheduler/jobCollections/jobs",
   "name":"southeastasiajc/httpjob",
   "properties":{  
      "startTime":"2015-05-14T14:10:00Z",
      "action":{  
         "request":{  
            "uri":"https://mywebserviceendpoint.com",
            "method":"GET",
            "headers":{  
               "x-ms-version":"2013-03-01"
            },
            "authentication":{  
               "username":"user1",
               "type":"Basic"
            }
         },
         "type":"http"
      },
      "recurrence":{  
         "frequency":"minute",
         "endTime":"2016-04-10T08:00:00Z",
         "interval":1
      },
      "state":"enabled",
      "status":{  
         "nextExecutionTime":"2016-03-16T19:06:00Z",
         "executionCount":0,
         "failureCount":0,
         "faultedCount":0
      }
   }
}
```

## <a name="request-body-for-activedirectoryoauth-authentication"></a>Authentifizierungsanfrage Körper ActiveDirectoryOAuth

Beim Hinzufügen von Authentifizierung mit der `ActiveDirectoryOAuth` Modell, andere Elemente im Hauptteil Anforderung angeben.

|Element |Beschreibung |
|:--|:--|
|_Authentifizierung (übergeordnetes Element)_ |Authentifizierungsobjekt ActiveDirectoryOAuth Authentifizierung.|
|_Typ_ |Erforderlich. Art der Authentifizierung. Für die ActiveDirectoryOAuth Authentifizierung muss der Wert `ActiveDirectoryOAuth`.|
|_Mieter_ |Erforderlich. Der Mieter Bezeichner für Azure AD-Mandanten.|
|_Zielgruppe_ |Erforderlich. Dies wird auf https://management.core.windows.net/ festgelegt.|
|_clientId_ |Erforderlich. Bereitstellen Sie die Client-ID für die Azure AD-Anwendung.|
|_Geheimnis_ |Erforderlich. Kennwort des Clients, die das Token angefordert werden.|

### <a name="determining-your-tenant-identifier"></a>Mandanten-ID bestimmen

Finden Sie die Mandanten-ID für Azure AD-Mandanten mit `Get-AzureAccount` in Azure PowerShell.

## <a name="response-body-for-activedirectoryoauth-authentication"></a>Antworttext für ActiveDirectoryOAuth-Authentifizierung

Wenn eine Anforderung mit Authentifizierungsinformationen gesendet wird, enthält die Antwort die folgenden Authentifizierung Elemente.

|Element |Beschreibung |
|:--|:--|
|_Authentifizierung (übergeordnetes Element)_ |Authentifizierungsobjekt ActiveDirectoryOAuth Authentifizierung.|
|_Typ_ |Art der Authentifizierung. Für ActiveDirectoryOAuth-Authentifizierung ist der Wert `ActiveDirectoryOAuth`.|
|_Mieter_ |Der Mieter Bezeichner für Azure AD-Mandanten. |
|_Zielgruppe_ |Dies wird auf https://management.core.windows.net/ festgelegt.|
|_clientId_ |Die Client-ID für die Azure AD-Anwendung.|

## <a name="sample-rest-request-for-activedirectoryoauth-authentication"></a>ANDEREN Beispielanfrage-für ActiveDirectoryOAuth-Authentifizierung

```
PUT https://management.azure.com/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 757
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "tenant":"microsoft.onmicrosoft.com",
          "audience":"https://management.core.windows.net/",
          "clientId":"dc23e764-9be6-4a33-9b9a-c46e36f0c137",
          "secret": "G6u071r8Gjw4V4KSibnb+VK4+tX399hkHaj7LOyHuj5=",
          "type":"ActiveDirectoryOAuth"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-activedirectoryoauth-authentication"></a>REST Beispielantwort für ActiveDirectoryOAuth-Authentifizierung

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 885
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 86d8e9fd-ac0d-4bed-9420-9baba1af3251
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
x-ms-routing-request-id: WESTUS:20160316T191003Z:5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:10:02 GMT

{  
   "id":"/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type":"Microsoft.Scheduler/jobCollections/jobs",
   "name":"southeastasiajc/httpjob",
   "properties":{  
      "startTime":"2015-05-14T14:10:00Z",
      "action":{  
         "request":{  
            "uri":"https://mywebserviceendpoint.com",
            "method":"GET",
            "headers":{  
               "x-ms-version":"2013-03-01"
            },
            "authentication":{  
               "tenant":"microsoft.onmicrosoft.com",
               "audience":"https://management.core.windows.net/",
               "clientId":"dc23e764-9be6-4a33-9b9a-c46e36f0c137",
               "type":"ActiveDirectoryOAuth"
            }
         },
         "type":"http"
      },
      "recurrence":{  
         "frequency":"minute",
         "endTime":"2016-04-10T08:00:00Z",
         "interval":1
      },
      "state":"enabled",
      "status":{  
         "lastExecutionTime":"2016-03-16T19:10:00.3762123Z",
         "nextExecutionTime":"2016-03-16T19:11:00Z",
         "executionCount":5,
         "failureCount":5,
         "faultedCount":1
      }
   }
}
```

## <a name="see-also"></a>Siehe auch


 [Was ist der Taskplaner?](scheduler-intro.md)

 [Azure Scheduler Konzepte, Terminologie und Entitätshierarchie](scheduler-concepts-terms.md)

 [Erste Schritte mit Planer im Azure-portal](scheduler-get-started-portal.md)

 [Pläne und Fakturierung in Azure Scheduler](scheduler-plans-billing.md)

 [Azure Scheduler REST-API-Referenz](https://msdn.microsoft.com/library/mt629143)

 [Azure Scheduler-PowerShell-Cmdlets verweisen](scheduler-powershell-reference.md)

 [Azure Scheduler hohe Verfügbarkeit und Zuverlässigkeit](scheduler-high-availability-reliability.md)

 [Azure Scheduler Grenzen, Standards und Fehlercodes](scheduler-limits-defaults-errors.md)
