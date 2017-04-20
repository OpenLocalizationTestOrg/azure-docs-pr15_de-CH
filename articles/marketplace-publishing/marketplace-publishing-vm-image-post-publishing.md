<properties
   pageTitle="Verwalten Ihre virtuellen Computerabbild auf Azure | Microsoft Azure"
   description="Ausführlicher Leitfaden zur Verwaltung des virtuellen Bildes auf Azure Marketplace nach der ersten Veröffentlichung."
   services="Azure Marketplace"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="Azure"
   ms.workload="na"
   ms.date="08/03/2016"
   ms.author="hascipio;"/>

# <a name="post-production-guide-for-virtual-machine-offers-in-the-azure-marketplace"></a>Nachbearbeitung Handbuch für virtuelle Computer bietet Azure Marketplace

Dieser Artikel beschreibt, wie eine live Virtual Machine Angebot auf dem Markt Azure aktualisiert werden kann. Außerdem führt Sie auf eine oder mehrere neue SKUs zu einem vorhandenen Angebot hinzufügen und Entfernen von live Virtual Machine Angebot oder SKU von Azure Marketplace.

Nachdem eine Angebot-SKU in [Azure-Portal](http://portal.azure.com)bereitgestellt wird, kann nicht die Felder unten ändern:

- **Bieten ID:** [Portal publishing-VMs > Wählen Sie das Angebot -> -> Registerkarte Bilder VM -> Bezeichner bieten]
- **SKU-Kennung:** [Portal publishing-VMs > Wählen Sie das Angebot -> -> -> Registerkarte SKUs SKU hinzufügen]
- **Publisher Namespace:** [Portal publishing-VMs > -> Registerkarte erkennen Sie uns über Ihre Firma (zu finden unter "Schritt 2 Registrieren Sie Ihr Unternehmen") -> -> Walkthrough Publisher Namespace-Namespace >]

Angebot/SKU in [Azure Marketplace](http://azure.microsoft.com/marketplace)aufgeführt ist, können nicht die Felder unten ändern Sie:

- **Bieten ID:** [Portal publishing-VMs > Wählen Sie das Angebot -> -> Registerkarte Bilder VM -> Bezeichner bieten]
- **SKU-Kennung:** [Portal publishing-VMs > Wählen Sie das Angebot -> -> -> Registerkarte SKUs SKU hinzufügen]
- **Publisher Namespace:** [Portal publishing-VMs > Walkthrough Registerkarte -> -> Teilen Sie uns über Ihr Unternehmen (gefunden unter Schritt 2 registrieren) Publisher Namespace-Namespace >]
- **Anschlüsse** [Portal publishing-VMs > Wählen Sie das Angebot -> -> Registerkarte Bilder VM-offene Ports >]
- **Ändern der aufgelisteten eigene(n) Preisgestaltung**
- **Änderung der aufgeführten eigene(n) Modell Abrechnung**
- **Entfernen der Abrechnung aufgeführten eigene(n) Regionen**
- **Ändern die Daten Datenträgeranzahl aufgeführten eigene(n)**



## <a name="1-how-to-update-the-technical-details-of-a-sku"></a>1 wie die technischen Details der SKU aktualisieren

Eine neue Version aufgeführten SKU hinzufügen und erneut veröffentlichen Ihres Angebots anhand der folgenden Schritte aus:

1. Anmeldung zum [Portal veröffentlichen](https://publish.windowsazure.com).
2. Navigieren Sie zur Registerkarte **virtuelle Computer** , und wählen Sie das Angebot.
3. Klicken Sie aus dem linken Menü auf die Registerkarte **Bilder VM** .
4. Abschnitt **SKUs** der Registerkarte **VM Bilder** suchen Sie die SKU, die Sie aktualisieren möchten.
5. Danach fügen Sie eine neue Versionsnummer der Lagerhaltungsdaten und klicken Sie auf die Schaltfläche **"+"** . Die neue Version sollte X.Y.Z Format, X, Y, Z ganze Zahlen sind. Ändert sollte inkrementelle.
6. Fügen Sie im **Betriebssystem VHD URL** erstellte URI für das Betriebssystem VHD SAS hinzu und speichern Sie der.

    >[AZURE.IMPORTANT] Sie können keine Inkrement/Daten Datenträgeranzahl der aufgelisteten SKU Dekrement. Sie müssen in diesem Fall erstellen Sie eine neue SKU. Siehe Abschnitt [3. Hinzufügen neuen SKU unter aufgeführten Angebot](#3-how-to-add-a-new-sku-under-a-live-offer) ausführliche Anleitung.

7. Navigieren Sie nach der Änderung zu der Registerkarte **Veröffentlichen** , und klicken Sie auf die **STAGING PUSH**. Ausführliche Informationen zum Testen Ihres Angebots in die Stagingumgebung finden Sie auf diesen [link](marketplace-publishing-vm-image-test-in-staging.md)
8. Nachdem Sie Ihr Angebot Staging getestet haben, navigieren Sie zu der Registerkarte **Veröffentlichen** die Veröffentlichung Portal und klicken Sie auf die Schaltfläche **PUSH Genehmigung zu Produktion anfordern** , Ihr Angebot auf dem Markt Azure erneut veröffentlichen.

    ![Zeichnen](media/marketplace-publishing-vm-image-post-publishing/img01_07.png)

## <a name="2-how-to-update-the-non-technical-details-of-an-offer-or-a-sku"></a>2. zum technischen Details ein Angebot oder eine SKU aktualisieren

Sie können die technischen (marketing, rechtliche, Unterstützung, Kategorien) Details der live oder SKU in Azure Marketplace.

### <a name="21-update-the-offer-description-and-logos"></a>2.1 angebotsbeschreibung und Logos aktualisieren

Sie aktualisieren die Einzelheiten und erneut veröffentlichen Ihr Angebot durch die folgenden Schritte:

1. Anmeldung zum [Portal veröffentlichen](https://publish.windowsazure.com).
2. Navigieren Sie zur Registerkarte **virtuelle Computer** , und wählen Sie das Angebot.
3. Klicken Sie aus dem linken Menü auf **MARKETING** .
4. Klicken Sie auf **Englisch (USA)** .
5. Klicken Sie aus dem linken Menü auf die Registerkarte **DETAILS** . Im Bereich *Beschreibung* der Registerkarte **DETAILS** können aktualisieren Angebot Titel, Zusammenfassung, lange Zusammenfassung und Speichern der.

    >[AZURE.NOTE] Bitte achten Sie Folgendes beim Aktualisieren der SKU-Informationen.
    **Geben Sie doppelte Text unter angebotsbeschreibung und SKU-Beschreibung. Geben Sie doppelte Text unter Titel SKU und lange Zusammenfassung bieten. Geben Sie doppelte Text SKU Titel und Zusammenfassung des Angebots.**

    ![Zeichnen](media/marketplace-publishing-vm-image-post-publishing/img02.1_05.png)

6. Abschnitt *LOGOS* der Registerkarte **DETAILS** können Sie Logos aktualisieren. Allerdings sicherstellen, dass die Logos [Azure Marketplace-Richtlinien](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content) folgen (finden Sie im Abschnitt Schritt 1: Marketingmaterial bieten Marketplace-Details > -> Azure Marketplace-Logo-Richtlinien).

    >[AZURE.NOTE] Heldensymbol ist optional. Sie können ein heldensymbol hochladen. Jedoch nachdem Hero Symbol hochgeladen wird, besteht keine Möglichkeit zum Löschen aus der Veröffentlichung Portal. In diesem Fall muss der [Held Symbol Richtlinien](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content) (finden Sie im Abschnitt Schritt 1: Marketingmaterial bieten Marketplace-Details > -> Weitere Richtlinien für Held Logo Banner).

7. Navigieren Sie nach der Änderung zu der Registerkarte **Veröffentlichen** , und klicken Sie auf die **STAGING PUSH**. Ausführliche Informationen zum Testen Ihres Angebots in die Stagingumgebung finden Sie auf diesen [Link](marketplace-publishing-vm-image-test-in-staging.md).
8. Nachdem Sie Ihr Angebot Staging getestet haben, navigieren Sie zu der Registerkarte **Veröffentlichen** die Veröffentlichung Portal und klicken Sie auf die Schaltfläche **PUSH Genehmigung zu Produktion anfordern** , Ihr Angebot auf dem Markt Azure erneut veröffentlichen.

    ![Zeichnen](media/marketplace-publishing-vm-image-post-publishing/img02.1_08.png)

### <a name="22-update-the-sku-description"></a>2.2. Aktualisieren Sie die SKU-Beschreibung

SKU-Informationen zu aktualisieren und Sie erneut veröffentlichen Ihr Angebot folgt:

1. Anmeldung beim [Portal veröffentlichen](https://publish.windowsazure.com)
2. Navigieren Sie zur Registerkarte **virtuelle Computer** , und wählen Sie das Angebot.
3. Klicken Sie aus dem linken Menü auf **MARKETING** .
4. Klicken Sie auf **Englisch (USA)** .
5. Klicken Sie aus dem linken Menü auf die Registerkarte **Pläne** . Abschnitt *SKUs* der Registerkarte **Pläne** können SKU Titel, Zusammenfassung SKU und SKU Beschreibungsdetails aktualisieren und Speichern der.

    >[AZURE.NOTE] Bitte achten Sie Folgendes beim Aktualisieren der SKU-Informationen. **Geben Sie doppelte Text unter angebotsbeschreibung und SKU-Beschreibung. Geben Sie doppelte Text unter die SKU Titel und lange Zusammenfassung bieten. Geben Sie doppelte Text SKU Titel und Zusammenfassung des Angebots.**

6. Navigieren Sie nach der Änderung zu der Registerkarte **Veröffentlichen** , und klicken Sie auf die **STAGING PUSH**. Ausführliche Informationen zum Testen Ihres Angebots in die Stagingumgebung finden Sie auf diesen link
7. Nachdem Sie Ihr Angebot Staging getestet haben, navigieren Sie zu der Registerkarte **Veröffentlichen** die Veröffentlichung Portal und klicken Sie auf die Schaltfläche **PUSH Genehmigung zu Produktion anfordern** , Ihr Angebot auf dem Markt Azure erneut veröffentlichen.

    ![Zeichnen](media/marketplace-publishing-vm-image-post-publishing/img02.2_07.png)

### <a name="23-change-the-existing-links-or-add-new-links"></a>2.3 vorhandene Links ändern oder neue Links hinzufügen

Vorhandene Links ändern oder neue Links hinzufügen, und dann erneut veröffentlichen Ihr Angebot folgt:

1. Anmeldung beim [Portal veröffentlichen](https://publish.windowsazure.com)
2. Navigieren Sie zur Registerkarte **virtuelle Computer** , und wählen Sie das Angebot.
3. Klicken Sie aus dem linken Menü auf **MARKETING** .
4. Klicken Sie auf **Englisch (USA)** .
5. Klicken Sie aus dem linken Menü auf die Registerkarte **LINKS** .
6. Wenn Sie einen neuen Link hinzufügen möchten, unter *Links* Abschnitt klicken Sie auf die Schaltfläche **Hyperlink hinzufügen** . Dialogfeld *"Link hinzufügen"* wird geöffnet. In diesem Dialogfeld können Sie den Link Titel und URL Felder und speichern. Sie geben einen Link enthält Informationen, die Kunden helfen können.
7. Möchten Sie aktualisieren oder eine vorhandene Verknüpfung zu löschen, wählen Sie den entsprechenden Link und klicken auf die Bearbeitungsschaltfläche oder die Schaltfläche Löschen entsprechend.

    >[AZURE.NOTE] Stellen Sie sicher, dass die Links in diesem Abschnitt getretene richtig funktioniert, wie diese Links bei der Produktion Anforderung überprüft.

8. Navigieren Sie nach der Änderung zu der Registerkarte **Veröffentlichen** , und klicken Sie auf die **STAGING PUSH**. Ausführliche Informationen zum Testen Ihres Angebots in die Stagingumgebung finden Sie auf diesen [Link](marketplace-publishing-vm-image-test-in-staging.md).
9. Nachdem Sie Ihr Angebot Staging getestet haben, navigieren Sie zu der Registerkarte **Veröffentlichen** die Veröffentlichung Portal und klicken Sie auf die Schaltfläche **PUSH Genehmigung zu Produktion anfordern** , Ihr Angebot auf dem Markt Azure erneut veröffentlichen.

    ![Zeichnen](media/marketplace-publishing-vm-image-post-publishing/img02.3_09-01.png)

    ![Zeichnen](media/marketplace-publishing-vm-image-post-publishing/img02.3-2.png)

### <a name="24-change-an-existing-sample-image-or-add-a-new-sample-image"></a>2.4 ein vorhandenes Bild ändern oder ein neues Bild hinzufügen

Eine vorhandene Bilder ändern oder neue Bilder hinzufügen, und dann erneut veröffentlichen Ihr Angebot folgt:

>[AZURE.NOTE] Nur ein Bild wird im [https://portal.azure.com](https://portal.azure.com)angezeigt.

1. Anmeldung beim [Portal veröffentlichen](https://publish.windowsazure.com)
2. Navigieren Sie zur Registerkarte **virtuelle Computer** , und wählen Sie das Angebot.
3. Klicken Sie aus dem linken Menü auf **MARKETING** .
4. Klicken Sie auf **Englisch (USA)** .
5. Klicken Sie aus dem linken Menü auf die Registerkarte **Bilder** .
6. Wenn Sie ein neues Bild hinzufügen möchten, im Abschnitt *Bilder* klicken Sie auf die Schaltfläche **Neues Bild hochladen** und dann speichern.

    >[AZURE.NOTE] Ein Bild ist ein optionaler Schritt.

7. Ggf. aktualisieren oder Löschen eines vorhandenen Bilds suchen Sie die entsprechende Vorlage und klicken Sie auf die Schaltfläche **Bild ersetzen** oder löschen entsprechend.

8. Navigieren Sie nach der Änderung zu der Registerkarte **Veröffentlichen** , und klicken Sie auf die **STAGING PUSH**. Ausführliche Informationen zum Testen Ihres Angebots in die Stagingumgebung finden Sie auf diesen [Link](marketplace-publishing-vm-image-test-in-staging.md).
9. Nachdem Sie Ihr Angebot Staging getestet haben, navigieren Sie zu der Registerkarte **Veröffentlichen** die Veröffentlichung Portal und klicken Sie auf die Schaltfläche **PUSH Genehmigung zu Produktion anfordern** , Ihr Angebot auf dem Markt Azure erneut veröffentlichen.

    ![Zeichnen](media/marketplace-publishing-vm-image-post-publishing/img02.4_09.png)

### <a name="25-update-the-legal-content"></a>2.5 aktualisieren Sie Inhalte

Sie können Inhalte aktualisieren und erneut veröffentlichen Ihr Angebot folgt:

1. Anmeldung beim [Portal veröffentlichen](https://publish.windowsazure.com)
2. Navigieren Sie zur Registerkarte **virtuelle Computer** , und wählen Sie das Angebot.
3. Klicken Sie aus dem linken Menü auf **MARKETING** .
4. Klicken Sie auf **Englisch (USA)** .
5. Klicken Sie aus dem linken Menü auf die Registerkarte **rechtliche** . *Rechtliche* Abschnitt können Sie Ihre Richtlinien/Geschäftsbedingungen aktualisieren. Geben Sie oder fügen Sie Richtlinien Termini in Textbox *Geschäftsbedingungen* und speichern Sie der.
6. Die Zeichengrenze die rechtliche Vereinbarung beträgt 1.000.000 Zeichen.
7. Navigieren Sie nach der Änderung zu der Registerkarte **Veröffentlichen** , und klicken Sie auf die **STAGING PUSH**. Ausführliche Informationen zum Testen Ihres Angebots in die Stagingumgebung finden Sie auf diesen [link](marketplace-publishing-vm-image-test-in-staging.md)
8. Nachdem Sie Ihr Angebot Staging getestet haben, navigieren Sie zu der Registerkarte **Veröffentlichen** die Veröffentlichung Portal und klicken Sie auf die Schaltfläche **PUSH Genehmigung zu Produktion anfordern** , Ihr Angebot auf dem Markt Azure erneut veröffentlichen.

    ![Zeichnen](media/marketplace-publishing-vm-image-post-publishing/img02.5_08.png)

### <a name="26-update-the-support-information"></a>2.6 Supportinformationen aktualisieren

Die Supportinformationen aktualisieren können und erneut veröffentlichen Ihr Angebot durch die folgenden Schritte:

1. Anmeldung beim [Portal veröffentlichen](https://publish.windowsazure.com)
2. Navigieren Sie zur Registerkarte **virtuelle Computer** , und wählen Sie das Angebot.
3. Klicken Sie aus dem linken Menü auf die Registerkarte **SUPPORT** .
4. Abschnitt *Engineering Kontakt* **SUPPORT** -Registerkarte können Sie die Kontaktdaten aktualisieren. Diese Details werden für die interne Kommunikation zwischen dem Partner und Microsoft verwendet.
5. Registerkarte **Unterstützung** im Abschnitt *Customer Support* können Sie Support-Kontaktinformationen wie **Name, E-Mail, Telefon** und **Support-URL**aktualisieren. Diese Details werden für die interne Kommunikation zwischen dem Partner und Microsoft verwendet.

    >[AZURE.NOTE] Wenn Sie nur e-Mail-Support bereitstellen möchten, Telefonnummer dummy im Bereich **Kundensupport** . In diesem Fall wird der bereitgestellte e-Mail stattdessen verwendet.

6. Navigieren Sie nach der Änderung zu der Registerkarte **Veröffentlichen** , und klicken Sie auf die **STAGING PUSH**. Ausführliche Informationen zum Testen Ihres Angebots in die Stagingumgebung finden Sie auf diesen [link](marketplace-publishing-vm-image-test-in-staging.md)
7. Nachdem Sie Ihr Angebot Staging getestet haben, navigieren Sie zu der Registerkarte **Veröffentlichen** die Veröffentlichung Portal und klicken Sie auf die Schaltfläche **PUSH Genehmigung zu Produktion anfordern** , Ihr Angebot auf dem Markt Azure erneut veröffentlichen.

    ![Zeichnen](media/marketplace-publishing-vm-image-post-publishing/img02.6_07.png)

### <a name="27-update-the-categories"></a>2.7 Updatekategorien

Abschnitt Kategorien für Ihr Angebot aktualisieren kann und erneut veröffentlichen Ihr Angebot durch die folgenden Schritte:

1. Anmeldung beim [Portal veröffentlichen](https://publish.windowsazure.com)
2. Navigieren Sie zur Registerkarte **virtuelle Computer** , und wählen Sie das Angebot.
3. Klicken Sie im linken Seite auf die Registerkarte **Kategorien** .
4. Im Bereich *Kategorien* können Sie Kategorien für Ihr Angebot aktualisieren und Speichern der. Sie können bis zu fünf Kategorien für Azure Marketplace-Galerie auswählen.
5. Navigieren Sie nach der Änderung zu der Registerkarte **Veröffentlichen** , und klicken Sie auf die **STAGING PUSH**. Ausführliche Informationen zum Testen Ihres Angebots in die Stagingumgebung finden Sie auf diesen [link](marketplace-publishing-vm-image-test-in-staging.md)
6. Nachdem Sie Ihr Angebot Staging getestet haben, navigieren Sie zu der Registerkarte **Veröffentlichen** die Veröffentlichung Portal und klicken Sie auf die Schaltfläche **PUSH Genehmigung zu Produktion anfordern** , Ihr Angebot auf dem Markt Azure erneut veröffentlichen.

    ![Zeichnen](media/marketplace-publishing-vm-image-post-publishing/img02.7_06.png)

## <a name="3-how-to-add-a-new-sku-under-a-listed-offer"></a>3. das neuen SKU unter aufgeführten Angebot hinzufügen

Einen neuen SKU können unter live-Angebot anhand der folgenden Schritte aus:

1. Anmeldung zum [Portal veröffentlichen](https://publish.windowsazure.com).
2. Navigieren Sie zur Registerkarte **virtuelle Computer** , und wählen Sie das Angebot.
3. Klicken Sie aus dem linken Menü auf die Registerkarte **SKUs** . Danach klicken Sie auf die Schaltfläche **Hinzufügen ein SKU**.  Ein neues Dialogfeld wird geöffnet. Geben Sie eine SKU-Kennung in Kleinbuchstaben. Aktivieren Sie das Kontrollkästchen für Rechnung model(BYOL) bringen eigene neue SKU mit Abrechnung BYOL veröffentlicht werden soll. Andernfalls deaktivieren Sie das Kontrollkästchen für BYOL. Danach klicken Sie auf die Markierung im Dialogfeld neuen SKU erstellen. Wenn Sie nicht für die Abrechnung BYOL-Modell für die neue SKU, wird das Rechnungsadresse Modell automatisch stündlich für neue SKU festgelegt. 30 Tage kostenlose Testversion für stündliche Abrechnung Modell soll, "ist auf die Option"Ein Monat"für eine kostenlose Testversion zur Verfügung?". Wählen Sie andernfalls "Kein Test". [Hinweis: die Option "ist eine kostenlose Testversion?" wird nur angezeigt, wenn Sie BYOL nicht im Dialogfeld, beim Erstellen neuen SKUs ausgewählt haben.]

    >[AZURE.IMPORTANT] Die Option "Diese SKU vom Marketplace ausblenden, da immer über eine Projektmappenvorlage gekauft werden soll" ist als "Ja" nur, wenn Sie zum Veröffentlichen einer Vorlage Lösungsangebots Azure Markt zugelassen sind. Andernfalls sollte diese Option immer als "Nein" gekennzeichnet.

4. Jetzt aus dem linken Menü, klicken Sie auf die Registerkarte **Bilder VM** und ermitteln Sie die neue SKU erstellt haben.
5. Um neue SKU einrichten, finden Sie in Schritt 5 dieser [Link](marketplace-publishing-vm-image-creation.md#5-obtain-certification-for-your-vm-image) Anleitung.
6. Fügen Sie marketing-Material für die neue SKU finden Sie im Abschnitt Schritt 1: Marketingmaterial bieten Marketplace-Details > -> Zahlen 2 bis 5 für diese [Verknüpfung](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).
7. Um die Preisinformationen für neue SKU hinzufügen, finden Sie in Abschnitt 2.1. Die VM Preise dieser [link](marketplace-publishing-push-to-staging.md#step-2-set-your-prices)
8. Navigieren Sie nach der Änderung zu der Registerkarte **Veröffentlichen** , und klicken Sie auf die **STAGING PUSH**. Ausführliche Informationen zum Testen Ihres Angebots in die Stagingumgebung finden Sie auf diesen [link](marketplace-publishing-vm-image-test-in-staging.md)
9. Nachdem Sie Ihr Angebot Staging getestet haben, navigieren Sie zu der Registerkarte **Veröffentlichen** die Veröffentlichung Portal und klicken Sie auf die Schaltfläche **PUSH Genehmigung zu Produktion anfordern** , Ihr Angebot auf dem Markt Azure erneut veröffentlichen.

    ![Zeichnen](media/marketplace-publishing-vm-image-post-publishing/img03_09-01.png)

    ![Zeichnen](media/marketplace-publishing-vm-image-post-publishing/img03_09-02.png)

## <a name="4-how-to-change-the-data-disk-count-for-a-listed-sku"></a>4. wie die Daten Datenträger für aufgeführten SKU ändern

Sie können keine Inkrement/Daten Datenträgeranzahl der aufgelisteten SKU Dekrement. Erstellen einer neuen SKU benötigen in diesem Fall. Siehe Abschnitt [3. Einen neuen SKU unter live Angebot hinzufügen](#3-how-to-add-a-new-sku-under-a-live-offer) ausführliche Anleitung.

## <a name="5---how-to-delete-a-listed-offer-from-the-azure-marketplace"></a>5. wie Azure Marketplace aufgelisteten Angebot löschen

Es gibt verschiedene Aspekte, die bei einem aktiven Angebot entfernen durchgeführt werden müssen. Bitte gehen Sie zu Anleitung Supports Azure Marketplace aufgelisteten Angebot entfernt:

1.  Lösen Sie Support-Ticket mit diesem [Link aus](https://support.microsoft.com/en-us/getsupport?wf=0&tenant=ClassicCommercial&oaspworkflow=start_1.0.0.0&locale=en-us&supportregion=en-us&pesid=15635&ccsid=635993707583706681)
2.  Wählen Sie Problemtyp wie **"verwalten"** und Kategorie als **"Ein Angebot bzw. SKU bereits in Produktion ändern"**
3.  Der Antrag

Das Support-Team führt Sie durch den Löschvorgang Angebot-SKU.

>[AZURE.NOTE] Sie immer können das Angebot Entwurfsstatus wird (d. h. nicht im STAGING- oder PRODUKTIONSSERVER) durch Klicken auf die Schaltfläche **Abbrechen** Registerkarte **Verlauf** .

## <a name="6-how-to-delete-a-listed-sku-from-the-azure-marketplace"></a>6 wie Azure Marketplace aufgelistete SKU löschen

Die unten angegebenen Schritte, um aufgeführten SKU Azure Marketplace zu löschen:

1. Anmeldung zum [Portal veröffentlichen](https://publish.windowsazure.com).
2. Navigieren Sie zur Registerkarte **virtuelle Computer** , und wählen Sie das Angebot.
3. Klicken Sie im linken Seitenbereich auf **SKUS** .
4. Wählen Sie die Lagerhaltungsdaten möchten, und klicken auf die Schaltfläche Löschen für diese SKU.
5. Erledigt, navigieren Sie zu der Registerkarte Veröffentlichen die Veröffentlichung Portal und klicken Sie auf die Schaltfläche **PUSH Genehmigung zu Produktion anfordern** Angebot in Azure Marketplace erneut veröffentlichen.
6. Sobald das Angebot Azure Markt neu veröffentlicht, werden die SKU von Azure Marketplace und Azure-Portal gelöscht.

## <a name="7-how-to-delete-the-current-version-of-a-listed-sku-from-the-azure-marketplace"></a>7. wie die aktuelle Version der aufgelisteten SKU Azure Marketplace löschen

Die nachfolgenden Schritte können Sie die aktuelle Version der aufgelisteten SKU Azure Marketplace löschen. Nach Abschluss des Vorgangs wird die SKU auf die vorherige Version zurückgesetzt.

1. Anmeldung zum [Portal veröffentlichen](https://publish.windowsazure.com).
2.  Navigieren Sie zur Registerkarte **virtuelle Computer** , und wählen Sie das Angebot.
3.  Klicken Sie im linken Seitenbereich auf **VM-IMAGES** .
4.  Wählen Sie die Lagerhaltungsdaten, deren aktuelle Version, die Sie löschen möchten und klicken auf die Schaltfläche Löschen für diese Version.
5.  Erledigt, navigieren Sie zu der Registerkarte **Veröffentlichen** die Veröffentlichung Portal und klicken Sie auf die Schaltfläche **PUSH Genehmigung zu Produktion anfordern** Angebot in Azure Marketplace erneut veröffentlichen.
6.  Sobald das Angebot Azure Markt neu veröffentlicht, werden die aktuelle Version der aufgelisteten Lagerhaltungsdaten Azure Marketplace und Azure-Portal gelöscht. Die SKU wird die frühere Version zurückgesetzt.

## <a name="8-how-to-revert-listing-price-to-production-values"></a>8. wie Listenpreis Produktion Werte wiederherstellen
Die Preise aufgeführten SKUs geändert haben oder entfernt von Regionen Rechnung aufgeführten SKU. Da es nicht im Azure Marketplace unterstützt, möchten Meine Ändern der Produktionswerte zurückgesetzt werden. Wie erreichen kann ich das?

Führen Sie die folgenden Schritte aus:

1. Anmeldung zum [Portal veröffentlichen](https://publish.windowsazure.com).
2. Navigieren Sie zur Registerkarte **virtuelle Computer** , und wählen Sie das Angebot.
3. Klicken Sie aus dem linken Menü auf der Registerkarte **Preise** .
4. Wählen Sie unter der Registerkarte Preise eines Bereichs, deren Preis Sie zurücksetzen möchten.

    ![Zeichnen](media/marketplace-publishing-vm-image-post-publishing/img08-04.png)

5. Bei SKUs mit stündlichen Abrechnung Zurücksetzen der Preise für alle Kerne sind in der Produktion für den ausgewählten Bereich. Für SKUs mit BYOL Abrechnung SKU verfügbar machen im Bereich das Kontrollkästchen für die SKU Abschnitt EXTERNALLY-LICENSED (BYOL) SKU Verfügbarkeit (siehe Abbildung unten).

    ![Zeichnen](media/marketplace-publishing-vm-image-post-publishing/img08-05.png)

6. Jetzt klicken Sie **AUTOPRICE andere Märkte basierend auf Preisen IN Deutschland**.

    >[AZURE.NOTE] Beschriftung der Schaltfläche kann je nach Region abweichen ausgewählt haben. Da wir beim Erstellen dieses Dokuments USA ausgewählt haben, so die Schaltfläche trägt wie "Auto Preis andere Märkte in USA" in der Abbildung unten.

    ![Zeichnen](media/marketplace-publishing-vm-image-post-publishing/img08-06.png)

7. Automatische Preis-Assistent wird geöffnet. Die erste Seite wird angezeigt, für Basis-Markt. Der Abschnitt und wechseln zur nächsten Seite durch Klicken auf die Schaltfläche **"->"** .

    ![Zeichnen](media/marketplace-publishing-vm-image-post-publishing/img08-07.png)

8. Option zum Auswählen der Adern und wird auf Seite 2 angezeigt. Wählen Sie die gewünschten Pläne und die Kerne, und klicken Sie auf die Schaltfläche "->".

    ![Zeichnen](media/marketplace-publishing-vm-image-post-publishing/img08-08.png)

9. Seite 3 zeigt die Märkte/Regionen. Klicken Sie alle umschalten, wählen Sie alle Regionen oder manuell die Kontrollkästchen für die Region. Klicken Sie auf die Schaltfläche "->", um zur nächsten Seite wechseln.

    ![Zeichnen](media/marketplace-publishing-vm-image-post-publishing/img08-09.png)

10. Seite 4 zeigt die Wechselkurse. Klicken Sie auf die Schaltfläche Fertig stellen die Schritte. Der Assistent wird die Preise entsprechend Ihrer Auswahl zurückgesetzt.

11. Jetzt wechseln Sie zur Registerkarte Preise, und klicken Sie auf "Ansicht Zusammenfassung und CHANGES".
Wählen Sie "Entwurf" im Abschnitt "Anzeigen" und "Produktion" im Abschnitt "Vergleichen mit" (siehe Abbildung unten). Wenn keine Preise Unterschied angezeigt wird, bedeutet das, dass Preise die Produktion Werte erfolgreich wiederhergestellt wurde.

    ![Zeichnen](media/marketplace-publishing-vm-image-post-publishing/img08-11.png)

12. Navigieren Sie nach der Änderung zu der Registerkarte veröffentlichen, und klicken Sie auf die **STAGING PUSH**. Ausführliche Informationen zum Testen Ihres Angebots in die Stagingumgebung finden Sie auf diesen [link](marketplace-publishing-vm-image-test-in-staging.md)
13. Nachdem Sie Ihr Angebot Staging getestet haben, navigieren Sie zu der Registerkarte Veröffentlichen die Veröffentlichung Portal und klicken Sie auf die Schaltfläche **PUSH Genehmigung zu Produktion anfordern** , Ihr Angebot auf dem Markt Azure erneut veröffentlichen.

## <a name="9-how-to-revert-billing-model-to-production-values"></a>9. wie Rechnungsadresse Modell Produktion wiederherstellen
Rechnung aufgeführten SKU-Modell geändert haben. Da es nicht im Azure Marketplace unterstützt, möchten Meine Ändern der Produktionswerte zurückgesetzt werden. Wie erreichen kann ich das?

Führen Sie die folgenden Schritte aus:

1. Anmeldung zum [Portal veröffentlichen](https://publish.windowsazure.com).
2. Navigieren Sie zur Registerkarte **virtuelle Computer** , und wählen Sie das Angebot.
3. Klicken Sie aus dem linken Menü auf die Registerkarte **SKUS** .
4. Schaltfläche Bearbeiten Abrechnung Modell wiederherstellen. Ein Fenster wird geöffnet. Aktivieren Sie oder deaktivieren Sie das Kontrollkästchen **"Abrechnung und Lizenzierung erfolgt extern von Azure (aka bringen Ihre eigene Lizenz)"** entsprechend.

    ![Zeichnen](media/marketplace-publishing-vm-image-post-publishing/img09-04.png)

5. Einmal finden Sie die Antwort auf die Frage 8 in diesem Dokument die Preise wieder zurück.
6. Nachdem die Registerkarte **Veröffentlichen** die Veröffentlichung navigieren Portal und Push das Angebot für die Bereitstellung zu testen. Sobald Sie erfolgt mit dem Angebot, dann klicken Sie auf **PUSH Genehmigung zu Produktion anfordern** , Ihr Angebot auf dem Markt Azure erneut veröffentlichen.

## <a name="10-how-to-revert-visibility-setting-of-a-listed-sku-to-the-production-value"></a>10. wie sichtbarkeitseinstellung aufgeführten SKU Produktion Wert zurückgesetzt

Führen Sie die folgenden Schritte aus:

1. Anmeldung zum [Portal veröffentlichen](https://publish.windowsazure.com).
2. Navigieren Sie zur Registerkarte **virtuelle Computer** , und wählen Sie das Angebot.
3. Klicken Sie aus dem linken Menü auf die Registerkarte **SKUS** .
4. Wählen Sie die gewünschte SKU und sichtbarkeitseinstellung der Lagerhaltungsdaten Produktion Wert zurückgesetzt.

    ![Zeichnen](media/marketplace-publishing-vm-image-post-publishing/img10-04.png)

5. Einmal erfolgen entsprechend und klicken Sie auf **PUSH Genehmigung zu Produktion anfordern** , Ihr Angebot auf dem Markt Azure erneut veröffentlichen.

## <a name="see-also"></a>Siehe auch
- [Erste Schritte: Wie Veröffentlichen eines Angebots im Azure Marketplace](marketplace-publishing-getting-started.md)
- [Grundlegendes zum Verkäufer Einblicke reporting](marketplace-publishing-report-seller-insights.md)
- [Grundlegendes zur Auszahlung reporting](marketplace-publishing-report-payout.md)
- [Die Cloud-Lösungsanbieter Reseller Incentive ändern](marketplace-publishing-csp-incentive.md)
- [Problembehandlung bei gemeinsamen publishing auf dem Markt](marketplace-publishing-support-common-issues.md)
- [Erhalten Sie Unterstützung als Verleger](marketplace-publishing-get-publisher-support.md)
- [Lokale erstellen VM-Abbild](marketplace-publishing-vm-image-creation-on-premise.md)
- [Erstellen Sie einen virtuellen Computer mit Windows Azure Vorschau-Portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md)
