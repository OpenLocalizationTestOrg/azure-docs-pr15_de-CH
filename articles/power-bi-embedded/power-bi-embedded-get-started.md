<properties
   pageTitle="Erste Schritte mit Microsoft Power BI Embedded"
   description="Schalten Sie eingebettete BI, fügen interaktive Power BI-Berichten in Business Intelligence-Anwendung hinzu"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="get-started-with-microsoft-power-bi-embedded"></a>Erste Schritte mit Microsoft Power BI Embedded

**Power BI Embedded** ist eine Azure Service, mit dem Entwickler interaktive Power BI-Berichte in eigene Programme hinzufügen kann. **Power BI eingebettete** funktioniert mit vorhandenen ohne Überarbeitung oder Ändern der Benutzer anmelden.

Ressourcen für **Microsoft Power BI Embedded** werden über [Azure ARM APIs](https://msdn.microsoft.com/library/mt712306.aspx)bereitgestellt. In diesem Fall ist die Ressource, die Sie bereitstellen **Power BI Arbeitsbereich Auflistung**.

![](media\power-bi-embedded-get-started\introduction.png)

## <a name="create-a-workspace-collection"></a>Erstellen Sie eine Arbeitsbereich-Auflistung
Eine **Arbeitsbereich-Auflistung** ist Ressource der obersten Ebene Azure und Container für die Inhalte, die in der Anwendung eingebettet werden. Eine **Arbeitsbereich-Auflistung** kann auf zwei Arten erstellt werden:

   -    Manuell mithilfe von Azure-Portal
   -    Programmgesteuert mithilfe der APIs Azure Ressource Manager(ARM)

Gehen Sie durch die Schritte zum Erstellen eine **Arbeitsbereich-Auflistung** mithilfe der Azure-Portal.

   1.   Öffnen und **Azure-Portal**anmelden: [http://portal.azure.com](http://portal.azure.com).

   2.   Klicken Sie auf **+ neu** im Kopf.

       ![](media\power-bi-embedded-get-started\create-workspace-1.png)

   3.   Klicken Sie auf **Power BI eingebettete** **Daten** + Analytics.
   4.   **Erstellung Blade**Geben Sie die erforderlichen Informationen. **Preise**finden Sie unter [Power BI eingebettete Preise](http://go.microsoft.com/fwlink/?LinkID=760527).

       ![](media\power-bi-embedded-get-started\create-workspace-2.png)

   5. Klicken Sie auf **Erstellen**.

Der **Arbeitsbereich Auflistung** dauert einen Moment bereitstellen. Wenn abgeschlossen, werden Sie zum **Arbeitsbereich Auflistung Blade**weitergeleitet.

   ![](media\power-bi-embedded-get-started\create-workspace-3.png)

**Erstellung Blade** Informationen benötigen Sie die APIs aufrufen, Arbeitsbereiche und Inhalte für sie bereitstellen.

<a name="view-access-keys"/>
## <a name="view-power-bi-api-access-keys"></a>Ansicht Zugriffstasten Power BI-API

Eines der wichtigsten Informationen zum Power BI REST APIs aufrufen werden die **Zugriffstasten**. Diese verwendet, verwendet die API authentifizieren, **app Token** zu generieren. Klicken Sie die **Zugriffstasten** **Zugriffstasten** **Blatt Einstellungen**. Finden Sie weitere Informationen zu **app-Token** [Tauglichkeit und Power BI Embedded autorisieren](power-bi-embedded-app-token-flow.md).

   ![](media\power-bi-embedded-get-started\access-keys.png)

Sie ' Beachten Sie, dass zwei Schlüssel haben.

   ![](media\power-bi-embedded-get-started\access-keys-2.png)

Kopieren Sie diese Schlüssel und sicher in der Anwendung speichern. Es ist sehr wichtig, dass Sie diese Schlüssel behandeln wie ein Kennwort, da sie Zugriff auf alle Inhalte in Ihrem **Arbeitsbereich Auflistung**bereitstellen.

Zwei Schlüssel angegeben sind, wird nur ein Schlüssel zu einem bestimmten Zeitpunkt benötigt. Der zweite Schlüssel wird bereitgestellt, damit Sie Schlüssel in regelmäßigen Abständen Regenerieren können, ohne Zugriff auf den Dienst unterbrechen.

Jetzt haben Sie eine Instanz von Power BI für die Anwendung und **Zugriffstasten**können Sie einen Bericht in Ihre Anwendung. Bevor Sie lernen wie einen Bericht importieren, beschreibt im nächste Abschnitt Erstellen von Power BI Datasets und Berichte in eine Anwendung einbetten.

## <a name="create-power-bi-datasets-and-reports-to-embed-into-an-app"></a>Erstellen Sie Power BI Datasets und Berichte in eine Anwendung einbetten

Jetzt, dass Sie eine Instanz von Power BI für Ihre Anwendung erstellt haben und **Zugriffstasten**, müssen Sie die Power BI Datasets und Berichte, die Sie einbetten möchten. Mit **Power BI Desktop**können Datasets und Berichte erstellt werden. Sie können [Power BI Desktop freizugeben](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/). Oder zum schnellen Einstieg können Sie das [Retail Analyse Beispiel PBIX] (http://go.microsoft.com/fwlink/?LinkID=780547). Weitere Informationen zum Verwenden von **Power BI Desktop**finden Sie unter [Erste Schritte mit Power BI Desktop](https://powerbi.microsoft.com/en-us/guided-learning/powerbi-learning-0-2-get-started-power-bi-desktop).

Mit **Power BI Desktop**verbinden Sie **Power BI Desktop** eine Kopie der Daten importieren oder Herstellen einer Verbindung mit der Datenquelle **DirectQuery**direkt an die Datenquelle.

Hier sind die Unterschiede zwischen **Import** und **DirectQuery**.

|Importieren | DirectQuery
|---|---
|Tabellen, Spalten *und Daten* importiert oder in **Power BI Desktop**kopiert. Arbeiten mit Visualisierung fragt **Power BI Desktop** eine Kopie der Daten. Änderungen finden, die an die zugrunde liegenden Daten müssen aktualisieren oder eine vollständige aktuelle Dataset erneut zu importieren.|Nur *Tabellen und Spalten* importiert oder in **Power BI Desktop**kopiert. Arbeiten mit Visualisierung fragt **Power BI Desktop** die zugrunde liegenden Datenquelle, wodurch Sie immer aktuelle Daten anzeigen.

Weitere Informationen zum Verbinden mit einer Datenquelle anzeigen Sie [Verbinden mit einer Datenquelle](power-bi-embedded-connect-datasource.md)

Nach dem Speichern Ihrer Arbeit in **Power BI Desktop**ist eine PBIX-Datei erstellt. Den Bericht enthält. Darüber hinaus Daten importieren enthält die PBIX des vollständigen Datasets oder **DirectQuery**verwenden, enthält die PBIX einer Dataset-Schema. Die PBIX werden programmgesteuert in Arbeitsbereich mit [Power BI Import-API](https://msdn.microsoft.com/library/mt711504.aspx)bereitstellen.

> [AZURE.NOTE] **Power BI Embedded** hat weitere APIs zu ändern, den Server- und Datenbanknamen Dataset zeigt eine Service-Anmeldeinformationen, mit denen das Dataset mit der Datenbank herstellen. Siehe [Post SetAllConnections](https://msdn.microsoft.com/library/mt711505.aspx) und [Patch-Gateways Datasource](https://msdn.microsoft.com/library/mt711498.aspx).

## <a name="next-steps"></a>Nächste Schritte
In den vorherigen Schritten haben Sie eine Sammlung Arbeitsbereich Ihren ersten Bericht und Dataset erstellt. Jetzt ist es Zeit zu lernen für **Power BI eingebetteten**Code schreiben. Um Ihnen beim Einstieg helfen wir eine Beispiel Web app erstellt: [Erste Schritte mit dem Beispiel](power-bi-embedded-get-started-sample.md). Das Beispiel zeigt, wie auf:

  - Inhalt bereitstellen
      - Erstellen eines Arbeitsbereichs
      - PBIX importieren
      - Aktualisieren Sie Verbindungszeichenfolgen und Anmeldeinformationen für Ihre Datensätze festgelegt.

  - Sichere Einbetten eines Berichts

## <a name="see-also"></a>Siehe auch
- [Erste Schritte mit](power-bi-embedded-get-started-sample.md)
- [Authentifizieren und Autorisieren von Power BI Embedded](power-bi-embedded-app-token-flow.md)
- [Power BI desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)
