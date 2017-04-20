<properties
    pageTitle="Azure Active Directory Vorschau der benutzerdefinierten Domänennamen hinzufügen | Microsoft Azure"
    description="Domänennamen Ihres Unternehmens in Azure Active Directory hinzufügen, und überprüfen Sie den Domänennamen ein."
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
    ms.date="10/17/2016"
    ms.author="curtand"/>

# <a name="add-a-custom-domain-name-to-azure-active-directory-preview"></a>Azure Active Directory Vorschau einen benutzerdefinierten Domänennamen hinzufügen

> [AZURE.SELECTOR]
- [Azure-portal](active-directory-domains-add-azure-portal.md)
- [Azure-Verwaltungsportal](active-directory-add-domain.md)

Sie haben eine oder mehrere Domänennamen, die Ihre Organisation verwendet Geschäften Benutzer zum Firmennetzwerk Unternehmensnetzwerk Domänennamen anmelden. Azure Active Directory (Azure AD) Vorschau können Sie Ihr Unternehmen Domänennamen sowie Azure AD hinzufügen. [Neuigkeiten in der Vorschau](active-directory-preview-explainer.md) Dadurch zuweisen wie den Benutzern vertraut sind Benutzernamen im Verzeichnis ‘alice@contoso.com.’ der Vorgang ist einfach:

1. Hinzufügen von benutzerdefinierten Domänennamen zum Verzeichnis
2. Fügen Sie einen DNS-Eintrag für den Domänennamen auf der Domänennamen-Registrierungsstelle
3. Überprüfen Sie den Domainnamen in Azure AD

## <a name="how-do-i-add-a-domain-name"></a>Wie fügen Sie einen Domänennamen

