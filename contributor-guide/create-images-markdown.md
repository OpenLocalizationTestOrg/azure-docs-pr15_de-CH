<properties
    pageTitle="Bilder in Abzug erstellen"
    description="Erklärt, wie Bilder in Abzug nach Richtlinien für Azure Repositories erstellen."
    services=""
    solutions=""
    documentationCenter=""
    authors="kenhoff"
    manager="ilanas"
    editor="tysonn"/>

<tags
    ms.service="contributor-guide"
    ms.devlang=""
    ms.topic="article"
    ms.tgt_pltfrm=""
    ms.workload=""
    ms.date="06/25/2015"
    ms.author="kenhoff" />

# <a name="create-images-in-markdown"></a>Bilder in Abzug erstellen

## <a name="image-folder-creation-and-link-syntax"></a>Bild Ordner erstellen und Link-syntax

Neue Artikel müssen Sie einen Ordner im folgenden Verzeichnis erstellen:

    /articles/<service-directory>/media/<article-name>/

Zum Beispiel:

    /articles/app-service/media/app-service-enterprise-multichannel-apps/

Nach dem Erstellen der Ordner und zusätzliche Bilder verwenden Sie die folgende Syntax in Ihrem Artikel Bilder erstellen:

```
![Alt image text](./media/article-name/your-image-filename.png)
```
Beispiel:

[Abzug Vorlage](../markdown%20templates/markdown-template-for-new-articles.md) beispielsweise anzeigen  Bild Verweis Links in dieser Vorlage Abzug sollen am unteren Rand der Vorlage.

## <a name="guidelines-specific-to-azuremicrosoftcom"></a>Richtlinien für azure.microsoft.com

Screenshots werden derzeit ist es nicht möglich, Reproduktionsschritte. Schreiben Sie den Inhalt, so dass der Inhalt ohne die Screenshots ggf. stehen.

Verwenden Sie die folgenden Richtlinien beim Erstellen und Dateien einschließlich:
- Freigeben Sie Dateien nicht über Dokumente. Kopieren Sie die Datei benötigen und für Ihr Thema Medienordner hinzufügen. Zwischen Dateien wird abgeraten, ist einfacher zu entfernen veraltete Inhalte und Bilder hält Repo bereinigen.

- Dateiformate: PNG-Dateien - verwenden sie höhere Qualität und deren Qualität während des Lokalisierungsprozesses. Die Qualität sowie beibehalten andere Dateiformate nicht. Das JPEG-Format ist zulässig, aber nicht bevorzugt.  Keine animierten GIF-Dateien.

- Rote Quadrate in Paint bereitgestellten Standardbreite verwenden (5 px) die Aufmerksamkeit auf bestimmte Elemente in Screenshots.  

    Beispiel:

    ![Dies ist ein Beispiel für ein rotes Quadrat als Legende verwendet.](./media/create-images-markdown/gs13noauth.png)

- Wenn es sinnvoll ist, können Sie Bilder zuschneiden, damit die Benutzeroberflächenelemente in voller Größe angezeigt werden. Stellen Sie sicher, dass der Kontext eindeutig, aber.

- Vermeiden Sie Leerzeichen an Screenshots. Wenn Sie einen Screenshot so, die Hintergrund an den Rändern lässt zuschneiden, fügen Sie einen Pixels graue Rahmen um das Bild.  Mit Paint verwenden Sie hellere Grau in der Standard-Farbpalette (0xC3C3C3). Verwenden eine anderen grafische app ist die RGB-Farbe R195, G195, 195. Können Sie problemlos einen grauen Rahmen um ein Bild in Visio - zu dazu, wählen das Bild, wählen Sie Zeile hinzufügen die richtige Farbe festgelegt ist, und ändern Sie die Linienstärke 1 1/2 pt.  Screenshots müssen einen grauen Rahmen von 1 Pixel Breite, weiße Bereiche des Screenshots in die Webseite Unschärfe.

    Beispiel:

    ![Dies ist ein Beispiel für einen grauen Rahmen Leerzeichen.](./media/create-images-markdown/agent.png)
    
    Ein Tool zum Automatisieren der erforderlichen Rahmen zu Bilder hinzufügen finden Sie unter [AddACOMBorder tool - erforderlich 1 Pixel graue Rahmen ACOM Bilder hinzufügen zu automatisieren](https://github.com/Azure/Azure-CSI-Content-Tools/tree/master/Tools/AddACOMImageBorder).

- Einen grauen Rahmen erforderlich konzeptionelle Bilder mit Leerzeichen nicht.  

    Beispiel:

    ![Dies ist ein Beispiel für eine konzeptionelle Darstellung mit Leerzeichen und keine grauen Rahmen.](./media/create-images-markdown/ic727360.png)

- Versuchen Sie nicht, ein Bild zu breit.  Bilder werden automatisch angepasst werden, sind zu breit. Größe manchmal aufgrund der Unschärfe, empfehlen wir, die Breite der Bilder 780 beschränken px und Größe Bilder vor ggf. manuell.

- Befehl Screenshots anzeigen  Wenn Ihr Artikel Schritte enthält, die Benutzer in einer Shell arbeiten, empfiehlt es Befehlsausgabe Screenshots zeigen. In diesem Fall sorgt die Shell Breite im Allgemeinen auf 72 Zeichen beschränken das Bild in der 780 px Breite Richtlinie bleibt. Vor einen Screenshot der Ausgabe, die Größe des Fensters so, dass nur der Befehl und Ausgabe (optional mit einer leeren Zeile rechts) angezeigt.

- Gesamte Screenshots der Windows Wenn möglich. Wenn einen Screenshot von einem Browser-Fenster, Größe Browserfenster auf 780 px breit oder weniger, und halten Sie die Höhe des Browserfensters als möglichst kurz wie die Anwendung in das Fenster passt.

    Beispiel:

    ![Dies ist ein Beispiel für einen Browser Fenster Screenshot.](./media/create-images-markdown/helloworldlocal.png)

- Vorsicht mit Informationen in Screenshots angezeigt wird.  Geben Sie keine interne oder persönlichen Informationen.

- Kunst oder Diagramme mithilfe der offiziellen Symbole in der Cloud und Enterprise-Symbol und Symbol. Eine öffentliche Gruppe ist unter http://aka.ms/CnESymbols verfügbar.

- Ersetzen von persönlichen oder privaten Informationen Screenshots mit Platzhaltertext in spitze Klammern eingeschlossen. Dies schließt Benutzernamen, Abonnement-IDs und andere Informationen. Namen können mit einem [fiktiven Namen genehmigt](https://aka.ms/ficticiousnames)(nur Mitarbeiter Link) ersetzt werden. Verwenden Sie Tipp Stift oder Marker verdecken oder Weichzeichnen von persönlichen oder privaten Informationen nicht in Paint.

  Das folgende Bild wurde ordnungsgemäß aktualisiert die tatsächliche **Abonnement-ID** durch Platzhalter ersetzen:

  ![Private Informationen durch Platzhalter ersetzt](./media/create-images-markdown/placeholder-in-screenshot-correct.png)

### <a name="contributors-guide-links"></a>Mitwirkenden Guide Links

- [Übersichtsartikel](./../README.md)
- [Index der Artikel](./contributor-guide-index.md)
