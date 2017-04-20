<properties
    pageTitle="Azure AD Verbindung synchronisieren: Verweis Funktionen | Microsoft Azure"
    description="Referenz der deklarativen Bereitstellung Ausdrücke in Azure AD-Verbindung synchronisieren."
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
    ms.date="08/23/2016"
    ms.author="andkjell;markvi"/>


# <a name="azure-ad-connect-sync-functions-reference"></a>Azure AD Verbindung synchronisieren: Funktionen Verweis

In Azure AD Connect werden Funktionen während der Synchronisierung Attributwert bearbeiten.  
Die Syntax der Funktionen wird im folgenden Format angegeben:  
`<output type> FunctionName(<input type> <position name>, ..)`

Wenn eine Funktion überladen ist, mehrere Syntax akzeptiert werden alle gültigen Syntax aufgeführt.  
Die Funktionen sind stark typisiert und sie überprüfen, ob der Typ entspricht den dokumentierten Typ übergeben.  
Wenn der Typ nicht übereinstimmt, wird ein Fehler ausgelöst.

Die Typen werden mit der folgenden Syntax angegeben:

- **Bin** -Binärdatei
- **Bool** -boolescher Wert
- **dt** – UTC-Datum/Zeit
- **Enum** -Enumeration von bekannten Konstanten
- **exp** -Ausdruck erwartet einen booleschen Wert ergeben
- **Mvbin** – binäres mehrwertige
- **Mvstr** – mehrwertige Zeichenfolge
- **Mvref** – mehrwertige Verweis
- **Num** -numerisch
- **Ref** -Referenz
- **str** -Zeichenfolge
- **Var** -Variante (fast) beliebigen Typs
- **void** -zurück nicht

Die Typen, **Mvbin**, **Mvstr**und **Mvref** können nur für mehrwertige Attribute funktioniert. **Lagerplatz**, **str**und **Ref** funktioniert sowohl einwertige als auch mehrwertige Attribute.

## <a name="functions-reference"></a>Funktionen-Referenz

