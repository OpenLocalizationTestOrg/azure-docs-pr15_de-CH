<properties
    pageTitle="Sichern Sie wichtige Vault | Microsoft Azure"
    description="Verwalten von Berechtigungen zum Verwalten von Depots und Schlüssel und geheime Schlüssel Vault. Authentifizierung und Autorisierung Modell Schlüssel Depot und den wichtigsten Tresor sichern"
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
    ms.date="10/07/2016"
    ms.author="ambapat"/>


# <a name="secure-your-key-vault"></a>Sichern Sie Ihre wichtigsten vault

Azure Key Vault ist ein Clouddienst, der Schlüssel und geheime Schlüssel (z. B. Zertifikate, Verbindungszeichenfolgen, Kennwörter) für Ihre Cloud schützt. Diese Daten sind vertraulich und geschäftskritische, Zugriff auf Ihre wichtigsten Depots sichern, sodass nur autorisierte soll möglich Applikationen und Benutzer wichtige Depot. Dieser Artikel bietet eine Übersicht über wichtige Vault-Zugriffsmodell, erläutert, Authentifizierung und Autorisierung und beschreibt Zugang zu schlüsseltresor für die Cloud-Anwendung mit einem Beispiel.

## <a name="overview"></a>Übersicht

Zugriff auf einen Schlüssel Tresor erfolgt über zwei separate Schnittstellen: Managementebene und Datenebene. Für beide Ebenen ist ordnungsgemäße Authentifizierung und Autorisierung erforderlich, bevor ein Aufrufer (Benutzer oder eine Anwendung) Schlüssel Tresor zugreifen kann. Authentifizierung wird die Identität des Anrufers und Autorisierung welche Vorgänge bestimmt der Aufrufer ausführen darf.

Für die Authentifizierung verwenden Verwaltungsebene und Datenebene Azure Active Directory. Zur Autorisierung verwendet Verwaltungsebene rollenbasierte Zugriffskontrolle (RBAC) Datenebene Schlüssel Richtlinie verwendet.

Hier wird eine kurze Übersicht über die Themen:

[Authentifizierung mit Active Directory Azure](#authentication-using-azure-active-direcrory) - in diesem Abschnitt wird erläutert, wie ein Aufrufer Azure Active Directory auf Schlüssel Depot Verwaltungsebene und Datenebene authentifiziert. 

[Management-Ebene und Datenebene](#management-plane-and-data-plane) - Management-Ebene und Datenebene sind zwei Access Ebenen für den Zugriff auf den Schlüssel Tresor. Jede Ebene Zugriff unterstützt bestimmte Operationen. Dieser Abschnitt beschreibt die Endpunkte Zugriff, unterstützt und von jeder Ebene verwendeten Methode zuzugreifen. 

[Management-Ebene Access Control](#management-plane-access-control) - In diesem Abschnitt werden betrachten wir Zugriff auf Management-Ebene Operationen rollenbasierte Zugriffskontrolle mit.

[Daten plane Zugriffskontrolle](#data-plane-access-control) - dieser Abschnitt beschreibt, wie mit Schlüssel Richtlinie Ebene Datenzugriff.

[Beispiel](#example) – in diesem Beispiel beschreibt die Zugriffskontrolle für den Schlüssel Tresor zu drei verschiedenen Teams (Sicherheitsteam Entwickler-Operatoren und Prüfer) für bestimmte Aufgaben zu entwickeln, verwalten und Überwachen eine Anwendung in Azure einrichten.


## <a name="authentication-using-azure-active-directory"></a>Authentifizierung mit Active Directory Azure

Erstellen eines Schlüssels Depots in Azure-Abonnement wird automatisch das Abonnement Azure Active Directory Mieter zugeordnet. In diesem Mandanten auf dieses wichtigsten Depot müssen alle Aufrufer (Benutzer und Programme) registriert werden. Mit Azure Active Directory Schlüssel Tresor zugreifen muss eine Anwendung oder ein Benutzer authentifizieren. Dies gilt für beide Management-Ebene und Ebene Datenzugriff. In beiden Fällen kann eine Anwendung Schlüssel Depot auf zwei Arten zugreifen:

-  **Benutzer + app Zugriff** – ist normalerweise für Applikationen, die wichtigsten Depot für einen angemeldeten Benutzer zugreifen. Azure PowerShell sind Azure-Portal Beispiele für diese Art des Zugriffs. Es gibt zwei Methoden, um Benutzern Zugriff gewähren: eine Möglichkeit besteht darin, Benutzern Zugriff gewähren, können sie jede Anwendung Key Vault zugreifen und umgekehrt, gewährt einem Benutzerzugriff auf wichtige Depot nur, wenn sie eine bestimmte Anwendung (so genannte zusammengesetzte Identität). 
-  **nur app** - für Applikationen, die Hintergrundaufträge usw. Daemon Services ausführen. Die Anwendungsidentität erhält Zugriff auf die wichtigsten Tresor.

Die Anwendung in beiden Anwendungstypen Azure Active Directory eines [unterstützten Authentifizierungsmethoden](../active-directory/active-directory-authentication-scenarios.md) authentifiziert und erhält einen Token. Verwendete Authentifizierungsmethode hängt von der Anwendung. Die Anwendung verwendet dieses Token und Schlüssel Tresor REST-API-Anforderung sendet. Bei Zugriff auf Management-Ebene werden die Anfragen über Azure Ressourcenmanager Endpunkt geleitet. Beim Zugriff auf Datenebene vault Anwendung kommuniziert direkt mit einem Schlüssel Endpunkt. Weitere Informationen finden Sie auf den [gesamten Authentifizierungsablauf](../active-directory/active-directory-protocols-oauth-code.md). 

Für die Anwendung einen Token anfordert Ressourcenname ist abhängig davon, ob die Anwendung Verwaltungsebene oder Datenebene zugreift. Daher ist der Ressourcenname Management-Ebene oder Daten Ebene Endpunkten in der Tabelle in einem späteren Abschnitt je nach Azure beschrieben.

Eine einzelne Authentifizierungsmechanismus Management und Datenebene hat ihre Vorteile:

- Organisationen können Zugriff auf alle wichtigen Depots in ihrem Unternehmen zentral steuern.
- Verlässt ein Benutzer verlieren sie sofort Zugriff auf alle wichtigen Depots in der Organisation
- Organisationen können Authentifizierung über die Optionen in Azure Active Directory (z. B. aktivieren mehrstufige Authentifizierung Sicherheit)

## <a name="management-plane-and-data-plane"></a>Managementebene und Datenebene

Azure Key Vault ist ein Azure über Azure-Ressourcen-Manager-Bereitstellungsmodell. Beim Erstellen eines Schlüssels Depots erhalten Sie virtuellen Container in dem andere Objekte wie Schlüssel, Schlüssel und Zertifikate erstellen. Anschließend greifen Sie auf den Schlüssel Tresor mit Verwaltungsebene Datenebene zur Ausführung bestimmter Vorgänge. Ebene Verwaltungsoberfläche zum Verwalten von Key Vault selbst erstellen, löschen, Aktualisieren der Attribute Key Vault und Festlegen von Richtlinien für die Datenebene. Ebene Schnittstelle dient zum Hinzufügen, löschen, ändern und verwenden Schlüssel, Schlüssel und Zertifikate in Ihrem Schlüssel Tresor.

Die Ebene und Daten Ebene Verwaltungsschnittstellen über verschiedene Endpunkte zugegriffen werden (siehe Tabelle). Die zweite Spalte in der Tabelle beschreibt die DNS-Namen für diese Endpunkte in unterschiedlichen Umgebungen Azure. Die dritte Spalte beschreibt die Vorgänge jeder Ebene Zugriff ausführen. Jede Ebene Zugriff hat auch eigene Zugriffssteuerungsmechanismus: für Management-Ebene Zugriffskontrolle mit (Azure Resource Manager Role-Based Access Control, RBAC) festgelegt wird, zwar für Daten Ebene Zugriffskontrolle mit Schlüssel Richtlinie festgelegt wird.

| Access-Ebene | Access-Endpunkte | Vorgänge | Zugriffssteuerungsmechanismus|
|--------------|------------------|--------------------|--------|
| Verwaltungsebene|**Global:**<br> Management.Azure.com:443<br><br> **Azure China:**<br> Management.chinacloudapi.CN:443<br><br> **Azure US-Regierung:**<br> Management.usgovcloudapi.NET:443<br><br> **Azure Deutschland:**<br> Management.microsoftazure.de:443 | Erstellen/Read/Update/Delete Key vault <br> Legen Sie Richtlinien für Schlüssel Depot<br>Satz Tags für Key vault | Azure Resource Manager Role-Based Access Control (RBAC) |
| Datenebene | **Global:**<br> &lt;Vault-Name&gt;. vault.azure.net:443<br><br> **Azure China:**<br> &lt;Vault-Name&gt;. vault.azure.cn:443<br><br> **Azure US-Regierung:**<br> &lt;Vault-Name&gt;. vault.usgovcloudapi.net:443<br><br> **Azure Deutschland:**<br> &lt;Vault-Name&gt;. vault.microsoftazure.de:443 | Schlüssel: Entschlüsseln, verschlüsseln, UnwrapKey, WrapKey, überprüfen, signieren, abrufen, auflisten, aktualisieren, erstellen, importieren, löschen, Sicherung, Wiederherstellen<br><br> Für vertrauliche Daten: abzurufen, Liste Satz löschen | Wichtige Richtlinie|

Die Management-Ebene und Daten Ebene Zugriffskontrollen arbeiten unabhängig voneinander. Beispielsweise ggf. eine Anwendung gewähren, um Tasten in einem Schlüssel müssen nur Ebene Zugriffsberechtigungen mit Key Vault-Richtlinien gewähren und keine Ebene Verwaltungszugriff für diese Anwendung erforderlich. Andererseits soll einen Benutzer Eigenschaften des Tresorraums und Tags lesen, aber keinen Zugriff auf Schlüssel, Schlüssel oder Zertifikate, diesen Benutzer erteilen können 'read' mit Rollenbasierten Zugriff und keinen Zugriff auf Datenebene ist erforderlich.

## <a name="management-plane-access-control"></a>Zugriffskontrolle für Management-Ebene

Die Verwaltungsebene besteht aus Operationen, die die wichtige Tresor selbst auswirken. Sie können z. B. erstellen oder löschen ein Schlüssel Depot. Sie erhalten eine Liste der Depots in einem Abonnement. Sie können abrufen Key Vault-Eigenschaften (z. B. SKU Tags) und Key Vault-Richtlinien, die steuern, die Benutzer und Programme, die Schlüssel und geheime Schlüssel im Schlüssel Tresor zugreifen können. Management-Ebene Zugriffskontrolle verwendet RBAC. Finden Sie die vollständige Liste der wichtigsten Vault-Operationen über Managementebene in der Tabelle im vorhergehenden Abschnitt ausgeführt werden können. 

### <a name="role-based-access-control-rbac"></a>Rollenbasierte Zugriffskontrolle (RBAC)
Jede Azure-Abonnement hat Azure Active Directory. Benutzer, Gruppen und Programme in diesem Verzeichnis können erteilt werden Zugriff auf Ressourcen in der Azure-Abonnement, das Bereitstellungsmodell Azure-Ressourcen-Manager verwenden. Diese Art der Zugriffskontrolle wird als Role-Based Access Control (RBAC) bezeichnet. Um diesen Zugriff zu verwalten, können Sie der [Azure-Portal](https://portal.azure.com/) [Azure CLI-Tools](../xplat-cli-install.md), [PowerShell](../powershell-install-configure.md)oder [Azure Ressourcenmanager REST-APIs](https://msdn.microsoft.com/library/azure/dn906885.aspx).

Mit der Azure-Ressourcen-Manager erstellen Sie den Schlüssel Tresor in einer Gruppe und Steuerelement Zugriff auf Ressourcen auf dem Management dieses wichtigsten Depot mithilfe von Azure Active Directory. Sie können z. B. Benutzer oder eine Gruppe können wichtige Depots in einer bestimmten Ressourcengruppe zu gewähren.

Sie können Benutzer, Gruppen und Programme in einem bestimmten Bereich Zugriff durch entsprechende RBAC-Rollen zuweisen. Beispielsweise würde Benutzer wichtige Depots verwalten Zugriff gewähren Sie eine vordefinierte Rolle "Key Vault Contributor" dieser Benutzer über einen bestimmten Bereich zuweisen. Der Bereich wäre in diesem Fall ein Abonnement, eine Ressourcengruppe oder nur einem bestimmten Schlüssel Depot. Eine Rolle auf Abonnement gilt für alle Ressourcengruppen und Ressourcen innerhalb dieses Abonnement. Eine Rolle auf Ressource gilt für alle Ressourcen in dieser Ressourcengruppe. Eine Rolle für eine bestimmte Ressource gilt nur für die Ressource. Werden mehrere vordefinierte Rollen (siehe [RBAC: Standardrollen](../active-directory/role-based-access-built-in-roles.md)), und wenn die vordefinierten Rollen nicht Ihren Bedürfnissen entsprechen können Sie auch eigene Rollen definieren.

>[AZURE.IMPORTANT] Beachten Sie, dass ein Benutzer hat Teilnehmer (RBAC) auf einen Schlüssel Depot Management kann sie selbst Berechtigungen zugreifen auf Daten festlegen wichtige Richtlinie, die auf Datenebene gesteuert. Daher ist es genau steuern, empfohlen 'Teilnehmer' Zugriff auf Ihre wichtigsten Depots, dass nur autorisierte Personen Zugriff auf und wichtige Depots, Schlüssel, Schlüssel und Zertifikate verwalten können.

## <a name="data-plane-access-control"></a>Zugriffskontrolle für Daten-Ebene

Datenebene Key Vault besteht aus Operationen, die Einfluss auf die Objekte in einem Tresor Schlüssel wie Schlüssel, Schlüssel und Zertifikate.  Dazu gehören Vorgänge wie erstellen Schlüssel importieren Update, Liste, Sicherung und Wiederherstellung Schlüssel kryptografischen Operationen wie, überprüfen Sie verschlüsseln, entschlüsseln, umschließen, entpacken und Tags und andere Attribute für Schlüssel festlegen. Ebenso für vertrauliche Daten, die sie enthält, get, set Liste, löschen.

Ebene Datenzugriff wird erteilt, indem wichtige Vault-Richtlinien. Benutzer, Gruppe oder eine Anwendung müssen Berechtigungen (RBAC) für Managementebene ein Schlüssel Depot Zugriffsrichtlinien für dieses wichtigsten Depot festgelegt werden. Benutzer, Gruppe oder Anwendung kann so bestimmte Vorgänge für Schlüssel oder Kennwörter in einem Schlüssel zugreifen. Key Vault unterstützt bis zu 16 Zugriff Richtlinieneinträge Key Vault. Eine Azure Active Directory-Sicherheitsgruppe erstellen und Hinzufügen von Benutzern zu dieser Gruppe mehrere Schlüssel Tresor Daten Ebene Zugriff gewähren.

### <a name="key-vault-access-policies"></a>schlüsseltresor-Richtlinien

Key Vault-Richtlinien Berechtigungen Schlüssel, Schlüssel und Zertifikate unabhängig. Beispielsweise kann einem Benutzerzugriff auf nur Schlüssel, aber keine Berechtigungen für vertrauliche Daten Ihnen. Zugriffsberechtigungen für Schlüssel oder Kennwörter oder Zertifikate sind jedoch Ebene Depot. Wichtige Richtlinie unterstützt also keine Berechtigungen auf Objektebene. [Azure-Portal](https://portal.azure.com/) [Azure CLI-Tools](../xplat-cli-install.md), [PowerShell](../powershell-install-configure.md)oder [Schlüssel Depot Management REST-APIs](https://msdn.microsoft.com/library/azure/mt620024.aspx) können Sie Richtlinien für Schlüssel Depot festgelegt.

>[AZURE.IMPORTANT] Beachten Sie, dass wichtige Vault-Richtlinien auf Vault. Wenn ein Benutzer über die Berechtigung zum Erstellen und Löschen von Schlüsseln gewährt wird, können sie diese Vorgänge auf alle Schlüssel in dieses wichtigsten Depot ausführen.

## <a name="example"></a>Beispiel

Angenommen, Sie entwickeln eine Anwendung, die ein Zertifikat für SSL, Azure-Speicher zum Speichern von Daten verwendet und RSA 2048-Bit-Schlüssel für Vorgänge Zeichen verwendet. Angenommen, diese Anwendung in einer VM (oder VM Skalierung festlegen) ausgeführt wird. Sie können ein Schlüssel Depot verwenden, um alle geheime Daten speichern und Schlüssel Depot bootstrap Zertifikat speichern, das von der Anwendung zur Authentifizierung mit Active Directory Azure verwenden.

So, hier ist eine Zusammenfassung aller Schlüssel und geheime Informationen in einem Schlüssel gespeichert werden.

- **SSL-Zertifikat** - SSL verwendet
- **Speicherschlüssel** - Konto zugreifen
- **RSA 2048-Bit-Schlüssel** - Zeichen-Operationen
- **Bootstrap-Zertifikat** - zur Authentifizierung in Azure Active Directory Zugriff auf wichtige Vault Speicherschlüssel abrufen und RSA-Schlüssel zum Signieren verwendet.

Jetzt treffen wir Menschen, die Verwaltung, Bereitstellung und Überwachung dieser Anwendung. In diesem Beispiel verwenden wir drei Rollen.

- **Sicherheitsteam** - in der Regel sind IT-Mitarbeiter von "Office von CSO (Chief Security Officer)" oder gleichwertig, verantwortlich für die korrekte Aufbewahrung Geheimnisse wie SSL-Zertifikate zum Signieren von Verbindungszeichenfolgen für Datenbanken, Speicherschlüssel Konto RSA-Schlüssel.
- **Entwickler-Operatoren** - sind die Leute, die diese Anwendung entwickeln und dann in Azure bereitstellen. Sie sind nicht Teil des Teams und damit sie sollte keinen Zugriff auf vertraulichen Daten, wie SSL-Zertifikate mit RSA-Schlüssel normalerweise die Anwendung, die sie bereitstellen müssen Zugang zu den.
- **Prüfer** - Dies ist normalerweise eine andere Gruppe von Menschen isoliert von Entwicklern und allgemeine IT-Mitarbeiter. Verantwortung ist auf Einhaltung der Datensicherheitsstandards und richtigen Verwendung und Wartung, Schlüssel usw. überprüfen. 

Eine weitere Aufgabe, die außerhalb dieser Anwendung aber entsprechende hier erwähnt werden, und die würde Abonnement (oder Gruppe) Administrator. Abonnement richtet anfängliche Zugriffsberechtigungen für das Team. Hier wird angenommen, dass der Abonnementadministrator an das Sicherheitsteam einer Ressourcengruppe, in der alle erforderlichen Ressourcen für diese Anwendung wohnen Zugriff hat.

Jetzt sehen Sie die einzelnen Rollen im Kontext der Anwendung führt Aktionen.

- **Sicherheitsteam**
    - Wichtige Depots erstellen
    - Schaltet auf vault Protokollierung
    - Schlüssel-Schlüssel hinzufügen
    - Sicherung der Schlüssel für die Wiederherstellung erstellen
    - Festlegen Sie wichtige Richtlinie Berechtigungen für Benutzer und Programme zur Ausführung bestimmter Vorgänge
    - Schlüssel-Kennwörter regelmäßig zurücksetzen
- **Entwickler-Operatoren**
    - Verweise auf bootstrap und SSL-Zertifikate (Fingerabdrücke) Speicher Key (Schlüssel URI) und Signaturschlüssel (Schlüssel URI) vom Sicherheitsteam zu erhalten
    - Entwickeln Sie und Bereitstellen Sie der Anwendung, die Schlüssel und geheime Daten programmgesteuert zugreift
- **Prüfer**
    - Überprüfung Verwendungsprotokolle Benutzung Schlüssel-Schlüssel und Einhaltung der Datensicherheitsstandards

Jetzt sehen, welche Zugriffsberechtigungen Schlüssel Tresor durch jede Rolle und die Anwendung ihre zugewiesenen Aufgaben erforderlich sind. 

| Benutzerrolle    | Flugzeug-Management-Berechtigungen | Von Ebene Berechtigungen |
|--------------|------------------------------|------------------------|
|Sicherheitsteam|Key Vault Teilnehmer|Schlüssel: backup, erstellen, löschen, abrufen, importieren, auflisten, Wiederherstellen <br> Kennwörter: alle|
|Entwickler-Operator| Key Vault Berechtigung bereitstellen, sodass sie bereitstellen VMs Geheimnisse aus dem Schlüssel Tresor abrufen kann | Keine |
|Prüfer| Keine | Schlüssel: Liste<br>Schlüssel: Liste|
|Anwendung| Keine | Schlüssel: Zeichen<br>Kennwörter: Abrufen |

>[AZURE.NOTE] Prüfer müssen Berechtigungen für Schlüssel und geheime Liste, so können sie Attribute und Kennwörter, die in den Protokollen wie Tags nicht ausgegebenen überprüfen Aktivierung und Ablaufdaten.

Neben der Berechtigung Schlüssel Tresor benötigen alle drei Rollen Zugriff auf andere Ressourcen. Z. B. VMs (oder Web Apps etc.) bereitstellen Entwickler-Operatoren benötigen auch 'Teilnehmer' Zugriff auf die Ressource. Prüfer benötigen Lesezugriff auf das Speicherkonto, Key Vault-Protokolle gespeichert werden.

Da der Schwerpunkt dieses Artikels auf den wichtigsten Tresor sichern, nur die relevanten Teile zu diesem Thema veranschaulichen und Überspringen der Details zum Bereitstellen von Zertifikaten, Schlüssel und geheime Daten programmgesteuert usw. zugreifen. Diese Details werden bereits an anderer Stelle behandelt. Zertifikate im Schlüssel Tresor VMs Bereitstellen in einen [Blogbeitrag](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/)abgedeckt und [Beispielcode](https://www.microsoft.com/download/details.aspx?id=45343) zur Verfügung, die das bootstrap Zertifikat authentifizieren Azure AD Schlüssel Tresor zugreifen veranschaulicht.

Die meisten Zugriffsberechtigungen mit Azure-Portal erteilt werden, aber um abgestufte Berechtigungen erteilen müssen Azure PowerShell (oder Azure CLI) um das gewünschte Ergebnis zu erzielen. 

Die folgenden PowerShell Ausschnitte angenommen:

- Azure Active Directory Administrator hat Sicherheitsgruppen erstellt, die drei Rollen: Contoso-Sicherheitsteam, Contoso App Devops Contoso App Auditors darstellen. Der Administrator hat auch Benutzer zu den Gruppen gehören sie.

- **ContosoAppRG** ist die Ressourcengruppe, in denen die Ressourcen befinden. **Contosologstorage** ist, wo die Protokolle gespeichert werden. 

- Key Vault **ContosoKeyVault** und Speicher Konto für Key Vault Protokolle **Contosologstorage** müssen am gleichen Azure


Zunächst weist dem Abonnementadministrator 'key Vault Teilnehmer' und ' Benutzer Zugriff ' Administratorrollen an das Sicherheitsteam. Dadurch wird das Sicherheitsteam verwalten Zugriff auf andere Ressourcen und wichtige Depots in der Ressourcengruppe ContosoAppRG.

```
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "key vault Contributor" -ResourceGroupName ContosoAppRG
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "User Access Administrator" -ResourceGroupName ContosoAppRG
```

Das folgende Skript zeigt, wie das Sicherheitsteam Schlüssel Tresor Setup-Protokollierung und Zugriffsberechtigungen für andere Rollen und die Anwendung erstellen kann. 

```
# Create key vault and enable logging
$sa = Get-AzureRmStorageAccount -ResourceGroup ContosoAppRG -Name contosologstorage
$kv = New-AzureRmKeyVault -VaultName ContosoKeyVault -ResourceGroup ContosoAppRG -SKU premium -Location 'westus' -EnabledForDeployment
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

# Data plane permissions for Security team
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -PermissionToKeys backup,create,delete,get,import,list,restore -PermissionToSecrets all

# Management plane permissions for Dev/ops
# Create a new role from an existing role
$devopsrole = Get-AzureRmRoleDefinition -Name "Virtual Machine Contributor"
$devopsrole.Id = $null
$devopsrole.Name = "Contoso App Devops"
$devopsrole.Description = "Can deploy VMs that need secrets from key vault"
$devlopsrole.AssignableScopes = @("/subscriptions/<SUBSCRIPTION-GUID>")

# Add permission for dev/ops so they can deploy VMs that have secrets deployed from key vaults
$devopsrole.Actions.Add("Microsoft.KeyVault/vaults/deploy/action")
New-AzureRmRoleDefinition -Role $role

# Assign this newly defined role to Dev ops security group
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Devops')[0].Id -RoleDefinitionName "Contoso App Devops" -ResourceGroupName ContosoAppRG

# Data plane permissions for Auditors
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Auditors')[0].Id -PermissionToKeys list -PermissionToSecrets list
```

Die benutzerdefinierte Rolle definiert, ist nur das Abonnement zugeordnet werden, in dem die Ressourcengruppe ContosoAppRG erstellt. Die gleichen benutzerdefinierten Rollen für andere Projekte in andere Abonnements verwendet werden, konnte der Bereich Abonnements hinzugefügt haben.

Die benutzerdefinierte Rolle Zuordnung der Entwickler Betreiber für die Berechtigung "/ Bereitstellungsvorgang" ist der Ressourcengruppe beschränkt. Auf diese Weise erstellten Ressourcengruppe 'ContosoAppRG' VMs erhalten Geheimnisse (SSL-Zertifikat und bootstrap Cert). Mitglied Dev-Ops in andere Ressourcengruppe erstellt VMs werden nicht zu dieser geheimen Schlüssel auch wüßten geheimen URIs.

Dieses Beispiel zeigt ein einfaches Szenario. Praxisnaher Szenarios möglicherweise komplexer und müssen Sie Berechtigungen für den Schlüssel Tresor Ihren Bedürfnissen entsprechend anpassen. In unserem Beispiel wir angenommen, Sicherheitsteam bieten Schlüssel und geheime Verweise (URIs und Fingerabdrücke) Team Entwicklern-Operatoren müssen in ihrer Anwendung verweisen. Daher müssen sie Entwickler-Operatoren Daten Ebene Zugriff gewähren. Beachten Sie, dass dieses Beispiel konzentriert sich auf den Schlüssel Tresor sichern. Wie berücksichtigen [die VMs](https://azure.microsoft.com/services/virtual-machines/security/), [Speicherkonten](../storage/storage-security-guide.md) und anderen Azure-Ressourcen zu sichern.

>[AZURE.NOTE] Hinweis: Dieses Beispiel zeigt, wie Vault Access wird in der Produktion gesperrt. Die Entwickler müssen ihre eigenen Abonnements oder Resourcegroup haben Vollzugriff auf ihre Depots, VMs und Speicherkonto, in dem sie die Anwendung entwickeln.


## <a name="resources"></a>Ressourcen

-   [Azure Active Directory rollenbasierte Zugriffskontrolle](../active-directory/role-based-access-control-configure.md)

    Dieser Artikel beschreibt die Azure Active Directory rollenbasierte Zugriffskontrolle und deren Funktionsweise.

-   [RBAC: Integrierte Rollen](../active-directory/role-based-access-built-in-roles.md)

    Dieser Artikel beschreibt alle integrierten Rollen in RBAC verfügbar.

-   [Grundlegendes zum Ressourcen-Manager und klassischen Bereitstellung](../resource-manager-deployment-model.md)

    Dieser Artikel beschreibt die Ressourcenmanager Bereitstellung und klassischen Bereitstellungsmodelle und erläutert die Vorteile der Verwendung von Gruppen Ressourcenmanager und Ressourcen

-    [Rollenbasierte Zugriffskontrolle Azure PowerShell verwalten](../active-directory/role-based-access-control-manage-access-powershell.md)

     Dieser Artikel erläutert die rollenbasierte Zugriffskontrolle Azure PowerShell verwalten

-   [Rollenbasierte Zugriffskontrolle mit der REST-API verwalten](../active-directory/role-based-access-control-manage-access-rest.md)

    Dieser Artikel beschreibt, wie Sie die REST-API RBAC verwalten.

-   [Rollenbasierte Zugriffskontrolle für Microsoft Azure aus entzünden](https://channel9.msdn.com/events/Ignite/2015/BRK2707)

    Dies ist eine Verknüpfung mit einem Video auf Channel 9 2015 MS entzünden Konferenz. Diese Sitzung reden über Management und reporting-Funktionen in Azure und best Practices Absicherung von Azure Abonnements Azure Active Directory durchsuchen.

-   [Autorisieren des Zugriffs auf Web Applications OAuth 2.0 und Azure Active Directory verwenden](../active-directory/active-directory-protocols-oauth-code.md)

    Dieser Artikel beschreibt vollständige OAuth 2.0 für Azure Active Directory authentifiziert.

-   [Key Vault Management REST-APIs](https://msdn.microsoft.com/library/azure/mt620024.aspx)

    Dieses Dokument ist die Referenz für die REST-APIs programmgesteuert zu den wichtigsten Tresor einschließlich Schlüssel Richtlinie festlegen.

-   [Key Vault REST-APIs](https://msdn.microsoft.com/library/azure/dn903609.aspx)

    Verknüpfen Sie mit Schlüssel Depot REST API-Referenzdokumentation.

-   [Schlüssel-Zugriffskontrolle](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_KeyAccessControl)

    Verknüpfen Sie mit Schlüssel Access Control Dokumentation.

-   [Geheime Zugriffskontrolle](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_SecretAccessControl)

    Verknüpfen Sie mit Schlüssel Access Control Dokumentation.

-   [Festlegen](https://msdn.microsoft.com/library/mt603625.aspx) und [Entfernen von](https://msdn.microsoft.com/library/mt619427.aspx) Schlüssel Richtlinie mithilfe von PowerShell

    Enthält Links zu Referenzdokumentation für PowerShell-Cmdlets Schlüssel Richtlinie verwalten.

## <a name="next-steps"></a>Nächste Schritte

Ein immer gestartet Administrator finden Sie unter [Erste Schritte mit Azure-Tresor](key-vault-get-started.md).

Weitere Informationen zur Verwendung des Key Vault Protokollierung finden Sie in der [Azure-Tresor anmelden](key-vault-logging.md).

Weitere Informationen über Schlüssel und geheime Schlüssel mit Azure Schlüssel finden Sie unter [über Schlüssel und geheime Schlüssel](https://msdn.microsoft.com/library/azure/dn903623.aspx).

Haben Sie Fragen zum Schlüssel Depot besuchen Sie [Azure Key Vault Foren](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)
