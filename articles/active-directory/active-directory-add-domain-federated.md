<properties
    pageTitle="Ihre benutzerdefinierte Domain-Namen und verbundenen anmelden Azure Active Directory einrichten | Microsoft Azure"
    description="Domänennamen Ihres Unternehmens in Azure Active Directory hinzufügen und wie Verbund zwischen Azure Active Directory und die Lösung lokal anmelden."
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
    ms.topic="get-started-article"
    ms.date="10/04/2016"
    ms.author="curtand;jeffsta"/>

# <a name="add-your-custom-domain-name-to-azure-active-directory"></a>Hinzufügen der benutzerdefinierten Domänennamen zu Azure Active Directory

Sie können einen benutzerdefinierten Domänennamen, z. B. "contoso.com" konfigurieren, sodass Benutzer auf contoso.com eine federated einzelne Anmeldung im Unternehmensnetzwerk haben. Haben Sie bereits Active Directory Federation Services (AD FS) oder anderen Verbundserver auf das Unternehmensnetzwerk können Sie Azure AD Verwendung der benutzerdefinierten Domänennamen Werkzeug Azure AD Connect konfigurieren. Sie können mithilfe von Azure AD Connect eine neue AD FS-Umgebung bereitstellen und konfigurieren, die für föderierte einmaliges Azure AD.

Wenn Sie keinen und nicht AD FS oder anderen Verbundserver bereitstellen möchten, folgen Sie dieser Anleitung: [Hinzufügen einer benutzerdefinierten Domänennamen in Azure Active Directory](active-directory-add-domain.md).

## <a name="add-a-custom-domain-name-to-your-directory"></a>Fügen Sie einen benutzerdefinierten Domänennamen zum Verzeichnis hinzu

