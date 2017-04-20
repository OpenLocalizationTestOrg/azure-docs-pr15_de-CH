<properties
   pageTitle="Gerät mit Linux | Microsoft Azure"
   description="Beschreibt, wie ein Gerät der Azure IoT Suite vorkonfiguriert remote monitoring-Lösung mit einer Anwendung auf Linux c geschrieben."
   services=""
   suite="iot-suite"
   documentationCenter="na"
   authors="dominicbetts"
   manager="timlt"
   editor=""/>

<tags
   ms.service="iot-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="dobett"/>


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-linux"></a>Verbinden Sie das Gerät remote monitoring vorkonfigurierte Lösung (Linux)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-a-sample-c-client-linux"></a>Erstellen und Ausführen eines C Beispielclient Linux

Die folgenden Verfahren zeigen, wie Sie Erstellen einer Clientanwendung, in C geschriebene erstellt und ausgeführt unter Ubuntu Linux kommuniziert, remote monitoring vorkonfigurierte Lösung. Um diese Schritte auszuführen, benötigen Sie ein Gerät mit Ubuntu Version 15.04 oder 15.10. Installieren Sie bevor Sie fortfahren Pakete auf Ihrem Ubuntu-Gerät mit dem folgenden Befehl:

```
sudo apt-get install cmake gcc g++
```

## <a name="install-the-client-libraries-on-your-device"></a>Die Clientbibliotheken auf Ihrem Gerät installieren

Azure IoT Hub-Clientbibliotheken stehen als Paket auf Ihrem Ubuntu-Gerät mit **apt-Get** -Befehl installieren können. Führen Sie die folgenden Schritte aus, um das Paket zu installieren, das die IoT Hub Client Library und Header-Dateien auf Ihrem Ubuntu-System enthält:

1. AzureIoT Repository zum Computer hinzufügen:

    ```
    sudo add-apt-repository ppa:aziotsdklinux/ppa-azureiot
    sudo apt-get update
    ```

2. Installieren Sie Azure-Iot-Sdk-c-Dev-Paket

    ```
    sudo apt-get install -y azure-iot-sdk-c-dev
    ```

## <a name="add-code-to-specify-the-behavior-of-the-device"></a>Fügen Sie Code hinzu, um das Verhalten des Geräts

Erstellen Sie einen Ordner auf Ihrem Computer Ubuntu **remote\_Überwachung**. In der **remote\_Überwachung** Ordner erstellen vier Dateien **main.c** **remote\_monitoring.c**, **remote\_monitoring.h**, und **CMakeLists.txt**.

IoT Hub Serialisierungsprogramm-Clientbibliotheken verwenden ein Modell, das Format von Nachrichten an das Gerät sendet IoT Hub und Befehle aus IoT Hub.

1. Öffnen Sie in einem Text-Editor die **remote\_monitoring.c** Datei. Fügen Sie den folgenden `#include` Aussagen:

    ```
    #include "iothubtransportamqp.h"
    #include "schemalib.h"
    #include "iothub_client.h"
    #include "serializer.h"
    #include "schemaserializer.h"
    #include "azure_c_shared_utility/threadapi.h"
    #include "azure_c_shared_utility/platform.h"
    ```

2. Fügen Sie die folgenden Variablendeklarationen nach dem `#include` Aussagen. Ersetzen Sie die Platzhalterwerte [Geräte-Id] und [Gerät] mit Werten für Ihr Gerät remote monitoring Lösung Dashboard. [Name des IoTHub] ersetzen mithilfe IoT Hub Hostname aus dem Dashboard. Ist der Hostname IoT Hub **contoso.azure devices.net**, ersetzen Sie [Name des IoTHub] mit **Contoso**fest:

    ```
    static const char* deviceId = "[Device Id]";
    static const char* deviceKey = "[Device Key]";
    static const char* hubName = "IoTHub Name]";
    static const char* hubSuffix = "azure-devices.net";
    ```

