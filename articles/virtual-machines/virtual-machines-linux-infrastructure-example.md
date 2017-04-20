<properties
    pageTitle="Exemplarische Vorgehensweise Infrastrukturen | Microsoft Azure"
    description="Erfahren Sie mehr über wichtigen Entwurf und Implementierung von Richtlinien für die Bereitstellung einer Infrastruktur wird in Azure."
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="example-azure-infrastructure-walkthrough"></a>Exemplarische Vorgehensweise Azure-Infrastruktur

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

Dieser Artikel führt durch Erstellen einer Beispiel-Anwendungsinfrastruktur. Entwerfen einer Infrastruktur für einen einfachen Online-Store, der die Richtlinien und Beschlüsse Namenskonventionen und Verfügbarkeit legt, virtuelle Netzwerke Lastenausgleich zusammenführt und Bereitstellung der virtuellen Maschinen (VMs) erläutert.


## <a name="example-workload"></a>Beispiel-Arbeitslast

Adventure Works Cycles will eine Online-Store-Anwendung in Azure, die aus:

- Zwei Nginx Server in eine Webebene Front-End-client
- Zwei Nginx Server verarbeitet Daten und Aufträge in einer Anwendungsebene
- Zwei MongoDB Server Teil eines Clusters Sharding zum Speichern von Daten und Aufträge in eine Datenbankebene
- Zwei Active Directory-Domänencontroller für Kunden und Lieferanten in eine Ebene Authentifizierung
- Die Server befinden sich zwei Subnetzen:
    - Ein Front-End-Subnetz für Webserver 
    - ein Back-End-Subnetz für Anwendungsserver, MongoDB Cluster und Domänencontroller

![Diagramm der verschiedenen Ebenen für Infrastruktur](./media/virtual-machines-common-infrastructure-service-guidelines/example-tiers.png)

Eingehende sicherer Webverkehr muss werden Lastenausgleich zwischen den Webservern Kunden Online Store durchsuchen. Auftragsabwicklung Verkehr in Form von HTTP fordert von der Web-Server Application Server Lastenausgleich müssen werden. Außerdem muss die Infrastruktur für hohe Verfügbarkeit konzipiert.

Das resultierende Design muss enthalten:

- Ein Azure-Abonnement und Konto
- Einer einzelnen Ressourcengruppen
- Speicherkonten
- Ein virtuelles Netzwerk mit zwei Subnetzen
- Verfügbarkeit wird für die virtuellen Computer in einer ähnlichen Rolle
- Virtuelle Computer

Alle genannten entsprechen diesen Namenskonventionen:

- Adventure Works Cycles verwendet **[IT-Arbeitslast]-[Ort]-[Azure Ressource]** als Präfix
    - In diesem Beispiel "**Azos**" (Azure Online Store) der IT Arbeitslastname und "**verwenden**" (USA Osten 2) befindet
- Speicherkonten verwenden Adventureazosusesa**[Beschreibung]**
    - "Adventure" wurde hinzugefügt, um das Präfix für die Eindeutigkeit und Kontonamen Speicher unterstützen nicht die Verwendung von Bindestrichen.
- Virtuelle Netzwerke verwenden AZOS mit VN**[Anzahl]**
- Verfügbarkeit verwenden Azos-verwendet-als-**[Funktion]**
- Virtual Machine verwenden Azos-verwendet-Vm -**[Vmname]**


## <a name="azure-subscriptions-and-accounts"></a>Azure-Abonnements und Konten

Adventure Works Cycles verwendet Abonnementzeitraums Unternehmen mit dem Namen Adventure Works Enterprise Abonnement zu Abrechnung IT erfolgen.


## <a name="storage-accounts"></a>Speicherkonten

Adventure Works Cycles festgestellt, dass sie zwei Speicherkonten erforderlich:

- **Adventureazosusesawebapp** für Standardspeicher Webserver, Anwendungsserver, Domänencontroller und ihre Datenträger.
- **Adventureazosusesadbclust** für den Premium-Speicher MongoDB Sharding Clusterserver und ihre Datenträger.


## <a name="virtual-network-and-subnets"></a>Virtuelles Netzwerk und Subnetze

Da das virtuelle Netzwerk nicht kontinuierliche Verbindung zum Netzwerk lokalen Adventure Arbeitsabläufe, entschieden Sie sich für einen Cloud-virtuelle Netzwerk.

Sie erstellt ein virtuelles Netzwerk Cloud nur mit Azure-Portal mit folgenden Optionen:

- Name: AZOS-mit-VN01
- Ort: East USA 2
- Virtuelles Netzwerk-Adressraum: 10.0.0.0/8
- Erste Subnetz:
    - Name: Front-End
    - Adressraum: 10.0.1.0/24
- Zweite Subnetz:
    - Name: Back-End
    - Adressraum: 10.0.2.0/24


## <a name="availability-sets"></a>Verfügbarkeit-sets

Um alle vier Ebenen des Online-Shop hoher Verfügbarkeit beschlossen Adventure Works Cycles auf vier Verfügbarkeit:

- **Azos Verwendung als Webseite** für Webserver
- **Azos Verwendung als app** für den Anwendungsserver
- **Azos Verwendung als Db** für Server Clusters MongoDB Sharding
- **Azos Verwendung als Domänencontroller** für die Domänencontroller


## <a name="virtual-machines"></a>Virtuelle Computer

Adventure Works Cycles hat den folgenden Namen für ihre Azure-VMs:

- **Azos Verwendung-Vm web01** für den ersten Webserver
- **Azos Verwendung-Vm web02** für den zweiten Webserver
- **Azos Verwendung-Vm app01** für den ersten Anwendungsserver
- **Azos-verwendet-Vm-app02** für den zweiten Anwendungsserver
- **Azos Verwendung-Vm db01** für den ersten MongoDB Server im cluster
- **Azos Verwendung-Vm db02** für den zweiten MongoDB Server im cluster
- **Azos Verwendung-Vm dc01** für den ersten Domänencontroller
- **Azos-verwendet-Vm-dc02** für den zweiten Domänencontroller

Hier ist die resultierende Konfiguration.

![Anwendungsinfrastruktur in Azure bereitgestellt](./media/virtual-machines-common-infrastructure-service-guidelines/example-config.png)

Diese Konfiguration umfasst:

- Nur Cloud virtuelles Netzwerk mit zwei Subnetzen (Front-End- und Back-End)
- Zwei Storage-Konten
- Vier Verfügbarkeit legt für jede Ebene der Online-Shop
- Die virtuellen Computer für die vier Ebenen
- Ein externer Lastenausgleich Satz für HTTPS-basierten Web-Datenverkehr aus dem Internet auf den Webserver
- Ein interner Lastenausgleich für unverschlüsselte Webdatenverkehr vom Webserver auf den Anwendungsservern eingerichtet
- Einer einzelnen Ressourcengruppen


## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 