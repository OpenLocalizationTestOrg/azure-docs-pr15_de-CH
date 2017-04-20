<properties
   pageTitle="Übersicht über die Sicherheit im Datenspeicher See | Microsoft Azure"
   description="Verstehen Sie, wie Azure See Datenspeicher einen großen sicherer Datenspeicher"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/02/2016"
   ms.author="nitinme"/>

# <a name="security-in-azure-data-lake-store"></a>Sicherheit in Azure See Datenspeicher

Viele Unternehmen sind große Datenanalyse für Einblicke zu smart Entscheidungen nutzen. Eine Organisation kann komplexe und regulierten Umgebung mit immer mehr Benutzer verfügen. Es ist entscheidend für ein Unternehmen sicherstellen, dass wichtige Geschäftsdaten sicher, mit den richtigen Zugriffsberechtigungen für einzelne Benutzer gespeichert ist. Azure See Datenspeicher soll diese Sicherheit erfüllen. In diesem Artikel erfahren Sie mehr über die Sicherheitsfunktionen von Datenspeicher-See einschließlich:

* Authentifizierung
* Autorisierung
* Netzwerkisolation
* Datenschutz
* Überwachung

## <a name="authentication-and-identity-management"></a>Authentifizierung und management

Authentifizierung ist der Vorgang durch den Identität eines Benutzers überprüft wird, wenn der Benutzer interagiert mit See Datenspeicher oder jeden Dienst auf See Datenspeicher herstellt. Identitätsmanagement und Authentifizierung verwendet See Datenspeicher [Azure Active Directory](../active-directory/active-directory-whatis.md), umfassendes Identitäts- und Cloudlösung für die Verwaltung, die vereinfacht die Verwaltung von Benutzern und Gruppen.

Jede Azure-Abonnement kann eine Instanz von Azure Active Directory zugeordnet werden. Nur Benutzer und Dienstidentitäten in Azure Active Directory-Dienstes definiert Ihr Konto See Datenspeicher mithilfe von Azure-Portal Befehlszeilentools zugreifen oder über Clientanwendungen erstellt Ihrer Organisation mithilfe von Azure Data Lake Speicher SDK. Vorteile der Verwendung von Azure Active Directory als zentrale Zugriffssteuerungsmechanismus sind:

* Identity Lifecycle Management vereinfacht. Die Identität eines Benutzers oder eines Dienstes (ein principal Dienstidentität) kann schnell erstellt und schnell durch Löschen oder deaktivieren das Konto im Verzeichnis gesperrt.

* Mehrstufige Authentifizierung. [Mehrteilige Authentifizierung](../multi-factor-authentication/multi-factor-authentication.md) bietet eine zusätzliche Sicherheitsebene für Benutzer anmelden und Transaktionen.

* Authentifizierung von einem Client über einen geöffneten Standardprotokoll OAuth oder OpenID.

* Föderation mit Enterprise-Verzeichnisdienste und Cloud Identitätsanbieter.

## <a name="authorization-and-access-control"></a>Autorisierung und Zugriffskontrolle

Nachdem Active Directory Azure Benutzerauthentifizierung, damit der Benutzer Azure See Datenspeicher zugreifen kann Zugriffsberechtigungen Autorisierung See Datenspeicher. Datenspeicher See trennt Autorisierung für Konto und Daten Aktivitäten auf folgende Weise:

* [Rollenbasierte Zugriffskontrolle](../active-directory/role-based-access-control-what-is.md) (RBAC) vorgesehenen Azure für die Kontenverwaltung
* POSIX ACL für den Zugriff auf Daten im Speicher


### <a name="rbac-for-account-management"></a>RBAC für die Kontenverwaltung

Vier grundlegende Rollen sind für See Datenspeicher definiert. Die Rollen ermöglichen verschiedene Operationen für einen Datenspeicher See Azure-Portal, PowerShell-Cmdlets und REST-APIs. Der Besitzer und die Teilnehmer können verschiedener Verwaltungsfunktionen des Kontos Aufgaben. Sie können Benutzer, die nur mit Daten interagieren, Rolle zuweisen.

