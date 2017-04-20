<properties 
   pageTitle="StorSimple Manager Service Dashboard - Virtual Array | Microsoft Azure"
   description="Dashboard Service StorSimple Manager beschrieben und erläutert, wie es zum Überwachen der Funktionsfähigkeit des Arrays virtuellen StorSimple."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/07/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-dashboard-for-the-storsimple-virtual-array"></a>Verwenden Sie StorSimple Manager Service-Dashboard für StorSimple Virtual Array

## <a name="overview"></a>Übersicht

StorSimple Manager Service Dashboard-Seite bietet eine Übersicht der StorSimple virtuellen Arrays (auch lokal StorSimple virtuellen oder virtuelle Geräte), die mit der StorSimple Manager-Dienst hervorheben, die ein Systemadministrator Aufmerksamkeit. Dieses Lernprogramm stellt die Dashboardseite, Dashboardinhalt und Funktion erklärt und beschreibt die Aufgaben, die Sie auf dieser Seite ausführen können.

![Service-dashboard](./media/storsimple-ova-service-dashboard/dashboard1.png)

StorSimple Manager Service-Schaltpult zeigt die folgende Informationen:

- Im **Diagrammbereich am oberen Rand der Seite** sehen Sie relevanten Kennzahlen für virtuelle Geräte. Sie können auf allen virtuellen Geräten verwendeten primären Speicher sowie cloudspeicher belegt durch virtuelle Geräte über einen Zeitraum anzeigen. Verwenden Sie die Steuerelemente in der rechten oberen Ecke des Diagramms, relativ oder absolut Verwendung angeben und 1 Woche, 1 Monat, 3 Monat oder 1 Jahr Zeitskala. Verwenden Sie das Aktualisierungssteuerelement ![Aktualisierungssteuerelement](./media/storsimple-ova-service-dashboard/refresh-control.png) im Diagramm aktualisiert.

- **Übersicht über die Verwendung** zeigt Primärspeicher, das bereitgestellt und von den virtuellen Geräten gegenüber den gesamten Speicher auf allen virtuellen Geräten. **Bereitgestellt** auf der Speichermenge, die erstellt und verwendet, **verwendet** bezieht sich auf Verwendung von Freigaben oder Volumes durch die Initiatoren angezeigt, die der virtuellen Geräte verbunden sind und **Max. Kapazität** zeigt maximal alle virtuellen Geräte.

- **Alerts** -Bereich stellt eine Momentaufnahme aller aktiven Warnungen auf allen virtuellen Geräten Warnungsschweregrad gruppiert. Auf den Schweregrad öffnet **der Benachrichtigungsseite Bereich beschränkt sich diese angezeigt** . Auf der Seite **Warnung** können Sie eine einzelne Warnung um zusätzliche Details dieser Warnung empfohlenen Aktionen klicken. Die Warnung kann auch deaktivieren, wenn das Problem behoben wurde.

- " **Projekte** " bietet eine Momentaufnahme der aktuellen Aufträge auf allen virtuellen Geräten, die mit dem Dienst verbunden sind. Gibt, mit denen Sie Aufträge anzeigen, die derzeit in Bearbeitung und den Erfolg oder Fehler in den letzten 24 Stunden. 

- **Blick** Bereich auf der rechten Seite bietet nützliche Informationen wie Dienststatus insgesamt virtuelle Geräte, Service Speicherort des Diensts und Details des Abonnements, das dem Dienst zugeordnet ist. Außerdem wird eine Verknüpfung zu dem Protokoll. Klicken Sie auf den Link, um eine Liste aller abgeschlossenen StorSimple Manager Dienstvorgänge. 

StorSimple Manager Service Dashboard-Seite können Sie Folgendes starten:

- Den Dienst-Registrierungsschlüssel zu erhalten.
- Anzeigen der Protokolle.

## <a name="get-the-service-registration-key"></a>Den Dienst-Registrierungsschlüssel zu erhalten

Dienst-Registrierungsschlüssel wird sich ein virtuelles Gerät StorSimple mit der StorSimple Manager-Dienst verwendet, sodass das Gerät im klassischen Azure-Portal für weitere Maßnahmen angezeigt wird. Die Taste auf dem ersten virtuellen Gerät erstellt und für die übrigen virtuellen Geräte freigegeben. 

**Registrierungsschlüssel** (am Seitenende) Dialogfenster das **Dienstregistrierungsschlüssel** , wo den aktuellen Service-Registrierungsschlüssel in die Zwischenablage kopieren oder Regenerieren der Dienstregistrierungsschlüssel.

![Registrierungsschlüssel](./media/storsimple-ova-service-dashboard/service-dashboard3.png)

Regenerieren des Schlüssels wirkt sich nicht auf zuvor registrierten virtuellen Geräte: betrifft nur die virtuellen Geräte, die mit dem Dienst registriert werden, nachdem der Schlüssel neu erstellt.

Finden Sie weitere Informationen zu Service-Registrierungsschlüssel zu [Service-Registrierungsschlüssel](storsimple-ova-manage-service.md#get-the-service-registration-key).

## <a name="view-the-operations-logs"></a>Das Log anzeigen

Die Protokolle sehen den Link Vorgang Protokolle im **Blick** des Dashboards verfügbar. Dadurch gelangen Sie zur Verwaltungsseite Services, können gefiltert und bestimmte Protokolle mit Ihrem StorSimple Manager-Dienst.

![Protokoll](./media/storsimple-ova-service-dashboard/ops-log.png)

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie [lokale Webbenutzeroberfläche StorSimple Virtual Array verwalten](storsimple-ova-web-ui-admin.md).