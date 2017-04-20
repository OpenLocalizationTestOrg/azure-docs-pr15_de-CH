<properties
    pageTitle="Azure Traffic Manager Profile verwalten | Microsoft Azure"
    description="Dieser Artikel soll Ihnen erstellen, deaktivieren Sie aktivieren, löschen und Azure Traffic Manager Profil anzeigen."
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="manage-an-azure-traffic-manager-profile"></a>Ein Azure Traffic Manager-Profil verwalten

Traffic Manager Profile Methoden routing von Datenverkehr steuern Sie die Verteilung des Datenverkehrs auf die Cloud-Services oder Website-Endpunkte. Erläutert das Erstellen und Verwalten dieser Profile.

## <a name="create-a-traffic-manager-profile-using-quick-create"></a>Traffic Manager Profil mit Firma erstellen

Traffic Manager-Profil können schnell im klassischen Azure-Portal mit schnellen erstellen. Schnell erstellen können Sie Profile mit grundlegenden erstellen. Allerdings mithilfe nicht Schnellerfassungsformular Einstellungen wie Endpunkte (Cloud-Dienste und Websites), dem Failover für die Routingmethode Failover Datenverkehr oder Einstellung Überwachung. Nach dem Erstellen Ihres Profils können Sie diese klassischen Azure-Portal konfigurieren. Traffic Manager unterstützt bis zu 200 Endpunkte pro Profil. Die meisten Szenarien erfordern jedoch nur wenige Endpunkte.

### <a name="to-create-a-traffic-manager-profile"></a>Traffic Manager-Profil erstellen

