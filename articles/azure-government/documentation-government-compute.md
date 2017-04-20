<properties
    pageTitle="Azure Regierung Dokumentation | Microsoft Azure"
    description="Dies bietet einen Vergleich der Features und Hinweise auf die Anwendungsentwicklung für Azure"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="09/29/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-compute"></a>Regierung Azure Compute

##  <a name="virtual-machines"></a>Virtuelle Computer

Ausführliche Informationen über diesen Dienst und dessen Verwendung finden Sie unter [Azure VMs Größen](../virtual-machines/virtual-machines-windows-sizes.md).

### <a name="variations"></a>Variationen

Die folgenden VM-SKUs sind allgemein verfügbar (GA) in Azure Government:

VM-SKU|US Gov VA|US Gov IA|Notizen
---|---|---|---
EIN|GA|GA|Keine
Dv1|GA|-|Keine
DSv1|GA|-|Keine
Dv2|GA|GA|15 kommt bald
F|GA|GA|Keine
G|Geplant|-|Keine

###  <a name="data-considerations"></a>Aspekte der Daten

Die folgenden Informationen identifizieren Azure Government Grenze für Azure Virtual Machines:

| Reguliert/gesteuert Daten erlaubt | Reguliert/gesteuert Daten nicht zulässig |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Daten eingegeben, gespeichert und innerhalb eine VM Exportkontrolle Daten enthalten. Binärdateien in Azure virtuellen Computer ausgeführt. Statische Authentifikatoren Smartcard-PINs auf Azure Plattformkomponenten wie Kennwörter. Private Schlüssel von Zertifikaten zur Verwaltung von Azure Plattformkomponenten. SQL-Verbindungszeichenfolgen.  Andere Sicherheit Informationen/Geheimnisse, wie Zertifikate, Verschlüsselungsschlüssel Hauptschlüssel und Speicherschlüssel in Azure Services gespeichert.  | Metadaten darf nicht Exportkontrolle Daten enthalten. Zu diesen Metadaten gehören alle Konfigurationsdaten beim Erstellen und Verwalten von Azure Virtual Machine eingegeben.  Regulierte/gesteuert Daten nicht in die folgenden Felder eingeben: Mieter Rollennamen, Ressourcengruppen, Bereitstellung, Ressourcennamen, Resource-Tags  

## <a name="next-steps"></a>Nächste Schritte

Zusätzliche Informationen und Updates Abonnieren der <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Regierung Blog.</a>
