<properties
    pageTitle="Ein Azure-Suchindex mithilfe der Azure-Portal erstellen | Microsoft Azure | Gehostete Cloud-Suchdienst"
    description="Erstellen eines Indexes mithilfe der Azure-Portal."
    services="search"
    manager="jhubbard"
    authors="ashmaka"
    documentationCenter=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-index-using-the-azure-portal"></a>Erstellen Sie ein Azure-Suchindex mithilfe der Azure-Portal
> [AZURE.SELECTOR]
- [Übersicht](search-what-is-an-index.md)
- [Portal](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [REST](search-create-index-rest-api.md)

Dieser Artikel führt Sie durch einen Azure Search- [Index](search-what-is-an-index.md) mit Azure-Portal erstellen.

Diese Anleitung und Erstellen eines Indexes sollten Sie bereits [erstellt einen Azure-Suchdienst](search-create-service-portal.md)verfügen.


## <a name="i-go-to-your-azure-search-blade"></a>. Gehen Sie zu Ihrem Azure Search-blade
1. Klicken Sie auf "Alle Ressourcen" im Menü auf der linken Seite der [Azure-Portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices)
2. Wählen Sie Ihre Azure-Suchdienst

## <a name="ii-add-and-name-your-index"></a>II. Fügen Sie hinzu und benennen Sie den index
1. Klicken Sie auf die Schaltfläche "Index hinzufügen"
2. Nennen Sie Suchindex Azure. Da wir eines Indexes erstellen zu suchenden Hotels in diesem Handbuch haben wir unserem Index "Hotels" genannt.
  * Der Indexname muss mit einem Buchstaben beginnen und enthalten nur Kleinbuchstaben, Ziffern und Bindestriche ("-").
  * Ähnlich den Dienstnamen Indexname holen Sie auch werden die Endpunkt-URL, HTTP-Anfragen für Azure Such-API senden Sie
3. Klicken Sie auf die neuen Blade öffnen "Felder"

![](./media/search-create-index-portal/add-index.png)


## <a name="iii-create-and-define-the-fields-of-your-index"></a>III. Erstellen Sie und definieren Sie die Felder des Indexes
1. Auswählen den Eintrag "Felder", wird kein neuer Blade mit einem Formular Geben Sie die Definition des Indexes geöffnet.
2. Der Index mit dem Formular fügen Sie Felder hinzu.

  * *Ein Schlüsselfeld vom Typ Edm.String* ist erforderlich für jede Azure-Suchindex. Dieses Feld wird standardmäßig mit dem Feld "Id" erstellt. Wir haben es in unserem Index in "HotelId" geändert.
  * Bestimmte Eigenschaften des Schemas Index können nur einmal festgelegt werden und können später aktualisiert werden. Aus diesem Grund werden schemaaktualisierungen, die Indizierung wie Feldtypen ändern erfordert nicht derzeit nach der Erstkonfiguration
  * Wir haben sorgfältig die Eigenschaftenwerte für jedes Feld basierend auf wie wir glauben, dass sie in einer Anwendung verwendet werden. Dabei Ihre Erfahrung und Bedürfnisse suchen Benutzer bei Index als jedes Feld die [entsprechenden Eigenschaften](https://msdn.microsoft.com/library/azure/dn798941.aspx)zugewiesen werden muss. Diese Eigenschaften steuern die Funktionen (filtern, facettierung, Sortierung, Volltextsuche, etc.) zu suchen, welche Felder anwenden. Beispielsweise ist es wahrscheinlich, dass Menschen Hotels interessiert Schlüsselwort entspricht, im Feld "Beschreibung" Wir Volltextsuche für dieses Feld aktivieren, indem Sie die Eigenschaft "Durchsuchbar".
    * [Sprache Analyzer](https://msdn.microsoft.com/en-us/library/azure/dn879793.aspx) für jedes Feld lassen auf der Registerkarte "Analyzer" am oberen Rand der Blade. Unten finden Sie, dass wir in unserem Index zur französischen französischen Analyzer für ein Feld ausgewählt haben.

3. **Klicken Sie auf "Felder" Blade die Felddefinitionen bestätigen**
4. Klicken Sie auf die "Index hinzufügen" zu speichern, erstellen Sie den soeben definierten Index auf **OK** .

Im folgenden Screenshots sehen Sie, wie wir benannt und definiert die Felder für Index "Hotels".

![](./media/search-create-index-portal/field-definitions.png)

![](./media/search-create-index-portal/set-analyzer.png)

## <a name="next"></a>Weiter
Nach dem Erstellen einer Azure Suchindex, werden Sie zum [Hochladen in den Index](search-what-is-data-import.md) so können Sie Ihre Daten suchen.
