<properties
    pageTitle="Häufig gestellte Fragen zur ClearDB MySql-Datenbanken mit Azure App Service | Microsoft Azure"
    description="Antworten auf häufig gestellte Fragen zu Azure App Service mit ClearDB MySQL-Datenbanken."
    documentationCenter="php"
    services=""
    authors="sunbuild"
    manager="yochayk"
    editor=""
    tags="mysql"/>

<tags
    ms.service="multiple"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="sumuth"/>

# <a name="faq-for-cleardb-mysql-databases-with-azure-app-service"></a>Häufig gestellte Fragen zur ClearDB MySql-Datenbanken mit Azure App Service

Dieser FAQ beantwortet häufige gestellte Fragen und ClearDB MySQL-Datenbanken für Azure Web Apps erwerben.

## <a name="what-options-do-i-have-for-mysql-on-azure"></a>Welche Optionen für MySQL Azure habe?

Sie haben mehrere Optionen:

* [ClearDB gemeinsame MySQL-Datenbank](/marketplace/partners/cleardb/databases/)

* [ClearDB MySQL Premium-Clustern](/marketplace/partners/cleardb-clusters/cluster/)

* [MySQL Cluster auf eine Azure-VM](https://github.com/azure/azure-quickstart-templates/tree/master/mysql-replication)

* [Instanz von MySQL auf Azure-VM](virtual-machines/virtual-machines-windows-classic-mysql-2008r2.md)

ClearDB ist eine dem Dienst MySQL und MySQL-Infrastruktur verwaltet. Wenn eigene MySQL Cluster-Datenbank eine Azure Virtual Machine oder ausführen MySQL-Server einrichten und mit Patches aktualisiert werden können.

## <a name="do-i-need-a-credit-card-for-the-web-app--mysql-template-in-the-azure-marketplace"></a>Benötige ich eine Kreditkarte für WebApp + MySQL-Vorlage in Azure Marketplace?

Dies hängt vom Typ des Abonnements verwendete. Hier sind einige häufig verwendete abonnementtypen:

* [Sie](/offers/ms-azr-0003p/): eine Kreditkarte und beim Kauf Ihrer Kreditkarte bezahlten MySQL-Datenbank benötigt.

* [Kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/): Credits für mit Microsoft Azure services, erlaubt jedoch kein Kauf von Fremdanbieter-Ressourcen enthält. Erwerben Sie Fremdanbieterdienste oder eine bezahlte MySQL-Datenbank, die Sie ein Abonnement aktiviert Kreditkarte verwenden müssen. Web Apps können Sie eine kostenlose ClearDB MySQL-Datenbank erstellen.

* [MSDN-Abonnements](/pricing/member-offers/msdn-benefits/) und **MSDN Dev Test bezahlen Sie gehen**: wie kostenlose Testversion ein MSDN-Abonnements benötigen Sie eine Kreditkarte bezahlte MySQL-Lösung von ClearDB erwerben.

* [Konzernvertrag (EA)](/pricing/enterprise-agreement/): EA-Kunden werden anhand ihrer EA jedes Quartal für alle ihre Einkäufe Azure Marketplace (dritte) in einer separaten, konsolidierte Rechnung berechnet. Sie werden außerhalb der finanziellen Zusage für Marktplatz Einkäufe berechnet. Beachten Sie, dass zu diesem Zeitpunkt Azure-Speicher nicht für Kunden in Aserbaidschan, Kroatien, Norwegen und Puerto Rico. 

*  [DreamSpark](https://www.dreamspark.com/Product/Product.aspx?productid=99): nur freie ClearDB Datenbanken für Web Apps erstellen. Es gibt keine Beschränkung für die Anzahl der freien ClearDB MySQL-Datenbanken, die Sie erstellen können. Beachten Sie, dass freie Datenbanken nicht für Produktion Web apps verwendet werden wie dieser Dienst nur für Testversion vorgesehen ist.

## <a name="why-was-i-charged-350-for-a-web-app--mysql-from-the-azure-marketplace"></a>Warum wurde 3,50 $ Web app + MySQL Azure Marketplace berechnet?

Die Standard-Datenbankoption ist Titan 3,50 $ ist. Die Kosten nicht beim Erstellen der Datenbank angezeigt und Kauf möglicherweise versehentlich eine Datenbank, die, der Sie möchten nicht. Wir suchen eine Möglichkeit zu verbessern, aber Sie müssen alle Ihre ausgewählten Tarifen für WebApp und die Datenbank erst dann überprüfen, bevor **Erstellen** klicken und die Bereitstellung von Ressourcen.

## <a name="i-am-running-mysql-on-my-own-azure-virtual-machine-can-i-connect-my-azure-web-app-to-my-database"></a>Ich führe MySQL auf eigene Azure virtuellen Computer. Kann ich meine Azure Web app zur Datenbank verbinden?

Ja. Sie können Ihrer Anwendung mit der Datenbank verbinden, als Azure-VM RAS Ihrer Anwendung gegeben hat. Weitere Informationen finden Sie unter [MySQL auf einem virtuellen Computer installieren](virtual-machines/virtual-machines-windows-classic-mysql-2008r2.md).

## <a name="in-which-countries-are-cleardb-premium-mysql-clusters-supported"></a>In welchen Ländern sind ClearDB Premium MySQL Cluster unterstützt?

[ClearDB Premium MySQL Cluster](/marketplace/partners/cleardb-clusters/cluster/) stehen in allen Azure Regionen weltweit mit Ausnahme von Indien, Australien, Brasilien Süd und China.

## <a name="can-i-create-a-new-cluster-prior-to-creating-a-database-with-cleardb-premium-cluster-solution"></a>Kann ich einen neuen Cluster vor dem Erstellen einer Datenbank mit ClearDB Premium-Clusterlösung erstellen?

Erstellen von leeren ClearDB-Clustern wird leider nicht unterstützt. Azure-Portal können Sie Datenbanken in einem Cluster erstellen ein neues Clusters gleichzeitig erstellen können.

## <a name="will-i-be-warned-if-i-try-to-delete-a-cleardb-mysql-database-that-is-in-use-by-one-of-my-applications"></a>Werden ich gewarnt, wenn der versuch ClearDB MySQL-Datenbank löschen, die verwendet wird um eins meiner Anwendung?

Nein, wird Azure keine Warnung löschen Marketplace kaufen, von denen die Anwendung abhängt.

## <a name="which-regions-can-i-create-cleardb-databases-in"></a>Welche Bereiche können ClearDB Datenbanken werden erstellt?

Azure Marketplace ist nicht verfügbar, um Kunden in Aserbaidschan, Kroatien, Norwegen oder Puerto Rico. ClearDB ist in diesen Gebieten nicht verfügbar.

## <a name="what-pricing-tier-should-i-choose-for-a-production-web-app-and-database"></a>Welche Tarif wähle ich für eine Produktion WebApp und die Datenbank?

Verwenden Sie Basic oder einen höheren Tarif für Web Apps. ClearDB empfehlen wir Saturn oder Jupiter Plan. Überprüfen Sie die Funktionen und Grenzen der einzelnen Tarif für [Web Apps](/pricing/details/app-service/) und [ClearDB MySQL-Datenbanken](/marketplace/partners/cleardb/databases/) auswählen, die Ihren Bedürfnissen entspricht.

## <a name="how-do-i-upgrade-my-cleardb-database-from-one-plan-to-another"></a>Wie aktualisiere ich meine ClearDB-Datenbank aus einem Plan in einen anderen?

In [Azure-Portal](https://portal.azure.com)können Sie einer freigegebenen hosting ClearDB skalieren. Lesen Sie diesen [Artikel](https://blogs.msdn.microsoft.com/appserviceteam/2016/10/06/upgrade-your-cleardb-mysql-database-in-azure-portal/) erfahren Sie mehr. Aktualisierung unterstützt nicht aktuell ClearDB Premium-Cluster im Azure-Portal.

## <a name="i-cant-see-my-cleardb-database-in-azure-portal"></a>ClearDB Datenbank in Azure-Portal nicht angezeigt?

Wenn wir Azure Ressource-Manager oder [Neue Azure-Portal](https://portal.azure.com)ClearDB Datenbank erstellen, werden in das [Alte Azure-Portal](https://manage.windowsazure.com)angezeigt. Um dies umgehen ist die Datenbank manuell zu Web app. Auch wenn ClearDB Datenbank erstellen in das [alte Portal](https://manage.windowsazure.com) wird möglicherweise nicht an die Datenbank in die [Neue Azure-Portal](https://portal.azure.com). Gibt es keine Umgehung Fragments.

## <a name="who-do-i-contact-for-support-when-my-database-is-down"></a>Die Unterstützung kontaktiere ich bei meiner Datenbank ausfällt?

Probleme im Zusammenhang mit Kontakt [ClearDB Unterstützung](https://www.cleardb.com/developers/help/support) für jede Datenbank. Bereiten Sie Ihre Azure Abonnementinformationen geben.

## <a name="can-i-create-additional-users-for-my-cleardb-mysql-database-cluster-solution"></a>Kann ich für meine ClearDB MySQL Datenbank Clusterlösung weitere Benutzer erstellen? 

Nein. Zusätzliche Benutzer kann nicht erstellt, aber zusätzliche Datenbanken auf der ClearDB-Datenbank-Cluster erstellen.  

## <a name="can-basicpro-series-databases-be-upgraded-in-place-similar-to-planetary-plans-today-on-cleardb-portal"></a>Kann Basic-Pro Serie Datenbanken aktualisiert direktes wie heute Planeten Pläne ClearDB Portal?

Ja, aktualisiert Basisgruppe Datenbanken werden direkte (grundlegende 60 über grundlegende 500). Pro Serie kann direkt (Pro 125 bis Pro 1000) außer Pro 60 aktualisiert. 60 Pro Datenbank derzeit Aktualisierung wird nicht unterstützt. 

## <a name="when-i-migrate-my-resources-from-one-subscription-to-another-does-my-cleardb-mysql-database-get-migrated-as-well"></a>Wenn ich meine Ressourcen von einem Abonnement zu einem anderen migrieren, migriert Meine ClearDB MySQL-Datenbank ebenfalls? 

Beim Ausführen der Ressourcenmigration über Abonnements anwenden [Grenzen](./app-service-web/app-service-move-resources.md) . ClearDB MySQL und einer Datenbank einen Drittanbieterdienst daher nicht migriert während der Migration der Azure-Abonnement. Wenn Sie nicht die Migration der MySQL-Datenbank vor dem Migrieren von Azure Ressourcen verwalten, können Ihre ClearDB MySQL-Datenbanken deaktiviert. Manuell migrieren Sie zuerst Datenbanken und führen Sie Azure-Abonnement Migration für Ihre Webanwendung aus. 

## <a name="can-i-transfer-a-cleardb-database-from-a-credit-card-subscription-to-an-ea-subscription"></a>Kann ich ClearDB Datenbank aus einer Kreditkarte eine EA-Abonnement übertragen?

ClearDB Datenbanken verwenden Kreditkartennummer, die vorhandenen Abonnements. Ein EA-Abonnement verwenden, Sie Ihre Daten in eine neue Datenbank migrieren müssen:

* Erwerben Sie eine neue ClearDB-Datenbank mit Ihrem EA-Abonnement.
* Migrieren der Daten in die neue Datenbank.
* Aktualisieren Sie die Anwendung für die neue Datenbank.
* Löschen der alten ClearDB-Datenbank.

Beim Erstellen einer neuen Web-Applikation mit MySQL (ClearDB) oder erstellen Sie eine MySQL-Datenbank (ClearDB) bestimmt Abonnement wählen Sie, wie Sie für den Service bezahlen. Mit einem Abonnement EA Sperren wir nicht die Beschaffung der Dienste von Drittanbietern wie ClearDB in Azure-Portal. EA-Abonnements werden außerhalb der finanziellen Zusage berechnet und Quartal und überfälligen abgerechnet werden. EA-Kunden müssten Zahlungsmethode wie eine Kreditkarte bezahlen für Dienste von Drittanbietern Marketplace einrichten.

## <a name="where-can-i-see-the-charges-for-cleardb-resources-in-an-ea-subscription"></a>Wo kann die Kosten für Ressourcen in einem Abonnement EA ClearDB sehen?

Direkte EA-Kunden fallen Azure Marketplace in Enterprise Portal angezeigt. Beachten Sie, dass alle Marketplace Einkäufe und Verbrauch außerhalb der finanziellen Zusage abgerechnet werden vierteljährlich und überfälligen berechnet. EA-Kunden direkt an die Fremdanbieter-Dienstanbieter haben und dazu eine Zahlungsmethode wie eine Kreditkarte mit ihrem EA-Konto aktivieren.

Indirekte EA finden ihre Azure Marketplace-Abonnements auf der Seite **Abonnements verwalten** von Enterprise Portal jedoch Preise ausgeblendet ist. Kunden sollten ihre LSP erhalten Sie Informationen über Marketplace.

Zugriff auf Azure Marketplace für Dienste von Drittanbietern kann Administratoren Registrierung EA Azure verwaltet werden. Sie können deaktivieren oder reaktivieren Zugriff 3rd Party Einkauf aus dem Speicher verwalten Konten und Abonnements im Bereich Konten im Enterprise Portal.

## <a name="who-do-i-contact-for-questions-about-my-bill-for-cleardb-services-in-my-ea-subscription"></a>Wen wende ich mich bei Fragen zu meiner Abrechnung für ClearDB Dienste in meinem EA-Abonnement?

Kundensupport [Enterprise](http://aka.ms/AzureEntSupport) bezüglich Abrechnung unter ihre EA-Registrierung. EA-Portal-Supportteam Beantwortung Ihrer Frage oder Ihr Problem beheben.

 



## <a name="more-information"></a>Weitere Informationen

[Häufig gestellte Fragen zur Azure Marketplace](/marketplace/faq/)
