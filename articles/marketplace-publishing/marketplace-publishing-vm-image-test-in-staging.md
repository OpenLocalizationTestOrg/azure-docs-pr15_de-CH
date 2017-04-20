<properties
   pageTitle="Das VM-Angebot für den Markt testen | Microsoft Azure"
   description="Verstehen Sie, wie die VM-Image Azure Marketplace."
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
   ms.date="08/01/2016"
   ms.author="hascipio" />

# <a name="test-your-vm-offer-for-the-azure-marketplace-in-staging"></a>Testen Sie das VM-Angebot für den Azure, Staging

Staging bedeutet die SKU in einer "Sandbox", wo Sie testen und überprüfen seine Funktionalität vor der Bereitstellung auf dem Markt, bereitstellen. Die SKU wird im staging wie an einen Kunden, der bereitgestellt hat. VM-Image muss zertifiziert sein, Staging abgelegt werden.

## <a name="step-1-push-your-offer-to-staging"></a>Schritt 1: Drücken Sie Ihr Angebot zum staging

1. Klicken Sie auf die Registerkarte **Veröffentlichen** auf **Staging drücken**.

    ![Zeichnen](media/marketplace-publishing-vm-image-test-in-staging/vm-image-push-to-staging.png)

2. Wenn Veröffentlichungsportal Fehler besagt, korrigieren Sie sie.
3.  In der **können die bereitgestellte Angebot zugreifen?** Dialogfeld Geben Sie die Liste der Azure-Abonnements, mit denen Sie Ihr Angebot im [vorschauportal Azure](https://portal.azure.com)Vorschau.

    >[AZURE.NOTE] Bei virtuellen Computern und Lösung Vorlagen bitte **nicht** weißen Abonnements vom Typ CSP, DreamSpark oder Azure in öffnen.


    > Bei virtuellen Computern Wenn auf die Schaltfläche **Drücken, STAGING**folgendermaßen hinter der Szene erfolgt. Sie werden den Fortschritt der einzelnen Schritte auf der Registerkarte Veröffentlichen die Veröffentlichung anzeigen Portal. Überprüfen Sie diese Seite in regelmäßigen Abständen (bis der Status bereitgestellt wird) für alle Informationen die Korrektur von Ihrer Seite aus.

    > - Zunächst wird die staging Anfrage an Zertifizierungsstellen-Teams, die virtuelle Festplatte überprüfen. Jedoch wenn Ihre Anforderung nur marketing ändern hat, wird der Zertifizierung Schritt übersprungen.
    > - Nach Abschluss die Zertifizierung des Angebots Replikationsstart für Azure Rechenzentren. Im Allgemeinen dauert 24-48 Stunden für die Replikation abgeschlossen, jedoch kann bis zu einer Woche je nach Größe der virtuellen Festplatte dauern. Allerdings ist Ihre Anforderung hat nur marketing ändern, die Replikation schneller.
    > - Wenn die Replikation abgeschlossen ist, wird das Angebot in [Azure-Portal](http:/portal.azure.com)verfügbar. Damals den Status der Veröffentlichung bereitgestellt werden Portal. Bereitgestellte Angebot ist nur mit e-Mail-IDs mit dem Angebot bereitgestellt Abonnement zugeordneten [Azure-Portal](http:/portal.azure.com) angezeigt.

4. Melden Sie sich bei [Azure Vorschau Portal](https://portal.azure.com) mit einer Azure im vorherigen Schritt aufgeführten Abonnements.
5. Finden Sie Ihr Angebot und überprüfen Sie die VM Image Punkte:
  - Stellen Sie sicher, dass marketing-Inhalt korrekt auf dem Markt wird angezeigt.
  - End-to-End-Bereitstellung von VM-Image.

      ![IMG-Karte-portal](media/marketplace-publishing-push-to-staging/pubportal-mapping-azure-portal.jpg)

> [AZURE.IMPORTANT] Das Angebot bleibt im staging bis Microsoft Portal Publishing benachrichtigen [Registerkarte**Veröffentlichen** > Klicken Sie auf die Schaltfläche **"Request Genehmigung auf Push auf Produktion"**] müssen für die Produktion bereit ist. Dies ist ein idealer Zeitpunkt müssen alle Mitglieder des Teams Check alles als Vorbereitung für das Angebot aufgeführten gehen.

> Staging-Plattform dient zum Testen des Angebots in einer Seitenansicht vom Verleger. Wir raten dieses Platofrm für kommerzielle Zwecke verwenden.

## <a name="next-steps"></a>Nächste Schritte
Ihr Preisvorschlag "findet" und Sie seine Funktionalität und marketing-Content getestet haben, können Sie die Veröffentlichung Abschlussphase, **Schritt 4**fortfahren: [Bereitstellen Ihres Angebots auf dem Markt](marketplace-publishing-push-to-production.md).

## <a name="see-also"></a>Siehe auch
- [Erste Schritte: Angebot Azure Marketplace veröffentlichen](marketplace-publishing-getting-started.md)
