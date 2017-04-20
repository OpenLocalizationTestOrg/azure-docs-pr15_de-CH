<properties
    pageTitle="Freigeben einer Azure AD-Mandanten über Office 365 und Azure-Abonnements | Microsoft Azure"
    description="Erfahren Sie, wie Ihre Office 365 Azure AD-Mandanten und Benutzer mit Ihrem Azure-Abonnement freigeben oder umgekehrt"
    services=""
    documentationCenter=""
    authors="JiangChen79"
    manager="mbaldwin"
    editor=""
    tags="billing,top-support-issue"/>

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="cjiang"/>

# <a name="use-an-existing-office-365-account-with-your-azure-subscription-or-vice-versa"></a>Verwenden Sie ein vorhandenes Office 365-Konto mit Ihrem Azure-Abonnement oder umgekehrt
Szenario: Sie bereits ein Office 365-Abonnement und Azure-Abonnement können jedoch vorhandenen Office 365 Benutzerkonten für Ihre Azure-Abonnement verwenden möchten. Alternativ Azure Abonnent sind und ein Office 365-Abonnement für die Benutzer in Ihrem vorhandenen Azure Active Directory abrufen möchten. Dieser Artikel beschreibt, wie einfach es ist beides erreichen.

> [AZURE.NOTE] Dieser Artikel gilt nicht für Konzernvertrag (EA) Kunden. Benötigen Sie weitere Informationen in diesem Artikel [wenden Sie](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) jederzeit zu Ihrem Problem schnell gelöst.


## <a name="quick-guidance"></a>Kurze Anleitung

