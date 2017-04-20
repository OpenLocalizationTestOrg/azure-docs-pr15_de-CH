<properties
    pageTitle="Azure CDN Cloud-Dienst integriert | Microsoft Azure"
    description="Ein Lernprogramm, in dem einen Cloud-Dienst bereitgestellt, der Inhalte aus einem integrierten Azure CDN-Endpunkt unterstützt erklärt"
    services="cdn, cloud-services"
    documentationCenter=".net"
    authors="camsoper"
    manager="erikre"
    editor="tysonn"/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>


# <a name="intro"></a>Azure CDN Cloud-Dienst integriert

Azure CDN alle Inhalte aus der Cloud-Dienst kann ein Cloud-Dienst integriert. Dieser Ansatz bietet folgende Vorteile:

- Einfache Bereitstellung und Bilder, Skripts und Stylesheets in der Cloud-Dienst Projektverzeichnisse aktualisieren
- Einfaches upgrade NuGet-Pakete in der Cloud-Dienst wie jQuery oder Bootstrap-Versionen
- Verwalten von Webtests und CDN bedient Inhalt alles von der gleichen Visual Studio-Schnittstelle
- Einheitliche Bereitstellungsworkflow für Webtests und Ihre Inhalte CDN bereitgestellt
- Azure CDN ASP.NET bündeln und Verkleinerung integriert

## <a name="what-you-will-learn"></a>Lernziele ##

In diesem Lernprogramm erfahren Sie, wie:

-   [Cloud-Dienst integrieren Sie einen Endpunkt Azure CDN und bedienen Sie statischen Inhalt Ihrer Webseiten von Azure CDN](#deploy)
-   [Einstellungen Sie Cache-für statischen Inhalt in Ihrem Cloud-Dienst](#caching)
-   [Inhalt von Controller-Aktionen über Azure CDN bedienen](#controller)
-   [Gebündelt und minimierte Content über Azure CDN Beibehaltung Skript Debuggen in Visual Studio](#bundling)
-   [Konfigurieren Sie Ersatz der Skripts und CSS Azure CDN offline ist](#fallback)

## <a name="what-you-will-build"></a>Erstellen Sie ##

Sie eine Cloud-Dienst Webrolle mit ASP.NET MVC-Vorlage bereitstellen, fügen Sie Code zum Inhalt einer integrierten Azure CDN ein Bild Aktionsergebnisse Controller und der standardmäßigen JavaScript und CSS-Dateien dienen und auch Code schreiben, um Konfigurieren der Fallbackmechanismus für Pakete dass CDN offline ist.

## <a name="what-you-will-need"></a>Sie benötigen ##

Dieses Lernprogramm enthält die folgenden Komponenten:

-   Ein aktives [Microsoft Azure-Konto](/account/)
-   Visual Studio 2015 [Azure](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409) SDK

> [AZURE.NOTE] Ein Azure-Konto zum Bearbeiten dieses Lernprogramms benötigen Sie:
> + Sie können [ein Azure-Konto frei](/pricing/free-trial/) - Sie erhalten ein Guthaben können Sie ausprobieren, bezahlte Azure Services und sogar nachdem sie verwendet bis hält das Konto verwenden kostenlosen Azure Services wie Websites.
> + [Aktivieren Sie MSDN-Abonnementvorteile](/pricing/member-offers/msdn-benefits-details/) können – Ihr MSDN-Abonnement erhalten Sie Credits monatlich für bezahlte Azure Services verwenden können.

<a name="deploy"></a>
## <a name="deploy-a-cloud-service"></a>Bereitstellung eines Cloud-Diensts ##

In diesem Abschnitt wird standardmäßig ASP.NET MVC-Anwendungsvorlage in Visual Studio 2015 für einen Cloud-Dienst Webrolle bereitstellen und Integration mit einem neuen CDN-Endpunkt. Wie folgt:

1. In Visual Studio 2015 erstellen einen neuen Azure-Cloud-Dienst in der Menüleiste auf **Datei > Neu > Projekt > Cloud > Azure Cloud Service**. Geben sie einen Namen, und klicken Sie auf **OK**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-1-new-project.PNG)

2. **ASP.NET Web-Rolle** auswählen und auf die **>** Schaltfläche. Klicken Sie auf OK.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-2-select-role.PNG)

3. Wählen Sie **MVC** und auf **OK**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-3-mvc-template.PNG)

4. Jetzt veröffentlichen Sie diese Webrolle Azure Cloud-Dienst. Cloud-Dienstprojekt Maustaste und wählen Sie **Veröffentlichen**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-4-publish-a.png)

5. Wenn Sie Microsoft Azure noch nicht angemeldet haben, klicken Sie auf die Dropdownliste **ein Konto hinzufügen** und klicken Sie auf das Menüelement **Konto hinzufügen** .

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-5-publish-signin.png)

