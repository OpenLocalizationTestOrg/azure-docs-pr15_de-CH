<properties
    pageTitle="Erste Schritte mit Azure CDN-Bibliothek für .NET | Microsoft Azure"
    description="Informationen Sie zum Verwalten von Azure CDN mit Visual Studio schreiben."
    services="cdn"
    documentationCenter=".net"
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="casoper"/>

# <a name="get-started-with-azure-cdn-development"></a>Erste Schritte mit Azure CDN

> [AZURE.SELECTOR]
- [Node.js](cdn-app-dev-node.md)
- [.NET](cdn-app-dev-net.md)

[Azure CDN-Bibliothek für .NET](https://msdn.microsoft.com/library/mt657769.aspx) können Erstellung und Verwaltung von CDN Profile und Endpunkte.  Dieses Lernprogramm führt durch die Erstellung einer einfachen .NET Konsole Anwendung, die mehrere verfügbare Vorgänge veranschaulicht.  Dieses Lernprogramm soll nicht alle Aspekte der Azure CDN-Bibliothek für .NET ausführlich zu beschreiben.

Sie benötigen Visual Studio 2015 um das Lernprogramm abzuschließen.  [Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) ist kostenlos zum Download zur Verfügung.

> [AZURE.TIP] [Projekt aus diesem Lernprogramm](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c) steht zum Download auf MSDN.

[AZURE.INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-nuget-packages"></a>Das Projekt erstellen und Hinzufügen von Nuget-Paketen

Unsere Azure AD Anwendung Erlaubnis CDN Profile und Endpunkte innerhalb der Gruppe verwalten und eine Ressourcengruppe für CDN Profile erstellt haben, können wir beginnen unsere Anwendung.

Klicken Sie in Visual Studio 2015 auf **Datei**, **neu**, **Projekt** zum neuen Projekt öffnen.  Erweitern Sie **Visual C#**, und wählen Sie dann **Windows** im Bereich auf der linken Seite.  Klicken Sie im mittleren Bereich auf **Konsolenanwendungsprojekt** .  Nennen Sie das Projekt und dann auf **OK**.  

![Neues Projekt](./media/cdn-app-dev-net/cdn-new-project.png)

Unser Projekt soll einige Azure Bibliotheken in Nuget-Pakete enthalten.  Fügen Sie diese in das Projekt.

1. Klicken Sie auf **Extras** , **Nuget Paket-Manager**und **Paket-Manager-Konsole**.

    ![Nuget-Pakete verwalten](./media/cdn-app-dev-net/cdn-manage-nuget.png)

2. Führen Sie in der Konsole Paket-Manager den folgenden Befehl zum Installieren von **Active Directory Authentifizierung Library (ADAL)**:

    `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory`

3. Führen Sie zum Installieren der **Azure CDN Management Library**Folgendes:

    `Install-Package Microsoft.Azure.Management.Cdn`

## <a name="directives-constants-main-method-and-helper-methods"></a>Richtlinien, Konstanten Hauptmethode und Hilfsmethoden

Lassen Sie die grundlegende Struktur des Programms geschrieben.

1. Ersetzen Sie in der Registerkarte "Program.cs" die `using` mit den folgenden Richtlinien:

    ```csharp
    using System;
    using System.Collections.Generic;
    using Microsoft.Azure.Management.Cdn;
    using Microsoft.Azure.Management.Cdn.Models;
    using Microsoft.Azure.Management.Resources;
    using Microsoft.Azure.Management.Resources.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```

2. Wir müssen einige Konstanten definieren unserer Methoden verwenden.  In der `Program` Klasse vor der `Main` Methode, fügen Sie Folgendes hinzu.  Platzhalter, einschließlich ersetzen die ** &lt;Winkel&gt;**, mit eigenen Werten.

    ```csharp
    //Tenant app constants
    private const string clientID = "<YOUR CLIENT ID>";
    private const string clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    private const string authority = "https://login.microsoftonline.com/<YOUR TENANT ID>/<YOUR TENANT DOMAIN NAME>";

    //Application constants
    private const string subscriptionId = "<YOUR SUBSCRIPTION ID>";
    private const string profileName = "CdnConsoleApp";
    private const string endpointName = "<A UNIQUE NAME FOR YOUR CDN ENDPOINT>";
    private const string resourceGroupName = "CdnConsoleTutorial";
    private const string resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```

3. Auch auf der Klassenebene definieren Sie dieser beiden Variablen.  Wir verwenden diese später um festzustellen, ob unser Profil- und Endpunkt vorhanden.

    ```csharp
    static bool profileAlreadyExists = false;
    static bool endpointAlreadyExists = false;
    ```

4.  Ersetzen Sie die `Main` -Methode wie folgt:

    ```csharp
    static void Main(string[] args)
    {
        //Get a token
        AuthenticationResult authResult = GetAccessToken();

        // Create CDN client
        CdnManagementClient cdn = new CdnManagementClient(new TokenCredentials(authResult.AccessToken))
            { SubscriptionId = subscriptionId };

        ListProfilesAndEndpoints(cdn);

        // Create CDN Profile
        CreateCdnProfile(cdn);

        // Create CDN Endpoint
        CreateCdnEndpoint(cdn);
        
        Console.WriteLine();

        // Purge CDN Endpoint
        PromptPurgeCdnEndpoint(cdn);

        // Delete CDN Endpoint
        PromptDeleteCdnEndpoint(cdn);

        // Delete CDN Profile
        PromptDeleteCdnProfile(cdn);

        Console.WriteLine("Press Enter to end program.");
        Console.ReadLine();
    }
    ```

5. Einige unserer Methoden werden die Benutzer mit "Ja/Nein" Fragen.  Fügen Sie die folgende Methode, die ein wenig zu vereinfachen:

    ```csharp
    private static bool PromptUser(string Question)
    {
        Console.Write(Question + " (Y/N): ");
        var response = Console.ReadKey();
        Console.WriteLine();
        if (response.Key == ConsoleKey.Y)
        {
            return true;
        }
        else if (response.Key == ConsoleKey.N)
        {
            return false;
        }
        else
        {
            // They pressed something other than Y or N.  Let's ask them again.
            return PromptUser(Question);
        }
    }
    ```

Da die grundlegende Struktur des Programms steht, sollten wir vom aufgerufenen Methoden erstellen die `Main` Methode.

## <a name="authentication"></a>Authentifizierung

Bevor wir Azure CDN Management Bibliothek verwenden können, müssen wir unsere Dienstprinzipalnamen authentifizieren und ein Authentifizierungstoken.  Diese Methode verwendet die ADAL, das Token abzurufen.

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority); 
    ClientCredential credential = new ClientCredential(clientID, clientSecret);
    AuthenticationResult authResult = 
        authContext.AcquireTokenAsync("https://management.core.windows.net/", credential).Result;

    return authResult;
}
```

Verwenden Sie einzelne Benutzerauthentifizierung die `GetAccessToken` -Methode sieht etwas anders.

>[AZURE.IMPORTANT] Verwenden Sie dieses Codebeispiel nur, wenn Benutzer Authentifizierung statt Dienstprinzipal Auswahl.

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority);
    AuthenticationResult authResult = authContext.AcquireTokenAsync("https://management.core.windows.net/",
        clientID, new Uri("http://<redirect URI>"), new PlatformParameters(PromptBehavior.RefreshSession)).Result;

    return authResult;
}
```

