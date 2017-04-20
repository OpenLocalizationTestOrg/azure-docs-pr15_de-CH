<properties 
   pageTitle="StorSimple virtuelles Gerät Admin-Kennwort ändern | Microsoft Azure"
   description="Beschreibt, wie mithilfe der Azure-Verwaltungsportal oder StorSimple Virtual Array Webbenutzeroberfläche Gerät Administratorkennwort ändern."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="06/17/2016"
   ms.author="alkohli" />

# <a name="change-the-storsimple-virtual-array-device-administrator-password"></a>Ändern des Administratorkennworts für Virtual Array StorSimple-Gerät

## <a name="overview"></a>Übersicht

Wenn Sie die Windows PowerShell-Schnittstelle auf StorSimple virtuelles Gerät verwenden, müssen Sie ein Gerät Administratorkennwort eingeben. StorSimple-Gerät zuerst bereitgestellt und gestartet wird, wird das Standardkennwort *Kennwort1*. Für die Sicherheit Ihrer Daten läuft das Kennwort beim ersten, die Sie sich anmelden und Sie müssen dieses Kennwort ändern.

Außerdem können Sie der lokalen Web-Benutzeroberfläche oder der Azure-Verwaltungsportal Administratorkennwort Gerät jederzeit nach der Bereitstellung der Geräte in Ihrem Unternehmen ändern. Jedes dieser Verfahren wird in diesem Artikel beschrieben.

## <a name="use-the-azure-classic-portal-to-change-the-password"></a>Mit klassischen Azure-Portal zum Ändern des Kennworts

Die folgenden Schritte das Administratorkennwort Gerät durch klassischen Azure-Portal ändern.

#### <a name="to-change-the-device-administrator-password-via-the-azure-classic-portal"></a>Das Administratorkennwort Gerät über klassischen Azure-Portal ändern

1. Klicken Sie im Portal auf **Geräte** > **Konfiguration** Ihres Geräts.

2. Bildlauf **Gerät Administratorkennwort** Bereich. Geben Sie ein Administratorkennwort, das von 8 bis 15 Zeichen enthält. Das Kennwort muss aus Großbuchstaben, Kleinbuchstaben, numerischen und Sonderzeichen sein.

3. Bestätigen Sie das Kennwort ein.

4. Klicken Sie am unteren Rand der Seite **Speichern** .

Das Administratorkennwort Gerät sollte jetzt aktualisiert werden. Das geänderte Kennwort können Sie das Gerät lokal zugreifen.

## <a name="use-the-storsimple-virtual-array-web-ui-to-change-the-password"></a>Verwenden der StorSimple Virtual Array-Web-Benutzeroberfläche zum Ändern des Kennworts

Die folgenden Schritte das Administratorkennwort Gerät mithilfe der lokalen Web-Benutzeroberfläche ändern.

#### <a name="to-change-the-device-administrator-password-via-the-local-web-ui"></a>Das Administratorkennwort Geräte über lokale Webbenutzeroberfläche ändern

1. Klicken Sie in der lokalen Webbenutzeroberfläche auf **Wartung** > **Passwort** für Ihr Gerät.

    ![Kennwort1 ändern](./media/storsimple-ova-change-device-admin-password/image40.png)

2. Geben Sie das **aktuelle Kennwort**ein.

3. Geben Sie ein **Neues Kennwort**. Das Kennwort muss mindestens 8 Zeichen lang sein. Muss 3 von 4 Folgendes: Großbuchstaben, Kleinbuchstaben, numerischen und Sonderzeichen.

    Beachten Sie, dass Ihr Kennwort die letzten 24 Kennwörter identisch sein kann.

3. Geben Sie das Kennwort zur Bestätigung erneut ein.

    ![Kennwort2 ändern](./media/storsimple-ova-change-device-admin-password/image41.png)

4. Klicken Sie am unteren Rand der Seite auf **Übernehmen**. Das neue Kennwort werden angewendet. Ist die Änderung des Kennworts nicht erfolgreich, wird die folgende Fehlermeldung angezeigt.

    ![Fehler](./media/storsimple-ova-change-device-admin-password/image42.png)

    Nachdem das Kennwort erfolgreich aktualisiert wurde, werden Sie benachrichtigt. Das geänderte Kennwort können Sie das Gerät lokal zugreifen.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [virtuelle StorSimple Array verwalten](storsimple-ova-web-ui-admin.md).
