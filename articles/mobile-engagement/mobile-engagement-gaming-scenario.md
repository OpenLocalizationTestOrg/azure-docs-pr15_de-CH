<properties 
    pageTitle="Azure Mobile Engagement-Implementierung für Spiele"
    description="Spiele app Szenario implementieren Azure Mobile Engagement" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="08/19/2016"
    ms.author="piyushjo"/>

#<a name="implement-mobile-engagement-with-gaming-app"></a>Mobile Engagement mit Gaming-Anwendung implementieren

## <a name="overview"></a>Übersicht

Spiele neu hat eine neue Fischerei basierte role-play Strategie app. Das Spiel seit läuft 6 Monate. Dieses Spiel ist ein großer Erfolg hat Millionen Downloads und die Aufbewahrung ist im Vergleich zu anderen apps Start Spiel sehr hoch. Der Quartalsbericht Tagung Stakeholder benötigten pro Kunden (ARPU) zu erhöhen. Premium-Spiel Pakete stehen als Sonderangebote. Diese Spiele Packs ermöglichen Benutzern, Darstellung und Leistung Angelschnur und lockt oder greift im Spiel aktualisieren. Paket Verkauf sind jedoch gering. So beschließen sie zuerst die Kundenzufriedenheit mit einem Analysetool analysieren und dann Erweiterte Anwendung sales Verwendung entwickeln Verlobung Segmentierung.

Basierend auf [Azure Mobile Engagement - Getting Started Guide mit Best Practices](mobile-engagement-getting-started-best-practices.md) erstellen sie eine Strategie Engagement.

##<a name="objectives-and-kpis"></a>Ziele und KPIs

Verantwortlichen für das Spiel erfüllt. Ein Hauptziel - Paket Prämie 15 % Umsatzsteigerung einig. Sie erstellen dieses Ziel Business Key Performance Indicators (KPIs) zu messen und Laufwerk

* Gekauft diese Pakete werden auf der Ebene des Spiels?
* Was ist der Umsatz pro Benutzer, pro Sitzung pro Woche und pro Monat?
* Was sind die bevorzugten Einkauf?

Teil 1 [Getting Started Guide](mobile-engagement-getting-started-best-practices.md) erläutert die Ziele und KPIs definieren. 

Jetzt Business-KPIs erstellt Produktmanager Mobile Engagement KPIs um neue Benutzertrends und Aufbewahrung zu bestimmen.

* Aufbewahrung überwachen und auf folgende Intervalle: täglich 2 täglich, wöchentlich, monatlich und alle 3 Monate
* Anzahl der aktiven Benutzer
* Die Bewertung app im Speicher

Basierend auf Empfehlung des IT-Teams wurden die folgenden technischen KPIs hinzugefügt die folgenden Fragen beantworten:

* Was ist mein Pfad (die Seite wie viel Benutzer ausgeben)
* Anzahl der Abstürze oder Fehler pro Sitzung gefunden
* Welche Betriebssystem-Versionen laufen meine Benutzer?
* Was ist die durchschnittliche Größe der Bildschirm Benutzer?
* Welche Art von Internetzugang haben Benutzer?

Für jeden KPI gibt Produktmanager Mobile Daten sie benötigt und wo befindet sich in ihrem Playbook.

## <a name="engagement-program-and-integration"></a>Engagement-Programm und integration

Vor dem Erstellen einer erweiterten Engagement Programm sollte der Projektleiter Mobile Projekt sind wie und Produkte von den Benutzern verwendet werden.

Nach 3 Monaten sammelte Projektleiter Mobile genügend Daten um seine Verkäufe in app Push Notification zu verbessern. Er erfährt, dass:

* Der erste Kauf erfolgt im Allgemeinen auf 14. 90 % der Fälle ist der Kauf neuer legendären Waffen für $3.
* 80 % der Fälle erworben haben, das Produkt und weitere Benutzer kauft.
* Benutzer auf 20 bestanden haben mehr als $10/Woche gestartet.
* Benutzer neigen dazu, Ebene 16, 24 und 32 Premium-Pakete kaufen.

Dank dieser Analyse beschließt Projektleiter Mobile bestimmten Push Notification Verkäufe zu erstellen. Er erstellt drei er ruft Push Sequenzen: Programm, Sales Programm und inaktive Programm Willkommen. Weitere Informationen finden Sie in der [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks)
    ![][1]

<!--Image references-->

[1]: ./media/mobile-engagement-game-scenario/notification-scenario.png

<!--Link references-->
