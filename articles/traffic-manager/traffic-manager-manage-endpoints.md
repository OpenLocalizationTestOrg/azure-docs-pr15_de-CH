<properties
    pageTitle="Endpunkte in Azure Traffic Manager verwalten | Microsoft Azure"
    description="Dieser Artikel hilft Ihnen beim Hinzufügen, entfernen, aktivieren und Deaktivieren von Azure Traffic Manager Endpunkte."
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="add-disable-enable-or-delete-endpoints"></a>Hinzufügen, deaktivieren, aktivieren oder Löschen von Endpunkten

Web Apps-Feature in Azure App Service bietet bereits Failover und Routingfunktionen Roundrobin-Datenverkehr für Websites in einem Rechenzentrum, unabhängig von der Website. Azure Traffic Manager können Sie Failover und Streckenführung Round Robin für Websites und Cloud in verschiedenen Rechenzentren an. Der erste Schritt notwendig, dass die Funktionalität den Cloud Service oder Website-Endpunkt Traffic Manager hinzufügen.

>[AZURE.NOTE]  Erläutert, wie das klassische Portal verwenden. Azure-Verwaltungsportal unterstützt nur die Erstellung und Zuweisung von Cloud-Services und webapps als Endpunkte. Das neue [Azure-Portal](https://portal.azure.com) ist die bevorzugte Schnittstelle.

Sie können auch einzelne Endpunkte, die Teil eines Profils Traffic Manager deaktivieren. Deaktivieren eines Endpunkts als Teil des Profils bleibt, aber das Profil verhält, als wäre der Endpunkt nicht darin enthalten ist. Diese Aktion ist vorübergehend entfernen einen Endpunkt im Wartungsmodus oder bereitgestellt wird. Sobald der Endpunkt wieder ist, kann es aktiviert werden.

>[AZURE.NOTE] Deaktivieren eines Endpunkts hat nichts mit der Bereitstellung in Azure. Fehlerfreier Endpunkt bleibt, auch wenn in Traffic Manager deaktiviert Datenverkehr empfangen. Deaktivieren eines Endpunkts in einem Profil wirkt sich außerdem nicht auf seinen Status in einem anderen Profil aus.

## <a name="to-add-a-cloud-service-or-website-endpoint"></a>Um einen Cloud-Dienst oder Website Endpunkt hinzufügen

1. Suchen Sie im Bereich Traffic Manager im klassischen Azure-Portal Traffic Manager-Profil mit Endpunkt-Einstellung, die Sie ändern möchten. Um die Einstellungen zu öffnen, klicken Sie auf den Pfeil rechts neben dem Profilnamen.
2. Klicken Sie oben auf der Seite **Endpunkte** zum Anzeigen der Endpunkte, die bereits Teil der Konfiguration.
3. Klicken Sie am unteren Rand der Seite auf **Hinzufügen** zum Aufrufen der Seite **Endpunkte hinzufügen** . Standardmäßig enthält die Seite die Clouddienste unter **Endpunkte**.
4. Wählen Sie für Cloud-Services Cloud-Dienste in der Liste als Endpunkte für dieses Profil hinzufügen. Deaktivieren der Cloud Service Name entfernt aus der Liste der Endpunkte.
5. Websites klicken Sie auf die Dropdown-Liste **Diensttyp** und wählen Sie **WebApp**.
6. Wählen Sie die Websites in der Liste als Endpunkte für dieses Profil hinzufügen. Der Name der Website löschen entfernt aus der Liste der Endpunkte. Sie können nur eine Website pro Azure-Rechenzentrum (Region) auswählen. Wenn Sie die erste Website auswählen, werden Webseiten im gleichen Datencenter nicht ausgewählt Beachten Sie, dass nur Websites aufgeführt sind.
7. Die Endpunkte für dieses Profil auszuwählen, klicken Sie auf das Häkchen unten rechts, Ihre Änderungen zu speichern.

>[AZURE.NOTE] Nach dem Hinzufügen oder entfernen einen Endpunkt aus einem Profil Methode *Failover* Datenverkehr weiterleiten die Prioritätenliste Failover kann nicht bestellt werden sie wünschen. Sie können die Reihenfolge der Prioritätenliste Failover auf der Seite Konfiguration anpassen. Weitere Informationen finden Sie unter [Konfigurieren Failover Datenverkehr weiterleiten](traffic-manager-configure-failover-routing-method.md).

## <a name="to-disable-an-endpoint"></a>So deaktivieren Sie einen Endpunkt

1. Suchen Sie im Bereich Traffic Manager im klassischen Azure-Portal Traffic Manager-Profil mit Endpunkt-Einstellung, die Sie ändern möchten. Um die Einstellungen zu öffnen, klicken Sie auf den Pfeil rechts neben dem Profilnamen.
2. Klicken Sie oben auf der Seite **Endpunkte** die Endpunkte anzeigen, die in der Konfiguration enthalten sind.
3. Klicken Sie auf den Endpunkt, den Sie deaktivieren möchten, und klicken Sie am unteren Rand der Seite **Deaktivieren** .
4. Clients weiterhin zum Senden von Datenverkehr an den Endpunkt für die Dauer der Time-to-Live (TTL). Sie können die Gültigkeitsdauer auf der Seite Konfiguration des Profils Traffic Manager ändern.

## <a name="to-enable-an-endpoint"></a>So aktivieren Sie einen Endpunkt

1. Suchen Sie im Bereich Traffic Manager im klassischen Azure-Portal Traffic Manager-Profil mit Endpunkt-Einstellung, die Sie ändern möchten. Um die Einstellungen zu öffnen, klicken Sie auf den Pfeil rechts neben dem Profilnamen.
2. Klicken Sie oben auf der Seite **Endpunkte** die Endpunkte anzeigen, die in der Konfiguration enthalten sind.
3. Klicken Sie auf den Endpunkt, den Sie aktivieren möchten, und klicken Sie am unteren Rand der Seite **Aktivieren** .
4. Clients werden gemäß dem Profil aktiviert Endpunkt geleitet.

## <a name="to-delete-a-cloud-service-or-website-endpoint"></a>So löschen Sie einen Cloud-Dienst oder Website Endpunkt

1. Suchen Sie im Bereich Traffic Manager im klassischen Azure-Portal Traffic Manager-Profil mit Endpunkt-Einstellung, die Sie ändern möchten. Um die Einstellungen zu öffnen, klicken Sie auf den Pfeil rechts neben dem Profilnamen.
2. Klicken Sie oben auf der Seite **Endpunkte** zum Anzeigen der Endpunkte, die bereits Teil der Konfiguration.
3. Klicken Sie auf der Seite Endpunkte auf den Endpunkt, den aus dem Profil löschen möchten.
4. Klicken Sie am unteren Rand der Seite auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

* [Traffic Manager Profile verwalten](traffic-manager-manage-profiles.md)
* [Konfigurieren Sie Verteilermethoden](traffic-manager-configure-routing-method.md)
* [Problembehandlung bei Traffic Manager verminderten Zustand](traffic-manager-troubleshooting-degraded.md)
* [Traffic Manager Performance-Aspekte](traffic-manager-performance-considerations.md)
* [Operationen auf Traffic Manager (REST-API-Referenz)](http://go.microsoft.com/fwlink/p/?LinkID=313584)
