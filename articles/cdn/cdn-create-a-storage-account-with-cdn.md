<properties
    pageTitle="Ein Speicherkonto CDN integrieren | Microsoft Azure"
    description="Informationen Sie zum Azure Content Delivery Network (CDN), breitbandige Inhalte durch Zwischenspeichern Blobs von Azure-Speicher verwenden."
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


# <a name="integrate-a-storage-account-with-cdn"></a>Ein Speicherkonto CDN integriert

CDN kann Cacheinhalt von Azure-Speicher aktiviert sein. Es bietet Entwicklern eine globale Lösung für Inhalte durch Zwischenspeichern Blobs und statische Inhalte Serverinstanzen auf physischen Knoten in den Vereinigten Staaten, Europa, Asien, Australien und Südamerika.


## <a name="step-1-create-a-storage-account"></a>Schritt 1: Erstellen Sie ein Speicherkonto

Gehen Sie folgendermaßen vor, ein neues Speicherkonto ein Azure-Abonnement erstellen. Ein Speicherkonto ermöglicht Zugriff auf Azure-Speicherdienste. Das Speicherkonto stellt die höchste Ebene des Namespace für den Zugriff auf alle Komponenten des Azure-Speicher: BLOB-Services, Warteschlangendienste und -Dienste. Weitere Informationen finden Sie unter [Einführung in Microsoft Azure Storage](../storage/storage-introduction.md).

Erstellen Sie ein Speicherkonto muss Dienstadministrator oder Co-Administrator für das Abonnement zugeordnet werden.

> [AZURE.NOTE] Es gibt verschiedene Methoden, ein Speicherkonto, einschließlich der Azure-Portal und Powershell erstellen.  Für dieses Lernprogramm verwenden wir Azure-Portal.  

**Ein Speicherkonto Azure-Abonnement erstellen**