1. Melden Sie sich bei der [Azure-Verwaltungsportal](https://manage.windowsazure.com/) ein Benutzerkonto, ein globaler Administrator Azure AD-Verzeichnis ist.

2. Öffnen Sie in **Active Directory**das Verzeichnis, und wählen Sie die Registerkarte **Domänen** .

3. Die Befehlsleiste **Hinzufügen Sie**, und geben Sie den Namen Ihrer benutzerdefinierten Domäne z. B. "contoso.com". Unbedingt .com, .net oder eine andere Erweiterung der obersten Ebene.

4. Aktivieren Sie das Kontrollkästchen **ich für einmaliges Anmelden mit meinem lokalen Active Directory-Domäne konfigurieren möchten** .

5. Wählen Sie **Hinzufügen**.

Führen Sie das Tool Azure AD Connect zu Azure AD mit der Domäne überprüfen den DNS-Eintrag. Den DNS-Eintrag in der **AD-Domäne Azure** Schritt des Assistenten wird angezeigt. Sehen Sie nach diesem Schritt des Assistenten sieht aus wie [in dieser Anleitung](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation). Wenn Sie keinen Azure AD Connect-Tool können Sie [hier downloaden](http://go.microsoft.com/fwlink/?LinkId=615771).

## <a name="add-the-dns-entry-at-the-domain-name-registrar-for-the-domain"></a>Den DNS-Eintrag in der Domänennamen-Registrierungsstelle für die Domäne hinzufügen

Nächstes Azure AD benutzerdefinierten Domänennamen mit wird die DNS-Zonendatei der Domäne aktualisieren. Dies ermöglicht Azure AD zu überprüfen, ob den benutzerdefinierten Domänennamen im Besitz der Organisation.

1. Melden Sie sich bei der Website für Domänennamen-Registrierungsstelle für Ihren Domänennamen. Wenden Sie Zugang dazu haben, Personen oder Teams in der Organisation, die diesen Zugriff Schritt 2 und wann sie abgeschlossen wird.

2. Aktualisieren der DNS-Zonendatei für die Domäne den DNS-Eintrag von Azure AD für Sie bereitgestellt. Dieser DNS-Eintrag ermöglicht Azure AD der Besitzer der Domäne überprüfen. Der DNS-Eintrag ändern nicht Verhaltensweisen wie e-Mail-Nachrichten oder Webhosting.

Hilfe zu diesem Schritt Anleitung [zum Hinzufügen eines DNS-Eintrags in beliebte DNS Registrierstellen](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-the-domain-name-with-azure-ad"></a>Überprüfen Sie den Domänennamen mit Azure

Nachdem Sie den DNS-Eintrag hinzugefügt haben, können Sie den Domänennamen mit Azure überprüfen.

Überprüfen Sie die Domäne wählen Sie **Azure AD-Domäne** Schritt des Assistenten Azure AD Connect **Weiter** . Azure AD sucht nach den DNS-Eintrag in der DNS-Zonendatei für die Domäne. Azure AD nur überprüfen, ob der Domänenname DNS-Datensätze weitergegeben wurden. Verteilung oft nur Sekunden, jedoch können manchmal dauert eine Stunde oder mehr. Wenn Prüfung zum erste Mal nicht funktioniert, versuchen Sie es erneut.

Fahren Sie die verbleibenden Schritte im Assistenten Azure AD verbinden. Dies wird Benutzern von Windows Server Active Directory Azure AD synchronisieren. Synchronisierte Benutzer in der Domäne, die für die Föderation konfiguriert werden zu einer verbundenen einzelne Anmeldung vom Firmennetzwerk Azure AD.

## <a name="troubleshooting"></a>Problembehandlung

Versuchen Sie Folgendes, wenn Sie einen benutzerdefinierten Domänennamen überprüfen können. Wir beginnen mit die häufigste und auf mindestens häufig arbeiten.

1.  **Eine Stunde dauert**. DNS-Datensätze müssen verbreiten bevor Azure AD Domäne überprüfen kann. Dies kann eine Stunde oder länger dauern.

2.  **Sicherstellen, dass der DNS-Eintrag eingegeben wurde, und richtig ist**. Führen Sie diesen Schritt auf der Website für den Domänennamen-Registrierungsstelle für die Domäne. Azure AD kann den Domänennamen nicht überprüfen, falls der DNS-Eintrag nicht in der DNS-Zone-Datei ist, oder wenn es keine genaue Übereinstimmung mit den DNS-Eintrag, Azure AD. Wenn Sie keinen Zugriff auf DNS-Einträge für die Domäne den Domänennamen-Registrierungsstelle aktualisieren Personen oder Teams in der Organisation Zugriff hat den DNS-Eintrag freigeben und den DNS-Eintrag hinzufügt.

3.  **Löschen Sie den Domänennamen aus einem anderen Verzeichnis in Azure AD**. Ein Domänenname kann in einem einzelnen Verzeichnis überprüft. Wenn ein Domänenname in einem anderen Verzeichnis festgestellt wurde, müssen sie es gelöscht bevor in Ihrem neuen Verzeichnis überprüft werden kann. Informationen zum Löschen von Domänennamen finden Sie [benutzerdefinierten Domänennamen verwalten](active-directory-add-manage-domain-names.md).

## <a name="add-more-custom-domain-names"></a>Weitere benutzerdefinierte Domänennamen hinzufügen

Wenn Ihre Organisation mehrere benutzerdefinierte Domänennamen "contoso.com" und "contosobank.com" können sie maximal 900 Domänennamen hinzufügen. Verwenden Sie die gleichen Schritte in diesem Artikel jeweils den Domänennamen hinzufügen.

## <a name="next-steps"></a>Nächste Schritte

-   [Verwalten von benutzerdefinierten Domänennamen](active-directory-add-manage-domain-names.md)
-   [Erfahren Sie mehr über Managementkonzepte in Azure AD](active-directory-add-domain-concepts.md)
-   [Zeigen Sie Ihres Unternehmens OEM-branding beim Benutzer anmelden an](active-directory-add-company-branding.md)
-   [Mithilfe von PowerShell Domänennamen in Azure AD verwalten](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
