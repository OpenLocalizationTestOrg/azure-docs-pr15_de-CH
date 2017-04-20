<properties 
    pageTitle="Azure Mobile Engagement erste Schritte mit Best Practices"
    description="Erste Schritte für Azure Mobile Engagement und Best Practices für onboarding" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="wesmc7777"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="10/04/2016"
    ms.author="wesmc;ricksal"/>

# <a name="azure-mobile-engagement---getting-started-guide-with-best-practices"></a>Azure Mobile Engagement - erste Schritte mit Best practices

## <a name="overview"></a>Übersicht

**Mobile Bildschirm ist überfüllt Speicherplatz:** 2013 fand eine durchschnittliche Mobilgerät 27 installiert waren. Benutzer werden ihre apps normalerweise 30 Stunden pro Monat ausgegeben. Die meiste Zeit verbrachte auf social networking und Spiele (20 Stunden). 2014 hatte im Market 1,5 Millionen Applikationen für Benutzer aus. Apple Store enthalten rund 1,2 Millionen apps. Verwendung der mobilen Anwendung wächst weiterhin, wie Entwickler in diesem Markt. 

Durchschnittliche mobile Benutzer installieren und Deinstallieren von apps mit je nach Interessen ändern und in der Anwendung auftritt. Um den Erfolg einer App wird wichtig mehr wie viele Benutzer die Anwendung installieren. Unbedingt wissen, wie gut ist Ihre app und dieser Trend der Verwendung ändern. Die folgenden Fragen sind wichtig:

- Werden die Benutzer Ihrer app uninteressant oder veraltete finden? 
- Wie viele Benutzer verwenden Ihre app beendet haben? 
- Sind in der App nach oben oder unten tendiert?
- Nicht Benutzer, aufgrund von Problemen mit der app oder mangelndes Arbeiten abschließen? 
- Könnten Sie Ihre app nützlich und relevant halten drücken Sie neue Inhalte für Ihre Benutzer? 
- Würde diese neue Inhalte für alle Benutzer gleich oder auf Benutzersegmente basierend auf Verhalten in Ihrer Anwendung? 
 
Antworten auf Fragen wie diese könnten Leben und Einnahmen aus Ihrer Anwendung erweitern. Sie können Sie definieren und Ihre Benutzerbasis beibehalten. 

Medien-bezogenen apps neigen dazu, einige der höchsten Aufbewahrung von Benutzern. Dies ist sie ständig neue Inhalte für Benutzer bereitstellen. Frühe Anpassung hilfreich Pushbenachrichtigungen angewiesen, ein Benutzersegment eher eine starke Auswirkung auf app. 

Azure Mobile Engagement-Programm soll Ihnen das Leben und die Aufbewahrung Ihrer Anwendung erweitern, indem eine Methode zum Sammeln und Analysieren von Informationen über die Verwendung Ihrer Anwendung. Es hilft Ihren Benutzerstamm Verhalten klassifizieren und gezielte Kampagnen für Pushbenachrichtigungen und Nachrichten-app zu ermittelten Benutzersegmenten. Key Performance Indicators (KPI) messen, wie aktiv die Benutzer mit Ihrer Anwendung sind. Azure Mobile Engagement bietet Methoden, die Sie diese KPIs zu bestimmen müssen. Sie können die Investitionsrendite (ROI) durch Erhöhen der Infrastruktur, die Sie mit der mobilen Anwendung erhöhen müssen. 

Um Azure Mobile Engagement optimal müssen Sie mit einem gut entworfenen Engagement starten. Ihr Plan können Sie detaillierten Daten feststellen zu Benutzerbasis segmentieren müssen. Hierfür können Verhalten und in der Anwendung auftritt. Für den Plan zu ist es empfehlenswert, den KPI klar zu definieren, die Ziele Ihrer Anwendung messen. Mit klar definierten Leistungsindikatoren können Sie die notwendige Logik einfach Einbetten in Ihrer Anwendung, feinkörnige Daten analysieren und Auswerten von KPIs verwendet. Dieses Thema ist ein Handbuch zum Definieren von KPIs, die Sie mit Ihrem Projekt verwenden. 


