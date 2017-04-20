<properties
    pageTitle="Identität Synchronisierung und doppelte Attribut Stabilität | Microsoft Azure"
    description="Neues Verhalten zur Behandlung Objekte UPN oder ProxyAddress Konflikte während der Directory-Synchronisierung mit Azure AD verbinden."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="markusvi"/>



# <a name="identity-synchronization-and-duplicate-attribute-resiliency"></a>Identität Synchronisierung und doppelte Attribut Stabilität
Doppelte Attribut Stabilität ist ein Feature in Azure Active Directory, die Reibung **UserPrincipalName** und **ProxyAddress** Konflikte beim Ausführen eines Microsoft Synchronisierungsprogramme eliminieren.

Diese zwei Attribute sind im Allgemeinen erforderlich, für alle **Benutzer**, **Gruppe**oder **Kontakt** Objekte in einem bestimmten Mandanten Azure Active Directory eindeutig sein.

> [AZURE.NOTE] Nur Benutzer können UPNs haben.

Das neue Verhalten dieses Leistungsmerkmal im Bereich Cloud Sync-Pipeline, deshalb Client unabhängig und für jedes Produkt von Microsoft Synchronisation einschließlich Azure AD verbinden, Dirsync-MIM + Anschluss. Der allgemeine Begriff "Sync Client" wird in diesem Dokument zur Darstellung eines dieser Produkte verwendet.

## <a name="current-behavior"></a>Aktuelles Verhalten
Bei Versuch, ein neues Objekt mit einem Benutzerprinzipalnamen oder ProxyAddress bereitstellen, die diese Eindeutigkeits-Integritätsregel verletzt wird blockiert Azure Active Directory das Objekt erstellt wird. Auch wenn ein Objekt mit einer nicht eindeutigen UPN oder ProxyAddress aktualisiert wird, schlägt die Aktualisierung. Der Bereitstellungsversuch oder Update Sync Client auf jedem Zyklus exportieren wiederholt, und weiterhin fehl, bis der Konflikt behoben ist. E-Mail Bericht Fehler bei jedem Versuch generiert und Sync Client ein Fehler protokolliert.

## <a name="behavior-with-duplicate-attribute-resiliency"></a>Doppeltes Attribut Resiliency Verhalten
Statt vollständig andernfalls bereitstellen oder ein Objekt mit zwei Attribute aktualisieren isoliert"Azure Active Directory" doppelte Attribut, das Eindeutigkeits-Integritätsregel verletzen würde. Wenn dieses Attribut für die Bereitstellung wie UserPrincipalName, weist der Dienst einen Platzhalterwert. Das Format dieser temporäre Werte  
"***<OriginalPrefix>+<4DigitNumber>@<InitialTenantDomain>. onmicrosoft.com***".  
Wenn das Attribut nicht wie **ProxyAddress**erforderlich ist, wird Azure Active Directory einfach Attributs Konflikt isoliert und fährt mit der Erstellung oder Aktualisierung.

Informationen über den Konflikt wird auf das Attribut isolieren, die gleichen Fehler e-Mail-Bericht verwendet das alte Verhalten gesendet. Jedoch diese Informationen wird nur im Bericht einmal geschieht die Quarantäne es nicht weiter in zukünftige e-Mails protokolliert werden. Da Export für dieses Objekt gelungen Sync Client protokolliert keine Fehler auch erstellen nicht wiederholen / Aktualisierungsvorgang auf nachfolgende Synchronisierung Zyklen.

Um dieses Verhalten zu unterstützen wurde ein neues Attribut den Benutzer-, Gruppen- und Kontakt Objektklassen hinzugefügt:  
**DirSyncProvisioningErrors**

Dies ist ein mehrwertiges Attribut mit in Konflikt stehenden Attribute gespeichert, die Eindeutigkeits-Integritätsregel verletzen würde normalerweise hinzugefügt werden soll. Eine Hintergrundaufgabe Zeitgeber wurde in Azure Active Directory aktiviert, der stündlich doppeltes Attributkonflikte gesucht, die behoben wurden und die entsprechenden Attribute automatisch aus der Quarantäne entfernt wird.

### <a name="enabling-duplicate-attribute-resiliency"></a>Doppeltes Attribut Resiliency aktivieren
Doppelte Attribut Stabilität werden das neue Standardverhalten über alle Azure Active Directory Mieter. Es werden standardmäßig für alle, die Synchronisierung zum ersten Mal am 22. August 2016 oder höher aktiviert. Mandanten, die Synchronisierung vor diesem Datum aktiviert werden die stapelweise aktiviert haben. Diese Einführung im September 2016 beginnt und eine e-Mail-Benachrichtigung erhalten des Mieters Tech an einem bestimmten Datum bei aktiviertem.

