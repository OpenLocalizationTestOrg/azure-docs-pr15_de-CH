<properties
    pageTitle="Verbessern der Leistung durch Komprimieren der Dateien in Azure CDN | Microsoft Azure"
    description="Informationen zu Datei Geschwindigkeit und seitenleistung durch Komprimieren der Dateien in Azure CDN."
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

# <a name="improve-performance-by-compressing-files"></a>Verbessern der Leistung durch Komprimieren der Dateien

Komprimierung ist eine einfache und effektive Methode zu Datei Übertragungsrate Leistungssteigerung Seite Laden von Dateigröße verringern, bevor sie vom Server gesendet werden. Es reduziert die Kosten für Bandbreite und bietet besser zu gestalten.

Es gibt zwei Methoden zur Aktivierung von Komprimierung:

- Sie können auf der Ursprungsserver Komprimierung in denen Fall CDN durchlaufen die komprimierten Dateien und komprimierte Dateien für Clients, die diese Anforderung übermitteln.
- In dem Fall CDN Komprimieren der Dateien und Benutzer dienen, selbst wenn sie von dem Ursprungsserver nicht komprimiert sind, können Sie Komprimierung auf CDN-Edgeserver.

> [AZURE.IMPORTANT] Neukonfiguration CDN Ruhe über das Netzwerk übertragen.  Übertragung abgeschlossen für <b>Azure CDN von Akamai</b> Profile normalerweise unter einer Minute.  <b>Azure CDN von Verizon</b> Profilen sehen Sie normalerweise Ihre Änderungen innerhalb von 90 Minuten.  Ist dies das erste Mal Sie Komprimierung für CDN-Endpunkt eingerichtet haben, sollten Sie warten 1-2 Stunden damit die Komprimierung Einstellungen POPs vor der Fehlerbehebung bei übertragen haben

## <a name="enabling-compression"></a>Komprimierung aktivieren

> [AZURE.NOTE] Standard- und Premium-CDN-Ebenen bieten dieselbe Komprimierungsfunktionalität, aber die Benutzeroberfläche unterscheidet.  Weitere Informationen über die Unterschiede zwischen Standard- und Premium-CDN Übersicht [Azure CDN](cdn-overview.md).

### <a name="standard-tier"></a>Standard-Stufe

> [AZURE.NOTE] Dieser Abschnitt gilt für **Azure CDN Standard von Verizon** und **Azure CDN Standard von Akamai** Profile.

1. Klicken Sie auf Profil CDN-Blade CDN-Endpunkt zu verwalten.

    ![CDN Profil Blade-Endpunkte](./media/cdn-file-compression/cdn-endpoints.png)

    Das CDN-Endpunkt-Blatt wird geöffnet.

2. Klicken Sie auf die Schaltfläche **Konfigurieren** .

    ![CDN Profil Blade Schaltfläche verwalten](./media/cdn-file-compression/cdn-config-btn.png)

    Das Blade CDN-Konfiguration wird geöffnet.

3. Aktivieren Sie **Komprimierung**.

    ![Komprimierungsoptionen CDN](./media/cdn-file-compression/cdn-compress-standard.png)

4. Verwenden Sie die Standardtypen oder ändern Sie die Liste entfernen oder Hinzufügen von Dateitypen.
    
    > [AZURE.TIP] Während möglich wird nicht empfohlen Komprimierung komprimierten Formaten wie ZIP, MP3, MP4, JPG usw. anwenden.
    
5. Klicken Sie nach der Änderung auf die Schaltfläche **Speichern** .

### <a name="premium-tier"></a>Premium-Ebene

> [AZURE.NOTE] Dieser Abschnitt gilt für **Azure CDN Premium von Verizon** Profilen.

1. Blatt Profil CDN klicken Sie auf **Verwalten** .

    ![CDN Profil Blade Schaltfläche verwalten](./media/cdn-file-compression/cdn-manage-btn.png)

    Das CDN-Verwaltungsportal wird geöffnet.

2. Hovern Sie über **HTTP große** Registerkarte und Maus über Flyout **Cache Settings** .  Klicken Sie auf **Komprimierung**.

    Dabei werden angezeigt.

    ![Komprimierung](./media/cdn-file-compression/cdn-compress-files.png)

