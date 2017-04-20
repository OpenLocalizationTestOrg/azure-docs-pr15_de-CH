<properties
   pageTitle="Migrieren und veröffentlichen eine Web-Anwendung ein Azure-Cloud-Dienst von Visual Studio | Microsoft Azure"
   description="Informationen Sie zum Migrieren und veröffentlichen Ihre Web-Anwendung ein Azure-Cloud-Dienst mithilfe von Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="how-to-migrate-and-publish-a-web-application-to-an-azure-cloud-service-from-visual-studio"></a>Gewusst wie: Migrieren und Veröffentlichen einer Webanwendung ein Azure-Cloud-Dienst von Visual Studio

Um die Hostingdienste und Erweiterbarkeit Azure nutzen möchten Sie migrieren, und veröffentlichen Sie die Webanwendung ein Azure-Cloud-Dienst. Sie können eine Anwendung in Azure mit minimaler Ihrer vorhandenen Anwendung ausführen.

>[AZURE.NOTE] Dieses Thema wird Cloud-Dienste nicht auf Websites bereitstellen. Informationen zum Bereitstellen von Websites finden Sie unter [Bereitstellen einer Web-app in Azure App Service](./app-service-web/web-sites-deploy.md).

Eine Liste bestimmter Vorlagen für Visual C# und Visual Basic unterstützt werden, finden Sie im Abschnitt **Vorlagen unterstützt** weiter unten in diesem Thema.

Sie müssen zuerst Ihre Web-Anwendung für Azure aus Visual Studio aktivieren. Die folgende Abbildung zeigt die wichtigsten Schritte die vorhandenen Anwendung veröffentlichen von Azure-Projekt für Bereitstellung hinzufügen. Dieser Prozess hinzugefügt der Projektmappe ein Azure-Projekt mit der erforderlichen Web. Basierend auf dem Typ des Webprojekts, die Projekteigenschaften für Assemblys werden auch erfordert das Service-Paket zusätzliche Assemblys für die Bereitstellung.

![Eine Web-Anwendung in Microsoft Azure veröffentlichen](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC748917.png)

>[AZURE.NOTE] **Konvertieren**, Befehl **in Azure Cloud Service Project konvertieren** ist nur für das Webprojekt in der Projektmappe angezeigt. Beispielsweise ist der Befehl nicht für ein Silverlight-Projekt in der Projektmappe verfügbar.
Wenn ein Servicepaket erstellen oder veröffentlichen Sie die Anwendung in Azure können Warn- oder Fehlermeldungen auftreten. Diese Warnung und Fehler können Sie beheben, bevor Sie in Azure bereitstellen. Beispielsweise erhalten Sie eine Warnung über fehlende Assembly. Weitere Informationen dazu, wie Sie alle Warnungen als Fehler behandeln, finden Sie unter [Konfigurieren einer Azure Cloud Service Project mit Visual Studio](vs-azure-tools-configuring-an-azure-project.md). Wenn die Anwendung erstellen, führen Sie ihn lokal Serveremulator oder Azure veröffentlichen möglicherweise die folgende Fehlermeldung im Fenster **Fehlerliste** angezeigt: **der angegebene Pfad, Dateiname oder beide sind zu lang**. Dieser Fehler tritt auf, weil den vollqualifizierten Azure Projektnamen zu lang dauert. Die Länge der Name des Projekts, einschließlich des vollständigen Pfades darf nicht mehr als 146 Zeichen sein. Dies ist z. B. der Gesamtprojekt Name einschließlich Pfad für ein Azure Projekt für eine Silverlight-Anwendung erstellt: `c:\users\<user name>\documents\visual studio 2015\Projects\SilverlightApplication4\SilverlightApplication4.Web.Azure.ccproj`. Möglicherweise wird die Lösung in ein anderes Verzeichnis verschieben, die einen kürzeren Pfad, den vollqualifizierten Namen kürzen.

Migrieren von Visual Studio eine Web-Anwendung in Azure veröffentlichen und folgen

## <a name="enable-a-web-application-for-deployment-to-azure"></a>Aktivieren Sie eine Anwendung für die Bereitstellung in Azure

