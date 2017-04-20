
<properties
    pageTitle="Verwalten von Ressourcen mit der Azure-CLI | Microsoft Azure"
    description="Verwenden der Azure-Befehlszeilenschnittstelle (CLI) Azure Ressourcen und Gruppen verwalten"
    editor=""
    manager="timlt"
    documentationCenter=""
    authors="dlepow"
    services="azure-resource-manager"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="danlep"/>

# <a name="use-the-azure-cli-to-manage-azure-resources-and-resource-groups"></a>Verwenden der Azure-CLI Azure Ressourcen und Ressourcengruppen verwalten


> [AZURE.SELECTOR]
- [Portal](azure-portal/resource-group-portal.md) 
- [Azure CLI](xplat-cli-azure-resource-manager.md)
- [Azure PowerShell](powershell-azure-resource-manager.md)
- [REST-API](resource-manager-rest-api.md)


Der Azure-Befehlszeilenschnittstelle (CLI Azure) ist eines der Tools, die Sie zum Bereitstellen und Verwalten von Ressourcen mit dem Ressourcen-Manager verwenden können. Dieser Artikel stellt allgemeine Verfahren zum Verwalten von Azure Ressourcen und Ressourcengruppen mit der Azure-CLI im Ressourcen-Manager-Modus. Informationen über die CLI Ressourcen finden Sie unter [Ressourcen mit Ressourcen-Manager und CLI Azure bereitstellen](resource-group-template-deploy-cli.md). Hintergrundinformationen zur Azure-Ressourcen und Ressourcen-Manager finden Sie auf der [Azure-Ressourcen-Manager (Übersicht)](azure-resource-manager/resource-group-overview.md).

>[AZURE.NOTE] Azure-Ressourcen mit der Azure-CLI verwalten müssen [der Azure-CLI installieren](xplat-cli-install.md)und [Azure anmelden](xplat-cli-connect.md) mithilfe der `azure login` Befehl. Sicherzustellen, dass die CLI im Ressourcen-Manager-Modus (ausführen `azure config mode arm`). Wenn Sie diese Dinge getan haben, können Sie gehen.



## <a name="get-resource-groups-and-resources"></a>Ressourcengruppen und Ressourcen

### <a name="resource-groups"></a>Ressourcengruppen

Führen Sie diesen Befehl, um eine Liste aller Ressourcengruppen in Ihr Abonnement und ihre Standorte.

    azure group list
    

### <a name="resources"></a>Ressourcen
 Verwenden Sie den folgenden Befehl, um alle Ressourcen in einer Gruppe mit Namen *TestRG*Liste.

    azure resource list testRG

Zum Anzeigen einer einzelnen Ressource in der Gruppe wie ein virtueller Computer mit dem Namen *MyUbuntuVM*einen Befehl wie folgt.

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"
    
Beachten Sie den **Microsoft.Compute/virtualMachines** -Parameter. Dieser Parameter gibt den Typ der Ressource, der Sie Informationen wünschen.
    
>[AZURE.NOTE]Bei **Azure Ressource** Befehle als den Befehl **Liste** verwenden, müssen Sie die API-Version der Ressource mit dem Parameter **-o** angeben. Wenn Sie die API-Version unsicher sind, finden Sie in der Vorlage und finden Sie Feld Anforderungsparameter für die Ressource zu. Weitere Informationen zu Versionen der Ressourcen-Manager-API finden Sie unter [Ressourcen-Manager-Anbieter, Regionen, API-Versionen und Schemas](resource-manager-supported-services.md).

Beim Anzeigen der Details für eine Ressource ist es häufig sinnvoll, verwenden die `--json` Parameter. Dieser Parameter macht die Ausgabe besser lesbar, weil einige Werte geschachtelten Strukturen oder Sammlungen. Das folgende Beispiel veranschaulicht die Ergebnisse des Befehls **Anzeigen** als JSON-Dokument zurückgegeben.

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json

>[AZURE.NOTE] Gegebenenfalls können Sie die JSON-Daten in Datei speichern, mit der &gt; Zeichen die Ausgabe in eine Datei umleiten. Zum Beispiel:
>
> `azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json > myfile.json`

### <a name="tags"></a>Tags

[AZURE.INCLUDE [resource-manager-tag-resources-cli](../includes/resource-manager-tag-resources-cli.md)]

## <a name="manage-resources"></a>Verwalten von Ressourcen


Um eine Ressource wie Speicher einer Ressourcengruppe hinzuzufügen, führen Sie einen Befehl:

    azure resource create testRG MyStorageAccount "Microsoft.Storage/storageAccounts" "westus" -o "2015-06-15" -p "{\"accountType\": \"Standard_LRS\"}"
    
Neben die API-Version der Ressource mit dem Parameter **-o** angeben, mithilfe des Parameters **-p** eine JSON-formatierte Zeichenfolge erforderlichen oder zusätzlichen Eigenschaften übergeben.
    
    
Um eine vorhandene Ressource wie eine VM-Ressource zu löschen, verwenden Sie einen Befehl wie den folgenden.

    azure resource delete testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

Um vorhandene Ressourcen in einer anderen Ressourcengruppe oder Abonnement verschieben, Befehl **Azure Ressource verschieben** . Im folgenden Beispiel wird veranschaulicht, wie Redis Cache eine neue Ressourcengruppe verschieben. Der Parameter **-i** bieten Sie eine durch Kommas getrennte Liste der Ressourcen-Id zu.


    azure resource move -i "/subscriptions/{guid}/resourceGroups/OldRG/providers/Microsoft.Cache/Redis/examplecache" -d "NewRG"

## <a name="control-access-to-resources"></a>Steuern des Zugriffs auf Ressourcen

