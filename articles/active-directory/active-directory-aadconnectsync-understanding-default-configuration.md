<properties
    pageTitle="Azure AD Verbindung synchronisieren: Verstehen der Standardkonfiguration | Microsoft Azure"
    description="Dieser Artikel beschreibt die Standardkonfiguration in Azure AD-Verbindung synchronisieren."
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
    ms.date="09/01/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-sync-understanding-the-default-configuration"></a>Azure AD Connect-Synchronisierung: die Standardkonfiguration verstehen
Standardkonfiguration Regeln erläutert. Sie dokumentiert die Regeln und wie diese Regeln die Konfiguration auswirken. Er führt Sie auch durch die Standardkonfiguration von Azure AD-Verbindung synchronisieren. Ziel ist es, dass der Leser versteht wie Konfigurationsmodell namens deklarative Bereitstellung in einem Beispiel arbeitet. Es wird vorausgesetzt, dass bereits installiert haben und Azure AD Connect Synchronisierung mithilfe des Assistenten konfigurieren.

Die Details des Konfigurationsmodells lesen [Verstehen deklarative Bereitstellung](active-directory-aadconnectsync-understanding-declarative-provisioning.md)verstehen.

## <a name="out-of-box-rules-from-on-premises-to-azure-ad"></a>Out-of-Box-Regeln von lokalen Azure AD
Die folgenden Ausdrücke können in der Out-of-Box-Konfiguration gefunden.

### <a name="user-out-of-box-rules"></a>Benutzer von vordefinierten Regeln
Diese Regeln gelten auch für das iNetOrgPerson-Objekttyp.

Ein Benutzerobjekt muß folgende synchronisiert werden:

- Sie müssen eine SourceAnchor.
- Nach dem Erstellen des Objekts in Azure AD kann SourceAnchor nicht ändern. Wenn der Wert lokal geändert, beendet das Objekt synchronisiert, bis die SourceAnchor auf den vorherigen Wert geändert wird.
- Das Attribut AccountEnabled (UserAccountControl) muss ausgefüllt haben. Mit einem lokalen Active Directory ist dieses Attribut immer vorhanden und gefüllt.

Die folgenden Benutzer Objekte in Azure AD **nicht** synchronisiert:

- `IsPresent([isCriticalSystemObject])`. Viele Out-of-Box-Objekte in Active Directory wie das integrierte Administratorkonto sicherzustellen, werden nicht synchronisiert.
- `IsPresent([sAMAccountName]) = False`. Sicherstellen Sie, dass Benutzerobjekte ohne sAMAccountName-Attribut nicht synchronisiert werden. Dieser Fall wäre nur praktisch in einer Aktualisierung von NT4-Domäne.
- `Left([sAMAccountName], 4) = "AAD_"`, `Left([sAMAccountName], 5) = "MSOL_"`. Azure AD-Verbindung synchronisieren und früheren Versionen verwendete Dienstkonto nicht synchronisiert.
- Exchange-Konten, die nicht im Exchange Online funktioniert nicht synchronisiert.
    - `[sAMAccountName] = "SUPPORT_388945a0"`
    - `Left([mailNickname], 14) = "SystemMailbox{"`
    - `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`
    - `(Left([sAMAccountName], 4) = "CAS_" && (InStr([sAMAccountName], "}")> 0))`
- Objekte, die nicht im Exchange Online funktioniert nicht synchronisiert.
`CBool(IIF(IsPresent([msExchRecipientTypeDetails]),BitAnd([msExchRecipientTypeDetails],&H21C07000) > 0,NULL))`  
Diese Bitmaske (& H21C07000) würde den folgenden Objekten filtern:
    - E-Mail-aktivierten Öffentlichen Ordners
    - Das Postfach der Systemaufsicht
    - Postfachdatenbank-Postfach (Systempostfach)
    - Die universelle Sicherheitsgruppe (würde nicht für einen Benutzer anwenden, aber steht aus)
    - Nicht-universelle Gruppe (würde nicht für einen Benutzer anwenden, aber steht aus)
    - Postfachplan
    - Discovery-Postfach