Doppelte Attribut Resiliency eingeschaltet kann nicht deaktiviert werden.

Prüfen, ob das Feature für Ihren Mandanten aktiviert ist, können Sie dazu die neueste Version des Moduls Azure Active Directory PowerShell herunterladen und ausführen:

`Get-MsolDirSyncFeatures -Feature DuplicateUPNResiliency`

`Get-MsolDirSyncFeatures -Feature DuplicateProxyAddressResiliency`

Möchten Sie vorbeugend das Feature aktivieren, bevor es für Ihren Mandanten eingeschaltet ist, möchten die aktuelle Version des Moduls Azure Active Directory PowerShell herunterladen und ausführen:

`Set-MsolDirSyncFeature -Feature DuplicateUPNResiliency -Enable $true`

`Set-MsolDirSyncFeature -Feature DuplicateProxyAddressResiliency -Enable $true`

## <a name="identifying-objects-with-dirsyncprovisioningerrors"></a>Identifizieren von Objekten mit DirSyncProvisioningErrors
Es gibt derzeit zwei Objekte identifizieren, die diese Fehler aufgrund doppelter Eigenschaft Azure Active Directory PowerShell und dem Verwaltungsportal von Office 365. Sollen weitere Portal basierte Berichterstattung in Zukunft erweitern.

### <a name="azure-active-directory-powershell"></a>Azure Active Directory PowerShell
In diesem Thema PowerShell-Cmdlets gilt Folgendes:

- Alle folgenden Cmdlets Groß-/Kleinschreibung.
- **ErrorCategory-PropertyConflict** sein enthalten. Zurzeit keine andere **ErrorCategory**, aber dies in Zukunft erweitert werden kann.

Erstens zunächst **Verbinden MsolService** ausführen und Anmeldeinformationen für eine-Mandantenadministrators eingeben.

Anschließend verwenden Sie die folgenden Cmdlets und Operatoren Fehler auf verschiedene Arten anzeigen:

1. [Alle](#see-all)

2. [Vom Typ](#by-property-type)

3. [In Konflikt stehende Wert](#by-conflicting-value)

4. [Mithilfe einer Zeichenfolge suchen](#using-a-string-search)

5. [Sortiert](#sorted)

6. [In einer begrenzten Menge oder alle](#in-a-limited-quantity-or-all)


#### <a name="see-all"></a>Alle
Sobald verbunden, ausführen, um eine allgemeine Übersicht Attribut Bereitstellung Fehler im Mandanten:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict`

Ein Ergebnis wie folgt:  
 ![MsolDirSyncProvisioningError abrufen](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/1.png "Get-MsolDirSyncProvisioningError")  


#### <a name="by-property-type"></a>Vom Typ
Fehler vom Typ finden kennzeichnen Sie die **PropertyName -** Argument **UserPrincipalName** oder **ProxyAddresses** :

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName UserPrincipalName`

Oder

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName ProxyAddresses`

#### <a name="by-conflicting-value"></a>In Konflikt stehende Wert
Um Fehler auf eine bestimmte Eigenschaft **PropertyValue -** Flag (**PropertyName -** muss auch verwendet werden, wenn dieses Kennzeichen hinzufügen) hinzufügen:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyValue User@domain.com -PropertyName UserPrincipalName`


#### <a name="using-a-string-search"></a>Mithilfe einer Zeichenfolge suchen
Umfassende Zeichenfolgensuche verwenden **SearchString -** Flag. Dies kann unabhängig verwendet werden aller oben Flags außer **-ErrorCategory PropertyConflict**, der immer erforderlich ist:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -SearchString User`

#### <a name="in-a-limited-quantity-or-all"></a>In einer begrenzten Menge oder alle
1. **MaxResults <Int> ** wird die Abfrage auf bestimmte Werte zu beschränken.

2. **Alle** kann verwendet werden um sicherzustellen, dass alle Ergebnisse bei abgerufen werden, die eine große Anzahl von Fehlern vorhanden ist.

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -MaxResults 5`

## <a name="office-365-admin-portal"></a>Verwaltungsportal von Office 365

Sie können Synchronisierungsfehler Verzeichnis in Office 365-Verwaltungskonsole anzeigen. Bericht in Office 365-Portal zeigt nur **Benutzerobjekte, die diesen Fehler aufweisen** . Es zeigt Informationen zu Konflikten zwischen **Gruppen** und **Kontakte**.


![Aktive Benutzer] (./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/1234.png "Aktive Benutzer")

Anleitung zum Verzeichnis Synchronisierungsfehler in Office 365 Administrationscenter anzeigen finden Sie unter [identifizieren Verzeichnis Synchronisierungsfehler in Office 365](https://support.office.com/en-us/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067).


### <a name="identity-synchronization-error-report"></a>Identität Synchronisierungsbericht
Wenn ein Objekt mit einer doppelten Attributkonflikt behandelt dieses neue Verhalten ist eine Benachrichtigung standard Identität Synchronisierung Fehlerbericht e-Mails, die an die technische Mitteilung im Kontakt für den Mandanten. Es ist jedoch eine wichtige Änderung in diesem Verhalten. In der Vergangenheit würden Informationen über ein doppeltes Attributkonflikt in jedem nachfolgenden Bericht einbezogen werden, bis der Konflikt behoben wurde. Mit diesem neuen Verhalten erscheint Fehlermeldung für einen Konflikt nur einmal gleichzeitig in Konflikt stehendes Attribut isoliert wird.

Hier ist ein Beispiel für die e-Mail-Benachrichtigung zu einem Konflikt ProxyAddress sieht:  
    ![Aktive Benutzer](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/6.png "Active Users")  

## <a name="resolving-conflicts"></a>Auflösen von Konflikten
Problembehandlung bei Strategie und Auflösung Taktiken für diese Fehler sollte nicht wie anders Doppeltes Attribut Fehler in der Vergangenheit behandelt wurden. Der einzige Unterschied ist, dass Zeitgeber Aufgabe durch Mieter auf der Dienstseite das betreffende Attribut auf das richtige Objekt automatisch hinzugefügt führt, sobald der Konflikt behoben ist.

Der folgende Artikel veranschaulicht verschiedene Strategien zur Problembehandlung und Lösung: [doppelte oder ungültige Attribute Verzeichnissynchronisation in Office 365 zu verhindern](https://support.microsoft.com/kb/2647098).

## <a name="known-issues"></a>Bekannte Probleme
Keines dieser Probleme wird Daten Verlust oder Service Leistungsabfall. Sind mehrere ästhetisch andere auslösen standard "*vor Resiliency*" Doppeltes Attribut Fehler statt isolieren Attributs Konflikt ausgelöst und anderen manuellen Fixup erfordern bestimmte Fehler verursacht.

**Core-Verhalten:**

1. Objekte mit bestimmten Attribut weiterhin exportfehler doppelte Attribute isoliert.  
Zum Beispiel:

    ein. Neuer Benutzer in Active Directory mit dem Benutzerprinzipalnamen erstellt **Joe@contoso.com** und ProxyAddress**smtp:Joe@contoso.com**

    b. Die Eigenschaften dieses Objekts in Konflikt mit einer vorhandenen Gruppe ProxyAddress ist **SMTP:Joe@contoso.com**.

    c. Beim Export wird statt Attribute Konflikt isolierten **ProxyAddress Konflikt** Fehler ausgelöst. Der Vorgang ist bei jeder nachfolgenden Sync-Zyklus wiederholt, wie es vor Resiliency-Feature aktiviert wurde.

2. Wenn zwei Gruppen mit der gleichen SMTP-Adresse lokal erstellt werden, kann eine Bereitstellung beim ersten Versuch mit einem doppelten **ProxyAddress** Standardfehler. Doppelte Wert wird jedoch bei der nächsten Synchronisierung Zyklus korrekt isoliert.

**Portal Berichten**:

1. Die ausführliche Fehlermeldung für zwei Objekte in einer Konflikt UPN entspricht. Dies bedeutet, dass sie beide ihre UPN geändert haben / isoliert, wenn tatsächlich nur eine geänderte Daten hatte.

2. Die ausführliche Fehlermeldung eines Konflikts UPN zeigt falsche DisplayName für einen Benutzer, der deren UPN geändert/isoliert hat. Zum Beispiel:

    ein. **Benutzer A** synchronisiert zuerst mit **UPN = User@contoso.com **.

    b. **Benutzer B** versucht, mit synchronisiert **UPN = User@contoso.com **.

    c. **Benutzer B** UPN geändert **User1234@contoso.onmicrosoft.com** und **User@contoso.com** **DirSyncProvisioningErrors**hinzugefügt.

    d. Die Fehlermeldung für **Benutzer B** anzugeben, dass **ein Benutzer** bereits **User@contoso.com** UPN, sondern zeigt **Den** eigenen DisplayName.



**Identität Synchronisierung Fehlerbericht**:

Die Verknüpfung wie *zum Beheben dieses Problems* ist falsch:  
    ![Aktive Benutzer](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/6.png "Active Users")  

Es sollte auf [https://aka.ms/duplicateattributeresiliency](https://aka.ms/duplicateattributeresiliency)verweisen.


## <a name="see-also"></a>Siehe auch

- [Azure AD verbinden synchronisieren](active-directory-aadconnectsync-whatis.md)

- [Integration von lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)

- [Directory Synchronization Fehler in Office 365](https://support.office.com/en-us/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067)
