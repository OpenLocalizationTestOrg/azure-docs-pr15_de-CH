<properties
    pageTitle="Hohe Verfügbarkeit zwischen geografischen AD FS-Bereitstellung in Azure mit Azure Traffic Manager | Microsoft Azure"
    description="In diesem Dokument erfahren Sie, wie Sie AD FS in Azure für hohe Verfügbarkeit bereitstellen."
    keywords="AD fs Azure Traffic Manager, Adfs mit Azure Traffic Manager geografische Multi Datacenter geografische Rechenzentren, mehrere geografische Rechenzentren AD FS in Azure bereitstellen, Bereitstellen von Azure Adfs Azure Adfs, Azure Ad fs Adfs bereitstellen, Bereitstellen von Ad fs Adfs in Azure Adfs in Azure bereitstellen, Bereitstellen von AD FS Azure Adfs Azure, Einführung in AD FS, Azure, AD FS in Azure Iaas , ADFS Adfs in Azure verschieben"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="anandy;billmath"/>
    
#<a name="high-availability-cross-geographic-ad-fs-deployment-in-azure-with-azure-traffic-manager"></a>Hohe Verfügbarkeit zwischen geografischen AD FS-Bereitstellung in Azure mit Azure Traffic Manager

[AD FS-Bereitstellung in Azure](active-directory-aadconnect-azure-adfs.md) bietet schrittweise Leitfaden, wie Sie eine einfache AD FS-Infrastruktur für Ihre Organisation in Azure bereitstellen können. Dieser Artikel enthält die nächsten Schritte erstellen eine übergreifende geografische Bereitstellung von AD FS in Azure [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md)verwenden. Azure Traffic Manager sorgt ein geografisch Druckbogen, hohe Verfügbarkeit und leistungsstarke AD FS-Infrastruktur für Ihre Organisation machen Verteilermethoden für unterschiedliche Bedürfnisse der Infrastruktur einsetzen.

Eine Infrastruktur mit hoher verfügbare Cross geografische AD FS ermöglicht:

* **Beseitigung der Fehlerquelle:** Mit Failover-Funktionen von Azure Traffic Manager erreichen eine AD FS-Infrastruktur mit hoher verfügbare Sie auch wenn eines der Daten in einem Teil der Welt nach unten
* **Verbessert:** Können die empfohlene Bereitstellung in diesem Artikel eine leistungsstarke AD FS-Infrastruktur bereitstellen, die Benutzer schneller zu authentifizieren. 

##<a name="design-principles"></a>Entwurfsprinzipien

![Gesamtentwurf](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/blockdiagram.png)

Grundlegenden Entwurfsprinzipien werden wie Entwurfsprinzipien im AD FS-Bereitstellung in Azure-Artikel aufgeführt. Das Diagramm oben zeigt eine einfache Erweiterung der grundlegenden Bereitstellung einer anderen geographischen Region. Im folgenden sind einige Punkte, die bei die Bereitstellung neuer geografischen Bereich erweitern

* **Virtual Network:** Sie sollten ein neues virtuelles Netzwerk in der geografischen Region erstellen zusätzlichen AD FS-Infrastruktur bereitstellen. Im obigen Diagramm angezeigt Geo1 VNET und Geo2 VNET als zwei virtuelle Netzwerke in jeder geografischen Region.

* **Domänencontroller und AD FS-Servern in neue geografische VNET:** Es wird empfohlen, Domänen Controller in neuen geografischen Region damit AD FS-Server in der neuen Region kein Domänencontroller in einer anderen weit Netzwerk zum Abschließen einer Authentifizierung und verbessern damit die Leistung bereitstellen.

* **Speicherkonten:** Speicherkonten werden eine Region zugeordnet. Da Sie Computer in eine neue Region bereitstellen, müssen Sie zum Erstellen neuer Speicherkonten in der Region verwendet werden.  

* **Netzwerk-Sicherheitsgruppen:** Wie in anderen geografischen Region Speicherkonten Netzwerksicherheitsgruppen erstellt in einem Bereich verwendet werden können. Daher müssen Sie neues Netzwerk Sicherheitsgruppen in neuen geografischen Region ähnlich der ersten geografischen für INT und DMZ-Subnetz erstellen.

* **DNS-Etiketten für öffentliche IP-Adressen:** Azure Traffic Manager Endpunkte finden nur über DNS-Etiketten. Daher müssen Sie DNS-Etiketten für die externe Load-Balancer öffentlichen IP-Adressen erstellen.

