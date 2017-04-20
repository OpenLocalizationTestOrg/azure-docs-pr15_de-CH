<properties
    pageTitle="Erstellen Sie eine Java Web app in Azure App Service | Microsoft Azure"
    description="Dieses Lernprogramm veranschaulicht die Java Web app Azure App Service bereitstellen."
    services="app-service\web"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="get-started-article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="create-a-java-web-app-in-azure-app-service"></a>Erstellen Sie eine Java Web app in Azure App Service

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

In diesem Lernprogramm wird das Java [WebApp in Azure App Service] mithilfe der [Azure-Portal]. Azure-Portal ist eine Weboberfläche, Azure Ressourcen verwalten.

> [AZURE.NOTE] Um dieses Lernprogramm benötigen Sie ein Microsoft Azure-Konto. Wenn Sie ein Konto haben, können Sie [Ihre Visual Studio-Abonnementvorteile aktivieren] oder [Registrieren Sie sich für eine kostenlose Testversion].
>
> Wenn Sie mit Azure App Service beginnen, bevor Sie für ein Azure-Konto, gehen Sie [versuchen App]Service. Dort können Sie sofort eine kurzlebige Starter Web app in App Service erstellen. ist keine Kreditkarte erforderlich, und.

## <a name="java-application-options"></a>Java-Anwendungsoptionen

Es gibt verschiedene Arten können Sie eine Java-Anwendung in einer Web App Service. 

