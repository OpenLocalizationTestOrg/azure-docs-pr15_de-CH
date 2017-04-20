<properties
   pageTitle="Azure Security Center häufig gestellte Fragen (FAQ) | Microsoft Azure"
   description="Dieses Dokument beantwortet Fragen zur Azure Security Center."
   services="security-center"
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
   ms.date="10/27/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-frequently-asked-questions-faq"></a>Häufig gestellte Fragen (FAQ) Azure Security Center

Diese FAQ beantwortet Fragen zur Azure Security Center, ein Dienst, zu verhindern, erkennen und auf Gefahren mehr Transparenz und Kontrolle über die Sicherheit Ihrer Microsoft Azure-Ressourcen.

## <a name="general-questions"></a>Allgemeine Fragen

### <a name="what-is-azure-security-center"></a>Was ist Azure Security Center?
Azure Security Center können Sie verhindern, erkennen und reagieren auf Gefahren mit mehr Einblick und Kontrolle über die Sicherheit von Azure-Ressourcen. Bietet integrierte Überwachung und Policy-Management über Abonnements, hilft bei der Ermittlung Gefahren, die andernfalls vielleicht übersehen würden und arbeitet mit einem ausgedehnten System von Sicherheitsfunktionen.

### <a name="how-do-i-get-azure-security-center"></a>Wie wird die Azure Security Center?
Azure Security Center mit Microsoft Azure-Abonnement aktiviert und [Azure-Portal](https://azure.microsoft.com/features/azure-portal/)zugreifen. ([Das Portal anmelden](https://portal.azure.com), wählen Sie **Durchsuchen**und einen Bildlauf zum **Security Center**).  

## <a name="billing"></a>Abrechnung

### <a name="how-does-billing-work-for-azure-security-center"></a>Wie funktioniert die Abrechnung für Azure Security Center?
Sicherheitscenter wird in zwei Stufen angeboten: frei und Standard.

Freie Ebene können Sie Sicherheitsrichtlinien und Sicherheitshinweise Vorfälle und Vorschläge, die Sie schrittweise durch den Prozess des Konfigurierens benötigten Steuerelemente angezeigt. Mit der freien können Sie auch den Sicherheitsstatus Ihrer Azure-Ressourcen und Partner Solutions integriert Azure-Abonnement überwachen.

Die Standard-Stufe bietet kostenlos plus fortschrittliche Erkennung: Intelligenz, Verhaltensanalyse Absturzanalyse und Erkennen von Netzwerkanomalien Bedrohung. 90-Tage-Testversion der Standard-Stufe steht. Wählen Sie Aktualisierung Preise Tier in der [Sicherheitsrichtlinie](security-center-policies.md#setting-security-policies-for-subscriptions). Finden Sie unter [Security Center Preise](security-center-pricing.md) informieren.

## <a name="data-collection"></a>Datensammlung

Sicherheitscenter sammelt Daten von der virtuellen Maschinen um ihren Sicherheitsstatus bewerten, Sicherheitsaspekte, und Sie auf Gefahren hinweisen. Beim Aufrufen von Security Center ist die Datensammlung auf allen virtuellen Computern in Ihrem Abonnement aktiviert. Datensammlung wird empfohlen, jedoch kann entscheiden Sie [deaktivieren die Datensammlung](#how-do-i-disable-data-collection) in der Security Center.

### <a name="how-do-i-disable-data-collection"></a>Wie deaktivieren Sie Daten

Sie können **Daten** für ein Abonnement in der Sicherheitsrichtlinie jederzeit deaktivieren. ([Azure-Portal anmelden](https://portal.azure.com), wählen Sie **Durchsuchen**, wählen Sie **Security Center**und wählen Sie **Richtlinie**.)  Beim Auswählen eines Abonnements ein neues Blatt wird geöffnet und bietet die Möglichkeit, **Daten** zu deaktivieren. Die Option **Agenten löschen** im oberen Menüband aus vorhandenen virtuellen Computern Agents entfernen.

> [AZURE.NOTE] Sicherheitsrichtlinien auf der Azure-Abonnement und Gruppenebene Ressource festgelegt werden, aber wählen Sie ein Abonnement für die Datensammlung deaktivieren.

### <a name="how-do-i-enable-data-collection"></a>Aktivieren die Datensammlung
Sie können Daten für Ihre Azure-Abonnements in der Sicherheitsrichtlinie. Um Daten [zum Azure-Portal anmelden](https://portal.azure.com), aktivieren wählen Sie **Durchsuchen**, wählen Sie **Security Center**und **Richtlinien**. **Datensammlung** **auf** und konfigurieren Speicherkonten an Daten gesammelt werden (siehe Frage "[, in dem die Daten gespeichert?](#where-is-my-data-stored)"). Bei aktiviertem **Daten** werden automatisch Security Configuration und Event-Informationen von allen unterstützten virtuellen Computern im Abonnement sammelt.

> [AZURE.NOTE] Konfiguration der Datensammlung tritt nur auf Abonnementebene jedoch Sicherheitsrichtlinien auf der Azure-Abonnement und Gruppenebene Ressource festgelegt werden.

### <a name="what-happens-when-data-collection-is-enabled"></a>Was geschieht, wenn die Datensammlung aktiviert ist?
Datensammlung wird über Azure Monitoring Agent und Erweiterung Azure Security Überwachung aktiviert. Die Erweiterung Azure Security Überwachung sucht nach verschiedenen relevanten Sicherheitskonfiguration und [Event Tracing for Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) Spuren sendet. Außerdem erstellt das Betriebssystem Ereignisprotokolleinträge.  Azure Monitoring Agent liest Ereignisprotokolleinträge und ETW verfolgt und kopiert diese in das Speicherkonto für die Analyse.  Dies ist das Speicherkonto in der Sicherheitsrichtlinie konfiguriert. Weitere Informationen über das Speicherkonto finden Sie unter "[, in dem die Daten gespeichert?](#where-is-my-data-stored)"

### <a name="does-the-monitoring-agent-or-security-monitoring-extension-impact-the-performance-of-my-servers"></a>Beeinträchtigt die Erweiterung Monitoring Agent oder die Sicherheit überwachen die Leistung meiner Server?
Der Agent und die Erweiterung einen Nominalbetrag von Systemressourcen verbraucht und müssen geringfügig auf die Leistung.

### <a name="where-is-my-data-stored"></a>Wo sind meine Daten gespeichert?
Für jede Region, in der virtuelle Computer ausgeführt haben, wählen Sie das Speicherkonto Daten aus diesen virtuellen Computern Speicherort. Dadurch für Daten im selben geografischen Gebiet für Datenschutz und Daten Hoheit. Sie wählen das Speicherkonto für ein Abonnement in der Sicherheitsrichtlinie. ([Azure-Portal anmelden](https://portal.azure.com), wählen Sie **Durchsuchen**, wählen Sie **Security Center**und wählen Sie **Richtlinie**.) Wenn Sie ein Abonnement klicken, öffnet ein neues Blatt. Wählen Sie **Speicherkonten wählen Sie** einen Bereich auswählen.

> [AZURE.NOTE] Wählen einen Bereich für das Speicherkonto nur auf Abonnementebene tritt jedoch Sicherheitsrichtlinien auf der Azure-Abonnement und Gruppenebene Ressource festgelegt werden.

Weitere Informationen zu Azure-Speicher und Speicherkonten finden Sie [Dokumentation](https://azure.microsoft.com/documentation/services/storage/) und [zum Azure-Speicherkonten](../storage/storage-create-storage-account.md).

## <a name="using-azure-security-center"></a>Verwenden von Azure Security Center

### <a name="what-is-a-security-policy"></a>Was ist eine Sicherheitsrichtlinie?
Eine Sicherheitsrichtlinie definiert die Steuerelemente für das angegebene Abonnement oder Ressourcengruppe Ressourcen empfohlen. In Azure Security Center definieren Sie Richtlinien für Ihre Azure-Abonnements und Ressourcengruppen Sicherheitserfordernisse des Unternehmens und der Anwendung oder die Daten in jedem Abonnement.

Beispielsweise müssen Ressourcen für Entwicklungs- oder Testzwecken verwendet andere Sicherheitsstandards gelten als für die Produktion. Ebenso können mit geregelten Daten personenbezogene Informationen (Personally Identifiable Information) eine höhere Sicherheit benötigen. Die Sicherheitsrichtlinien in Azure Security Center aktiviert fahren Sicherheitsaspekte und Überwachung. Weitere Informationen zu Sicherheitsrichtlinien finden Sie unter [Sicherheit Systemüberwachungsereignissen in Azure Security Center](security-center-monitoring.md).

> [AZURE.NOTE] Bei einem Konflikt zwischen Abonnement Ebene und Ressource auf Vorrang Ressourcen auf Gruppenrichtlinien.

### <a name="who-can-modify-a-security-policy"></a>Wer eine Sicherheitsrichtlinie?
Sicherheitsrichtlinien sind für jedes Abonnement oder Ressourcengruppe konfiguriert. Um eine Sicherheitsrichtlinie auf Abonnement oder Gruppenebene Ressource zu ändern, müssen Sie Eigentümer oder Beiträge für dieses Abonnement sein.

So konfigurieren Sie eine Sicherheitsrichtlinie finden Sie unter [Einrichten von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md).

### <a name="what-is-a-security-recommendation"></a>Was ist eine sicherheitsempfehlung?
Azure Security Center analysiert den Sicherheitsstatus Ihrer Azure-Ressourcen. Wenn potenzielle Sicherheitsrisiken identifiziert werden, werden Vorschläge erstellt. Empfehlung führen Sie durch den Prozess des Konfigurierens erforderlichen Steuerelements. Beispiele sind:

- Bereitstellung von Malware zu identifizieren und entfernen bösartigen software
- Konfigurieren von [Netzwerk-Sicherheitsgruppen](../virtual-network/virtual-networks-nsg.md) und Regeln für die Kontrolle des Datenverkehrs zu virtuellen Maschinen
- Bereitstellung von Web Application Firewall zur Verteidigung gegen Angriffe auf einer Anwendung
- Bereitstellen von fehlenden Systemupdates
- OS-Konfigurationen, die nicht die empfohlene Baselines Adressierung

Hier sind nur Vorschläge, die Sicherheitsrichtlinien aktiviert dargestellt.

### <a name="how-can-i-see-the-current-security-state-of-my-azure-resources"></a>Wie kann ich den aktuellen Sicherheitsstatus meiner Azure Ressourcen sehen?
Eine Kachel **Ressourcen Zustand** auf das **Sicherheitscenter** zeigt den Sicherheitsstatus Ihrer Umgebung virtuelle Computer, Webapplikationen und andere Ressourcen aufgeschlüsselt. Jede Ressource weist einen Indikator mit potenzielle Sicherheitslücken identifiziert wurden. Auf Ressourcen Health Kachel zeigt Ihre Ressourcen und identifiziert, wo Aufmerksamkeit erforderlich ist oder Probleme existieren.

### <a name="what-triggers-a-security-alert"></a>Was löst eine Warnung?
Azure Security Center automatisch erfasst, analysiert und Sicherungen Protokolldaten Azure Ressourcen, Netzwerk und Partner Solutions Antimalware und Firewalls. Wenn Viren gefunden werden, wird eine Warnung erstellt. Beispiele für Erkennung:

- Betroffenen virtuellen Maschinen mit bekannten bösartigen IP-Adressen
- Erweiterte Malware entdeckt mit Windows-Fehlerberichterstattung
- Brute-Force-Angriffe auf virtuellen Maschinen
- Sicherheitshinweise von integrierten Partner Security Solutions Web Application Firewalls oder Anti-Malware

### <a name="whats-the-difference-between-threats-detected-and-alerted-on-by-microsoft-security-response-center-versus-azure-security-center"></a>Was ist der Unterschied zwischen Gefahren erkannt und eine vom Microsoft Security Response Center im Vergleich zu Azure Security Center?
Microsoft Security Response Center (MSRC) Auswahl von Azure-Netzwerk und Infrastruktur Überwachung durchführt und Threat Intelligence und Missbrauch Beschwerden von Dritten erhält. MSRC bewusst, dass Kundendaten durch eine rechtswidrige oder nicht autorisierte zugegriffen wurde oder die Nutzung von Azure Begriffe für zulässige nicht erfüllt ein Vorfall-Manager benachrichtigt den Kunden wird. Benachrichtigung erfolgt in der Regel per e-Mail Security Kontakte in Azure Security Center oder Azure Abonnementbesitzer angegeben, wenn sicherheitskontakt angegeben ist.

Das Sicherheitscenter ist ein Azure Dienst, die kontinuierlich überwacht die Umgebung des Kunden Azure Analytics automatisch eine Vielzahl potenziell bösartige Aktivitäten gilt. Diese Erkennung werden als Sicherheitshinweise im Sicherheitscenter Dashboard dargestellt.

### <a name="how-are-permissions-handled-in-azure-security-center"></a>Wie werden Berechtigungen in Azure Security Center behandelt?
Azure Security Center unterstützt rollenbasierten Zugriff. Weitere Informationen über rollenbasierte Zugriffskontrolle (RBAC) in Azure finden Sie unter [Azure Active Directory Role-based Access Control](../active-directory/role-based-access-control-configure.md).

Ein Benutzer öffnet Sicherheitscenter nur Vorschläge als Ressourcen verknüpft sind, hat der Benutzer Zugriff auf Alerts angezeigt. Dies bedeutet, dass Benutzer nur Elemente im Zusammenhang mit Ressourcen sehen, in denen der Benutzer zugeordnet ist die Rolle des Besitzers, Teilnehmer- oder Lesezugriff Abonnement oder Ressourcengruppe, zu der eine Ressource gehört.

Bei Bedarf:

- **Bearbeiten eine Sicherheitsrichtlinie**müssen Sie Besitzer oder Mitwirkender des Abonnements sein.
- **Anwenden einer Empfehlung**muss Besitzer oder Mitwirkender des Abonnements sein.
- **Einblicke in den Sicherheitsstatus aller Abonnements verfügen**, müssen Sie einen Besitzer, Teilnehmer- oder Lesezugriff (IT Admin, Sicherheitsteam) für jedes Abonnement sein.
- **Einblicke in den Sicherheitsstatus Ihrer Ressourcen verfügen**, müssen Sie eine Ressourcengruppe Besitzer, Teilnehmer- oder Lesezugriff (DevOps) sein.

### <a name="which-azure-resources-are-monitored-by-azure-security-center"></a>Azure Ressourcen von Azure Security Center überwacht werden?
Azure Security Center überwacht Azure Folgendes:

- Virtuelle Maschinen (VMs) (einschließlich [Cloud-Dienste](../cloud-services/cloud-services-choose-me.md))
- Azure Virtual Networks
- Azure SQL-Dienst
- Partner Solutions Azure Abonnements wie eine Web Application Firewall VMs und [App Service-Umgebung](../app-service/app-service-app-service-environments-readme.md) integriert

## <a name="virtual-machines"></a>Virtuelle Computer

### <a name="what-types-of-virtual-machines-will-be-supported"></a>Welche Arten von virtuellen Maschinen werden unterstützt?
Security Health monitoring und Vorschläge stehen für virtuelle Maschinen (VMs) mit der [klassischen und Ressourcenmanager Bereitstellungsmodelle](../azure-classic-rm.md)erstellt.

Windows-VMs unterstützt:

- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2

Linux VMs unterstützt:

- Ubuntu-Versionen 12.04, 14.04, 16.04
- Debian Versionen 7, 8
- CentOS Version 6. \*, 7.*
- Red Hat Enterprise Linux (RHEL) Version 6. \*, 7.*
- SUSE Linux Enterprise Server (SLES) Version 11. \*, 12.*
- Oracle Linux-Versionen 6. \*, 7.*

Virtuelle Computer in einem Clouddienst ausgeführt werden ebenfalls unterstützt. Nur Cloud services Web- und Workerrollen Rollen, die als Produktionssysteme Steckplätze überwacht. Über Cloud-Dienst finden Sie unter [Übersicht über Cloud-Services](../cloud-services/cloud-services-choose-me.md).

### <a name="why-doesnt-azure-security-center-recognize-the-antimalware-solution-running-on-my-azure-vm"></a>Warum erkennen nicht Azure Security Center auf meinem Azure VM Antimalware-Lösung?

Azure Security Center hat nur Transparenz Antimalware Azure Extensions installiert. Sicherheitscenter ist z. B. nicht Antimalware erkennen, die auf ein Bild eingegebene oder Antimalware auf die virtuellen Computer verwenden Ihre eigenen Prozesse (beispielsweise Konfiguration) installiert wurde.

### <a name="why-do-i-get-the-message-missing-scan-data-for-my-vm"></a>Warum erhalte die Meldung "Scan Daten fehlt" ich für meine VM?

Es kann einige Zeit dauern (in der Regel weniger als eine Stunde) Scan-Daten aufzufüllen, nachdem Daten in Azure Security Center aktiviert ist. Scans füllt nicht für virtuelle Computer beendet.

### <a name="why-do-i-get-the-message-vm-agent-is-missing"></a>Warum erhalte ich die Meldung? "VM-Agent fehlt?"

VM-Agent muss auf virtuellen Computern installiert werden, um Daten ermöglichen. VM-Agent ist für virtuelle Computer standardmäßig, die aus dem Markt Azure bereitgestellt werden. Informationen zum Installieren der VM-Agent auf andere VMs finden Sie im Blogbeitrag [VM und Extensions](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/).
