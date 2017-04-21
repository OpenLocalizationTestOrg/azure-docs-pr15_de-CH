# <a name="pull-request-etiquette-and-best-practices-for-microsoft-contributors-to-azure-documentation"></a>Pull-Anforderung Etikette und best Practices für Mitwirkende Microsoft Azure-Dokumentation

Um Änderungen Dokumentation veröffentlicht Anträge Sie ziehen aus der Verzweigung. Alle Pull-Anforderung muss vor zusammenzuführenden überprüft werden. Diesem Artikel verstehen Sie mit Pull-Anforderung sollte und Pull Anfragen, das Erstellen einfacher und schneller zu überprüfen, damit besser die Anforderungswarteschlange Pull für alle funktioniert sind.

## <a name="working-with-pull-request-reviewers"></a>Arbeiten mit Pull-Anforderung

Hier werden die Grundlagen zum Arbeiten mit Pull-Anforderung kennen müssen. 

- <b>Einführung in die Rolle des Bearbeiters Pull-Anforderung. Bearbeiter:</b>
  - Die grundlegende Qualität des Inhalts
  - Verhindert, dass Regressionen im repository
  - Gibt Feedback vor dem Zusammenführen

  Pull-Anforderung Bearbeiter sind Content Governance-Rolle. Primäre soll nicht einfach Zusammenführen was so schnell wie möglich übermittelt wird. Erwarten Sie Feedback, das aktualisiert, insbesondere für neue und überarbeitete stark erforderlich.

- <b>Planen Sie im voraus die Pull-Anforderung Reviewer:</b>
  - Anfragen mit hoher Priorität ziehen
  - Pull-Anfragen für Versionen Timeout/Datum
  - Pull-Anfragen, die ändern oder Hinzufügen von Dateien

- <b>SLA für Pull Überprüfung anfordern</b>

  Im privaten Repository versucht jedem Pull-Anforderung die Warteschlange mit der Bezeichnung Ready Zusammenführen Pull gibt das Team Pull-Anforderung innerhalb von 12 Stunden (Mo-Fr, 8-17 Uhr) überprüfen und Feedback oder zusammengeführt werden, wenn kein Feedback erforderlich ist. SLA wendet auf die Überprüfung der PR nicht zusammengeführt. PRs werden zusammengeführt, wenn sie [die](contributor-guide-pr-criteria.md)Kriterien für die Zusammenführung. 

## <a name="make-the-pull-request-queue-work-better-for-everyone"></a>Machen Sie die Pull Warteschlange für alle besser

Es gibt zwei grundlegende Realitäten in der PR-Warteschlange:

- Pull-Anfragen, die sind klein und enthalten sehr ähnliche, schneller überprüfen. 
- Pull-Anfragen sind große Bereich oder enthalten andere, gemischte Art der Änderung dauert länger überprüfen.

Sie können die Warteschlange ziehen durch solche optimalen Vorgehensweisen besser machen:

- Separate kleinere Updates vorhandene Artikel neue Artikel oder großen schreibt. Diese Änderungen in getrennten Zweigen arbeiten arbeiten. 

- Wenn Sie Artikel oder Bilder löschen, verwenden Sie den Löschvorgang neue Inhalte hinzufügen oder Updates. Behandeln Sie ändern bzw. neuen Inhalt in einem separaten Zweig arbeiten.

- Versionen oder Inhalte Umgestaltung Planen der PR-Reviewer. Möglicherweise seine Hilfe zum Erstellen einer Veröffentlichung oder Zusammenführen Zeiten mit publishing koordiniert, damit Ihre Inhalte zum richtigen Zeitpunkt veröffentlicht wird.

- Wenn Sie Aktualisierung vorgenommen ACOM REPO (z.B. Änderungen Navigationsleiste landing Pages, leitet oder Learning Maps) mit Azure Inhalt Pr-Repository machen möchten, müssen Sie voraus, dass die Arbeit mit der PR-Prüfer koordinieren. Riskieren Sie viele fehlerhafte Hyperlinks.

## <a name="criteria-for-expedited-pull-requests"></a>Kriterien für beschleunigte Pull-Anfragen

- Wenden Sie sich an Azdocprs PRs bei beschleunigen. Auf Wunsch beschleunigte PR für rote Zone, Datenschutz, Legal und Sicherheitsprobleme; für Kundenzufriedenheit wirklich unterbrochen; und executive Benutzerberechtigungen. 
- Inhalt für Feature Versionen für beschleunigte Behandlung nicht qualifiziert - Release Inhalt erfordert Vorausplanung oder muss durch die Standardpriorität Warteschlange behandelt werden.


## <a name="in-a-hurry-submit-prs-that-can-be-accepted-automatically"></a>In Eile? Automatisch akzeptiert PRs senden

Verwenden Sie PRMerger Automatisierungsregeln mehr Ihre täglichen prs automatisch zusammengeführt.

PRMerger akzeptiert PR, wenn:
* Es wirkt sich auf 10 Dateien oder weniger.
* Enthält 15 Commits oder weniger.
* Weniger als 20 % der Text geändert.
* Selektoren-Switcher werden nicht aktualisiert.
* Keine Dateien gelöscht oder hinzugefügt.
* Keine Bilder sind neu, geändert oder gelöscht.

Pull-Anforderung diese Kriterien nicht erfüllt, wird die Bezeichnung "PRmerger kann nicht zusammenführen" automatisch zugewiesen also überprüfen müssen durch ein PR-Prüfer.

### <a name="need-to-make-a-lot-of-little-changes"></a>Viele kleine ändern müssen?

Ihr Stichwort von oben PRMerger Automatisierungsregeln und gehen Sie folgendermaßen vor:
* Übermitteln Sie Artikel mit Licht in das Druckerfach mit 10 oder weniger Dateien.
* Erstellen Sie eine separate PR Artikel in dem Bilder oder Selektoren. Menschliche Überprüfung erforderlich.
* Erstellen Sie eine separate PR für neue oder gelöschte Artikel. Menschliche Überprüfung erforderlich.

## <a name="related"></a>Bezug

- [Qualitätskriterien für Pull-Anforderung überprüfen](contributor-guide-pr-criteria.md)

- [Pull-Anforderung Kommentar Automatisierung](contributor-guide-pull-request-comments.md)
