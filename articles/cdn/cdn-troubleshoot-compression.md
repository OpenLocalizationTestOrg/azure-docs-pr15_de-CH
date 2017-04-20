<properties
    pageTitle="Problembehandlung bei Komprimierung in Azure CDN | Microsoft Azure"
    description="Problembehandlung mit Komprimierung Azure CDN."
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
    ms.date="09/01/2016"
    ms.author="casoper"/>
    
# <a name="troubleshooting-cdn-file-compression"></a>Problembehandlung bei CDN Komprimierung

Dieser Artikel unterstützt Sie bei Problemen mit [Komprimierung CDN](cdn-improve-performance.md).

Benötigen Sie weitere Hilfe zu diesem Artikel erhalten Sie Azure-Experten auf [MSDN Azure und Foren Stapelüberlauf](https://azure.microsoft.com/support/forums/). Alternativ können Sie eine Azure Supportfall Datei. Der [Azure-Support-Website](https://azure.microsoft.com/support/options/) und klicken Sie auf **Support zu erhalten**.

## <a name="symptom"></a>Symptom

Komprimierung für den Endpunkt aktiviert, aber Dateien werden dekomprimiert zurückgegeben.

>[AZURE.TIP] Überprüfen Sie, ob die Dateien komprimiert zurückgegeben werden, müssen Sie ein Tool wie [Fiddler](http://www.telerik.com/fiddler) oder Browsers [Entwicklertools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).  Überprüfen der HTTP-Antwortheader zurück mit zwischengespeicherten CDN content  Wenn eine mit dem Namen Kopfzeile `Content-Encoding` mit dem Wert **Gzip**, **bzip2**oder **Verkleinern**die Inhalte komprimiert.
>
>![Content-Encoding-Headers](./media/cdn-troubleshoot-compression/cdn-content-header.png)

## <a name="cause"></a>Ursache

Es gibt mehrere mögliche Ursachen:

- Der angeforderte Inhalt ist nicht für die Komprimierung.
- Komprimierung ist für den angeforderten Typ nicht aktiviert.
- Die HTTP-Anforderung enthalten einen Header Anfordern einer gültigen Komprimierungstyp nicht.

## <a name="troubleshooting-steps"></a>Schritte zur Fehlerbehebung

> [AZURE.TIP] Wie bei der Bereitstellung von neuen Endpunkten Ruhe CDN-Konfiguration ändert sich über das Netzwerk übertragen.  In der Regel werden 90 Minuten angewendet.  Ist dies das erste Mal Sie Komprimierung für CDN-Endpunkt eingerichtet haben, sollten Sie warten 1-2 Stunden damit die Komprimierung Einstellungen POPs übertragen haben. 

### <a name="verify-the-request"></a>Überprüfen Sie die Anforderung

Zuerst tun wir schnellen Überprüfung auf die Anforderung.  Ihr Browser [Entwicklertools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) können Sie die Anträge anzeigen.

- Überprüfen Sie die Anforderung an den Endpunkt-URL gesendet wird `<endpointname>.azureedge.net`, und nicht der Ursprung.
- Überprüfen Sie die Anforderung enthält ein **Accept-Encoding** -Headers, der Wert für diesen Header **Gzip**, **Verkleinern**oder **bzip2**.

> [AZURE.NOTE] **Azure CDN von Akamai** Profile unterstützen nur die **Gzip** -Codierung.

![Anforderungsheader CDN](./media/cdn-troubleshoot-compression/cdn-request-headers.png)

### <a name="verify-compression-settings-standard-cdn-profile"></a>Überprüfen Sie die Einstellungen (Standard CDN Profil)

> [AZURE.NOTE] Dieser Schritt gilt nur, wenn Ihr Profil CDN **Azure CDN Standard von Verizon** oder **Azure CDN Standard von Akamai** -Profil ist. 

Navigieren Sie zu Ihrem Endpunkt in [Azure-Portal](https://portal.azure.com) , und klicken Sie auf die Schaltfläche **Konfigurieren** .

- Überprüfen Sie, ob die Komprimierung aktiviert ist.
- Überprüfen Sie, ob der MIME-Typ für den Inhalt komprimiert werden in der Liste der komprimierten Formate enthalten ist.

![Komprimierungsoption CDN](./media/cdn-troubleshoot-compression/cdn-compression-settings.png)

### <a name="verify-compression-settings-premium-cdn-profile"></a>Überprüfen Sie die Einstellungen (Premium CDN Profil)

> [AZURE.NOTE] Dieser Schritt gilt nur, wenn Ihr Profil CDN **Azure CDN Premium von Verizon** -Profil ist.

Navigieren Sie zu Ihrem Endpunkt in [Azure-Portal](https://portal.azure.com) , und klicken Sie auf **Verwalten** .  Zusätzliche Portal wird geöffnet.  Hovern Sie über **HTTP große** Registerkarte und Maus über Flyout **Cache Settings** .  Klicken Sie auf **Komprimierung**. 

- Überprüfen Sie, ob die Komprimierung aktiviert ist.
- Überprüfen Sie, ob die **Dateitypen** enthält einer durch Kommas getrennte Liste (ohne Leerzeichen) MIME-Typen.
- Überprüfen Sie, ob der MIME-Typ für den Inhalt komprimiert werden in der Liste der komprimierten Formate enthalten ist.

![Premium-Komprimierungsoption CDN](./media/cdn-troubleshoot-compression/cdn-compression-settings-premium.png)

### <a name="verify-the-content-is-cached"></a>Überprüfen Sie, ob der Inhalt zwischengespeichert

> [AZURE.NOTE] Dieser Schritt gilt nur, wenn Ihr Profil CDN ein **Azure CDN von Verizon** (Standard oder Premium) ist.

Überprüfen Sie mithilfe des Browsers Entwicklertools Antwortheader um sicherzustellen, dass die Datei im Bereich zwischengespeichert wird, wo es angefordert wird.

- Überprüfen des **Server** Antwortheaders.  Der Header müssen Format **Plattform (POP-Server-ID)**, wie im folgenden Beispiel.
- Überprüfen des Antwort-Headers **X-Cache** .  Der Header Lektüre **erreicht**.  

![Antwortheader CDN](./media/cdn-troubleshoot-compression/cdn-response-headers.png)

### <a name="verify-the-file-meets-the-size-requirements"></a>Stellen Sie sicher, dass die Datei die Größe erfüllt

> [AZURE.NOTE] Dieser Schritt gilt nur, wenn Ihr Profil CDN ein **Azure CDN von Verizon** (Standard oder Premium) ist.

Komprimierung werden, muss eine die folgenden Anforderungen erfüllen:

- Größer als 128 Bytes.
- Kleiner als 1 MB.

### <a name="check-the-request-at-the-origin-server-for-a-via-header"></a>Überprüfen Sie auf dem ursprünglichen Server **Via** -header

**Über** HTTP-Header zeigt der Webserver die Anforderung über einen Proxyserver übergeben wird.  Microsoft IIS-Webserver standardmäßig komprimieren nicht antworten, wenn die Anforderung **Via** -Header enthält.  Um dieses Verhalten zu überschreiben, führen Sie folgende Schritte aus:

- **IIS 6**: [HcNoCompressionForProxies festlegen = "FALSE" in der IIS-Metabasis-Eigenschaften](https://msdn.microsoft.com/library/ms525390.aspx)
- **IIS 7 und**: [ **noCompressionForHttp10** und **NoCompressionForProxies** in der Serverkonfiguration auf False festlegen](http://www.iis.net/configreference/system.webserver/httpcompression)

