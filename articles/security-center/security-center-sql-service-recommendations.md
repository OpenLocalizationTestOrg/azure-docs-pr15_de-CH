<properties
   pageTitle="SQL Azure-Dienst in Azure Security Center schützen | Microsoft Azure"
   description="Dieses Dokument Adressen Recommendations in Azure Security Center, mit denen Sie SQL Azure Service schützen und unter Einhaltung von Sicherheitsrichtlinien bleiben."
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

# <a name="protecting-azure-sql-service-in-azure-security-center"></a>SQL Azure-Dienst in Azure Security Center schützen

Azure Security Center analysiert den Sicherheitsstatus Ihrer Azure-Ressourcen. Beim Sicherheitscenter Sicherheitslücken identifiziert, erstellt Vorschläge, die Sie schrittweise durch den Prozess des Konfigurierens benötigten Steuerelemente.  Vorschläge gelten für Azure Ressourcentypen: virtuelle Maschinen (VMs) Netzwerken, SQL und Applikationen.

Dieser Artikel behandelt die empfohlenen SQL Azure Service gelten.  Azure SQL Service Recommendations Mittelpunkt Aktivieren der Überwachung für SQL Azure-Server und Datenbanken und ermöglicht Verschlüsselung für SQL-Datenbanken.  Verwenden Sie die Tabelle als Referenz erfahren Sie verfügbaren SQL Service Recommendations und was jede bei Anwendung.

## <a name="available-sql-service-recommendations"></a>Verfügbare SQL Service empfohlen

|Empfehlung|Beschreibung|
|-----|-----|
|[Aktivieren der Überwachung von SQL server](security-center-enable-auditing-on-sql-servers.md)|Empfiehlt, dass Sie auditing für SQL Azure-Server (SQL Azure Service nicht nur SQL auf Ihre virtuellen Computer enthalten) aktivieren.|
|[Datenbank SQL-Überwachung](security-center-enable-auditing-on-sql-databases.md)|Empfiehlt, dass Sie Überwachung für SQL Azure-Datenbanken (SQL Azure Service nicht nur SQL auf Ihre virtuellen Computer enthalten) aktivieren.|
|[Transparente Verschlüsselung auf SQL-Datenbanken aktivieren](security-center-enable-transparent-data-encryption.md)|Empfiehlt, dass Verschlüsselung für SQL-Datenbanken (nur SQL Azure-Dienst) zu aktivieren.|

## <a name="see-also"></a>Siehe auch

Erfahren Sie mehr über Vorschläge, die für andere Typen von Azure gelten finden Sie hier:

- [Schützen Ihre virtuellen Computer in Azure Security Center](security-center-virtual-machine-recommendations.md)
- [Schützen Ihre Anwendung in Azure Security Center](security-center-application-recommendations.md)
- [Schützen Sie Ihr Netzwerk in Azure Security Center](security-center-network-recommendations.md)

Erfahren Sie mehr über das Sicherheitscenter finden Sie hier:

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md) – Informationen zum Konfigurieren von Sicherheitsrichtlinien für Ihren Azure-Abonnements und Ressourcengruppen.
- [Verwalten von und reagieren auf Sicherheitshinweise in Azure Security Center](security-center-managing-and-responding-alerts.md) - Informationen verwalten und Sicherheitshinweise auf.
- [Azure Security Center FAQ](security-center-faq.md) - finden Sie häufig gestellte Fragen zur Verwendung des Dienstes.
