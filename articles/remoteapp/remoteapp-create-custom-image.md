<properties
    pageTitle="Erstellen Sie eine benutzerdefinierte Vorlagenbild für Azure RemoteApp | Microsoft Azure"
    description="Informationen Sie zum Erstellen einer benutzerdefinierten Vorlagenbild für Azure RemoteApp. Mit dieser Vorlage können mit Hybrid oder Cloud."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="how-to-create-a-custom-template-image-for-azure-remoteapp"></a>Erstellen Sie eine benutzerdefinierte Vorlagenbild für Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Azure RemoteApp verwendet ein Windows Server 2012 R2 Vorlagenbild hosten alle Programme, die Sie für die Benutzer freigeben möchten. Erstellen Sie ein benutzerdefiniertes Bild der RemoteApp-Vorlage kann mit einem vorhandenen Abbild starten oder eine neue erstellen. 


> [AZURE.TIP] Wussten Sie, dass ein Bild aus einer Azure VM erstellen können? Wahre Geschichte und reduziert die Zeitspanne es notwendig ist, um das Bild zu importieren. Sehen Sie sich die Schritte [hier](remoteapp-image-on-azurevm.md).

Sind die Vorschriften für das Bild, das für die Verwendung mit Azure RemoteApp hochgeladen werden können:


- Die Größe des Bildes sollte ein Vielfaches von MBs. Wenn Sie versuchen, ein Bild, das kein Vielfaches ist, schlägt der Upload fehl.
- Die Bildgröße muss 127 GB oder kleiner.
- Muss auf einer VHD-Datei (VHDX Dateien [Hyper-V virtuelle Festplatten] werden derzeit nicht unterstützt).
- Die virtuelle Festplatte darf einem Generation 2 virtuelle Computer.
- Die virtuelle Festplatte kann fester Größe oder Dyn. Eine dynamisch erweiterbare virtuelle Festplatte wird empfohlen, da weniger Zeit als ein fester Größe VHD-Datei in Azure hochladen.
- Der Datenträger muss mit der Master Boot Record (MBR) Partitionstyp initialisiert werden. Partitionsstil der GUID-Partitionstabelle (GPT) wird nicht unterstützt.
- Die virtuelle Festplatte muss eine einzige Installation von Windows Server 2012 R2 enthalten. Sie können mehrere Volumes, aber nur ein, die einer Installation von Windows enthalten.
- Die Funktion Remote Desktop Session Host (RDSH) und die Desktop Experience-Funktion müssen installiert sein.
- Die Remotedesktop-Verbindungsbroker-Rolle muss *nicht* installiert werden.
- Das Verschlüsseln File System (EFS) muss deaktiviert sein.
- Das Bild mithilfe der Parameter SYSPREPed muss **/oobe / generalize/shutdown** ( **/mode:vm** -Parameter nicht verwenden).
- Hochladen der virtuellen Festplatte aus einem Snapshot wird nicht unterstützt.


**Bevor Sie beginnen**

Sie müssen Folgendes vor dem Erstellen des Dienstes:

