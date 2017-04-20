<properties 
    pageTitle="Erste Schritte mit Azure Suche Management REST API | Microsoft Azure | Gehostete Cloud-Suchdienst" 
    description="Verwalten der Cloud gehostete Azure-Suchdienst mit einer Management-REST-API" 
    services="search" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="jhubbard" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/17/2016" 
    ms.author="heidist"/>

# <a name="get-started-with-azure-search-management-rest-api"></a>Erste Schritte mit Azure Search Management REST-API
> [AZURE.SELECTOR]
- [Portal](search-manage.md)
- [PowerShell](search-manage-powershell.md)
- [REST-API](search-get-started-management-api.md)

Azure Suche REST Management-API ist eine Alternative zum Ausführen von Verwaltungsaufgaben im Portal. Servicemanagement enthalten erstellen oder Löschen des Dienstes, den Dienst Skalierung und Verwaltung von Schlüsseln. Dieses Lernprogramm enthält eine Beispiel-Clientanwendung, die Servicemanagement-API veranschaulicht. Zusätzlich zum Ausführen des Beispiels in lokalen Umgebung erforderlichen Konfigurationsschritte.

Um dieses Lernprogramm benötigen Sie:

- Visual Studio 2012 oder 2013
- Beispiel zum Client Anwendung herunterladen

Abschluss des Lernprogramms, zwei Dienste bereitgestellt: Azure Suche und Azure Active Directory (AD). Außerdem erstellen Sie eine Anwendung, die Vertrauensstellung zwischen der Clientanwendung und den Endpunkt des Ressource-Managers in Azure einrichtet.

Ein Azure-Konto zum Bearbeiten dieses Lernprogramms benötigen.


##<a name="download-the-sample-application"></a>Beispielanwendung herunterladen

Diese praktische Einführung basiert auf einer Windows-Konsole-Anwendung geschrieben in C#, bearbeiten und Ausführen in Visual Studio 2012 oder 2013

