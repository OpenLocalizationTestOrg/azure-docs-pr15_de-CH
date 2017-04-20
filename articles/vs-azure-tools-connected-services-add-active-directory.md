<properties 
   pageTitle="Hinzufügen von Azure Active Directory verbundenen Dienste in Visual Studio mit | Microsoft Azure"
   description="Fügen Sie Azure Active Directory hinzu, indem Sie über das Dialogfeld Visual Studio verbunden Dienste hinzufügen"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="active-directory"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="adding-an-azure-active-directory-by-using-connected-services-in-visual-studio"></a>Hinzufügen von Azure Active Directory mit verbundenen Dienste in Visual Studio 

##<a name="overview"></a>Übersicht
Mithilfe von Azure Active Directory (Azure AD) können Sie einmaliges Anmelden (SSO) für ASP.NET MVC Web Applications oder AD-Authentifizierung im Web API-Dienste unterstützen. Mit Azure AD-Authentifizierung können die Benutzer ihre Konten von Azure AD mit einer Anwendung verbinden. Azure AD-Authentifizierung mit Web API Vorteile verbesserte Sicherheit, eine API über eine Web-Anwendung verfügbar zu machen. Azure AD müssen Sie keinen eigenen Authentifizierungssystem mit eigenen Konto und Benutzer verwalten.

## <a name="supported-project-types"></a>Unterstützte Projekttypen

Das Dialogfeld Verbindung können Sie Azure AD in die folgenden Projekttypen herstellen.

- ASP.NET MVC-Projekte

- ASP.NET Web API-Projekte


### <a name="connect-to-azure-ad-using-the-connected-services-dialog"></a>Verbinden Sie mit Azure AD mit dem Dialogfeld verbunden

1. Stellen Sie sicher, dass Sie ein Azure-Konto verfügen. Wenn Sie ein Azure-Konto haben, können Sie eine [kostenlose Testversion](http://go.microsoft.com/fwlink/?LinkId=518146)anmelden.

1. In Visual Studio öffnen im Kontextmenü des Knotens **Verweise** im Projekt, und wählen Sie **Verbundene Dienste hinzufügen**.
1. Wählen Sie **Azure AD-Authentifizierung aus** , und wählen Sie dann **Konfigurieren**.

    ![Wählen Sie Azure AD-Authentifizierung hinzufügen](./media/vs-azure-tools-connected-services-add-active-directory/connected-services-add-active-directory.png)

1. Überprüfen Sie auf der ersten Seite der **Azure konfigurieren Sie AD-Authentifizierung** **Konfigurieren Sie einmaliges Anmelden mit Azure AD**.

    Wenn das Projekt mit einer anderen Konfiguration konfiguriert ist, warnt Sie der Assistent weiter die vorherige Konfiguration deaktivieren.

    ![Konfigurieren von Azure AD im Assistenten](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-1.png)

1.  Wählen Sie auf der zweiten Seite eine Domäne aus der Dropdownliste **Domäne** . Die Liste der Domänen enthält alle verfügbaren Domänen im Dialogfeld Konto aufgelistet. Alternativ können Sie einen Domänennamen eingeben, wenn Sie das nicht finden Sie, wie mydomain.onmicrosoft.com suchen. Wählen Sie die Option zum Erstellen eines neuen Azure AD app oder verwenden Sie die Einstellung von einer vorhandenen Azure AD app. 

    ![Konfigurieren von Azure AD im Assistenten](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-2.png)


1. Sicherstellen Sie auf der dritten Seite des Assistenten **Verzeichnisdaten lesen** aktiviert ist. Der Assistent füllt den **geheimen**. 

    ![Konfigurieren von Azure AD im Assistenten](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-3.png)

1. Wählen Sie die Schaltfläche **Fertig stellen** . Das Dialogfeld fügt die erforderlichen Codes und Verweise auf das Projekt für Azure AD-Authentifizierung aktivieren. Die AD-Domäne finden auf der [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Überprüfen der Einstieg Seite in Ihrem Browser Ideen für nächste Schritte und wo Seite zu sehen, wie das Projekt geändert wurde. Möchten Sie überprüfen, dass alles funktioniert, Öffnen eines geänderten Konfigurationsdateien und überprüfen werden gemäß, wo es. Beispielsweise haben die wichtigsten web.config in einer ASP.NET MVC-Projekt diese Einstellungen hinzugefügt:

        <appSettings> 
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:AADInstance" value="https://login.windows.net/" />
            <add key="ida:Domain" value="Your selected domain" />
            <add key="ida:TenantId" value="The Id of your selected Azure AD Tenant" />
            <add key="ida:PostLogoutRedirectUri" value="The default redirect URI from the project" />
        </appSettings>

## <a name="how-your-project-is-modified"></a>Wie Projekt geändert

Beim Ausführen des Assistenten Visual Studio fügt Azure AD und Verweise auf das Projekt verknüpft ist. Konfigurationsdateien und Codedateien im Projekt werden um Azure AD unterstützen auch geändert. Bestimmten Änderungen, die Visual Studio macht hängen Projekt. Ausführliche Informationen dazu, wie ASP.NET MVC-Projekte geändert werden finden Sie unter [Was passiert – MVC-Projekte](http://go.microsoft.com/fwlink/p/?LinkID=513809). Web API-Projekten finden Sie unter [wo-API Webprojekte](http://go.microsoft.com/fwlink/p/?LinkId=513810).

##<a name="next-steps"></a>Nächste Schritte

Fragen stellen und Hilfe.

 - [MSDN-Forum: Azure AD](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD)

 - [Azure AD-Dokumentation](https://azure.microsoft.com/documentation/services/active-directory/)

 - [Blogbeitrag: Einführung in Azure AD](http://blogs.msdn.com/b/brunoterkaly/archive/2014/03/03/introduction-to-windows-azure-active-directory.aspx)

