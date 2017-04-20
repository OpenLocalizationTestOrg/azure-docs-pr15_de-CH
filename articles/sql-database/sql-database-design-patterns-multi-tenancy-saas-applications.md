<properties
   pageTitle="Entwerfen von Mustern mandantenfähigen SaaS-Applikationen und Azure SQL-Datenbank | Microsoft Azure"
   description="Dieser Artikel beschreibt die Vorschriften und allgemeinen Architektur Datenmuster mandantenfähigen Datenbank benötigten Programme in einer Cloud-Umgebung zu berücksichtigen und verschiedene Nachteile dieser Muster zugeordnet. Darüber hinaus wie Azure SQL-Datenbank mit elastischen Pools elastische Tools helfen diese Anforderungen Weise ohne Kompromisse."
   keywords=""
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-design"
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# <a name="design-patterns-for-multitenant-saas-applications-and-azure-sql-database"></a>Entwerfen von Mustern mandantenfähigen SaaS-Applikationen und Azure SQL-Datenbank

In diesem Artikel erfahren Sie über den Vorschriften und allgemeinen Daten Architekturmuster mandantenfähigen Software als ein Service (SaaS) ergeben, die in einer Cloud-Umgebung ausgeführt. Darüber hinaus die Faktoren, die berücksichtigt werden müssen und die Nachteile bei anderen Entwurfsmuster. Elastische Pools und flexible Tools in Azure SQL-Datenbank helfen Ihnen Ihre spezifischen Bedürfnisse ohne andere Ziele.

Entwickler entscheiden manchmal, die ihre langfristigen Interessen verwenden sie Anwesens Modelle für die Datenebenen mandantenfähigen Anwendung entwerfen. Anfangs möglicherweise, Entwickler einfache Entwicklung und Cloud Service Provider senken wichtiger als Pächter Isolation oder die Skalierung einer Anwendung verstehen. Dadurch kann später Zufriedenheit Kundenprobleme zu teuer Kurs-Korrektur führen.

Eine mehrinstanzenfähige Anwendung ist eine Anwendung, die in einer Cloud-Umgebung gehostet und dieselben Dienste Hunderte oder Tausende von Mieter nicht freigeben oder die Daten bereitstellt. Ein Beispiel ist eine SaaS-Anwendung, die Mandanten in einer Cloud-gehosteten Umgebung bietet.

## <a name="multitenant-applications"></a>Mandantenfähigen Applikationen

Mandantenfähigen Applikationen können Daten und Arbeitslast problemlos partitioniert werden. Sie können Daten und Arbeitslast, z. B. an Mieter Grenzen partitionieren, da die meisten Anfragen innerhalb der Mieter auftreten. Diese Eigenschaft ist die Daten und die Arbeitslast und fördert die Anwendungsmuster in diesem Artikel beschriebenen.

Entwickler verwenden diese Anwendung in der gesamten Cloud-basierte Anwendung, einschließlich:

- Partner ergeben, die als SaaS-Applikationen in die Cloud gewechselt wird
- SaaS-Applikationen für die Cloud von Grund auf neu aufgebaut
- Direkte, kundenorientierte Applikationen
- Mitarbeiter mit Anwendung

SaaS-Anwendung, die für die Cloud und Wurzeln entwickelt sind als Datenbanken normalerweise partner mandantenfähigen. Diesen SaaS anzubieten Mieter eine spezialisierte Software-Anwendung als Dienst. Mieter können den Anwendungsdienst zuzugreifen und Verantwortung der zugeordneten Daten als Teil der Anwendung. Aber zur Nutzung der Vorteile von SaaS Mieter kontrollieren ihre eigenen Daten ergeben müssen. Sie vertrauen den SaaS-Dienstanbieter halten ihre Daten aus anderen Mandanten Daten sicher und isoliert. Beispiele solcher mandantenfähigen SaaS-Anwendung sind MYOB, SnelStart und Salesforce.com. Jede kann partitioniert Mieter Grenzen und Unterstützung der Anwendungsentwurf Muster in diesem Artikel beschrieben.

