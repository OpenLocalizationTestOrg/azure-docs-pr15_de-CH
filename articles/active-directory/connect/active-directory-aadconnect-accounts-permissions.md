<properties
   pageTitle="Azure AD Connect: Konten und Berechtigungen | Microsoft Azure"
   description="Dieses Thema beschreibt die Konten verwendet und erstellt und Berechtigungen."
   services="active-directory"
   documentationCenter=""
   authors="billmath"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"  
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/04/2016"
   ms.author="billmath"/>


# <a name="azure-ad-connect-accounts-and-permissions"></a>Azure AD verbinden: Konten und Berechtigungen
Der Installationsassistent Azure AD Connect bietet zwei verschiedene Wege:

- Im Express benötigt der Assistent mehr Berechtigungen, damit sie Ihre Konfiguration ohne Benutzer erstellen oder Berechtigungen einzeln konfigurieren, setup.

- Benutzerdefinierte Einstellung der Assistent bietet mehr Optionen und Wahlmöglichkeiten, aber es gibt einige Situationen, in denen müssen sicherstellen, dass Sie die richtigen Berechtigungen selbst.

## <a name="related-documentation"></a>Zugehörige Dokumentation
Dokumentation zur [Integration von Ihrem lokalen Identitäten Azure Active Directory](../active-directory-aadconnect.md)nicht gelesen haben, enthält die folgende Tabelle enthält Links zu Themen.

