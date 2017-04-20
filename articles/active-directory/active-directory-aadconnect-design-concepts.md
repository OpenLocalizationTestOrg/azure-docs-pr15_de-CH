<properties
   pageTitle="Azure AD Connect: Designkonzepte | Microsoft Azure"
   description="In diesem Thema werden bestimmte Bereiche der Implementierung design"
   services="active-directory"
   documentationCenter=""
   authors="billmath"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.custom = "azure-ad-connect"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="Identity"
   ms.date="09/13/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-design-concepts"></a>Azure AD Connect: Konzepte
Dieses Thema ist Bereiche beschreiben, die während der Implementierungsentwurf Azure AD Connect durchdacht sein muss. Dieses Thema ist ein gründlicher Blick auf bestimmte Bereiche und diese Konzepte werden in anderen Themen kurz beschrieben.

## <a name="sourceanchor"></a>sourceAnchor
Das SourceAnchor-Attribut wird als *Attribut unveränderlich während der Lebensdauer eines Objekts*definiert. Er kennzeichnet eindeutig ein Objekt die gleichen Objekt lokal und in Azure AD. Das Attribut wird auch als **ImmutableId** bezeichnet, und die zwei Namen sind austauschbar verwendet.

Das Wort unveränderlich, das "kann nicht geändert werden", muss dieses Thema. Da der Wert dieses Attributs geändert werden kann, nachdem es festgelegt wurde, ist es wichtig, eine Design auswählen, die das Szenario unterstützt.

Das Attribut wird für die folgenden Szenarien verwendet:

- Wenn eine neue Engine Synchronisierungsserver erstellt oder nach einem Systemausfall neu erstellt, verknüpft dieses Attribut vorhandene Objekten in Azure AD mit lokalen Objekten.
- Verschieben von Cloud nur Identität synchronisierten Identitätsmodell ermöglicht dieses Attribut Objekte "harte Entsprechung" vorhandenen Objekten in Azure AD mit lokalen Objekten.
- Bei Verwendung von Föderationen wird dann dieses Attribut mit **UserPrincipalName** Anspruch zum Benutzer eindeutig identifiziert.

In diesem Thema spricht nur über SourceAnchor auf Benutzer bezieht. Die gleichen Regeln gelten für alle Objekttypen, aber es ist nur für Benutzer ist dies normalerweise ein Problem.

### <a name="selecting-a-good-sourceanchor-attribute"></a>Ein guter SourceAnchor Attribut auswählen
Der Attributwert muss die folgenden Regeln:

- Weniger als 60 Zeichen lang sein
    - Zeichen, die kein-Z, A-Z oder 0-9 codiert und als 3 Zeichen
- Ein besonderes Zeichen: & #92;! # $ % & * + / = ? ^ & #96; { } | ~ < > () "; : , [ ] " @ _
- Muss eindeutig sein
- Muss Zeichenfolge, Ganzzahl oder binär
- Keine Benutzernamen geändert beruhen
- Nicht beachtet und vermeiden Fall variieren Werte
- Sollten beim Erstellen des Objekts zugewiesen werden

Ist die ausgewählte SourceAnchor nicht vom Typ String, dann Azure AD verbinden Base64Encode der den Attributwert keine Sonderzeichen angezeigt. Verwenden Sie einen anderen Verbundserver als ADFS sicherstellen Sie Servers können das Attribut auch Base64Encode.

Das Attribut SourceAnchor beachtet werden. Der Wert "JohnDoe" ist nicht "Johndoe" identisch. Aber Sie sollten nicht zwei verschiedene Objekte mit nur einer Anfrage.

Haben Sie eine einzelne Gesamtstruktur ist lokal, dann das Attribut verwendende **ObjectGUID**. Dies ist auch das Attribut express Einstellungen in Azure AD Connect verwenden und das Attribut DirSync verwendet.

Wenn Sie mehrere Gesamtstrukturen und Benutzer nicht zwischen Gesamtstrukturen und Domänen verschieben, ist **ObjectGUID** eine gute Attribut auch in diesem Fall verwenden.

Beim Verschieben von Benutzern zwischen Gesamtstrukturen und Domänen müssen finden Sie ein Attribut, das nicht geändert oder verschoben werden mit den Benutzern beim Verschieben. Einführen einer synthetischen Attributs wird ein empfohlen. Ein Attribut, das etwas halten, das aussieht wie eine GUID geeignet. Beim Erstellen, des Objekts eine neue GUID erstellt und für den Benutzer versehen. In Modul Synchronisierungsserver dieser Wert **ObjectGUID** erstellt und aktualisiert das ausgewählte Attribut fügt kann benutzerdefinierte Synchronisierungsregel erstellt werden. Wenn Sie das Objekt verschieben, müssen Sie auch kopiert den Inhalt dieses Werts.

Eine andere Lösung ist ein vorhandenes Attribut wählen Sie wissen Sie nicht ändern. Häufig verwendete Attribute umfassen **EmployeeID**. Betrachten Sie ein Attribut, das Buchstaben enthält, sicher, dass kein Fall (Großbuchstabe oder Kleinbuchstabe) für den Wert des Attributs ändern kann. Ungültige Attribute, die nicht verwendet werden sollte enthalten die Attribute mit dem Namen des Benutzers. Ehe oder Scheidung der Namen soll ändern, die für dieses Attribut nicht zulässig. Dies ist auch ein Grund Attribute **UserPrincipalName** **Mail**und **TargetAddress** nicht möglich Installationsassistenten von Azure AD Verbinden wählen. Diese Attribute auch enthalten die @-character, darf nicht in die SourceAnchor.

