<properties 
    pageTitle="Wie verwalten Sie Benutzerkonten in Azure API Management | Microsoft Azure" 
    description="Informationen Sie zum Erstellen und Laden in Azure API Management" 
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

# <a name="how-to-manage-user-accounts-in-azure-api-management"></a>Verwalten von Benutzerkonten in Azure API Management

API-Verwaltung sind Entwickler Benutzer APIs mit Management-API verfügbar. Dieses Handbuch wird zum Erstellen und Laden Entwickler APIs und Produkte, mit Ihrem Management-API zur Verfügung stellen. Informationen zum programmgesteuerten Verwalten von Benutzerkonten finden Sie in der Dokumentation [Benutzerentität](https://msdn.microsoft.com/library/azure/dn776330.aspx) [Management REST-API-](https://msdn.microsoft.com/library/azure/dn776326.aspx) Referenz.

## <a name="create-developer"> </a>Einen neuen Entwickler erstellen

Klicken Sie zum Erstellen neuen Entwicklers **Verwalten** in Azure-Verwaltungsportal für Ihre API-Verwaltungsdienst. Dadurch gelangen Sie zu Publisher API Verwaltungsportal. Wenn Sie eine Dienstinstanz API Management noch nicht erstellt haben, finden Sie im Lernprogramm [Erste Schritte mit Azure API Management][] [Erstellen API Management Service][] .

![Publisher-portal][api-management-management-console]

Klicken Sie auf **Benutzer** aus der **API-** Menü auf der linken Seite und klicken Sie dann auf **Benutzer hinzufügen**.

![Entwickler erstellen][api-management-create-developer]

Geben Sie **E-Mail**, **Kennwort**und **Namen** für die neue Entwickler, und klicken Sie auf **Speichern**.

![Entwickler erstellen][api-management-add-new-user]

Standardmäßig neu erstellten entwicklerkonten **aktiv**sind und **die Entwicklergruppe** zugeordnet.

![Neue Entwickler][api-management-new-developer]

Entwicklerkonten in einem **aktiven** Zustand können auf alle APIs für die haben Abonnements verwendet werden. Um den neu erstellten Entwickler weitere Gruppen zuzuordnen, finden Sie unter [Gruppen mit Entwicklern zuzuordnen][].

## <a name="invite-developer"> </a>Entwickler einladen

Um Entwickler einzuladen, klicken Sie auf **Benutzer** aus der **API-** Menü auf der linken Seite und dann auf **Benutzer einladen**.

![Entwickler einladen][api-management-invite-developer]

Geben Sie den e-Mail-Adresse des Entwicklers, und klicken Sie auf **Laden**.

![Entwickler einladen][api-management-invite-developer-window]

Eine Meldung wird angezeigt, aber neu geladenen Developer erscheint nicht in der Liste erst nach Annahme der Einladung. 

![Bestätigung einladen][api-management-invite-developer-confirmation]

Wenn Entwickler eingeladen, wird eine e-Mail an den Entwickler. Diese e-Mail wird mithilfe einer Vorlage generiert und ist anpassbar. Weitere Informationen finden Sie unter [Konfigurieren e-Mail-Vorlagen][].

Nachdem die Einladung angenommen, wird das Konto aktiviert.

## <a name="block-developer"></a> Deaktivieren oder Reaktivieren einer Entwicklerkonto

Neu erstellte oder eingeladene entwicklerkonten sind standardmäßig **aktiv**. Ein Entwicklerkonto zu deaktivieren, klicken Sie auf **Blockieren**. Um eine gesperrte Entwicklerkonto zu reaktivieren, klicken Sie auf **Aktivieren**. Blockierte Entwicklerkonto nicht Entwickler zugreifen oder APIs aufrufen. Um ein Benutzerkonto zu löschen, klicken Sie auf **Löschen**.

![Block-Entwickler][api-management-new-developer]

## <a name="reset-a-user-password"></a>Zurücksetzen eines Benutzerkennworts

Um das Kennwort für ein Benutzerkonto zurückzusetzen, klicken Sie auf den Namen des Kontos.

![Kennwort zurücksetzen][api-management-view-developer]

Klicken Sie auf **Kennwort zurücksetzen** , um einen Link zum Zurücksetzen des Kennworts senden.

![Kennwort zurücksetzen][api-management-reset-password]

Programmgesteuertes Arbeiten mit Benutzerkonten finden Sie im [Benutzerentität](https://msdn.microsoft.com/library/azure/dn776330.aspx) [Management REST-API-](https://msdn.microsoft.com/library/azure/dn776326.aspx) Referenz. Um das Kennwort eines Benutzerkontos mit einem bestimmten Wert zurückzusetzen, Sie verwenden die Operation [Benutzer aktualisieren](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser) und geben Sie das gewünschte Kennwort.

## <a name="pending-verification"></a>Ausstehende Überprüfung

![Ausstehende Überprüfung][api-management-pending-verification]

## <a name="next-steps"> </a>Nächste Schritte

Sobald ein Entwicklerkonto erstellt wurde, können Rollen zuordnen und Produkte und APIs abonnieren. Weitere Informationen finden Sie unter [Erstellen und Verwenden von Gruppen][].


[api-management-management-console]: ./media/api-management-howto-create-or-invite-developers/api-management-management-console.png
[api-management-add-new-user]: ./media/api-management-howto-create-or-invite-developers/api-management-add-new-user.png
[api-management-create-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png
[api-management-invite-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer.png
[api-management-new-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-new-developer.png
[api-management-invite-developer-window]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-window.png
[api-management-invite-developer-confirmation]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-confirmation.png
[api-management-pending-verification]: ./media/api-management-howto-create-or-invite-developers/api-management-pending-verification.png
[api-management-view-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-view-developer.png
[api-management-reset-password]: ./media/api-management-howto-create-or-invite-developers/api-management-reset-password.png
[]: ./media/api-management-howto-create-or-invite-developers/.png



[Create a new developer]: #create-developer
[Invite a developer]: #invite-developer
[Deactivate or reactivate a developer account]: #block-developer
[Next steps]: #next-steps
[Zum Erstellen und Verwenden von Gruppen]: api-management-howto-create-groups.md
[Zuordnen von Gruppen zu Entwickler]: api-management-howto-create-groups.md#associate-group-developer

[Erste Schritte mit Azure API Management]: api-management-get-started.md
[Erstellen Sie eine Instanz der API Management service]: api-management-get-started.md#create-service-instance
[Konfigurieren Sie e-Mail-Vorlagen]: api-management-howto-configure-notifications.md#email-templates