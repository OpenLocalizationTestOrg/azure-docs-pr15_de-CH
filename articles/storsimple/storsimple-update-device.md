<properties
   pageTitle="Aktualisieren Sie Ihr Gerät StorSimple | Microsoft Azure"
   description="Erläutert, wie die Aktualisierung StorSimple reguläre und Wartung-Modus-Updates und Hotfixes installieren."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="06/28/2016"
   ms.author="v-sharos" />

# <a name="update-your-storsimple-8000-series-device"></a>Aktualisieren Sie Ihr Gerät StorSimple 8000-Serie

## <a name="overview"></a>Übersicht

StorSimple Updates Funktionen können Sie einfach das Gerät StorSimple Stand. Je nach Update können Sie das Gerät mit klassischen Azure-Portal oder über die Windows PowerShell-Schnittstelle Updates anwenden. In diesem Lernprogramm beschreibt die Aktualisierungstypen und diese zu installieren.

Sie können zwei Arten von Geräteupdates anwenden: 

- Reguläre (oder normalen Modus) Updates
- Wartung-Modus-updates

Sie können regelmäßige Updates über die klassischen Azure-Portal oder Windows PowerShell; Allerdings müssen Sie Windows PowerShell verwenden, Wartung Modus Updates installieren. 

Jeder Aktualisierungstyp ist unten separat.

### <a name="regular-updates"></a>Regelmäßige updates

Unterbrechungsfreie Updates installiert werden können, wenn das Gerät im normalen Modus sind regelmäßige Updates. Diese Updates sind über Microsoft Update-Website jedem Gerät Controller angewendet. 

> [AZURE.IMPORTANT] Controller-Failover kann während der Aktualisierung auftreten. Aber wirkt diese Verfügbarkeit oder Vorgang sich nicht.

