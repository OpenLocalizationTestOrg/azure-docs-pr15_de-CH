<properties 
    pageTitle="Benachrichtigung und e-Mail-Vorlagen in Azure API Management konfigurieren" 
    description="Informationen Sie zum Konfigurieren Benachrichtigung und e-Mail-Vorlagen in Azure API Management." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-configure-notifications-and-email-templates-in-azure-api-management"></a>Benachrichtigung und e-Mail-Vorlagen in Azure API Management konfigurieren

API-Management ermöglicht Benachrichtigungen für bestimmte Ereignisse konfigurieren und e-Mail-Vorlagen zu konfigurieren, mit denen Administratoren und Entwickler einer Instanz-Management-API kommunizieren. In diesem Thema wird die Benachrichtigung für die verfügbaren Ereignisse konfiguriert und Überblick zum Konfigurieren der e-Mail-Vorlagen für diese Ereignisse verwendet.

## <a name="publisher-notifications"> </a>Publisher-Benachrichtigung konfigurieren

Um Benachrichtigungen zu konfigurieren, klicken Sie auf **Verwalten** in Azure-Verwaltungsportal für Ihre API-Verwaltungsdienst. Dadurch gelangen Sie zu Publisher API Verwaltungsportal.

![Publisher-portal][api-management-management-console]

>Wenn Sie eine Dienstinstanz API Management noch nicht erstellt haben, finden Sie im Lernprogramm [Erste Schritte mit Azure API Management][] [Erstellen API Management Service][] .

Klicken Sie auf **Benachrichtigung** **API Management** Menü auf der linken Seite verfügbaren Benachrichtigungen anzeigen.

![Publisher-Benachrichtigung][api-management-publisher-notifications]

Folgende Ereignisse kann für Benachrichtigungen konfiguriert werden.

-   **Abonnement Anfragen (Genehmigung anfordern)** - die angegebene e-Mail-Empfänger und Benutzer erhalten e-Mail-Benachrichtigungen Abonnement Anfragen für API Erzeugnisse, die Zustimmung.
-   **Neue Abonnements** - die angegebene e-Mail-Empfänger und Benutzer erhalten e-Mail-Benachrichtigung über neue API Produkt-Abonnements.
-   **Anträge Gallery** - die angegebene e-Mail-Empfänger und Benutzer erhalten Benachrichtigungen der Galerie neue Anträge übermittelt werden.
-   **BCC** - die angegebene e-Mail-Empfänger und Benutzer erhalten e-Mail Blindkopien alle e-Mails für Entwickler.
-   **Neues Problem oder Kommentar** - die angegebene e-Mail-Empfänger und Benutzer erhalten per e-Mail benachrichtigt, wenn ein neues Problem oder Kommentar Developer Portal übermittelt.
-   **Konto schließen Meldung** - angegebenen e-Mail-Empfänger und Benutzer erhalten Benachrichtigungen ein Konto geschlossen wurde.
-   **Abonnement-Kontingentgrenze nähern** - die folgenden e-Mail-Empfänger und Benutzer erhalten Benachrichtigungen Abonnementverwendung nahe Verwendung Kontingent wird.

Für jedes Ereignis können Sie in das Textfeld e-Mail-Adresse e-Mail-Empfänger oder Benutzer aus einer Liste auswählen.

Um die e-Mail-Adressen zu übermittelnde anzugeben, geben sie in das Textfeld e-Mail-Adresse. Wenn Sie mehrere e-Mail-Adressen haben, trennen Sie diese mit Kommas.

![Empfänger der Benachrichtigung][api-management-email-addresses]

Festlegen benachrichtigt werden, klicken Sie auf **Empfänger hinzufügen**, aktivieren Sie das Kontrollkästchen neben der zu benachrichtigenden Benutzer zu und klicken Sie auf **OK**.

>Beachten Sie, dass nur Administratoren in der Liste angezeigt werden.

Nach dem Konfigurieren der Empfänger der Benachrichtigung, klicken Sie auf **Speichern** , um die aktualisierte Benachrichtigungsempfänger anzuwenden.

>Wenn die Registerkarte **Verleger Benachrichtigungen** verlassen weist Publisher-Portal Sie es ändert ungespeicherte sind.

## <a name="email-templates"> </a>Konfigurieren e-Mail-Vorlagen

API Management bietet e-Mail-Vorlagen für e-Mail-Nachrichten, die Verwaltung und den Dienst gesendet werden. Die folgenden e-Mail-Vorlagen dienen.

-   Einreichung des Antrags Katalog genehmigt
-   Abschiedsbrief Entwickler
-   Entwickler Kontingentgrenze nähert Benachrichtigung
-   Benutzer einladen
-   Neuen Kommentar zu einem Problem
-   Neues Problem erhalten
-   Neues Abonnement aktiviert
-   Abonnement erneuert Bestätigung
-   Abonnementanforderung abgelehnt
-   Abonnementsanfrage empfangen

Diese Vorlagen können geändert werden nach Bedarf.

Anzeigen und Konfigurieren von e-Mail-Vorlagen für Ihre API Management-Instanz auf **Benachrichtigung** **API Management** -Menü auf der linken Seite und wählen Sie die Registerkarte **E-Mail-Vorlagen** .

![E-Mail-Vorlagen][api-management-email-templates]

Anzeigen oder Ändern einer bestimmten Vorlage, aus der Dropdown-Liste **Vorlagen** auswählen.

![Liste der e-Mail-Vorlagen][api-management-email-templates-list]

Jeder e-Mail-Vorlage hat einen Betreff im nur-Text und eine Körperdefinition im HTML-Format. Jedes Element kann angepasst werden wie gewünscht.

![E-Mail-Vorlagen-editor][api-management-email-template]

**Die Parameterliste** enthält eine Liste von Parametern, die bei in den Betreff oder Nachrichtentext eingefügt werden festgelegten Wert ersetzt, wenn die e-Mail gesendet wird. Um Parameter einzufügen, platzieren Sie den Cursor, den Parameter gehen möchten und klicken Sie auf den Pfeil links neben dem Parameternamen.

Klicken Sie auf **Vorschau** oder **einen Test senden** , um festzustellen, wie die e-Mail suchen oder Test e-Mail.

>Beachten Sie, dass die Parameter nicht mit den tatsächlichen Werten bei der Vorschau oder senden einen Test ersetzt werden.
>
>Speichern der e-Mail-Vorlage, klicken Sie auf **Speichern**oder Abbrechen klicken Sie auf die **Abbrechen**.



[api-management-management-console]: ./media/api-management-howto-configure-notifications/api-management-management-console.png
[api-management-publisher-notifications]: ./media/api-management-howto-configure-notifications/api-management-publisher-notifications.png
[api-management-email-addresses]: ./media/api-management-howto-configure-notifications/api-management-email-addresses.png


[api-management-email-templates]: ./media/api-management-howto-configure-notifications/api-management-email-templates.png
[api-management-email-templates-list]: ./media/api-management-howto-configure-notifications/api-management-email-templates-list.png
[api-management-email-template]: ./media/api-management-howto-configure-notifications/api-management-email-template.png


[Configure publisher notifications]: #publisher-notifications
[Configure email templates]: #email-templates

[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Erste Schritte mit Azure API Management]: api-management-get-started.md
[Erstellen Sie eine Instanz der API Management service]: api-management-get-started.md#create-service-instance