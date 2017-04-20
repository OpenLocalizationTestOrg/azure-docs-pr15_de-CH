## <a name="download-and-understand-the-arm-template"></a>Herunterladen und Verstehen der ARM-Vorlage

Sie können die ARM-Vorlage für eine VNet und zwei Subnetzen von Github herunterladen, ändern Sie möglicherweise möchten, und. Dazu gehen Sie unten.

1. Navigieren Sie zu [der Vorlage Beispielseite](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).
2. Klicken Sie auf **azuredeploy.json**, und klicken Sie auf **RAW**.
3. Speichern Sie die Datei auf einen lokalen Ordner auf Ihrem Computer.
4. Wenn Sie mit ARM-Vorlagen vertraut sind, fahren Sie mit Schritt 7.
5. Die gerade gespeicherte Datei öffnen und den Inhalt unter **Parameter** in Zeile 5. ARM Vorlagenparameter bieten einen Platzhalter für Werte, die während der Bereitstellung ausgefüllt werden können.

    | Parameter | Beschreibung |
    |---|---|
    | **Speicherort** | Azure-Region, in dem das VNet erstellt werden |
    | **vnetName** | Name für neue VNet |
    | **addressPrefix** | Adressraum für VNet im CIDR-format |
    | **subnet1Name** | Namen für das erste VNet |
    | **subnet1Prefix** | CIDR-Block für das erste Subnetz |
    | **subnet2Name** | Namen für die zweite VNet |
    | **subnet2Prefix** | CIDR-Block für das zweite Subnetz |

    >[AZURE.IMPORTANT] ARM-Vorlagen verwaltet Github können sich ändern. Stellen Sie sicher, dass Sie die Vorlage überprüfen, bevor Sie ihn verwenden.
    
6. Überprüfen Sie den Inhalt Resources ( **Ressourcen)** , und beachten Sie Folgendes:

    - **Typ**. Typ der Ressource, die von der Vorlage erstellt wird. In diesem Fall darstellen, **Microsoft.Network/virtualNetworks**, die ein VNet.
    - **Name**. Name der Ressource. Beachten Sie die Verwendung von **[parameters('vnetName')]**, d. h. der Name vom Benutzer oder durch eine Datei während der Bereitstellung als Eingabe bereitgestellt.
    - **Eigenschaften**. Liste der Eigenschaften für die Ressource. Diese Vorlage verwendet Speicherplatz und Subnetz Eigenschaften beim VNet erstellen.

7. Navigieren Sie zu [der Vorlage Beispielseite](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).
8. **Azuredeploy paremeters.json**auf, und klicken Sie auf **RAW**.
9. Speichern Sie die Datei auf einen lokalen Ordner auf Ihrem Computer.
10. Öffnen Sie die gerade gespeicherte Datei und bearbeiten Sie der Werte für die Parameter. Verwenden Sie die unten stehenden Werte in diesem Szenario beschriebene VNet bereitstellen.

        {
          "location": {
            "value": "Central US"
          },
          "vnetName": {
              "value": "TestVNet"
          },
          "addressPrefix": {
              "value": "192.168.0.0/16"
          },
          "subnet1Name": {
              "value": "FrontEnd"
          },
          "subnet1Prefix": {
            "value": "192.168.1.0/24"
          },
          "subnet2Name": {
              "value": "BackEnd"
          },
          "subnet2Prefix": {
              "value": "192.168.2.0/24"
          }
        }

11. Speichern Sie die Datei.
  