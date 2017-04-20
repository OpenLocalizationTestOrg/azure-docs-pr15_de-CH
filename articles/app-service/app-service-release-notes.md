<properties 
   pageTitle="Azure SDK für .NET 2.5.1-Versionsinformationen" 
   description="Azure SDK für .NET 2.5.1-Versionsinformationen" 
   services="app-service" 
   documentationCenter=".net,nodejs,java" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/10/2016"
   ms.author="juliako"/>


# <a name="azure-sdk-for-net-251-release-notes"></a>Azure SDK für .NET 2.5.1-Versionsinformationen

Dieses Dokument enthält die Versionsinformationen für Azure SDK für .NET 2.5.1 freizugeben. 

##<a name="azure-sdk-for-net-251-release-notes"></a>Azure SDK für .NET 2.5.1-Versionsinformationen

Im folgenden werden neue Features und Updates in Azure SDK für .NET 2.5.1.

- Neue Features\scenarios **Web Tools Extensions**beziehen. 

    - Azure Websites wurde umbenannt in Azure App Service. Weitere Informationen finden Sie unter [Azure App Service und vorhandene Azure Services](app-service-changes-existing-services.md).
    - Azure API-Apps (Vorschau) Unterstützung wurde hinzugefügt, damit kann ASP.NET als API-Apps Projekte, und Sie dann hinzufügen verwenden > Azure API-App Client Bewegung in C#-Projekten zum Generieren von Code basierend auf der Struktur der bereitgestellten API-App. 
    - Webseiten-Knoten im Server-Explorer anstelle der Knoten Azure App Service weiterentwickelt unterstützt gruppenbasierte Gruppierung von Azure API-Apps, Mobile Apps und Web Apps Ressource enthält.
    - Azure Mobile Apps (Vorschau) Unterstützung wurde hinzugefügt, damit Kunden neue Mobile Apps Projekte erstellen, Mobile Apps Controller, die Projekte und Remote Applikationen Debuggen
    - Hinzufügen > Azure API-App Client Bewegung unterstützt jetzt lokale Swagger JSON-Dateien, damit Entwickler von Web-API können Drittanbieter-NuGets wie Swashbuckle stolz generieren oder manuell erstellen. Auf diese Weise Client Entwickler Code Generation Features können alle stolz Endpunkt in C#-Projekten genutzt. 
    - Web App und API App veröffentlichen Dialoge wurde zur Unterstützung des Azure-Portal Konzepts der Ressource gruppieren verbessert und Auswahl/erstellen von Azure-Ressourcengruppen und App Service-Pläne werden im neuen Web App und API-App Bereitstellung Dialogfeld dargestellt. 
    - Azure-API App Server Explorer Knoten enthalten Links zu den API-Apps Tiefe Link Azure-Portal als auch andere Funktionen wie Streaming-Protokoll und Remotedebuggen.

    Bekannte Probleme und aktuelle Grenzen in Azure SDK .NET 2.5.1 [dieser](app-service-release-notes.md#known_issues_2_5_1) Abschnitt.


- Neue Features\scenarios Zusammenhang mit **HDInsight-Tools** in Visual Studio werden in dieser Version. 
    - Lokale Überprüfung der Hive-Skripts. Klicken Sie auf Überprüfen Skriptschaltfläche in der Symbolleiste, um festzustellen, ob Fehler in Ihrem Skript. 
    - Verbesserte Struktur Aufträge Debuggen. Sie können Hive-Aufträge jetzt auf Garn Protokolle in Visual Studio debuggen. Wenn die Anwendung Leistungsprobleme, wird GARN Protokolle untersuchen nützliche Informationen...
    - (Public Preview) Schlüsselwort automatische Vervollständigung und IntelliSense unterstützen für Struktur. Zu Hive-Skripts erstellen, HDInsight-Tools für Visual Studio hinzugefügt Schlüsselwort automatische Vervollständigung und IntelliSense für Struktur unterstützen.
    - Storm-Unterstützung. HDInsight-Tools für Visual Studio können jetzt Sturm Topologien/Ausläufe/Schrauben in C# entwickeln. Sie können dann senden entwickelte Topologie Storm-Cluster und Topologie, Bolzen/Schnauze Status. Systemprotokolle und Kunden können Ihre Sturm Topologien/Bolzen/Ausläufe beheben. Sie können auch vorhandene JAVA-Ressourcen in auf HDInsight.
    
    Weitere Informationen finden Sie unter [Erste Schritte mit HDInsight Hadoop Tools für Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).



##<a id="known_issues_2_5_1"></a>Azure SDK für .NET 2.5.1 bekannte Probleme und Grenzen

- Azure API-Apps wird als Bereitstellungsziel für Mobile Apps angezeigt. Web Apps sollte das einzige Ziel für Mobile Apps bis zu einer späteren Version. 
- Bereitstellen von Azure API-App Erfolg führen können zeitweise nicht den Fortschritt im Fenster Azure App Serviceaktivität aktualisieren. Status der neuen Azure-API im Azure-Portal überprüfen kann umgangen werden. 
- Datei > Neues Projekt > API App > F5 Erfahrung führt einen HTTP-Fehler, da gibt es keine default/index.html. Umgangen werden, manuell zu /api/values URL navigieren. 
- Zeitweise werden Server Explorer Symbole abgeflacht. VS Neustart behebt dieses Problem. 
- Wenn eine Ausnahme ausgelöst wird, während der Bereitstellung von Web App oder API-App (oder Kontingent überschritten Fehler Doppelte Azure API Anwendungsname-Gateway), Anzeigen der Fehler raw JSON text 
- Zeitweise-Erstellung bei Anwendung Erkenntnisse zur Erstellung des Projekts überprüft.
- Gelegentlich generierte Azure API-App-Clientcode fehlt Namespaces, müssen manuell enthalten (oder automatisch über Visual Studio Cues importiert) Code kompilieren. 
- Mobile-Anwendung Projekte sollten Web Apps veröffentlicht, aber wählen Sie eine Website erstellten als Mobile Anwendung in Azure-Portal da Mobile App Projekte eine Datenbank erforderlich. 
- Die Startseite für Mobile Apps verwendet den Begriff "mobile Service" statt "mobiler apps" 
- Mobile-Anwendung erstellen kann eine Minute dauern erstellen. 
- Während API App zurückkehrt provisioning (in einigen Fällen) Fehler von Azure API reflektieren, dass die Berechtigungen nicht ordnungsgemäß festgelegt werden konnte, während API-App ordnungsgemäß bereitgestellt und betriebsbereit ist. Sie können Berechtigungen mithilfe der Azure-Portal manuell.
- Application Insights wird auf API App und Mobile App Vorlagen nicht unterstützt.
- API-App-Projekte können nicht zusammen mit Cloud-Dienst-Projekte verwendet werden.
- API App-Projektvorlagen sind nur in C# verfügbar.
- API App Verbrauch Kontextmenü "Azure API App Client hinzufügen" wird nur in C# unterstützt.

 
