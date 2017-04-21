<properties
pageTitle="Installieren und Einrichten von Tools für das Schreiben in GitHub"
description="Tools und Schritte zur Azure Inhalte in GitHub eingerichtet."
services="contributor-guide"
documentationCenter=""
authors="tysonn"  
manager="carolz" />

<tags
ms.service="contributor-guide"
 ms.devlang=""
 ms.topic="article"
  ms.tgt_pltfrm=""
  ms.workload=""
  ms.date="01/19/2015"
  ms.author="tysonn" />

#<a name="install-and-set-up-tools-for-authoring-in-github"></a>Installieren und Einrichten von Tools für das Schreiben in GitHub

Führen Sie die Schritte in diesem Artikel Tools zu Azure Dokumentation einrichten. Contributors unautorisiertes und gelegentlich können wahrscheinlich GitHub Benutzeroberfläche in Schritt 2 beschrieben.

Wenn Sie mit Git nicht vertraut sind, möchten einige Begriffe Git überprüfen: [https://help.github.com/articles/github-glossary](https://help.github.com/articles/github-glossary). StackOverflow Thread enthält zudem ein Glossar Git in diesen Schritten treten: [http://stackoverflow.com/questions/7076164/terminology-used-by-git](http://stackoverflow.com/questions/7076164/terminology-used-by-git)

## <a name="contents"></a>Inhalt

- [Erstellen Sie ein GitHub-Konto und richten Sie Ihres Profils ein](#create-a-github-account-and-set-up-your-profile)
- [Registrieren Sie sich für Disqus](#sign-up-for-disqus)
- [Bestimmen Sie, ob Sie wirklich Sie die restlichen Schritte müssen](#determine-whether-you-really-need-to-follow-the-rest-of-these-steps)
- [Berechtigungen auf GitHub](#permissions-in-github)
- [Git für Windows installieren](#install-git-for-windows)
- [Zwei-Faktor-Authentifizierung aktivieren](#enable-two-factor-authentication)
- [Installieren Sie einen Editor Abzug](#install-a-markdown-editor)
- [Atom konfigurieren](#configure-atom)
- [Verzweigung Repository und auf den Computer kopieren](#fork-the-repository-and-copy-it-to-your-computer)
- [Konfigurieren Sie Ihren Benutzernamen und die e-Mail lokal](#configure-your-user-name-and-email-locally)
- [Nächste Schritte](#next-steps)

## <a name="create-a-github-account-and-set-up-your-profile"></a>Erstellen Sie ein GitHub-Konto und richten Sie Ihres Profils ein

Beitrag zur Azure technischen Inhalten benötigen Sie ein Konto [GitHub](http://www.github.com) .

Wenn Microsoft Contributor sind müssen Sie GitHub Konto eingerichtet, damit Sie als Mitarbeiter von Microsoft eindeutig sind. Richten Sie Ihr Profil wie folgt:

- **Profilbild**: ein Bild Ihrer (erforderlich)
- **Name**: der vor- und Nachname (erforderlich)
- **E-Mail**: Microsoft EMailAdresse (optional)
- **Unternehmen**: Microsoft Corporation (erforderlich)
- **Ort**: Liste Ihren Standort (optional)

Ihr Profil sollte dieses Profil entsprechen:

<p align="center">
 ![Beispielprofil GitHub](./media/tools-and-setup/githubprofile.png)

## <a name="sign-up-for-disqus"></a>Registrieren Sie sich für Disqus

Alle veröffentlichten Azure technische Artikel hat einen Kommentar Stream-Dienst Disqus.

 ![Diskussion-logo](./media/tools-and-setup/discus.png)

Sind Sie ein Microsoft-Mitarbeiter und Autor oder Beitragender zu einem Artikel müssen Sie für Disqus anmelden, damit Sie im Kommentar Stream für Artikel können.

1. Melden Sie sich für ein Konto [http://www.disqus.com/](http://www.disqus.com/)
2. Füllen Sie Ihr Profil wie folgt:

 - **Vollständiger Name**: Ihren vollständigen Namen wie Microsoft Adressbuch Angebot sowie Klammern Informationen Ihr Alias ist plus @MSFT. Format: *zuerst zuletzt [alias@MSFT] *
 - **Standort**: der Standort
 - **Kurz vorgestellt**: Titel

## <a name="determine-whether-you-really-need-to-follow-the-rest-of-these-steps"></a>Bestimmen Sie, ob Sie wirklich Sie die restlichen Schritte müssen

Sie müssen nicht alle Schritte in diesem Artikel folgen. Es abhängig von Content Beitrag möchten oder müssen.

###<a name="submit-a-text-only-change-to-an-existing-article"></a>Senden Sie eine nur-Text-Änderung zu einem vorhandenen Artikel

Wenn Sie nur oder Texte aktualisieren, einen vorhandenen Artikel möchten, müssen Sie wahrscheinlich führen die restlichen Schritte. GitHub Abzug webbasierten Editor können Sie Ihre Änderungen. Klicken Sie einfach auf GitHub Link in dem Artikel, den Sie ändern möchten:

 ![Beispielprofil GitHub](./media/tools-and-setup/contributetogit.png)

 Klicken Sie das Bearbeitungssymbol GitHub Version des Artikels

 ![Beispielprofil GitHub](./media/tools-and-setup/editicon.PNG)

 Einfach zu verwendende Web-Editor, der Änderungen vereinfacht wird geöffnet. Sie müssen die Schritte in diesem Artikel ausführen.

###<a name="all-other-changes"></a>Alle anderen Änderungen
GitHub UI unterstützt die Erstellung neuer Dateien und Drag & Drop Bilder. Beim Arbeiten in der Benutzeroberfläche kann verwalten Zweige verwirrend jedoch normalerweise empfehlen wir die Tools installieren und lernen die Befehle zum Erstellen und Verwalten von Artikeln. Wenn Sie die Benutzeroberfläche verwenden möchten, finden Sie unter:

- [Erstellen von Dateien auf Github](https://github.com/blog/1327-creating-files-on-github)
- [Hochladen von Dateien in Ihre repositories](https://github.com/blog/2105-upload-files-to-your-repositories)

Die folgenden Arten von Arbeit empfehlen wir installieren und Verwenden der Informationen:

 - Wichtige Änderung eines Artikels
 - Erstellung und Veröffentlichung eines neuen Artikels
 - Neue Bilder hinzufügen oder Bilder aktualisieren
 - Einen Artikel aktualisieren über einen Zeitraum von Tagen ohne Veröffentlichung ändert dieser Tage
 - Erstellen von Inhalten für eine Version, die an einem bestimmten Tag zu einer bestimmten Zeit gehen

##<a name="permissions-in-github"></a>Berechtigungen auf GitHub

Jemand mit einem GitHub-Konto kann Azure technische Inhalte unserer öffentlichen Repository zur [https://github.com/Azure/azure-content](https://github.com/Azure/azure-content)hinzufügen. Es sind keine besonderen Berechtigungen erforderlich.

Sind Sie ein Microsoft-Uhr oder Schriftsteller arbeitet auf Azure Content Arbeit in unserem Content Repository Azure Inhalt Pr. Besuchen Sie [https://repos.opensource.microsoft.com](https://repos.opensource.microsoft.com ) um die Leseberechtigung anfordern, mit denen Sie Beiträge über private Repo - GitHub über die Schaltfläche anmelden > Klicken Sie auf Azure > Klicken Sie auf **Verknüpfung ein Team** oder **ein anderes Team beizutreten**, suchen und Mitglied **Azure Inhalt lesen** .

## <a name="install-git-for-windows"></a>Git für Windows installieren

Installieren Sie Git für Windows von [http://git-scm.com/download/win](http://git-scm.com/download/win). Dieser Download installiert das Versionskontrollsystem Git und Git Bash Befehlszeilen app, mit denen Sie interagieren mit dem lokalen Git Repository installiert.

Sie können die Standardeinstellungen übernehmen; die Befehle in der Windows-Befehlszeile werden gegebenenfalls die Option, die es ermöglicht.

<p align="center">
 ![Beispielprofil GitHub](./media/tools-and-setup/gitbashinstall.png)

(Hinweis: Dies ist nicht dasselbe wie "Github für Windows". "Github für Windows" ist ein anderes GUI-basiertes Tool, das auch funktioniert, wenn Sie Sie lesen möchten. [https://windows.github.com/](https://windows.github.com/))

## <a name="enable-two-factor-authentication"></a>Zwei-Faktor-Authentifizierung aktivieren

Wenn im privaten Inhalt-Repository arbeiten, müssen Sie auf Ihrem Konto GitHub zwei-Faktor-Authentifizierung (2FA) aktivieren. Es muss im privaten Repository.

Dazu führen Sie die Schritte sowohl die folgenden GitHub Hilfethemen:

- [Über zwei-Faktor-Authentifizierung](https://help.github.com/articles/about-two-factor-authentication/)
- [Erstellen von Zugriffstoken für Befehlszeilen verwenden](https://help.github.com/articles/creating-an-access-token-for-command-line-use/)

Wählen Sie beim Erstellen des Tokens alle Bereiche der Benutzeroberfläche Token erstellen ([Informationen über jeden Bereich](https://developer.github.com/v3/oauth/#scopes))

Nach dem Aktivieren 2FA müssen Sie das Zugriffstoken anstatt Ihr Kennwort GitHub an der Befehlszeile eingeben, beim Zugriff auf ein GitHub Repository über die Befehlszeile. Das Zugriffstoken ist nicht der Authentifizierungscode in einer Textnachricht zu erhalten, wenn Sie 2FA einrichten. Es ist eine lange Zeichenfolge, die etwa so aussieht: fdd3b7d3d4f0d2bb2cd3d58dba54bd6bafcd8dee. Einige Hinweise:

- Beim Erstellen der Zugriffstoken speichern Sie in einer Textdatei leicht zugänglich machen, wenn Sie sie benötigen.

- Später, wenn Sie das Token einfügen müssen, weiß zweierlei in die Befehlszeile einfügen:

 - Klicken Sie auf das Symbol oben links das Befehlszeilenfenster > Bearbeiten > einfügen.
 - Mit der rechten Maustaste des Symbols oben links im Fenster und wählen Sie Eigenschaften > Optionen > QuickEdit-Modus. Dies konfiguriert die Befehlszeile, mit der rechten Maustaste im Fenster Befehlszeile einfügen können.

## <a name="install-a-markdown-editor"></a>Installieren Sie einen Editor Abzug

Wir verfassen Inhalte einfacher "Abzug" Schreibweise der Dateien eher als komplexe "Markup" (HTML, XML usw.). Daher müssen Sie einen Abzug-Editor installieren.

- **Atom**: die meisten von uns GitHub Atom Abzug Editor verwenden: [http://atom.io](http://atom.io). Es benötigt keine Lizenz für die geschäftliche Nutzung. Es wurde eine Rechtschreibprüfung.

- **Editor**: können Sie Editor für eine sehr kompakte Option.

- **Prosa**: Dies ist eine einfache, elegante, Online und open Source Abzug Editor, eine Vorschau bietet. Besuchen Sie [http://prose.io](http://prose.io) und autorisieren Sie Prosa in Ihrem Repository.

- **[Visual Studio-Code](https://www.visualstudio.com/products/code-vs.aspx)** - Eintrag von Microsoft in diesem Bereich.

## <a name="configure-atom"></a>Atom konfigurieren

Wenn Sie Atom verwenden, müssen Sie einige Dinge eingerichtet.

- Atom standardmäßig 2 Leerzeichen für Registerkarten, aber Abzug 4 Leerzeichen erwartet. Wenn Sie den Standardwert von zwei lassen, Ihren Artikel sieht hervorragend für lokale Vorschau aber nicht wann importiert in Azure. So konfigurieren Sie Atom um 4 Leerzeichen - finden Sie diese Einstellung unter Datei > Settings > Einstellungen > Registerkarte Länge.
- Sie werden wahrscheinlich auch Soft Wrap in diesem Abschnitt auch aktivieren die entspricht der "Zeilenumbruch" im Editor.
- Abzug Vorschau klicken Pakete > Abzug Vorschau > Vorschau aktivieren/deaktivieren. STRG-UMSCHALT-M können Sie die Vorschau-HTML-Ansicht wechseln.

## <a name="fork-the-repository-and-copy-it-to-your-computer"></a>Verzweigung Repository und auf den Computer kopieren

1. Erstellen einer Verzweigung des Repositorys in GitHub - rechts oben auf der Seite zur und Verzweigung klicken. Wählen Sie ggf. Ihr Konto als Speicherort, in dem die Verzweigung erstellt werden soll. Erstellt eine Kopie des Repositorys in Ihrem-Konto Git Hub. Im Allgemeinen müssen technische Autoren und Programmmanager Verzweigung Azure-Inhalt-Pr, private Repo. Community Contributors müssen Öffentliche Repo Verzweigung Azure-Inhalt. Sie müssen nur einmal Verzweigung. nach der ersten Installation ggf. die Verzweigung auf einen anderen Computer kopieren müssen Sie nur die Befehle ausführen, die in diesem Abschnitt Repo auf Ihren Computer kopieren.  Wenn Sie Zweige der beiden Repositories erstellen, müssen Sie für jedes Repository eine Verzweigung erstellen.

2. Kopieren Sie die persönliche Zugriffstoken, das Sie von [https://github.com/settings/tokens](https://github.com/settings/tokens)erhalten haben. Sie können die Standardberechtigungen für das Token übernehmen.  Speichern Sie die persönliche Zugriffstoken in einer Textdatei zur späteren Wiederverwendung.

3. Kopieren Sie als Nächstes das Repository auf den Computer mit Ihren Anmeldeinformationen in der Befehlszeichenfolge eingebettet.  Hierzu öffnen Sie Git Bash und als Administrator ausführen. Geben Sie den folgenden Befehl in der Befehlszeile.  Dieser Befehl erstellt ein Verzeichnis azure-content(-pr) auf Ihrem Computer.  Wenn Sie den Standardspeicherort verwenden, es werden c:\users<your Windows user name>\azure-content(-pr).

Öffentliche Repo:

        git clone https://[your GitHub user name]:[token]@github.com/<your GitHub user name>/azure-content.git

Private Repo:

        git clone https://[your GitHub user name]:[token]@github.com/<your GitHub user name>/azure-content-pr.git

Beispielsweise könnte dieser Befehl Clone wie folgt aussehen:

        git clone https://smithj:b428654321d613773d423ef2f173ddf4a312345@github.com/smithj/azure-content-pr.git  

## <a name="set-remote-repository-connection-and-configure-credentials"></a>Remote-Repository Verbindung und Anmeldeinformationen konfigurieren

Erstellen Sie einen Verweis auf das Stamm-Repository eingeben dieser Befehle. Dies richtet auf GitHub, damit der letzten Änderung auf dem lokalen Computer abrufen und Änderungen auf GitHub zurückschieben. Dieser Befehl konfiguriert außerdem das Token lokal, damit Sie nicht Ihren Namen und Ihr Kennwort jedes Mal eingeben upstream Repo und Ihre Gabel GitHub Zugriff auf.

Öffentliche Repo:

        cd azure-content
        git remote add upstream https://[your GitHub user name]:[token]@github.com/Azure/azure-content.git
        git fetch upstream

Private Repo:

        cd azure-content-pr
        git remote add upstream https://[your GitHub user name]:[token]@github.com/Azure/azure-content-pr.git
        git fetch upstream

Dies dauert eine Weile. Nachdem Sie dies tun, müssen Sie erneut Verzweigung oder Ihre Anmeldeinformationen erneut eingeben. Sie müssen nur die Zweige auf einem lokalen Computer erneut kopieren, wenn Sie die Tools auf einem anderen Computer einrichten.


## <a name="configure-your-user-name-and-email-locally"></a>Konfigurieren Sie Ihren Benutzernamen und die e-Mail lokal

Um sicherzustellen, dass korrekt als Teilnehmer aufgeführt sind, müssen Sie Ihren Benutzernamen und die e-Mail lokal Git.

1. Starten Sie Git Bash und schalten Sie Sie in Azure Content oder Azure Inhalt Pr:

   ````
   cd azure-content
   ````

 oder

   ````
   cd azure-content-pr
   ````

2. Konfigurieren Sie Ihr Benutzername entsprechend Ihren Namen als Sie es in Ihrem Profil GitHub einrichten:

    ````
    git config --global user.name "John Doe"
    ````
3. Konfigurieren Sie Ihre e-Mail entsprechend die e-Mail in Ihrem Profil GitHub festgelegt; MSFT-Mitarbeiter sind, sollte Ihre e-Mail-Adresse MSFT sein:

    ````
    git config --global user.email "alias@example.com"
    ````
4. Typ `git config -l` überprüfen Sie Ihre lokalen Einstellungen den Benutzernamen und e-Mail in der Konfiguration korrekt sind.

##<a name="next-steps"></a>Nächste Schritte

- Der Typ des Inhalts im technischen Content Repo gehört und wissen, was nicht gehört. [Content Channel Anleitung](./content-channel-guidance.md)anzeigen
- Folgendermaßen Sie [Erstellen oder Ändern eines Artikels und senden es veröffentlichen](./git-commands-for-master.md).
- Kopieren Sie [die Vorlage Abzug](../markdown templates/markdown-template-for-new-articles.md) als Basis für einen neuen Artikel.
- Verwenden Sie [Diese Prüfliste Pull-Anforderung überprüfen erfüllen die Kriterien](./contributor-guide-pr-criteria.md) für die Zusammenführung.


###<a name="contributors-guide-navigation"></a>Mitwirkenden Guide navigation

- [Übersichtsartikel](./../README.md)
- [Index der Artikel](./contributor-guide-index.md)



<!--Anchors-->
[Use a customer-friendly voice]: #use-a-customer-friendly-voice
[Consider localization and machine translation]: #consider-localization-and-machine-translation
[other style and voice issues to watch for]: #other-style-and-voice-issues-to-watch-for


[Create a GitHub account and set up your profile]: #create-a-github-account-and-set-up-your-profile
[Determine whether you really need to follow the rest of these steps]: #determine-whether-you-really-need-to-follow-the-rest-of-these-steps
[Permissions in GitHub]: #permissions-in-github
[Install Git for Windows]: #install-git-for-windows
[Enable two-factor authentication]: #enable-two-factor-authentication
[Install a markdown editor]: #install-a-markdown-editor
[Fork the repository and copy it to your computer]: #fork-the-repository-and-copy-it-to-your-computer
[Install git-credential-winstore]: #install-git-credential-winstore
[Sign up for Disqus]: #sign-up-for-disqus
[Configure your user name and email locally]: #configure-your-user-name-and-email-locally
[Next steps]: #next-steps
