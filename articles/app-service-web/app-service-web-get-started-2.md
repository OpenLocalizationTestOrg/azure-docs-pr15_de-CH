<properties
    pageTitle="Ihrer ersten Anwendung Funktionen hinzufügen"
    description="Fügen Sie tolle Features Ihrer ersten Anwendung in wenigen Minuten hinzu."
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
    ms.date="05/12/2016"
    ms.author="cephalin"
/>

# <a name="add-functionality-to-your-first-web-app"></a>Ihrer ersten Anwendung Funktionen hinzufügen

In [Ihrer ersten Anwendung in Azure Minuten bereitstellen](app-service-web-get-started.md)wird eine Beispiel Web app auf [Azure App Service](../app-service/app-service-value-prop-what-is.md)bereitgestellt. In diesem Artikel werden Sie schnell Ihrer bereitgestellte Anwendung einige großartigen Funktionen hinzufügen. In wenigen Minuten können wie folgt vor:

- für die Benutzer erzwingen
- Ihre app automatisch skalieren
- die Leistung Ihrer Anwendung erhalten

Unabhängig von der Beispiel-app in diesem Artikel bereitgestellt, können Sie im Lernprogramm folgen.

Drei Aktivitäten in diesem Lernprogramm sind nur einige Beispiele für viele nützliche Features erhalten Sie bei Ihrer Anwendung in App-Dienst gestellt. Viele der Features stehen in der **frei** (also nach Ihrer ersten Anwendung ausgeführt wird), und Sie können Ihre Testversion Credits Funktionen ausprobieren, die Preise höher stufen erfordern. Versichert, dass Ihrer Anwendung **frei** Ebene bleibt, wenn Sie explizit in einen anderen Tarif ändert.

