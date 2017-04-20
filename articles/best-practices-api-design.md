<properties
   pageTitle="API-Designrichtlinien | Microsoft Azure"
   description="Hinweise auf ein erstellen entwickelt API."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="rest-api"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/13/2016"
   ms.author="masashin"/>

# <a name="api-design-guidance"></a>API-Designrichtlinien

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

## <a name="overview"></a>Übersicht

Viele moderne Web-basierten Lösungen nutzen Webdienste von Webservern zu Funktionen für remote Client Applications. Operationen, die einen Webdienst verfügbar macht bilden eine Web-API. Eine gut entworfene Web API soll unterstützen:

- **Plattformunabhängig**. Clientanwendungen sollen die API verwenden, die der Webdienst ohne Daten oder Operationen API stellt physische Implementierung bereitstellt. Dazu muss die API gemeinsame Standards einhält, mit denen ein Clientdienst Anwendung sowie auf die Formate und die Struktur der Daten, die zwischen Clientanwendungen und dem Webdienst ausgetauscht werden.

- **Service-Entwicklung**. Der Webdienst sollte entwickeln und hinzufügen (oder entfernen) Funktionen unabhängig von Clientanwendungen. Vorhandene Clientanwendungen sollten weiterhin unverändert als Funktionen von Web Service ändern können. Alle Funktionen sollte sichtbar, auch damit Clientanwendungen vollständig nutzen können.

Diese Anleitung dient Probleme beschreiben, die beim Entwerfen einer Webs-API berücksichtigen sollten.

## <a name="introduction-to-representational-state-transfer-rest"></a>Einführung in Representational State Transfer (REST)

In Dissertation 2000 vorgeschlagenen Roy Fielding eine Alternative zur Strukturierung der Operations Webdienste Architektur; REST. REST ist eine Architektur für die Erstellung verteilter Systeme Hypermedia. Ein wichtiger Vorteil von REST-Modell ist basiert auf offenen Standards und die Implementierung des Modells oder Clientanwendungen, die Zugriff auf eine bestimmte Implementierung nicht binden. Beispielsweise ein REST-Webdienst kann mit Microsoft ASP.NET Web API implementiert und Clientanwendungen können mit jeder Sprache und Toolset, das Generieren von HTTP-Anfragen und HTTP-Antworten analysieren entwickelt werden.

> [AZURE.NOTE] REST ist tatsächlich unabhängig vom zugrunde liegenden Protokoll und nicht notwendigerweise an HTTP gebunden. Häufigsten Implementierung von Systemen, die auf anderen verwenden jedoch HTTP als Anwendungsprotokoll zum Senden und Empfangen von Anfragen. Dieses Dokument konzentriert sich auf Systeme, die für den Betrieb mit HTTP REST-Prinzipien zuordnen.

REST-Modell verwendet ein Navigation Objekten und Diensten in einem Netzwerk (als _Ressourcen_bezeichnet) dargestellt. Viele Systeme, die übrigen in der Regel implementieren verwenden das HTTP-Protokoll Anfragen auf diese Ressourcen zuzugreifen. In diesen Systemen fordert eine Clientanwendung aus ein URI, der eine Ressource identifiziert und HTTP-Methode (das häufigste, GET, POST, PUT oder DELETE), die mit der Ressource auszuführenden Vorgang angibt.  Der Hauptteil der HTTP-Anforderung enthält die Daten, die zum Ausführen des Vorgangs erforderlich. Wichtig zu verstehen, ist, dass REST statusfreie Anforderung Modell definiert. HTTP-Anfragen unabhängig sein und können in beliebiger Reihenfolge auftreten, so versucht Übergangszustand Informationen zwischen nicht möglich ist.  Ressourcen selbst ist der einzige Ort, wo die Informationen gespeichert werden, und jede Anforderung sollte eine atomare Operation sein. Ein REST-Modell implementiert, einen endlichen Automaten, eine Anforderung eine Ressource aus einem definierten nicht flüchtigen Zustand in einen anderen übergeht.

> [AZURE.NOTE] Zustandsfrei sind einzelne Anfragen im REST-Modell ermöglicht ein System anhand dieser Prinzipien zu hoch skalierbare erstellt. Gibt es keine Affinität zwischen einer Clientanwendung, die eine Reihe von Anfragen und bestimmten Webserver behandeln diese Anfragen beibehalten müssen.

Entscheidend bei effektives Modell für die übrigen ist die Beziehung zwischen den verschiedenen Ressourcen verstehen, das Modell zugreifen. Diese Ressourcen sind in der Regel als Sammlungen und Beziehungen organisiert. Angenommen, eine schnelle Analyse eines e-Commerce-Systems zeigt, dass es zwei Sammlungen in Client wahrscheinlich interessiert sind: Orders und Customers. Jede Bestellung und Kunden müssen eigene eindeutigen Schlüssel zur Identifizierung. Der URI der Sammlung von Aufträgen konnte etwas so einfaches wie _soll_und ebenso sein URI zum Abrufen aller Kunden _"/ Customers"_. Ausgabe einer HTTP GET-Anforderung an _soll_ URI sollte eine Liste aller Aufträge in der Auflistung als eine HTTP-Antwort darstellt zurück:

```HTTP
GET http://adventure-works.com/orders HTTP/1.1
...
```

Die Antwort unten codiert die Aufträge als JSON-Listenstruktur:

```HTTP
HTTP/1.1 200 OK
...
Date: Fri, 22 Aug 2014 08:49:02 GMT
Content-Length: ...
[{"orderId":1,"orderValue":99.90,"productId":1,"quantity":1},{"orderId":2,"orderValue":10.00,"productId":4,"quantity":2},{"orderId":3,"orderValue":16.60,"productId":2,"quantity":4},{"orderId":4,"orderValue":25.90,"productId":3,"quantity":1},{"orderId":5,"orderValue":99.90,"productId":1,"quantity":1}]
```
Um eine einzelne Bestellung abrufen muss den Bezeichner für den Auftrag aus der Ressource _Aufträge_ wie _/orders/2_:

```HTTP
GET http://adventure-works.com/orders/2 HTTP/1.1
...
```

```HTTP
HTTP/1.1 200 OK
...
Date: Fri, 22 Aug 2014 08:49:02 GMT
Content-Length: ...
{"orderId":2,"orderValue":10.00,"productId":4,"quantity":2}
```

