## <a name="load-balancer-differences"></a>Load Balancer Unterschiede

Es gibt verschiedene Optionen zum Verteilen des Netzwerkverkehrs mit Microsoft Azure. Diese Optionen funktionieren anders, nachdem eine andere Funktion und unterstützen Szenarien. Sie können jede Kombination oder isoliert verwendet werden.

- **Azure-Lastenausgleich** arbeitet auf der Transportschicht (Ebene 4 des OSI-Stacks Verweis). Es bietet Verteilung des Datenverkehrs auf Instanzen einer Anwendung im gleichen Azure-Rechenzentrum.

- **Application Gateway** arbeitet auf der Anwendungsebene (Layer 7 des OSI-Stacks Verweis). Sie fungiert als Reverse Proxy-Dienst beendet die Verbindung und Weiterleiten von Anfragen an den Back-End-Endpunkte.

- **Traffic Manager** funktioniert DNS-Ebene.  DNS-Antworten verwendet direkten Endbenutzer-Verkehr an weltweit verteilten Endpunkte. Clients verbinden dann direkt mit diesen Endpunkten.

In der folgenden Tabelle werden die Funktionen jeder Dienst zusammengefasst:

| Dienst | Azure Lastenausgleich | Application Gateway | Traffic Manager |
|---|---|---|---|
|Technologie| Transportebene (Ebene 4) | Anwendungsebene (Folie 7) | DNS-Ebene |
| Anwendungsprotokolle unterstützt | Alle | HTTP und HTTPS |  (Ein HTTP-Endpunkt ist erforderlich für die endpunktüberwachung) |
| Endpunkte | Azure VMs und Cloud-Services-Instanzen | Azure interne IP-Adresse oder öffentlichen Internet-IP-Adresse | Azure VMs, Cloud-Services und Azure Web Apps, externer Endpunkte |
| Vnet-Unterstützung | Kann für beide Internet gerichteten und interne (Vnet) verwendet werden | Kann für beide Internet gerichteten und interne (Vnet) verwendet werden |    Unterstützt nur Internetzugriff Applikationen |
Endpunkt überwachen | Über Prüfpunkte unterstützt | Über Prüfpunkte unterstützt | Über HTTP/HTTPS GET unterstützt | 

Azure Lastenausgleich und Application Gateway Route Netzwerkverkehr an Endpunkte verfügen jedoch über unterschiedliche Szenarien, welcher Datenverkehr verarbeiten. In der folgenden Tabelle wird der Unterschied zwischen den beiden Lastenausgleich:

| Typ | Azure Lastenausgleich | Application Gateway |
|---|---|---|
| Protokolle | UDP/TCP | HTTP / HTTPS |
| IP-Reservierung | Unterstützt | Nicht unterstützt | 
| Load balancing Modus | 5 tuple(source IP, source port, destination IP, destination port, protocol type) | Round-Robin<br>Routing basierend auf URL | 
| Lastenausgleich Modus (Quell-IP / sticky Sessions) |  2-Tupel (Quell-IP- und Ziel-IP), 3-Tupel (Quell-IP Ziel-IP und Port). Nach oben oder unten skalierbar basierend auf der Anzahl von virtuellen Maschinen | Cookie-basierte Affinität<br>Routing basierend auf URL |
| Gesundheit Prüfpunkte | Standard: Prüfpunkt Interval - 15 Sekunden. Aus der Rotation ausgenommen: 2 fortlaufende Fehler. Unterstützt benutzerdefinierte Prüfpunkte | Im Leerlauf Prüfpunkt Intervall 30 Sekunden. Nach 5 aufeinander folgenden Verkehrsinformationen Fehler oder einen einzigen Fehler im Leerlauf entnommen. Unterstützt benutzerdefinierte Prüfpunkte | 
| SSL-Abladung | Nicht unterstützt | Unterstützt | 
  