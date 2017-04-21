# <a name="compute"></a>Berechnen

Azure ermöglicht das Bereitstellen und Überwachen des Anwendungscodes in einem Microsoft-Rechenzentrum. Wenn Sie eine Anwendung erstellen und auf Azure ausführen, Code und Konfiguration zusammen heißt ein Azure gehosteten Dienst. Gehostete Dienste sind einfach zu verwalten, nach oben und unten skaliert, konfigurieren und Aktualisieren mit neuen Versionen der Anwendung. Dieser Artikel befasst sich gehosteten Azure Service-Anwendungsmodell.<a id="compare" name="compare"></a>

## Inhaltsverzeichnis<a id="_GoBack" name="_GoBack"></a>

-   [Azure Modell Vorteile][]
-   [Grundbegriffe gehosteter Dienst][]
-   [Vorüberlegungen zum gehosteten Dienst][]
-   [Entwerfen Ihre Anwendung für die Skalierung][]
-   [Hosted Servicedefinition und Konfiguration][]
-   [Service-Definitionsdatei][]
-   [Die Dienstkonfigurationsdatei][]
-   [Erstellen und Bereitstellen von gehosteter Dienst][]
-   [Referenzen][]

## <a id="benefits"> </a>Azure Modell Vorteile

Beim Bereitstellen der Anwendung als gehosteten Dienst Azure erstellt ein oder mehrere virtuelle Maschinen (VMs, die die Anwendung Code enthalten) und startet die VMs auf physischen Computern in einem Azure Rechenzentren wohnen. Anfragen an die gehostete Anwendung Rechenzentrum eingeben, verteilt ein Lastenausgleich diese Anfragen gleichmäßig auf die VMs. Während Ihre Anwendung in Azure gehostet wird, wird es drei wichtige Vorteile:

-   **Hohe Verfügbarkeit.** Hoher Verfügbarkeit bedeutet Azure wird sichergestellt, dass die Anwendung so weit wie möglich ausgeführt wird und auf Clientanforderungen reagieren. Wenn Ihre Anwendung beendet (aufgrund einer nicht behandelten Ausnahme, z. B.), und Azure erkannt, es automatisch wird starten die Anwendung neu. Läuft der Computer die Anwendung auf eine Art von Hardwarefehlern, wird Azure auch erkennen und automatisch erstellen einen neuen virtuellen Computer auf einem anderen physischen Maschine und den Code von dort aus auszuführen. Hinweis: Damit die Anwendung Microsoft Service Level Agreement von 99,95 % verfügbar zu mindestens zwei VMs Anwendungscode ausführen müssen Sie. Dadurch wird eine VM zu Clientanforderungen Azure Code von einer fehlgeschlagenen VM neue, gute VM verschoben wird.

-   **Skalierbar.** Azure können Sie leicht und dynamisch ändern der Anzahl der VMs unter dem Anwendungscode die tatsächliche Belastung der Anwendung behandeln. Dadurch werden die Arbeitslast die Anwendung anpassen, die Ihre Kunden Platzieren auf und nur für die virtuellen Computer, wenn Sie sie benötigen. Wenn Sie die Anzahl der VMs ändern möchten, reagiert Azure innerhalb von Minuten zu dynamisch ändern, die Anzahl der VMs beliebig oft ausführen.

-   **Verwaltung.** Da Azure als Service (PaaS) bietet eine Plattform ist, verwaltet die Infrastruktur (Hardware, Strom und Networking) zu diesen Computern. Azure verwaltet außerdem die Plattform, ein aktuelles Betriebssystem mit alle die richtigen Patches und Sicherheitsupdates sowie Updates für Komponenten wie.NET Framework und Internet Information Server. Da die VMs Windows Server 2008 ausgeführt werden, bietet Azure zusätzliche Diagnose Überwachung remote desktop Support, Firewalls und Speicher – Konfigurieren von Zertifikaten. Diese Funktionen dienen ohne zusätzliche Kosten. Beim Ausführen der Anwendung in Azure ist sogar Windows Server 2008-Betriebssystem (OS) Lizenz enthalten. Da alle VMs Windows Server 2008 ausführen, funktioniert Code, der auf Windows Server 2008 nur im Azure.

