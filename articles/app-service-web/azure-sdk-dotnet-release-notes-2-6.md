<properties 
   pageTitle="Azure SDK für .NET 2.6-Versionsinformationen" 
   description="Azure SDK für .NET 2.6-Versionsinformationen" 
   services="app-service/web" 
   documentationCenter=".net" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/17/2016"
   ms.author="juliako"/>

 
# <a name="azure-sdk-for-net-26-release-notes"></a>Azure SDK für .NET 2.6-Versionsinformationen

Dieses Dokument enthält die Versionsinformationen für das Azure SDK für .NET 2.6 Version. 

Azure SDK 2.6 entwickeln Sie Service Cloudanwendungen (PaaS) für .NET 4.5.2 .NET 4.6, auf Cloud-Dienst Rollen manuell das Ziel.NET Framework installiert. Finden Sie unter [.NET eine Rolle Cloud-Dienst installieren](http://go.microsoft.com/fwlink/?LinkID=309796).


##<a name="service-bus-updates"></a>Service Bus-updates

- Ereignis-Hubs: 

    - Gezielte Zugriffskontrolle können jetzt beim Senden von Ereignissen durch zusätzliche Publisher Endpunkt für Ereignis Hubs verfügbar machen.
    - Zusätzliche Stabilität und Verbesserung Ereignis Hubs Feature hinzugefügt.
    - Hinzufügen von Unterstützung Amqp Protokoll über WebSocket für messaging und Ereignis.

##<a name="hdinsight-tools-for-visual-studio-updates"></a>HDInsight aktualisieren für Visual Studio

- **IntelliSense-Erweiterung**: remote Metadaten Vorschlag

    HDInsight-Tools für Visual Studio unterstützt jetzt immer remote Metadaten beim Bearbeiten von Skripts Struktur. Sie können beispielsweise * *Wählen* von** alle Tabellennamen angezeigt. Außerdem werden die Spaltennamen nach Angabe einer Tabelle angezeigt.

- **HDInsight Emulator-Unterstützung**

    Jetzt HDInsight für Visual Studio Supporttools HDInsight Emulator herstellen, diese Skripts mit HDInsight-Cluster ausführen, damit Sie Ihre Struktur Skripts lokal entwickeln konnte ohne Kosten, 

    Weitere Informationen finden Sie [in diesem Handbuch](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409).

- **HDInsight für Visual Studio Supporttools für generische Hadoop Cluster** (Vorschau)

    HDInsight-Tools für Visual Studio unterstützt jetzt generische Hadoop Cluster HDInsight Tools für Visual Studio Verwendung Folgendes:

    - Verbindung zum cluster 
    - Schreiben Sie Hive-Abfrage mit erweiterten IntelliSense, Auto-Vervollständigung Unterstützung 
    - Alle Aufträge im Cluster mit einer intuitiven Benutzeroberfläche anzeigen. 

    Weitere Informationen finden Sie [in diesem Handbuch](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409).

##<a name="in-role-cache-updates"></a>Aktualisierung in Rolle

- **Cache Funktion** wurde aktualisiert **Microsoft Azure Storage SDK** Version 4.3. Bis jetzt wurde **In Rolle Cache** Azure Storage SDK Version 1.7 verwenden.

    Kunden mit Azure SDK 2.5 oder unten sollte Azure SDK 2.6 aktualisieren und verschieben auf die neue Version von Azure Storage SDK. 

    Zu diesem Zeitpunkt soll Azure Storage Version 2011-08-18 1. August 2016 entfernt werden. Alle Migrationen In Rolle Cache von Azure SDK 2.5 oder unten müssen 2,6 bis zu diesem Zeitpunkt abzuschließen. Finden Sie weitere Informationen zu den Ruhestand Azure Storage Version 2011-08-18 [Microsoft Azure Storage Service Version entfernen Update: Erweiterung 2016](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx).

>[AZURE.IMPORTANT]Wir haben am 30. November 2016 Ruhestand Azure Managed Cache Service und Azure-Rolle Cache bekannt. Wir empfehlen, Azure Redis Cache in Vorbereitung dieser Rente migrieren. Weitere Hinweise zu Datums- und Hinweise zur Migration finden Sie unter [die Azure Cache Angebot ist richtig für mich?](../redis-cache/cache-faq.md#which-azure-cache-offering-is-right-for-me)

##<a name="azure-app-service-tools"></a>Azure App Service-Tools

Die folgenden Elemente aktualisiert wurden in der Version 2.6 von Azure SDK.

- Erweitert und Azure API-Apps als Bereitstellungsziel Azure veröffentlichen.
- API-Apps Bereitstellung Funktionen können auch Benutzer mit API App erstellen und bereitstellen.
- Server-Explorer geändert neue App-Dienstknoten Web, Mobile und API-Apps Ressourcengruppe gruppiert.
- Fügen Sie Azure API-App Client Bewegung hinzugefügt, die meisten C#-Projekte, die automatische Erstellung von stolz aktiviert API-Apps in Azure Abonnement des Benutzers ausgeführt werden kann.
- API-Apps Werkzeuge und App Service-Knoten im Server-Explorer sind nur verfügbar in Visual Studio 2013. 

##<a name="azure-resource-manager-tools-updates"></a>Azure aktualisieren Resource Manager

Azure-Ressourcen-Manager-Tools wurden aktualisiert, enthält Vorlagen für virtuelle Computer, Netzwerke und Speicher. JSON Bearbeitung aktualisiert eine neue Gliederung für Vorlagen und bearbeiten Sie die Vorlagen mit JSON Ausschnitte angezeigt. Von Visual Studio bereitgestellten Vorlagen PowerShell-Skript mit dem Projekt bereitgestellt sodass Änderungen an das Skript von Visual Studio verwendet werden.

##<a name="diagnostics-improvements-for-cloud-services"></a>Verbesserung der Diagnose für Cloud-Services

Azure SDK 2.6 ist wieder eine Unterstützung für das Sammeln Diagnoseprotokolle in Azure-Serveremulator und Entwicklung Speicher übertragen. Alle Diagnose protokolliert (einschließlich Anwendung Trace Logs, Event Tracing for Windows (ETW) Ereignisprotokollen, Leistungsindikatoren, Infrastruktur-Protokolle und Windows-Ereignisprotokolle) generiert, wenn die Anwendung im Emulator ausgeführt wird übertragbar Entwicklung Speicher zu überprüfen, ob die Diagnose-Protokollierung auf dem lokalen Computer funktioniert. 

Das Speicherkonto für die Diagnose kann nun in der unterschiedlichen Umgebungen mit verschiedenen Diagnose Speicherkonten erleichtert Service-Konfigurationsdatei (.cscfg) angegeben werden. Es gibt einige wichtige Unterschiede zwischen wie die Verbindungszeichenfolge Azure SDK 2.4 und 2.6 von Azure SDK tätig. Informationen wie die Diagnose Speicher Verbindung Zeichenfolge und wie sie Ihre Projekte finden Sie unter [Diagnose für Azure Cloud Services konfigurieren](http://go.microsoft.com/fwlink/?LinkID=532784).

##<a name="breaking-changes"></a>Grundlegend geändert

###<a name="azure-resource-manager-tools"></a>Azure-Ressourcen-Manager-Tools 

- Der **Cloud Deployment Projects** -Projekttyp Azure SDK 2.5 für wurde **Azure**-Ressourcengruppe umbenannt.
- **Cloud-Bereitstellungsprojekte** in Azure SDK 2.5 erstellte Projekte können verwendet werden in 2.6 Bereitstellen der Vorlage von Visual Studio schlägt jedoch fehl. Bereitstellung mit PowerShell-Skript noch funktioniert jedoch wie zuvor.  Informationen zur Verwendung finden **Cloud Bereitstellungsprojekte** 2.6 dieser [Buchen](http://go.microsoft.com/fwlink/?LinkID=534086).
 
##<a name="known-issues"></a>Bekannte Probleme

- Sammeln Diagnoseprotokolle im Emulator ist ein 64-Bit-Betriebssystem erforderlich. Diagnoseprotokolle werden bei Ausführung auf einem 32-Bit-Betriebssystem nicht gesammelt. Dies wirkt sich nicht auf andere Funktionen des Emulators. 

- Azure SDK 2.6 4/29/2015 veröffentlicht hat zwei Aspekte: 

    - Universelle App konnte nicht in Visual Studio 2015 geladen werden, wenn Azure SDK 2.6 auf dem Computer installiert wurde.
    - Debuggen ein Cloud-Dienstprojekt fehl 2013 für Visual Studio und Visual Studio 2015, wo Visual Studio reagiert und stürzt ab, wenn ein Dialogfeld mit der Meldung "Diagnose konfigurieren für Emulator" angezeigt.
    
    Ein Update auf Azure SDK 2.6 wurde am 18/5/2015. Die aktualisierte Version ist 2.6.30508.1601. Es behebt zwei Probleme erläutert. Den Build SDK unter Systemsteuerungsoption identifizieren -> Programme und Features -> Microsoft Azure-Tools für Microsoft Visual Studio 2013 – V 2.6. Die Spalte Version zeigt die Build-Nummer.

    Wenn Sie weiterhin die oben genannten Probleme sind, installieren Sie die neueste Version von Azure 2.6 SDK für [VS 2012](http://go.microsoft.com/fwlink/p/?linkid=323511&clcid=0x409), [VS 2013](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) oder [VS 2015](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409).
 
##<a name="see-also"></a>Siehe auch

[Support und Informationen zur Pensionierung Azure SDK für .NET und APIs](https://msdn.microsoft.com/library/azure/dn479282.aspx/)
