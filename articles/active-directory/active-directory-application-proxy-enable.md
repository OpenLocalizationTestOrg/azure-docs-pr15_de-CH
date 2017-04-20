<properties
    pageTitle="Aktivieren von Azure AD-Anwendungsproxy | Microsoft Azure"
    description="Anwendungsproxy im klassischen Azure-Portal aktivieren Sie, und installieren Sie die Anschlüsse für den reverse-Proxyserver."
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
    ms.topic="get-started-article"
    ms.date="07/19/2016"
    ms.author="kgremban"/>

# <a name="enable-application-proxy-in-the-azure-portal"></a>Anwendungsproxy in Azure-Portal aktivieren

Dieser Artikel führt Sie durch die Schritte zum Aktivieren von Microsoft Azure AD-Anwendungsproxy für die cloudverzeichnis in Azure AD.

Wenn Sie welche Anwendungsproxy vertraut sind, können Sie tun, erfahren Sie, [wie secure remote Access auf lokalen Anwendung](active-directory-application-proxy-get-started.md).

## <a name="application-proxy-prerequisites"></a>Anwendung Proxy erforderliche Komponenten
Bevor Sie aktivieren und Anwendungsproxy Dienste verwenden können, müssen Sie:

- [Microsoft Azure AD Basis- oder Premium-Abonnement](active-directory-editions.md) und ein Azure AD-Verzeichnis für den globalen Administrator sind.
- Ein Server mit Windows Server 2012 R2 oder Windows 8.1 oder höher, auf dem der Application Connector Proxy installieren können. Der Server sendet Anfragen an den Anwendungsproxy Services in der Cloud und eine HTTP oder HTTPS-Verbindung auf die veröffentlichte Anwendung benötigt.

    - Für einmaliges Anmelden auf der veröffentlichten Anwendung sollten diesem Computer in derselben AD-Domäne als die Anwendung Domäne sein, die Sie veröffentlichen.

- Ist eine Firewall im Pfad sicherzustellen Sie, damit der Connector HTTPS (TCP) Anfragen an den Anwendungsproxy kann geöffnet ist. Der Connector verwendet diese Ports mit Subdomänen, die Teil der allgemeinen Domänen msappproxy.net und servicebus.windows.net. Stellen Sie sicher die folgenden Ports für **ausgehenden** Datenverkehr geöffnet:

  	| Port-Nummer | Beschreibung |
  	| --- | --- |
  	| 80 | Ausgehender HTTP-Verkehr für die Validierung zu aktivieren. |
  	| 443 | Aktivieren der Benutzerauthentifizierung für Azure AD (nur für die Connector-Anmeldung erforderlich) |
  	| 10100 – 10120 | Aktivieren von LOB-HTTP-Antworten an den Proxy gesendet |
  	| 9352, 5671 | Ermöglichen Sie die Kommunikation zwischen den Connector zu Azure Service für eingehende Anfragen. |
  	| 9350 | Optional, ermöglicht eine bessere Leistung für eingehende Anfragen |
  	| 8080 | Aktivieren Sie Connector-bootstrap-Sequenz und automatische Verbindung |
  	| 9090 | Aktivieren Sie Connector-Registrierung (nur für die Connector-Anmeldung erforderlich) |
  	| 9091 | Connector vertrauenswürdigen Zertifikat automatische Verlängerung aktivieren |

    Wenn Ihre Firewall Datenverkehr nach Ursprung Benutzer erzwingt, öffnen Sie diese Ports für den Datenverkehr von Windows-Diensten als Netzwerkdienst ausgeführt. Stellen Sie außerdem Port 8080 für Systemkonto zu aktivieren.

