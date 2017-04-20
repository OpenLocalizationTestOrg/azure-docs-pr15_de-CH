<properties 
   pageTitle="StorSimple Sicherheit | Microsoft Azure" 
   description="Beschreibt die Sicherheit und Datenschutz-Features, die die StorSimple Service Geräte und Daten lokal und in der Cloud zu schützen." 
   services="storsimple" 
   documentationCenter="NA" 
   authors="SharS" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="05/03/2016"
   ms.author="v-sharos"/>

# <a name="storsimple-security-and-data-protection"></a>StorSimple und Datenschutz

## <a name="overview"></a>Übersicht

Sicherheit spielt eine wichtige Rolle, wer eine neue Technologie einführt, vor allem bei Technologie mit vertrauliche Daten. Bewerten Technologien berücksichtigen Sie höhere Risiken und Kosten für den Datenschutz. Microsoft Azure StorSimple bietet eine Sicherheit und Datenschutz-Lösung für den Datenschutz zu: 

- **Vertraulichkeit** – nur autorisierte Entitäten Daten anzeigen können. 
- **Integrität** – nur autorisierte Entitäten ändern oder löschen können.

Microsoft Azure StorSimple-Lösung besteht aus vier Hauptkomponenten, die miteinander interagieren:

- **StorSimple Manager-Dienst in Microsoft Azure** – der Verwaltungsdienst konfigurieren und das StorSimple-Gerät verwenden.
- **StorSimple-Gerät** ein physisches Gerät im Rechenzentrum installiert. Alle Hosts und Clients, die Daten mit StorSimple-Gerät und das Gerät die Daten verwaltet und verschiebt sie in Azure Cloud entsprechend.
- **Kunden-Hosts mit dem Gerät verbunden** – die Clients in Ihrer Infrastruktur, die mit dem StorSimple-Gerät verbinden und Daten geschützt werden.
- **Cloud-Speicher** – die Position in der Azure-Cloud Daten.

Die folgenden Abschnitte beschreiben StorSimple-Sicherheitsfunktionen, die diese Komponenten und die darauf gespeicherten Daten schützen. Darüber hinaus steht eine Liste mit Fragen zu Microsoft Azure StorSimple Sicherheit und die entsprechenden Antworten.

## <a name="storsimple-manager-service-protection"></a>StorSimple Manager Service-Schutz

Der StorSimple-Manager ist ein Management-Dienst in Microsoft Azure gehostet und zur Verwaltung aller StorSimple Geräte, die Ihrer Organisation beschafft hat. Den Dienst StorSimple Manager möglich mithilfe Ihrer Organisation Anmeldeinformationen klassischen Azure-Portal über einen Webbrowser anmelden. 

Zugriff auf StorSimple-Manager-Dienst erfordert, dass die Organisation ein Azure-Abonnement, das StorSimple enthält. Ihr Abonnement steuert die Funktionen, die Sie im klassischen Azure-Portal zugreifen können. Wenn Ihre Organisation keinen Azure-Abonnement und mehr darüber erfahren möchten, finden Sie unter [für Azure Organisation anmelden](../active-directory/sign-up-organization.md). 

Da der StorSimple Manager-Dienst in Azure gehostet wird, ist es durch Azure Sicherheitsfeatures geschützt. Weitere Informationen über die Sicherheitsfeatures von Microsoft Azure finden Sie im [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/security/).

## <a name="storsimple-device-protection"></a>StorSimple Schutz

StorSimple-Gerät ist eine lokale Hybrid-Speichergerät, das solid-State-Laufwerken (SSDs) und Festplattenlaufwerken (HDDs) mit redundanten Controllern und automatischen Failover-Funktionen enthält. Der Controller verwaltet Storage tiering, platzieren derzeit verwendete (aktiven) Daten auf lokalem Speicher (in der StorSimple Gerät oder lokalen Servern), während weniger häufig verwendeten Daten in die Cloud verschieben.