1. Erstellen Sie eine Anwendung und konfigurieren Sie **ApplicationSettings**.

    App Service bietet mehrere Tomcat und Steg Versionen Standardkonfiguration. Wenn die Anwendung, die Sie Hosten eines integrierten Versionen arbeiten, wird diese Methode von Web-Container einrichten am einfachsten und ist ideal, wenn Sie möchten lediglich Web-Container eine War-Datei hinzufügen. Für diese Methode erstellen eine Anwendung in Azure-Portal und fahren mit Blade **Einstellungen** für Ihre Anwendung die Version von Java und den Java Web-Container auswählen. Bei Verwendung dieser Methode werden Java und Web Containers Programme ausführen. Andere Methoden setzen Webcontainer und potenziell JVM in Speicherplatz. Bei Verwendung dieses Modells haben Sie keinen Zugriff zum Bearbeiten von Dateien in diesem Teil des Dateisystems. Dies bedeutet z. B. nicht konfigurieren Datei *server.xml* oder Bibliotheksdateien in der *lib* -Ordner. Weitere Informationen finden Sie unter der [Erstellen und konfigurieren eine Java Web app](#appsettings) weiter unten in diesem Lernprogramm.
    
2. Verwenden Sie eine Vorlage aus dem Azure Marketplace.

    Azure Marketplace enthält Vorlagen, die automatisch erstellen und die Java webapps mit Tomcat oder Steg Web konfigurieren. Konfiguriert werden die Web-Container, die die Vorlagen erstellt. Weitere Informationen finden Sie unter [Java Vorlage Azure Marketplace](#marketplace) -Abschnitt dieser praktischen Einführung.
  
3. Erstellen Sie eine Anwendung manuell kopieren Sie und Konfigurationsdateien 

    Sie möchten eine benutzerdefinierte Java-Anwendung hosten, die nicht in einem Container Web App Dienst bereitgestellt wird. Zum Beispiel:
    
    * Die Java-Anwendung erfordert eine Version von Tomcat oder Anleger, die nicht direkt vom App-Dienst unterstützt oder im Katalog bereitgestellt.
    * Die Java-Anwendung akzeptiert HTTP-Anfragen und stellt eine in einem bereits vorhandenen Webcontainer bereit.
    * Möchten Sie den Webcontainer neu zu konfigurieren. 
    * Sie möchten eine Version von Java in App Service nicht unterstützt wird und Sie hochladen möchten.

    Für Fälle wie diese können erstellen eine Anwendung mithilfe von Azure-Portal und die entsprechenden Laufzeitdateien manuell angeben. In diesem Fall werden die Dateien der Speicherplatz Speicherkontingente für Ihren App Service-Plan angerechnet. Weitere Informationen finden Sie in [einer benutzerdefinierten Java Web app Azure hochladen].

## <a name="portal"></a>Erstellen und Konfigurieren einer Java Web app

Dieser Abschnitt veranschaulicht eine Webanwendung erstellen und konfigurieren sie für Java mit den **Einstellungen** des Portals.

1. [Azure-Portal]anmelden.

2. Klicken Sie auf **Neu > Web + Mobile > Web App**.

    ![Neue Web App][newwebapp]

4. Geben Sie einen Namen für die Webanwendung im **WebApp** .

    Dieser Name muss in der Domäne *.azurewebsites.NET da URL Web App {Name} werden. *.azurewebsites.NET. Wenn der eingegebene Name nicht eindeutig ist, wird im Feld ein rotes Ausrufezeichen angezeigt.

5. Wählen Sie eine **Ressourcengruppe** oder erstellen Sie eine neue.

    Weitere Informationen zu Ressourcengruppen finden Sie unter [Verwenden der Azure-Verwaltungsportal Azure Ressourcen verwalten].

6. Wählen Sie eine **App Plan/Speicherort** oder erstellen Sie eine neue.

    Weitere Informationen zu App Service-Pläne finden Sie unter [Übersicht über Azure App Service-Pläne].

7. Klicken Sie auf **Erstellen**.

    ![WebApp erstellen][newwebapp2]
 
8. Wenn die Webanwendung erstellt wurde, klicken Sie auf **Web Apps > {Ihrer Anwendung}**.
 
    ![Wählen Sie WebApp][selectwebapp]

9. **Klicken Sie auf Blatt **WebApp** .**

10. Klicken Sie auf **Anwendung**.

11. Wählen Sie die gewünschten **Java-Version**. 

12. Wählen Sie die gewünschten **Java Nebenversion**. Wählt **neueste**verwendet Ihre Anwendung die neueste Nebenversion in App Service für diese Java-Hauptversion ist. Das **neueste** Element ist eindeutig Java apps **Einstellungen**erstellt. Wenn Sie Ihre Java-Anwendung aus dem Katalog erstellen, müssen Sie verwalten Ihre eigenen Container und JVM ändert. 

12. Wählen Sie die gewünschte **Webcontainer**. Bei Auswahl einer, bei denen **neueste**wird Ihre app auf die neueste Version dieses Web Container wichtige in App Service steht gehalten. 

    ![Container-Versionen][versions]

13. Klicken Sie auf **Speichern**.

    In wenigen Augenblicken wird Ihrer Anwendung Java-basierte und mit ausgewählten Webcontainer konfiguriert.

14. Klicken Sie auf **Web-apps > {Ihrer neuen Anwendung}**.

15. Klicken Sie auf die **URL** , um den neuen Standort zu suchen.

    Die Webseite bestätigt, dass eine Java-basierte Web app erstellt haben.

## <a name="marketplace"></a>Verwenden einer Vorlage Java Azure Marketplace

Dieser Abschnitt beschreibt, wie Sie Azure Marketplace Java Web app erstellen. Der gleiche Vorgang kann auch verwendet werden, Java-basierten mobilen oder API-app erstellen. 

1. [Azure-Portal] anmelden

2. Klicken Sie auf **Neu > Markt**.

    ![Neuer Marktplatz][newmarketplace]

3. Klicken Sie auf **Web + Mobile**.

    Möglicherweise links blättern, um das **Marketplace** -Blade sehen Sie **Web + Mobile**auswählen können.

4. Geben Sie im Suchfeld ein Java-Anwendungsserver **Steg**oder **Apache Tomcat** und drücken Sie die EINGABETASTE.

5. Klicken Sie in den Suchergebnissen auf Java-Anwendungsserver.

    ![Mobile Web-Steg][webmobilejetty]

6. Klicken Sie auf das erste **Apache Tomcat** oder **Steg** Blade **Erstellen**.

    ![Steg Portal Blade][jettyblade]

7. Geben Sie einen Namen für das Web app im Feld **WebApp** Weiter **Apache Tomcat** oder **Steg** Blatt.

    Dieser Name muss in der Domäne *.azurewebsites.NET da URL Web App {Name} werden. *.azurewebsites.NET. Wenn der eingegebene Name nicht eindeutig ist, wird im Feld ein rotes Ausrufezeichen angezeigt.

8. Wählen Sie eine **Ressourcengruppe** oder erstellen Sie eine neue.

    Weitere Informationen zu Ressourcengruppen finden Sie unter [Verwenden der Azure-Verwaltungsportal Azure Ressourcen verwalten].

9. Wählen Sie eine **App Plan/Speicherort** oder erstellen Sie eine neue.

    Weitere Informationen zu App Service-Pläne finden Sie unter [Übersicht über Azure App Service-Pläne].

10. Klicken Sie auf **Erstellen**.

    ![Steg Portal erstellen][jettyportalcreate2]

    In kurzer Zeit in der Regel weniger als einer Minute beendet Azure neue Web app erstellen.

11. Klicken Sie auf **Web-apps > {Ihrer neuen Anwendung}**.

12. Klicken Sie auf die **URL** , um den neuen Standort zu suchen.

    ![Steg URL][jettyurl]

    Tomcat Schiffe mit einer Reihe von Seiten; Wenn Sie Tomcat gewählt haben, sehen Sie eine Seite ähnlich dem folgenden Beispiel.

    ![WebApp mit Apache Tomcat][tomcat]

    Wenn Steg gewählt haben, sehen Sie eine Seite ähnlich dem folgenden Beispiel. Steg ist einen Standardsatz Seite die gleiche JSP eine leere Java-Website dient hier wiederverwendet wird.

    ![WebApp mit Steg][jetty]

Erstellung die Webanwendung mit einer Anwendung finden Sie im Abschnitt [Weiter](#next-steps) Informationen zum Uploaden der Anwendung Web App.

## <a name="next-steps"></a>Nächste Schritte

Jetzt haben Sie einen Java-Anwendungsserver in Ihrer Anwendung in Azure App Service ausgeführt. Um eigenen Code zu Web app bereitzustellen, finden Sie unter [Hinzufügen einer Anwendung oder eine Webseite zu Ihrer Anwendung Java].

Weitere Informationen zum Entwickeln von Java-Anwendung in Azure finden Sie unter [Java Developer Center].

<!-- URL List -->

[Hinzufügen einer Anwendung oder Webseite Ihrer Anwendung Java]: ./web-sites-java-add-app.md
[Azure App Service-Pläne (Übersicht)]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[Azure-Portal]: https://portal.azure.com/
[Aktivieren Sie Ihre Visual Studio-Abonnementvorteile]: http://go.microsoft.com/fwlink/?LinkId=623901
[Melden Sie sich für eine kostenlose Testversion]: http://go.microsoft.com/fwlink/?LinkId=623901
[Versuchen Sie App Service]: http://go.microsoft.com/fwlink/?LinkId=523751
[Web-app in Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Java Developer Center]: /develop/java/
[Mithilfe von Azure-Portal Azure Ressourcen verwalten]: ../azure-portal/resource-group-portal.md
[Eine benutzerdefinierte Java Web app in Azure hochladen]: ./web-sites-java-custom-upload.md

<!-- IMG List -->

[newwebapp]: ./media/web-sites-java-get-started/newwebapp.png
[newwebapp2]: ./media/web-sites-java-get-started/newwebapp2.png
[selectwebapp]: ./media/web-sites-java-get-started/selectwebapp.png
[versions]: ./media/web-sites-java-get-started/versions.png
[newmarketplace]: ./media/web-sites-java-get-started/newmarketplace.png
[webmobilejetty]: ./media/web-sites-java-get-started/webmobilejetty.png
[jettyblade]: ./media/web-sites-java-get-started/jettyblade.png
[jettyportalcreate2]: ./media/web-sites-java-get-started/jettyportalcreate2.png
[jettyurl]: ./media/web-sites-java-get-started/jettyurl.png
[tomcat]: ./media/web-sites-java-get-started/tomcat.png
[jetty]: ./media/web-sites-java-get-started/jetty.png
