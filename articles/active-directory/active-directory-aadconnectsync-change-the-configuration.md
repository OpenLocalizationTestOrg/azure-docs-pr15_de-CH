<properties
    pageTitle="Azure AD Verbindung synchronisieren: wie die Standardkonfiguration ändern | Microsoft Azure"
    description="Führt Sie durch das Ändern der Konfigurations in Azure AD-Verbindung synchronisieren."
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
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-how-to-make-a-change-to-the-default-configuration"></a>Azure AD Verbindung synchronisieren: wie Sie die Konfiguration ändern
Dieses Thema ist durchlaufen wie Ändern der Standardkonfiguration in Azure AD-Verbindung synchronisieren. Diese Anleitung für einige häufige Szenarien. Mit diesem wissen sollten Sie einfach zur Konfiguration basierend auf Ihren eigenen Geschäftsregeln ändern können.

## <a name="synchronization-rules-editor"></a>Synchronisierung Regel-Editor
Synchronisierung Regel-Editor wird zum Anzeigen und Ändern der Standardkonfiguration. Sie finden es im Startmenü unter **Azure AD Connect** -Gruppe.  
![Startmenü mit Sync-Regel-Editor](./media/active-directory-aadconnectsync-change-the-configuration/startmenu2.png)

Beim Öffnen, sehen Sie die Regeln für die Out-of-Box.

![Sync-Regel-Editor](./media/active-directory-aadconnectsync-change-the-configuration/sre2.png)

### <a name="navigating-in-the-editor"></a>Navigieren im editor
Dropdown-Liste oben Editor können Sie schnell eine bestimmte Regel suchen. Möchten Sie die Regeln sehen, wobei das Attribut ProxyAddresses enthalten, würden Sie Dropdown-Liste wie folgt ändern:  
![Filtern von SRE](./media/active-directory-aadconnectsync-change-the-configuration/filtering.png)  
Um das Filtern zurückzusetzen und eine neue Konfiguration zu laden, drücken Sie **F5** auf der Tastatur.

Oben rechts können Sie eine **neue Regel hinzufügen**-Schaltfläche. Diese Schaltfläche wird verwendet, um eigene benutzerdefinierte Regel erstellen.

Unten haben Sie Schaltflächen zum Bearbeiten in einer ausgewählten synchronisieren. **Bearbeiten** und **Löschen** , was sie erwarten. **Exportieren** erzeugt einer PowerShell-Skript neu Synchronisierungsregel. Dieses Verfahren können Sie eine Synchronisierungsregel von einem Server auf einen anderen verschieben.

## <a name="create-your-first-custom-rule"></a>Erstellen Sie die erste benutzerdefinierte Regel
Häufigste ist an das Attribut ändert. Die Daten im Quellverzeichnis möglicherweise nicht in Azure AD. Im Beispiel in diesem Abschnitt soll sicherstellen, dass der Vorname des Benutzers immer **Schreibweise**ist.

### <a name="disable-the-scheduler"></a>Deaktivieren des Planers
Der [Taskplaner](active-directory-aadconnectsync-feature-scheduler.md) wird standardmäßig alle 30 Minuten ausgeführt. Sie möchten sicherstellen, dass er nicht gestartet wird, und Sie ändern die neuen Regeln zu beheben. Deaktivieren Sie vorübergehend den Planer, PowerShell starten und ausführen`Set-ADSyncScheduler -SyncCycleEnabled $false`

![Deaktivieren des Planers](./media/active-directory-aadconnectsync-change-the-configuration/schedulerdisable.png)  

### <a name="create-the-rule"></a>Erstellen Sie die Regel

