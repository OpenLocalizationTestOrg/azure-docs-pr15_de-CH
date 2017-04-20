<properties
   pageTitle="Empfohlene Namenskonventionen für Azure-Ressourcen | Hinweise | Microsoft Azure"
   description="Empfohlene Namenskonventionen für Azure-Ressourcen. Virtuelle Computer, Speicherkonten, Netzwerke, virtuelle Netzwerke, Subnetze und Handelsnamen Azure benennen"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/27/2016"
   ms.author="christb"/>
   
# <a name="recommended-naming-conventions-for-azure-resources"></a>Empfohlene Namenskonventionen für Azure-Ressourcen

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Die Auswahl eines Namens für eine Ressource in Microsoft Azure ist wichtig da:

- Es ist schwierig, einen Namen später ändern.
- Namen müssen die Anforderungen ihrer spezifischen Ressourcentyp aus.

Konsistente Benennungskonventionen verbessern Ressourcen suchen. Sie können auch die Rolle einer Ressource in einer Projektmappe angeben.

Dieser Artikel ist eine Zusammenfassung der Regeln und Einschränkungen für Azure-Ressourcen und eine Reihe von empfohlenen für Benennungskonventionen.  Diese Empfehlung können als Ausgangspunkt für eigene bestimmte Konventionen für Ihre Bedürfnisse.  

Der Schlüssel zum Erfolg Benennungskonventionen einrichten und Ihre Applikationen und Organisationen folgen. 

## <a name="naming-subscriptions"></a>Benennen von Abonnements

Beim Benennen von Azure-Abonnements stellen ausführliche Namen verstehen den Kontext und den Zweck jedes Abonnement löschen.  Beim Arbeiten in einer Umgebung mit vielen Abonnements können freigegebene Benennungskonvention Klarheit verbessert.

Empfohlene Muster für Benennung Abonnements ist:

`<Company> <Department (optional)> <Product Line (optional)> <Environment>`

- Unternehmen wäre normalerweise für jedes Abonnement. Einige Unternehmen haben jedoch untergeordnete Firmen innerhalb der Organisationsstruktur. Diese Unternehmen können von einer zentralen IT-Gruppe verwaltet werden. In diesen Fällen können sie mit den übergeordneten Firmennamen (*Contoso*) und untergeordnete Firmenname (*Nordwind*) unterschieden werden.

- Abteilung ist ein Name in der Organisation, in der eine Gruppe von Personen arbeiten. Dieses Element im Namespace als optional.

- Produktreihe ist einem bestimmten Namen für ein Produkt oder eine Funktion, die in der Abteilung ausgeführt wird.
Dies ist im Allgemeinen optional für interne Dienste und Programme. Jedoch wird empfohlen für Öffentliche Dienste verwenden, einfache Trennung und Identifizierung (wie klar der Abrechnung Datensätze) erforderlich sind.

- Umgebung ist der Name, der Lebenszyklus Bereitstellung von Applikationen oder Services oder Entwickler, QA, FA beschreibt.

| Unternehmen | Abteilung | Produktlinie oder Dienst | Umgebung | Vollständiger Name  |
----------| ---------- | ----------------------- | ----------- | ---------- |
| Contoso | SocialGaming | AwesomeService | Produktion | Contoso SocialGaming AwesomeService Produktion |
| Contoso | SocialGaming | AwesomeService | Entw. | Contoso SocialGaming AwesomeService Dev |
| Contoso | ES | InternalApps | Produktion | Contoso IT InternalApps Produktion |
| Contoso | ES | InternalApps | Entw. | Contoso IT InternalApps Dev |

<!-- TODO; include more information about organizing subscriptions for application deployment, pods, etc. -->

## <a name="use-affixes-to-avoid-ambiguity"></a>Verwenden Sie Affixe, um Mehrdeutigkeiten zu vermeiden

Beim Benennen von Ressourcen in Azure wird empfohlen, Präfixe oder Suffixe Typ und Kontext der Ressource identifizieren.  Alle Informationen über Art, Metadaten Kontext programmgesteuert verfügbar ist, vereinfacht die visuelle Kennung gemeinsamen anbringt anwenden.  Bei Affixtyp in die Benennungskonvention, unbedingt klar anzugeben, ob das Affix am Anfang des Namens (Präfix) oder Ende (Suffix).  

