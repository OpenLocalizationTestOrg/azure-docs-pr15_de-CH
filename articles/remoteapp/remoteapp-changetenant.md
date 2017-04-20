
<properties
    pageTitle="Ändern den Azure Active Directory Mieter in Azure RemoteApp | Microsoft Azure"
    description="Informationen Sie zum Ändern von Azure Active Directory Mieters Azure RemoteApp zugeordnet"
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



# <a name="change-the-azure-active-directory-tenant-in-azure-remoteapp"></a>Ändern Sie den Mieter Azure Active Directory Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Azure RemoteApp verwendet Azure Active Directory (Azure AD) Zugriff. Nur Azure AD-Mandanten, mit denen Sie in Azure RemoteApp ist der Azure-Abonnement zugeordnet. Sie können die zugeordneten Abonnements auf **der Einstellungsseite im Portal** anzeigen. Betrachten Sie die Spalte **Directory** auf der Registerkarte **Abonnements** .

> [AZURE.NOTE] Für diese Änderung erfolgreich ist zunächst entfernen Sie alle vorhandenen Azure Active Directory Mieter von Azure RemoteApp-Sammlungen. Zu diesem Zweck zum Azure-Portal, wechseln Sie zur Registerkarte **Azure RemoteApp** und öffnen jede Azure RemoteApp-Auflistung. Auf der Registerkarte **Benutzer** und entfernen Sie Benutzer, die Ihren aktuellen Azure Active Directory Mandanten angehören. Wiederholen Sie für alle vorhandenen Azure RemoteApp-Sammlungen. Dies nicht tun, werden Sie nicht erstellen oder Sammlungen patch.

Wenn Sie einen anderen Mandanten verwenden möchten, folgendermaßen Sie auf die Zuordnung mit Ihrem Abonnement ändern:

1. Entfernen Sie im Portal Azure AD Benutzer, Zugriff auf Azure RemoteApp-Sammlungen gewährt haben. (Siehe Hinweis oben Schritte dazu.)


2. Festlegen Sie ein Microsoft-Konto (früher eine Live ID) als Dienstadministrator. (Weiß nicht, ob Sie bereits die Dienstadministratoren sind? Sie finden indem Sie auf **Administratoren ->**.) Jetzt sieht das ändern:
    1. Klicken Sie auf den Benutzer in der oberen rechten Ecke und dann auf **Ansicht Rechnung**.
    2. Klicken Sie auf das Abonnement. Dann die neue Seite einen Bildlauf nach unten und dann auf **Abonnementdetails bearbeiten** rechts. (Art der mittleren unteren rechten, wenn hilft finden.)
    3. Geben Sie das Microsoft-Konto für den Benutzer Dienstadministrators werden soll

3. Jetzt melden Sie des Portals, und dann mit Microsoft-Konto, das Sie im vorherigen Schritt angegeben.


4. Klicken Sie auf **Neu -> App Services -> Active Directory-Verzeichnis > -> benutzerdefinierte erstellen**.
5. Wählen Sie unter **Verzeichnis** **vorhandenen Verzeichnis verwenden**. Wir werden, melden Sie das Portal jetzt wählen Sie **ich bin bereit, jetzt abgemeldet**haben.
6. Zeichen zurück in das Portal als globaler Administrator dem Verzeichnis, das Sie hinzufügen möchten. (Wenn Sie nicht bereits ein globaler Administrator, werden nach einer anmelden und dann abmelden.)
7. Sie werden aufgefordert bei der Anmeldung Ihre vorhandenen AD-Mandanten mit Ihrem Abonnement verwendet werden soll. Klicken Sie auf **Weiter**, und klicken Sie dann auf **Abmelden**.
5. Melden Sie sich erneut an und **Settings**-> Abonnements zurück. Wählen Sie Ihr Abonnement und klicken Sie dann auf **Verzeichnis bearbeiten**. Wählen Sie die gewünschte Azure AD-Mandanten.



Sie können jetzt mit den neuen Azure AD Mieter Azure-Abonnement Zugriffskontrolle und Azure RemoteApp Benutzerzugriff konfigurieren.