Programme, die direkt für Kunden oder Mitarbeiter in einer Organisation (oft als Benutzer Mieter bezeichnet) eine andere Kategorie mandantenfähigen Anwendungsspektrum. Kunden abonnieren und Dienstanbieter sammelt und speichert Daten nicht besitzen. Dienstanbieter müssen weniger strenge Vorschriften Daten ihrer Kunden über behördlicher Richtlinien voneinander isoliert. Dieser Anwendungstyp kundenorientierte mandantenfähigen sind Media Content-Anbieter wie Netflix, Spotify und Xbox LIVE. Andere Anwendungsbeispiele leicht partitionierbare sind Kunden, Internetebene Applikationen oder Internet der Dinge (IoT) Applikationen in jeder Kunde oder Gerät kann als eine Partition. Partitionsgrenzen können Benutzer und Geräte trennen.

Nicht alle Programme Partition problemlos auf eine einzelne Eigenschaft Mieter, Kunden, Benutzer oder Gerät. Komplexe-Exports (ERP) Anwendung hat z. B. Produkte, Aufträge und Kunden. Es hat ein komplexes Schema mit Tausenden von hochgradig vernetzten Tabellen.

Keine einzelne Partition Strategie kann gelten für alle Tabellen und in einer Anwendung. Dieser Artikel befasst sich mandantenfähigen Applikationen problemlos partitionierbare Daten und Arbeitslasten.

## <a name="multitenant-application-design-trade-offs"></a>Mandantenfähigen Anwendung Design Kompromisse

Das Entwurfsmuster mandantenfähigen Anwendungsentwickler normalerweise wählt basiert unter Berücksichtigung der folgenden Faktoren:

-   **Mieter Isolation**. Der Entwickler muss hat keine Mieter unerwünschten Zugriff auf andere Mandanten Daten. Die Isolation Anforderung erweitert andere zum Schutz vor lauten Nachbarn und können Daten des Mieters mandantenspezifische Anpassungen implementieren.
-   **Cloud-Ressourcenkosten**. Eine SaaS-Anwendung muss wettbewerbsfähig sein. Mandantenfähigen Anwendungsentwickler können für geringere Kosten bei der Verwendung von Cloudressourcen wie Speicher optimieren und Kosten zu berechnen.
-   **Einfache DevOps**. Mandantenfähigen Anwendungsentwickler muss Isolation Schutz integrieren, verwalten, und Überwachung des Status ihrer Anwendung und Datenbankschema und Mieter Problembehandlung. Komplexität bei der Entwicklung und Betrieb übersetzt direkt zu erhöhten Kosten und Mieter zufrieden stellt.
-   **Skalierbar**. Inkrementell hinzufügen Weitere Mieter und Kapazität für Mandanten, die sie benötigen unbedingt eine SaaS erfolgreich.

Alle diese Faktoren hat vor-und Nachteile im Vergleich zu anderen. Kostengünstigste Cloud bietet nicht die bequemste Erfahrung anbieten kann. Es ist wichtig für Entwickler zu diesen Optionen und ihre vor-und Nachteile während des Entwurfsprozesses Anwendung entscheiden.

Eine beliebte Entwicklungsmuster ist mit mehreren Mandanten eine oder mehrere Datenbanken. Die Vorteile dieses Ansatzes sind eine kostengünstige bezahlen einige Datenbanken und der relativ einfachen Handhabung mit einer begrenzten Anzahl von Datenbanken. Jedoch im Laufe der Zeit eine SaaS mandantenfähigen Anwendungsentwickler wird dadurch erhebliche Nachteile Mieter Isolation und skalierbar ist. Wenn Mieter isoliert wird, ist zusätzliche Aufwand Mieter Daten im freigegebenen Speicher vor unbefugtem Zugriff oder laut Nachbarn zu schützen. Dieser Aufwand kann Entwicklung und Isolation Wartungskosten erheblich steigern. Auch wenn hinzufügen Mieter erforderlich ist, erfordert dieses normalerweise Fachwissen Mieter Daten in Datenbanken auf die Datenebene einer Anwendung Skalierung neu verteilt.  