1.  [Azure-Portal](https://portal.azure.com)anmelden.
2.  Wählen Sie in der oberen linken Ecke **neu**. Klicken Sie im Dialogfeld **neue** **Daten + Speicher**, klicken Sie auf **Konto**.

    Das Blade **Speicherkonto erstellen** wird angezeigt.

    ![Konto erstellen][create-new-storage-account]

4. Geben Sie im Feld **Name** einen Unterdomänennamen. Dieser Eintrag kann 3 24 Kleinbuchstaben und Zahlen enthalten.

    Dieser Wert wird der Hostname in den URI an Ressourcen-Blob, Warteschlange oder Tabelle für das Abonnement verwendet wird. Um eine Ressource Container im Blob-Dienst zu beheben, verwenden Sie einen URI im folgenden Format, * &lt;StorageAccountLabel&gt; * bezieht sich auf den eingegebenen **URL eingeben**:

    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;Mycontainer&gt; *

    **Wichtig:** Das URL-Label muss Subdomäne Speicherkonto URI und für alle gehosteten Dienste in Azure eindeutig.

    Dieser Wert dient auch als Namen für dieses Speicherkonto im Portal oder dieses Konto programmgesteuerten Zugriff auf.

5. Belassen Sie die Standardeinstellungen für **Bereitstellung**, **Konto Art**, **Leistung**und **Replikation**. 

6. Wählen Sie das Speicherkonto mit **Abonnement** .

7. Wählen Sie oder erstellen Sie eine **Ressourcengruppe**.  Weitere Informationen zu Ressourcengruppen finden Sie unter [Übersicht über Azure Ressource-Manager](azure-resource-manager/resource-group-overview.md#resource-groups).

8. Wählen Sie einen Speicherort für das Speicherkonto.

8. Klicken Sie auf **Erstellen**. Erstellen Sie das Speicherkonto möglicherweise mehrere Minuten.


## <a name="step-2-create-a-new-cdn-profile"></a>Schritt 2: Erstellen eines neuen Profils CDN

Eine CDN-Profil besteht aus CDN-Endpunkte.  Jedes Profil enthält mindestens eine CDN-Endpunkte.  Sie können mehrere Profile CDN Endpunkte Internet-Domäne, Anwendung oder anderen Kriterien zu verwenden.

> [AZURE.TIP] Haben Sie bereits ein CDN-Profil, das Sie für dieses Lernprogramm verwenden möchten, fahren Sie mit [Schritt 3](#step-3-create-a-new-cdn-endpoint)fort.

[AZURE.INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="step-3-create-a-new-cdn-endpoint"></a>Schritt 3: Erstellen einer neuen CDN

**Neue CDN für das Speicherkonto erstellen**

1. Navigieren Sie in [Azure-Verwaltungsportal](https://portal.azure.com)zu Ihrem Profil CDN.  Sie können es dem Dashboard im vorherigen Schritt angeheftet haben.  Wenn Sie nicht Sie es finden auf **Durchsuchen**und **CDN-Profile**und auf im Profil den Endpunkt hinzugefügt werden soll.

    CDN Profil Blade wird angezeigt.

    ![CDN-Profil][cdn-profile-settings]

2. Klicken Sie auf **Endpunkt hinzufügen** .

    ![Endpunkt-Schaltfläche hinzufügen][cdn-new-endpoint-button]

    Das Blade **einen Endpunkt hinzufügen** wird angezeigt.

    ![Endpunkt Blade hinzufügen][cdn-add-endpoint]

3. Geben Sie einen **Namen** für diesen Endpunkt CDN.  Dieser Name wird verwendet, um die zwischengespeicherten Ressourcen in der Domäne zugreifen `<endpointname>.azureedge.net`.

4. Wählen Sie in der Dropdownliste **Herkunftstyp** *Speicher*.  

5. Wählen Sie in der Dropdownliste **Ursprung Hostname** Ihres Speicherkontos.

6. Belassen Sie die Standardeinstellungen für **Ursprünglicher Pfad** **Hostheader Ursprung**und **Protokoll-Ursprungs-Port**.  Sie müssen mindestens ein Protokoll (HTTP oder HTTPS) angeben.

    > [AZURE.NOTE] Diese Konfiguration ermöglicht alle Ihre öffentlich sichtbare Container das Speicherkonto Zwischenspeichern im CDN.  Wenn Sie auf einen einzigen Container einschränken möchten, verwenden Sie **Ursprung Pfad**.  Hinweis der Container muss Visibility public festgelegt.

7. Klicken Sie auf **Hinzufügen** , um den neuen Endpunkt zu erstellen.

8. Nach Erstellung der Endpunkt in einer Liste der Endpunkte des Profils angezeigt. Die Listenansicht zeigt die URL auf zwischengespeicherte Inhalte sowie der Ursprungsdomäne zugreifen.

    ![CDN-Endpunkt][cdn-endpoint-success]

    > [AZURE.NOTE] Der Endpunkt wird sind nicht sofort verfügbar.  Es dauert bis zu 90 Minuten die Registrierung über das CDN-Netzwerk weitergegeben. Benutzer versuchen, die CDN sofort verwenden erhalten Statuscode 404, bis der Inhalt über die CDN verfügbar ist.


## <a name="step-4-access-cdn-content"></a>Schritt 4: Access CDN content

Um zwischengespeicherte Inhalte auf das CDN zuzugreifen, verwenden Sie die CDN-URL im Portal. Die Adresse für zwischengespeicherte BLOBs werden ähnlich der folgenden:

http://<*endPointName angibt*\>.azureedge.net/ <*MyPublicContainer*\>/<*BlobName*\>

> [AZURE.NOTE] Ermöglichen den Zugriff auf ein Speicherkonto CDN oder gehosteten Dienst sind alle öffentlich verfügbaren Objekte CDN Rand Zwischenspeichern. Wenn Sie ein Objekt, die derzeit in der CDN zwischengespeichert ändern, wird der neue Inhalt nicht über die CDN bis CDN seinen Inhalt aktualisiert, wenn die zwischengespeicherten Inhalte TTL Ablauf.

## <a name="step-5-remove-content-from-the-cdn"></a>Schritt 5: Entfernen Sie Inhalt aus dem CDN

Wenn Sie kein Objekt in Azure Content Delivery Network (CDN) zwischenspeichern möchten, nehmen Sie die folgenden Schritte aus:

-   Container stellen Private statt öffentlichen. Weitere Informationen finden Sie unter [Verwalten anonymen Lesezugriff auf Container und Blobs](../storage/storage-manage-access-to-resources.md) .
-   Sie können mit dem Verwaltungsportal CDN-Endpunkt löschen oder deaktivieren.
-   Sie können Ihr gehosteten Dienst reagiert nicht mehr auf das Objekt.

Ein Objekt im CDN zwischengespeichert wird zwischengespeichert bleiben bis TTL für das Objekt Ablauf oder der Endpunkt gelöscht. Wenn der TTL Ablauf überprüfen CDN, ob CDN-Endpunkt gültig ist und das Objekt weiterhin anonym zugegriffen werden kann. Ist dies nicht, wird das Objekt nicht zwischengespeichert.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

-   [Zum Zuordnen von CDN Inhalt zu einer benutzerdefinierten Domäne](cdn-map-content-to-custom-domain.md)

[create-new-storage-account]: ./media/cdn-create-a-storage-account-with-cdn/CDN_CreateNewStorageAcct.png

[cdn-profile-settings]: ./media/cdn-create-a-storage-account-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-a-storage-account-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-a-storage-account-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-a-storage-account-with-cdn/cdn-endpoint-success.png
