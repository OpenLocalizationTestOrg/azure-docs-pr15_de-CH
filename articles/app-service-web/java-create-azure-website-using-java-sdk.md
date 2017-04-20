<properties 
    pageTitle="Erstellen Sie eine Web-Anwendung in Azure App Service mit dem Azure SDK für Java" 
    description="Informationen Sie zum Azure App Service programmgesteuert mithilfe von Azure SDK für Java Web App erstellen." 
    tags="azure-classic-portal"
    services="app-service\web" 
    documentationCenter="Java" 
    authors="donntrenton" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="02/25/2016" 
    ms.author="v-donntr"/>


# <a name="create-a-web-app-in-azure-app-service-using-the-azure-sdk-for-java"></a>Erstellen Sie eine Web-Anwendung in Azure App Service mit dem Azure SDK für Java

<!-- Azure Active Directory workflow is not yet available on the Azure Portal -->

## <a name="overview"></a>Übersicht

In dieser exemplarischen Vorgehensweise wird veranschaulicht, wie ein Azure SDK für Java-Anwendung, eine Web-Anwendung in [Azure App Service][]erstellen und Bereitstellen einer Anwendung. Es besteht aus zwei Teilen:

- Teil 1 veranschaulicht, wie eine Java-Anwendung erstellen, die eine Webanwendung erstellt.
- Teil 2 veranschaulicht das Erstellen eines einfachen "Hello World" JSP-Anwendung und verwenden Sie ein FTP-Client Code App Service bereitstellen.


## <a name="prerequisites"></a>Erforderliche Komponenten

### <a name="software-installations"></a>Software-Installationen

AzureWebDemo Anwendungscode in diesem Artikel wurde mit Azure Java SDK 0.7.0, Installation mit dem [Webplattform-Installer][] (WebPI) geschrieben. Stellen Sie außerdem unbedingt die neueste Version von [Azure-Toolkit für Eclipse][]. Nach der Installation des SDK aktualisieren Abhängigkeiten in Eclipse Projekt **Update Index** **Maven**Repositories und die neueste Version jedes Pakets im Fenster **Abhängigkeiten** erneut. Überprüfen Sie die Version der installierten Software in Eclipse auf **Hilfe > Details zur**; Sie müssen mindestens die folgenden Versionen:

- Paket für Microsoft Azure Bibliotheken für Java 0.7.0.20150309
- Eclipse-IDE für Java EE Entwickler 4.4.2.20150219


### <a name="create-and-configure-cloud-resources-in-azure"></a>Erstellen Sie und konfigurieren Sie in Azure Cloud-Ressourcen

Bevor Sie beginnen, müssen Sie ein aktives Abonnement Azure und Standard Active Directory (AD) in Azure.


### <a name="create-an-active-directory-ad-in-azure"></a>Erstellen Sie Active Directory (AD) in Azure

Wenn Sie nicht bereits eine Active Directory (AD) Ihres Azure-Abonnements verfügen, melden Sie sich bei [Azure-Verwaltungsportal][] mit Ihrem Microsoft-Konto. Haben Sie mehrere Abonnements auf **Abonnements** , und wählen Sie das Standardverzeichnis für das Abonnement für dieses Projekt verwenden möchten. Klicken Sie auf **Übernehmen** , Abonnement-Ansicht wechseln.

1. Wählen Sie im Menü links **Active Directory** . **Klicken Sie auf Neu > Verzeichnis > benutzerdefinierte**.

2. Wählen Sie **Neues Verzeichnis erstellen**, im **Verzeichnis hinzufügen**.

3. Geben Sie im Feld **Name**einen Verzeichnisnamen.

4. **Domäne**Geben Sie einen Domänennamen ein. Dies ist eine grundlegende Domänennamen, der mit Ihrem Verzeichnis enthalten. ist `<domain_name>.onmicrosoft.com`. Sie können Namen basierend auf den Verzeichnisnamen oder einem anderen Domänennamen, die Sie besitzen. Später können Sie einen anderen Domänennamen hinzufügen, die Ihre Organisation bereits verwendet.

5. Gebietsschema wählen Sie **Land oder eine Region**.

Weitere Informationen zu Active Directory finden Sie unter [Azure AD-Verzeichnis][]?


### <a name="create-a-management-certificate-for-azure"></a>Erstellen eines Zertifikats Management für Azure

Azure SDK für Java verwendet Management Zertifikate zur Authentifizierung mit Azure-Abonnements. Dies sind x. 509 v3-Zertifikate eine Clientanwendung authentifizieren, mit denen die Dienst-API im Namen der Abonnementbesitzer Abonnement Ressourcen verwenden.

Der Code in dieser Prozedur verwendet ein selbstsigniertes Zertifikat Azure authentifiziert. Für dieses Verfahren müssen Sie ein Zertifikat erstellen und Laden Sie sie vorher [Azure-Verwaltungsportal][] . Dies umfasst die folgenden Schritte aus:

- Erstellen einer PFX-Datei, die das Clientzertifikat darstellt und lokal speichern.
- Generieren Sie Management-Zertifikat (CER-Datei) aus der PFX-Datei.
- Hochladen der CER-Datei zu Ihrem Azure-Abonnement.
- Konvertieren Sie die PFX-Datei in JKS, da Java dieses Format verwendet, um mithilfe von Zertifikaten authentifiziert.
- Bezieht sich auf die lokale Datei JKS Authentication Code der Anwendung schreiben.