3. Fügen Sie den folgenden Code für das Modell definieren, das können das Gerät mit IoT Hub kommunizieren. Dieses Modell gibt an, dass das Gerät Temperatur, Temperatur, Luftfeuchtigkeit und Geräte-Id als Telemetrie sendet. Das Gerät sendet auch Metadaten über das Gerät IoT Hub, einschließlich einer Liste von Befehlen, die das Gerät unterstützt. Das Gerät reagiert auf die Befehle **SetTemperature** und **SetHumidity**:

    ```
    // Define the Model
    BEGIN_NAMESPACE(Contoso);

    DECLARE_STRUCT(SystemProperties,
      ascii_char_ptr, DeviceID,
      _Bool, Enabled
    );

    DECLARE_STRUCT(DeviceProperties,
      ascii_char_ptr, DeviceID,
      _Bool, HubEnabledState
    );

    DECLARE_MODEL(Thermostat,

      /* Event data (temperature, external temperature and humidity) */
      WITH_DATA(int, Temperature),
      WITH_DATA(int, ExternalTemperature),
      WITH_DATA(int, Humidity),
      WITH_DATA(ascii_char_ptr, DeviceId),

      /* Device Info - This is command metadata + some extra fields */
      WITH_DATA(ascii_char_ptr, ObjectType),
      WITH_DATA(_Bool, IsSimulatedDevice),
      WITH_DATA(ascii_char_ptr, Version),
      WITH_DATA(DeviceProperties, DeviceProperties),
      WITH_DATA(ascii_char_ptr_no_quotes, Commands),

      /* Commands implemented by the device */
      WITH_ACTION(SetTemperature, int, temperature),
      WITH_ACTION(SetHumidity, int, humidity)
    );

    END_NAMESPACE(Contoso);
    ```

### <a name="add-code-to-implement-the-behavior-of-the-device"></a>Fügen Sie Code zum Implementieren des Verhaltens des Geräts

Hinzufügen von Funktionen ausführen, wenn das Gerät von der Hub und den Code zum simulierten Telemetrie an den Hub sendet einen Befehl empfängt.

1. Fügen Sie die folgenden Funktionen ausführen, erhält das Gerät im Modell definierten Befehle **SetTemperature** und **SetHumidity** :

    ```
    EXECUTE_COMMAND_RESULT SetTemperature(Thermostat* thermostat, int temperature)
    {
      (void)printf("Received temperature %d\r\n", temperature);
      thermostat->Temperature = temperature;
      return EXECUTE_COMMAND_SUCCESS;
    }

    EXECUTE_COMMAND_RESULT SetHumidity(Thermostat* thermostat, int humidity)
    {
      (void)printf("Received humidity %d\r\n", humidity);
      thermostat->Humidity = humidity;
      return EXECUTE_COMMAND_SUCCESS;
    }
    ```

2. Fügen Sie die folgende Funktion, die eine Nachricht an IoT Hub sendet:

    ```
    static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
    {
      IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
      if (messageHandle == NULL)
      {
        printf("unable to create a new IoTHubMessage\r\n");
      }
      else
      {
        if (IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, NULL, NULL) != IOTHUB_CLIENT_OK)
        {
          printf("failed to hand over the message to IoTHubClient");
        }
        else
        {
          printf("IoTHubClient accepted the message for delivery\r\n");
        }

        IoTHubMessage_Destroy(messageHandle);
      }
    free((void*)buffer);
    }
    ```

3. Fügen Sie die folgende Funktion, die die Serialisierung Bibliothek im SDK verknüpft:

    ```
    static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
    {
      IOTHUBMESSAGE_DISPOSITION_RESULT result;
      const unsigned char* buffer;
      size_t size;
      if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
      {
        printf("unable to IoTHubMessage_GetByteArray\r\n");
        result = EXECUTE_COMMAND_ERROR;
      }
      else
      {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
          printf("failed to malloc\r\n");
          result = EXECUTE_COMMAND_ERROR;
        }
        else
        {
          memcpy(temp, buffer, size);
          temp[size] = '\0';
          EXECUTE_COMMAND_RESULT executeCommandResult = EXECUTE_COMMAND(userContextCallback, temp);
          result =
            (executeCommandResult == EXECUTE_COMMAND_ERROR) ? IOTHUBMESSAGE_ABANDONED :
            (executeCommandResult == EXECUTE_COMMAND_SUCCESS) ? IOTHUBMESSAGE_ACCEPTED :
            IOTHUBMESSAGE_REJECTED;
          free(temp);
        }
      }
      return result;
    }
    ```

