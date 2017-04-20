<properties
    pageTitle="Mit Datei-Connector Logik Apps | Microsoft Azure App Service"
    description="Zum Erstellen und konfigurieren Sie die Datei Connector-API-app und in eine Anwendung Logik in Azure App Service verwenden"
    authors="rajeshramabathiran"
    manager="erikre"
    editor=""
    services="logic-apps"
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="rajram"/>

# <a name="get-started-with-the-file-connector-and-add-it-to-your-logic-app"></a>Erste Schritte mit der Datei-Connector und Ihrer Anwendung Logik hinzufügen
>[AZURE.NOTE] Diese Version des Artikels gilt für Logik apps 2014-12-01-Vorschau-Schemaversion.

Verbinden Sie mit einem Dateisystem hochladen, herunterladen und mehr Dateien auf einem Host-Computer. Logik-apps auslösen basierend auf einer Vielzahl von Datenquellen und bieten Anschlüsse und Prozessdaten. Sie können Datei-Connector Workflow- und Prozesskontrolle Geschäftsdaten als Teil dieser Workflows innerhalb einer Anwendung Logik hinzufügen. 

Datei-Connector verwendet Hybrid Connection Manager hybride Konnektivität auf Host.

## <a name="creating-a-file-connector-for-your-logic-app"></a>Erstellen eines Connectors Datei für Ihre Logik ##
Um den Datei-Connector verwenden, müssen Sie zunächst eine Instanz der Datei Connector API-app erstellen. Dies kann folgendermaßen erfolgen:

1.  Öffnen Azure Marketplace mit + neue Option auf der linken Seite der Azure-Portal.
2.  Suchen Sie nach "Connector file".
3.  Wählen Sie **Datei-Connector (Vorschau)** aus den Suchergebnissen aus.
4.  Wählen Sie die Schaltfläche **Erstellen**
5.  Konfigurieren Sie den Datei-Connector wie folgt:  
![][1]

    - **Name** – Geben Sie einen Namen für die Datei-connector
    - **Paket-Einstellung**
        - **Stammordner** - Pfad des Stammordners auf dem Hostcomputer angeben. . D:\FileConnectorTest
        - **Service Bus-Verbindungszeichenfolge** - Service Bus Verbindungszeichenfolge bereitstellen. Sicherstellen, dass Service Bus-Namespace vom Typ Standard und grundlegende Verwendung von Service Bus Relays zulassen.  Service Bus Relay wird zum Hybrid Verbindungs-Manager herstellen.
    - **App Service-Plan** - auswählen oder erstellen einen App Service-plan
    - **Tarif** - Tarif für den Connector auswählen
    - **Ressourcengruppe** - wählen oder erstellen Sie eine Ressourcengruppe, in denen der Connector befinden sollten
    - **Abonnement** - wählen Sie ein Abonnement dieser Connector erstellt werden soll
    - **Position** – wählen Sie den geografischen Standort, möchten die Verbindung bereitgestellt werden,

4. Klicken Sie auf erstellen. Neue Datei-Connector wird erstellt

## <a name="configure-hybrid-connection-manager"></a>Hybrid-Verbindungs-Manager konfigurieren ##
Erstellte API-App-Instanz Durchsuchen der Dashboard.  Dies kann durch Klicken auf Durchsuchen > API-Apps > Wählen Sie den Datei Connector API App.  Hier muss der Hybrid-Verbindungs-Manager konfiguriert werden.
Weitere Informationen zur Konfiguration und Fehlerbehebung der Hybrid-Verbindungs-Manager finden Sie unter [Verwenden der Hybrid-Verbindungs-Manager].

## <a name="using-the-file-connector-in-your-logic-app"></a>Verwenden des Datei-Connectors in Ihrer Anwendung Logik ##
Erstellte API-app jetzt können den Datei Connector als Aktion für Ihre Logik Sie. Dazu müssen Sie:

1.  Erstellen Sie neue Logik app, und wählen Sie die Datei Connector derselben Ressourcengruppe. Anleitung zum [Erstellen einer neuen Applikation Logik].

2.  Öffnen Sie "Trigger und Aktionen" erstellte Logik App Logik apps Designer öffnen und Konfigurieren der Fluss.

3.  Der Datei-Connector wird im Abschnitt "API-Apps in dieser Ressourcengruppe" in der Galerie auf der rechten Seite angezeigt.

4.  Sie können Datei Connector API-app in den Editor auf "Datei-Connector" löschen. Datei-Connector stellt einen Trigger und 4 Aktionen:  
![][5]

6.  Diese macht bestimmte Eigenschaften. Das Bild unten Listet die Eigenschaften des Triggers und Datei Aktion:  
![][6]

7. Sobald diese konfiguriert sind, kann der Auslöser und die Aktion im Datenfluss verwendet werden. Ebenso können andere Aktionen sowie konfiguriert werden.

> [AZURE.NOTE] Datei-Trigger löscht die Datei nach dem erfolgreich aus dem Ordner lesen.

## <a name="file-connector-rest-apis"></a>Datei-Connector REST-APIs ##
Um den Connector außerhalb einer Logik-app verwenden, können vom Connector verfügbaren REST-APIs verwendet werden. Sie diese API-Definitionen durchsuchen-Api App > Ansicht kann -> Datei-Connector. Klicken Sie jetzt auf objektiv-API-Definition im Abschnitt Zusammenfassung an die APIs, die von diesem Connector verfügbar gemacht:  
![][7]

APIs [Datei]Connector-API-Definition finden.

## <a name="do-more-with-your-connector"></a>Möchten Sie den Connector
Der Connector erstellt wurde, können Sie es mit einer Logik app Arbeitsabläufe hinzufügen. Siehe [welche Logik apps?](app-service-logic-what-are-logic-apps.md).

>[AZURE.NOTE] Wenn Sie Azure Logik Apps beginnen, bevor Sie sich für ein Azure-Konto, gehen Sie [versuchen Logik app](https://tryappservice.azure.com/?appservice=logic)sofort eine kurzlebige Starter Logik-app in App Service können Sie erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

Anzeigen der Swagger REST API Reference [Connectors und API-Apps](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Sie können auch die Leistung Statistiken und Kontrolle der Sicherheit an den Anschluss überprüfen. [Verwalten und Überwachen von integrierten API-Apps und Connectors](app-service-logic-monitor-your-connectors.md)anzeigen

<!-- Image reference -->
[1]: ./media/app-service-logic-connector-file/img1.PNG
[5]: ./media/app-service-logic-connector-file/img5.PNG
[6]: ./media/app-service-logic-connector-file/img6.PNG
[7]: ./media/app-service-logic-connector-file/img7.PNG

<!-- Links -->
[Erstellen einer neuen Logik]: app-service-logic-create-a-logic-app.md
[Datei-Connector-API-definition]: https://msdn.microsoft.com/library/dn936296.aspx
[Der Hybrid-Verbindungs-Manager verwenden]: app-service-logic-hybrid-connection-manager.md
