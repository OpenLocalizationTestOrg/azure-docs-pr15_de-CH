<properties
    pageTitle="Zum Einrichten der Business-Glossar für tagging geregelt | Microsoft Azure"
    description="Artikel hervorheben Business Glossar in Azure Data Catalog definieren und verwenden ein gemeinsame Vokabular Tag registrierten Daten."
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

# <a name="how-to-set-up-the-business-glossary-for-governed-tagging"></a>Zum Einrichten der Business-Glossar für gesteuert

## <a name="introduction"></a>Einführung

Azure Data Catalog bietet Funktionen zur Erkennung von Quelle, Benutzer leicht erkennen und Verstehen der Datenquellen müssen sie analysieren und entscheiden. Diese Discovery-Funktionen stellen die größte Auswirkung Wenn Benutzer finden und die breiteste Palette an verfügbaren Datenquellen verstehen.

Ein Data Catalog-Feature, das Verständnis der Anlagen Daten fördert tagging. Tagging ermöglicht Benutzern Schlüsselwörter Zuordnen eines Vermögenswertes oder einer Spalte, die wiederum die Anlage über Suchen entdecken erleichtert, und ermöglicht den Kontext und die Absicht des Vermögenswertes leichter verstehen.

Jedoch kann Tags auch eigene Probleme. Beispiele für Probleme, die durch tagging eingeführt werden können sind:

1.  Benutzer Abkürzungen Anlagen und erweiterten Text auf andere während tagging. Diese Inkonsistenz verhindert die Suche nach Ressourcen, auch wenn beabsichtigt ist, die Elemente mit demselben Tag markieren.
2.  Tags in verschiedenen Kontexten unterschiedliche Bedeutung. Beispielsweise ein Tag namens "Einnahmen" auf einen Kunden bedeutet Einnahmen pro Kunde jedoch das gleiche Tag für einen vierteljährlichen sales Dataset könnte Umsatz für das Unternehmen.  

Data Catalog enthält ein Glossar Business erleichtern diese und andere ähnliche Probleme beheben.

Daten Katalog Business Glossar ermöglicht Organisationen Dokument wichtigsten Begriffe und ihrer Definitionen ein gemeinsame Vokabular erstellen. Diese Steuerung ermöglicht Konsistenz bei Daten in der gesamten Organisation. Nach Business-Glossar und Begriffe zugewiesen werden mit Daten in den Katalog mit dem gleichen Ansatz als auszeichnen, damit _tagging geregelt_.

> [AZURE.NOTE] In diesem Artikel beschriebene Funktionen stehen nur in der Standard Edition von Azure Data Catalog. Die kostenlose Version bietet keine Funktionen für gesteuerten tagging oder eine Business-Glossar.

## <a name="glossary-availability-and-privileges"></a>Glossar-Verfügbarkeit und Berechtigungen

*Business-Glossar steht in der Standard Edition von Azure Data Catalog. Kostenlose Ausgabe des Data Catalog schließt kein Glossar.*

Business-Glossar erfolgt über die Option "Glossar" im Navigationsmenü Data Catalog-Portal.  

![Zugriff auf Business-Glossar](./media/data-catalog-how-to-business-glossary/01-portal-menu.png)


Datenadministratoren Katalog können Mitglieder der Systemadministratorrolle Glossar erstellen, bearbeiten und Glossar Glossary Business löschen. Alle Data Catalog Benutzer sehen die Begriffsdefinitionen und können mit Glossar markieren.

![Hinzufügen einer neuen Glossareintrag](./media/data-catalog-how-to-business-glossary/02-new-term.png)


## <a name="creating-glossary-terms"></a>Glossar erstellen

Daten Katalog und Glossar Administratoren neue Glossar auf den neuen Begriff erstellen ', um mit den folgenden Feldern Glossar zu erstellen:

* Eine Business-Definition für den Begriff
* Eine Beschreibung, die beabsichtigten Gebrauch oder Geschäftsregeln für die Anlage/Spalte erfasst
* Eine Liste der Beteiligten den Begriff kennen
* Übergeordneter Ausdruck definiert die Hierarchie der Begriff organisiert ist


## <a name="glossary-term-hierarchies"></a>Glossar Begriff Hierarchien

Glossar Business Data Catalog bietet die Möglichkeit, Ihr Vokabular als Hierarchie von Begriffen beschrieben. Dies ermöglicht eine Klassifizierung von Begriffen erstellen, die ihre Business-Taxonomie besser darstellt.

Der Name eines Begriffs muss auf einer bestimmten Ebene der Hierarchie - doppelte Namen sind nicht zulässig. Es gibt keine Beschränkung für die Anzahl der Ebenen in einer Hierarchie, aber hierarchisch oft leichter verständlich, wenn drei weniger oder.

Die Verwendung von Hierarchien im Glossar Business ist optional. Verlassen der übergeordneten Begriff Feld leer Glossar erstellt eine flache (hierarchische) Liste der Begriffe im Glossar.  

## <a name="tagging-assets-with-glossary-terms"></a>Kennzeichnung mit Glossar

Nachdem Glossar im Katalog definiert wurde, ist Erfahrung Tags Anlagen optimiert Glossar Benutzertypen ihren Tag suchen. Data Catalog-Portal wird eine Liste der übereinstimmenden Glossarbegriffe für den Benutzer aus. Wählt der Benutzer einen Glossarbegriff aus der Liste wird die Anlage als Tag (so genanntes hinzugefügt Glossar-Tag). Benutzer können auch durch Eingabe einen Begriff ist nicht im Glossar (auch bekannt als ein neues Tag erstellen Benutzertag).

![Datenressource mit einem Benutzertag und zwei Glossar Tags markiert](./media/data-catalog-how-to-business-glossary/03-tagged-asset.png)

> [AZURE.NOTE] Benutzer-Tags sind die einzigen Tag im freien Edition des Daten-Katalog unterstützt.

### <a name="hover-behavior-on-tags"></a>Hover-Verhalten für tags
Im Portal Datenkatalog sind zwei Typen von Tags visuell mit verschiedenen Hover-Verhalten. Wenn der Benutzer über ein Benutzertag zeigt sehen sie den Kennzeichnungstext und den oder die Benutzer, die das Tag hinzugefügt haben. Wenn Benutzer über ein Glossar Tag bewegt, sehen sie auch die Definition der Glossarbegriff und eine Verknüpfung zu Business Glossar zum Anzeigen der vollständigen Definition des Begriffs öffnen.

### <a name="search-filters-for-tags"></a>Suchfilter für tags
Glossar-Tags und Benutzer Tags durchsucht werden, und können als Filter in einer Suche angewendet werden.

## <a name="summary"></a>Zusammenfassung
Business Glossar in Azure Data Catalog und der gesteuerten tagging ermöglicht, können Datenbestände erkannt, verwaltet und konsistent entdeckt. Business-Glossar kann lernen von Vokabeln geschäftliche Benutzer einer Organisation heraufstufen und unterstützt sinnvolle Metadaten erfasst werden, Anlage Entdeckung und Grundlegendes zu einem Kinderspiel.

## <a name="see-also"></a>Siehe auch

- [REST-API-Dokumentation für Geschäftsvorgänge Glossar](https://msdn.microsoft.com/library/mt708855.aspx)
