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
    ms.date="10/12/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-security-and-identity"></a>Azure Regierung und Identität

##  <a name="key-vault"></a>Key Vault
Einzelheiten zu diesen Dienst und dessen Verwendung finden Sie die <a href="https://azure.microsoft.com/documentation/services/key-vault">Öffentliche Dokumentation Azure Key Vault.</a>

### <a name="data-considerations"></a>Aspekte der Daten
Die folgenden Informationen identifizieren Azure Government Grenze Azure Key Vault:

| Reguliert/gesteuert Daten erlaubt | Reguliert/gesteuert Daten nicht zulässig |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Alle mit einer Azure Key Vault-Schlüssel verschlüsselte Daten enthalten Daten reguliert/gesteuert. | Azure Key Vault Metadaten darf nicht Exportkontrolle Daten enthalten. Zu diesen Metadaten gehören alle Konfigurationsdaten beim Erstellen und Verwalten von Ihrem Tresor Schlüssel eingegeben.  Regulierte/gesteuert Daten nicht in die folgenden Felder eingeben: Ressourcengruppennamen Key Vault Namen, Namen |

Key Vault ist erhältlich in Azure Government. In der Öffentlichkeit ist es keine Erweiterung Key Vault über PowerShell und CLI nur.

## <a name="next-steps"></a>Nächste Schritte

Zusätzliche Informationen und Updates Abonnieren der <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Regierung Blog.</a>
