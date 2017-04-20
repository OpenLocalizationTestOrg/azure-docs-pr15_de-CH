<properties 
    pageTitle="Lang andauernde Vorgänge abrufen | Microsoft Azure" 
    description="Dieses Thema zeigt, wie lang andauernde Vorgänge Abfragen." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="juliako"/>


#<a name="delivering-live-streaming-with-azure-media-services"></a>Bereitstellung von Live-Streaming mit Azure Media Services

##<a name="overview"></a>Übersicht

Microsoft Azure Media Services bieten APIs, die Anfragen an Media-Dienste Betrieb (z. B.: erstellen, starten, beenden oder Löschen eines Kanals). Diese Vorgänge sind langlebige.

Media Services .NET SDK enthält APIs, die Anforderung senden und nach Abschluss des Vorgangs warten (intern die APIs sind abrufen Arbeitsgangstatus in einigen Intervallen). Beispielsweise wenn Sie Kanal aufrufen. Start(), gibt die Methode nach dem Start des Kanals. Sie können auch die asynchrone Version: Kanal erwarten. StartAsync() (Informationen zu asynchronen Muster aufgabenbasierte finden Sie [TIPPEN](https://msdn.microsoft.com/library/hh873175(v=vs.110).aspx)). APIs, die eine Operation-Anforderung senden und dann den Status Abfragen, bis der Vorgang abgeschlossen ist, heißen "Polling-Methoden". Diese Methoden (insbesondere die asynchrone Version) für umfangreiche Clientanwendungen und statusbehaftete Dienste empfohlen.

Es gibt Szenarien, in denen eine Anwendung nicht langer HTTP-Anforderung warten und laufenden Vorgang manuell abrufen möchte. Ein Beispiel wäre ein Browser eine statusfreie Webdienst interagieren: der Browser fordert einen Kanal erstellen, der Webdienst initiiert einen zeitaufwändigen Vorgang als Vorgangs-ID an den Browser zurückgibt. Der Browser konnte den Webdienst Vorgangsstatus auf der Grundlage der ID zu bitten Media Services .NET SDK enthält APIs, die für dieses Szenario hilfreich sind. Diese APIs werden "-Polling-Methoden" bezeichnet.
"-Polling-Methoden" haben das folgende Muster: Sendevorgang*OperationName*(z. B. SendCreateOperation). Senden Sie*OperationName*Methoden **IOperations** -Objekts zurück. Das zurückgegebene Objekt enthält Informationen, die verwendet werden kann, um den Vorgang zu verfolgen. Senden*OperationName*OperationAsync Methoden zurückgeben **Aufgabe<IOperation>**.

Die folgenden Klassen unterstützen gegenwärtig nicht Polling-Methoden: ** **Kanal**, **StreamingEndpoint**und**.

Um den Vorgangsstatus abzufragen verwenden Sie die **GetOperation** -Methode der **OperationBaseCollection** -Klasse. Folgende Intervalle verwenden, um den Vorgangsstatus überprüfen: Verwenden Sie für **Channel** und **StreamingEndpoint** 30 Sekunden; **Programm** -Operationen verwenden Sie 10 Sekunden.


##<a name="example"></a>Beispiel

Im folgende Beispiel wird die **ChannelOperations**-Klasse definiert. Diese Klassendefinition könnte ein Ausgangspunkt für die Klassendefinition des Webdiensts. Der Einfachheit halber verwenden die folgenden Versionen nicht asynchrone Methoden.

Das Beispiel zeigt auch, wie der Client diese Klasse verwenden kann.

###<a name="channeloperations-class-definition"></a>Definition der ChannelOperations-Klasse

    /// <summary> 
    /// The ChannelOperations class only implements 
    /// the Channel’s creation operation. 
    /// </summary> 
    public class ChannelOperations
    {
        // Read values from the App.config file.
        private static readonly string _mediaServicesAccountName =
            ConfigurationManager.AppSettings["MediaServicesAccountName"];
        private static readonly string _mediaServicesAccountKey =
            ConfigurationManager.AppSettings["MediaServicesAccountKey"];
    
        // Field for service context.
        private static CloudMediaContext _context = null;
        private static MediaServicesCredentials _cachedCredentials = null;
    
        public ChannelOperations()
        {
                _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName,
                    _mediaServicesAccountKey);
    
                _context = new CloudMediaContext(_cachedCredentials);    }
    
        /// <summary>  
        /// Initiates the creation of a new channel.  
        /// </summary>  
        /// <param name="channelName">Name to be given to the new channel</param>  
        /// <returns>  
        /// Operation Id for the long running operation being executed by Media Services. 
        /// Use this operation Id to poll for the channel creation status. 
        /// </returns> 
        public string StartChannelCreation(string channelName)
        {
            var operation = _context.Channels.SendCreateOperation(
                new ChannelCreationOptions
                {
                    Name = channelName,
                    Input = CreateChannelInput(),
                    Preview = CreateChannelPreview(),
                    Output = CreateChannelOutput()
                });
    
            return operation.Id;
        }
    
        /// <summary> 
        /// Checks if the operation has been completed. 
        /// If the operation succeeded, the created channel Id is returned in the out parameter.
        /// </summary> 
        /// <param name="operationId">The operation Id.</param> 
        /// <param name="channel">
        /// If the operation succeeded, 
        /// the created channel Id is returned in the out parameter.</param>
        /// <returns>Returns false if the operation is still in progress; otherwise, true.</returns> 
        public bool IsCompleted(string operationId, out string channelId)
        {
            IOperation operation = _context.Operations.GetOperation(operationId);
            bool completed = false;
    
            channelId = null;
    
            switch (operation.State)
            {
                case OperationState.Failed:
                    // Handle the failure. 
                    // For example, throw an exception. 
                    // Use the following information in the exception: operationId, operation.ErrorMessage.
                    break;
                case OperationState.Succeeded:
                    completed = true;
                    channelId = operation.TargetEntityId;
                    break;
                case OperationState.InProgress:
                    completed = false;
                    break;
            }
            return completed;
        }
    
    
        private static ChannelInput CreateChannelInput()
        {
            return new ChannelInput
            {
                StreamingProtocol = StreamingProtocol.RTMP,
                AccessControl = new ChannelAccessControl
                {
                    IPAllowList = new List<IPRange>
                    {
                        new IPRange
                        {
                            Name = "TestChannelInput001",
                            Address = IPAddress.Parse("0.0.0.0"),
                            SubnetPrefixLength = 0
                        }
                    }
                }
            };
        }
    
        private static ChannelPreview CreateChannelPreview()
        {
            return new ChannelPreview
            {
                AccessControl = new ChannelAccessControl
                {
                    IPAllowList = new List<IPRange>
                    {
                        new IPRange
                        {
                            Name = "TestChannelPreview001",
                            Address = IPAddress.Parse("0.0.0.0"),
                            SubnetPrefixLength = 0
                        }
                    }
                }
            };
        }
    
        private static ChannelOutput CreateChannelOutput()
        {
            return new ChannelOutput
            {
                Hls = new ChannelOutputHls { FragmentsPerSegment = 1 }
            };
        }
    }

###<a name="the-client-code"></a>Der Clientcode

    ChannelOperations channelOperations = new ChannelOperations();
    string opId = channelOperations.StartChannelCreation("MyChannel001");
    
    string channelId = null;
    bool isCompleted = false;
    
    while (isCompleted == false)
    {
        System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
        isCompleted = channelOperations.IsCompleted(opId, out channelId);
    }
    
    // If we got here, we should have the newly created channel id.
    Console.WriteLine(channelId);
 


##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
