<properties
   pageTitle="CSV-Dateiformat für Azure Active Directory B2B Zusammenarbeit Vorschau | Microsoft Azure"
   description="Azure Active Directory B2B unterstützt die unternehmensübergreifende Beziehungen Business Partner corporate Anwendung selektiv auf"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-csv-file-format"></a>Azure AD B2B Zusammenarbeit Vorschau: CSV-Dateiformat

Die Vorabversion von Azure AD B2B-Zusammenarbeit erfordert eine CSV-Datei Partner Benutzerinformationen Azure AD-Portal hochgeladen werden. Die CSV-Datei sollte erforderlichen Etiketten unten und optionale Felder nach Bedarf. Ändern Sie Beispiel-CSV-Datei (siehe unten) ohne die Schreibweise der Etiketten in der ersten Zeile.

>[AZURE.NOTE] Die erste Zeile (wie E-Mail, DisplayName, usw.) ist für die CSV-Datei erfolgreich analysiert werden. Die Schreibweise muss die nachstehenden Felder übereinstimmen. Diese Etiketten identifizieren den Inhalt unter ihnen. Für optionale Felder, die erforderlich sind, können ihre aus der CSV-Datei entfernt. Die gesamte Spalte kann leer gelassen werden.

## <a name="required-fields-br"></a>Erforderliche Felder: <br/>
**E-Mail:** E-Mail-Adresse des eingeladenen Benutzers. <br/>
**DisplayName:** Anzeigename für eingeladene Benutzer (normalerweise vor- und Nachname).<br/>


## <a name="optional-fields-br"></a>Optionale Felder: <br/>

**InvitationText:** Nach Anwendung branding und vor Rückzahlung Link Einladung e-Mail-Text anpassen.<br/>
**InvitedToApplications:** AppIDs corporate Anwendung Benutzer zuweisen. AppIDs werden telefonisch in PowerShell aufgerufen`Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId`<br/>
**InvitedToGroups:** Gruppen Benutzer hinzufügen. Sind abrufbar in PowerShell aufrufen`Get-MsolGroup | fl DisplayName, ObjectId`<br/>
**InviteRedirectURL:** URL nach Annahme der Einladung eingeladenen Benutzer angewiesen. Dies sollte eine unternehmensspezifische URL (z. B. [*contoso.my.salesforce.com*](http://contoso.my.salesforce.com/)). Wenn dieses optionale Feld nicht angegeben wird, der eingeladene Benutzer Zugriffsbereich App richtet, in dem sie die ausgewählten Unternehmen apps navigieren können. App Access Panel-URL hat die Form `https://account.activedirectory.windowsazure.com/applications/default.aspx?tenantId=<TenantID>`.<br/>
**CcEmailAddress**: e-Mail-Adresse e-Mail-Einladung zu kopieren. Wenn das CcEmailAddress verwendet wird, kann diese Einladung für Benutzer e-Mail bestätigt oder Mieter erstellen verwendet werden.<br/>
**Sprache:** Sprache für die Einladung e-Mail und Rückzahlung Erfahrung mit "En" (Englisch) standardmäßig nicht angegeben. Die 10 unterstützten Sprache sind:<br/>
1. de: Deutsch<br/>
2. es: Spanisch<br/>
3. FR: Französisch<br/>
4. es: Italienisch<br/>
5. Ja: Japanisch<br/>
6. Ko: Koreanisch<br/>
7. pt-BR: Portugiesisch (Brasilien)<br/>
8. RU: Russisch<br/>
9. Zh-HANS: Chinesisch (vereinfacht)<br/>
10. Zh-HANT: Chinesisch (traditionell)<br/>

## <a name="sample-csv-file"></a>Beispiel-CSV-Datei
Hier ist ein Beispiel CSV ändern können.

>[AZURE.NOTE] Kopieren und in den Editor einzufügen, und mit der Erweiterung ".csv" speichern. Klicken Sie in Excel bearbeiten. Sie werden in einer Tabelle in der ersten Zeile mit strukturiert.

> Fügen Sie weitere optionale Felder in Excel mit der Bezeichnung Auffüllen der Spalte darunter hinzu.

```
Email,DisplayName,InvitationText,InviteRedirectUrl,InvitedToApplications,InvitedToGroups,CcEmailAddress,Language
wharp@contoso.com,Walter Harp,Hi Walter! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en
jsmith@contoso.com,Jeff Smith,Hi Jeff! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en
bsmith@contoso.com,Ben Smith,Hi Ben! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en

```

## <a name="related-articles"></a>Verwandte Artikel
Durchsuchen Sie unsere Artikel Azure AD B2B-Zusammenarbeit

- [Was ist Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Funktionsweise](active-directory-b2b-how-it-works.md)
- [Ausführliche Anleitung](active-directory-b2b-detailed-walkthrough.md)
- [Externe Benutzer token format](active-directory-b2b-references-external-user-token-format.md)
- [Externe Benutzer objektänderungen](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Aktuelle Vorschau Grenzen](active-directory-b2b-current-preview-limitations.md)
- [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)
