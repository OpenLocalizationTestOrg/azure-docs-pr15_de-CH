<properties
 pageTitle="Erstellen Sie ein Azure-Funktion app und Azure Storage-Konto | Microsoft Azure"
 description="Azure Funktion app Azure IoT Hub Ereignisse überwacht, verarbeitet die eintreffenden Nachrichten und schreibt sie in Azure-Tabellenspeicher."
 services="iot-hub"
 documentationCenter=""
 authors="shizn"
 manager="timlt"
 tags=""
 keywords=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/21/2016"
 ms.author="xshi"/>

# <a name="31-create-an-azure-function-app-and-azure-storage-account"></a>3.1 erstellen Sie eine Azure Funktion app und Azure Storage-Konto

[Azure-Funktionen](../../articles/azure-functions/functions-overview.md) ist eine Lösung für einfache kleine Codeabschnitte namens "Funktionen" in der Cloud ausgeführt. Eine Azure-Funktion app hostet die Ausführung Ihrer Funktionen in Azure.

## <a name="311-what-will-you-do"></a>3.1.1 Vorgehensweise

Verwenden einer Vorlage Ressourcenmanager Azure Azure Funktion app und Azure Storage-Konto erstellen. Azure Funktion app Azure IoT Hub Ereignisse überwacht, verarbeitet die eintreffenden Nachrichten und schreibt sie in Azure-Tabellenspeicher. Erfüllt Probleme suchen Sie Lösungen [Seite Problembehandlung](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="312-what-will-you-learn"></a>3.1.2 Lernziele

- Wie [Azure Ressourcenmanager](../../articles/azure-resource-manager/resource-group-overview.md) Azure Ressourcen bereitstellen.
- Azure-Funktion Anwendung IoT Hub zu Nachrichten und in einer Tabelle in Azure-Tabellenspeicher zu schreiben.

## <a name="313-what-do-you-need"></a>3.1.3 Was benötigen Sie

- Sie müssen erfolgreich abgeschlossen haben die vorhergehenden Lektionen: [mit Ihrem Himbeeren Pi 3](iot-hub-raspberry-pi-kit-node-get-started.md) und [erstellen Ihre Azure IoT Hub](iot-hub-raspberry-pi-kit-node-get-started.md).

## <a name="314-open-the-sample-app"></a>3.1.4 die Beispiel-app öffnen

Öffnen Sie das Beispielprojekt in Visual Studio Code durch Ausführen der folgenden Befehle:

```bash
cd Lesson3
code .
```

![REPO-Struktur](media/iot-hub-raspberry-pi-lessons/lesson3/repo_structure.png)

- Die `app.js` in der Datei der `app` Unterordner ist der Schlüssel. Diese Datei enthält den Code zum Senden einer Nachricht 20 Mal an IoT Hub und blinkt die LED für jede Nachricht sendet.
- Die `arm-template.json` Datei ist Azure-Ressourcen-Manager-Vorlage mit einer Azure Funktion app und Azure Storage-Konto.
- Die `arm-template-param.json` Datei ist die Konfigurationsdatei von Azure-Ressourcen-Manager-Vorlage verwendet.
- Die `ReceiveDeviceMessages` Unterordner enthält den Node.js Azure-Funktion.

## <a name="315-configure-azure-resource-manager-templates-and-create-resources-in-azure"></a>3.1.5 konfigurieren Sie Azure Ressourcenmanager Vorlagen und erstellen Ressourcen in Azure

Aktualisierung der `arm-template-param.json` -Datei in Visual Studio Code.

![Azure Ressourcenmanager Vorlagenparameter](media/iot-hub-raspberry-pi-lessons/lesson3/arm_para.png)

- Ersetzen Sie **[Ihr IoT Hub-Name]** **{Benutzername Hub}** , die Sie in [Lektion 2](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)angegeben.
- Gewünschte Präfix ersetzen Sie **[Präfixzeichenfolge für neue Ressourcen]** . Das Präfix wird sichergestellt, dass der Ressourcenname eindeutig einen Konflikt zu vermeiden. Verwenden Sie Bindestrich oder im Präfix anfängliche Anzahl.

> [AZURE.NOTE] Sie müssen nicht `azure_storage_connection_string` in diesem Abschnitt. Halten Sie es.

Nach der Aktualisierung der `arm-template-param.json` Datei, die Ressourcen in Azure bereitzustellen, durch Ausführen des folgenden Befehls:

```bash
az resource group deployment create --template-file-path arm-template.json --parameters-file-path arm-template-param.json -g iot-sample -n mydeployment
```

Es dauert etwa fünf Minuten auf um diese Ressourcen zu erstellen. Während der Ressource ausgeführt wurde, können Sie zum nächsten Abschnitt übergehen.

## <a name="316-summary"></a>3.1.6 Zusammenfassung

Sie haben Ihre app Azure Funktion IoT Hub Nachrichten und Azure Storage-Konto diese Nachrichten erstellt. Sie können übergehen zum nächsten Abschnitt zum Bereitstellen und Ausführen des Beispiels in Pi Gerät Cloud-Nachrichten senden.

## <a name="next-steps"></a>Nächste Schritte

[3.2 führen Sie beispielanwendung Gerät Cloud-Nachrichten auf Ihrem Himbeeren Pi 3 aus](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md)

