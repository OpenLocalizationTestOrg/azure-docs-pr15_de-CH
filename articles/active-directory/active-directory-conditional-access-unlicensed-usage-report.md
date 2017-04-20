<properties
    pageTitle="Nichtlizenzierte Verwendungsbericht | Microsoft Azure"
    description="Nichtlizenzierte Verwendung Bericht können nicht lizenzierte Benutzer zu identifizieren, die verwenden bezahlt Azure AD-Funktionen."
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

# <a name="unlicensed-usage-report"></a>Nichtlizenzierte Verwendungsbericht

Nichtlizenzierte Verwendung Bericht können nicht lizenzierte Benutzer zu identifizieren, die verwenden bezahlt Azure AD-Funktionen. Dadurch können Sie besser zu verwenden, der erworbenen Lizenzen und zum Identifizieren Sie zusätzliche Lizenzen benötigen. 

Der Bericht zeigt aktive Nutzung der bezahlten Features in den letzten 30 Tagen. 

## <a name="report-structure"></a>Des Berichts
 
| Spaltenname          |    Beschreibung |
| :--                  | :--         |
| Nicht lizenzierte Benutzer      |    Name des Benutzers |
| Funktion              | Der Featurename. Beispiel: Zugangskontrolle |
| Anwendung zugegriffen | Der Name der Anwendung, die mit der Funktion erfolgt. Beispiel: Office 365 SharePoint Online |

 
> [AZURE.NOTE] Wenn ein Benutzerkonto gelöscht wurde, die Spalte "Nicht lizenzierte Benutzer" mit einer ID wie 1003000090D8B285 gefüllt


## <a name="conditional-access-feature"></a>Funktion des bedingten Zugriffs

Beim Zugriff auf einen Dienst, die bedingte Richtlinie angewendet, wenn sie keine Azure AD Premium-Lizenz werden nicht lizenzierten Benutzer gekennzeichnet. 

Dies gilt für MFA / Richtlinien Speicherort sowie Geräte, die Windows Intune verwenden.
 

## <a name="see-also"></a>Siehe auch

- [Apps mit Zugangskontrolle mit Office 365 und anderen Azure Active Directory verbunden](active-directory-conditional-access.md)
- [Erste Schritte mit bedingten Azure AD-Zugriff](active-directory-conditional-access-azuread-connected-apps.md) 


