<properties 
    pageTitle="Windows Phone Silverlight SDK Upgrade-Verfahren" 
    description="Windows Phone Silverlight SDK Aktualisierungsverfahren Azure Mobile Engagement"                  
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede"
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-sdk-upgrade-procedures"></a>Windows Phone Silverlight SDK Upgrade-Verfahren

Wenn bereits eine ältere Version unseres SDK in die Anwendung integriert haben, müssen Sie Punkte im SDK aktualisieren.

Möglicherweise müssen mehrere Verfahren, wenn Sie mehrere Versionen des SDK verpasst. Zum Beispiel wenn Sie migrieren von 0.10.1, Sie zuerst die Prozedur "von 0.9.0 zu 0.10.1 müssen" 0.11.0 "von 0.10.1 zu 0.11.0" Verfahren.

##<a name="from-200-to-330"></a>Von 2.0.0 auf 3.3.0

### <a name="test-logs"></a>Testprotokolle

Konsolenprotokolle erzeugt vom SDK können jetzt aktiviert/deaktiviert/gefiltert werden. Um dies anzupassen, aktualisieren Sie die Eigenschaft `EngagementAgent.Instance.TestLogEnabled` auf einen Wert von der `EngagementTestLogLevel` -Enumeration, beispielsweise:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

##<a name="from-111-to-200"></a>Von 1.1.1 zu 2.0.0

Die folgenden beschreibt der Capptain Service Capptain SAS in einer Anwendung bereitgestellt von Azure Mobile Engagement SDK Integration migrieren. 

> [Azure.IMPORTANT] Capptain und Mobile Engagement nicht dieselben Dienste und der unten beschriebenen Vorgehensweise zeigt nur die Clientanwendung zu migrieren. Migrieren das SDK in die Anwendung migrieren nicht Daten von Servern Capptain Mobile Engagement Server zu

Wenn Sie von einer früheren Version migrieren, finden Sie in der Capptain-Website migrieren, 1.1.1 zuerst dann folgenden Verfahren

### <a name="nuget-package"></a>NuGet-Paket

Ersetzen Sie **Capptain.WindowsPhone** durch **MicrosoftAzure.MobileEngagement** Nuget-Paket.

### <a name="applying-mobile-engagement"></a>Mobile Engagement anwenden

Das SDK verwendet den Begriff `Engagement`. Sie müssen das Projekt entsprechend dieser Änderung aktualisieren.

Sie müssen das aktuelle Capptain Nuget-Paket deinstallieren. Beachten Sie, dass alle Änderungen im Ordner Capptain Ressourcen entfernt werden. Möchten Sie diese Dateien dann eine Kopie davon.

Danach installieren Sie neue Microsoft Azure Engagement Nuget-Paket im Projekt. Sie finden direkt auf [Nuget](http://www.nuget.org/packages/MicrosoftAzure.MobileEngagement). Diese Aktion ersetzt alle Ressourcendateien Engagement und Ihre Projektverweise neue Engagement DLL hinzugefügt.

Sie müssen die Projektverweisen Capptain DLL-Verweise löschen säubern. Wenn Sie dies nicht vornehmen, Version Capptain Konflikte und Fehler auftreten.

Wenn Sie Capptain Ressourcen angepasst haben, kopieren Sie die alten Dateien Inhalte, und fügen sie im neuen Projekt Dateien. Beachten Sie, dass Xaml und Cs-Dateien aktualisiert werden.

Wenn diese Schritte müssen Sie nur alte Capptain Referenzen durch neuen Engagement Referenzen ersetzen.

1. Alle Capptain Namespaces müssen aktualisiert werden.

    Vor der Migration:
    
        using Capptain.Agent;
        using Capptain.Reach;
    
    Nach der Migration:
    
        using Microsoft.Azure.Engagement;

2. Alle Capptain Klassen mit "Capptain" sollte "Projekt".

    Vor der Migration:
    
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
    
    Nach der Migration:
    
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }

3. Für XAML-Dateien ändern sich auch Capptain Namespace und Attribute.

    Vor der Migration:
    
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
    
    Nach der Migration:
    
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>

4. Andere Ressourcen wie Bilder Capptain Beachten Sie, dass sie auch umbenannt wurden, verwenden Sie "Projekt".

### <a name="application-id--sdk-key"></a>ID der Anwendung / SDK-Schlüssel

Engagement verwendet eine Verbindungszeichenfolge. Sie haben eine ID und einen Schlüssel SDK mit Mobile angeben, müssen Sie nur eine Verbindungszeichenfolge angeben. Sie können sie auf die Datei EngagementConfiguration einrichten.

Engagement Konfiguration legen Sie der `Resources\EngagementConfiguration.xml` Datei des Projekts.

Bearbeiten Sie diese Datei an:

-   Die Verbindungszeichenfolge der Anwendung zwischen Tags `<connectionString>` und `<\connectionString>`.

Wenn Sie stattdessen zur Laufzeit angeben möchten, können Sie die folgende Methode vor der Initialisierung Agent Engagement aufrufen:

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        
        /* Initialize Engagement angent with above configuration. */
        EngagementAgent.Instance.Init(engagementConfiguration);

Die Verbindungszeichenfolge für die Anwendung wird in der Azure-Verwaltungsportal angezeigt.

### <a name="items-name-change"></a>Namen ändern

Alle Elemente mit dem Namen *Capptain* wurden *Engagement*benannt. Auch für *Capptain* *zu*.

Beispiele für häufig verwendete Capptain Artikel:

-   Nun den Namen EngagementConfiguration CapptainConfiguration
-   Nun den Namen EngagementAgent CapptainAgent
-   Nun den Namen EngagementReach CapptainReach
-   Nun den Namen EngagementHttpConfig CapptainHttpConfig
-   Nun den Namen GetEngagementPageName GetCapptainPageName

Beachten Sie diese umbenennen überschriebene Methoden beeinflusst.



 
