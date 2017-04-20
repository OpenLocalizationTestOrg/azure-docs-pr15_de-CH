<properties
   pageTitle="Ein Azure Cloud Service-Projekt mit Visual Studio konfigurieren | Microsoft Azure"
   description="Erfahren Sie, wie ein Azure Cloud Service-Projekt in Visual Studio je nach Bedarf für das Projekt konfigurieren."
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

# <a name="configure-an-azure-cloud-service-project-with-visual-studio"></a>Konfigurieren Sie ein Azure Cloud Service-Projekt mit Visual Studio

Ein Azure-Cloud-Dienstprojekt konfigurieren Sie je nach Bedarf für das Projekt. Legen Sie Eigenschaften für das Projekt für die folgenden Kategorien:

- **Azure Cloud Service veröffentlichen**

  Sie können festlegen, dass eine Eigenschaft zu ein vorhandenen Cloud-Dienst in Azure bereitgestellt nicht versehentlich gelöscht werden.

- **Ausführen oder Debuggen eines Cloud-Dienstes auf dem lokalen computer**

  Sie können eine Dienstkonfiguration und angeben, ob den Azure-Speicheremulator starten auswählen.

- **Überprüfen einer Cloud Service-Paket bei der Erstellung**

  Sie können Warnungen als Fehler behandelt werden, damit Sie sicherstellen können, dass das Cloud Service-Paket ohne Probleme bereitstellen. Dadurch erhalten die Wartezeit bereitstellen und später feststellen, dass ein Fehler aufgetreten.

Die folgende Abbildung zeigt eine Konfiguration beim Ausführen oder lokal dem Cloud-Dienst Debuggen auswählen. Lassen sich keines der Projekteigenschaften Sie in diesem Fenster wie in der Abbildung dargestellt.

![Microsoft Azure-Projekt konfigurieren](./media/vs-azure-tools-configuring-an-azure-project/IC713462.png)

## <a name="to-configure-an-azure-cloud-service-project"></a>So konfigurieren Sie eine Azure-Cloud-Dienstprojekt

1. Konfigurieren Sie ein Cloud-Dienstprojekt im **Projektmappen-Explorer**öffnen Sie das Kontextmenü für das Projekt für den Cloud-Dienst und dann auf **Eigenschaften**.

  Eine Seite mit dem Namen Projekt für den Cloud-Dienst wird im Visual Studio-Editor.

1. Wählen Sie die Registerkarte **Entwicklung** .

1. Wählen Sie **True**, um sicherzustellen, dass Sie nicht versehentlich eine vorhandene Bereitstellung in Azure Aufforderung vor dem Löschen einer vorhandenen bereitstellungsliste löschen.

1. Um die Dienstkonfiguration soll beim Ausführen oder, die Cloud-Dienst in der Liste **Konfiguration Debuggen** auszuwählen die Dienstkonfiguration.

  >[AZURE.NOTE] Möchten Sie eine Konfiguration verwenden, finden Sie unter Erstellen wie: Verwalten von Konfigurationen und Profile. Wenn Sie eine Konfiguration für eine Rolle ändern möchten, finden Sie unter [Konfigurieren der Rollen für einen Azure-Cloud-Dienst mit Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).

1. Wählen Sie Azure Speicheremulator starten beim Ausführen oder, die Cloud-Dienst in Debuggen **Azure starten Speicheremulator** **True**.

1. Um sicherzustellen, dass Sie veröffentlichen können, liegen Validierungsfehler Paket Warnungen **als Fehler behandeln**, wählen Sie **True**.

1. Um sicherzustellen, dass die Web-Rolle denselben Port verwendet in IIS Express im **Web Projekt Anschlüsse verwenden**, startet wählen Sie **True**. Um einen bestimmten Port für ein bestimmtes Webprojekt verwenden, öffnen Sie das Kontextmenü für das Webprojekt, wählen Sie die Registerkarte **Eigenschaften** wählen Sie die Registerkarte **Web** , und ändern Sie die Portnummer in der **Projekt-Url** -Einstellung im Abschnitt **IIS Express** . Geben Sie beispielsweise `http://localhost:14020` Projekt-URL.

1. Um Eigenschaften der Cloud-Dienstprojekt vorgenommenen Änderungen zu speichern, wählen Sie auf der Symbolleiste auf die Schaltfläche **Speichern** .

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Konfigurieren von Azure Cloud Service-Projekte in Visual Studio finden Sie unter [Konfigurieren der Azure-Projekt mit mehreren Dienstkonfigurationen](vs-azure-tools-multiple-services-project-configurations.md).
