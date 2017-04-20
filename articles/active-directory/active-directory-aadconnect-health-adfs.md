
<properties
    pageTitle="Schließen Sie Gesundheit mit Azure AD mit AD FS | Microsoft Azure"
    description="Dies ist die Seite Azure AD Connect Health überwachen Ihre lokalen AD FS-Infrastruktur."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="using-azure-ad-connect-health-with-ad-fs"></a>Schließen Sie Gesundheit mit Azure AD mit AD FS
Die folgende Dokumentation ist spezifisch für Ihre AD FS-Infrastruktur mit Azure AD Connect Health monitoring. Informationen zur Überwachung von Azure AD Connect (synchronisiert) mit Azure AD verbinden [Mithilfe von Azure AD Connect Health Synchronisierung](active-directory-aadconnect-health-sync.md)anzeigen Finden Sie außerdem Informationen zur Überwachung der Active Directory-Domänendienste mit Azure AD verbinden [Mithilfe von Azure AD Connect Health in AD DS](active-directory-aadconnect-health-adds.md).

## <a name="alerts-for-ad-fs"></a>Alarme für AD FS
Die Azure AD verbinden Gesundheitswarnungen finden Sie der Liste der aktiven Warnungen. Jede Warnung enthält Informationen, die Schritte und Links zu verwandter Dokumentation.

Sie können eine Warnung aktive oder gelöst öffnen ein neues Blatt mit zusätzlichen Informationen Schritte können Sie zum Auflösen der Warnung und Links in Unterlagen doppelklicken. Sie können auch Verlaufsdaten Warnungen anzeigen, die in der Vergangenheit gelöst wurden.

![Azure AD Connect Gesundheitsportal](./media/active-directory-aadconnect-health/alert2.png)



## <a name="usage-analytics-for-ad-fs"></a>Verwendungsanalyse für AD FS
Azure AD verbinden Gesundheit Verwendungsanalysen analysiert Authentifizierungsdatenverkehr Federation Server. Doppelklicken Sie Feld Analytics Verwendung Blade Analytics Verwendung verschiedene Metriken und Gruppen zeigt zu öffnen.

