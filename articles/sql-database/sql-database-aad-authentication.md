<properties
   pageTitle="Verbindung mit SQL-Datenbank oder SQL Datawarehouse Azure Active Directory Authentifizierung | Microsoft Azure"
   description="Informationen Sie zum SQL-Datenbank herstellen, von Azure Active Directory-Authentifizierung."
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/16/2016"
   ms.author="rick.byham@microsoft.com"/>

# <a name="connecting-to-sql-database-or-sql-data-warehouse-by-using-azure-active-directory-authentication"></a>Herstellen einer Verbindung mit SQL-Datenbank oder SQL Datawarehouse Azure Active Directory-Authentifizierung

Azure Active Directory-Authentifizierung ist ein mit Microsoft Azure SQL-Datenbank und [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) mit Identitäten in Azure Active Directory (Azure AD). Azure Active Directory-Authentifizierung können Sie zentral die Identitäten der Benutzer und anderen Microsoft-Diensten an einem zentralen Ort verwalten. Zentrale ID-Verwaltung bietet eine zentrale Stelle zum Benutzer verwalten und Berechtigungsmanagement vereinfacht. Bietet die folgenden Vorteile:

- Es stellt eine Alternative zur SQL Server-Authentifizierung.
- Stoppt die Verbreitung von Benutzeridentitäten auf Datenbankserver.
- Ermöglicht Kennwortrotation an einem einzigen Ort
- Kunden können Datenbankberechtigungen externe (AAD) Gruppen verwalten.
- Speichern von Kennwörtern eliminieren sie integrierte Windows-Authentifizierung und andere Formen der Authentifizierung von Azure Active Directory unterstützt.
- Azure verwendet Active Directory enthalten Benutzer zur Authentifizierung von Identitäten auf Datenbankebene.
- Azure Active Directory unterstützt Token-basierter Authentifizierung für Applikationen mit SQL-Datenbank.
- Azure Active Directory-Authentifizierung unterstützt ADFS (Domäne Verbund) oder native Benutzerkennwort-Authentifizierung für eine lokale Azure Active Directory ohne Domänen synchronisieren.  
- Azure Active Directory unterstützt Verbindungen von SQL Server Management Studio Active Directory Universal-Authentifizierung die mehrstufige Authentifizierung (MFA) enthält.  MFA enthält strenge Authentifizierung mit einfachen Überprüfungsoptionen, Telefonanruf, SMS, Smartcards mit Pin oder mobile-app-Benachrichtigung. Weitere Informationen finden Sie unter [SSMS Unterstützung für Azure AD MFA SQL-Datenbank mit SQL Data Warehouse](sql-database-ssms-mfa-authentication.md).

Die Konfigurationsschritte enthalten die folgenden Verfahren zum Konfigurieren und Verwenden von Azure Active Directory-Authentifizierung.

1. Erstellen und Füllen einer Azure Active Directory.
2. Sicherstellen Sie, dass Ihre Datenbank in Azure SQL-Datenbank V12. (Nicht erforderlich für SQL Data Warehouse).
3. Optional: Ordnen oder Ändern von active Directory mit der Azure-Abonnement verknüpft ist.
4. Erstellen Sie Administrator Azure Active Directory Azure SQL Server oder [Azure SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).
5. Konfigurieren Sie die Client-Computern.
6. Erstellen Sie enthaltene Benutzer in der Datenbank zugeordnet Azure AD Identitäten.
7. Verbinden Sie mit Ihrer Datenbank mithilfe von Azure AD Identitäten.


## <a name="trust-architecture"></a>Vertrauen Architektur

Folgende allgemeine Diagramm fasst die Lösungsarchitektur Azure SQL-Datenbank mit Azure AD-Authentifizierung. Dieselben Konzepte gelten für SQL Data Warehouse. Unterstützung für native Benutzerkennwort Azure Active Directory nur Cloud Teil und Azure AD-Azure SQL-Datenbank gilt. Verbundene Authentifizierung oder Benutzerkennwort für Windows-Anmeldeinformationen, Unterstützung die Kommunikation mit ADFS ist erforderlich. Die Pfeile zeigen Kommunikationswege.

![AAD Auth-Diagramm][1]

Das folgende Diagramm gibt die Föderation vertrauen und hosting Relationships, mit denen eine Verbindung zu einer Datenbank mit einem Token. Das Token von Azure AD authentifiziert, und die Datenbank vertrauenswürdig ist. Debitor 1 kann Azure Active Directory mit systemeigenen Benutzern oder Azure Active Directory mit Föderationsbenutzern darstellen. Debitor 2 repräsentiert eine mögliche Lösung einschließlich der importierten Benutzer; In diesem Beispiel aus einer verbundenen Azure Active Directory ADFS Azure Active Directory synchronisiert. Es ist wichtig zu verstehen, dass Zugriff auf eine Datenbank mit Azure AD-Authentifizierung erfordert, dass das Hosting-Abonnement in Azure Active Directory zugeordnet ist. Dieselbe Abonnement muss zum SQL Server hosten Azure SQL-Datenbank oder SQL Data Warehouse erstellen.

![Abonnement-Beziehung][2]

## <a name="administrator-structure"></a>Administrator-Struktur

