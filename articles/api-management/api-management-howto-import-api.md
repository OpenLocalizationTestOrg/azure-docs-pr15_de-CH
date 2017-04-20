<properties 
    pageTitle="API-Schlüsselkonzepte" 
    description="Erfahren Sie mehr über APIs, Produkte, Rollen, Gruppen und andere API Schlüsselkonzepte." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-import-the-definition-of-an-api-with-operations-in-azure-api-management"></a>Die Definition einer API mit Azure API Management importieren

API Management neue APIs erstellt werden und die Vorgänge manuell hinzugefügt oder die API und die Vorgänge in einem Schritt importiert werden.

APIs und Vorgänge können über die folgenden Formate importiert werden.

-   WADL
-   Stolz

Dieses Handbuch zeigt wie erstellen Sie eine neue API und Vorgänge in einem Schritt importieren. Informationen über eine API manuell erstellen und Hinzufügen von Operationen finden Sie unter [APIs erstellen][] und [Hinzufügen zu einer API][].

## <a name="import-api"> </a>API importieren

APIs erstellt und im Publisher-Portal konfiguriert. Klicken Sie Publisher-Portal **Verwalten** in Azure-Verwaltungsportal für Ihre API-Verwaltungsdienst. Wenn Sie eine Dienstinstanz API Management noch nicht erstellt haben, finden Sie im Lernprogramm [Erste Schritte mit Azure API Management][] [Erstellen API Management Service][] .

![Publisher-portal][api-management-management-console]

Auf **APIs** der **API** Menü auf der linken Seite und dann auf **API importieren**.

![Import-API][api-management-import-apis]

Das Fenster **Import-API** hat drei Registerkarten, die drei Methoden zum Bereitstellen von API-Spezifikation entsprechen.

-   **Aus Zwischenablage** können API-Spezifikation im angegebenen Textfeld einfügen.
-   **Aus Datei** können Sie auf Durchsuchen und wählen Sie die Datei mit der API-Spezifikation.
-   **Von URL** können Sie die URL der API-Spezifikation angegeben.

![Importformat API][api-management-import-api-clipboard]

Verwenden Sie nach der API-Spezifikation Optionsfelder rechts an der Format Specification. Folgende Formate werden unterstützt.

-   WADL
-   Stolz

Geben Sie ein **Suffix Web API-URL**an. Dies ist die Basis-URL für Ihre API-Verwaltungsdienst hinzugefügt. Die Basis-URL ist für alle APIs auf jeweils eine API Management-Dienst gehostet. API-Management unterscheidet die APIs von dem Suffix und daher das Suffix muss für jede API in eine bestimmte API Management Service-Instanz.

Nachdem alle Werte eingegeben sind, klicken Sie auf **Speichern** , um die API und die zugeordneten Vorgänge erstellen. 

>[AZURE.NOTE] Ein Importieren von einem Rechner API stolz Format finden Sie unter [Verwalten der erste API in Azure API Management](api-management-get-started.md).

## <a name="export-api"></a> Exportieren eine API

Neben neuen APIs importieren, können Sie die Definitionen der APIs von Publisher-Portal exportieren. Klicken Sie hierzu auf **Export-API** auf der **Registerkarte Zusammenfassung** der **API**.

![Export-API][api-management-export-api]

APIs können mit WADL oder stolz exportiert werden. Wählen Sie das gewünschte Format und den Speicherort zum Speichern der Datei klicken Sie auf **Speichern**.

![API-Format exportieren][api-management-export-api-format]

## <a name="next-steps"> </a>Nächste Schritte

API erstellt und importiert Vorgänge können und konfigurieren alle Berechtigungen, die API zu einem Produkt hinzufügen und veröffentlichen, sodass Entwickler verfügbar ist. Weitere Informationen finden Sie in den folgenden Handbüchern.

-   [API-Einstellungen konfigurieren][]
-   [Erstellen und Veröffentlichen eines Produkts][]




[api-management-management-console]: ./media/api-management-howto-import-api/api-management-management-console.png
[api-management-import-apis]: ./media/api-management-howto-import-api/api-management-api-import-apis.png
[api-management-import-api-clipboard]: ./media/api-management-howto-import-api/api-management-import-api-wizard.png
[api-management-export-api]: ./media/api-management-howto-import-api/api-management-export-api.png
[api-management-export-api-format]: ./media/api-management-howto-import-api/api-management-export-api-format.png

[Import an API]: #import-api
[Export an API]: #export-api
[Configure API settings]: #configure-api-settings
[Next steps]: #next-steps

[Erste Schritte mit Azure API Management]: api-management-get-started.md
[Erstellen Sie eine Instanz der API Management service]: api-management-get-started.md#create-service-instance

[Wie eine API Operationen hinzugefügt]: api-management-howto-add-operations.md
[Erstellen und Veröffentlichen eines Produkts]: api-management-howto-add-products.md
[APIs erstellen]: api-management-howto-create-apis.md
[API-Einstellungen konfigurieren]: api-management-howto-create-apis.md#configure-api-settings
