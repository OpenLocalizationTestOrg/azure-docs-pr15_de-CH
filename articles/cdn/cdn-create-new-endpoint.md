<properties
     pageTitle="Mit Azure CDN | Microsoft Azure"
     description="In diesem Thema wird das Content Delivery Network (CDN) für Azure aktivieren. Das Lernprogramm führt durch die Erstellung eines neuen CDN-Profil und Endpunkt."
     services="cdn"
     documentationCenter=""
     authors="camsoper"
     manager="erikre"
     editor=""/>
<tags
     ms.service="cdn"
     ms.workload="media"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.date="07/28/2016" 
     ms.author="casoper"/>

# <a name="using-azure-cdn"></a>Mithilfe von Azure CDN  

Dieses Thema führt durch Azure CDN durch Erstellen einer neuen CDN-Profil und Endpunkt aktivieren.

>[AZURE.IMPORTANT] Einführung in EUR, sowie eine Liste der Features, Funktionsweise finden Sie unter [CDN Overview](./cdn-overview.md).

## <a name="create-a-new-cdn-profile"></a>Erstellen eines neuen Profils CDN

Eine CDN-Profil besteht aus CDN-Endpunkte.  Jedes Profil enthält mindestens eine CDN-Endpunkte.  Sie können mehrere Profile CDN Endpunkte Internet-Domäne, Anwendung oder anderen Kriterien zu verwenden.

> [AZURE.NOTE] Standardmäßig ist ein einzelnes Azure-Abonnement auf acht CDN-Profile. Jedes Profil CDN ist zehn CDN-Endpunkte auf.
>
> CDN Preise auf der Profilebene CDN angewendet. Möchten Sie eine Mischung Tarifen Azure CDN verwenden, benötigen Sie mehrere CDN-Profile.

