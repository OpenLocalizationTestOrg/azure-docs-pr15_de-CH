<properties
   pageTitle="Erste Schritte mit Azure Batch CLI | Microsoft Azure"
   description="Abrufen Sie eine kurze Einführung in die Batch-Befehle zum Verwalten von Azure Batch Ressourcen in Azure CLI"
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="multiple"
   ms.workload="big-compute"
   ms.date="09/30/2016"
   ms.author="marsma"/>

# <a name="get-started-with-azure-batch-cli"></a>Erste Schritte mit Azure Batch CLI

Plattformübergreifende Azure-Befehlszeilenschnittstelle (CLI Azure) können Sie zum Verwalten von Batch-Konten und Ressourcen wie Pools, Projekte und Aufgaben in Windows, Linux und Mac Befehlszeilenprogramme. Mit der CLI Azure können Sie Skript für viele der Aufgaben, die Batch-APIs, Azure-Portal und Batch-PowerShell-Cmdlets durchführen und ausführen.

Dieser Artikel basiert auf Azure CLI Version 0.10.5.

## <a name="prerequisites"></a>Erforderliche Komponenten

* [Azure CLI installieren](../xplat-cli-install.md)

* [Verbinden der Azure-CLI mit Azure-Abonnement](../xplat-cli-connect.md)

* Wechseln Sie zum **Ressourcen-Manager-Modus**:`azure config mode arm`

>[AZURE.TIP] Wir empfehlen Sie Ihren Azure-Befehlszeilenschnittstelle zum Nutzen des Service-Updates und verbesserte aktualisieren.

## <a name="command-help"></a>Befehl Hilfe

Sie können Hilfetext für jeden Befehl in der Azure-CLI Anhängen anzeigen `-h` als einzige Option nach dem Befehl. Zum Beispiel:

* Hilfe für die `azure` Befehl, geben:`azure -h`
* Um eine Liste aller Stapel Befehle in der CLI, verwenden:`azure batch -h`
* Hilfe zum Erstellen eines Batch-Kontos eingeben:`azure batch account create -h`

Verwenden Sie im Zweifelsfall die `-h` Befehlszeilenoption, um Hilfe zu den Azure-CLI-Befehlen.

## <a name="create-a-batch-account"></a>Ein Batch-Konto erstellen

Syntax:

    azure batch account create [options] <name>

Beispiel:

    azure batch account create --location "West US"  --resource-group "resgroup001" "batchaccount001"

Erstellt ein neues Batch-Konto mit den angegebenen Parametern. Sie müssen mindestens einen Standort, Ressourcengruppe und Kontonamen angeben. Haben Sie bereits eine Ressourcengruppe, erstellen Sie mit `azure group create`, und geben Sie eine Azure Regionen (wie "West US") für die `--location` Option. Zum Beispiel:

    azure group create --name "resgroup001" --location "West US"

> [AZURE.NOTE] Batch-Kontoname muss Azure Bereich eindeutig das Konto erstellt wird. Nur Kleinbuchstaben alphanumerische Zeichen enthalten und muss 3 24 Zeichen lang sein. Sie können keine Sonderzeichen wie `-` oder `_` in Batch-Kontonamen.

### <a name="linked-storage-account-autostorage"></a>Verknüpftes Konto (Autostorage)

Sie können beim Erstellen (optional) ein **allgemeiner** Speicherkonto Batch-Konto verknüpfen. [Anwendungspakete](batch-application-packages.md) Feature des Stapels verwendet BLOB-Speicher in verknüpfte allgemeine Speicherkonto wie [Batch Datei Konventionen .NET](batch-task-output.md) Library. Dieser optionalen Features Sie bei der Bereitstellung die Stapelverarbeitungsaufgaben ausgeführt Applications und Beibehalten von Daten, die sie erzeugen.

Geben Sie zum Verknüpfen eines Azure Storage-Kontos mit einem neuen Batch-Konto beim Erstellen der `--autostorage-account-id` Option. Diese Option muss die vollqualifizierte Ressourcen-ID des Speicherkontos.

Das Speicherkonto Details zuerst anzeigen:

    azure storage account show --resource-group "resgroup001" "storageaccount001"

Verwenden Sie den **Url** -Wert für die `--autostorage-account-id` Option. Der Url-Wert mit "Abonnements /" beginnt und Ihre Abonnement-ID und Ressource Pfad das Speicherkonto enthält:

    azure batch account create --location "West US"  --resource-group "resgroup001" --autostorage-account-id "/subscriptions/8ffffff8-4444-4444-bfbf-8ffffff84444/resourceGroups/resgroup001/providers/Microsoft.Storage/storageAccounts/storageaccount001" "batchaccount001"

