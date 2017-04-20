
<properties
    pageTitle="Migrieren Sie von DataMarket empfohlene API Azure kognitive Services empfohlene API | Microsoft Azure"
    description="Azure maschinelles lernen Recommendations - Empfehlung kognitive Dienst migrieren"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="luisca"/>


# <a name="migrate-to-azure-cognitive-services-recommendations-api-from-the-datamarket-recommendations-api"></a>Migrieren Sie von DataMarket empfohlene API Azure kognitive Services empfohlene API
Dieser Artikel veranschaulicht die [Microsoft DataMarket empfohlene API](https://datamarket.azure.com/dataset/amla/recommendations) [Azure kognitive Services empfohlene API](https://www.microsoft.com/cognitive-services/en-us/recommendations-api)migrieren.

DataMarket empfohlene API akzeptieren Neukunden 31. Dezember 2016 beendet und wird vom 28. Februar 2017 veraltet.

## <a name="how-do-i-start-using-the-azure-cognitive-services-recommendations-api"></a>Vorgehensweise mit der Azure-API Recommendations kognitive Services?

Kognitive Services empfohlene API migrieren, führen Sie folgende Schritte aus:

1.  Haben Sie bereits ein Azure-Abonnement für eines [Anmelden](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1) . 

1.  [Quick Start Guide](cognitive-services-recommendations-quick-start.md) für kognitive Services empfohlene API und programmgesteuert verwenden Anleitung erhalten. 

1.  Gehen Sie zu der [Zielseite kognitive Services empfohlene API](https://www.microsoft.com/cognitive-services/en-us/recommendations-api) den Dienst und Dokumentation.

## <a name="i-used-the-recommendations-ui-to-build-my-models-is-there-a-similar-tool-for-the-cognitive-services-recommendations-api"></a>Habe die empfohlene Benutzeroberfläche Modelle erstellen. Gibt ein ähnliches Tool für kognitive Services empfohlene API?

! Die URL für die neue [Empfohlene Benutzeroberfläche](http://recommendations-portal.azurewebsites.net/) ist http://recommendations-portal.azurewebsites.net. 

>[AZURE.NOTE] Anmeldedaten DataMarket funktionieren hier nicht. Für ein Abonnement im Azure-Portal anmelden und das Konto Schlüssel mithilfe der neuen [Benutzeroberfläche empfohlenen](http://recommendations-portal.azurewebsites.net/)erforderlich.
Haben Sie eine Konto-Schlüssel finden Sie unter Aufgabe 1 [Quick Start Guide](cognitive-services-recommendations-quick-start.md).

##<a name="is-the-new-api-format-the-same-as-the-datamarket-recommendations-api"></a>Ist das neue API-Format DataMarket empfohlene API identisch?

Die API unterstützt gleichen Szenarien und Prozessabläufe als die Szenarien in der DataMarket-Version unterstützt die tatsächliche API-Schnittstelle wurden [Microsoft REST API-Richtlinien](https://github.com/Microsoft/api-guidelines/blob/master/Guidelines.md)entsprechen. Die APIs sind konsistenter und Arbeit besser mit tools, die stolz unterstützen.

Checken Sie die APIs verstehen [API-Explorer](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3db)aus.
*Versuchen Sie* Schaltfläche Testen einen API-Aufruf verwenden. Das Format von Dateien belegten empfohlene API (Katalog und Nutzung Dateien) wurde nicht geändert, damit Sie weiterverwenden können denselben Dateien bzw. Infrastruktur zum Generieren dieser Dateien erstellt haben.

##<a name="what-are-some-new-features-in-the-cognitive-services-recommendations-api"></a>Was sind einige neue Features in kognitive Services Recommendations-API?

In den letzten zwei Monaten haben wir neue Funktionen für kognitive Services empfohlene API veröffentlicht:
-   Empfohlene Benutzeroberfläche für Schulung und Modelle testen, ohne eine einzige Codezeile schreiben
-   Batch Zählsystems Tausende empfohlenen gleichzeitig bereitstellen
-   Metriken Unterstützung Abfrage die Qualität der Empfehlung Modelle
-   Unterstützung für Geschäftsregeln
-   Möglichkeit zum Auflisten und Verwendung und Katalog Dateien
-   Die Qualität der Artikel KEs in einem Modell Recommendations abfragen Ranking Buildunterstützung
-   Zusätzliche Möglichkeit, für ein Produkt im Katalog durchsuchen

## <a name="when-does-microsoft-stop-supporting-the-datamarket-recommendations-api"></a>Wann wird Microsoft beendet DataMarket empfohlene API unterstützt?

[Empfohlene API auf DataMarket](https://datamarket.azure.com/dataset/amla/recommendations) neue Kunden nach dem 31. Dezember 2016 akzeptiert und wird vollständig vom 28. Februar 2017 veraltet. 

## <a name="what-if-i-dont-have-the-data-that-i-need-to-recreate-my-models-in-the-cognitive-services-recommendations-api"></a>Was passiert, wenn ich Daten haben, die ich meine Modelle kognitive Services empfohlene API erstellen?

Wir möchten Sie diesen Übergang so einfach wie möglich machen. Wir unterstützen Sie die alten Modellen von Ihrem DataMarket auf Ihr neues Abonnement Azure kognitive Services empfohlene API verschieben. Wenden Sie sich an uns unter [mlapi@microsoft.com](mailto://mlapi@microsoft.com). Wir bitten Sie Ihre Abonnement-ID und die kognitive Services-Abonnement-ID DataMarket bereitstellen, bevor wir die Modelle mit dem neuen Konto zuordnen.
