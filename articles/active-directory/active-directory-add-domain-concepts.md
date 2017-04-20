<properties
    pageTitle="Übersicht über benutzerdefinierte Domänennamen in Azure Active Directory | Microsoft Azure"
    description="Erläutert das Konzept für die Verwendung von benutzerdefinierten Domänennamen in Azure Active Directory, einschließlich der Föderation für einmaliges Anmelden"
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand;jeffsta"/>

# <a name="conceptual-overview-of-custom-domain-names-in-azure-active-directory"></a>Übersicht über benutzerdefinierte Domänennamen in Azure Active Directory

Ein Domänenname ist ein wichtiger Bestandteil der Bezeichner für viele Verzeichnisressourcen: ein Benutzer oder eine e-Mail-Adresse für einen Benutzer zu der Adresse für eine Gruppe gehört und Teil der app-ID-URI für eine Anwendung. Ressource in Azure Active Directory (Azure AD) kann einen Domänennamen enthalten, der bereits überprüft das Verzeichnis gehören, die die Ressource enthält. Nur globale Administratoren kann in Azure AD Domain Verwaltungsaufgaben ausführen.

Domänennamen in Azure AD sind global eindeutig. Ein Domänenname kann von einem einzelnen Azure AD verwendet werden. Wenn ein Azure AD-Verzeichnis ein Domänennamens überprüft hat, kann keine anderen Azure AD-Verzeichnis überprüfen oder, denselben Domänennamen verwenden.

## <a name="initial-and-custom-domain-names"></a>Anfängliche und benutzerdefinierten Domänennamen

Jeder Domänenname in Azure AD ist eine anfängliche Domänennamen oder einen benutzerdefinierten Domänennamen.

Jede Azure Anzeige enthält einen anfänglichen Domänennamen in der Form contoso.onmicrosoft.com. Diese dritte Ebene Domänennamen in diesem Beispiel "contoso.onmicrosoft.com" wurde beim Erstellen des Verzeichnisses, in der Regel vom Administrator, die das Verzeichnis erstellt. Der ersten Namen ein Verzeichnis kann nicht geändert oder gelöscht werden. Der ersten Domänennamen während voll funktionsfähige soll hauptsächlich als Bootstrapper Mechanismus verwendet werden, bis, dass Sie ein benutzerdefinierten Domänennamen sichergestellt ist.

In den meisten Fällen Produktion ein Verzeichnis verfügt über mindestens einen verifizierten benutzerdefinierte Domäne, z. B. "contoso.com" und dieser benutzerdefinierten Domäne für Benutzer sichtbar ist. Ein benutzerdefinierten Domänennamen ist ein Domänenname, der Besitzer und von dieser Organisation z. B. "contoso.com" verwendet für Zwecke wie die Website hostet. Diese Domain kennt Mitarbeiter ist Teil des Benutzernamens, mit denen sie das Unternehmensnetzwerk anmelden oder senden und Empfangen von e-Mail.

Bevor sie von Azure AD verwendet werden kann, muss der benutzerdefinierten Domänennamen zum Verzeichnis hinzugefügt und überprüft.

## <a name="verified-and-unverified-domain-names"></a>Überprüft und nicht überprüfte Domänennamen

Der ersten Namen ein Verzeichnis von Azure AD überprüft implizit ausgewertet. Ein Administrator einen benutzerdefinierten Domänennamen Azure AD hinzufügt, wird zunächst in einem nicht überprüften Zustand. Azure AD lässt keine Verzeichnisressourcen nicht überprüfte Domänennamen verwenden. Dadurch kann nur ein einziges Verzeichnis einen bestimmten Domänennamen und die Organisation der Domänenname verwendet diesen Domänennamen tatsächlich besitzt.

