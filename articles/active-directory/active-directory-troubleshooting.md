<properties
   pageTitle="Problembehandlung: "Active Directory"-Element ist nicht vorhanden oder nicht verfügbar | Microsoft Azure "
   description="Vorgehensweise bei Active Directory-Menüelement in Azure-Verwaltungsportal angezeigt wird."
   services="active-directory"
   documentationCenter="na"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="troubleshooting-active-directory-item-is-missing-or-not-available"></a>Problembehandlung: "Active Directory"-Element ist nicht vorhanden oder nicht verfügbar

Viele der im Abschnitt Azure Active Directory-Funktionen und Dienste beginnen mit "Zur Azure-Verwaltungsportal und klicken Sie auf **Active Directory"**. Aber was tun Sie, wenn der Active Directory-Erweiterung oder ein Menüelement nicht angezeigt wird oder **Nicht verfügbar**gekennzeichnet ist? Dieses Thema soll. Der Bedingung unter denen **Active Directory** nicht angezeigt oder ist nicht verfügbar, und erläutert, wie beschrieben.

## <a name="active-directory-is-missing"></a>Active Directory ist nicht vorhanden

Normalerweise wird ein **Active Directory** -Element im linken Navigationsmenü angezeigt. Die Anleitung in Azure Active Directory Verfahren davon aus, dass dieses Element in der Ansicht.

![Abbildung: Active Directory in Azure](./media/active-directory-troubleshooting/typical-view.png)

Active Directory-Element wird im linken Navigationsmenü angezeigt, wenn Folgendes stimmt. Andernfalls wird das Element nicht angezeigt.

* Der aktuelle Benutzer mit einem microsoftkonto (ehemals eine Windows Live ID) angemeldet.

    ODER

* Azure Mieter hat ein Verzeichnis und die Directory-Administrator ist.

    ODER

* Azure Mieter hat mindestens eine Azure AD Access Control (ACS) Namespace. Weitere Informationen finden Sie in der [Access Control Namespace](https://msdn.microsoft.com/library/azure/gg185908.aspx).

    ODER

* Azure Mieter hat mindestens ein Azure Multi-Factor Authentication Provider. Weitere Informationen finden Sie unter [Verwalten von Azure Mehrfaktor-Authentifizierungsanbieter](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

Um eine Access Control Namespace oder eine mehrstufige Authentifizierung zu erstellen, klicken Sie auf **+ neu** > **App Services** > **Active Directory**.

Um Administratorrechte zu einem Verzeichnis zu erhalten, müssen Sie Administrator Ihr Konto eine Administratorrolle zuweisen. Weitere Informationen finden Sie unter [Zuweisen von Administratorrollen](active-directory-assign-admin-roles.md).

## <a name="active-directory-is-not-available"></a>Active Directory ist nicht verfügbar

Beim Klicken auf **+ neu** > **App Services**ein **Active Directory** -Element angezeigt wird. Insbesondere wird das Active Directory-Element beim Active Directory-Funktionen wie Directory Access Control oder kombinierte Authentifizierungsanbieter für den aktuellen Benutzer verfügbar sind.

Aber während die Seite geladen wird, das Element ist abgeblendet und als **Nicht verfügbar**gekennzeichnet. Dies ist ein vorübergehender Zustand. Ein paar Sekunden wird das Element verfügbar. Wenn die Verzögerung verlängert wird, löst Webseite häufig aktualisieren das Problem.

![Abbildung: Active Directory ist nicht verfügbar](./media/active-directory-troubleshooting/not-available.png)
