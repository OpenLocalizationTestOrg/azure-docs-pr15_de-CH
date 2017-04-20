<properties 
    pageTitle="Erstellen einer Web-App mit Redis Cache | Microsoft Azure" 
    description="Erstellen Sie Web-App mit Redis Cache" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="hero-article" 
    ms.date="10/11/2016" 
    ms.author="sdanie"/>

# <a name="how-to-create-a-web-app-with-redis-cache"></a>Erstellen einer Web-App mit Redis Cache

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Dieses Lernprogramm zeigt das Erstellen und Bereitstellen einer ASP.NET Web-Anwendung auf eine Webanwendung in Azure App Service mit Visual Studio 2015. Beispiel-Anwendung zeigt eine Liste der Teams Statistiken aus einer Datenbank und andere Verwendungsmöglichkeiten von Azure Redis Cache zum Speichern und Abrufen von Daten aus dem Cache. Abschluss des Lernprogramms haben Sie eine laufenden Web app, die Lese- und Schreibvorgänge in eine Datenbank mit Azure Redis Cache optimiert und in Azure gehostet.

Sie erfahren:

-   Zum Erstellen einer ASP.NET MVC 5 Web-Anwendung in Visual Studio.
-   Zum Zugreifen auf Daten aus einer Datenbank mit Entity Framework.
-   Zum Datendurchsatz Verbesserung und Reduzierung der Datenbank zu speichern und Abrufen von Daten mit Azure Redis Cache.
-   Verwendung einer Redis sortiert auf die Top 5 Teams abrufen.
-   Wie die Azure Ressourcen für die Anwendung Ressourcen-Manager.
-   Wie die Anwendung mit Visual Studio Azure veröffentlichen.

## <a name="prerequisites"></a>Erforderliche Komponenten

Um das Lernprogramm abzuschließen, müssen Sie die folgenden erforderlichen Komponenten verfügen.

