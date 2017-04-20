<properties
    pageTitle="Erstellen und Bereitstellen einer Java-API-Anwendung in Azure App Service"
    description="Informationen Sie zum Java-API-Anwendung erstellen und in Azure App Service bereitzustellen."
    services="app-service\api"
    documentationCenter="java"
    authors="bradygaster"
    manager="mohisri"
    editor="tdykstra"/>

<tags
    ms.service="app-service-api"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="get-started-article"
    ms.date="08/31/2016"
    ms.author="rachelap"/>

# <a name="build-and-deploy-a-java-api-app-in-azure-app-service"></a>Erstellen und Bereitstellen einer Java-API-Anwendung in Azure App Service

[AZURE.INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

Dieses Lernprogramm veranschaulicht eine Java-Anwendung erstellen und in Azure App Service API-Apps Bereitstellen mit [Git]. Die in diesem Lernprogramm kann auf einem beliebigen Betriebssystem Anweisungen, die zur Ausführung von Java. Der Code in diesem Lernprogramm basiert auf [Maven]. [Jax-RS] dient zum Erstellen von Rest-Diensts und wird gemäß der Metadatenspezifikation [Swagger] mit dem [Swagger-Editor].

## <a name="prerequisites"></a>Erforderliche Komponenten

1. [Java Developer Kit 8] \(oder höher)
1. [Maven] auf dem Entwicklungscomputer installiert
1. [Git] auf Ihrem Entwicklungscomputer installiert
1. Ein [Microsoft Azure-] Abonnement gezahlten oder [kostenlose Testversion]
1. Ein HTTP-Anwendung wie [Postman]

## <a name="scaffold-the-api-using-swaggerio"></a>Gerüst der API mit Swagger.IO

Im swagger.io online-Editor können Sie die Struktur der API JSON Swagger oder YAML Code eingeben. Haben Sie die API-Oberfläche entworfen, können Sie Code für eine Vielzahl von Plattformen und Frameworks exportieren. Im nächsten Abschnitt wird der erstellten Code mock-Funktionalität geändert. 

In dieser Demo beginnt mit Swagger JSON-Text, die in den swagger.io-Editor einfügen wird die Nutzung JAX-RS auf einen Endpunkt REST-API Code generieren verwendet werden soll. Anschließend bearbeiten erstellten Code zum Zurückgeben von simulierten Daten Sie simulieren einen REST-API auf Daten Dauerhaftigkeitsmechanismus erstellt.  

1. Kopieren Sie den folgenden Swagger JSON-Code in die Zwischenablage:

        {
            "swagger": "2.0",
            "info": {
                "version": "v1",
                "title": "Contact List",
                "description": "A Contact list API based on Swagger and built using Java"
            },
            "host": "localhost",
            "schemes": [
                "http",
                "https"
            ],
            "basePath": "/api",
            "paths": {
                "/contacts": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_get",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                },
                "/contacts/{id}": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_getById",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "parameters": [
                            {
                                "name": "id",
                                "in": "path",
                                "required": true,
                                "type": "integer",
                                "format": "int32"
                            }
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                }
            },
            "definitions": {
                "Contact": {
                    "type": "object",
                    "properties": {
                        "Id": {
                            "format": "int32",
                            "type": "integer"
                        },
                        "Name": {
                            "type": "string"
                        },
                        "EmailAddress": {
                            "type": "string"
                        }
                    }
                }
            }
        }

1. Navigieren Sie zu [Swagger Online Editor]. Klicken Sie einmal, auf **File-einfügen JSON** Menüelement.

    ![JSON-Menüelement einfügen][paste-json]

1. Fügen Sie in Kontakte Liste API Swagger JSON zuvor kopiert. 

    ![JSON-Code in Swagger einfügen][pasted-swagger]

1. Anzeigen der Dokumentationsseiten und API-Zusammenfassung im Editor dargestellt. 

    ![Ansicht stolz generierte Dokumente][view-swagger-generated-docs]

