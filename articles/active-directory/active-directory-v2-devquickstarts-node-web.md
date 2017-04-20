<properties
    pageTitle="Azure AD v2. 0 NodeJS Web App | Microsoft Azure"
    description="Wie eine Knoten JS Web app erstellen, die Benutzer mit beiden persönlichen Microsoft Account und Geschäfts-oder Zeichen."
    services="active-directory"
    documentationCenter="nodejs"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
  ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="add-sign-in-to-a-nodejs-web-app"></a>Hinzufügen einer NodeJS Web App anmelden


> [AZURE.NOTE]
    Nicht alle Azure Active Directory Szenarien und Funktionen werden vom Endpunkt v2. 0 unterstützt.  Um festzustellen, ob der v2. 0-Endpunkt verwenden sollten, lesen Sie [v2. 0-Einschränkungen](active-directory-v2-limitations.md).


Hier verwenden wir Passport:

- Die app Azure AD Benutzer anmelden und den Endpunkt des v2. 0.
- Einige Informationen über den Benutzer angezeigt.
- Abmelden des Benutzers von der Anwendung.

**Passport** ist Middleware für Node.js Authentifizierung. Flexibel und modular Passport kann unauffällig gelöscht werden auf jedem Express oder Restify Anwendung. Umfassende Strategien unterstützt Authentifizierung mit Benutzername und Kennwort, Facebook und Twitter. Wir haben eine Strategie für Microsoft Azure Active Directory entwickelt. Wir installieren dieses Modul und fügen Sie die Microsoft Azure Active Directory `passport-azure-ad` -Plug-in.

## <a name="download"></a>Herunterladen

Der Code für dieses Lernprogramm wird [auf GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs)verwaltet.  Um folgen, können Sie [die Anwendung Skelett als .zip downloaden](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/skeleton.zip) oder das Skelett Klonen:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

Die fertiggestellte Anwendung wird am Ende dieses Lernprogramms bereitgestellt.

## <a name="1-register-an-app"></a>1. Registrieren einer Anwendung
Erstellen Sie eine neue Anwendung auf [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)und befolgen Sie diese [Anleitung](active-directory-v2-app-registration.md).  Stellen Sie sicher, dass:

- Kopieren Sie die **Id der Anwendung** für Ihre Anwendung zugewiesen, Sie benötigen rasch.
- **Web** Plattform für Ihre Anwendung hinzufügen.
- Geben Sie den entsprechenden **URI umleiten**. Die Umleitung URI gibt Azure AD, Authentifizierungsantworten richten - die Standardeinstellung für dieses Lernprogramm ist `http://localhost:3000/auth/openid/return`.

## <a name="2-add-pre-requisities-to-your-directory"></a>2. vor-Requisities zum Verzeichnis hinzufügen

Die Befehlszeile wechseln Sie Stammordner nicht bereits vorhanden, und führen Sie die folgenden Befehle:

- `npm install express`
- `npm install ejs`
- `npm install ejs-locals`
- `npm install restify`
- `npm install mongoose`
- `npm install bunyan`
- `npm install assert-plus`
- `npm install passport`
- `npm install webfinger`
- `npm install body-parser`
- `npm install express-session`
- `npm install cookie-parser`

- Darüber hinaus verwenden wir `passport-azure-ad` im Skelett Schnellstart.

- `npm install passport-azure-ad`


Dies installiert die Bibliotheken, Passport-Azure-Ad abhängig.

## <a name="3-set-up-your-app-to-use-the-passport-node-js-strategy"></a>3. richten Sie Ihrer Anwendung die Passport-Knoten-Js-Strategie ein
Konfigurieren Sie hier Express Middleware um das Authentifizierungsprotokoll OpenID verbinden verwenden.  Passport verwendet anmelden und Abmelden Anforderungen, die Sitzung des Benutzers verwalten und Informationen über den Benutzer unter anderem.

-   Öffnen Sie zunächst das `config.js` -Datei im Stammverzeichnis des Projekts, und geben Sie Werte in Ihre Anwendung die `exports.creds` Abschnitt.
    -   Die `clientID:` ist Ihre app im registrierungsportal zugewiesene **Id der Anwendung** .
    -   Die `returnURL` ist im Portal eingegebene **URI umleiten** .
    - Die `clientSecret` ist der Schlüssel im Portal erstellt.

