<properties 
   pageTitle="Ersetzen Sie das Gehäuse auf einem Gerät StorSimple | Microsoft Azure"
   description="Beschreibt das Entfernen und Ersetzen Sie das Gehäuse für die primäre Einheit StorSimple oder EBOD-Gehäuse."
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

# <a name="replace-the-chassis-on-your-storsimple-device"></a>Ersetzen Sie das Gehäuse auf dem StorSimple-Gerät

## <a name="overview"></a>Übersicht

In diesem Lernprogramm erläutert und Gehäuse eines Geräts StorSimple 8000-Serie. StorSimple 8100-Modell ist ein Gehäuse (ein Gehäuse) der 8600 ein dual-Gehäuse (zwei Gehäuse). Für ein Modell 8600 sind möglicherweise zwei Gehäuse, die das Gerät ausfallen könnten: Gehäuse für primäre Gehäuse oder Gehäuse für das EBOD-Gehäuse.

In beiden Fällen ist das Austauschgehäuse von Microsoft ausgeliefert leer. Keine Stromversorgung und Kühlmodule (PCM) controllermodule solid-State-Festplatten (SSDs), Festplattenlaufwerken (HDDs) oder EBOD-Modulen sein enthalten.

>[AZURE.IMPORTANT] Entfernen und ersetzen das Gehäuse der Sicherheitshinweise [StorSimple Hardware-Komponente](storsimple-hardware-component-replacement.md)ersetzen.

## <a name="remove-the-chassis"></a>Entfernen Sie das Gehäuse

Die folgenden Schritte des Gehäuses auf dem StorSimple-Gerät entfernen.

#### <a name="to-remove-a-chassis"></a>Einem Gehäuse entfernen

1. Stellen Sie sicher, dass das Gerät StorSimple heruntergefahren und alle Stromquellen getrennt.

2. Entfernen Sie ggf. alle Netzwerk- und SAS-Kabel.

3. Entfernen Sie die Einheit aus dem Einbaugehäuse

4. Entfernen Sie alle Laufwerke, und beachten Sie die Steckplätze, aus denen sie entfernt werden. Weitere Informationen finden Sie unter [Entfernen der Festplatte](storsimple-disk-drive-replacement.md#remove-the-disk-drive).

5. Auf der EBOD-Gehäuse (ist dieser Fehler Gehäuse) das EBOD Controller entfernen. Weitere Informationen finden Sie unter [EBOD-Controller entfernen](storsimple-ebod-controller-replacement.md#remove-an-ebod-controller). 

    Auf die primäre Einheit (ist dieser Fehler Gehäuse) entfernen Sie die Controller und beachten Sie die Steckplätze aus dem entfernt. Weitere Informationen finden Sie in [einem Domänencontroller zu entfernen](storsimple-controller-replacement.md#remove-a-controller).

## <a name="install-the-chassis"></a>Installieren des Gehäuses

Führen Sie die folgenden Schritte aus, um das Gehäuse StorSimple installiert.

#### <a name="to-install-a-chassis"></a>Um ein Gehäuse

1. Montieren Sie das Gehäuse im rack Weitere Informationen finden Sie in [Ihr Gerät StorSimple 8100 für Rackmontage](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) oder [Rackmontage Geräts StorSimple 8600](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device).

2. Nach Gehäuse in Racks montiert, installieren Sie die controllermodule an den gleichen Positionen, denen sie zuvor in installiert wurden.

3. Installieren der Laufwerke in derselben Position und Steckplätze, denen sie zuvor in installiert wurden.

    >[AZURE.NOTE] Wir empfehlen die SSDs zuerst in den Steckplätzen installiert und installieren Sie die Festplatten.

2. Verbinden Sie mit dem Gerät im Rack und die Komponenten bereitgestellt das Gerät mit dem entsprechenden Stromquellen und das Gerät einschalten. Details finden Sie in [Ihr Gerät StorSimple 8100](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) oder [Kabel das Gerät StorSimple 8600](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [StorSimple Hardware-Komponente ersetzt](storsimple-hardware-component-replacement.md).

