<properties
   pageTitle="Erstellen einen Analysis Services-Server in Azure | Microsoft Azure"
   description="Informationen Sie zum Erstellen einer Analysis Services-Serverinstanz in Azure."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="create-an-analysis-services-server"></a>Erstellen eines Analysis Services-Servers
Dieser Artikel führt Sie durch das Erstellen einer neuen Ressource der Analysis Services-Server in Azure-Abonnement.

## <a name="before-you-begin"></a>Bevor Sie beginnen
Zunächst müssen wie folgt vor:

- **Azure-Abonnement**: Besuchen Sie [Azure-Testversion](https://azure.microsoft.com/offers/ms-azr-0044p/) ein Konto erstellen.
- **Ressourcengruppe**: Verwenden einer Ressourcengruppe bereits oder [eine neue erstellen](../azure-resource-manager/resource-group-overview.md).

> [AZURE.NOTE] Erstellen eines Analysis Services-Server möglicherweise eine neue fakturierbaren Service. Informationen finden Sie unter Analysis Services Preise.

## <a name="create-an-analysis-services-server"></a>Erstellen eines Analysis Services-Servers

1. Mit der [Azure-Portal](https://portal.azure.com)anmelden.

2. Klicken Sie auf **+ neu** > **Intelligence + Analytics** > **Analysis Services**.

3. In **Analysis Services** -Blade die erforderlichen Felder, und drücken Sie **Erstellen**.

    ![Server erstellen](./media/analysis-services-create-server/aas-create-server-blade.png)

    - **Server-Name**: Geben Sie einen eindeutigen Namen auf dem Server verwendet.

    - **Abonnement**: Abonnement dieser Server Rechnungen zu.

    - **Ressourcengruppe**: sind Container um eine Sammlung von Azure Ressourcen verwalten. Weitere anzeigen Sie [Ressourcengruppen](../resource-group-overview.md)

    - **Ort**: Diese Azure Datacenter Speicherort auf dem Server befinden. Wählen Sie nächste größte Benutzerbasis an.

    - **Preisstufe**: einen Tarif wählen. Tabelle modelliert bis zu 100 GB werden unterstützt. Der Tarif können Sie später jederzeit ändern.

4. Klicken Sie auf **Erstellen**.

Erstellen Sie dauert weniger als eine Minute; oft nur wenige Sekunden. Navigieren Sie **Hinzufügen zum Portal**aktiviert, zu Ihr Portal auf den neuen Server. Oder navigieren Sie zu **Weitere Services** > **Analysis Services** auf der Server bereit. Wenn sie die Liste aktualisieren erscheint.

 ![Dashboard](./media/analysis-services-create-server/aas-create-server-dashboard.png)


## <a name="next-steps"></a>Nächste Schritte
Nach dem Erstellen des Servers können Sie das [Bereitstellen ein Modells](analysis-services-deploy.md) mit SSDT oder SSMS.

Wenn ein Modell zum Server bereitgestellten lokalen Datenquellen verbunden ist, müssen Sie einen [lokalen Daten-Gateway](analysis-services-gateway.md) auf einem Computer im Netzwerk installieren.
