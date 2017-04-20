<properties
    pageTitle="Azure AD Verbindung synchronisieren: Understanding deklarative Bereitstellung Ausdrücke | Microsoft Azure"
    description="Erläutert die Bereitstellung deklarativen Ausdrücken."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understanding-declarative-provisioning-expressions"></a>Azure AD Verbindung synchronisieren: Understanding deklarative Bereitstellung Ausdrücke
Azure AD Connect Sync baut auf deklarative Bereitstellung Forefront Identity Manager 2010 eingeführt. Dadurch werden vollständige Identity Integration Geschäftslogik ohne kompilierten Code schreiben implementieren.

Ein wesentlicher Bestandteil deklarative Bereitstellung ist Attribut Gelder Ausdruckssprache. Die Sprache ist eine Teilmenge von Microsoft® Visual Basic® für Applikationen (VBA). Diese Sprache wird in Microsoft Office verwendet und Benutzer mit VBScript auch erkennt. Deklarative Bereitstellung Ausdruckssprache nur Funktionen verwendet und ist eine strukturierte Sprache. Es sind keine Methoden oder Anweisungen. Funktionen werden stattdessen express Programmablauf verschachtelt.

Weitere Informationen finden Sie unter [Visual Basic for Applications-Sprachreferenz für Office 2013 Willkommen](https://msdn.microsoft.com/library/gg264383.aspx).

Die Attribute sind stark typisiert. Eine Funktion akzeptiert nur Attribute vom richtigen Typ. Außerdem ist die Groß-/Kleinschreibung beachtet. Funktionsnamen und Attributnamen müssen korrekte Groß-/Kleinschreibung oder ein Fehler ausgelöst.

## <a name="language-definitions-and-identifiers"></a>Definitionen und Language IDs

- Funktionen haben einen Namen, gefolgt von Argumenten in Klammern: Funktionsname (Argument 1, N-Argument).
- Attribute werden durch Klammern gekennzeichnet: [Attributname]
- Parameter werden durch Prozentzeichen identifiziert: ParameterName %
- Zeichenfolgenkonstanten sind von Anführungszeichen umgeben: beispielsweise "Contoso" (Hinweis: Verwenden Sie gerade Anführungszeichen "" und nicht typografische Anführungszeichen "")
- Numerische Werte werden ohne Anführungszeichen ausgedrückt und voraussichtlich decimal. Hexadezimalwerte vorangestellt & h Z. B. 98052 & HFF
- Boolesche Werte werden mit Konstanten ausgedrückt: True, False.
- Integrierte Konstanten und Literale mit ihrem Namen angegeben: NULL, CRLF, IgnoreThisFlow

### <a name="functions"></a>Funktionen
Deklarative Bereitstellung verwendet viele Funktionen können Werte umwandeln. Diese Funktionen können geschachtelt werden, damit das Ergebnis einer Funktion an eine Funktion übergeben wird.

`Function1(Function2(Function3()))`

Die vollständige Liste der Funktionen finden in der [Funktionsverweis](active-directory-aadconnectsync-functions-reference.md).

### <a name="parameters"></a>Parameter
Ein Parameter ist ein Connector oder vom Administrator mit PowerShell definiert. Parameter enthalten normalerweise Werte, die von System zu System unterschiedlich sind, z. B. der Namen der Domäne der Benutzer befindet. Diese Parameter können in attributflüssen verwendet werden.

Active Directory Connector bereitgestellt die folgenden Parameter für eingehende Regeln:

| Parametername | Kommentar |
| --- | --- |
| Domain.Netbios | NetBIOS-Format der Domäne aktuell importiert wird, z. B. FABRIKAMSALES |
| Domain.FQDN | FQDN-Format der Domäne aktuell importiert wird, z. B. sales.fabrikam.com |
| Domain.LDAP | LDAP-Format der Domäne aktuell importiert wird, z. B. DC = Sales, DC = Fabrikam, DC = com |
| Forest.Netbios | NetBIOS-Name der Gesamtstruktur aktuell importiert wird, z. B. FABRIKAMCORP format |
| Forest.FQDN | Name der Gesamtstruktur aktuell importiert wird, z. B. fabrikam.com FQDN format |
| Forest.LDAP | LDAP-Format der Gesamtstrukturname aktuell importiert wird, z. B. DC = Fabrikam, DC = com |

Das System bietet den folgenden Parameter den Bezeichner der laufenden Verbindung abgerufen:  
`Connector.ID`

Hier ist ein Beispiel, das Metaverse-Attributdomäne mit dem NetBIOS-Namen der Domäne auffüllt, wo sich der Benutzer befindet:  
`domain` <- `%Domain.Netbios%`

### <a name="operators"></a>Operatoren
Die folgenden Operatoren können verwendet werden:

- **Vergleich**: <, < = <>, =, >, > =
- **Mathematik**: +, -, \*,
- **Zeichenfolge**: & (Verkettung)
- **Logische**: & & und || (oder)
- **Auswertungsreihenfolge**:)

Links nach rechts ausgewertet und die gleiche Bewertung Priorität. Das heißt, die \* (Multiplikator) nicht vor - (Subtraktion) ausgewertet. 2\*(5 + 3) entspricht nicht 2\*5 + 3. Klammern () werden verwendet, um die Auswertungsreihenfolge von links nach rechts Auswertungsreihenfolge entsprechende nicht ändern.

## <a name="multi-valued-attributes"></a>Mehrwertige Attribute
Die Funktion können sowohl einwertige als auch mehrwertige Attribute bearbeiten. Für mehrwertige Attribute die Funktion arbeitet über jeden Wert und jeder Wert für dieselbe Funktion.

Zum Beispiel:  
`Trim([proxyAddresses])`Führen Sie Trim jeden Wert im Attribut ProxyAddress.  
`Word([proxyAddresses],1,"@") & "@contoso.com"`Für jeden Wert mit einer @-sign, Domäne ersetzen durch @contoso.com.  
`IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])`Die SIP-Adresse suchen und die Werte entfernen.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen über Konfigurationsmodell [Verständnis deklarative](active-directory-aadconnectsync-understanding-declarative-provisioning.md)Bereitstellung.
- Siehe wie deklarative Bereitstellung verwendeten Out-of-Box Verständnis [der Standardkonfiguration](active-directory-aadconnectsync-understanding-default-configuration.md)ist.
- Finden Sie unter praktische mit deklarativen Bereitstellung wie [Ändern Sie die Standardkonfiguration](active-directory-aadconnectsync-change-the-configuration.md)ändern.

**Themen (Übersicht)**

- [Azure AD Verbindung synchronisieren: verstehen und Synchronisation anpassen](active-directory-aadconnectsync-whatis.md)
- [Integration von lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)

**Themen**

- [Azure AD Verbindung synchronisieren: Funktionen Verweis](active-directory-aadconnectsync-functions-reference.md)