## <a name="step-1-define-your-kpis-to-fit-the-bet-model"></a>Schritt 1: Definieren Sie KPIs SUCHERGEBNIS Modell anpassen


Korrekt definieren KPIs kann eine schwierige Aufgabe sein. Apps für verschiedene Branchen entwickelt haben ihre eigenen Merkmale und Ziele. Diese tendenziell Ansatz zu verwechseln. Um dies zu vermeiden, Ziele und KPIs einzustufen in drei Hauptkategorien unterteilt: **Business** **Engagement**und **technische**. Dies nennt man das **SUCHERGEBNIS Modell**.

Ein guter Plan haben im Allgemeinen mit KPIs, die Erfolge in den folgenden Kategorien des Modells SUCHERGEBNIS messen.


#### <a name="business-kpis"></a>Business-KPIs

Business-KPIs sollten die einfachste erstellen. Sie definiert bereits in irgendeiner Form, bei der mobilen Anwendung. Diese KPIs Hilfe im Allgemeinen Maßnahme Umsatz und Rentabilität app. Die folgende Liste enthält einige Beispiele Business-KPIs, die helfen können Sie beim Definieren der Leistungsindikatoren:

- Medium Business KPIs
    - Anzahl der anzeigen geklickt
    - Anzahl der Besuche pro Benutzer
    - Anzahl von Abonnements
- Gaming-Business-KPIs
    - Anzahl der in der App
    - Pro Kunde
    - Zeit pro Sitzung
    - Tage gespielt und aktuellen Spiel auf
- E-Commerce-Geschäft KPIs
    - Tage-app verwendet
    - Pro Kunde
    - Durchschnittliche Warenkorb Kasse
    - Kategorie für die meisten Ansichten und Einkäufe
- Bank und Versicherung Business-KPIs
    - Anzahl der Konten
    - Aktivierte Funktionen
    - Besuchte Angebotsseiten
    - Alerts geklickt oder aktiviert      



#### <a name="engagement-kpis"></a>Engagement-KPIs

Ein KPI Engagement ist ein Leistungsindikator Engagement Ihrer Benutzer zu messen. Entwicklungen in diesem Bereich ermitteln die Aufbewahrung Ihrer App. Hier sind einige Beispiel-Kennzahlen für diesen KPI:

- Aktive Benutzer in den letzten 7 Tagen
- Inaktive Benutzeranzahl für die letzten 7 Tage
- Anzahl der Benutzer nicht die app 30 Tage verwendet haben  

Einige offensichtlichen externen Faktoren beeinflussen die Indikatoren in diesem Bereich. Beispielsweise sollten Sie ein mobiles Gerät mit einem Benutzer sein. Dies kann oder nicht zutreffen. Gaming-app möglicherweise tendenziell höhere Auslastung um Feiertage Wenn ein Spieler mehr Arbeit oder Schule spielen.   

Gut definierten helfen KPIs in dieser Kategorie messen die Beziehung zwischen Ihrer Anwendung und Ihren Kunden Ihnen.



#### <a name="technical-kpis"></a>Technische KPIs

Leistungsindikatoren in dieser Kategorie können Sie ermitteln, ob Ihre app richtig verhält, hängt oder abstürzt. Diese Indikatoren können den Zustand der Anwendung zu messen und Nutzbarkeit Probleme, die Benutzer der Anwendung verhindern. Informationen für diese Kategorie könnte auch Informationen enthalten, die für marketing-Teams. Daten könnte nützlich für die Problembehandlung IT und support-Teams können Sie erkennen. 
 
Hier sind einige Beispiele für technische KPIs:

- Informationen über die Ausnahme nicht behandelt bzw. behandelt und count 
- Zeitstempel für letzten Absturz
- Zuletzt geklickt oder letzte Seite
- Speicherverwendung der Anwendung
- App-Framerate
- Version des Betriebssystems, der die Anwendung ausgeführt wird
- Anw.-version