![RBAC-Rollen] (./media/data-lake-store-security-overview/rbac-roles.png "RBAC-Rollen")

Beachten Sie, dass auch das Management von Rollen zugewiesen sind, einige Rollen Zugriff auf die Daten auswirken. Sie müssen mit ACLs auf Vorgänge, die ein Benutzer auf dem System ausführen können. Die folgende Tabelle zeigt eine Zusammenfassung der Verwaltung und Datenzugriffsrechte für die Standardrollen.

| Rollen                    | Verwaltung               | Datenzugriffsrechte | Erklärung |
| ------------------------ | ------------------------------- | ------------------ | ----------- |
| Keine Rolle         | Keine                            | ACL unterliegt    | Der Benutzer können der Azure-Portal oder Azure PowerShell-Cmdlets See Datenspeicher durchsuchen. Der Benutzer kann nur die Befehlszeilentools verwenden.
| Besitzer  | Alle  | Alle  | Die Rolle ist ein Super-User. Diese Rolle kann alles verwalten und hat vollständigen Zugriff auf Daten.
| Reader   | Schreibgeschützt  | ACL unterliegt    | Rolle sehen alles zur Kontenverwaltung wie die Benutzer die Rolle zugewiesen wird. Rolle kann nicht ändern.   |
| Teilnehmer              | Alle außer hinzufügen und Entfernen von Rollen | ACL unterliegt    | Die Rolle eines Beitragenden kann einige Aspekte eines Kontos Bereitstellungen erstellen und Verwalten von Alerts verwalten. Der Beitragendenrolle nicht hinzufügen oder Entfernen von Rollen.
| Benutzeradministrator-Zugriff | Hinzufügen und Entfernen von Rollen            | ACL unterliegt    | Der Benutzeradministratorrolle Access kann Benutzerzugriff auf Konten verwalten. |

