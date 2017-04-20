<properties
   pageTitle="Azure Active Directory reporting - Vorschau | Microsoft Azure"
   description="Listet die verschiedenen Berichte Azure Active Directory-Vorschau"
   services="active-directory"
   documentationCenter=""
   authors="markusvi"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/30/2016"
   ms.author="markvi"/>

# <a name="azure-active-directory-reporting---preview"></a>Azure Active Directory reporting - Vorschau

> [AZURE.SELECTOR]
- [Azure-portal](active-directory-reporting-azure-portal.md)
- [Azure-Verwaltungsportal](active-directory-reporting-guide.md)

*Diese Dokumentation ist Teil der [Azure Active Directory-Berichte-Handbuch](active-directory-reporting-guide.md).*

Mit Berichten in der Vorschau Azure Active Directory erhalten Sie alle Informationen müssen Sie bestimmen, wie Ihre Umgebung geht. [Neuigkeiten in der Vorschau](active-directory-preview-explainer.md)

Es gibt zwei Hauptbereiche der Berichterstattung:

- **Anmelden Aktivitäten** – Informationen über die Verwendung von verwalteten Programmen und Benutzeraktivitäten anmelden

- **Überwachungsprotokolle** - Aktivität Systeminformationen Benutzer Verwaltung, Ihren verwalteten Clientanwendungen und Directory-Aktivitäten

