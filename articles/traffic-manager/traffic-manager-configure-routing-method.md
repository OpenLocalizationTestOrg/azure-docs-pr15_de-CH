<properties
    pageTitle="Traffic Manager Verteilermethoden konfigurieren | Microsoft Azure"
    description="Dieser Artikel erläutert die verschiedene Verteilermethoden in Traffic Manager konfiguriert"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="sewhee" />
<!-- repub for nofollow -->

# <a name="configure-traffic-manager-routing-methods"></a>Konfigurieren Sie Verteilermethoden Traffic Manager

Azure Traffic Manager bietet drei Verteilermethoden, die steuern, wie Verkehr verfügbaren Dienstendpunkte weitergeleitet wird. Datenverkehr routing-Methode gilt für jede DNS-Abfrage empfangen, um zu bestimmen, welcher Endpunkt DNS-Antwort zurückgegeben werden sollen.

Es stehen drei Datenverkehr Verteilermethoden in Traffic Manager:

- **Priorität:** Wählen Sie 'Priority', wenn Sie einen primären Endpunkt und Backups für den Fall der nicht verfügbar ist.
- **Gewichtet:** Wählen "gewichteten" Wenn Endpunkte Datenverkehr verteilt werden soll, definieren entweder gleichmäßig oder Gewicht
- **Leistung:** Wählen Sie "Leistung", wenn Sie Endpunkte an unterschiedlichen geographischen Standorten und Endbenutzer "nächstliegende" Endpunkt in der niedrigsten Netzwerklatenz verwendet werden soll.

## <a name="configure-priority-routing-method"></a>Priorität Routingmethode konfigurieren

Unabhängig von der Website Funktionalität Azure Websites bereits Failover für Websites innerhalb eines Datencenters (Region). Traffic Manager bietet Failover für Websites in verschiedenen Rechenzentren.

Ein häufiges Muster für Service-Failover wird Datenverkehr primären Dienst senden und mehrere identische backup-Services für Failover. Die folgenden Schritte erläutern diese priorisierte Failover Azure-Cloud-Dienste und Websites zu konfigurieren:

1. Klicken Sie im klassischen Azure-Portal, im linken Bereich **Traffic Manager** um das Traffic Manager zu öffnen.
2. Suchen Sie im Bereich Traffic Manager im klassischen Azure-Portal Traffic Manager-Profil, das die Einstellung enthält, die Sie ändern möchten. Öffnen Sie die Profilseite Einstellungen klicken Sie auf den Pfeil rechts neben dem Profilnamen.
3. Klicken Sie auf Ihrer Profilseite auf **Endpunkte** am oberen Rand der Seite. Stellen Sie sicher, dass die Cloud-Dienste und Websites, die Sie in Ihre Konfiguration aufnehmen möchten vorhanden sind.
4. Klicken Sie oben auf der Konfigurationsseite **Konfigurieren** .
5. **Datenverkehr Routingeinstellungen Methode**sicher, dass die Routingmethode Datenverkehr **Failover**. Ist es nicht, klicken Sie auf **Failover** aus der Dropdownliste aus.
6. **Failover-Prioritätenliste**passen Sie Failover Reihenfolge für Ihre Endgeräte an Bei der Auswahl der **Failover** Datenverkehr Routingmethode spielt die Reihenfolge der ausgewählten. Die primären ist auf. Oben und unten nach Bedarf ändern. Informationen zum Festlegen der Failoverpriorität mithilfe von Windows PowerShell finden Sie unter [Set-AzureTrafficManagerProfile](http://go.microsoft.com/fwlink/p/?LinkId=400880).
7. Überprüfen Sie, ob die **Einstellung Überwachung** richtig konfiguriert sind. Überwachung wird sichergestellt, dass die Endpunkte, die offline sind kein Datenverkehr gesendet werden. Endpunkte zu überwachen, müssen Sie einen Pfad und einen Dateinamen angeben. Ein Schrägstrich "/" ist ein gültiger Eintrag für den relativen Pfad und impliziert, dass die Datei im Stammverzeichnis (Standard).
8. Schließen Sie die geänderte Konfiguration, klicken Sie am unteren Rand der Seite **Speichern** .
9. Testen Sie die Änderung in Ihrer Konfiguration.
10. Sobald Ihr Profil Traffic Manager arbeitet, bearbeiten Sie den DNS-Eintrag des autorisierenden DNS-Servers der Traffic Manager-Domänenname den Firmennamen für die Domäne auf

## <a name="configure-weighted-routing-method"></a>Gewichtete Routingmethode konfigurieren

Eine allgemeine routing Methode Verkehrsmuster ist identische Endpunkte Cloud-Dienste und Websites in Roundrobin-Datenverkehr senden. Die folgenden Schritte beschreiben, wie diese Routingmethode Datenverkehr konfigurieren.

>[AZURE.NOTE] Azure Websites Lastenausgleich bereits Roundrobin-Funktionalität für Websites innerhalb eines Datencenters (Region). Traffic Manager können Sie Roundrobin-Datenverkehr routing für Websites in verschiedenen Rechenzentren an.

1. Klicken Sie im klassischen Azure-Portal, im linken Bereich **Traffic Manager** um das Traffic Manager zu öffnen.
2. Suchen Sie im Traffic Manager Traffic Manager-Profil, das die Einstellung enthält, die Sie ändern möchten. Öffnen Sie die Profilseite Einstellungen klicken Sie auf den Pfeil rechts neben dem Profilnamen.
3. Auf der Seite für Ihr Profil **Endpunkte** am oberen Rand der Seite auf, und überprüfen Sie, ob die Endpunkte, die Sie in Ihre Konfiguration aufnehmen möchten vorhanden sind.
4. Klicken Sie auf Ihrer Profilseite oben auf der Konfigurationsseite **Konfigurieren** .
5. **Datenverkehr Routingmethode Einstellungen**sicher, dass die Routingmethode Datenverkehr **Round-Robin**. Ist es nicht, klicken Sie in der Dropdownliste auf **Round-Robin** .
6. Überprüfen Sie, ob die **Einstellung Überwachung** richtig konfiguriert sind. Überwachung wird sichergestellt, dass die Endpunkte, die offline sind kein Datenverkehr gesendet werden. Endpunkte zu überwachen, müssen Sie einen Pfad und einen Dateinamen angeben. Ein Schrägstrich "/" ist ein gültiger Eintrag für den relativen Pfad und impliziert, dass die Datei im Stammverzeichnis (Standard).
7. Schließen Sie die geänderte Konfiguration, klicken Sie am unteren Rand der Seite **Speichern** .
8. Testen Sie die Änderung in Ihrer Konfiguration.
9. Sobald Ihr Profil Traffic Manager arbeitet, bearbeiten Sie den DNS-Eintrag des autorisierenden DNS-Servers der Traffic Manager-Domänenname den Firmennamen für die Domäne auf

## <a name="configure-performance-traffic-routing-method"></a>Konfigurieren Sie Leistung Datenverkehr weiterleiten

Die Leistung Datenverkehr Routingmethode können Sie Datenverkehr vom Netzwerk des Clients auf den Endpunkt und die niedrigste Latenz leiten. Normalerweise liegt das Rechenzentrum und die niedrigste Latenz der geographischen Entfernung. Diese Routingmethode Datenverkehr nicht in Echtzeit Änderungen in der Konfiguration oder laden.

1. Klicken Sie im klassischen Azure-Portal, im linken Bereich **Traffic Manager** um das Traffic Manager zu öffnen.
2. Suchen Sie im Traffic Manager Traffic Manager-Profil, das die Einstellung enthält, die Sie ändern möchten. Öffnen Sie die Profilseite Einstellungen klicken Sie auf den Pfeil rechts neben dem Profilnamen.
3. Auf der Seite für Ihr Profil **Endpunkte** am oberen Rand der Seite auf, und überprüfen Sie, ob die Endpunkte, die Sie in Ihre Konfiguration aufnehmen möchten vorhanden sind.
4. Klicken Sie auf der Seite für Ihr Profil **Konfigurieren** nach oben, um die Konfigurationsseite zu öffnen.
5. **Datenverkehr Routingeinstellungen Methode**sicher, dass die Routingmethode Datenverkehr * *Leistung*. Wenn dies nicht der Fall ist, klicken Sie auf * *Leistung** in der Dropdown-Liste.
6. Überprüfen Sie, ob die **Einstellung Überwachung** richtig konfiguriert sind. Überwachung wird sichergestellt, dass die Endpunkte, die offline sind kein Datenverkehr gesendet werden. Endpunkte zu überwachen, müssen Sie einen Pfad und einen Dateinamen angeben. Ein Schrägstrich "/" ist ein gültiger Eintrag für den relativen Pfad und impliziert, dass die Datei im Stammverzeichnis (Standard).
7. Schließen Sie die geänderte Konfiguration, klicken Sie am unteren Rand der Seite **Speichern** .
8. Testen Sie die Änderung in Ihrer Konfiguration.
9. Sobald Ihr Profil Traffic Manager arbeitet, bearbeiten Sie den DNS-Eintrag des autorisierenden DNS-Servers der Traffic Manager-Domänenname den Firmennamen für die Domäne auf

## <a name="next-steps"></a>Nächste Schritte

* [Traffic Manager Profile verwalten](traffic-manager-manage-profiles.md)
* [Traffic Manager Verteilermethoden](traffic-manager-routing-methods.md)
* [Traffic Manager testen](traffic-manager-testing-settings.md)
* [Zeigen Sie eine Internetdomäne Unternehmen Traffic Manager-Domäne](traffic-manager-point-internet-domain.md)
* [Traffic Manager Endpunkte verwalten](traffic-manager-manage-endpoints.md)
* [Problembehandlung bei Traffic Manager verminderten Zustand](traffic-manager-troubleshooting-degraded.md)
