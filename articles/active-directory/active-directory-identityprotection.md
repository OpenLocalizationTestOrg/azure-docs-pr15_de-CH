<properties
    pageTitle="Azure Active Directory-Schutz | Microsoft Azure"
    description="Erfahren Sie, wie Azure AD-Schutz ermöglicht die Fähigkeit der Angreifer eine gefährdete Identität oder Gerät und sichere Identität oder ein Gerät, das zuvor vermutet oder gefährdet bezeichnet."
    services="active-directory"
    keywords="Azure active Directory Schutz, Cloud app Discovery, Verwalten von Applikationen, Sicherheit, Risiken, Risiko, Schwachstelle, Sicherheitsrichtlinien"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection"></a>Azure Active Directory-Schutz 

Azure Active Directory Schutz ist ein Feature von Azure AD Premium P2-Edition, die bietet eine konsolidierte Ansicht Risiken und potenzielle Schwachstellen Ihres Unternehmens Identitäten. Microsoft Cloud-basierten Identitäten für ein Jahrzehnt Sicherung wurde und mit Azure AD Schutz, Microsoft macht dieselben Schutzsysteme verfügbar für Unternehmenskunden. Identitätsschutz nutzt vorhandene Azure AD Anomalien Erkennungsfunktionen (von Azure AD abweichenden Berichte) und stellt neue Risiken Ereignistypen, die Anomalien in Echtzeit erkannt werden.



##<a name="getting-started"></a>Erste Schritte

Die meisten Sicherheitslücken stattfinden, wenn Angreifer auf einer Umgebung durch Diebstahl der Identität eines Benutzers zugreifen. Angreifer haben immer wirksam bei Nutzung von Drittanbieter-Verstöße und anspruchsvolle Phishing-Attacken. Sobald ein Angreifer selbst ein Konto gering privilegierten Benutzer zugreift, ist relativ einfach für den Zugriff auf wichtige Unternehmensressourcen über Seitwärtsbewegung. Deshalb alle Identitäten zu schützen und eine Identität gefährdet, proaktiv verhindern die gefährdete Identität missbraucht. 

Ermitteln von gefährdeten Identitäten ist keine einfache Aufgabe. Glücklicherweise können Schutz: Schutz adaptive maschinelles lernalgorithmen und Heuristiken Anomalien erkennen und Ereignisse, die eine Identität kompromittiert werden möglicherweise verwendet.
 
Schutz mit diesen Daten generiert werden, Berichten und Alarmen, mit dem Sie diese Risikoereignisse untersuchen und geeignete Wiederherstellung und Minderungsaktion.
 
Aber Azure Active Directory Schutz ist mehr als ein Tool für Überwachung und Berichterstattung. Basierend auf Risiken, berechnet Schutz ein Benutzer Risikostufe für die einzelnen Benutzer aktivieren Sie risikobasierte Richtlinien automatisch Schutz Ihrer Organisation konfigurieren.  Diese risikobasierte Richtlinien neben anderen Zugangskontrolle Kontrollen Azure Active Directory mit EMS können automatisch blockieren oder bieten adaptive Abhilfemaßnahmen, die das Zurücksetzen von Kennwörtern und mehrstufige Authentifizierung erzwingen.  

####<a name="explore-identity-protections-capabilities"></a>Schutz des Fähigkeiten 

**Erkennen von Risiken und riskant Konten:**  

- 6 Risiko Ereignistypen maschinelles Lernen mit heuristische Regeln erkennen 

- Berechnung der Benutzer Risikostufen

- Benutzerdefinierte Empfehlungen Gesamt-Sicherheitslage verbessern durch Sicherheitslücken hervorheben



**Untersuchen von Risiken:**

- Senden von Benachrichtigungen für Risiken

- Kontextbezogene Informationen über Risikoereignisse untersuchen

- Bietet grundlegende Workflows Ermittlungen verfolgen

