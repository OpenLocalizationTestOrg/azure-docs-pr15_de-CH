<properties 
    pageTitle="Funktionalität des Betriebssystems auf Azure App Service" 
    description="Erfahren Sie mehr über die OS Funktionalität webapps mobile-app-Back-Ends und API-apps in Azure App Service" 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="mollybos"/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/01/2016" 
    ms.author="cephalin"/>

# <a name="operating-system-functionality-on-azure-app-service"></a>Funktionalität des Betriebssystems auf Azure App Service #

Dieser Artikel beschreibt die allgemeine Betriebssystem Grundfunktionen, die alle apps auf [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)verfügbar ist. Hierzu zählen Datei-, Netzwerk-, und Zugriff auf die Registrierung und Diagnoseprotokolle und Ereignisse. 

<a id="tiers"></a>
## <a name="app-service-plan-tiers"></a>App Dienstebenen plan

Anwendung wird Kunden apps in einer Multi-Tenant-hosting-Umgebung. Apps in Ebenen **frei** und **Shared** bereitgestellt führen in Arbeitsprozesse für freigegebene virtuelle Computer apps im **Standard-** und **Premium** -Ebenen bereitgestellt auf virtuellen speziell für einen einzelnen Debitor zugeordnete apps ausgeführt.

Da App Service Skalierung problemlos zwischen verschiedenen unterstützt, bleibt die Sicherheitskonfiguration für App Service apps erzwungen. Dies gewährleistet, dass apps auf unerwartete Weise fehlschlagen App Service-Plan aus einer Ebene in eine andere wechselt plötzlich anders Verhalten nicht.

<a id="developmentframeworks"></a>
## <a name="development-frameworks"></a>Entwicklungs-frameworks

App Service-Tarifen steuern die Serverressourcen (CPU, Speicher, Speicher und Netzwerk-Ausgang) apps verfügbar. Jedoch Breite Frameworkfunktionen apps zur Skalierung Ebenen gleich.

App-Dienst unterstützt eine Vielzahl von Entwicklungsframeworks, wie ASP.NET klassischen ASP, node.js PHP und Python - alle Erweiterungen in IIS ausgeführt. Vereinfachen und standardisieren Sicherheitskonfiguration ausführen App Service apps in der Regel verschiedene Entwicklungsframeworks mit Standardeinstellungen. Ein Verfahren zum Konfigurieren von apps hätte API-Oberfläche und Funktionalität für jede einzelne Entwicklungsframework anpassen. App Service wird stattdessen einen allgemeineren Ansatz eine gemeinsame Grundlinie Betriebssystem Funktionen unabhängig von einer Anwendung Entwicklungsframework aktivieren.

In den folgenden Abschnitten werden allgemeinen Arten von Betriebssystem-Funktionalität App Service apps zusammengefasst.

<a id="FileAccess"></a>
##<a name="file-access"></a>Dateizugriff

Verschiedene Laufwerke sind in App Service, einschließlich lokaler Laufwerke und Netzwerklaufwerke.

<a id="LocalDrives"></a>
### <a name="local-drives"></a>Lokale Laufwerke

Im Kern ist App ein Dienst auf die Infrastruktur Azure PaaS (Plattform als Dienst) ausgeführt. Dies hat zur Folge, dass die lokalen Laufwerke "zu einer virtuellen Maschine zugeordnet sind" Laufwerk gleichartige für alle Worker-Rolle in Azure ausgeführt. Dazu gehören ein Betriebssystem-Laufwerk (Laufwerk D:\), eine Anwendung Laufwerk mit Azure Cspkg Dateien ausschließlich von App Service (und für Kunden) und "Benutzer" fahren (das Laufwerk C:\), deren Größe je nach Größe der VM.

<a id="NetworkDrives"></a>
### <a name="network-drives-aka-unc-shares"></a>Netzlaufwerke (aka UNC Aktien)