>[AZURE.NOTE] Web app mit Azure CLI **frei** Ebene nur ermöglicht eine gemeinsame VM-Instanz mit Ressource ausgeführt wird. Weitere Informationen Sie mit **frei erhalten** finden Sie unter [App Service Grenzen](../azure-subscription-service-limits.md#app-service-limits).

## <a name="authenticate-your-users"></a>Benutzer authentifizieren

Nun sehen, wie einfach es ist, Ihrer app (Weitere Informationen zur [App Service Authentifizierung/Autorisierung](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/)) Authentifizierung hinzufügen.

1. Im Portal Blade für Ihre Anwendung die soeben, **Klicken Sie** > **Authentifizierung / Autorisierung**.  
    ![Authentifizierung - Einstellungen-Blades](./media/app-service-web-get-started/aad-login-settings.png)

2. Klicken Sie **auf** Authentifizierung aktivieren.  

4. Klicken Sie auf **Active Directory Azure** **Authentifizierungsanbieter**.  
    ![Authentifizierung – Azure Anzeige auswählen](./media/app-service-web-get-started/aad-login-config.png)

5. Blatt **Azure Active Directory Einstellungen** **Express**, klicken auf **OK**. Die Standardeinstellungen erstellen Sie eine neue Anwendung im Standardverzeichnis Azure AD.  
 ![Authentifizierung - express-Konfiguration](./media/app-service-web-get-started/aad-login-express.png)

6. Klicken Sie auf **Speichern**.  
    ![Authentifizierung - Konfiguration speichern](./media/app-service-web-get-started/aad-login-save.png)

    Wenn die Änderung erfolgreich ist, sehen Sie die Benachrichtigung Bell Grün, und eine Meldung.

7. Klicken Sie im Portal Blade Ihrer App auf den **URL** -Link (oder **Wechseln Sie** in der Menüleiste). Der Link ist eine HTTP-Adresse.  
    ![Authentifizierung – zu URL wechseln](./media/app-service-web-get-started/aad-login-browse-click.png)  
    Aber die Anwendung in einer neuen Registerkarte geöffnet wird, der URL Feld leitet mehrmals und Ihrer App eine HTTPS-Adresse ist. Was Sie sehen, dass Sie bereits angemeldet ist der Azure-Abonnement, und Sie sind automatisch in der Anwendung authentifiziert.  
    ![Authentifizierung - angemeldet](./media/app-service-web-get-started/aad-login-browse-http-postclick.png)  
    Also wenn Sie jetzt eine nicht authentifizierte Sitzung in einem anderen Browser öffnen, sehen einen Anmeldebildschirm Sie demselben URL navigieren.  
    <!-- ![Authenticate - login page](./media/app-service-web-get-started/aad-login-browse.png)  -->
   Wenn Sie noch nie mit Azure Active Directory getan haben, möglicherweise Standardverzeichnis Azure AD Benutzer nicht. In diesem Fall ist das einzige Konto dort wahrscheinlich das Microsoft-Konto mit Ihrem Azure-Abonnement. Deshalb Sie automatisch der Anwendung im Browser bereits angemeldet wurden.
   Microsoftkonto können Sie diese Anmeldeseite und melden.

Herzlichen Glückwunsch, Sie alle Datenverkehr auf Ihrer Anwendung authentifizieren.

Sie haben vielleicht bemerkt der **Authentifizierung / Autorisierung** Blade viel mehr wie möglich:

- Soziale Anmeldung aktivieren
- Aktivieren Sie mehrerer Anmeldeoptionen
- Ändern Sie das Standardverhalten, wenn Benutzer Ihre app erstmals aufrufen

App Service bietet eine Lösung für einige allgemeine Authentifizierung müssen Sie nicht die Authentifizierungslogik bereitstellen muss.
Weitere Informationen finden Sie unter [App Service Authentifizierung/Autorisierung](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/).

## <a name="scale-your-app-automatically-based-on-demand"></a>Skalieren Sie Ihre app basierend auf Anforderung automatisch

Weiter Wir Skalieren Ihrer Anwendung, damit sie es Benutzer bei Bedarf (Weitere Informationen zu [Skalieren Ihrer Anwendung in Azure](web-sites-scale.md) [Skalieren Instanzenzahl manuell oder automatisch](../monitoring-and-diagnostics/insights-how-to-scale.md)) reagieren automatisch angepasst werden.

Skalieren Sie, Ihrer Anwendung auf zwei Arten:

- [Skalieren](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): erhalten mehr CPU, Speicher, Speicherplatz und zusätzliche Features wie dedizierten VMs, benutzerdefinierte Domänen und Zertifikate, staging-Steckplätze und Skalierung. Heraufskalierung Tarif App Service-Plan gehört Ihre app zu ändern.
- [Skalieren](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): Zahl der VM Instanzen Ihrer app.
Sie können bis zu 50 Instanzen je nach Ihrem Tarif skalieren.

Ohne weiteres richten Sie automatische Skalierung.

1. Erstens skalieren wir Skalierung aktivieren. **Klicken Sie im Portal Blade Ihrer Anwendung** > **Aufwärtsskalieren (App Service-Plan)**.  
    ![Skalieren - Einstellungen-Blades](./media/app-service-web-get-started/scale-up-settings.png)

2. Blättern Sie und wählen Sie die **S1 Standard** Ebene der niedrigsten Ebene, die Skalierung (in Abbildung eingekreist) unterstützt, klicken Sie auf **auswählen**.  
    ![Skalieren - Ebene auswählen](./media/app-service-web-get-started/scale-up-select.png)

    Sie Heraufskalieren.

    >[AZURE.IMPORTANT] Diese Ebene verbraucht Ihre kostenlose Testversion Kredite. Wenn Sie Pay-per-Use-Konto verfügen, fallen Gebühren für Ihr Konto.

3. Als Nächstes konfigurieren wir Skalierung. **Klicken Sie im Portal Blade Ihrer Anwendung** > **Dezentrales (App Service-Plan)**.  
    ![Dezentrales Skalieren - Einstellungen-Blades](./media/app-service-web-get-started/scale-out-settings.png)

4. Ändern Sie **Skalierung von** **CPU-**Prozentsatz. Der Schieberegler unter der Dropdownliste entsprechend aktualisieren. Definieren Sie dann einen **Instanzen** zwischen **1** und **2** sowie einen **Bereich** zwischen **40** und **80**. Führen sie in die Felder eingeben oder indem Sie den Schieberegler.  
 ![Skalierung - Automatische Skalierung konfigurieren](./media/app-service-web-get-started/scale-out-configure.png)

    Auf Grundlage dieser Konfiguration skaliert Ihrer Anwendung automatisch Wenn CPU-Auslastung 80 % CPU-Auslastung wird unter 40 % skaliert.

5. **Klicken Sie in der Menüleiste.**

Herzlichen Glückwunsch, Ihre app automatische Skalierung.

Blatt **Einstellungen** aufgefallen, denen viel mehr wie möglich:

- Manuelles skalieren Sie auf eine bestimmte Anzahl von Instanzen
- Andere Leistungsmetriken wie Prozentsatz oder Datenträger Speicherwarteschlange skalieren
- Passen Sie Skalierung an, eine Performance-Regel ausgelöst
- Automatische Skalierung nach einem Zeitplan
- Verhalten Sie automatische Skalierung für ein zukünftiges Ereignis

Weitere Informationen zu app Skalierung finden Sie unter [Skalieren Ihrer Anwendung in Azure](../app-service-web/web-sites-scale.md). Weitere Informationen zu skalieren finden Sie unter [Skalieren Instanzenzahl manuell oder automatisch](../monitoring-and-diagnostics/insights-how-to-scale.md).

## <a name="receive-alerts-for-your-app"></a>Benachrichtigung für Ihre Anwendung

Da Ihre app Skalierung, was geschieht, wenn die maximale Instanzenzahl (2) erreicht und CPU über gewünschte Auslastung (80 %)?
Sie können eine Warnung (Weitere Informationen zur [Benachrichtigung empfangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)) einrichten, informiert Sie über diese Situation so weiter oben ein-und Ihre Anwendung beispielsweise skalieren können. Schnell richten Sie eine Warnung für dieses Szenario.

1. Portal Blatt Ihrer App klicken Sie auf **Extras** > **Alarme**.  
    ![Alerts - Einstellungen-Blades](./media/app-service-web-get-started/alert-settings.png)

2. Klicken Sie auf **Warnung hinzufügen**. Wählen Sie im Feld **Ressource** die Ressource mit **(Serverfarms)**. Das ist Ihre App Service-Plan.  
    ![Alerts - hinzufügen Warnung für App Service-plan](./media/app-service-web-get-started/alert-add.png)

3. Geben Sie **Namen** wie `CPU Maxed`, **Metrik** als **Prozentwert der CPU**und **Schwellenwert** `90`wählen Sie **e-Mail-Besitzer und Beteiligte Personen, Reader**, und klicken Sie auf **OK**.   
 ![Alerts - konfigurieren Benachrichtigung](./media/app-service-web-get-started/alert-configure.png)

    Abschluss Azure erstellen die Warnung sehen Sie sie im **Alerts** -Blade.  
    ![Alerts - fertig anzeigen](./media/app-service-web-get-started/alert-done.png)

Herzlichen Glückwunsch, Sie erhalten jetzt Alerts.

Diese Warnung Einstellung prüft CPU-Auslastung von fünf Minuten. Wenn diese Nummer über 90 % geht, erhalten Sie eine e-Mail-Benachrichtigung mit jeder, der berechtigt ist. An alle, die die Benachrichtigung zurück zum Portal Blade Ihrer App und klicken Sie auf die Schaltfläche **Zugriff** autorisiert ist.  
![Alerts wer](./media/app-service-web-get-started/alert-rbac.png)

Sie sollten sehen, dass **Abonnement-Admins** bereits der **Besitzer** der app. Dieser Gruppe zählen Sie, wenn Sie das Kontoadministrator Ihres Azure-Abonnements (z. B. Ihr Testabonnement). Weitere Informationen zu Azure rollenbasierte Zugriffskontrolle finden Sie unter [Azure_Role-Based Access Control](../active-directory/role-based-access-control-configure.md).

> [AZURE.NOTE] Warnregeln ist Azure. Weitere Informationen finden Sie unter [Benachrichtigung empfangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

## <a name="next-steps"></a>Nächste Schritte

Auf dem Weg zum Konfigurieren der Warnung Sie umfassende Tools **Tools** Blatt aufgefallen. Hier können Sie beheben Probleme Überwachen der Leistung, Schwachstellen prüfen, Ressourcen interagieren mit der VM-Konsole und nützliche Erweiterung hinzufügen. Wir laden Sie auf jeweils an die einfache, aber leistungsstarke Tools zu ermitteln.

Erfahren Sie mehr mit Ihrer Anwendung bereitgestellt. Hier ist nur ein Auszug:

- [Kaufen und konfigurieren Sie einen benutzerdefinierten Domänennamen](custom-dns-web-site-buydomains-web-app.md) - kaufen eine attraktive Domäne Ihrer Anwendung anstelle der *. *.azurewebsites.NET-Domäne. Oder eine Domäne, die Sie bereits verwenden.
- [Einrichten von Stagingumgebungen](web-sites-staged-publishing.md) - app staging URL vor dem Einfügen in die Produktion bereitstellen. Aktualisieren Sie Ihrer Web app vertrauen. Richten Sie eine aufwendige DevOps Lösung mit mehreren Bereitstellung Steckplätzen.
- [Einrichten der kontinuierlichen Bereitstellung](app-service-continuous-deployment.md) - Bereitstellung in Ihrem Quellcodeverwaltungssystem integriert. Mit jeder Commit in Azure bereitstellen.
- [Zugriff auf lokale Ressourcen](web-sites-hybrid-connection-get-started.md) - Zugriff auf eine vorhandene lokale Datenbank oder CRM-System.
- [Sichern Sie Ihre app](web-sites-backup.md) - Back einrichten und Wiederherstellung für Ihrer Anwendung. Unerwartete Fehler bereiten Sie vor und behoben Sie werden können.
- [Aktivieren von Diagnoseprotokollen](web-sites-enable-diagnostic-log.md) - die IIS-Protokolle von Azure oder Anwendung Spuren gelesen. In einem Stream lesen, herunterladen oder diese [Anwendung Einblicke](../application-insights/app-insights-overview.md) in schlüsselfertige Analyse port.
- [Ihre app Sicherheitsrisiken Scannen](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) -
Ihrer Anwendung moderner Bedrohungen Service von [Folie Sicherheit](https://www.tinfoilsecurity.com/)überprüfen.
- [Ausführen von Hintergrundjobs](../azure-functions/functions-overview.md) - Aufträge für die Datenverarbeitung, reporting usw..
- [Funktionsweise App Service](../app-service/app-service-how-works-readme.md)