- Wenn Sie bereits ein Office 365-Abonnement und bei Azure anmelden möchten, verwenden Sie die Option **Konto Ihrer Organisation anmelden** . Fahren Sie den Azure Anmeldevorgang mit Ihrem Office 365-Konto. [Detaillierte Schritte in diesem Artikel](#s1)angezeigt.

- Wenn Sie bereits ein Azure-Abonnement haben und ein Office 365-Abonnement erhalten möchten, melden Sie sich Office 365 mit Ihrem Azure-Konto. Fahren Sie mit den Schritten Anmeldung. Nach Abschluss der Anmeldung wird Office 365-Abonnement dieselbe Active Directory Azure-Instanz hinzugefügt, die Ihre Azure-Abonnement zugeordnet. Weitere Informationen finden Sie im Abschnitt [detaillierte Schritte in diesem Artikel](#s2).

>[AZURE.NOTE] Zu Office 365-Abonnement die Konten verwenden Sie für die Anmeldung muss Mitglied der globale Administrator oder Admin Abrechnung Verzeichnis in Ihrem Mandanten Azure Active Directory. [Erfahren Sie, wie die Rolle in Azure Active Directory zu bestimmen](#how-to-know-your-role-in-your-azure-active-directory).

Was geschieht, wenn Sie ein Konto ein Abonnement hinzufügen, finden Sie die [Informationen später in diesem Artikel](#background-information).

## <a name="detailed-steps"></a>Ausführliche Anleitung
<a id="s1"></a>
### <a name="scenario-1-office-365-users-who-plan-to-buy-azure"></a>Szenario 1: Office 365 Benutzer, Azure kaufen
In diesem Szenario gehen wir Kelley Wall ist ein Benutzer, ein Office 365-Abonnement, und Abonnieren von Azure planen. Es gibt zwei zusätzliche aktive Benutzer Andrea und Katrin. Kelley Konto admin@contoso.onmicrosoft.com.

![Office 365 Benutzer Administrationscenter](./media/billing-use-existing-office-365-account-azure-subscription/1-office365-users-admin-center.png)

Um Azure anzumelden, gehen Sie folgendermaßen vor:

1. Melden Sie sich bei Azure unter [Azure.com](https://azure.microsoft.com/). Klicken Sie auf **ausprobieren**. Klicken Sie auf der nächsten Seite auf **jetzt starten**.

    ![Testen Sie Azure kostenlos.](./media/billing-use-existing-office-365-account-azure-subscription/2-azure-signup-try-free.png)

2. Klicken Sie auf **Konto Ihrer Organisation anmelden**.

    ![Melden Sie sich bei Azure.](./media/billing-use-existing-office-365-account-azure-subscription/3-sign-in-to-azure.png)

3. Ihr Office 365-Konto anmelden. In diesem Fall ist Kelleys Office 365-Konto.

    ![Ihr Office 365-Konto anmelden.](./media/billing-use-existing-office-365-account-azure-subscription/4-sign-in-with-org-account.png)

4. Geben Sie die Informationen und registrieren.

    ![Geben Sie Informationen und Abschließen Sie Anmeldung.](./media/billing-use-existing-office-365-account-azure-subscription/5-azure-sign-up-fill-information.png)

    ![Klicken Sie auf meinem Dienst verwalten.](./media/billing-use-existing-office-365-account-azure-subscription/6-azure-start-managing-my-service.png)

Jetzt sind Sie alle festlegen. In Azure-Portal erhalten Sie die gleichen Benutzer angezeigt wird. Gehen Sie hierzu folgendermaßen vor:

1. Klicken Sie in der zuvor gezeigte Bildschirm **Verwalten Serviceinformationen** .
2. Klicken Sie auf **Durchsuchen**, und klicken Sie dann auf **Active Directory**.

    ![Klicken Sie auf Durchsuchen, und klicken Sie dann auf Active Directory.](./media/billing-use-existing-office-365-account-azure-subscription/7-azure-portal-browse-ad.png)

3. Klicken Sie auf **Benutzer**.

    ![Die Registerkarte Benutzer](./media/billing-use-existing-office-365-account-azure-subscription/8-azure-portal-ad-users-tab.png)

4. Alle Benutzer, einschließlich Kelley, aufgelisteten wie erwartet.

    ![Benutzerliste](./media/billing-use-existing-office-365-account-azure-subscription/9-azure-portal-ad-users.png)

<a id="s2"></a>
### <a name="scenario-2-azure-users-who-plan-to-buy-office-365"></a>Szenario 2: Azure Benutzer Office 365 kaufen

In diesem Szenario Kelley Wall ist ein Benutzer mit Azure-Abonnement unter dem Konto admin@contoso.onmicrosoft.com. Kelley möchte Office 365 und dasselbe Verzeichnis, das sie mit Azure bereits verwenden.

>[AZURE.NOTE] Um ein Office 365-Abonnement erhalten, muss das Konto für die Anmeldung verwenden der globale Administrator oder Admin Abrechnung Verzeichnis in Ihrem Mandanten Azure Active Directory angehören. [Erfahren Sie, wie die Rolle in Azure Active Directory](#how-to-know-your-role-in-your-azure-active-directory).

![Azure-Portal Abonnement settings](./media/billing-use-existing-office-365-account-azure-subscription/10-azure-portal-settings-subscription.png)

![Azure Active Directory Portalbenutzer](./media/billing-use-existing-office-365-account-azure-subscription/11-azure-portal-ads-users.png)

Um Office 365 abonnieren, gehen Sie folgendermaßen vor:

1. [Office 365-Produktseite](https://products.office.com/business)und wählen Sie dann einen Plan, der für Sie geeignet ist.
2. Nachdem Sie den Plan auswählen, wird die folgende Seite angezeigt. Nicht im Formular ausfüllen. Klicken Sie auf der oberen rechten Ecke der Seite **Anmelden** .

    ![Seite der Office 365-Testversion](./media/billing-use-existing-office-365-account-azure-subscription/12-office-365-trial-page.png)

3. Ihre Anmeldeinformationen anmelden. In diesem Beispiel ist es Kelleys Konto.

    ![Office 365 anmelden](./media/billing-use-existing-office-365-account-azure-subscription/13-office-365-sign-in.png)

4. Klicken Sie auf **Testen**.

    ![Bestätigen Sie Ihre Bestellung für Office 365.](./media/billing-use-existing-office-365-account-azure-subscription/14-office-365-confirm-your-order.png)

5. Klicken Sie auf den Eingang auf **Weiter**.

    ![Wareneingang für Office 365](./media/billing-use-existing-office-365-account-azure-subscription/15-office-365-order-receipt.png)

Jetzt sind Sie alle festlegen. Im Office 365 Administrationscenter finden Sie Benutzer aus der Contoso-Verzeichnis als aktive Benutzer. Gehen Sie hierzu folgendermaßen vor:

1. Öffnen Sie Office 365 Administrationscenter.
2. Erweitern Sie **Benutzer**, und klicken Sie auf **Aktive Benutzer**.

    ![Office 365 Admin Center Benutzer](./media/billing-use-existing-office-365-account-azure-subscription/16-office-365-admin-center-users.png)

### <a name="how-to-know-your-role-in-your-azure-active-directory"></a>Kennenlernen Ihrer Rolle in Azure Active Directory

1. Mit der [Azure-Portal](https://portal.azure.com/)anmelden.
2. Klicken Sie auf **Durchsuchen**, und klicken Sie dann auf **Active Directory**.

    ![Active Directory im Azure-portal](./media/billing-use-existing-office-365-account-azure-subscription/7-azure-portal-browse-ad.png)

3. Klicken Sie auf **Benutzer**.

    ![Azure Portal Standard Active Directory-Benutzer](./media/billing-use-existing-office-365-account-azure-subscription/17-azure-portal-default-ad-users.png)

4. Klicken Sie auf Benutzer. In diesem Beispiel wird der Benutzer Kelley Wall.

    Beachten Sie das Feld **ORGANISATORISCHE Funktion**.

    ![Azure-Portal Benutzeridentität](./media/billing-use-existing-office-365-account-azure-subscription/18-azure-portal-user-identity.png)

## <a name="background-information-about-azure-and-office-365-subscriptions"></a>Hintergrundinformationen zu Azure und Office 365-Abonnements
Office 365 und Azure nutzen Azure Active Directory Benutzer und Abonnements zu verwalten. Sollten Sie ein Azure-Verzeichnis als ein Container, in dem Sie Benutzer und Abonnements gruppieren können. Um dasselbe Benutzerkonto für Ihre Azure und Office 365-Abonnements verwenden, müssen Sie sicherstellen, dass die Abonnements im gleichen Verzeichnis erstellt werden. Beachten Sie Folgendes:

- Ein Abonnement erstellt ein Verzeichnis nicht umgekehrt.
- Benutzer gehören Verzeichnisse, nicht umgekehrt.
- Ein Abonnement landet im Verzeichnis des Benutzers, der das Abonnement erstellt. Daher ist Ihre Office 365-Abonnement auf das Konto der Azure-Abonnement gebunden, wenn Sie dieses Konto das Office 365-Abonnement erstellen.

![Hintergrundinformationen](./media/billing-use-existing-office-365-account-azure-subscription/19-background-information.png)

Weitere Informationen finden Sie unter [wie Azure-Abonnements Azure Active Directory zugeordnet sind](./active-directory/active-directory-how-subscriptions-associated-directory.md).

>[AZURE.NOTE] Azure-Abonnements sind im Besitz einzelner Benutzer im Verzeichnis.

>[AZURE.NOTE] Office 365-Abonnements gehören das Verzeichnis selbst. Wenn Benutzer im Verzeichnis erforderlichen Berechtigungen verfügen, können sie diese Abonnements ausgeführt werden.

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie Ihre Azure und Office 365-Abonnements separat in der Vergangenheit, und Office 365-Mandanten aus der Azure-Abonnement zugreifen möchten, finden Sie unter [Office 365-Mandanten mit Azure-Abonnement zuzuordnen](billing-add-office-365-tenant-to-azure-subscription.md).

> [AZURE.NOTE] Haben Sie noch Fragen [wenden Sie sich](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) an Ihr Problem schnell gelöst.
