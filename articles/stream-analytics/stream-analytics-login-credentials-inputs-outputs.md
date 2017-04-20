<properties 
    pageTitle="Stream Analytics: Drehen Anmeldeinformationen für Eingaben und Ausgaben | Microsoft Azure" 
    description="Informationen Sie zum Aktualisieren der Anmeldeinformationen für Stream Analytics Eingaben und Ausgaben."
    keywords="Anmeldedaten"
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

#<a name="rotate-login-credentials-for-inputs-and-outputs-in-stream-analytics-jobs"></a>Drehen Sie Anmeldeinformationen für Eingaben und Ausgaben in Stream Analytics-Aufträge

##<a name="abstract"></a>Zusammenfassung
Azure Stream Analytics ist heute möglich ersetzen die Anmeldeinformationen auf einem Ausgang, während der Auftrag ausgeführt wird.

Azure Stream Analytics unterstützt einen Auftrag aus der letzten Ausgabe fortsetzen, wollten wir den gesamten Prozess minimiert die Verzögerung zwischen der Beendigung des Einzelvorgangs starten und drehen die Anmeldeinformationen freigeben.

##<a name="part-1---prepare-the-new-set-of-credentials"></a>Teil 1 – der neue Satz von Anmeldeinformationen:
Hierbei gilt folgende ein-und Ausgänge:

* BLOB-Speicher
* Ereignis-Hubs
* SQL-Datenbank
* Tabelle speichern

Andere ein-und Ausgänge fahren Sie mit Teil2 fort.

###<a name="blob-storagetable-storage"></a>BLOB-Speicher-Tabelle Speicher
1.  Gehen Sie auf Azure-Verwaltungsportal zu Speicher-Erweiterung:  
![graphic1][graphic1]
2.  Suchen von Ihrem Projekt verwendeten Speicher und eingehen:  
![graphic2][graphic2]
3.  Klicken Sie auf den Befehl Zugriffstasten verwalten:  
![graphic3][graphic3]
4.  Zwischen Zugang Primärschlüssel und sekundäre Tastenkombination **nicht für Ihr Projekt auswählen**.
5.  Drücken Sie Regenerieren (Regenerate):  
![graphic4][graphic4]
6.  Kopieren Sie den neu generierten Schlüssel:  
![graphic5][graphic5]
7.  Fahren Sie mit Teil 2.

###<a name="event-hubs"></a>Ereignis-hubs
1.  Azure-Verwaltungsportal Service Bus-Erweiterung weiter:  
![graphic6][graphic6]
2.  Suchen Sie den Service Bus-Namespace verwendet, die für Ihre Arbeit und eingehen:  
![graphic7][graphic7]
3.  Wenn Ihre Arbeit freigegebene Zugriffsrichtlinie auf Service Bus-Namespace verwendet, gehen Sie zu Schritt 6  
4.  Wechseln Sie zur Registerkarte Ereignis Hubs:  
![graphic8][graphic8]
5.  Suchen von Ihrem Projekt verwendeten Event-Hub und eingehen:  
![graphic9][graphic9]
6.  Wechseln Sie zur Registerkarte konfigurieren:  
![graphic10][graphic10]
7.  Der Name der Dropdownliste Suchen von Ihrem Projekt verwendeten freigegebenen Richtlinie:  
![graphic11][graphic11]
8.  Zwischen den Primärschlüssel und Sekundärschlüssel **nicht für Ihr Projekt auswählen**.  
9.  Drücken Sie Regenerieren (Regenerate):  
![graphic12][graphic12]
10. Kopieren Sie den neu generierten Schlüssel:  
![graphic13][graphic13]
11. Fahren Sie mit Teil 2.  

###<a name="sql-database"></a>SQL-Datenbank

>[AZURE.NOTE] Hinweis: Sie müssen mit der SQL-Datenbankdienst verbinden. Werden wir dazu die Erfahrung mit der Azure-Verwaltungsportal anzeigen, jedoch können einige clientseitige Tool wie SQL Server Management Studio verwenden.

