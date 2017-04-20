
<properties 
    pageTitle="Wie wird Azure RemoteApp Einstellungen und Benutzerdaten gespeichert? | Microsoft Azure"
    description="Erfahren Sie, wie Azure RemoteApp Benutzerdaten mit der Benutzer-Profil gespeichert."
    services="remoteapp"
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />

# <a name="how-does-azure-remoteapp-save-user-data-and-settings"></a>Wie wird Azure RemoteApp Einstellungen und Benutzerdaten gespeichert?

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Azure RemoteApp speichert Benutzeridentität und Anpassungen Geräte-und Sessions. Dieser Daten werden in einem pro Benutzer pro Auflistung, bekannt als Benutzer Profil Datenträger (UPD) gespeichert. Der Datenträger wird für diesen Benutzer und stellt sicher, dass der Benutzer ein konsistentes, unabhängig davon, wo sie sich anmelden. speichert 

Benutzer Profil Festplatten sind für den Benutzer transparent – Benutzer Dokumente in ihren Ordner speichern (auf einem lokalen Laufwerk) und ändern ihre app wie gewohnt. Zur gleichen Zeit bestehen alle persönlichen beim Verbinden von Geräten mit Azure RemoteApp. Der Benutzer sieht lediglich ihre Daten am selben Ort.

Jede UPD hat 50GB Speicherung und enthält sowohl Benutzer und. 

Lesen Sie Einzelheiten auf Benutzerprofildaten.

