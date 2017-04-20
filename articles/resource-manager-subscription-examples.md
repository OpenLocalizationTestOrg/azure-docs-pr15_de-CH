<properties
   pageTitle="Szenarien und Beispiele für Abonnement Governance | Microsoft Azure"
   description="Beispiele zum Azure-Abonnement Governance für allgemeine Szenarien implementieren."
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

# <a name="examples-of-implementing-azure-enterprise-scaffold"></a>Beispiele für die Implementierung von Azure Enterprise Gerüst

Dieses Thema enthält Beispiele für die Implementierung empfohlenen für eine [Azure Enterprise Gerüst](resource-manager-subscription-governance.md)von Unternehmen. Wird ein fiktives Unternehmen namens Contoso best Practices für Szenarien veranschaulichen. 

## <a name="background"></a>Hintergrund

Contoso ist ein weltweites Unternehmen, Lösungen für Kunden von einem Modell "Software als Dienst" Modell verpackt lokal bereitgestellt.  Software entwickeln sie weltweit mit Entwicklung in Indien, USA und Kanada. 

ISV Teil des Unternehmens ist in mehrere unabhängige Geschäftsbereiche unterteilt, die Produkte in bedeutende Unternehmen verwalten. Jeder Geschäftsbereich eine eigene Entwickler, Produktmanager und Architekten. 

Geschäftsbereich Enterprise Technology Services (EHs) ermöglicht zentralisierte IT und mehrere Rechenzentren Unternehmenseinheiten ihrer Anwendung Host verwaltet. ETS Organisation und Management von Rechenzentren bietet und verwaltet zentrale Zusammenarbeit (z. B. e-Mail und Websites) und Netzwerk-Telefonie-Services. Sie verwalten für kleinere Unternehmenseinheiten, die Mitarbeiter haben Kunden Arbeitslasten. 

In diesem Thema werden die folgenden Rollen verwendet:

- Dave ist der ETS Azure-Administrator.
- Alice ist Director of Development in der Supply Chain Unternehmenseinheit Contoso. 

Contoso benötigt eine LOB Anwendung und kundenorientierte app. Hat der apps in Azure ausgeführt. Dave [unterstützende Abonnement Governance](resource-manager-subscription-governance.md) Thema liest und kann nun die Empfehlung implementiert. 

## <a name="scenario-1-line-of-business-application"></a>Szenario 1: LOB Anwendung

Contoso erstellt ein Quellcodeverwaltungssystem (BitBucket) von Entwicklern auf der ganzen Welt verwendet werden.  Die Anwendung verwendet Infrastruktur als Service (IaaS) zum Hosten und Webserver und einen Datenbankserver aus. Entwickler Zugriff auf Server in ihrer Entwicklung allerdings nicht benötigen Zugriff auf die Server in Azure. Contoso ETS möchte die Anwendungsbesitzer und Team die Anwendung verwalten können. Die Anwendung ist nur bei Contoso Unternehmensnetzwerk verfügbar. Dave muss das Abonnement für diese Anwendung einrichten. Das Abonnement wird auch zukünftig andere Entwickler Software hosten.  

### <a name="naming-standards--resource-groups"></a>Benennungsstandards und Ressourcengruppen

Dave erstellt eine Subskription Unterstützung für alle Geschäftsbereiche gelten Entwicklertools. Er muss aussagekräftige Namen für Abonnements und Ressourcen (für die Anwendung und die Netzwerke) erstellen. Er erstellt die folgenden Abonnement und Resource-Gruppen:

| Artikel | Name | Beschreibung |
| ---- | ---- | ----------- |
| Abonnement | Contoso ETS Entwicklertools Produktion | Unterstützt Entwickler-tools |
| Ressourcengruppe | rgBitBucket | Enthält die Anwendung Webserver und Datenbankserver |
| Ressourcengruppe | rgCoreNetworks | Enthält das virtuelle Netzwerke und Gateway Standort-zu-Standort-Verbindung |


### <a name="role-based-access-control"></a>Rollenbasierte Zugriffskontrolle

Dave möchte nach seinem Abonnement erstellen, damit die entsprechenden Teams und Anwendungseigentümer Ressourcen zugreifen können. Dave erkennt jedes Team hat andere Vorschriften. Er nutzt die Gruppen von Contoso lokalen Active Directory (AD) in Azure Active Directory synchronisiert und stellt die richtige Zugriffsebene Teams. 

