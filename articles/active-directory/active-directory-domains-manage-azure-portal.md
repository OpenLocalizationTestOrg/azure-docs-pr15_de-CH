<properties
    pageTitle="Verwalten von benutzerdefinierten Domänennamen in Azure Active Directory Vorschau | Microsoft Azure"
    description="Konzepte und Vorgehensweisen für die Verwaltung von Domänennamen in Azure Active Directory"
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
    ms.date="09/12/2016"
    ms.author="curtand;jeffsta"/>

# <a name="managing-custom-domain-names-in-your-azure-active-directory-preview"></a>Verwalten von benutzerdefinierten Domänennamen in Azure Active Directory Vorschau

Ein Domänenname ist ein wichtiger Bestandteil der Bezeichner für viele Verzeichnisressourcen: ein Benutzer oder eine e-Mail-Adresse für einen Benutzer zu der Adresse für eine Gruppe gehört und Teil der app-ID-URI für eine Anwendung. Ressource in Azure Active Directory (Azure AD) Vorschau kann einen Domänennamen enthalten, der bereits überprüft das Verzeichnis gehören, die die Ressource enthält. [Neuigkeiten in der Vorschau](active-directory-preview-explainer.md) Nur globale Administratoren kann in Azure AD Domain Verwaltungsaufgaben ausführen.

## <a name="set-the-primary-domain-name-for-your-azure-ad-directory"></a>Festlegen der primären Domänennamens Azure AD-Verzeichnis

Das Verzeichnis erstellt wird, wird der ersten Domänennamen wie "contoso.onmicrosoft.com" auch primärer Domänenname. Die primäre Domäne ist der Standard-Domänenname für einen neuen Benutzer beim Erstellen eines neuen Benutzers. Den Vorgang zum Erstellen neuer Benutzer im Portal Administrator optimiert. Den primäre Domänenname ändern:

1.  Melden Sie sich bei [Azure-Portal](https://portal.azure.com) mit einem Konto kein globaler Administrator für das Verzeichnis.

2.  Wählen Sie **Weitere Dienste**, geben Sie **Azure Active Directory** in das Textfeld ein und Sie dann die **EINGABETASTE**.

    ![Verwaltung öffnen](./media/active-directory-domains-add-azure-portal/user-management.png)

3. Wählen Sie auf Blade ***Verzeichnisnamen*** **Domänennamen**.

4. Blade ** *Verzeichnisnamen* - Domänennamen** wählen Sie primärer Domänenname möchten Domänennamen.

5.  Wählen Sie auf der ***Domänenname*** (d. h. das Blade, das geöffnet wird, die neuen Domänennamen im Titel) **primären** . Bestätigen der Aufforderung.

    ![Stellen Sie einen Domänennamen primäre](./media/active-directory-domains-manage-azure-portal/make-primary.png)

Ändern Sie den primären Namen für das Verzeichnis überprüft benutzerdefinierte Domäne sein, die nicht verbunden ist. Ändern der primären Domäne für das Verzeichnis ändert nicht die Benutzernamen für alle vorhandenen Benutzer.

## <a name="add-custom-domain-names-to-your-azure-ad"></a>Azure AD benutzerdefinierten Domänennamen hinzufügen

Sie können bis zu 900 benutzerdefinierten Domänennamen jeder Azure AD-Verzeichnis hinzufügen. Der Prozess zum [Hinzufügen eines zusätzlichen benutzerdefinierten Domänennamen](active-directory-domains-add-azure-portal.md) ist für den ersten benutzerdefinierten Domänennamen.

## <a name="add-subdomains-of-a-custom-domain"></a>Unterdomänen einer benutzerdefinierten Domäne hinzufügen

Wenn Sie einen dritten Ebene Domänennamen wie "europe.contoso.com" zum Verzeichnis hinzufügen möchten, sollten Sie hinzufügen und überprüfen die Domäne zweiter Ebene, z. B. contoso.com. Die Domäne wird von Azure AD automatisch überprüft. Um anzuzeigen, dass die hinzugefügten Domäne überprüft wurde, aktualisieren Sie die Seite im Browser, der Domänen auflistet.

## <a name="what-to-do-if-you-change-the-dns-registrar-for-your-custom-domain-name"></a>Wenn die DNS-Registrierung für den benutzerdefinierten Domänennamen ändern

Wenn Sie die DNS-Registrierung für Ihren benutzerdefinierten Domänennamen ändern, können Sie weiterhin mit benutzerdefinierten Domänennamen mit Azure selbst ohne Unterbrechung und ohne zusätzliche Konfigurationsaufgaben ausführen. Verwenden Sie Ihren Domainnamen mit Office 365 finden Sie in der Dokumentation für diese Dienste Intune oder andere Dienste, die auf benutzerdefinierten Domänennamen in Azure AD.

## <a name="delete-a-custom-domain-name"></a>Löschen Sie einen benutzerdefinierten Domänennamen

Wenn Ihre Organisation mehr, Domänennamen oder müssen Sie diesen Domänennamen mit anderen Azure AD verwenden, können Sie einen benutzerdefinierten Domänennamen von Azure AD löschen.

Um einen benutzerdefinierten Domänennamen zu löschen, müssen Sie zunächst sicherstellen, dass keine Ressourcen in Ihrem Verzeichnis auf den Domänennamen. Sie können keinen Domänennamen aus dem Verzeichnis löschen, wenn:

-   Jeder Benutzer hat einen Benutzernamen, e-Mail-Adresse oder Proxyadresse, die den Domänennamen einschließt.

-   Jede Gruppe hat eine e-Mail-Adresse oder Proxy, die den Domänennamen einschließt.

-   Jede Anwendung in Azure AD ist eine app-ID-URI, der den Domänennamen enthält.

Sie müssen ändern oder eine solche Ressource in Azure AD-Verzeichnis löschen, bevor Sie den benutzerdefinierten Domänennamen löschen können.

## <a name="use-powershell-or-graph-api-to-manage-domain-names"></a>Mit dem PowerShell oder Graph API Domänennamen verwalten

Die meisten Verwaltungsaufgaben für Domänennamen in Azure Active Directory können auch Microsoft PowerShell oder Programmgesteuertes Azure AD Graph-API (in public Preview) abgeschlossen werden.

-   [Mithilfe von PowerShell Domänennamen in Azure AD verwalten](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

-   [Mithilfe von Graph-API Domänennamen in Azure AD verwalten](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a>Nächste Schritte

-   [Hinzufügen von benutzerdefinierten Domänennamen](active-directory-domains-add-azure-portal.md)