Nach Abschluss dieser Prozedur Zertifikat CER befinden sich in der Azure-Abonnement und JKS Zertifikat auf Ihrem lokalen Laufwerk gespeichert. Weitere Informationen zu Management-Zertifikaten finden Sie unter [Erstellen und Verwaltungszertifikat für Azure hochladen][].


#### <a name="create-a-certificate"></a>Erstellen eines Zertifikats

Erstellen Sie Ihr eigenes selbstsignierte Zertifikat öffnen Sie eine Befehlskonsole auf Ihrem Betriebssystem, und führen Sie die folgenden Befehle.

> **Hinweis:**  Der Computer, auf dem Sie diesen Befehl ausführen, müssen JDK installiert. Der Pfad zu den Keytool hängt auch die Position in der JDK installieren. Weitere Informationen finden Sie in der Online-Dokumentation von Java [Schlüssel und Zertifikat (Keytool)][] .

Die PFX-Datei zu erstellen:

    <java-install-dir>/bin/keytool -genkey -alias <keystore-id>
     -keystore <cert-store-dir>/<cert-file-name>.pfx -storepass <password>
     -validity 3650 -keyalg RSA -keysize 2048 -storetype pkcs12
     -dname "CN=Self Signed Certificate 20141118170652"

Die CER-Datei zu erstellen:

    <java-install-dir>/bin/keytool -export -alias <keystore-id>
     -storetype pkcs12 -keystore <cert-store-dir>/<cert-file-name>.pfx
     -storepass <password> -rfc -file <cert-store-dir>/<cert-file-name>.cer

Wo:

- `<java-install-dir>`ist der Pfad zu dem Verzeichnis, in dem Java installiert.
- `<keystore-id>`Schlüsselspeicher Eintrags-ID ist (z. B. `AzureRemoteAccess`).
- `<cert-store-dir>`der Pfad zu dem Verzeichnis, in dem Zertifikate gespeichert werden soll (z.B. `C:/Certificates`).
- `<cert-file-name>`Der Name der Zertifikatdatei (z. B. `AzureWebDemoCert`).
- `<password>`ist das Kennwort zum Schutz des Zertifikats. Es muss mindestens 6 Zeichen lang sein. Sie können kein Kennwort eingeben, obwohl dies nicht empfohlen wird.
- `<dname>`der x. 500 Distinguished Name Alias zugeordnet werden und wie die Aussteller und Antragsteller Felder das selbstsignierte Zertifikat verwendet.

Weitere Informationen finden Sie unter [Erstellen und Verwaltungszertifikat für Azure hochladen][].


#### <a name="upload-the-certificate"></a>Hochladen des Zertifikats

Um ein selbstsigniertes Zertifikat in Azure hochladen, **die Einstellungsseite im klassischen Portal** , klicken Registerkarte **Verwaltungszertifikate** . Klicken Sie am unteren Rand der Seite **Hochladen** , und navigieren Sie zum Speicherort der erstellten CER-Datei.


#### <a name="convert-the-pfx-file-into-jks"></a>Die PFX-Datei in JKS konvertieren

In der Windows-Befehlszeile (als Administrator ausgeführt), cd in das Verzeichnis mit den Zertifikaten und führen Sie den folgenden Befehl, `<java-install-dir>` ist das Verzeichnis, in dem Sie Java auf Ihrem Computer installiert:

    <java-install-dir>/bin/keytool.exe -importkeystore
     -srckeystore <cert-store-dir>/<cert-file-name>.pfx
     -destkeystore <cert-store-dir>/<cert-file-name>.jks
     -srcstoretype pkcs12 -deststoretype JKS

1. Geben Sie bei Aufforderung das Ziel Schlüsselspeicher Kennwort; das Kennwort für die Datei JKS werden.

2. Geben Sie bei Aufforderung Quelle Schlüsselspeicher Kennwort; Dies ist das Kennwort für die PFX-Datei angegeben.

Die beiden Kennwörter müssen nicht identisch sein. Sie können kein Kennwort eingeben, obwohl dies nicht empfohlen wird.


## <a name="build-a-web-app-creation-application"></a>Erstellen einer Web-Anwendung erstellen

### <a name="create-the-eclipse-workspace-and-maven-project"></a>Eclipse-Arbeitsbereich und Maven-Projekt erstellen

In diesem Abschnitt erstellen Sie einen Arbeitsbereich und eines Maven für das Web app Anwendung mit dem Namen AzureWebDemo.

1. Erstellen Sie ein neues Maven-Projekt. Klicken Sie auf **Datei > Neu > Maven-Projekt**. **Neues Maven-Projekt** **Erstellen Sie ein einfaches Projekt** und wählen Sie **Arbeitsbereich Standardspeicherort verwenden aus**

2. Geben Sie auf der zweiten Seite der **Neuen Maven-Projekt**Folgendes an:

    - Gruppen-ID:`com.<username>.azure.webdemo`
    - Artefakt-ID: AzureWebDemo
    - Version: 0.0.1-SNAPSHOT
    - Verpackung: Glas
    - Name: AzureWebDemo

    Klicken Sie auf **Fertig stellen**.

