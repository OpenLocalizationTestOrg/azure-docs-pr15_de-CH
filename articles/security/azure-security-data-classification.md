<properties
   pageTitle="Klassifizierung für Azure | Microsoft Azure"
   description="Dieser Artikel bietet eine Einführung in die Grundlagen der Klassifizierung und hebt dessen Wert speziell im Bereich Cloud computing und mit Microsoft Azure"
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="yurid"/>

# <a name="data-classification-for-azure"></a>Klassifizierung für Azure

Dieser Artikel bietet eine Einführung in die Grundlagen der Klassifizierung und hebt dessen Wert speziell im Bereich Cloud computing und mit Microsoft Azure. 

## <a name="data-classification-fundamentals"></a>Grundlegendes zu Klassifizierung

Erfolgreiche Klassifizierung in einer Organisation erfordert umfassende für Ihre Organisation und auskennen, Ihrer Datenbestände befinden.  
 
Daten in einem von drei grundlegenden Zustandswerten vorhanden sind: 

- Im Ruhezustand 
- Im Prozess 
- Bei der Übertragung 
 
Alle drei Zustände erfordern eindeutige Lösungen für Daten sollte, die angewandten Grundsätze der Klassifizierung jedoch gleich. Die als vertraulich eingestuft werden Daten im Ruhezustand im Prozess und bei der Übertragung vertraulich bleiben. 
 
Daten können auch strukturierte oder unstrukturierte sein. Normale Klassifizierung Prozesse für strukturierten Daten in Datenbanken und Tabellen sind weniger komplex und zeitaufwändig als unstrukturierte Daten wie Dokumente, Quellcode und e-Mail verwalten. 

> [AZURE.TIP] Weitere Informationen zu Azure-Funktionen und best Practices für Daten finden Sie unter Verschlüsselung [Azure Data Encryption Best Practices](azure-security-data-encryption-best-practices.md)

Im Allgemeinen Organisationen haben mehr unstrukturierte Daten als strukturierte Daten. Unabhängig davon, ob Daten strukturiert oder unstrukturiert ist es wichtig für Vertraulichkeit der Daten zu verwalten. Bei ordnungsgemäßer Implementierung trägt Daten vertrauliche Daten Anlagen mit größere Kontrolle als Daten verwaltet, die öffentlich oder frei verteilt werden. 

### <a name="controlling-access-to-data"></a>Steuern des Zugriffs auf Daten 

Authentifizierung und Autorisierung werden oft miteinander verwechselt und ihre Rollen missverstanden. In der Praxis sind ganz anders, wie in der folgenden Abbildung dargestellt.  

![Datenzugriff und Kontrolle](./media/azure-security-data-classification/azure-security-data-classification-fig1.png)

### <a name="authentication"></a>Authentifizierung 

Authentifizierung in der Regel besteht aus zwei Teilen: einem Benutzernamen oder Benutzer-ID einen Benutzer und ein Token wie ein Kennwort, um sicherzustellen, dass die Anmeldeinformationen Benutzername gültig ist. Der Prozess bietet keine authentifizierten Benutzer Zugriff auf alle Artikel oder Dienstleistungen. Er überprüft, dass der Benutzer sie vorgeben.   

> [AZURE.TIP] [Azure Active Directory](../active-directory/active-directory-whatis.md) bietet Identität Cloud-basierte Services, mit denen Sie zum Authentifizieren und Autorisieren von Benutzern. 

### <a name="authorization"></a>Autorisierung
 
Die Autorisierung wird authentifizierten Benutzer Zugriff auf eine Anwendung, Daten, Datei oder ein anderes Objekt bereitstellen. Zuweisen von authentifizierten Benutzern Rechte verwenden, ändern oder Elemente löschen, auf die sie Zugriff auf Daten benötigt. 

Erfolgreicher Autorisierung erfordert ein Mechanismus zum Überprüfen der einzelne Benutzer muss Zugriff auf Dateien und Informationen aus Rolle, Sicherheitsrichtlinien und Risiko-Policy Considerations basiert. Z. B. Daten aus bestimmten Line-of-Business (LOB)-Applikationen müssen alle Mitarbeiter zugreifen und benötigen nur eine kleine Teilmenge der Mitarbeiter wahrscheinlich Zugriff auf Dateien der Personalabteilung (HR). Aber für Organisationen möglich, die Daten sowie wann und wie ein effizientes System zur Benutzerauthentifizierung werden muss. 

