<properties
    pageTitle="Sichern einer Anwendung in Azure App Service"
    description="Informationen Sie zum Sichern einer WebApp mobile app Back-End- oder API-app in Azure App Service."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="01/12/2016"
    ms.author="cephalin"/>


#<a name="secure-an-app-in-azure-app-service"></a>Sichern einer Anwendung in Azure App Service

Dieser Artikel hilft Ihnen die Sicherung der WebApp mobile app Back-End- oder API-app in Azure App Service beginnen. 

Sicherheit in Azure App Service besteht aus zwei Ebenen: 

- **Infrastruktur und Plattform Sicherheit** - vertrauen Azure um Dienste Dinge sicher in der Cloud ausgeführt werden müssen.
- **Sicherheit** - Sie müssen die Anwendung selbst fest. Dazu gehören wie in Azure Active Directory integrieren, wie Sie Zertifikate verwalten und wie Sie sicherstellen, dass Sie verschiedene Dienste sicher kommunizieren können. 

#### <a name="infrastructure-and-platform-security"></a>Infrastruktur und Plattform-Sicherheit
Da App Service Azure-VMs, Speicher, Netzwerk, Web Frameworks, Management und Integrationsfunktionen und vieles mehr verwaltet ist aktiv gesichert und abgesichert und durch strenge Compliance und überprüft laufend sicherstellen:

- Ihre App Service apps werden aus dem Internet und die anderen Kunden Azure Ressourcen isoliert.
- Kommunikation zwischen Ihrer App Service-app und andere Azure-Ressourcen (z. B. SQL-Datenbank) in einer Ressourcengruppe Geheimnisse (z. B. Verbindungszeichenfolgen) bleibt in Azure und Netzwerkgrenzen nicht überschreiten. Geheime Informationen werden verschlüsselt.
- Die gesamte Kommunikation zwischen Ihren App Service-app und externe Ressourcen wie PowerShell Management, Befehlszeilenschnittstelle Azure SDKs, REST-APIs und hybridverbindungen werden ordnungsgemäß verschlüsselt.
- 24 Stunden Threat Management App Ressourcen schützt vor Malware, distributed Denial of Service (DDoS) Man-in-the-Middle (MITM) und anderen Gefahren. 

Weitere Informationen zur Infrastruktur und Plattform in Azure anzeigen Sie [Azure Trust Center](/support/trust-center/security/)

#### <a name="application-security"></a>Sicherheit

Azure ist verantwortlich für die Infrastruktur und Plattform, die Ihre Anwendung ausgeführt wird, ist es Ihre Aufgabe, die Anwendung selbst. Also müssen Sie entwickeln, bereitstellen und Verwalten von dem Anwendungscode und Inhalt sicher. Ohne diese möglich Anwendungscode oder Inhalt noch Gefahren wie:

- SQL-Injektion
- Sitzungsübernahmen
- Cross-Site scripting
- Anwendungsebene MITM
- Anwendungsebene DDoS