- Bequem Abhilfemaßnahmen wie das Zurücksetzen von Kennwörtern



**Risikobasierte bedingte Richtlinien:**

- Richtlinie zu riskant anmelden Anmelden blockieren oder dass mehrstufige Authentifizierung Probleme minimieren.

- Richtlinie zum Blockieren oder sichere riskant Benutzerkonten

- Richtlinie für mehrstufige Authentifizierung Registrierung erforderlich


## <a name="detection-and-risk"></a>Erkennung und Risiken

### <a name="risk-events"></a>Risiken

Ereignisse, die vom Identitätsschutz als verdächtig markiert wurden, und angeben, dass eine Identität beschädigt wurden. Eine vollständige Liste der Risiken finden Sie unter [Ereignistypen Risiko von Azure Active Directory Schutz erkannt](active-directory-identityprotection-risk-events-types.md). 


### <a name="risk-level"></a>Risiko

Die Risikostufe für ein Risikoereignis ist ein Hinweis Schweregrad des Risikoereignisses (hoch, Mittel oder niedrig). Die Risikostufe hilft Benutzern, Schutz Aktionen zu priorisieren, die sie ausführen müssen, um das Risiko für ihre Organisation. Der Schweregrad des Risikoereignisses stellt die Signalstärke als Indikator für Identität gefährden, kombiniert mit der Betrag an Störung, der in der Regel führt. 

- **Hoch**: hohe Zuverlässigkeit und hohem Schweregrad eintreten. Diese Ereignisse sind sichere Indikatoren, die Identität des Benutzers gefährdet und betroffenen Benutzerkonten sollten sofort wiederhergestellt.

- **Mittel**: hoher Priorität, aber niedriger vertrauen Risikoereignis oder umgekehrt. Diese Ereignisse sind gefährlich und betroffenen Benutzerkonten sollten behoben.

- **Niedrig**: geringe vertrauen und niedrigem Schweregrad eintreten. Dieses Ereignis eine sofortigen Maßnahmen erfordern jedoch zusammen mit anderen Risikoereignissen können ein Anzeichen, dass die Identität beschädigt wurde. 


![Risiko] (./media/active-directory-identityprotection/01.png "Risiko")

 

Risiken erkannt werden **in Echtzeit**, oder nach der Verarbeitung nach Risikoereignis bereits eingetreten ist (offline). Derzeit die meisten Risikoereignisse Schutz offline berechnet und im Schutz innerhalb von 2 bis 4 Stunden angezeigt. Während der Auswertung in Echtzeit Echtzeit Risikoereignisse erscheinen in der Konsole Identität Schutz innerhalb von 5 bis 10 Minuten.

Einige ältere Clients unterstützen derzeit nicht in Echtzeit Risiko Ereignis Detection und Prevention. Daher Anmeldungen von diesen Clients nicht erkannt oder in Echtzeit verhindert.


## <a name="investigation"></a>Untersuchung
Ihre Reise durch Schutz beginnt in der Regel mit dem Dashboard-Schutz. 

![Behebung] (./media/active-directory-identityprotection/1000.png "Behebung")

Das Dashboard bietet Zugriff auf:
 
- Berichte wie **Benutzer Risiken gekennzeichnet**, **Risiken** und **Schwachstellen**
- Wie die Konfiguration von **Sicherheitsrichtlinien**, **Benachrichtigung** und **mehrstufige Authentifizierung Registrierung**
 

Ist in der Regel der Ausgangspunkt für die Untersuchung der Prozess ist der Überprüfung der Aktivitäten, Protokolle und andere relevante Informationen zu einem Risikoereignis entscheiden oder eindämmende Maßnahmen erforderlich sind, und wie die Identität wurde beschädigt und verstehen, wie die betroffenen Identität verwendet wurde.

Sie können Ihre Ermittlungen [Benachrichtigungen](active-directory-identityprotection-notifications.md) binden Azure Active Directory Schutz per e-Mail sendet.

