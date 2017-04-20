<properties
   pageTitle="Best Practices für Unternehmen zu Azure | Microsoft Azure"
   description="Beschreibt ein Gerüst, mit dem Unternehmen eine sichere und verwaltbare Umgebung sicherzustellen."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="rdendtler"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="rodend;karlku;tomfitz"/>

# <a name="azure-enterprise-scaffold---prescriptive-subscription-governance"></a>Azure Enterprise Scaffold - unterstützende Abonnement governance

Unternehmen setzen immer die öffentliche Cloud für die Agilität und Flexibilität. Nutzen sie die Cloud Stärken Umsatz oder Ressourcen für Unternehmen optimieren. Microsoft Azure bietet einer Vielzahl von Dienstleistungen, Unternehmen wie Bausteine an eine Vielzahl von Workloads und Applikationen zusammenstellen können. 

Aber zu wissen ist oft schwierig. Nach der Entscheidung zur Azure verwenden, treten Fragen im Allgemeinen:

- "Wie wird treffen unsere rechtlichen Daten Souveränität in bestimmten Ländern?"
- "Wie sicherstellen kann ich, dass jemand kritische Systemfehler nicht versehentlich ändern?"
- "Wie wissen kann ich was jede Ressource unterstützt, damit ich machen können und Rechnung wieder korrekt?"

Der Interessent eine leere Abonnement keine Geländer ist entmutigend. Diese Leerzeichen kann Ihren Umzug in Azure behindern.

Dieser Artikel bietet einen Ausgangspunkt für technisches Fachpersonal die Notwendigkeit für Governance und es mit der Flexibilität. Das Konzept einer Enterprise-Gerüst, das Organisationen implementieren und Verwalten von Azure-Abonnements führt. 

## <a name="need-for-governance"></a>Erforderlich für governance

Beim Verschieben in Azure müssen Sie das Thema Governance bereits den erfolgreichen Einsatz der Cloud innerhalb des Unternehmens sichergestellt. Leider Zeit und Bürokratie erstellen eine umfassende Governance bedeutet, dass sich einige Unternehmensgruppen an Kreditoren ohne Unternehmen. Dadurch kann Unternehmen geöffnet lassen Schwachstellen Wenn die Ressourcen nicht richtig verwaltet werden. Die Merkmale der öffentlichen Cloud Agilität, Flexibilität und Verbrauch Preise - liegen Unternehmensgruppen, die schnell Anforderungen Kunden (intern und extern). Aber Unternehmen muss sicherstellen, dass Daten und Systeme geschützt werden.

In der Praxis wird auf der Grundlage der Struktur Gerüstbau verwendet. Das Gerüst führt die Grundzüge und bietet Ankerpunkte für permanente Systeme bereitgestellt werden. Ein Enterprise-Gerüst entspricht: eine Reihe von flexiblen Steuerelemente und Azure-Funktionen, die Struktur der Umgebung und Anker für basierend auf der öffentlichen Cloud-Dienste bereitstellen. Bietet die Generatoren (IT und Geschäft) Foundation erstellen und neue Dienste.

Das Gerüst basiert auf Methoden gesammelten aus vielen Projekten für Kunden mit verschiedenen Größen. Diese Clients reichen von kleinen Unternehmen in der Cloud Fortune 500-Unternehmen und unabhängige Softwareanbieter, Migration und Lösungen in der Cloud, entwickeln. Enterprise-Gerüst ist "speziell" flexibel traditionellen IT-Arbeitslasten und agile Arbeitslasten; Azure Funktionen wie Entwickler Software-as-a-Service (SaaS) Applikationen Grundlage.

Enterprise-Gerüst soll die Grundlage für jedes neue Abonnement in Azure. Sie können Administratoren gewährleisten Arbeitslasten minimale gesetzliche Auflagen einer Organisation ohne Geschäft und Entwickler von schnell ihre eigenen Ziele.