* **Azure Traffic Manager:** Microsoft Azure Traffic Manager ermöglicht die Verteilung von Benutzern auf Ihre Endpunkte aus verschiedenen Rechenzentren auf der ganzen Welt steuern. Azure Traffic Manager funktioniert DNS-Ebene. DNS-Antworten verwendet direkten Endbenutzer-Verkehr an weltweit verteilten Endpunkte. Clients verbinden dann direkt mit diesen Endpunkten. Mit anderen Routingoptionen Leistung, gewichtet und Priorität können Sie problemlos die routing-Option für Ihre Organisation am besten geeignet. 

* **V-Netz zu V-Netz Konnektivität zwischen zwei Bereiche:** Sie müssen nicht die virtuellen Netzwerke selbst haben. Da jedes virtuelles Netzwerk Zugriff auf Domänencontroller und AD FS und WAP-Server ist, können sie ohne jede Verbindung zwischen virtuellen Netzwerken in unterschiedlichen Regionen arbeiten. 

##<a name="steps-to-integrate-azure-traffic-manager"></a>Schritte zur Integration von Azure Traffic Manager

###<a name="deploy-ad-fs-in-the-new-geographical-region"></a>Bereitstellen von AD FS in neuen geografischen region
Führen Sie die Schritte und Richtlinien in [AD FS-Bereitstellung in Azure](active-directory-aadconnect-azure-adfs.md) Topologie in neuen geografischen Region.

###<a name="dns-labels-for-public-ip-addresses-of-the-internet-facing-public-load-balancers"></a>DNS-Etiketten für öffentliche IP-Adressen der Lastenausgleich Internet Facing (öffentlich)
Wie bereits erwähnt, Azure Traffic Manager finden nur DNS-Etiketten als Endpunkte und deshalb DNS-Etiketten für die externe Load-Balancer öffentlichen IP-Adressen. Screenshot unten zeigt, wie die DNS-Bezeichnung für die öffentliche IP-Adresse konfigurieren können. 

![DNS-Bezeichnung](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfabstsdnslabel.png)

###<a name="deploying-azure-traffic-manager"></a>Bereitstellen von Azure Traffic Manager

Schritte ein Traffic Manager-Profil erstellen. Weitere Informationen finden Sie auch [ein Profil Azure Traffic Manager](../traffic-manager/traffic-manager-manage-profiles.md)verwalten.

1. **Traffic Manager-Profil erstellen:** Geben Sie Ihr Profil Traffic Manager einen eindeutigen Namen. Dieser Name des Profils ist Teil der DNS-Name und ein Präfix für den Namen des Traffic Manager-Domäne fungiert. Der Name / Präfix hinzugefügt. trafficmanager.net DNS-Bezeichnung für den Verkehr-Manager erstellen. Der folgende Screenshot zeigt Traffic Manager DNS-Präfix als Mysts und resultierende DNS-Bezeichnung mysts.trafficmanager.net. 

    ![Traffic Manager Profil erstellen](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/trafficmanager01.png)
 
2. **Datenverkehr Routingmethode:** Gibt drei Routingoptionen Traffic Manager:

    * Priorität 
    * Leistung
    * Gewichteter
    
    **Leistung** wird empfohlen, reaktionsschnellen AD FS-Infrastruktur. Sie können jedoch Verteilungsmethode für Ihre Bedürfnisse am besten geeignet. AD FS-Funktionalität ist der Option routing nicht beeinflusst. Weitere Informationen finden Sie unter [Traffic Manager Datenverkehr Verteilermethoden](../traffic-manager/traffic-manager-routing-methods.md) . In diesem Beispiel sehen Screenshot oben **Leistung** Methode ausgewählt.
   