Definieren Sie diese KPIs app Leistung messen und mögliche Fehler zu ermitteln. Diese Indikatoren sollen die Zeit verringern, die Sie ein Update für Ihre Kunden bereitstellen müssen. Sie können Ihnen auch definieren, die einer bestimmten Problemen Benutzersegments. Diese benutzersegmentierung können Sie Kampagnen um Benachrichtigungen verfügbaren Updates und möglichen Aktionen auf Kundenzufriedenheit wiederherzustellen. 


#### <a name="playbook-exercise-1-create-your-kpi-dashboard"></a>Playbook Übung 1: Erstellen des KPI-Dashboards

Festlegung Ihrer Marketingstrategie vorzulegen KPIs für Ihre wichtigsten Ziele eine Ansicht. Sie sollte klar definierten Daten, mit denen Sie wichtige Informationen zum Überwachen Ihrer app und das Verhalten des Endbenutzers.

Erstellen eines KPI-Dashboards enthält die folgenden Informationen

1.  Was sind die KPIs für die Anwendung?
2.  Welche Datenpunkte wird jeden KPI dargestellt verwende?
3.  Wo befindet sich diese Daten für die Anwendung (Bildschirm, Einstellungen, System)?
4.  Kann ich eine Sequenz Engagement für diesen KPI spielen?

Verwenden der **KPI-Generator** Arbeitsblatts in [Media Playbook Vorlage] [ Media Playbook link] Beispiele und Hinweise.



## <a name="step-2-your-engagement-program"></a>Schritt 2: Ihr Engagement-Programm


Eine hervorragende mobile Engagement-Programm eine Ihrer Anwendung berücksichtigen. Dazu zählen unbedingt ein hervorragendes Programm Willkommens, das in den ersten Tagen des app-Nutzung für einen Benutzer ausgeführt wird. Dadurch sehr positiv Engagement und Aufbewahrung Ihrer Anwendung auswirken. Studien haben gezeigt, dass die meisten Benutzer Beenden einer Anwendung mit den ersten Tagen nach der Installation. Soll zu erfüllen und treibenden Erwartung Kundeninteresse frühzeitig während der Benutzer weiterhin auf Ihre app konzentriert. Stellen Sie sicher, dass Wert und Nutzen Ihrer Anwendung an Ihre Kunden weiter. 


![](./media/mobile-engagement-getting-started-best-practices/unsegmented-push-notifications.png)

Pushbenachrichtigungen sind am besten zu früh Projekten für Benutzer mobiler Geräte. Allerdings große achten beim Benutzer für Pushbenachrichtigungen segmentieren. Da nach Benutzer fühlt er erhält Spam oder irrelevante Benachrichtigungen können schwerwiegende Beeinträchtigung. In einigen Klicks kann ein Benutzer die Anwendung nie zurück löschen. Der Benutzer sollten hochgradig personalisierten in app Wert statt generischen Spam erhalten.

Wenn Benutzer aktiv sind, kann Ihr Engagement Programm Laufwerk andere Aspekte der Anwendung helfen.

Beispielsweise konnte Sie eine Kampagne einrichten, die aktive Benutzer Bewerten Ihrer Anwendung anfordert. Da dieses Benutzersegment besonders aktiv und der Erfahrung mit Sie app, erwarten Sie sie möglichst genaue Bewertung abgeben. Haben Sie eine hohe app Bewertung, hilft es Laufwerk organischen Download Ihrer Anwendung reduzieren sowie die neuen Akquisitionskosten.



#### <a name="engagement-sequence"></a>Engagement-Sequenz


Globales Engagement Programm umfasst verschiedene Engagement Sequenzen. Jede Sequenz soll mehrere Ziele erreichen.


###### <a name="life-push-sequence"></a>Life-Push-Sequenz


Die nacheinander Push Leben sind je nach den Lebenszyklus des Projekts mit der Anwendung des Benutzers. Ein Benutzer möglicherweise neue, inaktiv oder sehr aktiv. In verschiedenen Phasen des Lebenszyklus einer Engagement können Benutzer Ihre neue Inhalte in Form von Tipps oder Links Dokumentation profitieren. 

