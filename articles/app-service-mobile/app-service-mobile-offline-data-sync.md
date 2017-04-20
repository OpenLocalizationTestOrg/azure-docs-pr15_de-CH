<properties
    pageTitle="Offline-Daten synchronisieren in Azure mobiler Apps | Microsoft Azure"
    description="Übersicht über Offlinedaten Synchronisierungsfunktion für Azure Mobile Apps und grundlegende Referenz"
    documentationCenter="windows"
    authors="adrianhall"
    manager="dwrede"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="offline-data-sync-in-azure-mobile-apps"></a>Offline-Daten synchronisieren in Azure mobiler Apps

## <a name="what-is-offline-data-sync"></a>Was ist offline-Daten synchronisieren?

Offline-Daten synchronisieren ist ein Client- und SDK-Feature von Azure Mobile Apps, die Entwickler apps erstellen, die funktionale Verbindung erleichtert.

Wenn Ihre Anwendung im Offlinemodus befindet, können noch Benutzer erstellen und Ändern von Daten in einem lokalen Speicher gespeichert werden. Wenn die Anwendung wieder online ist, können sie lokale mit Backend Azure Mobile-Anwendung synchronisieren. Die Funktion unterstützt auch zum Erkennen von Konflikten, wenn auf dem Client und dem Back-End denselben Datensatz geändert wird. Konflikte können dann entweder auf dem Server oder dem Client verarbeitet werden.

Offline synchronisieren hat eine Reihe von Vorteilen:

* Verbessern Sie app-Reaktionsfähigkeit, indem Sie Serverdaten lokal zwischenspeichern
* Erstellen Sie stabile apps nützlich sein bei Netzwerkproblemen
* Möglichkeit für Anwender zum Erstellen und Ändern von Daten, selbst wenn kein Netzwerkzugriff Szenarien mit wenig oder ganz ohne Unterstützung
* Synchronisieren von Daten über mehrere Devices hinweg und Konflikte erkennen, wenn zwei Geräte denselben Datensatz geändert wird
* Beschränken Sie Netzwerk mit hoher Latenz oder gemessenen

Die folgenden Lernprogramme anzeigen wie Ihre mobile Clients mithilfe von Azure Mobile Apps offline synchronisieren hinzugefügt:

* [Android: Offline-Synchronisierung aktivieren]
* [iOS: offline-Synchronisierung aktivieren]
* [Xamarin iOS: offline-Synchronisierung aktivieren]
* [Xamarin Android: Offline-Synchronisierung aktivieren]
* [Xamarin.Forms: Enable offline synchronisieren](app-service-mobile-xamarin-forms-get-started-offline-data.md)
* [Universelle Windows Plattform: Offline-Synchronisierung aktivieren]

## <a name="what-is-a-sync-table"></a>Was ist eine Synchronisation?

Zugriff auf den Endpunkt "/ Tabellen" bieten Azure Mobile Client SDKs Schnittstellen wie `IMobileServiceTable` (.NET Client SDK) oder `MSTable` (iOS-Client). Diese APIs direkt an Azure Mobile App Backend und schlägt fehl, wenn das Clientgerät keine Netzwerkverbindung haben.

Unterstützung für offline-Verwendung Ihrer app sollte verwenden *Synchronisierungstabelle* APIs wie `IMobileServiceSyncTable` (.NET Client SDK) oder `MSSyncTable` (iOS-Client). Alle arbeiten CRUD-Vorgänge (Create, Read, Update, Delete) gegen Synchronisierungstabelle APIs außer jetzt sie lesen oder zu einem *lokalen Speicher schreiben*. Vor dem Synchronisieren Tabellenvorgänge ausgeführt werden können, muss der lokale Speicher initialisiert.

## <a name="what-is-a-local-store"></a>Was ist ein lokaler Speicher?

Lokaler Speicher ist der Persistenzebene Daten auf dem Client. Azure Mobile Apps Client SDKs bieten eine standardmäßige lokale Implementierung. Unter Windows, Xamarin und Android SQLite basiert auf; In iOS basiert sie Stammdaten.

Um die SQLite-Implementierung für Windows Phone oder Windows Store 8.1 verwenden, müssen Sie eine SQLite Erweiterung installieren. Weitere Informationen finden Sie unter [Universal Windows Plattform: offline Synchronisierung aktivieren]. Android Lieferumfang iOS und Version SQLite im Betriebssystem Geräts, so dass es nicht notwendig, eine eigene Version von SQLite verweisen.

Entwickler können auch ihre eigenen lokalen Speicher implementieren. Möchten Sie Daten in einem verschlüsselten Format auf dem mobilen Client speichern, können Sie beispielsweise einen lokalen Speicher definieren, der sqlcipher für die Verschlüsselung verwendet.

## <a name="what-is-a-sync-context"></a>Was ist ein Synchronisierungskontext?

Ein *Synchronisierungskontext* mobilen Client-Objekt zugeordnet ist (z. B. `IMobileServiceClient` oder `MSClient`) und Sync Tabellen vorgenommenen Änderungen. Der Synchronisierungskontext unterhält eine *Vorgangswarteschlange* hält einer geordneten Liste von CRUD-Vorgänge (Create, Update, Delete), die später an den Server gesendet werden.

Lokaler Speicher zugeordnet ist wie eine Initialisierungsmethode mit Synchronisierungskontext `IMobileServicesSyncContext.InitializeAsync(localstore)` im [.NET Client SDK].

## <a name="how-sync-works"></a>Offlinedateien-Synchronisierung

