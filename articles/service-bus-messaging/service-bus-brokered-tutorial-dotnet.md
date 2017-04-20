<properties 
    pageTitle="Service Bus messaging .NET Lernprogramm vermittelt | Microsoft Azure"
    description="Vermittelte messaging .NET Tutorial."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/27/2016"
    ms.author="sethm" />

# <a name="service-bus-brokered-messaging-net-tutorial"></a>Service Bus messaging .NET Lernprogramm vermittelt

Azure Service Bus bietet umfassende messaging gegeben – eine über einen zentralen "Relay" in der Cloud, die unterstützt eine Vielzahl von verschiedenen Transportprotokollen und Web services Standards wie SOAP, WS-*, und. Der Client eine direkte Verbindung mit dem lokalen Dienst nicht benötigen noch muss er wissen, wo sich der Dienst befindet und der lokalen Dienst benötigt keine eingehenden Ports in der Firewall geöffnet.

Die zweite messaging ermöglicht "Vermittelter" messaging-Funktionen. Diese können man als asynchron oder Unterstützung Veröffentlichen-Abonnieren Messagingfunktionen zeitliche Entkopplung und Lastenausgleich Szenarien, in denen der Messaginginfrastruktur Service Bus abgekoppelt. Entkoppelte Kommunikation hat viele Vorteile. Beispielsweise können Clients und Server Bedarf verbinden und ihre Vorgänge asynchron ausführen.

Dieses Lernprogramm soll Ihnen einen Überblick und praktische Erfahrung mit Warteschlangen, eine der Kernkomponenten des Service Bus messaging vermittelt. Nachdem Sie die Reihenfolge der Themen in diesem Lernprogramm durcharbeiten, haben Sie eine Anwendung füllt eine Liste von Nachrichten, die eine Warteschlange erstellt und Nachrichten an die Warteschlange gesendet. Schließlich die Anwendung empfängt und zeigt die Nachrichten aus der Warteschlange und bereinigt Ressourcen und beendet. Eine entsprechende Anleitung, die beschreibt, wie Sie eine Anwendung erstellen, die den Service Bus Relay verwendet finden Sie unter [Service Bus messaging Lernprogramm weitergeleitet](../service-bus-relay/service-bus-relay-tutorial.md).

## <a name="introduction-and-prerequisites"></a>Einführung und Komponenten

Warteschlangen bieten erste, Nachrichtenübermittlung an einen oder mehrere konkurrierende Consumer erste Out (FIFO). FIFO bedeutet, dass Nachrichten in der Regel sollen empfangen und vom Empfänger in der zeitlichen Reihenfolge in die Warteschlange gestellt wurden, und jede Nachricht empfangen und verarbeitet nur eine Nachricht Verbraucher verarbeitet werden. Ein wichtiger Vorteil von Warteschlangen zu *zeitliche Entkopplung* der Komponenten ist: also den Erzeugern und Verbrauchern müssen nicht senden und Empfangen von Nachrichten gleichzeitig, da Nachrichten dauerhaft in der Warteschlange gespeichert werden. Verwandter Vorteil ist *Laden Abgleich*, Hersteller und Verbraucher zum Senden und Empfangen von Nachrichten unterschiedlich.

Es folgen einige administrative und erforderlichen Schritte sollten vor Beginn des Lernprogramms. Die erste ist Service-Namespace erstellen und einen gemeinsamen Zugriff Signaturschlüssel (SAS) erhalten. Ein Namespace bietet Anwendungsgrenzen für jede Anwendung über Service Bus zur Verfügung gestellt. SAS-Taste wird automatisch vom System generiert, wenn ein Dienstnamespace erstellt. Die Kombination von Service Namespace und SAS-Taste stellt Anmeldeinformationen mit der Service Bus Zugriff auf eine Anwendung authentifiziert.

### <a name="create-a-service-namespace-and-obtain-a-sas-key"></a>Erstellen eines Service-Namespaces und einen SAS-Schlüssel

