
<properties
   pageTitle="Internetzugriff Load Balancer Übersicht | Microsoft Azure "
   description="Übersicht für Internetzugriff Lastenausgleich und seine Funktionen. Wie funktioniert ein Lastenausgleich für Azure virtuelle Maschinen mit Cloud-Diensten."
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />


# <a name="internet-facing-load-balancer-overview"></a>Mit Load Balancer Übersicht über Internet

Azure Lastenausgleich ordnet die öffentliche IP-Adresse und Anschlussnummer Anzahl eingehender Datenverkehr die private IP-Adresse und Port-Nummer des virtuellen Computers und für den Antwortverkehr virtuellen Computer. Load Balance Regeln können Sie bestimmte Typen von Datenverkehr zwischen mehreren virtuellen Computern oder Diensten verteilen. Beispielsweise können Sie Load Webdatenverkehr auf mehrere Webserver oder Webrollen zuweisen.

Cloud-Dienst, der Instanzen Webrollen oder Workerthreads enthält, definieren Sie einen öffentlichen Endpunkt in der Service-Definitionsdatei (.csdef).

Die Datei _servicedefinition.csdef_ enthält die Endpunktkonfiguration und wenn mehrere Instanzen einer Web oder Worker-Rolle Bereitstellung haben Lastenausgleich Setup für sie. Instanzen der Cloudbereitstellung hinzufügen ist die Anzahl der Instanzen für die Service-Konfigurationsdatei (.csfg) ändern.

Die folgende Abbildung zeigt einen Endpunkt mit Lastenausgleich für drei virtuelle Computer für die öffentlichen und privaten TCP-Port 443 freigegebenes verschlüsselte Web-Verkehr. Diese drei virtuelle Computer sind in einer Gruppe mit Lastenausgleich.

![Öffentliche Load Balancer-Beispiel](./media/load-balancer-internet-overview/IC727496.png))

Abbildung 1: Lastenausgleich Endpunkt für verschlüsselte Web-Verkehr

Sendet Internetclients Webseiten öffentliche IP-Adresse des Cloud-Dienst auf TCP-Port 443, verteilt der Azure-Lastenausgleich Anfragen zwischen drei virtuelle Computer in der Gruppe mit Lastenausgleich. Weitere Informationen zu Load Balancer Algorithmen finden Sie unter [Balancer Seite geladen](load-balancer-overview.md#load-balancer-features).

Standardmäßig stellt Azure Lastenausgleich Netzwerkverkehr gleichmäßig auf mehrere Instanzen virtueller Computer. Sie können auch sitzungsaffinität für Weitere Informationen siehe [Load Balancer Verteilung Modus](load-balancer-distribution-mode.md)konfigurieren.

## <a name="next-steps"></a>Nächste Schritte

Lernen Sie [innere Lastenausgleichsmodul](load-balancer-internal-overview.md) verdeutlichen die Lastenausgleich für die Cloudbereitstellung ist.

Sie können auch [ein Lastenausgleich mit Internetzugriff erstellen beginnen](load-balancer-get-started-internet-arm-ps.md) und welche [Verteilung Modus](load-balancer-distribution-mode.md) für einen bestimmten Load Balancer Netzwerk Verhalten konfigurieren.

Wenn Ihre Anwendung Verbindungen für Server hinter einem Lastenausgleich beibehalten werden soll, können Sie mehr über [Leerlauf TCP-Timeouteinstellungen für einen Lastenausgleich](load-balancer-tcp-idle-timeout.md)verstehen. Damit Kennenlernen Verbindung im Leerlauf Verhalten bei Verwendung der Azure-Lastenausgleich.
