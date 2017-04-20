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

# <a name="data-service-publishing-guide-for-the-azure-marketplace"></a>Handbuch für Azure Marketplace veröffentlichen Datendienst

>[AZURE.IMPORTANT] **Zu diesem Zeitpunkt sind wir nicht mehr Onboarding alle neuen Datendienst Verleger. Neue Dataservices wird nicht für Angebot genehmigt erhalten.** Haben Sie eine SaaS Business Anwendung Elemente verwenden veröffentlichen möchten Sie weitere Informationen [hier](https://appsource.microsoft.com/partners). Wenn ein IaaS-Applikationen oder Developer Service Azure Marketplace veröffentlichen möchten weitere Informationen [hier](https://azure.microsoft.com/marketplace/programs/certified/).

Nach dem Schritt 1 [Konto erstellen und registrieren](marketplace-publishing-accounts-creation-registration.md), führten wir Sie über die [allgemeinen technischen](marketplace-publishing-pre-requisites.md) und [Technischen](marketplace-publishing-data-service-creation-prerequisites.md) Daten Serviceangebot Azure Marketplace. Nachdem wir Sie durch die Schritte zum Erstellen einer Data Service-Angebot im [Veröffentlichungsportal] gehen[ link-pubportal] für Azure Marketplace.

## <a name="1---login-to-the-publishing-portal"></a>1. melden die Veröffentlichung Portal.

Gehe zu [https://publish.windowsazure.com](https://publish.windowsazure.com. )

**Verwenden Sie zum ersten Mal anmelden Veröffentlichungsportal dasselbe Konto mit dem Verkäufer Ihr Firmenprofil im Developer Center registriert wurde.**  (Später können Sie Mitarbeiter Ihres Unternehmens als co-Administrator im Veröffentlichungsportal hinzufügen).

Klicken Sie auf die Kachel **Veröffentlichen Data Services** ist dies die erste anmelden Veröffentlichungsportal.

## <a name="2---choose-data-services-in-the-navigation-menu-on-the-left-side"></a>2. Wählen Sie im Navigationsmenü auf der linken Seite **Data Services** .

  ![Zeichnen](media/marketplace-publishing-data-service-creation/pubportal-main-nav.png)

## <a name="3---create-a-new-data-service"></a>3. einen neuen Datendienst erstellen

Geben Sie den Titel für die neue Service bieten, und klicken Sie auf "+" auf der rechten Seite.

  ![Zeichnen](media/marketplace-publishing-data-service-creation/step-3.png)

## <a name="4---review-the-sub-menu-under-the-newly-created-data-service-in-the-navigation-menu"></a>4. Überprüfen Sie das Untermenü unter den neu erstellten Datendienst aus.

Klicken Sie auf die Registerkarte **Exemplarische Vorgehensweise** und überprüfen Sie alle erforderlichen Schritte ordnungsgemäß Datendienst auf Azure veröffentlichen.

> [AZURE.TIP] Immer klicken Sie links auf der Seite "Exemplarische Vorgehensweise" oder verwenden Sie Registerkarten auf der Datendienst Angebot Untermenü auf der linken Seite.

## <a name="5---create-a-new-plan"></a>5. erstellen Sie 5. einen neuen Plan.

### <a name="offers-plans-transactions"></a>Bietet Pläne Transaktionen.

Jedes Angebot kann mehrere Pläne aber muss mindestens eins (1) planen. Wenn Endbenutzer Ihr Angebot abonnieren abonnieren sie einen Plan für das Angebot. Jeder Plan definiert wie Endbenutzer den Dienst verwenden können.

Azure Marketplace unterstützt derzeit nur Monatliches Abonnement Transaktion Modell für Datendienste, d. h. Anwender Zahlen monatliche Gebühr nach dem Preis des betreffenden Plans, dem sie abonniert und können jeden Monat Anzahl der Transaktion des Plans definiert.

Jede Buchung, die normalerweise als Anzahl der Datensätze, die der Datendienst zurückgeben definiert Grundlage auf der Abfrage an den Dienst gesendet. Der Standardwert ist 100. Jede Abfrage zurückgegebenen Transaktionen werden Zahl von Datensätzen geteilt durch 100 und die nächste ganze Zahl aufgerundet.

Es obliegt Azure Marketplace Service Layer (Meter) jede Abfrage belegten Transaktionen überwachen.

> [AZURE.IMPORTANT] Endbenutzer die Buchung im Monat erreicht blockiert weiterhin bis zum Ende ihrer monatlichen Abonnement nutzen.

> Der Plan oder eines Pläne können (aber nicht) unbegrenzte Anzahl von Transaktionen enthalten.

### <a name="create-a-plan"></a>Erstellen eines Plans.
1. Klicken Sie auf **"+"** neben "Hinzufügen ein neues Plans".

2. Wählen Sie eine der Optionen: **unbegrenzt** oder **Eingeschränkte** Verwendung für diesen Plan.  Wenn begrenzt die Anzahl der Transaktion geben lässt der Plan monatlich nutzen.

    ![Zeichnen](media/marketplace-publishing-data-service-creation/step-5.1.png)  

    Portal veröffentlichen auch schlägt "Bezeichner", der Name des Plans in der Benutzeroberfläche der Endbenutzer mitgeteilt und des Markt auch verwendet, um den Plan zu identifizieren. Wenn Sie möchten, können Sie "Plan-ID" ändern.

    > [AZURE.NOTE] "Planbezeichner" muss innerhalb jedes Angebot eindeutig sein. Wie viele andere Bezeichner in Bezeichner Publishing Portal Plan wird nach die erste Veröffentlichung in Produktion und Ändern dieser Bezeichner nicht gesperrt werden.

3. Klicken Sie auf, um die Auswahl zu bestätigen.

4. Dann werden Sie weitere Fragen zu den neu erstellten Plan aufgefordert.

    ![Zeichnen](media/marketplace-publishing-data-service-creation/step-5.2.png)


|Frage|Bedeutung|
|----|----|
|**Dieser Plan ist kostenlos?**|Sie können einen Plan vollständig-kostenlos erstellen. Ist der einzige Plan für dieses Angebot bedeutet dies, dass Sie "Kostenlose bieten" auf dem Markt veröffentlichen. Ist nur für eine (von wenigen) Plan It gibt Ihnen die Möglichkeit an Endbenutzer Informationen mit einer relativ geringen Anzahl Transaktionen pro Monat.  Wenn die Antwort "Ja", werden keine weiteren Fragen gestellt.|

> [AZURE.NOTE] Benutzer können immer bezahlten Pläne aktualisieren.

|Frage|Bedeutung|
|----|----|
|**Ist kostenlose Testversion verfügbar?**|Wählen Sie zwischen "Keine Testversion" überhaupt oder geben eine Option, um den Plan "Monat". Herausgeber wie diese Option ermöglicht Endbenutzern die Möglichkeit, die Vorteile des Angebots kostenlos für einen Monat.|

> [AZURE.IMPORTANT] Endbenutzer werden nur eine kostenlose Testversion erwerben, wenn sie Zahlungsmittel z. B. Kreditkarte, Enterprise-Vertrag haben.

> Nach einem Monat kostenlose Testversion startet Azure Marketplace den Ladevorgang Kunden Preis zum Zeitpunkt des Abonnements, wenn der Kunde das Abonnement Abbrechen initiiert. Der Endbenutzer wird keine spezielle Benachrichtigung erfolgt.

|Frage|Bedeutung|
|----|----|
|**Dieser Plan muss einen Aktionscode erwerben?**| Herausgeber können ihre Service-Pläne begrenzen, mit einem speziellen Code namens "A Promocode" für einige Kunden. Nur Endbenutzer mit diesem Promocode können Plan abonnieren. Wenn "Nein" klicken, bestätigen Sie, dass von der Region des Angebots verfügbar (siehe [Marketing HotPicks Marketplace](marketplace-publishing-push-to-staging.md) für weitere Einzelheiten) dieser Plan abonnieren kann. Keine weiteren Fragen gestellt werden.|
|**Ausblenden dieser Plan vor, der einen gültigen Code auch?**|Ist die Antwort auf die Frage "Ja" hat der Verleger eine Option zum Entfernen dieser Plan in der Benutzeroberfläche des Marktes angezeigt werden. Es bedeutet Kunden dabei das Angebot Seite sehen. Promocode um erwerben, erhalten Endbenutzer werden mit diesem Promocode abonnieren.|

## <a name="6---create-your-marketplace-marketing-content"></a>6. erstellen Sie der Markt marketing-content
Informationen zu in **Marketing, Preise, Support und Kategorien** Bitte besuchen Sie benötigten Informationen [Marketing HotPicks Markt](marketplace-publishing-push-to-staging.md) in alle Artefakte in Azure Marketplace veröffentlicht.  

## <a name="7---connect-your-offer-to-your-service-sql-azure-based-or-web-service-based"></a>7. Verbinden Sie Offerte zu Ihrem Dienst (SQL Azure basiert oder Webdienst basiert).

Klicken Sie auf das Untermenü **Data Services** .

In der oberen Hälfte der Seite werden Sie aufgefordert, das Angebot **Namespace**bereitstellen.  

  ![Zeichnen](media/marketplace-publishing-data-service-creation/step-7.png)

Die Frage unten definieren wie der Verleger Azure Marketplace Angebot machen neu erstellt wird. (Weitere Informationen finden Sie unter [Data Services technische Voraussetzung für](marketplace-publishing-data-service-creation-prerequisites.md)).

  ![Zeichnen](media/marketplace-publishing-data-service-creation/step-7.2.png)

**Die Datenbank-Veröffentlichungsdienst**

Klicken Sie auf **Datenbank**. Die folgende Seite wird angezeigt:

  ![Zeichnen](media/marketplace-publishing-data-service-creation/step-7.3.png)

Erstellen eine CSDL-Zuordnung für das Dataset basierend auf SQL Azure DB:

  ![Zeichnen](media/marketplace-publishing-data-service-creation/step-7.4.png)

Für jede Tabelle

  ![Zeichnen](media/marketplace-publishing-data-service-creation/step-7.5.png)

  ![Zeichnen](media/marketplace-publishing-data-service-creation/step-7.6.png)

Wenn Web Service

  ![Zeichnen](media/marketplace-publishing-data-service-creation/step-7.7.png)

> [AZURE.IMPORTANT] [Zuordnen eines vorhandenen Webdiensts OData über CSDL](marketplace-publishing-data-service-creation-odata-mapping.md) für detaillierte Informationen und Beispiele für das Erstellen eines Webdiensts CSDL gelesen.

## <a name="next-steps"></a>Nächste Schritte
Erstellung das Datendienst Angebot bitte sicher, dass Sie der Anleitung im [Markt Marketing HotPicks](marketplace-publishing-push-to-staging.md) vor dem Testen [Der Datendienst Staging](marketplace-publishing-data-service-test-in-staging.md)vorwärts verschieben.

## <a name="see-also"></a>Siehe auch
- [Erste Schritte: Wie Veröffentlichen eines Angebots im Azure Marketplace](marketplace-publishing-getting-started.md)
- Wenn Sie allgemeine OData Zuordnungsprozess und Zweck verstehen möchten, dieser Artikel [Service OData Zuordnung](marketplace-publishing-data-service-creation-odata-mapping.md) Definitionen, Strukturen und Informationen zu überprüfen.
- Wenn Sie lernen und Verstehen der bestimmten Knoten und deren Parameter lesen [Dienst OData Zuordnung Datenknoten](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) für Definitionen, Erläuterung und Beispiele und Case Kontext.
- Wenn Sie beispielsweise Lesen [Daten OData Zuordnung Beispiele](marketplace-publishing-data-service-creation-odata-mapping-examples.md) finden Beispielcode und Codesyntax Kontext überprüfen.


[link-pubportal]:https://publish.windowsazure.com
