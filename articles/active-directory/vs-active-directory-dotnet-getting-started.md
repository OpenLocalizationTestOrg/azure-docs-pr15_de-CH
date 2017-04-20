<properties 
    pageTitle="Erste Schritte mit Azure Active Directory und Visual Studio verbunden Services (MVC-Projekte) | Microsoft Azure" 
    description="Erste Schritte mit Azure Active Directory in MVC-Projekte nach dem Herstellen einer Verbindung mit einer Azure AD mit Visual Studio erstellen Services verbunden" 
    services="active-directory" 
    documentationCenter="" 
    authors="TomArcher" 
    manager="douge" 
    editor=""/>
  
<tags 
    ms.service="active-directory" 
    ms.workload="web" 
    ms.tgt_pltfrm="vs-getting-started" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="tarcher"/>

# <a name="getting-started-with-azure-active-directory-and-visual-studio-connected-services-mvc-projects"></a>Erste Schritte mit Azure Active Directory und Visual Studio verbunden Services (MVC-Projekte)

> [AZURE.SELECTOR]
> - [Erste Schritte](vs-active-directory-dotnet-getting-started.md)
> - [Was ist passiert](vs-active-directory-dotnet-what-happened.md)
 
##<a name="requiring-authentication-to-access-controllers"></a>Authentifikation Access Controller 

Alle Domänencontroller in Ihrem Projekt wurden Attributs **Autorisieren** zieren. Dieses Attribut erfordert den Benutzer vor dem Zugriff auf diese Domänencontroller authentifiziert werden. Damit Domänencontroller anonym zugegriffen werden kann, wird entfernen Sie dieses Attribut vom Controller. Wenden Sie Berechtigungen auf einer tieferen Ebene festgelegt werden soll, das Attribut für jede Methode, die anstatt der Controller-Klasse erfordert.
 
##<a name="adding-signin--signout-controls"></a>Hinzufügen von Anmeldung / Abmeldung gesteuert 

Hinzufügen einer Anmeldung/Abmeldung Steuerelemente zur Ansicht können Sie die Teilansicht **_LoginPartial.cshtml** die Funktionalität eines Ansichten hinzufügen. Hier ist ein Beispiel der Funktionen auf standard **_Layout.cshtml** . (Beachten Sie das letzte Element Div mit Klasse Navigationsleiste minimieren):

<pre>
    &lt;! DOCTYPE html&gt; 
&lt;html&gt; 
&lt;head&gt; 
&lt;meta charset="utf-8" /&gt; 
&lt;meta name="viewport" content="width=device-width, initial-scale=1.0"&gt; 
&lt;title&gt;@ViewBag.Title - My ASP.NET Application&lt;/title&gt; 
@Styles.Render("~/Content/css") @Scripts.Render("~/bundles/modernizr") &lt;/head&gt; 
&lt;body&gt; 
&lt;div class="navbar navbar-inverse navbar-fixed-top"&gt; 
&lt;div class="container"&gt; 
&lt;div class="navbar-header"&gt; 
&lt;button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse"&gt; 
&lt;span class="icon-bar"&gt;&lt;/span&gt; 
&lt;span class="icon-bar"&gt;&lt;/span&gt; 
&lt;span class="icon-bar"&gt;&lt;/span&gt; 
&lt;/button&gt; 
@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" }) &lt;/div&gt; 
&lt;div class="navbar-collapse collapse"&gt; 
&lt;ul class="nav navbar-nav"&gt; 
&lt;li&gt;@Html.ActionLink("Home", "Index", "Home")&lt;/li&gt; 
&lt;li&gt;@Html.ActionLink("About", "About", "Home")&lt;/li&gt; 
&lt;li&gt;@Html.ActionLink("Contact", "Contact", "Home")&lt;/li&gt; 
&lt;/ul&gt; 
                    <span style="background-color:yellow">@Html.Partial("_LoginPartial")</span> 
                &lt;/ div&gt; 
&lt;/div&gt; 
&lt;/div&gt; 
&lt;div class="container body-content"&gt; 
@RenderBody() &lt;hr /&gt; 
&lt;footer&gt; 
&lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET Application&lt;/p&gt; 
&lt;/footer&gt; 
&lt;/div&gt; 
@Scripts.Render("~/bundles/jquery") @Scripts.Render("~/bundles/bootstrap") @RenderSection("scripts", required: false) &lt;/body&gt; 
&lt;/html                                                                                                                                                                                                                                                                                                                                                                                                                                                           &gt;
</pre>

[Weitere Informationen zu Active Directory Azure](https://azure.microsoft.com/services/active-directory/) 