### <a name="to-enable-a-web-application-for-deployment-to-azure"></a>So aktivieren Sie eine Webanwendung für die Bereitstellung in Azure

1. Um die Webanwendung für die Bereitstellung in Azure zu aktivieren, öffnen Sie das Kontextmenü für ein Webprojekt in der Projektmappe, und wählen Sie Azure-Bereitstellungsprojekt hinzufügen.

    Die folgenden Aktionen ausgeführt:

    - Ein Azure-Projekt namens `<name of the web project>.Azure` Lösung für Ihre Anwendung hinzugefügt wird.

    - Azure-Projekt eine Webrolle für das Webprojekt hinzugefügt.

    - Die Eigenschaft **Lokale Kopie** wird festgelegt auf true für alle Assemblys, die für MVC 2 MVC 3 MVC 4 und Silverlight Business Applications. Das Service-Paket für die Bereitstellung verwendet hinzugefügt diese Assemblys.

  >[AZURE.IMPORTANT] Haben Sie andere Assemblys und Dateien, die für diese Webanwendung erforderlich sind, müssen Sie die Eigenschaften für diese Dateien manuell. Informationen zum Festlegen dieser Eigenschaften finden Sie im Abschnitt **Dateien im Leistungspaket enthalten** in diesem Artikel.

  >[AZURE.NOTE] Wenn eine Webrolle für ein bestimmtes Webprojekt in Azure-Projekt in der Projektmappe **Konvertieren**bereits wird nicht **in Azure Cloud Service Project konvertieren** im Kontextmenü für das Webprojekt angezeigt.

  Wenn Sie mehrere Webprojekte in Webtests und Webrollen für jedes Webprojekt erstellen möchten, führen Sie die Schritte in dieser Prozedur für jedes Webprojekt. Separate Azure Projekte für jede Webrolle erstellt. Jedes Webprojekt kann einzeln veröffentlicht. Alternativ können Sie manuell einen anderen Webrolle ein vorhandenes Azure-Projekt in der Webanwendung hinzufügen. Hierzu öffnen Sie das Kontextmenü für den Ordner **Rollen** in Azure-Projekts, wählen Sie **Hinzufügen**und **Rolle Webprojekt in Lösung**wählen Sie das Projekt als eine Webrolle hinzufügen und wählen Sie dann **OK** .

## <a name="use-an-azure-sql-database-for-your-application"></a>Verwenden einer Azure SQL-Datenbank für die Anwendung

Haben Sie eine Verbindungszeichenfolge für die Webanwendung, die eine SQL Server-Datenbank verwendet, der auf dem Gelände, müssen Sie diese Verbindungszeichenfolge eine Instanz der SQL-Datenbank hostet Azure stattdessen ändern.

