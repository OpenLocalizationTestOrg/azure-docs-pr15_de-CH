<properties
  pageTitle="Nutzen Sie DevOps-Umgebung für Ihre Webanwendung"
  description="Informationen Sie zur Verwendung von Bereitstellung Steckplätze einrichten und Verwalten von mehreren Umgebungen für Ihre Anwendung"
  services="app-service\web"
  documentationCenter=""
  authors="sunbuild"
  manager="yochayk"
  editor=""/>

<tags
  ms.service="app-service"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="web"
  ms.date="10/24/2016"
  ms.author="sumuth"/>

# <a name="use-devops-environments-effectively-for-your-web-apps"></a>Nutzen Sie DevOps-Umgebung für Ihre Web-apps

Dieser Artikel beschreibt das Einrichten und Verwalten von Web-anwendungsbereitstellung für mehrere Versionen einer Anwendung, wie QA und Produktion bereitstellen. Jede Version der Anwendung kann als Umgebung für Bedarf der Bereitstellung gelten. QS-Umgebung kann beispielsweise von Ihrem Team die Qualität der Anwendung testen, bevor die Änderungen Produktion push verwendet werden.
Einrichten von mehreren Umgebungen kann eine Herausforderung sein, Bedarf verfolgen, Ressourcen (Compute WebApp Datenbank, Cache usw.) und Code in der Umgebung bereitstellen.

## <a name="setting-up-a-non-production-environment-stagedevqa"></a>Einrichten einer nicht produktiven Umgebung (Phase, Entwickler, QA)
Haben Sie eine Produktion Web app laufen, besteht der nächste Schritt zum Erstellen einer nicht-produktiven Umgebung. Nutzung unbedingt Bereitstellung Steckplätze im Plan **Standard** oder **Premium** App Service ausgeführt werden. Bereitstellung Steckplätze sind tatsächlich live webapps mit eigenen Hostnamen. Web app Inhalts- und Elemente können zwischen zwei Bereitstellung, einschließlich Produktion Steckplatz ausgetauscht werden. Bereitstellen der Anwendung in einen Steckplatz Bereitstellung bietet folgende Vorteile:

1. Sie können Web app ändert sich in einen Stagingbereich Bereitstellung Steckplatz vor Austausch mit der Produktion überprüfen.
2. Web app zuerst in einen Steckplatz bereitstellen und Austausch in der Produktion wird sichergestellt, dass alle Instanzen des Feldes vor in Betrieb ausgetauscht erwärmt werden. Dadurch werden Ausfallzeiten beim Bereitstellen Ihrer Anwendung. Die Umleitung Datenverkehr ist nahtlos und keine Anfragen durch Swap-Vorgänge gelöscht. Diese gesamten Workflow automatisiert konfigurieren [Automatisch austauschen](web-sites-staged-publishing.md#configure-auto-swap-for-your-web-app) , wenn vor Swap-Prüfung nicht benötigt wird.
3. Nach einem Austausch mittlerweile mit zuvor bereitgestellte WebApp vorherigen Produktion Web app. In der Produktion Steckplatz getauscht Änderungen nicht wie erwartet, können ausführen derselbe Swap sofort zu Ihrer "letzten bekannten guten Anwendung" zurück.

Um einen Stagingserver Bereitstellung Steckplatz einzurichten, finden Sie unter [Stagingumgebungen für Web-apps in Azure App Service einrichten](web-sites-staged-publishing.md). Jede Umgebung sollte einen eigenen Satz von Ressourcen enthalten, Beispiel wenn web app eine Datenbank verwendet Produktion und staging WebApp Datenbanken verwenden werden. Fügen Sie staging-Umgebung Ressourcen wie Datenbank, Speicher oder Cache festlegen staging Umgebung hinzu.

## <a name="examples-of-using-multiple-development-environments"></a>Beispiele für die Verwendung mehrerer Umgebungen

Jedes Projekt muss eine Quellcode-Management mit mindestens zwei Unternehmen, Entwicklung und Produktion Umgebung bei Content Management Systeme Anwendungsframeworks usw. wir stoßen kann Probleme, wo die Anwendung dabei standardmäßig nicht unterstützt. Dies gilt für einige beliebte Frameworks erläutert. Beim Arbeiten mit CMS/Frameworks wie kommen viele Fragen in den Sinn

1. Wie Sie sich in unterschiedlichen Umgebungen aufteilen
2. Welche Dateien ändern und pflegen können auswirken Framework-Versionen
3. Pro Umgebung verwalten
4. Verwalten von Modulen-Plugins Versionsupdates Core Framework-Versionen

Es gibt viele Methoden zum Einrichten einer Umgebung mit mehreren für das Projekt und die Beispiele sind eine nur eine solche Methode der jeweiligen Anwendung.

### <a name="wordpress"></a>WordPress
In diesem Abschnitt erfahren Sie, wie eine Bereitstellungsworkflow mit Steckplätzen für WordPress eingerichtet. WordPress wie die meisten Lösungen mit mehreren Umgebungen standardmäßig nicht unterstützt. App Service Web Apps hat einige Features, die die Konfiguration Einstellungen außerhalb des Code vereinfachen.

Vor dem Erstellen eines Stagingdatenbank Steckplatz Anwendungscode mehreren Umgebungen Unterstützung eingerichtet. Umgebung mit mehreren WordPress unterstützen Sie bearbeiten müssen `wp-config.php` in Ihrer Entwicklung Anwendung fügen Sie den folgenden Code am Anfang der Datei. Dadurch kann die Anwendung die richtige Konfiguration basierend auf der ausgewählten Umgebung auswählen.

```
// Support multiple environments
// set the config file based on current environment
if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) {
// local development
 $config_file = 'config/wp-config.local.php';
}
elseif ((strpos(getenv('WP_ENV'),'stage') !== false) || (strpos(getenv('WP_ENV'),'prod' )!== false ))
//single file for all azure development environments
 $config_file = 'config/wp-config.azure.php';
}
$path = dirname(__FILE__). '/';
if (file_exists($path. $config_file)) {
// include the config file if it exists, otherwise WP is going to fail
require_once $path. $config_file;
```

Erstellen Sie einen Ordner unter Web app Root aufgerufen `config` und eine Datei zwei Dateien: `wp-config.azure.php` und `wp-config.local.php` bzw. Ihrer Azure und lokalen Umgebung darstellt.

Kopieren Sie die folgenden in `wp-config.local.php` :

```
<?php
// MySQL settings
/** The name of the database for WordPress */

define('DB_NAME', 'yourdatabasename');

/** MySQL database username */
define('DB_USER', 'yourdbuser');

/** MySQL database password */
define('DB_PASSWORD', 'yourpassword');

/** MySQL hostname */
define('DB_HOST', 'localhost');
/**
 * For developers: WordPress debugging mode.
 * * Change this to true to enable the display of notices during development.
 * It is strongly recommended that plugin and theme developers use WP_DEBUG
 * in their development environments.
 */
define('WP_DEBUG', true);

//Security key settings
define('AUTH_KEY', 'put your unique phrase here');
define('SECURE_AUTH_KEY','put your unique phrase here');
define('LOGGED_IN_KEY','put your unique phrase here');
define('NONCE_KEY', 'put your unique phrase here');
define('AUTH_SALT', 'put your unique phrase here');
define('SECURE_AUTH_SALT', 'put your unique phrase here');
define('LOGGED_IN_SALT', 'put your unique phrase here');
define('NONCE_SALT', 'put your unique phrase here');

/**
 * WordPress Database Table prefix.
 *
 * You can have multiple installations in one database if you give each a unique
 * prefix. Only numbers, letters, and underscores please!
 */
$table_prefix = 'wp_';
```

Festlegen der obigen Sicherheitsschlüssel kann Hilfe verhindert, dass Ihrer Anwendung gehackt, so eindeutige Werte. Benötigen Sie die Zeichenfolge für die oben genannten Sicherheitsschlüssel generiert, gelangen Sie zu der automatischen Generator erstellen neue Schlüssel/Werte mit diesem [Link] (https://api.wordpress.org/secret-key/1.1/salt)

Kopieren Sie folgenden Code im `wp-config.azure.php`:


``` <?php
    // MySQL settings
    /** The name of the database for WordPress */
    
    define('DB_NAME', getenv('DB_NAME'));
    
    /** MySQL database username */
    define('DB_USER', getenv('DB_USER'));
    
    /** MySQL database password */
    define('DB_PASSWORD', getenv('DB_PASSWORD'));
    
    /** MySQL hostname */
    define('DB_HOST', getenv('DB_HOST'));
    
    /**
    * For developers: WordPress debugging mode.
    *
    * Change this to true to enable the display of notices during development.
    * It is strongly recommended that plugin and theme developers use WP_DEBUG
    * in their development environments.
    * Turn on debug logging to investigate issues without displaying to end user. For WP_DEBUG_LOG to
    * do anything, WP_DEBUG must be enabled (true). WP_DEBUG_DISPLAY should be used in conjunction
    * with WP_DEBUG_LOG so that errors are not displayed on the page */
    
    */
    define('WP_DEBUG', getenv('WP_DEBUG'));
    define('WP_DEBUG_LOG', getenv('TURN_ON_DEBUG_LOG'));
    define('WP_DEBUG_DISPLAY',false);
    
    //Security key settings
    /** If you need to generate the string for security keys mentioned above, you can go the automatic generator to create new keys/values: https://api.wordpress.org/secret-key/1.1/salt **/
    define('AUTH_KEY',getenv('DB_AUTH_KEY'));
    define('SECURE_AUTH_KEY', getenv('DB_SECURE_AUTH_KEY'));
    define('LOGGED_IN_KEY', getenv('DB_LOGGED_IN_KEY'));
    define('NONCE_KEY', getenv('DB_NONCE_KEY'));
    define('AUTH_SALT', getenv('DB_AUTH_SALT'));
    define('SECURE_AUTH_SALT', getenv('DB_SECURE_AUTH_SALT'));
    define('LOGGED_IN_SALT',  getenv('DB_LOGGED_IN_SALT'));
    define('NONCE_SALT',  getenv('DB_NONCE_SALT'));
    
    /**
    * WordPress Database Table prefix.
    *
    * You can have multiple installations in one database if you give each a unique
    * prefix. Only numbers, letters, and underscores please!
    */
    $table_prefix = getenv('DB_PREFIX');
```

#### <a name="use-relative-paths"></a>Relative Pfade verwenden
Eine Sache werden konfigurieren WordPress app relative Pfade verwendet. WordPress-URL-Informationen in der Datenbank gespeichert. Dadurch Verschieben von Inhalt aus einer Umgebung auf einen anderen müssen Sie die Datenbank aktualisieren, jedes Mal Sie lokal oder sich schwieriger. Verringern der Probleme, die bei der Bereitstellung einer Datenbank jedes Mal aus einer Umgebung für einen anderen [Stamm Relative Links Plugin](https://wordpress.org/plugins/root-relative-urls/) Bereitstellen über WordPress Administrator Dashboard installiert oder manuell [hier](https://downloads.wordpress.org/plugin/root-relative-urls.zip)herunterladen verursacht werden können.


Die folgenden Einträge, die `wp-config.php` Datei vor der `That's all, stop editing!` Kommentar:

```

  define('WP_HOME', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_SITEURL', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_CONTENT_URL', '/wp-content');
    define('DOMAIN_CURRENT_SITE', filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
```

Aktivieren Sie das Plug-in über die `Plugins` in WordPress Administrator Dashboard im Menü. Speichern Sie die Permalink WordPress App.

#### <a name="the-final-wp-configphp-file"></a>Die endgültige `wp-config.php` Datei
WordPress Core Updates hat keinen Einfluss auf die `wp-config.php`, `wp-config.azure.php` und `wp-config.local.php` Dateien. Am Ende dieser Anleitung `wp-config.php` Datei wird wie folgt aussehen

```
<?php
/**
 * The base configurations of the WordPress.
 *
 * This file has the following configurations: MySQL settings, Table Prefix,
 * Secret Keys, and ABSPATH. You can find more information by visiting
 *
 * Codex page. You can get the MySQL settings from your web host.
 *
 * This file is used by the wp-config.php creation script during the
 * installation. You don't have to use the web web app, you can just copy this file
 * to "wp-config.php" and fill in the values.
 *
 * @package WordPress
 */

// Support multiple environments
// set the config file based on current environment
if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) { // local development
  $config_file = 'config/wp-config.local.php';
}
elseif ((strpos(getenv('WP_ENV'),'stage') !== false) ||(strpos(getenv('WP_ENV'),'prod' )!== false )){
  $config_file = 'config/wp-config.azure.php';
}


$path = dirname(__FILE__). '/';
if (file_exists($path. $config_file)) {
  // include the config file if it exists, otherwise WP is going to fail
  require_once $path. $config_file;
}

/** Database Charset to use in creating database tables. */
define('DB_CHARSET', 'utf8');

/** The Database Collate type. Don't change this if in doubt. */
define('DB_COLLATE', '');


/* That's all, stop editing! Happy blogging. */

define('WP_HOME', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_SITEURL', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_CONTENT_URL', '/wp-content');
define('DOMAIN_CURRENT_SITE', $_SERVER['HTTP_HOST']);

/** Absolute path to the WordPress directory. */
if ( !defined('ABSPATH') )
    define('ABSPATH', dirname(__FILE__). '/');

/** Sets up WordPress vars and included files. */
require_once(ABSPATH. 'wp-settings.php');
```

#### <a name="set-up-a-staging-environment"></a>Staging-Umgebung einrichten
Wenn Sie bereits eine WordPress Web app auf Azure Web bei [Azure Management Vorschau Portal](http://portal.azure.com) und gehen Sie zu Ihrer Anwendung WordPress. Wenn Apps können nicht aus dem Markt erstellen. [Hier](web-sites-php-web-site-gallery.md)erfahren.
Klicken Sie auf Einstellungen -> Bereitstellung Steckplätze -> hinzufügen, um eine Bereitstellung Steckplatz mit dem Namen erstellen. Bereitstellung Steckplatz ist einem anderen dieselben Ressourcen wie die oben erstellte primäre Web app freigeben.

![Phase Bereitstellung Steckplatz erstellen](./media/app-service-web-staged-publishing-realworld-scenarios/1setupstage.png)

Hinzufügen einer anderen MySQL-Datenbank z. B. `wordpress-stage-db` der Ressourcengruppe `wordpressapp-group`.

 ![MySQL-Datenbank zu Ressourcengruppe hinzufügen](./media/app-service-web-staged-publishing-realworld-scenarios/2addmysql.png)

Aktualisieren Sie die Verbindungszeichenfolgen für die Bühne Bereitstellung Steckplatz neu erstellte Datenbank, `wordpress-stage-db`. Beachten Sie, dass die Produktion app Web `wordpressprodapp` und staging WebApp `wordpressprodapp-stage` auf verschiedene Datenbanken verweisen.

#### <a name="configure-environment-specific-app-settings"></a>Umgebung-spezifische Anwendung konfigurieren
Entwickler können in Azure als Teil der Konfigurationsinformationen von einem Web app namens App Settings Zeichenfolge mit Schlüssel-Wert-Paare speichern. Zur Laufzeit App Service Web Apps automatisch ruft diese Werte für Sie und macht sie in Ihrer Anwendung ausgeführten Code verfügbar. Sicherheit ist eine schöne Perspektive profitieren da vertrauliche Informationen wie Datenbankverbindungszeichenfolgen, Kennwörter niemals unverschlüsselt in einer Datei wie oben `wp-config.php`.

Diese unten ist sinnvoll bei geändert und Datenbank ändert WordPress App enthält:
- WordPress-Version aktualisieren
- Neue hinzufügen oder bearbeiten oder Aktualisieren von Plug-Ins
- Hinzufügen neuer oder bearbeiten oder Aktualisieren von Themen

Konfigurieren Sie Anwendung für:

- Datenbankinformationen
- ein-und WordPress-Protokollierung
- WordPress-Sicherheitsrichtlinien

![App-Einstellungen für Wordpress WebApp](./media/app-service-web-staged-publishing-realworld-scenarios/3configure.png)

Stellen Sie sicher, dass Folgendes app für Ihre Produktion Web app und Bühne Steckplatz hinzugefügt haben. Beachten Sie, dass die Produktion WebApp und Staging WebApp verschiedene Datenbanken.
Deaktivieren Sie Kontrollkästchen **Steckplatz Einstellung** Einstellungen alle außer WP_ENV. Dadurch wird die Konfiguration Ihrer Anwendung, Datenbank und Inhalt ersetzen. Wenn **Steckplatz** **aktiviert**ist, werden Web app Appeinstellungen und Konfiguration Verbindungszeichenfolge nicht umgebungsübergreifend bei einem Swap und daher datenbankänderungen vorhanden sind dies bricht nicht Ihrer produktionsanwendung.

Bereitstellen Sie lokale Entwicklung Umgebung Web app für Phase WebApp und Datenbank mit WebMatrix oder Tools Ihrer Wahl wie FTP, Git oder PhpMyAdmin.

![Web Matrix veröffentlichen Dialogfeld WordPress Web App](./media/app-service-web-staged-publishing-realworld-scenarios/4wmpublish.png)

Durchsuchen und Bereitstellung Ihrer Anwendung zu testen. Ein Szenario, in dem das Design der Web-app aktualisiert werden, da ist hier die Stagingdatenbank Web app.

![Staging-WebApp vor Austausch Steckplätze durchsuchen](./media/app-service-web-staged-publishing-realworld-scenarios/5wpstage.png)


 Wenn alles in Ordnung, klicken Sie auf Ihrer Anwendung die Inhalte dieser Umgebung zu staging Schaltfläche **austauschen** . In diesem Fall tauschen Sie Web app und die Datenbank umgebungsübergreifend bei **jedem Austauschvorgang** .

![Tauschen Sie WordPress ändert](./media/app-service-web-staged-publishing-realworld-scenarios/6swaps1.png)

 > [AZURE.NOTE]
 >Wenn Sie ein Szenario haben, in denen Sie nur Dateien (keine Datenbankupdates) müssen, verknüpft dann **Steckplatz Einstellung** der Datenbank **Überprüfen** *app* und *Zeichenfolgen Einstellungen* im Web app Einstellung Blade im vorschauportal Azure vor dem Austausch. In diesem Fall DB_NAME, DB_HOST, DB_PASSWORD, DB_USER sollte Standardeinstellung Connection String nicht in ändert angezeigt Wenn ein **Swap**. Zu dieser Zeit, wenn Sie **tauschen** Vorgang WordPress Web app Updates muss **nur**Dateien.

Zuvor eine SWAP hier ist die Produktion WordPress Web app ![Produktion Web app vor Austausch Steckplätze](./media/app-service-web-staged-publishing-realworld-scenarios/7bfswap.png)

Nach der Operation SWAP wurde das Design Ihrer Produktion Web App aktualisiert.

![Produktion Web app nach dem Austausch Steckplätze](./media/app-service-web-staged-publishing-realworld-scenarios/8afswap.png)

In einer Situation beim **Zurücksetzen**, Sie müssen können Sie zur Produktion Web app Settings und klicken auf die Schaltfläche **Swap** WebApp und staging-Steckplatz aus Datenbank ausgetauscht. Ein wichtiger ist, dass Änderungen mit einem **Swap** jederzeit werden Sie Ihrer Bereitstellung Anwendung erneut bereitstellen, die Sie die Datenbank müssen das nächste Mal auf die aktuelle Datenbank für Ihre Bereitstellung Web app wechselt der vorherigen Produktionsdatenbank oder die Datenbank.

#### <a name="summary"></a>Zusammenfassung
Den Prozess für jede Anwendung mit einer Datenbank zu verallgemeinern

1. Installieren Sie die Anwendung in der lokalen Umgebung
2. Konfigurieren Sie Umgebung bestimmte (lokale und Azure Web App)
3. Richten Sie Ihre Umgebung in App Service Web Apps-Staging und Produktion
4. Haben Sie eine bereits auf Azure ausgeführte produktionsanwendung synchronisieren Sie Ihre Produktion Inhalt (Dateien-Code und Datenbank) lokale und staging-Umgebung.
5. Entwickeln Sie Ihre Anwendung in der lokalen Umgebung
6. Platzieren Sie Ihrer produktionsanwendung Wartung oder gesperrten Modus und Datenbank Content aus Staging und Entwickler
7. Bereitstellen Sie und Staging und testen
8. Bereitstellung auf
9. Wiederholen Sie die Schritte 4 bis 6

### <a name="umbraco"></a>Umbraco
In diesem Abschnitt erfahren Sie, wie Umbraco CMS ein benutzerdefiniertes Modul zur Bereitstellung von über mehrere DevOps-Umgebung verwendet. Dieses Beispiel bietet einen anderen Ansatz zum Verwalten von mehreren Umgebungen.

[Umbraco CMS](http://umbraco.com/) ist eine popular.NET CMS Lösungen von vielen Entwicklern bietet [Courier2](http://umbraco.com/products/more-add-ons/courier-2) Modul Entwicklung Staging zur Produktion bereitstellen. Sie können einfach eine lokale Entwicklungsumgebung für eine Umbraco CMS Web-Anwendung mit Visual Studio oder WebMatrix erstellen.

1. Erstellen Sie eine Umbraco Web app mit Visual Studio, [Klicken Sie hier](https://our.umbraco.org/documentation/Installation/install-umbraco-with-nuget).
2. Eine Umbraco Web app mit WebMatrix [hier](http://umbraco.com/help-and-support/video-tutorials/getting-started/working-with-webmatrix)erstellen.

Entfernen Sie stets die `install` Ordner unter der Anwendung nie auf Stufe oder Produktion webapps hochladen. In diesem Lernprogramm werden ich WebMatrix verwenden

#### <a name="set-up-a-staging-environment"></a>Einrichten einer Stagingumgebung
- Erstellen Sie einen Bereitstellung Steckplatz erwähnt Umbraco CMS Web App, vorausgesetzt, Sie bereits eine Umbraco CMS Web app einrichten und ausführen. Andernfalls können aus dem Markt.

- Aktualisieren Sie die Verbindungszeichenfolge für die Bühne Bereitstellung Steckplatz neu erstellte Datenbank, **Umbraco Phase Db**. Die Produktion Web app (Umbraositecms-1) und staging WebApp (Umbracositecms-1-Phase) **muss** verweisen auf verschiedene Datenbanken.

![Aktualisieren Sie Verbindungszeichenfolge für staging-Web-app mit neuen Stagingdatenbank](./media/app-service-web-staged-publishing-realworld-scenarios/9umbconnstr.png)

- Klicken Sie auf **Veröffentlichen erhalten Einstellungen** Bereitstellung Steckplatz **Phase**. Dadurch wird eine Datei veröffentlichen, die alle Angaben von Visual Studio oder Web Matrix Anwendung lokale Entwicklung WebApp Azure Web App Veröffentlichen speichern herunterladen.

 ![Festlegen der Stagingdatenbank Web app veröffentlichen Sie erhalten](./media/app-service-web-staged-publishing-realworld-scenarios/10getpsetting.png)

- Öffnen Sie Ihrer Anwendung lokale Entwicklung in **WebMatrix** oder **Visual Studio**. In diesem Lernprogramm Web Matrix verwenden und müssen Sie zunächst die Einstellungsdatei Veröffentlichen Ihrer Bereitstellung Anwendung importieren

![Importieren Sie veröffentlichen für Web Matrix mit Umbraco](./media/app-service-web-staged-publishing-realworld-scenarios/11import.png)

- Überprüfen Sie ändern Sie im Dialogfeld und Bereitstellen Sie Ihrer lokalen Anwendung für Ihre Azure-Web-Anwendung *Umbracositecms-1-Phase*. Wenn Dateien direkt auf Ihrer Bereitstellung Anwendung bereitstellen lassen Sie Dateien in die `~/app_data/TEMP/` Ordner wie diese wiederhergestellt werden, wenn die Bühne Web app zuerst gestartet. Auch auslassen soll die `~/app_data/umbraco.config` , Datei, neu generiert.

![Veröffentlichen im Web Matrix Änderungen](./media/app-service-web-staged-publishing-realworld-scenarios/12umbpublish.png)

- Nach der erfolgreichen Veröffentlichung Umbraco lokales Web app zu staging-WebApp, nach Ihrer Bereitstellung Anwendung und führen Sie einige Tests, um Probleme.

#### <a name="set-up-courier2-deployment-module"></a>Courier2 Bereitstellung Modul einrichten
Sie können mit [Courier2](http://umbraco.com/products/more-add-ons/courier-2) Inhalt, Stylesheets, Entwicklung Module drücken und mit einer einfachen auf staging Web App Produktion Web App für eine weitere Aufwand kostenlose Bereitstellung gesenkt und Risiken Ihrer produktionsanwendung beschädigen, wenn ein Update bereitstellen.
Eine Lizenz für Courier2 für die Domäne `*.azurewebsites.net` und Ihre benutzerdefinierte Domain (z. B. http://abc.com) nach der Lizenz heruntergeladenen License platzieren (. LIC-Datei) in die `bin` Ordner.

![Lizenzdatei Bin-Ordner ablegen](./media/app-service-web-staged-publishing-realworld-scenarios/13droplic.png)

Paket Courier2 [hier](https://our.umbraco.org/projects/umbraco-pro/umbraco-courier-2/). Melden Sie sich bei Ihrer Anwendung Phase, http://umbracocms-site-stage.azurewebsites.net/umbraco sagen und **Developer** -Menü und wählen Sie **Pakete**auf. Klicken Sie auf Lokales Paket **Installieren**

![Umbraco Package installer](./media/app-service-web-staged-publishing-realworld-scenarios/14umbpkg.png)

Hochladen des courier2-Pakets mit dem Installer.

![Paket für Courier-Modul laden](./media/app-service-web-staged-publishing-realworld-scenarios/15umbloadpkg.png)

Konfigurieren Sie müssen courier.config Datei **Config** -Ordner von Ihrer Anwendung aktualisieren.

```xml
<!-- Repository connection settings -->
 <!-- For each site, a custom repository must be configured, so Courier knows how to connect and authenticate-->
 <repositories>
    <!-- If a custom Umbraco Membership provider is used, specify login & password + set the passwordEncoding to clear: -->
    <repository name="production web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
      <url>http://umbracositecms-1.azurewebsites.net</url>
      <user>0</user>
      <!--<login>user@email.com</login> -->
      <!-- <password>user_password</password>-->
      <!-- <passwordEncoding>Clear</passwordEncoding>-->
      </repository>
 </repositories>
 ```

Unter `<repositories>`, geben Sie die Produktion Website-URL und Benutzer. Wenn Umbraco Standardmitgliedschaftsanbieter verwenden, fügen Sie die ID für den Benutzer Verwaltung in <user> Abschnitt. Wenn Sie einen benutzerdefinierten Mitgliedschaftsanbieter Umbraco verwenden `<login>`,`<password>` Courier2 Modul können Standort herstellen. Weitere Informationen finden Sie in der [Dokumentation](http://umbraco.com/help-and-support/customer-area/courier-2-support-and-download/developer-documentation) Courier-Modul.

Entsprechend Courier-Modul auf Ihrem installieren und konfigurieren auf Stufe WebApp in der jeweiligen courier.config-Datei wie hier dargestellt

```xml
 <!-- Repository connection settings -->
 <!-- For each site, a custom repository must be configured, so Courier knows how to connect and authenticate-->
 <repositories>
    <!-- If a custom Umbraco Membership provider is used, specify login & password + set the passwordEncoding to clear: -->
    <repository name="Stage web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
      <url>http://umbracositecms-1-stage.azurewebsites.net</url>
      <user>0</user>
      </repository>
 </repositories>
```

Klicken Sie auf Courier2 Umbraco CMS Web app Dashboard und wählen Sie Speicherorte aus. Repository-Name sollte angezeigt werden, wie unter `courier.config`. Hierzu auf Produktion und staging-webapps.

![Ansicht Ziel Web app repository](./media/app-service-web-staged-publishing-realworld-scenarios/16courierloc.png)

Jetzt können zum Produktionsstandort Bereitstellungsstandort Inhalten bereitstellen. Zum Inhalt und eine vorhandene Seite auswählen oder eine neue Seite erstellen. Ich meine Web app, der Titel der Seite **erste** Schritte – neue geändert wird, eine vorhandene Seite auswählen und klicken Sie auf **Speichern und veröffentlichen**.

![Titel der Seite ändern und veröffentlichen](./media/app-service-web-staged-publishing-realworld-scenarios/17changepg.png)

Wählen Sie jetzt geänderten Seite und *Klicken Sie mit der rechten Maustaste* , um die Optionen anzuzeigen. Klicken Sie auf **Courier** Bereitstellung Dialogfeld anzeigen. Klicken Sie auf **Bereitstellen** Bereitstellung initiieren

![Courier Modul Bereitstellung Dialogfeld](./media/app-service-web-staged-publishing-realworld-scenarios/18dialog1.png)

Änderungen Sie die, und klicken Sie auf Weiter.

![Courier Modul Bereitstellung Dialogfeld Änderungen](./media/app-service-web-staged-publishing-realworld-scenarios/19dialog2.png)

Bereitstellungsprotokoll zeigt, ob die Bereitstellung erfolgreich war.

 ![Bereitstellungsprotokolle Courier Modul anzeigen](./media/app-service-web-staged-publishing-realworld-scenarios/20successdlg.png)

Durchsuchen Sie Ihrer produktionsanwendung um festzustellen, ob die Änderungen.

 ![Produktion WebApp durchsuchen](./media/app-service-web-staged-publishing-realworld-scenarios/21umbpg.png)

Weitere Informationen zum Courier verwenden, lesen Sie die Dokumentation.

#### <a name="how-to-upgrade-umbraco-cms-version"></a>Wie Umbraco CMS Version aktualisieren

Courier hilft nicht bei der Aktualisierung einer Version von Umbraco CMS auf einen anderen bereitstellen. Umbraco CMS-Version aktualisieren, müssen Sie mit Ihrer benutzerdefinierten Module oder Drittanbieter-Module und Bibliotheken Umbraco Core Inkompatibilitäten überprüfen. Als bewährte Methode

1. Sichern Sie Ihr WebApp und Datenbank immer vor der Aktualisierung. Azure Web App lassen sich automatische Backups für Ihre Websites mit der Sicherung verfügen und die Site Wiederherstellen mit Funktion Wiederherstellen benötigt. Weitere Informationen finden Sie in [Ihrer Anwendung sichern](web-sites-backup.md) und [Wiederherstellen Ihrer Anwendung](web-sites-restore.md).

2. Überprüfen Sie Drittanbieterpakete verwendeten kompatibel mit der Version, die Sie aktualisieren möchten. Des Pakets herunterladen Sie Seite, überprüfen Sie Projekt-Kompatibilität mit Umbraco CMS-Version.

Weitere Informationen zum Aktualisieren Ihrer Anwendung lokal Richtlinien Sie die wie erwähnt [hier](https://our.umbraco.org/documentation/getting-started/set up/upgrading/general).

Aktualisieren der lokalen Entwicklungssite veröffentlichen Sie die Änderungen staging-WebApp. Testen Sie Ihre Anwendung und wenn alles in Ordnung, Schaltfläche **austauschen** **tauschen** Ihre Bereitstellungsstandort Produktion WebApp. Beim Ausführen der Operation **austauschen** können Sie Änderungen, die betroffen sind in Ihrem Web app Konfiguration anzeigen. Mit diesem Vorgang **Swap** sind wir webapps und Datenbanken austauschen. Dies bedeutet SWAP Produktion Web app wird jetzt Umbraco Phase Db Datenbank und staging WebApp Umbraco-FA-Db-Datenbank verweist.

![Tauschen Sie Vorschau Umbraco CMS bereitstellen](./media/app-service-web-staged-publishing-realworld-scenarios/22umbswap.png)

Der Vorteil der Austausch der WebApp und die Datenbank:
1. Können Sie die vorherige Version Ihrer Anwendung mit einer anderen **tauschen** Rollback, gibt es Probleme.
2. Für eine Aktualisierung müssen Sie Dateien und staging-Web app Produktion WebApp und Datenbank-Datenbank bereitstellen. Es gibt viele Faktoren können bei der Bereitstellung von Dateien und Datenbanken. Mithilfe der **Swap** -Funktion von Zeitnischen wir Ausfallzeiten während einer Aktualisierung und das Risiko von Fehlern, die auftreten können, wenn Änderungen bereitstellen.
3. Gibt Ihnen die Möglichkeit, **A / B-Tests** Funktion [Testen in der Produktion](https://azure.microsoft.com/documentation/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/)

Dieses Beispiel zeigt die Flexibilität der Plattform, in dem Sie benutzerdefinierte Module wie Umbraco Courier Modul umgebungsübergreifend Bereitstellung zu erstellen.

## <a name="references"></a>Referenzen
[Agile-Softwareentwicklung mit Azure App Service](app-service-agile-software-development.md)

[Staging-Umgebungen für Web-apps in Azure App Service einrichten](web-sites-staged-publishing.md)

[Blockieren von Web Access nicht Produktionsbetrieb Steckplätze](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