Die folgenden Abschnitte enthalten weitere Informationen und die Schritte, die Ermittlungen im Zusammenhang.  



## <a name="what-is-a-user-risk-level"></a>Was ist ein Benutzer Risiko?

Eine Risiko Benutzerebene zeigt (hoch, Mittel oder niedrig) der Wahrscheinlichkeit, dass die Identität des Benutzers gefährdet ist. Es ist auf Risikoereignisse Benutzer berechnet, die Identität des Benutzers zugeordnet sind. 

Der Status eines Risikoereignisses ist **aktiv** oder **geschlossen**. Nur Risikoereignisse **, die** beitragen zur Berechnung Benutzer. 

Die Risikostufe Benutzer errechnet die folgenden Eingaben:

- Aktive Risikoereignisse Beeinträchtigung des Benutzers
- Risikostufe Ereignisse 
- Gibt an, ob alle Abhilfemaßnahmen getroffen wurden 

![Benutzer-Risiken] (./media/active-directory-identityprotection/1001.png "Benutzer-Risiken")



Sie können Benutzer Risikostufen zu bedingten Richtlinien riskant Benutzern Anmelden verwenden oder zwingen, ihre Kennwörter sicher ändern. 


## <a name="closing-risk-events-manually"></a>Risiken manuell schließen

In den meisten Fällen gelangen Sie Abhilfemaßnahmen wie ein sicheres Kennwort zurücksetzen, um Risiken automatisch geschlossen. Jedoch kann dies nicht immer möglich.  
Dies ist beispielsweise der Fall, wenn:

- Benutzer mit aktiven Risiken wurde gelöscht
- Bei genauerem hinsehen, dass gemeldete Risikoereignis wurde durch berechtigte Benutzer durchführen

Da Risikoereignisse **, die** zur Berechnung Benutzer beitragen, müssen Sie manuell ein Risiko senken Risiken manuell schließen.  
Im Laufe der Untersuchung können Sie alle Aktionen zum Ändern des Status eines Risikoereignisses:

![Aktionen] (./media/active-directory-identityprotection/34.png "Aktionen")

- **Auflösen** - nach einem Risikoereignis untersuchen Sie geeignete Behebungsaktion außerhalb Schutz, und Sie glauben, dass das Risikoereignis berücksichtigt werden sollen markieren geschlossen, Sie das Ereignis als gelöst. Ereignisse legen die Risikoereignis Status auf geschlossen und Risikoereignis wird nicht mehr zur Gefahr aufgelöst wird.

- **Markieren als falsch-positiv** - In einigen Fällen kann einem Risikoereignis untersuchen und feststellen, dass es fälschlicherweise als riskant gekennzeichnet wurde. Sie können solche Ereignisse markieren Risikoereignis als falsch-positiven weniger. Dadurch wird den Computer lernen Algorithmen zur Klassifizierung von ähnlichen Ereignissen verbessern. Der Status von falsch-positiv-Ereignisse **geschlossen** und wird nicht zum Risiko beitragen.

- **Ignorieren** - Remediation-Maßnahmen nicht getroffen, aber das Risikoereignis aus der aktiven Liste entfernt werden soll, markieren Sie Risikoereignis ignorieren und des Ereignisstatus geschlossen. Ignorierte Ereignisse liefern keine Gefahr. Diese Option sollte nur unter ungewöhnlichen Umständen verwendet werden. 

- **Aktivieren** - manuell (durch Auswahl **lösen**, **False Positives**oder **ignorieren**) geschlossene Risikoereignisse kann reaktiviert werden Festlegen des Ereignisstatus in **aktiv**. Reaktiviert Risiken tragen zu den Benutzer risikoberechnung. Risikoereignisse geschlossen über Remediation (z. B. ein sicheres Kennwort zurücksetzen) können nicht reaktiviert werden. 




**Öffnen Sie das Dialogfeld Konfiguration**:

