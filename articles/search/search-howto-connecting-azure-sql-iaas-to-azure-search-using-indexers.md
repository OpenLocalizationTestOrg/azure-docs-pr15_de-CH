<properties 
    pageTitle="Konfigurieren einer Verbindung von Azure Search Indexer zu SQL Server auf einem virtuellen Computer Azure | Microsoft Azure | Indexer" 
    description="Aktivieren Sie verschlüsselte Verbindung und konfigurieren Sie den Firewall Verbindungen zu SQL Server auf eine Azure Virtual Machine (VM) über einen Indexer Azure suchen." 
    services="search" 
    documentationCenter="" 
    authors="jack4it" 
    manager="pablocas" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="09/26/2016" 
    ms.author="jackma"/>

# <a name="configure-a-connection-from-an-azure-search-indexer-to-sql-server-on-an-azure-vm"></a>Konfigurieren einer Verbindung von Azure Search Indexer zu SQL Server auf eine Azure-VM

Wie in [Azure SQL-Datenbank in Azure Search Indexer mit verbinden](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md#frequently-asked-questions), Indexer für **SQL Server auf Azure VMs** (oder kurz **SQL Azure VMs** ) Erstellen von Azure Search unterstützt wird, aber es gibt einige Sicherheits-Komponenten zuerst kümmern. 

**Aufgabe Dauer:** Ca. 30 Minuten installiert sofern Sie bereits ein Zertifikat auf dem virtuellen Computer.

## <a name="enable-encrypted-connections"></a>Verschlüsselte Verbindung aktivieren

Azure Suche erfordert einen verschlüsselten Kanal für alle Indexer Anfragen über einen Internetzugang. Dieser Abschnitt enthält die Schritte hierzu.

1. Überprüfen Sie die Eigenschaften des Zertifikats zu überprüfen, ob der Antragstellername den vollqualifizierten Domänennamen (FQDN) der Azure-VM ist. Tools wie CertUtils oder das Zertifikate-Snap-in können Sie die Eigenschaften anzeigen. Sie erhalten den FQDN VM Service Blade Essentials im Abschnitt im Feld **öffentlicher IP-Adressen und DNS-Namen Bezeichnung** in [Azure-Portal](https://portal.azure.com/).

    - Für virtuelle Computer mit neueren **Ressourcenmanager** Vorlage erstellt der FQDN formatiert, als `<your-VM-name>.<region>.cloudapp.azure.com`. 

    - Für ältere VMs als eine **klassische** VM erstellt der FQDN formatiert, als `<your-cloud-service-name.cloudapp.net>`. 

2. Konfigurieren Sie SQL Server um das Zertifikat mit dem Registrierungseditor (Regedit). 

    Obwohl SQL Server Configuration Manager häufig dazu verwendet, kann nicht für dieses Szenario Verwendung. Es kann das importierte Zertifikat nicht finden, weil der FQDN der VM auf Azure FQDN entspricht bestimmt die VM (es identifiziert die Domäne oder den lokalen Computer der Netzwerkdomäne, angehört). Wenn die Namen nicht übereinstimmen, verwenden Sie Regedit Zertifikat angeben.

    - Navigieren Sie in Regedit zu dieser Registrierungsschlüssel: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\[MSSQL13.MSSQLSERVER]\MSSQLServer\SuperSocketNetLib\Certificate`.
     
    Die `[MSSQL13.MSSQLSERVER]` Teil variiert je nach Version und Instanz. 

    - Legen Sie den Wert des **Zertifikatsschlüssels** , den **Fingerabdruck** des SSL-Zertifikats für die VM importiert.

    Es gibt mehrere Arten den Fingerabdruck einige besser als andere. Wenn aus das **Zertifikate** -Snap-in in MMC kopieren holen wahrscheinlich Sie ein unsichtbares Zeichen [gemäß diesem Artikel unterstützt](https://support.microsoft.com/kb/2023869/), wodurch Fehler bei einem Verbindungsversuch. Mehrere Abhilfen gibt es für dieses Problem zu beheben. Ist am einfachsten über die RÜCKTASTE und geben das erste Zeichen des Fingerabdrucks führende Zeichen im Feld Wert Regedit zu entfernen. Ein anderes Tool können Sie auch um den Fingerabdruck zu kopieren.

3. Berechtigungen Sie für das Dienstkonto. 

    Stellen Sie sicher, dass das SQL Server-Dienstkonto berechtigt für den privaten Schlüssel des SSL-Zertifikats gewährt wird. Wenn Sie diesen Schritt übersehen, wird SQL Server nicht gestartet. **Zertifikate** -Snap-in oder **CertUtils** verwenden Sie für diese Aufgabe.

4. Starten Sie den SQL Server-Dienst.

## <a name="configure-sql-server-connectivity-in-the-vm"></a>Konfigurieren der SQL Server-Konnektivität auf dem virtuellen Computer

Nach dem Einrichten von Azure Suche erforderlich verschlüsselt, sind zusätzliche Konfigurationsschritte in SQL Server in Azure VMs. Wenn Sie dies noch nicht getan haben, besteht der nächste Schritt abgeschlossen-Konfiguration mit einem der folgenden Artikel:

- Ein **Ressourcen-Manager** VM finden Sie unter [Verbindung zu einer SQL Server virtuellen Maschine auf Azure mit Ressourcen-Manager](../virtual-machines/virtual-machines-windows-sql-connect.md). 

- Eine **klassische** VM finden Sie unter [Verbindung zu einem SQL Server virtuellen Computer Azure Classic](../virtual-machines/virtual-machines-windows-classic-sql-connect.md).

Insbesondere überprüfen Sie im Abschnitt jedes Artikels "über das Internet verbinden".

## <a name="configure-the-network-security-group-nsg"></a>Konfigurieren Sie die Netzwerk-Sicherheitsgruppe (NSG)

Es ist nicht ungewöhnlich, die NSG und entsprechenden Azure Endpunkt konfigurieren oder eine Zugriffssteuerungsliste (ACL) Azure-VM an andere Parteien zugänglich. Wahrscheinlich haben Sie getan haben eigene Anwendungslogik Verbindung zu Ihrem SQL Azure-VM zu. Es unterscheidet sich Azure Search Verbindung mit Ihrem SQL Azure-VM. 

Die unten stehenden Links bereitgestellt NSG Konfiguration für VM-Bereitstellung. Gehen Sie folgendermaßen vor um ACL anliegenden Azure Suche anhand seiner IP-Adresse.

> [AZURE.NOTE] Hintergrundinformationen finden Sie unter [Was ist eine Netzwerk-Sicherheitsgruppe?](../virtual-network/virtual-networks-nsg.md)

- Ein **Ressourcen-Manager** VM finden Sie unter [NSGs für ARM-Installationen erstellen](../virtual-network/virtual-networks-create-nsg-arm-pportal.md). 

- Eine **klassische** VM finden Sie unter [NSGs für Classic Bereitstellungen erstellen](../virtual-network/virtual-networks-create-nsg-classic-ps.md).

IP-Adressierung bergen einige Probleme, die überwunden werden, wenn Sie das Problem und mögliche Abhilfen kennen. Die verbleibenden Abschnitte enthalten Vorschläge für die Behandlung von Problemen im Zusammenhang mit IP-Adressen in der ACL.

#### <a name="restrict-access-to-the-search-service-ip-address"></a>Beschränken des Zugriffs auf die Suche Dienst-IP-Adresse

Es wird dringend empfohlen, dass der Zugriff auf die IP-Adresse in der ACL anstatt der SQL Azure-VMs offen auf alle Verbindungsanfragen Suchdienst. Leicht finden Sie die IP-Adresse pingen Sie den FQDN (z. B. `<your-search-service-name>.search.windows.net`) der Suchdienst.

#### <a name="managing-ip-address-fluctuations"></a>IP-Adresse Fluktuationen verwalten

Suchdienst nur eine Suche Einheit (ein Replikat und eine Partition) hat, ändert die IP-Adresse während der Wartung wird neu gestartet, eine vorhandene ACL mit der Search-Dienst-IP-Adresse ungültig.

Können die nachfolgenden Verbindungsfehler zu vermeiden ist für mehr als ein Replikat und eine Partition Azure verwendet. Dies erhöht die Kosten jedoch löst das Problem der IP-Adresse. Azure-Suche ändern nicht IP-Adressen, wenn mehrere Suchen Einheit verfügen.

Ein zweiter Ansatz ist die Verbindung fehlschlagen, und konfigurieren die ACLs in der NSG. Im Durchschnitt erwarten Sie IP-Adressen für alle Wochen geändert. Für Kunden, die kontrollierte Indizierung seltener möglicherweise geeignete Vorgehensweise.

Eine dritte Methode geeignete (jedoch nicht besonders sichere) ist an den IP-Adressbereich der Azure-Region, wo Suchdienst bereitgestellt wird. Die Liste der IP-Adressbereiche aus dem Azure-Ressourcen öffentliche IP-Adressen zugewiesen werden erscheint in [Azure Datacenter IP-Bereiche](https://www.microsoft.com/download/details.aspx?id=41653). 

#### <a name="include-the-azure-search-portal-ip-addresses"></a>Die Azure Search Portal IP-Adressen

Verwenden Sie Azure-Portal Indexer erstellen, benötigt Azure Suchlogik Portal auch Zugriff auf SQL Azure VM während der Erstellung. Azure Suche Portal IP-Adressen pingen finden `stamp2.search.ext.azure.com`.

## <a name="next-steps"></a>Nächste Schritte

Mit dem Weg können Sie SQL Server jetzt auf Azure VM als Datenquelle für Azure Search Indexer angeben. Weitere Informationen finden Sie in [Azure SQL-Datenbank in Azure Search Indexer mit verbinden](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md) .
