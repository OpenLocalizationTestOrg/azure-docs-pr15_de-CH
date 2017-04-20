<properties 
    pageTitle="Implementierung von Disaster Recovery mit Service Backup und Wiederherstellen in Azure API Management | Microsoft Azure" 
    description="Erfahren Sie, wie Sicherung und Wiederherstellung in Azure API Management so wiederherstellen." 
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

# <a name="how-to-implement-disaster-recovery-using-service-backup-and-restore-in-azure-api-management"></a>Implementierung von Disaster Recovery mit Service Backup und Wiederherstellen in Azure API Management

Veröffentlichen und Verwalten von APIs über Azure API Management auswählen, indem nutzen Sie viele Fehlertoleranz und Infrastrukturfunktionen, die Sie sonst entwerfen, implementieren und verwalten. Die Azure-Plattform reduziert einen großen Teil der potenzielle Fehler zu einem Bruchteil der Kosten.

Wiederherstellen von Verfügbarkeit sollten Probleme, Ihre API Management Service gehostet, Region Ihren Dienst in einer anderen Region jederzeit wiederherstellen bereit. Je nach Verfügbarkeitsziele und Wiederherstellungszeit möchten Sicherungsdienst in einem oder mehreren Bereichen reservieren und versuchen, ihre Konfiguration und Inhalt mit aktiven Dienst. Service Backup- und Wiederherstellungsfunktion erforderlichen Baustein für Disaster Recovery-Strategie implementieren.

Dieses Handbuch zeigt Azure Ressourcenmanager authentifizieren und zum Sichern und Wiederherstellen der Dienstinstanzen API Management.

>[AZURE.NOTE] Der Prozess zum Sichern und Wiederherstellen einer API Management Service-Instanz für die Wiederherstellung kann auch für das Replizieren von Dienstinstanzen für Szenarien wie Staging-API Management verwendet werden.
>
>Beachten Sie, dass jede Sicherung nach 7 Tagen abläuft. Beim Wiederherstellen einer Sicherung nach Ablauf die Gültigkeitsdauer 7 Tage fehl die Wiederherstellung mit einem `Cannot restore: backup expired` angezeigt.

## <a name="authenticating-azure-resource-manager-requests"></a>Authentifizierung Azure-Ressourcen-Manager angefordert