3. Aktivieren Sie Komprimierung, indem Sie auf das Optionsfeld **Komprimierung aktiviert** .  Geben Sie die MIME-Typen als durch Trennzeichen getrennte Liste (ohne Leerzeichen) im Textfeld **Dateien** komprimieren möchten.
        
    > [AZURE.TIP] Während möglich wird nicht empfohlen Komprimierung komprimierten Formaten wie ZIP, MP3, MP4, JPG usw. anwenden. 

4. Klicken Sie nach der Änderung auf **Aktualisieren** .


## <a name="compression-rules"></a>Komprimierung Regeln

Diese Tabellen werden Azure CDN Komprimierung Verhalten für jedes Szenario beschrieben.

> [AZURE.IMPORTANT] **Azure CDN von Verizon** (Standard und Premium) werden geeignete Dateien komprimiert.  Komprimierung werden, müssen eine Datei:
>
> - Nicht größer als 128 Bytes.
> - Kleiner als 1 MB sein.
> 
> **Azure CDN von Akamai**sind alle Dateien für die Komprimierung.
>
> Für alle Azure CDN muss eine MIME-Typ, der [für die Komprimierung konfiguriert](#enabling-compression)wurde.
>
> **Gzip**, **Verkleinern**oder **bzip2** Codierung unterstützen **Azure CDN von Verizon** Profile (Standard und Premium).  **Azure CDN von Akamai** Profile unterstützen nur die **Gzip** -Codierung.
>
> **Azure CDN von Akamai** Endpunkte anfordern **Gzip** codierte Dateien immer vom Ursprung, unabhängig von der Clientanforderung.

### <a name="compression-disabled-or-file-is-ineligible-for-compression"></a>Komprimierung deaktiviert oder Datei ist nicht für Komprimierung

|Client angeforderten Format (Accept-Encoding-Header)|Zwischengespeicherte Dateiformat|CDN-Antwort an den client|Notizen|
|----------------|-----------|------------|-----|
|Komprimiert|Komprimiert|Komprimiert|   |
|Komprimiert|Nicht komprimiert|Nicht komprimiert|    | 
|Komprimiert|Nicht zwischengespeichert|Komprimiert oder nicht komprimiert|Ursprung Antwort hängt|
|Nicht komprimiert|Komprimiert|Nicht komprimiert|    |
|Nicht komprimiert|Nicht komprimiert|Nicht komprimiert|    |   
|Nicht komprimiert|Nicht zwischengespeichert|Nicht komprimiert|     |

### <a name="compression-enabled-and-file-is-eligible-for-compression"></a>Aktivierter Komprimierung und Datei ist für die Komprimierung

|Client angeforderten Format (Accept-Encoding-Header)|Zwischengespeicherte Dateiformat|CDN-Antwort an den client|Notizen|
|----------------|-----------|------------|-----|
|Komprimiert|Komprimiert|Komprimiert|CDN transcodiert zwischen Formate|
|Komprimiert|Nicht komprimiert|Komprimiert|CDN führt die Komprimierung|
|Komprimiert|Nicht zwischengespeichert|Komprimiert|CDN führt die Komprimierung kehrt Ursprung dekomprimiert.  **Azure CDN von Verizon** nicht komprimierte Datei bei der ersten Anforderung übergeben und dann und cache-Datei für nachfolgende Anforderungen.  Dateien mit `Cache-Control: no-cache` Header nicht komprimiert. 
|Nicht komprimiert|Komprimiert|Nicht komprimiert|CDN führt Dekomprimieren|
|Nicht komprimiert|Nicht komprimiert|Nicht komprimiert|     |  
|Nicht komprimiert|Nicht zwischengespeichert|Nicht komprimiert|     |

## <a name="media-services-cdn-compression"></a>Media Services CDN-Komprimierung

Für Media Services CDN streaming Endpunkte aktiviert, Komprimierung ist standardmäßig für die folgenden Inhaltstypen: Application/vnd.ms-voller Xml, application/dash+xml,application/vnd.apple.mpegurl, Application-f4m + Xml. Sie können keine Komprimierung für die genannten mit Azure-Portal aktivieren.  

## <a name="see-also"></a>Siehe auch
- [Problembehandlung bei CDN Komprimierung](cdn-troubleshoot-compression.md)    