- [Anmelden](https://azure.microsoft.com/services/remoteapp/) für RemoteApp.
- Erstellen Sie ein Benutzerkonto in Active Directory als RemoteApp-Dienstkonto verwenden. Schränken Sie die Berechtigungen für dieses Konto so ein, dass nur Computer in der Domäne beitreten können. Weitere Informationen finden Sie unter [Azure Active Directory konfigurieren für RemoteApp](remoteapp-ad.md) .
- Informationen über das lokale Netzwerk: IP-Adresse, Informationen und VPN-Geräte.
- [Azure PowerShell](../powershell-install-configure.md) -Modul zu installieren.
- Informationen Sie über den Benutzer Zugriff zu gewähren. Dies kann Microsoft-Kontoinformationen oder Active Directory arbeiten Kontoinformationen für Benutzer.



## <a name="create-a-template-image"></a>Erstellen einer Vorlagenbild ##

Dies sind die Schritte eine neue Vorlagenbild erstellen:

1.  Suchen Sie Windows Server 2012 R2 aktualisieren DVD oder ISO-Image.
2.  Erstellen Sie eine VHD-Datei.
4.  Installieren Sie Windows Server 2012 R2.
5.  Installieren der Rolle Remote Desktop Session Host (RDSH) und die Desktop Experience-Funktion.
6.  Installieren Sie zusätzliche Features der Anwendung erforderlich.
7.  Installieren Sie und konfigurieren Sie der Anwendung. Fügen Sie zur Vereinfachung der Freigabe apps apps oder Programme im **Startmenü** des Bildes in **%systemdrive%\ProgramData\Microsoft\Windows\Start \Programme freigeben möchten.
8.  Führen Sie zusätzlichen Windows Konfigurationen Anwendung erforderlich.
9.  Deaktivieren Sie das verschlüsselnde Dateisystem (EFS).
10. **Erforderlich:** Windows Update und installieren Sie alle wichtigen Updates.
9.  SYSPREP das Bild.

Ausführliche Anleitung zum Erstellen eines neuen Bildes sind:

1.  Suchen Sie Windows Server 2012 R2 aktualisieren DVD oder ISO-Image.
2.  Erstellen Sie eine VHD-Datei verwenden.
    1.  Starten Sie Disk Management (diskmgmt.msc).
    2.  Erstellen Sie eine dynamisch erweiterbare virtuelle Festplatte mindestens 40 GB Größe. (Schätzen Sie für Windows, Applikationen und Anpassung benötigten Speicherplatz. Mit der RDSH-Rolle und die Desktop Experience-Feature installiert WindowsServer benötigen ca. 10 GB Speicherplatz).
        1.  Klicken Sie auf **Aktion > virtuelle Festplatte erstellt**.
        2.  Geben Sie den Speicherort, Größe und VHD-Format. **Wählen Sie **dynamisch erweiterbar****

            Dies führt für einige Sekunden. Nach Abschluss die Erstellung VHD sollte eine neue Festplatte ohne Laufwerk und "Nicht initialisiert" in der Datenträgerverwaltungskonsole angezeigt werden.

        - Maustaste auf der Festplatte (nicht der Speicherplatz) und klicken Sie dann auf **Datenträger initialisieren**. **Wählen Sie den Partitionstyp **MBR** (Master Boot Record)**
        - Erstellen eines neuen Volumes: Maustaste auf den freien Speicherplatz und klicken Sie dann auf **Neues Volume**. Übernehmen Sie die Standardeinstellungen des Assistenten können sicherstellen, dass zur Vermeidung möglicher Probleme beim Bild der Vorlage einen Laufwerkbuchstaben zuweisen.
        - Klicken Sie den Datenträger und dann auf **VHD trennen**.





1. Installieren Sie Windows Server 2012 R2:
    1. Erstellen Sie einen neuen virtuellen Computer. Verwenden Sie Assistent für neue virtuelle Computer in Hyper-V-Manager oder Client Hyper-V.
        1. Wählen Sie auf der Seite Specify Generation **Generation 1**.
        2. Wählen **Sie auf der Seite virtuelle Festplatte verbinden eine vorhandene virtuelle Festplatte**und suchen auf der virtuellen Festplatte, die Sie im vorherigen Schritt erstellt haben.
        2. Wählen Sie auf der Seite Installationsoptionen **ein Betriebssystem von einer CD-ROM starten**, und wählen Sie den Speicherort der Windows Server 2012 R2-Installationsmedium.
        3. Optionen Sie andere im Assistenten für Windows und die Anwendung installieren. Beenden Sie den Assistenten.
    2.  Nach Abschluss des Assistenten bearbeiten Sie des virtuellen Computers Änderungen Sie Windows und die Programme wie Anzahl der virtuellen Prozessoren installieren und klicken Sie dann auf **OK**.
    4.  Verbinden mit dem virtuellen Computer, und installieren Sie Windows Server 2012 R2.
1. Installieren der Rolle Remote Desktop Session Host (RDSH) und die Desktop Experience-Funktion:
    1. Starten Sie Server-Manager.
    2. Klicken Sie auf der Willkommensseite oder Menü **Verwalten** auf **Hinzufügen von Rollen und Features** .
    3. Klicken Sie auf der Seite Vorbereitung auf **Weiter** .
    4. Wählen Sie **Rolle oder Funktion-basierten Installation**und klicken Sie dann auf **Weiter**.
    5. Aus der Liste Wählen Sie aus und dann auf **Weiter**.
    6. Wählen Sie **Remote Desktop Services**, und klicken Sie auf **Weiter**.
    7. Erweitern Sie **Benutzeroberflächen und Infrastruktur** zu, und wählen Sie **Desktop Experience**.
    8. Klicken Sie auf **Features hinzufügen**, und klicken Sie dann auf **Weiter**.
    9. Klicken Sie auf der Seite Remote Desktop Services auf **Weiter**.
    10. Klicken Sie auf **Remote Desktop Session Host**.
    11. Klicken Sie auf **Features hinzufügen**, und klicken Sie dann auf **Weiter**.
    12. Aktivieren Sie auf der Seite Confirm Installation Auswahl, **Starten Sie den Zielserver ggf. automatisch**und **Klicken Sie die Neustart-Warnung** .
    13. Klicken Sie auf **Installieren**. Der Computer wird neu gestartet.
1.  Installieren Sie zusätzlicher Features von den Programmen wie.NET Framework 3.5 erforderlich. Um die Features führen Sie Rollen hinzufügen und Funktionen Assistenten aus.
7.  Installieren und Konfigurieren von Programmen und Applikationen über RemoteApp veröffentlichen möchten.

>[AZURE.IMPORTANT]
>
>Installieren der Rolle RDSH vor Anwendung sicherstellen, dass Probleme mit der Anwendungskompatibilität erkannt werden, bevor das Bild RemoteApp hochgeladen wird.
>
>Stellen Sie sicher, dass eine Verknüpfung mit der Anwendung (**lnk** -Datei) im **Startmenü** für alle Benutzer (%systemdrive%\ProgramData\Microsoft\Windows\Start \Programme) wird angezeigt. Auch sicherstellen Sie, dass das Symbol im **Startmenü** sehen was Benutzer sehen dürfen. Wenn dies nicht der Fall ist, ändern. (Nicht *haben* hinzufügen die Anwendung zum Anfang Menü jedoch erleichtert die Anwendung in RemoteApp veröffentlichen. Andernfalls müssen Sie den Installationspfad für die Anwendung bereitstellen, wenn Sie die Anwendung veröffentlichen.)


8.  Führen Sie zusätzlichen Windows Konfigurationen Anwendung erforderlich.
9.  Deaktivieren Sie das verschlüsselnde Dateisystem (EFS). Führen Sie den folgenden Befehl in einem Befehlsfenster mit erhöhten Rechten aus:

        Fsutil behavior set disableencryption 1

    Alternativ legen Sie oder fügen Sie den folgenden DWORD-Wert in der Registrierung:

        HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisableEncryption = 1
9.  Wenn Sie das Bild einer Azure virtuellen Computer erstellen, Umbenennen der ** \%windir%\Panther\Unattend.xml** Datei, da dies das Upload-Skript verwendet, später aus blockieren. Ändern Sie den Namen dieser Datei in Unattend.old, damit Sie die Datei noch werden bei die Bereitstellung wiederherstellen müssen.
10. Windows Update und installieren Sie alle wichtigen Updates. Möglicherweise müssen mehrmals alle Updates zu Windows Update ausführen. (Manchmal ein Update installieren, und das Update selbst ist ein Update erforderlich.)
10. SYSPREP das Bild. Führen Sie den folgenden Befehl in ein Eingabeaufforderungsfenster mit erhöhten Rechten:

    **C:\Windows\System32\sysprep\sysprep.exe / generalize/shutdown/oobe**

    **Hinweis:** Verwenden Sie den Schalter **/mode:vm** Befehl SYSPREP nicht, obwohl dies einen virtuellen Computer.


## <a name="next-steps"></a>Nächste Schritte ##
Sie Ihre benutzerdefinierte Vorlagenbild haben, müssen Sie dieses Bild Ihrer RemoteApp-Sammlung. Verwenden Sie die Informationen in den folgenden Artikeln eine Auflistung erstellen:


- [Erstellen eine hybridsammlung von RemoteApp](remoteapp-create-hybrid-deployment.md)
- [Erstellen Sie eine cloudsammlung von RemoteApp](remoteapp-create-cloud-deployment.md)
 