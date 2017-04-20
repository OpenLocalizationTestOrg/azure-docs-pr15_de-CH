<properties
 pageTitle="Erste Schritte mit Azure Scheduler in Azure-Portal | Microsoft Azure"
 description="Erste Schritte mit Azure Scheduler in Azure-portal"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="hero-article"
 ms.date="08/10/2016"
 ms.author="deli"/>

# <a name="get-started-with-azure-scheduler-in-azure-portal"></a>Erste Schritte mit Azure Scheduler in Azure-portal

Es ist einfach, geplante Aufträge in Azure Scheduler erstellen. In diesem Lernprogramm erfahren Sie, wie Sie einen Auftrag erstellen. Sie lernen auch Planers überwachen und Verwaltungsfunktionen.

## <a name="create-a-job"></a>Erstellen Sie einen Auftrag

1.  Melden Sie sich bei [Azure-Portal](https://portal.azure.com/).  

2.  Klicken Sie auf **+ neu** > _Planer_ in das Suchfeld eingeben > wählen **Sie Planer** Ergebnisse > Klicken Sie auf **Erstellen**.

     ![][marketplace-create]

3.  Erstellen Sie einen Auftrag, der einfach http://www.microsoft.com/ mit einer GET-Anforderung trifft. Geben Sie im Bildschirm **Steuerprogrammauftrag** die folgenden Informationen ein:

    1.  **Name:**`getmicrosoft`  

    2.  **Abonnement:** Azure-Abonnement   

    3.  **Projekt-Auflistung:** Wählen Sie eine vorhandenen auftragsauflistung oder **Neu erstellen** > geben.

4.  **Einstellung**als Nächstes die folgenden Werte:

    1.  **Typ:**` HTTP`  

    2.  **Methode:**`GET`  

    3.  **URL:**` http://www.microsoft.com`  

      ![][action-settings]

5.  Abschließend definieren eines Zeitplans. Der Auftrag als einmaligen Auftrag definiert werden, aber nehmen wir einen Wiederholungszeitplan:

    1. **Serie**:`Recurring`

    2. **Starten**: heute

    3. **Wiederholt jedes**:`12 Hours`

    4. **Ende**: zwei Tage ab heute das Datum  

      ![][recurrence-schedule]

6.  Klicken Sie auf **Erstellen**

## <a name="manage-and-monitor-jobs"></a>Verwalten und Überwachen von Aufträgen

Sobald ein Auftrag erstellt wird, wird er im Hauptmenü Azure Dashboard. Klicken Sie auf das Projekt und öffnet ein neues Fenster mit den folgenden Registerkarten:

1.  Eigenschaften  

2.  Einstellung  

3.  Zeitplan  

4.  Verlauf

5.  Benutzer

    ![][job-overview]

### <a name="properties"></a>Eigenschaften

Diese schreibgeschützten Eigenschaften beschreiben die Metadaten für den Job Scheduler.

   ![][job-properties]


### <a name="action-settings"></a>Einstellung

Auf einen Auftrag im Fenster **Aufträge** kann, konfigurieren. Dadurch erweitert, konfigurieren sie konfigurieren nicht den Assistenten schnell erstellen.

Für alle Aktivitätstypen können Sie die wiederholungsrichtlinie und die Fehleraktion ändern.

Bei HTTP und HTTPS Auftragsaktion können Sie alle zulässigen HTTP-Verb die Methode ändern. Sie können auch hinzufügen, löschen oder Ändern von Kopf- und Standardauthentifizierung Informationen.

Für Warteschlange Aktion Speichertypen kann das Speicherkonto, Warteschlangenname SAS-Token und Text ändern.

Für Service Bus Aktivitätstypen können Sie den Namespace, Thema/Warteschlangenpfad Authentifizierung, Transporttyp, Nachrichteneigenschaften und Nachrichtentext ändern.

   ![][job-action-settings]

### <a name="schedule"></a>Zeitplan

Hiermit können Sie den Zeitplan konfigurieren möchten, ändern Sie den Zeitplan in erstellt der Assistent zum schnellen erstellen.

Dies ist eine Möglichkeit, [komplexe Abfolgepläne und erweiterte Serie in Ihrem Auftrag](scheduler-advanced-complexity.md)

Sie können das Startdatum ändern und Zeit, Zeitplan und Ende Datum und Uhrzeit (wenn der Auftrag wiederholt wird.)

   ![][job-schedule]


### <a name="history"></a>Verlauf

Die Registerkarte **Verlauf** Zeigt ausgewählte Metriken für alle Auftragsausführungsergebnisse im System für den ausgewählten Auftrag. Diese Metriken geben Werte in Echtzeit über den Zustand der Planer:

1.  Status  

2.  Details  

3.  Wiederholungsversuche

4.  Vorkommen: 1., 2., 3., etc..

5.  Startzeit der Ausführung  

6.  Endzeit der Ausführung

   ![][job-history]

Sie können bei der Ausführung die folgenden **Versionsgeschichtedetails**die gesamte Antwort für jede Ausführung anzeigen klicken. In diesem Dialogfeld können Sie die Antwort in die Zwischenablage kopieren.

   ![][job-history-details]

### <a name="users"></a>Benutzer

(Azure Role-Based Access Control, RBAC) ermöglicht die detaillierte Verwaltung für Azure Scheduler. Informationen zum Verwenden der Registerkarte Benutzer finden Sie unter [Azure Role-Based-Zugriffskontrolle](../active-directory/role-based-access-control-configure.md)


## <a name="see-also"></a>Siehe auch

 [Was ist der Taskplaner?](scheduler-intro.md)

 [Taskplaner (Konzepte), Terminologie und Entitätshierarchie](scheduler-concepts-terms.md)

 [Pläne und Fakturierung in Azure Scheduler](scheduler-plans-billing.md)

 [Wie Sie komplexe Abfolgepläne und erweiterte Serie mit Azure Scheduler erstellen](scheduler-advanced-complexity.md)

 [Planer REST-API-Referenz](https://msdn.microsoft.com/library/mt629143)

 [Planer-PowerShell-Cmdlets verweisen](scheduler-powershell-reference.md)

 [Planer für hohe Verfügbarkeit und Zuverlässigkeit](scheduler-high-availability-reliability.md)

 [Planer Grenzen, Standards und Fehlercodes](scheduler-limits-defaults-errors.md)

 [Ausgehende Authentifizierung Planer](scheduler-outbound-authentication.md)


[marketplace-create]: ./media/scheduler-get-started-portal/scheduler-v2-portal-marketplace-create.png
[action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-action-settings.png
[recurrence-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-recurrence-schedule.png
[job-properties]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-properties.png
[job-overview]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-overview-1.png
[job-action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-action-settings.png
[job-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-schedule.png
[job-history]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history.png
[job-history-details]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history-details.png


[1]: ./media/scheduler-get-started-portal/scheduler-get-started-portal001.png
[2]: ./media/scheduler-get-started-portal/scheduler-get-started-portal002.png
[3]: ./media/scheduler-get-started-portal/scheduler-get-started-portal003.png
[4]: ./media/scheduler-get-started-portal/scheduler-get-started-portal004.png
[5]: ./media/scheduler-get-started-portal/scheduler-get-started-portal005.png
[6]: ./media/scheduler-get-started-portal/scheduler-get-started-portal006.png
[7]: ./media/scheduler-get-started-portal/scheduler-get-started-portal007.png
[8]: ./media/scheduler-get-started-portal/scheduler-get-started-portal008.png
[9]: ./media/scheduler-get-started-portal/scheduler-get-started-portal009.png
[10]: ./media/scheduler-get-started-portal/scheduler-get-started-portal010.png
[11]: ./media/scheduler-get-started-portal/scheduler-get-started-portal011.png
[12]: ./media/scheduler-get-started-portal/scheduler-get-started-portal012.png
[13]: ./media/scheduler-get-started-portal/scheduler-get-started-portal013.png
[14]: ./media/scheduler-get-started-portal/scheduler-get-started-portal014.png
[15]: ./media/scheduler-get-started-portal/scheduler-get-started-portal015.png
