<properties
    pageTitle="Lokal mit BLOB-Speicher (Java) | Microsoft Azure"
    description="Erfahren Sie, wie eine erstellen, ein Bild in Azure hochgeladen und zeigt das Bild im Browser. Codebeispiele in Java."
    services="storage"
    documentationCenter="java"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="on-premises-application-with-blob-storage"></a>Lokal mit BLOB-Speicher

## <a name="overview"></a>Übersicht

Das folgende Beispiel zeigt eine mögliche Verwendung von Azure-Speicher zum Speichern von Bildern in Azure. Der Code in diesem Artikel ist für eine Konsolenanwendung, die ein Bild in Azure hochgeladen und erstellt eine HTML-Datei, in der das Bild im Browser angezeigt.

## <a name="prerequisites"></a>Erforderliche Komponenten

- Eine Java Developer Kit (JDK), Version 1.6 oder höher, installiert ist.
- Azure SDK installiert.
- Azure-Bibliotheken für Java JAR-Datei alle anwendbaren Abhängigkeit Gläser installiert sind und im Erstellungspfad vom Java-Compiler verwendet werden. Informationen zum Installieren von Azure Bibliotheken für Java finden Sie unter [Download Azure SDK für Java](java-download-azure-sdk.md).
- Ein Azure Storage-Konto eingerichtet wurde. Den Namen und das kontoschlüssel für das Speicherkonto werden durch den Code in diesem Artikel verwendet. [So erstellen Sie ein Speicherkonto](storage-create-storage-account.md#create-a-storage-account) Informationen ein Speicherkonto, und [anzeigen und kopieren Storage Access](storage-create-storage-account.md#view-and-copy-storage-access-keys) Informationen zum Abrufen von kontoschlüssel anzeigen

- Sie haben eine lokale Bild-Datei gespeichert unter Pfad c:\\Myimages\\image1.jpg. Alternativ ändern Sie   **FileInputStream** Konstruktor in diesem Beispiel verwenden Sie ein anderes Bild Pfad und Dateiname.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="to-use-azure-blob-storage-to-upload-a-file"></a>Hochladen einer Datei mit Azure BLOB-Speicher

Eine schrittweise Anleitung wird hier angezeigt. Wenn Sie überspringen möchten, ist der gesamte Code in diesem Artikel dargestellt.

Zunächst den Code einschließlich Importe Speicherklassen Azure Core, Azure Blob-Clientklassen Java IO-Klassen und die **URISyntaxException** -Klasse.

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import java.io.*;
    import java.net.URISyntaxException;

Deklarieren Sie eine Klasse mit dem Namen **StorageSample**und die öffnenden Klammer { **}**enthalten.

    public class StorageSample {

Deklarieren Sie innerhalb der Klasse **StorageSample** eine Zeichenfolgenvariable, die Endpunkt-Standardprotokoll speicherkontonamen und den speicherzugriffsschlüssel gemäß Ihrem Konto Azure-Speicher enthalten. Ersetzen Sie die Platzhalterwerte **der\_Konto\_Namen** und **der\_Konto\_Key** mit Ihrem eigenen Konto und Schlüssel bzw..

    public static final String storageConnectionString =
           "DefaultEndpointsProtocol=http;" +
               "AccountName=your_account_name;" +
               "AccountKey=your_account_name";

In der Deklaration für **Hauptmenü**hinzufügen ein **try** -Blocks, und fügen die erforderlichen offenen Klammern **{**.

    public static void main(String[] args)
    {
        try
        {

Variablen der folgenden Typen (die Beschreibung sind für die Verwendung in diesem Beispiel):

-   **CloudStorageAccount**: verwendet zum Initialisieren des Objekts Konto mit den Azure-speicherkontonamen und Schlüssel, das Client-BLOB.
-   **CloudBlobClient**: Zugriff auf BLOB-Dienst verwendet.
-   **CloudBlobContainer**: verwendet einen BLOB-Container erstellen, die Blobs im Container aufgelistet und den Container löschen.
-   **CloudBlockBlob**: eine lokale Bild Hochladen der Container verwendet.

<!-- -->

    CloudStorageAccount account;
    CloudBlobClient serviceClient;
    CloudBlobContainer container;
    CloudBlockBlob blob;

**Konto** -Variablen einen Wert zuweisen.

    account = CloudStorageAccount.parse(storageConnectionString);

**ServiceClient** -Variablen einen Wert zuweisen.

    serviceClient = account.createCloudBlobClient();

**Container** -Variablen einen Wert zuweisen. Wir erhalten einen Verweis auf ein Container namens **Erste Schritte**.

    // Container name must be lower case.
    container = serviceClient.getContainerReference("gettingstarted");

Erstellen Sie den Container. Diese Methode wird den Container erstellen, wenn sie nicht vorhanden ist (und **true**zurückgeben). Wenn der Container vorhanden ist, wird **false**zurückgegeben. Eine Alternative zur **CreateIfNotExists** ist die **create** -Methode (die einen Fehler zurückgibt, wenn der Container bereits vorhanden ist).

    container.createIfNotExists();

Anonymen Zugriff für den Container festgelegt.

    // Set anonymous access on the container.
    BlobContainerPermissions containerPermissions;
    containerPermissions = new BlobContainerPermissions();
    containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
    container.uploadPermissions(containerPermissions);

Abrufen eines Verweises auf Block-Blob, das Blob in Azure-Speicher darstellen.

    blob = container.getBlockBlobReference("image1.jpg");

Verwenden Sie den **Datei** -Konstruktor zu auf lokal erstellte Datei, die Sie hochladen möchten. Stellen Sie sicher, dass Sie diese Datei erstellt haben, bevor der Code ausgeführt.

    File fileReference = new File ("c:\\myimages\\image1.jpg");

Laden Sie die lokale Datei durch einen Aufruf der **CloudBlockBlob.upload** -Methode. Der erste Parameter der Methode **CloudBlockBlob.upload** ist ein **FileInputStream** -Objekt, das die lokale Datei darstellt, die in Azure-Speicher hochgeladen werden. Der zweite Parameter ist die Größe in Bytes der Datei.

    blob.upload(new FileInputStream(fileReference), fileReference.length());

Rufen Sie eine Hilfsfunktion namens **MakeHTMLPage**zu enthält eine einfache HTML-Seite ein ** &lt;Bild&gt; ** mit der Quelle festgelegt, Blob, das jetzt in Ihrem Konto Azure-Speicher ist. Der Code für **MakeHTMLPage** werden weiter unten in diesem Artikel behandelt.

    MakeHTMLPage(container);

Drucken Sie einen Status und Informationen über die erstellte HTML-Seite.

    System.out.println("Processing complete.");
    System.out.println("Open index.html to see the images stored in your storage account.");

Den **try** -Block durch Einfügen einer schließenden Klammer schließen: **}**

Behandeln Sie die folgenden Ausnahmen:

-   **FileNotFoundException**: **FileInputStream** oder **FileOutputStream** Konstruktoren ausgelöst werden.
-   **StorageException**: von der Azure-Speicher-Clientbibliothek ausgelöst werden.
-   **URISyntaxException**: von der **ListBlobItem.getUri** -Methode ausgelöst werden können.
-   **Ausnahme**: generische Ausnahmebehandlung.

<!-- -->

    catch (FileNotFoundException fileNotFoundException)
    {
        System.out.print("FileNotFoundException encountered: ");
        System.out.println(fileNotFoundException.getMessage());
        System.exit(-1);
    }
    catch (StorageException storageException)
    {
        System.out.print("StorageException encountered: ");
        System.out.println(storageException.getMessage());
        System.exit(-1);
    }
    catch (URISyntaxException uriSyntaxException)
    {
        System.out.print("URISyntaxException encountered: ");
        System.out.println(uriSyntaxException.getMessage());
        System.exit(-1);
    }
    catch (Exception e)
    {
        System.out.print("Exception encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

**Hauptfenster** durch Einfügen einer schließenden Klammer schließen: **}**

Erstellen Sie eine Methode namens **MakeHTMLPage** zum Erstellen einer einfachen HTML-Seite. Diese Methode hat einen Parameter vom Typ **CloudBlobContainer**, die verwendet wird, um die Liste der hochgeladenen Blobs durchlaufen. Diese Methode löst Ausnahmen vom Typ **FileNotFoundException**, die ausgelöst werden können **FileOutputStream** Konstruktor und **URISyntaxException**, die von der **ListBlobItem.getUri** -Methode ausgelöst werden können. Klammer { **}**enthalten.

    public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
    {

Erstellen Sie eine lokale Datei **index.html**.

    PrintStream stream;
    stream = new PrintStream(new FileOutputStream("index.html"));

Schreiben in lokale Datei hinzufügen in der ** &lt;html&gt;**, ** &lt;Header&gt;**, und ** &lt;Body&gt; ** Elemente.

    stream.println("<html>");
    stream.println("<header/>");
    stream.println("<body>");

Durchlaufen Sie die Liste der hochgeladenen Blobs. Erstellen für jedes Blob in der HTML-Seite ein ** &lt;Img&gt; ** Element mit seinem **Src** -Attribut gesendet an den URI des BLOBs in Azure Storage-Konto vorhanden ist.
Obwohl Sie nur ein Bild in diesem Beispiel hinzugefügt, wenn mehr hinzugefügt, würde dieser Code alle durchlaufen.

Der Einfachheit halber wird angenommen, dass jeder hochgeladenen Blob ein Bild ist. Wenn Sie Blobs als Bilder oder Seitenblobs statt Block-Blobs aktualisiert haben, passen Sie den Code nach Bedarf an.

    // Enumerate the uploaded blobs.
    for (ListBlobItem blobItem : container.listBlobs()) {
    // List each blob as an <img> element in the HTML body.
    stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
    }

Schließen der ** &lt;Body&gt; ** Element und ** &lt;html&gt; ** Element.

    stream.println("</body>");
    stream.println("</html>");

Schließen Sie die lokale Datei.

    stream.close();

**MakeHTMLPage** durch Einfügen einer schließenden Klammer schließen: **}**

**StorageSample** durch Einfügen einer schließenden Klammer schließen: **}**

Folgendes ist der vollständige Code für dieses Beispiel. Denken Sie daran, die Platzhalter ändern **der\_Konto\_Namen** und **der\_Konto\_Schlüssel** Ihren Kontonamen und kontoschlüssel verwenden.

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import java.io.*;
    import java.net.URISyntaxException;

    // Create an image, c:\myimages\image1.jpg, prior to running this sample.
    // Alternatively, change the value used by the FileInputStream constructor
    // to use a different image path and file that you have already created.
    public class StorageSample {

        public static final String storageConnectionString =
                "DefaultEndpointsProtocol=http;" +
                       "AccountName=your_account_name;" +
                       "AccountKey=your_account_name";

        public static void main(String[] args) {
            try {
                CloudStorageAccount account;
                CloudBlobClient serviceClient;
                CloudBlobContainer container;
                CloudBlockBlob blob;

                account = CloudStorageAccount.parse(storageConnectionString);
                serviceClient = account.createCloudBlobClient();
                // Container name must be lower case.
                container = serviceClient.getContainerReference("gettingstarted");
                container.createIfNotExists();

                // Set anonymous access on the container.
                BlobContainerPermissions containerPermissions;
                containerPermissions = new BlobContainerPermissions();
                containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
                container.uploadPermissions(containerPermissions);

                // Upload an image file.
                blob = container.getBlockBlobReference("image1.jpg");

                File fileReference = new File("c:\\myimages\\image1.jpg");
                blob.upload(new FileInputStream(fileReference), fileReference.length());

                // At this point the image is uploaded.
                // Next, create an HTML page that lists all of the uploaded images.
                MakeHTMLPage(container);

                System.out.println("Processing complete.");
                System.out.println("Open index.html to see the images stored in your storage account.");

            } catch (FileNotFoundException fileNotFoundException) {
                System.out.print("FileNotFoundException encountered: ");
                System.out.println(fileNotFoundException.getMessage());
                System.exit(-1);
            } catch (StorageException storageException) {
                System.out.print("StorageException encountered: ");
                System.out.println(storageException.getMessage());
                System.exit(-1);
            } catch (URISyntaxException uriSyntaxException) {
                System.out.print("URISyntaxException encountered: ");
                System.out.println(uriSyntaxException.getMessage());
                System.exit(-1);
            } catch (Exception e) {
                System.out.print("Exception encountered: ");
                System.out.println(e.getMessage());
                System.exit(-1);
            }
        }

        // Create an HTML page that can be used to display the uploaded images.
        // This example assumes all of the blobs are for images.
        public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
        {
            PrintStream stream;
            stream = new PrintStream(new FileOutputStream("index.html"));

            // Create the opening <html>, <header>, and <body> elements.
            stream.println("<html>");
            stream.println("<header/>");
            stream.println("<body>");

            // Enumerate the uploaded blobs.
            for (ListBlobItem blobItem : container.listBlobs()) {
                // List each blob as an <img> element in the HTML body.
                stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
            }

            stream.println("</body>");

            // Complete the <html> element and close the file.
            stream.println("</html>");
            stream.close();
        }
    }

Neben lokale Bilddatei Azure-Speicher hochladen erstellt der Beispielcode eine lokale Datei-namedindex.html, können Sie in Ihrem Browser finden Ihr hochgeladene Bild öffnen.

Da der Code den Kontonamen und das kontoschlüssel enthält, sicherzustellen Sie, dass Quellcode sicher ist.

## <a name="to-delete-a-container"></a>Entfernen eines Containers

Da Storage berechnet soll den **Erste Schritte** -Container löschen Sie anschließend mit diesem Beispiel experimentieren. Um einen Container zu löschen, verwenden Sie die **CloudBlobContainer.delete** -Methode.

    container = serviceClient.getContainerReference("gettingstarted");
    container.delete();

Zum Aufrufen der **CloudBlobContainer.delete** -Methode ist beim Initialisieren der Objekte **CloudStorageAccount**, **ClodBlobClient**und **CloudBlobContainer** wie für die **Wertereihe** Methode angezeigt. Das folgende ist ein vollständiges Beispiel, das den Schlüsselcontainer **Einfu¨hrung**löscht.

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;

    public class DeleteContainer {

        public static final String storageConnectionString =
                "DefaultEndpointsProtocol=http;" +
                   "AccountName=your_account_name;" +
                   "AccountKey=your_account_key";

        public static void main(String[] args)
        {
            try
            {
                CloudStorageAccount account;
                CloudBlobClient serviceClient;
                CloudBlobContainer container;

                account = CloudStorageAccount.parse(storageConnectionString);
                serviceClient = account.createCloudBlobClient();
                // Container name must be lower case.
                container = serviceClient.getContainerReference("gettingstarted");
                container.delete();

                System.out.println("Container deleted.");

            }
            catch (StorageException storageException)
            {
                System.out.print("StorageException encountered: ");
                System.out.println(storageException.getMessage());
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.print("Exception encountered: ");
                System.out.println(e.getMessage());
                System.exit(-1);
            }
        }
    }

Eine Übersicht über andere BLOB-Speicherklassen und Methoden finden Sie unter [Verwenden von BLOB-Speicher von Java](storage-java-how-to-use-blob-storage.md).

## <a name="next-steps"></a>Nächste Schritte

Folgen Sie diesen Links, um weitere Informationen zu komplexeren Speicheraufgaben.

- [Azure-Speicher SDK für Java](https://github.com/azure/azure-storage-java)
- [Azure-Speicher Client SDK-Referenz](http://dl.windowsazure.com/storage/javadoc/)
- [Azure-Speicherdienste REST-API](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Azure-Speicher-Teamblog](http://blogs.msdn.com/b/windowsazurestorage/)
