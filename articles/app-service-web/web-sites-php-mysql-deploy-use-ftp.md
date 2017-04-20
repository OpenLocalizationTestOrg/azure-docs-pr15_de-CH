<properties 
    pageTitle="Erstellen Sie eine PHP-MySQL Web app in Azure App Service und mithilfe von FTP" 
    description="Ein Lernprogramm, das veranschaulicht, wie eine PHP Web app erstellen, die Daten in MySQL und mit FTP-Bereitstellung in Azure speichert." 
    services="app-service\web" 
    documentationCenter="php" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>


#<a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-ftp"></a>Erstellen Sie eine PHP-MySQL Web app in Azure App Service und mithilfe von FTP

Dieses Lernprogramm zeigt Ihnen, wie eine PHP-MySQL Web app erstellen und mithilfe von FTP bereitstellen. In diesem Lernprogramm wird vorausgesetzt, dass [PHP][install-php], [MySQL][install-mysql], einen Webserver und einen FTP-Client auf Ihrem Computer installiert. Die Anleitung in diesem Lernprogramm können auf jedem Betriebssystem, einschließlich Windows, Mac und Linux folgen. Nach Abschluss dieses Handbuchs, haben Sie eine PHP-MySQL Web app in Azure ausgeführt.
 
Lernen Sie Folgendes:

* Eine Web-app und einer MySQL-Datenbank mithilfe der Azure-Portal erstellen Da PHP im Web Apps standardmäßig aktiviert ist, muss nichts Besonderes PHP-Code ausführen.
* Wie Sie Ihre Anwendung in Azure mithilfe von FTP veröffentlichen.
 
Anhand dieses Lernprogramms erstellen Sie eine einfache Registrierung Web app in PHP. Die Anwendung wird in einem Web App gehostet. Screenshot der abgeschlossenen Anwendung lautet wie folgt:

![Azure PHP-Website][running-app]

>[AZURE.NOTE] Wenn Sie mit Azure App Service beginnen, bevor Sie sich für ein Konto, gehen Sie [Versuchen App Service](http://go.microsoft.com/fwlink/?LinkId=523751)sofort eine kurzlebige Starter Web app in App Service können Sie erstellen. Keine Kreditkarten erforderlich, keine Zusagen. 


##<a name="create-a-web-app-and-set-up-ftp-publishing"></a>Erstellen einer Webanwendung und FTP-Veröffentlichung einrichten

Gehen einer Web app und einer MySQL-Datenbank erstellen

1. Anmeldung bei [Azure-Portal][management-portal].
2. Azure-Portal **+ Neuer** Symbol oben links klicken.

    ![Erstellen Sie neue Azure-Website][new-website]

3. Geben Sie im Bereich **WebApp + MySQL** und **WebApp**+ MySQL auf.

    ![Benutzerdefinierte neue Website erstellen][custom-create]

4. Klicken Sie auf **Erstellen**. Geben Sie einen eindeutigen app-Dienstnamen, einen gültigen Namen für die Ressourcengruppe und neue Service-Plan.

    ![Namen festlegen][resource-group]


6. Geben Sie Werte für die neue Datenbank, einschließlich der Geschäftsbedingungen zustimmen.

    ![Erstellen Sie neue MySQL-Datenbank][new-mysql-db]
    
7. Erstellung die Web-app sehen Sie das neuen app Blatt.


6. Klicken Sie **auf** > **Bereitstellung Anmeldeinformationen**. 

    ![Bereitstellung Anmeldeinformationen festlegen][set-deployment-credentials]

7. Um FTP-Veröffentlichung zu aktivieren, müssen Sie einen Benutzernamen und ein Kennwort angeben. Speichern der Anmeldeinformationen, und notieren Sie den Benutzernamen und das Kennwort.

    ![Publishing Anmeldeinformationen erstellen][portal-ftp-username-password]

##<a name="build-and-test-your-app-locally"></a>Erstellen Sie und Testen Sie Ihre Anwendung lokal

Die Anmeldung ist eine einfache PHP-Anwendung, die zum Registrieren eines Ereignisses ermöglicht durch Ihre Namen und e-Mail-Adresse. Informationen zu vorherigen Teilnehmer wird in einer Tabelle angezeigt. Registrierungsinformationen werden in einer Datenbank gespeichert. Die Anwendung besteht aus zwei Dateien:

* **Index.PHP**: Zeigt ein Formular für die Registrierung und eine Tabelle mit Registrant-Informationen.
* **CreateTable.PHP**: die MySQL-Tabelle für die Anwendung erstellt. Diese Datei wird nur einmal verwendet werden.

Gehen Sie folgendermaßen vor um und führen Sie die Anwendung lokal. Hinweis Diese Schritte setzen voraus, Sie MySQL, PHP und ein Webserver auf Ihrem lokalen Computer, einrichten konnten die [PDO-Erweiterung für MySQL][pdo-mysql].

1. Erstellen Sie eine MySQL-Datenbank namens `registration`. Hierzu können Sie in der MySQL-Befehlszeile mit dem folgenden Befehl:

        mysql> create database registration;

2. Erstellen Sie im Stammverzeichnis des Webservers einen Ordner namens `registration` und erstellen zwei Dateien - aufgerufen `createtable.php` und eine mit der Bezeichnung `index.php`.

3. Öffnen der `createtable.php` in einem Text-Editor oder IDE-Datei und fügen Sie folgenden Code hinzu. Dieser Code wird zum Erstellen der `registration_tbl` Tabelle die `registration` Datenbank.

        <?php
        // DB connection info
        $host = "localhost";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        try{
            $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            $sql = "CREATE TABLE registration_tbl(
                        id INT NOT NULL AUTO_INCREMENT, 
                        PRIMARY KEY(id),
                        name VARCHAR(30),
                        email VARCHAR(30),
                        date DATE)";
            $conn->query($sql);
        }
        catch(Exception $e){
            die(print_r($e));
        }
        echo "<h3>Table created.</h3>";
        ?>

    > [AZURE.NOTE] 
    > Sie müssen zum Aktualisieren der Werte für <code>$user</code> und <code>$pwd</code> mit Ihrem lokalen MySQL-Benutzernamen und Kennwort.

4. Öffnen Sie einen Webbrowser, und navigieren Sie zu [http://localhost/registration/createtable.php][localhost-createtable]. Dadurch wird die `registration_tbl` Tabelle in der Datenbank.

5. Die **index.php** -Datei in einem Text-Editor oder IDE und grundlegenden HTML- und CSS-Code für die Seite (PHP-Code werden später hinzugefügt) hinzufügen.

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

        ?>
        </body>
        </html>

6. Fügen Sie in PHP-Tags PHP-Code für die Verbindung mit der Datenbank hinzu.

        // DB connection info
        $host = "localhost";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        // Connect to database.
        try {
            $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
        }
        catch(Exception $e){
            die(var_dump($e));
        }

    > [AZURE.NOTE]
    > Wieder müssen zum Aktualisieren der Werte für <code>$user</code> und <code>$pwd</code> mit Ihrem lokalen MySQL-Benutzernamen und Kennwort.

7. Fügen Sie folgenden Code Verbindung Datenbank Code für Registrierungsdaten in die Datenbank einfügen.

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

8. Schließlich nach obigem Code fügen Sie Code zum Abrufen von Daten aus der Datenbank.

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

Jetzt können Sie durchsuchen, [http://localhost/registration/index.php] [ localhost-index] die Anwendung testen.

##<a name="get-mysql-and-ftp-connection-information"></a>Erhalten Sie MySQL und FTP-Verbindung

Verbindung mit MySQL-Datenbank, die in Ihren Web-Apps läuft die Verbindungsinformationen benötigen. Um MySQL Informationen abzurufen, gehen Sie folgendermaßen vor:

1. App Service Blatt Web app klicken Sie auf den Link Ressource:

    ![Ressourcengruppe auswählen][select-resourcegroup]

1. Klicken Sie auf die Datenbank, aus der Ressourcengruppe:

    ![Datenbank auswählen][select-database]

2. **Aus der Zusammenfassung Datenbank Einstellungen** > **Eigenschaften**.

    ![Klicken Sie auf Eigenschaften][select-properties]
    
2. Notieren Sie die Werte für `Database`, `Host`, `User Id`, und `Password`.

    ![Hinweis Eigenschaften][note-properties]

3. Ihrer Anwendung klicken Sie auf den Link **Download Profil veröffentlichen** in der unteren rechten Ecke der Seite:

    ![Download Profil veröffentlichen][download-publish-profile]

4. Öffnen der `.publishsettings` -Datei im XML-Editor. 

3. Suchen der `<publishProfile >` mit `publishMethod="FTP"` , die wie folgt aussehen:

        <publishProfile publishMethod="FTP" publishUrl="ftp://[mysite].azurewebsites.net/site/wwwroot" ftpPassiveMode="True" userName="[username]" userPWD="[password]" destinationAppUrl="http://[name].antdf0.antares-test.windows-int.net" 
            ...
        </publishProfile>
    
Notieren Sie die `publishUrl`, `userName`, und `userPWD` Attribute.

##<a name="publish-your-app"></a>Veröffentlichen Sie Ihre app

Nachdem Sie Ihre Anwendung lokal getestet haben, können Sie sie Ihrer Anwendung mithilfe von FTP veröffentlichen. Allerdings müssen Sie die Datenbank-Verbindungsinformationen in der Anwendung aktualisieren. Die folgenden Informationen in **sowohl** aktualisieren die Datenbankverbindungsinformationen Sie erhalten früher (unter **MySQL erhalten und Informationen zur FTP-Verbindung** ), mit dem `createdatabase.php` und `index.php` Dateien mit den entsprechenden Werten:

    // DB connection info
    $host = "value of Data Source";
    $user = "value of User Id";
    $pwd = "value of Password";
    $db = "value of Database";

Nun können Sie mithilfe von FTP veröffentlichen.

1. Öffnen Sie Ihre FTP-Client Wahl.

2. Geben Sie den *Hostnamenteil* aus der `publishUrl` Attribut in Ihrem FTP-Client oben.

3. Geben Sie die `userName` und `userPWD` oben aufgeführten Attribute unverändert in der FTP-Client.

4. Eine Verbindung herstellen.

Nachdem Sie verbunden werden Sie möglicherweise zum Uploaden und Downloaden von Dateien nach Bedarf. Sicher, dass Sie Dateien in das Stammverzeichnis Hochladen ist `/site/wwwroot`.

Nach dem Hochladen beide `index.php` und `createtable.php`, **http://[site name].azurewebsites.net/createtable.php** , MySQL-Tabelle für die Anwendung erstellen und navigieren Sie zu **http://[site name].azurewebsites.net/index.php** verwenden die Anwendung suchen.
 
## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der [PHP-Entwicklercenter](/develop/php/).

[install-php]: http://www.php.net/manual/en/install.php
[install-mysql]: http://dev.mysql.com/doc/refman/5.6/en/installing.html
[pdo-mysql]: http://www.php.net/manual/en/ref.pdo-mysql.php
[localhost-createtable]: http://localhost/tasklist/createtable.php
[localhost-index]: http://localhost/tasklist/index.php
[running-app]: ./media/web-sites-php-mysql-deploy-use-ftp/running_app_2.png
[new-website]: ./media/web-sites-php-mysql-deploy-use-ftp/new_website2.png
[custom-create]: ./media/web-sites-php-mysql-deploy-use-ftp/create_web_mysql.png
[website-details]: ./media/web-sites-php-web-site-mysql-deploy-use-ftp/website_details.jpg
[new-mysql-db]: ./media/web-sites-php-mysql-deploy-use-ftp/create_db.png
[go-to-webapp]: ./media/web-sites-php-mysql-deploy-use-ftp/select_webapp.png
[set-deployment-credentials]: ./media/web-sites-php-mysql-deploy-use-ftp/set_credentials.png
[portal-ftp-username-password]: ./media/web-sites-php-mysql-deploy-use-ftp/save_credentials.png
[resource-group]: ./media/web-sites-php-mysql-deploy-use-ftp/set_group.png
[new-web-app]: ./media/web-sites-php-mysql-deploy-use-ftp/create_wa.png
[select-database]: ./media/web-sites-php-mysql-deploy-use-ftp/select_database.png
[select-resourcegroup]: ./media/web-sites-php-mysql-deploy-use-ftp/select_resourcegroup.png
[select-properties]: ./media/web-sites-php-mysql-deploy-use-ftp/select_properties.png
[note-properties]: ./media/web-sites-php-mysql-deploy-use-ftp/note-properties.png

[connection-string-info]: ./media/web-sites-php-web-site-mysql-deploy-use-ftp/connection_string_info.png
[management-portal]: https://portal.azure.com
[download-publish-profile]: ./media/web-sites-php-mysql-deploy-use-ftp/download_publish_profile_3.png
 
