<properties
    pageTitle="Azure AD Connect: Problembehandlung während der Synchronisierung | Microsoft Azure"
    description="Fehler während der Synchronisierung mit Azure AD Connect Problembehandlung erläutert."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="troubleshooting-errors-during-synchronization"></a>Problembehandlung bei Fehlern während der Synchronisierung
Fehler können auftreten, wenn Daten in Azure Active Directory (Azure AD) von Windows Server Active Directory (AD DS) synchronisiert ist. Dieser Artikel bietet eine Übersicht über verschiedene Synchronisierungsfehler einige Szenarien, die diesen Fehler und Möglichkeiten, die Fehler zu beheben. Dieser Artikel kann enthält die allgemeine Fehler und nicht alle möglichen Fehler.

 Vorausgesetzt der Reader kennt das zugrunde liegende [Konzepte von Azure AD und Azure AD Connect](active-directory-aadconnect-design-concepts.md).

Mit der neuesten Version von Azure AD Connect \(August 2016 oder höher\), Bericht Fehler steht in den [Azure-Portal](https://aka.ms/aadconnecthealth) als Teil des Azure AD Verbindung synchronisieren.


Ab 1. September 2016 [Azure Active Directory doppelte Attribut Resiliency](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) -Feature für alle *neuen* Azure Active Directory Mieter standardmäßig aktiviert. Diese Funktion wird automatisch in den nächsten Monaten für bestehende Mieter aktiviert.

Azure AD Connect führt 3 Arten von Operationen aus den Verzeichnissen synchron hält: Import und Synchronisierung exportieren. Fehler möglich bei allen Operationen. Dieser Artikel konzentriert sich hauptsächlich auf Fehler beim Exportieren in Azure AD.

## <a name="errors-during-export-to-azure-ad"></a>Fehler beim Exportieren in Azure AD
Abschnitt beschreibt verschiedene Fehler, die während des Exportvorgangs in Azure AD mit Azure AD Connector auftreten können. Dieser Connector kann durch das Format "Contoso. wird identifiziert werden *onmicrosoft.com*".
Fehler beim Exportieren in Azure AD an, die die Operation \(hinzufügen, aktualisieren und löschen usw.\) versucht Azure AD Connect \(Synchronisierungsmodul\) Azure Active Directory ist fehlgeschlagen.

![Fehler Übersicht über](.\media\active-directory-aadconnect-troubleshoot-sync-errors\Export_Errors_Overview_01.png)

## <a name="data-mismatch-errors"></a>Missverhältnis Datenfehlern
### <a name="invalidsoftmatch"></a>InvalidSoftMatch
#### <a name="description"></a>Beschreibung
- Beim Verbinden von Azure AD \(Synchronisierungsmodul\) weist Azure Active Directory hinzufügen oder Aktualisieren von Objekten, Azure AD eingehende Objekt in Azure AD mit dem Attribut **SourceAnchor** auf das **ImmutableId** -Attribut des Objekts entspricht. Dieses Spiel heißt eine **Festplatte übereinstimmen**.
- Bei Azure AD **nicht findet** jedes Objekt, das dem **ImmutableId** entspricht-Attribut mit dem **SourceAnchor** -Attribut des eingehenden Objekts vor der Bereitstellung eines neuen Objekts fällt es wieder um ProxyAddresses und UserPrincipalName Attribute verwenden, um eine Übereinstimmung zu finden. Dieses Spiel heißt eine **Weiche übereinstimmen**. Weiche übereinstimmen soll Objekte bereits in Azure AD (die in Azure AD stammen) mit den neuen Objekten wird während der Synchronisierung hinzugefügt/aktualisiert übereinstimmen, die dieselbe Entität (Benutzer, Gruppen) lokal darstellen.
- **InvalidSoftMatch** Fehler beim harte Entsprechung findet alle übereinstimmenden Objekts übereinstimmen **und** weiche ein entsprechendes Objekt findet, aber das Objekt hat einen anderen Wert *ImmutableId* als eingehende Objekt *SourceAnchor*, vorschlägt, dass das entsprechende Objekt mit einem anderen Objekt aus Active Directory lokal synchronisiert wurde.

In anderen Worten nacheinander soft Übereinstimmung zu müssen Objekt mit weichen abgeglichen werden nicht Wert *ImmutableId*. Wenn ein Objekt mit *ImmutableId* mit Wert andernfalls die harte Entsprechung aber zufrieden Soft Übereinstimmungskriterien der Vorgang in eine InvalidSoftMatch-Synchronisationsfehler ergeben würde.

Azure Active Directory-Schema lässt nicht zwei oder mehr Objekte den gleichen Wert der folgenden Attribute. \(Dies ist keine vollständige Liste.\)

- ProxyAddresses
- UserPrincipalName
- onPremisesSecurityIdentifier
- ObjectId

>[AZURE.NOTE] [AD-Attribut Azure Duplikat Attribut Resiliency](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) -Feature wird auch als Standardverhalten von Azure Active Directory eingeführt.  Dies reduziert die Anzahl der Synchronisationsfehler Azure AD Connect (sowie anderen Sync) mit Azure AD ausfallsicherer die Art duplizierten ProxyAddresses UserPrincipalName Attribute und im lokalen Active Directory-Umgebung vorhanden. Diese Funktion wird nicht die Duplizierung Fehler beheben. So müssen die Daten festgesetzt. Aber es ermöglicht die Bereitstellung von neuen Objekten, die andernfalls blockiert durch doppelte Werte in Azure Active Directory bereitgestellt werden. Dies verringert auch die Anzahl der Fehler an den Synchronisationsclient zurückgegeben.
Wenn diese Funktion für Ihren Mandanten aktiviert ist, sehen Sie nicht Synchronisierungsfehler InvalidSoftMatch während der Bereitstellung neuer Objekte angezeigt.


#### <a name="example-scenarios-for-invalidsoftmatch"></a>Beispielszenarien für InvalidSoftMatch
1. Mindestens zwei Objekte mit demselben Wert ProxyAddresses-Attribut ist in Active Directory lokal vorhanden. Nur eine wird in Azure AD bereitgestellt bekommen.
2. Mindestens zwei Objekte mit demselben Wert UserPrincipalName existiert Active Directory vor. Nur eine wird in Azure AD bereitgestellt bekommen.
3. Ein Objekt wurde in den in Active Directory mit dem gleichen Wert ProxyAddresses-Attribut, ein vorhandenes Objekt in Azure Active Directory hinzugefügt. Das Objekt lokal hinzugefügt wird nicht in Azure Active Directory bereitgestellt bekommen.
4. Ein Objekt wurde in lokal Active Directory mit dem gleichen Wert Attribut UserPrincipalName, ein Konto in Azure Active Directory hinzugefügt. Das Objekt ist nicht in Azure Active Directory bereitgestellt bekommen.
5. Synchronisierte Konto aus Gesamtstruktur verschoben wurde eine Gesamtstruktur b Azure AD verbinden (Synchronisierungsmodul) verwendete Attribut ObjectGUID der SourceAnchor berechnet. Nach dem Verschieben Gesamtstruktur ist der Wert der SourceAnchor andere. Das neue Objekt (A) nicht das vorhandene Objekt in Azure AD synchronisieren.
6. Ein synchronisiertes Objekt wurde versehentlich aus Active Directory und ein neues Objekt wurde in Active Directory für dieselbe Entität (z. B. Benutzer) das Benutzerkonto in Active Directory Azure lokal gelöscht. Das neue Konto nicht mit dem vorhandenen Azure AD-Objekt.
7. Azure AD Connect wurde deinstalliert und neu installiert. Ein anderes Attribut wurde während der Neuinstallation der SourceAnchor gewählt. Alle Objekte, die bereits synchronisiert wurde beendet die Synchronisierung mit InvalidSoftMatch Fehler.

#### <a name="example-case"></a>Fallbeispiel:
1. **Bob Smith** ist ein synchronisiertes Benutzer in Azure Active Directory lokal Active Directory *contoso.com*
2. Bob Smith **UserPrincipalName** festzulegen, als **bobs@contoso.com**.
3. **"Abcdefghijklmnopqrstuv =="** wird berechnet, indem Azure AD Verbinden mit Bob Smith **ObjectGUID** ab **SourceAnchor** lokale Active Directory **ImmutableId** für Bob Smith in Azure Active Directory ist.
4. Außerdem hat Bob folgende Werte für das Attribut **ProxyAddresses** :
    - smtp:bobs@contoso.com
    - smtp:bob.smith@contoso.com
    - **smtp:bob@contoso.com**
5.  Ein neuer Benutzer **Bob Taylor**wird am lokalen Active Directory hinzugefügt.
6. Bob Taylor **UserPrincipalName** festzulegen, als **bobt@contoso.com**.
7. **"abcdefghijkl0123456789 ==" "** wird berechnet, indem Azure AD Verbinden mit Bob Taylor **ObjectGUID** ab **SourceAnchor** lokale Active Directory. Bob Taylor Objekt wurde noch nicht in Azure Active Directory synchronisiert.
8. Bob Taylor besitzt die folgenden Werte für das Attribut proxyAddresses
    - smtp:bobt@contoso.com
    - smtp:bob.taylor@contoso.com
    - **smtp:bob@contoso.com**
9. Während der Synchronisierung Azure AD Connect erkennt zusätzlich Bob Taylor im lokalen Active Directory und Azure AD Änderung zu bitten.
10. Azure AD führt zunächst harte Entsprechung. Also sucht ist ein Objekt mit dem ImmutableId gleich "abcdefghijkl0123456789 ==". Harte Entsprechung fehl wie kein anderes Objekt in Azure AD, ImmutableId.
11. Azure AD versucht Soft-Bob Taylor Übereinstimmung. Also wird gesucht wird jedes Objekt mit ProxyAddresses gleich drei Werte,smtp:bob@contoso.com
12. Azure AD finden Bob Smith Objekt Soft Übereinstimmungskriterien übereinstimmen. Dieses Objekt hat aber den Wert ImmutableId = "Abcdefghijklmnopqrstuv ==". wodurch dieses Objekt wurde von einem anderen Objekt aus Active Directory lokal synchronisiert. Folglich Azure AD Soft-Übereinstimmung kann nicht diese Objekte und führt ein Synchronisierungsfehler **InvalidSoftMatch** .

#### <a name="how-to-fix-invalidsoftmatch-error"></a>Wie Fehler InvalidSoftMatch
Die häufigste Grund für den Fehler InvalidSoftMatch ist zwei Objekte mit anderen SourceAnchor \(ImmutableId\) haben den gleichen Wert ProxyAddresses oder UserPrincipalName-Attribute, die während der weichen Übereinstimmung auf Azure AD verwendet werden. Um die ungültige Soft Übereinstimmung beheben

1.  Identifizieren Sie doppelter ProxyAddresses, UserPrincipalName oder anderen Attributwert, der den Fehler verursacht. Außerdem bestimmt, die zwei \(oder mehrere\) Objekten der Konflikt beteiligt sind. Der Bericht durch [Azure AD Connect Health Synchronisierung](https://aka.ms/aadchsyncerrors) hilft Ihnen zwei Objekte identifizieren.
2. Welches Objekt den doppelten Wert weiterhin und welches Objekt nicht identifizieren.
3. Das Objekt, das keinen Wert aufheben Sie den duplizierten Wert. Beachten Sie, dass Sie im Verzeichnis ändern sollten, in dem das Objekt von stammt. In einigen Fällen müssen Sie eines der Objekte in Konflikt löschen.
4. Sollten Sie die Änderung in den auf AD Azure AD Connect Änderung synchronisiert.

Beachten Sie, dass Fehlerbericht in Azure AD Connect Health Synchronisierung synchronisieren alle 30 Minuten aktualisiert wird und der neuesten Synchronisierungsversuch Fehler enthält.

>[AZURE.NOTE] ImmutableId, sollten nicht per Definition in der Lebensdauer des Objekts ändern. Wenn Azure AD Verbinden mit Szenarien Beachten Sie oben nicht konfiguriert wurde, Sie könnten am Ende in einer Situation, in Azure AD Connect berechnet verschiedene SourceAnchor für AD-Objekt, das die gleiche darstellt, Entität (dieselbe Gruppe/Benutzer/Kontakt usw.), ein vorhandenes Azure AD-Objekt enthält, die Sie weiterhin verwenden möchten.

#### <a name="related-articles"></a>Verwandte Artikel
- [Doppelte oder ungültige Attribute verhindern Verzeichnissynchronisation in Office 365](https://support.microsoft.com/en-us/kb/2647098)

### <a name="objecttypemismatch"></a>ObjectTypeMismatch
#### <a name="description"></a>Beschreibung
Azure AD versucht, zwei Objekte soft übereinstimmen, es ist möglich, dass zwei Objekte verschiedenen "" (z. B. Benutzer, Gruppe, Kontakt usw. Objekttyp) haben dieselben Werte für die Attribute die weiche Übereinstimmung ausgeführt. Als Duplikate dieser Attribute in Azure AD nicht zulässig ist, kann der Vorgang "ObjectTypeMismatch" Synchronisierungsfehler führen.

#### <a name="example-scenarios-for-objecttypemismatch-error"></a>Beispielszenarien für ObjectTypeMismatch-Fehler
- E-Mail-aktivierten Gruppe wird in Office 365 erstellt. Administrator fügt einen neuen Benutzer oder Kontakt in AD (die noch Azure AD nicht synchronisiert) lokal mit dem gleichen Wert für das Attribut ProxyAddresses, Office 365-Gruppe.

#### <a name="example-case"></a>Fallbeispiel

1. Admin erstellt eine neue e-Mail-aktivierten Sicherheitsgruppe in Office 365 für die Abteilung und bietet eine e-Mail-Adresse als tax@contoso.com. Dies weist das Attribut ProxyAddresses für diese Gruppe mit dem Wert**smtp:tax@contoso.com**
2. Ein neuer Benutzer Contoso.com verbindet und ein Konto für den Benutzer mit ProxyAddress als erstellt**smtp:tax@contoso.com**
3. Wenn Azure AD Verbinden des neuen Benutzerkontos synchronisiert wird, erhalten sie den Fehler "ObjectTypeMismatch".

#### <a name="how-to-fix-objecttypemismatch-error"></a>Wie Fehler ObjectTypeMismatch
Die häufigste Grund für den Fehler ObjectTypeMismatch ist zwei Objekte unterschiedlichen Typs (Benutzer, Gruppe, Kontakt usw.) den gleichen Wert für das Attribut ProxyAddresses haben. Um die ObjectTypeMismatch zu beheben:

1.  Identifizieren doppelter ProxyAddresses (oder andere Attribute)-Wert, der den Fehler verursacht. Außerdem bestimmt, die zwei \(oder mehrere\) Objekten der Konflikt beteiligt sind. Der Bericht durch [Azure AD Connect Health Synchronisierung](https://aka.ms/aadchsyncerrors) hilft Ihnen zwei Objekte identifizieren.
2. Welches Objekt den doppelten Wert weiterhin und welches Objekt nicht identifizieren.
3. Das Objekt, das keinen Wert aufheben Sie den duplizierten Wert. Beachten Sie, dass Sie im Verzeichnis ändern sollten, in dem das Objekt von stammt. In einigen Fällen müssen Sie eines der Objekte in Konflikt löschen.
4. Sollten Sie die Änderung in den auf AD Azure AD Connect Änderung synchronisiert. Fehlerbericht in Azure AD Connect Health Synchronisierung synchronisieren alle 30 Minuten aktualisiert und der neuesten Synchronisierungsversuch Fehler enthält.


## <a name="duplicate-attributes"></a>Doppelte Attribute
### <a name="attributevaluemustbeunique"></a>AttributeValueMustBeUnique
#### <a name="description"></a>Beschreibung
Azure Active Directory-Schema lässt nicht zwei oder mehr Objekte den gleichen Wert der folgenden Attribute. Das bedeutet, dass jedes Objekt in Azure AD gezwungen ist, zu einer bestimmten Instanz einen eindeutigen Wert dieser Attribute haben.

- ProxyAddresses
- UserPrincipalName

Azure AD-Verbindung versucht, ein neues Objekt hinzufügen oder Aktualisieren eines vorhandenen Objekts mit Werten für diese Attribute, die bereits mit einem anderen Objekt in Azure Active Directory zugewiesen ist, führt der Vorgang zu Synchronisierungsfehler "AttributeValueMustBeUnique".
#### <a name="possible-scenarios"></a>Mögliche Szenarien:
1. Doppelter Wert wird Objekteigenschaften bereits synchronisiert zugewiesen steht in mit einem anderen Objekt synchronisiert Konflikt.

#### <a name="example-case"></a>Fallbeispiel:
1. **Bob Smith** ist ein synchronisiertes Benutzer in Azure Active Directory lokal Active Directory contoso.com
2. Bob Smith **UserPrincipalName** lokal festgelegt ist, als **bobs@contoso.com**.
3. Außerdem hat Bob folgende Werte für das Attribut **ProxyAddresses** :
    - smtp:bobs@contoso.com
    - smtp:bob.smith@contoso.com
    - **smtp:bob@contoso.com**
4. Ein neuer Benutzer **Bob Taylor**wird am lokalen Active Directory hinzugefügt.
5. Bob Taylor **UserPrincipalName** festzulegen, als **bobt@contoso.com**.
6. **Bob Taylor** hat die folgenden Werte für das Attribut **ProxyAddresses** i. smtp:bobt@contoso.comII. smtp:bob.taylor@contoso.com
7. Bob Taylor-Objekt mit Azure erfolgreich synchronisiert.
8. Admin mit dem folgenden Attribut **ProxyAddresses** Bob Taylor aktualisieren möchte: ich. **smtp:bob@contoso.com**
9. Azure AD versucht Bob Taylor Objekt in Azure AD mit den oben genannten Wert zu aktualisieren, jedoch als fehlschlagen, wird Wert ProxyAddresses bereits Bob Smith zugewiesen "AttributeValueMustBeUnique" angezeigt.

#### <a name="how-to-fix-attributevaluemustbeunique-error"></a>Wie Fehler AttributeValueMustBeUnique
Die häufigste Grund für den Fehler AttributeValueMustBeUnique ist zwei Objekte mit anderen SourceAnchor \(ImmutableId\) denselben Wert ProxyAddresses oder UserPrincipalName Attribute. Um Fehler AttributeValueMustBeUnique

1.  Identifizieren Sie doppelter ProxyAddresses, UserPrincipalName oder anderen Attributwert, der den Fehler verursacht. Außerdem bestimmt, die zwei \(oder mehrere\) Objekten der Konflikt beteiligt sind. Der Bericht durch [Azure AD Connect Health Synchronisierung](https://aka.ms/aadchsyncerrors) hilft Ihnen zwei Objekte identifizieren.
2. Welches Objekt den doppelten Wert weiterhin und welches Objekt nicht identifizieren.
3. Das Objekt, das keinen Wert aufheben Sie den duplizierten Wert. Beachten Sie, dass Sie im Verzeichnis ändern sollten, in dem das Objekt von stammt. In einigen Fällen müssen Sie eines der Objekte in Konflikt löschen.
4. Sollten Sie die Änderung in den auf AD Azure AD Connect synchronisiert die Änderung der Fehler behoben.

#### <a name="related-articles"></a>Verwandte Artikel
-[Doppelte oder ungültige Attribute verhindern Verzeichnissynchronisation in Office 365](https://support.microsoft.com/en-us/kb/2647098)


## <a name="data-validation-failures"></a>Überprüfungsfehler Daten
### <a name="identitydatavalidationfailed"></a>IdentityDataValidationFailed
#### <a name="description"></a>Beschreibung
Azure Active Directory erzwingt verschiedene Beschränkungen die Daten vor, Daten in das Verzeichnis geschrieben werden. Dies ist sicherzustellen, dass Benutzer die besten Erfahrungen während der Anwendung, die auf diesen Daten abhängen.
#### <a name="scenarios"></a>Szenarien
ein. UserPrincipalName Attributwert ist ungültig oder nicht unterstütztes Zeichen.
b. Das Attribut UserPrincipalName folgt nicht das erforderliche Format.
#### <a name="how-to-fix-identitydatavalidationfailed-error"></a>Wie Fehler IdentityDataValidationFailed

ein. Sicherstellen Sie, dass das Attribut UserPrincipalName Zeichen und erforderliche Format unterstützt.

#### <a name="related-articles"></a>Verwandte Artikel
- [Vorbereiten der Benutzer mittels Verzeichnissynchronisation zu Office 365 bereitgestellt]( https://support.office.com/en-us/article/Prepare-to-provision-users-through-directory-synchronization-to-Office-365-01920974-9e6f-4331-a370-13aea4e82b3e)


### <a name="datavalidationfailed"></a>DataValidationFailed
#### <a name="description"></a>Beschreibung
Dies ist ein sehr speziellen Fall ein Synchronisierungsfehler **"DataValidationFailed"** ergibt, wenn das Suffix des Benutzers UserPrincipalName einer föderierten Domäne in eine andere föderierten Domäne geändert wird.

#### <a name="scenarios"></a>Szenarien
Für einen Benutzer synchronisiert wurde UserPrincipalName Suffix einer föderierten Domäne in eine andere föderierten Domäne lokal geändert. Beispielsweise *UserPrincipalName = bob@contoso.com * wurde *UserPrincipalName = bob@fabrikam.com *.

#### <a name="example"></a>Beispiel
1. Bob Smith, ein Konto für Contoso.com wird als neuer Benutzer in Active Directory mit UserPrincipalName hinzugefügt.bob@contoso.com
2. Bob in einem anderen Abschnitt namens Fabrikam.com contoso.com verschoben und seine UserPrincipalName geändertbob@fabrikam.com
3. Contoso.com und fabrikam.com Domänen sind Föderationen mit Azure Active Directory.
4. Bobs UserPrincipalName wird nicht aktualisiert und führt zu einem Synchronisierungsfehler "DataValidationFailed".

#### <a name="how-to-fix"></a>Wie beheben
Wenn ein Benutzer UserPrincipalName Suffix aus aktualisiert wurde bob@ **contoso.com** , bob@ **fabrikam.com**, wo **contoso.com** und **fabrikam.com** **Föderationen**sind, führen Sie die so die Synchronisierungsfehler beheben

1. Aktualisieren des Benutzers UserPrincipalName in Azure AD aus bob@contoso.com , bob@contoso.onmicrosoft.com. Den folgenden PowerShell-Befehl können Sie mit dem Azure AD PowerShell-Modul:`Set-MsolUserPrincipalName -UserPrincipalName bob@contoso.com -NewUserPrincipalName bob@contoso.onmicrosoft.com`

2. Lassen Sie beim nächsten Zyklus synchronisieren Synchronisation versuchen zu Werden diese Synchronisierung und aktualisiert die UserPrincipalName von Bob bob@fabrikam.com wie erwartet.


## <a name="largeobject"></a>LargeObject
### <a name="description"></a>Beschreibung
Überschreitet ein Attribut zulässige Größe, Beschränkung oder maximale Anzahl von Azure Active Directory-Schema festlegen, führt der Synchronisierungsvorgang Synchronisierungsfehler **LargeObject** oder **ExceededAllowedLength** . In der Regel tritt dieser Fehler die folgenden Attribute

- userCertificate
- thumbnailPhoto
- proxyAddresses

### <a name="possible-scenarios"></a>Mögliche Szenarien
1. Bobs UserCertificate-Attribut speichern viele Zertifikate Bob zugewiesen. Dazu gehören ältere, abgelaufene Zertifikate.
2. Bob ThmubnailPhoto in Active Directory festgelegt ist groß in Azure Active Directory synchronisiert werden.
3. Während der automatische Auffüllung des ProxyAddresses-Attribut in Active Directory ein Objekt zugewiesen haben > 500 ProxyAddresses.

### <a name="how-to-fix"></a>Wie beheben

1. Sicherstellen Sie, dass das Attribut, das den Fehler verursacht in zulässige Beschränkung.

## <a name="related-links"></a>Verwandte links
- [Suchen Sie Active Directory-Objekte in Active Directory Administrative Center] (https://technet.microsoft.com/library/dd560661.aspx)
- [Wie ein Objekt mithilfe von Azure Active Directory PowerShell Azure Active Directory abgefragt](https://msdn.microsoft.com/library/azure/jj151815.aspx)
