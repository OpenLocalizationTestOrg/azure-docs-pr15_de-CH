<properties
 pageTitle="Entwicklerhandbuch - IoT Hub SDKs | Microsoft Azure"
 description="Azure IoT Hub Entwicklerhandbuch - Informationen und Links zu den verschiedenen Azure IoT Hub Gerät und SDKs."
 services="iot-hub"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016"
 ms.author="dobett"/>

# <a name="iot-hub-sdks"></a>IoT Hub SDKs

## <a name="iot-hub-device-sdks"></a>IoT Hub-Gerät SDKs

Microsoft Azure IoT Gerät SDKs enthalten Code, der erleichtert die Erstellung Geräte und Programme, die zum Verbinden von Azure IoT Hub Services verwaltet werden.

Folgende IoT-Gerät SDKs von GitHub heruntergeladen werden:

- [Azure IoT Device SDK c] [ lnk-c-device-sdk] für Portabilität und Breite Plattform-Kompatibilität in ANSI C (C99) geschrieben.
- [Azure IoT Device SDK für .NET][lnk-dotnet-device-sdk]
- [Azure IoT Gerät-SDK für Java][lnk-java-device-sdk]
- [Azure IoT-Gerät SDK Node.js][lnk-node-device-sdk]
- [Microsoft Azure IoT Device SDK für Python 2.7][lnk-python-device-sdk]

> [AZURE.NOTE] Finden Sie die Readme-Dateien in den Repositories GitHub Informationen über Sprache und Plattform-spezifische Paket-Manager-Binärdateien und Abhängigkeit auf dem Entwicklungscomputer installieren.

## <a name="os-platforms-and-hardware-compatibility"></a>Betriebssystemen und Hardware-Kompatibilität

Weitere Informationen zur Kompatibilität mit bestimmter Hardwaregeräte SDK finden Sie unter [Azure IoT Gerät Katalog Certified][lnk-certified].

## <a name="iot-hub-service-sdks"></a>IoT Hub Service SDKs

Microsoft Azure IoT Service SDKs enthalten Code, der Building Applications, die interagieren direkt mit IoT Hub Verwaltung von Geräten und Sicherheit ermöglicht.

Der folgende Dienst IoT SDKs sind von GitHub heruntergeladen:

- [Azure IoT Service SDK für .NET][lnk-dotnet-service-sdk]
- [Azure IoT Service SDK für Node.js][lnk-node-service-sdk]
- [Azure IoT Service SDK für Java][lnk-java-service-sdk]

> [AZURE.NOTE] Finden Sie die Readme-Dateien in den Repositories GitHub Informationen über Sprache und Plattform-spezifische Paket-Manager-Binärdateien und Abhängigkeit auf dem Entwicklungscomputer installieren.

## <a name="azure-iot-gateway-sdk"></a>Azure IoT Gateway SDK

Azure IoT Gateway SDK enthält die Infrastruktur und Module IoT Gateway-Lösungen zu erstellen. Das SDK Gateways zugeschnitten auf alle End-to-End-Szenario erstellen kann.

[Azure IoT Gateway SDK] downloaden[ lnk-gateway-sdk] von GitHub.

## <a name="online-api-reference-documentation"></a>Online-API-Referenzdokumentation

Folgendes ist eine Liste mit Links zu Online-API-Referenzdokumentation für Azure IoT Gerät Service und Gateway-Bibliotheken:

- [Das Internet der Dinge (IoT) .NET][lnk-dotnet-ref]
- [IoT Hub REST][lnk-rest-ref]
- [Azure IoT Device SDK für C][lnk-c-ref]
- [Azure IoT Gerät-SDK für Java][lnk-java-ref]
- [Azure IoT Service SDK für Java][lnk-java-service-ref]
- [Azure IoT-Gerät SDK Node.js][lnk-node-ref]
- [Azure IoT Service SDK für Node.js][lnk-node-service-ref]
- [Azure IoT Gateway SDK][lnk-gateway-ref]

## <a name="next-steps"></a>Nächste Schritte

Andere Referenzthemen in diesem Handbuch IoT Hub Entwickler enthalten:

- [IoT Hub Endpunkte][lnk-devguide-endpoints]
- [Abfragesprache für im Vergleich, Methoden und Aufträge][lnk-devguide-query]
- [Kontingente und Drosselung][lnk-devguide-quotas]
- [IoT Hub MQTT support][lnk-devguide-mqtt]

<!-- Links and images -->

[lnk-c-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/c/readme.md
[lnk-dotnet-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/csharp/device/readme.md
[lnk-java-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/java/device/readme.md
[lnk-dotnet-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/csharp/service/README.md
[lnk-java-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/java/service/readme.md
[lnk-node-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/node/device/readme.md
[lnk-node-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/node/service/README.md
[lnk-python-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/python/device/readme.md
[lnk-certified]: https://catalog.azureiotsuite.com/
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/README.md

[lnk-dotnet-ref]: https://msdn.microsoft.com/library/mt488521.aspx
[lnk-c-ref]: http://azure.github.io/azure-iot-sdks/c/api_reference/index.html
[lnk-java-ref]: http://azure.github.io/azure-iot-sdks/java/device/api_reference/index.html
[lnk-node-ref]: http://azure.github.io/azure-iot-sdks/node/api_reference/azure-iot-device/1.0.15/index.html
[lnk-rest-ref]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-java-service-ref]: http://azure.github.io/azure-iot-sdks/java/service/api_reference/index.html
[lnk-node-service-ref]: http://azure.github.io/azure-iot-sdks/node/api_reference/azure-iothub/1.0.17/index.html
[lnk-gateway-ref]: http://azure.github.io/azure-iot-gateway-sdk/api_reference/c/html/

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md