<properties
   pageTitle="SQL Azure Azure RemoteApp | Microsoft Azure"
   description="Informationen Sie zur Verwendung von SQL Azure Azure RemoteApp."
   services="remoteapp"
   documentationCenter=""
   authors="ericorman"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>

# <a name="sql-azure-with-azure-remoteapp"></a>SQL Azure Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Wenn Kunden ihre Windows Applications in der Cloud Azure RemoteApp host wählen sie auch ihre Daten wie SQL Server in die Cloud für eine gesamte Cloudbereitstellung migrieren möchten. Dadurch können ganze gehostete Cloudlösung, die jederzeit von jedem Gerät mit Azure RemoteApp zugegriffen werden kann. Nachfolgend sind Links und Referenzen mit Anleitung dabei helfen.  

## <a name="migrate-your-sql-data"></a>Die SQL-Daten migrieren

Beginnen Sie mit [eine Azure SQL-Datenbank SQL Server-Datenbank migrieren](../sql-database/sql-database-cloud-migrate.md). 

## <a name="configure-azure-remoteapp"></a>Konfigurieren von Azure RemoteApp
Die Windows-Anwendung in Azure RemoteApp zu hosten. Unten ist eine sehr hohe Schritt für Schritt:

1.     [Azure RemoteApp Vorlage VM](remoteapp-imageoptions.md)erstellen. 
2.     Installieren Sie die Anwendung auf dem virtuellen Computer.
3.     Konfigurieren Sie die Anwendung in die SQL-Datenbank verbindet und bestätigen Sie, dass es funktioniert.
4.     Sysprep und Herunterfahren der virtuellen Computer. Als Bild für die Verwendung mit Azure erfassen. **Hinweis:** Sie müssen sicherstellen, dass die Anwendung zu DB-Verbindungsinformationen über den Sysprep-Prozess. Wenn die Anwendung die Verbindungsinformationen DB beibehalten kann, empfiehlt es sich zu den Hersteller der Anwendung überprüfen, wie wir die Verbindungszeichenfolge angeben können.
5.     Importieren Sie benutzerdefinierte Bild in die richtige Lage, die SQL Azure-Bereitstellung befindet auswählen Azure RemoteApp-Bibliothek 
6.     Bereitstellen von RemoteApp-Sammlung in das Rechenzentrum der SQL Azure-Bereitstellung mit der obigen Vorlage und veröffentlichen Sie die Anwendung. Bereitstellen von Azure RemoteApp im gleichen Rechenzentrum als Ihrem SQL Azure unterstützt Bereitstellung gewährleisten die schnellste Geschwindigkeit und Latenz. 

## <a name="app-and-sql-configuration-considerations"></a>App und SQL-Konfiguration zu beachten:
Sind einige Punkte zu SQL Azure RemoteApp verwenden:

Informationen Sie [zum Firewall eine SQL Azure-Datenbank konfigurieren](../sql-database/sql-database-firewall-configure.md). Auszug aus den Mitgliedstaaten Artikel "zunächst alle Zugriff auf den Azure SQL-Datenbank-Server von der Firewall blockiert wird. Um Ihren Azure SQL-Datenbank-Server zunächst müssen Sie zum klassischen Portal und geben mindestens auf Serverebene Firewallregeln, die Zugriff auf den Azure SQL-Datenbankserver. Verwenden Sie die Firewall-Regeln, geben Sie die IP-Adressbereiche aus dem Internet zulässig sind und ob Azure Applications versuchen können, Ihre Azure SQL-Datenbankserver herstellen."

Auch wenn ein Computer über das Internet eine Verbindung zum Datenbankserver versucht, die Firewall prüft die ursprüngliche IP-Adresse der Anforderung mit den vollständigen Satz der Serverebene und (falls erforderlich) Firewallregeln auf Datenbankebene. "Ist die IP-Adresse der Anforderung innerhalb eines der Serverebene Firewallregeln angegeben, wird die Verbindung mit dem Azure SQL-Datenbankserver gewährt." Daher können wir mithilfe der IP-Bereiche und nicht nur einzelne IP-Adresse.

Befolgen Sie die Anleitung in [wie: Konfigurieren der Firewall für SQL-Datenbank mithilfe der Azure-Portal](../sql-database/sql-database-configure-firewall-settings.md) IP-Adressbereich angeben. Beim Konfigurieren der SQL-Firewall-Regeln Geben Sie den IP-Adressbereich des Subnetzes angegeben für Azure RemoteApp-Auflistung. Dadurch sollte die ARA-Server Verbindung mit der SQL-Datenbank, obwohl sie-IP Adressen dynamisch zugewiesen werden müssen.

## <a name="troubleshooting"></a>Problembehandlung
Wenn die Benutzung von einer Clientanwendung gehostet in Azure RemoteApp Verbindung eine SQL-Datenbank bei Azure gehostet oder lokalen langsam gäbe Gründe warum.  

- Netzwerklatenz vom Gerät in Azure ist hoch. Verschieben der schnellste und Netzwerk-Verbindung können Sie für optimale Leistung. Verwenden Sie [azurespeed.com](http://azurespeed.com/) als allgemeines Tool zum Testen Ihre Geräte Latenz Azure-Rechenzentrum.  
- Client-Anwendung in Azure RemoteApp gehostet ist ausgelastet. Auswahl verschiedenen Fakturierungsplan wie Premium Abrechnung verbessert die Leistung. Ein anderer Trick besteht zum Überwachen der Ressourcen die Anwendung belegt: während einer aktiven Sitzung ausführen eine Tastenkombination STRG-Alt-ENTF SAS-Bildschirm zu starten, wählen Sie Task-Manager und Ressourcenverwendung für Ihre Anwendung beobachten.
- SQL Server ist ausgelastet oder nicht optimiert. Folgen Sie SQL-Anleitung zur Problembehandlung. 