Z. B. benötigen ein neuer Benutzer Orientierung einer App Hilfe oder eine neue Benutzer-Incentive ähnlich der folgenden beim ersten der app starten profitieren...

*"Gerne an Bord haben! Denken Sie daran, Ihre 1. Monat kostenlos anmelden!"*


###### <a name="behavioral-push-sequence"></a>Verhaltensbasierte Push-Sequenz

Verhaltensbasierte Push-Sequenz soll Nutzung entsprechend Verhalten für die Anwendung gesammelt.  

Zum Beispiel profitieren ein sehr aktiver Benutzer einer Fantasy Football app wird an folgenden Push-Benachrichtigung...

*"John schwerwiegende Fußballfan sind! Melden Sie sich im Bereich NFL und gewinnen Sie freien Zugang zu den SuperBowl!"*


###### <a name="alerting-push-sequence"></a>Warnung Push-Sequenz

Benutzer schätzen Nachrichten konzentriert sich ihre Interessen. Eine Warnung Push-Sequenz erweitert Engagement durch Versand Interessen, Benutzer gezeigt hat. Explizite möglicherweise ihre eigenen Interessen der Anwendung wählt der Benutzer aus. Es könnte auch implizit basierend auf Daten während der Benutzerinteraktion mit der Anwendung bestimmt werden.

Beispielsweise kann der Benutzer eine E-Commerce-Anwendung regelmäßig eine bestimmte Kaffee kaufen das Sie mit einem KPI erfasst haben. Die folgende Warnung kann Engagement des Benutzers mit der Anwendung verbessern.
 
*"Hallo Wes, Ihre bevorzugten Marken Kaffee werden 25 % der ersten Woche September 2015. Wir schätzen Sie als Kunden und sicherstellen wollten Sie das wussten."*

###### <a name="rentention-push-sequence"></a>Besicherte Push-Sequenz

Diese Sequenz soll Benutzer einen sich wiederholenden Push Notification Kampagnen zu reguläre Gewohnheit mit der Anwendung beibehalten. Dadurch können app Aufbewahrung erhöhen, wenn der Benutzer Aktivitäten genießt. 

Beispielsweise kann der Benutzer eine verknüpfte Anwendung Sport wöchentlich Mannschaften des Benutzers anhand die folgenden Push-Benachrichtigung erhalten:

*"Chance, 200 Punkte go abstimmen, ob New York Hertha Woche gegen Toronto Blue Jays Spiel gewinnen!"*


#### <a name="the-3w-approach"></a>3W Ansatz

Mastering anderen Push-Sequenzen hilft Ihnen mit Anwendern nutzen. Allerdings müssen Sie noch 3W Verfahren zum Personalisieren der Benachrichtigung verwenden. 3W Ansatz befassen wer, wann und für jede Benachrichtigung. Wenn klar diese drei Fragen erfüllen sollten Benachrichtigungen ordnungsgemäß Engagement ausgerichtet.

![](./media/mobile-engagement-getting-started-best-practices/who-what-when.png)



###### <a name="who-the-user-segment-that-will-receive-messages"></a>Wer: Segment der Benutzer, die Nachrichten erhalten

Push Notifications für die Benutzer sehr empfindlich Kommunikationskanal berücksichtigen. Stellen Sie sicher, dass Benachrichtigungen an ein Benutzersegment senden wollen auch den Interessen dieses Benutzersegment beschränkt sind. Fehlerhaft weitergeleitete Meldung dürfte ein Benutzer ein negativ beeinflussen. Sie sollten es Spam zu Ihrer Anwendung deinstalliert wird. 

Kombinieren von bestimmten technischen und Verhalten beim Benutzersegmente zu definieren, die Benachrichtigungen erhalten. Ein einfaches Beispiel definiert ein Benutzersegment könnte mit der folgenden Anweisung:

"Alle Benutzer gestartet der mobile Anwendung für die erste 3 Tage vor und die Anmeldeseite zweimal ohne tatsächlich eine Anmeldung besucht haben".
 
Diese Anweisung identifiziert Daten sammeln, um ein bestimmtes Szenario müssten.