Hier sind beispielsweise zwei mögliche Namen für einen Dienst hostet ein Berechnungsmodul:

- SvcCalculationEngine (Präfix)
- CalculationEngineSvc (Suffix)

Affixe finden verschiedene Aspekte, die bestimmten Ressourcen beschreiben. Die folgende Tabelle zeigt einige Beispiele normalerweise verwendet.

| Seitenverhältnis | Beispiel | Notizen |
| ------ | ------- | ----- |
| Umgebung | Dev FA QA | Gibt die Umgebung für die Ressource |
| Speicherort | UW (US West), Ue (uns OST) | Identifiziert den Bereich in dem Ressource bereitgestellt wird |
| Instanz | 01, 02 | Ressourcen verfügen, mehrere benannte Instanz (Webserver usw.). |
| Produkt oder Dienst | Dienst | Identifiziert Produkt, Anwendung oder Dienst, der die Ressource unterstützt |
| Rolle | SQL web messaging | Gibt die Rolle der zugeordneten Ressource |

Eine spezielle Benennungskonvention für Ihr Unternehmen oder Projekte entwickeln, wird vor allem einen gemeinsamen Satz von Affixe und ihre Position (Suffix oder Präfix) auswählen.

## <a name="naming-rules-and-restrictions"></a>Regeln und Einschränkungen

Jeder Art Ressource oder den Dienst in Azure erzwingt eine Reihe von Einschränkungen und Bereich benennen; Namenskonvention oder Muster einzuhalten erforderlichen Regeln und Umfang.  Während der Name eines virtuellen Computers einen DNS-Namen zugeordnet und muss daher auf allen Azure eindeutig sein ist der Name einer VNET beispielsweise der Ressourcengruppe beschränkt, die in erstellt wird.

Im Allgemeinen vermeiden Sonderzeichen (`-` oder `_`) als erste oder letzte Zeichen im Namen. Diese Zeichen werden die meisten Validierungsregeln nicht verursachen.

| Kategorie | Dienst oder Entität | Gültigkeitsbereich | Länge | Gehäuse | Gültige Zeichen | Vorgeschlagenes Muster | Beispiel |
| ------------- | ----------------- | ----- | ------ | ------ | ---------------- | ----------------- | ------- |
| Ressourcengruppe | Ressourcengruppe | Globale | 1-64 | Kleinschreibung | Alphanumerisch, Unterstrich und Bindestrich | `<service short name>-<environment>-rg` | `profx-prod-rg` |
| Ressourcengruppe | Festlegen der Verfügbarkeit | Ressourcengruppe | 1-80 | Kleinschreibung | Alphanumerisch, Unterstrich und Bindestrich | `<service-short-name>-<context>-as` | `profx-sql-as` |
| Allgemein | Tag | Zugeordnete Entität | 512 (Name), 256 (Wert) | Kleinschreibung | Alphanumerische | `"key" : "value"` | `"department" : "Central IT"` |
| Berechnen | Virtual Machine | Ressourcengruppe | 1-15 | Kleinschreibung | Alphanumerisch, Unterstrich und Bindestrich | `<name>-<role>-<instance>` | `profx-sql-001` |
| Speicher | Speicher-Kontoname (Daten) | Globale | 3 24 | Kleinbuchstaben | Alphanumerische | `<service short name><type><number>` | `profxdata001` |
| Speicher | Speicher-Kontoname (Datenträger) | Globale | 3 24 | Kleinbuchstaben | Alphanumerische | `<vm name without dashes>st<number>` | `profxsql001st0` |
| Speicher | Containername | Konto | 3-63 |   Kleinbuchstaben | Alphanumerisch und Strich | `<context>` | `logs` |
| Speicher | BLOB-name | Container | 1-1024. | Groß-/Kleinschreibung | Jede URL-Zeichen | `<variable based on blob usage>` | `<variable based on blob usage>` |
| Speicher | Name der Warteschlange | Konto | 3-63 | Kleinbuchstaben | Alphanumerisch und Strich | `<service short name>-<context>-<num>` | `awesomeservice-messages-001` |
| Speicher | Tabellenname | Konto | 3-63 |Kleinschreibung | Alphanumerische | `<service short name>-<context>` | `awesomeservice-logs` |
| Speicher | Dateiname | Konto | 3-63 | Kleinbuchstaben | Alphanumerische | `<variable based on blob usage>` | `<variable based on blob usage>` |
| Netzwerk | Virtuelles Netzwerk (VNet) | Ressourcengruppe | 2-64 | Kleinschreibung | Alphanumerisch, Bindestrich, Unterstrich und Punkt | `<service short name>-[section]-vnet` | `profx-vnet` |
| Netzwerk | Subnetz | Übergeordnete VNet | 2-80 | Kleinschreibung | Alphanumerisch, Unterstrich, Strich und Punkt | `<role>-subnet` | `gateway-subnet` |
| Netzwerk | Netzwerkschnittstelle | Ressourcengruppe | 1-80 | Kleinschreibung | Alphanumerisch, Bindestrich, Unterstrich und Punkt | `<vmname>-<num>nic` | `profx-sql1-1nic` |
| Netzwerk | Netzwerk-Sicherheitsgruppe | Ressourcengruppe | 1-80 | Kleinschreibung | Alphanumerisch, Bindestrich, Unterstrich und Punkt | `<service short name>-<context>-nsg` | `profx-app-nsg` |
| Netzwerk | Netzwerk-Sicherheitsgruppe Regel | Ressourcengruppe | 1-80 | Kleinschreibung | Alphanumerisch, Bindestrich, Unterstrich und Punkt | `<descriptive context>` | `sql-allow` |
| Netzwerk | Öffentliche IP-Adresse | Ressourcengruppe | 1-80 | Kleinschreibung | Alphanumerisch, Bindestrich, Unterstrich und Punkt | `<vm or service name>-pip` | `profx-sql1-pip` |
| Netzwerk | Lastenausgleich | Ressourcengruppe | 1-80 | Kleinschreibung | Alphanumerisch, Bindestrich, Unterstrich und Punkt | `<service or role>-lb` | `profx-lb` |
| Netzwerk | Ausgewogene Regeln Konfiguration laden | Lastenausgleich | 1-80 | Kleinschreibung | Alphanumerisch, Bindestrich, Unterstrich und Punkt | `descriptive context` | `http` |

