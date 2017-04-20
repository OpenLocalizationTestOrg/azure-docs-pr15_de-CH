<properties
    pageTitle="Zugriff auf Ihre Inhalte Azure CDN nach Land | Microsoft Azure"
    description="Informationen Sie zum Zugriff auf Azure CDN Content mit Geo-Filterung."
    services="cdn"
    documentationCenter=""
    authors="camsoper, rli"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="casoper"/>

#<a name="restrict-access-to-your-content-by-country---verizon"></a>Beschränken des Zugriffs auf den Inhalt nach Land - Verizon

> [AZURE.SELECTOR]
- [Verizon](cdn-restrict-access-by-country.md)
- [Akamai-Standard](cdn-restrict-access-by-country-akamai.md)

##<a name="overview"></a>Übersicht

Wenn ein Benutzer den Inhalt standardmäßig, serviert Inhalt unabhängig davon, wo der Benutzer diese Anforderung aus. In einigen Fällen möchten Sie Zugriff auf Ihre Inhalte nach Land. Dieses Thema erläutert das **Geo -** Filterung verwenden, um den Dienst zu konfigurieren, Sperren von Land.

> [AZURE.IMPORTANT] Verizon und Akamai Produkte bieten die gleiche Funktionalität Geo-Filter, aber die Benutzeroberfläche unterscheidet. Dieses Dokument beschreibt die Schnittstelle für **Azure CDN Standard von Verizon** und **Azure CDN von Verizon**. Geo-Filtern mit **Azure CDN Standard von Akamai**finden Sie unter [Zugriff auf den Inhalt nach Land - Akamai beschränken](cdn-restrict-access-by-country-akamai.md).

Informationen zu Faktoren, die bewirken, dass diese Art der Einschränkung finden Sie im Abschnitt [Hinweise](cdn-restrict-access-by-country.md#considerations) am Ende des Themas.  

>[AZURE.NOTE] Nach die Konfiguration eingerichtet ist, gilt es für alle **Azure CDN von Verizon** Endpunkte in diesem Profil Azure CDN.

![Land filtern](./media/cdn-filtering/cdn-country-filtering.png)

##<a name="step-1-define-the-directory-path"></a>Schritt 1: Definieren Sie den Verzeichnispfad

Wenn Land Filter konfigurieren, müssen Sie den relativen Pfad zum Speicherort angeben, Benutzer gewährt oder verweigert. Anwenden können Land Filterung nach allen Dateien mit "/" oder ausgewählte Ordner von Verzeichnispfade angeben.

Pfad Verzeichnisfilter Beispiel:

    /                                 
    /Photos/
    /Photos/Strasbourg

##<a name="step-2-define-the-action-block-or-allow"></a>Schritt 2: Definieren die Aktion: blockieren oder zulassen

**Block:** Benutzer aus den angegebenen Ländern werden Zugriff auf Ressourcen von diesem Pfad rekursiv angefordert verweigert. Wenn keine anderen Land Filteroptionen für diesen Standort konfiguriert wurden, werden dann alle Benutzer zugreifen.

**Ermöglichen:** Nur Benutzer aus den angegebenen Ländern können Zugriff auf Ressourcen von diesem Pfad rekursiv angefordert.

##<a name="step-3-define-the-countries"></a>Schritt 3: Definieren der Länder

Wählen Sie die Länder zu blockieren oder zulassen für den Pfad. Weitere Informationen finden Sie unter [Azure CDN von Verizon Ländercodes](https://msdn.microsoft.com/library/mt761717.aspx).

Beispielsweise wird die Regel für die Blockierung /Photos/Straßburg einschließlich filtern:

    http://<endpoint>.azureedge.net/Photos/Strasbourg/1000.jpg
    http://<endpoint>.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg


##<a name="country-codes"></a>Ländercodes

**Geo -** Filterung verwendet Ländercodes Länder definieren, von denen eine Anforderung gewährt oder blockiert ein sicheres Verzeichnis. Die Ländercodes finden Sie in [Azure CDN von Verizon Ländercodes](https://msdn.microsoft.com/library/mt761717.aspx). Wenn Sie "EU" (Europa) oder "AP" (Asien), eine Teilmenge des IP-Adressen, die aus einem Land stammen, Regionen blockiert oder zugelassen werden.


##<a id="considerations"></a>Hinweise

- Bis zu 90 Minuten ändern dauern, Ihr Land Filterkonfiguration zu.
- Dieses Feature unterstützt keine Platzhalterzeichen (z. B. "*").
- Das Land Filtern den relativen Pfad zugeordnet werden rekursiv auf diesen Pfad angewendet wird.
- Nur eine Regel kann auf dem gleichen relativen Pfad angewendet werden (die Land filtern, die auf dem gleichen relativen Pfad kann nicht erstellt werden. Jedoch möglicherweise ein Ordner Land filtern. Dies ist aufgrund der rekursive Land filtern. Unterordner eines bereits konfigurierten Ordners kann also einen anderes Land Filter zugewiesen werden.
