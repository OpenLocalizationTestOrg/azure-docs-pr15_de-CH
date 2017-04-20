<properties
    pageTitle="Nicht Azure-Abonnement anmelden | Microsoft Azure"
    description="Beschreibt einige Azure-Abonnement anmelden Probleme beheben."
    services=""
    documentationCenter=""
    authors="genlin"
    manager="mbaldwin"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="genli"/>

# <a name="i-cant-sign-in-to-manage-my-azure-subscription"></a>Anmeldung nicht möglich Azure Abonnement verwalten

Dieser Artikel führt Sie durch einige der häufigsten Methoden zu Anmeldeproblemen.

## <a name="page-hangs-in-the-loading-status"></a>Seite hängt in den Ladestatus

Hängt von Ihrem Internet-Browser-Seite sollten Sie die folgenden Schritte bis zum [Azure-Portal](https://portal.azure.com)zu erhalten.

-   Aktualisieren Sie die Seite.
-   Verwenden Sie einen anderen Browser.
-   Wenn Sie Microsoft Internet Explorer verwenden, wechseln Sie zu Azure-Portal das InPrivate-Browsen Modus. 

    A.  Klicken Sie auf **Extras** ![Schaltfläche Extras](./media/billing-cannot-login-subscription/Toolsbutton.png) > **Sicherheit** > **InPrivate-Browsen**.

    B.  [Azure-Portal](https://portal.azure.com)durchsuchen und dann auf das Portal anmelden.

## <a name="error-message-no-subscriptions-found"></a>Fehlermeldung "Keine Abonnements gefunden"

Wenn Ihr Konto nicht über ausreichende Berechtigungen haben, sehen Sie eine Fehlermeldung **wurden keine Abonnements gefunden** . Nur Konto-Administrator erhalten [Account Center](https://account.windowsazure.com/)nicht die Dienstadministratoren (SA) oder Co-Administratoren (CA).

**Szenario 1: Fehlermeldung wird der [Azure-Portal](https://portal.azure.com) erhalten.**

Zum Beheben dieses Problems für das Konto [die Rolle Co-Administrator oder Besitzer hinzufügen](billing-add-change-azure-subscription-administrator.md) .

**Szenario 2: Fehlermeldung in [Azure Account Center](https://account.windowsazure.com/Subscriptions) empfangen**

Überprüfen Sie, ob das Konto Konto-Administrator. Gehen folgendermaßen Sie vor um zu überprüfen, Konto-Administrator:

1.  Mit der [Azure-Portal](https://portal.azure.com)anmelden.
2.  Wählen Sie im Hub **Abonnement**.
3.  Abonnement, das Sie überprüfen möchten, und wählen Sie **Eigenschaften**.
4.  Wählen Sie **Eigenschaften**. Konto-Administrator des Abonnements ist im **Administratorkonto** angezeigt.

## <a name="you-are-automatically-signed-in-as-a-different-user"></a>Sie werden automatisch angemeldet als anderer Benutzer

Dieses Problem kann auftreten, wenn Sie über ein Benutzerkonto in einem Internetbrowser verwenden.

Um das Problem zu beheben, führen Sie eine der folgenden Methoden:

-   Leeren Sie den Cache und löschen Sie Internetcookies. Klicken Sie in Internet Explorer auf **Extras** ![Schaltfläche Extras](./media/billing-cannot-login-subscription/Toolsbutton.png) > **Optionen** > **Löschen**. Stellen Sie sicher, dass die Kontrollkästchen für temporäre Dateien, Cookies, Kennwort und Verlauf ausgewählt sind, und klicken Sie dann auf Löschen.

-   Zurücksetzen der Standardeinstellungen von Internet Explorer persönlichen wiederherzustellen, die Sie vorgenommen haben. Klicken Sie auf **Extras** ![Schaltfläche Extras](./media/billing-cannot-login-subscription/Toolsbutton.png)> **Optionen** > **Erweitert** > Einstellungen das **Löschen** > **Zurücksetzen**.

-   Wechseln Sie zum Azure-Portal im InPrivate-Browsen. Klicken Sie auf **Extras** ![Schaltfläche Extras](./media/billing-cannot-login-subscription/Toolsbutton.png) > **Sicherheit** > **InPrivate-Browsen**.

## <a name="need-help-contact-support"></a>Benötigen Sie Hilfe? Kontakt zum Support. 

Benötigen Sie noch Hilfe [wenden](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , um Ihr Problem schnell gelöst. 