Zunächst ist einen Service-Namespace erstellen und [SAS](service-bus-sas-overview.md) (SAS) Schlüssel erhalten. Ein Namespace bietet Anwendungsgrenzen für jede Anwendung über Service Bus zur Verfügung gestellt. SAS-Taste wird automatisch vom System generiert, wenn ein Dienstnamespace erstellt. Die Kombination von Service Namespace und SAS-Taste stellt Anmeldeinformationen für Service Bus zum Authentifizieren des Zugriffs auf eine Anwendung.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Im nächste Schritt wird ein Visual Studio-Projekt erstellen und Schreiben zwei Hilfsfunktionen, die eine durch Trennzeichen getrennte Liste der Nachrichten in eine stark typisierte [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) .NET [Liste](https://msdn.microsoft.com/library/6sh2ey19.aspx) laden.

### <a name="create-a-visual-studio-project"></a>Erstellen Sie ein Visual Studio-Projekt

1. Öffnen Sie Visual Studio als Administrator das Programm im Startmenü, und klicken Sie auf **als Administrator ausführen**.

1. Erstellen Sie ein neues Konsolenanwendungsprojekt. Klicken Sie im Menü **Datei** auf **neu**, und klicken Sie auf **Projekt**. Klicken Sie im Dialogfeld **Neues Projekt** auf **Visual C#** (Wenn **Visual C#** nicht sehen unter **Andere Sprachen**angezeigt wird), klicken Sie auf **die Konsolenanwendungsvorlage** und nennen Sie es **QueueSample**. Verwenden Sie den **Speicherort**. Klicken Sie auf **OK** , um das Projekt zu erstellen.

1. Verwenden Sie die NuGet-Paket-Manager Service Bus-Bibliotheken zum Projekt hinzufügen:
    1. Im Projektmappen-Explorer mit der rechten Maustaste des **QueueSample** Projekts und dann auf **NuGet-Pakete verwalten**.
    2. Klicken Sie auf die Registerkarte **Suchen** im Dialogfeld " **Nuget-Pakete verwalten** " Suchen Sie **Azure Service Bus**, und klicken Sie auf **Installieren**.
<br />
1. Doppelklicken Sie im Projektmappen-Explorer auf die Datei Program.cs, um im Visual Studio-Editor geöffnet. Ändern Sie den Namespacenamen aus den Standardnamen des `QueueSample` , `Microsoft.ServiceBus.Samples`.

    ```
    Microsoft.ServiceBus.Samples
    {
        ...
    ```

1. Ändern der `using` Anweisung wie im folgenden Code gezeigt.

    ```
    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.IO;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.ServiceBus.Messaging;
    ```

1. Erstellen Sie eine Textdatei namens Data.csv und Kopie im folgenden Trennzeichen.

    ```
    IssueID,IssueTitle,CustomerID,CategoryID,SupportPackage,Priority,Severity,Resolved
    1,Package lost,1,1,Basic,5,1,FALSE
    2,Package damaged,1,1,Basic,5,1,FALSE
    3,Product defective,1,2,Premium,5,2,FALSE
    4,Product damaged,2,2,Premium,5,2,FALSE
    5,Package lost,2,2,Basic,5,2,TRUE
    6,Package lost,3,2,Basic,5,2,FALSE
    7,Package damaged,3,7,Premium,5,3,FALSE
    8,Product defective,3,2,Premium,5,3,FALSE
    9,Product damaged,4,6,Premium,5,3,TRUE
    10,Package lost,4,8,Basic,5,3,FALSE
    11,Package damaged,5,4,Basic,5,4,FALSE
    12,Product defective,5,4,Basic,5,4,FALSE
    13,Package lost,6,8,Basic,5,4,FALSE
    14,Package damaged,6,7,Premium,5,5,FALSE
    15,Product defective,6,2,Premium,5,5,FALSE
    ```

    Speichern und schließen Sie die Datei Data.csv, und Sie sich den Speicherort, in dem Sie gespeichert.

1. Klicken Sie im Projektmappen-Explorer den Namen des Projekts (in diesem Beispiel **QueueSample**), auf **Hinzufügen**und dann auf **Vorhandenes Element**.

1. Suchen Sie die Datei Data.csv, die Sie in Schritt 6 erstellt haben. Klicken Sie auf Datei und dann auf **Hinzufügen**. Sicherstellen, dass * *Alle Dateien (*.*) ** wird in der Liste der Dateitypen ausgewählt.

### <a name="create-a-method-that-parses-a-list-of-messages"></a>Erstellen Sie eine Methode, die eine Liste von Nachrichten analysiert

1. In der `Program` Klasse vor der `Main()` Methode deklarieren Sie zwei Variablen: eine Art **DataTable**für die Liste der Nachrichten in Data.csv. Andererseits sollte vom Typ Objekt zu [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx)stark typisiert. Letzteres ist vermittelten Nachrichtenliste, die Schritte im Lernprogramm verwendet wird.

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        class Program
        {
    
            private static DataTable issues;
            private static List<BrokeredMessage> MessageList;
    ```

1. Externe `Main()`, definieren Sie eine `ParseCSV()` Methode, die die Liste der Nachrichten in Data.csv analysiert und lädt die Nachrichten in einer [DataTable](https://msdn.microsoft.com/library/azure/system.data.datatable.aspx) -Tabelle wie folgt. Die Methode gibt ein **DataTable** -Objekt.

    ```
    static DataTable ParseCSVFile()
    {
        DataTable tableIssues = new DataTable("Issues");
        string path = @"..\..\data.csv";
        try
        {
            using (StreamReader readFile = new StreamReader(path))
            {
                string line;
                string[] row;
    
                // create the columns
                line = readFile.ReadLine();
                foreach (string columnTitle in line.Split(','))
                {
                    tableIssues.Columns.Add(columnTitle);
                }
    
                while ((line = readFile.ReadLine()) != null)
                {
                    row = line.Split(',');
                    tableIssues.Rows.Add(row);
                }
            }
        }
        catch (Exception e)
        {
            Console.WriteLine("Error:" + e.ToString());
        }
    
        return tableIssues;
    }
    ```

1. In die `Main()` -Methode fügen Sie eine Anweisung ruft die `ParseCSVFile()` Methode:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
    
    }
    ```

### <a name="create-a-method-that-loads-the-list-of-messages"></a>Erstellen Sie eine Methode, die die Liste der Nachrichten lädt

1. Externe `Main()`, Definieren einer `GenerateMessages()` Methode, die das **DataTable** -Objekt zurückgegebene `ParseCSVFile()` und die Tabelle in eine stark typisierte Liste von vermittelten Nachrichten. Die Methode gibt dann der **Liste** wie im folgenden Beispiel. 

    ```
    static List<BrokeredMessage> GenerateMessages(DataTable issues)
    {
        // Instantiate the brokered list object
        List<BrokeredMessage> result = new List<BrokeredMessage>();
    
        // Iterate through the table and create a brokered message for each row
        foreach (DataRow item in issues.Rows)
        {
            BrokeredMessage message = new BrokeredMessage();
            foreach (DataColumn property in issues.Columns)
            {
                message.Properties.Add(property.ColumnName, item[property]);
            }
            result.Add(message);
        }
        return result;
    }
    ```

1. In `Main()`, direkt nach dem Aufruf von `ParseCSVFile()`, fügen Sie eine Anweisung ruft die `GenerateMessages()` -Methode mit dem Rückgabewert von `ParseCSVFile()` als Argument:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
        MessageList = GenerateMessages(issues);
    }
    ```

### <a name="obtain-user-credentials"></a>Benutzeranmeldeinformationen abrufen

1. Erstellen Sie zunächst drei globalen String-Variablen, um diese Werte. Diese Variablen direkt nach dem vorherigen Variablendeklarationen deklariert. Zum Beispiel:

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        public class Program
        {
    
            private static DataTable issues;
            private static List<BrokeredMessage> MessageList; 

            // Add these variables
            private static string ServiceNamespace;
            private static string sasKeyName = "RootManageSharedAccessKey";
            private static string sasKeyValue;
            …
    ```

1. Als Nächstes erstellen Sie eine Funktion, die akzeptiert und speichert die Dienstnamespace und SAS-Taste. Fügen Sie dieser Methode außerhalb `Main()`. Zum Beispiel: 

    ```
    static void CollectUserInput()
    {
        // User service namespace
        Console.Write("Please enter the namespace to use: ");
        ServiceNamespace = Console.ReadLine();
    
        // Issuer key
        Console.Write("Enter the SAS key to use: ");
        sasKeyValue = Console.ReadLine();
    }
    ```

1. In `Main()`, direkt nach dem Aufruf von `GenerateMessages()`, fügen Sie eine Anweisung ruft die `CollectUserInput()` Methode:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
        MessageList = GenerateMessages(issues);
        
        // Collect user input
        CollectUserInput();
    }
    ```

### <a name="build-the-solution"></a>Erstellen Sie die Projektmappe

In Visual Studio im Menü **Erstellen** klicken Sie **Projektmappe** oder drücken Sie **STRG + UMSCHALT + B** , um die Richtigkeit Ihrer Arbeit bisher bestätigen.

## <a name="create-management-credentials"></a>Management-Anmeldeinformationen erstellen

In diesem Schritt definieren Sie Verwaltungsvorgänge, mit der gemeinsamen Zugriff Signatur (SAS) Anmeldeinformationen erstellen, mit denen Ihre Anwendung autorisiert werden.

1. Aus Gründen der Übersichtlichkeit setzt dieses Lernprogramm die Warteschlangenoperationen in eine separate Methode. Erstellen eine asynchronen `Queue()` -Methode in der `Program` -Klasse nach dem `Main()` Methode. Zum Beispiel:
 
    ```
    public static void Main(string[] args)
    {
    …
    }
    static async Task Queue()
    {
    }
    ```

1. Der nächste Schritt ist die Erstellung SAS-Anmeldeinformationen mit einem [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) -Objekt. Die Methode erhält die SAS-Schlüssel und Wert abgerufen der `CollectUserInput()` Methode. Fügen Sie den folgenden Code in die `Queue()` Methode:

    ```
    static async Task Queue()
    {
        // Create management credentials
        TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName,sasKeyValue);
    }
    ```

2. Erstellen einer neuen Namespace Verwaltungsobjekt mit einem URI den Namespacenamen mit Management Anmeldeinformationen als Argumente im vorherigen Schritt abgerufen. Fügen Sie diesen Code nach dem im vorherigen Schritt hinzugefügten Code. Ersetzen Sie `<yourNamespace>` mit dem Namen des Namespaces Service:
    
    ```
    NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    ```

### <a name="example"></a>Beispiel

Code sollte jetzt etwa wie folgt aussehen:

```
using System;
using System.Collections.Generic;
using System.Data;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.ServiceBus.Samples
{
  public class Program
  {
    private static DataTable issues;
    private static List<BrokeredMessage> MessageList;
    private static string ServiceNamespace;
    private static string sasKeyName = "RootManageSharedAccessKey";
    private static string sasKeyValue;

    static void Main(string[] args)
    {
      // Populate test data
      issues = ParseCSVFile();
      MessageList = GenerateMessages(issues);

      // Collect user input
      CollectUserInput();

      // Add this call
      Task.WaitAll(Queue());
    }

    static async Task Queue()
    {
      // Create management credentials
      TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName, sasKeyValue);
      NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    }

    static DataTable ParseCSVFile()
    {
      DataTable tableIssues = new DataTable("Issues");
      string path = @"..\..\data.csv";
      try
      {
        using (StreamReader readFile = new StreamReader(path))
        {
          string line;
          string[] row;

          // create the columns
          line = readFile.ReadLine();
          foreach (string columnTitle in line.Split(','))
          {
            tableIssues.Columns.Add(columnTitle);
          }

          while ((line = readFile.ReadLine()) != null)
          {
            row = line.Split(',');
            tableIssues.Rows.Add(row);
          }
        }
      }
      catch (Exception e)
      {
        Console.WriteLine("Error:" + e.ToString());
      }

      return tableIssues;
    }

    static List<BrokeredMessage> GenerateMessages(DataTable issues)
    {
      // Instantiate the brokered list object
      List<BrokeredMessage> result = new List<BrokeredMessage>();

      // Iterate through the table and create a brokered message for each row
      foreach (DataRow item in issues.Rows)
      {
        BrokeredMessage message = new BrokeredMessage();
        foreach (DataColumn property in issues.Columns)
        {
          message.Properties.Add(property.ColumnName, item[property]);
        }
        result.Add(message);
      }
      return result;
    }

    static void CollectUserInput()
    {
      // User service namespace
      Console.Write("Please enter the service namespace to use: ");
      ServiceNamespace = Console.ReadLine();

      // Issuer key
      Console.Write("Please enter the issuer key to use: ");
      sasKeyValue = Console.ReadLine();
    }
  }
}
```

## <a name="send-messages-to-the-queue"></a>Nachrichten an die Warteschlange senden

In diesem Schritt erstellen Sie eine Warteschlange und in der Liste der vermittelten Nachrichten an die Warteschlange senden.

### <a name="create-queue-and-send-messages-to-the-queue"></a>Erstellen von Warteschlangen und Nachrichten an die Warteschlange senden

1. Erstellen Sie zunächst die Warteschlange. Beispielsweise rufen `myQueue`, und nach Verwaltungsvorgänge in hinzugefügten deklarieren die `Queue()` -Methode im letzten Schritt:

    ```
    QueueDescription myQueue;

    if (namespaceClient.QueueExists("IssueTrackingQueue"))
    {
        namespaceClient.DeleteQueue("IssueTrackingQueue");
    }

    myQueue = namespaceClient.CreateQueue("IssueTrackingQueue");
    ```

1. In die `Queue()` -Methode erstellt ein Factoryobjekt messaging mit einem neu erstellten Service Bus URI als Argument. Fügen Sie folgenden Code direkt nach Verwaltungsoperationen im letzten Schritt hinzugefügt. Ersetzen Sie `<yourNamespace>` mit dem Namen des Namespaces Service:

    ```
    MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    ```

1. Erstellen Sie anschließend das Warteschlangenobjekt mithilfe der [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) -Klasse. Fügen Sie folgenden Code direkt nach dem Code, den Sie im letzten Schritt hinzugefügt:

    ```
    QueueClient myQueueClient = factory.CreateQueueClient("IssueTrackingQueue");
    ```

1. Fügen Sie Code vermittelten Nachrichtenliste durchläuft zuvor erstellten jede an die Warteschlange senden. Fügen Sie folgenden Code direkt nach dem `CreateQueueClient()` Anweisung im vorherigen Schritt:
    
    ```
    // Send messages
    Console.WriteLine("Now sending messages to the queue.");
    for (int count = 0; count < 6; count++)
    {
        var issue = MessageList[count];
        issue.Label = issue.Properties["IssueTitle"].ToString();
        await myQueueClient.SendAsync(issue);
        Console.WriteLine(string.Format("Message sent: {0}, {1}", issue.Label, issue.MessageId));
    }
    ```

## <a name="receive-messages-from-the-queue"></a>Nachrichten aus der Warteschlange

In diesem Schritt erhalten Sie die Liste der Nachrichten aus der Warteschlange, die Sie im vorherigen Schritt erstellt haben.

### <a name="create-a-receiver-and-receive-messages-from-the-queue"></a>Empfänger erstellen und Empfangen von Nachrichten aus der Warteschlange

In der `Queue()` , der Warteschlange durchlaufen und Empfangen von Nachrichten mithilfe der [QueueClient.ReceiveAsync](https://msdn.microsoft.com/library/azure/dn130423.aspx) -Methode jede Meldung in der Konsole ausgeben. Fügen Sie folgenden Code direkt nach dem im vorherigen Schritt hinzugefügten Code:

```
Console.WriteLine("Now receiving messages from Queue.");
BrokeredMessage message;
while ((message = await myQueueClient.ReceiveAsync(new TimeSpan(hours: 0, minutes: 1, seconds: 5))) != null)
    {
        Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
        message.Complete();
    
        Console.WriteLine("Processing message (sleeping...)");
        Thread.Sleep(1000);
    }
