<properties
   pageTitle="Logik Apps lokalen Gateway Datenverbindungstyp | Microsoft Azure"
   description="Informationen zum Erstellen einer Verbindung zum lokalen Daten-Gateway Logik App."
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="07/05/2016"
   ms.author="jehollan"/>

# <a name="connect-to-the-on-premises-data-gateway-for-logic-apps"></a>Verbindung zum lokalen Daten-Gateway für Logik-Apps

Unterstützte Logik apps Connectors können Sie die Verbindung mit lokalen Daten über das lokale Daten-Gateway konfigurieren.  Die folgenden Schritte führen Sie durch das Installieren und Konfigurieren des Gateways für lokale Daten mit einer Logik app.

## <a name="prerequisites"></a>Erforderliche Komponenten

* Eine Arbeit oder Schule Adresse Azure Ihr Konto (Azure Active Directory basierend) das lokalen Daten-Gateway zugeordnet
    * Bei Verwendung von Microsoft Account (z.B. @outlook.com, @live.com) können Sie Ihre Azure-Konto Erstellen einer Arbeitsaufgabe oder e-Mail-Adresse nach [der Anleitung](../virtual-machines/virtual-machines-windows-create-aad-work-id.md#locate-your-default-directory-in-the-azure-classic-portal) Schule

> [AZURE.WARNING] Es besteht derzeit das lokale Gateway installieren nur abgeschlossen wird, wenn ein Konto verwenden, die mit Power BI registriert wurde.  In der Zwischenzeit registrieren Sie ein Konto mit"Power BI" um die Installation erfolgreich abzuschließen.

* Müssen die lokalen Daten-Gateway [auf einem lokalen Computer installiert](app-service-logic-gateway-install.md)haben.
* Gateway muss nicht schon vergeben von anderen lokalen Azure datengateway ([Anspruch geschieht, mit Schritt 2 unten](#2-create-an-azure-on-premises-data-gateway-resource)) - Installation kann nur ein Gateway Ressource zugeordnet werden.

## <a name="installing-and-configuring-the-connection"></a>Installieren und Konfigurieren der Verbindung

### <a name="1-install-the-on-premises-data-gateway"></a>1. installieren Sie 1. das Gateway für lokale Daten

Informationen zum Installieren des lokalen Daten-Gateways finden [in diesem Artikel](app-service-logic-gateway-install.md).  Das Gateway muss auf einem lokalen Computer installiert sein, bevor Sie die restlichen Schritte fortsetzen können.

### <a name="2-create-an-azure-on-premises-data-gateway-resource"></a>2. erstellen Sie eine lokale Azure Gateway Datenressource

Nach der Installation müssen Sie Ihre Azure-Abonnement mit lokalen datengateway zuordnen.

1. Anmeldung bei Azure mit gleiche oder e-Mail-Adresse, die während der Installation des Gateways verwendet wurde
1. **Neue** Ressource klicken
1. Suchen Sie und wählen Sie den **lokalen Daten-gateway**
1. Informationen Sie zum Gateway mit Ihrem Konto - einschließlich der entsprechenden **Installation Namen** zuordnen

    ![Verbindung zum lokalen Daten-Gateway][1]
1. Klicken Sie auf **Erstellen** , um die Ressource zu erstellen

### <a name="3-create-a-logic-app-connection-in-the-designer"></a>3. erstellen Sie eine Logik app Verbindung im designer

Azure-Abonnement eine Instanz des lokalen Daten-Gateways zugeordnet ist, können Sie eine Verbindung aus einer Logik-App erstellen.

1. Öffnen einer Anwendung Logik und wählen Sie einen Connector, der lokalen Verbindung (derzeit SQL Server) unterstützt
1. Aktivieren Sie das Kontrollkästchen für die **Verbindung lokale Daten-gateway**

    ![Logik App Designer Gateway erstellen][2]
1. Wählen Sie den **Gateway** herstellen und alle anderen erforderlichen Verbindungsinformationen
1. Klicken Sie auf **Erstellen** , um die Verbindung zu erstellen

Die Verbindung sollte jetzt erfolgreich für die Verwendung in Ihrer Anwendung Logik konfiguriert werden.  

## <a name="next-steps"></a>Nächste Schritte
- [Häufige Beispiele und Szenarien für Logik-apps](app-service-logic-examples-and-scenarios.md)
- [Enterprise-Integrationsfunktionen](app-service-logic-enterprise-integration-overview.md)

<!-- Image references -->
[1]: ./media/app-service-logic-gateway-connection/createblade.PNG
[2]: ./media/app-service-logic-gateway-connection/blankconnection.PNG
[3]: ./media/app-service-logic-gateway-connection/checkbox.PNG