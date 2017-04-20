<properties
    pageTitle="Verbindung mit lokalen SQL Server von Web-app in Azure App Service Hybrid Verbindung"
    description="Microsoft Azure erstellen Sie eine Webanwendung und mit einer lokalen SQL Server-Datenbank"
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"
    ms.author="cephalin"/>

# <a name="connect-to-on-premises-sql-server-from-a-web-app-in-azure-app-service-using-hybrid-connections"></a>Verbindung mit lokalen SQL Server von Web-app in Azure App Service Hybrid Verbindung

Hybridverbindungen können [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps auf lokale Ressourcen, die einen statischen TCP-Port zu verwenden. Unterstützte Ressourcen gehören Microsoft SQL Server, MySQL, HTTP-Web-APIs, App Service und die meisten benutzerdefinierte Webdienste.

In diesem Lernprogramm erfahren Sie, wie App Service Web app im [Azure-Portal](http://go.microsoft.com/fwlink/?LinkId=529715)erstellen, Web app zur lokal lokale SQL Server-Datenbank mithilfe der neuen Funktion Hybrid-Verbindung herstellen, erstellen Sie eine einfache ASP.NET Anwendung, hybridverbindung verwenden, und in der App Service Web app bereitzustellen. Fertig gestellte Web app auf Azure werden Benutzeranmeldeinformationen in einer Gruppenmitgliedschaft lokal gespeichert. Keine Erfahrung mit Azure oder ASP.NET vorausgesetzt.

>[AZURE.NOTE] Wenn Sie mit Azure App Service beginnen, bevor Sie sich für ein Azure-Konto, gehen Sie [Versuchen App Service](http://go.microsoft.com/fwlink/?LinkId=523751)sofort eine kurzlebige Starter Web app in App Service können Sie erstellen. Keine Kreditkarten erforderlich; keine Zusagen.
>
>Web Apps Teil Hybridverbindungen Feature steht nur in der [Azure-Portal](https://portal.azure.com). Zum Erstellen einer Verbindung im BizTalk-Dienste finden Sie unter [Hybridverbindungen](http://go.microsoft.com/fwlink/p/?LinkID=397274).  

## <a name="prerequisites"></a>Erforderliche Komponenten ##

Um dieses Lernprogramm benötigen Sie die folgenden Produkte. Alle stehen kostenlose Versionen damit entwickeln für Azure für freien gestartet werden kann.

- **Azure-Abonnement** - ein kostenloses Abonnement finden Sie unter [Azure-Testversion](/pricing/free-trial/).

- **Visual Studio 2013** - Testversion von Visual Studio 2013 herunterladen, finden Sie unter [Visual Studio-Downloads](http://www.visualstudio.com/downloads/download-visual-studio-vs). Installieren Sie diese, bevor Sie fortfahren.

- **Microsoft.NET Framework 3.5 Servicepack 1** - Betriebssystem ist Windows 8.1, Windows Server 2012 R2, Windows 8, Windows Server 2012, Windows 7 oder Windows Server 2008 R2 können Sie dies im Bedienfeld > Programme und Funktionen > Windows-Funktionen ein- oder ausschalten. Andernfalls können Sie es aus dem [Microsoft Download Center](http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=22)herunterladen.

- **SQL Server 2014 Express mit Tools** - Microsoft SQL Server Express kostenlos auf der [Seite Microsoft-Webplattform Datenbank](http://www.microsoft.com/web/platform/database.aspx)herunterladen. Wählen Sie die **Express** (nicht LocalDB). **Express mit Tools** Version enthält SQL Server Management Studio, das in diesem Lernprogramm verwenden.

- **SQL Server Management Studio Express** – Dies ist mit SQL Server 2014 Express mit oben erwähnten Tools herunterladen, jedoch benötigen Sie separat installieren, können Sie herunterladen und Installieren von [SQL Server Express-Downloadseite](http://www.microsoft.com/web/platform/database.aspx).

Das Lernprogramm wird vorausgesetzt, dass Sie ein Azure-Abonnement, dass Visual Studio 2013 installiert und installiert bzw. aktiviert.NET Framework 3.5. Das Lernprogramm zeigt, wie SQL Server Express 2014 in einer Konfiguration installieren, die mit Azure Hybrid Connections-Funktion (eine Standardinstanz mit einem statischen TCP-Port). Herunterladen Sie bevor Sie das Lernprogramm starten SQL Server 2014 Express mit Tools aus genannten, wenn Sie nicht SQL Server installiert haben.

### <a name="notes"></a>Notizen ###
Um eine lokale SQL Server oder SQL Server Express-Datenbank mit einer hybridverbindung verwenden, muss TCP/IP auf einen statischen Port aktiviert werden. Standardinstanzen von SQL Server verwenden statischen Port 1433 benannte Instanzen nicht.

Der Computer, auf dem lokalen Hybrid Connection Manager Agent installiert:

- Ausgehende Verbindungen in Azure müssen über

Anschluss|Warum
---|---
80|Für HTTP-Port für die Überprüfung des Zertifikats und optional für Datenkonnektivität **erforderlich** .
443|**Optional** für Datenkonnektivität. Ausgehende Verbindungen auf 443 nicht verfügbar ist, wird TCP-Port 80 verwendet.
5671 und 9352|Aber Optional für Datenkonnektivität **empfohlen** . Hinweis: Dieser Modus normalerweise höheren Durchsatz führt. Ausgehende Verbindungen auf diese Ports nicht verfügbar ist, wird TCP-Port 443 verwendet.
- Muss zu der *Hostname*:*Portnummer* der lokalen Ressource.

Die Schritte in diesem Artikel wird davon ausgegangen, dass Sie den Browser auf dem Computer verwenden, die lokalen Hybrid Verbindungsagent hosten.

Haben Sie bereits SQL Server in eine Konfiguration einer Umgebung, die die oben beschriebenen erfüllt installiert, können überspringen und mit [lokalen SQL Server-Datenbank](#CreateSQLDB)zu starten.

<a name="InstallSQL"></a>
## <a name="a-install-sql-server-express-enable-tcpip-and-create-a-sql-server-database-on-premises"></a>A. Installieren Sie SQL Server Express, TCP/IP aktivieren und Erstellen einer SQL Server-Datenbank für lokale ##

In diesem Abschnitt wird das Installieren von SQL Server Express, TCP/IP aktivieren und Erstellen einer Web-Anwendung mit dem Azure-Portal funktioniert veranschaulicht.

### <a name="install-sql-server-express"></a>Installieren Sie SQL Server Express ###

1. Führen Sie zum Installieren von SQL Server Express heruntergeladene Datei **SQLEXPRWT_x64_ENU.exe** oder **SQLEXPR_x86_ENU.exe** . Der SQL Server-Installationscenter-Assistent wird angezeigt.

    ![SQL Server-Installation][SQLServerInstall]

2. Wählen Sie **eigenständige neue SQL Server-Installation oder Hinzufügen von Features zu einer vorhandenen Installation**. Anweisungen Sie, Standardeinstellungen und Einstellungen akzeptieren, bis Sie zur Seite **Konfiguration** erhalten.

3. Wählen Sie auf der Seite **Konfiguration** **Standardinstanz**.

    ![Wählen Sie Standardinstanz][ChooseDefaultInstance]

    Standardmäßig wartet die Standardinstanz von SQL Server Anfragen von Clients statische Port 1433, SQL Server ist wie die Funktion Hybrid-Verbindung erfordert. Benannte Instanzen verwenden dynamische Ports und UDP Hybrid-Verbindungen nicht unterstützt werden.

4. Übernehmen Sie die Standardeinstellungen auf der Seite **Server-Konfiguration** .

5. Auf der Seite **Datenbankmodulkonfiguration** **Authentifizierung** **Im gemischten Modus (SQL Server und Windows-Authentifizierung)**wählen und ein Kennwort.

    ![Wählen Sie im gemischten Modus][ChooseMixedMode]

    In diesem Lernprogramm verwenden Sie SQL Server-Authentifizierung. Achten Sie darauf, dass Sie leicht zu merken, die Sie bereitstellen, da Sie sie später benötigen.

6. Durchlaufen Sie den Assistenten für die Installation.

### <a name="enable-tcpip"></a>TCP/IP aktivieren ###
Um TCP/IP zu aktivieren, verwenden Sie SQL Server Configuration Manager bei der Installation von SQL Server Express installiert wurde. Schritte in [TCP/IP-Netzwerkprotokoll für SQL Server aktivieren](http://technet.microsoft.com/library/hh231672%28v=sql.110%29.aspx) bevor Sie fortfahren.

<a name="CreateSQLDB"></a>
### <a name="create-a-sql-server-database-on-premises"></a>Erstellen einer SQL Server-Datenbank für lokale ###

Die Visual Studio Web-Anwendung erfordert eine Mitgliedschaftsdatenbank Azure zugreifen kann. Dies erfordert eine SQL Server oder SQL Server Express Datenbank nicht die LocalDB, die MVC-Vorlage verwendet, damit Sie als Nächstes die Mitgliedschaftsdatenbank erstellen.

1. In SQL Server Management Studio eine Verbindung mit SQL Server gerade installiert. (Wenn **mit Server verbinden** Dialogfeld erscheint nicht automatisch im linken Fensterbereich zum **Objekt-Explorer** navigieren, klicken Sie auf **Verbinden**, und klicken Sie dann auf **Datenbank-Engine**.)  ![Mit Server verbinden][SSMSConnectToServer]

    **Typ**wählen Sie **Datenbank-Engine**. **Servernamen**können Sie **Localhost** oder den Namen des Computers. Wählen Sie **SQL Server-Authentifizierung**, und melden Sie sich mit den sa-Benutzernamen und das Kennwort, das Sie zuvor erstellt haben.

2. Erstellen eine neue Datenbank mit SQL Server Management Studio mit der rechten Maustaste im Objekt-Explorer **Datenbanken** und klicken Sie dann auf **Neue Datenbank**.

    ![Neue Datenbank erstellen][SSMScreateNewDB]

3. Geben Sie im Dialogfeld **Neue Datenbank** MembershipDB für den Datenbanknamen, und klicken Sie auf **OK**.

    ![Datenbanknamen bereitstellen][SSMSprovideDBname]

    Beachten Sie, dass Sie keine Änderungen auf die Datenbank zu diesem Zeitpunkt treffen. Die Informationen zur Gruppenmitgliedschaft wird von der Webanwendung automatisch später hinzugefügt werden, bei der Ausführung.

4. Im Objekt-Explorer Wenn Sie **Datenbanken**, erweitern sehen Sie die Mitgliedschaftsdatenbank erstellt wurde.

    ![MembershipDB erstellt][SSMSMembershipDBCreated]

<a name="CreateSite"></a>
## <a name="b-create-a-web-app-in-the-azure-portal"></a>B. Erstellen einer Webanwendung in Azure-Portal ##

> [AZURE.NOTE] Wenn Sie bereits eine Webanwendung in Azure-Portal, die Sie für dieses Lernprogramm verwenden möchten erstellt haben, können überspringen [einer Hybrid-Verbindung](#CreateHC) und eines BizTalk-Dienstes und von dort aus.

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) **neu** > **Web + Mobile** > **WebApp**.

    ![Neue Schaltfläche][New]

2. Konfigurieren Sie Ihrer Anwendung, und klicken Sie dann auf **Erstellen**.

    ![Name der Website][WebsiteCreationBlade]

3. Nach einigen Augenblicken Web app erstellt und seine Web app Blade angezeigt. Das Blade ist ein vertikal bildlauffähigen Dashboard, die Sie Ihrer Anwendung verwalten kann.

    ![Website unter][WebSiteRunningBlade]

    Um sicherzustellen, dass die Webanwendung ist, können Sie das Symbol **Durchsuchen** , um die Startseite anzuzeigen klicken.

Anschließend erstellen Sie eine hybridverbindung und eines BizTalk-Dienstes für Web app.

<a name="CreateHC"></a>
## <a name="c-create-a-hybrid-connection-and-a-biztalk-service"></a>C. Erstellen einer Hybridverbindung und eines BizTalk-Dienstes ##

1. Zurück im Portal Settings und klicken Sie auf **Netzwerk** > **hybride Verbindungsendpunkte konfigurieren**.

    ![Hybridverbindungen][CreateHCHCIcon]

2. -Blade Hybrid-Verbindungen klicken Sie auf **Hinzufügen** > **Hybrid neu**.

3. Auf der **Hybrid-Verbindung erstellen** :
    - **Name**Geben Sie einen Namen für die Verbindung.
    - **Hostname**Geben Sie den Computernamen des Computers Host SQL Server.
    - Geben Sie für **Port**1433 (der Standardport für SQL Server).
    - Klicken Sie auf **BizTalk** > **Neue BizTalk-Dienst** , und geben Sie einen Namen für den BizTalk-Dienst.

    ![Erstellen einer hybridverbindung][TwinCreateHCBlades]

5. Klicken Sie zweimal auf **OK** .

    Wenn abgeschlossen, **Benachrichtigungen** Bereich wird eine grüne **Erfolg** flash und der **Hybrid-Verbindung** Blade neue hybridverbindung mit dem Status als **nicht verbunden**angezeigt.

    ![Erstellt ein Hybrid-Verbindung][CreateHCOneConnectionCreated]

An diesem Punkt haben Sie Bestandteil der hybridverbindungsinfrastruktur der Cloud. Anschließend erstellen Sie einen entsprechenden lokalen Stück.

<a name="InstallHCM"></a>
## <a name="d-install-the-on-premises-hybrid-connection-manager-to-complete-the-connection"></a>D. Installieren Sie lokale Hybrid Verbindungs-Manager die Verbindung ##

[AZURE.INCLUDE [app-service-hybrid-connections-manager-install](../../includes/app-service-hybrid-connections-manager-install.md)]

Abschluss die hybridverbindungsinfrastruktur erstellen Sie eine Web-Anwendung, die verwendet wird.

<a name="CreateASPNET"></a>
## <a name="e-create-a-basic-aspnet-web-project-edit-the-database-connection-string-and-run-the-project-locally"></a>E. Erstellen Sie ein grundlegende ASP.NET Web-Projekt, bearbeiten Sie die Verbindungszeichenfolge und lokal führen Sie des Projekts aus ##

### <a name="create-a-basic-aspnet-project"></a>Erstellen Sie ein einfaches Projekt für ASP.NET ###
1. Erstellen Sie in Visual Studio im Menü **Datei** ein neues Projekt ein:

    ![Neue Visual Studio-Projekt][HCVSNewProject]

2. Wählen Sie im Abschnitt **Vorlagen** des Dialogfelds **Neues Projekt** **Web** wählen Sie **ASP.NET Web-Anwendung aus**und klicken Sie dann auf **OK**.

    ![Wählen Sie ASP.NET Web-Anwendung][HCVSChooseASPNET]

3. Klicken Sie im Dialogfeld **Neues Projekt von ASP.NET** wählen Sie **MVC aus**und dann auf **OK**.

    ![Wählen Sie MVC][HCVSChooseMVC]

4. Beim Erstellen des Projekts wird die Anwendung Readme angezeigt. Führen Sie das Webprojekt noch.

    ![Readme-Seite][HCVSReadmePage]

### <a name="edit-the-database-connection-string-for-the-application"></a>Bearbeiten Sie die Verbindungszeichenfolge der Datenbank für die Anwendung ###

In diesem Schritt bearbeiten Sie die Verbindungszeichenfolge, die der Anwendung wo zu Ihrer lokalen SQL Server Express-Datenbank. Die Verbindungszeichenfolge ist in der Anwendung Datei Web.config enthält Konfigurationsinformationen für die Anwendung.

> [AZURE.NOTE] Um sicherzustellen, dass die Anwendung die Datenbank verwendet, die in SQL Server Express und nicht den in Visual Studio standardmäßig LocalDB erstellt, ist es wichtig, dass Sie diesen Schritt abschließen, bevor Sie das Projekt ausführen.

1. Doppelklicken Sie im Projektmappen-Explorer auf die Datei Web.config.

    ![Web.config][HCVSChooseWebConfig]

2. Bearbeiten Sie den **ConnectionStrings** -Abschnitt der SQL Server-Datenbank auf dem lokalen Computer folgt der Syntax im folgenden Beispiel:

    ![Verbindungszeichenfolge][HCVSConnectionString]

    Wenn die Verbindungszeichenfolge Beachten Sie Folgendes:

    - Wenn zu einer benannten Instanz anstelle einer Standardinstanz (z. B. YourServer\SQLEXPRESS verbinden) müssen Sie die SQL Server-statischen Anschlüsse konfigurieren. Informationen zum Konfigurieren statischer Ports finden Sie unter [SQL Server zum Abhören eines bestimmten Ports konfigurieren](http://support.microsoft.com/kb/823938). Benannte Instanzen verwenden standardmäßig UDP und dynamische Ports Hybrid-Verbindungen nicht unterstützt.

    - Es wird empfohlen, dass Sie den Port angeben (wie im Beispiel gezeigt standardmäßig 1433) der Verbindungszeichenfolge, damit Sie sicher sein können Ihre lokalen SQL Server TCP aktiviert ist und den richtigen Anschluss.

    - Denken Sie daran, mit SQL Server-Authentifizierung eine Verbindung herstellen, den Benutzernamen und das Kennwort in der Verbindungszeichenfolge angeben.

3. Klicken Sie auf **Speichern** in Visual Studio die Web.config-Datei gespeichert.

### <a name="run-the-project-locally-and-register-a-new-user"></a>Führen Sie das Projekt lokal und registrieren einen neuen Benutzer ###

1. Führen Sie nun das neue Webprojekt lokal durch Klicken auf die Schaltfläche Durchsuchen unter Debuggen. In diesem Beispiel wird Internet Explorer verwendet.

    ![Projekt ausführen][HCVSRunProject]

2. Klicken Sie rechts oben die Standardwebseite wählen Sie **Registrieren** , um ein neues Konto registrieren:

    ![Ein neues Konto registrieren][HCVSRegisterLocally]

3. Geben Sie einen Benutzernamen und ein Kennwort:

    ![Geben Sie Benutzernamen und Kennwort][HCVSCreateNewAccount]

    Dies erstellt eine Datenbank automatisch auf die lokale SQL Server, der die Mitgliedschaft für die Anwendung enthält. Eine der Tabellen (**Dbo. AspNetUsers**) enthält web app Benutzeranmeldeinformationen wie diejenigen, die Sie gerade eingegeben haben. Diese Tabelle wird später im Lernprogramm angezeigt werden.

4. Schließen Sie das Browserfenster die Standardwebseite. Dies beendet die Anwendung in Visual Studio.

Sie sind nun bereit für den nächsten Schritt die Anwendung in Azure veröffentlichen und testen.

<a name="PubNTest"></a>
## <a name="f-publish-the-web-application-to-azure-and-test-it"></a>F. Die Webanwendung in Azure veröffentlichen und testen ##

Nun, Sie veröffentlichen Sie die Anwendung auf Ihre App Service Web app und testen, um festzustellen, wie zuvor konfigurierten Hybrid-Verbindung zum Verbinden Ihrer Anwendung mit der Datenbank auf dem lokalen Computer verwendet wird.

### <a name="publish-the-web-application"></a>Veröffentlichen der Anwendung ###

1. Sie können Ihre Präsentationsprofil für App Service Web app im Azure-Portal herunterladen. Blade für Ihr Web app auf **Get Profil veröffentlichen**und speichern Sie die Datei auf Ihren Computer.

    ![Download Profil veröffentlichen][PortalDownloadPublishProfile]

    Anschließend importieren Sie diese Datei in der Visual Studio-Web-Anwendung.

2. In Visual Studio mit der rechten Maustaste im Projektmappen-Explorer des Projektnamen, und wählen Sie **Veröffentlichen**.

    ![Wählen Sie veröffentlichen][HCVSRightClickProjectSelectPublish]

3. Wählen Sie im Dialogfeld **Web veröffentlichen** auf der Registerkarte **Profil** **Importieren**.

    ![Importieren][HCVSPublishWebDialogImport]

4. **Heruntergeladene Präsentationsprofil suchen und auswählen.**

    ![Profil suchen][HCVSBrowseToImportPubProfile]

5. Die Veröffentlichung von Informationen importiert und auf der Registerkarte **Verbindung** des Dialogfelds angezeigt.

    ![Klicken Sie auf Veröffentlichen][HCVSClickPublish]

    Klicken Sie auf **Veröffentlichen**.

    Abschluss veröffentlichen Ihre Browser starten und anzeigen die bekannte Anwendung ASP.NET jetzt live in Azure Cloud ist!

Anschließend verwenden Sie die Web-Anwendung die Hybrid-Verbindung in Aktion sehen.

### <a name="test-the-completed-web-application-on-azure"></a>Testen Sie die Anwendung fertig gestellte Web Azure ###

1. Oben rechts von der Webseite auf Azure wählen **Anmelden**.

    ![Test anmelden][HCTestLogIn]

2. Ihrer Anwendung App Service ist jetzt Mitgliedschaftsdatenbank Ihre Web-Anwendung auf dem lokalen Computer verbunden. Hierzu melden Sie sich mit denselben Anmeldeinformationen, die Sie zuvor in der lokalen Datenbank eingegeben.

    ![Hallo Begrüßung][HCTestHelloContoso]

3. Die neue hybridverbindung weiter zu testen, melden Sie Azure Webtests und als anderer Benutzer anmelden. Einen neuen Benutzernamen und Kennwort, und klicken Sie dann auf **Registrieren**.

    ![Testen einer anderen Benutzer registrieren][HCTestRegisterRelecloud]

4. Um sicherzustellen, dass die neuen Anmeldeinformationen in der lokalen Datenbank über hybridverbindung gespeichert haben, öffnen Sie SQL Management Studio auf dem lokalen Computer. Im Objekt-Explorer erweitern Sie die Datenbank **MembershipDB** und dann **Tabellen**. Klicken Sie auf die **Dbo. AspNetUsers** Mitgliedschaft Tabelle und **Wählen Sie Top 1000 Zeilen** um die Ergebnisse anzuzeigen.

    ![Anzeigen der Ergebnisse][HCTestSSMSTree]

5. Die Mitgliedschaft in lokalen Tabelle jetzt beide Konten - lokal erstellt und in der Azure-Cloud erstellt. In der Cloud erstellt wurde zur lokalen Datenbank über Azure Hybrid-Verbindung Funktion gespeichert.

    ![Registrierte Benutzer in der lokalen Datenbank][HCTestShowMemberDb]

Sie haben nun erstellt und Bereitstellung eine ASP.NET Web-Anwendung, die eine hybridverbindung zwischen Web-app in Azure Cloud und einer lokalen SQL Server-Datenbank verwendet. Herzlichen Glückwunsch!

## <a name="see-also"></a>Siehe auch ##
[Hybrid-Verbindungen (Übersicht)](http://go.microsoft.com/fwlink/p/?LinkID=397274)

[Josh Twist führt hybridverbindungen (Channel 9 Video)](http://channel9.msdn.com/Shows/Azure-Friday/Josh-Twist-introduces-hybrid-connections)

[Hybrid-Verbindungen (Übersicht)](/services/biztalk-services/)

[BizTalk-Dienste: Dashboard, Monitor, Skalierung, konfigurieren und Hybrid-Verbindung Registerkarten](../biztalk-services/biztalk-dashboard-monitor-scale-tabs.md)

[Erstellen einer realen Hybridcloud mit nahtlosen Anwendungsportabilität (Channel 9 video)](http://channel9.msdn.com/events/TechEd/NorthAmerica/2014/DCIM-B323#fbid=)

[Zugriff auf lokale Ressourcen mit hybridverbindungen in Azure App Service](../app-service-web/web-sites-hybrid-connection-get-started.md)

[Übersicht über ASP.NET Identität](http://www.asp.net/identity)

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]


<!-- IMAGES -->
[SQLServerInstall]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A01SQLServerInstall.png
[ChooseDefaultInstance]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A02ChooseDefaultInstance.png
[ChooseMixedMode]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A03ChooseMixedMode.png
[SSMSConnectToServer]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A04SSMSConnectToServer.png
[SSMScreateNewDB]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A05SSMScreateNewDBlh.png
[SSMSprovideDBname]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A06SSMSprovideDBname.png
[SSMSMembershipDBCreated]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A07SSMSMembershipDBCreated.png
[New]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B01New.png
[NewWebsite]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B02NewWebsite.png
[WebsiteCreationBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B03WebsiteCreationBlade.png
[WebSiteRunningBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B04WebSiteRunningBlade.png
[Browse]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B05Browse.png
[DefaultWebSitePage]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B06DefaultWebSitePage.png
[CreateHCHCIcon]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C01CreateHCHCIcon.png
[CreateHCAddHC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C02CreateHCAddHC.png
[TwinCreateHCBlades]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C03TwinCreateHCBlades.png
[CreateHCCreateBTS]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C04CreateHCCreateBTS.png
[CreateBTScomplete]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C05CreateBTScomplete.png
[CreateHCSuccessNotification]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C06CreateHCSuccessNotification.png
[CreateHCOneConnectionCreated]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C07CreateHCOneConnectionCreated.png
[HCIcon]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D01HCIcon.png
[NotConnected]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D02NotConnected.png
[NotConnectedBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D03NotConnectedBlade.png
[ClickListenerSetup]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D04ClickListenerSetup.png
[ClickToInstallHCM]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D05ClickToInstallHCM.png
[ApplicationRunWarning]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D06ApplicationRunWarning.png
[UAC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D07UAC.png
[HCMInstalling]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D08HCMInstalling.png
[HCMInstallComplete]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D09HCMInstallComplete.png
[HCStatusConnected]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D10HCStatusConnected.png
[HCVSNewProject]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E01HCVSNewProject.png
[HCVSChooseASPNET]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E02HCVSChooseASPNET.png
[HCVSChooseMVC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E03HCVSChooseMVC.png
[HCVSReadmePage]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E04HCVSReadmePage.png
[HCVSChooseWebConfig]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E05HCVSChooseWebConfig.png
[HCVSConnectionString]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E06HCVSConnectionString.png
[HCVSRunProject]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E06HCVSRunProject.png
[HCVSRegisterLocally]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E07HCVSRegisterLocally.png
[HCVSCreateNewAccount]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E08HCVSCreateNewAccount.png
[PortalDownloadPublishProfile]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F01PortalDownloadPublishProfile.png
[HCVSPublishProfileInDownloadsFolder]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F02HCVSPublishProfileInDownloadsFolder.png
[HCVSRightClickProjectSelectPublish]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F03HCVSRightClickProjectSelectPublish.png
[HCVSPublishWebDialogImport]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F04HCVSPublishWebDialogImport.png
[HCVSBrowseToImportPubProfile]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F05HCVSBrowseToImportPubProfile.png
[HCVSClickPublish]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F06HCVSClickPublish.png
[HCTestLogIn]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F07HCTestLogIn.png
[HCTestHelloContoso]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F08HCTestHelloContoso.png
[HCTestRegisterRelecloud]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F09HCTestRegisterRelecloud.png
[HCTestSSMSTree]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F10HCTestSSMSTree.png
[HCTestShowMemberDb]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F11HCTestShowMemberDb.png