Verwenden der Azure-CLI erstellen und Verwalten von Richtlinien zur Steuerung des Zugriffs auf Azure Ressourcen. Hintergrundinformationen zu Richtliniendefinitionen und Zuweisen von Richtlinien zu Ressourcen finden Sie unter [verwenden Gruppenrichtlinien zum Verwalten von Ressourcen und Zugriffskontrolle](resource-manager-policy.md).

Beispielsweise alle verweigern Speicherort nicht Westen der USA oder US North Central ist die folgende Richtlinie definieren und Policy Definition Datei policy.json speichern:

    {
    "if" : {
        "not" : {
        "field" : "location",
        "in" : ["westus" ,  "northcentralus"]
        }
    },
    "then" : {
        "effect" : "deny"
    }
    }

Führen Sie den Befehl **Richtliniendefinition erstellen** :

    azure policy definition create MyPolicy -p c:\temp\policy.json
    
Dieser Befehl zeigt die Ausgabe ähnelt der folgenden.

    + Richtliniendefinition MeineRichtlinie Daten erstellen: Richtlinienname: MeineRichtlinie Daten: PolicyDefinitionId: /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy

    Daten: PolicyType: benutzerdefinierten Daten: DisplayName: undefined Daten: Beschreibung: undefined Daten: PolicyRule: Feld = Speicherort, in = [Westus Northcentralus], Effekt = verweigern

 Eine Richtlinie im Bereich zuweisen, verwenden Sie die **PolicyDefinitionId** des vorherigen Befehls zurückgegeben werden soll. Im folgenden Beispiel dieses Abonnement ist, jedoch können Sie einzelne Ressourcen und Ressourcengruppen festlegen:

    Zuweisung von Azure Sicherheitsrichtlinien erstellen MyPolicyAssignment -p /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy -s /subscriptions/###-###-###-###-### /

Sie können abrufen, ändern oder Entfernen von Richtliniendefinitionen Befehle **Richtliniendefinition anzeigen**, **Policy-Definition**und **Policy-Definition löschen** .

Ebenso können Sie abrufen, ändern oder entfernen zugewiesenen Richtlinien mithilfe der Befehle **richtlinienzuweisung anzeigen**, **Zuweisung von Sicherheitsrichtlinien**und **Richtlinie löschen** .


## <a name="export-a-resource-group-as-a-template"></a>Eine Ressourcengruppe als Vorlage exportieren

Für eine vorhandene Ressourcengruppe Ressourcen-Manager-Vorlage für die Ressourcengruppe angezeigt. Exportieren der Vorlage bietet zwei Vorteile:

1. Zukünftige Bereitstellung der Lösung kann problemlos automatisieren, da die Infrastruktur in der Vorlage definiert ist.

2. Sie können mit vertraut Vorlagensyntax JSON, die Ihre Lösung ansehen.

Azure-CLI verwenden, exportieren Sie eine Vorlage, die den aktuellen Zustand der Ressourcengruppe darstellt, oder downloaden Sie die Vorlage, die für eine bestimmte Bereitstellung verwendet wurde.

* **Exportieren Sie die Vorlage für eine Ressourcengruppe** - Dies ist hilfreich, wenn an einer Ressourcengruppe Änderungen und die JSON-Repräsentation der aktuellen Zustand abgerufen werden müssen. Die generierte Vorlage enthält jedoch nur eine minimale Anzahl von Parametern und keine Variablen. Die meisten Werte in der Vorlage sind hartcodiert. Bevor der erstellten Vorlage bereitstellen, können Sie die Werte in Parameter konvertieren, damit die Bereitstellung für andere Umgebung anpassen können.

    Um die Vorlage für eine Ressourcengruppe in einem lokalen Verzeichnis zu exportieren, führen Sie die `azure group export` wie im folgenden Beispiel gezeigt. (Ersetzen Sie ein lokales Verzeichnis für Ihr Betriebssystem.)

        azure group export testRG ~/azure/templates/

* **Downloaden Sie die Vorlage für eine bestimmte Bereitstellung** – Dies ist hilfreich, wenn müssen Sie die aktuelle Vorlage anzuzeigen, die zur Bereitstellung von Ressourcen. Die Vorlage enthält alle Parameter und Variablen für die ursprüngliche Bereitstellung definiert. Wenn jemand in Ihrer Organisation die Ressourcengruppe außerhalb der Definition in der Vorlage geändert haben, wird diese Vorlage den aktuellen Zustand der Ressource darstellen.

    Vorlage für eine bestimmte Bereitstellung in einem lokalen Verzeichnis herunterladen, führen die `azure group deployment template download` Befehl. Zum Beispiel:

        azure group deployment template download TestRG testRGDeploy ~/azure/templates/downloads/
 
>[AZURE.NOTE] Vorlage exportieren ist in der Vorschau und nicht alle Ressourcentypen derzeit unterstützt eine Vorlage exportieren. Beim Exportieren einer Vorlage können Sie eine Fehlermeldung, die besagt, dass einige Ressourcen nicht exportiert wurden. Bei Bedarf manuell definieren Sie diese Ressourcen in die Vorlage nach dem herunterladen.



## <a name="next-steps"></a>Nächste Schritte

* Details des Bereitstellungsvorgänge Bereitstellung zur Fehlerbehebung mit der Azure-CLI und anzeigen Sie [Bereitstellungsvorgänge mit Azure CLI Ansicht](resource-manager-troubleshoot-deployments-cli.md)
* Die CLI verwendet eine Anwendung oder Skript auf Ressourcen zugreifen, finden Sie unter [Verwenden von Azure CLI Dienstprinzipal Zugriff auf Ressourcen zu](resource-group-authenticate-service-principal-cli.md).


