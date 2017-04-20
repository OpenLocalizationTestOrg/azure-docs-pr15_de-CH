<properties
    pageTitle="Wie Android Mobile Apps-Clientbibliothek"
    description="Wie Android Client SDK für Azure Mobile Apps."
    services="app-service\mobile"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>


# <a name="how-to-use-the-android-client-library-for-mobile-apps"></a>Verwendung die Android-Clientbibliothek für Mobile Apps

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Dieses Handbuch beschreibt wie Sie Android Client SDK für Mobile Apps wie allgemeine Szenarien implementieren:

- Abfragen von Daten (einfügen, aktualisieren und löschen).
- Authentifizierung.
- Behandeln von Fehlern.
- Anpassen des Clients. 

Außerdem bietet eine tief greifende common Client-Code in die meisten mobilen apps verwendet.

Dieses Handbuch konzentriert sich auf clientseitige Android SDK.  Erfahren Sie mehr über die serverseitige SDKs für Mobile Apps finden Sie unter [Arbeiten mit .NET SDK Back-End] [ 10] oder [Verwendung Node.js Backend SDK][11].

## <a name="reference-documentation"></a>Dokumentation

[Javadoc-API-Referenz] finden[ 12] für Android-Clientbibliothek auf GitHub.

## <a name="supported-platforms"></a>Unterstützte Plattformen

Azure Mobile Apps Android SDK unterstützt API 19 bis 24 (KitKat durch Nougat).  

"Server-Flow" Authentifizierung verwendet eine Webansicht dargestellten Benutzeroberfläche. Ist das Gerät nicht WebView UI vorlegen, dann andere Methoden der Authentifizierung werden, die außerhalb des Bereichs des Produkts.  Dieses SDK ist nicht für-Art oder eingeschränkten Geräten.

## <a name="setup-and-prerequisites"></a>Setup und Komponenten

[Mobile Apps Schnellstart](app-service-mobile-android-get-started.md) -Lernprogramm abgeschlossen.  Dieser Aufgabe wird sichergestellt, dass alle erforderlichen Komponenten für die Entwicklung von Azure Mobile Apps erfüllt wurden.  Schnellstart können Sie Ihr Konto konfigurieren und erstellen Sie Ihre erste Mobile Anwendung Backend.

Wenn Sie nicht das Schnellstart-Lernprogramm abzuschließen, führen Sie die folgenden Aufgaben:

- [Erstellen Sie eine Mobile Anwendung Back-End-] [ 13] mit Ihrem Android.
- In Android Studio [Build Gradle aktualisieren](#gradle-build).
- [Aktivieren der Internet-Berechtigungssatz](#enable-internet).

###<a name="gradle-build"></a>Aktualisieren Sie die Datei Gradle erstellen

Ändern Sie beide **build.gradle** :

1. Fügen Sie diesen Code in *der Projektdatei auf **build.gradle** innerhalb des Tags *Mal* * :

        buildscript {
            repositories {
                jcenter()
            }
        }

2. Fügen Sie diesen Code *Modul app* auf **build.gradle** Datei innerhalb des Tags *abhängig* :

        compile 'com.microsoft.azure:azure-mobile-android:3.1.0'

    Derzeit ist die neueste Version 3.1.0. Die unterstützten Versionen finden Sie [hier][14].

###<a name="enable-internet"></a>Berechtigungssatz Internet aktivieren

Zugriff auf Azure müssen Ihre app Berechtigungssatz INTERNET aktiviert. Wenn es nicht bereits aktiviert ist, die Datei **AndroidManifest.xml** Hinzufügen der folgenden Codezeile:

    <uses-permission android:name="android.permission.INTERNET" />

## <a name="the-basics-deep-dive"></a>Grundlagen zu tief gehende

Dieser Abschnitt behandelt der Code in der Schnellstart-app mit Azure Mobile Apps bezieht.  

###<a name="data-object"></a>Client-Datenklassen definieren

Zugriff auf Daten aus SQL Azure-Tabellen definieren Sie Client Datenklassen entsprechen den Tabellen im Backend Mobile-Anwendung. Beispiele in diesem Thema angenommen, eine Tabelle namens **ToDoItem**, die die folgenden Spalten:

- ID
- Text
- Abschließen

Die entsprechenden Client-Objekt eingegeben:

    public class ToDoItem {
        private String id;
        private String text;
        private Boolean complete;
    }

Der Code befindet sich in einer Datei namens **ToDoItem.java**.

SQL Azure-Tabelle Spalten enthält, wird diese Klasse die entsprechenden Felder hinzugefügt werden.  Z. B. wenn das DTO (Data Transfer Object) eine Ganzzahlspalte Priorität hat und dieses Feld zusammen mit der Getter- und Setter-Methoden hinzufügen:

    private Integer priority;

    /**
    * Returns the item priority
    */
    public Integer getPriority() {
        return mPriority;
    }
    
    /**
    * Sets the item priority
    *
    * @param priority
    *            priority to set
    */
    public final void setPriority(Integer priority) {
        mPriority = priority;
    }

Zusätzliche Tabellen im Backend Mobile Apps erstellen, finden Sie [wie: definieren einen Tabelle Controller] [ 15] (.NET Backend) oder [definieren Sie Tabellen mit dynamischen Schema] [ 16] (Node.js Backend). Für Node.js-Backend können Sie auch die Einstellung **einfache Tabellen** im [Azure-Portal].

###<a name="create-client"></a>Gewusst wie: Clientkontext erstellen

Dieser Code erstellt das **MobileServiceClient** -Objekt, Backend Mobile-Anwendung zugreifen. Der Code geht in die `onCreate` -Methode der **Aktivitätsklasse in *AndroidManifest.xml* als **MAIN** Aktion **LAUNCHER** Kategorie angegeben** . Im Schnellstart-Code geht es in der Datei **ToDoActivity.java** .

        MobileServiceClient mClient = new MobileServiceClient(
            "MobileAppUrl", // Replace with the Site URL
            this)

In diesem Code ersetzen `MobileAppUrl` mit der URL der Mobile-Anwendung Backend s in [Azure-Portal] Blatt für die Mobile Anwendung Backend. Für diese Codezeile kompiliert müssen Sie auch die **import** -Anweisung hinzufügen:

    import com.microsoft.windowsazure.mobileservices.*;

###<a name="instantiating"></a>Gewusst wie: Erstellen einer

Die einfachste Möglichkeit zum Abfragen und Ändern von Daten im Backend ist in *typisierten Programmiermodell*, da Java eine stark typisierte Sprache. Dieses Modell bietet nahtlose JSON-Serialisierung und Deserialisierung mit [Gson] [ 3] Bibliothek beim Senden von Daten zwischen Clientobjekten und Tabellen in der Back-End-SQL Azure.

Zum Öffnen eine Tabelle erstellen Sie zunächst eine [MobileServiceTable] [ 8] Objekt durch Aufrufen der **getTable** Methode [MobileServiceClient][9].  Diese Methode hat zwei Überladungen:

    public class MobileServiceClient {
        public <E> MobileServiceTable<E> getTable(Class<E> clazz);
        public <E> MobileServiceTable<E> getTable(String name, Class<E> clazz);
    }

Im folgenden Code ist **mClient** ein Verweis auf das Objekt MobileServiceClient.  Die erste Überladung verwendet wird, den Klassennamen und den Tabellennamen stimmen und dient der Schnellstart:

    MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable(ToDoItem.class);

Die zweite Überladung wird verwendet, wenn der Tabellenname vom Klassennamen unterscheidet: der erste Parameter ist der Name der Tabelle.

    MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);

###<a name="binding"></a>Gewusst wie: Binden von Daten an die Benutzeroberfläche

Binden von Daten umfasst drei Komponenten:

- Datenquelle
- Das Bildschirmlayout
- Der Adapter, der die beiden miteinander verbindet.

In unserem Codebeispiel geben wir die Daten aus der Tabelle Mobil-Apps SQL Azure **ToDoItem** in einem Array zurück. Diese Aktivität ist ein allgemeines Muster für die Daten.  Datenbankabfragen zurück häufig eine Auflistung von Zeilen, die der Client in eine Liste bzw. ein Array abruft. In diesem Beispiel wird das Array der Datenquelle.

Der Code gibt ein Bildschirmlayout, die Ansicht der Daten auf dem Gerät definiert.  Die beiden verbunden, mit einen Adapter, der in diesem Code eine Erweiterung ist von der **ArrayAdapter&lt;ToDoItem&gt; ** Klasse.

#### <a name="layout"></a>Gewusst wie: Definieren des Layouts

Das Layout wird durch mehrere XML-Codeausschnitte definiert. Wenn ein vorhandenes Layout, entspricht der folgende Code die **ListView** wir mit unseren Server füllen möchten.

    <ListView
        android:id="@+id/listViewToDo"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        tools:listitem="@layout/row_list_to_do" >
    </ListView>

Im vorangehenden Code gibt das *Listitem* -Attribut Id des Layouts für eine einzelne Zeile in der Liste. Dieser Code gibt ein Kontrollkästchen und den dazugehörigen Text und ruft für jedes Element in der Liste einmal instanziiert. Dieses Layout das **Id** -Feld nicht angezeigt, und komplexeres Layout geben zusätzliche Felder angezeigt. Dieser Code ist in der Datei **row_list_to_do.xml** .

    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="horizontal">
        <CheckBox
            android:id="@+id/checkToDoItem"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/checkbox_text" />
    </LinearLayout>


#### <a name="adapter"></a>Gewusst wie: Definieren Sie den Adapter

Seit die Datenquelle unseres Array **ToDoItem**wir Unterklasse Adapter von einer **ArrayAdapter&lt;ToDoItem&gt; ** Klasse. Diese Unterklasse erstellt eine Ansicht für jede **ToDoItem** unter Verwendung des **Row_list_to_do** .

In unserem Code definieren wir die folgende Klasse eine Erweiterung der **ArrayAdapter&lt;E&gt; ** Klasse:

    public class ToDoItemAdapter extends ArrayAdapter<ToDoItem> {

Überschreiben Sie die Adapter **GetView** -Methode. Zum Beispiel:

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        View row = convertView;

        final ToDoItem currentItem = getItem(position);

        if (row == null) {
            LayoutInflater inflater = ((Activity) mContext).getLayoutInflater();
            row = inflater.inflate(R.layout.row_list_to_do, parent, false);
        }

        row.setTag(currentItem);

        final CheckBox checkBox = (CheckBox) row.findViewById(R.id.checkToDoItem);
        checkBox.setText(currentItem.getText());
        checkBox.setChecked(false);
        checkBox.setEnabled(true);

        checkBox.setOnClickListener(new View.OnClickListener() {

            @Override
            public void onClick(View arg0) {
                if (checkBox.isChecked()) {
                    checkBox.setEnabled(false);
                    if (mContext instanceof ToDoActivity) {
                        ToDoActivity activity = (ToDoActivity) mContext;
                        activity.checkItem(currentItem);
                    }
                }
            }
        });


        return row;
    }

Wir erstellen eine Instanz dieser Klasse in unsere Aktivitäten wie folgt:

    ToDoItemAdapter mAdapter;
    mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);