###### <a name="what-the-message-that-you-will-send"></a>Was: Die Nachricht, die Sie senden

**Ton**

Verwenden Sie Ihre Projekte einen Ton für die segmentierten Benutzer. Dies ist definitiv eine gute Möglichkeit den Endbenutzern und ein Benutzer Interesse an Ihrer app. 

**Umleitung**

Eine Push-Benachrichtigung kann über Öffnung der Anwendungdes verwendet werden. Stellt die Benachrichtigung Hintergrund broadcast-Nachrichten oder einer Werbung, diese Benachrichtigung kann deep links direkt an die richtigen Inhalte in der Anwendung. Dazu müssen Sie ein URL der Anwendung verwalten die Umleitung erstellen. Wenn Ihr Engagement Sequenzen arbeiten, ist dies ein wichtiger Schritt, der nicht vergessen.

Umleitung kann auch für andere Systeme verwaltet werden. Beispielsweise kann eine Aktion URL umleiten Endbenutzer zu vielen anderen Systemen, einschließlich der folgenden:

- Eine website
- Ein Postfach mit e-Mail bereits
- Ein SMS
- Einen Dienst wählen
- Speichern Sie direkt an die Anwendung zur Bewertung der Anwendungdes. 

Dadurch werden viele Verkaufschancen Endbenutzer und automatische Regeln zur Verbesserung der Performance.


**Format-Inhalt**

Arten und Push Notification Formate:

1. **Ankündigung** : können Sie Werbung für Benutzer zu verschiedenen Zeitpunkten senden (App App oder jederzeit).
2. **Umfragen** : Sie Endbenutzer Informationen sammeln, indem sie Fragen aktiviert. Diese Antworten sind dann verfügbar, wenn Kriterien Ziel Endbenutzer erstellen.
3. **Legt Daten** : können Sie eine binäre oder base64-Datendatei aktualisieren die Anwendung senden. Die Informationen in einem datenpush wird die Anwendung die Benutzer Ihrer App personalisieren gesendet. Ihre Anwendung muss so konzipiert, dass die Daten in einem datenpush-unterstützen.
4. **Kacheln (nur Windows Phone)** : aktiviert mit Microsoft Push Notification Service (MPNS) senden systemeigenen Push-Benachrichtigung mit XML-Daten (unterstützt seit SDK Version 0.9.0. Letzte Nutzlast Fliesen darf 32 KB nicht überschreiten.). Die Nachricht wird direkt auf Ihr Mainboard Kachel.
5. **WebView** : eine Webansicht wird ein Popupmenü mit Webinhalt. Dieses Popupfenster angezeigt wird, wenn der Endbenutzer auf die Push-Benachrichtigung geklickt hat. Eine Webansicht ermöglicht mehr Interaktion mit dem Endbenutzer.
 
>[AZURE.NOTE] Sicherstellen Sie, dass der Inhalt, den Sie als Push-Benachrichtigung senden der jeweiligen Plattform (iOS, Android, Windows) Richtlinien für die Entwicklung von apps und Senden von Pushbenachrichtigungen entspricht.

 


###### <a name="when-the-timing-of-your-campaign"></a>Wann: Das Timing der Kampagne

Auslösen der optimale Zeitpunkt für eine Kampagne zu aktivieren ist beim Push-Benachrichtigung? Sollte es manuell oder automatisch sein? Sollte es wiederholt? Bestimmen der richtigen Zeit oder Frequenz ist für Nutzer mit den besten Ergebnissen. Für jede Sequenz Engagement und Szenario müssen Sie festlegen, wann wird die beste Zeit, Pushbenachrichtigungen zu senden. Hier sind einige Beispiele:

![](./media/mobile-engagement-getting-started-best-practices/campaign-timing-examples.png)

Wenn Sie täglich viele Benachrichtigungen senden, müssen Sie ernsthaft, dass die Benutzern die Kommunikation als Spam erkennen können. 

