<properties
pageTitle="Azure Active Directory Anwendung und Service Principal-Objekte | Microsoft Azure"
description="Eine Beschreibung der Beziehung zwischen Anwendung und Service principal-Objekte in Active Directory Azure"
documentationCenter="dev-center-name"
authors="bryanla"
manager="mbaldwin"
services="active-directory"
editor=""/>

<tags
ms.service="active-directory"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="identity"
ms.date="08/10/2016"
ms.author="bryanla;mbaldwin"/>

# <a name="application-and-service-principal-objects-in-azure-active-directory"></a>Anwendung und Service principal-Objekte in Active Directory Azure
Beim Lesen über eine Azure Active Directory (AD) "Application" ergibt nicht immer genau wie vom Autor Bezug genommen wird. Das Ziel dieses Artikels ist es, klarer definieren die konzeptionelle und konkrete Aspekte der Azure AD Anwendungsintegration mit einem Beispiel und Registrierung für [Multi-Tenant-Anwendung](active-directory-dev-glossary.md#multi-tenant-application).

## <a name="overview"></a>Übersicht
Eine Azure AD-Anwendung ist mehr als nur ein Stück Software. Ist eine grundlegende, nicht nur auf Anwendungssoftware, sondern auch die Registrierung (aka: Identität Konfiguration) mit Azure AD wodurch es Authentifizierung und Autorisierung "Diskussionen" zur Laufzeit an. Definitionsgemäß kann eine Anwendung [eine Clientrolle (Nutzung einer Ressource)](active-directory-dev-glossary.md#client-application) , eine [Ressource](active-directory-dev-glossary.md#resource-server) -Serverrolle verfügbar machen von APIs (Clients) oder beides fungieren. Ein [Fluss OAuth 2.0 Berechtigung erteilen](active-directory-dev-glossary.md#authorization-grant), mit dem Ziel, die Clientressource ermöglicht Zugriff/einer Ressource bzw. Datenschutz Unterhaltung Protokoll bestimmt. Nun gehen Sie einen Ebene tiefer und sehen Sie, wie das Anwendungsmodell Azure AD eine Anwendung intern darstellt. 

## <a name="application-registration"></a>Anwendungsregistrierung
Beim Registrieren einer Anwendung in [Azure-Verwaltungsportal][AZURE-Classic-Portal], zwei Objekte in Azure AD-Mandanten erstellt: ein Application-Objekt und ein Service principal-Objekt.

#### <a name="application-object"></a>Application-Objekt
Azure AD-Anwendung wird durch Ihre *definiert* und nur Anwendungsobjekt, das in Azure AD-Mandanten befindet, in dem die Anwendung registriert wurde, bezeichnet als "Privat" Mieter der Anwendung. Das Application-Objekt bietet Identitätsinformationen für eine Anwendung und die Vorlage, aus der der entsprechende Service principal-Objekte sind für die Verwendung zur Laufzeit *abgeleitet,* ist. 

Sie können die Anwendung als *globale* Darstellung der Anwendung (zur Verwendung für alle Mandanten) und Dienstprinzipalnamen als *Vertretung (z. B. für einen bestimmten Mandanten)* vorstellen. Azure AD Graph [Anwendung Entität] [ AAD-Graph-App-Entity] definiert das Schema für ein Application-Objekt. Anwendungsobjekt ist daher eine 1:1-Beziehung mit der Software-Anwendung und eine 1:*n* -Beziehung mit der entsprechenden *n* Service principal-Objekte.

#### <a name="service-principal-object"></a>Service principal-Objekt
Das Prinzipalobjekt Service definiert die Richtlinie und die Berechtigungen für eine Anwendung, die die Grundlage für einen Sicherheitsprinzipal zur Darstellung der Anwendung den Zugriff auf Ressourcen zur Laufzeit. Azure AD Graph [ServicePrincipal Entität] [ AAD-Graph-Sp-Entity] definiert das Schema für ein Service principal-Objekt. 

Ein Service principal-Objekt muss jeder für die eine Instanz der Anwendung muss sicheren Zugriff auf Ressourcen Benutzerkonten, Pächter dargestellt werden. Eine einzelne Mieter Anwendung haben nur einen Dienstprinzipalnamen (seinen home Mieter). Ein Multi-Tenant- [Anwendung](active-directory-dev-glossary.md#web-client) müssen einen Dienstprinzipalnamen in jeder, Administrator oder Benutzer aus diesem Mandanten, zugestimmt haben Zugriff auf ihre Ressourcen ermöglicht. Nach Zustimmung wird für zukünftige Autorisierungsanfragen Service principal-Objekt gehört. 

> [AZURE.NOTE] Alle Änderungen auf das Application-Objekt spiegeln auch im Service principal-Objekt in der Anwendung privat Mandanten nur (die Mandanten, in dem es registriert wurde). Für Multi-Tenant-Applikationen sind auf das Application-Objekt keine Consumer Mieter Service principal Objekte Änderungen bis Consumer Mieter entfernt Zugriff erneut gewährt.

## <a name="example"></a>Beispiel
Das folgende Diagramm veranschaulicht die Beziehung zwischen einer Anwendung Anwendungsobjekt und entsprechenden service principal-Objekte im Kontext einer Probe mehrinstanzenfähigen Anwendung **HR app**. In diesem Szenario gibt es drei Azure AD-Mandanten: 

- **Adatum** - Mieter des Unternehmens, das die **HR-Anwendung**
- **Contoso** - von der Contoso-Organisation **HR app Verbraucher** verwendeten Mandanten
- **Fabrikam** - Fabrikam-Organisation, die **HR-Anwendung** verbraucht Mieter

![Beziehung zwischen ein Application-Objekt und ein Service principal-Objekt](./media/active-directory-application-objects/application-objects-relationship.png)

Im vorherigen Diagramm ist Schritt 1 den Prozess der Erstellung der Anwendung und Service principal-Objekte in der Anwendung privat Mandanten.

In Schritt2 Abschluss Contoso und Fabrikam Administratoren Zustimmung ein Prinzipalobjekt Service erstellt in Azure AD-Mandanten ihres Unternehmens und die Administrator erteilten Berechtigungen zugewiesen. Beachten Sie, dass die HR-Anwendung konfiguriert / Zustimmung ermöglichen Benutzer für den eigenen Gebrauch soll konnte.

In Schritt 3 haben Mieter Consumer der HR-Anwendung (Contoso und Fabrikam) jeder eigenen Service principal-Objekt. Jeder ihrer Verwendung einer Instanz der Anwendung zur Laufzeit Berechtigungen unterliegt zugestimmt stellt vom jeweiligen Administrator.

## <a name="next-steps"></a>Nächste Schritte
Anwendungsobjekt eine Anwendung API Azure AD Graph erfolgt durch die OData- [Anwendung Entität][AAD-Graph-App-Entity]

Eine Anwendung Service principal-Objekt API Azure AD Graph erfolgt durch die OData- [ServicePrincipal-Entität][AAD-Graph-Sp-Entity]



<!--Image references-->

<!--Reference style links -->
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AZURE-Classic-Portal]: https://manage.windowsazure.com