<properties
   pageTitle="Azure AD verbinden: Versionsverlauf Version | Microsoft Azure"
   description="Dieses Thema listet alle Versionen von Azure AD Connect und Azure AD synchronisieren"
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-version-release-history"></a>Azure AD verbinden: Versionsverlauf Version

Das Team Azure Active Directory aktualisiert Azure AD Verbinden mit neuen Features und Funktionen. Nicht alle Zugänge gelten für alle Zielgruppen.

Dieser Artikel soll, können Sie die Versionen des verfolgen, die veröffentlicht wurden und Sie wissen, ob Sie oder nicht auf die neueste Version aktualisieren müssen.

Dies ist eine Liste verwandter Themen:

Thema |  
--------- | --------- |
Schritte zum Aktualisieren von Azure AD verbinden | Methoden zur [Aktualisierung von einer früheren Version auf die neueste](active-directory-aadconnect-upgrade-previous-version.md) Azure AD Connect Version.
Erforderliche Berechtigungen | Berechtigungen zum Anwenden von Updates finden Sie unter [Konten und Berechtigungen](./connect/active-directory-aadconnect-accounts-permissions.md#upgrade)
Herunterladen| [Download Azure AD verbinden](http://go.microsoft.com/fwlink/?LinkId=615771)

## <a name="112810"></a>1.1.281.0
Veröffentlicht: 2016 August

**Behobene Probleme:**

- Ändert Intervall synchronisieren nicht erfolgt erst nach der nächsten Synchronisierung Zyklus abgeschlossen ist.
- Azure AD Connect-Assistent akzeptiert keine Azure Konto, dessen Benutzername mit einem Unterstrich beginnt (\_).
- Azure AD Connect-Assistenten nicht authentifiziert Azure AD-Konto bereitgestellte Kennwort enthält zu viele Sonderzeichen. Fehlermeldung "Anmeldeinformationen überprüfen. Ein unerwarteter Fehler aufgetreten." wird zurückgegeben.
- Deinstallation Stagingserver Kennwortsynchronisation in Azure AD-Mandanten deaktiviert und Kennwort synchronisiert active Server fehlschlägt.
- Kennwort Synchronisierungsfehlern in selten Fällen wird keine Kennworthash für den Benutzer gespeichert.
- Wenn Azure AD Connect Server für staging-Modus aktiviert ist, ist Kennwort Rückschreiben nicht vorübergehend deaktiviert.
- Azure AD Connect-Assistent wird Synchronisierung Kennwort und Kennwort Rückschreiben Konfiguration bei Server staging-Modus wird nicht angezeigt. Es zeigt immer sie deaktiviert.
- Neukonfiguration Kennwortsynchronisation und Kennwort Rückschreiben werden von Azure AD Connect-Assistenten beim Server staging-Modus wird nicht beibehalten.

**Verbessert:**

- Aktualisierte Start-ADSyncSyncCycle-Cmdlet an, ob einen neuer Zyklus synchronisieren oder nicht gestartet werden kann.
- Hinzugefügte Stop ADSyncSyncCycle Cmdlet Sync-Zyklus und Vorgang zurzeit beendet.
- Aktualisierte Stop-ADSyncScheduler-Cmdlet Sync-Zyklus und Vorgang zurzeit beendet.
- Wenn [Verzeichnis Extensions](active-directory-aadconnectsync-feature-directory-extensions.md) Azure AD Connect-Assistenten konfigurieren, kann AD-Attribut vom Typ "Teletex-Zeichenfolge" jetzt ausgewählt.

## <a name="111890"></a>1.1.189.0
Veröffentlicht: 2016 Juni

**Behoben und verbessert:**

- Azure AD Connect kann nun auf einem FIPS-kompatible Server installiert werden.
    - Synchronisierung von Kennwörtern finden Sie unter [Kennwort und FIPS](active-directory-aadconnectsync-implement-password-synchronization.md#password-synchronization-and-fips)
- Behoben, konnte ein NetBIOS-Name nicht der FQDN in Active Directory Connector aufgelöst werden.

## <a name="111800"></a>1.1.180.0
Veröffentlicht: 2016 Mai

**Neue Funktionen:**

- Gibt eine Warnung aus und hilft Ihnen, Domänen überprüfen, wenn Sie es nicht vor dem Ausführen von Azure AD verbinden.
- Unterstützung für [Microsoft-Cloud Deutschland](active-directory-aadconnect-instances.md#microsoft-cloud-germany).
- Unterstützung für die neuesten [Microsoft Azure Government Cloud](active-directory-aadconnect-instances.md#microsoft-azure-government-cloud) -Infrastruktur mit neuen URL.

**Behoben und verbessert:**

- Zusätzlicher Filter, synchronisieren Regeleditor Sync Regeln erleichtert.
- Verbesserte Leistung beim Löschen einer Connectorspace.
- Ein Fehler behoben, wenn dasselbe Objekt sowohl gelöscht und in der gleichen ausführen (aufgerufene löschen/hinzufügen) hinzugefügt wurde.
- Deaktivierte synchronisieren können nicht mehr reaktivieren enthalten Objekte und Attribute im Schema aktualisieren oder Verzeichnis aktualisieren.

## <a name="111300"></a>1.1.130.0
Veröffentlicht: 2016 April

**Neue Funktionen:**

- Unterstützung für mehrwertige Attribute [Verzeichnis](active-directory-aadconnectsync-feature-directory-extensions.md)Extensions.
- Unterstützung für weitere Konfigurationsvarianten für [Automatische Aktualisierung](active-directory-aadconnect-feature-automatic-upgrade.md) gelten für die Aktualisierung.
- Einige Cmdlets für [benutzerdefinierten Planer](active-directory-aadconnectsync-feature-scheduler.md#custom-scheduler)hinzugefügt.

## <a name="111190"></a>1.1.119.0
Veröffentlicht: 2016 März

**Behobene Probleme:**

- Gemacht wird sicher, dass die Express-Installation unter Windows Server 2008 (vor R2) seit kennwortsynchronisierung verwendet werden kann auf diesem Betriebssystem nicht.
- Aktualisierung von DirSync mit einer benutzerdefinierten Konfiguration funktionierte nicht wie erwartet.
- Beim Upgrade auf eine neuere Version gibt es keine Änderung der Konfiguration und eine vollständige Import-Synchronisierung nicht geplant.

## <a name="111100"></a>1.1.110.0
Veröffentlicht: 2016 Februar

**Behobene Probleme:**

- Upgrade von früheren Versionen funktioniert nicht, wenn die Installation nicht im Standardordner **C:\Program Files** ist.
- Wenn Sie installieren und **Starten der Synchronisierung Prozess...** deaktivieren am Ende des Installations-Assistenten ermöglicht erneut ausführen des Installationsassistenten nicht den Planer.
- Der Planer funktioniert nicht erwartungsgemäß auf Servern Datum/Zeitformat nicht US-En ist. Es blockiert auch `Get-ADSyncScheduler` zum korrekten Zeitpunkt zurück.
- Wenn Sie als Anmeldeoption und Aktualisierung eine frühere Version von Azure AD Verbinden mit ADFS installiert, kann nicht den Installationsassistenten erneut ausführen.

## <a name="111050"></a>1.1.105.0
Veröffentlicht: 2016 Februar

**Neue Funktionen:**

- Funktion [Automatische Aktualisierung](active-directory-aadconnect-feature-automatic-upgrade.md) Express Einstellungen Kunden.
- Unterstützung für globale Administratoren mit MFA PIM im Installations-Assistenten.
    - Sie müssen den Proxy auch auf https://secure.aadcdn.microsoftonline-p.com Datenverkehr verwenden MFA zulassen.
    - Sie müssen https://secure.aadcdn.microsoftonline-p.com in Ihre Liste vertrauenswürdiger Sites für MFA ordnungsgemäß hinzufügen.
- Ändern der Benutzer-Methode nach der Erstinstallation zulässig.
- [Domäne und Organisationseinheit Filtern](./connect/active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) in der Installations-Assistent zulassen. Dies ermöglicht außerdem das Verbinden zu einer Gesamtstruktur, wenn nicht alle Domänen verfügbar sind.
- [Planer](active-directory-aadconnectsync-feature-scheduler.md) wird in dem Synchronisierungsmodul integriert.

**Funktionen zum GA Vorschau:**

- [Gerät Rückschreiben](active-directory-aadconnect-feature-device-writeback.md).
- [Verzeichnis Extensions](active-directory-aadconnectsync-feature-directory-extensions.md).

**Neue Vorschaufunktionen:**

- Der neue Standard synchronisieren Zyklus Intervall beträgt 30 Minuten. 3 Stunden für alle früheren Versionen verwendet. Unterstützung das [Planer](active-directory-aadconnectsync-feature-scheduler.md) Verhalten ändern.

**Behobene Probleme:**

- Seite überprüfen DNS-Domänen erkannt nicht immer Domänen.
- Fordert Administratoranmeldeinformationen Domäne ADFS konfigurieren.
- Der lokale AD-Konten werden vom Installations-Assistenten nicht erkannt, wenn in einer Domäne mit einer anderen DNS-Struktur als die Stammdomäne.

## <a name="1091310"></a>1.0.9131.0
Veröffentlicht: 2015 Dezember

**Behobene Probleme:**

- Kennwortsynchronisierung funktioniert möglicherweise nicht, Kennwörter in AD DS, aber Works zu ändern, wenn Sie Kennwort festlegen.
- Wenn Sie einen Proxy-Server haben, möglicherweise während der Installation oder aufheben Azure AD-Authentifizierung auf der Konfigurationsseite aktualisieren.
- Aktualisieren von einer früheren Version von Azure AD Verbinden mit einer Fehler SQL Server-, wenn nicht SA in SQL sind.
- Aktualisieren von einer früheren Version von Azure AD-Verbindung mit einem Remote zeigt SQL Server den Fehler "Unable to ADSync SQL Datenbank".

## <a name="1091250"></a>1.0.9125.0
Zur Verfügung: 2015 November

**Neue Funktionen:**

- ADFS Azure AD Trust können neu konfigurieren.
- Active Directory-Schema aktualisieren können und Synchronisierungsregeln regenerieren.
- Können Synchronisierungsregel deaktivieren.
- Kann "AuthoritativeNull" als neue Literal Sync Regel definieren.

**Neue Vorschaufunktionen:**

- [Azure AD Connect Health für die Synchronisierung](active-directory-aadconnect-health-sync.md).
- Unterstützung für Kennwortsynchronisation [Azure Active Directory-Domänendienste](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords) .

**Neue unterstützt:**

- Mehrere lokale Exchange-Organisationen unterstützt. Weitere Informationen finden Sie unter [Hybrid-Bereitstellung mit mehreren Active Directory-Gesamtstrukturen](https://technet.microsoft.com/library/jj873754.aspx) .

**Behobene Probleme:**

- Synchronisierungsprobleme Kennwort:
    - Ein Objekt außerhalb des Bereichs im Bereich verschoben haben nicht das Kennwort synchronisiert. Diese Hilfebereich OU und Attribute filtern.
    - Auswählen einer Organisationseinheit synchron sind erfordert keine vollständige Kennwort synchronisieren.
    - Aktiviert ein deaktivierten Benutzer das Kennwort nicht synchronisiert.
    - Die Wiederholungswarteschlange Kennwort ist und die vorherige Grenze von 5.000 Objekten zurückgezogen werden entfernt.
    - [Verbesserte Fehlerbehebung](active-directory-aadconnectsync-implement-password-synchronization.md#troubleshoot-password-synchronization).
- Kann nicht mit Windows Server 2016 Gesamtstruktur-Funktionsebene mit Active Directory herzustellen.
- Kann nicht zur Gruppe filtern nach Erstinstallation Gruppe ändern.
- Ein neues Benutzerprofil erstellt auf Azure AD Connect Server für jeden Benutzer durchführen einer Änderung mit aktiviertem Rückschreiben Kennwort nicht mehr.
- Nicht für Long Integer Werte synchronisieren Regeln Bereiche.
- Das Kontrollkästchen "Gerät Rückschreiben" bleibt deaktiviert, wenn der Domänencontroller nicht erreichbar sind.

## <a name="1086670"></a>1.0.8667.0
Veröffentlicht: 2015 August

**Neue Funktionen:**

- Azure AD Connect-Installationsassistenten ist jetzt für alle Windows Server-Sprachen lokalisiert.
- Unterstützung für Konto entsperren Verwendung Azure AD Kennwort-Management.

**Behobene Probleme:**

- Azure AD Connect-Installationsassistenten stürzt ab, wenn ein anderer Benutzer weiterhin anstelle die Installation bei der ersten Person.
- Wenn eine frühere Deinstallation Azure AD Connect nicht Azure AD Connect Sync saubere Deinstallation kann nicht neu installieren.
- Nicht installieren Azure AD Verbinden mit Express installieren, ist der Benutzer nicht in der Stammdomäne der Gesamtstruktur oder eine nicht-englischen Version von Active Directory verwendet wird.
- Der FQDN des Active Directory-Benutzerkontos aufgelöst werden kann, erscheint eine irreführende Fehlermeldung "Fehler beim Schema übernehmen".
- Wenn das Konto auf dem Active Directory Connector außerhalb des Assistenten geändert wird, fehl Assistenten auf nachfolgende.
- Azure AD Connect kann auch auf einem Domänencontroller installieren.
- Kann nicht aktivieren und Deaktivieren von "Staging-Modus" Wenn Erweiterungsattribute hinzugefügt wurden.
- Kennwort Rückschreiben scheitert in einigen Konfiguration eines falschen Kennworts auf Active Directory Connector.
- DirSync kann nicht aktualisiert werden, wenn dn Attribut Filtern verwendet wird.
- Eine übermäßige CPU-Auslastung beim Zurücksetzen des Kennworts verwenden.

**Entfernt Vorschaufunktionen:**

- Die Vorschaufunktion [Zellenrückschreiben Benutzer](active-directory-aadconnect-feature-preview.md#user-writeback) vorübergehend entfernt wurde basierend auf Feedback von Kunden Vorschau. Es wird später erneut hinzugefügt werden, wenn wir die angegebene Bewertung eingegangen.

## <a name="1086410"></a>1.0.8641.0
Veröffentlicht: 2015 Juni

**Erste Version von Azure AD verbinden.**

Der Name von Azure AD Sync Azure AD verbinden.

**Neue Funktionen:**

- Expressinstallation [settings](./connect/active-directory-aadconnect-get-started-express.md)
- [ADFS konfigurieren](./connect/active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) können
- [Aktualisieren von DirSync](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md) können
- [Versehentliche Löschen sperren](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)
- [Staging-Modus](active-directory-aadconnectsync-operations.md#staging-mode) eingeführt

**Neue Vorschaufunktionen:**

- [Benutzer Rückschreiben](active-directory-aadconnect-feature-preview.md#user-writeback)
- [Gruppenrückschreiben](active-directory-aadconnect-feature-preview.md#group-writeback)
- [Gerät Rückschreiben](active-directory-aadconnect-feature-device-writeback.md)
- [Verzeichnis extensions](active-directory-aadconnect-feature-preview.md#directory-extensions)


## <a name="104940501"></a>1.0.494.0501
Veröffentlicht: 2015 Mai

**Neue Anforderung:**

- Azure AD-Synchronisierung benötigt .net Frameworkversion 4.5.1 installiert werden.

**Behobene Probleme:**

- Rückschreiben von Azure AD Kennwort ist mit einem Verbindungsfehler Servicebus fehl.

## <a name="104910413"></a>1.0.491.0413
Veröffentlicht: 2015 April

**Behoben und verbessert:**

- Active Directory Connector verarbeitet keine löscht korrekt, wenn der Papierkorb aktiviert ist und mehrere Domänen in der Gesamtstruktur vorhanden sind.
- Die Leistung der Importvorgänge verbessert für Azure Active Directory Connector.
- Wenn eine Gruppe wurde überschritten Mitgliedschaft (standardmäßig das Limit soll 50k Objekte), die Gruppe in Azure Active Directory gelöscht wurde. Das neue Verhalten ist die Gruppe, wird ein Fehler ausgelöst und keine neue Gruppenmitgliedschaft ändert exportiert werden.
- Ein neues Objekt kann bereitgestellt werden, wenn eine bereitgestellte löschen mit dem gleichen DN bereits im Connectorspace vorhanden ist.
- Einige Objekte werden markiert, für während der deltasynchronisierung synchronisiert werden, obwohl keine des Objekts bereitgestellt Änderung.
- Erzwingen der kennwortsynchronisierung entfernt auch die bevorzugte DC.
- CSExportAnalyzer hat Probleme mit einigen Objekten.

**Neue Funktionen:**

- Eine Verknüpfung können jetzt auf "ANY" Objekt im Metaverse.

## <a name="104850222"></a>1.0.485.0222
Veröffentlicht: 2015 Februar

**Verbessert:**

- Verbesserte Import-Leistung.

**Behobene Probleme:**

- Kennwort-Sync berücksichtigt das Attribut CloudFiltered durch Attribute filtern. Gefilterte Objekte nicht mehr im Gültigkeitsbereich für die Synchronisierung von Kennwörtern.
- Kennwort synchronisiert funktioniert seltenen Situationen, in denen die Topologie sehr viele Domänencontroller, nicht.
- "Beendet-Server" beim Importieren von Azure Active Directory Connector nach Device-Management wurde in Azure AD/Intune aktiviert.
- Fremde Sicherheitsprinzipale (FSPs) aus mehreren Domänen in derselben Gesamtstruktur beitreten verursacht Fehler mehrdeutige Join.

## <a name="104751202"></a>1.0.475.1202
Veröffentlicht: 2014 Dezember

**Neue Funktionen:**

- Kennwort Synchronisierung mit Attribut-basierte Filterung wird jetzt unterstützt. Weitere Informationen finden Sie unter [Kennwortsynchronisation mit Filtern](active-directory-aadconnectsync-configure-filtering.md).
- Attribut MsDS-ExternalDirectoryObjectID wird wieder in Active Directory geschrieben. Diese Unterstützung für Office 365-Applikationen mit OAuth2 Zugriff sowohl Online und lokale Postfächer in einer Exchange-Bereitstellung Hybrid.

**Update behoben:**

- Eine neuere Version von den Assistenten ist auf dem Server verfügbar.
- Ein benutzerdefinierten Installationspfad wurde zur Azure AD Sync installieren.
- Eine ungültige benutzerdefinierte Verknüpfung Kriterium blockiert das Upgrade.

**Weitere Updates:**

- Vorlagen fest für Office Pro Plus.
- Feste Installationsprobleme aufgrund von Benutzernamen mit starten.
- Feste SourceAnchor Einstellung verlieren, wenn Sie den Assistenten erneut ausführen.
- Feste ETW für Kennwortsynchronisation

## <a name="104701023"></a>1.0.470.1023
Veröffentlicht: 2014 Oktober

**Neue Funktionen:**

- Synchronisierung von Kennwörtern von mehreren lokalen Anzeige Azure AD.
- Lokalisierte Benutzeroberfläche für Installation auf allen Windows Server-Sprachen.

**Aktualisieren von AADSync 1.0 GA**

Haben Sie bereits Azure AD Sync installiert, ist ein zusätzlicher Schritt haben Sie im Falle eines Out-of-Box-Synchronisierungsregeln geändert haben. Nach der Aktualisierung auf die 1.0.470.1023 freigeben Sie, die Synchronisation, die Regeln geändert wurden dupliziert werden. Für jede geänderte Synchronisierungsregel folgendermaßen vor:

- Suchen Sie die Regel Synchronisierung geändert und beachten Sie die Änderung.
- Löschen Sie Synchronisierungsregel
- Suchen von Azure AD Sync erstellt Sync-Neuregelung und neu zuweisen.

**Berechtigungen für das Konto**

Das Konto muss zusätzliche die Kennwort-Hashes aus Active Directory gelesen werden Berechtigungen. Die zu gewährenden Berechtigungen heißen "Verzeichniswechsel replizieren" und "Replizieren Directory ändert alle". Die Kennworthashes lesen sind beide Berechtigungen erforderlich

## <a name="104190911"></a>1.0.419.0911
Veröffentlicht: 2014 September

**Erste Version von Azure AD synchronisieren.**

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über [die lokalen Identitäten Azure Active Directory integrieren](active-directory-aadconnect.md).