> [AZURE.TIP] Achten Sie darauf, Microsoft Azure nutzen Azure Role-Based Access Control (RBAC) um dem Betrag von Access erteilt werden, die Benutzer für ihre Aufgaben benötigen. Weitere Informationen finden Sie bei [verwenden Arbeitsaufträge für Benutzerrollen Zugriff auf Ihre Ressourcen Azure Active Directory verwalten](../active-directory/role-based-access-control-configure.md) . 

### <a name="roles-and-responsibilities-in-cloud-computing"></a>Rollen und Verantwortungsbereiche im cloud-computing 

Zwar Cloudanbieter können Risiken Kunden müssen diese Klassifizierung Verwaltung und Durchsetzung wird ordnungsgemäß implementiert, um die entsprechenden Daten-Management-Services bieten.  
 
Daten Klassifizierung Aufgaben hängen die Cloud Service-Modell ist wie in der folgenden Abbildung dargestellt. Die drei primären Cloud-Dienstmodelle sind Infrastruktur als Service (IaaS), Plattform als Service (PaaS) und Software als Service (SaaS). Implementierung von Data Classification Mechanismen wird auch Abhängigkeit und erwartet die Cloud-Anbieter abhängig. 

![Rollen](./media/azure-security-data-classification/azure-security-data-classification-fig2.png)

Obgleich Sie Daten klassifizieren, sollten Cloudanbieter schriftliche Verpflichtung zu wie sie sichern und Schützen der Daten in die Cloud gespeichert.  

- **IaaS-Anbieter** Vorschriften sind auf sicherstellen, dass die virtuelle Funktionen für Klassifizierung und Kunden Auflagen aufnehmen kann. IaaS-Anbieter haben eine geringere Rolle in Klassifizierung, brauchen sie nur sicherzustellen, dass Kundendaten Auflagen erfüllt. Allerdings müssen Anbieter noch ihre virtuelle Umgebung Daten Klassifizierung zusätzlich zur Sicherung ihrer Rechenzentren Anforderungen.
- **PaaS-Anbieter** Aufgaben können gemischt werden, da die Plattform Sicherheit für Klassifizierungstool Ebenen verwendet werden könnten. PaaS-Anbieter können möglicherweise einige Autorisierungsregeln zu Authentifizierung und Sicherheit und Klassifizierung Funktionen ihrer Anwendung Ebene erforderlich. Viel müssen PaaS-Anbieter wie IaaS-Anbieter auf ihrer Plattform alle relevanten Klassifizierung entsprechen.
- **SaaS-Anbieter** wird häufig als Teil einer Kette Autorisierung und müssen sicherstellen, dass die Daten in der SaaS Anwendung kann gesteuert werden durch Klassifizierungstyp. SaaS können eingesetzt werden für LOB-Applikationen und Natur muss Instrument zum Authentifizieren und autorisieren verwendet und gespeichert. 

## <a name="classification-process"></a>Klassifizierung 

Viele Organisationen, die Daten verstehen und sie implementieren eine grundlegende Herausforderung: zu?

