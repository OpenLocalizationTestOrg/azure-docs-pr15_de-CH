<properties
    pageTitle="Azure Active Directory Zugangskontrolle FAQ | Microsoft Azure"
    description="Häufig gestellte Fragen zur Zugangskontrolle "
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="markvi"/>

# <a name="azure-active-directory-conditional-access-faq"></a>Azure Active Directory Zugangskontrolle FAQ

## <a name="which-applications-work-with-conditional-access-policies"></a>Die Anwendung funktioniert mit bedingten Richtlinien?

**A:** Bitte finden Sie unter [Conditional Access was Applikationen unterstützt](active-directory-conditional-access-supported-apps.md).

## <a name="are-conditional-access-policies-enforced-for-b2b-collaboration-and-guest-users"></a>B2B-Zusammenarbeit und Gastbenutzern werden bedingte Richtlinien werden erzwungen?

**A:** Richtlinien werden für B2B Zusammenarbeit Benutzer erzwungen. Jedoch in einigen Fällen ein Benutzer möglicherweise nicht auf Policy-Anforderung zu erfüllen, wenn beispielsweise eine Organisation mehrstufige Authentifizierung nicht unterstützt. 

Die Richtlinie wird derzeit nicht für SharePoint Gastbenutzer erzwungen. Gast-Beziehung wird in SharePoint verwaltet. Gastbenutzer sind Konten nicht Zugang Richtlinien an den Authentifizierungsserver. Gastzugriff kann SharePoint verwaltet werden.

## <a name="does-a-sharepoint-online-policy-also-apply-to-onedrive-for-business"></a>Gilt eine SharePoint Online-Richtlinie auch für OneDrive für Unternehmen?

**A:** Ja.
 
## <a name="why-cant-i-set-a-policy-on-client-apps-like-word-or-outlook"></a>Warum aktiviert können keine Clientanwendungen wie Word oder Outlook eine Richtlinie?

**A:** Eine bedingte Richtlinie legt Vorschriften für den Zugriff auf einen Dienst und zu diesem Dienst geschieht Authentifizierung erzwungen. Die Richtlinie wird auf eine Clientanwendung nicht festgelegt. Stattdessen wird sie angewendet, wenn ein Dienst aufruft. Z. B. eine Richtlinie auf SharePoint bezieht auf SharePoint aufrufenden Clients und eine Richtlinie auf Exchange mit Outlook.


## <a name="does-a-conditional-access-policy-apply-to-service-accounts"></a>Gilt eine bedingte Richtlinie für Dienstkonten?

**A:** Bedingte Richtlinien gelten für alle Benutzerkonten. Dies schließt Benutzerkonten als Dienstkonten verwendet. In vielen Fällen kann ein Dienstkonto unbeaufsichtigt ausgeführt wird keine Richtlinie zu erfüllen. Dies ist beispielsweise der Fall, wenn MFA erforderlich ist. In diesen Fällen können eine Richtlinie mit Zugangskontrolle Management Richtlinien Dienstkonten ausgeschlossen. Erfahren Sie mehr über das Anwenden einer Richtlinie auf Benutzer.
