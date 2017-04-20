<properties
   pageTitle="Lokal überwachen und diagnose-Services mit Azure Service geschrieben | Microsoft Azure"
   description="Informationen Sie zum Überwachen und analysieren Ihre Dienste mit Microsoft Azure Service Fabric auf einem lokalen Entwicklungscomputer geschrieben."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/24/2016"
   ms.author="subramar"/>


# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a>Überwachung und diagnose-Services in einer lokalen Entwicklung Installation


> [AZURE.SELECTOR]
- [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
- [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)

Überwachung, Erkennung, Diagnose und Problembehandlung ermöglichen für Dienste weiterhin mit minimaler Unterbrechung für den Anwender. Überwachung und Diagnose sind bereitgestellten tatsächlichen unabdingbar. Annahme ein ähnliches Modell während der Entwicklung von Diensten gewährleistet, dass beim Wechsel zu einer Diagnose Pipeline funktioniert. Fabric Service erleichtert Dienstentwickler Diagnose implementieren, die einem einzelnen Computer lokale Entwicklung Setups und tatsächlichen Produktion Cluster Setups arbeiten können.


## <a name="debugging-service-fabric-java-applications"></a>Debuggen von Service Fabric Java

Für Java-Programme sind [mehrere Protokollierung Frameworks](http://en.wikipedia.org/wiki/Java_logging_framework) verfügbar. Da `java.util.logging` ist die Standardoption mit der JRE dient auch [Codebeispiele in Github](http://github.com/Azure-Samples/service-fabric-java-getting-started).  Folgenden erläutert das Konfigurieren der `java.util.logging` Framework. 
 
Mit java.util.logging können Sie die Anwendungsprotokolle Speicher, Ausgabe, Dateien oder Sockets umleiten. Für jede dieser Optionen sind bereits in Framework bereitgestellten Standardhandler. Erstellen Sie eine `app.properties` Datei Dateihandler für Ihre Anwendung alle Protokolle in eine lokale Datei umleiten zu konfigurieren. 

Der folgende Codeausschnitt enthält eine Beispielkonfiguration: 

```java 
handlers = java.util.logging.FileHandler
 
java.util.logging.FileHandler.level = ALL
java.util.logging.FileHandler.formatter = java.util.logging.SimpleFormatter
java.util.logging.FileHandler.limit = 1024000
java.util.logging.FileHandler.count = 10
java.util.logging.FileHandler.pattern = /tmp/servicefabric/logs/mysfapp%u.%g.log             
```

Der Ordner, auf die die `app.properties` Datei muss vorhanden sein. Nach der `app.properties` erstellt wird, müssen Sie auch das Skript Eintrag Punkt ändern `entrypoint.sh` in die `<applicationfolder>/<servicePkg>/Code/` Ordner zum Festlegen der Eigenschaft `java.util.logging.config.file` , `app.propertes` Datei. Der Eintrag sieht wie im folgenden Codeausschnitt:

```sh 
java -Djava.library.path=$LD_LIBRARY_PATH -Djava.util.logging.config.file=<path to app.properties> -jar <service name>.jar
```
 
 
Diese Konfiguration führt Protokolle rotierenden Weise zur gesammelten `/tmp/servicefabric/logs/`. **%U** und **%g** ermöglichen Weitere Dateien mit Dateinamen mysfapp0.log, mysfapp1.log usw. erstellen. Standardmäßig ist kein Handler explizit konfiguriert, ist Console registriert. Eine kann die Protokolle in Syslog unter /var/log/syslog anzeigen.
 
Weitere Informationen finden Sie in den [Codebeispielen in Github](http://github.com/Azure-Samples/service-fabric-java-getting-started).  



## <a name="next-steps"></a>Nächste Schritte
Zur Anwendung hinzugefügt, die dieselbe Ablaufverfolgungscode funktioniert auch mit Ihrer Anwendung in Azure Cluster Diagnostics. Lesen Sie diesen Artikel, besprechen Sie die verschiedenen Optionen für die Tools, die beschreiben, wie Sie sie einrichten können.
* [Sammeln Sie mit Azure-Diagnose](service-fabric-diagnostics-how-to-setup-lad.md)