4. Fügen Sie die folgende Funktion Verbindung IoT Hub senden und Empfangen von Nachrichten und trennen Sie den Hub. Beachten Sie, wie das Gerät sendet Metadaten, einschließlich Befehlen IoT Hub Verbindung unterstützt werden. Diese Metadaten kann die Lösung zum Aktualisieren des Status des Geräts auf dem Dashboard **Ausführen** :

    ```
    void remote_monitoring_run(void)
    {
      if (platform_init() != 0)
      {
        printf("Failed to initialize the platform.\r\n");
      }
      else
      {
        if (serializer_init(NULL) != SERIALIZER_OK)
        {
          printf("Failed on serializer_init\r\n");
        }
        else
        {
          IOTHUB_CLIENT_CONFIG config;
          IOTHUB_CLIENT_HANDLE iotHubClientHandle;

          config.deviceId = deviceId;
          config.deviceKey = deviceKey;
          config.deviceSasToken = NULL;
          config.iotHubName = hubName;
          config.iotHubSuffix = hubSuffix;
          config.protocol = AMQP_Protocol;
          iotHubClientHandle = IoTHubClient_Create(&config);
          if (iotHubClientHandle == NULL)
          {
            (void)printf("Failed on IoTHubClient_CreateFromConnectionString\r\n");
          }
          else
          {
            Thermostat* thermostat = CREATE_MODEL_INSTANCE(Contoso, Thermostat);
            if (thermostat == NULL)
            {
              (void)printf("Failed on CREATE_MODEL_INSTANCE\r\n");
            }
            else
            {
              STRING_HANDLE commandsMetadata;

              if (IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, thermostat) != IOTHUB_CLIENT_OK)
              {
                printf("unable to IoTHubClient_SetMessageCallback\r\n");
              }
              else
              {

                /* send the device info upon startup so that the cloud app knows
                   what commands are available and the fact that the device is up */
                thermostat->ObjectType = "DeviceInfo";
                thermostat->IsSimulatedDevice = false;
                thermostat->Version = "1.0";
                thermostat->DeviceProperties.HubEnabledState = true;
                thermostat->DeviceProperties.DeviceID = (char*)deviceId;

                commandsMetadata = STRING_new();
                if (commandsMetadata == NULL)
                {
                  (void)printf("Failed on creating string for commands metadata\r\n");
                }
                else
                {
                  /* Serialize the commands metadata as a JSON string before sending */
                  if (SchemaSerializer_SerializeCommandMetadata(GET_MODEL_HANDLE(Contoso, Thermostat), commandsMetadata) != SCHEMA_SERIALIZER_OK)
                  {
                    (void)printf("Failed serializing commands metadata\r\n");
                  }
                  else
                  {
                    unsigned char* buffer;
                    size_t bufferSize;
                    thermostat->Commands = (char*)STRING_c_str(commandsMetadata);

                    /* Here is the actual send of the Device Info */
                    if (SERIALIZE(&buffer, &bufferSize, thermostat->ObjectType, thermostat->Version, thermostat->IsSimulatedDevice, thermostat->DeviceProperties, thermostat->Commands) != IOT_AGENT_OK)
                    {
                      (void)printf("Failed serializing\r\n");
                    }
                    else
                    {
                      sendMessage(iotHubClientHandle, buffer, bufferSize);
                    }

                  }

                  STRING_delete(commandsMetadata);
                }

                thermostat->Temperature = 50;
                thermostat->ExternalTemperature = 55;
                thermostat->Humidity = 50;
                thermostat->DeviceId = (char*)deviceId;

                while (1)
                {
                  unsigned char*buffer;
                  size_t bufferSize;

                  (void)printf("Sending sensor value Temperature = %d, Humidity = %d\r\n", thermostat->Temperature, thermostat->Humidity);

                  if (SERIALIZE(&buffer, &bufferSize, thermostat->DeviceId, thermostat->Temperature, thermostat->Humidity, thermostat->ExternalTemperature) != IOT_AGENT_OK)
                  {
                    (void)printf("Failed sending sensor value\r\n");
                  }
                  else
                  {
                    sendMessage(iotHubClientHandle, buffer, bufferSize);
                  }

                  ThreadAPI_Sleep(1000);
                }
              }

              DESTROY_MODEL_INSTANCE(thermostat);
            }
            IoTHubClient_Destroy(iotHubClientHandle);
          }
          serializer_deinit();
        }
        platform_deinit();
      }
    }
    ```
    
    Zu Referenzzwecken hier ist ein Beispiel **DeviceInfo** Nachricht an IoT Hub beim Start:

    ```
    {
      "ObjectType":"DeviceInfo",
      "Version":"1.0",
      "IsSimulatedDevice":false,
      "DeviceProperties":
      {
        "DeviceID":"mydevice01", "HubEnabledState":true
      }, 
      "Commands":
      [
        {"Name":"SetHumidity", "Parameters":[{"Name":"humidity","Type":"double"}]},
        { "Name":"SetTemperature", "Parameters":[{"Name":"temperature","Type":"double"}]}
      ]
    }
    ```
    
    Zu Referenzzwecken hier ist ein Beispiel **Telemetrie** Nachricht IoT Hub:

    ```
    {"DeviceId":"mydevice01", "Temperature":50, "Humidity":50, "ExternalTemperature":55}
    ```
    
    Zu Referenzzwecken ist hier ein Beispiel von IoT Hub erhaltenen **Befehl** :
    
    ```
    {
      "Name":"SetHumidity",
      "MessageId":"2f3d3c75-3b77-4832-80ed-a5bb3e233391",
      "CreatedTime":"2016-03-11T15:09:44.2231295Z",
      "Parameters":{"humidity":23}
    }
    ```

