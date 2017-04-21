# <a name="steps-to-follow-when-you-retire-or-change-the-name-of-an-acom-technical-article"></a>Schritte deaktivieren oder ändern Sie den Namen einer ACOM Artikel

Diese Anleitung gilt für KMU, die als Autor des Artikels aufgeführt sind, die im Abschnitt technische Dokumentation der azure.microsoft.com zurückgezogen werden. Die Schritte gelten auch, wenn eine Datei umbenannt wird.

Wenn Sie Mitglied unserer Community Azure und man ein Artikel aus irgendeinem Grund deaktiviert werden soll, lassen Sie einen Kommentar im Stream Disqus Kommentar für den Artikel zu dem Autor mitgeteilt, dass etwas nicht stimmt mit dem Artikel.

KMU-Autoren müssen mehrere Schritte ordnungsgemäß Inhalt deaktivieren, damit Benutzer die Website eine schlechte Erfahrung nicht Rente Inhalt von der Website. Artikel löschen oder ändern den Namen sollte der letzten, was geschieht!

## <a name="step-1-set-the-article-to-no-indexno-follow-and-republish-it-recommended"></a>Schritt 1: Artikel auf Nein Index/nicht folgen und veröffentlichen Sie (empfohlen)

Zunächst müssen Sie ist veröffentlichen Artikel als Nein Index/nicht folgen Wochen bevor Sie tatsächlich löschen. Dies der beste Praxis "vor" zur Abschaltung Content gilt. Dadurch entfernen Artikel Suchmaschine indiziert, Personen Artikel Suche finden. [Interne Wiki Details anzeigen](https://microsoft.sharepoint.com/teams/azurecontentguidance/wiki/Pages/Remove%20published%20pages%20and%20request%20redirects.aspx)

## <a name="step-2-manage-inbound-links-required"></a>Schritt 2: Verwalten von eingehenden Links (erforderlich)

Ermitteln Sie, ob eingehende Hyperlinks zu Ihren Inhalt nicht Microsoft. Häufig Blogs, Foren und andere Inhalte im Web verweist auf Artikel. Häufig kann für Blogbesitzer diese Links ändern und entfernen oder aktualisieren Sie Hyperlinks von Forenbeiträge. Web Analytics-Tools erkennen Sie hohem Datenverkehr eingehenden Verknüpfungen, die auf diese Weise verwalten müssen.

## <a name="step-3-remove-all-crosslinks-to-the-article-from-the-technical-content-repository-required"></a>Schritt 3: Entfernen Sie alle Querverbindungen Artikel aus dem technischen Content Repository (erforderlich)

Nicht auf leitet Querverbindungen aus anderen Artikeln kümmern. Aktualisieren oder entfernen das Kreuz Artikel verweist, Sie löschen oder umbenennen, von anderen Leuten gehört einschließlich in Artikel.

1. Gewährleisten Sie arbeiten in einer aktuellen Zweigstelle – ausführen `git pull upstream master` (oder die entsprechende Variante für diesen Befehl.

2.  Azure-Inhalt-Pr/Artikel Ordner scannen und Azure-Inhalt-Pr/enthält Ordner für alle Artikel enthält, die mit Artikel zu deaktivieren und entfernen die Querverbindungen verknüpft und eine entsprechenden neuen Querverbindungen ersetzen. Sie können einen suchen und Ersetzen-Dienstprogramm, um die Querverbindungen finden installiert haben. Wenn nicht, können Sie Windows PowerShell kostenlos! Hier ist PowerShell die Querverbindungen zu verwenden:

 ein. Starten Sie Windows PowerShell.

 b. PowerShell Prompt ändern Sie in Azure Content Pr\articles Ordner:

 `cd azure-content-pr\articles`

 c. Geben Sie diesen Befehl, der alle Dateien aufgelistet, die einen Verweis auf den Artikel enthalten, die Sie löschen:

 `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name`

  Wenn Sie die Liste der Namen in einer Datei (in diesem Fall, benannte psoutput.txt) senden möchten, können Sie:

  `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name | Out-File C:\Users\<your account>\psoutput.txt`

3. Hinzufügen alle Ihre Änderungen, die Aufspaltung push und erstellen eine Pull-Anforderung ändern von der Verzweigung an die master Haupt-Repository verschieben.

## <a name="step-4-update-the-fwlink-tool-required"></a>Schritt 4: Aktualisieren des FWLink Tools (erforderlich)

Alle FWLinks Artikel hinweisen überprüfen Sie FWLink-Tool. Zeigen Sie alle FWLinks am Inhalt. sind Sie nicht auf den Alias, der den Link besitzt, verknüpfen Sie ihn. Wenn der Besitzer die Verknüpfung aktualisieren, wird nicht Datei einen Ticket mit MSCOM der Verknüpfung geändert. Weitere Informationen - [internen Wiki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Manage%20inbound%20links%20to%20retired%20topics.aspx).

## <a name="step-5-remove-all-crosslinks-to-the-article-from-other-pages-on-azuremicrosoftcom-and-create-a-redirect-for-the-retired-page-if-appropriate-required"></a>Schritt 5: Entfernen alle Querverbindungen Artikel von anderen Seiten auf azure.microsoft.com und erstellen eine Umleitung für zurückgezogene Seite ggf. entsprechende ()

Sie müssen mit der Person zusammenarbeiten, verwaltet und aktualisiert die Dokumentation Zielseite für den Dienst für dieses Teil. Ansprechpartner, wenn Sie nicht wissen, wer diese Person ist der Content-Team. Wer verwaltet und aktualisiert die Zielseite Doc müssen zwei Dinge:

1. Durchsuchen Sie in Visual Studio die **gesamte** ACOM weblösung für Verweise auf die Datei deaktivieren. Entfernen Sie der Querverweise oder aktualisierte Referenz ersetzen. Sie müssen die HTML-Links sowie die zugehörige Ressourcenzeichenfolgen für HTML-Links zu entfernen. Weitere Informationen - siehe [interne Wiki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Create%20or%20edit%20a%20service%20landing%20page%20or%20left%20nav.aspx)

2. Wenn ein Ersatzartikel vorhanden ist, erstellen Sie eine Umleitung. Weitere Informationen - [internen Wiki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Remove%20published%20pages%20and%20request%20redirects.aspx)angezeigt.

3. Überprüfen Sie die Änderungen in das Repository.

## <a name="step-6-retire-the-article"></a>Schritt 6: Deaktivieren des Artikels

Nachdem Sie die vorherigen Schritte abgeschlossen haben und diese Änderungen sind, können Sie den Artikel aus dem Repository löschen. 

**Wichtig:** Wenn Sie Dateien löschen, verwenden Sie die `git add --all` Befehl.

## <a name="step-7-remove-links-from-msdn-required"></a>Schritt 7: Entfernen von Links aus MSDN (erforderlich)

Überprüfen Sie Content QA-Tool für fehlerhafte Links zum Thema zurückgezogen oder umbenannte und entfernen/Fix Hyperlinks in allen betroffenen MSDN-Themen.

## <a name="step-8-remove-cached-pages-from-search-engines-only-if-absolutely-necessary"></a>Schritt 8: Entfernen Sie zwischengespeicherte Seiten von Suchmaschinen (nur wenn unbedingt notwendig)

Tun Sie dies nur, wenn die Inhalte schnell durch rechtliche oder schwerwiegenden Kundenproblemen entfernt werden. Normaler Priorität Seite löschen sollten pro best Practices von Google nur natürliche Search Engine Prozesse behandelt werden. Besuchen Sie diese Seiten Suchmaschinen zwischengespeicherte Webseiten entfernen:

[Bing](https://www.bing.com/webmaster/tools/content-removal?rflid=1)
[Google](https://www.google.com/webmasters/tools/removals?pli=1)


### <a name="contributors-guide-links"></a>Mitwirkenden Guide links

- [Übersichtsartikel](./../README.md)
- [Index der Artikel](./contributor-guide-index.md)