Häufig Mieter Isolierung ist eine Grundvoraussetzung im mandantenfähigen SaaS-Anwendung, die Unternehmen und Organisationen bieten. Ein Entwickler könnte durch wahrgenommenen gegenüber Einfachheit und Mieter Isolierung und Skalierung versucht. Dieser Kompromiss nachweist komplex und kostspielig wie der Dienst wächst und Mieter isoliert werden wichtige und verwalteten auf Anwendungsebene. In mandantenfähigen Clientanwendungen, die einen direkten, kundenorientierter Service Kunden werden Mieter Isolation jedoch eine niedrigere Priorität als die Optimierung für die Cloud Ressourcenkosten.

## <a name="multitenant-data-models"></a>Mandantenfähigen Datenmodelle

Common Design für Mieter Daten Vorgehensweisen drei unterschiedliche Modelle in Abbildung 1 dargestellt.


  ![Datenmodelle mandantenfähigen Anwendung](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-multi-tenant-data-models.png)
 Abbildung 1: Allgemeine Entwurfstechniken mandantenfähigen Datenmodelle

-   **Datenbank pro Mieter**. Jeder verfügt über eine eigene Datenbank. Alle Mandanten Daten auf den Mieter Datenbank beschränkt und isoliert von anderen Mandanten und ihre Daten.
-   **Freigegebene Datenbank Sharding**. Mehrere Mandanten nutzt mehrere Datenbanken. Wertegruppe Mieter wird jede Datenbank mit Partitionierungsstrategie Hash, Bereich oder Partitionierung Liste zugewiesen. Diese Daten Distributionsstrategie wird häufig als Sharding bezeichnet.
-   **Freigegebene Datenbank Datensatz**. Manchmal große Datenbank enthält Daten für alle Mandanten, die in einer Spalte Mieter auseinander gehalten werden.

> [AZURE.NOTE] Anwendungsentwickler können andere Mandanten in verschiedenen Datenbankschemas und den Schemanamen Sie eindeutig von anderen Mandanten verwenden. Dieser Ansatz wird nicht empfohlen, da in der Regel erfordert die Verwendung von dynamic SQL nicht effektiv Zwischenspeichern von Abfrageplänen. Im restlichen Artikel konzentrieren wir uns auf die gemeinsamen Ansatz für diese Kategorie mandantenfähigen Anwendung.

## <a name="popular-multitenant-data-models"></a>Beliebte mandantenfähigen Datenmodelle

Es ist zu verschiedener mandantenfähigen Datenmodelle in Anwendung Design Kompromisse, die wir bereits identifiziert haben. Diese Faktoren helfen kennzeichnen die drei häufigsten mandantenfähigen Datenmodelle beschriebenen und ihre Datenbankverwendung, wie in Abbildung 2 dargestellt.

-   **Isolierung**. Ein Maß wie viel Mieter Isolation kann ein Datenmodell erreicht das Maß an Isolierung zwischen.
-   **Cloud-Ressourcenkosten**. Der Betrag der Ressourcenfreigabe zwischen optimieren Cloud Ressourcenkosten. Eine Ressource kann als Computing- und Kosten definiert.
-   **DevOps Kosten**. Einfache Anwendungsentwicklung, Bereitstellung und Verwaltung reduziert Gesamtkosten SaaS-Vorgang.  

Abbildung 2 zeigt die y-Achse der Mieter Isolation. Die x-Achse zeigt die gemeinsame Nutzung von Ressourcen. Die grauen Diagonaler Pfeil in der Mitte gibt die Richtung der DevOps Kosten zu erhöhen oder zu verringern.

![Beliebte mandantenfähigen Anwendung Entwurfsmuster](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-popular-application-patterns.png) Abbildung 2: beliebte mandantenfähigen Datenmodelle

Quadrant unten rechts in Abbildung 2 zeigt eine Anwendung, die eine große verwendet freigegebenen einzelne Datenbank und der gemeinsam genutzten Tabelle (oder separates Schema) Ansatz. Es ist gut für Ressourcen freigeben, da alle die gleichen Ressourcen (CPU, Arbeitsspeicher, e/a) in einer einzigen Datenbank verwendet. Mieter Isolation ist jedoch beschränkt. Sie müssen zusätzliche Schritte Mieter auf Anwendungsebene voreinander. Diese zusätzlichen Schritte können die Kosten DevOps entwickeln und Verwalten der Anwendung erheblich. Skalierbarkeit wird durch das Skalieren der Hardware beschränkt, der die Datenbank hostet.

