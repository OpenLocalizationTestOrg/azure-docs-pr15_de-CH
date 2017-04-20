<properties
    pageTitle="Erstellen Sie eine PHP-MySQL Web app in Azure App Service und Bereitstellung mit Git"
    description="Ein Lernprogramm, das veranschaulicht, die Daten in MySQL speichert PHP Web app erstellen und Git Bereitstellung in Azure."
    services="app-service\web"
    documentationCenter="php"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="mysql"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-git"></a>Erstellen Sie eine PHP-MySQL Web app in Azure App Service und Bereitstellung mit Git

Dieses Lernprogramm zeigt Ihnen wie eine PHP-MySQL Web app erstellen und [App](http://go.microsoft.com/fwlink/?LinkId=529714) Service bereitstellen mit Git. Verwenden Sie [PHP][install-php], MySQL-Befehlszeilentool ( [MySQL]Teil[install-mysql]), und [Git] [ install-git] auf Ihrem Computer installiert. Die Anleitung in diesem Lernprogramm können auf jedem Betriebssystem, einschließlich Windows, Mac und Linux folgen. Nach Abschluss dieses Handbuchs, haben Sie eine PHP-MySQL Web app in Azure ausgeführt.

Lernen Sie Folgendes:

* Erstellen einer Webanwendung und einer MySQL-Datenbank mithilfe der [Azure-Portal][management-portal]. Da PHP in [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) standardmäßig aktiviert ist, muss nichts Besonderes PHP-Code ausführen.
* Wie Veröffentlichen und erneut veröffentlichen Sie die Anwendung in Azure Git verwenden.
* Aktivieren der Erweiterung Composer Composer automatisieren Aufgaben an jedem `git push`.

Anhand dieses Lernprogramms erstellen Sie eine einfache Registrierung Web app in PHP. Die Anwendung wird im Web Apps gehostet werden. Screenshot der abgeschlossenen Anwendung lautet wie folgt:

![Azure PHP-Website][running-app]

## <a name="set-up-the-development-environment"></a>Einrichten der Entwicklungsumgebung

In diesem Lernprogramm wird vorausgesetzt, dass [PHP][install-php], MySQL-Befehlszeilentool ( [MySQL]Teil[install-mysql]), und [Git] [ install-git] auf Ihrem Computer installiert.

<a id="create-web-site-and-set-up-git"></a>
## <a name="create-a-web-app-and-set-up-git-publishing"></a>Erstellen einer Webanwendung und Git Veröffentlichung einrichten

Gehen einer Web app und einer MySQL-Datenbank erstellen

1. Anmeldung bei [Azure-Portal][management-portal].
2. Klicken Sie auf das Symbol **neu** .

3. Klicken Sie auf **Alle anzeigen** neben **Marketplace**. 

4. Klicken Sie auf **Web + Mobile**und **Web app + MySQL**. Klicken Sie dann auf **Erstellen**.

4. Geben Sie einen gültigen Namen für die Ressourcengruppe.

    ![Namen festlegen][resource-group]

5. Geben Sie Werte für die neue Web app.

    ![WebApp erstellen][new-web-app]

6. Geben Sie Werte für die neue Datenbank, einschließlich der Geschäftsbedingungen zustimmen.

    ![Erstellen Sie neue MySQL-Datenbank][new-mysql-db]

7. Erstellung die Web-app sehen Sie neue Web app Blade.

7. **Einstellung** **Kontinuierliche**Bereitstellung, klicken auf _Configure Settings erforderlich_.

    ![Git Veröffentlichung einrichten][setup-publishing]

8. Wählen Sie **Lokale Git Repository** für die Quelle.

    ![Git Repository einrichten][setup-repository]


9. Um Git veröffentlichen können, müssen Sie einen Benutzernamen und ein Kennwort angeben. Notieren Sie sich den Benutzernamen und das Kennwort. (Wenn Git Repository vor eingerichtet haben, wird dieser Schritt übersprungen werden.)

    ![Publishing Anmeldeinformationen erstellen][credentials]


## <a name="get-remote-mysql-connection-information"></a>MySQL Informationen abrufen

Verbindung mit MySQL-Datenbank, die in Ihren Web-Apps läuft die Verbindungsinformationen benötigen. Um MySQL Informationen abzurufen, gehen Sie folgendermaßen vor:

1. Klicken Sie auf die Datenbank, aus der Ressourcengruppe:

    ![Datenbank auswählen][select-database]

2. Wählen Sie aus der Datenbank **Einstellungen** **Eigenschaften**.

    ![Klicken Sie auf Eigenschaften][select-properties]

2. Notieren Sie die Werte für `Database`, `Host`, `User Id`, und `Password`.

    ![Hinweis Eigenschaften][note-properties]

## <a name="build-and-test-your-app-locally"></a>Erstellen Sie und Testen Sie Ihre Anwendung lokal

Erstellung eine Webanwendung können Sie die Anwendung lokal entwickeln und dann nach dem Testen bereitzustellen.

Die Anmeldung ist eine einfache PHP-Anwendung, die zum Registrieren eines Ereignisses ermöglicht durch Ihre Namen und e-Mail-Adresse. Informationen zu vorherigen Teilnehmer wird in einer Tabelle angezeigt. Registrierungsinformationen werden in einer Datenbank gespeichert. Die Anwendung besteht aus einer Datei (nachstehend kopieren Code):

* **Index.PHP**: Zeigt ein Formular für die Registrierung und eine Tabelle mit Registrant-Informationen.

Gehen Sie folgendermaßen vor um und führen Sie die Anwendung lokal. Beachten Sie, dass diese Schritte die PHP und MySQL-Befehlszeilentool (Teil von MySQL) auf dem lokalen Computer eingerichtet und Aktivierung [PDO-Erweiterung für MySQL][pdo-mysql].

1. Verbindung zum remote MySQL-Server mit dem Wert für `Data Source`, `User Id`, `Password`, und `Database` , die zuvor abgerufen:

        mysql -h{Data Source] -u[User Id] -p[Password] -D[Database]

2. Der MySQL-Befehlszeile wird angezeigt:

        mysql>

3. Fügen Sie in den folgenden `CREATE TABLE` Befehl zum Erstellen der `registration_tbl` Tabelle in der Datenbank:

        CREATE TABLE registration_tbl(id INT NOT NULL AUTO_INCREMENT, PRIMARY KEY(id), name VARCHAR(30), email VARCHAR(30), date DATE);

4. Erstellen Sie im Stammverzeichnis der lokalen Anwendungsordner **index.php** Datei.

5. Die **index.php** -Datei in einem Text-Editor oder IDE und fügen Sie folgenden Code mit markierten Änderungen abgeschlossen `//TODO:` Kommentare.


        <html>
        <head>
        <Title>Registration Form</Title>
        <style type="text/css">
            body { background-color: #fff; border-top: solid 10px #000;
                color: #333; font-size: .85em; margin: 20; padding: 20;
                font-family: "Segoe UI", Verdana, Helvetica, Sans-Serif;
            }
            h1, h2, h3,{ color: #000; margin-bottom: 0; padding-bottom: 0; }
            h1 { font-size: 2em; }
            h2 { font-size: 1.75em; }
            h3 { font-size: 1.2em; }
            table { margin-top: 0.75em; }
            th { font-size: 1.2em; text-align: left; border: none; padding-left: 0; }
            td { padding: 0.25em 2em 0.25em 0em; border: 0 none; }
        </style>
        </head>
        <body>
        <h1>Register here!</h1>
        <p>Fill in your name and email address, then click <strong>Submit</strong> to register.</p>
        <form method="post" action="index.php" enctype="multipart/form-data" >
              Name  <input type="text" name="name" id="name"/></br>
              Email <input type="text" name="email" id="email"/></br>
              <input type="submit" name="submit" value="Submit" />
        </form>
        <?php
            // DB connection info
            //TODO: Update the values for $host, $user, $pwd, and $db
            //using the values you retrieved earlier from the Azure Portal.
            $host = "value of Data Source";
            $user = "value of User Id";
            $pwd = "value of Password";
            $db = "value of Database";
            // Connect to database.
            try {
                $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
                $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            }
            catch(Exception $e){
                die(var_dump($e));
            }
            // Insert registration info
            if(!empty($_POST)) {
            try {
                $name = $_POST['name'];
                $email = $_POST['email'];
                $date = date("Y-m-d");
                // Insert data
                $sql_insert = "INSERT INTO registration_tbl (name, email, date)
                           VALUES (?,?,?)";
                $stmt = $conn->prepare($sql_insert);
                $stmt->bindValue(1, $name);
                $stmt->bindValue(2, $email);
                $stmt->bindValue(3, $date);
                $stmt->execute();
            }
            catch(Exception $e) {
                die(var_dump($e));
            }
            echo "<h3>Your're registered!</h3>";
            }
            // Retrieve data
            $sql_select = "SELECT * FROM registration_tbl";
            $stmt = $conn->query($sql_select);
            $registrants = $stmt->fetchAll();
            if(count($registrants) > 0) {
                echo "<h2>People who are registered:</h2>";
                echo "<table>";
                echo "<tr><th>Name</th>";
                echo "<th>Email</th>";
                echo "<th>Date</th></tr>";
                foreach($registrants as $registrant) {
                    echo "<tr><td>".$registrant['name']."</td>";
                    echo "<td>".$registrant['email']."</td>";
                    echo "<td>".$registrant['date']."</td></tr>";
                }
                echo "</table>";
            } else {
                echo "<h3>No one is currently registered.</h3>";
            }
        ?>
        </body>
        </html>

4.  In einem Terminal Anwendungsordner und geben Sie folgenden Befehl:

        php -S localhost:8000

Jetzt können Sie durchsuchen **http://localhost: 8000 /** zum Testen der Anwendung.


## <a name="publish-your-app"></a>Veröffentlichen Sie Ihre app

Nachdem Sie Ihre Anwendung lokal getestet haben, können Sie sie Web-Apps mit Git veröffentlichen. Sie initialisieren das lokale Git Repository und veröffentlichen Sie die Anwendung.

> [AZURE.NOTE]
> Dies sind die gleichen Schritte im Azure-Portal am Ende des Web app erstellen und veröffentlichen Git Abschnitt oben angezeigt.

1. (Optional)  Wenn Sie vergessen oder verlegt den Git remote zusammen URL, navigieren Sie zu Web app Eigenschaften Azure-Portal.

1. GitBash öffnen (oder ein Terminal Git ist in der `PATH`), wechseln Sie zum Stammverzeichnis der Anwendung, und führen Sie die folgenden Befehle:

        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master

    Sie werden für das Kennwort aufgefordert, die Sie zuvor erstellt haben.

    ![Anstoß zu Azure über Git][git-initial-push]

2. Navigieren Sie zu **http://[site name].azurewebsites.net/index.php** verwendet werden (diese Informationen werden auf dem Konto Dashboard gespeichert) Anwendung:

    ![Azure PHP-Website][running-app]

Veröffentlichung Ihrer Anwendung können Sie die Änderung beginnen und Git veröffentlichen verwenden.

## <a name="publish-changes-to-your-app"></a>Ändert Ihre app veröffentlichen

Gehen Sie folgendermaßen vor, um Änderungen für Ihre Anwendung zu veröffentlichen:

1. Ändern Sie Ihre Anwendung lokal.
2. GitBash öffnen (oder ein Terminal Git ist in der `PATH`), wechseln Sie zum Stammverzeichnis der Anwendung, und führen Sie die folgenden Befehle:

        git add .
        git commit -m "comment describing changes"
        git push azure master

    Sie werden für das Kennwort aufgefordert, die Sie zuvor erstellt haben.

    ![Seite in Azure über Git drücken][git-change-push]

3. Navigieren Sie zu **http://[site name].azurewebsites.net/index.php** app und Änderungen, die Sie erstellt haben:

    ![Azure PHP-Website][running-app]

>[AZURE.NOTE] Wenn Sie mit Azure App Service beginnen, bevor Sie sich für ein Azure-Konto, gehen Sie [Versuchen App Service](http://go.microsoft.com/fwlink/?LinkId=523751)sofort eine kurzlebige Starter Web app in App Service können Sie erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

<a name="composer"></a>
## <a name="enable-composer-automation-with-the-composer-extension"></a>Composer Automatisierung mit Composer-Erweiterung

Standardmäßig Git Bereitstellungsprozess in App Service mit composer.json, nicht in Ihrem PHP-Projekt haben. Sie können im composer.json `git push` Composer-Erweiterung aktivieren.

1. In PHP web app Blade in [Azure-Portal][management-portal], klicken Sie auf **Extras** > **Extensions**.

    ![Composer Erweiterung Settings][composer-extension-settings]

2. Klicken Sie auf **Hinzufügen**und dann auf **Composer**.

    ![Composer-Erweiterung][composer-extension-add]
    
3. Klicken Sie auf **OK** , um die Geschäftsbedingungen zu akzeptieren. Klicken Sie auf **OK** , um die Erweiterung.

    **Installierte Erweiterung** Blade zeigt nun die Composer-Erweiterung.  
    ![Composer Erweiterung anzeigen][composer-extension-view]
    
4. Nun, `git add`, `git commit`, und `git push` wie im vorherigen Abschnitt. Sie sehen jetzt, dass Composer Abhängigkeiten definiert im composer.json installiert.

    ![Composer Erweiterung Erfolg][composer-extension-success]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der [PHP-Entwicklercenter](/develop/php/).

<!-- URL List -->

[install-php]: http://www.php.net/manual/en/install.php
[install-SQLExpress]: http://www.microsoft.com/download/details.aspx?id=29062
[install-Drivers]: http://www.microsoft.com/download/details.aspx?id=20098
[install-git]: http://git-scm.com/
[install-mysql]: http://dev.mysql.com/downloads/mysql/
[pdo-mysql]: http://www.php.net/manual/en/ref.pdo-mysql.php
[management-portal]: https://portal.azure.com
[sql-database-editions]: http://msdn.microsoft.com/library/windowsazure/ee621788.aspx

<!-- IMG List -->

[running-app]: ./media/web-sites-php-mysql-deploy-use-git/running_app_2.png
[new-website]: ./media/web-sites-php-mysql-deploy-use-git/new_website2.png
[custom-create]: ./media/web-sites-php-mysql-deploy-use-git/create_web_mysql.png
[new-mysql-db]: ./media/web-sites-php-mysql-deploy-use-git/create_db.png
[go-to-webapp]: ./media/web-sites-php-mysql-deploy-use-git/select_webapp.png
[setup-git-publishing]: ./media/web-sites-php-mysql-deploy-use-git/setup_git_publishing.png
[credentials]: ./media/web-sites-php-mysql-deploy-use-git/save_credentials.png
[resource-group]: ./media/web-sites-php-mysql-deploy-use-git/set_group.png
[new-web-app]: ./media/web-sites-php-mysql-deploy-use-git/create_wa.png
[setup-publishing]: ./media/web-sites-php-mysql-deploy-use-git/setup_deploy.png
[setup-repository]: ./media/web-sites-php-mysql-deploy-use-git/select_local_git.png
[select-database]: ./media/web-sites-php-mysql-deploy-use-git/select_database.png
[select-properties]: ./media/web-sites-php-mysql-deploy-use-git/select_properties.png
[note-properties]: ./media/web-sites-php-mysql-deploy-use-git/note-properties.png

[git-instructions]: ./media/web-sites-php-mysql-deploy-use-git/git-instructions.png
[git-change-push]: ./media/web-sites-php-mysql-deploy-use-git/php-git-change-push.png
[git-initial-push]: ./media/web-sites-php-mysql-deploy-use-git/php-git-initial-push.png

[composer-extension-settings]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-settings.png
[composer-extension-add]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-add.png
[composer-extension-view]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-view.png
[composer-extension-success]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-success.png
