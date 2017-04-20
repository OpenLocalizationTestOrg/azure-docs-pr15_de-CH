<properties
   pageTitle="Management von Verwaltungseinheiten in Azure Active Directory"
   description="Verwenden genauere Delegierung von Berechtigungen in Active Directory Azure Verwaltungseinheiten"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="administrative-units-management-in-azure-ad---public-preview"></a>Management von Verwaltungseinheiten in Azure AD - Public Preview

Dieser Artikel beschreibt Verwaltungseinheiten – einen neuen Azure Active Directory-Container der Ressourcen, die zum Erteilen von administrativen Berechtigungen über Teilmengen von Benutzern und Anwendung von Richtlinien auf eine Gruppe von Benutzern verwendet werden kann. Azure Active Directory aktivieren Verwaltungseinheiten Administratoren Berechtigungen an regionale Administratoren delegiert oder Richtlinie genauer festzulegen.

Dies ist nützlich in Organisationen mit unabhängigen Bereichen, z. B. eine große Universität, die viele autonomen Schulen (Business School, technische Hochschule usw.) besteht unabhängig voneinander sind. Diese Bereiche haben ihre eigenen IT-Administratoren, Zugriffskontrolle, Benutzer verwalten und Festlegen von Richtlinien für ihre Abteilung. Administratoren können möchten diese Abteilung gewähren Administratorberechtigungen den Benutzern in die einzelnen Geschäftsbereiche. Insbesondere kann in diesem Beispiel ein zentraler Administrator beispielsweise eine administrative Einheit für eine Schule (Business School) erstellen und Füllen mit nur Benutzer der Schule. Gewähren Sie über den Verwaltungseinheit Business School ein zentraler Administrator Business School IT-Personal zu einer gültigen Rolle hinzufügen kann also das IT-Personal Business School administrative Berechtigungen.

> [AZURE.IMPORTANT]
> Sie können erstellen und Verwaltungseinheiten nur verwenden, wenn Sie Azure Active Directory Premium aktivieren. Weitere Informationen finden Sie unter [Erste Schritte mit Azure AD Premium](active-directory-get-started-premium.md).

Aus Sicht der zentralen Administrator ist eine administrative Einheit ein Verzeichnisobjekt, die erstellt und mit Ressourcen gefüllt. **In dieser Version können diese Ressourcen nur Benutzer sein.** Erstellt und aufgefüllt, kann die Verwaltungseinheit über Ressourcen in den Verwaltungseinheit erteilte Berechtigung einschränken als Bereich verwendet werden.

## <a name="managing-administrative-units"></a>Verwalten von Einheiten

In dieser Vorabversion können Sie erstellen und Verwalten von Verwaltungseinheiten Azure Active Directory-Modul für Windows PowerShell-Cmdlets verwenden.

Weitere Informationen für Software und Azure AD-Modul installieren und Informationen zu Azure AD Modul Cmdlets zur Verwaltung von Einheiten, einschließlich Syntax, Parameter Beschreibung und Beispiele finden Sie unter [Manage Azure AD mit Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx).


## <a name="next-steps"></a>Nächste Schritte
[Azure Active Directory-Editionen](active-directory-editions.md)