Nur autorisierte Geräte dürfen sich in Azure-Abonnement erstellt den StorSimple Manager-Dienst StorSimple. Um ein Gerät zu autorisieren, müssen Sie es mit der StorSimple Manager-Dienst registrieren, mit Dienst-Registrierungsschlüssel. Dienst-Registrierungsschlüssel ist eine zufällige 128-Bit-Schlüssel im klassischen Azure-Portal generiert. 

![Dienst-Registrierungsschlüssel](./media/storsimple-security/ServiceRegistrationKey.png)

Lernen wie Abrufen einer Dienstregistrierungsschlüssel zu [Schritt2: Abrufen der Dienstregistrierungsschlüssel](storsimple-deployment-walkthrough.md#step-2-get-the-service-registration-key).

Service-Registrierungsschlüssel ist ein langer Schlüssel, der 100 Zeichen enthält. Sie können den Schlüssel kopieren und in einer Textdatei an einem sicheren Ort speichern, sodass Verwendung nach Bedarf zusätzliche Geräte autorisieren. Wenn der Dienstregistrierungsschlüssel verloren gehen nach dem ersten Gerät registrieren, erzeugen Sie einen neuen Schlüssel aus der StorSimple Manager-Dienst. Dies wirkt sich nicht auf vorhandene Geräte. 

Nachdem ein Gerät registriert wurde, wird für die Kommunikation mit Microsoft Azure Token. Der Dienst-Registrierungsschlüssel wird danach Gerät nicht verwendet.

> [AZURE.NOTE] Wir empfehlen, Service-Registrierungsschlüssel nach jeder Verwendung neu erstellt.

## <a name="protect-your-storsimple-solution-via-passwords"></a>Die StorSimple-Lösung über Kennwörter schützen

Kennwörter sind ein wichtiger Aspekt der Sicherheit und werden ausgiebig in StorSimple Lösung um sicherzustellen, dass Ihre Daten nur für autorisierte Benutzer zugänglich. StorSimple können Sie die folgenden Kennwörter konfigurieren:

- Administratorkennwort für StorSimple-Gerät
- Fordern Sie Initiator und Ziel Kennwörter Handshake Authentication Protocol (CHAP)
- StorSimple Snapshot-Manager-Kennwort

### <a name="windows-powershell-for-storsimple-and-the-storsimple-device-administrator-password"></a>Windows PowerShell für StorSimple und das Administratorkennwort StorSimple-Gerät

Windows PowerShell für StorSimple ist eine Befehlszeilenschnittstelle, die zum Verwalten des StorSimple verwenden können. Windows PowerShell für StorSimple hat Funktionen, die ermöglichen es Ihnen, registrieren Sie Ihr Gerät Konfigurieren der Netzwerkschnittstelle auf Ihrem Gerät installieren bestimmter Updates, Geräteproblem durch Zugriff auf die Sitzung und Ändern des Geräts. Sie können Windows PowerShell für StorSimple mit der seriellen Konsole auf dem Gerät oder mithilfe von Windows PowerShell-Remoting zugreifen.

PowerShell-Remoting über HTTPS oder HTTP möglich. Wenn remote-Management über HTTPS aktiviert ist, müssen Sie das remote-Management-Zertifikat vom Gerät herunterladen und installieren auf dem Remoteclient. Weitere Informationen zu PowerShell-Remoting finden Sie [Remote mit dem StorSimple-Gerät verbinden](storsimple-remote-connect.md).

Nachdem Sie Windows PowerShell für StorSimple Verbindung mit dem Gerät verwenden, müssen Sie das Gerät Administratorkennworts auf dem Gerät anmelden.

![Gerät-Administratorkennwort](./media/storsimple-security/DeviceAdminPW.png)

Beachten Sie die folgenden best Practices:

- Remoteverwaltung ist standardmäßig deaktiviert. Den StorSimple Manager-Dienst können um zu aktivieren. Als optimale Sicherheitsmethode RAS aktiviert werden soll, nur während der Zeit, die benötigt wird.
- Wenn Sie das Kennwort ändern, müssen Sie alle RAS-Benutzer benachrichtigen, damit sie keine unerwartete Konnektivität Datenverlust auftreten.
- Der StorSimple Manager-Dienst kann nicht abgerufen werden vorhandenen Kennwörter: er kann nur zurückgesetzt werden. Wir empfehlen, dass alle Kennwörter an einem sicheren Ort speichern, sodass Sie keinen Passwort vergessen wird. Wenn Sie ein Kennwort zurücksetzen müssen, müssen Sie alle Benutzer benachrichtigen, bevor Sie es zurücksetzen. 

Für den Zugriff auf die Windows PowerShell-Schnittstelle über eine serielle Verbindung mit dem Gerät. Sie können auch darauf zugreifen Remote mit HTTP oder HTTPS, die zusätzlichen Sicherheit. HTTPS bietet eine höhere Sicherheit als eine Serien- oder HTTP-Verbindung. Jedoch zur Verwendung von HTTPS installieren Sie ein Zertifikat auf dem Clientcomputer zuerst, die auf das Gerät zugreifen. Sie können RAS-Zertifikat auf der Konfigurationsseite Gerät in der StorSimple Manager-Dienst. Wenn das Zertifikat für den Remotezugriff verloren geht, müssen ein neues Zertifikat herunterladen und an alle Clients, die remote-Verwaltung verwenden weitergegeben.

### <a name="challenge-handshake-authentication-protocol-chap-initiator-and-target-passwords"></a>Fordern Sie Initiator und Ziel Kennwörter Handshake Authentication Protocol (CHAP)

CHAP ist ein Authentifizierungsschema, StorSimple-Gerät der Identität des Remoteclients. Die Prüfung basiert auf ein gemeinsames Kennwort. CHAP kann unidirektional sein (unidirektional) oder (bidirektional). Ziel (StorSimple-Gerät) authentifiziert mit unidirektionaler CHAP Initiator (Host). Gegenseitige oder umgekehrte CHAP erfordert, dass das Ziel authentifiziert den Initiator und der Initiator authentifiziert das Ziel. Die StorSimple kann für Methoden konfiguriert werden.

Achten Sie auf Folgendes, wenn Sie CHAP konfigurieren:

- Der CHAP-Benutzername muss weniger als 233 Zeichen enthalten.
- Das CHAP-Kennwort muss zwischen 12 und 16 Zeichen lang sein. Mehr Benutzernamen oder Kennwort führt zu einem Authentifizierungsfehler auf dem Windows-Host.
- Sie können nicht dasselbe Kennwort für die Initiator-CHAP und den CHAP-Ziel verwenden.
- Nach dem Festlegen des Kennworts kann geändert werden, jedoch können nicht abgerufen werden. Wenn das Kennwort geändert wird, müssen Sie alle RAS-Benutzer benachrichtigen, damit sie das StorSimple-Gerät herstellen können.

Weitere Informationen zu CHAP und Konfiguration für die StorSimple-Lösung dazu [Geräts StorSimple CHAP konfigurieren](storsimple-configure-chap.md).

### <a name="storsimple-snapshot-manager-password"></a>StorSimple Snapshot-Manager-Kennwort

StorSimple Snapshot-Manager ist ein Snap-in der Microsoft Management Console (MMC), das Volume-Gruppen und den Windows Volumeschattenkopie-Dienst anwendungskonsistente Backups erstellen. Darüber hinaus können Sie StorSimple Snapshot-Manager Sicherungszeitpläne und Klon erstellen oder Volumes wiederherstellen.

Wenn Sie ein Gerät StorSimple Snapshot-Manager konfigurieren, müssen Sie StorSimple Snapshot-Manager-Kennwort eingeben. Dieses Kennwort wird zuerst in Windows PowerShell für StorSimple während der Registrierung festgelegt. Das Kennwort kann auch festgelegt und von der StorSimple Manager-Dienst geändert. Dieses Kennwort authentifiziert das Gerät mit StorSimple Snapshot-Manager.

![StorSimple Snapshot-Manager-Kennwort](./media/storsimple-security/SnapshotMgrPassword.png)

StorSimple Snapshot-Manager-Kennwort 14 bis 15 Zeichen sein und muss mindestens 3 aus Großbuchstaben, Kleinbuchstaben, numerischen und Sonderzeichen enthalten. Nach dem Festlegen des StorSimple Snapshot-Manager-Kennworts kann geändert werden, jedoch können nicht abgerufen werden. Wenn Sie das Kennwort ändern, müssen Sie alle entfernten Benutzer benachrichtigen.

Weitere Informationen über StorSimple Snapshot-Manager, [Was StorSimple Snapshot-Manager ist?](storsimple-what-is-snapshot-manager.md)

### <a name="password-best-practices"></a>Best Practices für Kennwörter

Es wird empfohlen, die folgenden Richtlinien verwenden, um sicherzustellen, dass StorSimple starke und gut geschützten Kennwörtern:

- Ändern Sie Ihre Kennwörter alle drei Monate. Ändern der Kennwörter wird jährlich erzwungen.
- Verwenden Sie sichere Kennwörter. Weitere Informationen zur [Erstellen sicherer Kennwörter und schützen](http://blogs.microsoft.com/cybertrust/2014/08/25/create-stronger-passwords-and-protect-them/).
- Verwenden Sie immer unterschiedliche Kennwörter für verschiedene Zugriffsmechanismen; Jedes angegebene Kennwort muss eindeutig sein.
- Teilen Sie Kennwörter nicht mit wer nicht StorSimple Gerät zugreifen darf.
- Sprechen Sie über ein Kennwort vor oder Hinweis auf das Format eines Kennwortes nicht.
- Wenn Sie vermuten, dass ein Konto oder Kennwort gefährdet ist, melden Sie den Vorfall Ihrer sicherheitsabteilung.
- Alle Kennwörter als sensible und vertrauliche Informationen behandelt. 

## <a name="storsimple-data-protection"></a>StorSimple Datenschutz

Dieser Abschnitt beschreibt die StorSimple Sicherheitsfunktionen, die Daten bei der Übertragung und Daten zu schützen.

Wie in anderen Abschnitten beschrieben, sind Kennwörter zum Autorisieren und Benutzer authentifizieren, bevor sie die Projektmappe StorSimple zugreifen können. Ein weiterer Sicherheitsaspekt schützt Daten vor nicht autorisierten Benutzern während der Übertragung ist zwischen Speichersystemen und Speicherung. Die folgenden Abschnitten enthaltenen StorSimple Datenschutzfunktionen.

> [AZURE.NOTE] Deduplizierung bietet zusätzlichen Schutz für Daten auf dem Gerät StorSimple und Microsoft Azure-Speicher. Wenn Daten dedupliziert Datenobjekte werden separat gespeichert von Metadaten und darauf zugreifen: Es gibt keinen verfügbar auf Speicherebene Kontext die Daten basierend auf Volumestruktur, Dateisystem oder Dateiname.

## <a name="protect-data-flowing-through-the-service"></a>Datenschutz durch den Dienst

Der Hauptzweck der StorSimple Manager-Dienst ist zum Verwalten und konfigurieren Sie das Gerät StorSimple. Der StorSimple Manager-Dienst in Microsoft Azure ausgeführt wird. Verwenden der Azure-Verwaltungsportal Konfigurationsdaten eingeben und anschließend Microsoft Azure StorSimple Manager-Dienst verwendet, um die Daten an das Gerät senden. StorSimple verwendet eine System von asymmetrischen Schlüsselpaaren um sicherzustellen, dass eine Gefährdung von Azure Service nicht Verlust von Informationen führt. 

![Verschlüsselung im Flug](./media/storsimple-security/DataEncryption.png)

Asymmetrische Schlüssel System schützt die Daten, die über den Dienst wie folgt verläuft:

1. Ein Verschlüsselungszertifikat Daten verwendet, die einen asymmetrischen öffentlichen und privaten Schlüssel auf dem Gerät generiert und wird verwendet, um die Daten zu schützen. Die Schlüssel werden generiert, wenn das erste Gerät registriert ist. 
2. Zertifikat Verschlüsselungsschlüssel werden in einer privaten Informationsaustausch (PFX) Datei exportiert, die den Verschlüsselungsschlüssel Dienst Daten geschützt ist eine starke 128-Bit-Schlüssel, der durch das erste Gerät während der Registrierung nach dem Zufallsprinzip generiert wird.
3. Der öffentliche Schlüssel des Zertifikats ist sicher der StorSimple Manager-Dienst zur Verfügung gestellt, und der private Schlüssel verbleibt das Gerät.
4. Daten in den Dienst mit öffentlichen Schlüssel und entschlüsselt mit dem privaten Schlüssel auf dem Gerät gespeicherten damit Azure Service die Daten auf dem Gerät entschlüsseln kann verschlüsselt.

Nur das erste Gerät Dienst registriert wird der Verschlüsselungsschlüssel Daten generiert. Alle nachfolgenden Geräte mit dem Dienst registriert müssen den gleichen Daten Verschlüsselungsschlüssel verwenden. 

> [AZURE.IMPORTANT] 
> 
> Es ist sehr wichtig, den Verschlüsselungsschlüssel Daten kopieren und an einem sicheren Ort speichern. Eine Kopie der Daten Verschlüsselungsschlüssel sollten so gespeichert werden, autorisierte Personen zugänglich und leicht an den Geräteadministrator mitgeteilt werden.
>
> Wenn der Verschlüsselungsschlüssel Daten verloren gehen, können einen Microsoft-Supportmitarbeiter Sie vorausgesetzt, dass Sie mindestens ein Gerät in einem Online-Status abrufen. Wir empfehlen den Verschlüsselungsschlüssel Daten ändern, nachdem sie abgerufen werden. Informationen finden Sie unter [Ändern der Verschlüsselungsschlüssel Daten](storsimple-service-dashboard.md#change-the-service-data-encryption-key).

Die Option **Service Daten Chiffrierschlüssel ändern** auf dem Service-Dashboard können Sie den Verschlüsselungsschlüssel Daten und die entsprechenden Daten Verschlüsselungszertifikat ändern. Um sicherzustellen, dass Sicherheit nicht gefährdet ist, verwenden Sie ein physisches Gerät StorSimple den Verschlüsselungsschlüssel Daten ändern. Ändern die Verschlüsselungsschlüssel erfordert, dass alle Geräte mit dem neuen Schlüssel aktualisiert werden. Daher wird empfohlen, dass Sie den Schlüssel ändern, wenn alle Geräte online sind. Wenn Geräte offline sind, können der Schlüssel zu einem anderen Zeitpunkt geändert werden. Geräte mit veralteten Schlüssel werden weiterhin Backups ausgeführt, aber sie werden nicht wiederherstellen, bis der Schlüssel aktualisiert. Weitere Informationen finden Sie [StorSimple Manager Service Dashboard verwenden](storsimple-service-dashboard.md).

Der Verschlüsselungsschlüssel für Daten und das Verschlüsselungszertifikat Daten laufen nicht ab. Wir empfehlen jedoch ändern den Verschlüsselungsschlüssel Daten jährlich um Schlüsselkompromiss zu verhindern.

## <a name="protect-data-at-rest"></a>Daten schützen

StorSimple-Gerät verwaltet Daten lokal und in der Cloud, je nach Häufigkeit der Benutzung in Ebenen speichern. Alle an den Computer angeschlossenen Hostcomputer senden an Gerät sich dann Daten in die Cloud gegebenenfalls. Daten werden vom Gerät in die Cloud sicher über das Internet übertragen. Jedes Gerät hat ein iSCSI-Ziel, der alle freigegebenen Volumes auf dem Gerät bereitstellt. Alle Daten werden verschlüsselt, bevor sie cloud-Speicher gesendet werden. 

![Verschlüsselungsschlüssel für Cloud-Speicher](./media/storsimple-security/CloudStorageEncryption.png)

Um die Sicherheit und Integrität der Daten in der Cloud zu gewährleisten, können mit StorSimple Cloud Storage Schlüssel wie folgt definieren:

- Cloud Storage-Verschlüsselungsschlüssel wird beim Erstellen eines Volume-Containers angeben. Der Schlüssel kann nicht geändert oder später hinzugefügt. 
- Alle Volumes in einer volumecontainer teilen den gleichen Verschlüsselungsschlüssel. Soll eine andere Form der Verschlüsselung für ein bestimmtes Volume wird empfohlen, einen neuen Datenträger Container zum Hosten dieses Volumes erstellen.
- Wenn Sie Cloud Storage-Verschlüsselungsschlüssel in der StorSimple Manager-Dienst eingeben, wird der Schlüssel verschlüsselt, den öffentlichen Teil der Verschlüsselungsschlüssel Daten verwenden und dann an das Gerät gesendet.
- Cloud Storage-Verschlüsselungsschlüssel nicht überall im Dienst gespeichert und gilt nur für das Gerät.
- Angeben eines Schlüssels Cloud Speicher ist optional. Sie können Daten senden, die auf dem Host an das Gerät verschlüsselt wurde.

### <a name="additional-security-best-practices"></a>Bewährte Sicherheit

- Aufteilung der Datenströme: Isolieren der iSCSI-SAN von Benutzerdatenverkehr in einem Unternehmens-LAN vollständig getrennten Netzwerk bereitstellen und Verwenden von VLANs ist physische Isolation nicht optional. Dediziertes Netzwerk für iSCSI-Speicher garantiert die Sicherheit und Leistung Ihrer unternehmenskritischen Daten. Mischen von Speicherplatz und Datenverkehr über ein Firmennetzwerk wird nicht empfohlen, Latenz und Netzwerk auftreten.

- Verwenden Sie hostseitige Netzwerksicherheit Netzwerkschnittstellen, die TCP/IP Offload Engine (TOE) unterstützen. PO verringert CPU-Auslastung durch die Verarbeitung von TCP auf dem Netzwerkadapter.

## <a name="protect-data-via-storage-accounts"></a>Schützen von Daten über Speicher-Konten

Jedes Microsoft Azure-Abonnement kann ein oder mehrere Storage-Konten erstellen. (Ein Speicherkonto bietet einen eindeutigen Namespace für die Arbeit mit Daten in Azure Cloud.) Die Abonnements und Access Schlüssel mit diesem Storage-Konto Zugriff auf ein Speicherkonto gesteuert. 

Wenn Sie ein Speicherkonto erstellen, generiert Microsoft Azure zwei 512-Bit-Speicher-Zugriffstasten, die davon für die Authentifizierung verwendet wird, wenn das StorSimple-Gerät das Speicherkonto zugreift. Beachten Sie, dass nur eines dieser Schlüssel verwendet. Der Schlüssel findet zu reservieren, können Sie die Schlüssel regelmäßig rotieren. Rotieren Sie Schlüssel aktivieren Sie den sekundären Schlüssel und löschen Sie den Primärschlüssel. Sie können dann einen neuen Schlüssel für die Verwendung während der nächsten Rotation erstellen. (Aus Gründen der Sicherheit müssen viele Rechenzentren Schlüsselrotation.) 

Wir empfehlen, diese best Practices zur Schlüsselrotation folgen:

- Drehen Sie Konto Speicherschlüssel regelmäßig, um sicherzustellen, dass das Speicherkonto nicht von unbefugten Benutzern zugegriffen werden kann.
- In regelmäßigen Abständen Systemadministrator Azure ändern oder Regenerieren den primären oder sekundären Schlüssel mit den Lagerbereich klassischen Azure-Portal das Speicherkonto direkt auf.


## <a name="protect-data-via-encryption"></a>Datenschutz durch Verschlüsselung

StorSimple verwendet die folgenden Verschlüsselungsalgorithmen zum Schutz von Daten in oder zwischen den Komponenten der Lösung StorSimple.

| Algorithmus | Schlüssellänge | Protokolle/Applications/Kommentare |
| --------- | ---------- | ------------------------------- |
| RSA       | 2048       | RSA PKCS 1 v1. 5 von klassischen Azure-Portal verwendet, um Konfigurationsdaten zu verschlüsseln, die an das Gerät gesendet werden: z. B. Storage Konto Anmeldeinformationen StorSimple Gerätekonfiguration und cloud-Speicher Verschlüsselungsschlüssel. |
| AES       | 256        | AES mit CBC wird verwendet, um den öffentlichen Teil der Verschlüsselungsschlüssel Daten verschlüsseln, bevor sie den Azure-Verwaltungsportal von StorSimple-Gerät gesendet werden. StorSimple-Gerät dient zum Verschlüsseln von Daten vor dem Senden der Daten an das Speicherkonto Cloud. |


## <a name="storsimple-virtual-device-security"></a>StorSimple virtuelles gerätesicherheit

[AZURE.INCLUDE [storsimple virtual device security](../../includes/storsimple-virtual-device-security.md)]

## <a name="frequently-asked-questions-faq"></a>Häufig gestellte Fragen (FAQ)

Es folgen einige Fragen und Antworten zur Sicherheit und Microsoft Azure StorSimple.

**Q:** Mein Service ist gefährdet. Was sollte meine nächsten Schritte?

**A:** Ändern Sie sofort den Verschlüsselungsschlüssel Daten und Speicher kontoschlüssel für das Speicherkonto für tiering Daten verwendet wird. Eine Anleitung hierzu finden Sie unter 

- [Ändern Sie den Verschlüsselungsschlüssel für Daten](storsimple-service-dashboard.md#change-the-service-data-encryption-key)
- [Schlüsselrotation Speicherkonten](storsimple-manage-storage-accounts.md#key-rotation-of-storage-accounts)

**Q:** Ich habe ein neues StorSimple-Gerät, das Registrierungsschlüssel Service angefordert werden. Wie rufe ich es?

**A:** Dieser Schlüssel wurde bei der Erstellung des StorSimple Manager-Dienstes. Wenn Sie Verbindung mit dem Gerät StorSimple Manager-Dienst verwenden, können Sie Service quick-Start-Seite anzeigen oder Regenerieren den Service-Registrierungsschlüssel. Generiert einen neuen Schlüssel Service wirkt sich nicht auf vorhandene registrierte Geräte aus. Eine Anleitung hierzu finden Sie unter

- [Zeigen Sie an oder Regenerieren Sie der Dienstregistrierungsschlüssel](storsimple-service-dashboard.md#view-or-regenerate-the-service-registration-key)

**Q:** Mein Dienst Daten Verschlüsselungsschlüssel Verlust. Was soll ich tun?

**A:** Wenden Sie sich an Microsoft Support Services. Sie können mit einer supportsitzung auf dem Gerät und Hilfe den Schlüssel abrufen, (vorausgesetzt, mindestens ein Gerät online) anmelden. Sofort nach den Verschlüsselungsschlüssel Daten Erhalt sollten Sie ändern, um sicherzustellen, dass der neue Schlüssel nur Ihnen bekannt ist. Eine Anleitung hierzu finden Sie unter

- [Ändern Sie den Verschlüsselungsschlüssel für Daten](storsimple-service-dashboard.md#change-the-service-data-encryption-key)

**Q:**  Ich authorized Service Data Encryption Key sich ein Gerät, aber die Änderung nicht starten. Was soll ich tun?

**A:** Wenn das Timeout abgelaufen ist, müssen Sie erneut Autorisieren Gerät für Service Data Encryption Key ändern und erneut starten.

**Q:**  Den Verschlüsselungsschlüssel Daten geändert, aber ich war nicht die Geräte innerhalb von 4 Stunden aktualisieren. Muss erneut gestartet werden?

**A:** Die 4-Stunden-Periode ist nur für die Änderung initiiert. Nach dem Starten des Aktualisierungsvorgangs auf autorisierte StorSimple-Gerät ist der Autorisierung gültig, bis alle Geräte aktualisiert werden.

**Q:** Unsere StorSimple-Administrator hat das Unternehmen verlassen. Was soll ich tun?

**A:** Ändern und Zurücksetzen von Kennwörtern, die Zugriff auf das Gerät StorSimple, und den Verschlüsselungsschlüssel Daten um sicherzustellen, dass die neue Informationen nicht autorisiertes Personal nicht bekannt ist. Eine Anleitung hierzu finden Sie unter

- [Verwenden des StorSimple Manager-Dienstes Storsimple Kennwörter ändern](storsimple-change-passwords.md)
- [Ändern Sie den Verschlüsselungsschlüssel für Daten](storsimple-service-dashboard.md#change-the-service-data-encryption-key)
- [Konfigurieren von CHAP für Ihr StorSimple-Gerät](storsimple-configure-chap.md)

**Q:** Ich möchte das Kennwort StorSimple Snapshot-Manager an einem Host, die mit dem StorSimple-Gerät verbinden, aber das Kennwort ist nicht verfügbar. Was kann ich tun?

**A:** Wenn Sie das Kennwort vergessen haben, sollten Sie eine neue erstellen. Anschließend müssen Sie alle vorhandenen Benutzer informieren, dass das Kennwort geändert wurde und die Clients für das neue Kennwort aktualisiert werden soll. Eine Anleitung hierzu finden Sie unter

- [StorSimple Snapshot-Manager-Kennwort ändern](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password)
- [Ein Gerät authentifizieren](storsimple-snapshot-manager-manage-devices.md#authenticate-a-device)

**Q:** Das Zertifikat für den Remotezugriff auf die Windows PowerShell für StorSimple wurde auf dem Gerät geändert. Wie aktualisiere ich meine RAS-Clients?

**A:** Sie können der StorSimple Manager-Dienst das neue Zertifikat herunterladen und geben sie im Zertifikatspeicher RAS-Clients installiert werden. Eine Anleitung hierzu finden Sie unter

- [Zertifikat importieren cmdlet](https://technet.microsoft.com/library/hh848630.aspx)

**Q:** Ist Meine geschützten Daten, wenn StorSimple Manager Service gefährdet ist?

**A:** Dienstkonfigurationsdaten werden mit Ihrem öffentlichen Schlüssel verschlüsselt, wenn Sie in einem Webbrowser anzeigen. Da der Dienst Zugriff auf den privaten Schlüssel besitzt, wird der Dienst nicht keine Daten sehen können. Wenn der StorSimple Manager-Dienst gefährdet ist, gibt keine Auswirkung keine Tasten in der StorSimple Manager-Dienst gespeichert.

**Q:** Ein Benutzer erhält Zugriff auf das Verschlüsselungszertifikat Daten werden Daten gefährdet?

**A:** Microsoft Azure werden die Kunden Datenschlüssel (PFX-Datei) in einem verschlüsselten Format gespeichert. Da die PFX-Datei verschlüsselt und der StorSimple-Dienst nicht den Verschlüsselungsschlüssel Daten zum Entschlüsseln der PFX-Datei, wird einfach immer Zugriff auf die PFX-Datei nicht vertrauliche Daten verfügbar machen.

**Q:** Was geschieht, wenn ein Microsoft Daten anfordert?

**A:** Da alle Daten verschlüsselt den Dienst und der private Schlüssel mit dem Gerät werden, bitte staatlichen Stelle der Kunde für die Daten. 

## <a name="next-steps"></a>Nächste Schritte

[Das StorSimple-Gerät bereitstellen](storsimple-deployment-walkthrough.md).
 
