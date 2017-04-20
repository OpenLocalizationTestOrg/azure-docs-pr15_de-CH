<properties
    pageTitle="Vorab laden auf einen Endpunkt Azure CDN | Microsoft Azure"
    description="Erfahren Sie, wie zwischengespeicherte Inhalte auf einen CDN-Endpunkt vorab geladen."
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

# <a name="pre-load-assets-on-an-azure-cdn-endpoint"></a>Vorab laden Sie auf einen Endpunkt Azure CDN

[AZURE.INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Standardmäßig werden Anlagen zuerst zwischengespeichert, wie angefordert. Dies bedeutet, dass die erste Anforderung aus jeder Region kann länger dauern, da die Edge-Server nicht der Inhalt zwischengespeichert und muss Anfrage auf den ursprünglichen Server weiterleiten. Vorab laden von Inhalten vermeidet diese ersten Treffer Wartezeit.

Neben einer besseren Customer Experience Reduzierung laden die zwischengespeicherten Elemente auch auf dem Ursprungsserver der Netzwerklast.

> [AZURE.NOTE] Elemente laden ist nützlich für große Ereignisse oder Inhalte gleichzeitig für eine große Anzahl von Benutzern eine neue Film-Version oder ein Update verfügbar ist.

Dieses Lernprogramm führt Sie durch die zwischengespeicherte Inhalte auf alle Azure CDN Randknoten vorab laden.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

1. Suchen Sie in [Azure-Portal](https://portal.azure.com)CDN Profil mit dem Endpunkt zu laden.  Profil-Blatt wird geöffnet.

2. Klicken Sie auf den Endpunkt in der Liste.  Endpunkt-Blatt wird geöffnet.

3. CDN-Endpunkt Blatt klicken Sie auf laden.

    ![CDN-Endpunkt blade](./media/cdn-preload-endpoint/cdn-endpoint-blade.png)

    Load-Blatt wird geöffnet.

    ![CDN laden blade](./media/cdn-preload-endpoint/cdn-load-blade.png)

4. Geben Sie den vollständigen Pfad der Ressource geladen werden soll (z.B. `/pictures/kitten.png`) im Textfeld **Pfad** .

    > [AZURE.TIP] Weitere **Pfad** Textfelder erscheint nach der Eingabe von Text, um eine Liste mit mehreren Anlagen erstellen können.  Durch Klicken auf die Schaltfläche mit den Auslassungszeichen (...) können Sie Elemente aus der Liste löschen.
    >
    > Pfade muss eine relative URL, die den folgenden [regulären Ausdruck](https://msdn.microsoft.com/library/az24scfc.aspx)entspricht: `^(?:\/[a-zA-Z0-9-_.\u0020]+)+$`.  Jede Anlage muss seinen eigenen Weg.  Es gibt keine Platzhalter-Funktionalität für Anlagen vorab laden.

    ![Schaltfläche Laden](./media/cdn-preload-endpoint/cdn-load-paths.png)

5. Klicken Sie auf die Schaltfläche **Laden** .

    ![Schaltfläche Laden](./media/cdn-preload-endpoint/cdn-load-button.png)

> [AZURE.NOTE] Es ist eine Einschränkung des 10 laden Anfragen pro Minute pro CDN-Profil.

## <a name="see-also"></a>Siehe auch
- [Einen Azure CDN-Endpunkt löschen](cdn-purge-endpoint.md)
- [Azure CDN REST-API-Referenz - löschen oder einen Endpunkt laden](https://msdn.microsoft.com/library/mt634451.aspx)
