<properties
    pageTitle="Status auf Azure Traffic Manager beschädigt Problembehandlung"
    description="Behebung von Traffic Manager Profile wird als fehlerhaft Status."
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

# <a name="troubleshooting-degraded-state-on-azure-traffic-manager"></a>Problembehandlung bei verminderten Zustand auf Azure Traffic Manager

Dieser Artikel beschreibt die Behandlung von Azure Traffic Manager-Profil, das nicht mehr redundanten Status angezeigt wird. In diesem Szenario sollten Sie Profil Traffic Manager auf einige cloudapp.net gehosteten Dienste konfiguriert. Überprüfen Sie den Zustand der Traffic Manager angezeigt, der Status heruntergestuft.

![defekt](./media/traffic-manager-troubleshooting-degraded/traffic-manager-degraded.png)

Wenn eingehen des Profils aus sehen Sie eine oder mehrere Endpunkte mit Offline-Status:

![Offline](./media/traffic-manager-troubleshooting-degraded/traffic-manager-offline.png)

## <a name="understanding-traffic-manager-probes"></a>Grundlegendes zu Traffic Manager Prüfpunkte

- Traffic Manager betrachtet Endpunkt ONLINE nur, wenn der Prüfpunkt aus den Prüfpunkt Pfad eine HTTP 200 Antwort empfängt. Andere nicht-200-Antwort ist.
- Eine Umleitung 30 X schlägt fehl, auch wenn die umgeleitete URL ein 200 zurückgibt.
- Für HTTPs-Prüfpunkte werden Zertifikatfehler ignoriert.
- Der tatsächliche Inhalt der Prüfpunkt Pfad spielt keine Rolle, wie 200 zurückgegeben wird. Suche URL zu statischem Inhalt wie "/ favicon.ico" ist eine gängige Technik. Dynamischer Inhalte, wie ASP-Seiten kann nicht immer 200 zurück, auch wenn die Anwendung fehlerfrei ist.
- Empfohlen wird den Prüfpunkt Pfad gesetzt, der genügend Logik bestimmt, dass die Seite nach oben oder unten. Im vorherigen Beispiel, indem Sie auf den Pfad "/ favicon.ico", werden nur Tests, w3wp.exe reagiert. Dieser Prüfpunkt kann nicht, dass die Webanwendung fehlerfrei ist. Eine bessere Option wäre einen Pfad zu einer etwas wie "/ Probe.aspx" Logik für die Integrität der Website hat. Sie könnten z. B. Leistungsindikatoren CPU-Auslastung verwenden oder messen die Anzahl der fehlgeschlagenen Anfragen. Oder Sie könnten versuchen, Zugriff auf Ressourcen oder Sitzungszustand sicherstellen, dass die Anwendung funktioniert.
- Wenn alle Endpunkte in einem Profil beschädigt sind, behandelt Traffic Manager alle fehlerfrei Endpunkte und Routen Datenverkehr auf alle Endpunkte. Dieses Verhalten wird sichergestellt, dass Probleme mit der Überprüfungsmechanismus keinen vollständigen Ausfall des Dienstes führen.

## <a name="troubleshooting"></a>Problembehandlung

Um einen Prüfpunkt Fehler zu beheben, benötigen Sie eine Tool, die den HTTP-Statuscode Prüfpunkt URL zurückgegeben wird. Es gibt viele Tools, die die unformatierte HTTP-Antwort angezeigt.

* [Fiddler](http://www.telerik.com/fiddler)
* [Aufrollen](https://curl.haxx.se/)
* [wget](http://gnuwin32.sourceforge.net/packages/wget.htm)

Auch können die Registerkarte Netzwerk F12 Debugging Tools in Internet Explorer Sie die HTTP-Antworten anzeigen.

Wir möchten die Antwort unserer Prüfpunkt URL beispielsweise: Http://watestsdp2008r2.cloudapp.net:80-Prüfpunkt. Das folgende PowerShell-Beispiel veranschaulicht das Problem.

```powershell
    Invoke-WebRequest 'http://watestsdp2008r2.cloudapp.net/Probe' -MaximumRedirection 0 -ErrorAction SilentlyContinue | Select-Object StatusCode,StatusDescription
```

Ausgabe:

```text
    StatusCode StatusDescription
    ---------- -----------------
            301 Moved Permanently
```

Beachten Sie, dass wir eine Umleitungsantwort empfangen. Wie zuvor alle StatusCode als ist 200 als Fehler betrachtet. Traffic Manager ändert den Endpunktstatus offline. Um das Problem zu beheben, überprüfen Sie die Website Konfiguration um sicherzustellen, dass den entsprechenden StatusCode aus dem Prüfpunkt zurückgegeben werden kann. Konfigurieren Sie Traffic Manager Prüfpunkt auf einen Pfad verweisen, der eine 200.

Wenn der Prüfpunkt das HTTPS-Protokoll verwenden, müssen Sie zur Vermeidung von SSL/TLS-Fehler während des Tests überprüfen von Zertifikaten deaktiviert. PowerShell Aussagen Validierung von Zertifikaten für die aktuelle Sitzung PowerShell zu deaktivieren:

```powershell
    add-type @"
    using System.Net;
    using System.Security.Cryptography.X509Certificates;
    public class TrustAllCertsPolicy : ICertificatePolicy {
        public bool CheckValidationResult(
        ServicePoint srvPoint, X509Certificate certificate,
        WebRequest request, int certificateProblem) {
        return true;
        }
    }
    "@
    [System.Net.ServicePointManager]::CertificatePolicy = New-Object TrustAllCertsPolicy
```

## <a name="next-steps"></a>Nächste Schritte

[Traffic Manager Datenverkehr routing-Methoden](traffic-manager-routing-methods.md)

[Was ist Traffic Manager](traffic-manager-overview.md)

[Cloud-Dienste](http://go.microsoft.com/fwlink/?LinkId=314074)

[Azure webapps](https://azure.microsoft.com/documentation/services/app-service/web/)

[Operationen auf Traffic Manager (REST-API-Referenz)](http://go.microsoft.com/fwlink/?LinkId=313584)

[Azure Traffic Manager-Cmdlets][1]

[1]: https://msdn.microsoft.com/library/mt125941(v=azure.200).aspx
