<properties pageTitle="Git Befehle für einen neuen Artikel erstellen oder einen vorhandenen Artikel aktualisieren" description="Schritte für die Arbeit mit technischen Azure content Repositories GitHub." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="carolz" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="01/16/2015" ms.author="tysonn" />

# <a name="git-commands-for-creating-a-new-article-or-updating-an-existing-article"></a>Git Befehle für einen neuen Artikel erstellen oder einen vorhandenen Artikel aktualisieren


## <a name="standard-process-working-from-master"></a>Standardverfahren (Master an)
Führen Sie die Schritte in diesem Artikel Niederlassung arbeiten auf Ihrem Computer erstellen, damit ein neuer Abschnitt technische azure.microsoft.com erstellen oder einen vorhandenen Artikel aktualisieren.

1. Starten Sie Git Bash (oder das Befehlszeilenprogramm Git verwenden).

 **Hinweis:** Wenn in öffentlichen Repository Arbeit ändern Sie Azure Inhalt Pr Azure-Inhalt in den Befehlen.

2. Ändern Sie in Azure Inhalt Pr:

        cd azure-content-pr
3. Schauen Sie sich den master Zweig:

        git checkout master

4. Erstellen Sie neue Arbeit Niederlassung abgeleitet aus der master:

        git pull upstream master:<working branch>


5. Verschieben Sie in der Niederlassung arbeiten:

        git checkout <working branch>

6. Lassen Sie die Verzweigung wissen arbeiten Zweig erstellt:

        git push origin <working branch>

7. Erstellen Sie neuen Artikel oder ändern Sie einen vorhandenen Artikel. Verwenden Sie Windows Explorer öffnen und Erstellen neuer Abzug Atom (http://atom.io) als Abzug Editor verwenden. Nach dem Erstellen oder Änderung Ihrer Artikel und Bilder gehen Sie zum nächsten Schritt.

8. Hinzufügen und vorgenommenen Änderungen:

        git add .
        git commit –m "<comment>"
        
   Oder nur bestimmte hinzufügen Dateien geändert:

        git add <file path>
        git commit –m "<comment>"

   Wenn Sie Dateien haben Sie verwenden:
   
        git add --all
        git commit -m "<comment>"

9. Aktualisieren der Zweigstelle arbeiten mit aus upstream:

        git pull upstream master

10. Drücken Sie die Änderung der Aufspaltung GitHub:

        git push origin <working branch>

12. Wenn Sie bereit sind, Ihre Inhalte in den vorgelagerten master Zweig für staging, Überprüfung und Veröffentlichung in GitHub UI erstellen Sie eine Pull-Anforderung von der Verzweigung master Zweig.

13. Ein Mitarbeiter arbeiten im privaten Repository vorgenommenen Absenden werden automatisch bereitgestellt und staging Link in Pull-Anforderung geschrieben. Die bereitgestellten Inhalte und Anmelden von Pull-Anforderung Kommentare **#Sign-off** -Kommentar hinzufügen.  Dies bedeutet, dass die Änderungen live abgelegt werden können.  Möchten Sie die Pull-Anforderung angenommen - Wenn Sie nur die Änderungen - staging fügen Sie hinzu **#Hold-off** Hinweis Pull-Anforderung.

14. Pull-Anforderung Acceptor Pull-Anforderung prüft, gibt Feedback und Pull-Anforderung akzeptiert. 

15. Optional die veröffentlichten Artikel oder Änderungen überprüfen

 http://Azure.Microsoft.com/Documentation/articles/*name-of-your-article-without-the-MD-extension*

## <a name="publishing"></a>Veröffentlichen

- Artikel 10:00 Uhr und 3:00 PM Pacific Time, Montag bis Freitag. Es dauert bis zu 30 Minuten für Artikel nach Veröffentlichung erscheinen. Beachten Sie, dass die Pull-Anforderung hat eine Pull-Anforderung Bearbeiter zusammengeführt werden, bevor die Änderung in der nächsten geplanten Veröffentlichung ausführen aufgenommen werden können. Sie müssen mit der Pull Anforderung Prüfer voraus um sicherzustellen, dass eine Pull-Anforderung für eine bestimmte Veröffentlichung ausführen zusammengeführt wird. Andernfalls werden PRs in der Reihenfolge überprüft übermittelt wurden.

- Ein Mitarbeiter im privaten Repository hingegen unterliegen alle Pull-Anfragen Validierungsregeln, die beseitigt werden, bevor die Pull-Anforderung zusammengeführt werden kann. 

## <a name="working-with-release-branches"></a>Arbeiten mit Version

Wenn Sie mit einer Version arbeiten, ist der beste Weg Niederlassung arbeiten aus der Freigabe erstellen diese Befehlssyntax verwenden:

    git checkout upstream/<upstream branch name> -b <local working branch name>

Dadurch Zweig direkt aus dem upstream Zweig lokale Zusammenführung vermeiden.

