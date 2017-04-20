<properties
   pageTitle="Ausführliche Anleitung mit Azure Active Directory B2B Zusammenarbeit Vorschau | Microsoft Azure"
   description="Azure Active Directory B2B-Zusammenarbeit unterstützt die unternehmensübergreifende Beziehungen Business Partner corporate Anwendung selektiv auf"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-detailed-walkthrough"></a>Azure AD B2B Zusammenarbeit Vorschau: ausführliche exemplarische

In dieser exemplarischen Vorgehensweise wird beschrieben, wie Azure AD B2B-Zusammenarbeit verwenden. Als IT-Administrator von Contoso sollen Mitarbeiter drei Partner Programme freigeben. Kein Unternehmen benötigen Azure AD.

- Alice aus einfachen Partner Org
- Bob von Medium Partner Org benötigt Zugriff auf einen Satz von apps
- Carol von komplexen Partner Org benötigt Zugriff auf eine Reihe von apps und Mitgliedschaft in Gruppen von Contoso

Nach einer Partner-Benutzern gesendet werden, können wir sie in Azure AD auf apps und Mitgliedschaft in Gruppen Azure-Portal. Beginnen wir mit Alice hinzufügen.

## <a name="adding-alice-to-the-contoso-directory"></a>Das Contoso-Verzeichnis Alice hinzufügen
1. Erstellen einer CSV-Datei mit den Headern Siehe füllen nur Alice **E-Mail**, **DisplayName**und **InviteContactUsUrl**. **DisplayName** ist in der Einladung angezeigt und Contoso Azure AD-Verzeichnis angezeigt. **InviteContactUsUrl** kann Alice an Contoso. Im folgenden Beispiel gibt InviteContactUsUrl LinkedIn Profil von Contoso. Es ist wichtig, die Etiketten in der ersten Zeile der CSV-Datei genau wie angegeben in der [Übersicht der CSV-Datei](active-directory-b2b-references-csv-file-format.md)zu schreiben.  
![Beispiel-CSV-Datei für Alice](./media/active-directory-b2b-detailed-walkthrough/AliceCSV.png)

2. Benutzer in der Contoso-Verzeichnis im Azure-Portal hinzufügen (Active Directory > Contoso > Benutzer > Benutzer hinzufügen). Wählen Sie im Dropdownfeld "Typ von Benutzer" "Benutzer im Partnerunternehmen". Hochladen Sie die CSV-Datei. Stellen Sie sicher, dass die CSV-Datei vor dem Hochladen abgeschlossen ist.  
![CSV-Datei hochladen für Alice](./media/active-directory-b2b-detailed-walkthrough/AliceUpload.png)

3. Alice wird jetzt als externen Benutzer im Contoso Azure AD-Verzeichnis dargestellt.  
![Alice wird in Azure AD aufgeführt.](./media/active-directory-b2b-detailed-walkthrough/AliceInAD.png)

4. Alice erhält die folgende e-Mail.  
![Einladung für Alice](./media/active-directory-b2b-detailed-walkthrough/AliceEmail.png)

5. Alice klickt auf den Hyperlink, und sie wird aufgefordert, die Einladung anzunehmen und mit ihren eigenen Anmeldeinformationen anmelden. Wenn Alice nicht in Azure AD-Verzeichnis ist, wird Alice Anmelden aufgefordert.  
![Einladung für Alice anmelden](./media/active-directory-b2b-detailed-walkthrough/AliceSignUp.png)

6. Alice wird leer Zugriffsbereich App umgeleitet, bis sie apps zugreifen kann.  
![Abdeckung für Alice](./media/active-directory-b2b-detailed-walkthrough/AliceAccessPanel.png)

Dieses Verfahren ermöglicht die einfachste Form der B2B-Zusammenarbeit. Als Benutzer im Contoso Azure AD-Verzeichnis kann Alice Applikationen und Gruppen über das Azure-Portal zugreifen. Jetzt fügen wir Bob, wer Zugriff auf die Anwendung Moodle und Salesforce.

## <a name="adding-bob-to-the-contoso-directory-and-granting-access-to-apps"></a>Das Contoso-Verzeichnis Bob hinzufügen und Zugriff auf apps
1. Mit Windows PowerShell installierte Anwendung IDs Moodle und Salesforce mit der Azure AD. Die IDs können mithilfe des Cmdlets abgerufen werden: `Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId` wird eine Liste aller verfügbaren Applikationen Contoso und ihre AppPrincialIds.  
![IDs für Bob abrufen](./media/active-directory-b2b-detailed-walkthrough/BobPowerShell.png)

