<properties 
    pageTitle="Wie Sie eine AS2-Vereinbarung für Enterprise-Integrationspaket erstellen" 
    description="Erstellen einer AS2-Vereinbarung für Enterprise-Integrationspaket lernen | Microsoft Azure App Service" 
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
    ms.date="06/29/2016" 
    ms.author="deonhe"/>

# <a name="enterprise-integration-with-as2"></a>Enterprise-Integration mit AS2

## <a name="create-an-as2-agreement"></a>AS2 Vereinbarung
Verwendung von Enterprise-Features in Logik-apps müssen Verträge erstellen. 

### <a name="heres-what-you-need-before-you-get-started"></a>Sie benötigen zunächst
- Ein [Konto Integration](./app-service-logic-enterprise-integration-accounts.md) in Azure-Abonnement definiert  
- Mindestens zwei [Partner](./app-service-logic-enterprise-integration-partners.md) bereits in Ihrem Konto integration  

>[AZURE.NOTE]Beim Erstellen einer Vereinbarung muss der Inhalt der Datei Vereinbarung der Vertragstyp übereinstimmen.    


Nachdem Sie [erstellt ein Konto Integration](./app-service-logic-enterprise-integration-accounts.md) und [Partner](./app-service-logic-enterprise-integration-partners.md)haben, können Sie eine Vereinbarung folgendermaßen erstellen:  

### <a name="from-the-azure-portal-home-page"></a>Von Azure Homepage des Portals

Nach dem Anmelden in [Azure-Portal](http://portal.azure.com "Azure-Portal"):  
1. Wählen Sie im Menü auf der linken Seite **Durchsuchen** .  

>[AZURE.TIP]Wenn Sie den Link **Suchen** nicht sehen, müssen Sie zuerst das Menü zu erweitern. Wählen Sie zunächst den **Menü** -Link, der befindet reduzierten Menü links oben.  

![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. *Integration* in das Suchfeld Filter geben und **Integration Konten** aus der Liste der Ergebnisse auswählen.       
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. Öffnet **Integration Konten** Blatt wählen Sie das Integration in dem Sie die Vereinbarung erstellen. Wenn eine Integration Konten Listen angezeigt [Erstellen Sie einen ersten](./app-service-logic-enterprise-integration-accounts.md "All about integration accounts").  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4.  Wählen Sie das Musterelement **Abkommen** . Wenn Abkommen Kachel zuerst hinzufügen nicht angezeigt wird.   
![](./media/app-service-logic-enterprise-integration-agreements/agreement-1.png)   
5. Wählen Sie **Add** Abkommen Blatt, das geöffnet wird.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-2.png)  
6. Geben Sie einen **Namen** für die Vereinbarung und wählen Sie dann den **Host Partner** **Host Identität** **Gastpartner**, **Gast Identität**Abkommen Blatt, das geöffnet wird.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-3.png)  

Hier sind einige Details finden Sie hilfreich beim Konfigurieren der Einstellungen für die Vereinbarung: 
  
|Eigenschaft|Beschreibung|
|----|----|
|Host-Partner|Eine Vereinbarung braucht einen Host und Gast-Partner. Host-Partner stellt die Organisation, die die Vereinbarung konfigurieren.|
|Host-Identität|Ein Bezeichner für den Host-Partner. |
|Gastpartner|Eine Vereinbarung braucht einen Host und Gast-Partner. Gastpartner stellt Unternehmen, die Geschäfte mit der Host-Partner.|
|Gast-Identität|Ein Bezeichner für den gastpartner.|
|Einstellungen erhalten|Diese Eigenschaften gelten für alle Nachrichten von einer Vereinbarung|
|Einstellungen senden|Diese Eigenschaften gelten für alle Nachrichten von einer Vereinbarung|  
Weiter:  
7. Wählen Sie **Erhalten Einstellungen** zu konfigurieren, wie Nachrichten über diese Vereinbarung behandelt werden.  
 
 - Optional können Sie die Eigenschaften der eingehenden Nachricht überschreiben. Dazu aktivieren Sie das Kontrollkästchen **Eigenschaften überschreiben** .
  - Aktivieren Sie das Kontrollkästchen **Nachricht signiert** , möchten Sie alle eingehende Nachrichten signiert werden müssen. Wenn Sie diese Option auswählen, müssen Sie das **Zertifikat** auswählen, mit der die Signatur auf Nachrichten überprüfen.
  - Optional können Sie Nachrichten verschlüsselt werden muss. Dazu aktivieren Sie das Kontrollkästchen **Nachricht verschlüsselt werden soll** . Sie müssten das **Zertifikat** auswählen, mit der eingehenden Nachrichten entschlüsseln.
  - Sie können auch Nachrichten komprimiert festlegen. Dazu aktivieren Sie das Kontrollkästchen **Nachricht komprimiert werden soll** .  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-4.png)  