## <a id="concepts"> </a>Gehosteten Dienst Grundbegriffe

Wenn Ihre Anwendung als gehosteten Dienst in Azure bereitgestellt wird, führt Sie ein oder mehrere *Rollen.* Eine *Rolle* gemeint, Anwendungsdateien und Konfiguration. Sie können eine oder mehrere Rollen für die Anwendung, jedes mit einem eigenen Satz von Dateien und Konfiguration definieren. Die Anzahl der VMs oder *Instanzen*können Sie für jede Rolle in der Anwendung führen. Abbildung zeigen zwei Beispiele einer Anwendung ein gehosteter Dienst mit Rollen und Instanzen modelliert.

##### <a name="figure-1-a-single-role-with-three-instances-vms-running-in-an-azure-data-center"></a>Abbildung 1: Eine einzelne Rolle mit drei Instanzen (VMs) in Azure-Rechenzentrum

![Bild][0]

##### <a name="figure-2-two-roles-each-with-two-instances-vms-running-in-an-azure-data-center"></a>Abbildung 2: Zwei Rollen mit jeweils zwei Instanzen (VMs) im Azure-Rechenzentrum

![Bild][1]

Rolleninstanzen Anfragen werden in der Regel Internet Client Rechenzentrum durch *input Endpunkt sogenannte*. Eine Rolle kann 0 oder mehr input Endpunkte aufweisen. Jeder Endpunkt gibt ein Protokoll (HTTP, HTTPS oder TCP) und einen Port an. Es ist üblich, eine Rolle zu zwei input Endpunkte konfigurieren: HTTP auf Port 80 und HTTPS-Port 443 abhört. Die Abbildung zeigt ein Beispiel zwei Rollen mit anderen Eingabe Endpunkten Clientanforderungen an sie weiterleiten.

![Bild][2]

Beim Erstellen eines gehosteten Diensts in Azure ist es eine öffentlich adressierbare IP-Adresse, die Clients darauf zugreifen können. Beim Erstellen des gehosteten Diensts müssen Sie auch ein URL-Präfix auswählen, die IP-Adresse zugeordnet ist. Dieses Präfix muss als *Präfix*im Grunde reserviert. cloudapp.net URL, so dass es haben kann. Clients kommunizieren mit der Rolleninstanzen mithilfe der URL. In der Regel nicht verteilen oder veröffentlichen Azure *Präfix*. cloudapp.net URL. Stattdessen einen DNS-Namen von Ihrer DNS-Registrierungsstelle Wahl erwerben und konfigurieren Sie die DNS-Clientanforderungen an den Azure-URL umzuleiten. Weitere Informationen finden Sie unter [Konfigurieren einer benutzerdefinierten Domänennamen in Azure][].

## <a id="considerations"> </a>Gehosteten Dienst Vorüberlegungen

Beim Entwerfen einer Anwendung in einer Cloud-Umgebung ausgeführt werden mehrere Aspekte wie Wartezeit, hohe Verfügbarkeit und Skalierung überlegen.

Entscheiden, wo die Anwendungscode ist eine wichtige Überlegung einen gehosteten Dienst in Azure. Es ist üblich, die Anwendung in Rechenzentren bereitstellen, die Ihre Kunden Latenz und optimale Leistung möglich am nächsten.
Allerdings können Sie einen Rechenzentrums näher an Ihr Unternehmen oder Ihren Daten näher Gerichts- oder rechtlichen Bedenken Daten und Speicherort. Es gibt sechs Rechenzentren weltweit Anwendungscode hosten. Die folgende Tabelle zeigt die verfügbaren Standorte:

<table border="2" cellspacing="0" cellpadding="5" style="border: 2px solid #000000;">
<tbody>
<tr>
<td style="width: 100px;">
**Land/Region**

</td>
<td style="width: 200px;">
**Regionen**

</td>
</tr>
<tr>
<td>
USA

</td>
<td>
Süden und Norden

