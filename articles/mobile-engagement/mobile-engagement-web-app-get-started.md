<properties
    pageTitle="Erste Schritte mit Azure Mobile Engagement für Web Apps | Microsoft Azure"
    description="Enthält Informationen zum Verwenden von Azure Mobile Engagement mit Analysen und Push Notifications für Web Apps."
    services="mobile-engagement"
    documentationCenter="Mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="js"
    ms.topic="hero-article"
    ms.date="06/01/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-web-apps"></a>Erste Schritte mit Azure Mobile Engagement für Web Apps

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In diesem Thema wird veranschaulicht, wie mit Azure Mobile Engagement die Web App Verwendung verstehen.

In diesem Lernprogramm ist Folgendes erforderlich:

+ Visual Studio 2015 oder Editor Ihrer Wahl
+ [Web SDK](http://aka.ms/P7b453) 

Dieses Web-SDK ist in der Vorschau nur Analytics derzeit unterstützt und sendende Browser oder in app Pushbenachrichtigungen noch unterstützt. 

> [AZURE.NOTE] Um dieses Lernprogramm benötigen Sie ein aktives Azure-Konto. Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-web-app-get-started).

##<a name="setup-mobile-engagement-for-your-web-app"></a>Mobile Engagement für Ihre Web-app einrichten

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Mobile Engagement Backend Ihrer app herstellen

In diesem Lernprogramm wird eine "einfache Integration" Mindestsatz zum Sammeln von Daten erforderlich ist.

Wir erstellen eine einfachen Web-Anwendung mit Visual Studio Integration zu veranschaulichen, wenn Sie mit einer Webanwendung auch außerhalb von Visual Studio erstellt die Schritte ausführen können. 

###<a name="create-a-new-web-app"></a>Erstellen einer neuen Web-Applikation

Die folgenden Schritte gehen die Verwendung von Visual Studio 2015 Schritte in früheren Versionen von Visual Studio ähnlich sind. 

1. Starten Sie Visual Studio, und wählen Sie im **Startbildschirm,** **Neues Projekt**.

2. Wählen Sie im Popupfenster **Web** -> **ASP.NET-Webanwendung**. App **Name**, **Speicherort** und **Projektmappennamen**füllen Sie, und klicken Sie dann auf **OK**.

3. Im Dialogfenster **Wählen Sie eine Vorlage** **leere** unter **ASP.Net 4.5 Vorlagen** wählen Sie und auf **OK**. 

Sie haben nun ein neues leeres Projekt Web App erstellt, in dem wir Azure Mobile Engagement Web SDK integrieren.

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Mobile Engagement Backend Ihrer app herstellen

1. Erstellen Sie einen neuen Ordner namens **Javascript** in der Projektmappe und fügen Sie Web SDK JS-Datei **Azure engagement.js** . 

2. Fügen Sie eine neue Datei namens **main.js** in diesem Ordner Javascript durch den folgenden Code hinzu. Stellen Sie sicher, dass die Verbindungszeichenfolge. Diese `azureEngagement` -Objekt auf Web SDK Methoden zugreifen. 

        var azureEngagement = {
            debug: true,
            connectionString: 'xxxxx'
        };

    ![Visual Studio Js-Dateien][1]

##<a name="enable-real-time-monitoring"></a>Echtzeit-Überwachung aktivieren

Starten Sie Daten senden und sicherstellen, dass der Benutzer aktiv sind, müssen Sie mindestens eine Aktivität Mobile Engagement Backend senden. Eine Aktivität im Kontext einer Webanwendung ist eine Webseite. 

1. Eine neue Seite namens **home.html** in der Projektmappe und als die Startseite Ihrer Anwendung festlegen. 
2. Enthalten Sie zwei Javascripts, die wir weiter oben auf dieser Seite hinzugefügt, indem Sie Folgendes in das Body-Tag hinzufügen. 

        <script type="text/javascript" src="javascript/main.js"></script>
        <script type="text/javascript" src="javascript/azure-engagement.js"></script>

3. Aktualisiert das Body-Tag zum Aufrufen des EngagementAgent `startActivity` Methode
        
        <body onload="engagement.agent.startActivity('Home')">

4. Hier wird die **home.html** aussehen
        
        <html>
        <head>
            ...
        </head>
        <body onload="engagement.agent.startActivity('Home')">
            <script type="text/javascript" src="javascript/main.js"></script>
            <script type="text/javascript" src="javascript/azure-engagement.js"></script>
        </body>
        </html>

##<a name="connect-app-with-real-time-monitoring"></a>Verbinden von app durch Echtzeit-Überwachung

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

![][2]

##<a name="extend-analytics"></a>Analytics erweitern

Hier sind die Methoden mit Web-SDK, die für Analysen verwendet werden können:

1. Aktivitäten--Webseiten:

        engagement.agent.startActivity(name);
        engagement.agent.endActivity();

2. Ereignisse
        
        engagement.agent.sendEvent(name, extras);

3. Fehler

        engagement.agent.sendError(name, extras);

4. Aufträge

        engagement.agent.startJob(name);
        engagement.agent.endJob(name);

<!-- Images. -->
[1]: ./media/mobile-engagement-web-app-get-started/visual-studio-solution-js.png
[2]: ./media/mobile-engagement-web-app-get-started/session.png

