<properties
 pageTitle="Exportieren von IoT Hub Gerät Identitäten importieren | Microsoft Azure"
 description="Konzepte und .NET Codeausschnitte für Massenkopieren Management IoT Hub Gerät Identitäten"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/05/2016"
 ms.author="dobett"/>

# <a name="bulk-management-of-iot-hub-device-identities"></a>Massenkopieren Sie Verwaltung IoT Hub Gerät Identitäten

Jede IoT Hub ist ein geräteidentitätsregistrierung, mit Ressourcen pro Gerät im Dienst wie eine Warteschlange erstellen, Flight Cloud-zu-Gerät-Nachrichten enthält. Die Registrierung des Geräts Identität können Sie Zugriff auf Gerät verbundenen Endpunkte steuern. Dieser Artikel beschreibt das Importieren und Exportieren einer Identität Registrierung Gerät Gerät Identitäten gebündelt.

Importieren und Exportieren von Operationen statt im Kontext der *Aufträge* , die Massenvorgänge gegen IoT Hub Service ausführen können.

**RegistryManager** -Klasse enthält die **ExportDevicesAsync** und **ImportDevicesAsync** -Methoden das **Job** -Framework verwenden. Diese Methoden können Sie exportieren, importieren und synchronisieren die gesamte Registrierung ein IoT Hub-Geräts.

## <a name="what-are-jobs"></a>Was sind Projekte?

Gerät Identität Registrierungsvorgängen verwenden **das Auftragssystem** bei der Operation:

*  Potenziell Ausführung lange im Vergleich zu standard-RCWs Vorgänge hat oder
*  Eine große Menge von Daten zurückgegeben an den Benutzer.

In diesen Fällen statt einer einzigen API-Aufruf warten oder blockierend auf das Ergebnis der Operation erstellt der Vorgang asynchron einen **Auftrag** für diesen IoT Hub. Der Vorgang dann zurückgegeben sofort ein **JobProperties** -Objekt.

Der folgende C#-Codeausschnitt zeigt, wie ein Exportvorgangs erstellen:

```
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);
```

Die **RegistryManager** -Klasse können Sie dann den **Auftrag** zurückgegebene **JobProperties** -Metadaten Abfragen.

Der folgende C#-Codeausschnitt zeigt, wie alle fünf Sekunden abgefragt, um festzustellen, ob der Auftrag ausgeführt wurde:

```
// Wait until job is finished
while(true)
{
  exportJob = await registryManager.GetJobAsync(exportJob.JobId);
  if (exportJob.Status == JobStatus.Completed || 
      exportJob.Status == JobStatus.Failed ||
      exportJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="export-devices"></a>Geräte exportieren

Verwenden Sie die **ExportDevicesAsync** -Methode die gesamte Registrierung ein IoT Hub-Geräts ein [Azure](https://azure.microsoft.com/documentation/services/storage/) BLOB-Speichercontainer mithilfe [SAS](https://msdn.microsoft.com/library/ee395415.aspx)exportieren.

Diese Methode können Sie zuverlässige Backups die Geräteinformationen in einem BLOB-Container erstellen, die Sie steuern.

**ExportDevicesAsync** -Methode erfordert zwei Parameter:

*  Eine *Zeichenfolge* mit URI BLOB-Container. Dieser URI muss ein SAS-Token, die Schreibzugriff auf den Container gewährt. Der Auftrag wird ein in diesem Container zum Speichern der serialisierten Export Gerätedaten erstellt. Das SAS-Token muss diese Berechtigungen enthalten:
    
    ```
    SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
    ```

*  Ein *boolean* , der angibt, ob die Exportdaten Authentifizierungsschlüssel ausgeschlossen werden soll. Wenn **falsche**Schlüssel im Export enthalten Authentifizierung ausgeben; Andernfalls werden Schlüssel als **null**exportiert.

Der folgende C#-Codeausschnitt zeigt, wie ein Projekt exportieren, das in den Exportvorgang Gerät Authentifizierungsschlüssel enthält und Abschluss Fragen:

```
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);