Eine umfassende Erläuterung der Sicherheitsaspekte für Web-basierte Anwendung ist nicht Gegenstand dieses Dokuments. Als Ausgangspunkt für Weitere Hinweise zur Sicherung Ihrer Anwendung finden Sie unter der [Open Web Application Security Project (OWASP)](https://www.owasp.org/index.php/Main_Page), speziell die [Top 10 Projekt.](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project), die Listen die aktuellen Top 10 wichtige Web Anwendung Sicherheitslücken OWASP Mitglieder bestimmt.

## <a name="perform-penetration-testing-on-your-app"></a>Führen Sie Eindringversuchen für Ihre Anwendung aus

Die einfachsten noch mit Sicherheitslücken App Service-app gestartet ist mit [Integration mit Alufolie](/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) Mausklick Schwachstellen-Scans auf Ihre Anwendung ausführen. Sie können die Testergebnisse in einem leicht verständlichen Bericht anzeigen und so jede Sicherheitslücke Schritt beheben.

Penetrationstests ausführen möchten oder einen anderen Scanner Suite oder Anbieter verwenden Sie folgen [Azure Penetration Tests Genehmigungsprozess](https://security-forms.azure.com/penetration-testing/terms) und vorherige Genehmigung die gewünschten Penetrationstests durchführen.

##<a name="https"></a>Sichere Kommunikation mit Kunden

Verwenden Sie die ** \*. *.azurewebsites.NET** für Ihre App Service-app erstellte sofort können Sie HTTPS, wie ein SSL-Zertifikat für alle ** \*. *.azurewebsites.NET** Domänennamen. Wenn Ihre Site einen [benutzerdefinierten Domänennamen](web-sites-custom-domain-name.md)verwendet, können Sie ein SSL-Zertifikat für die benutzerdefinierte Domäne [HTTPS aktivieren](web-sites-configure-ssl-certificate.md) hochladen.

[HTTPS](https://en.wikipedia.org/wiki/HTTPS) aktivieren kann schützen MITM Angriffe auf die Kommunikation zwischen Ihrer Anwendung und Benutzer.

## <a name="secure-data-tier"></a>Sichere Datenebene

App Service integriert sehr SQL-Datenbank alle Verbindungszeichenfolgen werden durchgehend verschlüsselt und entschlüsselt sind nur auf dem virtuellen Computer *und* die Anwendung ausgeführt wird, nur, wenn die Anwendung ausgeführt wird. Azure SQL-Datenbank enthält außerdem viele Sicherheitsfunktionen helfen Cyber Gefahren, einschließlich [in - Verschlüsselung](https://msdn.microsoft.com/library/dn948096.aspx), [Verschlüsselt](https://msdn.microsoft.com/library/mt163865.aspx), [Dynamische Daten maskieren](../sql-database/sql-database-dynamic-data-masking-get-started.md)und [Erkennung](../sql-database/sql-database-threat-detection-get-started.md)von Anwendungsdaten sichern. Haben Sie vertrauliche Daten oder Vorschriften, finden Sie weitere Informationen zum Sichern der Daten [Ihrer SQL-Datenbank sichern](../sql-database/sql-database-security.md) .

Verwenden Sie einen Drittanbieter-Anbieter, wie ClearDB, konsultieren Sie die Dokumentation des Anbieters auf bewährte Sicherheitsmethoden.  

##<a name="develop"></a>Sichere Entwicklung und Bereitstellung

### <a name="publishing-profiles-and-publish-settings"></a>Veröffentlichung Profile und Einstellungen veröffentlichen

Bei der Entwicklung von Applikationen, Ausführen von Verwaltungsaufgaben und Automatisierung von Aufgaben mit Dienstprogrammen wie **Visual Studio**und **Web Matrix**, **Azure PowerShell** **Azure-Befehlszeilenschnittstelle (CLI Azure)**können Sie eine Datei *Veröffentlichen Einstellungen* oder ein *Präsentationsprofil*. Beide Dateitypen mit Azure authentifizieren und sollten zum Schutz vor unberechtigtem Zugriff geschützt werden.

* Eine Datei **Veröffentlichen settings**

    * Ihre Azure-Abonnement-ID

    * Ein Management-Zertifikat, die Verwaltungsaufgaben für Ihr Abonnement *ohne Kontoname oder Kennwort*ermöglicht.

* Ein **Veröffentlichungsprofil** enthält

    * Informationen zum Veröffentlichen Ihrer app

Wenn Sie ein Programm verwenden, eine Datei veröffentlichen oder veröffentlichen Profildatei importieren Sie Datei Veröffentlichungsoptionen oder Profil in das Dienstprogramm und dann **Löschen** . Wenn Sie die Datei gemeinsam mit anderen an dem Projekt z. B. halten müssen speichern Sie es an einem sicheren Ort wie einem *verschlüsselten* Verzeichnis mit eingeschränkten Berechtigungen.

Außerdem sollten Sie sicherstellen, dass die importierten Anmeldeinformationen geschützt sind. Z. B. **Azure PowerShell** und der **Azure-Befehlszeilenschnittstelle (CLI Azure)** speichern importierten Informationen in Ihrem **Basisverzeichnis** (*~* */users/yourusername* auf Windows-Systemen zu Linux oder OS X.) Für zusätzliche Sicherheit können **verschlüsselt** diesen Speicherorten mit Verschlüsselungstools für Ihr Betriebssystem Sie.

### <a name="configuration-settings-and-connection-strings"></a>Konfiguration und Verbindungszeichenfolgen
Es ist üblich, Verbindungszeichenfolgen, Anmeldeinformationen und andere vertrauliche Informationen in Konfigurationsdateien gespeichert. Leider können diese Dateien auf Ihrer Website offen gelegt oder diese Informationen öffentlich-Repository eingecheckt werden. Eine einfache Suche auf [GitHub](https://github.com)kann beispielsweise unzählige Konfigurationsdateien mit öffentlichen Repositories verfügbar gemachten Geheimnisse aufdecken.

Empfiehlt diese Informationen aus Konfigurationsdateien für Ihre Anwendung zu. App Service können Sie die Informationen als Teil der Common Language Runtime-Umgebung **Appeinstellungen** und **Verbindungszeichenfolgen**zu speichern. Die Werte werden der Anwendung zur Laufzeit durch *Umgebungsvariablen* für die meisten Programmiersprachen verfügbar gemacht. Diese Werte werden für .NET Applications in Ihrer Konfiguration .NET Laufzeit eingefügt. Neben diesen Situationen bleibt diese Konfigurationen verschlüsselt, wenn Sie anzeigen oder konfigurieren Sie sie mithilfe der [Azure-Portal](https://portal.azure.com) oder Dienstprogramme der Azure-CLI oder PowerShell. 

Speichern von Konfigurationsinformationen in App Service ermöglicht die app Administrator vertrauliche Informationen für Produktions-apps sperren. Entwickler können einen separaten Satz von Konfigurationsdateien für Anwendungsentwicklung und Einstellungen können werden automatisch durch die Einstellungen in App Service. Auch die Entwickler müssen die Geheimnisse für produktionsanwendung konfiguriert. Weitere Informationen zum Konfigurieren von Anwendungseinstellungen und Verbindungszeichenfolgen in App Service finden Sie unter [Konfigurieren von webapps](web-sites-configure.md).

### <a name="ftps"></a>FTPS

Azure App Service bietet sicheren FTP-Zugriff auf das Dateisystem für Ihre Anwendung über **FTP**. Dadurch können Sie sicheren Zugriff auf den Anwendungscode WebApp sowie Diagnose Protokolle. Es wird empfohlen, immer FTPS statt FTP verwenden. 

FTP-Link für Ihre finden die folgenden Schritte aus:

1. [Azure-Portal](https://portal.azure.com)zu öffnen.
2. **Sie können alle**auswählen
3. Wählen Sie das Blade **Durchsuchen** **Anwendungsdienste**.
4. **App-Services** -Blade wählen Sie die gewünschte app.
5. Die app-Blatt wählen Sie **Alle**.
6. Wählen Sie **Eigenschaften**aus dem Blade **Settings** .
7. FTP und FTPS Links werden auf die **Standardeinstellungen** bereitgestellt. 

Weitere Informationen zu FTP finden Sie unter [File Transfer Protocol](http://en.wikipedia.org/wiki/File_Transfer_Protocol).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über die Sicherheit der Azure-Plattform Informationen zu einem **Sicherheitsvorfall oder**Berichte oder Microsoft darüber informiert, dass Sie Ihrer Site **Penetration Tests** durchführen finden Sie im Abschnitt Security [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/security/).

Weitere Informationen zu App Service apps **web.config** oder **applicationhost.config** Dateien finden Sie unter [Konfigurationsoptionen in Azure App Service webapps entsperrt](https://azure.microsoft.com/blog/2014/01/28/more-to-explore-configuration-options-unlocked-in-windows-azure-web-sites/).

Informationen zum Protokollieren von Informationen für die App Service apps können das Erkennen von Angriffen hilfreich sein, finden Sie unter [Diagnoseprotokoll aktivieren](web-sites-enable-diagnostic-log.md).

>[AZURE.NOTE] Wenn Sie mit Azure App Service beginnen, bevor Sie sich für ein Azure-Konto, gehen Sie [Versuchen App Service](http://go.microsoft.com/fwlink/?LinkId=523751)sofort eine kurzlebige Starter-app in App Service können Sie erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="whats-changed"></a>Was hat sich geändert

* Eine Anleitung zur Änderung von Websites zu App Service finden Sie unter: [Azure App Service und seine Auswirkung auf vorhandene Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)