</td>
</tr>
<tr>
<td>
Europa

</td>
<td>
Norden und Westen

</td>
</tr>
<tr>
<td>
Asien

</td>
<td>
Südosten & Osten

</td>
</tr>
</tbody>
</table>
Beim gehosteten Dienst zu erstellen, wählen Sie einen Teilbereich der Speicherort in dem Code ausgeführt werden soll.

Hohe Verfügbarkeit und Skalierung ist es entscheidend, dass die Anwendung die Daten in einem zentralen Repository für mehrere Instanzen aufbewahrt werden. Azure bietet dafür, mehrere Speicheroptionen Blobs und Tabellen mit SQL-Datenbank. Finden Sie Artikel [Speicherangebote Daten in Azure][] für Weitere Informationen über diese Technologien. Die folgende Abbildung zeigt, wie der Lastenausgleich innerhalb der Azure-Rechenzentrum Clientanforderungen an andere Rolleninstanzen verteilt alle auf denselben Datenspeicher zugreifen.

![Bild][3]

Normalerweise soll den Anwendungscode und die Daten im gleichen Rechenzentrum wie dadurch für niedrige Latenz (bessere Leistung), Anwendungscode Daten zugreift. Darüber hinaus sind Sie nicht Bandbreite berechnet Wenn Daten im selben Rechenzentrum verschoben werden.

## <a id="scale"> </a>Entwurf Ihrer Anwendung für Skalierung

Manchmal möchten Sie eine einzelne Anwendung (z. B. eine einfache Website) und in Azure gehostet. Aber häufig Ihre Anwendung möglicherweise mehrere Rollen, die zusammenarbeiten. Zum Beispiel in der folgenden Abbildung werden zwei Instanzen der Rolle der Website drei Instanzen der Rolle der Auftragsabwicklung und eine Instanz der Rolle Bericht-Generator Diese Funktionen arbeiten zusammen und Code für alle zusammen verpackt und als Einheit Azure bereitgestellt werden kann.

![Bild][4]

Der Hauptgrund für eine Anwendung in verschiedenen Rollen mit jeder einen eigenen Satz von Instanzen (d. h. VMs) aufgeteilt werden Rollen unabhängig voneinander zu skalieren. Beispielsweise können während der Urlaubszeit viele Kunden kaufen Produkte aus Ihrem Unternehmen damit die Zahl der Instanzen Ihrer Website Rolle sowie die Anzahl der Instanzen die Auftragsabwicklung Rolle ausgeführt werden soll. Nach Weihnachten erhalten Sie viele Produkte zurückgegeben, müssen Sie dennoch viele Instanzen Website aber weniger Auftragsabwicklung Instanzen können. Während des restlichen Jahres benötigen Sie nur einige Website und Auftragsabwicklung Instanzen. Ganz davon benötigen Sie nur eine Instanz von Berichts-Generator. Die Flexibilität der rollenbasierte Bereitstellung in Azure können Sie die Anwendung an Ihre geschäftlichen Bedürfnisse anpassen.

Es ist häufig die Rolle, die Instanzen der gehosteten Dienst kommunizieren. Z. B. Website-Funktion akzeptiert eine Bestellung aber dann überträgt der Auftragsabwicklung Rolleninstanzen Auftragsabwicklung. Am besten Arbeitsformular übergeben einen Satz von Instanzen auf andere Instanzen mit der Warteschlangen-Technologie von Azure Warteschlangendienst oder Service Bus-Warteschlangen. Die Verwendung einer Warteschlange ist ein wichtiger Bestandteil der Geschichte. Die Warteschlange kann den gehosteten Dienst skaliert seine Funktionen unabhängig ermöglicht die Arbeitslast Kosten auszugleichen. Steigt die Anzahl der Nachrichten in der Warteschlange mit der Zeit können Sie die Anzahl der Rolleninstanzen Auftragsabwicklung skalieren. Verringert die Anzahl der Nachrichten in der Warteschlange mit der Zeit kann die Anzahl der Rolleninstanzen Auftragsabwicklung skaliert werden. So werden Sie Zahlen nur für Instanzen erforderlich, um die tatsächliche Arbeitslast zu bewältigen.