### <a name="add-code-to-invoke-the-remotemonitoringrun-function"></a>Fügen Sie Code zum Aufrufen der Funktion remote_monitoring_run

Öffnen Sie in einem Texteditor die Datei **remote_monitoring.h** . Fügen Sie den folgenden Code hinzu:

```
void remote_monitoring_run(void);
```

In einem Texteditor die Datei **main.c** aus. Fügen Sie den folgenden Code hinzu:

```
#include "remote_monitoring.h"

int main(void)
{
    remote_monitoring_run();

    return 0;
}
```

## <a name="use-cmake-to-build-the-client-application"></a>Verwenden Sie CMake, um die Client-Anwendung erstellen

Die folgenden Schritte beschreiben, wie mit *CMake* die Clientanwendung erstellen.

1. In einem Texteditor die Datei **CMakeLists.txt** im Ordner " **Remote_monitoring** ".

2. Fügen Sie folgendermaßen vor, um die Clientanwendung zu definieren:

    ```
    cmake_minimum_required(VERSION 2.8.11)

    set(AZUREIOT_INC_FOLDER ".." "/usr/include/azureiot")

    include_directories(${AZUREIOT_INC_FOLDER})

    set(sample_application_c_files
        ./remote_monitoring.c
        ./main.c
    )

    set(sample_application_h_files
        ./remote_monitoring.h
    )

    add_executable(sample_app ${sample_application_c_files} ${sample_application_h_files})

    target_link_libraries(sample_app
        serializer
        iothub_client
        iothub_client_amqp_transport
        aziotsharedutil
        uamqp
        pthread
        curl
        ssl
        crypto
    )
    ```

3. Erstellen Sie im Ordner **Remote_monitoring** einen Ordner zum Speichern von Dateien *Erstellen* , die CMake generiert und führen Sie die **Cmake** und **Stellen** Befehle wie folgt:

    ```
    mkdir cmake
    cd cmake
    cmake ../
    make
    ```

4. Die Clientanwendung ausführen und Telemetrie IoT Hub senden:

    ```
    ./sample_app
    ```

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