>[AZURE.NOTE] Um Verwendungsanalysen mit AD FS verwenden, müssen die AD FS-Überwachung aktiviert ist. Weitere Informationen finden Sie unter [AD FS Überwachung aktivieren](active-directory-aadconnect-health-agent-install.md#enable-auditing-for-ad-fs).

![Azure AD Connect Gesundheitsportal](./media/active-directory-aadconnect-health/report1.png)

Wählen Sie zusätzliche Statistiken, geben Sie einen Zeitraum zu ändern, die Gruppierung, Verwendung Analytics Diagramm Maustaste, und wählen Diagramm bearbeiten. Sie können dann Zeitraum, wählen Sie eine andere Metrik und die Gruppierung ändern. Können Sie die Verteilung der Authentifizierungsdatenverkehr basierend auf verschiedenen "Metrics" und Gruppe jede Metrik entsprechende "Group by" Parameter in der folgenden Tabelle beschrieben:

| Metrik | Gruppieren nach | Die Gruppierung ist und warum es nützlich? |
| ------ | -------- | -------------------------------------------- |
| Insgesamt angefordert: Die Gesamtzahl der Anfragen, die vom Verbunddienst verarbeitet | Alle | Zeigt die Gesamtanzahl der Anfragen ohne Gruppierung. |
|  | Anwendung | Gruppiert die Gesamtanzahl basierend auf die gezielte relying Party. Diese Gruppierung ist hilfreich, zu verstehen, welche Anwendung wie viel Prozent des gesamten Datenverkehrs empfangen. |
|  | Server | Gruppiert die Gesamtanzahl basierend auf dem Server, der die Anforderung verarbeitet. Diese Gruppierung ist für die Verteilung des gesamten Datenverkehrs zu verstehen. |
|  | Arbeitsplatz-Verknüpfung | Gruppiert die Gesamtanzahl abhängig, ob sie Geräte kommen, die Arbeitsplatz (bekannt). Diese Gruppierung ist hilfreich, zu verstehen, wenn Ihre Ressourcen erfolgt mit Geräten Identitätsinfrastruktur unbekannt sind. |
|  | Authentifizierungsmethode | Gruppiert die Gesamtanzahl für die Authentifizierung verwendete Authentifizierungsmethode abhängig. Diese Gruppierung ist die gemeinsame Authentifizierungsmethode vertraut machen, die für die Authentifizierung verwendet wird. Es folgen mögliche Authentifizierungsmethoden <ol> <li>Integrierte Windows-Authentifizierung (Windows)</li> <li>Formularbasierte Authentifizierung (Forms)</li> <li>SSO (einmaliges Anmelden)</li> <li>X509 Zertifikat Authentifizierung (Zertifikat)</li> <br>Wenn Verbundserver mit SSO-Cookie empfangen, wird die Anforderung als SSO (Single Sign On) gezählt. In solchen Fällen ist das Cookie gültig ist, der Benutzer wird nicht aufgefordert, Anmeldeinformationen und nahtlosen Zugriff auf die Anwendung. Dies ist häufig mehrere Identitätsempfänger Verbundserver geschützt haben. |
|  | Netzwerk | Gruppiert die Gesamtanzahl der Netzwerkadresse des Benutzers abhängig. Kann entweder Intranet oder extranet. Diese Gruppierung ist nützlich zu wissen, welcher Prozentsatz des Datenverkehrs im Intranet oder extranet stammt. |
| Gesamtanzahl fehlgeschlagen: Die Gesamtanzahl Fehler Anfragen vom Verbunddienst verarbeitet. <br> (Dieser Wert ist nur auf AD FS für Windows Server 2012 R2)| Fehlertyp | Zeigt die Anzahl der Fehler basierend auf vordefinierten Fehlertypen. Diese Gruppierung ist für häufige Arten von Fehlern zu verstehen. <ul><li>Falscher Benutzername oder Kennwort: Fehler aufgrund falscher Benutzername oder Kennwort.</li> <li>"Extranet sperren": Fehler aufgrund von Anfragen von einem Benutzer von extranet gesperrt wurde </li><li> "Kennwort abgelaufen": Fehler durch Benutzer mit einem abgelaufenen Kennwort.</li><li>"Konto deaktiviert": Fehler durch Benutzer mit einem deaktivierten Konto.</li><li>"Geräteauthentifizierung": Fehler durch Benutzer nicht authentifizieren Authentifizierung Gerät.</li><li>"Benutzerauthentifizierung Zertifikat": Fehler aufgrund nicht durch ein ungültiges Zertifikat authentifiziert Benutzer.</li><li>"MFA": Fehler durch Benutzer zu authentifizieren, mehrstufige Authentifizierung.</li><li>"Andere Anmeldeinformationen": "Ausgabe Autorisierung": Fehler aufgrund fehlgeschlagener Autorisierung.</li><li>"Emission Delegation": Fehler Fehlern Delegierung Ausgabe.</li><li>"Annahme token": aufgrund von ADFS Token aus der Identität von Drittanbieter ablehnen.</li><li>"Protokoll": aufgrund Protokollfehler.</li><li>"Unbekannt": alle. Andere Fehler, die nicht in den definierten Kategorien.</li> |
|  | Server | Gruppen Fehler basierend auf dem Server. Diese Gruppierung ist fehlerverteilung über Server vertraut machen. Ungleichmäßige Verteilung konnte ein Server in einem fehlerhaften Zustand sein. |
|  | Netzwerk | Gruppen Fehler basierend auf den Netzwerkspeicherort von Anfragen (Intranet Vs extranet). Diese Gruppierung ist nützlich, welche Anfragen zu verstehen, die fehlschlagen. |
|  | Anwendung | Fehler basierend auf der zielgerichteten Anwendung (relying Party) gruppiert. Diese Gruppierung ist hilfreich, zu verstehen, welche Einsatzzweck die höchste Anzahl von Fehlern sehen. |
| Durchschnittliche Anzahl der eindeutigen Benutzer im System aktiv Benutzeranzahl: | Alle | Diese Metrik stellt die durchschnittliche Anzahl der Benutzer mit dem Verbunddienst im ausgewählten Zeitscheibe. Die Benutzer werden nicht gruppiert. <br>Die durchschnittliche hängt die Zeitscheibe ausgewählt. |
|  | Anwendung | Die durchschnittliche Anzahl der Benutzer auf die gezielte Anwendung (relying Party) gruppiert. Diese Gruppierung ist hilfreich, zu verstehen, wie viele Benutzer die Anwendung verwenden. |


## <a name="performance-monitoring-for-ad-fs"></a>Systemmonitor für AD FS
Azure AD verbinden Health Performance Monitoring bietet Überwachungsinformationen Kennzahlen. Die Überwachung Kontrollkästchen öffnet ein neues Blatt mit detaillierten Informationen über die Kennzahlen.


![Azure AD Connect Gesundheitsportal](./media/active-directory-aadconnect-health/perf1.png)


Durch die Option Filter oben das Blade, können Sie vom Server an einen einzelnen Server Metriken filtern. Ändern Sie Metriken überwachen Diagramm unter Überwachung Blade Maustaste, und wählen Sie Diagramm bearbeiten. Dann neues Blatt öffnet, können Sie zusätzliche Statistiken aus der Dropdownliste auswählen und geben Sie einen Zeitraum für die Leistungsdaten.

## <a name="reports-for-ad-fs"></a>Berichte für AD FS
Azure AD Connect Health berichtet über Aktivität und Leistung von AD FS. Diese Berichte können Administratoren Einblick auf den AD FS-Servern.

### <a name="top-50-users-with-failed-usernamepassword-logins"></a>Top 50 Benutzer mit fehlgeschlagener Anmeldeversuche von Benutzername und Kennwort

Einer der Gründe für eine fehlgeschlagene Authentifizierungsanforderung für einen AD FS-Server ist eine Anforderung mit ungültigen Anmeldeinformationen, einen falschen Benutzernamen oder Kennwort. In der Regel Benutzer komplexe Kennwörter, vergessenen Kennwörtern oder Tippfehler ausgeführt.

Aber es gibt andere Gründe, die eine unerwartete Anzahl von Anfragen behandelt den AD FS-Servern z. B. führen können: eine Anwendung, die Benutzeranmeldeinformationen Caches und die Anmeldeinformationen ablaufen oder ein böswilliger Benutzer versuchen, ein Konto mit einer Reihe von bekannten Kennwörter anmelden. Diese Beispiele sind gute Gründe, die zu einem Anstieg der Anfragen.

Azure AD Connect Health für ADFS stellt einen Bericht über Top-50 Benutzer mit fehlgeschlagenen Anmeldeversuchen durch Benutzername oder Kennwort ungültig. Dieser Bericht wird erreicht, indem alle AD FS-Servern in der generierten Prüfereignisse verarbeiten

![Azure AD Connect Gesundheitsportal](./media/active-directory-aadconnect-health-adfs/report1a.png)

In diesem Bericht können Sie leicht auf die folgenden Informationen:

- Gesamtanzahl der fehlgeschlagenen Anfragen mit falschen Benutzernamen und Kennwort in den letzten 30 Tagen
- Durchschnittliche Anzahl der Benutzer mit einer Benutzername/Kennwort ungültig Anmeldung pro Tag


Auf diesem Teil führt Sie Hauptbericht Blade, das zusätzliche Details enthält. Dieses Blatt enthält ein Diagramm mit Trendinformationen geplanten Anfragen mit falschen Benutzernamen oder Kennwort einzurichten. Außerdem bietet es die Liste der Top 50-Benutzer die Anzahl der fehlgeschlagenen Versuche.

Das Diagramm enthält folgende Informationen:

- Die Gesamtanzahl fehlgeschlagener Anmeldeversuche durch falsche Benutzername/Kennwort pro Tag.
- Die Gesamtanzahl der eindeutigen Benutzer, die Logins pro Tag nicht.
- Clientadresse der letzten Anforderung

![Azure AD Connect Gesundheitsportal](./media/active-directory-aadconnect-health-adfs/report3a.png)

Der Bericht enthält folgende Informationen:

| Angebot melden | Beschreibung
| ------ | -------- |
|Benutzer-ID| Zeigt die Benutzer-ID, die verwendet wurde. Dieser Wert Eingabe des Benutzers in einigen Fällen ist der falsche Benutzer-ID verwendet wird.|
|Fehlgeschlagene Versuche| Zeigt die Gesamtanzahl der fehlgeschlagenen Versuche, bestimmte Benutzer-ID Die Tabelle wird mit den meisten Versuche in absteigender Reihenfolge sortiert.|
|Letzten Fehler| Zeigt den Zeitstempel der letzten Fehler.
|Letzter Fehler IP | Zeigt die Client-IP-Adresse aus der neuesten fehlerhafte Anforderung.|



>[AZURE.NOTE] Dieser Bericht wird automatisch aktualisiert, nachdem alle zwei Stunden mit den neuen Informationen innerhalb dieses Zeitraums gesammelt. Daher können Anmeldeversuche innerhalb der letzten zwei Stunden im Bericht nicht berücksichtigt.



## <a name="related-links"></a>Verwandte links

* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Azure AD Connect Health Agent-Installation](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health-Operationen](active-directory-aadconnect-health-operations.md)
* [Verwenden von Azure AD Connect Health für Synchronisierung](active-directory-aadconnect-health-sync.md)
* [Gesundheit in AD DS verbinden über Azure AD](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health FAQ](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect Version Gesundheitsdaten](active-directory-aadconnect-health-version-history.md)
