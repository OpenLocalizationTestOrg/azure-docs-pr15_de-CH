<properties
    pageTitle="Azure AD um den Dienst Auth mit OAuth2.0 | Microsoft Azure"
    description="Dieser Artikel beschreibt, wie Sie HTTP-Nachrichten mit den OAuth2.0 Client Anmeldeinformationen gewähren, Dienst Authentifizierung implementieren."
    services="active-directory"
    documentationCenter=".net"
    authors="priyamohanram"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="priyamo"/>

# <a name="service-to-service-calls-using-client-credentials"></a>Service Austauschakkus mit Clientanmeldeinformationen

OAuth 2.0 Client Anmeldeinformationen gewähren fließen lässt einen Webdienst (eine *vertrauliche Client*) mit eigenen Anmeldeinformationen authentifizieren beim Aufruf eines anderen Web Service statt einen Benutzer imitiert. In diesem Szenario ist der Client in der Regel ein Middle-Tier-Webdienst, ein Daemon-Dienst oder der Website.

## <a name="client-credentials-grant-flow-diagram"></a>Clientanmeldeinformationen gewähren Datenflussdiagramm

Im folgende Diagramm wird erläutert, wie die Clientanmeldeinformationen Fluss funktioniert in Azure Active Directory (Azure AD) gewähren.

![OAuth2.0 Client Anmeldeinformationen gewähren Fluss](media/active-directory-protocols-oauth-service-to-service/active-directory-protocols-oauth-client-credentials-grant-flow.jpg)

1. Die Clientanwendung Azure AD Tokenausgabe Endpunkt authentifiziert und ein Zugriffstoken anfordert.
2. Azure AD Tokenausgabe Endpunkt stellt das Zugriffstoken.
3. Das Zugriffstoken wird der abgesicherten Ressource authentifizieren verwendet.
4. Daten aus der gesicherten Ressource werden an die Anwendung zurückgegeben.

## <a name="register-the-services-in-azure-ad"></a>Registrieren Sie die Dienste in Azure AD

Registrieren der Gesprächsdienst und den empfangenden Dienst in Azure Active Directory (Azure AD). Ausführliche Informationen finden Sie unter [Hinzufügen, aktualisieren und Entfernen einer Anwendung](active-directory-integrating-applications.md#BKMK_Native)

## <a name="request-an-access-token"></a>Ein Zugriffstoken anfordern

Anfordern ein Zugriffstokens verwenden HTTP POST für die mandantenspezifische Azure AD-Endpunkt.

```
https://login.microsoftonline.com/<tenant id>/oauth2/token
```

## <a name="service-to-service-access-token-request"></a>Dienst-token Anfrage

Eine Dienst-token Anfrage enthält die folgenden Parameter.

| Parameter | | Beschreibung |
|-----------|------|------------|
| Antworttyp | Erforderlich | Gibt den angeforderten Antwort. In einem Client Anmeldeinformationen gewähren muss der Wert **Client_credentials**.|
| client_id | Erforderlich | Gibt die Azure AD Client-Id des aufrufenden Web Service. Um die aufrufende Anwendung Client-ID in Azure-Verwaltungsportal zu suchen, klicken Sie auf **Active Directory**, klicken Sie auf das Verzeichnis klicken Sie auf die Anwendung und klicken Sie auf **Konfigurieren**.|
| client_secret | Erforderlich |  Geben Sie einen Schlüssel für den aufrufenden Webdienst in Azure AD registriert. Zum Erstellen eines Schlüssels in der Azure-Verwaltungsportal auf **Active Directory**, klicken Sie auf das Verzeichnis, klicken Sie auf die Anwendung, und klicken Sie dann auf **Konfigurieren**. |
| Ressource | Erforderlich | Geben Sie den App-ID URI des empfangenden Web Service. Um URI-ID App in Azure-Verwaltungsportal zu suchen, klicken Sie auf **Active Directory**, klicken Sie auf das Verzeichnis klicken Sie auf die Anwendung und klicken Sie auf **Konfigurieren**. |

## <a name="example"></a>Beispiel

HTTP POST fordert ein Zugriffstoken für den Webdienst https://service.contoso.com/. Die `client_id` assoziierte, das Zugriffstoken anfordert.

```
POST contoso.com/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&client_id=625bc9f6-3bf6-4b6d-94ba-e97cf07a22de&client_secret=qkDwDJlDfig2IpeuUZYKH1Wb8q1V0ju6sILxQQqhJ+s=&resource=https%3A%2F%2Fservice.contoso.com%2F
```

## <a name="service-to-service-access-token-response"></a>Token Response Service-Zugriff

Eine erfolgreiche Antwort enthält eine Antwort JSON OAuth 2.0 mit den folgenden Parametern.

| Parameter   | Beschreibung |
|-------------|-------------|
|Zugriffstoken |Die angeforderte Zugriffstoken. Dieses Token können der aufrufenden Webdienst empfangen Webdienst authentifiziert. |
|access_type  | Gibt den Tokentyp Wert. Der einzige Typ Azure AD unterstützt ist **Träger**. Weitere Informationen zu trägertoken finden Sie unter der [OAuth 2.0 Autorisierungsframeworks: Träger Token Verwendung (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt).
|expires_in   | Wie lange das Zugriffstoken (in Sekunden) gültig ist.|
|expires_on   |Die Zeit des Zugriffstokens Ablauf. Das Datum wird als die Anzahl der Sekunden zwischen 1970 dargestellt-01-01T0:0:0Z UTC Zeitpunkt ablaufen. Dieser Wert wird verwendet, um die Gültigkeitsdauer des zwischengespeicherten Tokens zu bestimmen. |
|Ressource     | App-ID URI des Webdiensts empfangen. |

## <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine Erfolg Antwort auf eine Anforderung für ein Zugriffstoken an einen Webdienst.

```
{
"access_token":"eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0ODI2NywibmJmIjoxMzg4NDQ4MjY3LCJleHAiOjEzODg0NTIxNjcsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsInN1YiI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZS8iLCJhcHBpZCI6ImQxN2QxNWJjLWM1NzYtNDFlNS05MjdmLWRiNWYzMGRkNThmMSIsImFwcGlkYWNyIjoiMSJ9.aqtfJ7G37CpKV901Vm9sGiQhde0WMg6luYJR4wuNR2ffaQsVPPpKirM5rbc6o5CmW1OtmaAIdwDcL6i9ZT9ooIIicSRrjCYMYWHX08ip-tj-uWUihGztI02xKdWiycItpWiHxapQm0a8Ti1CWRjJghORC1B1-fah_yWx6Cjuf4QE8xJcu-ZHX0pVZNPX22PHYV5Km-vPTq2HtIqdboKyZy3Y4y3geOrRIFElZYoqjqSv5q9Jgtj5ERsNQIjefpyxW3EwPtFqMcDm4ebiAEpoEWRN4QYOMxnC9OUBeG9oLA0lTfmhgHLAtvJogJcYFzwngTsVo6HznsvPWy7UP3MINA",
"token_type":"Bearer",
"expires_in":"3599",
"expires_on":"1388452167",
"resource":"https://service.contoso.com/"
}
```

## <a name="see-also"></a>Siehe auch

* [OAuth 2.0 in Azure AD](active-directory-protocols-oauth-code.md)