Der unteren linken Quadranten in Abbildung 2 veranschaulicht mehrere Mandanten Sharding in mehreren Datenbanken (normalerweise andere Hardware skaliert Einheiten). Jede Datenbank enthält eine Teilmenge der Mandanten, die welche Skalierbarkeit Anliegen der anderen Mustern Adressen. Wenn mehr Kapazität für mehr Mieter erforderlich ist, setzen Sie einfach Mieter auf neue Datenbanken neue Hardware Maßeinheiten zugeordnet. Allerdings wird die Ressourcenfreigabe reduziert. Nur Mieter platziert gemeinsam genutzter Ressourcen auf die gleichen Maßeinheiten. Dieser Ansatz bietet kleine Verbesserung um Isolation Mieter, da viele Mandanten noch zusammengestellt werden, ohne die Aktionen automatisch geschützt. Anwendungskomplexität hoch.

Der oberen linken Quadranten in Abbildung 2 ist das dritte Verfahren. Jeder Mandant Daten wird in einer eigenen Datenbank platziert. Dieser Ansatz hat gute Tenant-Isolation Eigenschaften aber Ressourcen gemeinsam nutzen, wenn jede Datenbank verfügt über eine eigene dedizierten Ressourcen beschränkt. Dieser Ansatz ist gut, wenn alle vorhersehbare Arbeitslasten. Wenn Mieter Arbeitslasten vorhersehbar sind, kann nicht der Anbieter Ressourcenfreigabe optimieren. Unvorhersehbarkeit ist für SaaS-Applikationen. Der Anbieter muss über Bereitstellung Anforderungen oder niedriger Ressourcen. Eine Aktion führt höhere Kosten oder niedrigeren mieterzufriedenheit. Höhere Ressourcenfreigabe über Mieter wird die Lösung kostengünstiger wünschenswert. Die Anzahl der Datenbanken erhöht auch DevOps Kosten für Bereitstellung und Verwaltung der Anwendungdes. Trotz dieser Bedenken stellt diese Methode die einfachste Isolation für Mandanten.

Diese Faktoren beeinflusst auch das Entwurfsmuster, das ein Kunde möchte:

-   **An Mieter Daten**. Eine Anwendung in der Mieter eigene Daten behalten bevorzugt das Muster einer einzelnen Datenbank pro Mandant.
-   **Skalieren**. Eine Anwendung für Hunderttausende oder Millionen von Mieter bevorzugt Datenbank Ansätze Sharding freigeben. Isolation Vorschriften bergen immer noch Probleme.
-   **Wert und**. Wenn einer Anwendung pro Mandanten Umsatz Wenn kleine (weniger als DM), Isolierung Vorschriften weniger kritisch und einer freigegebenen Datenbank sinnvoll. Umsatz pro Mieter einige Dollar oder mehr ist, eine Datenbank pro Mieter Modell mehr möglich. Sie können Kosten verringern.

Da Design Kompromisse in Abbildung 2 dargestellt, muss mandantenfähigen ideales Modell guten Mieter Isolation Eigenschaften mit optimalen Ressourcenfreigabe zwischen Mieter einbeziehen. Dieses Modell passt in die Kategorie im oberen rechten Quadranten Abbildung 2 beschrieben.

## <a name="multitenancy-support-in-azure-sql-database"></a>Mandatenfähiges-Unterstützung in Azure SQL-Datenbank

Azure SQL-Datenbank unterstützt alle mandantenfähigen Anwendungsmuster in Abbildung 2 beschrieben. Elastische Pools unterstützt auch eine Anwendungsmuster, die gute Ressourcenfreigabe kombiniert mit Isolation Vorzüge der Datenbank pro Mieter Ansatz (Siehe Quadrant oben rechts in Abbildung 3). Elastische Datenbanktools und Funktionen von SQL-Datenbank verringern Kosten zum Entwickeln und betreiben eine Anwendung mit vielen Datenbanken (Siehe den schattierten Bereich in Abbildung 3). Diese Tools können Sie erstellen und verwalten, die einem der Muster mit mehreren Datenbanken verwenden.

