<properties
    pageTitle="Azure AD Verbund-Kompatibilitätsliste"
    description="Diese Seite ist nicht Microsoft Identitätsanbieter, mit der Anmeldung zu implementieren."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="billmath"/>

# <a name="azure-ad-federation-compatibility-list"></a>Azure AD Verbund-Kompatibilitätsliste
Azure Active Directory bietet Single-Sign-in und erweiterte anwendungssicherheit für Office 365 und anderen Microsoft Online Services für nur-Cloud-Implementierung und Hybrid ohne eine nicht-Microsoft-Lösung. Office 365 wie Microsoft Online Services ist für Verzeichnisdienste, Authentifizierung und Autorisierung in Azure Active Directory integriert. Azure Active Directory auch einmaliges Anmelden für Tausende von SaaS-Applikationen und lokalen web-Applikationen. Finden Sie in der Galerie Azure Active Directory für unterstützte SaaS-Applikationen.

Für Unternehmen, die nicht Microsoft Federation Lösungen investiert haben, enthält dieses Thema Anleitung für einmaliges Anmelden für die Windows Server Active Directory-Benutzer mit Microsoft Online Services mit Drittanbietern Identitätsanbieter aus"Azure Active Directory Federation Kompatibilität" unter konfigurieren. 


![](./media/active-directory-aadconnect-federation-compatibility/oxford2.jpg)   
[Oxford Computergruppe](http://oxfordcomputergroup.com/)ein Drittanbieter für Microsoft getestet diese einzelnen anmelden Erlebnisse Identitätsanbieter anhand einer Reihe von Fällen häufig nicht Microsoft Azure Active Directory.

Informationen zum Lernen der dritten Identitätsprovider Hier erhalten Oxford Computergruppe auf [idp@oxfordcomputergroup.com](mailto:idp@oxfordcomputergroup.com).

>[AZURE.IMPORTANT] Oxford Computer getestet nur die Föderation Funktionen dieser einzelnen Szenarien anmelden. Oxford Computergruppe nicht Testen jeder Synchronisation, zweistufige Authentifizierung usw. Komponenten dieser Szenarien für einzelne Zeichen.

>Verwendung von alternativen UPN-ID anmelden wird auch in diesem Programm nicht getestet.



- [Azure Active Directory](#azure-active-directory)
- [Optimale IDM Virtual Identity Server-Verbunddienste](#optimal-idm-virtual-identity-server-federation-services) 
- [PingFederate 6.11](#pingfederate-611) 
- [7.2 PingFederate](#pingfederate-72) 
- [PingFederate 8.x](#pingfederate-8x)
- [Centrify](#centrify) 
- [IBM Tivoli Federated Identity Manager 6.2.2](#ibm-tivoli-federated-identity-manager-622) 
- [SecureAuth IdP 7.2.0](#secureauth-idp-720) 
- [CA SiteMinder 12,52](#ca-siteminder-1252-sp1-cumulative-release-4) 
- [RadiantOne CFS 3.0](#radiantone-cfs-30) 
- [Okta](#okta) 
- [OneLogin](#onelogin) 
- [NetIQ Access Manager 4.0.1](#netiq-access-manager-401) 
- [Mit Access Policy Manager BIG-IP Version 11.3 x – 11,6 BIG-IP](#big-ip-with-access-policy-manager-big-ip-ver-113x-116x) 
- [VMware Workspace Portal Version 2.1](#vmware-workspace-portal-version-21) 
- [5.3 gehen & melden](#signampgo-53) 
- [IceWall Föderation Version 3.0](#icewall-federation-version-30) 
- [CA-Sicherheit Cloud](#ca-secure-cloud) 
- [Dell eine Identität Cloud Access Manager V7. 1](#dell-one-identity-cloud-access-manager-v71) 
- [AuthAnvil Single Sign-On 4.5](#authavil-single-sign-on-45) 

>[AZURE.IMPORTANT] Da diese Drittanbieterprodukte sind, bietet Microsoft keine Unterstützung für die Bereitstellung, Konfiguration, Problembehandlung, best Practices, usw. Probleme und fragen diesen Identitätsanbieter. Support und fragen diesen Identitätsanbieter erhalten Sie unterstützten dritten direkt.

>Diese Drittanbieter-Identitätsanbieter wurden für die Interoperabilität mit Microsoft-Cloud-Diensten mit WS-Federation und WS-Trust-Protokolle getestet. Tests enthielt das SAML-Protokoll verwenden.

## <a name="azure-active-directory"></a>Azure Active Directory 
Azure Active Directory können Benutzer authentifizieren, indem eine Föderation mit der lokalen Active Directory oder ohne einen lokalen Verbundservers durch Kennwort synchronisieren. 

Szenario-Unterstützungsmatrix für diese Erfahrung ist: 


| Client |Unterstützung  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Exchange Web Access SharePoint Online | Unterstützt |Keine|
| Rich-Clientanwendungen wie Lync, Office-Abonnement, CRM |  Unterstützt |Keine|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|
|Moderne Applikationen mit ADAL wie Office 2016| Unterstützt|Keine|

Weitere Informationen zu AD FS mit Azure Active Directory finden Sie unter [Active Directory Federation Services (ADFS)](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs)

Weitere Informationen zum Kennwort synchronisieren mit Azure Active Directory finden Sie unter [Azure AD verbinden](active-directory-aadconnect.md).


## <a name="optimal-idm-virtual-identity-server-federation-services"></a>Optimale IDM Virtual Identity Server-Verbunddienste 
Optimale IDM Virtual Identity Server-Verbunddienste können Benutzer authentifizieren, die Kunden für lokale Active Directory befinden.

Ist Szenario Supportübersicht dieser einzelnen anmelden:


| Client |Unterstützung  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Exchange Web Access SharePoint Online | Unterstützt |Keine|
| Rich-Clientanwendungen wie Lync, Office-Abonnement, CRM |  Unterstützt |Integrierte Windows-Authentifizierung|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Für Weitere Informationen über den Clientzugriff Siehe Richtlinien [begrenzen auf Office 365 Services basierend auf den Client.](https://technet.microsoft.com/library/hh526961.aspx)|



## <a name="pingfederate-611"></a>PingFederate 6.11 

PingFederate 6.11 implementiert verbreiteten WS-Federation Identität Standard ein einmaliges und Attribut Exchange Framework.

Szenario-Unterstützungsmatrix für diese Erfahrung für einzelne Zeichen ist:


| Client |Unterstützung  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Exchange Web Access SharePoint Online | Unterstützt |Keine|
| Rich-Clientanwendungen wie Lync, Office-Abonnement, CRM |  Unterstützt |Keine (frühere Versionen müssen auf 6.11 aktualisieren|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|

Die PingFederate Anleitung hierzu STS der einzelnen Anmeldung Ihre Active Directory-Benutzer zu Pdf downloaden [hier.](http://go.microsoft.com/fwlink/?LinkID=266321)

## <a name="pingfederate-72"></a>7.2 PingFederate 
PingFederate 7.2 implementiert verbreiteten WS-Federation/WS-Trust Identität Standard ein einmaliges und Attribut Exchange Framework.

Szenario-Unterstützungsmatrix für diese Erfahrung für einzelne Zeichen ist:


| Client |Unterstützung  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Exchange Web Access SharePoint Online | Unterstützt |Keine|
| Rich-Clientanwendungen wie Lync, Office-Abonnement, CRM |  Unterstützt |Keine|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|

Die PingFederate-Anleitung zum Konfigurieren dieser STS zu den einzelnen Anmeldung für die Active Directory-Benutzer finden Sie [hier.](http://documentation.pingidentity.com/display/PF72/PingFederate+7.2)

## <a name="pingfederate-8x"></a>PingFederate 8.x 
PingFederate 8.x verbreiteten WS-Federation/WS-Trust Identität Standard ein einmaliges und Attribut Exchange Framework implementiert.

Szenario-Unterstützungsmatrix für diese Erfahrung für einzelne Zeichen ist:


| Client |Unterstützung  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Exchange Web Access SharePoint Online | Unterstützt |Keine|
| Rich-Clientanwendungen wie Lync, Office-Abonnement, CRM |  Unterstützt |Keine|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|

Die PingFederate-Anleitung zum Konfigurieren dieser STS zu den einzelnen Anmeldung für die Active Directory-Benutzer finden Sie [hier.](http://documentation.pingidentity.com/display/PFS/SSO+to+Office+365+Introduction)

## <a name="centrify"></a>Centrify 
Centrify hilft bieten einen föderativen einzelne anmelden Office 365 ohne einen lokalen Verbundservers hosten.

Szenario-Unterstützungsmatrix für diese Erfahrung für einzelne Zeichen ist:


| Client |Unterstützung  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Exchange Web Access SharePoint Online | Unterstützt |Keine|
| Rich-Clientanwendungen wie Lync, Office-Abonnement, CRM |  Unterstützt |Keine|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Client Access Control wird nicht unterstützt. 

Weitere Informationen über Centrify finden Sie [hier.](http://www.centrify.com/cloud/apps/single-sign-on-for-office-365.asp)|

## <a name="ibm-tivoli-federated-identity-manager-622"></a>IBM Tivoli Federated Identity Manager 6.2.2 
IBM Tivoli Federated Identity Manager 6.2.2 mit IBM Security Access Manager für Microsoft Applications 1.4 implementiert verbreiteten WS-Federation/WS-Trust Identität Standard ein einmaliges und Attribut Exchange Framework.

Szenario-Unterstützungsmatrix für diese Erfahrung für einzelne Zeichen ist: 

| Client |Unterstützung  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Exchange Web Access SharePoint Online | Unterstützt |Keine|
| Rich-Clientanwendungen wie Lync, Office-Abonnement, CRM |  Unterstützt |Keine|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|

Weitere Informationen zu IBM Tivoli Federated Identity Manager finden Sie unter [IBM Security Access Manager für Microsoft Applications.](http://www-01.ibm.com/support/docview.wss?uid=swg24029517)

## <a name="secureauth-idp-720"></a>SecureAuth IdP 7.2.0 
SecureAuth IdP 7.2.0 implementiert verbreiteten WS-Federation/WS-Trust Identität Standard eine einzige Anmeldung und Attribut Exchange Framework.

Szenario-Unterstützungsmatrix für diese Erfahrung für einzelne Zeichen ist: 

| Client |Unterstützung  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Exchange Web Access SharePoint Online | Unterstützt |Keine|
| Rich-Clientanwendungen wie Lync, Office-Abonnement, CRM |  Unterstützt |Keine|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|

Weitere Informationen zu SecureAuth finden Sie unter [SecureAuth IdP](http://go.microsoft.com/?linkid=9845293).

## <a name="ca-siteminder-1252-sp1-cumulative-release-4"></a>CA SiteMinder 12,52 SP1 kumulative Version 4
CA SiteMinder Föderation 12,52 implementiert verbreiteten WS-Federation/WS-Trust Identität Standard ein einmaliges und Attribut Exchange Framework.

Szenario-Unterstützungsmatrix für diese Erfahrung für einzelne Zeichen ist: 

| Client |Unterstützung  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Exchange Web Access SharePoint Online | Unterstützt |Keine|
| Rich-Clientanwendungen wie Lync, Office-Abonnement, CRM |  Unterstützt |Keine|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|

Weitere Informationen über CA SiteMinder finden Sie [CA SiteMinder Föderation.](http://www.ca.com/us/products/ca-single-sign-on.html) 

## <a name="radiantone-cfs-30"></a>RadiantOne CFS 3.0 
RadiantOne Cloud Federation Service (CFS) 3.0 implementiert verbreiteten WS-Federation/WS-Trust Identität Standard ein einmaliges und Attribut Exchange Framework.

Szenario-Unterstützungsmatrix für diese Erfahrung für einzelne Zeichen ist: 

| Client |Unterstützung  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Exchange Web Access SharePoint Online | Unterstützt |Keine|
| Rich-Clientanwendungen wie Lync, Office-Abonnement, CRM |  Unterstützt |Integrierte Windows-Authentifizierung|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|

Weitere Informationen zu RadiantOne CFS finden Sie unter [RadiantOne CFS.](http://www.radiantlogic.com/products/radiantone-cfs/)


## <a name="okta"></a>Okta 
Okta implementiert verbreiteten WS-Federation/WS-Trust Identität Standard ein einmaliges und Attribut Exchange Framework.

Szenario-Unterstützungsmatrix für diese Erfahrung für einzelne Zeichen ist: 


| Client |Unterstützung  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Exchange Web Access SharePoint Online | Unterstützt |Integrierte Windows-Authentifizierung müssen zusätzliche Webserver und Okta Anwendung.|
| Rich-Clientanwendungen wie Lync, Office-Abonnement, CRM |  Unterstützt |Integrierte Windows-Authentifizierung|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|

Weitere Informationen zu Okta finden Sie unter [Okta.](https://www.okta.com/)
 
## <a name="onelogin"></a>OneLogin 
OneLogin im Mai 2014 getesteten implementiert verbreiteten WS-Federation/WS-Trust Identität Standard ein einmaliges und Attribut Exchange Framework.

Szenario-Unterstützungsmatrix für diese Erfahrung für einzelne Zeichen ist: 

| Client |Unterstützung  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Exchange Web Access SharePoint Online | Unterstützt |Integrierte Windows-Authentifizierung|
| Rich-Clientanwendungen wie Lync, Office-Abonnement, CRM |  Unterstützt |Integrierte Windows-Authentifizierung|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|

Weitere Informationen zu OneLogin finden Sie unter [OneLogin.](https://www.onelogin.com/)

## <a name="netiq-access-manager-401"></a>NetIQ Access Manager 4.0.1 
NetIQ Access Manager 4.0.1 implementiert verbreiteten WS-Federation/WS-Trust Identität Standard ein einmaliges und Attribut Exchange Framework.

Szenario-Unterstützungsmatrix für diese Erfahrung für einzelne Zeichen ist:

| Client |Unterstützung  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Exchange Web Access SharePoint Online | Unterstützt |* Kerberos Verträge unterstützt|
| Rich-Clientanwendungen wie Lync, Office-Abonnement, CRM |  Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt.|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|

* NetIQ unterstützt Kerberos-Authentifizierung über die Konfiguration eines Kerberos.  Zur Unterstützung dieser Konfiguration wenden Sie sich an NetIQ oder zeigen Sie das Handbuch an. Weitere Informationen zu NetIQ Access Manager finden Sie unter [NetIQ Access Manager.](https://www.netiq.com/documentation/netiqaccessmanager4/identityserverhelp/data/b12iqp0m.html)

## <a name="big-ip-with-access-policy-manager-big-ip-ver-113x--116x"></a>BIG-IP mit Access Policy Manager BIG-IP-Version 11.3 x – 11,6 
BIG-IP mit Access Policy Manager (APM) BIG-IP-Version 11.3 X – implementiert 11,6 x verbreiteten SAML Identität Standard eine einzige Anmeldung und Attribut Exchange Framework.

Szenario-Unterstützungsmatrix für diese Erfahrung für einzelne Zeichen ist: 

| Client |Unterstützung  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Exchange Web Access SharePoint Online | Unterstützt |Keine|
| Rich-Clientanwendungen wie Lync, Office-Abonnement, CRM |  Nicht unterstützt |Nicht unterstützt|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|

Weitere Informationen zu BIG-IP Access Policy Manager finden Sie unter [BIG-IP Access Policy Manager.](https://f5.com/products/modules/access-policy-manager) 

Informationen der BIG-IP Access Policy Manager konfigurieren STS der einzelnen Anmeldung Ihre Active Directory-Benutzer zu Pdf downloaden [hier.](http://www.f5.com/pdf/deployment-guides/microsoft-office-365-idp-dg.pdf)

## <a name="vmware--workspace-portal-version-21"></a>VMware Workspace Portal Version 2.1 
VMware Workspace Portal Version 2.1 implementiert verbreiteten WS-Federation/WS-Trust Identität Standard ein einmaliges und Attribut Exchange Framework.

Szenario-Unterstützungsmatrix für diese Erfahrung für einzelne Zeichen ist:

| Client |Unterstützung  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Exchange Web Access SharePoint Online | Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt.|
| Rich-Clientanwendungen wie Lync, Office-Abonnement, CRM | Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt.|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|

Weitere Informationen über VMware Workspace Portal Version 2.1 Pdf downloaden [hier.](http://pubs.vmware.com/workspace-portal-21/topic/com.vmware.ICbase/PDF/workspace-portal-21-resource.pdf)

## <a name="signgo-53"></a>5.3 gehen & melden 
Signieren & go 5.3 implementiert verbreiteten WS-Federation/WS-Trust-Identität standard-auf und Attributs Exchange Framework.

Szenario-Unterstützungsmatrix für diese Erfahrung für einzelne Zeichen ist:

| Client |Unterstützung  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Exchange Web Access SharePoint Online | Unterstützt |Unterstützt Kerberos-Verträge |
| Rich-Clientanwendungen wie Lync, Office-Abonnement, CRM | Unterstützt |Keine|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|


Zeichen & Go 5.3 unterstützt Kerberos-Authentifizierung über die Konfiguration eines Kerberos.  Mit dieser Konfiguration, Sie wenden sich an Ilex oder das Handbuch anzeigen [hier.](http://www.ilex-international.com/docs/sign&go_wsfederation_en.pdf)


## <a name="icewall-federation-version-30"></a>IceWall Föderation Version 3.0 
IceWall Föderation Version 3.0 implementiert verbreiteten WS-Federation/WS-Trust Identität Standard ein einmaliges und Attribut Exchange Framework.

Szenario-Unterstützungsmatrix für diese Erfahrung für einzelne Zeichen ist:

| Client |Unterstützung  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Exchange Web Access SharePoint Online | Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt.|
| Rich-Clientanwendungen wie Lync, Office-Abonnement, CRM | Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt.|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|

Weitere Informationen über IceWall Föderation finden Sie [hier](http://h50146.www5.hp.com/products/software/security/icewall/eng/federation/) und [hier.](http://h50146.www5.hp.com/products/software/security/icewall/federation/office365.html)

## <a name="ca-secure-cloud"></a>CA-Sicherheit Cloud 

Zertifizierungsstelle sichern Cloud implementiert verbreiteten WS-Federation/WS-Trust Identität Standard ein einmaliges und Attribut Exchange Framework.

Szenario-Unterstützungsmatrix für diese Erfahrung für einzelne Zeichen ist:

| Client |Unterstützung  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Exchange Web Access SharePoint Online | Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt.|
| Rich-Clientanwendungen wie Lync, Office-Abonnement, CRM | Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt.|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|

Weitere Informationen über die Zertifizierungsstelle sichern Cloud finden Sie [CA Secure Cloud.](http://www.ca.com/us/products/security-as-a-service.aspx)

## <a name="dell-one-identity-cloud-access-manager-v71"></a>Dell eine Identität Cloud Access Manager V7. 1 
Dell eine Identität Cloud Access Manager implementiert verbreiteten WS-Federation/WS-Trust Identität Standard ein einmaliges und Attribut Exchange Framework.

Szenario-Unterstützungsmatrix für diese Erfahrung für einzelne Zeichen ist:

| Client |Unterstützung  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Exchange Web Access SharePoint Online | Unterstützt |Keine|
| Rich-Clientanwendungen wie Lync, Office-Abonnement, CRM |  Unterstützt |Keine|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|

Weitere Informationen zu Dell eine Identität Cloud Access Manager finden Sie unter [Dell eine Identität Cloud Access Manager.](http://software.dell.com/products/cloud-access-manager)

 Informationen zum Konfigurieren dieser STS zu den einzelnen anmelden den Office 365-Benutzern finden Sie unter [Office 365-Benutzer konfigurieren.](http://documents.software.dell.com/dell-one-identity-cloud-access-manager/7.1/how-to-configure-microsoft-office-365) 

## <a name="authanvil-single-sign-on-45"></a>AuthAnvil Single Sign-On 4.5 
AuthAnvil einzelne Zeichen auf 4.5 implementiert verbreiteten WS-Federation/WS-Trust Identität Standard ein einmaliges und Attribut Exchange Framework.

Szenario-Unterstützungsmatrix für diese Erfahrung für einzelne Zeichen ist:

| Client |Unterstützung  |Ausnahmen|
| --------- | --------- |--------- |
| Web-basierte Clients wie Exchange Web Access SharePoint Online | Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt.|
| Rich-Clientanwendungen wie Lync, Office-Abonnement, CRM | Unterstützt |Integrierte Windows-Authentifizierung wird nicht unterstützt.|
| E-Mail-Rich Clients wie Outlook und ActiveSync |  Unterstützt |Keine|


Weitere Informationen finden Sie unter [AuthAnvil Single Sign-On.](https://help.scorpionsoft.com/entries/26538603-How-can-I-Configure-Single-Sign-On-for-Office-365-)
