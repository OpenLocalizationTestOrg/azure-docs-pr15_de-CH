<properties
    pageTitle="Key Vault Mandanten-ID ändern, nachdem ein Abonnement verschieben | Microsoft Azure"
    description="Erfahren Sie, wie die Mandanten-ID für ein Schlüssel Depot wechseln, nachdem ein Abonnement in einem anderen Mandanten verschoben wird"
    services="key-vault"
    documentationCenter=""
    authors="amitbapat"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/13/2016"
    ms.author="ambapat"/>

# <a name="change-a-key-vault-tenant-id-after-a-subscription-move"></a>Ändern Sie eine Schlüssel Depot Mandanten-ID, nachdem ein Abonnement verschieben
### <a name="q-my-subscription-was-moved-from-tenant-a-to-tenant-b-how-do-i-change-the-tenant-id-for-my-existing-key-vault-and-set-correct-acls-for-principals-in-tenant-b"></a>F: mein Abonnement wurde von Mandanten ein Mieter b verschoben. Wie ändern die Mandanten-ID für meine vorhandene Schlüssel Depot und festgelegt richtige Zugriffssteuerungslisten für Hauptbenutzer Mandanten B?

Beim Erstellen eines neuen Schlüssel Depots in einem Abonnement ist es Standard Azure Active Directory Tenant-ID für dieses Abonnement automatisch verbunden. Alle Access-Richtlinie sind auch diesem Mandanten-ID verbunden. Azure-Abonnement Mieter ein Mieter B verschieben, werden die vorhandenen Schlüssel Depots Sicherheitsprinzipale (Benutzer und Programme) Mandanten b zugreifen Um dieses Problem zu beheben, müssen Sie:

- Ändern die Mandanten-ID verknüpft, mit allen vorhandenen Schlüssel dieses Abonnement Mieter b
- Entfernen Sie alle vorhandenen Richtlinien Zugriffseinträge
- Fügen Sie neuer Access Policy Einträge die Mieter b hinzu

Beispielsweise haben Sie wichtige Depot 'Myvault' in einem Abonnement, die aus Mieter ein Mieter B, hier die Mandanten-ID für diesen Schlüssel Tresor ändern und Entfernen von alten Richtlinien.

<pre>
$vaultResourceId = (Get-AzureRmKeyVault - VaultName Myvault). ResourceId $vault = Get-AzureRmResource – ResourceId $vaultResourceId - ExpandProperties $vault. Properties.TenantId = (Get-AzureRmContext). Tenant.TenantId $vault. Properties.AccessPolicies = @() $vaultResourceId AzureRmResource Set - ResourceId-Eigenschaften $vault. Eigenschaften
</pre>

Da dieses Depot Mieter A vor dem Verschieben der ursprüngliche Wert des **$vault wurde. Properties.TenantId** ist ein Mieter bei **(Get-AzureRmContext). Tenant.TenantId** ist b Mieter

Vault ist die richtige Mandanten-ID zugeordnet und alte Access Policy Einträge entfernt festgelegt neue Richtlinieneinträge [Satz AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/mt603625.aspx).

## <a name="next-steps"></a>Nächste Schritte

Haben Sie Fragen zur Azure Key Vault, besuchen Sie die [Azure Key Vault-Foren](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).
