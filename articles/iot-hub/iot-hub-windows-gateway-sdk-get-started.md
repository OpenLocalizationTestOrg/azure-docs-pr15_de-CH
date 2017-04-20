<properties
    pageTitle="Erste Schritte mit IoT Hub Gateway SDK | Microsoft Azure"
    description="Azure IoT Gateway SDK Exemplarische Vorgehensweise Windows Sie verstehen sollten bei der Verwendung von Azure IoT Gateway SDK Grundbegriffe erläutern."
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta---get-started-using-windows"></a>Azure IoT Gateway SDK (Beta) - erste Schritte mit Windows

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>Wie das Beispiel erstellen

Bevor Sie beginnen, müssen Sie die [Umgebung einrichten] [ lnk-setupdevbox] für das SDK auf Windows.

1. Befehlzeile ein **Entwickler Befehlszeile für VS2015** .
2. Navigieren Sie zum Stammordner Ihrer lokalen Kopie der **Azure-Iot-Gateway-Sdk** -Repository.
3. Ausführen der **Tools\\build.cmd** Skript. Dieses Skript erstellt eine Visual Studio-Projektmappendatei, die Projektmappe erstellt und Tests ausgeführt. Die Visual Studio-Projektmappe im Ordner **Erstellen** in Ihrer lokalen Kopie der **Azure-Iot-Gateway-Sdk** Repository gefunden werden.

## <a name="how-to-run-the-sample"></a>Wie das Beispiel ausgeführt

1. Das **build.cmd** -Skript erstellt Ordner **Erstellen** in der lokalen Kopie des Repository. Dieser Ordner enthält zwei Module in diesem Beispiel verwendet.

    Das Skript fügt **logger_hl.dll** in die **Erstellen\\Module\\Protokollierung\\Debuggen** Ordner und **hello_world_hl.dll** in der **Erstellen\\Module\\Hello_world\\Debuggen** Ordner. Verwenden Sie diese Pfade **Modulpfad** Wert wie in der JSON unten dargestellt.

2. Die Datei **hello_world_win.json** in den **Proben\\Hello_world\\Src** Ordner ist eine JSON Einstellungen für Windows, die Sie verwenden können, um das Beispiel auszuführen. Konfigurieren der JSON unten vorausgesetzt IoT Gateway SDK-Repository in das Stammverzeichnis von Laufwerk **C:** geklont. Wenn Sie an einem anderen Speicherort heruntergeladen, müssen zu **Modulpfad** in der **Beispiele\\Hello_world\\Src\\hello_world_win.json** -Datei entsprechend.

3. Für das Modul **Logger_hl** im Abschnitt **Args** eingestellt **Dateiname** den Namen und Pfad der Datei, die die Daten enthalten.

    Dies ist ein Beispiel für eine JSON-Einstellungsdatei für Windows die Datei **log.txt** im Stammverzeichnis von Laufwerk **C:** geschrieben werden.

    ```
    {
      "modules" :
      [
        {
          "module name" : "logger_hl",
          "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\logger\\Debug\\logger_hl.dll",
          "args" : 
          {
            "filename":"C:\\log.txt"
          }
        },
        {
          "module name" : "hello_world",
          "module path" : "C:\\azure-iot-gateway-sdk\\build\\\\modules\\hello_world\\Debug\\hello_world_hl.dll",
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

3. Navigieren Sie in einer Befehlszeile zum Stammordner der lokalen Kopie des Repository **Azure Iot-Gateway Sdk** .
4. Führen Sie den folgenden Befehl ein:
  
  ```
  build\samples\hello_world\Debug\hello_world_sample.exe samples\hello_world\src\hello_world_win.json
  ```

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md