- Wenn Ihre Organisation für die Verbindung zum Internet Proxy-Server verwendet, Bitte betrachten Sie ein Blogbeitrag [Arbeiten mit vorhandenen lokalen Proxyserver](https://blogs.technet.microsoft.com/applicationproxyblog/2016/03/07/working-with-existing-on-prem-proxy-servers-configuration-considerations-for-your-connectors/) Informationen zum Konfigurieren.

## <a name="step-1-enable-application-proxy-in-azure-ad"></a>Schritt 1: Aktivieren der Anwendungsproxy in Azure AD
1. Melden Sie sich als Administrator in [Azure-Verwaltungsportal](https://manage.windowsazure.com/)an.
2. Wechseln Sie zu Active Directory und wählen Sie das Verzeichnis in dem Anwendungsproxy erstellt werden soll.

    ![Active Directory - Symbol](./media/active-directory-application-proxy-enable/ad_icon.png)

3. Wählen Sie auf der Verzeichnisseite **Konfigurieren** und scrollen Sie **Anwendungsproxy**.
4. Umschalten **aktiviert** **Anwendung Proxy-Dienste für dieses Verzeichnis zu aktivieren** .

    ![Anwendungsproxy aktivieren](./media/active-directory-application-proxy-enable/app_proxy_enable.png)

5. Wählen Sie **jetzt downloaden**. Damit gelangen Sie zur **Azure AD Anwendung Proxy Connector Herunterladen**. Lesen Sie den Lizenzvertrag akzeptieren und klicken Sie auf **herunterladen** , um Windows Installer-Datei (.exe) für den Connector zu speichern.

## <a name="step-2-install-and-register-the-connector"></a>Schritt 2: Installieren Sie und registrieren Sie den Connector
1. Führen Sie **AADApplicationProxyConnectorInstaller.exe** auf dem Server nach den erforderlichen Komponenten bereit.
2. Führen Sie die Schritte des Assistenten installieren.
3. Während der Installation wird den Connector mit den Anwendungsproxy Azure AD-Mandanten registriert werden.

  - Anmeldeinformationen Sie Ihre Azure AD globaler Administrator. Der globale Administrator Mieter unterscheiden Ihre Microsoft Azure-Anmeldeinformationen.
  - Sicherstellen der Administrator, der den Connector registriert in demselben Verzeichnis, den Anwendungsproxy-Dienst aktiviert. Beispielsweise ist die Mieter Domäne contoso.com, der Administrator sollte admin@contoso.com oder anderen Alias in dieser Domäne.
  - Wenn die **Verstärkte Sicherheitskonfiguration für IE** soll **auf** auf dem Server, auf dem Sie den Connector installieren, kann das Erfassungsformular blockiert. Führen Sie die Schritte in der Fehlermeldung Zugriff. Stellen Sie sicher, dass verstärkte Sicherheitskonfiguration für Internet Explorer deaktiviert ist.
  - Connector-Registrierung nicht erfolgreich ist, finden Sie unter [Anwendungsproxy zu beheben](active-directory-application-proxy-troubleshoot.md).  

4. Nach Abschluss der Installation den Server zwei neue Dienste hinzugefügt:

    - **Microsoft AAD Application Proxy Connector** ermöglicht Konnektivität
    - **Microsoft AAD Anwendung Proxy Connector Updater** ist eine automatische Updatedienst, der regelmäßig neue Versionen des Connectors überprüft und aktualisiert die Verbindung bei Bedarf.

    ![App Proxy Connectordienste - screenshot](./media/active-directory-application-proxy-enable/app_proxy_services.png)

5. Klicken Sie im Installationsfenster **abgeschlossen** .

Zu hohe Verfügbarkeit sollten Sie mindestens zwei Connectors bereitstellen. Mehrere Connectors bereitstellen möchten, wiederholen Sie die Schritte 2 und 3 über. Jeder Connector muss separat registriert werden.

So deinstallieren Sie den Connector, deinstallieren Sie den Connector-Dienst und den Updater-Dienst. Neustart des Dienstes vollständig entfernen.


## <a name="next-steps"></a>Nächste Schritte

Sie können jetzt [Veröffentlichen Anwendung Anwendungsproxy](active-directory-application-proxy-publish.md).

Haben Sie Programme, die auf getrennten Netzwerken oder Standorten können connectorgruppen Sie verschiedene Connectors in logische Einheiten strukturieren. Erfahren Sie mehr über das [Arbeiten mit Anwendungsproxy Connectors](active-directory-application-proxy-connectors.md).
