<properties
   pageTitle="Ausführen und Debuggen eines Cloud-Dienstes auf einem lokalen Computer mit Emulator Express | Microsoft Azure"
   description="Verwendung von Emulator Express ausführen und Debuggen eines Cloud-Dienstes auf einem lokalen Computer"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />


# <a name="using-emulator-express-to-run-and-debug-a-cloud-service-on-a-local-machine"></a>Verwendung von Emulator Express ausführen und Debuggen eines Cloud-Dienstes auf einem lokalen Computer

Emulator Express verwenden, können Sie testen und Debuggen einen Cloud-Dienst ohne Visual Studio als Administrator ausführen. Sie können Ihrem projekteinstellungen Emulator Express oder vollständige Emulation je nach Ihrem Cloud-Dienst festlegen. Weitere Informationen zu vollständigen Emulator finden Sie in der [Azure-Anwendung im Emulator berechnen ausführen](./storage/storage-use-emulator.md). Emulator Express wurde ursprünglich in Azure SDK 2.1, und zum Azure SDK 2.3 Standard-Emulator ist.

## <a name="using-emulator-express-in-the-visual-studio-ide"></a>Verwenden in Visual Studio IDE Emulator Express

Beim Erstellen eines neuen Projekts in Azure SDK 2.3 oder höher Emulator Express bereits ausgewählt. Gehen Sie für Projekte, die mit einer früheren Version des SDK erstellt wurden Emulator Express wählen.

### <a name="to-configure-a-project-to-use-emulator-express"></a>So konfigurieren Sie ein Projekt für Emulator Express

1. Im Kontextmenü für den Azure-Projekt wählen Sie **Eigenschaften**und wählen Sie dann die Registerkarte **Web** .

1. Wählen Sie unter **Lokale Development Server** **Option IIS Express verwenden** . Emulator Express ist nicht kompatibel mit IIS-Webserver.

1. Wählen Sie unter **Emulator**Optionsfeld **Emulator Express verwenden** .

    ![Emulator Express](./media/vs-azure-tools-emulator-express-debug-run/IC673363.gif)

## <a name="launching-emulator-express-at-a-command-prompt"></a>In der Befehlszeile starten Emulator Express

In einer Befehlszeile können Sie die express-Version der Azure-Serveremulator umfasst, csrun.exe, mit der Option /useemulatorexpress starten.

## <a name="limitations"></a>Grenzen

Bevor Sie Emulator Express verwenden, sollten Sie einige Nachteile beachten:

- Emulator Express ist nicht kompatibel mit IIS-Webserver.

- Cloud-Dienst kann mehrere Rollen enthalten, aber jede Rolle ist eine Instanz auf.

- Anschlussnummern unter 1000 kann nicht zugegriffen werden. Wenn Sie Authentifizierungsanbieter, der normalerweise einen Port unter 1000 verwendet verwenden, müssen Sie diesen Wert auf eine Port-Nummer ändern, die über 1000.

- Einschränkungen der Azure-Serveremulator zuweisen gelten auch für Emulator Express. Beispielsweise kann nicht mehr als 50 Instanzen pro Bereitstellung haben. Finden Sie unter [Ausführen eine Azure-Anwendung im Serveremulator](http://go.microsoft.com/fwlink/p/?LinkId=623050)

## <a name="next-steps"></a>Nächste Schritte

[Debuggen von Clouddiensten](https://msdn.microsoft.com/library/azure/ee405479.aspx)
