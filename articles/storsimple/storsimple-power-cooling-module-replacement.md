<properties 
   pageTitle="Ersetzen einer PCM auf Ihrem Gerät StorSimple | Microsoft Azure"
   description="Erläutert das Entfernen und Ersetzen der Stromversorgung und Kühlung Modul (PCM) auf dem StorSimple-Gerät"
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
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="replace-a-power-and-cooling-module-on-your-storsimple-device"></a>Ersetzen einer Stromversorgung und Kühlmodul auf dem StorSimple-Gerät

## <a name="overview"></a>Übersicht

Stromversorgung und Kühlung Modul (PCM) in Ihrem Microsoft Azure StorSimple-Gerät besteht aus einer Stromversorgung und Kühlung, die durch Primär- und EBOD Gehäuse gesteuert werden. Es ist nur ein Modell für jede Einheit zertifiziert PCM. Die primäre Einheit für 764 W PCM zertifiziert und EBOD-Gehäuse ist für 580 W PCM zertifiziert. Unterscheiden der PCM für die primäre Einheit und EBOD-Gehäuse ist das Verfahren identisch.

In diesem Lernprogramm wird erläutert, wie Sie:

- PCM entfernen
- Setzen Sie einen Ersatz PCM

>[AZURE.IMPORTANT] Vor dem Entfernen und Austauschen von PCM, die Sicherheitshinweise [StorSimple Hardware-Komponente](storsimple-hardware-component-replacement.md)ersetzen.

## <a name="before-you-replace-a-pcm"></a>Bevor Sie ein PCM ersetzen

Die folgenden Faktoren bewusst sein, bevor die PCM ersetzen:

- Ausfall die Stromversorgung des PCM lassen Sie das fehlerhafte Modul installiert, aber ziehen Sie das Netzkabel. Der Lüfter weiterhin vom Gehäuse mit Strom versorgt und weiterhin ausreichende Kühlung. Wenn der Lüfter ausfällt, muss das PCM ausgetauscht werden.

- Vor dem Entfernen des PCM, unterbrechen Sie die PCM ausschalten (sofern vorhanden) oder durch das Netzkabel entfernen. Dies ist einer Warnung für Ihr System ein Ausschaltvorgang bevorsteht.

- Sicherstellen Sie, dass das PCM für kontinuierliche Betrieb vor fehlerhafte PCM ist. Ein fehlerhaftes PCM muss so bald wie möglich durch ein betriebsbereites PCM ersetzt.

- PCM Austausch dauert nur wenige Minuten, aber es muss innerhalb von 10 Minuten entfernt ausgefallene PCM um Überhitzung abgeschlossen.

- Beachten Sie, dass ab Werk ausgelieferten Ersatz 764 W PCM Module keine backup-Batterie-Modul enthalten. Sie müssen Entfernen von fehlerhaften PCM und fügen Sie es in das Ersatzmodul vor dem Austausch. Weitere Informationen finden Sie unter [Entfernen](storsimple-battery-replacement.md)und backup-Batteriemodul einfügen.


## <a name="remove-a-pcm"></a>PCM entfernen

Gehen Sie folgendermassen vor, wenn eine Stromversorgung und Kühlung Modul (PCM) von Ihrem Microsoft Azure StorSimple Gerät entfernen möchten.

>[AZURE.NOTE] Vor dem Entfernen der PCM sicher, dass Sie richtigen Ersatz (764 W für die primäre Einheit) oder 580 W für das EBOD-Gehäuse.

#### <a name="to-remove-a-pcm"></a>PCM entfernen

1. Klicken Sie in der Azure-Verwaltungsportal auf **Geräte** > **Wartung** > **Hardware-Status**. Status der PCM-Komponenten unter **Freigegebene Komponenten** identifizieren PCM fehlgeschlagen ist:

     - Wenn ein Netzteil in PCM 0 fehlgeschlagen ist, wird der Status der **Stromversorgung im PCM 0** rot angezeigt.

     - Wenn ein Netzteil in PCM 1 ausgefallen ist, wird der Status der **Stromversorgung PCM 1** rot angezeigt.

     - Wenn PCM 1 Lüfter ausgefallen ist, wird der Status der **Kühlung für PCM 0 0** oder **1 für PCM 0 Kühlung** rot angezeigt.

2. Suchen Sie nach fehlerhaften PCM auf der Geräterückseite der primären Einheit. Ausführen ein Modells 8600 identifizieren Sie die primäre Einheit an die System Einheit ID-Nummer auf der Vorderseite LED-Anzeige angezeigt. Die Standard-Einheit-ID auf der primären Einheit angezeigt ist **00**Standard EBOD-Gehäuse Unit ID angezeigt **01**. Das folgende Diagramm und Tabelle erläutern die Vorderseite des Displays.

    ![System-ID auf Frontblende OPS](./media/storsimple-power-cooling-module-replacement/IC740991.png)

     **Abbildung 1** Vorderseite des Geräts  

  	|Bezeichnung|Beschreibung|
  	|:---|:-----------|
  	|1|Stummtaste|
  	|2|Stromversorgung|
  	|3|Modul-Fehler|
  	|4|Logische Fehler|
  	|5|Einheit-ID anzeigen|