> [AZURE.IMPORTANT] Governance ist entscheidend für den Erfolg von Azure. Dieser Artikel zielt die technische Implementierung einer Enterprise-Gerüst aber nur berührt den umfassenderen Prozess und Beziehung zwischen den Komponenten. Richtlinie Governance fließt von oben nach unten und festgelegten das Unternehmen zu erreichen will. Natürlich beinhaltet die Erstellung ein für Azure Vertretern IT, aber wichtiger sollten starke Vertretung Gruppe Führungskräfte und Sicherheit und Risikomanagement. Am Ende ist ein Gerüst Enterprise über geschäftliche Risiken einer Organisation Mission und Ziele.

Die folgende Abbildung beschreibt die Komponenten des Gerüsts. Die Foundation basiert auf ein solider Plan für Kostenstellen, Konten und Abonnements. Die Säulen bestehen aus Ressourcenmanager Richtlinien und starken Benennungsstandards. Die restlichen Scaffold stammt aus Azure Funktionen und features, mit denen eine sichere und verwaltbare Umgebung.

![Scaffold-Komponenten](./media/resource-manager-subscription-governance/components.png)

> [AZURE.NOTE] Azure ist seit seiner Einführung im Jahr 2008 schnell gewachsen. Dieses Wachstum benötigt Microsoft engineering-Teams Ansatz für die Verwaltung und Bereitstellung von Diensten zu überdenken. Azure-Ressourcen-Manager-Modell wurde 2014 eingeführt und ersetzt das klassische Bereitstellungsmodell. Ressourcen-Manager können Unternehmen leichter bereitstellen, organisieren und Azure-Ressourcen steuern. Ressourcen-Manager enthält Parallelisierung beim Erstellen von Ressourcen für schnellere Bereitstellung komplexer, voneinander abhängiger Lösungen. Darüber hinaus detaillierte Zugriffskontrolle und die Möglichkeit, Tag-Ressourcen mit Metadaten. Microsoft empfiehlt, alle Ressourcen über Ressourcen-Manager-Modell zu erstellen. Enterprise-Gerüst dient explizit der Ressourcen-Manager-Modell.

## <a name="define-your-hierarchy"></a>Definieren der Hierarchie

Die Grundlage des Gerüsts ist Azure Enterprise Agreement (und Enterprise Portal). Enterprise Agreement legt die Form und Azure Dienste innerhalb eines Unternehmens und Verwaltungsstruktur Core. In Enterprise Agreement können Kunden Weitere Umgebung in Abteilungen, Konten und schließlich Abonnements unterteilen. Azure-Abonnement ist die Grundeinheit, in dem alle Ressourcen enthalten sind. Außerdem werden verschiedene Grenzen Azure wie Anzahl der Kerne, Ressourcen usw. definiert.

![Hierarchie](./media/resource-manager-subscription-governance/agreement.png)

Jedes Unternehmen ist anders, und die Hierarchie in der vorherigen Abbildung erhebliche Flexibilität bei der Organisation Azure innerhalb des Unternehmens ermöglicht. Vor dem Implementieren der Anleitung in diesem Dokument enthaltenen Sie modellieren die Hierarchie und die Auswirkungen Abrechnung Ressourcenzugriff und Komplexität.

Drei allgemeine Muster für Azure Anmeldungen sind:

- **Funktionale** Muster

    ![funktionale](./media/resource-manager-subscription-governance/functional.png)

- **Business Unit** -Muster 

    ![Unternehmen](./media/resource-manager-subscription-governance/business.png)

- **Geografische** Muster

    ![geografische](./media/resource-manager-subscription-governance/geographic.png)

Sie übernehmen das Gerüst auf Abonnementebene Governance Vorschriften des Unternehmens in das Abonnement erweitern.

## <a name="naming-standards"></a>Namensstandards

Pfeiler Gerüst nennt Standards. Gut entworfene Benennungsstandards können Sie Ressourcen im Portal in eine Stückliste und in Skripts zu identifizieren. Wahrscheinlich verfügen Sie bereits über Benennungsstandards für lokale Infrastruktur. Wenn Ihre Umgebung Azure hinzufügen, sollten Sie diese Benennungsstandards Azure Ressourcen erweitern. Benennung Standard eine effizientere Verwaltung der Umgebung auf allen Ebenen erleichtern.

