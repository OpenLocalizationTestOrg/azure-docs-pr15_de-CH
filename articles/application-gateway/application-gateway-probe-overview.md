

<properties
   pageTitle="Überwachen des Zustands Übersicht für Azure Application Gateway | Microsoft Azure"
   description="Erfahren Sie mehr über die Überwachungsfunktionen in Azure Application Gateway"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="application-gateway-health-monitoring-overview"></a>Gateway Health monitoring Anwendungsübersicht

Azure Application Gateway standardmäßig überwacht den Zustand aller Ressourcen in der Back-End-Pool und entfernt automatisch alle Ressourcen aus dem Pool fehlerhaft betrachtet. Application Gateway weiterhin fehlerhaften Instanzen und verfügbar und Gesundheit Prüfpunkte reagieren, fehlerfreien Back-End-Pool hinzugefügt. Application Gateway sendet der Zustand mit demselben Port Prüfpunkte, die Back-End HTTP-Einstellungen definiert ist. Dadurch Prüfpunkts denselben Port testen, den Verbindung zu den Back-End-Kunden verwenden werden.

![Gateway-Prüfpunkt wird][1]

Neben Standard Prüfpunkt Überwachung, können Sie auch Integritätstest für Anforderungen der Anwendung anpassen. In diesem Artikel werden sowohl Standard-als auch benutzerdefinierte Health Prüfpunkte behandelt.

## <a name="default-health-probe"></a>Standard-Integritätstest

Ein Gateway konfiguriert automatisch eine Standard-Integritätstest, wenn alle benutzerdefinierten Test-Konfiguration nicht festlegen. Überwachung Verhalten funktioniert durch eine HTTP-Anforderung an die IP-Adressen für den Back-End-Pool konfiguriert. Für Standard-Prüfpunkte Back-End-HTTP-Einstellungen für HTTPS konfiguriert sind verwenden Prüfpunkts sowie Https Gesundheit die Downloadzeit zu testen.

Beispiel: Ihre Anwendungsgateway um Back-End-Server A, B und C HTTP Netzwerkdatenverkehr an Port 80 konfigurieren. Drei Server testet Standard Gesundheitsberichterstattung alle 30 Sekunden eine gesunde HTTP-Antwort. Eine gesunde HTTP-Antwort hat einen [Statuscode](https://msdn.microsoft.com/library/aa287675.aspx) zwischen 200 und 399 ein.

Kontrollkästchen Prüfpunkt Standard Server A ausfällt, Application Gateway aus dem Back-End-Pool entfernt und Netzwerkverkehr hält an diesem Server. Der Standard-Prüfpunkt weiterhin Server alle 30 Sekunden überprüfen. Wenn Server A aus einem Standard-Integritätstest erfolgreich auf eine Anforderung antwortet, wird es wieder fehlerfrei an den Back-End-Pool hinzugefügt und Datenverkehr fließt an den Server erneut.

### <a name="default-health-probe-settings"></a>Standardeinstellungen für Gesundheit Prüfpunkt

|Prüfpunkt-Eigenschaft | Wert | Beschreibung|
|---|---|---|
| Prüfpunkt-URL| http://127.0.0.1:\<Anschluss\>/ | URL-Pfad |
| Intervall | 30 | Prüfpunkt-Intervall in Sekunden |
| Zeitlimit  | 30 | Prüfpunkt Timeout in Sekunden |
| Fehlerhafte Schwellenwert | 3 | Prüfpunkt Wiederholungsanzahl. Back-End-Server ist unten markiert, aufeinander folgende Prüfpunkt Fehlerzähler fehlerhaften Schwellenwert erreicht. |

> [AZURE.NOTE] Die Ports werden immer denselben Port wie HTTP-Back-End.

Standard-Probe untersucht nur http://127.0.0.1:\<Port\> Zustand zu bestimmen. Benötigen Sie konfigurieren Integritätstest eine benutzerdefinierte URL oder eine Einstellung ändern, müssen Sie Benutzerdefinierte Probes verwenden, wie in den folgenden Schritten beschrieben.

## <a name="custom-health-probe"></a>Benutzerdefinierte Integritätstest

Benutzerdefinierte Probes ermöglichen Ihnen eine genauere Steuerung der Health monitoring. Bei benutzerdefinierten Prüfpunkte können Sie das Intervall Prüfpunkt, URL und Pfad zu testen und wie viele Fehlgeschlagene Antworten an die Back-End-Pool-Instanz als fehlerhaft markiert konfigurieren.

### <a name="custom-health-probe-settings"></a>Benutzerdefinierte Health Prüfpunkt settings

Die folgende Tabelle enthält Definitionen für die Eigenschaften der benutzerdefinierten Integritätstest.

|Prüfpunkt-Eigenschaft| Beschreibung|
|---|---|
| Name | Name des Prüfpunkts. Dieser Name dient der Prüfpunkt im Back-End HTTP-Einstellungen auf. |
| Protokoll | Protokoll den Prüfpunkt senden. Der Prüfpunkt wird im Back-End HTTP-Einstellungen definiert Protokoll verwenden. |
| Host |  Host-Name des Prüfpunkts senden. Für ist nur, wenn mehrere Standorte auf Application Gateway, andernfalls verwenden "127.0.0.1". Dies unterscheidet sich von VM-Hostnamen. |
| Pfad | Relativer Pfad des Prüfpunkts. Der gültige Pfad beginnt mit '/'. |
| Intervall | Prüfpunkt Intervall in Sekunden an. Dies ist das Zeitintervall zwischen zwei aufeinander folgende Stichproben.|
| Zeitlimit | Prüfpunkt Timeout in Sekunden an. Der Prüfpunkt wird als fehlgeschlagen, wenn eine gültige Antwort nicht innerhalb dieses Timeouts eingeht gekennzeichnet. |
| Fehlerhafte Schwellenwert | Prüfpunkt Wiederholungsanzahl. Back-End-Server ist unten markiert, aufeinander folgende Prüfpunkt Fehlerzähler fehlerhaften Schwellenwert erreicht. |

> [AZURE.IMPORTANT] Wenn Application Gateway für eine einzelne Site konfiguriert ist, sollte standardmäßig Host Name als "127.0.0.1" angegeben werden, wenn andernfalls benutzerdefinierte Probe konfiguriert.
Zu Referenzzwecken benutzerdefinierte Probe an gesendet \<Protokoll\>://\<Server\>:\<Port\>\<Pfad\>. Port verwendet werden denselben Port in den Back-End HTTP-Einstellungen definiert.

## <a name="next-steps"></a>Nächste Schritte

Nach dem Kennenlernen Application Gateway Überwachung können Sie einen [benutzerdefinierten Integritätstest](application-gateway-create-probe-portal.md) in Azure-Portal oder einen [benutzerdefinierten Integritätstest](application-gateway-create-probe-ps.md) PowerShell mit Bereitstellungsmodell Azure-Ressourcen-Manager konfigurieren.

[1]: ./media/application-gateway-probe-overview/appgatewayprobe.png