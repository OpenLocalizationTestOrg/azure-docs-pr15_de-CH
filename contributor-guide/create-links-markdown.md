<properties
   pageTitle="Erstellen von Links in Abzug Artikel" description="Querverbindungen in Abzug code erläutert." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="carolz" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="02/03/2015" ms.author="tysonn" />

# <a name="linking-guidance-for-azure-technical-content"></a>Hinweise zur Azure technischen Inhalt verknüpfen

### <a name="links-from-one-acom-article-to-another"></a>Links von ACOM Artikel zu einem anderen

Um ein Inline-Link aus einem ACOM technische Artikel zu einem anderen ACOM technische Artikel erstellen, verwenden Sie folgenden Link Syntax:  

- Ein Artikel in einer Service Verzeichnis Links zu einem anderen Artikel im gleichen Verzeichnis:

  `[link text](article-name.md)`

- Ein Artikel Links von einem Unterverzeichnis Service zu einem Artikel im Root-Verzeichnis:

  `[link text](../article-name.md)`

- Ein Artikel im Stamm Verzeichnis Links auf einen Artikel in einem Unterverzeichnis Service: 

  `[link text](./service-directory/article-name.md)`

- Ein Artikel in ein Unterverzeichnis Service Links auf einen Artikel in einem anderen Unterverzeichnis Service:

  `[link text](../service-directory/article-name.md)`
 

## <a name="links-to-anchors"></a>Links zu Ankern

Sie keinen Anker erstellen - sie sind zur Zeit für alle H2 Überschriften veröffentlichen automatisch generiert. Müssen Sie lediglich Links zu den Abschnitten H2 erstellen.

- Verknüpfen mit einer Überschrift innerhalb desselben Artikels:

  `[link](#the-text-of-the-H2-section-separated-by-hyphens)`  
  `[Create cache](#create-cache)`

- Verbindung zu einem Anker in einem anderen Artikel im gleichen Unterverzeichnis:

  `[link text](article-name.md#anchor-name)`
  `[Configure your profile](media-services-create-account.md#configure-your-profile)`

- Verbindung zu einem Anker in einem anderen Unterverzeichnis Service:

  `[link text](../service-directory/article-name.md#anchor-name)`
  `[Configure your profile](../service-directory/media-services-create-account.md#configure-your-profile)`