### <a name="changing-the-sourceanchor-attribute"></a>Ändern des SourceAnchor-Attributs
Wert des SourceAnchor-Attributs kann geändert werden, nachdem das Objekt in Azure AD erstellt wurde und die Identität.

Aus diesem Grund gelten die folgenden Einschränkungen Azure AD Connect:

- Das SourceAnchor-Attribut kann nur bei der ersten Installation festgelegt werden. Wenn Sie den Installationsassistenten erneut ausführen, ist diese Option schreibgeschützt. Wenn Sie diese Einstellung ändern müssen, müssen Sie deinstallieren und neu installieren.
- Wenn Sie einen anderen Azure AD Connect Server installieren, müssen Sie zuvor verwendete dasselbe SourceAnchor Attribut auswählen. Wenn Sie haben zuvor DirSync und Azure AD Connect verschieben, müssen Sie denn DirSync verwendete Attribut **ObjectGUID** verwenden.
- Der Wert für SourceAnchor nach geändert wurde das Objekt Sync löst einen Fehler aus und lässt keine Änderungen auf Objekt auf, bevor das Problem behoben wurde und die SourceAnchor im Quellverzeichnis geändert Azure AD dann Azure AD Connect exportiert.

## <a name="azure-ad-sign-in"></a>Azure AD anmelden
Und das lokale Verzeichnis mit Azure, ist es wichtig zu verstehen, wie die Synchronisierung wie Benutzer Einstellungen können authentifiziert. Azure AD verwendet UserPrincipalName (UPN) zur Authentifizierung des Benutzers. Beim Synchronisieren der Benutzer müssen Sie das Attribut Wert UserPrincipalName sorgfältig verwendet werden auswählen.

### <a name="choosing-the-attribute-for-userprincipalname"></a>Das Attribut UserPrincipalName auswählen
Bei der Auswahl des Attributs für die Bereitstellung sollte der Wert des UPN in Azure verwendet werden

- Die Attributwerte entsprechen die UPN-Syntax (RFC 822) Format sein sollteusername@domain
- Das Suffix der Werte entspricht einer überprüften benutzerdefinierten Domänen in Azure AD

In express-Einstellungen ist die angenommene Wahl für das Attribut UserPrincipalName. Enthält das UserPrincipalName-Attribut nicht den Wert der Benutzer Azure anmelden, dann wählen Sie **Benutzerdefinierte Installation**.

### <a name="custom-domain-state-and-upn"></a>Benutzerdefinierte Domänenstatus und UPN
Es ist wichtig sicherzustellen, dass eine Domäne überprüfte das UPN-Suffix.

John ist ein Benutzer auf contoso.com. John lokalen UPN soll john@contoso.com Azure anmelden, nachdem Sie Ihre Azure AD-Verzeichnis contoso.onmicrosoft.com Benutzer synchronisiert haben. Hierzu müssen Sie hinzufügen und contoso.com in Azure AD vor die Benutzern die Synchronisierung als benutzerdefinierte Domäne überprüfen. Entspricht das UPN-Suffix von John, z. B. contoso.com nicht überprüfte Domäne in Azure AD, ersetzt Azure AD mit contoso.onmicrosoft.com das UPN-Suffix.

### <a name="non-routable-on-premises-domains-and-upn-for-azure-ad"></a>Nicht routbare lokale Domänen und UPN für Azure AD
Einige Organisationen haben nicht routingfähige Domänen contoso.local oder einfache Etikett Domänen wie Contoso. Sie können nicht in Azure AD nicht Routingfähige Domäne überprüfen. Azure AD verbinden kann nur überprüfte Domäne in Azure AD synchronisieren. Beim Erstellen von Azure AD-Verzeichnis wird Routingfähige Domäne als Standarddomäne für Ihre Azure Anzeige beispielsweise contoso.onmicrosoft.com wird erstellt. Daher ist es notwendig alle Routingfähige Domäne der Fall bei der Standarddomäne onmicrosoft.com synchronisieren möchten.

Weitere Informationen zum Hinzufügen und Überprüfen von Domänen lesen Sie [Ihre benutzerdefinierten Domänennamen in Azure Active Directory hinzufügen](active-directory-add-domain.md) .

Azure AD Connect erkennt, wenn Sie in einer Umgebung nicht routingfähige Domänen und entsprechend warnen würden von mit express-Einstellungen. Wenn Sie in einer nicht Routingfähige Domäne befinden, ist wahrscheinlich der Benutzerprinzipalnamen der Benutzer nicht routbare Suffixe zu. Wenn Sie unter contoso.local ausgeführt, schlägt beispielsweise Azure AD verbinden Sie Standardeinstellungen anstatt express-Einstellungen verwenden. Standardeinstellungen verwenden, können Sie das Attribut an, das als UPN Azure anmelden, nachdem Benutzer in Azure Active Directory synchronisiert verwendet werden soll.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über [die lokalen Identitäten Azure Active Directory integrieren](active-directory-aadconnect.md).
