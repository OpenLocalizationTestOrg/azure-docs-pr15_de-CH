<properties
    pageTitle="Azure Active Directory B2C: Benutzerdefinierte Attribute | Microsoft Azure"
    description="Verwendung benutzerdefinierter Attribute in Azure Active Directory B2C Informationen über Ihre Kunden sammeln"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

#  <a name="azure-active-directory-b2c-use-custom-attributes-to-collect-information-about-your-consumers"></a>Azure Active Directory B2C: Verwenden Sie benutzerdefinierte Attribute, um Informationen über Ihre Kunden

Azure Active Directory (Azure AD) B2C-Verzeichnis verfügt über integrierte Informationen (Attribute): Vorname, Nachname, Ort, Postleitzahl und andere Attribute. Allerdings hat jede Anwendung kundenorientierter Anforderungen auf welche Attribute sich von Verbrauchern. Erweitern Sie mit Azure AD B2C Attributen auf jede Kundenkonto gespeichert. Sie können benutzerdefinierte Attribute im [Azure-Portal](https://portal.azure.com/) erstellen und in Ihren Richtlinien Anmeldung verwenden, wie unten dargestellt. Sie können auch lesen und Schreiben diese Attribute mithilfe der [Azure AD Graph-API](active-directory-b2c-devquickstarts-graph-dotnet.md).

> [AZURE.NOTE]
Benutzerdefinierte Attribute verwenden, [Azure AD Graph-API Directory Schema Extensions](https://msdn.microsoft.com/library/azure/dn720459.aspx).

## <a name="create-a-custom-attribute"></a>Benutzerdefiniertes Attribut erstellen

1. [Folgendermaßen B2C Features Blade Azure-Portal navigieren](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klicken Sie auf **Benutzer**.
3. Klicken Sie auf **+ Hinzufügen** oben das Blade.
4. Geben Sie einen **Namen** für das benutzerdefinierte Attribut (z. B. "Schuhgröße") und optional eine **Beschreibung**ein. Klicken Sie auf **Erstellen**.

    > [AZURE.NOTE]
    Nur der "Zeichenfolge" **Datentyp** ist derzeit verfügbar.

Das benutzerdefinierte Attribut steht nun in der Liste der **Benutzerattribute**in Ihre Anmeldung Richtlinien verwendet.

## <a name="use-a-custom-attribute-in-your-sign-up-policy"></a>Verwenden Sie ein benutzerdefiniertes Attribut in der Registrierungsrichtlinie

1. [Folgendermaßen B2C Features Blade Azure-Portal navigieren](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klicken Sie auf **Anmeldung**.
3. Klicken Sie auf die Registrierungsrichtlinie (z. B. "B2C_1_SiUp") zu öffnen. Klicken Sie am oberen Rand der Blade **Bearbeiten** .
4. Klicken Sie auf **Anmeldung Attribute** , und wählen Sie das benutzerdefinierte Attribut (z. B. "Schuhgröße"). Klicken Sie auf **OK**.
5. **Anwendung Ansprüche** auf, und wählen Sie das benutzerdefinierte Attribut. Klicken Sie auf **OK**.
6. Klicken Sie am oberen Rand der Blade **Speichern** .

Die Funktion "Jetzt ausführen" können in der Richtlinie Sie das Kundenerlebnis überprüfen. Sie sollten jetzt finden Sie in der Liste der Attribute während Verbraucher Anmeldung "Schuhgröße", und in Token zurück an die Anwendung gesendet.

## <a name="notes"></a>Notizen

- Benutzerdefinierte Attribute können mit Anmeldung Richtlinien auch anmelden oder Einloggen Richtlinien und Profile Bearbeiten von Richtlinien verwendet werden.
- Es ist eine bekannte Einschränkung von benutzerdefinierten Attributen. Es wird nur beim ersten erstellt alle Richtlinien und nicht beim Hinzufügen zur Liste der **Benutzerattribute**verwendet.