Azure AD überprüft den Besitz eines Domänennamens einen bestimmten Eintrag in der Zonendatei Domain Name Service (DNS) für den Domänennamen suchen. Überprüfen Sie den Besitz eines Domänennamens ruft Administrator dem DNS-Eintrag von Azure AD Azure AD sucht und fügt den Eintrag in die Zonendatei DNS-Domänennamen. Die DNS-Zonendatei ist von der Domänennamen-Registrierungsstelle für diese Domäne verwaltet. Die Schritte zum Überprüfen einer Domäne werden im Artikel für das [Hinzufügen einer benutzerdefinierten Domäne in Ihr Verzeichnis Azure AD](active-directory-add-domain.md)angezeigt.

Hinzufügen eines DNS-Eintrags in der Zonendatei für den Domänennamen wirkt sich nicht auf andere Domänendienste wie e-Mail oder Web-hosting.

## <a name="federated-and-managed-domain-names"></a>Föderations- und verwalteten Domänennamen

Ein benutzerdefinierten Domänennamen in Azure AD kann konfiguriert werden, um Benutzern Erfahrung zwischen dem lokalen Active Directory und Azure AD verbundenen Zeichen. Konfigurieren einer Domäne für die Föderation benötigt Updates privilegierte Ressourcen in Azure AD und der Windows Server Active Directory. Konfigurieren eine Föderationsdomäne von Azure AD Connect erfolgen muss oder mit PowerShell. Föderation einer benutzerdefinierten Domäne kann nicht aus dem Azure-Verwaltungsportal initiiert werden. [In diesem Video erfahren Sie AD FS für Benutzer sich mit Azure AD-Verbindung konfigurieren](http://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect).

Nicht verbundene Domänen sind verwaltete Domänen bezeichnet. Die erste Domäne einer Azure AD-Verzeichnis wird implizit als einer verwalteten Domäne ausgewertet.

## <a name="primary-domain-names"></a>Primären Domänennamen

Der primäre Domänenname für ein Verzeichnis ist der Domänenname bereits als Standardwert für 'Domain'-Teil der Benutzername ausgewählt ist, erstellt ein Administrator einen neuen Benutzer in [Azure-Verwaltungsportal](https://manage.windowsazure.com/) oder anderen Portals wie dem Verwaltungsportal von Office 365. Ein Verzeichnis kann nur eine primäre Domänennamen besitzen. Ein Administrator kann den primäre Domänenname zu einer benutzerdefinierten Domäne kein Verbund, überprüft ändern oder die erste Domäne.

## <a name="domain-names-in-azure-ad-and-other-microsoft-online-services"></a>Domänennamen in Azure AD und anderen Microsoft Online Services

Ein Domänenname muss überprüft werden, in Azure AD, bevor es von einem anderen Microsoft-Onlinedienst wie Exchange Online, SharePoint Online und Intune verwendet werden kann. Diese Dienste erfordern in der Regel einen Administrator einen oder mehrere DNS-Einträge hinzufügen für den Dienst.

Eine Azure Web app verwendet einen eigenen Mechanismus an einer Domäne überprüfen. Eine Domäne muss mit Azure AD überprüft werden, selbst wenn bereits für die Verwendung einer Azure Web App in einem Abonnement überprüft worden ist, die diese Azure AD verwendet. Eine Azure Web app können einen Domänennamen, der in einem anderen Verzeichnis aus dem Verzeichnis überprüft wurde, die die Web app sichert.

## <a name="managing-domain-names"></a>Verwalten von Domänennamen

Domäne-Verwaltungsaufgaben können Azure-Verwaltungsportal und PowerShell abgeschlossen werden. Viele Aufgaben können bei Azure AD Graph-API (public Preview) ausgeführt werden.

-   [Hinzufügen und einen benutzerdefinierten Domänennamen überprüfen](active-directory-add-domain.md)

-   [Verwalten von Domänen im klassischen Azure-portal](active-directory-add-manage-domain-names.md)

-   [Mithilfe von PowerShell Domänennamen in Azure AD verwalten](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

-   [Mithilfe der API Azure AD Graph Domänennamen in Azure AD verwalten](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)
