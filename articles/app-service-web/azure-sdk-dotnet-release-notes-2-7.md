
<properties 
   pageTitle="Azure SDK für .NET 2.7 und .NET 2.7.1 Versionsinformationen" 
   description="Azure SDK für .NET 2.7 und .NET 2.7.1 Versionsinformationen" 
   services="app-service\web" 
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

# <a name="azure-sdk-for-net-27-and-net-271-release-notes"></a>Azure SDK für .NET 2.7 und .NET 2.7.1 Versionsinformationen

##<a name="overview"></a>Übersicht

Dieses Dokument enthält die Versionsinformationen für das Azure SDK für .NET 2.7 Version. 

Enthält das Dokument auch die Versionshinweise Azure SDK für .NET 2.7.1 freizugeben.

Azure SDK 2.7 ist nur in Visual Studio 2015 und Visual Studio 2013 unterstützt. [Azure SDK 2.6](https://azure.microsoft.com/downloads/) ist die letzte SDK für Visual Studio 2012.

Detaillierte Informationen zu dieser Version finden Sie in [Azure SDK 2.7 Ankündigung](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) und [Azure SDK 2.7.1 Ankündigung](http://go.microsoft.com/fwlink/?LinkId=623850).

##<a name="azure-sdk-for-net-27"></a>Azure SDK für .NET 2.7

###<a name="sign-in-improvements-for-visual-studio-2015"></a>Verbesserte Visual Studio 2015 anmelden

Azure SDK 2.7 für Visual Studio 2015 unterstützt neue Identity Managementfeatures in Visual Studio 2015.  Dies umfasst Unterstützung für Konten Zugriff auf Azure Rolle Based Access Control, Cloud-Lösungsanbieter, DreamSpark und andere Konto und Ihr Abonnement.

Enthaltene Azure SDK 2.7-in verbesserte stehen in Visual Studio 2015. Visual Studio 2013 ist in Azure SDK 2.7.1 unterstützt.


###<a name="mobile-sdk"></a>Mobile SDK

**Mobile Apps** Vorlagen neueste [NuGet-Paket](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) und der Setup-Prozess entsprechend aktualisiert.

###<a name="service-bus"></a>Servicebus 

Allgemeine Fehlerbehebungen und Optimierungen. Informationen zu Updates und Funktionen finden Sie in den Release Notes des neuesten [Service Bus NuGet](http://www.nuget.org/packages/WindowsAzure.ServiceBus/).

###<a name="hdinsight-tools"></a>HDInsight-Tools 

In dieser Version wurden die folgenden Updates. Diese Updates sind in der Vorschau. Weitere Informationen finden Sie in [diesem Blog](http://go.microsoft.com/fwlink/?LinkId=619108).

- Diagramme für Struktur Tez Einzelvorgänge Struktur
- Vollständige Struktur DML IntelliSense-Unterstützung
- Schwein Unterstützung
- Storm-Vorlagen für Azure services

####<a name="breaking-changes"></a>Grundlegend geändert

- Altes **Storm** -Projekt muss aktualisiert werden, wenn diese Version des Tools verwenden. Weitere Informationen finden Sie in [diesem Blog](http://go.microsoft.com/fwlink/?LinkId=619108).
- Visual Studio Web Express wird nicht mehr unterstützt. Weitere Informationen finden Sie in [diesem Blog](http://go.microsoft.com/fwlink/?LinkId=619108).

###<a name="azure-app-service-tools"></a>Azure App Service-Tools

In dieser Version wurden die folgenden Updates Web Tools Extensions. Weitere Informationen finden Sie in [diesem](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) Blog. 

- Unterstützung für Konten DreamSpark
- Vollständige Änderung Azure Tools der neuen Azure Resource Management-APIs unterstützen
- Unterstützung für Azure App Service [Cloud Explorer](#cloud_explorer) hinzugefügt

####<a name="known-issues"></a>Bekannte Probleme

Nicht Web App Deployment Steckplatz Knoten unter dem Knoten Steckplätze im Server-Explorer angezeigt und Web App Deployment Steckplatz untergeordnete Knoten nicht unter Cloud Explorer laden. Dieses Problem wurde behoben und die nächste SDK-Version vorbereitet. 


###<a name="cloud_explorer"></a>Cloud Explorer für Visual Studio 2015

2.7 Azure SDK enthält Cloud Explorer für Visual Studio 2015 Azure Ressourcen anzeigen, überprüfen Sie deren Eigenschaften und wichtige Entwickler Aktionen in Visual Studio ausführen können. 

Cloud Explorer unterstützt:

- Ressourcengruppe und die Ressource auf der Azure-Ressourcen 
- Suchen nach Ressourcen (verfügbar in Ressourcentyp anzeigen)
- Unterstützung für Abonnements und Ressourcen Rolle basiert Zugang Steuerelement (RBAC) angewendet 
- Integrierte Aktionsbereich zeigt Entwickler Aktionen für die ausgewählten Ressourcen. Beispiel: Fügen Sie remote Debugger für erstellte virtuelle Maschinen mit Stapel Ressourcenmanager Diagnosedaten für virtuelle Maschinen usw. anzeigen.
- Integrierte Eigenschaftenpanel zeigt Entwickler Eigenschaften häufig während Entwicklung/test 
- Schnelles Umschalten der Kontos beim Auflisten der Ressourcen (Einstellungen Symbolleiste verwenden) 
- Filtern von Abonnements beim Auflisten der Ressourcen (Einstellungen Symbolleiste verwenden) 
- Deep Links zum Azure-Portal für die Verwaltung von Ressourcen und Ressourcengruppen 
 
 
###<a name="azure-resource-manager-tools"></a>Azure-Ressourcen-Manager-Tools 

Azure-Ressourcen-Manager-Tools wurden mit Rolle basiert Zugang Steuerelement (RBAC) und neue Abonnement aktualisiert.  Diese Änderung umfasst die Möglichkeit, neue Speicherkonten neben klassischen Speicher verwenden, um Elemente während der Bereitstellung gespeichert.  

Wenn Sie eine Ressourcengruppe Azure-Projekt aus einer früheren Version des SDK mit 2.7 SDK verwenden, ist ein neues Bereitstellungsskript mithilfe eines neuen Speicherkontos statt klassische Speicher erforderlich.  Sie werden aufgefordert, bevor das Projekt geändert wird das neue Skript hinzuzufügen.  Das alte Skript umbenannt, und Sie müssen Änderungen an das neue Skript manuell vornehmen.
 
 
###<a name="storage-explorer-tools"></a>Speicher-Explorer-Tools 

- Unterstützung für das Anzeigen von Blobs anfügen. In [diesem Blogbeitrag](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/04/13/introducing-azure-storage-append-blob.aspx)mehr. 
- Unterstützung für Premium-Speicherkonten durch Server-Explorer anzeigen. Server-Explorer zeigt nur Seitenblobs für Premium-Speicherkonten sind der einzige unterstützte Typ für Premium-Speicherkonten.

###<a name="azure-data-factory-tools-for-visual-studio"></a>Azure Data Factory-Tools für Visual Studio 

Einführung in **Azure Data Factory-Tools** für Visual Studio. Unten werden die aktivierten Features. [Diesen Blog](http://go.microsoft.com/fwlink/?LinkId=617530) für Weitere Informationen sehen.

- **Vorlage erstellen**: Wählen Sie Groß-/Kleinschreibung verwenden Vorlagen, Datenvorlagen Bewegung oder Vorlagen eine End-to-End-integrationslösung bereitstellen und schnell mit Data Factory Datenverarbeitung basiert. 
- **Integration in Projektmappen-Explorer für die Erstellung und Bereitstellung von Data Factory Entitäten**: Erstellen und Bereitstellen von Rohrleitungen und verknüpften Entitäten als Visual Studio-Projekte. 
- **Für visuelle Interaktion Authoring Integration Diagramm anzeigen**: Rohrleitungen und Datasets mit Hilfe der Diagrammansicht visuell erstellen. 
- **Integration in Server-Explorer zum Durchsuchen und Interaktion mit bereits bereitgestellten**: Server-Explorer durchsuchen, die bereits Daten Factorys und entsprechenden Entitäten nutzen. Importieren Sie eine Factory bereitgestellten Daten oder eine Entität (Pipeline verknüpften Serviceartikel Datasets) in Ihr Projekt. 
- **JSON-Schema-Validation und umfangreiche Intellisense bearbeiten**: effizient konfigurieren und JSON-Dokumente Data Factory Entitäten mit umfangreichen Intellisense und Schema bearbeiten 
- **Multi-Umgebung veröffentlichen**: veröffentlicht Pipelines für Entwicklung, Test oder FA-Umgebung erstellen Sie separate Dateien für jede Umgebung erstellt.
- **Schwein, Struktur und .net basiert Datenverarbeitung Support**: Support für Schweine und Struktur Skripts in Data Factory-Projekt. Unterstützung für C#-Projekt für die Verwaltung von .net auf Aktivität.

##<a name="azure-sdk-for-net-271"></a>Azure SDK für .NET 2.7.1

Der folgende Abschnitt enthält Updates, die mit Azure SDK für .NET 2.7.1 Version eingeführt wurden.
###<a name="hdinsight-tools"></a>HDInsight-Tools 

Genauere HDInsight Tools Updates finden Sie in [diesem Blog](http://go.microsoft.com/fwlink/?LinkId=623831).

- Struktur Auftrag Operator anzeigen (neu)

    Damit Sie verstehen Hive-Abfrage wurde, Struktur Operator anzeigen Feature hinzugefügt. Klicken Sie alle Operatoren in einen Scheitelpunkt Doppelklicken auf die Scheitelpunkte des Diagramms Auftrag. Bewegen Sie zum Anzeigen von Details zu bestimmten Operatoren über diesen Operator.
- Struktur Fehler Marker (neu)

    Aktivieren Sie die Grammatikfehler anzeigen wurde sofort Struktur Fehler Marker-Funktion hinzugefügt. Fehlermeldungen wurden verbessert auch detaillierte Grammatikfehler nun sofort angezeigt (bis dieser Version hatte Sie Hive-Skript zum Cluster und warten Sie einige Zeit bevor Details über die Fehlermeldung erhalten).  
- Storm-Topologie-Diagramm (neu)

    Visualisierung ist sehr wichtig, wenn Sie Ihre Topologie erwartungsgemäß sehen möchten. In dieser Version haben wir Visualisierung Sturm Diagramme hinzugefügt. Visualisieren wichtigen Metriken für Ihre Topologie (gibt z. B. eine bestimmte Bolzen Wetter "beschäftigt" oder nicht). Sie können auch Bolzen/Schnauze Weitere Einzelheiten doppelklicken.

- Unterstützung für HDInsight-Cluster, die in Azure-Portal (Fehlerkorrektur) erstellt

    Jetzt können Visual Studio anzeigen und Aufträge alle HDInsight-Cluster egal wo der Cluster erstellt wurden.

- Weitere IntelliSense-Unterstützung und schneller Struktur Metadaten laden (Verbesserung)

    Wir haben die IntelliSense verbessert, indem weitere benutzerfreundliche Vorschläge. Beispielsweise kann Tabellenalias jetzt auch IntelliSense vorgeschlagen, Ihre Abfrage leichter schreiben können. Außerdem haben wir die Struktur Metadaten laden dauert nur einige Sekunden zum Auflisten aller Datenbanken, Tabellen und Spalten der Struktur Metastore verbessert.

Genauere HDInsight Tools Updates finden Sie in [diesem Blog](http://go.microsoft.com/fwlink/?LinkId=623831).

###<a name="improvements-in-visual-studio-2013"></a>Verbesserung der Visual Studio-2013

- Azure SDK 2.7.1 ermöglicht Visual Studio 2013 Azure Konten und Abonnements Rolle Based Access Control, Cloud-Lösungsanbieter und Dreamspark.
- Azure SDK 2.7.1 steht das neue Cloud Explorer-Toolfenster auch Visual Studio 2013.

###<a name="known-issues"></a>Bekannte Probleme

Installieren von Azure SDK 2.6 oder 2.7.1 für Visual Studio Community 2013 auf ein nicht - englischen Betriebssystem wird eine Warnung angezeigt, dass Englisch und nicht englische Ressourcen von Visual Studio möglicherweise falsch zugeordnet. Diese Warnung kann sicher geschlossen werden. Es findet nur statt, wenn der Computer eine vorherige Installation von Visual Studio Community 2013 nicht enthalten und auf eine nicht - englischen Betriebssystem das SDK installieren. Die Warnung wird angezeigt, nachdem das Language Pack für Visual Studio RTM Ressourcen gilt aber vor dem Update 4 gilt. Schließen der Warnung ermöglichen Sprachpaket fortzusetzen und die Update 4 Dateiversion Language Pack anwenden.

LightSwitch-Projekte sind nicht mit dieser Version leiterplattenbestückung. Dieses Problem wird mit der nächsten SDK-Version gelöst werden.

##<a name="also-see"></a>Siehe auch
[Azure SDK 2.7.1 Ankündigung](http://go.microsoft.com/fwlink/?LinkId=623850)

[Azure SDK 2.7-Ankündigung](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/)

[Support und Informationen zur Pensionierung Azure SDK für .NET und APIs](https://msdn.microsoft.com/library/azure/dn479282.aspx/)
