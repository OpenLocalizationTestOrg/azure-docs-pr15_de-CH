
<properties 
    pageTitle="Anpassen von Swashbuckle generierte API-Definitionen" 
    description="Informationen Sie zum Swagger API-Definitionen anpassen, die von Swashbuckle für eine API-app in Azure App Service generiert werden." 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="bradygaster" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/29/2016" 
    ms.author="rachelap"/>

# <a name="customize-swashbuckle-generated-api-definitions"></a>Anpassen von Swashbuckle generierte API-Definitionen 

## <a name="overview"></a>Übersicht

Dieser Artikel erläutert das Swashbuckle um Szenarien zu behandeln, sollten Sie das Standardverhalten ändern anpassen:

* Swashbuckle generiert doppelte Vorgang Bezeichner für überladenen Methoden des Controllers
* Swashbuckle setzt voraus, dass die Antwort nur eine Methode HTTP 200 (OK) 
 
## <a name="customize-operation-identifier-generation"></a>Die Generierung des Vorgangs-ID anpassen

Swashbuckle generiert stolz Vorgang Bezeichner durch Verketten von Controller und Methodennamens. Dieses Muster führt zu einem Problem bei mehreren überladenen Methode: Swashbuckle generiert doppelte Operation Ids Swagger JSON ungültig ist.

Beispielsweise verursacht der folgende controllercode Swashbuckle drei Contact_Get Vorgang Ids generiert.

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsincode.png)

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsinjson.png)

Sie können die manuell Problem mit Methoden eindeutige Namen, wie beispielsweise:

* Erhalten
* GetById
* GetPage

Die Alternative ist Swashbuckle dafür automatisch eindeutige Vorgangs-Ids zu erweitern.

Die folgenden Schritte zeigen, wie Swashbuckle über die *SwaggerConfig.cs* Datei im Projekt die Projektvorlage Visual Studio API-Apps Vorschau anpassen.  Swashbuckle kann in einem Web API-Projekt angepasst werden, die für die Bereitstellung als API-Anwendung konfigurieren.

1. Erstellen eines benutzerdefinierten `IOperationFilter` Implementierung 

    Die `IOperationFilter` Schnittstelle stellt einen Erweiterungspunkt für Swashbuckle Benutzer verschiedene Aspekte stolz Metadaten anpassen möchten. Der folgende Code veranschaulicht eine Methode, das Vorgangs-Id-Generation Verhalten ändern. Der Code fügt Parameternamen Name Id Vorgang.  

        using Swashbuckle.Swagger;
        using System.Web.Http.Description;
        
        namespace ContactsList
        {
            public class MultipleOperationsWithSameVerbFilter : IOperationFilter
            {
                public void Apply(
                    Operation operation,
                    SchemaRegistry schemaRegistry,
                    ApiDescription apiDescription)
                {
                    if (operation.parameters != null)
                    {
                        operation.operationId += "By";
                        foreach (var parm in operation.parameters)
                        {
                            operation.operationId += string.Format("{0}",parm.name);
                        }
                    }
                }
            }
        }

2. *App_Start\SwaggerConfig.cs* Datei rufen die `OperationFilter` zu Swashbuckle, die neue Methode `IOperationFilter` Implementierung.

        c.OperationFilter<MultipleOperationsWithSameVerbFilter>();

    ![](./media/app-service-api-dotnet-swashbuckle-customize/usefilter.png)

    *SwaggerConfig.cs* -Datei, die vom Swashbuckle NuGet-Paket abgelegt ist enthält viele Beispiele für auskommentierte Erweiterungspunkte. Zusätzliche Kommentare werden hier nicht angezeigt. 

    Nach dem vornehmen dieser Änderung der `IOperationFilter` Implementierung dient und eindeutige Vorgangs-Ids generiert wird.
 
    ![](./media/app-service-api-dotnet-swashbuckle-customize/uniqueids.png)

<a id="multiple-response-codes" name="multiple-response-codes"></a>
    
## <a name="allow-response-codes-other-than-200"></a>Antwort als 200 erlauben

