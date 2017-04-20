<properties
   pageTitle="Azure Active Directory Reporting: Erste Schritte | Microsoft Azure"
   description="Listet die verschiedenen Berichte in Azure Active Directory reporting"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="03/07/2016"
   ms.author="dhanyahk"/>

# <a name="getting-started-with-azure-active-directory-reporting"></a>Erste Schritte mit Azure Active Directory

## <a name="what-it-is"></a>Was ist

Azure Active Directory (Azure AD) enthält, Sicherheit, Aktivität und Audit-Berichte für das Verzeichnis. Hier ist eine Liste der Berichte:

### <a name="security-reports"></a>Sicherheitsberichte

- Anmelden von unbekannten Quellen
- Anmeldungen nach mehreren Fehlern
- Anmelden über mehrere Regionen
- Anmelden von IP-Adressen mit verdächtigen Aktivitäten
- Ungewöhnliche Aktivität
- Anmelden von möglicherweise infizierten Geräte
- Benutzer mit außergewöhnlicher Aktivität

### <a name="activity-reports"></a>Aktivitätsberichte

- Anwendung: Zusammenfassung
- Anwendung: detaillierte
- Anwendung-dashboard
- Berücksichtigen Sie Fehler-Bereitstellung
- Einzelne Geräte
- Einzelne Benutzer Aktivitäten
- Aktivitätsbericht für Gruppen
- Kennwort zurücksetzen Registrierung Aktivitätsbericht
- Passwort-Aktivität

### <a name="audit-reports"></a>Audit-Berichte

- Verzeichnis-Überwachungsbericht

> [AZURE.TIP] Weitere Dokumentation zur Azure AD-Berichte anzeigen Sie [Berichte Verwendung anzeigen](active-directory-view-access-usage-reports.md)



## <a name="how-it-works"></a>Funktionsweise


### <a name="reporting-pipeline"></a>Pipeline Reporting

Reporting Pipeline besteht aus drei Hauptschritten. Jedes Mal, wenn ein Benutzer anmeldet oder eine Authentifizierung erfolgt, geschieht Folgendes:

- Zuerst (erfolgreich oder nicht erfolgreich), wird der Benutzer authentifiziert, und das Ergebnis in Azure Active Directory Service-Datenbanken gespeichert.
- In regelmäßigen Abständen alle aktuellen Zeichen ins verarbeitet. An diesem Punkt Sicherheit und ungewöhnliche Aktivität Algorithmen suchen alle aktuelle Zeichen ins auf verdächtige Aktivitäten.
- Nach der Verarbeitung werden die Berichte geschrieben, zwischengespeichert und im klassischen Azure-Portal bereitgestellt.

### <a name="report-generation-times"></a>Bericht generierungszeiten

Aufgrund der großen Anzahl von Authentifizierungen und Signieren ins verarbeitet der Azure AD-Plattform, die neuesten Anmeldungen verarbeitet, im Durchschnitt eine Stunde alt. In seltenen Fällen dauert es bis zu 8 Stunden aktuelle Anmeldungen verarbeiten.

Die letzten verarbeiteten-Anmeldung finden Sie anhand den Hilfetext am oberen Rand jeder Bericht.

![Hilfetext am oberen Rand jeder Bericht](./media/active-directory-reporting-getting-started/reportingWatermark.PNG)

> [AZURE.TIP] Weitere Dokumentation zur Azure AD-Berichte anzeigen Sie [Berichte Verwendung anzeigen](active-directory-view-access-usage-reports.md)



## <a name="getting-started"></a>Erste Schritte


### <a name="sign-into-the-azure-classic-portal"></a>Klassischen Azure-Portal anmelden