Dave weist die folgenden Funktionen für das Abonnement: 

| Rolle | Zugewiesen | Beschreibung |
| ---- | ----------- | ----------- |
| [Besitzer](./active-directory/role-based-access-built-in-roles.md#owner) | Verwaltete ID von Contoso AD | Diese ID mit Just-in-Time (JIT)-Zugriff über Contoso Identity Management Tool gesteuert und stellt sicher, dass Abonnements Besitzerzugriff vollständig überwacht. |
| [Sicherheits-Manager](./active-directory/role-based-access-built-in-roles.md#security-manager) | Sicherheit und Verwaltung | Diese Rolle ermöglicht Benutzern das Azure Security Center und den Status der Ressourcen. |
| [Netzwerk-Teilnehmer](./active-directory/role-based-access-built-in-roles.md##network-contributor) | Netzwerkteam | Diese Rolle kann Contoso Netzwerk die Standort-zu-Standort-VPN und virtuelle Netzwerke verwalten. |
| *Benutzerdefinierte Rolle* | Besitzer der Anwendung | Dave erstellt eine Rolle, die die Möglichkeit zum Ändern von Ressourcen innerhalb der Ressourcengruppe gewährt. Weitere Informationen finden Sie unter [Benutzerdefinierte Funktionen in Azure RBAC](./active-directory/role-based-access-control-custom-roles.md) |

### <a name="policies"></a>Richtlinien

Dave hat Folgendes zum Verwalten von Ressourcen im Abonnement:

- Da die Entwicklungstools Entwicklern weltweit unterstützen, will er Benutzern Ressourcen in jeder Region erstellen. Er muss jedoch wissen, wo die Ressourcen erstellt werden. 
- Er ist mit Kosten. Deshalb will er verhindern, dass Anwendungseigentümer unn virtuelle Computer erstellen.  
- Da diese Anwendung Entwickler in vielen Unternehmen dient, möchte er jede Ressource mit der Eigentümer der Einheit und Anwendung versehen. Mit diesen Tags können ETS entsprechenden Teams Rechnung.

Er erstellt die folgenden [Ressourcen-Manager-Richtlinien](resource-manager-policy.md): 

| Feld | Effekt | Beschreibung |
| ----- | ------ | ----------- |
| Speicherort | Überwachen | Überwachen der Erstellung von Ressourcen in jeder region |
| Typ | Verweigern | Erstellung der G-Serie virtuelle Computer verweigern |
| Tags | Verweigern | Anwendung Besitzer Tag erforderlich |
| Tags | Verweigern | Kosten Center-Tag erforderlich |
| Tags | Anhängen | Alle Ressourcen Tagname **Geschäftseinheit** und Tagwert **ETS** hinzufügen |


### <a name="resource-tags"></a>Resource-tags

Dave versteht, muss bestimmte Informationen auf die Kostenstelle für die Implementierung BitBucket identifizieren. Dave möchte außerdem wissen, alle Ressourcen, die ETS besitzt. 

Er fügt die folgenden [Tags](resource-group-using-tags.md) Ressourcengruppen und Ressourcen. 
 
| Tagname | Markenwert |
| -------- | --------- |
| ApplicationOwner | Der Name der Person, die diese Anwendung verwaltet. |
| CostCenter | Die Kostenstelle der Gruppe Azure Verbrauch bezahlen. |
| Geschäftseinheit | **ETS** (das Abonnement zugeordneten Unternehmenseinheit) |

### <a name="core-network"></a>Kernnetzwerk

Contoso ETS Information Sicherheits- und Risiko-Managementteam überprüft Dave vorgeschlagenen Plan Anwendung in Azure verschoben. Sie möchten sicherstellen, dass die Anwendung nicht mit dem Internet verbunden ist.  Dave hat auch Entwickler apps, die zukünftig in Azure verschoben werden. Diese apps müssen öffentliche Schnittstellen.  Diese Anforderung erfüllt, bietet er interne und externe virtuelle Netzwerke und eine Netzwerk-Sicherheitsgruppe Zugriff einschränken.

Er erstellt die folgenden Ressourcen:

| Ressourcentyp | Name | Beschreibung |
| ------------- | ---- | ----------- |
| Virtuelles Netzwerk | vnInternal | Mit BitBucket Anwendung verwendet und über ExpressRoute Contosos-Unternehmensnetzwerk verbunden ist.  Ein Subnetz (SbBitBucket) stellt einen bestimmten IP-Adressbereich. |
| Virtuelles Netzwerk | vnExternal | Für zukünftige Applikationen, die öffentliche Endpunkte verfügbar. |
| Netzwerk-Sicherheitsgruppe | nsgBitBucket | Sichergestellt, dass die Angriffsfläche erfolgen Verbindungen nur an Port 443 für das Subnetz lebt (SbBitBucket) durch die Anwendung minimiert wird. |

### <a name="resource-locks"></a>Ressourcensperren 

Dave erkennt, dass die Konnektivität von Contoso Unternehmensnetzwerk internes virtuelles Netzwerk versehentliches Löschen oder fehlerhaften Skripts geschützt werden muss. 

Er erstellt die folgenden [Ressourcen Sperren](resource-group-lock-resources.md):

| Sperrentyp | Ressource | Beschreibung |
| --------- | -------- | ----------- |
| **CanNotDelete** | vnInternal | Verhindert, dass Benutzer die virtuelle Netzwerke oder Subnetze löschen, jedoch verhindert nicht das Hinzufügen neuer Subnetze. |

### <a name="azure-automation"></a>Azure-Automatisierung 

Dave hat nichts für diese Anwendung zu automatisieren. Obwohl er ein Azure Automation-Konto erstellt, wird nicht er anfänglich verwenden. 

### <a name="azure-security-center"></a>Azure Security Center 

Contoso IT Servicemanagement muss schnell identifizieren und Behandeln von Gefahren. Sie möchten wissen, welche Probleme auftreten.  

Um diese Anforderung zu erfüllen, Dave ermöglicht [Azure Security Center](./security-center/security-center-intro.md), sowie die Security Manager-Rolle. 

## <a name="scenario-2-customer-facing-app"></a>Szenario 2: kundenorientierte app

Führende Unternehmen im Bereich Supply Chain identifizierten Möglichkeiten mit Contoso Kunden mit einer Treuekarte erhöhen. Alice Team muss diese Anwendung und entscheidet, dass Azure ihre Fähigkeit, Business Bedarf erhöht. Alice arbeitet mit Dave ETS zwei Abonnements für die Entwicklung und den Betrieb dieser Anwendung konfigurieren.

### <a name="azure-subscriptions"></a>Azure-Abonnements 

Dave Azure Enterprise Portal anmelden und Supply Chain-Abteilung bereits sieht.  Wie dieses Projekt die erste Entwicklungsprojekt für das Supply Chain Team in Azure erkennt Dave jedoch die Notwendigkeit für ein neues Konto für Alice-Entwicklungsteam.  Er erstellt das Konto "R & D" für das Team und weist Access an Alice. Alice Portal Konto anmeldet und zwei Abonnements erstellt: eines, das den Entwicklungsservern und eines, das die Produktionsservern.  Die folgenden Abonnements erstellen folgt sie zuvor festgelegten Benennungsstandards: 

| Abonnement verwenden | Name |
| ---------------- | ---- |
| Entwicklung | SupplyChain ResearchDevelopment LoyaltyCard Entwicklung |
| Produktion | SupplyChain Operationen LoyaltyCard Produktion |

### <a name="policies"></a>Richtlinien

Dave Alice erläutert die Anwendung und feststellen, dass diese Anwendung nur Kunden in Nordamerika dient.  Alice und ihre Anwendung Service-Umgebung und Azure SQL Azure zum Erstellen der Anwendung verwenden möchten. Sie müssen während der Entwicklung erstellen.  Alice möchte sicherstellen, dass ihre Entwickler zu untersuchen von Problemen ohne ETS benötigten Ressourcen haben. 

**Entwicklung Abonnements**erstellen sie die folgende Richtlinie:

| Feld | Effekt | Beschreibung |
| ----- | ------ | ----------- |
| Speicherort | Überwachen | Überwachen der Erstellung von Ressourcen in jeder Region. |

Sie beschränken nicht den Typ der Sku, Entwicklung erstellt werden können, und sie benötigen keine Tags für Ressourcen oder Ressourcengruppen.

**Produktion-Abonnement**erstellen sie die folgenden Richtlinien:

| Feld | Effekt | Beschreibung |
| ----- | ------ | ----------- |
| Speicherort | Verweigern | Die Erstellung von Ressourcen außerhalb der USA-Rechenzentren zu verweigern. |
| Tags | Verweigern | Anwendung Besitzer Tag erforderlich |
| Tags | Verweigern | Abteilung Tag erforderlich. | 
| Tags | Anhängen | Jede Ressourcengruppe, die Produktion fügen Sie Tag hinzu. |

Sie beschränken nicht den Typ der Sku, die Produktion erstellt werden können.

### <a name="resource-tags"></a>Resource-tags 

Dave versteht, muss bestimmte Informationen die richtigen Geschäftsgruppen für Abrechnung und Besitz. Er definiert Resource-Tags für Ressourcengruppen und Ressourcen. 
 
Tagname | Markenwert |
| -------- | --------- |
| ApplicationOwner | Der Name der Person, die diese Anwendung verwaltet. |
| Abteilung | Die Kostenstelle der Gruppe Azure Verbrauch bezahlen. |
| EnvironmentType | **Produktion** (Obwohl **das Abonnement im Namen umfasst,** können Sie diesen Tag leicht auf Ressourcen im Portal oder auf der Rechnung.) |

### <a name="core-networks"></a>Core-Netzwerke

Contoso ETS Information Sicherheits- und Risiko-Managementteam überprüft Dave vorgeschlagenen Plan Anwendung in Azure verschoben. Sie möchten sicherstellen, dass die Treuekarte Anwendung ordnungsgemäß isoliert und in ein DMZ-Netzwerk geschützt.  Um diese Anforderung zu erfüllen, Dave und erstellen Alice ein externes virtuelles Netzwerk und ein Netzwerk-Sicherheitsgruppe Treuekarte Anwendung Contoso Netzwerk isolieren.  

**Entwicklung Abonnement**erstellen sie:

| Ressourcentyp | Name | Beschreibung |
| ------------- | ---- | ----------- |
| Virtuelles Netzwerk | vnInternal | Dient die Treuekarte Contoso Development Environment und über ExpressRoute Contosos-Unternehmensnetzwerk verbunden ist. |

**Produktion-Abonnement**erstellen sie:

| Ressourcentyp | Name | Beschreibung |
| ------------- | ---- | ----------- |
| Virtuelles Netzwerk | vnExternal | Die Treuekarte Anwendung gehostet und nicht direkt mit Contoso ExpressRoute verbunden. Code wird direkt an die PaaS-Dienste über ihren Quellcode-System abgelegt. |
| Netzwerk-Sicherheitsgruppe | nsgBitBucket | Stellt sicher, dass die Angriffsfläche erfolgen nur Kommunikation in gebunden auf TCP 443 minimiert wird.  Contoso untersucht Web Application Firewall für zusätzlichen Schutz mit. |  

### <a name="resource-locks"></a>Ressourcensperren 

Dave Alice verleihen und Ressourcensperren einiger der wichtigsten Ressourcen in der Umgebung verhindert versehentliches Löschen während einer fehlerhaften Code Push hinzufügen möchten. 

Sie erstellen die folgenden sperren:

| Sperrentyp | Ressource | Beschreibung |
| --------- | -------- | ----------- |
| **CanNotDelete** | vnExternal | Um zu verhindern, dass Personen löschen virtuelles Netzwerk oder Subnetze. Die Sperre verhindert nicht das Hinzufügen neuer Subnetze. |

### <a name="azure-automation"></a>Azure-Automatisierung 

Alice und ihrem Entwicklungsteam haben umfassende Runbooks Aufgaben für diese Anwendung. Die Runbooks ermöglichen Hinzufügen/Löschen von Knoten und andere DevOps Aufgaben. 

Aktivieren sie [Automatisierung](./automation/automation-intro.md), um diese Runbooks verwenden.

### <a name="azure-security-center"></a>Azure Security Center 

Contoso IT Servicemanagement muss schnell identifizieren und Behandeln von Gefahren. Sie möchten wissen, welche Probleme auftreten.  

Um diese Anforderung zu erfüllen, kann Dave Azure Security Center. Er gewährleistet, dass Azure Security Center die Ressourcen überwacht und ermöglicht den Zugriff auf die DevOps und Sicherheitsteams. 

## <a name="next-steps"></a>Nächste Schritte

- Zum Erstellen von Ressourcen-Manager-Vorlagen finden Sie unter [Best Practices für Azure-Ressourcen-Manager Vorlagen erstellen](resource-manager-template-best-practices.md).

*[Karl Kuhnhausen](https://github.com/karlkuhnhausen) Beitrag zu diesem Thema.*