1. Wählen Sie Menüoption **generieren Server-> JAX-RS** Gerüst serverseitigen Code später bearbeiten werden simulierte Implementierung hinzufügen. 

    ![Menüelement Code generieren][generate-code-menu-item]

    Sobald der Code generiert wird, werden Sie eine Zipdatei herunterladen bereitgestellt. Diese Datei enthält den Code der Codegenerator stolz Gerüstbau und alle Skripts erstellen. Extrahieren Sie die gesamte Bibliothek in ein Verzeichnis auf Ihrer Entwicklungsarbeitsstation. 

## <a name="edit-the-code-to-add-api-implementation"></a>Bearbeiten Sie den Code zum Hinzufügen von API-Implementierung

In diesem Abschnitt ersetzen Sie serverseitige Implementierung der stolz generierten Code durch den benutzerdefinierten Code. Der neue Code wird eine ArrayList von Contact-Entitäten an den aufrufenden Client zurück. 

1. Die Datei *Contact.java* Modell, liegt im Ordner *Src/Gen/Java/Io/stolz/Modell* mit [Visual Studio-Code] oder einen Texteditor. 

    ![Kontakt-Modelldatei öffnen][open-contact-model-file]

1. Der **Kontakt** -Klasse den folgenden Konstruktor hinzufügen. 

        public Contact(Integer id, String name, String email) 
        {
            this.id = id;
            this.name = name;
            this.emailAddress = email;
        }

1. Öffnen Sie die *ContactsApiServiceImpl.java* Implementierungsdatei liegt im Ordner *Src/Main/Java/Io/stolz/api/Impl* mit [Visual Studio-Code] oder einen Texteditor.

    ![Datei öffnen Kontaktdienst][open-contact-service-code-file]

1. Überschrieben Sie den Code in der Datei mit dem neuen Code den Code eine simulierte Implementierung hinzufügen. 

        package io.swagger.api.impl;

        import io.swagger.api.*;
        import io.swagger.model.*;
        import com.sun.jersey.multipart.FormDataParam;
        import io.swagger.model.Contact;
        import java.util.*;
        import io.swagger.api.NotFoundException;
        import java.io.InputStream;
        import com.sun.jersey.core.header.FormDataContentDisposition;
        import com.sun.jersey.multipart.FormDataParam;
        import javax.ws.rs.core.Response;
        import javax.ws.rs.core.SecurityContext;

        @javax.annotation.Generated(value = "class io.swagger.codegen.languages.JaxRSServerCodegen", date = "2015-11-24T21:54:11.648Z")
        public class ContactsApiServiceImpl extends ContactsApiService {
  
            private ArrayList<Contact> loadContacts()
            {
                ArrayList<Contact> list = new ArrayList<Contact>();
                list.add(new Contact(1, "Barney Poland", "barney@contoso.com"));
                list.add(new Contact(2, "Lacy Barrera", "lacy@contoso.com"));
                list.add(new Contact(3, "Lora Riggs", "lora@contoso.com"));
                return list;
            }
  
            @Override
            public Response contactsGet(SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                return Response.ok().entity(list).build();
                }
  
            @Override
            public Response contactsGetById(Integer id, SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                Contact ret = null;
            
                for(int i=0; i<list.size(); i++)
                {
                    if(list.get(i).getId() == id)
                        {
                            ret = list.get(i);
                        }
                }
                return Response.ok().entity(ret).build();
            }
        }

1. Öffnen Sie ein Eingabeaufforderungsfenster, und ändern Sie Verzeichnis in den Stammordner der Anwendung.

1. Führen Sie den folgenden Maven Befehl erstellen den Code und mit Steg Anwendungsserver ausführen. 

        mvn package jetty:run

1. Sehen Sie im Befehlsfenster widerzuspiegeln, dass Steg Code Port 8080 gestartet wurde. 

    ![Datei öffnen Kontaktdienst][run-jetty-war]

