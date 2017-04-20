<properties 
   pageTitle="Zwischen zwei Azure virtuelle Netzwerke konfigurieren | Microsoft Azure" 
   description="Informationen zum Konfigurieren von VPN-Verbindungen und Auflösung des Domänennamens zwischen zwei virtuelle Netzwerke und HBase Geo-Replikation konfigurieren." 
   services="hdinsight,virtual-network" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="06/28/2016"
   ms.author="jgao"/>

# <a name="configure-dns-between-two-azure-virtual-networks"></a>Zwischen zwei Azure virtuelle Netzwerke konfigurieren

> [AZURE.SELECTOR]
- [Konfigurieren von VPN-Konnektivität](../hdinsight-hbase-geo-replication-configure-vnets.md)
- [Konfigurieren von DNS](hdinsight-hbase-geo-replication-configure-dns.md)
- [HBase Replikation](hdinsight-hbase-geo-replication.md) 


Informationen Sie zum Hinzufügen und Konfigurieren von DNS-Servern Azure virtuelle Netzwerke Auflösung innerhalb und über virtuelle Netzwerke behandelt.

Dieses Lernprogramm ist der zweite Teil der [Serie] [ hdinsight-hbase-geo-replication] auf HBase Geo-Replikation:

- [Konfigurieren einer VPN-Verbindung zwischen zwei virtuelle Netzwerke][hdinsight-hbase-geo-replication-vnet]
- Konfigurieren von DNS für die virtuellen Netzwerke (dieses Lernprogramm)
- [HBase Geo Replikation][hdinsight-hbase-geo-replication]


Das folgende Diagramm veranschaulicht die beiden virtuellen Netzwerke unter [Konfigurieren einer VPN-Verbindung zwischen zwei virtuelle Netzwerke]erstellt[hdinsight-hbase-geo-replication-vnet]:

![HDInsight HBase Replikation virtuelles Netzwerkdiagramm][img-vnet-diagram]

##<a name="prerequisites"></a>Erforderliche Komponenten
Bevor Sie dieses Lernprogramm beginnen, müssen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Eine Workstation mit Azure PowerShell**.

    Stellen Sie bevor PowerShell-Skripts sicher, dass Sie Ihre Azure-Abonnement mit dem folgenden Cmdlet verbunden sind:

        Add-AzureAccount

    Haben mehrere Azure-Abonnements mit dem folgenden Cmdlet das aktuelle Abonnement fest:

        Select-AzureSubscription <AzureSubscriptionName>
        
    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Zwei Azure virtuelles Netzwerk mit VPN-Konnektivität**.  Eine Anleitung finden Sie [konfigurieren eine VPN-Verbindung zwischen zwei virtuellen Netzwerken Azure][hdinsight-hbase-geo-replication-vnet].

>[AZURE.NOTE] Azure Service und virtuellen Namen müssen eindeutig sein. In diesem Lernprogramm verwendeten Namen ist Contoso [Azure Service/VM-Name]-[EU / US]. Beispielsweise ist Contoso-VNet-EU Azure virtuelles Netzwerk im Rechenzentrum Nordeuropa. Contoso-DNS-US ist der DNS-Server VM im südostasiatischen US-Rechenzentrum. Sie müssen mit Ihrem eigenen Namen kommen.
 
 
##<a name="create-azure-virtual-machines-to-be-used-as-dns-servers"></a>Maschinen Sie Azure virtuelle als DNS-Server verwendet werden

**Erstellen eines virtuellen Computers in Contoso-VNet-EU mit dem Namen Contoso-DNS-EU**

1.  Klicken Sie auf **neu** **berechnen**, **VIRTUAL MACHINE**, **FROM GALLERY**.
2.  Wählen Sie **Windows Server 2012 R2 Datacenter**.
3.  Geben Sie ein:
    - **NAME des virtuellen Computers**: Contoso-DNS-EU
    - **Neuer Benutzername**: 
    - **Neues Kennwort**: 
4.  Geben Sie ein:
    - **CLOUD-Dienst**: neuen Clouddienst erstellen
    - **Bereich Gruppe/VIRTUAL NETWORK**: (Wählen Sie Contoso-VNet-EU)
    - **Virtuelle Netzwerk-SUBNETZE**: Subnetz 1
    - **Konto**: eine automatisch generierte Speicherkonto verwenden
    
        Cloud Service Name wird als Name des virtuellen Computers übereinstimmen. In diesem Fall ist das Contoso-DNS-EU. Für nachfolgende virtuelle Maschinen können ich den gleichen Clouddienst verwenden.  Alle virtuellen Computer unter denselben Clouddienst nutzen gleichen virtuellen Netzwerk und Domänensuffix.

        Das Speicherkonto zum virtuellen Computer Image-Datei speichern. 
    - **ENDPUNKTE**: (Scrollen Sie nach unten und wählen Sie **DNS**) 

