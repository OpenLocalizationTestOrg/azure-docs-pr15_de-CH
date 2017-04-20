<properties 
   pageTitle="PowerShell für StorSimple Device Management | Microsoft Azure"
   description="Erfahren Sie, wie mit Windows PowerShell für StorSimple StorSimple-Gerät verwalten."
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
   ms.date="08/18/2016"
   ms.author="alkohli@microsoft.com" />

# <a name="use-windows-powershell-for-storsimple-to-administer-your-device"></a>Verwenden Sie Windows PowerShell für StorSimple, um Ihr Gerät verwalten

## <a name="overview"></a>Übersicht

Windows PowerShell für StorSimple bietet eine Befehlszeilenschnittstelle, mit denen Sie Ihr Microsoft Azure StorSimple-Gerät verwalten. Wie der Name schon sagt, ist eine eingeschränkte Runspace basiert Befehlszeilenschnittstelle Windows PowerShell basiert. Aus Sicht des Benutzers in der Befehlszeile wird ein eingeschränkter Runspace als eine eingeschränkte Version von Windows PowerShell. Gleichzeitig einige grundlegende Funktionen von Windows PowerShell hat diese Schnittstelle zusätzliche dedizierte Cmdlets, die Verwaltung der Microsoft Azure StorSimple Geräte ausgerichtet sind. 

Dieser Artikel beschreibt das Windows PowerShell für StorSimple KEs wie Verbindung diese Schnittstelle und enthält Links zu Verfahren oder Workflows, die Sie mit dieser Schnittstelle ausführen können. Workflows enthalten wie Ihr Gerät registrieren, Konfigurieren der Netzwerkschnittstelle auf dem Gerät installieren, das muss im Wartungsmodus, Ändern des Geräts, und beheben Sie alle Probleme, die auftreten können.

Nach dem Lesen dieses Artikels können, Sie:

- Verbinden Sie mit Geräts StorSimple mit Windows PowerShell für StorSimple.

- Verwalten Sie das StorSimple-Gerät mithilfe von Windows PowerShell für StorSimple.

- Hilfe in Windows PowerShell für StorSimple.

>[AZURE.NOTE]   

