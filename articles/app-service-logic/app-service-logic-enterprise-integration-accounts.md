<properties 
    pageTitle="Übersicht von Integration und Enterprise-Integrationspaket | Microsoft Azure App Service | Microsoft Azure" 
    description="Erfahren Sie alles über Integration Konten Integrationspaket Enterprise und Logik-apps" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="overview-of-integration-accounts"></a>Übersicht über Integration Konten

## <a name="what-is-an-integration-account"></a>Was ist eine Integration-Konto?
Ein Konto Integration ist ein Azure, die Unternehmensintegration apps Artefakte Schemas, Karten, Zertifikate, Partnern und Verträge verwalten können. Jede Integration Anwendung erstellten müssen ein Integration-Konto verwenden, um z. B. ein Schema, Karte oder das Zertifikat zugreifen.

## <a name="create-an-integration-account"></a>Erstellen Sie ein Konto integration 
1. Wählen Sie **Durchsuchen**   
![](./media/app-service-logic-enterprise-integration-accounts/account-1.png)  
2. Geben Sie im Suchfeld Filter **Integration** und wählen **Integration Konten** aus der Liste     
 ![](./media/app-service-logic-enterprise-integration-accounts/account-2.png)  
3. Wählen Sie im Menü am oberen Rand der Seite *Hinzufügen*      
![](./media/app-service-logic-enterprise-integration-accounts/account-3.png)  
4. Geben Sie den **Namen**, das **Abonnement** zu verwenden, erstellen Sie eine neue **Ressourcengruppe** oder eine Ressourcengruppe auswählen, wählen Sie einen **Speicherort** , in dem Ihr Konto Integration wird, wählen **Preisstufe**, und **Erstellen** .   

  An diesem Punkt wird Integration Konto an der gewählten bereitgestellt werden. Dies sollte innerhalb einer Minute abgeschlossen.    
![](./media/app-service-logic-enterprise-integration-accounts/account-4.png)  
5. Aktualisieren Sie die Seite. Sie sehen das neue Integration Konto aufgeführt. Herzlichen Glückwunsch!  
![](./media/app-service-logic-enterprise-integration-accounts/account-5.png) 

## <a name="how-to-link-an-integration-account-to-a-logic-app"></a>Verknüpfen ein Kontos Integration mit Logik-app
Reihenfolge für Logik-apps auf Karten, Schemas, Verträge und andere Artefakte, die sich in Ihrem Konto Integration müssen Sie Ihre Anwendung Logik zuerst Integration Konto verknüpfen.

### <a name="here-are-the-steps-to-link-an-integration-account-to-a-logic-app"></a>Hier sind die Schritte, verknüpfen Sie ein Mitgliedskonto Integration Logik-App 

#### <a name="prerequisites"></a>Erforderliche Komponenten
- Ein Konto integration
- Eine Logik-app

>[AZURE.NOTE]Die Integration Konto und Logik-app in **Azure Speicherort** sind vor

1. Menü **Einstellungen** der Logik-app  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-1.png)   
2. Wählen Sie **Konto Integration** Einstellungen-Blades  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-2.png)   
3. Wählen Sie das Konto Integration zu verknüpfen, die Logik app **Integration Konto auswählen** Dropdown-Listenfeld  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-3.png)   
4. Speichern Sie Ihre Arbeit  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-4.png)   
5. Eine Benachrichtigung wird angezeigt, die bedeutet, dass Ihr Konto Integration Ihrer Anwendung Logik verknüpft wurde und alle Elemente in Ihrem Konto Integration jetzt Ihre app Logik zur Verfügung stehen.  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-5.png)   

Da Ihr Konto Integration Ihrer Anwendung Logik verknüpft ist, können Sie für Ihre Anwendung Logik gehen und B2B-Connectors oder die XML-Validierung, Flatfile codieren/decodieren Transformation apps B2B-Funktionen erstellen.  
    
## <a name="how-to-delete-an-integration-account"></a>Wie ein Integration Konto löschen?
1. Wählen Sie **Durchsuchen**  
![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. Geben Sie im Suchfeld Filter **Integration** und wählen **Integration Konten** aus der Liste     
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. Wählen Sie das **Konto Integration** , die Sie löschen möchten  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4. Wählen Sie die Verknüpfung **Löschen** im Menü befindet   
![](./media/app-service-logic-enterprise-integration-accounts/delete.png)  
5. Bestätigen    

## <a name="how-to-move-an-integration-account"></a>Wie eine Integration Konto verschieben?
Sie können ein Konto Integration einfach ein neues Abonnement und eine neue Ressourcengruppe verschieben. Möchten Sie die Integration Konto gehen Sie folgendermaßen vor:

>[AZURE.IMPORTANT] Sie müssen alle Skripts verwenden die neue Ressourcen-IDs nach dem Verschieben einer Integration Konto aktualisieren.

1. Wählen Sie **Durchsuchen**  
![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. Geben Sie im Suchfeld Filter **Integration** und wählen **Integration Konten** aus der Liste     
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. Wählen Sie das **Konto Integration** , die Sie löschen möchten  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4. Wählen Sie den Link **Verschieben** , der im Menü befindet   
![](./media/app-service-logic-enterprise-integration-accounts/move.png)  
5. Bestätigen    

## <a name="next-steps"></a>Nächste Schritte
- [Erfahren Sie mehr über Enterprise-Integrationspaket] (./app-service-logic-enterprise-integration-overview.md "Erfahren Sie mehr über Enterprise-Integrationspaket")  
- [Weitere Informationen zu Verträgen] (./app-service-logic-enterprise-integration-agreements.md "Erfahren Sie mehr über Enterprise Integration Abkommen")  


 