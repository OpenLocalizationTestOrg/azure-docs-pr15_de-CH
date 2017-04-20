<properties
    pageTitle="Verbinden Sie mit Azure-Speicher in Ihrer Anwendung Xamarin.Forms"
    description="Die app Todo Liste Xamarin.Forms von Azure BLOB-Speicher mit fügen Sie Bilder hinzu"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="connect-to-azure-storage-in-your-xamarinforms-app"></a>Verbinden Sie mit Azure-Speicher in Ihrer Anwendung Xamarin.Forms

## <a name="overview"></a>Übersicht

Azure Mobile Apps Client und Server SDK unterstützt offline Synchronisieren von strukturierten Daten mit CRUD-Operationen für den Endpunkt Tables. Diese Daten werden im Allgemeinen in einer Datenbank oder ähnliche Speicher gespeichert und im Allgemeinen diese Datenspeicher können nicht speichern große binäre Daten effizient. Außerdem haben einige Programme beziehen, die Daten an anderer Stelle (z.B. BLOB-Speicher, Dateifreigaben) und es ist sinnvoll, zwischen Datensätzen in der Tables-Endpunkt und andere Daten können.

In diesem Thema wird veranschaulicht, wie Mobile Apps Todo Liste Schnellstart für Grafiken hinzufügen. Sie müssen zuerst das Lernprogramm [Xamarin.Forms app erstellen].

In diesem Lernprogramm erstellen ein Speicherkonto und Backend Mobile-Anwendung eine Verbindungszeichenfolge hinzugefügt. Anschließend fügen Sie eine neue erben den neuen Mobile Apps ein `StorageController<T>` Server-Projekt.

