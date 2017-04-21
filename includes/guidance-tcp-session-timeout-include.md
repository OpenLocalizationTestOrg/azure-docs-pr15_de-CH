##<a name="tcp-settings-for-azure-vms"></a>TCP-Einstellungen für Azure VMs

Azure VMs kommunizieren mithilfe von [NAT] mit dem öffentlichen Internet[ nat] (Network Address Translation). NAT-Geräte Zuweisen einer öffentlichen IP-Adresse und Port Azure-VM damit VM einen Socket für die Kommunikation mit anderen Geräten herzustellen. Wenn Pakete nicht über diesen Socket nach einer bestimmten Zeit fortgesetzt mehr, bricht das NAT-Gerät die Zuordnung und Sockel ist von anderen virtuellen Computern verwendet werden.

Dies ist ein verbreitetes Verhalten von NAT, das Kommunikation auf TCP-basierte Programme, die einen Socket verursachen können weiterhin über einen Timeoutzeitraum erwarten. Es werden zwei Leerlaufzeitlimit für Sessions Status *Verbindung* berücksichtigen:

- **eingehende** durch [Azure Lastenausgleich][azure-lb-timeout]. Dieses Timeout standardmäßig 4 Minuten und kann auf 30 Minuten eingestellt werden.
- **ausgehende** [SNAT] mit[ snat] (Quell-NAT). Dieses Timeout 4 Minuten festgelegt und kann nicht angepasst werden.

Um sicherzustellen, dass die Verbindung über das Zeitlimit nicht verloren gehen, sollten Sie die Anwendung hält die Sitzung aktiv oder das zugrunde liegenden Betriebssystem dazu konfigurieren sicherstellen. Die Einstellungen unterscheiden sich für Linux- und Windows-Systemen, wie unten dargestellt.

Für [Linux][linux], ändern Sie die folgenden Kernelvariablen.
NET.IPv4.tcp_keepalive_time = 120 net.ipv4.tcp_keepalive_intvl = 30 net.ipv4.tcp_keepalive_probes = 8
 
[Windows][windows], sollten Sie die folgenden Registrierungswerte ändern.
KeepAliveInterval = 30 KeepAliveTime = 120 TcpMaxDataRetransmissions = 8


Die Einstellungen sicherstellen ein Keepalive-Pakets nach 2 Minuten (120 Sekunden) Leerlaufzeit gesendet und dann alle 30 Sekunden gesendet. Und wenn nicht 8 dieser Pakete die Sitzung gelöscht.

<!-- links -->
[nat]: http://computer.howstuffworks.com/nat.htm
[snat]: ../load-balancer/load-balancer-overview.md/#source-nat
[linux]: http://tldp.org/HOWTO/TCP-Keepalive-HOWTO/usingkeepalive.html
[windows]: http://blogs.technet.com/b/nettracer/archive/2010/06/03/things-that-you-may-want-to-know-about-tcp-keepalives.aspx
[azure-lb-timeout]: ../load-balancer/load-balancer-tcp-idle-timeout.md