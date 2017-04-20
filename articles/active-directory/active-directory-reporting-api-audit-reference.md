<properties
    pageTitle="Azure Active Directory Audit-API-Referenz | Microsoft Azure"
    description="Erste Schritte mit Azure Active Directory Audit-API"
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
    ms.date="10/24/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-audit-api-reference"></a>Azure Active Directory Audit-API-Referenz

Dieses Thema ist Teil einer Sammlung von Themen zu Azure Active Directory reporting API.  
Azure AD reporting bietet eine API, die Audit-Daten mithilfe von Code oder verwandte Tools zugreifen kann.
Dieses Thema soll bieten Informationen über die **API überwachen**.

Sieh:

- [Überwachungsprotokolle](active-directory-reporting-azure-portal.md#audit-logs) konzeptionelle Informationen
- [Erste Schritte mit der Azure Active Directory Reporting-API](active-directory-reporting-api-getting-started.md) Weitere Informationen die reporting-API.

Fragen, Fragen oder Feedback wenden Sie [AAD Reporting helfen](mailto:aadreportinghelp@microsoft.com).


## <a name="who-can-access-the-data"></a>Wer kann auf die Daten zugreifen?

- Benutzer in der Sicherheitsadministrator oder Sicherheit Reader

- Globale Administratoren

- Jede Anwendung, die Autorisierung auf die API zugreifen (app Autorisierung kann Setup nur für globale Administratoren Berechtigung)



## <a name="prerequisites"></a>Erforderliche Komponenten

Um diesen Bericht über die Reporting-API zugreifen, benötigen Sie Folgendes:

- Ein [Azure Active Directory oder bessere edition](active-directory-editions.md)

- [Komponenten auf Azure AD reporting API](active-directory-reporting-api-prerequisites.md)abgeschlossen. 
 

##<a name="accessing-the-api"></a>Zugriff auf die API

Sie können diese API über [Graph-Explorer](https://graphexplorer2.cloudapp.net) oder programmgesteuert zugreifen mit PowerShell. PowerShell in AAD Graph REST verwendeten OData-Filtersyntax korrekt interpretieren, Sie müssen verwenden den umgekehrten Apostroph (aka: Graviszeichen) Zeichen das Zeichen $ "Escapezeichen". Das Zeichen umgekehrten Apostroph dient als [PowerShell Escapezeichen](https://technet.microsoft.com/library/hh847755.aspx)PowerShell wörtliche Interpretation $-Zeichen und nicht als Variablenname PowerShell verwechseln zulassen (z.B.: $filter).

Der Schwerpunkt dieses Themas ist im Graph-Explorer. Finden Sie ein Beispiel PowerShell das [PowerShell-Skript](active-directory-reporting-api-audit-samples.md#powershell-script).

 
 

## <a name="api-endpoint"></a>API-Endpunkt


Sie können mit dem folgenden URI API zugreifen:  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta

Es gibt keine Beschränkung für die Anzahl der Datensätze von Azure AD Audit-API (mit OData Paginierung) zurückgegeben.
Aufbewahrungsfristen auf Berichtsdaten finden Sie [Aufbewahrungsrichtlinien Reporting](active-directory-reporting-retention.md).

Dieser Aufruf gibt die Daten in Batches. Jede Gruppe hat maximal 1000 Datensätzen.  
Um den nächsten Stapel von Datensätzen abzurufen, verwenden Sie den Hyperlink Weiter. Erhalten Sie die Skiptoken-Informationen aus dem ersten Satz der zurückgegebenen Datensätze. Skip-Token wird am Ende des Resultsets festgelegt.  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta&%24skiptoken=-1339686058




## <a name="supported-filters"></a>Unterstützte Filter

Die Anzahl der durch eine API zurückgegebenen Datensätze einschränken in Form eines Filters aufrufen.  
-In API bezogene Daten folgende Filter unterstützt:

- **$top =\<Anzahl der zurückzugebenden Datensätze\> ** - die Anzahl der zurückgegebenen Datensätze beschränken. Dies ist ein aufwändiger Vorgang. Verwenden Sie diesen Filter nicht, möchten Sie Tausende Objekte zurückgeben.     
- **$filter =\<Filter-Anweisung\> ** -, aufgrund unterstützten Filterfelder Datensätze geben Sie interessieren



## <a name="supported-filter-fields-and-operators"></a>Unterstützte Felder und Operatoren

Um Datensätze geben, die Sie interessieren, können Sie Filter-Anweisung erstellen, die eine oder eine Kombination der folgenden Filterfelder enthalten kann:

- ein Datum oder einen Datumsbereich definiert [ActivityDate](#activitydate) -
- [ActivityType](#activitytype) - definiert den Typ der Aktivität
- [Aktivität](#activity) - definiert Aktivität als Zeichenfolge  
- [Schauspieler-Name](#actorname) - definiert den Akteur in Form eines Namens
- [Schauspieler-Objectid](#actorobjectid) - definiert den Akteur in Form der Akteur-ID   
- [Schauspieler-Upn](#actorupn) - definiert den Akteur in Form der Akteur Benutzerprinzipalnamen (UPN) 
- [Ziel-Namen](#targetname) - definiert das Ziel in Form eines Namens
- [Ziel-Objectid](#targetobjectid) - definiert das Ziel in Form von der Ziel-ID  
- [Ziel-Upn](#targetupn) - definiert den Akteur in Form der Akteur Benutzerprinzipalnamen (UPN)   




----------

### <a name="activitydate"></a>activityDate

**Unterstützte Operatoren**: Eq, ge, le, Gt, Lt

**Beispiel**:

    $filter=tdomain + 'activities/audit?api-version=beta&`$filter=eventTime gt ' + $7daysago    

**Notizen**:

DateTime sollte im UTC-Format sein.

----------

### <a name="activitytype"></a>"activityType"

**Unterstützte Operatoren**: Eq

**Beispiel**:

    $filter=activityType eq 'User'  

**Notizen**:

Groß-/Kleinschreibung

----------

### <a name="activity"></a>Aktivität

**Unterstützte Operatoren**: StartsWith Eq enthält

**Beispiel**:

    $filter=activity eq 'Add application' or contains(activity, 'Application') or startsWith(activity, 'Add')   

**Notizen**:

Groß-/Kleinschreibung

----------

### <a name="actorname"></a>/ Name des Darstellers

**Unterstützte Operatoren**: StartsWith Eq enthält

**Beispiel**:

    $filter=actor/name eq 'test' or contains(actor/name, 'test') or startswith(actor/name, 'test')  

**Notizen**:

Kleinschreibung

    

----------
### <a name="actorobjectid"></a>Schauspieler-objectId

**Unterstützte Operatoren**: Eq

**Beispiel**:

    $filter=actor/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba'    

----------
### <a name="targetname"></a>Ziel-name

**Unterstützte Operatoren**: StartsWith Eq enthält

**Beispiel**:

    $filter=targets/any(t: t/name eq 'some name')   

**Notizen**:

Kleinschreibung

----------

### <a name="targetupn"></a>Ziel-upn

**Unterstützte Operatoren**: Eq "StartsWith"

**Beispiel**:

    $filter=targets/any(t: startswith(t/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity/userPrincipalName,'abc')) 

**Notizen**:

- Kleinschreibung
- Sie müssen den vollständigen Namespace hinzufügen beim Abfragen der Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity

----------

### <a name="targetobjectid"></a>Ziel-objectId

**Unterstützte Operatoren**: Eq

**Beispiel**:

    $filter=targets/any(t: t/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba')    

----------

### <a name="actorupn"></a>Schauspieler-upn

**Unterstützte Operatoren**: Eq "StartsWith"

**Beispiel**:

    $filter=startswith(actor/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity/userPrincipalName,'abc')  

**Notizen**:

- Kleinschreibung 
- Sie müssen den vollständigen Namespace hinzufügen beim Abfragen der Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity

----------




## <a name="next-steps"></a>Nächste Schritte

- Möchten Sie Beispiele für gefilterte Systemaktivitäten? Checken Sie [Azure Active Directory überwachen API Samples](active-directory-reporting-api-audit-samples.md).

- Möchten Sie mehr über Azure AD API reporting? Finden Sie unter [Erste Schritte mit der Azure Active Directory Reporting-API](active-directory-reporting-api-getting-started.md).