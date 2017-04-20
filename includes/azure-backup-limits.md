 (backup Depots<properties
   Titel = "Azure Backup limits table"
   Beschreibung = "beschreibt die Systemgrenzen für Azure Backup."
   services="backup"
   documentationCenter="NA"
   authors="Jim-Parker"
   manager="jwhit"
   editor="" />
<tags
   ms.service="backup"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="10/05/2015"
   ms.author="trinadhk";"jimpark"; "aashishr" />


Die folgenden Grenzwerte gelten für Azure Backup.

| Limit-Bezeichner | Standardlimit |
|---|---|
|Anzahl für jedes Depot Registrierung Server/Computer|50 für Windows Server/Client/SCDPM <br/> 200 für IaaS VMs|
|Größe einer Datenquelle Daten in Azure Vault-Speicher|54400 GB max<sup>1</sup>|
|Anzahl der backup Depots in jede Azure-Abonnement erstellt werden kann|25 (Backup-Depots) <br/> 25-Rückgewinnungsservice vault pro region|
|Anzahl der Sicherung pro Tag eingeplant werden kann|3 pro Tag für Windows Server-Client <br/> 2 pro Tag SCDPM <br/> Täglich für IaaS VMs|
|Datenträger mit einer Azure Virtual Machine für Sicherung verbunden|16|

- <sup>1</sup> 54400-GB-Grenzwert gilt nicht für IaaS VM Backup.

