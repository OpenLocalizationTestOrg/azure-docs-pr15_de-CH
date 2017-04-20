<properties
   pageTitle="Verwalten von Azure Analysis Services | Microsoft Azure"
   description="Informationen Sie zum Verwalten eines Analysis Services-Servers in Azure."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="manage-analysis-services"></a>Verwalten von Analysis Services

Nach dem Erstellen eines Analysis Services-Servers in Azure möglicherweise einige Verwaltungsaufgaben sofort oder irgendwann in der Zukunft ausführen müssen. Verarbeitung auf die Aktualisierungsdaten steuern, wer die Modelle auf dem Server zugreifen oder überwachen die Fehlerfreiheit des Servers kann beispielsweise ausführen. Einige Verwaltungsaufgaben können nur in Azure-Portal andere in SQL Server Management Studio (SSMS) ausgeführt werden und einige Aufgaben können entweder.

## <a name="azure-portal"></a>Azure-portal
[Azure-Portal](http://portal.azure.com/) ist, Sie können erstellen und Löschen von Servern Überwachen von Serverressourcen, Größe ändern und verwalten, die Zugriff auf Ihre Server.  Wenn Sie Probleme haben, können Sie auch eine Support-Anfrage senden.

![Servername in Azure abrufen](./media/analysis-services-manage/aas-manage-portal.png)

## <a name="sql-server-management-studio"></a>SQL Server Management Studio
Herstellen einer Verbindung mit Ihrem Server in Azure ist wie eine Serverinstanz in Ihrem Unternehmen mit. SSMS können Sie viele Aufgaben wie Daten durchzuführen oder Verarbeitung Erstellungsskript, Verwalten von Rollen und PowerShell verwenden.

![SQL Server Management Studio](./media/analysis-services-manage/aas-manage-ssms.png)

 Eine größere Unterschiede ist die Authentifizierung für die Verbindung zum Server verwenden. Um Ihre Azure Analysis Services-Server herstellen, müssen Sie **Active Directory-Kennwort-Authentifizierung**wählen.

### <a name="to-connect-with-ssms"></a>Verbindung mit SSMS
1. Bevor Sie eine Verbindung herstellen, müssen Sie den Servernamen erhalten. In **Azure-Portal** > Server > **Übersicht** > **Servername**Server kopieren.

    ![Servername in Azure abrufen](./media/analysis-services-deploy/aas-deploy-get-server-name.png)

2. In SSMS > **Objekt-Explorer**, klicken Sie auf **Verbinden** > **Analysis Services**.

3. Fügen Sie den Servernamen und anschließend in **Authentifizierung**Dialogfeld **mit Server verbinden** , wählen Sie eine der folgenden:

    **Active Directory-integrierte Authentifizierung** einmaliges Anmelden mit Active Directory Azure Active Directory-Verbunddienste verwenden.

    **Active Directory-Kennwort-Authentifizierung** organisatorische Konto verwenden. Z. B. beim Herstellen einer Verbindung von einer nicht-Domäne Computer hinzugefügt.

    Hinweis: Wenn Sie Active Directory-Authentifizierung nicht sehen, Sie in SSMS [Azure Active Directory-Authentifizierung aktivieren](#enable-azure-active-directory-authentication) müssen.

    ![In SSMS verbinden](./media/analysis-services-manage/aas-manage-connect-ssms.png)

Verwaltung Ihres Servers in Azure mit SSMS ähnlich wie Verwaltung der lokalen Server ist, gehen wir nicht ins Detail gehen. Die Hilfe Sie müssen [Analysis Services-Instanz-Management](https://msdn.microsoft.com/library/hh230806.aspx) auf MSDN finden.

## <a name="server-administrators"></a>Server-Administratoren
**Analysis Services-Administratoren** können in der Blade-Steuerelement für den Server in Azure-Portal oder SSMS Administratoren verwalten. Analysis Services-Administratoren sind Server Datenbankadministratoren mit allgemeinen Datenbank-Verwaltungsaufgaben wie hinzufügen und Entfernen von Datenbanken und Verwalten von Benutzern. Standardmäßig ist der Benutzer, der den Server in Azure-Portal erstellt automatisch als Administrator Analysis Services hinzugefügt

Außerdem sollten Sie wissen:

-   Windows Live ID ist nicht unterstützter Identitätstyp für Azure Analysis Services.  
-   Analysis Services-Administratoren müssen gültige Azure Active Directory-Benutzer sein.
-   Wenn einen Azure Analysis Services-Server über Azure Resource Manager Vorlagen erstellen, nimmt Analysis Services-Administratoren ein JSON-Array der Benutzer als Administratoren hinzugefügt werden soll.

Analysis Services-Administratoren können von Azure Ressourcenadministratoren Ressourcen für Azure-Abonnements verwalten können. Dies erhält die Kompatibilität mit vorhandenen XMLA und TSML Verhalten in Analysis Services verwalten und Sie zwischen Azure Ressourcenmanagement und Analyse trennen können Datenbank-Management.

Um alle Rollen anzeigen und Typen für die Ressource ein Azure Analysis Services zugreifen, verwenden Sie (IAM) auf das Steuerelement.

## <a name="database-users"></a>Benutzer
Azure Benutzern des Analysis Services-Datenbank muss in Azure Active Directory. Benutzernamen für die Modelldatenbank angegebene müssen Organisation e-Mail-Adresse oder UPN sein. Dies unterscheidet sich von lokalen modelldatenbanken die Windows-Domäne Benutzernamen Benutzer unterstützen.

Sie können Benutzer mit [Rolle Aufgaben in Azure Active Directory](../active-directory/role-based-access-control-configure.md) oder [Tabular Model Scripting Language](https://msdn.microsoft.com/library/mt614797.aspx) (TMSL) in SQL Server Management Studio hinzufügen.

**Beispielskript für TMSL**

```
{
  "createOrReplace": {
    "object": {
      "database": "SalesBI",
      "role": "Users"
    },
    "role": {
      "name": "Users",
      "description": "All allowed users to query the model",
      "modelPermission": "read",
      "members": [
        {
          "memberName": "user1@contoso.com",
          "identityProvider": "AzureAD"
        },
        {
          "memberName": "group1@contoso.com",
          "identityProvider": "AzureAD"
        }
      ]
    }
  }
}
```

## <a name="enable-azure-active-directory-authentication"></a>Azure Active Directory-Authentifizierung aktivieren
Erstellen Sie eine Textdatei namens EnableAAD.reg, um die Azure Active Directory-Authentifizierung für SSMS in der Registrierung aktiviert, kopieren Sie und fügen Sie den folgenden:


```
Windows Registry Editor Version 5.00
[HKEY_CURRENT_USER\Software\Microsoft\Microsoft SQL Server\Microsoft Analysis Services\Settings]
"AS AAD Enabled"="True"
```

Speichern Sie und führen Sie die Datei.



## <a name="next-steps"></a>Nächste Schritte
Wenn ein Tabellenmodell bereits auf den neuen Server bereitgestellt haben, ist jetzt ein guter Zeitpunkt. Weitere Informationen finden Sie unter [Bereitstellen Azure Analysis Services](analysis-services-deploy.md).

Wenn Sie ein Modell zum Server bereitgestellt haben, können Sie mit einem Client oder Browser herstellen. Informationen finden Sie unter [Daten in Azure Analysis Services-Server](analysis-services-connect.md).
