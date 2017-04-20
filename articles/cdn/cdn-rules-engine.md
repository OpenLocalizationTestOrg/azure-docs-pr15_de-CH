<properties
    pageTitle="Überschreiben Standardverhalten in Azure CDN mit dem Regelmodul HTTP | Microsoft Azure"
    description="Das Regelmodul können Sie anpassen, wie HTTP-Anfragen von Azure CDN behandelt werden, wie die Lieferung bestimmter Inhalte blockiert, eine Cacherichtlinie definieren und Ändern von HTTP-Headern."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="override-default-http-behavior-using-the-rules-engine"></a>Mit dem Regelmodul HTTP-Standardverhalten überschreiben

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Übersicht

Das Regelmodul können Sie anpassen, wie HTTP-Anfragen behandelt werden, die Lieferung bestimmter Inhalte blockiert und definieren eine Cacherichtlinie HTTP-Header ändern.  In diesem Lernprogramm zeigen eine Regel erstellen, die das Zwischenspeicherungsverhalten CDN Ressourcen ändern.  Im Abschnitt "[Siehe auch](#see-also)" ist auch Videoinhalte verfügbar.

## <a name="tutorial"></a>Lernprogramm

1. Blatt Profil CDN klicken Sie auf **Verwalten** .

    ![CDN Profil Blade Schaltfläche verwalten](./media/cdn-rules-engine/cdn-manage-btn.png)

    Das CDN-Verwaltungsportal wird geöffnet.

2. Klicken Sie auf **Http-große** , gefolgt von **Regelmodul**.

    Optionen für eine neue Regel werden angezeigt.

    ![Neue Optionen CDN](./media/cdn-rules-engine/cdn-new-rule.png)

    >[AZURE.IMPORTANT] Die Reihenfolge mehrerer Regeln beeinflusst, wie sie behandelt werden. Eine Regel kann von einer vorherigen Regel angegebenen Aktionen überschreiben.
    
3. Geben Sie einen Namen in der **Name / Beschreibung** Textfeld.

4. Identifizieren Sie die Anfragen, denen die Regel angewendet wird.  **Immer** übereinstimmungsbedingung ist standardmäßig aktiviert.  Sie werden **immer** für dieses Lernprogramm verwenden, also, ausgewählt.

    ![CDN übereinstimmungsbedingung](./media/cdn-rules-engine/cdn-request-type.png)

    >[AZURE.TIP] Es gibt viele Arten der Übereinstimmung in der Dropdownliste zur Verfügung.  Auf das blaue Informationssymbol neben Bedingung erfüllt wird die ausgewählte Bedingung ausführlich erläutert.
    >
    >Vollständige Übereinstimmung Startbedingungen im Detail finden Sie unter [Regeln Engine übereinstimmen und Feature-Details](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_0).

5.  Klicken Sie auf die **+** neben **Features** neu.  Wählen Sie in der Dropdownliste auf der linken Seite **Geltenden internen Max-Age**.  Im Textfeld angezeigt wird, geben Sie **300**.  Lassen Sie die übrigen Standardwerte.

    ![CDN-Funktion](./media/cdn-rules-engine/cdn-new-feature.png)

    >[AZURE.NOTE] Als Übereinstimmung mit zeigt blauen Informationssymbol neben dem neuen Feature auf Details dieses Features.  Bei **Geltenden internen Max-Age**überschreiben wir die Anlage **Cache-Control** und **Expires** -Header Steuerelement Kantenknoten CDN Anlage vom Ursprung aktualisieren.  Unser Beispiel von 300 Sekunden bedeutet Kantenknoten CDN Anlage 5 Minuten vor dem Aktualisieren der Anlage aus dem Ursprung zwischengespeichert werden.
    >
    >Die vollständige Liste der Funktionen im Detail finden Sie unter [Regeln Engine Übereinstimmung und Feature-Details](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1).

6.  Klicken Sie auf **Hinzufügen** , um die neue Regel speichern.  Die neue Regel wird jetzt genehmigen. Sobald sie genehmigt wurde, ändert den Status von **Ausstehenden XML** **Active**XML.

    >[AZURE.IMPORTANT] Regeln ändern dauern bis 90 Minuten über das CDN verbreiten.

## <a name="see-also"></a>Siehe auch
* [Azure Freitag: Azure CDN leistungsstarke neue Premium-Funktionen](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (Video)
* [Regeln Engine Übereinstimmungsbedingung und Feature-Details](https://msdn.microsoft.com/library/mt757336.aspx)