Die Warteschlange enthält auch Zuverlässigkeit. Beim Skalieren die Anzahl der Rolleninstanzen Auftragsabwicklung entscheidet Azure Instanzen beenden. Sie können eine Instanz beendet, die während der Verarbeitung einer warteschlangennachricht eine. Da die Verarbeitung nicht erfolgreich abgeschlossen wird, wird die Nachricht erneut für eine andere Auftragsabwicklung Instanz der Rolle, die sie aufnimmt und verarbeitet. Wegen Warteschlange Nachricht Sichtbarkeit schließlich verarbeitet Nachrichten garantiert. Die Warteschlange fungiert auch als Lastenausgleichsmodul effektiv verteilt die Nachrichten alle Instanzen, die es Nachrichten anfordern.

Für Rolleninstanzen Website können in diesen Datenverkehr überwachen und skaliert die Anzahl entscheiden nach oben oder unten sowie. Die Warteschlange können Sie die Anzahl der Instanzen Website unabhängig von der Auftragsabwicklung Rolleninstanzen skalieren. Dies ist sehr leistungsstark und bietet viel Flexibilität. Natürlich besteht die Anwendung weitere Rollen können Sie hinzufügen Weitere Warteschlangen als Kanal für die Kommunikation zwischen damit nutzen dieselbe Skalierung und Kostenvorteile.

## <a id="defandcfg"> </a>Hosted Servicedefinition und Konfiguration

Bereitstellen von gehosteter Dienst in Azure benötigen Sie auch eine Service-Definitionsdatei und eine Service-Konfigurationsdatei. Beide Dateien sind XML-Dateien, und sie können Bereitstellungsoptionen für Ihr gehosteter Dienst deklarativ angegeben. Service-Definitionsdatei beschreibt alle Rollen, aus denen Ihr gehosteter Dienst und wie sie miteinander kommunizieren. Die Dienstkonfigurationsdatei beschreibt die Anzahl der Instanzen für jede Rolle und jede Instanz der Rolle konfigurieren, verwendet.

## <a id="def"> </a>Service-Definitionsdatei

