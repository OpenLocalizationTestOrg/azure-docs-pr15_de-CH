<properties
    pageTitle="Cassandra mit Linux auf Azure ausführen | Microsoft Azure"
    description="Cassandra Cluster unter Linux in Azure Virtual Machines von Node.js-Anwendung ausführen"
    services="virtual-machines-linux"
    documentationCenter="nodejs"
    authors="hanuk"
    manager="wpickett"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="hanuk;robmcm"/>

# <a name="running-cassandra-with-linux-on-azure-and-accessing-it-from-nodejs"></a>Cassandra mit Linux auf Azure und Node.js zugreifen

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Erfahren Sie, wie [diese Schritte mit dem Ressourcen-Manager-Modell](https://azure.microsoft.com/documentation/templates/datastax-on-ubuntu/).

## <a name="overview"></a>Übersicht
Microsoft Azure ist eine offene Cloudplattform Microsoft als auch als nicht-Microsoft-Software umfasst Betriebssysteme Anwendungsserver, messaging Middleware sowie SQL und NoSQL-Datenbanken sowohl kommerzielle und open Source-Modelle. Sowie Services für öffentliche Wolken einschließlich Azure erstellen erfordert sorgfältige Planung und absichtliche Architektur für Applikationen Server als auch Speicherebenen. Verteilte Speicherarchitektur des Cassandra hilft beim Erstellen von Systems mit hoher Verfügbarkeit, die Fehlertoleranz für Clusterfehler. Cassandra ist eine Cloud NoSQL-Datenbank von Apache Software Foundation am cassandra.apache.org; Cassandra ist in Java geschrieben und damit sowohl Windows als auch Linux Plattformen ausgeführt.

Dieser Artikel konzentriert Cassandra Bereitstellung auf Ubuntu als einzelne und mehrere Rechenzentren Cluster nutzt Microsoft Azure-Computer und virtuelle Netzwerke angezeigt. Clusterbereitstellung für Produktionsarbeitslasten optimiert ist außerhalb des Bereichs dieses Artikels als Multi-Laufwerk-Knotenkonfiguration, entsprechende Ring Topologiedesign und Modellierung zur Unterstützung der Replikation erforderlichen Datenkonsistenz, Durchsatz und hohe Verfügbarkeit erforderlich.

Dieses Artikels nimmt Grundkonzept was im Zusammenhang mit dem Einrichten des Clusters Cassandra verglichen Andockfenster oder Verwaltungsangestellte Marionette der infrastrukturbereitstellung erleichtern kann.  

## <a name="the-deployment-models"></a>Die Bereitstellungsmodelle
Microsoft Azure-Netzwerke ermöglicht die Bereitstellung der isolierten private Cluster der, die Zugang zu feinkörnige Sicherheit beschränkt werden kann.  Da mit Cassandra Bereitstellung auf einer grundlegenden Ebene ist, konzentrieren wir uns nicht Konsistenz und optimale Speicherdesign für den Durchsatz. Hier ist die Liste der Netzwerke für unser hypothetisches Cluster:

- Externe Systeme können nicht Cassandra Datenbank von innerhalb oder außerhalb Azure zugegriffen werden.
- Cassandra Cluster muss hinter einem Lastenausgleich Sparsamkeit Datenverkehr
- Bereitstellen Sie Cassandra Knoten in zwei Gruppen in jedem Rechenzentrum für eine verbesserte Clusterverfügbarkeit
- Sperren des Clusters so, dass nur Serverfarm Anwendung Zugriff auf die Datenbank direkt
- Keine öffentlichen Netzwerk Endpunkte als SSH
- Jeder Knoten Cassandra benötigt eine feste interne IP-Adresse

Cassandra kann eine Azure Region oder mehrere Regionen basierend auf die verteilte Natur der Arbeitslast bereitgestellt werden. Mit mehreren Bereitstellungsmodell kann Endbenutzer näher bestimmten geografischen über dieselbe Cassandra Infrastruktur dienen genutzt werden. Cassandra des integrierten Knoten Replikation übernimmt die Synchronisation mit mehreren Mastern schreibt mehrere Rechenzentren aus und stellt eine konsistente Ansicht der Daten in. Bereitstellung mit mehreren helfen auch bei der Risikominderung breiteren Azure Dienstausfälle. Cassandra des einstellbaren Konsistenz und Replikationstopologie hilft Bedürfnissen verschiedener RPO Applikationen.

### <a name="single-region-deployment"></a>Bereitstellung in einer Region
Wir beginnen mit einem einzelnen Bereich Bereitstellung und erstellen Sie ein Modell mit mehreren Gelerntes. Azure virtuelle Netzwerke verwendet isolierten Subnetze erstellen, damit der Netzwerk Sicherheit genannten erfüllt werden können.  Schritte bei der Erstellung der Bereitstellung in einer Region verwendet Ubuntu 14.04 LTS und Cassandra 2.08. der Prozess kann jedoch leicht andere Linux-Varianten übernommen werden. Es folgen einige Aspekte der Bereitstellung in einer Region systemische.  

**Hohe Verfügbarkeit:** In Abbildung 1 gezeigte Cassandra-Knoten werden auf zwei Verfügbarkeit bereitgestellt, sodass mehrere Fehlerdomänen für hohe Verfügbarkeit Knoten verteilt werden. VMs mit Verfügbarkeit kommentiert 2 Fehlerdomänen zugeordnet ist.  Microsoft Azure verwendet das Konzept der Fehlerdomäne zu ungeplanten Ausfallzeiten (z.B. Hardware- oder Softwarefehlers) das Konzept der Aktualisierungsdomäne (Host oder Gast OS Patches und Upgrades, Anwendungsupgrades) Verwalten von geplanten Ausfälle zum. [Disaster Recovery und hohe Verfügbarkeit für Azure Applications](http://msdn.microsoft.com/library/dn251004.aspx) für die Rolle des Fehlers angezeigt, und aktualisieren Sie Domänen hohen Verfügbarkeit erreichen.

![Bereitstellung in einer region](./media/virtual-machines-linux-classic-cassandra-nodejs/cassandra-linux1.png)

Abbildung 1: Bereitstellung in einer region

Beachten Sie, dass zum Zeitpunkt der Erstellung dieses Dokuments Azure explizite Zuordnung einer Gruppe von VMs auf bestimmten Fehlerdomäne nicht; Folglich ist es auch mit den in Abbildung 1 gezeigte Bereitstellungsmodell statistisch wahrscheinlich, dass die virtuellen Computer zwei Fehlerdomänen statt vier zugeordnet werden kann.

**Lastenausgleich Sparsamkeit Verkehr:** Sparsamkeit Clientbibliotheken innerhalb der Webserver verbinden Cluster über einen internen Lastenausgleich. Dies erfordert Lastenausgleich interne Subnetz "Data" hinzufügen (siehe Abbildung 1) im Kontext der Cloud-Dienst Cassandra-Cluster hosten. Definiertes internen Lastenausgleich erfordert jeder Knoten Lastenausgleich Endpunkt mit Kommentaren mehrerer Lastenausgleich mit zuvor definierten Load Balancer Namen hinzugefügt werden. Weitere Informationen finden Sie in der [Internen Lastenausgleich Azure ](../load-balancer/load-balancer-internal-overview.md).

**Körner cluster:** Es ist wichtig, die meisten hochverfügbaren Knoten für Saatgut auswählen, wie die neuen Knoten mit Saatgut der Topologie des Clusters zu kommunizieren. Seed-Knoten bezeichnet einen Knoten aus jeder Verfügbarkeit um Einzelpunktversagen zu vermeiden.

**Replikationsfaktor und Konsistenz auf:** Cassandra integrierte hohe Verfügbarkeit Haltbarkeit zeichnet sich durch Replikationsfaktor (RF - Anzahl Kopien jeder Zeile im Cluster gespeichert) und Konsistenz (Anzahl der Replikate gelesen/geschrieben werden, bevor das Ergebnis an den Aufrufer zurückgegeben). Replikationsfaktor wird während des Erstellens der erledigten (ähnlich einer relationalen Datenbank) angegeben, während auf Konsistenz beim Ausgeben der CRUD-Abfrage angegeben wird. Cassandra Dokumentation [Konfigurieren Konsistenz](http://www.datastax.com/documentation/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) ausführliche Konsistenz und die Formel für die Berechnung der Quorumressource.

Cassandra unterstützt zwei Arten von Integrität-Modelle – Konsistenz und Eventual Consistency; die Replikationsfaktor und Konsistenz wird zusammen festzustellen, ob die Daten konsistent, sobald ein Schreibvorgang abgeschlossen ist, oder es schließlich konsistent werden. Beispielsweise QUORUM wie Konsistenzebene immer sorgt Daten Konsistenz keinerlei Konsistenz, die Anzahl der Replikate nach Bedarf zu schreibenden QUORUM (z.B. eine) Daten werden schließlich führt.

Die obigen Replikation Faktor 3 und QUORUM Cluster mit 8 Knoten (2 Knoten gelesen bzw. geschrieben werden Konsistenz) Lese-/Schreibzugriff auf Konsistenz, theoretischen Verlust von höchstens 1 Knoten pro Replikationsgruppe vor Anwendung den Fehler bemerken überleben können. Dies setzt voraus, dass alle Schlüssel Leerzeichen ausgewogen haben Anfragen schreibgeschützt.  Folgende sind die Parameter, für den bereitgestellten Cluster mit:

Clusterkonfiguration für einzelne Region Cassandra:

| Clusterparameter | Wert | Beschreibung |
| ----------------- | ----- | ------- |
| Anzahl der Knoten (N) | 8   | Gesamtanzahl der Knoten im cluster |
| Replikationsfaktor (RF) | 3 | Anzahl der Replikate einer gegebenen Zeile |
| Konsistenz auf (Schreiben) | QUORUM[(RF/2) +1) = 2] das Ergebnis der Formel wird abgerundet. | Höchstens 2 Replikate schreibt, bevor die Antwort an den Aufrufer gesendet wird. 3. Replikat schriftlich schließlich konsistent. |
| Konsistenz auf (Lesen) | QUORUM [(RF/2) + 1 = 2] das Ergebnis der Formel wird abgerundet. | Liest 2 Replikate vor dem Senden der Antwort an den Aufrufer. |
| Strategie für die Replikation | NetworkTopologyStrategy Siehe [Datenreplikation](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureDataDistributeReplication_c.html) in Cassandra – Dokumentation | Versteht die Bereitstellungstopologie und Replikate in Knoten, dass alle Replikate auf einem Rack geraten |
| Snitch | GossipingPropertyFileSnitch in Cassandra Dokumentation weitere Informationen siehe [Snitches](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureSnitchesAbout_c.html) | NetworkTopologyStrategy verwendet ein Konzept von Snitch die Topologie verstehen. GossipingPropertyFileSnitch bietet eine bessere Steuerung im Rechenzentrum und Rack jedem Knoten zuordnen. Der Cluster verwendet dann Klatsch diese Informationen zu verbreiten. Dies ist viel einfacher, dynamische IP-Einstellung gegenüber PropertyFileSnitch |


**Azure Considerations for Cassandra Cluster:** Microsoft Azure Virtual Machines Funktion verwendet Azure BLOB-Speicher für Datenträger Dauerhaftigkeit. Azure-Speicher speichert 3 Replikate der Laufwerke für lange Haltbarkeit. Das bedeutet, jede Zeile von Daten in eine Tabelle Cassandra bereits 3 Replikate gespeichert und daher Datenkonsistenz bereits erledigt ist Replikation Faktor (RF) 1. Das Hauptproblem mit 1-Replikation werden Ausfallzeiten wirklichen Ausfall ein einzelnen Cassandra-Knoten. Wenn ein Knoten ausfällt Probleme (Hardware, Software Systemausfälle) von Azure Fabric Controller erkannt, wird es einen neuen Knoten mit demselben Speicherlaufwerke stattdessen bereitstellen. Bereitstellung eines neuen Knotens zum Ersetzen der alten kann einige Minuten dauern.  Ebenso für geplante Wartungsarbeiten wie Gast OS Cassandra aktualisiert, und ändert sich Azure Fabric Controller führt Wechsel der Knoten im Cluster.  Wechsel auch Sie einige Knoten übernehmen kann und damit des Clusters kann kurze Ausfallzeit für einige Partitionen. Jedoch verloren die Daten nicht durch den Azure-Speicher Redundanz.  

Für Systeme, Azure, die hohen Verfügbarkeit (z. B. um 99,9 der entspricht 8,76 Stunden pro Jahr, Einzelheiten finden Sie unter [Hohe Verfügbarkeit](http://en.wikipedia.org/wiki/High_availability) ) erfordert möglicherweise mit RF = 1 und Konsistenz Level = ein.  Für Applikationen mit hoher Verfügbarkeit RF = 3 und Konsistenz Level = QUORUM toleriert die Zeit von einem Knoten eines der Replikate. RF = 1 in herkömmliche Bereitstellung (z. B. lokale) nicht aufgrund der möglichen Probleme wie Festplattenfehler verwendet werden.   

## <a name="multi-region-deployment"></a>Mit mehreren Bereitstellung
Cassandra des Data-Center-aware Replikation und Konsistenz beschriebenen hilft bei der Bereitstellung mit mehreren ohne die Notwendigkeit von externen Tools. Dies unterscheidet von den herkömmlichen relationalen Datenbanken, kann das Setup für das Datenbankspiegeln für Multi-master Schreibvorgänge sehr komplex sein. Cassandra in einer Multi-Region einrichten können mit Verwendungsszenarien, einschließlich der folgenden:

**Nähe basierte Bereitstellung:** Multi-Tenant-Applikationen mit klar Mieter Benutzer-zu-Bereich kann mit mehreren Cluster niedrige Wartezeiten ergaben. Beispielsweise ein Learning Management Systeme für Bildungseinrichtungen bereitstellen ein verteilten Clusters in USA Osten und Westen der USA zu den jeweiligen Standorten für Transaktions-und Analysen. Die Daten können zur Zeit Lese- und Schreibvorgänge lokal konsistent sein und können übereinstimmen schließlich in den Regionen. Es gibt andere Beispiele wie Medien, e-Commerce und alles und alles, was Geo konzentrierten Benutzer Basis dient ein Nutzen bei diesem Bereitstellungsmodell.

**Hohe Verfügbarkeit:** Redundanz ist ein wichtiger Faktor beim Erzielen hoher Verfügbarkeit von Software und Hardware. Einzelheiten finden Sie unter Erstellen zuverlässiger Cloudsysteme auf Microsoft Azure. Microsoft Azure ist die einzige zuverlässige Möglichkeit echte Redundanz durch Bereitstellen eines Serverclusters mit mehreren. Applikationen in einem aktiv / aktiv- oder Aktiv / Passiv-Modus eingesetzt werden und ist eines der unten können Azure Traffic Manager Datenverkehr in den aktiven Bereich leiten.  Mit der Bereitstellung in einer Region, ist die Verfügbarkeit von 99,9, zwei Region Bereitstellung erreichen eine Verfügbarkeit von 99,9999 durch die Formel berechnet: (1-(1-0.999) *(1-0,999))*100); finden Sie im obigen Artikel Details.

**Wiederherstellung:** Ordnungsgemäß entworfen, kann mit mehreren Cassandra Cluster katastrophalem Center Ausfälle standhalten. Wenn Region ausfällt, kann in anderen Regionen bereitgestellte Anwendung starten mit den Endbenutzern. Die Anwendung muss wie alle anderen Business Continuity Implementierungen Toleranz für Datenverlust anhand der Daten in der asynchrone Pipeline. Macht Cassandra – die Wiederherstellung wesentlich schneller als herkömmliche Datenbank-Recovery-Prozesse Zeit. Abbildung 2 zeigt das Modell mit mehreren typischerweise mit acht Knoten in jeder Region. Beide Bereiche sind Spiegelbilder voneinander für dasselbe Symmetrie. reale Designs hängt der Arbeitsauslastungstyp (Transaktions- oder analytische), RPO RTO, Konsistenz und Verfügbarkeit.

![Bereitstellung von Multi-region](./media/virtual-machines-linux-classic-cassandra-nodejs/cassandra-linux2.png)

Abbildung 2: Mit mehreren Cassandra Bereitstellung

### <a name="network-integration"></a>Netzwerkintegration
Gruppen virtueller Maschinen bereitgestellt mit privaten Netzwerken befindet sich auf zwei Bereiche miteinander über einen VPN-Tunnel kommuniziert. Der VPN-Tunnel verbindet zwei Software-Gateways während des Bereitstellungsprozesses Netzwerk bereitgestellt. Beide Bereiche haben ähnliche Netzwerkarchitektur "Web" und "Daten" Subnetze. Azure-Netzwerken können je nach Bedarf beliebig viele Subnetze und Anwenden von ACLs vom Netzwerksicherheit. Beim Entwerfen der Clustertopologie inter Data Center Kommunikationslatenz und der wirtschaftlichen Netzwerkverkehr berücksichtigt werden müssen.

### <a name="data-consistency-for-multi-data-center-deployment"></a>Datenkonsistenz für Multi-Rechenzentrums-Bereitstellung
Verteilte Installationen müssen über die Cluster-Topologie Auswirkung auf Durchsatz und hohe Verfügbarkeit. Die RF und Konsistenz müssen so gewählt werden, dass das Quorum auf der Verfügbarkeit der Rechenzentren abhängig ist.
Die hohe System wird LOCAL_QUORUM Konsistenz Ebene (für Lese- und Schreibvorgänge) sicherstellen, dass lokale Lese- und Schreibvorgänge von lokalen Knoten erfüllt sind, während Daten asynchron auf remote-Rechenzentren repliziert werden.  Tabelle 2 fasst die Konfigurationsdetails für Cluster mit mehreren später in das oben beschriebene.

**Zwei Region Cassandra Clusterkonfiguration**


| Clusterparameter | Wert | Beschreibung |
| ----------------- | ----- | ------- |
| Anzahl der Knoten (N) | 8 + 8 | Gesamtanzahl der Knoten im cluster |
| Replikationsfaktor (RF) | 3 | Anzahl der Replikate einer gegebenen Zeile |
| Konsistenz auf (Schreiben) | LOCAL_QUORUM [(sum(RF)/2) +1) = 4] das Ergebnis der Formel wird abgerundet. | 2 Knoten werden zum ersten Rechenzentrum synchron geschrieben. die zusätzlichen 2 Knoten zur Quorumressource werden asynchron zum 2. Rechenzentrum geschrieben. |
| Konsistenz auf (Lesen) | LOCAL_QUORUM ((RF/2) + 1) = 2 das Ergebnis der Formel abgerundet wird. | Leseanfragen sind nur eine Region; 2 Knoten werden gelesen, bevor die Antwort an den Client gesendet wird. |
| Strategie für die Replikation | NetworkTopologyStrategy Siehe [Datenreplikation](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureDataDistributeReplication_c.html) in Cassandra – Dokumentation | Versteht die Bereitstellungstopologie und Replikate in Knoten, dass alle Replikate auf einem Rack geraten |
| Snitch | GossipingPropertyFileSnitch in Cassandra Dokumentation weitere Informationen siehe [Snitches](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureSnitchesAbout_c.html) | NetworkTopologyStrategy verwendet ein Konzept von Snitch die Topologie verstehen. GossipingPropertyFileSnitch bietet eine bessere Steuerung im Rechenzentrum und Rack jedem Knoten zuordnen. Der Cluster verwendet dann Klatsch diese Informationen zu verbreiten. Dies ist viel einfacher, dynamische IP-Einstellung gegenüber PropertyFileSnitch |


##<a name="the-software-configuration"></a>DIE SOFTWAREKONFIGURATION
Die folgenden Softwareversionen werden während der Bereitstellung verwendet:

<table>
<tr><th>Software</th><th>Quelle</th><th>Version</th></tr>
<tr><td>JRE </td><td>[JRE 8](http://www.oracle.com/technetwork/java/javase/downloads/server-jre8-downloads-2133154.html) </td><td>8U5</td></tr>
<tr><td>JNA </td><td>[JNA](https://github.com/twall/jna) </td><td> 3.2.7</td></tr>
<tr><td>Cassandra</td><td>[Apache Cassandra 2.0.8](http://www.apache.org/dist/cassandra/2.0.8/apache-cassandra-2.0.8-bin.tar.gz)</td><td> 2.0.8</td></tr>
<tr><td>Ubuntu  </td><td>[Microsoft Azure](https://azure.microsoft.com/) </td><td>14.04 LTS</td></tr>
</table>

Da manuelle Akzeptanztests Oracle Lizenz herunterladen von JRE benötigt werden, zur Vereinfachung der Bereitstellung herunterladen Sie erforderliche Software auf dem Desktop für später Hochladen in das Vorlagenbild Ubuntu, das wir als Vorläufer der Bereitstellung erstellen

Den oben genannten Software in einem bekannten Download-Verzeichnis (z. B. %TEMP%/downloads unter Windows oder ~/Downloads für die meisten Linux-Distributionen oder Mac) auf dem lokalen Computer herunterladen

### <a name="create-ubuntu-vm"></a>UBUNTU VM ERSTELLEN
In diesem Schritt des Prozesses wird Ubuntu-Image mit der erforderlichen Software erstellt, damit das Bild für die Bereitstellung von mehreren Cassandra Knoten wiederverwendet werden kann.  
####<a name="step-1-generate-ssh-key-pair"></a>Schritt 1: SSH-Schlüsselpaar generieren
Azure benötigt eine öffentliche Schlüssel PEM oder DER Bereitstellung Zeitpunkt codierte X509. Ein öffentliches/privates Schlüsselpaar mit den Anweisungen wie verwendet SSH mit Linux auf Azure zu generieren. Möchten Sie putty.exe als SSH-Client auf Windows oder Linux verwenden, müssen Sie codierte PEM konvertieren privaten RSA-Schlüssel PPK-Format mit puttygen.exe; Anleitung hierfür finden oben Seite.

####<a name="step-2-create-ubuntu-template-vm"></a>Schritt 2: Erstellen von Ubuntu Vorlage VM
Zum Erstellen der Vorlage VM klassischen Azure-Portal anmelden und Folgendes verwenden: Klicken Sie auf neu berechnen, VIRTUAL MACHINE, FROM GALLERY, UBUNTU, Ubuntu Server 14.04 LTS, und klicken Sie dann auf den Pfeil nach rechts. Ein Tutorial beschreibt, wie Linux VM erstellen, finden Sie unter Erstellen einer virtuellen Linux ausgeführt.

Geben Sie folgende Informationen im Fenster "Konfiguration des virtuellen Computers" #1:

<table>
<tr><th>FELDNAME              </td><td>       FELDWERT               </td><td>         BESCHREIBUNG                </td><tr>
<tr><td>VERÖFFENTLICHUNGSDATUM    </td><td> Wählen Sie ein Datum aus der drow</td><td></td><tr>
<tr><td>NAME DES VIRTUELLEN COMPUTERS    </td><td> Cass-Vorlage                </td><td> Dies ist der Hostname des virtuellen Computers </td><tr>
<tr><td>EBENE                     </td><td> STANDARD                        </td><td> Übernehmen Sie die Standardeinstellung              </td><tr>
<tr><td>GRÖßE                     </td><td> A1                              </td><td>Wählen Sie anhand der e/a-Anforderungen VM. zu diesem Zweck Standardwert </td><tr>
<tr><td> NEUER BENUTZERNAME           </td><td> LocalAdmin                      </td><td> "Admin" ist ein reservierter Benutzername in Ubuntu 12. Xx und nach</td><tr>
<tr><td> AUTHENTIFIZIERUNG      </td><td> Klicken Sie auf das Kontrollkästchen                 </td><td>Überprüfen Sie, ob Sie mit einem SSH-Schlüssel sichern möchten </td><tr>
<tr><td> ZERTIFIKAT             </td><td> Dateiname des Zertifikats für öffentlichen Schlüssel </td><td> Verwenden Sie den zuvor erstellten öffentlichen Schlüssel</td><tr>
<tr><td> Neues Kennwort   </td><td> sicheres Kennwort </td><td> </td><tr>
<tr><td> Kennwort bestätigen   </td><td> sicheres Kennwort </td><td></td><tr>
</table>

Geben Sie folgende Informationen im Fenster "Konfiguration des virtuellen Computers" #2:

<table>
<tr><th>FELDNAME             </th><th> FELDWERT                       </th><th> BESCHREIBUNG                                 </th></tr>
<tr><td> CLOUD-DIENST  </td><td> Neue Clouddienst erstellen    </td><td>Cloud-Dienst ist ein Container Serverressourcen wie virtuelle Maschinen</td></tr>
<tr><td> CLOUD-DIENST DNS-NAMEN </td><td>Ubuntu template.cloudapp.net   </td><td>Geben Sie einen Computernamen agnostischen Load balancer</td></tr>
<tr><td> BEREICH GRUPPE/VIRTUELLES NETZWERK </td><td>    Westen der USA </td><td> Wählen Sie einen Bereich aus dem einer Anwendung Cassandra Cluster zugreifen</td></tr>
<tr><td>KONTO </td><td>   Standard verwenden </td><td>Verwenden Sie das Standardkonto Speicher oder vorab erstellten Speicherkonto in einer bestimmten region</td></tr>
<tr><td>FESTLEGEN DER VERFÜGBARKEIT </td><td>  Keine </td><td>  Leer lassen</td></tr>
<tr><td>ENDPUNKTE   </td><td>Standard verwenden </td><td>  Verwenden Sie die SSH-Konfiguration </td></tr>
</table>

Klicken Sie auf rechts, belassen Sie die Standardeinstellungen auf dem Bildschirm #3 und klicken Sie auf "Suchen", um VM Bereitstellung abgeschlossen. Nach einigen Minuten sollte die VM mit dem Namen "Ubuntu-Template" Status "ausführen".

###<a name="install-the-necessary-software"></a>INSTALLIEREN DER ERFORDERLICHEN SOFTWARE
####<a name="step-1-upload-tarballs"></a>Schritt 1: Upload tarballs
Kopieren Sie scp oder Pscp zuvor heruntergeladene Software in ~/downloads-Verzeichnis im folgenden Befehl Format:

#####<a name="pscp-server-jre-8u5-linux-x64targz-localadminhk-cas-templatecloudappnethomelocaladmindownloadsserver-jre-8u5-linux-x64targz"></a>Pscp Server-Jre-8u5-Linux-x64.tar.gzlocaladmin@hk-cas-template.cloudapp.net:/home/localadmin/downloads/server-jre-8u5-linux-x64.tar.gz

Wiederholen Sie den obigen Befehl für JRE sowie für Cassandra Bits.

####<a name="step-2-prepare-the-directory-structure-and-extract-the-archives"></a>Schritt 2: Vorbereiten der Verzeichnisstruktur und Archiv extrahieren
In der VM und die Verzeichnisstruktur erstellen und Software als super-User mit folgenden Skript zu extrahieren:

    #!/bin/bash
    CASS_INSTALL_DIR="/opt/cassandra"
    JRE_INSTALL_DIR="/opt/java"
    CASS_DATA_DIR="/var/lib/cassandra"
    CASS_LOG_DIR="/var/log/cassandra"
    DOWNLOADS_DIR="~/downloads"
    JRE_TARBALL="server-jre-8u5-linux-x64.tar.gz"
    CASS_TARBALL="apache-cassandra-2.0.8-bin.tar.gz"
    SVC_USER="localadmin"

    RESET_ERROR=1
    MKDIR_ERROR=2

    reset_installation ()
    {
       rm -rf $CASS_INSTALL_DIR 2> /dev/null
       rm -rf $JRE_INSTALL_DIR 2> /dev/null
       rm -rf $CASS_DATA_DIR 2> /dev/null
       rm -rf $CASS_LOG_DIR 2> /dev/null
    }
    make_dir ()
    {
       if [ -z "$1" ]
       then
          echo "make_dir: invalid directory name"
          exit $MKDIR_ERROR
       fi

       if [ -d "$1" ]
       then
          echo "make_dir: directory already exists"
          exit $MKDIR_ERROR
       fi

       mkdir $1 2>/dev/null
       if [ $? != 0 ]
       then
          echo "directory creation failed"
          exit $MKDIR_ERROR
       fi
    }

    unzip()
    {
       if [ $# == 2 ]
       then
          tar xzf $1 -C $2
       else
          echo "archive error"
       fi

    }

    if [ -n "$1" ]
    then
       SVC_USER=$1
    fi

    reset_installation
    make_dir $CASS_INSTALL_DIR
    make_dir $JRE_INSTALL_DIR
    make_dir $CASS_DATA_DIR
    make_dir $CASS_LOG_DIR

    #unzip JRE and Cassandra
    unzip $HOME/downloads/$JRE_TARBALL $JRE_INSTALL_DIR
    unzip $HOME/downloads/$CASS_TARBALL $CASS_INSTALL_DIR

    #Change the ownership to the service credentials

    chown -R $SVC_USER:$GROUP $CASS_DATA_DIR
    chown -R $SVC_USER:$GROUP $CASS_LOG_DIR
    echo "edit /etc/profile to add JRE to the PATH"
    echo "installation is complete"


Wenn Sie dieses Skript in Vim-Fenster einfügen, unbedingt Wagenrücklaufzeichen entfernen ("\r") mit dem folgenden Befehl:

    tr -d '\r' <infile.sh >outfile.sh

####<a name="step-3-edit-etcprofile"></a>Schritt 3: Etc-Profil bearbeiten
Fügen Sie am Ende folgende:

    JAVA_HOME=/opt/java/jdk1.8.0_05
    CASS_HOME= /opt/cassandra/apache-cassandra-2.0.8
    PATH=$PATH:$HOME/bin:$JAVA_HOME/bin:$CASS_HOME/bin
    export JAVA_HOME
    export CASS_HOME
    export PATH

####<a name="step-4-install-jna-for-production-systems"></a>Schritt 4: Installieren JNA für Produktionssysteme
Verwenden Sie die folgende Befehlsfolge: der folgende Befehl wird installieren Jna 3.2.7.jar und Jna-Plattform-3.2.7.jar /usr/share.java Verzeichnis Sudo apt-Get Libjna Java

Erstellen Sie symbolische Links in $CASS_HOME/Lib-Verzeichnis, damit Cassandra Startskript diese Gläser finden:

    ln -s /usr/share/java/jna-3.2.7.jar $CASS_HOME/lib/jna.jar

    ln -s /usr/share/java/jna-platform-3.2.7.jar $CASS_HOME/lib/jna-platform.jar

####<a name="step-5-configure-cassandrayaml"></a>Schritt 5: Konfigurieren von cassandra.yaml
Bearbeiten Sie cassandra.yaml für jede VM entsprechend der Konfiguration aller virtuellen Computer [wir werden während der eigentlichen Bereitstellung dieser tweak] erforderlich:

<table>
<tr><th>Feldname   </th><th> Wert  </th><th> Beschreibung </th></tr>
<tr><td>Clustername </td><td>  "Kundendienst"   </td><td> Verwenden Sie den Namen, der die Bereitstellung</td></tr>
<tr><td>listen_address  </td><td>[leer]   </td><td> "Localhost" löschen </td></tr>
<tr><td>rpc_addres   </td><td>[leer]  </td><td> "Localhost" löschen </td></tr>
<tr><td>Samen   </td><td>"10.1.2.4., 10.1.2.6, 10.1.2.8" </td><td>Liste aller IP-Adressen als Basis bezeichnet.</td></tr>
<tr><td>endpoint_snitch </td><td> org.apache.cassandra.locator.GossipingPropertyFileSnitch </td><td> Dies wird durch die NetworkTopologyStrateg zum Herleiten Rechenzentrum und die VM-rack</td></tr>
</table>

####<a name="step-6-capture-the-vm-image"></a>Schritt 6: Aufnahme VM
Melden Sie sich auf dem virtuellen Computer mit dem Hostnamen (Hk-cas-template.cloudapp.net) und SSH privaten Schlüssel zuvor erstellt. Siehe wie verwendet SSH mit Linux auf Azure Informationen mit dem Befehl ssh oder putty.exe anmelden.

Führen Sie die folgende Sequenz von Aktionen auf:
#####<a name="1-deprovision"></a>1. entziehen
Verwenden Sie den Befehl "Sudo Waagent – entziehen + User" virtuelle Instanz computerspezifische Informationen entfernen. Finden Sie unter [wie virtuellen Linux-Maschine](virtual-machines-linux-classic-capture-image.md) als Vorlage Details auf das Bild erfassen.

#####<a name="2-shutdown-the-vm"></a>2: Herunterfahren der virtuellen Computer
Stellen Sie sicher, dass der virtuelle Computer ist, und klicken Sie auf der Befehlsleiste unten Herunterfahren.

#####<a name="3-capture-the-image"></a>3: Aufnahme
Stellen Sie sicher, dass der virtuelle Computer ist, und klicken Sie auf der Befehlsleiste unten erfassen. Geben Sie im nächsten Bildschirm einen IMAGE-Namen (z.B. hk-cas-2-08-ub-14-04-2014071), gegebenenfalls Beschreibung und klicken Sie auf "Häkchen", um den Aufnahmevorgang abgeschlossen.

Dies dauert einige Sekunden, und das Bild soll in Meine Bilder Teil der Bildergalerie. Die Quelle VM Quellcomputer, werden automatisch nach der Aufnahme erfolgreich.

##<a name="single-region-deployment-process"></a>Bereitstellungsprozess für einzelne Region
**Schritt 1: Erstellen des virtuellen Netzwerks** Klassischen Azure-Portal melden Sie an und erstellen Sie ein virtuelles Netzwerk mit den Attributen in der Tabelle. Ausführliche Schritte finden Sie unter [Konfigurieren eines virtuellen Cloud-Only-Netzwerkes im klassischen Azure-Portal](../virtual-network/virtual-networks-create-vnet-classic-portal.md) .      

<table>
<tr><th>VM-Attributnamen</th><th>Wert</th><th>Beschreibung</th></tr>
<tr><td>Name</td><td>Vnet-Cass-West-us</td><td></td></tr>
<tr><td>Region</td><td>Westen der USA</td><td></td></tr>
<tr><td>DNS-Server </td><td>Keine</td><td>Wie wir einen DNS-Server nicht ignorieren</td></tr>
<tr><td>Konfigurieren von Punkt-zu-Standort-VPN</td><td>Keine</td><td> Ignorieren</td></tr>
<tr><td>Konfigurieren eines Standort-zu-Standort-VPN</td><td>Nnone</td><td> Ignorieren</td></tr>
<tr><td>Adressbereich</td><td>10.1.0.0/16</td><td></td></tr>
<tr><td>Start-IP-</td><td>10.1.0.0</td><td></td></tr>
<tr><td>CIDR </td><td>/ 16 (65531)</td><td></td></tr>
</table>

Fügen Sie die folgenden Subnetze:

<table>
<tr><th>Name</th><th>Start-IP-</th><th>CIDR</th><th>Beschreibung</th></tr>
<tr><td>Web</td><td>10.1.1.0</td><td>/ 24 (251)</td><td>Subnetz für die Webfarm</td></tr>
<tr><td>Daten</td><td>10.1.2.0</td><td>/ 24 (251)</td><td>Subnetz für Datenbankknoten</td></tr>
</table>

Daten und Web Subnetze können über netzwerksicherheitsgruppen geschützt werden die Abdeckung der Gültigkeitsbereich für diesen Artikel ist.  

**Schritt 2: Bereitstellung virtueller Computer** Verwenden Sie das zuvor erstellte Image in den Cloudserver "Hk-c-svc-West" die folgenden virtuellen Computer erstellen und Binden an die jeweiligen Subnetze wie folgt:

<table>
<tr><th>Name des Computers    </th><th>Subnetz </th><th>IP-Adresse </th><th>Festlegen der Verfügbarkeit</th><th>DC-Rack</th><th>SEED?</th></tr>
<tr><td>HK-c1-West-us   </td><td>Daten   </td><td>10.1.2.4.   </td><td>HK-c-Aset-1    </td><td>DC = WESTUS Rack = Rack 1 </td><td>Ja</td></tr>
<tr><td>HK-c2-West-us   </td><td>Daten   </td><td>10.1.2.5.   </td><td>HK-c-Aset-1    </td><td>DC = WESTUS Rack = Rack 1 </td><td>Nein </td></tr>
<tr><td>HK-c3-West-us   </td><td>Daten   </td><td>10.1.2.6   </td><td>HK-c-Aset-1    </td><td>DC = WESTUS Rack = rack2 </td><td>Ja</td></tr>
<tr><td>HK-c4-West-us   </td><td>Daten   </td><td>10.1.2.7   </td><td>HK-c-Aset-1    </td><td>DC = WESTUS Rack = rack2 </td><td>Nein </td></tr>
<tr><td>HK-c5-West-us   </td><td>Daten   </td><td>10.1.2.8   </td><td>HK-c-Aset-2    </td><td>DC = WESTUS Rack = rack3 </td><td>Ja</td></tr>
<tr><td>HK-c6-West-us   </td><td>Daten   </td><td>10.1.2.9   </td><td>HK-c-Aset-2    </td><td>DC = WESTUS Rack = rack3 </td><td>Nein </td></tr>
<tr><td>HK-c7-West-us   </td><td>Daten   </td><td>10.1.2.10  </td><td>HK-c-Aset-2    </td><td>DC = WESTUS Rack = rack4 </td><td>Ja</td></tr>
<tr><td>HK-c8-West-us   </td><td>Daten   </td><td>10.1.2.11  </td><td>HK-c-Aset-2    </td><td>DC = WESTUS Rack = rack4 </td><td>Nein </td></tr>
<tr><td>HK-w1-West-us   </td><td>Web    </td><td>10.1.1.4   </td><td>HK-w-Aset-1    </td><td>                       </td><td>N/A</td></tr>
<tr><td>HK-w2-West-us   </td><td>Web    </td><td>10.1.1.5   </td><td>HK-w-Aset-1    </td><td>                       </td><td>N/A</td></tr>
</table>

Die obige Liste der VMs muss den folgenden Prozess:

1.  Erstellen Sie einen leere Cloud-Dienst in einer bestimmten region
2.  Erstellen einer VM aus zuvor aufgenommenen Bilds und das zuvor erstellte virtuelle Netzwerk an; Wiederholen Sie diesen Vorgang für alle virtuellen Computer
3.  Fügen Sie ein interner Lastenausgleich Cloud-Dienst hinzu und Subnetz "Daten" an
4.  Jede VM zuvor erstellte fügen Sie einen Lastenausgleich Endpunkt Sparsamkeit Datenverkehr über einen Lastenausgleich Satz der zuvor erstellten internen Lastenausgleich verbunden hinzu

Das obige Verfahren kann mit klassischen Azure-Portal ausgeführt werden. Windows-Computer (Verwenden einer VM auf Azure haben Sie Zugriff auf einen Windows-Computer) verwenden das folgende PowerShell-Skript 8 VMs automatisch bereitstellen.

**Liste 1: PowerShell-Skript für die Bereitstellung von virtuellen Maschinen**

        #Tested with Azure Powershell - November 2014
        #This powershell script deployes a number of VMs from an existing image inside an Azure region
        #Import your Azure subscription into the current Powershell session before proceeding
        #The process: 1. create Azure Storage account, 2. create virtual network, 3.create the VM template, 2. crate a list of VMs from the template

        #fundamental variables - change these to reflect your subscription
        $country="us"; $region="west"; $vnetName = "your_vnet_name";$storageAccount="your_storage_account"
        $numVMs=8;$prefix = "hk-cass";$ilbIP="your_ilb_ip"
        $subscriptionName = "Azure_subscription_name";
        $vmSize="ExtraSmall"; $imageName="your_linux_image_name"
        $ilbName="ThriftInternalLB"; $thriftEndPoint="ThriftEndPoint"

        #generated variables
        $serviceName = "$prefix-svc-$region-$country"; $azureRegion = "$region $country"

        $vmNames = @()
        for ($i=0; $i -lt $numVMs; $i++)
        {
           $vmNames+=("$prefix-vm"+($i+1) + "-$region-$country" );
        }

        #select an Azure subscription already imported into Powershell session
        Select-AzureSubscription -SubscriptionName $subscriptionName -Current
        Set-AzureSubscription -SubscriptionName $subscriptionName -CurrentStorageAccountName $storageAccount

        #create an empty cloud service
        New-AzureService -ServiceName $serviceName -Label "hkcass$region" -Location $azureRegion
        Write-Host "Created $serviceName"

        $VMList= @()   # stores the list of azure vm configuration objects
        #create the list of VMs
        foreach($vmName in $vmNames)
        {
           $VMList += New-AzureVMConfig -Name $vmName -InstanceSize ExtraSmall -ImageName $imageName |
           Add-AzureProvisioningConfig -Linux -LinuxUser "localadmin" -Password "Local123" |
           Set-AzureSubnet "data"
        }

        New-AzureVM -ServiceName $serviceName -VNetName $vnetName -VMs $VMList

        #Create internal load balancer
        Add-AzureInternalLoadBalancer -ServiceName $serviceName -InternalLoadBalancerName $ilbName -SubnetName "data" -StaticVNetIPAddress "$ilbIP"
        Write-Host "Created $ilbName"
        #Add add the thrift endpoint to the internal load balancer for all the VMs
        foreach($vmName in $vmNames)
        {
            Get-AzureVM -ServiceName $serviceName -Name $vmName |
                Add-AzureEndpoint -Name $thriftEndPoint -LBSetName "ThriftLBSet" -Protocol tcp -LocalPort 9160 -PublicPort 9160 -ProbePort 9160 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ilbName |
                Update-AzureVM

            Write-Host "created $vmName"     
        }

**Schritt 3: Cassandra jedes virtuellen Computers konfigurieren**

Melden Sie den virtuellen Computer und führen Sie folgende Schritte:

* Bearbeiten Sie die $CASS_HOME/conf/cassandra-rackdc.properties die Data Center und Rack-Eigenschaften festlegen:

       dc =EASTUS, rack =rack1

* Bearbeiten Sie cassandra.yaml Saatgut Knoten wie folgt konfigurieren:

       Seeds: "10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10"

**Schritt 4: Starten Sie die VMs zu und Testen Sie des Clusters**

Melden Sie sich bei einem der Knoten (z.B. Hk-c1-West-us) und führen Sie den folgenden Befehl, um den Status des Clusters anzuzeigen:

       nodetool –h 10.1.2.4 –p 7199 status

Sie sollten die ähnlich dem unten für einen Cluster mit 8 Knoten angezeigt:

<table>
<tr><th>Status</th><th>Adresse  </th><th>Laden   </th><th>Token </th><th>Besitzt </th><th>Host-ID  </th><th>Rack</th></tr>
<tr><th>AUFHEBEN  </td><td>10.1.2.4.   </td><td>87.81 KB   </td><td>256    </td><td>38,0 %  </td><td>GUID (entfernt)</td><td>Rack 1</td></tr>
<tr><th>AUFHEBEN  </td><td>10.1.2.5.   </td><td>41.08 KB   </td><td>256    </td><td>68,9 %  </td><td>GUID (entfernt)</td><td>Rack 1</td></tr>
<tr><th>AUFHEBEN  </td><td>10.1.2.6   </td><td>55.29 KB   </td><td>256    </td><td>68,8 %  </td><td>GUID (entfernt)</td><td>rack2</td></tr>
<tr><th>AUFHEBEN  </td><td>10.1.2.7   </td><td>55.29 KB   </td><td>256    </td><td>68,8 %  </td><td>GUID (entfernt)</td><td>rack2</td></tr>
<tr><th>AUFHEBEN  </td><td>10.1.2.8   </td><td>55.29 KB   </td><td>256    </td><td>68,8 %  </td><td>GUID (entfernt)</td><td>rack3</td></tr>
<tr><th>AUFHEBEN  </td><td>10.1.2.9   </td><td>55.29 KB   </td><td>256    </td><td>68,8 %  </td><td>GUID (entfernt)</td><td>rack3</td></tr>
<tr><th>AUFHEBEN  </td><td>10.1.2.10  </td><td>55.29 KB   </td><td>256    </td><td>68,8 %  </td><td>GUID (entfernt)</td><td>rack4</td></tr>
<tr><th>AUFHEBEN  </td><td>10.1.2.11  </td><td>55.29 KB   </td><td>256    </td><td>68,8 %  </td><td>GUID (entfernt)</td><td>rack4</td></tr>
</table>

## <a name="test-the-single-region-cluster"></a>Testen des einzelnen Bereich Clusters
Gehen Sie folgendermaßen vor, um den Cluster zu testen:

1.    Abrufen Sie die IP-Adresse des internen Systems zum Lastenausgleich mit Powershell Befehl Get-AzureInternalLoadbalancer-Cmdlet, (z.B.  10.1.2.101). Die Syntax des Befehls unten: Get-AzureLoadbalancer-ServiceName "Hk-c-svc-West-us" [zeigt die Details des internen Lastenausgleich mit der IP-Adresse]
2.  Melden Sie sich bei der Webfarm VM (z.B. Hk-w1-West-us) mit kitten oder ssh
3.  $CASS_HOME/Bin/Cqlsh 10.1.2.101 ausführen 9160
4.  Verwenden Sie die folgenden CQL Befehle um zu überprüfen, ob der Cluster funktionsfähig ist:

        CREATE KEYSPACE customers_ks WITH REPLICATION = { 'class' : 'SimpleStrategy', 'replication_factor' : 3 };
        USE customers_ks;
        CREATE TABLE Customers(customer_id int PRIMARY KEY, firstname text, lastname text);
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES (2, 'Jane', 'Doe');

        SELECT * FROM Customers;

Sie sollten angezeigt wie dem folgenden:

<table>
  <tr><th> customer_id </th><th> Vorname </th><th> Nachname </th></tr>
  <tr><td> 1 </td><td> John </td><td> Doe </td></tr>
  <tr><td> 2 </td><td> Jane </td><td> Doe </td></tr>
</table>

Beachten Sie, dass in Schritt 4 erstellte nur SimpleStrategy mit einer replikationsfaktor von 3 verwendet. SimpleStrategy für einzelne Daten Datenzentren empfiehlt während NetworkTopologyStrategy für Multi-Data center Bereitstellungen. Replikationsfaktor 3 Geben Toleranz für Knotenausfälle.

##<a id="tworegion"> </a>Mit mehreren Bereitstellung
Wird die Bereitstellung in einer Region abgeschlossen nutzen und Wiederholen derselben zum zweiten Bereich installieren. Der Hauptunterschied zwischen der Bereitstellung einzelner und mehrerer Region ist VPN-Tunnel einrichten für die Kommunikation zwischen der Region. Wir beginnen mit der Netzwerkinstallation, Bereitstellung von VMs und Cassandra konfigurieren.

###<a name="step-1-create-the-virtual-network-at-the-2nd-region"></a>Schritt 1: Erstellen des virtuellen Netzwerks 2. Region
Klassischen Azure-Portal melden Sie an und erstellen Sie ein virtuelles Netzwerk mit den Attributen in der Tabelle. Ausführliche Schritte finden Sie unter [Konfigurieren eines virtuellen Cloud-Only-Netzwerkes im klassischen Azure-Portal](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) .      

<table>
<tr><th>Attributname    </th><th>Wert    </th><th>Beschreibung</th></tr>
<tr><td>Name    </td><td>Vnet-Cass-OST-us</td><td></td></tr>
<tr><td>Region  </td><td>Osten der USA</td><td></td></tr>
<tr><td>DNS-Server     </td><td></td><td>Wie wir einen DNS-Server nicht ignorieren</td></tr>
<tr><td>Konfigurieren von Punkt-zu-Standort-VPN</td><td></td><td>     Ignorieren</td></tr>
<tr><td>Konfigurieren eines Standort-zu-Standort-VPN</td><td></td><td>      Ignorieren</td></tr>
<tr><td>Adressbereich   </td><td>10.2.0.0/16</td><td></td></tr>
<tr><td>Start-IP- </td><td>10.2.0.0   </td><td></td></tr>
<tr><td>CIDR    </td><td>/ 16 (65531)</td><td></td></tr>
</table>

Fügen Sie die folgenden Subnetze:
<table>
<tr><th>Name    </th><th>Start-IP-    </th><th>CIDR   </th><th>Beschreibung</th></tr>
<tr><td>Web </td><td>10.2.1.0   </td><td>/ 24 (251)  </td><td>Subnetz für die Webfarm</td></tr>
<tr><td>Daten    </td><td>10.2.2.0   </td><td>/ 24 (251)  </td><td>Subnetz für Datenbankknoten</td></tr>
</table>


###<a name="step-2-create-local-networks"></a>Schritt 2: Erstellen von lokalen Netzwerken
Ein lokales Netzwerk in Azure virtuellen Netzwerken ist ein Proxy-Adressraum, der remote-Standort einschließlich einer privaten Cloud oder anderen Azure Region zugeordnet ist. Dieser Proxy-Adressraum ist einem Remotegateway für routing-Netzwerk zu den richtigen Netzwerk gebunden. Auf VNET VNET Verbindung finden Sie unter [Konfigurieren einer VNet VNet Verbindung](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) .

Erstellen Sie zwei lokale Netzwerke pro Folgendes:

| Netzwerkname | VPN-Gateway-Adresse | Adressbereich | Beschreibung |
| ------------ | ------------------- | ------------- | ------- |
| HK-lnet-Map-to-East-US | 23.1.1.1  | 10.2.0.0/16   | Das lokale Netzwerk geben Sie einen Platzhalter-Gateway-Adresse. Echte Gatewayadresse wird gefüllt, erstellte das Gateway. Sicherstellen Sie, dass Adressraum das jeweiligen entfernte VNET übereinstimmt. In diesem Fall erstellt das VNET in der Region USA OST. |
| HK-lnet-Map-to-West-US | 23.2.2.2  | 10.1.0.0/16   | Das lokale Netzwerk geben Sie einen Platzhalter-Gateway-Adresse. Echte Gatewayadresse wird gefüllt, erstellte das Gateway. Sicherstellen Sie, dass Adressraum das jeweiligen entfernte VNET übereinstimmt. In diesem Fall erstellt das VNET in der Region West USA. |


###<a name="step-3-map-local-network-to-the-respective-vnets"></a>Schritt 3: "Local" Netzlaufwerk zum jeweiligen VNETs
Wählen Sie aus dem Azure-Verwaltungsportal jedes Vnet, klicken Sie auf "Konfigurieren" Überprüfen Sie "Verbindung mit dem lokalen Netzwerk" und wählen Sie die lokale Netzwerke pro Folgendes:


| Virtuelles Netzwerk | Lokalen Netzwerk |
| --------------- | ------------- |
| HK-Vnet-West-us | HK-lnet-Map-to-East-US |
| HK-Vnet-OST-us | HK-lnet-Map-to-West-US |


###<a name="step-4-create-gateways-on-vnet1-and-vnet2"></a>Schritt 4: Erstellen von Gateways auf VNET1 und VNET2
Klicken Sie auf Dashboard der virtuellen Netzwerke GATEWAY erstellen die VPN-Gateway Bereitstellung auslösen. Nach einigen Minuten sollte die tatsächliche Gatewayadresse Dashboard jedes virtuellen Netzwerk angezeigt werden.

###<a name="step-5-update-local-networks-with-the-respective-gateway-addresses"></a>Schritt 5: Update "Local" Netzwerke mit den Adressen der jeweiligen "Gateway"###
Bearbeiten Sie die LANs um Platzhalter Gateway IP-Adresse durch die tatsächliche IP-Adresse nur bereitgestellten Gateways zu ersetzen. Verwenden Sie die folgende Zuordnung:

<table>
<tr><th>Lokalen Netzwerk    </th><th>Virtuelle Netzwerk-Gateway</th></tr>
<tr><td>HK-lnet-Map-to-East-US </td><td>Hk-Vnet-West-us-Gateway</td></tr>
<tr><td>HK-lnet-Map-to-West-US </td><td>Hk-Vnet-OST-uns-Gateway</td></tr>
</table>

###<a name="step-6-update-the-shared-key"></a>Schritt 6: Aktualisieren Sie den freigegebenen Schlüssel
Verwenden Sie das folgende Powershell-Skript IPSec-Schlüssel der einzelnen VPN-Gateway [Willen Schlüssel für die Gateways verwenden] aktualisiert: Set-AzureVNetGatewayKey - VNetName-Hk-Vnet-OST-us - LocalNetworkSiteName Hk-lnet-map-to-west-us - SharedKey D9E76BKK Set-AzureVNetGatewayKey - VNetName-Hk-Vnet-West-us - LocalNetworkSiteName Hk-lnet-map-to-east-us - SharedKey D9E76BKK

###<a name="step-7-establish-the-vnet-to-vnet-connection"></a>Schritt 7: Die VNET VNET Verbindung
Aus dem Azure-Verwaltungsportal mithilfe des Menüs "DASHBOARD" virtuelle Netzwerke Gateway-zu-Gateway-Verbindung. Verwenden Sie die Menüelemente "CONNECT" auf der unteren. Nach einigen Minuten sollte das Dashboard die Verbindungsdetails grafisch dar.

###<a name="step-8-create-the-virtual-machines-in-region-2"></a>Schritt 8: Erstellen der virtuellen Computer im Bereich #2
Erstellen Sie Bild Ubuntu gemäß Region #1 Bereitstellung die gleichen Schritte oder Kopie VHD Bilddatei Azure Storage-Konto im Bereich #2 und das Bild. Dieses Bild und die folgenden virtuellen Computer in eine neue Cloud-Dienst erstellen Hk-c-svc-OST-uns:


| Name des Computers | Subnetz | IP-Adresse | Festlegen der Verfügbarkeit | DC-Rack | SEED? |
| ------------ | ------ | ---------- | ---------------- | ------- | ----- |
| HK-c1-OST-us | Daten  | 10.2.2.4   | HK-c-Aset-1      | DC = EASTUS Rack = Rack 1 | Ja |
| HK-c2-OST-us | Daten  | 10.2.2.5   | HK-c-Aset-1      | DC = EASTUS Rack = Rack 1 | Nein  |
| HK-c3-OST-us | Daten  | 10.2.2.6   | HK-c-Aset-1      | DC = EASTUS Rack = rack2 | Ja |
| HK-c5-OST-us | Daten  | 10.2.2.8   | HK-c-Aset-2      | DC = EASTUS Rack = rack3 | Ja |
| HK-c6-OST-us | Daten  | 10.2.2.9   | HK-c-Aset-2      | DC = EASTUS Rack = rack3 | Nein  |
| HK-c7-OST-us | Daten  | 10.2.2.10  | HK-c-Aset-2      | DC = EASTUS Rack = rack4 | Ja |
| HK-c8-OST-us | Daten  | 10.2.2.11  | HK-c-Aset-2      | DC = EASTUS Rack = rack4 | Nein  |
| HK-w1-OST-us | Web   | 10.2.1.4   | HK-w-Aset-1      | N/A                    | N/A |
| HK-w2-OST-us | Web   | 10.2.1.5   | HK-w-Aset-1      | N/A                    | N/A |


Gehen Sie als Bereich #1, aber verwenden Sie 10.2.xxx.xxx Adressraum.

###<a name="step-9-configure-cassandra-on-each-vm"></a>Schritt 9: Cassandra jedes virtuellen Computers konfigurieren
Melden Sie den virtuellen Computer und führen Sie folgende Schritte:

1. Bearbeiten Sie $CASS_HOME/conf/cassandra-rackdc.properties um die Data Center und Rack im Format anzugeben: dc = EASTUS Rack = Rack 1
2. Bearbeiten cassandra.yaml Saatgut Knoten konfigurieren: Samen: "10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10,10.2.2.4,10.2.2.6,10.2.2.8,10.2.2.10"

###<a name="step-10-start-cassandra"></a>Schritt 10: Cassandra starten
Jede VM anmelden und Cassandra im Hintergrund starten, indem Sie den folgenden Befehl ausführen: $CASS_HOME/Bin/Cassandra

## <a name="test-the-multi-region-cluster"></a>Testen Sie den Cluster mit mehreren
Jetzt wurde Cassandra 16 Knoten mit 8 Knoten in jeder Region Azure bereitgestellt. Diese Knoten werden in demselben Cluster gemeinsamen Clusternamen und Konfiguration Knoten Saatgut. Verwenden Sie folgende Verfahren zum Testen des Clusters:

###<a name="step-1-get-the-internal-load-balancer-ip-for-both-the-regions-using-powershell"></a>Schritt 1: Abrufen Sie interne Load Balancer IP für beide Regionen mit PowerShell
- Get-AzureInternalLoadbalancer - ServiceName "Hk-c-svc-West-us"
- Get-AzureInternalLoadbalancer - ServiceName "Hk-c-svc-OST-us"  

    Beachten Sie die IP-Adressen (z. B. - 10.1.2.101, Ost - West 10.2.2.101) angezeigt.

###<a name="step-2-execute-the-following-in-the-west-region-after-logging-into-hk-w1-west-us"></a>Schritt 2: Führen Sie Folgendes in der Region West nach Anmeldung Hk-w1-West-us
1.    $CASS_HOME/Bin/Cqlsh 10.1.2.101 ausführen 9160
2.  Führen Sie die folgenden CQL-Befehle:

        CREATE KEYSPACE customers_ks
        WITH REPLICATION = { 'class' : 'NetworkToplogyStrategy', 'WESTUS' : 3, 'EASTUS' : 3};
        USE customers_ks;
        CREATE TABLE Customers(customer_id int PRIMARY KEY, firstname text, lastname text);
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES (2, 'Jane', 'Doe');
        SELECT * FROM Customers;

Sie sollten angezeigt wie dem folgenden:

| customer_id | Vorname | Nachname |
| ----------- | --------- | -------- |
| 1           | John      | Doe      |
| 2           | Jane      | Doe      |


###<a name="step-3-execute-the-following-in-the-east-region-after-logging-into-hk-w1-east-us"></a>Schritt 3: Führen Sie Folgendes in der Region OST nach Anmeldung Hk-w1-OST-uns:
1.    $CASS_HOME/Bin/Cqlsh 10.2.2.101 ausführen 9160
2.  Führen Sie die folgenden CQL-Befehle:

        USE customers_ks;
        CREATE TABLE Customers(customer_id int PRIMARY KEY, firstname text, lastname text);
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES (2, 'Jane', 'Doe');
        SELECT * FROM Customers;

Die gleiche Anzeige sollte wie für die Region West angezeigt werden:


| customer_id | Vorname | Nachname   |
|------------ | --------- | ---------- |
| 1           | John      | Doe        |
| 2           | Jane      | Doe        |


Einige weitere fügt ausführen und feststellen, dass die nach Westen repliziert-uns Teil des Clusters.

## <a name="test-cassandra-cluster-from-nodejs"></a>Cassandra Cluster Node.js Test
Mit einem Linux VMs "Web" erstellten Stufe zuvor, führen wir ein einfaches Node.js-Skript zum Lesen von zuvor Daten

**Schritt 1: Installieren von Node.js und Cassandra Client**

1. Installieren von Node.js und npm
2. Knoten Paket "Cassandra Client-Installation mit Npm
3. Führen Sie das folgende Skript an die Shell die Json-Zeichenfolge der abgerufenen Daten angezeigt:

        var pooledCon = require('cassandra-client').PooledConnection;
        var ksName = "custsupport_ks";
        var cfName = "customers_cf";
        var hostList = ['internal_loadbalancer_ip:9160'];
        var ksConOptions = { hosts: hostList,
                             keyspace: ksName, use_bigints: false };

        function createKeyspace(callback){
           var cql = 'CREATE KEYSPACE ' + ksName + ' WITH strategy_class=SimpleStrategy AND strategy_options:replication_factor=1';
           var sysConOptions = { hosts: hostList,  
                                 keyspace: 'system', use_bigints: false };
           var con = new pooledCon(sysConOptions);
           con.execute(cql,[],function(err) {
           if (err) {
             console.log("Failed to create Keyspace: " + ksName);
             console.log(err);
           }
           else {
             console.log("Created Keyspace: " + ksName);
             callback(ksConOptions, populateCustomerData);
           }
           });
           con.shutdown();
        }

        function createColumnFamily(ksConOptions, callback){
          var params = ['customers_cf','custid','varint','custname',
                        'text','custaddress','text'];
          var cql = 'CREATE COLUMNFAMILY ? (? ? PRIMARY KEY,? ?, ? ?)';
        var con =  new pooledCon(ksConOptions);
          con.execute(cql,params,function(err) {
              if (err) {
                 console.log("Failed to create column family: " + params[0]);
                 console.log(err);
              }
              else {
                 console.log("Created column family: " + params[0]);
                 callback();
              }
          });
          con.shutdown();
        }

        //populate Data
        function populateCustomerData() {
           var params = ['John','Infinity Dr, TX', 1];
           updateCustomer(ksConOptions,params);

           params = ['Tom','Fermat Ln, WA', 2];
           updateCustomer(ksConOptions,params);
        }

        //update will also insert the record if none exists
        function updateCustomer(ksConOptions,params)
        {
          var cql = 'UPDATE customers_cf SET custname=?,custaddress=? where custid=?';
          var con = new pooledCon(ksConOptions);
          con.execute(cql,params,function(err) {
              if (err) console.log(err);
              else console.log("Inserted customer : " + params[0]);
          });
          con.shutdown();
        }

        //read the two rows inserted above
        function readCustomer(ksConOptions)
        {
          var cql = 'SELECT * FROM customers_cf WHERE custid IN (1,2)';
          var con = new pooledCon(ksConOptions);
          con.execute(cql,[],function(err,rows) {
              if (err)
                 console.log(err);
              else
                 for (var i=0; i<rows.length; i++)
                    console.log(JSON.stringify(rows[i]));
            });
           con.shutdown();
        }

        //exectue the code
        createKeyspace(createColumnFamily);
        readCustomer(ksConOptions)


## <a name="conclusion"></a>Abschluss
Microsoft Azure ist eine flexible Plattform, die Ausführung von sowohl Microsoft als auch open Source-Software wie dieser Übung. Hochverfügbare Cassandra Clustern können auf einem einzelnen Data Center über die Verbreitung der Clusterknoten über mehrere Fehlerdomänen bereitgestellt. Cassandra Cluster können auch in mehreren geografisch entfernten Azure Regionen Disaster zukunftssichere Systeme bereitgestellt werden. Azure und Cassandra zusammen können die hochgradig skalierbare, hochverfügbare und Notfall wiederhergestellt Clouddienste benötigt heutige Internet skalieren Services.  

##<a name="references"></a>Referenzen##
- [http://Cassandra.apache.org](http://cassandra.apache.org)
- [http://www.datastax.com](http://www.datastax.com)
- [http://www.nodejs.org](http://www.nodejs.org)
