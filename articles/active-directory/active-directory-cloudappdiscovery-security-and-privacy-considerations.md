<properties
    pageTitle="Cloud App Discovery-Sicherheit und Datenschutz | Microsoft Azure"
    description="Dieses Thema beschreibt die Cloud App Ermittlungskosten Aspekte der Sicherheit und Datenschutz."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi"/>

# <a name="cloud-app-discovery-security-and-privacy-considerations"></a>Cloud App Discovery-Sicherheit und Datenschutz

Microsoft ist bestrebt, Ihre Privatsphäre zu schützen und sichern Ihre Daten, und Software und Services, die Ihnen die Sicherheit Ihrer Organisation verwalten. <br>
Wir erkennen, die beim Übertragen der Daten an andere Vertrauensstellung strengen Sicherheitsinvestitionen engineering und Know-how zu erforderlich sind.
Microsoft befolgt strikt und Sicherheitsrichtlinien von sicherer Software Development Lifecycle Praktiken zu einem. <br>
Sichern und Schützen von Daten ist bei Microsoft.

In diesem Thema wird erläutert, wie Daten gesammelt, verarbeitet und in Azure Active Directory Cloud App Discovery gesichert




##<a name="overview"></a>Übersicht

Cloud App Discovery ist eine Funktion von Azure AD und in Microsoft Azure gehostet wird. <br>
Die Cloud App Discovery Endgeräte-Agenten wird zum IT-verwaltete Computer Discovery Daten gesammelt. <br>
Die gesammelten Daten werden über einen verschlüsselten Kanal Azure AD Cloud App Discovery Service sicher gesendet. <br>
Die Cloud App Discovery-Daten für eine Organisation ist in Azure-Portal sichtbar. <br>


<center>![Funktionsweise von Cloud App Discovery](./media/active-directory-cloudappdiscovery-security-and-privacy-considerations/cad01.png)</center> <br>


Die folgenden Abschnitte folgen Informationen und beschreiben, wie sie von Ihrer Organisation zum Cloud App-Suchdienst bewegt gesichert wird und die Cloud App Discovery-Portal.



## <a name="collecting-data-from-your-organization"></a>Sammeln von Daten aus Ihrer Organisation

Um Active Directory Azure Cloud App-Suchfunktion Einblicke in Clientanwendungen von Mitarbeitern in Ihrer Organisation verwendet, müssen Sie zuerst die Azure AD Cloud App Discovery Endgeräte-Agenten auf Computern in Ihrer Organisation bereitstellen.

Administratoren von Azure Active Directory Mieter (oder Stellvertretung) können das Agenteninstallationspaket von Azure-Portal herunterladen. Der Agent kann manuell installiert oder auf mehreren Computern in der Organisation mit SCCM oder Gruppenrichtlinien installiert.

Weitere Informationen über Bereitstellungsoptionen finden Sie unter [Cloud App Discovery Group Policy Deployment Guide](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx).
<br>

### <a name="data-collected-by-the-agent"></a>Vom Agent gesammelte Daten

