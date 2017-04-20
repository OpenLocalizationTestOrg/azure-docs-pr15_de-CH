<properties 
    pageTitle="Einrichten und Verwenden der Computer lernen empfohlene API | Microsoft Azure" 
    description="Microsoft Empfehlung API integriert Azure Machine Learning FAQ" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/> 

#<a name="setting-up-and-using-machine-learning-recommendations-api-faq"></a>Einrichten und Verwenden von Computer lernen empfohlene API FAQ


**Was ist die Empfehlung?**

>[AZURE.NOTE]Sie sollten diese Version anstelle des empfohlenen API kognitive starten. Empfohlene kognitive Service wird dieser Dienst ersetzt und alle neuen Features es entwickelt. Es verfügt über neue Funktionen wie Batchverarbeitung Support besser API Explorer eine sauberere API-Oberfläche und Anmeldung/Abrechnung Erfahrung, usw..
> Weitere Informationen zu [neuen kognitive Dienst migrieren](http://aka.ms/recomigrate)

Organisationen und Unternehmen, die auf Cross-Selling Up-Selling und ihren Kunden auf RECOMMENDATIONS in Azure Machine Learning bietet eine Empfehlung Self-service-Modul. Es ist eine Implementierung von collaborative Filterung, die Matrix faktorisierung wie der Core-Algorithmus verwendet. Entwickler können mithilfe von REST-APIs RECOMMENDATIONS zugreifen. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**Was kann ich mit Empfehlung?**

Empfehlung akzeptiert als Eingabe ein Element oder einen Satz von Elementen und gibt eine Liste der relevanten Recommendations. Beispiel: ein Kunde von einem online-Händler ein Produkt klickt. Online-Händler sendet das Produkt als Eingabe empfohlen, wiederum ruft eine Liste der Produkte und entscheidet, welche dieser Produkte an den Debitor angezeigt. Sie Vorschläge Ihres online-Kaufhauses optimieren oder Ihren internen informieren verwenden möchten sales Department oder Call Center.

**Gibt es Einschränkungen Verwendung?**

Empfehlung hat die folgenden Nachteile der Verwendung:
* Maximale Anzahl der Modelle pro Abonnement: 10
* Maximale Anzahl von Elementen, die ein Katalog enthalten kann: 100.000
* Maximal bleiben Verwendung-Punkt ist ~ 5.000.000. Die älteste gelöscht Wenn neue hochgeladen oder gemeldet werden.
* Maximale Größe von Daten (z. B. Katalogdaten importieren Daten importieren) e-Mail gesendet werden können beträgt 200 MB
* Anzahl der Transaktionen pro Sekunde eine empfohlene Modell erstellen, die nicht aktiv ist (TPS) ist ca. 2 TPS. Ein empfohlene Modellbuild active kann bis zu 20 TPS enthalten.

##<a name="purchase-and-billing"></a>Bestellung und Rechnung 


**Wie viel kostet empfohlene Zeitraum starten?**

Empfohlen wird ein Abonnement-Service. Laden basiert auf Transaktionen pro Monat. [Angebotsseite] überprüfen (https://datamarket.azure.com/dataset/amla/recommendations) Microsoft Azure Marketplace für Preisinformationen.

**Gibt alle Kosten mit empfohlenen verfolgen und Speichern von Benutzeraktivitäten für mich?**

Derzeit nicht.

**Müssen die Empfehlung eine kostenlose Testversion?**

Gibt eine kostenlose ist auf 10.000 Transaktionen pro Monat.

**Wann wird Recommendations in Rechnung gestellt?**

Ein Abonnement ist jedes Abonnement, für das eine monatliche Gebühr. Wenn Sie ein Abonnement erwerben, Zahlen Sie sofort für den ersten Monat verwenden. Die Zahlen des Betrags auf der Anmeldeseite (plus Steuern) zugeordnet ist. Monatliche Gebühr erfolgt jeden Monat am gleichen Kalenderdatum als die ursprüngliche Bestellung, bis Sie das Abonnement kündigen. 

**Wie aktualisiere ich auf eine höhere Ebene Service?**

Kaufen oder aktualisieren Sie Ihr Abonnement auf der [Angebot] (https://datamarket.azure.com/dataset/amla/recommendations) Seite Microsoft Azure Marketplace.

Wenn Sie ein Abonnement aktualisieren:

* Auf Ihr altes Abonnement verbleibenden Transaktionen werden nicht auf Ihr neues Abonnement hinzugefügt. 
* Gesamtpreis für das neue Abonnement zu bezahlen, auch wenn Sie nicht verwendete Transaktionen auf Ihr altes Abonnement.

Prozess ein Abonnement aktualisieren:

* Nevigate [Angebot Seite] (https://datamarket.azure.com/dataset/amla/recommendations).
* Melden Sie sich auf dem Markt, wenn Sie nicht bereits angemeldet sind.
* Im rechten Bereich werden die verfügbaren Pläne aufgelistet. Klicken Sie auf das Optionsfeld für den Plan zu aktualisieren möchten.
* Wenn Sie aktualisieren möchten, klicken Sie auf **OK**. Wenn Sie nicht aktualisieren möchten, klicken Sie auf **Abbrechen**.

**Wichtig** Lesen Sie vor der Aktualisierung, da Abrechnung und Verwendung Auswirkungen im Dialogfeld.

**Wann läuft mein Abonnement auf Empfehlung?**

Ihr Abonnement wird beendet, wenn Sie Abbrechen. Wenn Sie Ihre Abonnements zu kündigen möchten, finden Sie folgende.

**Wie wird Abbrechen mein Abonnement Recommendations?**

Gehen Sie folgendermaßen vor, um Ihr Abonnement zu kündigen. Ist Ihr aktuelle Abonnement ein kostenpflichtiges Abonnement weiterhin Ihr Abonnement gültig bis zum Ende des aktuellen Abrechnungszeitraums. Benötigen Sie die Stornierung sofort wenden Sie [Support von Microsoft](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).

**Hinweis** Keine Rückerstattung wird vor dem Ende einer Abrechnungsperiode oder nicht verwendete Transaktionen in einen Abrechnungszeitraum Abbrechen.

* Navigieren Sie zum [Angebotsseite] (https://datamarket.azure.com/dataset/amla/recommendations).
* Melden Sie sich auf dem Markt, wenn Sie nicht bereits angemeldet sind.
* Klicken Sie auf rechts neben der DatasetName und Status **Abbrechen** . Dieses Abonnement bis zum Ende des aktuellen Abrechnungszeitraums verwenden oder die Buchung erreicht ist (je nachdem, was zuerst eintritt).

Wenn Sie Ihr Abonnement kündigen, sofort damit Sie ein neues Abonnement erwerben möchten, Datei ein Ticket an [Microsoft Support Services](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).

##<a name="getting-started-with-recommendations"></a>Erste Schritte mit Empfehlung

**Ist eine Empfehlung für mich?** 

Empfohlene Computer Lernen ist für Organisationen und Unternehmen, die auf Cross-Selling- und Up-Selling Produkte oder Dienstleistungen für ihre Kunden benötigen. Haben Sie eine kundenorientierte Website, ein Vertriebsmitarbeiter einen Vertriebsmitarbeiter oder ein Call-Center und wenn Sie einen Katalog mehr als ein paar Dutzend Produkte oder Dienstleistungen anbieten, Ihre Vorschläge verwenden. 

Mit empfohlenen soll relativ einfach. Die aktuelle API-Version ist grundlegende Programmierkenntnisse erforderlich. Wenn Sie Hilfe benötigen, wenden Sie sich an Händler Ihre Website entwickelt. Haben Sie eine IT-Abteilung oder ein interner Entwickler, sollten sie können Vorschläge für Sie arbeiten. 

**Was sind Voraussetzung für empfohlene einrichten?**

Empfohlene erfordert ein Protokoll der Benutzerauswahl auf Katalog. Wenn Sie nicht haben ein Protokoll mit Kundenwebsite haben und Vorschläge können Sie Benutzeraktivitäten sammeln. 

Empfohlene erfordert einen Katalog Ihrer Produkte oder Dienstleistungen. Wenn Sie nicht den Katalog, Vorschläge können Kundendaten und destillieren einen Katalog. Ein impliziter Katalog enthalten Elemente, die nicht als Teil des Benutzertransaktionen gemeldet wurden.

**Wie richte ich Vorschläge zum ersten Mal ein?**

Nach [abonnieren] (https://datamarket.azure.com/dataset/amla/recommendations) auf Empfehlung, verwenden Sie die API-Dokumentation in [Azure Machine Learning Recommendations Quick Start Guide](machine-learning-recommendation-api-quick-start-guide.md) , um den Dienst einzurichten.

**Wo finde ich die API-Dokumentation?** 

Die API-Dokumentation ist [Azure Machine Learning Recommendations Quick Start Guide](machine-learning-recommendation-api-quick-start-guide.md).

**Welche Optionen haben Katalog und Nutzungsdaten Recommendations hochgeladen werden?**

Sie haben zwei Optionen für das Hochladen der Daten Katalog und Nutzung: exportieren Sie die Daten aus Ihrem CRM-System oder andere Protokolle und Recommendations hochladen oder die Website, die Benutzeraktivitäten verfolgt Tags hinzufügen. Wenn Sie diese Methode verwenden, werden die Daten in Azure gespeichert.

##<a name="maintenance-and-support"></a>Wartung und support

**Wie groß können meine Daten festgelegt?**

Jeder Datensatz kann bis zu 100.000 Katalogelemente enthalten und bis zu 2048 MB Daten.
Darüber hinaus kann ein Abonnement maximal 10 Datensätze (Modelle) enthalten.

**Wo erhalte ich technischen Support für Vorschläge?**

Support ist auf der Website [Microsoft Azure unterstützt](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) .

**Wo finde ich die Vereinbarung?**

[Microsoft Azure maschinelles lernen empfohlene API Vertragsbedingungen](https://datamarket.azure.com/dataset/amla/recommendations#terms).



 
