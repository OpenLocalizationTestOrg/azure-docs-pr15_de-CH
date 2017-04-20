<properties
   pageTitle="Verwalten von Daten | Microsoft Azure"
   description="Artikel hervorheben steuern Sichtbarkeit und an Daten in Azure Data Catalog registriert."
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
   ms.date="10/04/2016"
   ms.author="maroche"/>


# <a name="how-to-manage-data-assets"></a>Verwalten von Daten

## <a name="introduction"></a>Einführung

**Azure Data Catalog** bietet Funktionen zur Erkennung von Quelle, Benutzer leicht erkennen und Verstehen der Datenquellen müssen sie analysieren und entscheiden. Diese Discovery-Funktionen stellen die größte Wirkung bei allen Benutzern finden und die breiteste Palette an verfügbaren Datenquellen verstehen. Dabei das Standardverhalten des Data Catalog sichtbar, und von werden alle registrierten Datenquellen – alle Benutzer des Katalogs.

Data Catalog hat keine Benutzer auf die Daten Zugriff. Der Besitzer der Datenquelle Daten gesteuert. Data Catalog kann Benutzer Daten Quellen und die Metadaten im Katalog registrierten Quellen anzeigen.

Möglicherweise gibt es Situationen, in denen Datenquellen nur bestimmte Benutzer oder Mitglieder bestimmter Gruppen sichtbar ist. Für diese Szenarien Datenkatalog ermöglicht die Ressourcen innerhalb des Katalogs erfassten Daten übernehmen und dann steuern die Sichtbarkeit der Anlagen besitzen.

> [AZURE.NOTE] In diesem Artikel beschriebene Funktionen stehen nur in der Standard Edition von Azure Data Catalog. Die kostenlose Version bietet keine Funktionen für den Besitz und Einschränkung Daten Anlage Sichtbarkeit.

## <a name="managing-ownership-of-data-assets"></a>Verwalten von Daten an
Standardmäßig sind Datenbestände in Data Catalog registriert ohne Besitzer Jeder Benutzer mit Zugriff auf den Katalog kann erkennen und kommentieren diese Ressourcen. Benutzer Daten übernehmen und können dann die Sichtbarkeit der Ressourcen, die sie besitzen.

Wenn eine Datenressource Datenkatalog gehört, nur Benutzern der Inhaber die Anlage entdecken und seine Metadaten anzeigen und nur die Besitzer können die Anlage aus dem Katalog löschen.

> [AZURE.NOTE] An Data Catalog betrifft nur die Metadaten im Katalog gespeichert. Dies verleiht nicht Berechtigungen für die zugrunde liegende Datenquelle.

### <a name="taking-ownership"></a>Besitz
Benutzer können Daten übernehmen durch Auswahl der Option "Besitzrechte" in Data Catalog-Portal. Besonderen Berechtigungen müssen Anlage Daten übernehmen. Jeder Benutzer kann eine Anlage Daten übernehmen.

### <a name="adding-owners-and-co-owners"></a>Hinzufügen von Besitzern und Eigentümer
Wenn eine Datenressource bereits gehört, können einfach Benutzer den Besitz – als Eigentümer eines vorhandenen Besitzers hinzugefügt werden. Alle Besitzer kann als Eigentümer zusätzliche Benutzer oder Gruppen hinzufügen.

> [AZURE.NOTE] Es wird empfohlen, mindestens zwei Personen als Besitzer für eine Anlage besitzt Daten.

### <a name="removing-owners"></a>Entfernen von Besitzern
Wie alle verantwortlichen Mitbesitzer hinzufügen kann, kann alle verantwortlichen alle Mitbesitzer entfernen.

Ein Ressourceneigentümer selbst als Besitzer entfernt, kann er nicht mehr Anlage verwalten. Wenn ein Ressourceneigentümer sich selbst als Besitzer entfernt gibt es keine gemeinsame Besitzer, wird die Anlage in deaktiviertem Zustand zurückgesetzt.

## <a name="visibility"></a>Sichtbarkeit
Eigentümer Daten steuern die Sichtbarkeit der Datenbestände, die sie besitzen. Sichtbarkeit von Standard-beschränkt kann, in dem alle Data Catalog entdecken und zeigen die Datenressource – Verantwortlichen die sichtbarkeitseinstellung "Jeder", "Eigentümer & Diese Benutzer" in den Eigenschaften der Anlage umschalten. Besitzer können Sie bestimmten Benutzern und Sicherheitsgruppen hinzufügen.

> [AZURE.NOTE] Nach Möglichkeit sollte Anlage Verantwortlichkeit und Präsenz Berechtigungen Sicherheitsgruppen und nicht einzelnen Benutzern zugewiesen werden.

## <a name="catalog-administrators"></a>Katalogadministratoren
Katalog Datenadministratoren sind implizit Mitbesitzer des Gesamtvermögens im Katalog. Eigentümer nicht Sichtbarkeit aus Katalogadministratoren entfernen und Administratoren können Besitzrechte und Sichtbarkeit für alle Datenbestände im Katalog verwalten.

## <a name="summary"></a>Zusammenfassung
Metadaten und Asset Discovery Data Catalog Crowdsourcing Modell kann alle Katalog zu tragen. Der Standard Edition von Data Catalog bietet Funktionen für den Besitz und Verwaltung der Sichtbarkeit und Verwendung von bestimmten Daten.