> [AZURE.TIP]
>
> - Überprüfen und übernehmen wo möglich die [Patterns and Practices-Leitfaden](./guidance/guidance-naming-conventions.md). Diese Anleitung hilft Ihnen auf einen sinnvollen Benennungsstandard entscheiden.
> - Verwenden Sie CamelCasing für Ressourcen (wie MyResourceGroup und VnetNetworkName). Hinweis: Gibt es bestimmte Ressourcen, wie Speicher nur ist Kleinbuchstaben (und keine Sonderzeichen).
> - Verwenden Sie Azure-Ressourcen-Manager-Richtlinien (im nächsten Abschnitt beschrieben) Benennungsstandards erzwingen.
 
## <a name="policies-and-auditing"></a>Richtlinien und Überwachung

Säule Gerüst umfasst das Erstellen von [Azure-Ressourcen-Manager-Richtlinien](resource-manager-policy.md) und [Überwachung des Aktivitätsprotokolls](resource-group-audit.md). Ressourcen-Manager-Richtlinien bieten Ihnen Risikomanagement in Azure. Sie können Richtlinien definieren, die datensouveränität einschränken, erzwingen oder bestimmte Aktionen überwachen sicherstellen. 

- Richtlinie ist ein Standard **ermöglichen** . Sie steuern die Aktionen definieren und Zuweisen von Richtlinien zu Ressourcen, die zu verweigern oder Aktionen auf Ressourcen überwachen.
- Richtlinien werden durch Policy-Definitionen in der Policy Definition Language (Wenn-dann-Bedingung).
- Sie erstellen Richtlinien mit JSON (Javascript Object Notation) formatierte Dateien. Nach dem Definieren einer Richtlinie zuweisen es an einen bestimmten Bereich: Abonnement, Ressourcengruppe. oder Ressource.

Richtlinien sind mehrere Aktionen, die eine differenzierte Ansatz Szenarien zu ermöglichen. Die Aktionen sind:

-   **Ablehnen**: die Anforderung blockiert
-   **Audit**: die Anforderung ermöglicht jedoch das Aktivitätsprotokoll (die Alarme oder Runbooks auslösen verwendet werden kann) eine Linie hinzugefügt
-   **Append**: die Ressource angegebenen Informationen hinzugefügt. Wenn ein Tag "CostCenter" eine Ressource nicht vorhanden ist, fügen Sie Tags mit einem Standardwert ein.

### <a name="common-uses-of-resource-manager-policies"></a>Gemeinsame Nutzung der Ressourcen-Manager-Richtlinien

Azure Ressourcen-Manager-Richtlinien sind ein leistungsfähiges Tool in den Azure. Sie können zu unerwarteten Kosten eine Kostenstelle für Ressourcen durch tagging zu identifizieren und sicherzustellen, dass Konformität erfüllt. Kombination von Richtlinien mit integrierten Überwachungsfeatures können komplex und flexible Lösung Mode. Richtlinien können Unternehmen Steuerelemente für Arbeitslasten "Traditionelle IT" und "Agile" Arbeitslasten. Kunden wie entwickeln. Die am häufigsten verwendeten Richtlinien sehen wir sind:

- **Geo-Konformitätsdaten Hoheit** - Azure bietet Regionen weltweit. Häufig möchten Unternehmen steuern Ressourcen entstehen (ob datenhoheit oder um sicherzustellen, dass Ressourcen nah Ende der Ressourcen erstellt werden).
- **Kosten-Management** – ein Azure-Abonnement enthalten Ressourcen vieler Typen und skalieren. Häufig möchten Unternehmen standard-Abonnements zu vermeiden unnötig Ressourcen, die Hunderte von Dollar pro Monat oder mehr Kosten können.
- **Governance durch erforderlichen Tags Standard** - Tags erfordern ist eine der häufigsten und begehrten. Azure Ressourcenmanager Richtlinien können Unternehmen sicherstellen, dass eine Ressource entsprechend gekennzeichnet ist. Die am häufigsten verwendeten Tags sind: Abteilung, Ressourcenbesitzer und Umgebung Typ (-Produktion, Test, Entwicklung)

**Beispiele**

"Traditionelle IT" Abonnement für Line of Business Applications

-   Tags für Abteilung und Besitzer für alle Ressourcen erzwingen
-   Erstellen von Ressourcen in Nordamerika Bereich beschränken
-   Einschränken der G-Serie VMs und HDInsight-Cluster erstellen

"Agile" Umgebung für eine Unternehmenseinheit erstellen Cloudanwendungen

- Daten Hoheit Anforderungen ermöglichen die Erstellung von Ressourcen nur in einer bestimmten Region.
- Umgebung-Tag für alle Ressourcen zu erzwingen. Wenn ohne ein Tag eine Ressource erstellt wurde, fügen Sie der **Umgebung: Unbekannte** Tag auf die Ressource.
- Prüfung auf Ressourcen außerhalb Nordamerikas erstellt, aber nicht verhindert.
- Prüfung bei hohen Kosten Ressourcen erstellt werden.

> [AZURE.TIP]
>
> Die häufigste Verwendung der Ressourcen-Manager-Richtlinien Unternehmen ist Steuerelement *, in dem* Ressourcen erstellt werden und *welche Ressourcen* erstellt werden. Viele Unternehmen verwenden neben Steuerelemente *und *welche** Richtlinien um sicherzustellen, dass Ressourcen die entsprechende Metadaten für Verbrauch in Rechnung. Wir empfehlen für Ebene Abonnement:
>
> - Geo-Konformitätsdaten Hoheit
> - Kostenmanagement
> - Erforderliche Tags (festgelegt durch Geschäftsbedarf wie BillTo Anwendungsbesitzer)
>
> Sie können zusätzliche Richtlinien auf niedrigeren Ebenen Bereich anwenden.
 
### <a name="audit---what-happened"></a>Audit - was?

Zum Anzeigen der Funktionsweise Ihrer Umgebung müssen Sie Benutzeraktivitäten zu überwachen. Die meisten Ressourcentypen in Azure erstellen Diagnoseprotokolle über ein Log-Tool oder in Azure Operations Management Suite analysieren können. Aktivitätsprotokolle sammeln Sie über mehrere Abonnements zu einer Abteilung oder Unternehmen anzeigen. Datensätze sind ein wichtiges diagnostic Tool und ein wichtiger Mechanismus auf triggerereignisse in der Azure-Umgebung.

Aktivitätsprotokolle vom Ressourcenmanager Bereitstellung können Sie **Vorgänge** ermitteln, die durchgeführt wurde platzieren und wer Sie erledigt hat. Aktivitäten können gesammelt und mithilfe von Tools wie Protokollanalyse zusammengefasst.

## <a name="resource-tags"></a>Resource-tags

Benutzer in Ihrer Organisation das Abonnement Ressourcen hinzufügen, wird es zunehmend entsprechenden Abteilung, Kunden und Umgebung Ressourcen zuordnen. Sie können Metadaten Ressourcen durch [Tags](resource-group-using-tags.md)hinzufügen. Verwenden Sie Tags Informationen der Ressource oder der Besitzer. Tags können nicht nur zusammenfassen und Gruppieren von Ressourcen auf verschiedene Weise, aber Daten zwecks Chargeback. Sie können Ressourcen mit 15 Schlüssel-Wert-Paare markieren. 

Resource-Tags sind flexibel und an die meisten Ressourcen. Beispiele für Resource-Tags sind:

- Rechnungsadresse
- Abteilung (oder Unternehmenseinheit)
- Umgebung (Phase Produktionsentwicklung)
- Ebene (Webebene, Anwendungsebene)
- Besitzer der Anwendung
- Projektname