// Wait until job is finished
while(true)
{
    exportJob = await registryManager.GetJobAsync(exportJob.JobId);
    if (exportJob.Status == JobStatus.Completed || 
        exportJob.Status == JobStatus.Failed ||
        exportJob.Status == JobStatus.Cancelled)
    {
    // Job has finished executing
    break;
    }

    await Task.Delay(TimeSpan.FromSeconds(5));
}
```

Der Auftrag speichert die Ausgabe im bereitgestellten Blob-Container als Blockblob mit dem Namen **Devices.txt ein**. Die Ausgabedaten besteht aus JSON serialisiert Gerätedaten mit einem Gerät pro Zeile.

Das folgende Beispiel zeigt die Ausgabe von Daten:

```
{"id":"Device1","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device2","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device3","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device4","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device5","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
```

Benötigen Sie Zugriff auf diese Daten im Code, können diese Daten mithilfe der **ExportImportDevice** -Klasse einfach deserialisiert werden. Der folgende C#-Codeausschnitt zeigt, wie Informationen lesen, die zuvor ein exportiert wurde:

```
var exportedDevices = new List<ExportImportDevice>();

using (var streamReader = new StreamReader(await blob.OpenReadAsync(AccessCondition.GenerateIfExistsCondition(), RequestOptions, null), Encoding.UTF8))
{
  while (streamReader.Peek() != -1)
  {
    string line = await streamReader.ReadLineAsync();
    var device = JsonConvert.DeserializeObject<ExportImportDevice>(line);
    exportedDevices.Add(device);
  }
}
```

> [AZURE.NOTE]  Die **GetDevicesAsync** -Methode der **RegistryManager** -Klasse können Sie um eine Liste der Geräte zu erhalten. Dieser Ansatz hat jedoch schwer maximal 1000 auf die Anzahl der Geräteobjekte, die zurückgegeben werden. Erwartete Verwendung bei der **GetDevicesAsync** -Methode wird für Szenarien zum Debuggen zu unterstützen und sollte nicht für Produktions-Workloads.

## <a name="import-devices"></a>Geräte importieren

Die **ImportDevicesAsync** -Methode in der **RegistryManager** -Klasse können Sie Bulk Import und Synchronisierung in einem IoT Hub geräteregistrierung Operationen. Wie die **ExportDevicesAsync** -Methode verwendet die **ImportDevicesAsync** Methode **Job** -Framework.

Kümmern Sie **ImportDevicesAsync** -Methode verwenden, da neben neue Geräte in Ihrem geräteidentitätsregistrierung Bereitstellung auch aktualisieren und löschen Sie vorhandene Geräte.

> [AZURE.WARNING]  Ein Importvorgang kann nicht rückgängig gemacht werden. Sichern Sie die vorhandenen Daten mit der **ExportDevicesAsync** -Methode in einen anderen blobcontainer vor Änderungen zur Registrierung Identität Gerät immer.

**ImportDevicesAsync** -Methode akzeptiert zwei Parameter:

*  Eine *Zeichenfolge* mit URI [Azure](https://azure.microsoft.com/documentation/services/storage/) BLOB-Speichercontainer für die Verwendung als *Eingabe* für den Einzelvorgang. Dieser URI muss ein SAS-Token, die auf den Container gewährt. Container enthalten ein Blob mit dem Namen **Devices.txt ein** , die die serialisierten Daten in die geräteidentitätsregistrierung importieren. Daten importieren müssen Geräteinformationen in demselben JSON-Format enthalten, die das **ExportImportDevice** Projekt verwendet **Devices.txt ein** Blob erstellt. Das SAS-Token muss diese Berechtigungen enthalten:

    ```
    SharedAccessBlobPermissions.Read
    ```

*  Eine *Zeichenfolge* mit URI [Azure](https://azure.microsoft.com/documentation/services/storage/) Blob Behälter mit *Ausgabe* aus dem Auftrag. Das Projekt erstellt ein in diesem Container Fehlerinformationen von der abgeschlossene Import **Auftrag**speichern. Das SAS-Token muss diese Berechtigungen enthalten:
    
    ```
    SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
    ```

> [AZURE.NOTE]  Zwei Parameter können auf demselben BLOB-Container verweisen. Separate Parameter können einfach mehr Kontrolle über Ihre Daten als Ausgabe Container zusätzliche Berechtigungen erforderlich.

Der folgende C#-Codeausschnitt zeigt, wie einen Importauftrag initiieren:

```
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);
```

## <a name="import-behavior"></a>Importieren von Verhalten

**ImportDevicesAsync** -Methode können Sie in Ihrem geräteidentitätsregistrierung folgenden Massenvorgänge ausführen:

-   BULK-Registrierung neuer Geräte
-   Massenlöschungen für vorhandene Geräte
-   Massenkopieren ändert (aktivieren oder Deaktivieren von Geräten)
-   Zuweisung von neuen Gerät Authentifizierungsschlüssel Massen
-   Automatische Regenerierung Gerät Authentifizierungsschlüssel Massen

Sie können eine beliebige Kombination von vorherigen Vorgängen in **ImportDevicesAsync** einen Aufruf durchführen. Sie können beispielsweise neue Geräte registrieren und löschen oder vorhandene Geräte gleichzeitig aktualisieren. Mit der **ExportDevicesAsync** -Methode verwendet, können Sie alle Geräte vollständig aus einem IoT Hub zu einem anderen migrieren.

Verwenden Sie die optionale **ImportMode** -Eigenschaft in Serialisierung Importdaten für jedes Gerät importieren Prozess pro-steuern. Die **ImportMode** -Eigenschaft enthält die folgenden Optionen:

| importMode |  Beschreibung |
| -------- | ----------- |
| **createOrUpdate** | Wenn ein Gerät nicht mit der angegebenen **Id**vorhanden ist, wird es neu registriert. <br/>Wenn das Gerät bereits vorhanden ist, wird mit bereitgestellten Eingabedaten ungeachtet der **ETag** -Wert vorhandene Informationen überschrieben. |
| **Erstellen** | Wenn ein Gerät nicht mit der angegebenen **Id**vorhanden ist, wird es neu registriert. <br/>Wenn das Gerät bereits vorhanden ist, wird ein Fehler in die Protokolldatei geschrieben. |
| **Aktualisieren** | Wenn ein Gerät mit der angegebenen **Id**vorhanden ist, wird mit bereitgestellten Eingabedaten ungeachtet der **ETag** -Wert vorhandene Informationen überschrieben. <br/>Wenn das Gerät nicht vorhanden ist, wird ein Fehler in die Protokolldatei geschrieben. |
| **updateIfMatchETag** | Wenn ein Gerät mit der angegebenen **Id**vorhanden ist, wird Informationen nur dann, wenn eine Übereinstimmung **ETag** bereitgestellten Eingabedaten überschrieben. <br/>Wenn das Gerät nicht vorhanden ist, wird ein Fehler in die Protokolldatei geschrieben. <br/>Ein **ETag** -Konflikt besteht, ist ein Fehler in die Protokolldatei geschrieben. |
| **createOrUpdateIfMatchETag** | Wenn ein Gerät nicht mit der angegebenen **Id**vorhanden ist, wird es neu registriert. <br/>Wenn das Gerät bereits vorhanden ist, wird Informationen nur dann, wenn eine Übereinstimmung **ETag** bereitgestellten Eingabedaten überschrieben. <br/>Ein **ETag** -Konflikt besteht, ist ein Fehler in die Protokolldatei geschrieben. |
| **Löschen** | Wenn ein Gerät mit der angegebenen **Id**vorhanden ist, wird er ungeachtet der **ETag** -Wert gelöscht. <br/>Wenn das Gerät nicht vorhanden ist, wird ein Fehler in die Protokolldatei geschrieben. |
| **deleteIfMatchETag** | Wenn ein Gerät mit der angegebenen **Id**vorhanden ist, wird sie nur dann, wenn eine Übereinstimmung **ETag** gelöscht. Wenn das Gerät nicht vorhanden ist, wird ein Fehler in die Protokolldatei geschrieben. <br/>Ein ETag-Konflikt besteht, ist ein Fehler in die Protokolldatei geschrieben. |

> [AZURE.NOTE] Wenn die Serialisierungsdaten ein **ImportMode** -Flag ein nicht explizit definieren, wird standardmäßig **CreateOrUpdate** während des Importvorgangs.

## <a name="import-devices-example--bulk-device-provisioning"></a>Importieren von Geräten wird – Massen Gerät bereitstellen 

Im folgende C#-Codebeispiel veranschaulicht, wie Identitäten Geräte generieren:

- Enthalten Sie Authentifizierungsschlüssel.
- Schreiben Sie diese Informationen in ein.
- Importieren Sie Geräte in die Registrierung des Geräts Identität.

```
// Provision 1,000 more devices
var serializedDevices = new List<string>();

