<properties 
   pageTitle="Verwalten von Azure Ressourcen mit Cloud Explorer | Microsoft Azure"
   description="Informationen Sie zum Cloud Explorer durchsuchen und Azure Ressourcen in Visual Studio verwenden."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="managing-azure-resources-with-cloud-explorer"></a>Verwalten von Azure Ressourcen mit Cloud Explorer

##<a name="overview"></a>Übersicht

Cloud Explorer gestaltet und schneller durchsuchen und verwalten Sie Ihre Azure Ressourcen innerhalb der Visual Studio-IDE. Sie können z. B. eine Webanwendung in [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oder in einem Browser öffnen oder einen Debugger, oder zeigen Sie die Eigenschaften eines Containers Blob und der Blob-Container-Editor zu öffnen.

Cloud Explorer wird auf dem Stapel Azure-Ressourcen-Manager wie [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)erstellt. Ressourcen wie Azure Ressourcengruppen und Azure Services wie Logik und API-apps versteht und [Rollenbasierte Zugriffskontrolle](./active-directory/role-based-access-control-configure.md) (RBAC) unterstützt. Um Azure Ressourcen anzuzeigen, die hinzugefügt oder geändert, wählen Sie die Cloud Explorer-Symbolleiste auf die Schaltfläche **Aktualisieren aus**

Cloud Explorer wird als Teil der Visual Studio-Tools für Azure SDK 2.7 installiert. 

## <a name="prerequisites"></a>Erforderliche Komponenten

- Visual Studio 2015 RTM.

- Der Visual Studio-Tools für Azure SDK. 
- Sie müssen ein Azure-Konto und ein Azure-Ressourcen in Cloud Explorer anzeigen protokolliert werden. Wenn Sie eine haben, können Sie ein Konto in wenigen Minuten erstellen. Wenn Sie ein MSDN-Abonnement verfügen, finden Sie unter [Azure nutzen für MSDN-Abonnenten](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Andernfalls finden Sie [ein kostenloses Testabo erstellen](https://azure.microsoft.com/pricing/free-trial/).

- Cloud-Explorer nicht sichtbar, können Sie es **Anzeigen**, **Andere Windows** **Cloud Explorer** in der Menüleiste anzeigen.

## <a name="manage-azure-accounts-and-subscriptions"></a>Azure Konten und Abonnements verwalten

Finden Sie Ihre Azure Cloud Explorer-Ressourcen müssen Sie mit einem oder mehreren aktiven Abonnements ein Azure-Konto anmelden. Haben mehrere Azure-Konto können Cloud Explorer hinzufügen und wählen Sie dann die Abonnements in der Ressourcenansicht Cloud Explorer einschließen möchten.

Wenn Visual Studio noch nicht die erforderlichen Konten hinzugefügt Azure vor nicht verwendet noch, werden Sie dazu aufgefordert.

## <a name="to-add-azure-accounts-to-cloud-explorer"></a>Hinzufügen von Azure Cloud Explorer Konten

1. Wählen Sie das Symbol in der Cloud Explorer-Symbolleiste.

1. Wählen Sie den Link **Konto hinzufügen** . Melden Sie sich bei Azure-Konto, dessen Ressourcen Sie durchsuchen möchten. Das soeben hinzugefügte Konto sollte in der Datumsauswahl Dropdown-Liste ausgewählt. Abonnements für dieses Konto werden unter den Kontoeintrag angezeigt.

    ![Hinzufügen von Azure-Abonnements](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819514.png)

    ![Auswählen von Azure-Abonnements](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819515.png)

1. Aktivieren Sie die Kontrollkästchen für die Kontoabonnements zu suchen, und klicken Sie dann auf **Übernehmen** .

    Azure-Ressourcen für die ausgewählten Abonnements im Cloud Explorer angezeigt.

## <a name="to-remove-an-azure-account"></a>Ein Azure-Konto entfernen

1. Wählen Sie **Datei**, **Einstellungen** in der Menüleiste.

1. Wählen Sie im Abschnitt **Alle Konten** im Dialogfeld **Konto** Befehl **Entfernen** neben dem Konto, das Sie entfernen möchten. Beachten Sie, dass dieser Befehl nur das Konto aus Visual Studio – entfernt beeinflussen nicht die Azure selbst.

## <a name="view-resource-types-or-groups"></a>Ressourcentypen anzeigen oder Gruppen

Zum Anzeigen der Azure Ressourcen können Sie **Ressourcentypen** oder **Ressourcengruppen** anzeigen.

![Dropdownliste für Ressource anzeigen](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819516.png)

- **Ressourcentypen** zeigt, ist auch die allgemeine Ansicht der [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)Azure Ressourcen nach Typ, wie webapps, Speicherkonten und virtuellen Maschinen kategorisiert. Dies ist ähnlich wie Azure Ressourcen im Server-Explorer angezeigt.

- Ressourcenansicht Gruppen kategorisiert Azure Ressourcen nach der Ressourcengruppe Azure, denen, der Sie zugeordnet sind.

 
    Eine Ressourcengruppe ist ein Bündel von Azure-Ressourcen in der Regel von einer bestimmten Anwendung verwendet. Erfahren Sie mehr über Azure Ressourcengruppen finden Sie in [Azure Ressourcen-Manager](./resource-group-overview.md).

## <a name="view-and-navigate-resources"></a>Anzeigen von und Navigieren von Ressourcen

Zum Navigieren zu Azure Ressourcen und Informationen in Cloud Explorer, erweitern Sie des Elements Typ- oder zugeordnete Ressource und dann die Ressource. Wenn Sie eine Ressource auswählen, werden Informationen auf zwei Registerkarten unten Cloud Explorer angezeigt.

![Wählen Sie eine Ressourcenansicht](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819517.png)

- **Die Registerkarte** zeigt können Sie Aktionen für die ausgewählte Ressource in Cloud Explorer. Aktionen können auch im Kontextmenü der Ressource angezeigt werden.

- Die Registerkarte **Eigenschaften** zeigt die Eigenschaften der Ressource, wie seine, Gebietsschema und Gruppe zugeordnet ist.

Jede Ressource hat die Aktion **im Portal**. Wenn Sie diese Aktion auswählen, zeigt Cloud Explorer die ausgewählte Ressource in [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040). Diese Funktion ist besonders nützlich für die Navigation zu tief verschachtelt Ressourcen.

Zusätzliche Aktionen und Eigenschaftenwerte möglicherweise auch auf Azure Ressource angezeigt. Beispielsweise können webapps und Logik apps Aktionen **im Browser öffnen** und **Debugger Anhängen** zusätzlich **im Portal**. Aktionen Editoren geöffnet werden Wenn ein BLOB-Speicher-Konto, eine Warteschlange oder eine Tabelle auswählen. Azure apps haben Eigenschaften der **URL** und **Status** Speicherressourcen Schlüssel und Verbindungszeichenfolge-Eigenschaften.

## <a name="search-resources"></a>Ressourcen

Um Abonnements Azure-Konto mit einem bestimmten Namen zu suchen, geben Sie im Feld Suchen in Cloud Explorer.

![Suchen von Ressourcen in Cloud Explorer](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC820394.png)

Zeichen in das Suchfeld eingeben, werden nur Ressourcen, die die Zeichen in der Ressourcenstruktur angezeigt.

