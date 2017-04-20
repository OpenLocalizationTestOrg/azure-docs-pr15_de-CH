<properties
    pageTitle="Azure Active Directory PowerShell Vorschau Cmdlets für die Verwaltung der in Azure AD | Microsoft Azure"
    description="Diese Seite enthält Beispiele, PowerShell Gruppen in Azure Active Directory verwalten"
    keywords="Azure AD Azure Active Directory PowerShell Gruppen, Verwaltung"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="curtand"/>

# <a name="azure-active-directory-preview-cmdlets-for-group-management"></a>Azure Active Directory Vorschau Cmdlets für die Verwaltung

> [AZURE.SELECTOR]
- [Azure-portal](active-directory-groups-create-azure-portal.md)
- [Azure-Verwaltungsportal](active-directory-accessmanagement-manage-groups.md)
- [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)

Das folgende Dokument erhalten Sie Beispiele für PowerShell zum Verwalten von Gruppen in Active Directory Azure (Azure AD) verwenden.  Darüber hinaus Informationen zum Azure AD PowerShell Vorschau Modul einrichten. Zunächst müssen Sie [den Azure AD PowerShell-Modul](http://go.microsoft.com/fwlink/p/?LinkId=828627).

## <a name="installing-the-azure-ad-powershell-module"></a>Azure AD PowerShell-Modul installieren

Um AzureAD PowerShell Vorschau Modul installieren, verwenden Sie die folgenden Befehle:

    PS C:\Windows\system32> install-module azureadpreview

Um sicherzustellen, dass das Vorschau-Modul installiert wurde, verwenden Sie den folgenden Befehl ein:

    PS C:\Windows\system32> get-module azureadpreview

    ModuleType Version    Name                                ExportedCommands
    ---------- -------    ----                                ----------------
    Binary     1.1.146.0  azureadpreview                      {Add-AzureADAdministrati...}

Jetzt können Sie mit den Cmdlets im Modul starten. Eine vollständige Beschreibung des Cmdlets im Modul AzureAD Vorschau finden Sie in der [Online-Dokumentation](https://msdn.microsoft.com/library/azure/mt757216.aspx).

## <a name="connecting-to-the-directory"></a>Verbinden zum Verzeichnis
Bevor Sie Gruppen mithilfe von Azure AD PowerShell Vorschau Cmdlets verwalten können, müssen Sie die PowerShell-Sitzung mit Verzeichnis verbinden zu verwalten. Verwenden Sie dazu den folgenden Befehl ein:

    PS C:\Windows\system32> Connect-AzureAD -Force

Das Cmdlet fordert Anmeldeinformationen Sie Ihr Verzeichnis zugreifen möchten. In diesem Beispiel verwenden wir karen@drumkit.onmicrosoft.com Zugriff auf das Verzeichnis Demo. Das Cmdlet wird eine Bestätigung angezeigt, die Sitzung zu Ihrem Verzeichnis hergestellt wurde zurückgegeben:

    Account                       Environment Tenant
    -------                       ----------- ------
    Karen@drumkit.onmicrosoft.com AzureCloud  85b5ff1e-0402-400c-9e3c-0f…

Sie können jetzt beginnen, mit AzureAD Vorschau Cmdlets zu Gruppen in Ihrem Verzeichnis.

## <a name="retrieving-groups"></a>Abrufen von Gruppen
Rufen Sie vorhandene Gruppen im Verzeichnis können Sie das Cmdlet "Get-AzureADGroups". Um alle Gruppen im Verzeichnis abzurufen, verwenden Sie das Cmdlet ohne Parameter:

    PS C:\Windows\system32> get-azureadgroup

Das Cmdlet gibt alle Gruppen im verbundenen Verzeichnis zurück.

Den ObjectID - Parameter können Sie eine bestimmte Gruppe abrufen für die Angabe der Gruppe ObjectID:

    PS C:\Windows\system32> get-azureadgroup -ObjectId e29bae11-4ac0-450c-bc37-6dae8f3da61b

Das Cmdlet wird jetzt die Gruppe zurückgegeben, deren ObjectID eingegebenen Wert des Parameters übereinstimmt:

    DeletionTimeStamp            :
    ObjectId                     : e29bae11-4ac0-450c-bc37-6dae8f3da61b
    ObjectType                   : Group
    Description                  :
    DirSyncEnabled               :
    DisplayName                  : Pacific NW Support
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 9bb4139b-60a1-434a-8c0d-7c1f8eee2df9
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Sie können Suchen für eine bestimmte Gruppe mithilfe der – Filterparameter. Dieser Parameter nimmt eine Filterklausel ODATA und gibt alle Gruppen, die Filter wie im folgenden Beispiel entsprechen:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Beachten Sie, dass die AzureAD-PowerShell-Cmdlets Implementieren des OData-Abfrage Standards Weitere Informationen finden Sie [hier](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).

## <a name="creating-groups"></a>Erstellen von Gruppen
Verwenden Sie zum Erstellen einer neuen Gruppe im Verzeichnis New-AzureADGroup-Cmdlet. Dieses Cmdlet erstellt eine neue Sicherheitsgruppe mit der Bezeichnung "Marketing":

    PS C:\Windows\system32> New-AzureADGroup -Description "Marketing" -DisplayName "Marketing" -MailEnabled $false -SecurityEnabled $true -MailNickName "Marketing"

## <a name="updating-groups"></a>Aktualisieren von Gruppen
Aktualisieren Sie eine vorhandene Gruppe verwenden Sie das Cmdlet "Set-AzureADGroup". In diesem Beispiel ändern wir die DisplayName-Eigenschaft der Gruppe "Administratoren Intune." Zunächst mit dem Cmdlet Get-AzureADGroup Gruppe finden und Filtern das DisplayName-Attribut:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Anschließend ändern wir die Description-Eigenschaft auf den neuen Wert "Intune Geräteadministratoren":

    PS C:\Windows\system32> Set-AzureADGroup -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -Description "Intune Device Administrators"

Wenn wir die Gruppe wieder finden, sehen wir werden die Description-Eigenschaft den neuen Wert wiederzugeben:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Device Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

## <a name="deleting-groups"></a>Löschen von Gruppen
Um Gruppen aus dem Verzeichnis löschen, verwenden Sie das Cmdlet "Remove-AzureADGroup" wie folgt:

    PS C:\Windows\system32> Remove-AzureADGroup -ObjectId b11ca53e-07cc-455d-9a89-1fe3ab24566b

## <a name="managing-members-of-groups"></a>Verwalten von Mitgliedern der Gruppen
Wenn Sie neue Mitglieder zu einer Gruppe hinzufügen möchten, verwenden Sie das Cmdlet AzureADGroupMember hinzufügen. Dieser Befehl fügt der Administratorengruppe Intune im vorherigen Beispiel verwendet:

    PS C:\Windows\system32> Add-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

ObjectId - Parameter ist die ObjectID Gruppe wir ein Mitglied hinzufügen möchten, und - RefObjectId ist die ObjectID des Benutzers zu der Gruppe hinzufügen.

Verwenden Sie zum Abrufen der Mitglieder einer Gruppe mit dem Cmdlet Get-AzureADGroupMember wie in diesem Beispiel:

    PS C:\Windows\system32> Get-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                        72cd4bbd-2594-40a2-935c-016f3cfeeeea User
                        8120cc36-64b4-4080-a9e8-23aa98e8b34f User

Um das Element zu entfernen, die wir zuvor hinzu, dem Cmdlet entfernen AzureADGroupMember, wie hier gezeigt:

    PS C:\Windows\system32> Remove-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -MemberId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Überprüfen Sie die Gruppenmitgliedschaften des Benutzers dem Cmdlet AzureADGroupIdsUserIsMemberOf auswählen. Dieses Cmdlet nimmt als Parameter ObjectId des Benutzers für die Gruppenmitgliedschaften überprüft und eine Liste der Gruppen, für die die Mitgliedschaft überprüft. Die Liste der Gruppen vorzusehen, in Form einer komplexen Variablen vom Typ "Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck" so wir zunächst eine Variable mit diesem Typ erstellen müssen:

    PS C:\Windows\system32> $g = new-object Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck

Als Nächstes geben Sie Werte für die GroupIds im Attribut "GroupIds" dieser komplexen Variablen überprüfen:

    PS C:\Windows\system32> $g.GroupIds = "b11ca53e-07cc-455d-9a89-1fe3ab24566b", "31f1ff6c-d48c-4f8a-b2e1-abca7fd399df"

Jetzt wollen wir die Gruppenmitgliedschaften des Benutzers mit ObjectID 72cd4bbd-2594-40a2-935c-016f3cfeeeea gegen die in $g überprüfen, verwenden wir:

    PS C:\Windows\system32> Select-AzureADGroupIdsUserIsMemberOf -ObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea -GroupIdsForMembershipCheck $g

    OdataMetadata                                                                                               Value
    -------------                                                                                               -----
    https://graph.windows.net/85b5ff1e-0402-400c-9e3c-0f9e965325d1/$metadata#Collection(Edm.String)             {31f1ff6c-d48c-4f8a-b2e1-abca7fd399df}


Der zurückgegebene Wert ist eine Liste der Gruppen, die dieser Benutzer angehört. Außerdem können Sie diese Methode, um Kontakte, Gruppen oder Service-Hauptbenutzer ist für eine bestimmte Liste von Gruppen mit AzureADGroupIdsContactIsMemberOf wählen, wählen AzureADGroupIdsGroupIsMemberOf oder wählen AzureADGroupIdsServicePrincipalIsMemberOf anwenden

## <a name="managing-owners-of-groups"></a>Besitzer von Gruppen verwalten
Um Besitzer einer Gruppe hinzuzufügen, verwenden Sie Add-AzureADGroupOwner-Cmdlet:

    PS C:\Windows\system32> Add-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

ObjectId - Parameter ist die ObjectID Gruppe wir Besitzer hinzufügen möchten, und - RefObjectId ist die ObjectID des Benutzers als Besitzer der Gruppe hinzufügen möchten.

Um den Besitzer einer Gruppe abzurufen, verwenden Sie Get-AzureADGroupOwner:

    PS C:\Windows\system32> Get-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

Das Cmdlet wird die Liste der Besitzer für die angegebene Gruppe zurückgegeben:

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                        e831b3fd-77c9-49c7-9fca-de43e109ef67 User

Wenn Sie einen Besitzer aus einer Gruppe entfernen möchten, verwenden Sie Remove-AzureADGroupOwner:

    PS C:\Windows\system32> remove-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -OwnerId e831b3fd-77c9-49c7-9fca-de43e109ef67

## <a name="next-steps"></a>Nächste Schritte

Weitere Azure Active Directory PowerShell-Dokumentation finden unter [Azure Active Directory Cmdlets](http://go.microsoft.com/fwlink/p/?LinkId=808260).

* [Verwalten des Zugriffs auf Ressourcen mit Azure Active Directory-Gruppen](active-directory-manage-groups.md)

* [Integration von lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)
