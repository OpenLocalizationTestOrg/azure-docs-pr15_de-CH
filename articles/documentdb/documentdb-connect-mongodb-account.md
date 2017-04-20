<properties 
    pageTitle="Ein DocumentDB Konto Protokoll unterstützt MongoDB | Microsoft Azure" 
    description="Erfahren Sie, wie ein Konto DocumentDB Protokoll unterstützt jetzt verfügbar für Vorschau MongoDB herstellen. Verbinden Sie die MongoDB-Verbindungszeichenfolge." 
    keywords="MongoDB-Verbindungszeichenfolge"
    services="documentdb" 
    authors="AndrewHoh" 
    manager="jhubbard" 
    editor="" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="anhoh"/>

# <a name="how-to-connect-to-a-documentdb-account-with-protocol-support-for-mongodb"></a>DocumentDB-Konto Protokoll unterstützt MongoDB Verbindung

Erfahren Sie, wie ein Konto Azure DocumentDB Protokoll unterstützt standard MongoDB URI Verbindungszeichenfolgenformat mit MongoDB herstellen.  

## <a name="get-the-accounts-connection-string-information"></a>Informationen zur Verbindungszeichenfolge des Kontos abrufen

1. Melden Sie sich in einem neuen Fenster der [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie in der **Linken** Navigationsleiste der Konto **-Verbindungszeichenfolge**. Zum **Konto Blade**zu navigieren, auf der Indexleiste auf **Mehr Dienste**, klicken Sie auf **DocumentDB (NoSQL)** und wählen Sie das Konto DocumentDB Protokoll unterstützt MongoDB.

    ![Screenshot des alle Blades](./media/documentdb-connect-mongodb-account/SettingsBlade.png)

3. **Verbindungszeichenfolge** Blade wird geöffnet und alle Informationen zur Verbindung mit MongoDB eine bereits erstellte Verbindungszeichenfolge über ein Konto verfügt.

    ![Screenshot des Connection String Blades](./media/documentdb-connect-mongodb-account/ConnectionStringBlade.png)

## <a name="connection-string-requirements"></a>Zeichenfolge Anforderungen

Muss DocumentDB standard MongoDB Format der Verbindungszeichenfolge URI, mit wenigen Anforderungen unterstützt: DocumentDB Konten Authentifizierung und sichere Kommunikation über SSL erforderlich.  Daher ist das Format der Verbindungszeichenfolge:

    mongodb://username:password@host:port/[database]?ssl=true

Wo sind die Werte dieser Zeichenfolge in der obigen Verbindungszeichenfolge Blade verfügbar.

- Benutzername (erforderlich)
    - DocumentDB Namen
- Kennwort (erforderlich)
    - DocumentDB Kennwort
- Host (erforderlich)
    - FQDN des DocumentDB-Konto
- Port (erforderlich)
    - 10250
- Datenbank (optional)
    - Die von der Verbindung verwendeten Standarddatenbank
- SSL = True (erforderlich)

Betrachten Sie beispielsweise das Konto in der obigen Verbindungszeichenfolge.  Eine gültige Verbindungszeichenfolge ist:
    
    mongodb://contoso123:<password@contoso123.documents.azure.com:10250/mydatabase?ssl=true

## <a name="connecting-with-the-c-driver-for-mongodb"></a>Herstellen einer Verbindung mit dem C#-Treiber für MongoDB
Wie bereits erwähnt müssen alle DocumentDB-Konten Authentifizierung und sichere Kommunikation über SSL. MongoDB URI Verbindungszeichenfolgenformat Ssl unterstützt = true Abfragezeichenfolgenparameter, arbeiten mit der MongoDB C# Treiber benötigen Sie das MongoClientSettings-Objekt beim Erstellen einer MongoClient.  Wenn die oben genannten Informationen, veranschaulicht der folgende Codeausschnitt zu dem Konto verbinden mit der Datenbank "Aufgaben".

            MongoClientSettings settings = new MongoClientSettings();
            settings.Server = new MongoServerAddress("contoso123.documents.azure.com", 10250);
            settings.UseSsl = true;
            settings.SslSettings = new SslSettings();
            settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;

            MongoIdentity identity = new MongoInternalIdentity("Tasks", "contoso123");
            MongoIdentityEvidence evidence = new PasswordEvidence("<password>");

            settings.Credentials = new List<MongoCredential>()
            {
                new MongoCredential("SCRAM-SHA-1", identity, evidence)
            };
            MongoClient client = new MongoClient(settings);
            var database = client.GetDatabase("Tasks",);
    

## <a name="next-steps"></a>Nächste Schritte


- Erfahren Sie, wie [MongoChef verwenden](documentdb-mongodb-mongochef.md) , mit einem DocumentDB-Konto mit Unterstützung für MongoDB.
- Untersuchen Sie DocumentDB Protokoll unterstützt MongoDB [Beispiele](documentdb-mongodb-samples.md).

 
