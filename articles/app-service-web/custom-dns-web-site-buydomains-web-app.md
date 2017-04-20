<properties
    pageTitle="Einen benutzerdefinierten Domänennamen in Azure App Service Web Apps kaufen"
    description="Informationen Sie zum benutzerdefinierten Domänennamen mit einer Web-app in Azure App Service kaufen."
    services="app-service\web"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="buy-and-configure-a-custom-domain-name-in-azure-app-service"></a>Kaufen und einen benutzerdefinierten Domänennamen in Azure App Service konfigurieren

[AZURE.INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

Beim Erstellen einer Webanwendung zugewiesen Azure eine Subdomäne von *.azurewebsites.NET. Beispielsweise ist Ihrer Anwendung **Contoso**lautet, die URL **contoso.azurewebsites.net**. Azure weist auch eine virtuelle IP-Adresse.

Für eine Produktion Web app sollten Sie Benutzern einen benutzerdefinierten Domänennamen. Dieser Artikel erläutert die kaufen und Konfigurieren einer benutzerdefinierten Domäne mit [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). 

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]


## <a name="overview"></a>Übersicht

Haben Sie einen Domänennamen für Ihre Webanwendung, können Sie eine [Azure](https://portal.azure.com/)-Portal einfach kaufen. Bei der Bestellung können Sie WWW und Root Domain DNS-Datensätze automatisch Ihrer Anwendung zugeordnet werden. Sie können auch die Domäne direkt in Azure-Portal verwalten.


Gehen Sie folgendermaßen vor, Domänennamen und Ihrer Anwendung zuweisen.

1. Öffnen Sie in Ihrem Browser [Azure-Portal](https://portal.azure.com/).

2. Auf der Registerkarte **Web Apps** Ihrer Anwendung klicken Sie auf **Bearbeiten Sie**und wählen Sie **benutzerdefinierte Domänen**

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

3. Klicken Sie auf **Buy Domänen**, Blatt **Custom Domains** .

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-1.png)

4. Blatt **Kaufen Domänen** verwenden Sie das Textfeld an den Domänennamen eingeben und EINGABETASTE. Die vorgeschlagenen verfügbaren Domänen werden direkt unter dem Textfeld angezeigt. Wählen Sie welche Domäne Sie kaufen möchten. Sie können mehrere Domänen auf einmal kaufen. 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.png)

5. Klicken Sie auf den **Kontakt** und füllen Sie der Domäne Kontaktinformationen aus.

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-3.png)

    > [AZURE.NOTE] Es ist sehr wichtig, dass Sie alle erforderlichen Felder mit Genauigkeit wie möglich, insbesondere die e-Mail-Adresse eingeben. Bei Kauf der Domäne ohne "Datenschutz" möglicherweise Sie aufgefordert, Ihre e-Mail-Adresse die Domäne zu überprüfen. In einigen Fällen führt die falsche Daten Informationen Fehler Domänen erwerben. 

6. Jetzt können Sie auswählen,

    (a) "Automatische Verlängerung" der Domäne jedes Jahr
    
    (b) anmelden "Datenschutz" kostenlos im Kaufpreis enthalten (außer TLDs, die Registrierung nicht unterstützt Datenschutz. Zum Beispiel:. co.in,. co.uk etc..)  
    
    (c) "weisen standardmäßig Hostnamen" www und root-Domäne der aktuellen Web App. 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.5.png)
  
    > [AZURE.NOTE] Option C konfiguriert DNS-Bindings und Hostname Bindings automatisch für Sie.  Auf diese Weise können benutzerdefinierte Domäne verwenden, als die Bestellung abgeschlossen (baring DNS-Verteilung bei einigen Fällen) ist Ihrer Anwendung zugegriffen werden. Bei Ihrer Anwendung hinter Azure Traffic Manager ist, sehen Sie eine Datensätze nicht mit Traffic Manager arbeiten nicht Option Stammdomäne zuweisen. Sie können immer die Domänen/sub-domains ein Web App Web App und umgekehrt erworbene zuweisen. Siehe Schritt 8 Weitere Informationen. 
    
7. **Wählen Sie** auf **Domänen kaufen** klicken Sie sehen Sie die Informationen auf **Bestätigung erwerben** . Wenn Sie die Vertragsbedingungen akzeptieren, klicken Sie auf **Buy**Bestellung übermittelt und den Einkaufsprozess **Benachrichtigung**überwachen. Domäne Einkauf kann einige Minuten dauern. 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-4.png)

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-5.png)

