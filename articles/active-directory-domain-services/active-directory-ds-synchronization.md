<properties
    pageTitle="Azure Active Directory Domain Services: Synchronisierung in verwalteten Domains | Microsoft Azure"
    description="Verstehen Sie Synchronisation in einer verwalteten Domäne Azure Active Directory Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="synchronization-in-an-azure-ad-domain-services-managed-domain"></a>Synchronisation in einer verwalteten Domäne Azure Active Directory-Domänendienste
Das folgende Diagramm veranschaulicht die Synchronisierung in Azure Active Directory-Domänendiensten verwalteten Domains Funktionsweise.

![Azure Active Directory-Domänendienste-Synchronisierungstopologie](./media/active-directory-domain-services-design-guide/sync-topology.png)

## <a name="synchronization-from-your-on-premises-directory-to-your-azure-ad-tenant"></a>Synchronisierung aus dem lokalen Verzeichnis Azure AD-Mandanten
Azure AD Connect-Synchronisierung wird verwendet, um Benutzerkonten zu synchronisieren, Mitgliedschaften und Anmeldeinformationen hashes, Azure AD-Mandanten. Attribute des Benutzers wie UPN Konten und lokale SID (Security Identifier) synchronisiert. Bei Verwendung von Azure Active Directory-Domänendienste werden ältere Anmeldeinformationen Hashes für NTLM und Kerberos-Authentifizierung erforderlich, Azure AD-Mandanten synchronisiert.

Konfigurieren Zurückschreiben werden Veränderungen in Azure AD-Verzeichnis auf Ihrem lokalen Active Directory synchronisiert. Beispielsweise wenn Sie Ihr Kennwort mit Azure AD Self-service-Kennwort ändern Features ändern, das geänderte Kennwort wird aktualisiert in der lokalen Active Directory-Domäne.

> [AZURE.NOTE] Verwenden Sie immer die neueste Version von Azure AD Connect um sicherzustellen, dass Sie alle bekannten Fehler ist.


## <a name="synchronization-from-your-azure-ad-tenant-to-your-managed-domain"></a>Synchronisierung von Azure AD-Mandanten zu verwalteten Domäne
Benutzerkonten, Gruppenmitgliedschaften und Anmeldeinformationen Hashes werden von Azure AD-Mandanten Ihrer verwalteten Azure Active Directory-Domänendienste-Domäne synchronisiert. Diese Synchronisation erfolgt automatisch. Sie müssen nicht konfigurieren, überwachen oder Verwalten dieser Synchronisierungsprozess. Die Synchronisierung ist auch one-way/unidirektional Natur. Der verwalteten Domäne ist weitgehend außer alle benutzerdefinierten Organisationseinheiten erstellen. Daher ändern nicht Sie Benutzerattribute, Kennwörter oder Gruppenmitgliedschaften innerhalb der verwalteten Domäne. Daher ist keine umgekehrte Synchronisierung von verwalteten Domäne in Azure AD-Mandanten geändert.


## <a name="synchronization-from-a-multi-forest-on-premises-environment"></a>Synchronisation von einer Umgebung mit mehreren Gesamtstrukturen lokal
Viele Unternehmen haben eine ziemlich komplexe lokale Identitätsinfrastruktur mehrere Kontengesamtstrukturen aus. Azure AD Connect unterstützt das Synchronisieren von Benutzern, Gruppen und Anmeldeinformationen Hashes aus mehreren Gesamtstrukturen zu Azure AD-Mandanten.

Im Gegensatz dazu ist Azure AD-Mandanten ein viel einfacher und flache Namespace. Zu Clientanwendungen von Azure AD gesichert zuverlässig auf Konflikte UPN über Benutzerkonten in verschiedenen Gesamtstrukturen. Ihre Azure Active Directory-Domänendienste verwalteten Domäne Bären Ähnlichkeit mit Ihrem Azure AD-Mandanten schließen Daher sehen Sie eine flache Struktur für Organisationseinheiten in der verwalteten Domäne. Alle Benutzer und Gruppen werden im Container "AADDC Benutzer unabhängig von der lokalen Domäne oder Gesamtstruktur aus dem in synchronisiert wurden gespeichert. Eine hierarchische Organisationseinheit so konfiguriert lokalen Struktur. Der verwalteten Domäne hat jedoch noch eine einfache flache OU-Struktur.


## <a name="exclusions---what-isnt-synchronized-to-your-managed-domain"></a>Ausschlüsse – was der verwalteten Domäne synchronisiert ist nicht
Die folgenden Objekte oder Attribute nicht Azure AD-Mandanten oder verwalteten Domäne synchronisiert:

- **Attribute ausgeschlossen:** Sie können bestimmte Attribute von Azure AD-Mandanten aus der lokalen Domäne mit Azure AD verbinden Synchronisieren ausschließen. Diese ausgeschlossenen Attribute sind in der verwalteten Domäne nicht verfügbar.

- **Gruppenrichtlinien:** Gruppenrichtlinien in der lokalen Domäne konfiguriert sind nicht mit der verwalteten Domäne synchronisiert.

- **SYSVOL-Freigabe:** Der Inhalt der SYSVOL-Freigabe in der lokalen Domäne werden entsprechend der verwalteten Domäne nicht synchronisiert.

- **Computerobjekte:** Computerobjekte für Computer, die der lokalen Domäne werden nicht verwaltete Domäne synchronisiert. Diese Computer haben eine Vertrauensstellung mit der verwalteten Domäne und nur der lokalen Domäne gehören. In der verwalteten Domäne finden Sie Computerobjekte für Computer, die Sie explizit mit der verwalteten Domäne Domäne haben.

