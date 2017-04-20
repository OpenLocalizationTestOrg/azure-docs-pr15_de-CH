<properties
   pageTitle="Das Verzeichnis für Ihr Office 365-Abonnement in Azure verwalten | Microsoft Azure"
   description="Ein Office 365-Abonnementverzeichnis Azure Active Directory mit klassischen Azure-Portal verwalten"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="manage-the-directory-for-your-office-365-subscription-in-azure"></a>Verwalten Sie das Verzeichnis für Ihr Office 365-Abonnement in Azure

Dieser Artikel beschreibt, wie ein Verzeichnis zu verwalten, die für ein Office 365-Abonnement mit klassischen Azure-Portal erstellt wurde. Dienstadministrator oder Co-Administrator der Azure-Abonnement klassischen Azure-Portal anmelden muss. Haben Sie noch Azure-Abonnement, können Ihre erste Cloudlösung unter 5 Minuten mit diesem Link Sie heute für eine [30-tägige Testversion](https://azure.microsoft.com/trial/get-started-active-directory/) anmelden und bereitstellen. Achten Sie darauf, dass bei Office 365 anmelden Arbeits- oder Schulcomputer Konto verwenden.

Nach Abschluss das Azure-Abonnement können Sie klassischen Azure-Portal anmelden und Azure Services zugreifen. Klicken Sie auf die Active Directory-Erweiterung um dasselbe Verzeichnis zu verwalten, das Ihr Office 365-Benutzer authentifiziert.

Wenn Sie bereits ein Azure-Abonnement haben, ist der Prozess ein zusätzliches Verzeichnis einfach. Z. B. möglicherweise Michael Smith ein Office 365-Abonnement für Contoso.com. Er hat auch ein Azure-Abonnement, die er für seine Microsoft-Konto mit signiert msmith@hotmail.com. In diesem Fall leitet er zwei Verzeichnisse.

  Abonnement |  Office 365  |  Azure
  -------------- | ------------- | -------------------------------
  Angezeigter name |  Contoso  |     Standardverzeichnis Azure Active Directory (Azure AD)
  Domänenname  |  Contoso.com  | msmithhotmail.onmicrosoft.com

Will er mit seinem Konto Microsoft Azure angemeldet ist, damit er Azure AD-Funktionen wie mehrstufige Authentifizierung kann Benutzeridentitäten in der Contoso-Verzeichnis verwalten. Im folgende Diagramm kann helfen, Erläutern Sie den Vorgang.

![Diagramm zwei unabhängige Verzeichnisse verwalten](./media/active-directory-manage-o365-subscription/AAD_O365_03.png)

In diesem Fall sind die zwei Verzeichnisse voneinander unabhängig.

## <a name="to-manage-two-independent-directories"></a>Zwei unabhängige Verzeichnisse verwalten
Reihenfolge für Michael Smith beide Verzeichnisse verwalten, während er in Azure als wird msmith@hotmail.com, er muss die folgenden Schritte ausführen:

> [AZURE.NOTE]
> Diese Schritte können ausgeführt werden, nur, wenn ein Benutzer mit einem microsoftkonto angemeldet ist. Wenn der Benutzer eine Arbeit oder schulkonto, können angemeldet ist nicht **mit vorhandenen Verzeichnis** verfügbar. Arbeits- oder Schulcomputer Konto kann nur von dessen Basisverzeichnis (d. h. das Verzeichnis Arbeits- oder Schulcomputer Konto gespeichert, wobei, das Geschäft oder Schule besitzt) authentifiziert werden.

1.  Melden Sie sich bei [Azure-Verwaltungsportal](https://manage.windowsazure.com) als msmith@hotmail.com.
2.  Klicken Sie auf **neu** > **App Services** > **Active Directory** > **Verzeichnis** > **Benutzerdefinierte**.
3.  Klicken Sie auf vorhandenes Verzeichnis verwenden und das Kontrollkästchen Sie **ich möchte jetzt abgemeldet** .
4.  Als globaler Administrator Contoso.onmicrosoft.com klassischen Azure-Portal anmelden (z. B. msmith@contoso.com).
5.  Aufforderung zum **verwenden das Contoso-Verzeichnis mit Azure?**, klicken Sie auf **Weiter**.
6.  Klicken Sie auf **Abmelden**.
7.  Melden Sie sich beim klassischen Azure-Portal als msmith@hotmail.com. Das Contoso-Verzeichnis und das Standardverzeichnis in Active Directory-Erweiterung angezeigt.

Nach Abschluss dieser Schritte msmith@hotmail.com ist ein globaler Administrator in der Contoso-Verzeichnis.

## <a name="to-administer-resources-as-the-global-admin"></a>Verwalten von Ressourcen als globaler Administrator
Nun nehmen wir an, dass Jane Doe muss Websites und Ressourcen verwalten, die mit der Azure-Abonnement für msmith@hotmail.com. Bevor sie dies tun kann, muss Michael Smith diese zusätzlichen Schritte ausführen:

1.  Melden Sie sich bei Azure-Abonnement über die Dienstadministratorkonten [Azure-Verwaltungsportal](https://manage.windowsazure.com) (in diesem Beispiel msmith@hotmail.com).
2.  Das Abonnement in das Contoso-Verzeichnis übertragen: **Klicken Sie** > **Abonnements** > Abonnement > **Verzeichnis bearbeiten** > **Contoso (Contoso.com)**wählen. Als Teil der Übertragung, Arbeit oder Schule werden Konten Co-Administratoren des Abonnements entfernt.
3.  Susanne als Co-Administrator des Abonnements hinzufügen: **Klicken Sie** > **Administratoren** > Abonnement > **Hinzufügen** > Typ **JohnDoe@Contoso.com**.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen über die Beziehung zwischen Abonnements und Verzeichnissen finden Sie unter [wie ein Abonnement ein Verzeichnis zugeordnet ist](active-directory-how-subscriptions-associated-directory.md).
