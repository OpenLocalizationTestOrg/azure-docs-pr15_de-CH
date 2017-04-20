<properties 
    pageTitle="Welche Art von Auflistung benötigen Sie für Azure RemoteApp? | Microsoft Azure" 
    description="Informationen Sie zu den Arten von Sammlungen mit Azure RemoteApp." 
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



# <a name="what-kind-of-collection-do-you-need-for-azure-remoteapp"></a>Welche Art von Auflistung benötigen Sie für Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Azure RemoteApp können Sie Benutzer auf allen Geräten apps und Ressourcen freigeben. Dazu Sammlungen apps und Ressourcen enthält, und dann diesen Sammlungen für Benutzer freigeben. Es gibt 2 unterschiedliche Optionen mit anderen Authentifizierung - die richtige für Sie?

Gehen Sie durch verschiedene Faktoren und Optionen Sie Ihre Azure RemoteApp-Sammlung optimal müssen. 


## <a name="quick-differences-between-the-collection-types"></a>Schnelle Unterschiede Auflistungstypen

|           | Cloud | Hybrid |
|-----------|-------|--------|
|Verwenden Sie eine vorhandene VNET| Ja| Ja|
|Erfordert AD verbundene Konten (DirSync)| Nein| Ja|
|Beitreten zu einer Domäne erforderlich| Nein| Ja|
|Erfordert VNET für Domänencontroller| Nein| Ja|

## <a name="cloud-collections"></a>Cloud-Sammlungen
- Schnell zu erstellen – die Auflistung schnell bereitgestellt wird, d. h. Ihre apps schneller für Benutzer.
- Bringen Sie Ihre eigenen apps oder Teilen Sie uns. Sie können ein benutzerdefiniertes Bild (erstellt von Azure-VM) oder eines der Bilder in Ihrem Abonnement enthalten.
- Sie müssen eine Verbindung zwischen Ihr und der lokalen Domäne.
- Aber optional können eigene Azure-VNET in Ihrer lokalen Umgebung für die gemeinsame Nutzung von Daten zugreifen oder nicht-Windows-Authentifizierung in Ressourcen wie SQL Server (Datenbank Authentifizierung).


OK, wie erstelle ich eine?

- Cloud? Mit der Option **Firma** im Portal erstellen.
- Cloud + VNET? Mit der Option **Create vnet** erstellen, aber keiner Domäne beitreten sollen.

## <a name="hybrid-collections"></a>Hybrid-Sammlungen
- Bietet vollständigen Zugriff auf lokalen Netzwerk + Azure VNET.
- Verknüpfung zugreifen apps und Daten enthält. Remoteanwendungen können Authentifizierung in der lokalen Active Directory - kann dann Zugriff auf Ressourcen in der Domäne.
- Erweiterte Überwachung und Management mit vorhandenen System Center Solutions und Windows-Gruppenrichtlinien (über ein benutzerdefiniertes Bild basiert auf Windows Server 2012 R2)
- [ExpressRoute](https://azure.microsoft.com/services/expressroute/) lokale VNET der Azure-VNET Verbindung unterstützen.

Mit der Option **Create vnet** erstellen und Sie einer Domäne beitreten wählen.

## <a name="authentication-options"></a>Optionen
Azure RemoteApp unterstützt Microsoft Konten und Azure Active Directory-Konten, jedoch nicht alle Sammlungen aller Methoden. 

| Kontotyp                      |                                                             | Cloud | Cloud + VNET | Hybrid |
|-----------------------------------|-------------------------------------------------------------|-------|--------------|--------|
| Microsoft-Konto                 |                                                             | Ja   | Ja          | Nein     |
| Azure Active Directory (Azure AD) |                                                             |       |              |        |
|                                   | Azure AD                                               | Ja   | Ja          | Nein     |
|                                   | AD-Verbindung mit Kennwort synchronisieren                               | Ja   | Ja          | Ja    |
|                                   | AD Connect ohne Kennwort synchronisieren                            | Ja   | Ja          | Nein     |
|                                   | AD-Verbindung mit AD FS                                       | Ja   | Ja          | Ja    |
|                                   | 3rd Party Azure unterstützt Identitätsprovider (wie Ping) | Ja   | Ja          | Ja    |
| Mehrstufige Authentifizierung       |                                                             | Ja   | Ja          | Ja    |



### <a name="cloud-and-cloud--vnet"></a>Cloud, die Cloud + VNET 
Mit Cloud-Sammlungen können Sie Microsoft-Konten, Azure AD-Konten oder einer Kombination der beiden. Verwenden Sie die Konten, die für die Benutzer am besten geeignet.

Es gibt keine spezielle Vorschriften für mit Microsoft-Konten. 

Wenn Sie Azure AD-Konten verwenden möchten, müssen Sie sicherstellen, dass Ihre Azure AD-Mandanten mit Ihrem Abonnement verknüpfte entspricht. Beim Erstellen von Abonnements Azure RemoteApp wurde Azure AD-Mandanten verwendeten automatisch mit Ihrem Abonnement verknüpft. Jede Azure AD-Benutzerobjekt, Berechtigung erteilen, muss, denselben Mandanten. Bei Bedarf können Sie Ihr Abonnement zugeordneten [Azure AD-Mandanten zu ändern](remoteapp-changetenant.md) .
 
### <a name="hybrid-or-cloud--azure-ad--ad"></a>Hybride (oder Cloud + Azure AD + AD)

Mit Azure AD + lokale Active Directory ist eine Voraussetzung für eine Hybrid-Sammlung. Sie müssen mit AD verbinden um zwei Verzeichnisse zu integrieren. Sie verfügen jedoch über eine Auswahl bei der Konfiguration von Active Directory verbinden. 

Es gibt 2 AD verbinden Szenarien - Kennwortsynchronisation oder AD Föderation. Überprüfen Sie die [AD-Verbindungsinformationen](../active-directory/active-directory-aadconnect.md) , die diese Arbeiten für Sie herauszufinden.

Sie können auch Azure AD + AD mit Cloud. Sicherstellen Sie, dass Sie dasselbe Schritte befolgen.

Auschecken [Azure AD + Active Directory Azure RemoteApp an](remoteapp-ad.md) für die Schritte zum Konfigurieren von Azure AD und Active Directory.

## <a name="go-create-your-collection"></a>Ihre Sammlung gehen!
OK, glaube ich, dass wir es nun herausgefunden haben deshalb nur eines tun - erstellen Ihre erste Azure RemoteApp-Sammlung.

[Erstellen einer cloudsammlung](remoteapp-create-cloud-deployment.md) oder [eine hybridsammlung erstellen](remoteapp-create-hybrid-deployment.md) – get nur erstellen.
