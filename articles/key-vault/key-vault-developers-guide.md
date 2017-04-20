<properties
   pageTitle="Vault-Entwicklerhandbuch Schlüssel | Microsoft Azure"
   description="Azure Key Vault können Entwickler kryptografische Schlüssel innerhalb der Microsoft Azure-Umgebung verwalten. "
   services="key-vault"
   documentationCenter=""
   authors="BrucePerlerMS"
   manager="mbaldwin"
   editor="bruceper" />
<tags
   ms.service="key-vault"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/03/2016"
   ms.author="bruceper" />

# <a name="azure-key-vault-developers-guide"></a>Das Entwicklerhandbuch von Azure-Tresor
Mit Key Vault, werden vertrauliche Informationen in Ihrer Anwendung sicher auf, dass:

- Schlüssel und geheime Schlüssel ohne den Code selbst schreiben geschützt und Sie einfach können sie aus einer Anwendung.
- Sie werden Ihre eigenen Kunden und verwalten ihre eigenen Schlüssel auf die wichtigsten Funktionen konzentrieren können. Auf diese Weise werden Ihre Anwendung nicht Verantwortung oder potentielle Kunden Mieter und geheime Schlüssel besitzen.
- Ihrer Anwendung können Schlüssel für die Signierung und Verschlüsselung noch hält das Schlüsselmanagement externe aus Ihrer Anwendung so, dass die Lösung für eine Anwendung, die geografisch verteilt.