3.  **Endpunkte konfigurieren:** Traffic Manager Seite Endpunkte auf, und wählen Sie hinzufügen. Eine hinzufügen Seite ähnlich wie im folgenden Screenshot wird geöffnet
 
    ![Endpunkte konfigurieren](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfsendpoint.png)
 
    Anderen Eingaben führen Sie die folgenden Richtlinien:

    **Typ:** Wählen Sie Azure Endpunkt, wie wir ein Azure öffentlichen IP-Adresse verweisen.

    **Name:** Erstellen Sie einen Namen, den dem Endpunkt zugeordnet werden soll. Dies ist nicht der DNS-Name und hat keinen Einfluss auf die DNS-Einträge.

    **Ziel Ressourcentyp:** Wählen Sie öffentliche IP-Adresse als Wert für diese Eigenschaft. 

    **Ziel Ressource:** Dadurch erhalten Sie eine Option an unterschiedlichen DNS-Etiketten unter Abonnements zur Verfügung steht. Wählen Sie die DNS-Bezeichnung an.

    Fügen Sie für jede geografische Region Azure Traffic Manager Datenverkehr zum gewünschten Endpunkt.
    Weitere Informationen sowie detaillierte Schritte zum Hinzufügen / Konfigurieren Sie Endpunkte im Verkehr-Manager finden Sie in [Hinzufügen, deaktivieren, aktivieren oder Löschen von Endpunkten](../traffic-manager/traffic-manager-endpoints.md)
    
4. **Konfigurieren Prüfpunkt:** Traffic Manager Seite klicken Sie auf Konfiguration. Auf der Konfigurationsseite müssen, HTTP-Port 80 zu relativen Pfad /adfs/probe Prüfpunkt Monitor ändern

    ![Konfigurieren der probe](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/mystsconfig.png) 

    >[AZURE.NOTE] **Stellen Sie sicher, dass der Status der Endpunkte ONLINE nach Abschluss der Konfiguration**. Wenn alle Endpunkte "intakt" befinden, führen Azure Traffic Manager besten Versuch zum Weiterleiten von Datenverkehr vorausgesetzt, dass die Diagnose falsch und alle Endpunkte erreichbar.

5. **Ändern von DNS-Datensätzen für Azure Traffic Manager:** Der Verbunddienst sollte CNAME Azure Traffic Manager DNS-Namen. Erstellen Sie CNAME in öffentlichen DNS-Datensätze, damit wer versucht den Verbunddienst zu Azure Traffic Manager erreicht.

    Beispielsweise darauf Federation Service fs.fabidentity.com Traffic Manager, müssten Sie Ihre Ressourceneintrag Folgendes zu aktualisieren:

    <code>fs.fabidentity.com IN CNAME mysts.trafficmanager.net</code>

##<a name="test-the-routing-and-ad-fs-sign-in"></a>Testen Sie das routing und AD FS-Anmeldung   

###<a name="routing-test"></a>Arbeitsplan testen

Ein grundlegenden Test für den Arbeitsplan wäre Federation Service DNS-Namen von einem Computer in jeder geografischen Region ping versuchen. Je nach routing ausgewählt wird, tatsächlich Pingt, Endpunkt Ping an berücksichtigt. Beispielsweise bei Auswahl von Performance-routing wird dann der Endpunkt am nächsten Bereich des Clients erreicht. Es folgt Snapshot zwei Pings aus zwei anderen Region Clientcomputer ostasiatische Region und im Westen der USA. 

![Arbeitsplan testen](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/pingtest.png)

###<a name="ad-fs-sign-in-test"></a>AD FS-in-test

AD FS testen am einfachsten mithilfe der Seite IdpInitiatedSignon.aspx. Um die IdpInitiatedSignOn der AD FS-Eigenschaften aktivieren muss. Gehen Sie die AD FS-Installation überprüfen
 
1. Führen Sie die folgenden Cmdlet auf dem AD FS PowerShell auf aktiviert festgelegt. $True Set-AdfsProperties - EnableIdPInitiatedSignonPage
2. Von jeder externe Computer Zugriff https://<yourfederationservicedns>/adfs/ls/IdpInitiatedSignon.aspx
3. Die AD FS-Seite sollte angezeigt werden wie folgt:

    ![Test der ADFS - Herausforderung](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest1.png)

    und bei erfolgreicher Anmeldung, es wird Ihnen eine wie folgt:

    ![Test der ADFS - Authentifizierung erfolgreich](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest2.png)
 
##<a name="related-links"></a>Verwandte links
* [Grundlegende AD FS-Bereitstellung in Azure](active-directory-aadconnect-azure-adfs.md)
* [Microsoft Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md)
* [Traffic Manager Datenverkehr Verteilermethoden](../traffic-manager/traffic-manager-routing-methods.md)

##<a name="next-steps"></a>Nächste Schritte
* [Ein Azure Traffic Manager-Profil verwalten](../traffic-manager/traffic-manager-manage-profiles.md)
* [Hinzufügen, deaktivieren, aktivieren oder Löschen von Endpunkten](../traffic-manager/traffic-manager-endpoints.md) 

