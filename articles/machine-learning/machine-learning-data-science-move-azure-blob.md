<properties
    pageTitle="Verschieben von Daten zu und von Azure BLOB-Speicher | Microsoft Azure"
    description="Verschieben von Daten zu und von Azure BLOB-Speicher"
    services="machine-learning,storage"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="bradsev;sachouks" />

# <a name="move-data-to-and-from-azure-blob-storage"></a>Verschieben von Daten zu und von Azure BLOB-Speicher

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

Hinweise auf Daten und/oder von Azure BLOB-Speicher verschoben werden hier verknüpft:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]
 
Welche Methode am besten ist hängt vom Szenario ab. [Szenarien für die erweiterte Analyse in Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md) -Artikel können Sie die Ressourcen bestimmen für eine Vielzahl von Daten wissenschaftlichen Workflows verwendet die erweiterte Analyse.

> [AZURE.NOTE] Eine vollständige Einführung in Azure BLOB-Speicher finden Sie in [Azure Blob Grundlagen](../storage/storage-dotnet-how-to-use-blobs.md) und [Azure BLOB-Dienst](https://msdn.microsoft.com/library/azure/dd179376.aspx).

Alternativ können Sie die [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) : 

- Erstellen Sie und Planen Sie eine Pipeline, die Daten von Azure BLOB-Speicher heruntergeladen 
- veröffentlichte Azure Machine Learning-Webdienst übergeben, 
- Empfangen der Ergebnisse Vorhersageanalytik und 
- Hochladen der Ergebnisse an Speicher. 

Weitere Informationen finden Sie unter [Erstellen vorhersehbaren Rohrleitungen Azure Data Factory und Azure maschinelles lernen](../data-factory/data-factory-azure-ml-batch-execution-activity.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Dieses Dokument wird vorausgesetzt, dass Sie ein Azure-Abonnement ein Speicherkonto und entsprechende Speicherschlüssel für dieses Konto. Bevor Sie Daten hochladen/herunterladen, müssen Sie Ihre Azure-Konto und Speicherschlüssel kennen.

- Zum Einrichten der Azure-Abonnement finden Sie [kostenlose Testversion für einen Monat](https://azure.microsoft.com/pricing/free-trial/).
- Anleitung zum Erstellen ein Speicherkonto und Konto und wichtige Informationen anzeigen Sie [zur Azure-Speicherkonten](../storage/storage-create-storage-account.md)
