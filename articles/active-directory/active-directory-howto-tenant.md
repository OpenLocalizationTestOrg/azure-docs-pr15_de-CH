<properties
    pageTitle="Wie man einen Azure AD-Mandanten | Microsoft Azure"
    description="Wie man einen Mandanten Azure Active Directory für das Registrieren und erstellen."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="terrylan"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/28/2015"
    ms.author="dastrock"/>

# <a name="how-to-get-an-azure-active-directory-tenant"></a>Wie man einen Mandanten Azure Active Directory

In Azure Active Directory (Azure AD) ist ein [Mieter](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) Vertreter einer Organisation.  Es ist eine dedizierte Instanz von Azure AD-Dienst, den eine Organisation erhält und besitzt, wenn er für einen Microsoft Cloud-Dienst wie Azure, Windows Intune oder Office 365 anmeldet.  Jede Azure AD-Mandanten ist distinct und getrennt von anderen Azure AD-Mandanten.  

Ein Mieter befinden sich Benutzer in einem Unternehmen und die Informationen ihrer Kennwörter, Benutzerprofildaten, Berechtigungen und usw..  Es enthält außerdem Gruppen, Programme und andere Informationen zu einer Organisation und die Sicherheit.

Azure AD-Benutzern bei der Anwendung anmelden müssen Sie Ihre Anwendung in einem Mandanten selbst registrieren, damit.  Veröffentlichen einer Anwendung in Azure AD-Mandanten ist **kostenlos**.  Sogar erstellt die meisten Entwickler mehrere Mandanten Anträge experimentieren Entwicklung und staging und testen.  Organisationen, die anmelden und die Anwendung nutzen, können optional auch Lizenzen erwerben, wenn sie erweiterte Directory-Funktionen nutzen möchten.

Also, wie können wir Azure AD-Mandanten abrufen?  Der Prozess möglicherweise ein wenig andere, wenn Sie:

- [Haben Sie ein Office 365-Abonnement](#use-an-existing-office-365-subscription)
- [Haben Sie ein Azure-Abonnement Microsoft Account zugeordnet](#use-an-msa-azure-subscription)
- [Haben Sie ein Azure-Abonnement einer Organisationseinheit Firma](#use-an-organizational-azure-subscription)
- [Keines der obigen und neu starten möchten](#start-from-scratch)

## <a name="use-an-existing-office-365-subscription"></a>Verwenden Sie ein Office 365-Abonnement
Wenn Sie ein Office 365-Abonnement haben, verfügen Sie bereits über ein Azure AD-Mandanten! Sie können mit Ihrem Office 365-Konto bei [Azure-Portal](https://portal.azure.com) anmelden und verwenden Azure AD.

## <a name="use-an-msa-azure-subscription"></a>Verwenden Sie eine MSA Azure-Abonnement
Wenn Sie zuvor für Azure-Abonnement mit Ihrem einzelne Microsoft Account angemeldet haben, haben Sie bereits einen Mieter!  Beim [Azure-Portal](https://portal.azure.com)anmelden, werden Sie automatisch Ihrem Mandanten Standard angemeldet. Sie können mit diesen Mandanten beliebig - aber ein Organisations Administratorkonto erstellen möchten.

Gehen Sie dazu folgendermaßen vor.  Alternativ können Sie einen neuen Mandanten erstellen und Administrator in diesem Mandanten nach einem ähnlichen Prozess erstellen.

1.  [Azure-Portal](https://portal.azure.com) mit dem einzelnen Konto anmelden
2.  Navigieren Sie zum Abschnitt "Azure Active Directory" des Portals (in der linken Navigationsleiste unter **Weitere Dienste**gefunden)
3.  Sie automatisch "Standard-Verzeichnis" angemeldet werden sollte, wenn nicht Sie können Verzeichnisse auf Ihren Kontonamen in der oberen rechten Ecke.
4.  Wählen Sie im Abschnitt **Quick Tasks** **Benutzer hinzufügen**.
5.  Im Formular Benutzer hinzufügen Angaben:

    - Name: (Wählen Sie einen entsprechenden Wert)
    - Benutzername: (Wählen Sie einen Benutzernamen für Administrator)
    - Profil: (Geben Sie die entsprechenden Werte für Vorname, letzte Name, Titel und Abteilung)
    - Rolle: Globaler Administrator

6.  Wenn Sie das Benutzer-Formular ausgefüllt haben und das temporäre Kennwort für den neuen Administrator erhalten, müssen Sie dieses Kennwort aufzeichnen müssen mit diesen neuen Benutzer anmelden, um das Kennwort zu ändern. Sie können das Kennwort auch direkt mit dem Benutzer über eine alternative E-mail senden.
7.  Klicken Sie auf **Erstellen** , um den neuen Benutzer erstellt.
8.  Ändern Sie das temporäre Kennwort [https://login.microsoftonline.com](https://login.microsoftonline.com) mit diesem neuen Benutzerkonto melden Sie an, und ändern Sie das Kennwort angefordert.


## <a name="use-an-organizational-azure-subscription"></a>Verwenden Sie Organisationseinheiten Azure-Abonnement
Wenn Sie zuvor für Azure-Abonnement mit Ihrem organisatorische Konto angemeldet haben, haben Sie bereits einen Mieter!  In [Azure-Portal](https://portal.azure.com)sollte einen Mieter finden Sie beim Navigieren zu "Weitere Dienste" und "Azure Active Directory".  Sie können diesen Mandanten beliebig verwenden. 


## <a name="start-from-scratch"></a>Von vorne beginnen
Wenn alle oben genannten Kauderwelsch möchten, kein Problem.  Besuchen Sie [https://account.windowsazure.com/organization](https://account.windowsazure.com/organization) für Azure mit einer neuen Organisation anmelden.  Nach Abschluss des Prozesses haben eigenen Azure AD-Mandanten mit dem Domänennamen Sie, die Sie während der Anmeldung auswählen.  In [Azure-Portal](https://portal.azure.com)finden Ihrem Mandanten Sie Navigieren im linken NAV "Azure Active Directory"

Als Teil des Prozesses zum Azure anmelden müssen Sie Kreditkartendaten.  Vertrauensvoll - fortfahren können Sie nicht berechnet für Anwendung in Azure AD veröffentlichen oder Erstellen von neuen Mandanten.