Swashbuckle geht standardmäßig davon aus, dass eine HTTP 200 (OK) Antwort *nur* berechtigten Antwort von einer Web-API-Methode. In einigen Fällen möchten Sie anderen Antwort zurück, ohne den Client eine Ausnahme auslöst.  Beispielsweise zeigt der folgende Web API Code ein Szenario, in dem den Client eine 200 oder 404 als gültige Antworten akzeptieren möchten.

    [ResponseType(typeof(Contact))]
    public HttpResponseMessage Get(int id)
    {
        var contacts = GetContacts();

        var requestedContact = contacts.FirstOrDefault(x => x.Id == id);

        if (requestedContact == null)
        {
            return Request.CreateResponse(HttpStatusCode.NotFound);
        }
        else
        {
            return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
        }
    }

In diesem Szenario gibt der Swashbuckle standardmäßig generiert stolz nur eine legitime HTTP-Statuscode, HTTP 200.

![](./media/app-service-api-dotnet-swashbuckle-customize/http-200-output-only.png)

Da Visual Studio die Swagger-API-Definition zum Generieren von Code für den Client verwendet, erstellt Clientcode, der für die Antwort als HTTP 200, eine Ausnahme auslöst. Der folgende Code ist ein C#-Client für dieses Beispiel Web-API-Methode generiert.

    if (statusCode != HttpStatusCode.OK)
    {
        HttpOperationException<object> ex = new HttpOperationException<object>();
        ex.Request = httpRequest;
        ex.Response = httpResponse;
        ex.Body = null;
        if (shouldTrace)
        {
            ServiceClientTracing.Error(invocationId, ex);
        }
        throw ex;
    } 

Swashbuckle bietet zwei Methoden zum Anpassen der Liste der erwarteten HTTP-Antwortcodes, die es erzeugt mit XML-Kommentaren oder `SwaggerResponse` Attribut. Das Attribut ist einfacher, jedoch ist nur verfügbar in Swashbuckle 5.1.5. Enthält die API-Apps Vorschau neue-Projektvorlage in Visual Studio 2013 Swashbuckle Version 5.0.0, also wenn Sie die Vorlage und nicht Swashbuckle aktualisieren möchten, nur von XML-Kommentaren. 

### <a name="customize-expected-response-codes-using-xml-comments"></a>Passen Sie erwartete Antwort-Codes mit XML-Kommentaren an

Mit dieser Methode können Antwortcodes angeben, ist die Version Swashbuckle 5.1.5 vor.

1. Fügen Sie XML-Dokumentationskommentare zunächst über die Methoden zu HTTP-Antwortcodes für angeben. Die Probenahme Web API ergibt Aktion oben und die XML-Dokumentation zuweisen Code wie im folgenden Beispiel. 

        /// <summary>
        /// Returns the specified contact.
        /// </summary>
        /// <param name="id">The ID of the contact.</param>
        /// <returns>A contact record with an HTTP 200, or null with an HTTP 404.</returns>
        /// <response code="200">OK</response>
        /// <response code="404">Not Found</response>
        [ResponseType(typeof(Contact))]
        public HttpResponseMessage Get(int id)
        {
            var contacts = GetContacts();
        
            var requestedContact = contacts.FirstOrDefault(x => x.Id == id);
        
            if (requestedContact == null)
            {
                return Request.CreateResponse(HttpStatusCode.NotFound);
            }
            else
            {
                return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
            }
        }

1. Informationen in der Datei *SwaggerConfig.cs* Swashbuckle nutzen das XML direkt hinzufügen Dokumentation.

    * Öffnen Sie *SwaggerConfig.cs* , und erstellen Sie eine Methode für die *SwaggerConfig* -Klasse an den Pfad zu der XML-Dokumentationsdatei. 

            private static string GetXmlCommentsPath()
            {
                return string.Format(@"{0}\XmlComments.xml", 
                    System.AppDomain.CurrentDomain.BaseDirectory);
            }

    * Scrollen Sie in der Datei *SwaggerConfig.cs* Sie auskommentierte Zeile Code ähnlich der Abbildung unten. 

        ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-commented-out.png)
    
    * Kommentieren Sie die Zeile, um die XML-Kommentare im stolz Generation. 
    
        ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-uncommented.png)
    
