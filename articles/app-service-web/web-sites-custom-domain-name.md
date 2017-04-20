<properties
    pageTitle="Eine Azure-Anwendung einen benutzerdefinierten Domänennamen zuordnen"
    description="Erfahren Sie, wie Ihre Anwendung in Azure App Service einen benutzerdefinierten Domänennamen (personalisiertes Domäne) zuordnen."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="jimbe"
    tags="top-support-issue"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/27/2016"
    ms.author="cephalin"/>

# <a name="map-a-custom-domain-name-to-an-azure-app"></a>Eine Azure-Anwendung einen benutzerdefinierten Domänennamen zuordnen

[AZURE.INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

Dieser Artikel veranschaulicht die benutzerdefinierten Domänennamen WebApp, mobile-app Back-End-oder API-app in [Azure App Service](../app-service/app-service-value-prop-what-is.md)manuell zuordnen. 

Ihre Anwendung enthält bereits eine eindeutige Unterdomäne *.azurewebsites.NET. Beispielsweise ist der Name der Anwendung **Contoso**, ist der Domänenname **contoso.azurewebsites.net**. Sie können jedoch eine benutzerdefinierte Domäne zuordnen name app so, dass die URL wie `www.contoso.com`, Ihre Marke widerspiegelt.

>[AZURE.NOTE] Hilfe von Azure-Experten in [Azure-Foren](https://azure.microsoft.com/support/forums/). Für noch höheren Stufe der [Azure-Support-Website](https://azure.microsoft.com/support/options/) und klicken Sie auf **Support zu erhalten**.

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

## <a name="buy-a-new-custom-domain-in-azure-portal"></a>Kaufen Sie eine neue benutzerdefinierte Domäne in Azure-portal

Wenn Sie bereits einen Domainnamen gekauft haben, können Sie kaufen und direkt in Ihre Anwendung Einstellungen in [Azure-Portal](https://portal.azure.com)verwalten. Diese Option erleichtert Ihre Anwendung eine benutzerdefinierte Domäne zuordnen, ob Ihre app [Azure Traffic Manager](web-sites-traffic-manager-custom-domain-name.md) verwendet. 

Eine Anleitung finden Sie in der [benutzerdefinierten Domänennamen für App kaufen](custom-dns-web-site-buydomains-web-app.md).

## <a name="map-a-custom-domain-you-purchased-externally"></a>Zuordnen einer benutzerdefinierten Domäne extern erworbenen

Wenn Sie bereits eine benutzerdefinierte Domäne [Azure](https://azure.microsoft.com/services/dns/) DNS oder von einem Drittanbieter erworben haben, sind drei Hauptschritte zuordnen die benutzerdefinierte Domäne für Ihre Anwendung:

1. [Die IP-Adresse *(einen Datensatz)* Get app](#vip).
2. [Erstellen Sie die DNS-Datensätze, die auf Ihre Anwendung zuordnen](#createdns). 
    - **Wo**: die Domäne des Registrators Management-Tool (z. B. Azure DNS-GoDaddy usw.).
    - **Warum**: damit Ihre Domainregistrierungsstelle löst die gewünschte benutzerdefinierte Domäne Azure-Anwendung kennt.
1. [Aktivieren Sie den benutzerdefinierten Domänennamen für Ihre Azure-Anwendung](#enable).
    - **Wo**: [Azure-Portal](https://portal.azure.com).
    - **Warum**: damit Ihre app Anfragen zur benutzerdefinierten Domänennamen erkennt.
3. [Überprüfen der DNS-Verteilung](#verify).

### <a name="types-of-domains-you-can-map"></a>Typen von Domänen zugeordnet werden können

Azure App Service können Sie Ihre Anwendung folgende benutzerdefinierte Domänen zuordnen.

- **Stammdomäne** – der Domänenname mit dem Domain-Registrar reserviert (dargestellt durch die `@` Datensatz normalerweise hosten). Z. B. **contoso.com**.
- **Domäne** - allen Domänen unter der Stammdomäne. Zum Beispiel **www.contoso.com** (dargestellt durch die `www` Eintrag).  Sie können andere apps in Azure verschiedene Unterdomänen dieselbe Stammdomäne zuordnen.
- **Platzhalterdomäne** - [Domäne, deren erste DNS-Bezeichnung ist `*` ](https://en.wikipedia.org/wiki/Wildcard_DNS_record) (z.B. Hostdatensätze `*` und `*.blogs`). Beispielsweise ** \*. contoso.com**.

### <a name="types-of-dns-records-you-can-use"></a>Typen von DNS-Einträgen verwenden können

Zwei Arten von standard-DNS-Einträge können Sie je nach Bedarf Ihre benutzerdefinierte Domain zuordnen: 

- [A](https://en.wikipedia.org/wiki/List_of_DNS_record_types#A) - Karten direkt Ihren benutzerdefinierten Domänennamen Azure-Anwendung virtuelle IP-Adresse. 
- [CNAME](https://en.wikipedia.org/wiki/CNAME_record) - ordnet Ihre benutzerdefinierten Domänennamen Ihrer app Azure Domänennamen, * *&lt;*Appname*>. azurewebsites.net**. 

Der Vorteil der CNAME werden weiterhin über IP-Adresse ändert. Wenn löschen und Ihre app erstellen oder Ändern von höheren Tarif in der **freigegebenen** Ebene kann Ihre Anwendung virtuelle IP-Adresse ändern. Durch eine solche Änderung gilt ein CNAME-Eintrag noch ein A-Datensatz eine Aktualisierung erfordert. 

Das Lernprogramm erfahren Sie Schritt für den A-Eintrag mit und für den CNAME-Eintrag verwenden.

>[AZURE.IMPORTANT] Erstellen Sie einen CNAME-Eintrag für Ihre Stammdomäne (d. h. der "Stammdatensatz"). Weitere Informationen finden Sie unter [Warum kein CNAME-Eintrag verwendet werden die Stammdomäne](http://serverfault.com/questions/613829/why-cant-a-cname-record-be-used-at-the-apex-aka-root-of-a-domain).
Um eine Stammdomäne der Azure-Anwendung zuzuordnen, verwenden Sie stattdessen einen A-Eintrag.

<a name="vip"></a>
## <a name="step-1-a-record-only-get-apps-ip-address"></a>Schritt 1. *(Einen Datensatz)* App IP-Adresse
Um einen benutzerdefinierten Domänennamen einen A-Datensatz zuzuordnen, benötigen Sie Ihre Azure-Anwendung IP-Adresse. Wenn Sie stattdessen einen CNAME-Eintrag zugeordnet sind, überspringen Sie diesen Schritt und auf den nächsten Abschnitt verschieben.

1.  Auf der [Azure-Portal](https://portal.azure.com)anmelden.

2.  Klicken Sie im linken Menü auf **Anwendungsdienste** .

4.  Ihre app, klicken Sie auf **Custom Domains**.

6.  Notieren Sie die IP-Adresse über Hostnamen Abschnitt...

    ![Karte benutzerdefinierten Domänennamen mit: Get IP-Adresse für Ihre Azure App Service](./media/web-sites-custom-domain-name/virtual-ip-address.png)

7.  Lassen Sie dieses Portal Blade geöffnet. Sie kommen zurück, nachdem Sie die DNS-Datensätze erstellt.

<a name="createdns"></a>
## <a name="step-2-create-the-dns-records"></a>Schritt 2. Die DNS-Datensätze erstellen

Melden Sie sich bei Ihrer Registrierungsstelle und ihre verwenden, um eine hinzufügen oder CNAME. Jeder Registrar Benutzeroberfläche unterscheidet sich geringfügig, so sollten Sie der Dokumentation Ihres Anbieters. Hier sind jedoch einige allgemeinen Richtlinien.

1.  Suchen Sie die Seite zum Verwalten von DNS-Einträgen. Suchen Sie nach Links oder Bereiche der Website mit der Bezeichnung **Domänennamen**, **DNS**oder **Name Server Management**. Häufig finden Sie den Link Kontoinformationen anzeigen und dann eine Verknüpfung wie **Meine Domains**.
2.  Eine Verknüpfung, mit der Sie hinzufügen oder Bearbeiten von DNS-Datensätzen, suchen. Eine **Zonendatei** oder **DNS-Datensätzen** verknüpfen oder eine **Erweiterte** Konfiguration Link möglicherweise.
3.  Den Datensatz erstellen und speichern.
    - [Anleitung für einen A-Datensatz sind](#a).
    - [Anleitung für einen CNAME-Datensatz sind](#cname).

<a name="a"></a>
### <a name="create-an-a-record"></a>Erstellen eines A-Datensatzes

Um einen A-Eintrag zu Azure app IP-Adresse zuzuordnen, müssen Sie tatsächlich einen A-Datensatz und TXT-Eintrag zu erstellen. Ein Datensatz ist für die DNS-Auflösung selbst, und TXT-Datensatz für Azure zu bestätigen, dass Sie den benutzerdefinierten Domänennamen. 

Ein Datensatz wie folgt konfigurieren (@ in der Regel stellt die Stammdomäne):
 
<table cellspacing="0" border="1">
  <tr>
    <th>FQDN-Beispiel</th>
    <th>Ein Host</th>
    <th>Wert</th>
  </tr>
  <tr>
    <td>Contoso.com (Stamm)</td>
    <td>@</td>
    <td>IP-Adresse <a href="#vip">Schritt 1</a></td>
  </tr>
  <tr>
    <td>www.contoso.com (Sub)</td>
    <td>www</td>
    <td>IP-Adresse <a href="#vip">Schritt 1</a></td>
  </tr>
  <tr>
    <td>*. contoso.com (Platzhalter)</td>
    <td>*</td>
    <td>IP-Adresse <a href="#vip">Schritt 1</a></td>
  </tr>
</table>

Zusätzliche Texteintrag übernimmt das Übereinkommen von Karten &lt; *Sub*>. &lt; *Rootdomain*>, &lt; *Anwendungsname*>. *.azurewebsites.NET. Konfigurieren Sie TXT-Eintrag wie folgt:

<table cellspacing="0" border="1">
  <tr>
    <th>FQDN-Beispiel</th>
    <th>TXT Host</th>
    <th>TXT-Wert</th>
  </tr>
  <tr>
    <td>Contoso.com (Stamm)</td>
    <td>@</td>
    <td>&lt;<i>Appname</i>>. *.azurewebsites.NET</td>
  </tr>
  <tr>
    <td>www.contoso.com (Sub)</td>
    <td>www</td>
    <td>&lt;<i>Appname</i>>. *.azurewebsites.NET</td>
  </tr>
  <tr>
    <td>*. contoso.com (Platzhalter)</td>
    <td>*</td>
    <td>&lt;<i>Appname</i>>. *.azurewebsites.NET</td>
  </tr>
</table>

<a name="cname"></a>
###Erstellen Sie einen CNAME-Eintrag

Verwenden Sie einen CNAME-Eintrag Standarddomänennamen Ihre Azure-Anwendung zuordnen, brauchen Sie keinen zusätzlichen TXT-Datensatz wie einen A-Eintrag. 

>[AZURE.IMPORTANT] Erstellen Sie einen CNAME-Eintrag für Ihre Stammdomäne (d. h. der "Stammdatensatz"). Weitere Informationen finden Sie unter [Warum kein CNAME-Eintrag verwendet werden die Stammdomäne](http://serverfault.com/questions/613829/why-cant-a-cname-record-be-used-at-the-apex-aka-root-of-a-domain).
Um eine Stammdomäne der Azure-Anwendung zuzuordnen, verwenden Sie stattdessen einen [A-Eintrag](#a) .

Konfigurieren Sie Ihren CNAME-Eintrag wie folgt (@ in der Regel stellt die Stammdomäne):

<table cellspacing="0" border="1">
  <tr>
    <th>FQDN-Beispiel</th>
    <th>CNAME-Host</th>
    <th>CNAME-Wert</th>
  </tr>
  <tr>
    <td>www.contoso.com (Sub)</td>
    <td>www</td>
    <td>&lt;<i>Appname</i>>. *.azurewebsites.NET</td>
  </tr>
  <tr>
    <td>*. contoso.com (Platzhalter)</td>
    <td>*</td>
    <td>&lt;<i>Appname</i>>. *.azurewebsites.NET</td>
  </tr>
</table>

<a name="enable"></a>
##Schritt 3. Aktivieren Sie den benutzerdefinierten Domänennamen für Ihre Anwendung

In **Benutzerdefinierten Domänen** Blade im Azure-Portal (siehe [Schritt 1](#vip)) müssen Sie den vollqualifizierten Domänennamen (FQDN) der benutzerdefinierten Domäne zur Liste hinzuzufügen.

1.  Wenn Sie den [Azure-Portal](https://portal.azure.com)Anmelden nicht getan.

2.  Klicken Sie in Azure-Portal im linken Menü auf **Anwendungsdienste** .

3.  Klicken Sie auf Ihre Anwendung **benutzerdefinierte Domänen** > **Hostnamen hinzufügen**.

4.  Fügen Sie den FQDN der benutzerdefinierten Domäne hinzu (z. B. **www.contoso.com**).

    ![Eine Azure-Anwendung einen benutzerdefinierten Domänennamen zuordnen: Hinzufügen von Domänennamen](./media/web-sites-custom-domain-name/add-custom-domain.png)

    >[AZURE.NOTE] Azure wird versucht, den Domänennamen zu überprüfen, den Sie verwenden. Sicher sein, dass sie denselben Domänennamen für den DNS-Datensatz in [Schritt2](#createdns)erstellt haben. 

5.  Klicken Sie auf **Überprüfen**.

6.  Beim Klicken auf **Validate** Azure Auftakt Domain Verification Workflow. Dies überprüft Domäne Besitz Hostname Verfügbarkeit und Bericht erfolgreich oder Fehler mit ausführlichen Anleitung zur Fehlerbehebung.    

7.  Nach erfolgreicher Überprüfung **Hostname hinzufügen** Schaltfläche aktiv, und Sie werden Hostnamen zuordnen. 

8.  Nach Azure Ihre benutzerdefinierten Domänennamen konfigurieren hat, navigieren Sie zu Ihren benutzerdefinierten Domänennamen in einem Browser. Der Browser sollte Ihre Azure-Anwendung öffnen Ihren benutzerdefinierten Domänennamen konfiguriert ist.

> [AZURE.NOTE] Ist der DNS-Eintrag bereits (aktive Domäne Serving Datenverkehr Szenario) und Sie müssen präventiv Ihrer Anwendung zur Überprüfung der Domäne gebunden, einfach erstellen Sie eine TXT-Datensätze als Beispiele in der folgenden Tabelle. Zusätzliche Texteintrag übernimmt das Übereinkommen von Karten &lt; *Sub*>. &lt; *Rootdomain*>, &lt; *Anwendungsname*>. *.azurewebsites.NET. 
> <table cellspacing="0" border="1">
  <tr>
    <th>FQDN-Beispiel</th>
    <th>TXT Host</th>
    <th>TXT-Wert</th>
  </tr>
  <tr>
    <td>Contoso.com (Stamm)</td>
    <td>awverify.contoso.com</td>
    <td>&lt;<i>Appname</i>>. *.azurewebsites.NET</td>
  </tr>
  <tr>
    <td>www.contoso.com (Sub)</td>
    <td>awverify.www.contoso.com</td>
    <td>&lt;<i>Appname</i>>. *.azurewebsites.NET</td>
  </tr>
    <tr>
    <td>*. contoso.com (Sub)</td>
    <td>awverify.*.contoso.com</td>
    <td>&lt;<i>Appname</i>>. *.azurewebsites.NET</td>
  </tr>
</table>
Wenn dieser DNS-Eintrag erstellt, Azure-Portal zurück und Ihrer Anwendung Ihren benutzerdefinierten Domänennamen hinzufügen.
 

<a name="verify"></a>
##DNS-Übermittlung überprüfen

Nach Abschluss der Konfigurationsschritte kann es eine Weile dauern ändert sich je nach DNS-Anbieter übertragen wird. Sie können überprüfen, ob die DNS-Übermittlung erwartungsgemäß mit [http://digwebinterface.com/](http://digwebinterface.com/). Nach dem Durchsuchen der Website geben Sie die Hostnamen im Textfeld und **graben**auf. Überprüfen Sie die Ergebnisse, um sicherzustellen, dass die Änderungen übernommen wurden.  

![Eine Azure-Anwendung einen benutzerdefinierten Domänennamen zuordnen: DNS-Übermittlung überprüfen](./media/web-sites-custom-domain-name/1-digwebinterface.png)

> [AZURE.NOTE] Die Weitergabe der DNS-Einträge kann (manchmal mehr) bis zu 48 Stunden dauern. Wenn Sie alles richtig konfiguriert haben, müssen Sie warten, bis die Übermittlung erfolgreich.

## <a name="next-steps"></a>Nächste Schritte
Informationen Sie zum Sichern Ihrer benutzerdefinierten Domänennamen mit HTTPS [ein SSL-Zertifikat in Azure kaufen](web-sites-purchase-ssl-web-site.md) oder [Verwenden Sie ein SSL-Zertifikat aus](web-sites-configure-ssl-certificate.md).

>[AZURE.NOTE] Wenn Sie mit Azure App Service beginnen, bevor Sie sich für ein Azure-Konto, gehen Sie [Versuchen App Service](http://go.microsoft.com/fwlink/?LinkId=523751)sofort eine kurzlebige Starter Web app in App Service können Sie erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

[Erste Schritte mit Azure DNS](../dns/dns-getstarted-create-dnszone.md)  
[Erstellen Sie DNS-Datensätze für eine Webanwendung in einer benutzerdefinierten Domäne](../dns/dns-web-sites-custom-domain.md)  
[Delegaten Azure DNS-Domäne](../dns/dns-domain-delegation.md)


<!-- Images -->
[subdomain]: media/web-sites-custom-domain-name/azurewebsites-subdomain.png
