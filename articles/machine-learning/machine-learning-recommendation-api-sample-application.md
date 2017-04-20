<properties 
    pageTitle="Allgemeine Operationen in der Computer lernen empfohlene API | Microsoft Azure" 
    description="Azure ML Empfehlung Sample Application" 
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


# <a name="recommendations-api-sample-application-walkthrough"></a>Empfohlene API Beispiel Anwendung Exemplarische Vorgehensweise

>[AZURE.NOTE]Sie sollten diese Version anstelle des empfohlenen API kognitive starten. Empfohlene kognitive Service wird dieser Dienst ersetzt und alle neuen Features es entwickelt. Es verfügt über neue Funktionen wie Batchverarbeitung Support besser API Explorer eine sauberere API-Oberfläche und Anmeldung/Abrechnung Erfahrung, usw..
> Weitere Informationen zu [neuen kognitive Dienst migrieren](http://aka.ms/recomigrate)

##<a name="purpose"></a>Zweck

Dieses Dokument zeigt die Verwendung des Azure Machine Learning Recommendations API über eine [Beispiel-Anwendung](https://code.msdn.microsoft.com/Recommendations-144df403).

Diese Anwendung soll nicht vollständige Funktionen verwendet, noch es die APIs. Es zeigt einige allgemeine Vorgänge ausführen, wenn Sie zuerst mit der Empfehlung maschinelles lernen wiedergeben möchten. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="introduction-to-machine-learning-recommendation-service"></a>Einführung maschinelles lernen Empfehlung

Empfehlung über Computerlernen empfehlungsservice sind aktiviert, wenn Sie eine empfehlungsmodell anhand der folgenden Daten:

* Eine Sammlung von Elementen zu empfehlen, auch einen Katalog
* Daten, die Verwendung pro Benutzer oder Sitzung (dieser kann mit der Zeit über die Datenerfassung nicht als Teil der Beispiel-app abgerufen werden)

Nach der Erstellung eines empfehlungsmodells können sie vorhersagen Elemente, die ein Benutzer möglicherweise interessiert, gemäß Artikel (oder ein einzelnes Element) den Benutzer auswählt.

Zum vorherige Szenario zu aktivieren, gehen Sie im Dienst Empfehlung maschinelles lernen:

* Erstellen eines Modells: Dies ist ein logischer Container, der die Daten (Katalog und Nutzung) und Vorhersage-Modelle enthält. Jedes Modellcontainer wird über eine eindeutige ID identifiziert, die bei der Erstellung zugewiesen wird. Diese ID wird die Modell-ID aufgerufen und wird durch die meisten APIs verwendet. 
* Upload Katalog: beim Erstellen ein Modellcontainer stehen, einen Katalog.

**Hinweis**: Modelle erzeugen und Hochladen auf einen Katalog in der Regel erfolgt einmal für den Modell-Lebenszyklus.

* Verwendung hochladen: dieses Modellcontainer Daten hinzugefügt.
* Empfehlung Modell: Nachdem Sie ausreichend Daten haben, erstellen das empfehlungsmodell. Dieser Vorgang verwendet Top Machine Learning-Algorithmen zum Erstellen eines empfehlungsmodells. Jedem Build ist eine eindeutige ID zugeordnet Sie müssen zu dieser ID ist für die Funktionsweise von einige APIs erforderlich.
* Überwachen des Entwicklungsprozesses: ein Modellbuild Empfehlung ist ein asynchroner Vorgang, und es kann mehrere Minuten je nach Datenmenge (Katalog und Nutzung) und die Buildparameter mehrere Stunden dauern. Daher müssen Sie den Build zu überwachen. Empfehlungsmodell wird nur erstellt, wenn seine zugeordneten Build erfolgreich abgeschlossen wird.
* (Optional) Wählen Sie eine aktive Empfehlung Modell: Dieser Schritt ist nur erforderlich, wenn mehr als eine empfehlungsmodell in Ihrem Modellcontainer erstellt haben. Fragen zu empfohlenen ohne aktive empfehlungsmodell wird automatisch vom System standardmäßig aktive Buildkonfiguration umgeleitet. 

**Hinweis**: eine aktive empfehlungsmodell Produktionsbeginn und für Arbeitslast erstellt. Dies unterscheidet sich von einem nicht aktiven empfehlungsmodell in einer Test-Umgebung bleibt (Staging bezeichnet).

* Lassen Sie sich: nach dem Einrichten ein empfehlungsmodells lösen Sie Vorschläge für ein einzelnes Element oder eine Liste von Elementen, die Sie auswählen. 

Normalerweise wird erhalten Empfehlung für einen bestimmten Zeitraum aufrufen. Während dieser Zeit können Sie Daten System Empfehlung maschinelles lernen umleiten, die diese Daten dem angegebenen Modell Container hinzugefügt. Wenn Sie genügend Daten haben, können Sie ein neues empfehlungsmodell erstellen, das zusätzliche Daten enthält. 

##<a name="prerequisites"></a>Erforderliche Komponenten

* Visual Studio 2013
* Internetzugriff 
* Abonnement für empfohlene API (https://datamarket.azure.com/dataset/amla/recommendations).

##<a name="azure-machine-learning-sample-app-solution"></a>Azure Machine Learning Beispielprojektmappe app

Diese Lösung enthält Quellcode, Beispiel für die Verwendung, Katalog und Richtlinien die Pakete herunterladen, die für die Kompilierung erforderlich sind.

##<a name="the-apis-used"></a>Verwendet APIs

Die Anwendung verwendet maschinelles lernen Empfehlung Funktionen über eine Teilmenge der verfügbaren APIs. Die folgenden APIs werden in der Anwendung veranschaulicht:

* Modell erstellen: erstellt einen logischen Container zum Speichern von Daten und Empfehlung Modelle. Ein Modell durch einen Namen identifiziert und Sie können nicht mehrere Modelle mit demselben Namen erstellen.
* Katalog hochladen: Katalogdaten hochladen.
* Verwendung hochladen: Daten hochladen.
* Build ausgelöst: empfehlungsmodell erstellen.
* Überwachen der Buildausführung: Überwachen des Status eines Builds Empfehlung Modell verwenden.
* Wählen Sie ein Modell für die Empfehlung: die empfehlungsmodell standardmäßig für einen bestimmten Modellcontainer angeben. Dieser Schritt ist erforderlich, wenn Sie mehr als eine empfehlungsmodell und einen nicht aktiven Build als aktive empfehlungsmodell aktivieren möchten.
* Empfehlung erhalten: empfohlene Elemente ein bestimmtes einzelnes Element oder einen Satz von Elementen abrufen. 

Eine vollständige Beschreibung der APIs finden Sie in der Dokumentation zu Microsoft Azure Marketplace. 

**Hinweis**: ein Modell können mehrere Builds mit der Zeit (nicht gleichzeitig). Jedem Build ist gleich einen aktualisierten Katalog oder zusätzliche Daten erstellt.

## <a name="common-pitfalls"></a>Häufige Probleme

* Sie müssen Ihren Benutzernamen und Ihr Microsoft Azure Marketplace Hauptkonto Schlüssel die Beispiel-app ausgeführt.
* Die Beispiel-app durchlaufend schlägt fehl. Die Anwendungsablaufs enthält erstellen, hochladen, erstellen den Monitor und erste Vorschläge aus einem Modell. daher fehl sie aufeinanderfolgende Ausführung, wenn der Modellname zwischen Aufrufe nicht zu ändern.
* Empfohlene möglicherweise keine Daten zurück. Die Beispiel-app verwendet eine sehr kleine Datei Katalog und Nutzung. Daher haben einige Elemente aus dem Katalog keine empfohlene Elemente.

## <a name="disclaimer"></a>Haftungsausschluss
Die Beispiel-app soll nicht in der Produktion ausgeführt werden. Die Daten in den Katalog ist sehr klein und stellen eine sinnvolle empfehlungsmodell. Die Daten werden als Beispiel bereitgestellt. 
 
