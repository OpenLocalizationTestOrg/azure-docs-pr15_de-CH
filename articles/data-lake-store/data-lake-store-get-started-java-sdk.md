<properties
   pageTitle="Daten See Speicher Java SDK zur Anwendungsentwicklung verwenden | Microsoft Azure"
   description="Verwenden Sie Azure Data Lake Speicher Java SDK zur Anwendungsentwicklung"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/17/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-java"></a>Erste Schritte mit Azure See Datenspeicher mit Java

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST-API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Informationen Sie zum Azure Data Lake Speicher Java SDK verwenden grundlegende Operationen ausführen, die z. b. Ordner erstellen hoch- und Herunterladen von Dateien usw. Weitere Informationen zu Data Lake finden Sie unter [Azure See Datenspeicher](data-lake-store-overview.md).

Sie können Java SDK API-Dokumentation für Azure See Datenspeicher in [Azure Data Lake Speicher Java API-Dokumentation](https://azure.github.io/azure-data-lake-store-java/javadoc/)zugreifen.

## <a name="prerequisites"></a>Erforderliche Komponenten

* Java Development Kit (JDK 7 oder höher, mit Java Version 1.7 oder höher)
* See Datenspeicher Azure-Konto. Gehen Sie auf der [Einstieg in Azure See Datenspeicher Azure-Portal](data-lake-store-get-started-portal.md).
* [Maven](https://maven.apache.org/install.html). In diesem Lernprogramm verwendet Maven für Build- und Project Dependencies. Zwar kann ohne Erstellen einer Buildsystem wie Maven oder Gradle ist diese Systeme einfacher Abhängigkeiten verwalten.
* (Optional) Und wie [IntelliJ IDEE](https://www.jetbrains.com/idea/download/) oder [Eclipse](https://www.eclipse.org/downloads/) -IDE oder ähnlich.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Wie werden ich mithilfe von Azure Active Directory authentifiziert?

In diesem Lernprogramm verwenden wir einen geheimen Azure AD-Anwendung einen Azure Active Directory Token (Dienst-Authentifizierung) abgerufen. Wir verwenden dieses Token einen Datenspeicher See Client-Objekt Operationen Datei- und Operationen. Hinweise Azure See Datenspeicher Clientschlüssel Authentifizierung führen wir die folgenden Schritte:

1. Erstellen Sie eine Azure AD
2. Die Client-ID, geheimen und token Endpunkt für Azure AD der Anwendung abzurufen.
3. Konfigurieren des Zugriffs für Azure AD der Anwendung auf See Datenspeicher Datei oder der Ordner, den die Java-Anwendung zugreifen, die Sie erstellen möchten.

Informationen zum Ausführen dieser Schritte finden Sie unter [Erstellen einer Active Directory-Anwendung](data-lake-store-authenticate-using-active-directory.md#create-an-active-directory-application).

Azure Active Directory bietet andere Optionen, ein Token abzurufen. Sie können aus einer Reihe von Authentifizierungsverfahren entsprechend Szenario z. B. eine Anwendung in einem Browser, eine Anwendung wie eine desktop-Anwendung oder eine Server-Anwendung mit lokalen oder in Azure virtuellen Computer auswählen. Sie können auch aus verschiedenen Typen von Anmeldeinformationen, Kennwörtern, Zertifikaten, 2-Faktor-Authentifizierung auswählen. Darüber hinaus können mit Azure Active Directory Ihre lokale Active Directory-Benutzer mit der Cloud synchronisieren. Weitere Informationen finden Sie unter [Authentifizierungsszenarien Azure Active Directory](../active-directory/active-directory-authentication-scenarios.md). 

## <a name="create-a-java-application"></a>Erstellen Sie eine Java-Anwendung

Das Codebeispiel verfügbar [auf GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/) führt Sie durch den Prozess der Erstellung im Informationsspeicher Dateien Verketten von Dateien herunterladen einer Datei, und löschen Sie einige Dateien in den Speicher. In diesem Abschnitt des Artikels gehen Sie durch die wichtigsten Teile des Codes.

1. Erstellen eines Maven mit [Mvn-Urtyp](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) über die Befehlszeile oder mithilfe einer IDE. Anleitung zum Erstellen eines Java-Projekts mit IntelliJ finden Sie [hier](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html). Anleitung zum Erstellen eines Projekts mit Eclipse finden Sie [hier](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm). 

2. Die folgenden abhängigen Maven **pom.xml** Datei hinzufügen Fügen Sie den folgenden Codeausschnitt zwischen dem ** \</Version >** Tag und ** \</project >** Tag:

        <dependencies>
          <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-data-lake-store-sdk</artifactId>
            <version>2.0.4-SNAPSHOT</version>
          </dependency>
          <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-nop</artifactId>
            <version>1.7.21</version>
          </dependency>
        </dependencies>

    Die erste Abhängigkeit ist die Verwendung Data See Speicher SDK (`azure-datalake-store`) aus dem Repository Maven. Die zweite Abhängigkeit (`slf4j-nop`) ist die protokollierungsframework für diese Anwendung angeben. Daten See Speicher SDK verwendet [slf4j](http://www.slf4j.org/) Protokollierung Fassade, die Sie aus einer Reihe von gängigen Protokollierung Frameworks wie log4j, Java-Protokollierung, Logback usw. auswählen kann oder nicht. In diesem Beispiel wir Protokollierung deaktiviert, daher verwenden wir die **slf4j Nop** -Bindung. Andere Protokollierungsoptionen in Ihrer Anwendung verwenden, finden Sie [hier](http://www.slf4j.org/manual.html#projectDep).

### <a name="add-the-application-code"></a>Fügen Sie den code

Es gibt drei Hauptkomponenten der Code.

1. Azure Active Directory Token abrufen

2. Verwenden Sie das Token See Datenspeicher Client erstellen.

3. Verwenden Sie den Datenspeicher See Client Operationen.

#### <a name="step-1-obtain-an-azure-active-directory-token"></a>Schritt 1: Einen Token Azure Active Directory abrufen.

Daten See Speicher SDK bietet benutzerfreundliche Methoden, die mit dem Datenspeicher See Konto benötigten Sicherheitstoken zu erhalten. Das SDK ist jedoch nicht festgelegt werden, dass diese Methoden verwendet werden. Andere Mittel, Token wie [Azure Active Directory SDK](https://github.com/AzureAD/azure-activedirectory-library-for-java)oder Ihren eigenen benutzerdefinierten Code erhalten können.

Um Daten See Speicher SDK Token für die Anwendung Active Directory erhalten Sie zuvor erstellt haben, verwenden die statischen Methoden in `AzureADAuthenticator` Klasse. Die tatsächlichen Werte für Azure Active Directory Anwendung ersetzen Sie **Hier ausfüllen** .

    private static String clientId = "FILL-IN-HERE";
    private static String authTokenEndpoint = "FILL-IN-HERE";
    private static String clientKey = "FILL-IN-HERE";

    AzureADToken token = AzureADAuthenticator.getTokenUsingClientCreds(authTokenEndpoint, clientId, clientKey);

#### <a name="step-2-create-an-azure-data-lake-store-client-adlstoreclient-object"></a>Schritt 2: Erstellen einer Azure See Datenspeicher Clientobjekt (ADLStoreClient)

Erstellen eines [ADLStoreClient](https://azure.github.io/azure-data-lake-store-java/javadoc/) -Objekts erfordert See Datenspeicher Kontonamen und Azure Active Directory-Token, das Sie im letzten Schritt generiert. Beachten Sie, dass der Kontoname See Datenspeicher ein vollqualifizierter Domänenname sein. Beispielsweise ersetzen Sie **Ausfüllen hier** etwas **mydatalakestore.azuredatalakestore.net**.

    private static String accountFQDN = "FILL-IN-HERE";  // full account FQDN, not just the account name
    ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, token);

### <a name="step-3-use-the-adlstoreclient-to-perform-file-and-directory-operations"></a>Schritt 3: Verwenden der ADLStoreClient Datei- und Operationen

Der folgende Code enthält Beispiel Codeausschnitte häufig verwendete Vorgänge. Betrachten Sie die vollständigen [Daten See Speicher Java SDK API-Dokumentation](https://azure.github.io/azure-data-lake-store-java/javadoc/) des **ADLStoreClient** -Objekts auf andere Vorgänge.
 
Beachten Sie, dass Dateien lesen und Schreiben mit Java Standarddatenströme. Dies bedeutet, daß Sie alle Java-Streams auf See Datenspeicher Streams Standardfunktionen Java (z. B. Print Streams für formatierte Ausgabe oder Komprimierung oder Verschlüsselung Streams zusätzliche Funktionen oben usw.) nutzen können.

    // set file permission
    client.setPermission(filename, "744");

    // append to file
    stream = client.getAppendStream(filename);
    stream.write(getSampleContent());
    stream.close();

    // Read File
    InputStream in = client.getReadStream(filename);
    byte[] b = new byte[64000];
    while (in.read(b) != -1) {
        System.out.write(b);
    }
    in.close();

    // concatenate the two files into one
    List<String> fileList = Arrays.asList("/a/b/c.txt", "/a/b/d.txt");
    client.concatenateFiles("/a/b/f.txt", fileList);

    //rename the file
    client.rename("/a/b/f.txt", "/a/b/g.txt");

    // list directory contents
    List<DirectoryEntry> list = client.enumerateDirectory("/a/b", 2000);
    System.out.println("Directory listing for directory /a/b:");
    for (DirectoryEntry entry : list) {
        printDirectoryInfo(entry);
    }

    // delete directory along with all the subdirectories and files in it
    client.deleteRecursive("/a");

#### <a name="step-4-build-and-run-the-application"></a>Schritt 4: Erstellen und Ausführen der Anwendungdes

1. Führen Sie von einer IDE suchen und der Schaltfläche **Ausführen** . Führen von Maven mit [Exec: Exec](http://www.mojohaus.org/exec-maven-plugin/exec-mojo.html).

2. Erstellen eigenständige JAR-Datei, die Sie aus der Befehlszeile ausführen die JAR-Datei mit allen Abhängigkeiten enthalten, mit dem [Maven Assembly-Plug-in](http://maven.apache.org/plugins/maven-assembly-plugin/usage.html)erstellen. Im [Beispiel-Quellcode auf Github](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started/blob/master/pom.xml) pom.xml enthält ein Beispiel dazu.


## <a name="next-steps"></a>Nächste Schritte

- [Sichere Daten im Datenspeicher See](data-lake-store-secure-data.md)
- [Verwenden Sie Azure Data Lake Analytics See Datenspeicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Verwenden von Azure HDInsight Lake Datenspeicher](data-lake-store-hdinsight-hadoop-use-portal.md)
