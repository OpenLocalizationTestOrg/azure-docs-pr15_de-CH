<properties
   pageTitle="Erste Schritte mit SQL Data Warehouse Erkennung"
   description="Erste Schritte mit Erkennung"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/24/2016"
   ms.author="lodipalm;sonyama;barbkess"/>


# <a name="get-started-with-threat-detection"></a>Erste Schritte mit Erkennung

> [AZURE.SELECTOR]
- [Überwachung](sql-data-warehouse-auditing-overview.md)
- [Erkennung](sql-data-warehouse-security-threat-detection.md)

## <a name="overview"></a>Übersicht

Erkennung erkennt außergewöhnlicher Datenbankaktivitäten potenzielle Sicherheitsrisiken für die Datenbank angibt. Erkennung ist in der Vorschau und für SQL Data Warehouse unterstützt.

Erkennung ermöglicht eine neue Ebene der Sicherheit ermöglicht Kunden erkennen und reagieren auf mögliche Gefahren mit Sicherheitshinweisen auf ungewöhnliche Aktivitäten an. Benutzer können die verdächtige Ereignisse bestimmen, ob sie versucht zuzugreifen daraus, verletzen oder Daten in das Datawarehouse mit [Azure SQL Data Warehouse Überwachung](sql-data-warehouse-auditing-overview.md) durchsuchen.
Erkennung erleichtert zu Adresse in das Datawarehouse ohne Sicherheitsexperten oder erweiterte Sicherheit Überwachungssysteme.

Beispielsweise erkennt Erkennung bestimmter außergewöhnlicher Datenbankaktivitäten mögliche SQL Injection Versuche angibt. SQL Injection ist einer der Web Application Security Probleme im Internet basierenden Angriff verwendet. Angreifer nutzen Sicherheitslücken böswillige SQL-Anweisungen in Anwendung Eingabefelder für Verletzung oder Ändern von Daten in die Datenbank eingefügt.


## <a name="set-up-threat-detection-for-your-database"></a>Erkennung für Ihre Datenbank einrichten

1. Starten Sie das Azure-Portal unter [https://portal.azure.com](https://portal.azure.com).

2. Navigieren Sie zu dem Blade Konfiguration des zu überwachenden SQL Data Warehouse. Wählen Sie im Blatt Einstellungen **Überwachung und Erkennung**.

    ![Navigationsbereich][1]

3. Aktivieren Sie in der Blade- **Überwachung und Erkennung** **auf** Überwachung der Bedrohung Erkennung Einstellungen angezeigt wird.

    ![Navigationsbereich][2]

4. **Bei** Erkennung zu aktivieren.

5. Konfigurieren Sie die Liste der e-Mails, die Sicherheitshinweise bei Lageraktivitäten anomale Daten erhalten.

6. **Überwachung & Bedrohung Erkennung** Konfiguration Blade zu speichern, die neue oder aktualisierte Überwachung Bedrohung Erkennung Richtlinie klicken Sie auf **Speichern** .

    ![Navigationsbereich][3]


## <a name="explore-anomalous-data-warehouse-activities-upon-detection-of-a-suspicious-event"></a>Durchsuchen Sie anomale Daten Lageraktivitäten entdeckte verdächtige Ereignis

1. Sie erhalten eine e-Mail-Benachrichtigung bei abweichenden Datenbankaktivitäten. <br/>
E-Mail informieren auf einschließlich der Art der außergewöhnlichen Aktivitäten, Datenbankname, Servernamen und die Ereigniszeit verdächtige Ereignis. Darüber hinaus enthalten Informationen zu den möglichen Ursachen und empfohlene Aktionen und Verringern der Bedrohungspotenzials in der Datenbank.<br/>

    ![Navigationsbereich][4]

2. Klicken Sie in der e-Mail auf **Überwachung Protokollieren von Azure SQL** Azure-Verwaltungsportal zu starten und entsprechende Überwachung Datensätze um verdächtige Ereignis anzeigen.

    ![Navigationsbereich][5]

3. Klicken Sie auf den Überwachungsdatensätzen Details verdächtigen Datenbankaktivitäten wie SQL-Anweisung an Fehler Ursache und Client-IP.

    ![Navigationsbereich][6]

4. Blatt Überwachung Datensätze klicken Sie auf eine vorkonfigurierte öffnen **Öffnen in Excel** excel-Vorlage importieren und tiefere Analyse des Überwachungsprotokolls um verdächtige Ereignis.<br/>
**Hinweis:** In Excel 2010 oder höher ist Power Query und die **Schnelle kombinieren** erforderlich

    ![Navigationsbereich][7]

5. Um Einstellung **Schnell kombinieren** - **POWER QUERY** Menüband-Registerkarte konfigurieren, wählen Sie **Optionen** für das Dialogfeld Optionen anzuzeigen. Wählen Sie den Datenschutz, und wählen Sie die zweite Option - "Sicherheitsstufen zu ignorieren und die Leistung möglicherweise verbessern":

    ![Navigationsbereich][8]

6. Laden SQL-Überwachungsprotokollen sicher, dass die Parameter in den Einstellungen Registerkarte ordnungsgemäß festgelegt sind und wählen Sie 'Daten' Menüband und klicken Sie auf "Alle aktualisieren".

    ![Navigationsbereich][9]

7. Die Ergebnisse werden in dem **SQL-Überwachungsprotokollen** ermöglicht tiefere Analyse der außergewöhnlichen Aktivitäten ausführen, die erkannt wurden und die Auswirkungen Ereignis in der Anwendung.


<!--Image references-->
[1]: ./media/sql-data-warehouse-security-threat-detection/1_td_click_on_settings.png
[2]: ./media/sql-data-warehouse-security-threat-detection/2_td_turn_on_auditing.png
[3]: ./media/sql-data-warehouse-security-threat-detection/3_td_turn_on_threat_detection.png
[4]: ./media/sql-data-warehouse-security-threat-detection/4_td_email.png
[5]: ./media/sql-data-warehouse-security-threat-detection/5_td_audit_records.png
[6]: ./media/sql-data-warehouse-security-threat-detection/6_td_audit_record_details.png
[7]: ./media/sql-data-warehouse-security-threat-detection/7_td_audit_records_open_excel.png
[8]: ./media/sql-data-warehouse-security-threat-detection/8_td_excel_fast_combine.png
[9]: ./media/sql-data-warehouse-security-threat-detection/9_td_excel_parameters.png
