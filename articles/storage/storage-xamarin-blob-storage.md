<properties
    pageTitle="Das BLOB-Speicherung von Xamarin | Microsoft Azure"
    description="Azure Storage-Clientbibliothek für Xamarin können Entwickler iOS, Android und Windows Store-apps mit ihren systemeigenen Benutzeroberflächen erstellen. Dieses Lernprogramm zeigt, wie mit Xamarin eine Anwendung erstellen, die Azure BLOB-Speicher verwendet."
    services="storage"
    documentationCenter="xamarin"
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/08/2016"
    ms.author="micurd"/>

# <a name="how-to-use-blob-storage-from-xamarin"></a>Verwendung von Xamarin-BLOB-Speicher

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

## <a name="overview"></a>Übersicht

Xamarin ermöglicht Entwicklern die Verwendung freigegebene C# Codebasis iOS, Android und Windows Store-apps mit ihren systemeigenen Benutzeroberflächen erstellen. Dieses Lernprogramm zeigt Verwendung von Azure BLOB-Speicher mit einer Xamarin Anwendung. Möchten Sie weitere Informationen zu Azure-Speicher vor dem Code, finden Sie unter [Einführung in Microsoft Azure-Speicher](storage-introduction.md).

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## <a name="create-a-new-xamarin-application"></a>Erstellen einer neuen Anwendung Xamarin

Diese erste Schritte, erstellen wir eine Anwendung, die Android, iOS und Windows abzielt. Diese app einfach einen Container erstellen und dieser Container einen Blob hochladen. Wir verwenden Visual Studio unter Windows für die ersten Schritte, aber dieselbe Erkenntnisse können beim Erstellen einer Anwendung mit Xamarin Studio unter Mac OS angewendet werden.

Führen Sie diese Schritte, um Ihre Anwendung zu erstellen:

1. Wenn Sie noch nicht herunterladen und Installieren von [Xamarin für Visual Studio](https://www.xamarin.com/download).
2. Öffnen Sie Visual Studio, und erstellen Sie eine leere App (Native freigegeben): **Datei > Neu > Projekt > plattformübergreifende > leere App(Native Shared)**.
3. **NuGet-Pakete verwalten, Lösung**auswählen und die Projektmappe im Projektmappen-Explorer klicken. Suchen Sie **WindowsAzure.Storage** und installieren Sie die neueste stabile Version für alle Projekte in der Projektmappe.
4. Erstellen Sie und führen Sie Ihr Projekt.

Sie haben nun eine Anwendung, mit der Sie auf eine Schaltfläche, die Zähler erhöht.

> [AZURE.NOTE] Azure Storage-Clientbibliothek für Xamarin unterstützt derzeit die folgenden Projekttypen: systemeigene freigegebene Xamarin.Forms freigegeben, Xamarin.Android und Xamarin.iOS.

## <a name="create-container-and-upload-blob"></a>Erstellen Sie Container und Hochladen Sie blob

Anschließend die freigegebene Klasse Code hinzugefügt werden können `MyClass.cs` , erstellt einen Container und lädt einen Blob in diesem Container. `MyClass.cs`sollte wie folgt aussehen:

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using System.Threading.Tasks;

    namespace XamarinApp
    {
        public class MyClass
        {
            public MyClass ()
            {
            }

            public static async Task createContainerAndUpload()
            {
                // Retrieve storage account from connection string.
                CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here");

                // Create the blob client.
                CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

                // Retrieve reference to a previously created container.
                CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

                // Create the container if it doesn't already exist.
                await container.CreateIfNotExistsAsync();

                // Retrieve reference to a blob named "myblob".
                CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

                // Create the "myblob" blob with the text "Hello, world!"
                await blockBlob.UploadTextAsync("Hello, world!");
            }
        }
    }

Achten Sie darauf, dass der tatsächliche Kontoname und kontoschlüssel "Your_account_name_here" und "Your_account_key_here" ersetzen. Anschließend können Sie diese freigegebenen Klasse iOS, Android und Windows Phone-Anwendung. Sie können einfach `MyClass.createContainerAndUpload()` für jedes Projekt. Zum Beispiel:

### <a name="xamarinappdroid--mainactivitycs"></a>XamarinApp.Droid > MainActivity.cs

    using Android.App;
    using Android.Widget;
    using Android.OS;

    namespace XamarinApp.Droid
    {
        [Activity (Label = "XamarinApp.Droid", MainLauncher = true, Icon = "@drawable/icon")]
        public class MainActivity : Activity
        {
            int count = 1;

            protected override async void OnCreate (Bundle bundle)
            {
                base.OnCreate (bundle);

                // Set our view from the "main" layout resource
                SetContentView (Resource.Layout.Main);

                // Get our button from the layout resource,
                // and attach an event to it
                Button button = FindViewById<Button> (Resource.Id.myButton);

                button.Click += delegate {
                    button.Text = string.Format ("{0} clicks!", count++);
                };

              await MyClass.createContainerAndUpload();
            }
        }
    }

### <a name="xamarinappios--viewcontrollercs"></a>XamarinApp.iOS > ViewController.cs

    using System;
    using UIKit;

    namespace XamarinApp.iOS
    {
        public partial class ViewController : UIViewController
        {
            int count = 1;

            public ViewController (IntPtr handle) : base (handle)
            {
            }

            public override async void ViewDidLoad ()
            {
                base.ViewDidLoad ();
                // Perform any additional setup after loading the view, typically from a nib.
                Button.AccessibilityIdentifier = "myButton";
                Button.TouchUpInside += delegate {
                    var title = string.Format ("{0} clicks!", count++);
                    Button.SetTitle (title, UIControlState.Normal);
                };

                await MyClass.createContainerAndUpload();
            }

            public override void DidReceiveMemoryWarning ()
            {
                base.DidReceiveMemoryWarning ();
                // Release any cached data, images, etc that aren't in use.
            }
        }
    }

### <a name="xamarinappwinphone--mainpagexaml--mainpagexamlcs"></a>XamarinApp.WinPhone > MainPage.xaml > MainPage.xaml.cs

    using Windows.UI.Xaml.Controls;
    using Windows.UI.Xaml.Navigation;

    // The Blank Page item template is documented at http://go.microsoft.com/fwlink/?LinkId=391641

    namespace XamarinApp.WinPhone
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            int count = 1;

            public MainPage()
            {
                this.InitializeComponent();

                this.NavigationCacheMode = NavigationCacheMode.Required;
            }

            /// <summary>
            /// Invoked when this page is about to be displayed in a Frame.
            /// </summary>
            /// <param name="e">Event data that describes how this page was reached.
            /// This parameter is typically used to configure the page.</param>
            protected override async void OnNavigatedTo(NavigationEventArgs e)
            {
                // TODO: Prepare page for display here.

                // TODO: If your application contains multiple pages, ensure that you are
                // handling the hardware Back button by registering for the
                // Windows.Phone.UI.Input.HardwareButtons.BackPressed event.
                // If you are using the NavigationHelper provided by some templates,
                // this event is handled for you.
                Button.Click += delegate {
                    var title = string.Format("{0} clicks!", count++);
                    Button.Content = title;
                };

                await MyClass.createContainerAndUpload();
            }
        }
    }


## <a name="run-the-application"></a>Führen Sie die Anwendung

Sie können jetzt diese Anwendung im Android oder Windows Phone-Emulator ausführen. Können diese Anwendung auch in iOS Emulator ausführen, aber dies erfordert einen Mac Spezifische Informationen lesen Sie die Dokumentation für [Visual Studio auf einem Mac verbinden](https://developer.xamarin.com/guides/ios/getting_started/installation/windows/connecting-to-mac/)

Nachdem Sie Ihre Anwendung ausführen, erstellen Sie den Container `mycontainer` in das Speicherkonto. Sollte das Blob `myblob`, wurde der Text in der `Hello, world!`. Mit [Microsoft Azure-Speicher-Explorer](http://storageexplorer.com/)können Sie dies überprüfen.

## <a name="next-steps"></a>Nächste Schritte

Diese erste Schritte, gelernt Sie eine plattformübergreifende Anwendung in Xamarin erstellen, Azure-Speicher verwendet. Diese Einführung speziell konzentriert sich ein Szenario im BLOB-Speicher. Allerdings möglich viel mehr mit nur BLOB-Speicher, sondern auch mit Tabelle, Datei und Queue Storage. Prüfen Sie die folgenden Artikel mehr:
- [Erste Schritte mit Azure BLOB-Speicher mit .NET](storage-dotnet-how-to-use-blobs.md)
- [Erste Schritte mit Azure Table Storage mit .NET](storage-dotnet-how-to-use-tables.md)
- [Erste Schritte mit Azure Queue Storage mit .NET](storage-dotnet-how-to-use-queues.md)
- [Erste Schritte mit Dateispeicher auf Windows Azure](storage-dotnet-how-to-use-files.md)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]