> [AZURE.NOTE] Der Einfachheit halber zeigt Beispiele den Antworten als JSON-Text-Daten zurückgegeben. Jedoch besteht kein Grund, warum Ressourcen nicht sonstige unterstützt über HTTP, wie binäre oder verschlüsselte Informationen enthält; der Inhaltstyp der HTTP-Antwort sollte den Typ angeben. Auch kann ein REST-Modell dieselben Daten in verschiedenen Formaten wie XML oder JSON zurückgeben können. In diesem Fall sollte der Webdienst Inhalt verhandeln mit dem anfordernden Client durchführen können. Die Anforderung zählen _Accept_ -Header gibt das bevorzugte Format an, die der Client möchte und der Webdienst sollte dabei möglichst berücksichtigt.

Beachten Sie, dass die Antwort von einer weiteren Anforderung verwendet den standard-HTTP-Statuscodes. Eine Anforderung, die gültige Daten gibt sollte beispielsweise den HTTP-Antwortcode 200 (OK), während eine Anforderung, die nicht finden oder löschen eine angegebene Ressource eine Antwort zurückgeben soll, die den HTTP-Statuscode 404 (nicht gefunden) enthält.

## <a name="design-and-structure-of-a-restful-web-api"></a>Gestaltung und Struktur eines RESTful API

Die Schlüssel zum Entwerfen einer erfolgreichen Web-APIs sind Einfachheit und Konsistenz. Eine Web-API, die diese beiden Faktoren zeigt erleichtert die Clientanwendungen erstellen, die die API verwenden.

Eine RESTful Web API konzentriert sich auf ein Satz von Ressourcen, mit denen eine Anwendung diese Ressourcen bearbeiten und Navigieren zwischen diesen Kerngeschäft. Aus diesem Grund sollten URIs, die ein RESTful Webdienst API bilden macht Daten ausgerichtet sein und verwenden die Funktionen von HTTP auf diese Daten. Dieser Ansatz erfordert eine andere Einstellung aus, die normalerweise bei einer Reihe von Klassen in einer objektorientierten API wodurch mehr motiviert das Verhalten von Objekten und Klassen verwendet. Darüber hinaus sollte RESTful Web API statusfrei sein und nicht auf in einer bestimmten Reihenfolge aufgerufen. In den folgenden Abschnitten zusammengefasst Punkte sollten Sie beim Entwerfen einer RESTful Web-API.

### <a name="organizing-the-web-api-around-resources"></a>Organisieren von Ressourcen im Web API

> [AZURE.TIP] Von einem REST-Webdienst verfügbar gemachten URIs sollte anhand der Substantive (die Daten, die Web-API Zugriff bietet) und keine Verben (eine Anwendung mit den Daten tun kann).

Web-API macht Geschäftseinheiten konzentrieren. Beispielsweise werden in einer Web-API unterstützt das e-Commerce-System beschriebenen primären Entitäten Customers und orders Wie Act Bestellung lässt sich durch die Bereitstellung einer HTTP POST-Operation, die die Bestellinformationen und die Liste der Aufträge für den Debitor hinzugefügt. Intern kann diese POST-Operation Lagerbestände überprüfen, wie der Kunde Abrechnung Aufgaben. Die HTTP-Antwort kann angeben, ob die Bestellung erfolgreich aufgegeben wurde. Beachten Sie, dass eine Ressource nicht auf einem einzelnen physischen Datenelement. Beispielsweise kann eine Ressource Reihenfolge intern Informationen aggregiert viele Zeilen auf mehrere Tabellen in einer relationalen Datenbank verteilt, aber auf dem Client als eine einzige Entität angezeigt implementiert werden.

> [AZURE.TIP] Vermeiden Sie entwerfen eine REST-Schnittstelle, die gespiegelt oder interne Struktur macht Daten abhängt. REST ist über mehr als einfache (erstellen, abrufen, aktualisieren, löschen) Vorgänge in separate Tabellen in einer relationalen Datenbank implementieren. Der REST soll diese physischen Details zuordnen Geschäftseinheiten und die Vorgänge, die eine Anwendung kann auf diese Entitäten die physische Implementierung dieser Entitäten ausführen, jedoch kein Client verfügbar gemacht werden.

Einzelne Unternehmen selten isoliert existieren (obwohl einige Singleton-Objekte möglicherweise vorhanden ist), aber stattdessen in Gruppen zusammengefasst werden. Im übrigen sind, jede Entität jede Sammlung Ressourcen. Rest Web API jede Auflistung hat eigene URI innerhalb des Webdiensts und Ausführen einer HTTP GET-Anforderung über einen URI für eine Auflistung ruft eine Liste der Elemente in dieser Auflistung. Jedes einzelnes Element verfügt auch über einen eigenen URI und anderen HTTP GET-Anforderung, die Details dieses Elements abrufen mit diesen URI beantragen können. Sie sollten die URIs für Sammlungen und Objekte hierarchisch organisieren. Im System eCommerce-URI _"/ Customers"_ gibt der Kunde Auflistung und _/customers/5_ die Details für die einzelnen Kunden mit 5-ID aus der Auflistung abgerufen. Dieser Ansatz hilft, die Web-API intuitiv.

> [AZURE.TIP] Erlassen Sie eine konsistente Namenskonvention in URIs. im Allgemeinen sollten sie für URIs, Verweis Sammlungen Nomen im plural verwenden.

Sie müssen auch beachten die Beziehung zwischen verschiedenen Ressourcen und wie Sie diese Zuordnung können. Beispielsweise können Kunden oder mehr bestellen. Natürliche Weise diese Beziehung dargestellt müssten durch einen URI wie _/customers/5/orders_ alle Aufträge für Kunden 5 finden. Auch sollten Sie die Zuordnung aus einer Bestellung für einen bestimmten Debitor über einen URI wie _/orders/99/customer_ Reihenfolge 99 zu den Kunden darstellt kann, aber erweitert dieses Modell weit schwierig zu implementieren. Eine bessere Lösung ist die Navigation Links zu verbundenen Ressourcen wie Kunden im Hauptteil der HTTP-Antwort zurückgegeben, wenn die Reihenfolge abgefragt wird. Dieser Mechanismus wird ausführlicher im Abschnitt mit dem HATEOAS Ansatz aktivieren Navigation zu verwandten Ressourcen weiter unten in diesem Handbuch beschrieben.

