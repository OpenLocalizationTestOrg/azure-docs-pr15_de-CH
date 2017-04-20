<properties
   pageTitle="Erste Schritte mit SQL-Datenbank dynamische Daten maskieren (Azure-Portal)"
   description="Erste Schritte mit SQL-Datenbank dynamische Daten Masking in Azure-Portal"
   services="sql-database"
   documentationCenter=""
   authors="ronitr"
   manager="jhubbard"
   editor="v-romcal"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/10/2016"
   ms.author="ronitr; ronmat; v-romcal; sstein"/>


# <a name="get-started-with-sql-database-dynamic-data-masking-azure-portal"></a>Erste Schritte mit SQL-Datenbank dynamische Daten maskieren (Azure-Portal)

> [AZURE.SELECTOR]
- [Dynamic Data-Masking - Azure-Verwaltungsportal](sql-database-dynamic-data-masking-get-started-portal.md)

## <a name="overview"></a>Übersicht

SQL Datenbank dynamische Daten Masking vertrauliche Daten von nicht berechtigten Benutzern masking beschränkt. Dynamische datenmaskierung wird Version V12 Azure SQL-Datenbank unterstützt.

Dynamische Daten maskieren kann verhindert unberechtigte Zugriffe auf vertrauliche Daten von Kunden wie viel sensiblen Daten mit minimaler Auswirkung auf Anwendungsebene anzeigen festlegen. Es ist eine Policy-basierte Sicherheitsfunktion, die vertraulichen Daten im Resultset einer Abfrage angegebenen Datenbank Felder ausgeblendet, während die Daten in der Datenbank nicht geändert werden.

Beispielsweise Kundendienstmitarbeiter in einem Callcenter kann Anrufer durch mehrere Ziffern ihrer Sozialversicherungsnummer oder Kreditkartennummer identifizieren, aber Datenelemente nicht vollständig angezeigt werden sollen, die Kundendienstmitarbeiter. Maskierungsregel kann definiert werden, dass alle Masken die letzten vier Ziffern der Sozialversicherungsnummer oder Kreditkartennummer im Ergebnis einer Abfrage festlegen. Beispielsweise kann eine geeignete Daten ein Schutz personenbezogene Informationen (PII) Daten definiert werden, damit Entwickler produktionsumgebung ohne gegen Auflagen zu Problembehandlungszwecken Abfragen kann.

## <a name="sql-database-dynamic-data-masking-basics"></a>SQL Datenbank dynamische Daten Masking-Grundlagen

Eine dynamischen Daten masking Richtlinie im Azure-Portal auswählen dynamische Daten Masking-Operation in der SQL-Datenbank-Konfiguration Blade oder Blatt Einstellungen einrichten.


### <a name="dynamic-data-masking-permissions"></a>Dynamische Daten Maskierung Berechtigungen

Dynamische Daten maskieren kann von Admin Azure-Datenbank-Administrator oder Security Officer Rollen konfiguriert.

### <a name="dynamic-data-masking-policy"></a>Richtlinien für dynamische Daten maskieren

* **SQL-Benutzer ausgeschlossen Masking** - eine Reihe von SQL-Benutzer oder AAD Identitäten, die nicht maskierte Daten in die SQL-Abfrageergebnisse erhalten. Beachten Sie, dass Benutzer mit Administratorrechten immer maskieren ausgeschlossen und die ursprünglichen Daten ohne Maske sehen.

* **Masking Regeln** - Regeln, in denen die vorgesehenen Felder zu maskierenden und Masking-Funktion, die verwendet werden soll. Die vorgesehenen Felder können mithilfe einer Datenbank Schema, Tabellen- und Spaltennamen definiert werden.

* **Masking-Funktionen** – eine Reihe von Methoden, die die Offenlegung von Daten für verschiedene Szenarien zu steuern.

| Masking-Funktion | Masking-Logik |
|----------|---------------|
| **Standard**  |**Vollständige Maskierung nach Datentypen festgelegten Felder**<br/><br/>• Verwendung von XXXX oder weniger Xs ist die Größe des Feldes weniger als 4 Zeichen für Zeichenfolgen-Datentypen (Nchar, Ntext Nvarchar).<br/>• Verwenden Sie einen Nullwert für numerische Datentypen (Bigint Bit Decimal, Int, Money, Numeric, Smallint, Smallmoney, Tinyint, Float, Real).<br/>• Verwenden Sie 01-01-1900 für Datum/Uhrzeit-Datentypen (Datum datetime2 Datetime, Datetimeoffset, Smalldatetime, Zeit).<br/>• Bei SQL Variante der Standardwert des aktuellen Typs verwendet wird.<br/>• XML-Dokument <masked/> wird verwendet.<br/>• Verwenden Sie einen leeren Wert für besondere Datentypen (Timestamp Tabelle Hierarchyid, GUID, Binary, Image, Varbinary räumliche Datentypen).
| **Kreditkarte** |**Masking-Methode die letzten vier Ziffern der bezeichneten Felder verfügbar macht** und eine Konstante Zeichenfolge als Präfix in Form einer Kreditkarte.<br/><br/>XXXX-XXXX-XXXX-1234|
| **Sozialversicherungsnummer** |**Masking-Methode die letzten vier Ziffern der bezeichneten Felder verfügbar macht** und eine Konstante Zeichenfolge als Präfix in Form von American Sozialversicherungsnummer.<br/><br/>XXX-XX-1234 |
| **E-Mail** | **Masking-Methode die macht des ersten Buchstabens und ersetzt die Domäne XXX.com** eine Konstante Zeichenfolge Präfix in Form einer e-Mail-Adresse.<br/><br/>aXX@XXXX.com |
| **Zufallszahl** | Nach der ausgewählten Berandungen und tatsächlichen **masking-Methode, die eine Zufallszahl generiert** . Wenn die festgelegten Grenzen gleich sind, werden Masking-Funktion eine Konstante Anzahl.<br/><br/>![Navigationsbereich](./media/sql-database-dynamic-data-masking-get-started/1_DDM_Random_number.png) |
| **Benutzerdefinierten text** | **Masking-Methode verfügbar die ersten und letzten Zeichen macht** und fügt eine Zeichenfolge benutzerdefinierter Abstand dazwischen. Wenn die ursprüngliche Zeichenfolge kürzer als die verfügbar gemachten Präfix und Suffix ist, wird nur die Füllzeichenfolge verwendet. <br/>Präfix [Abstand] suffix<br/><br/>![Navigationsbereich](./media/sql-database-dynamic-data-masking-get-started/2_DDM_Custom_text.png) |