![Tags](./media/resource-manager-subscription-governance/resource-group-tagging.png)

Weitere Beispiele für Tags finden Sie unter [Namenskonventionen für Azure Ressourcen empfohlen](./guidance/guidance-naming-conventions.md).

> [AZURE.TIP]
>
> Erstellen einer Tag-Strategie, die über Abonnements identifiziert, welche Metadaten für Business, Finanzen, Sicherheit, Risikomanagement und allgemeine Verwaltung der Umgebung benötigt werden. Erwägen Sie eine Richtlinie, die Vorschriften für die Kennzeichnung:
>
> - Ressourcengruppen
> - Speicher
> - Virtuelle Computer
> - Service-Umgebung/Webservern

## <a name="resource-group"></a>Ressourcengruppe 

Ressourcen-Manager können Sie Ressourcen in sinnvollen Gruppen für Management, Rechnungsadresse oder natürliche Affinität setzen. Wie bereits erwähnt, hat Azure zwei Bereitstellungsmodelle. In früheren Klassisch war die Basiseinheit Management das Abonnement. Es war schwer Ressourcen innerhalb eines Abonnements zur Erstellung einer großen Anzahl von Abonnements führte. Mit dem Ressourcen-Manager wurden wir von Ressourcengruppen. Ressourcengruppen sind Container für Ressourcen, die gemeinsame Lebenszyklus oder ein Attribut wie "alle SQL Server" oder "A".

Ressourcengruppen können nicht ineinander enthalten und Ressourcen können nur einer Ressourcengruppe angehören. Sie können bestimmte Aktionen für alle Ressourcen in einer Ressourcengruppe anwenden. Löschen einer Ressourcengruppe entfernt beispielsweise alle Ressourcen in der Ressourcengruppe. Normalerweise stellen Sie eine gesamte Anwendung oder verknüpfte in derselben Ressourcengruppe. Eine dreistufige Anwendung namens Contoso Web-Anwendung würde z. B. Webserver, Anwendungsserver und SQL Server in derselben Ressourcengruppe enthalten.

> [AZURE.TIP]
>
> Wie Sie Ressourcengruppen organisieren variieren "Traditionelle IT" Arbeitslasten "Agile IT" Arbeitslasten von
>
> - "Traditionelle IT" Arbeitslasten werden am häufigsten von Elementen innerhalb der gleichen Lebenszyklus wie eine Anwendung gruppiert. Gruppierung von Anwendung kann einzelne Anwendungsmanagement.
> - "Agile IT" Arbeitslasten neigen dazu, auf externe Kunden Cloudanwendungen. Ressourcengruppen sollten Ebenen Bereitstellung (z. B. Webebene App Tier) und Management.

## <a name="role-based-access-control"></a>Rollenbasierte Zugriffskontrolle

Sie werden wahrscheinlich Fragen "sollte Zugriff auf Ressourcen haben?" und "wie ich diesen Zugriff kontrollieren?" Ermöglicht oder verhindert Zugriff auf Azure-Portal und Steuern des Zugriffs auf Ressourcen im Portal ist entscheidend. 

Azure ursprünglich veröffentlicht wurde, wurden Zugriffskontrolle zu einem Abonnement: oder Co-Administrator. Zugriff auf ein Abonnement im klassischen Modell impliziert Zugriff auf alle Ressourcen im Portal. Diese mangelnde detaillierte Kontrolle führte zu Abonnements bieten einen angemessenen Zugriffskontrolle für eine Azure.

Diese Verbreitung von Abonnements nicht mehr benötigt. Mit rollenbasierter Zugriffskontrolle können Sie Benutzer Standardrollen (z. B. "Leser" und "Autor" gängigen Funktionen) zuweisen. Sie können auch benutzerdefinierte Rollen definieren.

