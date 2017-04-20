<properties
    pageTitle="Azure Active Directory B2C: Self-service Passwort | Microsoft Azure"
    description="Ein Thema demonstriert, wie Sie Self-service für Ihre Kunden in Azure Active Directory B2C zurücksetzen festlegen"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="curtand"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>


# <a name="azure-active-directory-b2c-set-up-self-service-password-reset-for-your-consumers"></a>Azure Active Directory B2C: Self-service-Kennwort zurücksetzen für Ihre Kunden einrichten

Mit dem Feature Self-service-Kennwort zurücksetzen können Ihre Konsumenten (für lokale Konten angemeldet haben) ihre eigenen Kennwörter zurücksetzen. Dies reduziert die Belastung für das Supportpersonal vor allem, wenn Ihre Anwendung Millionen Kunden regelmäßig verwenden. Derzeit unterstützen wir nur überprüft e-Mail-Adresse als eine Wiederherstellungsmethode verwenden. Wir werden in Zukunft weitere Wiederherstellungsmethoden (bestätigte Telefonnummer, Fragen usw.) hinzufügen.

> [AZURE.NOTE]
Dieser Artikel gilt für Self-service-Kennwort zurücksetzen im Kontext einer Richtlinie verwendet. Wenn Sie anpassbare Kennwort zurücksetzen Richtlinien aus Ihrer Anwendung aufgerufen, [Artikel](./active-directory-b2c-reference-policies.md#create-a-password-reset-policy).

In der Standardeinstellung das Verzeichnis keinen Self-service-Kennwort zurücksetzen aktiviert. Gehen Sie folgendermaßen vor, um es zu aktivieren:

1. Melden Sie sich bei [Azure-Verwaltungsportal](https://manage.windowsazure.com/) als Abonnement-Administrator an. Dies ist die gleiche Arbeit oder schulkonto oder das Microsoft-Konto, mit dem Sie das Verzeichnis erstellen.
2. Navigieren Sie zu der Active Directory-Erweiterung auf der Navigationsleiste auf der linken Seite.
3. Das Verzeichnis unter der Registerkarte **Verzeichnis** und klicken Sie darauf.
4. Klicken Sie auf die Registerkarte **Konfigurieren** .
5. Abschnitt **Benutzerkennwort zurücksetzen Richtlinie** Scrollen Sie und schalten Sie die Option **Benutzer Kennwort zurücksetzen** auf **Ja**. Beachten Sie, dass die Option **Alternative e-Mail-Adresse** . Lassen sie unverändert.

    ![Self-service-Kennwort zurücksetzen](./media/active-directory-b2c-reference-sspr/sspr.png)

6. Klicken Sie am unteren Rand der Seite **Speichern** . Sie haben es geschafft!

Testen, verwenden Sie die Funktion "Jetzt ausführen" für alle, die lokale Konten als Identitätsanbieter Richtlinien. Das lokale Konto anmelden Seite (wo Sie eine e-Mail-Adresse und Kennwort oder Benutzername und Kennwort eingeben), klicken Sie auf **kann nicht auf Ihr Konto zugreifen?** Benutzerfunktionen zu überprüfen.

> [AZURE.NOTE]
Self-service-Kennwort zurücksetzen Seiten können [Unternehmen branding Feature](../active-directory/active-directory-add-company-branding.md)angepasst werden.
