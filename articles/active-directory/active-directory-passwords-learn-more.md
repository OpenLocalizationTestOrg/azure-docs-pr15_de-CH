<properties
    pageTitle="Weitere Informationen: Azure AD Password Management | Microsoft Azure"
    description="Weiterführende Themen zum Azure AD Password Management, einschließlich Kennwort Rückschreiben Funktionsweise Rückschreiben Kennwortschutz, wie Passwort Portal arbeiten und Daten durch Zurücksetzen des Kennworts verwendet werden."
    services="active-directory"
    documentationCenter=""
    authors="asteen"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="asteen"/>

# <a name="learn-more-about-password-management"></a>Erfahren Sie mehr über Kennwort-Management

> [AZURE.IMPORTANT] **Sind Sie hier beim Anmelden Probleme auftreten?** In diesem Fall [hier wie ändern und Ihr eigenes Kennwort](active-directory-passwords-update-your-own-password.md).

Wenn Sie Password Management bereits bereitgestellt haben, oder einfach weitere technische Sache an der Funktionsweise vor der Bereitstellung dieser Abschnitt Sie einen guten Überblick über die technischen Konzepte des Dienstes geben. Wir behandeln Folgendes:

* [**Kennwort Rückschreiben – Übersicht**](#password-writeback-overview)
  - [Wie funktioniert das Rückschreiben Passwort](#how-password-writeback-works)
  - [Szenarien für das Rückschreiben Kennwort unterstützt](#scenarios-supported-for-password-writeback)
  - [Kennwort Rückschreiben Sicherheitsmodell](#password-writeback-security-model)
* [**Wie Passwort das Portal Arbeit?**](#how-does-the-password-reset-portal-work)
  - [Welche Daten durch das Zurücksetzen von Kennwörtern verwendet werden?](#what-data-is-used-by-password-reset)
  - [Daten für Benutzer zurücksetzen auf Kennwort](#how-to-access-password-reset-data-for-your-users)

## <a name="password-writeback-overview"></a>Kennwort Rückschreiben – Übersicht
Kennwort Rückschreiben ist eine [Azure Active Directory verbinden](active-directory-aadconnect.md) -Komponente, die aktiviert und aktuellen Azure Active Directory Premium-Abonnenten verwendet werden. Weitere Informationen finden Sie unter [Azure Active Directory Editionen](active-directory-editions.md).

Rückschreiben Kennwort können Sie die Cloud Mieter schreiben Sie Kennwörter für lokale Active Directory konfigurieren.  Es entfällt Sie einrichten und Verwalten einer komplizierten lokalen Self-service-Kennwort zurücksetzen Lösung und es bequem cloudbasierten Benutzer ihre lokalen Kennwörter zurückgesetzt werden.  Hier sind einige der wichtigsten Features von Kennwort Rückschreiben:

- **Keine Verzögerung Feedback.**  Kennwort Rückschreiben ist eine synchrone Operation.  Die Benutzer werden sofort benachrichtigt, wenn ihr Kennwort nicht Richtlinie oder konnte nicht zurückgesetzt oder geändert werden.
- **Zurücksetzen von Kennwörtern für Benutzer mit AD FS oder andere Föderation unterstützt.**  Mit Kennwort Rückschreiben als verbundenen Benutzerkonten in Azure AD-Mandanten synchronisiert werden sie werden zu ihrer lokalen AD Kennwörter aus der Cloud.
- **Zurücksetzen von Kennwörtern für Benutzer mit Kennwort-Hash-Sync unterstützt.** Kennwort zurücksetzen Dienst erkennt, dass eine synchronisierte Benutzerkonto Kennwort Hash Synchronisierung aktiviert ist, setzen wir dieses Konto lokal und Cloud Kennwort gleichzeitig.
- **Ändern von Kennwörtern von Abdeckung und Office 365 unterstützt.**  Wenn verbundene oder Kennwort Sync Benutzer sind abgelaufen oder nicht abgelaufen geändert, wir schreiben diese Kennwörter wieder auf der AD-Umgebung.
- **Unterstützt Zurückschreiben von Kennwörter beim Administrator sie Zurücksetzen der** [**Azure-Verwaltungsportal**](https://manage.windowsazure.com).  Wenn ein Administrator zurückgesetzt Benutzerkennwort in [Azure-Verwaltungsportal](https://manage.windowsazure.com), wenn Benutzer verbunden ist oder Kennwort synchronisieren möchten, legen wir das Kennwort wählt der Administrator auf Ihrem lokalen Anzeige sowie.  Dies ist derzeit nicht im Verwaltungsportal von Office unterstützt.
- **Erzwingt die lokalen AD Kennwortrichtlinien.**  Wenn ein Benutzer sein Kennwort zurücksetzt, wir sicherstellen, dass Ihre lokalen erfüllt AD-Richtlinie vor Aufnahme in dieses Verzeichnis.  Dazu gehören Verlauf, Komplexität, Alter, Kennwortfilter und andere kennworteinschränkungen definierten lokalen AD.
- **Nicht erforderlich alle eingehenden Firewall-Regeln.**  Kennwort Rückschreiben verwendet ein Azure Service Bus Relay als eine zugrunde liegende Kommunikationskanal bedeutet, dass Sie keine eingehenden Ports in der Firewall für diese Funktion zu öffnen.
- **Wird nicht für Benutzerkonten unterstützt, die geschützten Gruppen in der lokalen Active Directory vorhanden sind.** Weitere Informationen zu geschützten Gruppen finden Sie unter [geschützten Konten und Gruppen in Active Directory](https://technet.microsoft.com/library/dn535499.aspx).

### <a name="how-password-writeback-works"></a>Wie funktioniert das Rückschreiben Kennwort
Rückschreiben Kennwort besteht aus drei Hauptkomponenten:

- Kennwort zurücksetzen Cloud-Dienst (Dies ist in Azure AD Kennwort ändern Seiten integriert)
- Mandantenspezifische Azure Service Bus relay
- Passwort Prem Endpunkt

Sie zusammenpassen gemäß dem folgenden Diagramm:

  ![][001]

Beim Synchronisieren von Föderations- oder Kennwort-Hash wird Benutzer kommt zurücksetzen oder ändern das Kennwort in der Cloud Folgendes:

1.  Wir überprüfen, welche Art von Kennwort der Benutzer verfügt.  Wenn wir, dass das Kennwort lokal verwaltet wird sehen, dann stellen wir sicher, dass der Rückschreiben-Dienst ausgeführt wird.  Es ist wir den Formularausfüller fortzufahren, wenn nicht wir dem Benutzer mitteilen, die ihr Kennwort hier kann nicht zurückgesetzt werden.
2.  Anschließend Benutzer übergibt der entsprechenden Authentifizierung Gates und der Bildschirm Kennwort zurücksetzen erreicht.
3.  Der Benutzer wählt ein neues Kennwort und bestätigt.
4.  Beim Klicken auf Senden verschlüsseln wir das Klartextkennwort mit einem symmetrischen Schlüssel beim Rückschreiben Setup erstellt wurde.
5.  Nach dem Verschlüsseln des Kennworts enthalten wir es in eine Nutzlast, die über einen HTTPS-Kanal Ihr Mieter bestimmten Service Bus Relay gesendet wird (das wir auch während des Installationsvorgangs Rückschreiben eingerichtet).  Dieses ist ein zufällig generiertes Kennwort geschützt, die nur die lokale Installation kann.
6.  Erreicht die Nachricht Dienstbus reaktiviert Kennwort zurücksetzen Endpunkt automatisch und erkennt, dass eine Zurücksetzen der Anforderung ausstehend ist.
7.  Der Dienst sucht nach Benutzer betreffenden Ankerattribut Cloud mit.  Diese Suche erfolgreich Benutzerobjekts bestehen im AD-Connectorspace mit entsprechenden AAD Connectorobjekt verknüpft sein und dem entsprechenden Metaverse-Objekt verknüpft sein. Schließlich in der Reihenfolge für die Synchronisierung dieses Benutzerkonto zu der Link AD Connectorobjekt MV müssen Synchronisierungsregel `Microsoft.InfromADUserAccountEnabled.xxx` auf den Link.  Dies ist erforderlich, da geht der Aufruf aus der Cloud, das Synchronisierungsmodul das CloudAnchor-Attribut zum Suchen des AAD Connectorspace-Objekts verwendet, dann die Verknüpfung auf das MV-Objekt folgt und folgt dann den Link zum AD-Objekt. Da mehrere AD-Objekte (Gesamtstrukturen) für denselben Benutzer möglicherweise, das Synchronisierungsmodul basiert auf der `Microsoft.InfromADUserAccountEnabled.xxx` Link korrekte zu wählen.
8.  Das Benutzerkonto gefunden, versucht, direkt in die entsprechenden AD-Gesamtstruktur Passwort.
9.  Wenn Kennwort festlegen wird, sagen wir den Benutzer sein Kennwort geändert wurde und dass sie können Frohe unterwegs.
10. Kennworts fehl werden wir den Fehler an den Benutzer zurück und wiederholen lassen.  Der Vorgang fehlschlagen, da war, da das Kennwort ausgewählt Unternehmensrichtlinien, weil wir nicht den Benutzer im lokalen AD nicht entspricht oder aus.  Wir haben eine bestimmte Nachricht für viele dieser Fälle und dem Benutzer mitteilen, was sie tun können, um das Problem zu beheben.

### <a name="scenarios-supported-for-password-writeback"></a>Szenarien für das Rückschreiben Kennwort unterstützt
Die Tabelle unten beschreibt die Szenarien für die Versionen unserer Synchronisierungsfunktionen unterstützt werden.  Im Allgemeinen wird empfohlen, die neueste Version von [Azure AD Connect](active-directory-aadconnect.md#install-azure-ad-connect) installieren, wenn Sie Kennwort Rückschreiben verwenden möchten.

  ![][002]

### <a name="password-writeback-security-model"></a>Kennwort Rückschreiben Sicherheitsmodell
Kennwort Rückschreiben ist ein sehr sicherer und robuster Dienst.  Um sicherzustellen, dass Ihre Informationen geschützt sind, ermöglichen wir 4 mehrstufige Sicherheitsmodell, die nachfolgend beschrieben.

- **Mieter bestimmten Service Bus Relay** – beim Einrichten der Dienst eingerichtet, die nach dem Zufallsprinzip generierten Kennwort geschützt sind, die Microsoft nicht auf mandantenspezifische Service Bus Relay.
- **Locked down kryptografisch starken Verschlüsselungsschlüssel** – nachdem Service Bus Relay erstellt wurde, erstellen wir einen starken symmetrischen Schlüssel verwenden wir das Kennwort verschlüsselt über das Netzwerk stammt.  Dieser Schlüssel befindet sich nur in Ihrem Unternehmen geheim in der Cloud stark gesperrt und überwacht, wie jedes Kennwort im Verzeichnis.
- **Industry standard TLS** -Kennwort zurücksetzen oder Änderung in der Cloud auftritt, wir nehmen das Klartextkennwort und mit Ihrem öffentlichen Schlüssel zu verschlüsseln.  Wir plop, die dann in eine HTTPS-Nachricht über einen verschlüsselten Kanal mit Microsoft SSL-Zertifikate Ihre Service Bus Relay gesendet.  Nach Eingang der Nachricht in Service Bus auf Prem Agents reaktiviert, authentifiziert Service Bus die Verwendung sicherer Kennwörter, der zuvor erstellt wurden, nimmt die verschlüsselte Nachricht, entschlüsselt ihn mit den privaten Schlüssel, den wir erstellt und versucht, das Kennwort über die AD DS-SetPassword-API.  Dieser Schritt ist in die Cloud uns AD-Prem Kennwortrichtlinie (Komplexität, Alter, Geschichte, Filter etc.) erzwingen können.
- **Richtlinien zum Kennwortablauf** – schließlich Wenn aus irgendeinem Grund die Nachricht in Service Bus befindet, da die auf Prem Dienst es Timeout und nach einigen Minuten entfernt, um die Sicherheit weiter zu erhöhen.

## <a name="how-does-the-password-reset-portal-work"></a>Wie Passwort das Portal Arbeit?
Navigiert zum Zurücksetzen des Kennworts Portal, ein Workflow wird bestimmt, ob das Benutzerkonto gültig ist startete welche Organisation Benutzer gehört, wo Kennwort des Benutzers verwaltet wird und ob der Benutzer die Funktion lizenziert ist.  So lernen die Logik der Seite lesen.

1.  Benutzer klickt auf das nicht auf Ihr Konto oder geht direkt auf [https://passwordreset.microsoftonline.com](https://passwordreset.microsoftonline.com)zugreifen.
2.  Benutzer gibt eine Benutzer-Id und eine Captcha übergibt.
3.  Azure AD überprüft, wenn der Benutzer diese Funktion wie folgt verwenden:
    - Überprüft, dass der Benutzer diese Funktion aktiviert ist und eine Azure AD-Lizenz zugewiesen.
        - Verfügt der Benutzer nicht diese aktiviert oder eine Lizenz zugeordnet, wird der Benutzer aufgefordert, seine Administrator, um das Kennwort zurückzusetzen.
    - Überprüft, dass der Benutzer das Recht Herausforderung Daten auf Konto Administrator Richtlinie definiert.
        - Richtlinie nur eine Herausforderung benötigt, ist sichergestellt, dass der Benutzer die entsprechenden Daten für mindestens eine der aktivierten Richtlinie Administrator definiert.
          - Wenn der Benutzer nicht konfiguriert ist, empfiehlt der Benutzer seine Administrator, um das Kennwort zurückzusetzen.
        - Die Richtlinie zwei Probleme benötigt, wird sichergestellt, dass der Benutzer die entsprechenden Daten für mindestens zwei der Administratorrichtlinie aktivierten definiert.
          - Wenn der Benutzer nicht konfiguriert, wir des Benutzers aufgefordert, seine Administrator, um das Kennwort zurückzusetzen.
    - Überprüft, ob das Kennwort des Benutzers lokal (federated oder Kennwort-Hash Sync hatte) verwaltet wird.
       - Rückschreiben bereitgestellt wird, und das Kennwort des Benutzers lokal verwaltet wird, darf der Benutzer zu authentifizieren und das Kennwort zurücksetzen.
       - Rückschreiben nicht bereitgestellt wird, und das Kennwort des Benutzers lokal verwaltet wird, wird der Benutzer aufgefordert, um seine Administrator, um das Kennwort zurückzusetzen.
4.  Wenn festgestellt wird, dass der Benutzer erfolgreich das Kennwort zurücksetzen, wird der Benutzer durch den Vorgang geführt.

Weitere Informationen zum Rückschreiben auf Kennwort bereitstellen [Erste Schritte: Azure AD Password Management](active-directory-passwords-getting-started.md).

### <a name="what-data-is-used-by-password-reset"></a>Welche Daten durch das Zurücksetzen von Kennwörtern verwendet werden?
Die folgende Tabelle zeigt, wo und wie diese Daten Zurücksetzen des Kennworts verwendet und soll Ihnen helfen zu entscheiden, welche Optionen für Ihre Organisation geeignet sind. Diese Tabelle zeigt auch Formatierungen Vorschriften für Fälle, in denen Sie Daten für Benutzer von Eingabepfade bereitstellen, die diese Daten nicht überprüfen.

> [AZURE.NOTE] Telefon (Büro) erscheint nicht im registrierungsportal, weil Benutzer derzeit nicht diese Eigenschaft im Verzeichnis bearbeiten können.

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Kontaktmethode Name</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Azure Active Directory Datenelement</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Verwendet / einstellbare wo?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Format-Vorschriften</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Telefon (Büro)</p>
            </td>
            <td>
              <p>Telefonnummer</p>
              <p>z. B. Set-MsolUser - UserPrincipalName JWarner@contoso.com - Telefonnummer "1234567890 + 1 x 1234"</p>
            </td>
            <td>
              <p>Verwendet:</p>
              <p>Kennwort zurücksetzen Portal</p>
              <p>Einstellbare aus:</p>
              <p>Telefonnummer ist einstellbar von PowerShell, DirSync Azure-Verwaltungsportal und dem Verwaltungsportal von Office</p>
            </td>
            <td>
              <p>+ ccc Xxxyyyzzzz (z.B. 1234567890 + 1)</p>
              <ul>
                <li class="unordered">
Eine Landeskennzahl ist erforderlich<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Eine erforderlich (gegebenenfalls)<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Muss ein + vor der jeweiligen Code bereitgestellt haben<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Muss ein Leerzeichen zwischen Ländercode und der Rest der Nummer<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Extensions werden nicht unterstützt, haben alle Erweiterungen angegeben wir entfernt es aus der Zahl vor dem Versand den Telefonanruf.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Mobiltelefon</p>
            </td>
            <td>
              <p>AuthenticationPhone</p>
              <p>ODER</p>
              <p>Kontaktdetails</p>
              <p>(Authentifizierung Telefon verwendet, wenn vorhanden Daten, wird andernfalls das Feld Mobiltelefon).</p>
              <p>z. B. Set-MsolUser - UserPrincipalName JWarner@contoso.com - Handy "1234567890 + 1 x 1234"</p>
            </td>
            <td>
              <p>Verwendet:</p>
              <p>Kennwort zurücksetzen Portal</p>
              <p>Registrierungsportal</p>
              <p>Einstellbare aus: </p>
              <p>AuthenticationPhone kann vom Kennwort zurücksetzen registrierungsportal oder MFA registrierungsportal festgelegt werden.</p>
              <p>Handy ist einstellbar von PowerShell, DirSync Azure-Verwaltungsportal und dem Verwaltungsportal von Office</p>
            </td>
            <td>
              <p>+ ccc Xxxyyyzzzz (z.B. 1234567890 + 1)</p>
              <ul>
                <li class="unordered">
Eine Landeskennzahl ist erforderlich.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Geben eine (falls zutreffend).<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Müssen haben ein + vor der jeweiligen Code bereitstellen.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Muss ein Leerzeichen zwischen Ländercode und der Rest der Nummer.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Extensions werden nicht unterstützt, haben alle Erweiterungen angegeben wir ignorieren bei den Telefonanruf.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Alternative e-Mail-Adresse</p>
            </td>
            <td>
              <p>AuthenticationEmail</p>
              <p>ODER</p>
              <p>AlternateEmailAddresses [0] </p>
              <p>(Authentifizierung E-Mail verwendet wird, wenn vorhanden Daten, wird andernfalls das Feld alternative e-Mail-Adresse).</p>
              <p>Hinweis: das Feld alternative e-Mail wird als ein Array von Zeichenfolgen im Verzeichnis angegeben.  Wir verwenden den ersten Eintrag in diesem Array.</p>
              <p>z. B. Set-MsolUser - UserPrincipalName JWarner@contoso.com - AlternateEmailAddresses"email@live.com"</p>
            </td>
            <td>
              <p>Verwendet:</p>
              <p>Kennwort zurücksetzen Portal</p>
              <p>Registrierungsportal</p>
              <p>Einstellbare aus: </p>
              <p>AuthenticationEmail kann vom Kennwort zurücksetzen registrierungsportal oder MFA registrierungsportal festgelegt werden.</p>
              <p>AlternateEmail ist einstellbar von PowerShell Azure-Verwaltungsportal und dem Verwaltungsportal von Office</p>
            </td>
            <td>
              <p>
                <a href="mailto:user@domain.com">user@domain.com</a>oder甲斐@黒川.日本</p>
              <ul>
                <li class="unordered">
Standardformate wie befolgen e-Mails.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Unicode-e-Mails werden unterstützt.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Fragen und Antworten</p>
            </td>
            <td>
              <p>Direkt in das Verzeichnis ändern nicht verfügbar.</p>
            </td>
            <td>
              <p>Verwendet:</p>
              <p>Kennwort zurücksetzen Portal</p>
              <p>Registrierungsportal </p>
              <p>Einstellbare aus: </p>
              <p>Nur geht Sicherheit Fragen über den Azure-Verwaltungsportal.</p>
              <p>Über das Portal-Registrierung ist die einzige Möglichkeit, Antworten auf Sicherheitsfragen für einen bestimmten Benutzer.</p>
            </td>
            <td>
              <p>Sicherheitsfragen sind maximal 200 Zeichen und min 3 Zeichen</p>
              <p>Antworten können maximal 40 Zeichen und min 3 Zeichen</p>
            </td>
          </tr>
        </tbody></table>

###<a name="how-to-access-password-reset-data-for-your-users"></a>Daten für Benutzer zurücksetzen auf Kennwort
####<a name="data-settable-via-synchronization"></a>Daten über Synchronisierung festgelegt werden
Die folgenden Felder können lokal synchronisiert werden:

* Mobiltelefon
* Telefon (Büro)

####<a name="data-settable-with-azure-ad-powershell"></a>Daten mit Azure AD PowerShell festgelegt werden
Die folgenden Felder sind mit Azure AD PowerShell und Graph-API:

* Alternative e-Mail-Adresse
* Mobiltelefon
* Telefon (Büro)
* Authentifizierung Telefon
* Authentifizierung von E-Mail

####<a name="data-settable-with-registration-ui-only"></a>Konfigurierbare Benutzeroberfläche Registrierung nur Daten
Die folgenden Felder sind nur über die Registrierung SSPR UI (https://aka.ms/ssprsetup):

* Fragen und Antworten

####<a name="what-happens-when-a-user-registers"></a>Was geschieht, wenn ein Benutzer registriert?
Wenn Benutzer registriert Registrierungsseite wird **immer** Satz folgende Felder:

* Authentifizierung Telefon
* Authentifizierung von E-Mail
* Fragen und Antworten

Wenn eingegebene Wert für **Mobiltelefon** oder **Andere e-Mail-**können Benutzer sofort die ihre Kennwörter zurücksetzen, wenn sie für den Dienst registriert haben.  Darüber hinaus Benutzer sehen diese Werte beim ersten Mal registrieren, und ändern, wenn sie möchten.  Nachdem sie erfolgreich registriert haben, werden diese Werte im Bereich **Authentifizierung Telefon** und **E-Mail-Authentifizierung** bzw. beibehalten.

Dies ist sinnvoll für zahlreiche Benutzer mit SSPR gleichzeitig Benutzer diese Informationen über die Registrierung aufheben.

####<a name="setting-password-reset-data-with-powershell"></a>Festlegen des Kennworts zurücksetzen mit PowerShell
Sie können Werte für die folgenden Felder mit Azure AD PowerShell festlegen.

* Alternative e-Mail-Adresse
* Mobiltelefon
* Telefon (Büro)

Zunächst müssen Sie zuerst [herunterladen](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule)und Installieren von Azure AD PowerShell-Modul.  Nachdem Sie es installiert haben, können Sie so konfigurieren Sie jedes Feld folgen

#####<a name="alternate-email"></a>Alternative e-Mail-Adresse
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
```

#####<a name="mobile-phone"></a>Mobiltelefon
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
```

#####<a name="office-phone"></a>Telefon (Büro)
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"
```

####<a name="reading-password-reset-data-with-powershell"></a>Beim Lesen des Kennworts zurücksetzen mit PowerShell
Sie können Werte für die folgenden Felder mit Azure AD PowerShell lesen.

* Alternative e-Mail-Adresse
* Mobiltelefon
* Telefon (Büro)
* Authentifizierung Telefon
* Authentifizierung von E-Mail

Zunächst müssen Sie zuerst [herunterladen](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule)und Installieren von Azure AD PowerShell-Modul.  Nachdem Sie es installiert haben, können Sie so konfigurieren Sie jedes Feld folgen

#####<a name="alternate-email"></a>Alternative e-Mail-Adresse
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
```

#####<a name="mobile-phone"></a>Mobiltelefon
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
```

#####<a name="office-phone"></a>Telefon (Büro)
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber
```

#####<a name="authentication-phone"></a>Authentifizierung Telefon
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
```

#####<a name="authentication-email"></a>Authentifizierung von E-Mail
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

## <a name="links-to-password-reset-documentation"></a>Links zu Kennwort zurücksetzen Dokumentation
Unten finden Sie Links zu allen Dokumentationsseiten Azure AD Kennwort zurücksetzen:

* **Sind Sie hier beim Anmelden Probleme auftreten?** In diesem Fall [hier wie ändern und Ihr eigenes Kennwort](active-directory-passwords-update-your-own-password.md).
* [**Funktionsweise**](active-directory-passwords-how-it-works.md) - erfahren Sie mehr über die sechs verschiedenen Komponenten des Dienstes und jeder
* [**Erste Schritte**](active-directory-passwords-getting-started.md) - erfahren Sie, wie Sie Benutzern zurücksetzen und ihre Cloud oder lokalen Kennwörter ändern
* [**Anpassen**](active-directory-passwords-customize.md) - Informationen zum Anpassen von Aussehen und Verhalten und Verhalten des Diensts zu Ihrem Unternehmen
* [**Best Practices**](active-directory-passwords-best-practices.md) - so schnell bereitstellen und Kennwörter in Ihrer Organisation verwalten
* Sie [**erfahren**](active-directory-passwords-get-insights.md) – erfahren Sie mehr über unsere integrierten reporting-Funktionen
* [**FAQ**](active-directory-passwords-faq.md) - Antworten auf häufig gestellte Fragen
* [**Problembehandlung**](active-directory-passwords-troubleshoot.md) – erfahren Sie schnell Probleme mit dem Dienst



[001]: ./media/active-directory-passwords-learn-more/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-learn-more/002.jpg "Image_002.jpg"
