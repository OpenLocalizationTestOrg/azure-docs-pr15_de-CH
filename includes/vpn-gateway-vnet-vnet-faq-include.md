- Virtuelle Netzwerke können denselben oder anderen Azure Regionen (Standorte).

- Cloud-Dienst oder ein Lastenausgleich Endpunkt kann nicht über virtuelle Netzwerke umfassen, auch wenn sie miteinander verbunden sind.

- Verbinden mehrere Azure virtuelle Netzwerke erforderlich keine lokalen VPN-Gateways, wenn standortübergreifende Konnektivität erforderlich ist.

- VNet VNet unterstützt virtuelle Netzwerke verbinden. Nicht verbundenen virtuellen Maschinen unterstützen oder cloud-Dienste nicht in ein virtuelles Netzwerk.

- VNet VNet erfordert Azure VPN-Gateways mit RouteBased (früher dynamisches Routing) VPN-Typen. 

- Virtuelle Netzwerkkonnektivität kann gleichzeitig mit Multi-Standort-VPNs mit maximal 10 (Standard-Standard-Gateways) verwendet oder 30 (hohe Leistung Gateways)-VPN-Tunnel für ein virtuelles Netzwerk VPN-Gateway mit anderen virtuellen Netzwerken lokalen Standorten.

- Die Adressräume der virtuellen Netzwerke und lokalen LAN dürfen nicht überlappen. Sich überlappende Adressräume verursacht die Erstellung der VNet VNet-Verbindungen nicht.

- Redundante Tunnel zwischen zwei virtuelle Netzwerke werden nicht unterstützt.

- Alle VPN-Tunnel des virtuellen Netzwerks teilen die verfügbare Bandbreite auf Azure VPN-Gateway und die gleichen VPN-Gateway Betriebszeit SLA in Azure.

- VNet VNet Datenverkehr bewegt über das Microsoft Network nicht im Internet.

- VNet VNet Datenverkehr innerhalb derselben Region ist für beide Richtungen. Cross-Bereich VNet-VNet-Ausgang ist basierten auf quellregionen ausgehende inter-VNet-Datenübertragungsraten Datenverkehr angeklagt. Finden Sie die [Preisseite](https://azure.microsoft.com/pricing/details/vpn-gateway/) für Details.