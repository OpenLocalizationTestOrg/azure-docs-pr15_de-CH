<properties
   pageTitle="Traffic Manager Failover Datenverkehr Routingmethode konfigurieren | Microsoft Azure"
   description="Dieser Artikel hilft Ihnen Failover Datenverkehr Routingmethode in Traffic Manager konfigurieren"
   services="traffic-manager"
   documentationCenter=""
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="sewhee" />
<!-- repub for nofollow -->

# <a name="configure-failover-routing-method"></a>Konfigurieren Sie Failover routing

Oft will eine Organisation Zuverlässigkeit für die Dienste. Dies geschieht durch backup Service bei ihrer primäre Dienst ausfällt. Ein häufiges Muster für Service-Failover ist um identische Dienste bieten und Datenverkehr an primären Dienst Beibehaltung einer konfigurierten Liste ein oder mehrere backup-Services. Diese Art der Sicherung können mit Azure-Cloud-Dienste und Websites anhand der folgenden Verfahren.

Beachten Sie, dass Azure Websites bereits Failover Datenverkehr routing Funktionalität für Websites innerhalb eines Datencenters (auch bekannt als Region) unabhängig von der Website. Traffic Manager können Sie Failover Datenverkehr routing für Websites in verschiedenen Rechenzentren an.

## <a name="to-configure-failover-traffic-routing-method"></a>Routingmethode für Failover-Datenverkehr zu konfigurieren:

1. Klicken Sie im klassischen Azure-Portal, im linken Bereich **Traffic Manager** um das Traffic Manager zu öffnen. Wenn Sie Ihr Profil Traffic Manager noch nicht erstellt haben, finden Sie unter [Traffic Manager Profile verwalten](traffic-manager-manage-profiles.md) Schritte zum Erstellen eines grundlegenden Traffic Manager-Profils.
2. Im Traffic Manager im klassischen Azure-Portal suchen Sie Traffic Manager-Profil, das die Einstellung enthält, die Sie ändern möchten, und klicken Sie dann auf den Pfeil rechts neben dem Profilnamen. Dies öffnet die Einstellungsseite für das Profil.
3. Auf Ihrer Profilseite **Endpunkte** am oberen Rand der Seite auf, und stellen Sie sicher, dass die cloud-Dienste und Websites (Endpunkte), die in der Konfiguration enthalten sind. Hinzufügen oder Entfernen von Endpunkten finden Sie unter [Verwalten von Endpunkten in Traffic Manager](traffic-manager-endpoints.md).
4. Klicken Sie auf Ihrer Profilseite oben auf der Konfigurationsseite **Konfigurieren** .
5. **Datenverkehr Routingeinstellungen Methode**sicher, dass die Routingmethode Datenverkehr **Failover**. Ist es nicht, klicken Sie auf **Failover** aus der Dropdownliste aus.
6. **Failover-Prioritätenliste**passen Sie Failover Reihenfolge für Ihre Endgeräte an Bei der Auswahl der **Failover** Datenverkehr Routingmethode spielt die Reihenfolge der ausgewählten. Die primären ist auf. Oben und unten nach Bedarf ändern. Informationen zum Festlegen der Failoverpriorität mithilfe von Windows PowerShell finden Sie unter [Set-AzureTrafficManagerProfile](http://go.microsoft.com/fwlink/p/?LinkId=400880).
7. Überprüfen Sie, ob die **Einstellung Überwachung** richtig konfiguriert sind. Überwachung wird sichergestellt, dass die Endpunkte, die offline sind kein Datenverkehr gesendet werden. Um Endpunkte zu überwachen, müssen Sie einen Pfad und einen Dateinamen angeben. Hinweis ein Forward-Schrägstrich "/" ist ein gültiger Eintrag für den relativen Pfad und impliziert, dass die Datei im Stammverzeichnis (Standard). Weitere Informationen zur Überwachung finden Sie unter [Traffic Manager überwachen](traffic-manager-monitoring.md).
8. Schließen Sie die geänderte Konfiguration, klicken Sie am unteren Rand der Seite **Speichern** .
9. Testen Sie die Änderung in Ihrer Konfiguration. Weitere Informationen finden Sie unter [Testen Traffic Manager Settings](traffic-manager-testing-settings.md) .
10. Nach Ihrem Traffic Manager-Profil einrichten und arbeiten bearbeiten Sie des autorisierenden DNS-Servers der Traffic Manager-Domänenname den Firmennamen für die Domäne auf den DNS-Eintrag Weitere Informationen hierzu finden Sie unter [Punkt einer Unternehmensdomäne Internet Traffic Manager-Domäne](traffic-manager-point-internet-domain.md).

## <a name="next-steps"></a>Nächste Schritte

[Zeigen Sie eine Internetdomäne Unternehmen Traffic Manager-Domäne](traffic-manager-point-internet-domain.md)

[Traffic Manager Verteilermethoden](traffic-manager-routing-methods.md)

[Konfigurieren von Round-Robin Verteilungsmethode](traffic-manager-configure-round-robin-routing-method.md)

[Routingmethode Leistung konfigurieren](traffic-manager-configure-performance-routing-method.md)

[Problembehandlung bei Traffic Manager verminderten Zustand](traffic-manager-troubleshooting-degraded.md)

[Manager - deaktivieren, aktivieren oder Löschen eines Profils](disable-enable-or-delete-a-profile.md)

[Traffic Manager - deaktivieren oder Aktivieren eines Endpunkts](disable-or-enable-an-endpoint.md)

