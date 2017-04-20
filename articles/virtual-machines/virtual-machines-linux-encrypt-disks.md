<properties
   pageTitle="Verschlüsseln von Datenträgern auf Linux VM | Microsoft Azure"
   description="Datenträger auf einem Linux VM Azure-CLI mit der Ressourcen-Manager-Bereitstellungsmodell verschlüsseln"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/11/2016"
   ms.author="iainfou"/>

# <a name="encrypt-disks-on-a-linux-vm-using-the-azure-cli"></a>Verschlüsseln Sie Festplatten auf einem Linux VM mit der Azure-CLI
Für erweiterte Virtual Machine (VM) Sicherheit und Compliance können virtuelle Laufwerke in Azure ruhende verschlüsselt werden. Festplatten sind verschlüsselt kryptografische Schlüssel, die in einem Tresor Azure Schlüssel gesichert werden. Sie steuern diese kryptografische Schlüssel und deren Verwendung überwachen können. Dieser Artikel beschreibt, wie virtuelle Laufwerke auf einem Linux VM Azure-CLI mit der Ressourcen-Manager-Bereitstellungsmodell verschlüsselt.


## <a name="quick-commands"></a>Schnellzugriff
Möchten Sie schnell, die folgenden Abschnitt Details die Base Aufgabe Befehle virtuelle Laufwerke auf Ihrem virtuellen Computer verschlüsseln. Ausführliche Informationen und Kontext für jeden Schritt können den Rest des Dokuments [hier](#overview-of-disk-encryption)gefunden.

Sie benötigen die [Neueste Azure-CLI](../xplat-cli-install.md) installiert und mit den Ressourcen-Manager-Modus wie folgt angemeldet:

```
azure config mode arm
```

Ersetzen Sie in den folgenden Beispielen Parameternamen wird durch Ihre eigenen Werte. Beispiel-Parameternamen enthalten `myResourceGroup`, `myKeyVault`, und `myVM`.

Erstens Azure Key Vault-Anbieter in der Azure-Abonnement aktivieren und eine Ressourcengruppe erstellen. Im folgenden Beispiel wird ein Ressourcenname Gruppe `myResourceGroup` in der `WestUS` Speicherort:

```bash
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

Erstellen Sie ein Azure-Depot. Im folgenden Beispiel wird ein Schlüssel Depot mit dem Namen `myKeyVault`:

```bash
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

Kryptografischen Schlüssel in Ihrem Tresor Schlüssel erstellen und für festplattenverschlüsselung aktivieren. Im folgenden Beispiel wird einen Schlüssel mit dem Namen `myKey`:

```bash
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```

Erstellen Sie eine mit Azure Active Directory Authentifizierung und Austausch von kryptografischen Schlüsseln aus Schlüsseltresor. Die `--home-page` und `--identifier-uris` nicht routingfähige Adresse sein müssen. Für höchste Sicherheit müssen Clientschlüssel anstelle von Kennwörtern verwendet werden. Azure-CLI kann derzeit nicht Clientschlüssel generieren. Clientschlüssel können nur in Azure-Portal erstellt. Das folgende Beispiel erstellt einen Azure Active Directory Endpunkt mit dem Namen `myAADApp` und dem Kennwort `myPassword`. Geben Sie Ihr Kennwort folgendermaßen an:

```bash
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Hinweis Die `applicationId` in der Ausgabe des voranstehenden Befehls angezeigt. Diese Anwendung-ID wird in den folgenden Schritten:

```bash
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```

Eine vorhandene VM Datenträger hinzufügen. Im folgenden Beispiel wird einen Datenträger einer VM mit dem Namen `myVM`:

```bash
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Überprüfen Sie die Details für den Schlüsseltresor und erstellten Schlüssel. Sie benötigen die Depot-ID des Schlüssels URI und Schlüssel URL im letzten Schritt. Das folgende Beispiel überprüft die Details für ein Schlüssel Depot mit dem Namen `myKeyVault` und mit dem Namen `myKey`:

```bash
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

Verschlüsseln Sie Festplatten wie folgt in eigene Parameternamen eingeben:

```bash
azure vm enable-disk-encryption --resource-group myResourceGroup --vm-name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

Azure-CLI zur ausführliche Fehlermeldungen bei der Verschlüsselung nicht Verfügung. Weitere Informationen zur Problembehandlung finden Sie unter `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log`. Befehl hat viele Variablen erhalten Sie nicht viel erkennen, warum der Prozess fehlschlägt, wäre eine vollständige Beispiel wie folgt:

```bash
azure vm enable-disk-encryption -g myResourceGroup -n MyVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \ 
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

Überprüfen Sie schließlich den Verschlüsselungsstatus erneut, um sicherzustellen, dass Ihre virtuellen Laufwerke nun verschlüsselt wurden. Im folgende Beispiel überprüft den Status eines virtuellen Computers mit dem Namen `myVM` in der `myResourceGroup` Ressourcengruppe:

```bash
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```

## <a name="overview-of-disk-encryption"></a>Übersicht über festplattenverschlüsselung
Virtuelle Laufwerke auf Linux VMs werden statisch mit [dm-Crypt](https://wikipedia.org/wiki/Dm-crypt)verschlüsselt. Kostenfrei für die Verschlüsselung von virtuellen Laufwerken in Azure. Kryptografische Schlüssel werden in Azure Key Vault Software-Schutz und importieren oder Erstellen der Schlüssel im Hardwaresicherheitsmodule (HSMs) zertifiziert FIPS 140-2 Level 2-Standards. Sie behalten die Kontrolle dieser kryptografische Schlüssel und deren Verwendung überwachen können. Diese kryptografische Schlüssel zum Verschlüsseln und Entschlüsseln von virtuellen Datenträgern für die VM. Ein Endpunkt Azure Active Directory bietet einen sicheren Mechanismus zum Ausstellen dieser kryptografische Schlüssel wie VMs und aus betrieben werden.

Der Prozess zum Verschlüsseln einer VM lautet wie folgt:

1. Erstellen Sie einen kryptografischen Schlüssel in Azure-Schlüsseltresor.
2. Konfigurieren Sie den kryptografischen Schlüssel für die Verschlüsselung von Festplatten verwendbar.
3. Zum Lesen des Kryptografieschlüssel Azure Key Vault erstellen Sie eine Azure Active Directory mit den entsprechenden Berechtigungen.
4. Befehl verschlüsseln Sie Ihre Laufwerke Azure Active Directory Endpunkt und entsprechenden kryptografischen Schlüssels angeben.
5. Endpunkt Azure Active Directory fordert den erforderlichen kryptografischen Schlüssel von Azure Key Vault.
6. Virtuelle Laufwerke werden mit den bereitgestellten kryptografischen Schlüssel verschlüsselt.


## <a name="supporting-services-and-encryption-process"></a>Unterstützung von Services und Verschlüsselung
Datenträgerverschlüsselung benötigt die folgenden zusätzlichen Komponenten:

- **Azure Key Vault** - zum Schutz von kryptografischer Schlüsseln und geheime Schlüssel für die Ver-bzw. Entschlüsselung Datenträger verwendet. 
  - Sofern vorhanden, können Sie eine vorhandene Azure Key Vault. Sie haben kein Depot Schlüssel verschlüsseln Festplatten widmen.
  - Separate administrative Grenzen und wichtige Sichtbarkeit, können Sie ein dediziertes Schlüssel Depot erstellen.
- **Azure Active Directory** - behandelt sicheren Austausch von erforderlichen kryptografische Schlüssel und Authentifizierung für angeforderten Aktionen. 
  - In der Regel können eine vorhandene Instanz von Azure Active Directory Sie für Ihre Anwendung mit. 
  - Die Anwendung wird über einen Endpunkt für die Schlüssel Depot und virtuellen Computer anfordern und erhalten die entsprechenden kryptografischen Schlüssel ausgestellt. Entwickeln Sie keine Anwendung, die in Azure Active Directory integriert.


## <a name="requirements-and-limitations"></a>Vorschriften und Grenzen
Unterstützte Szenarien und für festplattenverschlüsselung:

- Die folgenden LinuxServer - SKUs Ubuntu CentOS, SUSE und SUSE Linux Enterprise Server (SLES) und Red Hat Enterprise Linux.
- Alle Ressourcen (wie Schlüssel Depot Speicherkonto und VM) muss dieselbe Azure und Abonnements.
- Standard, D, DS, G und GS-Serie VMs.

Datenträgerverschlüsselung ist in den folgenden Szenarien derzeit nicht unterstützt:

- Grundlegende Ebene VMs.
- VMs mit klassischen Bereitstellungsmodell erstellt.
- Deaktivieren der OS festplattenverschlüsselung unter Linux VMs.
- Aktualisieren die kryptografischen Schlüssel auf eine bereits verschlüsselte Linux VM.


## <a name="create-the-azure-key-vault-and-keys"></a>Azure Key Vault und Schlüssel erstellen
Schließen Sie den Rest dieses Handbuchs benötigen Sie die [Neueste Azure-CLI](../xplat-cli-install.md) installiert und mit den Ressourcen-Manager-Modus wie folgt angemeldet:

```
azure config mode arm
```

Beispiele Befehl Ersetzen Sie alle Beispiel mit Namen, Position und Werte. Die folgenden Beispiele verwenden ein `myResourceGroup`, `myKeyVault`, `myAADApp`usw..

Der erste Schritt ist die Erstellung einer Azure Schlüssel Depot Ihre kryptografischen Schlüssel speichern. Azure Key Vault speichert Schlüssel, geheime Schlüssel oder Kennwörter, die Sie sicher in Ihrer Programme und Dienste implementieren können. Virtuelles Laufwerk Verschlüsselung verwenden Sie Key Vault einen kryptografischen Schlüssel speichern, der zum Verschlüsseln oder Entschlüsseln der virtuellen Laufwerke. 

Azure Key Vault-Anbieter in der Azure-Abonnement aktivieren und eine Ressourcengruppe erstellen. Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen `myResourceGroup` in der `WestUS` Speicherort:

```bash
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

Azure Key Vault mit der kryptografischen Schlüssel zugeordneten Serverressourcen wie Speicher und die VM selbst muss sich im selben Bereich befinden. Das folgende Beispiel erstellt eine Azure Schlüssel Depot mit dem Namen `myKeyVault`:

```bash
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

Sie können kryptografische Schlüssel mit Software oder Hardware Security Model (HSM) Schutz speichern. Eine HSM muss eine Prämie Schlüssel Depot. Gibt zusätzliche Kosten zum Erstellen einer Prämie Schlüssel Depot als standard Schlüssel Vault, in dem Schlüssel Software geschützt gespeichert. Erstellen Sie eine Prämie Schlüssel Depot im vorherigen Schritt hinzufügen `--sku Premium` dem Befehl. Das folgende Beispiel verwendet Schlüssel Software geschützt seit wir ein standard Schlüssel Depot. 

Für beide Modelle Schutz muss die Azure-Plattform Zugriff werden die kryptografischen Schlüssel anfordern, wenn die VM startet um die virtuellen Laufwerken zu entschlüsseln. Erstellen Sie einen Schlüssel in den Schlüsseltresor und mit virtuellen festplattenverschlüsselung aktivieren. Im folgenden Beispiel wird einen Schlüssel mit dem Namen `myKey` und für festplattenverschlüsselung aktiviert:

```bash
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```


## <a name="create-the-azure-active-directory-application"></a>Active Directory Azure-Anwendung erstellen
Virtuelle Laufwerke verschlüsselt oder entschlüsselt, verwenden Sie einen Endpunkt um die Authentifizierung und den Austausch von kryptografischen Schlüsseln aus Schlüsseltresor. Dieser Endpunkt einer Anwendung Azure Active Directory ermöglicht die Azure-Plattform die entsprechenden kryptografischen Schlüssel für die VM anfordern. Eine Standardinstanz Azure Active Directory ist in Ihrem Abonnement verfügbar, wenn viele Organisationen Azure Active Directory Verzeichnisse gewidmet haben.

Erstellen Sie keine vollständige Active Directory Azure-Anwendung die `--home-page` und `--identifier-uris` Parameter im folgenden Beispiel müssen nicht routingfähige Adresse sein. Im folgenden Beispiel wird ebenfalls eine geheime Passwort statt Generieren von Schlüsseln aus in Azure-Portal. Dieses Mal Schlüssel generieren von Azure-CLI möglich. 

Erstellen Sie Azure Active Directory Anwendung. Im folgenden Beispiel wird eine Anwendung mit dem Namen `myAADApp` und dem Kennwort `myPassword`. Geben Sie Ihr Kennwort folgendermaßen an:

```bash
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Notieren Sie die `applicationId` in der Ausgabe des voranstehenden Befehls zurückgegeben wird. ID der Anwendung wird der verbleibenden Schritte verwendet. Als Nächstes Erstellen eines Dienstprinzipalnamens (SPN) so, dass die Anwendung in Ihrer Umgebung. Um erfolgreich ver- und Entschlüsseln von virtuellen Laufwerken, müssen Berechtigungen für den kryptografischen Schlüssel im Schlüssel Depot gespeichert die Azure Active Directory Anwendung Schlüssel lesen festgelegt. 

Den SPN erstellen und geeignete Berechtigungen wie folgt festlegen:

```bash
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```


## <a name="add-a-virtual-disk-and-review-encryption-status"></a>Hinzufügen eines virtuellen Laufwerks und Verschlüsselung überprüfen
Einige virtuelle Laufwerke tatsächlich verschlüsselt einen Datenträger auf einem vorhandenen virtuellen Computer hinzufügen können. Fügen Sie einen 5Gb-Datenträger vorhandenen VM wie folgt:

```bash
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Virtuelle Laufwerke sind derzeit nicht verschlüsselt. Überprüfen Sie den aktuellen Verschlüsselungsstatus Ihrer VM wie folgt:

```bash
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="encrypt-virtual-disks"></a>Virtuelle Laufwerke verschlüsseln
Um die virtuellen Laufwerke jetzt verschlüsseln, bringen Sie zusammen die vorherigen Komponenten:

1. Active Directory Azure-Anwendung und ein Kennwort angeben.
2. Geben Sie Schlüssel Depots zur Speicherung der Metadaten für verschlüsselte Datenträger an.
3. Geben Sie die kryptografischen Schlüssel für das aktuelle Ver- bzw. Entschlüsselung verwendet.
4. Geben Sie an, ob Betriebssystem-Datenträger, der Datenträger oder alle verschlüsseln.

Überprüfen Sie die Details für Azure Key Vault und dem erstellten Schlüssel Depot ID URI, und drücken die URL im letzten Schritt können:

```bash
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

Verschlüsseln Sie Ihre Laufwerke mit der Ausgabe der `azure keyvault show` und `azure keyvault key show` Befehle wie folgt:

```bash
azure vm enable-disk-encryption --resource-group myResourceGroup --vm-name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

Befehl viele Variablen hat, wird im folgenden Beispiel wird der vollständige Befehl Referenz:

```bash
azure vm enable-disk-encryption -g myResourceGroup -n MyVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \ 
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

Azure-CLI zur ausführliche Fehlermeldungen bei der Verschlüsselung nicht Verfügung. Weitere Informationen zur Problembehandlung finden Sie unter `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log` auf dem virtuellen Computer Sie verschlüsseln.

Überprüfen Sie den Verschlüsselungsstatus erneut, um sicherzustellen, dass Ihre virtuellen Laufwerke nun verschlüsselt wurden und letztendlich ermöglicht:

```bash
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="add-additional-data-disks"></a>Fügen zusätzlicher Datenträger hinzu
Nachdem der Datenträger verschlüsselt haben, können Sie später weitere virtuelle Laufwerke für die VM hinzufügen und auch verschlüsseln. Beim Ausführen der `azure vm enable-disk-encryption` Befehl, erhöhen die Sequenz Version mit der `--sequence-version` Parameter. Parameters dieser Version können Sie wiederholte Operationen auf dem gleichen virtuellen Computer ausführen.

Beispielsweise können die VM eine zweite virtuelle Festplatte wie folgt hinzufügen:

```bash
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Erneut den Befehl virtuelle Laufwerke Verschlüsseln dieser zum Hinzufügen der `--sequence-version` -Parameter und erhöht den Wert unserer ersten Ausführung wie folgt:

```bash
azure vm enable-disk-encryption --resource-group myResourceGroup --vm-name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
  --sequence-version 2
```


## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Verwalten von Azure Key Vault, einschließlich kryptografische Schlüssel und Gewölben löschen finden Sie unter [Key Vault verwalten CLI verwenden](../key-vault/key-vault-manage-with-cli.md).
- Weitere Informationen zu datenträgerverschlüsselung Vorbereitung einer verschlüsselten benutzerdefinierten VM Azure hochladen finden Sie unter [Azure Datenträgerverschlüsselung](../security/azure-security-disk-encryption.md).