<properties
   pageTitle="Wie Suche und pin Daten | Microsoft Azure"
   description="Gewusst wie-Artikel hervorheben Azure Data Catalog-Funktionen für Datenquellen und Datenbestände für die spätere Verwendung speichern."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/10/2016"
   ms.author="maroche"/>

# <a name="how-to-save-searches-and-pin-data-assets"></a>Wie Suche und Fixieren von Daten

## <a name="introduction"></a>Einführung

Microsoft Azure Data Catalog bietet Funktionen zur Erkennung von Quelle. Benutzer können schnell suchen und Filtern im Katalog suchen Datenquellen und deren Verwendungszweck verstehen die richtigen Daten für die Aufgabe zu erleichtern.

Aber was ist, wenn Benutzer regelmäßig mit denselben Daten arbeiten müssen? Was als Benutzer regelmäßig dieselben Datenquellen im Katalog wissen beitragen? In diesen Fällen müssen wiederholt dieselben Suchvorgänge ausstellen kann ineffizient sein – gespeicherte Suche und Anlagen können Daten fixiert.

## <a name="saved-searches"></a>Gespeicherte Suchen

Eine gespeicherte Suche in Azure Data Catalog ist eine wieder verwendbare, pro Benutzer Suchdefinition. Wenn eine Suche – einschließlich Suchbegriffe, Tags und andere Filter – ein benutzerdefinierter kann er zur späteren Verwendung speichern. Definition der gespeicherten Suche kann dann zu einem späteren Zeitpunkt wieder alle Datenbestände, die die Suchkriterien erneut ausgeführt werden.

### <a name="creating-a-saved-search"></a>Erstellen einer gespeicherten Suche

Erstellen Sie eine gespeicherte Suche geben Sie zunächst die Suchkriterien wiederverwendet werden. Klicken Sie dann auf den Link "Speichern" im Feld "Suchkriterien" Azure Data Catalog-Portal.

 ![Wählen Sie "Speichern", um die aktuelle Suche speichern](./media/data-catalog-how-to-save-pin/01-save-option.png)

Wenn Sie aufgefordert werden, geben Sie einen Namen für die gespeicherte Suche. Wählen Sie ein sinnvollen und aussagekräftigen Datenbestände, die von der Suche zurückgegeben werden.

 ![Geben Sie einen Namen für die gespeicherte Suche](./media/data-catalog-how-to-save-pin/02-name.png)

### <a name="managing-saved-searches"></a>Gespeicherte Suchen verwalten

Sobald ein Benutzer eine Suche gespeichert, wird eine Option "Gespeicherte Suche" in Azure Data Catalog-Portal unter "aktuelle" Suchfeld angezeigt. Wenn erweitert, wird die vollständige Liste der speichert suchen angezeigt.

 ![Liste der gespeicherten Suchen](./media/data-catalog-how-to-save-pin/03-list.png)

Eine gespeicherte Suche aus der Liste auswählen, wird die Suche ausgeführt werden.

Wählen Sie im Dropdown Menü bietet eine Reihe von Optionen:

 ![Optionen zum Verwalten von gespeicherten Suchen](./media/data-catalog-how-to-save-pin/04-managing.png)

"Umbenennen" fordert den Benutzer zur Eingabe eines neuen Namens für die gespeicherte Suche. Definition der Suche wird nicht geändert.

Wählen "Löschen" fordert den Benutzer zur Bestätigung und die gespeicherte Suche wird dann aus der Liste entfernt.

Auswahl "Als Standard speichern" markiert ausgewählte gespeicherte Suche als Standardsuche für den Benutzer. Der Benutzer von Azure Data Catalog-Homepage "empty" Suche ausführt, wird der Benutzer Standardsuche durchgeführt. Darüber hinaus die Suche als standardmäßig am Anfang der Liste Gespeicherte Suchen angezeigt wird markiert.

### <a name="organizational-saved-searches"></a>Organisationseinheit gespeicherten Suchen

Alle Benutzer kann suchen für den eigenen Gebrauch speichern. Außerdem können Datenadministratoren Katalog sucht nach allen Benutzern in der Organisation speichern. Wenn Sie eine Suche speichern, werden Administratoren mit die gespeicherte Suche innerhalb des Unternehmens freigeben dargestellt. Wenn diese Option aktiviert ist, wird die gespeicherte Suche in der Liste der verfügbaren sucht nach allen Benutzern enthalten.

 ![Organisationseinheit gespeicherten Suchen](./media/data-catalog-how-to-save-pin/08-organizational-saved-search.png)


## <a name="pinned-data-assets"></a>Fixierte Daten

Gespeicherte Suchen können Benutzer speichern und Wiederverwenden von Suchdefinitionen. von der Suche zurückgegebenen Datenbestände können wie der Inhalt der Katalog ändern. Fixieren von Daten kann Benutzer bestimmte Datenressourcen zu einfacher machen eine Suche ohne explizit identifiziert.

Fixieren einer Datenressource ist einfach – Benutzer können klicken Sie das Symbol "Stift" Datenressource angehefteten Liste hinzu. Dieses Symbol wird in der Ecke der Anlage Fläche in der Tile-Ansicht und in der Spalte ganz links in der Listenansicht in Azure Data Catalog-Portal.

![Eine Datenressource fixieren](./media/data-catalog-how-to-save-pin/05-pinning.png)

Nicht mehr festhalten einer Anlage ist gleichermaßen einfach – Benutzer klicken einfach auf das Symbol "Stift" erneut aus, um die Einstellung für die ausgewählte Anlage.

![Eine Datenressource Fixierung aufheben](./media/data-catalog-how-to-save-pin/06-unpinning.png)

## <a name="my-assets"></a>"Meine Ressourcen"
Die Portalhomepage Azure Data Catalog enthält einen Abschnitt "Eigene Ressourcen", der Anlagen von Interesse für den aktuellen Benutzer angezeigt. Dieser Abschnitt enthält sowohl fixierte Elemente und gespeicherte Suchen.

!['Meine Ressourcen' auf der Homepage](./media/data-catalog-how-to-save-pin/07-my-assets.png)

## <a name="summary"></a>Zusammenfassung
Azure Data Catalog bietet Funktionen, mit denen sie Benutzern zu Datenquellen, die sie benötigen, die damit sie weniger Zeit für Daten und mehr Zeit damit verbringen können. Gespeicherte Suchen und Daten auf diesen zentralen Funktionen erstellen einfach Daten besser mit denen sie wiederholt funktionieren fixiert.