3. Öffnen Sie das neue Projekt pom.xml Datei im Projekt-Explorer. Wählen Sie die Registerkarte **abhängig** . Da dies ein neues Projekt, sind noch keine Pakete aufgeführt.

4. Öffnen Sie Maven Repositories. **Klicken Sie auf Fenster > Ansicht anzeigen > andere > Maven > Maven Repositories** und klicken Sie auf **OK**. Die Ansicht **Maven Repositories** wird am unteren Rand der IDE angezeigt.

5. **Global Repositories**öffnen, mit der rechten Maustaste des **zentrale** Repository und wählen Sie **Index neu erstellen**.

    ![][1]
    
    Dieser Schritt kann mehrere Minuten je nach der Geschwindigkeit der Verbindung. Wenn der Index neu erstellt wird, sollte Microsoft Azure Pakete in der **zentralen** Maven Repository angezeigt werden.

6. **Abhängigkeiten**klicken Sie auf **Hinzufügen**. Geben Sie in **Geben Sie Gruppe ID...** `azure-management`. Wählen Sie die Pakete für Basis- und App Service Web Apps-Management:

        com.microsoft.azure  azure-management
        com.microsoft.azure  azure-management-websites

    > **Hinweis:** Wenn Sie die Abhängigkeiten nach dem Release einer neuen Version aktualisieren, müssen Sie jede Abhängigkeit in dieser Liste wieder hinzufügen.
    > Klicken Sie auf **Hinzufügen** und wählen Sie jede einzelne Abhängigkeit wird mit der neuen Versionsnummer in der Liste der **Abhängigkeiten** angezeigt.

Klicken Sie auf **OK**. Azure Pakete werden dann in der Liste der **Abhängigkeiten** angezeigt.


### <a name="writing-java-code-to-create-a-web-app-by-calling-the-azure-sdk"></a>Java Web App erstellen, indem Sie das Azure SDK Code schreiben

Schreiben Sie den Code, APIs in Azure SDK für Java App Service Web app erstellen.

1. Erstellen Sie eine Java-Klasse Haupteintrag punktcode enthalten. Im Projekt-Explorer mit der rechten Maustaste auf den Projektknoten, und wählen Sie **Neu > Klasse**.

2. Geben Sie **Neue Java-Klasse**und der Klasse `WebCreator` und aktivieren das Kontrollkästchen **public static void Main** . Die Auswahl sollte wie folgt aussehen:

    ![][2]

3. Klicken Sie auf **Fertig stellen**. Die Datei WebCreator.java wird im Projektmappen-Explorer angezeigt.


### <a name="calling-the-azure-api-to-create-an-app-service-web-app"></a>Aufrufen der Azure-API zum Erstellen einer App Service Web App


#### <a name="add-necessary-imports"></a>Hinzufügen der erforderlichen Importe

Fügen Sie in WebCreator.java die folgenden Einfuhren; Diese Einfuhren ermöglichen den Zugriff auf Klassen in den Bibliotheken Management Nutzung der Azure-APIs:

    // General imports
    import java.net.URI;
    import java.util.ArrayList;
    
    // Imports for Exceptions
    import java.io.IOException;
    import java.net.URISyntaxException;
    import javax.xml.parsers.ParserConfigurationException;
    import com.microsoft.windowsazure.exception.ServiceException;
    import org.xml.sax.SAXException;
    
    // Imports for Azure App Service management configuration
    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.management.configuration.ManagementConfiguration;
    
    // Service management imports for App Service Web Apps creation
    import com.microsoft.windowsazure.management.websites.*;
    import com.microsoft.windowsazure.management.websites.models.*;
    
    // Imports for authentication
    import com.microsoft.windowsazure.core.utils.KeyStoreType;


#### <a name="define-the-main-entry-point-class"></a>Definieren Sie die wichtigsten Einstiegspunktklasse

Ist der Zweck der AzureWebDemo-Anwendung eine App Service Web App erstellen, benennen Sie die Hauptklasse für diese Anwendung `WebAppCreator`. Diese Klasse stellt den Haupteintrag zeigen Code, der Azure-Dienst-API Web app erstellen.