1. Klicken Sie auf **neue Regel hinzufügen**.
2. Geben Sie auf der Seite **Beschreibung** Folgendes ein:  
![Eingehende Regel filtern](./media/active-directory-aadconnectsync-change-the-configuration/description2.png)  
    - Name: Geben der Regel einen beschreibenden Namen.
    - Beschreibung: Klarheit damit jemand verstehen kann, was die Regel ist.
    - System verbunden: das Objekt Sie im finden System. In diesem Fall wählen Sie Active Directory Connector.
    - Verbundene System-Metaverse-Objekttyp: Wählen Sie **Benutzer** und **Person** .
    - Linktyp: Ändern Sie diesen Wert **an**.
    - Priorität: Geben Sie einen Wert, der im System eindeutig ist. Eine niedrigere Zahl gibt Vorrang.
    - Tags: Leer. Nur Out-of-Box-Regeln von Microsoft sollte dieses Feld mit einem Wert gefüllt haben.
3. Geben Sie auf der Seite **Filter Scoping** **GivenName ISNOTNULL**.  
![Eingehende Regel scoping filter](./media/active-directory-aadconnectsync-change-the-configuration/scopingfilter.png)  
In diesem Abschnitt wird verwendet, um die Objekte definieren, die die Regel angewendet werden soll. Wenn leer, gilt die Regel für alle Benutzerobjekte. Jedoch würde, Konferenzräume, Dienstkonten und andere nicht Personen Benutzerobjekte enthalten.
4. Lassen Sie auf **Regeln beitreten**leer.
5. Ändern Sie auf der Seite **Transformationen** der Wechselkurs des Flusstyps **Ausdruck**. Wählen Sie das Zielattribut **GivenName**und Quelle geben `PCase([givenName])`.
![Eingehende Regel Transformationen](./media/active-directory-aadconnectsync-change-the-configuration/transformations.png)  
Das Synchronisierungsmodul beachtet den Funktionsnamen und der Name des Attributs. Wenn Sie etwas eingeben, wird beim Hinzufügen der Regel eine Warnung angezeigt. Editor können Sie speichern und fortfahren, so hätte die Regel öffnen und korrigieren Sie die Regel.
6. Klicken Sie auf **Hinzufügen** , um die Regel zu speichern.

Die neue benutzerdefinierte Regel wird mit anderen Synchronisierungsregeln im System angezeigt.

### <a name="verify-the-change"></a>Überprüfen Sie die Änderung
Mit dieser neuen möchten Sie sicher ist und wirft nicht fehlerfrei. Je nach Anzahl der Objekte, die Sie sind zwei verschiedene Arten dieser Schritt.

1. Führen Sie eine vollständige Synchronisierung aller Objekte
2. Führen Sie eine Vorschau und vollständige Synchronisierung auf ein einzelnes Objekt

Starten Sie **Synchronisierungsdienst** im Startmenü. Die Schritte in diesem Abschnitt werden alle in diesem Tool.

1. **Vollständige Synchronisierung für alle Objekte**  
Wählen Sie oben **Connectors** . Identifizieren Sie der Connectors im vorherigen Abschnitt bei Active Directory-Domänendienste eine Änderung vorgenommen und auswählen. Wählen Sie **Ausführen** aus Aktionen und **Vollständige Synchronisierung** und **OK**.
![Vollständige Synchronisierung](./media/active-directory-aadconnectsync-change-the-configuration/fullsync.png)  
Die Objekte werden jetzt im Metaverse. Nun möchten das Objekt im Metaverse.

2. **Vorschau und vollständige Synchronisierung auf ein einzelnes Objekt**  
Wählen Sie oben **Connectors** . Identifizieren Sie der Connectors im vorherigen Abschnitt bei Active Directory-Domänendienste eine Änderung vorgenommen und auswählen. Wählen Sie **Suche Connectorspace**. Verwenden Sie Bereich, ein Objekt zu verwenden, um die Änderung testen möchten. Wählen Sie das Objekt, und klicken Sie auf **Vorschau**. Wählen Sie im Fenster neue **Commit Vorschau**.
![Commit-Vorschau](./media/active-directory-aadconnectsync-change-the-configuration/commitpreview.png)  
Die Änderung ist jetzt Metaverse verpflichtet.

