<properties
    pageTitle="Assistent zum Kopieren von Factory | Microsoft Azure"
    description="Enthält Informationen zum Assistenten zum Kopieren von Factory verwenden zum Kopieren von Daten aus unterstützten Datenquellen zu senken."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="spelluru"/>

# <a name="data-factory-copy-wizard"></a>Assistent zum Kopieren von Factory
Der Assistent zum Kopieren von Azure Data Factory ist technologische Daten erleichtern normalerweise ist ein erster Schritt in einem End-to-End Data Integration. Wenn durch den Assistenten zum Kopieren von Azure Data Factory müssen Sie nicht verstehen, JSON Definitionen für verknüpfte Dienste, Datasets und Rohrleitungen. Jedoch nach Abschluss der Schritte im Assistenten erstellt der Assistent automatisch eine Pipeline zum Kopieren von Daten aus der ausgewählten Datenquelle an das ausgewählte Ziel. Darüber hinaus die Kopie Assistent zur Validierung der Daten zum Zeitpunkt der Erstellung aufgenommen spart viel Zeit, besonders wenn Sie sind Einnahme Daten zum ersten Mal aus der Datenquelle. Starten Sie den Assistenten zum Kopieren klicken Sie **Daten** auf der Homepage Ihrer Data Factory.

![Assistent zum Kopieren von](./media/data-factory-copy-wizard/copy-data-wizard.png)


## <a name="an-intuitive-wizard-for-copying-data"></a>Ein intuitiver Assistent zum Kopieren von Daten
Mit diesem Assistenten können Sie Daten aus einer Vielzahl von Quellen, Zielen in Minuten problemlos. Nach Abschluss des Assistenten, wird Sie automatisch eine Rohrleitung mit einer Kopie mit abhängigen Daten Factory Einheiten (verknüpften Diensten und Datasets) erstellt. Beim Erstellen der Pipeline sind keine weiteren Schritte erforderlich.   

![Datenquelle auswählen](./media/data-factory-copy-wizard/select-data-source-page.png)

> [AZURE.NOTE] [Assistent zum Kopieren von Lernprogramm](data-factory-copy-data-wizard-tutorial.md) finden Sie im Artikel eine schrittweise Anleitung zum Erstellen einer Pipeline Beispiel kopieren Daten aus einer Azure blob zu einer Azure SQL-Datenbank. 

Der Assistent dient big Data Bedenken ab. Es ist einfach und effizient Data Factory Pipelines erstellen, die Hunderte von Ordnern, Dateien oder Tabellen mit dem Assistenten Daten verschieben. Der Assistent unterstützt die folgenden drei Funktionen: Automatische Vorschau Schema Erfassung und Zuordnung und Filtern von Daten. 

## <a name="automatic-data-preview"></a>Automatische Vorschau 
Der Assistent zum Kopieren können Sie Bestandteil der Daten aus der ausgewählten Datenquelle für Sie überprüfen, ob die Daten sie Daten kopieren möchten. Darüber hinaus ist die Daten in einer Textdatei analysiert der Assistenten zum Kopieren Textdatei Zeilen- und Spaltentrennzeichen und Schema automatisch erhalten. 

![Formateinstellungen](./media/data-factory-copy-wizard/file-format-settings.png)

## <a name="schema-capture-and-mapping"></a>Schema Erfassung und Zuordnung 
Das Schema der Daten kann das Schema der Ausgabedaten in einigen Fällen nicht überein. In diesem Szenario müssen Sie Spalten Quellschema Zielschema Spalten zuordnen. 

Der Assistent zum Kopieren wird automatisch Spalten im Quellschema Spalten im Zielschema. Sie können Zuordnungen mithilfe der Dropdownlisten überschreiben oder angeben, ob eine Spalte muss beim Kopieren der Daten übersprungen werden.   

![Schema-Zuordnung](./media/data-factory-copy-wizard/schema-mapping.png)

