Die meisten der Authentifizierungsfehler entstehen, falsch oder inkonsistent Konfigurationen. Hier sind einige Vorschläge für zu überprüfen.

* Stellen Sie sicher, dass **überall das Speichern** verpasst haben. Dies ist oft einfach und das Ergebnis ist, dass Sie die richtigen Werte auf einer Portalseite suchen werden jedoch nicht tatsächlich in der Azure-Umgebung oder Azure AD-Anwendung gespeichert wurden.
* Einstellungen **Einstellungen** Blatt der Azure-Portal stellen Sie sicher, dass die richtigen API-app oder WebApp beim eingegebenen Einstellungen ausgewählt wurde.  Außerdem sicherstellen Sie, dass die Einstellung **Einstellungen App** und keine **Verbindungszeichenfolgen**eingegebenen Format zwei Abschnitten vergleichbar ist.
* Für die Authentifizierung mit einem JavaScript-front-End, downloaden das Manifest erneut, um sicherzustellen, dass `oauth2AllowImplicitFlow` wurde erfolgreich in `true`.
* Stellen Sie sicher, dass HTTPS verwendet, wo Sie URLs konfiguriert:

    * Im Projektcode
    * In CORS
    * Azure-Umgebung App Einstellungen für jede API oder WebApp
    * In Azure AD Einstellungen.
    
    Beachten Sie, dass beim Kopieren einer API-app-URL aus dem Portal es oft `http://` und manuell zu ändern, `https://`.

* Stellen Sie sicher, dass codeänderungen erfolgreich bereitgestellt wurden. Beispielsweise kann in einer Projektmappe mehrere Projekte ein Projekt Code ändern und einen anderen versehentlich auszuwählen, wenn Sie die Änderung bereitstellen möchten.
* Stellen Sie sicher, dass Sie auf HTTPS-URLs im Browser nicht HTTP-URLs. Visual Studio erstellt standardmäßig Profile mit HTTP-URLs, und was im Browser geöffnet, nachdem Sie ein Projekt bereitstellen.
* Für die Authentifizierung mit einem JavaScript-front-End unbedingt CORS API-Anwendung, die der JavaScript-Code aufruft, ordnungsgemäß konfiguriert ist. Bei Zweifeln, ob das Problem im Zusammenhang mit CORS versuchen Sie "*" als zulässige Ursprungs-URL. 
* Ein JavaScript-front-End des Browsers Developer Toolskonsole Registerkarte um weitere Informationen zu erhalten und HTTP-Anfragen im Netzwerk überprüfen. Konsole Fehlermeldungen können jedoch irreführend sein. Wenn Sie eine Meldung mit eines CORS Fehlers erhalten, möglicherweise das wahre Problem Authentifizierung. Sie können überprüfen, geschieht dies durch Ausführen der Anwendung mit Authentifizierung vorübergehend vorübergehend deaktiviert.
* Stellen Sie für eine Anwendung .NET API sicher, dass Sie so viele Informationen in Fehlermeldungen möglichst [CustomErrors Mode auf Off](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remoteview)festlegen.
* Für eine Anwendung .NET API [remote debugging-Sitzung](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug)starten Sie, und überprüfen Sie die Werte der Variablen an Code zum ADAL trägertoken abzurufen oder Code, Ansprüche erwartete Service principal ID überprüft Beachten Sie, dass Code Werte aus vielen verschiedenen Quellen abholen kann so überrascht so finden kann. Beispielsweise, wenn Sie falsch `ida:ClientId` als `ida:ClientID` beim Azure App Service-umgebungseinstellungen konfigurieren, kann der Code werden die `ida:ClientId` Wert aus der Web.config-Datei ignoriert die Einstellung Azure App Service gesucht. 
* Wenn Elemente in einem normalen Fenster von Internet Explorer nicht kann eine vorhandene anmelden beeinträchtigen; InPrivate und versuchen Sie Chrome oder Firefox.