<!-- TODO fill in the rest of these resources
| Networking | Azure Application Gateway | Resource Group | 1-80 | Case-insensitive | Alphanumeric, dash, underscore, and period | `<service or role>-aag` | `profx-aag`
| Networking | Azure Application Gateway Connection | Azure Application Gateway | 1-80 | Case-insensitive | Alphanumeric, dash, underscore, and period | `` | TODO
| Networking | Traffic Manager Profile | Resource Group | 1-80 | Case-insensitive | Alphanumeric, dash, underscore, and period | TODO | TODO
-->

## <a name="organizing-resources-with-tags"></a>Organisieren von Ressourcen mit Tags

Der Azure-Ressourcen-Manager unterstützt tagging Entitäten mit beliebigen Zeichenfolgen zu Kontext Automatisierung optimieren.  Beispielsweise das Tag `"sqlVersion: "sql2014ee"` VMs in einer Bereitstellung mit SQL Server 2014 Enterprise Edition für die Ausführung von eines automatischen Skripts dafür ermitteln konnte.  Tags sollten zu ergänzen Kontext auf der gewählten Benennungskonventionen verwendet werden.

> [AZURE.TIP]Ein weiterer Vorteil von Tags ist Tags umfassen Ressourcengruppen und Korrelieren von Entitäten in unterschiedlichen Bereitstellung ermöglicht.

Jede Ressource oder Ressourcengruppe kann maximal **15** Tags haben. Der Tagname ist auf 512 Zeichen beschränkt und den Wert der Marke ist auf 256 Zeichen beschränkt.

Weitere Informationen zum tagging Ressourcen finden Sie in [Tags Azure Ressourcen zu verwenden](../resource-group-using-tags.md).

Tagging Einsatzbereiche sind:

- **Abrechnung**; Gruppieren von Ressourcen und Zuordnen zu Abrechnung oder kostenlos back-Codes.
- **Identifikation von Diensten Kontext**; Identifizieren Sie Gruppen von Ressourcen in Ressourcengruppen für allgemeine Operationen und Gruppierung
- **Zugriffskontrolle und Sicherheitskontext**. Portfolio, Service, app, Instanz usw. Grundlage Administratorrolle-ID.

