<properties
    pageTitle="Erste Schritte mit Data Catalog | Microsoft Azure"
    description="End-to-End-Lernprogramm stellt die Szenarien und Funktionen von Azure Data Catalog."
    documentationCenter=""
    services="data-catalog"
    authors="steelanddata"
    manager="jhubbard"
    editor=""
    tags=""/>
<tags
    ms.service="data-catalog"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-catalog"
    ms.date="09/20/2016"
    ms.author="spelluru"/>

# <a name="get-started-with-azure-data-catalog"></a>Erste Schritte mit Azure Data Catalog
Azure Data Catalog ist ein vollständig verwaltete Clouddienst, der als eine Registrierung und Entdeckung für Datenbestand Enterprise dient. Eine Übersicht finden Sie unter [Azure Data Catalog](data-catalog-what-is-data-catalog.md).

In diesem Lernprogramm soll Ihnen den Einstieg in Azure Data Catalog. Führen Sie die folgenden Schritte in diesem Lernprogramm:

| Verfahren | Beschreibung |
| :--- | :---------- |
| [Bereitstellung Datenkatalog](#provision-data-catalog) | In diesem Verfahren bereitstellen oder Azure Data Catalog einrichten. Sie führen diesen Schritt nur dann, wenn der Katalog nicht vor eingerichtet wurde. Sie können nur einen Datenkatalog pro Unternehmen (Microsoft Azure Active Directory-Domäne) gibt es mehrere Abonnements Azure-Konto zugeordnet. |
| [Erfassen von Daten](#register-data-assets) | In diesem Verfahren registrieren Sie Data Catalog Datenbestände aus der Datenbank AdventureWorks2014. Registrierung ist die strukturelle Metadaten wie Namen, Typen und Positionen aus der Datenquelle und Metadaten in den Katalog kopieren. Datenquelle und Daten bleiben, sind jedoch die Metadaten werden vom Katalog leicht erkennbar und verständlich verwendet. |
| [Suchen von Daten](#discover-data-assets) | In diesem Verfahren verwenden Sie Azure Data Catalog-Portal zu Daten, die im vorherigen Schritt registriert wurden. Nachdem eine Datenquelle bei Azure Data Catalog registriert wurde, wird seine Metadaten vom Dienst indiziert, so dass problemlos die Daten suchen können, die sie benötigen. |
| [Kommentieren Sie Daten](#annotate-data-assets) | In diesem Verfahren geben Sie Kommentare (Informationen wie Beschreibung, Tags, Dokumentation oder Experten) für die Datenbestände. Diese Informationen ergänzen die Metadaten extrahiert Daten aus der Datenquelle und die Datenquelle mehr Menschen verständlicher zu machen. |
| [Verbinden mit Daten](#connect-to-data-assets) | In diesem Verfahren öffnen Sie Datenbestände integrierte Client-Tools (wie Excel und SQL Server-Tools) und nicht integrierte Tool (SQL Server Management Studio). |
| [Daten verwalten](#manage-data-assets) | In diesem Verfahren richten Sie Sicherheit für Ihre Datenbestände. Data Catalog hat keine Benutzer auf die Daten Zugriff. Der Besitzer der Datenquelle steuert den Datenzugriff. <br/><br/> Mit Data Catalog entdecken Sie Datenquellen und Ansicht im Katalog registrierten Quellen **Metadaten** verknüpft. Möglicherweise gibt es Situationen, in denen Daten nur für bestimmte Benutzer oder Mitglieder bestimmter Gruppen sichtbar ist. Für diese Szenarios können Sie Data Catalog Besitz von Ressourcen innerhalb des Katalogs erfassten Daten und steuern die Sichtbarkeit der Ressourcen, die Sie besitzen. |
| [Entfernen von Daten](#remove-data-assets) | Im folgenden erfahren Sie, wie Daten aus dem Datenkatalog entfernt. |  

## <a name="tutorial-prerequisites"></a>Tutorial erforderliche Komponenten

### <a name="azure-subscription"></a>Azure-Abonnement
Um Azure Data Catalog einzurichten, müssen Sie der Besitzer oder an ein Azure-Abonnement sein.

Azure-Abonnements können Sie Zugriff auf Service Cloudressourcen wie Azure Data Catalog organisieren. Sie auch unterstützt Sie wie Ressourcenverbrauch gemeldet wird, berechnet und bezahlt. Jedes Abonnement haben eine andere Einstellung für Abrechnung und Bezahlung, so können Sie verschiedene Abonnements und verschiedene Pläne von Abteilung, Projekt, regionale Office. Jeder Cloud-Dienst gehört zu einem Abonnement und benötigen Sie ein Abonnement vor dem Einrichten von Azure Data Catalog. Finden Sie weitere [Konten verwalten, Abonnements und Verwaltungsfunktionen](../active-directory/active-directory-how-subscriptions-associated-directory.md).

Wenn Sie ein Abonnement haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Einzelheiten finden Sie [Kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/) .

### <a name="azure-active-directory"></a>Azure Active Directory
Um Azure Data Catalog einzurichten, müssen Sie mit einer Azure Active Directory (Azure AD) Benutzerkonto angemeldet. Sie müssen Besitzer oder an ein Azure-Abonnement sein.  

Azure AD bietet eine einfache Möglichkeit für Ihr Unternehmen Identitäts- und, in der Cloud und lokal verwalten. Eine Arbeit oder schulkonto können Sie jede Web-Anwendung Cloud oder lokal anmelden. Azure Data Catalog verwendet Azure AD-Anmeldung authentifizieren. Weitere Informationen finden Sie unter [Azure Active Directory](../active-directory/active-directory-whatis.md).

### <a name="azure-active-directory-policy-configuration"></a>Azure Active Directory Konfiguration

Eine Situation auftreten, Azure Data Catalog-Portal anmelden können, aber beim Anmelden des Quell-Registrierungstool, Sie eine Fehlermeldung, die Anmeldung verhindert. Dieser Fehler kann auftreten, wenn im Unternehmensnetzwerk sind oder wenn Sie sich von außerhalb des Unternehmensnetzwerks verbinden.

Das Registrierungstool verwendet *Formularauthentifizierung* , um Benutzer anmelden Azure Active Directory überprüfen. Erfolgreiche Anmeldung muss Azure Active Directory-Administrator in der *globalen Authentifizierungsrichtlinie*Formularauthentifizierung.

Mit der globalen Authentifizierungsrichtlinie können Sie Authentifizierung für Intranet- und extranet-Verbindungen getrennt, wie in der folgenden Abbildung dargestellt. Anmelden Fehler auftreten, wenn formularbasierte Authentifizierung nicht für das Netzwerk aktiviert ist, aus dem Sie eine Verbindung herstellen.

 ![Azure Active Directory globale Authentifizierungsrichtlinie](./media/data-catalog-prerequisites/global-auth-policy.png)

Weitere Informationen finden Sie unter [Konfigurieren von Authentifizierungsrichtlinien](https://technet.microsoft.com/library/dn486781.aspx).

## <a name="provision-data-catalog"></a>Bereitstellung Datenkatalog
Sie können nur einen Datenkatalog pro Unternehmen (Azure Active Directory-Domäne) bereitstellen. Daher, wenn der Besitzer oder Mitbesitzer des Azure-Abonnement dieser Azure Active Directory-Domäne gehörender bereits einen Katalog erstellt, Sie dürfen einen Katalog erneut erstellen, auch wenn Sie mehrere Azure-Abonnements. Testen, ob ein Datenkatalog von einem Benutzer in Azure Active Directory-Domäne erstellt wurde, der [Azure Data Catalog-Homepage](http://azuredatacatalog.com) und überprüfen Sie, ob der Katalog finden Sie unter. Wenn Sie bereits ein Katalog erstellt wurde, die folgende Schritte überspringen und zum nächsten Abschnitt wechseln.    

1. [Data Catalog Service-Seite](https://azure.microsoft.com/services/data-catalog) , und klicken Sie auf **Erste Schritte**.

    ![Azure Data Catalog - marketing-Startseite](media/data-catalog-get-started/data-catalog-marketing-landing-page.png)
2. Ein Benutzerkonto, den Besitzer oder an ein Azure-Abonnement anmelden. Die folgende Seite nach der Anmeldung angezeigt.

    ![Azure Data Catalog - Bereitstellung Datenkatalog](media/data-catalog-get-started/data-catalog-create-azure-data-catalog.png)
3. Geben Sie einen **Namen** für Data Catalog **Abonnement** zu verwenden und den **Speicherort** für den Katalog.
4. Erweitern Sie **Preise** und wählen Sie Azure Data Catalog **Edition** (frei oder Standard).
    ![Azure Data Catalog - Edition auswählen](media/data-catalog-get-started/data-catalog-create-catalog-select-edition.png)
5. Erweitern Sie **Katalogbenutzer** , und klicken Sie auf **Hinzufügen** , um Benutzer für den Datenkatalog hinzufügen. Sie werden dieser Gruppe automatisch hinzugefügt.
    ![Azure Data Catalog - Benutzer](media/data-catalog-get-started/data-catalog-add-catalog-user.png)
6. Erweitern Sie **Katalogadministratoren** , und klicken Sie auf **Hinzufügen** , um weitere Administratoren für den Datenkatalog hinzufügen. Sie werden dieser Gruppe automatisch hinzugefügt.
    ![Azure Data Catalog - Administratoren](media/data-catalog-get-started/data-catalog-add-catalog-admins.png)
7. Klicken Sie auf **Katalog erstellen** , um Datenkatalog für Ihre Organisation erstellen. Sie finden Sie auf der Daten-Katalog nach der Erstellung.
    ![Azure Data Catalog--erstellt](media/data-catalog-get-started/data-catalog-created.png)    

### <a name="find-a-data-catalog-in-the-azure-portal"></a>Suchen Sie einen Datenkatalog in Azure-portal
1. Auf einer separaten Registerkarte in einem Webbrowser oder in einem separaten Browserfenster zum [Azure-Portal](https://portal.azure.com) und das Konto, das zur Erstellung den Datenkatalog im vorherigen Schritt anmelden.
2. Wählen Sie **Durchsuchen** und dann auf **Data Catalog**.

    ![Azure Data Catalog - Azure Durchsuchen](media/data-catalog-get-started/data-catalog-browse-azure-portal.png) Siehe Datenkatalog erstellen.

    ![Azure Data Catalog - Katalog in Liste anzeigen](media/data-catalog-get-started/data-catalog-azure-portal-show-catalog.png)
4.  Klicken Sie auf den Katalog, den Sie erstellt haben. **Data Catalog** Blade im Portal angezeigt.

    ![Azure Data Catalog - Blade im portal ](media/data-catalog-get-started/data-catalog-blade-azure-portal.png)
5. Sie können Eigenschaften des Katalogs Daten anzeigen und aktualisieren. Beispielsweise auf **Preisstufe** , und ändern Sie die Ausgabe.

    ![Azure Data Catalog - Preisstufe](media/data-catalog-get-started/data-catalog-change-pricing-tier.png)

### <a name="adventure-works-sample-database"></a>Adventure Works-Beispieldatenbank
In diesem Lernprogramm Datenbestände (Tabellen) aus der AdventureWorks2014-Beispieldatenbank für SQL Server-Datenbankmodul registrieren, jedoch können Sie unterstützte Datenquelle möchten Sie mit Daten arbeiten, die bekannte und relevanter. Eine Liste unterstützter Datenquellen finden Sie unter [Datenquellen unterstützt](data-catalog-dsr.md).

### <a name="install-the-adventure-works-2014-oltp-database"></a>Adventure Works 2014 OLTP-Datenbank installieren
Adventure Works-Datenbank unterstützt standard Online-Transaktion Szenarien für ein fiktiver Fahrradhersteller (Adventure Works Cycles) umfasst Produkte, Verkauf und Einkauf. In diesem Lernprogramm können Sie Informationen in Azure Data Catalog erfassen.

Installieren Sie die AdventureWorks-Beispieldatenbank

1. [Adventure Works 2014 vollständige Datenbank Backup.zip](https://msftdbprodsamples.codeplex.com/downloads/get/880661) auf CodePlex herunterladen
2. Zum Wiederherstellen der Datenbank auf dem Computer gehen Sie in [eine Datenbank-Sicherung mit SQL Server Management Studio](http://msdn.microsoft.com/library/ms177429.aspx)oder folgendermaßen:
    1. Öffnen Sie SQL Server Management Studio und SQL Server-Datenbank-Engine an.
    2. **Datenbanken** und **Datenbank wiederherstellen**.
    3. Klicken Sie unter **Datenbank wiederherstellen**auf die Option **Gerät** für **Quelle** und klicken Sie auf **Durchsuchen**.
    4. **Wählen Sie Sicherungsmedien**klicken Sie auf **Hinzufügen**.
    5. Wechseln Sie zu dem Ordner, in dem die **AdventureWorks2014.bak** Datei, wählen Sie die Datei und klicken Sie auf **OK** , um das Dialogfeld **Sicherungsdatei suchen** zu schließen.
    6. Klicken Sie auf **OK** , um das Dialogfeld **Wählen Sie Sicherungsmedien** schließen.    
    7. Klicken Sie auf **OK** , um das Dialogfeld **Datenbank wiederherstellen** zu schließen.

Sie können jetzt Datenbestände aus der AdventureWorks-Beispieldatenbank mit Azure Data Catalog registrieren.

## <a name="register-data-assets"></a>Erfassen von Daten

In dieser Übung verwenden Sie das Registrierungstool Datenbestände aus der AdventureWorks-Datenbank mit dem Katalog registrieren. Registrierung ist die strukturelle Metadaten wie Namen, Typen und Positionen aus der Datenquelle und die darin enthaltenen Anlagen extrahiert und Metadaten in den Katalog kopieren. Datenquelle und Daten bleiben, sind jedoch die Metadaten werden vom Katalog leicht erkennbar und verständlich verwendet.

### <a name="register-a-data-source"></a>Registrieren einer Datenquelle

1.  [Azure Data Catalog-Homepage](https://azuredatacatalog.com) , und klicken Sie auf **Veröffentlichen**.

    ![Azure Data Catalog - Daten Schaltfläche](media/data-catalog-get-started/data-catalog-publish-data.png)

2.  Klicken Sie auf **Anwendung starten** zum Herunterladen, installieren und Ausführen der Registrierung auf Ihrem Computer.

    ![Azure Data Catalog - starten](media/data-catalog-get-started/data-catalog-launch-application.png)

3. Klicken Sie auf der Seite **Willkommen** auf **Anmelden** und geben Sie Ihre Anmeldeinformationen.    

    ![Azure Data Catalog - Homepage](media/data-catalog-get-started/data-catalog-welcome-dialog.png)

4. Klicken Sie auf der Seite **Microsoft Azure Data Catalog** auf **SQL Server** und **Weiter**.

    ![Azure Data Catalog - Datenquellen](media/data-catalog-get-started/data-catalog-data-sources.png)

5.  Geben Sie die SQL Server-Verbindungseigenschaften für **AdventureWorks2014** (siehe folgende Beispiel), und klicken Sie auf **Verbinden**.

    ![Azure Data Catalog - SQL Server-Verbindungszeichenfolge](media/data-catalog-get-started/data-catalog-sql-server-connection.png)

6.  Registrieren Sie die Metadaten der Datenressource. In diesem Beispiel registrieren Sie **Produktions-Product** -Objekte aus dem Namespace AdventureWorks Produktion:

    1. In der **Server** -Hierarchie erweitern Sie **AdventureWorks2014** , und klicken Sie auf **Produktion**.
    2. Wählen Sie **Product**, **ProductCategory** **ProductDescription**und **ProductPhoto** mit STRG + klicken.
    3. Klicken Sie auf **ausgewählte Pfeil verschieben** (**>**). Diese Aktion verschiebt alle ausgewählten Objekte in der Liste **Objekte registriert werden** .

        ![Azure Data Catalog-Lernprogramm – suchen und Auswählen von Objekten](media/data-catalog-get-started/data-catalog-server-hierarchy.png)
    4. **Einschließen einer Vorschau** Snapshot Vorschau der Daten auswählen Der Snapshot bietet bis zu 20 Datensätze aus jeder Tabelle und in den Katalog kopiert.
    5. Wählen Sie **Datenprofil enthalten** einen Snapshot der Statistiken Objekt Datenprofil enthalten (Beispiel: minimum, maximum und Durchschnitt Werte für eine Spalte Anzahl Zeilen).
    6. Geben Sie im Feld **Tags hinzufügen** **Zyklen Adventure funktioniert**. Dieser Vorgang fügt suchtags für diese Datenbestände. Tags sind eine hervorragende Möglichkeit, Benutzer registrierte Datenquelle finden.
    7. Geben Sie den Namen des ein- **Experten** auf diese Daten (optional).

        ![Azure Data Catalog Tutorial - Objekte registriert werden](media/data-catalog-get-started/data-catalog-objects-register.png)

    8. Klicken Sie auf **Registrieren**. Azure Data Catalog registriert die ausgewählten Objekte. In dieser Übung werden die ausgewählten Objekte von Adventure Works registriert. Das Registrierungstool Metadaten aus der Daten extrahiert und diese Daten in Azure Data Catalog Service kopiert. Die Daten bleiben, wo derzeit befindet und bleibt unter der Kontrolle der Administratoren und Richtlinien des aktuellen Systems.

        ![Azure Data Catalog - registrierten Objekte](media/data-catalog-get-started/data-catalog-registered-objects.png)

    9. Klicken Sie der registrierte Datenquellenobjekte **Portal anzeigen**. Bestätigen Sie im Portal Azure Data Catalog alle vier Tabellen und der Datenbank in der Rasteransicht angezeigt.

        ![Objekte in Azure Data Catalog-portal ](media/data-catalog-get-started/data-catalog-view-portal.png)


In dieser Übung wird Objekte aus der Beispieldatenbank AdventureWorks registriert, sodass sie problemlos von Benutzern in Ihrer Organisation entdeckt werden können. In der nächsten Übung erfahren Sie, wie registrierten Daten ermitteln.

## <a name="discover-data-assets"></a>Suchen von Daten
Erkennung in Azure Data Catalog verwendet zwei primäre: Suchen und filtern.

Suchen soll intuitives und leistungsfähiges Tool. Standardmäßig werden Eigenschaften in den Katalog vom Benutzer bereitgestellte Strukturänderungen einschließlich Suchbegriffe verglichen.

Filtern als Ergänzung suchen. Wählen Sie bestimmte Merkmale Experten Datenquellentyp, Objekttyp und Tags übereinstimmenden Datenbestände und kongruent Suchergebnisse einzuschränken.

Mithilfe einer Kombination aus suchen und Filtern können Sie schnell Datenquellen, die mit Azure Data Catalog der Datenbestände entdecken Sie registriert wurden.

In dieser Übung verwenden Sie Azure Data Catalog-Portal zu Daten, die in der vorherigen Übung registriert. Informationen zum Suchsyntax finden Sie in der [Suche im Katalog Syntax Verweis](https://msdn.microsoft.com/library/azure/mt267594.aspx) .

Folgende sind Beispiele für Datenbestände im Katalog ermitteln.  

### <a name="discover-data-assets-with-basic-search"></a>Entdecken Sie Datenbestände mit Suche
Einfache Suche können Sie einen Katalog mit einen oder mehrere Begriffe suchen. Die Ergebnisse sind Anlagen, die eine Eigenschaft mit einem oder mehreren angegebenen Begriffe entsprechen.

1. Klicken Sie auf **Startseite** in Azure Data Catalog-Portal. Wenn der Browser geschlossen haben, wechseln Sie zur [Homepage Azure Data Catalog](https://www.azuredatacatalog.com).
2. Geben Sie im Suchfeld `cycles` und drücken Sie die **EINGABETASTE**.

    ![Azure Data Catalog - einfache Textsuche](media/data-catalog-get-started/data-catalog-basic-text-search.png)
3. Bestätigen Sie, dass alle vier Tabellen und der Datenbank (AdventureWorks2014) in den Ergebnissen angezeigt. Zwischen **der Datenblattansicht** und **Listenansicht** wechseln über Schaltflächen in der Symbolleiste wie in der folgenden Abbildung dargestellt. Beachten Sie, dass der Suchbegriff in den Suchergebnissen hervorgehoben ist, da die Option **Markieren** **aktiviert**ist. Sie können auch die Anzahl der **Ergebnisse pro Seite** in den Suchergebnissen angezeigt.

    ![Azure Data Catalog - Basistext Suchergebnisse](media/data-catalog-get-started/data-catalog-basic-text-search-results.png)

    Bedienfeld " **Suchen** " wird auf der linken Seite und der **Eigenschaften** Bereich auf der rechten Seite. Im Feld **Suchen** können Sie Suchkriterien ändern und Filtern der Ergebnisse. **Im Eigenschaftenpanel** zeigt Eigenschaften des ausgewählten Objekts in der Liste oder Raster.

4. **Klicken Sie in den Suchergebnissen.** Klicken Sie auf die **Vorschau**, **Spalten** **Datenprofil**und **Dokumentation** Registerkarten oder klicken Sie auf den Pfeil, um den unteren Bereich zu erweitern.  

    ![Azure Data Catalog - unten](media/data-catalog-get-started/data-catalog-data-asset-preview.png)

    Auf der Registerkarte **Vorschau** eine Vorschau der Daten in **der Tabelle** angezeigt.  
5. Klicken Sie auf **Spalten** , um Informationen über Spalten (z. B. **Name** und **Datentyp**) der Daten-Ressourcen.
6. Klicken Sie auf die Registerkarte **Datenprofil** finden Sie unter Data profiling (z. B.: Anzahl der Zeilen, Größe oder kleinsten Wert in einer Spalte) in der Anlage Daten.
7. Filtern Sie die Ergebnisse mithilfe von **Filtern** auf der linken Seite. Klicken Sie z. B. auf **Tabelle** **für**, und Sie sehen nur die vier Tabellen nicht aus der Datenbank.

    ![Azure Data Catalog - Suchergebnisse filtern](media/data-catalog-get-started/data-catalog-filter-search-results.png)

### <a name="discover-data-assets-with-property-scoping"></a>Entdecken Sie Datenbestände mit scoping-Eigenschaft
Scoping-Eigenschaft können Sie Daten suchen, in denen der Suchbegriff mit der angegebenen Eigenschaft entspricht.

1. Deaktivieren Sie **Tabellenfilter unter **Objekttyp** **Filter**** .  
2. Geben Sie im Suchfeld `tags:cycles` und drücken Sie die **EINGABETASTE**. Alle Eigenschaften, die zum Suchen des Daten-Katalogs finden Sie unter [Daten Katalog Suchverweis Syntax](https://msdn.microsoft.com/library/azure/mt267594.aspx) .
3. Bestätigen Sie, dass alle vier Tabellen und der Datenbank (AdventureWorks2014) in den Ergebnissen angezeigt.  

    ![Data Catalog - Eigenschaft scoping Suchergebnisse](media/data-catalog-get-started/data-catalog-property-scoping-results.png)

### <a name="save-the-search"></a>Speichern Sie die Suche
1. Geben Sie im Bereich **Suchen** im Abschnitt **Aktuelle Suche** einen Namen für die Suche, und klicken Sie auf **Speichern**.

    ![Azure Data Catalog - Suche speichern](media/data-catalog-get-started/data-catalog-save-search.png)
2. Bestätigen Sie die gespeicherte Suche unter **Gespeicherte Suchen**angezeigt.

    ![Azure Data Catalog - gespeicherte Suchen](media/data-catalog-get-started/data-catalog-saved-search.png)
3. Wählen Sie eine der Aktionen können Sie auf die gespeicherte Suche (**Umbenennen**, **Löschen**, suchen **Als Standard speichern** ).

    ![Azure Data Catalog - Suchoptionen gespeichert](media/data-catalog-get-started/data-catalog-saved-search-options.png)

### <a name="boolean-operators"></a>Boolesche Operatoren
Sie erweitern oder Grenzen Sie Ihre Suche mit booleschen Operatoren.

1. Geben Sie im Suchfeld `tags:cycles AND objectType:table`, und drücken Sie die **EINGABETASTE**.
2. Bestätigen Sie, dass Sie nur Tabellen (nicht die Datenbank) in den Ergebnissen anzuzeigen.  

    ![Azure Data Catalog - booleschen Operator Suche](media/data-catalog-get-started/data-catalog-search-boolean-operator.png)

### <a name="grouping-with-parentheses"></a>Mit Klammern
Durch Gruppierung mit Klammern können Sie Teile der Abfrage zu logischen Isolierung besonders mit booleschen Operatoren gruppieren.

1. Geben Sie im Suchfeld `name:product AND (tags:cycles AND objectType:table)` und drücken Sie die **EINGABETASTE**.
2. Bestätigen Sie, dass nur die Tabelle **Product** in den Suchergebnissen angezeigt.

    ![Azure Data Catalog - Gruppierung suchen](media/data-catalog-get-started/data-catalog-grouping-search.png)   

### <a name="comparison-operators"></a>Vergleichsoperatoren
Mit Vergleichsoperatoren können Sie Vergleiche als Gleichheit für Eigenschaften, die numerischen und Datumsdatentypen.

1. Geben Sie im Suchfeld `lastRegisteredTime:>"06/09/2016"`.
2. Deaktivieren Sie **Tabellenfilter unter **Objekttyp**** .
3. Drücken Sie die **EINGABETASTE**.
4. Bestätigen Sie, dass die Tabellen **Product**, **ProductCategory** **ProductDescription**und **ProductPhoto** und AdventureWorks2014-Datenbank registriert in den Suchergebnissen angezeigt.

    ![Azure Data Catalog - Vergleich Suchergebnisse](media/data-catalog-get-started/data-catalog-comparison-operator-results.png)

Informationen Sie [zu Daten](data-catalog-how-to-discover.md) detaillierte Daten und [Daten Katalogsuche Syntax](https://msdn.microsoft.com/library/azure/mt267594.aspx) Suchsyntax entdecken.

## <a name="annotate-data-assets"></a>Kommentieren Sie Daten
In dieser Übung verwenden Sie Azure Data Catalog-Portal versehen (Hinzufügen Informationen wie Beschreibung, Tags oder Experten) Datenbestände, die Sie bereits im Katalog registriert haben. Die Strukturänderungen ergänzen und verbessern die strukturelle Metadaten aus der Datenquelle extrahiert werden, während der Registrierung und erleichtert die Datenbestände zu ermitteln.

In dieser Übung beschriften Sie einen einzelnen Vermögenswert (ProductPhoto). Angezeigter Name und Beschreibung hinzufügen ProductPhoto Daten Anlage.  

1.  [Azure Data Catalog-Homepage](https://www.azuredatacatalog.com) , und suchen Sie mit `tags:cycles` der Datenbestände finden Sie registriert haben.  
2. Klicken Sie in den Suchergebnissen auf **ProductPhoto** .  
3. **Bilder** für **Namen** und **Fotos von Produkten für marketing-Materialien** **Beschreibung**eingeben.

    ![Azure Data Catalog - ProductPhoto Beschreibung](media/data-catalog-get-started/data-catalog-productphoto-description.png)

    **Beschreibung** unterstützt erkennen und verstehen, warum und wie die Anlage ausgewählten Daten. Sie können auch mehrere Tags hinzufügen und Spalten. Jetzt können Sie versuchen suchen und Filtern von Daten mithilfe der beschreibenden Metadaten zum Katalog hinzugefügte entdecken.

Sie können auch auf dieser Seite folgendermaßen:

- Experten für die Anlage Daten hinzufügen. Klicken Sie auf **Hinzufügen** im Bereich **Experten** .
- Fügen Sie Tags auf Datensatzebene. Klicken Sie auf **Hinzufügen** im Bereich **Tags** . Ein Tag kann ein Benutzertag oder ein Glossar-Tag. Der Standard Edition von Data Catalog umfasst ein Business-Glossar, das Definieren einer Taxonomie zentralen Katalogadministratoren hilft. Katalogbenutzer können dann Datenbestände mit Glossar versehen. Weitere Informationen finden Sie unter [Business Glossar für geregelt einrichten](data-catalog-how-to-business-glossary.md)
- Fügen Sie Tags auf Spaltenebene. Klicken Sie auf **Hinzufügen** unter **Tags** für die Spalte, die Sie kommentieren möchten.
- Beschreibung auf Zeilenebene hinzufügen. Geben Sie die **Beschreibung** für eine Spalte. Sie können auch die Beschreibungsmetadaten extrahiert Daten aus der Datenquelle anzeigen.
- Fügen Sie **Zugriff beantragen** Informationen Benutzer Zugriff auf die Daten anfordern.

    ![Azure Data Catalog - Tags, Beschreibung hinzufügen](media/data-catalog-get-started/data-catalog-add-tags-experts-descriptions.png)

- Wählen Sie die Registerkarte **Dokumentation** und Dokumentation der Anlage Daten. Azure Data Catalog Dokumentation können Datenkatalog als Content Repository Sie eine vollständige Darstellung Ihrer Datenbestände erstellen.

    ![Azure Data Catalog - Registerkarte Dokumentation](media/data-catalog-get-started/data-catalog-documentation.png)


Sie können auch mehrere Datenbestände eine Anmerkung hinzufügen. Beispielsweise können Sie alle Datenbestände registrierten auswählen und Experte für sie angeben.

![Azure Data Catalog - Anmerkung mehrere Datenbestände](media/data-catalog-get-started/data-catalog-multi-select-annotate.png)

Azure Data Catalog unterstützt eine Crowdsourcing Strukturänderungen. Jeder Benutzer Datenkatalog kann Tags (Benutzer oder Glossar), eine Beschreibung und andere Metadaten hinzufügen, damit jeder Benutzer mit einer Perspektive eine Datenressource und die Verwendung dieser Perspektive erfasst und für andere Benutzer verfügbar haben.

Finden Sie unter [Daten versehen](data-catalog-how-to-annotate.md) detaillierte Daten hinzufügen.

## <a name="connect-to-data-assets"></a>Verbinden mit Daten
In dieser Übung öffnen Sie Datenbestände in eine integrierte Client-Tool (Excel) und nicht integrierte Tool (SQL Server Management Studio) mit Verbindungsinformationen.

> [AZURE.NOTE] Ist zu bedenken, dass Azure Data Catalog Zugriff auf die aktuelle Datenquelle gibt – es macht es leichter zu erkennen und zu verstehen. Beim Herstellen einer zu einer Datenquelle Verbindung gewählte Clientanwendung verwendet die Windows-Anmeldeinformationen oder Anmeldeinformationen aufgefordert, nach Bedarf. Wenn Sie nicht bereits Zugriff auf die Datenquelle verfügen, müssen Sie vor dem verbinden können.

### <a name="connect-to-a-data-asset-from-excel"></a>Verbinden Sie zu einem Daten aus Excel

1. **Produkt** aus den Suchergebnissen auswählen. Klicken Sie auf der Symbolleiste **Öffnen** , und klicken Sie auf **Excel**.

    ![Azure Data Catalog - Verbindung auf Daten](media/data-catalog-get-started/data-catalog-connect1.png)
2. Klicken Sie auf **Öffnen** im Popupfenster herunterladen. Dies kann je nach Browser variieren.

    ![Azure Data Catalog - Datei Excel-Verbindung](media/data-catalog-get-started/data-catalog-download-open.png)
3. Klicken Sie im Fenster **Sicherheitshinweis für Microsoft Excel** auf **Aktivieren**.

    ![Azure Data Catalog - Excel-Sicherheit-popup](media/data-catalog-get-started/data-catalog-excel-security-popup.png)
4. Übernehmen Sie die Standardeinstellungen im Dialogfeld **Daten importieren** , und klicken Sie auf **OK**.

    ![Azure Data Catalog - Daten importieren](media/data-catalog-get-started/data-catalog-excel-import-data.png)
5. Die Datenquelle in Excel anzeigen

    ![Azure Data Catalog - Product-Tabelle in Excel](media/data-catalog-get-started/data-catalog-connect2.png)

In dieser Übung werden mithilfe von Azure Data Catalog entdeckt Datenbestände verbunden. Azure Data Catalog-Portal können Sie direkt über die Clientanwendungen **im** Menü integriert. Sie können auch mit einer Anwendung mithilfe der Speicherort Verbindungsinformationen enthalten in den Metadaten Anlage auswählen. SQL Server Management Studio können Sie Verbinden mit der Datenbank AdventureWorks2014 Zugriff auf die Daten in diesem Lernprogramm registriert Datenbestände.

1. Öffnen Sie **SQL Server Management Studio**.
2. Geben Sie im Dialogfeld **Verbindung mit Server herstellen** den Servernamen im Fenster **Eigenschaften** die Azure Data Catalog-Portal.
3. Verwenden Sie entsprechende Authentifizierung und Anmeldeinformationen auf die Datenressource. Wenn Sie keinen Zugriff haben, verwenden Sie Informationen im Feld **Request Access** zu.

    ![Azure Data Catalog - Zugriff beantragen](media/data-catalog-get-started/data-catalog-request-access.png)

Klicken Sie auf **Ansicht Verbindungszeichenfolgen** anzeigen und ADF.NET, ODBC und OLE DB-Verbindungszeichenfolgen zur Verwendung in der Anwendung in die Zwischenablage kopieren.

## <a name="manage-data-assets"></a>Daten verwalten
In diesem Schritt erfahren Sie, wie Sicherheit für Ihre Daten Anlagen eingerichtet. Data Catalog hat keine Benutzer auf die Daten Zugriff. Der Besitzer der Datenquelle steuert den Datenzugriff.

Data Catalog Datenquellen zu ermitteln und die Metadaten im Katalog registrierten Quellen anzeigen können. Möglicherweise gibt es Situationen, in denen Datenquellen nur bestimmte Benutzer oder Mitglieder bestimmter Gruppen sichtbar ist. Für diese Szenarios können Sie Data Catalog Anlagen innerhalb des Katalogs erfassten Daten übernehmen und dann steuern die Sichtbarkeit der Ressourcen Sie besitzen.

> [AZURE.NOTE] In dieser Übung beschriebenen Managementfunktionen stehen nur in der Standard Edition von Azure Data Catalog, nicht in die kostenlose Version.
In Azure Data Catalog können Sie Daten übernehmen, Daten Mitbesitzer hinzu und legen die Sichtbarkeit der Daten.

### <a name="take-ownership-of-data-assets-and-restrict-visibility"></a>Übernehmen Sie des Besitzes von Daten und beschränken Sichtbarkeit

1. Zur [Homepage von Azure Data Catalog](https://www.azuredatacatalog.com). Geben Sie im Feld **Suchen** `tags:cycles` und drücken Sie die **EINGABETASTE**.
2. Klicken Sie auf ein Element in der Liste, und klicken Sie auf der Symbolleiste auf **Übernehmen** .
3. Klicken Sie im Abschnitt **Verwaltung** des **Eigenschaftenpanels** auf **Übernehmen**.

    ![Azure Data Catalog - Besitz](media/data-catalog-get-started/data-catalog-take-ownership.png)
4. Um Sichtbarkeit zu beschränken, wählen Sie **Eigentümer und diese Benutzer** in die **Sichtbarkeit** und klicken Sie auf **Hinzufügen**. Benutzer e-Mail-Adressen in das Textfeld eingeben und **ENTER**betätigen.

    ![Azure Data Catalog - Zugriff](media/data-catalog-get-started/data-catalog-ownership.png)

## <a name="remove-data-assets"></a>Entfernen von Daten

In dieser Übung verwenden Sie Azure Data Catalog-Portal Datenvorschau aus registrierten Daten und Daten aus dem Katalog löschen.

In Azure Data Catalog können Sie löschen ein einzelnes Element oder mehrere Elemente.

1. Zur [Homepage von Azure Data Catalog](https://www.azuredatacatalog.com).
2. Geben Sie im Feld **Suchen** `tags:cycles` und drücken Sie die **EINGABETASTE**.
3. Wählen Sie ein Element in der Liste und klicken auf **Löschen der Symbolleiste wie in der folgenden Abbildung dargestellt:**

    ![Azure Data Catalog - Grid-Element löschen](media/data-catalog-get-started/data-catalog-delete-grid-item.png)

    Bei Verwendung die Listenansicht ist das Kontrollkästchen links vom Element wie in der folgenden Abbildung dargestellt:

    ![Azure Data Catalog - Element löschen](media/data-catalog-get-started/data-catalog-delete-list-item.png)

    Sie können auch mehrere Datenbestände auswählen und löschen, wie in der folgenden Abbildung dargestellt:

    ![Azure Data Catalog - mehrere Daten löschen](media/data-catalog-get-started/data-catalog-delete-assets.png)


> [AZURE.NOTE] Das Standardverhalten des Katalogs ist Benutzer registrieren alle Datenquellen und Benutzer Vermögensgegenständen Daten löschen, die registriert wurden. Die Verwaltungsfunktionen in der Standard Edition von Azure Data Catalog werden zusätzliche Optionen an Ressourcen einschränken, die Ressourcen finden und einschränken, die Elemente löschen können.


## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm wurden Sie wichtige Funktionen von Azure Data Catalog, einschließlich der Registrierung, anmerken, erkennen und Verwalten von Enterprise-Datenbestände. Da das Lernprogramm abgeschlossen haben, ist es Zeit für den Einstieg. Sie können heute beginnen, setzen Sie und Ihr Team auf Datenquellen registrieren und ansprechende Kollegen Katalogs.

## <a name="references"></a>Referenzen

- [Zum Registrieren von Daten](data-catalog-how-to-register.md)
- [Ermitteln von Daten](data-catalog-how-to-discover.md)
- [Wie Daten versehen](data-catalog-how-to-annotate.md)
- [Daten dokumentieren](data-catalog-how-to-documentation.md)
- [Herstellen einer Verbindung zu Daten](data-catalog-how-to-connect.md)
- [Verwalten von Daten](data-catalog-how-to-manage.md)