**Sehen Sie sich das Objekt im metaverse**  
Sie möchten nun einige Beispielobjekte stellen sicher, dass der Wert erwartet wird und dass die Regel angewendet. Wählen Sie oben **Metaverse-Suche** . Fügen Sie alle Filter Sie die jeweiligen Objekte müssen hinzu. Öffnen Sie im Suchergebnis ein Objekt. Die Attributwerte betrachten und **Synchronisierungsregeln** -Spalte, die die Regel als auch überprüfen.  
![Metaverse-Suche](./media/active-directory-aadconnectsync-change-the-configuration/mvsearch.png)  
### <a name="enable-the-scheduler"></a>Aktivieren Sie die Planung
Wenn alles wie erwartet, können Sie den Planer erneut aktivieren. Ausführen von PowerShell `Set-ADSyncScheduler -SyncCycleEnabled $true`.

## <a name="other-common-attribute-flow-changes"></a>Andere allgemeine Flow Änderungen
Im vorherige Abschnitt beschrieben, wie ein Attributfluss ändern. In diesem Abschnitt werden einige Beispiele bereitgestellt. Die Schritte zum Erstellen die Synchronisierungsregel abgekürzt, aber finden Sie die vollständigen Schritte im vorherigen Abschnitt.

### <a name="use-another-attribute-than-the-default"></a>Verwenden Sie ein anderes Attribut als Standard
Bei Fabrikam ist eine Gesamtstruktur, lokale Alphabet für Vorname, Nachname und Anzeigename verwendet wird. Lateinische zeichendarstellung dieser Attribute finden in der Erweiterungsattribute. Beim Erstellen der globalen Adressliste in Azure AD und Office 365 Organisation diese Attribute verwendet werden.

Mit einer Standardkonfiguration sieht ein Objekt aus der lokalen Gesamtstruktur:  
![Attributfluss 1](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp1.png)

Zum Erstellen einer Regel mit anderen Attribut folgendermaßen Sie vor:

- Starten Sie **Synchronisierung Regel-Editor** aus dem Startmenü.
- Klicken Sie mit **eingehenden** markiertem Links auf die Schaltfläche **neue Regel hinzufügen**.
- Geben Sie einen Namen und eine Beschreibung der Regel. Wählen Sie lokalen Active Directory und die entsprechenden Objekttypen.  Wählen Sie **Linktyp** **beitreten**. Vorrang für die Kommissionierung für einer Zahl von einer anderen Regel verwendet wird. Out-of-Box-Regeln beginnen 100, also der Wert 50 in diesem Beispiel verwendet werden kann.
![Attributfluss 2](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp2.png)
- Bereich leer (d. h. gelten für alle Benutzerobjekte in der Gesamtstruktur).
- Join-Regeln leer (d. h. Regelaktionen Out-of-Box alle Joins behandeln).
- Transformationen erstellen Sie die folgenden Abläufe:  
![Attributfluss 3](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp3.png)
- Klicken Sie auf **Hinzufügen** , um die Regel zu speichern.
- **Synchronisierung Dienst**-Manager wechseln Wählen Sie **Connectors**Connector, wo wir die Regel hinzugefügt. Wählen Sie **Ausführen**und **vollständige Synchronisierung**. Eine vollständige Synchronisierung berechnet mit Regeln.

Dies ist das Ergebnis für ein Objekt mit dieser benutzerdefinierten Regel:  
![Attributfluss 4](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp4.png)

### <a name="length-of-attributes"></a>Länge der Attribute
Attribute sind standardmäßig indiziert werden und die maximale Länge 448 Zeichen. Wenn Sie Attribute, die möglicherweise mehr enthalten verwenden, stellen Sie sicher, dass Folgendes in den Attributfluss:  
`attributeName` <- `Left([attributeName],448)`

### <a name="changing-the-userprincipalsuffix"></a>Ändern der userPrincipalSuffix
UserPrincipalName-Attribut in Active Directory von den Benutzern nicht immer bekannt und kann nicht als die ID Azure AD verbinden können Sync-Installationsassistenten Kommissionierung ein anderes Attribut z. B. e-Mail. Aber in einigen Fällen müssen Sie das Attribut berechnet. Beispielsweise hat das Unternehmen Contoso zwei Azure AD-Verzeichnissen, Produktion und zum Testen. Sie möchten die Benutzer ihren Mandanten Test mit einem anderen Suffix in der ID  
`userPrincipalName` <- `Word([userPrincipalName],1,"@") & "@contosotest.com"`