Beim Synchronisierungstabellen steuert Clientcode als lokale Änderung mit einer Azure Mobile App-Backend synchronisiert werden. Nichts wird auf dem Back-End gesendet, bis ein zum *lokalen Änderungen Aufruf* . Ebenso ist der lokale Speicher mit neuen Daten nur aufgefüllt, wenn ein von *Daten Aufruf* .

* **Push**: Push ist der Synchronisierungskontext und sendet alle CUD geändert seit der letzten Push. Beachten Sie, dass es nicht möglich, nur eine einzelne Tabelle ändert, da andernfalls Vorgänge nicht gesendet werden konnte. Push führt eine Reihe von weiteren Aufrufe Backend Azure Mobile-Anwendung, die wiederum die Serverdatenbank ändern.

* **Pull**: Ziehen erfolgt jeweils pro Tabelle und einer Abfrage nur eine Teilmenge der Serverdaten abrufen angepasst werden. Azure Mobile Client-SDKs und Einfügen die resultierenden Daten in den lokalen Speicher.

* **Implizite legt**: Wenn ein Pullabonnement für eine Tabelle ausgeführt wird, ausstehende lokale Updates hat, führen die Pull einen Push zuerst auf den Synchronisierungskontext. Dadurch minimieren Sie Konflikte zwischen Änderungen, die bereits in der Warteschlange und neuen Daten vom Server.

* **Inkrementelle Synchronisierung**: der erste Parameter der Pull-Vorgang ist eine *Abfrage* , die nur auf dem Client verwendet wird. Wenn Sie ein nicht-Null-Abfrage verwenden, führt Azure Mobile SDK eine *inkrementelle Synchronisierung*.
  Jedes Mal ein Pull-Vorgang gibt Ergebnisse, die neuesten `updatedAt` Zeitstempel aus der Ergebnismenge in den lokalen SDK-Systemtabellen gespeichert. Nachfolgende Pull-Vorgänge werden nur Datensätze nach dieser Zeitstempel abgerufen.

  Um inkrementelle Synchronisierung zu verwenden, muss der Server sinnvoll zurückgeben `updatedAt` Werte und unterstützen auch nach diesem Feld sortieren. Da das SDK eigene Art im Feld UpdatedAt hinzufügt, Sie jedoch können nicht verwenden eine Pull-Abfrage, die eine eigene `$orderBy$` Klausel.

  Name der Abfrage kann eine beliebige Zeichenfolge, die Sie auswählen, aber es muss für jeden logischen Abfrage in Ihrer Anwendung.
  Andernfalls anderen Pullvorgänge können denselben Zeitstempel inkrementelle Synchronisierung überschreiben und Abfragen können falsche Ergebnisse zurück.

  Wenn die Abfrage Parameter enthält, ist eine Möglichkeit zum Erstellen eindeutiger Abfragename Parameterwert integrieren.
  Filtern auf Userid kann beispielsweise der Abfragenamen (in C#) werden:

        await todoTable.PullAsync("todoItems" + userid,
            syncTable.Where(u => u.UserId == userid));

  Wenn Sie inkrementelle Synchronisierung deaktivieren möchten, übergeben Sie `null` als Abfrage-ID. In diesem Fall werden alle Datensätze abgerufen werden, bei jedem Aufruf von `PullAsync`, potenziell aufgeführt.

* **Löschen**: Löschen Sie den Inhalt der lokalen Speicher mit `IMobileServiceSyncTable.PurgeAsync`.
  Dies möglicherweise veraltete Daten in der Clientdatenbank haben, ob alle ausstehende Änderungen verwerfen möchten.

  Eine Säuberung Löscht eine Tabelle aus dem lokalen Speicher. Sind Vorgänge ausstehend mit der Serverdatenbank, wird der Löschvorgang eine Ausnahme ausgelöst, wenn der Parameter *Force löschen* festgelegt ist.

  Beispielsweise von veralteten Daten auf dem Client Angenommen Sie im Beispiel "Aufgabenliste" Gerät1 nur Elemente abruft, die nicht abgeschlossen sind. Dann eine Todoitem "Milch kaufen" gekennzeichnet ist von einem anderen Gerät auf dem Server ausgeführt. Jedoch müssen Gerät1 noch "Milch kaufen" Todoitem lokalen Informationsspeicher, da sie nur Elemente ziehen, die nicht als erledigt gekennzeichnet sind. Eine Säuberung löscht diese veraltete Element.

## <a name="next-steps"></a>Nächste Schritte

* [iOS: offline-Synchronisierung aktivieren]
* [Xamarin iOS: offline-Synchronisierung aktivieren]
* [Xamarin Android: Offline-Synchronisierung aktivieren]
* [Universelle Windows Plattform: Offline-Synchronisierung aktivieren]

<!-- Links -->
[.NET Client SDK]: app-service-mobile-dotnet-how-to-use-client-library.md
[Android: Offline-Synchronisierung aktivieren]: app-service-mobile-android-get-started-offline-data.md
[iOS: offline-Synchronisierung aktivieren]: app-service-mobile-ios-get-started-offline-data.md
[Xamarin iOS: offline-Synchronisierung aktivieren]: app-service-mobile-xamarin-ios-get-started-offline-data.md
[Xamarin Android: Offline-Synchronisierung aktivieren]: app-service-mobile-xamarin-ios-get-started-offline-data.md
[Universelle Windows Plattform: Offline-Synchronisierung aktivieren]: app-service-mobile-windows-store-dotnet-get-started-offline-data.md
