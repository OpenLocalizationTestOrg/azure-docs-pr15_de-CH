<properties 
    pageTitle="Zum Erstellen und Verwalten von Azure Redis Cache Azure Befehlszeilenschnittstelle (CLI Azure) | Microsoft Azure" 
    description="Informationen Sie zu Ihrem Azure-Konto verbunden Azure-CLI auf allen Plattformen installieren und zum Erstellen und Verwalten eines Redis Caches von Azure-CLI." 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="how-to-create-and-manage-azure-redis-cache-using-the-azure-command-line-interface-azure-cli"></a>Zum Erstellen und Verwalten von Azure Redis Cache Azure Befehlszeilenschnittstelle (CLI Azure)

> [AZURE.SELECTOR]
- [PowerShell](cache-howto-manage-redis-cache-powershell.md)
- [Azure CLI](cache-manage-cli.md)

Azure-CLI lässt der Azure-Infrastruktur von jeder Plattform verwalten. In diesem Artikel wird das Erstellen und Verwalten von Azure Redis Cache Instanzen der Azure-Befehlszeilenschnittstelle veranschaulicht.

## <a name="prerequisites"></a>Erforderliche Komponenten

Zum Erstellen und Verwalten von Azure Redis Cacheinstanzen Azure CLI verwenden, müssen Sie die folgenden Schritte ausführen.

-   Sie müssen ein Azure-Konto. Wenn Sie eine haben, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) in wenigen Minuten erstellen.
-   [Azure CLI installieren](../xplat-cli-install.md).
-   Verbinden die Azure-CLI-Installation mit einem persönlichen Konto Azure, oder oder Schule Azure-Konto, und melden Sie sich aus dem Azure-CLI mithilfe der `azure login` Befehl. Die Unterschiede auswählen und anzeigen Sie [mit Azure-Abonnement aus der Azure-Befehlszeilenschnittstelle (CLI Azure)](../xplat-cli-connect.md)
-   Wechseln Sie vor dem Ausführen der folgenden Befehle, Azure-CLI in Ressourcen-Manager-Modus mit der `azure config mode arm` Befehl. Weitere Informationen finden Sie unter [festlegen den Azure-Ressourcen-Manager-Modus](../xplat-cli-azure-resource-manager.md#set-the-azure-resource-manager-mode).

## <a name="redis-cache-properties"></a>Redis Cache-Eigenschaften

Die folgenden Eigenschaften werden beim Erstellen und Aktualisieren von Redis Cacheinstanzen verwendet.

| Eigenschaft            | Schalter                                  | Beschreibung                                                                                                                                                                                                                                          |
|---------------------|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Name                | -n, -name                              | Name des Redis-Cache.                                                                                                                                                                                                                             |
| Ressourcengruppe      | -g, -Ressourcengruppen                    | Name der Ressourcengruppe.                                                                                                                                                                                                                          |
| Speicherort            | -l, -Position                          | Speicherort zum Cache erstellen.                                                                                                                                                                                                                            |
| Größe                | -Z, Größe                              | Größe des Redis-Cache. Gültige Werte: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]                                                                                                                                                                  |
| SKU                 | -X, - sku                               | Redis SKU. Sollte: [Basic, Standard, Premium]                                                                                                                                                                                             |
| EnableNonSslPort    | -e, -Enable-nicht-Ssl-port               | EnableNonSslPort-Eigenschaft des Redis-Cache. Hinzufügen dieses Flag nicht SSL-Anschluss für den Cache aktivieren                                                                                                                                    |
| Redis Konfiguration | -c, Redis-Konfiguration               | Redis Konfiguration. JSON formatierte Zeichenfolge Konfigurationsschlüssel und Werte eingeben. Format: "{" ":","": ""} "                                                                                                                                     |
| Redis Konfiguration | -f, -Redis-Konfigurationsdatei          | Redis Konfiguration. Geben Sie den Pfad einer Datei mit Konfigurationsschlüsseln und Werte. Format für den Eintrag: {"": "," ":" "}                                                                                                                |
| Anzahl der Splitter         | -R, -Splitter-Anzahl                       | Anzahl der Splitter auf ein Premium-Cluster-Cache mit clustering erstellt.                                                                                                                                                                               |
| Virtuelles Netzwerk     | -V, virtuelles Netzwerk                   | Wenn den Cache in einem VNET, gibt die genaue ARM Ressource ID des virtuellen Netzwerks Redis Cache in bereitstellen. Beispiel-Format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| Schlüsseltyp            | -t, Schlüsseltyp                          | Der Typ des Schlüssels zu erneuern. Gültige Werte: [primäre, sekundäre]                                                                                                                                                                                             |
| StaticIP            | -p, statische Ip-< statische Ip >             | Wenn den Cache in einem VNET, gibt eine eindeutige IP-Adresse im Subnetz für den Cache. Wenn nicht angegeben, wird eine aus dem Subnetz für Sie ausgewählt.                                                                                                |
| Subnetz              | t, -Subnetz<subnet>                    | Wenn den Cache in einem VNET, gibt den Namen des Subnetzes, Cache bereitstellen.                                                                                                                                                    |
| VirtualNetwork      | -V, virtuelles Netzwerk < virtuellen Netzwerk > | Wenn den Cache in einem VNET, gibt die genaue ARM Ressource ID des virtuellen Netzwerks Redis Cache in bereitstellen. Beispiel-Format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| Abonnement        | -s, -Abonnement                      | Die Abonnement-ID.                                                                                                                                                                                                                         |

## <a name="see-all-redis-cache-commands"></a>Alle Redis Cache Befehle

Um alle Redis Cache-Befehle und Parameter finden Sie unter Verwenden der `azure rediscache -h` Befehl.

    C:\>azure rediscache -h
    help:    Commands to manage your Azure Redis Cache(s)
    help:
    help:    Create a Redis Cache
    help:      rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Delete an existing Redis Cache
    help:      rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    List all Redis Caches within your Subscription or Resource Group
    help:      rediscache list [options]
    help:
    help:    Show properties of an existing Redis Cache
    help:      rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Change settings of an existing Redis Cache
    help:      rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Renew the authentication key for an existing Redis Cache
    help:      rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:      rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help  output usage information
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="create-a-redis-cache"></a>Erstellen einer Redis

Erstellen Sie einen Cache Redis Befehl:

    azure rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]