![Muster in der Azure SQL-Datenbank](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-patterns-sqldb.png) Abbildung 3: Anwendungsmuster mandantenfähigen in Azure SQL-Datenbank

## <a name="database-per-tenant-model-with-elastic-pools-and-tools"></a>Datenbank pro Mieter Modell elastische Pools und tools

Elastische Pools in SQL Datenbank kombiniert Mieter Isolation und Ressourcenfreigabe zwischen Mieter Datenbanken besser unterstützen den Datenbank pro Mieter Ansatz. SQL-Datenbank ist eine Lösung Tier SaaS Anbieter mandantenfähigen Anwendung. Die Last der Ressourcenfreigabe zwischen Mieter wechselt von der Anwendungsschicht auf Dienstebene Datenbank. Die Komplexität der Verwaltung und Abfragen von Datenbanken Ebene wird mit elastische Aufträge, elastische Abfrage elastische Transaktionen und die Clientbibliothek elastische Datenbank vereinfacht.

| Anforderungen | Funktionen der SQL-Datenbank |
| ------------------------ | ------------------------- |
| Mieter Isolierung und gemeinsame Nutzung von Ressourcen | [Elastische Pools](sql-database-elastic-pool.md): Ressourcenpool SQL Datenbank reservieren und Freigeben von Ressourcen in unterschiedlichen Datenbanken. Auch können einzelne Datenbanken so viele Ressourcen aus dem Pool Bedarf Kapazität Bedarfsspitzen aufgrund Mieter Arbeitslasten aufzunehmen. Elastische Pool selbst kann nach oben oder unten skaliert werden, je nach Bedarf. Elastische Pools bieten auch Verwaltung und Überwachung und Fehlerbehebung auf Poolebene. |
| Einfache DevOps über Datenbanken | [Elastische Pools](sql-database-elastic-pool.md): wie oben erwähnt.|
||[Elastische Abfrage](sql-database-elastic-query-horizontal-partitioning.md): Abfrage in Datenbanken für reporting Cross- Tenant -Analyse.|
||[Elastische Aufträge](sql-database-elastic-jobs-overview.md): Paket und zuverlässig zu mehreren Datenbanken bereitstellen Datenbankwartung oder Datenbank geändert.|
||[Elastische Transaktionen](sql-database-elastic-transactions-overview.md): Prozess ändert Datenbanken atomare und isoliert. Elastische Transaktionen sind erforderlich, wenn Applications "Alles oder nichts" Garantien über mehrere Datenbankoperationen benötigen. |
||[Elastische Datenbank-Clientbibliothek](sql-database-elastic-database-client-library.md): Verwalten der Verteilung von Daten und Datenbanken Mieter zuordnen. |

## <a name="shared-models"></a>Freigegebene Modelle

Wie zuvor beschrieben die meisten SaaS-Anbieter kann ein freigegebenes modellansatz Probleme mit Mieter Isolation und Komplexität mit Entwicklung und Wartung darstellen. Allerdings mandantenfähigen handelt, die einen Dienst direkt an den Verbraucher zu ermöglichen, Pächter Isolation Vorschriften mit hoher Priorität als Kosten minimieren möglicherweise nicht. Sie möglicherweise eine oder mehrere Datenbanken in hoher Dichte Kostensenkung Mieter packen. Freigegebene Datenbank Modelle eine einzelne oder mehrere Sharding Datenbanken möglicherweise zusätzliche Effizienz Ressourcen freigeben und allgemeine Kosten. Azure SQL-Datenbank bietet einige Features, mit denen Kunden Isolation für Sicherheit und Verwaltung auf der Datenebene Ebene erstellen.

