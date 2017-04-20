<properties 
    pageTitle="Was sind Logik Apps?" 
    description="Weitere Informationen zu App Service Logik Apps" 
    authors="kevinlam1" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article" 
    ms.date="10/12/2016"
    ms.author="klam"/>

# <a name="what-are-logic-apps"></a>Was sind Logik Apps?

Logik-Apps können zu vereinfachen und skalierbar Integrationen und Workflows in der Cloud zu implementieren. Es bietet einen visuellen Designer zu modellieren automatisieren als eine Reihe von Schritten als Workflow bezeichnet.  Es gibt [viele Connectors](../connectors/apis-list.md) Cloud und lokale Dienste und Protokolle integrieren.  Logik-app mit einem Auslöser (wie "Wenn ein Konto mit Dynamics CRM hinzugefügt") beginnt und nach dem Brennen viele Kombinationen Aktionen, Umwandlungen und Bedingung Logik beginnen kann.

Folgende: Vorteile bei der Verwendung von Logik-Apps  

- Sparen Sie Zeit durch komplexe Prozesse mit leicht verständlichen Entwurfstools
- Nahtlose Implementierung Muster und Workflows, die andernfalls in Code implementiert wäre schwierig
- Schneller Einstieg in Vorlagen
- Anpassen von Logik-app mit eigenen benutzerdefinierten APIs, Code und Aktionen
- Verbinden Sie und synchronisieren Sie heterogener Systeme vor Ort und die cloud
- Erstellen von BizTalk Server, API Management Azure Funktionen und Azure Service Bus mit erstklassigen Unterstützung

Apps ist eine vollständig verwaltete iPaaS (Integration Plattform als Dienst) Entwickler keine Gedanken hosten, Skalierbarkeit, Verfügbarkeit und Management.  Logik Apps wird automatisch bei Bedarf zu skalieren.

![Datenfluss app-designer](./media/app-service-logic-what-are-logic-apps/LogicAppCapture2.png)

Wie Logik Apps können Sie Geschäftsprozesse automatisieren. Es folgen einige Beispiele:  
 
* Verschieben Sie Dateien auf einem FTP-Server in Azure-Speicher hochgeladen
* Prozess und Route Aufträge über lokale und cloud-Systeme
* Überwacht alle Tweets zu einem bestimmten Thema, die Stimmung analysieren und Alarme und Aufgaben benötigen Nachkontrolle Elemente erstellen.

Szenarien wie diese können von der visuelle Designer und ohne eine einzige Codezeile schreiben konfiguriert werden. Get gestartet [Logik-app jetzt erstellen][create].  -Nach kann eine Anwendung Logik [schnell bereitgestellt und konfiguriert](app-service-logic-create-deploy-template.md) Umgebung und Regionen.

## <a name="why-logic-apps"></a>Warum Logik Apps?

Logik Apps bringt Geschwindigkeit und Erweiterbarkeit in Enterprise Integration Speicherplatz.  Bedienung des Designers Vielzahl von verfügbaren Trigger und Aktionen und leistungsstarke Verwaltungstools stellen Zentralisierung APIs einfacher als je zuvor.  Verschieben von Unternehmen zur Digitalisierung können Logik Apps legacy und innovative Systeme miteinander zu verbinden.

Darüber hinaus mit unseren [Unternehmenskunden Integration] [ biztalk] skalierbar Integrationsszenarios mit einer [XML-messaging]Reifen[xml], [trading Partner-Management][tpm], und vieles mehr.

- **Benutzerfreundliches Designtools** - Logik Apps kann entwickelt End-to-End im Browser oder mit Visual Studio Tools. Trigger - einen einfachen Zeitplan gestartet werden, wenn ein GitHub Problem entsteht. Koordinieren Sie dann eine beliebige Anzahl von Aktionen mit umfangreichen Katalog mit Connectors.

