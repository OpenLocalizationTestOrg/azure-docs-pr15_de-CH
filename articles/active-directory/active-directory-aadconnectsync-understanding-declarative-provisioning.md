<properties
    pageTitle="Azure AD Verbindung synchronisieren: Understanding deklarative Bereitstellung | Microsoft Azure"
    description="Erläutert das deklarative Bereitstellung Konfigurationsmodell in Azure AD verbinden."
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
    ms.date="08/29/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-understanding-declarative-provisioning"></a>Azure AD Verbindung synchronisieren: Understanding deklarative Bereitstellung
Dieses Thema erklärt Konfigurationsmodell in Azure AD verbinden. Das Modell heißt deklarative Bereitstellung und können Konfiguration problemlos ändern. Viele Dinge in diesem Thema beschriebenen erweiterten und für den Kunden nicht erforderlich.

## <a name="overview"></a>Übersicht
Deklarative Objekte aus einem verbundenen Quellverzeichnis verarbeitet und bestimmt, wie Objekte und Attribute aus einer Quelle in ein Ziel umgewandelt werden soll. Ein Objekt in einer Pipeline synchron verarbeitet und die Pipeline wird für eingehenden und ausgehenden Regeln. Eine eingehende Regel aus einem Connector in das Metaversum und eine ausgehende Regel aus Metaverse einen Connectorspace.

![Sync-pipeline](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/sync1.png)  

Die Pipeline hat mehrere unterschiedliche Module. Jeder ist ein Konzept in Synchronisierung.

![Sync-pipeline](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/pipeline.png)  

