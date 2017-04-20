<properties
    pageTitle="Was ist Self-Service anmelden für Azure? | Microsoft Azure"
    description="Eine Übersicht über Self-service-Anmeldung für Azure, den Anmeldeprozess zu verwalten und einen DNS-Domänennamen zu."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/23/2016"
    ms.author="curtand"/>


# <a name="what-is-self-service-signup-for-azure"></a>Was ist Self-Service anmelden für Azure?

Dieses Thema erläutert die Self-service anmelden und einen DNS-Domänennamen zu.  

## <a name="why-use-self-service-signup"></a>Warum verwenden Sie Self-service registrieren?

- Können Sie Kunden Services schneller möchten.
- Erstellen Sie e-Mail-basierten Angebote für einen Dienst.
- Erstellen Sie e-Mail-basierte Anmeldung die schnelle Benutzer Identitäten mit ihrer Arbeit leicht zu merkende e-Mail-Aliasnamen erstellen können.
- Nicht verwaltete Azure Verzeichnisse erfolgen in verwalteten Verzeichnisse höher und für andere Dienste verwendet werden.

## <a name="terms-and-definitions"></a>Begriffe und Definitionen

+ **Self-Service anmelden**: Dies ist die Methode, die ein Benutzer für einen Clouddienst angemeldet und hat eine Identität in Azure Active Directory (Azure AD) für sie automatisch basierend auf ihrer e-Mail-Domäne.
+ **Nicht verwaltete Azure-Verzeichnis**: Dies ist das Verzeichnis, in dem diese Identität erstellt. Eine nicht verwaltete Verzeichnis ist ein Verzeichnis, das kein globaler Administrator verfügt.
+ **Benutzer e-Mail überprüft**: Dies ist ein Benutzerkonto in Azure AD. Ein Benutzer mit einer Identität erstellt automatisch nach der Anmeldung für ein Self-service-Angebot bekannt als Benutzer e-Mails überprüft. Ein e-Mail überprüft Benutzer ist Mitglied eines Verzeichnisses mit Creationmethod markiert = EmailVerified.

## <a name="user-experience"></a>Benutzer

Wir beispielsweise einen Benutzer, deren e-Mail-Adresse Dan@BellowsCollege.com vertrauliche Dateien per e-Mail empfängt. Die Dateien wurden von RMS (Azure RMS) geschützt. Aber Bellows College Dan Organisation wurde nicht für Azure RMS angemeldet und wurde Active Directory RMS bereitgestellt. In diesem Fall kann Dan für ein kostenloses Abonnement für RMS für Einzelpersonen anmelden um die geschützten Dateien lesen.

Wenn Dan der erste Benutzer mit einer e-Mail-Adresse BellowsCollege.com für dieses Angebot, Self-Service anmelden, nicht verwalteten Verzeichnis für BellowsCollege.com in Azure AD erstellt werden. Wenn andere Benutzer der Domäne BellowsCollege.com für dieses Angebot oder ähnliche Self-service-Angebot anmelden, müssen sie auch e-Mails überprüft Benutzerkonten in der nicht verwalteten Verzeichnis in Azure erstellt.

## <a name="admin-experience"></a>Admin-Erfahrung

Administrator Besitzer der DNS-Domänenname nicht verwaltete Azure-Verzeichnis kann übernehmen oder das Verzeichnis nach dem Nachweis der Besitz zusammenführen. In den nächsten Abschnitten Admin Erfahrung genauer erklären, aber hier ist eine Zusammenfassung:

- Übernehmen einer nicht verwalteten Azure-Verzeichnis werden Sie einfach der globale Administrator nicht verwalteten Verzeichnis. Dies ist eine interne Übernahme bezeichnet.
- Beim Zusammenführen nicht verwaltete Azure-Verzeichnis den DNS-Domänennamen des Verzeichnisses nicht verwalteten zum verwalteten Azure Verzeichnis hinzufügen und eine Zuordnung von Benutzern zu Ressourcen erstellt können so Benutzer weiterhin ohne Unterbrechung zugreifen. Dies ist eine externe Übernahme bezeichnet.

## <a name="what-gets-created-in-azure-active-directory"></a>Was wird in Azure Active Directory erstellt?

#### <a name="directory"></a>Verzeichnis

