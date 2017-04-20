<properties
    pageTitle="Deaktivieren von User anmelden für eine Enterprise-Anwendung in Azure Active Directory Vorschau | Microsoft Azure"
    description="Eine Enterprise-Anwendung deaktivieren, damit Benutzer es in Active Directory Azure anmelden kann"
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
    ms.date="10/17/2016"
    ms.author="curtand"/>


# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory-preview"></a>Deaktivieren Sie Benutzer anmelden für eine Enterprise-Anwendung in Azure Active Directory Vorschau

Es ist einfach eine Enterprise-Anwendung deaktivieren, sodass Benutzer sie in Azure Active Directory (Azure AD) Vorschau anmelden können. [Neuigkeiten in der Vorschau](active-directory-preview-explainer.md) Sie müssen die entsprechenden Berechtigungen zum Verwalten von Enterprise-Anwendung. In der aktuellen Vorschau müssen Sie globaler Administrator für das Verzeichnis sein.

## <a name="how-do-i-disable-user-sign-ins"></a>Wie deaktiviere ich Benutzer anmelden?

1. Melden Sie sich bei [Azure-Portal](https://portal.azure.com) mit einem Konto kein globaler Administrator für das Verzeichnis.

2. Wählen Sie **Weitere Dienste**, geben Sie **Azure Active Directory** in das Textfeld ein und Sie dann die **EINGABETASTE**.

3. Der *Verzeichnisname *Azure Active Directory - ** ** Blade (also den Azure AD Blade für das Verzeichnis, das Sie verwalten), wählen Sie **Enterprise Applications **.

    ![Enterprise-apps öffnen](./media/active-directory-coreapps-disable-app-azure-portal/open-enterprise-apps.png)

4. Blade **Anwendung** wählen Sie **Alle Programme**. Finden Sie eine Liste der apps verwalten können.

5. Blade **NTD - alle** Aktivieren einer Anwendung.

6. ***Appname*** Blade (Blade mit dem Namen der ausgewählten Anwendung im Titel) Wählen Sie **Eigenschaften**.

    ![Alle Befehl Anträge](./media/active-directory-coreapps-disable-app-azure-portal/select-app.png)

7. Auf der ***Anwendungsname*** **-Eigenschaften** Blade, wählen Sie **Nein** für **Benutzer anmelden aktiviert?**.

8. Wählen Sie den Befehl **Speichern** .

## <a name="next-steps"></a>Nächste Schritte

- [Alle meine Clubs anzeigen](active-directory-groups-view-azure-portal.md)
- [Eine Enterprise-Anwendung einen Benutzer oder eine Gruppe zuweisen](active-directory-coreapps-assign-user-azure-portal.md)
- [Entfernen Sie Benutzer oder Gruppen Zuweisung aus einer Enterprise-Anwendung](active-directory-coreapps-remove-assignment-azure-portal.md)
- [Ändern des Namens oder Logos eine Enterprise-Anwendung](active-directory-coreapps-change-app-logo-user-azure-portal.md)
