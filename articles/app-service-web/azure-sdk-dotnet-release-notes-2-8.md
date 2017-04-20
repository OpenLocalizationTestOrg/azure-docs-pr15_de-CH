
<properties 
   pageTitle="Azure SDK für .NET 2.8-Versionsinformationen" 
   description="Azure SDK für .NET 2.8-Versionsinformationen" 
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
 
# <a name="azure-sdk-for-net-28-281-and-282"></a>Azure SDK für .NET 2.8 2.8.1 2.8.2

##<a name="overview"></a>Übersicht
 
Dieser Artikel enthält die Versionsinformationen (die bekannte Probleme und Änderungen enthält) für Azure SDK für .NET 2.8, 2.8.1 und 2.8.2 frei. 

Vollständige Liste der neuen Funktionen und Änderungen in dieser Version finden Sie in der Ankündigung [Azure SDK 2.8 2013 für Visual Studio und Visual Studio 2015](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/) . 

##  <a name="azure-sdk-for-net-28"></a>Azure SDK für .NET 2.8

### <a name="download-azure-sdk-for-net-28"></a>Azure SDK für .NET 2.8 herunterladen

[Azure SDK für .NET 2.8 für Visual Studio 2015](http://go.microsoft.com/fwlink/?LinkId=699285) 

[Azure SDK für .NET 2.8 für Visual Studio 2013](http://go.microsoft.com/fwlink/?LinkId=699287)
 
### <a name="net-452-support"></a>.NET 4.5.2 unterstützt 

####<a name="known-issues"></a>Bekannte Probleme

Azure .NET SDK 2.8 ermöglicht .NET 4.5.2 Cloud Service-Pakete. Jedoch 4.5.2 .NET Framework nicht auf dem Gastbetriebssystem-Images bis Januar 2016 Gastbetriebssystem freigeben installiert. Bevor 4.5.2, .NET Framework werden über eine separate Gastbetriebssystem Releaseversion – November 2015-02. Siehe [Azure Gast Betriebssystemversionen und SDK-Kompatibilitätsmatrix](../cloud-services/cloud-services-guestos-update-matrix.md) nachverfolgt, wenn das Bild erscheint.  Sobald November 2015 02 Bild veröffentlicht können Sie das Bild durch Aktualisieren der Cloud Service-Konfigurationsdatei (.cscfg) verwenden. In der Dienstkonfiguration Dateigruppe OsVersion Attribut des ServiceConfiguration-Elements auf die Zeichenfolge "WA-Gast-Betriebssystem-4.26_201511-02". Möchten Sie dieses Bild verwenden, dann Sie automatische Updates auf dem Gastbetriebssystem nicht mehr erhalten nutzen. Um die automatischen Updates OsVersion eingestellt werden "*" und .NET 4.5.2 werden durch automatische Updates im Januar 2016.

###<a name="azure-data-factory"></a>Azure Data Factory

####<a name="known-issues"></a>Bekannte Probleme 

Während eine **Factory Vorlage** Erstellung mit Beispieldaten möglicherweise Azure Power Shell-Skript nach 0.9.8 ist Azure Power Shell Version auf dem Computer installiert.

Um diese Art von Projekt erfolgreich erstellt haben, installieren Sie [Azure Power Shell Version 0.9.8](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi).


### <a name="azure-resource-manager-tools"></a>Azure-Ressourcen-Manager-Tools 

####<a name="breaking-changes"></a>Grundlegend geändert

Das Powershellskript Ressourcengruppe Azure-Projekt wurde in dieser Version mit neuen Azure PowerShell-Cmdlets Version 1.0 arbeiten aktualisiert.  Das neue Skript funktioniert aus mit Visual Studio nicht bei Verwendung eine Version vor 2.8 SDK.  

Skripts in früheren Versionen des SDK erstellte Projekte werden nicht in Visual Studio ausführen Wenn 2.8 SDK verwenden.  Alle Skripts werden weiterhin außerhalb von Visual Studio mit der entsprechenden Version von Azure PowerShell-Cmdlets.  

2.8 SDK erfordert Version 1.0 von Azure PowerShell-Cmdlets.  Alle anderen Versionen des SDK erfordern Version 0.9.8 von Azure PowerShell-Cmdlets.  Weitere Informationen finden Sie in [diesem](http://go.microsoft.com/fwlink/?LinkID=623011) Blog.

###<a name="web-tools-extensions"></a>Web Tools Extensions

####<a name="known-issues"></a>Bekannte Probleme

Die folgenden bekannten Probleme werden in folgenden Versionen behoben.

- App Service verbundene Cloud und Server Explorer Bewegung für nicht-produktiven Umgebung (wie Azure China oder Azure Stapel Kunden) funktionieren nicht. Für Kunden in diesen betroffenen ermöglicht Veröffentlichungsprofil von Azure-Portal herunterladen publishing Fähigkeit. Eine zukünftige Version repariert Gesten wie "Debugger anfügen" und "Streaming-Protokolle" Azure China und Stack-Kunden. 
- Kunden können Fehler bei App sehen Erstellung beim Einblicke App-Instanz, die bereitgestellt werden als ostasiatischen US ist. In diesen Szenarien können ein App-Dienst im Portal erstellen und downloaden das Veröffentlichungsprofil Szenarien für die Veröffentlichung. 

###<a name="azure-hdinsight-tools"></a>Azure HDInsight Tools

####<a name="new-updates"></a>Neue updates

- Sie können im Cluster über HiveServer2 fast ohne Aufwand Hive-Abfrage ausführen und der Auftrag Echtzeit Protokolle.
- Die neue Struktur Ausführung Ansicht in Ihrem Auftrag tiefer zu graben, ausführliche und potenzielle Probleme identifizieren.

Informationen finden Sie in [Azure SDK 2.8 2013 für Visual Studio und Visual Studio 2015](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/). 

## <a name="azure-sdk-for-net-281"></a>Azure SDK für .NET 2.8.1

### <a name="known-issues-for-visual-studio-2013-and-visual-studio-2015"></a>Bekannte Probleme bei Visual Studio 2013 und Visual Studio 2015
 
1. Ausgelöste Webauftrags Steckplätze wird veröffentlicht anzeigen und Fehler und wird keinen Zeitplan jedoch den Webauftrag in Azure. Kunden, die einen geplanten Auftrag können Sie Azure-Portal Einrichten des Zeitplans für den Webauftrag. 
2. Python-Kunden können Debugger Probleme auftreten. Service-Team rollt eine Korrektur für dieses aber wenn Kunden betroffen sind, können Microsoft wissen in den Foren oder im Blog Ankündigung oder release Notes-Kommentaren. 
3. In bestimmten Regionen (wie Südindien) wird App Service-Bereitstellung Fehler auftreten. Dies ist mit dem Portal und Kunden dieses Problems auftreten können Azure-Portal Zugriff auf Geo-Regionen veröffentlichen anfordern. Sobald sie Zugriff auf diese Bereiche mit anfordern sollte die Azure Portal-Bereitstellung arbeiten. 

##<a name="azure-sdk-for-net-282"></a>Azure SDK für .NET 2.8.2

Nach der Installation der 2.8.2 Tools, Kunden können folgende Probleme auftreten.         

- Wenn Sie Windows 10 verwenden und Internet Explorer nicht installiert haben, erhalten Sie eine Fehlermeldung "InternetExplorer konnte nicht gefunden werden".
Um das Problem zu beheben, installieren Sie Internet Explorer über das Dialogfeld "Windows-Komponenten hinzufügen/entfernen".

Wenn Sie dieses Problem beobachten, Funktion senden Lächeln um zu melden.

Weitere Informationen finden Sie unter [diese](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-2-for-net/) buchen.
##<a name="other-updates"></a>Andere updates

Andere Updates finden Sie in [Azure SDK 2.8 Ankündigung](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).

##<a name="also-see"></a>Siehe auch

[Azure SDK 2.8-Ankündigung](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/)

[Support und Informationen zur Pensionierung Azure SDK für .NET und APIs](https://msdn.microsoft.com/library/azure/dn479282.aspx)