Nach dem Erstellen der virtuellen Maschine herausfinden der IP- und externe IP-Adresse.

1.  Klicken Sie auf den virtuellen Namen **Contoso-DNS-EU**.
2.  Klicken Sie auf **DashBoard**.
3.  Notieren Sie sich:
    - ÖFFENTLICHE VIRTUELLE IP-ADRESSE
    - INTERNE IP-ADRESSE


**Erstellen eines virtuellen Computers in Contoso-VNet-US mit dem Namen Contoso-DNS-US** 

- Wiederholen Sie den Vorgang zum Erstellen eines virtuellen Computers mit folgenden Werten:
    - NAME des virtuellen Computers: Contoso-DNS-US
    - REGION/Gruppe VIRTUAL NETWORK: Contoso-VNET-Deutschland
    - VIRTUELLE Netzwerk-SUBNETZE: Subnetz 1
    - SPEICHERKONTO: Verwenden Sie eine automatisch generierte Speicherkonto
    - ENDPUNKTE: (DNS aktivieren)

##<a name="set-static-ip-addresses-for-the-two-virtual-machines"></a>Adressen Sie statische IP-für zwei virtuelle Computer

DNS-Server erfordert statische IP-Adressen.  Dieser Schritt kann nicht aus dem Azure-Verwaltungsportal erfolgen. Azure PowerShell verwendet.

**Statische IP-Adresse für zwei virtuelle Computer konfigurieren**

1. Öffnen Sie Windows PowerShell ISE.
2. Führen Sie die folgenden Cmdlets.  

        Add-AzureAccount
        Select-AzureSubscription [YourAzureSubscriptionName]
        
        Get-AzureVM -ServiceName Contoso-DNS-EU -Name Contoso-DNS-EU | Set-AzureStaticVNetIP -IPAddress 10.1.0.4 | Update-AzureVM
        Get-AzureVM -ServiceName Contoso-DNS-US -Name Contoso-DNS-US | Set-AzureStaticVNetIP -IPAddress 10.2.0.4 | Update-AzureVM 

    Dienstname ist der Name Cloud. Ist der DNS-Server den ersten virtuellen Computer der Cloud-Dienst entspricht der Cloud-Dienstname Name des virtuellen Computers.

    Sie müssen update Dienstname und Name die Namen entspricht.


##<a name="add-the-dns-server-role-the-two-virtual-machines"></a>Hinzufügen von DNS-Server-Rolle die beiden virtuellen Computern

**Hinzufügen von DNS-Serverrolle für Contoso-DNS-EU**

1.  Azure-Verwaltungsportal klicken Sie auf **virtuelle Computer** auf der linken Seite. 
2.  Klicken Sie auf **Contoso-DNS-EU**.
3.  Klicken Sie oben auf **DASHBOARD** .
4.  Klicken Sie auf **Verbinden** von unten und folgen Sie der Anleitung für die Verbindung zum virtuellen Computer über RDP.
2.  Innerhalb der RDP-Sitzung klicken Sie Windows auf der unteren linken Ecke die Startseite geöffnet.
3.  Klicken Sie auf die Kachel **Server Manager** .
4.  Klicken Sie auf **Rollen und Features**.
5.  Klicken Sie auf **Weiter**
6.  Wählen Sie **Rolle oder Funktion-basierten Installation**und klicken Sie dann auf **Weiter**.
7.  Wählen Sie DNS virtuellen Computers (es sind bereits markiert) und klicken Sie dann auf **Weiter**.
8.  Überprüfen Sie die **DNS-Server**.
9.  Klicken Sie auf **Features hinzufügen**, und klicken Sie dann auf **Weiter**.
10. Klicken Sie dreimal auf **Weiter** , und klicken Sie dann auf **Installieren**. 

**DNS-Serverrolle für Contoso-DNS-US hinzufügen**

- Wiederholen Sie **Contoso-DNS-US**DNS-Rolle hinzufügen.

##<a name="assign-dns-servers-to-the-virtual-networks"></a>Virtuelle Netzwerke DNS-Server zuweisen

**Zwei DNS-Server registrieren**

1.  Azure-Verwaltungsportal klicken Sie auf **neu**, **Netzwerkdienste**, **Virtuelles Netzwerk** **DNS-SERVER registrieren**.
2.  Geben Sie ein:
    - **NAME**: Contoso-DNS-EU
    - **DNS-SERVER-IP-Adresse**: 10.1.0.4 – die IP-Adresse muss mit DNS-Server virtuellen IP-Adresse.
     
3.  Wiederholen Sie den Vorgang Contoso-DNS-US folgendermaßen registrieren:
    - **NAME**: Contoso-DNS-US
    - **DNS-SERVER-IP-Adresse**: 10.2.0.4

**Zuweisen von zwei DNS-Server mit zwei virtuellen Netzwerken**