- Öffnen Sie `app.js` im Stamm der Projekt-Datei und fügen Sie den folgenden Aufruf Aufrufen der `OIDCStrategy` mit Strategie`passport-azure-ad`


```JavaScript
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

// Add some logging
var log = bunyan.createLogger({
    name: 'Microsoft OIDC Example Web Application'
});
```

- Danach verwenden Sie wir nur verwiesen, um unsere Login Anfragen Strategie

```JavaScript
// Use the OIDCStrategy within Passport. (Section 2)
//
//   Strategies in passport require a `validate` function, which accept
//   credentials (in this case, an OpenID identifier), and invoke a callback
//   with a user object.
passport.use(new OIDCStrategy({
    callbackURL: config.creds.returnURL,
    realm: config.creds.realm,
    clientID: config.creds.clientID,
    clientSecret: config.creds.clientSecret,
    oidcIssuer: config.creds.issuer,
    identityMetadata: config.creds.identityMetadata,
    responseType: config.creds.responseType,
    responseMode: config.creds.responseMode,
    skipUserProfile: config.creds.skipUserProfile
    scope: config.creds.scope
  },
  function(iss, sub, profile, accessToken, refreshToken, done) {
    log.info('Example: Email address we received was: ', profile.email);
    // asynchronous verification, for effect...
    process.nextTick(function () {
      findByEmail(profile.email, function(err, user) {
        if (err) {
          return done(err);
        }
        if (!user) {
          // "Auto-registration"
          users.push(profile);
          return done(null, profile);
        }
        return done(null, user);
      });
    });
  }
));
```
Passport verwendet ein ähnliches Muster für alles Strategien (Twitter, Facebook, etc.), die alle Autoren Strategie entsprechen. Die Strategie sehen Sie, dass es eine möchten übergeben, die ein Token und fertig als Parameter. Strategie pflichtgemäß zurück zu uns kommen nach allen ist die Arbeit. Sobald dies wollen wir den Benutzer speichern und das Token speichern, damit wir wieder Fragen müssen.

> [AZURE.IMPORTANT]
Der obige Code hat jeder Benutzer, der zur Authentifizierung an unseren Server geschieht. Dies wird als automatische Registrierung bezeichnet. Bei Produktionsservern möchte Sie niemanden ohne eine Registrierung gehen Sie entscheiden. Dies ist normalerweise das Muster, das Sie im Consumer apps anzeigen, die können Sie mit Facebook anmelden, aber dann aufgefordert, zusätzliche Informationen. Diese Beispiel-Anwendung nicht könnten wir einfach die e-Mail aus dem token Objekt extrahieren haben, die zurückgegeben und dann aufgefordert, Weitere Informationen einzugeben. Da dies einen Testserver wir einfach die Speicherdatenbank hinzufügen.

- Als Nächstes fügen Sie die Methoden, die angemeldeten Benutzer gemäß Passport mitverfolgen können. Dazu gehören beim Serialisieren und Deserialisieren der Benutzerinformationen:

```JavaScript

// Passport session setup. (Section 2)

//   To support persistent login sessions, Passport needs to be able to
//   serialize users into and deserialize users out of the session.  Typically,
//   this will be as simple as storing the user ID when serializing, and finding
//   the user by ID when deserializing.
passport.serializeUser(function(user, done) {
  done(null, user.email);
});

passport.deserializeUser(function(id, done) {
  findByEmail(id, function (err, user) {
    done(err, user);
  });
});

// array to hold logged in users
var users = [];

var findByEmail = function(email, fn) {
  for (var i = 0, len = users.length; i < len; i++) {
    var user = users[i];
    log.info('we are using user: ', user);
    if (user.email === email) {
      return fn(null, user);
    }
  }
  return fn(null, null);
};

```

- Als Nächstes fügen Sie den Code, um das express-Modul geladen. Hier sehen Sie mithilfe der Standard-/views und Express /routes-Muster bietet.