Liste der Funktionen | | | | |  
--------- | --------- | --------- | --------- | --------- | ---------
**Konvertierung** |  
[CBool](#cbool) | [CDate](#cdate) | [CGuid](#cguid) | [ConvertFromBase64](#convertfrombase64)
[ConvertToBase64](#converttobase64) | [ConvertFromUTF8Hex](#convertfromutf8hex) | [ConvertToUTF8Hex](#converttoutf8hex) | [CNum](#cnum)
[CRef](#cref) | [CStr](#cstr) | [StringFromGuid](#StringFromGuid) | [StringFromSid](#stringfromsid)
**Datum und Uhrzeit** |  
[DateAdd](#dateadd) | [DateFromNum](#datefromnum) | [FormatDateTime](#formatdatetime) | [Jetzt](#now)
[NumFromDate](#numfromdate) |  
**Verzeichnis** |  
[DNComponent](#dncomponent) | [DNComponentRev](#dncomponentrev) | [EscapeDNComponent](#escapedncomponent)
**Auswertung** |  
[IsBitSet](#isbitset) | [IsDate](#isdate) | [IsEmpty](#isempty) | [IsGuid](#isguid)
[IsNull](#isnull) | [IsNullOrEmpty](#isnullorempty) | [IsNumeric](#isnumeric) | [Name](#ispresent) |
[IsString](#isstring) |  
**Mathematik** |  
[BitAnd](#bitand) | [BitOr](#bitor) | [RandomNum](#randomnum)
**Mehrwertige** |  
[Enthält](#contains) | [Anzahl](#count) | [Artikel](#item) | [ItemOrNull](#itemornull)
[Verknüpfung](#join) | [RemoveDuplicates](#removeduplicates) | [Teilen](#split) |
**Programmablauf** |  
[Fehler](#error) | [IIF](#iif)  | [Schalter](#switch)
**Text** |  
[GUID](#guid) | [InStr](#instr) | [InStrRev](#instrrev) | [LCase](#lcase)
[Links](#left) | [Len](#len) | [LTrim](#ltrim) | [Mitte](#mid)
[PadLeft](#padleft) | [PadRight](#padright) | [PCase](#pcase) | [Ersetzen](#replace)
[ReplaceChars](#replacechars) | [Richting](#right) | [RTrim](#rtrim) | [Zuschneiden](#trim)
[UCase](#ucase) | [Word](#word)

----------
### <a name="bitand"></a>BitAnd

**Beschreibung:**  
BITAND-Funktion festgelegt angegebenen Bits auf einen Wert.

**Syntax:**  
`num BitAnd(num value1, num value2)`

- Wert1, Wert2: numerische Werte Funktionsschlüssel zusammen werden soll

**Beschreibung:**  
Diese Funktion konvertiert beide Parameter in die Binärdaten und etwas auf:

- 0 - sind eine oder beide der entsprechenden Bits in der *Maske* und *Flag* 0
- 1 - Wenn beide Bits 1 sind.

Anders gesagt gibt 0 in allen Fällen, außer wenn die entsprechenden Bits der beiden Parameter 1 zurück.

**Beispiel:**  
`BitAnd(&HF, &HF7)`  
Gibt 7 zurück, da hexadezimale "F" und "F7" dieser Wert ergeben.

----------
### <a name="bitor"></a>BitOr

**Beschreibung:**  
BITOR-Funktion festgelegt angegebenen Bits auf einen Wert.

**Syntax:**  
`num BitOr(num value1, num value2)`

- Wert1, Wert2: numerische Werte, die Operator OR zusammen werden soll

**Beschreibung:**  
Diese Funktion konvertiert beide Parameter in die binäre Darstellung und etwas 1 sind eine oder beide der entsprechenden Bits in der Maske und Flag 1 und 0 Wenn die entsprechenden Bits 0 sind. Anders gesagt gibt 1 in allen Fällen außer dem entsprechenden Bits der beiden Parameter 0 zurück.

----------
### <a name="cbool"></a>CBool

**Beschreibung:**  
CBool-Funktion gibt einen booleschen Wert basierend auf der ausgewertete Ausdruck

**Syntax:**  
`bool CBool(exp Expression)`

**Beschreibung:**  
Wenn der Ausdruck einen Wert ungleich null ergibt, CBool gibt True zurück, andernfalls es gibt False zurück.

**Beispiel:**  
`CBool([attrib1] = [attrib2])`  

True, wenn beide Attribute gibt haben denselben Wert.

----------
### <a name="cdate"></a>CDate

**Beschreibung:**  
CDate-Funktion gibt eine UTC-DateTime aus einer Zeichenfolge zurück. DateTime ist kein natives Attribut synchron jedoch einige Funktionen verwendet.

**Syntax:**  
`dt CDate(str value)`

- Wert: Eine Zeichenfolge mit einem Datum, Uhrzeit und optional Zeitzone

**Beschreibung:**  
Die zurückgegebene Zeichenfolge ist immer in UTC.

**Beispiel:**  
`CDate([employeeStartTime])`  
Gibt ein DateTime-Wert anhand des Mitarbeiters Startzeit

`CDate("2013-01-10 4:00 PM -8")`  
Gibt einen DateTime zurück "2013-01-11 12:00 AM"

----------
### <a name="cguid"></a>CGuid

**Beschreibung:**  
Die CGuid-Funktion konvertiert String-Darstellung einer GUID in die binäre Darstellung.

**Syntax:**  
`bin CGuid(str GUID)`

- Eine Zeichenfolge in diesem Muster formatiert: Xxxxxxxx-Xxxx-Xxxx-Xxxx-Xxxxxxxxxxxx oder {Xxxxxxxx-Xxxx-Xxxx-Xxxx-Xxxxxxxxxxxx}

----------
### <a name="contains"></a>Enthält

**Beschreibung:**  
Die Contains-Funktion sucht eine Zeichenfolge in ein mehrwertiges Attribut

**Syntax:**  
`num Contains (mvstring attribute, str search)`-Groß-/Kleinschreibung  
`num Contains (mvstring attribute, str search, enum Casetype)`  
`num Contains (mvref attribute, str search)`-Groß-/Kleinschreibung

- Attribut: Attribut mit mehreren Werten zu suchen.
- Suche: Zeichenfolge im Attribut gefunden.
- Casetype: CaseInsensitive oder CaseSensitive.

Gibt den Index in der mehrwertigen Attribut, in dem die Zeichenfolge gefunden wurde. 0 wird zurückgegeben, wenn die Zeichenfolge nicht gefunden wird.

**Beschreibung:**  
Für mehrwertige Attribute findet die Suche Teilzeichenfolgen Werte.  
Für Attribute muss die gesuchte Zeichenfolge exakt dem Wert um als Übereinstimmung betrachtet.

**Beispiel:**  
`IIF(Contains([proxyAddresses],"SMTP:")>0,[proxyAddresses],Error("No primary SMTP address found."))`  
Das Attribut ProxyAddresses weist eine primäre e-Mail-Adresse (durch Großbuchstaben angegeben "SMTP:"), zurück ProxyAddress Attribut, sonst einen Fehler zurück.

----------
### <a name="convertfrombase64"></a>ConvertFromBase64

**Beschreibung:**  
Die ConvertFromBase64-Funktion konvertiert angegebene base64-codierter Wert in eine reguläre Zeichenfolge.

**Syntax:**  
`str ConvertFromBase64(str source)`-übernimmt Unicode-Codierung  
`str ConvertFromBase64(str source, enum Encoding)`

- Quelle: Base64-codierte Zeichenfolge  
- -Codierung: Unicode, ASCII, UTF8

**Beispiel**  
`ConvertFromBase64("SABlAGwAbABvACAAdwBvAHIAbABkACEA")`  
`ConvertFromBase64("SGVsbG8gd29ybGQh", UTF8)`

In beiden Beispielen zurück "*Hello World!*"

----------
### <a name="convertfromutf8hex"></a>ConvertFromUTF8Hex

**Beschreibung:**  
ConvertFromUTF8Hex-Funktion konvertiert angegebenen codierte UTF8 Hex-Wert in eine Zeichenfolge.

**Syntax:**  
`str ConvertFromUTF8Hex(str source)`

- Quelle: codierte UTF8 2-Byte-Sting

**Beschreibung:**  
Der Unterschied zwischen dieser Funktion und ConvertFromBase64([],UTF8) ist das Ergebnis für das DN-Attribut.  
Dieses Format wird als DN von Azure Active Directory verwendet.

**Beispiel:**  
`ConvertFromUTF8Hex("48656C6C6F20776F726C6421")`  
Gibt "*Hello World!*"

----------
### <a name="converttobase64"></a>ConvertToBase64

**Beschreibung:**  
Die ConvertToBase64-Funktion konvertiert eine Zeichenfolge in Unicode base64.  
Konvertiert den Wert eines Arrays von ganzen Zahlen in die entsprechende Zeichenfolge-Darstellung mit Base-64-Ziffern codiert ist.

**Syntax:**  
`str ConvertToBase64(str source)`

**Beispiel:**  
`ConvertToBase64("Hello world!")`  
Gibt "SABlAGwAbABvACAAdwBvAHIAbABkACEA"

----------
### <a name="converttoutf8hex"></a>ConvertToUTF8Hex

**Beschreibung:**  
Die ConvertToUTF8Hex-Funktion konvertiert eine Zeichenfolge in eine codierte UTF8 Hex-Wert.

**Syntax:**  
`str ConvertToUTF8Hex(str source)`

**Beschreibung:**  
Das Format für Ausgabe dieser Funktion wird als DN Attributformat Azure Active Directory verwendet.

**Beispiel:**  
`ConvertToUTF8Hex("Hello world!")`  
Gibt 48656C6C6F20776F726C6421

----------
### <a name="count"></a>Anzahl

**Beschreibung:**  
Die Funktion Anzahl gibt die Anzahl der Elemente in einem mehrwertigen Attribut

**Syntax:**  
`num Count(mvstr attribute)`

----------
### <a name="cnum"></a>CNum

**Beschreibung:**  
CNum-Funktion akzeptiert eine Zeichenfolge und gibt einen numerischen Datentyp.

**Syntax:**  
`num CNum(str value)`

----------
### <a name="cref"></a>CRef

**Beschreibung:**  
Konvertiert eine Zeichenfolge in ein Verweisattribut

**Syntax:**  
`ref CRef(str value)`

**Beispiel:**  
`CRef("CN=LC Services,CN=Microsoft,CN=lcspool01,CN=Pools,CN=RTC Service," & %Forest.LDAP%)`

----------
### <a name="cstr"></a>CStr

**Beschreibung:**  
CStr-Funktion konvertiert einen String-Datentyp.

**Syntax:**  
`str CStr(num value)`  
`str CStr(ref value)`  
`str CStr(bool value)`  

- Wert: kann einen numerischen Wert Verweisattribut oder boolescher Wert.

**Beispiel:**  
`CStr([dn])`  
Würden "Cn = Joe, dc = Contoso, dc = com"

----------
### <a name="dateadd"></a>DateAdd

**Beschreibung:**  
Gibt ein Datum mit einem Datum, zu dem ein angegebenes Zeitintervall hinzugefügt wurde.

**Syntax:**  
`dt DateAdd(str interval, num value, dt date)`

- Intervall: String-Ausdruck, der das Zeitintervall ist hinzufügen möchten. Die Zeichenfolge muss einer der folgenden Werte enthalten:
 - Yyyy Jahr
 - Q Quartal
 - m Monat
 - Tag des Jahres y
 - d Tag
 - w Wochentag
 - WW Woche
 - Stunde
 - n Minuten
 - s zweite
- Wert: die Anzahl der Einheiten, die Sie hinzufügen möchten. Er kann positiv (für Datumsangaben in der Zukunft) oder negativ (für Datumsangaben in der Vergangenheit) sein.
- Datum: DateTime darstellt, dem das Intervall hinzugefügt.

**Beispiel:**  
`DateAdd("m", 3, CDate("2001-01-01"))`  
Fügt 3 Monate und gibt einen datetime-Wert "2001-04-01" darstellt.

----------
### <a name="datefromnum"></a>DateFromNum

**Beschreibung:**  
Anzeige Datum ein Wert in einen DateTime format konvertiert die DateFromNum-Funktion.

**Syntax:**  
`dt DateFromNum(num value)`

**Beispiel:**  
`DateFromNum([lastLogonTimestamp])`  
`DateFromNum(129699324000000000)`  
Gibt eine DateTime darstellt 2012-01-01 23:00:00

----------
### <a name="dncomponent"></a>DNComponent

**Beschreibung:**  
Die DNComponent-Funktion gibt den Wert einer angegebenen DN-Komponente von links.

**Syntax:**  
`str DNComponent(ref dn, num ComponentNumber)`

- DN: Verweisattribut interpretieren
- Bauteilnummer: Die Komponente in der DN zurückgegeben

**Beispiel:**  
`DNComponent([dn],1)`  
Dn ist "Cn = Joe, Ou =..." gibt Joe

----------
### <a name="dncomponentrev"></a>DNComponentRev

**Beschreibung:**  
DNComponentRev-Funktion gibt den Wert einer angegebenen DN-Komponente von rechts (Ende).

**Syntax:**  
`str DNComponentRev(ref dn, num ComponentNumber)`  
`str DNComponentRev(ref dn, num ComponentNumber, enum Options)`

- DN: Verweisattribut interpretieren
- Bauteilnummer - Komponente im DN zurückgegeben
- Optionen: DC – alle Komponenten mit ignorieren "dc ="

**Beispiel:**  
Ist dn "Cn = Joe Ou Atlanta, Ou = GA, Ou = = US, dc = Contoso, dc = com" dann  
`DNComponentRev([dn],3)`  
`DNComponentRev([dn],1,"DC")`  
Beide geben uns.

----------
### <a name="error"></a>Fehler

**Beschreibung:**  
Fehler-Funktion wird verwendet, um eine benutzerdefinierte Fehlermeldung zurückzugeben.

**Syntax:**  
`void Error(str ErrorMessage)`

**Beispiel:**  
`IIF(IsPresent([accountName]),[accountName],Error("AccountName is required"))`  
Wenn der Kontoname Attribut nicht vorhanden ist, lösen Sie einen Fehler für das Objekt.

----------
### <a name="escapedncomponent"></a>EscapeDNComponent

**Beschreibung:**  
EscapeDNComponent-Funktion akzeptiert eine Komponente eines DN und schützt sie damit in LDAP dargestellt werden kann.

**Syntax:**  
`str EscapeDNComponent(str value)`

**Beispiel:**  
`EscapeDNComponent("cn=" & [displayName]) & "," & %ForestLDAP%)`  
Stellt sicher, dass das Objekt in einem LDAP-Verzeichnis erstellt werden kann, selbst wenn das DisplayName-Attribut Zeichen enthält, die in LDAP mit Escapezeichen versehen werden muss.

----------
### <a name="formatdatetime"></a>FormatDateTime

**Beschreibung:**  
FormatDateTime-Funktion wird verwendet, um einen datetime-Wert in eine Zeichenfolge mit einem angegebenen Format zu formatieren

**Syntax:**  
`str FormatDateTime(dt value, str format)`

- Wert: Wert im DateTime-Format
- Format: eine Zeichenfolge mit das Format konvertieren.

**Beschreibung:**  
Die möglichen Werte für das Format hier: [User-Defined Date/Time Formats (Format Function)](http://msdn2.microsoft.com/library/73ctwf33\(VS.90\).aspx)

**Beispiel:**  

`FormatDateTime(CDate("12/25/2007"),"yyyy-mm-dd")`  
Führt "2007-12-25".

`FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")`  
"20140905081453.0Z" kann

----------
### <a name="guid"></a>GUID

**Beschreibung:**  
Die Funktion GUID generiert eine neue zufällige GUID

**Syntax:**  
`str GUID()`

----------
### <a name="iif"></a>IIF

**Beschreibung:**  
Die Funktion gibt einen ein Satz möglicher Werte anhand einer angegebenen Bedingung zurück.

**Syntax:**  
`var IIF(exp condition, var valueIfTrue, var valueIfFalse)`

- Bedingung: beliebiger Wert oder Ausdruck, der true oder false ausgewertet werden kann.
- WertWennWahr: ergibt die Bedingung true zurückgegebenen Wert.
- Funktion Valueiffalse zurück: ergibt die Bedingung der Rückgabewert False.

**Beispiel:**  
`IIF([employeeType]="Intern","t-" & [alias],[alias])`  
 Ist der Benutzer Praktikant, gibt der Alias eines Benutzers mit"t" am Anfang der else hinzugefügt den Benutzeralias wie zurück.

----------
### <a name="instr"></a>InStr

**Beschreibung:**  
Die InStr-Funktion sucht das erste Vorkommen einer Teilzeichenfolge in einer Zeichenfolge

**Syntax:**  

`num InStr(str stringcheck, str stringmatch)`  
`num InStr(str stringcheck, str stringmatch, num start)`  
`num InStr(str stringcheck, str stringmatch, num start , enum compare)`

- StringCheck: Zeichenfolge durchsucht werden
- StringMatch: Zeichenfolge gefunden werden
- Starten: Anfangsposition die Teilzeichenfolge finden
- vergleichen: VbTextCompare oder VbBinaryCompare

**Beschreibung:**  
Gibt die Position, in die Teilzeichenfolge gefunden oder 0, wenn nicht gefunden.

**Beispiel:**  
`InStr("The quick brown fox","quick")`  
Auswertet, 5

`InStr("repEated","e",3,vbBinaryCompare)`  
7 ergibt

----------
### <a name="instrrev"></a>InStrRev

**Beschreibung:**  
InStrRev-Funktion sucht das letzte Vorkommen einer Teilzeichenfolge in einer Zeichenfolge

**Syntax:**  
`num InstrRev(str stringcheck, str stringmatch)`  
`num InstrRev(str stringcheck, str stringmatch, num start)`  
`num InstrRev(str stringcheck, str stringmatch, num start, enum compare)`

- StringCheck: Zeichenfolge durchsucht werden
- StringMatch: Zeichenfolge gefunden werden
- Starten: Anfangsposition die Teilzeichenfolge finden
- vergleichen: VbTextCompare oder VbBinaryCompare

**Beschreibung:**  
Gibt die Position, in die Teilzeichenfolge gefunden oder 0, wenn nicht gefunden.

**Beispiel:**  
`InStrRev("abbcdbbbef","bb")`  
Gibt 7

----------
### <a name="isbitset"></a>IsBitSet

**Beschreibung:**  
Die Funktion IsBitSet prüft, ob etwas ist oder nicht festgelegt.

**Syntax:**  
`bool IsBitSet(num value, num flag)`

- Wert: einen numerischen Wert evaluated.flag: eine Zahl, die das Bit ausgewertet werden

**Beispiel:**  
`IsBitSet(&HF,4)`  
Gibt true zurück, da in den Hexadezimalwert "F" Bit "4" festgelegt ist

----------
### <a name="isdate"></a>IsDate

**Beschreibung:**  
Wenn Ausdruck sein kann als DateTime-Typ ausgewertet und die IsDate-Funktion True ergibt.

**Syntax:**  
`bool IsDate(var Expression)`

**Beschreibung:**  
Bestimmt, ob CDate() erfolgreich sein können.

----------
### <a name="isempty"></a>IsEmpty

**Beschreibung:**  
Wenn das Attribut in CS oder MV aber eine leere Zeichenfolge ist, ist die Funktion IsEmpty wahr.

**Syntax:**  
`bool IsEmpty(var Expression)`

----------
### <a name="isguid"></a>IsGuid

**Beschreibung:**  
Wenn die Zeichenfolge in eine GUID konvertiert werden kann, die IsGuid-Funktion, der True ergibt.

**Syntax:**  
`bool IsGuid(str GUID)`

**Beschreibung:**  
Eine GUID wird als Zeichenfolge einem Muster definiert: Xxxxxxxx-Xxxx-Xxxx-Xxxx-Xxxxxxxxxxxx oder {Xxxxxxxx-Xxxx-Xxxx-Xxxx-Xxxxxxxxxxxx}

Bestimmt, ob CGuid() erfolgreich sein können.

**Beispiel:**  
`IIF(IsGuid([strAttribute]),CGuid([strAttribute]),NULL)`  
Hat der StrAttribute eine GUID-Format, eine binäre Darstellung zurück, andernfalls Null zurück.

----------
### <a name="isnull"></a>IsNull

**Beschreibung:**  
Wenn der Ausdruck Null ergibt, gibt die IsNull-Funktion true zurück.

**Syntax:**  
`bool IsNull(var Expression)`

**Beschreibung:**  
Für ein Attribut ist Null durch die Abwesenheit des Attributs ausgedrückt.

**Beispiel:**  
`IsNull([displayName])`  
Gibt True zurück, wenn das Attribut nicht im CS oder MV vorhanden ist.

----------
### <a name="isnullorempty"></a>IsNullOrEmpty

**Beschreibung:**  
Wenn der Ausdruck Null oder eine leere Zeichenfolge ist, gibt die Funktion IsNullOrEmpty true zurück.

**Syntax:**  
`bool IsNullOrEmpty(var Expression)`

**Beschreibung:**  
Für ein Attribut würde dies zu True ausgewertet, wenn das Attribut fehlt oder ist eine leere Zeichenfolge.  
Die Umkehrung dieser Funktion heißt Name.

**Beispiel:**  
`IsNullOrEmpty([displayName])`  
Gibt True zurück, wenn das Attribut nicht vorhanden oder eine leere Zeichenfolge im CS oder MV ist.

----------
### <a name="isnumeric"></a>IsNumeric

**Beschreibung:**  
Die IsNumeric-Funktion gibt einen booleschen Wert, der angibt, ob ein Ausdruck als Zahl Typ ausgewertet werden kann.

**Syntax:**  
`bool IsNumeric(var Expression)`

**Beschreibung:**  
Bestimmt, ob CNum() erfolgreich den Ausdruck analysiert werden können.

----------
### <a name="isstring"></a>IsString

**Beschreibung:**  
Wenn der Ausdruck einen Zeichenfolgentyp ausgewertet werden kann, ist die IsString Funktion WAHR.

**Syntax:**  
`bool IsString(var expression)`

**Beschreibung:**  
Bestimmt, ob CStr() erfolgreich den Ausdruck analysiert werden können.

----------
### <a name="ispresent"></a>Name

**Beschreibung:**  
Wenn der Ausdruck eine Zeichenfolge, die nicht Null und nicht leer ist ergibt, gibt die Funktion Name true zurück.

**Syntax:**  
`bool IsPresent(var expression)`

**Beschreibung:**  
Die Umkehrung dieser Funktion heißt IsNullOrEmpty.

**Beispiel:**  
`Switch(IsPresent([directManager]),[directManager], IsPresent([skiplevelManager]),[skiplevelManager], IsPresent([director]),[director])`

----------
### <a name="item"></a>Artikel

**Beschreibung:**  
Die Item-Funktion gibt ein Element aus einer mehrwertigen Zeichenfolgenattribut.

**Syntax:**  
`var Item(mvstr attribute, num index)`

- Attribut: Attribut mit mehreren Werten
- Index: Index für ein Element in die mehrwertige Zeichenfolge.

**Beschreibung:**  
Die Item-Funktion eignet sich zusammen mit der Funktion enthält die Funktion Index ein Element in das mehrwertige Attribut zurück.

Löst einen Fehler aus, wenn der Index außerhalb des gültigen Bereichs liegt.

**Beispiel:**  
`Mid(Item([proxyAddress],Contains([proxyAddress], "SMTP:")),6)`  
Die primäre e-Mail-Adresse zurückgegeben.

----------
### <a name="itemornull"></a>ItemOrNull

**Beschreibung:**  
Die ItemOrNull-Funktion gibt ein Element aus einer mehrwertigen Zeichenfolgenattribut.

**Syntax:**  
`var ItemOrNull(mvstr attribute, num index)`

- Attribut: Attribut mit mehreren Werten
- Index: Index für ein Element in die mehrwertige Zeichenfolge.

**Beschreibung:**  
Die ItemOrNull-Funktion eignet sich zusammen mit der Funktion enthält die Funktion Index ein Element in das mehrwertige Attribut zurück.

Wenn Index außerhalb des gültigen Bereichs, gibt Null zurück.

----------
### <a name="join"></a>Verknüpfung

**Beschreibung:**  
Die Funktion Join eine mehrwertige Zeichenfolge und gibt eine einwertige Zeichenfolge angegebene Trennzeichen zwischen den Elementen eingefügt.

**Syntax:**  
`str Join(mvstr attribute)`  
`str Join(mvstr attribute, str Delimiter)`

- Attribut: mehrwertiges Attribut mit Zeichenfolgen verknüpft werden.
- Trennzeichen: jede Zeichenfolge, die die Teilzeichenfolgen in der zurückgegebenen Zeichenfolge getrennt. Wenn nicht angegeben, wird das Leerzeichen ("") verwendet wird. Wenn Delimiter eine Zeichenfolge der Länge Null ("") oder keine alle Elemente in der Liste ohne Trennzeichen verkettet.

**Beschreibung**  
Es ist die Parität zwischen den Join und teilen. Join-Funktion ein Array von Zeichenfolgen und verknüpft diese mithilfe einer Trennzeichenfolge und eine einzige Zeichenfolge zurück. Split-Funktion akzeptiert eine Zeichenfolge, trennt diese am Trennzeichen und gibt ein Zeichenfolgenarray zurück. Jedoch unterscheiden Join kann Zeichenfolgen mit beliebigen Trennzeichenfolgen verketten, Split nur Zeichenfolgen mit einem einzigen Trennzeichen trennen kann.

**Beispiel:**  
`Join([proxyAddresses],",")`  
Konnte zurück:"SMTP:john.doe@contoso.com,smtp:jd@contoso.com"

----------
### <a name="lcase"></a>LCase

**Beschreibung:**  
Die LCase-Funktion konvertiert alle Zeichen in einer Zeichenfolge in Kleinbuchstaben.

**Syntax:**  
`str LCase(str value)`

**Beispiel:**  
`LCase("TeSt")`  
Gibt "Test".

----------
### <a name="left"></a>Links

**Beschreibung:**  
Die Funktion links gibt eine angegebene Anzahl von Zeichen von der linken Seite einer Zeichenfolge zurück.

**Syntax:**  
`str Left(str string, num NumChars)`

- Zeichenfolge: Zeichenfolge zurückzugebenden Zeichen
- Länge: eine Identifikationsnummer die Anzahl der Zeichen vom Anfang der Zeichenfolge (links) zurück

**Beschreibung:**  
Eine Zeichenfolge mit der ersten NumChars Zeichen in der Zeichenfolge:

- Wenn Länge = 0, leere Zeichenfolge zurück.
- Wenn Länge < 0 Eingabezeichenfolge zurück.
- Zurückgeben Sie leere Zeichenfolge, wenn die Zeichenfolge null ist.

Zeichenfolge weniger Zeichen als Zahlen im angegebenen Länge enthält, wird eine Zeichenfolge mit Zeichenfolge (die enthält alle Zeichen in Parameter 1) zurückgegeben.

**Beispiel:**  
`Left("John Doe", 3)`  
Gibt "Joh".

----------
### <a name="len"></a>Len

**Beschreibung:**  
Die Len-Funktion gibt die Anzahl der Zeichen in einer Zeichenfolge.

**Syntax:**  
`num Len(str value)`

**Beispiel:**  
`Len("John Doe")`  
Gibt 8

----------
### <a name="ltrim"></a>LTrim

**Beschreibung:**  
Die Funktion LTrim entfernt führende Leerzeichen aus einer Zeichenfolge.

**Syntax:**  
`str LTrim(str value)`

**Beispiel:**  
`LTrim(" Test ")`  
Gibt "Test"

----------
### <a name="mid"></a>Mitte

**Beschreibung:**  
Die Mid-Funktion gibt eine angegebene Anzahl von Zeichen aus einer angegebenen Position in einer Zeichenfolge zurück.

**Syntax:**  
`str Mid(str string, num start, num NumChars)`

- Zeichenfolge: Zeichenfolge zurückzugebenden Zeichen
- Starten: eine Identifikationsnummer Start-position innerhalb der Zeichenfolge in Zeichen zurück
- Länge: eine Nummer zur Identifizierung der Zeichenanzahl Position Zeichenfolge zurückgegeben

**Beschreibung:**  
Zurückgeben Sie Länge Zeichen ab der Startposition in Zeichenfolge.  
Eine Zeichenfolge mit Länge Zeichen ab der Position Start Zeichenfolge:

- Wenn Länge = 0, leere Zeichenfolge zurück.
- Wenn Länge < 0 Eingabezeichenfolge zurück.
- Wenn start > die Länge der Zeichenfolge zurückgeben Eingabezeichenfolge.
- Wenn start < = 0, Eingabezeichenfolge zurück.
- Zurückgeben Sie leere Zeichenfolge, wenn die Zeichenfolge null ist.

Wenn nicht NumChar Zeichen im String ab der Position Start möglichst viele Zeichen zurückgegeben werden.

**Beispiel:**  
`Mid("John Doe", 3, 5)`  
Gibt "hn Do".

`Mid("John Doe", 6, 999)`  
Gibt "Doe"

----------
### <a name="now"></a>Jetzt

**Beschreibung:**  
Die Funktion jetzt gibt eine DateTime das aktuelle Datum und die Uhrzeit gemäß Systemdatum und-Uhrzeit Ihres Computers angibt.

**Syntax:**  
`dt Now()`

----------
### <a name="numfromdate"></a>NumFromDate

**Beschreibung:**  
Die NumFromDate-Funktion gibt ein Datum im Datumsformat anzeigen.

**Syntax:**  
`num NumFromDate(dt value)`

**Beispiel:**  
`NumFromDate(CDate("2012-01-01 23:00:00"))`  
Gibt 129699324000000000

----------
### <a name="padleft"></a>PadLeft

**Beschreibung:**  
Die PadLeft Funktion links-Pads eine Zeichenfolge mit einer angegebenen Länge mit bereitgestellten Auffüllzeichen.

**Syntax:**  
`str PadLeft(str string, num length, str padCharacter)`

- Zeichenfolge: Zeichenfolge aufgefüllt.
- Länge: eine Ganzzahl für die gewünschte Länge der Zeichenfolge.
- PadCharacter: eine Zeichenfolge mit einem einzelnen Zeichen, die als Füllzeichen verwendet

**Beschreibung:**

- Wenn die Länge der Zeichenfolge Länge unterschreitet, wird dann PadCharacter wiederholt am Anfang (links) der Zeichenfolge erst eine Länge.
- PadCharacter kann ein Leerzeichen kann nicht, aber es einen null-Wert.
- Wenn die Länge der Zeichenfolge gleich oder größer als die Länge ist, wird String unverändert zurückgegeben.
- Wenn Zeichenfolge eine Länge größer oder gleich Länge besitzt, wird eine Zeichenfolge mit Zeichenfolge zurückgegeben.
- Wenn die Länge der Zeichenfolge Länge unterschreitet, wird eine neue Zeichenfolge der gewünschten Länge mit aufgefüllt mit einem PadCharacter Zeichenfolge zurückgegeben.
- Wenn die Zeichenfolge null ist, gibt die Funktion eine leere Zeichenfolge.

**Beispiel:**  
`PadLeft("User", 10, "0")`  
Gibt "000000User".

----------
### <a name="padright"></a>PadRight

**Beschreibung:**  
Die PadRight Funktion rechts-Pads eine Zeichenfolge mit einer angegebenen Länge mit bereitgestellten Auffüllzeichen.

**Syntax:**  
`str PadRight(str string, num length, str padCharacter)`

- Zeichenfolge: Zeichenfolge aufgefüllt.
- Länge: eine Ganzzahl für die gewünschte Länge der Zeichenfolge.
- PadCharacter: eine Zeichenfolge mit einem einzelnen Zeichen, die als Füllzeichen verwendet

**Beschreibung:**

- Wenn die Länge der Zeichenfolge Länge unterschreitet, wird PadCharacter wiederholt angefügt zum Ende der Zeichenfolge (rechts) bis Länge Länge hat.
- PadCharacter kann ein Leerzeichen kann nicht, aber es einen null-Wert.
- Wenn die Länge der Zeichenfolge gleich oder größer als die Länge ist, wird String unverändert zurückgegeben.
- Wenn Zeichenfolge eine Länge größer oder gleich Länge besitzt, wird eine Zeichenfolge mit Zeichenfolge zurückgegeben.
- Wenn die Länge der Zeichenfolge Länge unterschreitet, wird eine neue Zeichenfolge der gewünschten Länge mit aufgefüllt mit einem PadCharacter Zeichenfolge zurückgegeben.
- Wenn die Zeichenfolge null ist, gibt die Funktion eine leere Zeichenfolge.

**Beispiel:**  
`PadRight("User", 10, "0")`  
Gibt "User000000".

----------
### <a name="pcase"></a>PCase

**Beschreibung:**  
Die PCase-Funktion konvertiert das erste Zeichen aller Leerzeichen getrennte Wörter in einer Zeichenfolge in Großbuchstaben und alle anderen Zeichen werden in Kleinbuchstaben konvertiert.

**Syntax:**  
`String PCase(string)`

**Beschreibung:**

- Diese Funktion bietet derzeit keine richtige Schreibweise ein Wortes zu konvertieren, die ausschließlich aus Großbuchstaben besteht, z. B. ein Akronym.

**Beispiel:**  
`PCase("TEsT")`  
Gibt "Test".

`PCase(LCase("TEST"))`  
Gibt "Test"

----------
### <a name="randomnum"></a>RandomNum

**Beschreibung:**  
Die RandomNum-Funktion gibt eine Zufallszahl zwischen einem angegebenen Intervall.

**Syntax:**  
`num RandomNum(num start, num end)`

- Starten: eine Identifikationsnummer Untergrenze zufälligen Wert generieren
- Ende: eine Identifikationsnummer der Obergrenze von zufälligen Wert generieren

**Beispiel:**  
`Random(100,999)`  
734 können zurückgegeben werden.

----------
### <a name="removeduplicates"></a>RemoveDuplicates

**Beschreibung:**  
RemoveDuplicates-Funktion eine mehrwertige Zeichenfolge und stellen Sie sicher, dass jeder Wert eindeutig ist.

**Syntax:**  
`mvstr RemoveDuplicates(mvstr attribute)`

**Beispiel:**  
`RemoveDuplicates([proxyAddresses])`  
Ein sicherer ProxyAddress Attribut zurückgegeben, wurden alle doppelten Werte entfernt.

----------
### <a name="replace"></a>Ersetzen

**Beschreibung:**  
Die Funktion Replace ersetzt alle Vorkommen einer Zeichenfolge in einer anderen Zeichenfolge.

**Syntax:**  
`str Replace(str string, str OldValue, str NewValue)`

- Zeichenfolge: eine Zeichenfolge Werte ersetzen.
- OldValue: Die Zeichenfolge zu suchen und zu ersetzen.
- Neuer Wert: Die Zeichenfolge zu ersetzen.

**Beschreibung:**  
Die Funktion erkennt die folgenden speziellen Moniker:

- \n – neue Zeile
- \r – Wagenrücklauf
- \t – Registerkarte

**Beispiel:**  
`Replace([address],"\r\n",", ")`  
CRLF durch ein Komma und ein Leerzeichen ersetzt und könnte zu "One-Microsoft Möglichkeit, Redmond, WA, USA"

----------
### <a name="replacechars"></a>ReplaceChars

**Beschreibung:**  
Die ReplaceChars-Funktion ersetzt alle Vorkommen von Zeichen in der Zeichenfolge ReplacePattern gefunden.

**Syntax:**  
`str ReplaceChars(str string, str ReplacePattern)`

- Zeichenfolge: eine Zeichenfolge, die Zeichen ersetzt.
- ReplacePattern: eine Zeichenfolge, die ein Wörterbuch mit der zu ersetzenden Zeichen enthält.

Das Format ist {source1}: {target1} {source2}: {target2} {SourceN} {TargetN}, Quelle Zeichen suchen und ersetzen die Zeichenfolge ist.

**Beschreibung:**

- Die Funktion wird jedes Vorkommen des definierten Quellen und Ziele ersetzt.
- Die Quelle muss genau ein Zeichen (Unicode).
- Die Quelle kann nicht länger als ein Zeichen (Parserfehler) sein.
- Das Ziel haben mehrere Zeichen, z. B. Ö:oe, β:ss.
- Das Ziel kann leer, dass das Zeichen entfernt werden soll.
- Die Quelle wird Groß-/Kleinschreibung und muss eine genaue Übereinstimmung.
- (Komma) und: (Doppelpunkt) sind reservierte Zeichen und kann nicht mit dieser Funktion ersetzt werden.
- Leerzeichen und andere weißen Zeichen in der Zeichenfolge ReplacePattern werden ignoriert.

**Beispiel:**  
`%ReplaceString% = ’:,Å:A,Ä:A,Ö:O,å:a,ä:a,ö,o`

`ReplaceChars("Räksmörgås",%ReplaceString%)`  
Gibt Raksmorgas zurück

`ReplaceChars("O’Neil",%ReplaceString%)`  
Gibt "ONeil", das Häkchen wird entfernt definiert.

----------
### <a name="right"></a>Richting

**Beschreibung:**  
Die richtige Funktion gibt eine angegebene Anzahl von Zeichen von rechts (Ende) einer Zeichenfolge.

**Syntax:**  
`str Right(str string, num NumChars)`

- Zeichenfolge: Zeichenfolge zurückzugebenden Zeichen
- Länge: eine Identifikationsnummer der Anzahl von Zeichen vom Ende der Zeichenfolge (rechts) zurückgegeben

**Beschreibung:**  
Länge Zeichen werden von der letzten Position der Zeichenfolge zurückgegeben.

Eine Zeichenfolge, die die Länge Zeichen in Zeichenfolge enthält:

- Wenn Länge = 0, leere Zeichenfolge zurück.
- Wenn Länge < 0 Eingabezeichenfolge zurück.
- Zurückgeben Sie leere Zeichenfolge, wenn die Zeichenfolge null ist.

Wenn Zeichenfolge weniger Zeichen als Zahlen im angegebenen Länge enthält, wird eine Zeichenfolge mit Zeichenfolge zurückgegeben.

**Beispiel:**  
`Right("John Doe", 3)`  
Gibt "Doe".

----------
### <a name="rtrim"></a>RTrim

**Beschreibung:**  
Die Funktion RTrim entfernt nachgestellte Leerzeichen aus einer Zeichenfolge.

**Syntax:**  
`str RTrim(str value)`

**Beispiel:**  
`RTrim(" Test ")`  
Gibt "Test".

----------
### <a name="split"></a>Teilen

**Beschreibung:**  
Split-Funktion akzeptiert eine durch Trennzeichen getrennte Zeichenfolge und macht eine mehrwertige Zeichenfolge.

**Syntax:**  
`mvstr Split(str value, str delimiter)`  
`mvstr Split(str value, str delimiter, num limit)`

- Wert: die Zeichenfolge mit einem Trennzeichen trennen.
- Trennzeichen: Zeichen als Trennzeichen verwendet werden.
- Limit: maximale Anzahl von Werten zurückgeben kann.

**Beispiel:**  
`Split("SMTP:john.doe@contoso.com,smtp:jd@contoso.com",",")`  
Gibt eine mehrwertige Zeichenfolge mit 2 Elementen für ProxyAddress Attribut.

----------
### <a name="stringfromguid"></a>StringFromGuid

**Beschreibung:**  
Die StringFromGuid-Funktion akzeptiert eine binäre GUID und konvertiert es in eine Zeichenfolge

**Syntax:**  
`str StringFromGuid(bin GUID)`

----------
### <a name="stringfromsid"></a>StringFromSid

**Beschreibung:**  
Die StringFromSid-Funktion konvertiert ein Bytearray mit einer Sicherheits-ID in eine Zeichenfolge.

**Syntax:**  
`str StringFromSid(bin ObjectSID)`  

----------
### <a name="switch"></a>Schalter

**Beschreibung:**  
Der Switch-Funktion wird einen einzelnen anhand ausgewerteten Wert zurückgeben.

**Syntax:**  
`var Switch(exp expr1, var value1[, exp expr2, var value … [, exp expr, var valueN]])`

- Ausdruck: Variant-Ausdruck ausgewertet werden soll.
- Wert: Wert zurückgegeben wird, wenn der entsprechende Ausdruck True ist.

**Beschreibung:**  
Die Switch-Funktion Argumentliste besteht aus Paaren von Ausdrücken und Werten. Die Ausdrücke werden von links nach rechts ausgewertet, und der erste Ausdruck als True ausgewertet zugeordnete Wert zurückgegeben. Wenn Teile nicht richtig paarweise angegeben werden, tritt ein Laufzeitfehler auf.

Wenn Ausdr1 wahr ist, gibt Switch beispielsweise Wert1 zurück. Wenn Ausdr-1 False ist Ausdr-2 gilt jedoch gibt Switch Wert 2 und So weiter.

Switch gibt NULL zurück, wenn:

- Keiner der Ausdrücke sind True.
- Der erste Ausdruck True hat einen entsprechenden Wert Null ist.

Switch alle Ausdrücke ausgewertet, auch wenn nur eine zurückgegeben. Aus diesem Grund sollten Sie auf unerwünschte Nebeneffekte achten. Beispielsweise, wenn die Auswertung eines Ausdrucks aufgrund einer Division durch Null führt, tritt ein Fehler auf.

Wert kann auch eine benutzerdefinierte Zeichenfolge zurückgeben würde die Funktion Fehler sein.

**Beispiel:**  
`Switch([city] = "London", "English", [city] = "Rome", "Italian", [city] = "Paris", "French", True, Error("Unknown city"))`  
Gibt die Sprache in einigen Städten, andernfalls wird einen Fehler zurückgegeben.

----------
### <a name="trim"></a>Zuschneiden

**Beschreibung:**  
Die Funktion GLÄTTEN entfernt führende und nachfolgende Leerzeichen aus einer Zeichenfolge.

**Syntax:**  
`str Trim(str value)`  

**Beispiel:**  
`Trim(" Test ")`  
Gibt "Test".

`Trim([proxyAddresses])`  
Entfernt führende und nachfolgende Leerzeichen für jeden Wert im Attribut ProxyAddress.

----------
### <a name="ucase"></a>UCase

**Beschreibung:**  
Die UCase-Funktion konvertiert alle Zeichen in einer Zeichenfolge in Großbuchstaben.

**Syntax:**  
`str UCase(str string)`

**Beispiel:**  
`UCase("TeSt")`  
Gibt "TEST".

----------
### <a name="word"></a>Word

**Beschreibung:**  
Die Word-Funktion gibt ein Wort in einer Zeichenfolge auf Grundlage von Parametern Trennzeichen und die Anzahl von zurückzugebenden Beschreibung enthalten.

**Syntax:**  
`str Word(str string, num WordNumber, str delimiters)`

- Zeichenfolge: Zeichenfolge, die das Wort zurückgegeben.
- WordNumber: eine Nummer zur Identifizierung der Anzahl sollte zurückgeben.
- Trennzeichen: eine Zeichenfolge, die Wörter zu verwendenden Delimiter(s)

**Beschreibung:**  
Jede Zeichenfolge in Zeichenfolge, durch die Zeichen in Trennzeichen getrennt werden als Wörter identifiziert:

- Wenn Zahl < 1, gibt eine leere Zeichenfolge.
- Wenn die Zeichenfolge null ist, zurückgegeben leere Zeichenfolge.

Wenn Zeichenfolge weniger als die Anzahl der Wörter enthält Zeichenfolge keine Wörter durch Trennzeichen enthält, wird eine leere Zeichenfolge zurückgegeben.

**Beispiel:**  
`Word("The quick brown fox",3," ")`  
Gibt "brown"

`Word("This,string!has&many separators",3,",!&#")`  
"Hat" zurück

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Grundlegendes zur Bereitstellung deklarative Ausdrücken](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)
* [Azure AD Verbindung synchronisieren: Anpassen von Synchronisierungsoptionen](active-directory-aadconnectsync-whatis.md)
* [Integration von lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)