Komplexeren Systemen möglicherweise viele weitere Arten von Entität und kann verlockend zu URIs, mit denen eine Clientanwendung über mehrere Ebenen von Beziehungen, wie _/customers/1/orders/99/products_ die Liste der Produkte in der Reihenfolge vom Debitor 1 99 zu navigieren. Dieser Grad der Komplexität kann jedoch schwierig zu verwalten und unflexibel Beziehungen zwischen Ressourcen in der Zukunft ändern. Stattdessen sollten Sie URIs relativ einfach zu halten. Bedenken Sie, sobald eine Anwendung einen Verweis auf eine Ressource hat, mit diesem Verweis Elemente im Zusammenhang mit dieser Ressource suchen soll. Die vorherige Abfrage kann mit der URI- _/customers/1/orders_ finden alle Aufträge für Kunden 1 und dann Abfragen, URI- _/orders/99/products_ finden Sie die Produkte in dieser Reihenfolge (vorausgesetzt, 99 Kunde 1 Bestellung) ersetzt werden.

> [AZURE.TIP] Vermeiden Sie Ressourcen-URIs komplexer _Auflistung/Element/Auflistung_benötigen.

Ein weiterer wichtiger Aspekt ist, dass alle Webanfragen eine Last auf dem Server bedeuten und größer die Anzahl je größer die Last anfordert. Sie sollten versuchen, definieren die Ressourcen zur Vermeidung von "Ausführliche" Web-APIs, die eine große Anzahl von kleinen Ressourcen verfügbar machen. Eine API erfordern eine Clientanwendung mehrere Anträge zu erfordert die Daten. Es kann vorteilhaft sein Denormalisieren von Daten und Informationen zu größeren Ressourcen durch eine einzige Anforderung abgerufen werden können. Sie müssen jedoch gegen den Aufwand für das Abrufen von Daten, die nicht häufig vom Client möglicherweise dadurch ausgeglichen. Abrufen von großen Objekten kann erhöhen die Latenz einer Anforderung und Kosten zusätzliche Bandbreite für wenig wird die zusätzlichen Daten nicht häufig verwendet.

Vermeiden Sie Abhängigkeiten Web API Struktur, Typ und Speicherort der zugrunde liegenden Datenquellen. Beispielsweise wenn Ihre Daten in einer relationalen Datenbank gespeichert, muss die Web-API nicht gemacht Tabelle(n) als eine Sammlung von Ressourcen. Web-API als eine Abstraktion der Datenbank, und ggf. Führen Sie Zuordnungsschicht zwischen der Datenbank und Web-API ein. Auf diese Weise, Entwurf und Implementierung der Datenbank ändert (z. B. Sie verschieben aus einer relationalen Datenbank mit einer Auflistung der normalisierten Tabellen eine denormalisierte NoSQL-Speichersystem wie eine Datenbank) Clientanwendungen aus diesen isoliert.
> [AZURE.TIP] Die Datenquelle, die eine Web-API unterstützt keinen zu einem Datenspeicher; ein anderer Dienst sein oder LOB Anwendung oder sogar einer älteren Anwendung lokal innerhalb einer Organisation.

Schließlich könnte es nicht ordnen Sie jedem Vorgang auf eine bestimmte Ressource einer Web-API implementiert möglich. Sie können diese _nicht Ressource_ über HTTP GET-Anfragen Szenarios, die ein aufrufen und die Ergebnisse als eine HTTP-Response-Nachricht zurückgegeben. Web-API, die einfacheren Rechner Operationen implementiert wie Addition und Subtraktion könnten URIs, die diese Vorgänge als Pseudo-Ressourcen verfügbar machen und die Abfragezeichenfolge Parameter angeben. Beispielsweise eine GET-Anforderung an den URI _/add?operand1 = 99 operand2 = 1_ konnte eine Antwortnachricht mit der eigentliche Wert 100, und GET-Anforderung an den URI _/subtract?operand1 = 50 & operand2 = 20_ konnte eine Antwortnachricht mit der eigentliche Wert 30 zurück. Jedoch nur sparsam diesen URIs.

### <a name="defining-operations-in-terms-of-http-methods"></a>Definieren von Vorgängen in HTTP-Methoden

Das HTTP-Protokoll definiert eine Reihe von Methoden, die semantische Bedeutung einer Anforderung zuweisen. Allgemeinen HTTP-Methoden erholsamen Web-APIs sind:

- **Abrufen**eine Kopie der Ressource im angegebenen URI abgerufen. Der Textkörper der Antwort enthält die Details der angeforderten Ressource.

- **POST**, zum Erstellen einer neuen Ressource am angegebenen URI. Der Hauptteil der Anforderungsnachricht enthält die Details der neuen Ressource. Beachten Sie, dass POST auch Vorgänge auslösen, die tatsächlich Ressourcen erstellen nicht verwendet werden kann.

- **Speichern**, ersetzen oder Aktualisieren der Ressource am angegebenen URI. Der Hauptteil der Anforderungsnachricht gibt die Ressource geändert werden und die Werte.

- **Löschen**, entfernen Sie die Ressource unter dem angegebenen URI.

> [AZURE.NOTE] Das HTTP-Protokoll definiert auch andere weniger gebräuchliche Methoden, wie PATCH, der verwendet wird, um ausgewählte Updates auf eine Ressource anfordern, HEAD, mit dem eine Ressource Optionen Clientinformationen Informationen über die vom Server unterstützten Kommunikationsoptionen ermöglicht angefordert und verfolgen, wodurch einen Client Informationen, die für Test-und Diagnosezwecken verwenden können.

Die Auswirkung einer bestimmten Anforderung sollte abhängig, ob die Ressource auf der es angewendet wird eine Auflistung oder ein einzelnes Element ist. In der folgenden Tabelle werden die allgemeinen Konventionen vom erholsamen Implementierungen eCommerce-Beispiel zusammengefasst. Beachten Sie, dass nicht alle diese Anfragen implementiert werden könnte. Das hängt von des jeweiligen Szenarios ab.

| **Ressource** | **Bereitstellen** | **Erhalten** | **EINFÜGEN** | **LÖSCHEN** |
|--------------|----------|---------|---------|------------|
| "/ Customers" | Neuen Kunden erstellen | Alle Kunden abrufen | Massenaktualisieren von Kunden (_Falls implementiert_) | Entfernen Sie alle Kunden |
| Kunden/1 | Fehler | Abrufen von Details für Debitor 1 | Aktualisieren Sie Kunden 1 Falls vorhanden, andernfalls zurück ein Fehler beim | Debitor 1 entfernen |
| /Customers/1/Orders | Erstellen Sie einen neuen Auftrag für Kunde 1 | Alle Aufträge für Kunden 1 abrufen | Gleichzeitig aktualisieren der Aufträge für Kunden 1 (_Falls implementiert_) | Entfernen Sie alle Aufträge für Kunden 1 (_Falls implementiert_) |

GET und DELETE-Anfragen werden relativ einfach, aber gibt Verwirrung über den Zweck und die Effekte der POST und PUT-Anfragen.

Eine POST-Anforderung sollte mit Daten im Hauptteil der Anforderung eine neue Ressource erstellen. Im übrigen Modell wenden Sie häufig POST-Anfragen auf Ressourcen Sammlungen; die Auflistung wird die neue Ressource hinzugefügt.

> [AZURE.NOTE] Sie können auch POST-Anfragen, die einige Funktionen auslösen und nicht notwendigerweise zurückgeben, und solche Anforderung Sammlungen angewendet werden. Beispielsweise können Sie eine POST-Anforderung übergeben einer Arbeitszeittabelle an einer Payroll Service und die berechnete Mehrwertsteuer als Antwort.

Eine PUT-Anforderung soll eine vorhandene Ressource zu ändern. Wenn die angegebene Ressource nicht vorhanden ist, kann die PUT-Anforderung ein Fehler zurückgegeben (in einigen Fällen es möglicherweise tatsächlich erstellen die Ressource). PUT-Anfragen sind häufig auf Ressourcen angewendet, die einzelne Elemente (z. B. einen bestimmten Debitor oder Auftrag), obwohl sie Sammlungen angewendet werden können, obwohl dies weniger häufig implementiert. Hinweis die PUT Idempotent sind fordert während POST-Anfragen nicht; Wenn eine Anwendung die gleiche sendet Stelle mehrmals Ergebnisse sollten immer gleich sein (dieselbe Ressource wird mit den gleichen Werten geändert werden), aber wenn eine Anwendung die gleiche POST-Anforderung wiederholt werden die Erstellung mehrerer Ressourcen.

> [AZURE.NOTE] Genau genommen ersetzt eine HTTP PUT-Anforderung eine vorhandene Ressource im Hauptteil der Anforderung angegebene Ressource. Wenn ein Ändern der Eigenschaften einer Ressource aber anderen Eigenschaften unverändert, sollte dies implementiert per HTTP PATCH-Anforderung. Jedoch viele RESTful-Implementierung dieser Regel entspannen und PUT für beide Situationen.

### <a name="processing-http-requests"></a>HTTP-Anfragen verarbeiten
Daten von einer Clientanwendung in viele HTTP-Anfragen und die entsprechenden Antwortnachrichten vom Webserver können in verschiedenen Formaten (oder Medien) angezeigt. Beispielsweise konnte als XML, JSON oder ein anderes Format codiert und komprimiert die Daten, die Details für einen Debitor oder Auftrag bereitgestellt werden. Rest Web-API unterstützt verschiedene Medientypen, wie von der Clientanwendung, die eine Anforderung übermittelt.

Wenn eine Clientanwendung eine Anforderung, die Daten im Hauptteil einer Nachricht zurückgibt sendet, können sie die Medientypen behandeln im Accept-Header der Anforderung angeben. Der folgende Code zeigt eine HTTP GET-Anforderung, die Kunden 1 abruft und fordert das Ergebnis als JSON zurückgegeben werden (der Client prüfen noch den Medientyp der Daten auf das Format der zurückgegebenen Daten überprüfen):

```HTTP
GET http://adventure-works.com/orders/2 HTTP/1.1
...
Accept: application/json
...
```

Wenn der Webserver diesen Medientyp unterstützt, kann es mit einer Antwort Antworten Content-Type-Header, das Format der Daten im Hauptteil der Nachricht angibt:

> [AZURE.NOTE] Für eine optimale Interoperabilität erkannt, annehmen und Content-Type-Header referenzierten Typen Media MIME-Typen als einige benutzerdefinierte Medientyp.

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
Content-Length: ...
{"orderID":2,"productID":4,"quantity":2,"orderValue":10.00}
```

Wenn der Webserver den gewünschten Medientyp nicht unterstützt, können sie die Daten in einem anderen Format senden. IN allen Fällen müssen sie den Medientyp (z. B. _Anwendung/Json_) im Content-Type-Header angeben. Es obliegt der Clientanwendung die Antwortnachricht analysieren und Interpretieren der Ergebnisse im Nachrichtentext entsprechend.

Beachten Sie, dass in diesem Beispiel Webserver erfolgreich Ruft die angeforderten Daten erfolgreich übergeben wieder im Antwortheader Statuscode 200 gibt. Wenn keine übereinstimmenden Daten gefunden werden, sollten sie stattdessen einen Statuscode 404 (nicht gefunden) zurückgeben und den Text der Antwortnachricht enthalten zusätzliche Informationen. Das Format dieser Daten wird vom Content-Type-Header angegeben, wie im folgenden Beispiel gezeigt:

```HTTP
GET http://adventure-works.com/orders/222 HTTP/1.1
...
Accept: application/json
...
```

Reihenfolge 222 ist nicht vorhanden, damit die Antwortnachricht folgendermaßen aussieht:

```HTTP
HTTP/1.1 404 Not Found
...
Content-Type: application/json; charset=utf-8
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
Content-Length: ...
{"message":"No such order"}
```

Sendet eine Anwendung eine HTTP PUT-Anforderung eine Ressource aktualisieren, gibt den URI der Ressource und der Daten im Hauptteil der Anforderungsnachricht geändert werden. Er sollte das Format dieser Daten auch mit dem Content-Type-Header angeben. Ein allgemeine Format für Textinformationen ist _Application/X-www-form-urlencoded_, umfasst eine Reihe von Name-Wert-Paare getrennt durch die & Zeichen. Das nächste Beispiel zeigt eine HTTP PUT-Anforderung, die die Informationen 1 ändert:

```HTTP
PUT http://adventure-works.com/orders/1 HTTP/1.1
...
Content-Type: application/x-www-form-urlencoded
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
Content-Length: ...
ProductID=3&Quantity=5&OrderValue=250
```

Wenn die Änderung erfolgreich ist, sollte er idealerweise mit einem 204 HTTP-Statuscode angibt, dass der Prozess erfolgreich verarbeitet wurde, aber Antworttext keine weiteren Informationen enthält. Location-Header in der Antwort enthält den URI der aktualisierten Ressource:

```HTTP
HTTP/1.1 204 No Content
...
Location: http://adventure-works.com/orders/1
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
```

> [AZURE.TIP] Wenn eine HTTP PUT-Anforderungsnachricht Datums-und Uhrzeitinformationen enthält, stellen Sie sicher, dass der Webdienst Daten akzeptiert und Zeiten formatiert nach dem Standard ISO 8601.

Aktualisiert die Ressource nicht vorhanden ist, kann der Webserver wie beschrieben mit einer Antwort nicht gefunden reagieren. Auch wenn der Server das Objekt selbst erstellt konnte zurückgegeben Statuscodes HTTP 200 (OK) oder HTTP-201 (erstellt) und der Antworttext konnte die Daten für die neue Ressource enthält. Wenn der Content-Type-Header der Anforderung ein Datenformat, die der Webserver behandeln kann angibt, sollte es mit HTTP-Statuscode 415 (Unsupported Media Type).

> [AZURE.TIP] Sollten Sie implementieren HTTP PUT-Massenvorgänge, die batch Updates für mehrere Ressourcen in einer Auflistung. Die PUT-Anforderung sollte den URI der Auflistung angeben und den Hauptteil der Anforderung sollte die Angaben der Ressourcen geändert werden. Dadurch können ausgetauschten verringern und die Leistung verbessert.

Das Format der HTTP POST-Anfragen, die neue Ressourcen erstellen sind ähnlich PUT-Anfragen. der Nachrichtentext enthält die Details der neuen Ressource hinzugefügt werden. Allerdings gibt den URI normalerweise die Auflistung, die die Ressource hinzugefügt werden. Das folgende Beispiel erstellt einen neuen Auftrag und die Orders-Auflistung hinzugefügt:

```HTTP
POST http://adventure-works.com/orders HTTP/1.1
...
Content-Type: application/x-www-form-urlencoded
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
Content-Length: ...
productID=5&quantity=15&orderValue=400
```

Wenn die Anforderung erfolgreich ist, sollte der Webserver mit einer Nachricht mit HTTP-Statuscode 201 (erstellt). Location-Header sollte den URI der neu erstellten Ressource enthalten und der Hauptteil der Antwort eine neue Ressource; der Content-Type-Header gibt das Format der Daten:

```HTTP
HTTP/1.1 201 Created
...
Content-Type: application/json; charset=utf-8
Location: http://adventure-works.com/orders/99
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
Content-Length: ...
{"orderID":99,"productID":5,"quantity":15,"orderValue":400}
```

> [AZURE.TIP] PUT oder POST-Anforderung die Daten ungültig ist, sollte der Webserver mit einer Nachricht mit HTTP-Statuscode 400 (Ungültige Anforderung). Der Nachrichtentext enthalten zusätzliche Informationen über das Problem mit der Anforderung und den erwarteten Formaten oder einen Link zu einer URL, die Weitere Informationen enthalten.

Zum Entfernen einer Ressource bietet eine HTTP DELETE-Anforderung den URI der Ressource gelöscht werden. Im folgenden Beispiel wird versucht, Bestellung 99 zu entfernen:

```HTTP
DELETE http://adventure-works.com/orders/99 HTTP/1.1
...
```

Wenn der Löschvorgang erfolgreich ist, muss der Webserver reagiert mit HTTP-Statuscode 204 angibt, dass der Prozess erfolgreich verarbeitet wurde, aber der Antworttext enthält keine weiteren Informationen (dieselbe Antwort zurückgegebene erfolgreich PUT-Vorgang ist dies ohne Header Speicherort wie die Ressource nicht mehr vorhanden ist.) Es kann auch eine Löschanfrage HTTP-Statuscode 200 (OK) oder 202 (akzeptiert) zurückgegeben, wenn der Löschvorgang asynchron ausgeführt wird.

```HTTP
HTTP/1.1 204 No Content
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
```

Wenn die Ressource nicht gefunden wird, sollte der Webserver eine 404 (nicht gefunden) angezeigt stattdessen zurückgeben.

> [AZURE.TIP] Wenn alle Ressourcen in einer Auflistung werden gelöscht müssen, aktivieren Sie eine HTTP DELETE-Anforderung für den URI eine Anwendung zwingt, jede Ressource wiederum aus der Auflistung entfernen, anstatt der Auflistung angegeben werden.

### <a name="filtering-and-paginating-data"></a>Filtern und Daten Paginieren

Sie sollten sich bemühen, die URIs einfach und intuitiv zu halten. Verfügbarmachen einer Sammlung von Ressourcen über einen einzigen URI unterstützt in dieser Hinsicht aber führt Anwendung große Datenmengen abrufen, wenn nur eine Teilmenge der Informationen erforderlich ist. Generiert eine große Menge von Datenverkehr auswirkt, nicht nur die Leistung und Skalierung des Webservers aber auch beeinträchtigen die Reaktionsfähigkeit von Clientanwendungen, die angeforderten Daten.

Beispielsweise enthält Aufträge Preis für den Auftrag müssen eine Clientanwendung, die alle Aufträge abrufen Kosten über einen bestimmten Wert alle Aufträge aus _soll_ URI abgerufen und diese Aufträge lokal zu filtern. Klar ist dabei sehr ineffizient. es kostet Netzwerk-Bandbreite und Verarbeitung Leistung auf dem Server Web API.

Eine Lösung kann ein URI-Schema wie _/orders/ordervalue_greater_than_n_ , wobei _n_ der Reihenfolge Preis ist, aber für alle bereitstellen; eine begrenzte Anzahl von Preisen ein solcher Ansatz ist unbrauchbar Darüber hinaus benötigen Sie Abfrage Aufträgen auf Grundlage anderer Kriterien, können Sie am Ende eine lange Liste von URIs möglicherweise nicht intuitiv verständliche Namen vor.

Eine bessere Strategie zum Filtern von Daten zu Web-API, wie an den Filterkriterien in der Abfragezeichenfolge ist _/orders?ordervaluethreshold n =_. In diesem Beispiel ist die entsprechende Operation im Web API verantwortlich für die Analyse und Behandlung der `ordervaluethreshold` Parameter in der Abfragezeichenfolge und gefilterte Ergebnisse in der HTTP-Antwort zurückgegeben.

Einige einfachen HTTP GET-Anfragen über Sammlung Ressourcen können möglicherweise eine große Anzahl von Elementen zurück. Gegen die Möglichkeit dieses auftreten sollten Web API um jede einzelne Anforderung zurückgegebene Datenmenge begrenzen entwerfen. Dies können Sie erreichen durch die Unterstützung von Abfragezeichenfolgen, die dem Benutzer ermöglichen, geben Sie die maximale Anzahl von Elementen abgerufen werden (die selbst eine Obergrenze begrenzt Denial-of-Service-Angriffe verhindern handeln), und ein Start-Offset in der Auflistung. Z. B. die Abfragezeichenfolge im URI _/orders?limit = 25 & Offset = 50_ 25 Aufträge mit dem 50. Reihenfolge in der Orders-Auflistung abrufen soll. Beim Filtern von Daten, die Operation, die die GET-Anforderung im Web API implementiert verantwortlich für die Analyse und Behandlung ist die `limit` und `offset` Parameter in der Abfragezeichenfolge. Unterstützung für Clientanwendungen erhalten Sie Anfragen, die paginierte Daten auch sollten Metadaten zurück, die die Gesamtzahl der in der Auflistung verfügbaren Ressourcen an. Sie sollten auch andere intelligente Paging Strategien; Weitere Informationen finden Sie unter [API-Design Notes: Smart Paging](http://bizcoder.com/api-design-notes-smart-paging)

Folgen Sie eine Strategie zum Sortieren von Daten als abgerufen wird. Sie könnten Sortierparameter, die wie ein Feld als Wert akzeptiert _/orders?sort ProductID =_. Jedoch dadurch zeitliche Zwischenspeichern (Abfrage Parameter Resource Identifier, mit vielen cacheimplementierungen als Schlüssel für zwischengespeicherte Daten Bestandteil) haben.

Die Felder dieser Ansatz Limit (Projekt) erweitern zurückgegeben, wenn ein einzelnes Ressourcenelement eine große Datenmenge enthält. Sie können z. B. einen Abfragezeichenfolgen-Parameter, der eine durch Kommas getrennte Liste der Felder, wie z. B. akzeptiert _/orders?fields ProductID, Menge =_.

> [AZURE.TIP] Geben Sie alle optionalen Parameter in Abfragezeichenfolgen sinnvolle Standardwerte. Beispielsweise die `limit` Parameter 10 und `offset` Parameter 0 Wenn Paginierung implementieren Sortierparameter für den Schlüssel der Ressource festgelegt, wenn Sie implementieren Sortierung und legen die `fields` Parameter für alle Felder in der Ressource Projektionen unterstützt.

### <a name="handling-large-binary-resources"></a>Umgang mit großen binären Ressourcen

Eine Ressource kann große binäre Felder wie Dateien oder Bilder enthalten. Die Übertragung zu Problemen durch unzuverlässige und unterbrochene Verbindung und zur Verbesserung der Antwortzeiten, sollten Sie Vorgänge, die diese Ressourcen in Blöcken von der Client-Anwendung abgerufen werden. Hierzu sollte Web API Accept-Ranges-Header für die GET-Anfragen für große Ressourcen unterstützen und im Idealfall implementieren HTTP HEAD-Anfragen für diese Ressourcen. Accept-Ranges-Header gibt an, dass die Abrufoperation Teilergebnisse unterstützt und eine Clientanwendung GET-Anfragen senden kann, die eine Teilmenge einer Ressource als eine Folge von Bytes zurückzugeben. Eine HEAD-Anforderung ähnelt einer GET-Anforderung es nur einen Header zurück, der die Ressource und einen leeren Nachrichtentext beschrieben. Eine Clientanwendung kann eine HEAD-Anforderung zu bestimmen, ob eine Ressource mit partiellen GET-Anfragen abrufen ausstellen. Das folgende Beispiel zeigt eine HEAD-Anforderung, die Informationen über ein Produktbild erhält:

```HTTP
HEAD http://adventure-works.com/products/10?fields=productImage HTTP/1.1
...
```

Die Antwortnachricht enthält einen Header, der die Ressource (4580 Bytes) umfassen und Accept-Ranges-Header, der entsprechende Ladevorgang Teilergebnisse unterstützt:

```HTTP
HTTP/1.1 200 OK
...
Accept-Ranges: bytes
Content-Type: image/jpeg
Content-Length: 4580
...
```

Diese Informationen können die Client-Anwendung eine Reihe von GET-Operationen zum Abrufen des Abbildes in kleineren Segmenten erstellen. Die erste Anforderung Ruft die ersten 2500 Bytes mit der Range-Header:

```HTTP
GET http://adventure-works.com/products/10?fields=productImage HTTP/1.1
Range: bytes=0-2499
...
```

Die Antwortnachricht kennzeichnet eine partielle Antwort vom HTTP-Statuscode 206 zurückgibt. Content-Length-Header gibt die tatsächliche Anzahl der Byte im Nachrichtentext (nicht die Größe der Ressource) und Content-Range-Header gibt an, welcher Teil der Ressource Dies ist (0-2499 von 4580 Bytes):

```HTTP
HTTP/1.1 206 Partial Content
...
Accept-Ranges: bytes
Content-Type: image/jpeg
Content-Length: 2500
Content-Range: bytes 0-2499/4580
...
_{binary data not shown}_
```

Eine nachfolgende Anforderung von der Clientanwendung kann den Rest der Ressource mit entsprechenden Bereichsheader abrufen:

```HTTP
GET http://adventure-works.com/products/10?fields=productImage HTTP/1.1
Range: bytes=2500-
...
```

Ergebnisnachricht sollte wie folgt aussehen:

```HTTP
HTTP/1.1 206 Partial Content
...
Accept-Ranges: bytes
Content-Type: image/jpeg
Content-Length: 2080
Content-Range: bytes 2500-4580/4580
...
```

## <a name="using-the-hateoas-approach-to-enable-navigation-to-related-resources"></a>Aktivieren der Navigation zu verwandten Ressourcen mithilfe HATEOAS Ansatz

Die primären Motivation hinter REST gehört, den gesamten Satz von Ressourcen zu navigieren, ohne vorherige das URI-Schema möglich ist. HTTP GET-Anforderung sollte Informationen notwendig, die Ressourcen direkt auf das angeforderte Objekt über Hyperlinks in der Antwort enthalten, und muss auch mit Informationen über die verfügbaren Vorgänge auf diese Ressourcen bereitgestellt werden. Dieses Prinzip wird als HATEOAS oder Hypertext als Modul Anwendungszustand bezeichnet. Das System ist einer endlichen Automaten und auf jede Anforderung enthält Informationen von einem Zustand in einen anderen verschieben; keine weiteren Informationen sollten erforderlich.

> [AZURE.NOTE] Aktuell sind keine Normen oder Spezifikationen, die HATEOAS-Prinzip Modell definieren. Die Beispiele in diesem Abschnitt veranschaulichen eine mögliche Lösung.

So behandeln Sie die Beziehung zwischen Customers und Orders sollte in der Antwort für eine bestimmte Bestellung zurückgegebenen Daten URIs in Form eines Hyperlinks identifizieren den Debitor, der die Bestellung und diesen Kunden Operationen.

```HTTP
GET http://adventure-works.com/orders/3 HTTP/1.1
Accept: application/json
...
```

Der Textkörper der Antwort enthält ein `links` Array (im Beispiel hervorgehoben), Beziehung (_Kunde_) den URI des Kunden (_http://adventure-works.com/customers/3_) angibt, wie die Details des Kunden (_erhalten_) und MIME-Typen, die der Webserver unterstützt Abrufen dieser Informationen (_Text/Xml_ und _Application/Json_) abgerufen. Dies sind alle Informationen, die eine Clientanwendung zu den Kunden abrufen. Außerdem enthält das Array Links Links für die Vorgänge, die ausgeführt werden können, z. B. gesetzt (der Kunde und der Webserver den Client bereitstellen erwartet Format ändern) und löschen.

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"orderID":3,"productID":2,"quantity":4,"orderValue":16.60,"links":[(some links omitted){"rel":"customer","href":" http://adventure-works.com/customers/3", "action":"GET","types":["text/xml","application/json"]},{"rel":"
customer","href":" http://adventure-works.com /customers/3", "action":"PUT","types":["application/x-www-form-urlencoded"]},{"rel":"customer","href":" http://adventure-works.com /customers/3","action":"DELETE","types":[]}]}
```

