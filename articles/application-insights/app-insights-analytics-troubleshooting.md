<properties 
    pageTitle="Analytics - mächtiges Werkzeug Anwendung Erkenntnisse Fehlerbehebung | Microsoft Azure" 
    description="Probleme mit Application Insights Analytics? Beginnen Sie hier. " 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/11/2016" 
    ms.author="awills"/>


# <a name="troubleshoot-analytics-in-application-insights"></a>Problembehandlung bei Analysen in Anwendung Einblicke


Probleme mit [Anwendungsanalyse Einblicke](app-insights-analytics.md)? Beginnen Sie hier. Analytics ist der leistungsstarken Suchfunktion Visual Studio Application Insights.



## <a name="limits"></a>Grenzen

* Derzeit sind Abfrageergebnisse auf mehr als einer Woche Daten.
* Wir testen auf Browser: aktuelle Versionen von Chrome, Edge und Internet Explorer.


## <a name="known-incompatible-browser-extensions"></a>Browsererweiterungen von bekannten nicht kompatibel

* Ghostery

Deaktivieren der Erweiterung oder einen anderen Browser verwenden.


##<a name="e-a"></a>"Unerwarteter Fehler"

![Unerwarteter Fehler-Bildschirm](./media/app-insights-analytics-troubleshooting/010.png)

Interner Fehler beim Portal Runtime-Ausnahmefehler.

* Reinigen Sie den Cache des Browsers. 

## <a name="e-b"></a>403... zu laden versuchen

![403... zu laden versuchen](./media/app-insights-analytics-troubleshooting/020.png)

Eine Authentifizierung bezogenen Fehler (während der Authentifizierung oder während der Generierung von Überprüfungstoken). Das Portal haben keine Möglichkeit, ohne Browsereinstellungen wiederherzustellen.

* Vergewissern Sie sich im Browser [Cookies von Drittanbietern aktiviert](#cookies) . 


## <a name="authentication"></a>403... Sicherheitszone überprüfen

![403... Sicherheitszone .verify](./media/app-insights-analytics-troubleshooting/030.png)

Eine Authentifizierung bezogenen Fehler (während der Authentifizierung oder während der Generierung von Überprüfungstoken). Das Portal haben keine Möglichkeit, ohne Browsereinstellungen wiederherzustellen.

1. Vergewissern Sie sich im Browser [Cookies von Drittanbietern aktiviert](#cookies) . 

2. Haben Sie auf das Portal Analytics ein Favorit, Lesezeichen oder gespeicherte Verknüpfung verwendet? Ist signiert mit anderen Anmeldeinformationen als die Verknüpfung gespeichert?

2. Verwenden Sie eine Private/Incognito Browserfenster (nach dem schließen alle Fenster). Sie müssen Ihre Anmeldeinformationen. 

2. Öffnen einer anderen (normalen) Browserfenster und [Azure](https://portal.azure.com). Melden Sie sich ab. Öffnen Sie die Verknüpfung und mit Anmeldeinformationen anmelden.

2. Rand und Internet Explorer-Benutzer können auch diese Fehlermeldung wenn vertrauenswürdige Zonen nicht unterstützt werden.

    [Analytics-Portal](https://analytics.applicationinsights.io) und [Azure Active Directory Portal](https://portal.azure.com) befinden sich in der gleichen Sicherheitszone überprüfen:

 * Öffnen Sie in Internet Explorer **Internetoptionen**, **Sicherheit**, **Vertrauenswürdige Sites**, **Sites**:

    ![Dialogfeld "Internetoptionen", vertrauenswürdige Websites eine Website hinzufügen](./media/app-insights-analytics-troubleshooting/033.png)

    In der Liste Websites die folgenden URLs werden Stellen Sie sicher, dass die anderen auch enthalten sind:

    https://Analytics.applicationinsights.IO<br/>
   https://Login.microsoftonline.com<br/>
   https://Login.Windows.NET


## <a name="e-d"></a>404... Die Ressource wurde nicht gefunden.

![404... Ressource wurde nicht gefunden](./media/app-insights-analytics-troubleshooting/040.png)

Anwendungsressource von Anwendung gelöscht wurde und nicht mehr verfügbar. Dies ist die URL der Seite Analytics gespeichert.


## <a name="e-e"></a>403... Keine Berechtigung

![403... nicht autorisiert](./media/app-insights-analytics-troubleshooting/050.png)

Sie haben keine Berechtigung, diese Anwendung in Analytics öffnen.

* Haben Sie den Link von einer anderen Person? Fragen sie unbedingt in den [Leser oder die Mitwirkenden für diese Ressourcengruppe](app-insights-resources-roles-access-control.md)befinden.
* Haben Sie die Verknüpfung mit anderen Anmeldeinformationen gespeichert? Öffnen der [Azure-Portal](https://portal.azure.com), Abmelden und versuchen Sie es hier erneut Anmeldeinformationen bereitstellen.

## <a name="html-storage"></a>403... HTML5-Speicher

Unser Portal verwendet HTML5 LocalStorage und SessionStorage.

* Chrome: Einstellungen, Datenschutz, Content Einstellungen.
* InternetExplorer: Internetoptionen, Registerkarte Erweitert Sicherheit aktivieren DOM-Speicherung


![403... versuchen, HTML5 Speicher aktivieren](./media/app-insights-analytics-troubleshooting/060.png)

## <a name="e-g"></a>404... Abonnement wurde nicht gefunden


![404... Abonnement wurde nicht gefunden](./media/app-insights-analytics-troubleshooting/070.png)

Die URL ist ungültig. 

* Öffnen Sie die app Ressource in [Application Insights-Portal](https://portal.azure.com). Verwenden Sie dann die Schaltfläche Analytics.

## <a name="e-h"></a>404... existiert nicht

![404... Seite ist nicht vorhanden.](./media/app-insights-analytics-troubleshooting/080.png)

Die URL ist ungültig.

* Öffnen Sie die app Ressource in [Application Insights-Portal](https://portal.azure.com). Verwenden Sie dann die Schaltfläche Analytics.

## <a name="cookies"></a>Drittanbieter-Cookies aktivieren

  [Drittanbieter-Cookies deaktivieren](http://www.digitalcitizen.life/how-disable-third-party-cookies-all-major-browsers), sondern benötigen wir **Aktivieren** sie.

## <a name="e-x"></a>Wenn alles andere schiefgeht    

[Wenden Sie sich an uns](app-insights-get-dev-support.md).
 
[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