- Mit der Version September 2016 Key Vault Anwendung jetzt nutzen Schlüssel Depot Zertifikate. Weitere Informationen finden Sie unter **Schlüssel und geheime Daten Zertifikate** Artikel [REST Verweis](https://msdn.microsoft.com/library/azure/dn903623.aspx).

Weitere allgemeine Informationen auf Azure Schlüssel finden Sie unter [Key Vault](key-vault-whatis.md).

## <a name="videos"></a>Videos
Dieses Video zeigt, wie Key Vault erstellen und aus der Anwendung 'Hallo Schlüsseltresor' Beispiel verwenden.

[AZURE.VIDEO azure-key-vault-developer-quick-start]

Links zu Ressourcen im Video:
- [Azure PowerShell](http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409)
- [Azure-Tresor Beispielcode](http://go.microsoft.com/fwlink/?LinkId=521527&clcid=0x409)

Erfahren Sie können mehr [Schlüssel Depot Blog](http://aka.ms/kvblog) folgen und [Schlüssel Depot Forum](http://aka.ms/kvforum)teilnehmen.

## <a name="creating-and-managing-key-vaults"></a>Erstellen und Verwalten von wichtigen Depots

Bevor Azure Key Vault im Code bearbeiten, können Sie erstellen und Verwalten von Depots REST, Ressourcenmanager Vorlagen, PowerShell oder CLI wie in den folgenden Artikeln beschrieben:

- [Erstellen und Verwalten von wichtigen Depots Rest](https://msdn.microsoft.com/library/azure/mt620024.aspx)
- [Erstellen und Verwalten von wichtigen Depots mit PowerShell](key-vault-get-started.md)
- [Erstellen und Verwalten von wichtigen Depots mit CLI](key-vault-manage-with-cli.md)
- [Ein Schlüssel Depot erstellen und Hinzufügen eines geheimen Schlüssels über eine Vorlage Azure-Ressourcen-Manager](../resource-manager-template-keyvault.md)

>[AZURE.NOTE] Operationen mit wichtigen Depots werden durch AAD authentifiziert und autorisiert durch Schlüssel des eigenen Richtlinie, pro Depot definiert.

## <a name="coding-with-key-vault"></a>Programmieren mit Schlüssel

Key Vault-Managementsystem für Programmierer besteht aus mehreren Schnittstellen mit der Grundlage [Schlüssel Depot REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn903609.aspx).

Sie können erfolgreich genehmigungspflichtig Folgendes:

- Verwalten Sie [Erstellen](https://msdn.microsoft.com/library/azure/dn903634.aspx), [Importieren](https://msdn.microsoft.com/library/azure/dn903626.aspx), [Aktualisieren](https://msdn.microsoft.com/library/azure/dn903616.aspx), [Löschen](https://msdn.microsoft.com/library/azure/dn903611.aspx) und andere Vorgänge mit kryptografischen Schlüssel

- Verwalten Sie geheimer Daten [Abrufen](https://msdn.microsoft.com/library/azure/dn903633.aspx), [Aktualisieren](https://msdn.microsoft.com/library/azure/dn986818.aspx), [Löschen](https://msdn.microsoft.com/library/azure/dn903613.aspx) und andere Vorgänge mit

- Verwenden Sie kryptografische Schlüssel [mit](https://msdn.microsoft.com/library/azure/dn878096.aspx)/[Überprüfen](https://msdn.microsoft.com/library/azure/dn878082.aspx), [WrapKey](https://msdn.microsoft.com/library/azure/dn878066.aspx)/[UnwrapKey](https://msdn.microsoft.com/library/azure/dn878079.aspx) und [Verschlüsseln](https://msdn.microsoft.com/library/azure/dn878060.aspx)/Vorgänge[Entschlüsseln](https://msdn.microsoft.com/library/azure/dn878097.aspx)

Die folgenden SDKs sind für die Arbeit mit Schlüssel:

|[![.NET](./media/key-vault-developers-guide/msft.netlogo_purple.png)](https://msdn.microsoft.com/library/mt765854.aspx)|[![Node.js](./media/key-vault-developers-guide/nodejs.png)](http://azure.github.io/azure-sdk-for-node/azure-arm-keyvault/latest)
|:--:|:--:|
|[.NET SDK-Dokumentation](https://msdn.microsoft.com/library/mt765854.aspx)|[Node.js-SDK-Dokumentation](http://azure.github.io/azure-sdk-for-node/azure-arm-keyvault/latest)|
|[.NET SDK-Paket auf Nuget](http://www.nuget.org/packages/Microsoft.Azure.KeyVault)|[Node.js SDK-Paket](https://www.npmjs.com/package/azure-keyvault)|

Weitere Informationen zur Version 2.x .NET SDK finden Sie unter [Release Notes](key-vault-dotnet2api-release-notes.md).

## <a name="example-code"></a>Beispielcode
Vollständige Beispiele für Key Vault mit Ihrer Anwendung finden Sie unter:

- Beispiel-Anwendung mit .NET *HelloKeyVault* und Azure Webdienstbeispiel. [Azure Key Vault-Beispiele](http://www.microsoft.com/download/details.aspx?id=45343)
- Lernprogramm für Azure Key Vault von einer Webanwendung in Azure erlernen. [Verwenden von Azure Key Vault from a Web Application](key-vault-use-from-web-application.md)

## <a name="how-tos"></a>Gewusst wie

Die folgenden Artikel und Szenarien bieten Aufgabe bestimmten für die Arbeit mit Azure Schlüssel:

- [Key Vault Tenant-ID ändern nach dem Verschieben von Abonnement](key-vault-subscription-move-fix.md) - Azure-Abonnement Mieter ein Mieter B verschieben, sind Ihre vorhandenen Schlüssel Depots von Sicherheitsprinzipalen (Benutzer und Programme) Mandanten b Update verwenden dieses Handbuchs.
- [Zugriff auf Schlüssel Depot Firewall](key-vault-access-behind-firewall.md) - Schlüssel Tresor zugreifen muss die Clientanwendung Schlüssel Depot mehrere Endpunkte für verschiedene Funktionen zugreifen können.

- [Wie generieren und Transfer HSM-Protected für Azure Key Vault](key-vault-hsm-protected-keys.md) - dadurch planen, erstellen und übertragen eigener HSM-geschützten Schlüssel mit Azure Schlüssel.
- [Wie sichere Werte (z. B. Kennwörter) während der Bereitstellung](../resource-manager-keyvault-parameter.md) - Installation einen sicheren Wert (z. B. ein Kennwort) als Parameter übergeben werden sollen, können Sie diesen Wert als Geheimnis einer Azure Key Vault und der Wert im anderen Ressourcenmanager Vorlagen speichern.
- [Wie Schlüssel Depot für erweiterbare Schlüsselmanagement mit SQL Server](https://msdn.microsoft.com/library/dn198405.aspx) - der SQL Server-Connector Azure Key Vault ermöglicht SQL Server und SQL in VM Azure Key Vault-Dienst als erweiterbare Key Management (EKM) Provider Verschlüsselungsschlüssel für Applikationen Link zu nutzen; Transparente Verschlüsselung Sicherung Verschlüsselung und Verschlüsselung Spalte.
- [Zum Bereitstellen von Zertifikaten auf VMs aus Schlüsseltresor](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/) - Cloud-Anwendung auf Windows Azure ausgeführte in einer VM benötigt ein Zertifikat. Wie Sie dieses Zertifikat in diesem virtuellen Computer heute?
- [Wie Setup Key Vault mit Schlüsselrotation Ende Überwachung](key-vault-key-rotation-log-monitoring.md) - diese schrittweise Einrichtung Schlüsselrotation und Überwachung mit Azure Schlüssel.

Weitere aufgabenspezifische Informationen zum integrieren und Azure Schlüssel Depots mit Beispiele [Ryan Jones Azure Ressourcenmanager Vorlage für Schlüssel Depot](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).

## <a name="integrated-with-key-vault"></a>Integriert mit Schlüssel

Diese Artikel sind zu anderen Szenarios und Diensten, die uns oder mit Schlüssel integrieren.

- [Azure Datenträgerverschlüsselung](../security/azure-security-disk-encryption.md) nutzt der Branche [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) Standardfunktion von Windows und die Funktion [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux Verschlüsselung für das Betriebssystem und die Datenträger bereitstellen. Die Lösung ist integriert mit Azure Schlüssel zum Steuern und Verwalten der Datenträger Verschlüsselungsschlüssel und Geheimnisse in Ihrem Abonnement Key Vault dabei sicherzustellen, dass alle virtuellen Laufwerke ruhende in Azure-Speicher verschlüsselt werden.


## <a name="supporting-libraries"></a>Unterstützung von Bibliotheken

- [Microsoft Azure Key Vault-Kernbibliothek](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core) enthält `IKey` und `IKeyResolver` Schnittstellen für Schlüssel Bezeichner Lokalisierung und Vorgänge mit Schlüsseln.

- [Microsoft Azure Key Vault Extensions](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions) bietet erweiterte Funktionen für Azure Key Vault.

## <a name="other-key-vault-resources"></a>Andere Schlüssel Vault-Ressourcen
- [Key Vault-Blog](http://aka.ms/kvblog)
- [Key Vault-Forum](http://aka.ms/kvforum)
