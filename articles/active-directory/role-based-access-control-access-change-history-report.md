<properties
    pageTitle="Erstellen einer Access Änderungshistorie-Berichte | Microsoft Azure"
    description="Generieren Sie Bericht, der Listet alle Änderungen auf Ihre Azure-Abonnements mit Role-Based Access Control in den vergangenen 90 Tagen."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/03/2016"
    ms.author="kgremban"/>

# <a name="create-an-access-change-history-report"></a>Erstellen einer Access Änderungshistorie-Berichte

Jede Person erteilt oder widerrufen innerhalb Ihrer Abonnements erhalten Änderungen in Azure Ereignisse protokolliert. Sie können Access ändern Verlaufsberichte alle Änderungen in den letzten 90 Tagen erstellen.

## <a name="create-a-report-with-azure-powershell"></a>Erstellen eines Berichts mit Azure PowerShell
Verwenden Sie zum Erstellen einer Access Änderungshistorie-Berichte in PowerShell die `Get-AzureRMAuthorizationChangeLog` Befehl. Ausführliche Informationen zu diesen Cmdlets sind in [PowerShell Gallery](https://www.powershellgallery.com/packages/AzureRM.Storage/1.0.6/Content/ResourceManagerStartup.ps1).

Wenn Sie diesen Befehl aufrufen, können Sie die Eigenschaft die Zuweisung angeben, einschließlich der folgenden Liste werden soll:

| Eigenschaft | Beschreibung |
| -------- | ----------- |
| **Aktion** | Gibt an, ob der Zugriff erteilt oder aufgehoben wurde |
| **Anrufer** | Der Besitzer verantwortlich für Zugriff ändern |
| **Datum** | Datum und Uhrzeit Zugriff geändert wurde |
| **Verzeichnisname** | Azure Active Directory-Verzeichnis |
| **PrincipalName** | Der Name des Benutzer, Gruppe oder Anwendung |
| **PrincipalType** | Ob die Zuordnung für einen Benutzer, Gruppe oder Anwendung wurde |
| **RoleId** | Die GUID der Rolle, die erteilt oder aufgehoben wurde |
| **Rollenname** | Die Rolle, die erteilt oder aufgehoben wurde |
| **Bereichsname** | Den Namen des Abonnements, Ressourcengruppe oder Ressource |
| **ScopeType** | Ob auf Abonnement, Ressourcengruppe oder Ressourcenbereich war |
| **SubscriptionId** | Die GUID der Azure-Abonnement |
| **SubscriptionName** | Der Name des Azure-Abonnements |

Dieses Beispielbefehl Listet alle Änderungen im Abonnement für die letzten sieben Tage:

```
Get-AzureRMAuthorizationChangeLog -StartTime ([DateTime]::Now - [TimeSpan]::FromDays(7)) | FT Caller,Action,RoleName,PrincipalType,PrincipalName,ScopeType,ScopeName
```

![PowerShell Get-AzureRMAuthorizationChangeLog - screenshot](./media/role-based-access-control-configure/access-change-history.png)

## <a name="create-a-report-with-azure-cli"></a>Erstellen eines Berichts mit Azure-CLI
Verwenden Sie zum Erstellen einer Access Änderungshistorie-Berichte in der Azure-Befehlszeilenschnittstelle (CLI) der `azure role assignment changelog list` Befehl.

## <a name="export-to-a-spreadsheet"></a>In eine Kalkulationstabelle exportieren
Speichern Sie den Bericht oder die Daten bearbeiten, die Änderungen in eine CSV-Datei exportieren. In einer Kalkulationstabelle zur Überprüfung können Sie dann den Bericht anzeigen.

![Changelog als Kalkulationstabelle - screenshot](./media/role-based-access-control-configure/change-history-spreadsheet.png)

## <a name="see-also"></a>Siehe auch
- Erste Schritte mit [Azure Role-Based-Zugriffskontrolle](role-based-access-control-configure.md)
- Arbeiten Sie mit [benutzerdefinierten Rollen in Azure RBAC](role-based-access-control-custom-roles.md)