## <a name="delete-a-batch-account"></a>Batch-Konto löschen

Syntax:

    azure batch account delete [options] <name>

Beispiel:

    azure batch account delete --resource-group "resgroup001" "batchaccount001"

Löscht das angegebene Batch-Konto. Wenn Sie aufgefordert werden, bestätigen Sie das Konto entfernen möchten (Entfernen des Kontos kann einige Zeit dauern).

## <a name="manage-account-access-keys"></a>Zugriffstasten Konto verwalten

Sie benötigen eine Zugriffstaste [Erstellen](#create-and-modify-batch-resources) und Bearbeiten von Ressourcen in Batch-Konto.

### <a name="list-access-keys"></a>Liste von Zugriffstasten

Syntax:

    azure batch account keys list [options] <name>

Beispiel:

    azure batch account keys list --resource-group "resgroup001" "batchaccount001"

Listet die Konto-Tasten für die Stapelverarbeitung Konto.

### <a name="generate-a-new-access-key"></a>Erstellen Sie eine neue Tastenkombination

Syntax:

    azure batch account keys renew [options] --<primary|secondary> <name>

Beispiel:

    azure batch account keys renew --resource-group "resgroup001" --primary "batchaccount001"

Schlüssel angegebene Konto für die Stapelverarbeitung Konto regeneriert.

## <a name="create-and-modify-batch-resources"></a>Erstellen und Ändern von Batch-Ressourcen

Verwenden der Azure-CLI erstellen, lesen, aktualisieren und löschen (CRUD) Batch Ressourcen wie Pools, Knoten, Aufträgen und Aufgaben zu berechnen. Diese Vorgänge müssen Ihr Kontonamen Zugriffstaste und Endpunkt. Sie können mit den `-a`, `-k`, und `-u` Optionen oder festgelegten [Variablen](#credential-environment-variables) die CLI verwendet automatisch (wenn belegt).

### <a name="credential-environment-variables"></a>Anmeldeinformationen Umgebungsvariablen

Legen Sie `AZURE_BATCH_ACCOUNT`, `AZURE_BATCH_ACCESS_KEY`, und `AZURE_BATCH_ENDPOINT` Variablen statt `-a`, `-k`, und `-u` Optionen in der Befehlszeile für jeden Befehl ausführen. Batch CLI verwendet diese Variablen (falls festgelegt), weglassen können die `-a`, `-k`, und `-u` Optionen. Der Rest dieses Artikels wird die Verwendung dieser Umgebungsvariablen.

>[AZURE.TIP] Liste die Tasten mit `azure batch account keys list`, und das Konto Endpunkt mit `azure batch account show`.

### <a name="json-files"></a>JSON-Dateien

Beim Erstellen von Batch-Ressourcen-Pools und Aufträge können eine JSON-Datei mit der neuen Ressource Konfiguration anstatt Parameter als Befehlszeilenoptionen angeben. Zum Beispiel:

`azure batch pool create my_batch_pool.json`

Während Sie viele Ressourcen erstellen mithilfe von Befehlszeilenoptionen nur Operationen können, erfordern einige Features eine JSON-formatierte Datei mit der Ressource. Beispielsweise verwenden Sie eine JSON-Datei Wenn Sie Ressourcendateien für einen Start-Vorgang angeben möchten.

JSON zum Erstellen einer Ressource finden Sie auf den [Stapel REST-API-Referenz] [ rest_api] auf der MSDN-Dokumentation. Jedes Thema *"Ressource*hinzufügen" enthält Beispiel JSON zum Erstellen der Ressource, die Sie als Vorlagen für die JSON-Dateien verwenden können. Z. B. JSON für Pool-Erstellung finden Sie in [einem Pool ein Konto hinzufügen][rest_add_pool].

>[AZURE.NOTE] Wenn Sie beim Erstellen einer Ressource eine JSON-Datei angeben, werden alle Parameter, die in der Befehlszeile für diese Ressource angegeben ignoriert.

## <a name="create-a-pool"></a>Erstellen eines Pools

Syntax:

    azure batch pool create [options] [json-file]

Beispiel (virtuelle Computerkonfiguration):

    azure batch pool create --id "pool001" --target-dedicated 1 --vm-size "STANDARD_A1" --image-publisher "Canonical" --image-offer "UbuntuServer" --image-sku "14.04.2-LTS" --node-agent-id "batch.node.ubuntu 14.04"

Beispiel (Cloud-Konfiguration):

    azure batch pool create --id "pool002" --target-dedicated 1 --vm-size "small" --os-family "4"

Erstellt einen Pool von Compute-Knoten in der Batch-Dienst.

Wie in der [Batch-Features (Übersicht)](batch-api-basics.md#pool), Sie haben zwei Optionen beim Auswählen eines Betriebssystems für die Knoten im Pool: **Konfiguration des virtuellen Computers** und **Cloud Dienste**. Verwenden der `--image-*` Optionen zur Konfiguration des virtuellen Computers Pools erstellen und `--os-family` Pools Cloud Services-Konfiguration zu erstellen. Angeben `--os-family` und `--image-*` Optionen.

Sie können Pools [Anwendungspakete](batch-application-packages.md) und die Befehlszeile für eine [Aufgabe zu starten](batch-api-basics.md#start-task). Um Ressourcendateien für die Start-Aufgabe angeben, müssen Sie jedoch stattdessen eine [JSON-Datei](#json-files)verwenden.

Löschen eines Pools mit:

    azure batch pool delete [pool-id]

>[AZURE.TIP] Überprüfen der [Liste von Images virtueller Maschinen](batch-linux-nodes.md#list-of-virtual-machine-images) geeignete Werte für die `--image-*` Optionen.

## <a name="create-a-job"></a>Erstellen Sie einen Auftrag

Syntax:

    azure batch job create [options] [json-file]

Beispiel:

    azure batch job create --id "job001" --pool-id "pool001"

Einen Auftrag Batch-Konto und den Pool auf dem Aufgaben ausgeführt.

Löschen eines Auftrags mit:

    azure batch job delete [job-id]

## <a name="list-pools-jobs-tasks-and-other-resources"></a>Pools, Projekte, Aufgaben und anderen Ressourcen auflisten

Jeder Batch Ressourcentyp unterstützt eine `list` Befehl Batch-Konto Abfragen und Ressourcen dieses Typs enthält. Beispielsweise können Sie die Pools Ihr Konto und die Aufgaben eines Projekts auflisten:

    azure batch pool list
    azure batch task list --job-id "job001"

### <a name="listing-resources-efficiently"></a>Ressourcen auflisten effizient

Für schnellere Abfragen können Sie ** **Wählen**, **Filter**und** Klausel-Optionen für `list` Operationen. Mit diesen Optionen können der Batch-Dienst zurückgegebene Datenmenge begrenzen. Da alle serverseitigen eintritt, schneidet nur die Daten, die Sie interessieren die Leitung. Diese Klauseln mit der Bandbreite sparen (und daher Zeit) Wenn Sie Liste Vorgänge durchführen.

Beispielsweise wird nur Pools zurückgegeben, deren Ids mit "RenderTask" beginnen:

    azure batch task list --job-id "job001" --filter-clause "startswith(id, 'renderTask')"

Die Stapelverarbeitung CLI unterstützt alle drei Klauseln von Batch-Dienst unterstützt:

* `--select-clause [select-clause]`Eine Teilmenge der Eigenschaften für jede Entität zurückzugeben
* `--filter-clause [filter-clause]`Nur Personen, die mit dem angegebenen OData Ausdruck übereinstimmenden zurück
* `--expand-clause [expand-clause]`Die Entitätsinformationen im zugrunde liegenden REST Aufruf zu erhalten. Die erweitern-Klausel unterstützt nur die `stats` -Eigenschaft zu diesem Zeitpunkt.

Informationen zu den drei Klauseln und Abfragen ausführen Listen mit finden Sie unter [Azure Batch-Dienst effizient Abfragen](batch-efficient-list-queries.md).

## <a name="application-package-management"></a>Verwalten von Paketen

Pakete können vereinfachte Anwendung Computeknoten in Pools bereitstellen. Mit CLI Azure können Sie Pakete hochladen, Versionen verwalten und Pakete löschen.

Erstellen einer neuen Anwendung und fügen Sie ein Paket:

**Erstellen** einer Anwendung:

    azure batch application create "resgroup001" "batchaccount001" "MyTaskApplication"

**Hinzufügen** eines Anwendungspakets:

    azure batch application package create "resgroup001" "batchaccount001" "MyTaskApplication" "1.10-beta3" package001.zip

**Aktivieren** des Pakets:

    azure batch application package activate "resgroup001" "batchaccount001" "MyTaskApplication" "1.10-beta3" zip

Festlegen Sie die **Standardversion** für die Anwendung:

    azure batch application set "resgroup001" "batchaccount001" "MyTaskApplication" --default-version "1.10-beta3"

### <a name="deploy-an-application-package"></a>Ein Anwendungspaket bereitstellen

Sie können eine oder mehrere Anwendungspakete für die Bereitstellung, beim Erstellen eines neuen Pools. Wenn Sie ein Paket zur Erstellungszeit Pool angeben, wird als Knoten Joins Pool jeden Knoten bereitgestellt. Pakete werden auch bereitgestellt, wenn ein Knoten neu gestartet oder versehen ist.

Geben Sie die `--app-package-ref` option beim Erstellen eines Pools Antrag des Pools Knoten bereitstellen, wie sie das Pool. Die `--app-package-ref` Option akzeptiert eine durch Semikolons getrennte Liste der Anwendung Ids Computeknoten bereitgestellt.

    azure batch pool create --pool-id "pool001" --target-dedicated 1 --vm-size "small" --os-family "4" --app-package-ref "MyTaskApplication"

Beim Erstellen eines Pools mit Befehlszeilenoptionen kann nicht derzeit *die* Anwendung Paketversion auf Compute-Knoten, z. B. "1,10-beta3" bereitstellen soll. Daher müssen Sie zunächst eine Standardversion für die Anwendung angeben `azure batch application set [options] --default-version <version-id>` Pool erstellen (siehe vorherigen Abschnitt). Wenn Pool Erstellen einer [JSON-Datei](#json-files) anstelle von Befehlszeilenoptionen verwenden, können Sie jedoch eine Paketversion für den Pool angeben.

Weitere Informationen über Pakete finden in [Bereitstellung mit Azure Batch-Anwendungspaketen](batch-application-packages.md).

>[AZURE.IMPORTANT] [Link Azure Storage-Konto](#linked-storage-account-autostorage) müssen Sie Ihrem Konto Batch Pakete verwenden.

### <a name="update-a-pools-application-packages"></a>Aktualisieren des Pools Anwendungspakete

Aktualisieren Sie die Anwendung zu einem vorhandenen Pool zugewiesen Ausstellen der `azure batch pool set` Befehl mit der `--app-package-ref` Option:

    azure batch pool set --pool-id "pool001" --app-package-ref "MyTaskApplication2"

Bereitstellung neuer Anwendungspaket Knoten bereits in einem vorhandenen Pool berechnen müssen Sie neu starten oder image Knoten:

    azure batch node reboot --pool-id "pool001" --node-id "tvm-3105992504_1-20160930t164509z"

>[AZURE.TIP] Sie erhalten eine Liste der Knoten in einem Pool zusammen mit ihren Knoten-Ids mit `azure batch node list`.

Beachten Sie, dass Sie die Anwendung mit dem vor der Bereitstellung eine Standardversion konfiguriert sein müssen (`azure batch application set [options] --default-version <version-id>`).

## <a name="troubleshooting-tips"></a>Tipps zur Problembehandlung

Dieser Abschnitt soll Sie mit Ressourcen Azure CLI-Problemen. Werden nicht unbedingt alle Probleme lösen, aber kann Ihnen die Ursache eingrenzen, und zeigen Sie Ressourcen.

* Mit `-h` zu **Hilfetext** für jeden CLI-Befehl

* Mit `-v` und `-vv` **ausführliche** Ausgabe des Befehls, anzeigen `-vv` "extra" Ausführliche und zeigt die tatsächliche REST Anfragen und Antworten. Diese Optionen sind praktisch voll Ausgaben.

* **Ausgabe des Befehls als JSON** Anzeigen der `--json` Option. Beispielsweise `azure batch pool show "pool001" --json` pool001 Eigenschaften im JSON-Format angezeigt. Sie können kopieren und ändern diese Ausgabe mit einem `--json-file` (siehe [JSON-Dateien](#json-files) in diesem Artikel).

* [Batch-Forum auf] [ batch_forum] ist eine große Hilfe und Batch-Teammitglieder überwacht. Achten Sie darauf, dass Sie Ihre Fragen es buchen, wenn Sie Probleme oder Hilfe zu einem bestimmten Vorgang.

* Nicht jeder Ressource Batchvorgang wird derzeit von der Azure-CLI unterstützt. Beispielsweise können nicht Sie zurzeit eine Anwendung Paket *Version* für einen Pool die Paket-ID angeben In solchen Fällen möglicherweise liefern ein `--json-file` für den Befehl anstelle von Befehlszeilenoptionen. Achten Sie darauf, dass die neueste Version der CLI zukünftige Weiterentwicklungen abholen bleiben.

## <a name="next-steps"></a>Nächste Schritte

*  Finden Sie unter [Bereitstellung mit Azure Batch Anwendung](batch-application-packages.md) wie diese Funktion mit der Verwaltung und Bereitstellung von Applikationen auf Compute-Knoten Batch ausführen.

* Siehe [Batch-Dienst effizient Abfragen](batch-efficient-list-queries.md) mehr verringern die Anzahl der Elemente und Informationen, die für Abfragen Stapel zurückgegeben wird.

[batch_forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_readme]: https://github.com/Azure/azure-xplat-cli/blob/dev/README.md
[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx