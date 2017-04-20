<properties
   pageTitle="Implementieren von Azure Active Directory | Microsoft Azure"
   description="Wie Sie eine sichere Hybrid-Netzwerkarchitektur mit Azure Active Directory implementieren."
   services="guidance,virtual-network,active-directory"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/28/2016"
   ms.author="telmos"/>

# <a name="implementing-azure-active-directory"></a>Implementieren von Azure Active Directory

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Best Practices für die Integration von lokalen Active Directory (AD) Domänen und Gesamtstrukturen in Active Directory für Cloud-basierte Identitätsauthentifizierung Azure beschrieben.

> [AZURE.NOTE] Azure hat zwei verschiedene Bereitstellungsmodelle: [Ressourcen-Manager] [ resource-manager-overview] und classic. Diese Referenzarchitektur verwendet Ressourcen-Manager für die Bereitstellung neuer Microsoft empfiehlt.

Verzeichnis und Identität Dienste wie Active Directory-Verzeichnisdienste (AD DS) Identitäten zu authentifizieren können. Diese Identitäten können Benutzer, Computer, Anwendung oder anderen Ressourcen, die Teil einer Sicherheitsgrenze angehören. Verzeichnis host und Identität für lokale Dienste, aber im Hybrid-Szenario Elemente einer Anwendung in Azure befinden möglich effizienter, diese Funktionalität in die Cloud erweitern. Dieser Ansatz kann verringern die Verzögerung durch das Senden von Authentifizierung und lokale Autorisierungsanfragen aus der Cloud auf die unter lokalen Verzeichnis und Identität zurück.

Azure bietet zwei Projektmappen für die Implementierung von Verzeichnis-und Identität in der Cloud:

1. [Azure Active Directory (AAD)] können[ azure-active-directory] AD-Domäne in der Cloud erstellen und verknüpfen Sie es mit einem lokalen Active Directory-Domäne. AAD können Sie einmaliges Anmelden (SSO) konfigurieren für Benutzer Zugriff über die Cloud Applications. AAD nutzt [Azure AD Connect] [ azure-ad-connect] lokal repliziert Objekte und Identitäten ausgeführt in die Cloud im lokalen Repository gespeichert.

    AAD realisieren ohne lokalen Verzeichnis. In diesem Fall fungiert AAD als primäre Quelle für alle Identitätsinformationen statt mit Daten aus einem lokalen Verzeichnis repliziert.

    Beachten Sie, dass das Verzeichnis in der Cloud **** keine Erweiterung von einem lokalen Verzeichnis. Stattdessen wird eine Kopie mit dem gleichen Objekte und Identitäten. Auf diese Räume Elemente werden zur Cloud kopiert, aber Änderungen in der Cloud **nicht** in der lokalen Domäne repliziert werden.

    >[AZURE.NOTE] Azure Active Directory Premium-Editionen unterstützen Rückschreib Benutzerkennwörter, die lokalen Benutzer Kennwörtern Self-service in der Cloud ausgeführt. Dies ist die einzige Information, die an das lokale Verzeichnis AAD synchronisiert. Weitere Informationen über die verschiedenen Editionen von AAD und ihre Features finden Sie unter [Azure Active Directory Editionen][aad-editions].

2. Sie können eine VM mit AD DS als Domänencontroller in Azure, Erweiterung der vorhandenen AD-Infrastruktur (einschließlich AD DS und AD FS) von lokalen Datencenter bereitstellen. Dieser Ansatz ist für Szenarios, in dem lokalen Netzwerk und Azure virtual Network VPN- oder ExpressRoute-Verbindung verbunden sind. Diese Lösung unterstützt auch die bidirektionale Replikation aktivieren Sie Änderungen in der Cloud und lokal am besten geeignet ist.

Diese Architektur Schwerpunkt Lösung 1. Weitere Informationen über die zweite Lösung finden Sie unter [Erweitern von Active Directory in Azure][guidance-adds].

Normale Anwendungsfälle für diese Architektur gehören:

- Bereitstellen von SSO für Benutzer von SaaS und anderen Programmen in der Cloud lokalen Applikationen über dieselben Anmeldeinformationen, die sie angeben.

- Veröffentlichung lokalen ASP.NET-Webanwendungen Cloud Remotebenutzer zugreifen, die zu Ihrer Organisation gehören.

- Implementieren von Selbsthilfefunktionen für Endbenutzer ihre Kennwörter zurücksetzen, wie delegieren Gruppenmanagement. 

    >[AZURE.NOTE] Diese Funktionen können von einem Administrator über die Gruppenrichtlinie gesteuert werden.

- Situationen, in dem lokalen Netzwerk und Azure virtuelle Netzwerk Cloudanwendungen nicht direkt verknüpft sind mit einem VPN-Tunnel oder ExpressRoute-Verbindung.

Beachten Sie, dass AAD nicht alle Funktionen von Active Directory. Beispielsweise verarbeitet AAD derzeit nur Authentifizierung als Authentifizierung. Einige Programme und Dienste wie SQL Server-Authentifizierung erfordern die Lösung nicht geeignet ist. Darüber hinaus möglicherweise AAD nicht nur kopiert, sondern für Systeme, Komponenten über die Grenze lokal und Cloud migriert werden konnte.

> [AZURE.NOTE] Eine ausführliche Erläuterung der Funktionsweise von Azure Active Directory überwachen [Microsoft Active Directory erläutert][aad-explained].

## <a name="architecture-diagram"></a>Architekturdiagramm

Das folgende Diagramm zeigt wichtigen Komponenten in dieser Architektur. Lesen Sie weitere Informationen zu den Elementen Arbeitslast in Azure [VMs für N-Tier-Architektur in Azure ausgeführt][implementing-a-multi-tier-architecture-on-Azure]:

> [AZURE.NOTE] Zur Vereinfachung dieses Diagramm nur unmittelbar AAD zeigt und Web Browser Anforderung leitet nicht zeigen oder andere Protokoll Datenverkehr, die als Teil der Authentifizierung und Verbund auftreten. Z. B. ein Benutzer (lokal oder Remote) greift normalerweise auf Web app über einen Browser und Web app möglicherweise transparent Webbrowser zum Authentifizieren der Anforderung durch AAD leiten. Nach der Authentifizierung kann die Anforderung an Web app mit entsprechenden Identitätsinformationen übergeben.

[! [0]][0]

- **Azure Active Directory Mieter**. Dies ist eine Instanz der AAD von Ihrer Organisation erstellt. Sie fungiert als einfaches Verzeichnisdienst für Cloudanwendungen (enthält Objekte, die lokal kopiert AD), und Identitätsdienste.

- **Web-Tier-Subnetz**. Diesem Subnetz enthält VMs, die implementieren eine benutzerdefinierte Cloud-basierte Anwendung von Ihrer Organisation und für die AAD als Vermittler Identität fungieren kann.

- **Für lokale AD DS-Server**. Ihre Organisation wahrscheinlich bereits einen vorhandenen Verzeichnisdienst wie AD DS. Sie können viele Elemente in AD DS Verzeichnis (z. B. Benutzer- und Gruppeninformationen) AAD AAD mit diesen Informationen zur Authentifizierung von Identitäten aktivieren synchronisieren.

- **Azure AD Connect Sync-Server**. Dies ist eine lokale Computer unter Synchronisierungsdienst Azure AD verbinden. Dieser Dienst installieren mithilfe der Azure AD Connect-Software. Azure AD Connect-Synchronisierungsdienst synchronisiert Informationen (Benutzer, Gruppen, Kontakte usw.) in der lokalen Anzeige AAD in der Cloud. Beispielsweise bereitstellen oder Entziehen von Gruppen und Benutzern auf lokalen und diese Änderungen an AAD. Azure AD Connect-Synchronisierungsdienst übergibt Informationen an AAD-Mandanten.

    >[AZURE.NOTE] Aus Sicherheitsgründen werden Benutzerkennwörter nicht direkt in AAD gespeichert. Vielmehr enthält AAD einen Hash jedes Kennwort. Dies ist ausreichend, um das Kennwort eines Benutzers überprüfen. Wenn ein Benutzer das Zurücksetzen eines Kennworts erforderlich ist, ist vor Ort durchgeführt und den neuen Hash AAD gesendet. AAD Premium-Editionen enthalten Features, die diese Aufgabe Benutzer ihre eigenen Kennwörter zurücksetzen automatisieren können.

## <a name="recommendations"></a>Empfehlung