1.  Melden Sie sich bei [Azure-Portal](https://portal.azure.com) mit einem Konto kein globaler Administrator für das Verzeichnis.

2.  Wählen Sie **Weitere Dienste**, geben Sie **Azure Active Directory** in das Textfeld ein und Sie dann die **EINGABETASTE**.

    ![Verwaltung öffnen](./media/active-directory-domains-add-azure-portal/user-management.png)

3. Wählen Sie auf Blade ***Verzeichnisnamen*** **Domänennamen**.

4. Wählen Sie auf dem Blatt ** *Namen Verzeichnis* - Domain-Namen** **Hinzufügen** .

  ![Befehl hinzufügen](./media/active-directory-domains-add-azure-portal/add-command.png)

5. Geben Sie auf den **Domänennamen** den Namen der benutzerdefinierten Domäne im Feld z. B. "contoso.com" und wählen Sie dann die **Domäne hinzufügen**. Unbedingt .com, .net oder eine andere Erweiterung der obersten Ebene.

6. Abzurufen Sie auf der ***Domänenname*** (d. h. das Blade, das geöffnet wird, die neuen Domänennamen im Titel), die DNS-Eintragsinformationen, die Azure AD verwendet wird, um sicherzustellen, dass den benutzerdefinierten Domänennamen im Besitz der Organisation.

  ![DNS-Informationen abrufen](./media/active-directory-domains-add-azure-portal/get-dns-info.png)

Der Domänenname hinzugefügt haben, muss im Besitz der Organisation des Domänennamens Azure AD überprüfen. Bevor Azure AD diese Überprüfung durchführen kann, müssen Sie einen DNS-Eintrag in der Zonendatei DNS-Domänennamen hinzufügen. Diese Aufgabe wird auf der Website für Domänennamen-Registrierungsstelle für den Domänennamen.

## <a name="add-the-dns-entry-at-the-domain-name-registrar-for-the-domain"></a>Den DNS-Eintrag in der Domänennamen-Registrierungsstelle für die Domäne hinzufügen

Nächstes Azure AD benutzerdefinierten Domänennamen mit wird die DNS-Zonendatei der Domäne aktualisieren. Dies ermöglicht Azure AD zu überprüfen, ob den benutzerdefinierten Domänennamen im Besitz der Organisation.

1.  Mit dem Domänennamen-Registrierungsstelle für die Domäne anmelden. Haben Sie Zugriff auf den DNS-Eintrag aktualisieren, Fragen Sie Personen oder Teams, die diesen Zugriff Schritt 2 und wann sie abgeschlossen wird.

2.  Aktualisieren der DNS-Zonendatei für die Domäne den DNS-Eintrag von Azure AD für Sie bereitgestellt. Dieser DNS-Eintrag ermöglicht Azure AD der Besitzer der Domäne überprüfen. Der DNS-Eintrag ändern nicht Verhaltensweisen wie e-Mail-Nachrichten oder Webhosting.

Hilfe zu dieser DNS-Eintrag hinzufügen Anleitung [zum Hinzufügen eines DNS-Eintrags in beliebte DNS Registrierstellen](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-the-domain-name-with-azure-ad"></a>Überprüfen Sie den Domänennamen mit Azure

Nachdem Sie den DNS-Eintrag hinzugefügt haben, können Sie den Domänennamen mit Azure überprüfen.

Ein Domänenname kann überprüft werden, wenn die DNS-Datensätze weitergegeben wurden. Diese Propagierung häufig dauert nur wenige Sekunden, aber kann manchmal dauert eine Stunde oder mehr. Wenn Prüfung zum erste Mal nicht funktioniert, versuchen Sie es erneut.

1.  Melden Sie sich bei [Azure-Portal](https://portal.azure.com) mit einem Konto kein globaler Administrator für das Verzeichnis.

2.  Wählen Sie **Durchsuchen**, geben Sie User Management in das Textfeld ein und Sie dann die **EINGABETASTE**.

    ![Verwaltung öffnen](./media/active-directory-domains-add-azure-portal/user-management.png)

3. Wählen Sie auf Blade **User Management - Domänennamen** nicht überprüfte Domänennamen, den Sie überprüfen möchten.

4. Wählen Sie auf ***Domänenname*** Blade (d. h. das Blade, das geöffnet wird, die neuen Domänennamen im Titel) **Überprüfen** Überprüfung abgeschlossen.

Jetzt können Sie [die benutzerdefinierten Domänennamen enthalten Namen zuweisen](active-directory-users-create-azure-portal.md).

## <a name="troubleshooting"></a>Problembehandlung

Versuchen Sie Folgendes, wenn Sie einen benutzerdefinierten Domänennamen überprüfen können. Wir beginnen mit die häufigste und auf mindestens häufig arbeiten.

1.  **Eine Stunde dauert**. DNS-Datensätze müssen verbreiten bevor Azure AD Domäne überprüfen kann. Dies kann eine Stunde oder länger dauern.

2.  **Sicherstellen, dass der DNS-Eintrag eingegeben wurde, und richtig ist**. Führen Sie diesen Schritt auf der Website für den Domänennamen-Registrierungsstelle für die Domäne. Azure AD kann den Domänennamen nicht überprüfen, falls der DNS-Eintrag nicht in der DNS-Zone-Datei ist, oder wenn es keine genaue Übereinstimmung mit den DNS-Eintrag, Azure AD. Wenn Sie keinen Zugriff auf DNS-Einträge für die Domäne den Domänennamen-Registrierungsstelle aktualisieren Personen oder Teams in der Organisation Zugriff hat den DNS-Eintrag freigeben und den DNS-Eintrag hinzufügt.

3.  **Löschen Sie den Domänennamen aus einem anderen Verzeichnis in Azure AD**. Ein Domänenname kann in einem einzelnen Verzeichnis überprüft. Wenn ein Domänenname in einem anderen Verzeichnis festgestellt wurde, müssen sie es gelöscht bevor in Ihrem neuen Verzeichnis überprüft werden kann. Informationen zum Löschen von Domänennamen finden Sie [benutzerdefinierten Domänennamen verwalten](active-directory-domains-manage-azure-portal.md).    

## <a name="add-more-custom-domain-names"></a>Weitere benutzerdefinierte Domänennamen hinzufügen

Wenn Ihre Organisation mehrere benutzerdefinierte Domänennamen "contoso.com" und "contosobank.com" können sie maximal 900 Domänennamen hinzufügen. Verwenden Sie die gleichen Schritte in diesem Artikel jeweils den Domänennamen hinzufügen.

## <a name="next-steps"></a>Nächste Schritte

[Verwalten von benutzerdefinierten Domänennamen](active-directory-domains-manage-azure-portal.md)
