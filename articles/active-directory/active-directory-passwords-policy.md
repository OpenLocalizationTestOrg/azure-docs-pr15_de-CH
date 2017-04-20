<properties
    pageTitle="Kennwortrichtlinien und Restrictions in Azure Active Directory | Microsoft Azure"
    description="Beschreibt die Richtlinien, die Kennwörter in Azure Active Directory, einschließlich Zeichen Länge und Ablaufdatum zuweisen"
  services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand"/>


# <a name="password-policies-and-restrictions-in-azure-active-directory"></a>Kennwortrichtlinien und Restrictions in Azure Active Directory

Dieser Artikel beschreibt die Kennwortrichtlinien und Komplexität zugeordneten Benutzerkonten in Azure AD-Verzeichnis gespeichert.

> [AZURE.IMPORTANT] **Sind Sie hier beim Anmelden Probleme auftreten?** In diesem Fall [hier wie ändern und Ihr eigenes Kennwort](active-directory-passwords-update-your-own-password.md).

## <a name="userprincipalname-policies-that-apply-to-all-user-accounts"></a>UserPrincipalName Richtlinien, die auf alle Benutzerkonten angewendet

Alle Benutzerkonten, die in Azure AD-Authentifizierungssystem anmelden muss einen eindeutige Benutzerprinzipalnamen (UPN) Attributwert mit diesem Konto. Die folgende Tabelle enthält die Richtlinien, die sowohl lokal als Quelle von Active Directory-Benutzerkonten zuweisen (in der Cloud synchronisiert) und Cloud nur Benutzerkonten.

|   Eigenschaft           |     UserPrincipalName Vorschriften  |
|   ----------------------- |   ----------------------- |
|  Zeichen zulässig    |  <ul> <li>A – Z</li> <li>-z </li><li>0 – 9</li> <li> . - \_ ! \# ^ \~</li></ul> |
|  Nicht zulässige Zeichen  | <ul> <li>Alle '@' Zeichen, das nicht den Benutzernamen aus der Domäne trennen.</li> <li>Darf keine Punkte enthalten "." vor der '@' Symbol</li></ul> |
| Länge-Integritätsregeln  |       <ul> <li>Gesamtlänge muss 113 Zeichen nicht überschreiten.</li><li>64 Zeichen vor der ‘@’ Symbol</li><li>48 Zeichen nach dem ‘@’ Symbol</li></ul>

## <a name="password-policies-that-apply-only-to-cloud-user-accounts"></a>Kennwortrichtlinien, die nur für Cloud-Benutzerkonten

Die folgende Tabelle beschreibt die verfügbaren Benutzerkonten angewendet werden können, die erstellt und verwaltet in Azure AD, Kennwortrichtlinien.

|  Eigenschaft       |    Vorschriften          |
|   ----------------------- |   ----------------------- |
|  Zeichen zulässig   |   <ul><li>A – Z</li><li>-z </li><li>0 – 9</li> <li>@# $ % ^ & * - _ ! + = [] {} & #124; \ : ‘ , . ? / ` ~ “ ( ) ;</li></ul> |
|  Nicht zulässige Zeichen   |       <ul><li>Unicode-Zeichen</li><li>Leerzeichen</li><li> **Sichere Kennwörter**: dürfen nicht Punkt "." vor der '@' Symbol</li></ul> |
|   Kennwort eingeschränkt | <ul><li>mindestens 8 Zeichen und maximal 16 Zeichen</li><li>**Sichere Kennwörter**: erfordert 3 4 Folgendes:<ul><li>Kleinbuchstaben</li><li>Großbuchstaben</li><li>Zahlen (0-9)</li><li>Symbole (Siehe kennworteinschränkungen oben)</li></ul></li></ul> |
| Dauer bis zum Ablauf Kennwort      | <ul><li>Standardwert: **90** Tage </li><li>Wert lässt sich mithilfe des Set-MsolPasswordPolicy-Cmdlet aus dem Azure Active Directory-Modul für Windows PowerShell.</li></ul> |
| Benachrichtigung über abgelaufene Kennwörter |  <ul><li>Standardwert: **14** Tage (vor Ablauf des Kennworts)</li><li>Wert lässt sich mithilfe des Set-MsolPasswordPolicy-Cmdlet.</li></ul> |
| Kennwortablauf |  <ul><li>Standardwert: **false** Tage (gibt an, dass Kennwortablauf aktiviert) </li><li>Wert kann für einzelne Benutzerkonten über das Cmdlet "Set-MsolUser" konfiguriert werden. </li></ul> |
|  Kennwortchronik  | Letzte Kennwort kann nicht erneut verwendet werden. |
|  Kennwort Geschichte Dauer | Endlos |
|  Konto gesperrt | Nach 10 erfolglosen Anmeldeversuchen (Falsches Kennwort) wird der Benutzer eine Minute lang gesperrt. Weiter werden falsche Versuche der Benutzer sperren zur Erhöhung der Dauer. |


## <a name="next-steps"></a>Nächste Schritte

* **Sind Sie hier beim Anmelden Probleme auftreten?** In diesem Fall [hier wie ändern und Ihr eigenes Kennwort](active-directory-passwords-update-your-own-password.md).
* [Kennwörter überall verwalten](active-directory-passwords.md)
* [Funktionsweise von Kennwort-Management](active-directory-passwords-how-it-works.md)
* [Erste Schritte mit Kennwort-Management](active-directory-passwords-getting-started.md)
* [Anpassen von Kennwort-Management](active-directory-passwords-customize.md)
* [Best Practices für die Kennwort-Management](active-directory-passwords-best-practices.md)
* [Das operative Einblicke mit Kennwort-Management](active-directory-passwords-get-insights.md)
* [Verwaltung der Kennwörter FAQS](active-directory-passwords-faq.md)
* [Problembehandlung bei Kennwort-Management](active-directory-passwords-troubleshoot.md)
* [Weitere Informationen](active-directory-passwords-learn-more.md)
