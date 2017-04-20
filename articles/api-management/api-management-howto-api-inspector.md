<properties 
    pageTitle="Verwendung der API-Inspektor verfolgen ruft in Azure API Management" 
    description="Erfahren Sie, wie Aufrufe mit API-Inspektor in Azure API Management verfolgen." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-use-the-api-inspector-to-trace-calls-in-azure-api-management"></a>Verwendung der API-Inspektor verfolgen ruft in Azure API Management

API-Management bietet ein API-Inspektor-Tool zur Unterstützung bei der Fehlersuche und -Behebung APIs. API-Inspektor kann programmgesteuert verwendet werden und können auch direkt aus dem Developer Portal verwendet werden. 

Neben Tracing verfolgt API-Inspektor [Richtlinienausdruck](https://msdn.microsoft.com/library/azure/dn910913.aspx) Produkttests. Ein Beispiel finden Sie unter [Cloud decken Episode 177: mehr Management-API-Funktionen](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) und Vorspulen bis 21:00 Uhr.

Dieses Handbuch enthält API-Inspektor mit Blättern.

>[AZURE.NOTE] API-Inspektor Spuren sind nur generiert und zur Verfügung, die [dem Administratorkonto](api-management-howto-create-groups.md) gehören Abonnement-Schlüssel enthält.

## <a name="trace-call"></a> Mit API-Inspektor einen Aufruf verfolgen

Fügen Sie zum Verwenden von API-Inspektor ein **Ocp-Apim-Trace: true** Anfrage-Header auf der Vorgangsaufruf herunter und Trace mit Antwortheaders **Ocp-Apim-Trace-Adresse** angegebenen URL überprüfen. Dies kann programmgesteuert erfolgen und auch direkt aus dem Developer Portal erfolgen.

Dieses Lernprogramm zeigt, wie mithilfe der API-Inspektor Trace-Operationen mit der einfachen Rechner-API das [Verwalten Ihrer ersten API](api-management-get-started.md) immer gestartet Lernprogramm konfiguriert ist. Wenn Sie dieses Lernprogramm abgeschlossen haben dauert nur einen Augenblick der grundlegenden API Rechner importieren oder eine andere API Ihrer Wahl wie die Echo-API verwenden. Jede API Management Service-Instanz enthält vorkonfigurierte ein Echo-API, das experimentieren und API-Management verwendet werden kann. Die Echo-API kehrt zurück, was Eingabe gesendet wird. Verwendung jedes HTTP-Verb aufrufen, und der Rückgabewert werden einfach Sie gesendet. 



Klicken Sie zunächst auf **Entwicklerportal** im klassischen Azure-Portal für Ihren API Management-Dienst. Vorgänge können direkt aus dem Developer Portal bietet eine bequeme Möglichkeit zu testen die Vorgänge einer API aufgerufen werden.

>Wenn Sie noch eine API Management Service-Instanz erstellt haben, finden Sie im Lernprogramm [Erste Schritte mit Azure API Management][] [Erstellen API Management Service][] .

![API Management-Entwicklerportal][api-management-developer-portal-menu]

Klicken Sie im oberen Menü auf **APIs** und klicken Sie dann auf **Rechner**.

![Echo-API][api-management-api]

Klicken Sie auf **ausprobieren** , um das **Hinzufügen zweier Ganzzahlen** versuchen.

![Versuch es][api-management-open-console]

Übernehmen Sie den Standardnamen Parameterwerte, und wählen Sie den Abonnement-Key für das Produkt aus dem Dropdown- **Abonnement-Schlüssel** verwenden möchten.

Standardmäßig im Entwicklerportal **Ocp-Apim-Trace** ist Header bereits auf **true**festgelegt. Dieser Header konfiguriert ob Trace generiert wird.

![Senden][api-management-http-get]

Klicken Sie auf **Senden** , um den Vorgang aufzurufen.

![Senden][api-management-send-results]

In der Antwort werden Header ein **Ocp Apim Trace Speicherort** mit einem Wert wie im folgenden Beispiel.

    ocp-apim-trace-location : https://contosoltdxw7zagdfsprykd.blob.core.windows.net/apiinspectorcontainer/ZW3e23NsW4wQyS-SHjS0Og2-2?sv=2013-08-15&sr=b&sig=Mgx7cMHsLmVDv%2B%2BSzvg3JR8qGTHoOyIAV7xDsZbF7%2Bk%3D&se=2014-05-04T21%3A00%3A13Z&sp=r&verify_guid=a56a17d83de04fcb8b9766df38514742

Trace kann vom angegebenen Speicherort gedownloadet und überprüft werden, wie im nächsten Schritt.

## <a name="inspect-trace"> </a>Trace prüfen

Überprüfen Sie die Werte in der Verfolgung **Ocp Apim Trace Speicherort** URL herunterladen der Ablaufverfolgungsdatei. Es ist eine Textdatei im JSON-Format und Einträge wie das folgende Beispiel enthält.

    {
        "traceId": "abcd8ea63d134c1fabe6371566c7cbea",
        "traceEntries": {
            "inbound": [
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0725926",
                    "data": {
                        "request": {
                            "method": "GET",
                            "url": "https://contoso5.azure-api.net/calc/add?a=51&b=49",
                            "headers": [
                                {
                                    "name": "Ocp-Apim-Subscription-Key",
                                    "value": "5d7c41af64a44a68a2ea46580d271a59"
                                },
                                {
                                    "name": "Connection",
                                    "value": "Keep-Alive"
                                },
                                {
                                    "name": "Host",
                                    "value": "contoso5.azure-api.net"
                                }
                            ]
                        }
                    }
                },
                {
                    "source": "mapper",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0726213",
                    "data": {
                        "configuration": {
                            "api": {
                                "from": "/calc",
                                "to": {
                                    "scheme": "http",
                                    "host": "calcapi.cloudapp.net",
                                    "port": 80,
                                    "path": "/api",
                                    "queryString": "",
                                    "query": {},
                                    "isDefaultPort": true
                                }
                            },
                            "operation": {
                                "method": "GET",
                                "uriTemplate": "/add?a={a}&b={b}"
                            },
                            "user": {
                                "id": 1,
                                "groups": [
                                    "Administrators",
                                    "Developers"
                                ]
                            },
                            "product": {
                                "id": 1
                            }
                        }
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0727522",
                    "data": {
                        "message": "Request is being forwarded to the backend service.",
                        "request": {
                            "method": "GET",
                            "url": "http://calcapi.cloudapp.net/api/add?a=51&b=49",
                            "headers": [
                                {
                                    "name": "Ocp-Apim-Subscription-Key",
                                    "value": "5d7c41af64a44a68a2ea46580d271a59"
                                },
                                {
                                    "name": "X-Forwarded-For",
                                    "value": "33.52.215.35"
                                }
                            ]
                        }
                    }
                }
            ],
            "outbound": [
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1960601",
                    "data": {
                        "response": {
                            "status": {
                                "code": 200,
                                "reason": "OK"
                            },
                            "headers": [
                                {
                                    "name": "Pragma",
                                    "value": "no-cache"
                                },
                                {
                                    "name": "Content-Length",
                                    "value": "124"
                                },
                                {
                                    "name": "Cache-Control",
                                    "value": "no-cache"
                                },
                                {
                                    "name": "Content-Type",
                                    "value": "application/xml; charset=utf-8"
                                },
                                {
                                    "name": "Date",
                                    "value": "Tue, 23 Jun 2015 19:51:35 GMT"
                                },
                                {
                                    "name": "Expires",
                                    "value": "-1"
                                },
                                {
                                    "name": "Server",
                                    "value": "Microsoft-IIS/8.5"
                                },
                                {
                                    "name": "X-AspNet-Version",
                                    "value": "4.0.30319"
                                },
                                {
                                    "name": "X-Powered-By",
                                    "value": "ASP.NET"
                                }
                            ]
                        }
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1961112",
                    "data": {
                        "message": "Response headers have been sent to the caller. Starting to stream the response body."
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1963155",
                    "data": {
                        "message": "Response body streaming to the caller is complete."
                    }
                }
            ]
        }
    }

## <a name="next-steps"> </a>Nächste Schritte

-   Demo über tracing Richtlinienausdrücke in [Cloud decken Episode 177: mehr Management-API-Funktionen](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/). Schneller Vorlauf bis 21:00 Demo sehen.

>[AZURE.VIDEO episode-177-more-api-management-features-with-vlad-vinogradsky]

[Use API Inspector to trace a call]: #trace-call
[Inspect the trace]: #inspect-trace
[Next steps]: #next-steps

[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md

[Erste Schritte mit Azure API Management]: api-management-get-started.md
[Erstellen Sie eine Instanz der API Management service]: api-management-get-started.md#create-service-instance
[Azure Classic Portal]: https://manage.windowsazure.com/


[api-management-developer-portal-menu]: ./media/api-management-howto-api-inspector/api-management-developer-portal-menu.png
[api-management-api]: ./media/api-management-howto-api-inspector/api-management-api.png
[api-management-echo-api-get]: ./media/api-management-howto-api-inspector/api-management-echo-api-get.png
[api-management-developer-key]: ./media/api-management-howto-api-inspector/api-management-developer-key.png
[api-management-open-console]: ./media/api-management-howto-api-inspector/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-api-inspector/api-management-http-get.png
[api-management-send-results]: ./media/api-management-howto-api-inspector/api-management-send-results.png




 