Bei Azure AD-Authentifizierung sind zwei Administratorkonten für die SQL-Datenbank. der ursprünglichen SQL Server-Administrator und Azure AD Administrator. Dieselben Konzepte gelten für SQL Data Warehouse. Basierend auf Azure Konto Administrator kann ersten Azure AD enthaltenen Datenbankbenutzer in einer Datenbank erstellen. Azure AD-Administratorkonto kann ein Azure AD-Benutzer oder eine Gruppe Azure AD. Wenn der Administrator ein Gruppenkonto ist, kann von jedem Gruppenmitglied mehrere Azure AD Administratoren für die SQL Server-Instanz verwendet werden. Gruppe als Administrator mit verbessert die Verwaltung zu zentral hinzufügen und Entfernen von Gruppenmitgliedern in Azure AD ohne Benutzer oder Berechtigungen in der SQL-Datenbank. Nur ein Azure AD Administrator (Benutzer oder Gruppe) kann jederzeit konfiguriert werden.

![Admin-Struktur][3]

## <a name="permissions"></a>Berechtigungen

Um neue Benutzer zu erstellen, müssen Sie die `ALTER ANY USER` Berechtigungen in der Datenbank. Die `ALTER ANY USER` Berechtigung können alle Datenbankbenutzer. Die `ALTER ANY USER` Berechtigung findet auch durch den Server Administrator und Benutzer mit der `CONTROL ON DATABASE` oder `ALTER ON DATABASE` Berechtigung für die Datenbank, und die `db_owner` -Datenbankrolle.

Um einen eigenständigen Datenbankbenutzer in Azure SQL-Datenbank oder SQL Data Warehouse zu erstellen, müssen Sie die Datenbank mit einer Azure AD-Identität verbinden. Um die erste eigenständige Datenbank erstellen, müssen Sie Verbinden mit der Datenbank mit einer Azure Active Directory-Administrator (der Besitzer der Datenbank). Dies wird in den Schritten 4 und 5 unten veranschaulicht. Jede Azure Active Directory-Authentifizierung ist nur möglich, wenn Admin Azure Active Directory Azure SQL-Datenbank oder SQL Data Warehouse-Server erstellt wurde. Wenn Admin Azure Active Directory vom Server entfernt wurde, können angelegte Azure Active Directory Benutzer zuvor in SQL Server nicht mehr auf die Datenbank mit ihren Azure Active Directory-Anmeldeinformationen.

## <a name="azure-ad-features-and-limitations"></a>Azure AD-Funktionen und Grenzen

Die folgenden Member von Azure Active Directory können in Azure SQL Server oder SQL Data Warehouse bereitgestellt werden.

- Systemeigene Elemente: ein Element in der verwalteten Domäne oder in einer Kundendomäne in Azure AD erstellt. Weitere Informationen finden Sie unter [Ihren eigenen Domänennamen Azure AD hinzufügen](../active-directory/active-directory-add-domain.md).

- Verbundene Domänenmitglieder: Mitglied in Azure AD mit einer föderierten Domäne erstellt. Weitere Informationen finden Sie in der [Microsoft Azure jetzt mit Windows Server Active Directory unterstützt](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/).

- Importierte Elemente aus anderen Azure Active Directory, die pur oder föderierten Domäne gehören.

- Active Directory-Gruppen als Sicherheitsgruppen erstellt.