for (var i = 0; i < 1000; i++)
{
// Create a new ExportImportDevice
  var deviceToAdd = new ExportImportDevice()
  {
    Id = Guid.NewGuid().ToString(),
    Status = DeviceStatus.Enabled,
    Authentication = new AuthenticationMechanism()
    {
      SymmetricKey = new SymmetricKey()
      {
        PrimaryKey = CryptoKeyGenerator.GenerateKey(32),
        SecondaryKey = CryptoKeyGenerator.GenerateKey(32)
      }
    },
    ImportMode = ImportMode.Create
  };

  // Add device to existing list
  serializedDevices.Add(JsonConvert.SerializeObject(deviceToAdd));
}

// Write this list to the blob
var sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice => sb.AppendLine(serializedDevice));
await blob.DeleteIfExistsAsync();

using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Call import using the same blob to add new devices!
// This normally takes 1 minute per 100 devices the normal way
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="import-devices-example--bulk-deletion"></a>Importieren von Geräten Beispiel Bulk löschen

Das folgende Codebeispiel veranschaulicht die Geräte löschen über das vorherige Codebeispiel hinzugefügt:

```
// Step 1: Update each device's ImportMode to be Delete
sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice =>
{
  // Deserialize back to an ExportImportDevice
  var device = JsonConvert.DeserializeObject<ExportImportDevice>(serializedDevice);

  // Update property
  device.ImportMode = ImportMode.Delete;

  // Re-serialize
  sb.AppendLine(JsonConvert.SerializeObject(device));
});

// Step 2: Write the new import data back to the block blob
await blob.DeleteIfExistsAsync();
using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Step 3: Call import using the same blob to delete all devices!
importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}

```