> [AZURE.TIP]Tag früher - Tags oftmals.  Besser, eine Baseline tagging Regelung eingeführt und Anpassung im Laufe der Zeit anstatt nachträglich überarbeiten.  

Beispiel für einige gemeinsame Tag:

| Tagname | Schlüssel | Beispiel | Kommentar |
| -------- | --- | ------- | ------- |
| Rechnung und interne Chargeback-ID | Rechnungsadresse  | `IT-Chargeback-1234` | Ein interner e/A- oder Rechnungscode |
| Operator oder direkt verantwortliche Person (DRI) | managedBy | `joe@contoso.com`  | Alias oder die e-Mail-Adresse |
| Projektname | Projekt-name | `myproject`  | Projekt oder Produkt Linie |
| Projektversion | Projekt-version | `3.4`  | Version der Projekt oder Produkt |
| Umgebung | Umgebung | `<Production, Staging, QA >` | Umwelt-ID | 
| Ebene | Ebene | `Front End, Back End, Data` | Ebene oder Rolle-Kontext-ID |
| Datenprofil | dataProfile | `Public, Confidential, Restricted, Internal` | Vertraulichkeit der Daten in der Ressource |
 
## <a name="tips-and-tricks"></a>Tipps und Tricks

Einige Typen von Ressourcen erfordern zusätzliche Pflege zu benennen und Konventionen.

### <a name="virtual-machines"></a>Virtuelle Computer

Besonders in größeren Topologien optimiert sorgfältig Benennen von virtuellen Maschinen und Zweck der einzelnen Computer identifizieren und besser vorhersehbare Skripting aktivieren.

> [AZURE.WARNING]Alle virtuellen Computer in Azure hat einen Azure-Ressource, und ein Betriebssystem-Hostnamen.  
> Wenn Ressourcennamen und Hostnamen unterscheiden, die VMs verwalten kann eine Herausforderung sein und sollte vermieden werden.
> Beispielsweise erstellt ein virtuellen Computer aus einer VHD-Datei, die bereits enthält eine Betriebssystem konfigurierte mit einem Hostnamen.