## <a name="filtering-data"></a>Filtern von Daten  
Der Assistent ermöglicht das Filtern von Daten wählen Sie nur die Daten, die sich in die Ziel-Senke kopiert werden. Filterung verringert die Datenmenge in den Datenspeicher Senke kopiert werden und daher erhöht den Durchsatz des Kopiervorgangs. Es bietet eine flexible Möglichkeit zum Filtern von Daten in einer relationalen Datenbank mithilfe SQL Query Language (oder) Dateien in einem Ordner Azure BLOB- [Daten Factory Funktionen und Variablen](data-factory-functions-variables.md).   

### <a name="filtering-of-data-in-a-database"></a>Filtern von Daten in einer Datenbank  
Im Beispiel die SQL-Abfrage verwendet die `Text.Format` Funktion und `WindowStart` Variable. 

![Ausdrücke prüfen](./media/data-factory-copy-wizard/validate-expressions.png)

### <a name="filtering-of-data-in-an-azure-blob-folder"></a>Filtern von Daten in einem Ordner Azure blob
Können Sie Variablen in den Pfad kopieren von Daten aus einem Ordner, der zur Laufzeit basierend auf [Variablen](data-factory-functions-variables.md#data-factory-system-variables)bestimmt wird. Die unterstützten Variablen: **{Jahr}**, **{Monat}** **{Day}**, **{Stunde}**, **{Minute}**und **{benutzerdefinierte}**. Beispiel: Inputfolder / {Jahr} / {Monat} täglich {}.

Angenommen Sie, Sie Ordner im folgenden Format eingegeben haben:

    2016/03/01/01
    2016/03/01/02
    2016/03/01/03
    ...

Klicken Sie auf die Schaltfläche **Durchsuchen** für **Dateien oder Ordner**, suchen Sie einen Ordner (z. B. 2016-03 > -> 01-02 >), und klicken Sie auf **auswählen**. Sollte `2016/03/01/02` im Textfeld. Nun ersetzen Sie **2016** **{Jahr}**, **03** mit **{}** **{Day}** **01** und **02** mit **{}**, und drücken Sie Tab. Dropdown-Liste Wählen Sie das Format für diese vier Variablen sollten angezeigt werden:

![Mithilfe von Systemvariablen](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)   

Wie im folgenden Screenshot gezeigt, können Sie auch eine **benutzerdefinierte** Variable und [Formatzeichenfolgen unterstützt](https://msdn.microsoft.com/library/8kb3ddd4.aspx). Wählen Sie einen Ordner mit dieser Struktur zuerst verwenden Sie die Schaltfläche **Durchsuchen** . Ersetzen eines Wertes mit **{benutzerdefinierten}**, und Tab drücken, um das Textfeld angezeigt, können Sie die Zeichenfolge eingeben.     

![Mithilfe von benutzerdefinierten Variablen](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)


## <a name="support-for-diverse-data-and-object-types"></a>Unterstützung für unterschiedliche Daten- und Objekttypen
Mithilfe des Assistenten zum Kopieren können Sie effizient Hunderte von Ordnern, Dateien oder Tabellen verschieben.

![Wählen Sie Tabellen, Daten kopieren](./media/data-factory-copy-wizard/select-tables-to-copy-data.png)

## <a name="scheduling-options"></a>Planungsoptionen
Sie können den Kopiervorgang einmal oder nach einem Zeitplan (stündlich, täglich, usw.) ausführen. Beide Optionen können für die Breite der Connectors in lokalen Cloud und desktop Kopie verwendet werden.

Eine einmalige Kopie ermöglicht Datentransfer aus einer Quelle an ein Ziel nur einmal. Bezieht sich auf Daten jeder Größe und jedem unterstützten Format. Die gespeicherte Kopie können Sie Daten auf vorgegebene Serie kopieren. Rich Einstellungen (wie Retry Timeout und Alarme) können Sie die gespeicherte Kopie konfigurieren.

![Zeitplaneigenschaften](./media/data-factory-copy-wizard/scheduling-properties.png)


## <a name="next-steps"></a>Nächste Schritte
Eine kurze exemplarische Vorgehensweise mithilfe des Assistenten zum Kopieren von Factory eine Pipeline mit Kopie erstellt, finden Sie unter [Tutorial: eine Rohrleitung mit Assistent zum Kopieren von](data-factory-copy-data-wizard-tutorial.md).