Eine Möglichkeit zum Erstellen von Links in Ihre Artikel automatisch generierter ankerlinks automatisieren ist [MarkdownAnchorLinkGenerator - Tool ankerlinks für ACOM im richtigen Format generieren](https://github.com/Azure/Azure-CSI-Content-Tools/tree/master/Tools/ACOMMarkdownAnchorLinkGenerator).

## <a name="links-from-includes"></a>Enthält Links aus

Da gehören Dateien befinden sich in einem anderen Verzeichnis, müssen mehr relative Pfade verwenden, wie unten dargestellt. Um einen Artikel in einer Include-Datei verknüpfen, verwenden Sie Folgendes Format:

    [link text](../articles/service-folder/article-name.md)
    
Weitere Informationen zum verwenden eine Includedatei [benutzerdefinierte Abzug Extensions Richtlinien](custom-markdown-extensions.md#includes).

## <a name="links-in-selectors"></a>Links im Selektoren

Haben Sie Selektoren, die Include eingebettet sind, verwenden Sie diese Art von Verknüpfung: 

    > [AZURE. Liste Auswahl (Dropdown1 | Dropdown2)]     -  [(Text1 | Beispiel1)](../articles/service-folder/article-name1.md)
    - [(Text1 | Example2)](../articles/service-folder/article-name2.md)
    - [(Text2 | Beispiel3)](../articles/service-folder/article-name3.md)
    - [(Text2 | Beispiel4)](../articles/service-folder/article-name4.md)


## <a name="reference-style-links"></a>Referenz-Hyperlinks

Verweis Hyperlinks können Sie um Ihr Quellinhalt übersichtlicher zu gestalten. Formatvorlage Verweise ersetzen Link Inlinesyntax durch vereinfachte Syntax, die Sie am Ende des Artikels lange URLs wechseln kann. Hier ist Wagen Feuerball Beispiel:

Inline-Text:

    I get 10 times more traffic from [Google][1] than from [Yahoo][2] or [MSN][3].

Link am Ende dieses Artikels:

    <!--Reference links in article-->
    [1]: http://google.com/
    [2]: http://search.yahoo.com/  
    [3]: http://search.msn.com/

Stellen Sie sicher, dass Leerzeichen nach dem Doppelpunkt vor der Verknüpfung. Wenn Sie Leerzeichen sollten zu anderen technischen Artikeln verknüpfen, wird die Verknüpfung in den publizierten Artikel fehlerhaft. 

## <a name="link-to-acom-pages-that-are-not-part-of-the-technical-documentation-set"></a>Link zu ACOM Seiten, die nicht Teil der Dokumentation

Link zu einer Seite ACOM (oder eine Preisliste, SLA Seite alles, was nicht Dokumentation Artikel ist), eine absolute URL, aber das Gebietsschema weglassen. Das Ziel ist, dass Links Arbeit in GitHub und auf der gerenderten Seite:

    [link text](http://azure.microsoft.com/pricing/details/virtual-machines/)


## <a name="link-to-msdn-or-technet"></a>Link zu MSDN oder TechNet

Bei MSDN oder TechNet verknüpft werden sollen, vollständig zum Thema und entfernen die En-us Gebietsschema über den Link. 

### <a name="use-friendly-link-text-for-all-links"></a>Verwenden Sie für alle Links angezeigte Linktext

Die Wörter in eine Verknüpfung aufnehmen angezeigten - das heißt, sie sollte normale Wörter oder den Titel der Seite, der Sie verknüpfen. Verwenden Sie nicht "hier klicken". Es ist für SEO und Ziel nicht angemessen beschreiben.

**Richtig:**

- `For more information, see the [contributor guide index](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).`

- `For more details, see the [SET TRANSACTION ISOLATION LEVEL](https://msdn.microsoft.com/library/ms173763.aspx) reference.`

**Falsch:**

- `For more details, see [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).`

- `For more information, click [here](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).`


## <a name="fwlinks"></a>FWLinks

Vermeiden Sie FWLinks (unser Umleitungssystem) azure.microsoft.com Inhalt. Sie sollte nur als letztes Mittel verwendet werden, wenn Sie einen Link zu einer Seite erstellen, dessen URL Sie noch nicht wissen müssen. Sie werden fast nie. Für ACOM definieren Sie den Dateinamen, damit Sie wissen, was es rechtzeitig werden. Für eine noch nicht veröffentlicht Thema Library können Sie eine Verknüpfung erstellen, die das Thema GUID verwendet, damit Sie nicht mit einem FWLink.

Wenn Sie auf einer Webseite ein FWLink verwenden müssen, nehmen Sie den Parameter P dafür eine permanente Umleitung:

    http://go.microsoft.com/fwlink/p/?LinkId=389595

Beim Einfügen der Ziel-URLs in das Tool FWLink sollten Sie das Gebietsschema entfernen, wenn Ihr ACOM, oder MSDN oder TechNet Library ist.

## <a name="remember-the-azure-library-chrome"></a>Beachten Sie das Chrom Azure Bibliothek!
Möchten Sie ein Thema Azure Bibliothek verknüpfen, die unter [diesem Knoten](https://msdn.microsoft.com/library/azure)befindet, erinnern an den Azure Chrome Link (Azure /). Azure Chrom teilt die Navigationsoptionen ACOM und zeigt nur den Azure Inhalt der MSDN Library. Ordnungsgemäß bewertete Verknüpfung sieht folgendermaßen aus:

    http://msdn.microsoft.com/library/azure/dd163896.aspx

Andernfalls wird die Seite in der Standardansicht von MSDN mit der gesamte MSDN-Struktur angezeigt gerendert werden.

### <a name="contributors-guide-links"></a>Mitwirkenden Guide Links

- [Übersichtsartikel](./../README.md)
- [Index der Artikel](./contributor-guide-index.md)

<!--image references-->
[1]: ./media/create-tables-markdown/table-markdown.png
[2]: ./media/create-tables-markdown/break-tables.png
