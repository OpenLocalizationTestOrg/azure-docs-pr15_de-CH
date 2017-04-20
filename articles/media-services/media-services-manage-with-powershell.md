<properties 
    pageTitle="Konten Sie Azure Media Services mit PowerShell" 
    description="Informationen Sie zum Verwalten von Azure Media Services Konten mit PowerShell-Cmdlets." 
    authors="Juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016"
    ms.author="juliako"/>


#<a name="manage-azure-media-services-accounts-with-powershell"></a>Konten Sie Azure Media Services mit PowerShell

> [AZURE.SELECTOR]
- [Portal](media-services-portal-create-account.md)
- [PowerShell](media-services-manage-with-powershell.md)
- [REST](http://msdn.microsoft.com/library/azure/dn194267.aspx)

> [AZURE.NOTE] Um ein Azure Media Services Account erstellen können, müssen Sie ein Azure-Konto. Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Weitere Informationen finden Sie unter <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A8A8397B5" target="_blank">Azure-Testversion</a>.

##<a name="overview"></a>Übersicht 

Dieser Artikel listet die Azure PowerShell-Cmdlets für Azure Media Services (AMS) in Azure-Ressourcen-Manager-Framework. Die Cmdlets vorhanden im **Microsoft.Azure.Commands.Media** -Namespace.

## <a name="versions"></a>Versionen

**Anforderungsparameter**: "2015-10-01"
               

## <a name="new-azurermmediaservice"></a>Neue AzureRmMediaService

Erstellt einen Media-Dienst.

### <a name="syntax"></a>Syntax

Parameter festlegen: StorageAccountIdParamSet

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccountId] <string> [-Tags <hashtable>]  [<CommonParameters>]

Parameter festlegen: StorageAccountsParamSet

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccounts] <PSStorageAccount[]> [-Tags <hashtable>]  [<CommonParameters>]

### <a name="parameters"></a>Parameter

**ResourceGroupName - &lt;Zeichenfolge&gt;**

Gibt den Namen der Ressourcengruppe Media-Dienst gehört.

Aliase | keine
---|---
Erforderlich?   |  true
Positionieren?   |  0
Standardwert |keine
Annehmen Pipeline-Eingabe? |True(ByPropertyName)
Annehmen Platzhalterzeichen?  |falsch

**-Kontoname &lt;Zeichenfolge&gt;**

Gibt den Namen des mediaservice.

Aliase |Name
---|---
Erforderlich? |true
Positionieren? |1
Standardwert |keine
Annehmen Pipeline-Eingabe? |falsch
Annehmen Platzhalterzeichen? |falsch

**-Standort &lt;Zeichenfolge&gt;**

Gibt dem Ort des mediaservice.

Aliase |keine
---|---
Erforderlich? |true
Positionieren? |2
Standardwert  |keine
Annehmen Pipeline-Eingabe? |True(ByPropertyName)
Annehmen Platzhalterzeichen? |falsch

**StorageAccountId - &lt;Zeichenfolge&gt;**

Gibt einen primären Speicher an, die mit mediaservice.

- Neue Speicherkonto (erstellt mit der Ressourcen-Manager-API) unterstützt nur.

- Das Speicherkonto muss vorhanden und hat die gleichen Media Service.

Aliase |keine
---|---
Erforderlich? |true
Positionieren? |3
Standardwert  |keine
Annehmen Pipeline-Eingabe? |True(ByPropertyName)
Parameternamen festlegen |StorageAccountIdParamSet
Annehmen Platzhalterzeichen?|falsch

**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**

Gibt Speicherkonten mediaservice zugeordnet.

- Neue Speicherkonto (erstellt mit der Ressourcen-Manager-API) unterstützt nur.

- Das Speicherkonto muss vorhanden und hat die gleichen Media Service.

- Nur ein Speicherkonto kann als primäre angegeben werden.

Aliase |keine
---|---
Erforderlich?  |true
Positionieren?  |3
Standardwert |keine
Annehmen Pipeline-Eingabe? |True(ByPropertyName)
Parameternamen festlegen |StorageAccountsParamSet
Annehmen Platzhalterzeichen? |falsch

**-Tags &lt;Hashtabelle&gt;**

Gibt eine Hashtabelle Tags mediaservice zugeordnet sind.