Fügen Sie die folgenden Parameterdefinitionen für WebApp und Webspace. Sie müssen Ihre eigenen Azure-Abonnement-ID und Zertifikat angeben.

    public class WebAppCreator {
    
        // Parameter definitions used for authentication.
        private static String uri = "https://management.core.windows.net/";
        private static String subscriptionId = "<subscription-id>";
        private static String keyStoreLocation = "<certificate-store-path>";
        private static String keyStorePassword = "<certificate-password>";
    
        // Define web app parameter values.
        private static String webAppName = "WebDemoWebApp";
        private static String domainName = ".azurewebsites.net";
        private static String webSpaceName = WebSpaceNames.WESTUSWEBSPACE;
        private static String appServicePlanName = "WebDemoAppServicePlan";

Wo:

- `<subscription-id>`ist die Azure-Abonnement-ID die Ressource erstellt werden soll.
- `<certificate-store-path>`ist der Pfad und Dateiname der Datei JKS im Verzeichnis lokalen Zertifikats. Beispielsweise `C:/Certificates/CertificateName.jks` für Linux und `C:\Certificates\CertificateName.jks` für Windows.
- `<certificate-password>`ist das Erstellung das JKS Zertifikat angegebene Kennwort.
- `webAppName`kann einen beliebigen Namen wählen Sie; Diese Prozedur verwendet den Namen `WebDemoWebApp`. Der vollständige Domänenname der `webAppName` mit dem `domainName` angefügt, in diesem Fall die vollständige Domäne ist `webdemowebapp.azurewebsites.net`.
- `domainName`muss angegeben werden, wie oben dargestellt.
- `webSpaceName`sollte einer der Werte in der [WebSpaceNames][] -Klasse definiert.
- `appServicePlanName`muss angegeben werden, wie oben dargestellt.

> **Hinweis:** Jedem dieser Anwendung ausführen, müssen Sie den Wert ändern `webAppName` und `appServicePlanName` (oder Web app Azure-Portal löschen) vor der Ausführung der Anwendung. Ausführung fehl, da dieselbe Ressource auf Azure bereits.


#### <a name="define-the-web-creation-method"></a>Definieren Sie die Webmethode erstellen

Anschließend definieren Sie eine Methode zum Web app erstellen. Diese Methode `createWebApp`, gibt der Web app und die Webspace. Außerdem erstellt und der App Service Web Apps Management Client, das [WebSiteManagementClient][] -Objekt definiert. Der Management-Client ist Schlüssel zum Web Apps erstellen. Es bietet RESTful Webdiensten, die Verwalten von webapps (Vorgänge wie erstellen, Update und Delete) Servicemanagement-API aufrufen können.

    private static void createWebApp() throws Exception {

        // Specify configuration settings for the App Service management client.
        Configuration config = ManagementConfiguration.configure(
            new URI(uri),
            subscriptionId,
            keyStoreLocation,  // Path to the JKS file
            keyStorePassword,  // Password for the JKS file
            KeyStoreType.jks   // Flag that you are using a JKS keystore
        );

        // Create the App Service Web Apps management client to call Azure APIs
        // and pass it the App Service management configuration object.
        WebSiteManagementClient webAppManagementClient = WebSiteManagementService.create(config);

        // Create an App Service plan for the web app with the specified parameters.
        WebHostingPlanCreateParameters appServicePlanParams = new WebHostingPlanCreateParameters();
        appServicePlanParams.setName(appServicePlanName);
        appServicePlanParams.setSKU(SkuOptions.Free);
        webAppManagementClient.getWebHostingPlansOperations().create(webSpaceName, appServicePlanParams);

        // Set webspace parameters.
        WebSiteCreateParameters.WebSpaceDetails webSpaceDetails = new WebSiteCreateParameters.WebSpaceDetails();
        webSpaceDetails.setGeoRegion(GeoRegionNames.WESTUS);
        webSpaceDetails.setPlan(WebSpacePlanNames.VIRTUALDEDICATEDPLAN);
        webSpaceDetails.setName(webSpaceName);

        // Set web app parameters.
        // Note that the server farm name takes the Azure App Service plan name.
        WebSiteCreateParameters webAppCreateParameters = new WebSiteCreateParameters();
        webAppCreateParameters.setName(webAppName);
        webAppCreateParameters.setServerFarm(appServicePlanName);
        webAppCreateParameters.setWebSpace(webSpaceDetails);

        // Set usage metrics attributes.
        WebSiteGetUsageMetricsResponse.UsageMetric usageMetric = new WebSiteGetUsageMetricsResponse.UsageMetric();
        usageMetric.setSiteMode(WebSiteMode.Basic);
        usageMetric.setComputeMode(WebSiteComputeMode.Shared);

        // Define the web app object.
        ArrayList<String> fullWebAppName = new ArrayList<String>();
        fullWebAppName.add(webAppName + domainName);
        WebSite webApp = new WebSite();
        webApp.setHostNames(fullWebAppName);

        // Create the web app.
        WebSiteCreateResponse webAppCreateResponse = webAppManagementClient.getWebSitesOperations().create(webSpaceName, webAppCreateParameters);

        // Output the HTTP status code of the response; 200 indicates the request succeeded; 4xx indicates failure.
        System.out.println("----------");
        System.out.println("Web app created - HTTP response " + webAppCreateResponse.getStatusCode() + "\n");

        // Output the name of the web app that this application created.
        String shinyNewWebAppName = webAppCreateResponse.getWebSite().getName();
        System.out.println("----------\n");
        System.out.println("Name of web app created: " + shinyNewWebAppName + "\n");
        System.out.println("----------\n");
    }

Der Code Ausgabe den HTTP-Status der Antwort erfolgreich war oder nicht und wenn erfolgreich, gibt den Namen der erstellten Web-app.


#### <a name="define-the-main-method"></a>Definieren der Main()-Methode

Geben Sie den main()-Methode Code, createWebApp() Web app erstellen.

Schließlich rufen `createWebApp` von `main`:

        public static void main(String[] args)
            throws IOException, URISyntaxException, ServiceException,
            ParserConfigurationException, SAXException, Exception {

            // Create web app
            createWebApp();

        }  // end of main()

    }  // end of WebAppCreator class


#### <a name="run-the-application-and-verify-web-app-creation"></a>Führen Sie die Anwendung und überprüfen, ob Web app erstellt

Um sicherzustellen, dass die Anwendung ausgeführt wird, klicken Sie auf **Ausführen > Ausführen**. Nach Abschluss die Anwendung ausführen, sollte die folgende Ausgabe in der Konsole Eclipse angezeigt werden:

    ----------
    Web app created - HTTP response 200
    
    ----------
    
    Name of web app created: WebDemoWebApp
    
    ----------

