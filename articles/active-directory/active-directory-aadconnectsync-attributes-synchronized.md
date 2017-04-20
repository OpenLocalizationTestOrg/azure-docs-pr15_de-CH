<properties
    pageTitle="Azure AD Verbindung synchronisieren: Attribute in Azure Active Directory synchronisiert | Microsoft Azure"
    description="Listet die Attribute, die in Azure Active Directory synchronisiert werden."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="markvi;andkjell"/>


# <a name="azure-ad-connect-sync-attributes-synchronized-to-azure-active-directory"></a>Azure AD Verbindung synchronisieren: Attribute mit Azure Active Directory synchronisiert.
In diesem Thema werden die Attribute aufgeführt, die von Azure AD Connect Sync synchronisiert werden.  
Die Attribute gruppiert verwandte Azure AD app.

## <a name="attributes-to-synchronize"></a>Attribute zum Synchronisieren
Eine häufige gestellte Frage ist *die Liste der Mindestanforderungen erfüllt synchronisieren*. Standardmäßige und empfohlene Vorgehensweise ist die Standardattribute zu, damit eine vollständige GAL (globale Adressliste) in der Cloud erstellt werden kann und alle Funktionen in Office 365-Workloads. In einigen Fällen gibt es einige Attribute, der Ihre Organisation keine mit der Cloud synchronisiert diese Attribute enthalten vertrauliche oder personenbezogene Informationen (persönliche Daten), wie in diesem Beispiel:  
![Ungültige Attribute](./media/active-directory-aadconnectsync-attributes-synchronized/badextensionattribute.png)

In diesem Fall mit der Liste der Attribute in diesem Thema beginnen und die Attribute, die vertrauliche oder Personenbezogene Daten enthält und nicht synchronisiert werden. Deaktivieren Sie diese Attribute während der Installation [Azure AD app](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering)und Attribute filtern.

>[AZURE.WARNING] Beim Deaktivieren von Attributen sollten mit Vorsicht und nur deaktivieren diese Attribute nicht möglich, zu synchronisieren. Andere Attribute Auswahl möglicherweise negativ auf Funktionen.

## <a name="office-365-proplus"></a>Office 365 ProPlus

| Attributname| Benutzer| Kommentar |
| --- | :-: | --- |
| accountEnabled| X| Bestimmt, ob ein Konto aktiviert ist.|
| CN| X|  |
| displayName| X|  |
| objectSID| X| Eigenschaft. AD-Benutzer-ID für die Synchronisierung zwischen Azure verwendet Active Directory und Active Directory.|
| pwdLastSet| X| Eigenschaft. Wenn bereits ausgestellte Token ungültig wissen verwendet. Von Kennwort synchronisieren und Föderation verwendet.|
| sourceAnchor| X| Eigenschaft. Unveränderlicher Bezeichner Beziehung zwischen fügt Azure.|
| usageLocation| X| Eigenschaft. Land des Benutzers. Lizenz-Zuordnung verwendet.|
| userPrincipalName| X| Benutzerprinzipalname ist der Benutzername des Benutzers. Am häufigsten als [Mail] denselben Wert.|

## <a name="exchange-online"></a>Exchange Online

