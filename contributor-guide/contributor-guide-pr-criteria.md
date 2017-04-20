# <a name="quality-criteria-for-pull-request-review"></a>Qualitätskriterien für Pull-Anforderung überprüfen

Diese Kriterien sollen für Autoren erstellen und Verwalten von technischen Artikeln und Content Pull Anfragen überprüfen Pull-Anforderung Genehmiger. Pull-Anforderung für das [automatische Zusammenführen](contributor-guide-pull-request-etiquette.md#in-a-hurry-submit-prs-that-can-be-accepted-automatically)nicht qualifizieren, wird es von einem Bearbeiter menschlichen Pull-Anforderung überprüft. Pull-Anforderung Prüfer überprüfen, in der Regel nur was neue oder geänderte. Pull-Anforderung Prüfer bewerten ändert sich in eine Pull-Anforderung nach der blockierende und nicht blockierende Qualität Artikel in diesem Artikel aufgeführt.

## <a name="blocking-content-quality-items"></a>Qualität Objekte blockiert

Die Updates in der Pull-Anforderung müssen folgenden Kriterien zusammengeführt werden. Pull-Anforderung Prüfer Ihr Feedback in Pull-Anforderung Kommentare für diesen Artikel und `#hold-off` im Pull-Anforderung, um Feedback zu Ihnen (dem Autor) zurückzukehren.

| Kategorie | Qualität überprüfen Element |
|----------|---------------------|
|Erforderliche Komponenten| Das "Ready-zu-zusammenführen" und "Validierung erfolgreich" Etiketten PR zugewiesen|
|Erforderliche Komponenten| Pull-Anforderung kann nicht von Zusammenführungskonflikten blockiert.|
|REPO-Integrität|    Pull-Anforderung enthält keine offensichtlichen Content Regressionen.|
|REPO-Integrität|    Pull-Anforderung umfasst nicht eingebetteten Repo oder ungewöhnlichen, überflüssigen Dateien.|
|REPO-Integrität |Pull-Anforderung enthält weniger als 100 geänderte Dateien, wenn die PR absichtlich einen Zweig Release vom Master aktualisieren. (Wirklich PR sollte weit weniger als jedoch nach 100 geänderten Dateien nicht GitHub die Unterschiede anzuzeigen).|
|Benennen |Dateinamen für neue Dateien führen Sie die [Datei Benennungsrichtlinien](file-names-and-locations.md).|
|Benennen |Neue Ordner eingeführt Repo folgende [Richtlinien für Ordner](file-names-and-locations.md#folder-names-in-the-repo).|
|Inhalt    |Der Artikel ist ein technisches Dokument und in den richtigen Inhalt. Siehe die [was wo geht Leitfaden](content-channel-guidance.md).|
|Inhalt    |Im technischen Dokument ist für technische Artikel. Siehe die [was wo geht Leitfaden](content-channel-guidance.md).|
|Inhalt    |Der Artikel enthält einen einleitenden Absatz und eine Stelle eines prozedurale oder grundlegende Inhalt. Artikel muss ausreichend, vollständige Inhalt als ein Artikel stehen. Es sollte einen kleinen Teil Informationen. (Ausnahme: ein Thema "Grenzen" ist im Kontext einer großen Artikel, die Grenzen eines Dienstes Liste.)|
|Inhalt| Nummerierte Listen sind Elemente nummeriert sind Listen ungeordnet sollten Elemente mit Aufzählungszeichen. Dies ist eine einfache Verwendbarkeit.|
|Inhalt| Ungewöhnliche oder neue Grafiken, Informationsarchitektur oder Strukturen oder offensichtlich nicht standardmäßige Designs müssen mit Leads PR Prüfer überprüft werden. Mit neues experimentieren Teams müssen ein Problem-Lösung Leinwand oder Plan Dank Experimente verfügen.|
|Website-Design-Funktionalität| Switcher dienen nur für mehrere Versionen desselben Artikels wechseln.|
|Website-Design-Funktionalität| Titel Switchered Artikel enthalten Informationen, die jeder Artikel von Artikeln in den Switchered unterscheidet.|
|Website-Design-Funktionalität| Manuell erstellte sind Inhaltsverzeichnisse nicht zulässig. Artikel muss für die Inhaltsverzeichnis Seite auf H2s zurückgreifen.|
|Website-Design-Funktionalität| Wenn H2 Überschriften vorhanden sind, enthält der Artikel zwei H2 Überschriften. Eine H2-Überschrift erstellt einen einzelnes Artikel Inhaltsverzeichnis. H2 Überschriften müssen vor H3 Überschriften verwendet werden, um sicherzustellen, dass ein Inhaltsverzeichnis erstellt.|
|Abzug| HTML: Quellinhalt HTML auf Blockebene – kleine Inline enthält keinen HTML hochgestellt, tiefgestellt, Sonderzeichen und andere kleine Dinge mit Abzug nicht zulässig ist –. HTML-Tabellen sind nur zulässig, wenn die Tabelle enthält Aufzählung oder nummerierte Listen jedoch normalerweise Angabe muss der Inhalt vereinfacht werden, damit die Quelle in Abzug programmiert werden kann.|
|Abzug   |Benutzerdefinierte Abzug Elemente werden gegebenenfalls verwendet. Beispiel: Notizen werden codiert die AZURE. Hinweis-Erweiterung nicht als Klartext.|
|SEO    |Die "& #124; Microsoft Azure"Site-ID ist erforderlich|
|SEO    |H1-Titel enthält genügend Informationen, um den Inhalt des Artikels zur Unterscheidung von anderen Azure Artikel und Kunden prognostizieren Schlüsselwörter zuordnen beschreiben. Beispielsweise ist "Übersicht" als Titel H1 Fail.|
|Terminologie| Die Verwendung von ARM Akronym, V1 oder V2 als Referenzen die Classic und Ressourcenmanager Bereitstellungsmodelle ist einem blockierenden Element Terminologie.|
|Bilder |Animierte GIFs werden in der Repo nicht akzeptiert.|
|Bilder | Bilder löschen Auflösung sind falsch geschriebene Wörter und enthalten keine privaten Informationen | 
|Staging|Die Artikelvorschau muss auf bereitstellen. Offensichtlich Formatierungsprobleme zulässig nicht: <br><br>-Eine nummerierte Liste oder Aufzählung, die als Absatz angezeigt wird <br> -Code in einem Codeblock teilweise in den Codeblock und teilweise außerhalb <br>-Nummerierten Schritte durch fehlerhafte Einzug falsch nummeriert|

## <a name="non-blocking-content-quality-items"></a>Nicht blockierende content-Qualität

Für diese Artikel bieten Pull-Anforderung Prüfer Feedback und Anleitung für den Autor zu Updates in einer späteren Pull-Anforderung. Dieses Feedback wird jedoch die Entscheidung, nicht blockiert. Autoren sollten innerhalb von 3 Werktagen mit Updates folgen.

| Kategorie | Qualität überprüfen Element |
|----------|---------------------|
|Inhalt|Artikel müssen mit 1-3 relevant und überzeugende Schritte "Nächste Schritte". Kurzer Text einbezogen werden soll Ihnen dabei hilft, den Kunden verstehen, warum die nächsten Schritte relevant sind. (Nur neue Artikel). Beispiel: <https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-install/><br>![](media/contributor-guide-pr-criteria/nextstepsexample.PNG)|
|Inhalt|Rechtschreibung, Grammatik und anderem schreiben - Pull-Anforderung Überprüfer Feedback auf einige kleinere Probleme wie nicht blockierenden Feedback vorsehen. Wenn mehrere editorial Probleme protokollieren Bearbeiter eine Anforderung bearbeiten die Artikel nach der Veröffentlichung bearbeiten|
|Bilder|Bilder verwenden, die richtigen Stil und Farbe und Screenshots das richtige Format Rahmen und Platzhalter verwenden. [Finden Sie die Anleitung Bild](https://github.com/Azure/azure-content/blob/master/contributor-guide/create-images-markdown.md).|
|Bilder|Bilder enthalten Alternativtext. [Finden Sie die Anleitung Bild](https://github.com/Azure/azure-content/blob/master/contributor-guide/create-images-markdown.md).|
|Website-Design-Funktionalität|Die H2 Überschriften im Inhaltsverzeichnis auf der Seite gerendert sollte im Idealfall höchstens 2 Zeilen umbrochen. Mehr Überschriften erschweren Artikel Inhaltsverzeichnis gescannt.|
|Formatierungskonventionen|Alle Titel und Überschriften sind Satz Fall pro Azure Stil.|
|Prozess|Wenn die Pull-Anforderung einfach neu konfiguriert wurden konnte PRmerger Automatisierung profitieren, Feedback Pull-Anforderung Bearbeiter an den Autor zur Verwendung von Zweigen, die Änderung automatisch zusammengeführt werden konnten. Siehe [die PR Etikette](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-pull-request-etiquette.md#in-a-hurry-submit-prs-that-can-be-accepted-automatically).|
|Prozess|Löschen oder Umbenennen ein Artikels stellen Sie sicher, dass das Verfahren. Ziehen Sie Prüfer folgenden Kommentar und Link in einem Kommentar hinzufügen müssen Anfrage:<br><br>*Überprüfen Sie den Prozess in die Mitwirkenden Handbuch zum Löschen von Artikeln folgen: <https://github.com/Azure/azure-content/blob/master/contributor-guide/retire-or-rename-an-article.md> .*|

## <a name="related"></a>Bezug

- [Pull-Anforderung Etikette und best Practices für die Microsoft-Mitarbeiter](contributor-guide-pull-request-etiquette.md)

- [Pull-Anforderung Kommentar Automatisierung](contributor-guide-pull-request-comments.md)
