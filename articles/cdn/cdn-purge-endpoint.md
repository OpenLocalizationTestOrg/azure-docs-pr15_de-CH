<properties
    pageTitle="Einen Azure CDN-Endpunkt löschen | Microsoft Azure"
    description="Informationen Sie zum Löschen alle zwischengespeicherten Inhalt von einem CDN-Endpunkt."
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

# <a name="purge-an-azure-cdn-endpoint"></a>Einen Azure CDN-Endpunkt löschen

## <a name="overview"></a>Übersicht

Azure CDN Randknoten werden Ressourcen zwischengespeichert, die Anlage Time to live (TTL) abgelaufen ist.  Nachdem des Vermögenswertes Gültigkeitsdauer abläuft, wenn eine Anlage aus der Kantenknoten Clientanforderungen Rand Knoten abrufen eine neue aktualisierte Kopie der Ressource für die Clientanforderung bedienen und Speicher den Cache aktualisieren.

Manchmal möchten Sie löschen zwischengespeicherten Inhalt bei Edge und sie alle neue aktualisierte Ressourcen abrufen.  Möglicherweise durch Updates der Webanwendung oder schnell Ressourcen aktualisieren, die falsche Informationen enthalten.

> [AZURE.TIP] Beachten Sie, dass Aufräumen nur zwischengespeicherte Inhalte auf CDN Edge-Server gelöscht.  Alle Caches nachgeschaltete Proxyserver und lokalen Browser-Cache können weiterhin eine zwischengespeicherte Kopie der Datei enthalten.  Es ist wichtig zu beachten, wenn Sie einer Datei TTL festgelegt.  Sie können erzwingen, dass nachgeschalteten Client anfordern, die neueste Version der Datei durch Zuweisen eines eindeutigen Namens jedes Mal aktualisieren oder [Abfrage Zeichenfolge Zwischenspeichern](cdn-query-string.md)nutzen.  

Dieses Lernprogramm führt Sie durch das Löschen von Anlagen bei Rand eines Endpunkts.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

1. Suchen Sie in [Azure-Portal](https://portal.azure.com)CDN Profil mit dem Endpunkt löschen möchten.

2. Klicken Sie auf bereinigen, Blatt Profil CDN.

    ![CDN Profil blade](./media/cdn-purge-endpoint/cdn-profile-blade.png)

    Blatt löschen wird geöffnet.

    ![CDN bereinigen blade](./media/cdn-purge-endpoint/cdn-purge-blade.png)

3. Wählen Sie auf dem Blatt löschen Serviceadresse aus URL entfernen möchten.

    ![Formular löschen](./media/cdn-purge-endpoint/cdn-purge-form.png)

    > [AZURE.NOTE] Sie erhalten bereinigen Blatt auch mit der Schaltfläche **Löschen** auf der CDN-Endpunkt.  In diesem Fall wird das Feld **URL** mit Serviceadresse dieses bestimmten Endpunkts.

4. Wählen Sie Ressourcen aus Randknoten entfernen möchten.  Wenn Sie alle Anlagen deaktivieren möchten, aktivieren Sie das Kontrollkästchen **Alle bereinigen** .  Andernfalls geben den vollständigen Pfad der Ressource löschen möchten (z.B. `/pictures/kitten.png`) im Textfeld **Pfad** .

    > [AZURE.TIP] Weitere **Pfad** Textfelder erscheint nach der Eingabe von Text, um eine Liste mit mehreren Anlagen erstellen können.  Durch Klicken auf die Schaltfläche mit den Auslassungszeichen (...) können Sie Elemente aus der Liste löschen.
    >
    > Pfade muss eine relative URL, die den folgenden [regulären Ausdruck](https://msdn.microsoft.com/library/az24scfc.aspx)entsprechen: `^\/(?:[a-zA-Z0-9-_.\u0020]+\/)*\*$";`.  **Azure CDN von Verizon** (Standard und Premium), Sternchen (\*) kann als Platzhalter verwendet werden (z.B. `/music/*`).  Platzhalter und **Löschen Sie alle** dürfen nicht mit **Azure CDN von Akamai**.
    
5. Klicken Sie auf die Schaltfläche **Löschen** .

    ![Schaltfläche Löschen](./media/cdn-purge-endpoint/cdn-purge-button.png)

> [AZURE.IMPORTANT] Löschen Sie Anfragen nehmen etwa 2-3 Minuten mit **Azure CDN von Verizon** (Standard und Premium) und ca. 7 Minuten mit **Azure CDN von Akamai**.  Azure CDN kann maximal 50 gleichzeitig Anfragen jederzeit löschen. 

## <a name="see-also"></a>Siehe auch
- [Vorab laden Sie auf einen Endpunkt Azure CDN](cdn-preload-endpoint.md)
- [Azure CDN REST-API-Referenz - löschen oder einen Endpunkt laden](https://msdn.microsoft.com/library/mt634451.aspx)
