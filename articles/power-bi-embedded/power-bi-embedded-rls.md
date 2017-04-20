<properties
   pageTitle="Zeilenbasierter Sicherheit mit Power BI Embedded"
   description="Details zum zeilenbasierter Sicherheit mit Power BI Embedded"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="row-level-security-with-power-bi-embedded"></a>Sicherheit auf Zeilenebene mit Power BI Embedded

Sicherheit auf Zeilenebene (RLS) kann verwendet werden, um Benutzer Zugriff auf bestimmte Daten in einem Bericht oder Dataset für mehrere unterschiedliche Benutzer den gleichen Bericht sehen alle anderen Daten verwenden. Power BI Embedded unterstützt jetzt Datasets mit RLS konfiguriert.

![](media\power-bi-embedded-rls\pbi-embedded-rls-flow-1.png)

Um RLS nutzen zu können, ist es wichtig, drei Hauptkonzepte verstehen. Benutzer, Rollen und Regeln. Nehmen Sie sich jeweils an:

**Benutzer** – die tatsächlichen Endbenutzer sind Berichte anzeigen. In Power BI eingebettet sind werden Benutzer durch die Username-Eigenschaft in einer Anwendung Token identifiziert.

**Rollen** – Rollen Benutzer gehören. Eine Rolle ist ein Container für Regeln und Namen etwas wie "Sales Manager" oder "Vertriebsmitarbeiter". In Power BI eingebettet sind werden Benutzer von Rollen-Eigenschaft in einer Anwendung Token identifiziert.

**Regeln** – Rollen haben Regeln und Vorschriften sind die eigentlichen Filter auf die Daten angewendet werden soll. Dies kann so einfach sein wie "Country = USA" oder etwas dynamischeren.

### <a name="example"></a>Beispiel

