<properties
   pageTitle="Leitfaden zum Erstellen eines Datendiensts für Marketplace | Microsoft Azure"
   description="Ausführliche Informationen zum Erstellen, zertifizieren und Bereitstellen von einem Datendienst für Azure Marketplace erwerben."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

   <tags
      ms.service="marketplace"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="na"
      ms.date="08/26/2016"
      ms.author="hascipio; avikova" />

# <a name="understanding-the-nodes-schema-for-mapping-an-existing-web-service-to-odata-through-csdl"></a>Grundlegendes zum Schema der Knoten für einen vorhandenen Webdienst OData über CSDL zuordnen

>[AZURE.IMPORTANT] **Zu diesem Zeitpunkt sind wir nicht mehr Onboarding alle neuen Datendienst Verleger. Neue Dataservices wird nicht für Angebot genehmigt erhalten.** Haben Sie eine SaaS Business Anwendung Elemente verwenden veröffentlichen möchten Sie weitere Informationen [hier](https://appsource.microsoft.com/partners). Wenn ein IaaS-Applikationen oder Developer Service Azure Marketplace veröffentlichen möchten weitere Informationen [hier](https://azure.microsoft.com/marketplace/programs/certified/).

Dieses Dokument wird zur Klärung der Knotenstruktur für CSDL OData-Protokoll zuordnen. Es ist wichtig zu beachten, dass die Knotenstruktur gut wohlgeformten XML-Code. So gilt Stammknoten, übergeordnete und untergeordnete Schema bei der OData-Zuordnung.

## <a name="ignored-elements"></a>Ignorierte Elemente
Es folgen die hohe CSDL-Elemente (XML-Knoten), die nicht während des Imports der Webdienst Metadaten von Azure Marketplace Backend verwendet werden. Sie können aber ignoriert werden.

| Element | Gültigkeitsbereich |
|----|----|
| Mithilfe von Element | Der Knoten und Unterknoten alle Attribute |
| Dokumentationselement | Der Knoten und Unterknoten alle Attribute |
| ComplexType | Der Knoten und Unterknoten alle Attribute |
| Association-Element | Der Knoten und Unterknoten alle Attribute |
| Erweiterte Eigenschaft | Der Knoten und Unterknoten alle Attribute |
| EntityContainer | Die folgenden Attribute werden ignoriert: *Erweitert* und *AssociationSet* |
| Schema | Die folgenden Attribute werden ignoriert: *Namespace* |
| FunctionImport | Die folgenden Attribute werden ignoriert: *Modus* (Standardwert ln vorausgesetzt) |
| EntityType | Nur die folgenden Unterknoten werden ignoriert: *Schlüssel* und *PropertyRef* |

Folgendes beschreibt ändert (hinzugefügt und Elemente ignoriert) verschiedene CSDL XML-Knoten im Detail.

## <a name="functionimport-node"></a>FunctionImport-Knoten
FunctionImport-Knoten stellt eine URL (Einstiegspunkt), der einen Dienst für den Endbenutzer verfügbar macht. Der Knoten ermöglicht, beschreibt, wie die URL behandelt wird, werden die Parameter für den Endbenutzer verfügbar sind und wie diese Parameter bereitgestellt.

Details zu diesem Knoten befinden sich [Hier] [MSDNFunctionImportLink]

[MSDNFunctionImportLink]: (https://msdn.microsoft.com/library/cc716710 (v=vs.100).aspx)

Sind zusätzliche Attribute (oder Attribute hinzufügen) FunctionImport-Knoten verfügbar sind:

**D:baseUri** -
die URI-Vorlage für die REST-Ressource, die Marketplace verfügbar gemacht wird. Marketplace verwendet die Vorlage Abfragen REST-Webdienst erstellen. Die URI-Vorlage enthält Platzhalter für Parameter in der Form {ParameterName}, wobei ParameterName der Name des Parameters ist. Ex. Anforderungsparameter = {Anforderungsparameter}.
Parameter können als URI-Parameter oder als Teil des URL-Pfads angezeigt werden. Bei der Darstellung im Pfad sind immer obligatorisch (kann nicht markiert werden als NULL-Werte zulässt). *Beispiel:*`d:BaseUri="http://api.MyWeb.com/Site/{url}/v1/visits?start={start}&amp;end={end}&amp;ApiKey=3fadcaa&amp;Format=XML"`

**Name** - der Name der importierten Funktion.  Nicht identisch mit anderen in CSDL definierten Namen.  Ex. Name = "GetModelUsageFile"

**EntitySet** *(optional)* – Wenn die Funktion eine Auflistung von Entitätstypen zurückgibt Wert **EntitySet** muss mit dem die Auflistung gehört. Andernfalls muss das **EntitySet** -Attribut nicht verwendet. *Beispiel:*`EntitySet="GetUsageStatisticsEntitySet"`

**ReturnType** *(Optional)* – gibt den Typ der zurückgegebenen Elemente vom URI.  Verwenden Sie dieses Attribut nicht, wenn die Funktion keinen Wert zurückgibt. Die folgenden sind unterstützten Typen:

 - **Auflistung (<Entity type name>)**: Legt eine Auflistung von definierten Entitätstypen. Der Name wird im Name-Attribut des Knotens EntityType vorhanden. Ein Beispiel ist die Auflistung (WXC. HourlyResult).
 - **Raw (<mime type>)**: Gibt eine unformatierte Dokument/Blob, das an den Benutzer zurückgegeben. Ein Beispiel ist Raw(image/jpeg) Weitere Beispiele:

  - ReturnType="Raw(text/plain)"
  - ReturnType = "Sammlung (Sage. DeleteAllUsageFilesEntity) "*

**D:Paging** - gibt an, wie Paging REST Ressource behandelt wird. Die Parameterwerte verwendet in geschweifte beginnt z.B. Seite = {$page} & Itemsperpage = {$size} Optionen sind verfügbar:

- **Keine:** kein Paging ist verfügbar
- **Überspringen:** Paging durch ein logisches "Überspringen" und "Entnahme" (oben) ausgedrückt. Überspringen überspringt M Elemente und nehmen anschließend die nächsten N Elemente zurückgibt. Parameterwert: $skip
- **Übernehmen:** Nehmen zurückgegeben die nächsten N Elemente. Parameterwert: $take
- **PageSize:** Paging über eine logische Seite und Größe (Elemente pro Seite) ausgedrückt. Die aktuelle Seite die zurückgegebene darstellt. Parameterwert: $page
- **Größe:** Größe gibt die Anzahl der Elemente pro Seite zurückgegeben. Parameterwert: $size

**d:AllowedHttpMethods** *(Optional)* – gibt das Verb von REST-Ressource behandelt wird. Außerdem schränkt akzeptierten Verb auf den angegebenen Wert.  Standard = POST.  *Beispiel:* `d:AllowedHttpMethods="GET"` Optionen sind verfügbar:

- **Abrufen:** normalerweise verwendet, um Daten zurückzugeben
- **POST:** in der Regel verwendet, um neue Daten einfügen
- **Setzen:** in der Regel verwendet, um Daten aktualisieren
- **Löschen:** zum Löschen von Daten

Weitere untergeordnete Knoten (nicht die CSDL-Dokumentation behandelt) im FunctionImport-Knoten sind:

**requestbody** *(Optional)* – der Anforderungstext wird verwendet, um anzugeben, dass die Anforderung an eine Stelle erwartet. Parameter können in den Hauptteil der Anforderung angegeben. Sie werden ausgedrückt in Klammern, z.B. {ParameterName}. Diese Parameter werden aus Benutzereingaben in den Körper zugeordnet, die der Inhaltsanbieter Dienst übertragen. Das RequestBody-Element hat ein Attribut mit Namen HttpMethod. Das Attribut kann zwei Werte:

- **POST:** Wenn die Anforderung HTTP POST verwendet
- **Erhalten:** Wenn die Anforderung HTTP GET verwendet

    Beispiel:

        `<d:RequestBody d:httpMethod="POST">
        <![CDATA[
        <req1:Request xmlns:r1="http://schemas.mysite.com//generic/requests/1" Version="1.0">
        <req1:Query>{Query}</req1:Query><req1:AppId>D453474</req1:AppId>
        <req:DestinationSchemas><req1:Schema>Generic.RequestResponse[1.0]</req1:Schema>
        </req1:DestinationSchemas>
        </req1: Request>
        ]]>
        </d:RequestBody>`

**D:Namespaces** und **D:Namespace** - beschreibt dieses Knotens Namespaces, die in XML definiert sind, die von der Funktionsimport (URI Endpunkt) zurückgegeben. Das Back-End-Dienst zurückgegebene XML enthalten eine beliebige Anzahl von Namespaces, den Inhalt zu unterscheiden, der zurückgegeben wird. **Alle diese Namespaces in D:Map oder D:Match XPath-Abfragen verwendet aufgeführt werden müssen.** D:Namespaces-Knoten enthält eine Reihe/Liste D:Namespace Knoten. Jede enthält einen Namespace in die Back-End-Antwort verwendet. Es folgen das Attribut D:Namespace Knoten:

-   **D:Prefix:** Das Präfix für den Namespace im XML zurückgegebenen Ergebnisse vom Dienst, z. B. F:FirstName, F:LastName, wobei f das Präfix ist.
- **D:Uri:** Die vollständige URI der Namespace, in dem Dokument verwendet wird. Er entspricht dem Wert, den das Präfix zur Laufzeit aufgelöst wird.

**d:ErrorHandling** *(Optional)* - dieses Knotens enthält Vorschriften für die Fehlerbehandlung. Jede Bedingung überprüft das Ergebnis, das durch den Inhaltsanbieter Dienst zurückgegeben wird. Wenn eine Bedingung vorgeschlagenen HTTP-Fehlercode, den eine Fehlermeldung für den Endbenutzer entspricht.

**d:ErrorHandling** *(Optional)* und **D:Condition** enthält *(Optional)* – ein Bedingungsknoten eine Bedingung, die von der Inhaltsanbieter Dienst zurückgegebenen Ergebnisses überprüft wird. Im folgenden sind die **erforderlichen** Attribute:

- **D:Match:** Ein XPath-Ausdruck, der überprüft, ob ein angegebene Knotenwert in den Inhaltsanbieter XML-Ausgabedatei vorhanden ist. XPath wird auf die Ausgabe und sollte True, wenn die Bedingung eine Übereinstimmung oder False wird zurückgegeben.
- **HttpStatusCode:** Entspricht der HTTP-Statuscode, der vom Markt bei den Zustand zurückgegeben werden soll. Marketplace signalisiert Fehler dem Benutzer über HTTP-Statuscodes. Eine Liste der HTTP-Statuscodes finden Sie unter http://en.wikipedia.org/wiki/HTTP_status_code
- **D:ErrorMessage:** Die Fehlermeldung – mit HTTP-Statuscode – an den Endbenutzer zurückgegeben wird. Dies sollte eine kurze Fehlermeldung, die vertrauliche Daten enthalten.

**d:Title** *(Optional)* - ermöglicht den Titel der Funktion beschreibt. Stammt der Wert für den Titel

- Das optionale Karte Attribut (Xpath) gibt, finden Sie den Titel auf der Service-Anforderung zurückgegeben.
- – Oder – den Titel als Wert des Knotens angegeben.

**d:Rights** *(Optional)* - Rechte (z. B. das Urheberrecht) der Funktion. Der Wert für den rechten stammt:

- Das optionale Karte Attribut (Xpath) gibt, finden Sie die Rechte auf die Service-Anforderung zurückgegeben.
-   – Oder – Rechte als Wert des Knotens.

**d:Description** *(Optional)* - eine kurze Beschreibung der Funktion. Stammt der Wert für die Beschreibung

- Das optionale Karte Attribut (Xpath), finden Sie die Beschreibung in der Antwort von der serviceanforderung gibt.
- – Oder – die Beschreibung als Wert des Knotens angegeben.

**D:EmitSelfLink** - *finden Sie unter Beispiel "FunctionImport für 'Paging' Daten zurückgegeben"*

**D:EncodeParameterValue** - optionale Erweiterung OData

**D:QueryResourceCost** - optionale Erweiterung OData

**D:Map** - optionale Erweiterung OData

**D:Headers** - optionale Erweiterung OData

**D:Headers** - optionale Erweiterung OData

**D:Value** - optionale Erweiterung OData

**HttpStatusCode** - optionale Erweiterung OData

**D:ErrorMessage** - optionale Erweiterung OData

## <a name="parameter-node"></a>Parameter (Knoten)

Dieser Knoten stellt einen Parameter, die als Teil der Vorlage URI verfügbar / Anforderungsinhalt, die im FunctionImport-Knoten angegeben.

Eine Dokumentseite sehr nützliche Informationen über die "Parameter" Elementknoten befindet sich [hier](http://msdn.microsoft.com/library/ee473431.aspx) (mit der Dropdownliste **Andere Version** eine andere Version wählen Sie bei Bedarf die Dokumentation). *Beispiel:*`<Parameter Name="Query" Nullable="false" Mode="In" Type="String" d:Description="Query" d:SampleValues="Rudy Duck" d:EncodeParameterValue="true" MaxLength="255" FixedLength="false" Unicode="false" annotation:StoreGeneratedPattern="Identity"/>`

| Parameter-Attribut | Ist erforderlich | Wert |
|----|----|----|
| Name | Ja | Der Name des Parameters. Groß-/Kleinschreibung beachtet.  Die Groß-/BaseUri. **Beispiel:**`<Property Name="IsDormant" Type="Byte" />` |
| Typ | Ja | Der Parametertyp. Der Wert muss ein **EDMSimpleType** oder ein komplexer Typ, der innerhalb des Gültigkeitsbereichs des Modells. Weitere Informationen finden Sie unter "6 unterstützt Parametereigenschaft Typen".  (Groß-/Kleinschreibung beachtet. Erste Zeichen werden groß geschrieben, Rest sind Kleinbuchstaben)  Siehe auch [konzeptionellen Modell Typen (CSDL)] [MSDNParameterLink]. **Beispiel:**`<Property Name="LimitedPartnershipID " Type="Int32" />` |
| Modus | Nein | **In**, Out oder InOut je nachdem, ob der Parameter ein Eingabe-, Ausgabe oder Input-Output-Parameter. (Nur "IN" steht in Azure Marketplace.) **Beispiel:**`<Parameter Name="StudentID" Mode="In" Type="Int32" />` |
| MaxLength | Nein | Die maximal zulässige Länge des Parameters. **Beispiel:**`<Property Name="URI" Type="String" MaxLength="100" FixedLength="false" Unicode="false" />` |
| Genauigkeit | Nein | Die Genauigkeit des Parameters. **Beispiel:**`<Property Name="PreviousDate" Type="DateTime" Precision="0" />` |
| Skalierung | Nein | Die Skalierung des Parameters. **Beispiel:**`<Property Name="SICCode" Type="Decimal" Precision="10" Scale="0" />` |

[MSDNParameterLink]: (http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx)

Folgende sind die Attribute, die in der CSDL-Spezifikation hinzugefügt wurden:

| Parameter-Attribut | Beschreibung |
|----|----|
| **d:Regex** *(Optional)* | Ein Regex-Anweisung verwendet, um den Wert für den Parameter überprüfen. Wenn der Wert die Anweisung übereinstimmt wird der Wert abgelehnt. Dadurch können auch einen Satz möglicher Werte z. B. an ^ [0-9] +? $ nur Zahlen zulässig. **Beispiel:** ' < Parametername = "name" Modus "In" Type = = "String" d: Nullable = "false" D:Regex = "^ [a-zA-Z] * $" D:Description = "einen Namen, die Leerzeichen oder nicht alphanumerischen nichtenglische Zeichen enthalten kann" D:SampleValues = "Georg|John|Thomas|James "/ >" |
| **d:Enum** *(Optional)* | Eine Pipe getrennte Liste der Werte für den Parameter. Wert muss entsprechend den definierten Typ des Parameters. Beispiel: ' Englisch|Metrik|RAW`. Enum will display as a selectable dropdown list of parameters in the UI (service explorer). **Example:** `< Parametername = "Dauer" Type = "String" Modus = "In" Nullable = "true" D:Enum = "1 Jahr|5 Jahre|10 "/ >" |
| **d: Nullable** *(Optional)* | Können festlegen, ob der Parameter null sein kann. Der Standardwert ist: true. Parameter, die als Teil des Pfades in der URI-Vorlage verfügbar null nicht. Die Benutzereingabe wird überschrieben, wenn das Attribut für diesen Parameter auf False festgelegt ist. **Beispiel:**`<Parameter Name="BikeType" Type="String" Mode="In" Nullable="false"/>` |
| **d:SampleValue** *(Optional)* | Ein Beispielwert als Notiz an den Client in der Benutzeroberfläche angezeigt.  Können mehrere Werte hinzufügen, indem Sie eine Liste Pipe getrennt z. B. "ein|b|c` **Example:** `< Parametername = "BikeOwner" Type = "String" Modus = "In" D:SampleValues = "Georg|John|Thomas|James "/ >" |

## <a name="entitytype-node"></a>EntityType Knoten

Dieser Knoten stellt einen der Typen von Marketplace an den Endbenutzer zurückgegeben werden. Darüber hinaus enthält die Zuordnung aus der Ausgabe des Inhaltsanbieters Dienst die Werte wieder, die für den Endbenutzer zurückgegeben werden.

Details zu diesem Knoten befinden sich [hier](http://msdn.microsoft.com/library/bb399206.aspx) (verwenden Sie **Andere Version** Dropdown, wählen Sie eine andere Version ggf. um die Dokumentation anzuzeigen.)

| Attributname | Ist erforderlich | Wert |
|----|----|----|
| Name | Ja | Der Name des Entitätstyps. **Beispiel:**`<EntityType Name="ListOfAllEntities" d:Map="//EntityModel">` |
| BaseType | Nein | Der Name einer anderen Entität Typ, den Basistyp des Entitätstyps, der definiert wird. **Beispiel:**`<EntityType Name="PhoneRecord" BaseType="dqs:RequestRecord">` |

Folgende sind die Attribute, die in der CSDL-Spezifikation hinzugefügt wurden:

**D:Map** - ein XPath-Ausdruck für die Ausgabe ausgeführt. Die Annahme ist, dass die Ausgabe Elemente, die wiederholt werden, enthält, wie ein ATOM-wo Feed wiederholen Sie die Eingabe-Knoten gibt. Jede dieser wiederholten Knoten enthält einen Datensatz. Dann wird der XPath-Ausdruck auf einzelnen wiederholten Knoten des Inhaltsanbieters Service Ergebnis zeigen, der die Werte für einen einzelnen Datensatz festgelegt. Wird vom Dienst ausgegeben:

        `<foo>
          <bar> … content … </bar>
          <bar> … content … </bar>
          <bar> … content … </bar>
        </foo>`

Der XPath-Ausdruck wäre Foo/bar, da jeder Knoten ist der Leiste der wiederholten Knoten in der Ausgabe und enthält den eigentlichen Inhalt, der an den Endbenutzer zurückgegeben.

**Schlüssel** : Dieses Attribut wird vom Marketplace ignoriert. REST-Webdienste basieren, setzen einen Primärschlüssel in der Regel nicht.


## <a name="property-node"></a>-Knoten

Dieser Knoten enthält eine Eigenschaft des Datensatzes.

Details zu diesem Knoten finden Sie unter [http://msdn.microsoft.com/library/bb399546.aspx](http://msdn.microsoft.com/library/bb399546.aspx) (verwenden Sie **Andere Version** Dropdown, wählen Sie eine andere Version ggf. um die Dokumentation anzuzeigen.) *Beispiel:*
        `<EntityType Name="MetaDataEntityType" d:Map="/MyXMLPath">
        <Property Name="Name"   Type="String" Nullable="true" d:Map="./Service/Name" d:IsPrimaryKey="true" DefaultValue=”Joe Doh” MaxLength="25" FixedLength="true" />
        ...
        </EntityType>`

| AttributeName | Erforderlich | Wert |
|----|----|----|
| Name | Ja | Der Name der Eigenschaft. |
| Typ | Ja | Der Typ des Eigenschaftswerts. Typ der Wert muss ein **EDMSimpleType** oder ein komplexer Typ (gekennzeichnet durch einen voll qualifizierten Namen), die im Rahmen des Modells. Weitere Informationen finden Sie im konzeptionellen Modell Typen (CSDL). |
| NULL-Werte zulässt | Nein | **True** (Standardwert) oder **False,** je nachdem, ob die Eigenschaft den Wert null annehmen kann. Hinweis: In der Version vom Namespace [http://schemas.microsoft.com/ado/2006/04/edm](http://schemas.microsoft.com/ado/2006/04/edm) CSDL Eigenschaft eines komplexen Typs muss Nullable = "False". |
| Standardwert | Nein | Der Standardwert der Eigenschaft. |
|MaxLength | Nein | Die maximale Länge des Eigenschaftswerts. |
| FixedLength | Nein | **True** oder **False,** je nachdem, ob der Eigenschaftswert als eine Zeichenfolge festgesetzte gespeichert werden. |
| Genauigkeit | Nein | Bezieht sich auf die maximale Anzahl von Ziffern in numerischen Wert beibehalten. |
| Skalierung | Nein | Maximale Anzahl von Dezimalstellen an, die den numerischen Wert beibehalten. |
| Unicode | Nein | **True** oder **False,** je nachdem, ob der Wert der Eigenschaft als eine Unicode-Zeichenfolge gespeichert. |
| Sortierung | Nein | Eine Zeichenfolge, die die Sortierreihenfolge in der Datenquelle verwendet werden. |
| ConcurrencyMode | Nein | **Keine** (Standardwert) oder **fest**. Wenn der **feste**Wert ist, wird der Eigenschaftswert in optimistische Parallelität verwendet. |

Es folgen zusätzliche Attribute, die in der CSDL-Spezifikation hinzugefügt wurden:

**D:Map** - XPath-Ausdruck für den Dienst aufgerufen Ausgabe extrahiert eine Eigenschaft der Ausgabe. Der angegebene XPath ist relativ zum wiederholten Knoten in XPath-EntityType Knoten ausgewählt wurde. Es kann auch an einen absoluten XPath um Knoten Ausgabe wie z. B. eine copyright-Anweisung, die nur einmal in den ursprünglichen Dienst ist eine statische Ressource einschließlich Ausgabe sollte jedoch in jeder der Zeilen in der OData-Ausgabe zu ermöglichen. Wird vom Dienst:

        `<foo>
          <bar>
           <baz0>… value …</baz0>
           <baz1>… value …</baz1>
           <baz2>… value …</baz2>
          </bar>
        </foo>`

Der XPath-Ausdruck wäre ./bar/baz0 baz0 Knoten aus der Inhaltsanbieter Dienst abrufen.

**D:CharMaxLength** - für Typ String Geben Sie die maximale Länge. Siehe DataService CSDL-Beispiel

**D:IsPrimaryKey** - gibt an, ob die Spalte der Primärschlüssel in der Tabellenansicht. Siehe DataService CSDL-Beispiel.

**D:isExposed** - bestimmt, ob das Schema verfügbar gemacht wird (in der Regel true). Siehe DataService CSDL-Beispiel

**d:IsView** *(Optional)* - true zurück, wenn dies eine Ansicht anstelle einer Tabelle basiert.  Siehe DataService CSDL-Beispiel

**D:Tableschema** - Siehe DataService CSDL-Beispiel

**D:ColumnName** - ist der Name der Spalte in der Tabellenansicht.  Siehe DataService CSDL-Beispiel

**D:IsReturned** - ist der boolesche Wert, der bestimmt, ob der Dienst diesen Wert an den Client verfügbar macht.  Siehe DataService CSDL-Beispiel

**D:IsQueryable** - ist der boolesche Wert, der bestimmt, ob die Spalte in einer Abfrage verwendet werden kann.   Siehe DataService CSDL-Beispiel

**OrdinalPosition** - ist die Spalte numerische Position der Darstellung in der Tabelle oder der Ansicht x, X 1 auf die Anzahl der Spalten in der Tabelle ist.  Siehe DataService CSDL-Beispiel

**D:DatabaseDataType** - ist der Datentyp der Spalte in der Datenbank, d. h. SQL-Datentyp. Siehe DataService CSDL-Beispiel

## <a name="supported-parametersproperty-types"></a>Typen von unterstützten Parameters-Eigenschaft
Folgende sind die unterstützten Typen für Parameter und Eigenschaften. (Groß-/Kleinschreibung)

| Primitive Typen | Beschreibung |
|----|----|
| NULL | Stellt das Fehlen eines Wertes |
| Boolescher Wert | Stellt das mathematische Konzept der Binary-Werte-Logik|
| Byte | 8-Bit-Ganzzahl ohne Vorzeichen|
|DateTime| Stellt Datum und Zeit von 12:00:00 Mitternacht, 1. Januar 1753 n. Chr. bis 11:59:59 Uhr Dezember 9999 n. Chr.|
|Dezimal | Stellt numerische Werte mit fester Genauigkeit und Skalierung. Dieses Typs beschreiben einen numerischen Wert zwischen-10 ^ 255 + 1 positive 10 ^ 1, 255|
| Double | Stellt eine Gleitkommazahl mit 15 Stellen Genauigkeit, die mit ungefähren Bereich von ± 2, 23E-308 bis ± 1, 79E darstellen kann +308. **Verwenden Sie Decimal Exel Export Problem**|
| Einzelne | Stellt eine Gleitkommazahl mit einer Genauigkeit von 7 Ziffern, die mit ungefähren Bereich von ± 1,18E-38 bis ± 3, 40E darstellen kann 38|
|GUID |Stellt einen Bezeichnerwert von 16 Byte (128 Bit) eindeutige |
|Int16|Stellt einen Wert von 16-Bit-Ganzzahl mit Vorzeichen |
|Int32|Stellt einen Wert von 32-Bit-Ganzzahl mit Vorzeichen |
|Int64|Stellt einen Wert von 64-Bit-Ganzzahl mit Vorzeichen |
|Zeichenfolge | Stellt Zeichendaten fester oder Variable-Länge |


## <a name="see-also"></a>Siehe auch
- Wenn Sie allgemeine OData Zuordnungsprozess und Zweck verstehen möchten, dieser Artikel [Service OData Zuordnung](marketplace-publishing-data-service-creation-odata-mapping.md) Definitionen, Strukturen und Informationen zu überprüfen.
- Wenn Sie beispielsweise Lesen [Daten OData Zuordnung Beispiele](marketplace-publishing-data-service-creation-odata-mapping-examples.md) finden Beispielcode und Codesyntax Kontext überprüfen.
- Um wieder den vorgeschriebenen Pfad zum Veröffentlichen von einem Datendienst Azure Marketplace dieser Artikel [Veröffentlichen Servicehandbuch Daten](marketplace-publishing-data-service-creation.md).
