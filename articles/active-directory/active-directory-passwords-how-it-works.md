<properties
    pageTitle="Funktionsweise: Azure AD Password Management | Microsoft Azure"
    description="Erfahren Sie mehr über die verschiedenen Komponenten von Azure AD Password Management, einschließlich dem Benutzer registrieren, zurücksetzen und ihre Kennwörter ändern und wo Administratoren konfigurieren melden und ermöglichen die Verwaltung von lokalen Active Directory-Kennwörter."
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

# <a name="how-password-management-works"></a>Funktionsweise von Kennwort-Management

> [AZURE.IMPORTANT] **Sind Sie hier beim Anmelden Probleme auftreten?** In diesem Fall [hier wie ändern und Ihr eigenes Kennwort](active-directory-passwords-update-your-own-password.md).

Kennwort-Management in Azure Active Directory besteht aus mehreren logischen Komponenten die nachfolgend beschriebenen.  Klicken Sie auf jede Komponente erfahren.

- [**Kennwort-Konfiguration Verwaltungsportal**](#password-management-configuration-portal) – Administratoren können steuern, wie Kennwörter in ihren Mandanten erfolgt durch Navigieren zur Registerkarte Konfigurieren des Verzeichnisses der [Azure-Verwaltungsportal](https://manage.windowsazure.com)Facetten.
- [**User Registration Portal**](#user-registration-portal) -Benutzer können Kennwort zurücksetzen über das Webportal anmelden.
- [**Benutzer Kennwort zurücksetzen Portal**](#user-password-reset-portal) -Benutzer können ihre eigenen Kennwörter mit einer Reihe von verschiedenen Problemen gemäß der vom Administrator gesteuerte Kennwort zurücksetzen zurücksetzen
- [**Benutzer Kennwort ändern Portal**](#user-password-change-portal) -Benutzer können ihre eigenen Kennwörter jederzeit ändern, indem alte Passwort und ein neues Kennwort Nutzung dieses Webportals auswählen
- [**Kennwort-Verwaltungsberichten**](#password-management-reports) – Administratoren können anzeigen und Analysieren von Kennwort zurücksetzen und Registrierung Aktivität in ihren Mandanten navigieren Sie zum Abschnitt "Berichte" des Verzeichnisses "Berichte" auf den [Azure-Verwaltungsportal](https://manage.windowsazure.com)
- [**Kennwort Rückschreiben Komponente Azure AD Connect**](#password-writeback-component-of-azure-ad-connect) – Administratoren können optional das Kennwort Rückschreiben Feature beim Installieren von Azure AD Connect zum Management von federated oder Kennwort Kennwörter aus der Cloud synchronisiert.

## <a name="password-management-configuration-portal"></a>Kennwort-Konfiguration Verwaltungsportal
Kennwort-Management-Policies für ein bestimmtes Verzeichnis mithilfe der [Azure-Verwaltungsportal](https://manage.windowsazure.com) zu Abschnitt **Kennwort zurücksetzen Benutzerrichtlinie** Teilnehmerverzeichnis **Konfigurieren** können.  Über diese Seite können Sie viele Aspekte der Verwaltung von Kennwörtern in der Organisation steuern einschließlich:

- Aktivieren und Deaktivieren von Kennwortrücksetzdiskette für alle Benutzer in einem Verzeichnis
- Festlegen der Anzahl von Problemen (ein oder zwei) Benutzer durchlaufen muss, um das Kennwort zurückzusetzen
- Einstellen der bestimmte Probleme für Benutzer in Ihrer Organisation die folgenden Auswahlmöglichkeiten soll:
 - Mobiltelefon (einen Bestätigungscode per SMS oder ein Anruf)
 - Telefon (Anruf)
 - Alternative E-Mail (einen Bestätigungscode per e-Mail)
 - Sicherheitsfragen (wissensbasierte Authentifizierung)
- Festlegen der Anzahl der Fragen Benutzer registrieren Nutzung Fragen Sicherheitsauthentifizierungsmethode (nur sichtbar, wenn Fragen aktiviert sind)
- Festlegen der Anzahl der Fragen Benutzer muss zurücksetzen mit Sicherheit Fragen Authentifizierungsmethode (nur sichtbar, wenn Fragen aktiviert sind) liefern
- Mit vorerstellte, lokalisierte, Sicherheitsfragen, die ein Benutzer auswählen kann, Anmeldung Kennwort zurücksetzen (nur sichtbar, wenn Fragen aktiviert sind)
- Definieren von benutzerdefinierten Sicherheitsfragen, die ein Benutzer auswählen kann, Anmeldung Kennwort zurücksetzen (nur sichtbar, wenn Fragen aktiviert sind)
- Die Registrierung für das Zurücksetzen auf die Anwendung Abdeckung am [http://myapps.microsoft.com](http://myapps.microsoft.com).
- Die Benutzer bestätigen die zuvor erfassten Daten nach einer konfigurierbaren Anzahl Tage vergangen (nur sichtbar, wenn die erzwungene Registrierung aktiviert ist)
- Bereitstellen einer benutzerdefinierten Helpdesk e-Mail oder URL, die Benutzern angezeigt, wenn sie ein Problem beim Zurücksetzen ihrer Kennwörter haben
- Aktivieren oder deaktivieren die Funktion Rückschreiben Kennwort (wenn Kennwort Rückschreiben mit AAD Verbindung bereitgestellt)
- Anzeigen des Status des Agents Kennwort Rückschreiben (Kennwort Rückschreiben mit AAD Verbindung bereitgestellt)
- E-Mail-Benachrichtigung an Benutzer ihr eigenes Kennwort zurückgesetzt wurde (Abschnitt **Benachrichtigungen** [Azure-Verwaltungsportal](https://manage.windowsazure.com)gefunden) aktivieren
- Aktivierung per e-Mail benachrichtigt Administratoren anderer Administratoren eigene Kennwörter (zu finden im Abschnitt **Benachrichtigung** [Azure-Verwaltungsportal](https://manage.windowsazure.com) zurücksetzen
- Das Benutzerkennwort Branding Portal und Kennwort zurückgesetzt e-Mails mit Logo und Name Ihres Unternehmens mit branding Anpassungsfunktion (Abschnitt **Verzeichniseigenschaften** [Azure-Verwaltungsportal](https://manage.windowsazure.com) Mieter

Weitere Informationen zur Konfiguration von Passwort-Management in Ihrer Organisation finden Sie unter [Erste Schritte: Azure AD Password Management](active-directory-passwords-getting-started.md).

##<a name="user-registration-portal"></a>User Registration Portal
Bevor Benutzer mit Kennwort zurücksetzen können, müssen ihre Cloud-Benutzerkonten mit den richtigen Authentifizierungsdaten um sicherzustellen, dass die entsprechende Anzahl von vom Administrator definierte Kennwort zurücksetzen Probleme passieren kann aktualisiert werden.  Administratoren können auch Authentifizierungsinformationen im Auftrag des Benutzers mit definieren die Azure oder Office Web-Portale, DirSync / Azure AD verbinden und Windows PowerShell.

Aber wenn Sie lieber Ihre Benutzer ihre eigenen Daten haben, bieten wir auch eine Webseite, der Benutzer auf diese Informationen zugreifen können.  Auf dieser Seite können Benutzer angeben, nach dem Kennwort Authentifizierungsinformationen Richtlinien zurückgesetzt, die in ihrer Organisation aktiviert wurden.  Diese Daten überprüft werden, wird es in die Cloud Benutzerkonto für kontowiederherstellung zu einem späteren Zeitpunkt verwendet werden gespeichert. Sieht aus wie die Registrierung Portal aussieht:

  ![][001]

Weitere Informationen finden Sie unter [Erste Schritte: Azure AD Password Management](active-directory-passwords-getting-started.md) und [Best Practices: Azure AD Password Management](active-directory-passwords-best-practices.md).

##<a name="user-password-reset-portal"></a>Benutzer Kennwort zurücksetzen Portal
Wenn Sie aktiviert Self-service-Kennwort zurücksetzen Self-service-Kennwort zurücksetzen der Richtlinie Ihrer Organisation eingerichtet und sichergestellt, dass Benutzer die entsprechenden Daten im Verzeichnis, Benutzer in Ihrer Organisation werden automatisch auf jeder beliebigen Webseite ihre eigenen Kennwörter zurücksetzen, Arbeit oder Schule Konto anmelden (z. B. [portal.microsoftonline.com](https://portal.microsoftonline.com)) verwendet. Auf Seiten wie diese Benutzer sehen eine **kann nicht auf Ihr Konto zugreifen?** verknüpfen.

  ![][002]

Durch Klicken auf diesen Link wird das Self-service-Kennwort zurücksetzen Portal gestartet.

  ![][003]

Hier erfahren Sie, wie Benutzer ihre eigenen Kennwörter zurücksetzen, finden Sie unter [Erste Schritte: Azure AD Password Management](active-directory-passwords-getting-started.md).

##<a name="user-password-change-portal"></a>Portal für Benutzer Kennwort ändern
Wenn Benutzer ihre eigenen Kennwörter ändern möchten, können dabei mit dem Kennwort ändern Portal jederzeit.  Benutzer können das Kennwort ändern Portal Profilseite Abdeckung oder den Link "Kennwort ändern" aus in Office 365-Anwendung zugreifen.  Bei Ablauf ihrer Kennwörter werden Benutzer auch aufgefordert, beim Anmelden automatisch ändern.

  ![][004]

In beiden Fällen werden wenn Kennwort Rückschreiben aktiviert wurde und der Benutzer entweder verbundene oder Kennwort synchronisieren, diese Kennwörter geändert in lokalen Active Directory zurückgeschrieben. Hier ist wie das Kennwort ändern Portal aussieht:

  ![][005]

Hier erfahren Sie, wie Benutzer ihre eigenen lokalen Active Directory-Kennwörter ändern können, finden Sie unter [Erste Schritte: Azure AD Password Management](active-directory-passwords-getting-started.md).

##<a name="password-management-reports"></a>Kennwort-Management-Berichte
Navigieren zur Registerkarte " **Berichte** " und Abschnitt **Aktivitätsprotokolle** suchen, sehen Sie zwei Password Management-Berichte: **Kennwort Aktivität** und **Kennwort zurückgesetzt Registrierungsaktivität**.  Mit diesen zwei Berichten können Sie eine Ansicht der Benutzer Anmeldung und Kennwort zurücksetzen in der Organisation erhalten. Hier wird diese Berichte in [Azure-Verwaltungsportal](https://manage.windowsazure.com)aussehen:

  ![][006]

Weitere Informationen finden Sie unter [erhalten Einblicke: Azure AD Kennwort Berichte](active-directory-passwords-get-insights.md).

##<a name="password-writeback-component-of-azure-ad-connect"></a>Kennwort Rückschreiben Komponente Azure AD Connect
Aus der lokalen Umgebung (entweder über Föderation oder Kennwortsynchronisation) Kennwörter für Benutzer in Ihrer Organisation stammen, können Sie die neueste Version von Azure AD Connect aktivieren diese Kennwörter direkt aus der Cloud Aktualisierung installieren.  Dies bedeutet, dass Benutzer vergessen oder ihr Active Directory-Kennwort ändern möchten, sie also direkt aus dem Internet können.  Hier ist Kennwort Rückschreiben in Azure AD Connect-Installationsassistenten finden:

  ![][007]

Weitere Informationen zu Azure AD Connect finden Sie unter [Erste Schritte: Azure AD Connect](active-directory-aadconnect.md). Weitere Informationen zum Kennwort Rückschreiben finden Sie [Erste Schritte: Azure AD Password Management](active-directory-passwords-getting-started.md).


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>Links zu Kennwort zurücksetzen Dokumentation
Unten finden Sie Links zu allen Dokumentationsseiten Azure AD Kennwort zurücksetzen:

* **Sind Sie hier beim Anmelden Probleme auftreten?** In diesem Fall [hier wie ändern und Ihr eigenes Kennwort](active-directory-passwords-update-your-own-password.md).
* [**Erste Schritte**](active-directory-passwords-getting-started.md) - erfahren Sie, wie Sie Benutzern zurücksetzen und ihre Cloud oder lokalen Kennwörter ändern
* [**Anpassen**](active-directory-passwords-customize.md) - Informationen zum Anpassen von Aussehen und Verhalten und Verhalten des Diensts zu Ihrem Unternehmen
* [**Best Practices**](active-directory-passwords-best-practices.md) - so schnell bereitstellen und Kennwörter in Ihrer Organisation verwalten
* Sie [**erfahren**](active-directory-passwords-get-insights.md) – erfahren Sie mehr über unsere integrierten reporting-Funktionen
* [**FAQ**](active-directory-passwords-faq.md) - Antworten auf häufig gestellte Fragen
* [**Problembehandlung**](active-directory-passwords-troubleshoot.md) – erfahren Sie schnell Probleme mit dem Dienst
* [**Erfahren Sie mehr**](active-directory-passwords-learn-more.md) - gehen tief in die technischen Einzelheiten der Funktionsweise des Dienstes



[001]: ./media/active-directory-passwords-how-it-works/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-how-it-works/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-how-it-works/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-how-it-works/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-how-it-works/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-how-it-works/006.jpg "Image_006.jpg"
[007]: ./media/active-directory-passwords-how-it-works/007.jpg "Image_007.jpg"
