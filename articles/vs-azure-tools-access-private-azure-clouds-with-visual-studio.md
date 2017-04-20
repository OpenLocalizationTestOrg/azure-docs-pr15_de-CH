<properties 
   pageTitle="Zugriff auf private Azure Clouds mit Visual Studio | Microsoft Azure"
   description="Erfahren Sie, wie private Zugriff auf Cloudressourcen mithilfe von Visual Studio."
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

# <a name="accessing-private-azure-clouds-with-visual-studio"></a>Zugriff auf private Azure Clouds mit Visual Studio

##<a name="overview"></a>Übersicht

Visual Studio unterstützt standardmäßig öffentliche Azure Cloud REST-Endpunkte. Dies kann ein Problem sein, verwenden Sie Visual Studio mit einer privaten Azure Cloud. Zertifikate können Sie Visual Studio Zugriff auf private Azure Cloud REST-Endpunkte konfigurieren. Sie erhalten diese Zertifikate durch Ihre Azure veröffentlichen Einstellungsdatei.

## <a name="to-access-a-private-azure-cloud-in-visual-studio"></a>Auf private Azure Cloud in Visual Studio

1. Im [klassischen Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885) für die private Cloud veröffentlichen Einstellungsdatei herunterladen oder Systemadministrator für eine Datei veröffentlichen. Auf öffentlichen Version von Azure ist der Link zum download dieses [https://manage.windowsazure.com/publishsettings/](https://manage.windowsazure.com/publishsettings/). (Die heruntergeladene Datei sollte eine PUBLISHSETTINGS-Erweiterung haben.)

1. Wählen Sie den **Azure** -Knoten im **Server-Explorer** in Visual Studio und wählen Sie im Kontextmenü den Befehl **Abonnements verwalten** .

    ![Verwalten von Abonnements-Befehl](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790778.png)

1. Im Dialogfeld **Microsoft Azure-Abonnements verwalten** wählen Sie die Registerkarte **Zertifikate** , und wählen Sie dann die Schaltfläche **Importieren** .

    ![Azure Zertifikate importieren](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790779.png)

1. Klicken Sie im Dialogfeld **Import Microsoft Azure-Abonnements** suchen Sie den Ordner, veröffentlichen Einstellungsdatei gespeichert und die Datei dann **Importieren** wählen. Zertifikate in der Einstellungsdatei veröffentlichen wird in Visual Studio importiert. Jetzt werden mit der privaten Cloudressourcen interagieren.

    ![Importieren von Einstellungen veröffentlichen](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790780.png)

## <a name="next-steps"></a>Nächste Schritte

[Veröffentlichen in Azure Cloud-Dienst von Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx)

[How to: Download und Import Settings und Abonnementinformationen veröffentlichen](https://msdn.microsoft.com/library/dn385850(v=nav.70).aspx)