1.  Klicken Sie im linken Bereich im klassischen Portal auf **Netzwerke** .
2.  Klicken Sie auf **Contoso-VNet-EU**.
3.  Klicken Sie auf **Konfigurieren**.
4.  Wählen Sie **Contoso-DNS-EU** im Abschnitt **DNS-Server** .
5.  Klicken Sie auf das Ende der Seite **Speichern** , und klicken Sie auf **Ja,** um zu bestätigen.
6.  Wiederholen Sie den Vorgang **Contoso-VNet-US** virtuelle Netzwerk den **Contoso-DNS-US** DNS-Server zuweisen.

Alle virtuellen Computer, der mit virtuellen Netzwerken bereitgestellt muss neu gestartet werden, um die DNS-Serverkonfiguration zu aktualisieren.

**Die virtuellen Computer neu starten**

1. Azure-Verwaltungsportal klicken Sie auf **virtuelle Computer** auf der linken Seite.
2. Klicken Sie auf **Contoso-DNS-EU**.
3. Klicken Sie oben auf **Dashboard** .
4. Klicken Sie auf **neu starten** .
5. Wiederholen Sie diese Schritte **Contoso-DNS-US**neu starten.


##<a name="configure-dns-conditional-forwarders"></a>Konfigurieren von bedingter DNS-Weiterleitung

Der DNS-Server in jedem virtuellen Netzwerk kann nur DNS-Namen in diesem virtuellen Netzwerk auflösen. Sie müssen eine bedingte Weiterleitung auf Peer-DNS-Server für die Auflösung von Namen in virtuellen Peernetzwerk konfigurieren.

Um bedingte Weiterleitung zu konfigurieren, müssen Sie die Domänensuffixe zwei DNS-Server bekannt. DNS-Suffixe kann je nach Konfiguration Clouddienste beim Erstellen der virtuellen Computer verwendet. Für jede DNS-Suffix in das virtuelle Netzwerk verwendet müssen Sie eine bedingte Weiterleitung hinzufügen. 

**Die Domänensuffixe zwei DNS-Server gefunden**

1. RDP in **Contoso-DNS-EU**
2. Öffnen Sie Windows PowerShell-Konsole oder der Befehlszeile.
3. Führen Sie **Ipconfig**, und notieren Sie **verbindungsspezifische DNS-Suffix**.
4. Die RDP-Sitzung nicht geschlossen, Sie benötigen es. 
5. Wiederholen Sie die Schritte **verbindungsspezifischen DNS-Suffix** des **Contoso-DNS-US**zu.


**So konfigurieren Sie DNS-Weiterleitung**
 
1.  Die RDP-Sitzung auf **Contoso-DNS-EU**klicken Sie auf die Windows-Taste unten links.
2.  Klicken Sie auf **Verwaltung**.
3.  Klicken Sie auf **DNS**.
4.  Erweitern Sie im linken Bereich **DSN** **Contoso-DNS-EU**.
5.  Geben Sie Folgendes ein:
    - **DNS-Domäne**: Geben Sie das DNS-Suffix des Contoso-DNS-US. Beispiel: Contoso-DNS-US.b5.internal.cloudapp.net.
    - **IP-Adressen der Masterserver**: 10.2.0.4, die Contoso-DNS-US IP-Adresse eingeben.
6.  Drücken Sie die **EINGABETASTE**, und klicken Sie dann auf **OK**.  Nun werden Sie die Contoso-DNS-US IP-Adresse von Contoso-DNS-EU auflösen.
7.  Wiederholen Sie die Schritte zum Hinzufügen einer DNS-Weiterleitung des DNS-Dienstes auf dem Contoso-DNS-US virtuellen Computer mit den folgenden Werten:
    - **DNS-Domäne**: Geben Sie das DNS-Suffix des Contoso-DNS-EU. 
    - **IP-Adressen der Masterserver**: 10.2.0.4, die Contoso-DNS-EU IP-Adresse eingeben.

##<a name="test-the-name-resolution-across-the-virtual-networks"></a>Testen Sie die Auflösung über virtuelle Netzwerke

Jetzt können Sie die Auflösung des Hostnamens über virtuelle Netzwerke testen. Ping wird standardmäßig von der Firewall blockiert.  Nslookup können Sie DNS-Server virtuellen Maschinen (müssen Sie FQDN verwenden) in Peer-Netzwerken beheben.  


##<a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie gelernt, wie Auflösung über virtuelle Netzwerke mit VPN-Verbindungen zu konfigurieren. Die beiden Artikel der Serie behandelt:

- [Konfigurieren einer VPN-Verbindungs zwischen zwei Azure virtuelle Netzwerke][hdinsight-hbase-geo-replication-vnet]
- [HBase Geo Replikation][hdinsight-hbase-geo-replication]



[hdinsight-hbase-geo-replication]: hdinsight-hbase-geo-replication.md
[hdinsight-hbase-geo-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[powershell-install]: powershell-install-configure.md

[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-DNS/HDInsight.HBase.VPN.diagram.png 
