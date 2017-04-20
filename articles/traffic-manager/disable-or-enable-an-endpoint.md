<properties
   pageTitle="Deaktivieren oder aktivieren ein Endpunkts Traffic Manager | Microsoft Azure"
   description="Dieser Artikel hilft Endpunkte Profil Traffic Manager aktivieren oder deaktivieren."
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

# <a name="disable-or-enable-a-traffic-manager-endpoint"></a>Traffic Manager-Endpunkt aktivieren oder deaktivieren

Sie können auch einzelne Endpunkte, die Teil eines Profils Traffic Manager deaktivieren. Endpunkte umfassen Cloud-Dienste und Websites. Deaktivieren eines Endpunkts als Teil des Profils bleibt, aber das Profil verhält, als wäre der Endpunkt nicht darin enthalten ist. Diese Aktion ist hilfreich vorübergehend entfernt einen Endpunkt im Wartungsmodus oder bereitgestellt wird. Sobald der Endpunkt wieder ist werden es ermöglicht

>[AZURE.NOTE] **Deaktivieren eines Endpunkts hat nichts mit der Bereitstellung in Azure. Ein gesunder Endpunkt bleiben von Datenverkehr auch deaktivierte in Traffic Manager erhalten. Deaktivieren eines Endpunkts in einem Profil wirkt sich außerdem nicht auf seinen Status in einem anderen Profil aus.**

## <a name="to-disable-an-endpoint"></a>So deaktivieren Sie einen Endpunkt

1. Im Traffic Manager im klassischen Azure-Portal der Traffic Manager Profil mit Endpunkt-Einstellung, die Sie ändern möchten, und klicken Sie dann auf den Pfeil rechts neben dem Profilnamen. Dies öffnet die Einstellungsseite für das Profil.
1. Klicken Sie oben auf der Seite **Endpunkte** die Endpunkte anzeigen, die in der Konfiguration enthalten sind.
1. Klicken Sie auf den Endpunkt, den Sie deaktivieren möchten, und klicken Sie am unteren Rand der Seite **Deaktivieren** .
1. Zu den Endpunkt basierend auf den DNS--Gültigkeitsdauer (TTL) für den Domänennamen Traffic Manager konfiguriert wird anhalten. Sie können die Gültigkeitsdauer auf der Seite Konfiguration des Profils Traffic Manager ändern.

## <a name="to-enable-an-endpoint"></a>So aktivieren Sie einen Endpunkt


1. Im Traffic Manager im klassischen Azure-Portal der Traffic Manager Profil mit Endpunkt-Einstellung, die Sie ändern möchten, und klicken Sie dann auf den Pfeil rechts neben dem Profilnamen. Dies öffnet die Einstellungsseite für das Profil.
1. Klicken Sie oben auf der Seite **Endpunkte** die Endpunkte anzeigen, die in der Konfiguration enthalten sind.
1. Klicken Sie auf den Endpunkt, den Sie aktivieren möchten, und klicken Sie am unteren Rand der Seite **Aktivieren** .
1. Datenverkehr wird an den Dienst als durch das Profil fließen.

## <a name="next-steps"></a>Nächste Schritte

[Manager - deaktivieren, aktivieren oder Löschen eines Profils](disable-enable-or-delete-a-profile.md)

[Problembehandlung bei Traffic Manager verminderten Zustand](traffic-manager-troubleshooting-degraded.md)

[Traffic Manager Performance-Aspekte](traffic-manager-performance-considerations.md)