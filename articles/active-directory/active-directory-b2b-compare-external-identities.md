<properties
   pageTitle="Vergleichen von Funktionen zum Verwalten von externen Identitäten mit Active Directory Azure | Microsoft Azure"
   description="Vergleicht Azure Active Directory B2B-Zusammenarbeit, B2C und Multi-Tenant-Anwendung zur Unterstützung der Authentifizierung und Autorisierung für externe Identitäten"
   services="active-directory"
   documentationCenter="" 
   authors="arvindsuthar"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="02/24/2016"
   ms.author="asuthar"/>

# <a name="comparing-capabilities-for-managing-external-identities-using-azure-active-directory"></a>Vergleichen von Funktionen zum Verwalten von externen Identitäten mit Active Directory Azure

Neben Mitarbeiter Zugriff auf SaaS-apps, hilft Azure Active Directory (Azure AD) Ihrer Organisation Freigeben von Ressourcen mit Geschäftspartnern und Applikationen für Unternehmen und Verbraucher liefern.

## <a name="developing-applications-for-businesses"></a>Entwickeln für Unternehmen

Bereit ein Dienst oder eine Anwendung wie Payroll-Dienst für Unternehmen? Azure AD Plattform die Identität, mit der Sie die Anwendung erstellen, die nahtlos mit Millionen von Unternehmen integriert, die bereits als Teil der Bereitstellung von Office 365 oder anderen EnterpriseServices Azure AD konfiguriert haben.

**Beispiel:** Pharmazeutischer Verteiler bietet medizinische Versorgung und Informationssysteme im Gesundheitswesen. Feld eine Anwendung Analytics Arztpraxen und gewünschten Kunden ihre eigenen Identitäten verwalten musste. Diese Firma hat Azure AD als Identitätsplattform für ihre Anwendung zur Praxis um Azure AD Identitäten Kunden Zeichen von bei Bedarf. Weitere Informationen finden Sie unter [Active Directory Azure-Entwicklerhandbuch](active-directory-developers-guide.md).

## <a name="enabling-business-partner-access-to-your-corporate-resources"></a>Business Partnerzugriff auf die Unternehmensressourcen

Haben Sie Geschäftspartner oder andere Personen außerhalb Ihres Unternehmens, die auf die Unternehmensressourcen ERP-System oder einer SharePoint-Website zugreifen? Azure AD ermöglicht Administratoren, externe Benutzer (möglicherweise oder existiert nicht in Azure AD) einzelne Anmeldung Zugriff auf Unternehmens-Applikationen zu erteilen. Dies erhöht die Sicherheit, wie Benutzer, beim Verlassen der Partnerorganisation zugreifen während Richtlinien innerhalb Ihrer Organisation steuern. Dies vereinfacht auch die Verwaltung, wie Sie einen externen Partner Verzeichnis oder Föderation Partnerbeziehungen verwalten müssen.

**Beispiel:** Imaging Unternehmen bietet Einzelhändlern mit imaging Services und betreibt die größte Retail drucken Kioske. Sie wählten Azure AD Tausende Benutzer ihre Retail Business Partner mit eigenen Anmeldeinformationen Herunterladen der neuesten Partner marketing-Materials und Verarbeitung aus unserem Lieferanten extranet Foto sortieren. Weitere Informationen finden Sie in [Azure AD B2B-Zusammenarbeit](active-directory-b2b-what-is-azure-ad-b2b.md).

## <a name="developing-applications-for-consumers"></a>Anwendungsentwicklung für Verbraucher

Müssen Sie sicher und kostengünstig online-Applikationen wie ein Retail-Ladenzeile Millionen Verbraucher veröffentlichen? Azure AD bietet eine Plattform für soziale sowie Benutzernamen und Kennwort anmelden und Passwort Self-service für Kunden der Anwendung branded Self-Service anmelden. Dadurch Komfort für Ihre Kunden gleichzeitig Laden der Entwickler.

**Beispiel:** Die \#1 Sport Franchise weltweit, die sich direkt mit 450 Millionen Lüfter erforderlich. Hierzu erstellt sie eine mobile Anwendung mit Azure AD für Authentifizierung und Profile Benutzer. Lüfter vereinfachte Registrierung abrufen und mithilfe der sozialen Konten wie Facebook anmelden oder können herkömmliche Benutzernamen/Kennwörter für eine nahtlose iOS, Android und Windows Telefone. Auf die Azure AD-Plattform reduziert benutzerdefinierten Code während geben Franchise branding und Linderung Bedenken hinsichtlich der Sicherheit, Datenschutzverstöße und Skalierung angepasst. Weitere Informationen finden Sie in [Azure AD B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/).

## <a name="comparison-of-azure-ad-capabilities"></a>Vergleich von Azure AD-Funktionen

In diesem Artikel bereits beschriebenen Szenarien Funktionen in Azure AD begegnet. Diese Tabelle soll zu klären, welche Funktionen die für Sie relevant sind:

| **Betrachten Sie dieses Produkt...**       | [Azure AD Multi-Tenant SaaS-Anwendung](active-directory-developers-guide.md)    | [Azure AD B2B-Zusammenarbeit](active-directory-b2b-what-is-azure-ad-b2b.md)        | [Azure AD B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/)                |
|-----------------------|-------------------------|----------------------------|------------------------|
| **Falls erforderlich...** | ein Dienst für Unternehmen | Partnerzugriff auf Meine apps  | einer Dienstleistung für Verbraucher |
| **Und ich bin ähnelt...**  | Pharma-Verteiler      | Imaging Unternehmen            | Sport-franchise       |
| **Bereitstellen einer Anwendung für...**  | Verwaltung     | Lieferant extranet          | Fußballfans            |
| **Zielgruppenadressierung...**        | Sprechzimmer        | Zugelassene Geschäftspartner | Jeder e-Mail      |
| **Zugegriffen werden, wenn...**      | Kunden-Admin stimmt | Amp admin           | Der Verbraucher anmeldet      |
