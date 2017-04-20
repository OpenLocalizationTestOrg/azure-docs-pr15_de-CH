<properties
    pageTitle="Einfache Anwendung und Verwaltung in Azure Batch | Microsoft Azure"
    description="Mit Paketen Anwendungsfeature Azure Stapels zum Verwalten mehrerer Applikationen und Versionen auf Batch compute-Knoten."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/21/2016"
    ms.author="marsma" />

# <a name="application-deployment-with-azure-batch-application-packages"></a>Bereitstellung mit Azure Batch-Anwendungspakete

Die Pakete Anwendungsfunktion von Azure Batch bietet einfaches Management Task-Applikationen und deren Bereitstellung auf Compute-Knoten im Pool. Mit Anwendung können Sie laden und mehrere Versionen der Programme, die Ihre Aufgaben ausführen, einschließlich ihre unterstützenden Dateien. Sie können automatisch eine oder mehrere dieser Anwendung auf den Compute-Knoten im Pool bereitstellen.

In diesem Artikel lernen Sie hochladen und Anwendungspakete im Azure-Portal verwalten. Dann lernen Sie installieren auf einem Pool Compute-Knoten mit [Batch.NET] [ api_net] Bibliothek.

> [AZURE.NOTE] Die hier beschriebene Anwendung Pakete Funktion ersetzt die Funktion "Batch Apps" in früheren Versionen des Dienstes.

## <a name="application-package-requirements"></a>Paketanforderungen

