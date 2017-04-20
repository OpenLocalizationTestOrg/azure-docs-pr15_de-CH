<properties
    pageTitle="Best Practices: Azure AD Password Management | Microsoft Azure"
    description="Best Practices für Bereitstellung und Verwendung Beispiel Endbenutzerdokumentation und Schulungsunterlagen für Kennwort in Azure Active Directory."
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

# <a name="deploying-password-management-and-training-users-to-use-it"></a>Bereitstellen von Passwort-Management und Schulung der Benutzer

> [AZURE.IMPORTANT] **Sind Sie hier beim Anmelden Probleme auftreten?** In diesem Fall [hier wie ändern und Ihr eigenes Kennwort](active-directory-passwords-update-your-own-password.md).

Nach dem Passwort aktivieren, besteht der nächste Schritt müssen Sie zu Benutzern in Ihrer Organisation nutzen. Dazu müssen Sie sicherstellen, dass Ihre Benutzer konfiguriert den Dienst ordnungsgemäß und auch verwenden, dass die Benutzer die Schulung haben sie ihre eigenen Kennwörter verwalten zu müssen. Dieser Artikel erklärt Ihnen die folgenden Konzepte:

* [**Wie Sie Ihre Benutzer für Kennwort konfiguriert**](#how-to-get-users-configured-for-password-reset)
  * [Was macht ein Konto für das Zurücksetzen von Kennwörtern konfiguriert](#what-makes-an-account-configured)
  * [Wie Sie um Authentifizierungsdaten zu füllen](#ways-to-populate-authentication-data)
* [**Am besten Kennwortrücksetzdiskette für Ihre Organisation bereitstellen**](#what-is-the-best-way-to-roll-out-password-reset-for-users)
  * [E-Mail-basierte Implementierung + Beispiel e-Mail-Kommunikation](#email-based-rollout)
  * [Erstellen Sie Ihre eigenen benutzerdefinierten Kennworts Verwaltungsportal für Benutzer](#creating-your-own-password-portal)
  * [Wie Sie erzwungene Registrierung bei Anmeldung erzwingen](#using-enforced-registration)
  * [Uploaden von Authentifizierungsdaten für Benutzerkonten](#uploading-data-yourself)
* [**Beispielbenutzer und Schulungsmaterialien Support (in Kürze verfügbar!)**](#sample-training-materials)

## <a name="how-to-get-users-configured-for-password-reset"></a>Wie Benutzer für das Zurücksetzen von Kennwörtern konfiguriert
Dieser Abschnitt beschreibt verschiedene Methoden, durch die sichergestellt wird, dass alle Benutzer in Ihrer Organisation benutzerverantwortliche Kennwortrücksetzdiskette effektiv bei vergessen verwenden kann, sollen.

### <a name="what-makes-an-account-configured"></a>Was macht ein Konto konfiguriert
Bevor ein Benutzer das Zurücksetzen von Kennwörtern verwenden kann, müssen **Alle** Folgendes erfüllt sein:

1.  Zurücksetzen des Kennworts muss im Verzeichnis aktiviert.  Aktivieren Sie Kennwortrücksetzdiskette lesen [können Benutzer ihre Azure AD Kennwörter zurücksetzen](active-directory-passwords-getting-started.md#enable-users-to-reset-their-azure-ad-passwords) oder [Benutzer zurücksetzen oder Ändern ihrer Kennwörter AD](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords)
2.  Der Benutzer muss lizenziert sein.
 - Cloud-Benutzer müssen **bezahlten Office 365 Lizenz**oder **AAD Basic** oder zugewiesene **Lizenz AAD** .
 - Auf Prem Benutzer (federated oder Hash synchronisiert), Benutzer **benötigen eine AAD Premium-Lizenz zugewiesen**.
3.  Der Benutzer muss die **Mindestanzahl von Authentifizierungsdaten definiert** gemäß dem aktuellen Kennwort zurücksetzen.
 - Authentifizierungsdaten gilt definiert, wenn das entsprechende Feld im Verzeichnis wohlgeformte Daten enthält.
 - Ein minimalen Satz von Authentifizierungsdaten wird auf **mindestens eine** der aktivierten Authentifizierungsoptionen ein Tor Richtlinie konfiguriert ist und **mindestens zwei** aktivierte Authentifizierungsoptionen definiert, wenn zwei Gate-Richtlinie konfiguriert ist.
4.  Wenn der Benutzer ein lokales Konto verwenden, muss dann [Kennwort Rückschreiben](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords) aktiviert und eingeschaltet

### <a name="ways-to-populate-authentication-data"></a>Methoden zum Auffüllen von Authentifizierungsdaten
Sie haben mehrere Optionen zum Angeben von Daten für Benutzer in Ihrer Organisation für das Zurücksetzen von Kennwörtern verwendet werden.

- Benutzer in [Azure-Verwaltungsportal](https://manage.windowsazure.com) oder dem [Verwaltungsportal von Office 365](https://portal.microsoftonline.com)
- Mit Benutzereigenschaften in Azure AD aus lokalen Active Directory-Domäne synchronisiert werden Azure AD synchronisieren
- Verwenden Sie Windows PowerShell anhand [der Schritte](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users)Benutzereigenschaften bearbeiten.
- Benutzer können ihre eigenen Daten führen sie die registrierungsportal unter [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup) registrieren
- Benutzer für das Zurücksetzen ihrer Azure AD-Konto mit der Anmeldung bei der [**müssen Benutzer beim anmelden registrieren?**](active-directory-passwords-customize.md#require-users-to-register-when-signing-in) Konfigurationsoption auf **Ja**.

Benutzer müssen Kennwort zurücksetzen für das System nicht registrieren.  Beispielsweise haben Sie vorhandene Mobile oder Office Telefonnummern in Ihrem lokalen Verzeichnis in Azure AD synchronisieren und wir verwenden sie Kennwort automatisch zurückgesetzt.

Sie können auch über [Zurücksetzen wie Daten von Kennwort verwendet werden](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset) und [wie Sie einzelne Authentifizierung Felder mit PowerShell füllen](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users)lesen.

## <a name="what-is-the-best-way-to-roll-out-password-reset-for-users"></a>Was ist am besten Kennwortrücksetzdiskette für Benutzer bereitstellen?
Es folgen die allgemeine Einführung Schritte zum Zurücksetzen des Kennworts:

1.  Aktivieren Sie Kennwort zurücksetzen im Verzeichnis auf die Registerkarte **Konfigurieren** der [Azure-Verwaltungsportal](https://manage.windowsazure.com) und **Ja** für die **Benutzer aktiviert für Passwort** -Option.
2.  Jeder Benutzer, möchte an Kennwortrücksetzdiskette in, die entsprechenden Lizenzen zuweisen der auf der Registerkarte **Lizenzen** in [Azure-Verwaltungsportal](https://manage.windowsazure.com).
3.  Optional schränken Sie Kennwort zurücksetzen auf eine Gruppe von Benutzern, Rollen sich langsam Zeit durch **Einschränken des Zugriffs auf Passwort** Umschalten auf **Yes** und eine Sicherheitsgruppe für das Zurücksetzen von Kennwörtern (Beachten Sie, dass diese Benutzer alle Ihnen zugewiesene Lizenzen müssen) aktivieren ein.
4.  Weisen Sie die Benutzer Kennwort zurücksetzen oder sie per e-Mail beim Registrieren, aktivieren erzwungen Registrierung der Abdeckung und die entsprechenden Authentifizierungsdaten für Anwender über Dirsync-PowerShell und [Azure-Verwaltungsportal](https://manage.windowsazure.com)hochladen.  Einzelheiten hierzu werden weiter unten.
5.  Überprüfen Sie im Laufe der Zeit Benutzer navigieren zur Registerkarte "Berichte" und anzeigen den Aktivitätsbericht [**Kennwort zurücksetzen Registrierung**](active-directory-passwords-get-insights.md#view-password-reset-registration-activity) registrieren.
6.  Wenn zahlreiche Benutzer registriert haben, sehen sie das Kennwort zurücksetzen, indem Sie auf die Registerkarte Berichte navigieren und [**Kennwort zurücksetzen**](active-directory-passwords-get-insights.md#view-password-reset-activity) Aktivitätsbericht anzeigen.

Es gibt verschiedene Benutzer mitteilen, dass sie für registrieren und Kennwortrücksetzdiskette in Ihrer Organisation verwenden können.  Sie werden nachfolgend beschrieben.

### <a name="email-based-rollout"></a>E-Mail-basierte Bereitstellung
Per e-Mail sie dazu angewiesen ist am einfachsten den Benutzern mitteilen zu registrieren oder Kennwort zurücksetzen.  Unten ist eine Vorlage, die Sie hierzu verwenden können.  Gerne Farben ersetzen / Logos mit eigener Auswahl an Ihre Bedürfnisse anzupassen.

  ![][001]

Sie können [hier die e-Mail-Vorlage herunterladen](http://1drv.ms/1xWFtQM).

### <a name="creating-your-own-password-portal"></a>Kennwort-Portal erstellen
Eine Strategie eignet sich für größere Kunden Kennwort-Management-Funktionen ist die Erstellung einer "Kennwort Portal" Benutzer verwalten alles verwendbaren in Bezug auf ihre Kennwörter an einem einzigen Ort.  

Viele unserer größten Kunden möchten einen Stamm-DNS-Eintrag https://passwords.contoso.com mit Links zu Azure AD-Kennwort zurücksetzen Portal, Passwort registrierungsportal und Passwort Seiten erstellen.  Auf diese Weise e-Mails oder Flugblätter, senden Sie einem Erinnerungswert URL gehören, denen Benutzer bei einer Sekunde Einstieg in den Dienst zu navigieren.

Um hier benötigen wir eine einfache Seite mit neuesten reaktionsfähigen Benutzeroberfläche entwerfen Paradigmen erstellt haben und für alle Browser und mobile Geräte funktioniert.

  ![][007]

Sie können [die Website-Vorlage herunterladen](https://github.com/kenhoff/password-reset-page).  Wir empfehlen das Logo und Farben zu Ihrer Organisation anpassen.

### <a name="using-enforced-registration"></a>Erzwungene Registrierung verwenden
Sollen die Benutzer Kennwortrücksetzdiskette selbst registrieren können Sie auch zwingen, registrieren, wenn die Abdeckung am [http://myapps.microsoft.com](http://myapps.microsoft.com)anmelden.  Sie können diese Option Registerkarte **Konfigurieren** des Verzeichnisses durch Aktivieren der Option **Benötigen Benutzer bei der Anmeldung der Registrierung** .  

Optional können Sie definieren, ob sie aufgefordert werden, nach einem konfigurierbaren Zeitraum registrieren die Option **Tage bevor Benutzer ihre Kontaktdaten bestätigen müssen** ein NULL Wert ändern. Weitere Informationen finden Sie unter [Anpassen von Passwort Management Benutzerverhalten](active-directory-passwords-customize.md#password-management-behavior) .

  ![][002]

Nachdem Sie diese Option aktivieren, wenn Benutzer die Abdeckung anmelden, sehen sie ein Popup-Fenster, das sie darüber informiert, dass ihr Administrator sie Kontaktdaten überprüfen muss. Sie können ihr Kennwort zurücksetzen, wenn sie jemals auf ihr Konto zugreifen.

  ![][003]

**Jetzt überprüfen** auf bringt, das **Passwort registrierungsportal** zu [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup) und registriert sind.  Registrierung über diese Methode kann durch Klicken auf die Schaltfläche **Abbrechen** oder schließen Sie das Fenster geschlossen, aber zur Erinnerung: jedes Mal, wenn sie sich anmelden, wenn sie sich nicht registrieren.

  ![][004]

### <a name="uploading-data-yourself"></a>Hochladen von Daten selbst
Authentifizierungsdaten selbst hochladen möchten, müssen Benutzer nicht für das Zurücksetzen, damit Sie ihre Kennwörter zurücksetzen registrieren.  Als Benutzer auf ihr Konto, die das Kennwort erfüllt definierten Authentifizierungsdaten reset Richtlinie definierten und die Benutzer ihre Kennwörter zurücksetzen werden.

Welche Eigenschaften AAD verbinden oder festlegen Windows PowerShell finden Sie unter [Daten durch das Zurücksetzen von Kennwörtern verwendet](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset).

Durch die folgenden Schritte können Sie die Authentifizierungsdaten [Azure-Verwaltungsportal](https://manage.windowsazure.com) hochladen:

1.  Navigieren Sie zum Verzeichnis der **Active Directory-Erweiterung** in [Azure-Verwaltungsportal](https://manage.windowsazure.com).
2.  Klicken Sie auf die Registerkarte **Benutzer** .
3.  Wählen Sie aus der Liste interessiert sind.
4.  Auf der ersten Registerkarte finden Sie **Alternative e-Mail-Adresse**, die als Eigenschaft ermöglicht das Zurücksetzen von Kennwörtern verwendet werden können.

    ![][005]

5.  Klicken Sie auf der Registerkarte **Info arbeiten** .
6.  Auf dieser Seite finden Sie **Telefon**, **Mobiltelefon**, **Telefon Authentifizierung**und **Authentifizierung E-Mail**.  Diese Eigenschaften können auch festgelegt werden, Benutzer, das Kennwort zurückzusetzen.

    ![][006]

Finden Sie unter [welche Daten durch Zurücksetzen des Kennworts](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset) wie jede dieser Eigenschaften verwendet werden kann.

Anzeigen Sie können gelesen und diese Daten mit PowerShell [auf Kennwort zurücksetzen Daten für Benutzer von PowerShell](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users)

## <a name="sample-training-materials"></a>Schulungsmaterialien
Wir arbeiten an Schulungsmaterialien, mit denen Sie schnell Ihre IT-Organisation und Ihre Benutzer schnell bereitstellen und das Zurücksetzen von Kennwörtern verwenden.  Bleiben Sie dran!


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>Links zu Kennwort zurücksetzen Dokumentation
Unten finden Sie Links zu allen Dokumentationsseiten Azure AD Kennwort zurücksetzen:

* **Sind Sie hier beim Anmelden Probleme auftreten?** In diesem Fall [hier wie ändern und Ihr eigenes Kennwort](active-directory-passwords-update-your-own-password.md).
* [**Funktionsweise**](active-directory-passwords-how-it-works.md) - erfahren Sie mehr über die sechs verschiedenen Komponenten des Dienstes und jeder
* [**Erste Schritte**](active-directory-passwords-getting-started.md) - erfahren Sie, wie Sie Benutzern zurücksetzen und ihre Cloud oder lokalen Kennwörter ändern
* [**Anpassen**](active-directory-passwords-customize.md) - Informationen zum Anpassen von Aussehen und Verhalten und Verhalten des Diensts zu Ihrem Unternehmen
* Sie [**erfahren**](active-directory-passwords-get-insights.md) – erfahren Sie mehr über unsere integrierten reporting-Funktionen
* [**FAQ**](active-directory-passwords-faq.md) - Antworten auf häufig gestellte Fragen
* [**Problembehandlung**](active-directory-passwords-troubleshoot.md) – erfahren Sie schnell Probleme mit dem Dienst
* [**Erfahren Sie mehr**](active-directory-passwords-learn-more.md) - gehen tief in die technischen Einzelheiten der Funktionsweise des Dienstes



[001]: ./media/active-directory-passwords-best-practices/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-best-practices/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-best-practices/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-best-practices/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-best-practices/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-best-practices/006.jpg "Image_006.jpg"
[007]: ./media/active-directory-passwords-best-practices/007.jpg "Image_007.jpg"
