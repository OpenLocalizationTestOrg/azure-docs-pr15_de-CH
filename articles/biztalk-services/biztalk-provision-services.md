<properties
    pageTitle="Azure BizTalk-Dienste in Azure-Portal erstellen | Microsoft Azure"
    description="Informationen Sie zum Bereitstellen oder Azure BizTalk-Dienste in Azure-Portal erstellen; MAK, WABS"
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
    ms.topic="hero-article"
    ms.date="08/15/2016"
    ms.author="mandia"/>



# <a name="create-biztalk-services-using-the-azure-portal"></a>Erstellen Sie BizTalk-Dienste über das Azure-portal

Erstellen Sie Azure BizTalk-Dienste im Azure-Portal.

> [AZURE.TIP] Azure-Portal anmelden, benötigen Sie ein Azure-Konto und Azure-Abonnement. Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. [Azure Testversion](http://go.microsoft.com/fwlink/p/?LinkID=239738)anzeigen

## <a name="create-a-biztalk-service"></a>Erstellen eines BizTalk-Dienstes
Je nach gewählten Edition möglicherweise nicht alle Einstellungen BizTalk Service verfügbar.

1. Mit der [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=213885)anmelden.
2. Wählen Sie im Navigationsbereich unten **neu**:  
![Wählen Sie die Schaltfläche neu][NEWButton]

3. Wählen Sie **Anwendungsdienste** > **BIZTALK-Dienst** > **benutzerdefinierte erstellen**:  
![Wählen Sie BizTalk Service und benutzerdefinierte][NewBizTalkService]

4. Geben Sie die BizTalk Service:

    <table border="1">
    <tr>
    <td><strong>BizTalk-Dienstname</strong></td>
    <td>Geben Sie einen beliebigen Namen können spezifisch sein. Einige Beispiele:<br/><br/>
    <em>MyCompany</em>. biztalk.windows.net<br/>
    <em>Mycompanymyapplication</em>. biztalk.windows.net<br/>
    <em>MyApplication</em>. biztalk.windows.net<br/><br/>". biztalk.windows.net" eingegebenen Namen automatisch hinzugefügt. Dadurch kann eine URL wie auf Ihrem BizTalk Service <strong>https://<em>Myapplication</em>. biztalk.windows.net</strong>.
    </td>
    </tr>
    <tr>
    <td><strong>Edition</strong></td>
    <td>Wenn Sie in der Tests und Entwicklung, wählen Sie <strong>Entwickler</strong>. Werden in der Produktion verwenden die <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302279">der BizTalk-Dienste: Editionen Diagramm</a> , ob <strong>Premium</strong>, <strong>Standard</strong>oder <strong>Basic</strong> die richtige Wahl für Ihr Geschäftsszenario ist.
    </td>
    </tr>
    <tr>
    <td><strong>Region</strong></td>
    <td>Wählen Sie das geografische Gebiet BizTalk Service gehostet.</td>
    </tr>
    <tr>
    <td><strong>Domänen-URL</strong></td>
    <td><strong>Optional</strong>. Domänen-URL ist <em>YourBizTalkServiceName</em>. biztalk.windows.net. Eine benutzerdefinierte Domäne kann auch eingegeben werden. Beispielsweise ist die Domäne <em>Contoso</em>, können Sie eingeben: <br/><br/>
    <em>MyCompany</em>. contoso.com<br/>
    <em>MyCompanyMyApplication</em>. contoso.com<br/>
    <em>MyApplication</em>. contoso.com<br/>
    <em>YourBizTalkServiceName</em>. contoso.com<br/>
    </td>
    </tr>
    </table>
Wählen Sie weiter.

5. Geben Sie den Speicher und Einstellungen:

    <table border="1">
    <tr>
    <td><strong>Überwachung/Archivierung Speicherkonto</strong></td>
    <td>Wählen Sie ein Storage-Konto oder erstellen Sie ein neues Speicherkonto. <br/><br/>Beim Erstellen eines neuen Speicherkontos Geben Sie den <strong>Kontonamen Speicher</strong>.</td>
    </tr>
    <tr>
    <td><strong>Überwachungsdatenbank</strong></td>
    <td>Wenn Sie eine vorhandene Azure SQL-Datenbank verwenden, kann eine andere BizTalk-Service verwendet werden. Sie benötigen den Benutzernamen und Kennwort bei Azure SQL-Datenbankserver erstellt wurde.<br/><br/><strong>Tipp</strong> Erstellen Sie die Überwachungsdatenbank und Monitoring-Archivierung Speicherkonto in derselben Region als BizTalk-Service.</td>
    </tr>
    </table>
Wählen Sie weiter.

6. Geben Sie die Datenbank:

    <table border="1">
    <tr>
    <td><strong>Name</strong></td>
    <td>Verfügbar, wenn das <strong>Erstellen einer neuen SQL-Datenbankinstanz</strong> im vorherigen Bildschirm aktiviert ist.
    <br/><br/>
   Geben Sie eine SQL-Datenbank von BizTalk Service verwendet werden.</td>
    </tr>
    <tr>
    <td><strong>Server</strong></td>
    <td>Verfügbar, wenn das <strong>Erstellen einer neuen SQL-Datenbankinstanz</strong> im vorherigen Bildschirm aktiviert ist.
    <br/><br/>
   Wählen Sie eine vorhandene SQL-Datenbankserver oder erstellen Sie einen neuen SQL-Datenbankserver.</td>
    </tr>
    <tr>
    <td><strong>Server-Benutzername</strong></td>
    <td>Geben Sie den Benutzernamen ein.</td>
    </tr>
    <tr>
    <td><strong>Server-Kennwort</strong></td>
    <td>Geben Sie das Kennwort ein.</td>
    </tr>
    <tr>
    <td><strong>Region</strong></td>
    <td>Verfügbar, wenn <strong>eine neue SQL-Datenbankinstanz erstellen</strong> ausgewählt ist. Wählen Sie das geografische Gebiet der SQL-Datenbank hosten.</td>
    </tr>
    </table>

Wählen Sie das Häkchen, um den Assistenten abzuschließen. Der Fortschritt wird angezeigt:  
![Statusanzeige zeigt nach Abschluss][ProgressComplete]

Nach Abschluss des Vorgangs wird Azure BizTalk-Dienst erstellt und für die Anwendung bereit. Die Standardeinstellungen sind ausreichend. Wenn Sie die Standardeinstellungen ändern möchten, wählen Sie im linken Navigationsbereich **Der BIZTALK-Dienste** und wählen Sie die BizTalk Service. Berechtigungen werden im [Dashboard, Monitor und Skalierung Registerkarten](biztalk-dashboard-monitor-scale-tabs.md) oben angezeigt.

Je nach Zustand des BizTalk Service werden einige Vorgänge abgeschlossen werden können. Eine Liste dieser Vorgänge finden Sie unter [BizTalk-Dienste Zustandsdiagramms](biztalk-service-state-chart.md).


## <a name="post-provisioning-steps"></a>Schritte nach der Bereitstellung

-  [Installieren Sie das Zertifikat auf einem lokalen computer](#InstallCert)
-  [Hinzufügen eines Zertifikats Produktion](#AddCert)
-  [Access Control Namespace abrufen](#ACS)

#### <a name="InstallCert"></a>Installieren Sie das Zertifikat auf einem lokalen computer
Als Teil des BizTalk Service-Bereitstellung ein selbstsigniertes Zertifikat erstellt und Ihrem BizTalk Service-Abonnement zugeordnet. Sie müssen dieses Zertifikat herunterladen und auf Computern entweder BizTalk Service Applications bereitstellen oder Senden von Nachrichten an einen Endpunkt BizTalk Service installieren.

1. Mit der [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=213885)anmelden.
2. Wählen Sie im linken Navigationsbereich **Der BIZTALK-Dienste** , und wählen Sie Ihr Abonnement BizTalk Service.
3. Wählen Sie die Registerkarte **Dashboard** .
4. Wählen Sie **SSL-Zertifikat downloaden**:  
![SSL-Zertifikat ändern][QuickGlance]
5. Doppelklicken Sie auf das Zertifikat, und führen Sie den Assistenten installieren. Stellen Sie installieren Sie das Zertifikat unter dem Speicher für **Vertrauenswürdige Stammzertifizierungsstellen** .

#### <a name="AddCert"></a>Hinzufügen eines Zertifikats Produktion
Das selbstsignierte Zertifikat, das beim Erstellen der BizTalk-Dienste nur in Entwicklung soll automatisch erstellt. Für Produktionsszenarien Produktion Zertifikat ersetzen.

1. Wählen Sie auf der Registerkarte **Dashboard** **Update SSL-Zertifikat**.
2. Nach privaten SSL-Zertifikat (*CertificateName*PFX-Datei), das Ihre BizTalk Service-Namen enthält, geben Sie das Kennwort ein und klicken Sie auf das Häkchen.

#### <a name="ACS"></a>Access Control Namespace abrufen

1. Mit der [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=213885)anmelden.
2. Wählen Sie im linken Navigationsbereich **Der BIZTALK-Dienste** , und wählen Sie BizTalk Service.
3. Wählen Sie in der Taskleiste **Verbindungsinformationen**ein:  
![Verbindungsinformationen auswählen][ACSConnectInfo]

4. Kopieren Sie die Zugriffskontrolle Werte.

Beim Bereitstellen eines BizTalk Service-Projekts in Visual Studio Geben Sie diesem Namespace Zugriffskontrolle. Access Control Namespace wird automatisch für die BizTalk-Service erstellt.

Access Control-Werte können mit jeder Anwendung verwendet werden. Wenn Azure BizTalk-Dienste erstellt wird, steuert dieser Namespace Zugriffskontrolle Authentifizierung mit der BizTalk-Service-Bereitstellung. Wenn Sie das Abonnement ändern oder Verwalten der Namespace wählen Sie **ACTIVE DIRECTORY** im linken Navigationsbereich und dann Ihren Namespace. Die Task-Leiste sind die Optionen aufgeführt.

**Verwalten** , öffnet Access Control-Verwaltungsportal. Access Control-Verwaltungsportal verwendet BizTalk Service **Dienstidentitäten**:  
![ACS-Dienst Identitäten Zugriff steuern Verwaltungsportal][ACSServiceIdentities]

Access Control Service Identität ist ein Satz von Anmeldeinformationen, mit denen Clientanwendungen oder Clients direkt mit Access Control authentifizieren und einen Token empfangen.

> [AZURE.IMPORTANT]BizTalk Service verwendet **Besitzer** für die Standardidentität Service und **den Wert** . Wenn der Kennwortwert symmetrischen Schlüssel Wert der folgende Fehler auftreten anstelle.<br/><br/>*Access Control Management-Dienstkonto mit den angegebenen Anmeldeinformationen konnte keine Verbindung hergestellt werden.*

[Verwalten der ACS Namespace](https://msdn.microsoft.com/library/azure/hh674478.aspx) enthält einige Richtlinien und Ratschlägen.

## <a name="requirements-explained"></a>Anforderungen erläutert

Diese Vorschriften gelten nicht für die kostenlose Version.
<table border="1">
<tr bgcolor="FAF9F9">
        <td><strong>Was Sie benötigen</strong></td>
        <td><strong>Warum benötigen</strong></td>
</tr>
<tr>
<td>Azure-Abonnement</td>
<td>Das Abonnement bestimmt der Azure-Portal anmelden können. Kontoinhaber erstellt das Abonnement bei <a HREF="https://account.windowsazure.com/Subscriptions">Azure-Abonnements</a>.
<br/><br/>
Azure-Konto kann mehrere Abonnements und kann von jedem Benutzer darf verwaltet werden. Z. B. Azure Kontoinhaber ein Abonnement mit dem Namen <em>BizTalkServiceSubscription</em> erstellt und können BizTalk-Administratoren in Ihrem Unternehmen (z. B. ContosoBTSAdmins@live.com) Zugriff auf das Abonnement. In diesem Szenario wird BizTalk-Administratoren Azure-Portal anmelden und vollständige Administratorrechte für die gehosteten Dienste im Abonnement einschließlich Azure BizTalk-Dienste haben. BizTalk-Administratoren sind nicht Azure Kontoinhabern und daher keinen Zugriff auf alle Rechnungsinformationen.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=267577">Abonnements verwalten und Speicherkonten in Azure-Portal</a> bietet weitere Informationen.
</td>
</tr>
<tr>
<td>SQL Azure-Datenbank</td>
<td>Tabellen, Sichten und gespeicherte Prozeduren von BizTalk Service, einschließlich die Tracking-Daten speichert.
<br/><br/>
Beim Erstellen einer BizTalk Service können mithilfe einer vorhandenen Azure SQL Server-Datenbank SQL Azure oder automatisch einen neuen Server oder Datenbank erstellen.
<br/><br/>
Die SQL-Datenbank-Skala wird automatisch konfiguriert. Normalerweise ist die Standardskalierung für BizTalk Service. Ändern die Skalierung wirkt sich auf Preise. Siehe <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=234930">Konten und Abrechnung in Azure SQL-Datenbank</a>
<br/><br/>
<strong>Notizen</strong>
<br/>
<ul>
<li> Beim Erstellen einer neuen Azure SQL Server und Datenbank wird Azure Services automatisch aktiviert. BizTalk Service erfordert Azure Services aktiviert werden.</li>
<li>Erstellen einer neuen Azure SQL-Datenbank auf einer vorhandenen Azure SQL Server werden die Firewall-Regeln des Servers nicht geändert. Daher ist es möglich, andere Azure-Dienste dürfen Zugriff auf die Datenbanken des Servers.</li>
</ul>
</td>
</tr>
<tr>
<td>Azure Access Control namespace</td>
<td>Mit Azure BizTalk-Diensten authentifiziert. Beim Bereitstellen eines BizTalk Service-Projekts in Visual Studio Geben Sie diesem Namespace Zugriffskontrolle. Beim Erstellen einer BizTalk Service wird der Zugriffskontrolle Namespace automatisch erstellt.</td>
</tr>

<tr>
<td>Azure Storage-Konto</td>
<td>Ermöglicht den Zugriff auf Tabellen, Blobs und Warteschlangen in BizTalk Service Folgendes speichern:

<ul>
<li>Protokolldateien, die BizTalk Service zu überwachen. Die Überwachung Ausgabe wird auch auf der Registerkarte **Überwachung** in Azure-Portal angezeigt.</li>
<li>Beim Erstellen einer X12 oder AS2-Vereinbarung zwischen den Partnern können Sie Archivierung Feature Eigenschaften speichern aktivieren. Diese Daten werden im Speicherkonto gespeichert.</li>
</ul>
<br/>
Beim Erstellen einer BizTalk Service können Sie vorhandenes Speicherkonto verwenden oder ein neues Speicherkonto automatisch erstellen.
<br/><br/>
Die Speicher sind ausreichend für einen BizTalk-Service.
<br/><br/>
Ein Speicherkonto erstellen, werden automatisch ein Primärschlüssel und Sekundärschlüssel erstellt. Diese Schlüssel steuern den Zugriff auf das Speicherkonto. Der BizTalk-Service verwendet automatisch den Primärschlüssel.
<br/><br/>
Weitere Informationen finden Sie im <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285671">Speicher</a> .
</td>
</tr>

<tr>
<td>Privaten SSL-Zertifikat</td>
<td>
Beim Erstellen einer Azure BizTalk-Dienst ist auch ein HTTPS-URL mit Ihrem BizTalk Service Name erstellt. Diese URL wird automatisch konfiguriert ein selbstsigniertes Zertifikat Entwicklungsoptionen. Für Produktion benötigen Sie ein privates SSL-Zertifikat.
<br/><br/>
<strong>Wichtig SSL-Zertifikatsinformationen</strong>

<ul>
<li>Ablaufdatum des Zertifikats muss 5 Jahre.</li>
<li>Alle privaten Zertifikate ist ein Kennwort erforderlich. Kennen Sie dieses Kennwort, und teilen Sie empfohlen, dieses Kennwort mit Ihren Administratoren.</li>
<li>Selbstsignierte Zertifikate werden in einer Umgebung mit Test-Entwicklung verwendet. Verwendung selbstsignierter Zertifikate importieren Sie das Zertifikat den Zertifikatspeicher vertrauenswürdiger Stammzertifizierungsstellen zu Ihrem persönlichen Zertifikatspeicher.</li>
</ul>
<br/>Wenn die Produktion Anforderung an die Zertifizierungsstelle senden, geben Sie folgenden Zertifikateigenschaften:
<br/>

<ul>
<li><strong>Erweiterte Schlüsselverwendung</strong>: mindestens Azure BizTalk Services erfordert Authentifizierung.</li>
<li><strong>Allgemeiner Name</strong>: Geben Sie den vollqualifizierten Domänennamen (FQDN) der Azure BizTalk Service URL. Siehe <a HREF="#BizTalk">Erstellen eines BizTalk-Dienstes</a> in diesem Artikel.</li>
</ul>
<br/>
Ein neues oder anderes Zertifikat kann hinzugefügt werden, nachdem BizTalk Service erstellt wurde.
</td>
</tr>
</table>



## <a name="hybrid-connections"></a>Hybridverbindungen

Beim Erstellen einer Azure BizTalk-Dienst steht die Registerkarte **Hybrid-Verbindung** :

![Registerkarte Verbindung Hybrid][HybridConnectionTab]

Hybridverbindungen wird ein Azure-Website oder Azure mobile Service mit lokalen Ressourcen verbinden, die einen statischen TCP-Port wie SQL Server, MySQL, HTTP-Web-APIs, Mobile Dienste und die meisten benutzerdefinierte Webdienste verwendet.  Hybridverbindungen und BizTalk-Adapter-Dienst unterscheiden. BizTalk Adapter Service zum Azure BizTalk-Dienste einen lokalen Zeile Geschäftsbereiche (LOBS) System herstellen.

 [Hybrid-Verbindungen](integration-hybrid-connection-overview.md) , einschließlich der Erstellung und Verwaltung Hybrid mehr anzeigen


## <a name="next-steps"></a>Nächste Schritte

BizTalk Service erstellt, Kennenlernen der unterschiedlichen [der BizTalk-Dienste: Registerkarten Dashboard, Monitor und Skalierung](biztalk-dashboard-monitor-scale-tabs.md). BizTalk Service ist für die Anwendung bereit. Gehen Sie starten erstellen ASP.NET-Webanwendungen [Azure BizTalk-Dienste](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="see-also"></a>Siehe auch
- [BizTalk-Dienste: Editionen Diagramm](biztalk-editions-feature-chart.md)<br/>
- [Der BizTalk-Dienste: Status Diagramm](biztalk-service-state-chart.md)<br/>
- [Der BizTalk-Dienste: Sicherung und Wiederherstellung](biztalk-backup-restore.md)<br/>
- [BizTalk-Dienste: Drosselung](biztalk-throttling-thresholds.md)<br/>
- [BizTalk-Dienste: Ausstellername und Aussteller Schlüssel](biztalk-issuer-name-issuer-key.md)<br/>
- [Wie kann ich starten mithilfe von Azure BizTalk-Dienste SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
- [Hybridverbindungen](integration-hybrid-connection-overview.md)

[NewBizTalkService]: ./media/biztalk-provision-services/WABS_NewBizTalkService.png
[NEWButton]: ./media/biztalk-provision-services/WABS_New.png
[ProgressComplete]: ./media/biztalk-provision-services/WABS_ProgressComplete.png
[ACSConnectInfo]: ./media/biztalk-provision-services/WABS_ACSConnectInformation.png
[QuickGlance]: ./media/biztalk-provision-services/WABS_QuickGlance.png
[ACSServiceIdentities]: ./media/biztalk-provision-services/WABS_ACSServiceIdentities.png
[HybridConnectionTab]: ./media/biztalk-provision-services/WABS_HybridConnectionTab.png
