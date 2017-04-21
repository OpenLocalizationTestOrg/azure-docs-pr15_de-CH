Azure-Markt wird eine Vielzahl von gängigen webapps entwickelt von Microsoft, Firmen und open-Source-Software Initiativen. Webapps erstellt Azure Marketplace erfordern keine Installation von Software, die als Verbindung zu [Azure Vorschauportal](http://go.microsoft.com/fwlink/?LinkId=529715)verwendeten Browser. 

In diesem Lernprogramm erfahren Sie:

- Erstellen eine neuen Web-Applikation über Azure Marketplace.

- Zum Web app Azure Vorschau Portal bereitstellen.
 
Sie erstellen einen Blog WordPress, der Standardvorlage verwendet. Die folgende Abbildung zeigt die fertige Anwendung:


![WordPress-blog][13]

>[AZURE.NOTE] Wenn Sie mit Azure App Service beginnen, bevor Sie sich für ein Azure-Konto, gehen Sie [Versuchen App Service](http://go.microsoft.com/fwlink/?LinkId=523751)sofort eine kurzlebige Starter Web app in App Service können Sie erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="create-a-web-app-in-the-portal"></a>Erstellen Sie eine Webanwendung im portal

1. Der Azure-Vorschauportal anmelden.

2. Öffnen der Azure-Markt **Markt** klicken:

    ![Marketplace-Symbol][marketplace]

    Oder auf das Symbol **neu** oben rechts im Schaltpult und **Markt** Bottow der Liste auswählen.
    
    ![Neu erstellen][5]
    
3. Wählen Sie **Web + Mobile**. Suchen Sie **WordPress** , und klicken Sie auf **WordPress** .

    ![WordPress aus][7]
    
5. Wählen Sie nach dem Lesen der Beschreibung der app WordPress, **Erstellen**.

6. Klicken Sie auf **WebApp**und Bereitstellen Sie die erforderlichen Werte für die Konfiguration Ihrer Anwendung.
    
    ![Konfigurieren Sie Ihre Anwendung][8]

7. Klicken Sie auf **Datenbank**, und geben Sie die erforderlichen Werte für die MySQL-Datenbank konfigurieren. 

    ![Datenbank konfigurieren][database]

8. Geben Sie einen Namen für eine neue Ressourcengruppe.

    ![Ressourcengruppe][groupname]

8. Gegebenenfalls auf **ABONNEMENTS**und geben Sie das Abonnement verwenden. 

7. Sie nach dem Definieren der Web app auf **Erstellen**und warten Sie die neuen Web app erstellt.

   Erstellung die Anwendung sehen Sie die Ressourcengruppe, Webanwendung und Datenbank.

   ![Gruppe anzeigen][resourcegroup]

## <a name="launch-and-manage-your-wordpress-web-app"></a>Starten Sie und verwalten Sie Ihrer Anwendung WordPress
    
1. Klicken Sie auf Ihrer neuen Anwendung Einzelheiten zu Ihrer Anwendung.

    ![Dashboard starten][10]

2. Klicken Sie auf **Grundlagen** auf **Durchsuchen** oder den Link unter **Url** Web app-Homepage geöffnet.

    ![Website-URL][browse]

3. WordPress nicht installiert haben, geben Sie die entsprechenden Konfigurationsinformationen WordPress erforderlich und klicken auf **Installieren WordPress** Konfiguration abschließen und öffnen Sie die Web-app-Anmeldeseite.

4. Klicken Sie auf **Anmelden** und geben Sie Ihre Anmeldeinformationen.  

5. Die haben einer neuen WordPress Web app Web app unten ähnelt.    

    ![WordPress-Website][13]






[5]: ./media/website-from-gallery/start-marketplace.png
[6]: ./media/website-from-gallery/wordpressgallery-02.png
[7]: ./media/website-from-gallery/search-web-app.png
[8]: ./media/website-from-gallery/set-web-app.png
[9]: ./media/website-from-gallery/wordpressgallery-05.png
[10]: ./media/website-from-gallery/select-web.png
[13]: ./media/website-from-gallery/wordpressgallery-09.png
[webapps]: ./media/website-from-gallery/selectwebapps.png
[database]: ./media/website-from-gallery/set-db.png
[resourcegroup]: ./media/website-from-gallery/show-rg.png
[browse]: ./media/website-from-gallery/browse-web.png
[marketplace]: ./media/website-from-gallery/marketplace-icon.png
[groupname]: ./media/website-from-gallery/set-rg.png