1. **Bereitstellen Sie die Cloud-Dienste und Websites für Ihre produktionsumgebung.** Weitere Informationen zu Cloud-Diensten finden Sie unter [Cloud-Dienste](http://go.microsoft.com/fwlink/p/?LinkId=314074). Weitere Informationen zu Websites finden Sie auf [Websites](http://go.microsoft.com/fwlink/p/?LinkId=393327).

2. **Melden Sie sich im klassischen Azure-Portal.** Klicken Sie unten links im Portal **neu** **Network Services > Traffic Manager**, und klicken Sie dann auf Konfigurieren Ihres Profils **Schnell erstellen** .
3. **Konfigurieren Sie das DNS-Präfix.** Geben Sie Ihr Profil Traffic Manager einen eindeutigen DNS Präfix. Sie können das Präfix für einen Domänennamen Traffic Manager.
4. **Wählen Sie das Abonnement.** Wählen Sie das entsprechende Azure-Abonnement. Jedes Profil ist ein einzelnes Abonnement zugeordnet. Wenn Sie nur über ein Abonnement verfügen, wird diese Option nicht angezeigt.
5. **Wählen Sie die Datenverkehr weiterleiten.** Wählen Sie die Datenverkehr routing **routing Richtlinie**Datenverkehr. Weitere Informationen zum Datenverkehr Verteilermethoden anzeigen Sie [Verteilermethoden für Datenverkehr über Traffic Manager](traffic-manager-routing-methods.md)
6. **Klicken Sie auf "Erstellen", um das Profil zu erstellen**. Wenn die Konfiguration abgeschlossen ist, finden Sie Ihr Profil im Bereich Traffic Manager im klassischen Azure-Portal.
7. **Konfigurieren Sie Endpunkte und Überwachung sowie zusätzliche im klassischen Azure-Portal.** Mit schnellen Erstellen konfiguriert nur Standardeinstellungen. Es ist notwendig, weitere Einstellungen wie die Liste der Endpunkte und die Reihenfolge der Endpunkt-Failover.


## <a name="disable-enable-or-delete-a-profile"></a>Deaktivieren, aktivieren oder Löschen eines Profils

Sie können ein vorhandenes Profil deaktivieren, so dass Traffic Manager Benutzeranfragen an die konfigurierten Endpunkte bezieht sich nicht. Beim Deaktivieren eines Profils Traffic Manager das Profil im Profil enthaltenen Informationen bleiben und Traffic Manager-Benutzeroberfläche bearbeitet werden können.  Verweise auf fortgesetzt, wenn Sie das Profil erneut aktivieren. Beim Erstellen eines Profils Traffic Manager im klassischen Azure-Portal wird es automatisch aktiviert. Wenn Sie sich entscheiden, ein Profil nicht mehr erforderlich ist, können Sie es löschen.

### <a name="to-disable-a-profile"></a>Profil deaktivieren

1. Wenn Sie einen benutzerdefinierten Domänennamen verwenden, ändern Sie den CNAME-Eintrag auf dem Internet-DNS-Server, sodass nicht mehr auf Ihr Profil Traffic Manager zeigt.
2. Datenverkehr beendet die Endpunkte durch Einstellungen der Traffic Manager weitergeleitet.
3. Wählen Sie das Profil, das Sie deaktivieren möchten. Markieren Sie auf der Seite Traffic Manager das Profil durch Klicken auf die Spalte neben dem Profilnamen. Hinweis auf den Namen oder den Pfeil neben dem Namen wird die Einstellungsseite für das Profil geöffnet.
4. Klicken Sie nach Auswahl des Profils am unteren Rand der Seite **Deaktivieren** .

### <a name="to-enable-a-profile"></a>Profil aktivieren

1. Wählen Sie das Profil, das Sie deaktivieren möchten. Markieren Sie auf der Seite Traffic Manager das Profil durch Klicken auf die Spalte neben dem Profilnamen. Hinweis auf den Namen oder den Pfeil neben dem Namen wird die Einstellungsseite für das Profil geöffnet.
2. Klicken Sie nach Auswahl des Profils am unteren Rand der Seite **Aktivieren** .
3. Wenn Sie einen benutzerdefinierten Domänennamen verwenden, erstellen Sie CNAME-Ressourceneintrags auf dem Internet-DNS-Server auf den Domänennamen Ihres Profils Traffic Manager.
4. Datenverkehr wird wieder an den Endpunkten weitergeleitet.

### <a name="to-delete-a-profile"></a>Zum Löschen eines Profils

1. Sicherstellen Sie, dass der Ressourceneintrag für den Internet-DNS-Server nicht mehr CNAME-Ressourceneintrags verwendet, der auf den Domänennamen Ihres Profils Traffic Manager zeigt.
2. Wählen Sie das Profil, das Sie deaktivieren möchten. Markieren Sie auf der Seite Traffic Manager das Profil durch Klicken auf die Spalte neben dem Profilnamen. Hinweis auf den Namen oder den Pfeil neben dem Namen wird die Einstellungsseite für das Profil geöffnet.
3. Klicken Sie nach Auswahl des Profils am unteren Rand der Seite **Löschen** .

## <a name="view-traffic-manager-profile-change-history"></a>Traffic Manager Ansicht Profil Änderungsprotokoll

Sie können den Änderungsverlauf für Ihr Profil Traffic Manager im klassischen Azure-Portal Management Services anzeigen.

### <a name="to-view-your-traffic-manager-change-history"></a>Traffic Manager Änderungsverlauf anzeigen

1. Klicken Sie im linken Bereich des klassischen Azure-Portal auf **Verwaltungsdienste**.
2. Klicken Sie auf der Seite Management Services auf **Protokolle**.
3. Auf der Seite Protokolle können Sie filtern, um das Änderungsprotokoll für Ihr Profil Traffic Manager anzuzeigen. Klicken Sie nachdem Sie die Filteroptionen auswählen auf das Häkchen, um die Ergebnisse anzuzeigen.

   - Um die Änderung für alle Profile anzuzeigen, wählen Sie Ihr Abonnement und Zeitraum, und wählen Sie dann im Kontextmenü **Typ** **Traffic Manager** .
   - Profilname filtern, geben Sie den Namen des Profils im Feld **Dienstname** oder aus dem Kontextmenü auswählen.
   - Zum Anzeigen der Details für jede einzelne Änderung wählen Sie die Zeile mit der gewünschten Änderung und dann auf **Details** am unteren Rand der Seite. Im Fenster **Vorgangsdetails** sehen Sie die XML-Darstellung des API-Objekts, die erstellt oder als Teil des Vorgangs aktualisiert wurde.

## <a name="next-steps"></a>Nächste Schritte

- [Einen Endpunkt hinzufügen](traffic-manager-endpoints.md)
- [Konfigurieren Sie Failover routing](traffic-manager-configure-failover-routing-method.md)
- [Konfigurieren von Round-Robin Verteilungsmethode](traffic-manager-configure-round-robin-routing-method.md)
- [Routingmethode Leistung konfigurieren](traffic-manager-configure-performance-routing-method.md)
- [Zeigen Sie eine Internetdomäne Unternehmen Domänennamen Traffic Manager](traffic-manager-point-internet-domain.md)
- [Problembehandlung bei Traffic Manager verminderten Zustand](traffic-manager-troubleshooting-degraded.md)