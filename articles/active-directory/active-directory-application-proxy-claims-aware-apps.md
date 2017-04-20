<properties
    pageTitle="Arbeiten mit bewusst Apps Anwendungsproxy"
    description="Erläutert, wie mit Azure AD-Anwendungsproxy zu bringen."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>



# <a name="working-with-claims-aware-apps-in-application-proxy"></a>Arbeiten mit bewusst apps Anwendungsproxy

Ansprüche bewusst apps führen eine Umleitung auf die Sicherheit Sicherheitstokendienst (STS), die wiederum Anmeldeinformationen des Benutzers für ein Token anfordert, vor der Weiterleitung an die Anwendung. Um Anwendungsproxy diese Umleitung zu aktivieren, müssen die folgenden Schritte ausgeführt werden.

## <a name="prerequisites"></a>Erforderliche Komponenten
Vor dem Ausführen dieses Verfahrens sicherstellen Sie, dass der STS Ansprüche bewusst app leitet steht außerhalb Ihres lokalen Netzwerks.

## <a name="azure-classic-portal-configuration"></a>Azure klassische Portalkonfiguration

1. Veröffentlichen Sie die Anwendung nach der Anleitung in [Clientanwendungen mit Anwendungsproxy veröffentlichen](active-directory-application-proxy-publish.md).
2. Wählen Sie in der Liste der Programme Ansprüche bewusst app und klicken Sie auf **Konfigurieren**.
3. Bei Verwendung der **Vorauthentifizierung Methode** **Passthrough** müssen Sie **HTTPS** als **Externen URL** -Schema auswählen.
4. Wählen Sie **Azure Active Directory** als **Vorauthentifizierung Methode**, **None** als **Interne Authentifizierungsmethode**.


## <a name="adfs-configuration"></a>ADFS-Konfiguration

1. ADFS-Verwaltung zu öffnen.
2. Zum **Verlassen vertrauen**, klicken Sie mit der rechten Maustaste auf die app Veröffentlichen mit Anwendungsproxy, und wählen Sie **Eigenschaften**.  
  ![Anw.-Name - Screentshot Rechtsklick Relying Party vertraut](./media/active-directory-application-proxy-claims-aware-apps/appproxyrelyingpartytrust.png)  
3. Wählen Sie auf der Registerkarte **Endpunkte** unter **Endpunkttyp** **WS-Federation**.
4. Geben Sie unter **Vertrauenswürdige URL** Anwendungsproxy unter **Externe URL** eingegebene URL, und klicken Sie auf **OK**.  
  ![Fügen Sie einen Endpunkt - Wert vertrauenswürdige URL - Screenshot](./media/active-directory-application-proxy-claims-aware-apps/appproxyendpointtrustedurl.png)  

## <a name="see-also"></a>Siehe auch

- [Veröffentlichen Sie mit Anwendungsproxy](active-directory-application-proxy-publish.md)
- [Einmalige Anmeldung aktivieren](active-directory-application-proxy-sso-using-kcd.md)
- [Problembehandlung, mit Anwendungsproxy](active-directory-application-proxy-troubleshoot.md)
- [Aktivieren Sie systemeigene Clientanwendungen interagieren mit proxy](active-directory-application-proxy-native-client.md)

Überprüfen Sie die aktuellsten Neuigkeiten und Updates [Anwendungsproxy blog](http://blogs.technet.com/b/applicationproxyblog/)