Klassischen Azure-Portal melden Sie an und auf **Web Apps**. Neue Web app sollte innerhalb weniger Minuten in Web Apps-Liste angezeigt werden.


## <a name="deploying-an-application-to-the-web-app"></a>Bereitstellung einer Anwendung für das Web App

Nach dem AzureWebDemo ausführen und die neue Web App klassische Portal anmelden **Web Apps**und auf **WebDemoWebApp** in der Liste **Web Apps** auswählen. Klicken Sie in der Web-app Dashboardseite auf **Durchsuchen** (oder klicken Sie auf die URL `webdemowebapp.azurewebsites.net`) navigieren. Sie werden eine leere Platzhalter angezeigt, da noch keine Inhalte Web App veröffentlicht wurde.

Neben "Hello World"-Anwendung erstellen und auf der Webanwendung bereitstellen.


### <a name="create-a-jsp-hello-world-application"></a>Erstellen einer JSP Hello World-Anwendung

#### <a name="create-the-application"></a>Erstellen der Anwendung

Um der Verwendung eine Anwendung im Web bereitstellen veranschaulicht folgt eine einfache "Hello World" Java-Anwendung erstellen und Hochladen der App Service Web App, die die Anwendung erstellt.

1. Klicken Sie auf **Datei > Neu > dynamische Webprojekt**. Nennen Sie sie `JSPHello`. Sie müssen keine anderen Einstellungen in diesem Dialogfeld ändern. Klicken Sie auf **Fertig stellen**.

    ![][3]

2. Im Projekt-Explorer erweitern Sie das Projekt **JSPHello** , Maustaste auf **Inhaltsordner**Sie **Neu > JSP-Datei**. Im Dialogfeld neue JSP-Datei den Dateinamen der neuen `index.jsp`. Klicken Sie auf **Weiter**.

3. **JSP-Vorlage auswählen** -Dialogfeld **Neue JSP-Datei (html)** und klicken Sie auf **Fertig stellen**.

4. In index.jsp, fügen Sie den folgenden Code in die `<head>` und `<body>` tag Abschnitte:

        <head>
          ...
          java.util.Date date = new java.util.Date();
        </head>
    
        <body>
          Hello, the time is <%= date %> 
        </body>


#### <a name="run-the-hello-world-application-in-localhost"></a>Hello World-Anwendung localhost ausführen

Bevor Sie diese Anwendung ausführen, müssen Sie einige Eigenschaften.

1. Mit der rechten Maustaste des **JSPHello** -Projekts, und wählen Sie **Eigenschaften**.

2. Im Dialogfeld **Eigenschaften** : **Java Pfad erstellen**, wählen Sie die Registerkarte **Reihenfolge und Exportieren** , **JRE Systembibliothek**überprüfen, klicken Sie auf **oben** Verschieben an den Anfang der Liste.

    ![][4]

3. Auch im Dialogfeld **Eigenschaften** : **Ziel Laufzeiten** wählen und auf **neu**.

4. Klicken Sie im Dialogfeld **Neue Server Runtime Environment** wählen Sie wie **7.0 Apache Tomcat** Server aus und auf **Weiter**. Legen Sie im Dialogfeld **Tomcat Server** **Name** `Apache Tomcat v7.0`, und **Tomcat-Verzeichnis** in das Verzeichnis, in dem Tomcat Server soll-Version installiert.

    ![][5]

    Klicken Sie auf **Fertig stellen**.

5. Sie geben die **Laufzeiten Ziel** Seite des Dialogfelds **Eigenschaften** zurück. **Apache Tomcat 7.0**, klicken Sie auf **OK**.

    ![][6]

6. Klicken Sie Eclipse **Ausführen** , im Menü **Ausführen**. Wählen Sie im Dialogfeld **Ausführen als** **auf Server ausführen**. Wählen Sie im Dialogfeld **Ausführen auf Server** **7.0 Tomcat Server**:

    ![][7]

    Klicken Sie auf **Fertig stellen**.