Microsoft (z. B. outlook.com hotmail.com, live.com) oder andere Gastkonten (z. B. gmail.com, yahoo.com) werden nicht unterstützt. Wenn Sie sich mit dem Konto und Kennwort [https://login.live.com](https://login.live.com) anmelden können, sind Sie ein Microsoft-Konto verwenden, Azure AD-Authentifizierung für SQL Azure oder Azure SQL Data Warehouse nicht unterstützt wird.

### <a name="additional-considerations"></a>Weitere Aspekte

- Zur Verbesserung der Verwaltung wird empfohlen, eine dedizierte Azure Active Directory-Gruppe als Administrator bereitstellen.
- Nur ein Azure AD Administrator (Benutzer oder Gruppe) kann jederzeit Azure SQL Server oder Azure SQL Data Warehouse konfiguriert werden.
- Nur ein Administrator Active Directory Azure für SQL Server kann zunächst Azure SQL Server oder Azure SQL Data Warehouse mit Azure Active Directory-Konto verbinden. Active Directory-Administrator kann nachfolgende Benutzer Azure Active Directory konfigurieren.
- Wir empfehlen das Verbindungstimeout auf 30 Sekunden.
- SQL Server 2016 Management Studio und SQL Server-Tools für Visual Studio 2015 (Version 14.0.60311.1April 2016 oder höher) unterstützt Azure Active Directory-Authentifizierung. (Azure Active Directory-Authentifizierung wird vom **.NET Framework-Datenanbieter für SQL Server**, am wenigsten Version.NET Framework 4.6). Daher können die neuesten Versionen dieser Tools und Applikationen Datenebene (DAC und .bacpac) Azure Active Directory-Authentifizierung.
- [ODBC-Version 13.1](https://www.microsoft.com/download/details.aspx?id=53339) unterstützt Azure Active Directory-Authentifizierung `bcp.exe` keine Verbindung mit Azure Active Directory-Authentifizierung, da sie einen älteren ODBC-Provider verwendet.
- `sqlcmd`unterstützt Active Directory Azure Authentifizierung seit Version 13.1 im [Download Center](http://go.microsoft.com/fwlink/?LinkID=825643)verfügbar.  
- SQL Server Data Tools für Visual Studio 2015 erfordert mindestens die Version April 2016 Datentools (Version 14.0.60311.1). Azure Active Directory-Benutzer sind derzeit nicht im SSDT Objekt-Explorer angezeigt. Um dieses Problem zu umgehen wird zeigen Sie der Benutzer in [Kataloganzeige an](https://msdn.microsoft.com/library/ms187328.aspx).
- [Microsoft JDBC Treiber 6.0 für SQL Server](https://www.microsoft.com/en-us/download/details.aspx?id=11774) unterstützt Azure Active Directory-Authentifizierung. Siehe auch [die Verbindungseigenschaften festlegen](https://msdn.microsoft.com/library/ms378988.aspx).
- PolyBase kann nicht mithilfe von Azure Active Directory-Authentifizierung authentifizieren.
- Einige Tools wie BI und Excel nicht unterstützt werden.
- Azure Active Directory-Authentifizierung für Datenbank SQL Azure Portal **Datenbank importieren** und **Exportieren von** Blades unterstützt. Import und Export mit Azure Active Directory-Authentifizierung wird unterstützt von PowerShell-Befehl.


## <a name="1-create-and-populate-an-azure-ad"></a>1. erstellen Sie und füllen Sie einer Azure AD auf

Erstellen Sie Azure Active Directory und mit Benutzern und Gruppen. Azure Active Directory kann die erste Azure AD verwalteten Domäne. Azure Active Directory kann auch eine lokale Active Directory Domain Services, die Föderation besteht in Azure Active Directory.

Weitere Informationen finden Sie in [Ihrer lokalen Identitäten Azure Active Directory integrieren](../active-directory/active-directory-aadconnect.md), [Fügen Sie Ihren eigenen Domänennamen Azure AD](../active-directory/active-directory-add-domain.md) [unterstützt jetzt Microsoft Azure mit Windows Server Active Directory](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Verwalten von Azure AD-Verzeichnis](https://msdn.microsoft.com/library/azure/hh967611.aspx)und [Verwalten Azure AD mit Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx).

## <a name="2-ensure-your-sql-database-is-version-12"></a>2. sicherstellen Sie, dass die SQL-Datenbank ist Version 12

Azure Active Directory-Authentifizierung ist in den neuesten SQL Datenbank V12 unterstützt. Informationen zu SQL-Datenbank V12 und um zu erfahren, ob in Ihrer Region verfügbar ist finden Sie unter [Neuheiten neueste SQL Datenbank Update V12](sql-database-v12-whats-new.md). Dieser Schritt ist nicht erforderlich für Azure SQL Data Warehouse, da SQL Data Warehouse nur in V12 verfügbar ist.

Wenn Sie eine vorhandene Datenbank verfügen, zu überprüfen, ob es in SQL Datenbank V12 gehostet wird, Herstellen einer Verbindung mit der Datenbank (z. B. SQL Server Management Studio) und `SELECT @@VERSION;`. Die erwartete Ausgabe für eine Datenbank in SQL Datenbank V12 ist mindestens **Microsoft SQL Azure (RTM) - 12.0**. Wenn Ihre Datenbank nicht in SQL-Datenbank V12 gehostet wird, finden Sie unter [Planen und SQL Datenbank V12 aktualisieren](sql-database-v12-plan-prepare-upgrade.md), und besuchen Sie Azure-Verwaltungsportal um die Datenbank auf SQL Datenbank V12 migrieren.

Alternativ können Sie eine neue Datenbank in SQL Datenbank V12 erstellen die Schritte unter [erstellen Ihre erste Azure SQL-Datenbank](sql-database-get-started.md). **Tipp**: Nächstes lesen, bevor Sie ein Abonnement für die neue Datenbank auswählen.

## <a name="3-optional-associate-or-change-the-active-directory-that-is-currently-associated-with-your-azure-subscription"></a>3. Optional: Zuordnen oder Ändern von active Directory mit der Azure-Abonnement verknüpft ist

Um Azure AD-Verzeichnis für die Organisation die Datenbank zuzuordnen, macht das Verzeichnis vertrauenswürdigen Verzeichnis für die Datenbank Azure-Abonnement. Weitere Informationen finden Sie unter [wie Azure-Abonnements Azure AD zugeordnet sind](https://msdn.microsoft.com/library/azure/dn629581.aspx).

**Weitere Informationen:** Jede Azure-Abonnement besitzt eine Vertrauensstellung mit einer Azure AD-Instanz. Dies bedeutet, dass dieses Verzeichnis zum Authentifizieren von Benutzern, Dienste und Geräte vertraut. Mehrere Abonnements können dasselbe Verzeichnis vertrauen, aber ein Abonnement vertraut nur ein Verzeichnis. Sie können sehen, welches Verzeichnis Ihr Abonnement unter der Registerkarte **Einstellungen** [https://manage.windowsazure.com/](https://manage.windowsazure.com/)vertraut. Diese Vertrauensstellung ein Abonnement mit einem Verzeichnis ist unterscheidet sich die Beziehung, ein Abonnement mit allen Ressourcen in Azure (Websites, Datenbanken usw.) die untergeordneten Ressourcen eines Abonnements sind. Wenn ein Abonnement abläuft, reagiert auf die mit dem Abonnement zugeordneten Ressourcen auch. Das Verzeichnis in Azure bleibt und dieses Verzeichnis ein weiteres Abonnement zugeordnet und Directory Benutzer verwalten. Weitere Informationen zu Ressourcen finden Sie unter [Understanding Ressourcenzugriff in Azure](https://msdn.microsoft.com/library/azure/dn584083.aspx).

Die folgenden Prozeduren bieten Anleitung zum zugehörige Verzeichnis für das angegebene Abonnement ändern.

1. Verbinden Sie mit der [Azure-Verwaltungsportal](https://manage.windowsazure.com/) mit Administrator Azure-Abonnement.
2. **Wählen Sie im linken Banner.**
3. Abonnements werden im Bildschirm angezeigt. Wenn das gewünschte Abonnement nicht angezeigt wird, klicken Sie auf **Abonnements** Dropdownfeld **FILTER vom Verzeichnis** und wählen Sie das Verzeichnis mit Abonnements und klicken Sie auf **Übernehmen**.

    ![Abonnement auswählen][4]
4. Im Bereich **Einstellungen** auf Ihr Abonnement und klicken Sie dann auf **Verzeichnis bearbeiten** am unteren Rand der Seite.

    ![AD-Einstellungen-portal][5]
5. Wählen Sie im **Verzeichnis bearbeiten** Azure Active Directory, die mit SQL Server oder SQL Data Warehouse verknüpft sind, und klicken Sie auf den Pfeil weiter.

    ![Verzeichnis bearbeiten auswählen][6]
6. Im Dialogfeld Verzeichnis Zuordnung **bestätigen** bestätigen "**Alle Co-Administratoren entfernt.**"

    ![Bearbeiten-Verzeichnis bestätigen][7]
7. Klicken Sie auf das Kontrollkästchen, wenn das Portal neu laden.

> [AZURE.NOTE] Beim Ändern des Verzeichnisses, auf alle CO-Administratoren, Azure AD-Benutzern und Gruppen Verzeichnis gesichert Ressource Benutzer entfernt und haben mehr Zugriff auf dieses Abonnement oder seine Ressourcen. Nur können Sie als Administrator Service Access für Prinzipale basierend auf das neue Verzeichnis konfigurieren. Diese Änderung kann eine beträchtliche Zeit alle Ressourcen verbreiten dauern. Wechseln des Verzeichnisses ändert Azure AD Administrator für SQL-Datenbank und SQL Data Warehouse und Datenbankzugriff für vorhandene Azure AD Benutzer verweigern. Azure AD Admin muß zurücksetzen (wie unten beschrieben) und neue Azure AD-Benutzer erstellt werden müssen.

## <a name="4-create-an-azure-ad-administrator-for-azure-sql-server"></a>4. Erstellen von Azure SQL Server Administrator Azure AD

Jede Azure SQL Server (die eine SQL-Datenbank oder SQL Data Warehouse hostet) startet mit einem einzelnen Server-Administratorkonto, die der Administrator die gesamte Azure SQL Server. Ein zweiter SQL Server-Administrator muss erstellt werden, die Azure Konto. Dieses Prinzip wird als eigenständige Datenbankbenutzer in der master-Datenbank erstellt. Als Administratoren die Server Administrator-Konten sind Mitglieder der **Db_owner** -Rolle in jeder Datenbank, und geben jede Benutzerdatenbank als **Dbo** -Benutzer. Weitere Informationen über die Server Administrator-Konten finden Sie unter [Verwalten von Datenbanken und Anmeldedaten in Azure SQL-Datenbank](sql-database-manage-logins.md) und den **Benutzernamen und Benutzer** [Azure SQL Datenbank Richtlinien und Einschränkungen](sql-database-security-guidelines.md).

Bei Azure Active Directory mit Geo-Replikation muss der Azure Active Directory-Administrator für die primären und sekundären Server konfiguriert werden. Ein Server Administrator Azure Active Directory keinen erhalten Azure Active Directory-Benutzernamen und Benutzern eine "keine Serverfehler Verbindung".

> [AZURE.NOTE] Benutzer, die nicht auf ein Azure AD-Konto (einschließlich Azure SQL Server-Administratorkonto), Azure AD-Benutzer, kann nicht erstellt werden, da sie keine Berechtigung vorgeschlagenen Datenbankbenutzer Azure AD überprüft haben.

### <a name="provision-an-azure-active-directory-administrator-for-your-azure-sql-server-by-using-the-azure-portal"></a>Bereitstellen Sie ein Azure Active Directory-Administrator für Ihre Azure SQL Server mithilfe des Azure-Portals

1. Klicken Sie im [Azure-Portal](https://portal.azure.com/), in der oberen rechten Ecke auf der Verbindung eine Liste der möglichen Active Directory. Wählen Sie die richtigen Active Directory standardmäßig Azure AD. Dieser Schritt links abonnementzuordnung in Active Directory mit Azure SQL Server sicherstellen, dass für beide das gleiche Abonnement verwendet wird Azure AD und SQL Server. (Azure SQL Server können Azure SQL-Datenbank oder Azure SQL Data Warehouse verwalten.)

    ![Wählen Sie Anzeige][8]
2. In Linker Banner **SQL Server**auswählen, wählen Sie den **SQL Server**und **in der **SQL Server-** Blade-oben klicken**.

    ![AD-Optionen][9]
3. Blatt **Einstellungen** klicken ** Admin Active Directory.
4. **Active Directory Admin** Blatt klicken Sie auf **Active Directory-Administrator**und klicken Sie dann oben auf **Admin**.
5. Blatt **Hinzufügen Admin** Suche nach einem Benutzer wählen Sie Administratorrechte Benutzer oder Gruppe aus und dann auf **auswählen**. (Blade Admin Active Directory zeigt alle Elemente und Gruppen von Active Directory. Benutzer oder Gruppen, die deaktiviert werden können ausgewählt werden, da sie nicht als Administratoren Azure AD unterstützt werden. (Siehe die Liste unterstützter Administratoren in **Azure AD-Funktionen und Grenzen** ). Rollenbasierte Zugriffskontrolle (RBAC) gilt nur für das Portal und nicht an SQL Server weitergegeben.
6. Am Anfang des **Active Directory-Admin** -Blades klicken Sie auf **Speichern**.
    ![Wählen Sie admin][10]

    Ändern des Administrators kann mehrere Minuten dauern. Anschließend wird der neue Administrator im **Active Directory-Verwaltung** .

> [AZURE.NOTE] Beim Einrichten der Azure AD Administrator kann nicht der neuen Admin-Name (Benutzer oder Gruppe) als SQL Server-Authentifizierungsbenutzer bereits in der virtuellen Masterdatenbank vorhanden sein. Wenn vorhanden, wird Azure AD Administrator-Setup fehl. Rollback der Erstellung und angibt, dass dieser Administrator (Name) bereits vorhanden ist. Da ein SQL Server-Authentifizierungsbenutzer nicht von Azure AD gehört, jede Verbindung zum Server mithilfe der Azure AD-Authentifizierung nicht.

Oben Blade **Active Directory Admin** Administrator später entfernen auf **Admin entfernen**und klicken Sie dann auf **Speichern**.

### <a name="provision-an-azure-ad-administrator-for-azure-sql-server-by-using-powershell"></a>Bereitstellen Sie Administrator Azure AD Azure SQL Server mithilfe von PowerShell

Zum Ausführen des PowerShell-Cmdlets müssen Sie Azure PowerShell installiert und ausgeführt. Ausführliche Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

Administrator Azure AD bereitstellen, führen Sie die folgenden Azure PowerShell-Befehle:

- Hinzufügen AzureRmAccount
- AzureRmSubscription auswählen


Cmdlets verwendet zum Bereitstellen und Verwalten von Azure AD Administrator:

| Cmdlet-Namen                                       | Beschreibung                                                                                                     |
|---------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| [AzureRmSqlServerActiveDirectoryAdministrator festlegen](https://msdn.microsoft.com/library/azure/mt603544.aspx)    | Stellt ein Administrator Active Directory Azure Azure SQL Server oder Azure SQL Data Warehouse. (Das aktuelle Abonnement muss.) |
| [AzureRmSqlServerActiveDirectoryAdministrator entfernen](https://msdn.microsoft.com/library/azure/mt619340.aspx) | Entfernt ein Administrator Active Directory Azure Azure SQL Server oder Azure SQL Data Warehouse. |
| [AzureRmSqlServerActiveDirectoryAdministrator abrufen](https://msdn.microsoft.com/library/azure/mt603737.aspx)    | Gibt Informationen zu Administrator Azure Active Directory Zeit für Azure SQL Server oder Azure SQL Data Warehouse. |

Mit der PowerShell-Befehl Hilfe weitere Details dieser Befehle zum Beispiel finden Sie unter ``get-help Set-AzureRmSqlServerActiveDirectoryAdministrator``.

Folgendes Skript eine Azure AD Administrator-Gruppe mit dem Namen **DBA_Group** (Objekt-Id `40b79501-b343-44ed-9ce7-da4c8cc7353f`) **Demo_server** Server in eine Ressourcengruppe mit dem Namen **Gruppe 23**:

```
Set-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23"
–ServerName "demo_server" -DisplayName "DBA_Group"
```

Der **DisplayName** -Eingabeparameter akzeptiert den Anzeigenamen Azure AD oder Benutzerprinzipalnamen. Beispielsweise ``DisplayName="John Smith"`` und ``DisplayName="johns@contoso.com"``. Azure AD-Gruppen Azure AD werden Anzeigenamen.

> [AZURE.NOTE] Azure PowerShell-Befehl ```Set-AzureRmSqlServerActiveDirectoryAdministrator``` hindert Sie Azure AD-Administratoren nicht unterstützter Benutzer bereitstellen. Ein nicht unterstützter Benutzer bereitgestellt werden, aber kann keine Verbindung zu einer Datenbank herstellen. (Siehe die Liste unterstützter Administratoren in **Azure AD-Funktionen und Grenzen** ).

Im folgenden Beispiel wird der optionale **ObjectID**:

```
Set-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23"
–ServerName "demo_server" -DisplayName "DBA_Group" -ObjectId "40b79501-b343-44ed-9ce7-da4c8cc7353f"
```

> [AZURE.NOTE] Azure AD **ObjectID** ist erforderlich, wenn **DisplayName** nicht eindeutig ist. Zum Abrufen der Werte **ObjectID** und **DisplayName** Active Directory Abschnitt Azure-Verwaltungsportal und Anzeigen der Eigenschaften eines Benutzers oder einer Gruppe.

Das folgende Beispiel gibt Informationen zum aktuellen Azure AD Administrator Azure SQL Server:

```
Get-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23" –ServerName "demo_server" | Format-List
```

Im folgenden Beispiel wird einen Azure AD-Administrator:
```
Remove-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" –ServerName "demo_server"
```

Sie können auch ein Azure Active Directory-Administrator mithilfe der REST-APIs bereitstellen. Weitere Informationen finden Sie unter [Service Management REST-API-Referenz und Operationen für Azure SQL Datenbanken Azure SQL-Datenbanken](https://msdn.microsoft.com/library/azure/dn505719.aspx)

## <a name="5-configure-your-client-computers"></a>5. konfigurieren Sie 5. die Client-Computern

Auf allen Clientcomputern aus denen Anwendung und Benutzer Azure SQL Data Warehouse mithilfe von Azure AD Identitäten zu Azure SQL-Datenbank herstellen müssen die folgende Software installieren:

- .NET Framework 4.6 oder höher von [https://msdn.microsoft.com/library/5a4x27ek.aspx](https://msdn.microsoft.com/library/5a4x27ek.aspx).
- Azure Active Directory Authentifizierungsbibliothek für SQL Server (**ADALSQL. DLL**) steht in mehreren Sprachen (X86 und amd64) aus dem Downloadcenter in [Microsoft Active Directory Authentifizierungsbibliothek für Microsoft SQL Server](http://www.microsoft.com/download/details.aspx?id=48742).

### <a name="tools"></a>Tools

- Installieren von [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) oder [SQL Server Data Tools für Visual Studio 2015](https://msdn.microsoft.com/library/mt204009.aspx) erfüllt.NET Framework 4.6.
- SSMS installiert die X86 Version von **ADALSQL. DLL**.
- SSDT installiert die amd64-Version von **ADALSQL. DLL**.
- [Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs) -Downloads der neuesten Visual Studio.NET Framework 4.6 erfüllt, aber nicht erforderliche amd64-Version von **ADALSQL installiert. DLL**.

## <a name="6-create-contained-database-users-in-your-database-mapped-to-azure-ad-identities"></a>6. erstellen Sie enthaltenen Benutzer in der Datenbank zugeordnet Azure AD Identitäten

### <a name="about-contained-database-users"></a>Eigenständige Datenbank Benutzer

Azure Active Directory-Authentifizierung muss Benutzer als eigenständige Datenbankbenutzer erstellt werden. Ein eigenständigen Datenbankbenutzer basierend auf Azure AD-Identität ist ein Datenbankbenutzer, die keinen Benutzernamen in der master-Datenbank und die Identität in Azure AD-Verzeichnis, die der Datenbank zugeordnet ist. Azure AD-Identität kann einem Benutzerkonto oder einer Gruppe sein. Weitere Informationen zu enthaltenen Benutzer finden Sie unter [Enthaltenen Benutzer machen Ihre Datenbank tragbaren](https://msdn.microsoft.com/library/ff929188.aspx).

> [AZURE.NOTE] Benutzer (mit Ausnahme der Administratoren) können mithilfe von Portal erstellt werden. RBAC-Rollen werden nicht auf SQL Server, SQL-Datenbank oder SQL Data Warehouse übertragen. Azure RBAC-Rollen zum Verwalten von Azure Ressourcen und gelten nicht für Datenbankberechtigungen. Beispielsweise gewährt der Teilnehmerrolle für **SQL Server** keinen Zugriff für die Verbindung mit der SQL-Datenbank oder SQL Data Warehouse. Die Zugriffsrechte muss direkt in der Datenbank mithilfe von Transact-SQL-Anweisungen erhalten.

### <a name="connect-to-the-user-database-or-data-warehouse-by-using-sql-server-management-studio-or-sql-server-data-tools"></a>Herstellen einer Verbindung mit der Benutzer-Datenbank oder eines Data Warehouse mit SQL Server Management Studio oder SQL Server Data Tools

Azure AD Administrator bestätigen ist ordnungsgemäß eingerichtet, die **master** -Datenbank mit dem Azure AD-Administratorkonto herstellen.
Bereitstellen (außer der Serveradministrator, der die Datenbank besitzt), ein Azure AD-basierter enthaltenen Datenbankbenutzer Verbinden mit der Datenbank mit einer Azure AD-Identität, die auf die Datenbank zugreifen.

> [AZURE.IMPORTANT] Unterstützung für Azure Active Directory-Authentifizierung ist mit [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) und [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) in Visual Studio 2015. Veröffentlichung August 2016 von SSMS unterstützt auch Active Directory Universal Authentifizierung ermöglicht Administratoren, mehrstufige Authentifizierung mit einem Telefonanruf, SMS, Smartcards mit Pin oder mobile-app-Benachrichtigung.

#### <a name="connect-using-active-directory-integrated-authentication"></a>Eine Verbindung mit Active Directory-integrierte Authentifizierung

Verwenden Sie diese Methode, wenn Sie Windows mit Ihren Azure Active Directory-Anmeldeinformationen aus einer föderierten Domäne angemeldet sind.

1. Starten Sie Management Studio oder Data-Tools zu, und wählen Sie im Dialogfeld **Verbindung mit Server herstellen** (oder **Verbindung mit Datenbankmodul herstellen**) im **Feld** **Active Directory-integrierte Authentifizierung**. Kein Kennwort erforderlich ist oder eingegeben werden kann, da die vorhandenen Anmeldeinformationen für die Verbindung angezeigt.
    ![Wählen Sie Active Directory-integrierte Authentifizierung][11]

2. Klicken Sie auf **Optionen** , und geben Sie auf der Seite **Eigenschaften** im **Verbinden mit Datenbank** den Namen der Datenbank, die, der Sie herstellen möchten.
    ![Wählen Sie den Datenbanknamen][13]


#### <a name="connect-using-active-directory-password-authentication"></a>Herstellen Sie Active Directory-Kennwort-Authentifizierung

Verwenden Sie diese Methode, wenn mit einem Benutzerprinzipalnamen Azure AD mit Azure AD Domain verwaltet. Sie können auch für föderierte Konto ohne Zugriff auf die Domäne, z. B. Remote verwenden.

Verwenden Sie diese Methode, wenn Sie angemeldet sind, Windows unter Verwendung von Anmeldeinformationen einer Domäne, die nicht mit Azure verbunden ist oder basierend auf der ersten oder der Clientdomäne Azure AD Authentifizierung mit Azure AD.

1. Starten Sie Management Studio oder Data-Tools zu, und wählen Sie im Dialogfeld **Verbindung mit Server herstellen** (oder **Verbindung mit Datenbankmodul herstellen**) im **Feld** **Active Directory-Kennwort-Authentifizierung**.
2. Geben Sie im Feld **Benutzername** Ihren Benutzernamen Azure Active Directory im Format **username@domain.com**. Muss ein Konto von Azure Active Directory oder ein Konto in einer Domäne mit Active Directory Azure Föderation.
3. Geben Sie im Feld **Kennwort** das Benutzerkennwort Azure Active Directory-Konto oder Domänenkonto verbunden.
    ![Wählen Sie AD-Kennwortauthentifizierung][12]

4. Klicken Sie auf **Optionen** , und geben Sie auf der Seite **Eigenschaften** im **Verbinden mit Datenbank** den Namen der Datenbank, die, der Sie herstellen möchten. (Siehe die Grafik in der vorherigen Option.)


### <a name="create-an-azure-ad-contained-database-user-in-a-user-database"></a>Erstellen Sie einen Datenbankbenutzer Azure AD enthalten in einer Benutzerdatenbank

Erstellen (nicht der Serveradministrator, der die Datenbank besitzt) einen Azure AD-basierter enthaltenen Datenbankbenutzer verbinden Sie mit der Datenbank mit einer Identität Azure AD mit mindestens die **ALTER ANY USER** -Berechtigung. Verwenden Sie dann die folgende Transact-SQL-Syntax:

    CREATE USER <Azure_AD_principal_name>
    FROM EXTERNAL PROVIDER;


*Azure_AD_principal_name* kann der Benutzerprinzipalname Azure AD-Benutzerobjekt oder den Anzeigenamen für eine Azure AD-Gruppe.

**Beispiele:** Eigenständige Datenbank erstellen Benutzer für eine Azure Anzeige verbundene oder Domänenbenutzer verwaltet:

    CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER;
    CREATE USER [alice@fabrikam.onmicrosoft.com] FROM EXTERNAL PROVIDER;

Erstellen Sie einen eigenständigen Datenbankbenutzer Azure AD darstellt oder verbundene Domänengruppe bieten Sie den Anzeigenamen der Gruppe:

    CREATE USER [ICU Nurses] FROM EXTERNAL PROVIDER;

So erstellen Sie einen eigenständigen Datenbankbenutzer, eine Verbindung mit einer Azure AD-Token Anwendung darstellt:

    CREATE USER [appName] FROM EXTERNAL PROVIDER;

Weitere Informationen zum Erstellen von eigenständigen Datenbankbenutzer Active Directory Azure Identitäten finden Sie unter [CREATE USER (Transact-SQL)](http://msdn.microsoft.com/library/ms173463.aspx).


> [AZURE.NOTE] Azure Active Directory Administrator für Azure SQL Server entfernen wird verhindert, dass jeder Benutzer Azure AD-Authentifizierung mit dem Server verbinden. Ggf. unbrauchbar Azure AD-Benutzer können von einem SQL-Datenbankadministrator manuell gelöscht.

Beim Erstellen eines Datenbankbenutzers Benutzer empfängt die **CONNECT** -Berechtigung und kann diese Datenbank als Mitglied der **PUBLIC** -Rolle. Zunächst sind die einzigen Berechtigungen für den Benutzer der **PUBLIC** -Rolle Berechtigungen oder Berechtigungen auf die Windows-Gruppen, denen deren Mitglied Sie sind. Nach der Bereitstellung eines Azure AD-basierter enthaltenen Datenbankbenutzers können Sie Benutzer zusätzliche, genauso Berechtigungen als anderer Typ von Benutzer Berechtigungen erteilen. In der Regel Berechtigungen Sie Datenbankrollen und fügen Sie Benutzer zu Rollen hinzu. Weitere Informationen finden Sie unter [Grundlagen der Datenbank Engine Berechtigung](http://social.technet.microsoft.com/wiki/contents/articles/4433.database-engine-permission-basics.aspx). Weitere Informationen zu speziellen SQL Datenbankrollen finden Sie unter [Verwalten von Datenbanken und Anmeldedaten in Azure SQL-Datenbank](sql-database-manage-logins.md).
Föderierte Domänenbenutzer, der in einer Domäne verwalten importiert müssen die verwaltete Identität verwenden.

> [AZURE.NOTE] Azure AD-Benutzer werden in die Datenbank-Metadaten mit E (EXTERNAL_USER) und für Gruppen mit X (EXTERNAL_GROUPS) markiert. Weitere Informationen finden Sie unter [Kataloganzeige](https://msdn.microsoft.com/library/ms187328.aspx).


## <a name="7-connect-by-using-azure-ad-identities"></a>7 verbinden Sie mit Azure AD-Identitäten

Azure Active Directory-Authentifizierung unterstützt die folgenden Methoden für eine Datenbank mit Azure AD-Identität:

- Integrierte Windows-Authentifizierung
- Ein Azure AD principal Name und Kennwort verwenden
- Anwendung Authentifizierung token

### <a name="71-connecting-using-integrated-windows-authentication"></a>7.1. Herstellen einer Verbindung mithilfe der integrierten Authentifizierung (Windows)

Um die integrierte Windows-Authentifizierung verwenden, müssen Ihre Domäne Active Directory Azure Active Directory verbunden sein. Die Clientanwendung (oder Dienst) für die Datenbank muss auf einer Domäne unter Domänenanmeldeinformationen des Benutzers ausgeführt werden.

Zum Verbinden mit einer Datenbank, die integrierte Authentifizierung und Azure AD-Identität muss auf Active Directory-integrierte Authentifizierung-Schlüsselwort in der Verbindungszeichenfolge festgelegt werden. Im folgende C#-Codebeispiel wird ADO .NET verwendet.

    string ConnectionString =
    @"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Integrated; Initial Catalog=testdb;";
    SqlConnection conn = new SqlConnection(ConnectionString);
    conn.Open();

Beachten Sie, dass die Verbindungszeichenfolge Schlüsselwort ``Integrated Security=True`` wird für die Verbindung mit Azure SQL-Datenbank nicht unterstützt.
Beachten Sie, dass bei einer ODBC-Verbindung werden Leerzeichen entfernen und Authentifizierung 'ActiveDirectoryIntegrated'.

### <a name="72-connecting-with-an-azure-ad-principal-name-and-a-password"></a>7.2. Herstellen einer Verbindung mit einer Azure AD principal Name und Kennwort
Zum Verbinden mit einer Datenbank, die integrierte Authentifizierung und Azure AD-Identität muss das Schlüsselwort Authentifizierung auf Active Directory-Kennwort festgelegt werden. Die Verbindungszeichenfolge muss die Benutzer-ID/UID und Password-PWD Schlüsselwörter und Werte enthalten. Im folgende C#-Codebeispiel wird ADO .NET verwendet.

    string ConnectionString =
      @"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Password; Initial Catalog=testdb;  UID=bob@contoso.onmicrosoft.com; PWD=MyPassWord!";
    SqlConnection conn = new SqlConnection(ConnectionString);
    conn.Open();

Erfahren Sie mehr über Azure AD Authentifizierungsmethoden [Azure AD-Authentifizierung GitHub Demo](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/security/azure-active-directory-auth)mit der Demo-Codebeispiele zur Verfügung.


### <a name="73-connecting-with-an-azure-ad-token"></a>7.3 Herstellen einer Verbindung mit einer Azure AD
Diese Authentifizierungsmethode ermöglicht mittelstufigen Azure SQL-Datenbank oder Azure SQL Data Warehouse erhalten einen Token von Azure Active Directory (AAD) herstellen. Sie können anspruchsvolle einschließlich zertifikatbasierte Authentifizierung. Sie müssen vier grundlegende Schritte zum token Azure AD-Authentifizierung verwenden:

1. Registrieren Sie Ihrer Anwendung in Azure Active Directory und die Client-Id für den Code. 
2. Erstellen Sie einen Datenbankbenutzer, die die Anwendung darstellt. (In Schritt 6 ausgeführt.)
3. Erstellen Sie ein Zertifikat auf dem Client Computer der Anwendung.
4. Das Zertifikat wird als Schlüssel für die Anwendung hinzufügen.

Beispiel-Verbindungszeichenfolge:

```
string ConnectionString =@"Data Source=n9lxnyuzhv.database.windows.net; Initial Catalog=testdb;"
SqlConnection conn = new SqlConnection(ConnectionString);
connection.AccessToken = "Your JWT token"
conn.Open();
```

Weitere Informationen finden Sie im [Blog des SQL Server-Sicherheit](https://blogs.msdn.microsoft.com/sqlsecurity/2016/02/09/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/).

### <a name="connecting-with-sqlcmd"></a>Verbinden mit sqlcmd  
Die folgende Anweisung Verbinden mit Version 13.1 Sqlcmd, im [Download Center](http://go.microsoft.com/fwlink/?LinkID=825643)verfügbar.

```
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net  -G  
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net -U bob@contoso.com -P MyAADPassword -G -l 30
```

## <a name="see-also"></a>Siehe auch

[Verwalten von Datenbanken und Anmeldedaten in Azure SQL-Datenbank](sql-database-manage-logins.md)

[Enthaltene Benutzer](https://msdn.microsoft.com/library/ff929071.aspx)

[Erstellen Sie (Transact-SQL)](http://msdn.microsoft.com/library/ms173463.aspx)


<!--Image references-->

[1]: ./media/sql-database-aad-authentication/1aad-auth-diagram.png
[2]: ./media/sql-database-aad-authentication/2subscription-relationship.png
[3]: ./media/sql-database-aad-authentication/3admin-structure.png
[4]: ./media/sql-database-aad-authentication/4select-subscription.png
[5]: ./media/sql-database-aad-authentication/5ad-settings-portal.png
[6]: ./media/sql-database-aad-authentication/6edit-directory-select.png
[7]: ./media/sql-database-aad-authentication/7edit-directory-confirm.png
[8]: ./media/sql-database-aad-authentication/8choose-ad.png
[9]: ./media/sql-database-aad-authentication/9ad-settings.png
[10]: ./media/sql-database-aad-authentication/10choose-admin.png
[11]: ./media/sql-database-aad-authentication/11connect-using-int-auth.png
[12]: ./media/sql-database-aad-authentication/12connect-using-pw-auth.png
[13]: ./media/sql-database-aad-authentication/13connect-to-db.png

