<properties
    pageTitle="Referenz zum Navigieren in Azure-portal"
    description="Erfahren Sie verschiedene Benutzeroberflächen für App Service zwischen Verwaltungsportal und Azure-Portal"
    services="app-service"
    documentationCenter=""
    authors="jaime-espinosa"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/26/2016"
    ms.author="jaime-espinosa"/>

# <a name="reference-for-navigating-the-azure-portal"></a>Referenz zum Navigieren in Azure-portal

Azure Websites heißen jetzt [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). Wir aktualisieren alle diese Namensänderung widergespiegelt und Anleitung zum Azure-Portal-Dokumentation. Bis dieser Prozess abgeschlossen ist, können Sie dieses Dokument als Leitfaden für die Arbeit mit Web Apps in Azure-Portal verwenden.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 
 
## <a name="the-future-of-the-azure-classic-portal"></a>Die Zukunft der Azure-Verwaltungsportal

Die Brandingänderungen Azure-Verwaltungsportal bemerken werden, ist dieses Portal von Azure-Portal ersetzt. Als das Verwaltungsportal Auslaufen verlagert der Fokus für die Entwicklung neuer Azure-Portal. Alle anstehende neue Funktionen für Web Apps kommen in Azure-Portal. Nutzen Sie Azure-Verwaltungsportal zu Kunden haben Web Apps bieten.

## <a name="layout-differences-between-the-azure-classic-portal-and-azure-portal"></a>Layout Unterschiede zwischen Azure-Verwaltungsportal und Azure-Portal

Im klassischen Portal werden alle Azure-Dienste auf der linken Seite aufgelistet. Navigation im klassischen Portal folgt eine Baumstruktur, aus dem Dienst starten und in jedem Element navigieren. Diese Struktur funktioniert gut, wenn unabhängige Komponenten verwalten. Allerdings bei Azure sind eine Sammlung von miteinander verbundenen Diensten und diese Struktur ist ideal für die Arbeit mit Services nicht. 

Azure-Portal erleichtert die Applikationen End-to-End mit mehreren Diensten erstellen. Das Portal ist als *Reisen*organisiert. Eine *Reise* ist eine Reihe von *Blades*sind Container für die verschiedenen Komponenten. Beispiel: Einrichten von Automatische Skalierung für eine Webanwendung eine *Reise* führt mehrere klingen ist wie im folgenden Beispiel gezeigt: Blade **- Website** (die Blade-Titel noch nicht aktualisiert wurde um neue Begriffe verwenden) **Einstellungen** Blade- und Blade- **Skalierung** . Im Beispiel wird AutoSkalieren aufgebaut **CPU-Prozentsatz** Blade ist CPU-Verwendung abhängig. Die Komponenten in den *Blades* heißen *Teile*der Kacheln aussehen. 

![](./media/app-service-web-app-azure-portal/AutoScaling.png)

## <a name="navigation-example-create-a-web-app"></a>Navigationsbereich-Beispiel: erstellen eine Webanwendung

Erstellen neuer Web-apps ist weiterhin einfach wie 1-2-3. Die folgende Abbildung zeigt das Verwaltungsportal und Portal nebeneinander nachzuweisen, dass die Anzahl der Schritte benötigen eine Webanwendung und läuft nicht viel verändert. 

![](./media/app-service-web-app-azure-portal/CreateWebApp.png)

Im Portal können Sie aus den am häufigsten verwendeten Web-apps auch beliebte Gallery WordPress. Eine vollständige Liste der verfügbaren Programme finden Sie in [Azure Marketplace].

Beim Erstellen einer Webanwendung Geben Sie URL App Service-Plan und Speicherort im Portal wie im klassischen Portal führen. 

![](./media/app-service-web-app-azure-portal/CreateWebAppSettings.png)

Darüber hinaus ermöglicht das Portal andere allgemeine Einstellungen. Z. B. erleichtern [Ressourcengruppen](../azure-resource-manager/resource-group-overview.md) zu verwandten Azure Ressourcen. 

## <a name="navigation-example-settings-and-features"></a>Navigationsbereich-Beispiel: Einstellungen und Funktionen

Alle Optionen und Features sind nun in einem einzelnen Blade gruppiert aus denen navigieren können.

![](./media/app-service-web-app-azure-portal/WebAppSettings.png)

Beispielsweise können Sie benutzerdefinierte Domänen durch Blatt **Einstellungen** auf **Custom Domains und SSL** .

![](./media/app-service-web-app-azure-portal/ConfigureWebApp.png)

Klicken Sie zum Einrichten einer Überwachung Warnung auf **Anfragen und Fehler** und **Warnung hinzufügen**.

![](./media/app-service-web-app-azure-portal/Monitoring.png)

Aktivieren Sie Diagnose, um **Diagnoseprotokolle** **Einstellungen** Blatt.

![](./media/app-service-web-app-azure-portal/Diagnostics.png)
 
Um die Anwendung zu konfigurieren, klicken Sie auf **ApplicationSettings** **Einstellungen** -Blades. 

![](./media/app-service-web-app-azure-portal/AppSettingsPreview.png)

Außer dem Markennamen Punkte im Portal umbenannt oder anders gruppiert machen es leichter zu finden. Zum Beispiel unten ist ein Screenshot der entsprechenden Seite für die Anwendung im klassischen Portal Settings (**Konfigurieren**).

![](./media/app-service-web-app-azure-portal/AppSettings.png)

## <a name="more-resources"></a>Weitere Ressourcen

[Azure Portal]: https://portal.azure.com
[Azure Marketplace]: /marketplace/

>[AZURE.NOTE] Wenn Sie mit Azure App Service beginnen, bevor Sie sich für ein Azure-Konto, gehen Sie [Versuchen App Service](http://go.microsoft.com/fwlink/?LinkId=523751)sofort eine kurzlebige Starter Web app in App Service können Sie erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="whats-changed"></a>Was hat sich geändert
* Eine Anleitung zur Änderung von Websites zu App Service finden Sie unter: [Azure App Service und seine Auswirkung auf vorhandene Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)
 