- Details zur Installation regelmäßig über den Azure-Verwaltungsportal finden Sie unter [regelmäßige Updates über Azure klassische portal(#install-regular-updates-via-the-azure-classic-portal) installieren.

- Sie können auch regelmäßige Updates über Windows PowerShell für StorSimple. Weitere Informationen finden Sie unter [Installieren regelmäßige Updates über Windows PowerShell für StorSimple](#install-regular-updates-via-windows-powershell-for-storsimple).

### <a name="maintenance-mode-updates"></a>Wartung-Modus-updates

Wartung-Modus-Updates sind unterbrechungsfrei Updates wie Festplatten-Firmware-Upgrades. Diese Updates erfordern das Gerät in den Wartungsmodus versetzt werden. Weitere Informationen finden Sie unter [Schritt2: Geben Sie Wartungsmodus](#step2). Klassische Azure-Portal können Maintenance Mode Updates installieren. Stattdessen müssen Sie Windows PowerShell für StorSimple verwenden. 

Informationen zur Wartung Modus Updates installieren finden Sie unter [Wartung installieren Modus Updates über Windows PowerShell für StorSimple](#install-maintenance-mode-updates-via-windows-powershell-for-storsimple).

> [AZURE.IMPORTANT] Wartung-Modus-Updates müssen separat auf jeden Domänencontroller angewendet werden. 

## <a name="install-regular-updates-via-the-azure-classic-portal"></a>Installieren Sie regelmäßig über die klassischen Azure-portal

Klassische Azure-Portal können Sie Updates für Ihr StorSimple-Gerät verfügbar sind.

[AZURE.INCLUDE [storsimple-install-updates-manually](../../includes/storsimple-install-updates-manually.md)]

## <a name="install-regular-updates-via-windows-powershell-for-storsimple"></a>Installieren Sie regelmäßig über Windows PowerShell für StorSimple

Alternativ können Sie Windows PowerShell für StorSimple (Normalmodus) regelmäßig Updates installieren.

> [AZURE.IMPORTANT] Obgleich die Installation mit Windows PowerShell StorSimple regelmäßig wird dringend empfohlen, regelmäßig über die klassischen Azure-Portal installieren. Beginnend mit Update 1 Vorabprüfungen erfolgt vor der Installation von Updates aus dem Portal. Kontrollen vor Fehlern unterbrechen und einen reibungsloseren sicherzustellen. 

[AZURE.INCLUDE [storsimple-install-regular-updates-powershell](../../includes/storsimple-install-regular-updates-powershell.md)]

## <a name="install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>Wartung-Modus-Updates über Windows PowerShell StorSimple installiert

Sie verwenden Windows PowerShell für StorSimple mit Wartung Modus Updates StorSimple-Gerät. In diesem Modus werden alle e/a-Anfragen angehalten. Auch werden Dienste wie RAM nichtflüchtigen Speicher (NVRAM) oder der Clusterdienst beendet. Beide Controller werden neu gestartet, wenn eingeben oder diesen Modus beenden. Bei diesem Modus werden alle Dienste werden fortgesetzt und gesund sein. (Dies kann einige Minuten dauern.)

Benötigen Sie Maintenance Mode Updates installieren, erhalten Sie eine Warnung über das klassische Azure-Portal haben Sie Updates, die installiert werden müssen. Diese Warnung enthält Informationen zur Verwendung von Windows PowerShell für StorSimple Updates zu installieren. Nachdem Sie Ihr Gerät aktualisieren, verwenden Sie dasselbe Gerät in den normalen Modus ändern. Eine schrittweise Anleitung finden Sie unter [Schritt 4: Exit Wartungsmodus](#step4).

> [AZURE.IMPORTANT] 
> 
> - Vor dem versetzen in den Wartungsmodus, überprüfen Sie beide Gerätecontroller fehlerfrei durch Überprüfen des **Hardwarestatus** auf der Seite **Wartung** im klassischen Azure-Portal. Wenn der Controller nicht fehlerfrei ist, erhalten Sie Microsoft Support weiter. Weitere Informationen finden Sie im Microsoft-Support kontaktieren. 
> - Sind im Wartungsmodus müssen Sie das Update zuerst auf einen Controller und dann auf den Controller installieren.

### <a name="step-1-connect-to-the-serial-console-a-namestep1"></a>Schritt 1: Verbinden Sie mit der seriellen Konsole<a name="step1">

Verwenden Sie eine Anwendung wie kitten zunächst auf die serielle Konsole zugreifen. Das folgende Verfahren erläutert kitten Verbindung an die serielle Konsole verwenden.

[AZURE.INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]

### <a name="step-2-enter-maintenance-mode-a-namestep2"></a>Schritt 2: Den Wartungsmodus<a name="step2">

Nachdem Sie mit der Konsole verbunden, bestimmen Sie, ob Updates zu installieren und den Wartungsmodus Installation.

[AZURE.INCLUDE [storsimple-enter-maintenance-mode](../../includes/storsimple-enter-maintenance-mode.md)]

### <a name="step-3-install-your-updates-a-namestep3"></a>Schritt 3: Installieren von updates<a name="step3">

Als Nächstes installieren.

[AZURE.INCLUDE [storsimple-install-maintenance-mode-updates](../../includes/storsimple-install-maintenance-mode-updates.md)]
 
### <a name="step-4-exit-maintenance-mode-a-namestep4"></a>Schritt 4: Exit Wartungsmodus<a name="step4">

Schließlich beenden Sie im Wartungsmodus.

[AZURE.INCLUDE [storsimple-exit-maintenance-mode](../../includes/storsimple-exit-maintenance-mode.md)]

## <a name="install-hotfixes-via-windows-powershell-for-storsimple"></a>Installieren Sie Updates über Windows PowerShell für StorSimple

Im Gegensatz zu Updates für Microsoft Azure StorSimple werden Hotfixes von einem freigegebenen Ordner installiert. Mit Updates gibt es zwei Typen von Updates: 

- Regelmäßige Updates 
- Maintenance Mode-hotfixes  

Nachfolgend wird erläutert, wie Windows PowerShell für StorSimple mit normalen und Maintenance Mode-Hotfixes installieren.

[AZURE.INCLUDE [storsimple-install-regular-hotfixes](../../includes/storsimple-install-regular-hotfixes.md)]

[AZURE.INCLUDE [storsimple-install-maintenance-mode-hotfixes](../../includes/storsimple-install-maintenance-mode-hotfixes.md)]

## <a name="what-happens-to-updates-if-you-perform-a-factory-reset-of-the-device"></a>Was geschieht mit Updates, wenn eine Factory Zurücksetzen des Geräts durchführen?

Wenn ein Gerät auf Standardeinstellung zurücksetzen, werden alle Updates verloren. Nach Factory Rückstellung registriert und konfiguriert ist, müssen Sie installieren Updates mithilfe der Azure-Verwaltungsportal und Windows PowerShell für StorSimple. Weitere Informationen zum Factory zurücksetzen, finden Sie [das Gerät auf die werkseitigen Standardeinstellungen zurückgesetzt](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur [Verwendung von Windows PowerShell für StorSimple StorSimple-Gerät verwalten](storsimple-windows-powershell-administration.md).
- Weitere Informationen zur [Verwendung des StorSimple Manager-Dienstes zum Verwalten von StorSimple-Gerät](storsimple-manager-service-administration.md)
