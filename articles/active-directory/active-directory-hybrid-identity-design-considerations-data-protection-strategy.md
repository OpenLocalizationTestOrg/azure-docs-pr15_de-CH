<properties
    pageTitle="Azure Active Directory hybride Identität Vorüberlegungen - definieren Datenschutzstrategie | Microsoft Azure"
    description="Sie definieren die Datenschutzstrategie für die hybrididentitätslösung Bedürfnissen, die Sie definiert."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/08/2016"
    ms.author="billmath"/>


# <a name="define-data-protection-strategy-for-your-hybrid-identity-solution"></a>Definieren Sie Strategie für die hybrididentitätslösung

In dieser Aufgabe definieren Sie Datenschutzstrategie für Ihre hybrididentitätslösung Business Anforderungen, denen in definiert:

- [Bestimmen des Datenschutzes](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)
- [Content Management ermitteln](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)
- [Access Control Anforderungen](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)
- [Reaktion auf Sicherheitsvorfälle ermitteln](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="define-data-protection-options"></a>Optionen definieren
Wie [bestimmen Sie Directory Synchronization](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)Anforderungen erläutert wurde, entfernt Microsoft Azure AD mit der Active Directory-Domänendienste (AD DS) synchronisieren kann lokal Diese Integration ermöglicht Organisationen Azure AD Anmeldeinformationen überprüfen, wenn sie versuchen, auf Ressourcen zuzugreifen. Dies kann für beide Szenarien: in anderen lokal und in der Cloud.  Zugriff auf Daten in Azure AD muss über einen Sicherheitstokendienst (STS).

Authentifiziert, wird der Benutzerprinzipalname (UPN) das Authentifizierungstoken und die replizierten Partition lesen und Container für die Domäne des Benutzers bestimmt. Informationen zur Existenz, Aktivierung und Rolle des Benutzers das Autorisierungssystem dient bestimmen, ob der angeforderte Zugriff Mieter Ziel in dieser Sitzung für diesen Benutzer autorisiert ist. Bestimmte autorisierten Aktionen (insbesondere Benutzer, Zurücksetzen des Kennworts erstellen) erstellt einen Audit-Trail von Tenant-Administrator zur Verwaltung von Richtlinien oder Ermittlungen verwendet werden kann.

Daten aus lokalen Datencenter in Azure-Speicher über eine Internetverbindung können nicht immer Datenvolumen, Bandbreite oder andere Faktoren möglich sein. [Azure Storage-Import/Export-Service](../storage/storage-import-export-service.md) bietet eine Hardware-Option für große Datenmengen im BLOB-Speicher setzen und abrufen. Sie können Festplatten [BitLocker verschlüsselt](https://technet.microsoft.com/library/dn306081#BKMK_BL2012R2) an Azure-Rechenzentrum senden, wo Cloud Operatoren lädt den Inhalt an das Speicherkonto oder können sie Ihre Azure Daten auf Ihren Laufwerken zurück. Verschlüsselte Datenträger werden für diesen Prozess (mit BitLocker-Schlüssel durch den Dienst während der Installation Auftrag generiert). BitLocker-Schlüssel wird separat in Azure bereitgestellt bieten Band wichtige Freigabe.

Da Daten bei der Übertragung in verschiedenen Szenarien stattfinden können, ist auch relevant, dass Microsoft Azure [virtuelle Netzwerke](https://azure.microsoft.com/documentation/services/virtual-network/) Mieter Datenverkehr, isolieren mit Maßnahmen auf Host und Gast Firewalls IP-Paketfilter, Port-Blockierung und HTTPS Endpunkte verwendet. Die meisten Azure internen Kommunikation, einschließlich Infrastruktur Infrastruktur und Infrastruktur-Kunden (lokal) sind jedoch ebenfalls verschlüsselt. Ein anderes wichtiges Szenario ist die Kommunikation in Azure-Datencentern. Microsoft verwaltet Netzwerke, um sicherzustellen, dass keine VM auszugeben oder die IP-Adresse eines anderen abzuhören. TLS/SSL wird beim Zugreifen auf Azure Storage oder SQL-Datenbanken oder Verbinden mit Cloud-Diensten verwendet. In diesem Fall obliegt Kundenadministrator TLS/SSL-Zertifikat erhalten und auf ihre Mieter Infrastruktur bereitstellen. Datentransfer Datenverkehr zwischen virtuellen Computern in der gleichen Bereitstellung oder zwischen in eine einzelne Bereitstellung über Microsoft Azure Virtual Network kann über verschlüsselte Kommunikationsprotokolle wie HTTPS oder SSL/TLS geschützt werden.

Wie [bestimmen Datenschutzes](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)Fragen beantwortet haben sollten Sie werden können, wie Ihre Daten geschützt werden sollen und wie die hybrididentitätslösung, helfen wird. Die Tabelle zeigt die Optionen von Azure unterstützt für jedes Szenario Data Protection.


| Datenschutzoptionen         | Ruhende in der cloud | Bei Rest lokal | Bei der Übertragung |
|---------------------------------|----------------------|---------------------|------------|
| BitLocker Drive Encryption      | X                    | X                   |            |
| SQL Server-Datenbanken verschlüsseln | X                    | X                   |            |
| VM-VM-Verschlüsselung             |                      |                     | X          |
| SSL/TLS                         |                      |                     | X          |
| VPN                             |                      |                     | X          |


>[AZURE.NOTE]
Lesen Sie [Compliance durch Feature](https://azure.microsoft.com/support/trust-center/services/) [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/) , um mehr über die Zertifizierungen, die jede Azure Service entspricht.
Da die Optionen für den Datenschutz einen mehrstufigen Ansatz verwenden, Vergleich zwischen diesen Optionen gelten nicht für diese Aufgabe. Stellen Sie sicher, dass Sie alle Optionen für die einzelnen Zustände genutzt, die die Daten.

## <a name="define-content-management-options"></a>Content-Management-Optionen definieren
Ein Vorteil von Azure AD zu einer Hybrid-Identitätsinfrastruktur ist der Vorgang aus Sicht der Endbenutzer vollständig transparent. Der Benutzer versucht, eine freigegebene Ressource zugreifen, die Ressource erfordert Authentifizierung der Benutzer müssen eine Authentifizierungsanfrage Azure AD Token und auf die Ressource zugreifen. Der gesamte Vorgang geschieht im Hintergrund ohne Benutzereingriff. Es kann auch Berechtigung für eine [Gruppe](active-directory-manage-groups.md#getting-started-with-access-management) von Benutzern bestimmte allgemeinen Aktionen durchführen können.

Organisationen, die Daten Datenschutz normalerweise erforderlich Daten für ihre Lösung. Wenn ihre aktuelle lokale Infrastruktur bereits Daten verwendet, kann Azure AD als Haupt-Repository für die Identität des Benutzers zu nutzen. Ein gängiges Tool ist lokal verwendete Daten [Data Classification Toolkit](https://msdn.microsoft.com/library/Hh204743.aspx) für Windows Server 2012 R2 bezeichnet. Dieses Tool können erkennen, klassifizieren und Schutz von Daten auf Dateiservern in der privaten Cloud. Es kann auch die [Automatische Klassifizierung der Datei](https://technet.microsoft.com/library/hh831672.aspx) in Windows Server 2012 dazu nutzen.

Wenn Ihre Organisation muss vertrauliche Dateien zu schützen, ohne neue Server lokal keine Daten haben, können sie Microsoft [Azure Rights Management-Dienst](https://technet.microsoft.com/library/JJ585026.aspx).  Azure RMS verwendet Verschlüsselung, Identität und Autorisierungsrichtlinien zum Sichern von Dateien und e-Mails und funktioniert über mehrere Geräte, Telefone, Tablets und PCs. Da Azure RMS Cloud-Dienst ist, besteht keine Notwendigkeit Vertrauensstellungen mit anderen Organisationen explizit konfigurieren, bevor Sie geschützten Inhalte freigeben können. Haben sie bereits ein Office 365 oder einem Azure AD-Verzeichnis wird automatisch Zusammenarbeit zwischen Organisationen unterstützt. Sie können auch nur die Verzeichnisattribute synchronisieren, die Azure RMS eine gemeinsame Identität für Ihre lokale Active Directory-Konten mithilfe von Azure Active Directory Synchronization Services (AAD Sync) oder Azure AD verbinden muss.

Ein wesentlicher Bestandteil der Content-Management ist zu verstehen, wer auf die Ressource zugreift, umfangreiche Protokollfunktion deshalb wichtig für Identitätsmanagement-Lösung. Azure AD enthält Log über 30 Tage einschließlich:

- Änderung der Rollenmitgliedschaft (ex: Benutzer, die globale Administratorrolle)
- Updates von Anmeldeinformationen (ex: Kennwort ändern)
- Verwaltung (ex: Überprüfen einer benutzerdefinierten Domäne entfernen einer Domäne)
- Hinzufügen oder Entfernen von Programmen
- Verwaltung (ex: hinzufügen, entfernen, Benutzer aktualisieren)
- Hinzufügen oder Entfernen von Lizenzen

>[AZURE.NOTE]
Lesen Sie [Microsoft Azure Sicherheit und Verwaltung des Überwachungsprotokolls](http://download.microsoft.com/download/B/6/C/B6C0A98B-D34A-417C-826E-3EA28CDFC9DD/AzureSecurityandAuditLogManagement_11132014.pdf) Protokollierungsfunktionen in Azure kennenzulernen.
Je nachdem wie Fragen in [Content Management-Anforderungen ermitteln beantwortet](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)sollten Sie möglicherweise festlegen, wie den Inhalt der hybrididentitätslösung verwaltet werden. Während alle Optionen, die in Tabelle 6 ausgesetzt Azure Active Directory integrieren können, ist es wichtig definieren geeigneter für Ihren Bedarf.

| Content-Management-Optionen                                                               | Vorteile                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | Nachteile                                                                                                                                                                                                                               |
|------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Zentrale lokal (Active Directory Rights Management Server)                      | Vollständige Kontrolle über die Daten klassifizieren Serverinfrastruktur <br> Integrierte Funktion in Windows Server ohne zusätzliche Lizenzen und Abonnements <br> Im Hybrid-Szenario mit Azure integrierbar <br> Information Rights Management (IRM) Funktionen unterstützt in Microsoft Online Services wie Exchange Online und SharePoint Online, Office 365 <br> Unterstützt lokale Microsoft-Serverprodukten wie Exchange Server, SharePoint-Server und Dateiserver, auf denen Windows Server und Datei Klassifizierung Infrastruktur (FCI) ausgeführt. | Höher, Wartung (halten Updates, Konfiguration und mögliche Upgrades), da IT besitzt den Server <br> Lokale Server-Infrastruktur erforderlich<br> Doesn'tleverage Azure Funktionen nativ                                     |
| Zentral in der Cloud (Azure RMS)                                                     | Im Vergleich zu lokalen Lösung einfacher zu verwalten <br> Mit AD DS im Hybrid-Szenario integrierbar <br>  Integriert mit Azure <br> Benötigt einen Server lokal um den Dienst bereitstellen <br> Unterstützt lokale Microsoft-Produkten wie Exchange Server, SharePoint-Server und Dateiserver, die Windows Server Dateiklassifizierung Infrastruktur (FCI und) <br> IT haben vollständige Kontrolle über ihre Mieter Schlüssel mit BYOK.                                                                                    | Ihre Organisation muss ein Cloud-Abonnement verfügen, RMS unterstützt <br> Ihre Organisation muss ein Azure AD-Verzeichnis Benutzerauthentifizierung für RMS unterstützen                                                                                  |
| Hybride (Azure RMS integriert, für lokale Active Directory Rights Management-Server) | Dieses Szenario sammelt Vorteile, zentrale lokal und in der Cloud.                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Ihre Organisation muss ein Cloud-Abonnement verfügen, RMS unterstützt <br> Ihre Organisation muss ein Azure AD-Verzeichnis Benutzerauthentifizierung für RMS unterstützen, <br> Erfordert eine Verbindung zwischen Azure-Clouddienst und lokale Infrastruktur |

## <a name="define-access-control-options"></a>Zugriffssteuerungsoptionen definieren
Mithilfe der Authentifizierung steuern Autorisierung und Zugriff auf Funktionen in Azure AD ermöglichen Ihrem Unternehmen eine zentrale Identität Repository mit Benutzern und Partnern einmaliges Anmelden (SSO) verwenden, wie in der folgenden Abbildung dargestellt werden:

![](./media/hybrid-id-design-considerations/centralized-management.png)

Zentrales Management und Integration in andere Verzeichnisse

Azure Active Directory bietet einmaliges Anmelden für Tausende von SaaS-Applikationen und lokalen web-Applikationen. Lesen Sie die [Azure Active Directory Federation Kompatibilitätsliste: Drittanbietern Identitätsanbieter, mit der Anmeldung zu implementieren](https://msdn.microsoft.com/library/azure/jj679342.aspx) Artikel über Drittanbieter SSO, die von Microsoft getestet wurden. Diese Funktion ermöglicht B2B-Szenarien implementieren und Kontrolle von Identitäts- und Zugriffsmanagement. Jedoch während der B2B Prozess entwerfen die Authentifizierungsmethode beachten, die vom Partner verwendeten und überprüfen, ob diese Methode von Azure unterstützt wird. Derzeit sind Methoden, die von Azure AD unterstützt:

- Security Assertion Markup Language (SAML)
- OAuth
- Kerberos
- Token
- Zertifikate


>[AZURE.NOTE] [Azure Active Directory-Authentifizierungsprotokolle](https://msdn.microsoft.com/library/azure/dn151124.aspx) , um mehr Details über jedes Protokoll und seine Funktionen in Azure lesen.

Mit Unterstützung des Azure AD, können mobile Business Applications einfach Mobile Dienste Authentifizierung auf Mitarbeiter ihre mobilen Anwendung mit ihrem Unternehmen Active Directory-Anmeldeinformationen anmelden. Mit diesem Feature werden Azure AD als Identitätsanbieter in Mobile Dienste neben mit anderen Identitätsanbietern bereits unterstützt (einschließlich Microsoft Accounts, Facebook-ID, Google-ID und Twitter-ID). Lokalen apps verwendet die Anmeldeinformationen des Benutzers am des Unternehmens AD DS, sollte der Zugriff von Partnern und Benutzer aus der Cloud transparent sein. Sie können die bedingte Benutzerkontensteuerung ASP.NET-Webanwendungen (Cloud-basiert), Web-API, Microsoft Cloud-Dienste, 3rd Party SaaS-Applikationen und systemeigenen Clientanwendungen (Mobil) verwalten und haben die Vorteile der Sicherheit, Überwachung, reporting zentral. Allerdings empfiehlt es sich dies in einer nicht produktiven Umgebung oder mit begrenzten Benutzer bestätigen.


>[AZURE.TIP] Es ist wichtig zu erwähnen, dass Azure AD nicht Gruppenrichtlinien wie AD DS. Gerät-Management-Lösung wie [Windows Intune](https://technet.microsoft.com/library/jj676587.aspx)benötigen Sie für Geräte durchsetzen.

Nachdem der Benutzer authentifiziert ist, Azure AD-unbedingt die Zugriffsstufe auswerten, dass der Benutzer es. Die Zugriffsebene, die Benutzer über eine Ressource kann variieren, während Azure AD zusätzliche Sicherheitsstufe durch Steuern des Zugriffs auf Ressourcen hinzufügen kann, Sie müssen Denken Sie auch daran, dass die Ressource selbst auch eigene Zugriffssteuerungsliste, wie die Zugriffssteuerungslisten für Dateien auf einem Dateiserver. Abbildung werden die Ebenen der Zugriffskontrolle, die Sie im Hybrid-Szenario zusammengefasst:

![](./media/hybrid-id-design-considerations/accesscontrol.png)


Jeder Aktivität im Diagramm in Abbildung X zeigte stellt eine Access Control Szenario, das von Azure AD abgedeckt werden kann. Haben Sie eine Beschreibung der einzelnen Szenarios:

1. bedingten Zugriff auf Programme, die lokal gehostet: können registrierte Geräte mit Richtlinien für Applikationen AD FS mit Windows Server 2012 R2 verwenden. Weitere Informationen zum Einrichten von bedingten Zugriff für lokale finden Sie unter [Einrichten von lokalen Zugangskontrolle mit Azure Active Directory Geräteregistrierung](active-directory-conditional-access-on-premises-setup.md).
2. Zugriffskontrolle auf Azure-Verwaltungsportal: Azure hat auch Zugriff auf die Management-Portal mit RBAC (Rolle Based Access Control) gesteuert. Diese Methode ermöglicht dem Unternehmen, die eine Person durchführen kann, hat er Zugriff auf Azure-Verwaltungsportal Vorgänge beschränken. Mit RBAC zum Steuern des Zugriffs auf das Portal, IT-Administratoren ca Stellvertretungszugriff mit Access managementansätze:

 - Zuweisung der Sicherheitsrolle Gruppenbasierte: können Sie Access Azure AD-Gruppen synchronisiert werden aus dem lokalen Active Directory zuweisen. Dadurch können Sie vorhandene Investitionen nutzen, die Ihre Organisation in Werkzeuge und Prozesse zum Verwalten von Gruppen. Sie können auch die delegierte Verwaltungsfunktion Azure AD Premium.
 - Integrierte Rollen in Azure nutzen: Sie verwenden drei Rollen, Besitzer Mitwirkende und Leser um sicherzustellen, dass Benutzer und Gruppen berechtigt sind, die Aufgaben müssen sie ihre Arbeit.
 - Zugriff auf Ressourcen präzise: Zuweisen Rollen für Benutzer und Gruppen für ein bestimmtes Abonnement, Ressourcengruppe oder eine einzelne Azure Ressource wie einer Website oder einer Datenbank. Auf diese Weise können Sie sicherstellen, dass Benutzer Zugriff auf die benötigten Ressourcen und keinen Zugriff auf Ressourcen, die sie nicht verwalten müssen.

>[AZURE.NOTE] [Rollenbasierte Zugriffskontrolle in Azure](https://azure.microsoft.com/updates/role-based-access-control-in-azure-preview-portal/) , um weitere Einzelheiten zu dieser Funktion lesen. Für Entwickler, die Programme erstellen und die Zugriffskontrolle für sie anpassen möchten, kann es auch Azure AD Anwendungsrollen für die Autorisierung verwenden. Überprüfen Sie diese [WebApp RoleClaims DotNet wird](https://github.com/AzureADSamples/WebApp-RoleClaims-DotNet) Ihre app erstellen um diese Funktion zu verwenden.

3. Zugangskontrolle für Office 365-Applikationen mit Windows Intune: IT-Administratoren können bedingte Geräterichtlinien schützen Unternehmensressourcen gleichzeitig ermöglicht Information Worker auf kompatiblen Geräten Zugriff auf die Dienste bereitstellen. Weitere Informationen finden Sie unter [Bedingte Geräterichtlinien für Office 365-Diensten](active-directory-conditional-access-device-policies.md).

4. Zugangskontrolle für Saas-apps: [dieses Feature](http://blogs.technet.com/b/ad/archive/2015/06/25/azure-ad-conditional-access-preview-update-more-apps-and-blocking-access-for-users-not-at-work.aspx) können Sie Zugriffsregeln pro Anwendung mehrstufige Authentifizierung und Zugriff für Benutzer nicht in einem vertrauenswürdigen Netzwerk blockieren. Sie können Regeln mehrstufige Authentifizierung für alle Benutzer gelten, die für die Anwendung oder nur für Benutzer in bestimmten Sicherheitsgruppen zugewiesen sind. Benutzer können mehrstufige Authentifizierung erforderlich ausgeschlossen werden, wenn sie die Anwendung zugreifen, eine IP-Adresse, die in der Organisation Netzwerk.

Da die Zugriffsregeln einen mehrstufigen Ansatz verwenden, Vergleich zwischen diesen Optionen gelten nicht für diese Aufgabe. Stellen Sie sicher, dass Sie alle Optionen für jedes Szenario genutzt, die Steuerung des Zugriffs auf Ressourcen erfordert.

## <a name="define-incident-response-options"></a>Reaktion auf Sicherheitsvorfälle definieren
Azure AD unterstützen IT-Identität potenzielle Sicherheitsrisiken in der Umgebung durch die Überwachung der Benutzeraktivität können IT Azure AD Access nutzen und Verwendungsberichte Funktion erhalten Sie Einblick in die Integrität und Sicherheit des Verzeichnisses Ihrer Organisation. Mit diesen Informationen bestimmen IT Admin besser Sicherheitsrisiken liegen kann, damit sie angemessen, diese Risiken planen können.  [Azure AD Premium-Abonnement](active-directory-get-started-premium.md) verfügt über eine Reihe von Sicherheits-Reports können IT, diese Informationen zu erhalten. [Azure AD Reports](active-directory-view-access-usage-reports.md) werden wie folgt kategorisiert:

- **Anomalie Berichte**: enthalten Anmeldeereignisse, die zu abweichenden gefunden wurden. Soll Sie solche Aktivitäten und können entscheiden, ob ein Ereignis verdächtige ermöglichen.
- **Integrierte Bericht**: Einblicke in wie Cloudanwendungen in Ihrer Organisation verwendet werden. Azure Active Directory bietet Integration mit Tausenden von Cloudanwendungen.
- **Fehlerberichte**: Fehler bei der Bereitstellung von Konten zu externen Applikationen anzugeben.
- **Benutzerspezifische Berichte**: Geräte-Zeichen in Daten für einen bestimmten Benutzer anzeigen.
- **Aktivitätsprotokolle**: alle überwachten Ereignisse innerhalb der letzten 24 Stunden, 7 Tagen oder 30 Tagen sowie Gruppe Aktivität ändert und Kennwort zurücksetzen und Registrierung Aktivität enthalten.

>[AZURE.TIP]
Incident Response Team bearbeiten kann auch helfen anderer Bericht ist der Bericht [mit verlorene Anmeldeinformationen](http://blogs.technet.com/b/ad/archive/2015/06/15/azure-active-directory-premium-reporting-now-detects-leaked-credentials.aspx) .  Diesem Bericht werden keine Übereinstimmung zwischen diesen verlorene Anmeldeinformationen und Ihrem Mandanten.

Weitere Berichte in Azure AD, die während der Reaktion auf Sicherheitsvorfälle Untersuchung verwendet werden und sind:

- **Passwort-Aktivität**: Admin bieten Einblicke in das Zurücksetzen von Kennwörtern wie aktiv in der Organisation verwendet wird.
- **Passwort Registrierungsaktivität**: Einblicke in die Benutzer ihre Methoden zum Zurücksetzen von Kennwörtern registriert haben und welche Methoden sie ausgewählt haben.
- **Gruppenaktivität**: Stellt ein Änderungsprotokoll der Gruppe (ex: Benutzer hinzugefügt oder entfernt) in der eingeleitet.

Nutzen Sie zusätzlich die zentrale reporting-Funktionen für Azure AD Premium auch bei einem Incident Response Untersuchung können IT genutzt werden können Prüfungsbericht Informationen wie:

- Änderung der Rollenmitgliedschaft (ex: Benutzer, die globale Administratorrolle)
- Updates von Anmeldeinformationen (ex: Kennwort ändern)
- Verwaltung (ex: Überprüfen einer benutzerdefinierten Domäne entfernen einer Domäne)
- Hinzufügen oder Entfernen von Programmen
- Verwaltung (ex: hinzufügen, entfernen, Benutzer aktualisieren)
- Hinzufügen oder Entfernen von Lizenzen

Da die Optionen für die Reaktion auf Vorfälle übernehmen einen mehrstufigen Ansatz verwenden, Vergleich zwischen diesen Optionen gelten nicht für diese Aufgabe. Stellen Sie sicher, dass Sie alle Optionen für jedes Szenario genutzt, die Azure AD reporting-Funktionen als Teil Ihres Unternehmens Sicherheitsvorfällen verwenden müssen.

## <a name="next-steps"></a>Nächste Schritte
[Bestimmen der Hybrid Identity Management-Aufgaben](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)


## <a name="see-also"></a>Siehe auch
[Design Considerations (Übersicht)](active-directory-hybrid-identity-design-considerations-overview.md)