1. Um die XML-Dokumentationsdatei generieren in den Projekteigenschaften und aktivieren Sie die XML-Dokumentationsdatei wie im folgenden Screenshot gezeigt. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/enable-xml-documentation-file.png) 

Nachdem Sie diese Schritte wider von Swashbuckle generierten Swagger JSON HTTP-Antwortcodes im XML-Kommentare angegeben. Der folgende Screenshot zeigt diese neue JSON-Nutzlast. 

![](./media/app-service-api-dotnet-swashbuckle-customize/swagger-multiple-responses.png)

Wenn Sie Visual Studio zum Regenerieren des Clientcode für die REST-API verwenden, akzeptiert der C#-Code ohne Auslösen einer Ausnahme, im verwendeten Code zu wie die Rückgabe von null Kontaktdatensatz Statuscodes HTTP OK und nicht gefunden. 

        if (statusCode != HttpStatusCode.OK && statusCode != HttpStatusCode.NotFound)
        {
            HttpOperationException<object> ex = new HttpOperationException<object>();
            ex.Request = httpRequest;
            ex.Response = httpResponse;
            ex.Body = null;
            if (shouldTrace)
            {
                ServiceClientTracing.Error(invocationId, ex);
            }
                throw ex;
        }

Der Code für diese Demo finden in [diesem GitHub Repository](https://github.com/Azure-Samples/app-service-api-dotnet-swashbuckle-swaggerresponse). Mit Web-API ist markiert mit XML-Dokumentationskommentare Projekt ein Konsolenanwendungsprojekt, die generierten Client für diese API enthält. 

### <a name="customize-expected-response-codes-using-the-swaggerresponse-attribute"></a>Passen Sie erwartete Antwort-Codes mit dem Attribut SwaggerResponse an

Das [SwaggerResponse](https://github.com/domaindrivendev/Swashbuckle/blob/master/Swashbuckle.Core/Swagger/Annotations/SwaggerResponseAttribute.cs) -Attribut ist in Swashbuckle 5.1.5 und höher verfügbar. Bei einer früheren Version in das Projekt, startet dieser Abschnitt erläutert, wie das Swashbuckle NuGet-Paket aktualisieren, damit dieses Attribut verwenden.

1. Im **Projektmappen-Explorer**Maustaste Web API Projekt klicken und **NuGet-Pakete verwalten**. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/manage-nuget-packages.png)

1. Klicken Sie auf die Schaltfläche *Aktualisieren* neben *Swashbuckle* NuGet-Paket. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/update-nuget-dialog.png)

1. Fügen Sie die *SwaggerResponse* Attribute für die Web-API Aktionsmethoden gültige HTTP-Antwort-Codes angegeben werden soll. 

        [SwaggerResponse(HttpStatusCode.OK)]
        [SwaggerResponse(HttpStatusCode.NotFound)]
        [ResponseType(typeof(Contact))]
        public HttpResponseMessage Get(int id)
        {
            var contacts = GetContacts();

            var requestedContact = contacts.FirstOrDefault(x => x.Id == id);
            if (requestedContact == null)
            {
                return Request.CreateResponse(HttpStatusCode.NotFound);
            }
            else
            {
                return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
            }
        }

2. Hinzufügen einer `using` -Anweisung für das Attribut Namespace:

        using Swashbuckle.Swagger.Annotations;
        
1. Navigieren Sie zu */swagger/docs/v1* -URL des Projekts und verschiedenen HTTP-Antwortcodes in JSON Swagger werden. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/multiple-responses-post-attributes.png)

Der Code für diese Demo finden in [diesem GitHub Repository](https://github.com/Azure-Samples/API-Apps-DotNet-Swashbuckle-Customization-MultipleResponseCodes-With-Attributes). Mit Web-API ist das *SwaggerResponse* -Attribut Projekt ein Konsolenanwendungsprojekt, die generierten Client für diese API enthält. 

## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel zeigt, wie Swashbuckle Vorgang Ids und gültige Antwortcodes generiert anpassen. Weitere Informationen finden Sie unter [Swashbuckle auf GitHub](https://github.com/domaindrivendev/Swashbuckle).
 
