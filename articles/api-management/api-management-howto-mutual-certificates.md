<properties 
    pageTitle="Back-End-Services Client mit Zertifikatauthentifizierung in Azure API Management schützen" 
    description="Informationen Sie zum Back-End-Dienste mit Clientzertifikatsauthentifizierung in Azure API Management schützen." 
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

# <a name="how-to-secure-back-end-services-using-client-certificate-authentication-in-azure-api-management"></a>Back-End-Services Client mit Zertifikatauthentifizierung in Azure API Management schützen

API-Management bietet die Möglichkeit, Zugang zu den Back-End-Dienst einer API mit Clientzertifikaten. Dieses Handbuch Zeigt Zertifikate im Portal Publisher API verwalten und eine API Zertifikat verwenden den Back-End-Dienst konfigurieren.

Informationen zum Verwalten von Zertifikaten mithilfe der API-REST-API finden Sie unter [Azure API REST API Zeugnisse Entität][].

## <a name="prerequisites"> </a>Komponenten

Dieses Handbuch zeigt die Dienstinstanz API Management Clientzertifikatsauthentifizierung verwenden den Back-End-Dienst einer API zu konfigurieren. Vor dem Durchführen der Schritte in diesem Thema sollte Ihre Back-End-Dienst ([Konfigurieren Zertifikatauthentifizierung in Azure WebSites finden Sie in diesem Artikel][]) Clientzertifikatauthentifizierung konfiguriert haben und haben Zugriff auf das Zertifikat und das Kennwort für das Zertifikat zum Hochladen in Publisher API Verwaltungsportal.

## <a name="step1"> </a>Ein Client-Zertifikat hochladen

Klicken Sie zunächst auf **Verwalten** in Azure-Verwaltungsportal für Ihre API-Verwaltungsdienst. Dadurch gelangen Sie zu Publisher API Verwaltungsportal.

![API-Publisher-portal][api-management-management-console]

>Wenn Sie eine Dienstinstanz API Management noch nicht erstellt haben, finden Sie im Lernprogramm [Erste Schritte mit Azure API Management][] [Erstellen API Management Service][] .

Klicken Sie auf **Sicherheit** aus der **API-** Menü auf der linken Seite und auf **Clientzertifikate**.

![Client-Zertifikate][api-management-security-client-certificates]

Um ein neues Zertifikat hochzuladen, klicken Sie auf **Zertifikat hochladen**.

![Zertifikat hochladen][api-management-upload-certificate]

Suchen Sie das Zertifikat, und geben Sie das Kennwort für das Zertifikat.

>Das Zertifikat muss im **PFX** -Format. Selbstsignierte Zertifikate sind zulässig.

![Zertifikat hochladen][api-management-upload-certificate-form]

Klicken Sie auf das Zertifikat hochladen **hochgeladen** .

>Das Zertifikatkennwort ist zu diesem Zeitpunkt überprüft. Wenn sie falsch ist wird eine Fehlermeldung angezeigt.

![Zertifikat hochgeladen][api-management-certificate-uploaded]

Sobald das Zertifikat geladen wird, wird sie auf der Registerkarte **Client-Zertifikate** . Haben Sie mehrere Zertifikate stellen Sie eine Notiz Betreff oder die letzten vier Zeichen des Fingerabdrucks, der das Zertifikat auswählen, wenn API zur Verwendung von Zertifikaten, konfigurieren, wie im Abschnitt [konfigurieren eine API, um ein Clientzertifikat für Gateway-Authentifizierung verwenden][] .

