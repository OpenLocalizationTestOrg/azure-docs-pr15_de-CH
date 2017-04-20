<properties
    pageTitle="Azure Active Directory B2C: Erstellen eine Azure Active Directory B2C Mieter | Microsoft Azure"
    description="Ein Thema zur Verwendung einen Azure Active Directory B2C-Mandanten erstellen"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.topic="article"
    ms.devlang="na"
    ms.date="08/30/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-create-an-azure-ad-b2c-tenant"></a>Azure Active Directory B2C: Erstellen einer Azure AD B2C Mieters

Um mit Microsoft Azure Active Directory (Azure AD) B2C, führen Sie die drei in diesem Artikel beschriebenen Schritte.

## <a name="step-1-sign-up-for-an-azure-subscription"></a>Schritt 1: Für Azure-Abonnement anmelden

Wenn Sie bereits ein Azure-Abonnement haben, überspringen Sie diesen Schritt. Wenn nicht, für ein [Azure-Abonnement](../active-directory/sign-up-organization.md) anmelden und Zugriff auf Azure AD B2C.

## <a name="step-2-create-an-azure-ad-b2c-tenant"></a>Schritt 2: Erstellen einer Azure AD B2C Mieters

Gehen Sie folgendermaßen vor, einen neuen Azure AD B2C-Mandanten erstellen. Derzeit werden nicht B2C-Funktionen in vorhandenen Mandanten aktiviert.

1. Melden Sie sich bei [Azure-Verwaltungsportal](https://manage.windowsazure.com/) als Abonnement-Administrator an. Dies ist die gleiche Arbeit oder schulkonto oder das gleiche Microsoft-Konto für Azure anmelden verwendet.
2. **Klicken Sie auf** > **App Services** > **Active Directory** > **Verzeichnis** > **benutzerdefinierte**.

    ![Screenshot starten einen Mandanten erstellen](./media/active-directory-b2c-get-started/new-directory.png)

3. Wählen Sie den **Namen**, **Domänennamen** und **Land oder Region** für Ihren Mandanten.
4. Überprüfen Sie die Option **Dies ist ein B2C-Verzeichnis**.
5. Klicken Sie auf das Häkchen, um die Aktion abzuschließen.

    ![Screenshot des Häkchens ein B2C-Verzeichnis erstellen](./media/active-directory-b2c-get-started/create-b2c-directory.png)

6. Der Mieter wird erstellt und in der Active Directory-Erweiterung angezeigt. Sie werden außerdem ein globaler Administrator des Mandanten erstellt. Sie können nach Bedarf andere globale Administratoren hinzufügen.

    > [AZURE.IMPORTANT]
    Falls Sie B2C Mieter für eine Produktion app verwenden, lesen Sie den Artikel auf [Produktions - und Vorschau B2C-Mandanten](active-directory-b2c-reference-tenant-type.md). Hinweis es bekannte Probleme bei einen vorhandenen B2C-Mandanten löschen und erneut mit dem gleichen Domänennamen erstellen. Sie haben einen B2C-Mandanten mit einem anderen Domänennamen erstellen.

## <a name="step-3-navigate-to-the-b2c-features-blade-on-the-azure-portal"></a>Schritt 3: B2C Features Blatt Azure-Portal wechseln

1. Navigieren Sie zu der Active Directory-Erweiterung auf der Navigationsleiste auf der linken Seite.
2. Der Mieter Registerkarte **Verzeichnis** und klicken Sie darauf.
3. Klicken Sie auf die Registerkarte **Konfigurieren** .
4. Klicken Sie auf **Verwalten B2C-Einstellungen** , im Bereich **B2C-Verwaltung** .

    ![Screenshot der B2C-Verzeichniskonfiguration](./media/active-directory-b2c-get-started/b2c-directory-configure-tab.png)

5. Azure-Portal mit B2C-Features anzeigen wird in einer neuen Registerkarte oder Fenster geöffnet.

    > [AZURE.IMPORTANT]
    Es dauert bis zu 2 bis 3 Minuten für Ihren Mandanten Azure-Portal zugänglich. Wiederholen diese Schritte nach einiger Zeit dies beheben. Wenn dies nicht der Fall ist, wenden Sie sich an Support.

6. Diese Blade in Ihrem Startmenü für einfachen Zugriff zu fixieren. (Pin-Werkzeug wird in der oberen rechten Ecke des Blatts Funktionen rot markiert.)

    ![Screenshot des Blades B2C-Funktionen](./media/active-directory-b2c-get-started/b2c-features-blade.png)

    > [AZURE.NOTE]
    Sie können Benutzer und Gruppen, Self-service-Kennwort zurücksetzen Konfiguration und Unternehmen branding Funktionen von Ihrem Mandanten im [klassischen Azure-Portal](https://manage.windowsazure.com/)verwalten.

## <a name="easy-access-to-the-b2c-features-blade-on-the-azure-portal"></a>Einfachen Zugriff auf das B2C Features Blade Azure-Portal

Um die Auffindbarkeit zu verbessern, haben wir eine Verknüpfung B2C Features Blade Azure-Portal hinzugefügt.

1. Melden Sie sich bei Azure-Portal als globaler Administrator die B2C-Mandanten. Wenn Sie in einem anderen Mandanten angemeldet sind, wechseln Sie Mieter (in der oberen rechten Ecke).
2. Klicken Sie in der linken Navigationsleiste auf **Durchsuchen** .
3. Klicken Sie auf **Azure AD B2C** um das Blade B2C-Funktionen zuzugreifen.

    ![Screenshot des durchsuchen B2C Features Blatt](./media/active-directory-b2c-get-started/b2c-browse.png)

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Registrieren einer Anwendung bei Azure AD B2C und Schnellstart Anwendung lesen erstellen [Azure Active Directory B2C: Registrieren Sie die Anwendung](active-directory-b2c-app-registration.md).