Weitere Informationen zu diesem Befehl Ausführen der `azure rediscache create -h` Befehl.

    C:\>azure rediscache create -h
    help:    Create a Redis Cache
    help:
    help:    Usage: rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -l, --location <location>                                Location to create cache.
    help:      -z, --size <size>                                        Size of the Redis Cache. Valid values: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]
    help:      -x, --sku <sku>                                          Redis SKU. Should be one of : [Basic, Standard, Premium]
    help:      -e, --enable-non-ssl-port                                EnableNonSslPort property of the Redis Cache. Add this flag if you want to enable the Non SSL Port for your cache
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here. Format:"{"<key1>":"<value1>","<key2>":"<value2>"}"
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here. Format for the file entry: {"<key1>":"<value1>","<key2>":"<value2>"}
    help:      -r, --shard-count <shard-count>                          Number of Shards to create on a Premium Cluster Cache
    help:      -v, --virtual-network <virtual-network>                  The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1
    help:      -t, --subnet <subnet>                                    Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -p, --static-ip <static-ip>                              Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -s, --subscription <id>                                  the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="delete-an-existing-redis-cache"></a>Vorhandene Redis Cache löschen

Um einen Redis-Cache zu löschen, verwenden Sie den folgenden Befehl ein:

    azure rediscache delete [--name <name> --resource-group <resource-group> ]

Weitere Informationen zu diesem Befehl Ausführen der `azure rediscache delete -h` Befehl.

    C:\>azure rediscache delete -h
    help:    Delete an existing Redis Cache
    help:
    help:    Usage: rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which the cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-all-redis-caches-within-your-subscription-or-resource-group"></a>Listen Sie alle Redis Caches in Ihrem Abonnement oder Ressourcengruppe

Um alle Redis Caches in Ihrem Abonnement oder Ressourcengruppe sind, verwenden Sie den folgenden Befehl ein:

    azure rediscache list [options]

Weitere Informationen zu diesem Befehl Ausführen der `azure rediscache list -h` Befehl.

    C:\>azure rediscache list -h
    help:    List all Redis Caches within your Subscription or Resource Group
    help:
    help:    Usage: rediscache list [options]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="show-properties-of-an-existing-redis-cache"></a>Eigenschaften der vorhandene Redis Cache anzeigen

Verwenden Sie den folgenden Befehl, um die Eigenschaften einer vorhandenen Redis Cache anzuzeigen:

    azure rediscache show [--name <name> --resource-group <resource-group>]

Weitere Informationen zu diesem Befehl Ausführen der `azure rediscache show -h` Befehl.

    C:\>azure rediscache show -h
    help:    Show properties of an existing Redis Cache
    help:
    help:    Usage: rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

<a name="scale"></a>
## <a name="change-settings-of-an-existing-redis-cache"></a>Ändern einer vorhandenen Redis-Cache

Um einen vorhandenen Redis-Cache zu ändern, verwenden Sie den folgenden Befehl ein:

    azure rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]

Weitere Informationen zu diesem Befehl Ausführen der `azure rediscache set -h` Befehl.

    C:\>azure rediscache set -h
    help:    Change settings of an existing Redis Cache
    help:
    help:    Usage: rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here.
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here.
    help:      -s, --subscription <subscription>                        the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="renew-the-authentication-key-for-an-existing-redis-cache"></a>Erneuern Sie den Authentifizierungsschlüssel für vorhandene Redis Cache

Verwenden Sie den folgenden Befehl, um den Authentifizierungsschlüssel für vorhandene Redis Cache zu erneuern:

    azure rediscache renew-key [--name <name> --resource-group <resource-group> --key-type <key-type>]

Geben Sie `Primary` oder `Secondary` für `key-type`.

Weitere Informationen zu diesem Befehl Ausführen der `azure rediscache renew-key -h` Befehl.

    C:\>azure rediscache renew-key -h
    help:    Renew the authentication key for an existing Redis Cache
    help:
    help:    Usage: rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which cache exists
    help:      -t, --key-type <key-type>              type of key to renew. Valid values are: 'Primary', 'Secondary'.
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-primary-and-secondary-keys-of-an-existing-redis-cache"></a>Vorhandene Redis Cache Liste primärer und Sekundärer Schlüssel

Zu Liste primärer und Sekundärer Schlüssel vorhandene Redis Cache Befehl:

    azure rediscache list-keys [--name <name> --resource-group <resource-group>]

Weitere Informationen zu diesem Befehl Ausführen der `azure rediscache list-keys -h` Befehl.

    C:\>azure rediscache list-keys -h
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:
    help:    Usage: rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which Cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)
