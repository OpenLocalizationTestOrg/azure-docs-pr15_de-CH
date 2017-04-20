<properties 
    pageTitle="Mehrere Standorte in Azure App Service WordPress konvertieren" 
    description="Informationen zu einer vorhandenen WordPress Web app durch die Galerie in Azure erstellt und WordPress Multisite konvertieren" 
    services="app-service\web" 
    documentationCenter="php" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>



# <a name="convert-wordpress-to-multisite-in-azure-app-service"></a>Mehrere Standorte in Azure App Service WordPress konvertieren

## <a name="overview"></a>Übersicht

*Von [Ben Lobaugh][ben-lobaugh], [Microsoft Open Technologies Inc.][ms-open-tech]*

In diesem Lernprogramm erfahren Sie, wie eine vorhandene WordPress Web app durch die Galerie in Azure erstellt eine WordPress Multisite-Installation umwandeln. Darüber hinaus erfahren Sie wie alle Unterwebsites der Installation eine benutzerdefinierte Domäne zugewiesen.

Eine vorhandene Installation von WordPress vorausgesetzt. Wenn nicht, führen Sie Anleitung in [WordPress Website erstellen aus der Galerie in Azure][website-from-gallery].

Konvertieren einer vorhandenen WordPress Standort installieren mehrerer Standorte ist im Allgemeinen recht einfach und die ersten Schritte kommen direkt über [Ein Netzwerk erstellen] [ wordpress-codex-create-a-network] Seite [WordPress Codex](http://codex.wordpress.org).

Fangen wir an.

## <a name="allow-multisite"></a>Mehrere Standorte ermöglichen

Zunächst mehrere Standorte durch Aktivieren der `wp-config.php` Datei mit den **WP\_zulassen\_mehrere Standorte** konstant. Es gibt zwei Web app Dateien: ist FTP und das zweite durch Git. Wenn Sie mit einer dieser Methoden einrichten können, finden Sie die folgenden Lernprogramme:

* [PHP-Website mit MySQL und FTP][website-w-mysql-and-ftp-ftp-setup]

* [PHP-Website mit MySQL und Git][website-w-mysql-and-git-git-setup]

Öffnen der `wp-config.php` mit dem Editor Ihrer Wahl und fügen folgende oben die `/* That's all, stop editing! Happy blogging. */` Linie.

    /* Multisite */

    define( 'WP_ALLOW_MULTISITE', true );

Achten Sie darauf, und speichern Sie die Datei wieder auf den Server hochladen!

## <a name="network-setup"></a>Netzwerk-Setup

Melden Sie sich in *wp-Admin* -Bereich Ihres Web app und Sie sollten sehen ein neues Element im Menü **Tools** aufgerufen **Netzwerkinstallation**. Klicken Sie auf **Netzwerk-Setup** und füllen Sie die Details Ihres Netzwerks.

![Netzwerk-Setup-Bildschirm][wordpress-network-setup]

Dieses Lernprogramms *Unterverzeichnisse* Websiteschema sollte immer funktionieren und wir Einrichten von benutzerdefinierten Domänen für jede Unterwebsite später im Lernprogramm. Es sollte jedoch möglich, Setup eine Subdomäne installieren, wenn eine Domäne über [Azure-Portal](https://portal.azure.com) und Setup Wildcard-DNS ordnungsgemäß zugeordnet.

Weitere Unterdomäne Vs Unterverzeichnis Installationen finden Sie unter [Typen von Netzwerkarchitektur] [ wordpress-codex-types-of-networks] Artikel WordPress Codex.

## <a name="enable-the-network"></a>Netzwerk aktivieren

In der Datenbank ist jetzt im Netzwerk konfiguriert, aber verbleibende Schritt ermöglichen die Netzwerkfunktionalität. Abschließen der `wp-config.php` Settings und `web.config` jedem Standort ordnungsgemäß weiterleitet.


Nach der Schaltfläche **Installieren** auf der Seite *Netzwerk Setup* versucht WordPress aktualisieren die `wp-config.php` und `web.config` Dateien. Allerdings sollten Sie immer überprüfen die Dateien, um sicherzustellen, dass die Updates erfolgreich waren. Wenn dies nicht der Fall, hier stellen Ihnen die erforderlichen Updates. Bearbeiten und Speichern von Dateien.


Nach Ausführung zurück in wp-Admin-Dashboard diese Updates müssen Sie melden Sie sich ab und melden Sie sich.

Auf Admin bar gekennzeichneten **Meine Websites**sollte jetzt ein zusätzliches Menü sein. In diesem Menü können Sie neue Netzwerk durch den **Netzwerkadministrator** Dashboard steuern.

## <a name="adding-custom-domains"></a>Hinzufügen von benutzerdefinierten Domänen

[WordPress MU Domäne zuordnen] [ wordpress-plugin-wordpress-mu-domain-mapping] Plugin kann benutzerdefinierte Domänen zu einer Site in Ihrem Netzwerk hinzufügen. Reihenfolge für das Plugin ordnungsgemäß müssen Sie einige zusätzliche Einrichtungsschritte im Portal und auf Ihre Domainregistrierungsstelle.

## <a name="enable-domain-mapping-to-the-web-app"></a>Domänen-Mapping Web App aktivieren

**Freie** [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) planmodus unterstützt keine Web Apps benutzerdefinierte Domänen hinzufügen. Sie müssen **Shared** oder **Standard** -Modus wechseln. Dazu:

* Azure-Portal melden Sie an und suchen Sie Ihrer Anwendung. 
* Klicken Sie auf der Registerkarte **Skalierung** **Einstellungen**.
* Wählen Sie unter **Allgemein** *FREIGEGEBENE* oder *STANDARD*
* Klicken Sie auf **Speichern**

Möglicherweise aufgefordert zu überprüfen die Änderung bestätigen Ihrer Anwendung jetzt Kosten anfallen kann je nach Nutzung und der Konfiguration, die Sie festlegen.

Einige Sekunden zu neuen Einstellungen jetzt ist ein guter Zeitpunkt für Ihre Domäne beginnen.

## <a name="verify-your-domain"></a>Überprüfen Sie Ihre Domäne

Bevor Azure Web Apps Sie den Standort eine Domäne zuordnen können, müssen Sie zuerst überprüfen, ob die Berechtigung Domäne zuordnen. Hierzu müssen Sie den DNS-Eintrag einen CNAME-Eintrag hinzufügen.

* Melden Sie sich bei Ihrer Domäne DNS-manager
* Erstellen Sie einen neuen CNAME- *awverify*
* Zeigen Sie *Awverify* *Awverify. YOUR_DOMAIN.azurewebsites.NET*

Einige Zeit dauern DNS-Änderungen voll wirksam, wenn die folgenden Schritte nicht sofort funktionieren also eine Tasse Kaffee zurück und versuchen Sie es erneut.

## <a name="add-the-domain-to-the-web-app"></a>Die Domäne der Webanwendung hinzufügen

Zurück zu Ihrer Anwendung über das Azure-Portal **Klicken**und klicken Sie dann auf **benutzerdefinierte Domains und SSL**.

*SSL-Einstellungen* angezeigt werden, sehen Sie die Felder, in dem Sie alle Domänen Eingabe Ihrer Anwendung zuweisen möchten. Wenn eine Domäne nicht hier aufgeführt, wird es nicht für die Zuordnung in WordPress, unabhängig davon, wie die DNS-Domäne einrichten.

![Dialogfeld benutzerdefinierte Domänen verwalten][wordpress-manage-domains]

Danach Ihre Domäne in das Textfeld überprüft Azure CNAME-Eintrag, den Sie zuvor erstellt haben. Wenn DNS nicht vollständig weitergegeben wurde, zeigt ein roter Indikator. Wenn sie erfolgreich war, wird ein grünes Häkchen angezeigt. 

Notieren Sie die IP-Adresse unten im Dialogfeld aufgeführt. Sie benötigen diese setup-A-Eintrag für Ihre Domäne.

## <a name="setup-the-domain-a-record"></a>Einrichten der Domäne eines Datensatzes

Wenn die anderen Schritte erfolgreich waren, können Sie jetzt Ihre Azure Web app durch DNS A-Datensatz die Domäne zuweisen. 

Es ist wichtig zu beachten Azure webapps CNAME und A-Einträge, akzeptiert aber *muss* einen A-Datensatz richtige Domänen-Mapping ermöglicht. CNAME kann nicht weitergeleitet werden, einem anderen CNAME ist was Azure mit YOUR_DOMAIN.azurewebsites.net erstellt.

Die IP-Adresse aus dem vorherigen Schritt, zurück an den DNS-Manager mit setup den A-Eintrag auf dieser IP-Adresse.


## <a name="install-and-setup-the-plugin"></a>Installation und Konfiguration des Plug-Ins

WordPress Multisite verfügt derzeit nicht integrierte Methode, um benutzerdefinierte Domänen zugeordnet. Allerdings ist eine Plugin namens [WordPress MU Domäne zuordnen] [ wordpress-plugin-wordpress-mu-domain-mapping] , die Funktionalität für Sie hinzufügt. Melden Sie sich beim Netzwerkadministrator Teil Ihrer Website, und installieren Sie Plugin **WordPress MU Domäne zuordnen** .

Nach dem Installieren und aktivieren das Plug-in finden Sie auf **Einstellungen** > Plug-in konfigurieren**Domäne zuordnen** . Geben Sie in das erste Textfeld *IP-Adresse*die IP-Adresse verwendet, den A-Eintrag für die Domäne eingerichtet. Legen Sie alle gewünschten *Optionen* (Standardeinstellungen sind oft gut), und klicken Sie auf **Speichern**.

## <a name="map-the-domain"></a>Zuordnung der Domäne

Besuchen Sie das **Dashboard** für den Standort, die Domäne zugeordnet werden soll. Klicken Sie auf **Extras** > **Domäne zuordnen** und geben Sie die neue Domäne in das Textfeld, und klicken Sie auf **Hinzufügen**.

Standardmäßig wird die neue Domäne Domäne Website automatisch umgeschrieben werden. Wenn alle Datenverkehr in die neue Domäne sollen, aktivieren Sie die *primäre Domäne für diesen Blog* vor dem Speichern. Eine Website kann eine unbegrenzte Anzahl von Domänen hinzufügen, jedoch kann nur eine primäre.

## <a name="do-it-again"></a>Wiederholen

Azure Web Apps können Sie eine unbegrenzte Anzahl von Domänen eine Webanwendung hinzufügen. Hinzufügen einer anderen Domäne müssen Sie die Abschnitten **Überprüfen Ihrer Domäne** und **einrichten die Domäne einen Datensatz** für jede Domäne ausführen.  

>[AZURE.NOTE] Wenn Sie mit Azure App Service beginnen, bevor Sie sich für ein Azure-Konto, gehen Sie [Versuchen App Service](http://go.microsoft.com/fwlink/?LinkId=523751)sofort eine kurzlebige Starter Web app in App Service können Sie erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="whats-changed"></a>Was hat sich geändert
* Eine Anleitung zur Änderung von Websites zu App Service finden Sie unter: [Azure App Service und seine Auswirkung auf vorhandene Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

[ben-lobaugh]: http://ben.lobaugh.net
[ms-open-tech]: http://msopentech.com
[website-from-gallery]: https://www.windowsazure.com/develop/php/tutorials/website-from-gallery/
[wordpress-codex-create-a-network]: http://codex.wordpress.org/Create_A_Network
[website-w-mysql-and-ftp-ftp-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-ftp/#header-0
[website-w-mysql-and-git-git-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-git/#header-1
[wordpress-network-setup]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-network-setup.png
[wordpress-codex-types-of-networks]: http://codex.wordpress.org/Before_You_Create_A_Network#Types_of_multisite_network
[wordpress-plugin-wordpress-mu-domain-mapping]: http://wordpress.org/extend/plugins/wordpress-mu-domain-mapping/

[wordpress-manage-domains]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-manage-domains.png

 
