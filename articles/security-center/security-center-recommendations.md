<properties
   pageTitle="Sicherheitsaspekte in Azure Security Center verwalten | Microsoft Azure"
   description="Dieses Dokument führt Sie durch die Empfehlung in Azure Security Center helfen Sie Ihre Azure Ressourcen schützen und unter Einhaltung von Sicherheitsrichtlinien."
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
   ms.date="09/25/2016"
   ms.author="terrylan"/>

# <a name="managing-security-recommendations-in-azure-security-center"></a>Verwalten von Sicherheitsaspekte in Azure Security Center

Dieses Dokument führt Sie durch Verwendung von Recommendations in Azure Security Center zum Azure Ressourcen schützen.

> [AZURE.NOTE] Dieses Dokument stellt den Dienst mithilfe einer beispielbereitstellung.  Dies ist nicht Schritt.

## <a name="what-are-security-recommendations"></a>Was sind Sicherheitsaspekte?
Sicherheitscenter analysiert regelmäßig den Sicherheitsstatus Ihrer Azure-Ressourcen. Beim Sicherheitscenter Sicherheitslücken identifiziert, erstellt Recommendations. Empfehlung führen Sie durch den Prozess des Konfigurierens benötigten Steuerelemente.

## <a name="implementing-security-recommendations"></a>Implementieren der Sicherheitsaspekte

### <a name="set-recommendations"></a>Set-Empfehlung

[Einrichten von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md)lernen Sie:

- Konfigurieren von Sicherheitsrichtlinien
- Aktivieren Sie die Datensammlung.
- Wählen Sie die Vorschläge im Rahmen der Sicherheitsrichtlinien.

Aktuelle Richtlinie Recommendations Center Systemupdates, geplante Regeln, Antimalware-Programme [netzwerksicherheitsgruppen](../virtual-network/virtual-networks-nsg.md) Subnetze und Netzwerkschnittstellen, SQL Datenbank-auditing SQL Datenbank transparente Verschlüsselung und Web Application Firewalls.  [Festlegen von Sicherheitsrichtlinien](security-center-policies.md) enthält eine Beschreibung der einzelnen Empfehlung.

### <a name="monitor-recommendations"></a>Monitor-Empfehlung
Nach dem Festlegen einer Sicherheitsrichtlinie, analysiert Sicherheitscenter den Sicherheitsstatus Ihrer Ressourcen zur Identifizierung möglicher Sicherheitslücken. **Empfohlene** Kachel auf der **Security Center** können Sie die Gesamtzahl der Empfehlung vom Sicherheitscenter erkannt feststellen.

![Empfohlene Kachel][1]

Jede Empfehlung angezeigt:

1. Klicken Sie auf den **empfohlenen nebeneinander** auf der **Security Center** . **Empfohlene** Blatt wird geöffnet.

Die Empfehlung werden im Tabellenformat angezeigt, wobei jede Zeile eine Empfehlung darstellt. Die Spalten dieser Tabelle sind:

- **Beschreibung**: erläutert die Empfehlung und was um zu adressieren.
- **Ressource**: Listet die Ressourcen diese Empfehlung gilt.
- **Status**: Beschreibt den aktuellen Status der Empfehlung:
    - **Öffnen**: Empfehlung noch noch nicht behandelt wurde.
    - **In Bearbeitung**: Empfehlung wird derzeit auf die Ressourcen angewendet und ist keine Aktion Ihrerseits erforderlich.
    - **Gelöst**: Empfehlung wurde bereits abgeschlossen (in diesem Fall die Zeile grau dargestellt).
- **Schweregrad**: Beschreibt den Schweregrad dieser Empfehlung:
    - **Hoch**: eine Sicherheitslücke vorhanden ist (z. B. eine Anwendung, ein virtueller Computer oder ein Netzwerk-Sicherheitsgruppe) mit einer sinnvollen und Aufmerksamkeit.
    - **Mittel**: eine Schwachstelle und kritisch oder zusätzliche Schritte beheben oder zum Abschließen eines Prozesses erforderlich sind.
    - **Niedrig**: eine Schwachstelle werden jedoch keine unmittelbare Aufmerksamkeit erfordert. (Standardmäßig niedrige empfohlen werden nicht angezeigt, aber Filtern auf niedrige Empfehlung angezeigt werden soll.)

Verwenden Sie die Tabelle als Referenz erfahren Sie die verfügbaren Informationen und welche jeweils bei Anwendung.

> [AZURE.NOTE] Sie möchten die [Classic und Ressourcenmanager Bereitstellungsmodelle](../azure-classic-rm.md) für Azure-Ressourcen zu verstehen.

