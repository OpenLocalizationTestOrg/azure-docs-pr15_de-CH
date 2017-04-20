<properties
    pageTitle="Azure Active Directory-in Bericht-API-Referenz | Microsoft Azure"
    description="Referenz für Azure Active Directory Anmeldeaktivität Bericht API"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/25/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-sign-in-activity-report-api-reference"></a>Azure Active Directory-in Bericht-API-Referenz


Dieses Thema ist Teil einer Sammlung von Themen zu Azure Active Directory reporting API.  
Azure AD reporting bietet eine API, die-im Berichtsdaten mit Code oder verwandte Tools zugreifen kann.
Dieses Thema soll bieten Informationen über die **Aktivität Bericht API anmelden**.

Sieh:

- Weitere grundlegende Informationen [-in Aktivitäten](active-directory-reporting-azure-portal.md#sign-in-activities)
- [Erste Schritte mit der Azure Active Directory Reporting-API](active-directory-reporting-api-getting-started.md) Weitere Informationen die reporting-API.

Fragen, Fragen oder Feedback wenden Sie [AAD Reporting helfen](mailto:aadreportinghelp@microsoft.com).



## <a name="who-can-access-the-api-data"></a>Die API-Daten zugreifen können?

- Benutzer in der Sicherheitsadministrator oder Sicherheit Reader

- Globale Administratoren

- Jede Anwendung, die Autorisierung auf die API zugreifen (app Autorisierung kann Setup nur für globale Administratoren Berechtigung)



## <a name="prerequisites"></a>Erforderliche Komponenten

Um diesen Bericht über die reporting-API zugreifen, benötigen Sie Folgendes:

- Ein [Azure Active Directory Premium P1 und P2-edition](active-directory-editions.md)

- [Komponenten auf Azure AD reporting API](active-directory-reporting-api-prerequisites.md)abgeschlossen. 


##<a name="accessing-the-api"></a>Zugriff auf die API

Sie können diese API über [Graph-Explorer](https://graphexplorer2.cloudapp.net) oder programmgesteuert zugreifen mit PowerShell. PowerShell in AAD Graph REST verwendeten OData-Filtersyntax korrekt interpretieren, Sie müssen verwenden den umgekehrten Apostroph (aka: Graviszeichen) Zeichen das Zeichen $ "Escapezeichen". Das Zeichen umgekehrten Apostroph dient als [PowerShell Escapezeichen](https://technet.microsoft.com/library/hh847755.aspx)PowerShell wörtliche Interpretation $-Zeichen und nicht als Variablenname PowerShell verwechseln zulassen (z.B.: $filter).

Der Schwerpunkt dieses Themas ist im Graph-Explorer. Finden Sie ein Beispiel PowerShell das [PowerShell-Skript](active-directory-reporting-api-sign-in-activity-samples.md#powershell-script).


## <a name="api-endpoint"></a>API-Endpunkt

Sie können mithilfe der folgenden Basis-URI API zugreifen:  
    
    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta  



Diese API ist aufgrund der Datenmenge begrenzt Millionen Datensätze zurückgegeben. 

Dieser Aufruf gibt die Daten in Batches. Jede Gruppe hat maximal 1000 Datensätzen.  
Um den nächsten Stapel von Datensätzen abzurufen, verwenden Sie den Hyperlink Weiter. Erhalten Sie die [Skiptoken](https://msdn.microsoft.com/library/dd942121.aspx) -Informationen aus dem ersten Satz der zurückgegebenen Datensätze. Skip-Token wird am Ende des Resultsets festgelegt.  

    https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&%24skiptoken=-1339686058


## <a name="supported-filters"></a>Unterstützte Filter

Die Anzahl der durch eine API zurückgegebenen Datensätze einschränken in Form eines Filters aufrufen.  
-In API bezogene Daten folgende Filter unterstützt:

- **$top =\<Anzahl der zurückzugebenden Datensätze\> ** - die Anzahl der zurückgegebenen Datensätze beschränken. Dies ist ein aufwändiger Vorgang. Verwenden Sie diesen Filter nicht, möchten Sie Tausende Objekte zurückgeben.  
- **$filter =\<Filter-Anweisung\> ** -, aufgrund unterstützten Filterfelder Datensätze geben Sie interessieren



## <a name="supported-filter-fields-and-operators"></a>Unterstützte Felder und Operatoren

Um Datensätze geben, die Sie interessieren, können Sie Filter-Anweisung erstellen, die eine oder eine Kombination der folgenden Filterfelder enthalten kann:

- [SigninDateTime](#signindatetime) - definiert, ein Datum oder einen Datumsbereich

- [UserId](#userid) - definiert eine bestimmte Benutzer-ID des Benutzers basierend

- [UserPrincipalName](#userprincipalname) - definiert eine bestimmte benutzerbasierte des Benutzers für Benutzerprinzipalname (UPN)

- [AppId](#appid) - definiert eine bestimmte Anwendung basiert die app-ID

- [AppDisplayName](#appdisplayname) - definiert eine bestimmte Anwendung basiert die app Anzeigenamen

- [LoginStatus](#loginStatus) - definiert den Status der Benutzernamen (Erfolg / Fehler)


> [AZURE.NOTE] Graph-Explorer verwenden, wird Sie müssen die Groß-und Kleinschreibung für jeden Buchstaben die Filterfelder.


Um den Umfang der zurückgegebenen Daten einzuschränken, können Sie Kombinationen von unterstützte Filter und Felder erstellen. Die folgende Anweisung gibt z. B. die Top 10 Datensätze 1. Juli 2016 bis 6. Juli 2016:

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta&$top=10&$filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T00:00:00Z


----------

### <a name="signindatetime"></a>signinDateTime

**Unterstützte Operatoren**: Eq, ge, le, Gt, Lt

**Beispiel**:

Verwenden ein bestimmtes Datum

    $filter=signinDateTime+eq+2016-04-25T23:59:00Z  



Verwenden einen    

    $filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T17:05:21Z


**Notizen**:

Der Datetime-Parameter sollte im UTC-Format sein. 


----------

### <a name="userid"></a>Benutzer-ID

**Unterstützte Operatoren**: Eq

**Beispiel**:

    $filter=userId+eq+’00000000-0000-0000-0000-000000000000’

**Notizen**:

Der Wert von Benutzer_id ist ein String-Wert



----------

### <a name="userprincipalname"></a>userPrincipalName

**Unterstützte Operatoren**: Eq

**Beispiel**:

    $filter=userPrincipalName+eq+'audrey.oliver@wingtiptoysonline.com' 


**Notizen**:

Der Wert der UserPrincipalName ist ein String-Wert

----------

### <a name="appid"></a>appId

**Unterstützte Operatoren**: Eq

**Beispiel**:

    $filter=appId+eq+’00000000-0000-0000-0000-000000000000’



**Notizen**:

Der Wert des AppId ist ein String-Wert

----------


### <a name="appdisplayname"></a>appDisplayName

**Unterstützte Operatoren**: Eq

**Beispiel**:

    $filter=appDisplayName+eq+'Azure+Portal' 


**Notizen**:

Der Wert AppDisplayName ist ein String-Wert

----------

### <a name="loginstatus"></a>loginStatus

**Unterstützte Operatoren**: Eq

**Beispiel**:

    $filter=loginStatus+eq+'1'  


**Notizen**:

Es gibt zwei Optionen für die LoginStatus: 0 - erfolgreich 1 - Fehler

----------



## <a name="next-steps"></a>Nächste Schritte

- Möchten Sie Beispiele für gefilterte Aktivitäten? [Azure Active Directory Anmeldeaktivität Beispielberichte API](active-directory-reporting-api-sign-in-activity-samples.md)anzeigen

- Möchten Sie mehr über Azure AD API reporting? Finden Sie unter [Erste Schritte mit der Azure Active Directory Reporting-API](active-directory-reporting-api-getting-started.md).