<a name="Anchor1"></a>
### <a name="recommended-fields-to-mask"></a>Empfohlene Felder zum Maskieren

DDM Recommendations Engine flags sensible Felder gute Kandidaten für die Maskierung sind bestimmte Felder aus der Datenbank. Dynamische Daten Masking Blatt im Portal sehen Sie empfohlenen Spalten für Ihre Datenbank. Sie müssen lediglich **Maske hinzufügen** für eine oder mehrere Spalten und dann auf **Speichern** klicken, um eine Maske für diese Felder.

## <a name="set-up-dynamic-data-masking-for-your-database-using-the-azure-portal"></a>Dynamische datenmaskierung Datenbank Azure-Portal einrichten

1. Starten Sie das Azure-Portal unter [https://portal.azure.com](https://portal.azure.com).

2. Navigieren Sie zum Blatt Einstellungen der Datenbank, die vertraulichen Daten enthält, die Sie maskieren möchten.

3. Klicken Sie **Dynamische Daten Masking** Blade-Konfiguration **Dynamische Daten Masking** startet.

    * Alternativ können Sie unten im Abschnitt **Vorgänge** und klicken Sie auf **Dynamische Daten maskieren**.

    ![Navigationsbereich](./media/sql-database-dynamic-data-masking-get-started/4_ddm_settings_tile.png)<br/><br/>


4. In Blade-Konfiguration **Dynamische Daten Masking** sehen Sie einige Datenbankspalten, die Empfehlung Engine für Maskierung gekennzeichnet wurde. Annahme der Vorschläge für eine oder mehrere Spalten **Maske hinzufügen** klicken, und eine Maske auf der Standardtyp für diese Spalte erstellt. Ändern die Maskierung Funktion maskierungsregel und Bearbeiten der maskierungs Feldformat in ein anderes Format Ihrer Wahl. Achten Sie auf **Speichern** , um die Einstellung zu speichern.

    ![Navigationsbereich](./media/sql-database-dynamic-data-masking-get-started/5_ddm_recommendations.png)<br/><br/>


5. Klicken Sie eine Maske für jede Spalte in der Datenbank, oben Blade-Konfiguration **Dynamische Daten Masking** **Maske hinzufügen** öffnen Blade-Konfiguration **Masking Regel hinzufügen**

    ![Navigationsbereich](./media/sql-database-dynamic-data-masking-get-started/6_ddm_add_mask.png)<br/><br/>

6. Wählen Sie **Schema**, **Tabelle** und **Spalte** Feldes definieren, die maskiert werden.

7. **Feldformat Masking** aus Daten masking Kategorien auswählen

    ![Navigationsbereich](./media/sql-database-dynamic-data-masking-get-started/7_ddm_mask_field_format.png)<br/><br/>     

8. **Klicken Sie in der Maskierung Blatt aktualisieren festgelegten Regeln in der dynamischen masking Richtlinie zu maskieren.**

9. Geben Sie den SQL-Benutzer oder AAD Identitäten, die ausgeschlossen maskieren und haben Zugriff auf die nicht maskierten vertraulichen Daten. Dies sollte eine durch Semikolons getrennte Liste der Benutzer. Beachten Sie, dass Benutzer mit Administratorrechten immer Zugriff auf die ursprünglichen Daten nicht maskierten.

    ![Navigationsbereich](./media/sql-database-dynamic-data-masking-get-started/8_ddm_excluded_users.png)

    >[AZURE.TIP] Um zu machen, damit die Anwendungsschicht für privilegierte Benutzer Daten anzeigen kann, den SQL-Benutzer hinzufügen oder AAD Identität der Anwendung zur Abfrage der Datenbank verwendet. Es empfiehlt sich, diese Liste möglichst wenige privilegierte Benutzer zum Minimieren der sensiblen Daten enthalten.

10. **Klicken Sie in der Konfiguration Blade zum Speichern neuer oder aktualisierter Maskierung maskieren.**

## <a name="set-up-dynamic-data-masking-for-your-database-using-powershell-cmdlets"></a>Dynamische Daten Datenbank Powershell-Cmdlets masking einrichten

[SQL Azure-Datenbank Cmdlets](https://msdn.microsoft.com/library/azure/mt574084.aspx)anzeigen


## <a name="set-up-dynamic-data-masking-for-your-database-using-rest-api"></a>Einrichten von dynamischen datenmaskierung Datenbank REST-API

[Für SQL Azure-Datenbanken](https://msdn.microsoft.com/library/dn505719.aspx)anzeigen
