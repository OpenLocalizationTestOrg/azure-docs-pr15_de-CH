<properties
    pageTitle="Azure einmaliges SAML Protokoll | Microsoft Azure"
    description="Dieser Artikel beschreibt die einzelnen Abmelde SAML Protokoll in Azure Active Directory"
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


# <a name="single-sign-out-saml-protocol"></a>Einzelne Abmeldung SAML-Protokoll

Azure Active Directory (Azure AD) unterstützt SAML 2.0 web Browser-Abmeldung Profils. Für einzelne Abmeldung muss ordnungsgemäß arbeiten Azure AD Metadaten URL bei der anwendungsregistrierung registrieren. Azure AD Ruft die Logout-URL und den Signaturschlüssel Cloud-Dienst aus den Metadaten ab. Verwendet die LogoutURL Benutzer umleiten, wenn sie abgemeldet Azure AD verwendet Signaturschlüssel zum Überprüfen der Signatur eingehender LogoutRequest

Cloud-Dienst einen Metadatenendpunkt nicht unterstützt, nachdem die Anwendung registriert ist, muss der Entwickler Microsoft Support Logout URL und Signaturschlüssel zu wenden.

Dieses Diagramm zeigt den Workflow der Abmeldung Azure AD Arbeitsgang.

![Einmaliges, Workflow](media/active-directory-single-sign-out-protocol-reference/active-directory-saml-single-sign-out-workflow.png)

## <a name="logoutrequest"></a>LogoutRequest

Cloud-Dienst sendet einen `LogoutRequest` Azure AD an, um anzugeben, dass eine Sitzung beendet wurde. Der folgende Codeausschnitt zeigt ein Beispiel `LogoutRequest` Element.

```
<samlp:LogoutRequest xmlns="urn:oasis:names:tc:SAML:2.0:metadata" ID="idaa6ebe6839094fe4abc4ebd5281ec780" Version="2.0" IssueInstant="2013-03-28T07:10:49.6004822Z" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.workaad.com</Issuer>
  <NameID xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
</samlp:LogoutRequest>
```

### <a name="logoutrequest"></a>LogoutRequest

Die `LogoutRequest` erfordert die folgenden Attribute Element Azure AD gesendet:

- `ID`: Gibt die Abmelde Anforderung. Der Wert der `ID` sollte nicht mit einer Zahl beginnen. Die normale Vorgehensweise ist **in die String-Darstellung einer GUID anzufügen** .

- `Version`: Legen Sie den Wert dieses Elements **2.0**. Dieser Wert ist erforderlich.

- `IssueInstant`: Dies ist ein `DateTime` Zeichenfolge mit koordinieren Coordinated Universal Time (UTC) Wert [Roundtripformat ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD erwartet einen Wert dieses Typs, aber nicht erzwungen.

- Die `Consent`, `Destination`, `NotOnOrAfter` und `Reason` Attribute werden ignoriert, wenn sie Bestandteil einer `LogoutRequest` Element.

### <a name="issuer"></a>Aussteller

Die `Issuer` Element in einer `LogoutRequest` müssen exakt eines **ServicePrincipalNames** im Cloud-Dienst in Azure AD. Normalerweise wird diese **App ID URI** festgelegt, die während der anwendungsregistrierung angegeben wird.

### <a name="nameid"></a>NameID

Der Wert der `NameID` Element muss genau die `NameID` des Benutzers signiert werden.
## <a name="logoutresponse"></a>LogoutResponse

Azure AD sendet eine `LogoutResponse` auf eine `LogoutRequest` Element. Der folgende Codeausschnitt zeigt ein Beispiel `LogoutResponse`.

```
<samlp:LogoutResponse ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
</samlp:LogoutResponse>
```

### <a name="logoutresponse"></a>LogoutResponse

Azure AD legt die `ID`, `Version` und `IssueInstant` Werte in der `LogoutResponse` Element. Wird die `InResponseTo` Element auf den Wert der `ID` Attribut der `LogoutRequest` ausgelöst, die die Antwort.

### <a name="issuer"></a>Aussteller

Azure AD festgelegt `https://login.microsoftonline.com/<TenantIdGUID>/` , <TenantIdGUID> ist die Mandanten-ID Azure AD-Mandanten.

Auswerten den Wert der `Issuer` Element, verwendet der Wert der **App-ID-URI** während der anwendungsregistrierung.

### <a name="status"></a>Status

Azure AD verwendet die `StatusCode` Element in der `Status` Element an den Erfolg oder Misserfolg der Abmeldung. Wenn die Abmeldung fehlschlägt, die `StatusCode` Element kann auch benutzerdefinierte Fehlermeldungen enthalten.
