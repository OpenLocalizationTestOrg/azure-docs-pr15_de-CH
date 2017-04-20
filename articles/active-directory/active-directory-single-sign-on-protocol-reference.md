<properties
    pageTitle="Azure Einmalanmeldung SAML Protokoll | Microsoft Azure"
    description="Dieser Artikel beschreibt das einzelnen Zeichen auf SAML Protokoll in Azure Active Directory"
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

# <a name="single-sign-on-saml-protocol"></a>Einzelnes anmelden SAML-Protokoll

Dieser Artikel behandelt die Authentifizierungsanfragen SAML 2.0 und Antworten Azure Active Directory (Azure AD) für einmaliges Anmelden unterstützt.

Protokoll-Diagramm beschreibt die Abfolge für einzelne Zeichen. Cloud-Dienst (Service Provider) verwendet eine HTTP-Umleitung Bindung übergeben ein `AuthnRequest` (Authentifizierung) Anforderungselement Azure AD (Identitätsanbieter). Dann Azure AD verwendet HTTP Post verbindlich buchen einer `Response` Element zum Cloud-Dienst.

![Einmaliges Anmelden Workflow](media/active-directory-single-sign-on-protocol-reference/active-directory-saml-single-sign-on-workflow.png)

## <a name="authnrequest"></a>AuthnRequest

Um eine Authentifizierung anzufordern, Cloud-Dienste Senden einer `AuthnRequest` Element Azure AD. Ein Beispiel SAML 2.0 `AuthnRequest` könnte wie folgt aussehen:

```
<samlp:AuthnRequest
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="id6c1c178c166d486687be4aaf5e482730"
Version="2.0" IssueInstant="2013-03-18T03:28:54.1839884Z"
xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
</samlp:AuthnRequest>
```


| Parameter | | Beschreibung |
| ----------------------- | ------------------------------- | --------------- |
| ID | Erforderlich | Azure AD verwendet dieses Attribut zum Auffüllen der `InResponseTo` -Attribut des zurückgegebenen Antwort. ID darf nicht mit einer Zahl beginnen, damit eine verbreitete Strategie ist eine Zeichenfolge wie "Id" in den String-Darstellung einer GUID vorangestellt. Beispielsweise `id6c1c178c166d486687be4aaf5e482730` ist ungültig. |
| Version | Erforderlich | Dies sollte **2.0**.|
| IssueInstant | Erforderlich | Dies ist eine DateTime-Zeichenfolge mit UTC-Wert [Roundtripformat ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD erwartet einen DateTime-Wert dieses Typs, aber nicht ausgewertet oder Wert. |
| AssertionConsumerServiceUrl | Optionale | Wenn angegeben, muss dieser die `RedirectUri` des Cloud-Dienst in Azure AD. |
| ForceAuthn | Optionale | Sofern dies false sollte. Jeder andere Wert verursacht einen Fehler.|
| IsPassive | Optionale | Sofern dies false sollte. Jeder andere Wert verursacht einen Fehler. |  

Alle anderen `AuthnRequest` Attribute, z. B. Genehmigung, Ziel, AssertionConsumerServiceIndex, AttributeConsumerServiceIndex und ProviderName **ignoriert**werden.

Azure AD ignoriert die `Conditions` Element im `AuthnRequest`.

### <a name="issuer"></a>Aussteller

Die `Issuer` Element in einer `AuthnRequest` müssen exakt eines **ServicePrincipalNames** im Cloud-Dienst in Azure AD. Normalerweise wird diese **App ID URI** festgelegt, die während der anwendungsregistrierung angegeben wird.

Ein Beispiel SAML Auszug mit dem `Issuer` Element sieht folgendermaßen aus:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
```

### <a name="nameidpolicy"></a>NameIDPolicy

Dieses Format für einen bestimmten Name-ID in der Antwort anfordert und optional in `AuthnRequest` Elemente in Azure AD gesendet.

Beispiel `NameIdPolicy` Element sieht folgendermaßen aus:

```
<NameIDPolicy Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"/>
```

Wenn `NameIDPolicy` bereitgestellt, die optionale aufnehmen können `Format` Attribut. Die `Format` Attribut ist nur einer der folgenden Werte; Jeder andere Wert führt zu einem Fehler.

-  `urn:oasis:names:tc:SAML:2.0:nameid-format:persistent`: Azure Active Directory stellt NameID Anspruch als paarweise Bezeichner.
- `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`: Azure Active Directory stellt NameID Anspruch im e-Mail-Adressformat.
- `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified`: Dieser Wert ermöglicht Azure Active Directory Anspruch Format auswählen. Azure Active Directory stellt die NameID als paarweise Bezeichner.

Nehmen Sie nicht die `SPNameQualifer` Attribut. Azure AD ignoriert die `AllowCreate` Attribut.

### <a name="requestauthncontext"></a>RequestAuthnContext

Die `RequestedAuthnContext` Element gibt die gewünschten Authentifizierungsmethoden. Ist optional, `AuthnRequest` Elemente in Azure AD gesendet. Azure AD unterstützt nur eine `AuthnContextClassRef` Wert: `urn:oasis:names:tc:SAML:2.0:ac:classes:Password`.

### <a name="scoping"></a>Festlegen des Gültigkeitsbereichs

Die `Scoping` Element enthält eine Liste der Identitätsanbieter ist optional in `AuthnRequest` Elemente in Azure AD gesendet.

Sofern enthalten nicht die `ProxyCount` -Attribut, `IDPListOption` oder `RequesterID` Element nicht unterstützt.

### <a name="signature"></a>Signatur

Nicht enthalten eine `Signature` Element in `AuthnRequest` Elemente wie Azure AD unterstützt Authentifizierung signiert.

### <a name="subject"></a>Betreff

Azure AD ignoriert die `Subject` Element des `AuthnRequest` Elemente.

## <a name="response"></a>Antwort

Wenn eine angeforderte Anmeldung erfolgreich abgeschlossen ist, sendet Azure AD auf Cloud-Dienst. Beispiel auf einen erfolgreichen Versuch anmelden sieht folgendermaßen aus:

```
<samlp:Response ID="_a4958bfd-e107-4e67-b06d-0d85ade2e76a" Version="2.0" IssueInstant="2013-03-18T07:38:15.144Z" Destination="https://contoso.com/identity/inboundsso.aspx" InResponseTo="id758d0ef385634593a77bdf7e632984b6" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
    ...
  </ds:Signature>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
  <Assertion ID="_bf9c623d-cc20-407a-9a59-c2d0aee84d12" IssueInstant="2013-03-18T07:38:15.144Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">
    <Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
    <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      ...
    </ds:Signature>
    <Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
    </Subject>
    <Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
    </Conditions>
    <AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
    </AttributeStatement>
    <AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
    </AuthnStatement>
  </Assertion>
</samlp:Response>
```

### <a name="response"></a>Antwort

Die `Response` Element enthält das Ergebnis der Autorisierung. Azure AD legt die `ID`, `Version` und `IssueInstant` Werte in der `Response` Element. Er setzt außerdem die folgenden Attribute:

- `Destination`: Wenn die Anmeldung erfolgreich abgeschlossen wurde, dies soll die `RedirectUri` des Dienstanbieters (Cloud-Dienst).
- `InResponseTo`: Dieser Wert der `ID` Attribut der `AuthnRequest` -Element, das die Antwort initiiert.

### <a name="issuer"></a>Aussteller

Azure AD legt die `Issuer` Element, `https://login.microsoftonline.com/<TenantIDGUID>/` , <TenantIDGUID> Mieter ID des Azure AD-Mandanten.

Beispielsweise könnte eine Beispielantwort mit Aussteller aussehen:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

### <a name="status"></a>Status

Die `Status` Element vermittelt den Erfolg oder Misserfolg der Anmeldung. Sie enthält die `StatusCode` Element, das ein oder mehrere geschachtelte Codes, die den Status der Anforderung darstellen. Auch die `StatusMessage` Element enthält benutzerdefinierte Fehlermeldungen, die während der Anmeldung generiert.

<!-- TODO: Add a authentication protocol error reference -->

Die folgenden ist SAML auf Anmelden erfolglos.

```
<samlp:Response ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Requester">
      <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:RequestUnsupported" />
    </samlp:StatusCode>
    <samlp:StatusMessage>AADSTS75006: An error occurred while processing a SAML2 Authentication request. AADSTS90011: The SAML authentication request property 'NameIdentifierPolicy/SPNameQualifier' is not supported.
Trace ID: 66febed4-e737-49ff-ac23-464ba090d57c
Timestamp: 2013-03-18 08:49:24Z</samlp:StatusMessage>
  </samlp:Status>
```

### <a name="assertion"></a>Assertion

Zusätzlich zu den `ID`, `IssueInstant` und `Version`, Azure AD wird Folgendes in der `Assertion` Bestandteil der Antwort.

#### <a name="issuer"></a>Aussteller

Ist `https://sts.windows.net/<TenantIDGUID>/`, <TenantIDGUID> ist die Mandanten-ID Azure AD-Mandanten.

```
<Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

#### <a name="signature"></a>Signatur

Azure AD signiert die Assertion auf eine erfolgreiche Anmeldung. Die `Signature` Element enthält eine digitale Signatur, die Cloud-Dienst die Quelle zum Überprüfen der Integrität der Assertion authentifizieren kann.

Zum Generieren dieser digitalen Signatur Azure AD verwendet den Schlüssel in der `IDPSSODescriptor` Element von seinem Metadatendokument.

```
<ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      digital_signature_here
    </ds:Signature>
```

#### <a name="subject"></a>Betreff

Gibt die Anweisungen in die Assertion ist Principal. Es enthält einen `NameID` Element, das den authentifizierten Benutzer darstellt. Die `NameID` Wert ist eine gezielte Bezeichner nur an den Dienstanbieter weitergeleitet wird, die die Zielgruppe für das Token. Persistent - widerrufen, allerdings nie zugewiesen. Es ist außerdem undurchsichtig, offenbart nicht alles über den Benutzer und nicht als Bezeichner für Attributabfragen verwendet werden.

Die `Method` Attribut der `SubjectConfirmation` Element wird immer festgelegt `urn:oasis:names:tc:SAML:2.0:cm:bearer`.

```
<Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
</Subject>
```

#### <a name="conditions"></a>Startbedingungen

Dieses Element gibt die Bedingung, die die Nutzung von SAML-Assertionen zu definieren.

```
<Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
</Conditions>
```

Die `NotBefore` und `NotOnOrAfter` Attribute geben Sie das Intervall, in der die Assertion gültig ist.

- Der Wert der `NotBefore` Attribut ist gleich zu oder etwas (weniger als eine Sekunde) höher als der Wert der `IssueInstant` Attribut der `Assertion` Element. Azure AD nicht berücksichtigt alle Zeitunterschied zwischen sich selbst und Cloud-Dienst (Service Provider) und diesmal nicht Puffer hinzu.
- Der Wert der `NotOnOrAfter` Attribut ist höher als der Wert von 70 Minuten die `NotBefore` Attribut.

#### <a name="audience"></a>Zielgruppe

Enthält einen URI, der eine Zielgruppe identifiziert. Azure AD wird der Wert dieses Elements auf den Wert der `Issuer` Bestandteil der `AuthnRequest` , die die Anmeldung initiiert. Auswerten der `Audience` Wert, verwenden die `App ID URI` , die während der anwendungsregistrierung angegeben wurde.

```
<AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
</AudienceRestriction>
```

Wie die `Issuer` Wert der `Audience` Wert muss exakt einen Dienstprinzipalnamen, die Cloud-Dienst in Azure AD darstellt. Wenn der Wert des der `Issuer` Element ist kein URI-Wert der `Audience` Wert in der Antwort die `Issuer` Wert vorangestellt `spn:`.

#### <a name="attributestatement"></a>AttributeStatement

Vorgaben für den Betreff oder den Benutzer enthält. Der folgende Auszug enthält eine `AttributeStatement` Element. Die Ellipse besagt, dass das Element mehrere Attribute und Attributwerte enthalten kann.

```
<AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
</AttributeStatement>
```     

- **Namensanspruch** : der Wert der `Name` Attribut (`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`) ist der Benutzerprinzipalname des authentifizierten Benutzers `testuser@managedtenant.com`.
- **ObjectIdentifier Anspruch** : der Wert der `ObjectIdentifier` Attribut (`http://schemas.microsoft.com/identity/claims/objectidentifier`) ist die `ObjectId` des Directory-Objekts, das den authentifizierten Benutzer in Azure AD darstellt. `ObjectId`ist unveränderlich, global eindeutig und Wiederverwendung sicher Bezeichner des authentifizierten Benutzers.

#### <a name="authnstatement"></a>AuthnStatement

Dieses Element bestimmt, dass zu einem bestimmten Zeitpunkt von einem bestimmten Thema Assertion authentifiziert wurde.

- Die `AuthnInstant` Attribut gibt die Zeit an der Benutzerauthentifizierung mit Azure.
- Die `AuthnContext` Element gibt den Authentifizierung Kontext verwendet, um den Benutzer zu authentifizieren.

```
<AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
</AuthnStatement>
```
