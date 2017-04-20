<properties
 pageTitle="Azure-Lösungen für Internet der Dinge | Microsoft Azure"
 description="Eine Übersicht über IoT Azure sowie ein Beispiel Lösungsarchitektur Beziehung Azure IoT Hub, Geräte-SDKs und vorkonfigurierte Lösung"
 services="iot-hub"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/05/2016"
 ms.author="dobett"/>

[AZURE.INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## <a name="next-steps"></a>Nächste Schritte

Azure IoT Hub ist ein Azure Service sichere ermöglicht und zuverlässige bidirektionale Kommunikation zwischen der Anwendung Backend und Millionen von Geräten. Sie können die Anwendung Back-End:

- Die Geräte erhalten Sie Telemetrie Ebene.
- Weiterleitung von Daten von Geräten zu einem Stream Ereignisprozessor.
- Empfangen Sie Datei-Uploads von Geräten.
- Cloud-Gerät Befehle bestimmter Geräte

IoT Hub können Sie eigene Backend Lösung implementieren. IoT Hub zudem eine geräteidentitätsregistrierung Geräte, deren Anmeldeinformationen und ihre Rechte an den Hub anschließen bereitgestellt. Erfahren Sie mehr über IoT Hub finden Sie unter [Neuigkeiten IoT Hub?] [lnk-iot-hub].

Wie ermöglicht Azure IoT Hub standardbasierte IoT Device Management für Sie remote verwalten, konfigurieren und Aktualisieren Ihrer Geräte finden Sie unter [Übersicht über Azure IoT Hub Device-Management][lnk-device-management].

Zum Implementieren von Clientanwendungen auf einer Vielzahl von Hardware-Plattformen und Betriebssystemen können Sie das Gerät IoT SDKs. IoT Gerät SDKs sind Bibliotheken, die sendende Telemetrie IoT Hub und Cloud-zu-Gerät-Befehle ermöglichen. Bei Verwendung der SDKs können Sie mehrere Netzwerkprotokolle mit IoT Hub kommunizieren auswählen. Weitere [Informationen zum Geräte-SDKs]finden Sie[lnk-device-sdks].

Zunächst Code schreiben und Ausführen von Beispielen finden Sie [Erste Schritte mit IoT Hub] [ lnk-getstarted] Tutorial.

Möglicherweise auch Interesse an [Azure IoT Suite][lnk-iot-suite], eine Sammlung von vorkonfigurierten Lösungen. IoT Suite können Sie schnell loslegen und IoT Projekte häufig IoT - remote-Überwachung, Ressourcenmanagement und vorbeugende Wartung Szenarios.

[lnk-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-iot-hub]: iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-iotdev]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-overview.md