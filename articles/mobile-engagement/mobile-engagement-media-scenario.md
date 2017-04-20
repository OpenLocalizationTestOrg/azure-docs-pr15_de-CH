<properties 
    pageTitle="Azure Mobile Engagement-Implementierung für Medien-App"
    description="Medien-app-Szenario Azure Mobile Engagement implementieren" 
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

#<a name="implement-mobile-engagement-with-media-app"></a>Mobile Engagement Media App implementieren

## <a name="overview"></a>Übersicht

John ist mobile Projektmanager für eine große Medienunternehmen. Er hat vor kurzem eine neue app, die eine hohe Download-Anzahl. Er trifft seine Ziele für Download aber noch seine zurück auf downloadzahlen pro Benutzer nicht Anforderungen sein. 

John hat bereits identifiziert, warum seine ROI zu niedrig ist. Benutzer häufig aufhören app nach 2 Wochen und die meisten nie wieder. Aufbewahrung von app erhöhen will.

Nach dem anfänglichen Testen, sind er gelernt hat, wenn er seine Benutzer mit Push-Benachrichtigung aktiviert sie weiterhin app verwenden. Benutzer inaktiv waren werden auch häufig App je nach Benachrichtigung zurückgegeben, die er sendet. John möchte in einem Programm für seine app Engagement investieren, Pushbenachrichtigungen mit erweiterten auf.

John hat kürzlich gelesen [Azure Mobile Engagement - Getting Started Guide mit Best Practices](mobile-engagement-getting-started-best-practices.md) und Ratschläge aus dem Sicherheitshandbuch Implementierung hat.

##<a name="objectives-and-kpis"></a>Ziele und KPIs

Verantwortlichen für Johns Anwendung entsprechen. Business entsteht anzeigen Benutzer Medien nutzen. John erhöht seine Einnahmen Inhalt pro Benutzer erhöhen. Ein Hauptziel einig: Umsatzsteigerung anzeigen um 25 %. Sie erstellen dieses Ziel Business Key Performance Indicators (KPIs) zu messen und Laufwerk

* Anzahl der pro Benutzer geklickt anzeigen
* Wie viele Artikelseiten besucht (pro Benutzer pro Sitzung / pro Woche / pro Monat...)
* Was sind Bevorzugte Kategorien

Basierend auf John Besprechung mit den wichtigsten Prozessbeteiligten hat er seine Business-KPIs definiert. Er folgt Teil 1 [Azure Mobile Engagement - Getting Started Guide mit Best Practices](mobile-engagement-getting-started-best-practices.md). 

Anschließend erstellt er die folgenden Engagement KPIs um sicherzustellen, dass die Ziele erreicht werden:

* Aufbewahrung über folgende Intervalle überwachen: täglich, wöchentlich, zweiwöchentlich oder monatlich.
* Anzahl der aktiven Benutzer
* Die app-Bewertung in der Anwendung speichert.

Basierend auf Empfehlung des IT-Teams wurden die folgenden technischen KPIs hinzugefügt die folgenden Fragen beantworten:

* Was ist mein Pfad (die Seite wie viele Benutzer ausgeben)
* Anzahl der Abstürze oder Fehler pro Sitzung aufgetreten?
* Welche Betriebssystem-Versionen laufen meine Benutzer?
* Was ist die durchschnittliche Größe der Bildschirm Benutzer?
* Haben welche Internet-Verbindung Benutzer?

Für jeden KPI er die erforderlichen Daten klassifiziert und er an der richtigen Position seiner Playbook aufgezeichnet.

## <a name="engagement-program-and-integration"></a>Engagement-Programm und integration

Nun, dass John definieren seiner KPIs abgeschlossen hat, beginnt er sein Engagement Strategiephase 4 Engagement Programme und ihre Ziele zu erreichen:    ![][1]

Dann tiefer John von Pushbenachrichtigungen für jedes Programm mit. Push-Benachrichtigung fünf Elemente definiert werden:

1. Ziel: Was ist das Ziel der Mitteilung
2. Wie wird das Ziel erreicht
3. Zielgruppe: Wer erhält die Benachrichtigung?
4. Inhalt: Was ist die Formulierung und das Format der Benachrichtigung (im App/Out der App)
5. Wann: Was ist der beste Zeitpunkt dieser Push-Benachrichtigung

    ![][2]

Weitere Informationen finden Sie in der [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks).

Der Teil 2 Whitepaper John verwendet Zielabschnitt definieren, welche Daten sammeln hat und schreibt seine Tag mit IT-Team die Lösung implementieren möchten. Nach 1 Woche und Benutzerakzeptanztests kann John schließlich seine Programme starten.

##<a name="program-results"></a>Ergebnisse

4 Monaten überprüft John Leistungen der Programme. Willkommen beim Programm und Programm treffen seine Ziele. Die Anzahl der Benutzer nur eine Sitzung und weitere Features der app verwendeten verdoppelte sich die Anzahl der Verbindungen pro Woche.

**Inaktive Programm** hilft John Benutzer Tendenzen zu verstehen. Es scheint, dass 15 % der inaktive Benutzer an die Anwendung zurückgegeben. Jedoch nicht die meisten mehr als 1 Monat aktiv. John sieht eine mögliche Optimierung dieser Sequenz mit weiteren Benachrichtigungen seine Inhaltsauswahl erweitern.

Das **Programm Discover** gut nicht. Es erhöht die cross-selling aber nicht genug, um seine Ziele zu erreichen. John gibt an, dass er nicht genügend Daten machen hat Zielgruppenadressierung und entsprechenden Inhalte vorschlagen. Er beendet das Programm und zum Senden von "editorial Pushbenachrichtigungen" mit Azure Mobile Engagement. Seiner schon eine CMS-Lösung Pushbenachrichtigungen zu senden und nicht geändert werden soll.

John entscheidet sich API erreichen das eine HTTP-REST-API Reach-Kampagnen ermöglicht ohne AZME Web-Oberfläche verwalten. Dadurch kann John Datensammlung muss er und seine Autoren mit CMS-Lösung ermöglichen.

Um sicherzustellen, dass dieses Feature funktioniert, bittet Michael IT-Team auf folgende Punkte achten:

1. **Betriebssysteme** : alle haben ihre eigenen Regeln Pushbenachrichtigungen verwalten John möchte alle Fälle und überprüft, ob die APIs behandelt.
Z.B.: Android Push ermöglicht Überblick ist nicht bei iOS.

2. **Zeitrahmen**: John möchte eine API, die den Zeitrahmen und Ende Kampagnen. Benutzer störende Benachrichtigung bombing beibehalten will.

3. **Kategorien**: Marketing-Team bereitet Vorlage für jede Warnung. John fragt IT-Team Kategorien in der API.

Nach einigen Tests ist John zufrieden. Dank dieser API können Journalisten noch senden Pushbenachrichtigungen mit CMS Azure Mobile Engagement sammelt alle Verhalten für Sie

Nach 4 zuerst, Ergebnisse spiegeln eine gute Leistung und gibt vertrauen für John und seine, ROI pro Benutzer nimmt 15 % und Außendienst 17,5 % des Gesamtumsatzes, eine Steigerung von 7,5 % in nur vier Monaten dar.

<!--Image references-->
[1]: ./media/mobile-engagement-media-scenario/engagement-strategy.png
[2]: ./media/mobile-engagement-media-scenario/push-scenarios.png

<!--Link references-->
[Media Playbook link]: https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks
