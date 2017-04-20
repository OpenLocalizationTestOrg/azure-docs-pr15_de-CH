<properties
    pageTitle="Dedizierte Gruppen in Active Directory Azure | Microsoft Azure"
    description="Übersicht über wie Gruppen Engagement in Azure Active Directory und wie sie erstellt werden."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""
    />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="curtand"/>

# <a name="dedicated-groups-in-azure-active-directory"></a>Dedizierte Gruppen in Active Directory Azure

In Azure Active Directory (Azure AD) Feature dedizierte Gruppen automatisch erstellt und füllt Mitgliedschaft für Azure AD vordefinierte Gruppen. Dedizierte Gruppen kann nicht hinzugefügt oder entfernt mit dem klassischen Azure-Portal, Windows PowerShell-Cmdlets oder programmgesteuert.

>[AZURE.NOTE] Dedizierte Gruppen müssen eine Azure AD Premium-Lizenz zugewiesen ist
>- der Administrator die Regel auf eine Gruppe verwaltet
>- alle Benutzer, die Mitglied der Gruppe von der Regel ausgewählt werden

**So aktivieren Sie dedizierte Gruppen**

1. Wählen Sie im [klassischen Azure-Portal](https://manage.windowsazure.com) **Active Directory**, und öffnen Sie das Verzeichnis der Organisation.

2. Wählen Sie die Registerkarte **Gruppen** , und öffnen Sie dann die Gruppe, die Sie bearbeiten möchten.

3. Wählen Sie die Registerkarte **Konfigurieren** und **Dedizierte Gruppen aktivieren** auf **Ja**festgelegt.

Nach Option dedizierte Gruppen aktivieren auf **Ja**festgelegt ist, können weitere Verzeichnis automatisch alle dedizierte Benutzergruppe erstellen aktivieren die **"alle Benutzer" Gruppe** **Ja**wechseln. Sie können dann auch den Namen des engagierter durch Eingabe Bearbeiten der **Anzeigename für "Alle Benutzer" Gruppe** Feld.

Die Gruppe alle Benutzer kann verwendet werden, die Benutzer in Ihrem Verzeichnis dieselben Berechtigungen zuweisen. Beispielsweise können Sie durch Zuweisen des Zugriffs für alle Benutzer dedizierten Gruppe dieser Anwendung alle Benutzer in Ihrem Verzeichnis auf eine SaaS-Anwendung erteilen.

Dedizierte alle Benutzer Gruppe enthält alle Benutzer im Verzeichnis, einschließlich Gäste und externe Benutzer. Benötigen Sie eine Gruppe, die externe Benutzer ausgeschlossen und können zu diesem Zweck erstellen eine Gruppe mit einem Attribut-basierte dynamische Regel wie folgt:

                (user.userPrincipalName -notContains "#EXT#@")

Verwenden Sie für eine Gruppe, die alle Gäste ausschließt eine Regel wie folgt:

                (user.userType -ne "Guest")

Informationen zum Erstellen von *erweiterten* (Regeln, die mehrere Vergleiche enthalten kann) für dynamische Mitgliedschaft finden Sie unter [Using Attribute erweiterte Regeln erstellen](active-directory-accessmanagement-groups-with-advanced-rules.md).


Diese Artikel bieten zusätzliche Informationen zu Azure Active Directory.

* [Verwalten des Zugriffs auf Ressourcen mit Azure Active Directory-Gruppen](active-directory-manage-groups.md)
* [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)
* [Was ist Azure Active Directory?](active-directory-whatis.md)
* [Integration von lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)
