<properties 
    pageTitle="Erstellen und Verwalten von Hybridverbindungen | Microsoft Azure" 
    description="Informationen Sie zum Erstellen einer hybridverbindung und die Verbindung Hybrid Verbindungs-Manager installieren. MAK, WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="ccompy"/>


# <a name="create-and-manage-hybrid-connections"></a>Erstellen und Verwalten von Hybridverbindungen


## <a name="overview-of-the-steps"></a>Übersicht über die Schritte
1. Erstellen einer Hybrid-Verbindung durch Eingabe des **Hostnamens** oder **FQDN** der lokalen Ressource in Ihrem privaten Netzwerk.
2. Verknüpfen Sie Ihre Azure webapps oder Azure mobile Hybrid-Verbindung.
3. Installieren Sie der Hybrid-Verbindungs-Manager auf die lokale Ressource und bestimmte Hybrid-Verbindung an. Azure-Portal bietet Klick installieren und verbinden.
4. Verwalten Sie Hybrid-Verbindungen und ihre Verbindungsschlüssel.

Dieses Thema listet diese Schritte. 

> [AZURE.IMPORTANT] Es ist möglich, einen Endpunkt Hybrid-Verbindung eine IP-Adresse festgelegt. Verwenden Sie eine IP-Adresse kann oder erreichen nicht die lokale Ressource je nach Ihrem. Hybrid-Verbindung hängt der Client eine DNS-Suche durchführen. In den meisten Fällen ist der __Client__ den Anwendungscode. Wenn der Client keine DNS-Suche durchführen (es wird nicht versucht, die IP-Adresse aufgelöst wird, als wäre es ein Domänenname (x.x.x.x)), dann Datenverkehr nicht über die Hybrid-Verbindung gesendet wird.
>
> Beispiel (in Pseudocode) definieren Sie **10.4.5.6** als lokalen Host:
> 
> **Das folgende Szenario funktioniert:**  
> `Application code -> GetHostByName("10.4.5.6") -> Resolves to 127.0.0.3 -> Connect("127.0.0.3") -> Hybrid Connection -> on-prem host`
> 
> **Das folgende Szenario funktioniert nicht:**  
> `Application code -> Connect("10.4.5.6") -> ?? -> No route to host`


## <a name="CreateHybridConnection"></a>Erstellen einer Hybridverbindung

Hybrid-Verbindung kann in Azure-Portal Web Apps **oder** verwenden der BizTalk-Dienste erstellt werden. 

**Hybrid-Verbindungen mit Web Apps erstellen**, finden Sie unter [Verbinden Azure Web Apps auf eine Ressource lokal auf](../app-service-web/web-sites-hybrid-connection-get-started.md). Sie können auch Hybrid Connection Manager (HCM) von Ihrer Anwendung ist die bevorzugte Methode. 

**Hybrid-Verbindungen BizTalk-Dienste erstellen**:

1. Melden Sie sich bei [Azure-Verwaltungsportal](http://go.microsoft.com/fwlink/p/?LinkID=213885)an.
2. Klicken Sie im linken Navigationsbereich wählen Sie **BizTalk-Dienste** , und wählen Sie BizTalk Service. 

    Wenn Sie eine vorhandene BizTalk Service haben, können Sie [Erstellen eines BizTalk-Dienstes](biztalk-provision-services.md).
3. Wählen Sie die Registerkarte **Hybrid-Verbindung** :  
![Registerkarte Verbindung Hybrid][HybridConnectionTab]

4. Wählen Sie **Hybridverbindung erstellen** oder **die Schaltfläche** in der Taskleiste. Geben Sie Folgendes ein:

    Eigenschaft | Beschreibung
--- | ---
Name | Hybrid-Verbindungszeichenfolge muss eindeutig sein und darf nicht den gleichen Namen wie der BizTalk Service sein. Sie können einen beliebigen Namen geben jedoch mit ihren Zweck. Beispiele:<br/><br/>Gehaltsbuchhaltung-*SQLServer*<br/>SupplyList*SharepointServer*<br/>Kunden-*OracleServer*
Host-Name | Geben Sie den vollständig qualifizierten Hostnamen nur den Hostnamen und die IPv4-Adresse der lokalen Ressource. Beispiele:<br/><br/>mySQLServer<br/>*MySQLServer*. *Domäne*. corp.*Jobs*.com<br/>*myHTTPSharePointServer*<br/>*MyHTTPSharePointServer*. *Jobs*.com<br/>10.100.10.10<br/><br/>Beachten Sie die IPv4-Adresse verwenden, dass Client oder Anwendung Code nicht die IP-Adresse auflösen kann. Siehe Hinweis am Anfang dieses Themas wichtig.
Anschluss | Geben Sie die Portnummer der lokalen Ressource. Beispielsweise wenn Web Apps verwenden, geben Sie Port 80 oder 443. Wenn Sie SQL Server verwenden, geben Sie Port 1433.

5. Wählen Sie das Häkchen, um das Setup abzuschließen. 

#### <a name="additional"></a>Zusätzliche

- Mehrere Hybridverbindungen können erstellt werden. Finden Sie die [der BizTalk-Dienste: Editionen Diagramm](biztalk-editions-feature-chart.md) für die Anzahl der zulässigen Clientverbindungen. 
- Jede Hybridverbindung mit Verbindungszeichenfolgen erstellt: Anwendung Schlüssel, senden und lokalen Schlüssel, die ÜBERWACHEN. Jedes Paar hat einen primären und einen sekundären Schlüssel. 


## <a name="LinkWebSite"></a>Verknüpfen Sie Ihre Azure App Service WebApp oder Mobile-App

Um eine Hybridverbindung eine Web App oder Mobile-Anwendung in Azure App Service verknüpfen Wählen Sie **eine vorhandene Hybrid-Verbindung** Hybrid-Verbindungen Blatt aus [Zugriff auf lokale Ressourcen hybridverbindungen in Azure App Service](../app-service-web/web-sites-hybrid-connection-get-started.md)anzeigen

## <a name="InstallHCM"></a>Die lokal Hybrid-Verbindungs-Manager installieren

Nach dem Erstellen einer Hybrid-Verbindung Installieren der Hybrid-Verbindungs-Manager für die lokale Ressource. Sie können Ihre Azure Web apps oder BizTalk Service heruntergeladen werden. BizTalk-Dienste Schritte: 

1. Melden Sie sich bei [Azure-Verwaltungsportal](http://go.microsoft.com/fwlink/p/?LinkID=213885)an.
2. Klicken Sie im linken Navigationsbereich wählen Sie **BizTalk-Dienste** , und wählen Sie BizTalk Service. 
3. Wählen Sie die Registerkarte **Hybrid-Verbindung** :  
![Registerkarte Verbindung Hybrid][HybridConnectionTab]
4. Wählen Sie in der Taskleiste **Für lokale Installation**:  
![Lokale Installation][HCOnPremSetup]
5. Wählen Sie **Installieren und konfigurieren** oder den Hybrid-Verbindungs-Manager auf dem lokalen System herunterladen. 
6. Wählen Sie das Häkchen, um die Installation zu starten. 

<!--
You can also download the Hybrid Connection Manager MSI file and copy the file to your on-premises resource. Specific steps:

1. Copy the on-premises primary Connection String. See [Manage Hybrid Connections](#ManageHybridConnection) in this topic for the specific steps.
2. Download the Hybrid Connection Manager MSI file. 
3. On the on-premises resource, install the Hybrid Connection Manager from the MSI file. 
4. Using Windows PowerShell, type: 
> Add-HybridConnection -ConnectionString “*Your On-Premises Connection String that you copied*” 
--> 

#### <a name="additional"></a>Zusätzliche
- Hybrid-Verbindungs-Manager kann auf folgenden Betriebssystemen installiert werden:

    - Windows Server 2008 R2 (.NET Framework 4.5 + und Windows Management Framework 4.0 + erforderlich)
    - Windows Server 2012 (Windows Management Framework 4.0 + erforderlich)
    - Windows Server 2012 R2


- Nach der Installation der Verbindungsmanager Hybrid gilt Folgendes: 

    - Azure gehostet Hybrid-Verbindung wird automatisch konfiguriert die Verbindungszeichenfolge der primären Anwendung. 
    - Auf lokale Ressourcen wird automatisch für die primäre auf lokale Verbindungszeichenfolge konfiguriert.

- Der Hybrid-Verbindungs-Manager muss eine gültige lokale Verbindungszeichenfolge für die Autorisierung verwenden. Die Azure Web Apps oder Mobile muss eine gültige Anwendung Verbindungszeichenfolge für die Autorisierung verwenden.
- Eine andere Instanz der Hybrid-Verbindungs-Manager auf einem anderen Server installieren, um Hybrid-Verbindungen zu skalieren. Konfigurieren des lokalen Listeners um dieselbe Adresse als ersten lokalen Listener verwenden. In diesem Fall ist der Verkehr zwischen lokalen aktiven Listener zufällig verteilte (Round Robin). 


## <a name="ManageHybridConnection"></a>Hybridverbindungen verwalten
Zum Verwalten der Hybrid-Verbindungen können Sie:

- Verwenden des Azure-Portals und der BizTalk-Service. 
- Verwenden Sie [REST-APIs](http://msdn.microsoft.com/library/azure/dn232347.aspx).

#### <a name="copyregenerate-the-hybrid-connection-strings"></a>Regenerieren Sie kopieren/Hybrid-Verbindungszeichenfolgen

1. Melden Sie sich bei [Azure-Verwaltungsportal](http://go.microsoft.com/fwlink/p/?LinkID=213885)an.
2. Klicken Sie im linken Navigationsbereich wählen Sie **BizTalk-Dienste** , und wählen Sie BizTalk Service. 
3. Wählen Sie die Registerkarte **Hybrid-Verbindung** :  
![Registerkarte Verbindung Hybrid][HybridConnectionTab]
4. Wählen Sie Hybridverbindung. Wählen Sie in der Taskleiste **Verbindung verwalten**:  
![Verwalten von Optionen][HCManageConnection]

    **Verbindung verwalten** Listet die Verbindungszeichenfolgen Anwendung und lokalen. Sie können die Verbindungszeichenfolgen kopieren oder Regenerieren Zugriffstaste in der Verbindungszeichenfolge. 

    **Wenn Sie Regenerieren**, freigegebene Zugriffstaste in der Verbindungszeichenfolge wird geändert. Folgendermaßen Sie vor:
    - Wählen Sie in der Azure-Verwaltungsportal **Sync-Schlüssel** in der Azure-Anwendung.
    - Führen Sie den **Lokalen Setup**erneut aus. Wenn Sie Räume auf Setup erneut ausführen, ist die lokale Ressource automatisch konfiguriert aktualisierten primären Verbindungszeichenfolge.


#### <a name="use-group-policy-to-control-the-on-premises-resources-used-by-a-hybrid-connection"></a>Verwenden von Gruppenrichtlinien zum Steuern, lokale Ressourcen Hybrid-Verbindung

1. [Hybrid Connection Manager Administrative Vorlagen](http://www.microsoft.com/download/details.aspx?id=42963)herunterladen.
2. Extrahieren Sie die Dateien.
3. Auf dem Computer, Gruppenrichtlinien ändert, gehen Sie folgendermaßen vor:  

    - Kopieren der. ADMX-Dateien in den Ordner *%WINROOT%\PolicyDefinitions* .
    - Kopieren der. ADML-Dateien in den Ordner *%WINROOT%\PolicyDefinitions\en-us* .

Kopiert, können Gruppenrichtlinien-Editor Sie um die Richtlinie zu ändern.




## <a name="next"></a>Weiter

[Verbinden Sie Azure webapps auf eine lokale Ressource](../app-service-web/web-sites-hybrid-connection-get-started.md)  
[Verbindung mit lokalen SQL Server von Azure webapps](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md)   
[Hybrid-Verbindungen (Übersicht)](integration-hybrid-connection-overview.md)


## <a name="see-also"></a>Siehe auch

[REST-API zum Verwalten von BizTalk-Diensten auf Microsoft Azure](http://msdn.microsoft.com/library/azure/dn232347.aspx)  
[BizTalk-Dienste: Editionen Diagramm](biztalk-editions-feature-chart.md)  
[Erstellen einer BizTalk-Service mit klassischen Azure-portal](biztalk-provision-services.md)  
[Der BizTalk-Dienste: Registerkarten Dashboard, Monitor und skalieren](biztalk-dashboard-monitor-scale-tabs.md)


[HybridConnectionTab]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionManageConn.png 