Thema |  
--------- | ---------
Installieren mit Express-Standardeinstellungen | [Schnellinstallation Azure AD Connect](active-directory-aadconnect-get-started-express.md)
Installieren Sie benutzerdefinierte Einstellungen | [Benutzerdefinierte Installation von Azure AD verbinden](active-directory-aadconnect-get-started-custom.md)
Aktualisieren von DirSync | [Aktualisieren von Azure AD Synchronisierungsprogramm (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md)


## <a name="express-settings-installation"></a>Expressinstallationsdateien settings
Express-Einstellungen fordert der Installationsassistent AD DS Organisations-Admin-Anmeldeinformationen für lokale Active Directory erforderliche Berechtigungen für Azure AD-Verbindung konfiguriert werden kann. Aktualisieren von DirSync werden zum Zurücksetzen des Kennworts für das Konto DirSync AD DS Unternehmensadministratoren Anmeldeinformationen verwendet. Sie benötigen außerdem Azure AD globale Administratorrechte.

Seite  | Gesammelten Anmeldeinformationen | Erforderliche Berechtigungen| Zum
------------- | ------------- |------------- |-------------
N/A|Benutzer der Installations-Assistent| Administrator des lokalen Servers| <li>Erstellt das lokale Konto [Synchronisieren Engine-Dienstkonto](#azure-ad-connect-sync-service-account)verwendet wird.
Verbinden mit Azure AD| Azure AD Directory-Anmeldeinformationen | Globale Administratorrolle in Azure AD | <li>Aktivieren der Synchronisierung in Azure AD-Verzeichnis.</li>  <li>Erstellung von [Azure AD-Konto](#azure-ad-service-account) , das für kontinuierliches Synchronisierungsvorgänge in Azure AD verwendet wird.</li>
Verbinden mit AD DS | Lokale Active Directory-Anmeldeinformationen | Mitglied der Gruppe Enterprise Admins (EA) in Active Directory| <li>Erstellt ein [Konto](#active-directory-account) in Active Directory und er erteilt. Dieses Konto dient zum Lesen und Schreiben von Verzeichnisinformationen während der Synchronisation.</li>

### <a name="enterprise-admin-credentials"></a>Organisations-Admin-Anmeldeinformationen
Diese Anmeldeinformationen werden nur während der Installation verwendet und nach Abschluss der Installation verwendet. Ist Organisationsadministrator und kein Domänenadministrator, um sicherzustellen, dass die Berechtigungen in Active Directory in allen Domänen festgelegt werden können.

### <a name="global-admin-credentials"></a>Globale Administratoranmeldeinformationen
Diese Anmeldeinformationen werden nur während der Installation verwendet und werden nicht verwendet, nachdem die Installation abgeschlossen ist. Es dient zum Synchronisieren von Änderungen Azure AD [Azure AD-Konto](#azure-ad-service-account) erstellen. Das Konto kann auch Sync als Feature in Azure AD.

### <a name="permissions-for-the-created-ad-ds-account-for-express-settings"></a>Berechtigungen für die AD DS erstellte Konto für express-Einstellungen
Zum Lesen und Schreiben in AD DS erstellte [Konto](#active-directory-account) haben die folgenden Berechtigungen beim express Einstellungen:

Berechtigung | Zum
---- | ----
<li>Verzeichniswechsel replizieren</li><li>Replizieren Directory ändert alle | Kennwortsynchronisierung
Lese-/Schreibzugriff alle Eigenschaften Benutzer | Importieren und Exchange hybrid
Lese-/Schreibzugriff allen Eigenschaften iNetOrgPerson | Importieren und Exchange hybrid
Lese-/Schreibzugriff allen Gruppeneigenschaften | Importieren und Exchange hybrid
Lese-/Schreibzugriff erhalten alle Eigenschaften | Importieren und Exchange hybrid
Kennwort zurücksetzen | Vorbereitung für das Rückschreiben Kennwort aktivieren

## <a name="custom-settings-installation"></a>Benutzerdefinierte Einstellung installation
Wenn Sie Standardeinstellungen verwenden, muss das Konto zum Verbinden mit Active Directory vor der Installation erstellt werden. Berechtigungen Sie diesem Konto zuweisen müssen finden in [der AD DS-Konto erstellen](#create-the-ad-ds-account).

Seite  | Gesammelten Anmeldeinformationen | Erforderliche Berechtigungen| Zum
------------- | ------------- |------------- |-------------
N/A | Benutzer der Installations-Assistent|<li>Administrator des lokalen Servers</li><li>Wenn eine vollständige SQL Server verwenden, muss der Benutzer sein System Administrator (SA) in SQL</li>| Standardmäßig erstellt das lokale Konto [Synchronisieren Engine-Dienstkonto](#azure-ad-connect-sync-service-account)verwendet wird. Das Konto wird nur erstellt, wenn der Administrator kein bestimmtes Konto angeben.
Synchronisierungsdienste Kontooption Dienst installieren | Anzeige oder lokale Anmeldeinformationen | Benutzer werden vom Assistenten Berechtigungen | Wenn der Administrator ein Konto gibt, wird dieses Konto als Dienstkonto für Synchronisierungsdienst verwendet.
Verbinden mit Azure AD | Azure AD Directory-Anmeldeinformationen| Globale Administratorrolle in Azure AD| <li>Aktivieren der Synchronisierung in Azure AD-Verzeichnis.</li>  <li>Erstellung von [Azure AD-Konto](#azure-ad-service-account) , das für kontinuierliches Synchronisierungsvorgänge in Azure AD verwendet wird.</li>
Verbinden Sie Ihre Verzeichnisse | Lokale Active Directory-Anmeldeinformationen für jede Gesamtstruktur in Azure Active Directory verbunden ist | Die Berechtigungen abhängen, welche Features Sie aktivieren, und finden Sie unter [Erstellen der AD DS-Konto](#create-the-ad-ds-account) |Dieses Konto wird zum Lesen und Schreiben von Verzeichnisinformationen während der Synchronisierung verwendet.
AD FS-Server | Für jeden Server in der Liste erfasst der Assistenten Anmeldeinformationen wird die Anmeldeinformationen der Benutzer den Assistenten nicht verbinden | Domänenadministrator | Installation und Konfiguration der ADFS-Rolle.
Web Application Proxyserver |Für jeden Server in der Liste erfasst der Assistenten Anmeldeinformationen wird die Anmeldeinformationen der Benutzer den Assistenten nicht verbinden | Lokaler Administrator auf dem Zielcomputer | Installation und Konfiguration von WAP-Serverrolle.
Proxyanmeldeinformationen vertrauen |Verbunddienst vertrauen Anmeldeinformationen (die Anmeldeinformationen des Proxys verwendet registrieren für vertrauenswürdige Zertifikate von der FS |Domänenkonto, die ein lokaler Administrator die AD FS-server | Erstmaligen Registrierung Vertrauenszertifikat FS WAP.
AD FS Seite Dienstkonto "Eine Option Domäne Konto verwenden" | AD-Anmeldeinformationen | Domänenbenutzer | Active Directory-Benutzerkonto, dessen Anmeldeinformationen, wird als Anmeldekonto des AD FS-Dienstes verwendet.

### <a name="create-the-ad-ds-account"></a>AD DS-Konto erstellen
Bei der Installation von Azure AD Connect auf **verbinden die Verzeichnisse** angegebene Konto in Active Directory vorhanden sein und müssen Berechtigungen. Der Installationsassistent überprüft nicht die Berechtigungen und Probleme bei der Synchronisierung nur gefunden.

Sie benötigen Berechtigungen hängt die optionalen Funktionen ermöglichen. Wenn Sie über mehrere Domänen verfügen, müssen die Berechtigungen für alle Domänen in der Gesamtstruktur. Wenn Sie diese Funktionen nicht aktivieren, reichen die Standardberechtigungen **Domänenbenutzer** .

Funktion | Berechtigungen
------ | ------
Kennwortsynchronisierung | <li>Verzeichniswechsel replizieren</li>  <li>Replizieren Directory ändert alle
Exchange-Hybrid-Bereitstellung | Schreibberechtigungen Sie für die Attribute für Benutzer, Gruppen und Kontakten in [Exchange Hybrid Rückschreiben](../active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback) dokumentiert.
Kennwort Rückschreiben | Schreibberechtigungen Sie für die Attribute in [Erste Schritte mit Passwort-Management](../active-directory-passwords-getting-started.md#step-4-set-up-the-appropriate-active-directory-permissions) für Benutzer dokumentiert.
Gerät Rückschreiben | Berechtigungen mit PowerShell-Skript unter [Gerät Rückschreiben](../active-directory-aadconnect-feature-device-writeback.md).
Gruppenrückschreiben | Lesen Sie, erstellen Sie, aktualisieren Sie und löschen Sie Gruppenobjekte in der Organisationseinheit, in dem die Verteilergruppen befinden soll.

## <a name="upgrade"></a>Aktualisieren
Beim upgrade von einer Version von Azure AD-Verbindung zur neuen Version benötigen Sie die folgenden Berechtigungen:

Principal | Erforderliche Berechtigungen | Zum
---- | ---- | ----
Benutzer der Installations-Assistent | Administrator des lokalen Servers | Binärdateien aktualisiert.
Benutzer der Installations-Assistent | Mitglied ADSyncAdmins | Ändern Sie die Synchronisierungsregeln und anderen.
Benutzer der Installations-Assistent | Verwenden Sie einen vollständigen SQL Server: DBO (oder ähnlich) des Datenbankmoduls synchronisieren | Stellen Sie die Datenbank ändert, wie Tabellen mit neuen Spalten aktualisiert.

## <a name="more-about-the-created-accounts"></a>Weitere Informationen zu den erstellten Konten

### <a name="active-directory-account"></a>Active Directory-Konto
Wenn Sie express-Einstellungen verwenden, wird ein Konto in Active Directory erstellt, die für die Synchronisierung verwendet wird. Das erstellte Konto befindet sich in der Stammdomäne im Benutzercontainer und hat der Namen mit dem Präfix **MSOL_**. Das Konto ist mit einem langen komplexen Kennwort erstellt, das nicht abläuft. Haben Sie eine Kennwortrichtlinie in der Domäne sicher lange und komplexe Kennwörter für dieses Konto zulässig wäre.

![Konto](./media/active-directory-aadconnect-accounts-permissions/adsyncserviceaccount.png)

### <a name="azure-ad-connect-sync-service-accounts"></a>Azure AD Connect Sync-Dienstkonten
Lokales Dienstkonto wird vom Assistenten erstellt (sofern das Konto mit Standardeinstellungen angeben). Das Konto mit dem Präfix **AAD_** und zum tatsächlichen Synchronisierungsdienst ausgeführt. Wenn Sie Azure AD Connect auf einem Domänencontroller installieren, wird das Konto in der Domäne erstellt. Verwenden Sie einen Remoteserver mit SQL Server oder verwenden Sie einen Proxy, der Authentifizierung erfordert, muss das **AAD_** -Konto in der Domäne befinden.

![Sync-Dienstkonto](./media/active-directory-aadconnect-accounts-permissions/syncserviceaccount.png)

Das Konto ist mit einem langen komplexen Kennwort erstellt, das nicht abläuft.

Dieses Konto zum Speichern der Kennwörter für die Konten sicher. Diese anderen Konten Kennwörter sind verschlüsselt in der Datenbank gespeichert. Die privaten Schlüssel für den Verschlüsselungsschlüssel mit der Verschlüsselung mit geheimem Schlüssel Kryptografiedienste mit Windows Data Protection API (DPAPI) geschützt sind. Das Kennwort für das Dienstkonto sollte nicht zurückgesetzt werden, da Windows dann die Schlüssel aus Sicherheitsgründen gelöscht.

Verwenden Sie eine vollständige SQL Server ist das Dienstkonto DBO der erstellten Datenbank für das Synchronisierungsmodul. Der Dienst funktioniert nicht wie andere Berechtigungen. Eine SQL-Anmeldung wird ebenfalls erstellt.

Das Konto wird auch Berechtigungen auf Dateien, Registrierungsschlüssel und andere Objekte im Zusammenhang mit dem Synchronisierungsmodul.

### <a name="azure-ad-service-account"></a>Azure AD-Dienstkontos
Für die Verwendung der Synchronisierungsdienst wird ein Konto in Azure AD erstellt. Dieses Konto kann mit ihrem Anzeigenamen identifiziert werden.

![Konto](./media/active-directory-aadconnect-accounts-permissions/aadsyncserviceaccount.png)

Der Name des Servers, der das Konto verwendet wird, kann im zweiten Teil des Benutzernamens identifiziert werden. In der Abbildung ist der Servername FABRIKAMCON. Haben Sie Stagingserver hat jeder Server seine eigenen Konto. Gibt es ein Limit von 10 Sync-Dienstkonten in Azure AD.

Das Dienstkonto wird mit einem langen komplexen Kennwort erstellt, das nicht abläuft. Es ist eine besondere Rolle **Directory Synchronization Konten** gewährt, die nur zum Verzeichnis Synchronisierung Aufgaben Berechtigungen. Azure-Portal zeigt dieses Konto in der Rolle **Benutzer**und diese spezielle integrierte Rolle kann nicht außerhalb des Assistenten Azure AD Connect gewährt.

![Konto Rolle](./media/active-directory-aadconnect-accounts-permissions/aadsyncserviceaccountrole.png)

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [die lokalen Identitäten Azure Active Directory integrieren](../active-directory-aadconnect.md).
