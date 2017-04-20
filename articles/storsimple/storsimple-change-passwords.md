<properties 
   pageTitle="Ändern Sie Ihr Kennwort StorSimple | Microsoft Azure" 
   description="Beschreibt, wie mit der StorSimple Manager-Dienst das Administratorkennwort StorSimple Snapshot-Manager und Gerät ändern." 
   services="storsimple" 
   documentationCenter="NA" 
   authors="alkohli" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="08/17/2016"
   ms.author="alkohli"/>

# <a name="use-the-storsimple-manager-service-to-change-your-storsimple-passwords"></a>Verwenden des StorSimple Manager-Dienstes StorSimple Kennwörter ändern

## <a name="overview"></a>Übersicht 

Der Azure-Verwaltungsportal Seite **Konfigurieren** enthält alle Geräteparameter, die auf einem StorSimple-Gerät konfigurieren können, die von einem StorSimple Manager-Dienst verwaltet. In diesem Lernprogramm erläutert die Verwendung die Seite **Konfigurieren** den Geräteadministrator oder Kennwort StorSimple Snapshot-Manager ändern.

## <a name="change-the-device-administrator-password"></a>Ändern des Administratorkennworts Gerät

Wenn Sie Windows PowerShell-Schnittstelle auf dem StorSimple-Gerät verwenden, müssen Sie ein Administratorkennwort Gerät eingeben. Wenn das erste StorSimple-Gerät mit einem Dienst registriert wird, ist das Standard-Kennwort für diese Schnittstelle *Kennwort1*. Für die Sicherheit Ihrer Daten müssen Sie dieses Kennwort am Ende der Registrierung ändern. Sie können nicht aus der Registrierung beenden, ohne dieses Kennwort. Weitere Informationen finden Sie unter [Schritt 3: Konfigurieren und registrieren Sie das Gerät über Windows PowerShell für StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Das Kennwort, das zuerst über die Windows PowerShell-Schnittstelle während der Registrierung festgelegt wurde kann über den Azure-Verwaltungsportal geändert werden. Die folgenden Schritte das Gerät Administratorkennwort ändern.

#### <a name="to-change-the-device-administrator-password"></a>Das Gerät Administratorkennwort ändern

1. Klicken Sie im klassischen Portal auf **Geräte** > für Ihr Gerät**Konfigurieren** .

2. Bildlauf **Gerät Administratorkennwort** Bereich. Geben Sie ein Administratorkennwort, das von 8 bis 15 Zeichen enthält. Das Kennwort muss aus mindestens 3 Großbuchstaben, Kleinbuchstaben, numerischen und Sonderzeichen sein.

3. Bestätigen Sie das Kennwort ein.

4. Klicken Sie am unteren Rand der Seite **Speichern** .

Das Administratorkennwort Gerät sollte jetzt aktualisiert werden. Das geänderte Kennwort können Sie die Windows PowerShell-Benutzeroberfläche zugreifen.

## <a name="change-the-storsimple-snapshot-manager-password"></a>StorSimple Snapshot-Manager-Kennwort ändern

StorSimple Snapshot-Manager-Software befindet sich auf dem Windows-Server und Administratoren zum Verwalten des Geräts StorSimple in Form eines lokalen Backups und Snapshots.

Wenn ein Gerät StorSimple Snapshot-Manager konfigurieren, werden Sie aufgefordert zu Gerät IP-Adresse und ein Kennwort zur Authentifizierung des Speichergeräts. Dieses Kennwort wird zuerst über die Windows PowerShell-Schnittstelle konfiguriert. Weitere Informationen finden Sie unter [Schritt 3: Konfigurieren und registrieren Sie das Gerät über Windows PowerShell für StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Das Kennwort, das zuerst über die Windows PowerShell-Schnittstelle während der Registrierung festgelegt wurde kann dann über das Verwaltungsportal geändert werden. Führen Sie die folgenden Schritte zum Ändern des Kennworts StorSimple Snapshot-Manager.

#### <a name="to-change-the-storsimple-snapshot-manager-password"></a>StorSimple Snapshot-Manager-Kennwort ändern

1. Klicken Sie im klassischen Portal auf **Geräte** > für Ihr Gerät**Konfigurieren** .

2. Bildlauf **StorSimple Snapshot-Manager** -Bereich. Geben Sie ein Kennwort mit 14 oder 15 Zeichen. Stellen Sie sicher, dass das Kennwort aus mindestens 3 Großbuchstaben, Kleinbuchstaben, numerischen und Sonderzeichen enthält.

3. Bestätigen Sie das Kennwort ein.

4. Klicken Sie am unteren Rand der Seite **Speichern** .

StorSimple Snapshot-Manager-Kennwort sollte jetzt aktualisiert werden.
 

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [StorSimple Sicherheit](storsimple-security.md).

- Erfahren Sie mehr über [die Gerätekonfiguration zu ändern](storsimple-modify-device-config.md).

- Weitere Informationen zur [Verwendung des StorSimple Manager-Dienstes zum Verwalten von StorSimple-Gerät](storsimple-manager-service-administration.md)