|Empfehlung|Beschreibung|
|-----|-----|
|[Datensammlung für Abonnements aktivieren](security-center-enable-data-collection.md)|Empfiehlt, dass Sie Datensammlung in der Sicherheitsrichtlinie für Ihre Abonnements sowie alle virtuellen Maschinen (VMs) in Ihre Abonnements aktivieren.|
|[Betriebssystem-Sicherheitslücken beseitigen](security-center-remediate-os-vulnerabilities.md)|Ausrichten die OS-Konfigurationen mit Regeln empfohlene Konfiguration empfiehlt z. B. erlauben nicht Kennwörter gespeichert werden.|
|[System-Updates anwenden](security-center-apply-system-updates.md)|Empfiehlt, dass Sie fehlende Sicherheit und wichtige Updates auf VMs bereitstellen.|
|[Neustart nach System-updates](security-center-apply-system-updates.md#reboot-after-system-updates)|Empfiehlt, Neustart virtueller Computer zum Abschluss der System-Updates anwenden.|
|[Fügen Sie Web-Anwendung](security-center-add-web-application-firewall.md)|Empfiehlt die Bereitstellung einer Web Application Firewall (AAF) für Webendpunkte. Diese Programme Bereitstellung Ihrer vorhandenen WAF hinzufügen, um mehrere ASP.NET-Webanwendungen im Sicherheitscenter zu schützen. WAF Geräte (erstellt mit dem Ressourcen-Manager-Bereitstellungsmodell) müssen mit einem separaten virtuellen Netzwerk bereitgestellt werden. WAF Geräte (erstellt mit dem klassischen Bereitstellungsmodell) sind mit einer Netzwerk-Sicherheitsgruppe auf. Diese Unterstützung wird in Zukunft individuell Bereitstellung einer WAF Einheit (classic) erweitert. Sicherheitscenter wird empfehlen WAF um Verteidigung gegen Angriffe auf einer Anwendung auf virtuellen Computern und auf App Service-Umgebung (ASE) bereitstellen. Erfahren Sie mehr über ASE finden Sie in der [Dokumentation zum Dienst App](../app-service/app-service-app-service-environments-readme.md). |
|[Anwendungsschutz abschließen](security-center-add-web-application-firewall.md#finalize-application-protection)|Zum Abschließen der Konfigurations einer WAF muss Datenverkehr an die Appliance WAF umgeleitet. Diese Empfehlung vervollständigt die notwendigen Setup ändert.|
|[Hinzufügen einer Firewalls der nächsten Generation](security-center-add-next-generation-firewall.md)|Empfiehlt, Next Generation Firewall (NGFW) von einem Microsoft-Partner hinzuzufügen, um Ihre Sicherheitsmaßnahmen zu erhöhen.|
|[Datenverkehr über NGFW nur](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only)|Empfiehlt Network Security (NSG) Regeln konfigurieren, die eingehenden Datenverkehr für die VM durch Ihre NGFW erzwingen.|
|[Endpoint Protection installieren](security-center-install-endpoint-protection.md)|Empfiehlt Antimalware Programme VMs (nur Windows-VMs) bereitstellen.|
|[Endpoint Protection Health Alarme](security-center-resolve-endpoint-protection-health-alerts.md)|Empfiehlt, dass Sie Endpoint Protection Fehler beheben.|
|[Aktivieren Sie Netzwerk-Sicherheitsgruppen auf Subnetze oder virtuelle Computer](security-center-enable-network-security-groups.md)|Empfiehlt, Subnetzen oder VMs NSGs aktivieren.|
|[Einschränken des Zugriffs über Endpunkt mit Internetzugriff](security-center-restrict-access-through-internet-facing-endpoints.md)|Empfiehlt, dass eingehender Verkehr für NSGs konfigurieren.|
|[Aktivieren der Überwachung von SQL server](security-center-enable-auditing-on-sql-servers.md)|Empfiehlt, dass Sie auditing für SQL Azure-Server (SQL Azure Service nicht nur SQL auf Ihre virtuellen Computer enthalten) aktivieren.|
|[Datenbank SQL-Überwachung](security-center-enable-auditing-on-sql-databases.md)|Empfiehlt, dass Sie Überwachung für SQL Azure-Datenbanken (SQL Azure Service nicht nur SQL auf Ihre virtuellen Computer enthalten) aktivieren.|
|[Transparente Verschlüsselung auf SQL-Datenbanken aktivieren](security-center-enable-transparent-data-encryption.md)|Empfiehlt, dass Verschlüsselung für SQL-Datenbanken (nur SQL Azure-Dienst) zu aktivieren.|
|[VM-Agent aktivieren](security-center-enable-vm-agent.md)|Können Sie die VMs benötigen den VM-Agent. VM-Agent muss auf virtuellen Computern installiert werden, um Patchsuche, geplante Scans und Antimalware-Programme bereitstellen. VM-Agent ist für virtuelle Computer standardmäßig, die aus dem Markt Azure bereitgestellt werden. Artikel [VM und Extensions – Teil2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) enthält Informationen wie den VM-Agent installieren.|
| [Disk Encryption anwenden](security-center-apply-disk-encryption.md) |Empfiehlt die VM Datenträger Verschlüsselung Azure Datenträger (Windows und Linux VMs) verschlüsseln. Verschlüsselung wird für das Betriebssystem und die Daten-Volumes auf Ihrem virtuellen Computer empfohlen.|
|[Machen Sie Sicherheit](security-center-provide-security-contact-details.md) | Empfiehlt, dass Sie Sicherheit Kontaktinformationen aller Abonnements. Kontaktinformationen sind eine e-Mail-Adresse und Telefon Nummer. Die Daten werden an Sie unser Sicherheitsteam fest, dass Ihre Ressourcen beeinträchtigt werden. |
| [Aktualisieren Betriebssystem](security-center-update-os-version.md) | Empfiehlt, Version des Betriebssystems (OS) für den Cloud-Dienst auf die neueste verfügbare Version für Ihr Betriebssystem-Familie zu aktualisieren.  Über Cloud-Dienste finden Sie unter [Übersicht über Cloud-Services](../cloud-services/cloud-services-choose-me.md). |
| [Bewertung der Sicherheitslücken nicht installiert](security-center-vulnerability-assessment-recommendations.md) | Empfiehlt eine Schwachstelle Lösung auf Ihrem virtuellen Computer installieren. |
| [Beheben von Sicherheitslücken](security-center-vulnerability-assessment-recommendations.md#review-recommendation) | Können Sie auf System- und Sicherheitslücken erkannt durch die Sicherheitslücke Lösung auf die VM installiert. |

Filtern und Vorschläge schließen.

1. Klicken Sie auf die **empfohlenen** **Filter** auf. **Filter** -Blade wird geöffnet, und Sie auswählen, den Schweregrad und Status zu sehen.

    ![Filter-Empfehlung][2]

2. Wenn Sie feststellen, dass eine Empfehlung nicht anwendbar ist, können die Empfehlung schließen und aus der Ansicht filtern. Es gibt zwei Arten eine Empfehlung zu schließen. Eine Möglichkeit ist, klicken Sie mit der rechten Maustaste ein Element und wählen Sie **Ausblenden**. Das andere ist ein Element mit dem Mauszeiger auf den drei Punkten rechts und wählen Sie **Schließen**. Sie können ausgeblendete Recommendations anzeigen, durch Klicken auf **Filter**und dann **verworfen**.

    ![Empfehlung schließen][3]

### <a name="apply-recommendations"></a>Empfohlene anwenden
Nach dem Überprüfen entscheiden aller Vorschläge, welche Sie zuerst anwenden sollten. Wir empfehlen die Verwendung der Schweregrad als wichtigste Parameter welche auswerten zuerst angewendet werden soll.

In der Tabelle oben empfohlenen Empfehlung wählen und so eine Empfehlung zum erläutern.

## <a name="see-also"></a>Siehe auch
In diesem Dokument wurden Sicherheitsaspekte im Sicherheitscenter vorgestellt. Erfahren Sie mehr über das Sicherheitscenter finden Sie hier:

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md) – Informationen zum Konfigurieren von Sicherheitsrichtlinien für Ihren Azure-Abonnements und Ressourcengruppen.
- [Sicherheit von Systemüberwachungsereignissen in Azure Security Center](security-center-monitoring.md) – erfahren Sie, wie Ihre Azure Ressourcen überwachen.
- [Verwalten von und reagieren auf Sicherheitshinweise in Azure Security Center](security-center-managing-and-responding-alerts.md) – Informationen zum Verwalten und Sicherheitshinweise auf.
- [Überwachung Partner mit Azure Security Center](security-center-partner-solutions.md) – So überwachen Sie den Status Ihrer Lösungen Partner.
- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md) – häufig gestellte Fragen zur Verwendung des Dienstes suchen.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – finden Sie Artikel über Azure Sicherheit und Compliance.

<!--Image references-->
[1]: ./media/security-center-recommendations/recommendations-tile.png
[2]: ./media/security-center-recommendations/filter-recommendations.png
[3]: ./media/security-center-recommendations/dismiss-recommendations.png
