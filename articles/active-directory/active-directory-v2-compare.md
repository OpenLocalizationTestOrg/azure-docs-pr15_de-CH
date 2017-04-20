<properties
    pageTitle="Der Endpunkt Azure AD v2. 0 | Microsoft Azure"
    description="Ein Vergleich zwischen der ursprünglichen Azure und Endpunkte v2. 0."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="whats-different-about-the-v20-endpoint"></a>Was ist anders bei den Endpunkt v2. 0?

Wenn Sie Azure Active Directory kennen oder Azure AD in der Vergangenheit apps integriert haben, möglicherweise Unterschiede in V2. 0-Endpunkt, den Sie nicht erwarten.  In diesem Dokument wird die Unterschiede für Ihr Verständnis.

> [AZURE.NOTE]
    Nicht alle Azure Active Directory Szenarien und Funktionen werden vom Endpunkt v2. 0 unterstützt.  Um festzustellen, ob der v2. 0-Endpunkt verwenden sollten, lesen Sie [v2. 0-Einschränkungen](active-directory-v2-limitations.md).


## <a name="microsoft-accounts-and-azure-ad-accounts"></a>Microsoft und Azure AD-Konten
v2. 0-Endpunkt können Entwickler apps schreiben, die von Microsoft Accounts und Azure AD-Konten mithilfe eines einzigen Auth-Endpunkts anmelden akzeptieren.  Dadurch werden Ihre app vollständig Konto agnostischen schreiben; die Art des Kontos nicht möglich, die der Benutzer bei anmeldet.  Natürlich *können* Ihre app stellen den Typ des in einer bestimmten Sitzung verwendete Konto über, aber Sie müssen.

