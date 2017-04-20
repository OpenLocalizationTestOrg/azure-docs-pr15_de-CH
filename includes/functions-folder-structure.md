
Der Code für alle Funktionen in eine Funktion app lebt in ein Root-Ordner mit einer Hostkonfigurationsdatei und einem oder mehreren Unterordnern jeweils den Code für eine separate Funktion

```
wwwroot
 | - host.json
 | - mynodefunction
 | | - function.json
 | | - index.js
 | | - node_modules
 | | | - ... packages ...
 | | - package.json
 | - mycsharpfunction
 | | - function.json
 | | - run.csx
```

Die Datei *host.json* enthält eine Runtime-spezifische Konfiguration und befindet sich im Stammverzeichnis der Anwendung Funktion. Informationen über Optionen, die verfügbar sind, finden Sie unter [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) im WebJobs.Script Repository-Wiki.

Jede Funktion hat einen Ordner, der eine oder mehrere Dateien, die function.json Konfiguration und andere enthält.