>[AZURE.TIP] Dieses Lernprogramm ist eine [Companion-Beispiel](https://azure.microsoft.com/documentation/samples/app-service-mobile-dotnet-todo-list-files/) verfügbar, die zu Ihrem Konto Azure bereitgestellt werden kann. 

## <a name="prerequisites"></a>Erforderliche Komponenten

* Arbeiten Sie das Lernprogramm [Xamarin.Forms app erstellen] , das anderen Komponenten aufgeführt sind. In diesem Artikel verwendet die abgeschlossene Anwendung aus diesem Lernprogramm.

>[AZURE.NOTE] Wenn Sie mit Azure App Service beginnen, bevor Sie für ein Azure-Konto, gehen Sie [versuchen App](https://tryappservice.azure.com/?appServiceName=mobile)Service. Dort sofort können eine kurzlebige Starter-app in App Service – keine Kreditkarte und keine Zusagen.

## <a name="create-a-storage-account"></a>Ein Speicherkonto erstellen

1. Erstellen Sie ein Speicherkonto Lernprogramm [ein Azure Storage-Konto erstellen]. 

2. Navigieren Sie in Azure-Portal zu der neu erstellten Speicherkonto, und klicken Sie auf **Schlüssel** . **Primäre Verbindungszeichenfolge**kopieren.

3. Navigieren Sie zu der mobilen Anwendung Backend. Unter **Alle** -> **Application Settings** -> **Verbindungszeichenfolgen**erstellen einen neuen Schlüssel mit dem Namen `MS_AzureStorageAccountConnectionString` und den Wert aus dem Speicherkonto kopiert. Der Schlüsseltyp verwenden Sie **benutzerdefinierte** .

## <a name="add-a-storage-controller-to-the-server"></a>Speicher-Controller zum Server hinzufügen

Sie müssen das Serverprojekt einen neuen Domänencontroller hinzufügen, die wird antwortet auf Anfragen für SAS-Token für den Azure-Speicher sowie eine Liste mit Dateien entsprechen einem Datensatz zurück:

- [Das Serverprojekt Speicher-Controller hinzufügen](#add-controller-code)
- [Routen registriert den Speicher-controller](#routes-registered)
- [Kommunikation von Client und server](#client-communication)

###<a name="add-controller-code"></a>Das Serverprojekt Speicher-Controller hinzufügen

1. Öffnen Sie in Visual Studio Ihrer Server. Fügen Sie das Nuget-Paket [Microsoft.Azure.Mobile.Server.Files]. Achten Sie darauf, dass **Vorabversion einfügen**wählen.

2. Öffnen Sie in Visual Studio Ihrer Server. Maustaste auf **den Ordner** und wählen Sie **Hinzufügen** -> **Controller** -> **Web API 2 Controller - leer**. Nennen Sie den Controller `TodoItemStorageController`.

3. Fügen Sie die folgende Anweisung verwenden:

        using Microsoft.Azure.Mobile.Server.Files;
        using Microsoft.Azure.Mobile.Server.Files.Controllers;

4. Ändern Sie die Basisklasse `StorageController`:
    
        public class TodoItemStorageController : StorageController<TodoItem>

5. Fügen Sie der Klasse die folgenden Methoden:

        [HttpPost]
        [Route("tables/TodoItem/{id}/StorageToken")]
        public async Task<HttpResponseMessage> PostStorageTokenRequest(string id, StorageTokenRequest value)
        {
            StorageToken token = await GetStorageTokenAsync(id, value);

            return Request.CreateResponse(token);
        }

        // Get the files associated with this record
        [HttpGet]
        [Route("tables/TodoItem/{id}/MobileServiceFiles")]
        public async Task<HttpResponseMessage> GetFiles(string id)
        {
            IEnumerable<MobileServiceFile> files = await GetRecordFilesAsync(id);

            return Request.CreateResponse(files);
        }

        [HttpDelete]
        [Route("tables/TodoItem/{id}/MobileServiceFiles/{name}")]
        public Task Delete(string id, string name)
        {
            return base.DeleteFileAsync(id, name);
        }

6. Aktualisieren der Konfiguration Web API-Attribut Arbeitsplan eingerichtet. Fügen Sie **Startup.MobileApp.cs**die folgende Zeile, die `ConfigureMobileApp()` Methode nach der Definition von der `config` Variable:

        config.MapHttpAttributeRoutes();

7. Die mobile Anwendung Back-End-veröffentlichen Sie Serverprojekt.

###<a name="routes-registered"></a>Routen registriert den Speicher-controller

Die neue `TodoItemStorageController` macht zwei untergeordnete Ressourcen unter Datensatz verwaltet:

- StorageToken

    + HTTP POST: Erstellt ein Token mit Speicher
    
        `/tables/TodoItem/{id}/MobileServiceFiles`
    
- MobileServiceFiles

    + HTTP GET: Ruft eine Liste der Dateien mit der
    
        `/tables/TodoItem/{id}/MobileServiceFiles`

    + HTTP-löschen: Löscht die Datei Ressourcenbezeichner angegebene Datei
    
        `/tables/TodoItem/{id}/MobileServiceFiles/{fileid}`

###<a name="client-communication"></a>Kommunikation von Client und server

Beachten Sie, dass `TodoItemStorageController` wird ** keine Route für das Hochladen oder Herunterladen eines BLOBs aufweisen. Deshalb ein mobiler Client BLOB-Speicher *direkt* interagiert, um diese Vorgänge auszuführen, nach dem Abrufen von SAS-Token (SAS einstellen) auf sichere Weise bestimmten Blob oder Container. Eine wichtige Architektur ist anderweitig Zugriff auf Speicher skalierbar und mobile Back-End-Verfügbarkeit eingeschränkt würde Stattdessen kann durch direktes Verbinden mit Azure-Speicher, der mobile Client Features wie automatische Partitionierung Geo-Verteilung nutzen.

Eine SAS bietet delegierten Zugriff auf Ressourcen in Ihrem Speicherkonto. Dies bedeutet, dass ein Client für einen bestimmten Zeitraum mit einer bestimmten Gruppe von Berechtigungen, Berechtigungen für Objekte in Ihrem Speicherkonto begrenzt, ohne Ihr Konto Zugriffstasten gemeinsam gewähren können. Mehr finden Sie unter [Grundlegendes zu freigegebenen Zugriff Signaturen].

Das Diagramm unten zeigt die Interaktionen Client und Server. Vor dem Hochladen einer Datei fordert der Client ein SAS-Token aus dem Dienst. Der Dienst verwendet die Verbindungszeichenfolge Speicher zu neuen SAS, der dann an den Client zurückgegeben. SAS ist zeitlich begrenzt und Berechtigungen auf eine bestimmte Datei oder Container beschränkt. Mobile Client verwendet dann diese SAS und Azure Storage Client SDK BLOB-Speicher die Datei hochzuladen.

![Ein SAS-Token anfordert](./media/app-service-mobile-xamarin-forms-blob-storage/storage-token-diagram.png)

## <a name="update-your-client-app-to-add-image-support"></a>Aktualisieren Sie die Clientanwendung die Unterstützung hinzufügen

Öffnen Sie das Xamarin.Forms Schnellstart-Projekt in Visual Studio oder Xamarin Studio. Installieren von Nuget-Pakete und portable Library-Projekt und iOS, Android und Windows update Clientprojekte:

- [Hinzufügen von Nuget-Paketen](#add-nuget)
- [Fügen Sie IPlatform-Schnittstelle](#add-iplatform)
- [FileHelper-Klasse hinzufügen](#add-filehelper)
- [Fügen Sie einen Handler Datei synchronisieren](#file-sync-handler)
- [TodoItemManager aktualisieren](#update-todoitemmanager)
- [Fügen Sie eine Detailansicht](#add-details-view)
- [Hauptfenster aktualisieren](#update-main-view)
- [Android Projekt aktualisieren](#update-android), [iOS-Projekt](#update-ios) [Windows-Projekt](#update-windows)

>[AZURE.NOTE] Dieses Lernprogramm enthält nur Informationen für Android, iOS und Windows Store-Plattformen nicht Windows Phone.

###<a name="add-nuget"></a>Hinzufügen von Nuget-Paketen

Die Lösung Maustaste und wählen Sie **Verwalten Nuget-Pakete Lösung**. Die folgenden Nuget-Pakete für **Alle** Projekte in der Projektmappe hinzufügen. Überprüfen Sie **Vorabversion enthalten**sein.

  - [Microsoft.Azure.Mobile.Client.Files]

  - [Microsoft.Azure.Mobile.Client.SQLiteStore]

  - [PCLStorage]

Zur Vereinfachung dieses Beispiel verwendet die [PCLStorage] -Bibliothek, jedoch muss nicht von Azure Mobile Apps Client SDK.

[PCLStorage]: https://www.nuget.org/packages/PCLStorage/

###<a name="add-iplatform"></a>Fügen Sie IPlatform-Schnittstelle

Erstellen Sie eine neue Schnittstelle `IPlatform` in main portable Library-Projekt. Entspricht dem Muster [Xamarin.Forms DependencyService] rechts plattformspezifische Klasse zur Laufzeit laden. Sie werden später plattformspezifische Implementierungen aller Client-Projekten hinzufügen.

1. Fügen Sie die folgende Anweisung verwenden:

        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Sync;

2. Ersetzen Sie die Implementierung durch Folgendes:

        public interface IPlatform
        {
            Task <string> GetTodoFilesPathAsync();

            Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata);

            Task<string> TakePhotoAsync(object context);

            Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename);
        }

###<a name="add-filehelper"></a>FileHelper-Klasse hinzufügen

1. Erstellen Sie eine neue Klasse `FileHelper` in main portable Library-Projekt. Fügen Sie die folgende Anweisung verwenden:

        using System.IO;
        using PCLStorage;
        using System.Threading.Tasks;
        using Xamarin.Forms;

2. Fügen Sie der Klassendefinition hinzu:

        public class FileHelper
        {
            public static async Task<string> CopyTodoItemFileAsync(string itemId, string filePath)
            {
                IFolder localStorage = FileSystem.Current.LocalStorage;

                string fileName = Path.GetFileName(filePath);
                string targetPath = await GetLocalFilePathAsync(itemId, fileName);

                var sourceFile = await localStorage.GetFileAsync(filePath);
                var sourceStream = await sourceFile.OpenAsync(FileAccess.Read);

                var targetFile = await localStorage.CreateFileAsync(targetPath, CreationCollisionOption.ReplaceExisting);

                using (var targetStream = await targetFile.OpenAsync(FileAccess.ReadAndWrite)) {
                    await sourceStream.CopyToAsync(targetStream);
                }

                return targetPath;
            }

            public static async Task<string> GetLocalFilePathAsync(string itemId, string fileName)
            {
                IPlatform platform = DependencyService.Get<IPlatform>();

                string recordFilesPath = Path.Combine(await platform.GetTodoFilesPathAsync(), itemId);

                    var checkExists = await FileSystem.Current.LocalStorage.CheckExistsAsync(recordFilesPath);
                    if (checkExists == ExistenceCheckResult.NotFound) {
                        await FileSystem.Current.LocalStorage.CreateFolderAsync(recordFilesPath, CreationCollisionOption.ReplaceExisting);
                    }

                return Path.Combine(recordFilesPath, fileName);
            }

            public static async Task DeleteLocalFileAsync(Microsoft.WindowsAzure.MobileServices.Files.MobileServiceFile fileName)
            {
                string localPath = await GetLocalFilePathAsync(fileName.ParentId, fileName.Name);
                var checkExists = await FileSystem.Current.LocalStorage.CheckExistsAsync(localPath);

                if (checkExists == ExistenceCheckResult.FileExists) {
                    var file = await FileSystem.Current.LocalStorage.GetFileAsync(localPath);
                    await file.DeleteAsync();
                }
            }
        }

###<a name="file-sync-handler"></a>Fügen Sie einen Handler Datei synchronisieren

Erstellen Sie eine neue Klasse `TodoItemFileSyncHandler` in main portable Library-Projekt. Diese Klasse enthält Rückrufe von Azure SDK Code benachrichtigt, wenn eine Datei hinzugefügt oder entfernt wird.

Azure Mobile Client SDK keine gespeichert Dateidaten: Client SDK Ruft die Implementierung von `IFileSyncHandler` die bestimmt, ob und wie Dateien auf dem lokalen Gerät gespeichert sind.

1. Fügen Sie die folgende Anweisung verwenden:

        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Xamarin.Forms;

2. Ersetzen Sie die Klassendefinition durch Folgendes: 

        public class TodoItemFileSyncHandler : IFileSyncHandler
        {
            private readonly TodoItemManager todoItemManager;

            public TodoItemFileSyncHandler(TodoItemManager itemManager)
            {
                this.todoItemManager = itemManager;
            }

            public Task<IMobileServiceFileDataSource> GetDataSource(MobileServiceFileMetadata metadata)
            {
                IPlatform platform = DependencyService.Get<IPlatform>();
                return platform.GetFileDataSource(metadata);
            }

            public async Task ProcessFileSynchronizationAction(MobileServiceFile file, FileSynchronizationAction action)
            {
                if (action == FileSynchronizationAction.Delete) {
                    await FileHelper.DeleteLocalFileAsync(file);
                }
                else { // Create or update. We're aggressively downloading all files.
                    await this.todoItemManager.DownloadFileAsync(file);
                }
            }
        }

###<a name="update-todoitemmanager"></a>TodoItemManager aktualisieren

1. Kommentieren Sie die Zeile in **TodoItemManager.cs** `#define OFFLINE_SYNC_ENABLED`.

2. **TodoItemManager.cs**, fügen Sie folgende Anweisung verwenden:

        using System.IO;
        using Xamarin.Forms;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Eventing;

3. Im Konstruktor der `TodoItemManager`, fügen Sie Folgendes nach dem Aufruf von `DefineTable()`:

        // Initialize file sync
        this.client.InitializeFileSyncContext(new TodoItemFileSyncHandler(this), store);

4. Ersetzen Sie im Konstruktor den Aufruf von `InitializeAsync` mit den folgenden. Dadurch gibt Rückrufe, wenn Datensätze im lokalen Informationsspeicher geändert werden. Die Sync-Funktion verwendet diese Rückrufe den Datei Sync-Handler ausgelöst.

        this.client.SyncContext.InitializeAsync(store, StoreTrackingOptions.NotifyLocalAndServerOperations);

5. In `SyncAsync()`, fügen Sie Folgendes nach dem Aufruf von `PushAsync()`:

        await this.todoTable.PushFileChangesAsync();

6. Fügen Sie die folgenden Methoden zur `TodoItemManager`:

        internal async Task DownloadFileAsync(MobileServiceFile file)
        {
            var todoItem = await todoTable.LookupAsync(file.ParentId);
            IPlatform platform = DependencyService.Get<IPlatform>();

            string filePath = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name); 
            await platform.DownloadFileAsync(this.todoTable, file, filePath);
        }

        internal async Task<MobileServiceFile> AddImage(TodoItem todoItem, string imagePath)
        {
            string targetPath = await FileHelper.CopyTodoItemFileAsync(todoItem.Id, imagePath);
            return await this.todoTable.AddFileAsync(todoItem, Path.GetFileName(targetPath));
        }

        internal async Task DeleteImage(TodoItem todoItem, MobileServiceFile file)
        {
            await this.todoTable.DeleteFileAsync(file);
        }

        internal async Task<IEnumerable<MobileServiceFile>> GetImageFilesAsync(TodoItem todoItem)
        {
            return await this.todoTable.GetFilesAsync(todoItem);
        }

###<a name="add-details-view"></a>Fügen Sie eine Detailansicht

In diesem Abschnitt fügen Sie eine neue Detailansicht für Todo-Element. Die Ansicht wird erstellt, wenn der Benutzer Todo-Element und neue Bilder ein Element hinzugefügt werden.

1. Fügen Sie eine neue Klasse **TodoItemImage** portable Library-Projekt die folgende Implementierung:

        public class TodoItemImage : INotifyPropertyChanged
        {
            private string name;
            private string uri;

            public MobileServiceFile File { get; private set; }

            public string Name
            {
                get { return name; }
                set
                {
                    name = value;
                    OnPropertyChanged(nameof(Name));
                }
            }

            public string Uri
            {
                get { return uri; }      
                set
                {
                    uri = value;
                    OnPropertyChanged(nameof(Uri));
                }
            }

            public TodoItemImage(MobileServiceFile file, TodoItem todoItem)
            {
                Name = file.Name;
                File = file;

                FileHelper.GetLocalFilePathAsync(todoItem.Id, file.Name).ContinueWith(x => this.Uri = x.Result);
            }

            public event PropertyChangedEventHandler PropertyChanged;

            private void OnPropertyChanged(string propertyName)
            {
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
            }
        }

2. Bearbeiten Sie **App.cs** Ersetzen Sie die Initialisierung von `MainPage` mit den folgenden:
    
        MainPage = new NavigationPage(new TodoList());

3. Fügen Sie die folgende Eigenschaft in **App.cs**:

        public static object UIContext { get; set; }

4. Maustaste portable Library-Projekt, und wählen Sie **Hinzufügen** -> **Neu** -> **plattformübergreifende** -> **Formulare XAML-Seite**. Name die Ansicht `TodoItemDetailsView`.

5. Öffnen Sie **TodoItemDetailsView.xaml** und Ersetzen Sie das Inhaltsverzeichnis Text durch den folgenden:

          <Grid>
            <Grid.RowDefinitions>
              <RowDefinition Height="Auto"/>
              <RowDefinition Height="Auto"/>
              <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <Button Clicked="OnAdd" Text="Add image"></Button>
            <ListView x:Name="imagesList"
                      ItemsSource="{Binding Images}"
                      IsPullToRefreshEnabled="false"
                      Grid.Row="2">
              <ListView.ItemTemplate>
                <DataTemplate>
                  <ImageCell ImageSource="{Binding Uri}"
                             Text="{Binding Name}">
                  </ImageCell>
                </DataTemplate>
              </ListView.ItemTemplate>
            </ListView>
          </Grid>

6. **TodoItemDetailsView.xaml.cs** bearbeiten und fügen Sie die folgende Anweisung verwenden:

        using System.Collections.ObjectModel;
        using Microsoft.WindowsAzure.MobileServices.Files;

7. Ersetzen Sie die Implementierung von `TodoItemDetailsView` mit den folgenden:

        public partial class TodoItemDetailsView : ContentPage
        {
            private TodoItemManager manager;

            public TodoItem TodoItem { get; set; }        
            public ObservableCollection<TodoItemImage> Images { get; set; }

            public TodoItemDetailsView(TodoItem todoItem, TodoItemManager manager)
            {
                InitializeComponent();
                this.Title = todoItem.Name;

                this.TodoItem = todoItem;
                this.manager = manager;

                this.Images = new ObservableCollection<TodoItemImage>();
                this.BindingContext = this;
            }

            public async Task LoadImagesAsync()
            {
                IEnumerable<MobileServiceFile> files = await this.manager.GetImageFilesAsync(TodoItem);
                this.Images.Clear();

                foreach (var f in files) {
                    var todoImage = new TodoItemImage(f, this.TodoItem);
                    this.Images.Add(todoImage);
                }
            }

            public async void OnAdd(object sender, EventArgs e)
            {
                IPlatform mediaProvider = DependencyService.Get<IPlatform>();
                string sourceImagePath = await mediaProvider.TakePhotoAsync(App.UIContext);

                if (sourceImagePath != null) {
                    MobileServiceFile file = await this.manager.AddImage(this.TodoItem, sourceImagePath);

                    var image = new TodoItemImage(file, this.TodoItem);
                    this.Images.Add(image);
                }
            }
        }

###<a name="update-main-view"></a>Hauptfenster aktualisieren 

Aktualisieren der Hauptansicht die Detailansicht öffnen, wenn Todo-Element ausgewählt ist.

Ersetzen Sie **TodoList.xaml.cs**, die Implementierung von `OnSelected` mit den folgenden:

    public async void OnSelected(object sender, SelectedItemChangedEventArgs e)
    {
        var todo = e.SelectedItem as TodoItem;

        if (todo != null) {
            var detailsView = new TodoItemDetailsView(todo, manager);
            await detailsView.LoadImagesAsync();
            await Navigation.PushAsync(detailsView);
        }

        todoList.SelectedItem = null;
    }

###<a name="update-android"></a>Android Projekt aktualisieren

Plattformspezifischen Code das Android Projekt fügen Sie hinzu, einschließlich eine Datei herunterladen und in ein neues Bild verwenden. 

Dieser Code verwendet die Xamarin.Forms [DependencyService](https://developer.xamarin.com/guides/xamarin-forms/dependency-service/) rechts plattformspezifische Klasse zur Laufzeit geladen.

1. Die Komponente **Xamarin.Mobile** Android Projekt hinzufügen.

2. Fügen Sie eine neue Klasse `DroidPlatform` mit folgenden Implementierung. Der wichtigste Namespace des Projekts "YourNamespace" ersetzen.

        using System;
        using System.IO;
        using System.Threading.Tasks;
        using Android.Content;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Xamarin.Media;

        [assembly: Xamarin.Forms.Dependency(typeof(YourNamespace.Droid.DroidPlatform))]
        namespace YourNamespace.Droid
        {
            public class DroidPlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        var uiContext = context as Context;
                        if (uiContext != null) {
                            var mediaPicker = new MediaPicker(uiContext);
                            var photo = await mediaPicker.TakePhotoAsync(new StoreCameraMediaOptions());

                            return photo.Path;
                        }
                    }
                    catch (TaskCanceledException) {
                    }

                    return null;
                }

                public Task<string> GetTodoFilesPathAsync()
                {
                    string appData = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
                    string filesPath = Path.Combine(appData, "TodoItemFiles");

                    if (!Directory.Exists(filesPath)) {
                        Directory.CreateDirectory(filesPath);
                    }

                    return Task.FromResult(filesPath);
                }
            }
        }

3. Bearbeiten Sie **MainActivity.cs** In `OnCreate`, fügen Sie den folgenden vor dem Aufruf von `LoadApplication()`:

        App.UIContext = this;

###<a name="update-ios"></a>IOS-Projekt aktualisieren

IOS-Projekt plattformspezifischen Code hinzufügen.

1. Die Komponente **Xamarin.Mobile** iOS-Projekt hinzufügen.

2. Fügen Sie eine neue Klasse `TouchPlatform` mit folgenden Implementierung. Der wichtigste Namespace des Projekts "YourNamespace" ersetzen.

        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Xamarin.Media;

        [assembly: Xamarin.Forms.Dependency(typeof(YourNamespace.iOS.TouchPlatform))]
        namespace YourNamespace.iOS
        {
            class TouchPlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        var mediaPicker = new MediaPicker();
                        var mediaFile = await mediaPicker.PickPhotoAsync();
                        return mediaFile.Path;
                    }
                    catch (TaskCanceledException) {
                        return null;
                    }
                }

                public Task<string> GetTodoFilesPathAsync()
                {
                    string filesPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments), "TodoItemFiles");

                    if (!Directory.Exists(filesPath)) {
                        Directory.CreateDirectory(filesPath);
                    }

                    return Task.FromResult(filesPath);
                }
            }
        }

3. **AppDelegate.cs** bearbeiten und kommentieren Sie den Aufruf von `SQLitePCL.CurrentPlatform.Init()`.

###<a name="update-windows"></a>Windows-Projekt aktualisieren

1. Installieren Sie Visual Studio-Erweiterung [SQLite für Windows 8.1](http://go.microsoft.com/fwlink/?LinkID=716919). Weitere Informationen finden Sie im Lernprogramm [für Ihre Windows offline Synchronisierung aktivieren](app-service-mobile-windows-store-dotnet-get-started-offline-data.md). 

2. Bearbeiten Sie **Package.appxmanifest** , und überprüfen Sie die **Webcam** -Funktionalität.

3. Fügen Sie eine neue Klasse `WindowsStorePlatform` mit folgenden Implementierung. Der wichtigste Namespace des Projekts "YourNamespace" ersetzen.

        using System;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Windows.Foundation;
        using Windows.Media.Capture;
        using Windows.Storage;
        using YourNamespace;

        [assembly: Xamarin.Forms.Dependency(typeof(WinApp.WindowsStorePlatform))]
        namespace WinApp
        {
            public class WindowsStorePlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> GetTodoFilesPathAsync()
                {
                    var storageFolder = ApplicationData.Current.LocalFolder;
                    var filePath = "TodoItemFiles";

                    var result = await storageFolder.TryGetItemAsync(filePath);

                    if (result == null) {
                        result = await storageFolder.CreateFolderAsync(filePath);
                    }

                    return result.Name; // later operations will use relative paths
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        CameraCaptureUI dialog = new CameraCaptureUI();
                        Size aspectRatio = new Size(16, 9);
                        dialog.PhotoSettings.CroppedAspectRatio = aspectRatio;

                        StorageFile file = await dialog.CaptureFileAsync(CameraCaptureUIMode.Photo);
                        return file.Path;
                    }
                    catch (TaskCanceledException) {
                        return null;
                    }
                }
            }
        }

##<a name="summary"></a>Zusammenfassung

In diesem Artikel beschrieben, wie mit der neuen dateiunterstützung in Azure Mobile Client und Server SDK Azure-Speicher arbeiten. 

- Erstellen ein Speicherkonto und die mobile Anwendung Back-End-Verbindungszeichenfolge hinzuzufügen. Nur Backend der Azure-Speicher ist: der mobile Client fordert ein SAS-Token (SAS) Wenn Azure-Speicher zugreifen muss. Über SAS-Token in Azure-Speicher finden Sie unter [Grundlegendes zu freigegebenen Zugriff Signaturen].

- Erstellen Sie einen Controller, Unterklassen `StorageController` um die SAS-token-Anfragen und die Dateien, die einem Datensatz zugeordnet sind. Standardmäßig sind Dateien die Datensatz-ID als Teil der Containername mit einem Datensatz zugeordnet. das Verhalten kann durch Angabe einer Implementierung angepasst werden `IContainerNameResolver`. Die token SAS-Richtlinie kann auch angepasst werden.

- Azure Mobile Client SDK speichert Daten gespeichert. Ruft eher den Client SDK die `IFileSyncHandler`, die dann entscheidet wie (und ob) Dateien auf dem lokalen Gerät. Die Synchronisierung ist wie folgt registriert:

        client.InitializeFileSync(new MyFileSyncHandler(), store);

      + `IFileSyncHandler.GetDataSource`wird aufgerufen, wenn Azure Mobile Client SDK die Dateidaten (z. B. als Teil des Upload-Prozesses). Dadurch werden die verwaltet wie (und ob) Dateien auf dem lokalen Gerät gespeichert sind und diese Informationen bei Bedarf.

      + `IFileSyncHandler.ProcessFileSynchronizationAction`als Teil der Datei Synchronisation aufgerufen. Ein und ein FileSynchronizationAction-Enumerationswert werden bereitgestellt, so Sie entscheiden können, wie die Anwendung verarbeitet das Ereignis (z. B. eine Datei automatisch herunterladen, wenn erstellt oder aktualisiert, eine Datei vom lokalen Gerät löschen, wenn die Datei auf dem Server gelöscht wird).

- Ein `MobileServiceFile` kann entweder online oder offline mit verwendet ein `IMobileServiceTable` oder `IMobileServiceSyncTable`bzw.. Im offline-Szenario der Upload tritt auf, wenn die app ruft `PushFileChangesAsync`. Dadurch Offlinevorgang Warteschlange verarbeitet werden. für jeden Dateivorgang Azure Mobile Client SDK Ruft die `GetDataSource` -Methode der `IFileSyncHandler` Instanz zum Abrufen des Inhalts der Datei für den Upload.

- Um ein Element Dateien abzurufen, rufen Sie die "GetFilesAsync` method on the  `IMobileServiceTable<T> ` or IMobileServiceSyncTable<T>` Instanz. Diese Methode gibt eine Liste der Dateien mit dem Datenelement. (Hinweis: Dies ist eine *lokale* Operation und die Dateien basierend auf dem Zustand des Objekts beim letzten Synchronisierung zurück. Um eine aktualisierte Liste der Dateien vom Server zu erhalten, sollten Sie eine Synchronisierungsoperation zuerst starten.)

        IEnumerable<MobileServiceFile> files = await myTable.GetFilesAsync(myItem);

- Die Sync-Funktion verwendet Datensatz Benachrichtigungen auf den lokalen Speicher um Datensätze abzurufen, die der Client als Teil eines Push oder Pull empfangen. Dazu aktivieren auf lokale und für die Synchronisierung mit den `StoreTrackingOptions` Parameter. 

        this.client.SyncContext.InitializeAsync(store, StoreTrackingOptions.NotifyLocalAndServerOperations);

      + Andere Speicher Überwachungsoptionen gibt, wie nur lokale oder nur Benachrichtigung. Hinzufügen oder eigene benutzerdefinierte Rückruf mit der `EventManager` -Eigenschaft des `IMobileServiceClient`:

            jobService.MobileService.EventManager.Subscribe<StoreOperationCompletedEvent>(StoreOperationEventHandler);

- Es ist möglich, Dateien hinzufügen oder Entfernen eines Datensatzes BLOB-Speicher direkt ändern, die Zuordnung über eine Namenskonvention erzielt wird. Sie sollten jedoch in diesem Fall immer **den Zeitstempel des Eintrags Änderung von zugehörigen Blobs zu aktualisieren**. Azure Mobile Client SDK aktualisiert immer einen Datensatz hinzufügen oder Entfernen einer Datei. 

    Der Grund für diese Anforderung ist, dass einige mobile Clients bereits den Datensatz im lokalen Speicher haben. Bei diesen Clients einen inkrementellen Abruf dieser Datensatz nicht zurückgegeben und Client fragt nicht nach neuen zugeordneten Dateien. Um dieses Problem zu vermeiden, sollten Aktualisierung Datensatz Zeitstempel bei jeder Änderung BLOB-Speicher, die keine Azure Mobile Client SDK verwendet.

- Das Clientprojekt mithilfe des [Xamarin.Forms DependencyService] rechts plattformspezifische Klasse zur Laufzeit geladen. In diesem Beispiel definiert eine Schnittstelle `IPlatform` mit in jeder Plattform-spezifische Projekte.

<!-- URLs. -->

[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
[Erstellen einer Xamarin.Forms]: app-service-mobile-xamarin-forms-get-started.md
[Xamarin.Forms DependencyService]: https://developer.xamarin.com/guides/xamarin-forms/dependency-service/
[Microsoft.Azure.Mobile.Client.Files]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.Files/
[Microsoft.Azure.Mobile.Client.SQLiteStore]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/
[Microsoft.Azure.Mobile.Server.Files]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Files/
[Gemeinsam genutzter Zugriff Signaturen]: ../storage/storage-dotnet-shared-access-signature-part-1.md
[Azure-Speicher registrieren]:  ../storage/storage-create-storage-account.md#create-a-storage-account
