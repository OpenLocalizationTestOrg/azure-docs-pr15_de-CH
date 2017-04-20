# <a name="pull-request-comment-automation"></a>Pull-Anforderung Kommentar Automatisierung

Mithilfe Kommentar Automatisierung Pull-Anforderung Kommentare Mitwirkenden ermöglichen und Autoren Etiketten zuweisen, die Pull-Anforderung Laufwerk Überprüfungsprozess.

| Kommentar | Funktionsweise | Verfügbarkeit|
| -------- |-------------|-------------|
|#Abzeichnen | Wenn der Autor eines Artikels **#Sign-off** Kommentar im Stream Kommentar eingibt, ist **bereit Zusammenführen** Beschriftung zugewiesen. | Öffentliche und private|
|#Abzeichnen | Wenn ein Teilnehmer, der nicht aufgelisteten Autor versucht eine öffentliche Pull-Anforderung mit dem Kommentar **#Sign-off** abzeichnen, wird eine Meldung geschrieben die Pull Anforderung zeigt an, dass die Bezeichnung vom Autor zugewiesen werden kann. | Öffentliche |
|#Halten Sie | **#Hold-off** eine Pull-Anforderung Kommentar Eingabe wird entfernt **bereit Zusammenführen** Label - Ihre Meinung ändern oder einen Fehler machen. Private Repo weist diese Bezeichnung **führen nicht zusammenführen** . | Öffentliche und private |
| #Bitte schließen | Autoren geben den Kommentar **#please Close** in den Kommentar eine Pull-Anforderung zu schließen, wenn Sie nicht die Änderungen zusammengeführt. | Öffentliche |

##<a name="troubleshooting-sign-offs-in-the-public-repo"></a>Signoffs im öffentlichen Repo Problembehandlung

Öffentliche Repo Signoff Automatisierung ist den Autor um. Manuelle Ausnahme verarbeiten erforderlich:

- **Autoren der Artikel**: Verwendung öffentlicher Repository Kommentar Automatisierung Ihrem tatsächliche GitHub Konto muss exakt gemäß Artikel Metadaten GitHub-Konto. Die Aktivierung Ihres Kontos ist wichtig. Wenn Sie dieses Problem Signoff gehindert, senden Sie an den Alias Azdocprs.

- **Andere Mitarbeiter**: Sie Mitarbeiter Signoff ist für den Autor und Sie von der Automatisierung blockiert werden, wenden Sie sich an Azdocprs mit den Link ziehen. Wem Sie – PMs gleichen Produktteam, Kollegen im Team schreiben und Schreiben Teammanagement vertrauenswürdige Quellen gelten.



## <a name="related"></a>Bezug

- [Pull-Anforderung Etikette und best Practices für die Microsoft-Mitarbeiter](contributor-guide-pull-request-etiquette.md)

- [Qualitätskriterien für Pull-Anforderung überprüfen](contributor-guide-pr-criteria.md)