Azure Mobile Engagement bietet zwei Arten die Kommunikation wird als Spam zu vermeiden. Verwenden Sie detailliert Segmentierung, um sicherzustellen, dass Sie nicht dieselben Benutzer abzielen. Darüber hinaus bietet Azure Mobile Engagement eine "Quota". Dieses Feature kann Benachrichtigungen für eine Kampagne einschränken. Beispielsweise wird ein Kontingent auf 5 pro Woche festlegen sicherstellen, dass einen Benutzer als Teil der Kampagne Benutzersegment für diese Woche nicht mehr als 5 Benachrichtigungen erhält.





#### <a name="playbook-exercise-2-create-your-engagement-program"></a>Playbook Übung 2: Erstellen Sie Ihr Engagement-Programm

Einige Zeit Ihre Ziele zusammenfasst und Kampagnen erwartungsgemäß spielen Reihenfolge definieren. Vergewissern Sie 3W Ansatz Meldungen in Ihrer Kampagnen. 

Verwenden Sie das Arbeitsblatt **Engagement Programm** [Media Playbook Vorlage] [ Media Playbook link] Beispiele und Hinweise.


## <a name="step-3-app-integration"></a>Schritt 3: App-Integration


#### <a name="create-a-tag-plan"></a>Erstellen eines Plans tag

Azure Mobile Engagement in Ihre app integrieren Sie einen Tag-Plan erstellen müssen. Tag-Plan ist ein Eckpfeiler des Projekts. Definiert die Beziehung zwischen marketing Spezifikationen Workflow der Anwendung und echten Tag-Daten in der app KPIs zu messen. Er gibt an, welche Analyse Sie im Portal angezeigt werden. Es hilft Ihnen auch Benutzersegmente definieren und fokussierte Pushbenachrichtigungen Endbenutzer zu senden. Haben Sie den Tag-Plan definiert, der Code in Ihre app integrieren mit Azure Mobile Engagement SDK ist.

Tag-Plan sollte alles in einer Anwendung nicht markieren. Es sollten nur Transponder enthalten Bestandteil Ihrer Strategie mobile Engagement. Diese werden unterschiedlichen zwischen Clientanwendungen. [Media Playbook Vorlage] [ Media Playbook link] bereitgestellten Azure Mobile Engagement hilft Tag Plan mit einer bestimmten Methode erstellen. Verwenden des **Tag** -Arbeitsblatts als Leitfaden für die Tags Bauplan.

Beim Definieren eines Abschnitts Tag im Arbeitsblatt sehr spezifisch sein. Dies ist sehr wichtig, um Verwechslungen zu vermeiden. Die Details jedes Szenario Erwartetes, in der jeder Tag gesendet werden. Zählen der Name der Aktivität, wobei jedes Tag eingebettet ist. Dies sollte alle **Informative** Teil des Arbeitsblatts enthalten sein. Das Tag-Arbeitsblatt sollte die wichtigste Grundlage für Test-Überprüfung. 

Im Abschnitt **Daten sammeln** sollten Ihr Entwicklungsteam finden, Typen, Namen, Werte und zusätzlichen Informationen Schlüssel-/Wertpaare erforderlich, für jeden Tag in die Anwendung eingebettet werden.

Überprüfen des Plans Tag mit allen Teams mit dem Projekt verbundenen empfohlen. Korrekturen und bestätigen Sie, dass alles für Marketing- und Development-Teams.

**Pflichtenheft** Arbeitsblatt kann zu helfen, die Beteiligten im Projekt verwendet werden.


#### <a name="data-types"></a>Datentypen

Dies sind häufige Arten von Unterstützung von Azure Mobile Engagement Daten.

###### <a name="devices-and-users"></a>Geräte und Benutzer

Azure Mobile Engagement identifiziert Benutzer generiert einen eindeutigen Bezeichner für jedes Gerät. Dieser Bezeichner wird die Geräte-ID (Deviceid) bezeichnet. So wird, dass alle Programme auf dem gleichen Gerät denselben Gerätebezeichner freigeben.

###### <a name="sessions-and-activities"></a>Sessions und Aktivitäten

Eine Sitzung stellt eine Instanz der Anwendung von einem Benutzer ausgeführt wird. Die Sitzung umfasst der Benutzer die Anwendung, bis zum Anschlag startet ab.