In der folgenden Liste beschriebene Informationen werden vom Agenten beim Herstellen eine Verbindung eine Web-Anwendung. Die Informationen werden nur für Anwendungsbereiche, die der Administrator für die Erkennung konfiguriert wurde. <br>
Bearbeiten Sie die Liste der Agent über die Cloud App Discovery Blade im Microsoft [Azure-Portal](https://portal.azure.com/)unter **Einstellungen**überwacht Cloud-Apps->**Daten**->**App Sammlungsliste**. Weitere Informationen finden Sie unter [Erste Schritte mit Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)
<br>
**Kategorie**: Benutzerinformationen <br>
**Beschreibung**: <br>
Windows-Benutzername des Prozesses, der einen auf Anwendung Antrag (z. B.: DOMÄNE\Benutzername) sowie Windows Security Identifier (SID) des Benutzers.


**Kategorie**: Informationen <br>
**Beschreibung**: <br>
Der Name des Prozesses, der Antrag auf Anwendung (z.B.: "iexplore.exe")

**Kategorie**: Computer Informationen <br>
**Beschreibung**: <br>
Der NetBIOS-Computername, der Agent.

**Kategorie**: App Informationen <br>
**Beschreibung**: <br>

Die folgenden Verbindungsinformationen:

- Die Quelle (lokaler Computer) und IP-Adressen und Portnummern

- Die öffentliche IP-Adresse der Organisation, über die die Anforderung geht.

- Die Uhrzeit der Anforderung

- Der Umfang des Datenverkehrs gesendet und empfangen

- IP-Version (4 oder 6)

- Für TLS-Verbindungen: Zielhostname Erweiterung Server Name festgestellt oder das Server-Zertifikat.

Die folgenden HTTP-Informationen:

- Methode (GET, POST usw.).

- Protokoll (HTTP/1.1 usw.)

- Benutzeragententext

- Hostname

- Ziel-URI (Abfragezeichenfolge)

- Informationen

- Referenz-URL-Informationen (ausgenommen Abfragezeichenfolge)



> [AZURE.NOTE] HTTP-Informationen werden für alle Verbindungen ohne Verschlüsselung.
Für TLS-Verbindungen wird diese Informationen nur erfasst, wenn die Einstellung "Verschärfte Inspektion" im Portal aktiviert ist. Die Einstellung ist 'ON'.
Für weitere Details siehe unten und [Erste Schritte mit Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)


Zusätzlich zu den Daten der Agent über die Netzwerkaktivität sammelt sammelt er auch anonyme Informationen über Software und Hardware-Konfiguration, Berichte und Informationen wie der Agent verwendet wird.

<br><br>
### <a name="how-the-agent-works"></a>Wie funktioniert der agent

Die Agenteninstallation umfasst zwei Komponenten:

- Eine Komponente im Benutzermodus

- Ein Kernelmodus-Treiber-Komponente (Windows-Filterplattform Treiber)



Bei der Erstinstallation des Agents speichert sie computerspezifische vertrauenswürdiges Zertifikat auf dem Computer die sichere Verbindung mit dem Cloud App Discovery-Dienst verwendet. <br>
Der Agent ruft regelmäßig Konfiguration aus der Cloud App Discovery-Dienst über sichere Verbindung ab. <br>
Die Richtlinie enthält Informationen über die Cloudanwendungen zu überwachen, ob automatische Aktualisierung, unter anderem aktiviert werden soll.

Web-Datenverkehr gesendet und empfangen auf dem Computer Internet Explorer und Chrome, Cloud App Discovery-Agent analysiert den Datenverkehr und die relevante Metadaten extrahiert (siehe oben **vom Agent gesammelte Daten** ). <br>
Jede Minute lädt der Agent gesammelte Metadaten Cloud App Discovery-Dienst über einen verschlüsselten Kanal.

Die Komponente fängt verschlüsselten Datenverkehr und fügt sich in den verschlüsselten Stream. Weitere Informationen im Abschnitt **Abhören Daten verschlüsselte Verbindung (verschärfte Inspektion)** .


### <a name="respecting-user-privacy"></a>Privatsphäre der Benutzer

Soll Administratoren die Tools der Saldo zwischen detaillierte Anwendung Verwendung und Benutzer Privatsphäre entsprechend ihrer Organisation bereitzustellen. Zu diesem Zweck bieten wir die folgenden Regler auf der Einstellungsseite im Portal:

- **Datensammlung**: Administratoren können wählen, welche Anwendung oder sie Ermittlungsdaten möchten Kategorien angeben.

- **Verschärfte Inspektion**: können Administratoren angeben, wenn der Agent HTTP-Verkehr für SSL/TLS-Verbindung (auch bekannt als **"verschärfte Inspektion"**) erfasst haben. Mehr dazu im nächsten Abschnitt.

- **Optionen stimmen**: Administratoren können das Cloud App Discovery-Portal wählen, ob Benutzer die Datensammlung durch den Agent informieren und, ob Benutzer Zustimmung sammeln Daten vor dem Agent.

Die Cloud App Discovery Endgeräte-Agenten erfasst nur **Daten vom Agent** Abschnitt beschriebene Informationen.


### <a name="intercepting-data-from-encrypted-connections-deep-inspection"></a>Verschlüsselte Verbindung (verschärfte Inspektion) Daten abfangen
Wie bereits erwähnt, können Administratoren den Agent Daten von verschlüsselte Verbindung ("verschärfte Inspektion"). TLS ([Transport Layer Security](https://msdn.microsoft.com/library/windows/desktop/aa380516%28v=vs.85%29.aspx)) ist heute eines der am häufigsten verwendeten Protokolle im Internet. Kommunikation mit TLS verschlüsseln, kann ein Client einen sicheren und privaten Kommunikationskanal mit einem Webserver herstellen; TLS bietet grundlegenden Schutz für die Übergabe von Anmeldeinformationen für die Authentifizierung und die Offenlegung von vertraulichen Informationen verhindern.

Während der End-to-End-sichere verschlüsselte Kanal von TLS wichtige Sicherheits- und Datenschutz ermöglicht, ist das Protokoll häufig für böswillige oder schädliche Zwecke missbraucht. So viel ist tatsächlich, TLS oft als "universelle Firewall-Bypass-Protokoll" bezeichnet. Die Ursache für das Problem ist, dass die meisten Firewalls nicht TLS-Kommunikation überprüfen, da die Anwendungsebene mit SSL verschlüsselt ist. Dieses Wissen nutzen Angreifer häufig TLS zum Übermitteln von bösartiger Inhalten an Benutzer sicher, die intelligente Anwendungsebene-Firewalls vollständig blinden TLS TLS-Kommunikation zwischen Hosts müssen einfach weiterleiten. Endbenutzer nutzen häufig TLS Zugriffskontrollen erzwungen durch Unternehmensfirewalls und Proxy-Server umgehen, öffentlichen Proxys und für Tunneling-Protokolle durch die Richtlinie andernfalls blockiert werden, Firewall-TLS verwenden.

Verschärfte Inspektion gestattet der Cloud App Discovery-Agent fungieren als eine vertrauenswürdige Man-in-the-Middle. Bei eine Anfrage Zugriff auf eine Ressource HTTPS geschützt, der Endgeräte-Agenten Treiber fängt die Verbindung und eine neue Verbindung mit dem Zielserver sein SSL-Zertifikat für den Client abgerufen wird. Der Agent überprüft dann das Zertifikat vertrauenswürdig kann (ob es nicht gesperrt und anderen Zertifikat überprüft), und wenn diese, dann die Endgeräte-Agenten kopiert die Informationen aus dem Serverzertifikat und erstellt eine eigene Serverzertifikat – ein abfangzertifikat - Daten genannt. Das abfangzertifikat ist mit in-the-Fly durch die Endgeräte-Agenten mit einem Root-Zertifikat im Speicher für vertrauenswürdige Zertifikate Windows installiert ist. Diese selbstsigniertes Zertifikat nicht exportierbar markiert und ACL mussten Administratoren. Soll den Computer lassen Sie niemals auf dem es erstellt wurde. Erhält der Endbenutzer Clientanwendung abfangzertifikat, wird es vertrauen, da erfolgreich bis das Stammzertifikat die Zertifikatkette überprüfen können. Dieser Prozess ist größtenteils transparent Gesichtspunkt des Endbenutzers mit ein paar Vorsichtsmaßnahmen wie unten beschrieben.

Verschärfte Inspektion aktivieren, die Cloud App Discovery Endgeräte-Agenten entschlüsseln und Überprüfen TLS verschlüsselte Kommunikation ermöglicht des Diensts reduzieren und Informationen über die Verwendung verschlüsselter Cloud-Apps.

#### <a name="a-word-of-caution"></a>Vorsicht
Bevor Sie verschärfte Inspektion aktivieren, wird dringend empfohlen, Ihre Absichten die rechts- und Personalabteilung kommunizieren und ihre Zustimmung. Überprüfen des Endbenutzers private verschlüsselte Kommunikation kann ein sensibles Thema offensichtlich sein. Vor einer Produktion Rollout verschärfte Inspektion stellen sicher, dass die Sicherheit im Unternehmen und Nutzungsrichtlinien wurden aktualisiert, um anzugeben, dass verschlüsselter Kommunikation untersucht wird. Benachrichtigung und Befreiung von Websites als vertrauliche (z. B. Banken und medizinische) auch möglicherweise Cloud App Discovery, Überwachung konfigurieren. Wie bereits erwähnt, können Administratoren Cloud App Discovery-Portal wählen, ob Benutzer die Datensammlung durch den Agent informieren und Zustimmung des Benutzers erfordern, bevor der Agent startet die Erfassung von Benutzerdaten.

### <a name="known-issues-and-drawbacks"></a>Bekannte Probleme und Nachteile
Es gibt einige Fälle, TLS Abfangen des Endbenutzers auswirken:

- Erweiterte Überprüfung (EV) Zertifikate Rendern die Adressleiste des Webbrowsers Grün als visueller Hinweis dienen, dass Sie eine vertrauenswürdige Website besuchen. TLS-Prüfung kann nicht in das Zertifikat an den Client gibt also normalerweise von Websites, die Verwendung von EV EV duplizieren, aber die Adressleiste nicht grün angezeigt wird.  

- Öffentliche Schlüssel fixieren (auch bekannt als Zertifikat fixieren) sollen schützen Anwender vor Man-in-the-Middle-Angriffe und rogue Zertifizierungsstellen. Wenn das Stammzertifikat angehefteten Site nicht eines der bekannten guten Zertifizierungsstelle, lehnt Browser die Verbindung mit einem Fehler ab. Da TLS abfangen, einen Man-in-the-Middle, nämlich fehl Verbindung.

- Wenn Benutzer das Schlosssymbol im Browser Adresse Bar Browser Standortinformationen überprüfen klicken, eine Kette endet die Zertifizierungsstelle signiert das Zertifikat der Website nicht sehen, aber eine Zertifikatskette mit Windows stattdessen vertrauenswürdigen Zertifikatspeicher.

Um die Vorkommen dieser Probleme verringern, Verfolgen der wir Cloud-Diensten und Clientanwendungen erweiterte Prüfung oder öffentliche Schlüssel festhalten und weisen Endpunkt bekannt zu abfangen betroffenen Verbindungen. Auch in diesen Fällen erhalten Sie weiterhin Berichte mit dieser Cloud-Apps und die übertragenen Daten jedoch nicht tief sind geprüft, keine Informationen über die Verwendung der apps werden.


## <a name="sending-data-to-cloud-app-discovery"></a>Senden von Daten an Cloud App Discovery

Metadaten vom Agenten gesammelt wurden, wird es auf dem Computer zwischengespeichert bis zu einer Minute oder die zwischengespeicherten Daten eine Größe von 5 MB erreicht hat. Anschließend komprimiert und über eine sichere Verbindung mit der Cloud App Discovery-Dienst gesendet.

Wenn der Agent mit der Cloud App Discovery-Dienst aus irgendeinem Grund kommunizieren kann, ist gesammelte Metadaten in einem lokalen Cache gespeichert, die nur von berechtigten Benutzern auf dem Computer (wie die Gruppe Administratoren) zugegriffen werden kann. <br>
Der Agent versucht automatisch, die zwischengespeicherte Metadaten erneut sendet, bis es Cloud App Discovery Service erhalten wurde.



## <a name="receiving-the-data-at-the-service-end"></a>Empfangen von Daten auf der Dienstseite

Die Agents Authentifizieren der App Cloud mit Computer bestimmte Client-Authentifizierungszertifikat oben verwiesen und Daten über einen verschlüsselten Kanal weitergeleitet. <br>
Die Cloud App Discovery Service Analytics Pipeline verarbeitet Metadaten für jeden Kunden separat durch logische Partitionierung in allen Phasen der Pipeline Analytics.
Analysierte Metadaten Laufwerke der verschiedenen Berichte im Portal.

Unverarbeitete Metadaten und analysiert Metadaten werden bis zu 180 Tage lang gespeichert. Darüber hinaus können Kunden analysierte Metadaten in einem Konto Azure BLOB-Speicher ihrer Wahl zu erfassen.
Dies ist nützlich für die Offlineanalyse Metadaten sowie mehr Daten.

## <a name="accessing-the-data-using-the-azure-portal"></a>Zugriff auf die Daten über das Azure-portal

Im Bemühen um schützen die Metadaten gesammelt haben standardmäßig nur globale Administratoren Pächter Cloud App Discovery-Funktionen in Azure-Portal. <br>
Administratoren können jedoch diesen Zugriff für andere Benutzer oder Gruppen delegieren.



> [AZURE.NOTE] Weitere Informationen finden Sie unter [Erste Schritte mit Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)

<br>
Jeder Benutzer Zugriff auf die Daten im Portal muss mit einer Azure AD Premium-Lizenz lizenziert.



##<a name="additional-resources"></a>Zusätzliche Ressourcen


* [Wie kann ich Cloud-apps, mit denen im Unternehmen entdecken](active-directory-cloudappdiscovery-whatis.md)
* [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)
