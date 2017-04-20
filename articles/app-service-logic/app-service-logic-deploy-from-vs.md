<properties 
    pageTitle="Entwickeln von Apps Logik in Visual Studio | Microsoft Azure" 
    description="Erstellen Sie ein Projekt in Visual Studio erstellen und Bereitstellen Ihrer Anwendung Logik." 
    authors="jeffhollan" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/> 
    
# <a name="build-and-deploy-logic-apps-in-visual-studio"></a>Erstellen und Bereitstellen von Logik-Apps in Visual Studio

Obwohl [Azure-Portal](https://portal.azure.com/) Ihnen eine hervorragende Möglichkeit gibt zu Logik apps verwalten, sollten Sie auch entwerfen und Bereitstellen Ihrer Anwendung Logik aus Visual Studio stattdessen.  Logik Apps kommt eine umfangreiche Visual Studio-Tools der Logik-app mit dem Designer erstellen können, konfigurieren Vorlagen Bereitstellung und Automatisierung und in jeder Umgebung bereitstellen.  

## <a name="installation-steps"></a>Installationsschritte

Nachfolgend sind die Schritte zum Installieren und konfigurieren den Visual Studio-Tools für Logik-Apps.

### <a name="prerequisites"></a>Erforderliche Komponenten

- [Visual Studio 2015](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
- [Aktuelle Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 oder höher)
- [Azure PowerShell] (https://github.com/Azure/azure-powershell#installation)
- Zugriff auf die Verwendung des eingebetteten Designers

### <a name="install-visual-studio-tools-for-logic-apps"></a>Installieren Sie Visual Studio Tools für Logik-Apps

Sobald Sie die erforderlichen Komponenten installiert haben, 

1. Öffnen Sie Visual Studio 2015 zum Menü **Extras** und wählen Sie **Extensions und Updates**
1. Wählen Sie die Kategorie **Online** , online suchen
1. Suchen von **Logik Apps** **Azure Logik Apps-Tools für Visual Studio** anzeigen
1. Klicken Sie auf **Download** herunterladen und Installieren der Erweiterung
1. Starten Sie Visual Studio nach der installation

> [AZURE.NOTE] Sie können auch die Erweiterung direkt [hier](https://visualstudiogallery.msdn.microsoft.com/e25ad307-46cf-412e-8ba5-5b555d53d2d9) herunterladen

Nach der Installation werden Ressourcengruppe Azure-Projekt mit der Logik App-Designer verwenden.

## <a name="create-a-project"></a>Erstellen Sie ein Projekt

1. Das Menü **Datei** und wählen Sie **neu** >  **Projekt** (oder fahren Sie mit **Hinzufügen** und wählen Sie dann **Neues Projekt** einer vorhandenen Projektmappe hinzufügen):  ![Menü Datei](./media/app-service-logic-deploy-from-vs/filemenu.png)

1. Klicken Sie im Dialogfeld Suchen Sie **Cloud**, und wählen Sie **Azure Ressourcengruppe aus**. Geben Sie einen **Namen** , und klicken Sie dann auf **OK**.
    ![Neues Projekt hinzufügen](./media/app-service-logic-deploy-from-vs/addnewproject.png)

1. Wählen Sie die Vorlage **Logik app** . Dadurch entsteht eine Bereitstellungsvorlage leere Logik app zu.
    ![Azure Vorlage auswählen](./media/app-service-logic-deploy-from-vs/selectazuretemplate.png)

1. Nachdem Sie die **Vorlage**ausgewählt haben, drücken Sie **OK**.

    Jetzt wird das Logik-app-Projekt der Projektmappe hinzugefügt. Die Bereitstellungsdatei im Projektmappen-Explorer sollte angezeigt werden:  

    ![Bereitstellung](./media/app-service-logic-deploy-from-vs/deployment.png)

## <a name="using-the-logic-app-designer"></a>Mit dem Designer App Logik

Haben Sie ein Projekt Azure-Ressourcengruppe, die eine Anwendung Logik enthält, können Sie den Designer in Visual Studio unterstützen Sie beim Erstellen des Workflows öffnen.  Der Designer erfordert eine Internet-Verbindung Anschlüsse für verfügbar und Abfragen (z. B. wenn Dynamics CRM Online Connector, Designer fragt Ihr CRM-Instanz zum Auflisten der verfügbaren benutzerdefinierten und Standard-Eigenschaften).

1. Mit der rechten Maustaste auf den `<template>.json` Datei und wählen Sie **Öffnen mit Logik App Designer** (oder `Ctrl+L`)
1. Wählen Sie das Abonnement Ressourcengruppe und Speicherort für die Bereitstellungsvorlage
    - Es ist zu beachten, dass Entwerfen einer Anwendung Logik **API** Verbindungsressourcen Eigenschaften beim Entwurf Abfragen erstellen.  Die ausgewählten Ressourcengruppe werden die Ressourcengruppe zur Entwurfszeit-Verbindung erstellen.  Sie können anzeigen oder Ändern von API-Verbindungen Azure-Portal und **API-Verbindung**suchen.
    ![Abonnement-Auswahl](./media/app-service-logic-deploy-from-vs/designer_picker.png)
1. Der Designer Rendern sollte basierend auf der Definition in der `<template>.json` Datei.
1. Sie erstellen und Entwerfen Sie Ihre Anwendung Logik und ändert die Bereitstellungsvorlage aktualisiert.
    ![Visual Studio-Designer](./media/app-service-logic-deploy-from-vs/designer_in_vs.png)

Außerdem sehen Sie `Microsoft.Web/connections` Ressourcen Ressourcendatei für alle Anschlüsse der App Logik Funktion hinzugefügt wird.  Diese Verbindungseigenschaften können nach dem **API-Verbindungen** im Azure-Portal bereitstellen festgelegt, wenn Sie bereitstellen, und verwaltet werden.

### <a name="switching-to-the-json-code-view"></a>Der JSON-Codeansicht wechseln

Wählen Sie die **Codeansicht** unten auf die Registerkarte des Designers, um die JSON-Repräsentation der Logik-app wechseln.  Zum Wechseln auf die gesamte Ressource JSON Maustaste der `<template>.json` -Datei und wählen Sie **Öffnen**.

### <a name="saving-the-logic-app"></a>Speichern die Logik-app

Sie können die Logik app jederzeit über die Schaltfläche **Speichern** oder `Ctrl+S`.  Treten Fehler mit Ihrer Anwendung Logik Zeitpunkt speichern, werden sie im Fenster **Ausgaben** von Visual Studio angezeigt.

## <a name="deploying-your-logic-app"></a>Bereitstellen Ihrer Anwendung Logik

Nach der Konfiguration Ihrer Anwendung können Sie außerdem direkt von Visual Studio in nur wenigen Schritten bereitstellen. 

1. Mit der rechten Maustaste auf das Projekt im Projektmappen-Explorer und zum **Bereitstellen** > **Neue Bereitstellung...** 
     ![Neue Bereitstellung](./media/app-service-logic-deploy-from-vs/newdeployment.png)

2. Sie werden aufgefordert, Ihre Azure-Abonnement anmelden. 

3. Jetzt müssen Sie die Details der Ressourcengruppe auswählen, die Sie die Anwendung Logik bereitstellen möchten. 
    ![Ressourcengruppe bereitstellen](./media/app-service-logic-deploy-from-vs/deploytoresourcegroup.png)

     > [AZURE.NOTE]    Achten Sie darauf, wählen Sie die richtige Vorlage und Parameter Dateien für die Ressourcengruppe (z. B. bei der Bereitstellung auf einer Datei der Produktion auswählen möchten). 
4. Wählen Sie die Schaltfläche bereitstellen
 
    
6. Der Status der Bereitstellung wird im **Ausgabefenster** (möglicherweise **Azure-Bereitstellung**auswählen. 
    ![Ausgabe](./media/app-service-logic-deploy-from-vs/output.png)

In Zukunft können Sie Logik-app im Datenquellen-Steuerelement bearbeiten und Visual Studio verwenden, um neue Versionen bereitstellen. 

> [AZURE.NOTE] Wenn die Definition im Azure-Portal direkt ändern, wird das nächste Mal von Visual Studio diese Änderungen bereitstellen überschrieben.

## <a name="next-steps"></a>Nächste Schritte

- Zunächst mit Logik Apps Lernprogramm die [Logik App erstellen](app-service-logic-create-a-logic-app.md) .  
- [Ansicht-Beispiele und Szenarien](app-service-logic-examples-and-scenarios.md)
- [Automatisieren Sie Geschäftsprozesse mit Logik Apps](http://channel9.msdn.com/Events/Build/2016/T694) 
- [Integrieren Sie Ihre Systeme mit Logik Apps](http://channel9.msdn.com/Events/Build/2016/P462)