>[AZURE.NOTE] Die UPD deaktivieren möchten? -Überprüfung Pavithras Blogbeitrag, [Deaktivieren Benutzer Profil Festplatten (Benutzerprofil-Datenträger) in Azure RemoteApp](https://blogs.technet.microsoft.com/enterprisemobility/2015/11/11/disable-user-profile-disks-upds-in-azure-remoteapp/)Informationen möglich.


## <a name="how-can-an-admin-get-to-the-data"></a>Wie kann ein Administrator die Daten?

Benötigen Sie Zugriff auf die Daten für einen Benutzer (für Disaster Recovery oder wenn der Benutzer das Unternehmen verlässt), Azure wenden und Abonnements für die Sammlung und die Identität des Benutzers übermitteln. Azure RemoteApp-Team wird eine URL auf der virtuellen Festplatte angeben. Dieser VHD herunterladen und alle Dokumente oder Dateien, die Sie abrufen. Beachten Sie, dass die virtuelle Festplatte dauert etwas herunterladen 50 GB ist.


## <a name="is-the-data-backed-up"></a>Werden die Daten gesichert?

Ja, speichern Sie eine Sicherungskopie der Benutzerdaten pro Standort. Die Daten schreibgeschützt und erfolgt auf die gleiche Weise wie die regulären Daten (Kontakt Azure RemoteApp zu), wenn das primäre Rechenzentrum ausgefallen ist. Daten in Echtzeit in kopiert den Speicherort und wir Kopien verschiedener Versionen nicht beibehalten. Also auf beschädigte Daten, wir werden nicht auf eine zuvor funktionierende Version wiederherstellen, aber wenn das primäre Rechenzentrum ausgefallen ist, werden Daten aus den anderen Ort zu.

## <a name="how-do-users-see-the-upd-on-the-server-side"></a>Wie sehen Benutzer die UPD auf dem Server?

Jeder Benutzer muss eigenes Verzeichnis auf dem Server, der ihre UPD zugeordnet ist: c:\Users\username.

## <a name="whats-the-best-way-to-use-outlook-and-upd"></a>Was ist am besten Outlook und UPD verwenden?

Azure RemoteApp speichert Outlook Zustand (Postfächer, PST-Dateien) zwischen. Dazu benötigen wir die PST-Datei in die Benutzerprofildaten gespeichert werden (c:\users\<Benutzername >). Dies ist der Standardspeicherort für die Daten, so lange Sie nicht den Speicherort ändern, Daten zwischen Sitzungen beibehalten werden.

Wir empfehlen auch "Cache"-Modus in Outlook verwenden und "Server/online" Modus für die Suche verwenden.

[In diesem Artikel](remoteapp-outlook.md) Weitere Informationen zur Verwendung von Outlook und Azure RemoteApp anzeigen

## <a name="what-about-redirection"></a>Was Umleitung?
Azure RemoteApp um Benutzern lokale Geräte zugreifen [Umleitung](remoteapp-redirection.md)einrichten können. Lokale Geräte werden dann auf die Daten auf den UPD.

## <a name="can-i-use-my-upd-as-a-network-share"></a>Kann ich meine UPD als Netzwerkfreigabe verwenden?
Nein. Benutzerprofil-Datenträger können nicht als Netzwerkfreigabe verwendet werden. Ein UPD ist nur für den Benutzer verfügbar, wenn der Benutzer aktiv Azure RemoteApp verbunden ist.

## <a name="if-i-delete-a-user-from-a-collection-is-their-upd-deleted"></a>Löschen eines Benutzers aus einer Auflistung wird ihre UPD gelöscht?

Nein, wenn Sie einen Benutzer löschen, wir die UPD nicht automatisch gelöscht stattdessen Daten speichern wir die Auflistung löschen. 90 Tage nach dem Löschen der Auflistung löschen wir alle Benutzerprofil-Datenträger. 

Benötigen Sie eine UPD aus einer Auflistung löschen, Azure RemoteApp - Kontaktinformationen können wir von unserer Seite UPD löschen.

## <a name="can-i-access-my-users-upds-either-current-or-deleted-users"></a>Kann meine Benutzer Benutzerprofil-Datenträger (aktuelle oder gelöschten Benutzer) zugreifen?

Ja, wenn Sie [Azure RemoteApp](mailto:remoteappforum@microsoft.com)wenden, können wir Sie mit einer URL für den Datenzugriff einrichten. 10 Stunden müssen Daten oder Dateien vor der Zugriff aus den UPD herunterladen.

## <a name="are-upds-available-offline"></a>Gibt Benutzerprofil-Datenträger offline?

Jetzt bieten wir keine offline-Zugriff auf Benutzerprofil-Datenträger, über 10 Stunden Access-Fenster oben beschrieben. Dies bedeutet, dass wir derzeit zu lang Zugriff bieten keinen genug, um komplexere Aufgaben wie Antiviren-Software auf dem Benutzerprofil-Datenträger oder Datenzugriff für eine Überprüfung durchführen.

## <a name="do-registry-key-settings-persist"></a>Werden Registrierungsschlüssel beibehalten?
Ja, HKEY_Current_User geschrieben ist Teil der UPD.

## <a name="can-i-disable-upds-for-a-collection"></a>Kann ich für eine Auflistung Benutzerprofil-Datenträger deaktivieren?

Ja, kannst du Azure RemoteApp Benutzerprofil-Datenträger für ein Abonnement deaktivieren, aber nicht selbst. Dies bedeutet, dass Benutzerprofil-Datenträger für alle Sammlungen im Abonnement deaktiviert.

Sie möchten Benutzerprofil-Datenträger in folgenden Situationen deaktiviert: 

- Ausführen müssen Zugriff und Steuerung von Benutzerdaten (für überwachen und Analysieren der Zwecke wie Finanzinstitute).
- Sie haben Benutzer 3rd Party Management Solutions lokalen Profil und weiterhin in der Domäne Azure RemoteApp-Bereitstellung verwenden möchten. Dies erfordert Profil Agent in gold Bild geladen werden. 
- Sie brauchen keine lokalen Datenspeicher oder enthalten alle Daten in der Cloud oder die Dateifreigabe und steuern möchten Daten lokal mit Azure RemoteApp speichern.

Weitere Informationen finden Sie unter [Deaktivieren Benutzer Profil Festplatten (Benutzerprofil-Datenträger) in Azure RemoteApp](https://blogs.technet.microsoft.com/enterprisemobility/2015/11/11/disable-user-profile-disks-upds-in-azure-remoteapp/) .

## <a name="can-i-restrict-users-from-saving-data-to-the-system-drive"></a>Kann Benutzer werden eingeschränkt, Daten auf dem Systemlaufwerk speichern?

Ja, aber Sie müssen, bevor die Sammlung in das Vorlagenbild eingerichtet. Gehen Sie um auf dem Systemlaufwerk zu blockieren:

1. Führen Sie **gpedit.msc** auf dem.
2. Navigieren Sie zu **Benutzerkonfiguration > Administrative Vorlagen > Windows-Komponenten > Explorer**.
3. Optionen:
    - **Diese angegebenen Datenträger im Fenster Arbeitsplatz ausblenden**
    - **Verhindern Sie den Zugriff auf Laufwerke vom Arbeitsplatz**

## <a name="can-i-seed-upds-i-want-to-put-some-data-in-the-upd-thats-available-the-first-time-the-user-signs-in"></a>Kann Benutzerprofil-Datenträger Samen? Ich möchte einige Daten in die UPD, die verfügbaren erstmals der Benutzer.

Ja, wenn Vorlage erstellen, können Sie Informationen Profil hinzufügen. Diese Informationen wird die UPD hinzugefügt.

## <a name="can-i-change-the-size-of-the-upd-depending-on-how-much-data-i-want-to-store"></a>Kann die Größe der UPD je nach Datenmenge ändern zu speichern?

Nein, alle Benutzerprofil-Datenträger 50 GB Speicherkapazität. Wenn Sie unterschiedliche Datenmengen speichern möchten, versuchen Sie Folgendes:

1. Deaktivieren Sie Benutzerprofil-Datenträger für die Auflistung.
2. Richten Sie eine Dateifreigabe für Benutzer zugreifen.
3. Laden der Dateifreigabe ein Startskript. Details für Startskripte in Azure RemoteApp finden Sie unter.
4. Benutzer alle Daten in die Dateifreigabe speichern.


## <a name="how-do-i-run-a-startup-script-in-azure-remoteapp"></a>Wie führe ich ein Startskript in Azure RemoteApp?

Wenn Sie Startskripts ausführen möchten, erstellen Sie zunächst einen geplanten Task in das Vorlagenbild für die Auflistung verwendet werden. (Führen Sie *vor dem* Ausführen von Sysprep.) 

![Erstellen Sie eine](./media/remoteapp-upd/upd1.png)

![Erstellen Sie eine Systemaufgabe, die ausgeführt wird, wenn ein Benutzer anmeldet](./media/remoteapp-upd/upd2.png)

Werden Sie auf der Registerkarte **Allgemein** sicher, dass das **Benutzerkonto** unter Sicherheit "BUILTIN\Users".

![Ändern Sie das Benutzerkonto einer Gruppe](./media/remoteapp-upd/upd4.png)

Der Task wird Ihre Startskript mit den Benutzeranmeldeinformationen gestartet. Planen Sie den Task Ausführen jedes Mal ein Benutzer anmeldet.

![Legen Sie den Trigger für die Aufgabe "bei Anmeldung"](./media/remoteapp-upd/upd3.png)

Sie können auch [Gruppenrichtlinien-basierte Startskripts](https://technet.microsoft.com/library/cc779329%28v=ws.10%29.aspx). 

## <a name="what-about-placing-a-startup-script-in-the-start-menu-would-that-work"></a>Platzieren Sie ein Startskript was im Startmenü? Funktioniert das?

Also kann ich erstellen eine bat-Datei, die eine Konfiguration Fenster Skript ausführt c:\ProgramData\Microsoft\Windows\Start lautet Ordner speichern und müssen das Skript ausführen, wenn ein Benutzer eine Sitzung RemoteApp startet?

Leider wird, nicht mit Azure RemoteApp unterstützt auch Startskripts im Startmenü nicht unterstützt RDSH, verwendet.

## <a name="can-i-use-mstscexe-the-remote-desktop-program-to-configure-logon-scripts"></a>Können ich mstsc.exe (das Programm Remotedesktop) Anmeldeskripts konfigurieren?

Nein, unterstützt von Azure RemoteApp.

## <a name="can-i-store-data-on-the-vm-locally"></a>Können Daten auf dem virtuellen Computer lokal werden gespeichert?

Nein, überall auf dem virtuellen Computer nicht in den UPD gespeicherte Daten verloren. Besteht eine hohe Wahrscheinlichkeit der Benutzer erhalten der gleichen VM das nächste Mal sie Azure RemoteApp anmelden. Wir behaupten nicht Benutzer VM Dauerhaftigkeit, damit der Benutzer nicht die gleichen virtuellen Computer anmelden und die Daten gehen verloren. Wenn wir die Auflistung aktualisieren, werden vorhandene virtuelle Computer außerdem mit neuen VMs - ersetzt, die bedeutet, dass auf dem virtuellen Computer selbst gespeicherten Daten verloren gehen. UPD freigegebenen Speicher wie Azure Dateien einen Dateiserver innerhalb einer VNET oder in der Cloud ein Cloud-Speichersystem wie DropBox Datenspeicher wird empfohlen.

## <a name="how-do-i-mount-an-azure-file-share-on-a-vm-using-powershell"></a>Wie können eine Azure-Dateifreigabe auf einer VM mit PowerShell werden bereitgestellt?

Net-PSDrive-Cmdlet können Sie das Laufwerk wie folgt geladen:

    New-PSDrive -Name <drive-name> -PSProvider FileSystem -Root \\<storage-account-name>.file.core.windows.net\<share-name> -Credential :<storage-account-name>


Sie können auch Ihre Anmeldeinformationen speichern, ausgeführt werden:

    cmdkey /add:<storage-account-name>.file.core.windows.net /user:<storage-account-name> /pass:<storage-account-key>


Können Sie überspringen den - Credential-Parameter im neuen PSDrive-Cmdlet.