>[AZURE.IMPORTANT] Die REST-API für Backup und Wiederherstellung verwendet Azure Ressourcenmanager und einen anderen Authentifizierungsmechanismus als REST-APIs für die Verwaltung der API Verwaltungsorgane. Die Schritte in diesem Abschnitt beschrieben Azure Ressourcenmanager authentifizieren. Weitere Informationen finden Sie unter [Authentifizierung Azure Resource Manager benötigt](http://msdn.microsoft.com/library/azure/dn790557.aspx).

Alle Aufgaben, die Sie auf Ressourcen der Azure-Ressourcen-Manager muss mit Azure Active Directory mit den folgenden Schritten authentifiziert werden.

-   Hinzufügen einer Anwendung zu Azure Active Directory Mieter.
-   Legen Sie Berechtigungen für die Anwendung hinzufügen.
-   Rufen Sie das Token zur Authentifizierung fordert, Azure-Ressourcen-Manager.

Der erste Schritt ist eine Azure Active Directory-Anwendung erstellt. Mithilfe des Abonnements, die die API Management Service-Instanz enthält [Klassischen Azure-Portal](http://manage.windowsazure.com/) anmelden und auf die Registerkarte **Applications** Standard Azure Active Directory navigieren.

>[AZURE.NOTE] Wenden Sie das Standardverzeichnis Azure Active Directory nicht auf Ihr Konto angezeigt wird, Administrator der Azure-Abonnement, um auf Ihr Konto die erforderlichen Berechtigungen erteilen. Informationen zur Suche nach Standardverzeichnis finden Sie unter [Suchen Standardverzeichnis](../virtual-machines/virtual-machines-windows-create-aad-work-id.md#locate-your-default-directory-in-the-azure-portal).

![Active Directory Azure-Anwendung erstellen][api-management-add-aad-application]

Klicken Sie auf **Hinzufügen**, **Mein Unternehmen entwickelten Anwendung hinzufügen**und wählen Sie **Native Client-Anwendung**. Geben Sie einen beschreibenden Namen ein, und klicken Sie auf den Pfeil. Geben Sie z. B. eine Platzhalter-URL `http://resources` für den **URI umleiten**, als es ist ein Pflichtfeld, aber der Wert nicht höher verwendet. Klicken Sie auf das Kontrollkästchen, um die Anwendung zu speichern.

Sobald die Anwendung gespeichert ist, klicken Sie auf **Konfigurieren**, Abschnitt **Berechtigungen für andere Programme** Scrollen Sie und klicken Sie auf **Anwendung hinzufügen**.

![Hinzufügen von Berechtigungen][api-management-aad-permissions-add]

Wählen Sie **Windows Azure** **Service Management-API** , und klicken Sie auf das Kontrollkästchen, um die Anwendung hinzufügen.

![Hinzufügen von Berechtigungen][api-management-aad-permissions]

Klicken Sie neben der neu hinzugefügten **Windows Azure** **Service Management API** -Anwendungdes auf **Berechtigungen delegiert** für **Access Azure (Preview)**das Kontrollkästchen Sie und klicken Sie auf **Speichern**.

![Hinzufügen von Berechtigungen][api-management-aad-delegated-permissions]

Vor dem Aufrufen von APIs, die die Sicherung erstellen und wiederherstellen, ist es erforderlich, ein Token abzurufen. Das folgende Beispiel verwendet [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) Nuget-Paket, das Token abzurufen.

    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;

    namespace GetTokenResourceManagerRequests
    {
        class Program
        {
            static void Main(string[] args)
            {
                var authenticationContext = new AuthenticationContext("https://login.windows.net/{tenant id}");
                var result = authenticationContext.AcquireToken("https://management.azure.com/", {application id}, new Uri({redirect uri});

                if (result == null) {
                    throw new InvalidOperationException("Failed to obtain the JWT token");
                }

                Console.WriteLine(result.AccessToken);

                Console.ReadLine();
            }
        }
    }

Ersetzen Sie `{tentand id}`, `{application id}`, und `{redirect uri}` mithilfe der folgenden Anleitung.

Ersetzen Sie `{tenant id}` mit Mandanten-Id des soeben erstellten Active Directory Azure-Anwendung. Für den Zugriff auf die Id **Anzeigen-Endpunkte**auf.

![Endpunkte][api-management-aad-default-directory]

![Endpunkte][api-management-endpoint]

Ersetzen Sie `{application id}` und `{redirect uri}` Abschnitt **Uris umleiten** von Azure Active Directory Anwendung **Konfigurieren** Registerkarte **Client-Id** und die URL aus. 

![Ressourcen][api-management-aad-resources]

Sobald die Werte angegeben werden, sollte das Codebeispiel ein Token ähnlich dem folgenden Beispiel zurück.

![Token][api-management-arm-token]

Legen Sie anrufen die Sicherung und Wiederherstellung Operationen in den folgenden Abschnitten Autorisierung Anforderungsheader für den REST-Aufruf.

    request.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);

## <a name="step1"> </a>Sichern einer API-Verwaltungsdienst
Zum Sichern einer API Management Service HTTP-Anforderung ausgeben:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/backup?api-version={api-version}`

Wo:

* `subscriptionId`-Die Kennung des Dauerauftrags mit der API-Verwaltungsdienst Sie versuchen Sicherung
* `resourceGroupName`-String in der Form "Api - Standard-{Service-Region}", `service-region` identifiziert Azure Bereich wo API Management service Sie versuchen Sicherung gehostet wird, z. B.`North-Central-US`
* `serviceName`-der Name des API-Verwaltungsdienst Sie machen eine Sicherung zum Zeitpunkt seiner Erstellung angegeben
* `api-version`-Ersetzen`2014-02-14`

Geben Sie im Hauptteil der Anforderung der Zielkontoname Azure-Speicher, Zugriffstaste Blob Containernamen und Sicherungsname:

    '{  
        storageAccount : {storage account name for the backup},  
        accessKey : {access key for the account},  
        containerName : {backup container name},  
        backupName : {backup blob name}  
    }'

Legen Sie den Wert der `Content-Type` Anforderungsheader `application/json`.

Backup ist ein langer Vorgang, der mehrere Minuten in Anspruch nehmen kann.  Wenn die Anforderung erfolgreich war und der Sicherungsvorgang gestartet wurde erhalten Sie eine `202 Accepted` Antwortstatuscode mit einem `Location` Header.  Stellen Sie Anfragen an die URL in "GET" die `Location` Kopf den Status des Vorgangs. Während die Sicherung läuft weiterhin Statuscode '202 akzeptiert' empfangen werden. Antwortcode `200 OK` erfolgreichen Abschluss des Sicherungsvorgangs.

**Hinweis**:

- **Container** gemäß der Anforderung Text **vorhanden sein muss**.
* Während Sicherung läuft Sie wie SKU **sollten keine Servicemanagement** upgrade oder downgrade Domänenname usw.. 
* Wiederherstellen einer **Sicherung garantiert nur für 7 Tage** seit seiner Erstellung. 
* **Daten** zum Erstellen von Analytics meldet der Sicherung **ist nicht enthalten** . Mit [Azure API Management REST API][] können Analyseberichte Sicherheitsgründen regelmäßig abrufen.
* Die Häufigkeit mit der Service-Backups durchführen wirkt der RPO. Zum Minimieren empfehlen wir implementieren regelmäßige Backups sowie on-Demand-Backups nach wichtigen Änderungen an der API Management-Dienst.
* **Ändert** die Dienstkonfiguration (APIs, Richtlinien, Developer Portal Darstellung) beim Sicherungsvorgang ist **möglicherweise nicht in der Sicherung enthalten und daher verloren**.

## <a name="step2"> </a>Wiederherstellung einer API Management
Wiederherstellung einer API Management Service von einer zuvor erstellten Sicherung stellen HTTP-Anforderung:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}`

Wo:

* `subscriptionId`-Die Kennung des Dauerauftrags mit dem Wiederherstellen eine Sicherung in API Management-Dienst
* `resourceGroupName`-String in der Form "Api - Standard-{Service-Region}", `service-region` die Azure-Region, in dem Sie einer Sicherung in Wiederherstellung API Management-Dienst gehostet wird, z. B. identifiziert`North-Central-US`
* `serviceName`-der Name der API Management Service in wiederhergestellt zum Zeitpunkt seiner Erstellung angegeben
* `api-version`-Ersetzen`2014-02-14`

Geben Sie Speicherort der Sicherungsdatei, d. h. Kontoname Azure-Speicher, Zugriffstaste Blob Containernamen und Sicherungsname im Hauptteil der Anforderung:

    '{  
        storageAccount : {storage account name for the backup},  
        accessKey : {access key for the account},  
        containerName : {backup container name},  
        backupName : {backup blob name}  
    }'

Legen Sie den Wert der `Content-Type` Anforderungsheader `application/json`.

Wiederherstellung ist eine umfangreiche Operation, die bis zu 30 Minuten dauern kann abgeschlossen.  Wenn die Anforderung erfolgreich war und der Wiederherstellungsvorgang initiiert wurde erhalten Sie eine `202 Accepted` Antwortstatuscode mit einem `Location` Header.  Stellen Sie Anfragen an die URL in "GET" die `Location` Kopf den Status des Vorgangs. Während die Wiederherstellung ausgeführt wird Sie erhalten weiterhin "202 akzeptiert" Statuscode. Antwortcode `200 OK` erfolgreichen Abschluss des Wiederherstellungsvorgangs.

>[AZURE.IMPORTANT] **Die SKU** des Dienstes in **muss** die SKU des gesicherten Dienstes wiederhergestellt wird wiederhergestellt.
>
>**Ändert** die Dienstkonfiguration (APIs, Richtlinien, Developer Portal Darstellung) beim Wiederherstellungsvorgang wird Fortschritt **kann überschrieben werden**.

## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich folgende Microsoft Blogs für zwei verschiedene exemplarischen Vorgehensweisen Backup-und Recovery-Prozess.

-   [Replizieren von Azure API Konten](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/) 
    -   Danke Gisela für ihren Beitrag zu diesem Artikel.
-   [Azure API Management: Sichern und Wiederherstellen](http://blogs.msdn.com/b/stuartleeks/archive/2015/04/29/azure-api-management-backing-up-and-restoring-configuration.aspx)
    -   Von Stuart detaillierte Ansatz entspricht nicht gültig, aber es ist sehr interessant.

[Backup an API Management service]: #step1
[Restore an API Management service]: #step2


[Azure API Management REST-API]: http://msdn.microsoft.com/library/azure/dn781421.aspx

[api-management-add-aad-application]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-add-aad-application.png

[api-management-aad-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions.png
[api-management-aad-permissions-add]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions-add.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-delegated-permissions.png
[api-management-aad-default-directory]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-default-directory.png
[api-management-aad-resources]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-resources.png
[api-management-arm-token]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-arm-token.png
[api-management-endpoint]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-endpoint.png
 
