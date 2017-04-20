<properties
pageTitle="Anpassen der Anmeldeseite in Azure Active Directory Vorschau | Microsoft Azure"
description="Informationen Sie zum Unternehmensbranding auf der Azure-Anmeldeseite hinzufügen"
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
ms.date="09/30/2016"
ms.author="curtand"/>

# <a name="add-company-branding-to-your-sign-in-page-in-the-azure-active-directory-preview"></a>Firma branding auf der Anmeldeseite in Azure Active Directory-Vorschau

Um Verwirrung zu vermeiden, möchten viele Unternehmen übernehmen ein einheitliches Erscheinungsbild für alle Websites und Dienste, die sie verwalten. Azure Active Directory Vorschau bietet diese Funktion auf der Seite mit Ihrem Firmenlogo und benutzerdefinierte Farben anpassen. [Neuigkeiten in der Vorschau](active-directory-preview-explainer.md) Die Seite ist die Seite Anmeldung bei Office 365 oder andere webbasierte Programme, die Azure AD als der Identitätsanbieter verwenden. Sie interagieren mit dieser Seite, um Ihre Anmeldeinformationen einzugeben.

Auf dieser Seite Ihr Firmenlogo, Farben und andere anpassbare Elemente anzeigen, finden Sie die folgenden Bilder Unterschied zwischen zwei Erfahrungen.

Der folgende Screenshot zeigt und beispielsweise die Office 365-Anmeldeseite auf einem desktop-Computer **vor** einer Anpassung:

![Office 365-Anmeldeseite vor der Anpassung](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-before-customization.png)

Der folgende Screenshot zeigt und beispielsweise die Office 365-Anmeldeseite auf einem desktop-Computer **nach** einer Anpassung:

![Office 365-Anmeldeseite nach Anpassung](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-after-customization.png)


## <a name="customizing-the-sign-in-page"></a>Anpassen der Seite

Normalerweise benötigen Sie Browser-basierten Zugriff auf Ihre Cloud-apps und Services Ihrer Organisation abonniert, verwenden Sie die Seite.

Wenn Änderungen auf der Seite angewendet haben, dauert es bis zu einer Stunde geändert werden.

Branded Anmeldeseite wird nur angezeigt, wenn Servicebesuchs mandantenspezifische URL wie https://outlook.com/**Contoso**.com oder https://mail. **Contoso**. com.

Einen Dienst mit bestimmten URLs nicht Mieter Besuch (z.B.: https://mail.office365.com), eine nicht gebrandete Anmelden angezeigt. In diesem Fall wird Ihr branding eingegebenen Mitgliedsnamen oder Sie haben ein Anmeldebild ausgewählt.

> [AZURE.NOTE]
>
- Der Domänenname muss angezeigt als "Aktiv" im Abschnitt **Domänen** in der Konfiguration Azure-Portal branding. Weitere Informationen finden Sie unter [benutzerdefinierte Domänennamen hinzufügen](active-directory-domains-add-azure-portal.md).
- Branding-Anmeldeseite übernommen nicht auf der Anmeldeseite Consumer von Microsoft. Wenn Sie ein Microsoft-Konto anmelden, finden Sie eine branded Liste der gerenderten von Azure AD Benutzer jedoch branding Ihrer Organisation gilt nicht für das Microsoft-Konto-Anmeldeseite.

Auf der Anmeldeseite erlaubt das Kontrollkästchen **Ich möchte eingeloggt bleiben** angemeldet bleiben, schließen und öffnen Sie ihren Browser erneut. 

   ![Angemeldet bleiben](./media/active-directory-branding-custom-signon-azure-portal/01.png)

Es wirkt sich nicht auf Sitzungsdauer. Sie können das Kontrollkästchen auf der Azure Active Directory-Seite ausblenden.
Gibt an, ob das Kontrollkästchen angezeigt wird, hängt von der Einstellung der **deaktiviert eingeloggt bleiben**.

   ![Angemeldet bleiben](./media/active-directory-branding-custom-signon-azure-portal/02.png)


Ausblenden das Kontrollkästchen Diese Einstellung auf **Ja**. 

> [AZURE.NOTE] Einige Features von SharePoint Online und Office 2010 hängen Benutzer können dieses Kontrollkästchen aktivieren. Wenn Sie diese Einstellung ausgeblendet konfigurieren, sehen die Benutzer zusätzliche und unerwartete Aufforderung zum Anmelden.




**Firmenlogo zum Verzeichnis hinzu**

1.  Melden Sie sich bei [Azure-Portal](https://portal.azure.com) mit einem Konto kein globaler Administrator für das Verzeichnis.

2.  Wählen Sie **Weitere Dienste**, geben Sie im Textfeld **Benutzer und Gruppen** und wählen Sie dann die **EINGABETASTE**.

    ![Verwaltung öffnen](./media/active-directory-branding-custom-signon-azure-portal/user-management.png)

3. **Benutzer und Gruppen** Blade wählen Sie **Unternehmen branding**.

4. Auf der **Benutzer und Gruppen - branding des Unternehmens** Blade, wählen den Befehl **Bearbeiten** .

    ![Kundenspezifisches bearbeiten](./media/active-directory-branding-custom-signon-azure-portal/edit-branding.png)

5. Ändern Sie die Elemente, die Sie anpassen möchten. Alle Elemente sind optional.

6. Klicken Sie auf **Speichern**.

Sie können Stunde Änderungen dauern an Anmeldeseite um zu erscheinen.

## <a name="next-steps"></a>Nächste Schritte

[Fügen Sie sprachspezifische Firmenlogo hinzu](active-directory-branding-localize-azure-portal.md)
