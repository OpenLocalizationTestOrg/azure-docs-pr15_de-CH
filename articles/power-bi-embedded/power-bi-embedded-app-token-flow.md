<properties
   pageTitle="Authentifizieren und Autorisieren von Power BI Embedded"
   description="Authentifizieren und Autorisieren von Power BI Embedded"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="authenticating-and-authorizing-with-power-bi-embedded"></a>Authentifizieren und Autorisieren von Power BI Embedded

Der Power BI eingebetteten Dienst verwendet **Schlüssel** und **App-Token** zur Authentifizierung und Autorisierung statt expliziter Endbenutzers Authentifizierung. In diesem Modell verwaltet die Anwendung Authentifizierung und Autorisierung für die Endbenutzer. Gegebenenfalls Ihre app erstellt und sendet die App-Token, die unseren Service, der angeforderten Bericht gerendert wird. Dieses Design ist nicht Ihre app von Azure Active Directory für Authentifizierung und Autorisierung erforderlich, noch kann.

## <a name="two-ways-to-authenticate"></a>Zwei Arten zu authentifizieren

**Schlüssel** - Tasten können für alle Power BI eingebettete REST API-Aufrufe. Die Schlüssel in der **Azure-Portal** finden auf **Alle Einstellungen** und **Zugriffstasten**. Immer behandeln Sie den Schlüssel als Kennwort. Diese Schlüssel haben Berechtigungen für alle REST API für Arbeitsbereich Auflistung aufrufen.

Verwendung eines Schlüssels auf einen anderen Aufruf Hinzufügen des folgenden Authorization-Headers:            

    Authorization: AppKey {your key}

**App-Token** - App Token für alle Einbetten von Anfragen verwendet werden. Sie sind clientseitige, ausgeführt werden, damit sie auf einen einzelnen Bericht beschränkt sind und es empfiehlt eine Ablaufzeit festlegen soll.

App-Token sind JWT (JSON Web Token), die mit einem Schlüssel signiert.

Ihre app Token kann folgenden Angaben enthalten:

| Forderung      | Beschreibung        |
|--------------|------------|
| **Ver**      | Die Version des app-Token. 0.2.0 ist die aktuelle Version.       |
| **AUD**      | Der Empfänger des Tokens. Power BI Embedded verwendet: "https://analysis.windows.net/powerbi/api".  |
| **ISS**      |  Eine Zeichenfolge, die Anwendung das Token ausgestellt.    |
| **Typ**     | Der Typ der app Token erstellt wird. Aktuell ist der einzige unterstützte Typ **einbetten**.   |
| **WCN**      | Arbeitsbereich Sammlungsnamen ein Token wird für ausgegeben.  |
| **WID**      | Arbeitsplatz-ID das Token wird für ausgegeben.  |
| **Entfernen**      | Berichts-ID der Token wird für ausgegeben.     |
| **Benutzername** (optional) |  Mit RLS ist dies eine Zeichenfolge, die den Benutzer zu identifizieren, wenn RLS geltenden helfen. |
| **Rollen** (optional)   |   Eine Zeichenfolge, die Rollen bei Zeile Sicherheitsstufe Regeln auswählen. Wenn mehr als eine Rolle, sollten sie als Sting Array übergeben.    |
| **EXP** (optional)    |   Gibt die Zeit an, in der das Token abläuft. Diese sollten als Unix Timestamps übergeben werden.   |
| **NBF** (optional)    |   Gibt die Startzeit in der Token gültig. Diese sollten als Unix Timestamps übergeben werden.   |

Ein Beispiel-app-Token sieht folgendermaßen aus:

![](media\power-bi-embedded-app-token-flow\power-bi-embedded-app-token-flow-sample-coded.png)


Decodiert, wird wie folgt aussehen:

![](media\power-bi-embedded-app-token-flow\power-bi-embedded-app-token-flow-sample-decoded.png)


## <a name="heres-how-the-flow-works"></a>Der Fluss funktioniert

1. Kopieren Sie den API-Schlüssel zur Anwendung. Sie erhalten die Schlüssel in **Azure-Portal**.

    ![](media\powerbi-embedded-get-started-sample\azure-portal.png)

2. Token bestätigt Anspruch und hat eine Ablaufzeit.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-2.png)

3. Token wird mit einer API Zugriffstasten signiert.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-3.png)

4. Benutzeranfragen, einen Bericht anzuzeigen.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-4.png)

5.  Token wird mit einer API Zugriffstasten überprüft.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-5.png)

6.  Power BI eingebetteten Bericht an Benutzer gesendet wird.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-6.png)

Nachdem **Strom BI eingebetteten** Bericht an den Benutzer sendet, kann der Benutzer den Bericht in Ihrer benutzerdefinierten Anwendung anzeigen. Importiert [Analysieren Sales Data PBIX Beispiel](http://download.microsoft.com/download/1/4/E/14EDED28-6C58-4055-A65C-23B4DA81C4DE/Analyzing_Sales_Data.pbix)würde Beispiel Web app so aussehen:

![](media\powerbi-embedded-get-started-sample\sample-web-app.png)

## <a name="see-also"></a>Siehe auch
- [Erste Schritte mit Microsoft Power BI Embedded](power-bi-embedded-get-started-sample.md)
- [Microsoft macht BI Embedded Szenarien](power-bi-embedded-scenarios.md)
- [Erste Schritte mit Microsoft Power BI Embedded](power-bi-embedded-get-started.md)