Ersetzen Sie `<redirect URI>` mit Umleitung URI eingegeben, wenn die Anwendung in Azure AD registriert.

## <a name="list-cdn-profiles-and-endpoints"></a>Liste CDN Profile und Endpunkte

Wir können nun CDN-Operationen.  Unsere Methode zunächst ist Liste alle Profile und Endpunkte in unserer Ressourcengruppe und findet eine Übereinstimmung für Profil und Endpunkt in unserer Konstanten angegeben vermerkt, die später wir keine Duplikate zu erstellen versuchen.

```csharp
private static void ListProfilesAndEndpoints(CdnManagementClient cdn)
{
    // List all the CDN profiles in this resource group
    var profileList = cdn.Profiles.ListByResourceGroup(resourceGroupName);
    foreach (Profile p in profileList)
    {
        Console.WriteLine("CDN profile {0}", p.Name);
        if (p.Name.Equals(profileName, StringComparison.OrdinalIgnoreCase))
        {
            // Hey, that's the name of the CDN profile we want to create!
            profileAlreadyExists = true;
        }

        //List all the CDN endpoints on this CDN profile
        Console.WriteLine("Endpoints:");
        var endpointList = cdn.Endpoints.ListByProfile(p.Name, resourceGroupName);
        foreach (Endpoint e in endpointList)
        {
            Console.WriteLine("-{0} ({1})", e.Name, e.HostName);
            if (e.Name.Equals(endpointName, StringComparison.OrdinalIgnoreCase))
            {
                // The unique endpoint name already exists.
                endpointAlreadyExists = true;
            }
        }
        Console.WriteLine();
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a>Erstellen Sie CDN Profile und Endpunkte

Als Nächstes erstellen wir ein Profil.

```csharp
private static void CreateCdnProfile(CdnManagementClient cdn)
{
    if (profileAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating profile {0}.", profileName);
        ProfileCreateParameters profileParms =
            new ProfileCreateParameters() { Location = resourceLocation, Sku = new Sku(SkuName.StandardVerizon) };
        cdn.Profiles.Create(profileName, profileParms, resourceGroupName);
    }
}
```

Sobald das Profil erstellt wurde, erstellen wir einen Endpunkt.

```csharp
private static void CreateCdnEndpoint(CdnManagementClient cdn)
{
    if (endpointAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating endpoint {0} on profile {1}.", endpointName, profileName);
        EndpointCreateParameters endpointParms =
            new EndpointCreateParameters()
            {
                Origins = new List<DeepCreatedOrigin>() { new DeepCreatedOrigin("Contoso", "www.contoso.com") },
                IsHttpAllowed = true,
                IsHttpsAllowed = true,
                Location = resourceLocation
            };
        cdn.Endpoints.Create(endpointName, endpointParms, profileName, resourceGroupName);
    }
}
```

>[AZURE.NOTE] Im obigen Beispiel weist dem Endpunkt Ursprung namens *Contoso* mit einem Hostnamen `www.contoso.com`.  Sie sollten dies auf eigene Ursprung Hostname ändern.

## <a name="purge-an-endpoint"></a>Löschen Sie einen Endpunkt

Angenommen, der Endpunkt erstellt wurde, ist eine allgemeine Aufgabe, die wir in unserem Programm ausführen möchten unseren Endpunkt Inhalt entfernt.

```csharp
private static void PromptPurgeCdnEndpoint(CdnManagementClient cdn)
{
    if (PromptUser(String.Format("Purge CDN endpoint {0}?", endpointName)))
    {
        Console.WriteLine("Purging endpoint. Please wait...");
        cdn.Endpoints.PurgeContent(endpointName, profileName, resourceGroupName, new List<string>() { "/*" });
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

>[AZURE.NOTE] Im Beispiel oben die Zeichenfolge `/*` gibt an, dass ich alles in den Stamm des Pfads Endpunkt löschen möchten.  Dies entspricht der Azure-Portal "löschen" Dialogfeld Einchecken **Alle löschen** . In der `CreateCdnProfile` Methode habe das Profil als **Azure CDN von Verizon** Profil mit den `Sku = new Sku(SkuName.StandardVerizon)`, so dass dies erfolgreich sein wird.  Jedoch **Azure CDN von Akamai** Profile unterstützen nicht **Alle bereinigen**, damit ich eine Profil Akamai für dieses Lernprogramm verwendet wurde, bestimmte Pfade zu löschen muss würden.

## <a name="delete-cdn-profiles-and-endpoints"></a>Löschen von CDN Profile und Endpunkte

Die letzten Methoden werden unseren Endpunkt Profil gelöscht.

```csharp
private static void PromptDeleteCdnEndpoint(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN endpoint {0} on profile {1}?", endpointName, profileName)))
    {
        Console.WriteLine("Deleting endpoint. Please wait...");
        cdn.Endpoints.DeleteIfExists(endpointName, profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}

private static void PromptDeleteCdnProfile(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN profile {0}?", profileName)))
    {
        Console.WriteLine("Deleting profile. Please wait...");
        cdn.Profiles.DeleteIfExists(profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

## <a name="running-the-program"></a>Ausführen des Programms

Wir können jetzt kompilieren und führen Sie das Programm durch Klicken auf die Schaltfläche **Start** in Visual Studio.

![Programm ausführen](./media/cdn-app-dev-net/cdn-program-running-1.png)

Erreicht die Anwendung oben aufgefordert, sollen Sie die Ressourcengruppe im Azure-Portal und sehen, dass das Profil erstellt wurde.

![Erfolg!](./media/cdn-app-dev-net/cdn-success.png)

Wir können auf den Rest des Programms ausführen bestätigen.

![Programm abschließen](./media/cdn-app-dev-net/cdn-program-running-2.png)

## <a name="next-steps"></a>Nächste Schritte

Das fertige Projekt aus dieser exemplarischen Vorgehensweise, [Laden Sie das Beispiel](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c)zu sehen.

Zusätzliche Dokumentation Azure CDN Management Bibliothek für .NET finden [auf MSDN](https://msdn.microsoft.com/library/mt657769.aspx)anzeigen

Verwalten Sie Ihre Ressourcen CDN mit [PowerShell](./cdn-manage-powershell.md).