8. Wenn Sie eine Domäne erfolgreich bestellt, können Sie die Domäne verwalten und Ihrer Anwendung zuweisen. Klicken Sie auf die **"..."** auf der rechten Seite Ihrer Domäne. Dann können Sie **Einkauf Abbrechen** oder **Domäne verwalten**. Klicken Sie auf **Domäne verwalten** **Subdomäne** zu unserer Web app auf **Verwalten Domäne** gebunden werden kann. Wenn Sie eine **untergeordnete Domäne** zu einer anderen Web App binden möchten führen Sie diesen Schritt aus im Rahmen der jeweiligen Web App. Hier Namen auswählen zum Zuweisen der Domäne Traffic Manager-Endpunkt (bei Web App hinter TM) einfach auswählen Traffic Manager aus dem Dropdown-Menü. Dadurch wird/untergeordneten Domäne automatisch alle Web-Apps hinter dem Endpunkt Traffic Manager zugewiesen werden. 

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-6.png)

    > [AZURE.NOTE] Sie können "Bestellung innerhalb von 5 Tagen für volle Rückerstattung Abbrechen". Nach 5 Tagen werden Sie nicht auf "" Abbrechen "kaufen", wird stattdessen eine Option "Bereich löschen" angezeigt. Löschen der Domäne wird aus Ihrem Abonnement ohne Erstattung freigegeben und werden Sie verfügbare. 

Nach Abschluss der Konfiguration wird der benutzerdefinierten Domänennamen im Abschnitt **Hostname Bindungen** Ihrer Anwendung aufgeführt.

An diesem Punkt sollten Sie möglicherweise den benutzerdefinierten Domänennamen in Ihrem Browser eingeben und erfolgreich gelangen zu Ihrer Anwendung.
 
## <a name="what-happens-to-the-custom-domain-you-bought"></a>Was geschieht mit benutzerdefinierten Domäne gekauften

Azure-Abonnement die benutzerdefinierte Domäne Blade **Custom Domains und SSL** kaufte verbunden. Als Azure Ressource ist dieser benutzerdefinierten Domäne getrennt und unabhängig von den Kauf die Domäne App Service-app. Das bedeutet:

- In Azure-Portal können Sie benutzerdefinierte Domäne gekauften für mehrere App Service-Anwendung und nicht nur für den Kauf die benutzerdefinierte Domäne app. 
- Sie können benutzerdefinierten Domänen verwalten gekauften in Azure-Abonnement durch **benutzerdefinierte Domains und SSL** Falz *Alle* App Service-app in diesem Abonnement wird.
- Sie können alle App Service-app aus derselben Azure-Abonnement eine Unterdomäne der benutzerdefinierten Domäne zuweisen.
- Wenn Sie eine App Service-app löschen möchten, können Sie nicht die benutzerdefinierte Domäne löschen, der es gebunden ist, wenn Sie andere Apps verwenden möchten.

## <a name="if-you-cant-see-the-custom-domain-you-bought"></a>Wenn Sie benutzerdefinierte Domäne nicht sehen Sie gekauft

Wenn Sie die benutzerdefinierte Domäne aus Blade **Custom Domains und SSL** gekauft haben, aber nicht die benutzerdefinierte Domäne unter **verwalteten Domänen**angezeigt, überprüfen Sie Folgendes:

- Die Erstellung benutzerdefinierter Domäne möglicherweise nicht haben. Überprüfen Sie Bell Benachrichtigung am oberen Rand der Azure-Portal für den Fortschritt.
- Die Erstellung benutzerdefinierter Domäne möglicherweise aus irgendeinem Grund fehlgeschlagen. Überprüfen Sie Bell Benachrichtigung am oberen Rand der Azure-Portal für den Fortschritt.
- Die benutzerdefinierte Domäne möglicherweise gelungen, doch das Blade kann nicht aktualisiert werden. Versuchen Sie, öffnen das Blade **Custom Domains und SSL** .
- Sie können die benutzerdefinierte Domäne irgendwann gelöscht. Überprüfen Sie die Überwachungsprotokolle **Klicken Sie** > **Protokolle überwachen** Ihre app wichtigsten Blatt. 
- **Custom Domains und SSL** -Blade im gewünschten gehört möglicherweise zu einer Anwendung, die in einem anderen Azure-Abonnement erstellt wird. Wechseln Sie zu einer anderen Anwendung in ein anderes Abonnement, und überprüfen Sie die Blade **Custom Domains und SSL** .  
  Sie innerhalb des Portals wird nicht möglicherweise anzeigen oder Verwalten von benutzerdefinierten Domänen in ein anderes Azure Abonnement als die Anwendung erstellt. Jedoch wenn Sie **Erweiterte Verwaltung** der Domäne **Domäne verwalten** Blatt klicken, Sie werden weitergeleitet Domäne Anbieter Website   [Ihre benutzerdefinierte Domain wie externe benutzerdefinierte Domäne manuell](web-sites-custom-domain-name.md) konfigurieren, werden 
   für apps in ein anderes Azure-Abonnement erstellt. 