- `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Alle Replikationsobjekte Opfer nicht synchronisiert.

-Attribut Folgendes gelten:

- `sourceAnchor <- IIF([msExchRecipientTypeDetails]=2,NULL,..)`. Das SourceAnchor-Attribut wird nicht von verknüpften Postfachs beigetragen. Es wird angenommen, ein verknüpftes Postfachs gefunden wurde, das aktuelle Konto später hinzugefügt wird.
- Exchange-bezogenen Attribute nur synchronisiert werden, wenn das Attribut **MailNickName** Wert.
- Bei mehreren Gesamtstrukturen, sind Attribute in der folgenden Reihenfolge verarbeitet:
    1. Attribute anmelden (z. B. UserPrincipalName) werden aus der Gesamtstruktur ein aktiviertes Konto bereitgestellt.
    2. Attribute, die in einer Exchange-GAL (globale Adressliste) finden werden aus der Gesamtstruktur mit Exchange-Postfach bereitgestellt.
    3. Wenn kein Postfach gefunden werden, können diese Attribute aus jeder Gesamtstruktur stammen.
    4. Exchange bezogenen Attribute (Technische Attribute nicht in der GAL angezeigt) werden aus der Gesamtstruktur bereitgestellt, `mailNickname ISNOTNULL`.
    5. Sind mehrere Gesamtstrukturen, die diese Regeln erfüllen, dient der Erstellungsreihenfolge (Datum/Uhrzeit) der Connectors (Gesamtstrukturen) bestimmen, welche Gesamtstruktur Attribute beiträgt.

### <a name="contact-out-of-box-rules"></a>Kontakt Regeln
Ein Kontaktobjekt muß folgende synchronisiert werden:

- Der Kontakt muss e-Mail aktiviert sein. Es überprüft die folgenden Regeln:
    - `IsPresent([proxyAddresses]) = True)`. Das Attribut ProxyAddresses muss ausgefüllt werden.
    - Eine e-Mail-Adresse finden im Attribut ProxyAddresses oder das Mail-Attribut. Das Vorhandensein einer @ wird verwendet, um sicherzustellen, dass der Inhalt einer e-Mail-Adresse ist. Eines dieser beiden Regeln muss auf True ausgewertet werden.
        - `(Contains([proxyAddresses], "SMTP:") > 0) && (InStr(Item([proxyAddresses], Contains([proxyAddresses], "SMTP:")), "@") > 0))`. Gibt es ein Eintrag mit "SMTP:" und liegt, kann eine @ in der Zeichenfolge befinden?
        - `(IsPresent([mail]) = True && (InStr([mail], "@") > 0)`. Ist das Attribut Mail aufgefüllt und wenn es ist ein @ in der Zeichenfolge befinden?

Kontakt Objekte in Azure AD **nicht** synchronisiert:

- `IsPresent([isCriticalSystemObject])`. Stellen Sie sicher, kein Kontakt als wichtig markiert Objekte synchronisiert werden. Dies sollte keiner Standardkonfiguration sein.
- `((InStr([displayName], "(MSOL)") > 0) && (CBool([msExchHideFromAddressLists])))`.
- `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`. Diese Objekte würde nicht Exchange Online arbeiten.
- `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Alle Replikationsobjekte Opfer nicht synchronisiert.

### <a name="group-out-of-box-rules"></a>Gruppe Regeln
Ein Gruppenobjekt muß folgende synchronisiert werden:

- Muss weniger als 50.000. Diese Anzahl wird die Anzahl der Mitglieder der lokalen Gruppe.
    - Hat es mehr Elemente vor der Synchronisierung zum ersten Mal startet, wird die Gruppe nicht synchronisiert.
    - Wenn die Zahl der Mitglieder von ursprünglich erstellt, dann erreicht der 50.000 wachsen wird synchronisiert, bis die Anzahl der Mitgliedschaft unter 50.000 ist beendet.
    - Hinweis: Die Anzahl 50.000 Mitgliedschaft wird auch von Azure AD erzwungen. Sie können keine Gruppen mit mehr Elemente synchronisieren, auch wenn ändern oder diese Regel entfernen.
- Wenn die Gruppe eine **Verteilergruppe**ist, muss auch e-Mail-aktiviert sein. Siehe [Kontakt von vordefinierten Regeln](#contact-out-of-box-rules) für diese Regel erzwungen wird.

Die folgende Gruppe Objekte in Azure AD **nicht** synchronisiert:

- `IsPresent([isCriticalSystemObject])`. Viele Out-of-Box-Objekte in Active Directory wie der integrierten Administratorgruppe sicherzustellen, werden nicht synchronisiert.
- `[sAMAccountName] = "MSOL_AD_Sync_RichCoexistence"`. Ältere Gruppe von DirSync verwendet.
- `BitAnd([msExchRecipientTypeDetails],&amp;H40000000)`. Gruppe.
- `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Alle Replikationsobjekte Opfer nicht synchronisiert.

### <a name="foreignsecurityprincipal-out-of-box-rules"></a>ForeignSecurityPrincipal von vordefinierten Regeln
FSPs "alle" hinzugefügt (\*) Objekt im Metaverse. In der Praxis tritt diese Verknüpfung nur für Benutzer und Sicherheitsgruppen. Diese Konfiguration stellt sicher, dass gesamtstrukturübergreifende Mitgliedschaften gelöst und in Azure AD korrekt dargestellt.

### <a name="computer-out-of-box-rules"></a>Computer Out-of-Box-Regeln
Ein Computerobjekt muß folgende synchronisiert werden:

- `userCertificate ISNOTNULL`. Nur Windows 10 Computer Auffüllen dieses Attribut. Alle Computerobjekte mit einem Wert in diesem Attribut werden synchronisiert.

## <a name="understanding-the-out-of-box-rules-scenario"></a>Szenario Regeln verstehen
In diesem Beispiel verwenden wir eine Bereitstellung mit einem Konto (A) eine Ressourcengesamtstruktur (R) und einem Azure AD-Verzeichnis.

![Bild mit Beschreibung](./media/active-directory-aadconnectsync-understanding-default-configuration/scenario.png)

In dieser Konfiguration ist davon ein aktiviertes Konto in der Gesamtstruktur und der Gesamtstruktur mit verknüpften Postfachs ein deaktiviertes Konto.

Mit der Standardkonfiguration ist:

- Anmelden Attribute werden aus der Gesamtstruktur mit dem aktivierten Konto synchronisiert.
- Attribute, die in der GAL (globale Adressliste) finden werden aus der Gesamtstruktur mit dem Postfach synchronisiert. Wenn kein Postfach gefunden wird, wird jeder Gesamtstruktur verwendet.
- Wenn ein verknüpftes Postfach gefunden wird, muss Verknüpfte aktivierte Konto für das Objekt in Azure AD exportiert werden gefunden.

### <a name="synchronization-rule-editor"></a>Synchronisierung Regeleditor
Die Konfiguration kann angezeigt und mit der Synchronisierung Regeln Editor (SRE) geändert und eine Verknüpfung im Startmenü befinden.

![Symbol für Synchronisierung Regel-Editor](./media/active-directory-aadconnectsync-understanding-default-configuration/sre.png)

SRE ist ein Resource Kit-Tool und Azure AD Connect Sync installiert wird. Um starten zu können, müssen Sie Mitglied der Gruppe "ADSyncAdmins" sein. Beim Start sehen Sie etwa Folgendes:

![Eingehende Regeln](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulesinbound.png)

In diesem Bereich sehen Sie alle Regeln für die Konfiguration erstellt. Jede Zeile in der Tabelle gilt eine Synchronisierung. Links unter Regel zwei Arten aufgeführt: eingehende und ausgehende. Eingehende und ausgehende ist das Metaverse. Sie werden hauptsächlich sich eingehenden Regeln in dieser Übersicht. Die aktuelle Liste der Regeln hängt erkannte Schema in Active Directory. In der obigen Abbildung der Kontengesamtstruktur (fabrikamonline.com) muss keine Dienste wie Exchange und Lync und keine Regeln für diese Dienste erstellt wurden. Allerdings finden in der Ressourcengesamtstruktur (res.fabrikamonline.com) Sie Regeln für diese Dienste. Der Inhalt der Regeln ist je nach Version entdeckt. In einer Umgebung mit Exchange 2013 sind beispielsweise weitere attributflüsse als Exchange 2010-2007 konfiguriert.

### <a name="synchronization-rule"></a>Synchronisierungsregel
Eine Regel Synchronisierung ist ein Configuration-Objekt mit Attributen, wenn eine Bedingung erfüllt ist. Es dient auch beschreiben, wie ein Objekt in einem Connectorspace auf ein Objekt im Metaverse als **Verknüpfung** oder **entsprechen**verknüpft ist. Die Regeln haben Vorrang Dateizugriffsnummer Beziehung zueinander. Regel Synchronisierung mit einem niedrigeren numerischen Wert hat Vorrang und ein Attributkonflikt Fluss Vorrang erhält die Auflösung des Konflikts.

Betrachten Sie beispielsweise die Regel Synchronisierung **In aus AD-Benutzer AccountEnabled**. Markieren Sie diese Zeile in der SRE, und wählen Sie **Bearbeiten**.

Da diese Regel eine Out-of-Box-Regel ist, erhalten Sie eine Warnung, die Regel öffnen. Keine stellen [Änderung Regeln](active-directory-aadconnectsync-best-practices-changing-default-configuration.md)damit Sie gefragt werden, was Ihre Absichten. In diesem Fall möchten Sie nur die Regel anzuzeigen. Wählen Sie **Nein**.

![Synchronisierung Regeln Warnung](./media/active-directory-aadconnectsync-understanding-default-configuration/warningeditrule.png)

Synchronisierungsregel hat vier Konfigurationsabschnitte: Beschreibung Scoping Filter Join Regeln und Transformationen.

#### <a name="description"></a>Beschreibung
Der erste Abschnitt enthält grundlegende Informationen wie Name und Beschreibung.

![Registerkarte Beschreibung synchron Regeleditor ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncruledescription.png)

Finden Sie auch Informationen über die verbundenen System dieser Regel, ist der Objekttyp im verbundenen System gilt für und Metaverse-Objekttyp. Metaverse-Objekttyp wird immer unabhängig Person Quelle Objekttyp Benutzer, iNetOrgPerson oder Kontakt ist. Metaverse-Objekttyp sollten nie geändert als generischer Typ erstellt wird. Der Verbindungstyp kann Join, StickyJoin oder Bereitstellung festgelegt werden. Diese Einstellung mit Join Regeln und wird später behandelt.

Sie sehen auch, dass diese Synchronisierungsregel kennwortsynchronisierung verwendet wird. Wenn ein Benutzer im Bereich für diese Synchronisierungsregel ist, wird das Kennwort synchronisiert lokal Cloud (vorausgesetzt, die Synchronisierung Kennwortfunktion aktiviert haben).

#### <a name="scoping-filter"></a>Bereichsfilter
Abschnitt Bereiche Filter Dient zum Konfigurieren, wenn eine Synchronisation Regel gelten soll. Da Namen suchen bei der Synchronisierung nur für aktivierte Benutzer angewendet werden soll gibt, wird der Bereich so AD-Attribut **UserAccountControl** Bit 2 nicht muss konfiguriert festgelegt. Wenn das Synchronisierungsmodul einen Benutzer in Active Directory findet, gilt es diese Synchronisierung Wenn **UserAccountControl** Dezimalwert 512 (normale Benutzer aktiviert) festgelegt ist. Es gilt die Regel nicht, wenn Benutzer **UserAccountControl** 514 (normale deaktivierten) festgelegt wurde.

![Bereiche auf Sync Regeleditor ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulescopingfilter.png)

Die Bereichsfilter hat Gruppen und Klauseln können geschachtelt werden. Alle Klauseln innerhalb einer Gruppe müssen für Synchronisierungsregel erfüllt. Wenn mehrere Gruppen definiert werden, muss mindestens eine Gruppe für die Regel erfüllt. D. h. ein logisches OR zwischen Gruppen und eine logische ausgewertet und innerhalb einer Gruppe ausgewertet. Ein Beispiel für diese Konfiguration finden in der ausgehenden Regel Synchronisierung **zum AAD-Gruppe**. Gibt es mehrere Filter Synchronisierungsgruppen, beispielsweise eines für Sicherheitsgruppen (`securityEnabled EQUAL True`) und Verteilergruppen (`securityEnabled EQUAL False`).

![Bereiche auf Sync Regeleditor ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulescopingfilterout.png)

Diese Regel dient zum definieren, welche Gruppen in Azure Active Directory bereitgestellt werden sollten. Verteilergruppen müssen aktiviert werden mit Azure synchronisiert werden, aber für Sicherheitsgruppen e-Mail ist nicht erforderlich.

#### <a name="join-rules"></a>Verknüpfen von Regeln
Im dritten Abschnitt wird zum Konfigurieren, wie Objekte im Connectorspace auf Objekte im Metaverse beziehen. Die Regel, die Sie bereits betrachtet haben keine Konfigurationen für beitreten, stattdessen werden sich **In aus AD-Benutzer teilnehmen**.

![Sync Regeleditor Registerkarte teilnehmen ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulejoinrules.png)

Der Inhalt der Regel Join hängt die entsprechende Option im Installations-Assistenten. Für eine eingehende Regel Bewertung beginnt mit einem Objekt im Connectorspace Quelle und jede Gruppe in der Join-Regeln nacheinander ausgewertet. Wenn ein Quellobjekt um genau ein Objekt im Metaverse mithilfe einer Verknüpfungsregeln ausgewertet wird, werden die Objekte hinzugefügt. Wenn alle Regeln wurden und keine Übereinstimmung vorliegt, wird den Link auf der Beschreibungsseite verwendet. Wenn diese Konfiguration **Bereitstellen**festgelegt ist, wird ein neues Objekt im Ziel Metaverse erstellt. Um ein neues Objekt in das Metaversum bereitzustellen ist auch bekannt als **Projekt** ein Metaverse-Objekt.

Join-Regeln werden nur einmal ausgewertet. Wenn ein Connectorspace-Objekt und ein Metaverse-Objekt verknüpft werden, solange sie verknüpfte Regel Synchronisierung noch zufrieden ist.

Bei Regeln müssen nur eine Synchronisierungsregel mit Join Regeln im Gültigkeitsbereich befinden. Wenn mehrere Regeln mit Join Regeln für ein Objekt gefunden werden, wird ein Fehler ausgelöst. Aus diesem Grund empfiehlt, nur eine Synchronisierungsregel mit mehreren Regeln sind für ein Objekt definiert haben. In der Out-of-Box-Konfiguration für Azure AD verbinden synchronisieren diese Regeln gefunden werden, indem der Name und das Wort am Ende des Namens **Join** finden. Eine Synchronisation ohne Verknüpfungsregeln gilt Attribut Abläufe bei anderen Synchronisierung die Objekte miteinander verbunden sind oder ein neues Objekt im Ziel bereitgestellt.

Betrachten Sie das Bild, werden Sie sehen, dass die Regel an **ObjectSID** **MsExchMasterAccountSid** (Exchange) mit **MsRTCSIP-OriginatorSid** (Lync) erwarten wir in einer Konto-Topologie ist. Die gleiche Regel für alle Gesamtstrukturen befinden. Die Annahme ist, dass jeder Gesamtstruktur ein Konto oder Ressourcen-Gesamtstruktur sein kann. Diese Konfiguration funktioniert auch, wenn Sie Konten in einer einzigen Gesamtstruktur live und müssen nicht hinzugefügt werden.

#### <a name="transformations"></a>Transformationen
Der Abschnitt Transformation definiert alle Attribut Flows, die auf das Zielobjekt angewendet, wenn die Objekte hinzugefügt und der Bereichsfilter erfüllt. Zurück zu den **In aus AD-Benutzer AccountEnabled** Regel Synchronisierung finden die folgenden Transformationen:

![Registerkarte Transformationen synchron Regeleditor ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncruletransformations.png)

Setzen diese Konfiguration im Kontext oder in einer Gesamtstruktur Bereitstellung Ressource ist es erwartet ein aktiviertes Konto Konto und ein deaktiviertes Konto in der Ressourcengesamtstruktur mit Exchange und Lync. Sollen Sie Synchronisierung-Regel enthält die Anmeldung erforderlichen Attribute und diese Attribute aus der Gesamtstruktur sollte ein aktiviertes Konto vorliegt. Diese Attribut Datenströme sind in der Regel eine Synchronisierung zusammengestellt.

Eine Transformation kann andere Typen haben: Konstante Direct und Ausdruck.

- Ständig fließt immer einen hartcodierten Wert. Im obigen Fall legt immer den Wert **True** im Metaverse Attribut mit dem Namen **AccountEnabled**.
- Ein direkter Fluss fließt immer den Wert des Attributs in der Quelle auf das Zielattribut als-ist.
- Der dritte Flusstyp ist Ausdruck und ermöglicht erweiterte Konfigurationen.

Ausdruck ist VBA (Visual Basic for Applications), damit Personen mit Microsoft Office oder VBScript erkennt das Format. Attribute werden in Klammern [Attributname] eingeschlossen. Attributnamen und Funktionsnamen Groß-und Kleinschreibung, aber der Regeln Synchronisierung wertet die Ausdrücke und bieten eine Warnung, wenn der Ausdruck ungültig ist. Alle Ausdrücke werden in einer einzigen Zeile mit verschachtelten Funktionen ausgedrückt. Zeigen Sie die macht der konfigurationssprache erfolgt hier die PwdLastSet jedoch zusätzliche Kommentare eingefügt:

```
// If-then-else
IIF(
// (The evaluation for IIF) Is the attribute pwdLastSet present in AD?
IsPresent([pwdLastSet]),
// (The True part of IIF) If it is, then from right to left, convert the AD time format to a .Net datetime, change it to the time format used by Azure AD, and finally convert it to a string.
CStr(FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")),
// (The False part of IIF) Nothing to contribute
NULL
)
```

[Understanding deklarative Bereitstellung Ausdrücke](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) Weitere Ausdruckssprache Attribut Datenflüsse anzeigen

### <a name="precedence"></a>Rangfolge
Haben Sie nun einige Regeln für einzelne sah jedoch Regeln gemeinsam in der Konfiguration. In einigen Fällen ist Attributwert aus mehrere Regeln auf dasselbe Ziel-Attribut beigetragen. In diesem Fall dient Attribut Vorrang zu ermitteln, welches Attribut gewinnt. Betrachten Sie beispielsweise das Attribut SourceAnchor. Dieses Attribut ist ein wichtiges Attribut Azure AD anmelden können. Finden Sie einen Attributfluss für dieses Attribut in zwei verschiedenen Synchronisierungsregeln **In aus AD-Benutzer AccountEnabled** und **In aus AD-Benutzer häufig**. Durch Synchronisierung Regelpriorität ist das SourceAnchor-Attribut aus der Gesamtstruktur ein aktiviertes Konto zuerst beigetragen mehrere Objekte mit dem Metaverse-Objekt verknüpft. Wenn keine aktivierten Konten, das Synchronisierungsmodul Catchall-Synchronisierungsregel verwendet **In aus AD-Benutzer häufig**. Diese Konfiguration stellt sicher, dass auch für Computerkonten, die deaktiviert sind, gibt es noch eine SourceAnchor.

![Eingehende Regeln](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulesinbound.png)

Vorrang für Regeln wird vom Assistenten in Gruppen festgelegt. Alle Regeln in einer Gruppe haben denselben Namen, aber andere verbundene Verzeichnisse verbunden sind. Der Installations-Assistent gibt die Regel **In aus AD-Benutzer teilnehmen** höchste Priorität und wird über alle verbundenen AD-Verzeichnissen. Fährt dann mit der nächsten Gruppen von Regeln in einer vordefinierten Reihenfolge. Innerhalb einer Gruppe werden die Regeln in der Reihenfolge hinzugefügt, die Anschlüsse im Assistenten hinzugefügt wurden. Wenn ein weiterer Connector durch den Assistenten hinzugefügt wird, die Regeln neu angeordnet und den neuen Connector Regeln werden zuletzt in jeder Gruppe.

### <a name="putting-it-all-together"></a>Fazit
Wir wissen jetzt genug Regeln können die Konfiguration mit verschiedenen Synchronisierungsregeln Funktionsweise. Wenn Sie einen Benutzer und die Attribute, die in das Metaversum beigetragen betrachten, werden die Regeln in der folgenden Reihenfolge angewendet:

Name | Kommentar
:------------- | :-------------
In aus dem Active Directory-Benutzer-Verknüpfung | Regel für das Connectorspace-Objekte mit Metaverse verknüpfen.
In Active Directory-Benutzerkonto aktiviert | Erforderliche Attribute für eine Anmeldung in Azure AD und Office 365. Wir möchten diese Attribute aus dem aktiviert.
In von AD-Benutzer allgemein von Exchange | Attribute in der globalen Adressliste gefunden. Angenommen, die Qualität der Daten am besten in der Gesamtstruktur, wo wir das Postfach des Benutzers gefunden.
In von AD-Benutzer Allgemein | Attribute in der globalen Adressliste gefunden. Falls wir ein Postfach nicht finden, kann verknüpfte Objekt den Attributwert beitragen.
In von AD-Exchange-Benutzers | Nur vorhanden, wenn Exchange erkannt wurde. Alle Exchange-Attribute von Infrastruktur fließt.
Im AD-Benutzer Lync aus | Nur vorhanden, wenn Lync erkannt wurde. Alle Infrastruktur Lync Attribute fließt.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen über Konfigurationsmodell [Verständnis deklarative](active-directory-aadconnectsync-understanding-declarative-provisioning.md)Bereitstellung.
- Weitere Informationen über Ausdruckssprache [Verständnis deklarative Bereitstellung Ausdrücke](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).
- Lesen Sie [Grundlegendes zu Benutzern](active-directory-aadconnectsync-understanding-users-and-contacts.md) und Kontakten Funktionsweise von Out-of-Box-Konfiguration
- Finden Sie unter praktische mit deklarativen Bereitstellung wie [Ändern Sie die Standardkonfiguration](active-directory-aadconnectsync-change-the-configuration.md)ändern.

**Themen (Übersicht)**

- [Azure AD Verbindung synchronisieren: verstehen und Synchronisation anpassen](active-directory-aadconnectsync-whatis.md)
- [Integration von lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)