3. Die Überwachung LED in der primären Einheit kann auch zu fehlerhaften PCM verwendet werden. Anzeigen Sie das folgende Diagramm und Tabelle wie die LEDs zu fehlerhaften PCM Angenommen, leuchtet die LED **Lüfterausfall** der Lüfter ausgefallen. Ebenso leuchtet die LED **AC Fehler** Netzteil ausgefallen. 

    ![Rückwandplatine Gerät PCM Überwachung-LED](./media/storsimple-power-cooling-module-replacement/IC740992.png)

     **Abbildung 2** Rückseite PCM mit LED

  	|Bezeichnung|Beschreibung|
  	|:---|:-----------|
  	|1|AC-Stromausfall|
  	|2|Fehler beim Lüfter|
  	|3|Batterie defekt|
  	|4|PCM-OK|
  	|5|DC Stromausfall|
  	|6|Akku fehlerfrei|

4. Finden Sie im folgenden Diagramm der Rückseite des Geräts StorSimple fehlerhafte PCM-Modul gefunden. PCM 0 links und PCM 1 auf der rechten Seite. Die folgende Tabelle erläutert die Module.

     ![Rückwandplatine Gerät primäre Einheit Module](./media/storsimple-power-cooling-module-replacement/IC740994.png)

     **Abbildung 3** Rückseite mit Zusatzmodulen 

  	|Bezeichnung|Beschreibung|
  	|:---|:-----------|
  	|1|PCM 0|
  	|2|PCM 1|
  	|3|Controller 0|
  	|4|Domänencontroller 1|

5. Fehlerhafte PCM schalten Sie, und trennen Sie das Netzkabel. Sie können jetzt das PCM entfernen.

6. Fassen Sie den Hebel und die Seite PCM zwischen Daumen und Zeigefinger, und drücken sie, um das Handle öffnen.

    ![PCM-Handle öffnen](./media/storsimple-power-cooling-module-replacement/IC740995.png)

    **Abbildung 4** Der PCM öffnen

7. Der Griff und PCM entfernen.

    ![Entfernen eines Geräts PCM](./media/storsimple-power-cooling-module-replacement/IC740996.png)

    **Abbildung 5** PCM entfernen

## <a name="install-a-replacement-pcm"></a>Setzen Sie einen Ersatz PCM

PCM in Ihrem StorSimple-Gerät zu installieren, gehen Sie wie folgt vor. Sicherstellen Sie, dass Sie backup-Batterie-Modul vor dem Einbau der neuen PCM (gilt für nur 764 W PCM) eingefügt. Weitere Informationen finden Sie unter [Entfernen](storsimple-battery-replacement.md)und backup-Batteriemodul einfügen.

#### <a name="to-install-a-pcm"></a>PCM installieren

1. Überprüfen Sie die korrekte Ersetzung PCM für diese Einheit. Die primäre Einheit 764 W PCM und EBOD Gehäuse 580 W PCM. Sie sollten nicht versuchen, 580 W PCM in der primären Einheit oder 764 W PCM in EBOD-Gehäuse. Die folgende Abbildung zeigt, wo diese Informationen auf dem Etikett zu identifizieren, die PCM angebracht ist.

    ![Gerät PCM-Bezeichnung](./media/storsimple-power-cooling-module-replacement/IC740973.png)

    **Abbildung 6** PCM-Bezeichnung

2. Schäden am Gehäuse, insbesondere die Connectors überprüfen. 
                                        
    >[AZURE.NOTE] **Installieren Sie das Modul nicht, wenn Kontaktstifte verbogen sind.**

3. Schieben Sie mit dem PCM Griff in der geöffneten Position das Modul in das Gehäuse.

    ![Installieren des Geräts PCM](./media/storsimple-power-cooling-module-replacement/IC740975.png)

    **Abbildung 7** Installieren von PCM

4. Schließen Sie PCM-Handle manuell. Sie sollten ein Klicken hören, als Handle Verriegelung einrastet. 
                                        
    >[AZURE.NOTE] Um sicherzustellen, dass die Steckerstifte beteiligt sind, können Sie vorsichtig am Griff ziehen, ohne die Verriegelung. Wenn PCM Folien, impliziert dies, dass die Verriegelung geschlossen wurde, bevor die Connectors engagiert.

5. Schließen Sie die Netzkabel an die Stromversorgung und PCM.

6. Sichern Sie die Dehnung Befreiung ballen. 

7. PCM aktivieren.

8. Stellen Sie sicher, dass der Austausch erfolgreich war: im klassischen Azure-Portal des Dienstes StorSimple Manager, navigieren Sie zum **Geräte** > **Wartung** > **Hardware-Status**. Unter **Freigegebene Komponenten**sollte der Status des PCM grün. 
                                        
    >[AZURE.NOTE] Austausch PCM vollständig initialisiert einige Minuten dauern.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [StorSimple Hardware-Komponente ersetzt](storsimple-hardware-component-replacement.md).