>[AZURE.IMPORTANT] Ihr Abonnement müssen Sie SQL-Datenbank. Wenn Sie Ihr Abonnement über das [klassische Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885)zugreifen, können Sie ermitteln, welche Dienste Ihr Abonnement enthält. Folgende Hinweise gelten für freigegebene [Azure-Verwaltungsportal](http://go.microsoft.com/fwlink/?LinkID=213885). Bei Verwendung der [Azure-Portal](http://portal.microsoft.com), fahren Sie mit der nächsten Prozedur.

### <a name="to-use-a-sql-database-instance-in-your-web-role-for-your-connection-string"></a>Mit einer SQL-Datenbankinstanz in der Webrolle Verbindungszeichenfolge

1. Zum Erstellen einer Instanz der SQL-Datenbank in [Azure-Verwaltungsportal](http://go.microsoft.com/fwlink/?LinkID=213885)befolgen Sie die Schritte im folgenden Artikel: [Erstellen einer SQL-Datenbankserver](http://go.microsoft.com/fwlink/?LinkId=225109).

    >[AZURE.NOTE] Wenn Sie Firewall-Regeln für die Instanz der SQL-Datenbank einrichten, müssen Sie **andere Azure-Dienste auf diesem Server** das Kontrollkästchen auswählen.

1. Zum Erstellen einer Instanz der SQL-Datenbank für die Verbindungszeichenfolge verwendet, führen Sie die Schritte im nächsten Abschnitt im folgenden Artikel: [Erstellen einer SQL-Datenbank](http://go.microsoft.com/fwlink/?LinkId=225110).

1. Zum Kopieren der ADO.NET Verbindungszeichenfolge für die Verbindungszeichenfolge folgendermaßen Sie im [klassischen Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885).  

  1. Wählen Sie die **Datenbank** , und öffnen Sie den Knoten für das Abonnement, das Erstellen die Instanz der SQL-Datenbank verwendet.

  1. Wählen Sie zum Anzeigen der verfügbaren Instanzen von SQL-Datenbank **SQL** -Datenbanken.

  1. Wählen Sie zum Anzeigen der Eigenschaften für die Datenbank die Datenbank aus. **Die Ansicht** wird angezeigt.

      >[AZURE.NOTE] Wenn **die Ansicht** nicht angezeigt wird, müssen Sie es öffnen, indem Sie die Trennlinie.

  1. Um die Verbindungszeichenfolgen, wählen Sie das Auslassungszeichen (...) neben anzeigen.

    **Verbindungszeichenfolgen** -Dialogfeld wird angezeigt.

  1. Um die Verbindungszeichenfolge ADO.NET zu kopieren, markieren Sie den Text und wählen Sie die Tasten STRG + C.

  1. Um das Dialogfeld zu schließen, wählen Sie die Schaltfläche **Schließen** .

1. Ersetzen Sie die Verbindungszeichenfolge in der Datei web.config dieser Instanz der SQL-Datenbank öffnen Sie die Datei web.config, markieren Sie vorhandene Verbindungszeichenfolgeneintrag, und wählen Sie dann die Tasten STRG + V. ADO.NET Verbindungszeichenfolge für die Instanz der SQL-Datenbank ersetzt die vorhandene Verbindungszeichenfolge.

1. Sie müssen den Parameter hinzufügen `MultipleActiveResultSets=True` der Verbindungszeichenfolge. Die Verbindungszeichenfolge muss das folgende Format:

    ```
    connectionString=”Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"
    ```

1. (Optional) Eine alternative Methode zum Ändern der Verbindungszeichenfolge in der Datei web.config ist einen Abschnitt eines Transformationsdateien web.config je Build hinzugefügt, mit dem ein Servicepaket zu erstellen. Öffnen Sie die Datei "Web.Debug.config" oder die Datei Web.Release.Config. Fügen Sie den folgenden Abschnitt in diese Datei ein:

    ```
    XMLCopy<connectionStrings><addname="DefaultConnection"connectionString="Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"xdt:Transform="SetAttributes"xdt:Locator="Match(name)"/></connectionStrings>
    ```

1. Speichern Sie die Datei, die Sie geändert haben, und veröffentlichen Sie die Anwendung.

### <a name="to-use-an-instance-of-sql-database-by-using-the-azure-classic-portal"></a>Eine Instanz der SQL-Datenbank mithilfe der Azure-Verwaltungsportal

1. Wählen Sie in [Azure-Verwaltungsportal](http://go.microsoft.com/fwlink/?LinkID=213885)SQL-Datenbanken.

  - Wählen Sie öffnen, wird die Instanz der SQL-Datenbank, die Sie verwenden möchten.

  - Wenn Sie alle Instanzen erstellt haben, wählen Sie den entsprechenden Link und erstellen Sie eine Instanz.

1. Öffnen oder Erstellen einer Datenbankinstanz wählen Sie **Verbindungszeichenfolgen** Link aus

1. Wählen Sie am unteren Rand der Seite den Link Firewall-Einstellungen, und die Standardwerte übernehmen oder konfigurieren Sie die erforderlichen Werte.

1. ADO.NET Verbindungszeichenfolge kopieren und Einfügen der Datei web.config über alte Verbindungszeichenfolge für die lokale Datenbank unbedingt `MultipleActiveResultSets=True`.

## <a name="publish-a-web-application-to-azure"></a>Veröffentlichen einer Webanwendung in Azure

### <a name="to-publish-a-web-application-to-azure"></a>Eine Web-Anwendung in Azure veröffentlichen

1. Zum Testen der Anwendung im Bereich lokale Umgebung Azure compute Emulator Öffnen des Kontextmenüs für Azure-Projekt für die Web-Rolle und wählen Sie **als Startprojekt festlegen**. Wählen Sie **Debuggen**, **Debuggen starten** (Tastatur: **F5**).

    Das Dialogfenster **der Azure-Umgebung Debuggen starten** und die Anwendung im Browser gestartet wird. Spezifische Einzelheiten wie jede Art von Anwendung in Serveremulator finden Sie in der Tabelle in diesem Abschnitt.

1. Um die Dienste für Ihre Anwendung in Azure veröffentlichen einzurichten, benötigen Sie ein Microsoft-Konto und Azure-Abonnement. Gehen Sie die im folgenden Thema Ihre Dienste einrichten: [vorbereiten, zu veröffentlichen oder eine Azure-Anwendung aus Visual Studio bereitstellen](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).

1. Um Webanwendung in Azure veröffentlichen, öffnen Sie das Kontextmenü für das Webprojekt, und wählen Sie **in Azure veröffentlichen**.

    Öffnet das Dialogfeld **Azure-Anwendung veröffentlichen** und Visual Studio startet den Bereitstellungsprozess. Weitere Informationen zum Veröffentlichen der Anwendungdes finden Sie im Abschnitt veröffentlichen [Einen Cloud-Dienst mithilfe der Azure-Tools](vs-azure-tools-publishing-a-cloud-service.md) **eine Azure-Anwendung von Visual Studio veröffentlicht** .

    >[AZURE.NOTE] Sie können auch die Webanwendung aus dem Azure-Projekt veröffentlichen. Hierzu öffnen Sie das Kontextmenü für den Azure-Projekt, und wählen Sie **Veröffentlichen**.

1. Um den Fortschritt der Bereitstellung finden Sie unter **Azure Activity Log** -Fenster angezeigt. Dieses Protokoll wird automatisch beim Starten des Bereitstellungsprozesses angezeigt. Sie erweitern den Positionsartikel im Aktivitätsprotokoll, um detaillierte Informationen anzuzeigen, wie in der folgenden Abbildung dargestellt:

    ![VST_AzureActivityLog](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744149.png)

1. (Optional) Die Bereitstellung Abbrechen, öffnen Sie das Kontextmenü für den Positionsartikel im Aktivitätsprotokoll, und wählen **Abbrechen und entfernen**. Dies beendet den Bereitstellungsprozess und Deployment-Umgebung auf Azure gelöscht.

    >[AZURE.NOTE] Zum Deployment-Umgebung entfernen, nachdem sie bereitgestellt wurde, verwenden Sie das [klassische Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885).

1. (Optional) Nachdem die Rolleninstanzen gestartet haben, zeigt Visual Studio Deployment-Umgebung automatisch den **Azure Compute** -Knoten im **Cloud Explorer** oder im **Server-Explorer**. Hier können Sie den Status der einzelnen Rolleninstanzen anzeigen.

    Die folgende Abbildung zeigt die Rolleninstanzen im **Server-Explorer** , während sie weiterhin die Initialisierung sind:

    ![VST_DeployComputeNode](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744134.png)

1. Zugriff auf die Anwendung nach der Bereitstellung wählen Sie den Pfeil neben der Bereitstellung im **Azure-Aktivitätsprotokoll**erscheint Status **abgeschlossen aus** Die URL für die Webanwendung in Azure angezeigt. Siehe folgende Tabelle Informationen an einen bestimmten Typ von Anwendung von Azure.

    Die folgende Tabelle enthält Informationen zu bestimmten ASP.NET-Webanwendungen Azure oder zum Ausführen oder Debuggen einer Web-Anwendung lokal mithilfe der Azure-Serveremulator:

  	|Webanwendungstyp|Debuggen Sie lokal mit Serveremulator ausführen und|In Azure ausgeführt|
  	|---|---|---|
  	|ASP.NET Web-Anwendung|In der Menüleiste wählen Sie **Debuggen**, **Debuggen starten** (Tastatur: Wählen Sie die **F5** -Taste.).|Wählen Sie auf der Registerkarte **Bereitstellung** **Azure Aktivitätsprotokoll** laden die Startseite im Browser angezeigte URL Link.|
  	|ASP.NET MVC 2 Webanwendungsprojekt|In der Menüleiste wählen Sie **Debuggen**, **Debuggen starten** (Tastatur: Wählen Sie die **F5** -Taste.).|Wählen Sie auf der Registerkarte **Bereitstellung** **Azure Aktivitätsprotokoll** laden die Startseite im Browser angezeigte URL Link.|
  	|ASP.NET MVC 3 Web-Anwendung|In der Menüleiste wählen Sie **Debuggen**, **Debuggen starten** (Tastatur: Wählen Sie die **F5** -Taste.).|Wählen Sie auf der Registerkarte **Bereitstellung** **Azure Aktivitätsprotokoll** laden die Startseite im Browser angezeigte URL Link.|
  	|ASP.NET MVC 4 Web-Anwendung|In der Menüleiste wählen Sie **Debuggen**, **Debuggen starten** (Tastatur: Wählen Sie die **F5** -Taste.).|Wählen Sie auf der Registerkarte **Bereitstellung** **Azure Aktivitätsprotokoll** laden die Startseite im Browser angezeigte URL Link.|
  	|ASP.NET leere|Sie müssen eine ASPX-Seite in Ihrer Anwendung hinzufügen, die Sie als Startseite für das Webprojekt festlegen. In der Menüleiste wählen Sie **Debuggen**, **Debuggen starten** (Tastatur: Wählen Sie die **F5** -Taste.).|Haben Sie eine ASPX-Standardseite der Anwendung, wählen Sie in der Registerkarte **Bereitstellung** **Azure-Aktivitätsprotokoll** zu dieser Seite angezeigte URL-Link im Browser geladen wird. Haben Sie eine andere ASPX-Seite müssen Sie diese bestimmte Seite mit der URL navigieren:`<url for deployment>/<name of page>.aspx`|
  	|Silverlight-Anwendung|In der Menüleiste wählen Sie **Debuggen**, **Debuggen starten** (Tastatur: Wählen Sie die **F5** -Taste.).|Sie müssen die Seite für die Anwendung mit dem folgenden Format für den Url navigieren:`<url for deployment>/<name of page>.aspx`|
  	|Silverlight-Business-Anwendung|In der Menüleiste wählen Sie **Debuggen**, **Debuggen starten** (Tastatur: Wählen Sie die **F5** -Taste.).|Sie müssen die Seite für die Anwendung mit dem folgenden Format für den Url navigieren:`<url for deployment>/<name of page>.aspx`|
  	|Silverlight-Navigation-Anwendung|In der Menüleiste wählen Sie **Debuggen**, **Debuggen starten** (Tastatur: Wählen Sie die **F5** -Taste.).|Sie müssen die Seite für die Anwendung mit dem folgenden Format für den Url navigieren:`<url for deployment>/<name of page>.aspx`|
  	|WCF-Anwendung|Sie müssen die SVC-Datei für das WCF-Dienstprojekt als Startseite festlegen. In der Menüleiste wählen Sie **Debuggen**, **Debuggen starten** (Tastatur: Wählen Sie die **F5** -Taste.).|Sie müssen die svc-Datei für die Anwendung mit dem folgenden Format für den Url navigieren:`<url for deployment>/<name of service file>.svc`|
  	|WCF Workflow Service-Anwendung|Sie müssen die SVC-Datei für das WCF-Dienstprojekt als Startseite festlegen. In der Menüleiste wählen Sie **Debuggen**, **Debuggen starten** (Tastatur: Wählen Sie die **F5** -Taste.).|Sie müssen die svc-Datei für die Anwendung mit dem folgenden Format für den Url navigieren:`<url for deployment>/<name of service file>.svc`|
  	|ASP.NET Dynamic Entitäten|In der Menüleiste wählen Sie **Debuggen**, **Debuggen starten** (Tastatur: Wählen Sie die **F5** -Taste.).|Sie müssen die Verbindungszeichenfolge aktualisieren (siehe nächster Abschnitt). Sie müssen die Seite für die Anwendung mit dem folgenden Format für den Url navigieren:`<url for deployment>/<name of page>.aspx`|
  	|ASP.NET Dynamic Data Linq to SQL|In der Menüleiste wählen Sie **Debuggen**, **Debuggen starten** (Tastatur: Wählen Sie die **F5** -Taste.).|Sie müssen die Schritte in diesem Verfahren: eine SQL Azure-Datenbank für Ihre Anwendung verwenden (siehe Abschnitt weiter oben in diesem Thema). Sie müssen die Seite für die Anwendung mit dem folgenden Format für den Url navigieren:`<url for deployment>/<name of page>.aspx`|

## <a name="update-a-connection-string-for-aspnet-dynamic-entities"></a>Eine Verbindungszeichenfolge für ASP.NET Dynamic Entitäten aktualisieren

### <a name="to-update-a-connection-string-for-aspnet-dynamic-entities"></a>Eine Verbindungszeichenfolge für ASP.NET Dynamic Entitäten aktualisieren

1. Um eine SQL Azure-Datenbank erstellen, die für eine dynamische Entitäten ASP.NET Web-Anwendung verwendet werden kann, führen Sie die Schritte oben in diesem Thema das Verfahren **einer SQL Azure-Datenbank für die Anwendung** .

1. Fügen Sie die Tabellen und Felder, die Sie für diese Datenbank aus dem [Azure-Verwaltungsportal](http://go.microsoft.com/fwlink/?LinkID=213885).

1. Die Verbindungszeichenfolge für diese Art von Anwendung hat das folgende Format in der Datei web.config:  

    ```
    <addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;data source=<server name>\SQLEXPRESS;initial catalog=<database name>;integrated security=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

    Aktualisieren Sie den *ConnectionString* -Wert mit ADO.NET Verbindungszeichenfolge für SQL Azure-Datenbank wie folgt:

    ```
    XMLCopy<addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;Server=tcp:<SQL Azure server name>.database.windows.net,1433;Database=<database name>;User ID=<user name>;Password=<password>;Trusted_Connection=False;Encrypt=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

1. Zum Speichern die Datei web.config mit der Verbindungszeichenfolge in der Menüleiste vorgenommenen Änderungen **Datei** **web.config speichern**auswählen.

## <a name="supported-project-templates"></a>Unterstützte Vorlagen

Um eine Web-Anwendung in Azure veröffentlichen, muss die Anwendung mithilfe einer Projektvorlage für C# oder Visual Basic, die in der folgenden Tabelle aufgeführt.

|Projektgruppe Vorlage|Projektvorlage|
|---|---|
|Web|ASP.NET Web-Anwendung|
|Web|ASP.NET MVC 2 Webanwendungsprojekt|
|Web|ASP.NET MVC 3 Web-Anwendung|
|Web|MVC4 ASP.NET Web-Anwendung|
|Web|ASP.NET leere|
|Web|ASP.NET MVC 2 leere Webanwendungsprojekt|
|Web|ASP.NET Dynamic Data Entities Web-Anwendung|
|Web|ASP.NET Dynamic Data Linq to SQL Web-Anwendung|
|Silverlight|Silverlight-Anwendung|
|Silverlight|Silverlight-Business-Anwendung|
|Silverlight|Silverlight-Navigation-Anwendung|
|WCF|WCF-Anwendung|
|WCF|WCF Workflow Service-Anwendung|
|Workflow|WCF Workflow Service-Anwendung|

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Veröffentlichung finden Sie unter [Vorbereitung veröffentlichen oder Bereitstellen einer Azure-Anwendung von Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md). Überprüfen Sie auch [Festlegen, mit dem Namen Authentifizierungsinformationen](vs-azure-tools-setting-up-named-authentication-credentials.md).
