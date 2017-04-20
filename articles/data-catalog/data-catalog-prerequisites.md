<properties
   pageTitle="Azure Data Catalog erforderliche | Microsoft Azure"
   description="Azure Data Catalog erforderliche Komponenten - müssen Einstieg in Azure Data Catalog."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/21/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-prerequisites"></a>Azure Data Catalog-Komponenten

## <a name="what-do-i-need-to-get-started-with-azure-data-catalog"></a>Was muss ich Einstieg in Azure Data Catalog?

Es gibt ein paar Dinge kümmern **Azure Data Catalog**einrichten müssen. Keine Sorge – es wird nicht lange dauern!

## <a name="azure-subscription"></a>Azure-Abonnement
Um Azure Data Catalog einzurichten, müssen Sie der Besitzer oder an ein Azure-Abonnement sein.

Azure-Abonnements können Sie Zugriff auf Service Cloudressourcen wie Azure Data Catalog organisieren. Sie auch unterstützt Sie wie Ressourcenverbrauch gemeldet wird, berechnet und bezahlt. Jedes Abonnement haben eine andere Einstellung für Abrechnung und Bezahlung, so können Sie verschiedene Abonnements und verschiedene Pläne von Abteilung, Projekt, regionale Office. Jeder Cloud-Dienst gehört zu einem Abonnement und benötigen Sie ein Abonnement vor dem Einrichten von Azure Data Catalog. Finden Sie weitere [Konten verwalten, Abonnements und Verwaltungsfunktionen](../active-directory/active-directory-assign-admin-roles.md).

## <a name="azure-active-directory"></a>Azure Active Directory
Um Azure Data Catalog einzurichten, müssen Sie mit einer Azure Active Directory-Benutzerkonto angemeldet sein.

Azure Active Directory (Azure AD) bietet eine einfache Möglichkeit für Ihr Unternehmen Identitäts- und, in der Cloud und lokal verwalten. Benutzer können eine Arbeit oder schulkonto für einmaliges Anmelden Cloud und lokalen web-Anwendung. Azure Data Catalog verwendet Azure AD Authentifizierung anmelden. Weitere Informationen finden Sie unter [Azure Active Directory](../active-directory/active-directory-whatis.md).

> [AZURE.NOTE] [Azure-Portal](http://portal.azure.com/) können Benutzer anmelden persönlichen Microsoft-Account oder eine Azure Active Directory Arbeit oder Schule Konto. Azure Data Catalog Azure-Portal oder das [Portal Data Catalog](http://www.azuredatacatalog.com) einrichten müssen Sie über ein Azure Active Directory-Konto kein persönliches Konto angemeldet.

## <a name="active-directory-policy-configuration"></a>Konfiguration der Active Directory-Richtlinie

In einigen Situationen kann eine Situation treten, Anmeldung können Azure Data Catalog-Portal, aber beim Data Source-Registrierungstool melden sie eine Fehlermeldung, die Anmeldung verhindert. Dies Problem kann auftreten, nur, wenn der Benutzer im Unternehmensnetzwerk, oder nur, wenn der Benutzer von außerhalb des Unternehmensnetzwerks verbunden ist.

Das Source-Registrierungstool verwendet Formularauthentifizierung Anmeldeversuche in Active Directory überprüft. Für erfolgreiche Anmeldung muss Formularauthentifizierung von einem Active Directory-Administrator in der globalen Richtlinie Authentifizierung aktiviert.

Globale Authentifizierungsrichtlinie können Authentifizierungsmethoden für Intranet- und extranet-Verbindungen getrennt aktiviert werden, wie unten dargestellt. Fehler bei der Anmeldung möglicherweise Formularauthentifizierung nicht für das Netzwerk aktiviert ist, aus dem der Benutzer verbunden ist.

 ![Globale Authentifizierungsrichtlinie für Active Directory](./media/data-catalog-prerequisites/global-auth-policy.png)

Weitere Informationen finden Sie unter [Konfigurieren von Authentifizierungsrichtlinien](https://technet.microsoft.com/library/dn486781.aspx).
