<properties
   pageTitle="Endpunkte in Azure Traffic Manager verwalten | Microsoft Azure"
   description="Dieser Artikel hilft Ihnen beim Hinzufügen, entfernen, aktivieren und Deaktivieren von Azure Traffic Manager Endpunkte."
   services="traffic-manager"
   documentationCenter=""
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/17/2016"
   ms.author="sewhee" />

# <a name="add-disable-enable-or-delete-endpoints"></a>Hinzufügen, deaktivieren, aktivieren oder Löschen von Endpunkten

Web Apps-Feature in Azure App Service bietet bereits Failover und Routingfunktionen Roundrobin-Datenverkehr für Websites in einem Rechenzentrum, unabhängig von der Website. Azure Traffic Manager können Sie Failover und Streckenführung Round Robin für Websites und Cloud in verschiedenen Rechenzentren an. Der erste Schritt notwendig, dass die Funktionalität den Cloud Service oder Website-Endpunkt Traffic Manager hinzufügen.

>[AZURE.NOTE] Traffic Manager Profile oder externe Standorte kann nicht als Endpunkte mithilfe von Azure-Verwaltungsportal hinzufügen werden. Sie müssen die REST-API- [Definition erstellen](http://go.microsoft.com/fwlink/p/?LinkId=400772) oder Windows PowerShell [Add-AzureTrafficManagerEndpoint](http://go.microsoft.com/fwlink/p/?LinkId=400774)verwenden.

Sie können auch einzelne Endpunkte, die Teil eines Profils Traffic Manager deaktivieren. Endpunkte umfassen Cloud-Dienste und Websites. Deaktivieren eines Endpunkts als Teil des Profils bleibt, aber das Profil verhält, als wäre der Endpunkt nicht darin enthalten ist. Diese Aktion ist hilfreich vorübergehend entfernt einen Endpunkt im Wartungsmodus oder bereitgestellt wird. Sobald der Endpunkt wieder ist, kann es aktiviert werden.

>[AZURE.NOTE] Deaktivieren eines Endpunkts hat nichts mit der Bereitstellung in Azure. Ein gesunder Endpunkt bleiben von Datenverkehr auch deaktivierte in Traffic Manager erhalten. Deaktivieren eines Endpunkts in einem Profil wirkt sich außerdem nicht auf seinen Status in einem anderen Profil aus.

## <a name="to-add-a-cloud-service-or-website-endpoint"></a>Um einen Cloud-Dienst oder Website Endpunkt hinzufügen


1. Im Traffic Manager im klassischen Azure-Portal der Traffic Manager Profil mit Endpunkt-Einstellung, die Sie ändern möchten, und klicken Sie dann auf den Pfeil rechts neben dem Profilnamen. Dies öffnet die Einstellungsseite für das Profil.
2. Klicken Sie oben auf der Seite **Endpunkte** zum Anzeigen der Endpunkte, die bereits Teil der Konfiguration.
3. Klicken Sie am unteren Rand der Seite auf **Hinzufügen** zum Aufrufen der Seite **Endpunkte hinzufügen** . Standardmäßig enthält die Seite die Clouddienste unter **Endpunkte**.
4. Wählen Sie für Cloud-Services Cloud-Dienste in der Liste aktivieren als Endpunkte für dieses Profil. Deaktivieren der Cloud Service Name entfernt aus der Liste der Endpunkte.
5. Websites klicken Sie auf die Dropdown-Liste **Diensttyp** und wählen Sie **WebApp**.
6. Wählen Sie die Websites in der Liste als Endpunkte für dieses Profil hinzufügen. Der Name der Website löschen entfernt aus der Liste der Endpunkte. Beachten Sie, dass Sie nur eine Website pro Azure-Rechenzentrum (Region) auswählen können. Wenn Sie eine Website in einem Datencenter, die mehrere Websites hostet auswählen, wenn Sie die erste Website auswählen, werden die anderen im gleichen Datencenter nicht ausgewählt. Beachten Sie, dass nur Websites aufgeführt sind.
7. Die Endpunkte für dieses Profil auszuwählen, klicken Sie auf das Häkchen unten rechts, Ihre Änderungen zu speichern.

>[AZURE.NOTE] Sollten Sie Methode routing Verkehr *Failover* nach dem Hinzufügen oder einen Endpunkt entfernen verwenden, anpassen Prioritätenliste Failover auf der Seite Konfiguration entsprechend die Failover Reihenfolge für die Konfiguration ein. Weitere Informationen finden Sie unter [Konfigurieren Failover Datenverkehr weiterleiten](traffic-manager-configure-failover-routing-method.md).

## <a name="to-disable-an-endpoint"></a>So deaktivieren Sie einen Endpunkt

1. Im Traffic Manager im klassischen Azure-Portal der Traffic Manager Profil mit Endpunkt-Einstellung, die Sie ändern möchten, und klicken Sie dann auf den Pfeil rechts neben dem Profilnamen. Dies öffnet die Einstellungsseite für das Profil.
2. Klicken Sie oben auf der Seite **Endpunkte** die Endpunkte anzeigen, die in der Konfiguration enthalten sind.
3. Klicken Sie auf den Endpunkt, den Sie deaktivieren möchten, und klicken Sie am unteren Rand der Seite **Deaktivieren** .
4. Zu den Endpunkt basierend auf den DNS--Gültigkeitsdauer (TTL) für den Domänennamen Traffic Manager konfiguriert wird anhalten. Sie können die Gültigkeitsdauer auf der Seite Konfiguration des Profils Traffic Manager ändern.

## <a name="to-enable-an-endpoint"></a>So aktivieren Sie einen Endpunkt

1. Im Traffic Manager im klassischen Azure-Portal der Traffic Manager Profil mit Endpunkt-Einstellung, die Sie ändern möchten, und klicken Sie dann auf den Pfeil rechts neben dem Profilnamen. Dies öffnet die Einstellungsseite für das Profil.
2. Klicken Sie oben auf der Seite **Endpunkte** die Endpunkte anzeigen, die in der Konfiguration enthalten sind.
3. Klicken Sie auf den Endpunkt, den Sie aktivieren möchten, und klicken Sie am unteren Rand der Seite **Aktivieren** .
4. Datenverkehr wird an den Dienst als durch das Profil fließen.

## <a name="to-delete-a-cloud-service-or-website-endpoint"></a>So löschen Sie einen Cloud-Dienst oder Website Endpunkt


1. Im Traffic Manager im klassischen Azure-Portal der Traffic Manager Profil mit Endpunkt-Einstellung, die Sie ändern möchten, und klicken Sie dann auf den Pfeil rechts neben dem Profilnamen. Dies öffnet die Einstellungsseite für das Profil.
2. Klicken Sie oben auf der Seite **Endpunkte** zum Anzeigen der Endpunkte, die bereits Teil der Konfiguration.
3. Klicken Sie auf der Seite Endpunkte auf den Endpunkt, den aus dem Profil löschen möchten.
4. Klicken Sie am unteren Rand der Seite auf **Löschen**.

>[AZURE.NOTE] Sie löschen nicht als Endpunkte mit klassischen Azure-Portal externe Speicherorte oder Traffic Manager Profile. Sie müssen Windows PowerShell verwenden. Weitere Informationen finden Sie unter [AzureTrafficManagerEndpoint entfernen](https://msdn.microsoft.com/library/dn690251.aspx).

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren Sie Failover routing](traffic-manager-configure-failover-routing-method.md)
- [Konfigurieren von Round-Robin Verteilungsmethode](traffic-manager-configure-round-robin-routing-method.md)
- [Routingmethode Leistung konfigurieren](traffic-manager-configure-performance-routing-method.md)
- [Problembehandlung bei Traffic Manager verminderten Zustand](traffic-manager-troubleshooting-degraded.md)
- [Operationen auf Traffic Manager (REST-API-Referenz)](http://go.microsoft.com/fwlink/p/?LinkID=313584)
