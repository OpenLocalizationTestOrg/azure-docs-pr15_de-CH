<properties
    pageTitle="Traffic Manager testen | Microsoft Azure"
    description="Dieser Artikel hilft Ihnen Traffic Manager-Konfiguration zu testen"
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

# <a name="test-your-traffic-manager-settings"></a>Testen Sie Ihre Traffic Manager settings

Traffic Manager Einstellungen zu testen, müssen Sie mehrere Clients an verschiedenen Standorten der Tests ausführen. Bringen Sie dann Endpunkte in Ihrem Profil Traffic Manager Sie nacheinander.

* Den DNS-TTL-Wert niedrig festgelegt damit schnell (z. B. 30 Sekunden) weiterleiten.
* Kennen Sie die IP-Adressen der Azure-Cloud-Dienste und Websites in das Profil, das Sie testen.
* Verwenden Sie Tools, mit denen Sie einen DNS-Namen in eine IP-Adresse aufgelöst und die Adresse anzeigen.

Sie überprüfen, dass die DNS-Namen in IP-Adressen der Endpunkte in Ihrem Profil beheben. Die Namen sollten im Einklang mit den Datenverkehr Routingmethode Traffic Manager-Profil definiert aufgelöst. Tools wie **Nslookup** oder **dig** können Sie DNS-Namen auflösen.

In den folgenden Beispielen können Sie Ihre Traffic Manager-Profil testen.

### <a name="check-traffic-manager-profile-using-nslookup-and-ipconfig-in-windows"></a>Traffic Manager Profil mit Nslookup Ipconfig in Windows

1. Öffnen Sie ein Befehl oder eine Meldung von Windows PowerShell als Administrator.
2. Typ `ipconfig /flushdns` den DNS-Auflösungscache leeren.
3. Typ `nslookup <your Traffic Manager domain name>`. Der folgende Befehl prüft beispielsweise der Domänenname mit dem Präfix *myapp.contoso*

        nslookup myapp.contoso.trafficmanager.net

    Normale Ergebnis zeigt die folgende Informationen:

    * DNS-Namen und IP-Adresse des DNS-Servers beheben diesen Domänennamen Traffic Manager zugegriffen wird.
    * Traffic Manager-Domänenname eingegebenen in der Befehlszeile "Nslookup" und der IP-Adresse, die Traffic Manager-Domäne aufgelöst wird. Die zweite IP-Adresse ist wichtig überprüfen. Es sollte eine öffentliche virtuelle IP-Adresse (VIP) Adresse für Cloud-Dienste oder Websites werden Traffic Manager-Profil übereinstimmen.

## <a name="how-to-test-the-failover-traffic-routing-method"></a>Testen der Failover Datenverkehr Verteilungsmethode

1. Lassen Sie alle Endpunkte auf.
2. Mithilfe eines einzelnen Clients, die DNS-Auflösung für Ihr Unternehmen Domänennamen mit Nslookup oder einem ähnlichen Dienstprogramm anfordern
3. Stellen Sie sicher, dass die aufgelöste IP-Adresse der primären entspricht.
4. Ausfall der primären Datei oder entfernen die Überwachung so, dass Traffic Manager geht davon aus, dass die Anwendung nach unten.
5. Der DNS-Time-to-Live (TTL) Traffic Manager-Profil sowie weitere zwei Minuten warten. Beispielsweise ist der DNS TTL 300 Sekunden (5 Minuten), müssen Sie sieben Minuten warten.
6. Löschen der DNS-Client-Cache und Anforderung DNS-Auflösung mit Nslookup. In Windows können Sie den DNS-Cache mit dem Befehl Ipconfig/flushdns leeren.
7. Sicherstellen Sie, dass die aufgelöste IP-Adresse den zweiten Endpunkt entspricht.
8. Wiederholen Sie den Vorgang jeden Endpunkt wiederum Deaktivierung. Überprüfen Sie, ob DNS die IP-Adresse des nächsten Endpunkt in der Liste zurückgibt. Wenn alle Endpunkte sind, erhalten Sie die IP-Adresse der primären erneut.

## <a name="how-to-test-the-weighted-traffic-routing-method"></a>Die Routingmethode gewichteten Datenverkehr testen

1. Lassen Sie alle Endpunkte auf.
2. Mithilfe eines einzelnen Clients, die DNS-Auflösung für Ihr Unternehmen Domänennamen mit Nslookup oder einem ähnlichen Dienstprogramm anfordern
3. Sicherstellen Sie, dass die aufgelöste IP-Adresse einer der Endpunkte entspricht.
4. Leeren Sie den DNS-Client-Cache zu, und wiederholen Sie die Schritte 2 und 3 für jeden Endpunkt. Verschiedene IP-Adressen aller Endpunkte zurückgegeben sollte angezeigt werden.

## <a name="how-to-test-the-performance-traffic-routing-method"></a>Testen der Leistung Datenverkehr Verteilungsmethode

Um eine Leistung Datenverkehr Routingmethode effektiv zu testen, müssen Sie Kunden in verschiedenen Teilen der Welt. Sie können Clients in Azure Regionen erstellen, die verwendet werden können, um Ihre Dienste zu testen. Haben Sie ein globales Netzwerk können Remote Clients in anderen Teilen der Welt anmelden und führen Sie die Tests aus.

Es gibt kostenlose Web-basierten DNS-Lookup und graben verfügbaren Dienste. Einige dieser Tools können Sie die DNS-Auflösung von Standorten auf der ganzen Welt überprüft. Führen Sie eine Suche nach "DNS-Lookup" Beispiele. Fremdanbieterdienste wie Gomez oder Keynote können verwendet werden, um sicherzustellen, dass Ihre Profile erwartungsgemäß Datenverkehr verteilen.

## <a name="next-steps"></a>Nächste Schritte

* [Traffic Manager Datenverkehr routing-Methoden](traffic-manager-routing-methods.md)
* [Traffic Manager Performance-Aspekte](traffic-manager-performance-considerations.md)
* [Problembehandlung bei Traffic Manager verminderten Zustand](traffic-manager-troubleshooting-degraded.md)