Die Client-Anwendung finden Sie auf Github [Azure Suche .NET Management API-](https://github.com/Azure-Samples/search-dotnet-management-api/)Demo.


##<a name="configure-the-application"></a>Konfigurieren Sie die Anwendung

Vor dem Ausführen der beispielanwendung müssen Sie Authentifizierung aktivieren, sodass Anfragen von der Clientanwendung zum Ressourcen-Manager Endpunkt akzeptiert werden können. Authentifizierung Anforderung stammt der [Azure-Ressourcen-Manager](https://msdn.microsoft.com/library/azure/dn790568.aspx)ist die Grundlage für alle Portal-Vorgänge über eine API, einschließlich Suche Servicemanagement angefordert. Servicemanagement-API Azure Suche ist eine Erweiterung der Azure-Ressourcen-Manager und abhängigen erbt.  

Azure Ressourcenmanager benötigt als der Identitätsanbieter Azure Active Directory-Dienst. 

Ein Zugriffstoken zu erhalten, die Anfragen zu den Ressourcen-Manager ermöglicht enthält die Clientanwendung ein Codesegment, das Active Directory aufruft. Das Codesegment sowie die erforderlichen Schritte zur Verwendung des Codesegments wurden aus diesem Artikel ausgeliehen: [Authentifizierung Azure Resource Manager benötigt](http://msdn.microsoft.com/library/azure/dn790557.aspx).

Sie gehen in den oben stehenden Link oder gehen Sie in diesem Dokument möchten Sie das Tutorial Schritt für Schritt.

In diesem Abschnitt werden die folgenden Aufgaben ausführen:

1. Erstellen eines Active Directory-Dienstes
1. Erstellen einer AD-Anwendung
1. Konfigurieren Sie die Anwendung AD über der Beispielclientanwendung heruntergeladene registrieren
1. Laden der Beispielclientanwendung mit Werten zu Autorisierung für Anfragen verwenden wollen

> [AZURE.NOTE] Diese Links Hintergrundinformationen über die Verwendung von Azure Active Directory zum Authentifizieren von Clientanforderungen an den Ressourcen-Manager: [Azure Ressourcenmanager](http://msdn.microsoft.com/library/azure/dn790568.aspx), [Azure Ressourcenmanager Authentifizierung fordert](http://msdn.microsoft.com/library/azure/dn790557.aspx)und [Azure Active Directory](http://msdn.microsoft.com/library/azure/jj673460.aspx).

###<a name="create-an-active-directory-service"></a>Erstellen eines Active Directory-Dienstes

1. [Azure-Portal](https://manage.windowsazure.com)anmelden.

2. Bildlauf im linken Navigationsbereich, und klicken Sie auf **Active Directory**.

4. Klicken Sie auf **neu** , um die **App**öffnen | **Active Directory**. In diesem Schritt erstellen Sie einen neuen Active Directory-Dienst. Dieser Dienst hostet die Anwendung wenige jetzt definieren müssen. Erstellen einer neuen unterstützt das Lernprogramm aus anderen Programmen zu isolieren, die Sie bereits in Azure gehostet werden können.

5. Klicken Sie auf **Verzeichnis** | **benutzerdefinierte**.

6. Geben Sie einen Dienstnamen, Domäne und Standort. Die Domäne muss eindeutig sein. Klicken Sie auf das Häkchen, um den Dienst zu erstellen.

     ![][5]

###<a name="create-a-new-ad-application-for-this-service"></a>Erstellen einer neuen AD-Anwendung für diesen Dienst

1. Wählen Sie "SearchTutorial" Active Directory-Dienst, den Sie gerade erstellt haben.

2. Klicken Sie im oberen Menü auf **Programme**. 
 
3. Klicken Sie auf **eine Anwendung**. Eine Anwendung speichert Informationen über Clientanwendungen, die als Identitätsanbieter verwendet.  
 
4. Auswählen **von meinem Unternehmen entwickelte Anwendung hinzufügen**. Diese Option bietet überschneiden für Applikationen, die nicht in der Galerie veröffentlicht. Da die Clientanwendung nicht Teil der Galerie, ist dies die richtige Wahl für dieses Lernprogramm.

     ![][6]
 
5. Geben Sie einen Namen wie "Azure-Suche-Manager".

6. Wählen Sie **Native Client-Anwendung** für Anwendung. Dies ist für die beispielanwendung. Es ist ein Windows (Konsole) Clientanwendung kein Webanwendung.

     ![][7]
 
7. Geben Sie im URI umleiten "http://localhost/Azure-Search-Manager-App". Dieser URI, welche Azure Active Directory User-Agent auf eine Authentifizierungsanfrage OAuth 2.0 umleitet. Der Wert muss kein physischen Endpunkt aber muss ein gültiger URI sein. 

    Für die Zwecke dieses Lernprogramms Wert kann alles sein, aber was Sie eingeben, eine erforderliche Eingabe für die administrative Verbindung in diesem Beispiel werden. 
 
7. Klicken Sie zum Erstellen der Active Directory-Anwendung. "Azure-Suche-Manager-App" sollte im linken Navigationsbereich angezeigt werden.

###<a name="configure-the-ad-application"></a>Konfigurieren der Anwendungdes
 
9. Klicken Sie auf Anwendung AD "Azure-Suche-Manager-App", die Sie gerade erstellt haben. Sie sollten im linken Navigationsbereich aufgeführt sehen.

10. Klicken Sie im oberen Menü auf **Konfigurieren** .
 
11. Scrollen Sie Berechtigungen, und wählen Sie **Azure Management-API**. In diesem Schritt geben Sie die API (in diesem Fall der Azure-Ressourcen-Manager-API) benötigt Zugriff auf die Client-Anwendung, mit der Zugriffsebene erforderlich.

12. Delegierte Berechtigungen klicken Sie auf die Dropdown-Liste und wählen Sie **Access Azure Service Management (Vorschau**).
 
     ![][8]
 
13. Speichern. 

Lassen Sie die Konfigurationsseite Anwendung geöffnet. Im nächsten Schritt werden Sie Werte auf dieser Seite kopieren und in der beispielanwendung eingeben.

###<a name="load-the-sample-application-program-with-registration-and-subscription-values"></a>Laden der Beispiel-Anwendung mit Registrierung und Abonnement

In diesem Abschnitt bearbeiten Sie die Projektmappe in Visual Studio durch gültige Werte vom Portal aus.
Die Werte, die Sie hinzufügen oben "Program.cs" angezeigt:

        private const string TenantId = "<your tenant id>";
        private const string ClientId = "<your client id>";
        private const string SubscriptionId = "<your subscription id>";
        private static readonly Uri RedirectUrl = new Uri("<your redirect url>");

Wenn Sie noch nicht [die beispielanwendung von Github heruntergeladen](https://github.com/Azure-Samples/search-dotnet-management-api/)haben, benötigen Sie es für diesen Schritt.

1. Öffnen Sie in Visual Studio **ManagementAPI.sln** .

2. Öffnen Sie Program.cs.

3. Geben `ClientId`. Kopieren Sie aus der Konfigurationsseite Anwendung offen aus dem vorherigen Schritt die Client-ID auf der Konfigurationsseite des AD-Anwendung im Portal und Program.cs einfügen.

4. Geben `RedirectUrl`. URI Umleiten von demselben Portalseite kopieren und Program.cs einfügen.

    ![][9]

5. Bereitstellen`TenantID.` 
    - Gehen Sie zurück zu Active Directory | SearchTutorial (Dienst). 
    - Klicken Sie in der oberen Leiste auf **Applications** . 
    - Klicken Sie auf **Ansicht Endpunkte** am unteren Rand der Seite. 
    - Kopieren Sie OAUTH 2.0 Autorisierungsendpunkt am unteren Rand der Liste 
    - TenantID alle URI-Parameter außer Mieter ID Zuschneiden fügen Sie Endpunkt ein

    Wenn "https://login.windows.net/55e324c7-1656-4afe-8dc3-43efcd4ffa50/oauth2/authorize?api-version=1.0", löschen Sie alles außer "55e324c7-1656-4afe-8dc3-43efcd4ffa50".

    ![][10]

6. Geben `SubscriptionID`.
    - Gehen Sie zur Hauptseite des Portals.
    - Klicken Sie unten im linken Navigationsbereich **Settings** .
    - Die Registerkarte Abonnements kopieren Sie die Abonnement-ID und fügen Sie "Program.cs ein".

7. Speichern Sie und erstellen Sie die Projektmappe.


##<a name="explore-the-application"></a>Untersuchen der Anwendungdes

Fügen Sie einen Haltepunkt am ersten Methodenaufruf, damit Sie das Programm schrittweise ausführen können. Drücken Sie **F5** , um die Anwendung auszuführen, und drücken Sie **F11** , um den Code schrittweise durchlaufen.

Beispielanwendung erstellt Azure Search kostenlos für ein Azure-Abonnement. Kostenlos für Ihr Abonnement bereits vorhanden, wird die beispielanwendung fehl. Nur eine freie Suchdienst pro Abonnement ist zulässig.

1. Öffnen Sie Program.cs im Projektmappen-Explorer und wechseln Sie zur Funktion Main (String [] void). 
 
3. Beachten Sie, dass **ExecuteArmRequest** gegen den Endpunkt Azure Resource Manager ausführen `https://management.azure.com/subscriptions` für eine angegebene `subscriptionID`. Diese Methode wird in der gesamten Anwendung mithilfe der Azure-Ressourcen-Manager-API oder Suche Management API-Operationen verwendet.

3. Anfragen an Azure-Ressourcen-Manager müssen authentifiziert und autorisiert werden. Dies geschieht mithilfe der **GetAuthorizationHeader** -Methode wird von der **ExecuteArmRequest** -Methode [authentifizieren Azure Ressourcenmanager Anfragen](http://msdn.microsoft.com/library/azure/dn790557.aspx)entlehnt. Hinweis **GetAuthorizationHeader** ruft `https://management.core.windows.net` Zugriffstoken abrufen.

4. Sie werden aufgefordert, einen Benutzernamen und ein Kennwort für Ihr Abonnement anmelden.

5. Anschließend wird ein neuer Azure-Suchdienst Azure-Ressourcen-Manager-Anbieter registriert. Dies ist wiederum die **ExecuteArmRequest** -Methode verwendet, dieses Mal für Ihr Abonnement über den Suchdienst auf Azure erstellen `providers/Microsoft.Search/register`. 

6. Der Rest des Programms wird [Azure Search Management REST-API](http://msdn.microsoft.com/library/dn832684.aspx)verwendet. Beachten Sie, dass die `api-version` für diese API von Azure-Ressourcen-Manager-api-Version ist. Beispielsweise `/listAdminKeys?api-version=2014-07-31-Preview` zeigt die `api-version` von Azure Search Management REST-API.

    Die nächste Abfolge der Operationen Abrufen die Dienstdefinition nur Admin-api-Schlüssel erstellt, regeneriert und ruft Schlüssel, ändert sich das Replikat und die Partition und schließlich löscht den Dienst

    Beim Ändern der Anzahl der Dienste Replikat oder Partition erwartet diese Aktion schlägt fehl, wenn Sie kostenlose Version verwenden. Die standard Edition können zusätzliche Partitionen und Replikate verwenden.

    Löschen des Dienstes ist der letzte Arbeitsgang.

##<a name="next-steps"></a>Nächste Schritte

Nach Abschluss dieser praktischen Einführung, möchten Sie mehr über Service oder Authentifizierung mit Active Directory-Dienst:

- Erfahren Sie mehr über die Integration von einer Clientanwendung in Active Directory. Siehe [Anwendung in Azure Active Directory integrieren](http://msdn.microsoft.com/library/azure/dn151122.aspx).
- Lernen Sie andere Servicemanagement in Azure. Finden Sie unter [Verwalten von Sicherheitsdiensten](http://msdn.microsoft.com/library/azure/dn578292.aspx).

<!--Anchors-->
[Download the sample application]: #Download
[Configure the application]: #config
[Explore the application]: #explore
[Next Steps]: #next-steps

<!--Image references-->
[5]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-Service.PNG
[6]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-App.PNG
[7]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-App-prop.PNG
[8]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-ConfigPermissions.PNG
[9]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-ConfigPage.PNG
[10]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-OAuthEndpoint.PNG

<!--Link references-->
[Manage your search solution in Microsoft Azure]: search-manage.md
[Azure Search development workflow]: search-workflow.md
[Create your first azure search solution]: search-create-first-solution.md
[Create a geospatial search app using Azure Search]: search-create-geospatial.md


 
