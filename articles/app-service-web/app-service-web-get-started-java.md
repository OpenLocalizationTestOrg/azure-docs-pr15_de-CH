<properties 
    pageTitle="Bereitstellen Ihrer ersten Java Anwendung in Azure Minuten | Microsoft Azure" 
    description="Erfahren Sie, wie einfach die webapps durch Bereitstellung einer Beispiel-app in App Service ausgeführt wird. Starten Sie Entwicklung schnell und die sehen Sie Ergebnisse sofort." 
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/13/2016" 
    ms.author="cephalin"
/>
    
# <a name="deploy-your-first-java-web-app-to-azure-in-five-minutes"></a>Bereitstellen Sie Ihrer ersten Java Anwendung in Azure in fünf Minuten

In diesem Lernprogramm können Sie eine einfache Java Web app [Azure App](../app-service/app-service-value-prop-what-is.md)Service bereitstellen.
App Service können Sie Web-apps [mobile-app back-Ends](/documentation/learning-paths/appservice-mobileapps/)und [API-apps](../app-service-api/app-service-api-apps-why-best-platform.md)erstellen.

Sie können: 

- Erstellen Sie eine Webanwendung in Azure App Service.
- Bereitstellen Sie eine Beispiel Java-Anwendung.
- Siehe Code live in der Produktion ausgeführt.

## <a name="prerequisites"></a>Erforderliche Komponenten