7. Beim Ausführen der Anwendung müsste die **JSPHello** Seite in einem Fenster Localhost in Eclipse (`http://localhost:8080/JSPHello/`), die folgende Meldung angezeigt:

    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`


#### <a name="export-the-application-as-a-war"></a>Eine Anwendung exportieren

Exportieren Sie die Webprojektdateien als eine Web-Archivdatei (WAR), damit Sie auf die Webanwendung bereitstellen. Web-Projektdateien befinden sich im Ordner Inhaltsordner:

    META-INF
    WEB-INF
    index.jsp

1. Mit der rechten Maustaste des Inhaltsordner Ordners, und wählen Sie **Exportieren**.

2. Klicken Sie im Dialogfeld **Exportieren wählen** auf **Web > WAR** Datei und dann auf **Weiter**.

3. Wählen Sie im Dialogfeld **Export WAR** Src-Verzeichnis im aktuellen Projekt aus und Namen Sie den der WAR-Datei am Ende. Zum Beispiel:

    `<project-path>/JSPHello/src/JSPHello.war`

Weitere Informationen zum Bereitstellen der WAR-Dateien finden Sie unter [Hinzufügen einer Java-Anwendung auf Azure App Service Web Apps](web-sites-java-add-app.md).


### <a name="deploying-the-hello-world-application-using-ftp"></a>Hello World-Anwendung mithilfe von FTP bereitstellen

Wählen Sie einen Fremdanbieter-FTP-Client an die Anwendung. Zwei Optionen beschrieben: die Kudu-Konsole integriert Azure; und FileZilla beliebte Werkzeug mit einer geeigneten grafisch Benutzeroberfläche.

> **Hinweis:** Azure-Toolkit für Eclipse Bereitstellung Cloud-Dienste und Speicherkonten unterstützt jedoch unterstützt derzeit keine Bereitstellung auf Web-apps. Sie können Speicherkonten und Cloud-Diensten mit einer Azure-Bereitstellungsprojekt Siehe [Erstellen einer Hello World-Anwendung für Azure in Eclipse](http://msdn.microsoft.com/library/azure/hh690944.aspx)jedoch nicht webapps bereitstellen. Verwenden Sie andere Methoden wie FTP oder GitHub, um Dateien auf Ihrer Anwendung zu übertragen.

> **Hinweis:** Wir empfehlen nicht über FTP über die Windows-Befehlszeile (das FTP.EXE Befehlszeilenprogramm, das mit Windows geliefert wird). FTP-Clients active FTP wie FTP.EXE verwenden, oft nicht über Firewalls hinweg verwendet. Aktive FTP-Verbindung gibt eine interne LAN-basierte Adresse, FTP-Server wahrscheinlich fehl, eine Verbindung herzustellen.

Weitere Informationen zur Bereitstellung einer App Service Web App über FTP finden Sie unter den folgenden Themen:

- [Bereitstellung mit einem FTP-Dienstprogramm](web-sites-deploy.md)


#### <a name="set-up-deployment-credentials"></a>Anmeldeinformationen für die Bereitstellung einrichten

Sicherstellen Sie, dass das Ausführen der Anwendung **AzureWebDemo** Web app erstellen. Sie überträgt Dateien an diesem Speicherort.

1. Klassische Portal melden Sie an und auf **Web Apps**. Stellen Sie sicher, dass **WebDemoWebApp** in der Liste der Web-apps, und ausgeführt wird. Klicken Sie auf **WebDemoWebApp** um die **Dashboard** -Seite zu öffnen.

2. Klicken Sie unter **Kurzer Blick**auf **der Dashboardseite** auf **Ihre Bereitstellung Anmeldeinformationen eingerichtet** (wenn noch Bereitstellung Anmeldeinformationen liest **die Bereitstellung Anmeldeinformationen zurücksetzen**).

    Anmeldeinformationen für die Bereitstellung sind ein Microsoft-Konto zugeordnet. Sie müssen einen Benutzernamen und Kennwort mithilfe von Git und FTP. Diese Anmeldeinformationen können für jede Web-app in allen Azure-Abonnements mit Ihrem Microsoft-Konto verbundene bereitstellen. Git und FTP-Bereitstellung Anmeldeinformationen im Dialogfeld, und zeichnen Sie den Benutzernamen und das Kennwort für die zukünftige Verwendung.


#### <a name="get-ftp-connection-information"></a>FTP-Verbindung Informationen

Um FTP zu Anwendungsdateien zu neu erstellten Web app bereitstellen, müssen Sie Verbindungsinformationen. Gibt es zwei Methoden, um Informationen zu erhalten. Eine Möglichkeit ist das Web app **Dashboard** -Seite. umgekehrt ist zum download der app Profil veröffentlichen. Veröffentlichen ist eine XML-Datei, die Informationen wie FTP-Host und die Anmeldeinformationen für Ihre Web-apps in Azure App Service bereitstellt. Dieser Benutzername und das Kennwort können Sie für jede Web-app in alle Abonnements der Azure-Konto nicht nur zugeordnete bereitstellen.

FTP-Informationen aus dem Web app Blade in [Azure-Portal][]zu erhalten:

1. Suchen Sie unter **Essentials**und kopieren Sie der **FTP-Hostname**. Dies ist ein URI wie `ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net`.

2. Unter **Essentials**finden und **FTP-Bereitstellung Benutzername**kopieren. Dies ist der Form *Webappname\deployment Benutzername*. beispielsweise `WebDemoWebApp\deployer77`.

Das Veröffentlichungsprofil FTP-Informationen erhältlich:

1. Klicken Sie auf **Get Profil veröffentlichen**im Web app Blade. Dadurch wird eine PUBLISHSETTINGS-Datei auf Ihrem lokalen Laufwerk herunterladen

2. Die PUBLISHSETTINGS-Datei in einem XML-Editor oder Text-Editor, und suchen Sie die `<publishProfile>` mit `publishMethod="FTP"`. Es sollte wie folgt aussehen:

        <publishProfile
            profileName="WebDemoWebApp - FTP"
            publishMethod="FTP"
            publishUrl="ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net/site/wwwroot"
            ftpPassiveMode="True"
            userName="WebDemoWebApp\$WebDemoWebApp"
            userPWD="<deployment-password>"
            ...
        </publishProfile>

3. Beachten Sie, dass der Web app `publishProfile` Settings FileZilla Site Manager Settings wie folgt zuordnen:

- `publishUrl`entspricht der **FTP-Hostname** **Host**festgelegte Wert.
- `publishMethod="FTP"`bedeutet, dass **FTP - File Transfer Protocol**und **Verschlüsselung** mit **einfachen FTP** **Protokoll** fest.
- `userName`und `userPWD` sind für die tatsächliche Benutzername und Kennwortwerte beim Zurücksetzen der Bereitstellung Anmeldeinformationen angegeben. `userName`entspricht dem **Bereitstellung / FTP-Benutzer**. Sie ordnen Sie **Benutzer** und **Kennwort** in FileZilla.
- `ftpPassiveMode="True"`bedeutet, dass die FTP-Site passive FTP-Übertragung verwendet. Wählen Sie **passiven** , auf das **Übertragen** .


#### <a name="configure-the-web-app-to-host-a-java-application"></a>Konfigurieren der Web App eine Java-Anwendung hosten

Bevor Sie die Anwendung veröffentlichen, müssen Sie einige Konfigurationen ändern, sodass die Web app eine Java-Anwendung hosten kann.

1. Im klassischen Portal zum **Web app Dashboardseite** , und klicken Sie auf **Konfigurieren**. Geben Sie Folgendes auf der Seite **Konfigurieren** .

2. In **Java-Version** ist standardmäßig **deaktiviert**. Wählen Sie der Java-Version die Anwendung entwickelt. beispielsweise 1.7.0_51. Danach, stellen Sie außerdem sicher, dass eine Version der Tomcat Server **Webcontainer** festgelegt ist.

3. **Default Documents**index.jsp hinzufügen und an den Anfang der Liste verschieben. (Die Standarddatei für Web-apps ist hostingstart.html.)

4. Klicken Sie auf **Speichern**.


#### <a name="publish-your-application-using-kudu"></a>Veröffentlichen Sie die Anwendung mit Kudu

Eine Möglichkeit, die Anwendung zu veröffentlichen ist mit Kudu der Debug-Konsole in Azure integriert. Kudu ist bekanntermaßen stabilen und konsistenten App Service Web Apps mit Tomcat Server. Sie öffnen die Konsole für die Web-app unter eine URL im folgenden Format:

`https://<webappname>.scm.azurewebsites.net/DebugConsole`