1.  Zur Erweiterung SQL-Datenbanken auf der Azure-Verwaltungsportal:  
![graphic14][graphic14]
2.  Suchen Sie nach Ihrem Auftrag und **auf dem Server** Link in derselben Zeile verwendeten SQL-Datenbank:  
![graphic15][graphic15]
3.  Klicken Sie auf den Befehl verwalten:  
![graphic16][graphic16]
4.  Master-Datenbank Typ:  
![graphic17][graphic17]
5.  Geben Sie Ihren Benutzernamen, Kennwort, und klicken Sie auf:  
![graphic18][graphic18]
6.  Klicken Sie auf neue Abfrage:  
![graphic19][graphic19]
7.  Geben Sie die folgende Abfrage Benutzername < Login_name > ersetzen und Austauschen von <enterStrongPasswordHere> mit dem neuen Kennwort:  
`CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>'`
8.  Klicken Sie auf ausführen:  
![graphic20][graphic20]
9.  Zurück zu Schritt 2 und diese Zeit auf die Datenbank:  
![graphic21][graphic21]
10. Klicken Sie auf den Befehl verwalten:  
![graphic22][graphic22]
11. Geben Sie Ihren Benutzernamen, Kennwort, und klicken Sie auf Anmeldung:  
![graphic23][graphic23]
12. Klicken Sie auf neue Abfrage:  
![graphic24][graphic24]
13. Geben Sie in der folgenden Abfrage einen Namen zum Identifizieren dieser Anmeldung im Kontext dieser Datenbank (Sie können den gleichen Wert für < Login_name > Beispiel gab bereitstellen) soll < Benutzername > ersetzen und Ihr neuer Benutzername < Login_name > ersetzen:  
`CREATE USER <user_name> FROM LOGIN <login_name>`
14. Klicken Sie auf ausführen:  
![graphic25][graphic25]
15. Geben Ihre neuen Benutzer nun, mit derselben Rollen und Berechtigungen der ursprünglichen Benutzer.
16. Fahren Sie mit Teil 2.

##<a name="part-2-stopping-the-stream-analytics-job"></a>Teil 2: Beenden des Stream Analytics-Auftrags
1.  Azure-Verwaltungsportal Stream Analytics-Erweiterung weiter:  
![graphic26][graphic26]
2.  Suchen Sie Ihre Arbeit und eingehen:  
![graphic27][graphic27]
3.  Wechseln Sie zur Registerkarte Eingaben oder die Ausgaben abhängig, ob Sie die Anmeldeinformationen auf eine Eingabe oder Ausgabe drehen.  
![graphic28][graphic28]
4.  Auf den Stop-Befehl und bestätigen Sie, dass der Auftrag beendet wurde:  
![graphic29][graphic29] für den Auftrag zu warten.
5.  Suchen Sie die ein-/Ausgabe Anmeldeinformationen auf Drehen und sie möchten:  
![graphic30][graphic30]
6.  Fahren Sie mit Teil 3.

##<a name="part-3-editing-the-credentials-on-the-stream-analytics-job"></a>Teil 3: Die Anmeldeinformationen auf den Stream Analytics-Auftrag bearbeiten

###<a name="blob-storagetable-storage"></a>BLOB-Speicher-Tabelle Speicher
1.  Storage Konto Feld Suchen, und die neu generierten Schlüssel einfügen:  
![graphic31][graphic31]
2.  Klicken Sie auf den Befehl Speichern und bestätigen Sie speichern zu:  
![graphic32][graphic32]
3.  Ein Verbindungstest startet automatisch beim Speichern der Änderungen sicherzustellen, war erfolgreich.
4.  Fahren Sie mit Teil 4.

###<a name="event-hubs"></a>Ereignis-hubs
1.  Das Ereignis Hub Richtlinienschlüssel Feld und fügen Sie die neu generierten Schlüssel ein:  
![graphic33][graphic33]
2.  Klicken Sie auf den Befehl Speichern und bestätigen Sie speichern zu:  
![graphic34][graphic34]
3.  Ein Verbindungstest startet automatisch beim Speichern der Änderungen stellen Sie sicher, dass es erfolgreich ist.
4.  Fahren Sie mit Teil 4.

###<a name="power-bi"></a>Power BI
1.  Klicken Sie auf Autorisierung erneuern:  
* ![graphic35][graphic35]
* Sie erhalten die folgende Bestätigung:  
* ![graphic36][graphic36]
2.  Klicken Sie auf den Befehl Speichern und bestätigen Sie speichern zu:  
![graphic37][graphic37]
3.  Ein Verbindungstest wird automatisch gestartet, wenn die speichern, stellen Sie sicher, dass es war erfolgreich.
4.  Fahren Sie mit Teil 4.

