# <a name="azure-technical-documentation-contributor-guide"></a>Azure technische Contributor Guide

Sie haben GitHub Repository gefunden, das die Quelle für technische Dokumentation enthält, die in Azure Dokumentation Center unter [http://azure.microsoft.com/documentation](http://azure.microsoft.com/documentation)veröffentlicht.

Dieses Repository enthält auch dabei helfen, unsere technische Dokumentation beitragen.  Eine Liste der Artikel in die Mitwirkenden Guide finden Sie im [Index](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).

## <a name="contribute-to-azure-documentation"></a>Beitrag zur Azure-Dokumentation

Vielen Dank für Ihr Interesse an Azure-Dokumentation!

* [Beitragen](#ways-to-contribute)
* [Verhaltensregeln](#code-of-conduct)
* [Über Ihre Beiträge zu Azure content](#about-your-contributions-to-azure-content)
* [Repository-Organisation](#repository-organization)
* [Verwenden Sie GitHub, Git und dieses repository](#use-github-git-and-this-repository)
* [Wie Sie Abzug Thema formatieren](#how-to-use-markdown-to-format-your-topic)
* [Feedback, Kommentare und Unterstützung](./contributor-guide/feedback-and-comments.md)
* [Weitere Ressourcen](#more-resources)
* [Index alle Contributors Artikeln](./contributor-guide/contributor-guide-index.md) (öffnet neue Seite)

## <a name="ways-to-contribute"></a>Beitragen 

Sie können auf verschiedene Weise in [Azure Dokumentation](http://azure.microsoft.com/documentation/) beitragen:

* Ein [Diskussionsforum](http://social.msdn.microsoft.com/Forums/windowsazure/home)beitragen.
* Disqus Kommentare in Artikel einreichen.
* Sie können leicht zu technischen Artikeln in der GitHub beitragen. Artikel in diesem Repository suchen, oder besuchen Sie [http://azure.microsoft.com/documentation](http://azure.microsoft.com/documentation) Artikel, und klicken Sie im Artikel GitHub Quelle für Artikel geht.
* Wenn Sie einem vorhandenen Artikel wesentliche Änderung vornehmen, müssen hinzufügen oder Bilder ändern oder einen neuen Artikel beitragen Sie dieses Repository verzweigen, Git Bash Abzug Pad installieren und einige Befehle Git.

##<a name="code-of-conduct"></a>Verhaltensregeln

Dieses Projekt hat [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). Weitere Informationen finden Sie in den [Code führen FAQ](https://opensource.microsoft.com/codeofconduct/faq/) oder Kontakt [opencode@microsoft.com](mailto:opencode@microsoft.com) mit Fragen oder Kommentaren.

##<a name="about-your-contributions-to-azure-content"></a>Über Ihre Beiträge zu Azure content

###<a name="minor-corrections"></a>Kleinere Korrekturen

Kleinere Korrekturen oder Klärung, Dokumentation und Codebeispiele in diesem Repo senden, fallen unter die [Azure Website Geschäftsbedingungen (AGB)](http://azure.microsoft.com/support/legal/website-terms-of-use/).


###<a name="larger-submissions"></a>Größere Übermittlungen

Eine Pull-Anforderung mit neuen oder erhebliche Dokumentation und Codebeispiele vor, senden wir einen Kommentar in GitHub werden Sie aufgefordert, eine Online-Beitrag Lizenz Vereinbarung (CLA) senden, wenn in einer dieser Gruppen sind:

* Mitglieder der Gruppe Microsoft Open Technologies.
* Teilnehmer für Microsoft arbeiten.

Wir benötigen Sie das online-Formular ausfüllen, bevor wir ziehen akzeptieren können.

Ausführliche Informationen finden unter [http://azure.github.io/guidelines/#cla](http://azure.github.io/guidelines/#cla).

## <a name="repository-organization"></a>Repository-Organisation

Der Inhalt in Azure Content Repository folgt der Dokumentation auf [Azure.Microsoft.com](http://azure.microsoft.com). Dieses Repository enthält zwei Root-Ordner:

### <a name="articles"></a>\articles

Der Ordner *\articles* enthält Dokumentation Artikel als Abzug Dateien mit der Erweiterung *.md* formatiert.

Im Pfad Azure.Microsoft.com Artikel im Stammverzeichnis veröffentlicht *http://azure.microsoft.com/documentation/articles/ {Artikel-Name-ohne-Md} /*.

* **Artikel Dateinamen:** [Unsere Dateibenennung Anleitung](./contributor-guide/file-names-and-locations.md)anzeigen

Im Pfad Azure.Microsoft.com Artikel innerhalb ihrer eigenen-Ordner veröffentlicht *http://azure.microsoft.com/documentation/articles/service-folder/ {Artikel-Name-ohne-Md} /*

* **Media Unterordner:** Der *\articles* Ordner enthält den Ordner *\media* Root Directory Artikel Media-Dateien in die Unterordner mit Bildern für jeden Artikel.  Der Dienst enthalten separate Medienordner für Artikel im entsprechenden Service. Artikel Bildordner sind gleichnamigen Datei Artikel minus die Erweiterung *.md* .

### <a name="includes"></a>\Includes

Wieder verwendbaren Inhalt Abschnitte in einen oder mehrere Artikel enthalten sein können. Siehe [Custom Extensions in unsere technischen Inhalte verwendet](./contributor-guide/custom-markdown-extensions.md).

### <a name="markdown-templates"></a>\markdown Vorlagen

Dieser Ordner enthält die standardmäßigen Abzug Vorlagen mit dem grundlegenden Abzug für einen Artikel.

### <a name="contributor-guide"></a>\contributor-Guide

Dieser Ordner enthält Artikel, die Teil unserer Mitarbeiter-Handbuch.  

## <a name="use-github-git-and-this-repository"></a>Verwenden Sie GitHub, Git und dieses repository

Weitere Informationen über GitHub UI mit kleinen Änderungen contribute beitragen und zum Verzweigen und Klonen Repository für wesentlich mehr [Installieren und Tools für das Schreiben in GitHub](./contributor-guide/tools-and-setup.md).

Wenn Sie GitBash zu installieren und zu arbeiten, die Schritte zum Erstellen einer neuen lokalen arbeitsverzweigung, Änderungen und senden die ändert die main-Verzweigung zurück [Git Befehle für einen neuen Artikel erstellen oder aktualisieren einen vorhandenen Artikel](./contributor-guide/git-commands-for-master.md) entnehmen

### <a name="branches"></a>Zweigstellen

Wir empfehlen Sie Zweigstellen arbeiten erstellen einen bestimmten Bereich ändern. Jede Verzweigung sollte auf einem einzelnen Konzept/Artikel Arbeitsablauf optimieren und vermeiden Konflikte beim Zusammenführen.  Folgenden Maßnahmen sind von dem entsprechenden Bereich für einen neuen Zweig:

* Ein neuer Artikel (und zugehörigen Grafiken)
* Rechtschreibung und Grammatik bearbeitet für einen Artikel.
* Eine einzelne Änderung der Formatierung anwenden für eine große Gruppe von Artikeln (z. B. neue copyright Fußzeile).

## <a name="how-to-use-markdown-to-format-your-topic"></a>Wie Sie Abzug Thema formatieren

Alle Artikel in diesem Repository verwenden GitHub bezogene Abzug.  Hier wird eine Liste der Ressourcen.

- [Abzug-Grundlagen](https://help.github.com/articles/markdown-basics/)

- [Druckbare Abzug Spickzettel](./contributor-guide/media/documents/markdown-cheatsheet.pdf?raw=true)

- Unsere Liste der Abzug Editoren finden Sie [Tools und Thema Setup](./contributor-guide/tools-and-setup.md#install-a-markdown-editor).

## <a name="article-metadata"></a>Artikel-Metadaten

Artikel Metadaten können bestimmte Funktionen auf der Website azure.microsoft.com, Autor Zuordnung Contributor Attribution, Breadcrumbs, Artikelbeschreibung und SEO Optimierungen sowie reporting Microsoft verwendet, um die Ertragskraft des Inhalts. Die Metadaten ist wichtig! [Hier Hinweise dafür Metadaten direkt erfolgt](./contributor-guide/article-metadata.md).

### <a name="labels"></a>Etiketten

Automatisierte Etiketten werden Anfragen dazu Pull-Anfrage-Workflow zu verwalten und zu unterstützen, damit Sie wissen, was mit Pull-Anforderung abrufen:

* Beitrag Lizenzvertrag beziehen
    * CLA nicht erforderlich: der relativ gering und erfordert nicht, dass eine CLA signieren.
    * CLA erforderlich: die Änderung ist relativ groß und erfordert eine CLA signieren.
    * CLA signiert: Beitragenden signiert CLA, damit Pull-Anforderung zur Überprüfung jetzt vorwärts bewegen kann.
* Säule Etiketten: Etiketten wie PnP, moderne Apps, und TDC Pull-Anfragen von der internen Organisation kategorisiert, die Pull-Anforderung überprüfen.
* Änderung an den Autor gesendet: Autor der ausstehenden Pull-Anforderung mitgeteilt.

## <a name="more-resources"></a>Weitere Ressourcen

[Index des Spenders Handbuch](./contributor-guide/contributor-guide-index.md) für unsere Anleitung Themen angezeigt.
