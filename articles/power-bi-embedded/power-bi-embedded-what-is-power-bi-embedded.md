<properties
   pageTitle="Was ist Microsoft Power BI Embedded?"
   description="Power BI Embedded können Sie Power BI-Berichten in das Web oder mobilen Anwendung integrieren, müssen Sie keine benutzerdefinierten Lösungen um Daten für die Benutzer"
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

# <a name="what-is-microsoft-power-bi-embedded"></a>Was ist Microsoft Power BI Embedded?

Mit **Power BI Embedded**können Sie Power BI Berichte direkt in Ihr Web oder mobilen Anwendung integrieren.

![](media\powerbi-embedded-whats-is\what-is.png)

Power BI Embedded ist eine **Azure Service** , unabhängige Softwareanbieter und Entwickler Fläche Power BI Daten Erfahrungen in ihre Anwendung ermöglicht. Als Entwickler Applikationen erstellt haben und Anwendungsbereiche haben eigene Benutzer und unterschiedliche Funktionen. Diese apps Vorkommen auch einige integrierte Elemente wie Diagramme und Berichte, die nun von Microsoft Power BI Embedded betrieben werden können. Benutzer brauchen kein Power BI-Konto Ihre app verwenden. Sie können weiterhin die Anwendung wie zuvor anmelden und anzeigen und interagieren mit Power BI reporting Erfahrung ohne zusätzliche Lizenzierung.

## <a name="licensing-for-microsoft-power-bi-embedded"></a>Lizenzen für Microsoft Power BI eingebettet

In **Microsoft Power BI Embedded** Nutzungsmodell ist Lizenzierung für Power BI nicht die Verantwortung des Endbenutzers.  Stattdessen **stellt** vom Entwickler der Anwendung, die die visuellen Objekte belegt erworben und Zahlen auf das Abonnement, das die Ressourcen besitzt.

## <a name="microsoft-power-bi-embedded-conceptual-model"></a>Eingebettete Microsoft Power BI konzeptionelles Modell

![](media\powerbi-embedded-whats-is\model.png)

Wie jeden anderen Dienst in Azure werden Ressourcen für Power BI Embedded über [Azure ARM APIs](https://msdn.microsoft.com/library/mt712306.aspx)bereitgestellt. In diesem Fall ist die Ressource, die Sie bereitstellen **Power BI Arbeitsbereich Auflistung**.

## <a name="workspace-collection"></a>Workspace-Auflistung

Eine **Arbeitsbereich-Auflistung** ist der Container auf oberster Ebene Azure für Ressourcen, der 0 oder mehr **Arbeitsbereiche**enthält.  Eine **Arbeitsbereich** - **Auflistung** enthält alle Standardeigenschaften Azure sowie die folgenden:

-   **Zugriffstasten** -Schlüssel sicher Stromversorgung BI-APIs (in einem späteren Abschnitt beschrieben) angerufen.
-   **Benutzer** – Azure Active Directory (AAD) Benutzer mit Administratorrechten zu Power BI Arbeitsbereich Auflistung über Azure-Portal oder ARM-API.
-   **Region** können als Teil der Bereitstellung einer **Workspace-Auflistung**Sie Region in Bereitstellung auswählen. Weitere Informationen finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/).

## <a name="workspace"></a>Arbeitsbereich

Ein **Arbeitsbereich** ist ein Container für Power BI-Inhalte, die Datasets, Berichte und Dashboards. Ein **Arbeitsbereich** ist leer erstellt. In der Vorschau werden alle Inhalte mit Power BI Desktop erstellen und werden auf einem Ihrer Arbeitsbereiche mit [Power BI REST APIs](http://docs.powerbi.apiary.io/reference)hochladen.

## <a name="using-workspace-collections-and-workspaces"></a>Arbeitsbereich Sammlungen und Arbeitsbereiche verwenden
**Arbeitsbereich Sammlungen** und **Arbeitsbereiche** sind Container, die verwendet und organisiert wie am besten passt den Entwurf der Anwendung erstellen. Stehen viele verschiedene Arten, deren Inhalt anordnen können. Sie können alle Inhalte in einem Arbeitsbereich speichern und dann später app Token die Inhalte Ihrer Kunden weiter unterteilen. Sie können auch alle Ihre Kunden in getrennten Arbeitsbereichen, so dass einige Trennung ist. Alternativ können Benutzer nach Region nicht vom Kunden zu organisieren. Diese Flexibilität können Sie am besten Inhalt organisieren auswählen.

## <a name="cached-datasets"></a>Zwischengespeicherte Datasets

Zwischengespeicherte Datasets können in der Vorschau verwendet werden.  Jedoch kann nicht zwischengespeicherte Daten aktualisieren, nach dem Laden in **Microsoft Power BI Embedded**.

## <a name="authentication-and-authorization-with-app-tokens"></a>Authentifizierung und Autorisierung mit app-Token

**Microsoft macht BI Embedded** verschiebt die Anwendung aller notwendigen Authentifizierung und Autorisierung durchzuführen. Es ist keine explizite Anforderung, dass Endbenutzer Kunden von Azure Active Directory (Azure AD).  Stattdessen gibt die Anwendung **Microsoft Power BI Embedded** Berechtigung zum Power BI-Berichtanzeige **Authentifizierungstokens Anwendung (App Token)**mit.  Diese **App Token** werden erstellt, wenn Sie möchte, dass Ihre Anwendung einen Bericht gerendert.  Finden Sie unter [App-Token](power-bi-embedded-get-started-sample.md#key-flow).

![](media\powerbi-embedded-whats-is\app-tokens.png)

**Authentifizierungstokens Anwendung (App Token)** zum **Microsoft Power BI Embedded**authentifizieren.  Es gibt drei Arten von **App-Token**:

1.  Token - verwendet, wenn ein neuer **Arbeitsbereich** in einem **Arbeitsbereich Auflistung** Bereitstellung bereitstellen
2.  Entwicklung Token - verwendet bei **Power BI REST-APIs** direkt aufrufen
3.  Einbetten von Token - verwendet beim Rendern eines Berichts im eingebetteten Iframe aufrufen

Diese Tokens werden für die verschiedenen Phasen der Interaktionen mit **Microsoft Power BI Embedded**verwendet.  Token sind so konzipiert, dass Ihre App Berechtigungen Power BI delegieren. Weitere Informationen finden Sie unter [App Token fließen](power-bi-embedded-app-token-flow.md).

## <a name="see-also"></a>Siehe auch
- [Microsoft macht BI Embedded Szenarien](power-bi-embedded-scenarios.md)
- [Erste Schritte mit Microsoft Power BI Embedded](power-bi-embedded-get-started.md)