- [Namenskonventionen für Windows Server-VMs](https://support.microsoft.com/en-us/kb/188997)

<!-- TODO - recommendations on naming VMs. -->

### <a name="storage-accounts-and-storage-entities"></a>Speicherkonten und Storage-Entitäten

Es sind zwei primäre für Speicherkonten - Festplatten für virtuelle Computer sichern und Speichern von Daten in Blobs, Queues und Tabellen.  Speicherkonten zur VM Datenträger sollte folgen die Namenskonvention von übergeordneten VM Namen zuordnen und mit der Notwendigkeit für mehrere Speicherkonten High-End-VM-SKUs, gilt ein Suffix für.

> [AZURE.TIP]Speicherkonten - befolgen Daten oder Festplatten - Benennungskonventionen, die mehrere Speicherkonten genutzt werden (d. h. immer mit einem numerischen Suffix) ermöglicht.

Sie können einen benutzerdefinierten Domänennamen für den Zugriff auf BLOB-Daten in Azure Storage-Konto konfigurieren.
Der Standardendpunkt für den BLOB-Dienst ist `https://mystorage.blob.core.windows.net`.

Aber wenn Sie BLOB-Endpunkt für das Speicherkonto eine benutzerdefinierte Domäne (z. B. www.contoso.com) zuordnen, können Sie auch zugreifen BLOB-Daten in das Speicherkonto mit dieser Domäne. Beispielsweise mit einem benutzerdefinierten Domänennamen `http://mystorage.blob.core.windows.net/mycontainer/myblob` wie möglich `http://www.contoso.com/mycontainer/myblob`.

Weitere Informationen zum Konfigurieren dieses Features finden Sie [einen benutzerdefinierten Domänennamen für den BLOB-Speicher Endpunkt konfigurieren](../storage/storage-custom-domain-name.md).

Weitere Informationen zur Benennung von Blobs, Container und Tabellen:

- [Benennung und verweisen auf Container, Blobs und Metadaten](https://msdn.microsoft.com/library/dd135715.aspx)
- [Benennung von Warteschlangen und Metadaten](https://msdn.microsoft.com/library/dd179349.aspx)
- [Benennen von Tabellen](https://msdn.microsoft.com/library/azure/dd179338.aspx)

Ein Blob kann eine beliebige Kombination aus Zeichen enthalten, jedoch reservierte URL-Zeichen müssen ordnungsgemäß mit Escapezeichen versehen werden. Vermeiden Sie blobnamen, die mit einem Punkt (.), Schrägstrich (/) enden oder eine Sequenz oder eine Kombination aus beiden. Gemäß der Konvention ist der Schrägstrich **virtuellen** Verzeichnistrennzeichen. Verwenden Sie einen umgekehrten Schrägstrich (\) in ein Blob. Der Client APIs möglicherweise zulassen, aber dann nicht ordnungsgemäß hash und Signaturen stimmen nicht überein.

Es ist nicht möglich, den Namen eines Speicherkonto oder Container ändern, nachdem es erstellt wurde.
Wenn Sie einen neuen Namen verwenden möchten, müssen Sie löschen und eine neue erstellen.

> [AZURE.TIP] Wir empfehlen, eine Benennungskonvention für alle Speicherkonten und einzurichten, bevor auf die Entwicklung eines neuen Dienstes oder einer Anwendung.

## <a name="example---deploying-an-n-tier-service"></a>Beispiel - Bereitstellen eines n-Tier-Dienstes

In diesem Beispiel definieren wir eine n-Tier-Konfiguration mit Front-End-IIS-Servern (Windows Server VMs gehostet) mit SQL Server (in zwei Windows Server VMs gehostet) ein ElasticSearch Cluster (6 Linux VMs gehostet) und die zugehörige Speicher-Konten virtuelle Netzwerke Ressource und Lastenausgleich.

Wir beginnen die kontextbezogenen Konventionen für diese Anwendung definieren:

| Entität | Übereinkommen | Beschreibung  |
| ------ | ---------- | ------------ |  
| Dienstname | `profx` | Der Kurzname der Anwendung oder des Dienstes bereitgestellt wird |
| Umgebung | `prod` | Dies ist für die Bereitstellung in der Produktion (im Gegensatz zu QA, Test, etc..) |

Von dieser Basislinie können wir dann die Konventionen für jeden Ressourcentypen darstellen:

| Ressourcentyp | Konvention Base | Beispiel |
| ------------- | --------------- | ------- |
| Abonnement | `<Company> <Department (optional)> <Product Line (optional)> <Environment>` | `Contoso IT InternalApps Profx Production` |
| Ressourcengruppe | `servicename-rg` | `profx-rg` |
| Virtuelles Netzwerk | `servicename-vnet` | `profx-vnet` |
| Subnetz | `role-subnet` | `sql-vnet` |
| Lastenausgleich | `servicename-lb` | `profx-lb` |
| Virtual Machine | `servicename-role[number]` | `profx-sql0` |
| Konto | `<vmnamenodashes>st<num>` | `profxsql0st0` |

Wie im folgenden Diagramm dargestellt:

![Topologiediagramm Anwendung] (media/guidance-naming-conventions/guidance-naming-convention-example.png "Beispieltopologie Anwendung")

## <a name="sample---azure-cli-script-for-deploying-the-sample-above"></a>Beispiel - Azure CLI Skripts Beispiel

```bash
#!/bin/sh

#####################################################################
# Sample script using the Azure CLI to build out an application 
# demonstrating naming conventions.  
#
# Note; this script is not intended for production deployment, as it does 
# not create availability sets, configure network security rules, etc.
#####################################################################

# Set up variables to build out the naming conventions for deploying
# the cluster  
LOCATION=eastus2
APP_NAME=profx
ENVIRONMENT=prod
USERNAME=testuser
PASSWORD="testpass"

# Set up the tags to associate with items in the application
TAG_BILLTO="InternalApp-ProFX-12345"
TAGS="billTo=${TAG_BILLTO}"

# Explicitly set the subscription to avoid confusion as to which subscription
# is active/default
SUBSCRIPTION=3e9c25fc-55b3-4837-9bba-02b6eb204331

# Set up the names of things using recommended conventions
RESOURCE_GROUP="${APP_NAME}-${ENVIRONMENT}-rg"
VNET_NAME="${APP_NAME}-vnet"

# Set up the postfix variables attached to most CLI commands
POSTFIX="--resource-group ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}"

##########################################################################################
# Set up the VM conventions for Linux and Windows images

# For Windows, get the list of URN's via 
# azure vm image list ${LOCATION} MicrosoftWindowsServer WindowsServer 2012-R2-Datacenter
WINDOWS_BASE_IMAGE=MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20160126

# For Linux, get the list or URN's via 
# azure vm image list ${LOCATION} canonical ubuntuserver
LINUX_BASE_IMAGE=canonical:ubuntuserver:16.04.0-DAILY-LTS:16.04.201602130

#########################################################################################
## Define functions 
create_vm ()
{
    vm_name=$1
    vnet_name=$2
    subnet_name=$3
    os_type=$4
    vhd_path=$5
    vm_size=$6
    diagnostics_storage=$7

    # Create the network interface card for this VM
    azure network nic create --name "${vm_name}-0nic" --subnet-name ${subnet_name} --subnet-vnet-name ${vnet_name} \
        --tags="${TAGS}" ${POSTFIX}

    # Create the storage account for this vm's disks (premium locally redundant storage -> PLRS)
    # Note the ${var//-/} syntax to remove dashes from the vm name
    storage_account_name=${vm_name//-/}st01
    azure storage account create --type=PLRS --tags "${TAGS}" ${POSTFIX} "${storage_account_name}"

    # Map the name of the diagnostics storage account to a blob URI for boot diagnostics
    # This is (currently) required when deploying with a named premium storage account 
    diag_blob="https://${diagnostics_storage}.blob.core.windows.net/"

    # Create the VM
    azure vm create --name ${vm_name} --nic-name "${vm_name}-0nic" --os-type ${os_type} \
        --image-urn ${vhd_path} --vm-size ${vm_size} --vnet-name ${vnet_name} --vnet-subnet-name ${subnet_name} \
        --storage-account-name "${storage_account_name}" --storage-account-container-name vhds --os-disk-vhd "${vm_name}-osdisk.vhd" \
        --admin-username "${USERNAME}" --admin-password "${PASSWORD}" \
        --boot-diagnostics-storage-uri "${diag_blob}" \
        --tags="${TAGS}" ${POSTFIX} 
}

###################################################################################################
# Create resources

# Step 1 - create the enclosing resource group
azure group create --name "${RESOURCE_GROUP}" --location "${LOCATION}" --tags "${TAGS}" --subscription "${SUBSCRIPTION}"

# Step 2 - create the network security groups

# Step 3 - create the networks (VNet and subnets)
azure network vnet create --name "${VNET_NAME}" --address-prefixes="10.0.0.0/8" --tags "${TAGS}" ${POSTFIX}
# TODO - does subnet support tagging?
azure network vnet subnet create --name gateway-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.1.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}
azure network vnet subnet create --name es-master-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.2.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}
azure network vnet subnet create --name es-data-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.3.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}
azure network vnet subnet create --name sql-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.4.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}

# Step 4 - define the load balancer and network security rules
azure network lb create --name "${APP_NAME}-lb" ${POSTFIX}
# In a production deployment script, we'd create load balancer rules and 
# network security groups here

# Step 5 - create a diagnostics storage account
diagnostics_storage_account=${APP_NAME//-/}diag
azure storage account create --type=LRS --tags "${TAGS}" ${POSTFIX} "${diagnostics_storage_account}"

# Step 6.1 - Create the gateway VMs
for i in `seq 1 4`;
do
    create_vm "${APP_NAME}-gw${i}" "${APP_NAME}-vnet" "gateway-subnet" "Windows" "${WINDOWS_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}" 
done    

# Step 6.2 - Create the ElasticSearch master and data VMs
for i in `seq 1 3`;
do
    create_vm "${APP_NAME}-es-master${i}" "${APP_NAME}-vnet" "es-master-subnet" "Linux" "${LINUX_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
done
for i in `seq 1 3`;
do
    create_vm "${APP_NAME}-es-data${i}" "${APP_NAME}-vnet" "es-data-subnet" "Linux" "${LINUX_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
done

# Step 6.3 - Create the SQL VMs
create_vm "${APP_NAME}-sql0" "${APP_NAME}-vnet" "sql-subnet" "Windows" "${WINDOWS_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
create_vm "${APP_NAME}-sql1" "${APP_NAME}-vnet" "sql-subnet" "Windows" "${WINDOWS_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
```
