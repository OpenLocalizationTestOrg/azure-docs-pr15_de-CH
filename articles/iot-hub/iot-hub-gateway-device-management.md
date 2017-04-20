<properties
 pageTitle="Verwaltete Geräte hinter einem Gateway IoT aktivieren | Microsoft Azure"
 description="Anleitung Thema über einen IoT Gateway erstellt mit IoT Gateway SDK mit Geräten IoT Hub geleitet."
 services="iot-hub"
 documentationCenter=""
 authors="chipalost"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="04/29/2016"
 ms.author="cstreet"/>
 
# <a name="enable-managed-devices-behind-an-iot-gateway"></a>Verwaltete Geräte hinter einem Gateway IoT aktivieren

## <a name="basic-device-isolation"></a>Grundgerät isolation

Organisationen verwenden häufig IoT-Gateways, die allgemeine Sicherheit IoT Lösungen. Einige Geräte zum Senden von Daten in die Cloud müssen jedoch nicht schützen Sie sich vor Angriffen im Internet. Diese Geräte von externen Threads kann vermieden werden, indem sie die Kommunikation mit der Außenwelt über ein Gateway.

Das Gateway befindet sich zwischen Sicherheit und offenen Internet. Geräte zum Gateway sprechen und das Gateway überträgt Nachrichten entlang zum richtigen Cloud-Ziel. Das Gateway gehärtet gegen externe Threads, verhindert nicht autorisierte Anfragen autorisierte eingehende Datenverkehr und leitet, eingehende Datenverkehr an das richtige Gerät.

![][1]

Sie können auch Geräte platzieren, die sich hinter einem Gateway für zusätzliche Sicherheit schützen können. In diesem Szenario müssen Sie nur das Gateway OS Patch gegen die neuesten Sicherheitslücken statt aktualisieren das Betriebssystem auf jedem Gerät.

## <a name="isolation-plus-intelligence"></a>Isolierung und Intelligenz

Gesicherte Router ist ein ausreichend Gateway Geräte einfach zu isolieren. Allerdings erfordern IoT-Lösungen häufig ein Gateway mehr Energie als einfach isolieren Geräte bietet. Sie möchten beispielsweise Ihre Geräte aus der Cloud. Sie sind können mit [LWM2M](https://github.com/OpenMobileAlliance/OMA_LwM2M_for_Developers/wiki), ein standard-Device Management-Protokoll für die Cloud Management-Teil der Lösung. Die Geräte senden jedoch Telemetrie mit nicht TCP-Protokoll aktiviert. Darüber hinaus die Geräte erzeugen große Datenmengen und nur eine gefilterte Teilmenge der Telemetriedaten hochladen möchten. Eine Lösung erstellen, die einen Gateway IoT kann mit zwei unterschiedlichen Daten enthält. Das Gateway sollten:

-   Verstehen Sie der **Telemetrie**filtern Sie und in der Cloud über das Gateway hochladen. Das Gateway ist nicht mehr ein einfacher Router, der einfach Daten zwischen dem Gerät und der Cloud weiterleitet.

-   Einfach Exchange **LWM2M Gerätedaten** zwischen den Geräten und der Cloud. Das Gateway muss nicht die Daten in diese und muss nur sicherstellen, dass Daten hin-und zwischen den Geräten und der Cloud weitergeleitet.

Die folgende Abbildung zeigt dieses Szenario:

![][2]

## <a name="the-solution-azure-iot-device-management-and-the-iot-gateway-sdk"></a>Die Lösung: Azure IoT Device-Management und IoT Gateway SDK 

Die öffentliche Vorabversion von [Azure IoT Device Management] [ lnk-device-management] und Beta-Version von [Azure IoT Gateway SDK] dieses Szenario. Das Gateway behandelt jeden Datenstrom wie folgt:

-   **Telemetrie**: können IoT Gateway SDK eine Pipeline erstellen, versteht, Filter und sendet Daten in der Cloud. IoT Gateway SDK enthält Code, der Teile der Pipeline für den Entwickler implementiert. Finden Sie weitere Informationen über die Architektur des SDK im [IoT Gateway SDK - Einstieg] [ lnk-gateway-get-started] Tutorial.

-   **Device Management**: Azure Device-Management bietet einen LWM2M-Client, der das Gerät sowie eine Cloud-Schnittstelle für das Ausstellen von Befehlen auf dem Gerät ausgeführt wird.
    
    Sie erfordern keine spezielle Logik auf dem Gateway, da nicht die LWM2M Daten zwischen dem Gerät und IoT Hub. Sie können ICS, Bestandteil zahlreicher moderner Betriebssysteme Gateways ermöglichen den Austausch von LWM2M Daten. Sie können eine geeignete Betriebssystem für dieses Szenario SDK Gateway IoT verschiedener Betriebssysteme unterstützt. Anleitung zum Aktivieren von ICS auf [Windows 10] und [Ubuntu], zwei viele Betriebssysteme werden

Die folgende Abbildung zeigt die Architektur zum Aktivieren dieses Szenarios mithilfe von [Azure IoT Device Management] [ lnk-device-management] und [Azure IoT Gateway SDK].

![][3]

## <a name="next-steps"></a>Nächste Schritte

Zur Verwendung von IoT Gateway SDK finden Sie unter die folgenden Lernprogramme:

- [IoT Gateway SDK - erste Schritte mit Linux][lnk-gateway-get-started]
- [IoT Gateway SDK-Nachrichten Gerät Cloud mit einem simulierten Gerät mit Linux][lnk-gateway-simulated]

Weitere Funktionen des IoT Hub kann, finden Sie unter:

- [Das Entwicklerhandbuch][lnk-devguide]
- [Ein SDK Gateway IoT simulieren][lnk-gateway-simulated]

<!-- Images and links -->
[1]: media/iot-hub-gateway-device-management/overview.png
[2]: media/iot-hub-gateway-device-management/manage.png
[Azure IoT Gateway SDK]: https://github.com/Azure/azure-iot-gateway-sdk/
[Windows 10]: http://windows.microsoft.com/en-us/windows/using-internet-connection-sharing#1TC=windows-7
[Ubuntu]: https://help.ubuntu.com/community/Internet/ConnectionSharing
[3]: media/iot-hub-gateway-device-management/manage_2.png
[lnk-gateway-get-started]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-gateway-simulated]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-device-management]: iot-hub-device-management-overview.md

[lnk-devguide]: iot-hub-devguide.md