```

Beachten Sie, dass die `Thread.Sleep` nur zur Simulation der Nachricht verarbeiten und Sie wahrscheinlich nicht brauchen sie in echten Messaginganwendung.

### <a name="end-the-queue-method-and-clean-up-resources"></a>Beenden der Warteschlange-Methode und Ressourcen bereinigen

Fügen Sie den folgenden Code die Meldung Factory und Warteschlange Ressourcen zu bereinigen, direkt nach dem vorherigen Code:

```
factory.Close();
myQueueClient.Close();
namespaceClient.DeleteQueue("IssueTrackingQueue");
```

### <a name="call-the-queue-method"></a>Rufen Sie die Warteschlange-Methode

Der letzte Schritt ist das Hinzufügen Ruft eine Anweisung der `Queue()` aus `Main()`. Fügen Sie das folgende hervorgehobene Codezeile Ende Main() hinzu:
    
```
public static void Main(string[] args)
{
    // Collect user input
    CollectUserInput();
    
    // Populate test data
    issues = ParseCSVFile();
    MessageList = GenerateMessages(issues);
    
    // Add this call
    Queue();
}
```

### <a name="example"></a>Beispiel

Der folgende Code enthält die vollständige **QueueSample** -Anwendung.

```
using System;
using System.Collections.Generic;
using System.Data;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.ServiceBus.Samples
{
    public class Program
    {
        private static DataTable issues;
        private static List<BrokeredMessage> MessageList;

        // Add these variables
        private static string ServiceNamespace;
        private static string sasKeyName = "RootManageSharedAccessKey";
        private static string sasKeyValue;

        static void Main(string[] args)
        {

            // Populate test data
            issues = ParseCSVFile();
            MessageList = GenerateMessages(issues);

            // Collect user input
            CollectUserInput();

            Queue();

        }

        static async Task Queue()
        {
            // Create management credentials
            TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName, sasKeyValue);
            NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);

            QueueDescription myQueue;

            if (namespaceClient.QueueExists("IssueTrackingQueue"))
            {
                namespaceClient.DeleteQueue("IssueTrackingQueue");
            }
            
            myQueue = namespaceClient.CreateQueue("IssueTrackingQueue");
            
            MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);

            QueueClient myQueueClient = factory.CreateQueueClient("IssueTrackingQueue");

            // Send messages
            Console.WriteLine("Now sending messages to the queue.");
            for (int count = 0; count < 6; count++)
            {
                var issue = MessageList[count];
                issue.Label = issue.Properties["IssueTitle"].ToString();
                await myQueueClient.SendAsync(issue);
                Console.WriteLine(string.Format("Message sent: {0}, {1}", issue.Label, issue.MessageId));
            }

            Console.WriteLine("Now receiving messages from Queue.");
            BrokeredMessage message;
            while ((message = await myQueueClient.ReceiveAsync(new TimeSpan(hours: 0, minutes: 1, seconds: 5))) != null)
            {
                Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
                message.Complete();

                Console.WriteLine("Processing message (sleeping...)");
                Thread.Sleep(1000);
            }

            factory.Close();
            myQueueClient.Close();
            namespaceClient.DeleteQueue("IssueTrackingQueue");


        }

        static void CollectUserInput()
        {
            // User service namespace
            Console.Write("Please enter the namespace to use: ");
            ServiceNamespace = Console.ReadLine();

            // Issuer key
            Console.Write("Enter the SAS key to use: ");
            sasKeyValue = Console.ReadLine();
        }

        static List<BrokeredMessage> GenerateMessages(DataTable issues)
        {
            // Instantiate the brokered list object
            List<BrokeredMessage> result = new List<BrokeredMessage>();

            // Iterate through the table and create a brokered message for each row
            foreach (DataRow item in issues.Rows)
            {
                BrokeredMessage message = new BrokeredMessage();
                foreach (DataColumn property in issues.Columns)
                {
                    message.Properties.Add(property.ColumnName, item[property]);
                }
                result.Add(message);
            }
            return result;
        }

        static DataTable ParseCSVFile()
        {
            DataTable tableIssues = new DataTable("Issues");
            string path = @"..\..\data.csv";
            try
            {
                using (StreamReader readFile = new StreamReader(path))
                {
                    string line;
                    string[] row;

                    // create the columns
                    line = readFile.ReadLine();
                    foreach (string columnTitle in line.Split(','))
                    {
                        tableIssues.Columns.Add(columnTitle);
                    }

                    while ((line = readFile.ReadLine()) != null)
                    {
                        row = line.Split(',');
                        tableIssues.Rows.Add(row);
                    }
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Error:" + e.ToString());
            }

            return tableIssues;
        }
    }
}
```

## <a name="build-and-run-the-queuesample-application"></a>QueueSample-Anwendung erstellen und ausführen

Nun, dass Sie die vorherigen Schritte abgeschlossen haben, können erstellen und Ausführen die Anwendung **QueueSample** .

### <a name="build-the-queuesample-application"></a>QueueSample-Anwendung erstellen

In Visual Studio im Menü **Erstellen** auf **Projektmappe erstellen**oder drücken Sie **STRG + UMSCHALT + B**. Wenn Fehler auftreten, überprüfen Sie, dass der Code korrekt ist basierend auf im vollständigen Beispiel am Ende des vorherigen Schrittes.

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm zeigte, wie ein Client Service Bus Anwendung und Service Service Bus vermittelte Messagingfunktionen erstellen. Ein ähnlich, die Service Bus [Relay](service-bus-messaging-overview.md#Relayed-messaging)verwendet, finden Sie unter [Service Bus messaging Lernprogramm weitergeleitet](../service-bus-relay/service-bus-relay-tutorial.md).

Weitere Informationen zu [Service Bus](https://azure.microsoft.com/services/service-bus/)finden Sie unter den folgenden Themen.

- [Übersicht über Service Bus](service-bus-messaging-overview.md)
- [Service Bus-Grundlagen](service-bus-fundamentals-hybrid-solutions.md)
- [Service-Architektur](service-bus-architecture.md)