- **Connect-APIs leicht** -auch Komposition Aufgaben einfach zu beschreiben sind schwer in Code implementiert. Logik Apps erleichtert die unterschiedliche Systemen herstellen. Verbindung der Cloud marketing-Lösung zu Ihrem Firmensitz Abrechnungssystem? APIs und Systeme mit Enterprise Service Bus messaging zentralisieren möchten? Logik-apps sind die schnellste und zuverlässigste Möglichkeit Lösungen für diese Probleme.

- **Beginnen von Vorlagen** - Einstieg haben wir eine [Sammlung von Vorlagen] bereitgestellten[ templates] können Sie schnell allgemeinen Lösungen erstellen. Von advanced B2B-Lösung auf einfache SaaS-Konnektivität und sogar einige'spaßeshalber' - Katalog ist der schnellste Weg ins mit Logik-Apps.

- **Erweiterbarkeit baked-in** - Connector Sie müssen nicht sehen? Logik Apps soll mit APIs und Code arbeiten. Sie können problemlos erstellen eigene API-app verwenden Sie als einen benutzerdefinierten Connector oder in einer [Azure Funktion](https://functions.azure.com) Ausschnitte von Code bei Bedarf ausführen. 

- **Echte Integration PS** - Start einfach und wachsen müssen. Logik Apps kann problemlos nutzen die BizTalk Microsoft branchenweit führende integrationslösung für Integration-Experten die Projektmappen erstellen, die sie benötigen. Erfahren Sie mehr über [Enterprise Integration Packs](./app-service-logic-enterprise-integration-overview.md).

## <a name="logic-app-concepts"></a>Logik App-Konzepte

Es folgen einige der wichtigsten, die Logik Apps Erfahrung umfassen. 

- **Workflow** - Logik Apps stellt grafisch als eine Reihe von Schritten oder einen Workflow Geschäftsprozesse modellieren.
- **Verwaltete Connectors** - Logik-apps benötigen Zugriff auf Daten und Dienste. Verwaltete Connectors werden speziell zur Unterstützung beim Herstellen einer Verbindung mit und Arbeiten mit Daten erstellt. Liste der Connectors [Connectors]verwaltet jetzt[managedapis].
- **Trigger** - einige verwaltete Connectors kann auch als Trigger fungieren. Ein Trigger startet eine neue Instanz eines Workflows auf ein bestimmtes Ereignis wie dem Eingang einer E-mail oder eine Änderung in Ihrem Konto Azure-Speicher basiert.
-  **Aktionen** - jedem Trigger in einem Workflow eine Aktion aufgerufen wird. Jede Aktion wird in der Regel eine Operation auf dem verwalteten Connector oder benutzerdefinierte API-apps.
- **Enterprise-Integrationspaket** - enthält für erweiterte Integrationsszenarios Logik Apps Funktionen von BizTalk. BizTalk ist Microsofts branchenweit führenden Integrationsplattform. Enterprise-Integrationspaket Connectors können Sie problemlos Validierung, Transformation und mehr in Ihre Workflows Logik App.

## <a name="getting-started"></a>Erste Schritte  

- Zunächst mit Logik Apps folgen [einer Logik erstellen] [ create] Tutorial.  
- [Ansicht-Beispiele und Szenarien](app-service-logic-examples-and-scenarios.md)
- [Automatisieren Sie Geschäftsprozesse mit Logik Apps](http://channel9.msdn.com/Events/Build/2016/T694) 
- [Integrieren Sie Ihre Systeme mit Logik Apps](http://channel9.msdn.com/Events/Build/2016/P462)

[biztalk]: app-service-logic-enterprise-integration-accounts.md
[appservice]: ../app-service/app-service-value-prop-what-is.md
[create]: app-service-logic-create-a-logic-app.md
[managedapis]: ../connectors/apis-list.md
[tpm]: app-service-logic-enterprise-integration-accounts.md
[xml]: app-service-logic-enterprise-integration-b2b.md
[templates]: app-service-logic-use-logic-app-templates.md
