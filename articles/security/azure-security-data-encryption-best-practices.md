<properties
   pageTitle="Sicherheit und Verschlüsselung Best Practices | Microsoft Azure"
   description="Dieser Artikel enthält eine Reihe von best Practices für die Sicherheit und Verschlüsselung in Azure Funktionen integriert."
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
   ms.author="yuridio"/>

#<a name="azure-data-security-and-encryption-best-practices"></a>Best Practices für Azure Sicherheit und Verschlüsselung

Eines der Datenschutz in der Cloud ist die möglichen Zustände auftreten, die Daten und für diesen Zustand verfügbaren Steuerelemente entfallen. Sicherheit und Verschlüsselung best Practices-Empfehlung werden zur Azure Data Staaten die folgenden Daten:

- Bei Rest: Diese enthält alle Informationen magnetisch oder optischen Datenträgern Speicherobjekte, Container und Typen, die statisch auf einem Datenträger vorhanden sein.

- In Transit: Wenn Daten übertragen zwischen Komponenten, Positionen oder Programme, z. B. über das Netzwerk über ein Servicebus (lokal Cloud und umgekehrt, einschließlich Hybriden wie ExpressRoute) oder bei einer Eingabe/Ausgabe-es wird betrachtet als in Bewegung.

In diesem Artikel besprechen wir eine Sammlung von best Practices für Azure Daten Sicherheit und Verschlüsselung. Diese best Practices stammen aus unserer Erfahrung Azure Datenschutz und Verschlüsselung und den Kunden.

Für jede best Practice erklären wir:

- Was ist die beste Vorgehensweise
- Warum soll, empfohlen
- Was möglicherweise scheitern am besten aktivieren
- Mögliche Alternativen empfohlen
- Wie erhalten Sie die besten aktivieren

Dieser Artikel Azure Sicherheit und Verschlüsselung Best Practices basiert auf einen Konsens und Azure-Plattformfunktionen und Features, wie sie zum Zeitpunkt dieser Artikel geschrieben wurde. Stellungnahmen und Technologie Zeitverlauf und dieser Artikel wird in regelmäßigen Abständen entsprechend aktualisiert.

Azure Data Sicherheit und Verschlüsselung Vorgehensweisen in diesem Artikel beschriebenen gehören:

- Mehrstufige Authentifizierung erzwingen
- Mit rollenbasierte Zugriffskontrolle (RBAC)
- Verschlüsseln von Azure virtuelle Computer
- Verwenden Sie Hardware Sicherheitsmodelle
- Verwalten mit Sicherheit
- SQL-Verschlüsselung aktivieren
- Schützen von Daten während der Übertragung
- Erzwingen der Verschlüsselung von Daten


## <a name="enforce-multi-factor-authentication"></a>Mehrstufige Authentifizierung erzwingen

Der erste Schritt beim Zugriff auf Daten und deren Steuerung in Microsoft Azure ist die Benutzerauthentifizierung. [Azure mehrstufige Authentifizierung (MFA)](../multi-factor-authentication/multi-factor-authentication.md) ist eine Methode zum Überprüfen der Identität des Benutzers mithilfe einer anderen Methode als nur einen Benutzernamen und ein Kennwort. Diese Authentifizierung Methode hilft Schutz Zugriff auf Daten und Applikationen Benutzer Nachfrage nach einem einfachen Vorgang.

Azure MFA für Benutzer aktivieren, wird eine zweite Sicherheitsebene Benutzer anmelden und Transaktionen hinzugefügt werden. In diesem Fall kann eine Transaktion Dokumente auf einem Dateiserver oder Ihre SharePoint Online zugreifen. Azure MFA hilft IT die Wahrscheinlichkeit verringern, dass beeinträchtigte Anmeldeinformationen auf Unternehmensdaten zugreifen kann.