- Einen FTP-FTP-Client wie [FileZilla](https://filezilla-project.org/)zu erhalten.
- Microsoft Azure-Konto zu erhalten. Haben Sie ein Konto, können Sie [sich für eine kostenlose Testversion](/pricing/free-trial/?WT.mc_id=A261C142F) oder [die Visual Studio-Abonnementvorteile aktivieren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] Sie können [Versuchen App Service](http://go.microsoft.com/fwlink/?LinkId=523751) ohne ein Azure-Konto. Erstellen einer app Starter und spielen für bis zu einer Stunde – keine Kreditkarte, keine Zusagen.

<a name="create"></a>
## <a name="create-a-web-app"></a>Erstellen einer Webanwendung

1. Mit der [Azure-Portal](https://portal.azure.com) mit Ihrem Azure-Konto anmelden.

2. Klicken Sie im linken Menü auf **neu** > **Web + Mobile** > **Web App**.

    ![](./media/app-service-web-get-started-languages/create-web-app-portal.png)

3. Blatt erstellen app verwenden Sie Folgendes für Ihre neue Anwendung:

    - **Anw.-Name**: Geben Sie einen eindeutigen Namen.
    - **Gruppe**: Wählen Sie **neu erstellen** und benennen Sie der Ressourcengruppe aus.
    - **App Plan/Speicherort**: sie konfigurieren klicken Sie auf **Neu erstellen** , um Name, Speicherort und Tarif App Service-Plan festgelegt. Preisstufe **frei** verwenden können.

    Ihre app erstellen Blade sollte wie folgt aussehen:

    ![](./media/app-service-web-get-started-languages/create-web-app-settings.png)

3. Klicken Sie unten auf " **Erstellen** ". Klicken Sie auf **das Symbol oben, um die Fortschritte** .

    ![](./media/app-service-web-get-started-languages/create-web-app-started.png)

4. Wenn die Bereitstellung abgeschlossen ist, erhalten Sie diese Benachrichtigung. Klicken Sie auf die Bereitstellung Blade Öffnen der Nachricht.

    ![](./media/app-service-web-get-started-languages/create-web-app-finished.png)

5. Blatt **Bereitstellung war erfolgreich** klicken Sie **Ressource** Blatt der neuen Web app öffnen.

    ![](./media/app-service-web-get-started-languages/create-web-app-resource.png)

## <a name="deploy-a-java-app-to-your-web-app"></a>Bereitstellen einer Java-Anwendung auf Ihrer Anwendung

Nun, wir eine Java-Anwendung in Azure mithilfe von FTP bereitstellen.

5. Web app Blade **Einstellungen** oder suchen Scrollen Sie und klicken Sie auf. 

    ![](./media/app-service-web-get-started-languages/set-java-application-settings.png)

6. **Java-Version** **Java 8** und klicken Sie auf **Speichern**.

    ![](./media/app-service-web-get-started-languages/set-java-application-settings.png)

    Wenn Sie **erfolgreich aktualisiert Web Appeinstellungen**benachrichtigt, navigieren Sie zu http://*&lt;Anwendungsname >*. *.azurewebsites.NET Standard JSP-Servlet in Aktion zu sehen.

7. Scrollen Sie im Web app Blade **Bereitstellung Anmeldeinformationen** oder suchen, klicken Sie darauf.

8. Festlegen Sie Ihre Bereitstellung Anmeldeinformationen, und klicken Sie auf **Speichern**.

7. Klicken Sie in das Web app Blade auf **Übersicht**. **FTP-Bereitstellung Benutzername** und **FTP-Hostname**klicken Sie auf die Schaltfläche **Kopieren** , um diese Werte zu kopieren.

    ![](./media/app-service-web-get-started-languages/get-ftp-url.png)

    Sie nun können Ihre Java-Anwendung mit FTP bereitstellen.

8. Melden Sie sich in Ihrem FTP-FTP-Client mit Werten, die Sie im letzten Schritt kopiert Ihre Azure Web app FTP-Server an. Verwenden Sie, das zuvor erstellte Bereitstellung Kennwort.

    Der folgende Screenshot zeigt mit FileZilla.

    ![](./media/app-service-web-get-started-languages/filezilla-login.png)

    Sicherheitshinweisen für unbekannte SSL-Zertifikat von Azure auftreten. Fortfahren und fortzufahren.

9. Klicken Sie [hier](https://github.com/Azure-Samples/app-service-web-java-get-started/raw/master/webapps/ROOT.war) um die WAR-Datei auf Ihrem lokalen Computer herunterzuladen.

9. In Ihrem FTP-FTP-Client **/site/wwwroot/webapps** am Remotestandort wechseln Sie, und ziehen Sie heruntergeladene WAR-Datei auf dem lokalen Computer in remote-Verzeichnis.

    ![](./media/app-service-web-get-started-languages/transfer-war-file.png)

    Klicken Sie auf **OK** , um die Datei in Azure überschreiben.

    >[AZURE.NOTE] Gemäß der Tomcat-Standardverhalten Dateiname **ROOT.war** in /site/wwwroot/webapps bietet Root Web app (http://*&lt;Appname >*. *.azurewebsites.NET), und der Dateiname ** * &lt;EinName >*.war** gibt eine benannte Web app (http://*&lt;Appname >*.azurewebsites.net/*&lt;EinName >*).

Das wars! Ihre Java-Anwendung ist jetzt aktiv in Azure ausgeführt. Navigieren Sie in Ihrem Browser auf http://*&lt;Anwendungsname >*. *.azurewebsites.NET in Aktion zu sehen. 

## <a name="make-updates-to-your-app"></a>Updates für Ihre Anwendung erstellen

Wenn Sie eine Aktualisierung vornehmen müssen, Uploaden der WAR-Datei in dasselbe Verzeichnis remote mit FTP-FTP-Client.

## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer Java Web app aus einer Vorlage in Azure Marketplace](web-sites-java-get-started.md#marketplace). Sie können eigene vollständig anpassbaren Tomcat Container abrufen und bekannte Manager-UI. 

Debuggen Sie Ihre Azure Web app auf [IntelliJ](app-service-web-debug-java-web-app-in-intellij.md) oder [Eclipse](app-service-web-debug-java-web-app-in-eclipse.md).

Oder möchten Sie Ihrer ersten Anwendung. Zum Beispiel:

- Probieren Sie [dazu Code in Azure bereitstellen](../app-service-web/web-sites-deploy.md). 
- Nehmen Sie Ihre Azure-Anwendung auf die nächste Ebene. Benutzerauthentifizierung. Skalierung auf Anforderung. Einige Performance-Alarme einrichten. Mit wenigen Mausklicks. Finden Sie unter [Hinzufügen auf Ihrer ersten Anwendung](app-service-web-get-started-2.md).

