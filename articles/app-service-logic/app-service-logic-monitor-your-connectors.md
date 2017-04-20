<properties
    pageTitle="Verwalten und Überwachen der Connectors und API-Apps in App Service | Microsoft Azure"
    description="Anzeigen der Leistung der Connectors und API-Apps in Logik-Apps; Microservices-Architektur"
    services="app-service\logic"
    documentationCenter=".net,nodejs,java"
    authors="MandiOhlinger"
    manager="anneta"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="mandia"/>

# <a name="manage-and-monitor-your-built-in-api-apps-and-connectors"></a>Verwalten Sie und überwachen Sie die integrierte API-Apps und Connectors

>[AZURE.NOTE] Diese Version des Artikels gilt für Logik apps 2014-12-01-Vorschau-Schemaversion.

Eine integrierte API-App erstellt. Und was jetzt?

In Azure ist jeder API-App eine separate Website in Azure. Daher können Sie leicht erkennen, wie viele Anfragen sind und wie viele Daten vom Connector verwendet wird. Sie können auch Sichern Ihrer App API Warnregeln erstellen, Folie Sicherheit aktivieren und Benutzer und Rollen hinzufügen.

Dieses Thema beschreibt einige der Optionen zum Verwalten Ihrer App API.

Um diese Funktionen anzuzeigen, öffnen Sie Ihre App API in [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040). Wenn Ihr Startmenü API-App ist, wählen sie Eigenschaften. Sie können auch auswählen, **Durchsuchen**, wählen Sie **API-Apps**und dann Ihre App API:

![][browse]

## <a name="see-the-properties-you-entered"></a>Anzeigen Sie die eingegebenen Eigenschaften

Beim Öffnen der API-App stehen mehrere Funktionen und Aufgaben:

![][settings]

Sie können:

- **Zeigt bestimmte Informationen zu API-App, einschließlich Ihre Abonnementdetails und listet die Benutzer mit Zugriff auf die API-app.** Sie können auch erhöhen oder verringern die Anzahl der Instanzen der App-API-Funktion skalieren; unter anderem.
- Verwenden Sie die Schaltflächen **Start** und **Stop** API App gesteuert.
- Bei Produkt-Updates mit den zugrunde liegenden Dateien durch Ihre App API können **Updates** zu den neuesten Versionen klicken. Beispielsweise besteht ein Patch oder ein Sicherheitsupdate von Microsoft veröffentlicht, aktualisiert automatisch **Aktualisieren** auf Ihre App-API, um dieses Update enthalten.
- Wählen Sie Upgrade oder Downgrade anhand Ihrer Datenverwendung der API-App **Plan ändern** . Diese Funktion können Sie Ihre Daten Verwendung finden Sie unter.
- Wenn Sie einen Connector erstellen, Tabellen, wie SQL-Connector können Sie optional einen Tabellennamen Verbindung eingeben. Ein Schema basierend auf der Tabelle wird automatisch erstellt und ist **Schemas laden**klicken. Heruntergeladene Schema können Sie um eine Transformation oder eine Karte zu erstellen.

## <a name="change-your-connector-or-api-configuration-values-you-entered"></a>Ändern Sie Anschluss oder API-Konfigurationswerte

Konfiguriert oder erstellt Connector erstellt können eingegebenen Werte ändern. Z. B. Wenn Sie SQL-Connector konfiguriert, und SQL Server-Namen oder Tabellennamen ändern möchten, möglich diese API App Blatt des Connectors.

Schritte umfassen:

1. Öffnen Sie die Connectors oder API-App. Dabei wird die API App Blatt geöffnet.
2. **Essentials**klicken Sie auf den Hyperlink unter Host-Eigenschaft. Der Hyperlink wird etwas wie *Slackconnector* oder *microsoftsqlconnector123*benannt:

    ![][apiapphost]

3. **Wählen Sie Blatt API App Host.** Wählen Sie im Blatt Einstellungen **Application Settings**. Die Werte sind unter **Appeinstellungen**aufgeführt:

    ![][hostsettings]

4. Klicken Sie auf die Einstellung zu ändern, geben den neuen Wert, und **Speichern Sie** die Änderung.


## <a name="install-the-hybrid-connection-manager---optional"></a>Installieren der Hybrid Verbindungs-Manager - Optional

![][hcsetup]

Der Hybrid-Verbindungs-Manager können auf einem lokalen System wie SQL Server oder SAP verbinden. Diese Hybrid-Konnektivität verwendet Azure Service Bus, und die Sicherheit zwischen Azure Ressourcen und lokalen Ressourcen steuern.

Siehe [Verwenden der Hybrid-Verbindungs-Manager in Azure App Service](app-service-logic-hybrid-connection-manager.md).

> [AZURE.NOTE] Hybrid-Verbindungs-Manager ist erforderlich, wenn Sie eine lokale Ressource Ihre Firewall herstellen. Wenn Sie nicht auf einem lokalen System verbinden, kann der Hybrid-Verbindungs-Manager nicht in der Connector-Blade aufgeführt.

## <a name="monitor-the-performance"></a>Überwachen der Leistung
Performance-Kennzahlen integriert und enthalten alle API-App erstellen. Diese Metriken sind Ihre API-App in Azure gehostet. Beispiel-Kennzahlen:

![][monitoring]

Sie können:

- Wählen Sie **Anfragen und Fehler** unterschiedliche Performance-Werte einschließlich allgemein bekannten HTTP-Fehlercodes wie 200, 400 und 500 HTTP-Statuscodes. Sie können auch finden Sie Reaktionszeiten, sehen, wie viele App API angefordert werden und wie viele Daten und wie viele Daten. Auf Grundlage der Leistungsmetrik, schaffen Sie e-Mail Alerts Wenn eine Metrik Ihrer Wahl überschreitet.
- **Verwendung**sehen wie viel **CPU** von der API-App verwendet, überprüfen Sie das aktuelle **Nutzungskontingent** in MB und sehen Ihre maximale Nutzung auf Ihre Kosten. **Geschätzte Ausgaben** unterstützen Sie die potenziellen Kosten für Ihre API App bestimmen.
- Wählen Sie **Prozesse** Process Explorer öffnen. Zeigt Web-Instanzen und zugehörigen Eigenschaften Thread Count und Speicherverwendung.

Mithilfe dieser Tools können Sie ermitteln, ob App Service-Plan vergrößert oder, basierend auf Ihren Bedarf verkleinert. Diese Features sind keine zusätzlichen Tools in das Portal integriert.

## <a name="control-the-security"></a>Steuern der Sicherheits

API-Apps mithilfe rollenbasierter Sicherheit. Diese Rollen gelten für die gesamte Azure Erfahrung, einschließlich API-Apps und andere Azure-Ressourcen. Die Rollen beinhalten:

Rolle | Beschreibung
--- | ---
Besitzer | Haben Sie vollen Zugriff auf die Verwaltungsfunktionen und anderen Benutzern oder Gruppen gewähren Sie Zugriff.
Teilnehmer | Haben Sie vollen Zugriff auf die Verwaltungsfunktionen. Kann nicht auf andere Benutzer oder Gruppen Zugriff.
Reader | Alle Ressourcen außer Geheimnisse können angezeigt werden.
Benutzeradministrator-Zugriff | Können alle Ressourcen, Rollen erstellen und managen, und Supportanfragen erstellen und managen.

[Rollenbasierte Zugriffskontrolle in Microsoft Azure-Portal](../active-directory/role-based-access-control-configure.md)anzeigen

Sie können einfach Benutzer hinzufügen und bestimmte Rollen zuweisen API App. Das Portal zeigt die Benutzer, die Zugriff und Rolle:

![][access]  

- Wählen Sie **Benutzern** eine Rolle zuweisen, Benutzer hinzufügen und Entfernen eines Benutzers.
- **Rollen** an alle Benutzer einer bestimmten Rolle auswählen eine Rolle einen Benutzer hinzufügen und Entfernen eines Benutzers aus einer Rolle.


## <a name="more-good-stuff"></a>Mehr gute
- Wählen Sie **API-Definition** für Ihre spezifischen API-Datei stolz automatisch öffnen.
- Wählen Sie **abhängig** von der API-App erforderlichen Dateien anzeigen. Wenn Sie SAP-Connector verwenden, installieren Sie Dateien auf den lokalen Hybrid-Verbindungs-Manager. Diese Faktoren werden in der API-app Blade angezeigt.

>[AZURE.IMPORTANT] Wenn die API-app-Eigenschaften öffnen und sehen Sie unter **Essentials**sind **Host-** und **Gateway-** Links, die neue öffnen:
>
> ![][host]
>
>Diese Eigenschaften sind spezifisch für die Website, die die API App hostet. Bei Verwendung eines integrierten API App oder Connector nicht wirklich die meisten dieser Eigenschaften anwenden, und wir empfehlen, dass Sie diese Eigenschaften nicht aktualisieren. Wenn Sie API App in Visual Studio erstellt und Abonnements Azure bereitgestellt, können Sie Host- und Gateway-Blades verwenden. <br/><br/>


>[AZURE.NOTE] Gehen Sie zunächst Logik Apps vor der Anmeldung für ein Azure-Konto [versuchen Logik](https://tryappservice.azure.com/?appservice=logic)App. Sie können eine kurzlebige Starter Logik-app erstellen. Keine Kreditkarten erforderlich und keine Zusagen.

## <a name="read-more"></a>Lesen Sie mehr

[Überwachen von Logik-Apps](app-service-logic-monitor-your-logic-apps.md)<br/>
[Anschlüsse und API-Apps-Liste in App Service](app-service-logic-connectors-list.md)<br/>
[Rollenbasierte Zugriffskontrolle in Microsoft Azure-portal](../active-directory/role-based-access-control-configure.md)<br/>
[Verwenden der Hybrid-Verbindungs-Manager in Azure App Service](app-service-logic-hybrid-connection-manager.md)


<!--Image references-->
[browse]: ./media/app-service-logic-monitor-your-connectors/browse.png
[settings]: ./media/app-service-logic-monitor-your-connectors/settings.png
[hcsetup]: ./media/app-service-logic-monitor-your-connectors/hcsetup.png
[monitoring]: ./media/app-service-logic-monitor-your-connectors/monitoring.png
[access]: ./media/app-service-logic-monitor-your-connectors/access.png
[host]: ./media/app-service-logic-monitor-your-connectors/host.png
[hostsettings]: ./media/app-service-logic-monitor-your-connectors/hostsettings.png
[apiapphost]: ./media/app-service-logic-monitor-your-connectors/apiapphost.png
