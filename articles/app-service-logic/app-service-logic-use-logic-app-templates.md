<properties
 pageTitle="Logik App Vorlagen | Microsoft Azure"
 description="Erfahren Sie, wie mithilfe von vorab erstellten Logik app Vorlagen Einstieg"
 authors="kevinlam1"
 manager="dwrede"
 editor=""
 services="app-service\logic"
 documentationCenter=""/>

<tags
    ms.service="app-service-logic"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="klam"/>

# <a name="logic-app-templates"></a>Logik App-Vorlagen

## <a name="what-are-logic-app-templates"></a>Was sind Logik app Vorlagen

Eine Logik app-Vorlage ist eine vorgefertigte Logik-app, mit denen Sie schnell loslegen erstellen eigene Workflows. 

Diese Vorlagen sind eine gute Möglichkeit zum Ermitteln der verschiedenen Mustern mit Logik apps erstellt werden können. Sie können diese Vorlagen- oder sie Ihr Szenario angepasst.

## <a name="overview-of-available-templates"></a>Überblick über die verfügbaren Vorlagen

Gibt es viele derzeit veröffentlichten Logik app-Plattform verfügbaren Vorlagen. Einige Beispiel-Kategorien sowie Anschlusstyp verwendet werden, sind unten aufgeführt.

### <a name="enterprise-cloud-templates"></a>Enterprise Cloud-Vorlagen
Vorlagen, die Dynamics CRM, Salesforce, Feld, Azure Blob und andere Connectors für Ihre Unternehmensbedürfnisse Cloud integrieren. Beispiele was dabei durchgeführt werden können gehören Ihre Leads organisieren und Ihr Unternehmen Daten sichern.

### <a name="enterprise-integration-pack-templates"></a>Enterprise-Integration Pack Vorlagen
Konfigurationen WETER (überprüfen, extrahieren, transformieren, gestalten, weiterleiten) Rohrleitungen empfangen ein X12 EDI über AS2 dokumentieren und in XML sowie X12 und AS2 Message Handling.

### <a name="protocol-pattern-templates"></a>Protokoll Muster Vorlagen
Vorlagen bestehen aus Logik-apps, die Muster wie Anforderung / Antwort-Protokoll über HTTP sowie Integration über FTP und SFTP enthalten. Verwenden Sie diese vorhandenen oder als Grundlage für komplexere Protokoll Muster erstellen.  

### <a name="personal-productivity-templates"></a>Persönliche Produktivität Vorlagen
Muster persönlichen Produktivität steigern enthalten Vorlagen, die tägliche Erinnerung, wichtige Arbeitsaufgaben in Vorgangslisten aktivieren und auf einen einzelnen Benutzer Genehmigungsschritt langwierige Aufgaben automatisieren.

### <a name="consumer-cloud-templates"></a>Cloud-Consumervorlagen
Einfachen Vorlagen, die in sozialen Netzwerken wie Twitter, Puffer und letztlich zur Stärkung des sozialen Marketinginitiativen e-Mail integriert. Dazu gehören auch Vorlagen wie bewölkt kopieren, wodurch Produktivität Zeit traditionell repetitive Aufgaben speichern. 

## <a name="how-to-create-a-logic-app-using-a-template"></a>Eine Logik Anwendung mithilfe einer Vorlage erstellen 

Gehen Sie zunächst eine Logik app Vorlage Logik app Designer. Wenn Sie den Designer öffnen eine vorhandene Logik app eingeben, lädt die Anwendung Logik automatisch in der Entwurfsansicht. Wenn Sie eine neue Logik-app erstellen, sehen Sie folgenden Bildschirm.  
 ![](../../includes/media/app-service-logic-templates/template7.png)  

In diesem Fenster können Sie entweder eine leere Logik-app oder eine vorgefertigte Vorlage starten. Wenn Sie eine Vorlage auswählen, erhalten Sie weitere Informationen. In diesem Beispiel verwenden wir die Vorlage *beim Erstellen eine neue Datei in Dropbox kopieren OneDrive* .  
 ![](../../includes/media/app-service-logic-templates/template2.png)  

Wenn Sie die Vorlage verwenden möchten, nur Schaltfläche *Diese Vorlage verwenden* . Sie werden aufgefordert, Ihre Konten anmelden auf welche, die Connectors die Vorlage verwendet. Oder wenn bereits eine Verbindung mit diesen Connectors eingerichtet haben, können Sie wie folgt fortfahren.  
 ![](../../includes/media/app-service-logic-templates/template3.png)  

Verbindungsaufbau *weiterhin*auswählen und öffnet die Anwendung Logik in der Entwurfsansicht.  
 ![](../../includes/media/app-service-logic-templates/template4.png)  

Im obigen Beispiel wie viele Vorlagen möglicherweise einige erforderliche Eigenschaftenfelder innerhalb der Connectors aufgefüllt werden; aber erforderlich einige dennoch einen Wert, damit Sie ordnungsgemäß app Logik bereitstellen. Wenn Sie versuchen, ohne die fehlenden Felder bereitstellen, werden Sie mit einer Fehlermeldung benachrichtigt.

Möchten Sie den Betrachter Vorlage zurück, wählen Sie *Vorlagen* in der oberen Navigationsleiste. Zur Vorlage Viewer wechseln, verlieren Sie alle nicht gespeicherten. Vor dem Wechsel in Vorlage Viewer sehen Sie eine Warnung benachrichtigt Sie.  
 ![](../../includes/media/app-service-logic-templates/template5.png)  

## <a name="how-to-deploy-a-logic-app-created-from-a-template"></a>Bereitstellen eine Vorlage Logik app

Nachdem Sie Ihre Vorlage geladen und beliebig ändern, wählen Sie speichern-Schaltfläche in der oberen linken Ecke. Dies spart und Logik-app veröffentlicht.  
 ![](../../includes/media/app-service-logic-templates/template6.png)  

Wenn Sie weitere Informationen wünschen bearbeitet Schritte in einer Logik app-Vorlage hinzufügen oder stellen im Allgemeinen mehr an [eine Logik-app erstellen](app-service-logic-create-a-logic-app.md).