## <a name="getting-the-container-sas-uri"></a>Erste Container SAS-URI


Das folgende Codebeispiel zeigt, wie ein [SAS-URI](../storage/storage-dotnet-shared-access-signature-part-2.md) mit Lese-, Schreib- und Löschberechtigungen für BLOB-Container zu:

```
static string GetContainerSasUri(CloudBlobContainer container)
{
  // Set the expiry time and permissions for the container.
  // In this case no start time is specified, so the
  // shared access signature becomes valid immediately.
  var sasConstraints = new SharedAccessBlobPolicy();
  sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
  sasConstraints.Permissions = 
    SharedAccessBlobPermissions.Write | 
    SharedAccessBlobPermissions.Read | 
    SharedAccessBlobPermissions.Delete;

  // Generate the shared access signature on the container,
  // setting the constraints directly on the signature.
  string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

  // Return the URI string for the container,
  // including the SAS token.
  return container.Uri + sasContainerToken;
}

```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt, wie Massen Operationen für die Registrierung des Geräts Identität in einem IoT Hub. Folgen Sie diesen Links, um weitere Informationen zum Verwalten von Azure IoT Hub:

- [Verwendung Metriken][lnk-metrics]
- [Überwachung von Vorgängen][lnk-monitor]

Weitere Funktionen des IoT Hub kann, finden Sie unter:

- [Das Entwicklerhandbuch][lnk-devguide]
- [Ein SDK Gateway IoT simulieren][lnk-gateway]

[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md