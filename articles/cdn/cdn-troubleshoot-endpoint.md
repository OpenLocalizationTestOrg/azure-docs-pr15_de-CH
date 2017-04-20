<properties
    pageTitle="Problembehandlung bei Azure CDN-Endpunkte wieder 404 Status | Microsoft Azure"
    description="Problembehandlung bei 404 Antwortcodes Azure CDN-Endpunkte."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>
    
# <a name="troubleshooting-cdn-endpoints-returning-404-statuses"></a>Problembehandlung bei 404 Status zurückgeben CDN-Endpunkte

In diesem Artikel können Sie die Problembehandlung mit [CDN-Endpunkte](cdn-create-new-endpoint.md) 404-Fehler zurückgegeben.

Benötigen Sie weitere Hilfe zu diesem Artikel erhalten Sie Azure-Experten auf [MSDN Azure und Foren Stapelüberlauf](https://azure.microsoft.com/support/forums/). Alternativ können Sie eine Azure Supportfall Datei. Der [Azure-Support-Website](https://azure.microsoft.com/support/options/) und klicken Sie auf **Support zu erhalten**.

## <a name="symptom"></a>Symptom

Erstellte Profil CDN und einen Endpunkt, aber Ihre Inhalte auf das CDN scheint.  Benutzern, die Zugriff auf Ihre Inhalte über die CDN-URL erhalten HTTP 404-Statuscodes. 

## <a name="cause"></a>Ursache

Es gibt mehrere mögliche Ursachen:

- Der Ursprung ist sichtbar CDN
- Der Endpunkt ist falsch verursacht CDN an der falschen Stelle gesucht,
- Der Host ist den Host-Header aus dem CDN ablehnen.
- Der Endpunkt hatte nicht Mal in CDN verbreiten

## <a name="troubleshooting-steps"></a>Schritte zur Fehlerbehebung

> [AZURE.IMPORTANT] Nach dem Erstellen eines CDN-Endpunkts, werden sofort zur Verfügung, wie es dauert die Registrierung über das CDN weitergegeben.  <b>Azure CDN von Akamai</b> Profile schließt Verteilung in der Regel innerhalb einer Minute.  <b>Azure CDN von Verizon</b> Profilen Verteilung wird in der Regel innerhalb von 90 Minuten abgeschlossen jedoch in einigen Fällen kann länger dauern.  Wenn die Schritte in diesem Dokument und Sie weiterhin 404-Antworten, sollten Sie ein paar Stunden erneut vor dem Öffnen einer Support-Ticket.

### <a name="check-the-origin-file"></a>Überprüfen Sie die Ursprungs-Datei

Wir sollten überprüfen Sie zunächst, wir zwischengespeicherte möchten, Datei zur Herkunft und öffentlich zugänglich.  Die schnellste Möglichkeit dazu ist zu öffnen in einem In privaten oder Incognito Sitzung direkt zur Datei.  Geben oder fügen Sie die URL in das Adressfeld und Siehe ergibt, die in der Datei, die Sie erwarten.  Für dieses Beispiel werde ich ein Azure Storage-Konto in habe eine Datei verwenden `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`.  Wie Sie sehen, wird den Test erfolgreich übergeben.

![Erfolg!](./media/cdn-troubleshoot-endpoint/cdn-origin-file.png)

> [AZURE.WARNING] Dies ist, zwar die schnellste und einfachste Möglichkeit zu überprüfen, ob die Datei öffentlich verfügbar ist könnte einige Netzwerkkonfigurationen in Ihrer Organisation die Illusion Ihnen, dass diese Datei öffentlich verfügbar ist, wird Benutzern des Netzwerks nur angezeigt (auch wenn es in Azure gehostet wird) tatsächlich.  Haben Sie einen externen Browser aus dem Sie, wie ein mobiles Gerät testen können, die nicht zum Netzwerk Ihrer Organisation oder einen virtuellen Computer in Azure am besten wäre.

### <a name="check-the-origin-settings"></a>Überprüfen Sie die Einstellungen Ursprung

Wir überprüft haben, dass die Datei öffentlich im Internet verfügbar ist, sollten wir unsere Ursprung Einstellungen überprüfen.  [Azure-Portal](https://portal.azure.com)wechseln Sie zu Ihrem CDN-Profil und klicken Sie auf den Endpunkt, den Sie beheben.  Klicken Sie in der resultierenden **Endpunkt** Blade-Ursprung.  

![Endpunkt Blade mit hervorgehoben](./media/cdn-troubleshoot-endpoint/cdn-endpoint.png)

**Ursprungs** -Blade wird angezeigt. 

![Ursprungs-blade](./media/cdn-troubleshoot-endpoint/cdn-origin-settings.png)

#### <a name="origin-type-and-hostname"></a>Herkunftstyp und hostname

Überprüfen Sie den **Ursprung Typ** und **Herkunft Hostname**überprüfen.  In meinem Beispiel `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`, der Hostnamenteil der URL ist `cdndocdemo.blob.core.windows.net`.  Wie in der Abbildung sehen können, ist dies korrekt.  Azure-Speicher, Web App und Cloud-Dienst Herkunft ist Feld **Ursprung Hostname** eine Dropdownliste müssen wir nicht korrekt Rechtschreibung sorgen.  Jedoch ist einen benutzerdefinierten Ursprung verwenden, *unabdingbar* , der Hostname korrekt geschrieben!

#### <a name="http-and-https-ports"></a>HTTP- und HTTPS-ports

Die anderen hier ist die **http-** und **HTTPS-Ports**.  In den meisten Fällen 80 und 443 korrekt sind, und Sie benötigen keine Änderung.  Wenn der Ursprungsserver auf einem anderen Port abhört, müssen, die hier dargestellt werden.  Wenn Sie nicht sicher sind, schauen Sie sich die URL für die Datei Ursprung.  HTTP- und HTTPS-Spezifikationen angeben Ports 80 und 443 als Standardprogramme. Meine URL `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`, ein Port nicht angegeben ist, wird der Standardwert 443 und Meine Einstellungen korrekt sind.  

Sagen die URL für die Ursprungs-Datei, die bereits überprüft ist `http://www.contoso.com:8080/file.txt`.  Hinweis Die `:8080` am Ende des Segments Hostname.  Das weist den Browser an Anschluss `8080` Verbindung mit dem Webserver unter `www.contoso.com`, so Sie im Feld **http-Port** 8080 eingeben müssen.  Es ist wichtig zu beachten, dass diese Einstellungen nur Anschluss am Endpunkt zum Abrufen von Informationen vom Ursprung verwendet.

> [AZURE.NOTE] Den vollständigen TCP-Portbereich Ursprung zulassen **Azure CDN von Akamai** Endpunkte nicht.  Eine Liste der Ursprungs-Ports, die nicht zulässig sind, finden Sie unter [Azure CDN von Akamai zulässige Ursprung Ports](https://msdn.microsoft.com/library/mt757337.aspx).  
  
### <a name="check-the-endpoint-settings"></a>Überprüfen Sie die Einstellung Endpunkt

Klicken Sie auf **Konfigurieren** , auf dem Blade **Endpunkt** .

![Endpunkt Blade mit hervorgehobenen konfigurieren](./media/cdn-troubleshoot-endpoint/cdn-endpoint-configure-button.png)

Der Endpunkt **Konfigurieren** Blade wird angezeigt.

![Blade konfigurieren](./media/cdn-troubleshoot-endpoint/cdn-configure.png)

#### <a name="protocols"></a>Protokolle

**Protokolle**stellen Sie sicher, dass das Protokoll von den Clients aktiviert ist.  Dasselbe Protokoll des Clients werden verwendet, um den Ursprung zugreifen muss Ursprung Ports im vorherigen Abschnitt ordnungsgemäß konfiguriert ist.  Der Endpunkt überwacht nur den Standard HTTP und HTTPS Ports 80 und 443 unabhängig von der Herkunft Ports.

Zurück zu unserem hypothetischen Beispiel `http://www.contoso.com:8080/file.txt`.  Wie Sie sich erinnern, Contoso angegeben `8080` als ihre HTTP-port, sondern auch nehmen sie angegebenen `44300` als HTTPS-Port.  Wenn sie einen Endpunkt Namens erstellt `contoso`, wäre die CDN-Endpunkt Hostnamen `contoso.azureedge.net`.  Eine Anforderung für `http://contoso.azureedge.net/file.txt` Endpunkt HTTP auf Port 8080 verwenden zum Abrufen aus der Ursprung eine HTTP-Anforderung ist.  Eine sichere Anforderung über HTTPS, `https://contoso.azureedge.net/file.txt`, würde den Endpunkt mit HTTPS an Port 44300 Wenn beim Abrufen der Datei vom Ursprung.

#### <a name="origin-host-header"></a>Hostheader Ursprung

**Hostheader Ursprung** ist der Hostheaderwert Ursprung mit jeder Anforderung gesendet.  In den meisten Fällen sollte dieser **Ursprung Hostname** identisch sein wir zuvor überprüft.  Ein falscher Wert in diesem Feld 404 Status nicht im Allgemeinen führen, aber kann andere Zustände 4xx je nach Ursprung erwartet.

#### <a name="origin-path"></a>Ursprünglicher Pfad

Schließlich sollten wir unsere **Ursprünglicher Pfad**überprüfen.  Standardmäßig ist dieser Bereich leer.  Sie sollten dieses Feld nur verwenden, möchten Sie die Ursprungs-gehosteten Ressourcen einschränken CDN verfügbar machen möchten.  

Beispielsweise wollte meine Endpunkt alle Ressourcen auf Mein Speicherkonto zu, sodass ich **Ursprünglicher Pfad** leer gelassen.  Dies bedeutet, dass eine Anforderung an `https://cdndocdemo.azureedge.net/publicblob/lorem.txt` eine Verbindung zu meinem Endpunkt führt `cdndocdemo.core.windows.net` fordert `/publicblob/lorem.txt`.  Ebenso eine Anforderung für `https://cdndocdemo.azureedge.net/donotcache/status.png` Endpunkt anfordern führt `/donotcache/status.png` vom Ursprung.

Aber was passiert, wenn ich nicht CDN für jeden Pfad auf Meine Herkunft verwenden möchten?  Angenommen, es verfügbar machen wollten die `publicblob` Pfad.  Wenn ich */publicblob* in meinem **Ursprung Pfad** eingeben, verursacht, die den Endpunkt */publicblob* vor jeder Anforderung an den Ursprung einfügen.  Dies bedeutet, dass die Anforderung für `https://cdndocdemo.azureedge.net/publicblob/lorem.txt` dauert nun tatsächlich Anforderung Teil der URL `/publicblob/lorem.txt`, und fügen `/publicblob` hinzu. Dies führt zu einer Anforderung für `/publicblob/publicblob/lorem.txt` vom Ursprung.  Wenn dieser Pfad auf eine tatsächliche Datei behoben wird, wird der Ursprung 404 Status zurück.  Die richtige URL abrufen lorem.txt in diesem Beispiel wäre `https://cdndocdemo.azureedge.net/lorem.txt`.  Beachten Sie, dass wir den Pfad */publicblob* , nicht der Anforderung Teil der URL ist `/lorem.txt` und der Endpunkt `/publicblob`, was `/publicblob/lorem.txt` wird die Anforderung an den Ursprung übergeben.
