<properties
   pageTitle="Azure IoT Protokollgateway | Microsoft Azure"
   description="Beschreibt das Azure IoT Protokollgateway zum Erweitern der Funktionen und Protokoll Azure IoT Hub."
   services="iot-hub"
   documentationCenter=""
   authors="kdotchkoff"
   manager="timlt"
   editor=""/>

<tags
   ms.service="iot-hub"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/23/2016"
   ms.author="kdotchko"/>

# <a name="supporting-additional-protocols-for-iot-hub"></a>Unterstützung zusätzlicher Protokolle IoT Hub

Azure IoT Hub unterstützt Kommunikation über MQTT, AMQP und HTTP-Protokolle. In einigen Fällen Geräte Feld Gateways möglicherweise nicht standardmäßige Protokolle verwenden oder Protokoll Anpassung benötigen. In solchen Fällen können Sie ein benutzerdefiniertes Gateway. Ein benutzerdefiniertes Gateway kann Protokoll Anpassung für IoT Hub Endpunkte Datenverkehr zum und vom IoT Hub bridging aktivieren. [Azure IoT Protokollgateway](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md) können als benutzerdefiniertes Gateway Sie Protokoll Anpassung IoT Hub ermöglichen.

## <a name="azure-iot-protocol-gateway"></a>Azure IoT Protokollgateway

Azure IoT Protokollgateway ist ein Framework für Protokoll-Anpassung, die hochgradig skalierbar, Bidirektionalität IoT Hub Gerät vorgesehen ist. Das Gateway Protocol ist eine Pass-Through-Komponente, die Verbindung über ein bestimmtes Protokoll akzeptiert. Den Datenverkehr IoT Hub Brücke über AMQP 1.0. Azure IoT Protokollgateway steht als Open Source-Software-Projekt Unterstützung für eine Vielzahl von Protokollen und Protokollversionen Flexibilität.

Mithilfe von Azure Cloud Services Worker-Rollen können Sie Protokollgateway in Azure eine hochgradig skalierbare Weise bereitstellen. Darüber hinaus kann Gateway Protocol in lokalen Umgebungen wie Feld Gateways bereitgestellt.

Azure IoT Protokollgateway enthält einen MQTT Protokoll Adapter, der Sie gegebenenfalls MQTT Protokollverhalten anpassen kann. Da IoT Hub Integrierte MQTT v3.1.1-Protokoll unterstützt, sollten Sie nur erwägen Adapter Protokoll MQTT haben müssen Protokoll angepasst oder spezielle Vorschriften für zusätzliche Funktionen.

MQTT Adapter veranschaulicht auch das Programmiermodell für Protokoll Adapter für andere Protokolle erstellen. Darüber hinaus ermöglicht das Programmiermodell Azure IoT Protokoll Gateway benutzerdefinierter Komponenten für spezielle Verarbeitung oder benutzerdefinierte Authentifizierung, Nachricht Transformationen, Kompression/Dekompression, Verschlüsselung und Entschlüsselung des Datenverkehrs zwischen Geräten und IoT Hub anschließen.

Flexibilität werden Protokollgateway und MQTT-Implementierung in einem Open-Source-Software-Projekt bereitgestellt. Dadurch können Sie die Implementierung nach Bedarf anpassen.

## <a name="next-steps"></a>Nächste Schritte

Azure IoT Protokollgateway und Verwendung und Bereitstellung als Teil der Projektmappe IoT finden Sie unter:

* [Azure IoT Protokoll Gateway Repository auf GitHub](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md)
* [Azure IoT Protokoll Gateway Entwicklerhandbuch](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/docs/DeveloperGuide.md)

Informationen zum Planen der Bereitstellung IoT Hub finden Sie unter:

- [Vergleichen Sie mit Event Hubs][lnk-compare]
- [Skalieren, HA und DR][lnk-scaling]

Weitere Funktionen des IoT Hub kann, finden Sie unter:

- [Das Entwicklerhandbuch][lnk-devguide]
- [Ein SDK Gateway IoT simulieren][lnk-gateway]

[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
