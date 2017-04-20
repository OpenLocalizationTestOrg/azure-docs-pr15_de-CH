<properties
    pageTitle="Problembehandlung: Azure AD Password Management | Microsoft Azure"
    description="Allgemeine Problembehandlung die Schritte für Azure AD Password Management, einschließlich zurücksetzen ändern Rückschreiben, Registrierung und welche Informationen wann Hilfe."
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
    ms.date="08/12/2016"
    ms.author="asteen"/>

# <a name="how-to-troubleshoot-password-management"></a>Beheben von Kennwort-Management

> [AZURE.IMPORTANT] **Sind Sie hier beim Anmelden Probleme auftreten?** In diesem Fall [hier wie ändern und Ihr eigenes Kennwort](active-directory-passwords-update-your-own-password.md).

Bei Problemen mit Passwort-Management sind wir hier helfen. Die meisten Probleme in ausführen können können mit wenigen Schritten zur Problembehandlung die unten Sie lesen können die Bereitstellung Problembehandlung gelöst werden:

* [**Informationen enthalten sein, wenn Sie Hilfe benötigen**](#information-to-include-when-you-need-help)
* [**Probleme mit Kennwort Verwaltungskonfiguration in Azure-Verwaltungsportal**](#troubleshoot-password-reset-configuration-in-the-azure-management-portal)
* [**Probleme mit Passwort Management in Azure-Verwaltungsportal**](#troubleshoot-password-management-reports-in-the-azure-management-portal)
* [**Probleme mit dem Kennwort zurücksetzen Registrierungsportal**](#troubleshoot-the-password-reset-registration-portal)
* [**Probleme mit dem Kennwort zurücksetzen Portal**](#troubleshoot-the-password-reset-portal)
* [**Probleme mit Kennwort Rückschreiben**](#troubleshoot-password-writeback)
  - [Kennwort Rückschreiben Ereignisprotokoll-Fehlercodes](#password-writeback-event-log-error-codes)
  - [Kennwort Rückschreiben Verbindungsprobleme](#troubleshoot-password-writeback-connectivity)

Wenn Sie die folgenden Problembehandlungsschritte haben, laufen noch Probleme eine Frage in den [Foren Azure AD](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD) oder Support und wir nehmen sich das Problem so schnell wie möglich.

## <a name="information-to-include-when-you-need-help"></a>Informationen enthalten sein, wenn Sie Hilfe benötigen

Wenn Sie das Problem mit der folgenden Anleitung lösen können, können Sie einen Support-Mitarbeiter kontaktieren. Kundenbetreuung, wird empfohlen, die folgenden Angaben enthalten:

 - **Allgemeine Beschreibung des Fehlers** – welche genauen Fehlermeldung hat den Benutzer sehen?  Wurde keine Fehlermeldung, beschreiben Sie unerwartete Verhalten im Detail bemerkt.
 - **Seite** -Seite waren Sie sah den Fehler (auch den URL)?
 - **Datum / Uhrzeit / Timezone** – was genau Datum und Uhrzeit den Fehler sah (einschließlich die Zeitzone)?
 - **Support-Code** – was den unterstützenden Code generiert, wenn der Benutzer den Fehler (zu finden, den Fehler reproduzieren klicken Sie am unteren Bildschirmrand Code unterstützen und Supportmitarbeiter senden führt GUID) sah.
   - Sind auf einer Seite ohne Unterstützungscode unten drücken Sie F12 und SID und CID suchen und diese zwei Ergebnisse an Supportmitarbeiter.

    ![][001]

 - **Benutzer-ID** – was die ID des Benutzers, den Fehler (z. B. sahuser@contoso.com)?
 - **Informationen über den Benutzer** wurde verbundene Benutzer, Kennworthash synchronisiert, nur Cloud?  Haben der Benutzer eine Lizenz AAD Premium oder Basic AAD zugewiesen
 - **Anwendungsereignisprotokoll** – Wenn Sie Kennwort Rückschreiben und der Fehler ist in der lokalen Infrastruktur und bitte Komprimieren das Ereignisprotokoll der Anwendung von Azure AD Connect-Server eine Anfrage senden.

Diese Informationen helfen uns, Ihr Problem möglichst schnell zu lösen.


## <a name="troubleshoot-password-reset-configuration-in-the-azure-management-portal"></a>Problembehandlung bei Kennwort zurücksetzen Konfiguration in Azure-Verwaltungsportal
Wenn ein beim Konfigurieren von Passwort Fehler, könnten Sie durch folgt Problembehandlung beheben:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Fehlerfall</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Welcher Fehler sieht ein Benutzer?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Lösung</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Im Abschnitt <strong>Kennwort zurücksetzen Benutzerrichtlinie </strong>Registerkarte <strong>Konfigurieren</strong> im Azure-Verwaltungsportal nicht angezeigt werden.</p>
            </td>
            <td>
              <p>Im Abschnitt <strong>Kennwort zurücksetzen Benutzerrichtlinie </strong>ist nicht auf die Registerkarte <strong>Konfigurieren</strong> der Azure-Verwaltungsportal.</p>
            </td>
            <td>
              <p>Dies kann auftreten, wenn Sie keine Lizenz AAD Premium oder Basic AAD der Administrator diesem Vorgang zugeordnet. </p>
              <p>Um dieses Problem zu beheben, das betreffende Administratorkonto durch Navigieren zur Registerkarte <strong>Lizenzen</strong> eine Lizenz AAD Premium oder Basic AAD zuweisen und wiederholen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Keinen im Abschnitt <strong>Kennwort zurücksetzen Benutzerrichtlinie</strong> angezeigt, die in der Dokumentation beschrieben werden.</p>
            </td>
            <td>
              <p>Abschnitt <strong>Benutzerrichtlinie Kennwort zurücksetzen </strong>sichtbar, jedoch das einzige Flag wird angezeigt das Flag <strong>Für Passwort Benutzer aktiviert</strong> .</p>
            </td>
            <td>
              <p>Der Rest der Benutzeroberfläche erscheint beim Wechseln der <strong>Benutzer aktiviert für Passwort</strong> Flag <strong>Ja.</strong></p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Eine bestimmte Option nicht angezeigt.</p>
            </td>
            <td>
              <p>Beispielsweise sehe ich nicht die <strong>Anzahl der Tage, bevor ein Benutzer ihre Kontaktdaten bestätigen muss</strong> Option Wenn ich Richtlinienabschnitt <strong>Benutzer Kennwort zurücksetzen</strong> (oder andere Beispiele dasselbe Problem) durchsuchen.</p>
            </td>
            <td>
              <p>Viele Elemente der Benutzeroberfläche werden ausgeblendet, bis sie benötigt werden. Versuchen Sie, alle Optionen auf der Seite zu aktivieren, wenn Sie anzeigen möchten.</p>
              <p>Weitere Informationen über alle verfügbaren Steuerelemente finden Sie unter <a href="active-directory-passwords-customize.md#password-management-behavior">Kennwort Verhalten</a> .</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Die Konfigurationsoption <strong>Wieder Kennwörter lokal schreiben</strong> nicht angezeigt werden.</p>
            </td>
            <td>
              <p>Die Option <strong>Wieder Kennwörter lokal schreiben</strong> ist nicht unter die Registerkarte <strong>Konfigurieren</strong> der Azure-Verwaltungsportal</p>
            </td>
            <td>
              <p>Diese Option ist nur sichtbar, wenn Sie Azure AD-Verbindung heruntergeladen und Kennwort Rückschreiben konfiguriert haben. Sie danach die Option angezeigt und ermöglicht das Aktivieren oder Deaktivieren des Rückschreibens aus der Cloud.</p>
              <p>Weitere Informationen hierzu finden Sie unter <a href="active-directory-passwords-getting-started.md#step-2-enable-password-writeback-in-azure-ad-connect">Aktivieren Kennwort Rückschreiben in Azure AD verbinden</a> .</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-management-reports-in-the-azure-management-portal"></a>Problembehandlung bei Kennwort Verwaltungsberichte in Azure-Verwaltungsportal
Wenn ein beim Kennwort Management-Berichte verwenden Fehler, könnten Sie durch folgt Problembehandlung beheben:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Fehlerfall</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Welcher Fehler sieht ein Benutzer?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Lösung</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Passwort Managementberichte nicht angezeigt werden.</p>
            </td>
            <td>
              <p><strong>Passwort-Aktivität</strong> und <strong>Passwort Registrierungsaktivität</strong> Berichte werden nicht <strong>Aktivitätsprotokoll</strong> Berichte Registerkarte <strong>Berichte</strong> angezeigt.</p>
            </td>
            <td>
              <p>Dies kann auftreten, wenn Sie keine Lizenz AAD Premium oder Basic AAD der Administrator diesem Vorgang zugeordnet. </p>
              <p>Um dieses Problem zu beheben, das betreffende Administratorkonto durch Navigieren zur Registerkarte <strong>Lizenzen</strong> eine Lizenz AAD Premium oder Basic AAD zuweisen und wiederholen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Registrierung der Benutzer anzeigen mehrmals</p>
            </td>
            <td>
              <p>Wenn Benutzer alternative e-Mail, Mobiltelefon und Sicherheitsfragen registriert, werden sie als separate Zeilen anstelle einer einzelnen Zeile.</p>
            </td>
            <td>
              <p>Derzeit als Benutzer registriert, kann nicht davon ausgegangen, dass sie alles auf der Registrierungsseite registrieren. Daher protokollieren wir derzeit jedes einzelne Daten als ein separates Ereignis registriert ist.</p>
              <p>Möchten Sie diese Daten, können die Daten als PivotTable in excel zu mehr Flexibilität Sie herunterladen und öffnen.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-the-password-reset-registration-portal"></a>Problembehandlung bei Kennwort zurücksetzen registrierungsportal
Wenn ein beim Zurücksetzen des Kennworts für einen Benutzer registrieren Fehler, könnten Sie durch die Schritte zur Problembehandlung unter beheben:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Fehlerfall</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Welcher Fehler sieht ein Benutzer?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Lösung</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Verzeichnis ist für das Zurücksetzen von Kennwörtern nicht aktiviert.</p>
            </td>
            <td>
              <p>Ihr Administrator hat nicht diese Funktion aktiviert.</p>
            </td>
            <td>
              <p>Wechseln Sie das Flag <strong>Für Passwort Benutzer aktiviert</strong> auf <strong>Ja</strong> , und drücken Sie <strong>Speichern</strong> auf der Registerkarte Verzeichnis Azure-Verwaltungsportal. Ein Azure AD Premium oder Basic der Administrator diesem Vorgang zugeordnete Lizenz erforderlich.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Benutzer verfügt nicht über eine zugewiesene Lizenz AAD Premium oder Basic AAD</p>
            </td>
            <td>
              <p>Ihr Administrator hat nicht diese Funktion aktiviert.</p>
            </td>
            <td>
              <p>Dem Benutzer Registerkarte <strong>Lizenzen</strong> in Azure-Verwaltungsportal Azure AD Premium oder Basic Azure AD-Lizenz zuweisen. Ein Azure AD Premium oder Basic der Administrator diesem Vorgang zugeordnete Lizenz erforderlich.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Fehler beim Verarbeiten der Anforderung</p>
            </td>
            <td>
              <p>Benutzer eine Fehlermeldung:</p>
              <p>

              </p>
              <p>Fehler beim Verarbeiten der Anforderung </p>
              <p>Beim Zurücksetzen des Kennworts.</p>
            </td>
            <td>
              <p>Dies kann durch viele Probleme verursacht, aber im Allgemeinen wird dieser Fehler verursacht, durch einen Dienst Ausfall oder Konfiguration Problem, das nicht aufgelöst werden kann. </p>
              <p>Wenn dieser Fehler angezeigt und es wird Ihr Unternehmen beeinträchtigen, Support kontaktieren und wir helfen Ihnen so schnell wie möglich. Finden Sie <a href="#information-to-include-when-you-need-help">Informationen, wenn Sie Hilfe benötigen</a> , Support-Engineer bei einer schnellen Lösung gewährleisten.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-the-password-reset-portal"></a>Problembehandlung bei Kennwort zurücksetzen portal
Wenn ein beim Zurücksetzen eines Kennworts für einen Benutzer Fehler, könnten Sie durch die Schritte zur Problembehandlung unter beheben:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Fehlerfall</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Welcher Fehler sieht ein Benutzer?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Lösung</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Verzeichnis ist für das Zurücksetzen von Kennwörtern nicht aktiviert.</p>
            </td>
            <td>
              <p>Ihr Konto ist für das Zurücksetzen von Kennwörtern nicht aktiviert.</p>
              <p>Es tut uns leid, aber der Administrator nicht eingerichtet Ihr Konto für diesen Dienst. </p>
              <p>

              </p>
              <p>Wenn Sie möchten, erhalten wir einen Administrator in Ihrer Organisation, um Ihr Kennwort zurückzusetzen.</p>
            </td>
            <td>
              <p>Wechseln Sie das Flag <strong>Für Passwort Benutzer aktiviert</strong> auf <strong>Ja</strong> , und drücken Sie <strong>Speichern</strong> auf der Registerkarte Verzeichnis Azure-Verwaltungsportal. Ein Azure AD Premium oder Basic der Administrator diesem Vorgang zugeordnete Lizenz erforderlich.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Benutzer verfügt nicht über eine zugewiesene Lizenz AAD Premium oder Basic AAD</p>
            </td>
            <td>
              <p>Während wir nicht Administrator Kennwörter automatisch zurücksetzen können, erreichen wir Ihrer Organisation Administrator durchführen.</p>
            </td>
            <td>
              <p>Dem Benutzer Registerkarte <strong>Lizenzen</strong> in Azure-Verwaltungsportal Azure AD Premium oder Basic Azure AD-Lizenz zuweisen. Ein Azure AD Premium oder Basic der Administrator diesem Vorgang zugeordnete Lizenz erforderlich.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Verzeichnis für das Zurücksetzen von Kennwörtern aktiviert, aber Benutzer hat Authentifizierungsinformationen fehlt oder ist ungültig</p>
            </td>
            <td>
              <p>Ihr Konto ist für das Zurücksetzen von Kennwörtern nicht aktiviert.</p>
              <p>Es tut uns leid, aber der Administrator nicht eingerichtet Ihr Konto für diesen Dienst. </p>
              <p>

              </p>
              <p>Wenn Sie möchten, erhalten wir einen Administrator in Ihrer Organisation, um Ihr Kennwort zurückzusetzen.</p>
            </td>
            <td>
              <p>Stellen Sie sicher, dass Benutzer Daten auf Datei im Verzeichnis zunächst richtig gebildet wurde. Finden Sie <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">Daten durch das Zurücksetzen von Kennwörtern verwendet</a> Informationen Authentifizierungsinformationen im Verzeichnis so konfigurieren, dass Benutzer diese Fehlermeldung nicht angezeigt.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Verzeichnis für das Zurücksetzen von Kennwörtern aktiviert, aber ein Benutzer nur hat eine Kontaktdaten hinterlegt bei Richtlinie zwei Schritte erforderlich</p>
            </td>
            <td>
              <p>Ihr Konto ist für das Zurücksetzen von Kennwörtern nicht aktiviert.</p>
              <p>Es tut uns leid, aber der Administrator nicht eingerichtet Ihr Konto für diesen Dienst. </p>
              <p>

              </p>
              <p>Wenn Sie möchten, erhalten wir einen Administrator in Ihrer Organisation, um Ihr Kennwort zurückzusetzen.</p>
            </td>
            <td>
              <p>Sicherstellen Sie, dass der Benutzer mindestens zwei konfigurierte Kontaktmethoden (z.B. Mobiltelefon und Telefon) vor. Finden Sie <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">Daten durch das Zurücksetzen von Kennwörtern verwendet</a> Informationen Authentifizierungsinformationen im Verzeichnis so konfigurieren, dass Benutzer diese Fehlermeldung nicht angezeigt.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Verzeichnis für das Zurücksetzen von Kennwörtern, aktiviert ist und Benutzer konfiguriert jedoch Benutzer kann keine Verbindung hergestellt werden </p>
            </td>
            <td>
              <p>Achtung!  Während der Kontaktaufnahme ist einen unerwarteten Fehler aufgetreten.</p>
            </td>
            <td>
              <p>Dies möglicherweise temporären Dienst Fehler oder falsch Daten, die wir nicht richtig erkennen kann. Wenn der Benutzer 10 Sekunden erneut versuchen wartet und "Systemadministrator" Link angezeigt wird. Versuchen Sie noch einmal auf versenden den Aufrufs erneut während "Systemadministrator" auf einem Formular Benutzer, Kennwort oder globalen Administratoren (in dieser Reihenfolge) fordert eine Kennwortrücksetzdiskette für dieses Benutzerkonto ausgeführt werden e-Mail wird.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Benutzer erhält nie Kennwortrücksetzung SMS oder Telefonanruf</p>
            </td>
            <td>
              <p>Benutzer klickt auf "Text me" oder "Mich anrufen" und dann geschieht nichts.</p>
            </td>
            <td>
              <p>Möglicherweise eine fehlerhafte Telefonnummer im Verzeichnis aus. Stellen Sie sicher, dass die Telefonnummer im Format "+ ccc XxxyyyzzzzXeeee". Weitere Informationen zum Formatieren von Telefon anzeigen bestehen beim Zurücksetzen des Kennworts <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">welche Daten durch das Zurücksetzen von Kennwörtern</a></p>
              <p>Auch wenn Sie im Verzeichnis angeben (sie werden entfernt, bevor der Dispatch) benötigen Sie eine Erweiterung für den betreffenden Benutzer weitergeleitet werden, beachten Sie, Kennwortrücksetzung Extensions nicht unterstützt. Verwenden Sie eine Zahl ohne Erweiterung oder die Telefonnummer in Ihre PBX Erweiterung integrieren.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Benutzer erhalten keine Kennwort zurücksetzen e-Mail</p>
            </td>
            <td>
              <p>Benutzer klickt auf "email" und dann geschieht nichts.</p>
            </td>
            <td>
              <p>Die häufigste Ursache für dieses Problem ist, dass die Nachricht einen Spam-Filter zurückgewiesen wird. Überprüfen Sie Ihre Spam, junk oder gelöschten Ordner für e-Mail.</p>
              <p>Sicherstellen, dass die richtigen e-Mails sind für die Nachricht... viele Leute haben sehr ähnliche e-Mail-Adressen und Überprüfung des richtigen Posteingangs für die Nachricht. Wenn keine dieser Optionen arbeiten, es ist auch möglich, dass die e-Mail-Adresse im Verzeichnis enthält, überprüfen, um sicherzustellen, dass die e-Mail-Adresse richtig ist, und versuchen Sie es erneut. Erfahren Sie mehr über die Formatierung von e-Mail finden Sie Adressen für das Zurücksetzen von Kennwörtern <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">Daten durch das Zurücksetzen von Kennwörtern verwendet</a>.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Ich habe eine Kennwortrichtlinie zurücksetzen, aber Admin-Kontos Zurücksetzen des Kennworts verwendet, wird die Richtlinie nicht angewendet</p>
            </td>
            <td>
              <p>Administratorkonten Zurücksetzung ihres Kennworts finden Sie unter Optionen für das Zurücksetzen von Kennwörtern, e-Mail und Mobiltelefon, egal welche Registerkarte <strong>Konfigurieren</strong> im Abschnitt <strong>Kennwort zurücksetzen Benutzerrichtlinie</strong> festgelegt ist aktiviert.</p>
            </td>
            <td>
              <p>Im Abschnitt <strong>Kennwort zurücksetzen Benutzerrichtlinie</strong> Registerkarte <strong>Konfigurieren</strong> konfigurierten Optionen gelten nur für Benutzer in Ihrer Organisation.</p>
              <p>Microsoft verwaltet und Steuerelemente das Administratorkennwort zurücksetzen Richtlinie um die höchste Sicherheitsstufe zu gewährleisten</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Benutzer, die versuchen, das Zurücksetzen zu oft an einem Tag verhindert</p>
            </td>
            <td>
              <p>Benutzer eine Fehlermeldung:</p>
              <p>

              </p>
              <p>Verwenden Sie eine andere Option.</p>
              <p>Sie haben versucht, Ihr Konto zu oft in den letzten 1 Stunde(n) zu überprüfen. Aus Sicherheitsgründen müssen Sie warten Sie 24 Stunden, bevor Sie erneut versuchen können. </p>
              <p>Wenn Sie möchten, erhalten wir einen Administrator in Ihrer Organisation, um Ihr Kennwort zurückzusetzen.</p>
            </td>
            <td>
              <p>Wir implementieren eine automatische Einschränkungsmechanismus Block Benutzer ihre Kennwörter zurücksetzen in kurzer Zeit zu oft versucht. Dies tritt auf, wenn:</p>
              <ol class="ordered">
                <li>
Benutzer versucht, eine 5-Mal in einer Stunde überprüfen.<br\><br\></li>
                <li>
Benutzer möchte Fragen Schranke 5 Mal in einer Stunde verwenden.<br\><br\></li>
                <li>
Benutzer versucht, ein Passwort für das gleiche Benutzerkonto 5 Mal in einer Stunde.<br\><br\></li>
              </ol>
              <p>Um dieses Problem zu beheben, der Benutzer warten Sie 24 Stunden nach dem letzten Versuch und Benutzer werden dann das Kennwort zurückzusetzen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Benutzer einen Fehler auf, wenn seine Telefonnummer überprüfen</p>
            </td>
            <td>
              <p>Beim Überprüfen einer Telefonnummer als Authentifizierungsmethode verwendet, wird dem Benutzer eine Fehlermeldung:</p>
              <p>

              </p>
              <p>Falsche Telefonnummer angegeben.</p>
            </td>
            <td>
              <p>Dieser Fehler tritt auf, wenn die eingegebene Telefonnummer nicht die Telefonnummer hinterlegt.</p>
              <p>Stellen Sie sicher, dass der Benutzer die vollständige Telefonnummer einschließlich Bereich und Land, beim Telefon-Methode für das Zurücksetzen von Kennwörtern verwenden eingeben.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Fehler beim Verarbeiten der Anforderung</p>
            </td>
            <td>
              <p>Benutzer eine Fehlermeldung:</p>
              <p>

              </p>
              <p>Fehler beim Verarbeiten der Anforderung </p>
              <p>Beim Zurücksetzen des Kennworts.</p>
            </td>
            <td>
              <p>Dies kann durch viele Probleme verursacht, aber im Allgemeinen wird dieser Fehler verursacht, durch einen Dienst Ausfall oder Konfiguration Problem, das nicht aufgelöst werden kann. </p>
              <p>Wenn dieser Fehler angezeigt und es wird Ihr Unternehmen beeinträchtigen, Support kontaktieren und wir helfen Ihnen so schnell wie möglich. Finden Sie <a href="#information-to-include-when-you-need-help">Informationen, wenn Sie Hilfe benötigen</a> , Support-Engineer bei einer schnellen Lösung gewährleisten.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-writeback"></a>Problembehandlung bei Kennwort Rückschreiben
Sollten Fehler aktivieren, deaktivieren oder mithilfe von Rückschreiben Kennwort könnten Sie durch folgt Problembehandlung beheben:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Fehlerfall</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Welcher Fehler sieht ein Benutzer?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Lösung</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Allgemeine Onboarding und Start-Fehler</p>
            </td>
            <td>
              <p>Kennwort zurücksetzen Dienst startet nicht lokal mit Fehler 6800 im Anwendungsereignisprotokoll Azure AD Connect Computer.</p>
              <p>

              </p>
              <p>Onboarding verbundene oder Kennworthash können nicht synchronisierte Benutzer ihre Kennwörter zurücksetzen.</p>
            </td>
            <td>
              <p>Beim Kennwort Rückschreiben aktiviert ist, wird das Synchronisierungsmodul mit Onboarding-Clouddienst Rückschreiben Bibliothek ausführen die Konfiguration (Onboarding) aufrufen. Fehler im Ereignisprotokoll im Ereignisprotokoll Azure AD Verbinden des Computers führt alle Fehler während Onboarding oder WCF-Endpunkt für das Rückschreiben Kennwort ab.</p>
              <p>Beim Neustart des Dienstes ADSync Rückschreiben konfiguriert wurde, wird der WCF-Endpunkt gestartet werden. Allerdings schlägt der Start des Endpunkts wir einfach Ereignis 6800 und Dienststart synchronisieren können. Dieses Ereignis bedeutet, dass das Kennwort Rückschreiben Endpunkt nicht gestartet wurde. Ereignisprotokoll-Details für dieses Ereignis (6800) mit Einträgen im Ereignisprotokoll generieren PasswordResetService Warum Endpunkt konnte nicht gestartet werden gekennzeichnet werden. Überprüfen Sie diese Fehler im Ereignisprotokoll und starten Sie Azure AD-Verbindung neu Wenn Rückschreiben Kennwort immer noch nicht funktioniert. Wenn das Problem weiterhin besteht, versuchen Sie, deaktivieren und reaktivieren Kennwort Rückschreiben.</p>
            </td>
          </tr>
                    <tr>
            <td>
              <p>Wenn ein Benutzer versucht, ein Passwort oder ein Konto mit aktiviertem Rückschreiben Kennwort entsperren, schlägt der Vorgang fehl. Darüber hinaus finden Sie ein Ereignis in das Ereignisprotokoll Azure AD Verbinden mit: "Synchronisierungsmodul zurückgegebenen Fehler hr = 800700CE, Nachricht = Dateiname oder Erweiterung zu lang" nach der Sperre erfolgt.
                            </p>
            </td>
            <td>
              <p>Dies kann auftreten, wenn Sie ältere Azure AD verbinden oder Dirsync-Upgrade. Aktualisieren auf ältere Versionen von Azure AD Connect Festlegen eines Kennworts 254 Zeichen für die Azure AD-Verwaltungsagent Konto (neuere Versionen werden ein Kennwort 127 Zeichen Länge). So lange Kennwörter für Active Directory Connector-Import und Export funktionieren, aber nicht von Unlock-Vorgang unterstützt.
                            </p>
            </td>
            <td>
              <p>[Suchen Sie das Active Directory-Konto](active-directory-aadconnect-accounts-permissions.md#active-directory-account) für Azure AD Connect und Zurücksetzen des Kennworts nicht mehr als 127 Zeichen enthalten. Öffnen Sie im Menü Start **Synchronisierungsdienst** . **Connectors** navigieren und Suchen von **Active Directory Connector**. Wählen sie aus, und klicken Sie auf **Eigenschaften**. Navigieren Sie zur Seite **Anmeldeinformationen** und geben Sie das neue Kennwort ein. Wählen Sie **OK** , um die Seite zu schließen.
                            </p>
            </td>
          </tr>
                    <tr>
            <td>
              <p>Fehler beim Rückschreiben in Azure AD Connect-Installation konfigurieren.</p>
            </td>
            <td>
              <p>Im letzten Schritt des Installationsprozesses Azure AD Connect sehen Sie eine Fehlermeldung, dass Kennwort Rückschreiben konnte nicht konfiguriert werden.</p>
              <p>

              </p>
              <p>Azure AD verbinden Anwendungsereignisprotokoll enthält Fehler 32009 Text "Fehlermeldung Auth token".</p>
            </td>
            <td>
              <p>Dieser Fehler tritt in folgenden Fällen:</p>
              <ul>
                <li class="unordered">
Sie haben ein falsches Kennwort für das globale Administratorkonto zu Beginn des Installationsvorgangs Azure AD Connect angegebenen angegeben.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Sie haben versucht, Verbundbenutzer für das globale Administratorkonto angegeben zu Beginn des Installationsvorgangs Azure AD Connect verwenden.<br\><br\></li>
              </ul>
              <p>Um diesen Fehler zu beheben, stellen Sie sicher, dass Sie nicht mit einem verbundenen Konto für den globalen Administrator zu Beginn des Installationsvorgangs Azure AD Connect angegebene und das angegebene Kennwort korrekt ist.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Fehler beim Rückschreiben in Azure AD Connect-Installation konfigurieren.</p>
            </td>
            <td>
              <p>Azure AD Connect Computer Ereignisprotokoll enthält Fehler 32002 der PasswordResetService ausgelöst.</p>
              <p>

              </p>
              <p>Der Fehler lautet: "Fehler ServiceBus herstellen, der Tokenanbieter wurde kein Sicherheitstoken..."</p>
              <p>

              </p>
            </td>
            <td>
              <p>Die Ursache dieses Fehlers ist der Kennwort zurücksetzen Service in Ihrer lokalen Umgebung nicht auf das Bus-Endpunkt in der Cloud zu verbinden. Dieser Fehler wird normalerweise normalerweise durch eine Firewallregel blockiert eine ausgehende Verbindung zu einem bestimmten Port oder Web-Adresse verursacht.</p>
              <p>

              </p>
              <p>Stellen Sie sicher, dass Ihre Firewall ausgehende Verbindungen für Folgendes ermöglicht:</p>
              <ul>
                <li class="unordered">
Gesamten Datenverkehr über TCP 443 (HTTPS)<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Ausgehende Verbindungen zu <br\><br\></li>
              </ul>
              <p>

              </p>
              <p>Nach der Aktualisierung dieser Regeln Azure AD Connect Neustart und Kennwort Rückschreiben wird wieder gestartet.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Kennwort Rückschreiben Endpunkt auf-Prem nicht erreichbar</p>
            </td>
            <td>
              <p>Nachdem für einige Zeit verbundene oder Kennworthash können nicht synchronisierte Benutzer ihre Kennwörter zurücksetzen.</p>
            </td>
            <td>
              <p>In einigen seltenen Fällen fehlschlagen Kennwort Rückschreiben-Dienst neu starten Wenn Azure AD Connect neu gestartet wurde. In diesen Fällen zuerst überprüfen Sie, ob Kennwort Rückschreiben anscheinend auf Prem aktiviert. Dies kann mit dem Azure AD Connect Assistenten oder Powershell (Siehe Anleitungen Abschnitt oben). Wenn die Funktion aktiviert, wird versuchen, aktivieren oder deaktivieren die Funktion erneut über die Benutzeroberfläche oder die PowerShell. Siehe "Schritt 2: Kennwort Rückschreiben Computers Directory Sync &amp; Firewall-Regeln konfigurieren" <a href="active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords">wie Kennwort Rückschreiben aktivieren</a> für Weitere Informationen dazu.</p>
              <p>

              </p>
              <p>Wenn dies nicht funktioniert, versuchen Sie vollständig deinstallieren und neu installieren Azure AD verbinden.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Berechtigungsfehler</p>
            </td>
            <td>
              <p>Verbundene oder Kennwort-Hash Sync verdächtigem Benutzer ihre Kennwörter Siehe Zurücksetzen Fehler nach dem Kennwort, dass Fehler Service übermitteln.</p>
              <p>

              </p>
              <p>Zusätzlich bei Kennwort zurücksetzen, sehen Sie möglicherweise ein Fehler bezüglich Management Agent auf auf lokalen Ereignisprotokollen verweigert wurde.</p>
            </td>
            <td>
              <p>Wenn diese Fehler im Ereignisprotokoll angezeigt wird, bestätigen Sie, dass das AD MA-Konto (die im Assistenten zum Zeitpunkt der Konfiguration angegeben wurde) die erforderlichen Berechtigungen für das Rückschreiben Kennwort.</p>
              <p>

              </p>
              <p>Beachten Sie, dass wenn diese Berechtigung erteilt Berechtigungen über Sdprop Hintergrundtask auf dem DC müssen bis zu einer Stunde dauert. </p>
              <p>Kennwort zurücksetzen funktioniert muss die Berechtigung auf den Sicherheitsdeskriptor des Benutzerobjekts gestempelt werden, dessen Kennwort zurückgesetzt wird. Bis diese Berechtigung auf zeigt, weiterhin das Zurücksetzen von Kennwörtern bei der Zugriff verweigert.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Fehler beim Rückschreiben Kennwort von Azure AD Connect-Konfigurations-Assistenten konfigurieren </p>
            </td>
            <td>
              <p>Fehler im Assistenten beim Rückschreiben Kennwort aktivieren/deaktivieren "Kann nicht zu suchen"</p>
            </td>
            <td>
              <p>Es ist ein bekanntes Problem in der freigegebenen Version von Azure AD Connect welche in den folgenden Situationen:</p>
              <ol class="ordered">
                <li>
Azure AD Connect Mieter abc.com (Domäne bestätigt) mit Anmeldeinformationen konfigurieren. Dadurch AAD Connector mit Namen "abc.com-AAD" erstellt wird.<br\><br\></li>
                <li>
Anschließend ändern Sie dann AAD Creds für den Connector (mit alten Sync UI) (Beachten Sie, dass der gleiche Mieter aber anderen Domänennamen ist). <br\><br\></li>
                <li>
Jetzt versuchen Sie Kennwort Rückschreiben aktivieren. Der Assistent erstellt den Namen mithilfe der Anmeldeinformationen als "abc.onmicrosoft.com-AAD" und Kennwort Rückschreiben Cmdlet übergeben. Dies schlägt fehl, da gibt es keinen Connector mit diesem Namen erstellt.<br\><br\></li>
              </ol>
              <p>Dies wurde in unserem neuesten Builds behoben. Wenn Sie eine ältere Version verfügen, ist eine Lösung Powershell-Cmdlet verwenden, um das Feature aktivieren. Siehe "Schritt 2: Kennwort Rückschreiben Computers Directory Sync &amp; Firewall-Regeln konfigurieren" <a href="active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords">wie Kennwort Rückschreiben aktivieren</a> für Weitere Informationen dazu.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Nicht in spezielle Gruppen wie Domänen-Admins Passwort / Enterprise Admins usw..</p>
            </td>
            <td>
              <p>Verbundene oder Kennwort-Hash Sync verdächtigem Benutzer geschützten Gruppen gehören, und versuchen Sie, ihre Kennwörter Siehe Zurücksetzen Fehler nach dem Kennwort, dass Fehler Service übermitteln.</p>
            </td>
            <td>
              <p>Privilegierte Benutzer in Active Directory sind mit AdminSDHolder geschützt. Finden Sie unter <a href="https://technet.microsoft.com/magazine/2009.09.sdadminholder.aspx">http://technet.microsoft.com/magazine/2009.09.sdadminholder.aspx</a> Weitere Informationen. </p>
              <p>

              </p>
              <p>Dies bedeutet Sicherheitsdeskriptoren für diese Objekte werden entsprechend einer im AdminSDHolder regelmäßig überprüft und werden zurückgesetzt, wenn sie unterschiedlich sind. Zusätzlichen Berechtigungen, die für das Rückschreiben Kennwort daher nicht für solche Benutzer Durchlassen. Dies kann das Rückschreiben Kennwort funktioniert nicht für solche Benutzer führen. Folglich unterstützen nicht Verwalten von Kennwörtern für Benutzer innerhalb dieser Gruppen wir da AD-Sicherheitsmodell wird unterbrochen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Zurücksetzen Vorgänge nicht mit Objekt konnte nicht gefunden werden</p>
            </td>
            <td>
              <p>Verbundene oder Kennwort-Hash Sync verdächtigem Benutzer ihre Kennwörter Siehe Zurücksetzen Fehler nach dem Kennwort, dass Fehler Service übermitteln.</p>
              <p>

              </p>
              <p>Darüber hinaus kann bei Kennwort zurücksetzen, Fehler in Ereignisprotokollen von Azure AD Connect Service Fehlermeldung "Objekt nicht gefunden" angezeigt.</p>
            </td>
            <td>
              <p>Dieser Fehler gibt normalerweise an, dass das Synchronisierungsmodul das Benutzerobjekt im Connectorspace AAD oder verknüpften MV oder AD Connectorspace-Objekt kann nicht gefunden wird. </p>
              <p>

              </p>
              <p>Dieses Problems stellen Sie sicher, dass der Benutzer tatsächlich aus auf Prem mit AAD über die aktuelle Instanz von Azure AD Connect synchronisiert ist, und überprüfen Sie den Status von Objekten im Anschluss Leerzeichen und MV. Bestätigen Sie, dass das AD CS-Objekt zu das MV-Objekt über die Regel "Microsoft.InfromADUserAccountEnabled.xxx" ist.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Zurücksetzen Vorgänge nicht mit mehrere Übereinstimmungen gefunden Fehler zurück</p>
            </td>
            <td>
              <p>Verbundene oder Kennwort-Hash Sync verdächtigem Benutzer ihre Kennwörter Siehe Zurücksetzen Fehler nach dem Kennwort, dass Fehler Service übermitteln.</p>
              <p>

              </p>
              <p>Darüber hinaus kann bei Kennwort zurücksetzen, in Ereignisprotokollen von Azure AD Connect Service zeigt "Mehrere Spiele gefunden" Fehler Fehlermeldung.</p>
            </td>
            <td>
              <p>Dies bedeutet, dass das Synchronisierungsmodul festgestellt, dass das MV-Objekt mit mehreren AD CS-Objekte über "Microsoft.InfromADUserAccountEnabled.xxx". Dies bedeutet, dass der Benutzer ein aktiviertes Konto in mehr als einer Gesamtstruktur. </p>
              <p>

              </p>
              <p>Dieses Szenario wird derzeit für das Kennwort Rückschreiben nicht unterstützt.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Kennwort schlagen mit einem Fehler.</p>
            </td>
            <td>
              <p>Kennwort schlagen mit einem Fehler. Das Anwendungsereignisprotokoll enthält Azure AD Connect Fehler 6329 mit Text: 0x8023061f (der Vorgang ist fehlgeschlagen, da Kennwortsynchronisation dieser Management Agent nicht aktiviert ist.)</p>
            </td>
            <td>
              <p>Dies tritt auf, wenn die Azure AD Connect Konfiguration geändert wird, fügen Sie&nbsp;ein neues AD-Gesamtstruktur (oder zu eine vorhandene Gesamtstruktur erneut) <strong>nach</strong> Kennwort Rückschreiben Feature wurde bereits aktiviert. Kennwort wird für Benutzer in diese neue Gesamtstrukturen fehlschlägt. Um das Problem zu beheben, deaktivieren und nach Abschluss die Neukonfiguration Gesamtstruktur Kennwort Rückschreiben-Funktion aktivieren.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Zurückschreiben von Kennwörtern durch Benutzer funktioniert korrekt zurückgesetzt wurden jedoch Zurückschreiben von Kennwörtern durch Benutzer geändert oder Zurücksetzen eines Benutzers vom Administrator fehlschlägt.</p>
            </td>
            <td>
              <p>Beim Zurücksetzen des Kennworts für einen Benutzer von Azure-Verwaltungsportal, sehen Sie eine Meldung: "der Kennwort zurücksetzen Dienst in Ihrer lokalen Umgebung nicht unterstützt Administratoren Benutzerkennwörter zurücksetzen. Aktualisieren Sie auf die neueste Version von Azure AD Connect zur Lösung des Problems."</p>
            </td>
            <td>
              <p>Dies tritt auf, wenn die Version das Synchronisierungsmodul bestimmten Kennwort Rückschreibvorgang nicht unterstützt, die verwendet wurde. Versionen von Azure AD Connect höher als 1.0.0419.0911 alle Verwaltungsvorgänge Kennwort unterstützt zurückgesetzt, einschließlich Kennwort Rückschreiben Kennwort ändern das Rückschreiben und Administrator initiiert Kennwort zurücksetzen Rückschreiben von Azure-Verwaltungsportal.&nbsp; Dirsync-Versionen unterstützen höher als 1.0.6862 nur Kennwort zurücksetzen Rückschreiben. Um dieses Problem zu beheben, wir empfehlen, die neueste Version von Azure AD Connect oder Azure Active Directory verbinden installieren (Weitere Informationen finden Sie unter <a href="active-directory-aadconnect">Verzeichnisintegrationstools</a>) um dieses Problem zu beheben und Kennwort Rückschreiben in Ihrer Organisation optimal.</p>
            </td>
          </tr>
        </tbody></table>


## <a name="password-writeback-event-log-error-codes"></a>Kennwort Rückschreiben Ereignisprotokoll-Fehlercodes
Eine bewährte Methode bei Problemen mit Kennwort Rückschreiben ist, Anwendungsereignisprotokoll auf dem Azure AD Connect Computer überprüfen. Das Ereignisprotokoll enthält Ereignisse aus zwei Quellen von Interesse für das Rückschreiben Kennwort. PasswordResetService Quelle werden Vorgänge und Probleme mit dem Betrieb eines Kennworts Rückschreiben beschrieben. ADSync Quelle werden Vorgänge und Passwörter in der AD-Umgebung Probleme beschrieben.

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Code</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Name / Nachricht</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Quelle</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Beschreibung</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>6329</p>
            </td>
            <td>
              <p>KAUTION: MMS(4924) 0x80230619 – "eine Einschränkung verhindert das Kennwort zum aktuellen angegeben geändert."</p>
            </td>
            <td>
              <p>ADSync</p>
            </td>
            <td>
              <p>Dieses Ereignis tritt ein, wenn das Kennwort Rückschreiben versucht, ein Kennwort in Ihrem lokalen Verzeichnis festlegen, die nicht Kennwortalter, Verlauf, Komplexität oder Filterung an der Domäne.</p>
              <ul>
                <li class="unordered">
Wenn Sie ein minimales Kennwortalter, und das Kennwort innerhalb dieses Zeitfensters wurden vor kurzem geändert, dürfen Sie das Kennwort erneut ändern, bis das angegebene Alter in Ihrer Domäne erreicht. Zu Testzwecken sollte Mindestalter auf 0 festgelegt.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Wenn Sie Kennwortverlauf aktiviert, dann wählen Sie ein Kennwort, das nicht in der letzten N-Mal eingesetzt ist N der Einstellung. Wenn Sie ein Kennwort auswählen, die in der letzten N Mal verwendet wurde, werden Sie in diesem Fall einen Fehler angezeigt. Zu Testzwecken sollte Verlauf auf 0 festgelegt.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Sie haben diese Anforderung alle gelten, wenn der Benutzer versucht, ein Kennwort ändern oder zurücksetzen.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Wenn Sie Kennwortfilter aktiviert, und der Benutzer wählt ein Kennwort nicht die Kriterien filtern, dann zurücksetzen oder ändern schlägt der Vorgang fehl.<br\><br\></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>HR 8023042</p>
            </td>
            <td>
              <p>Synchronisierungsmodul zurückgegebenen Fehler hr = 80230402, Nachricht = versucht, eine Objekt ist fehlgeschlagen, weil doppelte Einträge mit dem gleichen Anker</p>
            </td>
            <td>
              <p>ADSync</p>
            </td>
            <td>
              <p>Dieses Ereignis tritt ein, wenn die Benutzer-Id in mehreren Domänen aktiviert ist.  Beispielsweise wenn Sie Konto-Ressource Gesamtstrukturen synchronisieren und die gleichen Benutzer-Id vorhanden und in jedem aktiviert haben, kann dieser Fehler auftreten.  </p>
              <p>Dieser Fehler kann auch auftreten, wenn Sie eine nicht eindeutige Ankerattribut (wie Alias oder UPN) und zwei, dieselbe Ankerattribut Benutzern.</p>
              <p>Zum Beheben dieses Problems sicher, dass Sie nicht doppelt vorhandene Benutzer innerhalb Ihrer Domänen verfügen und unter einer eindeutigen Ankerattribut für jeden Benutzer Verwendung.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31001</p>
            </td>
            <td>
              <p>PasswordResetStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass der lokale Dienst ein Passwort anfordern einer verbundenen oder Kennwort Hash Sync verdächtigem Benutzer aus der Cloud erkannt. Dieses Ereignis ist das erste Ereignis jedes Kennwort Rückschreibvorgang zurücksetzen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31002</p>
            </td>
            <td>
              <p>PasswordResetSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass ein Benutzer ein neues Kennwort während einer Operation Kennwort zurücksetzen ausgewählt, wir festgestellt, dass dieses Kennwort corporate Kennwort erfüllt und Kennwort erfolgreich wieder auf der AD-Umgebung geschrieben wurde.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31003</p>
            </td>
            <td>
              <p>PasswordResetFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass ein Benutzer ein Kennwort und Kennwort kam erfolgreich in die lokale Umgebung, aber beim Versuch das Kennwort in Active Directory-Umgebung festgelegt, ist ein Fehler aufgetreten. Dies kann aus mehreren Gründen geschehen:</p>
              <ul>
                <li class="unordered">
Das Kennwort des Benutzers nicht erfüllen, Alter, Verlauf, Komplexität, oder an der Domäne filtern. Versuchen Sie, ein vollständig neues Kennwort zur Behebung dieses Problems.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
MA-Dienstkonto muss nicht die entsprechenden Berechtigungen für das neue Kennwort für das betreffende Benutzerkonto festgelegt.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Das Benutzerkonto ist in einer geschützten Gruppe wie Domänen- oder Organisationsadministrator-Administratoren, die Kennwort Mengenoperationen lässt.<br\><br\></li>
              </ul>
              <p>Siehe <a href="#troubleshoot-password-writeback">Problembehandlung Kennwort Rückschreiben</a> in erfahren mehr über welche Situationen diese Fehler verursachen kann.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31004</p>
            </td>
            <td>
              <p>OnboardingEventStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis tritt Kennwort Rückschreiben Azure AD Connect aktivieren und gibt an, dass wir Ihrer Organisation an den Webdienst Kennwort Rückschreiben Onboarding gestartet.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31005</p>
            </td>
            <td>
              <p>OnboardingEventSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt die Schritte erfolgreich war und Funktion Rückschreiben Kennwort verwenden kann.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31006</p>
            </td>
            <td>
              <p>ChangePasswordStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass der lokale Dienst Änderungsanträgen Kennwort für einen verbundenen erkannt oder Kennwort Hash Sync verdächtigem Benutzer aus der Cloud. Dieses Ereignis ist das erste Ereignis in jedem Rückschreibvorgang Kennwort ändern.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31007</p>
            </td>
            <td>
              <p>ChangePasswordSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass ein Benutzer ein neues Kennwort während einer Operation Kennwort ändern aktiviert, wir festgestellt, dass dieses Kennwort corporate Kennwort erfüllt und Kennwort erfolgreich wieder auf der AD-Umgebung geschrieben wurde.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31008</p>
            </td>
            <td>
              <p>ChangePasswordFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass ein Benutzer ein Kennwort und Kennwort kam erfolgreich in die lokale Umgebung, aber beim Versuch das Kennwort in Active Directory-Umgebung festgelegt, ist ein Fehler aufgetreten. Dies kann aus mehreren Gründen geschehen:</p>
              <ul>
                <li class="unordered">
Das Kennwort des Benutzers nicht erfüllen, Alter, Verlauf, Komplexität, oder an der Domäne filtern. Versuchen Sie, ein vollständig neues Kennwort zur Behebung dieses Problems.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
MA-Dienstkonto muss nicht die entsprechenden Berechtigungen für das neue Kennwort für das betreffende Benutzerkonto festgelegt.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Das Benutzerkonto ist in einer geschützten Gruppe wie Domänen- oder Organisationsadministrator-Administratoren, die Kennwort Mengenoperationen lässt.<br\><br\></li>
              </ul>
              <p>Siehe <a href="#troubleshoot-password-writeback">Problembehandlung Kennwort Rückschreiben</a> in erfahren mehr über welche Situationen diese Fehler verursachen kann.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31009</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Vor-Ort-Service erkannt ein Passwort anfordern einer verbundenen oder Kennwort-Hash Sync verdächtigem Benutzer Administrator im Namen eines Benutzers aus. Dieses Ereignis ist das erste Ereignis in jeder Rückschreibvorgang Admin initiierte Kennwort zurücksetzen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31010</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Der Administrator aktiviert ein neues Kennwort während einer Admin-initiierte Kennwort zurücksetzen, wir festgestellt, dass dieses Kennwort corporate Kennwort erfüllt und Kennwort erfolgreich wieder auf der AD-Umgebung geschrieben wurde.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31011</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Der Administrator ein Kennwort für einen Benutzer ausgewählt und das Kennwort der lokalen Umgebung erfolgreich eingegangen, aber beim Versuch das Kennwort in Active Directory-Umgebung festgelegt, ist ein Fehler aufgetreten. Dies kann aus mehreren Gründen geschehen:</p>
              <ul>
                <li class="unordered">
Das Kennwort des Benutzers nicht erfüllen, Alter, Verlauf, Komplexität, oder an der Domäne filtern. Versuchen Sie, ein vollständig neues Kennwort zur Behebung dieses Problems.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
MA-Dienstkonto muss nicht die entsprechenden Berechtigungen für das neue Kennwort für das betreffende Benutzerkonto festgelegt.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Das Benutzerkonto ist in einer geschützten Gruppe wie Domänen- oder Organisationsadministrator-Administratoren, die Kennwort Mengenoperationen lässt.<br\><br\></li>
              </ul>
              <p>Siehe <a href="#troubleshoot-password-writeback">Problembehandlung Kennwort Rückschreiben</a> in erfahren mehr über welche Situationen diese Fehler verursachen kann.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31012</p>
            </td>
            <td>
              <p>OffboardingEventStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis tritt Kennwort Rückschreiben Azure AD Connect deaktivieren und gibt an, dass wir Offboarding Ihrer Organisation an den Webdienst Kennwort Rückschreiben gestartet.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31013</p>
            </td>
            <td>
              <p>OffboardingEventSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt das Offboarding erfolgreich war und Funktion Rückschreiben Kennwort erfolgreich deaktiviert wurde.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31014</p>
            </td>
            <td>
              <p>OffboardingEventFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass das Offboarding nicht erfolgreich war. Kann entweder einen Berechtigungsfehler auf das während der Konfiguration angegebene Cloud oder lokalen Administratorkonto oder können verbundenen Cloud globaler Administrator Kennwort Rückschreiben deaktivieren möchten. Dieses Problem beheben, überprüfen Sie Ihre administrativen Berechtigungen und eine Verwendung verbundene Konto beim Rückschreiben von Kennwort-Funktion konfigurieren.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31015</p>
            </td>
            <td>
              <p>WriteBackServiceStarted </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass Kennwort Rückschreiben-Dienst erfolgreich gestartet wurde und Passwort Managementanfragen aus der Cloud akzeptieren.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31016</p>
            </td>
            <td>
              <p>WriteBackServiceStopped </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt Kennwort Rückschreiben-Dienst beendet wurde, die Anträge Management Kennwort aus der Cloud nicht erfolgreich sein werden.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31017</p>
            </td>
            <td>
              <p>AuthTokenSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass wir ein Autorisierungstoken für globale Administratoren Azure AD Connect Setup angegeben werden, um Offboarding oder Onboarding-Prozess starten erfolgreich abgerufen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31018</p>
            </td>
            <td>
              <p>KeyPairCreationSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass wir den Verschlüsselungsschlüssel erstellt, der zum Verschlüsseln der Kennwörter aus der Cloud an Ihre lokale Umgebung verwendet werden.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32000</p>
            </td>
            <td>
              <p>UnknownError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis weist einen unbekannten Fehler während einer Kennwort-Management. Der Ausnahmetext im Ereignis Weitere Informationen überprüfen. Wenn Sie Probleme haben, versuchen Sie deaktivieren und aktivieren das Rückschreiben Kennwort. Wenn dies nicht hilft, fügen Sie eine Kopie des Ereignisprotokolls und die Tracking-Id angegebenen Insider Ihr Supportbetreuer.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32001</p>
            </td>
            <td>
              <p>ServiceError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis gibt gab Fehler mit Kennwort Cloud-Dienst zurücksetzen und normalerweise tritt auf, wenn der lokale Dienst Kennwort zurücksetzen-Webdienst herstellen konnte. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32002</p>
            </td>
            <td>
              <p>ServiceBusError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt Fehler des Mieters Dienstinstanz Bus verbinden. Dies kann vorkommen, da Sie ausgehende Verbindungen in Ihrer lokalen Umgebung blockieren. Überprüfen Sie Ihre Firewall so Verbindung über TCP 443 und <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>zulassen und versuchen Sie es erneut. Sollten Sie weiterhin Probleme auftreten, deaktivieren und aktivieren das Rückschreiben Kennwort.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32003</p>
            </td>
            <td>
              <p>InPutValidationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass an unserer Web Service-API übergebene Eingabe ungültig ist. Wiederholen Sie den Vorgang.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32004</p>
            </td>
            <td>
              <p>DecryptionError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass beim Entschlüsseln des Kennworts aus der Cloud ein Fehler aufgetreten. Dies könnte durch Entschlüsselung Schlüssel überein Cloud-Dienst der lokalen Umgebung. Um dieses Problem zu beheben, deaktivieren Sie und reaktivieren Sie Kennwort Rückschreiben in Ihrer lokalen Umgebung.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32005</p>
            </td>
            <td>
              <p>ConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Während Onboarding speichern mandantenspezifische Informationen in einer Konfigurationsdatei in Ihrer lokalen Umgebung. Dieses Ereignis zeigt Fehler Datei speichern oder wenn es Dienststart Fehler Datei lesen. Um dieses Problem zu beheben, versuchen Sie deaktivieren und aktivieren Kennwort Rückschreiben neu schreiben diese Datei erzwingen. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32006</p>
            </td>
            <td>
              <p>EndPointConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>VERALTET – dieses Ereignis ist nicht vorhanden in Azure AD verbinden, der DirSync Rückschreiben unterstützt nur früh erstellt.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32007</p>
            </td>
            <td>
              <p>OnBoardingConfigUpdateError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Während Onboarding senden wir Daten aus der Cloud an lokalen Dienst zurückgesetzt. Daten werden dann in eine Datei im Arbeitsspeicher geschrieben, bevor an Synchronisierungsdienst diese Informationen sicher auf einem Datenträger speichern. Dieses Ereignis weist auf ein Problem mit schreiben oder aktualisieren die Daten im Speicher. Um dieses Problem zu beheben, versuchen Sie deaktivieren und aktivieren Kennwort Rückschreiben neu schreiben diese Konfiguration zu erzwingen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32008</p>
            </td>
            <td>
              <p>ValidationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass wir hat eine ungültige Antwort vom Webdienst Kennwort zurücksetzen. Um dieses Problem zu beheben, versuchen Sie Rückschreiben von Kennwort deaktivieren und wieder aktivieren.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32009</p>
            </td>
            <td>
              <p>AuthTokenError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass wir eine Autorisierung token für das globale Administratorkonto während Azure AD Connect Setup angegebenen konnte nicht. Dieser Fehler kann durch eine falsche Benutzername oder Kennwort für das globale Administratorkonto angegeben oder weil globale Administratorkonto angegeben Verbund. Um dieses Problem zu beheben, mit den richtigen Benutzernamen und das Kennwort erneut, und stellen Sie sicher, der Administrator wird (nur Cloud oder Kennwort-Sync) Konto.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32010</p>
            </td>
            <td>
              <p>CryptoError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt den Fehler beim Generieren des Kennworts Schlüssel oder ein Kennwort, das aus der Cloud-Dienst entschlüsseln. Dieser Fehler wahrscheinlich gibt ein Problem mit Ihrer Umgebung. Sehen Sie sich die Details des Ereignisprotokolls und dieses Problem zu beheben. Sie können auch versuchen, deaktivieren und reaktivieren Kennwort Rückschreiben Service zur Lösung des Problems.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32011</p>
            </td>
            <td>
              <p>OnBoardingServiceError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass der lokale Dienst nicht ordnungsgemäß Kennwort zurücksetzen Webdienst Onboarding-Prozess initiieren kommunizieren konnte. Eine Firewall-Regel oder Probleme Auth-Token für Ihren Mandanten möglicherweise. Zum Beheben dieses Problems sicher, dass Sie nicht blockieren ausgehende Verbindungen über TCP 443 und TCP 9350 9354 oder <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>und Sie mit dem integrierten AAD-Administratorkonto kein Verbund. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32012</p>
            </td>
            <td>
              <p>OnBoardingServiceDisableError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>VERALTET – dieses Ereignis ist nicht vorhanden in Azure AD verbinden, der DirSync Rückschreiben unterstützt nur früh erstellt.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32013</p>
            </td>
            <td>
              <p>OffBoardingError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass der lokale Dienst nicht ordnungsgemäß Kennwort zurücksetzen-Webdienst das Offboarding initiieren kommunizieren konnte. Eine Firewall-Regel oder Probleme Authentifizierungstoken für Ihren Mandanten möglicherweise. Zum Beheben dieses Problems sicher, dass Sie nicht blockieren ausgehende Verbindungen 443 oder <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>, die AAD Administratorkonto auf externe Verwendung befindet und kein Verbund. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32014</p>
            </td>
            <td>
              <p>ServiceBusWarning </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass wir zum Herstellen des Mieters Bus Dienstinstanz wiederholen. Unter normalen Umständen sollte dies nicht geplant, sondern wenn dieses Ereignis häufig sollte überprüft die Netzwerkschnittstelle Service Bus insbesondere ein Wartezeiten oder niedriger Bandbreite.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32015</p>
            </td>
            <td>
              <p>ReportServiceHealthError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Um die Überwachung des Dienstes Kennwort Rückschreiben senden wir Taktdatenbank unsere Kennwort Webdienst 5 Minuten zurückgesetzt. Dieses Ereignis zeigt an, dass Fehler beim Senden dieses Gesundheitsinformationen, die Cloud-Webdienst. Diese Gesundheitsinformationen einer OII oder PII-Daten und ist rein Herzschlag und Standard-Statistik, damit wir Service-Statusinformationen in der Cloud bereitstellen können.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33001</p>
            </td>
            <td>
              <p>ADUnKnownError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass unbekannte Fehler zurückgegebene AD, im Ereignisprotokoll Azure AD Connect Server Ereignisse aus ADSync Quelle für Weitere Informationen zu diesem Fehler.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33002</p>
            </td>
            <td>
              <p>ADUserNotFoundError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass der Benutzer, der versucht, zurücksetzen oder Ändern eines Kennworts im lokalen Verzeichnis nicht gefunden wurde. Kann auftreten, wenn der Benutzer lokal gelöscht wurde jedoch nicht in der Cloud oder ein Problem bei der Synchronisierung. Überprüfen der Sync-Protokolle sowie die letzte einige synchronisieren nähere ausführen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33003</p>
            </td>
            <td>
              <p>ADMutliMatchError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Beim Zurücksetzen eines Kennworts oder Anforderung aus der Cloud stammt verwenden wir Cloud Anker während des Installationsvorgangs Azure AD Connect angegeben, wie die Anforderung an einen Benutzer in der lokalen Umgebung zu verknüpfen. Dieses Ereignis zeigt an, dass wir zwei Benutzer im lokalen Verzeichnis mit der gleichen Wolke Ankerattribut gefunden. Überprüfen der Sync-Protokolle sowie die letzte einige synchronisieren nähere ausführen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33004</p>
            </td>
            <td>
              <p>ADPermissionsError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass Management Agent-Dienstkonto nicht die entsprechenden Berechtigungen für das Konto ein neues Kennwort festgelegt haben. Sicher, dass das MA-Konto in der Gesamtstruktur des Benutzers zurücksetzen und ändern Kennwort Berechtigungen für alle Objekte in der Gesamtstruktur.  Weitere Informationen dazu, siehe <a href="active-directory-passwords-getting-started.md#step-4-set-up-the-appropriate-active-directory-permissions">Schritt 4: Einrichten der entsprechenden Active Directory-Berechtigungen</a>.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33005</p>
            </td>
            <td>
              <p>ADUserAccountDisabled </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass wir versucht, zurücksetzen oder ändern ein Kennwort für ein Konto lokal deaktiviert wurde. Aktivieren Sie das Konto, und wiederholen Sie den Vorgang.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33006</p>
            </td>
            <td>
              <p>ADUserAccountLockedOut </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ereignis zeigt es zurücksetzen oder ändern ein Kennwort für ein Konto lokal gesperrt wurde. Sperren können auftreten, wenn ein Benutzer versucht, eine Änderung oder Zurücksetzens Kennwort zu oft kurzfristig. Heben Sie die Sperre, und versuchen Sie es erneut.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33007</p>
            </td>
            <td>
              <p>ADUserIncorrectPassword </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass der Benutzer beim Ausführen ein Kennwort Änderung aktuelle Kennwort falsch angegeben. Das aktuelle Kennwort und versuchen Sie es erneut.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33008</p>
            </td>
            <td>
              <p>ADPasswordPolicyError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis tritt ein, wenn das Kennwort Rückschreiben versucht, ein Kennwort in Ihrem lokalen Verzeichnis festlegen, die nicht Kennwortalter, Verlauf, Komplexität oder Filterung an der Domäne.</p>
              <ul>
                <li class="unordered">
Wenn Sie ein minimales Kennwortalter, und das Kennwort innerhalb dieses Zeitfensters wurden vor kurzem geändert, dürfen Sie das Kennwort erneut ändern, bis das angegebene Alter in Ihrer Domäne erreicht. Zu Testzwecken sollte Mindestalter auf 0 festgelegt.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Wenn Sie Kennwortverlauf aktiviert, dann wählen Sie ein Kennwort, das nicht in der letzten N-Mal eingesetzt ist N der Einstellung. Wenn Sie ein Kennwort auswählen, die in der letzten N Mal verwendet wurde, werden Sie in diesem Fall einen Fehler angezeigt. Zu Testzwecken sollte Verlauf auf 0 festgelegt.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Sie haben diese Anforderung alle gelten, wenn der Benutzer versucht, ein Kennwort ändern oder zurücksetzen.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Wenn Sie Kennwortfilter aktiviert, und der Benutzer wählt ein Kennwort nicht die Kriterien filtern, dann zurücksetzen oder ändern schlägt der Vorgang fehl.<br\><br\></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>33009</p>
            </td>
            <td>
              <p>ADConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Dieses Ereignis zeigt an, dass ein Problem schreiben ein Kennwort wieder in Ihrem lokalen Verzeichnis durch ein Konfigurationsproblem mit Active Directory ist. Ereignisprotokoll Azure AD Connect Computer Anwendung Nachrichten vom Dienst ADSync Weitere welcher Fehler aufgetreten ist. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34001</p>
            </td>
            <td>
              <p>ADPasswordPolicyOrPermissionError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>VERALTET – dieses Ereignis ist nicht vorhanden in Azure AD verbinden, der DirSync Rückschreiben unterstützt nur früh erstellt.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34002</p>
            </td>
            <td>
              <p>ADNotReachableError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>VERALTET – dieses Ereignis ist nicht vorhanden in Azure AD verbinden, der DirSync Rückschreiben unterstützt nur früh erstellt.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34003</p>
            </td>
            <td>
              <p>ADInvalidAnchorError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>VERALTET – dieses Ereignis ist nicht vorhanden in Azure AD verbinden, der DirSync Rückschreiben unterstützt nur früh erstellt.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-writeback-connectivity"></a>Problembehandlung bei Kennwort Rückschreiben Konnektivität

Service-Unterbrechung mit dem Kennwort Rückschreiben Azure AD Connect haben, sind hier einige Schritte, mit denen Sie um dieses Problem zu beheben:

 - [Neustart Azure AD Synchronisierungsdienst herstellen](#restart-the-azure-AD-Connect-sync-service)
 - [Deaktivieren Sie und aktivieren Sie die Funktion Rückschreiben Kennwort](#disable-and-re-enable-the-password-writeback-feature)
 - [Azure AD Connect Version installieren](#install-the-latest-azure-ad-connect-release)
 - [Problembehandlung bei Kennwort Rückschreiben](#troubleshoot-password-writeback)

Es wird empfohlen, dass diese Schritte in der oben angegebenen Reihenfolge der Service auf die schnelle Wiederherstellung ausführen.

### <a name="restart-the-azure-ad-connect-sync-service"></a>Neustart Azure AD Synchronisierungsdienst herstellen
Dienstneustart Azure AD Verbindung synchronisieren können um Konnektivitätsprobleme oder anderer vorübergehender Probleme mit dem Dienst zu beheben.

 1. Klicken Sie als Administrator auf dem **Azure AD Connect**-Server **Starten** .
 2. Geben Sie **"services.msc"** in das Suchfeld ein und drücken Sie die **EINGABETASTE**.
 3. Suchen Sie den Eintrag **Microsoft Azure AD verbinden** .
 4. Mit der rechten Maustaste auf den Eintrag klicken Sie auf **neu**, und warten Sie, bis des Vorgangs abgeschlossen.

    ![][002]

Diese Schritte erneut die Verbindung mit dem Clouddienst und Unterbrechung auftretenden möglicherweise beheben.  Synchronisierungsdienst Neustart das Problem nicht behoben, empfehlen wir Sie deaktivieren und reaktivieren Kennwort Rückschreiben Funktion als Nächstes.

### <a name="disable-and-re-enable-the-password-writeback-feature"></a>Deaktivieren Sie und aktivieren Sie die Funktion Rückschreiben Kennwort
Deaktivieren und reaktivieren Kennwort Rückschreiben Feature können um Verbindungsprobleme zu beheben.

 1. Als Administrator öffnen Sie **Azure AD Connect-Konfigurations-Assistenten**.
 2. Klicken Sie im Dialogfeld **Verbindung mit Azure AD** Geben Sie Ihre **Azure AD globaler Administrator-Anmeldeinformationen**
 3. Geben Sie im Dialogfeld **Verbindung mit AD DS** Ihre **Active Directory-Domänendienste-Admin-Anmeldeinformationen**.
 4. Klicken Sie auf **Weiter** , klicken Sie im Dialogfeld **Benutzer eindeutig identifiziert** .
 5. Deaktivieren Sie im Dialogfeld **Optionen** das Kontrollkästchen **Kennwort Zurückschreiben** .

    ![][003]

 6. Klicken Sie auf **Weiter** durch die verbleibenden dialogfeldseiten ohne etwas zu ändern, bis Sie zu der Seite **bereit zum Konfigurieren** .
 7. Sicherstellen Sie, dass die Seite konfigurieren **Kennwort Write-Back-Option deaktiviert** wird, und klicken Sie auf die grüne Schaltfläche **Konfigurieren** , Ihre Änderungen.
 8. Das Dialogfeld **Fertig** deaktivieren Sie die Option **Jetzt synchronisieren** , und klicken Sie dann auf **Fertig stellen** , um den Assistenten zu schließen.
 9. **Azure AD Connect-Konfigurationsassistenten**erneut öffnen.
 10.    **Wiederholen Sie die Schritte 2 bis 8**, außer Sie **optionale Funktionen** **die Kennwort-Write-Back-Option** sicherstellen Bildschirm den Dienst wieder aktivieren.

    ![][004]

Diese Schritte erneut die Verbindung mit Cloud Service und Unterbrechung auftretenden möglicherweise beheben.

Deaktivieren und reaktivieren Funktion Rückschreiben Kennwort Ihr Problem nicht löst, sollten Sie versuchen, Azure AD Verbinden als Nächstes installieren.

### <a name="install-the-latest-azure-ad-connect-release"></a>Azure AD Connect Version installieren
Neuinstallation von Azure AD Connect-Paket löst die auswirken können entweder Konfigurationsprobleme Cloud-Services oder Kennwörter in der Active Directory-Umgebung zu verbinden.
Empfohlen wird, führen Sie diesen Schritt erst versuchen, die ersten beiden oben beschriebenen Schritte.

 1. Herunterladen der neuesten Version von Azure AD Connect [hier](active-directory-aadconnect.md#install-azure-ad-connect).
 2. Da Azure AD Connect bereits installiert haben, müssen Sie nur ein direktes Upgrade Azure AD Connect-Installation auf die neueste Version aktualisieren.
 3. Führen Sie das heruntergeladene Paket, und führen Sie die auf dem Bildschirm auf den Azure AD Connect Computer aktualisieren.  Keine weiteren manuellen Schritte sind erforderlich, sofern Sie aus Feld Synchronisierungsregeln angepasst haben sollten Sie in diesem Fall **sichern diese vor dem Fortsetzen aktualisieren und manuell erneut bereitstellen, nachdem Sie fertig sind**.

Diese Schritte erneut die Verbindung mit Cloud Service und Unterbrechung auftretenden möglicherweise beheben.

Installieren die neueste Version von Azure AD Connect-Server das Problem nicht behoben, empfehlen wir Sie deaktivieren und aktivieren das Rückschreiben Kennwort als letzten Schritt nach der Installation der neuesten Synchronisierung QFE.

Wenn das Problem dadurch nicht behoben wird, empfehlen wir, dass Sie betrachten [Beheben Kennwort Rückschreiben](#troubleshoot-password-writeback) und [Azure AD Passwort Management – häufig gestellte Fragen](active-directory-passwords-faq.md) , ob Ihr es diskutiert werden kann.


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>Links zu Kennwort zurücksetzen Dokumentation
Unten finden Sie Links zu allen Dokumentationsseiten Azure AD Kennwort zurücksetzen:

* **Sind Sie hier beim Anmelden Probleme auftreten?** In diesem Fall [hier wie ändern und Ihr eigenes Kennwort](active-directory-passwords-update-your-own-password.md).
* [**Funktionsweise**](active-directory-passwords-how-it-works.md) - erfahren Sie mehr über die sechs verschiedenen Komponenten des Dienstes und jeder
* [**Erste Schritte**](active-directory-passwords-getting-started.md) - erfahren Sie, wie Sie Benutzern zurücksetzen und ihre Cloud oder lokalen Kennwörter ändern
* [**Anpassen**](active-directory-passwords-customize.md) - Informationen zum Anpassen von Aussehen und Verhalten und Verhalten des Diensts zu Ihrem Unternehmen
* [**Best Practices**](active-directory-passwords-best-practices.md) - so schnell bereitstellen und Kennwörter in Ihrer Organisation verwalten
* Sie [**erfahren**](active-directory-passwords-get-insights.md) – erfahren Sie mehr über unsere integrierten reporting-Funktionen
* [**FAQ**](active-directory-passwords-faq.md) - Antworten auf häufig gestellte Fragen
* [**Erfahren Sie mehr**](active-directory-passwords-learn-more.md) - gehen tief in die technischen Einzelheiten der Funktionsweise des Dienstes



[001]: ./media/active-directory-passwords-troubleshoot/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-troubleshoot/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-troubleshoot/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-troubleshoot/004.jpg "Image_004.jpg"
