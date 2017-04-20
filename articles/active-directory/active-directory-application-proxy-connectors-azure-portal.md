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


# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups---public-preview"></a>Veröffentlichen Sie auf separaten Netzwerken und Speicherorte Connector Gruppen - Public Preview

> [AZURE.SELECTOR]
- [Azure-portal](active-directory-application-proxy-connectors-azure-portal.md)
- [Azure-Verwaltungsportal](active-directory-application-proxy-connectors.md)


Connectorgruppen eignen sich für verschiedene Szenarios, einschließlich:

- Websites mit mehreren miteinander verbundenen Rechenzentren. In diesem Fall möchten Sie so viel Verkehr im Datencenter möglichst da Cross Datacenter Links teuer und zeitaufwändig sind. Sie können Konnektoren in jedem Rechenzentrum dazu im Rechenzentrum befinden sich die Anwendung bereitstellen. Dieser Ansatz minimiert Cross Datacenter Links und bietet eine vollständig transparent für die Benutzer.
- Verwalten von installierten in isolierten Netzwerken, die nicht Teil der Hauptnetzwerk des Unternehmens. Connectorgruppen können dedizierte Connectors in isolierten Netzwerken auch Programme im Netzwerk isolieren installieren.
- Anwendungsmöglichkeiten für Cloud Zugriff auf IaaS installiert bieten connectorgruppen einen gemeinsamen Dienst zum Sichern des Zugriffs auf die Datei apps. Connectorgruppen nicht erstellen zusätzliche Abhängigkeit im Unternehmensnetzwerk oder fragment app-Erfahrung. Connectors können auf jede Cloud Datacenter installiert werden und dienen nur Programme, die in diesem Netzwerk befinden. Sie können mehrere Connectors hohe Verfügbarkeit zu erreichen.
- Unterstützung für mehrere Gesamtstrukturen Umgebungen in denen spezifischen Connectors pro Gesamtstruktur bereitgestellt und werden bestimmte Applikationen zu.
- Connectorgruppen können in Disaster Recovery-Standorte um entweder Failover erkennen oder als Sicherung der Hauptsite verwendet.
- Connectorgruppen können auch, mehrere Unternehmen über einem einzelnen Mandanten verwendet werden.

## <a name="prerequisite-create-your-connectors"></a>Voraussetzung: Die Connectors erstellen
Um die Connectors zu gruppieren, müssen Sie sicherstellen, dass Sie [mehrere Connectors installiert](active-directory-application-proxy-enable.md). Bei der Installation eines neuen Connectors verbindet automatisch **der Standardgruppe Connector** .

## <a name="step-1-create-connector-groups"></a>Schritt 1: Connectorgruppen erstellen
Sie können beliebig viele connectorgruppen erstellen. Erstellung der Connector erfolgt in [Azure-Portal](https://portal.azure.com).

1. **Azure Active Directory** Management Dashboard für das Verzeichnis zu wählen. Wählen Sie dort **Anwendung** > **Anwendungsproxy**.

2. Wählen Sie die Schaltfläche **Connector** . Das neue Connector Blatt wird angezeigt.

3. Benennen Sie Ihre neue Anschluss-Gruppe und anschließend im Dropdownmenü auswählen, welche Connectors in diese Gruppe gehören.

4. Nach Abschluss der Connector Gruppe wählen Sie **Speichern** .

## <a name="step-2-assign-applications-to-your-connector-groups"></a>Schritt 2: Anwendung connectorgruppen zuweisen
Im letzte Schritt wird jede Anwendung Connector Gruppe festgelegt, die verwendet werden.

1. Wählen Sie das Management Dashboard für das Verzeichnis **Anwendung** > **Alle Programme** > Application Connector Gruppe zuweisen möchten > **Anwendungsproxy**.
2. Verwenden Sie unter **Gruppe Connectors**im Dropdown-Menü die Gruppe die Anwendung auswählen.
3. Wählen Sie die Änderung **Speichern** .


## <a name="see-also"></a>Siehe auch

- [Anwendungsproxy aktivieren](active-directory-application-proxy-enable.md)
- [Einmalige Anmeldung aktivieren](active-directory-application-proxy-sso-using-kcd.md)
- [Bedingter Zugriff](active-directory-application-proxy-conditional-access.md)
- [Problembehandlung, mit Anwendungsproxy](active-directory-application-proxy-troubleshoot.md)

Überprüfen Sie die aktuellsten Neuigkeiten und Updates [Anwendungsproxy blog](http://blogs.technet.com/b/applicationproxyblog/)
