<properties
    pageTitle="Arbeiten mit Connectors Azure AD-Anwendungsproxy | Microsoft Azure"
    description="Erläutert das Erstellen und Verwalten von Connectors in Azure AD-Anwendungsproxy."
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
    ms.date="09/09/2016"
    ms.author="kgremban"/>


# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups"></a>Veröffentlichen Sie auf separaten Netzwerken und Speicherorte Connector Gruppen

> [AZURE.SELECTOR]
- [Azure-portal](active-directory-application-proxy-connectors-azure-portal.md)
- [Azure-Verwaltungsportal](active-directory-application-proxy-connectors.md)


Connectorgruppen eignen sich für verschiedene Szenarios, einschließlich:

- Websites mit mehreren miteinander verbundenen Rechenzentren. In diesem Fall möchten Sie so viel Verkehr im Rechenzentrum wie möglich halten, weil Cross Datacenter Links in der Regel teuer und zeitaufwändig. Sie können Konnektoren in jedem Rechenzentrum dazu im Rechenzentrum befinden sich die Anwendung bereitstellen. Dieser Ansatz minimiert Cross Datacenter Links und bietet eine vollständig transparent für die Benutzer.
- Verwalten von installierten in isolierten Netzwerken, die nicht Teil der Hauptnetzwerk des Unternehmens. Connectorgruppen können dedizierte Connectors in isolierten Netzwerken auch Programme im Netzwerk isolieren installieren.
- Anwendungsmöglichkeiten für Cloud Zugriff auf IaaS installiert bieten connectorgruppen einen gemeinsamen Dienst zum Sichern des Zugriffs auf die Datei apps. Connectorgruppen nicht erstellen zusätzliche Abhängigkeit im Unternehmensnetzwerk oder fragment app-Erfahrung. Connectors können auf jede Cloud Datacenter installiert werden und dienen nur Programme, die in diesem Netzwerk befinden. Sie können mehrere Connectors hohe Verfügbarkeit zu erreichen.
- Unterstützung für mehrere Gesamtstrukturen Umgebungen in denen spezifischen Connectors pro Gesamtstruktur bereitgestellt und werden bestimmte Applikationen zu.
- Connectorgruppen können in Disaster Recovery-Standorte um entweder Failover erkennen oder als Sicherung der Hauptsite verwendet.
- Connectorgruppen können auch, mehrere Unternehmen über einem einzelnen Mandanten verwendet werden.

## <a name="prerequisite-create-your-connectors"></a>Voraussetzung: Die Connectors erstellen
Um die Connectors gruppieren lassen, stellen Sie sicher, dass Sie [mehrere Connectors installiert](active-directory-application-proxy-enable.md)und Sie Name und gruppieren. Schließlich müssen Sie bestimmte apps zuweisen.

## <a name="step-1-create-connector-groups"></a>Schritt 1: Connectorgruppen erstellen
Sie können beliebig viele connectorgruppen erstellen. Erstellung der Connectors erfolgt im klassischen Azure-Portal.

1. Wählen Sie das Verzeichnis, und klicken Sie auf **Konfigurieren**.  
    ![Anwendungsproxy, Bildschirmabbild konfigurieren - auf connectorgruppen verwalten](./media/active-directory-application-proxy-connectors/app_proxy_connectors_creategroup.png)

2. Unter Anwendungsproxy auf **Anschluss Gruppen verwalten** und neue Connector Gruppe erteilen Sie der Gruppe einen Namen.  
    ![Anwendung Proxy Connector Gruppen Screenshot - neue Namensgruppe](./media/active-directory-application-proxy-connectors/app_proxy_connectors_namegroup.png)

## <a name="step-2-assign-connectors-to-your-groups"></a>Schritt 2: Weisen Sie Connectors zu Gruppen
Erstellte connectorgruppen verschieben Sie die Connectors zur entsprechenden Gruppe.

1. **Anwendungsproxy**klicken Sie auf **Anschlüsse zu verwalten**.
2. Wählen Sie unter **Gruppe**die Gruppe für jeden Connector. Es dauert bis zu 10 Minuten in die neue Gruppe aktiv die Connectors.  
    ![Anwendung Proxy Connectors Screenshot - Gruppe im Dropdown-Menü auswählen](./media/active-directory-application-proxy-connectors/app_proxy_connectors_connectorlist.png)

## <a name="step-3-assign-applications-to-your-connector-groups"></a>Schritt 3: Anwendung connectorgruppen zuweisen
Im letzte Schritt wird jede Anwendung Connector Gruppe festgelegt, die verwendet werden.

1. Wählen Sie im klassischen Azure-Portal, in Ihrem Verzeichnis die Anwendung zuordnen und auf **Konfigurieren**möchten.
2. Wählen Sie unter **Connector Gruppe**der Gruppe, die die Anwendung verwenden soll. Diese Änderung wird sofort übernommen.  
    ![Anwendung Proxy Connector Gruppe Screenshot - Gruppe im Dropdown-Menü auswählen](./media/active-directory-application-proxy-connectors/app_proxy_connectors_newgroup.png)


## <a name="see-also"></a>Siehe auch

- [Anwendungsproxy aktivieren](active-directory-application-proxy-enable.md)
- [Einmalige Anmeldung aktivieren](active-directory-application-proxy-sso-using-kcd.md)
- [Bedingter Zugriff](active-directory-application-proxy-conditional-access.md)
- [Problembehandlung, mit Anwendungsproxy](active-directory-application-proxy-troubleshoot.md)

Überprüfen Sie die aktuellsten Neuigkeiten und Updates [Anwendungsproxy blog](http://blogs.technet.com/b/applicationproxyblog/)