Beispielsweise wenn Ihre Anwendung [Microsoft Graph](https://graph.microsoft.io)aufruft, werden einige zusätzliche Funktionen und Daten für Benutzer wie SharePoint-Websites oder Verzeichnisdaten verfügbar.  Jedoch für viele Aktionen, wie das [Lesen von e-Mail-Nachrichten des Benutzers](https://graph.microsoft.io/docs/api-reference/v1.0/resources/message), der Code geschrieben werden kann dasselbe Microsoft Accounts und Azure AD-Konten.  

Microsoft-Accounts und Azure AD-Konten Ihrer app integrieren ist jetzt ein einfacher Prozess.  Einheitliche Endpunkte einer Bibliothek und einer einzelnen app-Registrierung können Sie Zugriff auf die Verbraucher und Unternehmen weltweit.  Erfahren Sie mehr über die v2. 0-Endpunkt anzeigen [der Übersicht](active-directory-appmodel-v2-overview.md)


## <a name="new-app-registration-portal"></a>Neue Anwendung registrierungsportal
v2. 0-Endpunkt kann nur an einem neuen Speicherort registriert werden: [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Dies ist das Portal wo Personalnummer Anwendung bekomme Aussehen Ihrer app-Seite und mehr.  Auf das Portal zugreifen müssen lediglich ein powered microsoftkonto - persönliche oder Arbeit oder Schule.  

Werden wir diese App Registrierungsportal mit der Zeit mehr Funktionen hinzuzufügen.  Die Absicht ist, dass dieses Portal neue Position wo Sie alles mit Ihrem Microsoft-apps verwalten.


## <a name="one-app-id-for-all-platforms"></a>Eine app-Id für alle Plattformen
Im ursprünglichen Azure Active Directory-Dienst können mehrere verschiedene apps für ein einzelnes Projekt registriert haben.  Sie mussten separate app-Registrierungen für systemeigene Clients und Web-apps verwenden:

![Registrierung der alten Anwendung Benutzeroberfläche](../media/active-directory-v2-flows/old_app_registration.PNG)

Z. B. Wenn Sie eine Website und eine iOS-app erstellt, musste separat registriert mit zwei verschiedenen Anwendung Ids.  Wenn Sie eine Website und eine Back-End-web-api, können Sie als eine separate app in Azure AD registriert haben.  Wenn Sie eine app für iOS und Android app, Sie auch möglicherweise zwei verschiedene apps registriert haben.  

<!-- You may have even registered different apps for each of your build environments - one for dev, one for test, and one for production. -->

Jetzt brauchen Sie ist eine einzelne app-Registrierung und eine einzelne Anwendung Id für jedes Ihrer Projekte.  Sie können jedes Projekt mehrere "Plattformen" hinzufügen und die entsprechenden Angaben für jede Plattform, die Sie hinzufügen.  Natürlich können Sie beliebig viele apps je nach Bedarf, aber für den meisten Fällen sollte nur eine Anwendung Id erforderlich sein erstellen.

<!-- You can also label a particular platform as "production-ready" when it is ready to be published to the outside world, and use that same Application Id safely in your development environments. -->

Unser Ziel ist es, mehr vereinfachtes Management des app-Entwicklung führen, und Erstellen einer konsolidierte Ansicht eines einzelnen Projekts, an dem Sie arbeiten können.


## <a name="scopes-not-resources"></a>Bereiche nicht Ressourcen
Im ursprünglichen Azure AD-Dienst kann eine Anwendung als eine **Ressource**oder einen Empfänger des Tokens Verhalten.  Definieren einer Ressource kann mehrere **Bereiche** oder **oAuth2Permissions** versteht, sodass apps Token, die die Ressource für einen bestimmten Satz von Bereichen anfordern.  Beachten Sie die Azure AD Graph-API als Beispiel einer Ressource:

- Resource Identifier oder `AppID URI`:`https://graph.windows.net/`
- Bereiche oder `OAuth2Permissions`: `Directory.Read`, `Directory.Write`usw..  

All dies gilt für die v2. 0-Endpunkt.  Eine Anwendung kann dennoch Verhalten als Ressource definieren Bereiche und durch einen URI identifiziert werden.  Clientanwendungen können weiterhin auf diese Bereiche anfordern.  Die Möglichkeit, in der ein Client die Berechtigungen anfordert, wurde jedoch geändert.  In der Vergangenheit OAuth 2.0 autorisieren Anforderung Azure AD wie haben kann:

```
GET https://login.microsoftonline.com/common/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&resource=https%3A%2F%2Fgraph.windows.net%2F
...
```

**Ressource** -Parameter die Ressource angegeben die Clientanwendung Autorisierung anfordert.  Azure AD berechnet die app auf statische Konfiguration in der Azure-Portal und ausgestellte Token entsprechend erforderlichen Berechtigungen.  Jetzt autorisieren dieselbe OAuth 2.0 Anforderung sieht wie folgt aus:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

wobei **der Bereichsparameter** denen Ressourcen und Berechtigungen fordert die Anwendung Autorisierung für. Die gewünschte Ressource noch sehr in der Anforderung vorhanden - ist einfach in die Werte des Parameters Bereich umfasst.  Der Bereichsparameter auf diese Weise ermöglicht den v2. 0-Endpunkt um mehr OAuth 2.0-Spezifikation und Branchen-Practices genauer ausgerichtet.  Außerdem können apps [inkrementelle Zustimmung](#incremental-and-dynamic-consent)ausführen, die im nächsten Abschnitt beschrieben.

## <a name="incremental-and-dynamic-consent"></a>Inkrementelle und dynamische Zustimmung
Apps erhältlich Azure AD registriert erforderliche jeweils erforderlichen Berechtigungen OAuth 2.0 in Azure-Portal zur Erstellungszeit app angeben:

![Berechtigungen Registrierung Benutzeroberfläche](../media/active-directory-v2-flows/app_reg_permissions.PNG)

**Statisch**konfigurierten Berechtigungen eine Anwendung erforderlich sind.  Während dieser Konfiguration der Anwendung in Azure-Portal vorhanden sein darf und den Code einfach, stellt es ein paar Probleme für Entwickler:

- App hat alle Berechtigungen kennen, die sie jemals zur Erstellungszeit app muss.  Berechtigungen mit der Zeit war schwierig.
- Eine app mussten alle Ressourcen kennen, die sie immer rechtzeitig zugreifen.  Es war schwierig, apps erstellen, die eine beliebige Anzahl von Ressourcen zugreifen können.
- Eine Anwendung musste die Berechtigungen anzufordern, die jemals bei der ersten Anmeldung für den Benutzer müsste.  In einigen Fällen führte dies auf eine sehr lange Liste von Berechtigungen, die dem Endbenutzer die Genehmigung der Anwendung Zugriff auf ersten Anmelden abgeraten.

Mit dem Endpunkt v2. 0 können Sie die für Ihre Anwendung **dynamisch**zur Laufzeit während der normalen Verwendung Ihrer Anwendung erforderlichen Berechtigungen angeben.  Dazu können Sie Ihre app jederzeit zeitlich durch Einschließen in muss Bereiche der `scope` Parameter eine Authentifizierungsanfrage:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

Die obigen fordert die Berechtigung für die app ein Azure AD-Benutzerobjekt Verzeichnisdaten sowie Daten in ihrem Verzeichnis schreiben lesen.  Wenn der Benutzer die Berechtigungen in der Vergangenheit für diese spezielle app zugestimmt hat, sie geben ihre Anmeldeinformationen und die app angemeldet.  Wenn der Benutzer alle Berechtigungen nicht zugestimmt hat, fragt v2. 0-Endpunkt den Benutzer für die Erlaubnis, diese Berechtigungen.  Weitere können Sie [Berechtigungen](active-directory-v2-scopes.md)und Zustimmung Bereiche nachlesen.

Damit eine app Anfrageberechtigungen dynamisch über das `scope` bietet Ihnen vollständige Kontrolle über Ihre Benutzer.  Wenn Sie möchten, können Sie Frontload Ihre Zustimmung auftreten, und fordern Sie alle Berechtigungen in eine erste Anforderung.  Oder wenn Ihre Anwendung eine große Anzahl von Berechtigungen erfordert, können Sie die Berechtigungen des Benutzers inkrementell erfassen für bestimmte Features Ihrer App langfristig versuchen.

## <a name="well-known-scopes"></a>Bekannte Bereiche

#### <a name="offline-access"></a>Offline-Zugriff
v2. 0-Endpunkt erfordern die Verwendung einer neuen bekannten Berechtigung für apps – der `offline_access` Bereich.  Alle apps müssen diese Berechtigung anfordern, benötigen sie Zugriff auf Ressourcen im Auftrag eines Benutzers für längere Zeit, selbst wenn der Benutzer nicht aktiv die Anwendung verwendet werden kann.  Die `offline_access` Bereich erscheint dem Benutzer Zustimmung Dialogfeldern als "Zugriff auf Ihre Daten offline", denen der Benutzer zustimmen muss.  Anfordern der `offline_access` Berechtigung ermöglicht Ihrer Anwendung an v2. 0-Endpunkt OAuth 2.0 Aktualisierungstoken erhalten.  Aktualisierungstoken sind langlebig und längere Zugriff für neue OAuth 2.0 Access_tokens ausgetauscht werden können.  

Wenn Ihre Anwendung nicht wünscht die `offline_access` Bereich, erhalten keine Aktualisierungstoken.  Dies bedeutet, dass beim einlösen Authorization_code [OAuth 2.0 Authorization Code Datenfluss](active-directory-v2-protocols.md#oauth2-authorization-code-flow)nur wieder ein Zugriffstoken aus Sie erhalten die `/token` Endpunkt.  Dieses Zugriffstoken kurzer (in der Regel eine Stunde) gültig bleiben, aber schließlich ablaufen.  Zeitpunkt Ihrer app muss Benutzer umleiten zu sichern die `/authorize` Endpunkt eine neue Authorization_code abrufen.  Während diese Umleitung der Benutzer oder nicht müssen ihre Anmeldeinformationen oder Berechtigungen erneut Einverständnis abhängig von den Anwendungstyp.

Weitere Informationen über OAuth 2.0, Aktualisierungstoken und Access_tokens anzeigen Sie [2.0 Protokoll Referenz](active-directory-v2-protocols.md)

#### <a name="openid-profile--email"></a>OpenID, Profil und e-Mail

Im ursprünglichen Azure Active Directory-Dienstes würde der grundlegenden OpenID verbinden anmelden Fluss bereitstellen eine Fülle von Informationen über den Benutzer in der resultierenden ID-Token.  Ansprüche in ein ID-Token können des Benutzers Name, bevorzugte Benutzername, e-Mail-Adresse, Objekt-ID und mehr enthalten.

Wir werden nun die Informationen einschränken, die `openid` Bereich bietet Ihre app auf.  Bereich "Openid" nur können sich Benutzer in Ihrer Anwendung und einen anwendungsspezifische Bezeichner für den Benutzer angezeigt.  Wenn Sie personenbezogene Informationen (PII) über den Benutzer in der app abrufen möchten, müssen Ihre app Benutzer weitere Berechtigungen anfordern.  Wir stellen die beiden neuen Bereiche – der `email` und `profile` Bereiche – dazu können.

Die `email` ist sehr einfach – ermöglicht den Anwendung Zugriff auf e-Mail-Adresse des Benutzers über die `email` des Anspruchs der ID-Token.  Die `profile` Bereich bietet Ihre app auf andere grundlegende Informationen über den Benutzer-Namen, bevorzugte Benutzername, Objekt-ID usw..

Dadurch können Sie Ihre Anwendung in einer minimale Offenlegung Weise code – können Benutzer nur für diejenigen Informationen, die Ihre Anwendung erfordert zu bitten.  Finden Sie weitere Informationen zu diesen Bereichen auf [Bereich v2. 0](active-directory-v2-scopes.md). 

## <a name="token-claims"></a>Token Ansprüche

Die Ansprüche im Token ausgestellt von v2. 0-Endpunkt nicht allgemein verfügbar ausgestellte Token mit Endpunkten Azure AD - apps Migration zu den neuen Service nicht vorausgesetzt ein Anspruch besteht in der ID-Token oder Access_tokens.   V2. 0-Endpunkt ausgestellte Token entsprechen den Spezifikationen OAuth 2.0 und OpenID verbinden, aber andere Semantik als der in der Regel folgen Azure Active Directory.

Finden Sie Informationen zu bestimmten Ansprüche im Token v2. 0 ausgegeben [Tokenverweis v2. 0](active-directory-v2-tokens.md).

## <a name="limitations"></a>Grenzen
Gibt einige Einschränkungen bei der v2. 0 mit berücksichtigen.  Siehe [2.0 Einschränkungen Doc](active-directory-v2-limitations.md) auf diese Einschränkung für Ihr spezielles Szenario geeignet.