| Anforderungen | Funktionen der SQL-Datenbank |
| ------------------------ | ------------------------- |
| Isolation Sicherheitsfunktionen | [Zeilenbasierter Sicherheit](https://msdn.microsoft.com/library/dn765131.aspx) |
|| [Datenbankschema](https://msdn.microsoft.com/library/dd207005.aspx) |
| Einfache DevOps über Datenbanken | [Elastische Abfrage](sql-database-elastic-query-horizontal-partitioning.md) |
|| [Elastische Aufträge](sql-database-elastic-jobs-overview.md) |
|| [Elastische Transaktionen](sql-database-elastic-transactions-overview.md) |
|| [Elastische Datenbank-Clientbibliothek](sql-database-elastic-database-client-library.md) |
|| [Elastische Datenbank Teilen und Zusammenführen](sql-database-elastic-scale-overview-split-and-merge.md) |

## <a name="summary"></a>Zusammenfassung

Mieter Isolation Vorschriften sind für SaaS mandantenfähigen meisten wichtig. Die optimale Option isolieren beugt stark Datenbank pro Mieter Ansatz. Die beiden anderen Ansätze erfordern Investitionen in komplexen Anwendungsebenen, die erfordern qualifizierte Entwicklungspersonal zu isolieren, die Kosten und Risiken erhöht. Isolation Vorschriften nicht früh in der Entwicklung berücksichtigt werden, können sie also teurer in den ersten beiden Modellen sollten. Die Hauptnachteile Datenbank pro Mieter Modell zugeordnet beziehen sich auf höhere Cloud Kosten durch geringere freigeben und verwalten viele Datenbanken verwalten. SaaS-Anwendungsentwickler kämpfen häufig bei diese Kompromisse.

Zwar Kompromisse Hürden mit den meisten Cloud Datenbank beseitigt Azure SQL-Datenbank Hindernisse mit elastischen Pool elastische Datenbankfunktionen. SaaS-Entwickler die Isolation Merkmale eines Modells Datenbank pro Mieter kombinieren und Ressourcenfreigabe und eine verbesserte Verwaltung von vielen Datenbanken mit elastischen Pools und Tools zu optimieren.

Mandantenfähigen Anwendungsanbietern, die keine Mieter Isolation Vorschriften und können die Mandanten in einer Datenbank in hoher Dichte Pack, feststellen, dass freigegebene Daten zusätzliche Effizienz Ressourcenfreigabe Ergebnis Modelle und Kosten senken. Azure SQL-Datenbank elastische Datenbanktools, Sharding-Bibliotheken und Sicherheitsfunktionen helfen SaaS-Anbieter erstellen und Verwalten von mandantenfähigen Applikationen.

## <a name="next-steps"></a>Nächste Schritte

[Erste Schritte mit elastische Datenbanktools](sql-database-elastic-scale-get-started.md) eine Beispiel-App, die Clientbibliothek veranschaulicht.

Erstellen Sie ein [benutzerdefiniertes Dashboard SaaS elastischen Pool](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools-custom-dashboard) mit eine Beispielapp, die elastische Pools für eine kosteneffiziente, skalierbare datenbanklösung verwendet.

Verwenden Sie die Azure SQL-Datenbank migrieren [Datenbanken skalieren](sql-database-elastic-convert-to-use-elastic-tools.md).

Unser Lernprogramm zum [Erstellen eines elastischen Pools](sql-database-elastic-pool-create-portal.md)anzeigen  

Informationen zum [Überwachen und Verwalten einer elastischen](sql-database-elastic-pool-manage-portal.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Was ist ein Azure elastischen Pool?](sql-database-elastic-pool.md)
- [Dezentrales Skalieren mit Azure SQL-Datenbank](sql-database-elastic-scale-introduction.md)
- [Mandantenfähigen Applikationen elastische Datenbanktools mit Sicherheit auf Zeilenebene](sql-database-elastic-tools-multi-tenant-row-level-security.md)
- [Authentifizierung in mandantenfähigen apps mithilfe von Azure Active Directory und OpenID verbinden](../guidance/guidance-multitenant-identity-authenticate.md)
- [Tailspin Umfragen Anwendung](../guidance/guidance-multitenant-identity-tailspin.md)
- [Schnelle Lösung beginnt](sql-database-solution-quick-starts.md)

## <a name="questions-and-feature-requests"></a>Feature- Anfragen

Bei Fragen finden Sie uns im [Forum SQL-Datenbank](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted). Eine Funktion Anforderung [Bewertungsportal SQL-Datenbank](https://feedback.azure.com/forums/217321-sql-database/)hinzufügen.
