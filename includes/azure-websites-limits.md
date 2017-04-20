Ressource|Frei|Freigegebene (Vorschau)|Grundlegende|Standard|Premium (Vorschau)</th>
---|---|---|---|---|---
[Web, Mobil oder API-apps](https://azure.microsoft.com/services/app-service/) pro [App Service-Plan](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)<sup>1</sup>|10|100|Unbegrenzte<sup>2</sup>|Unbegrenzte<sup>2</sup>|Unbegrenzte<sup>2</sup>
[Logik apps](https://azure.microsoft.com/services/app-service/logic/) pro [App Service-Plan](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)</a><sup>1</sup>|10|10|10|20 pro Kern|20 pro Kern
[App Service-plan](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)|1 pro region|10 pro Ressourcengruppe|100 pro Ressourcengruppe|100 pro Ressourcengruppe|100 pro Ressourcengruppe
Instanztyp berechnen|Freigegeben|Freigegeben|Dedizierte<sup>3</sup>|Dedizierte<sup>3</sup>|Dedizierte<sup>3</sup></p>
[Dezentrales Skalieren](../articles/app-service-web/web-sites-scale.md) (max. Instanzen)|1 gemeinsam genutzt|1 gemeinsam genutzt|3 dedizierte<sup>3</sup>|10 dedizierte<sup>3</sup>|20 vorgesehen (50 ASE)<sup>3,4</sup>
Speicher-<sup>5</sup>|1 GB<sup>5</sup>|1 GB<sup>5</sup>|10 GB<sup>5</sup>|50 GB<sup>5</sup>|500 GB<sup>4,5</sup></p>
CPU-Zeit (short)<sup>6</sup>|3 Minuten|3 Minuten|Unbegrenzt, standard- [Raten](https://azure.microsoft.com/pricing/details/app-service/) bezahlen</a>|Unbegrenzt Standardsätze bezahlen|Unbegrenzt Standardsätze bezahlen
CPU-Zeit (Tage)<sup>6</sup>|60 Minuten|240 Minuten|Unbegrenzt, standard- [Raten](https://azure.microsoft.com/pricing/details/app-service/) bezahlen</a>|Unbegrenzt Standardsätze bezahlen|Unbegrenzt Standardsätze bezahlen
Speicher (1 Stunde)|1024 MB pro App Service-plan|1024 MB pro Anwendung|N/A|N/A|N/A
Bandbreite|165 MB|Unbegrenzt, [Datenübertragungsraten](https://azure.microsoft.com/pricing/details/data-transfers/) anwenden|Unbegrenzte Datenübertragung gelten|Unbegrenzte Datenübertragung gelten|Unbegrenzte Datenübertragung gelten
Anwendungsarchitektur|32-bit|32-bit|32-Bit/64-bit|32-Bit/64-bit|32-Bit/64-bit
Web Sockets pro Instanz<sup>7</sup>|5|35|350|Unbegrenzt|Unbegrenzt
Aktiven [Debugger Verbindungen](../articles/app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md) pro Anwendung|1|1|1|5|5
[*.azurewebsites.NET Subdomäne FTP-S mit SSL](../articles/app-service-web/web-sites-configure-ssl-certificate.md)|X|X|X|X|X
[Benutzerdefinierte Domain](../articles/app-service-web/web-sites-custom-domain-name.md) -Unterstützung||X|X|X|X
Benutzerdefinierte Domäne [SSL-Unterstützung](../articles/app-service-web/web-sites-configure-ssl-certificate.md)|||Unbegrenzt|Unbegrenzt, 5 SNI SSL und 1 IP SSL Verbindungen|Unbegrenzt, 5 SNI SSL und 1 IP SSL Verbindungen
Integrierte Lastenausgleich||X|X|X|X
[Dauerbetrieb](../articles/app-service-web/web-sites-configure.md)|||X|X|X
[Geplante Backups](../articles/app-service-web/web-sites-backup.md)||||Einmal pro Tag|Einmal alle 5 Minuten<sup>8</sup>
[Automatische Skalierung](../articles/app-service-web/web-sites-scale.md)|||X|X|X
[Webaufträge](../articles/app-service-web/web-sites-create-web-jobs.md) <sup>9</sup>|X|X|X|X|X
[Azure Scheduler](https://azure.microsoft.com/services/scheduler/) -Unterstützung||X|X|X|X
[Endpunkt überwachen](../articles/app-service-web/web-sites-monitor.md)|||X|X|X
[Staging-Steckplätze (Vorschau)](../articles/app-service-web/web-sites-staged-publishing.md)||||5|20
Benutzerdefinierte Domänen pro Anwendung</a>||500|500|500|500
SLA||<p>|99,9 %|99,95 %<sup>10</sup>|99,95 %<sup>10</sup>

<sup>1</sup> Apps und Speicherkontingente sind pro App Service-Plan, sofern nicht anders angegeben.  
<sup>2</sup> Die tatsächliche Anzahl von apps, die auf diesen Computern hosten können hängt die Aktivität der apps, die Größe der Maschine Instanzen und die entsprechenden Ressourcenverwendung.  
<sup>3</sup> Bestimmte Instanzen können unterschiedlich groß sein. Weitere Informationen finden Sie in der [Anwendung die Servicepreise](https://azure.microsoft.com/pricing/details/data-transfers/pricing/details/app-service/) .  
<sup>4</sup> Premium-Ebene kann bis zu 50 Instanzen (Verfügbarkeit) und 500 GB Speicherplatz berechnet, Verwendung von App Service-Umgebungen und 20 berechnen andernfalls Instanzen und 250 GB Speicher.  
<sup>5</sup> Die Speichergrenze ist Content Gesamtgröße über alle apps im gleichen App Service-Plan. Weitere Speicheroptionen stehen in [App Service-Umgebung](../articles/app-service-web/app-service-web-configure-an-app-service-environment.md#storage)  
<sup>6</sup> Diese Ressourcen werden vom physischen Ressourcen auf dedizierten Instanzen (die Größe und die Anzahl der Instanzen) eingeschränkt.  
<sup>7</sup> Skalieren eine Anwendung grundlegende Ebene zwei Instanzen haben Sie 350 aktiven Verbindungen für jede der beiden Instanzen.  
<sup>8</sup> Premium-Ebene kann Sicherungsintervalle nach unten bis zu 5 Minuten App Service-Umgebungen mit und 50 Mal täglich anders.  
<sup>9</sup> Benutzerdefinierte ausführbare Dateien oder Skripts bei Bedarf nach einem Zeitplan oder ständig im Hintergrund in Ihrer App Service-Instanz ausführen. Ständig ist für kontinuierliche Webaufträge Ausführung erforderlich. Azure Scheduler frei oder Standard ist für geplante Webaufträge erforderlich. Gibt es keine vordefinierte Einschränkung für die Anzahl der WebJobs in einer App Service-Instanz ausgeführt werden kann, aber es gibt praktische Grenzwerte hängen, was der Code versucht wird.   
<sup>10</sup> 99,95 % vorgesehenen Installationen, die mehrere Instanzen mit Azure Traffic Manager verwenden SLA konfiguriert für Failover.  