###<a name="sql-database"></a>SQL-Datenbank
1.  Die Felder Benutzername und Kennwort finden und den neu erstellten Satz von Anmeldeinformationen einfügen:  
![graphic38][graphic38]
2.  Klicken Sie auf den Befehl Speichern und bestätigen Sie speichern zu:  
![graphic39][graphic39]
3.  Ein Verbindungstest startet automatisch beim Speichern der Änderungen stellen Sie sicher, dass es erfolgreich ist.  
4.  Fahren Sie mit Teil 4.

##<a name="part-4-starting-your-job-from-last-stopped-time"></a>Teil 4: Zuletzt beendet Ihre Arbeit ab
1.  Verlassen Sie den Ausgang:  
![graphic40][graphic40]
2.  Klicken Sie auf den Befehl starten:  
![graphic41][graphic41]
3.  Wählen Sie das letzte Mal beendet und auf OK.  
 ![graphic42][graphic42]
4.  Fahren Sie mit Teil 5.  

##<a name="part-5-removing-the-old-set-of-credentials"></a>Teil 5: Entfernen den alten Satz von Anmeldeinformationen
Hierbei gilt folgende ein-und Ausgänge:
* BLOB-Speicher
* Ereignis-Hubs
* SQL-Datenbank
* Tabelle speichern

###<a name="blob-storagetable-storage"></a>BLOB-Speicher-Tabelle Speicher
Wiederholen Sie Teil 1 der Zugriffstaste, das zuvor von Ihrem Auftrag jetzt nicht verwendete Zugriffstaste zu erneuern.

###<a name="event-hubs"></a>Ereignis-hubs
Wiederholen Sie Teil 1 für den Schlüssel, das zuvor von Ihrem Auftrag jetzt nicht verwendeten Schlüssel erneuern.

###<a name="sql-database"></a>SQL-Datenbank
1.  Zurück zum Abfragefenster Teil 1 Schritt 7 und in der folgenden Abfrage ersetzen < Previous_login_name > den Namen, die zuvor von Ihrem Auftrag verwendet wurde:  
`DROP LOGIN <previous_login_name>`  
2.  Klicken Sie auf ausführen:  
    ![graphic43][graphic43]  

Sie sollten die folgende Bestätigung erhalten: 

    Command(s) completed successfully.

## <a name="get-help"></a>Hilfe
Versuchen Sie weitere Unterstützung benötigen, [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren von Azure Stream Analytics-Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)


