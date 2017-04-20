<properties
    pageTitle="Best Practices für Azure App Service"
    description="Lernen Sie bewährte Methoden und Problembehandlung bei Azure App Service."
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/30/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="best-practices-for-azure-app-service"></a>Best Practices für Azure App Service

Dieser Artikel beschreibt die empfohlenen Vorgehensweisen für [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). 

## <a name="colocation"></a>Colocation
Wenn eine Lösung wie eine Webanwendung und einer Datenbank verfassen Azure Ressourcen in unterschiedlichen Regionen befinden können Effekte Folgendes enthalten:

*  Latenzzeit zwischen Ressourcen
*  Monetäre Gebühren für ausgehende Daten übertragen Cross-Region auf [Azure Preisseite](https://azure.microsoft.com/pricing/details/data-transfers).

Colocation in derselben Region ist Azure Ressourcen Verfassen einer Lösung wie eine Webanwendung und einer Datenbank oder Speichergruppe Konto Inhalte oder Daten verwendet. Wenn Sie Ressourcen erstellen, die sollten Sie sicherstellen, sind in derselben Azure-Region, wenn Sie bestimmte Unternehmen oder Grund nicht zu entwerfen. Durch Nutzung der [App Service cloning-Funktion](app-service-web-app-cloning-portal.md) derzeit apps Premium App Service-Plan können Sie eine App Service-app auf den Bereich der Datenbank verschieben.   

## <a name="memoryresources"></a>Wenn apps verbrauchen mehr Speicher als erwartet
Bemerken Sie eine app benötigt mehr Speicher als erwartet Überwachung angegeben oder Service Vorschläge berücksichtigen [App Service Selbstkonfiguration Feature](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites). Eine der Optionen für die automatische Korrektur-Funktion nimmt ein Speichergrenzwert Grundlage benutzerdefinierte Aktionen. Aktionen umfassen das Spektrum von Benachrichtigungen Untersuchung über Speicherabbild, vor Ort zur Risikominderung durch Wiederverwendung des Arbeitsprozesses. Automatische Reparatur kann über web.config und eine benutzerfreundliche Benutzeroberfläche gemäß in diesem Blogbeitrag [App Service Support Site Erweiterung](https://azure.microsoft.com/blog/additional-updates-to-support-site-extension-for-azure-app-service-web-apps)konfiguriert werden.   

## <a name="CPUresources"></a>Wenn apps verbrauchen mehr CPU als erwartet
Wenn Sie feststellen, dass eine Anwendung verbraucht mehr CPU als erwartet oder es treten wiederholt CPU-Spitzen Überwachung angegeben oder Service Vorschläge sollten Sie Heraufskalierung und Breitenskalierung App Service-Plan. Die Anwendung ist zustandsbehaftete, Skalierung die Option nur während die Anwendung ist statusfrei, Skalierung aus mehr Flexibilität und höhere Skalierung wird Ihnen. 

Weitere Informationen zu "zustandsbehaftete" Vs "stateless" Sie können dieses Video: [Planen einer skalierbaren End-to-End Multi-Tier-Anwendung auf Microsoft Azure Web App](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B414#fbid=?hashlink=fbid). Weitere Informationen über App Service skalieren und skalieren: [Skalierung Web App in Azure App Service](web-sites-scale.md).  

## <a name="socketresources"></a>Wenn der Socket Ressourcen erschöpft
Eine häufige Ursache für erschöpfen ausgehenden TCP-Verbindungen ist die Verwendung Clientbibliotheken nicht implementierten TCP-Verbindungen wiederverwenden oder wenn ein Protokoll höherer Ebene wie HTTP - Keep-Alive nicht genutzt. Lesen Sie die Dokumentation für jedes der Bibliotheken auf die apps in Ihrem App-Dienst darauf konfiguriert oder im Code für die effiziente Wiederverwendung von Ausgeh zugegriffen. Folgen Sie Library-Dokumentation Anleitung für die ordnungsgemäße Erstellung und Freigabe oder Bereinigung zu Verbindungen undicht auch. Während Client Bibliotheken Untersuchungen werden möglicherweise Fortschritt abgemildert werden durch dezentrales Skalieren, mehrere Instanzen.  

## <a name="appbackup"></a>Beim Sichern Ihrer app starten fehlgeschlagen
Zwei häufigsten Gründe, warum app backup fehlgeschlagen: Ungültige Einstellungen und Ungültige Datenbankkonfiguration. Diese Ausfall in der Regel treten Änderungen auf Storage-Datenbank, oder zum Zugriff auf diese Ressourcen (z. B. Anmeldeinformationen für die Datenbank in den Sicherungsdateien ausgewählten aktualisiert). Backups werden normalerweise nach einem Zeitplan ausgeführt und benötigen Zugriff auf Speicher (Ausgabe der gesicherten Dateien) und Datenbanken (zum Kopieren und Lesen in der Sicherung enthalten sein). Mangelnde Ressourcen zugreifen würden konsistente backup-Fehler zurückzuführen. 

Bei der Sicherung Fehler auftreten, überprüfen Sie aktuelle Ergebnisse verstehen, welche Art von Fehler. Überprüfen Sie bei Storage Access Fehler und aktualisieren Sie die Sicherungskonfiguration verwendete Speicher-Einstellung. Überprüfen Sie bei Datenbankfehlern Zugriff und aktualisieren Sie die Verbindungszeichenfolgen als Teil des app-Einstellungen; Anschließend aktualisieren die Sicherungskonfiguration auf ordnungsgemäß erforderlichen Datenbanken. Weitere Informationen zu app-Sicherung finden Sie unter [Sichern Sie eine Webanwendung in Azure App Service](web-sites-backup.md) -Dokumentation.

## <a name="nodejs"></a>Wenn neue Node.js-apps in Azure App Service bereitgestellt werden
Azure App Service Standardkonfiguration für Node.js apps soll am besten der Bedürfnissen der am häufigsten verwendeten apps. Wenn Konfiguration für Ihre Node.js personalisierte tuning Leistung oder Ressourceneinsatz für CPU, Speicher/Netzwerk-Ressourcen profitieren würden, könnte Sie unseren best Practices und Schritte zur Problembehandlung überprüfen. Dokumentation beschrieben Iisnode Settings, die verschiedenen Szenarien beschreibt oder Fragen Ihre app möglicherweise werden, und zeigt, wie diese Probleme möglicherweise für Ihre Node.js müssen: [Best Practices und Fehlerbehebung für Knoten Applikationen auf Azure App Service](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md).   


