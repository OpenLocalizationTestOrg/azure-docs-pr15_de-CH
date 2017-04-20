<properties
    pageTitle="Azure AD Verbundmetadaten | Microsoft Azure"
    description="Dieser Artikel beschreibt das Verbundmetadaten-Dokument, das Azure Active Directory Dienste veröffentlicht, die Azure Active Directory Token akzeptieren."
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


# <a name="federation-metadata"></a>Verbundmetadaten

Azure Active Directory (Azure AD) veröffentlicht eine Verbundmetadaten-Dokument für Dienste an Azure AD gibt die Sicherheitstoken konfiguriert. Das Dokumentformat Föderation Metadaten beschreibt [Web Services Federation Language (WS-Federation) Version 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html)der [Metadaten für die OASIS Security Assertion Markup Language (SAML) 2.0](http://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf)erweitert.

## <a name="tenant-specific-and-tenant-independent-metadata-endpoints"></a>Mandantenspezifische und Metadatenendpunkte Mieter unabhängig

Azure AD veröffentlicht mandantenspezifische und Mieter unabhängig Endpunkte.

Mandantenspezifische Endpunkte sind für einen bestimmten Mandanten konzipiert. Die Verbundmetadaten mandantenspezifische enthält Informationen Mieter einschließlich mandantenspezifische Aussteller und Endpunkt. Programme, die Zugriff auf einen einzelnen Mandanten verwenden mandantenspezifische Endpunkte.

Die Angaben Mieter unabhängig Endpunkte für alle Azure AD-Mandanten. Diese Information gilt für Mieter am *login.microsoftonline.com* und Mieter gemeinsam ist. Unabhängige Mieter Endpunkte sollten für Multi-Tenant-Applikationen nicht bestimmten Mandanten zugeordnet sind.

## <a name="federation-metadata-endpoints"></a>Föderation Metadatenendpunkte

Azure AD veröffentlicht Verbundmetadaten unter `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.

Für **mandantenspezifische Endpunkte**der `TenantDomainName` kann eine der folgenden Arten:

- Ein registrierten Domänennamen von Azure AD Mieter wie: `contoso.onmicrosoft.com`.
- Die unveränderlichen Mieter ID der Domäne, z. B. `72f988bf-86f1-41af-91ab-2d7cd011db45`.

Für **unabhängige Mieter Endpunkte**der `TenantDomainName` ist `common`. Dieses Dokument Listet nur die Verbundmetadaten-Elemente, die für alle Azure AD Mieter bei login.microsoftonline.com gehostet werden.

Z. B. möglicherweise ein Endpunkt mandantenspezifische `https:// login.microsoftonline.com/contoso.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml`. Der Mieter unabhängig Endpunkt ist [https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml](https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml). Sie können das Verbundmetadaten-Dokument durch Eingabe der URL in einem Browser anzeigen.

## <a name="contents-of-federation-metadata"></a>Inhalt der Föderation Metadaten

Der folgende Abschnitt enthält Informationen von Diensten, die von Azure AD ausgestellten Token verwenden.

### <a name="entity-id"></a>Entitäts-ID

Die `EntityDescriptor` Element enthält ein `EntityID` Attribut. Der Wert der `EntityID` Attribut stellt Emittenten, d. h. den Sicherheitstokendienst (STS), die das Token ausgestellt. Es ist wichtig, den Emittenten überprüft, wenn ein Token zu erhalten.

Die folgende Metadaten enthält eine mandantenspezifische `EntityDescriptor` mit einem `EntityID` Element.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="_b827a749-cfcb-46b3-ab8b-9f6d14a1294b"
entityID="https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db45/">
```
Die Mandanten-ID eine mandantenspezifische erstellen die Mandanten-ID im Endpunkt Mieter unabhängig ersetzen `EntityID` Wert. Der resultierende Wert wird der Tokenausgeber identisch sein. Die Strategie kann eines Multi-Tenant Emittenten für einen bestimmten Mandanten überprüft.

Die folgende Metadaten Beispiel einer Tenant-unabhängige `EntityID` Element. Bitte beachten Sie, dass die `{tenant}` ist ein Literal kein Platzhalter.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="="_0e5bd9d0-49ef-4258-bc15-21ce143b61bd"
entityID="https://sts.windows.net/{tenant}/">
```

### <a name="token-signing-certificates"></a>Tokensignaturzertifikate

Erhält ein Dienst einen Token, die Azure AD-Mandanten ausgestellt hat, muss die Signatur des Tokens mit einen Signaturschlüssel das Verbundmetadaten-Dokument veröffentlicht wird überprüft. Die Verbundmetadaten enthält den öffentlichen Teil der Mieter für die Tokensignatur verwendet Zertifikate. Die unformatierten Bytes des Zertifikats werden in der `KeyDescriptor` Element. Das Tokensignaturzertifikat gilt für die Signierung nur, wenn der Wert der `use` ist `signing`.

Eine Verbundmetadaten-Dokument veröffentlicht von Azure AD können mehrere Signaturschlüssel wie bei Azure AD Signaturzertifikat aktualisieren vorbereitet. Wenn eine Verbundmetadaten-Dokument mehrere Zertifikate enthält, sollte ein Dienst, der die Token überprüft alle Zertifikate im Dokument unterstützen.

Die folgende Metadaten Beispiel einer `KeyDescriptor` mit einem Schlüssel.

```
<KeyDescriptor use="signing">
<KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
<X509Data>
<X509Certificate>
MIIDPjCCAiqgAwIBAgIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTIwNjA3MDcwMDAwWhcNMTQwNjA3MDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEArCz8Sn3GGXmikH2MdTeGY1D711EORX/lVXpr+ecGgqfUWF8MPB07XkYuJ54DAuYT318+2XrzMjOtqkT94VkXmxv6dFGhG8YZ8vNMPd4tdj9c0lpvWQdqXtL1TlFRpD/P6UMEigfN0c9oWDg9U7Ilymgei0UXtf1gtcQbc5sSQU0S4vr9YJp2gLFIGK11Iqg4XSGdcI0QWLLkkC6cBukhVnd6BCYbLjTYy3fNs4DzNdemJlxGl8sLexFytBF6YApvSdus3nFXaMCtBGx16HzkK9ne3lobAwL2o79bP4imEGqg+ibvyNmbrwFGnQrBc1jTF9LyQX9q+louxVfHs6ZiVwIDAQABo2IwYDBeBgNVHQEEVzBVgBCxDDsLd8xkfOLKm4Q/SzjtoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAA4IBAQAkJtxxm/ErgySlNk69+1odTMP8Oy6L0H17z7XGG3w4TqvTUSWaxD4hSFJ0e7mHLQLQD7oV/erACXwSZn2pMoZ89MBDjOMQA+e6QzGB7jmSzPTNmQgMLA8fWCfqPrz6zgH+1F1gNp8hJY57kfeVPBiyjuBmlTEBsBlzolY9dd/55qqfQk6cgSeCbHCy/RU/iep0+UsRMlSgPNNmqhj5gmN2AFVCN96zF694LwuPae5CeR2ZcVknexOWHYjFM0MgUSw0ubnGl0h9AJgGyhvNGcjQqu9vd1xkupFgaN+f7P3p3EVN5csBg5H94jEcQZT7EKeTiZ6bTrpDAnrr8tDCy8ng
</X509Certificate>
</X509Data>
</KeyInfo>
</KeyDescriptor>
  ```

Die `KeyDescriptor` Element wird an zwei Stellen in der Verbundmetadaten-Dokument; in der WS-Verbund-spezifische und SAML-spezifische. In beiden Abschnitten veröffentlicht Zertifikate gleich.

WS-Verbund-spezifischen Abschnitt einer WS-Federation Metadaten würde Lesen die Zertifikate von einer `RoleDescriptor` mit der `SecurityTokenServiceType` Typ.

Die folgende Metadaten Beispiel einer `RoleDescriptor` Element.

```
<RoleDescriptor xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:fed="http://docs.oasis-open.org/wsfed/federation/200706" xsi:type="fed:SecurityTokenServiceType"protocolSupportEnumeration="http://docs.oasis-open.org/wsfed/federation/200706">
```

Im Abschnitt SAML-spezifische lautet ein Metadaten-Leser WS-Federation Zertifikate von einer `IDPSSODescriptor` Element.

Die folgende Metadaten Beispiel einer `IDPSSODescriptor` Element.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
```
Es bestehen keine Unterschiede im Format mandantenspezifische und Mieter unabhängig.

### <a name="ws-federation-endpoint-url"></a>WS-Federation Endpunkt-URL

Die Verbundmetadaten enthält die URL für-in und einzelne Abmeldung WS-Federation Protokoll Azure AD verwendet. Dieser Endpunkt wird in der `PassiveRequestorEndpoint` Element.

Die folgende Metadaten zeigt ein Beispiel des `PassiveRequestorEndpoint` -Element für eine mandantenspezifische Endpunkt.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db45/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```
Für den Endpunkt Mieter unabhängig wird WS-Verbund-URL in WS-Verbund-Endpunkt wie im folgenden Beispiel gezeigt.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/common/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```

### <a name="saml-protocol-endpoint-url"></a>SAML Protokoll Endpunkt-URL

Die Verbundmetadaten enthält die URL Azure AD-in und einzelne Abmeldung SAML 2.0-Protokoll verwendet. Diese Endpunkte werden in der `IDPSSODescriptor` Element.

Anmelden und Abmelden URLs werden in den `SingleSignOnService` und `SingleLogoutService` Elemente.

Die folgende Metadaten Beispiel einer `PassiveResistorEndpoint` für eine mandantenspezifische Endpunkt.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com /saml2" />
  </IDPSSODescriptor>
```

Ebenso die Endpunkte für gemeinsame SAML 2.0 Protokoll Endpunkte die Mandanten unabhängig Verbundmetadaten erscheinen in wie im folgenden Beispiel gezeigt.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
  </IDPSSODescriptor>
```
