<properties 
   pageTitle="Mobile Dienste mit verbundenen Dienste in Visual Studio hinzufügen | Microsoft Azure"
   description="Fügen Sie Mobile Dienste hinzu, indem Sie über das Dialogfeld Visual Studio verbunden Dienste hinzufügen"
   services="visual-studio-online"
   documentationCenter="na"
   authors="mlhoop"
   manager="douge"
   editor="" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="mobile"
   ms.date="12/16/2015"
   ms.author="mlearned" />

# <a name="adding-mobile-services-by-using-visual-studio-connected-services"></a>Mobile Dienste mit Visual Studio verbunden Services hinzufügen

Visual Studio 2015 können Sie Azure Mobile Dienste mit dem Dialogfeld **Verbunden Dienst hinzufügen** . Sie können alle C#-Clientanwendung, JavaScript-app oder plattformübergreifende Cordova Anwendung. Wenn Sie verbunden sind, können Sie erstellen und auf Daten zugreifen, erstellen Sie benutzerdefinierte APIs und geplante Aufträge oder Unterstützung für Pushbenachrichtigungen.  Alle entsprechenden Verweise und Verbindungscode hinzugefügt Dienste Betrieb. Außerdem profitieren Sie von Unterstützung für die Authentifizierung mit einer Vielzahl von gängigen Identität Schemas wie Azure AD Microsoft Accounts, Facebook und Twitter.

## <a name="supported-project-types"></a>Unterstützte Projekttypen

>[AZURE.NOTE] Visual Studio 2015 ist eine universelle Windows (Windows 10) Projekte verbundenen Dienste hinzufügen Dialogfeld Azure Mobile Dienste hinzufügen nicht unterstützt. Installieren die entsprechenden Pakete mit NuGet Paket-Manager für das Projekt können Sie Azure Mobile Services hinzufügen.

Das Dialogfeld Verbindung können Azure Mobile Dienste in folgenden Projekttypen herstellen.

- .NET Windows 8.1 Store, Telefon und universelle App-Projekte

- JavaScript Windows 8.1 Store, Telefon und universelle App-Projekte

- Projekte mit Visual Studio-Tools für Apache Cordova


## <a name="connect-to-azure-mobile-services-using-the-add-connected-services-dialog"></a>Azure Mobile Dienste mit dem Dialogfeld verbunden Dienste hinzufügen

1. Stellen Sie sicher, dass Sie ein Azure-Konto verfügen. Wenn Sie ein Azure-Konto haben, können Sie eine [kostenlose Testversion](http://go.microsoft.com/fwlink/?LinkId=518146)anmelden.

1. Öffnen Sie das Dialogfeld **Verbundenen Dienste hinzufügen** .
 - Öffnen Sie für .NET apps das Projekt in Visual Studio das Kontextmenü für den Knoten **Verweise** im Projektmappen-Explorer und wählen Sie die **Verbundenen Dienst hinzufügen**
 
        ![Connecting to Azure Mobile Service](./media/vs-azure-tools-connected-services-add-mobile-services/IC797635.png)

 - Apache Cordova app Projekte öffnen Sie das Projekt in Visual Studio **Verbunden Dienst hinzufügen**wählen Sie und öffnen Sie das Kontextmenü für den Knoten im Projektmappen-Explorer.

1. Klicken Sie im Dialogfeld **Verbindung Dienst hinzufügen** **Azure Mobile**Dienste und wählen Sie dann die Schaltfläche **Konfigurieren** . Sie können aufgefordert Azure anmelden, wenn Sie dies nicht bereits getan haben.

    ![Hinzufügen von Azure Mobile Service](./media/vs-azure-tools-connected-services-add-mobile-services/IC797636.png)

1. Wählen Sie im Dialogfeld **Azure Mobile Dienste** einen vorhandenen mobilen Dienst ggf. aus Möchten Sie einen neuen Azure mobilen Service erstellen, gehen Sie zu tun. Fahren Sie andernfalls mit dem nächsten Schritt.

    So erstellen Sie ein neue mobile Service-Konto
    1. Wählen Sie **Create Service **-Link am unteren Rand des Dialogfelds.
        ![Neue mobile verbundenen Dienst hinzufügen](./media/vs-azure-tools-connected-services-add-mobile-services/IC797637.png)




    2. Im Dialogfeld **Mobile Service erstellen** können Sie ein JavaScript Back-End-mobile-Dienst oder ein .NET Backend-mobile Service **Runtime** Dropdown-Liste auswählen. 
  
        ![Erstellen eines mobilen Diensts](./media/vs-azure-tools-connected-services-add-mobile-services/IC797638.png)

        Ein JavaScript Back-End-Dienst ist einfacher und leistungsfähiger. Wenn ein JavaScript Backend-mobilen-Dienst erstellt wird, Serverskripts bearbeiten Sie Server-Explorer oder in der Azure-Verwaltungsportal serverseitige JavaScript-Code in der Cloud gespeichert 

        Ein .NET Backend-mobiler-Service bietet die Leistungsstärke und Flexibilität von Web-API und Entity Framework. Bei der Erstellung eines .NET Backend-mobilen Service wird ein Projekt erstellt und der Projektmappe hinzugefügt. 

    1. Die **Region** des mobilen Diensts werden soll, und geben Sie einen Benutzernamen und ein Kennwort für den Server.
 
    1. Nachdem Sie alle erforderlichen Informationen eingegeben haben, wählen Sie **Create** mobile Service erstellen.
    2. Der neue mobile Service erscheint in der Dienstliste im Dialogfeld **Azure Mobile Services** . Wählen Sie neuen mobilen Dienst aus der Liste aus, und wählen Sie dann " **Hinzufügen** ", den Dienst zu Ihrem Projekt hinzufügen.
    

1. Überprüfen Sie die erste gestartete Seite und herausfinden Sie, wie das Projekt geändert wurde. Getting Started-Seite wird im Browser angezeigt, verbundenen Dienst hinzufügen. Überprüfen der vorgeschlagenen Schritte und Codebeispiele oder wechseln Sie zur Seite geschehen, welche Verweise zum Projekt hinzugefügt wurden, und wie die Konfigurationsdateien Dateien geändert wurden.

1. Starten Sie die Codebeispiele als Richtlinie verwenden, Code schreiben, um Ihren mobilen Dienst zugreifen!

## <a name="how-your-project-is-modified"></a>Wie Projekt geändert

Wie Visual Studio das Projekt ändert, hängt von den Projekttyp ab. C# Clientanwendungen finden Sie unter [wo – C#-Projekte](http://go.microsoft.com/fwlink/p/?LinkId=513119). JavaScript-Clientanwendungen finden Sie unter [wo – JavaScript-Projekte](http://go.microsoft.com/fwlink/p/?LinkId=513120). Cordova-apps finden Sie unter [wo – Cordova Projekte](http://go.microsoft.com/fwlink/p/?LinkId=513116).


##<a name="next-steps"></a>Nächste Schritte

Fragen stellen und Hilfe: 

 - [MSDN-Forum: Azure Mobile Dienste](https://social.msdn.microsoft.com/forums/azure/home?forum=azuremobile)

 - [Azure Mobile Dienste Microsoft Azure Team Blog](https://azure.microsoft.com/blog/topics/mobile/)

 - [Azure Mobile Services auf azure.microsoft.com](https://azure.microsoft.com/services/mobile-services/)

 - [Azure Mobile Services-Dokumentation unter azure.microsoft.com](https://azure.microsoft.com/documentation/services/mobile-services/)