6. Die Anmeldeseite mit Azure freischalten verwendeten Microsoft-Konto anmelden.
7. Sobald Sie angemeldet sind, klicken Sie auf **Weiter**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-6-publish-signedin.png)

8. Sofern Sie ein Cloud-Dienst oder Storage-Konto erstellt haben, wird Visual Studio beide helfen. Geben Sie im Dialogfeld **Cloud-Dienst erstellen und Konto** den gewünschten Namen, und wählen Sie die gewünschte Region. Klicken Sie dann auf **Erstellen**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-7-publish-createserviceandstorage.png)

9. Überprüfen Sie die Konfiguration Einstellungsseite veröffentlichen und dann auf **Veröffentlichen**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-8-publish-finalize.png)

    >[AZURE.NOTE] Der Veröffentlichungsprozess für Clouddienste dauert sehr lange. Die aktivieren Web Deploy für alle Rollen Option, wird das Debuggen des Cloud-Dienstes schneller durch schnelle (aber temporäre) Updates für die Webrollen bereitstellen. Weitere Informationen zu dieser Option finden Sie unter [Veröffentlichen einer Cloud-Dienst mithilfe der Azure-Tools](http://msdn.microsoft.com/library/ff683672.aspx).

    **Microsoft Azure-Aktivitätsprotokoll** zeigt, dass der Veröffentlichungsstatus **abgeschlossen**ist, erstellen Sie einen CDN-Endpunkt, der Cloud-Dienst integriert.

    >[AZURE.WARNING] Wenn nach der Veröffentlichung bereitgestellte Cloud-Dienst einen Fehler anzeigt, ist es wahrscheinlich, da Cloud-Dienst eingesetzten [Gastbetriebssysteme, die keine .NET 4.5.2 umfasst,](../cloud-services/cloud-services-guestos-update-matrix.md#news-updates)verwendet wird.  Dieses Problem umgehen von [.NET 4.5.2 als starttask](../cloud-services/cloud-services-dotnet-install-dotnet.md).

## <a name="create-a-new-cdn-profile"></a>Erstellen eines neuen Profils CDN

Eine CDN-Profil besteht aus CDN-Endpunkte.  Jedes Profil enthält mindestens eine CDN-Endpunkte.  Sie können mehrere Profile CDN Endpunkte Internet-Domäne, Anwendung oder anderen Kriterien zu verwenden.

> [AZURE.TIP] Haben Sie bereits ein CDN-Profil, das Sie für dieses Lernprogramm verwenden möchten, fahren Sie mit [erstellen einen neuen CDN-Endpunkt](#create-a-new-cdn-endpoint).

[AZURE.INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Erstellen Sie eine neue CDN

**Neue CDN für das Speicherkonto erstellen**

1. Navigieren Sie in [Azure-Verwaltungsportal](https://portal.azure.com)zu Ihrem Profil CDN.  Sie können es dem Dashboard im vorherigen Schritt angeheftet haben.  Wenn Sie nicht Sie es finden auf **Durchsuchen**und **CDN-Profile**und auf im Profil den Endpunkt hinzugefügt werden soll.

    CDN Profil Blade wird angezeigt.

    ![CDN-Profil][cdn-profile-settings]

2. Klicken Sie auf **Endpunkt hinzufügen** .

    ![Endpunkt-Schaltfläche hinzufügen][cdn-new-endpoint-button]

    Das Blade **einen Endpunkt hinzufügen** wird angezeigt.

    ![Endpunkt Blade hinzufügen][cdn-add-endpoint]

3. Geben Sie einen **Namen** für diesen Endpunkt CDN.  Dieser Name wird verwendet, um die zwischengespeicherten Ressourcen in der Domäne zugreifen `<EndpointName>.azureedge.net`.

4. Wählen Sie in der Dropdownliste **Herkunftstyp** *Cloud-Dienst*.  

5. Wählen Sie in der Dropdownliste **Ursprung Hostname** Cloud-Dienst.

6. Belassen Sie die Standardeinstellungen für **Ursprünglicher Pfad** **Hostheader Ursprung**und **Protokoll-Ursprungs-Port**.  Sie müssen mindestens ein Protokoll (HTTP oder HTTPS) angeben.

7. Klicken Sie auf **Hinzufügen** , um den neuen Endpunkt zu erstellen.

8. Nach Erstellung der Endpunkt in einer Liste der Endpunkte des Profils angezeigt. Die Listenansicht zeigt die URL auf zwischengespeicherte Inhalte sowie der Ursprungsdomäne zugreifen.

    ![CDN-Endpunkt][cdn-endpoint-success]

    > [AZURE.NOTE] Der Endpunkt wird sind nicht sofort verfügbar.  Es dauert bis zu 90 Minuten die Registrierung über das CDN-Netzwerk weitergegeben. Benutzer versuchen, die CDN sofort verwenden erhalten Statuscode 404, bis der Inhalt über die CDN verfügbar ist.

## <a name="test-the-cdn-endpoint"></a>Testen des CDN-Endpunkts

Wenn der Veröffentlichungsstatus **abgeschlossen**ist, öffnen Sie ein Browserfenster, und navigieren Sie zu * *http://<cdnName>*.azureedge.net/Content/bootstrap.css**. In meinem Setup-URL ist:

    http://camservice.azureedge.net/Content/bootstrap.css

Die folgenden Ursprungs-URL CDN-Endpunkt entspricht:

    http://camcdnservice.cloudapp.net/Content/bootstrap.css

Beim Navigieren zu * *http://*&lt;CdnName >*.azureedge.net/Content/bootstrap.css**, abhängig von Ihrem Browser aufgefordert werden herunterladen oder öffnen die bootstrap.css, die aus Ihrer veröffentlichten Anwendung stammen.

![](media/cdn-cloud-service-with-cdn/cdn-1-browser-access.PNG)

Sie können ebenso alle öffentlich zugängliche URL zugreifen * *http://*&lt;Dienstname >*.cloudapp.net/** direkt aus CDN-Endpunkt. Zum Beispiel:

-   JS-Datei aus dem Pfad/Script
-   Jede Inhaltsdatei aus den/Content Pfad
-   Jeder Controller-Aktion
-   Wenn die Abfragezeichenfolge an die CDN-Endpunkt URL Abfragezeichenfolgen aktiviert ist

Hosten Sie tatsächlich mit der obigen Konfiguration den gesamten Cloud-Dienst aus * *http://*&lt;CdnName >*.azureedge.net/**. Wenn ich zu navigieren **http://camservice.azureedge.net/ ** man das Aktionsergebnis Home-Index.

![](media/cdn-cloud-service-with-cdn/cdn-2-home-page.PNG)

Dies bedeutet, jedoch nicht, dass es immer ratsam oder im Allgemeinen empfiehlt sich einen gesamte Cloud-Dienst über Azure CDN dienen. Einige Probleme sind:

-   Dieser Ansatz erfordert eine gesamte Website öffentlich sein, da Azure CDN private Inhalte zu diesem Zeitpunkt dienen kann.
-   Wenn CDN-Endpunkt offline geht, ob Wartungsarbeiten oder Benutzerfehler, Ihre gesamten Cloud-Dienst offline geschaltet wird, wenn die Kunden die Ursprungs-URL umgeleitet werden können * *http://*&lt;Dienstname >*.cloudapp.net/**.
-   Auch bei der Cache-Control-Standardeinstellungen (siehe [Konfigurieren von Zwischenspeicherungsoptionen für statische Dateien in Ihrem Cloud-Dienst](#caching)), CDN-Endpunkt verbessern die Leistung von hoch dynamischen Inhalt nicht. Wenn Sie die Homepage von CDN-Endpunkt wie oben gezeigt, laden, die mindestens 5 Sekunden dauerte die Standardstartseite beim ersten Laden eine relativ einfache Seite. Angenommen Sie, der Clientumgebung geschehen würde dieser Seite dynamischen Inhalte enthält, der jede Minute aktualisiert werden muss. Dynamische Inhalte von einem CDN-Endpunkt erforderlich kurz Cacheablaufzeit, was häufig Cachefehler CDN-Endpunkt. Dies beeinträchtigt die Leistung des Cloud-Diensts und dem Zweck der CDN.

Die Alternative ist der Inhalt von Azure CDN von Fall zu Fall im Cloud-Dienst zugestellt. Zu diesem Zweck haben Sie schon auf einzelnen Content-Dateien aus dem CDN-Endpunkt gesehen. Ich werde Ihnen zeigen, wie man eine bestimmten Controller Aktion über die CDN-Endpunkt [Inhalte in Controlleraktionen durch Azure CDN](#controller)dienen.

<a name="caching"></a>
## <a name="configure-caching-options-for-static-files-in-your-cloud-service"></a>So konfigurieren Sie Zwischenspeicherungsoptionen für statische Dateien in Ihrem Cloud-Dienst ##

Azure CDN-Integration in die Cloud-Dienst können Sie angeben, wie statische Inhalte CDN-Endpunkt zwischengespeichert werden soll. Hierzu öffnen Sie *Web.config* aus dem Webprojekt Rolle (z.B. WebRole1) und fügen eine `<staticContent>` Element `<system.webServer>`. Die unten stehenden konfiguriert den Cache in 3 Tagen ablaufen.  

    <system.webServer>
      <staticContent>
        <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00"/>
      </staticContent>
      ...
    </system.webServer>

Wenn Sie dies tun, werden alle Dateien in der Cloud-Dienst dieselbe Regel im Cache CDN feststellen. Fügen Sie für eine präzisere Steuerung des Cache Settings *Web.config* -Datei in einem Ordner hinzu und die Einstellung. Z. B. *\Content* Ordner eine *Web.config* -Datei hinzufügen und den Inhalt durch folgenden XML-Code ersetzen:

    <?xml version="1.0"?>
    <configuration>
      <system.webServer>
        <staticContent>
          <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="15.00:00:00"/>
        </staticContent>
      </system.webServer>
    </configuration>

Diese Einstellung bewirkt, dass alle Dateien aus dem Ordner *\Content* 15 Tage lang zwischengespeichert werden.

Weitere Informationen zum Konfigurieren der `<clientCache>` Element finden Sie unter [Clientcache &lt;ClientCache >](http://www.iis.net/configreference/system.webserver/staticcontent/clientcache).

[Inhalt von Controller-Aktionen über Azure CDN bedienen](#controller)auch zeige ich Ihnen wie Cache RasterX Controller Aktionsergebnisse in CDN-Cache konfigurieren können.

<a name="controller"></a>
## <a name="serve-content-from-controller-actions-through-azure-cdn"></a>Inhalt von Controller-Aktionen über Azure CDN bedienen ##

Bei Integration eine Cloud Service Web-Rolle in Azure CDN ist relativ einfach, Inhalte in Controlleraktionen durch Azure CDN. Als mit dem Cloud-Dienst direkt über Azure CDN (oben gezeigt), zeigt [Maarten Balliauw](https://twitter.com/maartenballiauw) wie mit MemeGenerator-Controller [Degressiv](http://channel9.msdn.com/events/TechDays/Techdays-2014-the-Netherlands/Reducing-latency-on-the-web-with-the-Windows-Azure-CDN)Latenz im Web mit Azure CDN. Ich werde Sie einfach hier reproduzieren.

Angenommen Sie, in Ihrem Clouddienst meme basierend auf einem jungen Chuck Norris Bild (Foto [Alan Licht](http://www.flickr.com/photos/alan-light/218493788/)) generiert werden soll:

![](media/cdn-cloud-service-with-cdn/cdn-5-memegenerator.PNG)

Sie haben eine einfache `Index` Aktion, die die Kunden an Superlative im Bild generiert das MEM, sobald sie auf die Aktion veröffentlichen. Ist Chuck Norris erwarten auf dieser Seite zu beliebten Global. Dies ist ein gutes Beispiel für Azure CDN-dynamische Inhalte mit.

Die obigen Schritte dadurch Controller einrichten:

1. Im Ordner " *\Controllers* " eine neue CS-Datei namens *MemeGeneratorController.cs* erstellen und den Inhalt durch folgenden Code ersetzen. Achten Sie darauf, dass der markierte Bereich mit dem Namen Ihres CDN ersetzen.  

        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Drawing;
        using System.IO;
        using System.Net;
        using System.Web.Hosting;
        using System.Web.Mvc;
        using System.Web.UI;

        namespace WebRole1.Controllers
        {
            public class MemeGeneratorController : Controller
            {
                static readonly Dictionary<string, Tuple<string ,string>> Memes = new Dictionary<string, Tuple<string, string>>();

                public ActionResult Index()
                {
                    return View();
                }

                [HttpPost, ActionName("Index")]
                public ActionResult Index_Post(string top, string bottom)
                {
                    var identifier = Guid.NewGuid().ToString();
                    if (!Memes.ContainsKey(identifier))
                    {
                        Memes.Add(identifier, new Tuple<string, string>(top, bottom));
                    }

                    return Content("<a href=\"" + Url.Action("Show", new {id = identifier}) + "\">here's your meme</a>");
                }

                [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
                public ActionResult Show(string id)
                {
                    Tuple<string, string> data = null;
                    if (!Memes.TryGetValue(id, out data))
                    {
                        return new HttpStatusCodeResult(HttpStatusCode.NotFound);
                    }

                    if (Debugger.IsAttached) // Preserve the debug experience
                    {
                        return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                    else // Get content from Azure CDN
                    {
                        return Redirect(string.Format("http://<yourCdnName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                }

                [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]
                public ActionResult Generate(string top, string bottom)
                {
                    string imageFilePath = HostingEnvironment.MapPath("~/Content/chuck.bmp");
                    Bitmap bitmap = (Bitmap)Image.FromFile(imageFilePath);

                    using (Graphics graphics = Graphics.FromImage(bitmap))
                    {
                        SizeF size = new SizeF();
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, top.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(top.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), 10f));
                        }
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, bottom.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(bottom.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), bitmap.Height - 10f - arialFont.Height));
                        }
                    }

                    MemoryStream ms = new MemoryStream();
                    bitmap.Save(ms, System.Drawing.Imaging.ImageFormat.Png);
                    return File(ms.ToArray(), "image/png");
                }

                private Font FindBestFitFont(Image i, Graphics g, String text, Font font, out SizeF size)
                {
                    // Compute actual size, shrink if needed
                    while (true)
                    {
                        size = g.MeasureString(text, font);

                        // It fits, back out
                        if (size.Height < i.Height &&
                             size.Width < i.Width) { return font; }

                        // Try a smaller font (90% of old size)
                        Font oldFont = font;
                        font = new Font(font.Name, (float)(font.Size * .9), font.Style);
                        oldFont.Dispose();
                    }
                }
            }
        }

2. Mit der rechten Maustaste in der standardmäßigen `Index()` Aktion und wählen Sie **Ansicht hinzufügen**.

    ![](media/cdn-cloud-service-with-cdn/cdn-6-addview.PNG)

3.  Übernehmen Sie die Einstellung, und klicken Sie auf **Hinzufügen**.

    ![](media/cdn-cloud-service-with-cdn/cdn-7-configureview.PNG)

4. Öffnen Sie die neue *Views\MemeGenerator\Index.cshtml* und Ersetzen Sie Inhalt durch die folgende einfache HTML Dank Superlative:

        <h2>Meme Generator</h2>

        <form action="" method="post">
            <input type="text" name="top" placeholder="Enter top text here" />
            <br />
            <input type="text" name="bottom" placeholder="Enter bottom text here" />
            <br />
            <input class="btn" type="submit" value="Generate meme" />
        </form>

5. Cloud-Dienst erneut veröffentlichen und navigieren Sie zu * *http://*&lt;Dienstname >*.cloudapp.net/MemeGenerator/Index** in Ihrem Browser.

Die Formularwerte eingestellten `/MemeGenerator/Index`, `Index_Post` Aktionsmethode gibt eine Verknüpfung mit der `Show` Aktionsmethode mit der entsprechenden Eingabe. Wenn Sie auf den Link klicken, erreichen Sie den folgenden Code:  

    [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
    public ActionResult Show(string id)
    {
        Tuple<string, string> data = null;
        if (!Memes.TryGetValue(id, out data))
        {
            return new HttpStatusCodeResult(HttpStatusCode.NotFound);
        }

        if (Debugger.IsAttached) // Preserve the debug experience
        {
            return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
        else // Get content from Azure CDN
        {
            return Redirect(string.Format("http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
    }

Wenn Ihre lokalen Debuggers erhalten regulären Debug Erfahrung mit lokalen Umleitung Sie. Wenn im Cloud-Dienst ausgeführt wird, wird eine Umleitung zu:

    http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>

Die folgenden Ursprungs-URL der CDN-Endpunkt entspricht:

    http://<youCloudServiceName>.cloudapp.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>


Können Sie die `OutputCacheAttribute` -Attribut auf die `Generate` -Methode können Sie angeben, wie das Aktionsergebnis zwischengespeichert werden sollte die Azure CDN berücksichtigt. Der folgende Code eine Ablaufzeit für den Cache von 1 Stunde (3.600 Sekunden) angeben

    [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]

Ebenso können Sie Inhalte aus allen Controller-Aktionen in der Cloud-Dienst über Ihre Azure CDN mit Zwischenspeichern gewünschte dienen.

Im nächsten Abschnitt werden Sie wie die gebündelt und minimierte Skripts und CSS über Azure CDN vorgestellt.

<a name="bundling"></a>
## <a name="integrate-aspnet-bundling-and-minification-with-azure-cdn"></a>Azure CDN ASP.NET bündeln und Verkleinerung integriert ##

Skripts und CSS-Stylesheets selten ändern und sind für den Azure CDN-Cache. Die gesamte Webrolle durch Azure CDN serviert am einfachsten bündeln und Verkleinerung in Azure CDN integriert. Aber wie Sie dies tun möchten, zeige ich Ihnen wie während die gewünschten Develper Erfahrung ASP.NET bündeln und Verkleinerung, wie beibehalten:

-   Große Debug Mode Erfahrung
-   Optimierte Bereitstellung
-   Sofortige Updates für Clients für Skript-CSS Versions-upgrades
-   -Fallbackmechanismus fällt CDN-Endpunkt
-   Code-Änderung minimieren

[Integrieren einer Azure CDN-Endpunkt mit der Azure-Website und dienen statische Inhalte in Webseiten von Azure CDN](#deploy)gegründet **WebRole1** Projekt öffnen *app_start\bundleconfig* und sehen Sie sich die `bundles.Add()` Methodenaufrufe.

    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                    "~/Scripts/jquery-{version}.js"));
        ...
    }

Die erste `bundles.Add()` Anweisung fügt ein Skript-Bundle im virtuellen Verzeichnis `~/bundles/jquery`. Öffnen Sie dann *Views\Shared ebenfalls einen\_Layout.cshtml* sehen wie Bundle Skripttags gerendert wird. Sie sollten die folgenden Razor-Code finden können:

    @Scripts.Render("~/bundles/jquery")

Wenn diese Razor-Code in der Azure-Webrolle ausgeführt wird, wird gerendert, ein `<script>` für Skript-Bundle ähnlich dem folgenden Tag:

    <script src="/bundles/jquery?v=FVs3ACwOLIVInrAl5sdzR2jrCDmVOWFbZMY6g6Q0ulE1"></script>

Jedoch beim Ausführen im Visual Studio durch Eingabe `F5`, jede Skriptdatei im Bündel wird einzeln gerendert (im obigen Beispiel nur eine Skriptdatei ist im Bündel):

    <script src="/Scripts/jquery-1.10.2.js"></script>

Dadurch können Sie JavaScript-Code in der Umgebung debuggen, um gleichzeitige Clientverbindungen (Bündelung) und die Datei verbessern Leistung (Verkleinerung) in der Produktion herunterladen. Es ist ein großartiges Feature zu Azure CDN-Integration. Außerdem da gerenderte Paket bereits eine automatisch generierte Zeichenfolge enthält soll die Funktionalität repliziert das Aktualisieren die jQuery-Version über NuGet kann aktualisiert werden auf dem Client so bald wie möglich.

Gehen Sie zu Integration ASP.NET Bündelung Verkleinerung mit CDN-Endpunkt.

1. Ändern Sie im *app_start\bundleconfig*die `bundles.Add()` Methoden einen anderen [Konstruktor Bündel](http://msdn.microsoft.com/library/jj646464.aspx), eine, die eine CDN-Adresse angibt. Ersetzen Sie dazu die `RegisterBundles` Methodendefinition durch den folgenden Code:  

        public static void RegisterBundles(BundleCollection bundles)
        {
            bundles.UseCdn = true;
            var version = System.Reflection.Assembly.GetAssembly(typeof(Controllers.HomeController))
                .GetName().Version.ToString();
            var cdnUrl = "http://<yourCDNName>.azureedge.net/{0}?v=" + version;

            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery")).Include(
                        "~/Scripts/jquery-{version}.js"));

            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval")).Include(
                        "~/Scripts/jquery.validate*"));

            // Use the development version of Modernizr to develop with and learn from. Then, when you're
            // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer")).Include(
                        "~/Scripts/modernizr-*"));

            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap")).Include(
                        "~/Scripts/bootstrap.js",
                        "~/Scripts/respond.js"));

            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }

    Ersetzen Sie `<yourCDNName>` mit dem Namen Azure CDN.

    In einfachen Worten einrichten `bundles.UseCdn = true` und jedes sorgfältig CDN-URL hinzugefügt. Angenommen, der erste Konstruktor im Code:

        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))

    entspricht:

        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "http://<yourCDNName>.azureedge.net/bundles/jquery?v=<W.X.Y.Z>"))

    Dieser Konstruktor weist ASP.NET bündeln und Verkleinerung rendern einzelne Skriptdateien beim lokalen Debuggen, aber die angegebene CDN-Adresse auf das betreffende Skript. Jedoch zwei wichtige Merkmale dieses sorgfältig CDN-URL:

    -   Der Ursprung dieser CDN-URL ist `http://<yourCloudService>.cloudapp.net/bundles/jquery?v=<W.X.Y.Z>`, ist eigentlich das virtuelle Verzeichnis der Skript-Bundle im Cloud-Dienst.
    -   Seit CDN-Konstruktor enthält CDN Skripttag für das Paket nicht mehr automatisch generierte Versionszeichenfolge im gerenderten URL. Jedem Skript-Bundle erzwingen Cachefehler Ihre Azure CDN geändert wird, müssen Sie manuell eine eindeutige Zeichenfolge generieren. Zur gleichen Zeit muss diese eindeutige Zeichenfolge konstant bleiben über die Lebensdauer der Bereitstellung Cachetreffer Ihre Azure CDN maximieren, nachdem das Paket bereitgestellt wird.
    -   Die Abfragezeichenfolge V = < W.X.Y.Z > zieht aus *Properties\AssemblyInfo* im Webprojekt Rolle. Sie können Bereitstellungsworkflow verfügen, der erhöhen die Version der Assembly bei jeder Veröffentlichung in Azure einschließt. Oder Sie können nur *Properties\AssemblyInfo* im Projekt automatisch erhöht die Versionszeichenfolge bei jedem Erstellen mit dem Platzhalterzeichen "*". Zum Beispiel:

            [assembly: AssemblyVersion("1.0.0.*")]

        Jede Strategie zur Rationalisierung generiert eine eindeutige Zeichenfolge für die Lebensdauer einer Bereitstellung funktioniert hier.

3. Cloud-Dienst veröffentlichen und die Startseite zugreifen.

4. HTML-Code der Seite anzuzeigen. Sie sollen die CDN URL mit eine eindeutige Zeichenfolge jedes Mal ändern zu Ihrem Clouddienst veröffentlichen gerendert. Zum Beispiel:  

        ...

        <link href="http://camservice.azureedge.net/Content/css?v=1.0.0.25449" rel="stylesheet"/>

        <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25449"></script>

        ...

        <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25449"></script>

        <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25449"></script>

        ...

5. In Visual Studio debuggen Cloud-Dienst in Visual Studio durch Eingabe `F5`.,

6. HTML-Code der Seite anzuzeigen. Sie sehen noch jede Datei einzeln gerendert, so dass Sie eine konsistente Debuggen in Visual Studio auftreten können.  

        ...

            <link href="/Content/bootstrap.css" rel="stylesheet"/>
        <link href="/Content/site.css" rel="stylesheet"/>

            <script src="/Scripts/modernizr-2.6.2.js"></script>

        ...

            <script src="/Scripts/jquery-1.10.2.js"></script>

            <script src="/Scripts/bootstrap.js"></script>
        <script src="/Scripts/respond.js"></script>

        ...   

<a name="fallback"></a>
## <a name="fallback-mechanism-for-cdn-urls"></a>Fallbackmechanismus für CDN-URLs ##

Wenn Azure CDN-Endpunkt aus irgendeinem Grund fehlschlägt, können Sie Ihre Webseite intelligent genug auf Ihre ursprünglichen Webserver als Fallbackoption für JavaScript oder Bootstrap geladen. Es ist schwerwiegend genug, um Bilder auf Ihrer Website durch CDN fehlen jedoch wichtige Seitenfunktionalität von Skripts und Stylesheets nicht mehr viel schwerer zu verlieren.

Die [Bundle](http://msdn.microsoft.com/library/system.web.optimization.bundle.aspx) -Klasse enthält eine Eigenschaft namens [CdnFallbackExpression](http://msdn.microsoft.com/library/system.web.optimization.bundle.cdnfallbackexpression.aspx) , die Sie der Fallbackmechanismus für CDN Fehler konfigurieren können. Um diese Eigenschaft zu verwenden, wie folgt:

1. Öffnen Sie in Ihrem Webprojekt Rolle *app_start\bundleconfig*, wobei eine CDN-URL in jedem [Paket Konstruktor](http://msdn.microsoft.com/library/jj646464.aspx)hinzugefügt, und ändern Sie den hervorgehobenen Standard Bündel Fallbackmechanismus hinzu:  

        public static void RegisterBundles(BundleCollection bundles)
        {
            var version = System.Reflection.Assembly.GetAssembly(typeof(BundleConfig))
                .GetName().Version.ToString();
            var cdnUrl = "http://cdnurl.azureedge.net/.../{0}?" + version;
            bundles.UseCdn = true;

            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))
                        { CdnFallbackExpression = "window.jquery" }
                        .Include("~/Scripts/jquery-{version}.js"));

            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval"))
                        { CdnFallbackExpression = "$.validator" }
                        .Include("~/Scripts/jquery.validate*"));

            // Use the development version of Modernizr to develop with and learn from. Then, when you&#39;re
            // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer"))
                        { CdnFallbackExpression = "window.Modernizr" }
                        .Include("~/Scripts/modernizr-*"));

            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap"))     
                        { CdnFallbackExpression = "$.fn.modal" }
                        .Include(
                                "~/Scripts/bootstrap.js",
                                "~/Scripts/respond.js"));

            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }

    Wenn `CdnFallbackExpression` ist nicht null Skript in HTML zu testen, ob das Paket erfolgreich geladen und, wenn nicht das Paket direkt aus dem ursprünglichen Webserver eingefügt. Diese Eigenschaft muss auf einen JavaScript-Ausdruck, der überprüft, ob das jeweiligen CDN-Paket ordnungsgemäß geladen wird. Der Ausdruck jedes testen musste hängt der Inhalt. Die Standard-Bundles oben:

    -   `window.jquery`wird in JS Jquery-{Version} definiert
    -   `$.validator`wird in jquery.validate.js definiert
    -   `window.Modernizr`wird in modernisierer-{Version} js definiert
    -   `$.fn.modal`wird in bootstrap.js definiert

    Ihnen ist vielleicht aufgefallen, dass ich nicht für CdnFallbackExpression das `~/Cointent/css` Paket. Ist derzeit gibt es einen [Fehler in System.Web.Optimization](https://aspnetoptimization.codeplex.com/workitem/104) , der Fügt eine `<script>` Tag für alternativen CSS statt der erwarteten `<link>` Tag.

    Es ist jedoch ein guter [Stil Bundles Fallback](https://github.com/EmberConsultingGroup/StyleBundleFallback) [Ember Consulting](https://github.com/EmberConsultingGroup)-Gruppe.

2. Um den Workaround für CSS verwenden, eine neue CS-Datei im Web Rolle des Projekts *App_Start* Ordner namens *StyleBundleExtensions.cs*erstellen und seinen Inhalt mit dem [Code von GitHub](https://github.com/EmberConsultingGroup/StyleBundleFallback/blob/master/Website/App_Start/StyleBundleExtensions.cs)ersetzen.

4. Benennen Sie in *App_Start\StyleFundleExtensions.cs*den Namespace in der Webrolle Namen (z.B. **WebRole1**).

3. Zurück zu `App_Start\BundleConfig.cs` und ändern den letzten `bundles.Add` mit den folgenden hervorgehobenen Code:  

        bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css"))
            <mark>.IncludeFallback("~/Content/css", "sr-only", "width", "1px")</mark>
            .Include(
                  "~/Content/bootstrap.css",
                  "~/Content/site.css"));

    Diese neue Erweiterungsmethode verwendet dasselbe Skript im HTML-DOM für überprüfen Einfügen der einen entsprechenden Klassennamen Regelname und Wert der CSS-Paket und auf dem ursprünglichen Webserver definiert, wenn die Übereinstimmung nicht gefunden.

4. Cloud-Dienst erneut veröffentlichen und die Startseite zugreifen.

5. HTML-Code der Seite anzuzeigen. Eingefügte Skripts sollte ähnlich der folgenden finden:    

        ...

        <link href="http://az632148.azureedge.net/Content/css?v=1.0.0.25474" rel="stylesheet"/>
        <script>(function() {
                        var loadFallback,
                            len = document.styleSheets.length;
                        for (var i = 0; i < len; i++) {
                            var sheet = document.styleSheets[i];
                            if (sheet.href.indexOf('http://camservice.azureedge.net/Content/css?v=1.0.0.25474') !== -1) {
                                var meta = document.createElement('meta');
                                meta.className = 'sr-only';
                                document.head.appendChild(meta);
                                var value = window.getComputedStyle(meta).getPropertyValue('width');
                                document.head.removeChild(meta);
                                if (value !== '1px') {
                                    document.write('<link href="/Content/css" rel="stylesheet" type="text/css" />');
                                }
                            }
                        }
                        return true;
                    }())||document.write('<script src="/Content/css"><\/script>');</script>

            <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25474"></script>
        <script>(window.Modernizr)||document.write('<script src="/bundles/modernizr"><\/script>');</script>

        ...

            <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25474"></script>
        <script>(window.jquery)||document.write('<script src="/bundles/jquery"><\/script>');</script>

            <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25474"></script>
        <script>($.fn.modal)||document.write('<script src="/bundles/bootstrap"><\/script>');</script>

        ...


    Eingefügtes Skript für CSS-Bundle noch fehlerhafte Überbleibsel enthält die `CdnFallbackExpression` -Eigenschaft in der Zeile:

        }())||document.write('<script src="/Content/css"><\/script>');</script>

    Seit dem ersten Teil der || Ausdruck immer (in der Zeile direkt oberhalb) true zurückgeben, document.write() Funktion nie ausgeführt.

## <a name="more-information"></a>Weitere Informationen ##
- [Übersicht über Azure Content Delivery Network (CDN)](http://msdn.microsoft.com/library/azure/ff919703.aspx)
- [Mithilfe von Azure CDN](cdn-create-new-endpoint.md)
- [ASP.NET bündeln und Verkleinerung](http://www.asp.net/mvc/tutorials/mvc-4/bundling-and-minification)



[new-cdn-profile]: ./media/cdn-cloud-service-with-cdn/cdn-new-profile.png
[cdn-profile-settings]: ./media/cdn-cloud-service-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-cloud-service-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-cloud-service-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-cloud-service-with-cdn/cdn-endpoint-success.png