Der Vollständigkeit halber sollten Links Array auch selbstreferenzierende Informationen über die Ressource abgerufen wurde. Diese Links aus dem vorherigen Beispiel weggelassen wurden, aber im folgenden Code hervorgehoben. Beachten Sie, dass diese Links Beziehung _selbst_ weist dies auf die Ressource, die von dem Vorgang zurückgegebenen verwendet wurde:

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"orderID":3,"productID":2,"quantity":4,"orderValue":16.60,"links":[{"rel":"self","href":" http://adventure-works.com/orders/3", "action":"GET","types":["text/xml","application/json"]},{"rel":" self","href":" http://adventure-works.com /orders/3", "action":"PUT","types":["application/x-www-form-urlencoded"]},{"rel":"self","href":" http://adventure-works.com /orders/3", "action":"DELETE","types":[]},{"rel":"customer",
"href":" http://adventure-works.com /customers/3", "action":"GET","types":["text/xml","application/json"]},{"rel":" customer" (customer links omitted)}]}
```

Clientanwendungen müssen dafür wirksam abrufen und analysieren diese zusätzlichen Informationen ausgearbeitet.

## <a name="versioning-a-restful-web-api"></a>Versioning RESTful Web-API

Es ist sehr unwahrscheinlich, mit Ausnahme der einfachsten Situationen Web API statisch bleiben. Unternehmen neuen ändern Ressourcen möglicherweise hinzugefügt Beziehungen zwischen Ressourcen ändert und die Struktur der Daten in Ressourcen möglicherweise geändert. Aktualisieren einer Webs-API, um neue oder unterschiedliche Anforderungen ist ein relativ einfacher Prozess müssen Sie Effekte berücksichtigen, die eine solche Änderung für Clientanwendungen consuming Web API. Das Problem ist, dass zwar der Entwickler entwerfen und Implementieren einer Webs-API vollständige Kontrolle über diese API, Entwickler nicht denselben Grad an Kontrolle über Clientanwendungen, von Dritten Organisationen Remote erstellt werden kann. Primäre Imperative ist vorhandene Clientanwendungen auch gleichzeitig neue Kunden Nutzung der neuen Features und Ressourcen unverändert.

Versioning ermöglicht eine Web-API an den Funktionen und Ressourcen macht und eine Clientanwendung kann Anträge, die auf eine bestimmte Version eines Features oder Ressource. Die folgenden Abschnitte beschreiben verschiedene Ansätze, von die jedes seine eigenen Vorteile und Nachteile hat.

### <a name="no-versioning"></a>Keine Versionskontrolle

Dies ist der einfachste Ansatz und möglicherweise für einige interne APIs. Änderungen konnte als neue Ressourcen oder neue Links dargestellt werden.  Hinzufügen von Inhalt zu vorhandenen Ressourcen möglicherweise keine unterbrechende Änderung als Clientanwendungen vorhanden, die nicht erwarten, dass dieser Inhalt einfach ignoriert werden.

Beispielsweise sollte eine Anforderung an URI _http://adventure-works.com/customers/3_ zurückgeben einen einzelnen Kunden mit `id`, `name`, und `address` Felder von der Clientanwendung erwartet:

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"id":3,"name":"Contoso LLC","address":"1 Microsoft Way Redmond WA 98053"}
```

