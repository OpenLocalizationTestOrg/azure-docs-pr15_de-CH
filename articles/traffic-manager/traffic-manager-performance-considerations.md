<properties
    pageTitle="Leistungsaspekte Azure Traffic Manager | Microsoft Azure"
    description="Leistung Traffic Manager und Leistung Ihrer Website testen Verwendung Traffic Manager"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="performance-considerations-for-traffic-manager"></a>Leistungsaspekte für Traffic Manager

Diese Seite erklärt Leistungsaspekte Traffic Manager verwenden. Szenario:

In WestUS und ostasiatische verfügen über Instanzen Ihrer Website. Fällt eine der Instanzen Health Check für Traffic Manager Prüfpunkt aus. Anwendungsdatenverkehr richtet fehlerfrei Region. Diese Failover erwartet kann Leistung jedoch ein Problem Wartezeit jetzt Datenverkehr auf einen entfernten Bereich.

## <a name="how-traffic-manager-works"></a>Funktionsweise von Traffic Manager

Nur Leistungseinbußen, die Traffic Manager auf Ihrer Website ist der erste DNS-Lookup. Eine DNS-Anforderung für den Namen Ihres Profils Traffic Manager erfolgt über den Microsoft DNS-Stammserver, der Host für die Zone trafficmanager.net. Traffic Manager füllt und aktualisiert regelmäßig die Microsoft DNS-Stammserver anhand der Traffic Manager-Richtlinie und die Testergebnisse. Keine DNS-Abfragen werden auch beim ersten DNS-Lookup Traffic Manager gesendet.

Traffic Manager besteht aus mehreren Komponenten: DNS-Server, API-Diensts, der Speicherschicht und ein Endpunkt Überwachungsdienst Namen. Schlägt eine Dienstkomponente Traffic Manager ist keine Auswirkung auf den DNS-Namen der Traffic Manager-Profil zugeordnet. Datensätze in Microsoft DNS Server bleiben unverändert. Allerdings passieren endpunktüberwachung und DNS-Aktualisierung nicht. Traffic Manager ist daher nicht aktualisieren DNS Failover-Standort auf, wenn der primäre Standort ausfällt.

DNS-Auflösung ist schnell und Ergebnisse werden zwischengespeichert. Die Geschwindigkeit der ursprünglichen DNS-Lookup hängt die DNS-Server, die der Client für die Auflösung verwendet. Normalerweise kann ein Client eine DNS-Suche in ca. 50 ms abgeschlossen. Die Ergebnisse der Suche werden für die Dauer der DNS-Time-to-live (TTL) zwischengespeichert. Die Standard-TTL für Traffic Manager beträgt 300 Sekunden.

Datenverkehr fließt nicht durch Traffic Manager. Sobald die DNS-Suche abgeschlossen ist, hat der Client eine IP-Adresse für eine Instanz der Website. Der Client verbindet direkt mit dieser Adresse und nicht durch Traffic Manager. Traffic Manager-Richtlinie wählen Sie hat keinen Einfluss auf die DNS-Leistung. Allerdings kann eine Leistung routing-Methode den Komfort beeinträchtigen. Beispielsweise wenn Ihre Richtlinie Datenverkehr aus einer Instanz gehostet in Asien umleitet, Netzwerklatenz für die Sitzung ein Leistungsproblem möglicherweise.

## <a name="measuring-traffic-manager-performance"></a>Traffic Manager Leistung

Es gibt mehrere Websites, mit der Leistung und das Verhalten eines Profils Traffic Manager zu verstehen. Viele dieser Seiten sind aber möglicherweise eingeschränkt. Einige Websites bieten erweiterte Überwachung und reporting für eine Gebühr.

Tools auf diesen Websites messen DNS-Wartezeiten und Anzeige der aufgelöst IP-Adressen für Clients Standorten weltweit. Die meisten dieser Tools Zwischenspeichern DNS-Ergebnisse nicht. Daher anzeigen Tools der vollständige DNS-Lookup jedes Mal ein Test ausgeführt wird. Beim Testen von Ihren eigenen Client werden nur vollständige DNS-Lookup Leistung Dauer TTL einmal auftreten.

## <a name="sample-tools-to-measure-dns-performance"></a>Beispieltools zur DNS-Leistung

- [SolveDNS](http://www.solvedns.com/dns-comparison/)

    SolveDNS bietet viele Leistungstools. DNS-Vergleichstool zeigen Ihnen, wie lange dauert die DNS-Namen auflösen und wie, DNS-Dienstanbieter vergleicht.

- [WebSitePulse](http://www.websitepulse.com/help/tools.php)

    Eines der einfachsten Tools ist WebSitePulse. Geben Sie die URL DNS-Auflösungszeit, erstes Byte, letzte Byte und anderen Performance-Statistiken. Sie können an drei verschiedenen Stellen. In diesem Beispiel sehen Sie, dass die erste Ausführung zeigt, dass DNS-Lookup 0.204 sec.

    ![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse.png)

    Da die Ergebnisse zwischengespeichert werden, dauert der zweite Test für den gleichen Traffic Manager-Endpunkt DNS-Lookup 0,002 Sekunden.

    ![pulse2](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse2.png)

- [CA App synthetische Monitor](https://asm.ca.com/en/checkit.php)

    Ehemals Watchmouse Kontrollkästchen Website Tool, diese Site anzeigen Sie DNS-Auflösung aus mehreren Regionen gleichzeitig. Geben Sie die URL DNS-Auflösungszeit, Zeit und Geschwindigkeit von mehreren Standorten aus. Verwenden Sie diesen Test an Standorten weltweit die gehosteter Dienst zurückgegeben wird.

    ![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-watchmouse.png)

- [Pingdom](http://tools.pingdom.com/)

    Dieses Tool bietet Performance-Statistiken für jedes Element einer Webseite. Die Registerkarte Seite Analyse zeigt den Prozentsatz der Zeit auf DNS-Lookup.

- [Was ist meine DNS?](http://www.whatsmydns.net/)

    Diese Site ist eine DNS-Suche von 20 Standorten und die Ergebnisse auf einer Karte angezeigt.

- [Graben Weboberfläche](http://www.digwebinterface.com)

    Diese Seite zeigt ausführliche Informationen einschließlich CNAMEs und A-Einträge in DNS. Stellen Sie sicher überprüfen "" Kolorieren "Ausgabe" und "Statistiken" unter Optionen, und wählen "Alle" unter Nameserver.

## <a name="next-steps"></a>Nächste Schritte

[Traffic Manager Datenverkehr routing-Methoden](traffic-manager-routing-methods.md)

[Testen Sie Ihre Traffic Manager settings](traffic-manager-testing-settings.md)

[Operationen auf Traffic Manager (REST-API-Referenz)](http://go.microsoft.com/fwlink/?LinkId=313584)

[Azure Traffic Manager-Cmdlets](http://go.microsoft.com/fwlink/p/?LinkId=400769)
