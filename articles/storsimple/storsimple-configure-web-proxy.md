<properties 
   pageTitle="Einrichten von Webproxy ein StorSimple | Microsoft Azure"
   description="Erfahren Sie, wie Windows PowerShell für StorSimple verwenden, um Webproxyeinstellungen für Ihr StorSimple-Gerät konfigurieren."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="configure-web-proxy-for-your-storsimple-device"></a>Webproxy für Ihr StorSimple-Gerät konfigurieren

## <a name="overview"></a>Übersicht

In diesem Lernprogramm beschreibt verwenden Sie Windows PowerShell für StorSimple zu konfigurieren Webproxyeinstellungen für Ihr StorSimple-Gerät. Die Webproxyeinstellungen dienen StorSimple-Gerät bei der Kommunikation mit der Cloud. Ein Webproxyserver zur Sicherheit Filtertext, einfache Bandbreitenbedarf oder sogar zu Analytics-Cache hinzugefügt.

Web-Proxy ist eine optionale Konfiguration für Ihr Gerät StorSimple. Sie können über Windows PowerShell Webproxy für StorSimple konfigurieren. Die Konfiguration ist ein zweistufiger Prozess wie folgt:

1. Konfigurieren Sie Webproxyeinstellungen über den Installationsassistenten oder Windows PowerShell-Cmdlets StorSimple.

2. Anschließend aktivieren Sie die konfigurierte Webproxyeinstellungen über Windows PowerShell-Cmdlets StorSimple.

Nach Abschluss der Web Proxy-Konfiguration die konfigurierten Webproxyeinstellungen Microsoft Azure StorSimple Manager-Dienst und Windows PowerShell für StorSimple angezeigt. 

Nach dem Lesen dieses Lernprogramm können, Sie:

- Konfigurieren Sie Web-Proxy mithilfe von Assistenten und cmdlets
- Aktivieren Sie Web-Proxy mithilfe von cmdlets
- Webproxyeinstellungen im klassischen Azure-Portal anzeigen
- Fehlerbehebung bei Web Proxy-Konfiguration


## <a name="configure-web-proxy-via-windows-powershell-for-storsimple"></a>Konfigurieren von Webproxy über Windows PowerShell für StorSimple

Sie verwenden eine der folgenden Web-Proxyeinstellungen konfigurieren:

- Setup-Assistent Sie durch die Konfigurationsschritte führen.

- Cmdlets in Windows PowerShell für StorSimple.

Jede dieser Methoden wird in den folgenden Abschnitten erläutert.

## <a name="configure-web-proxy-via-the-setup-wizard"></a>Webproxy über den Setup-Assistenten konfigurieren

Der Setup-Assistent können Sie durch die Schritte zum Web-Proxy-Konfiguration führen. Die folgenden Schritte Webproxy auf Ihrem Gerät zu konfigurieren.

#### <a name="to-configure-web-proxy-via-the-setup-wizard"></a>Webproxy über den Setup-Assistenten konfigurieren

1. Im Menü seriellen Konsole Option 1 **mit Vollzugriff anmelden** und das **Administratorkennwort Gerät**. Geben Sie den folgenden Befehl zum Starten einer Sitzung Setup-Assistenten:

    `Invoke-HcsSetupWizard`

2. Ist dies das erste Mal der Setup-Assistent für die Registrierung des Geräts verwendet haben, müssen Sie die erforderliche Einstellung konfigurieren, bis zu die Web Proxy-Konfiguration. Wenn das Gerät bereits registriert ist, können Sie alle konfigurierten Netzwerkeinstellungen übernehmen, bis zu Web-Proxy-Konfiguration. Im Setup-Assistenten zur Web-Proxyeinstellungen konfigurieren, geben Sie **Ja**.

3. **Web-Proxy-URL**Geben Sie die IP-Adresse oder den vollqualifizierten Domänennamen (FQDN) der Web-Proxy-Server und die TCP-Portnummer, die das Gerät mit der Cloud Kommunikation verwendet werden soll. Verwenden Sie das folgende Format:

    `http://<IP address or FQDN of the web proxy server>:<TCP port number>`

    Standardmäßig wird die TCP-Portnummer 8080 angegeben.