>Deaktivieren Überprüfung der Zertifikatkette Verwendung, z. B. ein selbstsigniertes Zertifikat, befolgen Sie die Schritte in diesem FAQ- [Artikel](api-management-faq.md#can-i-use-a-self-signed-ssl-certificate-for-a-back-end).

## <a name="step1a"> </a>Ein Client-Zertifikat löschen

Um ein Zertifikat zu löschen, klicken Sie neben das gewünschte Zertifikat **Löschen** .

![Zertifikat löschen][api-management-certificate-delete]

Klicken Sie auf **Ja, löschen** bestätigen.

![Löschen bestätigen][api-management-confirm-delete]

Wenn das Zertifikat von einer API verwendet wird, wird ein Warnbildschirm angezeigt. Um das Zertifikat zu löschen müssen Sie zuerst das Zertifikat APIs aufheben, die konfiguriert werden.

![Löschen bestätigen][api-management-confirm-delete-policy]

## <a name="step2"> </a>Konfigurieren eine API um ein Clientzertifikat für Gateway-Authentifizierung verwenden

Klicken Sie auf **APIs** der **API-** Menü auf der linken Seite klicken Sie auf den Namen der gewünschten API und klicken Sie auf die Registerkarte **Sicherheit** .

![API-Sicherheit][api-management-api-security]

Dropdownliste **mit** **Clientzertifikaten** auswählen.

![Client-Zertifikate][api-management-mutual-certificates]

Wählen Sie das gewünschte Zertifikat aus der Dropdownliste **Clientzertifikat** . Es sind mehrere Zertifikate können Sie den Betreff oder die letzten vier Zeichen des Fingerabdrucks wie im vorherigen Abschnitt erwähnt, bestimmt das richtige Zertifikat anzeigen.

![Zertifikat auswählen][api-management-select-certificate]

Klicken Sie auf **Speichern** , um die Konfiguration der API speichern.

>Diese Änderung wird sofort wirksam, und Aufrufe von Vorgängen, API verwenden das Zertifikat auf dem Back-End-Server authentifiziert.

![API-speichern][api-management-save-api]

>Wenn ein Zertifikat für die Authentifizierung für den Back-End-Dienst eine API Gateway angegeben ist, wird der in der Richtlinie für diese API und können im Systemrichtlinien-Editor angezeigt.

![Zertifikatsrichtlinie][api-management-certificate-policy]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über andere Ihren Back-End-Dienst, wie HTTP-basic oder gemeinsam genutzten geheimen Authentifizierung finden Sie im folgende Video.

> [AZURE.VIDEO last-mile-security]

[api-management-management-console]: ./media/api-management-howto-mutual-certificates/api-management-management-console.png
[api-management-security-client-certificates]: ./media/api-management-howto-mutual-certificates/api-management-security-client-certificates.png
[api-management-upload-certificate]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate.png
[api-management-upload-certificate-form]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate-form.png
[api-management-certificate-uploaded]: ./media/api-management-howto-mutual-certificates/api-management-certificate-uploaded.png
[api-management-api-security]: ./media/api-management-howto-mutual-certificates/api-management-api-security.png
[api-management-mutual-certificates]: ./media/api-management-howto-mutual-certificates/api-management-mutual-certificates.png
[api-management-select-certificate]: ./media/api-management-howto-mutual-certificates/api-management-select-certificate.png
[api-management-save-api]: ./media/api-management-howto-mutual-certificates/api-management-save-api.png
[api-management-certificate-policy]: ./media/api-management-howto-mutual-certificates/api-management-certificate-policy.png
[api-management-certificate-delete]: ./media/api-management-howto-mutual-certificates/api-management-certificate-delete.png
[api-management-confirm-delete]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete.png
[api-management-confirm-delete-policy]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete-policy.png



[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Erste Schritte mit Azure API Management]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Erstellen Sie eine Instanz der API Management service]: api-management-get-started.md#create-service-instance

[Azure-API Zeugnisse REST API-Entität]: http://msdn.microsoft.com/library/azure/dn783483.aspx
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Konfigurieren Zertifikatauthentifizierung in Azure WebSites finden Sie in diesem Artikel]: https://azure.microsoft.com/en-us/documentation/articles/app-service-web-configure-tls-mutual-auth/

[Prerequisites]: #prerequisites
[Upload a client certificate]: #step1
[Delete a client certificate]: #step1a
[Konfigurieren einer API um ein Clientzertifikat für Gateway-Authentifizierung verwenden]: #step2
[Test the configuration by calling an operation in the Developer Portal]: #step3
[Next steps]: #next-steps


 
