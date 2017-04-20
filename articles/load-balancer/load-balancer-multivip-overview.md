<properties
   pageTitle="Mehrere VIPs für Azure Lastenausgleich | Microsoft Azure"
   description="Übersicht über mehrere VIPs auf Azure Lastenausgleich"
   services="load-balancer"
   documentationCenter="na"
   authors="chkuhtz"
   manager="narayan"
   editor=""
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/11/2016"
   ms.author="chkuhtz"
/>

# <a name="multiple-vips-for-azure-load-balancer"></a>Mehrere VIPs für Azure Lastenausgleich

Azure Lastenausgleich können Sie mit Saldo Dienste auf mehrere Ports oder mehrere IP-Adressen. Öffentliche und interne Load Balancer Definitionen können Sie einer Gruppe von VMs Saldo Flows laden.

Dieser Artikel beschreibt die Grundlagen dieser Möglichkeit, wichtige Konzepte und Nebenbedingungen. Wenn Sie auf eine IP-Adresse verfügbar machen möchten, finden Sie vereinfachte für [Öffentliche](load-balancer-get-started-internet-portal.md) und [interne](load-balancer-get-started-ilb-arm-portal.md) Balancer Konfigurationen geladen werden. Mehrere VIPs ist inkrementell eine einzelne VIP-Konfiguration. Die Konzepte in diesem Artikel verwenden, können Sie eine vereinfachte Konfiguration jederzeit erweitern.

Beim Definieren einer Azure Lastenausgleich ein Front-End- und Back-End-Konfiguration mit Regeln verbunden. Die Regel verweist Integritätstest dient wie neue Knoten im Back-End-Pool gesendet. Frontend wird durch eine VIP (Virtual IP), definiert ein 3-Tupel besteht aus einer IP-Adresse (public oder internal) ein Transportprotokoll (UDP oder TCP) und eine Portnummer. Ein DIP ist eine IP-Adresse eine Azure virtuelle NIC VM im Back-End-Pool zugeordnet.

Die folgende Tabelle enthält einige Beispiel Front-End-Konfigurationen:

| VIP | IP-Adresse | Protokoll | Anschluss |
|-----|------------|----------|------|
|1|65.52.0.1|TCP|80|
|2|65.52.0.1|TCP|_8080_|
|3|65.52.0.1|_UDP_|80|
|4|_65.52.0.2_|TCP|80|

Die Tabelle zeigt vier verschiedenen Frontends. Frontends #1, #2 und #3 sind eine einzige VIP mit mehreren Regeln. Port oder Protokoll für jeden Front-End ist jedoch die IP-Adresse verwendet. Frontends #1 und #4 sind ein Beispiel für mehrere VIPs dem gleichen Front-End-Protokoll und Port über mehrere VIPs wiederverwendet werden.

Azure Lastenausgleich bietet Flexibilität beim Definieren des Lastenausgleich Regeln. Eine Regel wird erklärt, wie eine Adresse und Port auf dem Front-End zugeordnet ist die Zieladresse und der Back-End-Port. Ob Backend-Ports über Regeln wiederverwendet werden, hängt vom Typ der Regel ab. Jede Regel hat Anforderungen, die Host-Konfiguration und Sonde Entwurf auswirken können. Es gibt zwei Arten von Regeln:

1. Die Standardregel mit keine Back-End-port
2. Bewegliche IP-Regel, Back-End-Ports wiederverwendet werden

Azure Lastenausgleich können beide Regel Load Balancer identisches mischen. Lastenausgleich können gleichzeitig für eine VM oder einer Kombination solange Sie durch die Einschränkung der Regel. Regeltyp hängt davon ab, an die Anwendung und die Komplexität der Konfiguration unterstützen. Überprüfen Sie die Regel für Ihr Szenario am besten geeignet sind.

Wir untersuchen diese Szenarios weiter mit Standardverhalten.

## <a name="rule-type-1-no-backend-port-reuse"></a>Regeltyp #1: keine Back-End-Port wiederverwenden

![Abbildung MultiVIP](./media/load-balancer-multivip-overview/load-balancer-multivip.png)

In diesem Szenario sind die Front-End-VIPs wie folgt konfiguriert:

| VIP | IP-Adresse | Protokoll | Anschluss |
|-----|------------|----------|------|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|TCP|80|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|*65.52.0.2*|TCP|80|

DIP ist das Ziel der eingehenden Fluss. Im Back-End-Pool stellt jede VM den gewünschten Dienst einen eindeutigen Port auf sich. Dieser Dienst ist über Regeldefinitionen Frontend zugeordnet.

Wir definieren zwei Regeln:

| Regel | Front-End-Karte | Back-End-Pool |
|------|--------------|-----------------|
| 1 | ![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 | ![Back-End](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP1:80, ![Back-End](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP2:80 |
| 2 | ![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 | ![Back-End](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP1:81, ![Back-End](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP2:81 |

Vollständige Zuordnung in Azure Lastenausgleich sieht nun folgendermaßen aus:

| Regel | VIP-Adresse | Protokoll | Anschluss | Ziel | Anschluss |
|------|----------------|----------|------|-----|------|
|![Regel](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|TCP|80|DIP-IP-Adresse|80|
|![Regel](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|65.52.0.2|TCP|80|DIP-IP-Adresse|81|

Jede Regel muss einen Datenfluss mit eine eindeutige Kombination aus IP-Zieladresse und Zielport erzeugen. Durch variieren den Zielport des Flusses, liefern mehrere Regeln Flows gleichen DIP andere Ports.

Gesundheit Prüfpunkte werden immer DIP eines virtuellen Computers weitergeleitet. Sie müssen Sie sicherstellen, dass der Prüfpunkt den Zustand der VM widerspiegelt.

## <a name="rule-type-2-backend-port-reuse-by-using-floating-ip"></a>Regeltyp #2: Back-End-Port Wiederverwendung mit Floating IP

Azure Lastenausgleich bietet die Flexibilität den Frontend-Port mehrere VIPs unabhängig von der verwendeten Regeltyp wiederzuverwenden. Darüber hinaus einige Anwendungsszenarien möchten oder müssen denselben Anschluss von mehreren Anwendungsinstanzen auf einer einzelnen VM im Back-End-Pool verwendet werden. Beispiele für Port Wiederverwendung clustering für hohe Verfügbarkeit gehören, virtuelle Appliances, und mehrere TLS Endpunkte ohne erneute Verschlüsselung verfügbar machen.

Ggf. den Backend-Port über mehrere Regeln wiederverwenden müssen Sie bewegliche IP in die Regeldefinition aktivieren.

Umlaufende IP-ist ein Teil als direkte Server zurückgeben (DSR) bekannt ist. DSR besteht aus zwei Teilen: eine Flow und eine IP-Adresse aus. Ebene Plattform funktioniert Azure Lastenausgleich immer in einer Topologie DSR Fluss Floating IP oder nicht aktiviert ist. Dies bedeutet, dass ausgehende Teil eines Flusses immer richtig fließen direkt auf den Ursprung angepasst wird.

Mit dem Standard-Regeltyp macht Azure traditionelle Lastenausgleich IP Adresse Zuordnungsschema Bedienung. Bewegliche IP aktivieren ändert die IP-Adresse Zuordnungsschema ermöglicht zusätzliche Flexibilität wie unten beschrieben.

Die folgende Abbildung zeigt diese Konfiguration:

![Abbildung MultiVIP](./media/load-balancer-multivip-overview/load-balancer-multivip-dsr.png)

In diesem Szenario hat jede VM im Back-End-Pool drei Netzwerkschnittstellen:

* DIP: eine VM (Azure NIC Ressource) zugeordnete virtuelle Netzwerkkarte
* VIP1: eine Loopbackschnittstelle im Gastbetriebssystem, die mit IP-Adresse des VIP1 konfiguriert
* VIP2: eine Loopbackschnittstelle im Gastbetriebssystem, die mit IP-Adresse des VIP2 konfiguriert

>[AZURE.IMPORTANT] Die Konfiguration der logischen Schnittstellen erfolgt in das Gastbetriebssystem. Diese Konfiguration wird nicht ausgeführt oder von Azure. Ohne diese Konfiguration funktionieren die Regeln nicht. Gesundheit Prüfpunktdefinitionen verwenden DIP VM als logische VIP. Daher muss Ihr Dienst einen DIP-Port, der Stand der Service auf der logischen VIP Prüfpunkt Antworten bereitstellen.

Nehmen wir an der gleichen Front-End-Konfiguration wie im vorherigen Szenario:

| VIP | IP-Adresse | Protokoll | Anschluss |
|-----|------------|----------|------|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|TCP|80|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|*65.52.0.2*|TCP|80|

Wir definieren zwei Regeln:

| Regel | Front-End-Karte | Back-End-Pool |
|------|--------------|-----------------|
| 1 | ![Regel](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 | ![Back-End](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 (in VM1 und VM2) |
| 2 | ![Regel](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 | ![Back-End](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 (in VM1 und VM2) |

Die folgende Tabelle zeigt vollständige Zuordnung im Lastenausgleich:

| Regel | VIP-Adresse | Protokoll | Anschluss | Ziel | Anschluss |
|------|----------------|----------|------|-------------|------|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|TCP|80|wie VIP (65.52.0.1)|wie VIP (80)|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|65.52.0.2|TCP|80|wie VIP (65.52.0.2)|wie VIP (80)|

Das Ziel der eingehenden Fluss ist die VIP-Adresse auf die Loopback-Schnittstelle auf dem virtuellen Computer. Jede Regel muss einen Datenfluss mit eine eindeutige Kombination aus IP-Zieladresse und Zielport erzeugen. Durch variieren der IP-Zieladresse des Flusses, ist Port Wiederverwendung auf dem gleichen virtuellen Computer möglich. Der Dienst ist Lastenausgleich mit VIP IP-Adresse und Port des jeweiligen Loopbackschnittstelle ausgesetzt.

Beachten Sie, dass in diesem Beispiel wird den Zielport nicht ändern. Obwohl dies ein schwebendes IP-Szenario ist, unterstützt Azure Lastenausgleich Definieren einer Regel Backend-Zielport schreiben und von den Front-End-Zielport unterscheiden.

Bewegliche IP-Regeltyp beruht auf mehrere Load Balancer Konfigurationsmuster. Ein Beispiel, das derzeit ist [SQL AlwaysOn mit mehrere Listener](../virtual-machines/virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md) -Konfiguration. Im Laufe der Zeit werden wir weitere solche Szenarios dokumentieren.

## <a name="limitations"></a>Grenzen

* Mehrere VIP-Konfigurationen werden nur mit IaaS VMs unterstützt.
* Mit Floating IP-Regel muss die Anwendung DIP ausgehenden Datenflüsse verwenden. Bindet die Anwendung konfiguriert die Loopback-Schnittstelle im Gastbetriebssystem VIP-Adresse dann SNAT steht keine ausgehenden Ablauf schreiben und der Fluss schlägt fehl.
* Öffentliche IP-Adressen beeinflussen Abrechnung. Weitere Informationen finden Sie unter [IP-Adresse Preisgestaltung](https://azure.microsoft.com/pricing/details/ip-addresses/)
* Abonnement gilt. Weitere Informationen finden Sie unter [Service Grenzen](../azure-subscription-service-limits.md#networking-limits) Details.
