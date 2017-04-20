<properties 
    pageTitle="Verwenden der PM2 Konfiguration für NodeJS im webapps unter Linux | Microsoft Azure" 
    description="Verwenden der PM2-Konfiguration für NodeJS im webapps unter Linux" 
    keywords="Azure app Service WebApp Nodejs, pm2, Linux, Betriebssysteme"
    services="app-service" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="naziml"/>

# <a name="using-pm2-configuration-for-nodejs-in-web-apps-on-linux"></a>Verwenden der PM2-Konfiguration für Node.js in Web-Apps unter Linux

Anwendungsstapel, Node.js für Web-Apps unter Linux festlegen, erhalten Sie die Möglichkeit, eine Startdatei Node.js, wie in der folgenden Abbildung dargestellt.

![][1]

Hiermit können Sie entweder

-   Geben Sie das Startskript für Ihre Node.js (Beispiel: /bin/server.js)
-   Angeben die PM2-Konfigurationsdatei für Ihre Node.js verwenden (z.B.: /foo/process.json)

 >[AZURE.NOTE] Wenn der Knoten Prozesse automatisch neu gestartet, wenn bestimmte Dateien geändert werden soll, müssen Sie PM2 verwenden. Andernfalls wird die Anwendung kein Neustart empfängt Benachrichtigungen von z. B. fortlaufende Bereitstellung Anwendungscode ändert.

Sie können die Node.js- [Datei Prozessdokumentation](http://pm2.keymetrics.io/docs/usage/application-declaration/) für alle Optionen, aber unten ist ein Beispiel für welche als process.json-Datei Verwendung

        {
          "name"        : "worker",
          "script"      : "/bin/server.js",
          "instances"   : 1,
          "merge_logs"  : true,
          "log_date_format" : "YYYY-MM-DD HH:mm Z",
          "watch": ["/bin/server.js", "foo.txt"],
          "watch_options": {
            "followSymlinks": true,
            "usePolling"   : true,
            "interval"    : 5
          }
        }

Beachten Sie bei dieser Konfiguration stehen 

-   Die Eigenschaft "Script" gibt die Anwendung Start Skript.
-   Eigenschaft "Instanzen" gibt an, wie viele Instanzen des Prozesses Knoten gestartet. Wenn Sie Ihre Anwendung auf größere VM, die mehrere Kerne ausführen, möchten Sie Ihre Ressourcen maximieren, indem Sie hier einen höheren Wert festlegen.
-   "Überwachung" Array gibt alle Dateien, deren Änderung Sie Ihre Prozesse Knoten neu starten möchten.
-   Für "Watch_options" müssen Sie derzeit "UsePolling" als True aufgrund der angeben, die Ihre Anwendungsinhalt bereitgestellt wird.


## <a name="next-steps"></a>Nächste Schritte ##

* [Was ist App auf Linux?](./app-service-linux-intro.md)

<!--Image references-->
[1]: ./media/app-service-linux-using-nodejs-pm2/nodejs-startup-file.png