Eine Anleitung finden Sie unter [Zuweisen von Benutzern oder Sicherheitsgruppen auf See Datenspeicher](data-lake-store-secure-data.md#assign-users-or-security-groups-to-azure-data-lake-store-accounts).

### <a name="using-acls-for-operations-on-file-systems"></a>Mithilfe von ACLs für Operationen an Dateisystemen

See Datenspeicher ist ein hierarchisches Dateisystem wie Hadoop verteilt Datei System bietet und [POSIX-ACLs](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists)unterstützt. Lesen (R) gesteuert, schreiben (w) und ausführen (x) Berechtigungen für Ressourcen, die für die Rolle Eigentümer, Gruppe und für andere Benutzer und Gruppen. In der See Informationsspeicher Öffentlicher Datenvorschau (aktuelle Version) werden ACLs im Stammordner, Unterordner und einzelne Dateien. Die ACLs in den Stammordner anwenden gelten auch für alle Unterordner und Dateien.

Wir empfehlen Sie ACLs für mehrere Benutzer definieren mit [Sicherheitsgruppen](../active-directory/active-directory-accessmanagement-manage-groups.md). Hinzufügen von Benutzern zu einer Sicherheitsgruppe, und weisen Sie die ACLs für eine Datei oder einen Ordner dieser Sicherheitsgruppe. Dies ist nützlich, wenn Sie benutzerdefinierte Zugriff gewähren möchten, weil Sie nur durch Hinzufügen von bis zu neun Einträge für den benutzerdefinierten Zugriff. Weitere Informationen dazu, wie Sie mithilfe von Azure Active Directory-Sicherheitsgruppen besser sichern Daten im Datenspeicher See finden Sie unter [Benutzer oder Gruppe als ACLs im Dateisystem Azure See Datenspeicher zuweisen](data-lake-store-secure-data.md#filepermissions).

![Liste Standard- und benutzerdefinierten Zugriff] (./media/data-lake-store-security-overview/adl.acl.2.png "Liste Standard- und benutzerdefinierten Zugriff")

## <a name="network-isolation"></a>Netzwerkisolation

Mit See Datenspeicher zu steuern des Zugriffs auf Datenspeicher auf Netzwerkebene. Sie können Firewalls einrichten und einen IP-Adressbereich für vertrauenswürdige Clients definieren. Mit dem IP-Adressbereich können nur Clients, die eine IP-innerhalb des definierten Bereichs Adresse See Datenspeicher.

![Firewall Settings und IP-Zugriff] (./media/data-lake-store-security-overview/firewall-ip-access.png "Firewall Settings und IP-Adresse")

## <a name="data-protection"></a>Datenschutz

Organisationen möchten sicherstellen, dass ihre unternehmenskritischen Daten während des gesamten Lebenszyklus sicher ist. Daten während der Übertragung verwendet See Datenspeicher das Industriestandardprotokoll Transport Layer Security (TLS) um Daten zu schützen, die zwischen einem Client und einem Datenspeicher See.

Datenschutz für ruhende Daten werden in zukünftigen Versionen verfügbar.

## <a name="auditing-and-diagnostic-logs"></a>Überwachung und Diagnose-Protokolle

Sie können überwachen oder diagnostischen anmeldet, je nachdem, ob Sie Protokolle für Management-Aktivitäten oder Daten-Aktivitäten sind.

*  Management-Aktivitäten verwenden Azure-Ressourcen-Manager-APIs und Azure-Portal über Überwachungsprotokolle dargestellt werden.
*  Daten WebHDFS anderen APIs verwenden und Azure-Portal über Diagnoseprotokolle dargestellt werden.

### <a name="auditing-logs"></a>Überwachen von Protokollen

Zur Einhaltung der Vorschriften erfordern eine Organisation angemessene Audit-Trails, in Vorkommnisse graben muss. Datenspeicher See verfügt über integrierte Überwachung und protokolliert alle Konto-Aktivitäten.

Account Management Prüfpfade zeigen Sie an und wählen Sie die Spalten, die Sie protokollieren möchten. Sie können auch Überwachungsprotokolle Azure-Speicher exportieren.

![Audit-Protokolle] (./media/data-lake-store-security-overview/audit-logs.png "Audit-Protokolle")

### <a name="diagnostic-logs"></a>Diagnoseprotokolle

Sie können Datenzugriff Prüfpfade in Azure-Portal (im Diagnostic) festlegen und registrieren Azure BLOB-Speicher, in dem die Protokolle gespeichert werden.

![Diagnoseprotokolle] (./media/data-lake-store-security-overview/diagnostic-logs.png "Diagnoseprotokolle")

Nach der Diagnose konfigurieren können Sie die Protokolle auf der Registerkarte **Diagnoseprotokolle** anzeigen.

Weitere Informationen zum Arbeiten mit Diagnoseprotokolle Azure See Datenspeicher finden Sie unter [Zugriff Diagnoseprotokolle See Datenspeicher](data-lake-store-diagnostic-logs.md).

## <a name="summary"></a>Zusammenfassung

Unternehmen fordern eine Analytics-Cloud, die sicher und einfach zu verwenden ist. Azure See Datenspeicher soll können diese Vorschriften Identitätsmanagement und Authentifizierung über Azure Active Directory-Integration, ACL-Autorisierung, Netzwerkisolation in Transit und Verschlüsselung (in der Zukunft), sowie die Überwachung.

Möchten Sie neue Funktionen im Datenspeicher See, senden Sie uns Feedback [Daten See Speicher UserVoice Forum](https://feedback.azure.com/forums/327234-data-lake).

## <a name="see-also"></a>Siehe auch

- [Übersicht über Azure See Datenspeicher](data-lake-store-overview.md)
- [Erste Schritte mit See Datenspeicher](data-lake-store-get-started-portal.md)
- [Sichere Daten im Datenspeicher See](data-lake-store-secure-data.md)