Für den Rest dieses Artikels bieten wir ein Beispiel RLS erstellen und verwenden, die in einer eingebetteten Anwendung. In unserem Beispiel verwendet die [Einzelhandel Analyse](http://go.microsoft.com/fwlink/?LinkID=780547) PBIX Datei.

![](media\power-bi-embedded-rls\pbi-embedded-rls-scenario-2.png)

Unsere Retail-Analyse gezeigt Verkäufe für alle Shops in einer bestimmten Einzelhandelskette. Ohne RLS, egal welche District Manager anmeldet und zeigt den Bericht, sehen sie die gleichen Daten. Geschäftsführung hat District Manager sollte nur angezeigt werden die Umsätze für die Geschäfte, die sie verwalten und hierzu können RLS bestimmt.

RLS in Power BI Desktop erstellt. Beim Öffnen des Datasets und der Bericht wechseln wir Diagrammansicht Schema finden Sie unter:

![](media\power-bi-embedded-rls\pbi-embedded-rls-diagram-view-3.png)

Hier sind ein paar Dinge mit diesem Schema zu beachten:

-   Alle Maßnahmen **Gesamtumsatz**werden in der **Sales** -Faktentabelle gespeichert.
-   Vier zusätzliche verbundene Dimensionstabellen sind: ** **Artikel**, **Speicher**, und **Bezirk****.
-   Die Pfeile auf die Beziehungslinien anzugeben, wie Filter aus einer Tabelle in das andere fließen können. Beispielsweise wenn Filter **[Datum]**befindet, würde im aktuellen Schema nur Werte in die Tabelle " **Sales** " filtern. Andere Tabellen sind von diesem Filter seit alle Pfeile auf der Beziehung der sales-Tabelle und nicht betroffen.
-   Der **Bezirk** Tabelle Wer ist der Manager für jede:

    ![](media\power-bi-embedded-rls\pbi-embedded-rls-district-table-4.png)

Basierend auf diesem Schema, wenn wir von **District Manager** -Spalte in der Tabelle Region filtern und das entspricht der Benutzer den Bericht filtern, werden auch filter Sie **Speicher** und die **Sales** -Tabelle nur Daten für diese bestimmten Bezirk Manager angezeigt.

Hier ist wie:

1.  Klicken Sie auf der Registerkarte Modellierung auf **Rollen verwalten**.  
![](media\power-bi-embedded-rls\pbi-embedded-rls-modeling-tab-5.png)

2.  Erstellen Sie eine neue Rolle **Manager**aufgerufen.  
![](media\power-bi-embedded-rls\pbi-embedded-rls-manager-role-6.png)

3.  Geben Sie in der Tabelle **Region** den DAX Ausdruck: **[District Manager] = USERNAME()**  
![](media\power-bi-embedded-rls\pbi-embedded-rls-manager-role-7.png)

4.  Um sicherzustellen, dass die Arbeiten der Regeln auf der Registerkarte **Modellieren** auf **Ansicht Rollen**und geben Sie dann Folgendes:  
![](media\power-bi-embedded-rls\pbi-embedded-rls-view-as-roles-8.png)

    Die Berichte zeigt nun Daten, sofern Sie als **Andrew**signiert wurden.

Anwenden des Filters, filtert wie wir hier Sie alle Datensätze in den Tabellen **Bezirk**, **Speicher**und **Verkauf** . Jedoch werden wegen Richtung Filter Beziehungen zwischen ** **Verkauf** und** **Vertrieb** und **Artikel** **Artikel** und **Zeit** Tabellen nicht unten gefiltert.

![](media\power-bi-embedded-rls\pbi-embedded-rls-diagram-view-9.png)

Vielleicht für diese Anforderung, jedoch möchten wir Manager Einträge sehen, für die sie Verkäufe haben, konnte bidirektionale Cross-Filterfunktionen aktivieren für die Beziehung und den Sicherheitsfilter in beide Richtungen fließen. Hierzu werden die Beziehung zwischen **Verkauf** und **Element**wie folgt bearbeiten:

![](media\power-bi-embedded-rls\pbi-embedded-rls-edit-relationship-10.png)

Jetzt können Filter auch aus der Sales-Tabelle zu der Tabelle **Artikel** übertragen werden:

![](media\power-bi-embedded-rls\pbi-embedded-rls-diagram-view-11.png)

**Hinweis** Wenn Sie DirectQuery-Modus für Ihre Daten verwenden, müssen Sie bidirektionale Zuordnung filtern diese zwei Optionen aktivieren:

1.  **Datei** -> **-Optionen** -> **Vorschaufunktionen** -> **Kreuz in beide Richtungen DirectQuery Filtern aktivieren**.
2.  **Datei** -> **-Optionen** -> **DirectQuery** -> **erlauben uneingeschränkten im DirectQuery-Modus**.


Erfahren Sie mehr über bidirektionale Cross-Filterung herunterladen Sie [bidirektionale Zuordnung Filtern in SQL Server Analysis Services 2016 und Power BI Desktop](http://download.microsoft.com/download/2/7/8/2782DF95-3E0D-40CD-BFC8-749A2882E109/Bidirectional cross-filtering in Analysis Services 2016 and Power BI.docx) whitepaper

Dies schließt alle arbeiten, die Power BI Desktop erfolgen, jedoch gibt es eine weitere Arbeit, die die RLS machen wir Arbeit macht BI eingebettete definiert Regeln. Benutzer authentifiziert und autorisiert die Anwendung und App Token zum Gewähren Sie diesem Benutzer den Zugriff auf einen bestimmten Power BI eingebetteten Bericht. Power BI eingebettete haben nicht bestimmten Informationen, wer der Benutzer ist. RLS arbeiten müssen Sie einige zusätzlichen Kontext als Teil Ihrer app Token übergeben:
-   **Benutzername** (optional) – mit RLS verwendet eine Zeichenfolge zur Identifizierung den Benutzer bei RLS-Regeln ist. Siehe Zeile Sicherheit mit Power BI eingebettet
-   **Rollen** – Rollen auswählen, wenn Zeile Sicherheitsstufe geltenden eine Zeichenfolge. Wenn mehr als eine Rolle, sollten sie als Zeichenfolgenarray übergeben werden.

Wenn die Username-Eigenschaft vorhanden ist, müssen Sie mindestens einen Rollen übergeben.

Das vollständige Anwendung Token wird wie folgt aussehen:

![](media\power-bi-embedded-rls\pbi-embedded-rls-app-token-string-12.png)

Jetzt mit allen zusammen werden jemand in unserer Anwendung zum Anzeigen dieses Berichts meldet sie nur die Daten sehen, die sie sehen, unsere Sicherheit auf Zeilenebene festgelegten.

![](media\power-bi-embedded-rls\pbi-embedded-rls-dashboard-13.png)

## <a name="see-also"></a>Siehe auch
[Zeilenbasierter Sicherheit (RLS) mit](https://powerbi.microsoft.com/en-us/documentation/powerbi-admin-rls/)
