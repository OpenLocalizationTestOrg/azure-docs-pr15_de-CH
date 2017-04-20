<properties
    pageTitle="Azure AD Verbindung synchronisieren: Filterung | Microsoft Azure"
    description="Erläutert die Filterung in Azure AD-Verbindung synchronisieren."
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
    ms.date="09/13/2016"
    ms.author="andkjell;markvi"/>


# <a name="azure-ad-connect-sync-configure-filtering"></a>Azure AD Verbindung synchronisieren: Filter konfigurieren
Mit Filtern können Sie steuern, welche Objekte aus dem lokalen Verzeichnis in Azure AD angezeigt werden soll. Die Konfiguration hat alle Objekte in allen Domänen in den Gesamtstrukturen konfigurierten. Im Allgemeinen ist dies die empfohlene Konfiguration. Mit Office 365-Workloads wie Exchange Online Skype für Unternehmen, die Anwender profitieren von einer vollständigen globalen Adressliste e-Mail und alle. In der Standardkonfiguration erhalten sie die gleiche Erfahrung, die sie mit einer lokalen Implementierung oder Lync.

In einigen Fällen ist es erforderlich, die Standardkonfiguration ändern. Es folgen einige Beispiele:

- [Topologie mit mehreren Azure AD-Verzeichnis](active-directory-aadconnect-topologies.md#each-object-only-once-in-an-azure-ad-directory)verwendet werden soll. Müssen Sie filtern steuern, welches Objekt in einer bestimmten synchronisiert werden sollen Azure AD-Verzeichnis.
- Ausführen eines Pilotprojekts für Azure oder Office 365 und Sie möchten nur einen Teil der Benutzer in Azure AD. In kleinen Pilotprojekt ist es nicht unbedingt eine vollständige globale Adressliste die Funktionen vorzuführen.
- Sie haben viele Dienstkonten und nicht persönliche Konten, die nicht in Azure AD soll.
- Aus Compliance-Gründen löschen Sie keine Benutzerkonten auf lokalen. Nur deaktiviert wieder. Aber in Azure AD Sie nur aktive Firmen vorhanden sein.

Dieser Artikel beschreibt die verschiedenen Methoden konfigurieren.

> [AZURE.IMPORTANT]Microsoft unterstützt nicht die Änderung oder Operation Azure AD Connect Synchronisierung außerhalb dieser Aktionen formal dokumentiert. Diese Aktionen möglicherweise in einem inkonsistenten oder nicht unterstützte Azure AD verbinden synchronisieren und daher Microsoft kann keine technische Unterstützung für diese Installationen.

## <a name="basics-and-important-notes"></a>Grundlagen und Hinweise
In Azure AD-Verbindung synchronisieren können Sie jederzeit filtern. Sie starten mit einer Standardkonfiguration Verzeichnissynchronisation und Filter konfigurieren, werden herausgefiltert werden Objekte nicht mehr Azure AD synchronisiert. Durch diese Änderung werden alle Objekte in Azure AD, die gefiltert wurden, wurden zuvor synchronisiert in Azure AD gelöscht.

Bevor Sie beginnen, ändert zu filtern, müssen Sie Sie [Deaktivieren des Tasks](#disable-scheduled-task) , damit Sie nicht versehentlich ändern exportieren möchten, die Sie nicht noch richtig geprüft haben.

Da Filter viele Objekte gleichzeitig entfernen kann, möchten Sie sicherstellen, dass die neuen Filter korrekt sind, Sie vor dem Exportieren von Änderungen in Azure AD. Nach Abschluss der Konfigurationsschritte empfohlen die [Schritte](#apply-and-verify-changes) ausführen, bevor exportieren und Azure AD zu ändern.

Schützen Sie versehentlich zu viele Objekte löschen, ist [keine versehentliche Löschung](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) Feature standardmäßig. Wenn viele Objekte durch Filtern (standardmäßig 500) löschen, müssen Sie die Schritte in diesem Artikel löscht Azure AD durchlaufen können.

Verwenden Sie einen Build vor November 2015 ([1.0.9125](active-directory-aadconnect-version-history.md#1091250)), ändern die Konfiguration und Verwendung Kennwortsynchronisation, müssen Sie eine vollständige Synchronisierung aller Kennwörter nach Abschluss die Konfiguration auslösen. Schritte zum Auslösen eines Kennworts vollständige Synchronisierung finden Sie unter [Trigger eine vollständige Synchronisierung aller Kennwörter](active-directory-aadconnectsync-implement-password-synchronization.md#trigger-a-full-sync-of-all-passwords). Wenn Sie 1.0.9125 oder höher sind, berechnet die regelmäßige **vollständige Synchronisierung** Aktion auch Kennwörter synchronisiert werden sollen und dieser zusätzliche Schritt ist nicht mehr erforderlich.

**Benutzerobjekte** versehentlich in Azure AD Filtern Fehler gelöscht wurden, können Benutzerobjekte in Azure AD durch Entfernen der Filterung Konfigurationen erstellen und synchronisieren Sie die Verzeichnisse neu. Dadurch wird der Benutzer in Azure AD aus dem Papierkorb wiederhergestellt. Andere Objekttypen können jedoch kann nicht wiederhergestellt werden. Beispielsweise können Sie löschen versehentlich eine Sicherheitsgruppe und ACL verwendete Ressource, die Gruppe und die Zugriffssteuerungslisten wiederhergestellt werden.

Azure AD Connect nur Objekte gelöscht, die als es einmal im Gültigkeitsbereich befinden. Wenn Objekte in Azure AD, die von einem anderen Synchronisierungsmodul erstellt und diese Objekte nicht im Gültigkeitsbereich, Hinzufügen von Filtern nicht entfernen sie. Beispielsweise wenn mit einem Dirsync-Server beginnen eine vollständige Kopie des gesamten Verzeichnisses in Azure AD erstellt und eine neue Azure AD Connect Synchronisierungsserver mit Filtern ab aktiviert installiert entfernt es zusätzlichen Objekte erstellt DirSync nicht.

Die Filterkonfiguration bleibt beim Installieren oder auf eine neuere Version von Azure AD verbinden aktualisieren. Es ist immer empfehlenswert, stellen Sie sicher, dass die Konfiguration nicht versehentlich geändert wurde nach einer Aktualisierung auf eine neuere Version vor dem Ausführen die erste Synchronisierung wechseln.

Haben Sie mehrere Gesamtstrukturen, müssen in jeder Gesamtstruktur (vorausgesetzt, die gleiche Konfiguration alle gewünschten) filtern Konfigurationen in diesem Thema beschriebenen angewendet.

### <a name="disable-scheduled-task"></a>Deaktivieren von geplanten Tasks
Deaktivieren der integrierte Taskplaner, der alle 30 Minuten eine Synchronisierungszyklus auslöst, gehen Sie folgendermaßen vor:

1. Wechseln Sie zu einem PowerShell prompt
2. Führen Sie `Set-ADSyncScheduler -SyncCycleEnabled $False` um den Planer zu deaktivieren.
3. Ändern Sie wie in diesem Thema beschrieben.
4. Ausführen `Set-ADSyncScheduler -SyncCycleEnabled $True` den Planer erneut aktivieren.

**Verwenden Sie einen Build Azure AD Connect vor 1.1.105.0**  
Deaktivieren des Tasks, der Synchronisierungszyklus alle 3 Stunden auslöst, gehen Sie folgendermaßen vor:

1. Starten des **Taskplaners** aus dem Startmenü.
2. Suchen Sie direkt unter **Aufgabenplanungsbibliothek**Aufgabe **Azure AD Sync-Planer**, mit der rechten Maustaste und wählen Sie **Deaktivieren**.  
![Taskplaner](./media/active-directory-aadconnectsync-configure-filtering/taskscheduler.png)  
3. Sie können jetzt Konfiguration ändern und das Synchronisierungsmodul **Synchronisierung ServiceManager** -Konsole manuell ausführen.

Nach Abschluss der Filterung ändert vergessen kommen zurück und die Aufgabe zu **Aktivieren** .

## <a name="filtering-options"></a>Filteroptionen
Verzeichnissynchronisation Tool können folgende Konfiguration Filtertypen zugewiesen:

- [**Gruppe**](active-directory-aadconnect-get-started-custom.md#sync-filtering-based-on-groups): Filtern nach einer Gruppe kann nur auf Erstinstallation mithilfe des Assistenten konfiguriert. Es wird in diesem Thema nicht weiter behandelt.

- [**Domänenbasierte**](#domain-based-filtering): Diese Option können Sie die Domänen auswählen, die in Azure AD synchronisieren. Sie können hinzufügen und Entfernen von Domänen aus der Sync-Modulkonfiguration Wenn Sie ändern Ihre lokalen Infrastruktur nach der Installation von Azure AD-Verbindung synchronisieren.

- [**Organisatorische Einheit basierende**](#organizational-unitbased-filtering): dieser Filteroption können Sie auswählen, welche Organisationseinheiten Azure AD synchronisieren. Diese Option ist für alle Objekttypen in ausgewählten Organisationseinheiten.

- [**Attribut-basierte**](#attribute-based-filtering): mit dieser Option können Sie Objekte Attributwerte für die Objekte filtern. Sie können auch verschiedene Filter für verschiedene Objekttypen.

Sie können gleichzeitig mehrere Filteroptionen. Beispielsweise können Sie OU filtern, nur Objekte in einer Organisationseinheit zu der gleichen Zeit Attribut Filtern die Objekte weiter filtern. Bei Verwendung mehrerer Filtermethoden verwenden Filter ein logisches und zwischen den Filtern.

## <a name="domain-based-filtering"></a>Domäne filtern
Dieser Abschnitt enthält den Domäne Filter konfigurieren. Wenn Sie hinzugefügt oder nach der Installation von Azure AD Connect Domänen in der Gesamtstruktur entfernt haben, müssen Sie die Filterkonfiguration aktualisieren.

Der Installations-Assistent und ändern [Domänen und Organisationseinheiten Filtern](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering)ist vorzugsweise Domäne Filtern ändern. Der Installations-Assistent wird in diesem Thema dokumentierten Aufgaben automatisieren.

Sie sollten nur diese Schritte Wenn Sie aus irgendeinem Grund nicht den Installationsassistenten auszuführen.

Domänenbasierte Filterkonfiguration besteht folgendermaßen:

- [Wählen Sie die Domänen](#select-domains-to-be-synchronized) , die in die Synchronisierung einbezogen werden sollen.
- Passen Sie für jede Domäne hinzugefügt und entfernt [Profile ausgeführt](#update-run-profiles).
- [Übernehmen und Änderungen überprüfen](#apply-and-verify-changes).

### <a name="select-domains-to-be-synchronized"></a>Wählen Sie Domänen synchronisiert werden
**Gehen Sie folgendermaßen vor, um die Domäne filtern:**

1. Melden Sie sich bei dem Server mit Azure AD verbinden synchronisieren mit einem Konto, das Mitglied der Sicherheitsgruppe " **ADSyncAdmins** ".
2. Starten Sie **Synchronisierungsdienst** im Startmenü.
3. Wählen Sie **Connectors** und in der Liste **Connectors** des Connectors mit dem **Active Directory-Domänendienste**. Wählen Sie **Aktionen** **Eigenschaften**.  
![Connectoreigenschaften](./media/active-directory-aadconnectsync-configure-filtering/connectorproperties.png)  
4. Klicken Sie auf **Configure Verzeichnispartitionen**.
5. Der **Verzeichnispartitionen** Liste wählen und die Domänen nach Bedarf deaktivieren. Überprüfen Sie, ob die ausgewählten Partitionen, die Sie synchronisieren möchten.  
![Partitionen](./media/active-directory-aadconnectsync-configure-filtering/connectorpartitions.png)  
Wenn die lokal geändert haben AD-Infrastruktur sowie hinzugefügten oder entfernten Domänen der Gesamtstruktur klicken Sie **Aktualisieren** eine aktualisierte Liste abrufen. Wenn Sie aktualisieren, müssen Sie Anmeldeinformationen. Lokale Active Directory gewähren Sie Anmeldeinformationen mit Lesezugriff. Es muss nicht der Benutzer sein, der im Dialogfeld voreingestellt ist.  
![Aktualisierung erforderlich](./media/active-directory-aadconnectsync-configure-filtering/refreshneeded.png)  
6. Wenn Sie fertig sind, schließen Sie das Dialogfeld **Eigenschaften** durch Klicken auf **OK**. Wenn Sie Domänen aus der Gesamtstruktur entfernt haben, werden eine Popupmeldung angezeigt, dass eine Domäne entfernt wurde und die Konfiguration bereinigt.
7. Weiterhin [Ausführen Profile](#update-run-profiles)anpassen.

### <a name="update-run-profiles"></a>Ausführungsprofile aktualisieren
Wenn Sie den Filter Domäne aktualisiert haben, müssen Sie die Ausführungsprofile aktualisieren.

1. Stellen Sie in der Liste **Connectors** sicher, dass die Connectors im vorherigen Schritt geändert. Wählen Sie **Aktionen** **Ausführen Profile konfigurieren**.  
![Connector ausführen Profile](./media/active-directory-aadconnectsync-configure-filtering/connectorrunprofiles1.png)  

Sie müssen die folgenden Profile anpassen:

- Vollständigen Import
- Vollständige Synchronisierung
- Deltaimport
- Delta-Synchronisierung
- Exportieren

Für jede der fünf Profile die folgenden Schritte für jede Domäne **hinzugefügt** :

1. Das Ausführungsprofil klicken Sie und **Schritt**.
2. Auf der Seite **Schritt konfigurieren Sie** in der Dropdownliste **Typ** wählen Sie den Schritt mit demselben Namen wie das Profil, das Sie konfigurieren. Klicken Sie auf **Weiter**.  
![Connector ausführen Profile](./media/active-directory-aadconnectsync-configure-filtering/runprofilesnewstep1.png)  
3. Auf der Seite **Konfiguration des Connectors** in der Dropdownliste **Partition** wählen Sie die Domäne die Domäne Filter hinzugefügte.  
![Connector ausführen Profile](./media/active-directory-aadconnectsync-configure-filtering/runprofilesnewstep2.png)  
4. **Ausführungsprofil konfigurieren** Dialogfeld schließen, klicken Sie auf **Fertig stellen**.

Für jede der fünf Profile die folgenden Schritte für jede Domäne **entfernt** :

1. Wählen Sie das Profil ausführen.
2. Ist der **Wert** des Attributs **Partition** eine GUID, wählen Sie den Schritt ausführen und auf **Schritt löschen**.  
![Connector ausführen Profile](./media/active-directory-aadconnectsync-configure-filtering/runprofilesdeletestep.png)  

Das Ergebnis sollte jede Domäne synchronisieren möchten als Schritt in jeder Ausführungsprofil aufgelistet werden sollte.

Das Dialogfeld **Ausführen Profile konfigurieren** zu schließen, klicken Sie auf **OK**.

- Um die Konfiguration abzuschließen [Übernehmen und Änderungen überprüfen](#apply-and-verify-changes).

## <a name="organizational-unitbased-filtering"></a>Organisatorische Einheit – filtern
Die bevorzugte Methode zum Ändern der Organisationseinheit Filtern wird der Installations-Assistent und ändern [Domänen und Organisationseinheiten Filtern](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering). Der Installations-Assistent wird in diesem Thema dokumentierten Aufgaben automatisieren.

Sie sollten nur diese Schritte Wenn Sie aus irgendeinem Grund nicht den Installationsassistenten auszuführen.

**Gehen Sie folgendermaßen vor, um organisatorische Einheit basierenden Filter konfigurieren:**

1. Melden Sie sich bei dem Server mit Azure AD verbinden synchronisieren mit einem Konto, das Mitglied der Sicherheitsgruppe " **ADSyncAdmins** ".
2. Starten Sie **Synchronisierungsdienst** im Startmenü.
3. Wählen Sie **Connectors** und in der Liste **Connectors** des Connectors mit dem **Active Directory-Domänendienste**. Wählen Sie **Aktionen** **Eigenschaften**.  
![Connectoreigenschaften](./media/active-directory-aadconnectsync-configure-filtering/connectorproperties.png)  
4. Auf **Verzeichnispartitionen konfigurieren**, wählen Sie die Domäne konfigurieren möchten und klicken Sie auf **Container**.
5. Wenn Sie aufgefordert werden, Anmeldeinformationen Sie alle mit Lesezugriff zu lokalen Active Directory. Es muss nicht der Benutzer sein, der im Dialogfeld voreingestellt ist.
6. Deaktivieren Sie im Dialogfeld **Wählen Sie Container** Organisationseinheiten, die **Sie mit dem cloudverzeichnis synchronisieren möchten**.  
![ORGANISATIONSEINHEIT](./media/active-directory-aadconnectsync-configure-filtering/ou.png)  
  - **Computercontainer** sollte für Windows 10 Computer erfolgreich Azure AD synchronisiert werden ausgewählt. Domäne verbundene Computer in anderen Organisationseinheiten befinden, stellen Sie sicher, dass diese ausgewählt.
  - **ForeignSecurityPrincipals** Container sollte ausgewählt werden, haben Sie mehrere Gesamtstrukturen Vertrauensstellungen. Dieser Container ermöglicht gesamtstrukturübergreifende Sicherheitsgruppenmitgliedschaft aufgelöst werden.
  - **RegisteredDevices** OU sollte ausgewählt werden, wenn Sie Rückschreiben Gerätefunktion aktiviert haben. Verwenden Sie einen anderen Rückschreibfeature wie gruppenrückschreiben sicherzustellen Sie, dass diese Standorte ausgewählt werden.
  - Wählen Sie andere Organisationseinheit Benutzer, InetOrgPerson, Gruppen, Kontakte und Computer befinden. In dem Bild befinden diese sich OU ManagedObjects.
7. Wenn Sie fertig sind, schließen Sie das Dialogfeld **Eigenschaften** durch Klicken auf **OK**.
8. Um die Konfiguration abzuschließen [Übernehmen und Änderungen überprüfen](#apply-and-verify-changes).

## <a name="attribute-based-filtering"></a>Attribut-basierte Filterung
Stellen Sie sicher, Sie November 2015 ([1.0.9125](active-directory-aadconnect-version-history.md#1091250)) oder höher für diese Schritte zu erstellen.

-Attribut Filtern ist die flexibelste Vorgehensweise Objekte filtern. Die Leistungsfähigkeit von [deklarativen Bereitstellung](active-directory-aadconnectsync-understanding-declarative-provisioning.md) können Sie fast jeden Aspekt steuern, wann ein Objekt in Azure AD synchronisiert werden sollen.

Filter kann angewendet werden [eingehende](#inbound-filtering) aus Active Directory in das Metaversum und [ausgehende](#outbound-filtering) aus dem Metaverse Azure AD. Es wird empfohlen, den Filtern auf eingehende, die am einfachsten zu verwalten ist. Ausgehende Filterung sollte nur verwendet werden, wenn sich Objekte aus mehreren Gesamtstrukturen vor die Auswertung erforderlich ist.

### <a name="inbound-filtering"></a>Filtern eingehender Nachrichten
Eingehende basierend Filtern verwendet die Standardkonfiguration, Objekte zu Azure AD der Metaverse-Attribut CloudFiltered nicht auf einen Wert festgelegt, synchronisiert werden müssen. Wenn dieses Attribut auf **True**festgelegt ist, wird das Objekt nicht synchronisiert. Es sollte nicht auf **False** standardmäßig festgelegt werden. Um sicherzustellen, dass andere Regeln einen Wert beitragen können, dieses Attribut soll nur die Werte **True** oder **NULL** (nicht vorhanden).

In Eingehende Filter verwenden Sie die Stromversorgung des **Bereichs** Objekte sollten oder nicht synchronisiert werden. Dies ist, wo Sie Ihre eigene Organisation Anforderungen anpassen. Bereich muss das Modul **Gruppe** und **-Klausel** zu bestimmen, ob eine Synchronisierungsregel im Gültigkeitsbereich befinden sollten. Eine **Gruppe** enthält einen oder mehrere **Klausel**. Es ist ein logisches und zwischen mehreren Klauseln und eine logische oder mehrere Gruppen.

Lassen Sie uns ein Beispiel:  
![Bereich](./media/active-directory-aadconnectsync-configure-filtering/scope.png) sollte dies als gelesen **(Abteilung = IT) oder (Department = Vertrieb und c = US)**.

Beispiele und Schritte das Benutzerobjekt als Beispiel verwenden, aber können Sie dies für alle Objekttypen.

In den folgenden Beispielen Priorität beginnen mit 500. Dieser Wert stellt sicher, dass diese Regeln nach Out-of-Box-Regeln (Vorrang, höheren Zahlenwert) ausgewertet werden.

#### <a name="negative-filtering-do-not-sync-these"></a>Negative filtern "keine diese Synchronisierung"
Im folgenden Beispiel filtern Sie (nicht synchronisiert) alle Benutzer, **extensionAttribute15** Wert **NoSync**.

1. Melden Sie sich bei dem Server mit Azure AD verbinden synchronisieren mit einem Konto, das Mitglied der Sicherheitsgruppe " **ADSyncAdmins** ".
2. Starten Sie **Synchronisierung Regel-Editor** aus dem Startmenü.
3. **Eingehend** ausgewählt ist, und klicken Sie auf **Neue Regel hinzufügen**.
4. Geben Sie der Regel einen Namen wie "*In aus AD-Benutzer DoNotSyncFilter*". Wählen Sie die richtige Gesamtstruktur **Benutzer** als das **CS-Objekt**und **Person** als das **MV-Objekt**. Als **Link**wählen Sie **Verknüpfung** aus Vorrang geben Sie einen Wert derzeit nicht von einer anderen Synchronisierungsregel (z. B. 500) verwendet, und klicken Sie auf **Weiter**.  
![Eingehende 1 Beschreibung](./media/active-directory-aadconnectsync-configure-filtering/inbound1.png)  
5. Wählen Sie im Attribut **ExtensionAttribute15**Sie **Scoping-Filter**klicken Sie auf **Gruppe hinzufügen**und auf **-Klausel hinzufügen**. Stellen Sie sicher, dass der Operator **gleich** ist, und geben Sie im Feld Wert den Wert **NoSync** . Klicken Sie auf **Weiter**.  
![Eingang 2 Bereich](./media/active-directory-aadconnectsync-configure-filtering/inbound2.png)  
6. **Join** -Regeln leer, und klicken Sie auf **Weiter**.
7. Klicken Sie auf **Transformation hinzufügen**, wählen Sie **Wechselkurs des Flusstyps** auf **Konstante**Zielattribut **CloudFiltered** und geben Sie im Feld Quelle **True**. Klicken Sie auf **Hinzufügen** , um die Regel zu speichern.  
![Eingehende 3 transformation](./media/active-directory-aadconnectsync-configure-filtering/inbound3.png)
8. Um die Konfiguration abzuschließen [Übernehmen und Änderungen überprüfen](#apply-and-verify-changes).

#### <a name="positive-filtering-only-sync-these"></a>Filtern von "nur-sync diese" positiv
Ausdruck positive Filterung kann schwieriger werden, auch Objekte, die nicht synchronisiert werden, wie z. B. Konferenzräumen offensichtlich sind.

Positive Filteroption erfordert zwei Synchronisierungsregeln. Eine (oder mehrere) mit der richtigen Objekte zu synchronisieren und eine zweite Regel Catchall-Sync, Filter, alle Objekte, die noch nicht als Objekt identifiziert haben, die synchronisiert werden soll.

Im folgenden Beispiel synchronisieren Sie nur Benutzerobjekte Attributs Abteilung den Wert **Verkauf**hat.

1. Melden Sie sich bei dem Server mit Azure AD verbinden synchronisieren mit einem Konto, das Mitglied der Sicherheitsgruppe " **ADSyncAdmins** ".
2. Starten Sie **Synchronisierung Regel-Editor** aus dem Startmenü.
3. **Eingehend** ausgewählt ist, und klicken Sie auf **Neue Regel hinzufügen**.
4. Geben Sie der Regel einen Namen wie "*In AD-User Sales synchronisieren*". Wählen Sie die richtige Gesamtstruktur **Benutzer** als das **CS-Objekt**und **Person** als das **MV-Objekt**. Als **Link**wählen Sie **Verknüpfung** aus Vorrang geben Sie einen Wert von einer anderen Synchronisierungsregel (z. B. 501) derzeit nicht verwendet und klicken Sie dann auf **Weiter**.  
![Eingehende 4 Beschreibung](./media/active-directory-aadconnectsync-configure-filtering/inbound4.png)  
5. **Scoping-Filter**klicken Sie auf **Gruppe hinzufügen**, auf **-Klausel hinzufügen**und wählen Sie im Attribut **Abteilung**. Stellen Sie sicher, dass der Operator **gleich** ist, und geben Sie im Feld Wert den Wert **Verkauf** . Klicken Sie auf **Weiter**.  
![Eingehende 5 Bereich](./media/active-directory-aadconnectsync-configure-filtering/inbound5.png)  
6. **Join** -Regeln leer, und klicken Sie auf **Weiter**.
7. Klicken Sie auf **Transformation hinzufügen**, wählen Sie **Wechselkurs des Flusstyps** auf **Konstante**Zielattribut **CloudFiltered** und geben Sie im Feld Quelle **False**. Klicken Sie auf **Hinzufügen** , um die Regel zu speichern.  
![Eingehende 6 transformation](./media/active-directory-aadconnectsync-configure-filtering/inbound6.png)  
Dies ist ein Sonderfall, in dem Sie CloudFiltered explizit auf False festgelegt.

    Wir haben jetzt Catchall-Synchronisierungsregel zu erstellen.

8. Geben Sie der Regel einen Namen wie "*In aus AD-Benutzer Catchall-Filter*". Wählen Sie die richtige Gesamtstruktur **Benutzer** als das **CS-Objekt**und **Person** als das **MV-Objekt**. Als **Link**wählen Sie **Verknüpfung** aus und in der Rangfolge einen Wert derzeit nicht von einer anderen Synchronisierungsregel (z. B. 600) verwendet. Sie haben Vorrang Wert (niedriger Vorrang) als die vorherige Synchronisierungsregel aktiviert aber auch etwas Platz wir weitere Synchronisierung Filterregeln später hinzufügen können, wenn Sie zusätzliche Kostenstellen Synchronisierung starten möchten. Klicken Sie auf **Weiter**.  
![Eingehende 7 Beschreibung](./media/active-directory-aadconnectsync-configure-filtering/inbound7.png)  
9. **Scoping-Filter** leer, und klicken Sie auf **Weiter**. Ein leerer Filter gibt an, dass die Regel auf alle Objekte angewendet werden soll.
10. **Join** -Regeln leer, und klicken Sie auf **Weiter**.
11. Klicken Sie auf **Transformation hinzufügen**, wählen Sie **Wechselkurs des Flusstyps** auf **Konstante**Zielattribut **CloudFiltered** und geben Sie im Feld Quelle **True**. Klicken Sie auf **Hinzufügen** , um die Regel zu speichern.  
![Eingehende 3 transformation](./media/active-directory-aadconnectsync-configure-filtering/inbound3.png)  
12. Um die Konfiguration abzuschließen [Übernehmen und Änderungen überprüfen](#apply-and-verify-changes).

Wenn Sie möchten, können Sie weitere Regeln des ersten Typs erstellen, weitere Objekte in unserem Synchronisation enthalten.

### <a name="outbound-filtering"></a>Ausgehende Filterung
In einigen Fällen muss das Filtern nur, nachdem die Objekte im Metaverse hinzugefügt haben. Es könnte beispielsweise erforderlich betrachten das Mail-Attribut in der Ressourcengesamtstruktur und das UserPrincipalName-Attribut aus der Gesamtstruktur zu ermitteln, ob ein Objekt synchronisiert werden sollen In diesen Fällen erstellen Sie die Filterung für ausgehende Regel.

In diesem Beispiel ändern Sie den Filter nur Benutzer e-Mail und UserPrincipalName enden mit @contoso.com werden synchronisiert:

1. Melden Sie sich bei dem Server mit Azure AD verbinden synchronisieren mit einem Konto, das Mitglied der Sicherheitsgruppe " **ADSyncAdmins** ".
2. Starten Sie **Synchronisierung Regel-Editor** aus dem Startmenü.
3. Klicken Sie unter **Regeln Typ** **ausgehend**.
4. Suchen Sie die Regel **zum AAD-Benutzer teilnehmen SOAInAD**. Klicken Sie auf **Bearbeiten**.
5. Das Popup Antwort **Ja** zum Erstellen einer Kopie der Regel.
6. Ändern Sie auf der Seite **Beschreibung** Vorrang in einen nicht verwendeten Wert beispielsweise 50.
7. Klicken Sie in der linken Navigationsleiste auf **Scoping-Filter** . Klicken Sie auf **Klausel hinzufügen**wählen Attribut **Mail**, Operator auswählen **ENDSWITH**und Werttyp **@contoso.com**. In ausgewählten Attribut **UserPrincipalName**, Operator auswählen **ENDSWITH**und Werttyp klicken **Klausel hinzufügen**, **@contoso.com**.
8. Klicken Sie auf **Speichern**.
9. Um die Konfiguration abzuschließen [Übernehmen und Änderungen überprüfen](#apply-and-verify-changes).

## <a name="apply-and-verify-changes"></a>Anwenden und ändern überprüfen
Nachdem Sie die Konfiguration geändert haben, müssen diese Objekte bereits im System angewendet werden. Es könnte auch sein, dass Objekte nicht in das Synchronisierungsmodul verarbeitet werden sollen und das Synchronisierungsmodul das Quellsystem, um zu überprüfen, den Inhalt zu lesen.

Geänderte Konfiguration **Domäne** oder **Organisationseinheit** Filtern müssen Sie **vollständigen Import** gefolgt von **Delta-Synchronisierung**.

Mit **Attribute** Filtern Konfiguration geändert wird, müssen Sie **vollständige**Synchronisierung.

Gehen Sie folgendermaßen vor:

1. Starten Sie **Synchronisierungsdienst** im Startmenü.
2. Wählen Sie **Connectors** und in der Liste **Connectors** Connector, vorgenommenen Konfiguration oben ändern. Wählen Sie **Aktionen** **Ausführen**.  
![Connector ausführen](./media/active-directory-aadconnectsync-configure-filtering/connectorrun.png)  
3. **Führen Sie Profile**wählen Sie die im vorherigen Abschnitt beschriebene Operation aus Benötigen Sie zwei Aktionen ausgeführt, die zweite nach Abschluss der ersten Ausführen (die Spalte **State** ist **im Leerlauf** für den ausgewählten Verbinder).

Nach der Synchronisierung werden alle bereitgestellt, damit es exportiert werden. Bevor Sie tatsächlich das in Azure AD ändern, möchten Sie überprüfen diese Änderungen.

1. Cmd Prompt starten und zur`%Program Files%\Microsoft Azure AD Sync\bin`
2. Ausführen:`csexport "Name of Connector" %temp%\export.xml /f:x`  
Der Name des Konnektors in Synchronisierungsdienst finden. Es hat einen Namen wie "contoso.com-AAD" Azure AD.
3. Ausführen:`CSExportAnalyzer %temp%\export.xml > %temp%\export.csv`
4. Sie haben nun eine Datei im %Temp% namens export.csv in Microsoft Excel untersucht werden kann. Diese Datei enthält alle Änderungen, die exportiert werden sollen.
5. Erforderlichen Daten oder Konfigurationsinformationen ändern und diese Schritte erneut (Import, synchronisieren und überprüfen) bis geändert, die exportiert werden sollen.

Wenn Sie zufrieden sind, exportieren Sie die Änderungen in Azure AD.

1. Wählen Sie **Connectors** und in der Liste **Connectors** Azure Active Directory Connector. Wählen Sie **Aktionen** **Ausführen**.
2. **Führen Sie Profile**wählen Sie **Exportieren**.
3. Geänderte Konfiguration vieler Objekte löschen, dann sehen Sie Fehler exportieren Wenn die Zahl mehr als einen konfigurierten Schwellenwert (standardmäßig 500). Wenn dieser Fehler angezeigt wird, müssen Sie [keine versehentliche Löschung](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)Feature vorübergehend zu deaktivieren.

Jetzt ist es Zeit, den Planer erneut aktivieren.

1. Starten des **Taskplaners** aus dem Startmenü.
2. Suchen Sie direkt unter **Aufgabenplanungsbibliothek**Aufgabe **Azure AD Sync-Planer**, mit der rechten Maustaste und wählen Sie **Aktivieren**.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über die Konfiguration [Azure AD-Verbindung synchronisieren](active-directory-aadconnectsync-whatis.md) .

Erfahren Sie mehr über [die lokalen Identitäten Azure Active Directory integrieren](active-directory-aadconnect.md).