| Attributname| Benutzer| Kontakt| Gruppe| Kommentar |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Bestimmt, ob ein Konto aktiviert ist.|
| Assistent| X| X|  |  |
| authOrig| X| X| X|  |
| c| X| X|  |  |
| CN| X|  | X|  |
| Co| X| X|  |  |
| Unternehmen| X| X|  |  |
| Ländercode| X| X|  |  |
| Abteilung| X| X|  |  |
| Beschreibung| X| X| X|  |
| displayName| X| X| X|  |
| dLMemRejectPerms| X| X| X|  |
| dLMemSubmitPerms| X| X| X|  |
| extensionAttribute1| X| X| X|  |
| extensionAttribute10| X| X| X|  |
| extensionAttribute11| X| X| X|  |
| extensionAttribute12| X| X| X|  |
| extensionAttribute13| X| X| X|  |
| extensionAttribute14| X| X| X|  |
| extensionAttribute15| X| X| X|  |
| extensionAttribute2| X| X| X|  |
| extensionAttribute3| X| X| X|  |
| extensionAttribute4| X| X| X|  |
| extensionAttribute5| X| X| X|  |
| extensionAttribute6| X| X| X|  |
| extensionAttribute7| X| X| X|  |
| extensionAttribute8| X| X| X|  |
| extensionAttribute9| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| Vorname| X| X|  |  |
| homePhone| X| X|  |  |
| Info| X| X| X| Dieses Attribut wird derzeit nicht für Gruppen verwendet.|
| Initialen| X| X|  |  |
| l| X| X|  |  |
| legacyExchangeDN| X| X| X|  |
| mailNickname| X| X| X|  |
| mangedBy|  |  | X|  |
| Manager| X| X|  |  |
| Member|  |  | X|  |
| Mobile| X| X|  |  |
| MsDS-HABSeniorityIndex| X| X| X|  |
| MsDS-PhoneticDisplayName| X| X| X|  |
| msExchArchiveGUID| X|  |  |  |
| msExchArchiveName| X|  |  |  |
| msExchAssistantName| X| X|  |  |
| msExchAuditAdmin| X|  |  |  |
| msExchAuditDelegate| X|  |  |  |
| msExchAuditDelegateAdmin| X|  |  |  |
| msExchAuditOwner| X|  |  |  |
| msExchBlockedSendersHash| X| X|  |  |
| msExchBypassAudit| X|  |  |  |
| msExchCoManagedByLink|  |  | X|  |
| msExchDelegateListLink| X|  |  |  |
| msExchELCExpirySuspensionEnd| X|  |  |  |
| msExchELCExpirySuspensionStart| X|  |  |  |
| msExchELCMailboxFlags| X|  |  |  |
| msExchEnableModeration| X|  | X|  |
| msExchExtensionCustomAttribute1| X| X| X| Dieses Attribut wird derzeit nicht von Exchange Online genutzt. |
| msExchExtensionCustomAttribute2| X| X| X| Dieses Attribut wird derzeit nicht von Exchange Online genutzt. |
| msExchExtensionCustomAttribute3| X| X| X| Dieses Attribut wird derzeit nicht von Exchange Online genutzt. |
| msExchExtensionCustomAttribute4| X| X| X| Dieses Attribut wird derzeit nicht von Exchange Online genutzt. |
| msExchExtensionCustomAttribute5| X| X| X| Dieses Attribut wird derzeit nicht von Exchange Online genutzt. |
| msExchHideFromAddressLists| X| X| X|  |
| msExchImmutableID| X|  |  |  |
| msExchLitigationHoldDate| X| X| X|  |
| msExchLitigationHoldOwner| X| X| X|  |
| msExchMailboxAuditEnable| X|  |  |  |
| msExchMailboxAuditLogAgeLimit| X|  |  |  |
| msExchMailboxGuid| X|  |  |  |
| msExchModeratedByLink| X| X| X|  |
| msExchModerationFlags| X| X| X|  |
| msExchRecipientDisplayType| X| X| X|  |
| Dermsexchrecipienttypedetails| X| X| X|  |
| msExchRemoteRecipientType| X|  |  |  |
| msExchRequireAuthToSendTo| X| X| X|  |
| msExchResourceCapacity| X|  |  |  |
| msExchResourceDisplay| X|  |  |  |
| msExchResourceMetaData| X|  |  |  |
| msExchResourceSearchProperties| X|  |  |  |
| msExchRetentionComment| X| X| X|  |
| msExchRetentionURL| X| X| X|  |
| msExchSafeRecipientsHash| X| X|  |  |
| msExchSafeSendersHash| X| X|  |  |
| msExchSenderHintTranslations| X| X| X|  |
| msExchTeamMailboxExpiration| X|  |  |  |
| msExchTeamMailboxOwners| X|  |  |  |
| msExchTeamMailboxSharePointUrl| X|  |  |  |
| msExchUserHoldPolicies| X|  |  |  |
| MsOrg IsOrganizational|  |  | X|  |
| objectSID| X|  | X| Eigenschaft. AD-Benutzer-ID für die Synchronisierung zwischen Azure verwendet Active Directory und Active Directory.|
| oOFReplyToOriginator|  |  | X|  |
| otherFacsimileTelephone| X| X|  |  |
| otherHomePhone| X| X|  |  |
| otherTelephone| X| X|  |  |
| Pager| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| Postleitzahl| X| X|  |  |
| proxyAddresses| X| X| X|  |
| publicDelegates| X| X| X|  |
| pwdLastSet| X|  |  | Eigenschaft. Wenn bereits ausgestellte Token ungültig wissen verwendet. Von Kennwort synchronisieren und Föderation verwendet.|
| reportToOriginator|  |  | X|  |
| reportToOwner|  |  | X|  |
| securityEnabled|  |  | X| GroupType abgeleitet|
| Sn| X| X|  |  |
| sourceAnchor| X| X| X| Eigenschaft. Unveränderlicher Bezeichner Beziehung zwischen fügt Azure.|
| St| X| X|  |  |
| streetAddress| X| X|  |  |
| targetAddress| X| X|  |  |
| telephoneAssistant| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailPhoto| X| X|  |  |
| Titel| X| X|  |  |
| unauthOrig| X| X| X|  |
| usageLocation| X|  |  | Eigenschaft. Land des Benutzers. Lizenz-Zuordnung verwendet.|
| userCertificate| X| X|  |  |
| userPrincipalName| X|  |  | Benutzerprinzipalname ist der Benutzername des Benutzers. Am häufigsten als [Mail] denselben Wert.|
| userSMIMECertificates| X| X|  |  |
| wWWHomePage| X| X|  |  |

