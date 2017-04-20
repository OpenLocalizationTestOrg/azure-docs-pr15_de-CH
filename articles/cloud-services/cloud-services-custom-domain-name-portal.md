<properties
    pageTitle="Einen benutzerdefinierten Domänennamen in Cloud Services konfigurieren | Microsoft Azure"
    description="Erfahren Sie, wie Ihre Azure-Anwendung oder Daten mit dem Internet in einer benutzerdefinierten Domäne verfügbar machen, indem Sie DNS konfigurieren.  Diese Beispiele verwenden Azure-Portal."
    services="cloud-services"
    documentationCenter=".net"
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="adegeo"/>

# <a name="configuring-a-custom-domain-name-for-an-azure-cloud-service"></a>Konfigurieren einen benutzerdefinierten Domänennamen für einen Azure-Cloud-Dienst

> [AZURE.SELECTOR]
- [Azure-portal](cloud-services-custom-domain-name-portal.md)
- [Azure-Verwaltungsportal](cloud-services-custom-domain-name.md)

Beim Erstellen einer Cloud-Dienst zugewiesen Azure eine Subdomäne von **cloudapp.net**. Beispielsweise wenn Cloud-Dienst "Contoso" lautet, werden die Benutzer auf die Anwendung über eine URL wie http://contoso.cloudapp.net. Azure weist auch eine virtuelle IP-Adresse.

Allerdings können Sie Ihre Anwendung auf Ihren eigenen Domänennamen, z. B. **contoso.com**bereitstellen. Dieser Artikel erläutert oder einen benutzerdefinierten Domänennamen für Webrollen Cloud-Dienst konfigurieren.