Besonderheiten des App-Service, Bereitstellung und Wartung einfach macht, gehört, dass alle Benutzerinhalt auf UNC-Freigaben gespeichert wird. Dieses Modell wird gut allgemeine Muster der Speicherung von lokalen Web-hosting-Umgebungen mit mehreren Servern verwendet. 

App-Dienst gibt der UNC-Freigaben in jedem Data Center erstellt. Prozentsatz der Benutzerinhalte für alle Kunden in jedem Rechenzentrum reserviert für jeden UNC-Freigabe. Darüber hinaus haben alle Inhalt für ein einzelnes Kundenabonnement immer denselben UNC befindet. 

Beachten Sie, dass durch wie von Cloud-Dienste, der bestimmte Host UNC-Freigabe für virtuelle Computer mit der Zeit ändern. Es wird sichergestellt, dass UNC-Freigaben von anderen virtuellen Computern bereitgestellt werden, wie sie nach oben und nach unten im normalen Cloud-Betrieb sind. Aus diesem Grund sollten apps niemals hartcodierte Annahmen, dass die Informationen in einem Dateipfad UNC langfristig stabil. Sie verwenden stattdessen die *faux* absoluten Pfad **D:\home\site** App Service bietet. Dieser faux absolute Pfad bietet eine portable app und-Benutzer-agnostischen Methode auf eigene app. Mithilfe von **D:\home\site**kann man Dateien app app übertragen, ohne einen neuen absoluten Pfad für jede konfigurieren.

<a id="TypesOfFileAccess"></a>
### <a name="types-of-file-access-granted-to-an-app"></a>Zugriffsarten gewährt eine Anwendung

Abonnement des Kunden hat eine reservierte Verzeichnisstruktur auf einer bestimmten UNC-Freigabe in einem Rechenzentrum. Ein Kunde kann mehrere apps in einem bestimmten Data Center erstellt, sodass alle Kundenabonnement für Verzeichnisse auf derselben UNC erstellt gemeinsam haben. Die Freigabe gehören Verzeichnisse wie Inhalt, Fehler und Diagnoseprotokolle und früheren Versionen vom Datenquellen-Steuerelement erstellt. Wie erwartet, stehen Kunden app Verzeichnisse für Lese- und Schreibzugriff zur Laufzeit der Anwendung Anwendungscode.

Auf die lokalen Laufwerke der virtuellen Computer, der eine Anwendung ausgeführt wird, reserviert App Service Teil Speicherplatz auf Laufwerk C:\ für anwendungsspezifische temporären lokalen Speicher. Zwar eine Anwendung vollständigen Lese-/Schreibzugriff auf einen eigenen temporären lokalen Speicher, soll diesen Speicher nicht wirklich direkt von Anwendungscode verwendet werden. Stattdessen soll die Absicht Speicherung temporärer Dateien für IIS und Web Anwendungsframeworks. App Service beschränkt auch den lokalen Zwischenspeicher für jede Anwendung individuelle apps verhindern viel lokalen Dateispeicher.

Zwei Beispiele wie App-Dienst lokalen Zwischenspeicher verwendet das Verzeichnis für temporäre Dateien von ASP.NET sind und das Verzeichnis für IIS komprimierte Dateien. Das Kompilierungssystem von ASP.NET verwendet das Verzeichnis "Temporary ASP.NET Files" als Speicherort eine temporäre Sammlung. IIS verwendet das Verzeichnis "IIS Temporary Compressed Files" zum Speichern von komprimierten Antwort. Beide Typen der Datei Verwendung (wie auch andere) sind in App Service temporären lokalen Speicher pro Anwendung zugeordnet. Das Neuzuordnen wird sichergestellt, dass Funktionen weiterhin wie erwartet.

