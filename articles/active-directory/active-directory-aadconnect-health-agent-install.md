<properties
    pageTitle="Azure AD verbinden Gesundheit Agenteninstallation | Microsoft Azure"
    description="Dies ist der Azure AD Connect Health-Seite, die Agentinstallation für AD FS und Sync beschreibt."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>


# <a name="azure-ad-connect-health-agent-installation"></a>Azure AD Connect Health Agent-Installation

Dieses Dokument führt Sie durch die Installation und Konfiguration von Azure AD Connect Health Agents. [Hier](active-directory-aadconnect-health.md#download-and-install-azure-ad-connect-health-agent)können Sie die Agenten herunterladen.

##  <a name="requirements"></a>Vorschriften
In der folgenden Tabelle wird eine Liste der für die Verwendung von Azure AD Connect Health.

| Anforderung | Beschreibung|
| ----------- | ---------- |
|Azure AD Premium| Azure AD Connect Health ist eine Azure AD Premium und Azure AD Premium erfordert. </br></br>Weitere Informationen finden Sie unter [Erste Schritte mit Azure AD Premium](active-directory-get-started-premium.md) </br>Starten Sie eine kostenlose 30-tägige Testversion finden Sie unter [testen.](https://azure.microsoft.com/trial/get-started-active-directory/)|
|Sie müssen ein globaler Administrator Ihrer Azure Anzeige Einstieg in Azure AD Connect Health sein.|Standardmäßig können nur globalen Administratoren installieren und konfigurieren die Health Agents beginnen, auf das Portal zugreifen und Operationen in Azure AD Connect Health. Weitere Informationen finden Sie unter [Verwalten von Azure AD-Verzeichnis](active-directory-administer.md). <br><br> Mit Rolle Based Access Control können Sie für Azure AD verbinden andere Benutzer in Ihrer Organisation zugreifen. Weitere Informationen finden Sie unter [Rolle basierte Zugriffskontrolle für Azure AD Connect Health.](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control) </br></br>**Wichtig:** Das Konto bei der Installation der Agents Arbeits- oder Schulcomputer Konto. Es darf kein Microsoft-Konto sein. Weitere Informationen finden Sie unter [für Azure Organisation anmelden](sign-up-organization.md)
|Der Azure AD Connect-Agent wird auf jedem Zielserver installiert.| Azure AD Connect Health erfordert eine Agenteninstallation auf Zielservern Daten bereitstellen, die im Portal angezeigt wird. </br></br>Um Daten aus Ihrer lokalen AD FS-Infrastruktur, muss der Agent beispielsweise auf dem AD FS, AD FS-Proxy und Web Application Proxy Server installiert. Entsprechend der lokalen Daten zu AD DS-Infrastruktur der Agent muss auf den Domänencontrollern installiert werden. </br></br>**Wichtig:** Das Konto bei der Installation der Agents Arbeits- oder Schulcomputer Konto. Es darf kein Microsoft-Konto sein. Weitere Informationen finden Sie unter [für Azure Organisation anmelden](sign-up-organization.md)|
|Ausgehende Verbindung mit der Azure-Endpunkte|Während der Installation und Runtime benötigt der Agent Verbindung zu Azure AD Connect Health-Endpunkte. Ausgehende Verbindung blockiert ist, sicherzustellen Sie, dass die folgenden Endpunkte der zulässigen Liste hinzugefügt werden: </br></br><li>& #42;. BLOB.Core.Windows.NET </li><li>& #42;. Queue.Core.Windows.NET</li><li>adhsprodwus.Servicebus.Windows.NET - Anschluss: 5671 </li><li>https://Management.Azure.com </li><li>https://s1.adhybridhealth.Azure.com/</li><li>https://policykeyservice.DC.AD.msft.NET/</li><li>https://Login.Windows.NET</li><li>https://Login.microsoftonline.com</li><li>https://Secure.aadcdn.microsoftonline-p.com</li> |
|Firewall-Ports auf dem Server den Agenten.| Der Agent muss die folgenden Firewallports für den Agent Kommunikation mit Azure AD Health Dienstendpunkte geöffnet werden.</br></br><li>TCP/UDP-Port 443</li><li>TCP/UDP-Port 5671</li>
|Lassen Sie die folgenden Websites zu, wenn IE verstärkte Sicherheit aktiviert| Wenn IE verstärkte Sicherheit aktiviert ist, müssen folgende Websites auf dem Server zugelassen werden, die der Agent installiert wird.</br></br><li>https://Login.microsoftonline.com</li><li>https://Secure.aadcdn.microsoftonline-p.com</li><li>https://Login.Windows.NET</li><li>Der Verbundserver für Ihre Organisation Azure Active Directory vertraut. Beispiel: https://sts.contoso.com</li>




## <a name="installing-the-azure-ad-connect-health-agent-for-ad-fs"></a>Installieren von Azure AD Systemintegritäts-Agent für AD FS verbinden
Um die Agenteninstallation zu starten, doppelklicken Sie auf die .exe-Datei, die Sie heruntergeladen haben. Klicken Sie im ersten Bildschirm auf installieren.

![Überprüfen Sie Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install1.png)

Nachdem die Installation abgeschlossen ist, klicken Sie auf konfigurieren.

![Überprüfen Sie Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install2.png)

Befehlszeile wird gestartet, gefolgt von einigen PowerShell, die Register-AzureADConnectHealthADFSAgent ausgeführt wird. Wenn aufgefordert, Azure, weiter und anmelden.

![Überprüfen Sie Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install3.png)


Nach der Anmeldung weiterhin PowerShell. Nach Abschluss können Sie PowerShell schließen und die Konfiguration ist abgeschlossen.

An diesem Punkt sollte automatisch zum Überwachen und Erfassen von Daten kann so die Dienste gestartet werden. Wenn Sie alle erforderlichen Komponenten, die in den vorherigen Abschnitten beschriebenen nicht erfüllt, werden Warnungen im PowerShell-Fenster angezeigt. Achten Sie darauf, dass [Vorschriften](active-directory-aadconnect-health-agent-install.md#requirements) abgeschlossen ist, bevor die Installation des Agenten. Der folgende Screenshot zeigt diese Fehler.

![Überprüfen Sie Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install4.png)

Um sicherzustellen, dass der Agent installiert wurde, suchen Sie die folgenden Dienste auf dem Server. Wenn Sie die Konfiguration abgeschlossen haben, sollten sie bereits ausgeführt. Andernfalls werden sie angehalten, bis die Konfiguration abgeschlossen ist.

- Azure AD Connect Health AD FS-Diagnosedienst
- Azure AD Connect Gesundheitswesen AD FS Einblicke
- Azure AD Connect Health AD FS Überwachungsdienst

![Überprüfen Sie Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install5.png)


### <a name="agent-installation-on-windows-server-2008-r2-servers"></a>Agenteninstallation auf Windows Server 2008 R2-Server

Schritte für Windows Server 2008 R2-Server:

1. Stellen Sie sicher, dass der Server Service Pack 1 oder höher ausgeführt wird.
1. Für IE Agentinstallation deaktivieren:
1. Installieren Sie Windows PowerShell 4.0 auf jedem Server vor den AD-Agent installieren. So installieren Sie Windows PowerShell 4.0:
 - Installieren Sie [Microsoft.NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=40779) über den folgenden Link offline Installer herunterzuladen.
 - Installieren Sie PowerShell ISE (von Windows-Funktionen)
 - Installieren der [Windows Management Framework 4.0.](https://www.microsoft.com/download/details.aspx?id=40855)
 - Installieren Sie Internet Explorer, Version 10 oder höher auf dem Server. (Vorgeschriebenen Gesundheitswesen mit Ihren Azure-Admin-Anmeldeinformationen authentifiziert.)
1. Weitere Informationen zum Installieren von Windows PowerShell 4.0 auf Windows Server 2008 R2 finden Sie im Wiki-Artikel [hier](http://social.technet.microsoft.com/wiki/contents/articles/20623.step-by-step-upgrading-the-powershell-version-4-on-2008-r2.aspx).

### <a name="enable-auditing-for-ad-fs"></a>Aktivieren der Überwachung von AD FS

> [AZURE.NOTE] Dieser Abschnitt gilt nur für ADFS-Verbundserver.

Damit die Verwendungsanalyse Feature sammeln und analysieren Daten benötigt Azure AD Connect Health Agent Informationen in den Überwachungsprotokollen AD FS. Diese Protokolle sind standardmäßig nicht aktiviert. Gehen Sie AD FS Überwachung aktivieren und AD FS-Überwachungsprotokolle auf den AD FS-Servern zu suchen.

#### <a name="to-enable-auditing-for-ad-fs-20"></a>Aktivieren der Überwachung für AD FS 2.0

1. Klicken Sie auf **Start**, zeigen Sie auf **Programme**, zeigen Sie auf **Verwaltung**und klicken Sie dann auf **Lokale Sicherheitsrichtlinie**.
2. Navigieren Sie zum Ordner **Sicherheit Einstellungen\Sicherheitseinstellungen\Lokale von Rights Management** und anschließend auf Sicherheitsaudits generieren.
3. Überprüfen Sie auf der Registerkarte **Lokale Sicherheitsrichtlinie** AD FS 2.0-Dienstkonto aufgeführt. **Wenn es nicht vorhanden ist, klicken Sie auf **Benutzer oder Gruppe hinzufügen** ein und zur Liste hinzufügen.**
4. Überwachung öffnen Sie ein Eingabeaufforderungsfenster mit erhöhten Rechten, und führen Sie den folgenden Befehl:<code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable</code>
5. Schließen Sie lokale Sicherheitsrichtlinie und öffnen Sie die Management-Snap-in. Um Management-Snap-in öffnen, klicken Sie auf **Start**, zeigen Sie auf **Programme**, **Verwaltung**und klicken Sie auf AD FS 2.0 Verwaltung.
6. Klicken Sie im Aktionsbereich auf Federation Service-Eigenschaften bearbeiten.
7. Klicken Sie im Dialogfeld **Eigenschaften von Föderation** auf die Registerkarte **Ereignisse** .
8. Aktivieren Sie die Kontrollkästchen **Erfolg überwacht** und **Fehler überwacht** .
9. Klicken Sie auf **OK**.

#### <a name="to-enable-auditing-for-ad-fs-on-windows-server-2012-r2"></a>Aktivieren der Überwachung für AD FS unter Windows Server 2012 R2

1. Öffnen Sie **Lokale Sicherheitsrichtlinie** öffnen Sie **Server-Manager** auf dem Startbildschirm oder Server-Manager auf der Taskleiste auf dem Desktop und dann auf **Systemprogramme/Lokale Sicherheitsrichtlinie**.
2. Navigieren Sie zum Ordner **Sicherheit Richtlinien\Zuweisen von Benutzerrechten** , und doppelklicken Sie **Sicherheitsaudits generieren**.
3. Überprüfen Sie auf der Registerkarte **Lokale Sicherheitsrichtlinie** AD FS-Dienstkonto aufgeführt. **Wenn es nicht vorhanden ist, klicken Sie auf **Benutzer oder Gruppe hinzufügen** ein und zur Liste hinzufügen.**
4. Überwachung öffnen Sie ein Eingabeaufforderungsfenster mit erhöhten Rechten, und führen Sie den folgenden Befehl:<code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable.</code>
5. **Lokale Sicherheitsrichtlinie**schließen und öffnen Sie das Snap-in **AD FS-Verwaltung** (im Server-Manager klicken Sie auf Extras, und wählen Sie AD FS-Verwaltung).
6. Klicken Sie im Aktionsbereich auf **Federation Service-Eigenschaften bearbeiten**.
7. Klicken Sie im Dialogfeld Eigenschaften von Föderation auf die Registerkarte **Ereignisse** .
8. Kontrollkästchen Sie **Erfolg überwacht und Fehler überwacht** , und klicken Sie dann auf **OK**.






#### <a name="to-locate-the-ad-fs-audit-logs"></a>Der AD FS-Überwachung zu protokolliert


1. Öffnen Sie die **Ereignisanzeige**.
2. Wechseln Sie zu Windows-Protokolle und wählen Sie **Sicherheit**.
3. Klicken Sie auf der rechten Seite auf **Aktuelle Filter-Protokolle**.
4. Wählen Sie unter Ereignisquelle **AD FS überwachen**.

![AD FS-Überwachungsprotokolle](./media/active-directory-aadconnect-health-requirements/adfsaudit.png)

> [AZURE.WARNING] Existiert eine Gruppenrichtlinie, die AD FS-Überwachung deaktivieren, kann der Azure AD verbinden Agent Informationen nicht. Stellen Sie sicher, dass Sie eine Gruppenrichtlinie nicht, die Überwachung deaktivieren können.

[//]: # (Start of Agent Proxy Configuration Section)

## <a name="installing-the-azure-ad-connect-health-agent-for-sync"></a>Installieren des Agenten Azure AD Connect Health für Synchronisierung
Azure AD Connect Health Agent zur Synchronisierung wird im letzten Build Azure AD Connect automatisch installiert. Um Azure AD Connect für die Synchronisierung verwenden, müssen Sie die neueste Version von Azure AD Connect herunterladen und installieren. Sie können die neueste Version [hier](http://www.microsoft.com/download/details.aspx?id=47594).

Um sicherzustellen, dass der Agent installiert wurde, suchen Sie die folgenden Dienste auf dem Server. Wenn Sie die Konfiguration abgeschlossen haben, sollten sie bereits ausgeführt. Andernfalls werden sie angehalten, bis die Konfiguration abgeschlossen ist.

- Azure AD Connect Status Einblicke Synchronisierungsdienst
- Azure AD Connect Health Sync Überwachungsdienst

![Überprüfen Sie ob Azure AD Connect Health für Synchronisierung](./media/active-directory-aadconnect-health-sync/services.png)

> [AZURE.NOTE] Beachten Sie, dass Azure AD Connect Health Azure AD Premium muss. Wenn Sie keinen Azure AD Premium können Sie die Konfiguration im Azure-Portal. Weitere Informationen finden Sie auf der [Seite](active-directory-aadconnect-health-agent-install.md#requirements).


## <a name="manual-azure-ad-connect-health-for-sync-registration"></a>Manuelle Azure AD Connect Health Sync-Registrierung
Fällt der Azure AD Connect Health Sync-Agent Registrierung nach der erfolgreichen Installation Azure AD verbinden, können Sie den folgenden PowerShell-Befehl, den Agent manuell registrieren.

>[AZURE.IMPORTANT] Mit dem folgenden PowerShell-Befehl ist nur erforderlich, wenn die Agent-Registrierung fehlschlägt, nach der Installation von Azure AD verbinden.

Die folgende Befehl ist PowerShell erforderlich nur wenn Health Agent Registrierung auch nach einer erfolgreichen Installation und Konfiguration von Azure AD Connect nicht. Azure AD Connect Health Services werden gestartet, nachdem der Agent erfolgreich registriert wurde.

Azure AD Connect Health Agent für die Synchronisierung mit dem folgenden PowerShell-Befehl können manuell registrieren:

`Register-AzureADConnectHealthSyncAgent -AttributeFiltering $false -StagingMode $false`

Der Befehl verwendet folgende Parameter:

- AttributeFiltering: $true (Standard) – Wenn Azure AD Connect nicht das Standardattribut Synchronisierung festgelegt und gefilterte Attribut Verwendung angepasst wurde. andernfalls $false.
- StagingMode: $false (Standard) – Wenn Azure AD Connect-Server nicht im Modus $true bereitstellen, wenn der Server in der Stagingdatenbank Modus konfiguriert ist.

Aufforderung zur Authentifizierung verwenden Sie dieselbe globale Administratorkonto (z. B. admin@domain.onmicrosoft.com) für die Konfiguration von Azure AD Connect verwendet wurde.

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-ds"></a>Installieren von Azure AD Systemintegritäts-Agent für AD DS verbinden
Um die Agenteninstallation zu starten, doppelklicken Sie auf die .exe-Datei, die Sie heruntergeladen haben. Klicken Sie im ersten Bildschirm auf installieren.

![Überprüfen Sie Azure AD Connect Health](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install1.png)

Nachdem die Installation abgeschlossen ist, klicken Sie auf konfigurieren.

![Überprüfen Sie Azure AD Connect Health](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install2.png)

Befehlszeile wird gestartet, gefolgt von einigen PowerShell, die Register-AzureADConnectHealthADDSAgent ausgeführt wird. Wenn aufgefordert, Azure, weiter und anmelden.

![Überprüfen Sie Azure AD Connect Health](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install3.png)

Nach der Anmeldung weiterhin PowerShell. Nach Abschluss können Sie PowerShell schließen und die Konfiguration ist abgeschlossen.

An diesem Punkt sollte automatisch zum Überwachen und Erfassen von Daten kann so die Dienste gestartet werden. Wenn Sie alle erforderlichen Komponenten, die in den vorherigen Abschnitten beschriebenen nicht erfüllt, werden Warnungen im PowerShell-Fenster angezeigt. Achten Sie darauf, dass [Vorschriften](active-directory-aadconnect-health-agent-install.md#requirements) abgeschlossen ist, bevor die Installation des Agenten. Der folgende Screenshot zeigt diese Fehler.

![Überprüfen Sie ob Azure AD Connect Health für AD DS](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install4.png)

Um sicherzustellen, dass der Agent installiert wurde, suchen Sie die folgenden Dienste auf dem Domänencontroller.

- Azure AD Connect Gesundheitswesen AD DS Einblicke
- Azure AD Connect Health AD DS Überwachungsdienst

Wenn Sie die Konfiguration abgeschlossen haben, sollten diese Dienste bereits ausgeführt. Andernfalls werden sie angehalten, bis die Konfiguration abgeschlossen ist.

![Überprüfen Sie Azure AD Connect Health](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install5.png)

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-ds-on-server-core"></a>Installieren von Azure AD Systemintegritäts-Agent für AD DS auf Server Core verbinden.
Nach der Installation der .exe-Datei, können Sie die Registrierung mit dem folgenden PowerShell-Befehl ausführen:

`Register-AzureADConnectHealthADDSAgent -Credential $cred`

## <a name="configure-azure-ad-connect-health-agents-to-use-http-proxy"></a>Konfigurieren von Azure AD verbinden Health Agents HTTP-Proxy verwenden
Sie können Azure AD verbinden Health Agents einen HTTP-Proxy zu konfigurieren.

>[AZURE.NOTE]
- Verwenden von "Netsh WinHttp festgelegt ProxyServerAddress" wird nicht unterstützt, wie der Agent System.Net Webanfragen statt Microsoft Windows HTTP-Dienste zu.
- Die konfigurierte HTTP-Proxy-Adresse wird Pass-Through-verschlüsselten Https-Nachrichten verwendet.
- Authentifizierten Proxies (mit HTTPBasic) werden nicht unterstützt.

### <a name="change-health-agent-proxy-configuration"></a>Health Agent Proxy-Konfiguration ändern
Sie haben die folgenden Optionen zum Azure AD verbinden Health Agent einen HTTP-Proxy zu konfigurieren.

>[AZURE.NOTE]Azure AD verbinden Health Agent-Services müssen in Reihenfolge für die Proxyeinstellungen aktualisiert werden gestartet. Führen Sie den folgenden Befehl ein:<br>
Neustart-Service AdHealth *

#### <a name="import-existing-proxy-settings"></a>Importieren von vorhandenen Proxy Settings

##### <a name="import-from-internet-explorer"></a>Importieren von InternetExplorer
Internet Explorer HTTP-Proxyeinstellungen können importiert werden Azure AD verbinden Health Agents verwendet. Führen Sie auf jedem Server des Agents den folgenden PowerShell-Befehl:

    Set-AzureAdConnectHealthProxySettings -ImportFromInternetSettings

##### <a name="import-from-winhttp"></a>Importieren von WinHTTP
WinHTTP-Proxyeinstellungen können importiert werden Azure AD verbinden Health Agents verwendet. Führen Sie auf jedem Server des Agents den folgenden PowerShell-Befehl:

    Set-AzureAdConnectHealthProxySettings -ImportFromWinHttp

#### <a name="specify-proxy-addresses-manually"></a>Proxy-Adressen manuell festlegen
Sie können manuell einen Proxy-Server auf jedem Server Health Agent von den folgenden PowerShell-Befehl ausführen:

    Set-AzureAdConnectHealthProxySettings -HttpsProxyAddress address:port

Beispiel: *Set-AzureAdConnectHealthProxySettings - HttpsProxyAddress Myproxyserver: 443*

- "Adresse" kann einen auflösbaren DNS-Servernamen oder eine IPv4-Adresse sein.
- "Port" kann weggelassen werden. Wenn nicht angegeben, dann als Standardport 443 ausgewählt ist.

#### <a name="clear-existing-proxy-configuration"></a>Löschen vorhandene Proxykonfiguration
Mit dem folgenden Befehl können Sie die Proxy-Konfiguration deaktivieren:

    Set-AzureAdConnectHealthProxySettings -NoProxy


### <a name="read-current-proxy-settings"></a>Lesen Sie die aktuellen Proxyeinstellungen
Lesen Sie die aktuell konfigurierten Proxyeinstellungen durch Ausführen des folgenden Befehls:

    Get-AzureAdConnectHealthProxySettings


## <a name="test-connectivity-to-azure-ad-connect-health-service"></a>Testen der Konnektivität zu Azure AD Health Service herstellen
Es ist möglich, dass Probleme auftreten, die der Azure AD Connect Health Agent Konnektivität mit dem Azure AD Verbindung verlieren. Dazu gehören Netzwerkprobleme, Berechtigungsprobleme oder verschiedene Gründe.

Der Agent kann nicht zum Senden von Daten an Azure AD Connect Health Service für mehr als zwei Stunden ist, wird dies mit der folgenden Warnung im Portal: "Health-Daten ist nicht aktuell." Sie können bestätigen, das betroffene Azure AD Connect Systemintegritäts-Agent Daten in Azure AD Connect Health Service hochladen, mit dem folgenden PowerShell-Befehl ist:

    Test-AzureADConnectHealthConnectivity -Role ADFS

Der Parameter Rolle hat derzeit die folgenden Werte:

- ADFS
- Synchronisieren
- FÜGT HINZU

Flag - Ausstellungen können im Befehl Sie detaillierte Protokolle anzeigen. Verwenden Sie Folgendes Beispiel:

    Test-AzureADConnectHealthConnectivity -Role Sync -ShowResult

>[AZURE.NOTE]Konnektivitätstool verwenden, müssen Sie zunächst die Registrierung Agent ausführen. Wenn Sie nicht die Agent-Registrierung abschließen können, stellen Sie sicher, dass alle [Vorschriften](active-directory-aadconnect-health-agent-install.md#requirements) für Azure AD Connect Health eingehalten haben. Diese Verbindungstest wird standardmäßig während der Agent-Registrierung ausgeführt.



## <a name="related-links"></a>Verwandte links

* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Azure AD Connect Health-Operationen](active-directory-aadconnect-health-operations.md)
* [Schließen Sie Gesundheit mit Azure AD mit AD FS](active-directory-aadconnect-health-adfs.md)
* [Verwenden von Azure AD Connect Health für Synchronisierung](active-directory-aadconnect-health-sync.md)
* [Gesundheit in AD DS verbinden über Azure AD](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health FAQ](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect Version Gesundheitsdaten](active-directory-aadconnect-health-version-history.md)