[graphic1]: ./media/stream-analytics-login-credentials-inputs-outputs/1-stream-analytics-login-credentials-inputs-outputs.png
[graphic2]: ./media/stream-analytics-login-credentials-inputs-outputs/2-stream-analytics-login-credentials-inputs-outputs.png
[graphic3]: ./media/stream-analytics-login-credentials-inputs-outputs/3-stream-analytics-login-credentials-inputs-outputs.png
[graphic4]: ./media/stream-analytics-login-credentials-inputs-outputs/4-stream-analytics-login-credentials-inputs-outputs.png
[graphic5]: ./media/stream-analytics-login-credentials-inputs-outputs/5-stream-analytics-login-credentials-inputs-outputs.png
[graphic6]: ./media/stream-analytics-login-credentials-inputs-outputs/6-stream-analytics-login-credentials-inputs-outputs.png
[graphic7]: ./media/stream-analytics-login-credentials-inputs-outputs/7-stream-analytics-login-credentials-inputs-outputs.png
[graphic8]: ./media/stream-analytics-login-credentials-inputs-outputs/8-stream-analytics-login-credentials-inputs-outputs.png
[graphic9]: ./media/stream-analytics-login-credentials-inputs-outputs/9-stream-analytics-login-credentials-inputs-outputs.png
[graphic10]: ./media/stream-analytics-login-credentials-inputs-outputs/10-stream-analytics-login-credentials-inputs-outputs.png
[graphic11]: ./media/stream-analytics-login-credentials-inputs-outputs/11-stream-analytics-login-credentials-inputs-outputs.png
[graphic12]: ./media/stream-analytics-login-credentials-inputs-outputs/12-stream-analytics-login-credentials-inputs-outputs.png
[graphic13]: ./media/stream-analytics-login-credentials-inputs-outputs/13-stream-analytics-login-credentials-inputs-outputs.png
[graphic14]: ./media/stream-analytics-login-credentials-inputs-outputs/14-stream-analytics-login-credentials-inputs-outputs.png
[graphic15]: ./media/stream-analytics-login-credentials-inputs-outputs/15-stream-analytics-login-credentials-inputs-outputs.png
[graphic16]: ./media/stream-analytics-login-credentials-inputs-outputs/16-stream-analytics-login-credentials-inputs-outputs.png
[graphic17]: ./media/stream-analytics-login-credentials-inputs-outputs/17-stream-analytics-login-credentials-inputs-outputs.png
[graphic18]: ./media/stream-analytics-login-credentials-inputs-outputs/18-stream-analytics-login-credentials-inputs-outputs.png
[graphic19]: ./media/stream-analytics-login-credentials-inputs-outputs/19-stream-analytics-login-credentials-inputs-outputs.png
[graphic20]: ./media/stream-analytics-login-credentials-inputs-outputs/20-stream-analytics-login-credentials-inputs-outputs.png
[graphic21]: ./media/stream-analytics-login-credentials-inputs-outputs/21-stream-analytics-login-credentials-inputs-outputs.png
[graphic22]: ./media/stream-analytics-login-credentials-inputs-outputs/22-stream-analytics-login-credentials-inputs-outputs.png
[graphic23]: ./media/stream-analytics-login-credentials-inputs-outputs/23-stream-analytics-login-credentials-inputs-outputs.png
[graphic24]: ./media/stream-analytics-login-credentials-inputs-outputs/24-stream-analytics-login-credentials-inputs-outputs.png
[graphic25]: ./media/stream-analytics-login-credentials-inputs-outputs/25-stream-analytics-login-credentials-inputs-outputs.png
[graphic26]: ./media/stream-analytics-login-credentials-inputs-outputs/26-stream-analytics-login-credentials-inputs-outputs.png
[graphic27]: ./media/stream-analytics-login-credentials-inputs-outputs/27-stream-analytics-login-credentials-inputs-outputs.png
[graphic28]: ./media/stream-analytics-login-credentials-inputs-outputs/28-stream-analytics-login-credentials-inputs-outputs.png
[graphic29]: ./media/stream-analytics-login-credentials-inputs-outputs/29-stream-analytics-login-credentials-inputs-outputs.png
[graphic30]: ./media/stream-analytics-login-credentials-inputs-outputs/30-stream-analytics-login-credentials-inputs-outputs.png
[graphic31]: ./media/stream-analytics-login-credentials-inputs-outputs/31-stream-analytics-login-credentials-inputs-outputs.png
[graphic32]: ./media/stream-analytics-login-credentials-inputs-outputs/32-stream-analytics-login-credentials-inputs-outputs.png
[graphic33]: ./media/stream-analytics-login-credentials-inputs-outputs/33-stream-analytics-login-credentials-inputs-outputs.png
[graphic34]: ./media/stream-analytics-login-credentials-inputs-outputs/34-stream-analytics-login-credentials-inputs-outputs.png
[graphic35]: ./media/stream-analytics-login-credentials-inputs-outputs/35-stream-analytics-login-credentials-inputs-outputs.png
[graphic36]: ./media/stream-analytics-login-credentials-inputs-outputs/36-stream-analytics-login-credentials-inputs-outputs.png
[graphic37]: ./media/stream-analytics-login-credentials-inputs-outputs/37-stream-analytics-login-credentials-inputs-outputs.png
[graphic38]: ./media/stream-analytics-login-credentials-inputs-outputs/38-stream-analytics-login-credentials-inputs-outputs.png
[graphic39]: ./media/stream-analytics-login-credentials-inputs-outputs/39-stream-analytics-login-credentials-inputs-outputs.png
[graphic40]: ./media/stream-analytics-login-credentials-inputs-outputs/40-stream-analytics-login-credentials-inputs-outputs.png
[graphic41]: ./media/stream-analytics-login-credentials-inputs-outputs/41-stream-analytics-login-credentials-inputs-outputs.png
[graphic42]: ./media/stream-analytics-login-credentials-inputs-outputs/42-stream-analytics-login-credentials-inputs-outputs.png
[graphic43]: ./media/stream-analytics-login-credentials-inputs-outputs/43-stream-analytics-login-credentials-inputs-outputs.png
 
