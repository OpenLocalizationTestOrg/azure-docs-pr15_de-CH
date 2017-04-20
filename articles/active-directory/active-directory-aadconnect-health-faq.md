<properties
    pageTitle="Azure AD Connect Health FAQ"
    description="Diese FAQ beantwortet Fragen zur Azure AD Connect Health. Diese FAQ behandelt Fragen zu nutzen, einschließlich der Abrechnung Modell, Funktionen eingeschränkt und Support."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>


# <a name="azure-ad-connect-health-frequently-asked-questions-faq"></a>Azure AD Connect Health häufig gestellte Fragen (FAQ)

Diese FAQ beantwortet Fragen zur Azure AD Connect Health. Diese FAQ behandelt Fragen zu nutzen, einschließlich der Abrechnung Modell, Funktionen eingeschränkt und Support.

## <a name="general-questions"></a>Allgemeine Fragen



**F: Verwaltung mehrerer Azure AD-Verzeichnisse. Wie wechseln kann ich mit Azure Active Directory Premium?**

Sie können zwischen verschiedenen Azure AD Mieter derzeit mit Benutzernamen oben rechts markieren und dann das entsprechende Konto. Wenn das Konto hier nicht aufgeführt ist, wählen Sie abmelden und verwenden Sie globaler Administrator-Anmeldeinformationen des Verzeichnisses mit den Azure Active Directory Premium Anmelden aktiviert ist.

## <a name="installation-questions"></a>Fragen zur Installation



**Was ist die Auswirkung der Azure AD verbinden Agent auf einzelnen Servern installieren?**

Die Auswirkung der Installation von Microsoft Azure AD verbinden Health Agents ADFS, Web Application Proxy Server, Azure AD verbinden (Sycn)-Server, Domänencontroller ist minimal in Bezug auf CPU, Arbeitsspeicher Verbrauch Netzwerkbandbreite und Speicher.

Unten sind ein Näherungswert.

- CPU-Verbrauch: ~ 1 Prozent
- Arbeitsspeicher: bis zu 10 % des verfügbaren Systemspeichers

>[AZURE.NOTE] Bei den Agent nicht in Azure kommunizieren speichert der Agent die Daten lokal bis festgelegten Höchstgrenze. Der Agent überschreibt die "Cache" Daten "zuletzt bearbeitet".

- Lokaler Pufferspeicher für Azure AD verbinden Health: ~ 20 MB
- AD FS-Server wird empfohlen, Bereitstellung von Speicherplatz 1024 MB (1 GB) für AD FS überwachen Kanal für Azure AD verbinden Health Agents, die Audit-Daten verarbeiten, bevor sie überschrieben wird.

**F: muss ich mein Server während der Installation von Azure AD Connect Health Agents neu starten?**

Nein. Die Installation der Agents erfordert nicht den Server neu starten. Allerdings können einige der erforderlichen Schritte einen Neustart des Servers installiert.

Beispielsweise erfordert auf Windows Server 2008 R2 die Installation von .net Framework 4.5 Neustart des Servers.


**F: ist Azure AD verbinden Health Services arbeiten über Pass-Through-HTTP-Proxy?**

Ja.  Für können Sie auf Operationen der Agent zum Weiterleiten von ausgehenden HTTP-Anfragen über einen HTTP-Proxy konfigurieren. Weitere Informationen finden Sie [Konfigurieren Azure AD verbinden Health Agent HTTP-Proxy verwendet.](active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy)
Benötigen Sie einen Proxy während der Agent-Registrierung konfigurieren, müssen Sie vorher die Proxyeinstellungen von Internet Explorer ändern.
1. Öffnen Sie InternetExplorer-Einstellungen > Internet -> Optionen -> Connections-LAN Settings >.
2. Wählen Sie einen Proxyserver für LAN verwenden.
3. Wählen Sie erweitert, haben Sie unterschiedliche Proxy-Ports für HTTP und HTTPS-Secure.

**F: unterstützt Azure AD Connect Health Services Standardauthentifizierung HTTP-Proxys Verbindung?**

Nein. Ein Mechanismus zum Angeben beliebiger Benutzername und Kennwort für die Standardauthentifizierung wird derzeit nicht unterstützt.


**Welche Version von AD DS werden von Azure AD Connect Health für AD DS unterstützt?**

Überwachung von AD DS wird auf folgenden Betriebssystemversionen unterstützt:

- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2

## <a name="operations-questions"></a>Operations-Fragen



**F: muss ich mein AD FS-Proxy-Server oder Proxy Meine Webanwendungsserver Überwachung aktiviert?**

Nein, Überwachung muss nicht auf den Servern Web Application Proxy (WAP) aktiviert werden. Aktivieren sie den AD FS-Servern.


**F: wie behoben Azure AD verbinden Gesundheitswarnungen?**

Azure AD Health Alerts Verbinden einer Bedingung erfolgreich aufgelöst. Azure AD verbinden Gesundheit erkennen und Erfolgszuständen an den Dienst regelmäßig Bericht. Für einige Alarme ist die Unterdrückung zeitbasiert. Also bei demselben Fehler innerhalb von 72 Stunden von Alarm generiert, wird die Warnung automatisch aufgelöst.




**Welche Firewallports für den Azure AD verbinden Health Agent zu öffnen müssen?**

Sie müssen TCP/UDP-Ports 443 und 5671 Öffnen der Azure AD verbinden Health Agent mit Azure AD Health Dienstendpunkte kommunizieren.


**F: Warum wird sehen zwei Server mit demselben Namen in Azure AD verbinden Gesundheitsportal?**

Wenn Sie einen Agenten von einem Server entfernen, ist der Server von Azure AD Verbinden Portal nicht automatisch entfernt.  Manuell entfernt einen Agent auf einem Server oder den Server selbst entfernt, müssen Sie manuell den Servereintrag aus Azure AD verbinden Gesundheitsportal löschen. Weitere Informationen finden Sie unter [eine Server oder Dienst Instanz löschen.](active-directory-aadconnect-health-operations.md#delete-a-server-or-service-instance)

Verhindert einen Server oder erstellt einen neuen Server mit der gleichen (z.B. Computer) und wurden nicht registrierten Server von Azure AD verbinden Gesundheitsportal entfernt den Agenten installiert auf dem neuen Server sehen Sie zwei Einträge mit demselben Namen.  
In diesem Fall sollten Sie den Eintrag in den älteren Server manuell löschen. Die Daten für diesen Server sollte nicht mehr aktuell.

## <a name="related-links"></a>Verwandte links

* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Azure AD Connect Health Agent-Installation](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health-Operationen](active-directory-aadconnect-health-operations.md)
* [Schließen Sie Gesundheit mit Azure AD mit AD FS](active-directory-aadconnect-health-adfs.md)
* [Verwenden von Azure AD Connect Health für Synchronisierung](active-directory-aadconnect-health-sync.md)
* [Gesundheit in AD DS verbinden über Azure AD](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Version Gesundheitsdaten](active-directory-aadconnect-health-version-history.md)