-   [Ein Azure-Konto](#azure-account)
-   [Visual Studio 2015 Azure SDK für .NET](#visual-studio-2015-with-the-azure-sdk-for-net)

### <a name="azure-account"></a>Ein Azure-Konto

Sie benötigen ein Azure-Konto für dieses Lernprogramm erforderlich. Sie können:

* [Öffnen Sie ein Azure-Konto kostenlos](/pricing/free-trial/?WT.mc_id=redis_cache_hero). Sie erhalten ein Guthaben, die versuchen, bezahlte Azure Services verwendet werden können. Auch nach die Abspann verbraucht werden, können Sie das Konto und kostenlose Azure Dienste und Features verwenden.
* [Abonnementvorteile Visual Studio aktivieren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). Ihr MSDN-Abonnement erhalten Sie Credits monatlich für bezahlte Azure Services verwendet werden können.

### <a name="visual-studio-2015-with-the-azure-sdk-for-net"></a>Visual Studio 2015 Azure SDK für .NET

Das Lernprogramm ist für Visual Studio 2015 [Azure SDK für .NET](../dotnet-sdk.md) 2.8.2 geschrieben. [Herunterladen der neuesten Azure SDK für Visual Studio hier 2015](http://go.microsoft.com/fwlink/?linkid=518003). Visual Studio ist mit dem SDK installiert wenn es noch nicht.

Haben Sie Visual Studio 2013 können Sie [aktuelle Azure SDK für Visual Studio 2013 herunterladen](http://go.microsoft.com/fwlink/?LinkID=324322). Einige Bildschirme können Abbildungen in diesem Lernprogramm anders aussehen.

>[AZURE.NOTE] Wie viele Abhängigkeiten SDK auf Ihrem Computer bereits konnte das SDK installieren lange mehrere Minuten bis eine halbe Stunde dauern.

## <a name="create-the-visual-studio-project"></a>Erstellen Sie das Visual Studio-Projekt

1. Öffnen Sie Visual Studio, und klicken Sie auf **Datei**, **neu**, **Projekt**.

2. Erweitern Sie den Knoten **Visual C#** in der Liste **Vorlagen** , wählen Sie **Cloud**und auf **ASP.NET Web Application**. Stellen Sie sicher, dass **.NET Framework 4.5.2** ausgewählt ist.  Geben Sie **ContosoTeamStats** in **das Textfeld** , und klicken Sie auf **OK**.
 
    ![Projekt erstellen][cache-create-project]

3. **MVC** als Projekttyp auswählen Deaktivieren Sie das Kontrollkästchen **Host in der Cloud** . Sie werden [Azure Ressourcen bereitstellen](#provision-the-azure-resources) und [Veröffentlichen Sie die Anwendung in Azure](#publish-the-application-to-azure) in den nachfolgenden Schritten des Lernprogramms. Ein Beispiel für die Bereitstellung einer App Service Web app Visual Studio **Host in der Cloud** aktiviert lassen finden Sie unter [Einstieg in Web-Apps in Azure App Service mit ASP.NET und Visual Studio](../app-service-web/web-sites-dotnet-get-started.md).

    ![Wählen Sie die Projektvorlage][cache-select-template]

4. Klicken Sie auf **OK** , um das Projekt zu erstellen.

## <a name="create-the-aspnet-mvc-application"></a>Die ASP.NET MVC-Anwendung erstellen

In diesem Abschnitt des Lernprogramms erstellen Sie die grundlegende Anwendung, die gelesen und Team-Statistiken aus einer Datenbank angezeigt.

-   [Das Modell hinzufügen](#add-the-model)
-   [Den Controller hinzufügen](#add-the-controller)
-   [Konfigurieren von Ansichten](#configure-the-views)

### <a name="add-the-model"></a>Das Modell hinzufügen

1. Maustaste **Modelle** im **Projektmappen-Explorer**und wählen Sie **Hinzufügen**, **Klasse**. 

    ![Modell hinzufügen][cache-model-add-class]

2. Geben Sie `Team` für die Klasse benennen, und klicken Sie auf **Hinzufügen**.

    ![Modellklasse hinzufügen][cache-model-add-class-dialog]

3. Ersetzen der `using` Aussagen am oberen Rand der `Team.cs` Datei folgende using-Anweisung.


        using System;
        using System.Collections.Generic;
        using System.Data.Entity;
        using System.Data.Entity.SqlServer;


4. Ersetzen Sie die Definition der `Team` mit den folgenden Codeausschnitt, der eine aktualisierte enthält `Team` Klasse Definition sowie einige andere Entity Framework Hilfsklassen. Weitere Informationen zu Code ersten Ansatz für das Entity Framework, das in diesem Lernprogramm finden Sie unter [Code zuerst in eine neue Datenbank](https://msdn.microsoft.com/data/jj193542).


        public class Team
        {
            public int ID { get; set; }
            public string Name { get; set; }
            public int Wins { get; set; }
            public int Losses { get; set; }
            public int Ties { get; set; }
        
            static public void PlayGames(IEnumerable<Team> teams)
            {
                // Simple random generation of statistics.
                Random r = new Random();
        
                foreach (var t in teams)
                {
                    t.Wins = r.Next(33);
                    t.Losses = r.Next(33);
                    t.Ties = r.Next(0, 5);
                }
            }
        }
        
        public class TeamContext : DbContext
        {
            public TeamContext()
                : base("TeamContext")
            {
            }
        
            public DbSet<Team> Teams { get; set; }
        }
        
        public class TeamInitializer : CreateDatabaseIfNotExists<TeamContext>
        {
            protected override void Seed(TeamContext context)
            {
                var teams = new List<Team>
                {
                    new Team{Name="Adventure Works Cycles"},
                    new Team{Name="Alpine Ski House"},
                    new Team{Name="Blue Yonder Airlines"},
                    new Team{Name="Coho Vineyard"},
                    new Team{Name="Contoso, Ltd."},
                    new Team{Name="Fabrikam, Inc."},
                    new Team{Name="Lucerne Publishing"},
                    new Team{Name="Northwind Traders"},
                    new Team{Name="Consolidated Messenger"},
                    new Team{Name="Fourth Coffee"},
                    new Team{Name="Graphic Design Institute"},
                    new Team{Name="Nod Publishers"}
                };
        
                Team.PlayGames(teams);
        
                teams.ForEach(t => context.Teams.Add(t));
                context.SaveChanges();
            }
        }
        
        public class TeamConfiguration : DbConfiguration
        {
            public TeamConfiguration()
            {
                SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
            }
        }


2. Doppelklicken Sie im **Projektmappen-Explorer** **web.config** , um sie zu öffnen.

    ![Web.config][cache-web-config]

3.  Fügen Sie die folgende Zeichenfolge für die `connectionStrings` Abschnitt. Der Name der Verbindungszeichenfolge muss der Name der Datenkontextklasse Entity Framework-Datenbank ist `TeamContext`.

        <add name="TeamContext" connectionString="Data Source=(LocalDB)\v11.0;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True" providerName="System.Data.SqlClient" />


    Nach dem Hinzufügen, die `connectionStrings` Abschnitt sollte wie folgt aussehen.


        <connectionStrings>
            <add name="DefaultConnection" connectionString="Data Source=(LocalDb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\aspnet-ContosoTeamStats-20160216120918.mdf;Initial Catalog=aspnet-ContosoTeamStats-20160216120918;Integrated Security=True"
                providerName="System.Data.SqlClient" />
            <add name="TeamContext" connectionString="Data Source=(LocalDB)\v11.0;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True"  providerName="System.Data.SqlClient" />
        </connectionStrings>

### <a name="add-the-controller"></a>Den Controller hinzufügen

1. Drücken Sie **F6** , um das Projekt zu erstellen. 
2. Im **Projektmappen-Explorer**mit der rechten Maustaste in **des Ordners** , und wählen Sie **Hinzufügen**, **Controller**.

    ![Controller hinzufügen][cache-add-controller]

3. Wählen Sie **5 MVC-Controller mit Ansichten mit Entity Framework**, und klicken Sie auf **Hinzufügen**. Wenn eine Fehlermeldung angezeigt, nachdem Sie auf **Hinzufügen**, sicherstellen Sie, dass Sie zuerst das Projekt erstellt haben.

    ![Controller-Klasse hinzufügen][cache-add-controller-class]

5. Wählen Sie aus Dropdown- **Modellklasse** **Team (ContosoTeamStats.Models)** . Wählen Sie aus Dropdown- **Klasse** **TeamContext (ContosoTeamStats.Models)** . Typ `TeamsController` im Textfeld für den **Controller** (falls nicht automatisch ausgefüllt). Klicken Sie auf **Hinzufügen** , um die Controller-Klasse erstellen und die Standardansichten hinzufügen.

    ![Controller konfigurieren][cache-configure-controller]

4. Im **Projektmappen-Explorer**erweitern Sie **Global.asax** und doppelklicken Sie **Global.asax.cs** zum Öffnen.

    ![Global.asax.cs][cache-global-asax]

5. Fügen Sie die folgenden using-Anweisung am Anfang der Datei unter anderen using-Anweisung.


        using System.Data.Entity;
        using ContosoTeamStats.Models;


6. Fügen Sie folgenden Code am Ende der `Application_Start` Methode.


        Database.SetInitializer<TeamContext>(new TeamInitializer());


7. Erweitern Sie im **Projektmappen-Explorer**, `App_Start` , und doppelklicken Sie auf `RouteConfig.cs`.

    ![RouteConfig.cs][cache-RouteConfig-cs]

8. Ersetzen Sie `controller = "Home"` in den folgenden Code in die `RegisterRoutes` mit `controller = "Teams"` wie im folgenden Beispiel gezeigt.


        routes.MapRoute(
            name: "Default",
            url: "{controller}/{action}/{id}",
            defaults: new { controller = "Teams", action = "Index", id = UrlParameter.Optional }
        );


### <a name="configure-the-views"></a>Konfigurieren von Ansichten

1. Im **Projektmappen-Explorer**erweitern Sie den Ordner **Ansichten** und der **freigegebene** Ordner und doppelklicken Sie auf **_Layout.cshtml**. 

    ![_Layout.cshtml][cache-layout-cshtml]

2. Ändern der `title` Element, und Ersetzen `My ASP.NET Application` mit `Contoso Team Stats` wie im folgenden Beispiel gezeigt.


        <title>@ViewBag.Title - Contoso Team Stats</title>


3. In der `body` Abschnitt, aktualisieren Sie die erste `Html.ActionLink` Anweisung und Ersetzen `Application name` mit `Contoso Team Stats` und `Home` mit `Teams`.
    -   Vor:`@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })`
    -   Nach:`@Html.ActionLink("Contoso Team Stats", "Index", "Teams", new { area = "" }, new { @class = "navbar-brand" })`

    ![Ändern von Code][cache-layout-cshtml-code]

4. Drücken Sie **STRG + F5** , um die Anwendung erstellen und ausführen. Diese Version der Anwendung liest die Ergebnisse direkt aus der Datenbank. Beachten Sie die ** **Neu erstellen**, **Details**, und **Löschen Sie** **Aktionen, die automatisch die Anwendung **5 MVC-Controller mit Ansichten mit Entity Framework** Scaffold hinzugefügt wurden. Im nächsten Abschnitt des Lernprogramms fügen Sie Redis Cache Datenzugriff optimieren und zusätzliche Features der Anwendung.

![Anwendung][cache-starter-application]

## <a name="configure-the-application-to-use-redis-cache"></a>Konfigurieren Sie die Anwendung Redis verwendet werden

In diesem Teil des Lernprogramms konfigurieren Sie Beispiel-Anwendung zum Speichern und Abrufen von Contoso Team Statistiken von Azure Redis Cache-Instanz mithilfe des [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) -Cache-Clients.

-   [Konfigurieren Sie die Anwendung mit StackExchange.Redis](#configure-the-application-to-use-stackexchangeredis)
-   [Aktualisiert die TeamsController-Klasse, um Ergebnisse aus dem Cache oder der Datenbank zurück](#update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database)
-   [Aktualisieren Sie erstellen, bearbeiten und löschen Methoden zum Arbeiten mit dem cache](#update-the-create-edit-and-delete-methods-to-work-with-the-cache)
-   [Aktualisieren Sie die Teams Indexansicht Cache arbeiten](#update-the-teams-index-view-to-work-with-the-cache)


### <a name="configure-the-application-to-use-stackexchangeredis"></a>Konfigurieren Sie die Anwendung mit StackExchange.Redis

1. Um eine Clientanwendung in Visual Studio mit StackExchange.Redis NuGet-Paket konfigurieren, Maustaste auf das Projekt im **Projektmappen-Explorer** und **NuGet-Pakete verwalten**. 

    ![NuGet-Pakete verwalten][redis-cache-manage-nuget-menu]

2. Geben Sie **StackExchange.Redis** in das Textfeld Suchen, wählen Sie die gewünschte Version aus und klicken Sie auf **Installieren**.

    ![StackExchange.Redis NuGet-Paket][redis-cache-stack-exchange-nuget]

    NuGet-Paket heruntergeladen und erforderliche Assemblyverweise für die Clientanwendung auf Azure Redis Cache mit dem StackExchange.Redis Cache-Client hinzugefügt. Wenn eine starkem **StackExchange.Redis** -Clientbibliothek verwenden möchten, wählen Sie **StackExchange.Redis.StrongName**. Wählen Sie **StackExchange.Redis**.

3. Im **Projektmappen-Explorer**erweitern Sie **den Ordner** , und doppelklicken Sie auf **TeamsController.cs** , um sie zu öffnen.

    ![Teams-controller][cache-teamscontroller]

4. Fügen Sie die folgenden Anweisungen **TeamsController.cs**verwenden.

        using System.Configuration;
        using StackExchange.Redis;

5. Fügen Sie die folgenden zwei Eigenschaften, die `TeamsController` Klasse.

        // Redis Connection string info
        private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
        {
            string cacheConnection = ConfigurationManager.AppSettings["CacheConnection"].ToString();
            return ConnectionMultiplexer.Connect(cacheConnection);
        });
    
        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }
  
1. Erstellen Sie eine Datei auf dem Computer `WebAppPlusCacheAppSecrets.config` und an einem Ort mit den Quellcode der Anwendung sollten Sie irgendwo überprüfen überprüft wird nicht. In diesem Beispiel die `AppSettingsSecrets.config` Datei befindet sich unter `C:\AppSecrets\WebAppPlusCacheAppSecrets.config`.

    Bearbeiten der `WebAppPlusCacheAppSecrets.config` -Datei und fügen Sie den folgenden Inhalt hinzu. Wenn die Anwendung lokal ausgeführt wird diese Informationen Ihrer Azure Redis Cache-Instanz herstellen. Später im Lernprogramm Sie eine Azure Redis Cacheinstanz bereitstellen und aktualisieren den Cachenamen und das Kennwort. Wenn Sie Beispiel-Anwendung lokal ausführen möchten überspringen Sie die Erstellung dieser Datei und die nachfolgenden Schritte, die die Datei verweisen, da bei der Bereitstellung in Azure Cache-Verbindungsinformationen aus der app-Einstellung für das Web App und nicht aus dieser Datei abgerufen. Da die `WebAppPlusCacheAppSecrets.config` nicht bereitgestellt in Azure mit Ihrer Anwendung dies nicht erforderlich, wenn die Anwendung lokal ausgeführt werden soll.


        <appSettings>
          <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
        </appSettings>


2. Doppelklicken Sie im **Projektmappen-Explorer** **web.config** , um sie zu öffnen.

    ![Web.config][cache-web-config]

3. Fügen Sie den folgenden `file` -Attribut auf die `appSettings` Element. Wenn Sie einen anderen Namen oder Speicherort verwendet, ersetzen Sie diese Werte für die im Beispiel dargestellt.
    -   Vor:`<appSettings>`
    -   Nach:` <appSettings file="C:\AppSecrets\WebAppPlusCacheAppSecrets.config">`

    Die Runtime ASP.NET führt den Inhalt der externen Datei mit Markup in der `<appSettings>` Element. Wenn die angegebene Datei nicht gefunden, ignoriert die Laufzeit das Attribut. Ihre vertraulichen Daten (die Verbindungszeichenfolge für den Cache) sind nicht Teil des Quellcodes der Anwendung. Bei der Bereitstellung Ihrer Anwendung in Azure, die `WebAppPlusCacheAppSecrests.config` Datei wird nicht bereitgestellt (das gewünschte). Es gibt mehrere an diese Geheimnisse in Azure und in diesem Lernprogramm sie automatisch konfiguriert Sie beim [Bereitstellen die Azure-Ressourcen](#provision-the-azure-resources) in einem späteren Lernprogramm Schritt. Weitere Informationen zum Arbeiten mit Geheimnisse in Azure finden Sie unter [Best Practices für die Bereitstellung von Kennwörter und andere vertraulichen Daten auf ASP.NET und Azure App](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).


### <a name="update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database"></a>Aktualisiert die TeamsController-Klasse, um Ergebnisse aus dem Cache oder der Datenbank zurück

In diesem Beispiel können Team Statistiken aus der Datenbank oder aus dem Cache abgerufen werden. Team Statistiken werden im Cache als ein serialisiertes `List<Team>`, und auch eine sortierte Redis Datentypen. Beim Abrufen von Elementen aus einem sortierten Satz können alle, Abrufen oder Abfragen für bestimmte Elemente. In diesem Beispiel werden den sortierten Satz für die Top 5 Teams Ranking nach Anzahl der WINS-Abfrage.

>[AZURE.NOTE] Es ist nicht erforderlich die Team-Statistiken in mehreren Formaten im Cache speichern, um Azure Redis Cache verwenden. In diesem Lernprogramm mit mehreren Formaten den unterschiedlich und Datentypen zum Zwischenspeichern von Daten verwenden.



1. Fügen Sie die folgende using-Anweisung, die `TeamsController.cs` Datei mit anderen using-Anweisung.

        using System.Diagnostics;
        using Newtonsoft.Json;

2. Ersetzen Sie die aktuelle `public ActionResult Index()` Methode mit der folgenden Implementierung.


        // GET: Teams
        public ActionResult Index(string actionType, string resultType)
        {
            List<Team> teams = null;
        
            switch(actionType)
            {
                case "playGames": // Play a new season of games.
                    PlayGames();
                    break;
        
                case "clearCache": // Clear the results from the cache.
                    ClearCachedTeams();
                    break;
        
                case "rebuildDB": // Rebuild the database with sample data.
                    RebuildDB();
                    break;
            }
        
            // Measure the time it takes to retrieve the results.
            Stopwatch sw = Stopwatch.StartNew();
        
            switch(resultType)
            {
                case "teamsSortedSet": // Retrieve teams from sorted set.
                    teams = GetFromSortedSet();
                    break;
        
                case "teamsSortedSetTop5": // Retrieve the top 5 teams from the sorted set.
                    teams = GetFromSortedSetTop5();
                    break;
        
                case "teamsList": // Retrieve teams from the cached List<Team>.
                    teams = GetFromList();
                    break;
        
                case "fromDB": // Retrieve results from the database.
                default:
                    teams = GetFromDB();
                    break;
            }
        
            sw.Stop();
            double ms = sw.ElapsedTicks / (Stopwatch.Frequency / (1000.0));

            // Add the elapsed time of the operation to the ViewBag.msg.
            ViewBag.msg += " MS: " + ms.ToString();
        
            return View(teams);
        }


3. Hinzufügen der folgenden drei Methoden, um die `TeamsController` Klasse implementiert die `playGames`, `clearCache`, und `rebuildDB` Aktivitätstypen aus der Switch-Anweisung im vorherigen Codeausschnitt hinzugefügt.

    Die `PlayGames` Team Statistik durch simulieren Saison Spiele aktualisiert, speichert die Ergebnisse in der Datenbank und löscht die jetzt veralteten Daten aus dem Cache.


        void PlayGames()
        {
            ViewBag.msg += "Updating team statistics. ";
            // Play a "season" of games.
            var teams = from t in db.Teams
                        select t;
    
            Team.PlayGames(teams);
    
            db.SaveChanges();
    
            // Clear any cached results
            ClearCachedTeams();
        }


    Die `RebuildDB` Methode initialisiert die Datenbank mit Teams Statistiken generiert und jetzt veralteten Daten aus dem Cache gelöscht.
    
        void RebuildDB()
        {
            ViewBag.msg += "Rebuilding DB. ";
            // Delete and re-initialize the database with sample data.
            db.Database.Delete();
            db.Database.Initialize(true);

            // Clear any cached results
            ClearCachedTeams();
        }


    Die `ClearCachedTeams` -Methode zwischengespeicherten Team Statistiken aus dem Cache entfernt.

    
        void ClearCachedTeams()
        {
            IDatabase cache = Connection.GetDatabase();
            cache.KeyDelete("teamsList");
            cache.KeyDelete("teamsSortedSet");
            ViewBag.msg += "Team data removed from cache. ";
        } 


4. Fügen Sie die folgenden vier Methoden, die `TeamsController` Klasse implementiert die Arten der statistischen Team aus dem Cache abgerufen. Jede dieser Methoden gibt einen `List<Team>` , wird die Ansicht angezeigt.

    Die `GetFromDB` -Methode liest die Team-Statistiken aus der Datenbank.

        List<Team> GetFromDB()
        {
            ViewBag.msg += "Results read from DB. ";
            var results = from t in db.Teams
                orderby t.Wins descending
                select t; 
    
            return results.ToList<Team>();
        }


    Die `GetFromList` -Methode liest die Team-Statistiken aus Cache wie ein serialisiertes `List<Team>`. Ist ein fehlgeschlagener Cachezugriff, werden das Team aus der Datenbank gelesen und dann zum nächsten Mal im Cache gespeichert. In diesem Beispiel verwenden wir JSON.NET Serialisierung serialisiert .NET Objekte und aus dem Cache. Weitere Informationen finden Sie unter [Arbeiten mit .NET Objekte in Azure Redis Cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).

        List<Team> GetFromList()
        {
            List<Team> teams = null;

            IDatabase cache = Connection.GetDatabase();
            string serializedTeams = cache.StringGet("teamsList");
            if (!String.IsNullOrEmpty(serializedTeams))
            {
                teams = JsonConvert.DeserializeObject<List<Team>>(serializedTeams);

                ViewBag.msg += "List read from cache. ";
            }
            else
            {
                ViewBag.msg += "Teams list cache miss. ";
                // Get from database and store in cache
                teams = GetFromDB();

                ViewBag.msg += "Storing results to cache. ";
                cache.StringSet("teamsList", JsonConvert.SerializeObject(teams));
            }
            return teams;
        }


    Die `GetFromSortedSet` -Methode liest Team Statistiken aus einer zwischengespeicherten sortiert. Ist ein fehlgeschlagener Cachezugriff, werden das Team aus der Datenbank gelesen und im Cache als sortierten Satz gespeichert.


        List<Team> GetFromSortedSet()
        {
            List<Team> teams = null;
            IDatabase cache = Connection.GetDatabase();
            // If the key teamsSortedSet is not present, this method returns a 0 length collection.
            var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", order: Order.Descending);
            if (teamsSortedSet.Count() > 0)
            {
                ViewBag.msg += "Reading sorted set from cache. ";
                teams = new List<Team>();
                foreach (var t in teamsSortedSet)
                {
                    Team tt = JsonConvert.DeserializeObject<Team>(t.Element);
                    teams.Add(tt);
                }
            }
            else
            {
                ViewBag.msg += "Teams sorted set cache miss. ";
    
                // Read from DB
                teams = GetFromDB();
    
                ViewBag.msg += "Storing results to cache. ";
                foreach (var t in teams)
                {
                    Console.WriteLine("Adding to sorted set: {0} - {1}", t.Name, t.Wins);
                    cache.SortedSetAdd("teamsSortedSet", JsonConvert.SerializeObject(t), t.Wins);
                }
            }
            return teams;
        }


    Die `GetFromSortedSetTop5` -Methode liest die Top 5 Teams aus zwischengespeicherten sortierten Satz. Es beginnt mit der Überprüfung des Caches für das Vorhandensein der `teamsSortedSet` Schlüssel. Wenn dieser Schlüssel nicht vorhanden ist, die `GetFromSortedSet` -Methode aufgerufen, um das Team Statistiken und im Cache gespeichert. Weiter zwischengespeicherte sortierte Satz Top 5 Teams abgefragt, die zurückgegeben werden.


        List<Team> GetFromSortedSetTop5()
        {
            List<Team> teams = null;
            IDatabase cache = Connection.GetDatabase();

            // If the key teamsSortedSet is not present, this method returns a 0 length collection.
            var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
            if(teamsSortedSet.Count() == 0)
            {
                // Load the entire sorted set into the cache.
                GetFromSortedSet();

                // Retrieve the top 5 teams.
                teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
            }

            ViewBag.msg += "Retrieving top 5 teams from cache. ";
            // Get the top 5 teams from the sorted set
            teams = new List<Team>();
            foreach (var team in teamsSortedSet)
            {
                teams.Add(JsonConvert.DeserializeObject<Team>(team.Element));
            }
            return teams;
        }


### <a name="update-the-create-edit-and-delete-methods-to-work-with-the-cache"></a>Aktualisieren Sie erstellen, bearbeiten und löschen Methoden zum Arbeiten mit dem cache

Gerüstbau Code, der als Teil des Beispiels enthält Methoden zum Hinzufügen, bearbeiten und löschen. Jederzeit hinzugefügt, bearbeitet oder entfernt ein Team werden die Daten im Cache veraltet. In diesem Abschnitt ändern Sie diese drei Methoden der zwischengespeicherten Teams deaktivieren, so dass der Cache mit der Datenbank.

1. Wechseln Sie zu der `Create(Team team)` -Methode in der `TeamsController` Klasse. Fügen Sie einen Aufruf der `ClearCachedTeams` -Methode, wie im folgenden Beispiel gezeigt.


        // POST: Teams/Create
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Create([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
        {
            if (ModelState.IsValid)
            {
                db.Teams.Add(team);
                db.SaveChanges();
                // When a team is added, the cache is out of date.
                // Clear the cached teams.
                ClearCachedTeams();
                return RedirectToAction("Index");
            }
    
            return View(team);
        }


2. Wechseln Sie zu der `Edit(Team team)` -Methode in der `TeamsController` Klasse. Fügen Sie einen Aufruf der `ClearCachedTeams` -Methode, wie im folgenden Beispiel gezeigt.


        // POST: Teams/Edit/5
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Edit([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
        {
            if (ModelState.IsValid)
            {
                db.Entry(team).State = EntityState.Modified;
                db.SaveChanges();
                // When a team is edited, the cache is out of date.
                // Clear the cached teams.
                ClearCachedTeams();
                return RedirectToAction("Index");
            }
            return View(team);
        }


3. Wechseln Sie zu der `DeleteConfirmed(int id)` -Methode in der `TeamsController` Klasse. Fügen Sie einen Aufruf der `ClearCachedTeams` -Methode, wie im folgenden Beispiel gezeigt.


        // POST: Teams/Delete/5
        [HttpPost, ActionName("Delete")]
        [ValidateAntiForgeryToken]
        public ActionResult DeleteConfirmed(int id)
        {
            Team team = db.Teams.Find(id);
            db.Teams.Remove(team);
            db.SaveChanges();
            // When a team is deleted, the cache is out of date.
            // Clear the cached teams.
            ClearCachedTeams();
            return RedirectToAction("Index");
        }


### <a name="update-the-teams-index-view-to-work-with-the-cache"></a>Aktualisieren Sie die Teams Indexansicht Cache arbeiten

1. Im **Projektmappen-Explorer** **auf** Ordner, die **Teams** -Ordner, und doppelklicken Sie auf **Index.cshtml**.

    ![Index.cshtml][cache-views-teams-index-cshtml]

2. Suchen Sie im oberen Bereich der Datei die folgenden Absatzelement.

    ![Tabelle für Aktivität][cache-teams-index-table]

    Dies ist der Link zu einem neuen Teamprojekt. In der folgenden Tabelle wird ersetzen Sie das Absatzelement. Diese Tabelle hat Aktionslinks für neue Teams erstellen, eine neue Saison Spiele Löschen des Caches, die Teams aus dem Cache in verschiedenen Formaten abrufen, die Teams aus der Datenbank abgerufen und Datenbank mit frischen Daten.


        <table class="table">
            <tr>
                <td>
                    @Html.ActionLink("Create New", "Create")
                </td>
                <td>
                    @Html.ActionLink("Play Season", "Index", new { actionType = "playGames" })
                </td>
                <td>
                    @Html.ActionLink("Clear Cache", "Index", new { actionType = "clearCache" })
                </td>
                <td>
                    @Html.ActionLink("List from Cache", "Index", new { resultType = "teamsList" })
                </td>
                <td>
                    @Html.ActionLink("Sorted Set from Cache", "Index", new { resultType = "teamsSortedSet" })
                </td>
                <td>
                    @Html.ActionLink("Top 5 Teams from Cache", "Index", new { resultType = "teamsSortedSetTop5" })
                </td>
                <td>
                    @Html.ActionLink("Load from DB", "Index", new { resultType = "fromDB" })
                </td>
                <td>
                    @Html.ActionLink("Rebuild DB", "Index", new { actionType = "rebuildDB" })
                </td>
            </tr>    
        </table>


3. Führen Sie einen Bildlauf an das Ende der **Index.cshtml** -Datei und fügen Sie den folgenden `tr` Element, so dass die letzte Zeile in der letzten Tabelle in die Datei.

        <tr><td colspan="5">@ViewBag.Msg</td></tr>

    Diese Zeile zeigt den Wert der `ViewBag.Msg` enthält einen Statusbericht über den aktuellen Vorgang der festgelegt wird, wenn eine Aktionslinks aus dem vorherigen Schritt klicken.   

    ![Statusnachricht][cache-status-message]

4. Drücken Sie **F6** , um das Projekt zu erstellen.

## <a name="provision-the-azure-resources"></a>Azure Bereitstellungsressourcen

Zum Hosten der Anwendung in Azure müssen Sie zuerst die Azure Services bereitstellen, die Ihre Anwendung benötigt. Das Beispiel in diesem Lernprogramm Anwendung folgenden Azure Services.

-   Azure Redis Cache
-   App Service WebApp
-   SQL-Datenbank

Zum Bereitstellen dieser Dienste auf eine neue oder eine vorhandene Gruppe klicken Sie folgenden **Azure bereitstellen** .

[! [Bereitstellung in Azure] [Deploybutton]](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-redis-cache-sql-database%2Fazuredeploy.json)

Diese Schaltfläche **Bereitstellen in Azure** verwendet die [Create Web App plus Redis Cache sowie SQL-Datenbank](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-redis-cache-sql-database) [Azure Schnellstart](https://github.com/Azure/azure-quickstart-templates) Vorlage diese Dienste bereitstellen und die Verbindungszeichenfolge für die SQL-Datenbank und die Anwendung für die Verbindungszeichenfolge Azure Redis Cache.

>[AZURE.NOTE] Ein Azure-Konto haben, haben Sie [ein kostenloses Azure Konto](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero) in wenigen Minuten.

**Bereitstellen in Azure** gelangen Sie zum Azure-Portal und initiiert den Prozess der Erstellung der Vorlage beschriebenen Ressourcen.

![In Azure bereitstellen][cache-deploy-to-azure-step-1]

1. **Benutzerdefinierte Bereitstellung** Blade Azure Abonnement, und wählen Sie eine vorhandene Ressourcengruppe oder eine neue zu erstellen, und geben Sie den Speicherort der Ressource.
2. Das Blade **Parameter** geben einen Administratorkontonamen (**ADMINISTRATORLOGIN** - **Admin**nicht verwenden), Administrator-Passwort (**ADMINISTRATORLOGINPASSWORD**) und Datenbanknamen (**Datenbankname**). Die Parameter sind für App kostenlos hosting Plan und niedrigere Kostenoptionen für SQL-Datenbank und Azure Redis Cache, die mit einem freien nicht konfiguriert.
3. Einstellung bei Bedarf ändern Sie oder übernehmen Sie die Standardeinstellungen, und klicken Sie auf **OK**.


![In Azure bereitstellen][cache-deploy-to-azure-step-2]

1. Klicken Sie auf **Prüfung Vertragsbedingungen**.
2. Lesen Sie auf der **Bestellung** und klicken Sie auf **kaufen**.
3. Klicken Sie Bereitstellen von Ressourcen **Erstellen** auf die **benutzerdefinierte Bereitstellung** .

Um den Fortschritt der Bereitstellung anzuzeigen, klicken Sie auf das Symbol, und klicken Sie auf **Bereitstellung wurde gestartet**.

![Bereitstellung gestartet][cache-deployment-started]

Sie können den Status der Bereitstellung auf der **Microsoft.Template** anzeigen.

![In Azure bereitstellen][cache-deploy-to-azure-step-3]

Nach Abschluss der Bereitstellung können Sie Ihre Anwendung in Azure aus Visual Studio veröffentlichen.

>[AZURE.NOTE] Während des Bereitstellungsprozesses auftretende Fehler werden auf der **Microsoft.Template** angezeigt. Häufige Fehler sind zu viele SQL Server oder zu viele App kostenlos Pläne pro Abonnement hosten. Beheben Sie alle Fehler und den Prozess auf **erneut** **Microsoft.Template** Blade oder die Schaltfläche **Bereitstellen in Azure** in diesem Lernprogramm.

## <a name="publish-the-application-to-azure"></a>Veröffentlichen Sie die Anwendung in Azure

In diesem Schritt des Lernprogramms Sie Anwendung in Azure veröffentlichen und in der Cloud ausgeführt.

1. Maustaste auf das **ContosoTeamStats** -Projekt in Visual Studio und **Veröffentlichen**.

    ![Veröffentlichen][cache-publish-app]

2. Klicken Sie auf **Microsoft Azure App**.

    ![Veröffentlichen][cache-publish-to-app-service]

3. Abonnement verwendet, wenn die Azure-Ressourcen erstellen, erweitern Sie die Ressourcengruppe, Ressourcen wählen Sie das gewünschte Web App und klicken Sie auf **OK**. Wenn Sie die Schaltfläche **Bereitstellen in Azure** Ihr Web App mit **WebSite** einige zusätzliche Zeichen beginnt verwendet.

    ![Wählen Sie WebApp][cache-select-web-app]

4. Klicken Sie auf **Verbindung prüfen** Ihre Einstellungen und dann auf **Veröffentlichen**.

    ![Veröffentlichen][cache-publish]

    Nach einigen Augenblicken wird der Veröffentlichungsprozess abgeschlossen und Browser startet mit dem Beispiel. Wenn man einen DNS-Fehler beim Überprüfen oder Veröffentlichen der Bereitstellungsprozess Azure Ressourcen für die Anwendung kürzlich abgeschlossen wurde, warten Sie einen Moment und versuchen Sie es erneut.

    ![Cache hinzugefügt][cache-added-to-application]

Die folgende Tabelle beschreibt jeden Aktionslink aus der beispielanwendung.

| Aktion                  | Beschreibung                                                                                                                                                      |
|-------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Neu erstellen              | Erstellen Sie ein neues Team.                                                                                                                                               |
| Jahreszeit spielen             | Saison Spiele spielen, teamstatistiken aktualisieren und Entfernen veralteter Daten aus dem Cache-team.                                                                          |
| Cache löschen             | Löschen Sie Team Statistik aus dem Cache.                                                                                                                             |
| Liste aus Cache         | Teamstatistiken aus dem Cache abrufen. Ist ein fehlgeschlagener Cachezugriff, die Werte aus der Datenbank geladen und in den Cache beim nächsten speichern.                                        |
| Sortierten Satz aus Cache   | Rufen Sie teamstatistiken aus dem Cache mit sortierten ab. Ist ein fehlgeschlagener Cachezugriff, die Werte aus der Datenbank geladen und mit sortierten Cache speichern.  |
| Top 5-Teams aus Cache  | Rufen Sie Top 5 Teams aus einem sortierten Satz Cache ab. Ist ein fehlgeschlagener Cachezugriff, die Werte aus der Datenbank geladen und mit sortierten Cache speichern. |
| Aus Datenbank laden            | Teamstatistiken aus der Datenbank abrufen.                                                                                                                       |
| Rebuild DB              | Die Datenbank neu erstellen und mit Beispieldaten Team erneut.                                                                                                        |
| Bearbeiten / Details / löschen | Bearbeiten eines Teams werden Details für ein Team, ein Team löschen.                                                                                                             |


Klicken Sie auf Aktionen und experimentieren Sie mit Daten aus verschiedenen Quellen abrufen. Keine Unterschiede in der Zeit dauert es verschiedene Methoden zum Abrufen von Daten aus der Datenbank und der Cache.

## <a name="delete-the-resources-when-you-are-finished-with-the-application"></a>Löschen Sie die Ressourcen, wenn Sie mit der Anwendung

Wenn Sie der Anwendung der praktischen Einführung Beispiel haben, können Sie Azure Ressourcen verwendet, um Kosten und Ressourcen sparen löschen. Wenn Sie die Schaltfläche **Bereitstellen in Azure** im Abschnitt [Azure Ressourcen bereitstellen](#provision-the-azure-resources) und alle Ressourcen in derselben Ressourcengruppe befinden, können Sie sie zusammen in einem Vorgang löschen durch Löschen der Ressourcengruppe.

1. [Azure-Portal](https://portal.azure.com) melden Sie an und auf **Ressourcen**.
2. Geben Sie den Namen der Ressourcengruppe, in das Textfeld **filtern...** .
3. Klicken Sie auf **...** rechts neben der Ressourcengruppe.
4. Klicken Sie auf **Löschen**.

    ![Löschen][cache-delete-resource-group]

5. Geben Sie den Namen der Ressourcengruppe und klicken Sie auf **Löschen**.

    ![Löschen bestätigen][cache-delete-confirm]

Nach einigen Augenblicken werden die Ressourcengruppe und alle darin enthaltenen Ressourcen gelöscht.

>[AZURE.IMPORTANT] Hinweis eine Ressourcengruppe Löschen rückgängig gemacht werden, die der Ressourcengruppe und alle Ressourcen gelöscht werden. Stellen Sie sicher, dass nicht versehentlich die falsche Ressourcengruppe oder Ressourcen löschen. Wenn die Ressourcen für das hosting in diesem Beispiel in eine vorhandene Ressourcengruppe erstellt haben, können Sie jede Ressource aus ihrer jeweiligen einzeln löschen.

## <a name="run-the-sample-application-on-your-local-machine"></a>Beispielanwendung auf dem lokalen Computer ausführen

Um die Anwendung lokal auf Ihrem Computer auszuführen, benötigen Sie eine Instanz Azure Redis Cache, cache-Daten. 

-   Wenn Sie Ihre Anwendung in Azure veröffentlicht haben, wie im vorherigen Abschnitt beschrieben, können Sie die Azure Redis Cache-Instanz, die in diesem Schritt bereitgestellt wurde.
-   Haben Sie eine andere vorhandene Azure Redis Cache-Instanz können, die Sie dieses Beispiel lokal ausgeführt.
-   Benötigen Sie eine Azure Redis Cache-Instanz erstellen, können Sie unter [Create Cache](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache)Schritte.

Nach ausgewählt oder erstellt den Cache mit Cache in Azure-Portal suchen Sie und rufen Sie der [Host-](cache-configure.md#properties) und [Zugriffstasten](cache-configure.md#access-keys) für den Cache ab. Informationen finden Sie unter [Konfigurieren Redis Cache Settings](cache-configure.md#configure-redis-cache-settings).

1. Öffnen der `WebAppPlusCacheAppSecrets.config` beim [Konfigurieren der Anwendung Redis Cache](#configure-the-application-to-use-redis-cache) Schritt dieses Lernprogramms mit dem Editor Ihrer Wahl erstellten Datei.

2. Bearbeiten der `value` -Attribut und Ersetzen `MyCache.redis.cache.windows.net` mit dem [Hostnamen](cache-configure.md#properties) des Cache, und geben Sie entweder den [primären oder sekundären Schlüssel](cache-configure.md#access-keys) des Cache als Kennwort.


        <appSettings>
          <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
        </appSettings>


3. Drücken Sie **STRG + F5,** um die Anwendung auszuführen.

>[AZURE.NOTE] Beachten Sie, dass die Anwendung einschließlich der Datenbank lokal ausgeführt wird und die Redis Cache in Azure gehostet wird, der Cache erscheinen unter der Datenbank ausführen. Für eine optimale Leistung sollte die Clientanwendung und Azure Redis Cacheinstanz an derselben Stelle. 

## <a name="next-steps"></a>Nächste Schritte

-   Erfahren Sie mehr über [Erste Schritte mit ASP.NET MVC 5](http://www.asp.net/mvc/overview/getting-started/introduction/getting-started) auf [ASP.NET](http://asp.net/) Website.
-   Weitere Beispiele für das Erstellen einer ASP.NET Web App in App Service finden Sie unter [Erstellen und Bereitstellen eine ASP.NET Web-Anwendung in Azure App Service](https://github.com/Microsoft/HealthClinic.biz/wiki/Create-and-deploy-an-ASP.NET-web-app-in-Azure-App-Service) [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 verbinden [Demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/).
    -   Weitere Schnellstarts HealthClinic.biz Demo finden Sie unter [Azure Developer Tools Schnellstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).
-   Weitere Informationen über Verfahren [Code zuerst in eine neue Datenbank](https://msdn.microsoft.com/data/jj193542) in Entity Framework, das in diesem Lernprogramm verwendet wird.
-   Erfahren Sie mehr über [Web-apps in Azure App Service](../app-service-web/app-service-web-overview.md).
-   Erfahren Sie, wie [Monitor](cache-how-to-monitor.md) Cache im Azure-Portal.

-   Untersuchen von Azure Redis Cache-Zusatzfunktionen
    -   [Dauerhaftigkeit Premium Azure Redis Cache konfigurieren](cache-how-to-premium-persistence.md)
    -   [Clustering für Premium Azure Redis Cache konfigurieren](cache-how-to-premium-clustering.md)
    -   [Virtuelle Netzwerk-Support für Premium Azure Redis Cache konfigurieren](cache-how-to-premium-vnet.md)
    -   [Azure Redis Cache FAQ](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) für Weitere Informationen zur Größe, Durchsatz und Bandbreite mit Premium-Caches anzeigen



<!-- IMAGES -->
[cache-starter-application]: ./media/cache-web-app-howto/cache-starter-application.png
[cache-added-to-application]: ./media/cache-web-app-howto/cache-added-to-application.png
[cache-create-project]: ./media/cache-web-app-howto/cache-create-project.png
[cache-select-template]: ./media/cache-web-app-howto/cache-select-template.png
[cache-model-add-class]: ./media/cache-web-app-howto/cache-model-add-class.png
[cache-model-add-class-dialog]: ./media/cache-web-app-howto/cache-model-add-class-dialog.png
[cache-add-controller]: ./media/cache-web-app-howto/cache-add-controller.png
[cache-add-controller-class]: ./media/cache-web-app-howto/cache-add-controller-class.png
[cache-configure-controller]: ./media/cache-web-app-howto/cache-configure-controller.png
[cache-global-asax]: ./media/cache-web-app-howto/cache-global-asax.png
[cache-RouteConfig-cs]: ./media/cache-web-app-howto/cache-RouteConfig-cs.png
[cache-layout-cshtml]: ./media/cache-web-app-howto/cache-layout-cshtml.png
[cache-layout-cshtml-code]: ./media/cache-web-app-howto/cache-layout-cshtml-code.png
[redis-cache-manage-nuget-menu]: ./media/cache-web-app-howto/redis-cache-manage-nuget-menu.png
[redis-cache-stack-exchange-nuget]: ./media/cache-web-app-howto/redis-cache-stack-exchange-nuget.png
[cache-teamscontroller]: ./media/cache-web-app-howto/cache-teamscontroller.png
[cache-web-config]: ./media/cache-web-app-howto/cache-web-config.png
[cache-views-teams-index-cshtml]: ./media/cache-web-app-howto/cache-views-teams-index-cshtml.png
[cache-teams-index-table]: ./media/cache-web-app-howto/cache-teams-index-table.png
[cache-status-message]: ./media/cache-web-app-howto/cache-status-message.png
[deploybutton]: ./media/cache-web-app-howto/deploybutton.png
[cache-deploy-to-azure-step-1]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-1.png
[cache-deploy-to-azure-step-2]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-2.png
[cache-deploy-to-azure-step-3]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-3.png
[cache-deployment-started]: ./media/cache-web-app-howto/cache-deployment-started.png
[cache-publish-app]: ./media/cache-web-app-howto/cache-publish-app.png
[cache-publish-to-app-service]: ./media/cache-web-app-howto/cache-publish-to-app-service.png
[cache-select-web-app]: ./media/cache-web-app-howto/cache-select-web-app.png
[cache-publish]: ./media/cache-web-app-howto/cache-publish.png
[cache-delete-resource-group]: ./media/cache-web-app-howto/cache-delete-resource-group.png
[cache-delete-confirm]: ./media/cache-web-app-howto/cache-delete-confirm.png

