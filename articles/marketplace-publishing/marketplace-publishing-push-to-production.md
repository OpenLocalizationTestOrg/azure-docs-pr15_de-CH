<properties
   pageTitle="Das Angebot auf dem Markt Azure bereitstellen | Microsoft Azure"
   description="Lernen und Anleitung Ihr Angebot – Bereitstellung virtueller Computer Developer Service Datendienst, usw. – Azure Marketplace durchlaufen."
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
   ms.date="08/02/2016"
   ms.author="hascipio" />

# <a name="deploy-your-offer-to-the-azure-marketplace"></a>Das Angebot auf dem Markt Azure bereitstellen
Wenn Sie zufrieden sind mit Ihrem Angebot (d. h. Sie haben getestet Kundenszenarien, marketing-Content usw.) und können zu starten, **Drücken Sie für die Produktion** auf der Registerkarte **Veröffentlichen** .  

1. Die Schritte in der exemplarischen Vorgehensweise Seite veröffentlichen Portal sollte abgeschlossen und Grün. Virtual Machine bietet sicher, dass die nachstehenden Richtlinien befolgt werden.

    ![Zeichnen][img-pubportal-walkthru-checked]

2. Wählen Sie aus der Liste auf der linken Seite die Registerkarte **Veröffentlichen** .

    ![Zeichnen][img-pubportal-menu-publish]

3. Klicken Sie **auf Produktion Genehmigung anfordern**. Nach der Anforderung Genehmigung Team führt eine abschließende Bewertung und dann Ihr Angebot werden in Azure Marketplace.

    ![Zeichnen][img-pubportal-publish-pushproduction]

>[AZURE.IMPORTANT] Bei virtuellen Computern Wenn auf die Schaltfläche Genehmigung anfordern auf Produktion folgendermaßen hinter der Szene erfolgen. Sie werden den Fortschritt der einzelnen Schritte auf der Registerkarte Veröffentlichen die Veröffentlichung anzeigen Portal. Überprüfen Sie diese Seite in regelmäßigen Abständen (so dass der Status "Aufgeführt") für alle Informationen die Korrektur von Ihrer Seite aus.

> - Zunächst geht Wunsch Produktion auf Zertifizierung-Teams, die virtuelle Festplatte überprüfen. Allerdings wird bereits aufgeführte Angebot aktualisieren die Anforderung hat nur marketing ändern und der Zertifizierung, Schritt übersprungen.
> - Im nächsten Schritt die Anforderung sind Content-Validierung Teams, marketing-Content des Angebots zu überprüfen.
> - Wenn die oben beschriebenen Schritte erfolgreich sind, wird das Angebot in der Produktion genehmigt. Zu diesem Zeitpunkt der Status "Auflistung" im Portal veröffentlichen. Dieser Status "Aufgeführt" bedeutet jedoch nicht, dass der Vorgang abgeschlossen ist. Die folgenden Schritte müssen abgeschlossen sein, bevor das Angebot in Azure Marketplace verfügbar ist.
> - Sobald das Angebot in der Produktion im vorigen Schritt genehmigt, die Replikation des Angebots für Azure Rechenzentren starten. Im Allgemeinen dauert 24-48 Stunden für die Replikation abgeschlossen, jedoch kann bis zu einer Woche je nach Größe der virtuellen Festplatte dauern. Jedoch ist Sie bereits aufgeführte Angebot aktualisieren und hat nur marketing ändern, dann die Replikation schneller.
> - Nach Abschluss die Replikation wird dann das Angebot verfügbar im Azure Marketplace.

> Sie können immer das Angebot zwar **Entwurfsstatus (z.B. nie **Push Staging** oder **Push Produktion**)** . Klicken Sie auf der Registerkarte **Verlauf** **Entwurf verwerfen** am unteren Rand der Seite zum Löschen eines Entwurfs.


## <a name="production-checklist-for-all-virtual-machine-offers"></a>Produktion-Checkliste für alle virtuellen Computer Angebote