- Das Quellobjekt Quelle
- [Bereich](#scope)findet alle Synchronisierungsregeln im Bereich
- [Verknüpfen](#join), bestimmt die Beziehung zwischen Connectorspace- und metaverse
- [Wie Attribute transformiert sollten, berechnet und Datenfluss](#transform)
- [Vorrang](#precedence)löst Konflikte Attribut Beiträge
- Ziel des Zielobjekts

## <a name="scope"></a>Gültigkeitsbereich
Scope-Modul prüft ein Objekt und bestimmt die Regeln, die im Umfang und in die Verarbeitung eingeschlossen sein. Je nach der Attributwerte für das Objekt werden verschiedene Synchronisierungsregeln im Bereich ausgewertet. Beispielsweise verfügt ein deaktivierter Benutzer mit Exchange-Postfach andere Regeln als ein Benutzer mit einem Postfach aktiviert.  
![Gültigkeitsbereich](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/scope1.png)  

Der Bereich wird als Gruppen und Klauseln definiert. Die Klauseln sind innerhalb einer Gruppe. Ein logisches AND wird zwischen allen Klauseln in einer Gruppe verwendet. Zum Beispiel (Department = und Land = Dänemark). Ein logisches OR wird zwischen Gruppen verwendet.

![Gültigkeitsbereich](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/scope2.png)  
Der Bereich im Bild sollte als gelesen (Department = und Land = Dänemark) oder (Land = Schwedens). Wenn True, dann die Regel entweder Gruppe 1 oder 2 ausgewertet ist.

Scope-Modul unterstützt die folgenden Vorgänge.

Vorgang | Beschreibung
--- | ---
EQUAL, NOTEQUAL | Eine Zeichenfolge vergleichen, die sich ergibt, wenn der Wert gleich dem Wert im Attribut ist. Mehrwertige Attribute finden Sie unter ISIN und ISNOTIN.
LESSTHAN LESSTHAN_OR_EQUAL | Eine Zeichenfolge vergleichen, die sich ergibt, wenn der Wert kleiner als der Wert im Attribut.
ENTHÄLT NOTCONTAINS | Eine Zeichenfolge vergleichen, die sich ergibt, wenn der Wert irgendwo im Wert im Attribut gefunden werden kann.
"STARTSWITH", NOTSTARTSWITH | Eine Zeichenfolge vergleichen, die sich ergibt, wenn der Wert der Wert des Attributs ist.
ENDSWITH, NOTENDSWITH | Eine Zeichenfolge vergleichen, die sich ergibt, wenn der Wert am Ende des Werts im Attribut.
GREATERTHAN, GREATERTHAN_OR_EQUAL | Eine Zeichenfolge vergleichen, die sich ergibt, wenn der Wert größer als der Wert im Attribut ist.
ISNULL ISNOTNULL | Ausgewertet wird, fehlt das Attribut des Objekts. Wenn das Attribut nicht vorhanden ist und daher null ist, ist die Regel im Bereich.
ISIN / ISNOTIN | Ergibt, wenn der Wert im definierten Attribut vorhanden ist. Dieser Vorgang ist mehrwertig Variante gleich und NOTEQUAL. Das Attribut soll ein mehrwertiges Attribut, und wenn der Wert im Attribut Werte gefunden werden, die Regel wird im Bereich.
ISBITSET ISNOTBITSET | Ergibt, wenn ein bestimmtes Bit festgelegt ist. Beispielsweise dienen zum Auswerten der Bits in UserAccountControl um festzustellen, ob ein Benutzer aktiviert oder deaktiviert ist.
ISMEMBEROF ISNOTMEMBEROF | Der Wert sollte einer Gruppe im Connectorspace DN. Wenn das Objekt ein Mitglied der angegebenen Gruppe ist, ist die Regel im Bereich.

## <a name="join"></a>Verknüpfung
Join-Modul in der Pipeline Sync ist die Beziehung zwischen dem Objekt in der Quelle und ein Objekt im Ziel finden. Eine eingehende Regel wäre diese Beziehung eines Objekts in einem Connectorspace finden eine Beziehung zu einem Objekt im Metaverse.  
![Join zwischen Cs und mv](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join1.png)  
Das Ziel ist zu sehen, ist ein Objekt bereits im Metaverse einen anderen Connector, erstellt er zugeordnet werden soll. Beispielsweise sollte ein Konto Ressourcengesamtstruktur Benutzer der Kontengesamtstruktur mit dem Benutzer aus der Gesamtstruktur angehören.

Joins werden meist bei eingehenden Regeln zum Connectorspace-Objekte auf das gleiche metaverseobjekt verbinden.

Joins werden eine oder mehrere Gruppen definiert. Innerhalb einer Gruppe können Sie Klauseln. Ein logisches AND wird zwischen allen Klauseln in einer Gruppe verwendet. Ein logisches OR wird zwischen Gruppen verwendet. Die Gruppen werden in der Reihenfolge von oben nach unten verarbeitet. Wenn eine Gruppe genau eine Übereinstimmung mit einem Objekt im Ziel gefunden hat, werden keine anderen Join Regeln ausgewertet. Wenn NULL oder mehr als ein Objekt gefunden wurde, weiter verarbeitet die nächste Gruppe von Regeln Aus diesem Grund sollte die Regeln in expliziten zuerst erstellt und am Ende mehr fuzzy.  
![Join-definition](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join2.png)  
Joins in dieser Abbildung werden von oben nach unten verarbeitet. Zuerst sieht Sync-Pipeline ist eine Übereinstimmung für EmployeeID. Wenn nicht, die zweite Regel überprüft, ob der Kontoname Objekte gemeinsam verwendet werden kann. Die keine Übereinstimmung ist, ist die dritte Regel mehr Fuzzyübereinstimmung anhand des Namens des Benutzers.

Wenn alle Join Regeln ausgewertet wurde nicht genau eine Übereinstimmung vorhanden ist, wird auf der Seite **Beschreibung** der **Linktyp** verwendet. Wenn diese Option **Bestimmung**festgelegt ist, wird ein neues Objekt in das Ziel erstellt.  
![Bereitstellung oder Verknüpfung](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join3.png)  

Ein Objekt muss nur eine einzige Synchronisierungsregel Join Regeln im Bereich. Gibt mehrere Synchronisierungsregeln, Verknüpfung definiert ist, tritt ein Fehler auf. Vorrang wird kein Join-Konflikte. Ein Objekt muss eine Join-Regel im Bereich Attribute mit der gleichen eingehende und ausgehende Richtung fließen. Wenn Attribute ein- und ausgehenden auf dasselbe Objekt erforderlich, müssen Sie eine eingehende und eine ausgehende Synchronisierungsnachrichten Regel mit.

Ausgehende Verbindung weist ein besonderes Verhalten, wenn es versucht, ein Objekt einer Ziel-Connectorspace bereitgestellt. DN-Attribut wird zum Ausprobieren Reverse-Join. Es ist bereits ein Objekt im Connectorspace Ziel mit dem gleichen DN, werden die Objekte hinzugefügt.

Join-Modul wird nur ausgewertet, kommt als ein Sync-Neuregelung Bereich. Wenn ein Objekt verknüpft wurde, ist nicht getrennt, auch wenn die Verknüpfungskriterien nicht mehr erfüllt. Wenn Sie ein Objekt entfernen möchten, muss die Synchronisierungsregel, die die Objekte Gültigkeitsbereich wechseln.

### <a name="metaverse-delete"></a>Metaverse löschen
Ein Metaverse-Objekt bleibt so lange Regelsatznummer eine Synchronisierung im Bereich mit **Link** **Bereitstellen** oder **StickyJoin**. Ein StickyJoin wird verwendet, wenn eine Verbindung nicht zulässig ist, ein neues Objekt in das Metaversum Bereitstellung jedoch beigetreten ist, er muss in der Quelle gelöscht werden, bevor das metaverseobjekt gelöscht wird.

Wenn ein Metaverse-Objekt gelöscht wird, werden alle Objekte mit einer **Bereitstellung** markiert ausgehende Synchronisierungsnachrichten Regel zum Löschen markiert.

## <a name="transformations"></a>Transformationen
Transformationen dienen zum definieren, wie Attribute von der Quelle zum Ziel übertragen werden soll. Die Flüsse haben folgenden **Flusstypen**: direkt, Konstante oder Ausdruck. Ein direkter Fluss fließt als Attributwert-ohne zusätzliche Transformationen ist. Ein konstanter Wert legt den angegebenen Wert. Ein Ausdruck verwendet deklarative Bereitstellung Ausdruckssprache Ausdrücken wie die Transformation werden sollen. Einzelheiten der Expression-Sprache finden im Thema [deklarative Bereitstellung Ausdruckssprache verstehen](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) .

![Bereitstellung oder Verknüpfung](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/transformations1.png)  

Das Kontrollkästchen **einmal** definiert, dass das Attribut sollte nur festgelegt werden, wenn das Objekt erstellt wird. Diese Konfiguration kann beispielsweise verwendet werden, ein Ausgangskennwort für ein neues Benutzerobjekt festgelegt.

### <a name="merging-attribute-values"></a>Zusammenführen von Attributwerten
Gelder Attribut ist eine Einstellung, um festzustellen, ob mehrwertige Attribute aus mehreren verschiedenen Connectors zusammengeführt werden sollen. Der Standardwert ist **Update**, womit Synchronisierungsregel mit der höchsten Priorität gewinnt.

![Zusammenführen von Typen](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/mergetype.png)  

Es gibt auch **Zusammenführen** und **MergeCaseInsensitive**. Mit diesen Optionen können Sie Werte aus unterschiedlichen Quellen zusammenführen. Beispielsweise kann verwendet werden, Mitglied oder ProxyAddresses-Attribut aus mehreren unterschiedlichen Gesamtstrukturen zusammenführen. Wenn Sie diese Option verwenden, müssen alle Synchronisierungsregeln für ein Objekt im Bereich derselben Seriendruck verwenden. **Aktualisieren** von einem Anschluss und von einem anderen **Zusammenführen** können nicht definiert werden. Wenn Sie versuchen, erhalten Sie eine Fehlermeldung.

Der Unterschied zwischen **Zusammenführen** und **MergeCaseInsensitive** ist wie doppelte Werte. Das Synchronisierungsmodul wird sichergestellt, dass doppelte Werte nicht in das Zielattribut eingefügt werden. Mit **MergeCaseInsensitive**doppelte Werte mit nur bei nicht vorhanden sein werden. Beispielsweise sollten nicht angezeigt beide "SMTP:bob@contoso.com" und "smtp:bob@contoso.com" im Zielattribut. **Zusammenführen** nur sucht die genaue und mehrere Werte besteht nur ein Unterschied im Fall vorhanden wäre.

Die Option **Ersetzen** ist **identisch**, aber nicht verwendet.

### <a name="control-the-attribute-flow-process"></a>Steuern des Nachrichtenflusses Attribut
Wenn mehrere eingehende Synchronisierungsregeln Beitrag zur gleichen Metaverse-Attribut konfiguriert sind, wird Vorrang vor den Gewinner zu bestimmen. Synchronisierungsregel mit der höchsten Priorität (dem niedrigsten numerischen Wert) wird den Wert beitragen. Das gleiche gilt für ausgehende Regeln. Die Synchronisierung mit der höchsten Priorität gewinnt Regel und den Wert an das verbundene Verzeichnis beitragen.

In einigen Fällen als contribute einen Wert ermitteln Synchronisierungsregel Verhalten anderen Regeln. Es gibt einige spezielle Literale für diese Anfrage verwendet.

Für eingehende Synchronisierungsregeln kann das literal- **NULL** verwendet werden um anzugeben, dass der Fluss kein Wert beitragen. Eine andere Regel mit einer niedrigeren Priorität kann einen Wert beitragen. Wenn keine Regel einen Wert bereitgestellt, wird das Metaverse-Attribut entfernt. Für eine ausgehende Regel Wenn **NULL** der Endwert ist nach der Verarbeitung aller Synchronisierungsregeln wird der Wert im verbundenen Verzeichnis entfernt.

Das literal **AuthoritativeNull** entspricht **NULL** jedoch mit dem Unterschied, dass kein niedrigeren Prioritätsregeln Wert beitragen können.

Ein Attributfluss können auch **IgnoreThisFlow**. Es ist mit NULL im Sinne gibt an, dass keine beitragen. Der Unterschied ist, dass keine bereits vorhandenen Wert im Ziel entfernt werden. Es ist wie der Attributfluss es nie.

Hier ist ein Beispiel:

In *AD - Benutzer Exchange Hybrid Out* folgenden Schritten finden:  
`IIF([cloudSOAExchMailbox] = True,[cloudMSExchSafeSendersHash],IgnoreThisFlow)`  
Dieser Ausdruck sollte als gelesen: befindet sich das Postfach in Azure AD, übertragen Sie das Attribut von Azure AD AD. Wenn dies nicht der Fall ist, fließen nicht zu Active Directory. In diesem Fall würde es den vorhandenen Wert in Active Directory gespeichert.

### <a name="importedvalue"></a>ImportedValue
Die Funktion ImportedValue unterscheidet sich andere Funktionen seit der Attributnamen in Anführungszeichen statt in Klammern eingeschlossen werden muss:  
`ImportedValue("proxyAddresses")`.

In der Regel während der Synchronisierung verwendet ein Attribut der erwartete Wert, auch wenn noch exportiert wurde noch nicht oder Fehlermeldung beim Export ("Top Turm"). Eingehende Synchronisierung wird davon ausgegangen, dass ein Attribut, das noch ein verbundenes Verzeichnis schließlich erreicht hat erreicht. In einigen Fällen ist es wichtig, nur einen Wert zu synchronisieren, der vom verbundenen Verzeichnis ("Hologramm und Delta import Tower) bestätigt.

Ein Beispiel für diese Funktion finden Sie in der Regel der Out-of-Box-Synchronisierung *In aus AD-Benutzer häufig von Exchange*. Hybrid-Exchange sollte der Mehrwert Exchange online nur synchronisiert werden, wenn bestätigt wurde, dass der Wert erfolgreich exportiert wurde:  
`proxyAddresses` <- `RemoveDuplicates(Trim(ImportedValue("proxyAddresses")))`

## <a name="precedence"></a>Rangfolge
Wenn mehrere Synchronisierungsregeln versuchen, die Attributwerte für das Ziel beitragen, dient Priorität zu ermitteln. Die Regel mit höchster Priorität niedrigsten numerischen Wert wird dazu das Attribut in Konflikt.

![Zusammenführen von Typen](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/precedence1.png)  

Diese Reihenfolge kann verwendet werden, genauere attributflüsse für eine kleine Teilmenge der Objekte definieren. Beispielsweise achten Out-von-Feld-Regeln Attribute ein aktiviertes Konto (**User AccountEnabled**) Vorrang vor anderen Konten.

Rangfolge kann zwischen Connectors definiert werden. Dadurch Connectors mit bessere Werte zuerst beitragen.

### <a name="multiple-objects-from-the-same-connector-space"></a>Mehrere Objekte gleichen Connectorspace
Wenn Sie mehrere Objekte im gleichen Connectorspace dasselbe metaverseobjekt hinzugefügt haben, muss Vorrang angepasst werden. Wenn mehrere im Bereich der Synchronisierungsregel Objekte ist das Synchronisierungsmodul nicht Vorrang zu ermitteln. Es ist nicht eindeutig die Quellobjekt Wert dem Metaverse beitragen. Diese Konfiguration wird als mehrdeutig gemeldet, auch wenn die Attribute im Feld denselben Wert haben.  
![Mehrere Objekte mit derselben Mv-Objekt verbunden](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/multiple1.png)  

Für dieses Szenario müssen Sie den Bereich Sync-Regeln ändern, haben die Objekte unterschiedliche Synchronisierungsregeln im Bereich. Dadurch werden andere Rangfolge zu definieren.  
![Mehrere Objekte mit derselben Mv-Objekt verbunden](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/multiple2.png)  

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen über Ausdruckssprache [Verständnis deklarative Bereitstellung Ausdrücke](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).
- Siehe wie deklarative Bereitstellung verwendeten Out-of-Box Verständnis [der Standardkonfiguration](active-directory-aadconnectsync-understanding-default-configuration.md)ist.
- Finden Sie unter praktische mit deklarativen Bereitstellung wie [Ändern Sie die Standardkonfiguration](active-directory-aadconnectsync-change-the-configuration.md)ändern.
- Weiter lesen, wie Benutzer und Kontakte in [Benutzer verstehen und Kontakte](active-directory-aadconnectsync-understanding-users-and-contacts.md)zusammenarbeiten.

**Themen (Übersicht)**

- [Azure AD Verbindung synchronisieren: verstehen und Synchronisation anpassen](active-directory-aadconnectsync-whatis.md)
- [Integration von lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)

**Themen**

- [Azure AD Verbindung synchronisieren: Funktionen Verweis](active-directory-aadconnectsync-functions-reference.md)