1. Kudu-Konsole befindet sich für dieses Verfahren unter der folgenden URL; Wechseln Sie zu diesem Speicherort:

    `https://webdemowebapp.scm.azurewebsites.net/DebugConsole`

2. Wählen Sie im oberen Menü **Debug-Konsole > CMD**.

3. Navigieren Sie in der Befehlszeile der Konsole zu `/site/wwwroot` (oder klicken Sie auf `site`, dann `wwwroot` im Verzeichnis anzeigen am oberen Rand der Seite):

    `cd /site/wwwroot`

4. Nachdem Sie **Java-Version**angeben, sollten Tomcat Server Webapps Verzeichnis erstellen. Navigieren Sie in der Befehlszeile der Konsole Webapps-Verzeichnis:

    `mkdir webapps`

    `cd webapps`

5. Ziehen Sie die JSPHello.war von `<project-path>/JSPHello/src/` in der Verzeichnisansicht Kudu unter `/site/wwwroot/webapps`. Nicht ziehen Sie es in den Bereich "Hier ziehen zu hochladen komprimieren" da Tomcat Entpacken wird.

  ![][8]

Am ersten JSPHello.war wird im Verzeichnis selbst angezeigt:

  ![][9]

Kurzfristig (wahrscheinlich weniger als 5 Minuten) wird Tomcat Server die WAR-Datei in einem entpackt JSPHello Verzeichnis entpacken. Klicken Sie auf das Stammverzeichnis um festzustellen, ob index.jsp entpackt und kopiert wurde. Dann navigieren Sie zurück zum Verzeichnis Webapps um festzustellen, ob das entpackte JSPHello Verzeichnis erstellt wurde. Wenn diese Elemente nicht angezeigt wird, warten und wiederholen.

  ![][10]


#### <a name="publish-your-application-using-filezilla-optional"></a>Veröffentlichen Sie die Anwendung mit FileZilla (optional)