Möchten Sie mehr über den Empfang können, finden Sie unter Tabelle.  

|Eigenschaft|Beschreibung|
|----|----|
|Überschreiben von Eigenschaften|Wählen Sie diese Option, um anzugeben, dass empfangene Nachrichten außer Kraft gesetzt werden können |
|Nachricht muss signiert werden|Aktivieren Sie diese Nachrichten digital signiert werden muss|
|Nachricht sollte verschlüsselt werden|Aktivieren Sie diese Nachrichten verschlüsselt werden. Nicht verschlüsselte Nachrichten werden zurückgewiesen.|
|Nachricht muss komprimiert werden|Aktivieren Sie diese Nachrichten komprimiert werden. Nicht-komprimierte Nachrichten werden zurückgewiesen.|
|MDN Text|Dies ist eine Standard-MDN an den Absender der Nachricht|
|MDN senden|Aktivieren Sie diese MDNs gesendet werden können.|
|Signierte MDN senden|Aktivieren Sie diese MDNs signiert werden muss.|
|MIC-Algorithmus||
|Asynchrone MDN senden|Aktivieren Sie diese Nachrichten asynchron gesendet werden.|
|URL|Dies ist die URL, an die Nachrichten gesendet werden.|
Jetzt weiter:  
8. Wählen Sie **Senden Einstellungen** zu konfigurieren, wie Nachrichten über diese Vereinbarung behandelt werden.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-5.png)  

Möchten Sie mehr über das senden können, finden Sie unter der Tabelle.  

|Eigenschaft|Beschreibung|
|----|----|
|Signieren von Nachrichten aktivieren|Wählen Sie dieses Kontrollkästchen, damit alle Nachrichten aus der Vereinbarung unterzeichnet werden.|
|MIC-Algorithmus|Wählen Sie den Algorithmus zum Signieren von Nachrichten verwenden|
|Zertifikat|Wählen Sie das Zertifikat zum Signieren der Nachricht verwenden|
|Aktivieren der Verschlüsselung von Nachrichten|Aktivieren Sie dieses Kontrollkästchen, um alle Nachrichten aus dieser Vereinbarung zu verschlüsseln.|
|Verschlüsselungsalgorithmus|Wählen Sie den Verschlüsselungsalgorithmus zum Verschlüsseln verwenden|
|HTTP-Header entfalten|Aktivieren Sie dieses Kontrollkästchen HTTP Content-Type-Header in einer einzelnen Zeile zu entfalten.|
|MDN anfordern|Aktivieren Sie dieses Kontrollkästchen, um eine MDN für alle Nachrichten aus dieser Vereinbarung anfordern|
|Signierte MDN anfordern|Aktivieren, um anfordern alle MDNs dieser Vereinbarung unterzeichnet|
|Asynchrone MDN anfordern|Asynchrone MDN an dieser Vereinbarung anfordern aktivieren|
|URL|Die URL der MDNs an|
|NRR aktivieren|Aktivieren Sie dieses Kontrollkästchen Nachweisbarkeit Eingang aktivieren|
Wir sind fast fertig!  
9. Wählen Sie das Musterelement **Abkommen** Blade-Integration Konto und finden Sie neue Vereinbarung aufgeführt.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-6.png)

