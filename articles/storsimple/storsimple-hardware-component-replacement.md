<properties 
   pageTitle="Austausch von Hardware Komponenten StorSimple | Microsoft Azure"
   description="Beschreibt, wie sicher PCM, Akku, controllermodule, EBOD-Controller, Laufwerke und Gehäuse eines Geräts StorSimple ersetzen."
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
   ms.date="10/11/2016"
   ms.author="alkohli" />

# <a name="storsimple-hardware-component-replacement"></a>Austausch von Hardware Komponenten StorSimple

## <a name="overview"></a>Übersicht

Die Komponente Ersetzung Hardwarelernprogramme beschreiben Hardwarekomponenten des Geräts Microsoft Azure StorSimple 8000-Serie und die erforderlichen Schritte zum Entfernen und austauschen. Dieser Artikel beschreibt die Sicherheitssymbole,, enthält Verweise auf die ausführliche Lernprogramme und listet die Komponenten ersetzt werden.

>[AZURE.IMPORTANT] Sicherstellen Sie vor dem Entfernen oder Ersetzen einer Komponente StorSimple, dass [Sicherheit Symbol Konventionen](#safety-icon-conventions) und andere [Sicherheitsmaßnahmen](storsimple-safety.md)zu überprüfen.
 
### <a name="safety-icon-conventions"></a>Sicherheit Symbol Konventionen

Die folgende Tabelle beschreibt Sicherheitssymbole in diesen Lernprogrammen verwendet. Achten Sie insbesondere auf diese Sicherheitssymbole, wie Sie durch die Schritte zum Entfernen und Austauschen von Komponenten.

| Symbol | Text | Weitere Informationen |
|:---- |:---- |:-----------|
|![Warnsymbol](./media/storsimple-hardware-component-replacement/Warning.png)| **GEFAHR!** | Gibt eine gefährliche Situation, die nicht vermeiden, Tod oder eine schwere Körperverletzung führt. Dieses signalwort beschränkt den extremsten Situationen.|
|![Warnsymbol](./media/storsimple-hardware-component-replacement/Warning.png)| **WARNUNG!** | Gibt eine gefährliche Situation, die nicht vermeiden, Tod oder eine schwere Körperverletzung führen.|
|![Symbol "Vorsicht"](./media/storsimple-hardware-component-replacement/Caution.png)| **VORSICHT!** |Gibt eine gefährliche Situation, die nicht vermeiden, kleinere oder mittlere Schädigung führen.|
|![Hinweis-Symbol](./media/storsimple-hardware-component-replacement/NoticeIcon.png)| **HINWEIS:** | Gibt Informationen wichtig, aber nicht mit Gefahren verbunden.|
|![Stromschlag Symbol](./media/storsimple-hardware-component-replacement/Electric.png) | **Elektrische Stromschlag** | Hohen Spannung angibt.|
|![Heavyweight-Symbol](./media/storsimple-hardware-component-replacement/Weight.png)| **Schwere**| |
|![Kein Benutzer gewartet Parts-Symbol](./media/storsimple-hardware-component-replacement/NoUserServiceableParts.png)| **Keine Benutzer gewartet Parts** | Nicht greifen Sie zu, es sei denn, geschult.|
|![Symbol für Informationen lesen](./media/storsimple-hardware-component-replacement/ReadInstructions.png)|**Alle Informationen zuerst lesen**| |
|![Tipp Gefahr Symbol](./media/storsimple-hardware-component-replacement/TipHazard.png)|**Tipp Gefahr**| |

### <a name="before-you-begin"></a>Bevor Sie beginnen

Machen Sie sich mit Sicherheitsinformationen Gerät und Sicherheit in diesem Lernprogramm verwendeten Symbole. Ausführliche Informationen finden Sie in [problemlos installieren und Ihr StorSimple Gerät](storsimple-safety.md) . Achten Sie darauf, dass [Sicherheitsvorkehrungen](storsimple-safety.md#handling-precautions) überprüfen, bevor das Gerät StorSimple behandeln. 

Bevor Sie eine Komponente ersetzen, sollten Sie die folgende Informationen.

![Symbol](./media/storsimple-hardware-component-replacement/Warning.png) ![Elektroschocks Symbol](./media/storsimple-hardware-component-replacement/Electric.png) **Warnung!** 

- Erden Sie sich ordnungsgemäß mit einer elektrostatische Entladung oder antistatischen Matte Umgang von Modulen und Komponenten des Geräts StorSimple.

- Berühren Sie keine Schaltkreise. Verwenden Sie angegebenen Handles und Hilfslinien und Komponenten, die möglicherweise Schaltkreis ausgesetzt.

![Symbol](./media/storsimple-hardware-component-replacement/Warning.png) ![Symbol](./media/storsimple-hardware-component-replacement/NoticeIcon.png) **Hinweis:**

Wenn Sie ein Modul **lassen Sie niemals einen leeren Schacht an der Rückseite des Gehäuses**ersetzen. Erhalten Sie einen Ersatz oder leeres Modul vor entfernen das Problem.

## <a name="hardware-component-replacement-procedures"></a>Austausch von Hardware-Komponente

Das Gerät StorSimple 8000-Serie besteht verschiedene Zusatzmodule in Primär- und EBOD Gehäuse. 8100 hat eine primäre Gehäuse der 8600 ist ein dual-Gehäuse mit einem primären Gehäuse und EBOD-Gehäuse

Die wichtigsten Hardwarekomponenten in Ihrem Gerät sind in der folgenden Tabelle zusammengefasst. Klicken Sie in der Spalte **Verfahren** zugeordneten Lernprogramm zu.

|Komponenten|# Vorhanden|Plug-in-Modul?|Verfahren für den Austausch
|:---------|:--------|:--------------|:---------------------|
| Gehäuse|1|Nein|[Ersetzen Sie das Gehäuse auf dem StorSimple-Gerät](storsimple-chassis-replacement.md) |
|Primärer Domänencontroller|2|Ja| [Controller-Modul auf dem StorSimple-Gerät ersetzen](storsimple-controller-replacement.md) |
|764W Energie- und Kühlmodule (PCM)|2|Ja| [Ersetzen einer Stromversorgung und Kühlmodul auf dem StorSimple-Gerät](storsimple-power-cooling-module-replacement.md) |
|Backup-Batterie|2|Ja| [Ersetzen Sie das Modul backup-Batterie auf dem StorSimple-Gerät](storsimple-battery-replacement.md) |
|Festplatten|12|Ja| [Ersetzen einer Festplatte auf dem StorSimple-Gerät](storsimple-disk-drive-replacement.md) |

**Tabelle 1** Hardware-Komponenten der primären Einheit

Die primäre Einheit und EBOD Gehäuse unterscheiden sich in ihren e/a-Modulen. Außerdem verfügen die PCM andere Leistung. PCM in der primären Einheit sind 764 W, in EBOD-Gehäuse 580 W. PCM in der primären Einheit enthalten auch ein Modul backup-Batterie.

|Komponenten|# Vorhanden|Plug-in-Modul?| Verfahren für den Austausch
|:---------|:--------|:--------------|:---------------------|
|Gehäuse|1|Nein| [Ersetzen Sie das Gehäuse auf dem StorSimple-Gerät](storsimple-chassis-replacement.md) |
|EBOD-Controller|2|Ja| [Ersetzen Sie einen EBOD-Controller auf dem StorSimple-Gerät](storsimple-ebod-controller-replacement.md) |
|580 w bei Energie- und Kühlmodule (PCM)|2|Ja| [Ersetzen einer Stromversorgung und Kühlmodul auf dem StorSimple-Gerät](storsimple-power-cooling-module-replacement.md) |
|Festplatten|12|Ja| [Ersetzen einer Festplatte auf dem StorSimple-Gerät](storsimple-disk-drive-replacement.md) |

**Tabelle 2** Hardware-Komponenten EBOD-Gehäuse

Plug-in-Module auf dem Gerät werden in folgenden vorne und hinten Diagrammen hervorgehoben. Diese Diagramme können Sie um den Speicherort der verschiedenen Zusatzmodulen zu bestimmen, ob ein Ersatz erforderlich ist. Das vordere Diagramm zeigt Laufwerke und die hinteren Diagramme EBOD-Gehäuse und die primäre Einheit Plug-in-Module.

![Frontplane mit Festplatten](./media/storsimple-hardware-component-replacement/IC741028.png)

**Abbildung 1** Vorderseite

|Bezeichnung|Beschreibung|
|:----|:----------|
|0 - 11|Laufwerke (insgesamt 12)|

Die primäre Einheit und EBOD-Gehäuse haben Laufwerk Carrier Module. Das Gehäuse verfügt zwölf 3,5" Laufwerke in einem Format 3 x 4 angeordnet.

![Rückwandplatine Gerät primäre Einheit Module](./media/storsimple-hardware-component-replacement/IC740994.png)

**Abbildung 2** Rückseite der primären Einheit

|Bezeichnung|Beschreibung|
|:----|:----------|
|1|PCM 0|
|2|PCM 1|
|3|Controller 0|
|4|Domänencontroller 1|

![Rückwandplatine Gerät EBOD Gehäuse Plug-in-Module](./media/storsimple-hardware-component-replacement/IC769599.png)

**Abbildung 3** Rückseite des Gehäuses EBOD

|Bezeichnung|Beschreibung|
|:----|:----------|
|1|PCM 0|
|2|PCM 1|
|3|EBOD-Controller 0|
|4|EBOD-Controller 1|

## <a name="field-replaceable-units"></a>Ort austauschbare

Die folgenden Field replaceable Units (FRUs) sind für Ihr StorSimple-Gerät:

- Gehäuse (einschließlich der integrierten Bedienfeld)

- 764 W WECHSELSTROM PCM

- 580 W WECHSELSTROM PCM

- Festplatte mit Carrier Modul

- Controller-Modul

- EBOD-Controller-Modul

- Backup-Batterie-Modul

- Schienen-Kit für die Rackmontage

Bitte [wenden Sie sich an Microsoft Support Services](storsimple-contact-microsoft-support.md) , um diese Ersatzeinheiten bestellen.

## <a name="next-steps"></a>Nächste Schritte

Überprüfen Sie alle [Informationen](storsimple-safety.md) , bevor Sie versuchen, eine StorSimple Komponente ersetzen.