Ein anderes Tool, mit dem Sie die Anwendung veröffentlichen ist FileZilla eine beliebte Drittanbieter-FTP-Client mit einer geeigneten grafisch Benutzeroberfläche. Sie können herunterladen und Installieren von FileZilla aus [http://filezilla-project.org/](http://filezilla-project.org/) Wenn Sie bereits es keinen. Weitere Informationen zum Verwenden des Clients finden Sie unter [FileZilla Dokumentation](https://wiki.filezilla-project.org/Documentation) und diesen Blog-Eintrag auf [FTP-Clients - Teil 4: FileZilla](http://blogs.msdn.com/b/robert_mcmurray/archive/2008/12/17/ftp-clients-part-4-filezilla.aspx).

1. FileZilla, klicken Sie auf **Datei > Site-Manager**.
2. Klicken Sie im Dialogfeld **Site Manager** auf **Neue Website**. Eine neue leere FTP-Site erscheint im **Eintrag wählen** Sie aufgefordert werden, einen Namen anzugeben. Für dieses Verfahren Namen `AzureWebDemo-FTP`.

    Geben Sie auf der Registerkarte **Allgemein** Folgendes ein:
    - **Host:** Geben Sie den **FTP-Hostnamen** , die im Dashboard kopiert.
    - **Anschluss:** (Lassen Sie diese leer, eine Übertragung Passiv und Server bestimmt den Port verwendet.)
    - **Protokoll:** FTP-File Transfer Protocol
    - **Verschlüsselung:** Einfaches FTP verwenden
    - **Anmeldung Typ:** Normal
    - **Benutzer:** Geben Sie die Bereitstellung / FTP Benutzer aus dem Dashboard kopieren. Dies ist der vollständige FTP-Benutzername, hat die Form *Webappname\username*.
    - **Kennwort:** Geben Sie das Kennwort, das Sie beim Festlegen der Bereitstellung Anmeldeinformationen angegeben.

    Wählen Sie auf der Registerkarte **Einstellungen übertragen** **Passiv**.

3. Klicken Sie auf **Verbinden**. Wenn erfolgreich, FileZilla Konsole angezeigt wird ein `Status: Connected` Nachricht und Problem eine `LIST` Befehl den Verzeichnisinhalt aufzuführen.

4. Wählen Sie im Bereich **lokale** Website das Quellverzeichnis in dem Datei JSPHello.war befindet; der Pfad werden ähnlich der folgenden:

    `<project-path>/JSPHello/src/`

5. Wählen Sie im Bedienfeld **Remote** Zielordner. Stellen Sie bereit WAR-Datei in der `webapps` Verzeichnis Web app Root. Navigieren Sie zu `/site/wwwroot`, mit der rechten Maustaste auf `wwwroot`, und wählen Sie **Erstellen**. Benennen Sie das Verzeichnis `webapps` , und geben Sie das Verzeichnis.

6. Übertragen von JSPHello.war auf `/site/wwwroot/webapps`. Wählen Sie JSPHello.war in der **lokalen** Liste, mit der rechten Maustaste darauf und **Hochladen**. Sollte angezeigt werden, erscheinen Sie in `/site/wwwroot/webapps`.

7. Nach dem Kopieren von JSPHello.war in das Verzeichnis Webapps Tomcat Server automatisch entpackt (Extrahieren) Dateien in die WAR-Datei. Obwohl Tomcat Server sofort Entpacken beginnt, es möglicherweise lange Zeit (möglicherweise Stunden) für die Dateien im FTP-Client angezeigt werden.


#### <a name="run-the-hello-world-application-on-the-web-app"></a>Führen Sie die Hello World-Anwendung auf Web App

1. Nachdem Sie die WAR-Datei hochgeladen und sichergestellt, dass Tomcat Server eine entpackte erstellt hat `JSPHello` Verzeichnis durchsuchen, `http://webdemowebapp.azurewebsites.net/JSPHello` , die Anwendung auszuführen.

    > **Hinweis:** Wenn Sie **Durchsuchen** aus dem Verwaltungsportal klicken, erhalten Sie die Webseite standardmäßig sagen "Diese Java-basierten Web-Anwendung erfolgreich erstellt wurde." Möglicherweise müssen die Webseite aktualisieren, um die Anwendungsausgabe anstelle der Standard-Webseite anzeigen.

2. Wenn die Anwendung ausgeführt wird, erhalten Sie eine Webseite mit der folgenden Ausgabe:

    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`


#### <a name="clean-up-azure-resources"></a>Azure Ressourcen bereinigen

Diese Prozedur erstellt eine App Service Web app. Sie werden für die Ressource berechnet als vorhanden. Wenn Sie weiterhin Web app für Test- oder Entwicklungsaufgaben verwenden möchten, sollten Sie beenden oder löschen. Eine Webanwendung, die angehalten wurde Gebühr noch entstehen, aber Sie können jederzeit starten. Web app löschen Löscht alle Daten darauf geuploadeten.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

  [1]: ./media/java-create-azure-website-using-java-sdk/eclipse-maven-repositories-rebuild-index.png
  [2]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-java-class.png
  [3]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-dynamic-web-project.png
  [4]: ./media/java-create-azure-website-using-java-sdk/eclipse-java-build-path.png
  [5]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-tomcat-server.png
  [6]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-properties-page.png
  [7]: ./media/java-create-azure-website-using-java-sdk/eclipse-run-on-server.png
  [8]: ./media/java-create-azure-website-using-java-sdk/kudu-console-drag-drop.png
  [9]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-1.png
  [10]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-2.png
 

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Webplattform-Installer]: http://go.microsoft.com/fwlink/?LinkID=252838
[Azure-Toolkit für Eclipse]: https://msdn.microsoft.com/library/azure/hh690946.aspx
[Azure-Verwaltungsportal]: https://manage.windowsazure.com
[Was ist ein Azure AD-Verzeichnis]: http://technet.microsoft.com/library/jj573650.aspx
[Erstellen Sie und Hochladen Sie Verwaltungszertifikat für Azure]: ../cloud-services/cloud-services-certs-create.md
[Schlüssel und Zertifikat-Verwaltungstool (Keytool)]: http://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html
[WebSiteManagementClient]: http://azure.github.io/azure-sdk-for-java/com/microsoft/azure/management/websites/WebSiteManagementClient.html
[WebSpaceNames]: http://dl.windowsazure.com/javadoc/com/microsoft/windowsazure/management/websites/models/WebSpaceNames.html
[Azure-Portal]: https://portal.azure.com
