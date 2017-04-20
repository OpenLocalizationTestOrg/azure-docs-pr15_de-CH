<properties
    pageTitle="Hinzufügen eines Benutzers zu Azure RemoteApp-Auflistung | Microsoft Azure"
    description="Erfahren Sie, wie Benutzer Ihre Azure RemoteApp-Sammlung hinzufügen"
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

# <a name="how-to-add-a-user-to-your-azure-remoteapp-collection"></a>Wie Sie Ihre Azure RemoteApp-Sammlung einen Benutzer hinzufügen

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Benutzer anzeigen und Ihre apps in Azure RemoteApp verwenden können, müssen Sie zum Gewähren von Zugriff auf die Auflistung. Dies ist der einfache Teil: Geben Sie die Kontoinformationen für den Benutzer auf der Registerkarte **Zugriff** , und klicken Sie auf das Häkchen.

Welche Informationen benötigen Sie? Das hängt vom Typ der Auflistung Sie (Cloud oder Hybrid) und ob Sie Office 365 ProPlus in dieser Auflistung verwenden.

## <a name="supported-user-identities"></a>Unterstützte Benutzeridentitäten

Die anderen Auflistungstypen (und Hybriden Cloud) unterstützen mit verschiedenen Benutzeridentitäten für Zugriff auf Programme.  

Für eine Auflistung Hybrid RemoteApp müssen Sie Active Directory-Domäneninfrastruktur auf Räume und einen Mandanten Azure Active Directory Directory Integration (und optional einmaliges Anmelden). Außerdem müssen Sie einige Active Directory-Objekten im lokalen Verzeichnis erstellt.  

Für eine cloudsammlung von RemoteApp kann alle Benutzer, Azure Active Directory Identitäten unterstützen Benutzerzugriff RemoteApp enthalten Microsoft Accounts gewährt werden.  Siehe Tabelle unten.

Office 365-Benutzer sind Azure Active Directory-Benutzer. Haben sie Active Directory Azure Hybrid können Directory Konten synchronisiert sie Benutzer in einer Hybrid-Bereitstellung von RemoteApp zugreifen.   

Diese Tabelle können als Referenz für die Identität in der Auflistung und den Active Directory-Anforderungen unterstützt wird.

|Benutzerkonten |Cloud   |Hybrid|
|--------------|--------|------|
|Microsoft-Konto|     Ja|    Nein|
|Azure Active Directory (Azure AD)| | |
|Nur Azure AD-cloud    |Ja    |Nein |
|ADsync mit Kennwort synchronisieren  |Ja    |Ja    |
|ADsync ohne Kennwort synchronisieren|  Ja |Nein |
|ADsync mit AD FS  |Ja    |Ja    |
|[3rd Party Azure unterstützt Identitätsanbieter](https://msdn.microsoft.com/library/azure/jj679342.aspx)  (Ping-Beispiel) |Ja    |Ja|
|Mehrstufige Authentifizierung    |Ja    |Ja    |

[Weitere Informationen](remoteapp-ad.md) zum Konfigurieren von Active Directory für RemoteApp anzeigen


> [AZURE.NOTE] Azure Active Directory-Benutzer muss vom Mieter, die Ihrem Abonnement zugeordnet ist. (Sie können anzeigen und Ändern Ihres Abonnements auf **die Registerkarte im Portal** . Siehe [Ändern der Azure Active Directory Mieter von RemoteApp verwendet](remoteapp-changetenant.md) Informationen.)

## <a name="office-365-proplus-user-account-information"></a>Office 365 ProPlus Benutzerkontoinformationen
Wenn Sie Office 365 ProPlus Vorlagenbild in der Auflistung *oder* verwenden Sie ein benutzerdefiniertes Bild erstellt, das Office 365 verwendet, dürfen Sie nur Azure Active Directory-Benutzer, die Office 365-Abonnements für die Standarddomäne Ihres Abonnements hinzufügen. Weitere Informationen finden Sie unter [Verwendung von Office 365 Azure RemoteApp](remoteapp-o365.md) .
