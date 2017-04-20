<properties
    pageTitle="Unternehmensklasse WordPress auf Azure App Service | Microsoft Azure"
    description="Erfahren Sie, wie ein Unternehmen WordPress-Site auf Azure App Service"
    services="app-service\web"
    documentationCenter=""
    authors="sunbuild"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.devlang="php"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="web"
    ms.date="10/24/2016"
    ms.author="sumuth"/>

# <a name="enterprise-class-wordpress-on-azure-app-service"></a>Unternehmensklasse WordPress Azure App-Dienst

Azure App Service bietet eine skalierbare, sichere und benutzerfreundliche Umgebung für Mission wichtige, in großem Maßstab [WordPress] [ wordpress] Websites. Microsoft selbst führt Unternehmen Websites wie [Office] [ officeblog] und [Bing] [ bingblog] Blogs. Dieses Dokument veranschaulicht die Verwendung Azure App Service Web Apps und Beibehalten einer Unternehmensklasse cloudbasierten WordPress-Website, die eine große Anzahl von Besuchern behandeln können.

## <a name="architecture-and-planning"></a>Architektur und Planung

Eine einfache Installation von WordPress hat nur zwei Anforderungen.

* **MySQL-Datenbank** - [ClearDB in Azure Marketplace]erhältlich[cdbnstore], oder Sie können eigene MySQL-Installation auf Azure virtuelle Maschinen mit [Windows] [ mysqlwindows] oder [Linux][mysqllinux].

  > [AZURE.NOTE] ClearDB bietet mehrere MySQL-Konfigurationen mit unterschiedlichen Performance-Merkmale für jede Konfiguration. Das [Azure-Speicher] [ cdbnstore] Informationen zu Azure gespeichert oder direkt auf [ClearDB Website](http://www.cleardb.com/pricing.view)bereitgestellt.

* **PHP 5.2.4 mindestens** -Azure App Service bieten derzeit [PHP Version 5.4, 5.5 und 5.6][phpwebsite].

    > [AZURE.NOTE] Es wird empfohlen, immer mit der neuesten Version von PHP, um sicherzustellen, dass Sie die neuesten Sicherheitspatches.

### <a name="basic-deployment"></a>Einfache Bereitstellung

Nur die Mindestanforderungen können Sie eine einfache Lösung in Azure Region erstellen.

![ein Azure WebApp und MySQL-Datenbank in eine Region Azure gehostet][basic-diagram]

Während diese Ihre Anwendung Skalierung von mehreren Web Apps Instanzen der Website ermöglichen würde, ist alles in Rechenzentren in einer bestimmten geografischen Region gehostet. Besucher von außerhalb dieses Bereichs möglicherweise langsam bei Verwendung der Website, und Rechenzentren in der Region, so wird die Anwendung angezeigt.


### <a name="multi-region-deployment"></a>Mit mehreren Bereitstellung

Mithilfe von Azure [Traffic Manager][trafficmanager], kann Ihre Website WordPress gleichzeitig nur eine URL für Besucher über mehrere Regionen skalieren. Alle Besucher in über Traffic Manager und dann auf einen Bereich basierend auf der Konfiguration zum Netzwerklastenausgleich weitergeleitet.

![ein Azure Web app gehostet in mehreren Regionen mit hoher Verfügbarkeit CDBR Router MySQL Regionen weiterleiten][multi-region-diagram]

Innerhalb jeder Region WordPress-Website würde weiterhin über mehrere Instanzen von Web Apps skaliert werden, aber diese Skalierung ist bestimmten; Bereiche mit hohem Datenverkehr können anders als geringem Datenverkehr zu skaliert werden.

Replikation und MySQL-Datenbanken routing möglich mithilfe ClearDBs [CDBR hohe Verfügbarkeit Router] [ cleardbscale] (links dargestellt) oder [MySQL Cluster CGE][cge].

### <a name="multi-region-deployment-with-media-storage-and-caching"></a>Bereitstellung mit mehreren Speichermedien und Zwischenspeichern
Verwenden Sie die Website hochladen oder Mediendateien Host akzeptiert, Azure BLOB-Speicher. Sie benötigen das Zwischenspeichern, sollten [Redis Cache][rediscache], [Memcache Cloud](http://azure.microsoft.com/gallery/store/garantiadata/memcached/), [MemCachier](http://azure.microsoft.com/gallery/store/memcachier/memcachier/)oder eines anderen Zwischenspeichern Angebote in den [Azure-Speicher](http://azure.microsoft.com/gallery/store/).

![ein Azure Web app gehostet in mehreren Regionen mit hoher Verfügbarkeit CDBR Router für MySQL Cache verwaltet BLOB-Speicher und CDN][performance-diagram]

BLOB-Speicher ist standardmäßig Regionen geografisch verteilten Dateien standortübergreifende Replikation kümmern müssen. Sie können auch den Azure [Content Distribution Network (CDN)] [ cdn] für BLOB-Speicher verteilt die Dateien näher Besucher Knoten beendet.

### <a name="planning"></a>Planung

#### <a name="additional-requirements"></a>Zusätzliche Vorschriften

Dazu... | Verwenden Sie diese...
------------------------|-----------
**Hochladen oder große Dateien** | [WordPress-Plugin für BLOB-Speicherung][storageplugin]
**E-Mail senden** | [SendGrid] [ storesendgrid] und [WordPress-Plugin für SendGrid verwenden][sendgridplugin]
**Benutzerdefinierte Domänennamen** | [Konfigurieren Sie einen benutzerdefinierten Domänennamen in Azure App Service][customdomain]
**HTTPS** | [Aktivieren Sie HTTPS für eine Webanwendung in Azure App Service][httpscustomdomain]
**Pre-Production-Validierung** | [Staging-Umgebungen für Web-apps in Azure App Service einrichten][staging] <p>Wechseln einer Web app staging zur Produktion auch verschiebt WordPress-Konfiguration. Sie sollten sicherstellen, dass alle Einstellungen für Ihre Produktion an Wechsel bereitgestellte Anwendung in der Produktion aktualisiert werden.</p>
**Überwachung und Problembehandlung** | [Aktivieren des Diagnoseprotokolls für Web-apps in Azure App Service] [ log] und [Monitor Web Apps in Azure App Service][monitor]
**Bereitstellung der site** | [Eine Webanwendung in Azure App Service bereitstellen][deploy]

#### <a name="availability-and-disaster-recovery"></a>Verfügbarkeit und Disaster recovery

Dazu... | Verwenden Sie diese...
------------------------|-----------
**Load Balance Websites** oder **Geo-Standorte verteilen** | [Datenverkehr mit Azure Traffic Manager][trafficmanager]
**Sichern und Wiederherstellen** | [Sichern Sie eine Webanwendung in Azure App Service] [ backup] und [eine Webanwendung in Azure App Service wiederherstellen][restore]

#### <a name="performance"></a>Leistung

Leistung in der Cloud erfolgt hauptsächlich durch caching und Dezentrales Skalieren; jedoch sollte der Speicher, Bandbreite und andere Attribute Web Apps hosting betrachtet.

Dazu... | Verwenden Sie diese...
------------------------|-----------
**App Service Instanz Funktionen verstehen** | [Nutzen der App Dienstebenen Preisdetails][websitepricing]
**Cache-Ressourcen** | [Redis Cache][rediscache], [Memcache Cloud](/gallery/store/garantiadata/memcached/), [MemCachier](/gallery/store/memcachier/memcachier/)oder eines anderen Zwischenspeichern Angebote in den [Azure-Speicher](/gallery/store/)
**Skalieren der Anwendung** | [Skalieren eine Webanwendung in Azure App Service] [ websitescale] und [ClearDB hohe Verfügbarkeit Routing][cleardbscale]. Möchten Sie hosten und Verwalten Ihrer eigenen Installation MySQL, sollten Sie [MySQL Cluster CGE] [ cge] für dezentrales Skalieren

#### <a name="migration"></a>Migration

Es gibt zwei Methoden der Migration einer vorhandenen Website WordPress Azure App Service.

* ** [WordPress exportieren] [ export] ** -exportiert den Inhalt Ihres Blogs auf einer neuen Website WordPress auf Azure App Service mit [WordPress Importer Plugin]importiert werden kann[import].

    > [AZURE.NOTE] Dadurch können Sie Ihre Inhalte migrieren migriert sie keine Plug-Ins, Designs oder andere benutzerdefinierte. Diese müssen erneut manuell installiert werden.

* **Manuelle Migration** - [Sichern Ihrer Website] [ wordpressbackup] [Datenbank]und[wordpressdbbackup], manuell eine Webanwendung in Azure App Service wiederherstellen und zugeordnete MySQL-Datenbank zu migrieren individuelle Websites immer wieder mühsam manuell installieren Plugins, Designs und andere Anpassungen vermeiden.

## <a name="step-by-step-instructions"></a>Anleitung

### <a name="create-a-wordpress-site"></a>Erstellen einer Website WordPress

1. Verwenden von [Azure Marketplace] [ cdbnstore] zum Erstellen einer MySQL-Datenbank Größe identifiziert [und](#planning) , im Abschnitt Region(en), dass Ihre Website gehostet wird.

2. Folgen Sie den Schritten unter [Erstellen einer WordPress Web app in Azure App Service] [ createwordpress] WordPress Web app erstellen. Web app erstellen, wählen **Sie eine vorhandene MySQL-Datenbank** und wählen Sie in Schritt 1 erstellte Datenbank.

Bei der Migration von einer vorhandenen WordPress-Website finden Sie nach dem Erstellen einer neuen Web-Applikation [Migrieren einer vorhandenen Website WordPress in Azure](#Migrate-an-existing-WordPress-site-to-Azure) .

### <a name="migrate-an-existing-wordpress-site-to-azure"></a>Migrieren einer vorhandenen WordPress-Website in Azure

Wie im Abschnitt [und](#planning) zweierlei WordPress Website migrieren.

* **Exportieren und importieren** - Sites ohne viel Anpassung oder wo Sie nur den Inhalt verschieben.

* **Sicherung und Wiederherstellung** für Sites mit viel Anpassung alles verschoben werden soll.

Verwenden Sie die folgenden Abschnitte zum Migrieren Ihrer Website.

#### <a name="the-export-and-import-method"></a>Export und Import-Methode

1. [WordPress] exportieren[ export] vorhandene Website exportieren.

2. Mit den Schritten im Abschnitt [Erstellen einer Website WordPress](#Create-a-new-WordPress-site) Web app erstellen.

3. Melden Sie sich bei Ihrer Website WordPress Web Apps und **Plugins**auf -> **Hinzufügen**. Suchen und **Einführer WordPress** -Plugin zu installieren.

4. Nach Importer-Plug-In installiert wurde, klicken Sie auf **Extras** -> **Importieren**und wählen Sie dann **WordPress** WordPress Importer Plugin verwenden.

5. Klicken Sie auf der Seite **Importieren WordPress** auf **Datei auswählen**. WXR Datei exportiert aus vorhandenen WordPress-Website durchsuchen Sie und wählen Sie dann **Hochladen und importieren**.

6. Klicken Sie auf **Senden**. Sie werden aufgefordert, dass der Import erfolgreich war.

8. Nachdem Sie diese Schritte abgeschlossen haben, starten Ihre Site aus der Web app Blade in [Azure-Portal][mgmtportal].

Nach dem Import der Website müssen Sie gehen, können nicht in der Importdatei enthalten sind.

Wenn Sie diese verwenden. | Aktion
------------------ | ----------
**Permalinks** | **WordPress Dashboard des neuen Standorts klicken** -> **Permalinks** und Permalinks Struktur aktualisieren
**Image-Medium links** | Verwenden Sie zum Aktualisieren von Links an den neuen Speicherort [samt Blues aktualisieren URLs Plugin][velvet], suchen und Ersetzen-Tool oder manuell in der Datenbank
**Themen** | **Darstellung**zur -> **Design** und Update Websitedesign Bedarf
**Menüs** | Design Menüs unterstützt, möglicherweise Links zur Startseite weiterhin das alte Verzeichnis eingebettet. **Darstellung**zur -> **Menüs** und aktualisieren

#### <a name="the-backup-and-restore-method"></a>Die Sicherung und Wiederherstellung-Methode

1. Sichern Sie Ihre vorhandenen WordPress Website [WordPress Backups]Informationen mit[wordpressbackup].

2. Sichern Sie die vorhandene Datenbank mit den Informationen in [der Datenbank][wordpressdbbackup].

3. Erstellen Sie eine Datenbank und Wiederherstellen Sie die Sicherung.

    1. Kaufen Sie eine neue Datenbank von [Azure Marketplace][cdbnstore], oder eine MySQL-Datenbank auf einem [Windows] [ mysqlwindows] oder [Linux] [ mysqllinux] VM.

    2. MySQL-Client wie [MySQL Workbench]mit[workbench], mit der neuen Datenbank verbinden und WordPress importieren.

    3. Aktualisieren Sie die Datenbank zur neuen Azure App Service Domäne Domäneneinträge ändern. Beispielsweise mywordpress.azurewebsites.net. Verwenden Sie [Suchen und Ersetzen von WordPress Datenbanken Skript] [ searchandreplace] problemlos alle Instanzen ändern.

4. Eine Webanwendung in Azure-Portal erstellen und Veröffentlichen von WordPress-Backup.

    1. Erstellen Sie eine Webanwendung in [Azure-Portal] [ mgmtportal] mit einer Datenbank mit **neu** -> **Web + Mobile** -> **Azure Marketplace** -> **Web Apps** -> **WebApp + SQL** (oder **WebApp + MySQL**) -> **Erstellen**. Konfigurieren Sie die gewünschten Einstellungen eine leeres Web app erstellen.

    2. Der Sicherung WordPress **wp-config.php** Datei suchen und in einem Editor geöffnet. Ersetzen Sie die folgenden Einträge mit den Informationen für die neue MySQL-Datenbank.

        * **DB_NAME** - Benutzername der Datenbank

        * **DB_USER** - Zugriff auf die Datenbank verwendeten Benutzernamen

        * **DB_PASSWORD** - das Kennwort

        Nach dem Ändern dieser Einträge, speichern und Schließen der Datei **wp-config.php** .

    3. [Bereitstellen einer Web-app in Azure App Service] verwenden[ deploy] Informationen Bereitstellungsmethode verwenden und sichern WordPress auf Ihrer Anwendung in Azure App Service bereitstellen möchten.

5. Nach der Bereitstellung der Website WordPress sollte man neuen Standort (als App Service Web app) mit Zugriff auf die *. azurewebsite.net-URL für die Website.

### <a name="configure-your-site"></a>Konfigurieren der Website

Nach dem Erstellen oder migriert WordPress-Website anhand der folgenden Informationen verbessern oder zusätzliche Funktionen aktivieren.

Dazu... | Verwenden Sie diese...
------------- | -----------
**App planmodus, Größe und Skalierung aktivieren** | [Eine Webanwendung in Azure App Service skalieren][websitescale]
**Beständige Datenbank Verbindungen** | Standardmäßig verwendet WordPress keine permanente Datenbankverbindungen führen die Verbindung zu der Datenbank nach mehrere Verbindungen eingeschränkt werden kann. Damit ständige Verbindung installieren (ständige Verbindung Adapter Plugin) [https://wordpress.org/plugins/persistent-database-connection-updater/installation/]. 
**Verbessern der Leistung** | <ul><li><p><a href="http://ppe.blogs.msdn.com/b/windowsazure/archive/2013/11/18/disabling-arr-s-instance-affinity-in-windows-azure-web-sites.aspx">ARR-Cookies deaktivieren</a> - können die Leistung verbessern mehrmals Web Apps WordPress unter</p></li><li><p>Zwischenspeichern. <a href="http://msdn.microsoft.com/library/azure/dn690470.aspx">Redis cache</a> (Vorschau) kann <a href="https://wordpress.org/plugins/redis-object-cache/">Redis Objekt Cache WordPress-Plugin</a>oder eine andere Zwischenspeichern Angebote aus dem <a href="/gallery/store/">Azure-Speicher</a> verwendet werden</p></li><li><p><a href="http://ruslany.net/2010/03/make-wordpress-faster-on-iis-with-wincache-1-1/">Wie Sie WordPress schneller mit Wincache</a> - Wincache ist für Web Apps standardmäßig aktiviert.</p></li><li><p><a href="../web-sites-scale/">Skalieren einer Web-app in Azure App Service</a> und <a href="http://www.cleardb.com/developers/cdbr/introduction">ClearDB hohe Verfügbarkeit Routing</a> oder <a href="http://www.mysql.com/products/cluster/">MySQL Cluster CGE</a></p></li></ul>
**Mit Blobs zum Speichern** | <ol><li><p><a href="../storage-create-storage-account/">Ein Azure Storage-Konto erstellen</a></p></li><li><p>Erfahren Sie, wie mit <a href="../cdn-how-to-use/">Content Distribution Network (CDN)</a> Geo-Daten Blobs verteilen.</p></li><li><p>Installieren Sie und konfigurieren Sie der <a href="https://wordpress.org/plugins/windows-azure-storage/">Azure-Speicher für WordPress-Plugin</a>.</p><p>Detaillierte Setup- und Konfigurationsinformationen für das Plug-in finden Sie im <a href="http://plugins.svn.wordpress.org/windows-azure-storage/trunk/UserGuide.docx">Benutzerhandbuch</a>.</p> </li></ol>
**E-Mail aktivieren** | <a href="https://azure.microsoft.com/en-us/marketplace/partners/sendgrid/sendgrid-azure/">SendGrid</a> mit den Azure-Speicher zu aktivieren. <a href="http://wordpress.org/plugins/sendgrid-email-delivery-simplified">SendGrid Plugin</a> für WordPress installieren.
**Konfigurieren Sie einen benutzerdefinierten Domänennamen** | [Konfigurieren Sie einen benutzerdefinierten Domänennamen in Azure App Service][customdomain]
**Für einen benutzerdefinierten Domänennamen HTTPS aktivieren** | [Aktivieren Sie HTTPS für eine Webanwendung in Azure App Service][httpscustomdomain]
**Laden Saldo oder Geo-Verteilung Ihrer Site** | [Datenverkehr mit Azure Traffic Manager][trafficmanager]. Wenn Sie eine benutzerdefinierte Domäne verwenden, finden Sie unter [Konfigurieren einer benutzerdefinierten Domänennamen in Azure App Service] [ customdomain] Informationen zu benutzerdefinierten Domänennamen mit Traffic Manager
**Automatische Sicherung aktivieren** | [Sichern Sie eine Webanwendung in Azure App Service][backup]
**Aktivieren des Diagnoseprotokolls** | [Aktivieren des Diagnoseprotokolls für Web-apps in Azure App Service][log]

## <a name="next-steps"></a>Nächste Schritte

* [WordPress-Optimierung](http://codex.wordpress.org/WordPress_Optimization)

* [Mehrere Standorte in Azure App Service WordPress konvertieren](web-sites-php-convert-wordpress-multisite.md)

* [ClearDB upgrade-Assistenten für Azure](http://www.cleardb.com/store/azure/upgrade)

* [WordPress-Hosting in einem Unterordner Ihrer Anwendung in Azure App Service](http://blogs.msdn.com/b/webapps/archive/2013/02/13/hosting-wordpress-in-a-subfolder-of-your-windows-azure-web-site.aspx)

* [Schritt für Schritt: Erstellen einer WordPress-Website mithilfe von Azure](http://blogs.technet.com/b/blainbar/archive/2013/08/07/article-create-a-wordpress-site-using-windows-azure-read-on.aspx)

* [Hosten Sie Ihre vorhandenen WordPress Blog auf Azure](http://blogs.msdn.com/b/msgulfcommunity/archive/2013/08/26/migrating-a-self-hosted-wordpress-blog-to-windows-azure.aspx)

* [Ziemlich Permalinks in WordPress aktivieren](http://www.iis.net/learn/extensions/url-rewrite-module/enabling-pretty-permalinks-in-wordpress)

* [Migrieren und Ihrem Blog WordPress Azure App-Dienst ausführen](http://www.kefalidis.me/2012/06/how-to-migrate-and-run-your-wordpress-blog-on-windows-azure-websites/)

* [Ausführen von WordPress auf Azure App Service kostenlos](http://architects.dzone.com/articles/how-run-wordpress-azure)

* [WordPress Azure in 2 Minuten oder weniger](http://www.sitepoint.com/wordpress-windows-azure-2-minutes-less/)

* [WordPress-Blog nach Azure - Teil 1: Erstellen eines Blogs WordPress Azure](http://www.davebost.com/2013/07/10/moving-a-wordpress-blog-to-windows-azure-part-1)

* [Verschieben WordPress Blog in Azure - Teil2: übertragen Ihre Inhalte](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-transferring-your-content)

* [Verschieben von WordPress Blog in Azure - Teil 3: Einrichten Ihrer benutzerdefinierten Domäne](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-3-setting-up-your-custom-domain)

* [Azure - Teil 4 WordPress-Blog nach: ziemlich Permalinks und URL Rewrite Regeln](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-4-pretty-permalinks-and-url-rewrite-rules)

* [Azure - Teil 5 WordPress-Blog nach: auf den Stamm aus einem Unterordner verschieben](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-5-moving-from-a-subfolder-to-the-root)

* [Ein WordPress Web app in Azure-Konto einrichten](http://www.itexperience.net/2014/01/20/how-to-set-up-a-wordpress-website-in-your-windows-azure-account/)

* [WordPress Azure Stützung](http://www.johnpapa.net/wordpress-on-azure/)

* [Tipps für WordPress Azure](http://www.johnpapa.net/azurecleardbmysql/)

>[AZURE.NOTE] Wenn Sie mit Azure App Service beginnen, bevor Sie sich für ein Azure-Konto, gehen Sie [Versuchen App Service](http://go.microsoft.com/fwlink/?LinkId=523751)sofort eine kurzlebige Starter Web app in App Service können Sie erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="whats-changed"></a>Was hat sich geändert
* Eine Anleitung zur Änderung von Websites zu App Service finden Sie unter: [Azure App Service und seine Auswirkung auf vorhandene Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- URL List -->

[performance-diagram]: ./media/web-sites-php-enterprise-wordpress/performance-diagram.png
[basic-diagram]: ./media/web-sites-php-enterprise-wordpress/basic-diagram.png
[multi-region-diagram]: ./media/web-sites-php-enterprise-wordpress/multi-region-diagram.png
[wordpress]: http://www.microsoft.com/web/wordpress
[officeblog]: http://blogs.office.com/
[bingblog]: http://blogs.bing.com/
[cdbnstore]: http://www.cleardb.com/store/azure
[storageplugin]: https://wordpress.org/plugins/windows-azure-storage/
[sendgridplugin]: http://wordpress.org/plugins/sendgrid-email-delivery-simplified/
[phpwebsite]: web-sites-php-configure.md
[customdomain]: web-sites-custom-domain-name.md
[trafficmanager]: ../traffic-manager/traffic-manager-overview.md
[backup]: web-sites-backup.md
[restore]: web-sites-restore.md
[rediscache]: https://azure.microsoft.com/documentation/services/redis-cache/
[managedcache]: http://msdn.microsoft.com/library/azure/dn386122.aspx
[websitescale]: web-sites-scale.md
[managedcachescale]: http://msdn.microsoft.com/library/azure/dn386113.aspx
[cleardbscale]: http://www.cleardb.com/developers/cdbr/introduction
[staging]: web-sites-staged-publishing.md
[monitor]: web-sites-monitor.md
[log]: web-sites-enable-diagnostic-log.md
[httpscustomdomain]: web-sites-configure-ssl-certificate.md
[mysqlwindows]: ../virtual-machines/virtual-machines-windows-classic-mysql-2008r2.md
[mysqllinux]: ../virtual-machines/virtual-machines-linux-classic-mysql-on-opensuse.md
[cge]: http://www.mysql.com/products/cluster/
[websitepricing]: /pricing/details/app-service/
[export]: http://en.support.wordpress.com/export/
[import]: http://wordpress.org/plugins/wordpress-importer/
[wordpressbackup]: http://wordpress.org/plugins/wordpress-importer/
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[createwordpress]: web-sites-php-web-site-gallery.md
[velvet]: https://wordpress.org/plugins/velvet-blues-update-urls/
[mgmtportal]: https://portal.azure.com/
[wordpressbackup]: http://codex.wordpress.org/WordPress_Backups
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[workbench]: http://www.mysql.com/products/workbench/
[searchandreplace]: http://interconnectit.com/124/search-and-replace-for-wordpress-databases/
[deploy]: web-sites-deploy.md
[posh]: ../powershell-install-configure.md
[Azure CLI]: ../xplat-cli-install.md
[storesendgrid]: https://azure.microsoft.com/marketplace/partners/sendgrid/sendgrid-azure/
[cdn]: ../cdn/cdn-overview.md