[Link Azure Storage-Konto](#link-a-storage-account) müssen Sie Ihrem Konto Batch Pakete verwenden.

Die in diesem Artikel beschriebenen Pakete Anwendungsfunktion ist kompatibel *nur* mit Batch, die nach dem 10. März 2016 erstellt wurden. Pakete werden nicht berechnen Knoten vor diesem Datum erstellten Pools bereitgestellt werden.

Dieses Feature wurde in [Batch REST API] [ api_rest] Version 2015 12 01.2.2 und den entsprechenden [Stapel .NET] [ api_net] Library Version 3.1.0. Es wird empfohlen, immer die neueste Version der API verwenden, bei der Arbeit mit.

> [AZURE.IMPORTANT] Derzeit unterstützt nur *CloudServiceConfiguration* Pools Anwendungspakete. Pakete können keine Pools mit VirtualMachineConfiguration Bildern erstellt. Abschnitt der [Konfiguration des virtuellen Computers](batch-linux-nodes.md#virtual-machine-configuration) [Bestimmung Linux Computeknoten in Azure Batch Pools](batch-linux-nodes.md) weitere zwei verschiedenen Konfigurationen.

## <a name="about-applications-and-application-packages"></a>Applikationen und Anwendungspakete

In Azure Batch bezieht sich eine *Anwendung* auf versionierte Binärdateien auf den Compute-Knoten im Pool automatisch heruntergeladen werden kann. Ein *Anwendungspaket* bezieht sich auf einen *bestimmten Satz* die Binärdateien und einer bestimmten *Version* der Anwendung darstellt.

![Allgemeine Darstellung Applikationen und Anwendungspakete][1]

### <a name="applications"></a>Applikationen

Eine Anwendung im Stapel enthält einen oder mehr Anwendung verpackt und Konfigurationsoptionen für die Anwendung angibt. Eine Anwendung kann beispielsweise die Standard-Paket Anwendungsversion installiert Compute-Knoten und Pakete, die aktualisiert oder gelöscht werden können.

### <a name="application-packages"></a>Anwendungspakete

Der Antrag ist eine ZIP-Datei, die Binärdateien der Anwendung enthält, und Hilfsdateien, die zur Ausführung Ihrer Aufgaben erforderlich sind. Jedes Anwendungspaket stellt eine bestimmte Version der Anwendung.

Sie können Pakete Pool und Aufgabe. Wenn ein Pool oder eine Aufgabe erstellen, können Sie eine oder mehrere der folgenden Pakete und (optional) eine angeben.

* **Pool-Anwendungspakete** sind für *jeden* Knoten im Pool bereitgestellt. Applikationen werden bereitgestellt, wenn ein Knoten einen Pool Beitritt und neu gestartet oder versehen.

    Pool-Anwendungspakete sind geeignet, wenn alle Knoten in einem Pool ein Aufgaben ausführen. Sie können mindestens Anwendungspakete beim Pool erstellen und hinzufügen oder ein vorhandenes Pool Paketen angeben. Wenn Sie einem vorhandenen Pool Pakete aktualisieren, müssen Sie die Knoten, um das neue Paket installieren starten.

* **Task-Anwendungspakete** sind nur für eine Aufgabe ausgeführt wird, vor der Ausführung der Taskbefehlszeile Compute-Knoten bereitgestellt. Wenn das angegebene Anwendungspaket und Version bereits auf dem Knoten, nicht bereitgestellt, und das vorhandene Paket verwendet.

    Aufgabenpakete eignen sich freigegebene Pool Umgebungen, wo verschiedene Aufträge in einem Pool ausgeführt und Pool wird nicht gelöscht, wenn ein Auftrag abgeschlossen wurde. Hat Ihre Arbeit weniger Aufgaben als Knoten im Pool, können Aufgabe Anwendungspakete Datenübertragung minimieren, da Bereitstellung der Anwendung auf die Knoten, die Aufgaben ausführen.

    Andere Szenarien, die Aufgabenpakete profitieren werden Aufträge mit einer besonders großen Anwendung jedoch nur wenige Aufgaben. Eine Phase vor der Verarbeitung oder eine Zusammenführung Aufgabe Vorbehandlung oder Merge Anwendung schwer ist.

> [AZURE.IMPORTANT] Es sind begrenzte Anzahl von Programmen und Anwendungspakete Batch-Konto sowie die maximale Paketgröße. Details zu diesen Grenzen finden Sie unter [Kontingente und Grenzwerte für Azure Batch-Dienst](batch-quota-limit.md) .

### <a name="benefits-of-application-packages"></a>Vorteile von Anwendungspaketen

Anwendungspakete können vereinfachen Sie den Code in der Projektmappe Batch und senken den Aufwand der Anwendung verwalten, die Ihre Aufgaben ausführen.

Start-Aufgabe des Pools muss Geben Sie eine lange Liste von einzelnen Ressourcendateien auf Knoten installieren. Sie müssen nicht mehrere Versionen von Anwendungsdateien in Azure Storage oder Knoten manuell verwalten. Und Sie brauchen sich mit [SAS-URLs](../storage/storage-dotnet-shared-access-signature-part-1.md) für den Zugriff auf die Dateien in das Speicherkonto generieren. Stapel wird im Hintergrund mit Azure Storage Anwendungspakete speichern und bereitstellen, um Knoten zu berechnen.

## <a name="upload-and-manage-applications"></a>Upload und Verwalten von Applikationen

[Azure-Portal] können[ portal] oder [Batch Management.NET](batch-management-dotnet.md) Library Anwendungspakete im Batch Konto verwalten. In den nächsten Abschnitten wir verknüpfen ein Speicherkonto zunächst besprechen Hinzufügen von Applikationen und und mit dem Portal verwalten.

### <a name="link-a-storage-account"></a>Ein Speicherkonto verknüpfen

Um Pakete zu verwenden, müssen Sie zuerst ein Azure Storage-Konto Batch-Konto verknüpfen. Wenn ein Speicherkonto für die Batch-Konto noch nicht konfiguriert haben, zeigt das Azure-Portal eine Warnung erstmals der **Programme** untereinander Blade- **Batch-Konto** klicken.

> [AZURE.IMPORTANT] Batch derzeit unterstützt *nur* **Allgemeine** Speicher-Kontotyp wie in Schritt 5 [erstellen ein Speicherkonto](../storage/storage-create-storage-account.md#create-a-storage-account) [zum Azure-Speicherkonten](../storage/storage-create-storage-account.md)beschrieben. Wenn ein Azure Storage-Konto mit der Batch-Konto verknüpfen *nur* eine **Allgemeine** Speicher-Konto verknüpfen.

![Keine Speicherkonto Warnung in Azure-Portal konfiguriert][9]

Der Batch-Dienst verwendet zugeordnete Speicherkonto für die Speicherung und den Abruf von Anwendungspaketen. Nachdem Sie die beiden Konten verknüpft haben, kann Stapel automatisch gespeicherte Konto verknüpfte Speicher Compute-Knoten Pakete bereitstellen. Klicken Sie **Storage-Konto** auf die **Warnung** und dann auf **Konto** auf das **Speicherkonto** ein Speicherkonto Batch-Konto verknüpfen.

![Wählen Sie Konto Blatt Speicher in Azure-portal][10]

Wir empfehlen eine Storage-Konto *speziell* für die Verwendung mit Ihrem Konto Batch erstellen und hier auswählen. Ausführliche Informationen zum Erstellen eines Speicherkontos finden Sie unter "ein Speicherkonto [zu Azure Storage](../storage/storage-create-storage-account.md)-Konten erstellen". Nach dem Erstellen ein Speicherkonto können Sie dann es Batch-Konto verknüpfen mit dem **Konto** .

> [AZURE.WARNING] Da Batch Azure-Speicher Ihr Anwendungspakete Speichern verwendet, werden Sie [als normale] [ storage_pricing] für BLOB-Daten blockieren. Achten Sie darauf, sollten die Größe und Anzahl der Anwendungspakete und regelmäßig veraltete Pakete, um Kosten zu minimieren.

### <a name="view-current-applications"></a>Aktuellen Programme anzeigen

Klicken Sie die Anwendung in Ihrem Konto Batch **Applikationen** Menüelement im linken Menü beim Blade **Batch-Konto** anzeigen.

![Applikationen Kachel][2]

**Applikationen** Blade wird geöffnet:

![Liste Applikationen][3]

**Applikationen** Blade stellt jede Anwendung in Ihrem Konto und die folgenden Eigenschaften:

* **Pakete**- die Anzahl der Versionen dieser Anwendung zugeordnet.
* **Standard-Version**- die Version, die installiert werden, wenn Sie eine Version nicht angeben, wenn Sie die Anwendung für einen Pool festlegen. Diese Einstellung ist optional.
* **Updates zulassen**– der Wert, der angibt, ob Paket aktualisiert, löschen und hinzufügen dürfen. Wenn auf **Nein**festgelegt ist, sind für die Anwendung Paket-Updates und löschen deaktiviert. Nur neue Anwendung Paketversionen können hinzugefügt werden. Die Standardeinstellung ist **Ja**.

### <a name="view-application-details"></a>Bewerbungsdetails anzeigen

Klicken Sie auf eine Anwendung **Applikationen** Blatt das Blade öffnen, das die Details für diese Anwendung enthält.

![Bewerbungsdetails][4]

Blatt Details Anwendung können Sie Folgendes für die Anwendung konfigurieren.

* **Allow Updates**angeben, ob Anwendungspakete, die aktualisiert oder gelöscht werden. In diesem Artikel finden Sie unter "Aktualisieren oder löschen ein Anwendungspakets".
* **Standardversion**– Geben Sie ein Standard-Anwendungspaket bereitstellen, um Knoten zu berechnen.
* **Anzeigename**– Geben Sie einen "Anzeigenamen" name des Stapels Lösung können verwendet werden, wenn Informationen, wie in der Benutzeroberfläche eines Dienstes angezeigt, die Ihre durch Batch Kunden.

### <a name="add-a-new-application"></a>Hinzufügen einer neuen Anwendung

Erstellen einer neuen Anwendung, Hinzufügen eines Anwendungspakets und eine neuen, eindeutigen ID angeben. Erste Anwendungspaket mit der neuen Anwendung ID hinzufügen wird auch die neue Anwendung erstellen.

Klicken Sie auf **Hinzufügen** , auf die **Applikationen** Blade **neue Anwendung** geöffnet.

![Neue Anwendung Blade in Azure-portal][5]

**Neue Anwendung** Blade enthält die folgenden Felder der neuen Anwendung und Anwendungspaket Einstellungen.

**Id der Anwendung**

Dieses Feld gibt die ID der neuen Anwendung die standard Azure Stapel ID Validierungsregeln:

* Kann jede Kombination von alphanumerischen Zeichen, einschließlich der Bindestriche und Unterstriche enthalten.
* Kann nicht mehr als 64 Zeichen enthalten.
* Muss innerhalb der Batch-Konto eindeutig sein.
* Wird beibehalten von Groß- und Kleinschreibung.

**Version**

Gibt die Version der Anwendungspaket, das Sie hochladen. Zeichenfolgen gelten die folgenden Validierungsregeln:

* Kann eine beliebige Kombination von alphanumerischen Zeichen, einschließlich Bindestriche, Unterstriche und Punkte enthalten.
* Kann nicht mehr als 64 Zeichen enthalten.
* Muss innerhalb der Anwendung eindeutig sein.
* Fall beibehalten und Kleinschreibung.

**Anwendungspaket**

Dieses Feld gibt die ZIP-Datei, die die Binärdateien der Anwendung enthält und die unterstützenden Dateien, die zum Ausführen der Anwendungdes erforderlich sind. Klicken Sie im Feld **Wählen Sie eine Datei** oder das Ordnersymbol, wählen eine ZIP-Datei mit Dateien der Anwendung.

Nach Auswahl eine Datei klicken Sie auf **OK** , um Uploads auf Azure Storage beginnen. Wenn der Uploadvorgang abgeschlossen ist, werden Sie benachrichtigt und das Blade wird geschlossen. Die Größe der Datei, die Sie hochladen und die Geschwindigkeit der Netzwerkverbindung kann dieser Vorgang einige Zeit dauern.

> [AZURE.WARNING] Schließen Sie **neue Anwendung** Blade nicht, bevor der Uploadvorgang abgeschlossen ist. Dadurch wird der Ladevorgang beendet.

### <a name="add-a-new-application-package"></a>Hinzufügen eines neuen Anwendungspakets

Um eine neue Version der Anwendung Paket einer vorhandenen Anwendung hinzuzufügen, wählen Sie eine Anwendung in der Blade- **Applikationen** , **Pakete**, klicken **Hinzufügen** , um das Blade **Paket hinzufügen** öffnen.

![Anwendung Paket Blade in Azure-Portal hinzufügen][8]

Wie Sie sehen können, die Felder entsprechen der **neuen Anwendung** Blade, aber Feld **Id der Anwendung** deaktiviert. Wie für die neue Anwendung geben Sie die **Version** für das neue Paket **Anwendungspaket** ZIP-Datei suchen Sie und klicken Sie auf **OK** , um das Paket hochladen.

### <a name="update-or-delete-an-application-package"></a>Aktualisieren oder Löschen eines Anwendungspakets

Zum Aktualisieren oder Löschen einer vorhandenen Anwendungspaket, öffnen Sie Blade Einzelheiten für die Anwendung zu, klicken Sie auf **Pakete** **Pakete** Blade zu öffnen, klicken Sie auf die **Auslassungszeichen** in der Zeile der zu ändernden Anwendungspaket und wählen Sie die gewünschte Aktion.

![Aktualisieren oder Löschen von Paket in Azure-portal][7]

**Update**

Durch Klicken auf **Aktualisieren**wird das *Updatepaket* Blade angezeigt. Dieses Blatt ähnelt das Blade *neue Anwendungspaket* jedoch im Auswahlfeld Paket aktiviert ist, sodass Sie eine neue ZIP-Datei zum Hochladen angeben.

![Update-Paket Blade in Azure-portal][11]

**Löschen**

**Löschen**können Sie werden aufgefordert, das Löschen der Paketversion bestätigen, als Stapelverarbeitung löscht das Paket aus dem Azure-Speicher. Die Standardversion von einer Anwendung löschen, wird die **standardmäßige** Einstellung für die Anwendung entfernt.

![Anwendung löschen][12]

## <a name="install-applications-on-compute-nodes"></a>Installieren von Clientanwendungen auf Compute-Knoten

Nun, man Pakete mit Azure-Portal verwalten besprechen wir bereitstellen, um compute-Knoten und mit Stapelverarbeitungsaufgaben ausgeführt.

### <a name="install-pool-application-packages"></a>Pool Pakete installieren

Installieren ein Anwendungspakets auf alle Computeknoten in einem Pool Geben Sie mindestens eine Anwendung Paket *Verweise* für den Pool an. Anwendungspakete, die für einen Pool angeben werden auf jeder Compute-Knoten installiert, wenn dieser Knoten Pool verknüpft und Knoten neu gestartet oder versehen ist.

Batch .NET geben mindestens [CloudPool][net_cloudpool]. [ApplicationPackageReferences] [net_cloudpool_pkgref] beim Erstellen eines neuen Pools und einem vorhandenen Pool. [ApplicationPackageReference] [ net_pkgref] Klasse gibt eine Anwendung ID und Version in eines Pools installiert compute-Knoten.

```csharp
// Create the unbound CloudPool
CloudPool myCloudPool =
    batchClient.PoolOperations.CreatePool(
        poolId: "myPool",
        targetDedicated: "1",
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Specify the application and version to install on the compute nodes
myCloudPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "litware",
        Version = "1.1001.2b" }
};

// Commit the pool so that it's created in the Batch service. As the nodes join
// the pool, the specified application package will be installed on each.
await myCloudPool.CommitAsync();
```

>[AZURE.IMPORTANT] Die Bereitstellung einer Anwendung Paket aus irgendeinem Grund fehlschlägt, Batch-Dienst der Knoten [nicht mehr]markiert[net_nodestate], und keine Aufgaben zur Ausführung auf diesem Knoten geplant. In diesem Fall sollten Sie die Bereitstellung des Pakets dieses Knoten **neu starten** . Neustarten des Knotens können auch Taskplanung auf dem Knoten erneut.

### <a name="install-task-application-packages"></a>Aufgabenpakete installieren

Ähnlich einem Pool Geben Sie Anwendung Paket *Verweise* für einen Vorgang. Wenn eine Aufgabe auf einem Knoten ausgeführt werden soll, wird das Paket heruntergeladen und extrahiert vor Befehlszeile der Task ausgeführt wird. Wenn eine angegebene Paket und eine Version ist bereits auf dem Knoten installiert, das Paket nicht heruntergeladen, und das vorhandene Paket verwendet.

Um eine Aufgabe Anwendungspaket installieren, konfigurieren die Aufgabe [CloudTask][net_cloudtask]. [ApplicationPackageReferences] [net_cloudtask_pkgref] Eigenschaft:

```csharp
CloudTask task =
    new CloudTask(
        "litwaretask001",
        "cmd /c %AZ_BATCH_APP_PACKAGE_LITWARE%\\litware.exe -args -here");

task.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference
    {
        ApplicationId = "litware",
        Version = "1.1001.2b"
    }
};
```

## <a name="execute-the-installed-applications"></a>Die installierten Programme ausführen

Pakete, die für einen Pool oder Aufgabe angegeben wurden heruntergeladen und extrahiert, ein benanntes Verzeichnis innerhalb der `AZ_BATCH_ROOT_DIR` des Knotens. Batch erstellt auch eine Umgebungsvariable mit dem Pfad zu dem benannten Verzeichnis. Ihre Aufgabe Befehlszeilen verwenden diese Umgebungsvariable, wenn auf den Knoten verweisen. Die Variable ist im folgenden Format:

`AZ_BATCH_APP_PACKAGE_APPLICATIONID#version`

`APPLICATIONID`und `version` sind Werte, die die Version der Anwendung und Paket entsprechen Sie für die Bereitstellung angegeben haben. Beispielsweise sollte angegebene Anwendung *Mixer* , Version 2.7 installiert, Ihre Aufgabe Befehlszeilen verwenden diese Umgebungsvariable auf seine Dateien zugreifen:

`AZ_BATCH_APP_PACKAGE_BLENDER#2.7`

Wenn Sie eine Standardversion für eine Anwendung angeben, können Sie das Versionssuffix weglassen. Beispielsweise "2.7" als Standardversion für Anwendung *Mixer*festlegen, können Ihre Aufgaben folgende Umgebungsvariable und führt sie Version 2.7:

`AZ_BATCH_APP_PACKAGE_BLENDER`

Der folgende Codeausschnitt zeigt ein Beispiel Taskbefehlszeile, die Standardversion *Mixer* -Anwendung gestartet wird:

```csharp
string taskId = "blendertask01";
string commandLine =
    @"cmd /c %AZ_BATCH_APP_PACKAGE_BLENDER%\blender.exe -args -here";
CloudTask blenderTask = new CloudTask(taskId, commandLine);
```

> [AZURE.TIP] [Umgebung-Einstellungen für Vorgänge](batch-api-basics.md#environment-settings-for-tasks) im [Batch Featureübersicht](batch-api-basics.md) Weitere Compute-Knoten umgebungseinstellungen anzeigen

## <a name="update-a-pools-application-packages"></a>Aktualisieren des Pools Anwendungspakete

Wenn ein vorhandenen Pool mit Antrag bereits konfiguriert wurde, können Sie ein neues Paket für den Pool. Wenn Sie einen neuen Paket Verweis für ein Pool gilt Folgendes angeben:

* Alle neuen Knoten, die den Pool hinzugefügt und alle vorhandenen Knoten, der neu gestartet oder versehen, werden neu angegebene Paket installiert.
* Compute-Knoten, die bereits im Pool beim Aktualisieren der Paketverweise neue Anwendungspaket nicht automatisch installiert. Diese Berechnung Knoten müssen neu gestartet oder um neue Paket versehen.
* Bei der Bereitstellung eines neuen Pakets entsprechend erstellte Umgebungsvariablen neue Anwendung Paket verweisen.

In diesem Beispiel hat das vorhandene Pool Version 2.7 *Mixer* Anwendung als eines der [CloudPool][net_cloudpool]. [ApplicationPackageReferences] [net_cloudpool_pkgref]. Um den Pool Knoten mit Version 2.76b zu aktualisieren, werden neue [ApplicationPackageReference] [ net_pkgref] mit der neuen Version und Commit der Änderung.

```csharp
string newVersion = "2.76b";
CloudPool boundPool = await batchClient.PoolOperations.GetPoolAsync("myPool");
boundPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "blender",
        Version = newVersion }
};
await boundPool.CommitAsync();
```

Da die neue Version konfiguriert wurde, haben *neue* Knoten, der den Pool verknüpft Version 2.76b bereitgestellt. 2.76b der Knoten zu installieren, die *bereits* im Pool starten oder sie ein neues Abbild. Beachten Sie, dass Knoten neu gestartet Dateien aus vorherigen Paket Bereitstellung behalten.

## <a name="list-the-applications-in-a-batch-account"></a>Liste der Anwendung in einem Batch-Konto

Liste von der Anwendung und ihre Pakete in einem Batch-Konto mit [ApplicationOperations][net_appops]. [ListApplicationSummaries] [net_appops_listappsummaries] Methode.

```csharp
// List the applications and their application packages in the Batch account.
List<ApplicationSummary> applications = await batchClient.ApplicationOperations.ListApplicationSummaries().ToListAsync();
foreach (ApplicationSummary app in applications)
{
    Console.WriteLine("ID: {0} | Display Name: {1}", app.Id, app.DisplayName);

    foreach (string version in app.Versions)
    {
        Console.WriteLine("  {0}", version);
    }
}
```

## <a name="wrap-up"></a>Zusammenfassung

Mit Anwendung unterstützen Sie Ihre Kunden wählen die Anwendung für ihre Arbeit und geben Sie die genaue Version beim Aufträge mit der Stapelverarbeitung aktivierte Dienst verarbeiten. Sie können auch ermöglichen Ihren Kunden hochladen und verfolgen ihre eigenen Programme in Ihrem Dienst.

## <a name="next-steps"></a>Nächste Schritte

* Die [Stapelverarbeitung REST API] [ api_rest] bietet zudem Unterstützung Anwendungspakete arbeiten. Siehe z. B. [ApplicationPackageReferences] [ rest_add_pool_with_packages] Element in [einem Pool ein Konto hinzufügen] [ rest_add_pool] Informationen wie Pakete über die REST-API zu installieren. [Applikationen] finden Sie unter[ rest_applications] Informationen zu Informationen mithilfe der Stapelverarbeitung REST-API.

* Erfahren Sie, wie programmgesteuert [Azure Batch Konten und Kontingente mit Batch Management verwaltet](batch-management-dotnet.md). [Batch Management.NET] [ api_net_mgmt] Bibliothek können Konto erstellen und Löschen von Funktionen für die Batch-Anwendung oder Dienst.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[github_samples]: https://github.com/Azure/azure-batch-samples
[storage_pricing]: https://azure.microsoft.com/pricing/details/storage/
[net_appops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.aspx
[net_appops_listappsummaries]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.listapplicationsummaries.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.applicationpackagereferences.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.aspx
[net_cloudtask_pkgref]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.applicationpackagereferences.aspx
[net_nodestate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.state.aspx
[net_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationpackagereference.aspx
[portal]: https://portal.azure.com
[rest_applications]: https://msdn.microsoft.com/library/azure/mt643945.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_pool_with_packages]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_apkgreference

[1]: ./media/batch-application-packages/app_pkg_01.png "Allgemeine Pakete Anwendungsdiagramm"
[2]: ./media/batch-application-packages/app_pkg_02.png "Applikationen untereinander Azure-portal"
[3]: ./media/batch-application-packages/app_pkg_03.png "Applikationen Blade in Azure-portal"
[4]: ./media/batch-application-packages/app_pkg_04.png "Anwendung Details Blade in Azure-portal"
[5]: ./media/batch-application-packages/app_pkg_05.png "Neue Anwendung Blade in Azure-portal"
[7]: ./media/batch-application-packages/app_pkg_07.png "Aktualisieren oder Löschen von Paketen Dropdown in Azure-portal"
[8]: ./media/batch-application-packages/app_pkg_08.png "Neue Anwendung Paket Blade in Azure-portal"
[9]: ./media/batch-application-packages/app_pkg_09.png "Keine verknüpften Speicher Konto-Warnung"
[10]: ./media/batch-application-packages/app_pkg_10.png "Wählen Sie Konto Blatt Speicher in Azure-portal"
[11]: ./media/batch-application-packages/app_pkg_11.png "Update-Paket Blade in Azure-portal"
[12]: ./media/batch-application-packages/app_pkg_12.png "Paket Dialogfeld zur Bestätigung in Azure-portal"
