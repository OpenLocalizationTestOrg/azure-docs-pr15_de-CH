<properties 
    pageTitle="Azure Webaufträge Dokumentationsressourcen" 
    description="Empfohlene Ressourcen wie Azure Webaufträge und Azure Webaufträge SDK verwenden." 
    services="app-service" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="tdykstra"/>

# <a name="azure-webjobs-documentation-resources"></a>Azure Webaufträge Dokumentationsressourcen

## <a name="overview"></a>Übersicht

Dieses Thema enthält Links zu Dokumentationsressourcen zur Verwendung von Azure Webaufträge und Azure Webaufträge SDK. Azure Webaufträge bieten eine einfache Möglichkeit zum Ausführen von Skripts oder Programmen als Hintergrundprozesse im Kontext einer [App Service WebApp, API-app oder mobile app](../app-service/app-service-value-prop-what-is.md). Laden und Ausführen eine ausführbare Datei z. B. als Cmd, Bat, Exe (.NET), ps1, sh, Php Py, Js und. Diese Programme führen als Webaufträge Zeitplan (Cron) oder permanent.

[Webaufträge SDK](websites-webjobs-resources.md) soll die Code für häufige Aufgaben schreiben, ein Webauftrag können, wie Bild-, Verarbeitung in die Warteschlange, RSS-Aggregation, Wartung und e-Mails zu vereinfachen. Das WebJobs SDK enthält Funktionen für die Arbeit mit Azure Service Bus für Planungsaufgaben und Fehlerbehandlung und viele andere Szenarien. Ferner ist erweiterbar konzipiert und gibt [Quell-Repository für Erweiterungen zu öffnen](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview). [Azure Funktionen](../azure-functions/functions-overview.md) (derzeit in Vorschau) eine WebJobs SDK-Version basiert, die mit C# Skript Node.js und anderen Sprachen. 

Erstellen, bereitstellen und Verwalten von Webaufträge ist nahtlos mit integrierten Tools in Visual Studio. Webaufträge aus Vorlagen erstellen, veröffentlichen und verwalten (Run/Stop/Monitor/Debug) werden. 

Webaufträge Dashboard im Azure-Portal bietet leistungsfähige Management-Funktionen, die vollständige Kontrolle über die Ausführung der Webaufträge, einschließlich der Möglichkeit, einzelne Funktionen in Webaufträge aufrufen geben. Das Dashboard zeigt Funktion Laufzeiten und Protokollausgabe. 

##<a name="getstarted"></a>Erste Schritte mit WebJobs und WebJobs-SDK

* [Einführung in Azure Webaufträge](http://www.hanselman.com/blog/IntroducingWindowsAzureWebJobs.aspx)
* [Azure Webaufträge sind und Sie sollte jetzt mit!](http://www.troyhunt.com/2015/01/azure-webjobs-are-awesome-and-you.html) (Blogbeitrag Troy Hunt.)
* [Azure Webaufträge Features](/blog/2014/10/22/webjobs-goes-into-full-production/)
* [Was ist das WebJobs SDK](websites-dotnet-webjobs-sdk.md)
* [Hintergrund Aufträge Leitfäden von Microsoft Patterns and Practices](/documentation/articles/best-practices-background-jobs/)
* [Ankündigung der 1.1.0 RTM Microsoft Azure Webaufträge SDK](/blog/azure-webjobs-sdk-1-1-0-rtm/)
* [Erste Schritte mit Azure Webaufträge SDK](websites-dotnet-webjobs-sdk-get-started.md)
* [Verwendung von Azure Warteschlangenspeicher mit WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)
* [Verwendung von Azure BLOB-Speicher mit WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)
* [Verwendung von Azure-Tabellenspeicher mit WebJobs SDK](websites-dotnet-webjobs-sdk-storage-tables-how-to.md)
* [Verwendung von Azure Service Bus mit WebJobs SDK](websites-dotnet-webjobs-sdk-service-bus.md)
* [Verwendung von WebHooks mit WebJobs SDK mit Beispielen für GitHub, IFTTT und HTTP](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/WebHooks-Walkthrough)
* [Azure Webaufträge SDK Kurzübersicht (PDF-Download)](http://go.microsoft.com/fwlink/?LinkID=524028&clcid=0x409)
* [Webaufträge Einstellungen Dokumentation in GitHub](https://github.com/projectkudu/kudu/wiki/Web-jobs).
* Videos
    * [Webaufträge und Webaufträge SDK](http://channel9.msdn.com/Shows/Cloud+Cover/Episode-153-WebJobs-with-Pranav-Rastogi?utm_source=dlvr.it&utm_medium=twitter)
    * [Azure Webaufträge Videoserie auf Channel 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)
    * [Einführung in Webaufträge für Visual Studio](http://channel9.msdn.com/Shows/Web+Camps+TV/Introducing-WebJobs-Tooling-for-Visual-Studio-with-Brady-Gaster) 
    * [Webaufträge Werkzeuge und Remotedebuggen](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster)
    * [Pranav Rastogi - Version 1.1 Erweiterbarkeit Azure Webaufträge aktualisieren](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-183-Azure-WebJobs-Update-with-Pranav-Rastogi)

Siehe auch in den folgenden Abschnitten [Webaufträge bereitstellen](#deploy) und [Testen und Debuggen Webaufträge](#debug).

##<a name="deploy"></a>Webaufträge bereitstellen

* [Wie bereitstellen Azure Webaufträge mit Visual Studio](websites-dotnet-deploy-webjobs.md)
* [Webaufträge mithilfe der Azure-Portal bereitstellen](web-sites-create-web-jobs.md)
* [Befehlszeilen- oder kontinuierliche Bereitstellung von Azure Webaufträge aktivieren](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/)
* [Bereitstellen einer .NET Konsolenanwendung in Azure mit Webaufträge Git](http://blog.amitapple.com/post/73574681678/git-deploy-console-app/)
* [F#-Webauftrags Bereitstellen in Azure](http://blogs.msdn.com/b/dave_crooks_dev_blog/archive/2015/02/18/deploying-f-web-job-to-azure.aspx)
* [Bereitstellen von benutzerdefinierten Diensten als Azure Webaufträge](http://withouttheloop.com/articles/2015-06-23-deploying-custom-services-as-azure-webjobs/)
* Videos
    * [Einführung in Webaufträge für Visual Studio](http://channel9.msdn.com/Shows/Web+Camps+TV/Introducing-WebJobs-Tooling-for-Visual-Studio-with-Brady-Gaster) 
    * [Webaufträge Werkzeuge und Remotedebuggen](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster) 

##<a name="schedule"></a>Webaufträge planen

* [Die Azure Webauftrags Dialogfeld hinzufügen](websites-dotnet-deploy-webjobs.md#configure)
* [Erstellen einer geplanten Webauftrags in Azure-Portal](web-sites-create-web-jobs.md#CreateScheduled)
* [Steuerprogrammauftrag ein Webauftrag einbinden](http://blog.davidebbo.com/2015/05/scheduled-webjob.html)
* [Planen von Azure Webaufträge mit Cron-Ausdrücke](http://blog.amitapple.com/post/2015/06/scheduling-azure-webjobs/)
* [Planungsfunktionen einzelne Webauftrag mit Webaufträge SDK TimerTrigger](websites-dotnet-webjobs-sdk.md#schedule)

##<a name="debug"></a>Testen und Debuggen Webaufträge

* [Neue Entwickler und Features für Azure Webaufträge in Visual Studio debuggen](http://blogs.msdn.com/b/webdev/archive/2014/11/12/new-developer-and-debugging-features-for-azure-webjobs-in-visual-studio.aspx)
* [Webaufträge Dashboard anzeigen](websites-dotnet-webjobs-sdk-get-started.md#view-the-webjobs-sdk-dashboard)
* [Zum Schreiben von Protokollen mit WebJobs SDK und im Dashboard anzeigen](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs)
* [Remote debugging Webaufträge](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebugwj)
* [Wer hat dieses Blob?](http://blogs.msdn.com/b/jmstall/archive/2014/02/19/who-wrote-that-blob.aspx) 
* [Interaktive Hostcode in der Cloud](http://blogs.msdn.com/b/jmstall/archive/2014/04/26/hosting-interactive-code-in-the-cloud.aspx)
* [Azure Webaufträge Spur hinzufügen](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx)
* [Überwachung, diagnose und Problembehandlung von Microsoft Azure-Speicher](../storage/storage-monitoring-diagnosing-troubleshooting.md)
* Videos
    * [Webaufträge Werkzeuge und Remotedebuggen](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster) 

##<a name="scale"></a>Webaufträge skalieren

* [Skalieren Sie Ihre Webanwendung mit Azure Websites](http://msdn.microsoft.com/magazine/dn786914.aspx)
* [Azure App Service: Architektur massiv Ready For Business webapps](https://channel9.msdn.com/Events/Build/2014/3-626). Webapps mit Webaufträge, einschließlich WebJobs SDK Skalierung behandelt.
* Videos
    * [Webaufträge skalieren](http://channel9.msdn.com/Shows/Azure-Friday/Azure-WebJobs-105-Scaling-out-Web-Jobs)

##<a name="additional"></a>Zusätzliche Webaufträge Ressourcen

* [Azure Webaufträge GA Blogbeitrag Magnus Mårtensson](http://magnusmartensson.com/azure-webjobs-ga)
* [Powershell Webaufträge auf Azure App Service ausgeführt](http://blogs.msdn.com/b/nicktrog/archive/2014/01/22/running-powershell-web-jobs-on-azure-websites.aspx)
* [Benachrichtigung auslösen der Azure Webaufträge abgeschlossen](http://blog.amitapple.com/post/2014/03/webjobs-notification/)
* [Einfache Web App Backup Aufbewahrungsrichtlinie mit Webaufträge](https://azure.microsoft.com/blog/2014/04/28/simple-web-site-backup-retention-policy-with-webjobs/)
* [Azure webapps und Cloud-Dienste bei der ersten Anforderung langsam](http://wp.sjkp.dk/windows-azure-websites-and-cloud-services-slow-on-first-request/). Veranschaulicht, wie Webaufträge AlwaysOn-Feature simuliert, das nur für die Standard Tarif verfügbar ist.
* [Webaufträge heruntergefahren](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.U72Il_5OWUl). Für Webaufträge SDK ordnungsgemäßes Herunterfahren [ordnungsgemäßes Herunterfahren](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful)angezeigt.)
* [Automatisierte Backups mit Azure Webaufträge & AzCopy](http://markjbrown.com/azure-webjobs-azcopy/)
* Videos
    * [Magnus Mårtensson Azure Webaufträge videos](https://www.youtube.com/playlist?list=PLqp1ZOYYUSd81yEzMYLTw8cz91wx_LU9r)
    * [Azure Webaufträge Videoserie auf Channel 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)

##<a name="additionalsdk"></a>Zusätzliche WebJobs SDK-Ressourcen

* [Webaufträge SDK-Versionsinformationen](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes)
* [Webaufträge SDK-Quellcode](https://github.com/Azure/azure-webjobs-sdk)
* [Webaufträge SDK Extensions Quellcode](https://github.com/Azure/azure-webjobs-sdk-extensions)mit [ausführlicher Leitfaden zum Erweiterbarkeitsmodell](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).  
* [Webaufträge SDK Script-Quellcode](https://github.com/Azure/azure-webjobs-sdk-script/) (für [Azure](../azure-functions/functions-overview.md)verwendet)
* [Webauftrags Azure-Speicher mithilfe des SDK WebJobs FREB Dateien hochladen](http://thenextdoorgeek.com/post/WAWS-WebJob-to-upload-FREB-files-to-Azure-Storage-using-the-WebJobs-SDK)
* [Hosting außerhalb Azure Azure Webaufträge Webauftrag mit Protokollierung profitiert ein Azure gehostet](http://bypassion.dk/?p=510)
* [Erstellen eines Import-Tools mit Azure Webaufträge](http://www.freshconsulting.com/building-data-import-tool-azure-webjobs/)
* [Übersicht über Azure Funktionen](../azure-functions/functions-overview.md)
* Videos
    * [Azure Webaufträge Videoserie auf Channel 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)

##<a name="samples"></a>Webauftrags Anwendungsbeispiele

* [Anwendungsbeispiele Webaufträge Teams auf GitHub](https://github.com/azure/azure-webjobs-sdk-samples)
* [Einfache Azure Web App WebJobs Backend mit WebJobs SDK](http://code.msdn.microsoft.com/Simple-Azure-Website-with-b4391eeb)
* [SiteMonitR](http://code.msdn.microsoft.com/SiteMonitR-dd4fcf77). Geplante und ereignisgesteuerte Webaufträge veranschaulicht. Finden Sie im Blogbeitrag [Azure Webaufträge SDK mit SiteMonitR neu](http://www.bradygaster.com/post/rebuilding-the-sitemonitr-using-windows-azure-webjobs).

##<a name="blogs"></a>Blogs

* [Azure-blog](/blog)
* [Amit Apple Blog](http://blog.amitapple.com/). Konzentriert sich auf Webaufträge (nicht im SDK).
* [Magnus Mårtensson blog](http://magnusmartensson.com/)

##<a name="gethelp"></a>Hilfe zu Webaufträge

* [StackOverflow für Webaufträge](http://stackoverflow.com/questions/tagged/azure-webjobs)
* [StackOverflow Webaufträge SDK](http://stackoverflow.com/questions/tagged/azure-webjobssdk)
* [StackOverflow Azure Funktionen](http://stackoverflow.com/questions/tagged/azure-functions)
* [Azure und ASP.NET forum](http://forums.asp.net/1247.aspx)
* [Azure App Service Web Apps-forum](http://social.msdn.microsoft.com/Forums/azure/home?forum=windowsazurewebsitespreview)
* [Azure Web Apps Benutzer Voice-Website](https://feedback.azure.com/forums/169385-websites/)
* [Twitter](http://twitter.com/). Verwenden Sie Hashtag #AzureWebJobs.
* [Webaufträge Fehler oder Problem melden](https://github.com/projectkudu/kudu/wiki/Reporting-WebJobs-issues)