4. Wählen Sie **NTLM**, **Basic**oder **keine**Authentifizierung. Ist die am wenigsten sichere Authentifizierung für die Konfiguration des Proxyservers. NT LAN Manager (NTLM) ist ein äußerst sicheres und komplexes Authentifizierungsprotokoll, das verwendet ein drei-Wege-Messagingsystem (gelegentlich vier Wenn Integrität erforderlich ist) zur Authentifizierung eines Benutzers. Die Standardauthentifizierung ist NTLM. Weitere Informationen finden Sie unter [grundlegende](http://hc.apache.org/httpclient-3.x/authentication.html) und [NTLM-Authentifizierung](http://hc.apache.org/httpclient-3.x/authentication.html). 

    > [AZURE.IMPORTANT] **In der StorSimple Manager-Dienst Überwachung Diagramme Geräte funktionieren nicht, wenn grundlegende oder NTLM-Authentifizierung in der Konfiguration des Proxyservers für das Gerät aktiviert ist. Überwachung Diagrammen arbeiten müssen Sie sicherstellen, dass die Authentifizierung auf NONE festgelegt ist.**

5. Wenn Sie Authentifizierung verwenden, geben Sie **Web Proxy-Benutzername** und ein **Web-Proxy-Kennwort**. Sie müssen das Kennwort bestätigen.

    ![Konfigurieren von Webproxy auf StorSimple Gerät1](./media/storsimple-configure-web-proxy/IC751830.png)

Wenn Sie das Gerät zum ersten Mal registrieren, fahren Sie mit der Registrierung. Wenn das Gerät bereits registriert wurde, wird der Assistent beendet. Die Einstellungen werden gespeichert.

Web-Proxy wird jetzt auch aktiviert werden. Sie können [Web-Proxy aktivieren](#enable-web-proxy) überspringen und direkt zum [Webproxyeinstellungen im klassischen Azure-Portal anzeigen](#view-web-proxy-settings-in-the-azure-classic-portal).


## <a name="configure-web-proxy-via-windows-powershell-for-storsimple-cmdlets"></a>Konfigurieren Sie Web-Proxy über Windows PowerShell-Cmdlets StorSimple

Eine alternative Methode zum Konfigurieren von Web Proxy ist über die Windows PowerShell-Cmdlets StorSimple. Die folgenden Schritte Webproxy konfigurieren.

#### <a name="to-configure-web-proxy-via-cmdlets"></a>Konfigurieren von Webproxy über cmdlets

1. Wählen Sie im Menü seriellen Konsole Option 1 **Vollzugriff einloggen**. Wenn aufgefordert, geben Sie das **Administratorkennwort Gerät**. Das Standardkennwort lautet `Password1`.

2. In der Befehlszeile eingeben:

    `Set-HcsWebProxy -Authentication NTLM -ConnectionURI "<http://<IP address or FQDN of web proxy server>:<TCP port number>" -Username "<Username for web proxy server>"`

    Bereitstellen Sie und bestätigen Sie das Kennwort ein, wie unten dargestellt.

    ![Konfigurieren von Webproxy auf StorSimple Device3](./media/storsimple-configure-web-proxy/IC751831.png)

Der Webproxy ist nun konfiguriert und aktiviert werden muss.

## <a name="enable-web-proxy"></a>Web-Proxy aktivieren

Web-Proxy ist standardmäßig deaktiviert. Nach dem Konfigurieren der Webproxyeinstellungen auf dem StorSimple-Gerät müssen Sie Windows PowerShell für StorSimple verwenden, um Web-Proxyeinstellungen aktivieren.

> [AZURE.NOTE] **Dieser Schritt wird nicht erforderlich, wenn Sie den Setup-Assistenten Webproxy konfiguriert. Web-Proxy wird automatisch nach einer Setup-Assistenten standardmäßig aktiviert.**

Führen Sie die folgenden Schritte in Windows PowerShell für StorSimple Webproxy auf Ihrem Gerät aktivieren:

#### <a name="to-enable-web-proxy"></a>Web-Proxy aktivieren

1. Wählen Sie im Menü seriellen Konsole Option 1 **Vollzugriff einloggen**. Wenn aufgefordert, geben Sie das **Administratorkennwort Gerät**. Das Standardkennwort lautet `Password1`.

2. In der Befehlszeile eingeben:

    `Enable-HcsWebProxy`

    Jetzt haben Sie die Web-Proxy-Konfiguration auf dem StorSimple-Gerät aktiviert.

    ![Konfigurieren von Webproxy auf StorSimple gerät4](./media/storsimple-configure-web-proxy/IC751832.png)

## <a name="view-web-proxy-settings-in-the-azure-classic-portal"></a>Webproxyeinstellungen im klassischen Azure-Portal anzeigen

Die Webproxyeinstellungen über die Windows PowerShell-Schnittstelle konfiguriert und können nicht im klassischen Portal geändert werden. Sie können jedoch diese Einstellungen im klassischen Portal anzeigen. Die folgenden Schritte Webproxy anzeigen.

#### <a name="to-view-web-proxy-settings"></a>Webproxyeinstellungen anzeigen
1. Navigieren Sie zu **StorSimple Manager Service > Geräte**. Wählen Sie und klicken Sie auf ein Gerät und dann **Konfigurieren**.
1. Scrollen Sie auf der Seite **Konfigurieren** **Webproxyeinstellungen** Abschnitt. Sie können die konfigurierten Webproxyeinstellungen auf dem StorSimple-Gerät wie folgt anzeigen.

    ![Webproxy im Verwaltungsportal anzeigen](./media/storsimple-configure-web-proxy/ViewWebProxyPortal_M.png)
 
## <a name="errors-during-web-proxy-configuration"></a>Fehler bei Web Proxy-Konfiguration

Die Webproxyeinstellungen falsch konfiguriert, Fehlermeldungen an den Benutzer in Windows PowerShell für StorSimple erscheint. In der folgenden Tabelle werden einige dieser Fehlermeldungen ihre Ursachen und empfohlenen Aktionen erläutert.

|Seriennr.|HRESULT-Fehler Code|Mögliche Ursache|Empfohlene Aktion|
|:---|:---|:---|:---|
|1.|0 x 80070001|Befehl vom passiven Controller und kann nicht mit der aktiven Domänencontroller kommunizieren.|Führen Sie den Befehl auf dem aktiven Domänencontroller. Führen Sie den Befehl vom passiven Controller müssen Sie Verbindung zwischen passiven und aktiven Domänencontroller zu beheben. Sie müssen sich von Microsoft Support, wenn diese Verbindung unterbrochen wird.|
|2.|0x800710dd - Vorgangs-ID ist ungültig|Proxyeinstellungen werden auf StorSimple virtuelles Gerät nicht unterstützt.|Proxyeinstellungen werden auf StorSimple virtuelles Gerät nicht unterstützt. Diese können nur auf einem physischen Gerät StorSimple konfiguriert werden.|
|3.|0 x 80070057 - Ungültiger parameter|Einer der Parameter für die Proxyeinstellungen ist ungültig.|Der URI wird nicht im richtigen Format bereitgestellt. Verwenden Sie das folgende Format:`http://<IP address or FQDN of the web proxy server>:<TCP port number>`|
|4.|0x800706BA - RPC-Server nicht verfügbar|Die Ursache ist eine der folgenden:</br></br>Cluster ist nicht.</br></br>DataPath-Dienst wird nicht ausgeführt.</br></br>Der Befehl vom passiven Controller und kann nicht mit der aktiven Domänencontroller kommunizieren.|Führen Sie Microsoft Support für Cluster und Datapath Service ausgeführt wird.</br></br>Führen Sie den Befehl vom aktiven Controller. Wenn Sie den Befehl vom passiven Controller ausführen möchten, müssen Sie sicherstellen, dass der passive Controller mit dem aktiven Controller kommunizieren kann. Sie müssen sich von Microsoft Support, wenn diese Verbindung unterbrochen wird.|
|5.|0x800706BE - RPC-Aufruf ist fehlgeschlagen|Cluster ist.|Führen Sie Microsoft Support, um sicherzustellen, dass der Cluster ist.|
|6.|0x8007138f - Cluster-Ressource nicht gefunden|Plattform Service-Clusterressource wurde nicht gefunden. Dies kann passieren, wenn die Installation nicht ordnungsgemäße.|Sie müssen eine Factory zurücksetzen auf Ihrem Gerät ausführen. Sie müssen eine Plattform-Ressource erstellen. Wenden Sie sich an Microsoft Support für nächste Schritte.|
|7.|0x8007138c - Cluster-Ressource nicht online|Plattform oder Datapath Clusterressourcen sind nicht online.|Wenden Sie sich an Microsoft Support, um sicherzustellen, dass die Dienstressource Datapath und Plattform online sind.|

> [AZURE.NOTE] 
> 
> -  Fehlermeldungen die obige Liste ist nicht vollständig. 
> - Fehler im Zusammenhang mit Webproxyeinstellungen werden im klassischen Azure-Portal in Ihrem StorSimple Manager-Dienst nicht angezeigt. Liegt ein Problem mit dem Webproxy nach Abschluss die Konfiguration, wird der Gerätestatus **Offline** im klassischen Portal ändern. |

## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie beim Bereitstellen von Ihrem Gerät oder Web-Proxy konfigurieren Probleme, finden Sie in der [Problembehandlung bei der Bereitstellung der StorSimple-Gerät](storsimple-troubleshoot-deployment.md).

- Verwendung den StorSimple Manager-Dienst finden Sie unter verwenden [den StorSimple Manager-Dienst zum Verwalten von StorSimple-Gerät](storsimple-manager-service-administration.md).