```JavaScript

// configure Express (Section 2)

var app = express();


app.configure(function() {
  app.set('views', __dirname + '/views');
  app.set('view engine', 'ejs');
  app.use(express.logger());
  app.use(express.methodOverride());
  app.use(cookieParser());
  app.use(expressSession({ secret: 'keyboard cat', resave: true, saveUninitialized: false }));
  app.use(bodyParser.urlencoded({ extended : true }));
  // Initialize Passport!  Also use passport.session() middleware, to support
  // persistent login sessions (recommended).
  app.use(passport.initialize());
  app.use(passport.session());
  app.use(app.router);
  app.use(express.static(__dirname + '/../../public'));
});

```

- Schließlich fügen Sie POST-Routen, die aus der eigentlichen Anmeldung Anfragen an hand der `passport-azure-ad` Engine:

```JavaScript

// Our Auth routes (Section 3)

// GET /auth/openid
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  The first step in OpenID authentication will involve redirecting
//   the user to their OpenID provider.  After authenticating, the OpenID
//   provider will redirect the user back to this application at
//   /auth/openid/return

app.get('/auth/openid',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('Authenitcation was called in the Sample');
    res.redirect('/');
  });

// GET /auth/openid/return
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  If authentication fails, the user will be redirected back to the
//   login page.  Otherwise, the primary route function function will be called,
//   which, in this example, will redirect the user to the home page.
app.get('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {

    res.redirect('/');
  });

// POST /auth/openid/return
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  If authentication fails, the user will be redirected back to the
//   login page.  Otherwise, the primary route function function will be called,
//   which, in this example, will redirect the user to the home page.

app.post('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {

    res.redirect('/');
  });
```

## <a name="4-use-passport-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>4. Passport anmelden und Abmelde Anfragen Azure AD erteilen

Ihre app ist jetzt ordnungsgemäß mit der v2. 0 mit dem Authentifizierungsprotokoll OpenID verbinden Kommunikation konfiguriert.  `passport-azure-ad`hat alle hässlich Details Authentifizierungsnachrichten erstellen, Token von Azure AD überprüfen und Verwalten der Sitzung durchgeführt.  Bleibt, den Benutzern ermöglichen, anmelden und Abmelden der Benutzer zusätzliche Informationen sammelt.

- Standard, Anmeldung, Konto und Logout Methoden zum Hinzufügen, können unsere `app.js` Datei:

```JavaScript

//Routes (Section 4)

app.get('/', function(req, res){
  res.render('index', { user: req.user });
});

app.get('/account', ensureAuthenticated, function(req, res){
  res.render('account', { user: req.user });
});

app.get('/login',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('Login was called in the Sample');
    res.redirect('/');
});

app.get('/logout', function(req, res){
  req.logout();
  res.redirect('/');
});

```

-   Betrachten Sie im einzelnen:
    -   Die `/` Route leiten auf index.ejs den Benutzer die Anforderung übergeben (falls vorhanden)
    - Die `/account` Route wird erste ***sicherzustellen, dass wir authentifiziert*** (wir implementieren, unten) und dann den Benutzer in der Anforderung, dass wir zusätzliche Informationen zum Benutzer abrufen können.
    - Die `/login` Weg aufrufen unserer Authentifikator Azuread Openidconnect von `passport-azuread` und gelingt, die wird nicht umleiten des Benutzers auf Login
    - Die `/logout` einfach die logout.ejs aufrufen (und Weiterleiten) Löscht Cookies, den Benutzer dann zum index.ejs zurückzukehren


- Für den letzten Teil des `app.js`, fügen Sie die EnsureAuthenticated-Methode, in `/account` oben.

```JavaScript

// Simple route middleware to ensure user is authenticated. (Section 4)

//   Use this route middleware on any resource that needs to be protected.  If
//   the request is authenticated (typically via a persistent login session),
//   the request will proceed.  Otherwise, the user will be redirected to the
//   login page.
function ensureAuthenticated(req, res, next) {
  if (req.isAuthenticated()) { return next(); }
  res.redirect('/login')
}

```

- Schließlich tatsächlich erstellen den Server selbst in `app.js`:

```JavaScript

app.listen(3000);

```


## <a name="5-create-the-views-and-routes-in-express-to-display-our-user-in-the-website"></a>5. erstellen Sie 5. Ansichten und Routen in Express unsere Benutzer der Website angezeigt