> [AZURE.TIP]
>  
> - Verbinden der unternehmenseigenen Identitätsdatenbank (meistens Active Directory) in Azure Active Directory mit dem Tool Active Directory verbinden.
> - Steuern Sie den Admin/Co-Admin ein Abonnement mit einer verwalteten Identität. **Nicht** zuweisen eine neue Abonnementbesitzer Admin, Co-Admin. Verwenden Sie stattdessen RBAC-Rollen **Besitzerrechte** , einer Gruppe oder einzelne bereitstellen.
> - Azure Benutzer einer Gruppe fügen Sie hinzu (z. B. Anwendungseigentümer X) in Active Directory. Verwendung der synchronisierten Gruppe Mitglieder der Gruppe die entsprechenden Rechte für die Ressourcengruppe, die Anwendung zu verwalten.
> - Folgen Sie dem Prinzip der erwarteten arbeiten erforderliche **Mindestberechtigung** gewähren. Zum Beispiel:
>     - Bereitstellung Gruppe: Eine Gruppe, die nur Ressourcen bereitstellen.
>     - Verwaltung virtueller Computer: Eine Gruppe (für Vorgänge) VMs neu starten

## <a name="azure-resource-locks"></a>Azure Ressourcensperren

Ihre Organisation das Abonnement Basisdienste hinzufügt, wird es immer wichtiger, sicherzustellen, dass diese Dienste zu Geschäftsbetriebs. [Ressourcensperren](resource-group-lock-resources.md) können Sie beschränken auf hochwertige Ressourcen, ändern oder löschen auf Anwendung und Cloud-Infrastruktur auswirken würde. Sie können ein Abonnement, eine Ressourcengruppe oder eine Ressource sperren zuweisen. In der Regel weisen Sie Sperren grundlegenden Ressourcen wie virtuelle Netzwerke, Gateways und Speicherkonten. 

Ressourcensperren unterstützt derzeit zwei Werte: CanNotDelete und ReadOnly. CanNotDelete bedeutet, die immer Benutzer (mit den entsprechenden Berechtigungen noch) lesen oder ändern eine Ressource aber nicht löschen. ReadOnly bedeutet, dass nicht autorisierte Benutzer löschen oder eine Ressource ändern.

Erstellen oder Löschen von Management sperren, haben Sie Zugriff auf `Microsoft.Authorization/*` oder `Microsoft.Authorization/locks/*` Aktionen.
Integrierte Rollen werden diese Aktionen nur Besitzer und Benutzer Zugriff Administrator erteilt werden.

> [AZURE.TIP] Core-Netzwerkoptionen sollte mit Sperren geschützt werden. Das versehentliche Löschen von Gateway wäre Standort-zu-Standort-VPN verheerende Azure-Abonnement. Azure nicht möglich, ein virtuelles Netzwerk löschen, das verwendet wird, aber mehr eingeschränkt ist Vorsichtsmaßnahme hilfreich. Richtlinien sind auch die Verwaltung der entsprechenden Steuerelemente. Wir empfehlen übernehmen eine **CanNotDelete** Sperre Richtlinien, die in Verwendung sind.
>
> - Virtuelles Netzwerk: CanNotDelete
> - Netzwerk-Sicherheitsgruppe: CanNotDelete
> - Richtlinien: CanNotDelete

## <a name="core-networking-resources"></a>Core-Netzwerkressourcen

Ressourcen können intern (innerhalb des Unternehmens-Netzwerk) oder extern (über Internet) zugänglich. Es ist leicht für Benutzer in Ihrer Organisation versehentlich Ressourcen an der falschen Stelle und potenziell böswilligen Zugriff öffnen. Mit lokalen Geräten müssen Unternehmen angemessene Kontrollen, um sicherzustellen, dass Benutzer Azure Entscheidungen hinzufügen. Abonnement Governance ermitteln wir die wichtigsten Ressourcen, die grundlegende Steuerung des Zugriffs. Core-Ressourcen umfassen:

- **Virtuelle Netzwerke** sind Containerobjekte Teilnetze. Obwohl nicht unbedingt notwendig, wird häufig verwendet Verbindung Anträge auf interne Unternehmensressourcen.
- **Netzwerk-Sicherheitsgruppen** ähnelt einer Firewall und Regeln für wie eine Ressource "kommunizieren" kann über das Netzwerk bereitzustellen. Geben sie präzise steuern, wie wenn ein Subnetz (oder virtuellen Computer) im Internet oder in anderen Subnetzen im gleichen virtuellen Netzwerk herstellen können.