> [AZURE.NOTE] Für Einfachheit und Klarheit enthalten die in diesem Abschnitt dargestellten Beispiel Antworten nicht HATEOAS Links.

Wenn die `DateCreated` Feld ist der Kunde Ressource hinzugefügt und dann die Antwort sieht:

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"id":3,"name":"Contoso LLC","dateCreated":"2014-09-04T12:11:38.0376089Z","address":"1 Microsoft Way Redmond WA 98053"}
```

Vorhandene Clientanwendungen möglicherweise weiterhin ordnungsgemäß funktioniert, wenn sie können unbekannte Felder ignoriert, während neue Clientanwendungen behandeln dieses neue Feld entworfen werden können. Jedoch wenn radikalere Änderung des Schemas von Ressourcen (z. B. entfernen oder Umbenennen von Feldern) oder die Beziehung zwischen Ressourcen ändern stellen dann diese Änderungen, die verhindern, dass vorhandene Clientanwendungen nicht ordnungsgemäß funktioniert. In solchen Situationen sollten Sie eine der folgenden Methoden.

### <a name="uri-versioning"></a>URI-Versionen

Jedem Web API ändern oder ändern Sie das Schema von Ressourcen hinzufügen eine URI für jede Ressource. Bestehenden URIs sollten weiterhin wie zuvor ihre ursprünglichen Schema nach Ressourcen entsprechen.

Das vorherige Beispiel erweitert, wenn die `address` Feld in untergeordneten Felder mit jeder Bestandteil der Adresse neu (wie `streetAddress`, `city`, `state`, und `zipCode`), diese Version der Ressource durch einen URI mit einer Versionsnummer wie http://adventure-works.com/v2/customers/3 ausgesetzt sein:

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"id":3,"name":"Contoso LLC","dateCreated":"2014-09-04T12:11:38.0376089Z","address":{"streetAddress":"1 Microsoft Way","city":"Redmond","state":"WA","zipCode":98053}}
```