Wir haben unser `app.js` abgeschlossen. Nun müssen wir einfach die Routen und Ansichten, die die Informationen zeigen wir den Benutzer sowie Handle zu den `/logout` und `/login` Routen, die wir erstellt haben.

- Erstellen der `/routes/index.js` Route unter dem Stammverzeichnis.

```JavaScript

/*
 * GET home page.
 */

exports.index = function(req, res){
  res.render('index', { title: 'Express' });
};
```

- Erstellen der `/routes/user.js` Route unter dem Stammverzeichnis

```JavaScript

/*
 * GET users listing.
 */

exports.list = function(req, res){
  res.send("respond with a resource");
};
```

Diese einfachen Routen werden die Anforderung nur unsere Ansichten den Benutzer einschließlich übergeben.

- Erstellen der `/views/index.ejs` anzeigen im Stammverzeichnis. Dies ist eine einfache Seite, die unsere an- und Abmeldung aufrufen und Informationen erfassen lassen. Beachten Sie, dass wir die bedingte können `if (!user)` als Benutzer in der Anforderung durchlaufen Beweisen wir haben Benutzer angemeldet ist.

```JavaScript
<% if (!user) { %>
    <h2>Welcome! Please log in.</h2>
    <a href="/login">Log In</a>
<% } else { %>
    <h2>Hello, <%= user.displayName %>.</h2>
    <a href="/account">Account Info</a></br>
    <a href="/logout">Log Out</a>
<% } %>
```

- Erstellen der `/views/account.ejs` anzeigen im Stammverzeichnis, damit wir weitere Informationen angezeigt werden, die `passport-azuread` hat die Benutzeranfrage.

```Javascript
<% if (!user) { %>
    <h2>Welcome! Please log in.</h2>
    <a href="/login">Log In</a>
<% } else { %>
<p>displayName: <%= user.displayName %></p>
<p>givenName: <%= user.name.givenName %></p>
<p>familyName: <%= user.name.familyName %></p>
<p>UPN: <%= user._json.upn %></p>
<p>Profile ID: <%= user.id %></p>
<p>Full Claimes</p>
<%- JSON.stringify(user) %>
<p></p>
<a href="/logout">Log Out</a>
<% } %>
```

- Abschließend soll dieses Aussehen ziemlich durch Hinzufügen eines Layouts. Erstellen Sie die ' / views/layout.ejs Ansicht im Stammverzeichnis

```HTML

<!DOCTYPE html>
<html>
    <head>
        <title>Passport-OpenID Example</title>
    </head>
    <body>
        <% if (!user) { %>
            <p>
            <a href="/">Home</a> |
            <a href="/login">Log In</a>
            </p>
        <% } else { %>
            <p>
            <a href="/">Home</a> |
            <a href="/account">Account</a> |
            <a href="/logout">Log Out</a>
            </p>
        <% } %>
        <%- body %>
    </body>
</html>
```

Schließlich erstellen und Ausführen der app.

Ausführen `node app.js` und navigieren Sie zu`http://localhost:3000`


Persönlichen Microsoft-Account oder Arbeits- oder Schulcomputer Konto melden Sie an und feststellen Sie, wie die Identität des Benutzers in der Liste AccountType auswirkt.  Sie haben nun eine Web app gesichert Standardprotokolle, die Benutzer mit ihren persönlichen und Arbeit oder Schule Konten authentifizieren können.

##<a name="next-steps"></a>Nächste Schritte

Zu Referenzzwecken können vollständiges Beispiel (ohne Ihre Werte) [als eine .zip bereitgestellt wird](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/complete.zip)oder es von GitHub Klonen:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

Sie können jetzt auf Themen verschieben.  Möglicherweise möchten versuchen:

[Eine node.js Secure web api v2. 0-Endpunkt mit >>](active-directory-v2-devquickstarts-node-api.md)

Weitere Ressourcen zum Auschecken:
- [V2. 0-Entwicklerhandbuch >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow "Azure Active Directory" Tag >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Abrufen von Sicherheitsupdates für unsere Produkte

Wir empfehlen Ihnen Benachrichtigungen Sicherheitsvorfälle auftreten [dieser Seite](https://technet.microsoft.com/security/dd252948) und beratende Sicherheitshinweise abonnieren.
