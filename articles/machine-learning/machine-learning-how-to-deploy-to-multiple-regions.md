<properties
    pageTitle="Einen Webdienst in mehrere Regionen bereitstellen | Microsoft Azure"
    description="Schritte (Kopie) einen neuen Webdienst anderen Regionen bereitstellen."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondl"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="v-donglo"/>

# <a name="how-to-deploy-a-web-service-to-multiple-regions"></a>Einen Webdienst in mehrere Regionen bereitstellen

Neue Azure Web Services können Sie problemlos einen Webdienst in mehrere Regionen bereitstellen, ohne mehrere Abonnements oder Arbeitsbereiche. 

Die Preise werden regionsspezifisch daher Sie Fakturierungsplan für jeden Bereich definieren müssen, in dem Sie den Web Service bereitstellen.

## <a name="to-create-a-plan-in-another-region"></a>Zum Erstellen eines Plans in einer anderen region

1. Melden Sie sich bei [Microsoft Azure Machine Learning WebServices](https://services.azureml.net/).
2. Klicken Sie auf die Menüoption **plant** .
3. Klicken Sie auf **neu**auf der Seite anzeigen.
4. Wählen Sie Dropdownliste **Abonnement** Abonnement in dem neue Plan befindet.
5. Wählen Sie einen Bereich für den neuen Plan Dropdown **Region** . Planen Sie Optionen für die ausgewählte Region wird im Abschnitt **Optionen planen** der Seite angezeigt.
6. Wählen Sie in der Dropdownliste **Ressourcengruppe** für den Plan. Informationen für Ressourcengruppen finden Sie unter [Verwalten von Azure Ressourcen-Portal](../azure-portal/resource-group-portal.md).
7. Geben Sie den Namen des Plans **Plan** Namen.
8. Unter **Optionen**klicken Sie auf die Rechnung für den neuen Plan.
9. Klicken Sie auf **Erstellen**.


## <a name="deploying-the-web-service-to-another-region"></a>Bereitstellen des Webdienstes in eine andere region

1. Klicken Sie auf die Option **Web Services** .
2. Wählen Sie den Webdienst in einer neuen Region bereitstellen.
3. Klicken Sie auf **Kopieren**.
4. **Name des Webdienstes**Geben Sie einen neuen Namen für den Webdienst.
5. **Web Service-Beschreibung**eine Beschreibung für den Webdienst.
6. Wählen Sie Dropdownliste **Abonnement** Abonnement in dem neuen Webdienst befindet.
7. Wählen Sie in der Dropdownliste **Ressourcengruppe** für den Webdienst. Informationen für Ressourcengruppen finden Sie unter [Verwalten von Azure Ressourcen-Portal](../azure-portal/resource-group-portal.md).
8. Dropdown **Bereich** wählen Sie die Region, den Web Service bereitstellen.
9. Wählen Sie Dropdown **Speicherkonto** ein Speicherkonto, den Webdienst zu speichern.
10. Dropdown **Preisplan** wählen Sie einen Plan in der Region, die Sie in Schritt 8 ausgewählt.
11. Klicken Sie auf **Kopieren**.

