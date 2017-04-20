Die folgende Tabelle listet an Richtlinien und RouteBased VPN-Gateways. Diese Tabelle gilt für den Ressourcen-Manager und klassischen Bereitstellungsmodelle. Der Klassiker PolicyBased VPN-Gateways sind statische Gateways identisch und Route basierende Gateways sind dynamische Gateways identisch.


|   | **Richtlinien Basic VPN-Gateway** | **RouteBased Basic VPN-Gateway** | **RouteBased-Standard VPN-Gateway**   | **RouteBased hoher Performance VPN-Gateway** |
|---|---------------------------------------|---------------------------------------|----------------------------|----------------------------------|
|    **Standort-zu-Standort-Verbindung (S2S)**  | PolicyBased VPN-Konfiguration        | RouteBased VPN-Konfiguration  | RouteBased VPN-Konfiguration     | RouteBased VPN-Konfiguration    |
| **Punkt-zu-Standort-Verbindung (P2S**)      | Nicht unterstützt   | Unterstützt (können Koexistenz mit S2S)  | Unterstützt (können Koexistenz mit S2S)  | Unterstützt (können Koexistenz mit S2S) |
| **Authentifizierungsmethode**                 |    Vorinstallierter Schlüssel  | Vorinstallierten Schlüssel für Zertifikate für P2S-Konnektivität S2S-Konnektivität | Vorinstallierten Schlüssel für Zertifikate für P2S-Konnektivität S2S-Konnektivität | Vorinstallierten Schlüssel für Zertifikate für P2S-Konnektivität S2S-Konnektivität |
| **Maximale Verbindungsanzahl S2S**       | 1                              | 10                                                                    | 10                                | 30                               |
| **Maximale Verbindungsanzahl P2S**       | Nicht unterstützt                  | 128                                                                   | 128                               | 128                              |
|**Aktive Unterstützung (BGP)**           | Nicht unterstützt                  | Nicht unterstützt                                                         | Unterstützt                     | Unterstützt                   |
 
