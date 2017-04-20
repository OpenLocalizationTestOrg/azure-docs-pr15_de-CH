<properties
    pageTitle="Erstellen und Verbinden mit einer MySQL-Datenbank in Azure"
    description="Informationen Sie zum Verwenden des Azure-Portals eine MySQL-Datenbank erstellen und dann eine PHP Web App in Azure verbinden."
    documentationCenter="php"
    services="app-service\web"
    authors="cephalin"
    manager="wpickett"
    editor=""
    tags="mysql"/>

<tags
    ms.service="multiple"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm;cephalin"/>

# <a name="create-and-connect-to-a-mysql-database-in-azure"></a>Erstellen und Verbinden mit einer MySQL-Datenbank in Azure

In diesem Lernprogramm wird veranschaulicht, wie eine MySQL-Datenbank in [Azure-Portal](https://portal.azure.com) erstellen (Anbieter ist [ClearDB](http://www.cleardb.com/)) und von PHP Web-app im [Azure App-Dienst](./app-service/app-service-value-prop-what-is.md)herstellen. 

> [AZURE.NOTE] Sie können auch eine MySQL-Datenbank als Teil einer [Vorlage der Marketplace-app](./app-service-web/app-service-web-create-web-app-from-marketplace.md)erstellen.

## <a name="create-a-mysql-database-in-azure-portal"></a>Erstellen Sie eine MySQL-Datenbank in Azure-portal

Um eine MySQL-Datenbank in Azure-Portal zu erstellen, führen Sie folgende Schritte aus:

1. Auf der [Azure-Portal](https://portal.azure.com)anmelden.

2. Klicken Sie im linken Menü auf **neu** > **Daten + Speicher** > **MySQL-Datenbank**.

    ![Erstellen Sie eine MySQL-Datenbank in Azure - start](./media/store-php-create-mysql-database/create-db-1-start.png)

2. Konfigurieren Sie neue MySQL-Datenbank [Blade](azure-portal-overview.md)Ihre MySQL-Datenbank wie folgt (*Blade*: eine horizontal öffnet Portalseite):

    - **Datenbankname**: Geben Sie eindeutig identifizierbar
    - **Abonnement**: Wählen Sie das Abonnement verwenden
    - **Typ**: **Shared** für kostengünstige oder freien Ebenen oder **dedizierte** Ressourcen auswählen. 
    - **Gruppe**: eine vorhandene [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) hinzufügen die MySQL-Datenbank oder in eine neue. Ressourcen in der gleichen Gruppe können problemlos gemeinsam verwaltet werden.
    - **Ort**: Wählen Sie einen Speicherort. Wenn eine vorhandene Ressourcengruppe hinzufügen, sind Sie der Ressourcengruppe an gesperrt.
    - **Preise Tier**: **Tier Preise**klicken Sie Preise Option (**Mercury** Tier ist), und klicken Sie dann auf **auswählen**. 
    - **Geschäftsbedingungen**: auf **Rechtlich**, Details zu Bestellung und klicken Sie auf **kaufen**.
    - **PIN Dashboard**: Wählen Sie, wenn Sie direkt aus dem Dashboard zugreifen möchten. Dies ist besonders hilfreich, wenn Sie mit Portalnavigation noch nicht vertraut.
    
    Der folgende Screenshot ist nur ein Beispiel, wie Sie Ihre MySQL-Datenbank konfigurieren können.  
    ![Erstellen einer MySQL-Datenbank in Azure - konfigurieren](./media/store-php-create-mysql-database/create-db-2-configure.png)

3. Sie konfigurieren, klicken Sie auf **Erstellen**.

    ![Eine MySQL-Datenbank in Azure erstellen-](./media/store-php-create-mysql-database/create-db-3-create.png)

    Sie sehen Popupbenachrichtigung lassen Sie wissen, dass die Bereitstellung gestartet wurde.

    ![Erstellen Sie eine MySQL-Datenbank in Azure - gerade](./media/store-php-create-mysql-database/create-db-4-started-status.png)

    Eine andere erhalten Popup Sie nach der Bereitstellung erfolgreich war. Das Portal auch Öffnet Ihre MySQL-Datenbank Blade.

<a name="connect"></a>
## <a name="connect-to-your-mysql-database"></a>Ihre MySQL-Datenbank verbinden

Klicken Sie die Verbindungsinformationen für die neue MySQL-Datenbank nur **Eigenschaften** im Web app Blade.
    
![Erstellen Sie eine MySQL-Datenbank in Azure - Blatt der MySQL-Datenbank](./media/store-php-create-mysql-database/create-db-5-finished-db-blade.png)

Jetzt können die Verbindungsinformationen in jede Webanwendung. Ein Beispiel, das zeigt, wie Verbindungsinformationen aus einer einfachen PHP-Anwendung steht [hier](https://github.com/WindowsAzure/azure-sdk-for-php-samples/tree/master/tasklist-mysql).

## <a name="connect-a-laravel-web-app-from-the-php-get-started-tutorial"></a>Herstellen einer Laravel Web app (PHP Get gestartet Tutorial)

Angenommen Sie, gerade Lernprogramm [erstellen, konfigurieren und Bereitstellen einer PHP Web app in Azure](./app-service-web/app-service-web-php-get-started.md) und haben Sie [Laravel](https://www.laravel.com/) Web-app in Azure ausgeführt. Sie können Ihrer Anwendung Laravel leicht Datenbankfunktionen hinzufügen. Befolgen Sie die folgenden Schritte aus:

>[AZURE.NOTE] Die folgenden Schritte gehen Lernprogramm [erstellen, konfigurieren und Bereitstellen einer PHP Web app in Azure](./app-service-web/app-service-web-php-get-started.md)haben.

1. Konfigurieren Sie die Laravel-Anwendung in der lokalen Umgebung auf der MySQL-Datenbank. Öffnen Sie hierzu `.env` Laravel app Stammverzeichnis und MySQL-Datenbankoptionen konfigurieren.

        DB_CONNECTION=mysql
        DB_HOST=<HOSTNAME_from_properties_blade>
        DB_PORT=<PORT_from_properties_blade>
        DB_DATABASE=<see_note_below>
        DB_USERNAME=<USERNAME_from_properties_blade>
        DB_PASSWORD=<PASSWORD_from_properties_blade>

    >[AZURE.NOTE] Der Name der MySQL-Datenbank Blatt **Eigenschaften** möglicherweise oder möglicherweise nicht im Feld **Datenbankname** . Es ist besser, der Datenbankparameter im Feld **VERBINDUNGSZEICHENFOLGE** . 
    >
    >![Erstellen Sie eine MySQL-Datenbank in Azure - gerade](./media/store-php-create-mysql-database/connect-db-1-database-name.png)

2. Überprüfen Sie MySQL Zugriff jetzt am schnellsten werden [Laravels Standard-Authentifizierung Gerüstbau](https://laravel.com/docs/5.2/authentication#authentication-quickstart)verwendet. Führen Sie folgende Befehle Laravel app Stammverzeichnis, Befehlszeile Terminal:

         php artisan migrate
         php artisan make:auth

    Der erste Befehl erstellt die Tabellen in Azure basierend auf vordefinierten Migrationen in die `database/migrations` Verzeichnis und der zweite Befehl Gerüste Ansichten und Routen für Registrierung und Authentifizierung.

3. Den Entwicklungsserver jetzt ausführen:

        php artisan serve

4. Navigieren Sie zu http://localhost: 8000 im Browser und registrieren Sie einen neuen Benutzer, wie dargestellt:

    ![MySQL-Datenbank in Azure Connect - Benutzer registrieren](./media/store-php-create-mysql-database/connect-db-2-development-server.png)

    Führen Sie die Registrierung abgeschlossen Abfragen über die Benutzeroberfläche. Nach Abschluss der Registrierung werden Sie angemeldet.
    
    ![MySQL-Datenbank in Azure Connect - Benutzer registrieren](./media/store-php-create-mysql-database/connect-db-3-registered-user.png)

    Entwickeln Sie jetzt Ihre app für die MySQL-Datenbank in Azure.

5. Nun, Sie einfach replizieren müssen Ihre `.env` Einstellungen Ihrer Azure Anwendung. Führen Sie die folgenden Azure-CLI-Befehle:

        azure site appsetting add DB_CONNECTION=mysql
        azure site appsetting add DB_HOST=<HOSTNAME_from_properties_blade>
        azure site appsetting add DB_PORT=<PORT_from_properties_blade>
        azure site appsetting add DB_DATABASE=<Database_param_from_CONNECTION_INFO_from_properties_blade>
        azure site appsetting add DB_USERNAME=<USERNAME_from_properties_blade>
        azure site appsetting add DB_PASSWORD=<PASSWORD_from_properties_blade>

    Erfahren Sie, wie das [Konfigurieren der Azure-Web-Anwendung](./app-service-web/app-service-web-php-get-started.md#configure)funktioniert.

6. Commit, und lokalen Änderungen bereits während der Ausführung in Azure push `php artisan make:auth`.

        git add .
        git commit -m "scaffold auth views and routes"
        git push azure master

7. Wechseln Sie zum Azure Web app.

        azure site browse

8. Melden Sie sich mit den Anmeldeinformationen, die Sie zuvor erstellt haben.

    ![Verbindung mit MySQL-Datenbank in Azure - Azure Web App durchsuchen](./media/store-php-create-mysql-database/connect-db-4-browse-azure-webapp.png)

    Nach der Anmeldung sollte der Bildschirm nach der Anmeldung angezeigt werden.
    
    ![Verbinden Sie mit MySQL-Datenbank in Azure - angemeldet](./media/store-php-create-mysql-database/connect-db-5-logged-in.png)

    Herzlichen Glückwunsch, Ihrer Anwendung in Azure PHP ist jetzt Zugriff auf Daten aus einer MySQL-Datenbank. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der [PHP-Entwicklercenter](/develop/php/).
