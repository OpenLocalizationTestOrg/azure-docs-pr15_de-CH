<properties
    pageTitle="Computergruppen Protokollanalyse anmelden suchen | Microsoft Azure"
    description="Computergruppen Protokollanalyse können Sie Protokolldateien Suchbereiche für eine bestimmte Gruppe von Computern.  Dieser Artikel beschreibt verschiedene Methoden, mit Computergruppen und deren Verwendung in einer protokollsuche erstellen."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="bwren"/>

# <a name="computer-groups-in-log-analytics-log-searches"></a>Computergruppen Protokollanalyse anmelden suchen
Computergruppen Protokollanalyse können Bereich mit einer bestimmten Gruppe von Computern [sucht anmelden](log-analytics-log-searches.md) .  Jede Gruppe wird aufgefüllt mit Computern mit einer Abfrage, die Sie definieren oder Gruppen aus verschiedenen Quellen importieren.  Wenn die Gruppe eine Protokoll-Suche enthalten ist, sind die Ergebnisse auf Datensätze, die Computer in der Gruppe entsprechen.

## <a name="creating-a-computer-group"></a>Erstellen einer Computergruppe
Sie können eine Computergruppe in Protokollanalyse mithilfe einer der Methoden in der folgenden Tabelle erstellen.  In den folgenden Abschnitten werden Details zu jeder Methode bereitgestellt. 

| -Methode | Beschreibung |
|:---|:---|
| Protokolldatei suchen       | Erstellen Sie eine protokollsuche, die eine von Computern Liste und speichern Sie die Ergebnisse als eine Gruppe. |
| Protokoll-Such-API   | Verwenden Sie die Protokoll-Such-API programmgesteuert erstellen eine Computergruppe basierend auf den Ergebnissen einer Protokoll-Suche. |
| Active Directory | Die Gruppenmitgliedschaft alle Agent-Computer, die Mitglieder einer Active Directory-Domäne und erstellen Sie eine Gruppe für jede Sicherheitsgruppe in Protokollanalyse automatisch überprüft.
| WSUS              | Automatisch Scannen von WSUS-Servern oder Clients für Zielgruppen und erstellen Sie eine Gruppe für jede in Protokollanalyse. |


### <a name="log-search"></a>Protokolldatei suchen

Erstellt ein Protokoll Suche Computergruppen enthalten alle Computer zurückgegebene eine Suchabfrage definieren.  Diese Abfrage wird jedes Mal ausgeführt Computergruppe verwendet, sodass Änderungen seit die Gruppe übernommen.

Gehen Sie zu einer Computergruppe aus einer Protokolldatei suchen.

1. [Erstellen einer Protokolldatei suchen](log-analytics-log-searches.md) , die eine Liste von Computern zurückgegeben.  Die Suche muss eine bestimmten Gruppe von Computern mit etwas wie **Verschiedene Computer** oder **Measure count() Computer** in der Abfrage zurück.  
2. Klicken Sie am oberen Rand des Bildschirms auf die Schaltfläche **Speichern** .
3. Wählen Sie **Ja** , **dieser Abfrage als eine Gruppe speichern:**.
4. Geben Sie einen **Namen** und eine **Kategorie** für die Gruppe ein.  Wenn eine Suche mit demselben Namen und derselben Kategorie bereits vorhanden ist, werden Sie aufgefordert zu überschreiben.  Sie können mehrere Suchvorgänge mit demselben Namen in verschiedenen Kategorien. 

Folgen Beispiel suchen, die Sie als eine Gruppe speichern können.

    Computer="Computer1" OR Computer="Computer2" | distinct Computer 
    Computer=*srv* | measure count() by Computer

### <a name="log-search-api"></a>Protokoll-Such-API

Computergruppen mit Protokoll Such-API erstellt entsprechen Protokoll suchen erweiterte Suche.

