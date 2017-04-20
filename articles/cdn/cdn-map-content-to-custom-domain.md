<properties
     pageTitle="Zum Zuordnen von Azure Content Delivery Network (CDN) Inhalte zu einer benutzerdefinierten Domäne | Microsoft Azure"
     description="In diesem Thema führen eine benutzerdefinierte Domäne einen CDN Inhalt zuordnen."
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
     ms.topic="article"
    ms.date="07/28/2016"
     ms.author="casoper"/>

# <a name="how-to-map-custom-domain-to-content-delivery-network-cdn-endpoint"></a>Wie benutzerdefinierte Domäne Content Delivery Network (CDN) Endpunkt zugeordnet
Sie können CDN-Endpunkt benutzerdefinierte Domäne zuordnen, um Ihren eigenen Domänennamen in URLs auf zwischengespeicherte Inhalt, anstatt eine Subdomäne von azureedge.net verwenden.

Es gibt zwei Methoden, um Ihre benutzerdefinierte Domain CDN-Endpunkt zugeordnet:

1. [Erstellen Sie einen CNAME-Datensatz mit Ihrer Registrierungsstelle und Ihre benutzerdefinierte Domäne und Unterdomäne CDN-Endpunkt zugeordnet](#register-a-custom-domain-for-an-azure-cdn-endpoint)

    Ein CNAME-Eintrag ist ein DNS-Feature, das eine Quelldomäne wie Karten `www.contosocdn.com` oder `cdn.contoso.com`, um eine Zieldomäne. In diesem Fall ist die Quelldomäne benutzerdefinierte Domäne und Unterdomäne (eine Subdomäne, wie **Www** oder **Cdn** immer erforderlich). Die Zieldomäne ist CDN-Endpunkt.  

    Der Prozess der CDN-Endpunkt der benutzerdefinierten Domäne zuordnen kann, führen in einem kurzen Ausfallzeit für die Domäne beim Registrieren der Domäne in der Azure-Portal.

2. [Hinzufügen einer temporären Registrierung mit **cdnverify**](#register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain)

    Wenn Ihre benutzerdefinierte Domain zurzeit eine Anwendung mit einer Vereinbarung zum Servicelevel (SLA), die erfordert, dass keine Ausfallzeiten werden unterstützt, dann können Azure **Cdnverify** Subdomäne Sie intermediate Registrierung bereitstellen, damit Benutzer Zugriff auf Ihre Domäne beim DNS Zuordnung erfolgt ist.  

Nach dem Registrieren der benutzerdefinierten Domäne mit einer der oben beschriebenen Verfahren sollten Sie überprüfen, [ob die benutzerdefinierte Domäne CDN-Endpunkt verweist](#verify-that-the-custom-subdomain-references-your-cdn-endpoint).

> [AZURE.NOTE] Erstellen Sie einen CNAME-Eintrag mit Ihrer Registrierungsstelle CDN-Endpunkt Ihrer Domäne zuzuordnen. CNAME-Einträge ordnen bestimmte Unterdomänen wie `www.contoso.com` oder `cdn.contoso.com`. Es ist nicht möglich, eine Stammdomäne wie ein CNAME-Eintrag zuordnen `contoso.com`.
>    
> Eine Domäne kann nur einen CDN-Endpunkt zugeordnet werden. CNAME-Eintrag, den Sie erstellen werden alle an der Domäne an den angegebenen Endpunkt adressiert Datenverkehr.  Angenommen, Sie verknüpfen `www.contoso.com` mit CDN-Endpunkt klicken Sie zuordnen können es andere Azure Endpunkte einen Endpunkt Storage-Konto oder ein Cloud-Endpunkt. Allerdings können Sie verschiedene Unterdomänen aus derselben Domäne für verschiedene Endpunkte. Verschiedene Unterdomänen ordnen Sie mit dem gleichen CDN-Endpunkt.
>
> Beachten Sie für Endpunkte **Azure CDN von Verizon** (Standard und Premium) dauert bis **90 Minuten** für benutzerdefinierte Domäne ändert CDN Randknoten verbreiten.

## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint"></a>Registrieren Sie eine benutzerdefinierte Domäne für einen Azure CDN-Endpunkt

1.  [Azure-Portal](https://portal.azure.com/)anmelden.
2.  Klicken Sie auf **Durchsuchen**, anschließend **CDN Profile**dann CDN-Profil mit dem Endpunkt einer benutzerdefinierten Domäne zuordnen möchten.  
3.  Klicken Sie auf die Sie zuordnen, die Subdomäne möchten CDN-Endpunkt Blatt **CDN-Profil** .
4.  Klicken Sie auf **Benutzerdefinierte Domäne hinzufügen** , oberen Endpunkt Blade.  Blatt **Hinzufügen einer benutzerdefinierte Domäne** sehen Sie den Endpunkt Hostnamen abgeleitet CDN-Endpunkt für einen neuen CNAME-Datensatz erstellen. Das Format der Host Name Adresse erscheint als ** &lt;endPointName angibt >. azureedge.net**.  Sie können diesen Hostnamen für den CNAME-Eintrag erstellen.  
5.  Navigieren Sie zur Website Ihrer Domainregistrierungsstelle, und suchen Sie den Abschnitt zum Erstellen von DNS-Datensätzen. Dieser Abschnitt wie **Domänennamen**, **DNS**oder **Name Server Management**möglicherweise.
6.  Suchen Sie den Abschnitt für die Verwaltung von CNAMEs. Sie müssen eine erweiterte Einstellungen und Suchen nach Wörtern CNAME-Alias oder Unterdomänen.
7.  Erstellen Sie einen neuen CNAME-Eintrag, der der ausgewählten Domäne (z. B. **Www** oder **Cdn**) zugeordnet ist, der Hostname Blatt **eine benutzerdefinierte Domäne hinzufügen** .
8.  Zum **Hinzufügen einer benutzerdefinierten Domäne** Blatt zurück, und geben Sie Ihre benutzerdefinierte Domäne, einschließlich der Subdomäne im Dialogfeld. Geben Sie z. B. den Domänennamen im Format `www.contoso.com` oder `cdn.contoso.com`.   

    Azure überprüft der CNAME-Eintrag für den Domänennamen vorhanden ist, die Sie eingegeben haben. Stimmt der CNAME-Eintrag, wird Ihre benutzerdefinierte Domain überprüft.  Für Endpunkte **Azure CDN von Verizon** (Standard und Premium) dauert es bis zu 90 Minuten für benutzerdefinierte Einstellungen jedoch allen CDN Rand Knoten übertragen.  

    Beachten Sie, dass in einigen Fällen es Zeit für den CNAME-Eintrag Namenserver im Internet verbreiten kann. Wenn Ihre Domäne nicht sofort überprüft und stimmt der CNAME-Eintrag Ihrer Meinung warten Sie einige Minuten und versuchen Sie es erneut.


## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain"></a>Registrieren Sie eine benutzerdefinierte Domäne für einen zwischengeschalteten Cdnverify Subdomäne mit Azure CDN-Endpunkt  

1. [Azure-Portal](https://portal.azure.com/)anmelden.
2. Klicken Sie auf **Durchsuchen**, anschließend **CDN Profile**dann CDN-Profil mit dem Endpunkt einer benutzerdefinierten Domäne zuordnen möchten.  
3. Klicken Sie auf die Sie zuordnen, die Subdomäne möchten CDN-Endpunkt Blatt **CDN-Profil** .
4. Klicken Sie auf **Benutzerdefinierte Domäne hinzufügen** , oberen Endpunkt Blade.  Blatt **Hinzufügen einer benutzerdefinierte Domäne** sehen Sie den Endpunkt Hostnamen abgeleitet CDN-Endpunkt für einen neuen CNAME-Datensatz erstellen. Das Format der Host Name Adresse erscheint als ** &lt;endPointName angibt >. azureedge.net**.  Sie können diesen Hostnamen für den CNAME-Eintrag erstellen.
5. Navigieren Sie zur Website Ihrer Domainregistrierungsstelle, und suchen Sie den Abschnitt zum Erstellen von DNS-Datensätzen. Dieser Abschnitt wie **Domänennamen**, **DNS**oder **Name Server Management**möglicherweise.
6. Suchen Sie den Abschnitt für die Verwaltung von CNAMEs. Möglicherweise zu einer Seite erweitert die Worte **CNAME** **Alias**oder **Unterdomänen**.
7. Erstellen Sie einen neuen CNAME-Eintrag und einen Subdomäne Alias, der Unterdomäne **Cdnverify** enthält. Die Domäne, die Sie angeben werden beispielsweise im Format **cdnverify.www** oder **cdnverify.cdn**. Geben Sie den Hostnamen CDN-Endpunkt im Format **Cdnverify.&lt; EndPointName angibt >. azureedge.net**.
8. Zum **Hinzufügen einer benutzerdefinierten Domäne** Blatt zurück, und geben Sie Ihre benutzerdefinierte Domäne, einschließlich der Subdomäne im Dialogfeld. Geben Sie z. B. den Domänennamen im Format `www.contoso.com` oder `cdn.contoso.com`. Beachten Sie, dass dabei nicht die Domäne mit **Cdnverify**Vorwort müssen.  

    Azure wird Überprüfen des Vorhandenseins der CNAME-Eintrag für den Domänennamen Cdnverify eingegebene.
9. An dieser Stelle Ihre benutzerdefinierte Domain wurde von Azure überprüft jedoch Datenverkehr Ihrer Domäne noch nicht an die CDN-Endpunkt verschoben. Lange genug warten auf die benutzerdefinierte Domäne Randknoten CDN (90 Minuten für **Azure CDN von Verizon**, 1-2 Minuten **Azure CDN von Akamai**) verbreiten können Ihrer DNS-Registrierungsstelle Website zurückzukehren und ein anderes CNAME-Eintrag erstellen, der Ihre Subdomain CDN-Endpunkt zugeordnet. Beispielsweise die Domäne als **Www** oder **Cdn**und Hostname ** &lt;endPointName angibt >. azureedge.net**. Mit diesem Schritt ist die Registrierung Ihrer benutzerdefinierten Domäne abgeschlossen.
10. Schließlich können Sie mit **Cdnverify**erstellten CNAME-Eintrag löschen, nur ein Zwischenschritt erforderlich war.  


## <a name="verify-that-the-custom-subdomain-references-your-cdn-endpoint"></a>Überprüfen Sie, ob die benutzerdefinierte Domäne CDN-Endpunkt verweist

- Nach Abschluss die Registrierung Ihrer benutzerdefinierten Domäne möglich zwischengespeicherten Inhalt auf die benutzerdefinierte Domäne CDN-Endpunkt.
Zunächst sicherstellen Sie, dass Sie öffentliche Inhalte, die am Endpunkt zwischengespeichert. Beispielsweise wenn CDN-Endpunkt ein Speicherkonto zugeordnet ist, speichert CDN Inhalte in öffentlichen Blob-Container. Zum Testen der benutzerdefinierten Domäne sicherstellen Sie, dass der Container zum Zulassen öffentlichen Zugriffs festgelegt ist und mindestens ein Blob enthält.
- Navigieren Sie in Ihrem Browser an der benutzerdefinierten Domäne Blob. Angenommen, Ihre benutzerdefinierte Domain `cdn.contoso.com`, URL, zwischengespeicherte BLOBs werden ähnlich dem folgenden URL: http://cdn.contoso.com/mypubliccontainer/acachedblob.jpg

## <a name="see-also"></a>Siehe auch

[Für Azure Content Delivery Network (CDN) aktivieren](./cdn-create-new-endpoint.md)  