1. Klicken Sie auf Blade **Azure AD-Schutz** unter **untersuchen** **Risiken**.

    ![Zurücksetzen von Kennwörtern] (./media/active-directory-identityprotection/1002.png "Zurücksetzen von Kennwörtern")

2. Klicken Sie in der Liste **Risiken** auf Risiko.

    ![Zurücksetzen von Kennwörtern] (./media/active-directory-identityprotection/1003.png "Zurücksetzen von Kennwörtern")

2. Klicken Sie auf das Risiko Benutzer.

    ![Zurücksetzen von Kennwörtern] (./media/active-directory-identityprotection/1004.png "Zurücksetzen von Kennwörtern")



### <a name="closing-all-risk-events-for-a-user-manually"></a>Schließen alle Risikoereignisse für einen Benutzer manuell

Anstatt manuell Risikoereignisse für einen Benutzer einzeln, bietet Azure Active Directory Schutz auch eine Methode zum Schließen alle Ereignisse für einen Benutzer mit einem Mausklick.


![Aktionen] (./media/active-directory-identityprotection/2222.png "Aktionen")

Beim Klicken auf **schließen alle Ereignisse**alle Ereignisse werden geschlossen und der betroffene Benutzer nicht gefährdet.



## <a name="remediating-user-risk-events"></a>Beseitigen Benutzer Risikoereignisse

Eine Wiederherstellung ist zum Sichern einer Identität oder ein Gerät, das zuvor vermutet oder gefährdet bezeichnet. Eine Reparaturaktion stellt die Identität oder das Gerät in einen sicheren Zustand und löst vorherigen Risikoereignisse Identität oder Gerät zugeordnet.

Sie können zur Behebung Risikoereignisse Benutzer:

- Führen Sie ein sicheres Kennwort zurücksetzen Risikoereignisse Benutzer manuell beheben 

- Konfigurieren Sie Risiko Benutzersicherheitsrichtlinie verringern oder beseitigen Risikoereignisse Benutzer automatisch

- Image der infizierten Geräte  


### <a name="manual-secure-password-reset"></a>Manuelle Passwort zurücksetzen

Ein sicheres Kennwort zurücksetzen ist eine effektive Behebung für viele Risikoereignisse und bei automatisch schließt diese Risikoereignisse und Benutzerebene Risiko berechnet. Identitätsschutz-Dashboard können Sie eine Kennwortrücksetzdiskette für ein Risiko Benutzer initiieren. 

Das entsprechende Dialogfeld bietet zwei verschiedene Methoden zum Zurücksetzen eines Kennworts:

**Passwort** - Option **Benutzer Passwort** dem Benutzer erlauben auf sich selbst wiederherstellen, wenn den Benutzer ist für mehrstufige Authentifizierung registriert. Während der Benutzer weiter anmelden wird der Benutzer erforderlich, um eine mehrstufige Authentifizierung Herausforderung lösen und dann gezwungen, das Kennwort zu ändern. Diese Option ist nicht verfügbar, wenn das Benutzerkonto nicht bereits registrierte mehrstufige Authentifizierung.

**Passwort** - Option **generieren ein temporäres Kennwort** sofort das vorhandene Kennwort ungültig und erstellen ein neues temporäres Kennwort für den Benutzer. Senden Sie das neue Passwort, eine alternative e-Mail-Adresse für den Benutzer oder die Benutzer-Manager. Da temporäre, wird werden der Benutzer aufgefordert zum Ändern des Kennworts bei der Anmeldung.


![Richtlinie] (./media/active-directory-identityprotection/1005.png "Richtlinie")


**Öffnen Sie das Dialogfeld Konfiguration**:

1. Klicken Sie auf Blade **Azure AD-Schutz** auf **Benutzer Risiken gekennzeichnet**.

    ![Zurücksetzen von Kennwörtern] (./media/active-directory-identityprotection/1006.png "Zurücksetzen von Kennwörtern")


