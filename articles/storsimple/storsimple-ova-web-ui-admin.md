<properties 
   pageTitle="StorSimple Virtual Array Web UI Verwaltung | Microsoft Azure"
   description="Beschreibt das Grundgerät Verwaltungsaufgaben durch StorSimple Virtual Array Webbenutzeroberfläche."
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
   ms.date="04/07/2016"
   ms.author="alkohli" />

# <a name="use-the-web-ui-to-administer-your-storsimple-virtual-array"></a>Verwenden Sie die Webbenutzeroberfläche StorSimple Virtual Array verwalten

![Setup-Prozessablauf](./media/storsimple-ova-web-ui-admin/manage4.png)

## <a name="overview"></a>Übersicht

Die Lernprogramme in diesem Artikel gelten für Microsoft Azure StorSimple Virtual Array (auch bekannt als StorSimple für lokale virtuelle Gerät) März 2016 allgemein verfügbar (GA) Version ausführen. Dieser Artikel beschreibt einige der Arbeitsabläufe und Verwaltungsaufgaben für StorSimple Virtual Array ausgeführt werden können. StorSimple Virtual Array mit StorSimple-Manager verwalten service-Benutzeroberfläche (UI-Portal bezeichnet) und der lokalen Web-Benutzeroberfläche für das Gerät. Dieser Artikel konzentriert sich auf die Aufgaben, die Sie mit der Web-Benutzeroberfläche ausführen können.

Dieser Artikel enthält die folgenden Lernprogramme:

- Den Verschlüsselungsschlüssel Daten abrufen
- Problembehandlung bei Installationsfehlern für Web-Benutzeroberfläche
- Erstellen eines Pakets Protokoll
- Herunterfahren oder Neustarten des Geräts

## <a name="get-the-service-data-encryption-key"></a>Den Verschlüsselungsschlüssel Daten abrufen

Ein Dienst Datenschlüssel generiert, wenn das erste Gerät bei der StorSimple Manager-Dienst registrieren. Dieser Schlüssel wird dann mit Dienst-Registrierungsschlüssel erforderlich, zusätzliche Geräte mit StorSimple-Manager-Dienst registriert.

Wenn der Dienst Datenschlüssel und müssen abrufen verlegt haben, führen Sie die folgenden Schritte in der lokalen Web-Benutzeroberfläche des Geräts mit dem Dienst registriert.

#### <a name="to-get-the-service-data-encryption-key"></a>Zu den Verschlüsselungsschlüssel Daten

1. Verbinden Sie mit der lokalen Web-Benutzeroberfläche. Klicken Sie auf **Konfiguration** > **Cloud Settings**.
  

2. Am unteren Rand der Seite klicken Sie auf **Dienst Datenschlüssel**. Ein Schlüssel wird angezeigt. Kopieren Sie und speichern Sie dieser Schlüssel.
    
    ![erhalten Sie Service Data Encryption Key 1](./media/storsimple-ova-web-ui-admin/image27.png)
   


## <a name="troubleshoot-web-ui-setup-errors"></a>Problembehandlung bei Installationsfehlern für Web-Benutzeroberfläche

In einigen Fällen beim Konfigurieren des Geräts durch lokale Webbenutzeroberfläche, können Fehler auftreten. Zum Diagnostizieren und beheben diese Fehler, führen Sie die Diagnosetests.

#### <a name="to-run-the-diagnostic-tests"></a>Die Diagnosetests ausführen

1. Wechseln Sie in den lokalen Webbenutzeroberfläche zu **Problembehandlung** > **Diagnosetests**.

    ![Führen Sie eine Diagnose 1](./media/storsimple-ova-web-ui-admin/image29.png)

2. Klicken Sie am unteren Rand der Seite auf **Diagnosetests ausführen**. Tests zur diagnose möglicher Probleme mit Netzwerk, Gerät, Web-Proxy wird gestartet Zeit bzw. Cloud. Sie werden benachrichtigt, dass das Gerät Tests ausführt.

3. Nach Abschluss der Tests werden die Ergebnisse angezeigt. Das folgende Beispiel zeigt die Ergebnisse der Diagnosetests. Beachten Sie, dass die Webproxyeinstellungen auf diesem Gerät nicht konfiguriert wurden und daher der Web Proxy-Test wurde nicht ausgeführt. Alle Tests für Netzwerk, DNS-Server und Intervalle waren erfolgreich.

    ![Führen Sie eine Diagnose 2](./media/storsimple-ova-web-ui-admin/image30.png)

