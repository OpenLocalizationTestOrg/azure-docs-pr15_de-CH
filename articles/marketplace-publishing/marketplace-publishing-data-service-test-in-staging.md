<properties
   pageTitle="Der Datendienst Angebot für den Markt testen | Microsoft Azure"
   description="Verstehen Sie, wie Ihre Daten Serviceangebot für den Azure testen."
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

# <a name="testing-your-data-service-offer-in-staging"></a>Testen Ihr Angebot Datendienst Staging

>[AZURE.IMPORTANT] **Zu diesem Zeitpunkt sind wir nicht mehr Onboarding alle neuen Datendienst Verleger. Neue Dataservices wird nicht für Angebot genehmigt erhalten.** Haben Sie eine SaaS Business Anwendung Elemente verwenden veröffentlichen möchten Sie weitere Informationen [hier](https://appsource.microsoft.com/partners). Wenn ein IaaS-Applikationen oder Developer Service Azure Marketplace veröffentlichen möchten weitere Informationen [hier](https://azure.microsoft.com/marketplace/programs/certified/).

Nach Abschluss der ersten beiden Schritte [Ihr Microsoft Developer-Konto erstellen](marketplace-publishing-accounts-creation-registration.md) und [Ihre Daten Service bieten Veröffentlichungsportal](marketplace-publishing-data-service-creation.md) können Sie für Ihr Angebot im Azure Marketplace verfügbar machen. Dieses Thema führt Sie durch zuerst fortgeschrittene Schritt "Staging"

Staging bedeutet Ihr Angebot in einer "Sandbox", wo Sie testen und überprüfen Sie seine Funktionalität vor dem Laden in die Produktion, bereitstellen. Das Angebot erscheint staging wie an einen Kunden, der bereitgestellt hat.

## <a name="step-1-pushing-your-offer-to-staging"></a>Schritt 1. Das Unterstützungsangebot Staging drücken
Legt das Unterstützungsangebot Staging können Sie das Angebot testen, bevor sie zukünftige Abonnenten verfügbar.  Sie können sehen, wie Ihr Angebot angezeigt werden und funktionieren für diejenigen Daten abonnieren.  

  ![Zeichnen](media/marketplace-publishing-data-service-test-in-staging/step-1.1.png)

1.  Anmelden [Portal veröffentlichen](https://publish.windowsazure.com)
2.  Wählen Sie im linken Navigationsbereich **Datendienste**
3.  Wählen Sie das Angebot an Staging übertragen möchten. Sie werden oben angezeigt.
4.  Klicken Sie auf **Staging drücken** .  
5.  Wenn Probleme mit dem Angebot, die abgeschlossen werden, bevor auf Staging benötigt sehen Sie eine Liste angezeigt.  Korrigieren Sie diese Elemente auf jedes Element in der Liste. Wenn alle Korrekturen **zu Staging und drücken Sie** erneut klicken.

Wenn es keine Probleme mit Ihrem Angebot sehen Sie Popup-Fenster unten.  

Wenn Sie Ihr Angebot in Azure Oberfläche Planung/nicht genehmigt sind (derzeit beschränkte Kapazität), Popupfenster einfach schließen.

Testen Ihres Datendiensts in Azure-Portal (neben DataMarket Portal), benötigen Sie ein Azure-Abonnement-ID zum Testen.  Dieses Abonnement-ID wird das Konto identifizieren, das Ihr Angebot testen dürfen.  

Schneiden Sie Ihre Abonnement-ID, und klicken Sie auf das Häkchen fort.

  ![Zeichnen](media/marketplace-publishing-data-service-test-in-staging/step-1.2.png)

> [AZURE.NOTE] Diese Azure-Abonnements-IDs sind nur für Test- und staging in [Azure-Verwaltungsportal](https://manage.windowsazure.com)erforderlich. Sie müssen nicht in Azure Marketplace zu testen.

Den nächsten Bildschirm zeigt veröffentlichen statt mit dem Symbol "In Bearbeitung" hervorgehoben unten gelb. Auf Staging dauert 10 bis 15 Minuten.  Wenn es länger dauert, zuerst aktualisieren Sie Ihren Browser (drücken Sie F5 in Internet Explorer).  In den seltenen Fällen, wo Ihr Angebot noch, Staging nach einer Stunde treibt, klicken Sie auf link Kontakt lassen Sie uns wissen, dass ein Problem vorliegt.

  ![Zeichnen](media/marketplace-publishing-data-service-test-in-staging/step-1.3.png)

Abschluss zu Staging "In Bearbeitung" Symbol werden Stop verschieben und den Status werden auf "Bereitgestellt" aktualisiert.  Sie können jetzt Ihr Angebot zu testen.  

## <a name="step-2-test-your-staged-offer-in-datamarket"></a>Schritt 2. Testen Sie das bereitgestellte Angebot in DataMarket

Klicken Sie auf den Text **"Siehe Ihren Dienst bieten bei..."** um die Anzeige, dass der Abonnent Wenn Ihr Angebot in Produktion geht und wird im DataMarket angezeigt.

  ![Zeichnen](media/marketplace-publishing-data-service-test-in-staging/step-2.2.png)

Testen oder 12 Elemente markiert über sicherstellen alle Logos Preise-Transaktionen, Text, Bilder, Dokumentation und Links sind korrekt und arbeiten ordnungsgemäß überprüft werden.  Dies ist ein guter Zeitpunkt, um sicherzustellen, dass jeder Testwerte beim Erstellen Ihres Angebots durch tatsächliche Werte ersetzt wurden.

1. Angebot-logo
2. Name des Angebotes
3. Publisher Name/Link zur Website des Unternehmens
4. Kategorien für Ihr Angebot
5. Ihr Preisvorschlag Supportlink Abonnenten unterstützen
6. Kontextbezogene Beschreibung Ihres Angebots
7. Bieten Sie mit Abrechnungsinformationen
8. Verknüpfen mit Code zur Implementierung
9. Bilder, die veranschaulichen verwenden Angebot Daten
10. Eingabe/Ausgabe-Metadaten für jeden Dienst im Angebot
11. Geschäftsbedingungen des Angebots
12. Vorschau des Angebots Daten


Überprüfen Sie abschließend der Dienst über das Datamarket funktioniert, auf den Link "Dieses DATASET Durchsuchen".  Öffnet ein neues Fenster im Tool wir "Explorer Service" aufrufen, damit die Vorschau der Ergebnisse einer Abfrage an den Dienst.  In diesem Fenster können die erforderlichen Parameter eingeben und die Ergebnisse einer Abfrage an den Dienst angezeigt.   Angezeigt wird auch die URL für die Abfrage.  

> [AZURE.NOTE] Werden Sie überprüfen Sie die Beschreibung des Dienstes oben angezeigt.  Das Angebot besteht aus mehr als einen Dienst aufrufen, und klicken Sie auf die Registerkarten unten auf den nächsten Dienst testen.



## <a name="next-step"></a>Nächstes
Wenn Probleme und Lösungsvorschläge Hilfe [Azure Publisher Support]( http://go.microsoft.com/fwlink/?LinkId=272975)kontaktieren.

Sind Sie zufrieden und veröffentlichen Dokumentation Ihr Angebot bitte [Produktion Push Genehmigung anfordern](marketplace-publishing-push-to-production.md) .

## <a name="see-also"></a>Siehe auch
- [Erste Schritte: Wie Veröffentlichen eines Angebots im Azure Marketplace](marketplace-publishing-getting-started.md)
