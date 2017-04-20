<properties
    pageTitle="Steuern von Azure CDN Premium von Verizon Cacheverhalten Anfragen Abfragezeichenfolgen | Microsoft Azure"
    description="Azure CDN-Abfragezeichenfolge Zwischenspeichern Steuerelemente wie Dateien zwischengespeichert werden, wenn sie Abfragezeichenfolgen enthalten sind."
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

#<a name="controlling-caching-behavior-of-cdn-requests-with-query-strings---premium"></a>Zwischenspeichern Steuerungsverhalten CDN Anfragen Abfragezeichenfolgen - Premium

> [AZURE.SELECTOR]
- [Standard](cdn-query-string.md)
- [Azure CDN Premium von Verizon](cdn-query-string-premium.md)

##<a name="overview"></a>Übersicht

Abfragezeichenfolge Zwischenspeichern Steuerelemente wie Dateien zwischengespeichert werden, wenn sie Abfragezeichenfolgen enthalten sind.

> [AZURE.IMPORTANT] Standard- und Premium-CDN Produkte bieten die gleiche Abfragezeichenfolge Cache-Funktion, aber die Benutzeroberfläche unterscheidet.  Dieses Dokument beschreibt die Schnittstelle für **Azure CDN von Verizon**.  Abfrage-Zeichenfolge Cache mit **Azure CDN Standard von Akamai** **Azure CDN Standard von Verizon**, finden Sie unter [Controlling Zwischenspeicherungsverhalten CDN Anfragen Abfragezeichenfolgen](cdn-query-string.md).

Drei Modi stehen zur Verfügung:

- **Standard-Cache**: Dies ist der Standardmodus.  Kantenknoten CDN passieren Abfragezeichenfolge des Requestors auf den Ursprung der ersten Anforderung und Cache Anlage.  Alle nachfolgende Anforderungen für dieses Vermögenswertes Edge-Knoten bereitgestellt werden ignoriert die Abfragezeichenfolge zwischengespeicherte Anlage abgelaufen ist.
- **Nein-Cache**: In diesem Modus fordert Abfragezeichenfolgen sind nicht zwischengespeicherten Kantenknoten CDN.  Die Kantenknoten Anlage direkt vom Ursprung abgerufen und an dem Requestor mit jeder Anforderung übergibt.
- **eindeutige Cache**: Dieser Modus behandelt jede Anforderung mit einer Zeichenfolge als eine einzigartige Anlage mit seinem Cache.  Beispielsweise würde die Antwort vom Ursprung für eine Anforderung für *foo.ashx?q=bar* am Rand Knoten zwischengespeichert und für nachfolgende Caches, die genau dieselbe Abfrage zurückgegeben werden.  Eine Anforderung für *foo.ashx?q=somethingelse* würde als separater Vermögenswert mit eigenen Live zwischengespeichert.

##<a name="changing-query-string-caching-settings-for-premium-cdn-profiles"></a>Ändern der Abfragezeichenfolge Einstellungen für Premium CDN Profile Zwischenspeichern

1. Blatt Profil CDN klicken Sie auf **Verwalten** .

    ![CDN Profil Blade Schaltfläche verwalten](./media/cdn-query-string-premium/cdn-manage-btn.png)

    Das CDN-Verwaltungsportal wird geöffnet.

2. Hovern Sie über **HTTP große** Registerkarte und Maus über Flyout **Cache Settings** .  Klicken Sie auf die **Abfragezeichenfolge Zwischenspeichern**.

    Abfragezeichenfolge Zwischenspeicherungsoptionen angezeigt werden.

    ![CDN-Abfragezeichenfolge Zwischenspeicherungsoptionen](./media/cdn-query-string-premium/cdn-query-string.png)

3. Klicken Sie nach der Auswahl auf die Schaltfläche **Aktualisieren** .


> [AZURE.IMPORTANT] Die Einstellungen möglicherweise dauert bei der Registrierung über das CDN verbreiten nicht sofort sichtbar.  <b>Azure CDN von Verizon</b> Profilen Verteilung wird in der Regel innerhalb von 90 Minuten abgeschlossen jedoch in einigen Fällen kann länger dauern.