- Ein Azure Active Directory-Verzeichnisdienst für die Domäne erstellt, ein Verzeichnis pro Domäne.
- Azure AD-Verzeichnis ist kein globaler Administrator.

#### <a name="users"></a>Benutzer

- Für jeden Benutzer anmeldet, wird ein Benutzerobjekt in Azure AD-Verzeichnis erstellt.
- Jedes Benutzerobjekt wird als extern gekennzeichnet.
- Jeder Benutzer erhält Zugriff auf den Dienst, die sie sich angemeldet.

### <a name="how-do-i-claim-a-self-service-azure-ad-directory-for-a-domain-i-own"></a>Wie beantrage ich eine benutzerverantwortliche Azure AD für eine Domäne besitze?

Self-service Azure AD Anspruch von Domäne Validierung Verzeichnis. Domäne Validierung beweist die Domäne besitzen, DNS-Einträge erstellen.

Es gibt zwei Möglichkeiten DNS-Übernahme von Azure AD-Verzeichnis:

- interne Übernahme (Admin erkennt nicht verwaltete Azure-Verzeichnis und möchte in einem verwalteten Verzeichnis)
- externe Übernahme (Admin versucht ihre verwalteten Azure Verzeichnis eine neue Domäne hinzufügen)

Möglicherweise interessiert, eine Domäne besitzen, da Sie nicht verwalteten Verzeichnis übernehmen nach ein Benutzer Self-service-Anmeldung ausgeführt oder eine neue Domäne zu einer vorhandenen verwalteten Verzeichnis fügen überprüfen. Angenommen, Sie haben eine Domäne mit dem Namen contoso.com und eine neue Domäne mit dem Namen contoso.co.uk oder contoso.uk hinzufügen möchten.

## <a name="what-is-domain-takeover"></a>Was ist Domäne Übernahme?  

Dieser Abschnitt erläutert die überprüfen, ob Sie eine Domäne besitzen

### <a name="what-is-domain-validation-and-why-is-it-used"></a>Was ist die Domäne Validierung und warum verwendet?

Um Vorgänge in einem Verzeichnis erfordert Azure AD, an die DNS-Domäne zu überprüfen.  Überprüfung der Domäne können Sie auf das Verzeichnis und entweder fördern verwaltetes Verzeichnis Verzeichnis Self-service oder Self-service-Verzeichnis in einen vorhandenen verwalteten Verzeichnis.

## <a name="examples-of-domain-validation"></a>Der Domain-Validierung

Es gibt zwei Möglichkeiten DNS-Übernahme eines Verzeichnisses:

+ interne Übernahme (z. B. Administrator Self-service, nicht verwalteten Verzeichnis entdeckt und möchte in verwalteten Verzeichnis)
+ externe Übernahme (z. B. ein Administrator versucht, ein verwaltetes Verzeichnis eine neue Domäne hinzufügen)

### <a name="internal-takeover---promote-a-self-service-unmanaged-directory-to-be-a-managed-directory"></a>Interne Übernahme - Self-service, nicht verwalteten Verzeichnis verwaltetes Verzeichnis zu fördern

Dabei interne Übernahme wird das Verzeichnis verwaltetes Verzeichnis aus einem nicht verwalteten Verzeichnis konvertiert. Sie müssen DNS-Domain Name Überprüfung abgeschlossen, einem MX- oder TXT-Eintrag in der DNS-Zone erstellen. Diese Aktion:

+ Überprüft, ob die Domäne besitzen
+ Das Verzeichnis verwaltet wird
+ Macht den globalen Administrator des Verzeichnisses

Angenommen, ein IT-Administrator von Bellows College entdeckt, Self-service-Angebote für Benutzer von der Schule angemeldet haben. Als Besitzer der DNS-name BellowsCollege.com, IT-Administratoren an den DNS-Namen in Azure überprüfen und dann das nicht verwaltete Verzeichnis übernehmen. Das Verzeichnis wird verwaltetes Verzeichnis und IT-Administrator ist für das BellowsCollege.com-Verzeichnis die globalen Administratorrolle zugewiesen.

### <a name="external-takeover---merge-a-self-service-directory-into-an-existing-managed-directory"></a>Externe Übernahme - Self-service-Verzeichnis in ein vorhandenes verwaltetes Verzeichnis zusammenführen

