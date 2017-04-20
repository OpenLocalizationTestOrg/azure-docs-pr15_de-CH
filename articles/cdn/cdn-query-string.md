<properties
    pageTitle="Steuern von Azure CDN Cacheverhalten Anfragen Abfragezeichenfolgen | Microsoft Azure"
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

#<a name="controlling-caching-behavior-of-cdn-requests-with-query-strings"></a>Zwischenspeichern CDN Anfragen Abfragezeichenfolgen-Verhalten steuern

> [AZURE.SELECTOR]
- [Standard](cdn-query-string.md)
- [Azure CDN Premium von Verizon](cdn-query-string-premium.md)

##<a name="overview"></a>Übersicht

Abfragezeichenfolge Zwischenspeichern Steuerelemente wie Dateien zwischengespeichert werden, wenn sie Abfragezeichenfolgen enthalten sind.

> [AZURE.IMPORTANT] Standard- und Premium-CDN Produkte bieten die gleiche Abfragezeichenfolge Cache-Funktion, aber die Benutzeroberfläche unterscheidet.  Dieses Dokument beschreibt die Schnittstelle für **Azure CDN Standard von Akamai** und **Azure CDN Standard von Verizon**.  Abfrage-Zeichenfolge Cache mit **Azure CDN von Verizon**, finden Sie unter [Controlling Zwischenspeicherungsverhalten CDN Anfragen Abfragezeichenfolgen - Premium](cdn-query-string-premium.md).

Drei Modi stehen zur Verfügung:

- **Abfragezeichenfolgen ignorieren**: Dies ist der Standardmodus.  Kantenknoten CDN passieren Abfragezeichenfolge des Requestors auf den Ursprung der ersten Anforderung und Cache Anlage.  Alle nachfolgende Anforderungen für dieses Vermögenswertes Edge-Knoten bereitgestellt werden ignoriert die Abfragezeichenfolge zwischengespeicherte Anlage abgelaufen ist.
- **Bypass Zwischenspeichern für Abfragezeichenfolgen-URL**: In diesem Modus fordert Abfragezeichenfolgen sind nicht zwischengespeicherten Kantenknoten CDN.  Die Kantenknoten Anlage direkt vom Ursprung abgerufen und an dem Requestor mit jeder Anforderung übergibt.
- **Jeder eindeutige URL Cache**: Dieser Modus behandelt jede Anforderung mit einer Zeichenfolge als eine einzigartige Anlage mit seinem Cache.  Beispielsweise würde die Antwort vom Ursprung für eine Anforderung für *foo.ashx?q=bar* am Rand Knoten zwischengespeichert und für nachfolgende Caches, die genau dieselbe Abfrage zurückgegeben werden.  Eine Anforderung für *foo.ashx?q=somethingelse* würde als separater Vermögenswert mit eigenen Live zwischengespeichert.

##<a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a>Ändern der Abfragezeichenfolge Einstellungen für Standardprofile CDN Zwischenspeichern

1. Klicken Sie auf Profil CDN-Blade CDN-Endpunkt zu verwalten.

    ![CDN Profil Blade-Endpunkte](./media/cdn-query-string/cdn-endpoints.png)

    Das CDN-Endpunkt-Blatt wird geöffnet.

2. Klicken Sie auf die Schaltfläche **Konfigurieren** .

    ![CDN Profil Blade Schaltfläche verwalten](./media/cdn-query-string/cdn-config-btn.png)

    Das Blade CDN-Konfiguration wird geöffnet.

3. Wählen Sie eine Einstellung aus **Cacheverhalten Abfragezeichenfolge** .

    ![CDN-Abfragezeichenfolge Zwischenspeicherungsoptionen](./media/cdn-query-string/cdn-query-string.png)

4. Klicken Sie nach der Auswahl auf die Schaltfläche **Speichern** .

> [AZURE.IMPORTANT] Die Einstellungen möglicherweise dauert bei der Registrierung über das CDN verbreiten nicht sofort sichtbar.  Verteilung wird für <b>Azure CDN von Akamai</b> Profile normalerweise innerhalb einer Minute abgeschlossen.  <b>Azure CDN von Verizon</b> Profilen Verteilung wird in der Regel innerhalb von 90 Minuten abgeschlossen jedoch in einigen Fällen kann länger dauern.