## <a name="sharepoint-online"></a>SharePoint Online

| Attributname| Benutzer| Kontakt| Gruppe| Kommentar |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Bestimmt, ob ein Konto aktiviert ist.|
| authOrig| X| X| X|  |
| c| X| X|  |  |
| CN| X|  | X|  |
| Co| X| X|  |  |
| Unternehmen| X| X|  |  |
| Ländercode| X| X|  |  |
| Abteilung| X| X|  |  |
| Beschreibung| X| X| X|  |
| displayName| X| X| X|  |
| dLMemRejectPerms| X| X| X|  |
| dLMemSubmitPerms| X| X| X|  |
| extensionAttribute1| X| X| X|  |
| extensionAttribute10| X| X| X|  |
| extensionAttribute11| X| X| X|  |
| extensionAttribute12| X| X| X|  |
| extensionAttribute13| X| X| X|  |
| extensionAttribute14| X| X| X|  |
| extensionAttribute15| X| X| X|  |
| extensionAttribute2| X| X| X|  |
| extensionAttribute3| X| X| X|  |
| extensionAttribute4| X| X| X|  |
| extensionAttribute5| X| X| X|  |
| extensionAttribute6| X| X| X|  |
| extensionAttribute7| X| X| X|  |
| extensionAttribute8| X| X| X|  |
| extensionAttribute9| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| Vorname| X| X|  |  |
| hideDLMembership|  |  | X|  |
| HomePhone| X| X|  |  |
| Info| X| X| X|  |
| Initialen| X| X|  |  |
| "ipPhone"| X| X|  |  |
| l| X| X|  |  |
| e-Mail-Nachrichten| X| X| X|  |
| mailNickname| X| X| X|  |
| managedBy|  |  | X|  |
| Manager| X| X|  |  |
| Member|  |  | X|  |
| middleName| X| X|  |  |
| Mobile| X| X|  |  |
| msExchTeamMailboxExpiration| X|  |  |  |
| msExchTeamMailboxOwners| X|  |  |  |
| msExchTeamMailboxSharePointLinkedBy| X|  |  |  |
| msExchTeamMailboxSharePointUrl| X|  |  |  |
| objectSID| X|  | X| Eigenschaft. AD-Benutzer-ID für die Synchronisierung zwischen Azure verwendet Active Directory und Active Directory.|
| oOFReplyToOriginator|  |  | X|  |
| otherFacsimileTelephone| X| X|  |  |
| otherHomePhone| X| X|  |  |
| otherIpPhone| X| X|  |  |
| otherMobile| X| X|  |  |
| otherPager| X| X|  |  |
| otherTelephone| X| X|  |  |
| Pager| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| Postleitzahl| X| X|  |  |
| postOfficeBox| X| X|  |  |
| preferredLanguage| X|  |  |  |
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | Eigenschaft. Wenn bereits ausgestellte Token ungültig wissen verwendet. Von Kennwort synchronisieren und Föderation verwendet.|
| reportToOriginator|  |  | X|  |
| reportToOwner|  |  | X|  |
| securityEnabled|  |  | X| GroupType abgeleitet|
| Sn| X| X|  |  |
| sourceAnchor| X| X| X| Eigenschaft. Unveränderlicher Bezeichner Beziehung zwischen fügt Azure.|
| St| X| X|  |  |
| streetAddress| X| X|  |  |
| targetAddress| X| X|  |  |
| telephoneAssistant| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailPhoto| X| X|  |  |
| Titel| X| X|  |  |
| unauthOrig| X| X| X|  |
| URL| X| X|  |  |
| usageLocation| X|  |  | Eigenschaft. Land des Benutzers. Lizenz-Zuordnung verwendet.|
| userPrincipalName| X|  |  | Benutzerprinzipalname ist der Benutzername des Benutzers. Am häufigsten als [Mail] denselben Wert.|
| wWWHomePage| X| X|  |  |

