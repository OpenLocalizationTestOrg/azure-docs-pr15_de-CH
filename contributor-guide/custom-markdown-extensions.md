<properties
    title="required"
    pageTitle="Benutzerdefinierte Abzug Nebenstellen in unsere technischen Artikeln"
    description="Listet benutzerdefinierte Abzug Extensions, die eingebettete Videos, Hinweise und Tipps wieder verwendbaren Inhalt und andere Artikel in technischen Artikeln azure.microsoft.com ermöglichen."
    services=""
    solutions=""
    documentationCenter=""
    authors="tysonn"
    manager="carolz"
    editor=""/>

<tags
    ms.service="contributor-guide"
    ms.devlang=""
    ms.topic="article"
    ms.tgt_pltfrm=""
    ms.workload=""
    ms.date="01/22/2015"
    ms.author="tysonn"/>

## <a name="markdown-for-azuremicrosoftcom"></a>Abzug für Azure.microsoft.com

Allgemeine Abzug Tipps finden Sie unter [Abzug Grundlagen](https://help.github.com/articles/markdown-basics/) und unsere [Abzug Textgestaltung](./media/documents/markdown-cheatsheet.pdf?raw=true). Artikel Querverbindungen in Abzug zu erstellen, finden Sie unter [Link Anleitung] (. / create-links-markdown.md#markdown-syntax-for-acom-relative-links.md/).

Azure.Microsoft.com unterstützt [eingezäunt Codeblöcken](https://help.github.com/articles/github-flavored-markdown/#fenced-code-blocks) und [Syntax-Hervorhebung](https://help.github.com/articles/github-flavored-markdown/#syntax-highlighting). ACOM unterstützt nur eine Syntax Hervorhebung Farbschema unabhängig von der Sprache in einem Codeblock angeben.

## <a name="custom-markdown-extensions-used-in-our-technical-articles"></a>Benutzerdefinierte Abzug Nebenstellen in unsere technischen Artikeln

Unsere GitHub bezogene Abzug verwendet für die meisten Artikel Formatierung - Absätze, links, Überschriften usw. enthält. Aber wir verwenden benutzerdefinierte Abzug Extensions sollten wir umfangreichere Formatierung in den gerenderten Seiten azure.microsoft.com. Hier ist die derzeit verwendeten Erweiterungen:

+ [Hinweise und Tipps]
+ [Enthält]
+ [Eingebettete videos]
+ [Technologie und Plattform-Selektoren]

## <a name="notes-and-tips"></a>Hinweise und Tipps

Sie können aus 4 Hinweise und Tipps:

- AZURE. HINWEIS
- AZURE. WARNUNG
- AZURE. TIPss
- AZURE. WICHTIG

###<a name="usage"></a>Verwendung
Im Allgemeinen verwenden Sie Hinweise und Tipps sparsam in Artikel. Wenn Sie sie verwenden, wählen Sie die entsprechende Notiz oder Tipp:

- Verwenden Sie AZURE. Hinweis Informationen zu markieren, der betont oder wichtige Punkte des Haupttexts ergänzt. Eine Notiz liefert Informationen nur in besonderen Fällen.

  ![](./media/custom-markdown-extensions/Notes-note.PNG)

- Verwenden Sie AZURE. Warnung benachrichtigt Benutzer eine Bedingung, die in Zukunft zu Problemen führen kann. Beispielsweise kann eine bestimmte Option auswählen oder eine bestimmte Auswahl dauerhaft Sie in einem bestimmten Szenario sperren.

  ![](./media/custom-markdown-extensions/Notes-warning.PNG)

- Verwenden Sie AZURE. Tipp Sie können die Benutzer die Techniken und auf ihre speziellen Bedürfnisse beschriebenen Verfahren anwenden. Tipp möglicherweise auch Alternativen vorschlagen, die möglicherweise nicht offensichtlich. Tipps sind jedoch nicht unbedingt Grundkenntnisse des Texts.

  ![](./media/custom-markdown-extensions/Notes-tip.PNG)

- Verwenden Sie AZURE. WICHTIGE Informationen, die für den Abschluss der Aufgabe.

  ![](./media/custom-markdown-extensions/Notes-important.PNG)

Diese Hinweise und Tipps Codeblöcke, Bilder, Listen und Links unterstützen, versuchen Sie, Ihre Hinweise und Tipps einfach und unkompliziert. Wenn Sie sich komplexe Notizen mit der Formatierung erstellen, könnte sein Zeichen benötigen Sie nur ein weiterer Abschnitt in den Haupttext des Artikels. Und viele Notizen in einem Artikel ist störend und schwer zu scannen oder zu lesen.

###<a name="sample-markdown"></a>Beispiel-Abzug

Alle Beispiele zeigen eine AZURE. HINWEIS. Um einen Tipp, Warnung oder wichtig verwenden, ersetzen Sie "NOTE" in der Abzug:

    > [AZURE.TIP]

    > [AZURE.WARNING]

    > [AZURE.IMPORTANT]

Absatz:

    > [AZURE.NOTE] Um dieses Lernprogramm benötigen Sie ein aktives Microsoft Azure-Konto. Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen.

Multiparagraph:

    > [AZURE.NOTE] Um dieses Lernprogramm benötigen Sie ein aktives Microsoft Azure-Konto.
    >
    > Wenn Sie ein Konto haben, können Sie den [ein kostenloses Testabo erstellen](http://www.windowsazure.com/pricing/free-trial/) , in wenigen Minuten.

## <a name="includes"></a>Enthält

Wieder verwendbare Text in unserem GitHub-Repository befindet sich in Dateien nennen wir "enthält". Bei Text, der in mehreren Artikeln verwendet werden enthalten einen Verweis auf diese Datei wieder verwendbaren Informationen. Include selbst ist eine einfache Abzug (.md). Es kann gültige Abzug, einschließlich Text, Hyperlinks und Bilder enthalten. Alle Dateien in müssen Abzug enthalten [die / Verzeichnis enthält](https://github.com/Azure/azure-content/tree/master/includes) im Stammverzeichnis des Repository. Wenn Artikel veröffentlicht wird, wird Text einschließen veröffentlichten Thema nahtlos integriert.

- Wir verwenden eine spezielle Syntax auf Include.

- Mediendateien in Include setzen müssen in einem Medienordner für die erstellt werden. Media-Ordner für [Azure-Inhalt/enthält/Medienordner](https://github.com/Azure/azure-content/tree/master/includes/media)gehören. Media-Verzeichnis sollte keine Bilder in der Wurzel. Wenn die Bilder nicht haben, ist ein entsprechendes Verzeichnis Media nicht erforderlich.

###<a name="usage"></a>Verwendung

- Verwendung enthält überall den gleichen Text in mehreren Artikeln angezeigt werden.

- Enthält viel Inhalt - Absatz oder zwei, eine freigegebene Prozedur oder einem freigegebenen Abschnitt verwendet werden sollen. Verwenden sie nicht für etwas weniger als ein Satz. **sie sind nicht nach Produktnamen**.

- Stellen Sie sicher alle Text in Include geschrieben vollständige Sätze oder Sätze, die auf vorhergehenden oder folgenden Text im Artikel, der die verweist nicht abhängen. Ignoriert dieses Handbuchs eine unübersetzbare Zeichenfolge im Artikel, der der Lokalisierung wird erstellt. 

- Nicht einbetten in anderen enthält enthält. Sie werden von DPS publishing-System nicht unterstützt.

- Freigeben Sie keine Medien zwischen Dateien. Verwenden Sie eine separate Datei mit einem eindeutigen Namen für jede einschließen und Artikel. Speichern der Mediendatei im Ordner Media einschließen zugeordnet.

- Verwenden Sie Include als nur der Inhalt eines Artikels.  Enthält zusätzliche Inhalte im restlichen Artikel werden sollen.

- Da alle muß die / Verzeichnis enthält immer der Pfad zur Include aus einem Artikel

    .. / enthält

- Ein Hyperlink oder einem Bild Filename auf den Artikel und die nicht wiederholt. Fügen Sie "-einschließen" auf den Link Verweis oder Medien Dateinamen zu vermeiden des Verweis:

 **Verknüpfung**

 Änderung: odata.org an: odata.org einschließen

 **Verweis**

 Änderung: table.png an: Tabelle include.png

###<a name="sample-markdown"></a>Beispiel-Abzug
Die Syntax für einen Artikel Dokumentation Include hinzufügen ist:

    [AZURE.INCLUDE [include-short-name](../includes/include-file-name.md)]

Beispiel

    [AZURE.INCLUDE [howto-blob-storage](../includes/howto-blob-storage.md)]

Der erste Teil die heißt Include ohne Pfad und Erweiterung .md. Der zweite Teil ist der relative Pfad zum Einschließen in die / Verzeichnis mit der Erweiterung .md enthält.

###<a name="rendering"></a>Wiedergabe

Die wird auf der gerenderten Seite GitHub wie folgt dargestellt:

 [AZURE. ENTHALTEN Sie so wird's gemacht-Blob-Speicher]

Im gerenderten HTML auf azure.microsoft.com HTML aus dem enthält den Rest des Dokuments HTML zusammengeführt. Jedoch enthält HTML-HTML-Kommentar mit dem ursprünglichen Abzug Dateinamen und GitHub Commit Hash enthalten. Dieser Kommentar ist für die Problembehandlung, sodass der Quellinhalt kann problemlos identifiziert und in GitHub enthalten:

  ![](./media/custom-markdown-extensions/include.png)


## <a name="embedded-videos"></a>Eingebettete videos

Unsere technischen Artikeln unterstützen Embeddeded Videos in technischen Artikeln, solange die Videos auf [Channel 9](http://channel9.msdn.com/) Website. Videos von Channel 9 müssen [azure.microsoft.com Video Center](http://azure.microsoft.com/documentation/videos/home/)integriert. Wir unterstützen derzeit keine eingebetteten Videos von YouTube; BIST Community Contributor gerne auf YouTube zu verknüpfen, wenn gewünschte feature Video dort bereitgestellt wird. Microsoft-Mitarbeiter sollten Channel 9 und Video Center verwenden.

### <a name="usage"></a>Verwendung

- Stellen Sie sicher, dass das Video im Video Center.

- Kopieren Sie die video-ID aus der URL des Videos auf Channel 9 oder Azure Video Center. Die video-ID für das Video [http://azure.microsoft.com/documentation/videos/azure-scheduler-unusual-schedules/](http://azure.microsoft.com/documentation/videos/azure-scheduler-unusual-schedules/) ist z. B. **Azure-Scheduler-ungewöhnliche-Zeitpläne**.

### <a name="syntax"></a>Syntax

    > [AZURE.VIDEO video-id-string]

### <a name="rendering"></a>Wiedergabe

Auf GitHub: [https://github.com/Azure/azure-content-pr/blob/master/articles/web-sites-backup.md](https://github.com/Azure/azure-content-pr/blob/master/articles/web-sites-backup.md)

Veröffentlichte Artikel: [http://azure.microsoft.com/documentation/articles/web-sites-backup/](http://azure.microsoft.com/documentation/articles/web-sites-backup/)


## <a name="technology-and-platform-selectors"></a>Technologie und Plattform-Selektoren

Verwenden Sie Technologie und Plattform Switcher in technischen Artikeln beim Erstellen mehrerer Sorten des gleichen Artikels Adresse Unterschiede in der Implementierung Technologies oder Plattformen. Dies ist in der Regel am besten unsere Plattform Inhalte für Entwickler. Derzeit sind zwei Arten von Selektoren, [einfachen Selektoren](#simple-selectors) und [bidirektionale Selektoren](#two-way-selectors).

Die gleiche Auswahl Abzug in jedem Thema der Auswahl geht, empfehlen wir Include Auswahl für Ihr Thema versehen und anschließend, in der Themen, mit denen eine Auswahl einbeziehen.

###<a id="simple-selectors"></a>Einfachen Selektoren

Simple (einfache) Selektoren werden als eine Reihe von Optionsfeldern direkt unterhalb des Titels dargestellt. Verwenden Sie diese Schaltflächen, wenn Kunden müssen Themen in einer einzigen Plattform oder Technologie, wie .NET, Node.js und Java aus.  Verwenden Sie benutzerdefinierte Abzug Erweiterung für alle Selektoren - HTML für Selektoren nicht verwenden.  

Finden Sie [Schritte mit Notification Hubs](http://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started/) zu sehen, wie der Autor 8 Versionen desselben Artikels jedoch verwendeten Selektoren ermöglicht Navigation über alle erstellt.

![Einfache Auswahl-Beispiel](./media/custom-markdown-extensions/selectors.PNG)

####<a name="syntax"></a>Syntax

    > [AZURE.SELECTOR]
    - [Verknüpfen der Bezeichnung #1](link #1 url)
    - [Link Nr. 2 Etikett](link #2 url)

Beispiel:

    > [AZURE.SELECTOR]
    - [Universelle Windows](../articles/notification-hubs-windows-store-dotnet-get-started/)
    - [Windows Phone-](../articles/notification-hubs-windows-phone-get-started/)
    - [iOS](../articles/notification-hubs-ios-get-started/)
    - [Android](../articles/notification-hubs-android-get-started/)
    - [Kindle](../articles/notification-hubs-kindle-get-started/)
    - [Baidu](../articles/notification-hubs-baidu-get-started/)
    - [Xamarin.iOS](../articles/partner-xamarin-notification-hubs-ios-get-started/)
    - [Xamarin.Android](../articles/partner-xamarin-notification-hubs-android-get-started/)

#### <a name="rendering"></a>Wiedergabe

Die Abbildung zeigt das Rendering auf azure.microsoft.com. Auf den gerenderten Seiten GitHub Rendern des Selektors als Aufzählung von Links.

###<a id="two-way-selectors"></a>Bidirektionale Selektoren

Bidirektionale Selektoren ermöglicht Benutzern einer Themen aus einer zwei-Wege-Matrix. Dies ist wichtig, wenn eine Azure-Technologie wie Mobile Dienste mehrere Back-End-Plattformen sowie mehrere Clients unterstützt. Beachten Sie Folgendes:

- Während der so als `(Platform | Backend)`, Dropwdown-Text kann nun angepasst werden.
- Ein Element für jeden Punkt in der Matrix benötigen, aber nur ein Element, eine Thema URL vorhanden ist und keine Kopie, haben.
- Der Link kann eine beliebige URL zwar im Allgemeinen GitHub Thema.

Finden Sie unter [Erste Schritte mit Mobile Dienste](http://azure.microsoft.com/en-us/documentation/articles/mobile-services-ios-get-started/) wie Autor 15 Versionen desselben Artikels (9 mobilen Client-Plattformen und 2 Back-End-Plattformen), sondern ermöglicht Navigation über alle verwendeten Selektoren erstellt. Beachten Sie, dass 3 Artikel sowohl Back-End-Versionen haben.

![Bidirektionale Selektoren Beispiel](./media/custom-markdown-extensions/selector-list.png)

####<a name="syntax"></a>Syntax

    > [AZURE. Liste Auswahl (Dropdown1 | Dropdown2)]     -  [(Dropdown1Text1 | Dropdown2Text1)](../articles/dropdown1-text1-dropdown2-text1.md)
    - [(Dropdown1Text1 | Dropdown2Text2)](../articles/dropdown1-text1-dropdown2-text1.md)
    - [(Dropdown1Text2 | Dropdown2Text3)](../articles/dropdown1-text1-dropdown2-text1.md)
    - [(Dropdown1Text3 | Dropdown2Text4)](../articles/dropdown1-text1-dropdown2-text1.md)

Beispiel:

    > [AZURE. Liste Auswahl (Plattform | Back-End)]     -  [(iOS | .NET)](./mobile-services-dotnet-backend-ios-get-started-push.md)
    - [(iOS | JavaScript)](./mobile-services-javascript-backend-ios-get-started-push.md)
    - [(Windows universal C# | .NET)](./mobile-services-dotnet-backend-windows-universal-dotnet-get-started-push.md)
    - [(Windows universal C# | JavaScript)](./mobile-services-javascript-backend-windows-universal-dotnet-get-started-push.md)
    - [(Windows Phone | .NET)](./mobile-services-dotnet-backend-windows-phone-get-started-push.md)
    - [(Windows Phone | JavaScript)](./mobile-services-javascript-backend-windows-phone-get-started-push.md)
    - [(Android | .NET)](./mobile-services-dotnet-backend-android-get-started-push.md)
    - [(Android | JavaScript)](./mobile-services-javascript-backend-android-get-started-push.md)
    - [(iOS Xamarin | JavaScript)](./partner-xamarin-mobile-services-ios-get-started-push.md)
    - [(Xamarin Android | JavaScript)](./partner-xamarin-mobile-services-android-get-started-push.md)

#### <a name="rendering"></a>Wiedergabe

Die Abbildung zeigt das Rendering auf azure.microsoft.com. Auf den gerenderten Seiten GitHub Rendern des Selektors als Aufzählung von Links.

<!--Anchors-->
[Hinweise und Tipps]: #notes-and-tips
[Enthält]: #includes
[Eingebettete videos]: #embedded-videos
[Technologie und Plattform-Selektoren]: #technology-and-platform-selectors

###<a name="contributors-guide-links"></a>Mitwirkenden Guide Links

- [Übersichtsartikel](./../README.md)
- [Index der Artikel](./contributor-guide-index.md)
