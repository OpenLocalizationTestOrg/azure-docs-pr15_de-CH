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

# <a name="mapping-an-existing-web-service-to-odata-through-csdl"></a>OData über CSDL Zuordnen eines vorhandenen Webdiensts

>[AZURE.IMPORTANT] **Zu diesem Zeitpunkt sind wir nicht mehr Onboarding alle neuen Datendienst Verleger. Neue Dataservices wird nicht für Angebot genehmigt erhalten.** Haben Sie eine SaaS Business Anwendung Elemente verwenden veröffentlichen möchten Sie weitere Informationen [hier](https://appsource.microsoft.com/partners). Wenn ein IaaS-Applikationen oder Developer Service Azure Marketplace veröffentlichen möchten weitere Informationen [hier](https://azure.microsoft.com/marketplace/programs/certified/).

Dieser Artikel bietet eine Übersicht zur Verwendung einer CSDL kompatiblen OData-Dienst einen vorhandenen Dienst zuzuordnen. Wie das Zuordnungsdokument (CSDL) erstellen die Eingabe Anforderung vom Client über einen Serviceeinsatz transformiert und die Ausgabe (Daten) zurück an den Client über einen kompatiblen OData feed erklärt. Microsoft Azure Marketplace macht Dienste Endbenutzer über das OData-Protokoll. Dienste werden von Anbietern (Datenbesitzer) werden in verschiedenen Formen wie REST, SOAP verfügbar gemacht.

## <a name="what-is-a-csdl-and-its-structure"></a>Was ist ein CSDL und seine Struktur?
CSDL (konzeptionelle Schemadefinitionssprache) ist eine Spezifikation definieren Webdienst oder Datenbankdienst in allgemeine XML-Wortlaut Azure Marketplace beschreiben.

Einfache Übersicht über die **Fluss anfordern:**

  `Client -> Azure Marketplace -> Content Provider’s Web Service (Get, Post, Delete, Put)`

Der **Datenfluss** wird in entgegengesetzter Richtung:

  `Client <- Azure Marketplace <- Content Provider’s WebService`

**Abbildung 1** Diagramme wie ein Client Daten vom Inhaltsanbieter (Service) erhalten würden mit Azure Marketplace.  CSDL von Mapping-Transformation Komponente verwendet, um die Anfrage und Daten zwischen der Inhaltsanbieter Dienste und dem anfordernden Client übergeben.

*Abbildung 1: Ausführliche Datenfluss vom anfordernden Client Inhaltsanbieter über Azure Marketplace*

  ![Zeichnen](media/marketplace-publishing-data-service-creation-odata-mapping/figure-1.png)

Hintergrundinformationen zu Atom Atom Pub und dem Azure Marketplace Extensions aufbauen, OData-Protokoll Bitte überprüfen: [http://msdn.microsoft.com/library/ff478141.aspx](http://msdn.microsoft.com/library/ff478141.aspx)

Auszug von oben links:     *"der Open Data Protocol (nachstehend OData) soll ein REST-basiertes Protokoll für CRUD-Stil-Vorgänge (Create, Read, Update und Delete) Ressourcen als Data Services bereitstellen. "Data Service" wird ein Endpunkt besteht aus mindestens "Sammlungen" jede mit NULL oder mehr "Posten", die aus Daten eingegeben Namen-Wert-Paaren. OData ist von Microsoft veröffentlichten OASIS (Organization for die Advancement of Structured Information Standards) Standards, damit jeder möchte Server, Clients oder ohne Lizenzgebühren oder Einschränkung erstellen kann."*

### <a name="three-critical-pieces-that-have-to-be-defined-by-the-csdl-are"></a>Drei wichtige Teile, die von der CSDL definiert werden:

- Der **Endpunkt** von der Service Provider der Web Adresse (URI) des Dienstes
- Eingabe der übergebenen **Parameter** die Definitionen der Parameter gesendeten Service auf den Datentyp des Inhaltsanbieters.
- **Schema** der Dienst fordert das Schema der Daten durch den Inhaltsanbieter Service, einschließlich Container, Sammlungen-Tabellen, Variablen und Spalten und Datentypen zurückgegeben werden.

Das folgende Diagramm zeigt eine Übersicht der Ablauf, bei dem der Client OData-Anweisung (Aufruf des Inhaltsanbieters Webdienst) zum Abrufen der Ergebnisdaten eingibt.

  ![Zeichnen](media/marketplace-publishing-data-service-creation-odata-mapping/figure-2.png)

### <a name="steps"></a>Schritte:

1. Client sendet Anforderung über Serviceeinsatz mit Eingabeparameter in XML Azure Marketplace definiert
2. CSDL zum Überprüfen der Webdienstaufruf.
    - Formatiert Servicevorgangs Content Provider Service von Azure Marketplace zurückgesendet
3. Der Webdienst führt und die Aktion das HTTP-Verb vorgenommen (z. B. GET) zurückgegebenen Daten Azure Marketplace, wo die angeforderten Daten (sofern vorhanden) macht im XML-Format an den Client werden, in CSDL definierte Zuordnung verwenden.
4. Der Client ist die Daten (sofern vorhanden) in XML oder JSON-Format gesendet

## <a name="definitions"></a>Definitionen

### <a name="odata-atom-pub"></a>OData ATOM pub

Legen Sie eine Erweiterung ATOM Pub, in dem jeder Eintrag eine Datenzeile darstellt. Inhaltsteil des Eintrags erweitert die Werte der Zeile – als Schlüssel-Wert-Paare enthalten. Weitere Informationen finden Sie hier: [https://www.odata.org/documentation/odata-version-3-0/atom-format/](https://www.odata.org/documentation/odata-version-3-0/atom-format/)

### <a name="csdl---conceptual-schema-definition-language"></a>CSDL - Conceptual Schema Definition Language

Ermöglicht das Definieren von Funktionen (Routeninformationen) und Entitäten, die über eine Datenbank verfügbar gemacht werden. Weitere Informationen finden Sie hier: [http://msdn.microsoft.com/library/bb399292.aspx](http://msdn.microsoft.com/library/bb399292.aspx)  

> [AZURE.TIP] Klicken Sie auf die Dropdownliste **andere Versionen** , und wählen Sie eine Artikel angezeigt.

### <a name="edm---entry-data-model"></a>EDM - Eintrag Datenmodell
- Übersicht: [http://msdn.microsoft.com/library/vstudio/ee382825 (v=vs.100).aspx][OverviewLink] [OverviewLink]: http://msdn.microsoft.com/library/vstudio/ee382825 (v=vs.100).aspx
- Vorschau: [http://msdn.microsoft.com/library/aa697428 (v=vs.80).aspx][PreviewLink] [PreviewLink]: http://msdn.microsoft.com/library/aa697428 (v=vs.80).aspx
- Datentypen: [http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx][DataTypesLink] [DataTypesLink]: http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx

Das folgende zeigt detaillierte nach rechts Fluss aus, bei dem der Client OData-Anweisung (Aufruf des Inhaltsanbieters Webdienst) eingibt zum Abrufen der Ergebnisdaten zurück:

  ![Zeichnen](media/marketplace-publishing-data-service-creation-odata-mapping/figure-3.png)


## <a name="csdl-basics"></a>CSDL-Grundlagen

CSDL (konzeptionelle Schemadefinitionssprache) ist eine Spezifikation definieren Webdienst oder Datenbankdienst in allgemeine XML-Wortlaut Azure Marketplace beschreiben. CSDL beschreibt die Teile, die **ermöglicht die Übergabe von Daten aus der Datenquelle zu Azure Marketplace.** Die wichtigsten Teile werden hier beschrieben:

- Informationen zu allen öffentlich verfügbare Funktionen (FunctionImport Knoten) Schnittstelle
- Datentypinformationen für alle Nachrichten requests(input) und Meldung responses(outputs) (EntitySet/EntityContainer/EntityType Knoten)
- Informationen über das Transportprotokoll zu binden (Header Knoten) verwendet
- Adressinformationen für die Suche nach dem angegebenen Dienst (BaseURI Attribut)

Kurz gesagt, stellt CSDL Vertrag unabhängig von Plattform und Sprache zwischen Service Requestor und Dienstanbieter. Mit der CSDL, einem Client Web Service-Datenbank suchen und Aufrufen seiner öffentlich verfügbaren Funktionen.

### <a name="relating-a-csdl-to-a-database-or-a-collection"></a>Eine Datenbank oder eine Auflistung zur einer CSDL
**Die CSDL-Spezifikation**

CSDL ist XML-Grammatik zur Beschreibung eines Webdienstes. Die Spezifikation selbst 4 Hauptelemente unterteilt: EntitySet, FunctionImport; NameSpace und EntityType.

Diese Abstraktion ermöglicht verstehen erleichtern eine CSDL auf eine Tabelle beziehen.

Beachten Sie.

  CSDL stellt einen Vertrag unabhängig von Plattform und Sprache zwischen **Service Requestor** und **Dienstanbieter**. CSDL, **Client** kann ein **Webdienst Service-Datenbank** suchen und Aufrufen eines öffentlich zugänglichen **Funktionen.**

Für einen Datendienst können vier Teile einer CSDL eine Datenbank, Tabelle, Spalte und gespeicherte Prozedur vorstellen.

Bezug für einen Datendienst folgendermaßen:

- EntityContainer ~ = Datenbank
- EntitySet ~ = Tabelle
- EntityType ~ = Spalten
- FunctionImport ~ = gespeicherte Prozedur

**HTTP-Verben zulässig**
- ERHALTEN Sie gibt Werte aus der Datenbank (eine Auflistung zurückgibt)
- POST-Daten und optionale Rückgabewerte aus der Datenbank (Erstellen eines neuen Eintrags in der Auflistung zurückgeben Id-URI) übergeben
- Löschen Sie – löscht aus der Datenbank (löscht eine Auflistung)
- Speichern – Aktualisieren von Daten in einer Datenbank (eine Auflistung ersetzen oder erstellen)

## <a name="metadatamapping-document"></a>Dokument-Metadaten-Zuordnung

Dokument-Metadaten-Zuordnung dient zum vorhandenen Webdiensten des Inhaltsanbieters, damit es als Webdienst OData Azure Marketplace-System verfügbar gemacht werden kann. Es basiert auf CSDL und implementiert einige Erweiterungen CSDL REST Bedürfnisse Webdienste über Azure Marketplace. Die Extensions befinden sich im [http://schemas.microsoft.com/dallas/2010/04](http://schemas.microsoft.com/dallas/2010/04) -Namespace.

Ein Beispiel der CSDL: (Kopieren und Einfügen die folgenden Beispiel CSDL in einem XML-Editor und ändern den Dienst.  Fügen Sie in CSDL Zuordnung DataService Registerkarte beim Erstellen des Diensts in [Azure Marketplace Veröffentlichungsportal](https://publish.windowsazure.com)).

**Begriffe:** Im Zusammenhang mit der CSDL Begriffe der [Veröffentlichungsportal](https://publish.windowsazure.com) UI (PPUI).
- Bieten Sie "Titel" in der PPUI MyWebOffer betrifft
- MyCompany in der PPUI bezieht sich auf **Publisher Anzeigenamen** im [Microsoft Developer Center](http://dev.windows.com/registration?accountprogram=azure) UI
- Ihre API bezieht sich auf ein Web oder ein Datendienst (Plan in der PPUI)

**Hierarchie:** 
 Unternehmen (Content Provider) besitzt Angebot(e) die Pläne, nämlich die Linie mit einer API Dienste.

### <a name="webservice-csdl-example"></a>WebService CSDL-Beispiel

Verbindet mit einem Dienst, der einen Anwendungsendpunkt Web (z. B. eine C#-Anwendung) verfügbar machen

        <?xml version="1.0" encoding="utf-8"?>
        <!-- The namespace attribute below is used by our system to generate C#. You can change “MyCompany.MyOffer” to something that makes sense for you, but change “MyOffer” consistently throughout the document. -->
        <Schema Namespace="MyCompany.MyWebOffer" Alias="MyOffer" xmlns="http://schemas.microsoft.com/ado/2009/08/edm" xmlns:d="http://schemas.microsoft.com/dallas/2010/04" >
        <!-- EntityContainer groups all the web service calls together into a single offering. Every web service call has a FunctionImport definition. -->
          <EntityContainer Name="MyOffer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
        @Name is used as reference by FunctionImport @EntitySet. And is used in the customer facing UI as name of the Service.
        @EntityType is used to point at the type definition near the bottom of this file. -->
            <EntitySet Name="MyEntities" EntityType="MyOffer.MyEntityType" />
        <!-- Add a FunctionImport for every service method. Multiple FunctionImports can share a single return type (EntityType). -->
        <!-- ReturnType is either Raw() for a stream or Collection() for an Atom feed. Ex. of Raw: ReturnType=”Raw(text/plain)” -->
        <!—EntitySet is the entityset defined above, and is needed if ReturnType is not Raw -->
        <!-- BaseURI attribute defines the service call, replace & with the encode value (&amp;).
        In the input name value pairs {param} represents passed in value.
        Or the value can be hard coded as with AccountKey. -->
        <!-- AllowedHttpMethods optional (default = “GET”), allows the CSDL to specifically specify the verb of the service, “Get”, “Post”, “Put”, or “Delete”. -->
        <!-- EncodeParameterValues, True encodes the parameter values, false does not. -->
        <!-- BaseURI is translated into an URITemplate which defines how the web service call is exposed to marketplace customers.
        Ex. https://api.datamarket.azure.com/mycompany/MyOfferPlan?name={name}
        BaseURI is XML encoded, the {...} point to the parameters defined below.
        Marketplace will read the parameters from this URITemplate and fill the values into the corresponding parameters of the BaseUri or RequestBody (below) during calls to your service.  
        It is okay for @d:BaseUri to include information only for Marketplace consumption, it will not be exposed to end users. i.e. the hardcoded AccountKey in the above BaseURI does not show up in the client facing URITemplate. -->
            <FunctionImport Name="MyWebServiceMethod"
                            EntitySet="MyEntities"
                            ReturnType="Collection(MyOffer.MyEntityType)"
        d:AllowedHttpMethods="GET"
        d:EncodeParameterValues="true"
        d:BaseUri="http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <!-- Definition of the RequestBody is only required for HTTP POST requests and is optional for HTTP GET requests. -->
        <d:RequestBody d:httpMethod="POST">
                <!-- Use {} for placeholders to insert parameters. -->
                <!-- This example uses SOAP formatting, but any POST body can be used. -->
            <!-- This example shows how to pass userid and password via the header -->
                <![CDATA[<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:MyOffer="http://services.organization.net/MyServicePath">
                  <soapenv:Header/>
                  <soapenv:Body>
                    <MyOffer:ws_MyWebServiceMethod>
                      <myWebServiceMethodRequest>
                        <!--This information is not exposed to end users. -->
                        <UserId>userid</UserId>
                        <Password>password</Password>
                        <!-- {name} is replaced with the value read from @d:UriTemplate above -->
                        <Name>{name}</Name>
                        <!-- Parameters can be used more than once and are not limited to appearing as the value of an element -->
                        <CustomField Name="{name}" />
                        <MyField>Static content</MyField>
                      </myWebServiceMethodRequest>
                    </MyOffer:ws_MyWebServiceMethod>
                  </soapenv:Body>
                </soapenv:Envelope>      
              ]]>
        </d:RequestBody>
        <!-- Title, Rights and Description are optional and used to specify values to insert into the ATOM feed returned to the end user.  You can specify the element to contain a fixed message by providing a value for the element (this is the default value).  @d:Map is an XPath expression that points into the response returned by your service and is optional.  -->
        <d:Title d:Map="/MyResponse/Title">Default title.</d:Title>
        <d:Rights>© My copyright. This is a fixed response. It is okay to also add a d:Map attribute to override this text.</d:Rights>
        <d:Description d:Map="/MyResponse/Description"></d:Description>
        <d:Namespaces>
        <d:Namespace d:Prefix="p"  d:Uri="http://schemas.organization.net/2010/04/myNamespace" />
        <d:Namespace d:Prefix="p2" d:Uri="http://schemas.organization.net/2010/04/MyNamespace2" />
        </d:Namespaces>
        <!-- Parameters of the web service call:
        @Name should match exactly (case sensitive) the {…} placeholders in the @d:BaseUri, @d:UriTemplate, and d:RequestBody, i.e. “name” parameter in above BaseURI.
        @Mode is always "In", compatibility with CSDL
        @Type is the EDM.SimpleType of the parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx
        @d:Nullable indicates whether the parameter is required.
        @d:Regex - optional, attribute to describe the string, limiting unwanted input at the entry of the system
        @d:Description - optional, is used by Service Explorer as help information
        @d:SampleValues - optional, is used by Service Explorer as help information. Multiple Sample values are separated by '|', e.g. "804735132|234534224|23409823234"
        @d:Enum - optional for string type. Contains an enumeration of possible values separated by a '|', e.g. d:enum="english|metric|raw". Will be converted in a dropdown list in the Service Explorer.
        -->
        <Parameter name="name" Mode="In" Type="String" d:Nullable="false" d:Regex="^[a-zA-Z]*$" d:Description="A name that cannot contain any spaces or non-alpha non-English characters"
        d:Enum="George|John|Thomas|James"
        d:SampleValues="George"/>
        <Parameter Name=" AccountKey" Mode="In" Type="String" d:Nullable="false" />

        <!-- d:ErrorHandling is an optional element. Use it define standardized errors by evaluating the service response. -->
        <d:ErrorHandling>
        <!-- Any number of d:Condition elements are allowed, they are evaluated in the order listed.
        @d:Match is an Xpath query on the service response, it should return true or false where true indicates an error.
        @d:httpStatusCode is the error code to return if an response matches the error.
        @d:errorMessage is the user friendly message to return when an error occurs.
        -->
        <d:Condition d:Match="/Result/ErrorMessage[text()='Invalid token']" d:HttpStatusCode="403" d:ErrorMessage="User cannot connect to the service." />
        </d:ErrorHandling>
           </FunctionImport>

            <!-- The EntityContainer defines the output data schema -->
        </EntityContainer>
        <!-- The EntityType @d:Map defines the repeating node (an XPath query) in the response (output data schema). -->
        <!-- If these nodes are outside a namespace, add the prefix in the xpath. -->
        <!--
        @Name - define your user readable name, will become an XML element in the ATOM feed, so comply with the XML element naming restrictions (no spaces or other illegal characters).
        @Type is the EDM.SimpleType of the parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx.
        @d:Map uses an Xpath query to point at the location to extract the content from your services response.
        The "." is relative to the repeating node in the EntityType @d:Map Xpath expression.
        -->
            <EntityType Name="MyEntityType" d:Map="/MyResponse/MyEntities">
        <Property Name="ID" d:IsPrimaryKey="True" Type="Int32"  Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="Amount" Type="Double"   Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="City"   Type="String"   Nullable="false" d:Map="./City"/>
        <Property Name="State"  Type="String"   Nullable="false" d:Map="./State"/>
        <Property Name="Zip"    Type="Int32"    Nullable="false" d:Map="./Zip"/>
        <Property Name="Updated"    Type="DateTime" Nullable="false" d:Map="./Updated"/>
        <Property Name="AdditionalInfo" Type="String" Nullable="true"
        d:Map="./Info/More[1]"/>
            </EntityType>
        </Schema>

> [AZURE.TIP] Beispiele Weitere CSDL-Webdienst im Artikel [Zuordnen eines vorhandenen Webdiensts OData über CSDLs-Beispiele](marketplace-publishing-data-service-creation-odata-mapping-examples.md)

###<a name="dataservice-csdl-example"></a>DataService CSDL-Beispiel

Verbindet mit einem Dienst, der eine Tabelle oder Ansicht offen legt als ein Endpunkt unter Beispiel zwei APIs für Datenbank-API CSDL basiert zeigt (Sichten anstelle von Tabellen können).

        <?xml version="1.0"?>
        <!-- The namespace attribute below is used by our system to generate C#. You can change “MyCompany.MyOffer” to something that makes sense for you, but change “MyOffer” consistently throughout the document. -->
        <Schema Namespace="MyCompany.MyDataOffer" Alias="MyOffer" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ado/2009/08/edm">
        <!-- EntityContainer groups all the data service calls together into a single offering. Every web service call has a FunctionImport definition. -->
        <EntityContainer Name="MyOfferContainer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
            Think of the EntitySet as a Service
        @Name is used in the customer facing UI as name of the Service.
        @EntityType is used to point at the type definition (returned set of table columns). -->
        <EntitySet Name="CompanyInfoEntitySet" EntityType="MyOffer.CompanyInfo" />
        <EntitySet Name="ProductInfoEntitySet" EntityType="MyOffer.ProductInfo" />
        </EntityContainer>
        <!-- EntityType defines result (output); the table (or view) and columns to be returned by the data service.)
            Map is the schema.tabel or schema.view
            dals.TableName is the table Name
            Name is the name identifier for the EntityType and the Name of the service exposed to the client via the UI.
            dals:IsExposed determines if the table schema is exposed (generally true).
            dals:IsView (optional) true if this is based on a view rather than a table
            dals:TableSchema is the schema name of the table/view
        -->
        <EntityType
        Map="[dbo].[CompanyInfo]"
        dals:TableName="CompanyInfo"
        Name="CompanyInfo"
        dals:IsExposed="true"
        dals:IsView="false"
        dals:TableSchema="dbo"
        xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <!-- Property defines the column properties and the output of the service.
            dals:ColumnName is the name of the column in the table /view.
            Type is the emd.SimpleType
            Nullable determines if NULL is a valid output value
            dals.CharMaxLenght is the maximum length of the output value
            Name is the name of the Property and is exposed to the client facing UI
            dals:IsReturned is the Boolean that determines if the Service exposes this value to the client.
            IsQueryable is the Boolean that determines if the column can be used in a database query
            (For data Services: To improve Performance make sure that columns marked ISQueryable=”true” are in an index.)
            dals:OrdinalPosition is the numerical position x in the table or the View, where x is from 1 to the number of columns in the table.
            dals:DatabaseDataType is the data type of the column in the database, i.e. SQL data type dals:IsPrimaryKey indicates if the column is the Primary key in the table/view.  (The columns marked ISPrimaryKey are used in the Order by clause when returning data.)
        -->
        <Property dals:ColumnName="data" Type="String" Nullable="true" dals:CharMaxLength="-1" Name="data" dals:IsReturned="true" dals:IsQueryable="false" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="ticker" Type="String" Nullable="true" dals:CharMaxLength="10" Name="ticker" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        <EntityType Map="[dbo].[ProductInfo]" dals:TableName="ProductInfo" Name="ProductInfo" dals:IsExposed="true" dals:IsView="false" dals:TableSchema="dbo" xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <Property dals:ColumnName="companyid" Type="Int32" Nullable="true" Name="companyid" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="product" Type="String" Nullable="true" dals:CharMaxLength="50" Name="product" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        </Schema>

## <a name="see-also"></a>Siehe auch
- Wenn Sie lernen und Verstehen der bestimmten Knoten und deren Parameter lesen [Dienst OData Zuordnung Datenknoten](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) für Definitionen, Erläuterung und Beispiele und Case Kontext.
- Wenn Sie beispielsweise Lesen [Daten OData Zuordnung Beispiele](marketplace-publishing-data-service-creation-odata-mapping-examples.md) finden Beispielcode und Codesyntax Kontext überprüfen.
- Um wieder den vorgeschriebenen Pfad zum Veröffentlichen von einem Datendienst Azure Marketplace dieser Artikel [Veröffentlichen Servicehandbuch Daten](marketplace-publishing-data-service-creation.md).
