<properties
    pageTitle="Azure AD Connect: Gerät Rückschreiben aktivieren | Microsoft Azure"
    description="Dieses Dokument enthält Informationen zum Gerät Rückschreiben mit Azure AD verbinden aktivieren"
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-enabling-device-writeback"></a>Azure AD Connect: Gerät Rückschreiben aktivieren

>[AZURE.NOTE] Ein Abonnement für Azure AD Premium ist erforderlich für das Gerät Rückschreiben.

Folgende Dokumentation bietet Informationen zum Gerät Rückschreibfeature in Azure AD-Verbindung aktivieren. Gerät Rückschreiben ist in den folgenden Szenarien verwendet:

- Bedingter Zugriff basierend auf Geräten mit ADFS (2012 R2 oder höher) Anwendung (relying Party Vertrauensstellungen) geschützt.

Dies bietet zusätzliche Sicherheit und Sicherheit der Anwendung nur für vertrauenswürdige Geräte zugreifen kann. Weitere Informationen zu bedingten Zugriff finden Sie unter [Verwalten von Risiken mit Zugangskontrolle](active-directory-conditional-access.md) und [Einrichten von lokalen Zugangskontrolle mit Azure Active Directory Geräteregistrierung](https://msdn.microsoft.com/library/azure/dn788908.aspx).

>[AZURE.IMPORTANT]
<li>Geräte müssen in der gleichen Gesamtstruktur wie die Benutzer befinden. Da Geräte in einer einzigen Gesamtstruktur zurückgeschrieben werden müssen, unterstützt diese Funktion derzeit eine Bereitstellung mit mehreren Gesamtstrukturen der Benutzer nicht.</li>
<li>Lokale Active Directory-Gesamtstruktur kann nur ein Gerät Registrierungsobjekt Konfiguration hinzugefügt. Dieses Feature ist nicht kompatibel mit einer Topologie, lokale Active Directory mit mehreren Azure AD-Verzeichnissen synchronisiert werden.</li>

## <a name="part-1-install-azure-ad-connect"></a>Teil 1: Installation Azure AD verbinden
1. Azure AD Verbinden mit benutzerdefinierten installieren oder Express-Einstellungen. Microsoft empfiehlt zu allen Benutzern und Gruppen erfolgreich synchronisiert, bevor Gerät Rückschreiben aktivieren.

## <a name="part-2-prepare-active-directory"></a>Teil 2: Vorbereiten von Active Directory
Gehen Sie zur Vorbereitung der Geräte Rückschreiben verwenden.

1.  Von Azure AD Connect Installationsort Computer starten Sie PowerShell im erhöhten Modus.

2.  Wenn Active Directory PowerShell-Modul nicht installiert ist, installieren Sie mit dem folgenden Befehl:

    `Install-WindowsFeature –Name AD-Domain-Services –IncludeManagementTools`

3. Wenn Azure Active Directory PowerShell-Modul nicht installiert ist, downloaden und Installieren von [Azure Active Directory Modul für Windows PowerShell (64-Bit-Version)](http://go.microsoft.com/fwlink/p/?linkid=236297). Diese Komponente enthält eine Abhängigkeit auf den Anmelde-Assistenten, die mit Azure AD Connect installiert wird.

4.  Enterprise Administratoranmeldeinformationen führen Sie die folgenden Befehle mit PowerShell beenden.

    `Import-Module 'C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1'`

    `Initialize-ADSyncDeviceWriteback {Optional:–DomainName [name] Optional:-AdConnectorAccount [account]}`

Enterprise Administratoranmeldeinformationen sind erforderlich, da Änderungen Namespace Konfiguration benötigt werden. Ein Domänenadministrator haben nicht genügend Berechtigungen.

![PowerShell für Gerät Rückschreiben aktivieren](./media/active-directory-aadconnect-feature-device-writeback/powershell.png)

Beschreibung:

- Wenn sie nicht bereits vorhanden, erstellt und konfiguriert die neuen Container und Objekte unter CN = Registrierung Konfiguration, CN = Dienste, CN = Configuration, [Gesamtstruktur-dn.
- Wenn sie nicht bereits vorhanden, erstellt und neuen Container und Objekte CN konfiguriert = RegisteredDevices, [Domain-dn. Geräteobjekte werden in diesem Container erstellt.
- Erforderliche Berechtigungen festgelegt auf Azure Active Directory Connector-Konto zum Verwalten von Geräten in Active Directory.
- Nur muss in einer Gesamtstruktur ausführen, auch wenn Azure AD Verbinden mehrerer Gesamtstrukturen installiert wird.

Parameter:

- Domänenname: Active Directory-Domäne dem Gerät erstellt werden. Hinweis: Alle Geräte für eine bestimmte Active Directory-Gesamtstruktur werden in einer einzelnen Domäne erstellt.
- AdConnectorAccount: Active Directory-Konto, Azure AD Connect Objekte im Verzeichnis verwalten. Dies ist das Konto von Azure AD Connect Sync Verbindung mit AD verwendet. Installiert express-Einstellungen ist das Konto mit dem Präfix MSOL_.

## <a name="part-3-enable-device-writeback-in-azure-ad-connect"></a>Teil 3: Aktivieren Gerät Rückschreiben in Azure AD verbinden
Gehen Sie Rückschreiben in Azure AD Connect Gerät aktivieren.

1.  Führen Sie den Installationsassistenten erneut aus. **Anpassen von Optionen für die Synchronisierung** auf Weitere Aufgaben und auf **Weiter.**
![Benutzerdefinierte Installation anpassen Synchronisierungsoptionen](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback2.png)
2.  Auf der Seite Optionen werden Gerät Rückschreiben nicht abgeblendet. Beachten Sie, dass wenn Azure AD Connect Prep Schritte nicht abgeschlossenen Gerät Rückschreiben wird bitte, auf optionale Features deaktiviert. Aktivieren Sie das Kontrollkästchen für das Rückschreiben Gerät, und klicken Sie auf **Weiter**. Wenn das Kontrollkästchen immer noch deaktiviert ist, siehe [Abschnitt Problembehandlung](#the-writeback-checkbox-is-still-disabled).
![Benutzerdefinierte Installation Gerät Rückschreiben optionale Funktionen](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback3.png)
3.  Auf der Seite Rückschreiben sehen Sie angegebene Domäne als Standard Gerät Rückschreiben Gesamtstruktur.
![Benutzerdefinierte Installation Gerät Rückschreiben Zielgesamtstruktur](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback4.png)
4.  Abschließen der Installation des Assistenten ohne zusätzliche Konfiguration ändern. Bei Bedarf finden Sie [benutzerdefinierte Installation von Azure AD Connect.](./connect/active-directory-aadconnect-get-started-custom.md)

## <a name="enable-conditional-access"></a>Bedingter Zugriff
Informationen zum Aktivieren dieses Szenarios sind in [lokalen Zugangskontrolle mit Azure Active Directory Geräteregistrierung einrichten](https://msdn.microsoft.com/library/azure/dn788908.aspx)verfügbar.

## <a name="verify-devices-are-synchronized-to-active-directory"></a>Überprüfen Sie, ob Geräte mit Active Directory synchronisiert sind
Rückschreiben Gerät sollte jetzt ordnungsgemäß ausgeführt. Beachten Sie, dass Geräteobjekte geschriebene AD werden bis zu 3 Stunden dauern kann.  Um sicherzustellen, dass die Geräte ordnungsgemäß synchronisiert sind, folgendermaßen Sie nach Abschluss der Synchronisierungsregeln:

1.  Starten Sie Active Directory-Verwaltungscenter.
2.  Erweitern Sie RegisteredDevices in der Domäne verbunden ist.
![Verwaltungskonsole für Active Directory registrierte Geräte](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback5.png)
3.  Aktuelle registrierte Geräte werden hier aufgeführt.
![Active Directory-Verwaltungskonsole registriert die Geräteliste](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback6.png)

## <a name="troubleshooting"></a>Problembehandlung

### <a name="the-writeback-checkbox-is-still-disabled"></a>Das Rückschreiben ist weiterhin deaktiviert
Wenn das Kontrollkästchen für das Gerät Rückschreiben nicht aktiviert ist, obwohl Sie die obigen Schritte befolgt haben, die folgenden Schritte führen Sie durch was der Installationsassistent überprüft werden, bevor das aktiviert ist.

Zunächst erste:

- Stellen Sie sicher, dass mindestens eine Gesamtstruktur Windows Server 2012R2. Der Gerätetyp Objekt muss vorhanden sein.
- Wenn der Installations-Assistent bereits ausgeführt wird, werden jede Änderung nicht erkannt. In diesem Fall führen Sie den Installationsassistenten und erneut ausführen.
- Stellen Sie sicher, das Konto in der Initialisierungsskript bieten tatsächlich den richtigen Benutzer von Active Directory Connector verwendet. Gehen Sie hierzu folgendermaßen vor:
    - Öffnen Sie das Startmenü **Synchronisierungsdienst**.
    - Öffnen Sie die Registerkarte **Connectors** .
    - Die Verbindung mit Active Directory-Domänendienste suchen und auswählen.
    - Wählen Sie unter **Aktionen** **Eigenschaften**.
    - Fahren Sie mit **Active Directory-Gesamtstruktur herstellen**. Stellen Sie sicher, dass die Domäne und den Namen auf diesem Bildschirm die bereitgestellt, um das Skript angegeben.
![Connector-Konto in Sync-Dienst-Manager](./media/active-directory-aadconnect-feature-device-writeback/connectoraccount.png)

Überprüfen Sie die Konfiguration in Active Directory:
- Überprüfen, ob der Registrierungsdienst Gerät am folgenden Speicherort befindet (CN = DeviceRegistrationService, CN Gerät Registrierungsdienste, CN = Registrierung Konfiguration, CN = Dienste, CN = Konfiguration =) unter Konfigurationsnamenskontext.

![Problembehandlung bei DeviceRegistrationService im Konfigurations-namespace](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot1.png)

- Überprüfen Sie, ob nur ein Konfigurationsobjekt Konfiguration Namespace suchen. Wenn mehr als einmal vorhanden ist, löschen Sie den.

![Problembehandlung, doppelte Objekte suchen](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot2.png)

- Sicherstellen Sie auf das Objekt Gerät Registrierungsdienst Attribut MsDS-DeviceLocation vorhanden ist und einen Wert. Suche Speicherort und achten ist mit ObjectType MsDS-DeviceContainer.

![Problembehandlung bei MsDS-DeviceLocation](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot3.png)

![Problembehandlung bei RegisteredDevices Objektklasse](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot4.png)

- Überprüfen Sie, ob das Konto des Active Directory Connectors im vorherigen Schritt gefunden registrierte Geräte Container erforderlichen Berechtigungen verfügt. Dies ist die erwarteten Berechtigungen für diesen Container:

![Problembehandlung, überprüfen Sie die Berechtigungen für container](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot5.png)

- Überprüfen Sie das Active Directory-Konto hat Berechtigungen für CN Registrierung Konfiguration, CN = Dienste, CN = = Configuration-Objekt.

![Problembehandlung, überprüfen Sie die Berechtigungen auf Registrierung Gerätekonfiguration](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot6.png)

## <a name="additional-information"></a>Weitere Informationen
- [Risikomanagement mit Zugangskontrolle](active-directory-conditional-access.md)
- [Lokal Zugriff bedingten mit Azure Active Directory Geräteregistrierung](https://msdn.microsoft.com/library/azure/dn788908.aspx)

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über [die lokalen Identitäten Azure Active Directory integrieren](active-directory-aadconnect.md).
