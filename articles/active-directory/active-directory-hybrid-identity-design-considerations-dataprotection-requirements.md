<properties
    pageTitle="Azure Active Directory hybride Identität Design - bestimmen Datenschutzes | Microsoft Azure"
    description="Planung der hybrididentitätslösung Identifizieren der Datenschutzvorschriften für Ihr Unternehmen und welche Optionen diesen Anforderungen verfügbar sind."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

#<a name="plan-for-enhancing-data-security-through-strong-identity-solution"></a>Plan zur Verbesserung der Sicherheit durch starke identitätslösung

Der erste Schritt zum Schutz der Daten ist identifizieren, die auf die Daten zugreifen können und als Teil dieses Prozesses Sie eine Identität müssen Lösung, die mit der Authentifizierung und Autorisierung Funktionen integriert. Authentifizierung und Autorisierung werden oft miteinander verwechselt und ihre Rollen missverstanden. In der Praxis sind ganz anders, wie in der folgenden Abbildung dargestellt:

![](./media/hybrid-id-design-considerations/mobile-devicemgt-lifecycle.png)
 
**Mobile Device Management Lebenszyklusphasen**

Bei der Planung der hybride Identität müssen Sie verstehen die Datenschutzvorschriften für Ihr Unternehmen und welche Optionen diesen Anforderungen verfügbar sind.
 
>[AZURE.NOTE]
Nach Abschluss der Planung für die Sicherheit überprüfen Sie um sicherzustellen, dass Ihre Auswahl Anforderungen mehrstufige Authentifizierung nicht Entscheidungen betroffenen in diesem Abschnitt erstellte [mehrstufige Authentifizierung ermitteln](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md) .

## <a name="determine-data-protection-requirements"></a>Bestimmen des Datenschutzes
Im Zeitalter der Mobilität, die meisten Unternehmen haben ein gemeinsames Ziel: ihre Benutzer auf ihren mobilen Geräten lokal oder Remote überall produktiv um Produktivität. Ziel sein könnte, werden Unternehmen, die eine solche Verpflichtung auch Bedenken bezüglich der Risiken, die vermindert werden müssen, um zu schützen Sie Unternehmensdaten und Datenschutz. Jedes Unternehmen kann unterschiedliche Vorschriften haben; verschiedene Konformitätsregeln, die variiert entsprechend der, die Branche des Unternehmens handelt führen zu anderen Designentscheidungen. 

Es gibt jedoch einige Sicherheitsaspekte, die untersucht und überprüft, unabhängig von der Branche, die im nächsten Abschnitt erläutert.

## <a name="data-protection-paths"></a>Schutz Datenpfade

![](./media/hybrid-id-design-considerations/data-protection-paths.png)
 
**Schutz Datenpfade**

Im obigen Diagramm werden Identität Komponente den ersten überprüft werden, bevor Daten zugegriffen wird. Diese Daten kann in anderen Mitgliedstaaten die Zeit jedoch zugegriffen wurde. Jede Zahl in diesem Diagramm stellt einen Pfad, in dem Daten irgendwann Zeitpunkt befinden. Diese Zahlen werden im folgenden erläutert:

1. Datenschutz auf.
2. Datenschutz bei der Übertragung.
3. Datenschutz bei Rest lokal.
4. Datenschutz bei Rest in der Cloud.

Obwohl die technischen wird steuert aktivieren IT zum Schutz der Daten selbst auf jede dieser Phasen nicht direkt bieten die hybrididentitätslösung muss die hybrididentitätslösung Nutzung von lokalen und Cloud kann Identity Management-Ressourcen zur Identifizierung des Benutzers vor dem Zugriff auf die Daten gewähren. Beim Planen Ihrer hybrididentitätslösung sicherstellen, dass nach den Erfordernissen Ihrer Organisation die folgenden Fragen beantwortet werden:

## <a name="data-protection-at-rest"></a>Datenschutz im Ruhezustand
Unabhängig davon, wo die Daten statisch (Gerät, Cloud oder lokal) ist es wichtig, ein Assessment zur Organisation in dieser Hinsicht verstehen ausführen. Für diesen Bereich sicher, dass die folgenden Fragen:

- Muss Ihr Unternehmen Daten schützen?
 - Wenn Ja, ist die hybrididentitätslösung Ihre aktuelle lokale Infrastruktur integrieren?
 - Wenn Ja, kann die hybrididentitätslösung integrieren Ihre Arbeitslasten in der Cloud befindet?
- Kann das Cloud Identitätsmanagement die Anmeldeinformationen des Benutzers und andere Daten in der Cloud zu schützen?

## <a name="data-protection-in-transit"></a>Datenschutz bei der Übertragung
Daten zwischen dem Gerät und das Rechenzentrum oder zwischen dem Gerät und der Cloud müssen geschützt werden. Allerdings wird in Transit bedeutet nicht unbedingt ein Kommunikationsprozess mit einer Komponente außerhalb der Cloud-Dienst; Er verschiebt intern auch solche zwischen zwei virtuelle Netzwerke. Für diesen Bereich sicher, dass die folgenden Fragen:

- Muss Ihr Unternehmen Daten während der Übertragung zu schützen?
 - Wenn Ja, kann die hybrididentitätslösung Integration mit sicheren Steuerelementen wie SSL/TLS?
- Hält die Cloud Identitätsmanagement Verkehr und innerhalb der Verzeichnisspeicher (in und Datencentern) signiert?


## <a name="compliance"></a>Compliance
Vorschriften, Gesetzen und behördlichen Auflagen variieren je nach der Branche, die Ihr Unternehmen gehört. Hohe geregelten Unternehmen müssen Identitätsmanagement Probleme im Zusammenhang mit Compliance. Vorschriften wie Sarbanes-Oxley (SOX), Health Insurance Portability Accountability Act (HIPAA), der Gramm-Leach-Bliley Act (GLBA) und der Payment Card Industry Data Security Standard (PCI DSS) sind über Identitäts- und sehr streng. Hybrididentitätslösung, die Ihr Unternehmen übernehmen müssen die wichtigsten Funktionen, die an eine oder mehrere dieser Vorschriften erfüllen. Für diesen Bereich sicher, dass die folgenden Fragen:

- Entspricht die hybrididentitätslösung Vorschriften für Ihr Unternehmen?
- Aufgebaut die hybrididentitätslösung Funktionen, die Ihr Unternehmen kompatible Auflagen können? 
 
>[AZURE.NOTE]
Stellen Sie sicher zu Notizen jeder Antwort die Gründe für die Antwort. [Datenschutzstrategie definieren](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) gehen über die verfügbaren Optionen und die vor-und Nachteile jeder Option.  Durch die Beantwortung dieser Fragen wählen Sie welche Option am besten Ihr Unternehmen passt benötigt.

## <a name="next-steps"></a>Nächste Schritte
 [Content Management ermitteln](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)


## <a name="see-also"></a>Siehe auch
[Design Considerations (Übersicht)](active-directory-hybrid-identity-design-considerations-overview.md)