## <a name="lync-online"></a>Lync Online

| Attributname| Benutzer| Kontakt| Gruppe| Kommentar |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Bestimmt, ob ein Konto aktiviert ist.|
| c| X| X|  |  |
| CN| X|  | X|  |
| Co| X| X|  |  |
| Unternehmen| X| X|  |  |
| Abteilung| X| X|  |  |
| Beschreibung| X| X| X|  |
| displayName| X| X| X|  |
| facsimiletelephonenumber| X| X| X|  |
| Vorname| X| X|  |  |
| HomePhone| X| X|  |  |
| "ipPhone"| X| X|  |  |
| l| X| X|  |  |
| e-Mail-Nachrichten| X| X| X|  |
| mailNickname| X| X| X|  |
| managedBy|  |  | X|  |
| Manager| X| X|  |  |
| Member|  |  | X|  |
| Mobile| X| X|  |  |
| msExchHideFromAddressLists| X| X| X|  |
| MsRTCSIP-ApplicationOptions| X|  |  |  |
| MsRTCSIP-DeploymentLocator| X| X|  |  |
| MsRTCSIP-Line| X| X|  |  |
| MsRTCSIP-OptionFlags| X| X|  |  |
| MsRTCSIP-OwnerUrn| X|  |  |  |
| MsRTCSIP-PrimaryUserAddress| X| X|  |  |
| MsRTCSIP-UserEnabled| X| X|  |  |
| objectSID| X|  | X| Eigenschaft. AD-Benutzer-ID für die Synchronisierung zwischen Azure verwendet Active Directory und Active Directory.|
| otherTelephone| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| Postleitzahl| X| X|  |  |
| preferredLanguage| X|  |  |  |
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | Eigenschaft. Wenn bereits ausgestellte Token ungültig wissen verwendet. Von Kennwort synchronisieren und Föderation verwendet.|
| securityEnabled|  |  | X| GroupType abgeleitet|
| Sn| X| X|  |  |
| sourceAnchor| X| X| X| Eigenschaft. Unveränderlicher Bezeichner Beziehung zwischen fügt Azure.|
| St| X| X|  |  |
| streetAddress| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailPhoto| X| X|  |  |
| Titel| X| X|  |  |
| usageLocation| X|  |  | Eigenschaft. Land des Benutzers. Lizenz-Zuordnung verwendet.|
| userPrincipalName| X|  |  | Benutzerprinzipalname ist der Benutzername des Benutzers. Am häufigsten als [Mail] denselben Wert.|
| wWWHomePage| X| X|  |  |

