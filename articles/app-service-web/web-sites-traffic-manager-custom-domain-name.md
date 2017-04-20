<properties
    pageTitle="Konfigurieren Sie einen benutzerdefinierten Domänennamen für eine Webanwendung in Azure App Service, der Traffic Manager für den Lastenausgleich verwendet."
    description="Verwenden Sie einen benutzerdefinierten Domänennamen für eine Web-app in Azure App-Dienst mit Traffic Manager für den Lastenausgleich."
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
    ms.date="09/20/2016"
    ms.author="robmcm"/>

# <a name="configuring-a-custom-domain-name-for-a-web-app-in-azure-app-service-using-traffic-manager"></a>Konfigurieren einen benutzerdefinierten Domänennamen für eine Webanwendung in Azure App Service mit Traffic Manager

[AZURE.INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[AZURE.INCLUDE [intro](../../includes/custom-dns-web-site-intro-traffic-manager.md)]

Dieser Artikel beschreibt generische Azure App Service mit benutzerdefinierten Domänennamen, Traffic Manager für den Netzwerklastenausgleich verwenden.

[AZURE.INCLUDE [tmwebsitefooter](../../includes/custom-dns-web-site-traffic-manager-notes.md)]

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>
## <a name="understanding-dns-records"></a>Grundlegendes zu DNS-Datensätze

[AZURE.INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-traffic-manager.md)]

<a name="bkmk_configsharedmode"></a>
## <a name="configure-your-web-apps-for-standard-mode"></a>Konfigurieren der Web-apps für Standardmodus

[AZURE.INCLUDE [modes](../../includes/custom-dns-web-site-modes-traffic-manager.md)]

<a name="bkmk_configurecname"></a>
## <a name="add-a-dns-record-for-your-custom-domain"></a>Einen DNS-Eintrag für Ihre benutzerdefinierte Domain hinzufügen

> [AZURE.NOTE] Wenn Sie Domäne über Azure App Service Web Apps erworben haben Schritte überspringen und abschließend [Domäne für Web Apps kaufen](custom-dns-web-site-buydomains-web-app.md) Artikel verweisen.

Um eine Webanwendung in Azure App Service Ihre benutzerdefinierte Domain zuzuordnen, müssen Sie hinzufügen einen neuen Eintrag in der DNS-Tabelle für Ihre benutzerdefinierte Domain mit dem Erwerb der Domänenname von Domainregistrierungsstelle-Tools. Gehen Sie zu suchen und die DNS-Tools.

1. Melden Sie sich bei Ihrem Konto bei Ihrer Registrierungsstelle und eine Seite zum Verwalten von DNS-Datensätzen suchen. Suchen Sie nach Links oder Bereiche der Website als **Domänennamen**, **DNS**oder **Name-Server-Management**. Oft ein Link zu dieser Seite finden Sie Ihre Kontoinformationen anzeigen und dann eine Verknüpfung wie **Meine Domänen**suchen.

1. Wenn die Verwaltungsseite für den Domänenname gefunden haben, suchen Sie einen Link, der Sie die DNS-Datensätze bearbeiten kann. Dies kann als **Zonendatei** **DNS-Einträge**oder als **Advanced** Configuration Link aufgeführt.

    * Seite haben wahrscheinlich einige bereits erstellter Datensätze, wie ein Eintrag zuordnen '**@**'oder'\*' mit 'Domäne Parkplatz Seite. Sie enthalten auch Einträge für allgemeine Unterdomänen wie **Www**.
    * Die Seite erwähnen **CNAME-Einträge**oder bieten eine Dropdownliste, um einen Datensatz auszuwählen. Es kann auch andere Datensätze wie **ein** und **MX-Datensätze**erwähnen. In einigen Fällen werden CNAME-Einträge von anderen Namen wie einen **Aliaseintrag**aufgerufen.
    * Die Seite haben auch Felder, mit denen Sie **Zuordnung** von **Host** oder **Domänennamen** an einen anderen Domänennamen ein.

1. Die Einzelheiten der jeder Registrierungsstelle unterschiedlich, im Allgemeinen Sie zuordnen *aus* Ihren benutzerdefinierten Domänennamen (z. B. **contoso.com**) *,* der Traffic Manager-Domänenname (**contoso.trafficmanager.net**), die für Ihre Webanwendung verwendet wird.

    > [AZURE.NOTE] Wenn ein Datensatz bereits verwendet wird und präventiv apps gebunden müssen, können Sie auch einen zusätzlichen CNAME-Eintrag erstellen. Z. B. **www.contoso.com** präventiv an Ihrer Anwendung binden, erstellen Sie einen CNAME-Eintrag aus **awverify.www** , **contoso.trafficmanager.net**. Sie können Ihrer Anwendung dann "www.contoso.com" hinzufügen, ohne den CNAME-Eintrag "Www". Weitere Informationen finden Sie unter [Erstellen von DNS-Datensätzen für eine Webanwendung in einer benutzerdefinierten Domäne][CREATEDNS].

1. Wenn Sie fertig sind, hinzufügen oder Ändern von DNS-Einträgen in der Registrierung Speichern der.

<a name="enabledomain"></a>
## <a name="enable-traffic-manager"></a>Traffic Manager aktivieren

[AZURE.INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-traffic-manager.md)]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Node.js Developer Center](/develop/nodejs/).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[CREATEDNS]: ../dns/dns-web-sites-custom-domain.md
