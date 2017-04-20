<properties
   pageTitle="Azure AD Connect: Komponenten und Hardware | Microsoft Azure"
   description="Dieses Thema beschreibt die erforderlichen Komponenten und die Hardware Azure AD verbinden"
   services="active-directory"
   documentationCenter=""
   authors="andkjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/12/2016"
   ms.author="billmath"/>

# <a name="prerequisites-for-azure-ad-connect"></a>Erforderliche Komponenten für Azure AD verbinden
Dieses Thema beschreibt die erforderlichen Komponenten und die Hardware Azure AD verbinden.

## <a name="before-you-install-azure-ad-connect"></a>Vor der Installation von Azure AD verbinden
Vor der Installation von Azure AD Connect sind ein paar Dinge, die Sie benötigen.

### <a name="azure-ad"></a>Azure AD
- Ein Azure-Abonnement oder ein [Testabonnement Azure](https://azure.microsoft.com/pricing/free-trial/). Dies ist nur erforderlich für den Zugriff auf den Azure-Portal und nicht für die Verwendung von Azure AD verbinden. Mithilfe von PowerShell oder Office 365 brauchen Sie keine Azure-Abonnement Azure AD Connect verwenden. Haben Sie eine Lizenz für Office 365 können Sie auch das Office 365-Portal. Bezahlte Office 365-Lizenz erhalten Sie auch in der Azure-Portal von Office 365-Portal.
    - Sie können auch Azure AD-Vorschaufunktion in [Azure-Portal](https://portal.azure.com). Dieses Portal ist eine Azure-Lizenz nicht erforderlich.
- [Hinzufügen und Domäne überprüfen](active-directory-add-domain.md) in Azure AD verwenden möchten. Beispielsweise möchten Sie contoso.com für die Benutzer dann sicher dieser Domäne wurde überprüft und sind nicht nur die Standarddomäne contoso.onmicrosoft.com verwenden.
- Ein Azure AD-Mandanten kann standardmäßig 50k Objekte. Bei der Überprüfung Ihrer Domäne ist die Beschränkung auf 300 k-Objekte erhöht. Benötigen Sie weitere Objekte in Azure AD müssen Sie eine Supportanfrage um den Grenzwert weiter erhöht. Benötigen Sie mehr als 500 k Objekte benötigen Sie eine Lizenz wie Office 365, Azure AD grundlegende, Azure AD Premium oder Enterprise Mobility Suite.

### <a name="prepare-your-on-premises-data"></a>Die lokalen Daten
- [Optionale Synchronisierungsfunktionen, die in Azure AD können](active-directory-aadconnectsyncservice-features.md) überprüfen und bewerten mit aktivieren sollten.

### <a name="on-premises-servers-and-environment"></a>Auf lokalen Servern und Ihrer Umgebung
- Das AD-Schema Version Funktionsebenen muss WindowsServer 2003 oder höher. Domänencontroller können Version ausführen, solange die Schema und die Gesamtstruktur Anforderungen erfüllt sind.
- Wenn Sie Funktion **Rückschreiben Kennwort** verwenden müssen Domänencontroller unter Windows Server 2008 (mit aktuellsten SP) oder höher sein. Wenn Ihre Domänencontroller auf 2008 (vor R2) müssen Sie auch [Hotfix KB2386717](http://support.microsoft.com/kb/2386717)anwenden.
- Azure AD verwendete Domänencontroller muss schreibbar sein. Verwenden ein RODC (schreibgeschützte Domänencontroller) wird nicht unterstützt und Azure AD Connect keine schreiben leitet folgen.
- Azure AD Connect kann auf Small Business Server oder Windows Server Essentials installiert werden. Der Server muss Windows Server standard oder höher verwenden.
- Azure AD Connect-Server muss eine vollständige Benutzeroberfläche installiert haben. Installieren Sie auf Server Core wird nicht unterstützt.
- Azure AD Connect muss unter Windows Server 2008 oder höher installiert sein. Dieser Server eventuell ein Domänencontroller oder ein Mitgliedsserver express verwenden. Wenn Sie Standardeinstellungen verwenden, kann eigenständige Server und muss nicht Mitglied einer Domäne sein.
- Wenn Sie Azure AD Verbinden unter Windows Server 2008 installieren, müssen Sie der neuesten Hotfixes von Windows Update. Die Installation kann nicht mit einem nicht gepatchte Server starten.
- Wenn Sie die Funktion **Kennwortsynchronisation**verwenden möchten, muss Azure AD Connect-Server unter Windows Server 2008 R2 SP1 oder höher sein.
- Azure AD Connect Server muss [.NET Framework 4.5.1](#component-prerequisites) oder höher und [Microsoft PowerShell 3.0](#component-prerequisites) oder höher installiert sein.
- Wenn Active Directory Federation Services bereitgestellt wird, muss der Server dem AD FS oder Web Application Proxy installiert sind Windows Server 2012 R2 oder höher. Dieser Server für Remoteinstallationsdienste müssen [Windows-Remoteverwaltung](#windows-remote-management) aktiviert.
- Active Directory Federation Services bereitgestellt wird, sollten Sie [SSL-Zertifikate](#ssl-certificate-requirements).
- Wenn Active Directory Federation Services bereitgestellt wird, müssen Sie [Auflösung](#name-resolution-for-federation-servers)konfigurieren.
- Azure AD-Verbindung erfordert eine SQL Server-Datenbank zum Speichern von Daten. Ein SQL Server 2012 Express-LocalDB (eine light-Version von SQL Server Express) ist standardmäßig installiert, und das Dienstkonto für den Dienst auf dem lokalen Computer erstellt. SQL Server Express ist auf 10GB Größe begrenzt, die ungefähr 100.000 Objekten verwalten. Benötigen Sie eine höhere Verzeichnisobjekte verwalten, müssen Sie den Installationsassistenten auf einer anderen SQL Server-Installation zeigen.
- Bei Verwendung einer separaten SQL Server gelten diese Vorschriften:
    - Azure AD Connect unterstützt alle Versionen von Microsoft SQL Server von SQL Server 2008 (SP4) auf SQL Server 2014. Microsoft Azure SQL-Datenbank wird als Datenbank **nicht unterstützt** .
    - Verwenden Sie Groß-und Kleinschreibung SQL-Sortierung. Diese sind gekennzeichnet mit einer \_CI_ in ihrem Namen. Ist eine Groß-/Kleinschreibung Sortierung identifizierten verwenden **nicht unterstützt** \_CS_ in ihrem Namen.
    - Sie können nur ein Synchronisierungsmodul pro Instanz. Es ist die Datenbankinstanz FIM-MIM synchronisieren, DirSync oder Azure AD Sync freigeben **nicht unterstützt** .

### <a name="accounts"></a>Konten
- Azure AD globalen Administratorkonto für Azure AD-Verzeichnis integrieren möchten. Dies muss eine **Schule oder Organisation Konto** und ein **Microsoft-Konto**nicht möglich.
- Express konfigurieren oder Aktualisieren von DirSync müssen Enterprise-Administratorkonto für die lokale Active Directory Sie.
- [Konten in Active Directory](./connect/active-directory-aadconnect-accounts-permissions.md) den Installationspfad Standardeinstellungen verwenden.

### <a name="azure-ad-connect-server-configuration"></a>Konfiguration von Azure AD Connect server
- Wenn globalen Administratoren MFA aktiviert haben, muss die URL **https://secure.aadcdn.microsoftonline-p.com** in der Liste vertrauenswürdiger Sites sein. Sie werden aufgefordert, das zur Liste vertrauenswürdigen Sites hinzufügen, wenn sie nicht vor eine Herausforderung MFA aufgefordert werden. Internet Explorer können Sie zu Ihren vertrauenswürdigen Sites hinzufügen.

### <a name="connectivity"></a>Konnektivität
- Azure AD Connect Server benötigt DNS-Auflösung für Intranet und Internet. Der DNS-Server muss Namen für lokale Active Directory Azure AD-Endpunkten auflösen.
- Wenn Firewalls im Intranet und Sie Ports zwischen Azure AD Connect-Server und den Domänencontrollern öffnen müssen, sehen Sie [Azure AD Connect Ports](active-directory-aadconnect-ports.md) für Weitere Informationen.
- Wenn der Proxyserver oder Firewall Limit URLs zugegriffen werden können, dann die URLs in [Office 365 URLs und IP-Adressbereiche](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) dokumentiert geöffnet werden muss.
    - Wenn Sie Cloud von Microsoft in Deutschland oder Microsoft Azure Government Cloud verwenden, dann finden Sie unter [Azure AD Connect Sync Service Instanzen Aspekte](active-directory-aadconnect-instances.md) URLs.
- Azure AD Connect ist standardmäßig mit TLS 1.0 mit Azure AD kommunizieren. Sie können dies auf TLS 1.2 Schritte [Aktivieren Sie TLS](#enable-tls-12-for-azure-ad-connect)1.2 für Azure AD Connect ändern.
- Wenn Sie ausgehenden Proxy für die Verbindung mit dem Internet verwenden, muss die folgende Einstellung in der Datei **C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config** Installationsassistenten und Azure AD-Verbindung synchronisieren können die Verbindung zum Internet und Azure AD hinzugefügt werden. Dieser Text muss am Ende der Datei eingegeben werden. In diesem Code &lt;PROXYADRESS&gt; die tatsächliche IP-Adresse oder den Hostnamen Proxyname darstellt.

```
    <system.net>
        <defaultProxy>
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

- Wenn der Proxyserver Authentifizierung erfordert, das [Dienstkonto](./connect/active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts) muss in der Domäne befinden und benutzerdefinierten Einstellungen Installationspfad muss ein [benutzerdefiniertes Dienstkonto](./connect/active-directory-aadconnect-get-started-custom.md#install-required-components)verwenden. Außerdem benötigen Sie eine andere Änderung machine.config. Mit diesem Beantwortung Änderung in machine.config den Installationsassistenten und Synchronisierungsmodul Authentifizierungsanfragen vom Proxy-Server. Alle Installationsassistenten werden Seiten außer Seite **Konfigurieren** , das die Anmeldeinformationen des Benutzers verwendet. Auf der Seite **Konfigurieren** am Ende des Installations-Assistenten wird das [Dienstkonto](./connect/active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts) Kontextwechsel, die Sie erstellt wurde. Machine.config Abschnitt sollte wie folgt aussehen.

```
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

Weitere Informationen zu den [Standardproxy Element](https://msdn.microsoft.com/library/kd3cf2ex.aspx)finden Sie unter MSDN.

Haben Sie Probleme mit der Konnektivität, finden Sie unter [Problembehandlung bei Verbindungsproblemen](active-directory-aadconnect-troubleshoot-connectivity.md).

### <a name="other"></a>Andere
- Optional: Ein Testbenutzerkonto zum Überprüfen der Synchronisierung.

## <a name="component-prerequisites"></a>Komponente Komponenten

### <a name="powershell-and-net-framework"></a>PowerShell und .net Framework
Azure AD Connect ist Microsoft PowerShell und.NET Framework 4.5.1. Sie benötigen diese Version oder eine höhere Version auf dem Server installiert. Je nach Ihrer Version von Windows Server folgendermaßen Sie vor:

- Windows Server 2012R2
  - Microsoft PowerShell standardmäßig installiert ist, ist keine Aktion erforderlich.
  - .NET Framework 4.5.1 und späteren Versionen werden über Windows Update angeboten. Stellen Sie sicher, dass Sie Windows Server im Steuerungsbedienfeld die neuesten Updates installiert haben.
- Windows Server 2008 R2 und Windows Server 2012
  - Die neueste Version von Microsoft PowerShell ist in **Windows Management Framework 4.0**auf [Microsoft Download Center](http://www.microsoft.com/downloads)verfügbar.
  - .NET Framework 4.5.1 und späteren Versionen sind im [Microsoft Download Center](http://www.microsoft.com/downloads)verfügbar.
- WindowsServer 2008
  - Die neueste unterstützte Version von PowerShell ist in **Windows Management Framework 3.0**auf [Microsoft Download Center](http://www.microsoft.com/downloads)verfügbar.
 - .NET Framework 4.5.1 und späteren Versionen sind im [Microsoft Download Center](http://www.microsoft.com/downloads)verfügbar.

### <a name="enable-tls-12-for-azure-ad-connect"></a>Aktivieren Sie TLS 1.2 von Azure AD hergestellt
Azure AD Connect TLS 1.0 wird standardmäßig für die Verschlüsselung der Kommunikation zwischen den Synchronisierungsserver Engine und Azure AD verwendet. Durch Konfigurieren von .net Applications TLS 1.2 standardmäßig auf dem Server verwenden, können Sie dies ändern. Weitere Informationen zu TLS 1.2 finden im [Microsoft Security Advisory 2960358](https://technet.microsoft.com/security/advisory/2960358).

1. TLS 1.2 kann unter Windows Server 2008 aktiviert werden. Sie benötigen Windows Server 2008 R2 oder höher. Vergessen Sie .net 4.5.1 Hotfix für Ihr Betriebssystem installiert haben, finden Sie unter [Microsoft Security Advisory 2960358](https://technet.microsoft.com/security/advisory/2960358). Sie müssen diese oder eine spätere Version bereits auf dem Server installiert.
2. Wenn Sie Windows Server 2008 R2 verwenden, stellen Sie sicher, dass TLS 1.2 aktiviert ist. Windows Server 2012 Server und späteren Versionen sollten TLS 1.2 bereits aktiviert sein.
```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
```
3. Für alle Betriebssysteme dieser Registrierungsschlüssel, und starten Sie den Server.
```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319
"SchUseStrongCrypto"=dword:00000001
```
4. Wenn Sie TLS 1.2 zwischen den Synchronisierungsserver Engine und remote SQL Server aktivieren möchten, stellen Sie sicher, die benötigten Versionen für [TLS 1.2-Unterstützung für Microsoft SQL Server](https://support.microsoft.com/kb/3135244)installiert haben.

## <a name="prerequisites-for-federation-installation-and-configuration"></a>Erforderliche Komponenten für die Föderation Installation und Konfiguration

### <a name="windows-remote-management"></a>Windows-Remoteverwaltung
Wenn Azure AD-Verbindung mit Active Directory Federation Services oder Web Application Proxy bereitstellen, überprüfen die Anforderungen unten Konnektivität und Konfiguration erfolgreich:

- Wenn der Zielserver Domäne verbunden ist, sicherzustellen Sie, dass Windows Remote verwalteten aktiviert ist
    - In einem erhöhten PSH Befehlsfenster den Befehl`Enable-PSRemoting –force`
- Wenn der Zielserver ne WAP-Domain verbunden ist, gibt es ein paar zusätzliche Vorschriften
    - Auf dem Zielcomputer (WAP Computer):
         - Sicherstellen der Winrm (Windows Remote Management / WS-Management) Dienst über das Dienste-Snap-in
         - In einem erhöhten PSH Befehlsfenster den Befehl`Enable-PSRemoting –force`
    - Auf dem Computer, auf dem der Assistent ausgeführt wird (wenn der Zielcomputer nicht-verknüpften oder nicht vertrauenswürdigen Domäne ist):
        - In einem erhöhten PSH Befehlsfenster den Befehl`Set-Item WSMan:\localhost\Client\TrustedHosts –Value <DMZServerFQDN> -Force –Concatenate`
        - Im Server-Manager:
             - WAP DMZ Host Computer Pool hinzufügen (Server-Manager -> Verwaltung -> Server hinzufügen... DNS-Registerkarte)
             - Registerkarte Server Manager alle Server: Rechtsklick WAP-Server und wählen Sie verwalten As..., geben Sie lokale (keine Domäne) Anmeldeinformationen für WAP-Computer
             - PSH-Remotekonnektivität in der Registerkarte Server Manager alle Server überprüfen: rechts klicken WAP-Server und Windows PowerShell.  Eine Remotesitzung PSH soll um sicherzustellen, dass Remotesitzungen PowerShell hergestellt werden können.

### <a name="ssl-certificate-requirements"></a>SSL-Zertifikat an
**Wichtig:** es wird dringend empfohlen, alle Knoten des AD FS-Farm sowie alle Web Application Proxyserver dasselbe SSL-Zertifikat verwenden.

- Das Zertifikat muss ein X509 Zertifikat.
- Ein selbstsigniertes Zertifikat können für Verbundserver in einer Lab-testumgebung. Allerdings empfiehlt für eine produktionsumgebung Zertifikat von einer öffentlichen Zertifizierungsstelle.
    - Wenn ein Zertifikat verwenden, das nicht öffentlich vertrauenswürdig ist, sicherzustellen Sie, dass das Zertifikat auf jedem Server Web Application Proxy installiert auf dem lokalen Server und alle Verbundserver vertrauenswürdig ist
- Die Identität des Zertifikats muss der Föderation Dienstname (z. B. sts.contoso.com).
    - Die Identität ist entweder Subject alternative (SAN) Erweiterung des Typs dNSName oder keine SAN-Einträge vorhanden sind, der Antragstellernamen angegebenen gemeinsamen Namen.  
    - Mehrere SAN-Einträge können das Zertifikat werden davon Federation Service Name entspricht.
    - Wenn Sie Arbeitsplatz Join verwenden, muss eine zusätzliche SAN mit dem Wert **Enterpriseregistration.** gefolgt von Suffix (UPN = User Principal Name) des Unternehmens, z. B. **enterpriseregistration.contoso.com**.
- Zertifikate CryptoAPI next Generation (CNG) Schlüssel und Schlüsselspeicheranbieter werden nicht unterstützt. Daher müssen Sie eine Zertifikat basierend auf einen Kryptografiedienstanbieter (cryptographic Service Provider) und keinen KSP (Key Storage Provider) verwenden.
- Platzhalter-Zertifikate werden unterstützt.

### <a name="name-resolution-for-federation-servers"></a>Auflösung für Verbundserver
- Einrichten von DNS-Datensätzen für AD FS Federation Service Name (z.B. sts.contoso.com) für Intranet (die internen DNS-Server) und das extranet (Öffentliche DNS durch Ihre Domainregistrierungsstelle). Für das Intranet DNS Datensatzes müssen Sie ein verwenden und keine CNAME Datensätze. Dies ist erforderlich für die Windows-Authentifizierung für die Domäne verknüpften Computer ordnungsgemäß.
- Wenn Sie mehrere AD FS-Server oder Web Application Proxy Server bereitstellen, müssen Sie sicherstellen der Lastenausgleich konfiguriert haben und die DNS-Datensätze für AD FS Federation Service Name (z. B. sts.contoso.com) zum Lastenausgleich zeigen.
- Integrierte Windows-Authentifizierung mithilfe von Internet Explorer in Ihrem Intranet Browseranwendungen arbeiten sicher, dass der Intranetzone in Internet Explorer AD FS Federation Service Name (z. B. sts.contoso.com) hinzugefügt wird. Dies kann über die Gruppenrichtlinie gesteuert und Ihre Domäne verbundene Computer bereitgestellt werden.

## <a name="azure-ad-connect-supporting-components"></a>Azure AD verbinden Komponenten
Folgendes ist eine Liste der Komponenten, die Azure AD-Verbindung auf dem Server installiert, Azure AD Connect installiert ist. Diese Liste ist für eine einfache Expressinstallation von.  Möchten Sie eine andere SQL Server-Installation Services der Seite verwenden, ist SQL Express LocalDB nicht lokal installiert.

- Azure AD Connect Health
- Microsoft Online Services-In Assistant für IT-Experten (aber keine Abhängigkeit installiert)
- Microsoft SQL Server 2012-Befehlszeilen-Dienstprogramme
- Microsoft SQL Server 2012 Express LocalDB
- Microsoft SQL Server 2012 Native Client
- Microsoft Visual C++ 2013 Verteilungspakets

## <a name="hardware-requirements-for-azure-ad-connect"></a>Hardware-Mindestanforderungen für Azure AD verbinden
Folgende Tabelle enthält die Mindestanforderungen für Computer synchronisieren Azure AD verbinden.

| Anzahl der Objekte in Active Directory | CPU | Speicher | Festplattengröße |
| ------------------------------------- | --- | ------ | --------------- |
| Weniger als 10.000 | 1,6 GHz | 4 GB | 70 GB |
| 10.000-50.000 | 1,6 GHz | 4 GB | 70 GB |
| 50.000 bis 100.000 | 1,6 GHz | 16 GB | 100 GB |
| Für mindestens 100.000 Objekte ist die Vollversion von SQL Server erforderlich|  |  |  |
| 100.000 bis 300.000 | 1,6 GHz | 32 GB | 300 GB |
| 300.000 – 600.000 | 1,6 GHz | 32 GB | 450 GB |
| Mehr als 600.000 | 1,6 GHz | 32 GB | 500 GB |

Die Mindestanforderungen für AD FS oder Web Application Server Computern lautet wie folgt:

- CPU: Dual core 1,6 GHz oder höher
- Speicher: 2GB oder mehr
- Azure VM: Konfiguration A2 oder höher

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über [die lokalen Identitäten Azure Active Directory integrieren](active-directory-aadconnect.md).