## <a name="azure-rms"></a>Azure RMS

| Attributname| Benutzer| Kontakt| Gruppe| Kommentar |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Bestimmt, ob ein Konto aktiviert ist.|
| CN| X|  | X| Namen oder Alias. Häufig das Präfix [Mail] Wert.|
| displayName| X| X| X| Eine Zeichenfolge mit dem Namen häufig als angezeigter Name (Vorname Nachname) angezeigt.|
| e-Mail-Nachrichten| X| X| X| vollständige e-Mail-Adresse.|
| Member|  |  | X|  |
| objectSID| X|  | X| Eigenschaft. AD-Benutzer-ID für die Synchronisierung zwischen Azure verwendet Active Directory und Active Directory.|
| proxyAddresses| X| X| X| Eigenschaft. Von Azure AD verwendet. Enthält alle e-Mail-Adressen für den Benutzer.|
| pwdLastSet| X|  |  | Eigenschaft. Wenn bereits ausgestellte Token ungültig wissen verwendet.|
| securityEnabled|  |  | X| GroupType abgeleitet.|
| sourceAnchor| X| X| X| Eigenschaft. Unveränderlicher Bezeichner Beziehung zwischen fügt Azure.|
| usageLocation| X|  |  | Eigenschaft. Land des Benutzers. Lizenz-Zuordnung verwendet.|
| userPrincipalName| X|  |  | Diese Benutzerprinzipalname ist der Benutzername des Benutzers. Am häufigsten als [Mail] denselben Wert.|

## <a name="intune"></a>Windows Intune

| Attributname| Benutzer| Kontakt| Gruppe| Kommentar |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Bestimmt, ob ein Konto aktiviert ist.|
| c| X| X|  |  |
| CN| X|  | X|  |
| Beschreibung| X| X| X|  |
| displayName| X| X| X|  |
| e-Mail-Nachrichten| X| X| X|  |
| mailNickname| X| X| X|  |
| Member|  |  | X|  |
| objectSID| X|  | X| Eigenschaft. AD-Benutzer-ID für die Synchronisierung zwischen Azure verwendet Active Directory und Active Directory.|
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | Eigenschaft. Wenn bereits ausgestellte Token ungültig wissen verwendet. Von Kennwort synchronisieren und Föderation verwendet.|
| securityEnabled|  |  | X| GroupType abgeleitet|
| sourceAnchor| X| X| X| Eigenschaft. Unveränderlicher Bezeichner Beziehung zwischen fügt Azure.|
| usageLocation| X|  |  | Eigenschaft. Land des Benutzers. Lizenz-Zuordnung verwendet.|
| userPrincipalName| X|  |  | Benutzerprinzipalname ist der Benutzername des Benutzers. Am häufigsten als [Mail] denselben Wert.|

## <a name="dynamics-crm"></a>Dynamics CRM