1. Mit der API-Methode http://localhost: 8080/api/Kontakte anfordern zu den"alle Kontakte" [Briefträger] .

    ![Kontakte-API-aufrufen][calling-contacts-api]

1. [Postbote] am http://localhost: 8080/api/Kontakte/2 "get bestimmten Kontakt"-API-Methode eine Anforderung zu verwenden.

    ![Kontakte-API-aufrufen][calling-specific-contact-api]

1. Erstellen Sie abschließend Java WAR (Webarchiv)-Datei mit dem folgenden Maven Befehl in der Konsole. 

        mvn package war:war

1. Sobald die WAR-Datei erstellt wird, wird sie in **den Zielordner** abgelegt. Navigieren in **den Zielordner** , und benennen Sie die WAR-Datei in **ROOT.war**. (Stellen Sie sicher, dass die Groß-/Kleinschreibung dieses Format entspricht).

         rename swagger-jaxrs-server-1.0.0.war ROOT.war

1. Abschließend führen Sie die folgenden Befehle aus dem Stammverzeichnis der Anwendung zum Erstellen eines Ordners **Bereitstellen** die WAR-Datei in Azure bereitstellen. 

         mkdir deploy
         mkdir deploy\webapps
         copy target\ROOT.war deploy\webapps
         cd deploy

## <a name="publish-the-output-to-azure-app-service"></a>Die Ausgabe in Azure App Service veröffentlichen

In diesem Abschnitt werden Sie lernen, erstellen Sie eine neue API-App mit Azure-Verwaltungsportal App API zum Hosten von Java Applikationen vorbereiten und Bereitstellen die WAR-Datei neu erstellt Azure App Service Ihre neue API App ausgeführt. 

1. Erstellen Sie eine neue API-app im [Azure-Portal] **Neu -> Web + Mobile -> API-app** klicken Ihre app-Daten eingeben und **dann auf**.

    ![Neue API-App erstellen][create-api-app]

1. Erstellte API-app öffnen Sie Ihre app **Einstellungen** Blade, und klicken Sie dann auf das Menüelement **ApplicationSettings** . Java Versionen wählen Sie aus den verfügbaren Optionen Menü aktuellen Tomcat **Web-Container** , und klicken Sie auf **Speichern**.

    ![Java API App Blatt einrichten][set-up-java]

1. Klicken Sie auf das Menüelement **Bereitstellung Anmeldeinformationen** Einstellungen und geben Sie einen Benutzernamen und ein Kennwort für Ihre API App Dateien veröffentlichen möchten. 

    ![Bereitstellung Anmeldeinformationen festlegen][deployment-credentials]

1. Klicken Sie auf das Menüelement **bereitstellungsquelle** Settings. **Einmal, klicken Sie auf **Quelle auswählen** und die Option **Lokale Git Repository** .** Dadurch entsteht ein Git Repository in Azure, die mit Ihrer App API ausgeführt. Jedes Mal, wenn Sie Code an die *master* Repository Git commit Code in Ihre live ausgeführte API App-Instanz erscheint. 

    ![Richten Sie ein neues lokales Git repository][select-git-repo]

1. Kopieren Sie neue Git Repository-URL in die Zwischenablage. Speichern Sie als gleich wichtig werden. 

    ![Richten Sie ein neues Git Repository für Ihre Anwendung][copy-git-repo-url]

1. Git push die WAR-Datei online-Repository. Navigieren Sie hierzu in den Ordner **Bereitstellen** , den Sie zuvor erstellt haben, damit Sie Code bis unter App Service Repository Commit ausführen können. Wenn Sie in der Konsole und in den Ordner Webapps Ordner navigiert, Befehle der folgenden Git Prozesses und Feuer aus einer Bereitstellung. 

         git init
         git add .
         git commit -m "initial commit"
         git remote add azure [YOUR GIT URL]
         git push azure master

    Nachdem Sie die **Push** -Anforderung ausgeben, das Kennwort müssen Sie für die Bereitstellung Anmeldeinformationen zuvor erstellten. Nachdem Sie Ihre Anmeldeinformationen eingeben, erhalten Sie Ihr Portal anzeigen, dass das Update bereitgestellt wurde.