Der zweite Parameter für den ToDoItemAdapter-Konstruktor ist ein Verweis auf das Layout. Wir können jetzt **ListView** instanziieren und den Adapter der **ListView**.

    ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
    listViewToDo.setAdapter(mAdapter);

### <a name="api"></a>API-Struktur

Mobile Apps Tabellenvorgänge und benutzerdefinierte API-Aufrufe sind asynchron. Verwenden Sie die [Zukunft] und [AsyncTask] -Objekte für asynchronen Methoden mit Abfragen, einfügen, aktualisieren und löschen. Mit Futures erleichtert das mehrere Vorgänge in einem Hintergrundthread ausführen, ohne mehrere geschachtelte Rückrufe.

Überprüfen Sie die Datei **ToDoActivity.java** Schnelleinstieg Android Projekt aus dem [Azure-Portal] beispielsweise.

#### <a name="use-adapter"></a>Gewusst wie: verwenden den Adapter

Sie können jetzt Bindung verwenden. Der folgende Code Zeigt Elemente in der Tabelle und füllt den lokalen Adapter mit zurückgelieferten Artikel.

    public void showAll(View view) {
        AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final List<ToDoItem> results = mToDoTable.execute().get();
                    runOnUiThread(new Runnable() {

                        @Override
                        public void run() {
                            mAdapter.clear();
                            for (ToDoItem item : results) {
                                mAdapter.add(item);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        };
        runAsyncTask(task);
    }

Rufen Sie bei jedem der Tabelle **ToDoItem ändern auf** . Da Änderungen auf Basis von Datensätzen fertig sind, behandeln eine Zeile anstelle einer Auflistung. Beim Einfügen eines Elements rufen Sie die **add** -Methode für den Adapter. Beim Löschen, rufen Sie die Methode **Entfernen** .

##<a name="querying"></a>Gewusst wie: Abfragen von Daten aus Backend Mobile-Anwendung

Dieser Abschnitt beschreibt, wie Abfragen Backend Mobile-Anwendung umfasst die folgenden Aufgaben:

- [Gibt alle Elemente zurück]
- [Zurückgegebene Daten filtern]
- [Daten sortieren]
- [Daten in Seiten zurück]
- [Bestimmte Spalten auswählen]
- [Abfragemethoden verketten](#chaining)

### <a name="showAll"></a>Gewusst wie: Zurückgeben aller Elemente in einer Tabelle

Die folgende Abfrage gibt alle Elemente in der **ToDoItem** -Tabelle.

    List<ToDoItem> results = mToDoTable.execute().get();

Die Variable *Ergebnisse* gibt das Resultset der Abfrage als Liste.

### <a name="filtering"></a>Gewusst wie: Filter zurückgegebenen Daten

Die Ausführung der folgenden Abfrage gibt alle Elemente aus der Tabelle **ToDoItem** ist **vollständig** **falsch**.

    List<ToDoItem> result = mToDoTable.where()
                                .field("complete").eq(false)
                                .execute().get();

**mToDoTable** ist der Verweis auf zuvor erstellten mobilen Service-Tabelle.

Definieren Sie einen Filter mit **dem** Methodenaufruf auf Tabelle. Die **where** -Methode folgt Methode **Feld** , eine Methode, die das logische Prädikat angibt. Prädikat Methoden umfassen **Eq** (gleich), **Ne** (ungleich), **Gt** (größer als), " **ge** " (größer als oder gleich), **Lt** (kleiner als), **le** (kleiner oder gleich). Diese Methoden können Sie Number und String-Felder auf bestimmte Werte zu vergleichen.

Sie können Daten filtern. Die folgenden Methoden können Sie das gesamte Feld oder Teile des Datums vergleichen: **Jahr**, **Monat**, **Tag**, **Stunde**, **Minute**und **zweiten**. Das folgende Beispiel fügt einen Filter für Artikel, deren *Fälligkeitsdatum* 2013 entspricht.

    mToDoTable.where().year("due").eq(2013).execute().get();

Die folgenden Methoden unterstützen komplexe Filter auf Zeichenfolgenfelder: **StartsWith**, **EndsWith** **Concat**, **SubString**, **IndexOf**, **Ersetzen**, **ToLower**, **ToUpper**, **Zuschneiden**und **Länge**. Im folgende Beispiel filtert Tabellenzeilen beginnt *die Textspalte* mit "PRI0."

    mToDoTable.where().startsWith("text", "PRI0").execute().get();

-Operator folgt auf Felder unterstützt: **Hinzufügen**, **Sub**, **Mul**, **Div**, **mod**, **Boden**, **Decke**und **runden**. Im folgende Beispiel Filter für Zeilen, in denen die **Dauer** eine gerade Zahl ist.

    mToDoTable.where().field("duration").mod(2).eq(0).execute().get();

Prädikate mit diesen logischen Methoden kombinieren: **und**, **oder** und **nicht**. Das folgende Beispiel kombiniert zwei vorherigen Beispiele.

    mToDoTable.where().year("due").eq(2013).and().startsWith("text", "PRI0")
                .execute().get();

Gruppieren und Verschachteln von logischen Operatoren:

    mToDoTable.where()
                .year("due").eq(2013)
                    .and
                (startsWith("text", "PRI0").or().field("duration").gt(10))
                .execute().get();

Ausführlichere Informationen und Beispiele für Filter finden Sie [die Vielfalt der Android Client Abfrage Modell untersuchen](http://hashtagfail.com/post/46493261719/mobile-services-android-querying).

### <a name="sorting"></a>Gewusst wie: Sortieren Daten zurückgegeben

Der folgende Code gibt alle Elemente einer Tabelle **ToDoItems** *des Textfelds* aufsteigend sortiert. *mToDoTable* ist der Verweis auf die Back-End-Tabelle, die Sie zuvor erstellt haben:

    mToDoTable.orderBy("text", QueryOrder.Ascending).execute().get();

Der erste Parameter der Methode **OrderBy** ist eine Zeichenfolge gleich dem Namen des Feldes, sortieren. Der zweite Parameter verwendet die **QueryOrder** -Enumeration angeben, ob aufsteigend oder Absteigend sortieren.  Wenn Sie dabei ***,*** filtern, muss die ***where*** -Methode vor der ***OrderBy*** -Methode aufgerufen.

### <a name="paging"></a>Gewusst wie: Daten in Seiten zurück

Im ersten Beispiel wird veranschaulicht, wie die oberen fünf Elemente in einer Tabelle auswählen. Die Abfrage gibt die Elemente von einer **ToDoItems**. **mToDoTable** ist der Verweis auf die Back-End-Tabelle, die Sie zuvor erstellt haben:

    List<ToDoItem> result = mToDoTable.top(5).execute().get();


Hier ist eine Abfrage, die überspringt die ersten fünf Elemente und gibt die nächsten fünf:

    mToDoTable.skip(5).top(5).execute().get();

### <a name="selecting"></a>Gewusst wie: auswählen bestimmte Spalten

Der folgende Code veranschaulicht, wie alle Elemente von einer **ToDoItems**zurück, aber zeigt nur die Felder **abgeschlossen** und **Text** . **mToDoTable** ist der Verweis auf die Back-End-Tabelle, die wir zuvor erstellt.

    List<ToDoItemNarrow> result = mToDoTable.select("complete", "text").execute().get();

Die Parameter der Funktion sind die Zeichenfolgennamen von Spalten der Tabelle, die Sie zurückgeben möchten.

Die **select** -Methode muss Methoden wie **where** und **OrderBy**. Sie können Pagingmethoden wie **oben**stehen.

### <a name="chaining"></a>Gewusst wie: Verketten von Abfragemethoden

Die Back-End-Tabellen Abfragen verwendeten Methoden können verkettet werden. Verketten von Abfragemethoden können Sie bestimmte Spalten der gefilterten Zeilen auswählen, die sortiert und seitenweise durchlaufen werden. Sie können komplexe logische Filter erstellen.
Jede Abfragemethode gibt ein Abfrageobjekt. Ende der Reihe von Methoden und führen Sie die Abfrage tatsächlich rufen Sie die Methode **Ausführen** . Zum Beispiel:

    mToDoTable.where()
        .year("due").eq(2013)
        .and().startsWith("text", "PRI0")
        .or().field("duration").gt(10)
        .orderBy(duration, QueryOrder.Ascending)
        .select("id", "complete", "text", "duration")
        .top(20)
        .execute().get();

Die verketteten Abfragemethoden müssen wie folgt sortiert:

1. Filtermethoden (**,**).
2. Sortierung (**OrderBy**) Methoden.
3. (**Wählen Sie**) Auswahlmethoden.
4. Pagingmethoden (**Überspringen** und **oben**).

##<a name="inserting"></a>Gewusst wie: Einfügen von Daten in der Back-End

Eine Instanz der *ToDoItem* -Klasse instanziiert und seine Eigenschaften festlegen.

    ToDoItem item = new ToDoItem();
    item.text = "Test Program";
    item.complete = false;

Verwenden Sie dann **insert()** , ein Objekt einzufügen:

    ToDoItem entity = mToDoTable.insert(item).get();

Entspricht der zurückgegebenen Entität Daten in Back-End-Tabelle eingefügt enthalten die ID und alle anderen Werte im Back-End festgelegt.

Mobile Apps Tabellen benötigen eine Primärschlüsselspalte **Id**benannt. Standardmäßig ist diese Spalte eine Zeichenfolge. Der Standardwert der Spalte ID ist eine GUID.  Sie können andere eindeutige Werte, wie e-Mail-Adressen oder Benutzernamen. Wenn ein Zeichenfolgenwert ID eingefügten Datensatz nicht vorgesehen ist, generiert der Back-End-eine neue GUID.

ID-Zeichenfolgenwerten bieten folgende Vorteile:

+ IDs können ohne einen Roundtrip zur Datenbank generiert werden.
+ Datensätze sind leichter von anderen Tabellen oder Datenbanken zusammenführen.
+ ID-Werte integrieren besser mit der Anwendungslogik.

ID Zeichenfolgenwerte sind **erforderlich** für offline-Sync unterstützt.

##<a name="updating"></a>Gewusst wie: Aktualisieren von Daten in eine mobile Anwendung

Um Daten in einer Tabelle aktualisieren, übergeben Sie das neue Objekt **update()** -Methode.

    mToDoTable.update(item).get();

In diesem Beispiel ist *Element* einen Verweis auf eine Zeile in der Tabelle *ToDoItem* mit einigen Änderungen.
Die Zeile mit der gleichen **Id** ist aktualisiert.

##<a name="deleting"></a>Gewusst wie: Löschen von Daten in eine mobile Anwendung

Der folgende Code veranschaulicht, wie Daten aus einer Tabelle löschen, indem Sie das Objekt angeben.

    mToDoTable.delete(item);

Sie können auch ein Element durch Angabe der Feld **-Id** der zu löschenden Zeile löschen.

    String myRowId = "2FA404AB-E458-44CD-BC1B-3BC847EF0902";
    mToDoTable.delete(myRowId);

##<a name="lookup"></a>Gewusst wie: Suchen eines bestimmten Elements

Suchen eines Elements mit einem bestimmten **Id** -Feld mit der **lookUp()** -Methode:

    ToDoItem result = mToDoTable
                        .lookUp("0380BAFB-BCFF-443C-B7D5-30199F730335")
                        .get();

##<a name="untyped"></a>Gewusst wie: Verwenden von nicht typisierten Daten

Nicht typisierte Programmiermodell gibt Ihnen genaue Kontrolle über JSON-Serialisierung.  Es gibt einige Szenarien, in denen Sie nicht typisierten Programmiermodell verwenden möchten. Wenn beispielsweise Ihre Back-End-Tabelle viele Spalten enthält und Sie nur eine Teilmenge der Spalten verweisen müssen.  Typisierte Modell muss alle mobiler apps Tabellenspalten in die Klasse definieren.  

Die meisten Aufrufe API für den Datenzugriff ähneln typisierten Programmierung aufrufen. Der Hauptunterschied besteht im Modell nicht typisierten Methoden des Objekts **MobileServiceJsonTable** statt des **MobileServiceTable** -Objekts aufrufen.

### <a name="json_instance"></a>Gewusst wie: Erstellen Sie eine Instanz einer nicht typisierten Tabelle

Typisierte Modell ähnlich, Sie zuerst eine Referenz, aber in diesem Fall ist ein **MobileServicesJsonTable** -Objekt. Erhalten Sie den Verweis durch Aufrufen der **getTable** Methode für eine Instanz des Clients:

    private MobileServiceJsonTable mJsonToDoTable;
    //...
    mJsonToDoTable = mClient.getTable("ToDoItem");

Erstellen eine Instanz der **MobileServiceJsonTable**hat es praktisch die gleiche API als typisierte Programmiermodell zur Verfügung. In einigen Fällen übernehmen die Methoden nicht typisierten Parameter statt einen typisierten Parameter.

### <a name="json_insert"></a>Verwendung: eine nicht typisierte Tabelle einfügen

Im folgenden Codebeispiel wie Insert. Der erste Schritt ist die Erstellung [JsonObject][1], Teil [Gson] [ 3] Bibliothek.

    JsonObject jsonItem = new JsonObject();
    jsonItem.addProperty("text", "Wake up");
    jsonItem.addProperty("complete", false);

Verwenden Sie dann **insert()** nicht typisierte Objekt in die Tabelle einfügen.

    mJsonToDoTable.insert(jsonItem).get();

Möchten Sie die ID des eingefügten Objekts abzurufen, verwenden Sie die Methode **getAsJsonPrimitive()** .

    jsonItem.getAsJsonPrimitive("id").getAsInt());

### <a name="json_delete"></a>Gewusst wie: Löschen von eine nicht typisierte Tabelle

Im folgenden Codebeispiel wird eine Instanz des in diesem Fall die gleiche Instanz von **JsonObject** löschen, die im vorherigen *Einfügen* Beispiel erstellt wurde. Code ist identisch mit dem typisierten, aber die Methode hat eine andere Signatur da ein **JsonObject**verweist.

         mToDoTable.delete(jsonItem);

Sie können auch eine Instanz direkt mithilfe der ID löschen:

         mToDoTable.delete(ID);

### <a name="json_get"></a>Verwendung: eine nicht typisierte Tabelle alle Zeilen zurückgeben

Im folgenden Codebeispiel wird die gesamte Tabelle abrufen. Da Sie einer JSON-Tabelle mithilfe können Sie selektiv nur einige Spalten der Tabelle abrufen.

    public void showAllUntyped(View view) {
        new AsyncTask<Void, Void, Void>() {
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final JsonElement result = mJsonToDoTable.execute().get();
                    final JsonArray results = result.getAsJsonArray();
                    runOnUiThread(new Runnable() {

                        @Override
                        public void run() {
                            mAdapter.clear();
                            for (JsonElement item : results) {
                                String ID = item.getAsJsonObject().getAsJsonPrimitive("id").getAsString();
                                String mText = item.getAsJsonObject().getAsJsonPrimitive("text").getAsString();
                                Boolean mComplete = item.getAsJsonObject().getAsJsonPrimitive("complete").getAsBoolean();
                                ToDoItem mToDoItem = new ToDoItem();
                                mToDoItem.setId(ID);
                                mToDoItem.setText(mText);
                                mToDoItem.setComplete(mComplete);
                                mAdapter.add(mToDoItem);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        }.execute();
    }

Denselben Satz von Filtern filtern und paging für typisierte Modell verfügbaren Methoden sind für nicht typisierte Modell verfügbar.

##<a name="custom-api"></a>Gewusst wie: Aufrufen eine benutzerdefinierte API

Eine benutzerdefinierte API können Sie benutzerdefinierte Endpunkte, die Server Funktionen definieren, die nicht zuordnen einer einfügen, aktualisieren, löschen oder Lesevorgang. Mithilfe einer benutzerdefinierten API haben mehr Kontrolle über messaging, Sie auch lesen und HTTP-Nachrichtenheader und definiert ein Nachrichtenformat Körper als JSON.

Von einem Client Android rufen Sie die **InvokeApi** -Methode, um benutzerdefinierte API-Endpunkt aufrufen. Das folgende Beispiel zeigt wie anliegenden API namens **CompleteAll**, die **MarkAllResult**Auflistungsklasse zurückgibt.

    public void completeItem(View view) {

        ListenableFuture<MarkAllResult> result = mClient.invokeApi( "completeAll", MarkAllResult.class );

            Futures.addCallback(result, new FutureCallback<MarkAllResult>() {
                @Override
                public void onFailure(Throwable exc) {
                    createAndShowDialog((Exception) exc, "Error");
                }

                @Override
                public void onSuccess(MarkAllResult result) {
                    createAndShowDialog(result.getCount() + " item(s) marked as complete.", "Completed Items");
                    refreshItemsFromTable();
                }
            });
        }

**InvokeApi** -Methode wird auf dem Client sendet eine POST-Anforderung für die neue benutzerdefinierte API aufgerufen. Benutzerdefinierte API zurückgegebene Ergebnis ist wie Fehler in einem Meldungsdialogfeld angezeigt. Andere Versionen von **InvokeApi** können Sie optional ein Objekt im Hauptteil Anforderung senden, geben Sie die HTTP-Methode und Abfrageparameter mit der Anforderung gesendet. Nicht typisierte Versionen von **InvokeApi** werden ebenfalls bereitgestellt.

##<a name="authentication"></a>Gewusst wie: Authentifizierung für Ihre Anwendung hinzufügen

Lernprogramme beschrieben bereits ausführlich hinzugefügt.

App [app Benutzerauthentifizierung](app-service-mobile-android-get-started-users.md) mithilfe verschiedener externe Identitätsanbieter unterstützt: Facebook, Google, Microsoft Account, Twitter und Azure Active Directory. Sie können Berechtigungen für Tabellen für bestimmte Operationen nur authentifizierten Benutzern Zugriff festlegen. Die Identität des authentifizierten Benutzern können Sie um Autorisierungsregeln im Backend zu implementieren.

Zwei Authentifizierung Abläufe werden unterstützt: einem **Server** und einem **Client** . Fluss Server bietet die einfachste Authentifizierung als Identität Anbieter-Weboberfläche benötigt.  Keine zusätzliche SDKs Fluss Authentifizierung implementieren müssen. Fluss Authentifizierung bietet eine Tiefe Integration in das mobile Gerät und nur für Nachweis Konzept Szenarien empfohlen.

Der Client ermöglicht tiefere Integration gerätespezifische Funktionen wie einmaliges Anmelden als SDKs vom Identitätsanbieter bereitgestellte beruht.  Beispielsweise können Sie die Facebook-SDK in der mobilen Anwendung integrieren.  Mobile Client in Facebook-Anwendung wechselt und bestätigt die Anmeldung vor der mobilen Anwendung austauschen.

Aktivieren der Authentifizierung in Ihrer app sind vier Schritte erforderlich:

- Registrieren Sie Ihre app für die Authentifizierung mit Identitätsanbieter.
- Konfigurieren Sie Ihre App Service-Backend.
- Tabellenberechtigungen für authentifizierte Benutzer nur auf Back-End App Service-einschränken.
- Ihre app Authentifizierungscode hinzufügen.

Sie können Berechtigungen für Tabellen für bestimmte Operationen nur authentifizierten Benutzern Zugriff festlegen. Die SID eines authentifizierten Benutzers können Sie Anfragen bearbeiten.  Weitere Informationen finden Sie [mit Authentifizierung] und Server SDK HOWTO-Dokumentation.

### <a name="caching"></a>Gewusst wie: Ihre app Authentifizierungscode hinzufügen

Der folgende Code startet einen Server Login Prozessablauf mit Google-Anbieter:

    MobileServiceUser user = mClient.login(MobileServiceAuthenticationProvider.Google);

Erhalten Sie der angemeldete Benutzer-ID aus einer **MobileServiceUser** Methode **GetUserId** . Ein Beispiel zum Verwenden der Benutzername asynchronen APIs aufrufen finden Sie unter [Erste Schritte mit Authentifizierung].

### <a name="caching"></a>Gewusst wie: Zwischenspeichern von Authentifizierungstoken

Authentifizierungstoken Zwischenspeichern muss der Benutzername und das Authentifizierungstoken lokal auf dem Gerät speichern. Beim nächsten Start die Anwendung Überprüfen des Zwischenspeichers, und wenn diese vorhanden sind, können Sie anmelden Prozedur überspringen und mit diesen Daten aktivieren. Jedoch diese Daten sind vertraulich und bei Telefon gestohlen Sicherheit verschlüsselt gespeichert werden soll.

Ein vollständiges Beispiel zum Cache Authentifizierungstoken im [Cache Authentication Token Abschnitt]Siehe[7].

Wenn Sie versuchen, einen abgelaufenen Token verwenden, erhalten Sie eine Antwort *401 nicht autorisiert* . Sie können Filter Authentifizierungsfehler behandeln.  Filter abfangen Back-End App Service-Anfragen. Filtercode überprüft die Antwort auf eine 401, löst den Vorgang und setzt die Anforderung, die die 401 generiert. 

## <a name="adal"></a>Gewusst wie: Benutzerauthentifizierung mit der Active Directory-Authentifizierungsbibliothek

Active Directory Authentifizierung Library (ADAL) können Sie die Anwendung mithilfe von Azure Active Directory Benutzer anmelden. Client Fluss Anmeldung ist häufig vorzuziehen der `loginAsync()` wie eine weitere systemeigene UX Verhalten und zusätzliche Anpassung ermöglicht.

1. Konfigurieren Sie mobile app Backend AAD Anmeldung mithilfe Lernprogramm [App Service für Active Directory-Konto konfigurieren](app-service-mobile-how-to-configure-active-directory-authentication.md) . Stellen Sie sicher, das optionale Schritt eine native Client-Anwendung registrieren.

2. Installieren ADAL ändern die Datei build.gradle, um die folgenden Definitionen:

```
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
packagingOptions {
    exclude 'META-INF/MSFTSIG.RSA'
    exclude 'META-INF/MSFTSIG.SF'
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
    compile 'com.android.support:support-v4:23.0.0'
}
```

3. Fügen Sie folgenden Code auf die Anwendung, die folgende Ersatzteile:

* Ersetzen Sie **Einfügen Behörde hier** durch den Namen des Mandanten, in dem die Anwendung bereitgestellt. Das Format muss https://login.windows.net/contoso.onmicrosoft.com sein. Dieser Wert kann aus der Registerkarte Domänen Azure Active Directory in [klassischen Azure-Portal] kopiert.

* Die Client-ID für die mobile Anwendung Backend-ersetzen Sie **Einfügen-Ressource-ID-hier** . Die Client-ID erhalten Sie auf der Registerkarte **Erweitert** unter **Azure Active Directory Einstellungen** im Portal.

* Ersetzen Sie **INSERT-CLIENT-ID-hier** durch die Client-ID von der systemeigenen Clientanwendung kopiert.

* Ersetzen Sie **INSERT-REDIRECT-URI-hier** mit Ihrer Site _/.auth/login/done_ Endpunkt mit HTTPS-Schema. Dieser Wert sollte ähnlich wie _https://contoso.azurewebsites.net/.auth/login/done_.

        private AuthenticationContext mContext;

        private void authenticate() {
            String authority = "INSERT-AUTHORITY-HERE";
            String resourceId = "INSERT-RESOURCE-ID-HERE";
            String clientId = "INSERT-CLIENT-ID-HERE";
            String redirectUri = "INSERT-REDIRECT-URI-HERE";
            try {
                mContext = new AuthenticationContext(this, authority, true);
                mContext.acquireToken(this, resourceId, clientId, redirectUri, PromptBehavior.Auto, "", callback);
            } catch (Exception exc) {
                exc.printStackTrace();
            }
        }

        private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {
            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    Log.d(TAG, "Cancelled");
                } else {
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    Log.d(TAG, "Token is empty");
                } else {
                    try {
                        JSONObject payload = new JSONObject();
                        payload.put("access_token", result.getAccessToken());
                        ListenableFuture<MobileServiceUser> mLogin = mClient.login("aad", payload.toString());
                        Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                            @Override
                            public void onFailure(Throwable exc) {
                                exc.printStackTrace();
                            }
                            @Override
                            public void onSuccess(MobileServiceUser user) {
                                Log.d(TAG, "Login Complete");
                            }
                        });
                    }
                    catch (Exception exc){
                        Log.d(TAG, "Authentication error:" + exc.getMessage());
                    }
                }
            }
        };

        @Override
        protected void onActivityResult(int requestCode, int resultCode, Intent data) {
            super.onActivityResult(requestCode, resultCode, data);
            if (mContext != null) {
                mContext.onActivityResult(requestCode, resultCode, data);
            }
        }

## <a name="how-to-add-push-notification-to-your-app"></a>Gewusst wie: Push-Benachrichtigung zu Ihrer Anwendung hinzufügen

Sie können [eine Übersicht] [ 6] beschreibt, wie Microsoft Azure Notification Hubs eine Vielzahl von Pushbenachrichtigungen unterstützt.  In [diesem Lernprogramm][5], eine Push-Benachrichtigung wird an alle Geräte gesendet, jedes Mal, wenn ein Datensatz eingefügt wird.

## <a name="how-to-add-offline-sync-to-your-app"></a>Gewusst wie: offline synchronisieren Ihrer Anwendung hinzufügen

Das Schnellstart-Lernprogramm enthält Code, offline synchronisieren implementiert. Suchen Sie nach Code Kommentare vorangestellt:

    // Offline Sync

Indem Sie die folgenden Codezeilen auskommentieren können offline synchronisieren implementieren und andere Mobile Apps Code ähnlichen Code hinzufügen.

##<a name="customizing"></a>Gewusst wie: Anpassen den Client

Es gibt verschiedene Arten, das Standardverhalten des Clients anzupassen.

### <a name="headers"></a>Gewusst wie: Anpassen Anfrage-Header

Konfigurieren Sie einen **ServiceFilter** für jede Anforderung einen benutzerdefinierten Header hinzufügen:

    private class CustomHeaderFilter implements ServiceFilter {

        @Override
        public ListenableFuture<ServiceFilterResponse> handleRequest(
                    ServiceFilterRequest request,
                    NextServiceFilterCallback next) {

            runOnUiThread(new Runnable() {

                @Override
                public void run() {
                    request.addHeader("My-Header", "Value");                    }
            });

            SettableFuture<ServiceFilterResponse> result = SettableFuture.create();
            try {
                ServiceFilterResponse response = next.onNext(request).get();
                result.set(response);
            } catch (Exception exc) {
                result.setException(exc);
            }
        }

### <a name="serialization"></a>Gewusst wie: Anpassen der Serialisierung

Der Kunde übernimmt, Tabellennamen, Spaltennamen und Daten im Back-End alle entspricht genau der Datenobjekte im Client definiert eingibt. Es kann verschiedene Gründe, warum die Namen von Server und Client möglicherweise nicht übereinstimmt. In Ihrem Szenario sollten Sie die folgenden Arten von Anpassungen:

- Spaltennamen in der Tabelle App verwendet übereinstimmen nicht den Namen, auf dem Client verwenden.
- Verwenden Sie eine App Service-Tabelle mit einem anderen Namen als die Klasse, die auf dem Client zugeordnet.
- Aktivieren Sie automatische Eigenschaft Groß-/Kleinschreibung.
- Komplexe Eigenschaften zu einem Objekt hinzufügen.

### <a name="columns"></a>Gewusst wie: Zuordnen von Client und Server unterschiedliche

Angenommen Sie, Ihr Java-Clientcode Standardnamen für Objekteigenschaften **ToDoItem** wie die folgenden Java-Format verwendet:

- Mitte
- mText
- mComplete
- mDuration

Serialisiert die Clientnamen in JSON-Namen, die Spaltennamen der Tabelle **ToDoItem** auf dem Server übereinstimmen. Der folgende Code verwendet die [Gson] [ 3] Bibliothek Eigenschaften versehen:

    @com.google.gson.annotations.SerializedName("text")
    private String mText;

    @com.google.gson.annotations.SerializedName("id")
    private int mId;

    @com.google.gson.annotations.SerializedName("complete")
    private boolean mComplete;

    @com.google.gson.annotations.SerializedName("duration")
    private String mDuration;

### <a name="table"></a>Gewusst wie: verschiedene Namen zwischen dem Client und dem Back-End

Ein Tabellenname andere mobile Dienste mit einer Überschreibung der [getTable()] Client Tabellennamen zuordnen[ 4] Methode:

    mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);

### <a name="conversions"></a>Gewusst wie: Automatisieren Spalte Zuordnung

Geben Sie eine Konvertierungsstrategie, die für jede Spalte mit [Gson] gilt[ 3] API. Android-Clientbibliothek verwendet [Gson] [ 3] Hintergrund Java Objekte JSON-Daten serialisieren, bevor die Daten an Azure App Service gesendet werden.  Der folgende Code verwendet die **setFieldNamingStrategy()** -Methode die Strategie festlegen. In diesem Beispiel wird das erste Zeichen (ein "m"), löschen und Kleinbuchstaben das nächste Zeichen für jedes Feld. Beispielsweise würde "Mitte" in "Id" aktivieren

    client.setGsonBuilder(
        MobileServiceClient
        .createMobileServiceGsonBuilder()
        .setFieldNamingStrategy(new FieldNamingStrategy() {
            public String translateName(Field field) {
                String name = field.getName();
                return Character.toLowerCase(name.charAt(1))
                    + name.substring(2);
                }
            });

Dieser Code muss vor dem **MobileServiceClient**ausgeführt werden.

### <a name="complex"></a>Gewusst wie: Speichern Sie eine Objekt oder Array-Eigenschaft in einer Tabelle

Bisher haben Serialisierung Beispiele primitive Typen wie Ganzzahlen und Zeichenfolgen beteiligt.  Primitive Typen serialisieren problemlos in JSON.  Wenn wir JSON ein komplexes Objekt automatisch serialisiert nicht hinzufügen möchten, müssen wir JSON Serialisierungsmethode bereitzustellen.  Ein Beispiel zum Bereitstellen von benutzerdefinierten JSON-Serialisierung von überprüfen Blogbeitrag [Anpassen Serialisierung unter Verwendung der Gson-Bibliothek im Mobile Dienste Android-Client][2].

<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[How to: Create the Mobile Services client]: #create-client
[How to: Create a table reference]: #instantiating
[The API structure]: #api
[How to: Query data from a mobile service]: #querying
[Gibt alle Elemente zurück]: #showAll
[Zurückgegebene Daten filtern]: #filtering
[Daten sortieren]: #sorting
[Daten in Seiten zurück]: #paging
[Bestimmte Spalten auswählen]: #selecting
[How to: Concatenate query methods]: #chaining
[How to: Bind data to the user interface]: #binding
[How to: Define the layout]: #layout
[How to: Define the adapter]: #adapter
[How to: Use the adapter]: #use-adapter
[How to: Insert data into a mobile service]: #inserting
[How to: update data in a mobile service]: #updating
[How to: Delete data in a mobile service]: #deleting
[How to: Look up a specific item]: #lookup
[How to: Work with untyped data]: #untyped
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching
[How to: Handle errors]: #errors
[How to: Design unit tests]: #tests
[How to: Customize the client]: #customizing
[Customize request headers]: #headers
[Customize serialization]: #serialization
[Next Steps]: #next-steps
[Setup and Prerequisites]: #setup

<!-- Images. -->

<!-- URLs. -->
[Get started with Azure Mobile Apps]: app-service-mobile-android-get-started.md
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[Mobile Services SDK for Android]: http://go.microsoft.com/fwlink/p/?LinkID=717033
[Azure-portal]: https://portal.azure.com
[Erste Schritte mit Authentifizierung]: app-service-mobile-android-get-started-users.md
[1]: http://google-gson.googlecode.com/svn/trunk/gson/docs/javadocs/com/google/gson/JsonObject.html
[2]: http://hashtagfail.com/post/44606137082/mobile-services-android-serialization-gson
[3]: http://go.microsoft.com/fwlink/p/?LinkId=290801
[4]: http://go.microsoft.com/fwlink/p/?LinkId=296840
[5]: app-service-mobile-android-get-started-push.md
[6]: ../notification-hubs/notification-hubs-push-notification-overview.md#integration-with-app-service-mobile-apps
[7]: app-service-mobile-android-get-started-users.md#cache-tokens
[8]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/table/MobileServiceTable.html
[9]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/MobileServiceClient.html
[10]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[11]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[12]: http://azure.github.io/azure-mobile-apps-android-client/
[13]: app-service-mobile-android-get-started.md#create-a-new-azure-mobile-app-backend
[14]: http://go.microsoft.com/fwlink/p/?LinkID=717034
[15]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#how-to-define-a-table-controller
[16]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[Zukunft]: http://developer.android.com/reference/java/util/concurrent/Future.html
[AsyncTask]: http://developer.android.com/reference/android/os/AsyncTask.html