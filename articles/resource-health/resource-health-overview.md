<properties
   pageTitle="Azure Ressourcenübersicht Gesundheit | Microsoft Azure"
   description="Überblick über Zustand der Azure-Ressourcen"
   services="Resource health"
   documentationCenter="dev-center-name"
   authors="BernardoAMunoz"
   manager=""
   editor=""/>

<tags
   ms.service="resource-health"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="Supportability"
   ms.date="06/01/2016"
   ms.author="BernardoAMunoz"/>

# <a name="azure-resource-health-overview"></a>Azure Ressourcenübersicht Gesundheit

Zustand der Azure-Ressourcen ist ein Dienst, der den Zustand der einzelnen Azure Ressourcen verfügbar macht und umsetzbare Anleitung zur Problembehandlung. In einer Cloud-Umgebung, wo man direkt Zugriff auf Server und Infrastrukturen nicht, soll Ressource Gesundheit schneller verbringen Kunden zur Fehlerbehebung, insbesondere reduzieren die Zeit bestimmen, liegt die Ursache für das Problem in der Anwendung oder durch ein Ereignis innerhalb der Azure-Plattform verursacht wird.

## <a name="what-is-considered-a-resource-and-how-does-resource-health-decides-if-the-resource-is-healthy-or-not"></a>Was ist eine Ressource als und entscheidet Zustand der Ressourcen wie funktioniert, ob die Ressource fehlerfrei ist? 
Eine Ressource ist erstellte Instanz eines Ressourcentyps z. B. von einem Dienst bereitgestellt: einen virtuellen Computer, eine Webanwendung oder einer SQL-Datenbank. 

Zustand der Ressourcen basiert auf Signale die Ressource oder den Dienst zu ermitteln, ob eine Ressource fehlerfrei ist. Es ist wichtig, derzeit Zustand nur Ressourcenkonten für den Zustand einer bestimmten Ressource geben und andere Elemente, die den Gesamtzustand beitragen können nicht berücksichtigt. Beispielsweise wird bei der Status eines virtuellen Computers nur die Compute-Teil der Infrastruktur gilt also Probleme im Netzwerk nicht im Zustand Ressourcen angezeigt werden ohne deklarierte Dienstausfall, es durch das Banner am oberen Rand der Blade angezeigt werden. Weitere Informationen zum Dienstausfall wird später in diesem Artikel angeboten. 

## <a name="how-is-resource-health-different-from-service-health-dashboard"></a>Wie unterscheidet sich Ressource Health Service Health Dashboard?

Informationen vom Zustand der Ressourcen ist detaillierter als von Service Health Dashboard bereitgestellt werden. SHD Ereignisse kommuniziert, die die Verfügbarkeit eines Dienstes in einer Region auswirken, Zustand Ressourcen stellt Informationen über eine bestimmte Ressource, z. B. Stellen sie Ereignisse, die die Verfügbarkeit der virtuellen Computer, eine Webanwendung oder einer SQL-Datenbank auswirkt. Beispielsweise wenn ein Knoten unerwartet neu startet, werden Kunden, deren virtuelle Computer auf diesem Knoten ausgeführte, können zu den Grund, warum die VM für einen Zeitraum nicht verfügbar war.   

## <a name="how-to-access-resource-health"></a>Zustand der Ressourcen auf
Über Ressource Health Services stehen 2 Methoden zum Zustand der Ressourcen zugreifen.

### <a name="azure-portal"></a>Azure-Portal
Die Ressource Gesundheit im Azure-Portal enthält detaillierte Informationen über den Zustand der Ressource sowie empfohlene Aktionen, die den aktuellen Zustand der Ressource abhängig. Dieses Blatt bietet optimal beim Abfragen Zustand Ressourcen Zugriff auf andere Ressourcen im Portal erleichtert. Wie bereits erwähnt, variieren basierend auf den aktuellen Zustand mehrere empfohlene Aktionen Ressource Gesundheit Blatt:

* Gesunde Ressourcen: Da kein Problem, die Integrität der Ressource beeinträchtigen erkannt wurde, die Aktionen auf die Problembehandlung konzentrieren. Beispielsweise bietet direkten Zugriff auf Problembehandlung Blade bietet wie die gängigsten Probleme der Kunden zu lösen.
* Fehlerhafte Ressource: für Probleme von Azure zeigt das Blade Aktionen Microsoft ist (oder hat) um die Ressource wiederherzustellen. Probleme durch Benutzer initiierte Aktionen, Blade wird eine Liste der Aktionen Kunden nehmen so das Problem und die Ressource wiederherzustellen.  

Nach Anmeldung Azure-Verwaltungsportal zweierlei auf Ressource Gesundheit Blade: 

###<a name="open-the-resource-blade"></a>Öffnen Sie das Blade Ressource
Öffnen Sie das Blade Ressource für eine bestimmte Ressource. Die neben Blade Ressource öffnet Blatt Einstellungen klicken Sie auf Ressource Gesundheit Blade Health Ressource geöffnet. 

![Ressource Gesundheit blade](./media/resource-health-overview/resourceBladeAndResourceHealth.png)

### <a name="help-and-support-blade"></a>Hilfe- und Support-blade
Öffnen Sie das Hilfe- und Supportcenter Blade auf das Fragezeichen in der oberen rechten Ecke klicken Sie Hilfe und Support auswählen. 

**In der oberen Navigationsleiste**

![Hilfe und support](./media/resource-health-overview/HelpAndSupport.png)

