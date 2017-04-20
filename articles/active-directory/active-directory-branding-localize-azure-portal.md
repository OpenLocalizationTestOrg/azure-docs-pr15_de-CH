<properties
pageTitle="Sprachspezifische Unternehmensbranding auf der Anmeldeseite in Azure Active Directory Vorschau hinzufügen | Microsoft Azure"
description="Informationen zum Hinzufügen eines Sprache Mandanten branding Bilder und Text ein Azure-Anmeldeseite"
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
ms.date="09/12/2016"
ms.author="curtand"/>

# <a name="add-language-specific-company-branding-to-your-sign-in-page-in-the-azure-active-directory-preview"></a>Sprachspezifische Firma branding auf der Anmeldeseite in Azure Active Directory-Vorschau

Um Verwirrung zu vermeiden, möchten viele Unternehmen übernehmen ein einheitliches Erscheinungsbild für alle Websites und Dienste, die sie verwalten. Azure Active Directory Vorschau bietet diese Funktion auf der Seite mit Ihrem Firmenlogo und benutzerdefinierte Farben anpassen. [Neuigkeiten in der Vorschau](active-directory-preview-explainer.md) Die Seite ist die Seite Anmeldung bei Office 365 oder andere webbasierte Programme, die Azure AD als der Identitätsanbieter verwenden. Sie interagieren mit dieser Seite, um Ihre Anmeldeinformationen einzugeben.

## <a name="customizing-the-sign-in-page-for-another-language"></a>Die Anmeldeseite für eine andere Sprache anpassen

Sie können Ihre benutzerdefinierten Anmeldeseite sprachspezifische Elemente hinzufügen, nur, wenn Sie eine benutzerdefinierte Anmeldeseite als im [Unternehmen branding Ihrer Anmeldeseite hinzufügen](active-directory-branding-custom-signon-azure-portal.md)bereits erstellt haben. Sie können eine Anmeldeseite pro Verzeichnis mit anpassbaren Elemente konfigurieren. Nachdem Sie den Standardsatz von Seitenelementen konfiguriert haben, können Sie mehrere Versionen für unterschiedliche Gebietsschemas konfigurieren. Sie können auch und verschiedene Elemente. Beispielsweise können Sie:

- Erstellen einer **-im Bild** , die funktioniert für alle Kulturen erstellen Versionen für Englisch und Französisch. Beim Festlegen der Browser zu einer dieser beiden Sprachen wird das Bild sprachspezifischen, während Standard-Abbildung für andere Sprachen wird angezeigt.

- Konfigurieren Sie unterschiedliche Logos für Ihre Organisation (z. B. Japanisch oder Hebräisch Versionen).

Wir empfehlen die Anzahl Sprachvarianten für Wartung und Leistung gering halten.

**Firmenlogo zum Verzeichnis hinzu**

1.  Melden Sie sich bei [Azure-Portal](https://portal.azure.com) mit einem Konto kein globaler Administrator für das Verzeichnis.

2.  Wählen Sie **Weitere Dienste**, geben Sie im Textfeld **Benutzer und Gruppen** und wählen Sie dann die **EINGABETASTE**.

    ![Verwaltung öffnen](./media/active-directory-branding-localize-azure-portal/user-management.png)

3. **Benutzer und Gruppen** Blade wählen Sie **Unternehmen branding**.

4. Auf der **Benutzer und Gruppen - branding des Unternehmens** Blade, wählen Sie die **Sprache hinzufügen** .

    ![Sprachspezifische Brandingelemente hinzufügen](./media/active-directory-branding-localize-azure-portal/add-language.png)

5. Ändern Sie die Elemente, die Sie anpassen möchten. Alle Elemente sind optional.

6. Klicken Sie auf **Speichern**.

Sie können Stunde Änderungen dauern an Anmeldeseite um zu erscheinen.

## <a name="next-steps"></a>Nächste Schritte

[Auf der Seite branding Firma](active-directory-branding-custom-signon-azure-portal.md)
