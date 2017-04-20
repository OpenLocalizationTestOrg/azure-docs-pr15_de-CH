<properties
    pageTitle="Erste Schritte mit IoT Hub Gateway SDK | Microsoft Azure"
    description="Azure IoT Gateway SDK mithilfe Linux Grundbegriffe erläutern, die Sie verstehen sollten, wenn Sie Azure IoT Gateway SDK verwenden."
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="get-started-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta---get-started-using-linux"></a>Azure IoT Gateway SDK (Beta) - erste Schritte mit Linux

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>Wie das Beispiel erstellen

Bevor Sie beginnen, müssen Sie die [Umgebung einrichten] [ lnk-setupdevbox] für die Arbeit mit dem SDK unter Linux.

1. Öffnen Sie eine Shell.
2. Navigieren Sie zum Stammordner Ihrer lokalen Kopie der **Azure-Iot-Gateway-Sdk** -Repository.
3. Führen Sie das Skript **tools/build.sh** . Dieses Skript verwendet das Dienstprogramm **Cmake** Ordner **Erstellen** im Stammordner Ihrer lokalen Kopie der **Azure-Iot-Gateway-Sdk** Repository erstellen und generieren eine Makefile. Das Skript erstellt die Projektmappe und die Tests ausführt.

> [AZURE.NOTE]  Jedes Mal, wenn das Skript **build.sh** löscht und dann neu **Erstellen** Ordner im Stammordner Ihrer lokalen Kopie der **Azure-Iot-Gateway-Sdk** -Repository.

## <a name="how-to-run-the-sample"></a>Wie das Beispiel ausgeführt

1. Skript **build.sh** erzeugt die Ausgabe im Ordner **Erstellen** der lokalen Kopie von **Azure Iot-Gateway Sdk** -Repository. Dies umfasst zwei Module in diesem Beispiel verwendet.

    Das Skript fügt **liblogger_hl.so** in die **Module/Build/Logger/** Ordner und **libhello_world_hl.so** in der **Module/Build/Hello_world/** Ordner. Verwenden Sie diese Pfade **Modulpfad** Wert wie in der JSON unten dargestellt.

2. Die Datei **hello_world_lin.json** im Ordner " **Samples/Hello_world/Src** " ist eine JSON Einstellungen für Linux, die Sie verwenden können, um das Beispiel auszuführen. Konfigurieren der JSON unten wird vorausgesetzt, dass das Beispiel aus dem Stammverzeichnis Ihrer lokalen Kopie der **Azure-Iot-Gateway-Sdk** Repository ausgeführt wird.

3. Für das Modul **Logger_hl** im Abschnitt **Args** eingestellt **Dateiname** den Namen und Pfad der Datei, die die Daten enthalten.

    Dies ist ein Beispiel für eine JSON-Einstellungsdatei für Linux zum **log.txt** in den Ordner schreiben, in dem Sie das Beispiel ausführen.

    ```
    {
      "modules" :
      [ 
        {
          "module name" : "logger_hl",
          "module path" : "./build/modules/logger/liblogger_hl.so",
          "args" : 
          {
            "filename":"./log.txt"
          }
        },
        {
          "module name" : "hello_world",
          "module path" : "./build/modules/hello_world/libhello_world_hl.so",
          "args" : null
        }
      ],
      "links" :
      [
        {
          "source": "hello_world",
          "sink": "logger_hl"
        }
      ]
    }
    ```

3. Navigieren Sie in der Shell **Azure Iot-Gateway Sdk** -Ordner.
4. Führen Sie den folgenden Befehl ein:
  
  ```
  ./build/samples/hello_world/hello_world_sample ./samples/hello_world/src/hello_world_lin.json
  ``` 

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
