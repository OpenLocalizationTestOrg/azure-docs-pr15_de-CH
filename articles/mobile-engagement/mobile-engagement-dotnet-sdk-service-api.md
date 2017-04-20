<properties 
    pageTitle=".NET SDK verwenden auf Azure Mobile Engagement Service-APIs" 
    description="Beschreibt, wie Mobile Engagement .NET SDK auf Azure Mobile Engagement Service-APIs"        
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-multiple" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="using-net-sdk-to-access-azure-mobile-engagement-service-apis"></a>.NET SDK verwenden auf Azure Mobile Engagement Service-APIs

Azure Mobile Engagement stellt eine Reihe von APIs für Geräte, Reach-Push-Kampagnen usw.. Interaktion mit diesen APIs bieten wir Ihnen auch [stolz Datei](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) mit Anwendungsmöglichkeiten SDKs für Ihre bevorzugte Sprache generieren. Wir empfehlen [AutoRest](https://github.com/Azure/AutoRest) Tool Ihren SDK aus unserer stolz-Datei generiert. 

Haben wir .NET SDK ähnlich der Interaktion mit diesen APIs C#-Wrapper verwenden können, und Sie müssen token Authentifizierungsverhandlungen und selbst aktualisieren.  

In diesem Beispiel durchläuft verschiedene Schritte für das .NET SDK:

1. Zunächst müssen Sie die Authentifizierung für APIs unter Azure Active Directory beschrieben [hier](mobile-engagement-api-authentication.md#authentication)einrichten. Am Ende dieser Schritte benötigen Sie eine gültige **SubscriptionId**, **TenantId** **ApplicationId** und **Schlüssel**. 

2. Wir verwenden eine einfache Windows-Konsole-Anwendung mit .NET SDK Szenario erstellen Sie eine Ankündigung Kampagne veranschaulicht. So öffnen Sie Visual Studio, und erstellen Sie ein **Konsolenanwendungsprojekt**.   

3. Anschließend müssen Sie .NET SDK downloaden als **Microsoft Azure Engagement Management Library** in Nuget-Katalog [hier](https://www.nuget.org/packages/Microsoft.Azure.Management.Engagement/).
Wenn Sie die Nuget von Visual Studio installieren, müssen markiert die Option **Vorabversion einschließen** , bei der Suche nach dem Paket zu erzielen:

    ![][1]

4. In der `Program.cs` Datei, fügen Sie die folgenden Namespaces:

        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.Engagement;
        using Microsoft.Azure.Management.Engagement.Models;

5. Als Nächstes müssen Sie folgenden Konstanten definieren, die wir verwenden für die Authentifizierung und Interaktion mit Mobile Engagement App in der Ankündigung Kampagne erstellen:

        // For authentication
        const string TENANT_ID = "<Your TenantId>";
        const string CLIENT_ID = "<Your Application Id>";
        const string CLIENT_SECRET = "<Your Secret>";
        const string SUBSCRIPTION_ID = "<Your Subscription Id>";

        // This is the Azure Resource group concept for grouping together resources 
        //  see here: https://azure.microsoft.com/en-us/documentation/articles/resource-group-portal/
        const string RESOURCE_GROUP = "";

        // For Mobile Engagement operations
        // App Collection Name 
        const string APP_COLLECTION_NAME = "";
        // Application Resource Name - make sure you are using the one as specified in the Azure portal (NOT the App Name)
        const string APP_RESOURCE_NAME = "";

6. Definieren der `EngagementManagementClient` Variable, die mit dem Mobile Engagement SDK Methoden aufrufen:

        static EngagementManagementClient engagementClient; 

7. Fügen Sie Folgendes zu Ihrem `Main` Methode:

        try
            {
                // Initialize the Engagement SDK to call out APIs. 
                InitEngagementClient().Wait();

                // Create a Reach campaign
                CreateCampaign().Wait();
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.InnerException.Message);
                throw ex;
            }

8. Definieren Sie die folgende Methode die Initialisierung übernimmt die `EngagementManagementClient` durch zunächst authentifiziert und verbinden sich mit Mobile Engagement App Ankündigung Kampagne erstellt werden soll:

        private static async Task InitEngagementClient()
        {
            var credentials = await ApplicationTokenProvider.LoginSilentAsync(TENANT_ID, CLIENT_ID, CLIENT_SECRET);
            engagementClient = new EngagementManagementClient(credentials) { SubscriptionId = SUBSCRIPTION_ID };
            
            // This is the Azure concept of ResourceGroup
            engagementClient.ResourceGroupName = RESOURCE_GROUP;

            // This is your Mobile Engagement App Collection & App Resource Name in which you create the campaign
            engagementClient.AppCollection = APP_COLLECTION_NAME;
            engagementClient.AppName = APP_RESOURCE_NAME;
        }

    > [AZURE.IMPORTANT] Beachten Sie, dass der **Ressourcenname App** verwenden in Azure-Verwaltungsportal AppName Parameter definiert. 

9. Abschließend definieren die CreateCampaign-Methode die kümmern zuvor initialisierten EngagementClient zum Erstellen einer einfachen **jederzeit**mit & **Benachrichtigung nur** mit Titel und Meldung: 

        private async static Task CreateCampaign()
        {
            //  Refer to the Announcement Campaign format from here - 
            //      https://msdn.microsoft.com/en-us/library/azure/mt683751.aspx
            // Make sure you are passing all the non-optional parameters
            Campaign parameters = new Campaign(
                name:"WelcomeCampaign",
                notificationTitle: "Welcome", 
                notificationMessage: "Thank you for installing the app!",
                type:"only_notif",
                deliveryTime:"any"
                );

            // Refer to the Campaign Kinds from here - https://msdn.microsoft.com/en-us/library/azure/mt683742.aspx
            CampaignStateResult result = 
                await engagementClient.Campaigns.CreateAsync(CampaignKinds.Announcements, parameters);
            Console.WriteLine("Campaign Id '{0}' was created successfully and it is in '{1}' state", result.Id, result.State);
        }

10. Konsolenanwendung ausführen und bei der erfolgreichen Erstellung der Kampagne wird Folgendes angezeigt:

    **Kampagnen-Id '159' wurde erfolgreich erstellt, und es befindet sich im Zustand 'Entwurf'**

<!-- Images. -->

[1]: ./media/mobile-engagement-dotnet-sdk-service-api/include-prerelease.png