Je nach Umfang der Daten, die Sie suchen, können Sie diese Berichte durch Klicken auf **Benutzer und Gruppen** oder **Anwendung** in der Dienstliste in [Azure-Portal](https://portal.azure.com)zugreifen.

## <a name="sign-in-activities"></a>Anmelden Aktivitäten

### <a name="user-sign-in-activities"></a>Benutzeraktivitäten anmelden

Mit den Informationen der Benutzer-Bericht finden Sie Antworten auf Fragen wie:

- Was ist das Muster Anmelden eines Benutzers?
- Wie viele Benutzer verfügen Benutzer über eine Woche angemeldet?
- Wie lautet der Status dieser anmelden?

Der Einstiegspunkt dieser Daten ist Benutzer-im Diagramm in **der Übersicht unter **Benutzer und Gruppen**** .

 ![Reporting] (./media/active-directory-reporting-azure-portal/05.png "Reporting")

Das Benutzer-Diagramm zeigt wöchentliche Aggregationen Zeichen ins für alle Benutzer in einem bestimmten Zeitraum. Der Standardwert für den Zeitraum beträgt 30 Tage.

![Reporting] (./media/active-directory-reporting-azure-portal/02.png "Reporting")

Wenn Sie an einem Tag in das Diagramm klicken, erhalten Sie eine detaillierte Liste der Aktivitäten anmelden.

![Reporting] (./media/active-directory-reporting-azure-portal/03.png "Reporting")

Jede Zeile in der Liste anmelden Aktivitäten gibt Ihnen detaillierte Informationen über die ausgewählte anmelden wie:

- Wer angemeldet ist?

- Was war der zugehörigen UPN?

- Was war die Anmeldung?

- Was ist die IP-Adresse des anmelden?

- Was den Status der Anmeldung?

### <a name="usage-of-managed-applications"></a>Verwendung der verwalteten Anwendung

Eine anwendungsorientierte Ansicht der Daten können Sie Fragen beantworten:

- Wer ist meine Anwendung?

- Was sind die oberen 3 Anwendung in Ihrer Organisation?

- Ich haben kürzlich eine Anwendung zurückgesetzt. Wie geht es?


Der Einstiegspunkt dieser Daten ist die Top 3 Anwendung in Ihrer Organisation innerhalb der letzten 30 Tage in **der Übersicht unter **Anwendung**** .

 ![Reporting] (./media/active-directory-reporting-azure-portal/06.png "Reporting")


App Verwendung Graph wöchentlichen Aggregationen von einem Zeitraum ins für Ihre Top 3 anmelden. Der Standardwert für den Zeitraum beträgt 30 Tage.

![Reporting] (./media/active-directory-reporting-azure-portal/78.png "Reporting")

Wenn Sie möchten, können Sie den Fokus auf eine bestimmte Anwendung festlegen.

![Reporting] (./media/active-directory-reporting-azure-portal/single_spp_usage_graph.png "Reporting")


Wenn Sie an einem Tag im Diagramm Verwendung app klicken, erhalten Sie eine detaillierte Liste der Aktivitäten anmelden.


![Reporting] (./media/active-directory-reporting-azure-portal/top_app_sign_ins.png "Reporting")



Die Option **Anmelden** gibt einen Überblick über alle Ereignisse zu einer Anwendung.

![Reporting] (./media/active-directory-reporting-azure-portal/85.png "Reporting")

Mithilfe der Spaltenauswahl können Sie die Felder auswählen, die Sie anzeigen möchten.

![Reporting] (./media/active-directory-reporting-azure-portal/column_chooser.png "Reporting")



### <a name="filtering-sign-ins"></a>Filterung anmelden

Sie können ein Zeitintervall in den Umfang der angezeigten Daten beschränken anmelden filtern.

![Reporting] (./media/active-directory-reporting-azure-portal/927.png "Reporting")


Eine andere Methode der Posten Aktivitäten anmelden wird nach bestimmten Einträgen gesucht.
Search-Methode können Sie Ihre Anmeldungen bestimmte **Benutzer**, **Gruppen** oder **Applikationen**Bereich.


![Reporting] (./media/active-directory-reporting-azure-portal/84.png "Reporting")

## <a name="audit-logs"></a>Audit-Protokolle

Überwachung Protokollen in Azure Active Directory können Datensätze für Compliance-Aktivitäten.

Es gibt drei Kategorien für die Überwachung der Aktivitäten im Azure-Portal:

- Benutzer und Gruppen   

- Applikationen

- Verzeichnis   


Eine vollständige Liste der Bericht Aktivitäten überwachen finden Sie in der [Liste Bericht Überwachungsereignisse](active-directory-reporting-audit-events.md#list-of-audit-report-events).


Der Einstiegspunkt für alle Überwachungsdaten ist **Überwachungsprotokolle** im Bereich **Aktivitäten** von **Azure Active Directory**.


![Überwachung] (./media/active-directory-reporting-azure-portal/61.png "Überwachung")


Ein Überwachungsprotokoll ist eine Listenansicht, die die Akteure (,) (was) Aktivitäten und Ziele.


![Überwachung] (./media/active-directory-reporting-azure-portal/345.png "Überwachung")


Weitere Informationen erhalten Sie auf ein Element in der Listenansicht.

![Überwachung] (./media/active-directory-reporting-azure-portal/873.png "Überwachung")




### <a name="users-and-groups-audit-logs"></a>Benutzer und Gruppen von Überwachungsprotokollen


Benutzer und Gruppen Überwachungsberichte können Sie Antworten auf Fragen erhalten:

- Welche Arten von Updates wurden Benutzer angewendet?

- Wie viele Benutzer wurden geändert?

- Wie viele Kennwörter geändert wurden?

- Was ist ein Administrator in einem Verzeichnis durchgeführt?

- Was sind Gruppen, die hinzugefügt wurden?

- Gibt es Gruppen die Mitgliedschaft geändert wird?

- Wurden die Besitzer der Gruppe geändert?

- Welche Lizenzen einer Gruppe oder einem Benutzer zugeordnet?


Wenn Sie Überwachungsdaten überprüfen, die für Benutzer und Gruppen beziehen möchten, finden Sie im Abschnitt **Aktivität** der **Benutzer und Gruppen**eine gefilterte Ansicht unter **Audit-Protokolle** .


![Überwachung] (./media/active-directory-reporting-azure-portal/93.png "Überwachung")


### <a name="application-audit-logs"></a>Anwendung von Überwachungsprotokollen

Mit Anwendung Sicherheitsprüfungsberichte, erhalten Sie Antworten auf Fragen wie:

- Was sind die Programme, die hinzugefügt oder aktualisiert?

- Was sind die Programme, die entfernt wurden?

- Geändert ein Service-Prinzip einer Anwendung hat sich?

- Wurden die Namen der Programme geändert?

- Zustimmung erteilt von einer Anwendung


Möchten Sie nur Überwachung Daten überprüfen, die Anwendung verknüpft ist, finden Sie im Abschnitt **Aktivität** **Anwendung**eine gefilterte Ansicht unter **Audit-Protokolle** .


![Überwachung] (./media/active-directory-reporting-azure-portal/134.png "Überwachung")


### <a name="filtering-audit-logs"></a>Filtern von Überwachungsprotokollen

Sie können einen Bericht durch ein Zeitintervall begrenzen die Menge der angezeigten Daten filtern.

![Überwachung] (./media/active-directory-reporting-azure-portal/324.png "Überwachung")

Eine andere Methode die Posten eines Überwachungsprotokolls wird nach bestimmten Einträgen gesucht.

![Überwachung] (./media/active-directory-reporting-azure-portal/237.png "Überwachung")

## <a name="next-steps"></a>Nächste Schritte

Finden Sie den [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).