2. Erstellen Sie eine CSV-Datei mit Bob E-Mail und DisplayName, **InviteAppID**, **InviteAppResources**und InviteContactUsUrl. **InviteAppResources** AppPrincipalIds moodle füllen und Vertriebs von PowerShell, getrennt durch ein Leerzeichen gefunden. Füllen Sie **InviteAppId** mit demselben AppPrincipalId Moodle Marke e-Mail und Seiten mit einem Moodle Logo anmelden.  
![Beispiel-CSV-Datei für Bob](./media/active-directory-b2b-detailed-walkthrough/BobCSV.png)

3. Hochladen der CSV-Datei über das Azure-Portal einfach wie für Alice. Ralf ist damit ein externer Benutzer im Contoso Azure AD-Verzeichnis.

4. Bob empfängt die folgende e-Mail.  
![Einladung für Bob](./media/active-directory-b2b-detailed-walkthrough/BobEmail.png)

5. Bob auf den Link klickt und aufgefordert, die Einladung anzunehmen. Nachdem er angemeldet ist, wird er auf der und kann bereits Moodle und Salesforce.  
![Abdeckung für Bob](./media/active-directory-b2b-detailed-walkthrough/BobAccessPanel.png)

Wir werden Carol fügen, die Zugriff auf Programme sowie die Mitgliedschaft in Gruppen im Verzeichnis Contoso.

## <a name="adding-carol-to-the-contoso-directory-granting-access-to-apps-and-giving-group-membership"></a>Carol Contoso-Verzeichnis hinzufügen, Zugriff auf apps und Gruppenmitgliedschaft erteilen

1. Mit Windows PowerShell installierte Anwendung IDs und Gruppen-IDs von Contoso mit der Azure AD.
 - AppPrincipalId mit Cmdlet abrufen `Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId`, wie Bob
 - ObjectId für Gruppen Cmdlet abrufen `Get-MsolGroup | fl DisplayName, ObjectId`. Dadurch wird eine Liste aller Gruppen in Contoso und ihre ObjectIds. Gruppen-IDs können auch als die Objekt-ID der Registerkarte Eigenschaften der Gruppe im Azure-Portal abgerufen werden.  
![IDs und Gruppen für Carol abrufen](./media/active-directory-b2b-detailed-walkthrough/CarolPowerShell.png)

2. Erstellen Sie CSV-Datei Auffüllen Carol E-Mail, DisplayName, InviteAppID, InviteAppResources, **InviteGroupResources**und InviteContactUsUrl. **InviteGroupResources** wird durch der MyGroup1 und externen, getrennt durch ein Leerzeichen aufgefüllt.  
![Beispiel-CSV-Datei für Carol](./media/active-directory-b2b-detailed-walkthrough/CarolCSV.png)

3. Hochladen der CSV-Datei über das Azure-Portal.

4. Carol ist ein Benutzer in der Contoso-Verzeichnis und ist außerdem Mitglied der Gruppen MyGroup1 und externen, wie in der Azure-Portal.  
![Carol ist in einer Gruppe in Azure AD aufgeführt.](./media/active-directory-b2b-detailed-walkthrough/CarolGroup.png)

5. Carol erhält eine e-Mail mit einem Link, um die Einladung anzunehmen. Nachdem sie anmeldet, wird sie die Anwendung Abdeckung auf Moodle und Salesforce umgeleitet.  

Das ist zum Hinzufügen von Benutzern von Partnerunternehmen in Azure AD B2B-Zusammenarbeit gibt. In dieser exemplarischen Vorgehensweise gezeigt, wie Benutzer Alice, Bob und Carol drei separate CSV-Dateien mit Contoso-Verzeichnis hinzufügen. Dieser Prozess kann erleichtert werden durch Kondensation separate CSV-Dateien in einer einzigen Datei.  
![Beispiel-CSV-Datei für Alice, Bob und Carol](./media/active-directory-b2b-detailed-walkthrough/CombinedCSV.png)

## <a name="related-articles"></a>Verwandte Artikel
Durchsuchen Sie unsere Artikel Azure AD B2B-Zusammenarbeit:

- [Was ist Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Funktionsweise](active-directory-b2b-how-it-works.md)
- [Übersicht der CSV-Datei](active-directory-b2b-references-csv-file-format.md)
- [Externe Benutzer token format](active-directory-b2b-references-external-user-token-format.md)
- [Externe Benutzer objektänderungen](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Aktuelle Vorschau Grenzen](active-directory-b2b-current-preview-limitations.md)
- [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)
