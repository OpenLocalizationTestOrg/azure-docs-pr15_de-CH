<properties
    pageTitle="Service Bus REST Lernprogramm mit messaging weitergeleitet | Microsoft Azure"
    description="Erstellen Sie eine einfache Service Bus Relay Host-Anwendung, die eine REST-basierte Schnittstelle verfügbar macht."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/27/2016"
    ms.author="sethm" />

# <a name="service-bus-relay-rest-tutorial"></a>Service Bus Relay-REST-Lernprogramm

In diesem Lernprogramm beschreibt, wie eine einfache Service Bus Host-Anwendung erstellen, die eine REST-basierte Schnittstelle verfügbar macht. REST kann einen Webclient, z. B. einem Webbrowser auf HTTP-Anfragen Service Bus-APIs zugreifen.

Diese praktische Einführung verwendet den Windows Communication Foundation (WCF) REST Programmiermodell zum Erstellen von REST-Dienst im Service Bus. Weitere Informationen finden Sie unter [WCF REST-Programmiermodell](https://msdn.microsoft.com/library/bb412169.aspx) und [Entwerfen und Implementieren von Services](https://msdn.microsoft.com/library/ms729746.aspx) in WCF-Dokumentation.

## <a name="step-1-create-a-service-namespace"></a>Schritt 1: Erstellen eines Dienst-Namespaces

Der erste Schritt ist zu einem Namespace und einer freigegebenen Access Signatur (SAS) Schlüssel. Ein Namespace bietet Anwendungsgrenzen für jede Anwendung über Service Bus zur Verfügung gestellt. SAS-Taste wird automatisch vom System generiert, wenn ein Dienstnamespace erstellt. Die Kombination von Dienstnamespace und SAS-Taste stellt die Anmeldeinformationen für Dienst zum Authentifizieren des Zugriffs auf eine Anwendung.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="step-2-define-a-rest-based-wcf-service-contract-to-use-with-service-bus"></a>Schritt 2: Definieren Sie einen Servicevertrag WCF REST-basierten Service Bus mit

Als mit anderen Diensten Service Bus müssen beim Erstellen von REST-Stil-Dienst Sie den Vertrag definieren. Der Vertrag gibt an, welche Vorgänge der Host unterstützt. Ein Dienstvorgang kann als eine Webdienstmethode vorstellen. Verträge werden durch die Definition einer Schnittstelle C++, C# oder Visual Basic erstellt. Jede Methode in der Schnittstelle entspricht einem bestimmten Dienst. [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) -Attribut muss auf jeder Schnittstelle angewendet und [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) -Attribut muss auf jeden Vorgang angewendet werden. Wenn eine Methode in einer Schnittstelle mit [dem ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) nicht [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx)verfügen, ist diese Methode nicht verfügbar. Der Code für diese Aufgaben wird im Beispiel der Vorgehensweise angezeigt.

Der Hauptunterschied zwischen einem grundlegenden Service Bus Vertrag und ein REST-Stil ist das Hinzufügen einer Eigenschaft zu [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx): [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx). Mit dieser Eigenschaft können Sie eine Methode in der Schnittstelle einer Methode auf der anderen Seite der Schnittstelle zuordnen. [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx) nutzen wir in diesem Fall um eine Methode mit HTTP GET zu verknüpfen. Dies ermöglicht Service Bus genau abrufen und an die Schnittstelle gesendeten Befehle interpretiert.

### <a name="to-create-a-service-bus-contract-with-an-interface"></a>Erstellen Sie einen Service Bus-Vertrag mit einer Schnittstelle

1. Öffnen Sie Visual Studio als Administrator: das Programm im **Startmenü** klicken, und klicken Sie dann auf **als Administrator ausführen**.

2. Erstellen Sie ein neues Konsolenanwendungsprojekt. Klicken Sie im Menü **Datei** auf **neu**, und wählen Sie **Projekt aus**. Klicken Sie im Dialogfeld **Neues Projekt** klicken Sie auf **Visual C#**wählen Sie **die Konsolenanwendungsvorlage** und nennen Sie **ImageListener**. Verwenden Sie den **Speicherort**. Klicken Sie auf **OK** , um das Projekt zu erstellen.

3. Bei einem C#-Projekt erstellt Visual Studio eine `Program.cs` Datei. Diese Klasse enthält eine leere `Main()` Methode, für die ein Konsolenanwendungsprojekt ordnungsgemäß erstellt.

4. Fügen Sie Verweise auf Service Bus und **' System.ServiceModel.dll '** zum Projekt, Service Bus NuGet-Paket installieren. Dieses Paket hinzugefügt automatisch Verweise auf Bibliotheken Service Bus als auch WCF- **System.ServiceModel**. Im Projektmappen-Explorer mit der rechten Maustaste des **ImageListener** Projekts und dann auf **NuGet-Pakete verwalten**. Klicken Sie auf die Registerkarte **Durchsuchen** , und suchen Sie nach `Microsoft Azure Service Bus`. Klicken Sie auf **Installieren**und akzeptieren Sie die Vereinbarung.

5. Sie müssen das Projekt explizit auf **System.ServiceModel.Web.dll** hinzufügen:

    ein. Im Projektmappen-Explorer mit der rechten Maustaste unter dem Projektordner Ordner **Verweise** und dann auf **Verweis hinzufügen**.

    b. Im Dialogfeld **Verweis hinzufügen** auf der Registerkarte **Rahmen** auf der linken Seite und in **das Suchfeld** , geben Sie **System.ServiceModel.Web**. Aktivieren Sie das Kontrollkästchen **System.ServiceModel.Web** und dann auf **OK**.

6. Fügen Sie den folgenden `using` -Anweisung am Anfang der Datei Program.cs.

    ```
    using System.ServiceModel;
    using System.ServiceModel.Channels;
    using System.ServiceModel.Web;
    using System.IO;
    ```

    [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) ist der Namespace, der programmgesteuerten Zugriff auf die grundlegenden Funktionen von WCF ermöglicht. Service Bus verwendet viele der Objekte und Attribute von WCF Dienstverträge definieren. Dieser Namespace wird in den meisten Service Bus Relay-Anwendung verwendet werden. Ebenso können [vom Typ System.ServiceModel.Channels dargestellt](https://msdn.microsoft.com/library/system.servicemodel.channels.aspx) den Kanal definieren, der das Objekt wird über den Webbrowser des Clients mit Service Bus kommunizieren. [System.ServiceModel.Web](https://msdn.microsoft.com/library/system.servicemodel.web.aspx) enthält die Typen, die Web-basierte Anwendung erstellen können.

7. Benennen Sie die `ImageListener` Namespace **Microsoft.ServiceBus.Samples**.

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        ...
    ```

8. Direkt nach der öffnenden geschweiften Klammer-Namespacedeklaration definieren eine neue Schnittstelle mit dem Namen **IImageContract** und wenden Sie das Attribut **dem ServiceContractAttribute** Schnittstelle mit `http://samples.microsoft.com/ServiceModel/Relay/`. Der Wert unterscheidet sich von der Namespace, den Gültigkeitsbereich des Codes verwenden. Der Wert dient als eindeutiger Bezeichner für diesen Vertrag und muss Versionsinformationen. Weitere Informationen finden Sie unter [Service Versioning](http://go.microsoft.com/fwlink/?LinkID=180498). Den Namespace explizit angeben verhindert der Vertragsname hinzufügen Namespace Standardwert.

    ```
    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/RESTTutorial1")]
    public interface IImageContract
    {
    }
    ```

9. In der `IImageContract` Schnittstelle, deklarieren Sie eine Methode für die Operation der `IImageContract` macht die Benutzeroberfläche und Anwenden der `OperationContractAttribute` -Attribut auf die Methode, die Sie als Bestandteil des öffentlichen Vertrags Service Bus verfügbar machen möchten.

    ```
    public interface IImageContract
    {
        [OperationContract]
        Stream GetImage();
    }
    ```

10. Fügen Sie im Attribut **OperationContract** **WebGet** Wert.

    ```
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }
    ```

    Dadurch kann HTTP GET Anfragen an Service Bus `GetImage`, und die Rückgabewerte der `GetImage` in ein GETRESPONSE HTTP-Antwort. Später im Lernprogramm werden Sie einen Web-Browser auf diese Methode zugreifen und die Darstellung im Browser verwenden.

11. Direkt nach der `IImageContract` Definition, deklarieren Sie einen Kanal, der beide erbt die `IImageContract` und `IClientChannel` Schnittstellen.

    ```
    public interface IImageChannel : IImageContract, IClientChannel { }
    ```

    Ein Kanal ist das WCF-Objekt über den Dienst und Client Informationen zu übergeben. Später erstellen Sie den Kanal in Ihrer Anwendung. Service Bus mithilfe dieses Kanals **GetImage** Implementierung HTTP GET-Anfragen vom Browser übergeben. Service Bus verwendet auch den Kanal **GetImage** Rückgabewert und einen HTTP-GETRESPONSE für den Clientbrowser übersetzen.

12. Klicken Sie im Menü **Erstellen** auf **Projektmappe** bisher bestätigen Sie die Richtigkeit Ihrer Arbeit.

### <a name="example"></a>Beispiel

Der folgende Code zeigt eine grundlegende Schnittstelle, die einen Service Bus-Vertrag definiert.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="step-3-implement-a-rest-based-wcf-service-contract-to-use-service-bus"></a>Schritt 3: Implementieren Sie einen Servicevertrag WCF REST-basierten Service Bus verwenden

Ein REST-Stil Service Bus muss zum Erstellen den Vertrag erstellen mithilfe einer Schnittstelle definiert ist. Der nächste Schritt ist die Schnittstelle implementieren. Dies umfasst das Erstellen einer Klasse mit dem Namen **ImageService** , das benutzerdefinierte **IImageContract** -Schnittstelle implementiert. Nachdem Sie den Vertrag implementiert, konfigurieren Sie dann die Schnittstelle mit einer app. Die Konfigurationsdatei enthält die erforderlichen Informationen für die Anwendung den Namen des Dienstes, den Namen des Vertrags und den Typ des Protokolls zur Kommunikation mit Service Bus. Der Code für diese Aufgaben wird im Beispiel die Verfahren bereitgestellt.

Wie bei den vorherigen Schritten ist wenig Unterschiede zwischen einen Vertrag REST-Stil und einen grundlegenden Service Bus-Vertrag.

### <a name="to-implement-a-rest-style-service-bus-contract"></a>REST-Stil Service Bus-Vertrag implementieren

1. Erstellen Sie eine neue Klasse namens **ImageService** direkt nach der Definition der **IImageContract** -Schnittstelle. **ImageService** -Klasse implementiert die **IImageContract** -Schnittstelle.

    ```
    class ImageService : IImageContract
    {
    }
    ```
    Wie andere schnittstellenimplementierungen können Sie die Definition in einer anderen Datei implementieren. In diesem Lernprogramm die Implementierung jedoch in derselben Datei wie die Schnittstellendefinition und `Main()` Methode.

2. Die **IImageService** -Klasse an, dass die Klasse eine Implementierung eines WCF zuweisen Sie [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) Attribut.

    ```
    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
    }
    ```

    Wie bereits erwähnt, geht dieser Namespace nicht traditionellen Namespace. Stattdessen ist Teil der WCF-Architektur, die den Vertrag identifiziert. Weitere Informationen finden Sie unter [Daten Vertragsnamen](https://msdn.microsoft.com/library/ms731045.aspx) in WCF-Dokumentation.

3. JPG-Bild dem Projekt hinzufügen.  

    Dies ist ein Bild, das der Dienst im empfangenden Browser angezeigt wird. Maustaste auf das Projekt, und klicken Sie auf **Hinzufügen**. Klicken Sie auf **Vorhandenes Element**. Verwenden Sie das Dialogfeld **Vorhandenes Element hinzufügen** , um eine entsprechende JPG zu suchen, und klicken Sie auf **Hinzufügen**.

    Beim Hinzufügen der Datei sicherstellen, dass **Alle Dateien** in der Liste, neben ausgewählt ist den **Dateiname:** Feld. Der Rest dieses Lernprogramms wird davon ausgegangen, dass der Name des Bildes "image.jpg". Haben Sie eine andere Datei, müssen Sie das Bild umbenennen oder ändern Sie den Code um zu kompensieren.

4. Um sicherzustellen, dass der ausgeführte Dienst die Datei finden, im **Projektmappen-Explorer** mit der rechten Maustaste der Datei und anschließend auf **Eigenschaften**. Legen Sie im Bereich **Eigenschaften** **In Ausgabeverzeichnis kopieren** auf **Kopieren, wenn neuer**.

5. Einen Verweis auf die Assembly **System.Drawing.dll** für das Projekt, und folgende verbundenen `using` Aussagen.  

    ```
    using System.Drawing;
    using System.Drawing.Imaging;
    using Microsoft.ServiceBus;
    using Microsoft.ServiceBus.Web;
    ```

6. Fügen Sie in der Klasse **ImageService** den folgenden Konstruktor, der Bitmap geladen und an den Clientbrowser senden vorbereitet.

    ```
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }
    }
    ```

7. Fügen Sie direkt nach dem vorherigen Code **GetImage** folgt **ImageService** Klasse eine HTTP-Nachricht zurück, die das Bild enthält.

    ```
    public Stream GetImage()
    {
        MemoryStream stream = new MemoryStream();
        this.bitmap.Save(stream, ImageFormat.Jpeg);

        stream.Position = 0;
        WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

        return stream;
    }
    ```

    Diese Implementierung verwendet **MemoryStream** das Bild abgerufen und im Browser streaming vorbereiten. Die Position im Stream bei Null beginnt, erklärt den Stream Inhalt als Jpeg und überträgt die Informationen.

8. Klicken Sie im Menü **Erstellen** auf **Projektmappe erstellen**.

### <a name="to-define-the-configuration-for-running-the-web-service-on-service-bus"></a>Um die Konfiguration für den Webdienst auf Service Bus ausgeführt

1. Doppelklicken Sie im **Projektmappen-Explorer**auf **App.config** , um sie in der Visual Studio-Editor öffnen.

    Die Datei **App.config** ähnelt eine WCF-Konfigurationsdatei und enthält Namen, Endpunkt (d. h. den Speicherort Service Bus stellt für Clients und Server kommunizieren) und Bindung (Typ Protokoll kommunizieren). Der wichtigste Unterschied ist, dass der konfigurierten Endpunkt ist nicht Teil von.NET Framework eine Bindung [WebHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.webhttprelaybinding.aspx) bezieht.

2. Die `<system.serviceModel>` XML-Element ist ein WCF-Element, das einen oder mehrere Dienste definiert. Es dient hier Dienstnamen und Endpunkt definiert. Am Ende der `<system.serviceModel>` Element (aber noch `<system.serviceModel>`), Hinzufügen einer `<bindings>` Element mit dem folgenden Inhalt. Die Bindung der Anwendung definiert. Mehrere Bindungen definieren, jedoch für dieses Lernprogramm definieren Sie nur eine.

    ```
    <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
            <binding name="default">
                <security relayClientAuthenticationType="None" />
            </binding>
        </webHttpRelayBinding>
    </bindings>
    ```

    Dieser Schritt definiert eine Bindung Service Bus [WebHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.webhttprelaybinding.aspx) mit **RelayClientAuthenticationType** auf **None**festgelegt. Diese Einstellung gibt an, dass ein Endpunkt mithilfe dieser Bindung nicht Clientanmeldeinformationen.

3. Nach dem `<bindings>` Element hinzufügen ein `<services>` Element. Wie die Bindung können Sie mehrere Dienste in einer Konfigurationsdatei definieren. Definieren für dieses Lernprogramm Sie allerdings nur ein.

    ```
    <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
            <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IImageContract"
                    binding="webHttpRelayBinding"
                    bindingConfiguration="default"
                    behaviorConfiguration="sbTokenProvider"
                    address="" />
        </service>
    </services>
    ```

    Dieser Schritt konfiguriert einen Dienst, der zuvor definierten Standard- **WebHttpRelayBinding**verwendet. Darüber hinaus verwendet die standardmäßige **SbTokenProvider**, die im nächsten Schritt definiert.

4. Nach dem `<services>` Element erstellen ein `<behaviors>` mit folgendem Inhalt der *SAS* (SAS) Schlüssel Sie zuvor aus dem [Azure-Portal][]"SAS_KEY" ersetzen.

    ```
    <behaviors>
        <endpointBehaviors>
            <behavior name="sbTokenProvider">
                <transportClientEndpointBehavior>
                    <tokenProvider>
                        <sharedAccessSignature keyName="RootManageSharedAccessKey" key="SAS_KEY" />
                    </tokenProvider>
                </transportClientEndpointBehavior>
            </behavior>
            </endpointBehaviors>
            <serviceBehaviors>
                <behavior name="default">
                    <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
                </behavior>
            </serviceBehaviors>
    </behaviors>
    ```

5. Weiterhin in App.config, in der `<appSettings>` Element, ersetzen die gesamte Verbindungszeichenfolge Wert mit der Verbindungszeichenfolge, die Sie zuvor aus dem Portal erhalten. 

    ```
    <appSettings>
    <!-- Service Bus specific app settings for messaging connections -->
    <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```

6. Klicken Sie im Menü **Erstellen** auf **Projektmappe** Erstellen der gesamte Projektmappe.

### <a name="example"></a>Beispiel

Der folgende Code zeigt die Vertrag und Implementierung für einen REST-basierten Dienst, der auf Service Bus läuft die **WebHttpRelayBinding** Bindung.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{


    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Das folgende Beispiel zeigt die Datei App.config des Dienstes zugeordnet.

```
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2"/>
    </startup>
    <system.serviceModel>
        <extensions>
            <!-- In this extension section we are introducing all known service bus extensions. User can remove the ones they don't need. -->
            <behaviorExtensions>
                <add name="connectionStatusBehavior"
                    type="Microsoft.ServiceBus.Configuration.ConnectionStatusElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="transportClientEndpointBehavior"
                    type="Microsoft.ServiceBus.Configuration.TransportClientEndpointBehaviorElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="serviceRegistrySettings"
                    type="Microsoft.ServiceBus.Configuration.ServiceRegistrySettingsElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </behaviorExtensions>
            <bindingElementExtensions>
                <add name="netMessagingTransport"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingTransportExtensionElement, Microsoft.ServiceBus,  Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="tcpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.TcpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpsRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpsRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="onewayRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.RelayedOnewayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingElementExtensions>
            <bindingExtensions>
                <add name="basicHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.BasicHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="webHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WebHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="ws2007HttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WS2007HttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netOnewayRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetOnewayRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netEventRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetEventRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netMessagingBinding"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingExtensions>
        </extensions>
      <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
          <binding name="default">
            <security relayClientAuthenticationType="None" />
          </binding>
        </webHttpRelayBinding>
      </bindings>
      <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
          <endpoint name="RelayEndpoint"
                  contract="Microsoft.ServiceBus.Samples.IImageContract"
                  binding="webHttpRelayBinding"
                  bindingConfiguration="default"
                  behaviorConfiguration="sbTokenProvider"
                  address="" />
        </service>
      </services>
      <behaviors>
        <endpointBehaviors>
          <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
              <tokenProvider>
                <sharedAccessSignature keyName="RootManageSharedAccessKey" key="[SAS_KEY]" />
              </tokenProvider>
            </transportClientEndpointBehavior>
          </behavior>
        </endpointBehaviors>
        <serviceBehaviors>
          <behavior name="default">
            <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
          </behavior>
        </serviceBehaviors>
      </behaviors>
    </system.serviceModel>
    <appSettings>
        <!-- Service Bus specific app setings for messaging connections -->
        <add key="Microsoft.ServiceBus.ConnectionString"
            value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
</configuration>
```

## <a name="step-4-host-the-rest-based-wcf-service-to-use-service-bus"></a>Schritt 4: Host WCF REST-basierten Dienst zum Service Bus verwenden

Dieser Schritt beschreibt das Ausführen eines Webdienstes mit eine Service Bus. Eine vollständige Liste der Programmcode in diesem Schritt wird im Beispiel die Verfahren bereitgestellt.

### <a name="to-create-a-base-address-for-the-service"></a>Eine Basisadresse für den Dienst erstellen

1. In der `Main()` Funktionsdeklaration, erstellen Sie eine Variable zur Speicherung des Namespaces des Projekts Service Bus. Ersetzen Sie `yourNamespace` mit dem Namen der zuvor erstellten Dienstnamespace.

    ```
    string serviceNamespace = "yourNamespace";
    ```
    Service Bus verwendet den Namen des Namespaces einen eindeutigen URI erstellen.

2. Erstellen einer `Uri` Instanz für die Basisadresse des Diensts auf den Namespace.

    ```
    Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");
    ```

### <a name="to-create-and-configure-the-web-service-host"></a>Erstellen und konfigurieren Sie den Web Service host

- Erstellen Sie den Webdiensthost über die URI-Adresse weiter oben in diesem Abschnitt erstellt.

    ```
    WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
    ```
    Der Diensthost wird WCF-Objekt, das die Host-Anwendung instanziiert. In diesem Beispiel übergibt es Hosts zu erstellen (ein **ImageService**) sowie die Adresse, an der Sie die Host-Anwendung verfügbar machen möchten.

### <a name="to-run-the-web-service-host"></a>Die Web Service-Host ausführen

1. Öffnen Sie den Service.

    ```
    host.Open();
    ```
    Der Dienst wird jetzt ausgeführt.

2. Zeigen Sie eine Meldung, dass der Dienst ausgeführt wird und wie Sie den Dienst beenden.

    ```
    Console.WriteLine("Copy the following address into a browser to see the image: ");
    Console.WriteLine(address + "GetImage");
    Console.WriteLine();
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

3. Schließen Sie den Diensthost.

    ```
    host.Close();
    ```

## <a name="example"></a>Beispiel

Im folgende Beispiel enthält den Servicevertrag und Implementierung von Schritten im Lernprogramm und den Dienst in eine hostet. Kompilieren Sie den folgenden Code in eine ausführbare Datei namens ImageListener.exe.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string serviceNamespace = "InsertServiceNamespaceHere";
            Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");

            WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
            host.Open();

            Console.WriteLine("Copy the following address into a browser to see the image: ");
            Console.WriteLine(address + "GetImage");
            Console.WriteLine();
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            host.Close();
        }
    }
}
```

### <a name="compiling-the-code"></a>Kompilieren des Codes

Gehen Sie nach Erstellung der Projektmappe zum Ausführen der Anwendung:

1. Drücken Sie **F5**, oder wechseln Sie zum Speicherort ausführbaren Datei (ImageListener\bin\Debug\ImageListener.exe) zum Ausführen des Dienstes. Halten Sie die app, als nächsten Schritt erforderlich ist.

2. Kopieren Sie und fügen Sie die Adresse aus der Befehlszeile in einem Browser, um das Bild anzuzeigen.

3. Wenn Sie fertig sind, drücken Sie **EINGABETASTE** im Eingabeaufforderungsfenster die Anwendung schließen.

## <a name="next-steps"></a>Nächste Schritte

Damit eine Anwendung, die den Service Bus-Dienst verwendet erstellt haben, lesen Sie folgenden Artikel erfahren Sie mehr über messaging weitergeleitet:

- [Azure Service Bus-Architektur im Überblick](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#relays)

- [Verwendung den Service Bus Relay](service-bus-dotnet-how-to-use-relay.md)

[Azure-portal]: https://portal.azure.com