In einer externen Übernahme bereits verwaltetes Verzeichnis und alle Benutzer und Gruppen aus einem nicht verwalteten Verzeichnis, verwaltete Verzeichnis beitreten soll, anstatt eigene zwei getrennte Verzeichnisse.

Als Administrator verwalteten Verzeichnis eine Domäne hinzufügen und dieser Domäne zufällig einen nicht verwalteten Verzeichnis zugeordnet.

Angenommen Sie sind ein IT-Administrator und bereits verwaltetes Verzeichnis für Contoso.com, einen Domänennamen für Ihre Organisation registriert ist. Sie entdecken, dass Benutzer Ihrer Organisation Self-service anmelden, Angebot durchgeführten mithilfe der e-Mail-Domänenname user@contoso.co.uk, einen anderen Domänennamen ein, der im Besitz der Organisation ist. Diese Benutzer verfügen über Konten derzeit in einem nicht verwalteten Verzeichnis für contoso.co.uk.

Nicht zwei separate Verzeichnisse verwalten soll, das nicht verwaltete Verzeichnis contoso.co.uk in Ihr vorhandenes IT-verwaltete Verzeichnis für contoso.com zusammenführen.

Externe Übernahme folgt den gleichen DNS-Validierungsprozess als interne Übernahme.  Differenz wird: Benutzer und Dienste zum IT-verwaltete Verzeichnis zugeordnet.

#### <a name="whats-the-impact-of-performing-an-external-takeover"></a>Was ist die Auswirkung der Ausführung einer externen Übernahme?

Mit einer externen Übernahme wird eine Zuordnung von Benutzern zu Ressourcen erstellt, damit Benutzer Zugriff auf Dienste ohne Unterbrechung fortgesetzt werden können. RMS für Einzelpersonen, einschließlich häufig behandeln auch die Zuordnung von Benutzern zu Ressourcen, und Benutzer können weiterhin Zugriff auf diese Dienste ohne Änderung. Wenn eine Anwendung die Zuordnung von Benutzern zu Ressourcen nicht effektiv behandeln, werden externe Übernahme explizit blockiert um eine schlechte Erfahrung zu verhindern.

#### <a name="directory-takeover-support-by-service"></a>Verzeichnis Übernahme Support Service

Die folgenden Dienste unterstützen derzeit Übernahme:

- RMS


Die folgenden Dienste bald unterstützt Übernahme:

- PowerBI

Folgende nicht und erfordert zusätzliche Administratoraktion Benutzerdaten nach externen Übernahme migrieren.

- SharePoint-OneDrive


## <a name="how-to-perform-a-dns-domain-name-takeover"></a>DNS-Domain Name Übernahme durchführen

Sie haben einige Optionen wie zu Domäne Validierung (Übernahme Wunsch):

1.  Azure-Verwaltungsportal

    Hinzufügen einer Domäne durchführen eine Übernahme ausgelöst.  Wenn ein Verzeichnis für die Domäne vorhanden ist, müssen Sie eine externe Übernahme ausführen.

    Melden Sie sich mit Ihren Anmeldeinformationen Azure-Portal.  Navigieren Sie in Ihr vorhandenes Verzeichnis und anschließend in **Domäne hinzufügen**.

