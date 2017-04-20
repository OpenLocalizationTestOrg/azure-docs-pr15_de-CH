<properties
   pageTitle="Das Internet der Dinge Security Best Practices | Microsoft Azure"
   description="Der Artikel listet kuratierte Microsoft Internet der Dinge Security Best Practices und allgemeine Vorschläge."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="StevenPo"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="yurid"/>

# <a name="internet-of-things-security-best-practices"></a>Das Internet der Dinge Security Best Practices

Absicherung der Infrastruktur Internet der Dinge (IoT) ist ein wichtige Personen mit IoT-Lösungen. Die Anzahl der Beteiligten und die verteilte Natur dieser Geräte die folgen ein Sicherheitsereignis Millionen von Geräten IoT beeinträchtigen nicht trivial und weit auswirken.

Aus diesem Grund benötigt IoT Sicherheit eine Sicherheit Verteidigungsstrategie. Daten in der Cloud sicher sein und über private und öffentliche Netzwerke verschoben. Methoden müssen sicher IoT Geräte bereitgestellt werden. Jede Ebene vom Gerät Netzwerk cloud-Back-End benötigt starke Sicherheitsgarantien.

IoT best Practices können wie folgt kategorisiert:

- IoT Hardwarehersteller oder integrator
- IoT-Lösungsentwickler
- IoT Lösung Bereitsteller
- IoT Lösung operator

Dieser Artikel beschreibt [Internet der Dinge Security Best Practices](../iot-suite/iot-security-best-practices.md). Finden Sie in diesem Artikel Weitere Informationen.

## <a name="iot-hardware-manufacturer-or-integrator"></a>IoT Hardwarehersteller oder integrator

Führen Sie die folgenden best Practices ein IoT Hardwarehersteller oder Hardware Integrator:

- **Bereich Hardware Mindestanforderungen**: Hardware-Design sollte Mindesterfordernisse für die Hardware, und nichts mehr erforderlich. 
- **Hardware fälschungssicher zu machen**: Mechanismen erkennen physischen Übergriffen Hardware wie öffnen Gerät, einen Teil der usw. erstellen. 
- **Um sichere Hardware erstellen**: [COGS](https://en.wikipedia.org/wiki/Cost_of_goods_sold) zulassen Sicherheitsfunktionen wie sichere und verschlüsselte und Trusted Platform Module TPM-basierten Boot-Funktionen erstellen.
- **Dabei sicher**: Aktualisieren der Firmware während der Lebensdauer des Geräts ist unvermeidlich.

## <a name="iot-solution-developer"></a>IoT-Lösungsentwickler

Führen Sie die folgenden best Practices sind ein IoT-Lösungsentwickler:

- **Führen Sie sichere Software Development-Methodik**: Entwicklung sicherer Software Boden denken über Sicherheit seit Beginn des Projekts bis seine Implementierung, Test und Bereitstellung erfordert.
- **Wählen Sie open Source-Software mit Vorsicht**: open Source-Software ermöglicht die schnelle Lösungen.
- **Integration mit Vorsicht**: viele Software-Sicherheitslücken vorhanden an der Grenze von Bibliotheken und APIs. 

## <a name="iot-solution-deployer"></a>IoT Lösung Bereitsteller

Führen Sie die folgenden best Practices sind ein Bereitsteller IoT-Lösung:

- **Hardware sicher bereitstellen**: IoT Installationen erfordern Hardware an unsicheren Speicherorten wie Räume oder unbeaufsichtigt Gebietsschemas bereitgestellt werden.
- **Schützen Sie Authentifizierungsschlüssel**: während der Bereitstellung jedes Gerät benötigt Geräte-IDs sowie die zugehörigen Authentifizierungsschlüssel Cloud-Dienst generiert. Schützen Sie diese Schlüssel physisch auch nach der Bereitstellung. Ein gefährdeter Schlüssel von einem böswilligen Gerät lässt sich als ein vorhandenes Gerät auszugeben.

## <a name="iot-solution-operator"></a>IoT Lösung operator

Führen Sie die folgenden best Practices sind ein Operator IoT-Lösung:

- **Aktualisieren von Systemen**: gewährleisten Gerätebetriebssysteme und alle Treiber auf die neuesten Version aktualisiert. 
- **Schutz vor unberechtigten**: das Betriebssystem zulässt, setzen Sie die neuesten Anti-Virus und Anti-Malware-Funktionen auf jedem Gerät. 
- **Häufig Audit**: Überwachung IoT Infrastruktur für sicherheitsrelevante Probleme ist auf Sicherheitsvorfälle reagieren.
- **IoT Infrastruktur physisch geschützt**: schlechtesten Sicherheitsangriffe auf IoT Infrastruktur sind mit physischen Zugriff auf Geräte gestartet.
- **Cloud-Anmeldeinformationen schützen**: zur Konfiguration und den Betrieb einer Bereitstellung IoT Cloud-Anmeldeinformationen sind möglicherweise am einfachsten Zugriff und IoT-System. 
