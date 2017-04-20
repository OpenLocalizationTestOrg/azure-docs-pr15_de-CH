<properties 
   pageTitle="Erstellen von DNS-Zonen und Rekord in Azure DNS mit .NET SDK | Microsoft Azure" 
   description="DNS-Zonen und Datensätze in Azure DNS mit .NET SDK erstellen" 
   services="dns" 
   documentationCenter="na" 
   authors="jtuliani" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="09/19/2016"
   ms.author="jtuliani"/>


# <a name="create-dns-zones-and-record-sets-using-the-net-sdk"></a>Erstellen von DNS-Zonen und Datensätze mit .NET SDK

Sie können Vorgänge zum Erstellen, löschen oder Aktualisieren von DNS-Zonen, Datensätze und Einträge mit DNS SDK .NET DNS-Verwaltung automatisieren. Ein Gesamtprojekt Visual Studio steht [hier.](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True)

## <a name="create-a-service-principal-account"></a>Erstellen eines Haupt-Dienstkontos

Programmzugriff auf Azure-Ressourcen ist in der Regel über ein dediziertes Konto als Ihre eigenen Anmeldeinformationen gewährt. Diese dedizierten Konten heißen "principal" Dienstkonten. Um das Beispielprojekt Azure DNS-SDK verwenden, müssen Sie zuerst ein principal Dienstkonto erstellen und die entsprechenden Berechtigungen zuweisen.

1. Befolgen Sie [Diese Anleitung](../resource-group-authenticate-service-principal.md) zum principal Dienstkonto erstellen (das Beispielprojekt Azure DNS-SDK nimmt Authentifizierung.)

2. Eine Ressourcengruppe erstellen ([sieht wie](../azure-portal/resource-group-portal.md)).

3. Verwenden von Azure RBAC principal Dienstkonto "DNS-Zone" Teilnehmerberechtigungen Ressource gewähren ([sieht wie](../active-directory/role-based-access-control-configure.md).)

4. Wenn Beispielprojekt Azure DNS-SDK verwenden, bearbeiten Sie die Datei "program.cs" wie folgt:
    * Legen Sie die Werte für die TenantId ClientId (auch bekannt als Konto-ID), geheimen Service principal Konto und SubscriptionId wie in Schritt 1 verwendet.
    * Geben Sie in Schritt 2 ausgewählten Ressourcen Gruppennamen ein.
    * Geben Sie eine DNS-Zone Ihrer Wahl.

## <a name="nuget-packages-and-namespace-declarations"></a>NuGet-Pakete und Namespacedeklarationen

Um Azure DNS .NET SDK verwenden, müssen Sie **Azure DNS Management Library** NuGet-Paket und andere erforderliche Azure Pakete installieren.
 
1. Öffnen Sie in **Visual Studio**ein Projekt oder ein neues Projekt. 

2. **Tools** zur **>** **NuGet Paket-Manager** **>** **Lösung NuGet-Pakete verwalten**. 

3. Klicken Sie auf **Durchsuchen**, aktivieren Sie das Kontrollkästchen **Vorabversion enthalten** und geben Sie **Microsoft.Azure.Management.Dns** in das Suchfeld ein.

4. Wählen Sie das Paket, und klicken Sie auf **Installieren** , um Ihre Visual Studio-Projekt hinzufügen.
 
5. Wiederholen Sie den Vorgang über die folgenden Pakete installieren: **Microsoft.Rest.ClientRuntime.Azure.Authentication** und **Microsoft.Azure.Management.ResourceManager**.

## <a name="add-namespace-declarations"></a>Namespacedeklarationen hinzuzufügen

Fügen Sie die folgenden Namespacedeklarationen

    using Microsoft.Rest.Azure.Authentication;
    using Microsoft.Azure.Management.Dns;
    using Microsoft.Azure.Management.Dns.Models;

## <a name="initialize-the-dns-management-client"></a>Initialisieren des DNS-Management-Clients

