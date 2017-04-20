<properties
    pageTitle="HDInsight Anwendung veröffentlichen | Microsoft Azure"
    description="Informationen Sie zum Erstellen und Veröffentlichen von HDInsight Anwendung."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/18/2016"
    ms.author="jgao"/>

# <a name="publish-hdinsight-applications-into-the-azure-marketplace"></a>HDInsight Anwendung in Azure Marketplace veröffentlichen

Eine HDInsight-Anwendung ist eine Anwendung, die Benutzer auf einem Linux-basierten HDInsight-Cluster installieren können. Diese Programme können von Microsoft, unabhängige Softwareanbieter (ISV) oder selbst entwickelt werden. In diesem Artikel erfahren Sie, wie eine HDInsight Anwendung in Azure Marketplace veröffentlichen.  Allgemeine Informationen zur Veröffentlichung in Azure Marketplace finden Sie [ein Angebot auf dem Markt Azure veröffentlichen](../marketplace-publishing/marketplace-publishing-getting-started.md).

HDInsight Programme verwenden *Bringen Ihre eigene Lizenz (BYOL)* -Modell, wobei Anwendungsanbieters Lizenzierung Anwendung für Endbenutzer und Endbenutzer nur für die Ressourcen, die sie, die wie HDInsight-Cluster und seinen VMs/Knoten erstellen Azure erhoben werden. Zu diesem Zeitpunkt Abrechnung für die Anwendung selbst nicht Azure erfolgt.

HDInsight Anwendung verwandte Artikel:

- [Installieren HDInsight Applikationen](hdinsight-apps-install-applications.md): erfahren Sie, wie eine Anwendung HDInsight Cluster installieren.
- [Benutzerdefinierte HDInsight Anwendung installieren](hdinsight-apps-install-custom-applications.md): Informationen zu benutzerdefinierten HDInsight Anwendung testen.

 
## <a name="prerequisites"></a>Erforderliche Komponenten

Um Ihre benutzerdefinierte Anwendung auf dem Markt zu senden, müssen erstellen und Ihre benutzerdefinierte Anwendung getestet. Finden Sie in folgenden Artikeln:

- [Benutzerdefinierte HDInsight Anwendung installieren](hdinsight-apps-install-custom-applications.md): Informationen zu benutzerdefinierten HDInsight Anwendung testen.

Sie müssen auch haben Konto der Entwickler. Finden Sie [ein Angebot auf dem Markt Azure veröffentlichen](../marketplace-publishing/marketplace-publishing-getting-started.md) und [Microsoft Developer-Konto erstellen](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).

## <a name="define-application"></a>Anwendung definieren

Umfasst zwei Schritte zum Veröffentlichen von Applikationen Azure Marketplace.  Zuerst definieren Sie eine **createUiDef.json** , um anzugeben, welche Cluster die Anwendung mit. und die Vorlage von Azure-Portal veröffentlichen. Folgendes ist eine Beispieldatei createUiDef.json.

    {
        "handler": "Microsoft.HDInsight",
        "version": "0.0.1-preview",
        "clusterFilters": {
            "types": ["Hadoop", "HBase", "Storm", "Spark"],
            "tiers": ["Standard", "Premium"],
            "versions": ["3.4"]
        }
    }


|Feld  | Beschreibung   | Mögliche Werte|
|-------|---------------|----------------|
|Typen  | Die Clustertypen, denen die Anwendung mit kompatibel ist.    |Hadoop, HBase, Sturm, Spark (oder eine beliebige Kombination dieser)|
|Ebenen  | Die Cluster-Ebenen, denen die Anwendung mit kompatibel ist.    |Standard, Premium (oder beide)|
|Versionen|  Die Anwendung ist kompatibel mit HDInsight Clustertyp.    |3.4|

## <a name="package-application"></a>Paketanwendung