Nehmen Sie in diesem Ausdruck alles links des ersten @-sign (Word) und eine feste Zeichenfolge verketten.

### <a name="convert-a-multi-value-to-a-single-value"></a>Mehrwertig in einen einzelnen Wert konvertieren
Einige Attribute in Active Directory sind mehrwertig im Schema, auch wenn sie einzelne Werte in Active Directory-Benutzer und -Computer. Ein Beispiel ist das Description-Attribut.  
`description` <- `IIF(IsNullOrEmpty([description]),NULL,Left(Trim(Item([description],1)),448))`

In diesem Ausdruck das Attribut einen Wert hat wir nehmen das erste Element (Item) im Attribut entfernt führende und nachstehende Leerzeichen (Trim) und halten Sie die ersten 448 Zeichen (links) in der Zeichenfolge.

### <a name="do-not-flow-an-attribute"></a>Ein Attribut nicht übertragen
Hintergrund für das Szenario in diesem Abschnitt finden Sie unter [Kontrolle des Nachrichtenflusses Attribut](active-directory-aadconnectsync-understanding-declarative-provisioning.md#control-the-attribute-flow-process).

Gibt es zwei Methoden, um ein Attribut nicht übertragen. Die erste ist im Installations-Assistenten und ermöglicht es Ihnen, [Ausgewählte Attribute entfernen](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering). Diese Option funktioniert das Attribut vor nie synchronisiert. Aber wenn Sie begonnen haben, dieses Attribut synchronisieren und diese Funktion später entfernen dann Sync Engine stoppt verwalten das Attribut und die vorhandenen Werte in Azure AD verbleiben.

Möchten Sie den Wert eines Attributs und sicherstellen, dass in Zukunft nicht übergeben wird, müssen Sie stattdessen eine benutzerdefinierte Regel erstellen.

Bei Fabrikam haben wir festgestellt, dass einige der Attribute, die wir in der Cloud synchronisieren nicht vorhanden ist. Wir möchten sicherstellen, dass diese Attribute von Azure AD entfernt werden.  
![Ungültige Erweiterungsattribute](./media/active-directory-aadconnectsync-change-the-configuration/badextensionattribute.png)

- Erstellen Sie eine neue Regel für eingehende Synchronisierung und füllen die Beschreibung ![Beschreibung](./media/active-directory-aadconnectsync-change-the-configuration/syncruledescription.png)
- Erstellen Sie Attribut **Ausdruck** und mit **AuthoritativeNull**. Das literal **AuthoritativeNull** gibt an, dass der Wert im Metaverse leer sein sollte, auch wenn eine niedrigere Rangfolge Synchronisierungsregel den Wert aufgefüllt.
![Transformation für Erweiterungsattribute](./media/active-directory-aadconnectsync-change-the-configuration/syncruletransformations.png)
- Speichern Sie synchronisieren. Starten Sie **Synchronisierungsdienst**, finden Sie den Connector **Ausführen**und **Vollständige Synchronisierung**auswählen. Dieser Schritt berechnet alle Attribut Abläufe.
- Überprüfen Sie die geplanten Änderungen Suche Connectorspace exportiert werden.
![Bereitgestellte löschen](./media/active-directory-aadconnectsync-change-the-configuration/deletetobeexported.png)

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen über Konfigurationsmodell [Verständnis deklarative](active-directory-aadconnectsync-understanding-declarative-provisioning.md)Bereitstellung.
- Weitere Informationen über Ausdruckssprache [Verständnis deklarative Bereitstellung Ausdrücke](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).

**Themen (Übersicht)**

- [Azure AD Verbindung synchronisieren: verstehen und Synchronisation anpassen](active-directory-aadconnectsync-whatis.md)
- [Integration von lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)
