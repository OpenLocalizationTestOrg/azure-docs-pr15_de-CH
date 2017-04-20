<properties
   pageTitle="Schützen Sie Ihr Netzwerk in Azure Security Center | Microsoft Azure"
   description="Dieses Dokument Adressen Recommendations in Azure Security Center, mit denen Sie Azure Netzwerkschutz und bleiben unter Einhaltung von Sicherheitsrichtlinien."
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
   ms.date="08/04/2016"
   ms.author="terrylan"/>

# <a name="protecting-your-network-in-azure-security-center"></a>Schützen Sie Ihr Netzwerk in Azure Security Center

Azure Security Center analysiert den Sicherheitsstatus Ihrer Azure-Ressourcen. Beim Sicherheitscenter Sicherheitslücken identifiziert, erstellt Vorschläge, die Sie schrittweise durch den Prozess des Konfigurierens benötigten Steuerelemente.  Vorschläge gelten für Azure Ressourcentypen: virtuelle Maschinen (VMs) Netzwerken, SQL und Applikationen.

Dieser Artikel behandelt die Vorschläge für das Netzwerk.  Netzwerk Recommendations Mittelpunkt next Generation Firewalls, Netzwerk-Sicherheitsgruppen und Konfigurieren von Regeln für eingehenden Datenverkehr.  Verwenden Sie die Tabelle als Referenz erfahren Sie verfügbaren Vorschläge und was jede bei Anwendung.

## <a name="available-network-recommendations"></a>Netzwerk-Empfehlung

|Empfehlung|Beschreibung|
|-----|-----|
|[Hinzufügen einer Firewalls der nächsten Generation](security-center-add-next-generation-firewall.md)|Empfiehlt, Next Generation Firewall (NGFW) von einem Microsoft-Partner hinzuzufügen, um Ihre Sicherheitsmaßnahmen zu erhöhen.|
|[Datenverkehr über NGFW nur](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only)|Empfiehlt Network Security (NSG) Regeln konfigurieren, die eingehenden Datenverkehr für die VM durch Ihre NGFW erzwingen.|
|[Aktivieren Sie Netzwerk-Sicherheitsgruppen auf Subnetze oder virtuelle Computer](security-center-enable-network-security-groups.md)|Empfiehlt, Subnetzen oder VMs NSGs aktivieren.|
|[Einschränken des Zugriffs über Endpunkt mit Internetzugriff](security-center-restrict-access-through-internet-facing-endpoints.md)|Empfiehlt, dass eingehender Verkehr für NSGs konfigurieren.|

## <a name="see-also"></a>Siehe auch

Erfahren Sie mehr über Vorschläge, die für andere Typen von Azure gelten finden Sie hier:

- [Schützen Ihre virtuellen Computer in Azure Security Center](security-center-virtual-machine-recommendations.md)
- [Schützen Ihre Anwendung in Azure Security Center](security-center-application-recommendations.md)
- [Schützen Ihren SQL Azure-Dienst in Azure Security Center](security-center-sql-service-recommendations.md)

Erfahren Sie mehr über das Sicherheitscenter finden Sie hier:

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md) – Informationen zum Konfigurieren von Sicherheitsrichtlinien für Ihren Azure-Abonnements und Ressourcengruppen.
- [Verwalten von und reagieren auf Sicherheitshinweise in Azure Security Center](security-center-managing-and-responding-alerts.md) - Informationen verwalten und Sicherheitshinweise auf.
- [Azure Security Center FAQ](security-center-faq.md) - finden Sie häufig gestellte Fragen zur Verwendung des Dienstes.
