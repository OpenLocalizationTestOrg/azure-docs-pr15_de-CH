<properties
    pageTitle="Steuern von Azure app Webverkehr mit Azure Traffic Manager"
    description="Dieser Artikel enthält zusammenfassende Informationen für Azure Traffic Manager auf Azure webapps."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    writer="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/25/2016"
    ms.author="cephalin"/>

# <a name="controlling-azure-web-app-traffic-with-azure-traffic-manager"></a>Steuern von Azure app Webverkehr mit Azure Traffic Manager

> [AZURE.NOTE] Dieser Artikel enthält zusammenfassende Informationen für Microsoft Azure Traffic Manager auf Azure App Service Web Apps. Weitere Informationen über Azure Traffic Manager selbst finden auf die Links am Ende dieses Artikels.

## <a name="introduction"></a>Einführung
Azure Traffic Manager können Sie steuern, wie Anfragen von Webclients Web-apps in Azure App Service verteilt werden. Wenn ein Profil Azure Traffic Manager Web app Endpunkte hinzugefügt werden, verfolgt von Azure Traffic Manager Status Web Apps (ausgeführt, angehalten oder gelöscht), damit sie entscheiden kann, die diese Endpunkte passieren dürfen.

## <a name="load-balancing-methods"></a>Load-Balancing Methode
Azure Traffic Manager verwendet drei verschiedene Load balancing Methoden. Diese werden in der folgenden Liste beschrieben, wie sie Azure webapps betreffen.

* **Failover**: haben Sie Web app Clones in verschiedenen Bereichen können Sie mit dieser Methode kann ein Web app alle Webverkehr Client service konfigurieren und Konfigurieren einer anderen Webanwendung in einer anderen Region auf Datenverkehr bei die erste Web app nicht verfügbar ist.

* **Round-Robin**: haben Sie Web app Clones in verschiedenen Regionen, können Sie diese Methode Web apps in unterschiedlichen Regionen Verkehr gleichmäßig verteilt.

* **Leistung**: die Leistung Methode Datenverkehr auf Grundlage der kürzesten Roundtripzeit Clients verteilt. Performance-Methode kann für webapps innerhalb derselben Region oder in verschiedenen Bereichen verwendet werden.

##<a name="web-apps-and-traffic-manager-profiles"></a>Webapps und Traffic Manager Profile
Konfigurieren Sie das Steuerelement Webdatenverkehr app in Azure Traffic Manager mithilfe einer der drei Netzwerklastenausgleich zuvor beschriebene Methoden laden ein Profil erstellen, und fügen die Endpunkte (in diesem Fall webapps) zu dem Profil werden soll. Ihr Web app-Status (läuft, angehalten oder gelöscht) wird regelmäßig Profil mitgeteilt, damit Azure Traffic Manager Datenverkehr entsprechend weiterleiten können.

Bei Azure Traffic Manager mit Azure sollten beachten Sie folgende Punkte:

* Web app nur Bereitstellungen innerhalb derselben Region bietet Web Apps bereits Failover und Roundrobin-Funktionalität Web app-Modus.

* Für die Bereitstellung in derselben Region, die Web-Apps in Verbindung mit einem anderen Azure-Cloud-Dienst verwenden, können Sie beide Endpunkte hybride Szenarien kombinieren.

* Sie können in einem Profil nur einen Web app Endpunkt pro Region. Wenn Sie eine Webanwendung als Endpunkt für eine Region auswählen, werden den übrigen Web apps in diesem Bereich für das Profil nicht auswählbar

* Web app Endpunkte, die in Azure Traffic Manager-Profil angeben im Abschnitt **Domänennamen** auf der Seite konfigurieren Web App im Profil erscheint, aber werden nicht korrekt konfiguriert ist.

* Nach dem Hinzufügen einer Webanwendung zu einem Profil zeigt **Website-URL** im Schaltpult Web app Portalseite benutzerdefinierte Domänen-URL der Web-app, wenn eine eingerichtet haben. Andernfalls wird die Traffic Manager-Profil-URL angezeigt (z. B. `contoso.trafficmgr.com`). Sowohl der direkte Domänenname Web app und der Traffic Manager-URL werden auf Web app konfigurieren unter dem **Domänennamen** Abschnitt angezeigt.

* Die benutzerdefinierten Domänennamen funktioniert jedoch neben hinzufügen sie Web-apps, müssen auch DNS-Karte auf der Traffic Manager-URL konfigurieren. Informationen zum Einrichten einer benutzerdefinierten Domäne für Azure Web app finden Sie unter [konfigurieren einen benutzerdefinierten Domänennamen für eine Azure-Website](web-sites-custom-domain-name.md).

* Sie können nur webapps hinzufügen im Standardmodus Azure Traffic Manager-Profil.

## <a name="next-steps"></a>Nächste Schritte

Eine konzeptionelle und technische Übersicht von Azure Traffic Manager Übersicht [Traffic Manager](../traffic-manager/traffic-manager-overview.md).

Weitere Informationen zu Web Apps mit Traffic Manager finden Sie unter der Blogbeiträge [Mithilfe von Azure Traffic Manager Azure Websites](http://blogs.msdn.com/b/waws/archive/2014/03/18/using-windows-azure-traffic-manager-with-waws.aspx) und [Azure Traffic Manager können jetzt Azure Websites integrieren](https://azure.microsoft.com/blog/2014/03/27/azure-traffic-manager-can-now-integrate-with-azure-web-sites/).
