<properties title="" pageTitle="Dateinamen und Speicherorte für Azure technische Artikel" description="Erläutert die Dateistruktur für Artikel und die Namenskonventionen, die Sie befolgen sollten, wenn Sie einen neuen Artikel erstellen." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="required" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="03/14/2016" ms.author="tysonn" />

#<a name="file-names-and-locations-for-azure-technical-articles"></a>Dateinamen und Speicherorte für Azure technische Artikel

In unseren technischen Content Repository verwenden wir einen einzelnen Ordner (der **Artikel** ) für alle Artikel. Gibt es keine Ordnerhierarchie - alle Artikel live in Flatfile-Struktur. Erstellen von Ordnern mit ihnen können Ihre Artikel veröffentlicht werden.

Anstelle einer als ein Prinzip verwenden wir eine strikte Namenskonventionen, Themen eindeutig identifiziert und zu auf Erkennbarkeit im Web.

Hier ist, was Sie wissen müssen:

+ [Regeln]
+ [Muster]
+ [Standard-Beispiele]
+ [Marketplace-Inhalt]
+ [Datei Genehmigung]

##<a name="rules"></a>Regeln

- Dateinamen können nur Kleinbuchstaben, Zahlen und Bindestriche enthalten. 
- Keine Leerzeichen oder Interpunktionszeichen. Verwenden Sie Bindestriche zum Trennen von Wörtern und Zahlen im Dateinamen.
- Mehr als 80 Zeichen - Dies ist eine Veröffentlichung Systemgrenze
- Mit Verben, die bestimmte wie entwickeln, kaufen, erstellen, beheben. Keine Wörter - Wert.
- Keine kleinen - keine Suchbegriffe ein, und die in oder, etc..
- Alle Dateien müssen in Abzug und die Erweiterung .md.
- Schreiben Sie die Worte; Vermeiden Sie oder nicht mehr benötigte Akronyme in Dateinamen

Akronyme und Initialisms in Dateinamen - spezifische Richtlinien:

- Azure Dienstnamen nicht abgekürzt - sollten die ersten Wörter des Dateinamens der Standard Azure Service oder Technologie Namen geschrieben. 
-   Rm oder Arm nicht als Akronyme in einen Dateinamen zulassen
- Andere Abkürzungen Industriestandard sind ggf. in Dateinamen zulässig. 

##<a name="pattern"></a>Muster

Hier wird die:

 **Service-Platform-Language-Content-Product-Version.MD**

Verwenden Sie die Teile des Musters, die die Liste der Artikel im Repository ein vorhandener Eindruck und gelten. Namen, die nicht mit einer Plattform Dev beginnen oder Diensten sind wahrscheinlich verdächtige und überfälliger durch.

##<a name="standard-examples"></a>Standard-Beispiele

Hier sind einige Beispiele für gültige Namen, die dem Muster folgen. :

- Cloud-Services-Dotnet-continuous-delivery.md
- Mobile-Dienste-Ios-Get-started.md
- Documentdb Verwalten von account.md
- Mobile-Services-dotnet-Backend-Get-Started-Settings-Sync.MD
- Active-Directory-Java-Authenticate-Users-Access-Control-Eclipse.MD
- Virtual-Machines-Install-Windows-Server-2008r2.MD
- Cache-Aspnet-Session-State-provider
- Azure-SDK-dotnet-Release-Notes-2-8
- storsimple-Disaster-Recovery-Using-Azure-Site-Recovery

##<a name="marketplace-content"></a>Marketplace-Inhalt

Um Inhalt zu unterscheiden, die sich auf Partner zu Azure Marketplace, starten Sie Dateinamen mit "Marketplace". Diese Inhalte sollten nicht zu häufig die meisten Partner Inhalte auf den Partner-Websites erstellt werden.

- Marketplace-MongoDB-Virtual-Machines-Install-Windows-Server-2008r2.MD

##<a name="file-name-approval"></a>Datei Genehmigung

Es ist die Aufgabe der Gruppe von Pull-Anforderung Bearbeiter Dateinamen überprüfen, wenn eine neue Datei in das Repository zum ersten Mal übermittelt wird. Pull-Anforderung Prüfer sollte den Dateinamen überprüfen und Feedback über den Anforderungsstream Kommentar Pull ggf. ändern. Der Dateiname muss korrigiert werden, bevor die Pull-Anforderung akzeptiert wird. Beitragende können problemlos Update ausstehende Pull-Anforderung drücken.

##<a name="folder-names-in-the-repo"></a>Ordnernamen in repo

Ordner sollte nur für Dienste und der Dateiname übereinstimmen Slug Service. Verwenden Sie nur Buchstaben und Bindestrichen und verwenden Sie Kleinbuchstaben. Einholen der Genehmigung vom Repository-Administrator, bevor Sie einen neuen Ordner erstellen, der nicht für einen freigegebenen Dienst ist.

##<a name="changing-case-in-file-names"></a>Bei Dateinamen ändern

Windows-Betriebssysteme Groß-und Kleinschreibung, so benötigen Sie einen Dateinamen Gehäuse beheben ändern, es besser grundlegend ändern, wenn Sie auf einem Linux oder Mac ändern können Zum Beispiel:

  Biztalk-administration-and-Development-Task-List-in-BizTalk-Services--> biztalk-services-administration-and-development-task-list

Verwenden Sie den folgenden Befehl, um eine Datei umzubenennen:
```
  git mv <articles/service-folder/current-file-name.md> <articles/service-folder/new-file-name>
```

###<a name="contributors-guide-links"></a>Mitwirkenden Guide Links

- [Übersichtsartikel](./../README.md)
- [Index der Artikel](./contributor-guide-index.md)


<!--Anchors-->
[Regeln]: #rules
[Muster]: #pattern
[Standard-Beispiele]: #standard-examples
[Marketplace-Inhalt]: #marketplace-content
[Datei Genehmigung]: #file-name-approval
