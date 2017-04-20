<properties
   pageTitle="Schützen Ihre Anwendung in Azure Security Center | Microsoft Azure"
   description="Dieses Dokument Adressen Recommendations in Azure Security Center, mit denen Sie Ihre Azure Applications schützen und gemäß den Sicherheitsrichtlinien bleiben."
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

# <a name="protecting-your-applications-in-azure-security-center"></a>Schützen Ihre Anwendung in Azure Security Center

Azure Security Center analysiert den Sicherheitsstatus Ihrer Azure-Ressourcen. Beim Sicherheitscenter Sicherheitslücken identifiziert, erstellt Vorschläge, die Sie schrittweise durch den Prozess des Konfigurierens benötigten Steuerelemente.  Vorschläge gelten für Azure Ressourcentypen: virtuelle Maschinen (VMs) Netzwerken, SQL und Applikationen.

Dieser Artikel behandelt die empfohlenen gelten.  Anwendung Recommendations Mittelpunkt Bereitstellung einer Web Application Firewall.  Verwenden Sie die Tabelle als Referenz erfahren Sie Vorschläge der Anwendung und welche jeweils bei Anwendung.

## <a name="available-application-recommendations"></a>Anwendung empfohlen

|Empfehlung|Beschreibung|
|-----|-----|
|[Fügen Sie Web-Anwendung](security-center-add-web-application-firewall.md)|Empfiehlt die Bereitstellung einer Web Application Firewall (AAF) für Webendpunkte. Diese Programme Bereitstellung Ihrer vorhandenen WAF hinzufügen, um mehrere ASP.NET-Webanwendungen im Sicherheitscenter zu schützen. WAF Geräte (erstellt mit dem Ressourcen-Manager-Bereitstellungsmodell) müssen mit einem separaten virtuellen Netzwerk bereitgestellt werden. WAF Geräte (erstellt mit dem klassischen Bereitstellungsmodell) sind mit einer Netzwerk-Sicherheitsgruppe auf. Diese Unterstützung wird in Zukunft individuell Bereitstellung einer WAF Einheit (classic) erweitert.|
|[Anwendungsschutz abschließen](security-center-add-web-application-firewall.md#finalize-application-protection)|Zum Abschließen der Konfigurations einer WAF muss Datenverkehr an die Appliance WAF umgeleitet. Diese Empfehlung vervollständigt die notwendigen Setup ändert.|

## <a name="see-also"></a>Siehe auch

Erfahren Sie mehr über Vorschläge, die für andere Typen von Azure gelten finden Sie hier:

- [Schützen Ihre virtuellen Computer in Azure Security Center](security-center-virtual-machine-recommendations.md)
- [Schützen Sie Ihr Netzwerk in Azure Security Center](security-center-network-recommendations.md)
- [Schützen Ihren SQL Azure-Dienst in Azure Security Center](security-center-sql-service-recommendations.md)

Erfahren Sie mehr über das Sicherheitscenter finden Sie hier:

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md) – Informationen zum Konfigurieren von Sicherheitsrichtlinien für Ihren Azure-Abonnements und Ressourcengruppen.
- [Verwalten von und reagieren auf Sicherheitshinweise in Azure Security Center](security-center-managing-and-responding-alerts.md) - Informationen verwalten und Sicherheitshinweise auf.
- [Azure Security Center FAQ](security-center-faq.md) - finden Sie häufig gestellte Fragen zur Verwendung des Dienstes.