In diesem Abschnitt werden Vorgehensweisen für die Implementierung AAD folgendermaßen zusammengefasst.

- Active Directory herstellen
- Sicherheit

### <a name="azure-ad-connect-sync-service-recommendations"></a>Azure AD Connect Sync Service empfohlen

Synchronisierung geht, dass Benutzer Identitätsinformationen in der Cloud mit Räumen. Der Synchronisierungsdienst Azure AD verbinden soll diese Konsistenz. Die folgenden Punkte fassen für die Implementierung von Azure AD Connect-Synchronisierungsdienst:

- Vor dem Implementieren von Azure AD Connect synchronisieren, ermitteln die Synchronisation an Ihre Organisation (was synchronisieren, welche Domänen und wie oft. Artikel [bestimmen Directory Synchronization erforderlich] [ aad-sync-requirements] beschreibt die Punkte, die Sie berücksichtigen sollten.

- Synchronisierungsdienst Azure AD-Verbindung mit einem virtuellen Computer ausführen oder ein Computer lokal gehostet werden. Je nach Volatilität Informationen im Active Directory-Verzeichnis nach die erste Synchronisierung mit AAD durchgeführt wurde dürfte die Belastung Azure AD Connect-Synchronisierungsdienst hoch. Mithilfe einer VM können Sie problemlos skalieren Server (Überwachen der Aktivität gemäß Abschnitt [Aspekte der Überwachung](#monitoring-considerations) feststellen, ob dies erforderlich ist).

- Haben Sie mehrere lokale Domänen in einer Gesamtstruktur können Sie speichern und Synchronisieren von Informationen für die Gesamtstruktur eine einzelne AAD Mieter (Dies ist die empfohlene Vorgehensweise). Filterinformationen Sie für die Identitäten, die in mehr als einer Domäne, so dass jede Identität wird nur einmal in AAD als dupliziert werden, da dies zu Inkonsistenzen führen kann, wenn Daten synchronisiert werden. Dieser Ansatz erfordert mehrere Azure AD Connect Sync-Server implementieren. Weitere Informationen siehe Abschnitt [Aspekte der Topologie](#topology-considerations) mehrere AAD. 

- Verwenden Sie Filter einschränken in AAD auf gespeicherte Daten erforderlich ist. Beispielsweise könnte Ihre Organisation nicht Konten inaktiv oder nicht-persönliche Informationen in AAD speichern möchten. Filterung kann Gruppenbasierte domänenbasierten, OU-basierte oder Attribut-basierte, und kombinieren Sie Filter, um komplexere Regeln erstellen. Sie könnten z. B. nur Objekte in einer Domäne synchronisieren, die einen bestimmten Wert in einem ausgewählten Attribut auswählen. Ausführliche Informationen finden Sie unter [Azure AD-Verbindung synchronisieren: Filter konfigurieren][aad-filtering].

- Implementieren Sie hohen Verfügbarkeit für AD Connect-Synchronisierungsdienst führen Sie einen sekundären Stagingserver. Weitere Informationen finden Sie im Abschnitt [Hinweise Topologie](#topology-considerations) .

### <a name="security-recommendations"></a>Sicherheitsaspekte

Die folgenden Elemente Zusammenfassen der primären Sicherheitsaspekte für AAD, je nach Ihrer Organisation implementieren:

- Verwalten von Benutzern.

    Bei Verwendung eine Premium Edition AAD können Sie Kennwort Zurückschreiben von AAD zum lokalen Verzeichnis durch Ausführen einer benutzerdefinierten Installation von Azure AD Connect aktivieren:

    [! [9]][9]

    Dieses Feature ermöglicht es Benutzern, ihre eigenen Kennwörter innerhalb von Azure-Portal zurücksetzen, aber nur nach Prüfung Ihrer Organisation Kennwortsicherheitsrichtlinie aktiviert werden soll. Beispielsweise können Sie einschränken, welche Benutzer ihre Kennwörter ändern können und die Kennwort Erfahrung anzupassen. Weitere Informationen finden Sie unter [Password Management an die Bedürfnisse Ihres Unternehmens anpassen][aad-password-management].

- Verwalten von Schutz für lokale Applikationen, die extern zugegriffen werden kann.

    Verwenden der Azure AD-Anwendungsproxy den gesteuerten Zugriff auf lokale ASP.NET-Webanwendungen von externen Benutzern über AAD. Dieser Ansatz schützt Ihre Anwendung von nicht direkt mit dem Internet verfügbar machen. Nur Benutzer mit gültigen Anmeldeinformationen in der Azure-Verzeichnis sind zu Ihrer Anwendung. Weitere Informationen finden Sie im Artikel [Anwendungsproxy in Azure-Portal aktivieren][aad-application-proxy].

- Aktive Überwachung AAD nach Anzeichen für verdächtige Aktivitäten.

    Erwägen Sie AAD Premium P2 Edition AAD Schutz umfasst. Schutz wird adaptive maschinelles lernalgorithmen und Heuristiken Anomalien erkennen und Ereignisse, die eine Identität kompromittiert werden möglicherweise verwendet. Beispielsweise erkennt es möglicherweise anomale Aktivität wie unregelmäßige anmelden, Anmelden aus unbekannten Quellen oder verdächtige IP-Adressen oder Anmeldungen von Geräten, die infiziert werden kann. Schutz mit diesen Daten generiert werden, Berichten und Alarmen, mit dem Sie diese Risikoereignisse untersuchen und geeignete Wiederherstellung und Minderungsaktion. Weitere Informationen finden Sie unter [Azure Active Directory Schutz][aad-identity-protection].

    Die Berichtsfunktion von AAD im Azure-Portal können Sie verdächtige und andere sicherheitsrelevante Aktivitäten in Ihrem System überwachen. AAD kann zusammenfassende Berichte generieren:

    [! [17]][17]

    Weitere Informationen über diese Berichte finden Sie unter [Azure Active Directory Berichtshandbuch][aad-reporting-guide].

## <a name="topology-considerations"></a>Aspekte der Topologie

Integration von einem lokalen Verzeichnis mit AAD konfigurieren Azure AD verbinden, um eine Topologie zu implementieren, die Ihre Organisation entspricht. Die folgenden: Topologien Azure AD Connect unterstützt

- **Gesamtstruktur, AAD Verzeichnis**. In dieser Topologie mit Azure AD Connect Objekte synchronisiert werden und Identitätsinformationen in einer oder mehreren Domänen in einer lokalen Gesamtstruktur mit einer einzigen AAD-Mandanten. Dies ist die Standardtopologie Schnellinstallation Azure AD Connect implementiert.

    [! [1]][1]

    Erstellen mehrerer Azure AD Connect Synchronisierungsserver gleichen AAD Mieter verschiedene Domänen in derselben Gesamtstruktur lokale Verbindung, wenn Sie ein Azure AD Connect Synchronisierungsserver im staging-Modus ausführen (siehe unten Staging-Server).

- **Mehrere Gesamtstrukturen, AAD Verzeichnis**. Verwenden Sie diese Topologie mehrere lokale Gesamtstruktur haben. Identitätsinformationen können konsolidiert werden, um jeden einzelnen Benutzer einmal im Verzeichnis AAD dargestellt wird, selbst wenn derselbe Benutzer in mehr als einer Gesamtstruktur vorhanden ist. Alle Gesamtstrukturen verwenden dieselbe Azure AD Connect Sync-Server. Azure AD Connect Sync-Server muss nicht Teil einer Domäne sein, jedoch muss aus allen Gesamtstrukturen erreicht:

    [! [2]][2]

    In dieser Topologie keine separate Azure AD Connect verwenden Server eine AAD-Mandanten jeder Gesamtstruktur lokale Verbindung synchronisieren. Können dadurch doppelte Identitätsinformationen in AAD Benutzern in mehreren Gesamtstrukturen vorhanden sind.

- **Mehrere Gesamtstrukturen, separate Topologien**. Dadurch können Sie einzelne AAD Mieter Identitätsinformationen aus mehreren Gesamtstrukturen zusammenführen. Diese Strategie ist sinnvoll, wenn Gesamtstrukturen aus verschiedenen Organisationen (nach einer Fusion oder Übernahme z. B.) kombinieren und Identitätsinformationen für jeden Benutzer nur in einer Gesamtstruktur findet:

    GALs in jeder Gesamtstruktur synchronisiert sind, kann ein Benutzer in einer Gesamtstruktur in anderen als Kontakt vorhanden sein. Dies kann auftreten, wenn beispielsweise Ihre Organisation GALSync Forefront Identity Manager 2010 oder Microsoft Identity Manager 2016 implementiert hat. In diesem Szenario können Sie angeben, dass Benutzer mit ihrer *e-Mail-* Attribut bestimmt werden soll. Sie können auch die *ObjectSID* *MsExchMasterAccountSID* Attribute und Identitäten anpassen. Dies ist nützlich, wenn eine oder mehrere Ressourcengesamtstrukturen mit deaktivierten Konten.

- **Staging-Server**. In dieser Konfiguration wird eine zweite Instanz von Azure AD Connect Sync-Server mit der ersten parallel ausgeführt. Diese Struktur unterstützt Szenarios wie:

    - Hohe Verfügbarkeit.

    - Testen und Bereitstellen einer neuen Konfigurations von Azure AD Connect Sync-Server.

    - Einführung eines neuen Servers und Stilllegung einer älteren Konfigurations. 

    In diesen Fällen wird die zweite Instanz der *Modus*ausgeführt. Der Server zeichnet importierte Objekte und von Synchronisierungsdaten in der Datenbank aber übergibt die Daten an AAD. Nur beim staging Modus deaktivieren startet der Server Daten AAD und startet ausführen Kennwort Write-in lokalen Verzeichnissen gegebenenfalls rücken geschrieben:

    [! [4]][4]

    Weitere Informationen finden Sie unter [Azure AD-Verbindung synchronisieren: Aufgaben und Aspekte der][aad-connect-sync-operational-tasks].

- **Mehrere AAD Verzeichnisse**. Es wird empfohlen, ein einzelnes Verzeichnis AAD für eine Organisation erstellen, aber es gibt möglicherweise Situationen, in denen Sie Informationen über separate AAD Verzeichnisse müssen. In diesem Fall sollten Sie sicherstellen, jedes Objekt in der lokalen Gesamtstruktur nur in einem AAD Verzeichnis zu Synchronisierung und Kennwort Rückschreib Probleme angezeigt.

    Zur Implementierung dieses Szenarios konfigurieren Sie separate Azure AD Connect Server für jedes Verzeichnis AAD synchronisieren und Filtern so gegenseitig Objektgruppe jede Azure AD Connect Synchronisierungsserver verarbeitet: 

    [! [5]][5]

Weitere Informationen zu diesen Topologien finden Sie unter [Topologien für Azure AD Connect][aad-topologies].

## <a name="user-sign-in-considerations"></a>Benutzer anmelden Aspekte

Standardmäßig nimmt AAD Service Anmelden der Benutzer das gleiche Kennwort für lokale Verwendung und Azure AD Connect Synchronisierungsserver Kennwortsynchronisation zwischen der lokalen Domäne und AAD konfiguriert. Für viele Unternehmen dies, aber sollten Sie Richtlinien und Infrastruktur Ihres Unternehmens. Zum Beispiel:

- Die Sicherheitsrichtlinien Ihrer Organisation möglicherweise untersagen Kennworthashes in die Cloud synchronisieren

- Sie müssen ggf. treten nahtlose SSO (ohne zusätzliches Kennwort aufgefordert werden) bei Domäne Cloudressourcen zugreifen Computer im Unternehmensnetzwerk.

- Ihre Organisation möglicherweise bereits ADFS oder einem Drittanbieter Föderation Anbieter bereitgestellt. AAD um diese Infrastruktur zum Implementieren der Authentifizierung und SSO nicht mit Kennwortinformationen in der Cloud verwenden können.

Weitere Informationen finden Sie in [Azure AD verbinden Benutzer anmelden Optionen][aad-user-sign-in].

## <a name="azure-ad-application-proxy-considerations"></a>Azure AD Anwendung Proxy Aspekte

Azure AD Zugriff auf lokale Programme zu verwenden.

- Setzen der lokalen Web Applications Anwendungsproxy verwaltenden Connectors von Azure AD Anwendung Proxy-Komponente verwenden. Der Application Connector Proxy öffnet eine ausgehende Verbindung zu Azure AD-Anwendungsproxy. Remotebenutzer Anfragen werden von AAD über diese Verbindung zum Web apps weitergeleitet. Dieser Mechanismus wird auf eingehenden Ports in der lokalen Firewall Verringern der Angriffsfläche von Ihrer Organisation verfügbar gemacht werden.

Weitere Informationen finden Sie unter [Veröffentlichen Applications Azure AD-Anwendungsproxy mit][aad-application-proxy].

## <a name="object-synchronization-considerations"></a>Objekt-Synchronisations-Hinweise

Die Standardkonfiguration von Azure AD Connect synchronisiert Objekte aus Ihrem lokalen AD-Verzeichnis anhand der Regeln gemäß Artikel [Azure AD-Verbindung synchronisieren: Verstehen der Standardkonfiguration][aad-connect-sync-default-rules]. Nur Objekte, die diese Regeln werden synchronisiert, andere ignoriert erfüllen. Z. B. Benutzerobjekte müssen eindeutige *SourceAnchor* -Attribut und das *AccounEnabled* -Attribut aufgefüllt werden muss. Benutzerobjekte, die keinen *sAMAccountName* -Attribut oder *AAD_* oder *MSOL_* mit dem Text beginnen, werden nicht synchronisiert. Azure AD verbinden mehrere Regeln auf Benutzer-, Kontakt, Gruppe, ForeignSecurityPrincipal und Objekte angewendet wird. Benötigen Sie den Standardsatz von Regeln ändern, verwendet der Regeln Synchronisierung mit Azure AD Connect installiert (auch dokumentiert [Azure AD-Verbindung synchronisieren: Verstehen der Standardkonfiguration][aad-connect-sync-default-rules]).

Sie können eigene Filter einschränken, die Domäne oder Organisationseinheit synchronisiert werden. Oder komplexere benutzerdefinierte Filterung gemäß [Azure AD-Verbindung synchronisieren: Filter konfigurieren][aad-filtering].

## <a name="security-considerations"></a>Sicherheitsaspekte

Verwenden Sie bedingter Authentifizierungsanfragen von unerwarteten Quellen zu verweigern:

- Auslösen von [Azure mehrstufige Authentifizierung (MFA)] [ azure-multifactor-authentication] Wenn ein Benutzer versucht, die Verbindung von einem nicht vertrauenswürdigen Speicherort (z. B. über das Internet) vertrauenswürdiges Netzwerk.

- Verwenden Sie Gerät Plattform des Benutzers (iOS, Android, Windows Mobile, Windows), Zugriff auf Programme und Funktionen bestimmt.

- Zeichnen Sie aktivierten/deaktivierten Zustand von Benutzercomputern, und binden Sie diese Informationen in Access Richtlinie überprüft. Beispielsweise bei Verlust oder Diebstahl Telefon des Benutzers sollte es aufgezeichnet werden als deaktiviert von Zugriff verwendet wird.

- Die Zugriffsebene für einen Benutzer basierend auf der Gruppenmitgliedschaft zu steuern. Verwenden Sie [dynamische Mitgliedschaftsregeln AAD] [ aad-dynamic-membership-rules] gruppenverwaltung vereinfachen. Eine kurze Übersicht über die Funktionsweise dieser finden Sie unter [Einführung in dynamische Mitgliedschaft für Gruppen][aad-dynamic-memberships].

- Verwenden Sie bedingte Risiko-Richtlinien mit AAD Schutz erweiterter Schutz basierend auf ungewöhnliche Aktivitäten oder Ereignisse.

Weitere Informationen finden Sie unter [bedingte Azure Active Directory Access][aad-conditional-access].

## <a name="scalability-considerations"></a>Aspekte der Skalierung

Skalierung ist AAD Service und die Konfiguration von Azure AD Connect Synchronisierungsserver behandelt:

- Für den Dienst AAD müssen Sie keinen Skalierbarkeit implementieren Optionen konfigurieren. Der Dienst AAD unterstützt Skalierbarkeit basierend auf Replikate. AAD implementiert eine einzelne primäre Replikat die Handles Schreibvorgänge und mehrere schreibgeschützte sekundäre Replikate. AAD leitet transparent versucht Schreibvorgänge für sekundäre Replikate auf primäres Replikat. AAD bietet hin. Alle Änderungen primäres Replikat werden sekundäre Replikate weitergegeben. Wie die meisten Vorgänge AAD liest als schreibt, wird auch diese Architektur skaliert.

    Weitere Informationen finden Sie unter [Azure AD: Nähkästchen unser cloudverzeichnis Geo redundante, hochverfügbare, verteilte][aad-scalability].

- Für Azure AD Connect Sync-Server sollten Sie ermitteln, wie viele Objekte in Ihrem lokalen Verzeichnis wahrscheinlich synchronisiert werden. Wenn Sie weniger als 100.000 Objekte verfügen, können Sie die standardmäßige SQL Server Express LocalDB Software mit Azure AD verbinden. Haben Sie eine größere Anzahl von Objekten, installieren Sie eine Produktionsversion von SQL Server und führen eine benutzerdefinierte Installation von Azure AD Connect angeben, dass eine vorhandene Instanz von SQL Server verwenden soll.

## <a name="availability-considerations"></a>Aspekte der Verfügbarkeit

Mit Skalierbarkeit Aspekte umfasst Verfügbarkeit AAD Service und der Konfiguration von Azure AD verbinden:

- Der Dienst AAD soll hohen Verfügbarkeit. Es gibt keine konfigurierbare Verfügbarkeitsoptionen. Es ist geografisch verteilten und führt in mehreren Verbreitung weltweit, automatisches Failover. Wenn ein Rechenzentrum nicht verfügbar ist, sorgt AAD die Verzeichnisdaten für Instanz auf mindestens zwei weitere Regional dezentralisierten Rechenzentren.

    >[AZURE.NOTE] Die SLA für AAD Basic und Premium Services Garantien mindestens 99,9 % Verfügbarkeit. Es besteht kein DIENSTVERTRAG für den freien Tier AAD. Weitere Informationen finden Sie unter [SLA für Azure Active Directory][sla-aad].

- Zum Verbessern der Verfügbarkeit von Azure AD Connect Sync-Server führen eine zweite Instanz Sie im staging-Modus gemäß Abschnitt [Aspekte der Topologie](#topology-considerations) . 

    Außerdem, wenn Sie nicht die SQL Server Express LocalDB-Instanz, die mit Azure AD verbinden verwenden, sollten dann hohen Verfügbarkeit für SQL Server Sie. Beachten Sie, dass die Lösung nur hohe Verfügbarkeit unterstützt SQL-clustering. Solutions Spiegelung und ständig nicht Azure AD-Verbindung unterstützt.

    Finden Sie weitere Hinweise zur Verfügbarkeit von Azure AD Connect Synchronisierungsserver und Wiederherstellung nach einem Ausfall des [Azure AD-Verbindung synchronisieren: Aufgaben und Aspekte - Disaster Recovery][aad-sync-disaster-recovery].

## <a name="management-considerations"></a>Gesichtspunkte

Es gibt zwei Aspekte AAD verwalten:

1. Verwalten von AAD in der Cloud.

2. Verwalten von Azure AD Connect Synchronisierungsserver.

AAD bietet die folgenden Optionen zum Verwalten von Domänen und Verzeichnisse in der Cloud:

- [Azure Active Directory PowerShell-Modul][aad-powershell]. Verwenden Sie dieses Skript Azure AD Verwaltungsaufgaben wie Benutzer, Verwaltung und einmaliges Anmelden konfigurieren möchten.

- Azure AD Management Blade im Azure-Portal. Dieses Blatt bietet eine interaktive Management-Ansicht des Verzeichnisses und ermöglicht und die meisten Aspekte der AAD.

    [! [10]][10]

Azure AD Connect installiert die folgenden Tools Azure AD Connect Synchronisierungsdienste von Ihrem lokalen Computer verwalten:

- Die Konsole Microsoft Azure Active Directory herstellen. Dieses Tool können Sie die Konfiguration von Azure AD Sync-Server ändern, anpassen, wie Synchronisierung aktivieren oder staging-Modus deaktivieren und Modus die Benutzer anmelden (Sie AD FS Anmelden mit Ihrer lokalen Infrastruktur können).

- Synchronisierung Service Manager. Verwenden Sie die Registerkarte *Vorgänge* in diesem Tool verwalten die Synchronisierung und ermitteln, ob Teile des Prozesses fehlgeschlagen. Sie können manuell mit diesem Tool Synchronisationen auslösen. 

    [! [12]][12]

    Die Registerkarte *Connectors* können Sie die Verbindung für die Domänen steuern (lokal und in der Cloud), das Synchronisierungsmodul zugeordnet ist:

    [! [13]][13]

-  Synchronisierung Regel-Editor. Verwenden Sie dieses Tool, wie die Objekte, beim Kopieren zwischen lokalen Verzeichnis als AAD in der Cloud transformiert werden. Mit diesem Tool können Sie weitere Attribute und Objekte für die Synchronisation angeben und Filter bestimmen, welche Instanzen sollten oder nicht synchronisiert werden.

    Weitere Informationen finden Sie im Abschnitt Synchronisation Regeleditor im Dokument [Azure AD-Verbindung synchronisieren: Verstehen der Standardkonfiguration][aad-connect-sync-default-rules].

Die Seite [Azure AD-Verbindung synchronisieren: Best Practices für die Konfiguration ändern] [ aad-sync-best-practices] enthält weitere Informationen und Tipps zum Verwalten von Azure AD verbinden.

## <a name="monitoring-considerations"></a>Aspekte der Überwachung

Überwachung erfolgt durch eine Reihe von lokal installierten Agents:

- Azure AD Connect installiert einen Agent, der Informationen über Synchronisationen erfasst. Verwenden Sie Azure Active Directory verbinden Gesundheit Blade im Azure-Portal zur Überwachung von Zustand und Leistung von Azure AD Connect. Weitere Informationen finden Sie unter [Mithilfe von Azure AD Connect Health Synchronisierung][aad-health].

- Zum Überwachen der Active Directory-Domänen und Verzeichnisse von Azure installieren Sie die Azure AD Connect Health für AD DS-Agent auf einem Computer innerhalb der lokalen Domäne. Verwenden Sie Blade Azure Active Directory verbinden Health Monitor AD DS Azure-Portal. Weitere Informationen finden Sie unter [Verwendung von Azure AD Connect Health in AD DS][aad-health-adds]

- Installieren der Azure AD Connect Health AD FS-Agent Überprüfen des AD FS auf lokalen und Azure Active Directory verbinden Gesundheit Blade im Azure-Portal verwenden, um AD DS zu überwachen. Weitere Informationen finden Sie unter [Verwendung von Azure AD Connect Health mit AD FS][aad-health-adfs]

Weitere Informationen zum Installieren von AD Connect Health Agents und Bedürfnisse finden Sie unter [Azure AD verbinden Health Agent Installation][aad-agent-installation].

## <a name="sample-solution"></a>Probe

Dieser Abschnitt beschreibt die Schritte zum Erstellen einer beispiellösung, mit denen Sie die Konfiguration des AAD testen. Die Lösung umfasst die folgenden Elemente:

- Eine mehrstufige Webanwendung der Struktur von [VMs für N-Tier-Architektur in Azure ausgeführt]beschrieben[implementing-a-multi-tier-architecture-on-Azure].

- Client-Testcomputer. Wir empfehlen eine andere VM für diesen Computer. Die Konfiguration dieser VM ist unwichtig, Sie verfügen über Administratorrechte und können mit n-Tier-Anwendung verbinden.

- Eine simulierte lokalen Umgebung, die eigene Domäne mit AD DS erstellt.

Die folgenden Schritte demonstrieren Szenarien sind:

- Zugriff auf mit in die Cloud für externe Benutzer mit Kennwortauthentifizierung AAD mehrstufige Web-Anwendung.

- Zugriff auf der n-Tier Webapplikation AAD Kennwort Authentifizierung innerhalb der Organisation mit Benutzern in der Cloud ausgeführt.

### <a name="prerequisites"></a>Erforderliche Komponenten

Die Schritte angenommen, die folgenden Komponenten:

- Sie haben ein Azure-Abonnement Sie Ressourcengruppen erstellen können.

- Der [Azure-Befehlszeilenschnittstelle]installiert[azure-cli].

- Heruntergeladen und installiert den neuesten Build. Finden Sie [hier] [ azure-powershell-download] Informationen.

- Installiert eine Kopie des [Makecert] [ makecert] auf dem Client-Utility.

- Sie haben Zugriff auf eine Entwicklungscomputer mit Visual Studio installiert.

### <a name="create-the-n-tier-web-application-architecture"></a>Die Anwendungsarchitektur mehrstufige Web erstellen

1. Erstellen Sie einen Ordner mit dem Namen `Scripts` enthält einen Unterordner namens `Parameters`.

2. Download [Bereitstellen ReferenceArchitecture.ps1] [ solution-script] PowerShell-Skript zum Ordner "Scripts".

3. Erstellen Sie im Ordner Parameter einen weiteren Unterordner mit dem Namen Windows.

4. Die folgenden Dateien in Parameter-Windows-Ordner herunter

    - [virtualNetwork.parameters.json][vnet-parameters-windows]

    - [networkSecurityGroup.parameters.json][nsg-parameters-windows]

    - [webTierParameters.json][webtier-parameters-windows]

    - [businessTierParameters.json][businesstier-parameters-windows]

    - [dataTierParameters.json][datatier-parameters-windows]

    - [managementTierParameters.json][managementtier-parameters-windows]

5. Bearbeiten der Datei **Deploy-ReferenceArchitecture.ps-**1 in den Skriptordner, und ändern Sie folgende Zeile, um die Ressourcengruppe angeben, die erstellt oder die VM und Ressourcen vom Skript verwendet werden:

        ```powershell
        # PowerShell
        $resourceGroupName = "ra-aad-ntier-rg"
        ```

6. Die folgenden Dateien in den Ordner **Fenster Parameter**bearbeiten und legen die `resourceGroup` Wert der `virtualNetworkSettings` Abschnitt dieser Dateien identisch sein, die in der Skriptdatei **Bereitstellen ReferenceArchitecture.ps1** angegeben.

    - networkSecurityGroup.parameters.json

    - webTierParameters.json

    - businessTierParameters.json

    - dataTierParameters.json

    - managementTierParameters.json

7. Öffnen Sie ein Azure PowerShell-Fenster, wechseln Sie zum Ordner Skripts und führen Sie folgenden Befehl:

        ```powershell
        .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Windows ntier
        ```

    Ersetzen Sie **<subscription id>** mit Ihrer Azure-Abonnement-ID

    Für **<location>**, eine Azure-Region, *Eastus* oder *Westus*angeben.

8. Nach Abschluss des Skripts mit der Azure-Portal die öffentliche IP-Adresse des Web-Tier-Lastenausgleich (*Ra-Aad-Ntier-Web-lb*):

    [! [18]][18]

9. Melden Sie sich zu Ihrem Test Client Computer (VM) und überprüfen Sie mithilfe von Internet Explorer auf die öffentlichen IP-Adresse des Web-Tier-Lastenausgleich die Webebene zugreifen können. Die Standardseite für IIS sollte angezeigt werden.

### <a name="simulate-configuration-of-a-public-web-site"></a>Simulieren der Konfiguration einer öffentlichen Website

>[AZURE.NOTE] Die folgenden Schritte simulieren den DNS-Namen www.contoso.com durch Bearbeiten der Hostdatei auf dem Clientcomputer die Webebene zuordnen. Darüber hinaus sind die Webebene VMs selbstsignierte Zertifikate mit hosting Behörde falsch konfiguriert. Dies ist für nur und **nicht in einer dieser Techniken verwenden soll**.

1. Auf dem Clientcomputer mit dem Editor bearbeiten Sie die Datei **C:\Windows\System32\drivers\etc\hosts** , und fügen Sie einen Eintrag, der die öffentliche IP-Adresse des Web-Tier-Lastenausgleich des www.contoso.com zugeordnet:

        ```text
        # Copyright (c) 1993-2009 Microsoft Corp.
        #
        # This is a sample HOSTS file used by Microsoft TCP/IP for Windows.
        #
        # This file contains the mappings of IP addresses to host names. Each
        # entry should be kept on an individual line. The IP address should
        # be placed in the first column followed by the corresponding host name.
        # The IP address and the host name should be separated by at least one
        # space.
        #
        # Additionally, comments (such as these) may be inserted on individual
        # lines or following the machine name denoted by a '#' symbol.
        #
        # For example:
        #
        #      102.54.94.97     rhino.acme.com          # source server
        #       38.25.63.10     x.acme.com              # x client host
        
        # localhost name resolution is handled within DNS itself.
        #   127.0.0.1       localhost
        #   ::1             localhost
        
        52.165.38.64    www.contoso.com
        ```

2. Stellen Sie sicher, dass Sie jetzt von dem Testclientcomputer www.contoso.com wechseln können. Die gleiche IIS-Standardseite sollte wie zuvor angezeigt werden.

3. Der Clientcomputer ein Eingabeaufforderungsfenster als Administrator, und verwenden Sie das Dienstprogramm Makecert c
4. Erstellen eine gefälschte Stammzertifizierungsstellen-Zertifikat:

        ```
        makecert -sky exchange -pe -a sha256 -n "CN=MyFakeRootCertificateAuthority" -r -sv MyFakeRootCertificateAuthority.pvk MyFakeRootCertificateAuthority.cer -len 2048
        ```

    Stellen Sie sicher, dass die folgenden Dateien erstellt werden:

    - MyFakeRootCertificateAuthority.cer

    - MyFakeRootCertificateAuthority.pvk

4. Führen Sie den folgenden Befehl an die Test-Stammzertifizierungsstelle installieren:

        ```
        certutil.exe -addstore Root MyFakeRootCertificateAuthority.cer
        ```

5. Test-Zertifizierungsstelle ein Zertifikat für www.contoso.com zu verwenden:

        ```
        makecert -sk pkey -iv MyFakeRootCertificateAuthority.pvk -a sha256 -n "CN=www.contoso.com" -ic MyFakeRootCertificateAuthority.cer -sr localmachine -ss my -sky exchange -pe
        ```

6. Führen Sie den Befehl **Mmc** , und fügen Sie das Zertifikate-Snap-in für das Computerkonto für den lokalen Computer.

7. In der *Zertifikatinhaber (lokaler Computer) / Personal/Zertifikat/* speichern, exportieren Sie das Zertifikat www.contoso.com mit seinem privaten Schlüssel in einer Datei namens www.contoso.com.pfx:

    [! [20]][20]

8. Azure-Portal zurück und Verwaltungsebene VM (*Ra-Aad-Ntier-Management-vm1*) an. Der Standardbenutzername ist *Testbenutzer* mit Kennwort *AweS0me@PW*:

    [! [21]][21]
    
9. Verbinden mit jeder der Webebene VMs wiederum mit Benutzernamen *Testuser* Kennwort Verwaltungsebene VM *AweS0me@PW*, und führen Sie die folgenden Aufgaben. Beachten Sie, dass die VMs private Adressen IP 10.0.1.4, 10.0.1.5 und 10.0.1.6:

    >[AZURE.NOTE] Die Webebene VMs nur private IP-Adressen haben. Sie können nur mithilfe der Verwaltungsebene VM Sie verbinden.

    1. Kopieren Sie die Dateien *www.contoso.com.pfx* und *MyFakeRootCertificateAuthority.cer* aus dem Testclientcomputer.
    
    2. Öffnen Sie ein Eingabeaufforderungsfenster als Administrator, wechseln Sie zum Ordner mit den Dateien, die Sie kopiert und führen Sie die folgenden Befehle:
    
        ```
        certutil.exe -privatekey -importPFX my www.contoso.com.pfx NoExport

        certutil.exe -addstore Root MyFakeRootCertificateAuthority.cer
        ```

    3. Führen Sie die `mmc` Befehl, das Zertifikate-Snap-in für das Computerkonto für den lokalen Computer hinzufügen und überprüfen, ob die folgenden Zertifikate installiert wurden:

        - \Certificates (lokaler Computer) \Personal\Certificates\ www.contoso.com

        - \Certificates (lokaler Computer) \Trusted Root Certification Authorities\Certificates\MyFakeRootCertificateAuthority

    4. Starten Sie Internet Information Services (IIS) Manager-Konsole, und navigieren Sie auf dem Computer *Sites\Default Web Site* .

    5. Im *Aktionsbereich* *Bindings*auf und fügen Sie eine Https-Bindung mit www.contoso.com SSL-Zertifikat:

        [! [22]][22]

10. Zurück an den Client, und stellen Sie sicher, dass Sie jetzt https://www.contoso.com durchsuchen können.

### <a name="create-an-azure-active-directory-tenant"></a>Eine Mieter Azure Active Directory erstellen

1. Azure-Portal erstellen eine Azure Active Directory Mieter wie *Myaadname*. onmicrosoft.com, *Myaadname* Sie aktiviert ist.

2. Fügen Sie einen Anwender namens *Admin* mit der Rolle GlobalAdmin ein Verzeichnis. Notieren Sie sich den neu generierten Kennworts.

3. Internet Explorer, navigieren Sie zu https://account.activedirectory.windowsazure.com/ und melden Sie sich als admin@ *Myaadname*. onmicrosoft.com. Ändern Sie Ihr Kennwort ein.

### <a name="create-and-deploy-a-test-web-application"></a>Erstellen und Bereitstellen einer Webanwendung

1. Mit Visual Studio auf dem Entwicklungscomputer erstellen eine ASP.NET Web-Anwendung mit dem Namen ContosoWebApp1 (verwenden Sie.NET Framework 4.5.2):

    [! [23]][23]

2. Wählen Sie die *MVC* -Vorlage, ändern Sie die Authentifizierung *Arbeit*und Schule und Namen Sie AAD-Mandanten. Erstellen Sie ein App-Dienst nicht in der Cloud.

    [! [24]][24]

3. Erstellen Sie und führen Sie die Anwendung so testen Sie die Authentifizierung. Geben Sie in der Seite den Kontonamen admin@ *Myaadname*. onmicrosoft.com, geben Sie das Kennwort, und klicken Sie dann auf *Anmelden*:

    [! [25]][25]

4. Überprüfen Sie, ob AAD Zugriffsrechten anmelden und Ihr Profil gelesen und dann startet die Anwendung mit Admin.

5. Internet Explorer schließen und zurück zu Visual Studio.

6. Öffnen Sie die Datei "Web.config" und in der `<appSettings>` Abschnitt, ändern Sie den Wert des Schlüssels *Ida: PostLogoutRedirectUri* *Https://www.contoso.com:443 /*. Speichern Sie die Datei.

7. Im Fenster *Projektmappen-Explorer* mit der rechten Maustaste des ContosoWebApp1-Projekts, klicken Sie auf *Veröffentlichen*.

8. Klicken Sie auf *Benutzerdefiniert*, klicken Sie im *Web veröffentlichen* . Erstellen Sie ein benutzerdefiniertes Profil mit dem Namen *ContosoWebApp1*.

9. Auf der Seite *Verbindung* *Veröffentlichungsmethode* auf *Dateisystem* festlegen Sie und einen Ordner namens *ContosoWebApp*, an einem geeigneten Ort auf dem Entwicklungscomputer befindet.

10. Legen Sie auf *der Einstellungsseite* der *Konfiguration* *auf*.

11. Klicken Sie auf *der Vorschauseite* auf *Veröffentlichen*.

12. Mit jedem Webserver wiederum (über die Verwaltungsebene VM) und führen Sie die folgenden Aufgaben:

    1. Kopieren von *ContosoWebApp* Ordner und seinen Inhalt vom Entwicklungscomputer *C:\inetpub* Ordner.

    2. Navigieren Sie die Internet Information Services (IIS)-Manager-Konsole *Sites\Default* Website auf dem Computer.

    3. Klicken Sie im *Aktionsbereich* *Basis*, und ändern Sie den physischen Pfad der Website in *%SystemDrive%\inetpub\ContosoWebApp*:

        [! [26]][26]

### <a name="publish-the-test-web-application-through-aad"></a>Die Anwendung durch AAD Web veröffentlichen

1. Azure-Portal melden Sie an und navigieren Sie zum Verzeichnis AAD.

2. Klicken Sie auf die Registerkarte *Applications* ContosoWebApp1 Anwendung.

3. Stellen Sie sicher, dass die Anwendung das Verzeichnis hinzugefügt wird, und klicken Sie auf die Registerkarte *Konfigurieren* .

4. *SIGN-ON URL* in Https://www.contoso.com:443 ändern und *Antwort-URL* auf Https://www.contoso.com:443 (die gleiche URL) festgelegt.

5. Speichern Sie die Konfiguration.

6. Navigieren Sie auf dem Testclientcomputer https://www.contoso.com. AAD fordert Sie Ihre Anmeldeinformationen überprüfen und melden.

>[AZURE.NOTE] Dies ist der Endpunkt des ersten Szenarios: Zugriff auf die mehrstufige Webanwendung mit in der Cloud für externe Benutzer AAD Kennwort Authentifizierung aktivieren.

### <a name="create-a-simulated-on-premises-environment-with-active-directory"></a>Erstellen einer lokalen simulierten Umgebung mit Active Directory

Die lokale Umgebung enthält zwei Domänencontroller für die `contoso.com` Domäne und zwei Server zum Hosten von Azure AD Connect Dienst synchronisiert. Server zum Hosten von Azure AD-Verbindung sind keine Domäne.

1. Im Explorer zurück auf den Ordner Skripts mit dem Skript N-Tier-Webanwendungsprojekt erstellt.

2. Fügen Sie im Ordner Parameter einen anderen Unterordner mit dem Namen `Onpremise`.

3. Die folgenden Dateien in Parameter-Onpremise Ordner herunterladen:

    - [hinzufügen-Fügt-Domain-controller.parameters.json][add-adds-domain-controller-parameters]

    - [Erstellen-fügt-Gesamtstruktur-extension.parameters.json][create-adds-forest-extension-parameters]

    - [VirtualMachines adc.parameters.json][virtualMachines-adc-parameters]

    - [VirtualMachines-adc-joindomain.parameters.json][virtualMachines-adc-joindomain-parameters]

    - [VirtualMachines adds.parameters.json][virtualMachines-adds-parameters]

    - [virtualNetwork.parameters.json][virtualNetwork-parameters]

    - [VirtualNetwork fügt dns.parameters.json][virtualNetwork-adds-dns-parameters]

5. Bearbeiten Sie Deploy-ReferenceArchitecture.ps1-Datei im Ordner Skripts und ändern Sie folgende Zeile, um die Ressourcengruppe angeben, die erstellt oder die VM und Ressourcen vom Skript verwendet werden:

        ```powershell
        # PowerShell
        $resourceGroupName = "ra-aad-onpremise-rg"
        ```

6. Die folgenden Dateien im Ordner Parameter-Onpremise und legen die `resourceGroup` Wert der `virtualNetworkSettings` Abschnitt dieser Dateien identisch sein, die in der Skriptdatei bereitstellen ReferenceArchitecture.ps1 angegeben.

    - VirtualMachines adds.parameters.json

    - VirtualMachines adc.parameters.json

    - virtualNetwork.parameters.json

    - VirtualNetwork fügt dns.parameters.json

7. Öffnen Sie ein Azure PowerShell-Fenster, wechseln Sie zum Ordner Skripts und führen Sie folgenden Befehl:

        ```powershell
        .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Windows onpremise
        ```

    Ersetzen Sie `<subscription id>` mit Ihrer Azure-Abonnement-ID

    Für `<location>`, geben einen Bereich Azure `eastus` oder `westus`.

### <a name="install-and-configure-the-azure-ad-connect-sync-service"></a>Installation und Konfiguration von Azure AD Connect-Synchronisierungsdienst

In den folgenden Schritten dargestellte Konfiguration besteht aus zwei Instanzen der Synchronisierungsdienst Azure AD verbinden. Die erste ist aktiv, während die zweite im staging-Modus Schnelles Failover und hohe Verfügbarkeit bereitstellen, wenn der erste Server konfiguriert ist.

1. Azure-Portal verwenden, navigieren Sie zu der Ressourcengruppe VMs für Synchronisierungsdienste Azure AD Connect (*Ra Aad-lokale Edition Rg* standardmäßig) und *Ra-Aad-lokale Edition-adc-vm1* VM an. Melden Sie sich als *Testbenutzer* mit Kennwort *AweS0me@PW*.

2. Azure AD Connect [hier]herunterladen[aad-connect-download].

3. Führen Sie Azure AD Connect-Installationsprogramm aus, und führen Sie eine Installation mit der Option *Anpassen* .

    >[AZURE.NOTE] Sie können nicht die Option *Express Einstellungen* verwenden, da VM hosting Azure AD Connect-Synchronisierungsdienst nicht Mitglied einer Domäne ist.

4. Lassen Sie auf der Seite *erforderliche Komponenten installieren* optionale Konfigurationsinformationen leer, die Standardoptionen zu übernehmen.

5. Wählen Sie auf der Seite *Benutzer anmelden* *Kennwortsynchronisation*.

6. Geben Sie auf der Seite *Connect to Azure AD* admin@ *Myaadname*. onmicrosoft.com, wobei *Myaadname* der Name AAD-Mandanten ist. Geben Sie das Kennwort für das Administratorkonto ein.

7. Geben Sie auf der Seite *verbinden die Verzeichnisse* contoso.com für die Gesamtstruktur (Geben Sie den Wert in, weil es in der Dropdown-Liste angezeigt wird), geben Sie CONTOSO\testuser für den Benutzernamen ein, geben Sie AweS0me@PW ein, und klicken Sie dann auf *Verzeichnis hinzufügen*.

8. Übernehmen Sie auf der Seite *Azure AD - in Konfiguration* den Standardwert für den Benutzerprinzipalnamen. Überprüfen Sie *Weiter ohne verifizierten Domänen*.

    >[AZURE.NOTE] Das Verzeichnis "contoso.com" aufgelisteten als *Nicht überprüft*. Überprüfen Sie einen Domänennamen müssen Sie die Domänennamen Registrierungsstelle TXT-Eintrag erstellen. In diesem Beispiel wird die lokale Domäne nicht extern registriert. Weitere Informationen finden Sie unter [Hinzufügen einer benutzerdefinierten Domänennamen in Azure Active Directory][aad-custom-directory].

9. Wählen Sie auf der Seite *Domäne und Organisationseinheit Filtern* *synchronisieren alle Domänen und Organisationseinheiten* (Standard).

10. Übernehmen Sie die Standardwerte auf der Seite *, die die Benutzer eindeutig identifiziert* .

11. Wählen Sie auf der Seite *Filter Benutzer und Geräte* *synchronisieren alle Benutzer und Geräte* (Standard).

12. Wählen Sie auf der Seite *Optionen* *Kennwort Rückschreiben*.

13. Wählen Sie auf der Seite *Konfigurieren* *Starten der Synchronisierung nach Abschluss der Konfiguration*, und *staging Modus* deaktiviert lassen.

14. Überprüfen Sie der Konfiguration ohne Fehler abgeschlossen und beenden Sie das Installationsprogramm zu.

15. Azure-Portal verbinden Sie mit *Ra-Aad-lokale Edition-adc-vm2* VM. Melden Sie sich als *Testbenutzer* mit Kennwort *AweS0me@PW*.

16. Herunterladen Sie Azure AD Connect, und führen Sie das Installationsprogramm.

17. Assistenten schrittweise und wie in den Schritten 3 bis 12 beschrieben.

18. Wählen Sie auf der Seite *Konfigurieren* *Starten der Synchronisierung nach Abschluss der Konfiguration*und **auch** *staging-Modus aktivieren*. Dadurch Azure AD Connect-Synchronisierungsdienst Informationen über Konten und Objekte aus der Domäne "contoso.com" abgerufen und in der Datenbank gespeichert, aber es nicht zu Ihrem AAD Mandanten übertragen.

14. Überprüfen Sie der Konfiguration ohne Fehler abgeschlossen und beenden Sie das Installationsprogramm zu.

### <a name="test-the-aad-configuration"></a>Testkonfiguration AAD

1. Azure-Portal verwenden, wechseln Sie zum Verzeichnis AAD öffnen Sie Blade Azure Active Directory zu, klicken Sie auf *Benutzer und Gruppen*und klicken Sie auf *alle Benutzer* zum Anzeigen der Liste der Benutzer und Gruppen mit dem Verzeichnis synchronisiert. Benutzer für die folgenden Konten sollte angezeigt werden:

    - Admin (admin@ *Myaadname*. onmicrosoft.com)

    - Lokalen Verzeichnis Synchronisierungsdienstkonto (Sync_ADC1_*Nnnnnnnnnnnn*@*Myaadname*. onmicrosoft.com)

    - Lokalen Verzeichnis Synchronisierungsdienstkonto (Sync_ADC2_*Nnnnnnnnnnnn*@*Myaadname*. onmicrosoft.com)

2. In Azure-Portal mit VMs für die AD DS-Domänencontroller (*Ra Aad-lokale Edition Rg* standardmäßig) Ressourcengruppe navigieren Sie und *Ra-Aad-lokale Edition-Ad-vm1* VM an. Melden Sie sich als *Testbenutzer* mit Kennwort *AweS0me@PW*.

3. Über die Konsole *Active Directory-Benutzer und-Computer* fügen Sie neue Namen Diane Tibbot Anmeldung dianet@contoso.com. Geben Sie ein Kennwort Ihrer Wahl. Erleichtern Sie dem Benutzer ein Mitglied der lokalen Gruppe Administratoren (Dies ist, so dass Sie lokal als dieser Benutzer höher anmelden können-nur Computer in der Domäne Domänencontroller).

4. *Ra-Aad-lokale Edition-adc-vm1* VM wechseln Sie, öffnen Sie ein PowerShell-Fenster und führen Sie die folgenden Befehle:

        ```[powershell]
        Import-Module ADSync
        Start-ADSyncSyncCycle -PolicyType Delta
        ```

    Stellen Sie sicher, dass der Befehl *erfolgreich*zurückgegeben.

    >[AZURE.NOTE] Dieser Schritt ist erforderlich, da standardmäßig die Synchronisierung geplant ist 30 Minuten. Diese Befehle erzwingen eine Synchronisierung sofort.

5. Azure-Portal zurück, wechseln Sie zu dem AAD-Verzeichnis öffnen Blade Azure Active Directory klicken Sie auf *Benutzer und Gruppen*, und klicken Sie auf *alle Benutzer*. Diane Tibbot sollte nun angezeigt werden (dianet@ *Myaadname*. onmicrosoft.com) in der Liste der Benutzer angezeigt.

6. Blatt Azure Active Directory auf *Anwendung*und klicken Sie dann auf *Alle Programme*. Klicken Sie auf die *ContosoWebApp1* -Anwendung, und klicken Sie auf *Benutzer und Gruppen*. Klicken Sie in der Symbolleiste auf *Hinzufügen*. Klicken Sie auf *Benutzer und Gruppen*, und wählen Sie *Diane Tibbot*. Klicken Sie auf *zuweisen*.

7. Navigieren Sie in Internet Explorer auf der Website https://account.activedirectory.windowsazure.com. Melden Sie sich als dianet@ *Myaadname*. onmicrosoft.com mit dem Kennwort, das Sie zuvor angegeben haben.

    >[AZURE.NOTE] Klicken Sie nicht auf das ContosoWebApp-Symbol in der Liste der Programme. AAD sucht die Anwendung börsennotierten DNS Adresse für www.contoso.com unterscheidet sich von der Adresse der Webebene Lastenausgleich.

8. Klicken Sie auf die Registerkarte *Profil* . Die Details des Benutzers (falls eine angegeben wurde bei der Erstellung) sollte angezeigt werden.

    >[AZURE.NOTE] Wenn Sie *Kennwort ändern*klicken, dürfen Sie diese Aufgabe ausführen, wie dieser Vorgang vom Administrator zuerst aktiviert werden muss. Weitere Informationen finden Sie unter [Erste Schritte mit Passwort-Management][aad-password-management].

### <a name="enable-on-premises-users-to-run-the-application-by-using-authentication-through-aad"></a>Lokalen Benutzer zum Ausführen der Anwendung mit Authentifizierung über AAD

1. Zurück zum Domänencontroller *Ra-Aad-lokale Edition-Ad-vm1* VM.

2. Öffnen Sie die *DNS-Manager* -Konsole und fügen Sie einen neuen Hosteintrag www.contoso.com. Geben Sie die öffentliche IP-Adresse des Web-Tier-Lastenausgleich.

3. Kopieren Sie die Datei *MyFakeRootCertificateAuthority.cer* aus dem Testclientcomputer (Erstellung dieser Dateien in der Prozedur [simulieren Konfiguration einer öffentlichen Website](#simulate-configuration-of-a-public-web-site)
    
4. Öffnen Sie ein Eingabeaufforderungsfenster als Administrator und führen Sie den folgenden Befehl in die Datei, die gerade kopierten Ordner verschieben:

        ```
        certutil.exe -addstore Root MyFakeRootCertificateAuthority.cer
        ```

5. Navigieren Sie in Internet Explorer zu https://www.contoso.com. Ob die AAD-Anmeldeseite für die Anwendung ContosoWebApp1 angezeigt.

6. Melden Sie sich als dianet@ *Myaadname*. onmicrosoft.com. Die Anwendung führen und ordnungsgemäß anmelden.

## <a name="next-steps"></a>Nächste Schritte

- Lernen Sie die best Practices für die [Erweiterung Ihrer Domäne lokale fügt Azure][adds-extend-domain]

- Lernen Sie die optimalen Methoden zum [Erstellen einer Ressourcengesamtstruktur fügt] [ adds-resource-forest] in Azure

<!-- links -->
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[script]: #sample-solution-script
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[active-directory-domain-services]: https://technet.microsoft.com/library/dd448614.aspx
[active-directory-federation-services]: https://technet.microsoft.com/windowsserver/dd448613.aspx
[azure-active-directory]: ../active-directory-domain-services/active-directory-ds-overview.md
[azure-ad-connect]: ../active-directory/active-directory-aadconnect.md
[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[aad-explained]: https://youtu.be/tj_0d4tR6aM
[aad-editions]: ../active-directory/active-directory-editions.md
[guidance-adds]: ./guidance-iaas-ra-secure-vnet-ad.md
[sla-aad]: https://azure.microsoft.com/support/legal/sla/active-directory/v1_0/
[azure-multifactor-authentication]: ../multi-factor-authentication/multi-factor-authentication.md
[aad-conditional-access]: ../active-directory//active-directory-conditional-access.md
[aad-dynamic-membership-rules]: ../active-directory/active-directory-accessmanagement-groups-with-advanced-rules.md
[aad-dynamic-memberships]: https://youtu.be/Tdiz2JqCl9Q
[aad-user-sign-in]: ../active-directory/active-directory-aadconnect-user-signin.md
[aad-sync-requirements]: ../active-directory/active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md
[aad-topologies]: ../active-directory/active-directory-aadconnect-topologies.md
[aad-filtering]: ../active-directory/active-directory-aadconnectsync-configure-filtering.md
[aad-scalability]: https://blogs.technet.microsoft.com/enterprisemobility/2014/09/02/azure-ad-under-the-hood-of-our-geo-redundant-highly-available-distributed-cloud-directory/
[aad-connect-sync-default-rules]: ../active-directory/active-directory-aadconnectsync-understanding-default-configuration.md
[aad-identity-protection]: ../active-directory/active-directory-identityprotection.md
[aad-password-management]: ../active-directory/active-directory-passwords-customize.md
[aad-application-proxy]: ../active-directory/active-directory-application-proxy-enable.md
[aad-connect-sync-operational-tasks]: ../active-directory/active-directory-aadconnectsync-operations.md#staging-mode
[aad-custom-domain]: ../active-directory/active-directory-add-domain.md
[aad-powershell]: https://msdn.microsoft.com/library/azure/mt757189.aspx
[aad-sync-disaster-recovery]: ../active-directory/active-directory-aadconnectsync-operations.md#disaster-recovery
[aad-sync-best-practices]: ../active-directory/active-directory-aadconnectsync-best-practices-changing-default-configuration.md
[aad-adfs]: ../active-directory/active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs
[aad-health]: ../active-directory/active-directory-aadconnect-health-sync.md
[aad-health-adds]: ../active-directory/active-directory-aadconnect-health-adds.md
[aad-health-adfs]: ../active-directory/active-directory-aadconnect-health-adfs.md
[aad-agent-installation]: ../active-directory/active-directory-aadconnect-health-agent-install.md
[aad-reporting-guide]: ../active-directory/active-directory-reporting-guide.md
[azure-cli]: ../virtual-machines-command-line-tools.md
[azure-powershell-download]: ../powershell-install-configure.md
[solution-script]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/Deploy-ReferenceArchitecture.ps1
[vnet-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/virtualNetwork.parameters.json
[nsg-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/networkSecurityGroups.parameters.json
[webtier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/webTier.parameters.json
[businesstier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/businessTier.parameters.json
[datatier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/dataTier.parameters.json
[managementtier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/managementTier.parameters.json
[makecert]: https://msdn.microsoft.com/library/windows/desktop/aa386968(v=vs.85).aspx
[add-adds-domain-controller-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/add-adds-domain-controller.parameters.json
[create-adds-forest-extension-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/create-adds-forest-extension.parameters.json
[virtualMachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualMachines-adds.parameters.json
[virtualNetwork-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualNetwork.parameters.json
[virtualNetwork-adds-dns-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualNetwork-adds-dns.parameters.json
[virtualMachines-adc-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualMachines-adc.parameters.json
[virtualMachines-adc-joindomain-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualMachines-adc-joindomain.parameters.json
[aad-connect-download]: http://www.microsoft.com/download/details.aspx?id=47594
[aad-custom-directory]: ../active-directory/active-directory-add-domain.md
[aad-password-management]: ../active-directory/active-directory-passwords-getting-started.md#enable-users-to-reset-their-azure-ad-passwords
[adds-extend-domain]: ./guidance-identity-adds-extend-domain.md
[adds-resource-forest]: ./guidance-identity-adds-resource-forest.md
[0]: ./media/guidance-identity-aad/figure1.png "Architektur der Cloud Identität mit Active Directory Azure"
[1]: ./media/guidance-identity-aad/figure2.png "Einzelne Gesamtstruktur, einzelne AAD Directory-Topologie"
[2]: ./media/guidance-identity-aad/figure3.png "Einzelne AAD Directory-Topologie mit mehreren Gesamtstrukturen"
[4]: ./media/guidance-identity-aad/figure5.png "Staging-Server-Topologie"
[5]: ./media/guidance-identity-aad/figure6.png "Topologie mit mehreren AAD Verzeichnisse"
[6]: ./media/guidance-identity-aad/figure7.png "Benutzerdefinierte Installation von Azure AD verbinden Synchronisierung mit einer bestimmten Instanz von SQL Server"
[7]: ./media/guidance-identity-aad/figure8.png "Angeben der SSO-Methode für Benutzer anmelden"
[8]: ./media/guidance-identity-aad/figure9.png "Domäne und Organisationseinheit Filteroptionen angeben"
[9]: ./media/guidance-identity-aad/figure10.png "Write-Back-Kennwort aktivieren"
[10]: ./media/guidance-identity-aad/figure11.png "Azure AD Management Blade im portal"
[11]: ./media/guidance-identity-aad/figure12.png "Azure AD Connect-Konsole"
[12]: ./media/guidance-identity-aad/figure13.png "Registerkarte "Vorgänge" Synchronisierung Service Manager"
[13]: ./media/guidance-identity-aad/figure14.png "Die Registerkarte Connectors die Synchronisierung Service Manager"
[14]: ./media/guidance-identity-aad/figure15.png "Synchronisierung Regel-Editor"
[15]: ./media/guidance-identity-aad/figure16.png "Azure Active Directory verbinden Gesundheit Blade im Azure-Portal mit Synchronisation Zustands"
[16]: ./media/guidance-identity-aad/figure17.png "Azure Active Directory verbinden Gesundheit Blade im Azure-Portal mit AD DS-Zustand"
[17]: ./media/guidance-identity-aad/figure18.png "Sicherheitsberichte in Azure-Portal verfügbar"
[18]: ./media/guidance-identity-aad/figure19.png "Azure-Portal Hervorhebung öffentliche IP-Adresse des Web-Tier-Lastenausgleich"
[19]: ./media/guidance-identity-aad/figure20.png "Mit Internet Explorer zum Durchsuchen der öffentlichen IP-Adresse des Web-Tier-Lastenausgleich"
[20]: ./media/guidance-identity-aad/figure21.png "Das Snap-In Zertifikate www.contoso.com Zertifikat anzeigen"
[21]: ./media/guidance-identity-aad/figure22.png "Herstellen einer Verbindung mit der Management-Ebene VM"
[22]: ./media/guidance-identity-aad/figure23.png "Erstellen die HTTPS-Bindung für die Standardwebsite"
[23]: ./media/guidance-identity-aad/figure24.png "ContosoWebApp1 Anwendung erstellen"
[24]: ./media/guidance-identity-aad/figure25.png "Festlegen der Eigenschaften der Website ContosoWebApp1"
[25]: ./media/guidance-identity-aad/figure26.png "Anmelden bei Azure AAD aus ContosoWebApp1 der Anwendung"
[26]: ./media/guidance-identity-aad/figure27.png "Ändern den Ordner für die Standardwebsite"