Auf der Kachel Öffnet das Ressource Gesundheit Abonnement Blade aller Ressourcen in Ihrem Abonnement auflisten. Neben jeder Ressource wird ein Symbol für die Gesundheit. Für jede Ressource öffnet Ressourcen Health Blade.

**Ressource Gesundheit Kachel**

![Ressource Gesundheit Kachel](./media/resource-health-overview/resourceHealthTile.png)

## <a name="what-does-my-resource-health-status-mean"></a>Was meine Gesundheit Ressourcenstatus?
Es sind 4 verschiedenen Status, die für die Ressource angezeigt werden.

### <a name="available"></a>Verfügbar
Der Dienst hat nicht Probleme in der Plattform, die die Verfügbarkeit der Ressource beeinträchtigen könnte. Dies wird durch ein grünes Häkchen angezeigt. 

![Ressource ist verfügbar](./media/resource-health-overview/Available.png)

### <a name="unavailable"></a>Nicht verfügbar

In diesem Fall festgestellt der Dienst ein fortwährendes Problem der Plattform, die beeinträchtigen die Verfügbarkeit dieser Ressource, z. B. der Knoten, auf dem die VM ausgeführt wurde, unerwartet neu gestartet. Dies wird durch ein rotes Warnsymbol angezeigt. Zusätzliche Informationen erfolgt im mittleren Bereich des Blades, einschließlich: 

1.  Welche Aktionen Microsoft wird zum Wiederherstellen der Ressource 
2.  Eine detaillierte Zeitachse des Problems, einschließlich der Beendigung
3.  Eine Liste empfohlene Maßnahmen für Benutzer 

![Ressource ist nicht verfügbar](./media/resource-health-overview/Unavailable.png)

### <a name="unavailable--customer-initiated"></a>Nicht verfügbar-Kunden
Die Ressource wird aufgrund einer Kundenanfrage oder Beenden einer Ressource einen Neustart angefordert. Dies wird durch einen blauen Informationssymbol angezeigt. 

![Ressource ist nicht verfügbar, da Benutzer initiierte Aktion](./media/resource-health-overview/userInitiated.png)

### <a name="unknown"></a>Unbekannt
Der Dienst hat keine Informationen zu dieser Ressource mehr als 5 Minuten. Dies wird durch eine graue Fragezeichen angezeigt. 

Es ist wichtig zu beachten, dass dies keine endgültige Angabe, dass es ein Problem mit einer Ressource so Kunden diese Empfehlung befolgen:

* Wenn Ressource wie erwartet ausgeführt wird, aber deren Status unbekannt Ressource Gesundheit festgelegt ist, gibt es keine Probleme und erwarten Sie den Status der Ressource gesunden nach einigen Minuten aktualisiert.
* Gibt es Probleme beim Zugriff auf die Ressource und deren Status unbekannt Ressource Gesundheit festgelegt ist, dies sollte ein frühes Anzeichen sein könnte ein Problem und Untersuchungen geschehen bis zu fehlerfrei oder fehlerhaft der Zustand aktualisiert wird

![Zustand der Ressourcen ist unbekannt](./media/resource-health-overview/unknown.png)

## <a name="service-impacting-events"></a>Dienstereignisse beeinträchtigen
Wenn die Ressource eine kontinuierliche Service beeinträchtigen Ereignis betroffen sein könnten, wird ein Banner oben Blade Health Ressource angezeigt. Für das Banner öffnet Blade Überwachungsereignisse, das Ausfallzeit zusätzliche Informationen anzeigen.

![Zustand der Ressourcen kann eine SIE beeinflusst werden](./media/resource-health-overview/serviceImpactingEvent.png)

## <a name="what-else-do-i-need-to-know-about-resource-health"></a>Was muss ich wissen Zustand Ressourcen?

### <a name="signal-latency"></a>Signal-Wartezeit
Signale, die Ressource Gesundheit feed möglicherweise bis zu 15 Minuten verzögert, Diskrepanzen zwischen den aktuellen Status der Ressource und der tatsächlichen Verfügbarkeit führen. Es ist wichtig, diese Bedenken hilft nicht benötigte Zeit Untersuchung möglicher Probleme beseitigen. 

### <a name="special-case-for-sql"></a>Sonderfall für SQL 
Zustand der Ressourcen meldet den Status der SQL-Datenbank nicht den SQL-Server. Während diese Route ein realistischeres Bild Health bietet, müssen mehrere Komponenten und Dienste berücksichtigen den Zustand der Datenbank bestimmen. Das aktuelle Signal abhängig von Benutzernamen in der Datenbank, sodass für Datenbanken, die regulären Benutzernamen (einschließlich unter anderem Abfrage Ausführung Anfragen) den Zustand erhalten regelmäßig Status angezeigt werden. Wenn die Datenbank für einen Zeitraum von 10 Minuten oder länger nicht zugegriffen wurde, wird er in unbekannten Status verschoben. Dies bedeutet nicht, dass die Datenbank nicht verfügbar, so dass kein Signal ausgegeben wurde, da keine Benutzernamen ausgeführt wurde. Verbindung mit der Datenbank und Ausführen einer Abfrage gibt die Signale zu bestimmen den Status der Datenbank benötigt.

## <a name="feedback"></a>Feedback
Wir sind immer auf Feedback und Vorschläge. Senden Sie uns Ihre [Vorschläge](https://feedback.azure.com/forums/266794-support-feedback). Darüber hinaus können Sie mit uns über [Twitter](https://twitter.com/azuresupport) oder [MSDN-Foren](https://social.msdn.microsoft.com/Forums/azure)teilnehmen.