- Beispiel:@{"tag1"="value1";"tag2"=:value2"}

Aliase |keine
---|---
Erforderlich?  |falsch
Positionieren?  |mit dem Namen
Standardwert |keine
Annehmen Pipeline-Eingabe? |falsch
Annehmen Platzhalterzeichen? |falsch

**&lt;Befehlsparameter&gt;**

Dieses Cmdlet unterstützt die allgemeinen Parameter:-Debug - ErrorAction, ErrorVariable-, - InformationAction - InformationVariable-OutVariable,-OutBuffer, - PipelineVariable - ausführliche - WarningAction, und -WarningVariable.

### <a name="inputs"></a>Eingaben

Der Eingabetyp ist der Typ der Objekte, die an das Cmdlet pipe kann.

### <a name="outputs"></a>Ausgaben

Der Ausgabetyp ist der Typ der Objekte, die das Cmdlet ausgibt.

## <a name="set-azurermmediaservice"></a>AzureRmMediaService festlegen

Media-Dienst aktualisiert.

### <a name="syntax"></a>Syntax

    Set-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Tags <hashtable>] [-StorageAccounts <PSStorageAccount[]>]  [<CommonParameters>]

### <a name="parameters"></a>Parameter

**ResourceGroupName - &lt;Zeichenfolge&gt;**

Gibt den Namen der Ressourcengruppe Media-Dienst gehört.

Aliase |keine
---|---
Erforderlich?  |true
Positionieren?  |0
Standardwert |keine
Annehmen Pipeline-Eingabe? |True(ByPropertyName)
Annehmen Platzhalterzeichen? |falsch

**-Kontoname &lt;Zeichenfolge&gt;**

Gibt den Namen des mediaservice.

Aliase |Name
---|---
Erforderlich? |True
Positionieren? |1
Standardwert |Keine
Annehmen Pipeline-Eingabe? |True(ByPropertyName)
Annehmen Platzhalterzeichen? |False

**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**

Gibt Speicherkonten mediaservice zugeordnet.

- Neue Speicherkonto (erstellt mit der Ressourcen-Manager-API) unterstützt nur.

- Das Speicherkonto muss vorhanden und hat die gleichen Media Service.

- Nur ein Speicherkonto kann als primäre angegeben werden.

Aliase |keine
---|---
Erforderlich? |falsch
Positionieren? |Mit dem Namen
Standardwert |keine
Annehmen Pipeline-Eingabe? |True(ByPropertyName)
Parameternamen festlegen |StorageAccountsParamSet
Annehmen Platzhalterzeichen? |falsch

**-Tags &lt;Hashtabelle&gt;**

Gibt eine Hash-Tabelle der Tags, die diesem Mediendienst zugeordnet werden.

- Die mediaservice Tags werden vom Kunden angegebenen Wert ersetzt.

Aliase |keine
---|---
Erforderlich? |False
Positionieren?  |Mit dem Namen
Standardwert |Keine
Annehmen Pipeline-Eingabe? |True(ByPropertyName)
Annehmen Platzhalterzeichen? |falsch

**&lt;Befehlsparameter&gt;**

Dieses Cmdlet unterstützt die allgemeinen Parameter:-Debug - ErrorAction, ErrorVariable-, - InformationAction - InformationVariable-OutVariable,-OutBuffer, - PipelineVariable - ausführliche - WarningAction, und -WarningVariable.

### <a name="inputs"></a>Eingaben

Der Eingabetyp ist der Typ der Objekte, die an das Cmdlet pipe kann.

### <a name="outputs"></a>Ausgaben

Der Ausgabetyp ist der Typ der Objekte, die das Cmdlet ausgibt.

## <a name="remove-azurermmediaservice"></a>AzureRmMediaService entfernen

Entfernt einen Media-Dienst.

### <a name="syntax"></a>Syntax

    Remove-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parameter

**ResourceGroupName - &lt;Zeichenfolge&gt;**

Gibt den Namen der Ressourcengruppe Media-Dienst gehört.

Aliase |keine
---|---
Erforderlich? |true
Positionieren? |0
Standardwert |keine
Annehmen Pipeline-Eingabe? |True(ByPropertyName)
Annehmen Platzhalterzeichen? |falsch

**-Kontoname &lt;Zeichenfolge&gt;**

