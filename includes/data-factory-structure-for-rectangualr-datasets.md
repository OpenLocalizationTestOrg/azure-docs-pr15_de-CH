## <a name="specifying-structure-definition-for-rectangular-datasets"></a>Strukturdefinition für rechteckige Datasets angeben
Die Struktur in den Datensätzen JSON wird ein **Optionaler** Abschnitt rechteckige Tabellen (Zeilen und Spalten) und enthält eine Auflistung von Spalten für die Tabelle. Abschnitt Struktur verwenden Sie entweder die Typinformationen für Typumwandlungen oder Spalte Zuordnung ausführen. In den folgenden Abschnitten werden diese Features ausführlich beschrieben. 

Jede Spalte enthält die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| Name | Name der Spalte. | Ja |
| Typ | Der Datentyp der Spalte. Finden Sie unter Type Conversions Abschnitt für weitere details bezüglich Informationen spezifizieren | Nein |
| Kultur | .NET basiert Kultur beim Typ angegeben und .NET Typ Datetime oder Datetimeoffset verwendet werden. Standardwert ist "En-us".  | Nein |
| Format | Formatzeichenfolge beim Typ angegeben und .NET Typ Datetime oder Datetimeoffset verwendet werden. | Nein |

Das folgende Beispiel zeigt den Abschnitt Struktur JSON für eine Tabelle mit drei Spalten ID, Name und Lastlogindate.

    "structure": 
    [
        { "name": "userid"},
        { "name": "name"},
        { "name": "lastlogindate"}
    ],

Verwenden Sie die folgenden Richtlinien für "Struktur" Informationen hinzufügen und was im Bereich **Struktur** .

- Führen **für strukturierte Daten** , das Schema und Daten mit den Daten selbst (Quellen wie SQL Server, Oracle, Azure usw.) im Abschnitt "Structure" Angabe sollte nur speichern Zuordnen von bestimmten Spalten auf bestimmte Spalten in Senke und ihre Namen sind nicht identisch (siehe Spalte Zuordnung weiter unten im Abschnitt). 

    Wie bereits erwähnt, ist die Informationen im Abschnitt "Structure" optional. Strukturierte Quellen Informationen ist bereits verfügbar als Teil des Dataset-Definition im Datenspeicher so nicht anzugeben Typinformationen beim Abschnitt "Structure" beinhalten.
- **Schema lesen Datenquellen (insbesondere Azure Blob)** können Sie Daten speichern, ohne die Daten Schema- oder Typinformationen. Für diese Typen von Datenquellen sollten Sie "Struktur" 2 bei aufnehmen:
    - Zuordnen möchten.
    - Wird das Dataset Quelle in eine kopieraktivität, können Sie Informationen in "Structure" und "Data Factory" verwendet diese Informationen für die Konvertierung in systemeigenen Typen für die Senke. [Verschieben von Daten zwischen Azure-BLOBs](../articles/data-factory/data-factory-azure-blob-connector.md) finden Sie im Artikel Weitere Informationen.

### <a name="supported-net-based-types"></a>Unterstützt. NET-basierte Typen 
Data Factory unterstützt die folgenden CLS kompatiblen .NET Framework-basierte Typwerte geben Informationen in "Structure" Schema auf schreibgeschützte Datenquellen wie Azure Blob.

- Int16
- Int32 
- Int64
- Einzelne
- Double
- Dezimal
- Byte]
- Bool
- Zeichenfolge 
- GUID
- DateTime
- DateTimeOffset
- TimeSpan 

Datetime & Datetimeoffset soll optional "Kultur" und "format" Zeichenfolge zu erleichtern, die benutzerdefinierte Datetime-Zeichenfolge analysieren. Finden Sie im Beispiel unten Konvertierung.

