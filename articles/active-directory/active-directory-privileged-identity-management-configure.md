<properties
    pageTitle="Azure AD Berechtigungen Identitätsmanagement | Microsoft Azure"
    description="Ein Thema, das erklärt Azure AD privilegierten Identitätsmanagement und wie Sie PIM um Cloud-Sicherheit zu verbessern."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="kgremban"/>

# <a name="azure-ad-privileged-identity-management"></a>Azure AD Berechtigungen Identitätsmanagement

Azure Active Directory (AD) privilegierten Identity Management können Sie verwalten, steuern und überwachen Zugriff innerhalb Ihrer Organisation. Dies umfasst den Zugriff auf Ressourcen in Azure AD und andere online-Dienste von Microsoft Office 365 oder Windows Intune.  

> [AZURE.NOTE] Privilegierte Identity Management ist nur mit P2 Premium Edition von Azure Active Directory verfügbar. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md).

Organisationen möchten minimieren Sie die Anzahl Personen Zugriff auf sichere Informationen oder Ressourcen, da verringert die Wahrscheinlichkeit, dass ein böswilliger Benutzer diese zugreifen. Allerdings müssen Benutzer dennoch privilegierten in Azure, Office 365 und SaaS-apps durchzuführen. Unternehmen geben privilegierten Zugriff in Azure AD ohne Überwachung tun die Benutzer mit ihren Administratorberechtigungen. Azure AD Berechtigungen Identitätsmanagement ermöglicht Lösung dieses Risikos.  

Azure AD Berechtigungen Identitätsmanagement können Sie:  

- Welche Benutzer Azure AD-Administratoren sind
- Aktivieren Sie on-Demand "just in Time" Administratorzugriff auf Microsoft Online Services wie Office 365 und Windows Intune
- Abrufen von Berichten über Administrator Zugriff Geschichte und Administrator-Aufgaben
- Lassen Sie sich über einen privilegierten Rolle

Azure AD privilegierten Identity Management gelingt die integrierte Azure AD Rollen, einschließlich:  

- Globaler Administrator
- Abrechnungsadministrators
- Dienstadministratoren  
- Benutzer
- Administrator Kennwort

## <a name="just-in-time-administrator-access"></a>In Administratorzugriff

Früher konnte Sie Administratorrolle Azure-Verwaltungsportal oder Windows PowerShell einen Benutzer zuweisen. Daher wird dieser Benutzer eine **permanente Administrator**zugewiesene Rolle stets aktiv. Azure AD Berechtigungen Identitätsmanagement Konzept **berechtigt Admin**. Können Administratoren sollten Benutzer sein, die Berechtigungen und, aber nicht täglich zugreifen. Die Rolle ist inaktiv, bis der Benutzer Zugriff benötigt sie eine Aktivierung abgeschlossen und aktive Administrator für einen festgelegten Zeitraum.

## <a name="enable-privileged-identity-management-for-your-directory"></a>Privilegierte Identity Management für das Verzeichnis aktivieren