2.  Office 365

    Die Optionen auf der Seite [Domänen verwalten](https://support.office.com/article/Navigate-to-the-Office-365-Manage-domains-page-026af1f2-0e6d-4f2d-9b33-fd147420fac2/) können in Office 365 Sie mit Domänen und DNS-Datensätzen arbeiten. Finden Sie unter [Überprüfen der Domäne in Office 365](https://support.office.com/article/Verify-your-domain-in-Office-365-6383f56d-3d09-4dcb-9b41-b5f5a5efd611/).

3.  Windows PowerShell

    Die folgenden Schritte müssen die Validierung mithilfe von Windows PowerShell.

    Schritt    |   Cmdlet verwenden
    ------- | -------------
    Erstellen Sie ein Objekt mit Anmeldeinformationen | Get-Credential
    Verbinden mit Azure AD | Verbindung MsolService
    eine Liste der Domänen   | MsolDomain abrufen
    Eine Herausforderung  | MsolDomainVerificationDns abrufen
    DNS-Eintrag erstellen   | Hierzu des DNS-Servers
    Überprüfen Sie die Herausforderung    | MsolEmailVerifiedDomain bestätigen

Zum Beispiel:

1. Verbinden mit Azure AD mit den Anmeldeinformationen, mit denen auf das Self-service-Angebot zu reagieren: Import-Module MSOnline $msolcred = Get-Credential-Verbindung-Msolservice-$msolcred von Anmeldeinformationen

2. Eine Liste der Domänen abrufen:

    MsolDomain abrufen

3. Führen Sie das Cmdlet Get-MsolDomainVerificationDns um eine Abfrage zu erstellen:

    Get-MsolDomainVerificationDns-DomainName *Ihr_Domänenname* -Modus DnsTxtRecord

    Zum Beispiel:

    Get-MsolDomainVerificationDns-DomainName contoso.com-Modus DnsTxtRecord

4. Kopieren von diesem Befehl zurückgegebene Wert (Anfrage).

    Zum Beispiel:

    MS = 32DD01B82C05D27151EA9AE93C5890787F0E65D9

5. Erstellen Sie in Ihrem öffentlichen DNS-Namespace DNS-Txt-Datensatz mit dem Wert, den Sie im vorherigen Schritt kopiert.

    Der Name für diesen Eintrag ist der Name der übergeordneten Domäne, bei der Erstellung dieser Ressourceneintrag mit DNS-Funktion von Windows Server also Datensatz Name leeren und nur einfügen den Wert im Textfeld

6. Führen Sie das Cmdlet bestätigen MsolDomain überprüfen die Herausforderung:

    Bestätigen-MsolEmailVerifiedDomain - DomainName *Ihr_Domänenname*

    Zum Beispiel:

    Bestätigen-MsolEmailVerifiedDomain - DomainName contoso.com

Eine erfolgreiche Herausforderung gibt die Meldung ohne Fehler zurück.

## <a name="how-do-i-control-self-service-settings"></a>Steuern Self-service-Optionen

Administratoren haben heute zwei Self-service-Steuerelemente. Sie können Folgendes steuern:

- Ob der Benutzer das Verzeichnis e-Mail beitreten können.
- Ob Benutzer sich für Programme und Dienste lizenzieren können.


### <a name="how-can-i-control-these-capabilities"></a>Wie können diese Funktionen steuern?

Administrator kann diese Funktionen diese Azure AD-Cmdlet Set MsolCompanySettings Parameter konfigurieren:

+ **AllowEmailVerifiedUsers** steuert, ob ein Benutzer erstellen oder einer nicht verwalteten Verzeichnis beitreten. Wenn Sie diesen Parameter auf $false festgelegt, können Benutzer e-Mail überprüft das Verzeichnis verknüpfen.
+ **AllowAdHocSubscriptions** steuert die Möglichkeit für Benutzer von Self-service Anmelden ausführen. Wenn Sie diesen Parameter auf $false festgelegt, können keine Benutzer Self-service-Anmeldung durchführen.


### <a name="how-do-the-controls-work-together"></a>Wie funktionieren die Steuerelemente zusammen?

Diese beiden Parameter zusammen lässt sich eine bessere Kontrolle über Self-service Zeichen definieren. Beispielsweise können der folgende Befehl Benutzer Self-service anmelden, aber nur ausführen, wenn die Benutzer ein Konto in Azure AD bereits (also Benutzer ein Konto e-Mail überprüft erstellt werden müssten nicht möglich Zeichen Self-service einrichten):

    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true

Im folgende Flussdiagramm erläutert alle Kombinationen dieser Parameter und die resultierende Verzeichnis und Self-service anmelden.

![][1]

Weitere Informationen und Beispiele zur Verwendung dieser Parameter finden Sie unter [Set-MsolCompanySettings](https://msdn.microsoft.com/library/azure/dn194127.aspx).

## <a name="see-also"></a>Siehe auch

-  [Zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md)

-  [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)

-  [Azure-Cmdlet-Referenz](https://msdn.microsoft.com/library/azure/jj554330.aspx)

-  [MsolCompanySettings festlegen](https://msdn.microsoft.com/library/azure/dn194127.aspx)

<!--Image references-->
[1]: ./media/active-directory-self-service-signup/SelfServiceSignUpControls.png
