<properties
    pageTitle="Arbeiten mit zuverlässigen | Microsoft Azure"
    description="Lernen Sie die optimalen Methoden für das Arbeiten mit zuverlässigen Sammlungen."
    services="service-fabric"
    documentationCenter=".net"
    authors="JeffreyRichter"
    manager="timlt"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="multiple"
    ms.date="03/28/2016"
    ms.author="jeffreyr" />

# <a name="working-with-reliable-collections"></a>Arbeiten mit zuverlässigen Sammlungen

Service Fabric bietet .NET Entwickler über zuverlässige Sammlungen ein statusbehaftetes Programmiermodell zur Verfügung. Insbesondere stellt Service Fabric zuverlässige Wörterbuch und zuverlässige Warteschlangeklassen. Bei Verwendung dieser Klassen ist der Zustand (für Skalierung) partitioniert (bei Verfügbarkeit) repliziert und innerhalb einer Partition (für ACID-Semantik). Wir typisch für eine zuverlässige Wörterbuchobjekt betrachten und sehen, welche die tatsächlich.

~~~
retry:
try {
   // Create a new Transaction object for this partition
   using (ITransaction tx = base.StateManager.CreateTransaction()) {
      // AddAsync takes key's write lock; if >4 secs, TimeoutException
      // Key & value put in temp dictionary (read your own writes),
      // serialized, redo/undo record is logged & sent to  
      // secondary replicas
      await m_dic.AddAsync(tx, key, value, cancellationToken);

      // CommitAsync sends Commit record to log & secondary replicas
      // After quorum responds, all locks released
      await tx.CommitAsync();
   }
   // If CommitAsync not called, Dispose sends Abort
   // record to log & all locks released
}
catch (TimeoutException) { 
   await Task.Delay(100, cancellationToken); goto retry; 
}
~~~

Alle Operationen auf zuverlässige Dictionary-Objekte (außer ClearAsync, die nicht rückgängig gemacht werden), erfordert ein ITransaction-Objekt. Dieses Objekt wurde zugeordnet alle und alle Änderungen, die Sie zuverlässige Wörterbuch und zuverlässig zu Warteschlange Objekte innerhalb einer einzelnen Partition. Sie erhalten eine ITransaction StateManagers CreateTransaction-Methode des Objekts durch Aufrufen der Partition.
 
Im obigen Code ist ITransaction-Objekt an eine zuverlässige Wörterbuch AddAsync-Methode übergeben. Dictionary-Methoden, die einen Schlüssel akzeptiert nehmen intern eine Reader-/Writersperre Schlüssel zugeordnet. Die Methode den Wert des Schlüssels ändert, die Methode nimmt eine Schreibsperre auf den Schlüssel und liest die Methode nur über den Wert des Schlüssels, dann eine Lesesperre wird auf den Schlüssel. Da AddAsync den Wert des Schlüssels auf den neuen übergebenen Wert ändert, wird der Schlüssel Schreibsperre übernommen. 2 (oder mehr) Threads versuchen, gleichzeitig Werte mit demselben Schlüssel hinzufügen, wird ein Thread die Schreibsperre erwerben und andere Threads blockieren. Standardmäßig blockiert Methoden für bis zu 4 Sekunden auf die Sperre. nach 4 Sekunden lösen die Methoden einer TimeoutException. Überladene Methoden vorhanden sind, können Sie einen explizites Timeout-Wert übergeben möchten.
 
Normalerweise schreiben Sie den Code, um auf eine TimeoutException reagieren abfangen und den gesamten Vorgang wiederholen (siehe obige Code). In meinem Code einfach rufe ich einfach Task.Delay 100 Millisekunden bei jedem übergeben. Aber in Wirklichkeit möglicherweise besser, eine Art exponentielle Backoff-Verzögerung verwenden.
 
Sobald die Sperre fügt AddAsync Objektverweise Schlüssel und den Wert einer internen temporären Wörterbuch ITransaction-Objekt zugeordnet. Dies bieten lesen-your-eigenen-schreibt Semantik. Also nach dem Aufruf von AddAsync gibt ein späterer Aufruf von TryGetValueAsync (mit demselben ITransaction Objekt) den Wert zurück, wenn die Transaktion noch nicht verpflichtet haben. Nächstes AddAsync serialisiert den Schlüssel und Wert in Bytearrays Objekte und fügt diese Bytearrays in einer Protokolldatei auf dem lokalen Knoten. Sendet AddAsync Bytearrays an alle sekundären Replikate haben dieselben Schlüssel-Wert-Informationen. Obwohl die Schlüssel-Wert-Informationen in eine Protokolldatei geschrieben wurde, ist die Informationen nicht als Teil des Wörterbuchs bis Transaktion, denen sie zugeordnet sind. 

Im obigen Code schreibt der Aufruf von CommitAsync alle Vorgänge der Transaktion. Insbesondere hängt Commit Informationen in der Protokolldatei auf dem lokalen Knoten und auch alle sekundären Replikate an der Commitdatensatz. Sobald ein Quorum (Mehrheit) Replikate geantwortet hat, werden alle Änderungen dauerhaft und zugeordneten Schlüssel über das ITransaction-Objekt manipuliert wurden Sperren werden freigegeben, damit andere Threads-Transaktionen die gleichen Schlüssel und deren Werte bearbeiten können.

Wenn CommitAsync (normalerweise aufgrund einer Ausnahme) nicht aufgerufen wird, ruft das ITransaction Objekt verworfen. Bei einer ungebundenen ITransaction-Objekt, fügt Service Fabric Abbrechen Informationen des lokalen Knotens Protokolldatei und nichts an einen sekundären Replikate gesendet werden. Und dann sperren zugeordneten Schlüssel der Transaktion geändert wurden.

## <a name="common-pitfalls-and-how-to-avoid-them"></a>Häufige Probleme und deren Vermeidung
Nun, Sie die zuverlässigen Sammlungen intern verstehen, werfen einen Blick auf einige allgemeine Missbrauch von ihnen. Finden Sie den folgenden Code:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   // AddAsync serializes the name/user, logs the bytes, 
   // & sends the bytes to the secondary replicas.
   await m_dic.AddAsync(tx, name, user);

   // The line below updates the property’s value in memory only; the
   // new value is NOT serialized, logged, & sent to secondary replicas.
   user.LastLogin = DateTime.UtcNow;  // Corruption!

   await tx.CommitAsync();
}
~~~

Beim Arbeiten mit regulären .NET Wörterbuch können Sie dem Wörterbuch einen Schlüsselwert hinzufügen und ändern Sie den Wert einer Eigenschaft (z. B. LastLogin). Jedoch wird dieser Code nicht mit einem zuverlässigen Wörterbuch ordnungsgemäß. Speichern Sie der Aufruf von AddAsync serialisiert die Schlüssel-Wert-Objekte in Bytearrays und Arrays in einer lokalen Datei gespeichert und auch sekundäre Replikate sendet aus früheren Diskussion. Wenn Sie später eine Eigenschaft ändern, ändert dieser Eigenschaftswert nur im Speicher; Sie beeinflusst nicht die lokale Datei oder Replikate gesendeten Daten. Bei, was im Speicher ist sofort ausgelöst. Wenn ein neuer Prozess gestartet wird oder ein anderes Replikat primäre wird, ist der alte Eigenschaftswert verfügbar ist. 

Ich kann nicht betonen, wie einfach es ist die obigen Fehler machen. Und nur lernen Sie den Fehler sollte der Prozess geht. Schreiben Sie den Code richtig ist einfach um zwei Zeilen zu stornieren:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   user.LastLogin = DateTime.UtcNow;  // Do this BEFORE calling AddAsync
   await m_dic.AddAsync(tx, name, user);
   await tx.CommitAsync(); 
}
~~~

Hier ist ein weiteres Beispiel einen häufiger Fehler angezeigt:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> user = 
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (user.HasValue) {
      // The line below updates the property’s value in memory only; the
      // new value is NOT serialized, logged, & sent to secondary replicas.
      user.Value.LastLogin = DateTime.UtcNow; // Corruption!
      await tx.CommitAsync(); 
   }
}
~~~

Wieder mit regulären .NET Wörterbücher der obige Code funktioniert und ist ein übliches Schema: der Entwickler verwendet einen Schlüssel, um einen Wert zu suchen. Wenn der Wert vorhanden ist, ändert der Entwickler den Wert einer Eigenschaft. Dieser Code zeigt jedoch mit zuverlässigen, dasselbe Problem wie bereits beschrieben: __ein Objekt darf nicht geändert werden, nachdem Sie eine zuverlässige Auflistung gegeben haben.__
 
Richtig zu einem zuverlässigen Auflistung aktualisieren ist einen Verweis auf den vorhandenen Wert und das Objekt durch diesen Verweis unveränderlich bezeichnet. Erstellen Sie ein neues Objekt ist eine Kopie des ursprünglichen Objekts an. Jetzt können Sie den Status dieses neuen Objekts ändern und schreiben das neue Objekt in der Auflistung, damit es in Bytearrays serialisiert wird die lokale Datei angehängt und Replikate an. Nach dem Commit der Änderung, haben die Objekte im Arbeitsspeicher, die lokale Datei und alle Replikate genauen denselben Zustand. Alles ist gut!

Der folgende Code zeigt richtig zu einem zuverlässigen Auflistung aktualisieren:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> currentUser = 
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (currentUser.HasValue) {
      // Create new user object with the same state as the current user object.
      // NOTE: This must be a deep copy; not a shallow copy. Specifically, only
      // immutable state can be shared by currentUser & updatedUser object graphs.
      User updatedUser = new User(currentUser);

      // In the new object, modify any properties you desire 
      updatedUser.LastLogin = DateTime.UtcNow;

      // Update the key’s value to the updateUser info
      await m_dic.SetValue(tx, name, updatedUser);

      await tx.CommitAsync(); 
   }
}
~~~

## <a name="define-immutable-data-types-to-prevent-programmer-error"></a>Definieren Sie unveränderliche Datentypen Programmierer Fehler

Idealerweise möchten wir den Compiler Fehler meldet, wenn Sie versehentlich Code erzeugen, der Zustand eines Objekts ändert, die unveränderlich berücksichtigen soll. Aber der C#-Compiler nicht die Möglichkeit dazu haben. Also um potenzielle Programmierer Fehler zu vermeiden, empfehlen wir die Typen definieren Sie mit zuverlässigen unveränderlichen Typen zu verwenden. Das heißt, Core Werttypen (z. B. Zahlen [Int32 UInt64 usw.], DateTime, Guid, TimeSpan usw.) verwenden. Und natürlich können Sie auch Zeichenfolge. Am besten vermeiden Auflistungseigenschaften serialisieren und Deserialisierung kann häufig können Leistung beeinträchtigen. Wenn Sie die Auflistung verwenden möchten, empfehlen wir die Verwendung von. Die unveränderbare Sammlungen Netzwerkbibliothek (System.Collections.Immutable). Diese Bibliothek wird von http://nuget.org downloaden. Wir empfehlen auch versiegeln von Klassen und Felder schreibgeschützt Wenn möglich.

Welche UserInfo veranschaulicht das Definieren eines unveränderlichen Typs genannten Empfehlung nutzen.

~~~
[DataContract]
// If you don’t seal, you must ensure that any derived classes are also immutable
public sealed class UserInfo {
   private static readonly IEnumerable<ItemId> NoBids = ImmutableList<ItemId>.Empty;
 
   public UserInfo(String email, IEnumerable<ItemId> itemsBidding = null) {
      Email = email;
      ItemsBidding = (itemsBidding == null) ? NoBids : itemsBidding.ToImmutableList();
   }

   [OnDeserialized]
   private void OnDeserialized(StreamingContext context) {
      // Convert the deserialized collection to an immutable collection
      ItemsBidding = ItemsBidding.ToImmutableList();
   }
 
   [DataMember]
   public readonly String Email;
 
   // Ideally, this would be a readonly field but it can't be because OnDeserialized 
   // has to set it. So instead, the getter is public and the setter is private.
   [DataMember]
   public IEnumerable<ItemId> ItemsBidding { get; private set; }

   // Since each UserInfo object is immutable, we add a new ItemId to the ItemsBidding
   // collection by creating a new immutable UserInfo object with the added ItemId.
   public UserInfo AddItemBidding(ItemId itemId) {
      return new UserInfo(Email, ((ImmutableList<ItemId>)ItemsBidding).Add(itemId));
   }
}
~~~

ItemId-Typ ist auch ein unveränderlicher Typ, wie hier gezeigt:

~~~
[DataContract]
public struct ItemId {

   [DataMember] public readonly String Seller;
   [DataMember] public readonly String ItemName;
   public ItemId(String seller, String itemName) {
      Seller = seller;
      ItemName = itemName;
   }
}
~~~

## <a name="schema-versioning-upgrades"></a>Schema Versioning (Upgrades)

Intern serialisiert zuverlässige Sammlungen Objekte verwenden. NET DataContractSerializer. Serialisierte Objekte auf Festplatte primäres Replikat beibehalten und werden auch die sekundären Replikate. Mit fortschreitender Ihren Dienst dürfte Ihr Dienst benötigten Daten (Schema) ändern möchten. Sie müssen Versionen Ihrer Daten mit Sorgfalt Ansatz. Zunächst müssen Sie immer alte Daten deserialisiert sein. Insbesondere Deserialisierungscode muss daher unendlich abwärtskompatibel: 333 Version des Codes Service muss auf Daten in einer zuverlässigen Auflistung von Version 1 der Dienstcode 5 Jahren.

Außerdem ist Servicecode aktualisiert eine Aktualisierungsdomäne gleichzeitig. Während der Aktualisierung müssen Sie also zwei verschiedene Versionen des Service Codes gleichzeitig. Vermeiden Sie die neue Version des Codes Service das neue Schema alte Versionen von Service-Code behandeln das neue Schema möglicherweise nicht verwenden. Wenn möglich, sollten Sie jede Version Ihres Dienstes aufwärtskompatibel 1 Version zu entwerfen. Insbesondere bedeutet dies, dass V1 Service Code einfach alle Schemaelemente ignorieren, die nicht explizit verarbeitet werden. Allerdings werden sie speichern Daten, denen sie explizit nicht bekannt ist und einfach Zurückschreiben ein Wörterbuchschlüssel oder Wert aktualisieren. 

>[AZURE.WARNING] Beim Ändern des Schemas eines Schlüssels müssen Sie sicherstellen, dass Hashcode des Schlüssels und Algorithmen gleich sind. Ändern Sie entweder diese Algorithmen funktionieren werden Sie nicht den Schlüssel im Wörterbuch zuverlässig wieder suchen.
  
Alternativ können Sie in der Regel als Upgrade Phase 2 genannte durchführen. Mit einem Upgrade 2 Phasen Aktualisierung den Dienst V1 auf V2: V2 enthält den Code, der weiß, wie sich mit dem neuen Schema, aber dieser Code nicht ausgeführt. Wenn der Code V2 V1 Daten liest, arbeitet auf und schreibt V1 Daten. Dann nach Abschluss der Aktualisierung alle Upgrades domänenübergreifend, können Sie irgendwie ausgeführten Instanzen V2 signalisieren, dass die Aktualisierung abgeschlossen ist. (Eine Möglichkeit, Signal ist das Rollout Konfiguration aktualisieren, ist dies eine Aktualisierung 2 Phasen.) Jetzt können V2-Instanzen V1 Daten V2-Daten konvertieren, verarbeiten und als V2-Daten schreiben. Wenn andere Instanzen V2-Daten gelesen, benötigen keinen konvertieren und sie nur verarbeiten V2-Daten schreiben.

## <a name="next-steps"></a>Nächste Schritte
Zum Erstellen von Verträgen kompatible Daten finden Sie unter [Datenverträge kompatibel](https://msdn.microsoft.com/library/ms731083.aspx).

Best Practices für Datenverträge Versionskontrolle finden Sie unter [Daten Vertrag Versioning](https://msdn.microsoft.com/library/ms731138.aspx). 

Weitere Informationen zum Implementieren von fehlertoleranten Datenverträge Version anzeigen Sie [Versionstolerante Serialisierung Rückrufe](https://msdn.microsoft.com/library/ms733734.aspx) 

Wie eine Datenstruktur, die über mehrere Versionen zusammenarbeiten können, finden Sie unter [IExtensibleDataObject](https://msdn.microsoft.com/library/system.runtime.serialization.iextensibledataobject.aspx).
