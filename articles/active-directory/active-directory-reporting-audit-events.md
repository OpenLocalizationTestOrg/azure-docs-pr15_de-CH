<properties
   pageTitle="Azure Active Directory Überwachungsereignisse Bericht | Microsoft Azure"
   description="Überwacht Ereignisse, die zum Anzeigen und Herunterladen von Azure Active Directory verfügbar sind"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/19/2016"
   ms.author="dhanyahk"/>

# <a name="azure-active-directory-audit-report-events"></a>Azure Active Directory Überwachungsereignisse Bericht

*Diese Dokumentation ist Teil der [Azure Active Directory Berichtshandbuch] (Active-Directory-reporting-guide.md).*

Azure Active Directory Prüfungsbericht Kunden privilegierte Aktionen bestimmen, die in Azure Active Directory aufgetreten. Privilegierte Aktionen sind Höhenunterschiede (z. B. Erstellen von Rollen oder Kennwörtern), ändern Richtlinienkonfigurationen (z. B. Kennwortrichtlinien) oder Verzeichniskonfiguration (z. B. Änderungen Föderation Einstellungen) geändert. Berichte bieten Audit-Datensatz für den Ereignisnamen Schauspieler Aktion der Zielressource der Änderung sowie Datum und Uhrzeit (in UTC) betroffen ausgeführte. Kunden können die Überwachungsereignisse für Azure Active Directory [Azure-Portal](https://portal.azure.com/)abzurufen unter [Ansicht die Audit-Protokolle](active-directory-reporting-azure-portal.md).


## <a name="list-of-audit-report-events"></a>Liste der Überwachungsereignisse Bericht
<!--- audit event descriptions should be in the past tense --->

Ereignisse                               | Beschreibung
------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------
**Benutzer-Ereignisse**                      |
Benutzer hinzufügen                             | Benutzer hinzugefügt zum Verzeichnis.
Benutzer löschen                          | Einen Benutzer aus dem Verzeichnis gelöscht.
Lizenz-Eigenschaften festlegen               | Legen Sie die Lizenzeigenschaften für einen Benutzer im Verzeichnis.
Kennwort zurücksetzen                  | Setzen Sie das Kennwort für einen Benutzer im Verzeichnis.
Benutzerkennwort ändern                 | Das Kennwort für einen Benutzer im Verzeichnis geändert.
Lizenz für Benutzer ändern                  | Benutzer im Verzeichnis zugewiesene Lizenz geändert. Welche Lizenzen aktualisiert wurden, sehen Sie die [Benutzer aktualisieren](#update-user-attributes) Eigenschaften unten
Benutzer aktualisieren                          | Benutzer im Verzeichnis aktualisiert. [Siehe unten](#update-user-attributes) für Attribute, die aktualisiert werden können.
Benutzerkennwort festlegen Kraft ändern       | Legen Sie die Eigenschaft, die ein Benutzer ihr Kennwort bei der Anmeldung ändern.
Anmeldeinformationen aktualisieren        | Benutzer hat das Kennwort geändert  
**Ereignisse gruppieren**                     |
Gruppe hinzufügen                            | Erstellt eine Gruppe im Verzeichnis.
Aktualisierungsgruppe                         | Eine Gruppe im Verzeichnis aktualisiert. Um anzuzeigen, welche Eigenschaften aktualisiert wurden, finden Sie im Abschnitt [Gruppe Eigenschaften überwacht](#update-group-attributes)
Gruppe löschen                         | Eine Gruppe aus dem Verzeichnis gelöscht.
Mitglied zur Gruppe hinzufügen            | Mitglied hinzugefügt einer Gruppe im Verzeichnis.
Element aus Gruppe entfernen         | Entfernt ein Mitglied aus einer Gruppe im Verzeichnis.
CreateGroupSettings              | Erstellte Gruppen
UpdateGroupSettings          | Aktualisierte Einstellungen. Um anzuzeigen welche Einstellungen aktualisiert wurden, finden Sie im Abschnitt [Gruppe Eigenschaften überwacht](#update-group-attributes)
DeleteGroupSettings            | Gelöschte Gruppen
SetGroupLicense                  | Group-Lizenz festgelegt.
SetGroupManagedBy              | Gruppe Benutzer verwaltet werden
AddGroupMember               | Element zur Gruppe hinzugefügt
RemoveGroupMember            | Element aus Gruppe entfernen
AddGroupOwner                | Hinzugefügte Besitzer, Gruppe
RemoveGroupOwner                 | Entfernte Besitzer von Gruppe
**Application-Ereignisse**               |
Hinzufügen von Dienstprinzipalnamen                | Einen Dienstprinzipal hinzugefügt zum Verzeichnis.
Entfernen von Dienstprinzipalnamen             | Einen Dienstprinzipal entfernt aus dem Verzeichnis.
Principal Anmeldeinformationen hinzufügen    | Hinzugefügte Anmeldeinformationen für einen Dienstprinzipal.
Service principal Anmeldedaten entfernen | Anmeldeinformationen für einen Dienstprinzipal entfernt.
Eintrag für die Delegierung                 | Ein [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) erstellt im Verzeichnis.
Gruppe Delegierungseintrags                 | Ein [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) im Verzeichnis aktualisiert.
Delegierung Eintrag entfernen              | Ein [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) im Verzeichnis gelöscht.
**Rolle Ereignisse**                      |
Mitglied der Rolle zu Rolle hinzufügen              | Verzeichnisrolle hinzugefügt einen Benutzer.
Rolle der Rolle entfernen         | Einen Benutzer entfernt aus einer verzeichnisrolle.
Festlegen von Unternehmen      | Voreinstellungen Sie Unternehmen Kontakt. Dazu gehören e-Mail-Adressen für marketing sowie technische Benachrichtigung über Microsoft Online Services.
Eintrag für die Delegierung                 | Ein [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) erstellt im Verzeichnis.
Gruppe Delegierungseintrags                 | Ein [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) im Verzeichnis aktualisiert.
Delegierung Eintrag entfernen              | Ein [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) im Verzeichnis gelöscht.
AddSevicePrincipalOwner          | Hinzugefügte Besitzer Dienstprinzipalnamen.
RemoveSevicePrincipalOwner   | Besitzer von Dienstprinzipalnamen entfernt.
AddApplication  | Anwendung hinzufügen.
UpdateApplication | Aktualisieren Sie Anwendung. Um anzuzeigen welche Appeinstellungen aktualisiert wurden, finden Sie im Abschnitt [Anwendung Eigenschaften überwacht](#update-application-attributes)
DeleteApplication | Löschen Sie Anwendung
RestoreApplication|Wiederherstellen Sie Anwendung.
AddApplicationOwner   |     Anwendung Besitzer hinzugefügt.
RemoveApplicationOwner    |     Entfernen Sie Besitzer von Anwendung
**Rolle Ereignisse**                         |
Mitglied der Rolle zu Rolle hinzufügen                 | Verzeichnisrolle hinzugefügt einen Benutzer.
Rolle der Rolle entfernen            | Einen Benutzer entfernt aus einer verzeichnisrolle.
AddRoleDefinition               | Hinzugefügte Rollendefinition.
UpdateRoleDefinition                | Aktualisierte Rollendefinition. Um zu sehen, welche Einstellungen aktualisiert wurden, finden Sie im Abschnitt [Rolle Definition Eigenschaften überwacht](#update-role-definition-attributes)
DeleteRoleDefinition                | Gelöschte Rollendefinition.
AddRoleAssignmentToRoleDefinition | Hinzugefügte rollenzuweisung zur Rollendefinition.
RemoveRoleAssignmentFromRoleDefinition | Entfernt Rolle Auftrag Rollendefinition.
AddRoleFromTemplate     | Hinzugefügte Rolle von Vorlage.
UpdateRole  | Aktualisierte Rolle.
AddRoleScopeMemberToRole    | Bewertete Element Rolle hinzugefügt.
RemoveRoleScopedMemberFromRole  | Rolle entfernt Bereichselement.
**Ereignisse (alle neuen Ereignisse)**                    |
Hinzugefügt werden               | Hinzugefügte Gerät.
UpdateDevice            | Aktualisierte Gerätetreiber. Um anzuzeigen, welche Eigenschaften aktualisiert wurden, finden Sie unter [Eigenschaften Löschvorgänge](#update-device-attributes) im Abschnitt
DeleteDevice            | Gelöschte Gerät.
AddDeviceConfiguration      | Konfiguration des Geräts.
UpdateDeviceConfiguration   | Aktualisierte Konfiguration. Um anzuzeigen welche Konfigurationseigenschaften aktualisiert wurden, finden Sie unter [Konfiguration Geräteeigenschaften Löschvorgänge](#update-device-configuration-attributes) im Abschnitt
DeleteDeviceConfiguration   | Gelöschte Konfiguration.
AddRegisteredOwner     | Hinzugefügte Besitzer Gerät.
AddRegisteredUsers     | Registrierte Benutzer hinzugefügt Gerät.
RemoveRegisteredOwner    | Entfernen Sie Besitzer vom Gerät.
RemoveRegisteredUsers    | Entfernen Sie registrierte Benutzer vom Gerät.
RemoveDeviceCredentials  | Entfernen Sie Anmeldeberechtigungen des Geräts
**B2B-Ereignisse**                       |
Batch lädt hochgeladen.              | Ein Administrator hat eine Datei mit Einladungen an Partnerbenutzer hochgeladen.
Batch-Verarbeitung lädt.             | Eine Datei mit einer Partnerbenutzer wurde verarbeitet.
Externen Benutzer einladen.                | Das Verzeichnis wurde ein externer Benutzer eingeladen.
Einlösung von externen Benutzer einladen.         | Ein externer Benutzer hat die Einladung zum Verzeichnis eingelöst.
Externen Benutzer zu Gruppe hinzufügen.          | Ein externer Benutzer ist Mitglied einer Gruppe im Verzeichnis zugeordnet.
Externen Benutzer Anwendung zuweisen. | Ein externer Benutzer hat direkten Zugriff auf eine Anwendung zugeordnet.
Virale Mieter erstellen.               | Ein neuer Mandanten in Azure AD durch Rückzahlung Einladung wurde.
Virale erstellt.                 | Ein Benutzer in einem vorhandenen Mandanten in Azure AD durch Rückzahlung Einladung wurde.
**Einheiten (alle neuen Ereignisse)**             |
AddAdministrativeUnit   |   Administrative Einheit hinzufügen
UpdateAdministrativeUnit|   Aktualisieren Sie administrative Einheit. Um anzuzeigen welche Verwaltungseinheit Eigenschaften aktualisiert wurden, finden Sie im Abschnitt [Administrative Eigenschaften überwacht](#update-administrative-unit-attributes)
DeleteAdministrativeUnit|   Löschen Sie administrative Einheit
AddMemberToAdministrativeUnit|  Administrative Einheit Member hinzufügen.
RemoveMemberFromAdministrativeUnit|     Administrative Einheit entfernen Sie Element.
**Directory-Ereignisse**                 |
Unternehmen Partner hinzufügen               | Partner hinzugefügt zum Verzeichnis.
Partner von Unternehmen entfernen          | Partner entfernt aus dem Verzeichnis.
DemotePartner   |   Herabstufen Sie Partner.
Hinzufügen einer Domäne zu Unternehmen                | Eine Domäne hinzugefügt zum Verzeichnis.
Unternehmen Domäne entfernen           | Eine Domäne entfernt aus dem Verzeichnis.
Domäne aktualisieren                        | Eine Domäne im Verzeichnis aktualisiert. Um anzuzeigen welche Domäneneigenschaften aktualisiert wurden, finden Sie im Abschnitt [Eigenschaften überwacht](#update-domain-attributes)
Festlegen der Domänenauthentifizierung            | Die Standardeinstellung für die Domäne des Unternehmens geändert.
Festlegen von Unternehmen      | Voreinstellungen Sie Unternehmen Kontakt. Dazu gehören e-Mail-Adressen für marketing sowie technische Benachrichtigung über Microsoft Online Services.
Einstellen der Föderation in Domäne    | Aktualisiert die Einstellungen Föderation für eine Domäne.
Domäne überprüfen                        | Eine Domäne im Verzeichnis überprüft.
Überprüft e-Mail-Domäne überprüfen         | Überprüft eine Domäne für das Verzeichnis über e-Mail-Überprüfung.
Kennzeichen Sie DirSyncEnabled-für Unternehmen   | Legen Sie die Eigenschaft, die ein Verzeichnis für Azure AD synchronisieren kann.
Legen Sie Kennwortrichtlinien                  | Beschränkungen Sie Länge und Zeichen für Benutzerkennwörter.
Informationen festlegen              | Die Unternehmen Informationen aktualisiert. Das PowerShell-Cmdlet [Get-MsolCompanyInformation](https://msdn.microsoft.com/library/azure/dn194126.aspx) für weitere Details anzeigen
SetCompanyAllowedDataLocation  |    Satz darf Datenspeicherort.
SetCompanyDirSyncEnabled    |   DirSyncEnabled-Kennzeichen.
SetCompanyDirSyncFeature |  Dirsync-Funktion festgelegt.
SetCompanyInformation |   Unternehmensinformationen festlegen.
SetCompanyMultiNationalEnabled    |     Multinationale Unternehmen-Funktion aktiviert festgelegt.
SetDirectoryFeatureOnTenant   |     Verzeichnisfunktion Mieter festgelegt.
SetTenantLicenseProperties  |   Mieter Lizenzeigenschaften festgelegt.
CreateCompanySettings   |   Firma erstellen
UpdateCompanySettings   |   Einstellungen zu aktualisieren. Um anzuzeigen, welche Unternehmen Eigenschaften aktualisiert wurden, finden Sie im Abschnitt [Firma überwacht](#update-company-attributes)
DeleteCompanySettings   |   Einstellungen löschen
SetAccidentalDeletionThreshold   |  Festlegen Sie versehentliches Löschen Schwellenwert.
SetRightsManagementProperties   |   Festlegen Sie Rechte Eigenschaften.
PurgeRightsManagementProperties     |   Löschen Sie Rights Management Eigenschaften.
UpdateExternalSecrets   |   Externe Schlüssel aktualisieren
**Richtlinienereignisse (alle neuen Ereignisse)**           |
AddPolicy   |   Richtlinie hinzufügen.
UpdatePolicy    |   Richtlinie aktualisiert werden.
DeletePolicy    |   Löschen Sie Richtlinie.
AddDefaultPolicyApplication     |   Richtlinie Anwendung hinzufügen.
AddDefaultPolicyServicePrincipal    |   Dienstprinzipalnamen Richtlinie hinzufügen.
RemoveDefaultPolicyApplication  |   Entfernen Sie Richtlinie Anwendung.
RemoveDefaultPolicyServicePrincipal     |   Dienstprinzipalnamen entfernen Sie Richtlinie.
RemovePolicyCredentials     |   Entfernen Sie Richtlinie Anmeldedaten.

## <a name="audit-report-retention"></a>Audit Bericht Aufbewahrung
In Azure AD Prüfungsbericht sind 180 Tage lang aufbewahrt. Weitere Informationen zur Aufbewahrung in Berichten finden Sie in [Azure Active Directory Bericht-Aufbewahrungsrichtlinien](active-directory-reporting-retention.md).

Für Kunden, deren Überwachungsereignisse längere Aufbewahrung speichern API-Berichte dienen regelmäßig Überwachungsereignisse in einem separaten Datenspeicher übertragen. Einzelheiten finden Sie unter [Erste Schritte mit der Reporting-API](active-directory-reporting-api-getting-started.md) .

## <a name="properties-included-with-each-audit-event"></a>Eigenschaften der einzelnen Überwachungsereignis

Eigenschaft      | Beschreibung
------------- | --------------------------------------------------------------
Datum und Uhrzeit | Datum und Uhrzeit des Ereignisses überwachen
Schauspieler         | Der Benutzer oder die Dienstprinzipalnamen, die die Aktion durchgeführt
Aktion        | Ausgeführte Aktion
Ziel        | Der Benutzer oder die Dienstprinzipalnamen auf die Aktion ausgeführt wurde


## <a name="update-user-attributes"></a>Attribute "Benutzer aktualisieren"
Das Überwachungsereignis "Benutzer aktualisieren" enthält zusätzliche Informationen, welche Benutzerattribute aktualisiert wurden. Für jedes Attribut ist des vorherigen Wertes und der neue Wert enthalten.

Attribut                           | Beschreibung
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled                      | Benutzer kann sich anmelden.
AssignedLicense                     | Alle Lizenzen, die dem Benutzer zugewiesen wurde.
AssignedPlan                        | Service-Pläne aus dem Benutzer zugewiesenen Lizenzen.
LicenseAssignmentDetail             | Informationen zu dem Benutzer zugewiesenen Lizenzen. Gruppenbasierte Lizenzierung beteiligt war, würde dieser Gruppe zählen, die die Lizenz erteilt.
Mobile                              | Mobiltelefon des Benutzers.
OtherMail                           | Alternative e-Mail-Adresse des Benutzers.
OtherMobile                         | Alternative Mobiltelefon des Benutzers.
StrongAuthenticationMethod          | Eine Liste der Verfahren für die kombinierte Authentifizierung wie Stimme, SMS oder Überprüfung von einer mobilen Anwendung vom Benutzer konfiguriert.
StrongAuthenticationRequirement     | Wenn mehrstufige Authentifizierung erzwungen wird, aktiviert oder deaktiviert für diesen Benutzer.
StrongAuthenticationUserDetails     | Die Telefonnummer des Benutzers, alternativen Telefonnummern und e-Mail-Adresse für die kombinierte Authentifizierung und Kennwort zurücksetzen Überprüfung.
StrongAuthenticationPhoneAppDetail  | Detaillierte Forof Phone apps registriert 2FA Anmeldung ausführen
TelephoneNumber                     | Telefonnummer des Benutzers.
AlternativeSecurityId               | Eine alternative Sicherheits-ID für das Objekt.
CreationType                        |Erstellungsmethode des Benutzers (oder Einladung virale).
InviteTicket                        |Liste der Einladung Tickets für den Benutzer.
InviteReplyUrl                      |Liste der Urls Annahme der Einladung antworten.
InviteResources                     |Liste der Ressourcen, der Benutzer eingeladen wurde.
LastDirSyncTime                     |Letzte Aktualisierung des Objekts wurde aufgrund von autorisierenden synchronisieren (Kunde, lokale) Verzeichnis.
MSExchRemoteRecipientType           |Wird Empfängertypen MSO. [Empfängertypen MSO] https://msdn.microsoft.com/library/microsoft.office.interop.outlook.recipient.type.aspx für Empfängertypen finden
PreferredDataLocation               |Standort des Benutzers, Gruppe, Kontakt, öffentlichen Ordner und der Daten.
ProxyAddresses                      |Die Adresse, ein Exchange Server-Empfängerobjekt in einem fremden e-Mail-System erkannt wird.
StsRefreshTokensValidFrom           |Führt aktualisieren token Sperrinformationen - alle STS Aktualisierungstoken ausgestellt vor diesem Zeitpunkt gelten als abgelaufen.
UserPrincipalName                   |Der Benutzerprinzipalname ist ein Internet-Stil Anmeldename für einen Benutzer.
UserState                           |Status der Benutzer Ausstehende Genehmigung/PendingAcceptance/akzeptiert/PendingVerification.
UserStateChangedOn                  |Zeitstempel der letzten Änderung UserState. Zum Lebenszyklus Workflows auslösen.
Benutzertyp                            |Typ des Benutzers. Element (0), Gast (1) Viral(2).

## <a name="update-group-attributes"></a>"Update" Attribute
Attribut                           | Beschreibung
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
Klassifizierung            | Die Klassifizierung einer einheitlichen Gruppe (HBI MBI usw.).
Beschreibung               | Lesbare beschreibende Begriffe zum Objekt.  
DisplayName               |Der Anzeigename für ein Objekt
DirSyncEnabled            |Gibt an, ob die Synchronisierung von einem autorisierenden (Kunde, lokale) Verzeichnis.
GroupLicenseAssignment    |Lizenzzuweisung einer
GroupType                 |Gruppentyp, Unified (0)
IsMembershipRuleLocked    |Gibt an, dass die MembershipRule-Eigenschaft, indem der Gruppe Self-service-Verwaltungsdienst festgelegt wird und vom Benutzer geändert werden kann. Für Gruppen, wobei GroupType GroupType.DynamicMembership enthält
IsPublic                  |Flag zum Festlegen, ob die Gruppe öffentlich-Private.
LastDirSyncTime           |Letzte Aktualisierung des Objekts wurde durch die Synchronisierung von autorisierenden Verzeichnis (lokal Kunde).
E-Mail-Nachrichten                      |Primäre e-Mail-Adresse
MailEnabled               |Gibt an, ob dieser Gruppe e-Mail-Funktion.
MailNickname              |Für eine Adresse Buchobjekt Moniker, Teil des vorherigen e-Mail-Namens der "@" Symbol.   
MembershipRule            |Eine Zeichenfolge, die Kriterien der Gruppe Self-service-Verwaltungsdienst bestimmen, welche Mitglieder zu dieser Gruppe gehören sollen. Siehe auch IsMembershipRuleLocked. Gilt nur für Gruppen, wobei GroupType GroupType.DynamicMembership enthält.
MembershipRuleProcessingState| Ein Enumerationswert, der Gruppe Self-service-Verwaltungsdienst definieren den Status der Mitgliedschaft für diese Gruppe definiert. Gilt nur für Gruppen, wobei GroupType GroupType.DynamicMembership enthält.
ProxyAddresses            |Die Adresse, ein Exchange Server-Empfängerobjekt in einem fremden e-Mail-System erkannt wird.
RenewedDateTime           |Timestamp Datensatz als eine Gruppe zuletzt erneuert wurde.   
SecurityEnabled           |Gibt an, ob die Mitgliedschaft in der Gruppe Autorisierung Entscheidung beeinflussen kann.
WellKnownObject           |Bezeichnet ein Verzeichnisobjekt als einer vordefinierten Gruppe festlegen.

## <a name="update-device-attributes"></a>Attribute "Gerät aktualisieren"
Attribut                           | Beschreibung
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled | Gibt an, ob ein Sicherheitsprinzipal authentifizieren kann.
CloudAccountEnabled | Gibt an, ob ein Sicherheitsprinzipal authentifizieren kann. Wenn das Gerät lokal gemeistert InTune geschrieben.
CloudDeviceOSType | Gerätetyp basierend auf dem Betriebssystem z. B. Windows RT, iOS. Durch Cloud-Dienst (wie Intune) festgelegt, wird dieses Attribut autorisierenden im Verzeichnis DeviceOSType.
CloudDeviceOSVersion | Version des Betriebssystems. Durch Cloud-Dienst (wie Intune) festgelegt, wird dieses Attribut autorisierenden im Verzeichnis DeviceOSVersion.
CloudDisplayName | Wert der DisplayName LDAP-Attribut. Durch Cloud-Dienst (wie Intune) festgelegt, wird dieses Attribut im Verzeichnis DisplayName autorisierend.
CloudCreated |Gibt an, ob das Objekt von Cloud-Diensten erstellt wurde.
CompliantUntil | Bis wann gilt Gerät kompatibel.
DeviceMetadata |Benutzerdefinierte Metadaten für das Gerät
DeviceObjectVersion | Dieses Attribut wird verwendet, um die Schemaversion des Geräts zu identifizieren.
DeviceOSType |Gerätetyp basierend auf dem Betriebssystem z. B. Windows RT, iOS. Der Registrierungsdienst geschrieben und von MDM aktualisiert werden soll Licht Verwaltungsdienst oder STS-Verwaltungsdienst.
DeviceOSVersion |Version des Betriebssystems. Der Registrierungsdienst geschrieben und von MDM aktualisiert werden soll Licht Verwaltungsdienst oder STS-Verwaltungsdienst.
DevicePhysicalIds |Mehrwertiges Attribut soll Bezeichner des physischen Geräts speichern. Dazu gehören BIOS-IDs TPM Fingerabdrücke, Hardware usw. bestimmten IDs.
DirSyncEnabled | Gibt an, ob die Synchronisierung eines autorisierenden (Kunden, lokal) Verzeichnis.
DisplayName |Der Anzeigename für ein Objekt
IsCompliant | Dieses Attribut wird zur Verwaltung von mobile Device Management Status des Geräts.
IsManaged |Dieses Attribut wird verwendet, um anzugeben, dass das Gerät eine Wolke MDM verwaltet
LastDirSyncTime |Letzte Aktualisierung des Objekts wurde aufgrund von autorisierenden synchronisieren (Kunde lokal) Verzeichnis.

## <a name="update-device-configuration-attributes"></a>"Update Gerätekonfiguration" Attribute
Attribut                           | Beschreibung
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
MaximumRegistrationInactivityPeriod |Maximale Anzahl von Tagen, die eine davor werden gilt für entfernen.
RegistrationQuota   |Die Richtlinie beschränkt die Anzahl der geräteregistrierungen für einen einzelnen Benutzer zulässig.

## <a name="update-service-principal-configuration-attributes"></a>Attribute "Update Service principal-Konfiguration"
Attribut                           | Beschreibung
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled |Gibt an, ob ein Sicherheitsprinzipal authentifizieren kann.
AppPrincipalId | Externe Anwendung definierten Identität für einen Sicherheitsprinzipal.
DisplayName |Der Anzeigename für ein Objekt
ServicePrincipalName    | Service principal Name, mit "Name/Behörde" wo Name Gibt eine Anwendung Klassenwert und Autorität enthält mindestens Hostname [: Port] oder "Name", einen Bezeichner für den Dienstprinzipalnamen angibt.

## <a name="update-app-attributes"></a>Attribute "App aktualisieren"
Attribut                           | Beschreibung
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AppAddress |Der Satz von Adressen (Umleiten von URLs), die einen Dienstprinzipal zugewiesen sind.
AppId                               |ID der Anwendung der App   
AppIdentifierUri | Anwendung URI, der die Anwendung identifiziert.  Es ist normalerweise die URL zugreifen.
AppLogoUrl |Die Url für das Application-Logo in einem CDN gespeichert.
AvailableToOtherTenants | True, die Anwendung wird Multi-Tenant (d. h. dienen durch andere Mandanten).
DisplayName | Der Anzeigename für ein Anwendungsname
Berechtigung |Liste der Anwendung Berechtigungen.
ExternalUserAccountDelegationsAllowed |Zeigt an, ob ressourcenanwendung vertrauenswürdig ist und Delegierungseinträge für externe Benutzerkonten erstellen kann.
GroupMembershipClaims |Die Gruppenmitgliedschaft Ansprüche Richtlinie.
PublicClient | True, wenn der Client geheim halten kann (z. B. vertrauliche Client in OAuth2.0)
RecordConsentConditions | Arten von Zustimmung, gemäß den Vertragsbedingungen: keine (0) SilentConsentForPartnerManagedApp(1). Dieser Wert wird im Schema Graph-API verfügbar gemacht werden und kann nur von Administratoren Mieter/geändert.
RequiredResourceAccess |XML-Inhalt des Wertes der Eigenschaft RequiredResourceAccess.
WebApp |True gibt an, dass diese Anwendung eine Web app.
WwwHomepage |Der primären Webseite.

## <a name="update-role-attributes"></a>Attribute "Status aktualisieren"
Attribut                           | Beschreibung
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AppAddress |Der Satz von Adressen (Umleiten von URLs), die einen Dienstprinzipal zugewiesen sind.
BelongsToFirstLoginObjectSet |True gibt an, dass dieses Objekt dem Satz von Objekten erforderlich Anmeldung des ersten Admin eines neuen Mandanten gehört.
Vordefinierte |Gibt an, ob die Lebensdauer eines Objekts vom System gehört.
Beschreibung | Lesbare beschreibende Begriffe zum Objekt.  
DisplayName |Der Anzeigename für ein Objekt
MailNickname | Für eine Adresse Buchobjekt Moniker, Teil des vorherigen e-Mail-Namens der "@" Symbol.
RoleDisabled |Gibt an, ob die Rolle für Access Kontrolle ignoriert werden soll.
RoleTemplateId | Identität der Rollenvorlage.
ServiceInfo |Dienstspezifische Bereitstellungsinformationen, die von Verwaltungscenter oder andere Dienstinstanzen (der gleichen oder in verschiedenen Typen) belegt werden kann.
TaskSetScopeReference |Gibt eine TaskSet und verschiedene Bereiche einer Rolle oder Rollenvorlage.
ValidationError |Informationen von einem Verbunddienst einen dauerhaftes dienstspezifischen Fehler Eigenschaften oder Verknüpfung in einem Administrator Objekt zu beschreiben.
WellKnownObject |Bezeichnet ein Verzeichnisobjekt als einer vordefinierten Gruppe festlegen.

## <a name="update-role-definition-attributes"></a>"Update Rollendefinition" Attribute
Attribut                           | Beschreibung
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AssignableScopes    |Auflistung von Autorisierung Bereichen, die verwiesen werden kann, wenn ein Sicherheitsprinzipal dieses RoleDefinition zuweisen.
DisplayName     |Der Anzeigename für ein Objekt
GrantedPermissions  |Berechtigungen von diesem RoleDefinition.


## <a name="update-administrative-unit-attributes"></a>"Update Verwaltungseinheit" Attribute
Attribut                           | Beschreibung
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
Beschreibung |   Diese Eigenschaft wird aktualisiert, wenn Sie die Beschreibung einer Verwaltungseinheit ändern.
DisplayName |   Diese Eigenschaft wird aktualisiert, wenn Sie den Namen einer Verwaltungseinheit ändern.

## <a name="update-company-attributes"></a>"Update Unternehmen" Attribute
Attribut                           | Beschreibung
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AllowedDataLocation     | Ein Speicherort, in dem das Unternehmen Benutzer bereitgestellt werden.
AuthorizedServiceInstance   | Namen der Dienstinstanzen, ein Plan bereitgestellt werden kann.
DirSyncEnabled |Gibt an, ob die Synchronisierung eines autorisierenden (Kunden, lokal) Verzeichnis.
DirSyncStatus |Gibt an, ob Adressbuch Objekte Mieter dabei eine autorisierende (Kunden lokal) Synchronisierung Verzeichnis; eine Erweiterung der DirSyncEnabled-Eigenschaft für Unternehmen Objekte.
DirSyncFeatures |Bitflag zum Festlegen der aktivierten und deaktivierten Dirsync-Funktionen für den Mandanten an.
DirectoryFeatures |Aktiviert/deaktiviert Directory-Features.
DirSyncConfiguration |Enthält alle Dirsync-Konfiguration für den aktuellen Mandanten.
DisplayName |Der Anzeigename für ein Objekt
IsMnc |Ein boolesches Flag festlegen auf "true" iff Unternehmen für multinationale Unternehmen-Funktion aktiviert wird.
ObjectSettings | Eine Auflistung von Einstellungen für den Bereich des Objekts.
PartnerCommerceUrl |URL des Partners Commerce-Site.
PartnerHelpUrl |URL der Partner-Hilfeseite.
PartnerSupportEmail | URL des Partners e-Mail.
PartnerSupportTelephone |URL der Partner Support per Telefon.
PartnerSupportUrl |URL der Partner-Support-Website.
StrongAuthenticationDetails |Details zur StrongAuthentication.
StrongAuthenticationPolicy |Starke Authentifizierungsrichtlinie für das Unternehmen.
TechnicalNotificationMail |E-Mail-Adresse Unternehmen technische Fragen zu benachrichtigen.
TelephoneNumber |Telefonnummern, die ITU-Empfehlung E.123 entsprechen.
TenantType |Der Typ eines Mandanten. Wenn dieser Wert nicht angegeben, ist der Mieter eines Unternehmens. Andernfalls mögliche Werte sind: Microsoft-Support (0), SyndicatePartner (1), BreadthPartner (2) BreadthPartnerDelegatedAdmin (3) (4) ResellerPartnerDelegatedAdmin ValueAddedResellerPartnerDelegatedAdmin (5).
VerifiedDomain |Ein Satz von DNS-Domänennamen an ein Unternehmen gebunden.

## <a name="update-domain-attributes"></a>Attribute "Domäne aktualisieren"
Attribut                           | Beschreibung
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
Funktionen    |Bitflags, beschreibt die Funktionen der Domäne, falls vorhanden.
Standard |Gibt an, ob die Domäne der Standardwert ist. z. B. das standardmäßige UserPrincipalName Suffix erstellt ein Administrator einen neuen Benutzer in Verwaltungscenter.
Anfängliche |Gibt an, ob die Domäne die erste Domäne des Unternehmens OCP zugewiesenen. Die erste Domäne ist eine eindeutige Unterdomäne einer Domäne Microsoft Online. e.g.contoso3.microsoftonline.com.
LiveType    |Typ des entsprechenden Windows Live Namespace, falls vorhanden.
Name    | Bezeichner für den Endpunkt.
PasswordNotificationWindowDays  |Die Anzahl der Tage vor dem Kennwort der Benutzer wird benachrichtigt.
PasswordValidityPeriodDays  | Die Anzahl der Tage, denen ein Kennwort gut ist, bevor es geändert werden muss.

Datensätze sind ein erforderliches Steuerelement für viele Vorschriften. Für Kunden mit der Azure Active Directory Prüfbericht zu ihrer Vorschriften sollten der Kunde eine Kopie dieses Hilfethema mit der Kunde exportierte Prüfungsbericht erklären die Berichtsdetails übermitteln. Der Prüfer möchte verständlich Azure derzeit erfüllt die Vorschriften weiter Prüfer zur [Compliance-Seite](https://azure.microsoft.com/support/trust-center/compliance/) der Microsoft Azure Trust Center.
