<properties 
   pageTitle="Access Control-Datensätze für StorSimple Virtual Array verwalten | Microsoft Azure"
   description="Beschreibt das Verwalten von Access Control Datensätze (ACRs) um festzustellen, welche Hosts auf ein Volume auf StorSimple Virtual Array zugreifen können."
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
   ms.date="05/03/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-access-control-records-for-the-storsimple-virtual-array"></a>Verwenden des StorSimple Manager-Dienstes Zugriff Steuerelement Datensätze für StorSimple Virtual Array verwalten 

## <a name="overview"></a>Übersicht

Access Control Datensätze (ACRs) können Sie angeben, welche Hosts auf einem Volume im StorSimple Virtual Array (auch bekannt als StorSimple für lokale virtuelle Gerät) herstellen können. ACRs sind auf einen bestimmten Datenträger und iSCSI-qualifizierte Namen (IQNs) Hosts enthalten. Wenn ein Host versucht, einen Datenträger herstellen, überprüft das Volume IQN-Namen zugeordnete ACR und wenn eine Übereinstimmung vorliegt, die Verbindung wird hergestellt. Abschnitt **Access Control Datensätze** auf der Seite **Konfigurieren** zeigt alle Datensätze Access Control mit der entsprechenden IQNs Hosts.

In diesem Lernprogramm wird folgenden allgemeinen ACR bezogene Aufgaben erläutert:

- Abrufen der IQN
- Hinzufügen eines Steuerelements Zugriff 
- Bearbeiten eines Datensatzes Access control 
- Löschen eines Datensatzes Access control 

> [AZURE.IMPORTANT] 
> 
> - Wenn ein Volume ein ACR zuweisen, achten Sie die Lautstärke von mehr als einem nicht gruppierten Server nicht gleichzeitig zugegriffen werden kann, da dies das Volume beschädigt. 
> - Beim Löschen eines ACR von einem Volume stellen Sie sicher, dass der entsprechende Host nicht das Volume zugreift, da das Löschen einer schreibgeschützten Unterbrechung führen.

## <a name="get-the-iqn"></a>Abrufen der IQN

Führen Sie die folgenden Schritte zum Abrufen der IQN ein Windows-Host, der Windows Server 2012.

[AZURE.INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]

## <a name="add-an-acr"></a>Fügen Sie ein ACR hinzu

Mithilfe **die StorSimple Manager-Konfigurationsseite** ACRs hinzufügen. In der Regel ordnen Sie eine ACR mit ein.

Informationen zum Zuordnen eines ACR mit gehen Sie zum [Hinzufügen eines Datenträgers](storsimple-ova-deploy3-iscsi-setup.md#step-3-add-a-volume).

>[AZURE.IMPORTANT] 
> 
>Wenn ein Volume ein ACR zuweisen, achten Sie die Lautstärke von mehr als einem nicht gruppierten Server nicht gleichzeitig zugegriffen werden kann, da dies das Volume beschädigt.
 
Die folgenden Schritte ein ACR hinzufügen.

#### <a name="to-add-an-acr"></a>Hinzufügen ein ACR

1. Zielseite Service wählen Sie den Dienst, doppelklicken Sie auf den Namen, und klicken Sie dann auf die Registerkarte **Konfiguration** .

    ![Registerkarte "Konfiguration"](./media/storsimple-ova-manage-acrs/acr1.png)

2. Tabellarische Liste unter **Access Control-Datensätze**Geben Sie einen **Namen** für Ihre ACR.

3. Geben Sie unter **iSCSI-Initiatornamen**IQN-Namen des Windows-Host. 

4. Klicken Sie am unteren Rand der Seite **Speichern** , um die neu erstellte ACR speichern. Sie sehen die folgende Meldung angezeigt.

    ![Bestätigung](./media/storsimple-ova-manage-acrs/acr2.png)

5. Klicken Sie auf das Häkchensymbol ![Symbol überprüfen](./media/storsimple-ova-manage-acrs/check-icon.png). Tabellarische Liste wird aktualisiert, um diese Ergänzung widerzuspiegeln.

## <a name="edit-an-acr"></a>Bearbeiten Sie ein ACR

Mithilfe die Seite **Konfiguration** im klassischen Azure-Portal ACRs bearbeiten. 

> [AZURE.NOTE] Derzeit nicht in Verwendung sind sollte die ACRs geändert werden. Bearbeiten ein ACR zugeordnete ein Volume, das momentan verwendet wird sollten Sie zunächst das Volume offline.

Führen Sie die folgenden Schritte zum Bearbeiten eines ACR.

#### <a name="to-edit-an-acr"></a>So bearbeiten Sie eine ACR

1. Zielseite Service wählen Sie den Dienst, doppelklicken Sie auf den Namen, und klicken Sie dann auf die Registerkarte **Konfiguration** .

2. Bewegen Sie Tabellarische Liste der Access Control Datensätze über ACR, die Sie ändern möchten.

3. Geben Sie einen neuen und/oder IQN der ACR.

4. Klicken Sie am unteren Rand der Seite **Speichern** , um die geänderte ACR speichern. Sie sehen eine Meldung angezeigt. 

5. Klicken Sie auf das Häkchensymbol ![Symbol überprüfen](./media/storsimple-ova-manage-acrs/check-icon.png). Tabellarische Liste wird entsprechend aktualisiert.

## <a name="delete-an-access-control-record"></a>Löschen eines Datensatzes Access control

Mithilfe die Seite **Konfiguration** im klassischen Azure-Portal ACRs löschen. 

> [AZURE.NOTE] 
> 
> - Sie sollten diese ACRs löschen, die derzeit nicht verwendet. Um eine ACR zugeordnete ein Volume, das momentan verwendet wird löschen, sollten Sie zuerst das Volume offline schalten.
> - Beim Löschen eines ACR von einem Volume stellen Sie sicher, dass der entsprechende Host nicht das Volume zugreift, da das Löschen einer schreibgeschützten Unterbrechung führen.

Führen Sie die folgenden Schritte aus, um eine Access Control löschen.

#### <a name="to-delete-an-access-control-record"></a>So löschen Sie einen Zugriffsdatensatz Steuerelement

1. Zielseite Service wählen Sie den Dienst, doppelklicken Sie auf den Namen, und klicken Sie dann auf die Registerkarte **Konfiguration** .

2. Bewegen Sie die Tabellarische Liste der Access Control Datensätze (ACRs) über ACR, die Sie löschen möchten.

3. Löschsymbol (**X**) erscheint in der rechten Spalte extreme ACR, die Sie auswählen. Klicken Sie auf **X** , um die ACR löschen. Sie sehen die folgende Meldung angezeigt.

    ![Bestätigung](./media/storsimple-ova-manage-acrs/acr3.png)

5. Klicken Sie auf das Häkchensymbol ![Symbol überprüfen](./media/storsimple-ova-manage-acrs/check-icon.png). Tabellarische Liste aktualisiert die Streichung.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen über [Volumes hinzufügen und Konfigurieren von ACRs](storsimple-ova-deploy3-iscsi-setup.md#step-3-add-a-volume).