Zunächst müssen Sie den [Azure-Verwaltungsportal](https://manage.windowsazure.com) als oder Compliance-Administrator anmelden. Ein Azure-Abonnement Dienstadministrator oder Co-Administrator muss oder verwenden "Zugriff auf Azure AD" Azure-Abonnement.

### <a name="navigate-to-reports"></a>Navigieren Sie zu Berichte

Um Berichte anzuzeigen, navigieren Sie auf der Registerkarte Berichte am Anfang des Verzeichnisses.

Ist dies das erste Mal Berichte anzeigen, müssen Sie ein Dialogfeld zustimmen, bevor Sie die Berichte anzeigen können. Dies ist ist für Administratoren in Ihrer Organisation zum Anzeigen dieser Daten privaten Informationen in einigen Ländern betrachtet werden können.

![Im Dialogfeld](./media/active-directory-reporting-getting-started/dialogBox.png)

### <a name="explore-each-report"></a>Jeder Bericht untersuchen

Navigieren Sie in jeden Bericht auf die Daten und verarbeitet anmelden. Finden Sie eine [Liste aller Berichte hier](active-directory-reporting-guide.md)ein.

![Alle Berichte](./media/active-directory-reporting-getting-started/reportsMain.png)

### <a name="download-the-reports-as-csv"></a>Die Berichte als CSV-Datei herunterladen

Jeder Bericht kann als CSV (durch Trennzeichen getrennte) heruntergeladen werden. Sie können diese Dateien in Excel, PowerBI oder Fremdanbieter-Programme weiterhin Ihre Daten analysieren.

Jeder Bericht als CSV herunterladen navigieren Sie, den Bericht und klicken Sie unten auf "Herunterladen".

![Schaltfläche Herunterladen](./media/active-directory-reporting-getting-started/downloadButton.png)

> [AZURE.TIP] Weitere Dokumentation zur Azure AD-Berichte anzeigen Sie [Berichte Verwendung anzeigen](active-directory-view-access-usage-reports.md)





## <a name="next-steps"></a>Nächste Schritte

### <a name="customize-alerts-for-anomalous-sign-in-activity"></a>Passen Sie für ungewöhnliche Zeichen in der Aktivität an

Navigieren Sie zur Registerkarte "Konfigurieren" des Verzeichnisses.

Gehen Sie zum Abschnitt "Benachrichtigung".

Aktivieren Sie oder deaktivieren Sie im Abschnitt "E-Mail-Benachrichtigung von außergewöhnlicher anmelden".

![Im Abschnitt Benachrichtigung](./media/active-directory-reporting-getting-started/notificationsSection.png)

### <a name="integrate-with-the-azure-ad-reporting-api"></a>Azure AD Reporting API integriert

[Erste Schritte mit API-Berichte](active-directory-reporting-api-getting-started.md)anzeigen

### <a name="engage-multi-factor-authentication-on-users"></a>Mehrstufige Authentifizierung Benutzer führen

Wählen Sie einen Benutzer in einem Bericht.

Klicken Sie auf "MFA aktivieren", am unteren Bildschirmrand.

![Die kombinierte Authentifizierung Schaltfläche am unteren Bildschirmrand](./media/active-directory-reporting-getting-started/mfaButton.png)

> [AZURE.TIP] Weitere Dokumentation zur Azure AD-Berichte anzeigen Sie [Berichte Verwendung anzeigen](active-directory-view-access-usage-reports.md)




## <a name="learn-more"></a>Weitere Informationen


### <a name="audit-events"></a>Ereignisse überwachen

Erfahren Sie mehr über Ereignisse im Verzeichnis in [Azure Active Directory Reporting Audit-Ereignisse](active-directory-reporting-audit-events.md)überwacht werden.

### <a name="api-integration"></a>API-Integration

Finden Sie unter [Erste Schritte mit der Reporting-API](active-directory-reporting-api-getting-started.md) und die [API-Referenzdokumentation](https://msdn.microsoft.com/library/azure/mt126081.aspx).

### <a name="get-in-touch"></a>Kontakt

E-Mail [aadreportinghelp@microsoft.com](mailto:aadreportinghelp@microsoft.com) für Feedback, Hilfe und alle Fragen.

> [AZURE.TIP] Weitere Dokumentation zur Azure AD-Berichte anzeigen Sie [Berichte Verwendung anzeigen](active-directory-view-access-usage-reports.md)