2. Wählen Sie aus der Liste der Benutzer Benutzer mit mindestens Risiken.

    ![Zurücksetzen von Kennwörtern] (./media/active-directory-identityprotection/1007.png "Zurücksetzen von Kennwörtern")


2. Klicken Sie auf den Benutzer auf **Kennwort zurücksetzen**.

    ![Zurücksetzen von Kennwörtern] (./media/active-directory-identityprotection/1008.png "Zurücksetzen von Kennwörtern")





## <a name="user-risk-security-policy"></a>Benutzersicherheitsrichtlinie Risiko

Benutzersicherheitsrichtlinie Risiko ist eine bedingte Zugriffsrichtlinie, die wertet der Risikostufe für einen bestimmten Benutzer und Sanierung und Minderung Aktionen basierend auf vordefinierten Startbedingungen und Regeln angewendet.


![Benutzer Ridk Richtlinie] (./media/active-directory-identityprotection/1009.png "Benutzer Ridk Richtlinie")


Azure AD-Schutz können Sie Ausgleich und Beseitigung von Risiken gekennzeichnet, aktivieren Sie Benutzer verwalten:

- Legen Sie die Benutzer und Gruppen, denen die Richtlinie gilt: 

    ![Benutzer Ridk Richtlinie] (./media/active-directory-identityprotection/1010.png "Benutzer Ridk Richtlinie")


- Festlegen Sie den Benutzer Risiko der Schwellenwert (niedrig, Mittel oder hoch), der die Richtlinie ausgelöst: 

    ![Benutzer Ridk Richtlinie] (./media/active-directory-identityprotection/1011.png "Benutzer Ridk Richtlinie")


- Setzen Sie die erzwungen werden, wenn die Richtlinie auslöst:

    ![Benutzer Ridk Richtlinie] (./media/active-directory-identityprotection/1012.png "Benutzer Ridk Richtlinie")


- Wechseln der Ihrer Richtlinie:

    ![Benutzer Ridk Richtlinie] (./media/active-directory-identityprotection/403.png "MFA-Registrierung")


- Überprüfen Sie und Werten Sie die Auswirkung der Änderung vor dem aktivieren:

    ![Benutzer Ridk Richtlinie] (./media/active-directory-identityprotection/1013.png "Benutzer Ridk Richtlinie")


Auswählen einen **hohen** Schwellenwert reduziert oft eine Richtlinie ausgelöst und minimiert die Auswirkung auf Benutzer.
Es schließt jedoch **niedriger** und **mittlerer** Benutzer gekennzeichnet ist Risiko die Richtlinie, die Identitäten oder Geräte, die zuvor vermutet oder gefährdet bekanntermaßen nicht sichern.

Wenn Sie die Richtlinie festlegen

- Ausschließen von Benutzern, die voraussichtlich viele Fehlalarme (Entwickler, Sicherheitsanalysten) generieren

- Ausschließen von Benutzern in die Richtlinie nicht praktisch ist Gebietsschemas (z. B. kein Zugriff Helpdesk)

- **Hoher** Schwellenwert während der ersten Rollen, verwenden oder wenn Sie Endbenutzern angezeigten Probleme minimieren müssen.

- Verwenden Sie **niedriger** Schwellenwert, wenn Ihre Organisation mehr Sicherheit erfordert. **Niedriger** Schwellenwert auswählen führt zusätzlicher Benutzer anmelden Probleme, aber Sicherheit.

Der empfohlene Standardwert für die meisten Organisationen ist eine Regel für einen Schwellenwert **Mittel** einen Ausgleich zwischen Nutzbarkeit und Sicherheit zu konfigurieren.

Eine Übersicht über die zugehörige Benutzeroberfläche finden Sie unter:

- [Kompromittierten Konto Recovery Fluss](active-directory-identityprotection-flows.md#compromised-account-recovery).  

- [Kompromittierten Konto blockiert Fluss](active-directory-identityprotection-flows.md#compromised-account-blocked).  


**Öffnen Sie das Dialogfeld Konfiguration**:

1. Klicken Sie auf Blade **Azure AD-Schutz** im Abschnitt **Konfigurieren** auf **Risiko Benutzerrichtlinie**.

    ![Benutzer Ridk Richtlinie] (./media/active-directory-identityprotection/1009.png "Benutzer Ridk Richtlinie")






## <a name="mitigating-user-risk-events"></a>Verringerung der Risiken Benutzerereignisse
Administratoren können Benutzer bei der Anmeldung je nach Risiko Sperren Benutzersicherheitsrichtlinie Risiko fest. 

Sperren einer anmelden:
 
- Verhindert die Generierung neuer Benutzer Risiko für betroffene Benutzer

- Ermöglicht es Administratoren, manuell beheben Risikoereignisse beeinflussen die Identität des Benutzers und in einen sicheren Zustand wiederherstellen



## <a name="what-is-a-sign-in-risk-level"></a>Was ist eine Risikostufe anmelden?

Ein anmelden Risiko ist eine Angabe der Wahrscheinlichkeit, dass für eine bestimmte Anmeldung, jemand die Identität des Benutzers zu authentifizieren (hoch, Mittel oder niedrig). Die Risikostufe Anmelden bei einer Anmeldung ausgewertet und Risiken berücksichtigt und Indikatoren in Echtzeit, bestimmte anmelden. 

## <a name="mitigating-sign-in-risk-events"></a>Anmelden Risiken mindern 
Eine Lösung ist eine Aktion, die ein Angreifer auf einem gefährdeten Identität oder Gerät nutzen, ohne die Identität oder das Gerät in einen sicheren Zustand wiederherstellen einzuschränken. Eine Abschwächung löst keine vorherige Anmeldung Risikoereignisse Identität oder Gerät zugeordnet.

Zugangskontrolle können in Azure AD-Schutz Sie automatisch anmelden Risiken mindern. Mithilfe dieser Richtlinien berücksichtigen die Risikostufe oder Anmeldung gefährlich Anmeldungen blockieren oder des Benutzers, mehrstufige Authentifizierung durchzuführen. Diese Aktionen können verhindern, dass einen Angreifer ausnutzen Identitätsdiebstahl zu Schaden und gebe Sie einige Zeit in der Identität. 


## <a name="sign-in-risk-security-policy"></a>-In Risiko-Sicherheitsrichtlinie

-In Risiko-Policy ist eine bedingte Richtlinie, die überprüft das Risiko einer bestimmten anmelden und Gegenmaßnahmen basierend auf vordefinierten Startbedingungen und Regeln.

![Risiko anmelden] (./media/active-directory-identityprotection/1014.png "Risiko anmelden")


Azure AD-Schutz hilft Ihnen die Minderung riskant Anmeldungen verwalten möchten:

- Legen Sie die Benutzer und Gruppen, denen die Richtlinie gilt: 

    ![Risiko anmelden] (./media/active-directory-identityprotection/1015.png "Risiko anmelden")


- Legen Sie den Anmeldung Risiko der Schwellenwert (niedrig, Mittel oder hoch), der die Richtlinie ausgelöst: 

    ![Risiko anmelden] (./media/active-directory-identityprotection/1016.png "Risiko anmelden")


- Setzen Sie die erzwungen werden, wenn die Richtlinie auslöst:  

    ![Risiko anmelden] (./media/active-directory-identityprotection/1017.png "Risiko anmelden")
    

- Wechseln der Ihrer Richtlinie:

    ![MFA-Registrierung] (./media/active-directory-identityprotection/403.png "MFA-Registrierung")

- Überprüfen Sie und Werten Sie die Auswirkung der Änderung vor dem aktivieren: 

    ![Risiko anmelden] (./media/active-directory-identityprotection/1018.png "Risiko anmelden")


### <a name="what-you-need-to-know"></a>Was Sie wissen müssen

Sie können eine Sicherheitsrichtlinie anmelden Risiko Mehrfaktor-Authentifizierung konfigurieren:

![Risiko anmelden] (./media/active-directory-identityprotection/1017.png "Risiko anmelden")

Aus Sicherheitsgründen kann diese Einstellung jedoch nur für Benutzer, die bereits für die kombinierte Authentifizierung registriert wurden. Die Bedingung, die mehrstufige Authentifizierung eines Benutzers für mehrstufige Authentifizierung noch nicht registriert ist erfüllt ist, wird der Benutzer gesperrt. 

Als bewährte Methode möchten Sie mehrstufige Authentifizierung riskant anmelden, sollten Sie:

1. [Mehrstufige Authentifizierung Registrierungspolitik](#multi-factor-authentication-registration-policy) für die betroffenen Benutzer aktivieren.
2. Die betroffenen Benutzer sich nicht riskant Sitzung MFA-Registrierung ausführen erforderlich

Diese Schritte wird sichergestellt, dass die kombinierte Authentifizierung riskant Anmeldung erforderlich ist. 


### <a name="best-practices"></a>Bewährte Methoden

 
Auswählen einen **hohen** Schwellenwert reduziert oft eine Richtlinie ausgelöst und minimiert die Auswirkung auf Benutzer.  
 
Es schließt jedoch **tief** oder **Mittel** Anmeldungen gekennzeichnet ist Risiko die Richtlinie, die ein Angreifer ausnutzen einer gefährdeten Identität nicht blockieren kann. 

Wenn Sie die Richtlinie festlegen 

- Ausschließen von Benutzern, die nicht / mehrstufige Authentifizierung nicht möglich

- Ausschließen von Benutzern in die Richtlinie nicht praktisch ist Gebietsschemas (z. B. kein Zugriff Helpdesk)

- Ausschließen von Benutzern, die voraussichtlich viele Fehlalarme (Entwickler, Sicherheitsanalysten) generieren

- **Hoher** Schwellenwert während der ersten Rollen, verwenden oder wenn Sie Endbenutzern angezeigten Probleme minimieren müssen.

- Verwenden Sie **niedriger** Schwellenwert, wenn Ihre Organisation mehr Sicherheit erfordert. **Niedriger** Schwellenwert auswählen führt zusätzlicher Benutzer anmelden Probleme, aber Sicherheit.

Der empfohlene Standardwert für die meisten Organisationen ist eine Regel für einen Schwellenwert **Mittel** einen Ausgleich zwischen Nutzbarkeit und Sicherheit zu konfigurieren.

 
-In Risiko-Policy ist:

- Angewendet auf alle Internetverkehr und moderne Authentifizierung anmelden.
- Für Applikationen mit älteren Sicherheitsprotokolle Deaktivieren von WS-Trust-Endpunkt an verbundenen IDP wie ADFS angewendet.

Die Seite **Risiken** Identitätsschutz-Konsole listet alle Ereignisse:

- Diese Richtlinie wurde zugewiesen
- Aktivität überprüfen und bestimmen, ob die Aktion entsprechenden war 

Eine Übersicht über die zugehörige Benutzeroberfläche finden Sie unter:

- [Risikoreiche recovery](active-directory-identityprotection-flows.md#risky-sign-in-recovery) 

- [Risikoreiche anmelden blockiert](active-directory-identityprotection-flows.md#risky-sign-in-blocked)  

- [Mehrstufige Authentifizierung Registrierung während riskant anmelden](active-directory-identityprotection-flows.md#multi-factor-authentication-registration-during-a-risky-sign-in)  





**Öffnen Sie das Dialogfeld Konfiguration**:

1. Klicken Sie auf das Blade **Azure AD-Schutz** im Abschnitt **Konfigurieren** auf **Risiko anmelden**.

    ![Benutzer Ridk Richtlinie] (./media/active-directory-identityprotection/1014.png "Benutzer Ridk Richtlinie")





## <a name="multi-factor-authentication-registration-policy"></a>Mehrstufige Authentifizierung Registrierungspolitik

Azure mehrstufige Authentifizierung ist eine Methode, die Sie überprüfen, die die Verwendung von mehr als nur einen Benutzernamen und ein Kennwort erfordert. Es bietet eine zweite Benutzer anmelden und Transaktionen.  
Wir empfehlen Sie Azure mehrstufige Authentifizierung Benutzer anmelden da benötigen sie:

- Bietet starke Authentifizierung mit einfachen Überprüfungsoptionen

- Spielt eine wichtige Rolle bei der Vorbereitung Ihrer Organisation schützen und Wiederherstellen von Accounts

![Benutzer Ridk Richtlinie] (./media/active-directory-identityprotection/1019.png "Benutzer Ridk Richtlinie")



Weitere Informationen finden Sie unter [Neuigkeiten Azure mehrstufige Authentifizierung?](../multi-factor-authentication/multi-factor-authentication.md)


Azure AD-Schutz können Sie verwalten das Rollout der kombinierten Authentifizierung Registrierung durch Konfigurieren einer Richtlinie, die ermöglicht: 



- Legen Sie die Benutzer und Gruppen, denen die Richtlinie gilt: 

    ![MFA-Richtlinie] (./media/active-directory-identityprotection/1020.png "MFA-Richtlinie")



- Bedienelemente erzwungen werden, wenn die Richtlinie auslöst:  

    ![MFA-Richtlinie] (./media/active-directory-identityprotection/1021.png "MFA-Richtlinie")


- Wechseln der Ihrer Richtlinie:

    ![MFA-Richtlinie] (./media/active-directory-identityprotection/403.png "MFA-Richtlinie")

- Zeigen Sie den aktuellen Registrierungsstatus an: 

    ![MFA-Richtlinie] (./media/active-directory-identityprotection/1022.png "MFA-Richtlinie")


Eine Übersicht über die zugehörige Benutzeroberfläche finden Sie unter:

- [Mehrstufige Authentifizierung Registrierung Fluss](active-directory-identityprotection-flows.md#multi-factor-authentication-registration).  

- [Mehrstufige Authentifizierung Registrierung während riskant anmelden](active-directory-identityprotection-flows.md#multi-factor-authentication-registration-during-a-risky-sign-in).  





**Öffnen Sie das Dialogfeld Konfiguration**:

1. Das Blade **Azure AD-Schutz** im Abschnitt **Konfigurieren** klicken Sie auf **Registrierung mehrstufige Authentifizierung**.

    ![MFA-Richtlinie] (./media/active-directory-identityprotection/1019.png "MFA-Richtlinie")





## <a name="next-steps"></a>Nächste Schritte

 - [Channel 9: Azure AD und Identität: Identität Schutz Vorschau](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)
 - [Ereignistypen Risiko von Azure Active Directory Schutz erkannt](active-directory-identityprotection-risk-events-types.md)
 - [Azure Active Directory Schutz entdeckten Schwachstellen](active-directory-identityprotection-vulnerabilities.md)
 - [Azure Active Directory Schutz Meldungen](active-directory-identityprotection-notifications.md)
 - [Azure Active Directory Schutz playbook](active-directory-identityprotection-playbook.md)
 - [Azure Active Directory Schutz-Glossar](active-directory-identityprotection-glossary.md)

 - [Anmelden Erfahrungen mit Azure AD-Schutz](active-directory-identityprotection-flows.md)

 - [Azure Active Directory-Schutz aktivieren](active-directory-identityprotection-enable.md)
 - [Azure Active Directory Schutz - Benutzer entsperren](active-directory-identityprotection-unblock-howto.md)

 - [Erste Schritte mit Azure Active Directory Schutz und Microsoft Graph](active-directory-identityprotection-graph-getting-started.md)


