<properties
   pageTitle="Mit Azure Ressource Connector Logik Apps | Microsoft Azure App Service"
   description="Wie erstellen und Konfigurieren von Azure Ressource Connector oder API-app und in eine Anwendung Logik in Azure App Service verwenden"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="stepsic-microsoft-com"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="09/01/2016"
   ms.author="stepsic"/>

# <a name="get-started-with-the-azure-resource-connector-and-add-it-to-your-logic-app"></a>Erste Schritte mit Azure Ressource Connector und Ihre Anwendung Logik hinzufügen
>[AZURE.NOTE] Diese Version des Artikels gilt für Logik apps 2014-12-01-Vorschau-Schemaversion.

Verwenden Sie Azure Ressourcen zum Verwalten von Azure Ressourcen innerhalb Ihrer Anwendung Logik.

## <a name="create-the-azure-resource-connector"></a>Connector Azure-Ressource erstellen
Um Azure Ressource Connector API-app verwenden, müssen Sie zuerst eine Instanz davon erstellen. Dies kann entweder Inline beim Erstellen einer Anwendung Logik oder app Azure Resource Manager Connector-API von Azure Marketplace erfolgen.

Konfigurieren sie haben Sie auf einen Dienstprinzipal mit Berechtigungen zu tun, was möchten Sie in Azure festgelegt. Alle im Auftrag von diesem Service Principal erfolgt, die Sie haben eingerichtet. Dadurch können Sie den Connector verwenden, nur mit dem gewünschten Bereich, und nichts mehr.

David Ebbo hat [einen hervorragenden Blogbeitrag](http://blog.davidebbo.com/2014/12/azure-service-principal.html) zur Einrichtung dieser geschrieben. Gehen Sie alle vorhanden und Ihre **Mandanten-ID**, **Client-ID** und **Schlüssel**. Diese drei Felder sowie die **Abonnement-ID**werden wie der Connector konfiguriert werden müssen.

## <a name="using-the-azure-resource-connector-in-logic-apps-designer"></a>Verwenden von Azure Ressource Connector Logik apps Designer
### <a name="trigger"></a>Trigger
Es gibt zwei Trigger im Connector unterstützt werden:

Name | Beschreibung
---- | -----------
Ereignis | Trigger beim Eintreten eines Ereignisses zu einer Ressource in Ihrem Abonnement.
Metrik überschreitet Schwellenwert |  Trigger, wenn eine Metrik einen bestimmten Schwellenwert erreicht.

### <a name="action"></a>Aktion

Ebenso können Sie eine große Anzahl von Aktionen innerhalb der Azure-Abonnement bereitstellen:

Für **Ressourcengruppen** können Sie:

Name | Beschreibung
---- | -----------
Liste von Ressourcengruppen | Liste aller Ressourcengruppen im Abonnement.
Ressource Gruppe | Die Id eine Ressourcengruppe abrufen.
Ressourcengruppe erstellen | Erstellen oder Aktualisieren einer Ressourcengruppe.
Löschen von Ressourcengruppe | Löschen einer Ressourcengruppe.

**Ressourcen** können Sie:

Name | Beschreibung
---- | -----------
Liste mit Ressourcen | Liste von Ressourcen in Ihr Abonnement von verschiedenen Filtertypen.
Ressourcen erhalten | Eine einzelne Ressource durch eine Ressourcen-ID abrufen
Erstellen oder Aktualisieren von Ressourcen | Eine Ressource erstellen oder Aktualisieren einer vorhandenen Ressource. Sie müssen alle Eigenschaften für die Ressource angeben.
Ressource-Aktion |  Keine andere Aktion für eine Ressource ausgeführt. Sie müssen den Aktionsnamen und die Nutzlast, die diese Aktion (falls vorhanden) wird.
Ressource löschen | Löscht eine Ressource.

Für **Ressourcen** können Sie:

Name | Beschreibung
---- | -----------
Ressourcenanbietern | Listen Sie aller verfügbaren Ressourcenanbieter im Abonnement auf.

**Gruppe** für Ressourcen können Sie:

Name | Beschreibung
---- | -----------
Bereitstellung von Liste | Liste aller Bereitstellungen in einer Ressourcengruppe.
Bereitstellung abrufen | Die Id eine vorlagenbereitstellung abrufen.
Bereitstellung erstellen | Erstellen Sie eine neue Ressource Bereitstellung durch Bereitstellen einer Vorlage.

**Ereignisse** zu Ressourcen können Sie:

Name | Beschreibung
---- | -----------
Ereignisse | Ereignisse in einem Abonnement oder für eine Ressource zu erhalten.

Für **Metriken** zu Ressourcen können Sie:

Name | Beschreibung
---- | -----------
Abrufen von Metriken | Eine Metrik für eine Ressource ID abzurufen.

## <a name="do-more-with-your-connector"></a>Möchten Sie den Connector
Der Connector erstellt wurde, können Sie es zu einem Unternehmen mit einer Anwendung Logik hinzufügen. Siehe [welche Logik apps?](app-service-logic-what-are-logic-apps.md).

>[AZURE.NOTE] Wenn Sie Azure Logik Apps beginnen, bevor Sie sich für ein Azure-Konto, gehen Sie [versuchen Logik app](https://tryappservice.azure.com/?appservice=logic)sofort eine kurzlebige Starter Logik-app in App Service können Sie erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

Anzeigen der Swagger REST API Reference unter [Connectors und API-apps Verweis](http://go.microsoft.com/fwlink/p/?LinkId=529766).

<!--References -->

<!--Links -->
[Creating a Logic app]: app-service-logic-create-a-logic-app.md
