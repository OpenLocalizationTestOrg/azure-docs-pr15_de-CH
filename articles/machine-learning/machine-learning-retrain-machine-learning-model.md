<properties
    pageTitle="Eine Maschine Lernmodell umschulen | Microsoft Azure"
    description="Informationen Sie zum Trainieren eines Modells und aktualisieren Sie den Webdienst neu ausgebildete Modell in Azure Machine Learning verwenden."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondl"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="v-donglo"/>

# <a name="retrain-a-machine-learning-model"></a>Eine Maschine Lernmodell Umschulung

Als Teil des Prozesses operationalisierung lernen Modelle in Azure Machine Learning Modell geschult und gespeichert. Sie können dann damit prädikativen Webdienst erstellen. Der Webdienst kann dann in Websites, Dashboards und mobiler apps genutzt werden. 

Erstellten Modelle mit Computerlernen sind nicht statisch. Sobald neue Daten verfügbar sind oder der Consumer der API Daten muss das Modell einarbeiten. 

Umschulung kann häufig auftreten. Mit dem Feature programmgesteuerte Umschulung API können programmgesteuert umschulen Umschulung APIs mit und Webdienst neu ausgebildeten Modell aktualisieren. 

Dieses Dokument beschreibt die Umschulung und veranschaulicht die Umschulung APIs verwenden.

## <a name="why-retrain-defining-the-problem"></a>Warum umschulen: Definieren des Problems  

Als Teil der Schulung lernen Computer ist ein Modell mit Daten trainiert. Erstellten Modelle mit Computerlernen sind nicht statisch. Sobald neue Daten verfügbar sind oder der Consumer der API Daten muss das Modell einarbeiten.

In diesen Szenarien bequem eine programmgesteuerte API Sie oder der Consumer APIs Erstellen eines Clients, die regelmäßig eine einmalige oder regelmäßige mit Daten trainieren können. Sie können dann die Ergebnisse Umschulung und Web Service-API, um die neu ausgebildeten Modell aktualisieren.

>[AZURE.NOTE] Wenn Sie eine vorhandene Trainingsexperiment und neue Webdienst haben, möchten Sie Umschulung einen vorhandenen vorhersehbaren Webdienst anstatt gemäß Abschnitt dieser exemplarischen Vorgehensweise überprüfen.

## <a name="end-to-end-workflow"></a>End-to-End-workflow 

Der Prozess umfasst die folgenden Komponenten: ein Training experimentieren und ein Vorhersageexperiment als Webdienst veröffentlicht. Zum Aktivieren eines trainierten Modells Umschulung muss Experiment Schulung als Webdienst mit der Ausgabe eines trainierten Modells veröffentlicht werden. Dies ermöglicht den Zugriff auf das Modell Umschulung API. 

Die folgenden Schritte gelten für neu und klassische Web Services:

Erstellen Sie den anfänglichen vorhersehbaren Webdienst:

* Erstellen eines Experiments Schulung
* Erstellen Sie einen vorhersehbaren Web Test
* Bereitstellen eines vorhersehbaren Webdiensts

Trainieren Sie den Webdienst:

* Aktualisieren Sie Schulung Experiment zu Umschulung
* Umschulung Webdienst bereitstellen
* Trainieren das Modell mithilfe des Batch Execution Service

Eine exemplarische Vorgehensweise der vorherigen Schritte anzeigen Sie [umschulen maschinelles lernen Modelle programmgesteuert](machine-learning-retrain-models-programmatically.md)

Wenn Sie einen klassischen Webdienst bereitgestellt:

* Erstellen Sie eine neue vorhersehbaren Webdienst
* Abrufen der URL PATCH und code
* PATCH-URL verwenden Sie, um den neuen Endpunkt trainierte Modell zeigen 

Der vorherigen Schritte eine exemplarische Vorgehensweise finden Sie in der [klassischen Webdienst umschulen](machine-learning-retrain-a-classic-web-service.md).

Wenn Umschulung klassische Webdienst Probleme auftreten, finden Sie unter [Troubleshooting Umschulung ein Azure Machine Learning klassische Webdienst](machine-learning-troubleshooting-retraining-models.md).

Wenn Sie einen neuen Webdienst bereitgestellt:

* Der Azure-Ressourcen-Manager-Konto anmelden
* Abrufen der Webdienstdefinition
* Exportieren der Webdienstdefinition als JSON
* Aktualisieren Sie den Verweis auf die `ilearner` -Blob in JSON
* Web Service Definition JSON importieren
* Aktualisierung mit neuen Web Service Definition

Eine exemplarische Vorgehensweise der vorherigen Schritte finden Sie unter [Umschulung einen neuen Webdienst mit Computer Learning Management PowerShell-Cmdlets](machine-learning-retrain-new-web-service-using-powershell.md).

Der Prozess zum Einrichten eines klassischen Webdiensts Umschulung umfasst die folgenden Schritte:

![Umschulung Übersicht][1]

Der Prozess zum Einrichten von Umschulung für einen neuen Webdienst umfasst die folgenden Schritte aus:

![Umschulung Übersicht][7]

## <a name="other-resources"></a>Andere Ressourcen

- [Umschulung und Aktualisieren von Azure Machine Learning Modelle mit Azure Data Factory](https://azure.microsoft.com/blog/retraining-and-updating-azure-machine-learning-models-with-azure-data-factory/)
- [Erstellen Sie viele Computerlernen Modelle und Web-Endpunkte aus einem Experiment mit PowerShell](machine-learning-create-models-and-endpoints-with-powershell.md)
- [AML Umschulung Modelle mithilfe von APIs](https://www.youtube.com/watch?v=wwjglA8xllg) Video veranschaulicht maschinelles lernen Modelle in Azure Machine Learning umschulen Umschulung APIs und PowerShell verwenden.

<!--image links-->
[1]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE01.png
[7]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE07.png

