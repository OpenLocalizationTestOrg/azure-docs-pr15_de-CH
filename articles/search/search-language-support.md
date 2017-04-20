<properties
   pageTitle="Erstellen Sie einen Index für Dokumente in mehreren Sprachen in Azure suchen | Microsoft Azure | Gehostete Cloud-Suchdienst"
   description=" Azure unterstützt 56 Sprachen Sprache Analyzer von Lucene und Verarbeitung natürlicher Sprache von Microsoft nutzen."
   services="search"
   documentationCenter=""
   authors="yahnoosh"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="na"
   ms.workload="search"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.date="07/14/2016"
   ms.author="jlembicz"/>

# <a name="create-an-index-for-documents-in-multiple-languages-in-azure-search"></a>Erstellen Sie einen Index für Dokumente in mehreren Sprachen in Azure suchen
> [AZURE.SELECTOR]
- [Portal](search-language-support.md)
- [REST](https://msdn.microsoft.com/library/azure/dn879793.aspx)
- [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.analyzername.aspx)

Solche die macht der Sprache Analysatoren ist so einfach wie das Festlegen einer Eigenschaft in einem durchsuchbaren Feld in der Indexdefinition. Jetzt können Sie diesen Schritt im Portal ausführen.

Nachfolgend finden Sie Screenshots der Azure-Portal-Blades für Azure-Suche, bei der Benutzern ein IndexSchema definieren. Benutzer können dieses Blatt alle Felder erstellen und Analyzer-Eigenschaft einzeln festlegen.

> [AZURE.IMPORTANT] Sie können nur einen Sprache Analyzer im Felddefinition in einen neuen Index von Grund auf erstellen oder einen vorhandenen Index ein neues Feld hinzufügen festgelegt. Sicherstellen Sie, dass Sie alle Attribute den Analyzer beim Erstellen des Felds vollständig angeben. Nicht möglich Attribute bearbeiten oder Ändern der Analyzer nach Ihrem speichern.

## <a name="define-a-new-field-definition"></a>Definition eines neuen Felds definieren

1. [Azure-Portal](https://portal.azure.com) anmelden und das Blatt der Suchdienst öffnen.
2. Klicken Sie auf der Befehlsleiste oben Dashboard Service zu einem neuen Index **Index hinzufügen** oder öffnen Sie einen vorhandenen Index, um ein neue Felder, einen vorhandenen Index hinzufügen festlegen.
3. Felder Blatt angezeigt wird, Optionen zum Definieren des Schemas des Index, einschließlich der Registerkarte Analyzer zum Auswählen verwendet Sprache Analyzer.
4. Starten Sie in Feldern eine Felddefinition einen Namen und den Datentyp auswählen Attribute das Feld als vollständiger Text durchsuchbar und abrufbar in Suchergebnissen in Facet Navigationsstrukturen verwendet sortierbaren usw. markiert. 
5. Vor dem nächsten Feld, zur Registerkarte **Analyzer** . 

   
![][1]
*Wählen Sie ein Klicken Sie auf die Registerkarte Analyzer auf die Felder*

## <a name="choose-an-analyzer"></a>Wählen Sie ein

6. Scrollen Sie zu dem Feld, das Sie definieren. 
7. Wenn Sie das Feld als durchsuchbar markiert haben, klicken Sie das Kontrollkästchen jetzt um als **durchsuchbar**markiert.
8. Klicken Sie zum Anzeigen der Liste der verfügbaren Analysen Analyzer.
9. Wählen Sie den Analyzer verwenden möchten.

![][2]
*Wählen Sie eine der unterstützten Analysatoren für jedes Feld*

Standardmäßig verwenden alle durchsuchbaren Felder [Standard Lucene Analyzer](http://lucene.apache.org/core/4_10_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) Sprache unabhängig ist. Die vollständige Liste der unterstützten Analyzer finden Sie unter [Unterstützung von Sprachen in Azure Suche](https://msdn.microsoft.com/library/azure/dn879793.aspx).

Nach Sprache Analyzer für ein Feld ausgewählt ist, wird es bei jeder Anforderung Index- und Suchdienste für dieses Feld verwendet. Bei eine Abfrage mit mehreren Feldern mit verschiedenen Analysatoren wird die Abfrage unabhängig vom rechten Analysatoren für jedes Feld verarbeitet werden.

Viele Web und Dienste dienen Benutzern in unterschiedlichen Sprachen weltweit. Es ist möglich, einen Index für ein Szenario wie folgt definieren, indem Sie ein Feld für jede unterstützte Sprache erstellen.

![][3]
*Indexdefinition mit Beschreibung für jede unterstützte Sprache*

Wenn die Sprache des Agenten Ausgeben einer Abfrage bekannt ist, kann eine Suchabfrage auf ein bestimmtes Feld mit **Suchfeldern** Abfrageparameter beschränkt. Die folgende Abfrage erfolgt nur gegen die Beschreibung Polnisch:

`https://[service name].search.windows.net/indexes/[index name]/docs?search=darmowy&searchFields=description_pl&api-version=2015-02-28`

Den Index im Portal können mit **Search Explorer** ähnelt der obigen Abfrage einfügen abgefragt werden. Suche Explorer steht der Befehlsleiste in dem Blatt. Details finden Sie in der [Abfrage Azure Suchindex im Portal](search-explorer.md) .

Manchmal ist die Sprache des Agents eine Abfrage ausgegeben nicht bekannt die Abfrage gegen alle Felder gleichzeitig ausgestellt werden kann. Bei Bedarf kann Ergebnisse in einer bestimmten Sprache bevorzugt Gewichten [Profile](https://msdn.microsoft.com/library/azure/dn798928.aspx)definiert werden. Im folgenden Beispiel werden Ergebnisse in der Beschreibung in Englisch höher relativ entspricht Polnisch und Französisch erzielt:

    "scoringProfiles": [
      {
        "name": "englishFirst",
        "text": {
          "weights": { "description_en": 2 }
        }
      }
    ]

`https://[service name].search.windows.net/indexes/[index name]/docs?search=Microsoft&scoringProfile=englishFirst&api-version=2015-02-28`

Beachten Sie .NET Entwickler sollten, dass Sprache Analyzer mit [Azure Suche .NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Search)konfigurieren können. Die neueste Version unterstützt auch die Microsoft Language Analyzer.

<!-- Image References -->
[1]: ./media/search-language-support/AnalyzerTab.png
[2]: ./media/search-language-support/SelectAnalyzer.png
[3]: ./media/search-language-support/IndexDefinition.png