Haben Sie bereits Undestand welche CNAME und A-Einträge sind? [Sprung nach der Erklärung](#add-a-cname-record-for-your-custom-domain).

> [AZURE.NOTE]
> Die Verfahren in dieser Aufgabe gelten Azure Cloud Services. App-Dienste finden Sie [diese](../app-service-web/web-sites-custom-domain-name.md). Speicherkonten finden Sie [diese](../storage/storage-custom-domain-name.md).

<p/>

> [AZURE.TIP]
> Überblick schneller – neue Azure [geführte Exemplarische Vorgehensweise](http://support.microsoft.com/kb/2990804)verwenden.  Zuordnen einen benutzerdefinierten Domänennamen und Sichern der Kommunikation (SSL) macht Azure Cloud Services und Azure Websites ein Kinderspiel.

## <a name="understand-cname-and-a-records"></a>Verstehen Sie, CNAME und A-Einträge

CNAME (oder Alias-Datensätze) und Datensätze sowohl ein Domänenname mit einem bestimmten Server zuordnen (oder in diesem Fall service) jedoch anders funktionieren. Es gibt auch einige Besonderheiten Azure Cloud Services A-Datensätze mit, die Sie berücksichtigen sollten, bevor Sie sich entscheiden, verwenden.

### <a name="cname-or-alias-record"></a>CNAME oder Aliasdatensatz

Ein CNAME-Eintrag ordnet eine *bestimmten* Domäne **contoso.com** oder **www.contoso.com**kanonische Domänennamen. In diesem Fall die kanonische Domänenname ist die **[Anwendung] .cloudapp .net** Domänennamen der Azure gehostete Anwendung. Nach dem Erstellen der CNAME-Eintrag erstellt einen Alias für das **[Anwendung] .cloudapp .net**. CNAME-Eintrag wird die IP-Adresse aufgelöst Ihre **[Anwendung] .cloudapp .net** service automatisch so ändert die IP-Adresse des Cloud-Dienst Sie keinen ergreifen müssen.

> [AZURE.NOTE]
> Domain-Registrierer können nur Unterdomänen zugeordnet, wenn ein CNAME-Eintrag wie www.contoso.com nicht Root Namen contoso.com verwenden. Weitere Informationen auf CNAME-Einträge finden Sie in der Dokumentation Ihrer Registrierungsstelle, [Wikipedia-Eintrag CNAME-Datensatz](http://en.wikipedia.org/wiki/CNAME_record)oder dem Dokument [IETF Domain Names - Implementation and Specification](http://tools.ietf.org/html/rfc1035) .

### <a name="a-record"></a>Einen Datensatz

Ein *A* -Datensatz ordnet eine Domäne **contoso.com** oder **www.contoso.com** *oder eine Platzhalterdomäne* wie ** \*. contoso.com**, eine IP-Adresse. Bei einer Azure Cloud Service, die virtuelle IP-Adresse des Dienstes. Damit ist der wichtigste Vorteil der A-Datensatz über einen CNAME-Datensatz einen Eintrag enthalten kann, die wie einen Platzhalter verwendet \* **. contoso.com**, behandelt die Anfragen für mehrere untergeordnete Domänen wie **"Mail.contoso.com"**, **login.contoso.com**oder **www.contso.com**.

> [AZURE.NOTE]
> Da eine statische IP-Adresse ein A-Datensatz zugeordnet ist, kann es automatisch ändert die IP-Adresse des Cloud-Diensts nicht auflösen. Die IP-Adresse von Ihrem Cloud-Dienst wird beim ersten reserviert, die Bereitstellung einen leeren Steckplatz (Produktion oder Staging.) Löschen die Bereitstellung für den Slot die IP-Adresse wird von Azure freigegeben und zukünftigen Einsatz Steckplatz eine neue IP-Adresse angegeben werden können.
>
> Die IP-Adresse einer bestimmten Bereitstellung Steckplatz (Produktion oder Staging) wird, beim Austausch zwischen Staging und Produktion Bereitstellung oder eine direkte Aktualisierung einer vorhandenen Bereitstellung beibehalten. Weitere Informationen zum Ausführen dieser Aktionen finden Sie unter [Verwalten von Cloud-Diensten](cloud-services-how-to-manage.md).


## <a name="add-a-cname-record-for-your-custom-domain"></a>Ein CNAME-Eintrag für Ihre benutzerdefinierte Domain hinzufügen

Um einen CNAME-Eintrag zu erstellen, müssen Sie einen neuen Eintrag in der DNS-Tabelle für Ihre benutzerdefinierte Domain hinzufügen mithilfe der Tools von Ihrer Registrierungsstelle. Jeder Registrierungsstelle hat eine ähnliche aber geringfügig CNAME-Eintrag angeben, aber die Konzepte sind dieselben.

1. Verwenden Sie eine der folgenden Methoden zu der **. cloudapp.net** Domänennamen zu Ihrem Clouddienst zugewiesen.

    * Anmeldung zum [Azure-Portal], wählen Sie Ihre Cloud-Dienst die **Essentials** Abschnitt und suchen Sie den **Site-URL** -Eintrag.

        ![Blick Abschnitt zeigt die Website-URL][csurl]
            
        **ODER**
  
    * Installieren und Konfigurieren von [Azure Powershell](../powershell-install-configure.md), und verwenden Sie den folgenden Befehl:

        ```powershell
        Get-AzureDeployment -ServiceName yourservicename | Select Url
        ```
    
    Speichern Sie den Domänennamen in von einer Methode zurückgegebene URL verwendet, wie Sie ihn benötigen einen CNAME-Datensatz erstellen.

1.  Melden Sie sich bei DNS der Website Ihrer Registrierungsstelle und zur Seite zum Verwalten von DNS. Suchen Sie nach Links oder Bereiche der Website als **Domänennamen**, **DNS**oder **Name-Server-Management**.

2.  Jetzt können Sie auswählen oder Eingeben des CNAME finden. Möglicherweise wählen Sie den Datensatz aus einer nach unten oder auf einer Seite erweitert. Sie sollten Wörter **CNAME** **Alias**oder **Unterdomänen**suchen.

3.  Auch geben den Domäne oder Unterdomäne Alias für CNAME wie **Www** Sie ggf. einen Alias für **www.customdomain.com**erstellen. Wenn Sie einen Alias für die Stammdomäne erstellen möchten, kann es als geführt der '**@**' Symbol Ihres DNS-Tools.

4. Dann müssen Sie einen kanonischen Hostnamen in diesem Fall ist die Anwendung **cloudapp.net** Domäne bereitstellen.

CNAME-Eintrag leitet beispielsweise alle Datenverkehr von **www.contoso.com** an **contoso.cloudapp.net**, den benutzerdefinierten Domänennamen der bereitgestellten Anwendung:

| Alias-Host Name/Domäne | Kanonische Domäne     |
| ------------------------- | -------------------- |
| www                       | Contoso.cloudapp.NET |

> [AZURE.NOTE]
Besucher **www.contoso.com** sehen niemals true Host (contoso.cloudapp.net), damit die Weiterleitung an den Endbenutzer nicht sichtbar ist.

> Im Beispiel oben gilt nur für Datenverkehr am **Www** -Domäne. Da Sie Platzhalter mit CNAME-Datensätzen verwenden können, müssen Sie für jede Domäne-Unterdomäne ein CNAME erstellen. Möchten Sie Datenverkehr von untergeordneten Domänen wie direkte *. contoso.com, an die Adresse cloudapp.net können eine * *URL umleiten* * oder * *URL** Eintrag in Ihrem DNS-Clienteinstellungen oder einen A-Eintrag zu erstellen.


## <a name="add-an-a-record-for-your-custom-domain"></a>Fügen Sie einen A-Eintrag für Ihre benutzerdefinierte domain

Finden Sie die virtuelle IP-Adresse dem Cloud-Dienst, um einen A-Eintrag zu erstellen. Fügen Sie einen neuen Eintrag in der DNS-Tabelle für Ihre benutzerdefinierte Domain, mithilfe der Tools von Ihrer Registrierungsstelle. Jeder Registrierungsstelle hat eine ähnliche aber geringfügig einen A-Datensatz angeben, aber die Konzepte sind dieselben.

1. Verwenden Sie eine der folgenden Methoden an die IP-Adresse des Cloud-Diensts.

    * Anmeldung [Azure-Portal]Cloud-Dienst auswählen, sehen Sie die **Grundlagen** und suchen Sie den Eintrag **öffentliche IP-Adressen** .

        ![Blick Abschnitt zeigt die VIP][vip]

        **ODER**

    * Installieren und Konfigurieren von [Azure Powershell](../powershell-install-configure.md), und verwenden Sie den folgenden Befehl:

        ```powershell
        get-azurevm -servicename yourservicename | get-azureendpoint -VM {$_.VM} | select Vip
        ```
    
    Speichern Sie die IP-Adresse Bedarf wird beim Erstellen eines A-Datensatzes.

1.  Melden Sie sich bei DNS der Website Ihrer Registrierungsstelle und zur Seite zum Verwalten von DNS. Suchen Sie nach Links oder Bereiche der Website als **Domänennamen**, **DNS**oder **Name-Server-Management**.

2.  Jetzt können wählen oder geben Sie einen Datensatz suchen. Möglicherweise wählen Sie den Datensatz aus einer nach unten oder auf einer Seite erweitert.

3. Wählen Sie oder geben Sie die Domäne oder Subdomäne, die diesen A-Datensatz verwendet wird. Wählen Sie z. B. **Www** , wenn Sie einen Alias für **www.customdomain.com**erstellen. Geben Sie ggf. einen Platzhaltereintrag für alle Unterdomänen erstellen '__*__'. Dies umfasst alle Unterdomänen wie **mail.customdomain.com**, **login.customdomain.com**und **www.customdomain.com**.

    Wenn Sie einen A-Eintrag für die Stammdomäne erstellen möchten, kann es als aufgeführt die '**@**' Symbol Ihres DNS-Tools.

4. Geben Sie die IP-Adresse dem Cloud-Dienst im angegebenen Feld. Ordnet den Domain-Eintrag in den A-Eintrag mit der IP-Adresse der Cloud Service-Bereitstellung verwendet.

Beispielsweise folgenden, die ein Datensatz aller Datenverkehr von **contoso.com** in **137.135.70.239**, die IP-Adresse der bereitgestellten Anwendung weiterleitet:

| Host Name/Domäne | IP-Adresse     |
| ------------------- | -------------- |
| @                   | 137.135.70.239 |


Dieses Beispiel veranschaulicht einen A-Eintrag für die Stammdomäne erstellen. Wenn einen Platzhaltereintrag auf alle untergeordneten Domänen erstellen möchten, geben Sie "__*__" als der Unterdomäne.

>[AZURE.WARNING]
>IP-Adressen in Azure sind standardmäßig dynamisch. Sie möchten wahrscheinlich eine [reservierte IP-Adresse](../virtual-network/virtual-networks-reserved-public-ip.md) verwenden, um sicherzustellen, dass Ihre IP-Adresse nicht ändert.

## <a name="next-steps"></a>Nächste Schritte

* [Cloud-Dienste verwalten](cloud-services-how-to-manage.md)
* [Zum Zuordnen von CDN Inhalt zu einer benutzerdefinierten Domäne](../cdn/cdn-map-content-to-custom-domain.md)
* [Konfiguration des Cloud-Dienst](cloud-services-how-to-configure-portal.md).
* Erfahren Sie, wie [einen Cloud-Dienst bereitgestellt](cloud-services-how-to-create-deploy-portal.md).
* Konfigurieren von [Ssl-Zertifikaten](cloud-services-configure-ssl-certificate-portal.md).

[Expose Your Application on a Custom Domain]: #access-app
[Add a CNAME Record for Your Custom Domain]: #add-cname
[Expose Your Data on a Custom Domain]: #access-data
[VIP swaps]: cloud-services-how-to-manage-portal.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
[Create a CNAME record that associates the subdomain with the storage account]: #create-cname
[Azure-portal]: https://portal.azure.com
[vip]: ./media/cloud-services-custom-domain-name-portal/csvip.png
[csurl]: ./media/cloud-services-custom-domain-name-portal/csurl.png
 