1. Wenn Sie Briefträger wieder die neu bereitgestellte API-App im Azure App-Dienst verwenden, sehen Sie, dass das Verhalten und jetzt es Daten zurückgeben ist, wie erwartet und einfache Änderungen an der Swagger.io mit Java-Code Gerüstbau. 

    ![Die REST-API von Java Kontakte verwenden in Azure live][postman-calling-azure-contacts]

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurden mit einer Swagger JSON-Datei starten und Gerüstbau einige Java-Code-Editor Swagger.io gewonnen. Von dort einfachen ändern und ein Git Bereitstellungsvorgang geführt haben eine funktionale API-Anwendung in Java geschrieben. Die nächsten Lernprogramm zeigt wie Sie [API-apps von JavaScript-Clients mit CORS][App Service API CORS]. Nachfolgenden Lernprogramme in der Reihe zeigen, wie Authentifizierung und Autorisierung implementiert.

Um dieses Beispiel zu erstellen, erfahren Sie mehr über [Speicher-SDK für Java] JSON-Blobs beibehalten. Oder können Sie das [Dokument DB Java SDK] die Kontaktdaten in Azure Dokument DB speichern. 

Weitere Informationen über Java in Azure finden Sie unter [Java Developer Center].

<!-- URL List -->

[App Service API CORS]: app-service-api-cors-consume-javascript.md
[Azure-portal]: https://portal.azure.com/
[Dokumentieren Sie DB Java SDK]: ../documentdb/documentdb-java-application.md
[kostenlose Testversion]: https://azure.microsoft.com/pricing/free-trial/
[Git]: http://www.git-scm.com/
[Java Developer Center]: /develop/java/
[Java Developer Kit 8]: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
[JAX-RS]: https://jax-rs-spec.java.net/
[Maven]: https://maven.apache.org/
[Microsoft Azure]: https://azure.microsoft.com/
[Online-Swagger-Editor]: http://editor.swagger.io/
[Postman]: https://www.getpostman.com/
[Storage SDK für Java]: ../storage/storage-java-how-to-use-blob-storage.md
[Stolz]: http://swagger.io/
[Swagger-Editor]: http://editor.swagger.io/
[Visual Studio-Code]: https://code.visualstudio.com

<!-- IMG List -->

[paste-json]: ./media/app-service-api-java-api-app/paste-json.png
[pasted-swagger]: ./media/app-service-api-java-api-app/pasted-swagger.png
[view-swagger-generated-docs]: ./media/app-service-api-java-api-app/view-swagger-generated-docs.png
[generate-code-menu-item]: ./media/app-service-api-java-api-app/generate-code-menu-item.png
[open-contact-model-file]: ./media/app-service-api-java-api-app/open-contact-model-file.png
[open-contact-service-code-file]: ./media/app-service-api-java-api-app/open-contact-service-code-file.png
[run-jetty-war]: ./media/app-service-api-java-api-app/run-jetty-war.png
[calling-contacts-api]: ./media/app-service-api-java-api-app/calling-contacts-api.png
[calling-specific-contact-api]: ./media/app-service-api-java-api-app/calling-specific-contact-api.png
[create-api-app]: ./media/app-service-api-java-api-app/create-api-app.png
[set-up-java]: ./media/app-service-api-java-api-app/set-up-java.png
[deployment-credentials]: ./media/app-service-api-java-api-app/deployment-credentials.png
[select-git-repo]: ./media/app-service-api-java-api-app/select-git-repo.png
[copy-git-repo-url]: ./media/app-service-api-java-api-app/copy-git-repo-url.png
[postman-calling-azure-contacts]: ./media/app-service-api-java-api-app/postman-calling-azure-contacts.png
