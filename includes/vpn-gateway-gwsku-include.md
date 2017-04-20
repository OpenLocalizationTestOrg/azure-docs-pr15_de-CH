Wenn Sie ein virtuelles Netzwerk-Gateway erstellen, müssen Sie Gateway SKU angeben, die Sie verwenden möchten. Bei der Auswahl einer höheren Gateway SKU weitere CPUs und Netzwerkbandbreite Gateway zugeordnet und daher Gateway höher Netzwerkdurchsatz an das virtuelle Netzwerk unterstützen.

VPN-Gateways können die folgenden SKUs:

- Grundlegende
- Standard
- Leistungsstarker

Wenn eine SKU Folgendes:

- Wenn Sie einen PolicyBased VPN verwenden möchten, verwenden Sie grundlegende SKU. Richtlinien-VPNs (zuvor als statisches Routing bezeichnet) werden auf anderen SKU nicht unterstützt.
- BGP ist auf die grundlegende SKU nicht unterstützt.
- ExpressRoute VPN-Gateway koexistieren Konfigurationen werden für grundlegende SKU nicht unterstützt.
- Aktive S2S VPN-Gateway-Verbindung können auf dem HighPerformance-SKU konfiguriert werden.