*DnsManagementClient* enthält die Methoden und Eigenschaften zum Verwalten von DNS-Zonen und Recordsets erforderlich.  Der folgende Code principal Dienstkonto anmelden und erstellt ein DnsManagementClient-Objekt.

    // Build the service credentials and DNS management client
    var serviceCreds = await ApplicationTokenProvider.LoginSilentAsync(tenantId, clientId, secret);
    var dnsClient = new DnsManagementClient(serviceCreds);
    dnsClient.SubscriptionId = subscriptionId;

## <a name="create-or-update-a-dns-zone"></a>Erstellen oder Aktualisieren einer DNS-zone

Erstellen eine DNS-Zone ist zuerst ein Objekt "Zone" erstellt, um die DNS-Zone Parameter. Da DNS-Zonen nicht mit einer bestimmten Region verknüpft sind, ist der Speicherort auf "global" festgelegt. In diesem Beispiel wird ein [Azure-Ressourcen-Manager "Tag"](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) auch zur Zone hinzugefügt.

Tatsächlich erstellen oder Aktualisieren der Zone in Azure DNS, wird mit dem Zonenparameter Zonenobjekt der *DnsManagementClient.Zones.CreateOrUpdateAsyc* -Methode übergeben.

>[AZURE.NOTE] DnsManagementClient unterstützt drei Betriebsmodi: synchrone ('CreateOrUpdate'), asynchronous ('CreateOrUpdateAsync'), oder mit den asynchronen Zugriff auf den HTTP-Antwort ('CreateOrUpdateWithHttpMessagesAsync').  Sie können diese Modi je nach Bedarf der Anwendung.

Azure DNS unterstützt vollständige Parallelität [Etags](dns-getstarted-create-dnszone.md)aufgerufen. In diesem Beispiel angeben "*" für den "If-None-Match" Header weist Azure DNS eine DNS-Zone erstellen, wenn noch nicht vorhanden ist.  Der Aufruf schlägt fehl, wenn eine Zone mit dem angegebenen Namen in der angegebenen Ressourcengruppe bereits vorhanden ist.

    // Create zone parameters
    var dnsZoneParams = new Zone("global"); // All DNS zones must have location = "global"
    
    // Create a Azure Resource Manager 'tag'.  This is optional.  You can add multiple tags
    dnsZoneParams.Tags = new Dictionary<string, string>();
    dnsZoneParams.Tags.Add("dept", "finance");
    
    // Create the actual zone.
    // Note: Uses 'If-None-Match *' ETAG check, so will fail if the zone exists already.
    // Note: For non-async usage, call dnsClient.Zones.CreateOrUpdate(resourceGroupName, zoneName, dnsZoneParams, null, "*")
    // Note: For getting the http response, call dnsClient.Zones.CreateOrUpdateWithHttpMessagesAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*")
    var dnsZone = await dnsClient.Zones.CreateOrUpdateAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*");

## <a name="create-dns-record-sets-and-records"></a>Erstellen Sie Datensätze und DNS-Datensätze

DNS-Datensätze werden als eine Datensatzgruppe verwaltet. Eine Datensatzgruppe ist eine Gruppe von Datensätzen mit demselben Namen Datensatztyp innerhalb einer Zone.  Datensatz ist relativ Zonenname nicht den vollqualifizierten DNS-Namen.

Zum Erstellen oder Aktualisieren eines Datensatzes festlegen, ein "RecordSet-Objekt erstellt und an *DnsManagementClient.RecordSets.CreateOrUpdateAsync*übergeben. DNS-Zonen gibt es drei Betriebsmodi: synchrone ('CreateOrUpdate'), asynchronous ('CreateOrUpdateAsync'), oder mit den asynchronen Zugriff auf den HTTP-Antwort ('CreateOrUpdateWithHttpMessagesAsync').