Ausführliche Informationen zum Erstellen einer Computergruppe mit Protokoll Such-API finden Sie unter [Computergruppen Protokollanalyse anmelden Suche REST-API](log-analytics-log-search-api.md#computer-groups).

### <a name="active-directory"></a>Active Directory

Beim Konfigurieren der Protokollanalyse Gruppenmitgliedschaften Active Directory importieren analysiert er die Mitglieder aus jeder Domäne verbundene Computer mit der OMS.  Eine Computergruppe in Protokollanalyse für jede Sicherheitsgruppe in Active Directory erstellt und jedem Computer zu Sicherheitsgruppen sind Mitglieder der entsprechenden Computergruppen hinzugefügt.  Alle 4 Stunden Mitgliedschaft fortlaufend aktualisiert.  

Protokollanalyse zum Importieren von Active Directory-Sicherheitsgruppen aus **Computergruppen** Protokollanalyse **Einstellungen**konfigurieren.  Wählen Sie **Automatisierung** und **Gruppenmitgliedschaften von Computern aus Active Directory importieren**.  Es ist keine weitere Konfiguration erforderlich.

![Computergruppen aus Active Directory](media/log-analytics-computer-groups/configure-activedirectory.png)

Beim Importieren von Gruppen wird die Anzahl der Computer mit der Gruppenmitgliedschaft erkannt und die Anzahl der Gruppen importiert im Menü aufgelistet.  Sie können auf diese Links **ComputerGroup-Objekts** Datensätze mit diesen Informationen zurückgeben klicken.

### <a name="windows-server-update-service"></a>Windows Server Update Service

Beim Konfigurieren der Protokollanalyse Gruppenmitgliedschaften WSUS importieren analysiert er auf Gruppenmitgliedschaft der Computer mit dem OMS-Agent.  Verwenden Sie clientseitige haben für Computer, die mit OMS und Teil jeder WSUS Zielgruppen der Gruppenmitgliedschaft Protokollanalyse importiert. Verwenden Sie serverseitige sollte auf die OMS Agent auf dem WSUS-Server in der Reihenfolge für die Gruppenmitgliedschaftsinformationen in OMS importiert installiert werden.  Alle 4 Stunden Mitgliedschaft fortlaufend aktualisiert. 

Protokollanalyse zum Importieren von Active Directory-Sicherheitsgruppen aus **Computergruppen** Protokollanalyse **Einstellungen**konfigurieren.  Wählen Sie **Active Directory** und **Gruppenmitgliedschaften von Computern aus Active Directory importieren**.  Es ist keine weitere Konfiguration erforderlich.

![Computergruppen aus Active Directory](media/log-analytics-computer-groups/configure-wsus.png)

Beim Importieren von Gruppen wird die Anzahl der Computer mit der Gruppenmitgliedschaft erkannt und die Anzahl der Gruppen importiert im Menü aufgelistet.  Sie können auf diese Links **ComputerGroup-Objekts** Datensätze mit diesen Informationen zurückgeben klicken.

## <a name="managing-computer-groups"></a>Verwalten von Computergruppen

Sie können Computergruppen anzeigen, die eine Protokolldatei oder Protokoll Such-API, Menü **Computergruppen** Protokollanalyse **Einstellungen**erstellt wurden.  Klicken Sie auf das **X** in der Spalte **Entfernen** die Computergruppe löschen.  Klicken Sie auf **Mitglieder anzeigen** , für eine Gruppe der Gruppe protokollsuche ausführen, die die Member zurückgibt. 

![Gespeicherte Computergruppen](media/log-analytics-computer-groups/configure-saved.png)

Um die Gruppe zu ändern, erstellen Sie eine neue Gruppe mit der gleichen **Kategorie** und dem **Namen** die ursprüngliche Gruppe überschreiben.

## <a name="using-a-computer-group-in-a-log-search"></a>Eine Computergruppe verwenden in einem Protokoll suchen
Sie verwenden die folgende Syntax auf eine Computergruppe in einem Protokoll suchen.  Die Angabe der **Kategorie** ist optional und nur erforderlich haben Sie Computergruppen mit demselben Namen in unterschiedlichen Kategorien. 

    $ComputerGroups[Category: Name]

Wenn eine Suche ausgeführt wird, werden die Mitglieder von Computergruppen in der Suche enthalten zuerst aufgelöst.  Die Gruppe Anmelden Suche basiert, wird die Suche ausgeführt, um die Mitglieder der Gruppe zurückgegeben werden, bevor der obersten Ebene Protokoll Suche.

Computergruppen werden normalerweise **IN** -Klausel in der Protokolldatei suchen wie im folgenden Beispiel verwendet.

    Type=UpdateSummary Computer IN $ComputerGroups[My Computer Group]

## <a name="computer-group-records"></a>Computer gruppieren

Ein Datensatz ist im Repository OMS für jeden Computergruppenmitgliedschaft aus Active Directory oder WSUS erstellt erstellt.  Diese Einträge haben ein **ComputerGroup-Objekts** und Eigenschaften in der folgenden Tabelle.  Für Computergruppen basierend auf Protokoll suchen werden keine Datensätze erstellt.

| Eigenschaft | Beschreibung |
|:--|:--|
| Typ                | *ComputerGroup-Objekts* |
| SourceSystem        | *SourceSystem*  |
| Computer            | Name der Mitgliedscomputer. |
| Gruppe               | Name der Gruppe. |
| GroupFullName       | Vollständige Pfad einschließlich der Quelle und dem Quellnamen.
| GroupSource         | Gruppe Quelle gesammelt wurde. <br><br>Active Directory<br>WSUS<br>WSUSClientTargeting |
| GroupSourceName     | Name der Datenquelle, die von Gruppen erfasst wurde.  Active Directory ist dies der Domänenname. |
| "Verwaltungsgruppenname" | Name der Verwaltungsgruppe für SCOM-Agenten.  Für andere Agenten ist AOI -\<Arbeitsplatz-ID\> |
| TimeGenerated       | Datum und Uhrzeit die Computergruppe erstellt oder aktualisiert wurde. |



## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Protokoll suchen](log-analytics-log-searches.md) Daten aus Datenquellen und Solutions analysieren.  