Gibt den Namen des mediaservice.

Aliase |keine
---|---
Erforderlich? |true
Positionieren? |2
Standardwert |Keine
Annehmen Pipeline-Eingabe?  |True(ByPropertyName)
Annehmen Platzhalterzeichen? |False

**&lt;Befehlsparameter&gt;**

Dieses Cmdlet unterstützt die allgemeinen Parameter:-Debug - ErrorAction, ErrorVariable-, - InformationAction - InformationVariable-OutVariable,-OutBuffer, - PipelineVariable - ausführliche - WarningAction, und -WarningVariable.

### <a name="inputs"></a>Eingaben

Der Eingabetyp ist der Typ der Objekte, die an das Cmdlet pipe kann.

### <a name="outputs"></a>Ausgaben

Der Ausgabetyp ist der Typ der Objekte, die das Cmdlet ausgibt.

## <a name="get-azurermmediaservice"></a>AzureRmMediaService abrufen

Ruft alle Media-Dienste eine Ressourcengruppe oder eine Media Service mit einem angegebenen Namen ab.

### <a name="syntax"></a>Syntax

Parametersatz: ResourceGroupParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string>  [<CommonParameters>] 

Parametersatz: AccountNameParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parameter

**ResourceGroupName - &lt;Zeichenfolge&gt;**

Gibt den Namen der Ressourcengruppe Media-Dienst gehört.

Aliase |keine
---|---
Erforderlich? |true
Positionieren?  |0
Standardwert |keine
Annehmen Pipeline-Eingabe? |True(ByPropertyName)
Parameternamen festlegen |ResourceGroupParameterSet AccountNameParameterSet
Annehmen Platzhalterzeichen?   falsch

**-Kontoname &lt;Zeichenfolge&gt;**

Gibt den Namen des mediaservice.

Aliase |keine
---|---
Erforderlich? |true
Positionieren?  |1
Standardwert |keine
Annehmen Pipeline-Eingabe? |True(ByPropertyName)
Parameternamen festlegen  |AccountNameParameterSet
Annehmen Platzhalterzeichen? |falsch

**&lt;Befehlsparameter&gt;**

Dieses Cmdlet unterstützt die allgemeinen Parameter:-Debug - ErrorAction, ErrorVariable-, - InformationAction - InformationVariable-OutVariable,-OutBuffer, - PipelineVariable - ausführliche - WarningAction, und -WarningVariable.

### <a name="inputs"></a>Eingaben

Der Eingabetyp ist der Typ der Objekte, die an das Cmdlet pipe kann.

### <a name="outputs"></a>Ausgaben

Der Ausgabetyp ist der Typ der Objekte, die das Cmdlet ausgibt.

## <a name="get-azurermmediaservicekeys"></a>AzureRmMediaServiceKeys abrufen

Ruft einen Media Service ab.