Erstellen Sie eine Zip-Datei, die alle erforderliche Dateien für die Installation der HDInsight-Anwendung enthält. Die Zip-Datei [Veröffentlichen](#publish-application)Anwendung benötigen.

- [createUiDefinition.json](#define-application).
- mainTemplate.json. Finden Sie ein Beispiel unter [benutzerdefinierte HDInsight Anwendung zu installieren](hdinsight-apps-install-custom-applications.md).

    >[AZURE.IMPORTANT] Der Name der Anwendung installieren Skriptnamen muss für einen bestimmten Cluster mit dem folgenden Format eindeutig sein. Zusätzlich installieren und deinstallieren Skriptaktionen Idempotent sein, d. h. die Skripts kann aufgerufen werden repeatly gleichzeitig dasselbe Ergebnis.
    
    >   Name":" [Concat (' Farbton installieren v0 ','-', uniquestring('applicationName')] "
        
    >Beachten Sie, dass drei Teile der Skriptname:
        
    >   1. Ein Skript Präfix, der Name der Anwendung oder einen Namen für die Anwendung enthält.
    >   2. Ein "-" zur besseren Lesbarkeit.
    >   3. Eine eindeutige Zeichenfolgenfunktion als Parameter ein.

    >   Ein Beispiel ist die obigen enden zu: Farbton-Installation-v0-4wkahss55hlas in der Aktionsliste dauerhaften Skript. Ein Beispiel JSON-Nutzlast finden Sie unter [https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json](https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json).

- Alle erforderlichen Skripts.

> [AZURE.NOTE] Die Anwendungsdateien (einschließlich Webanwendungsdateien, falls vorhanden) für alle öffentlich zugänglichen Endpunkt gefunden werden.

## <a name="publish-application"></a>Veröffentlichen der Anwendung

Führen Sie die folgenden Schritte aus, um eine HDInsight Anwendung zu veröffentlichen:

1. Melden Sie sich auf der [Azure-Veröffentlichungsportal](https://publish.windowsazure.com/).
2. Klicken Sie auf **Lösungsvorlagen** von links, um eine neue Lösungsvorlage erstellen.
3. Geben Sie einen Titel und dann auf **eine neue Lösungsvorlage erstellen**.
3. Klicken Sie auf **Create Entwicklungscenter Konto und Azure-Programm teilnehmen** Ihres Unternehmens registrieren, wenn dies nicht geschehen.  Siehe [Microsoft Developer-Konto erstellen](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).
4. Klicken Sie auf **einige Topologien Schritte definieren**. Eine Projektmappenvorlage ist "Parent" aller seiner Topologien. Sie können mehrere Topologien in einem Angebot-Lösung Vorlage definieren. Ein Angebot zum Staging abgelegt, mit dessen Topologien verschoben. 
4. Geben Sie Topologie, und klicken Sie dann auf das Pluszeichen.
5. Geben Sie eine neue Version, und klicken Sie dann auf das Pluszeichen.
6. Laden Sie die Zip-Datei im [paketanwendung](#package-application)vorbereitet.  
7. Klicken Sie auf **Zertifikat anfordern**. Microsoft-Zertifizierungsteam Dateien überprüfen und bestätigen die Topologie.

## <a name="next-steps"></a>Nächste Schritte

- [Installieren HDInsight Applikationen](hdinsight-apps-install-applications.md): erfahren Sie, wie eine Anwendung HDInsight Cluster installieren.
- [Benutzerdefinierte HDInsight Anwendung installieren](hdinsight-apps-install-custom-applications.md): erfahren Sie, wie eine nicht veröffentlichte Anwendung HDInsight HDInsight bereitstellen.
- [Anpassen von Linux-basierten HDInsight Cluster mit Skriptaktion](hdinsight-hadoop-customize-cluster-linux.md): Informationen zum Skript-Aktion verwenden, um zusätzliche Applikationen zu installieren.
- [Hadoop erstellen Linux-basierten Clustern in Azure Ressourcenmanager Vorlagen HDInsight](hdinsight-hadoop-create-linux-clusters-arm-templates.md): erfahren Sie, wie Vorlagen erstellen HDInsight-Cluster Resource Manager aufrufen.
- [Verwenden Sie leere Rand Nodes in HDInsight](hdinsight-apps-use-edge-node.md): erfahren Sie, wie einen leere Kantenknoten HDInsight Cluster zugreifen und Anwendungstests HDInsight HDInsight Anwendung hosten.

