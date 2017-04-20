<properties
   pageTitle="Ein Azure-Projekt mit Visual Studio erstellen | Microsoft Azure"
   description="Ein Azure-Projekt erstellen mit Visual Studio"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="creating-an-azure-project-with-visual-studio"></a>Ein Azure-Projekt erstellen mit Visual Studio

Azure Tools für Visual Studio bieten eine Vorlage, die einen Azure-Clouddienst erstellen kann. Den Tools können Sie konfigurieren, Debuggen und Bereitstellen von Cloud-Dienst in Azure.

Eine Azure Cloud Service-Lösung enthält die folgenden Projekte:

- **Azure-Projekt**

    Azure-Projekt hat zu Rolle Projekte in der Projektmappe. Darüber hinaus die Dienstdefinition und Service-Konfigurationsdateien. Service-Definitionsdatei definiert die Runtime Einstellungen für Anwendung Rollen erforderlich sind, Endpunkte und Größe des virtuellen Computers. Wie viele Instanzen einer Rolle ausgeführt werden und die Werte für eine Rolle definierten Einstellungen konfiguriert Dienstkonfigurationsdatei. Weitere Informationen dazu finden Sie unter [wie: Konfigurieren von Rollen für eine Azure-Cloud-Dienst mit Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).

- **Webprojekt-Rolle**

    Eine Worker-Rolle führt im Hintergrund. Eine Worker-Rolle kann Speicher-Services und andere Internet-basierte Dienste kommunizieren. Eine Worker-Rolle können eine beliebige Anzahl von HTTP, HTTPS oder TCP-Endpunkte.

    - **ASP.NET Web-Rolle**für eine ASP.NET Anwendung mit einem Web-front-end
    - **Webrolle ASP.NET MVC5**
    - **Webrolle ASP.NET MVC4**
    - **Webrolle ASP.NET MVC3**
    - **WCF Service Webrolle**für einen WCF-Dienst erstellen
    - **Silverlight Geschäftsrolle Anwendung Web** (erfordert Visual Studio 2012)

- **Cache-Worker-Rolle**

    Eine Rolle, die einen dedizierten Cache für die Anwendung bereitstellt.

- **Worker-Rolle mit Service Bus-Warteschlange**

    Ein Service Bus-Warteschlange, die Message Queuing-Funktionalität zu den Arbeitsprozess bereitstellt. Weitere Informationen finden Sie unter [Service Bus-Warteschlangen verwenden](http://go.microsoft.com/fwlink/?LinkId=260560).

## <a name="to-create-an-azure-cloud-service-project-in-visual-studio"></a>Ein Azure Cloud Service-Projekt in Visual Studio erstellen

1. Starten Sie Microsoft Visual Studio als Administrator.

1. Wählen Sie in der Menüleiste **Datei**, **neu**, **Projekt**aus.

1. Klicken Sie im Bereich **Projekttypen** wählen Sie **Cloud** Visual C# oder Visual Basic-Projekt vor.

1. Wählen Sie im Bereich **Vorlagen** **Azure Cloud Service**.

1. Geben Sie an, welche Version von.NET Framework entwickeln von Projekten verwenden möchten.

1. Geben Sie einen Namen und Speicherort für das Projekt und einen Namen für die Projektmappe. Wählen Sie **OK** .

1. Klicken Sie im Dialogfeld **Neue Azure-Projekt** wählen Sie die Rollen, die Sie hinzufügen möchten, und die rechte Pfeiltaste auf der Projektmappe hinzufügen. Sie können so viele Rollen wie notwendig.

1. Eine Rolle, die dem Projekt hinzugefügt haben, zeigen Sie die Rolle im Dialogfeld **Neue Azure-Projekt** , und klicken Sie **Umbenennen** -Symbol rechts neben der Rolle. Sie können auch eine Rolle innerhalb der Projektmappe umbenennen, nachdem er hinzugefügt wurde.