Eine Aktivität ist eine logische Gruppierung mehrerer Dinge, die die Anwendung während einer Sitzung kann. Ist in der Regel einen bestimmten Bildschirm in der app aber es ist nichts von der Logik der Anwendung definiert. Zumindest sollten Sie jeden Bildschirm oder Aktivität für Ihre Anwendung markieren. Der Benutzerpfad verstehen können.


###### <a name="events"></a>Ereignisse

Ereignisse werden verwendet, um Benutzerinteraktion mit der Anwendung melden. Sie können instant, wie Inhalte oder Starten eines Videos handeln. Tagging Ereignisse bieten Daten, die zeigen, wie Benutzer mit der Anwendung interagieren. 

###### <a name="jobs"></a>Aufträge

Aufträge werden verwendet, um Aktionen zu melden, die eine Dauer. Beispiele wären:

- API-Aufrufe ausführen
- Anzeigedauer anzeigen
- Hintergrund Aufgaben Dauer 
- Einkauf Prozessdauer
- Anzeigen von Videos


###### <a name="errors"></a>Fehler

Fehler wird von der Anwendung erkannte Probleme zu melden. Z. B. falsche Benutzeraktionen oder Fehler beim Aufrufen der API.

###### <a name="application-information"></a>Anwendungsinformationen

Informationen zur Anwendung (App-Info) wird zum tag-Daten eine Benutzeroberfläche mit einer Anwendung. Es wird durch eine Benutzerinteraktion mit der Anwendung generiert. 

Für einen Schlüssel angegebenen app-Informationen verfolgt von Azure Mobile Engagement nur den letzten Wert (kein Verlauf). App-Info zeigt den Status Ihrer app oder Endbenutzer. Beispielsweise der Status oder bevorzugten Produktgruppe des Benutzers.

###### <a name="crash-data"></a>Absturzdaten

Abstürzen Sie Daten automatisch von der Anwendung nicht behandelt Mobile Engagement SDK Berichte Anwendungsfehler. Zum Beispiel eine nicht behandelte Ausnahme auftritt.


###### <a name="extra-data"></a>Zusätzliche Daten

Ereignisse, Fehler, Aktivitäten und Projekte können mit Parametern erweitert werden. Zusätzliche Informationen, die ein Entwickler bestimmte Daten aus der Anwendung möglicherweise ist Dies ist wichtig für abgestimmte Segmentierung definieren. 

Beispielsweise wird der Wert eines "Artikel" können Sie Segment Endbenutzer basierend auf, die diesem Artikel angezeigt. Allerdings kann das nicht ausreichen. Es möglicherweise besser, wenn die gleiche "Artikel" Kategorie zusätzliche Informationen wie "Nachrichtenkategorie" in einer Aktivität ebenfalls. Dies wäre die bevorzugten Kategorien für den Benutzer dynamisch bestimmt. 

Schlüssel-Wert-Paar zusätzlichen Informationen gemeldet. Im Beispiel für diese Media-Anwendung wäre die zusätzlichen Informationen "Nachrichtenkategorie" den Wert für diese Kategorie. Beispielsweise "Sports", "Wirtschaft" oder "Politik".





#### <a name="tag-and-sdk-integration"></a>Tag und SDK-integration 

Führen Sie Schritt für Schritt Anleitung für Ihre app integrieren Azure Mobile Engagement SDK Dokumentation [Engagement SDK Integration](mobile-engagement-windows-store-integrate-engagement.md) Azure-Website. Wählen Sie Links oben auf der Seite mit Zielplattform.

Es wird empfohlen, Projekte für zwei apps Azure Mobile Engagement aufbaut. Eines für Entwicklung und Test Staging und Produktion Staging. Das IT-Team kann dann fördern von Test Staging zur Produktion der Benutzerakzeptanztest erfolgreich ist.



#### <a name="user-acceptance-testing-uat"></a>Benutzerakzeptanztest (UAT)

