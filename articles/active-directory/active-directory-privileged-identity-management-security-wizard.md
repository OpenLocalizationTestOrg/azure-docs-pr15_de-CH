<properties
   pageTitle="Der Assistent Azure AD privilegierten Identitätsmanagement"
   description="Beim ersten die Erweiterung Azure Active Directory privilegierte Identity Management verwenden wird durch einen Assistenten angezeigt. Dieser Artikel beschreibt die Schritte zur Verwendung des Assistenten."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="07/01/2016"
   ms.author="kgremban"/>

# <a name="the-azure-ad-privileged-identity-management-security-wizard"></a>Der Assistent Azure AD privilegierten Identitätsmanagement

Wenn Sie die erste Person Azure privilegierten Identity Management (PIM) für Ihre Organisation ausführen, wird ein Assistent angezeigt. Der Assistent die Sicherheitsrisiken privilegierter Identitäten und PIM verwenden, um die Risiken kennen. Sie müssen vorhandene Rolle Arbeitsaufträge im Assistenten ändern möchten Sie später.

## <a name="what-to-expect"></a>Was erwartet

Vor Ihrer Organisation mit PIM sind alle rollenzuweisungen permanent: Benutzer sind immer in diesen Rollen, selbst wenn sie derzeit nicht über ihre Rechte benötigen.  Der erste Schritt des Assistenten wird eine Liste mit hoher Berechtigung Rollen und wie viele Benutzer diese Funktionen befinden. Sie können einer bestimmten Rolle Benutzer sofern Weitere Informationen anzeigen oder mehr vertraut sind.

Im zweiten Schritt des Assistenten haben Sie Gelegenheit, Ändern der Zuweisung von Administrator Rolle.  

> [AZURE.WARNING]Es ist wichtig, dass Sie mindestens ein globaler Administrator und mehrere bevorzugte Rollenadministrator eine Organisationseinheit Konto (kein microsoftkonto). Ist nur eine privilegierte Rollenadministrator, dürfen die Organisation PIM verwaltet, wenn das Konto gelöscht wird.
> Halten Sie Arbeitsaufträge für Benutzerrollen auch permanent, wenn ein Benutzer ein Microsoft-Konto (Konto verwenden sie Skype und Outlook.com von Microsoft anmelden). Möchten Sie MFA für Aktivierung für diese Rolle erforderlich ist, wird der Benutzer gesperrt.


Nach Änderungen vorgenommenen wird der Assistent nicht angezeigt. Beim nächsten Mal einem privilegierten Rolle Administrator PIM sehen Sie PIM-Dashboard.  

- Wenn Sie hinzufügen oder Entfernen von Benutzern aus Rollen oder aus permanent sein, um Anspruch mehr wie [Hinzufügen oder Entfernen der Rolle eines Benutzers](active-directory-privileged-identity-management-how-to-add-role-to-user.md)ändern möchten.
- Möchten Sie mehr Benutzer Zugang zu PIM, mehr wie [PIM verwalten zugreifen](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).



## <a name="next-steps"></a>Nächste Schritte
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