## <a name="generate-a-log-package"></a>Erstellen eines Pakets Protokoll

Log-Paket besteht aus alle relevanten Protokolle, die Microsoft Support bei der Fehlerbehebung Gerät Probleme unterstützen können. In dieser Version kann ein Paket Protokoll Internet lokale Benutzeroberfläche generiert werden.

#### <a name="to-generate-the-log-package"></a>Das Protokoll Paket generieren

1. Wechseln Sie in den lokalen Webbenutzeroberfläche zu **Problembehandlung** > **Systemprotokolle**.

    ![Protokoll Paket 1 generieren](./media/storsimple-ova-web-ui-admin/image31.png)

2. Klicken Sie am unteren Rand der Seite auf **Log-Paket erstellen**. Ein Paket der Systemprotokolle wird erstellt. Dies dauert einige Minuten.

    ![Protokoll Paket 2 generieren](./media/storsimple-ova-web-ui-admin/image32.png)

    Sie werden benachrichtigt, wenn das Paket erfolgreich erstellt und die Seite aktualisiert wird, um Uhrzeit und Datum der Erstellung des Pakets anzugeben.

    ![Protokoll Paket 3 generieren](./media/storsimple-ova-web-ui-admin/image33.png)

3. Klicken Sie auf **Protokoll Paket**. ZIP-Paket wird auf Ihr System heruntergeladen werden.

    ![Protokoll Paket 4 generieren](./media/storsimple-ova-web-ui-admin/image34.png)

4. Sie können das heruntergeladene Protokoll Paket entpacken und die Systemprotokolldateien.

## <a name="shut-down-and-restart-your-device"></a>Herunterfahren und Neustarten des Geräts

Herunterfahren oder Neustart des virtuellen Geräts mit der lokalen Webbenutzeroberfläche. Wir empfehlen, die vor dem Neustart die Volumes oder Freigaben auf dem Host und dem Gerät offline. Dadurch werden mögliche Datenverluste minimieren. 

#### <a name="to-shut-down-your-virtual-device"></a>Virtuelles Gerät Herunterfahren

1. Wechseln Sie in den lokalen Webbenutzeroberfläche **Wartung** > **Energieoptionen**.

2. Klicken Sie am unteren Rand der Seite auf **Herunterfahren**.

    ![Gerät Herunterfahren 1](./media/storsimple-ova-web-ui-admin/image36.png)

3. Eine Warnung wird angezeigt, dass ein Herunterfahren des Geräts e, die ausgeführt wird, eine Ausfallzeit unterbrochen wird. Klicken Sie auf das Häkchensymbol ![Symbol überprüfen](./media/storsimple-ova-web-ui-admin/image3.png).

    ![Gerät Herunterfahren Warnung](./media/storsimple-ova-web-ui-admin/image37.png)

    Sie werden benachrichtigt, dass das Herunterfahren.

    ![Gerät Herunterfahren gestartet](./media/storsimple-ova-web-ui-admin/image38.png)

    Das Gerät wird jetzt heruntergefahren. Wenn Sie Ihr Gerät starten möchten, müssen Sie das über Hyper-V-Manager tun.

#### <a name="to-restart-your-virtual-device"></a>Virtuelle Gerät neu starten

1. Wechseln Sie in den lokalen Webbenutzeroberfläche **Wartung** > **Energieoptionen**.

2. Klicken Sie am unteren Rand der Seite auf **neu starten**.

    ![Neustart des Geräts](./media/storsimple-ova-web-ui-admin/image36.png)

3. Eine Warnung wird angezeigt, dass der Neustart des Geräts unterbrechen alle IOs ausgeführt wurden eine Ausfallzeit. Klicken Sie auf das Häkchensymbol ![Symbol überprüfen](./media/storsimple-ova-web-ui-admin/image3.png).

    ![Warnung starten](./media/storsimple-ova-web-ui-admin/image37.png)

    Sie werden benachrichtigt, dass der Neustart initiiert wurde.

    ![Neustart initiiert](./media/storsimple-ova-web-ui-admin/image39.png)

    Während der Neustart ausgeführt wird, verlieren Sie die Verbindung auf der Benutzeroberfläche. Regelmäßig aktualisieren der Benutzeroberfläche, um den Neustart zu überwachen. Alternativ können Sie den Neustart Gerätestatus über Hyper-V-Manager überwachen.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [StorSimple Manager-Dienst das Gerät verwalten](storsimple-manager-service-administration.md).