Mit DNS-Zonen unterstützen auf Datensätze Parallelität.  In diesem Beispiel da 'If-Match' weder "If-None-Match" angegeben sind, Recordsets stets entsteht.  Dieser Aufruf überschreibt alle vorhandenen Datensatz mit den gleichen Namen Datensatztyp diese DNS-Zone.

    // Create record set parameters
    var recordSetParams = new RecordSet();
    recordSetParams.TTL = 3600;

    // Add records to the record set parameter object.  In this case, we'll add a record of type 'A'
    recordSetParams.ARecords = new List<ARecord>();
    recordSetParams.ARecords.Add(new ARecord("1.2.3.4"));

    // Add metadata to the record set.  Similar to Azure Resource Manager tags, this is optional and you can add multiple metadata name/value pairs
    recordSetParams.Metadata = new Dictionary<string, string>();
    recordSetParams.Metadata.Add("user", "Mary");

    // Create the actual record set in Azure DNS
    // Note: no ETAG checks specified, will overwrite existing record set if one exists
    var recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSetParams);

## <a name="get-zones-and-record-sets"></a>Zonen abrufen und Datensätze

Die Methoden *DnsManagementClient.Zones.Get* und *DnsManagementClient.RecordSets.Get* Abrufen einzelner Zonen und Datensätze. RecordSets identifizierten Typ, Name und die Zone und Ressource Gruppe, der Sie vorhanden sind. Zonen werden durch den Namen und die Ressourcengruppe existieren in identifiziert.

    var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);
    
## <a name="update-an-existing-record-set"></a>Einen vorhandenen Datensatz aktualisieren

Zum Aktualisieren einer vorhandenen DNS-Datensatz, rufen die Datensätze zunächst Datensatz aktualisieren anschließend die Änderung.  In diesem Beispiel geben wir das Etag "If-Match"-Parameter aus den abgerufenen Datensatz. Der Aufruf schlägt fehl, wenn ein gleichzeitiger Vorgang den Eintrag in der Zwischenzeit geändert hat.

    var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);

    // Add a new record to the local object.  Note that records in a record set must be unique/distinct
    recordSet.ARecords.Add(new ARecord("5.6.7.8"));

    // Update the record set in Azure DNS
    // Note: ETAG check specified, update will be rejected if the record set has changed in the meantime
    recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSet, recordSet.Etag);

## <a name="list-zones-and-record-sets"></a>Liste Zonen und Datensätze

Liste Zonen, verwenden Sie die *DnsManagementClient.Zones.List...* -Methoden unterstützen Auflisten aller Zonen in einer bestimmten Ressourcengruppe oder alle Zonen in einem bestimmten Azure-Abonnement (über Ressourcengruppen.) Datensätze auflisten, *DnsManagementClient.RecordSets.List...* -Methoden unterstützen Auflisten aller Datensätze in einer bestimmten Zone oder nur die Datensätze eines bestimmten Typs verwenden.

Beachten Sie beim Zonen und Datensätze führt möglicherweise paginiert.  Im folgenden Beispiel wird veranschaulicht, wie Seiten Ergebnisse durchlaufen. (Ein künstlich kleine Größe von "2" ist Paging erzwingen, in der Praxis sollte dieser Parameter weggelassen werden und die Standardseitengröße verwendet.)

    // Note: in this demo, we'll use a very small page size (2 record sets) to demonstrate paging
    // In practice, to improve performance you would use a large page size or just use the system default
    int recordSets = 0;
    var page = await dnsClient.RecordSets.ListAllInResourceGroupAsync(resourceGroupName, zoneName, "2");
    recordSets += page.Count();

    while (page.NextPageLink != null)
    {
        page = await dnsClient.RecordSets.ListAllInResourceGroupNextAsync(page.NextPageLink);
        recordSets += page.Count();
    }

## <a name="next-steps"></a>Nächste Schritte

Herunterladen Sie das [Beispielprojekt Azure DNS .NET SDK](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True)Weitere Beispiele zur Verwendung sowie Beispiele für den DNS-Eintragstypen Azure DNS .NET SDK