Dieser Mechanismus Versionen der Server die Anforderung an den entsprechenden Endpunkt routing hängt jedoch ist sehr einfach. Jedoch als Web API über mehrere Iterationen reift unhandlich werden und des Servers eine Anzahl verschiedener Versionen unterstützen. Auch aus Sicht des Puristen in allen Fällen die Clientanwendungen holen dieselben Daten (Kunde 3), so dass URI nicht wirklich je nach Version. Dieses Schema erschwert außerdem Implementierung von HATEOAS wie alle Links die Versionsnummer in den URIs enthalten müssen.

### <a name="query-string-versioning"></a>Abfrage Zeichenfolge Versionskontrolle

Anstatt mehrere URIs können Sie die Version der Ressource mit einem Parameter in der Abfragezeichenfolge an die HTTP-Anforderung wie _http://adventure-works.com/customers/3?version=2_angefügt. Version-Parameter sollte einen sinnvollen Wert wie 1 standardmäßig ältere Clientanwendungen fehlt.

Dieser Ansatz hat den semantischen Vorteil hängt Code, der die Anforderung verarbeitet an die Abfragezeichenfolge analysieren und die entsprechende HTTP-Antwort zurücksenden, die dieselbe Ressource wird immer vom gleichen URI abgerufen. Dieser Ansatz leidet gleichen Komplikationen für HATEOAS als URI Versioningmechanismus implementieren.

> [AZURE.NOTE] Einige ältere Webbrowser und Webproxys werden Antworten auf Anfragen, die in der URL eine Abfragezeichenfolge enthalten nicht zwischengespeichert. Dadurch können negativ auf die Leistung von ASP.NET-Webanwendungen Web API verwenden und aus dieser Webbrowser ausgeführt.

### <a name="header-versioning"></a>Header-Versionen

Die Versionsnummer als Abfrageparameter angehängt, könnten Sie einen benutzerdefinierten Header implementieren, der die Version der Ressource angibt. Dieser Ansatz erfordert, dass die Clientanwendung alle Anfragen den entsprechenden Header hinzufügt zwar der Code für die Clientanforderung Standardwert (Version 1) konnte den Versionsheader fehlt. Die folgenden Beispiele verwenden einen benutzerdefinierten Header mit dem Namen _Benutzerdefinierte Header_. Der Wert dieses Headers gibt die Version der Web-API.

Version 1:

```HTTP
GET http://adventure-works.com/customers/3 HTTP/1.1
...
Custom-Header: api-version=1
...
```

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"id":3,"name":"Contoso LLC","address":"1 Microsoft Way Redmond WA 98053"}
```

Version 2:

```HTTP
GET http://adventure-works.com/customers/3 HTTP/1.1
...
Custom-Header: api-version=2
...
```

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"id":3,"name":"Contoso LLC","dateCreated":"2014-09-04T12:11:38.0376089Z","address":{"streetAddress":"1 Microsoft Way","city":"Redmond","state":"WA","zipCode":98053}}
```

Beachten Sie, dass wie bei den vorherigen beiden Ansätzen HATEOAS implementieren einschließlich des entsprechenden benutzerdefinierten Headers in Links muss.

### <a name="media-type-versioning"></a>Medientyp Versionskontrolle

Wenn eine Clientanwendung eine HTTP GET-Anforderung an einen Webserver sendet das Format des Inhalts festlegen sollten, die sie weiter oben in diesem Handbuch beschriebenen Accept-Header mit behandeln können. Häufig soll der _Accept_ -Header ermöglichen die Clientanwendung an, ob die Antwort XML, JSON oder einem anderen Client analysieren kann allgemeine Format werden soll. Es ist jedoch möglich, benutzerdefinierte Medientypen definieren, die Informationen ermöglichen die Clientanwendung an, welche Version einer Ressource erwartet wird. Das folgende Beispiel zeigt eine Anforderung, die _Accept_ -Header mit dem Wert _application/vnd.adventure-works.v1+json_angibt. _Vnd.adventure-works.v1_ -Element gibt an den Webserver zurück es sollten Version 1 der Ressource während das _Json_ -Element gibt an, dass das Format des Antworttexts JSON sollte:

```HTTP
GET http://adventure-works.com/customers/3 HTTP/1.1
...
Accept: application/vnd.adventure-works.v1+json
...
```

Der Code für die Anforderung ist verantwortlich für die Verarbeitung des _Accept_ -Headers und möglichst (die Clientanwendung kann mehrere Formate im _Accept_ -Header, in dem Fall der Webserver das am besten geeignete Format für den Antworttext kann angeben) berücksichtigt. Der Webserver wird bestätigt, dass das Format der Daten im Antworttext mit dem Content-Type-Header:

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/vnd.adventure-works.v1+json; charset=utf-8
...
Content-Length: ...
{"id":3,"name":"Contoso LLC","address":"1 Microsoft Way Redmond WA 98053"}
```

Wenn Accept-Header nicht bekannte Medientypen angeben, konnte der Webserver generieren eine Antwortnachricht HTTP 406 (nicht akzeptabel) oder eine Nachricht mit einer Standard-Medientyp.

Dieser Ansatz ist wohl das reinste Versioning Mechanismen und eignet sich natürlich für HATEOAS der MIME-Typ der Daten in Ressourcen-Links enthalten kann.

> [AZURE.NOTE] Beim Auswählen einer Strategie für die Versionskontrolle sollten Auswirkungen auf die Performance, Sie besonders auf dem Webserver Zwischenspeichern. Versioning URI und Abfragezeichenfolge Versionsschemata sind Cache als auf die gleichen Daten jedes Mal dieselbe Kombination von URI-Abfrage Zeichenfolge verweist.

> Header Versioning und Medientyp Versioning Mechanismen erfordern in der Regel zusätzliche Logik zu den Werten des benutzerdefinierten Headers oder Accept-Header. In einer großen Umgebung können viele Clients mit anderen Versionen von Web-API viel duplizierten Daten in einem serverseitigen Cache führen. Dieses Problem kann akut, wenn eine Clientanwendung mit einem Webserver über ein Proxy kommuniziert implementiert Zwischenspeichern und, nur eine Anforderung an den Webserver weiterleitet, wenn er nicht gerade eine Kopie der angeforderten Daten im Cache enthält werden.

## <a name="more-information"></a>Weitere Informationen

- [Rest Kochbuch](http://restcookbook.com/) enthält eine Einführung in Rest APIs erstellen.
- Web [API-Checkliste](https://mathieu.fenniak.net/the-api-checklist/) enthält eine nützliche Liste Aspekte beim Entwerfen und Implementieren einer Web-API.