Testen der Benutzerakzeptanz (UAT) umfasst sicherstellen, dass alles erwartungsgemäß funktioniert. Arbeit abgeschlossen werden kann und alle erforderlichen Daten auf Grundlage Ihres Plans Tag:
 
- Informationen zu Tags sollten entsprechend dokumentierte AZME Konzepte
- Alle Informationen werden gesammelt (einschließlich zusätzliche Info Wert App Info)
- Nomenklatur entspricht nach Liquiditätskonten Tag planen
- Es gibt keine doppelten Tags gesendet

Testen Sie alle Typen von Verhalten im Infobereich, die Sie in Ihrer Anwendung eingebettet haben

- Legt Ankündigungen, Umfragen, Daten aus Anwendung und in der Anwendung
- Text-Web-Ansichten
- Badge Update Kategorien



#### <a name="setup"></a>Setup

Azure Mobile Engagement ist sehr einfach. Alle Informationen zur Benutzeroberfläche steht auf der Website Azure Mobile Engagement [die Benutzeroberfläche Navigieren](mobile-engagement-user-interface-home.md).

Es wird empfohlen, dass die richtigen Rollen und Rollenmitgliedschaften für die Benutzer des Projekts beginnen. So können Sie Zugang zur Plattform für alle Benutzer verwalten. Die Funktionen umfassen:

- Administratoren
- Entwickler
- Zuschauer 

Nachher:
- Registrieren Sie Ihre DeviceID auf Ihrem Gerät testen.
- Gehen Sie zu Ihrem Konto und richten Sie die Zeitzone Diagramme und Benachrichtigung Lieferung Zeit Ihrer Zeitzone.
- Die Einstellung der Anwendung und Registrieren der "App-Infos" Ziel Endbenutzer erreichen müssen.

Lesen Sie Informationen für Ihre ersten Push Notification Werbekampagne [zu verwenden und Verwalten von Push für Ihre Endbenutzer erreichen](mobile-engagement-how-tos.md).



## <a name="conclusion"></a>Abschluss


Engagement sind iterative und Sie sollte kontinuierlich verbessern Ihre Sie ausprobieren, was für Ihre Anwendung am besten geeignet. 

Zunächst beim Entwickeln mit Strategien versuchen ein gesamtes globales Engagement Strategie. Gehen Sie Schritt identifizieren KPIs und wie Sie sie nutzen können vor. Engagement-Strategie ist für jede Anwendung eindeutig.

Nachdem Sie einige Erfahrung entwickelt haben sollten Sie Ihre Programme Engagement Folgendes hinzufügen:

- Tracking: Sie erhalten Benutzer und wahrscheinlich Datenerfassung Quellen definieren. Azure mobile Engagement kann Datenerfassung Datenquellen verknüpft werden. Dadurch werden Leistungen der jeweiligen Quelle überwachen. Diese Informationen werden zur Maximierung Ihrer Investitionen Übernahme interessant. 

- A / B-Tests: Dies ist ein wesentlicher Bestandteil Engagement-Programm. Jede Anwendung hat eigene Besonderheiten. A / B-Tests können Sie Ihr Programm Engagement verbessern.

- Standort: Dies ist eine große Chance für Marken. Dadurch erreichen Sie am richtigen Ort und Zeit. Es wird empfohlen, überprüfen, ob Sie genügend Endbenutzerdaten vor Standort gesammelt haben.

- Datenpush: Daten-Push ist eine unsichtbare. Datenpush ermöglicht die Anwendung basierend auf Endbenutzerverhalten anpassen. Beispielsweise wenn ein Benutzersegment oft Produkte berät, kann Eigentümer app datenpush senden ihre Homepage mit Hightech-personalisieren.



## <a name="next-steps"></a>Nächste Schritte

- [Azure Mobile Engagement Konto erstellen](mobile-engagement-create.md).
- [Definieren der Strategie für Mobile Engagement](mobile-engagement-define-your-mobile-engagement-strategy.md) erfahren Sie mehr über Ihre Mobile Engagement Strategie besuchen.



  

<!--Image references-->


<!--Link references-->
[Media Playbook link]: https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks
