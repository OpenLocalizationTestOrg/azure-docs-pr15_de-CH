<properties 
   pageTitle="Ersetzen Sie StorSimple EBOD-Controller | Microsoft Azure"
   description="Erläutert das Entfernen und Austauschen von mindestens EBOD-Controller auf einem Gerät StorSimple 8600."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="replace-an-ebod-controller-on-your-storsimple-device"></a>Ersetzen Sie einen EBOD-Controller auf dem StorSimple-Gerät

## <a name="overview"></a>Übersicht

In diesem Lernprogramm wird das Ersetzen einer fehlerhaften EBOD Controller-Modul auf Ihrem Gerät Microsoft Azure StorSimple erläutert. Um eine EBOD-Controller-Modul ersetzen, müssen Sie:

- Entfernen Sie den fehlerhaften EBOD-controller
- Installieren Sie einen neuen EBOD-controller

Beachten Sie die folgenden Informationen, bevor Sie beginnen:

- Leere EBOD Module müssen alle ungenutzte Steckplätze eingefügt. Gehäuse kühlt nicht ordnungsgemäß, wenn ein Slot geöffnet ist.

- EBOD-Controller ist hot-Swap- und entfernt oder ersetzt werden können. Fehlerhafte Modul nicht entfernt, bis Sie einen Ersatz haben. Wenn Sie den Prozess starten, müssen Sie es innerhalb von 10 Minuten abschließen.

>[AZURE.IMPORTANT] Sicherstellen Sie vor dem Entfernen oder Ersetzen einer Komponente StorSimple, dass [Sicherheit Symbol Konventionen](storsimple-safety.md#safety-icon-conventions) und andere [Sicherheitsmaßnahmen](storsimple-safety.md)zu überprüfen.

## <a name="remove-an-ebod-controller"></a>Entfernen eines EBOD-Controllers

Ersetzen fehlgeschlagene EBOD Controller-Modul in Ihr Gerät StorSimple stellen sicher, dass das EBOD Controllermodul aktiv ist und ausgeführt wird. Die folgenden Verfahren und Tabelle erläutert EBOD Controller-Modul entfernen.

#### <a name="to-remove-an-ebod-module"></a>EBOD-Modul entfernen

1. Azure-Verwaltungsportal zu öffnen.

2. Navigieren Sie zum **Geräte** > **Wartung** > **Hardwarestatus**, und stellen Sie sicher, dass der Status der LED für aktive EBOD-Controller-Modul grün ist, und die LED für fehlgeschlagene EBOD Controller-Modul Rot ist.

3. Suchen Sie fehlgeschlagene EBOD Controller-Modul auf der Rückseite des Geräts.

4. Entfernen Sie die Kabel, die EBOD Controller-Modul mit dem Controller zu verbinden, vor dem EBOD-Modul aus dem System.

5. Notieren Sie sich den genauen SAS Port EBOD-Controller-Modul, das mit dem Controller verbunden. Sie müssen diese Konfiguration des Systems wiederherstellen, nachdem das EBOD-Modul ersetzt. 

    >[AZURE.NOTE] Normalerweise wird dieser Anschluss A im folgenden Diagramm als **Host** gekennzeichnet ist.

    ![Rückwandplatine EBOD controller](./media/storsimple-ebod-controller-replacement/IC741049.png)

     **Abbildung 1** Rückseite EBOD-Modul

  	|Bezeichnung|Beschreibung|
  	|:----|:----------|
  	|1|Fehler-LED|
  	|2|LED-Betriebsanzeige|
  	|3|SAS-Anschlüsse|
  	|4|SAS-LEDs|
  	|5|Serielle Anschlüsse für Factory-Verwendung|
  	|6|Port (Host in)|
  	|7|Anschluss B (Host)|
  	|8|Port C (nur werkseitig verwenden)|

## <a name="install-a-new-ebod-controller"></a>Installieren Sie einen neuen EBOD-controller

Die folgenden Verfahren und Tabelle erläutert eine EBOD-Controller-Modul in Ihr StorSimple-Gerät installieren.

#### <a name="to-install-an-ebod-controller"></a>EBOD-Controller zu installieren

1. Überprüfen Sie das EBOD Gerät, insbesondere für die Schnittstelle. Installieren Sie den neuen EBOD-Controller nicht, wenn Stifte verbogen sind.

2. Schieben Sie mit Scharnieren in der geöffneten Position das Modul in das Gehäuse, bis Sie einrastet.

    ![EBOD-Controller installieren](./media/storsimple-ebod-controller-replacement/IC741050.png)

    **Abbildung 2**  EBOD-Controller-Modul installieren

3. Schließen Sie die Verriegelung. Sie sollten ein Klicken hören, wie die Verriegelung einrastet.

    ![EBOD Latch freigeben](./media/storsimple-ebod-controller-replacement/IC741047.png)

    **Abbildung 3**  Schließen die Verriegelung EBOD

4. Schließen Sie die Kabel. Verwenden der Konfigurations, die vor dem Austausch vorhanden war. Finden Sie im folgenden Diagramm und Tabelle Weitere Informationen dazu, wie Sie die Kabel.

    ![Das Gerät 4U Power cable](./media/storsimple-ebod-controller-replacement/IC770723.png)

    **Abbildung 4**. Erneutes Anschließen der Kabel

  	|Bezeichnung|Beschreibung|
  	|:----|:----------|
  	|1|Primäre Einheit|
  	|2|PCM 0|
  	|3|PCM 1|
  	|4|Controller 0|
  	|5|Domänencontroller 1|
  	|6|EBOD-Controller 0|
  	|7|EBOD-Controller 1|
  	|8|EBOD-Gehäuse|
  	|9|Power Distribution Units|

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [StorSimple Hardware-Komponente ersetzt](storsimple-hardware-component-replacement.md).