![Core-Netzwerk](./media/resource-manager-subscription-governance/core-network.png)

> [AZURE.TIP]
>  
> - Erstellen Sie virtuelle Netzwerke Arbeitslasten externen und internen Arbeitslasten gewidmet. Dies verringert die Wahrscheinlichkeit versehentlich platzieren virtuelle Computer, die für interne Arbeitslasten in einer externen gegenüberliegende vorgesehen sind.
> - Netzwerk-Sicherheitsgruppen sind entscheidend für diese Konfiguration. Mindestens Zugriff auf das Internet über virtuelle Netzwerke und Zugriff auf das Unternehmensnetzwerk von externen virtuellen Netzwerken.

### <a name="automation"></a>Automatisierung

Verwalten von Ressourcen einzeln zeitaufwendig ist und möglicherweise für bestimmte Vorgänge. Azure bietet verschiedene Automatisierungsfunktionen einschließlich Azure Automation, Logik-Apps und Azure-Funktionen. [Azure Automation](./automation/automation-intro.md) können Administratoren erstellen und Definieren von Runbooks allgemeine Aufgaben bei der Verwaltung von Ressourcen. Erstellen Runbooks mit PowerShell-Code-Editor oder einem Grafik-Editor. Sie können komplexe mehrstufige Workflows erstellen. Azure Automation wird häufig allgemeine Aufgaben wie Herunterfahren nicht verwendete Ressourcen oder Ressourcen auf einen bestimmten Trigger erstellen, ohne menschlichen Eingriff.

> [AZURE.TIP]
>
> - Erstellen Sie Azure Automation-Konto, und überprüfen Sie die verfügbaren Runbooks (Grafik- und Command Line) in [Runbook Gallery](./automation/automation-runbook-gallery.md).
> - Importieren und wichtige Runbooks zur eigenen Verwendung anpassen.
>
> Ein häufiges Szenario ist die Möglichkeit zum Starten/Herunterfahren virtueller Maschinen auf einem Zeitplan. Es sind Beispiel Runbooks, die im Katalog verfügbar sind, die dieses Szenario behandelt und lernen zu erweitern.


## <a name="azure-security-center"></a>Azure Security Center

Vielleicht war der größte Blocker Annahme Cloud Bedenken Sicherheit. IT-Risiko-Manager und sicherheitsabteilungen müssen sicherstellen, dass Ressourcen in Azure sicher sind. 

[Azure Security Center](./security-center/security-center-intro.md) bietet eine zentrale Ansicht Sicherheitsstatus von Ressourcen in den Subskriptionen und Leitfaden, die gefährdete Ressourcen zu verhindern. Sie können genauere Politiken (z. B. anwenden, mit denen Unternehmen ihre Haltung zu den Risiken anpassen, die sie mit bestimmten Ressourcengruppen). Azure Security Center ist eine offene Plattform, die Microsoft-Partner und unabhängige Softwareanbieter, Software zu erstellen, die an Azure Security Center zur Verbesserung seiner Funktionen. 

> [AZURE.TIP]
>
> Azure Security Center wird standardmäßig in jedem Abonnement aktiviert. Jedoch müssen Sie Datensammlung von virtuellen Maschinen zu Azure Security Center zur Installation der Agents und Daten.
>
> ![Datensammlung](./media/resource-manager-subscription-governance/data-collection.png)

## <a name="next-steps"></a>Nächste Schritte

- Da Abonnements Governance erfahren haben, ist es Zeit, um diese Empfehlung in der Praxis. Beispiele [von Azure-Abonnement Governance implementieren](resource-manager-subscription-examples.md).

*[Karl Kuhnhausen](https://github.com/karlkuhnhausen) Beitrag zu diesem Thema.*