[AZURE.INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Erstellen Sie eine neue CDN

**Neue CDN erstellen**

1. Navigieren Sie in [Azure-Portal](https://portal.azure.com)zu Ihrem Profil CDN.  Sie können es dem Dashboard im vorherigen Schritt angeheftet haben.  Wenn Sie nicht Sie es finden auf **Durchsuchen**und **CDN-Profile**und auf im Profil den Endpunkt hinzugefügt werden soll.

    CDN Profil Blade wird angezeigt.

    ![CDN-Profil][cdn-profile-settings]

2. Klicken Sie auf **Endpunkt hinzufügen** .

    ![Endpunkt-Schaltfläche hinzufügen][cdn-new-endpoint-button]

    Das Blade **einen Endpunkt hinzufügen** wird angezeigt.

    ![Endpunkt Blade hinzufügen][cdn-add-endpoint]

3. Geben Sie einen **Namen** für diesen Endpunkt CDN.  Dieser Name wird verwendet, um die zwischengespeicherten Ressourcen in der Domäne zugreifen `<endpointname>.azureedge.net`.

4. Wählen Sie in der Dropdownliste **Herkunftstyp** der Ursprung.  Wählen Sie **Speicher** für Azure Storage-Konto, **Clouddienst** Azure Cloud Service, **Web App** Azure Web App oder **benutzerdefinierten Ursprung** für anderen öffentlich zugängliche Internet Server Ursprungs (in Azure oder gehostet).

    ![Herkunftstyp CDN](./media/cdn-create-new-endpoint/cdn-origin-type.png)
        
5. Wählen Sie in der Dropdownliste **Ursprung Hostname** oder geben Sie die Ursprungsdomäne.  Die Dropdownliste Listet alle verfügbaren Herkunft des Typs in Schritt 4 angegebenen.  Bei Auswahl von *benutzerdefinierten Ursprung* als Ihr **Ursprung eingeben**Geben Sie im Bereich benutzerdefinierte Ursprungs.

6. Geben Sie den Pfad im Feld **Pfad Ursprung** Ressourcen zwischenspeichern möchten oder nichts zu Cache alle Ressourcen in der Domäne, die Sie in Schritt 5 angegeben.

7. Geben Sie im **Hostheader Ursprung**Hostheader CDN, die mit jeder Anforderung gesendet werden soll, oder übernehmen Sie die Standardeinstellung.

    > [AZURE.WARNING] Einigen Herkunft Azure-Speicher und Web Apps muss des Host-Headers mit der Domäne des Ursprungs. Sofern Sie einen Ursprung, der von der Domäne einen Hostheader erforderlich sind, lassen Sie den Standardwert.

8. Geben Sie für **Protokoll** und **Ursprungs-Port**die Protokolle und Ports verwendet, um Zugriff auf Ihre Ressourcen am Ursprung.  Mindestens ein Protokoll (HTTP oder HTTPS) muss ausgewählt werden.
    
    > [AZURE.NOTE] **Ursprungs-Port** wirkt sich nur auf port wie der Endpunkt verwendet zum Abrufen von Informationen vom Ursprung.  Der Endpunkt selbst verfügbar Endkunden auf dem standardmäßigen HTTP und HTTPS-Ports 80 und 443 unabhängig von den **Ursprungs-Port**nur.  
    >
    > Den vollständigen TCP-Portbereich Ursprung zulassen **Azure CDN von Akamai** Endpunkte nicht.  Eine Liste der Ursprungs-Ports, die nicht zulässig sind, finden Sie unter [Azure CDN von Akamai zulässige Ursprung Ports](https://msdn.microsoft.com/library/mt757337.aspx).  
    >
    > Zugriff auf CDN hat mit HTTPS Inhalt die folgenden Nebenbedingungen:
    > 
    > - Verwenden Sie das SSL-Zertifikat von CDN. Zertifikate von Drittanbietern werden nicht unterstützt.
    > - Verwenden Sie Domäne CDN bereitgestellt (`<endpointname>.azureedge.net`), HTTPS-Inhalte zugreifen. HTTPS-Unterstützung ist nicht für benutzerdefinierte Domänennamen (CNAMEs), da das CDN benutzerdefinierte Zertifikate zu diesem Zeitpunkt nicht unterstützt.

9. Klicken Sie auf **Hinzufügen** , um den neuen Endpunkt zu erstellen.

10. Nach Erstellung der Endpunkt in einer Liste der Endpunkte des Profils angezeigt. Die Listenansicht zeigt die URL auf zwischengespeicherte Inhalte sowie der Ursprungsdomäne zugreifen.

    ![CDN-Endpunkt][cdn-endpoint-success]

    > [AZURE.IMPORTANT] Der Endpunkt sofort verfügbar, nicht dauert bei der Registrierung über das CDN verbreiten.  Verteilung wird für <b>Azure CDN von Akamai</b> Profile normalerweise innerhalb einer Minute abgeschlossen.  <b>Azure CDN von Verizon</b> Profilen Verteilung wird in der Regel innerhalb von 90 Minuten abgeschlossen jedoch in einigen Fällen kann länger dauern.
    >    
    > Benutzer, die den CDN-Domänennamen verwenden, bevor die Endpunktkonfiguration POPs übermittelt wurde erhalten HTTP 404-Antwortcodes.  Wenn mehreren Stunden seit erstellt den Endpunkt und Sie weiterhin 404-Antworten erhalten, finden Sie unter [Problembehandlung CDN-Endpunkte 404 Status zurückgegeben](cdn-troubleshoot-endpoint.md).


##<a name="see-also"></a>Siehe auch
- [Zwischenspeichern Anfragen Abfragezeichenfolgen-Verhalten steuern](cdn-query-string.md)
- [Zum Zuordnen von CDN Inhalt zu einer benutzerdefinierten Domäne](cdn-map-content-to-custom-domain.md)
- [Vorab laden Sie auf einen Endpunkt Azure CDN](cdn-preload-endpoint.md)
- [Einen Azure CDN-Endpunkt löschen](cdn-purge-endpoint.md)
- [Problembehandlung bei 404 Status zurückgeben CDN-Endpunkte](cdn-troubleshoot-endpoint.md)

[cdn-profile-settings]: ./media/cdn-create-new-endpoint/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-new-endpoint/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-new-endpoint/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-new-endpoint/cdn-endpoint-success.png
