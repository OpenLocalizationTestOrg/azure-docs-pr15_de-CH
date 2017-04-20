<properties 
    pageTitle="Azure Mobile Engagement - Back-End-integration" 
    description="Verbinden von Azure Mobile Engagement mit SharePoint Backend zu Kampagnen von SharePoint" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-multiple" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

# <a name="azure-mobile-engagement---api-integration"></a>Azure Mobile Engagement - API-integration

Ein automatisiertes marketing-System durchgeführt erstellen und Aktivieren der marketing Kampagnen auch automatisch. Zu diesem Zweck - ermöglicht Azure Mobile Engagement diese APIs sowie mit automatisierten Marketingkampagnen erstellen. 

Normalerweise verwenden Kunden Mobile Engagement-front-End-Schnittstelle ihrer Marketingkampagnen Ankündigungen/Umfragen usw. erstellen. Jedoch die Marketingkampagnen Reifen werden, besteht ein nutzen die Daten im Back-End-Systemen (wie CRM-System oder CMS-System wie SharePoint) gesperrt, sodass eine vollautomatisierte Pipeline erstellt Kampagnen in Mobile Engagement dynamisch basierend auf Daten aus Back-End-Systemen erstellt werden kann. 

![][5]

In diesem Lernprogramm durchläuft Szenario, in dem SharePoint Geschäftsbenutzer füllt eine SharePoint-Liste mit marketing und automatisiert nimmt Elemente aus der Liste und verbindet mit den Mobile Engagement mit verfügbaren REST-APIs zum Erstellen einer Marketingkampagne von SharePoint-Daten. 
 
> [AZURE.IMPORTANT] Im Allgemeinen können Sie dieses Beispiel als Ausgangspunkt für Kenntnisse zwei wichtige Aspekte des Aufrufens der APIs - Authentifizierung sowie die Übergabe von Parametern werden alle Mobile Engagement REST-API aufrufen. 

## <a name="sharepoint-integration"></a>SharePoint-integration
1. Hier sieht die Probe SharePoint-Liste. **Titel**, **Kategorie**, **NotificationTitle**, **Nachricht** und **URL** werden zum Erstellen der Ankündigung. Gibt es eine Spalte namens **IsProcessed** die Beispiel-Automatisierung in Form einer Konsolenprogramm verwendet wird. Führen Sie entweder diese Konsole als eine Azure Webauftrags so programmieren, dass Sie planen können, oder können Sie direkt den SharePoint-Workflow Programm erstellen und Aktivieren der Ankündigung beim Einfügen eines Elements in der SharePoint-Liste. In diesem Beispiel verwenden wir die Console durchläuft die Elemente in der SharePoint-Liste für jede Ankündigung in Azure Mobile Engagement erstellen und das **IsProcessed** -Flag True bei der Erstellung der Ankündigung erfolgreich schließlich markiert.

    ![][1]

2. Dabei wird den Code aus dem Beispiel *Remote Authentication in SharePoint Online mithilfe des Clientobjektmodells* [hier](https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c) Authentifizierung mit der SharePoint-Liste.
 
3. Nach der Authentifizierung wir durchlaufen Sie die Listenelemente zu neu erstellten Elemente (die haben **IsProcessed** = False). 

        static async void CreateCampaignFromSharepoint()
        {
            using (ClientContext clientContext = ClaimClientContext.GetAuthenticatedContext(targetSharepointSite))
            {
                // Using https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c for authentication     
                Web site = clientContext.Web;
                List list = site.Lists.GetByTitle("VideoContent");
                CamlQuery query = new CamlQuery();
                query.ViewXml = "<View/>";
                ListItemCollection items = list.GetItems(query);

                // Load the SharePoint list
                clientContext.Load(list);
                clientContext.Load(items);
                clientContext.ExecuteQuery();

                // Loop through the list to go through all the items which are newly added
                foreach (ListItem item in items)
                    if (item["IsProcessed"].ToString() != "Yes")
                    {
                        string name = item["Title"].ToString();
                        string notificationTitle = item["NotificationTitle"].ToString();
                        string notificationMessage = item["Message"].ToString();
                        string category = item["Category"].ToString();
                        string actionURL = ((FieldUrlValue)item["URL"]).Url;

                        // Send an HTTP request to create AzME campaign
                        int campaignId = CreateAzMECampaign
                            (name, notificationTitle, notificationMessage, category, actionURL).Result;

                        if (campaignId != -1)
                        {
                            // If creating campaign is successful then set this to true
                            item["IsProcessed"] = "Yes";

                            // Now try to activate the campaign also
                            await ActivateAzMECampaign(campaignId);
                        }
                        else
                        {
                            item["IsProcessed"] = "Error";
                        }
                        item.Update();
                    }
                clientContext.ExecuteQuery();
            }
        }

## <a name="mobile-engagement-integration"></a>Mobile Engagement integration
1.  Wenn man ein Element erfordert Verarbeitung - extrahieren wir Angaben zum Erstellen einer Ankündigung aus der Liste und rufen `CreateAzMECampaign` zu erstellen und `ActivateAzMECampaign` zu aktivieren. Dies sind im Wesentlichen um Mobile Engagement Backend REST API-Aufrufe. 

2.  Mobile Engagement REST APIs erfordern eine **Standardauthentifizierung Schema HTTP-Autorisierungsheader** besteht die `ApplicationId` und `ApiKey` von Azure-Portal erhalten. Stellen Sie sicher, dass Sie den Schlüssel vom Abschnitt **API-Schlüssel** verwenden und *nicht* aus dem Abschnitt **Sdk** . 

    ![][2]

        static string CreateAuthZHeader()
        {
            string BasicAuthzHeaderString = "Basic " + EncodeTo64(ApplicationId + ":" + ApiKey);
            return BasicAuthzHeaderString;
        }

        static public string EncodeTo64(string toEncode)
        {
            byte[] toEncodeAsBytes = System.Text.ASCIIEncoding.ASCII.GetBytes(toEncode);
            string returnValue = System.Convert.ToBase64String(toEncodeAsBytes);
            return returnValue;
        }  

3. Für die Erstellung der Ankündigung Typ Kampagne – lesen Sie die [Dokumentation](https://msdn.microsoft.com/library/azure/mt683750.aspx). Sie müssen sicherstellen, dass Sie die Kampagne angeben `kind` *Ankündigung* der [Nutzlast](https://msdn.microsoft.com/library/azure/mt683751.aspx) und als FormUrlEncodedContent übergeben. 

        static async Task<int> CreateAzMECampaign(string campaignName, string notificationTitle, 
            string notificationMessage, string notificationCategory, string actionURL)
        {
            string createURIFragment = "/reach/1/create";

            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());
                
                // Create the payload to send the content
                // Reference -> https://msdn.microsoft.com/library/dn913749.aspx
                string data =
                    @"{""name"":""" + campaignName + @"""," + 
                    @"""type"":""only_notif""," + 
                    @"""deliveryTime"":""any"","" + 
                    @""notificationTitle"":""" + notificationTitle + 
                    @""",""notificationMessage"":""" + notificationMessage + 
                    @""",""actionUrl"":""" + actionURL + @"""}";
                
                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("data", data),
                });

                // Send the POST request
                var response = await client.PostAsync(url + createURIFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                int campaignId = -1;
                if (response.StatusCode.ToString() == "OK")
                {
                    // This is the campaign id
                    campaignId = Convert.ToInt32(responseString);
                    Console.WriteLine("Campaign successfully created with id {0}", campaignId);
                }
                else
                {
                    Console.WriteLine("Campaign creation failed with error - '{0}'", responseString);
                }
                return campaignId;
            }
        }

4. Haben die Ankündigung erstellt wird Folgendes Mobile Engagement-Portal angezeigt (Beachten Sie, dass den Zustand Entwurf und aktiviert = n/v =)

    ![][3]

5. `CreateAzMECampaign`eine Ankündigung Kampagne erstellt und seine Id an den Aufrufer zurückgibt. `ActivateAzMECampaign`erfordert diese Id als Parameter an die Kampagne zu aktivieren. 

        static async Task<bool> ActivateAzMECampaign(int campaignId)
        {
            string activateUriFragment = "/reach/1/activate";
            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());

                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("id", campaignId.ToString()),
                });

                // Send the POST request
                var response = await client.PostAsync(url + activateUriFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                if (response.StatusCode.ToString() == "OK")
                {
                    Console.WriteLine("Campaign successfully activated");
                    return true;
                }
                else
                {
                    Console.WriteLine("Campaign activation failed with error - '{0}'", responseString);
                    return false;
                }
            }
        }

6. Nachdem Sie die Ankündigung aktiviert haben, sehen Sie Folgendes Mobile Engagement-Portal:

    ![][4]

7. Kampagne aktiviert wird, werden alle Geräte, die das Kriterium für die Kampagne sobald Benachrichtigungen sehen. 

8. Beachten Sie auch, dass das Listenelement mit IsProcessed markiert = False erstellte Ankündigung Kampagne auf True festgelegt wurde.  

Dieses Beispiel erstellt eine einfache Ankündigung Kampagne hauptsächlich die erforderlichen Eigenschaften angeben. Sie können dies wie über das Portal Sie mithilfe der Informationen können [hier](https://msdn.microsoft.com/library/azure/mt683751.aspx). 

<!-- Images. -->
[1]: ./media/mobile-engagement-sample-backend-integration-sharepoint/sharepointlist.png
[2]: ./media/mobile-engagement-sample-backend-integration-sharepoint/properties.png
[3]: ./media/mobile-engagement-sample-backend-integration-sharepoint/new-announcement.png
[4]: ./media/mobile-engagement-sample-backend-integration-sharepoint/activate-announcement.png
[5]: ./media/mobile-engagement-sample-backend-integration-sharepoint/diagram.png


 
