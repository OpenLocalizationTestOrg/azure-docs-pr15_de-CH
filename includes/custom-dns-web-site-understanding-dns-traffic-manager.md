Domain Name System (DNS) dient der Dinge im Internet zu suchen. Z. B. Wenn Sie eine in Ihrem Browser Adresse oder auf einer Webseite klicken, wird DNS Domäne in eine IP-Adresse übersetzt. Ist die IP-Adresse wie eine Adresse, aber ist nicht sehr menschenfreundlich. Beispielsweise ist es einfacher an einen DNS-Namen wie **"contoso.com"** als eine IP-Adresse wie 192.168.1.88 oder 2001:0:4137:1f67:24a2:3888:9cce:fea3 ist.

Die DNS- *Datensätze*basiert. Datensätze ordnen einen bestimmten *Namen*, z. B. **contoso.com**eine IP-Adresse oder einen anderen DNS-Namen. Wenn eine Anwendung z. B. ein Webbrowser einen Namen im DNS sucht, findet den Datensatz und verwendet was es an die Adresse. Der Wert weist auf eine IP-Adresse ist, wird der Browser Wert verwenden. Weist einen anderen DNS-Namen, muss die Anwendung die Auflösung wiederholen. Schließlich werden alle Auflösung eine IP-Adresse enden.

Beim Erstellen einer Azure-Website ist ein DNS-Name der Site automatisch zugewiesen. Dieser Name hat die Form ** &lt;Yoursitename&gt;. *.azurewebsites.NET**. Wenn Sie Ihre Website als Endpunkt Azure Traffic Manager hinzufügen, ist Ihre Website über die ** &lt;Yourtrafficmanagerprofile&gt;. trafficmanager.net** Domäne.

> [AZURE.NOTE] Wenn Ihre Website als Endpunkt Traffic Manager konfiguriert ist, verwenden Sie die **. trafficmanager.net** Adresse beim DNS-Datensätze erstellen.

> CNAME-Einträge können mit Traffic Manager Sie nur

Stehen auch mehrere Datensätze mit Funktionen und Grenzen, aber für Websites als Traffic Manager Endpunkte konfiguriert, wir nur sorgen über. *CNAME* -Einträge.

###<a name="cname-or-alias-record"></a>CNAME oder Aliasdatensatz

Ein CNAME-Eintrag ordnet einen *bestimmten* DNS-Namen **www.contoso.com**oder **mail.contoso.com** eines anderen Domänennamens (kanonisch). Bei Azure Websites mit Traffic Manager, der kanonische Domänenname ist die ** &lt;Myapp >. trafficmanager.net** Domänennamen Ihres Profils Traffic Manager. Nach dem Erstellen der CNAME-Eintrag erstellt einen Alias für die ** &lt;Myapp >. trafficmanager.net** Domänennamen. CNAME-Eintrag wird die IP-Adresse auflösen der ** &lt;Myapp >. trafficmanager.net** Domänenname, so ändert die IP-Adresse der Website haben Sie keinen ergreifen.

Datenverkehr wird an Traffic Manager leitet sie den Datenverkehr in der Website mithilfe von Load balancing-Methode für die Konfiguration. Dies ist für Besucher Ihrer Website vollständig transparent. Sie sehen nur den benutzerdefinierten Domänennamen in ihrem Browser.

> [AZURE.NOTE] Domain-Registrierer können nur Unterdomänen zugeordnet, wenn ein CNAME-Eintrag wie **www.contoso.com**nicht Root Namen **contoso.com**verwenden. Weitere Informationen auf CNAME-Einträge finden Sie in der Dokumentation Ihrer Registrierungsstelle, <a href="http://en.wikipedia.org/wiki/CNAME_record">Wikipedia-Eintrag CNAME-Datensatz</a>oder dem Dokument <a href="http://tools.ietf.org/html/rfc1035">IETF Domain Names - Implementation and Specification</a> .