- Sind Sie ein Microsoft Azure Certified partner
- Registerkarte SKUs sollte die Option "Ausblenden dieser SKU aus dem Markt, weil es immer über eine Projektmappenvorlage gekauft werden sollten" Ja gekennzeichnet werden nur, wenn die SKU eine Projektmappenvorlage gehört. In allen anderen Fällen sollte diese Option immer als Nr. markiert werden
- Hinweis: Sie sollten nicht ändern die SKU Sichtbarkeit festlegen, nachdem die SKU aufgeführt ist. Diese Funktion wird nicht unterstützt.
- Sicherstellen Sie, dass die Logos unten angegebenen Azure Marketplace-Logo-Richtlinien einhalten.
- Angebot und SKU Beschreibung dürfen nicht identisch sein.
- Titel-SKUs und bieten lange Zusammenfassung dürfen nicht identisch sein.
- SKU-Titel und Zusammenfassung der Preisvorschläge dürfen nicht identisch sein.
- SKU-Titel sollten nicht mit mehrere SKUs für ein Angebot identisch sein.

**Azure Marketplace-Logo-Richtlinien**

- Azure Entwurf hat eine einfache Farbpalette. Halten Sie die Anzahl der primären und sekundären Farben für Ihr Logo niedrig.
- Designfarben Azure-Portal sind weiß und Schwarz. Daher verwenden Sie diese Farben als Hintergrundfarbe des Logos. Verwenden Sie eine Farbe, die Ihre Logos im Azure-Portal machen. Einfache Primärfarben empfohlen. Bei Verwendung von transparenten Hintergrund stellen Sie sicher, dass der Logo/Text nicht weiß oder Schwarz.
- Verwenden Sie einen Hintergrund mit Farbverlauf nicht auf das Logo.
- Ordnen Sie Text auch Ihre Firma oder Marke auf das Logo.
- Das Erscheinungsbild Ihres Logos Farbverläufen vermeiden und "flach" sein.
- Das Logo sollte nicht gestreckt.

**Weitere Richtlinien für das Logo Held:**

- Das Logo Held ist optional. Verleger können einen Helden Logo hochladen. **Jedoch einmal hochgeladenen heldensymbol kann nicht gelöscht werden, von der Veröffentlichung Portal. Zu diesem Zeitpunkt muss Partner Richtlinien der Azure Marketplace Held Symbole andere das Angebot nicht angenommen wird zur Produktion.**
- Publisher-Anzeigename, SKU Titel und lange Zusammenfassung Angebot werden in weißer Schrift angezeigt. Daher sollten Sie helle Farbe im Hintergrund des Symbols Held halten. Schwarz, weiß und transparenten Hintergrund ist für Helden Symbole nicht zulässig.
- Verleger anzeigen, Name, Titel SKU, lange Zusammenfassung Angebot und erstellen werden programmgesteuert in einem Logo Held eingebettet, sobald das Angebot aufgeführten. So sollten Sie nicht Text eingeben, beim Entwerfen des Held-Logos. Lassen Sie Leerraum auf der rechten Seite da Text (d. h. Publisher Display name, SKU Titel lange Zusammenfassung Angebot) wird von uns dort programmgesteuert enthalten. Leere Text sollte 415 x 100 rechts und durch 370px von links versetzt.


## <a name="additional-production-checklist-for-already-listed-virtual-machine-offers"></a>Bietet zusätzliche Prüfliste für bereits aufgeführten virtuellen Computer

- Überprüfen Sie, ob bereits ein Angebot gleichnamige bieten Ihrem Unternehmen. Wenn Ja, sollten Sie eine neue Version der Lagerhaltungsdaten in das vorhandene Angebot anstatt ein neues doppeltes Angebot hinzufügen.
- Datenträger sollten zwischen zwei Versionen derselben SKU nicht ändern.
- Azure Marketplace unterstützt Preise ändern der aufgeführten SKUS wie die Rechnung an den Kunden auswirkt. Sicherstellen Sie, dass die Preise der aufgeführten SKUs in den Regionen SKU verfügbare nicht geändert wird. Jedoch neue SKUs hinzufügen oder vorhandene Lagerhaltungsdaten neue hinzufügen.


## <a name="next-steps"></a>Nächste Schritte
Sobald das Angebot live geht testen Sie Kundenszenarien um sicherzustellen, dass alle Verträge und Funktionen in der Produktion als ordnungsgemäß getestet und in die Stagingumgebung.

## <a name="see-also"></a>Siehe auch
- [Erste Schritte: Angebot Azure Marketplace veröffentlichen](marketplace-publishing-getting-started.md)

[img-pubportal-walkthru-checked]:media/marketplace-publishing-push-to-production/pubportal-walkthru-checked.png
[img-pubportal-menu-publish]:media/marketplace-publishing-push-to-production/pubportal-menu-publish.png
[img-pubportal-publish-pushproduction]:media/marketplace-publishing-push-to-production/pubportal-publish-pushproduction.png
