<properties
    pageTitle="Get Azure Mehrfaktor-Authentifizierungsanbieter gestartet | Microsoft Azure"
    description="Informationen Sie zum Erstellen eines Anbieters Azure mehrstufige Authentifizierung."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>



# <a name="getting-started-with-an-azure-multi-factor-auth-provider"></a>Erste Schritte mit einer Azure Mehrfaktor-Authentifizierungsanbieter
Zweistufige Bestätigung ist standardmäßig für globale Administratoren Active Directory Azure und Office 365-Benutzer verfügbar. Jedoch möchten Sie [Erweiterte](multi-factor-authentication-whats-next.md) Funktionen sollten Sie die Vollversion von Azure mehrstufige Authentifizierung (MFA) erwerben.

> [AZURE.NOTE]  Ein Azure Mehrfaktor-Authentifizierungsanbieter wird verwendet, um Funktionen der Vollversion von Azure MFA nutzen. Es ist für Benutzer, die **keinen Lizenzen Azure MFA, Azure AD Premium oder EMS**.  Azure MFA, Azure AD Premium und EMS enthalten die Vollversion von Azure MFA standardmäßig.  Wenn Sie Lizenzen haben, brauchen Sie keine Azure Mehrfaktor-Authentifizierungsanbieter.

Ein Azure Mehrfaktor-Authentifizierungsanbieter muss das SDK herunterladen.

> [AZURE.IMPORTANT]  Downloaden des SDK erstellen Sie ein Azure Mehrfaktor-Authentifizierungsanbieter, wenn Sie Azure MFA AAD Premium oder EMS-Lizenzen.  Sollten Sie ein Azure Mehrfaktor-Authentifizierungsanbieter für diesen Zweck erstellen und bereits Lizenzen den Anbieter mit dem Modell **Pro aktivierten Benutzer** erstellen. Verknüpfen Sie dann den Anbieter in das Verzeichnis von Azure MFA, Azure AD Premium oder EMS-Lizenzen.  Dadurch haben Sie mehr eindeutige Benutzer mithilfe des SDK als die Anzahl der Lizenzen, die Sie besitzen, nur abgerechnet werden.


## <a name="to-create-a-multi-factor-auth-provider"></a>Eine mehrstufige Authentifizierung Anbieter erstellen

Gehen Sie zu einer Azure Mehrfaktor-Authentifizierungsanbieter.

1. Melden Sie sich als Administrator in [Azure-Verwaltungsportal](https://manage.windowsazure.com) an.
2. Wählen Sie auf der linken Seite **Active Directory**.
3. Wählen Sie auf der Seite Active Directory oben **Mehrfaktor-Authentifizierungsanbieter**.
![Erstellen eines Anbieters MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider1.png)
4. Klicken Sie unten auf **neu**.
![Erstellen eines Anbieters MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider2.png)
5. Wählen Sie unter App Services **Mehrstufige Authentifizierungsanbieter**
![Erstellen eines MFA-Anbieters](./media/multi-factor-authentication-get-started-auth-provider/authprovider3.png)
6. Wählen Sie **Quick erstellen**.
![Erstellen eines Anbieters MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider4.png)
5. Die folgenden Felder und **Erstellen**.
    1. **Name** – der Name des Anbieters mehrstufige Authentifizierung.
    2. **Modell** – ob einzelne Benutzer oder pro Überprüfung werden soll. Wählen Sie eine der beiden Optionen aus:
        - Pro Authentifizierung – Einkauf Modell pro Authentifizierung Gebühren. In der Regel verwendet für Szenarien mit Azure mehrstufige Authentifizierung kundenorientierter Anwendung.
        - Pro Benutzer aktiviert aktiviert übliche Verfahren pro Gebühren Benutzer. Normalerweise verwendet für Mitarbeiterzugriff auf Programme wie Office 365. Wählen Sie diese Option, wenn einige Benutzer, die bereits für Azure MFA lizenziert sind.
    2. **Verzeichnis** der Azure Active Directory-Tenant mehrstufige Authentifizierungsanbieter zugeordnet ist. Beachten Sie Folgendes:
        - Sie brauchen keine Azure AD-Verzeichnis eine mehrstufige Authentifizierung Anbieter erstellt. Lassen Sie das Feld leer, wenn Sie nur Azure mehrstufige Authentifizierungsserver oder SDK verwenden.
        - Mehrstufige Authentifizierungsanbieter muss ein Azure AD-Verzeichnis zu den erweiterten Funktionen zugeordnet werden.
        - Azure AD Connect AAD Sync oder DirSync werden nur wenn lokale Active Directory-Umgebung mit einer Azure AD-Verzeichnis synchronisiert werden.  Wenn Sie nur ein Azure AD-Verzeichnis, die nicht synchronisiert ist, ist dies nicht erforderlich.
![Erstellen eines Anbieters MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider5.png)
5. Klick erstellen, mehrstufige Authentifizierungsanbieter erstellt und sollte eine Meldung angezeigt: **Mehrstufige Authentifizierungsanbieter wurde erfolgreich erstellt**. Klicken Sie auf **Ok**.
![Erstellen eines Anbieters MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider6.png)