>- Windows PowerShell-Cmdlets StorSimple können Sie Ihr StorSimple Gerät über eine serielle Konsole oder Remote über Windows PowerShell-Remoting. Weitere Informationen zu einzelnen Cmdlets, die in dieser Schnittstelle verwendet werden kann, finden Sie mit [Cmdlets für Windows PowerShell für StorSimple](https://technet.microsoft.com/library/dn688168.aspx).

>- Azure PowerShell StorSimple-Cmdlets sind eine andere Sammlung von Cmdlets, die Service Level StorSimple automatisiert und Schritte für die Migration von der Befehlszeile aus. Finden Sie weitere Informationen zu den Azure-PowerShell-Cmdlets für StorSimple der [Azure-StorSimple-Cmdlet Verweis](https://msdn.microsoft.com/library/azure/dn920427.aspx).

Sie können Windows PowerShell für StorSimple mit einer der folgenden Methoden aufrufen:

- [Verbinden Sie mit StorSimple Gerät serielle Konsole](#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console)
- [Eine Remoteverbindung mit StorSimple mit Windows PowerShell](#connect-remotely-to-storsimple-using-windows-powershell-for-storsimple)
    

## <a name="connect-to-windows-powershell-for-storsimple-via-the-device-serial-console"></a>Mit Windows PowerShell StorSimple über die serielle Konsole Gerät verbinden

Können [kitten herunterladen](http://www.putty.org/) oder ähnliche Terminal-Emulationssoftware zum Windows PowerShell StorSimple herstellen. Sie müssen kitten speziell für Microsoft Azure StorSimple Gerät zugreifen. Die folgenden Themen enthalten Informationen kitten konfigurieren und das Gerät. Verschiedene Menüoptionen in der seriellen Konsole erklärt.

### <a name="putty-settings"></a>PuTTY Optionen

Stellen Sie sicher, Folgendes PuTTY zu verwenden, mit der seriellen Konsole eine Verbindung zu Windows PowerShell-Schnittstelle.

#### <a name="to-configure-putty"></a>Kitten konfigurieren

1. Wählen Sie im Bereich **Kategorie** im Dialogfeld PuTTY **Rekonfiguration** **Tastatur**.

2. Stellen Sie sicher, dass die folgenden Optionen ausgewählt sind (Dies sind die Standardeinstellungen beim Starten einer neuen Sitzung). 

  	|Tastatur-Element|Wählen Sie|
  	|---|---|
  	|RÜCKTASTE|Steuerelement-? (127)|
  	|POS1 und Ende-Tasten|Standard|
  	|Funktionstasten und Tastatur|ESC [n ~|
  	|Ausgangszustand der Pfeiltasten|Normal|
  	|Ausgangszustand der Zehnertastatur|Normal|
  	|Zusätzliche Tastatur Features aktivieren|STRG + ALT + unterscheidet AltGr|

    ![Unterstützte Putty Settings](./media/storsimple-windows-powershell-administration/IC740877.png)

3. Klicken Sie auf **Übernehmen**.

4. Wählen Sie im Bereich **Kategorie** **Übersetzung**.

5. Wählen Sie im Listenfeld **Remote Zeichensatz** **UTF-8**.

6. Wählen Sie unter **Strichzeichnung Zeichen behandeln** **mit Zeichnung Codepunkte**. Die folgende Abbildung zeigt die Auswahl der richtigen PuTTY.

    ![UTF Putty Settings](./media/storsimple-windows-powershell-administration/IC740878.png)

7. Klicken Sie auf **Übernehmen**.


Kitten können nun folgendermaßen auf die serielle Konsole Gerät verbinden.

[AZURE.INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]


### <a name="about-the-serial-console"></a>Über die serielle Konsole

Beim Zugriff auf die Windows PowerShell-Schnittstelle des Geräts StorSimple über die serielle Konsole eine bannermeldung angezeigt, gefolgt von Menüoptionen. 

Banner-Nachricht enthält grundlegende StorSimple Geräteinformationen wie Modell, Name, installierte Softwareversion und Status des Controllers, auf die Sie zugreifen. Die folgende Abbildung zeigt ein Beispiel einer Nachricht Banner.

![Serielle Banner Nachricht](./media/storsimple-windows-powershell-administration/IC741098.png)

>[AZURE.IMPORTANT] Banner-Nachricht können Sie ermitteln, ob der Controller verbunden sind aktiv oder Passiv ist.

Die folgende Abbildung zeigt die verschiedenen Runspace verfügbaren Optionen im Menü seriellen Konsole.

![Registrieren Sie Ihr Gerät 2](./media/storsimple-windows-powershell-administration/IC740906.png)

Sie können aus folgenden Optionen auswählen:

1. **Melden Sie sich mit Vollzugriff** Diese Option ermöglicht (mit den entsprechenden Anmeldeinformationen) Runspace **SSAdminConsole** auf dem lokalen Domänencontroller. (Der lokale Domänencontroller ist der Controller über die serielle Konsole des Geräts StorSimple zugreift.) Diese Option kann auch verwendet werden, zu Microsoft Support auf uneingeschränkten Runspace (supportsitzung) mögliche Gerät Probleme analysieren. Nachdem Sie Option 1 anmelden, können Sie Microsoft Support-Techniker an uneingeschränkte Runspace mit einem bestimmten Cmdlet zugreifen. Weitere [Support-Sitzung](storsimple-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple)starten.

2. **Melden Sie sich beim Peer-Controller mit Vollzugriff** Diese Option ist identisch mit Option 1, außer dass (mit den entsprechenden Anmeldeinformationen) **SSAdminConsole** Runspace auf dem Peer-Controller herstellen können. Ist das Gerät StorSimple Gerät hohe Verfügbarkeit mit zwei Domänencontroller in einer Aktiv / Passiv-Konfiguration bezeichnet Peer der Controller das Gerät über die serielle Konsole zugreifen).
Option 1 ähnlich, kann diese Option auch verwendet werden zu Microsoft Support auf uneingeschränkten Runspace auf einem Peer.

3. **Verbinden mit eingeschränktem Zugriff** Diese Option dient Windows PowerShell Benutzeroberfläche im eingeschränkten Modus. Sie werden keine Anmeldeinformationen aufgefordert. Diese Option verbindet einen eingeschränkteren Runspace im Vergleich zu Optionen 1 und 2.  Einige der Aufgaben, die über die Option 1, **kann nicht* in diesen Runspace ausgeführt werden:

    - Die Herstellungsstandards verwenden
    - Kennwort ändern
    - Aktivieren oder Deaktivieren der Unterstützung für access
    - Anwenden von updates
    - Installieren von hotfixes 
                                                

    >[AZURE.NOTE] **Dies ist die bevorzugte Option Gerät Administratorkennwort vergessen haben und keine Verbindung mit Option 1 oder 2.**

4. **Sprache ändern** Mit dieser Option können Sie die Anzeigesprache für die Windows PowerShell-Schnittstelle ändern. Die unterstützten Sprachen sind Englisch, Japanisch, Russisch, Französisch, Südkorea, Spanisch, Italienisch, Deutsch, Chinesisch und Brasilianisches Portugiesisch.


## <a name="connect-remotely-to-storsimple-using-windows-powershell-for-storsimple"></a>Eine Remoteverbindung mit StorSimple mit Windows PowerShell für StorSimple

Windows PowerShell-Remoting können Sie Ihr StorSimple-Gerät herstellen. Wenn Sie auf diese Weise verbinden, sehen Sie ein Menü nicht. (Sie sehen ein Menü nur wenn Sie die serielle Konsole auf dem Gerät herstellen. Herstellen einer Remoteverbindung gelangen Sie direkt zu "Option 1 – Vollzugriff" an der seriellen Konsole.) Mit Windows PowerShell-Remoting verbinden Sie mit einem bestimmten Runspace. Sie können auch die Anzeigesprache. 

Die Sprache ist unabhängig von der Sprache, die mit der Option **Sprache ändern** im Menü seriellen Konsole festgelegt. Remote-PowerShell übernehmen automatisch das Gebietsschema des Geräts von angeschlossen werden, wenn keines angegeben ist.

>[AZURE.NOTE] Wenn Sie mit Microsoft Azure virtuelle Hosts und virtuellen Geräte StorSimple arbeiten, können Windows PowerShell-Remoting und virtuellen Hosts Sie das virtuelle Gerät herstellen. Wenn Sie ein freigegebenes Verzeichnis auf dem Host, Informationen von der Windows PowerShell-Sitzung speichern eingerichtet haben, beachten Sie die wichtigsten jeder authentifizierte Benutzer enthält. Wenn Sie der Freigabe jeder Zugriff eingerichtet haben und ohne Angabe von Anmeldeinformationen herstellen im nicht authentifizierte Prinzipal anonyme verwendet und ein Fehler angezeigt. Zum Beheben dieses Problems, die für die Freigabe Sie müssen das Gastkonto aktivieren und geben den Gast uneingeschränkten Zugriff auf die Freigabe oder geben Sie gültige Anmeldeinformationen zusammen mit Windows PowerShell-Cmdlet.

HTTP oder HTTPS können über Windows PowerShell-Remoting verbinden. Verwenden der Informationen in die folgenden Lernprogramme:

- [Herstellen Sie Remote über HTTP](storsimple-remote-connect.md#connect-through-http)
- [Verbinden Sie Remote über HTTPS](storsimple-remote-connect.md#connect-through-https)

## <a name="connection-security-considerations"></a>Verbindung-Sicherheitsaspekte

Wenn Sie Windows PowerShell für StorSimple Verbindung entscheiden, beachten Sie Folgendes:

- Die serielle Konsole über Netzwerk-Switches ist nicht direkt an die serielle Konsole Gerät sicher ist Seien Sie auf Sicherheitsrisiko, beim Verbinden mit seriellen Gerät über Netzwerk-Switches.

- Verbindung über eine HTTP-Sitzung bieten möglicherweise mehr Sicherheit als über die serielle Konsole über Netzwerk verbinden. Obwohl dies nicht die sicherste Methode ist, ist es auf vertrauenswürdige Netzwerke.

- Verbindung über eine HTTPS-Sitzung ist die sicherste und empfohlen.


## <a name="administer-your-storsimple-device-using-windows-powershell-for-storsimple"></a>Verwalten Sie das StorSimple-Gerät mithilfe von Windows PowerShell für StorSimple
Die folgende Tabelle zeigt eine Zusammenfassung aller häufige Verwaltungsaufgaben und Arbeitsabläufe erfolgen in Windows PowerShell-Schnittstelle des Geräts StorSimple. Weitere Informationen zu den einzelnen Workflows klicken Sie auf den entsprechenden Eintrag in der Tabelle.

#### <a name="windows-powershell-for-storsimple-workflows"></a>Windows PowerShell für StorSimple workflows

|Wenn Sie dies tun möchten...|Verwenden Sie dieses Verfahren.|
|---|---|
|Registrieren Sie Ihr Gerät|[Konfigurieren Sie und registrieren Sie das Gerät mithilfe von Windows PowerShell für StorSimple](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple) |
|Web-Proxy konfigurieren</br>Webproxyeinstellungen anzeigen|[Webproxy für Ihr StorSimple-Gerät konfigurieren](storsimple-configure-web-proxy.md)|
|Ändern Sie Daten 0 Netzwerk-Schnittstelle auf dem Gerät|[Ändern von Daten 0 Netzwerkschnittstelle für Ihr StorSimple-Gerät](storsimple-modify-data-0.md)|
|Beenden Sie einen Domänencontroller </br> Ein Controller Herunterfahren oder neu starten </br> Ein Herunterfahren</br>Das Gerät auf die werkseitigen Standardeinstellungen zurückgesetzt|[Gerätecontroller verwalten](storsimple-manage-device-controller.md)|
|Wartung-Modus-Updates und Hotfixes installieren|[Aktualisieren Sie Ihr Gerät](storsimple-update-device.md)|
|Wartungsmodus </br>Wartungsmodus beenden|[StorSimple gerätemodi](storsimple-device-modes.md)|
|Erstellen Sie ein Supportpaket</br>Entschlüsseln und Supportpaket bearbeiten|[Erstellen und Verwalten von Support-Paket](storsimple-create-manage-support-package.md)|
|Support-Sitzung starten</br>|[Starten Sie eine Sitzung in Windows PowerShell für StorSimple](/storsimple-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple)
 

## <a name="get-help-in-windows-powershell-for-storsimple"></a>Hilfe in Windows PowerShell für StorSimple

In Windows PowerShell für StorSimple ist Cmdlet Hilfe verfügbar. Eine online aktuelle Version der Hilfe ist auch verfügbar, können Sie die Hilfe auf Ihrem System aktualisieren.

Hilfe in dieser Schnittstelle ähnelt in Windows PowerShell, und die meisten Cmdlets hilfebezogene funktioniert. Finden Sie Hilfe für Windows PowerShell online in der TechNet-Bibliothek: [Scripting mit Windows PowerShell](http://go.microsoft.com/fwlink/?LinkID=108518).

Nachfolgend eine kurze Beschreibung der Typen der Hilfe für Windows PowerShell-Schnittstelle, einschließlich der Hilfe aktualisieren.

#### <a name="to-get-help-for-a-cmdlet"></a>Hilfe für ein cmdlet

- Hilfe für Cmdlet oder Funktion Befehl:`Get-Help <cmdlet-name>`

- Online-Hilfe für alle Cmdlets verwenden mit dem vorherige Cmdlet mit dem `-Online` Parameter:`Get-Help <cmdlet-name> -Online`

- Vollständige Hilfe können Sie die `–Full` Parameter und Beispiele, die `–Examples` Parameter.

#### <a name="to-update-help"></a>Hilfe zu aktualisieren

Sie können die Hilfe in der Windows PowerShell-Schnittstelle einfach aktualisieren. Die folgenden Schritte Hilfe auf Ihrem System aktualisieren.

#### <a name="to-update-cmdlet-help"></a>Cmdlet Hilfe aktualisieren

1. Starten Sie Windows PowerShell mit der Option **als Administrator ausführen** .

1. In der Befehlszeile eingeben: `Update-Help`

1. Die aktualisierten Hilfedateien installiert.

1. Geben Sie nach der Installation der Dateien: `Get-Help Get-Command`. Dies zeigt eine Liste der Cmdlets für die Hilfe verfügbar ist.


>[AZURE.NOTE] Um eine Liste aller verfügbaren Cmdlets in einen Runspace, melden Sie sich auf die entsprechende Menüoption, und führen Sie das `Get-Command` Cmdlet.

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie mit dem StorSimple-Gerät Probleme bei einer der oben genannten Workflows, finden Sie [Tools zur Bereitstellung von StorSimple](storsimple-troubleshoot-deployment.md#tools-for-troubleshooting-storsimple-deployments).

