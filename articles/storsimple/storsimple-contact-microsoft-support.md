<properties 
   pageTitle="Wenden Sie sich an Microsoft Support | Microsoft Azure"
   description="Informationen Sie zu eine Supportanfrage erstellen eine Sitzung auf dem Gerät StorSimple."
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
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="contact-microsoft-support"></a>Wenden Sie sich an Microsoft Support Services

Wenn Sie mit Ihrem Microsoft Azure StorSimple Probleme auftreten, können Sie eine Serviceanfrage für technischen Support erstellen. In einer Online-Sitzung mit Ihrem Support-Mitarbeiter müssen Sie auch eine Sitzung auf dem StorSimple-Gerät starten. Dieser Artikel führt Sie durch:

- Eine Support-Anfrage erstellen
- Wie Support-Sitzung in Windows PowerShell-Schnittstelle des Geräts StorSimple.

Überprüfen Sie [StorSimple 8000 Serie Unterstützung SLAs und Informationen](https://msdn.microsoft.com/library/mt433077.aspx) vor dem Erstellen einer Anfrage.

## <a name="create-a-support-request"></a>Erstellen Sie eine Support-Anfrage

Führen Sie die folgenden Schritte aus, um eine Supportanfrage zu erstellen:

#### <a name="to-create-a-support-request"></a>Eine Support-Anfrage erstellen

1. Im [klassischen Azure-Portal](https://manage.windowsazure.com/), in der oberen rechten Ecke auf Ihren Kontonamen, und dann auf **Microsoft-Support kontaktieren**.

    ![Kontakt MS Support ManagementPortal](./media/storsimple-contact-microsoft-support/Ibiza1.png)

2. Sie gelangen zum neuen Azure-Portal (portal.azure.com). Klicken Sie auf die Kachel **neue support-Anfragen** .

    ![Wenden Sie sich an Microsoft Support, neue Portal](./media/storsimple-contact-microsoft-support/Ibiza2.png)

    Auf der rechten Seite des Bildschirms wird der Bereich **neue support-Anfragen** . 

    ![Neue Unterstützung im Bereich Anforderung angezeigt](./media/storsimple-contact-microsoft-support/Ibiza3a.png)

3. Klicken Sie im Dialogfeld **Grundlagen** führen Sie folgende Schritte aus:                                
    1. Wählen Sie aus der Dropdownliste **Problemtyp** **technischen**.
    2. Wählen Sie ein **Abonnement** aus der Dropdownliste.
    3. Wählen Sie aus der Dropdown-Liste **Dienst** **StorSimple**. 
    4. Wählen Sie einen **Support-Plan** aus der Dropdown-Liste. Sie benötigen einen kostenpflichtigen Support technischen Support aktivieren möchten.

4. Klicken Sie auf **Weiter**. Das Dialogfenster **Problem** .

    ![Neue Unterstützung im Bereich Anforderung angezeigt](./media/storsimple-contact-microsoft-support/Ibiza5a.png) 

5. Klicken Sie im Dialogfeld **Problem** führen Sie folgende Schritte aus:

    1.  Wählen Sie **einen Schweregrad** aus der Dropdown-Liste aus.
    2.  Wählen Sie einen **Problemtyp** aus der Dropdown-Liste.
    3.  Wählen Sie eine **Kategorie** aus der Dropdown-Liste. 
    4.  Beschreiben Sie im Feld **Details** Ihr Problem.
    5.  Geben Sie im Feld **Zeit** Datum, Uhrzeit und Zeitzone, der das letzte Vorkommen von Ihrem Problem entspricht.
    6.  Klicken Sie unter **Datei hochzuladen**auf das Ordnersymbol, um Support-Paket zu suchen.
    7.  Aktivieren Sie das Kontrollkästchen **Freigabe Diagnoseinformationen** .

6. Klicken Sie auf **Weiter**. Das Dialogfenster **Kontaktinformationen** .

    ![Neue Unterstützung im Bereich Anforderung angezeigt](./media/storsimple-contact-microsoft-support/Ibiza6a.png) 

7. Geben Sie Ihre Kontaktinformationen und eine Kontaktmethode (telefonisch oder per e-Mail). 

8. Aktivieren Sie das Kontrollkästchen **für zukünftige Anfragen Kontakt speichern** .

9. Klicken Sie auf **Erstellen**.

Nachdem Ihr Antrag kontaktiert ein Supporttechniker Sie so bald wie möglich mit der Anforderung fortfahren.

## <a name="start-a-support-session-in-windows-powershell-for-storsimple"></a>Starten Sie eine Sitzung in Windows PowerShell für StorSimple

Um Probleme beheben, die mit dem StorSimple-Gerät auftreten, müssen Sie sich mit dem Microsoft-Support-Team. Microsoft Support müssen Support-Sitzung mit Ihrem Gerät anmelden. 

Führen Sie die folgenden Schritte aus, um eine Sitzung zu starten:

#### <a name="to-start-a-support-session"></a>Support-Sitzung starten

1. Zugreifen Sie das Gerät direkt mithilfe der seriellen Konsole oder über eine Telnet-Sitzung von einem Remotecomputer. Dazu gehen Sie in [Kitten Verbindung zu Gerät serielle Konsole verwenden](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).

2. Drücken Sie in der Sitzung, die geöffnet wird **die EINGABETASTE zu einer Befehlszeile** .

3. Im Menü seriellen Konsole Option 1, **Melden Sie sich mit Vollzugriff**.

4. Geben Sie dazu aufgefordert werden das folgende Kennwort: 

    `Password1`

5. Aufgefordert werden Geben Sie folgenden Befehl ein:

    `Enable-HcsSupportAccess`

6. Eine verschlüsselte Zeichenfolge wird Ihnen angezeigt werden. Kopieren Sie diese Zeichenfolge und in einen Texteditor wie Notepad.

7. Diese Zeichenfolge speichern und per e-Mail an Microsoft Support senden. 

> [AZURE.IMPORTANT] Deaktivieren-Support-Zugang mit `Disable-HcsSupportAccess`. StorSimple-Gerät versucht auch deaktivieren unterstützt 8 Stunden nach der Einleitung der Sitzung. In diesem Zusammenhang empfiehlt es sich, Anmeldedaten Gerät StorSimple ändern, nachdem Sie eine supportsitzung initiieren.
