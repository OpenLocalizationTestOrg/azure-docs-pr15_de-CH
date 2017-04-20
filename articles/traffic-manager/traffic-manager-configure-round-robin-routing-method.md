<properties
   pageTitle="Traffic Manager round Robin Datenverkehr Routingmethode konfigurieren | Microsoft Azure"
   description="Dieser Artikel hilft bei der Round-Robin-Lastenausgleich für Traffic Manager Endpunkte konfigurieren."
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

# <a name="configure-round-robin-routing-method"></a>Routing konfigurieren Round-Robin-Methode

Eine allgemeine routing Methode Verkehrsmuster ist identische Endpunkte Cloud-Dienste und Websites in Roundrobin-Datenverkehr senden. Die folgenden Schritte beschreiben, wie Traffic Manager konfigurieren, um diese Art von Datenverkehr Routingmethode. Weitere Informationen über die verschiedenen Verteilermethoden anzeigen Sie [Verteilermethoden für Datenverkehr über Traffic Manager](traffic-manager-routing-methods.md)

>[AZURE.NOTE] Azure Websites bietet bereits Roundrobin-Lastenausgleich Funktionalität für Websites innerhalb eines Datencenters (Region). Traffic Manager können Sie Roundrobin-Datenverkehr routing für Websites in verschiedenen Rechenzentren an.

## <a name="routing-traffic-equally-round-robin-across-a-set-of-endpoints"></a>Weiterleiten von Verkehr gleichmäßig (round Robin) über Endpunkte:

1. Klicken Sie im klassischen Azure-Portal, im linken Bereich **Traffic Manager** um das Traffic Manager zu öffnen. Wenn Sie Ihr Profil Traffic Manager noch nicht erstellt haben, finden Sie unter [Traffic Manager Profile verwalten](traffic-manager-manage-profiles.md) Schritte zum Erstellen eines grundlegenden Traffic Manager-Profils.
2. Suchen Sie in Azure Verwaltungsportal Traffic Manager im Bereich Traffic Manager-Profil, das die Einstellung enthält, die Sie ändern möchten, und klicken Sie auf den Pfeil rechts neben dem Profilnamen. Dies öffnet die Einstellungsseite für das Profil.
3. Auf der Seite für Ihr Profil **Endpunkte** am oberen Rand der Seite auf, und überprüfen Sie, ob die Endpunkte, die Sie in Ihre Konfiguration aufnehmen möchten vorhanden sind. Hinzufügen oder Entfernen von Endpunkten finden Sie unter [Verwalten von Endpunkten in Traffic Manager](traffic-manager-endpoints.md).
4. Klicken Sie auf Ihrer Profilseite oben auf der Konfigurationsseite **Konfigurieren** .
5. **Datenverkehr Routingmethode Einstellungen**sicher, dass die Routingmethode Datenverkehr **Round-Robin**. Ist es nicht, klicken Sie in der Dropdownliste auf **Round-Robin** .
6. Überprüfen Sie, ob die **Einstellung Überwachung** richtig konfiguriert sind. Überwachung wird sichergestellt, dass die Endpunkte, die offline sind kein Datenverkehr gesendet werden. Um Endpunkte zu überwachen, müssen Sie einen Pfad und einen Dateinamen angeben. Hinweis ein Forward-Schrägstrich "/" ist ein gültiger Eintrag für den relativen Pfad und impliziert, dass die Datei im Stammverzeichnis (Standard). Weitere Informationen zur Überwachung finden Sie unter [Traffic Manager zu überwachen](traffic-manager-monitoring.md).
7. Schließen Sie die geänderte Konfiguration, klicken Sie am unteren Rand der Seite **Speichern** .
8. Testen Sie die Änderung in Ihrer Konfiguration. Weitere Informationen finden Sie unter [Testen von Traffic Manager Settings](traffic-manager-testing-settings.md).
9. Nach Ihrem Traffic Manager-Profil einrichten und arbeiten bearbeiten Sie des autorisierenden DNS-Servers der Traffic Manager-Domänenname den Firmennamen für die Domäne auf den DNS-Eintrag Weitere Informationen hierzu finden Sie unter [Punkt einer Unternehmensdomäne Internet Traffic Manager-Domäne](traffic-manager-point-internet-domain.md).

## <a name="next-steps"></a>Nächste Schritte


[Zeigen Sie eine Internetdomäne Unternehmen Traffic Manager-Domäne](traffic-manager-point-internet-domain.md)

[Traffic Manager Verteilermethoden](traffic-manager-routing-methods.md)

[Konfigurieren Sie Failover routing](traffic-manager-configure-failover-routing-method.md)

[Routingmethode Leistung konfigurieren](traffic-manager-configure-performance-routing-method.md)

[Problembehandlung bei Traffic Manager verminderten Zustand](traffic-manager-troubleshooting-degraded.md)

[Manager - deaktivieren, aktivieren oder Löschen eines Profils](disable-enable-or-delete-a-profile.md)

[Traffic Manager - deaktivieren oder Aktivieren eines Endpunkts](disable-or-enable-an-endpoint.md)

