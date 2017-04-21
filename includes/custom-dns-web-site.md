#<a name="configuring-a-custom-domain-name-for-an-azure-website"></a>Konfigurieren einen benutzerdefinierten Domänennamen für eine Azure-website

Beim Erstellen einer Website Azure bietet angezeigte Domäne *.azurewebsites.NET Domäne, sodass die Benutzer Ihre Website über einen URL wie http:// zugreifen können&lt;meinesite >. *.azurewebsites.NET. Konfigurieren Ihrer Websites freigegeben oder Standard-Modus können Sie Ihre Website auf Ihren eigenen Domänennamen zuordnen.

Optional können Sie Azure Traffic Manager Saldo eingehenden Datenverkehr auf Ihrer Website geladen. Weitere Informationen zur Funktionsweise von Traffic Manager mit Websites finden Sie unter [Steuern von Azure Websites Verkehr mit Azure Traffic Manager][trafficmanager].

> [AZURE.NOTE] Die Verfahren in dieser Aufgabe gelten für Azure Websites. Cloud-Dienste finden Sie unter <a href="/develop/net/common-tasks/custom-dns/">Konfigurieren einer benutzerdefinierten Domänennamen in Azure</a>.

> [AZURE.NOTE] Die Schritte in dieser Aufgabe müssen Sie Konfigurieren Ihrer Websites für Shared oder Standard Modus ändern kann, wie viel Sie für Ihr Abonnement abgerechnet werden. Weitere Informationen finden Sie unter <a href="/pricing/details/web-sites/">Preisdetails Websites</a> .

In diesem Artikel:

-   [Grundlegendes zu CNAME und A-Einträge](#understanding-records)
-   [Konfigurieren Sie Ihrer Websites für freigegebene oder Standardmodus](#bkmk_configsharedmode)
-   [Fügen Sie Ihre Websites Traffic Manager](#trafficmanager)
-   [CNAME für Ihre benutzerdefinierte Domain hinzufügen](#bkmk_configurecname)
-   [Fügen Sie einen A-Eintrag für Ihre benutzerdefinierte domain](#bkmk_configurearecord)

<h2><a name="understanding-records"></a>Verstehen Sie, CNAME und A-Einträge</h2>

CNAME (oder Alias-Datensätze) und Datensätze sowohl einen Domänennamen mit einer Website verknüpfen aber jeweils unterschiedlich.

###<a name="cname-or-alias-record"></a>CNAME oder Aliasdatensatz

Ein CNAME-Eintrag ordnet eine *bestimmten* Domäne **contoso.com** oder **www.contoso.com**kanonische Domänennamen. In diesem Fall der kanonischen Domänennamen ist die ** &lt;Myapp >. *.azurewebsites.NET** Domänennamen Ihrer Azure Website oder ** &lt;Myapp >. trafficmgr.com** Domänennamen Ihres Profils Traffic Manager. Nach dem Erstellen der CNAME-Eintrag erstellt einen Alias für die ** &lt;Myapp >. *.azurewebsites.NET** oder ** &lt;Myapp >. trafficmgr.com** Domänennamen. CNAME-Eintrag wird die IP-Adresse auflösen der ** &lt;Myapp >. *.azurewebsites.NET** oder ** &lt;Myapp >. trafficmgr.com** Domänenname, so ändert die IP-Adresse der Website haben Sie keinen ergreifen.

> [AZURE.NOTE] Domain-Registrierer können nur Unterdomänen zugeordnet, wenn ein CNAME-Eintrag wie www.contoso.com nicht Root Namen contoso.com verwenden. Weitere Informationen auf CNAME-Einträge finden Sie in der Dokumentation Ihrer Registrierungsstelle, <a href="http://en.wikipedia.org/wiki/CNAME_record">Wikipedia-Eintrag CNAME-Datensatz</a>oder dem Dokument <a href="http://tools.ietf.org/html/rfc1035">IETF Domain Names - Implementation and Specification</a> .

###<a name="a-record"></a>Einen Datensatz

Ein A-Datensatz ordnet eine Domäne **contoso.com** oder **www.contoso.com** *oder eine Platzhalterdomäne* wie ** \*. contoso.com**, eine IP-Adresse. Bei einer Website Azure Adresse virtuelle IP-Adresse des Dienstes oder einer bestimmten IP-, die Sie für Ihre Website gekauft. Damit ist der wichtigste Vorteil der A-Datensatz über einen CNAME-Datensatz einen Eintrag enthalten kann, die wie einen Platzhalter verwendet * **. contoso.com**, behandelt die Anfragen für mehrere Unterdomänen wie * *"Mail.contoso.com"**, * *login.contoso.com**, oder * *www.contso.com**.

> [AZURE.NOTE] Da eine statische IP-Adresse ein A-Datensatz zugeordnet ist, kann es automatisch ändert die IP-Adresse Ihrer Website nicht auflösen. Eine IP-Adresse mit einem Datensätze wird bereitgestellt, wenn Sie benutzerdefinierte Domain Name für Ihre Website konfigurieren. Dieser Wert kann jedoch ändern, löschen und neu erstellen die Website oder die Website wechseln zurück zum Freigeben.

> [AZURE.NOTE] A-Datensätze können nicht für den Lastenausgleich mit Traffic Manager verwendet werden. Weitere Informationen finden Sie unter [Steuern von Azure Websites Verkehr mit Azure Traffic Manager][trafficmanager].

<a name="bkmk_configsharedmode"></a><h2>Konfigurieren Sie Ihre Websites für freigegebene oder standard</h2>

Festlegen einen benutzerdefinierten Domänennamen auf einer Website ist nur für Shared und Standardmodi Azure Websites verfügbar. Bevor Sie eine Website aus der Website finden, freigegeben oder Standard-Website Modus wechseln, müssen Sie zuerst Ausgaben Caps an Ihr Abonnement Website entfernen. Weitere Informationen zu freigegebenen und Standard Modus Preisen finden Sie unter [Preisdetails][PricingDetails].

1. Öffnen Sie in Ihrem Browser das [Verwaltungsportal][portal].
2. Klicken Sie auf der Registerkarte **Websites** Ihrer Website.

    ![][standardmode1]

3. Klicken Sie auf die Registerkarte **Skalierung** .

    ![][standardmode2]


4. Im **Abschnitt** Festlegen des Website-Modus auf **SHARED**.

    ![][standardmode3]

    > [AZURE.NOTE] Wenn Sie Traffic Manager mit dieser Website verwenden, müssen Sie statt Shared wählen Standardmodus verwenden.

5. Klicken Sie auf **Speichern**.
6. Aufforderung zum Anstieg der Kosten Freigabemodus (oder Standardmodus wählt Standard), klicken Sie auf **Ja** , wenn Sie zustimmen.

    <!--![][standardmode4]-->

    **Hinweis**<br />
   Wenn eine Fehlermeldung "Configuring Skalieren für Website 'Websitename konnte" können die Detailsschaltfläche Sie um weitere Informationen zu erhalten.

<a name="trafficmanager"></a><h2>(Optional) Traffic Manager Ihre Websites hinzufügen</h2>

Wenn Sie Ihre Website mit Traffic Manager verwenden möchten, führen Sie die folgenden Schritte.

1. Wenn Sie bereits ein Profil Traffic Manager keinen, anhand der Informationen in [Traffic Manager Profil mit schnellen Erstellen erstellen] [ createprofile] zu erstellen. Hinweis Die **. trafficmgr.com** Domänennamen der Traffic Manager-Profil zugeordnet. Dies wird in einem späteren Schritt verwendet werden.

2. Verwenden Sie die Informationen in [Delete Endpoints] [ addendpoint] Ihrer Website als Endpunkt der Traffic Manager-Profil hinzufügen.

    > [AZURE.NOTE] Wenn Sie einen Endpunkt hinzufügen Ihrer Website nicht aufgeführt ist, sicher, dass für Standard-Modus konfiguriert ist. Standardmodus für Ihre Website verwenden mit Traffic Manager arbeiten.

3. Melden Sie sich bei DNS der Website Ihrer Registrierungsstelle und zur Seite zum Verwalten von DNS. Suchen Sie nach Links oder Bereiche der Website als **Domänennamen**, **DNS**oder **Name-Server-Management**.

4. Jetzt suchen Sie können wählen oder CNAME-Datensätze eingeben. Möglicherweise wählen Sie den Datensatz aus einer nach unten oder auf einer Seite erweitert. Sie sollten Wörter **CNAME** **Alias**oder **Unterdomänen**suchen.

5. Sie müssen auch Domäne oder Unterdomäne ein Alias für den CNAME-Eintrag angeben. Zum Beispiel **Www** möchten Sie einen Alias für **www.customdomain.com**erstellen.

5. Sie müssen auch einen Hostnamen angeben, die den kanonischen Namen CNAME-Alias. Dies ist die **. trafficmgr.com** für Ihre Website.

CNAME-Eintrag leitet beispielsweise alle Datenverkehr von **www.contoso.com** an **contoso.trafficmgr.com**, der Domänenname einer Website:

<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tr>
<td><strong>Alias-Host Name/Domäne</strong></td>
<td><strong>Kanonische Domäne</strong></td>
</tr>
<tr>
<td>www</td>
<td>Contoso.trafficmgr.com</td>
</tr>
</table>

Besucher **www.contoso.com** sehen niemals true Host (contoso.azurewebsite.net), damit die Weiterleitung an den Endbenutzer nicht sichtbar ist.

> [AZURE.NOTE] Bei Verwendung von Traffic Manager mit einer Website müssen Sie nicht die Schritte in den folgenden Abschnitten "**CNAME für Ihre benutzerdefinierte Domain hinzufügen**" und "**A-Eintrag für Ihre benutzerdefinierte Domain hinzufügen**". Der CNAME-Eintrag in den vorherigen Schritten erstellten leitet eingehenden Datenverkehr zu Traffic Manager, dann den Datenverkehr an die Website-Endpunkte weiterleitet.

<a name="bkmk_configurecname"></a><h2>CNAME für Ihre benutzerdefinierte Domain hinzufügen</h2>

Um einen CNAME-Eintrag zu erstellen, müssen Sie einen neuen Eintrag in der DNS-Tabelle für Ihre benutzerdefinierte Domain hinzufügen mit Tools von Ihrer Registrierungsstelle. Jeder Registrierungsstelle hat eine ähnliche aber geringfügig CNAME-Eintrag angeben, aber die Konzepte sind dieselben.

1. Verwenden Sie eine der folgenden Methoden zu der **. azurewebsite.net** Domänennamen der Website zugewiesen.

    * Anmeldung bei [Azure-Verwaltungsportal][portal], wählen Sie Ihre Website **Dashboard aus**und suchen Sie den **Site-URL** Eintrag im Abschnitt **Blick** .

    * Installieren und Konfigurieren von [Azure Powershell](/manage/install-and-configure-windows-powershell/), und verwenden Sie den folgenden Befehl:

            get-azurewebsite yoursitename | select hostnames

    * Installieren Sie und konfigurieren Sie der [Azure-Befehlszeilenschnittstelle](/manage/install-and-configure-cli/), und verwenden Sie den folgenden Befehl:

            azure site domain list yoursitename

    Speichern **. azurewebsite.net** gemäß den folgenden Schritten verwendet wird.

3. Melden Sie sich bei DNS der Website Ihrer Registrierungsstelle und zur Seite zum Verwalten von DNS. Suchen Sie nach Links oder Bereiche der Website als **Domänennamen**, **DNS**oder **Name-Server-Management**.

4. Jetzt suchen Sie können wählen oder CNAME-Datensätze eingeben. Möglicherweise wählen Sie den Datensatz aus einer nach unten oder auf einer Seite erweitert. Sie sollten Wörter **CNAME** **Alias**oder **Unterdomänen**suchen.

5. Sie müssen auch Domäne oder Unterdomäne ein Alias für den CNAME-Eintrag angeben. Zum Beispiel **Www** möchten Sie einen Alias für **www.customdomain.com**erstellen. Wenn Sie einen Alias für die Stammdomäne erstellen möchten, kann es als geführt der '**@**' Symbol Ihres DNS-Tools.

5. Sie müssen auch einen Hostnamen angeben, die den kanonischen Namen CNAME-Alias. Dies ist die **. azurewebsite.net** für Ihre Website.

CNAME-Eintrag leitet beispielsweise alle Datenverkehr von **www.contoso.com** an **contoso.azurewebsite.net**, der Domänenname einer Website:

<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tr>
<td><strong>Alias-Host Name/Domäne</strong></td>
<td><strong>Kanonische Domäne</strong></td>
</tr>
<tr>
<td>www</td>
<td>Contoso.azurewebsite.NET</td>
</tr>
</table>

Besucher **www.contoso.com** sehen niemals true Host (contoso.azurewebsite.net), damit die Weiterleitung an den Endbenutzer nicht sichtbar ist.

> [AZURE.NOTE] Im Beispiel oben gilt nur für Datenverkehr am __Www__ -Domäne. Da Sie Platzhalter mit CNAME-Datensätzen verwenden können, müssen Sie für jede Domäne-Unterdomäne ein CNAME erstellen. Möchten Sie Datenverkehr von untergeordneten Domänen wie direkte *. contoso.com, an die Adresse azurewebsite.net können Sie __URL umleiten__ oder __URL weiterleiten__ Eintrag in Ihrem DNS-Clienteinstellungen konfigurieren oder einen A-Eintrag zu erstellen.

> [AZURE.NOTE] Es dauert einige Zeit Ihre CNAME über das DNS-System übertragen. Der CNAME-Eintrag für die Website kann nicht festgelegt werden, bis der CNAME-Eintrag übermittelt wurde. Einen Dienst wie <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> können Sie überprüfen, ob der CNAME-Eintrag verfügbar ist.

###<a name="add-the-domain-name-to-your-website"></a>Der Domänenname der Website hinzufügen

Nachdem CNAME-Eintrag für Domänennamen weitergegeben wurde, müssen Sie sie mit Ihrer Website zuordnen. Sie können den benutzerdefinierten Domänennamen definiert durch den CNAME-Eintrag Ihrer Website entweder Azure Befehlszeilenschnittstelle (CLI Azure) oder mithilfe der Azure-Verwaltungsportal hinzufügen.

**Einen Domänennamen mit den Befehlszeilenprogrammen hinzufügen**

Installieren Sie und konfigurieren Sie der [Azure-Befehlszeilenschnittstelle](/manage/install-and-configure-cli/), und verwenden Sie den folgenden Befehl:

    azure site domain add customdomain yoursitename

Beispielsweise wird die folgende benutzerdefinierte Namen **www.contoso.com** **contoso.azurewebsite.net** Website hinzugefügt:

    azure site domain add www.contoso.com contoso

Sie können bestätigen, dass der benutzerdefinierten Domänennamen der Website mit dem folgenden Befehl hinzugefügt wurde:

    azure site domain list yoursitename

Mit diesem Befehl zurückgegebene Liste sollte den benutzerdefinierten Domänennamen sowie die Standardeinstellung **. azurewebsite.net** Eintrag.

**Einen Domänennamen Azure-Verwaltungsportal hinzufügen**

1. Öffnen Sie in Ihrem Browser [Azure-Verwaltungsportal][portal].

2. Klicken Sie auf der Registerkarte **Websites** auf den Namen der Website **Dashboard aus**und wählen Sie dann am unteren Rand der Seite **Domänen verwalten** .

    ![][setcname2]

6. Geben Sie im Textfeld **DOMÄNENNAMEN** der Domänenname konfiguriert haben.

    ![][setcname3]

6. Klicken Sie auf das Häkchen, um den Domänennamen zu übernehmen.

Nach Abschluss der Konfiguration wird der benutzerdefinierten Domänennamen im Abschnitt **Domänennamen** **Konfigurieren** auf Ihrer Website aufgelistet.

<a name="bkmk_configurearecord"></a><h2>Fügen Sie einen A-Eintrag für Ihre benutzerdefinierte domain</h2>

Finden Sie die IP-Adresse der Website, um einen A-Eintrag zu erstellen. Fügen Sie einen Eintrag in der DNS-Tabelle für Ihre benutzerdefinierte Domain, mithilfe der Tools von Ihrer Registrierungsstelle. Jeder Registrierungsstelle hat eine ähnliche aber geringfügig einen A-Datensatz angeben, aber die Konzepte sind dieselben. Neben einem A-Eintrag, müssen Sie auch einen CNAME-Eintrag erstellen, den Azure verwendet den A-Eintrag zu überprüfen.

1. Öffnen Sie in Ihrem Browser [Azure-Verwaltungsportal][portal].

2. Klicken Sie auf der Registerkarte **Websites** auf den Namen der Website wählen Sie **Dashboard aus**und den unteren Rand des Bildschirms Wählen Sie **Domänen verwalten** .

    ![][setcname2]

5. Suchen Sie im Dialogfeld **benutzerdefinierte Domänen verwalten** **Der IP-Adresse beim Konfigurieren einer Datensätze**. Kopieren Sie die IP-Adresse. Dies wird verwendet, wenn ein Datensatz erstellt.

5. Beachten Sie im Dialogfeld **benutzerdefinierte Domänen verwalten** Awverify Domänennamen am Ende den Text am oberen Rand des Dialogfeldes. Es sollte **awverify.mysite.azurewebsites.net** sein, wobei **meinesite** den Namen Ihrer Website. Kopieren Sie, wie der Domänenname verwendet, wenn die Überprüfung CNAME-Eintrag erstellt.

6. Melden Sie sich bei DNS der Website Ihrer Registrierungsstelle und zur Seite zum Verwalten von DNS. Suchen Sie nach Links oder Bereiche der Website als **Domänennamen**, **DNS**oder **Name-Server-Management**.

6. Suchen, wählen Sie oder geben Sie A- und CNAME-Einträge. Möglicherweise wählen Sie den Datensatz aus einer nach unten oder auf einer Seite erweitert.

7. Führen Sie die folgenden Schritte aus, um den A-Eintrag zu erstellen:

    1. Wählen Sie oder geben Sie die Domäne oder Subdomäne, die den A-Datensatz verwendet wird. Wählen Sie z. B. **Www** , wenn Sie einen Alias für **www.customdomain.com**erstellen. Geben Sie ggf. einen Platzhaltereintrag für alle Unterdomänen erstellen '__*__'. Dies umfasst alle Unterdomänen wie **mail.customdomain.com**, **login.customdomain.com**und **www.customdomain.com**.

        Wenn Sie einen A-Eintrag für die Stammdomäne erstellen möchten, kann es als aufgeführt die '**@**' Symbol Ihres DNS-Tools.

    2. Geben Sie die IP-Adresse dem Cloud-Dienst im angegebenen Feld. Ordnet den Domain-Eintrag in den A-Eintrag mit der IP-Adresse der Cloud Service-Bereitstellung verwendet.

        Beispielsweise folgenden, die ein Datensatz aller Datenverkehr von **contoso.com** in **137.135.70.239**, die IP-Adresse der bereitgestellten Anwendung weiterleitet:

        <table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
        <tr>
        <td><strong>Host Name/Domäne</strong></td>
        <td><strong>IP-Adresse</strong></td>
        </tr>
        <tr>
        <td>@</td>
        <td>137.135.70.239</td>
        </tr>
        </table>

        Dieses Beispiel veranschaulicht einen A-Eintrag für die Stammdomäne erstellen. Wenn einen Platzhaltereintrag auf alle untergeordneten Domänen erstellen möchten, geben Sie "__*__" als der Unterdomäne.

7. Erstellen Sie anschließend einen CNAME-Eintrag, der Alias **Awverify**und eine kanonische Domäne der **awverify.mysite.azurewebsites.net** , die Sie zuvor erworben haben.

    > [AZURE.NOTE] Während Alias Awverify für einige Registrierstellen funktionieren, benötigen andere Domänennamen vollständige Alias Awverify.www.customdomainname.com oder awverify.customdomainname.com.

    Beispielsweise erstellt folgende CNAME-Eintrag, mit denen Azure überprüfen die ein.

    <table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
    <tr>
    <td><strong>Alias-Host Name/Domäne</strong></td>
    <td><strong>Kanonische Domäne</strong></td>
    </tr>
    <tr>
    <td>awverify</td>
    <td>awverify.contoso.azurewebsites.NET</td>
    </tr>
    </table>

> [AZURE.NOTE] Es dauert einige Zeit Awverify CNAME über das DNS-System übertragen. Sie können nicht bis Awverify CNAME übermittelt wurde durch den A-Eintrag für die Website definierten benutzerdefinierten Domänennamen festlegen. Einen Dienst wie <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> können Sie überprüfen, ob der CNAME-Eintrag verfügbar ist.

###<a name="add-the-domain-name-to-your-website"></a>Der Domänenname der Website hinzufügen

Nach **Awverify** CNAME-Eintrag für Domänennamen übermittelt wurde, können Sie die benutzerdefinierte Domäne definiert ein Datensatz mit Ihrer Website zuordnen. Sie können den benutzerdefinierten Domänennamen definiert ein Eintrag in der Website mithilfe der Azure-CLI oder Azure-Verwaltungsportal hinzufügen.

**Einen Domänennamen der Azure-Befehlszeilenschnittstelle (CLI Azure) hinzufügen**

Installieren Sie und konfigurieren Sie der [Azure-CLI](/manage/install-and-configure-cli/)und verwenden Sie den folgenden Befehl:

    azure site domain add customdomain yoursitename

Beispielsweise wird der folgenden benutzerdefinierten Domänennamen **"contoso.com"** **contoso.azurewebsite.net** Website hinzugefügt:

    azure site domain add contoso.com contoso

Sie können bestätigen, dass der benutzerdefinierten Domänennamen der Website mit dem folgenden Befehl hinzugefügt wurde:

    azure site domain list yoursitename

Mit diesem Befehl zurückgegebene Liste sollte den benutzerdefinierten Domänennamen sowie die Standardeinstellung **. azurewebsite.net** Eintrag.

**Einen Domänennamen Azure-Verwaltungsportal hinzufügen**

1. Öffnen Sie in Ihrem Browser [Azure-Verwaltungsportal][portal].

2. Klicken Sie auf der Registerkarte **Websites** auf den Namen der Website **Dashboard aus**und wählen Sie dann am unteren Rand der Seite **Domänen verwalten** .

    ![][setcname2]

6. Geben Sie im Textfeld **DOMÄNENNAMEN** der Domänenname konfiguriert haben.

    ![][setcname3]

6. Klicken Sie auf das Häkchen, um den Domänennamen zu übernehmen.

Nach Abschluss der Konfiguration wird der benutzerdefinierten Domänennamen im Abschnitt **Domänennamen** **Konfigurieren** auf Ihrer Website aufgelistet.

> [AZURE.NOTE] Nachdem ein Datensatz Ihrer Website festgelegten benutzerdefinierten Domänennamen hinzugefügt haben, können Sie mit den Tools von Ihrer Registrierungsstelle Awverify CNAME-Eintrag entfernen. Allerdings möchten Sie eine andere hinzufügen Datensatz in Zukunft müssen Sie die Awverify neu Datensatz, bevor Sie den neuen Domänennamen zuordnen können mit dem neuen Datensatz mit der Website definiert.

## <a name="next-steps"></a>Nächste Schritte

-   [Verwalten von Websites](/manage/services/web-sites/how-to-manage-websites/)

-   [Konfigurieren Sie ein SSL-Zertifikat für Websites](/develop/net/common-tasks/enable-ssl-web-site/)


<!-- Bookmarks -->

[Configure your web sites for shared mode]: #bkmk_configsharedmode
[Configure the CNAME on your domain registrar]: #bkmk_configurecname
[Configure a CNAME verification record on your domain registrar]: #bkmk_configurecname
[Configure an A record for the domain name]:#bkmk_configurearecord
[Set the domain name in management portal]: #bkmk_setcname

<!-- Links -->

[PricingDetails]: /pricing/details/
[portal]: http://manage.windowsazure.com
[digweb]: http://www.digwebinterface.com/
[cloudservicedns]: ../articles/custom-dns.md
[trafficmanager]: ../articles/app-service-web/web-sites-traffic-manager.md
[addendpoint]: ../articles/traffic-manager/traffic-manager-endpoints.md
[createprofile]: ../articles/traffic-manager/traffic-manager-manage-profiles.md

<!-- images -->

[setcname1]: ../media/dncmntask-cname-5.png


<!-- images -->
[standardmode1]: ./media/custom-dns-web-site/dncmntask-cname-1.png
[standardmode2]: ./media/custom-dns-web-site/dncmntask-cname-2.png
[standardmode3]: ./media/custom-dns-web-site/dncmntask-cname-3.png
[standardmode4]: ./media/custom-dns-web-site/dncmntask-cname-4.png


[setcname2]: ./media/custom-dns-web-site/dncmntask-cname-6.png
[setcname3]: ./media/custom-dns-web-site/dncmntask-cname-7.png
