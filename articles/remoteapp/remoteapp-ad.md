
<properties 
    pageTitle="Azure AD + an Active Directory Azure RemoteApp | Microsoft Azure" 
    description="Erlernen von Active Directory Azure RemoteApp arbeiten." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="azure-ad--active-directory-requirements-for-azure-remoteapp"></a>Azure AD + an Active Directory Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.


Ihre Azure RemoteApp Hybrid-Sammlung oder eine Föderation mit Active Directory verbinden möchten Cloud-Sammlung müssen Sie Folgendes tun.

### <a name="connect-azure-ad-and-active-directory"></a>Verbinden von Azure AD und Active Directory

Azure AD-Mandanten und Ihre lokale Active Directory-Umgebung herstellen, verwenden Sie Active Directory verbinden. Es dauert Sie nur [4 Klicks](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) für die Verbindung der beiden Verzeichnisse.

Hinweis: Verzeichnissynchronisation Hybrid Sammlungen erforderlich ist.

### <a name="make-sure-your-domaincom-match"></a>Stellen Sie sicher, dass Ihre "@domain.com" übereinstimmen
Bevor Sie beginnen, stellen Sie sicher, dass der UPN für die lokale Gesamtstruktur das Suffix der Azure AD-Domäne übereinstimmt. 

Nach dem Einrichten der UPN Domänensuffix in Azure AD anmelden Azure RemoteApp alle Benutzer können sich anmelden als “user@ <the suffix you set up>". Stellen Sie sicher, dass Benutzer auch gleich einloggen können user@suffix in der lokalen Domäne. In bestimmten Fällen können Sie einen Domänennamen in Azure AD beim Angeben von anderen Domänensuffix für die Vorz Benutzer einrichten In diesem Fall wird nicht die Benutzer Domänencomputern oder Ressourcen über Azure RemoteApp herstellen können.

Angenommen, Ihre UPN Domänensuffix in AAD als contoso.com eingerichtet, aber einige Benutzer lokal-Anzeige konfiguriert Anmeldung @contoso.uk, werden diese Benutzer nicht ordnungsgemäß anmelden ARA-Auflistung. Benutzer-UPN AAD und AD muss für die Anmeldung werden"

### <a name="create-objects-for-azure-remoteapp"></a>Erstellen von Objekten für Azure RemoteApp
Sie müssen die folgenden lokalen Active Directory-Objekte erstellt:

- Ein Dienstkonto für den Zugriff auf Domänenressourcen für RemoteApp-Programme bei RDSH Endpunkt der lokalen Domäne.
- Eine Organisationseinheit (OU) RemoteApp Computer Objekte enthält. Verwenden der Organisationseinheit empfohlen (aber nicht erforderlich) Konten und Richtlinien Sie RemoteApp verwenden isolieren.

Sie benötigen beide diese Objekte beim Erstellen von RemoteApp-Auflistung werden die ersten Schritte.