Eine effektive und einfache Möglichkeit, Daten zu implementieren ist, verwenden Sie PLAN tun, Modell von [MOF](https://technet.microsoft.com/solutionaccelerators/dd320379.aspx)handeln. In der folgenden Abbildung Diagramme die Aufgaben, die Daten in diesem Modell umzusetzen.  

1. **Planen**. Identifizieren Sie Datenbestände, ein Verwaltungsberechtigter Daten bereitstellen Programms Klassifizierung und schutzprofile. 
2. **DO**. Nach Klassifizierung Datenrichtlinien vereinbart, Bereitstellen der Anwendung und Durchsetzung Technologies für vertrauliche Daten implementieren.  
3. **Überprüfen**. Überprüfen und um zu gewährleisten, Tools und Methoden, die Klassifizierung Richtlinien effektiv Adressierung überprüfen. 
4. **ACT**. Überprüfen Sie den Status von Daten und Überprüfen von Dateien und Daten, die Änderung mit einer Umbuchung und Revision Methodik zu ändern und neue Risiken.  

![Planen Sie, und überprüfen Sie, fungieren](./media/azure-security-data-classification/azure-security-data-classification-fig3.png)
 
###<a name="select-a-terminology-model-that-addresses-your-needs"></a>Wählen Sie ein Modell Terminologie, die Ihre Bedürfnisse
 
Verschiedene Verfahren zur Klassifizierung von Daten, einschließlich manueller Prozesse vorhanden, standortbasierte Prozesse Daten klassifizieren anhand eines Benutzers oder des Systems Speicherorts Anwendung Prozesse wie datenbankspezifische Klassifizierung und automatisierte Prozesse verschiedener Technologien, die im Abschnitt "Schützen von vertraulichen Daten" weiter unten in diesem Artikel beschrieben werden.  
 
Dieser Artikel stellt zwei allgemeine Terminologie-Modelle, die gut und branchenweit anerkannten Modelle basieren. Diese Terminologie-Modelle, die, die der drei Klassifizierung Schutzstufe bereitstellen, werden in der folgenden Tabelle angezeigt.  

> [AZURE.NOTE] Klassifizierung von einer Datei oder Ressource, der Daten, die in der Regel auf unterschiedlichen Ebenen klassifiziert werden, sollte die höchste Einstufung vorhanden die Gesamtwertung. Z. B. eine Datei mit eingeschränkter Daten einzustufen eingeschränkt.  

| **Empfindlichkeit**   | **Terminologie-Modell 1**   | **Terminologie-Modell 2** |
|--------------------|---------------------------|-------------------------|
| Hoch               | Vertraulich              | Eingeschränkt              |
| Mittel             | Nur zur internen Verwendung     | Vertrauliche               |
| Niedrig                | Öffentliche                    | Nicht eingeschränkt            |

#### <a name="confidential-restricted"></a>Vertraulich (eingeschränkt) 

Als vertrauliche oder geheime gilt Informationen enthält Daten an Einzelpersonen oder Unternehmen katastrophale gefährdeten oder verloren. Solche Informationen häufig pro "Kenntnis notwendig" bereitgestellt und beinhalten: 

- Persönliche Daten, einschließlich personenbezogener Daten wie Sozialversicherung oder nationalen Identifikationsnummern Passnummer, Kreditkartennummern, Fahrers Lizenznummern, medizinische Daten und Krankenversicherung Richtlinien-ID-Nummern.  
- Finanzdaten, einschließlich finanzieller Kontonummern wie überprüfen oder Kontonummern Investitionen. 
- Geschäftsunterlagen, Dokumente oder Daten einmalige oder spezielle geistiges Eigentum.  
- Rechtliche Daten einschließlich potenziellen Anwalt Berechtigungen. 
- Authentifizierungsdaten, einschließlich privaten kryptografischen Schlüssel, Benutzername Kennwort-Paare oder andere Kennung Sequenzen wie private biometrische Schlüsseldateien. 

Daten, die häufig als vertraulich eingestuft hat behördlicher und Vorschriften für die Datenbehandlung von. 

#### <a name="for-internal-use-only-sensitive"></a>Für interne Verwendung nur (kritische)
 
Als mittlere Empfindlichkeit klassifiziert Informationen enthält Dateien und Daten, die nicht gravierend auf eine Person oder Organisation hätte verloren oder werden zerstört. Dazu gehören: 

- E-Mail, von denen meisten können gelöscht oder ohne eine Krise (außer Postfächern oder e-Mail von Personen, die in der vertraulich gekennzeichnet) verteilt.  
- Dokumente und Dateien, die keine vertraulichen Daten enthalten.
 
Im Allgemeinen enthält diese Klassifizierung etwas nicht vertraulich sind. Diese Klassifizierung kann die meisten Geschäftsdaten enthalten, da die meisten Dateien, die verwaltet oder täglichen verwendet, als vertraulich klassifiziert werden können. Mit Ausnahme der Daten, die öffentlich oder vertraulich, können alle Daten innerhalb eines Unternehmens standardmäßig als vertraulich klassifiziert werden. 

#### <a name="public-unrestricted"></a>Öffentliche (uneingeschränkt)
 
Informationen, die als öffentlich klassifiziert enthält Daten und Dateien, die nicht kritischen Vorgänge zu Unternehmen. Diese Klassifizierung kann auch enthalten Daten, die absichtlich zu ihrer Verwendung wie marketing-Material freigegeben wurde oder drücken Sie anzeigen. Darüber hinaus kann diese Klassifizierung Daten wie Spam e-Mail-Nachrichten von einem e-Mail-Dienst enthalten. 

### <a name="define-data-ownership"></a>Besitz von Daten definieren
 
Es ist wichtig, eine klare freiheitsentziehende Kette TCO für alle Datenbestände. Die folgende Tabelle zeigt verschiedene Besitz Rollen bei Daten Klassifizierung und ihre Rechte.  

| **Rolle**        | **Erstellen**    | **Ändern oder löschen**   | **Delegaten**  | **Lesen**    | **Archivierung/Wiederherstellung**   |
|-----------------|---------------|---------------------|---------------|-------------|-----------------------|
| Besitzer           | X             | X                   | X             | X           | X                     |
| Treuhänder       |               |                     | X             |             |                       |
| Administrator   |               |                     |               |             | X                     |
| Benutzer\*          |               | X                   |               | X           |                       |
**Benutzer können zusätzliche Rechte wie bearbeiten und löschen, indem Sie ein Verwaltungsberechtigter gewährt werden* 

> [AZURE.NOTE] Diese Tabelle bietet eine vollständige Liste der Rollen und Rechte, aber nur eine repräsentative nicht. 

**Daten verantwortlichen** ist der Urheber der Daten, die Besitzrechte zuweisen und weisen ein Verwaltungsberechtigter. Eine Datei erstellt wird, sollte der Besitzer eine Klassifikation zuweisen, was bedeutet, dass sie verstehen, was als vertraulich eingestuft werden basierend auf den Richtlinien ihrer Organisation verpflichtet sein. Ein Daten-Ressourceneigentümer Daten kann automatisch klassifiziert für interne Verwendung nur (empfindlich) für besitzen oder vertrauliche (eingeschränkte) Datentypen erstellen werden. Die Besitzerrolle ändern häufig, nachdem Daten klassifiziert werden. Z. B. Besitzer eine Datenbank Verschlusssachen erstellen und ihre Rechte Treuhänder Daten abgeben.  

> [AZURE.NOTE] Anlage Datenbesitzer verwenden oft Dienste, Geräte und Medien, einige persönliche und die Organisation gehören. Klare Unternehmensrichtlinien hilft sicherzustellen, dass die Verwendung von intelligenten Geräten und Geräten wie Laptops Daten Klassifizierung Richtlinien entspricht.  

**Daten Anlage Treuhänder** wird vom Verantwortlichen oder Stellvertretung verwalten die Anlage nach Absprache mit Verantwortlichen oder Vorschriften der betreffenden Richtlinie zugewiesen. Im Idealfall kann Treuhänder Rolle in einem automatisierten System implementiert werden. Ein Verwaltungsberechtigter Anlage wird sichergestellt, dass erforderlichen Zugriff erhalten und ist verantwortlich für die Verwaltung und Schutz an Ihre delegiert. Treuhänder Anlage konnte Verantwortungsbereiche:  

- Die Anlage der Ressourceneigentümer Richtung oder mit verantwortlichen schützen 
- Damit Klassifizierung Richtlinien eingehalten werden 
- Information Eigentümer Änderungen vereinbarten Steuerelemente und Prozeduren Schutz vor dieser Wirkung 
- Berichterstattung über ändern oder Entfernen der Anlage Verwaltungsberechtigte Aufgaben verantwortlichen 
- Ein **Administrator** stellt ein Benutzer dafür verantwortlich, dass Integrität bewahrt wird, aber sie nicht Daten Verantwortlichen, Treuhänder oder Benutzer sind. Tatsächlich bieten viele Administratorrollen Container Datenmanagement Services ohne Zugriff auf die Daten. Die Administratorrolle enthält Sicherung und Wiederherstellung von Daten Aufzeichnungen der Ressourcen auswählen, erwerben und Betrieb von Geräten und Speicher mit die Anlagen. 
- Anlage Benutzer umfasst alle Benutzer, die Zugriff auf eine Datei. Access-Zuordnung wird häufig vom Eigentümer der Anlage Verwaltungsberechtigte delegiert.  

### <a name="implementation"></a>Implementierung
  
Management gilt für alle Methoden der Klassifizierung. Diese Aspekte müssen Details, was, wo, wann und warum eine Datenressource verwendet, Zugriff, geändert oder gelöscht würden,. Asset-Management erfolgt Kenntnisse wie eine Organisation ihre Risiken angezeigt, aber eine einfache Methode kann gemäß der Klassifizierung von Daten angewendet werden. Zusätzliche Hinweise für die Klassifizierung von Daten umfassen die Einführung der neuen Tools, und verwalten ändern, nachdem eine Klassifizierungsmethode implementiert.  

### <a name="reclassification"></a>Umbuchung
 
Umbuchen oder Ändern des Zustands Klassifizierung eine Datenressource muss erfolgen, wenn einem Benutzer oder System feststellt, dass der Datenressource Bedeutung oder Risiko Profil geändert wurde. Dabei ist wichtig für der klassifizierungsstatus weiterhin aktuell und gültig sein. Die meisten Inhalte manuell nicht klassifiziert automatisch klassifiziert oder je nach Verwendung von einem Treuhänder Daten oder Datenbesitzer. 

### <a name="manual-data-reclassification"></a>Manuelle Umbuchung
 
Im Idealfall wird dabei sichergestellt, dass eine Änderung aufgezeichnet und überwacht werden. Die wahrscheinlichste Ursache für die manuelle Umbuchung wäre aus Gründen der Vertraulichkeit oder Aufzeichnungen in Papierform oder erforderlich, Daten überprüfen, die ursprünglich falsch klassifiziert wurde. Da dieses Dokument Daten und Daten in der Cloud berücksichtigt, manuelle Umbuchung Maßnahmen müssten auf einen Fall und Risiko-managementüberprüfung klassifizierungsanforderungen ideal wäre. Im Allgemeinen würden so Richtlinie über Was muss die Organisation klassifiziert, Klassifizierung Standardzustand alle Daten und Dateien vertraulichen jedoch nicht vertraulich und Behandeln von Ausnahmen für risikoreiche Daten. 

### <a name="automatic-data-reclassification"></a>Automatische Umbuchung
 
Automatische Umbuchung verwendet dieselbe Regel als manuelle Klassifizierung. Automatisierte sicherstellen, dass Regeln folgen und nach Bedarf angewendet werden. Klassifizierung von Daten als Teil einer Erzwingungsrichtlinie Daten Klassifizierung erfolgt die Daten gespeichert werden, verwendet und bei der Übertragung Autorisierung Technologie erzwungen werden kann.

- Anwendungsbasierte. Bestimmte Anwendung standardmäßig legt einen Geheimhaltungsgrad. Beispielsweise werden von Customer Relationship Management (CRM)-Software, HR und Health Records Management Tools standardmäßig vertraulich. 
- Standortbasierte. Datenspeicherort kann Daten Empfindlichkeit identifizieren. Beispielsweise ist ein HR oder Finanzabteilung gespeicherten Daten vertraulich zu.  
 
### <a name="data-retention-recovery-and-disposal"></a>Data Retention, Wiederherstellung und Entsorgung 

Daten-Recovery und Beseitigung, wie Daten Umbuchung ist ein wichtiger Aspekt der Verwaltung von Daten. Grundsätze für Daten-Recovery und Beseitigung würde eine Datenaufbewahrungsrichtlinie definiert und wie Daten Umbuchung erzwungen; So würde der Treuhänder und Administrator Rollen als gemeinsame Aufgabe durchgeführt werden.  

Fehlende eine Archivierungsrichtlinie könnte Datenverlust oder gesetzlichen Auflagen eingehalten. Die meisten Organisationen, die nicht über eine klar definierte Datenaufbewahrungsrichtlinie eher eine standardmäßige Aufbewahrungsrichtlinie "behalten". Solche eine Aufbewahrungsrichtlinie hat jedoch zusätzliche Risiken in Szenarien für Cloud-Services. 

Beispielsweise kann eine Archivierungsrichtlinie für Cloud-Dienstanbieter für "Dauer des Abonnements" gelten (sofern der Dienst bezahlt wird, die Daten aufbewahrt werden). Solche Zahlung für Aufbewahrung Vereinbarung möglicherweise nicht Unternehmens- oder anderen Aufbewahrungsrichtlinien behandelt. Definieren einer Richtlinie für vertrauliche Daten kann sicherstellen, dass Daten gespeichert und bewährten Grundlage entfernt. Darüber hinaus eine Archivierung Richtlinie erstellen, um formalisieren Kenntnisse über welche Daten entfernt werden und wann. 

Datenaufbewahrungsrichtlinie sollte die Adresse behördlichen Auflagen, sowie Unternehmen rechtlichen Auflagen. Geschützte Daten möglicherweise Fragen zur Aufbewahrung Dauer und Ausnahmen für Daten, die gespeichert wurden mit einem Anbieter provozieren; Diese Fragen sind eher für Daten, die nicht richtig klassifiziert. 

> [AZURE.TIP] Erfahren Sie mehr über Azure Data Retention Policies und durch Lesen des [Microsoft Online-Abonnement-Vertrag](https://azure.microsoft.com/support/legal/subscription-agreement/)

## <a name="protecting-confidential-data"></a>Vertrauliche Daten
  
Nachdem Daten klassifiziert wird suchen und Schützen von vertraulichen Daten implementieren Bestandteil jeder Bereitstellung Datenschutzstrategie. Vertrauliche Daten erfordert zusätzliche wie Daten gespeichert und übertragen in herkömmlichen Architekturen sowie in der Cloud. 

Dieser Abschnitt enthält grundlegende Informationen über einige Technologien, die Automatisierung von Durchsetzungsmaßnahmen, um Daten zu schützen, die als vertraulich eingestuft wurden. 
 
Wie die folgende Abbildung zeigt, können diese Technologie als lokale oder Cloud-Lösungen bereitgestellt – oder Hybrid, mit von Ihnen bereitgestellten lokal und in der Cloud. (Einige Technologien wie Verschlüsselung und Rights Management erstreckt sich auch auf Geräte.)  

![Technologies](./media/azure-security-data-classification/azure-security-data-classification-fig4.png)

### <a name="rights-management-software"></a>Rights Management-software  

Eine Lösung für Schutz vor Datenverlust ist Rights Management-Software. Im Gegensatz zu Methoden, die versuchen, Informationen an eine Organisation vorzeitig funktioniert Rights Managementsoftware tiefen Ebene innerhalb Wechselspeicher-Technologie. Dokumente verschlüsselt und Kontrolle, die sie entschlüsseln kann verwendet Zugriffskontrolle, die eine Authentifizierung Kontrollmechanismus wie einem Verzeichnisdienst definiert sind.  

> [AZURE.TIP] RMS (Azure RMS) können als die Information Protection Solution Sie Daten in verschiedenen Szenarien. Lesen [Neuigkeiten RMS?](https://docs.microsoft.com/rights-management/understand-explore/what-is-azure-rms) Weitere Informationen zu dieser Lösung Azure.

Die Vorteile von Rights Managementsoftware gehören: 

- Vertrauliche Informationen geschützt. Benutzer können ihre Daten direkt mit Rights Management-fähige Anwendung schützen. Es sind keine weiteren Schritte erforderlich – Erstellen von Dokumenten, e-Mail und Veröffentlichen von Daten bieten eine konsistente Schutz. 
- Schutz übertragen der Daten. Kunden bleiben steuern, wer Zugriff auf ihre Daten in der Cloud, vorhandene IT-Infrastruktur oder auf dem Desktop des Benutzers hat. Organisationen können die Daten verschlüsseln und den Zugriff entsprechend seinen Geschäftserfordernissen. 
- Informationen Schutz Standardrichtlinien. Administratoren und Benutzer können Standardrichtlinien für viele Business-Szenarien wie Unternehmen vertraulich – nur-Lese"und"Do Not Forward."verwenden. Umfassende Nutzung Rechte unterstützt wie lesen, kopieren, drucken, speichern, bearbeiten und Weiterleiten an Flexibilität definieren benutzerdefinierte Nutzungsrechte. 

> [AZURE.TIP] Sie können Daten in Azure-Speicher mithilfe von [Azure Storage Service Verschlüsselung](../storage/storage-service-encryption.md) für ruhende Daten schützen. [Azure Datenträgerverschlüsselung](azure-security-disk-encryption.md) können Sie um Daten auf virtuellen Laufwerken Azure virtuelle Maschinen zu schützen.

### <a name="encryption-gateways"></a>Verschlüsselung-gateways

Verschlüsselung Gateways in deren eigenen Schichten Dienstleistungen durch erneutes Vollzugriff für Cloud-basierte Verschlüsselung eingesetzt werden. Dieser Ansatz sollte nicht mit einem virtuellen privaten Netzwerk (VPN) verwechselt werden. Verschlüsselung Gateways sollen eine transparente Ebene Cloud-Lösungen.   

Verschlüsselung Gateways bieten eine Möglichkeit zum Verwalten und sichere Daten durch Verschlüsselung der Daten bei der Übertragung sowie Daten als vertraulich eingestuft wurde.  
 
Verschlüsselung Gateways werden in den Datenfluss zwischen Geräten und Anwendungsdaten Ressourcen Verschlüsselung/Entschlüsselung-Diensten platziert. Diese Lösungen wie VPNs sind überwiegend lokale. Sie sollen einen dritten Kontrolle über Verschlüsselungsschlüssel, welche das Risiko reduziert, dass Daten und Schlüssel-Management mit einem Anbieter. Lösung, wie Verschlüsselung, problemlos und transparent zwischen Benutzern und dem Dienst dienen. 

> [AZURE.TIP] Azure ExpressRoute können Sie Ihre lokale Netzwerke in der Cloud von Microsoft über eine dedizierte private Verbindung erweitern. Lesen Sie [Technische Übersicht über ExpressRoute](../expressroute/expressroute-introduction.md) für Weitere Informationen zu dieser Funktion. Andere Optionen für cross zwischen dem lokalen Netzwerk und [Azure ist ein Standort-zu-Standort-VPN-](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)Verbindungen.

### <a name="data-loss-prevention"></a>Datenverlust 
Daten (auch als Daten bezeichnet) ist eine wichtige Überlegung und externen Datenverlust über schädliche und unbeabsichtigte Insider ist für viele Organisationen.  
 
Data Loss Prevention (DLP) Technologie hilft sicherzustellen, dass Projektmappen wie e-Mail keine Daten übertragen, die als vertraulich eingestuft wurden. Organisationen können DLP-Funktionen vorhandene Produkte verhindern Datenverlust nutzen. Solche Funktionen Richtlinien einfach neu oder mithilfe einer Vorlage vom Software-Provider erstellt werden können.  
 
DLP-Technologie möglich Tiefe Inhaltsanalyse Schlüsselwort übereinstimmt, Wörterbuch übereinstimmt, regulärer Ausdrücke und andere Content-Prüfung Inhalte erkennen, die organisatorische DLP-Richtlinien verletzen. Für können DLP beispielsweise die folgenden Typen von Daten vermeiden: 

- Sozialversicherung und nationalen Nummern 
- Bankverbindung 
- Kreditkartennummer  
- IP-Adressen 

Einige DLP-Technologie ermöglichen auch die DLP-Konfiguration überschreiben (z. B. wenn eine Organisation Sozialversicherungsnummer Informationen zum Lohn-Prozessor benötigt). Darüber hinaus kann DLP so konfigurieren, dass Benutzer benachrichtigt werden, bevor sie sogar vertrauliche Informationen senden, die nicht übertragen werden soll. 

> [AZURE.TIP] Office 365 DLP-Funktionen können Sie Dokumente schützen. Lesen [Office 365 Kontrollen: Data Loss Prevention](https://blogs.office.com/2013/10/28/office-365-compliance-controls-data-loss-prevention/) Weitere Informationen.

## <a name="see-also"></a>Siehe auch

- [Azure Data Encryption Best Practices](azure-security-data-encryption-best-practices.md)
- [Azure Identity Management und Zugriff steuern Bewährte Sicherheitsmethoden](azure-security-identity-management-best-practices.md)
- [Azure Security-Teamblog](http://blogs.msdn.com/b/azuresecurity/)
- [Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx)

