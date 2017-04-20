<properties
    pageTitle="Sicherheitsaspekte für SQL Server in Azure | Microsoft Azure"
    description="Dieses Thema bezieht sich auf Ressourcen mit dem klassischen Bereitstellungsmodell und allgemeine Hinweise zum Sichern von SQL Server mit einer Azure Virtual Machine."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
   editor=""    
   tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="06/24/2016"
    ms.author="jroth" />

# <a name="security-considerations-for-sql-server-in-azure-virtual-machines"></a>Sicherheitsaspekte für SQL Server in Azure Virtual Machines
 
Dieses Thema enthält allgemeine Richtlinien, die sicheren Zugriff auf SQL Server-Instanzen in Azure-VM hergestellt. Allerdings empfiehlt Gewährleistung besseren Schutz der SQL Server-Datenbank-Instanzen in Azure die herkömmlichen lokalen Sicherheitsverfahren neben der bewährten Sicherheitsmethoden für Azure implementieren.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Weitere Informationen über die Sicherheitsverfahren für SQL Server finden Sie in [SQL Server 2008 R2 Security Best Practices - Operative und Administrationsaufgaben](http://download.microsoft.com/download/1/2/A/12ABE102-4427-4335-B989-5DA579A4D29D/SQL_Server_2008_R2_Security_Best_Practice_Whitepaper.docx)

Azure entspricht mehreren rechtlichen Vorschriften und Standards, mit denen eine Lösung mit SQL Server auf einem virtuellen Computer ausführen zu können. Finden Sie Informationen zur Einhaltung der Azure [Azure Trust Center](https://azure.microsoft.com/support/trust-center/).

Es folgt eine Liste der Sicherheitsaspekte beim Konfigurieren und eine Verbindung zu der Instanz von SQL Server in einer VM Azure berücksichtigt werden sollten.

## <a name="considerations-for-managing-accounts"></a>Faktoren für das Verwalten von Konten:

- Erstellen eines eindeutigen lokalen Administratorkontos nicht **Administrator**mit dem Namen.

- Verwenden Sie komplexe Kennwörter für alle Konten. Weitere Informationen zum Erstellen eines sicheren Kennworts finden Sie [Tipps zum Erstellen sicherer Kennwörter](http://windows.microsoft.com/en-us/windows-vista/Tips-for-creating-a-strong-password) Artikel.

- Standardmäßig wählt Azure Windows-Authentifizierung während der Installation von SQL Server virtueller Computer. Daher der Benutzernamen **SA** ist deaktiviert und ein Passwort von Setup. Wir empfehlen der Benutzernamen **SA** ist nicht verwendet oder aktiviert. Folgende sind alternative Strategien ggf. eine SQL-Anmeldung:
    - Erstellen Sie ein SQL-Konto, das Mitglied der Sysadmin ist.
    - Verwenden Sie **SA** -Anmeldung müssen, aktivieren Sie die Anmeldung umbenennen Sie und weisen Sie ein neues Kennwort zu.
    - Die Optionen, die zuvor erwähnt wurden erfordern eine Änderung der Authentifizierungsmodus auf **SQL Server und Windows-Authentifizierungsmodus**. Weitere Informationen finden Sie unter [Server-Authentifizierungsmodus ändern](https://msdn.microsoft.com/library/ms188670.aspx).

## <a name="considerations-for-securing-connections-to-azure-virtual-machine"></a>Hinweise zum Sichern von Verbindungen zu Azure Virtual Machine:

- Verwenden Sie [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) virtuelle Computer anstelle von öffentlichen RDP-Ports verwalten.

- Verwenden Sie eine [Netzwerk-Sicherheitsgruppe](../virtual-network/virtual-networks-nsg.md) (NSG) gewähren oder verweigern den Netzwerkverkehr auf dem virtuellen Computer. Ggf. eine NSG und bereits einen Endpunkt ACL zuerst den Endpunkt ACL. Informationen hierzu finden Sie unter [Verwalten von Zugriffssteuerungslisten (ACLs) für Endpunkte mithilfe von PowerShell](../virtual-network/virtual-networks-acl-powershell.md).

- Bei Verwendung von Endpunkten entfernen Sie alle Endpunkte auf dem virtuellen Computer, wenn Sie nicht verwenden. Hinweise zur Verwendung von ACLs mit Endpunkten finden Sie unter [Verwalten der ACL für einen Endpunkt](../virtual-network/virtual-machines-windows-classic-setup-endpoints.md#manage-the-acl-on-an-endpoint).

- Aktivieren Sie eine verschlüsselte Verbindung-Option für eine Instanz von SQL Server-Datenbankmodul in Azure Virtual Machines. Konfigurieren Sie SQL Server-Instanz ein signiertes Zertifikat. Weitere Informationen finden Sie unter [Aktivieren verschlüsselte Verbindung für das Datenbankmodul](https://msdn.microsoft.com/library/ms191192.aspx) und [Syntax für Verbindungszeichenfolgen](https://msdn.microsoft.com/library/ms254500.aspx).

- Verwenden Sie die virtuellen Computer nur über ein bestimmtes Netzwerk zugegriffen werden soll, Windows-Firewall den Zugriff auf bestimmte IP-Adressen oder Netzwerk-Subnetzen einschränken.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie auch bewährte Leistung interessiert sind, finden Sie unter [Best Practices zur Performance für SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-performance.md).

Weitere Themen zum Ausführen von SQL Server in Azure VMs finden Sie unter [SQL Server auf Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).
