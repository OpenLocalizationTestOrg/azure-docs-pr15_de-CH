<properties 
   pageTitle="Konfigurieren von CHAP für Ihr Gerät StorSimple | Microsoft Azure"
   description="Beschreibt das Challenge Handshake Authentication Protocol (CHAP) auf einem StorSimple-Gerät konfigurieren."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="configure-chap-for-your-storsimple-device"></a>Konfigurieren von CHAP für Ihr StorSimple-Gerät

In diesem Lernprogramm erläutert das CHAP für Ihr StorSimple-Gerät konfigurieren. In diesem Artikel beschriebenen Verfahren gilt für StorSimple-8000-Serie sowie StorSimple 1200 Geräte.

CHAP steht für Challenge Handshake Authentication Protocol. Es ist ein Authentifizierungsschema Server überprüft die Identität des Remoteclients. Die Prüfung basiert auf ein gemeinsames Kennwort oder geheimen Schlüssel. CHAP kann unidirektional sein (unidirektional) oder (bidirektional). Unidirektionale CHAP wird der Initiator authentifiziert. Gegenseitige oder umgekehrte CHAP erfordert auf der anderen Seite das Ziel authentifiziert den Initiator, der Initiator authentifiziert das Ziel. Initiator-Authentifizierung kann ohne Zielauthentifizierung implementiert werden. Ziel-Authentifizierung kann jedoch nur, wenn auch Initiator-Authentifizierung implementiert implementiert werden. 

Es wird empfohlen, CHAP iSCSI-Sicherheit zu verwenden.

>[AZURE.NOTE] Denken Sie daran, dass IPSEC StorSimple Geräte derzeit nicht unterstützt wird.

CHAP-Einstellungen auf dem Gerät StorSimple können folgendermaßen konfiguriert werden:

- Unidirektional oder unidirektionale Authentifizierung

- Bidirektionale oder gegenseitige oder reverse-Authentifizierung

In all diesen Fällen muss das Portal für das Gerät und die Server-iSCSI-Initiator-Software konfiguriert werden. Die detaillierten Schritte für diese Konfiguration sind in der folgenden Anleitung beschrieben.

## <a name="unidirectional-or-one-way-authentication"></a>Unidirektional oder unidirektionale Authentifizierung

Unidirektionale Authentifizierung authentifiziert das Ziel den Initiator. Diese Authentifizierung müssen Sie CHAP Initiator Settings auf dem StorSimple-Gerät und der iSCSI-Initiator-Software auf dem Host konfigurieren. Die detaillierten Verfahren für StorSimple-Gerät und Windows-Host werden beschrieben.

#### <a name="to-configure-your-device-for-one-way-authentication"></a>Das Gerät für die unidirektionale Authentifizierung konfigurieren

1. Klicken Sie im klassischen Azure-Portal, auf der Seite **Geräte** auf die Registerkarte **Konfigurieren** .

    ![CHAP-Initiator](./media/storsimple-configure-chap/IC740943.png)

2. Blättern Sie nach unten auf dieser Seite und im Abschnitt **Initiator-CHAP** :
                                                    
    1. Geben Sie einen Benutzernamen für die Initiator-CHAP.

    2. Geben Sie ein Kennwort für die CHAP-Initiator.

         > [AZURE.IMPORTANT] Der CHAP-Benutzername muss weniger als 233 Zeichen enthalten. Das CHAP-Kennwort muss zwischen 12 und 16 Zeichen lang sein. Eine längere Benutzername oder Kennwort führt eines Authentifizierungsfehlers Windows Host.
    
    3. Bestätigen Sie das Kennwort ein.

4. Klicken Sie auf **Speichern**. Eine Meldung wird angezeigt. Klicken Sie auf **OK** , um die Informationen zu speichern.

#### <a name="to-configure-one-way-authentication-on-the-windows-host-server"></a>Einweg-Authentifizierung auf dem Hostserver Windows konfigurieren

1. Starten Sie auf dem Hostserver Windows iSCSI-Initiator.

2. Im Fenster **iSCSI Initiator Properties** Schritte:
                                                    
    1. Klicken Sie auf die Registerkarte **Discovery** .

        ![iSCSI-Initiator-Eigenschaften](./media/storsimple-configure-chap/IC740944.png)

    2. Klicken Sie auf **Portal ermitteln**.

3. Im Dialogfeld **Zielportal erkennen** :
                                                    
    1. Geben Sie die IP-Adresse des Geräts.

    3. Klicken Sie auf **Erweitert**.

        ![Zielportal erkennen](./media/storsimple-configure-chap/IC740945.png)

4. Im Dialogfeld " **Erweiterte Einstellungen** ":
                                                    
    1. Aktivieren Sie das Kontrollkästchen **CHAP aktivieren anmelden** .

    2. Geben Sie im Feld **Name** den Benutzernamen für den CHAP-Initiator im klassischen Portal angegeben.

    3. Geben Sie im Feld **Target Secret** das CHAP Initiator im klassischen Portal angegebene Kennwort ein.

    4. Klicken Sie auf **OK**.

        ![Allgemeine erweitert](./media/storsimple-configure-chap/IC740946.png)

5. Auf der Registerkarte **Targets** im Fenster **iSCSI Initiator Properties** erscheint der Device-Status als **verbunden**. Bei Verwendung ein Geräts StorSimple 1200 wird dann jedes Volume als ein iSCSI-Ziel bereitgestellt werden wie unten dargestellt. Daher müssen Schritte 3 und 4 für jedes Volume wiederholt werden.

    ![Volumes, die als separate Ziele](./media/storsimple-configure-chap/chap4.png)

    > [AZURE.IMPORTANT] Wenn Sie den iSCSI-Namen ändern, der neue Namen für neue iSCSI-Sitzungen dienen. Ordner dienen nicht vorhandene Sitzung hinaus, bis Sie sich abmelden und Anmelden erneut.

Weitere Informationen zum Konfigurieren von CHAP auf dem Hostserver für Windows finden Sie [Weitere Hinweise](#additional-considerations).


## <a name="bidirectional-or-mutual-authentication"></a>Bidirektionale oder gegenseitige Authentifizierung

Bidirektionale Authentifizierung authentifiziert den Initiator und dann der Initiator authentifiziert das Ziel. Dies erfordert CHAP Initiator Settings sowie die umgekehrten CHAP-Einstellungen auf dem Gerät und iSCSI-Initiator-Software auf dem Host konfigurieren. Die folgenden Prozeduren beschreiben die Schritte, um gegenseitige Authentifizierung auf dem Gerät und auf dem Windows-Host konfigurieren.

#### <a name="to-configure-your-device-for-mutual-authentication"></a>So konfigurieren Sie das Gerät für die gegenseitige Authentifizierung

1. Klicken Sie im klassischen Azure-Portal, auf der Seite **Geräte** auf die Registerkarte **Konfigurieren** .

    ![CHAP-Ziel](./media/storsimple-configure-chap/IC740948.png)

2. Blättern Sie nach unten auf dieser Seite und im Abschnitt **CHAP-Ziel** :
                                                    
    1. Geben Sie einen **Reverse CHAP-Benutzernamen** für Ihr Gerät.

    2. Ein **Reverse CHAP-Kennwort** für das Gerät angeben.

    3. Bestätigen Sie das Kennwort ein.

3. **Initiator-CHAP** -Bereich:
                                                
    1. Geben Sie einen **Namen** für das Gerät.

    1. Geben Sie ein **Kennwort** für Ihr Gerät.

    3. Bestätigen Sie das Kennwort ein.

4. Klicken Sie auf **Speichern**. Eine Meldung wird angezeigt. Klicken Sie auf **OK** , um die Informationen zu speichern.

#### <a name="to-configure-bidirectional-authentication-on-the-windows-host-server"></a>Bidirektionale Authentifizierung auf Windows Hostserver konfigurieren

1. Starten Sie auf dem Hostserver Windows iSCSI-Initiator.

2. Klicken Sie im Fenster **iSCSI Initiator Properties** auf die Registerkarte **Konfiguration** .

3. Klicken Sie auf **CHAP**.

4. Klicken Sie im Dialogfeld **iSCSI Initiator gegenseitigen CHAP-Schlüssel** :
                                                    
    1. Geben Sie im klassischen Azure-Portal konfiguriert **CHAP-Kennwort umzukehren** .

    2. Klicken Sie auf **OK**.

        ![gegenseitiges CHAP für iSCSI-initiator](./media/storsimple-configure-chap/IC740949.png)

5. Klicken Sie auf die Registerkarte **Targets** .

6. Klicken Sie auf die Schaltfläche **Verbinden** . 

7. Klicken Sie im Dialogfeld **Verbindung zum Ziel** auf **Erweitert**.

8. Im Dialogfeld **Erweiterte Eigenschaften** :
                                                    
    1. Aktivieren Sie das Kontrollkästchen **CHAP aktivieren anmelden** .

    2. Geben Sie im Feld **Name** den Benutzernamen für den CHAP-Initiator im klassischen Portal angegeben.

    3. Geben Sie im Feld **Target Secret** das CHAP Initiator im klassischen Portal angegebene Kennwort ein.

    4. Aktivieren Sie das Kontrollkästchen **gegenseitige Authentifizierung durchführen** .

        ![Gegenseitige Authentifizierung erweitert](./media/storsimple-configure-chap/IC740950.png)

    5. Klicken Sie auf **OK** , um die CHAP-Konfiguration
     
Weitere Informationen zum Konfigurieren von CHAP auf dem Hostserver für Windows finden Sie [Weitere Hinweise](#additional-considerations).

## <a name="additional-considerations"></a>Weitere Aspekte

Das **Schnelle** Feature unterstützt keine Verbindungen, die CHAP aktiviert. Wenn CHAP aktiviert ist, stellen Sie sicher, dass die Schaltfläche **Verbinden** , die auf der Registerkarte **Targets** Verbindung zum Ziel.

![Auf Verbindung](./media/storsimple-configure-chap/IC740947.png)

Im Dialogfeld **Verbindung auf** angezeigt wird das Kontrollkästchen Sie **auf der Liste der Favoriten hinzufügen** . Dadurch wird sichergestellt, dass bei jedem des Computers Neustart versucht wird, die Verbindung mit den bevorzugten iSCSI-Zielen wiederherzustellen.

## <a name="errors-during-configuration"></a>Fehler bei der Konfiguration

Wenn die CHAP-Konfiguration nicht korrekt ist, sind Sie wahrscheinlich eine Fehlermeldung **Authentifizierungsfehler** .

## <a name="verification-of-chap-configuration"></a>Überprüfung der CHAP-Konfiguration

Um zu überprüfen, ob CHAP verwendet wird, indem Sie die folgenden Schritte aus.

#### <a name="to-verify-your-chap-configuration"></a>Der CHAP-Konfiguration

1. Klicken Sie auf **bevorzugte Ziele**.

2. Wählen Sie das Ziel für das Authentifizierung aktiviert.

3. Klicken Sie auf **Details**.

    ![iSCSI-Initiator Eigenschaften bevorzugten Zielen](./media/storsimple-configure-chap/IC740951.png)

4. Klicken Sie im Dialogfeld **Bevorzugte Ziel Details** Beachten Sie den Eintrag im Feld **Authentifizierung** . Wenn die Konfiguration erfolgreich war, sollte **CHAP**angezeigt werden.

    ![Bevorzugtes Ziel details](./media/storsimple-configure-chap/IC740952.png)

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [StorSimple Sicherheit](storsimple-security.md).

- Weitere Informationen zur [Verwendung des StorSimple Manager-Dienstes zum Verwalten von StorSimple-Gerät](storsimple-manager-service-administration.md)
