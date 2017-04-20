<properties
   pageTitle="Azure Security Center und Azure SQL-Datenbank-Dienst | Microsoft Azure"
   description="Dieser Artikel beschreibt das Sicherheitscenter die Datenbanken in Azure SQL-Datenbank sichern, helfen Sie."
   services="sql-database"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-and-azure-sql-database-service"></a>Azure Security Center und Azure SQL-Datenbank-Dienst

[Azure Security Center](https://azure.microsoft.com/documentation/services/security-center/) hilft zu verhindern, erkennen und reagieren auf Gefahren. Bietet integrierte Überwachung und Policy-Management über Abonnements Azure, Gefahren, die andernfalls vielleicht übersehen würden und arbeitet mit einem ausgedehnten System Security Solutions hilft bei der Ermittlung.

Dieser Artikel beschreibt das Sicherheitscenter die Datenbanken in Azure SQL-Datenbank sichern, helfen Sie.

## <a name="why-use-security-center"></a>Wozu dient das Sicherheitscenter?

Security Center können Sie die Daten in SQL-Datenbank mit Einblick in die Sicherheit aller Server und Datenbanken sichern. Mit Security Center können Sie:

- Definieren Sie Richtlinien für die SQL-Datenbank-Verschlüsselung und Überwachung.
- Überwacht die Sicherheit der SQL-Datenbank-Ressourcen über Ihre Abonnements.
- Schnell erkennen und Beheben von Sicherheitsproblemen.
- Integrieren Sie Warnung von [Erkennung Azure SQL-Datenbank](../sql-database/sql-database-threat-detection-get-started.md).

Neben Ihrer SQL-Datenbank-Ressourcen zu schützen, bietet Security Center auch Überwachung der Sicherheit und Management von Azure VMs, Clouddienste Anwendungsdienste, virtuelle Netzwerke und mehr. Erfahren Sie mehr über das Sicherheitscenter [hier](security-center-intro.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Zunächst mit Sicherheit müssen Sie ein Abonnement für Microsoft Azure. Freie Ebene des Security Center ist mit Ihrem Abonnement aktiviert. Weitere Informationen zum Security Center frei und Standard Ebenen finden Sie unter [Security Center Preise](https://azure.microsoft.com/pricing/details/security-center/).

Security Center unterstützt rollenbasierten Zugriff. Weitere Informationen über rollenbasierte Zugriffskontrolle (RBAC) in Azure finden Sie unter [Azure Active Directory Role-based Access Control](../active-directory/role-based-access-control-configure.md). Security Center FAQ enthält Informationen [wie Berechtigungen im Sicherheitscenter](security-center-faq.md#how-are-permissions-handled-in-azure-security-center)behandelt werden.

## <a name="access-security-center"></a>Access Security Center

Das Sicherheitscenter wird aus dem [Azure-Portal](https://azure.microsoft.com/features/azure-portal/)zugreifen. [Das Portal anmelden](https://portal.azure.com/) und wählen Sie die **Option Security Center**.

![Security Center-option][1]

Blatt **Security Center** wird geöffnet.
![Blatt Security Center][2]

## <a name="set-security-policy"></a>Festgelegte Sicherheitsrichtlinie

Eine Sicherheitsrichtlinie definiert die Gruppe von Steuerelementen, die für das angegebene Abonnement oder Ressourcengruppe Ressourcen empfohlen. Im Sicherheitscenter definieren Sie Richtlinien für Ihre Abonnements oder Ressourcengruppen Sicherheitsbedarfs Ihres Unternehmens und der Anwendung oder die Daten in jedem Abonnement.

Eine Richtlinie Vorschläge zur Überwachung von SQL und Verschlüsselung SQL transparent (DSA) anzeigen lassen.

- Beim Einschalten der **SQL Überwachung und Erkennung**empfiehlt Sicherheitscenter Überwachung des Zugriffs auf Azure-Datenbank für Compliance und erweiterte Erkennung und Untersuchung Zwecke aktiviert werden.
- **SQL transparente Verschlüsselung**Sicherheitscenter empfiehlt einschalten die Verschlüsselung ruhender für Azure SQL-Datenbank, zugeordneten Backups und Transaktionsprotokolldateien aktiviert werden.

Wählen Sie zum Festlegen einer Sicherheitsrichtlinie Kachel **Richtlinien** auf der Security Center. Wählen Sie auf die **Sicherheitsrichtlinie** den Dauerauftrag die Sicherheitsrichtlinie aktivieren soll. Wählen Sie **Prevention-Richtlinie aus** , und **schalten Sie die Sicherheitsaspekte, die Sie für dieses Abonnement verwenden möchten** .
![Sicherheitsrichtlinien][3]

Informationen finden Sie unter [Festlegen von Sicherheitsrichtlinien](security-center-policies.md).

## <a name="manage-security-recommendation"></a>Verwalten von sicherheitsempfehlung

Sicherheitscenter analysiert regelmäßig den Sicherheitsstatus Ihrer Azure-Ressourcen. Beim Sicherheitscenter Sicherheitslücken identifiziert, erstellt Recommendations. Empfehlung führen Sie durch den Prozess des Konfigurierens benötigten Steuerelemente.

Nach dem Festlegen einer Sicherheitsrichtlinie analysiert Sicherheitscenter des Sicherheitsstatus Ihrer Ressourcen zur Identifizierung möglicher Sicherheitslücken. Die Vorschläge werden im Tabellenformat angezeigt, wobei jede Zeile eine Empfehlung darstellt. Verwenden Sie folgende Tabelle als Referenz erfahren Sie verfügbaren Vorschläge für Azure SQL-Datenbank und macht jede Empfehlung, wenn Sie anwenden. Empfehlung auswählen gelangen Sie zu einem Artikel, der erklärt, wie die Empfehlung in Security Center implementiert.

| Empfehlung | Beschreibung |
| ----- | ----- |
| [Überwachung und Bedrohung für SQL Server aktivieren](security-center-enable-auditing-on-sql-servers.md) | Empfiehlt, Überwachung und Bedrohung Erkennung für SQL-Datenbankserver zu aktivieren. (Nur SQL-Datenbank-Dienst. Nicht enthalten Microsoft SQL Server auf den virtuellen Computern.) |
| [Aktivieren Sie die Überwachung und Bedrohung Erkennung auf SQL-Datenbanken](security-center-enable-auditing-on-sql-databases.md) | Empfiehlt, dass Sie Überwachung und Bedrohung Erkennung für SQL Datenbank Datenbanken aktivieren. (Nur SQL-Datenbank-Dienst. Nicht enthalten Microsoft SQL Server auf den virtuellen Computern.) |
| [Transparente Verschlüsselung aktivieren](security-center-enable-transparent-data-encryption.md) | Empfiehlt, Aktivieren der Verschlüsselung für SQL-Datenbanken. (Nur SQL Database Service.) |

Vorschläge für Azure Ressourcen finden wählen Sie **Recommendations** Kachel auf das Sicherheitscenter aus Wählen Sie Blade **Recommendations** Empfehlung angezeigt. In diesem Beispiel wählen wir **ermöglichen Überwachung und Erkennung auf SQL Server**.

![Empfehlung][4]

Wie unten dargestellt, zeigt das Sicherheitscenter SQL Server, Überwachung und Bedrohung nicht aktiviert. Nachdem Sie die Überwachung aktivieren, können Sie die Erkennung und e-Mail-Einstellungen um Sicherheitshinweise erhalten konfigurieren. Erkennung warnt Sie bei Entdeckung außergewöhnlicher Datenbankaktivitäten, die potenzielle Sicherheitsrisiken für die Datenbank angeben. Alarme werden im Sicherheitscenter Dashboard angezeigt.
![Überwachung und Bedrohung][5]

Gehen Sie in [Erste Schritte mit SQL Datenbank Erkennung](../sql-database/sql-database-threat-detection-get-started.md) aktivieren und Konfigurieren der Erkennung und Konfigurieren der e-Mails, die Sicherheitshinweise bei außergewöhnlichen Aktivitäten erhalten.

Weitere Informationen zu empfohlenen finden Sie unter [Verwalten von Sicherheitsaspekte](security-center-recommendations.md).

## <a name="monitor-security-health"></a>Sicherheitsstatus überwachen

Nach dem aktivieren [von Sicherheitsrichtlinien](security-center-policies.md) für ein Abonnement Ressourcen analysieren Sicherheitscenter der Sicherheits Ihrer Ressourcen zur Identifizierung möglicher Sicherheitslücken.  Sie können den Sicherheitsstatus Ihrer Ressourcen der Kachel **Sicherheitsstatus Ressource** anzeigen. Wenn Sie **Daten** der **Ressource Sicherheitsstatus** Kachel klicken, öffnet **Datenressourcen** Blade mit SQL Themen Überwachung und transparente Verschlüsselung nicht aktiviert. Es hat auch Vorschläge für den allgemeinen Zustand der Datenbank.
![Ressource-Sicherheitsstatus][6]

Mehr Informationen finden Sie unter [Security Health monitoring](security-center-monitoring.md).

## <a name="manage-and-respond-to-security-alerts"></a>Verwalten und Beantworten von Sicherheitshinweisen

Sicherheitscenter automatisch erfasst, analysiert und Integration von Daten aus [SQL Azure Erkennung](../sql-database/sql-database-threat-detection-get-started.md)sowie andere Azure Ressourcen zu nehmende Gefahren erkennen Fehlmeldungen. Eine Liste der bevorzugten Sicherheitshinweise Security Center Informationen zeigt zu schnell das Problem und die zu Angriff beheben müssen.

Um Alerts anzuzeigen, wählen Sie nebeneinander **von Sicherheitshinweisen** auf der Security Center. Wählen Sie auf dem Blatt **Sicherheitshinweise** eine Warnung erfahren die Ereignisse, die Warnung und was ausgelöst, wenn alle Schritte Sie um einen Angriff zu beseitigen müssen. In diesem Beispiel wählen wir **mögliche SQL Injection**.
![Sicherheitshinweise][7]

Wie unten dargestellt, Security Center enthält weitere Details, die Einblick in was Auslösen der Warnung Zielressource gegebenenfalls IP-Quelladresse und Vorschläge zum beheben.
![Mögliche SQL injection][8]

Weitere finden Sie unter [Verwalten von und reagieren auf Sicherheitshinweise](security-center-managing-and-responding-alerts.md).

## <a name="next-steps"></a>Nächste Schritte

- [Security Center FAQ](security-center-faq.md) , häufig gestellte Fragen zur Verwendung des Dienstes suchen.
- [Sicherheitscenter Planung und Operations Guide](security-center-planning-and-operations-guide.md) - folgen eine Reihe von Schritten und Aufgaben zum Optimieren des Sicherheitscenters basierend auf Sicherheit und Cloud-Verwaltungsmodell der Organisation.
- [Security Center Data Security](security-center-data-security.md) – erfahren Sie, wie das Sicherheitscenter sammelt und verarbeitet Daten über Ihre Azure Ressourcen Konfigurationsinformationen, Metadaten, Ereignisprotokolle, Dateien für das Absturzspeicherabbild und.
- [Behandlung von Sicherheitsvorfällen](security-center-incident.md) - lernen Security alert Funktion im Security Center zur Unterstützung bei der Behandlung von Sicherheitsvorfällen.

<!--Image references-->
[1]: ./media/security-center-sql-database/security-center.png
[2]: ./media/security-center-sql-database/security-center-blade.png
[3]: ./media/security-center-sql-database/security-policy.png
[4]: ./media/security-center-sql-database/recommendation.png
[5]: ./media/security-center-sql-database/turn-on-auditing.png
[6]: ./media/security-center-sql-database/monitor-health.png
[7]: ./media/security-center-sql-database/alert.png
[8]: ./media/security-center-sql-database/sql-injection.png
