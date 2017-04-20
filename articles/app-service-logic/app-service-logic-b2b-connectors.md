<properties 
    pageTitle="Business to Business Connectors und API-Apps in Logik-Apps | Microsoft Azure" 
    description="Informationen Sie zum Erstellen und Konfigurieren von EDI, EDIFACT AS2 und TPM-Anschlüsse; Microservices-Architektur" 
    services="logic-apps" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="anneta" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="mandia"/> 

# <a name="business-to-business-connectors-and-api-apps"></a>Business to Business Connectors und API-Apps

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Logik Apps umfasst viele BizTalk API-Apps, die auf Integration. Diese API basieren auf Konzepte und Tools in BizTalk Server jedoch stehen jetzt als Teil der Logik-Apps. 

Eine Kategorie von diese API sind Business to Business (B2B) API-Apps. Über diese API B2B, problemlos hinzufügen Partner, Verträge erstellen und führen Sie alle würde lokal mit EDI AS2 und EDIFACT.  

Diese API B2B bieten "Trigger" und "Aktion". Ein Trigger startet eine neue Instanz basierend auf einem bestimmten Ereignis wie dem Eingang einer X12 Nachricht von einem Partner. Eine Aktion ist das Ergebnis nach dem Empfang einer X12 wie Nachricht und senden Sie die Nachricht mit AS2.


## <a name="what-is-a-business-to-business-connector-or-api-apps"></a>Was ist eine Business-to-Business Connector oder API-Apps
Die Funktion Business to Business (B2B) umfasst bestehende, vordefinierten API-Apps, mit denen Unternehmen, Spaltung, Applikationen und Kommunikation mit AS2 EDI und EDIFACT. 

B2B-API-Apps gehören: 

Connectors oder API-Apps | Beschreibung
--- | ---
BizTalk Trading Partner-Management | Eine API-App, die Business-to-Business (B2B) Beziehung mit Partnern und Verträge erstellt. Diese Beziehung nutzen die AS2 EDIFACT und X12 Protokoll.<br/><br/>TPM-API-Anwendung muss die Basis von AS2-Connector und der X12 oder EDIFACT API-Apps. 
AS2-Connector | Ein Connector empfängt und sendet Nachrichten mit AS2-Transport. Der Connector überträgt Daten sicher und zuverlässig über das Internet.
BizTalk EDIFACT | Eine API-App, die empfängt und sendet Nachrichten mit EDIFACT. EDIFACT wird häufig auch als UN/EDIFACT (un-Electronic Data Interchange For Administration, Commerce und Transport) bezeichnet und Branchen verbreitet.
BizTalk-X12 | Eine API-App, die empfängt und sendet Nachrichten über das X12 Protokoll. X12 wird häufig auch als ASC X 12 (Accredited Standards Committee X12) bezeichnet und Branchen verbreitet. 


Diese API können Sie verschiedene EDI messaging Aufgaben abschließen. Z. B. AS2-Connector verwenden, Sie sicher empfangen und senden können verschiedene Arten von Nachrichten (EDI, XML, Flatfile usw.) ein Kunde eine Abteilung in Ihrem Unternehmen wie Human Resources oder jemand, der AS2 verwendet. 

Sie können beliebig viele API-Apps und leicht erstellen erstellen. Sie können auch eine einzelne API App in mehreren Szenarien oder Workflows wiederverwenden.

Dies ist möglich, ohne Code schreiben. Fangen wir an. 


## <a name="requirements-to-get-started"></a>Vorschriften für den Einstieg
Beim Erstellen von B2B-API-Apps sind erforderlichen Ressourcen. Diese Elemente müssen Sie erstellt werden, bevor sie andere API-Apps verwendet werden können. Diese Vorschriften sind: 