Beispiel: Wenn Sie Azure MFA für die Benutzer erzwingen, so konfigurieren, dass ein Anruf oder SMS als Überprüfung verwenden, wenn die Anmeldeinformationen des Benutzers gefährdet ist der Angreifer werden alle Ressourcen zugreifen, da er nicht auf Telefon des Benutzers. Organisationen, die nicht mit diesem zusätzlichen Schutz hinzufügen, sind anfälliger für Anmeldeinformationen Diebstahl Angriff zu Daten gefährden.

Eine Alternative für Unternehmen zu Authentifizierung Steuerelement lokalen ist mit [Azure mehrstufige Authentifizierungsserver](../multi-factor-authentication/multi-factor-authentication-get-started-server.md)auch lokal MFA bezeichnet. Mit dieser Methode werden Sie weiterhin mehrstufige Authentifizierung und MFA Server lokalen durchsetzen.

Weitere Informationen zu Azure finden Sie im Artikel [Erste Schritte mit Azure mehrstufige Authentifizierung in der Cloud](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

## <a name="use-role-based-access-control-rbac"></a>Mit rollenbasierte Zugriffskontrolle (RBAC)
Beschränken des Zugriffs auf Grundlage der Sicherheitsprinzipien [benötigen](https://en.wikipedia.org/wiki/Need_to_know) und [Mindestberechtigung](https://en.wikipedia.org/wiki/Principle_of_least_privilege) . Dies ist für Organisationen, die Sicherheitsrichtlinien für den Datenzugriff erzwingen möchten. (Azure Role-Based Access Control, RBAC) kann verwendet werden, um Berechtigungen für Benutzer, Gruppen und Programme in einem bestimmten Bereich zuweisen. Der Geltungsbereich einer Zuweisung der Sicherheitsrolle kann ein Abonnement, eine Ressourcengruppe oder eine Ressource.

Nutzen Sie [integrierte RBAC-Rollen](../active-directory/role-based-access-built-in-roles.md) in Azure Benutzern Berechtigungen zuweisen. Verwenden Sie *Storage Konto Mitwirkenden* für Cloud-Operatoren, die Speicherkonten und *klassischen Storage Konto* Teilnehmerrolle klassische Speicherkonten zu verwalten. Cloud-Operatoren, die VMs und Konto verwalten, sollten Sie in der *Virtual Machine* Beitragendenrolle hinzugefügt.

Organisationen, die nicht Access-Steuerelement durch Funktionen wie RBAC erzwingen möglicherweise mehr Berechtigungen als notwendig für ihre Benutzer geben. Dies führt zu Daten Gefährdung durch einige Benutzer Zugriff auf Daten, die in erster Linie hätte.

Weitere Informationen zu RBAC Azure erfahren lesen [Azure_Role-Based Access Control](../active-directory/role-based-access-control-configure.md).

## <a name="encrypt-azure-virtual-machines"></a>Verschlüsseln von Azure Virtual Machines
Für viele Unternehmen ist [Verschlüsselung ruhender](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) ein obligatorischer Schritt Datenschutz, Compliance und datenhoheit. Azure Datenträger-Verschlüsselung können IT-Administratoren Windows und Linux IaaS Virtual Machine (VM) Datenträger verschlüsselt. Azure Datenträgerverschlüsselung nutzt der Branche BitLocker Standardfunktion von Windows und die Funktion DM-Crypt Linux Verschlüsselung für das Betriebssystem und die Datenträger bereitstellen.

Nutzen Sie Azure Datenträgerverschlüsselung schützen und die Daten der Sicherheitsrichtlinien und Compliance. Organisationen sollten auch mit Verschlüsselung um mit nicht autorisierten Datenzugriff Risiken zu mindern. Es wird auch empfohlen, Laufwerke vor dem Schreiben von Daten auf diese verschlüsseln.

Vergewissern Sie sich Ihrer VM-Datenmengen und Startvolume verschlüsseln, um Daten in Ihrem Konto Azure-Speicher schützen. Schützen Sie Schlüssel und geheime Schlüssel durch Nutzung der [Azure Key Vault](../key-vault/key-vault-whatis.md).

Berücksichtigen Sie für die lokale Windows Server folgende best Practices:

- Verwenden von [BitLocker](https://technet.microsoft.com/library/dn306081.aspx) zum Verschlüsseln von Daten
- Wiederherstellungsinformationen in AD DS gespeichert.
- Ist bedenken, dass BitLocker-Schlüssel kompromittiert wurde, empfiehlt es sich, dass entweder alle Instanzen der BitLocker-Metadaten aus dem Laufwerk Entfernen der Formatierung oder entschlüsseln und verschlüsseln das Laufwerk erneut.

Organisationen, die keine Verschlüsselung erzwingen eher Datenintegritätsprobleme wie böswillige oder nicht autorisierte Benutzer Daten stehlen ausgesetzt und nicht autorisierten Zugriff auf Daten in übersichtlich Konten gefährdet. Neben dieser Risiken nachzuweisen Unternehmen, die branchenspezifische Vorschriften einzuhalten, dass sie sorgfältig und korrekt Sicherheitskontrollen zu Sicherheit.

Weitere Informationen zur Verschlüsselung von Azure Datenträger erhalten lesen [Azure Disk Encryption für Windows und Linux IaaS VMs](azure-security-disk-encryption.md).

## <a name="use-hardware-security-modules"></a>Hardwaresicherheitsmodule verwenden

Verschlüsselungslösungen verwenden geheimer Schlüssel zum Verschlüsseln von Daten. Daher ist es wichtig, dass diese Schlüssel sicher gespeichert werden. Schlüsselverwaltungsdienst wird Bestandteil des Datenschutzes, da es zum Speichern von geheimen Schlüssel zum Verschlüsseln Daten genutzt werden.

Azure Datenträger verschlüsselt [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) steuern und verwalten Datenträger Schlüssel und geheime Informationen in Ihrem Abonnement Key Vault dabei sicherzustellen, dass alle virtuellen Laufwerke ruhende in Azure-Speicher verschlüsselt werden. Verwenden Sie Azure Key Vault Schlüssel und Richtlinienverwendung.

Es gibt viele inhärente Risiken ohne geeignete Sicherheitsmaßnahmen zum Schutz geheimer Schlüssel, die zum Verschlüsseln der Daten verwendet wurden. Wenn Angreifer Zugriff auf den geheimen Schlüssel haben, werden sie haben möglicherweise Zugriff auf vertrauliche Informationen und Daten entschlüsseln kann.

Erfahren Sie mehr über allgemeine Empfehlung für Certificate Management in Azure den Artikel [Certificate Management in Azure: Tipps zur](https://blogs.msdn.microsoft.com/azuresecurity/2015/07/13/certificate-management-in-azure-dos-and-donts/).

Weitere Informationen zu Azure Key Vault finden Sie [Erste Schritte mit Azure Schlüssel](../key-vault/key-vault-get-started.md).

## <a name="manage-with-secure-workstations"></a>Verwalten mit Sicherheit

Da der Großteil der Angriffe Ziel der Endbenutzer, wird der Endpunkt eines der primären Angriff. Wenn ein Angreifer Endpunkt gefährdet, kann er die Anmeldeinformationen des Benutzers Zugriff auf Unternehmensdaten nutzen. Die meisten Endpunkt Angriffe können die Tatsache nutzen, dass Endbenutzer Administratoren in ihren lokalen Arbeitsstationen.

Diese Risiken reduzieren mit einem sicheren Verwaltungscomputer. Wir empfehlen einen [Privilegierten Zugriff auf Arbeitsstationen (KRALLE)](https://technet.microsoft.com/library/mt634654.aspx) mit dem Reduzieren der Angriffsfläche auf Arbeitsstationen. Diese sichere Management-Workstations können Sie einige dieser mindern Angriffe helfen sicherzustellen, dass Ihre Daten sicherer. Stellen Sie sicher mit KRALLE sichern und Sperren der Arbeitsstation. Dies ist ein wichtiger Schritt, hohe Sicherheit für vertrauliche Konten, Aufgaben und Datenschutz gewährleisten.

Mangelnde Endgeräteschutz kann Ihre Daten gefährden, Sicherheitsrichtlinien auf allen Geräten anwenden, die verwendet werden, Daten, Daten unabhängig, (Cloud oder lokal) sicherzustellen.

Sie erhalten über Berechtigungen Lesen [Systemzugriff sichern](https://technet.microsoft.com/library/mt631194.aspx)Workstation zugreifen.

## <a name="enable-sql-data-encryption"></a>SQL-Verschlüsselung aktivieren

[Azure SQL-Datenbank transparente Verschlüsselung](https://msdn.microsoft.com/library/dn948096.aspx) (DSA) schützt gegen bösartige Aktivitäten durch Echtzeit-Verschlüsselung und Entschlüsselung der Datenbank zugeordneten Backups und Transaktionsprotokolldateien ruhende ohne Änderung der Anwendung ausführen.  TDE verschlüsselt die Speicherung der gesamten Datenbank einen symmetrischen Schlüssel Chiffrierschlüssel der Datenbank bezeichnet.

Selbst wenn der gesamte Speicher verschlüsselt ist, unbedingt auch Ihre Datenbank verschlüsselt. Dies ist eine Implementierung der mehrstufigen Verteidigungsstrategie zum Datenschutz. Wenn Sie [Azure SQL](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx) -Datenbank und Daten wie Kreditkarten- oder Sozialversicherungsnummern schützen möchten, können Sie Datenbanken mit FIPS 140-2 überprüft 256-Bit AES-Verschlüsselung verschlüsseln die erfüllt viele Industriestandards (z. B. HIPAA, PCI).

Es ist wichtig zu verstehen, dass [Puffer Pool Erweiterung](https://msdn.microsoft.com/library/dn133176.aspx) (BPE) Dateien nicht verschlüsselt sind, wenn eine Datenbank mit TDE verschlüsselt ist. Datei System Verschlüsselungstools wie BitLocker verwenden oder das [Verschlüsselnde Dateisystem](https://technet.microsoft.com/library/cc700811.aspx) (EFS) BPE verknüpften Dateien.

Seit einem autorisierten Benutzer wie ein Sicherheitsadministrator oder Datenbankadministrator die Daten zugreifen kann, auch wenn die Datenbank mit TDE, verschlüsselt sollten Sie auch die folgenden Vorschläge beachten:

- SQL-Authentifizierung auf der Datenbankebene
- Azure AD-Authentifizierung mit RBAC-Rollen
- Benutzer und Programme verwenden gesondert authentifizieren. So beschränken die Berechtigungen für Benutzer und Applikationen und Risiken bösartige Aktivitäten
- Implementieren können auf Datenbankebene festen Datenbankrollen (wie Db_datareader und Db_datawriter), oder Sie benutzerdefinierte Rollen für die Anwendung explizite Berechtigungen für ausgewählte Datenbankobjekte erstellen

Organisationen, die Datenbank-Verschlüsselung nicht verwenden möglicherweise anfälliger für Angriffe werden, die Daten in SQL-Datenbanken gefährden könnten.

Weitere Informationen zur Verschlüsselung von SQL TDE erhalten den Artikel [Transparente Verschlüsselung in Azure SQL-Datenbank](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx).

## <a name="protect-data-in-transit"></a>Schützen von Daten während der Übertragung

Schutz von Daten während der Übertragung sollte Bestandteil der Datenschutzstrategie. Da Daten hin-und an vielen Stellen verschoben werden, wird allgemein empfohlen, immer SSL/TLS-Protokolle verwenden, um Daten an unterschiedlichen Standorten austauschen. In einigen Fällen möchten den gesamten Kommunikationskanal zwischen lokalen und Cloud isolieren Infrastruktur mit einem virtuellen privaten Netzwerk (VPN).

Für Daten zwischen dem lokalen Infrastruktur und Azure sollten Sie entsprechende Sicherheitsmaßnahmen HTTPS oder VPN.

Für Unternehmen, die sicheren Zugriff auf mehrere Arbeitsstationen müssen, verwenden sich lokal in Azure, [Azure Standort-zu-Standort-VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md).

Für Organisationen, die sicheren Zugriff von einer Arbeitsstation im lokalen Azure, verwenden [Punkt-zu-Standort-VPN-](../vpn-gateway/vpn-gateway-point-to-site-create.md).

Größere Daten Mengen verschoben werden können, über ein dediziertes Hochgeschwindigkeits-WAN wie [ExpressRoute](https://azure.microsoft.com/services/expressroute/)verknüpfen. Wenn Sie ExpressRoute verwenden, können Sie auch die Daten auf der Anwendungsebene Verschlüsseln mit [SSL/TLS](https://support.microsoft.com/kb/257591) oder andere Protokolle für zusätzlichen Schutz.

Interagiert mit Azure Storage Azure-Portal finden alle Transaktionen über HTTPS. [Storage REST-API](https://msdn.microsoft.com/library/azure/dd179355.aspx) über HTTPS kann auch mit [Azure Storage](https://azure.microsoft.com/services/storage/) und [Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/)verwendet werden.

Wenn Organisationen zum Schutz von Daten während der Übertragung sind [Man in the Middle Angriffe](https://technet.microsoft.com/library/gg195821.aspx) [Abhören](https://technet.microsoft.com/library/gg195641.aspx) und Sitzungsübernahmen. Diese Angriffe kann zunächst Zugriff auf vertrauliche Daten.

Weitere Informationen zu Azure VPN-Optionen erhalten lesen [Planung und Entwurf für VPN-Gateway](../vpn-gateway/vpn-gateway-plan-design.md).

## <a name="enforce-file-level-data-encryption"></a>Erzwingen der Verschlüsselung von Daten

Eine zusätzliche Schutzebene, die die Sicherheit Ihrer Daten erhöhen können verschlüsselt die Datei selbst, unabhängig vom Speicherort Datei.

[Azure RMS](https://technet.microsoft.com/library/jj585026.aspx) verwendet Verschlüsselung, Identität und Autorisierung Richtlinien zum Sichern von Dateien und e-Mails. Azure RMS funktioniert über mehrere Geräte, Telefone, Tablets und PCs in Ihrer Organisation und außerhalb Ihrer Organisation schützen. Dies ist möglich, da Azure RMS ein Schutzniveau, die mit den Daten bleibt hinzugefügt, auch wenn es Ihrer Organisation wird.

Wenn Sie Azure RMS verwenden, um Ihre Dateien zu schützen, verwenden Sie Industriestandard Kryptografie mit [FIPS 140-2](http://csrc.nist.gov/groups/STM/cmvp/standards.html). Bei Nutzung von Azure RMS für den Datenschutz haben Sie die Gewissheit, die der Schutz der Datei bleibt, auch wenn Speicher kopiert wird, die nicht unter der Kontrolle der IT wie Cloud-Speicherservice. Dasselbe passiert mit e-Mail, freigegebene Dateien die Datei als Anlage einer e-Mail-Nachricht mit geschützt wie geschützte Anlage geöffnet.

Beim Planen von Azure RMS Annahme empfohlen Folgendes:

- Installieren der [RMS app freigeben](https://technet.microsoft.com/library/dn339006.aspx). Diese app integrieren Office Applikationen durch die Installation einer Office-add-in, dass Benutzer Dateien einfach direkt schützen können.
- Konfigurieren Sie Programme und Dienste Azure RMS unterstützen
- Erstellen Sie [benutzerdefinierte Vorlagen](https://technet.microsoft.com/library/dn642472.aspx) , die Ihre Bedürfnisse widerspiegeln. Beispiel: eine Vorlage für obere geheime Daten in alle geheimen anzuwenden im Zusammenhang mit e-Mails.

Organisationen, die schwache [Klassifizierung](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) und Datei möglicherweise anfälliger für Datenverluste. Ohne ordnungsgemäße Dateischutz werden Organisationen erhalten Einblicke, Missbrauch überwachen und bösartigen Zugriff auf Dateien zu verhindern.

Weitere Informationen zu Azure RMS erhalten [RMS Einstieg](https://technet.microsoft.com/library/jj585016.aspx)lesen.