| Attributname| Benutzer| Kontakt| Gruppe| Kommentar |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Bestimmt, ob ein Konto aktiviert ist.|
| c| X| X|  |  |
| CN| X|  | X|  |
| Co| X| X|  |  |
| Unternehmen| X| X|  |  |
| Ländercode| X| X|  |  |
| Beschreibung| X| X| X|  |
| displayName| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| Vorname| X| X|  |  |
| l| X| X|  |  |
| managedBy|  |  | X|  |
| Manager| X| X|  |  |
| Member|  |  | X|  |
| Mobile| X| X|  |  |
| objectSID| X|  | X| Eigenschaft. AD-Benutzer-ID für die Synchronisierung zwischen Azure verwendet Active Directory und Active Directory.|
| physicalDeliveryOfficeName| X| X|  |  |
| Postleitzahl| X| X|  |  |
| preferredLanguage| X|  |  |  |
| pwdLastSet| X|  |  | Eigenschaft. Wenn bereits ausgestellte Token ungültig wissen verwendet. Von Kennwort synchronisieren und Föderation verwendet.|
| securityEnabled|  |  | X| GroupType abgeleitet|
| Sn| X| X|  |  |
| sourceAnchor| X| X| X| Eigenschaft. Unveränderlicher Bezeichner Beziehung zwischen fügt Azure.|
| St| X| X|  |  |
| streetAddress| X| X|  |  |
| telephoneNumber| X| X|  |  |
| Titel| X| X|  |  |
| usageLocation| X|  |  | Eigenschaft. Land des Benutzers. Lizenz-Zuordnung verwendet.|
| userPrincipalName| X|  |  | Benutzerprinzipalname ist der Benutzername des Benutzers. Am häufigsten als [Mail] denselben Wert.|

## <a name="3rd-party-applications"></a>3rd Party applications
Diese Gruppe ist als minimale Attribute für eine generische Arbeitslast oder Anwendung erforderlichen Attribute. Es kann für eine Arbeitslast in einem anderen Abschnitt nicht aufgelistet oder nicht-Microsoft-Anwendung verwendet werden. Explizit für Folgendes verwendet:

- Yammer (nur Benutzer belegt)
- [Hybride Business to Business (B2B) Cross-Org Szenarien für die Zusammenarbeit von Ressourcen wie SharePoint angeboten](http://go.microsoft.com/fwlink/?LinkId=747036)

Diese Gruppe ist eine Gruppe von Attributen verwendet werden können, wenn Azure AD-Verzeichnis nicht zur Unterstützung von Office 365, Dynamics oder Intune. Es ist eine kleine Gruppe von Kernattributen.

| Attributname| Benutzer| Kontakt| Gruppe| Kommentar |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Bestimmt, ob ein Konto aktiviert ist.|
| CN| X|  | X|  |
| displayName| X| X| X|  |
| Vorname| X| X|  |  |
| e-Mail-Nachrichten| X|  | X|  |
| managedBy|  |  | X|  |
| mailNickName| X| X| X|  |
| Member|  |  | X|  |
| objectSID| X|  |  | Eigenschaft. AD-Benutzer-ID für die Synchronisierung zwischen Azure verwendet Active Directory und Active Directory.|
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | Eigenschaft. Wenn bereits ausgestellte Token ungültig wissen verwendet. Von Kennwort synchronisieren und Föderation verwendet.|
| Sn| X| X|  |  |
| sourceAnchor| X| X| X| Eigenschaft. Unveränderlicher Bezeichner Beziehung zwischen fügt Azure.|
| usageLocation| X|  |  | Eigenschaft. Land des Benutzers. Lizenz-Zuordnung verwendet.|
| userPrincipalName| X|  |  | Benutzerprinzipalname ist der Benutzername des Benutzers. Am häufigsten als [Mail] denselben Wert.|

## <a name="windows-10"></a>Windows 10
Eine Domäne Windows 10 computer(device) synchronisiert einige Attribute in Azure AD. Weitere Informationen zu Szenarien finden Sie unter [Verbinden Domäne Geräte Azure AD für Windows 10 auftritt](active-directory-azureadjoin-devices-group-policy.md). Diese Attribute immer synchronisieren und Windows 10 erscheint nicht als Anwendung, die Sie aufheben können. Mit dem Attribut UserCertificate aufgefüllt wird Windows 10 Domäne beigetretenen Computer identifiziert.

| Attributname| Gerät| Kommentar |
| --- | :-: | --- |
| accountEnabled| X| |
| deviceTrustType| X| Wert für Domänencomputern hartcodiert. |
| displayName | X| |
| ms-DS-CreatorSID | X| Auch als RegisteredOwnerReference bezeichnet.|
| objectGUID | X| Die Abkürzung DeviceID.|
| objectSID | X| Auch als OnPremisesSecurityIdentifier bezeichnet.|
| Betriebssystem | X| Auch als DeviceOSType bezeichnet.|
| operatingSystemVersion | X| Auch als DeviceOSVersion bezeichnet.|
| userCertificate | X| |

Diese Attribute für **Benutzer** werden neben anderen apps, die Sie ausgewählt haben.  

| Attributname| Benutzer| Kommentar |
| --- | :-: | --- |
| domainFQDN| X| Die Abkürzung DnsDomänenName. Z. B. contoso.com.|
| domainNetBios| X| Die Abkürzung NetBiosName. Z. B. CONTOSO.|

## <a name="exchange-hybrid-writeback"></a>Exchange Hybrid Rückschreiben
Diese Attribute sind zurückgeschrieben von Azure AD lokale Active Directory beim **Exchange-Hybrid**aktivieren auswählen. Je nach der Version Exchange möglicherweise weniger Attribute synchronisiert werden.

| Attributname| Benutzer| Kontakt| Gruppe| Kommentar |
| --- | :-: | :-: | :-: | --- |
| MsDS-ExternalDirectoryObjectID| X|  |  | CloudAnchor in Azure AD abgeleitet. Dieses Attribut ist neu in Exchange 2016.|
| msExchArchiveStatus| X|  |  | Online-Archiv: Können Kunden e-Mail-Nachrichten archivieren.|
| msExchBlockedSendersHash| X|  |  | Filter: Schreibt zurück Filterung vor Ort und online sichere und blockierte absenderdaten von Clients.|
| msExchSafeRecipientsHash| X|  |  | Filter: Schreibt zurück Filterung vor Ort und online sichere und blockierte absenderdaten von Clients.|
| msExchSafeSendersHash| X|  |  | Filter: Schreibt zurück Filterung vor Ort und online sichere und blockierte absenderdaten von Clients.|
| msExchUCVoiceMailSettings| X|  |  | Unified Messaging (UM) - Online Voicemail aktivieren: verwendet von Microsoft Lync Server Integration Lync Server an lokalen hat der Benutzer Voicemail in online-Dienste.|
| msExchUserHoldPolicies| X|  |  | Litigation Hold: Unter rechtlich Clouddienste ermöglicht zu bestimmen, welche Benutzer sind.|
| proxyAddresses| X| X| X| Nur die X500 von Exchange Online-Adresse eingefügt.|

## <a name="device-writeback"></a>Gerät Rückschreiben
Geräteobjekte werden in Active Directory erstellt. Diese Objekte können Geräte Azure AD hinzugefügt werden oder Windows 10 Domänencomputern.

| Attributname| Gerät| Kommentar |
| --- | :-: | --- |
| altSecurityIdentities | X| |
| displayName | X| |
| DN | X| |
| MsDS-CloudAnchor | X| |
| MsDS DeviceID | X| |
| MsDS-DeviceObjectVersion | X| |
| MsDS-DeviceOSType | X| |
| MsDS-DeviceOSVersion | X| |
| MsDS-DevicePhysicalIDs | X| |
| MsDS-KeyCredentialLink | X| Nur mit Windows Server 2016 AD-schema |
| MsDS-IsCompliant | X| |
| MsDS IsEnabled | X| |
| MsDS-IsManaged | X| |
| MsDS-RegisteredOwner | X| |


## <a name="notes"></a>Notizen

- Bei Verwendung einer alternativen ID wird lokal Attribut UserPrincipalName mit Azure AD-Attribut OnPremisesUserPrincipalName synchronisiert. Alternative ID-Attribut für Beispiel Mail wird mit Azure AD-Attribut UserPrincipalName synchronisiert.
- In der Liste oben gilt der Objekttyp **User** Object Type **iNetOrgPerson**.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über die Konfiguration [Azure AD-Verbindung synchronisieren](active-directory-aadconnectsync-whatis.md) .

Erfahren Sie mehr über [die lokalen Identitäten Azure Active Directory integrieren](active-directory-aadconnect.md).
