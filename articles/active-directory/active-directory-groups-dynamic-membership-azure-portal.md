
<properties
    pageTitle="Erweiterte Regeln für Gruppenmitgliedschaft in Azure Active Directory Vorschau erstellen mit Attributen | Microsoft Azure"
    description="Erweiterte Regeln erstellen, einschließlich der dynamischen Mitgliedschaft Regel Ausdrucksoperatoren und Parameter unterstützt"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="curtand"/>


# <a name="using-attributes-to-create-advanced-rules-for-group-membership-in-azure-active-directory-preview"></a>Erweiterte Regeln für Gruppenmitgliedschaft in Azure Active Directory Vorschau erstellen mithilfe von Attributen

Azure-Portal bietet die Möglichkeit, erweiterte Regeln aktivieren komplexere Attribut-basierte dynamische Mitgliedschaften für Azure Active Directory (Azure AD) Vorschau-Gruppen erstellen. [Neuigkeiten in der Vorschau](active-directory-preview-explainer.md) Dieser Artikel beschreibt die regelattribute und Syntax dieser erweiterten Regeln erstellen.

## <a name="to-create-the-advanced-rule"></a>Erweiterte Regel erstellen

1.  Melden Sie sich bei [Azure-Portal](https://portal.azure.com) mit einem Konto kein globaler Administrator für das Verzeichnis.

2.  Wählen Sie **Weitere Dienste**, geben Sie im Textfeld **Benutzer und Gruppen** und wählen Sie dann die **EINGABETASTE**.

  ![Verwaltung öffnen](./media/active-directory-groups-dynamic-membership-azure-portal/search-user-management.png)

3.  Wählen Sie **alle Gruppen**, auf die **Benutzer und Gruppen** .

  ![Öffnen das Blade Gruppen](./media/active-directory-groups-dynamic-membership-azure-portal/view-groups-blade.png)

4. Wählen Sie auf dem Blatt **Benutzer und Gruppen - alle** **Hinzufügen** .

  ![Neue Gruppe hinzufügen](./media/active-directory-groups-dynamic-membership-azure-portal/add-group-type.png)

5. Geben Sie auf **das Blatt** einen Namen und eine Beschreibung für die neue Gruppe. Wählen Sie eine **Mitgliedschaft geben** **Dynamische** oder **Dynamische Gerät**, je nachdem, ob Sie eine Regel für Benutzer oder Geräte erstellen und dann **Dynamische Abfrage hinzufügen**. Attribute für Regeln finden Sie unter [Using Attribute Regeln für Gerät erstellen](#using-attributes-to-create-rules-for-device-objects).

  ![Dynamische Mitgliedschaftsregel hinzufügen](./media/active-directory-groups-dynamic-membership-azure-portal/add-dynamic-group-rule.png)

6. Blade **dynamische Mitgliedschaftsregeln** Geben Sie der Regel **Hinzufügen dynamischen Mitgliedschaft erweiterte Regel** im, drücken Sie die EINGABETASTE, und wählen Sie **Erstellen** am unteren Rand der Blade.

7. Wählen Sie **Erstellen** auf **dem Blatt erstellen** .

## <a name="constructing-the-body-of-an-advanced-rule"></a>Den Hauptteil einer erweiterten Regel erstellen

Die erweiterte Regel für dynamischen Mitgliedschaften für Gruppen erstellen ist im Wesentlichen ein binärer Ausdruck, der besteht aus drei Teilen und ein Ergebnis true oder false ergibt. Die drei Teile sind:

- Linke parameter
- Binärer operator
- Rechts-Konstante

Vollständige erweiterte Regel sieht etwa wie folgt: (LeftParameter BinaryOperator "RightConstant"), öffnende und schließende Klammer müssen für den gesamten binären Ausdruck Anführungszeichen sind erforderlich, damit die richtige Konstante und die Syntax für den linken Parameter ist user.property. Eine erweiterte Regel kann mehrere binäre Ausdrücke durch getrennte bestehen- und- oder -keine logischen Operatoren.

Es folgen Beispiele für einen ordnungsgemäß konstruierten erweiterte Regel:

- (user.department - Eq "Sales")- oder (user.department - Eq "Marketing")
- (user.department - Eq "Sales")- und nicht (user.jobTitle-enthält "SDE")

Vollständige Liste der unterstützten Parameter und Operatoren für Ausdrücke Regel finden Sie weiter unten. Attribute für Regeln finden Sie unter [Using Attribute Regeln für Gerät erstellen](#using-attributes-to-create-rules-for-device-objects).

Die Gesamtlänge des Textkörpers der erweiterten Regel darf 2048 Zeichen nicht überschreiten.

> [AZURE.NOTE]
>Zeichenfolge und Regex sind Groß-und Kleinschreibung. Sie können auch Null überprüft $null als Konstante, z. B. mit, user.department - Eq $null ausführen.
Zeichenfolgen mit Anführungszeichen "müssen mit Escapezeichen ' Zeichen user.department - Eq \`"Sales".

## <a name="supported-expression-rule-operators"></a>Unterstützte Ausdrucksoperatoren Regel
Die folgende Tabelle listet alle unterstützten Ausdruck Regeloperatoren und ihre Syntax im Hauptteil der erweiterten Regel verwendet werden:

| Operator        | Syntax         |
|-----------------|----------------|
| Ungleich      | -ne            |
| Ist gleich          | -eq            |
| Beginnt nicht mit | -notStartsWith |
| Beginnt mit     | StartsWith-    |
| Enthält nicht    | -notContains   |
| Enthält        | -enthält      |
| Keine Übereinstimmung       | -notMatch      |
| Übereinstimmung           | -entsprechen         |


## <a name="query-error-remediation"></a>Abfrage Fehler Behebung
Die folgende Tabelle enthält mögliche Fehler und korrigieren treten

| Fehler beim Analysieren der Abfrage     | Fehler Verwendung       | Korrigierte Verwendung             |
|-----------------------|-------------------|-----------------------------|
| Fehler: Attribut nicht unterstützt.                                      | (user.invalidProperty - Eq "Wert")       | (user.department - Eq "Wert")<br/>Eigenschaft übereinstimmen aus der [Eigenschaftenliste unterstützt](#supported-properties).                          |
| Fehler: Operator wird auf Attribut nicht unterstützt.                       | (user.accountEnabled-enthält True)                                                                               | (user.accountEnabled - Eq WAHR)<br/>Eigenschaft ist vom Typ Boolean. Die unterstützten Operatoren (-Eq oder - Ne) für booleschen Typ aus der obigen Liste.                                                                                                                                   |
| Fehler: Fragen Sie Kompilierungsfehler ab.                                      | (user.department - Eq "Sales")- und (user.department - Eq "Marketing")(user.userPrincipalName-match"*@domain.ext") | (user.department - Eq "Sales")- und (user.department - Eq "Marketing")<br/>Der logische Operator sollte aus der Liste unterstützten Eigenschaften überein. (user.userPrincipalName-entsprechen ".*@domain.ext")or(user.userPrincipalName -entsprechen "@domain.ext$")Error im regulären Ausdruck. |
| Fehler: Binärer Ausdruck ist nicht im richtigen Format.                     | (user.department – Eq "Verkauf") (user.department - Eq "Verkauf") (user.department-Eq "Verkauf")                             | (user.accountEnabled - Eq True)- und (user.userPrincipalName-enthält"alias@domain")<br/>Abfrage hat mehrere Fehler. Klammer nicht an der richtigen Stelle.                                                                                                                            |
| : Fehler Fehler beim dynamischen Mitgliedschaft einrichten. | (user.accountEnabled - Eq "True" und user.userPrincipalName-enthält"alias@domain")                               | (user.accountEnabled - Eq True)- und (user.userPrincipalName-enthält"alias@domain")<br/>Abfrage hat mehrere Fehler. Klammer nicht an der richtigen Stelle.                                                                                                                            |

## <a name="supported-properties"></a>Unterstützte Eigenschaften
Es folgen alle Benutzereigenschaften, die in der erweiterten Regel verwendet werden können:

### <a name="properties-of-type-boolean"></a>Eigenschaften des Typs boolean

Zulässige Operatoren

* -eq


* -ne


| Eigenschaften     | Zulässige Werte  | Verwendung                          |
|----------------|-----------------|--------------------------------|
| accountEnabled | wahr falsch      | user.accountEnabled - Eq WAHR)  |
| dirSyncEnabled | True, false null | (user.dirSyncEnabled - Eq WAHR) |

### <a name="properties-of-type-string"></a>Eigenschaften vom Typ string

Zulässige Operatoren

* -eq


* -ne


* -notStartsWith


* StartsWith-


* -enthält


* -notContains


* -entsprechen


* -notMatch

| Eigenschaften                 | Zulässige Werte                                                                                        | Verwendung                                                     |
|----------------------------|-------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| Ort                       | Zeichenfolge oder $null                                                                           | (User.Stadt - Eq "Wert")                                   |
| Land                    | Zeichenfolge oder $null                                                                            | (User.Land - Eq "Wert")                                |
| Abteilung                 | Zeichenfolge oder $null                                                                          | (user.department - Eq "Wert")                             |
| displayName                | Eine Zeichenfolge                                                                                 | (user.displayName - Eq "Wert")                            |
| facsimileTelephoneNumber   | Zeichenfolge oder $null                                                                           | (user.facsimileTelephoneNumber - Eq "Wert")               |
| Vorname                  | Zeichenfolge oder $null                                                                           | (user.givenName - Eq "Wert")                              |
| jobTitle                   | Zeichenfolge oder $null                                                                           | (user.jobTitle - Eq "Wert")                               |
| e-Mail-Nachrichten                       | Zeichenfolge oder $null (SMTP-Adresse des Benutzers)                                                  | (user.mail - Eq "Wert")                                   |
| mailNickName               | Jeder Zeichenfolgenwert (e-Mail-Alias des Benutzers)                                                            | (user.mailNickName - Eq "Wert")                           |
| Mobile                     | Zeichenfolge oder $null                                                                           | (user.mobile - Eq "Wert")                                 |
| objectId                   | GUID des Benutzerobjekts                                                                            | (user.objectId - Eq "1111111-1111-1111-1111-111111111111") |
| passwordPolicies           | Keine DisableStrongPassword DisablePasswordExpiration DisablePasswordExpiration, DisableStrongPassword |   (user.passwordPolicies - Eq "DisableStrongPassword")                                                      |
| physicalDeliveryOfficeName | Zeichenfolge oder $null                                                                            | (user.physicalDeliveryOfficeName - Eq "Wert")             |
| Postleitzahl                 | Zeichenfolge oder $null                                                                            | (user.postalCode - Eq "Wert")                             |
| preferredLanguage          | Code nach ISO 639-1                                                                                        | (user.preferredLanguage - Eq "En-US")                      |
| sipProxyAddress            | Zeichenfolge oder $null                                                                            | (user.sipProxyAddress - Eq "Wert")                        |
| Zustand                      | Zeichenfolge oder $null                                                                            | (user.state - Eq "Wert")                                  |
| streetAddress              | Zeichenfolge oder $null                                                                            | (user.streetAddress - Eq "Wert")                          |
| Nachname                    | Zeichenfolge oder $null                                                                            | (user.surname - Eq "Wert")                                |
| telephoneNumber            | Zeichenfolge oder $null                                                                            | (user.telephoneNumber - Eq "Wert")                        |
| usageLocation              | Zwei Buchstaben Ländercode                                                                           | (user.usageLocation - Eq "US")                             |
| userPrincipalName          | Eine Zeichenfolge                                                                                     | (user.userPrincipalName - eq"alias@domain")               |
| Benutzertyp                   | Mitglied Gast $null                                                                                    | (user.userType - Eq "Member")                              |

### <a name="properties-of-type-string-collection"></a>Eigenschaften des Typs String-Auflistung

Zulässige Operatoren

* -enthält


* -notContains

| Poperties      | Zulässige Werte                        | Verwendung                                                |
|----------------|---------------------------------------|------------------------------------------------------|
| otherMails     | Eine Zeichenfolge                      | (user.otherMails-enthält"alias@domain")           |
| proxyAddresses | SMTP: alias@domain smtp:alias@domain | (user.proxyAddresses-enthält "SMTP:alias@domain") |

## <a name="extension-attributes-and-custom-attributes"></a>Erweiterung und benutzerdefinierten Attributen
Erweiterung und benutzerdefinierten Attributen werden in dynamische Mitgliedschaftsregeln unterstützt.

Erweiterungsattribute aus lokal Windows Server Active Directory synchronisiert sind und haben das Format "ExtensionAttributeX", wobei X 1-15 entspricht.
Beispiel für eine Regel, die ein Erweiterungsattribut verwendet

(user.extensionAttribute15 - Eq "Marketing")

Benutzerdefinierte Attribute werden von Windows Server Active Directory lokal oder von einem verbundenen SaaS-Anwendung synchronisiert und das Format "user.extension_[GUID]\__ [Attribute]", wobei [GUID] der eindeutige Bezeichner in AAD für die Anwendung, die das Attribut AAD erstellt und [Attribute] ist der Name des Attributs, wie es erstellt wurde.
Ein Beispiel für eine Regel, die ein benutzerdefiniertes Attribut verwendet, wird

User.extension_c272a57b722d4eb29bfe327874ae79cb__OfficeNumber  

Der benutzerdefinierten Attributnamen finden Sie im Verzeichnis durch einen Benutzer Abfragen Attribut mit dem Diagramm-Explorer und der Attributname suchen.

## <a name="direct-reports-rule"></a>Mitarbeiter-Regel
Sie können nun basierend auf dem Manager-Attribut eines Benutzers Gruppe füllen.

**So konfigurieren Sie eine Gruppe als Gruppe "Manager"**

1. Führen Sie die Schritte 1 bis 5 in [die erweiterte Regel erstellen](#to-create-the-advanced-rule), und wählen Sie eine **Dynamische** **Mitgliedschaft** .

2. Geben Sie auf **dynamische Mitgliedschaftsregeln** Blade mit der folgenden Syntax:

    Direkte Berichte für *Mitarbeiter {ObectID_of_manager}*. Beispiel für eine gültige Regel für Mitarbeiter

                    Direct Reports for "62e19b97-8b3d-4d4a-a106-4ce66896a863”

    Wo ist "62e19b97-8b3d-4d4a-a106-4ce66896a863" die ObjectID des Managers. Die Objekt-ID in Azure AD auf der **Registerkarte Profil** der Seite für den Benutzer finden, die der Manager.

3. Beim Speichern dieser Regel werden alle Benutzer, die der Regel entsprechen als Mitglieder der Gruppe angehören. Es dauert einige Minuten für die Gruppe zunächst aufgefüllt.


## <a name="using-attributes-to-create-rules-for-device-objects"></a>Verwenden von Attributen zum Erstellen von Regeln für Gerät

Sie können auch eine Regel erstellen, die Geräteobjekte für die Mitgliedschaft in einer Gruppe auswählt. Die folgenden Gerätattribute können verwendet werden:

| Eigenschaften           | Zulässige Werte                  | Verwendung                                                |
|----------------------|---------------------------------|------------------------------------------------------|
| displayName          | eine Zeichenfolge                | (device.displayName - Eq "Rob Iphone")                 |
| deviceOSType         | eine Zeichenfolge                | (device.deviceOSType - Eq "IOS")                      |
| deviceOSVersion      | eine Zeichenfolge                | (Gerät. OSVersion - Eq "9.1")                         |
| isDirSynced          | True, false null                 | (device.isDirSynced - Eq "True")                      |
| isManaged            | True, false null                 | (device.isManaged - Eq "False")                       |
| isCompliant          | True, false null                 | (device.isCompliant - Eq "True")                      |


## <a name="additional-information"></a>Weitere Informationen
Diese Artikel bieten zusätzliche Informationen zu Gruppen in Active Directory Azure.

* [Vorhandene Gruppen](active-directory-groups-view-azure-portal.md)
* [Erstellen einer neuen Gruppe und Hinzufügen von Mitgliedern](active-directory-groups-create-azure-portal.md)
* [Verwalten einer Gruppe](active-directory-groups-settings-azure-portal.md)
* [Verwalten von Mitgliedschaften einer Gruppe](active-directory-groups-membership-azure-portal.md)
* [Dynamische Regeln für Benutzer in einer Gruppe verwalten](active-directory-groups-dynamic-membership-azure-portal.md)