Jede Anwendung in App Service als eine zufällige eindeutige geringen Arbeitsprozessidentität namens "Anwendungspoolidentität", beschriebenen weiter ausgeführt: [http://www.iis.net/learn/manage/configuring-security/application-pool-identities](http://www.iis.net/learn/manage/configuring-security/application-pool-identities). Anwendungscode verwendet diese Identität für grundlegende Lesezugriff auf dem Betriebssystem-Laufwerk (Laufwerk D:\). Dies bedeutet Anwendungscode Liste Allgemeine Verzeichnisstrukturen und gemeinsame Dateien auf Laufwerk des Betriebssystems lesen kann. Obwohl dies etwas breiter Ebene von Access werden die gleichen Verzeichnisse und Dateien stehen erscheint bei der Bereitstellung einer Arbeitskraft ein Azure gehosteten Dienst und Laufwerk lesen. 

<a name="multipleinstances"></a>
### <a name="file-access-across-multiple-instances"></a>Dateizugriff über mehrere Instanzen

Das Basisverzeichnis Inhalt einer Anwendung und Anwendungscode schreiben kann. Wenn mehrere Instanzen eine Anwendung ausgeführt wird, ist das Basisverzeichnis für alle Instanzen freigegeben, so dass alle Instanzen dasselbe Verzeichnis angezeigt. Also eine Anwendung im Stammverzeichnis Dateien speichert, sind beispielsweise Dateien sofort auf alle Instanzen. 

<a id="NetworkAccess"></a>
## <a name="network-access"></a>Netzwerkzugriff
Anwendungscode kann TCP und UDP-basierte Protokolle zu ausgehenden Netzwerk-Verbindungen zum Internet zugänglich Endpunkte, die externe Diensten verfügbar machen. Apps können diese dieselben Protokolle mit Diensten in Azure & #151; z. B. Herstellen einer HTTPS-Verbindung zu SQL-Datenbank herstellen.

Gibt es eine eingeschränkte Funktionalität Apps eine lokale Loopback-Verbindung herstellen und haben eine app, lokale Loopback-Socket überwacht. Diese Funktion ist hauptsächlich um apps aktivieren, die als Teil ihrer Funktionalität lokale Loopback-Sockets Abfragen. Beachten Sie, dass jede Anwendung "Privat" Loopback-Verbindung erkennt. App "A" kann nicht vom app "B" lokale Loopback Socket abhören.

Named Pipes werden auch als ein Mechanismus der prozessübergreifenden Kommunikation (IPC) zwischen verschiedenen Prozessen, die zusammen eine Anwendung ausgeführt. Beispielsweise beruht IIS FastCGI-Modul auf named Pipes einzelne Prozesse koordiniert, die PHP-Seiten ausgeführt.


<a id="Code"></a>
## <a name="code-execution-processes-and-memory"></a>Ausführung von Code, Prozesse und Speicher
Wie bereits erwähnt, ausführen apps in geringen Arbeitsprozesse mit zufälligen Anwendungspoolidentität. Anwendungscode greift zum Speicherplatz der Arbeitsprozess als auch untergeordnete Prozesse, die erzeugt möglicherweise CGI-Prozesse oder andere Programme zugeordnet. Allerdings kann eine app Speicher oder Daten von einer anderen Anwendung zugreifen auf demselben virtuellen Computer ist.

Apps können Skripts oder Seiten mit unterstützten Entwicklung ausführen. App Service konfigurieren keine Web Framework eingeschränkteren Modi. Z. B. ASP.NET apps unter App Service "full" Vertrauenswürdigkeit mehr eingeschränkt vertrauenswürdigen Modus ausgeführt. Web-Frameworks, wie ASP und ASP.NET, erreichen prozessinternen COM-Komponenten (aber nicht COM-Komponenten) wie ADO (ActiveX Data Objects), die auf das Betriebssystem Windows registriert sind.

Apps können erzeugen und beliebigen Code ausführen. Es ist für eine Anwendung z. B. Spawn eine Befehls-Shell oder Ausführen eines PowerShell-Skripts. Aber Obwohl willkürlichen Code und Prozesse von einer Anwendung erzeugt werden, sind ausführbare Programme und Skripts noch auf Berechtigungen von der übergeordneten Anwendungspool. Beispielsweise kann eine Anwendung erzeugen eine ausführbare Datei, die einen ausgehenden HTTP-Aufruf, aber derselben ausführbaren Datei versuchen kann nicht zum Auflösen der IP-adressedes eines virtuellen Computers von der NIC. Ein Netzwerk Ausgehender Anruf gering privilegierten Code darf jedoch Network Settings auf einem virtuellen Computer konfigurieren möchten, sind Administratorrechte erforderlich.


<a id="Diagnostics"></a>
## <a name="diagnostics-logs-and-events"></a>Diagnoseprotokolle und Ereignisse
Protokollinformationen sind andere Daten einige apps zuzugreifen versucht. Der Protokollinformationen in App Service ausgeführten Code zur Diagnose umfasst und Informationen von einer app auch leicht in die Anwendung generiert. 

Beispielsweise stehen von einer aktiven Anwendung generierten W3C HTTP-Protokolle auf ein Verzeichnis in der Netzwerkfreigabe für die app oder BLOB-Speicher für erstellt, wenn ein Kunde W3C-Protokollierung Speicher eingerichtet. Die letztgenannte Option kann große Mengen von Protokollen ohne überschreitet das Speicherlimit der zugeordneten Netzwerk gesammelt werden.

In ähnlicher Weise in Echtzeit Diagnoseinformationen aus .NET apps auch protokolliert werden kann Optionen die Ablaufverfolgungsinformationen in die app Netzwerkfreigabe schreiben mit .NET Verfolgung und Diagnoseinfrastruktur oder eine Blob-Speicherort.

Diagnose Protokollierung und Verfolgung, die Apps sind Windows-ETW-Ereignisse und allgemeine Windows-Ereignisprotokolle (z. B. System, Anwendung und Sicherheit Ereignisprotokolle). Da ETW-Ablaufverfolgungsinformationen sichtbaren computerweiten (mit ACLs) möglicherweise kann, werden Lese- und Schreibzugriff auf ETW-Ereignisse blockiert. Entwickler werden feststellen, dass API-zum Lesen und Schreiben-ETW-Ereignisse und allgemeine Windows-Ereignisprotokolle zu arbeiten, aber denn App Service "Aufrufe fälschen ist" Aufrufe, damit sie erfolgreich zu sein scheint. In Wirklichkeit hat Anwendungscode keinen Zugriff auf diese Daten.

<a id="RegistryAccess"></a>
## <a name="registry-access"></a>Zugriff auf die Registrierung
Apps haben schreibgeschützten Zugriff auf viele (aber nicht alle) der Registrierung des virtuellen Computers auf ausführen. In der Praxis bedeutet dies, dass die Registrierungsschlüssel, die nur-Lese-Zugriff auf lokalen Benutzergruppe apps erreichbar sind. Eine Registrierung, die derzeit nicht für Lese- oder Schreibzugriff unterstützt die HKEY ist\_aktuelle\_Benutzerstruktur.

Schreibzugriff auf die Registrierung ist blockiert, einschließlich Zugriff auf Registrierungsschlüssel pro Benutzer. Im Hinblick auf die Anwendung Schreibzugriff auf die Registrierung nicht stillschweigend in der Azure-Umgebung apps können (und) auf verschiedenen virtuellen Computern migriert. Nur dauerhafte beschreibbare Speicher, der von einer Anwendung abhängig sein kann ist-app Inhaltsordner auf App Service UNC gespeichert. 

>[AZURE.NOTE] Wenn Sie mit Azure App Service beginnen, bevor Sie sich für ein Azure-Konto, gehen Sie [Versuchen App Service](http://go.microsoft.com/fwlink/?LinkId=523751)sofort eine kurzlebige Starter Web app in App Service können Sie erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]
 
 
