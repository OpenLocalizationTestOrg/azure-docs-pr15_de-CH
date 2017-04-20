|    | **VPN-Gateway-Durchsatz (1)** | **Max. IPsec VPN-Gateway-Tunnel (2)** | **ExpressRoute-Gateway-Durchsatz** | **VPN-Gateway und ExpressRoute kombiniert werden.**|
|--- |----------------------------|-----------------------------------|-------------------------------------|-----------------------------------------|
| **Grundlegende SKU (3)(5)**              |  100 Mbit/s | 10                         |  500 Mbit/s                           | Nein   |
| **Standard-SKU ((4)(5)**           |  100 Mbit/s | 10                         | 1000 Mbit/s                           | Ja  |
| **Hohe Leistung SKU (4)**   | 200 Mbit/s  | 30                         | 2000 Mbit/s                           | Ja  |

- (1) ist VPN eine grobe Schätzung auf Maße zwischen VNets in derselben Azure-Region basiert. Es ist nicht garantiert Durchsatz für standortübergreifende Verbindungen über das Internet. Es sind maximal mögliche Durchsatz.
- (2) die Anzahl der Tunnel finden Sie RouteBased VPNs. PolicyBased VPN unterstützt nur eine Standort-zu-Standort-VPN-Tunnel.
- (3) BGP ist für die grundlegende SKU nicht unterstützt.
- (4) Richtlinien VPNs sind für diese SKU nicht unterstützt. Sie sind für die grundlegende SKU unterstützt.
- (5) aktive S2S VPN-Gateway-Verbindungen werden für diese SKU nicht unterstützt. Aktive wird auf dem HighPerformance-SKU unterstützt.