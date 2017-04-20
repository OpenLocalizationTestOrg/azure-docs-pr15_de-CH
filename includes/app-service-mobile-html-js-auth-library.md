###<a name="server-auth"></a>Gewusst wie: Authentifizierung mit einem Provider (Server Fluss)

Um Mobile Apps Authentifizierungsvorgang in Ihrer Anwendung zu verwalten, müssen Sie Ihre Anwendung mit der Identitätsanbieter registrieren. Dann müssen in Ihrem Azure App Service ID der Anwendung und vom Anbieter bereitgestellte Schlüssel konfigurieren.
Weitere Informationen finden Sie im Lernprogramm [Add Authentifizierung Ihrer app].

Wenn der Identitätsanbieter registriert haben, rufen Sie einfach die .login()-Methode mit dem Namen des Anbieters. Z. B. folgenden Code mit Facebook anmelden.

```
client.login("facebook").done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```

Wenn ein Identitätsprovider als Facebook ändern Anmeldemethode oben auf eine der folgenden übergebenen Wert: `microsoftaccount`, `facebook`, `twitter`, `google`, oder `aad`.

In diesem Fall verwaltet Azure App Service die OAuth 2.0-Authentifizierung wird die Anmeldeseite des ausgewählten Anbieters anzeigen ein Authentifizierungstoken App Service nach erfolgreicher Anmeldung mit der Identitätsanbieter. Die Login-Funktion gibt nach Abschluss des Vorgangs ein JSON-Objekt (Benutzer), die die Benutzer-ID und die App Service macht Authentifizierungstoken im Feld Benutzer-ID und AuthenticationToken bzw.. Dieses Token kann zwischengespeichert und wiederverwendet, bis es abläuft.

###<a name="client-auth"></a>Gewusst wie: Authentifizierung mit einem Provider (Client Fluss)

Ihre app kann auch wenden Sie sich an die Identität und geben das zurückgegebene Token mit dem App-Dienst für die Authentifizierung. Diese Client-Fluss können Sie einzelne anmelden für Benutzer oder weitere Daten vom Identitätsanbieter abrufen.

#### <a name="social-authentication-basic-example"></a>Einfaches Beispiel für soziale Authentifizierung

Dieses Beispiel verwendet für die Authentifizierung Facebook Client SDK:

```
client.login(
     "facebook",
     {"access_token": token})
.done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```
Es wird angenommen, dass das Token der jeweiligen Anbieter SDK in der token-Variablen gespeichert ist.

#### <a name="microsoft-account-example"></a>Microsoft Account-Beispiel

Im folgenden Beispiel wird das Live SDK Microsoft Account mit Single-Sign-on für Windows Store-apps unterstützt:

```
WL.login({ scope: "wl.basic"}).then(function (result) {
      client.login(
            "microsoftaccount",
            {"authenticationToken": result.session.authentication_token})
      .done(function(results){
            alert("You are now logged in as: " + results.userId);
      },
      function(error){
            alert("Error: " + err);
      });
});
```

In diesem Beispiel wird einen Token von Live Connect, die App Service Anmelde Funktion angegeben wird.

###<a name="auth-getinfo"></a>Gewusst wie: Informationen über den authentifizierten Benutzer

Die Authentifizierungsinformationen für den aktuellen Benutzer aus abgerufen werden die `/.auth/me` Endpunkt mit AJAX.  Sicherzustellen, legen Sie die `X-ZUMO-AUTH` Ihr Authentifizierungstoken Header.  Das Authentifizierungstoken werden in `client.currentUser.mobileServiceAuthenticationToken`.  Wenn Sie beispielsweise Fetch API verwenden:

```
var url = client.applicationUrl + '/.auth/me';
var headers = new Headers();
headers.append('X-ZUMO-AUTH', client.currentUser.mobileServiceAuthenticationToken);
fetch(url, { headers: headers })
    .then(function (data) {
        return data.json()
    }).then(function (user) {
        // The user object contains the claims for the authenticated user
    });
```

FETCH steht als ein Npm oder von CDNJS Browser heruntergeladen. JQuery oder anderen AJAX API können auch die Informationen abzurufen.  Daten werden als JSON-Objekt empfangen.