Anforderung | Beschreibung
--- | ---
SQL Azure-Datenbank | Speichert B2B-Elemente einschließlich Partner, Schemas, Zertifikate und Vereinbarungen. B2B-API-Apps erfordert einen eigenen Azure SQL-Datenbank. <br/><br/>**Hinweis** Kopieren Sie die Verbindung mit dieser Datenbank.<br/><br/>[Erstellen Sie eine SQL Azure-Datenbank](../sql-database/sql-database-get-started.md)
Azure BLOB-Speicher-container | Speichert Meldungseigenschaften AS2 Archivierung aktiviert ist. AS2-Nachricht Archivierung benötigen, ist ein Speichercontainer nicht erforderlich. <br/><br/>**Hinweis** Wenn Sie die Archivierung aktivieren, kopieren Sie die Verbindungszeichenfolge dieses Blob-Speicher.<br/><br/>[Konteninformationen Azure-Speicher](../storage/storage-create-storage-account.md)
Service Bus-Namespace und seine Schlüsselwerte | Speichert X12 und EDIFACT Batchverarbeitung von Daten. Benötigen Sie keine Batchverarbeitung, ist ein Service Bus-Namespace nicht erforderlich.<br/><br/>**Hinweis** Batchverarbeitung ermöglichen möchten, kopieren Sie diese Werte.<br/><br/>[Erstellen Sie einen Service Bus-Namespace](http://msdn.microsoft.com/library/azure/hh690931.aspx)
TPM-Instanz | BizTalk Trading Partner Management (TPM) Instanz muss eine AS2-Connector und X12 oder EDIFACT API-App erstellen. Beim Erstellen der TPM-API-Anwendung werden die TPM-Instanz erstellen. <br/><br/>**Hinweis** Kennen Sie den Namen der TPM-API-Anwendung. 


## <a name="create-the-api-apps"></a>API-Apps erstellen
B2B-API-Apps können Azure-Portal oder REST-APIs erstellt werden. 


### <a name="create-the-api-apps-using-rest-apis"></a>Erstellen Sie REST-APIs mit API-Apps
[Dokumentation zur Verwendung von REST-APIs.](http://go.microsoft.com/fwlink/p/?LinkId=529766)


### <a name="create-the-b2b-api-apps-in-the-azure-portal"></a>Erstellen Sie B2B-API-Apps in Azure-Portal
In Azure-Portal können Sie B2B-API-Apps erstellen, wenn Logik Apps, Web Apps und Mobile Apps erstellen. Oder erstellen Sie eine mit eigenen. Beides sind leicht auf Ihre Bedürfnisse und Vorlieben abhängig ist. Einige Benutzer bevorzugen B2B-API-Apps zuerst mit bestimmten Eigenschaften erstellen. Dann erstellen Sie Logik Apps-Web Apps-Mobile Apps und fügen Sie der B2B-API-Apps erstellen hinzu.  

Die folgenden Schritte erstellen B2B-API-Apps mit der API-Apps.


#### <a name="create-the-biztalk-trading-partner-management-tpm-api-apps"></a>Erstellen Sie BizTalk Trading Partner Management (TPM) API-Apps

> [AZURE.NOTE] BizTalk Trading Partner Management (TPM) Instanz muss eine AS2-Connector und X12 oder EDIFACT API-App erstellen. Beim Erstellen der TPM-API-Anwendung werden die TPM-Instanz erstellen.

Die folgenden Schritte erstellen Sie TPM-Instanz:

1. Wählen Sie in Azure-Portal Startmenü (Homepage) **Markt**. **API-Apps** Listet alle vorhandenen API-Apps und Connectors. Sie können auch **Suchen** für bestimmte B2B-API-Apps.
2. Wählen Sie **BizTalk Trading Partner-Management**. Wählen Sie das neue Blatt **Erstellen**. 
3. Geben Sie die Eigenschaften: 

    Eigenschaft | Beschreibung
--- | ---
Name | Geben Sie einen beliebigen Namen für die TPM-Instanz. Sie beispielsweise können *AccountsPayableTPM*nennen.
Paket-Einstellung | Geben Sie die ADO.NET **Verbindungszeichenfolge** auf den Azure SQL-Datenbank erstellen. <br/><br/>Beim Kopieren der Verbindungszeichenfolge wird das Kennwort der Verbindungszeichenfolge nicht hinzugefügt. Achten Sie darauf, dass das Kennwort in die Verbindungszeichenfolge eingeben.
App Service-Plan | Zahlungsplan führt. Sie können diese ändern, benötigen Sie mehr oder weniger Ressourcen.
Preisstufe | Schreibgeschützte Eigenschaft, die der Preiskategorie innerhalb Ihrer Azure-Abonnement enthält. 
Ressourcengruppe | Erstellen Sie eine neue oder eine vorhandene Gruppe verwenden. Alle API-Apps und Anschlüsse für Logik Apps, Web Apps und Mobile Apps müssen in derselben Ressourcengruppe sein. <br/><br/>[Ressourcengruppen](../azure-resource-manager/resource-group-overview.md) erklärt diese Eigenschaft. 
Abonnement | Schreibgeschützte Eigenschaft, die Ihr aktuelle Abonnement enthält.
Speicherort | Der Standort, der Ihre Azure Service hostet. 
Fügen zum Startmenü hinzu | Wählen Sie diese B2B-API-Anwendung der Steuerbord (Homepage) hinzufügen.

4. Wählen Sie **Erstellen**. 

Nach dem Erstellen der TPM-API-Anwendung (TPM-Instanz) kann dann AS2-Connector, der X12 bzw. EDIFACT API-Apps erstellen. 


#### <a name="create-the-as2-connector"></a>Erstellen von AS2-connector

1. Wählen Sie in Azure-Portal Startmenü (Homepage) **Markt**. **API-Apps** Listet alle vorhandenen API-Apps und Connectors. Sie können auch **Suchen** für bestimmte B2B-API-Apps.
2. **AS2-Connector**auswählen Wählen Sie das neue Blatt **Erstellen**. 
3. Geben Sie die Eigenschaften: 

    Eigenschaft | Beschreibung
--- | ---
Name | Geben Sie alle Namen von AS2-Connector. Sie beispielsweise können *AS2Connector*nennen.
Paket-Einstellung | Geben Sie die spezifische, App-API wie TPM-Instanz. <br/><br/>Finden Sie in diesem Thema die spezifischen Eigenschaften [AS2 Paketeinstellungen hinzufügen](#AddAS2Conn) . 
App Service-Plan | Zahlungsplan führt. Sie können diese ändern, benötigen Sie mehr oder weniger Ressourcen.
Preisstufe | Schreibgeschützte Eigenschaft, die der Preiskategorie innerhalb Ihrer Azure-Abonnement enthält. 
Ressourcengruppe | Erstellen Sie eine neue oder eine vorhandene Gruppe verwenden. [Ressourcengruppen](../azure-resource-manager/resource-group-overview.md) erklärt diese Eigenschaft. 
Abonnement | Schreibgeschützte Eigenschaft, die Ihr aktuelle Abonnement enthält.
Speicherort | Der Standort, der Ihre Azure Service hostet. 
Fügen zum Startmenü hinzu | Wählen Sie diese B2B-API-Anwendung der Steuerbord (Homepage) hinzufügen.

    **<a name="AddAS2Conn"></a>AS2-Connector Paket Settings**

    Eigenschaft | Beschreibung
--- | --- 
Datenbank-Verbindungszeichenfolge | Geben Sie die Verbindungszeichenfolge ADO.NET der Azure SQL-Datenbank erstellen. Beim Kopieren der Verbindungszeichenfolge wird das Kennwort der Verbindungszeichenfolge nicht hinzugefügt. Müssen Sie das Kennwort in die Verbindungszeichenfolge eingeben einfügen.
Aktivieren der Archivierung für eingehende Nachrichten | Optional. Aktivieren Sie diese Eigenschaft zum Speichern von Nachrichteneigenschaften einer eingehenden AS2-Nachricht von einem Partner erhalten haben. 
Azure BLOB-Speicher-Verbindungszeichenfolge  | Geben Sie die Verbindungszeichenfolge Azure BLOB-Speicher-Container, den Sie erstellt haben. Wenn Archivierung aktiviert ist, werden Nachrichten kodierten und dekodierten in diesem Behälter gespeichert.
TPM-Instanznamen | Geben Sie den Namen der **BizTalk Trading Partner Management** API-App zuvor erstellt. Beim Erstellen von AS2-Connector führt dieser Connector nur AS2 Abkommen in dieser bestimmten Instanz TPM.

4. Wählen Sie **Erstellen**. 


#### <a name="create-the-x12-or-edifact-api-apps"></a>X12 oder EDIFACT API-Apps erstellen

1. Wählen Sie in Azure-Portal Startmenü (Homepage) **Markt**. **API-Apps** Listet alle vorhandenen API-Apps und Connectors. Sie können auch **Suchen** für bestimmte B2B-API-Apps.
2. Wählen Sie **BizTalk X 12** oder **EDIFACT BizTalk**. Wählen Sie das neue Blatt **Erstellen**. 
3. Geben Sie die Eigenschaften: 

    Eigenschaft | Beschreibung
--- | ---
Name | Geben Sie einen Namen für die B2B-API-Anwendung. Sie beispielsweise können *EDI850APIApp*nennen.
Paket-Einstellung | Geben Sie die spezifische, App-API wie TPM-Instanz. <br/><br/>In diesem Thema für die einzelnen Eigenschaften finden Sie unter [X12 oder EDIFACT Paket](#AddX12) . 
App Service-Plan | Zahlungsplan führt. Sie können diese ändern, benötigen Sie mehr oder weniger Ressourcen.
Preisstufe | Schreibgeschützte Eigenschaft, die der Preiskategorie innerhalb Ihrer Azure-Abonnement enthält. 
Ressourcengruppe | Erstellen Sie eine neue oder eine vorhandene Gruppe verwenden. [Ressourcengruppen](../azure-resource-manager/resource-group-overview.md) erklärt diese Eigenschaft. 
Abonnement | Schreibgeschützte Eigenschaft, die Ihr aktuelle Abonnement enthält.
Speicherort | Der Standort, der Ihre Azure Service hostet. 
Fügen zum Startmenü hinzu | Wählen Sie diese B2B-API-Anwendung der Steuerbord (Homepage) hinzufügen.

    **<a name="AddX12"></a>X12 und EDIFACT API-Apps Paket Settings**  

    Eigenschaft | Beschreibung
--- | --- 
Datenbank-Verbindungszeichenfolge | Geben Sie die Verbindungszeichenfolge ADO.NET der Azure SQL-Datenbank erstellen. Beim Kopieren der Verbindungszeichenfolge wird das Kennwort der Verbindungszeichenfolge nicht hinzugefügt. Müssen Sie das Kennwort in die Verbindungszeichenfolge eingeben einfügen.
Service Bus-Namespace | Geben Sie den Service Bus-Namespace erstellten. Nur wenn die Batchverarbeitung aktiviert ist erforderlich. 
Service Bus Namespace Shared Access Schlüsselname | Geben Sie den Service Bus-Namespace Zugriffstaste erstellen. Nur wenn die Batchverarbeitung aktiviert ist erforderlich. 
Bus Namespace freigegebene Zugriffsschlüssel Wert | Geben Sie den Service Bus-Namespace erstellten Schlüssel-Wert. Nur wenn die Batchverarbeitung aktiviert ist erforderlich. 
TPM-Instanznamen | Geben Sie den Namen der **BizTalk Trading Partner Management** API-App zuvor erstellt. Beim Erstellen der X12 oder EDIFACT API-Apps führt diese API App nur die X12-EDFIACT Vereinbarungen innerhalb dieses bestimmten TPM-Instanz.

4. Wählen Sie **Erstellen**. 


## <a name="add-your-partners-agreements-certificates-and-schemas"></a>Ihre Partner, Verträge, Zertifikate und Schemas hinzufügen 
Öffnen Sie in Azure-Portal Ihrer App TPM-API. Fügen Sie im Abschnitt **Components** Ihre Partner, Verträge, Zertifikate und Schemas. 

Sie können auch Verträge die AS2-Connectors X12 hinzufügen API-Apps und EDIFACT API-Apps. 


## <a name="monitor-your-api-apps"></a>Überwachen der API-Apps
Öffnen Sie in Azure-Portal Ihrer App TPM-API. Im Abschnitt mit **Vorgängen** können Sie verschiedene Verwaltungsvorgänge anzeigen. Beispielsweise können Sie:

- Information anzeigen und Fehlerereignisse
- Arbeitsspeicher Verwendung und Thread-Anzahl der Worker-Prozess (w3wp) anzeigen
- Anwendung und Web-Serverprotokolle anzeigen

Auf [Monitor Logik-Apps](app-service-logic-monitor-your-logic-apps.md).


## <a name="add-the-api-apps-to-your-application"></a>API-Apps zu Ihrer Anwendung hinzufügen 
Microsoft Azure App Service stellt verschiedene Anwendungstypen, die diese API B2B verwenden können. Sie können erstellen Sie eine neue oder die vorhandene B2B-API-Apps Logik Apps Mobile Apps und Web-Apps hinzufügen. 

In der App hinzugefügt einfach B2B-API-Apps automatisch aus dem Katalog auswählen Ihrer App.  

> [AZURE.IMPORTANT] Erstellen Sie Anschlüsse und API-Apps, die Sie zuvor erstellt haben hinzuzufügen, Logik-Apps, Mobile Apps oder Web Apps in derselben Ressourcengruppe. 

Die folgenden Schritte hinzufügen Logik-Apps, Mobile Apps und Web Apps B2B-API-Apps: 

1. Gehen Sie in Azure-Portal Startmenü (Homepage) **Marketplace**und suchen Ihre Logik, Mobil oder Web Apps. 

    Wenn Sie eine neue Anwendung erstellen, suchen Sie nach Logik-Apps, Mobile Apps und Web Apps. Wählen Sie die Anwendung und das neue Blatt **Erstellen**. [Erstellen einer Anwendung Logik](app-service-logic-create-a-logic-app.md) führt die Schritte. 

2. Öffnen Sie Ihre Anwendung, und wählen Sie **Auslöser und Aktionen**. 

3. Wählen Sie aus der **Galerie**B2B-API App, automatisch an Ihrer Anwendung hinzugefügt. Sie können auch eine neue B2B-API-Anwendung erstellen.

    > [AZURE.IMPORTANT] AS2-Connector und X12 erfordern EDIFACT API-Apps TPM-Instanz. Wenn also erstellen neue B2B-API-Apps zunächst TPM API-App erstellen AS2-Connector X12 API-App oder EDIFACT API-App. 

4. Wählen Sie **OK** , Ihre Änderungen zu speichern. 

>[AZURE.NOTE] Einstieg mit Azure Logik Apps vor der Anmeldung für ein Azure-Konto [Versuchen Logik Apps](https://tryappservice.azure.com/?appservice=logic). Sie können sofort eine kurzlebige Starter Logik-app erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="more-b2b-resources"></a>Weitere B2B-Ressourcen

[Erstellen eines B2B-Prozess](app-service-logic-create-a-b2b-process.md)<br/>
[Erstellen einer Handelspartnervereinbarung](app-service-logic-create-a-trading-partner-agreement.md)<br/>
[Was sind Connectors und BizTalk API-Apps](app-service-logic-what-are-biztalk-api-apps.md)


## <a name="read-about-logic-apps-and-web-apps"></a>Informieren Sie sich über Logik und webapps
[Was sind Logik Apps?](app-service-logic-what-are-logic-apps.md)<br/>
[Websites und Web-Apps in Azure App Service](../app-service-web/app-service-web-overview.md)


## <a name="more-connectors"></a>Weitere Anschlüsse

[Anschlüsse und API-Apps-Liste](app-service-logic-connectors-list.md)<br/><br/>
[Was sind Connectors und BizTalk API-Apps](app-service-logic-what-are-biztalk-api-apps.md) 
