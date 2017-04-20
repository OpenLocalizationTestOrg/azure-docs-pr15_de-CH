<properties
    pageTitle="FAQ: Azure AD Password Management | Microsoft Azure"
    description="Häufig gestellte Fragen (FAQ) zur Verwaltung der Kennwörter in Azure AD, einschließlich Kennwort zurücksetzen, Registrierung, Berichte und Rückschreiben in lokale Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="asteen"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="asteen"/>

# <a name="password-management-frequently-asked-questions"></a>Verwaltung der Kennwörter häufig gestellte Fragen

> [AZURE.IMPORTANT] **Sind Sie hier beim Anmelden Probleme auftreten?** In diesem Fall [hier wie ändern und Ihr eigenes Kennwort](active-directory-passwords-update-your-own-password.md).

Es folgen einige häufig gestellten Fragen für alle Aktivitäten im Zusammenhang mit Kennwort.

Wenn Sie sich mit einer Frage, die Sie nicht beantworten, Suchen nach Hilfe zu einem bestimmten Problem konfrontiert sind, können Sie auf unten lesen, um festzustellen, ob wir bereits behandelt haben.  Wenn noch nicht keine Sorge! Zögern Sie Fragen haben, die nicht hier in [Azure AD-Foren](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD) und wir werden Sie so schnell wie möglich.

Diese FAQ ist in folgende Abschnitte unterteilt:

- [**Fragen zur Registrierung von Kennwort zurücksetzen**](#password-reset-registration)
- [**Fragen zum Zurücksetzen des Kennworts**](#password-reset)
- [**Fragen zum Passwort Management-Berichte**](#password-management-reports)
- [**Rückschreiben Kennwort Fragen**](#password-writeback)

## <a name="password-reset-registration"></a>Passwort-Registrierung
 - **F: registrieren können Benutzer ihre eigenen Daten Kennwort zurücksetzen?**

 > **A:** Ja, Zurücksetzen des Kennworts aktiviert und besitzen sie Kennwort zurücksetzen Registrierung Portal unter http://aka.ms/ssprsetup registrieren ihre Authentifizierungsinformationen mit Kennwort zurücksetzen gelangen. Benutzer können auch soll die Abdeckung am http://myapps.microsoft.com, auf die Registerkarte Profil und Register Option Passwort auf registrieren. Weitere Informationen können Ihr Kennwort zurücksetzen, indem Sie lesen, wie Benutzer für das Zurücksetzen von Kennwörtern konfiguriert konfiguriert.

 - **Kann Kennwort zurücksetzen Daten für Benutzer werden definiert?**

 > **A:** Ja, mit Dirsync-PowerShell oder über das Portal [Azure-Verwaltungsportal](https://manage.windowsazure.com) oder Office Admin möchten. Erfahren Sie mehr über dieses Feature auf dem Blogbeitrag verbessert Datenschutz für Azure AD MFA und Kennwort zurücksetzen Telefonnummern und Lesen erfahren Sie, wie Daten von Kennwort verwendet werden zurückgesetzt.

 - **F: synchronisieren kann ich Daten für Sicherheitsfragen vor?**

 > **A:** Nein, dies ist heute nicht möglich, aber wir werden es berücksichtigen.

 - **F: registrieren können Benutzer Daten so, dass andere Benutzer diese Daten nicht angezeigt?**

 > **A:** Ja, wenn Benutzer Daten mit Kennwort zurücksetzen Registrierungsportal registriert es gespeichert in privaten Authentifizierungsinformationen Felder nur sichtbar globale Administratoren und Benutzer selbst. Erfahren Sie mehr über dieses Feature auf dem Blogbeitrag verbessert Datenschutz für Azure AD MFA und Kennwort zurücksetzen Telefonnummern und Lesen erfahren Sie, wie Daten von Kennwort verwendet werden zurückgesetzt.

 - **F: müssen meine Benutzer registriert werden, bevor das Zurücksetzen von Kennwörtern verwendet werden kann?**

 > **A:** Nein, wenn Sie genügend Authentifizierungsinformationen selbst definieren, werden Benutzer keinen registrieren. Zurücksetzen des Kennworts funktionieren nur als Daten in die entsprechenden Felder im Verzeichnis ordnungsgemäß formatiert sind. Erfahren Sie weitere über Informationen wie Daten durch das Zurücksetzen von Kennwörtern verwendet werden.

 - **F: kann ich synchronisieren oder festlegen Authentifizierung Telefon, Felder Authentifizierung E-Mail oder Telefon-Authentifizierung für Benutzer?**

 > **A:** Aktuell nicht, aber wir diese Fähigkeit zu aktivieren.

 - **F: Woher weiß registrierungsportal die Optionen für Benutzer?**

 > **A:** Kennwort zurücksetzen registrierungsportal zeigt nur den Optionen, die Sie im Abschnitt Kennwort zurücksetzen Benutzerrichtlinie Registerkarte Konfigurieren des Verzeichnisses aktiviert haben. Dies bedeutet, dass wenn Sie nicht, z. B. Fragen, aktivieren dann Benutzer sich für diese Option nicht.

 - **F: wann Benutzer als registriert?**

 > **A:** Ein Benutzer gilt registriert, wenn er mindestens hat N Teile der Authentifizierungsinformationen definiert, wobei N die Anzahl der Authentifizierung Methoden erforderlich ist, die in [Azure-Verwaltungsportal](https://manage.windowsazure.com)festgelegt. Informationen finden Sie unter Anpassen Kennwort zurücksetzen Benutzerrichtlinie.


## <a name="password-reset"></a>Kennwort zurücksetzen

 - **F: wie lange sollte ich warten Kennwortrücksetzung eine e-Mail, SMS oder Telefonanruf erhalten?**

 > **A:** E-Mail, SMS und Telefonanrufe sollte in weniger als 1 Minute bei normalen, 5 bis 20 Sekunden ankommen. Wenn Sie in diesem Zeitraum nicht die Benachrichtigung erhalten, überprüfen Sie den Ordner junk, die Anzahl e-Mail kontaktiert der erwartet und Authentifizierungsdaten im Verzeichnis ordnungsgemäß formatiert ist. Weitere Informationen zum Formatieren von Telefonnummern und e-Mail-Adressen für das Zurücksetzen von Kennwörtern finden Sie unter Informationen wie Daten durch das Zurücksetzen von Kennwörtern verwendet werden.

 - **Welche Sprachen unterstützt Kennwort zurücksetzen?**

 > **A:** Zurücksetzen des Kennworts UI, SMS und Anrufe in derselben in Office 365 unterstützten 40 Sprachen lokalisiert. Sind: Arabisch, Bulgarisch, vereinfachtes Chinesisch, traditionell, Kroatisch, Tschechisch, Dänisch, Niederländisch, Englisch, Estnisch, Finnisch, Französisch, Deutsch, Griechisch, Hebräisch, Hindi, Ungarisch, Indonesisch, Italienisch, Japanisch, Kasachisch, Koreanisch, Lettisch, Litauisch, Malaiisch (Malaysia), Norwegisch (Bokmål), Polnisch, Portugiesisch (Brasilien), Portugiesisch (Portugal), Rumänisch, Russisch, Serbisch (Lateinisch), Slowakisch, Slowenisch, Spanisch, Schwedisch, Thai, Türkisch, Ukrainisch, und Vietnamesisch.

 - **F: welche Teile Kennwort zurücksetzen Erfahrung bei Organisationseinheiten in meinem Verzeichnis festgelegt branded Abrufen der Registerkarte Konfigurieren?**

 > **A:** Kennwort zurücksetzen Portal zeigen Ihre Organisation Logo und können Sie auch den Kontakt konfigurieren den Link Administrator auf eine benutzerdefinierte e-Mail- oder URL. E-Mails von Zurücksetzen des Kennworts gesendet wird Ihrer Organisation Logo, Farben (in diesem Fall Rot) Name im Hauptteil der e-Mail und Namen angepasst. Finden Sie ein Beispiel mit allen branded unten. Lesen Sie weitere Kennwortrücksetzung anpassen Erscheinungsbild.

  ![][001]

 - **F: wie kann meine Benutzer dazu, wo Sie ihre Kennwörter zurücksetzen informieren?**

 > **A:** Sie können die Benutzer direkt an https://passwordreset.microsoftonline.com senden, oder weisen Sie sie auf die die Verknüpfung finden Sie auf der Anmeldeseite jede Schule oder Arbeit-ID kann nicht zugegriffen werden. Sie können gerne veröffentlichen diese Hyperlinks (oder URL leitet Sie erstellen) überall den Benutzern zugänglich ist.

 - **F: verwenden kann ich diese Seite von einem mobilen Gerät?**

 > **A:** Ja, funktioniert diese Seite auf mobilen Geräten.

 - **F: unterstützen entsperren lokalen active Directory-Konten Sie Benutzer ihre Kennwörter zurücksetzen?**

 > **A:** Ja, wenn Benutzer ihr Kennwort und Kennwort Rückschreiben zurückgesetzt wurde in allen Versionen von Azure AD Connect oder Versionen von Azure AD Sync 1.0.0485.0222 bereitgestellten oder höher des Benutzerkontos werden automatisch entsperrt, wenn Benutzer ihr Kennwort zurückgesetzt.

 - **F: wie kann ich Kennwortrücksetzdiskette direkt in Meine Benutzer desktop anmelden integrieren?**

 > **A:** Dies kann nicht heute. Wenn Sie unbedingt diese Funktion benötigen, Azure AD Premium Kunde können Sie jedoch Microsoft Identity Manager kostenlos installieren und Bereitstellen die lokalen Kennwort zurücksetzen Lösung darin zu dieser Anforderung.

 - **F: werden kann verschiedene Fragen für unterschiedliche Gebietsschemas festgelegt?**

 > **A:** Nein, dies ist heute nicht möglich, aber wir werden es berücksichtigen.

 - **F: können wie viele Fragen wir für die Authentifizierungsoption Fragen?**

 > **A:** Sie können bis zu 20 benutzerdefinierte Sicherheitsfragen in [Azure-Verwaltungsportal](https://manage.windowsazure.com).

 - **F: möglicherweise wie lange Fragen?**

 > **A:** Fragen können zwischen 3 und 200 Zeichen lang sein.

 - **F: möglicherweise wie lange Antworten auf Fragen?**

 > **A:** Antworten können 3 bis 40 Zeichen lang sein.

 - **Es gibt doppelte Antworten auf Sicherheitsfragen abgelehnt?**

 > **A:** Ja, lehnen wir doppelte Antworten auf Fragen.

 - **F: kann ein Benutzer mehrere gleiche Sicherheitsfrage registrieren?**

 > **A:** Nein, sobald ein Benutzer eine bestimmte Frage registriert, kann er nicht für die Frage erneut registrieren.

 - **F: ist es möglich, eine Mindestanzahl von Sicherheitsfragen für die Registrierung und Zurücksetzen?**

 > **A:** Ja, kann ein Limit für Registrierung und Zurücksetzen festgelegt werden. 3-5 Sicherheitsfragen für die Registrierung und möglicherweise 3-5 zurücksetzen müssen.

 - **F: Wenn ein Benutzer mehr als die maximale Anzahl von Fragen zurücksetzen registriert hat, sind Sicherheitsfragen wie beim Zurücksetzen ausgewählt?**

 > **A:** N Fragen werden zufällig aus der Gesamtzahl der Fragen ausgewählt, denen Benutzer, registriert hat, wobei N die minimale Anzahl von Fragen zum Zurücksetzen des Kennworts ist. Z. B. wenn ein Benutzer 5 Sicherheitsfragen registriert, aber nur 3 zurücksetzen müssen, wird die 5 3 nach dem Zufallsprinzip ausgewählt und Zeitpunkt Zurücksetzen angezeigt. Erhält der Benutzer die Antworten auf die Fragen falsch, erneut geschieht Auswahl Frage hammering verhindern.

 - **F: Sie verhindern, dass Benutzer versuchen oft in kurzer Zeit zurückgesetzt?**

 > **A:** Ja, es gibt einige Sicherheitsfunktionen in das Zurücksetzen von Kennwörtern. Benutzer können nur 5 zurücksetzen Kennworteingaben innerhalb einer Stunde für 24 Stunden gesperrt versuchen. Benutzer können nur eine Telefonnummer 5 Mal innerhalb einer Stunde für 24 Stunden gesperrt überprüfen versuchen. Benutzer können nur eine einzige Methode 5 Mal innerhalb einer Stunde für 24 Stunden gesperrt versuchen.

 - **F: wie lange e-Mail- und SMS einmaligen Code gültig sind?**

 > **A:** Die Gültigkeitsdauer der Sitzung für das Zurücksetzen von Kennwörtern wird 105 Minuten. Dies bedeutet, das vom Anfang des Kennworts Vorgang zurückgesetzt, muss 105 Minuten das Kennwort zurückzusetzen. E-Mail- und SMS-einmalkennung sind nach Ablauf dieses Zeitraums ungültig.


## <a name="password-management-reports"></a>Kennwort-Management-Berichte

 - **F: dauert wie lange Daten auf Kennwort-Verwaltungsberichten?**

 > **A:** Daten erscheinen in der Kennwort-Verwaltungsberichten innerhalb von 5 bis 10 Minuten. Es manchmal dauert bis zu einer Stunde angezeigt werden.

 - **F: wie kann Kennwort Verwaltungsberichte filtern?**

 > **A:** Filtern Verwaltungsberichte Kennwort durch kleine Lupe extreme rechts der Spaltenüberschriften auf den oberen Rand des Berichts klicken (siehe Abbildung). Wenn Sie umfangreichere filtern möchten, können Sie den Bericht nach excel und Erstellen einer Pivottabelle.

  ![][002]

 - **Was ist die maximale Anzahl der Ereignisse in Verwaltungsberichte Kennwort gespeichert werden?**

 > **A:** Bis zu 1.000 Kennwort werden in Verwaltungsberichte Kennwort zurücksetzen oder das Kennwort zurücksetzen Registrierung Ereignisse gespeichert.  Wir arbeiten, um diese Nummer, um weitere Ereignisse zu erweitern.

 - **F: wie weit Verwaltungsberichte Kennwort tun?**

 > **A:** Kennwort-Verwaltungsberichten anzeigen Operationen innerhalb der letzten 30 Tage Wir untersuchen derzeit wie dies länger. Jetzt möchten Sie diese Daten archivieren können Sie Berichte in regelmäßigen Abständen herunterladen und an einem anderen Speicherort speichern.

 - **F: Gibt es eine maximale Anzahl Zeilen in Kennwort-Verwaltungsberichten aufgeführt werden können?**

 > **A:** Ja, maximal 1.000 Zeilen auf angezeigt Berichte Password Management, ob sie in der Benutzeroberfläche angezeigt werden oder heruntergeladen werden. Wir untersuchen derzeit diesen Grenzwert zu erhöhen.

 - **F: Gibt es eine API zum Zurücksetzen von Kennwörtern oder Registrierung Daten zugreifen?**

 > **A:** Ja, finden Sie die folgende Dokumentation wie das Zurücksetzen des Kennworts reporting-Datenstrom zugreifen können.  [Erfahren Sie auf Kennwort Ereignisberichterstattung programmgesteuert zurückzusetzen](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).

## <a name="password-writeback"></a>Kennwort Rückschreiben
 - **F: wie funktioniert Kennwort Rückschreiben im Hintergrund?**

 > **A:** Beim Rückschreiben Kennwort auch ermöglichen Vorgänge, wie das System in Ihrer lokalen Umgebung Daten durchläuft eine ausführliche Erläuterung finden Sie unter [wie Kennwort Rückschreiben funktioniert](active-directory-passwords-learn-more.md#how-password-writeback-works) . Finden Sie unter [Kennwort das Rückschreiben Sicherheitsmodell](active-directory-passwords-learn-more.md#password-writeback-security-model) wie Kennwort Rückschreiben um zu erfahren, wie wir sicherstellen, dass Kennwort Rückschreiben einer sehr sicheren Dienst ist.

 - **F: dauert wie lange Kennwort Rückschreiben arbeiten?  Gibt es eine Verzögerung Synchronisierung wie mit Kennwort Hash synchronisieren?**

 > **A:** Kennwort Rückschreiben ist sofort. Es ist eine synchrone Pipeline grundlegend anders als Hash Kennwortsynchronisation funktioniert. Rückschreiben Kennwort ermöglicht Benutzern Echtzeit Feedback über den Erfolg ihrer Kennwort zurücksetzen oder Änderung. Die durchschnittliche Zeit für das erfolgreiche Rückschreiben Kennwort ist unter 500 ms.

 - **F: funktioniert welche Konten Kennwort Rückschreiben für**

 > **A:** Kennwort Rückschreiben für verbundene und Kennwort Hash Sync verdächtigem Benutzer.

 - **F: Kennwort Rückschreiben meiner Domäne Kennwortrichtlinien erzwingen?**

 > **A:** Ja, erzwingt Kennwort Rückschreiben Kennwortalter, Komplexität, Filter und der Einschränkung, die Sie auf Kennwörter in der lokalen Domäne setzen.

 - **F: ist das Rückschreiben Kennwort sicher?  Wie kann ich sicher sein, dass ich gehackt werden nicht?**

 > **A:** Kennwort Rückschreiben ist sehr sicher. Um mehr über 4 Sicherheitsebenen Kennwort Rückschreiben Dienst implementiert, [Kennwort Rückschreiben Sicherheitsmodell](active-directory-passwords-learn-more.md#password-writeback-security-model) wie Kennwort Rückschreiben Works Auschecken.




## <a name="links-to-password-reset-documentation"></a>Links zu Kennwort zurücksetzen Dokumentation
Unten finden Sie Links zu allen Dokumentationsseiten Azure AD Kennwort zurücksetzen:

* **Sind Sie hier beim Anmelden Probleme auftreten?** In diesem Fall [hier wie ändern und Ihr eigenes Kennwort](active-directory-passwords-update-your-own-password.md).
* [**Funktionsweise**](active-directory-passwords-how-it-works.md) - erfahren Sie mehr über die sechs verschiedenen Komponenten des Dienstes und jeder
* [**Erste Schritte**](active-directory-passwords-getting-started.md) - erfahren Sie, wie Sie Benutzern zurücksetzen und ihre Cloud oder lokalen Kennwörter ändern
* [**Anpassen**](active-directory-passwords-customize.md) - Informationen zum Anpassen von Aussehen und Verhalten und Verhalten des Diensts zu Ihrem Unternehmen
* [**Best Practices**](active-directory-passwords-best-practices.md) - so schnell bereitstellen und Kennwörter in Ihrer Organisation verwalten
* Sie [**erfahren**](active-directory-passwords-get-insights.md) – erfahren Sie mehr über unsere integrierten reporting-Funktionen
* [**Problembehandlung**](active-directory-passwords-troubleshoot.md) – erfahren Sie schnell Probleme mit dem Dienst
* [**Erfahren Sie mehr**](active-directory-passwords-learn-more.md) - gehen tief in die technischen Einzelheiten der Funktionsweise des Dienstes


[001]: ./media/active-directory-passwords-faq/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-faq/002.jpg "Image_002.jpg"
