<properties
   pageTitle="Deaktivieren, aktivieren oder Löschen eines Profils Traffic Manager | Microsoft Azure"
   description="Dieser Artikel hilft Ihnen mit Ihrem Traffic Manager arbeiten."
   services="traffic-manager"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="sewhee" />
<!-- repub for nofollow -->

# <a name="disable-enable-or-delete-a-profile"></a>Deaktivieren, aktivieren oder Löschen eines Profils


Sie können ein Traffic Manager Profil deaktivieren, damit es nicht Benutzer konfigurierte Endpunkte bezeichnen. Beim Profil Traffic Manager und des Profils die Informationen im Profil deaktivieren bleibt intakt und Traffic Manager-Benutzeroberfläche bearbeitet werden kann. Wenn Sie das Profil erneut aktivieren möchten, können Sie problemlos tun in der Azure-Portal und Verweise fortgesetzt werden. Beim Erstellen eines Profils Traffic Manager in Azure-Portal wird es automatisch aktiviert.

## <a name="to-disable-a-profile"></a>Profil deaktivieren

1. Ändern des DNS-Ressourceneintrags auf dem Internet-DNS-Server und mit dem entsprechenden Datensatz auf einen anderen Namen oder die IP-Adresse eines bestimmten Lagerorts im Internet. In anderen Worten ändern Sie Ressourceneintrag auf dem Internet-DNS-Server, dass der Domänenname Ihres Profils Traffic Manager zeigt einen CNAME-Ressourceneintrag nicht mehr verwendet.
1. Die Endpunkte durch Einstellungen der Traffic Manager weitergeleitet wird anhalten.
1. Wählen Sie das Profil, das Sie deaktivieren möchten. Markieren Sie das Profil auf der Seite Traffic Manager aus Profil durch Klicken auf die Spalte neben dem Profilnamen. Klicken Sie nicht auf den Namen oder den Pfeil neben dem Namen wie diese Sie zur Einstellungsseite für das Profil.
1. Klicken Sie nach Auswahl des Profils am unteren Rand der Seite deaktivieren.

## <a name="to-enable-a-profile"></a>Profil aktivieren

1. Wählen Sie das Profil, das Sie aktivieren möchten. Markieren Sie das Profil auf der Seite Traffic Manager aus Profil durch Klicken auf die Spalte neben dem Profilnamen. Klicken Sie nicht auf den Namen oder den Pfeil neben dem Namen wie diese Sie zur Einstellungsseite für das Profil.
1. Aktivieren Sie nach Auswahl des Profils am unteren Rand der Seite.
1. Ändern des DNS-Ressourceneintrags auf dem Internet-DNS-Server verwenden Datensatztyp CNAME Ihren Firmennamen Domäne den Domänennamen Ihres Profils Traffic Manager zugeordnet. Weitere Informationen finden Sie unter [Punkt einer Unternehmensdomäne Internet Traffic Manager-Domäne](traffic-manager-point-internet-domain.md).
1. Datenverkehr wird angewiesen, die Endpunkte wieder gestartet.

## <a name="delete-a-profile"></a>Löschen eines Profils


1. Sicherstellen Sie, dass der Ressourceneintrag für den Internet-DNS-Server nicht mehr CNAME-Ressourceneintrags verwendet, der auf den Domänennamen Ihres Profils Traffic Manager zeigt.
1. Wählen Sie das Profil, das Sie löschen möchten. Markieren Sie das Profil auf der Seite Traffic Manager aus das Profil
1. Klicken Sie auf die Spalte neben dem Profil. Klicken Sie nicht auf den Namen oder den Pfeil neben dem Namen wie diese Sie zur Einstellungsseite für das Profil.
1. Klicken Sie nach Auswahl des Profils löschen am unteren Rand der Seite.

## <a name="next-steps"></a>Nächste Schritte

[Traffic Manager - deaktivieren oder Aktivieren eines Endpunkts](disable-or-enable-an-endpoint.md)

[Konfigurieren Sie Failover routing](traffic-manager-configure-failover-routing-method.md)

[Konfigurieren von Round-Robin Verteilungsmethode](traffic-manager-configure-round-robin-routing-method.md)

[Routingmethode Leistung konfigurieren](traffic-manager-configure-performance-routing-method.md)

[Problembehandlung bei Traffic Manager verminderten Zustand](traffic-manager-troubleshooting-degraded.md)

