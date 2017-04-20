<properties
   pageTitle="Wie Datenquellen kommentieren | Microsoft Azure"
   description="Gewusst wie-Artikel hervorheben wie Datenbestände in Azure Data Catalog, einschließlich Namen, Tags, Beschreibung und Experten kommentieren."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/21/2016"
   ms.author="maroche"/>


# <a name="how-to-annotate-data-sources"></a>Wie Datenquellen kommentieren

## <a name="introduction"></a>Einführung
**Microsoft Azure Data Catalog** ist eine vollständig verwaltete Cloud-Dienst, der als ein System der Registrierung und Entdeckung für Enterprise-Datenquellen fungiert. Also darum Datenkatalog Menschen entdecken, verstehen und Verwenden von Datenquellen und helfen Unternehmen mehr Nutzen aus vorhandenen Daten. Beim Registrieren einer Datenquelle in Data Catalog seine Metadaten kopiert und vom Dienst indiziert, aber die Geschichte nicht zu Ende. Data Catalog kann Benutzer eigene beschreibende Metadaten-Tags – Ergänzung die Metadaten aus der Datenquelle extrahiert und die Datenquelle zu mehr Aussagekraft wie beschrieben.

## <a name="annotation-and-crowdsourcing"></a>Anmerkung und crowdsourcing
Jeder hat eine Meinung. Und das ist eine gute Sache.
Data Catalog erkennt haben unterschiedliche Benutzer unterschiedliche Perspektiven auf Enterprise-Datenquellen und jede dieser Perspektiven wertvoll erweisen kann. Szenario:

* Der Systemadministrator weiß die Vereinbarung zum Servicelevel für die Server und Dienste, hosten Datenquelle.
* Der Datenbankadministrator kennt den Sicherungszeitplan für jede Datenbank und Windows zulässigen ETL-Verarbeitung.
* Der Besitzer des Systems kennen den Benutzern Zugriff auf die Datenquelle anzufordern.
* Daten-Verwalter weiß unternehmensdatenmodell wie Elemente und Attribute in der Datenquelle zugeordnet.
* Die Analyst weiß, wie die Daten im Rahmen der Geschäftsprozesse verwendet werden, die er unterstützt.

Jede dieser Perspektiven wertvolle und Data Catalog einen Crowdsourcing-Ansatz für Metadaten, die einzeln erfasst und verwendet, um ein vollständiges Bild der registrierten Datenquellen ermöglicht. Mit Data Catalog-Portal, jeden Benutzer hinzufügen und bearbeiten eigene Kommentare können von anderen Benutzern bereitgestellten anzeigen.

## <a name="different-types-of-annotations"></a>Arten von Notizen
Data Catalog unterstützt folgende Anmerkung:

| Anmerkung     | Notizen                                                                                                                                                                                                                                                                                                                                                           |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Angezeigter name  | Anzeigenamen können Ebene Anlage Daten leichter verständlich Datenbestände zu angegeben werden. Anzeigenamen sind besonders hilfreich, wenn zugrunde liegende Objektname unverständliche, abgekürzt oder andernfalls nicht für Benutzer sinnvoll ist.                                                                                                                            |
| Beschreibung    | Beschreibung zu Datenressource Attribut angegeben werden / Spalte Ebenen. Beschreibung sind kurze Freitext Kommentare, die des Benutzers beschreiben Perspektive auf Daten Anlage oder deren Nutzung.                                                                                                                                                              |
| Tags (Benutzer-Tags)          | Tags zu Datenressource Attribut angegeben werden / Spalte Ebenen. Benutzer-Tags sind benutzerdefinierte Etiketten mit Daten oder Attribute kategorisieren.                                                                                                                                                                                                    |
| Tags (Glossar Tags)          | Tags zu Datenressource Attribut angegeben werden / Spalte Ebenen. Glossar-Tags sind zentral definierte Glossarbegriffe, die zum Kategorisieren von Daten oder Attribute mit common Business Taxonomie verwendet werden können. Weitere Informationen finden Sie unter [Business Glossar für geregelt einrichten](data-catalog-how-to-business-glossary.md)                                                                                                                                                                                                    |
| Experten        | Experten können auf Datenebene Anlage angegeben werden. Experten Identifizieren von Benutzern oder Gruppen mit Experten der Daten dienen als Benutzer, die registrierten Datenquellen entdecken und vorhandene Kommentare nicht beantwortet Fragen.  |
| Zugriff beantragen | Zugriff auf Informationen auf Datenebene Anlage lieferbar. Diese Informationen sind für Benutzer, die eine Datenquelle zu ermitteln, die sie noch nicht über Berechtigungen zum Zugreifen auf verfügen. Benutzer können die e-Mail-Adresse des Benutzers oder der Gruppe gewährt, die URL des Prozesses oder Tool Benutzer Zugriff benötigen, oder den Prozess selbst als Text eingeben. |
| Dokumentation | Dokumentation kann auf Datenebene Anlage angegeben werden. Anlage-Dokumentation ist rich-Text-Informationen, Links und Bilder enthalten kann, und die können alle Informationen nicht durch eine Beschreibung und Tags. |


## <a name="annotating-multiple-assets"></a>Mehrere Anlagen hinzufügen
Bei Auswahl mehrerer Datenbestände in Data Catalog-Portal können Benutzer alle ausgewählte Anlagen in einem einzigen Vorgang versehen. Kommentare gelten für alle ausgewählten Anlagen auswählen und eine konsistente und Sätze von Tags und Experten für verwandte Daten erleichtern.

> [AZURE.NOTE] Tags und Experten können auch beim Registrieren Datenbestände mit Daten Katalogdaten Registrierungstool Datenquelle bereitgestellt werden.

Beim Auswählen mehrerer Tabellen und Ansichten nur Spalten, alle Daten ausgewählt, die Ressourcen gemeinsam haben im Data Catalog-Portal erscheint. Dies ermöglicht Benutzern, Tags und Beschreibungen für alle Spalten mit demselben Namen für alle ausgewählten Anlagen bereitzustellen.

## <a name="annotations-and-discovery"></a>Kommentare und Erkennung
Wie Data Catalog-Suchindex Metadaten aus der Datenquelle extrahiert werden, während der Registrierung hinzugefügt wird, wird auch benutzerdefinierte Metadaten indiziert. Dies bedeutet, dass nicht nur Kommentare erleichtert Benutzern, die Daten, die sie entdecken, Kommentare auch erleichtern Benutzern zu kommentierte Datenbestände suchen die Begriffe, die Ihnen sinnvoll.

## <a name="summary"></a>Zusammenfassung
Registrieren einer Datenquelle mit Datenkatalog macht die Daten strukturellen und beschreibenden Metadaten aus der Datenquelle in den Katalogdienst kopiert erkennbar. Nach der Registrierung einer Datenquelle können Benutzer Kommentare leichter erkennen und Verstehen von Data Catalog-Portal bereitstellen.

## <a name="see-also"></a>Siehe auch
- [Erste Schritte mit Azure Data Catalog](data-catalog-get-started.md) -Lernprogramm schrittweise Informationen zur Kommentierung von Datenquellen.