### <a name="syntax"></a>Syntax

    Get-AzureRmMediaServiceKeys [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parameter

**ResourceGroupName - &lt;Zeichenfolge&gt;**

Gibt den Namen der Ressourcengruppe Media-Dienst gehört.

Aliase |keine
---|---
Erforderlich? |true
Positionieren?  |0
Standardwert |keine
Annehmen Pipeline-Eingabe? |True(ByPropertyName)
Annehmen Platzhalterzeichen? |falsch

**-Kontoname &lt;Zeichenfolge&gt;**

Gibt den Namen des mediaservice.

Aliase |keine
---|---
Erforderlich? |true
Positionieren? |1
Standardwert |keine
Annehmen Pipeline-Eingabe? |True(ByPropertyName)
Annehmen Platzhalterzeichen? |falsch

**&lt;Befehlsparameter&gt;**

Dieses Cmdlet unterstützt die allgemeinen Parameter:-Debug - ErrorAction, ErrorVariable-, - InformationAction - InformationVariable-OutVariable,-OutBuffer, - PipelineVariable - ausführliche - WarningAction, und -WarningVariable.

### <a name="inputs"></a>Eingaben

Der Eingabetyp ist der Typ der Objekte, die an das Cmdlet pipe kann.

### <a name="outputs"></a>Ausgaben

Der Ausgabetyp ist der Typ der Objekte, die das Cmdlet ausgibt.

## <a name="set-azurermmediaservicekey"></a>AzureRmMediaServiceKey festlegen

Erstellt eine primäre oder sekundäre Taste Media-Dienst neu.

### <a name="syntax"></a>Syntax

    Set-AzureRmMediaServiceKey [-ResourceGroupName] <string> [-AccountName] <string> [-KeyType] <KeyType> {Primary | Secondary}  [<CommonParameters>]

### <a name="parameters"></a>Parameter

**ResourceGroupName - &lt;Zeichenfolge&gt;**

Gibt den Namen der Ressourcengruppe Media-Dienst gehört.

Aliase |keine
---|---
Erforderlich?  |true
Positionieren?  |0
Standardwert |keine
Annehmen Pipeline-Eingabe?  |True(ByPropertyName)
Annehmen Platzhalterzeichen? |falsch

**-Kontoname &lt;Zeichenfolge&gt;**

Gibt den Namen des mediaservice.

Aliase |keine
---|---
Erforderlich? |true
Positionieren?  |1
Standardwert |keine
Annehmen Pipeline-Eingabe?   |True(ByPropertyName)
Annehmen Platzhalterzeichen? |falsch

**KeyType - &lt;Schlüsseltyp&gt;**

Gibt den Schlüsseltyp des mediaservice an.

- Primäre oder sekundäre

Aliase |keine
---|---
Erforderlich?  |true
Positionieren?  |2
Standardwert |keine
Annehmen Pipeline-Eingabe? |falsch
Annehmen Platzhalterzeichen? |falsch

**&lt;Befehlsparameter&gt;**

Dieses Cmdlet unterstützt die allgemeinen Parameter:-Debug - ErrorAction, ErrorVariable-, - InformationAction - InformationVariable-OutVariable,-OutBuffer, - PipelineVariable - ausführliche - WarningAction, und -WarningVariable.

### <a name="inputs"></a>Eingaben

Der Eingabetyp ist der Typ der Objekte, die an das Cmdlet pipe kann.

### <a name="outputs"></a>Ausgaben

Der Ausgabetyp ist der Typ der Objekte, die das Cmdlet ausgibt.

## <a name="sync-azurermmediaservicestoragekeys"></a>AzureRmMediaServiceStorageKeys synchronisieren

Synchronisiert die speicherkontoschlüssel für ein Speicherkonto mediaservice zugeordnet.

### <a name="syntax"></a>Syntax

    Sync-AzureRmMediaServiceStorageKeys [-ResourceGroupName] <string> [-MediaServiceAccountName] <string>    [-StorageAccountId] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parameter

**ResourceGroupName - &lt;Zeichenfolge&gt;**

Gibt den Namen der Ressourcengruppe Media-Dienst gehört.

Aliase |keine
---|---
Erforderlich? |true
Positionieren? |0
Standardwert |keine
Annehmen Pipeline-Eingabe? |True(ByPropertyName)
Annehmen Platzhalterzeichen? |falsch

**-Kontoname &lt;Zeichenfolge&gt;**

Gibt den Namen des mediaservice.

Aliase |keine
---|---
Erforderlich? |true
Positionieren? |1
Standardwert |keine
Annehmen Pipeline-Eingabe? |True(ByPropertyName)
Annehmen Platzhalterzeichen? |falsch

**StorageAccountId - &lt;Zeichenfolge&gt;**

Gibt das Speicherkonto mediaservice zugeordnet.

Aliase |ID
---|---
Erforderlich? |true
Positionieren?  |2
Standardwert |keine
Annehmen Pipeline-Eingabe? |      True(ByPropertyName)
Annehmen Platzhalterzeichen? |falsch

**&lt;Befehlsparameter&gt;**

Dieses Cmdlet unterstützt die allgemeinen Parameter:-Debug - ErrorAction, ErrorVariable-, - InformationAction - InformationVariable-OutVariable,-OutBuffer, - PipelineVariable - ausführliche - WarningAction, und -WarningVariable.

### <a name="inputs"></a>Eingaben

Der Eingabetyp ist der Typ der Objekte, die an das Cmdlet pipe kann.

### <a name="outputs"></a>Ausgaben

Der Ausgabetyp ist der Typ der Objekte, die das Cmdlet ausgibt.

## <a name="next-step"></a>Nächstes 

Lernpfade Media Services anzeigen

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

 
