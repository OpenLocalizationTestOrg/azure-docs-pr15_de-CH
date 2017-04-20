<properties
   pageTitle="Schnellstart für Azure AD Graph API | Microsoft Aure"
   description="Azure Active Directory Graph API ermöglicht programmgesteuerten Zugriff auf Azure AD über OData REST API-Endpunkte. Clientanwendungen können die Graph-API ausführen erstellen, lesen, aktualisieren und löschen böte Objekte und Daten."
   services="active-directory"
   documentationCenter="n/a"
   authors="PatAltimore"
   manager="mbaldwin"
   editor=""
   tags=""/>


   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="09/16/2016"
      ms.author="patricka"/>

# <a name="quickstart-for-the-azure-ad-graph-api"></a>Schnellstart für Azure AD Graph API

Azure Active Directory (AD) Graph-API ermöglicht programmgesteuerten Zugriff auf Azure AD über OData REST API-Endpunkte. Clientanwendungen können die Graph-API ausführen erstellen, lesen, aktualisieren und löschen böte Objekte und Daten. Beispielsweise können Sie die Graph-API auf ein neues Benutzerkonto erstellen, anzeigen oder aktualisieren-Eigenschaften des Benutzers, Benutzerkennwort ändern, rollenbasierte Mitgliedschaft, deaktivieren oder Löschen des Benutzers. Weitere Informationen zu Graph-API-Funktionen und Anwendungsszenarios finden Sie in [Azure AD Graph-API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) und [Azure AD Graph-API-Komponenten](https://msdn.microsoft.com/library/hh974476.aspx). 

> [AZURE.IMPORTANT] Azure AD Graph-API-Funktion ist auch über [Microsoft Graph](https://graph.microsoft.io/)eine einheitliche API, die APIs von anderen Microsoft-Diensten wie Outlook, OneDrive, OneNote, Planer und Office Graph Zugriff durch einen einzelnen Endpunkt und mit einem einzigen Token enthält.

## <a name="how-to-construct-a-graph-api-url"></a>Wie Sie einer Graph-API-URL

Graph-API Daten und Objekte (also Ressourcen oder Entitäten) gegen die CRUD-Operationen durchzuführen können URLs basierend auf Protokoll öffnen Daten (OData) Sie. Graph-API verwendeten URLs bestehen aus vier Teilen: service Root, Pächter Bezeichner Ressourcenpfad und Abfrageoptionen Zeichenfolge: `https://graph.windows.net/{tenant-identifier}/{resource-path}?[query-parameters]`. Beispiel: die folgende URL: `https://graph.windows.net/contoso.com/groups?api-version=1.6`.

- **Dienst**: In Azure AD Graph API-Dienst Stamm ist immer https://graph.windows.net.
- **Mandanten-ID**: Dies ist eine überprüft (registrierten) Domänennamen im Beispiel oben contoso.com. Es kann auch ein Mieter Objekt-ID oder "Myorganiztion" oder "me" alias sein. Weitere Informationen finden Sie unter [Adressieren und Vorgänge in der Graph-API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-operations-overview)).
- **Ressourcenpfad**: der Teil einer URL identifiziert die Ressource um Interaktion werden (Benutzer, Gruppen, einen bestimmten Benutzer oder einer bestimmten Gruppe usw.) Im obigen Beispiel ist es der obersten Ebene "Gruppen" Ressource an. Sie können auch eine bestimmte Entität adressieren, beispielsweise "Benutzer / {ObjectId}" oder "Benutzer/UserPrincipalName".
- **Parameter**:? trennt den Pfad Ressourcenabschnitt Abfrage Parameter im Abschnitt. Der Abfrageparameter "api-Version" wird für alle Anfragen in der Graph-API erforderlich. Graph-API unterstützt auch die OData-Abfrage Folgendes: **$filter**, **$orderby** **$Erweitern**, **$top**und **$format**. Die folgenden Optionen werden nicht unterstützt: **$count** **$inlinecount**und **$skip**. Weitere Informationen finden Sie unter [Abfragen unterstützt, Filtern und Paging Optionen in Azure AD Graph-API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options).

## <a name="graph-api-versions"></a>Graph-API-Versionen

Die Version für ein Graph-API-Anforderung in der Abfrageparameter "api-Version" angegeben. Für Version 1.5 oder höher verwenden Sie einen numerischen Wert. API-Version = 1,6. Bei früheren Versionen verwenden Sie eine Datumszeichenfolge Format YYYY-MM-TT entspricht; z. B. api-Version = 2013-11-08. Vorschau-Features verwenden Sie die Zeichenfolge "Beta"; z. B. api-Version = Beta. Weitere Informationen zu Unterschieden zwischen Versionen Graph-API finden Sie unter [Azure AD Graph-API Versioning](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-versioning).

## <a name="graph-api-metadata"></a>Graph-API-Metadaten

Metadatendatei Graph-API zurückgegeben, Tenant-Bezeichner in der URL beispielsweise "$metadata" Segment hinzufügen, gibt die folgende URL Metadaten für das Demounternehmen Graph-Explorer verwendet: `https://graph.windows.net/GraphDir1.OnMicrosoft.com/$metadata?api-version=1.6`. Sie können diese URL in die Adressleiste eines Webbrowsers, der Metadaten eingeben. Die CSDL-Metadaten zurückgegeben beschrieben Entitäten und komplexen Typen, deren Eigenschaften und Funktionen und Aktionen von der Version der angeforderten Graph-API verfügbar gemacht. API-Version Parameter ausgelassen werden Metadaten für die aktuelle Version zurück.

## <a name="common-queries"></a>Allgemeine Abfragen

[Azure AD Graph-API allgemeine Abfragen](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options#CommonQueries) sind allgemeine Abfragen mit Azure AD Graph Abfragen auf der obersten Ebene im Verzeichnis zugreifen sowie Abfragen Operationen im Verzeichnis aufgeführt.

Beispielsweise `https://graph.windows.net/contoso.com/tenantDetails?api-version=1.6` gibt Unternehmensinformationen für Verzeichnis contoso.com.

Oder `https://graph.windows.net/contoso.com/users?api-version=1.6` Listet alle Benutzerobjekte in der Directory contoso.com.

## <a name="using-the-graph-explorer"></a>Im Graph-Explorer

Graph-Explorer für Azure AD Graph-API können Sie Verzeichnisdaten Abfragen während der Erstellung der Anwendung.

> [AZURE.IMPORTANT] Graph-Explorer unterstützt keine schreiben oder Löschen von Daten aus einem Verzeichnis. Sie können nur Lesevorgänge Verzeichnis Azure AD Graph-Explorer ausführen.

Im folgenden ist die Ausgabe Sie sieht würden Sie Graph-Explorer navigieren, wählen Demounternehmen verwenden, und geben `https://graph.windows.net/GraphDir1.OnMicrosoft.com/users?api-version=1.6` zur Anzeige aller Benutzer im Demo-Verzeichnis:

![Azure AD Graph API-explorer](./media/active-directory-graph-api-quickstart/graph_explorer.png)

**Laden der Graph-Explorer**: um das Tool zu laden, navigieren Sie zu [https://graphexplorer.cloudapp.net/](https://graphexplorer.cloudapp.net/). Klicken Sie auf **Demounternehmen verwenden** , um Daten aus Beispiel Mieter Graph-Explorer ausführen. Sie benötigen keine Anmeldeinformationen mit der Demo. Alternativ klicken Sie auf **Anmelden** und Anmeldung Ihre Anmeldeinformationen Azure AD Graph-Explorer für Ihren Mandanten ausführen. Wenn eigene Mieter Graph-Explorer ausführen, müssen Sie oder Ihr Administrator bei der Anmeldung zu bekommen. Wenn Sie ein Office 365-Abonnement haben, haben Sie automatisch einen Azure AD-Mandanten. Bei Office 365 anmelden verwendeten Anmeldeinformationen sind tatsächlich Azure AD-Konten und können diese Anmeldeinformationen mit Graph-Explorer.

**Führen Sie eine Abfrage**: zum Ausführen einer Abfrage geben Sie Ihre Abfrage im Textfeld Anforderung **Klicken Sie auf** oder klicken Sie auf den Schlüssel **eingeben** . Die Ergebnisse werden im Feld Antwort angezeigt. Beispielsweise `https://graph.windows.net/graphdir1.onmicrosoft.com /groups?api-version=1.6` Listet alle Gruppenobjekte im Demo-Verzeichnis.

Beachten Sie die folgenden Funktionen und Grenzen der Graph-Explorer:
- AutoVervollständigen-Funktion auf Ressource. Dazu auf **Demounternehmen verwenden** , und klicken Sie dann auf das Textfeld Anforderung (wo die Unternehmens-URL angezeigt wird). Sie können einen Ressourcensatz aus der Dropdown-Liste auswählen.

- Unterstützt "me" und "MeineOrganisation" Aliase. Beispielsweise können Sie `https://graph.windows.net/me?api-version=1.6` das Benutzerobjekt des angemeldeten Benutzers zurückgegeben oder `https://graph.windows.net/myorganization/users?api-version=1.6` an alle Benutzer im aktuellen Verzeichnis zurück. Beachten Sie, dass "me" alias einen Fehler des Unternehmens Demo wird nämlich keine angemeldeten Benutzers.

- Ein Antwort-Header-Abschnitt. Dies kann zu Problemen, die beim Ausführen von Abfragen verwendet werden.

- Ein JSON-Viewer für die Antwort erweitern und reduzieren.

- Keine Unterstützung für ein Vorschaubild angezeigt.

## <a name="using-fiddler-to-write-to-the-directory"></a>Mithilfe von Fiddler in das Verzeichnis schreiben

Für die Zwecke dieses Schnellstart-Handbuch können Fiddler Web Debugger Sie um "Schreiben" Operationen gegen Sie üben Azure AD-Verzeichnis. Weitere Informationen und Fiddler installieren finden Sie unter [http://www.telerik.com/fiddler](http://www.telerik.com/fiddler).

Im folgenden Beispiel verwenden Sie Fiddler Web Debugger zum Erstellen einer neuen Sicherheitsgruppe 'MyTestGroup' in Azure AD-Verzeichnis.

**Abrufen eines Zugriffstokens**: Azure AD Graph Zugriff auf Clients müssen erfolgreich Azure AD zunächst authentifiziert. Weitere Informationen finden Sie unter [Authentifizierungsszenarien Azure AD](active-directory-authentication-scenarios.md).

**Erstellen und Ausführen einer Abfrage**: Gehen Sie folgendermaßen vor.

1. Öffnen Sie Fiddler Web Debugger, und wechseln Sie zur Registerkarte **Composer** .
2. Da Sie eine neue Sicherheitsgruppe erstellen möchten, wählen Sie **Post** als die HTTP-Methode aus dem Pulldown Menü. Weitere Informationen über Funktionen und Berechtigungen für eine Gruppe finden Sie unter [Gruppe](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#GroupEntity) in [Azure AD Graph REST-API-Referenz](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).
3. Geben Sie im Feld **an**folgenden Request-URL: `https://graph.windows.net/mytenantdomain/groups?api-version=1.6`.

    > [AZURE.NOTE] Ersetzen Sie Mytenantdomain mit dem Domänennamen des Azure AD-Verzeichnisses.

4. Geben Sie im Feld unterhalb der Post-Pulldown Folgendes ein:

    ```
Host: graph.windows.net
Authorization: your access token
Content-Type: application/json
```

    > [AZURE.NOTE] Ersatz der &lt;Ihr Zugriffstoken&gt; mit dem Zugriffstoken für Azure AD-Verzeichnis.

5. Geben Sie im Feld **Anforderungsinhalt** Folgendes ein:

    ```
        {
            "displayName":"MyTestGroup",
            "mailNickname":"MyTestGroup",
            "mailEnabled":"false",
            "securityEnabled": true
        }
```

    Weitere Informationen zum Erstellen von Gruppen finden Sie unter [Erstellen von Gruppen](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#CreateGroup).

Weitere Informationen über Azure AD-Entitäten und Typen von Graph verfügbar gemacht werden und Informationen über die Vorgänge, die mit Graph durchgeführt werden können, finden Sie in [Azure AD Graph REST-API-Referenz](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Azure AD Graph-API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)
- Erfahren Sie mehr über [Azure AD Graph-API Berechtigungsbereiche](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes)