Wie bereits die Dienstdefinition (CSDEF erwähnt) ist die Datei eine XML-Datei, die die verschiedenen Rollen beschrieben, die die vollständige Anwendung bilden. Das vollständige Schema für die XML-Datei finden Sie hier: [http://msdn.microsoft.com/library/windowsazure/ee758711.aspx] [].
Die Datei CSDEF enthält ein WebRole oder WorkerRole-Element für jede Rolle in Ihrer Anwendung wünschen. Bereitstellen einer Rolle als Webrolle (mit WebRole-Element) bedeutet, dass der Code auf eine Rolleninstanz Windows Server 2008 mit Internet Information Server (IIS) ausgeführt wird.
Bereitstellen einer Rolle eine Worker-Rolle (mit WorkerRole-Element) bedeutet, dass die Instanz der Rolle auf Windows Server 2008 hat (IIS nicht installiert werden).

Sie sicherlich erstellen und Bereitstellen eine Worker-Rolle, die einen anderen Mechanismus verwendet, um eingehende Webanfragen empfangen (z. B. Code erstellen und Verwenden einer .NET HttpListener). Da die Rolleninstanzen allen Windows Server 2008 ausgeführt werden, kann Code Operationen durchführen, die normalerweise zur einer Anwendung auf Windows Server
2008.

Für jede Rolle geben Sie die gewünschte VM-Größe, die Instanzen dieser Rolle verwenden soll. Die folgende Tabelle zeigt die verschiedenen VM heute und die Attribute:

<table border="2" cellspacing="0" cellpadding="5" style="border: 2px solid #000000;">
<tbody>
<tr align="left" valign="top">
<td>
**Virtueller Speicher**

</td>
<td>
**CPU**

</td>
<td>
**RAM**

</td>
<td>
**Datenträger**

</td>
<td>
**Maximale Netzwerk e/a**

</td>
</tr>
<tr align="left" valign="top">
<td>
**Sehr klein**

</td>
<td>
1 x 1,0 GHz

</td>
<td>
768 MB

</td>
<td>
20GB

</td>
<td>
\~5 Mbit/s

</td>
</tr>
<tr align="left" valign="top">
<td>
**Kleine**

</td>
<td>
1 x 1,6 GHz

</td>
<td>
1,75 GB

</td>
<td>
225GB

</td>
<td>
\~100 Mbit/s

</td>
</tr>
<tr align="left" valign="top">
<td>
**Mittel**

</td>
<td>
2 x 1,6 GHz

</td>
<td>
3,5 GB

</td>
<td>
490GB

</td>
<td>
\~200 Mbit/s

</td>
</tr>
<tr align="left" valign="top">
<td>
**Große**

</td>
<td>
4 x 1,6 GHz

</td>
<td>
7 GB

</td>
<td>
1TB

</td>
<td>
\~400 MBit/s

</td>
</tr>
<tr align="left" valign="top">
<td>
**Sehr groß**

</td>
<td>
8 x 1,6 GHz

</td>
<td>
14 GB

</td>
<td>
2TB

</td>
<td>
\~800 Mbit/s

</td>
</tr>
</tbody>
</table>
Sie Zahlen stündlich für jede VM verwenden als eine Rolleninstanz und auch Zahlen für Daten, die Rolleninstanzen außerhalb des Rechenzentrums senden. Sie Zahlen nicht für Daten im Rechenzentrum. Weitere Informationen finden Sie unter [Azure Preis] []. Im Allgemeinen empfiehlt es sich viele kleine Instanzen im Gegensatz zu einigen großen Instanzen verwenden, damit die Anwendung weniger anfällig für Fehler ist. Nachdem alle weniger Rolleninstanzen haben, verheerender ein Fehler in einem der gesamten Anwendung. Außerdem müssen wie oben erwähnt bereitstellen mindestens zwei Instanzen für jede Rolle erzielen 99,95 % Servicelevel Microsoft bereitstellt.

Service-Definitionsdatei (CSDEF) wird auch Sie viele Attribute jeder Rolle in der Anwendung geben an. Hier sind einige weitere nützliche Elemente zur Verfügung:

-   **Zertifikate**. Verwenden Sie Zertifikate zum Verschlüsseln von Daten oder der Webdienst SSL unterstützt. Alle Zertifikate müssen in Azure hochgeladen werden. Weitere Informationen finden Sie unter [Verwalten von Zertifikaten in Azure][]. Diese XML-Einstellung installiert zuvor hochgeladene Zertifikate die Rolleninstanz Zertifikatspeicher, damit sie im Anwendungscode verwendet werden.

-   **Konfigurationsnamen festlegen**. Für Werte Ihre Anwendung(en) zu lesen, während auf eine Rolleninstanz. Der tatsächliche Wert der Konfigurationsinformationen wird im Dienstkonfigurationsdatei (mithilfe) jederzeit aktualisiert werden können, ohne den Code erneut festlegen. Tatsächlich können Sie Ihre Anwendung so erkennen die geänderten Werte ohne Ausfallzeiten schreiben.

-   **Endpunkte geben**. Hier geben Sie alle Endpunkte HTTP, HTTPS oder TCP (mit Ports), die Sie mit der Außenwelt über Ihre *Präfix*verfügbar machen möchten. cloadapp.net URL. Bei Azure Ihre Rolle bereitgestellt werden, wird automatisch die Firewall auf die Instanz der Rolle konfigurieren werden.

-   **Interne Endpunkte**. Geben Sie hier beliebigen HTTP oder TCP-Endpunkten die gewünschte ausgesetzt andere Rolleninstanzen, die als Teil der Anwendung bereitgestellt werden. Interne Endpunkte die Rolleninstanzen in Ihrer Anwendung miteinander kommunizieren lassen jedoch nicht für alle Instanzen, die außerhalb der Anwendung.

-   **Module importieren**. Optional installieren diese Komponenten auf der Rolleninstanzen. Komponenten für diagnostische Überwachung, Remotedesktop und Azure Connect (wodurch die Rolleninstanz lokalen Zugriff auf Ressourcen über einen sicheren Kanal) vorhanden sind.

-   **Lokalen Speicher**. Dies weist ein Unterverzeichnis auf die Instanz der Rolle für die Anwendung. Es ist im [Daten-Storage-Angebote in Azure][] Artikel ausführlicher beschrieben.

-   **Aufgaben**. Aufgaben können Sie Komponenten auf eine Rolleninstanz installieren, da es startet. Die Aufgaben können als Administrator bei Bedarf ausführen.

## <a id="cfg"> </a>Service-Konfigurationsdatei

Service-Konfigurationsdatei (mithilfe) ist eine XML-Datei, die Einstellungen beschrieben, die ohne erneutes Bereitstellen der Anwendung geändert werden können. Das vollständige Schema für die XML-Datei finden Sie hier: [http://msdn.microsoft.com/library/windowsazure/ee758710.aspx][].
Mithilfe Datei enthält ein Element Rolle für jede Rolle in der Anwendung. Hier sind einige der Elemente in der Datei mithilfe angeben:

-   **Version des Betriebssystems**. Dieses Attribut ermöglicht die Version des Betriebssystems (OS) gewünschte verwendet für alle Rolleninstanzen Anwendungscode ausführen wählen. Dieses Betriebssystem als *Gastbetriebssystem*bezeichnet, und jede neue Version enthält die neuesten Sicherheitspatches und Updates Zeitpunkt das Betriebssystem des Gastes freigegeben wird. Wenn Sie den Attributwert OsVersion auf "\*", Azure das Gastbetriebssystem auf der Rolleninstanzen automatisch aktualisiert, sobald neue Gast-BS-Versionen verfügbar sind. Allerdings können Sie automatische Updates entscheiden, bestimmten Gast Betriebssystemversion auswählen. OsVersion-Attribut beispielsweise auf den Wert festlegen "WA Gast-Betriebssystem 2.8\_201109-01" führt alle Rolleninstanzen zu beschrieben auf dieser Webseite: [http://msdn.microsoft.com/library/hh560567.aspx][]. Weitere Informationen zu Gast-BS-Versionen finden Sie unter [Verwalten von Upgrades Azure Gäste Betriebssystem].

-   **Instanzen**. Wert dieses Elements gibt die Anzahl der Instanzen, bereitgestellten Code für eine bestimmte Funktion ausführen soll. Da eine neue Datei mithilfe in Azure hochladen können (ohne erneutes Bereitstellen der Anwendung), ist es ganz einfach ändern Sie den Wert für dieses Element und Hochladen einer neuen mithilfe dynamisch erhöhen oder verringern die Anzahl der Instanzen den Anwendungscode ausgeführt. Dadurch werden die Anwendung problemlos skalieren, oben oder unten entsprechen tatsächlichen Leistungsdatenanalyse während auch steuern, wie viel Sie für die Ausführung der Instanzen berechnet.

-   **Werte festlegen**. Dieses Element gibt Werte für die Einstellung (definiert in der Datei CSDEF). Ihre Rolle kann diese Werte lesen, während er ausgeführt wird. Diese Konfigurationseinstellungswerte werden normalerweise für Verbindungszeichenfolgen Azure Storage oder SQL-Datenbank verwendet, aber für jeden gewünschten Zweck verwendet werden.

## <a id="hostedservices"> </a>Erstellen und Bereitstellen von gehosteter Dienst

Erstellen eines gehosteten Diensts muss zum [Azure-Verwaltungsportal] und einen gehosteten Dienst durch Angabe eines Präfix DNS bereitzustellen und Rechenzentrum letztendlich im Code soll. Umgebung, in der (CSDEF) Datei erstellen, dem Anwendungscode und Paket (Zip) alle diese Dateien in einem Paket (CSPKG) Datei erstellen. Sie müssen auch die Dienstkonfigurationsdatei (mithilfe). Zum Bereitstellen Ihrer Rolle wird die Dateien CSPKG und mithilfe Azure Service Management API hochladen. Sobald Azure bereitgestellt wird Bereitstellung Instanzen im Rechenzentrum (basierend auf Konfigurationsdaten) Anwendungscode aus dem Paket extrahiert den Rolleninstanzen kopieren und Instanzen starten. Code ist jetzt einsatzbereit.

Die nachfolgende Abbildung zeigt die Dateien CSPKG und mithilfe auf dem Entwicklungscomputer erstellen. Die Datei CSPKG enthält die Datei CSDEF und Code für zwei Rollen. Nach dem Hochladen der Dateien CSPKG und mithilfe Azure Service Management API, erstellt Azure die Rolleninstanzen im Rechenzentrum. In diesem Beispiel angegebene Datei mithilfe, dass Azure drei Instanzen der Rolle erstellt \#1 und zwei Instanzen der Rolle \#2.

![Bild][5]

Weitere Informationen zur Bereitstellung finden aktualisieren und Neukonfigurieren Ihrer Rollen [Bereitstellen und Aktualisieren von Azure Applications][] .<a id="Ref" name="Ref"></a>

## <a id="references"> </a>Verweise

-   [Erstellen eines gehosteten Diensts für Azure][]

-   [Verwalten von gehosteten Dienste in Azure][]

-   [Migrieren von Anwendung in Azure][]

-   [Konfiguration von Azure-Anwendung][]

<div style="width: 700px; border-top: solid; margin-top: 5px; padding-top: 5px; border-top-width: 1px;">

<p>Geschrieben von Jeffrey Richter (Wintellect)</p>

</div>

  [Azure Modell Vorteile]: #benefits
  [Grundbegriffe gehosteter Dienst]: #concepts
  [Vorüberlegungen zum gehosteten Dienst]: #considerations
  [Entwerfen Ihre Anwendung für die Skalierung]: #scale
  [Hosted Servicedefinition und Konfiguration]: #defandcfg
  [Service-Definitionsdatei]: #def
  [Die Dienstkonfigurationsdatei]: #cfg
  [Erstellen und Bereitstellen von gehosteter Dienst]: #hostedservices
  [Referenzen]: #references
  [0]: ./media/application-model/application-model-3.jpg
  [1]: ./media/application-model/application-model-4.jpg
  [2]: ./media/application-model/application-model-5.jpg
  [Konfigurieren einen benutzerdefinierten Domänennamen in Azure]: http://www.windowsazure.com/develop/net/common-tasks/custom-dns/
  [Daten Speicherangebote in Azure]: http://www.windowsazure.com/develop/net/fundamentals/cloud-storage/
  [3]: ./media/application-model/application-model-6.jpg
  [4]: ./media/application-model/application-model-7.jpg
  
  [Azure Pricing]: http://www.windowsazure.com/pricing/calculator/
  [Verwalten von Zertifikaten in Azure]: http://msdn.microsoft.com/library/windowsazure/gg981929.aspx
  [http://msdn.Microsoft.com/library/windowsazure/ee758710.aspx]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
  [http://msdn.Microsoft.com/library/hh560567.aspx]: http://msdn.microsoft.com/library/hh560567.aspx
  [Verwalten von Azure Gäste OS Upgrades]: http://msdn.microsoft.com/library/ee924680.aspx
  [Azure-Verwaltungsportal]: http://manage.windowsazure.com/
  [5]: ./media/application-model/application-model-8.jpg
  [Bereitstellung und Aktualisierung von Azure Applications]: http://www.windowsazure.com/develop/net/fundamentals/deploying-applications/
  [Erstellen eines gehosteten Diensts für Azure]: http://msdn.microsoft.com/library/gg432967.aspx
  [Verwalten von gehosteten Dienste in Azure]: http://msdn.microsoft.com/library/gg433038.aspx
  [Migrieren von Anwendung in Azure]: http://msdn.microsoft.com/library/gg186051.aspx
  [Konfiguration von Azure-Anwendung]: http://msdn.microsoft.com/library/windowsazure/ee405486.aspx