Sie können beginnen Azure AD privilegierten Identitätsmanagement in [Azure-Portal](https://portal.azure.com/).

>[AZURE.NOTE] Muss ein globaler Administrator eine Organisationseinheit Konto (z. B. @yourdomain.com), kein microsoftkonto (z. B. @outlook.com), Azure AD privilegierten Identitätsmanagement für ein Verzeichnis zu aktivieren.

1. Melden Sie sich der [Azure-Portal](https://portal.azure.com/) als globaler Administrator des Verzeichnisses.
2. Wenn Ihre Organisation mehrere Verzeichnisse verfügt, wählen Sie Ihren Benutzernamen in der oberen rechten Ecke der Azure-Portal. Wählen Sie das Verzeichnis, in Azure AD privilegierten Identitätsmanagement verwendet.
3. Wählen Sie **Weitere Dienste** und Filter Textbox **Azure AD privilegierten Identitätsmanagement**suchen.
4. Überprüfen **auf Dashboard** , und klicken Sie auf **Erstellen**. Privilegierte Identity Management-Anwendung wird geöffnet.

BIST die erste Person mit Azure AD privilegierten Identitätsmanagement im Verzeichnis führt Sie der [Assistent](active-directory-privileged-identity-management-security-wizard.md) anfänglicher Zuweisung Erfahrung. Danach werden Sie automatisch die erste **Sicherheitsadministrator** und **privilegierte Rollenadministrator** des Verzeichnisses.

Nur bevorzugte Rolle Administratoren kann anderen Administratoren Zugriff verwalten. Sie können [anderen Benutzern PIM verwalten](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="privileged-identity-management-dashboard"></a>Privilegierte Identity Management-dashboard

Azure AD Berechtigungen Identitätsmanager bietet ein Dashboard, das wichtige Informationen bietet:

- Verkaufschancen Sicherheit hin Alerts
- Die Anzahl der Benutzer für jede Rolle Berechtigungen  
- Die Anzahl der Administratoren berechtigt und dauerhaft
- Zugriff überprüft

![PIM-Dashboard - screenshot][2]

## <a name="privileged-role-management"></a>Privilegierte rollenmanagement

Azure AD privilegierten Identity Management können Sie die Administratoren hinzufügen oder Entfernen von permanenten oder können Administratoren jede Rolle verwalten.

![PIM-Administratoren hinzufügen/entfernen - screenshot][3]

## <a name="configure-the-role-activation-settings"></a>Die Rolle-Aktivierung konfigurieren

Mit den [Einstellungen](active-directory-privileged-identity-management-how-to-change-default-settings.md) können Sie die beihilfefähigen Aktivierung Rolleneigenschaften einschließlich konfigurieren:

- Die Dauer des Aktivierungszeitraums Rolle
- Die Benachrichtigung über die Aktivierung von Rolle
- Die Informationen, die Anwender bei der Aktivierung der Funktion  

![PIM Settings - Administrator Aktivierung - screenshot][4]

Beachten Sie, dass das Bild die Schaltflächen für die **Kombinierte Authentifizierung** deaktiviert sind. Für bestimmte, privilegierte Funktionen benötigen wir MFA erhöhter Schutz.

## <a name="role-activation"></a>Rollenaktivierung  

[Aktivieren eine Rolle](active-directory-privileged-identity-management-how-to-activate-role.md)fordert Administrator berechtigt einen zeitnah "Aktivierung" für die Rolle. Die Aktivierung kann mit aktivieren **Meine Rolle** in Azure AD privilegierten Identitätsmanagement angefordert werden.

Ein Administrator möchte eine Rolle aktivieren muss Azure AD privilegierten Identitätsmanagement im Azure-Portal zu initialisieren.

Rollenaktivierung kann angepasst werden. PIM-Einstellungen Sie bestimmen die Länge der Aktivierung und Informationen der Administrator benötigt, um die Rolle zu aktivieren.

![PIM-Administrator Anforderung rollenaktivierung - screenshot][5]

## <a name="review-role-activity"></a>Rolle Aktivitäten überprüfen

Es gibt zwei Möglichkeiten verfolgen, wie Ihre Mitarbeiter und Administratoren Berechtigungen Rollen verwenden. Die erste Option verwendet [Überwachungsverlauf](active-directory-privileged-identity-management-how-to-use-audit-log.md). Der Überwachungsverlauf protokolliert Änderungsprotokoll Aktivierung privilegierten Rolle Aufgaben und Rollen.

![PIM-Aktivierung History - screenshot][6]

Die zweite Option soll regulären [Access überprüft](active-directory-privileged-identity-management-how-to-start-security-review.md). Diese Access-Berichte ausgeführt und Prüfer (z. B. Teammanager) zugeordnet werden können oder die Mitarbeiter können selbst überprüfen. Dies ist die beste Möglichkeit, die noch Zugriff benötigt, und wer nicht.


## <a name="next-steps"></a>Nächste Schritte
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ./media/active-directory-privileged-identity-management-configure/PIM_AddRemove.png
[4]: ./media/active-directory-privileged-identity-management-configure/PIM_RoleActivationSettings.png
[5]: ./media/active-directory-privileged-identity-management-configure/PIM_RequestActivation.png
[6]: ./media/active-directory-privileged-identity-management-configure/PIM_ActivationHistory.png