- **SidHistory-Attribute für Benutzer und Gruppen:** Der primäre Benutzer und die primäre Gruppe SIDs der lokalen Domäne werden der verwalteten Domäne synchronisiert. Allerdings werden vorhandene SidHistory-Attribute für Benutzer und Gruppen nicht aus der lokalen Domäne der verwalteten Domäne synchronisiert.

- **Organisation (Organisationseinheit) Strukturen:** Organisatorische Einheiten in der lokalen Domäne synchronisieren nicht verwalteten Domäne. Es gibt zwei integrierte Organisationseinheiten in der verwalteten Domäne. Standardmäßig hat der verwalteten Domäne eine flache Struktur für Organisationseinheiten. Sie können jedoch [eine benutzerdefinierte Organisationseinheit in der verwalteten Domäne](./active-directory-ds-admin-guide-create-ou.md)erstellen.


## <a name="how-specific-attributes-are-synchronized-to-your-managed-domain"></a>Wie bestimmte Attribute der verwalteten Domäne synchronisiert werden
Die folgende Tabelle listet einige allgemeine Attribute und beschreibt, wie sie Ihre verwalteten Domäne synchronisiert werden.

| Das Attribut in der verwalteten Domäne | Quelle | Notizen |
|:---|:---|:---|
|UPN|Das UPN-Attribut des Benutzers in Azure AD-Mandanten|Das UPN-Attribut von Azure AD-Mandanten ist wie der verwalteten Domäne synchronisiert. Daher verwendet das zuverlässigste Verfahren der verwalteten Domäne anmelden Ihre UPN.|
|SAMAccountName|Benutzers MailNickname-Attribut in Azure AD-Mandanten oder automatisch generiert|Das Attribut "sAMAccountName" stammt aus dem MailNickname-Attribut in Azure AD-Mandanten. Haben mehrere Benutzerkonten das gleiche Attribut MailNickname ist SAMAccountName automatisch generiert. MailNickname oder UPN-Präfix des Benutzers ist mehr als 20 Zeichen SAMAccountName zu max. 20 Zeichen SAMAccountName-Attribute automatisch generiert.|
|Kennwörter|Benutzerkennwort von Azure AD-Mandanten|Für NTLM oder Kerberos-Authentifizierung (auch als zusätzliche Anmeldeinformationen) erforderlichen Anmeldeinformationen Hashes werden von Azure AD-Mandanten synchronisiert. Wenn Azure AD-Mandanten eine synchronisierte Mieter, stammen diese Anmeldeinformationen aus der lokalen Domäne.|
|Primäre Benutzer/Gruppen-SID|Automatisch generiert|Die primäre SID für Benutzer-oder Gruppenkonten wird in der verwalteten Domäne automatisch generiert. Dieses Attribut entspricht nicht die primäre Benutzer/Gruppe SID des Objekts in der lokalen Active Directory-Domäne. Dies ist die verwaltete Domäne als der lokalen Domäne einen anderen SID-Namespace hat.|
|SID-Verlauf für Benutzer und Gruppen|Primäre für lokale Benutzer und Gruppen-SID|Das Attribut SidHistory für Benutzer und Gruppen in der verwalteten Domäne wird die entsprechende primäre Benutzer oder Gruppen-SID in der lokalen Domäne festgelegt. Dadurch anheben und Verschiebung der lokalen Anwendung verwalteten Domäne zu vereinfachen, da Re-ACL Ressourcen brauchen.|

> [AZURE.NOTE] **Melden Sie sich bei der verwalteten Domäne UPN-Format verwenden:** SAMAccountName-Attribut kann für einige Benutzerkonten der verwalteten Domäne automatisch generiert werden. Wenn mehrere Benutzer das gleiche Attribut MailNickname oder Benutzer überlange UPN-Präfixe, möglicherweise der SAMAccountName für Benutzer automatisch generiert. Daher ist das SAMAccountName-Format (z. B. ' CONTOSO100\joeuser') nicht immer zuverlässig an der Domäne anmelden. Die UPN-Präfix kann Benutzer automatisch generierter SAMAccountName abweichen. Verwenden Sie das UPN-Format (z. B. 'joeuser@contoso100.com') zuverlässig bei verwalteten Domäne anmelden.


## <a name="objects-that-are-not-synchronized-to-your-azure-ad-tenant-from-your-managed-domain"></a>Objekte, die nicht Ihrem Azure AD-Mandanten aus der verwalteten Domäne synchronisiert werden
Wie im vorherigen Abschnitt dieses Artikels beschrieben, besteht keine Synchronisierung aus der verwalteten Domäne, Azure AD-Mandanten. Sie können in der verwalteten Domäne erstellen Sie [Eine benutzerdefinierte Organisationseinheit (OU)](./active-directory-ds-admin-guide-create-ou.md) . Außerdem können Sie andere Organisationseinheiten, Benutzer, Gruppen und Dienstkonten in dieser benutzerdefinierten Organisationseinheiten erstellen. Keines der Objekte in benutzerdefinierten Organisationseinheiten erstellt werden, Azure AD-Mandanten synchronisiert. Diese Objekte sind nur innerhalb der verwalteten Domäne verfügbar. Daher sind diese Objekte nicht sichtbar, Azure AD PowerShell-Cmdlets Azure AD Graph-API oder Azure AD Management UI.


## <a name="related-content"></a>Verwandte Inhalte
- [Features - Dienstleistungen Azure AD-Domäne](active-directory-ds-features.md)

- [Bereitstellungsszenarien - Azure Active Directory-Domänendienste](active-directory-ds-scenarios.md)

- [Netzwerk-Aspekte für Azure Active Directory-Domänendienste](active-directory-ds-networking.md)

- [Erste Schritte mit Azure Active Directory-Domänendienste](active-directory-ds-getting-started.md)
