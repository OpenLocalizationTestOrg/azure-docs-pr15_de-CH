<properties
   pageTitle="Best Practices für Softwareupdates auf Microsoft Azure IaaS | Microsoft Azure"
   description="Artikel enthält eine Sammlung von best Practices für Softwareupdates in einer Umgebung mit Microsoft Azure IaaS.  Richtet sich an IT-Experten und Sicherheitsexperten, die mit Änderungskontrolle Software Update und Asset Management täglich, einschließlich für Sicherheit und Compliance ihrer Organisation verantwortlich."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="best-practices-for-software-updates-on-microsoft-azure-iaas"></a>Best Practices für Softwareupdates auf Microsoft Azure IaaS

Vor jeder Art von Diskussion best Practices für eine Umgebung Azure [IaaS](https://azure.microsoft.com/overview/what-is-iaas/) , unbedingt verstehen Szenarien haben, die Sie Verwalten von Softwareupdates und Aufgaben. Das Diagramm sollte diese Grenzen Ihnen helfen:

![Cloud-Modelle und Verantwortung](./media/azure-security-best-practices-software-updates-iaas/sec-cloudstack-new.png)

Die linke Spalte zeigt sieben Aufgaben (in den folgenden Abschnitten definiert), dass Organisationen sollten, die zur Sicherheit und Privatsphäre einer Umgebung.
 
Klassifizierung, Verantwortlichkeit und Client und Endpunkt Schutz sind die Aufgaben, die nur in der Domäne des Kunden und physischen Host und Netzwerk-Aufgaben werden in der Cloud-Dienstanbieter PaaS und SaaS-Modelle. 

Die verbleibenden Aufgaben zwischen Kunden und eine cloud-Dienstanbieter. Aufgaben müssen der CSP und Kunden verwalten verantwortlich, einschließlich der Überwachung ihrer Domänen. Beispielsweise sollten Sie Identität & zugreifen Sie Management Azure Active Directory Services; Konfiguration von Diensten wie mehrstufige Authentifizierung obliegt dem Kunden, aber effektive Funktionen ist Microsoft Azure verantwortlich.

> [AZURE.NOTE] Lesen Sie weitere Informationen über freigegebene Aufgaben in der Cloud [Gemeinsame Verantwortung für Cloud Computing](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91/file/153019/1/Shared%20responsibilities%20for%20cloud%20computing.pdf) 

Diese Grundsätze gelten im Hybrid-Szenario, Ihr Unternehmen Azure IaaS VMs, die Kommunikation mit lokalen Ressourcen verwendet wie in der folgenden Abbildung dargestellt.

![Normale Hybrid-Szenario mit Microsoft Azure](./media/azure-security-best-practices-software-updates-iaas/sec-azconnectonpre.png)

## <a name="initial-assessment"></a>Anfängliche Bewertung

Selbst wenn Ihr Unternehmen bereits ein Verwaltungssystem verwendet und Sie bereits Aktualisierungsrichtlinien haben, ist es häufig vorherige Richtlinie Assessments und aktualisieren sie Ihre aktuellen Bedürfnisse wichtig. Dies bedeutet, dass mit den aktuellen Zustand der Ressourcen in Ihrem Unternehmen benötigen. Um diesen Zustand zu erreichen, müssen Sie wissen:

-   Die physische und virtuelle Computer in Ihrem Unternehmen.

-   Betriebssysteme und Versionen, die auf diesen physischen und virtuellen Computern.

-   Auf jedem Computer (Service Pack-Versionen, Software-Updates und sonstige Modifikationen) installierten Softwareupdates.

-   Die Funktion führt jeder Computer in Ihrem Unternehmen.

-   Applikationen und Programme auf jedem Computer.

-   Besitz- und Kontaktinformationen Informationen für jeden Computer.

-   Die Ressourcen vorhanden in Ihrer Umgebung und deren relativer Wert bestimmen, welche Bereiche die Aufmerksamkeit und Schutz benötigen.

-   Bekannte Sicherheitsprobleme und Prozesse hat Ihr Unternehmen neue Sicherheitsprobleme identifiziert oder Änderungen der Sicherheitsstufe.

-   Gegenmaßnahmen, die bereitgestellt wurden, um Ihre Umgebung zu sichern.

Diese Informationen sollten regelmäßig aktualisieren und Beteiligten Ihre Software Update-Prozess verfügbar wird.

## <a name="establish-a-baseline"></a>Einrichten einer Basislinie

Ein wichtiger Bestandteil der Software Update-Prozess ist standard Erstinstallationen Betriebssystemversionen, Programme und Hardware für Computer in Ihrem Unternehmen erstellen; Diese sind Baselines bezeichnet. Ein Basisplan ist die Konfiguration eines Produkts oder Systems zu einem bestimmten Zeitpunkt. Eine Anwendung oder ein Betriebssystem Baseline bietet beispielsweise die Möglichkeit, einen Computer oder Dienst auf einen bestimmten Zustand neu erstellen.

Baselines Grundlage für potenzielle Probleme erkennen und beheben und Software Update-Prozess nach Reduzierung der Softwareupdates, die Sie in Ihrem Unternehmen bereitstellen müssen und Ihre Fähigkeit zur Überwachung vereinfachen.

Nach dem Ausführen der ersten Prüfung des Unternehmens, verwenden Sie die Informationen, die aus der Prüfung eine operative Baseline für IT-Komponenten in Ihrem Produktionsumfeld definieren ermittelt wird. Eine Anzahl von Baselines möglicherweise erforderlich, die verschiedenen Arten von Hardware und Software in die Produktion bereitgestellt.

Einige Server benötigt beispielsweise ein Softwareupdate verhindern, hängen bei Herunterfahren beim Ausführen von Windows Server 2012. Eine Baseline für diese Server sollte dieses Update enthalten.

In großen Organisationen ist es oft hilfreich zu Kategorien unterteilen die Computer in Ihrem Unternehmen jede Kategorie auf eine standard-Baseline die gleichen Versionen von Software und Software-Updates. Anschließend können diese Kategorien einem Softwareverteilungspunkt Update priorisieren.

## <a name="subscribe-to-the-appropriate-software-update-notification-services"></a>Der entsprechenden Software Update Notification Services abonnieren

Nach der Durchführung einer ersten Prüfung der Software verwendet in Ihrem Unternehmen ermitteln Sie die beste Methode für Benachrichtigungen über neue Softwareupdates für jedes Produkt und die Version. Je nach Softwareprodukt möglicherweise die beste Methode e-Mail-Benachrichtigungen, Websites oder Computer Publikationen.

Beispielsweise Microsoft Security Response Center (MSRC) reagiert auf alle sicherheitsbezogenen Probleme zu Microsoft-Produkten und bietet Microsoft Security Bulletin Service, kostenlose e-Mail-Benachrichtigung neu identifizierten Sicherheitsrisiken und Softwareupdates, die veröffentlicht werden, um diese Schwachstellen zu beheben. Sie können diesen Dienst unter http://www.microsoft.com/technet/security/bulletin/notify.mspx abonnieren.

## <a name="software-update-considerations"></a>Aspekte der Aktualisierung

Nach der Durchführung einer ersten Prüfung der Software verwendet in Ihrem Unternehmen ermitteln Sie die Erfordernissen der Softwareupdate-Verwaltungssystem, Einrichten der Softwareupdate-Verwaltungssystem abhängig, die Sie verwenden. WSUS [Best Practices mit Windows Server Update Services](https://technet.microsoft.com/library/Cc708536)lesen Lesen für System Center [Softwareupdates in Configuration Manager planen](https://technet.microsoft.com/library/gg712696).

Es gibt jedoch einige allgemeine Informationen und best Practices, die Sie unabhängig von der Lösung, die Sie verwenden, wie in den Abschnitten anwenden können, die folgt.

### <a name="setting-up-the-environment"></a>Einrichten der Umgebung

Berücksichtigen Sie die folgenden Methoden, wenn setup-Software-Umgebung aktualisieren:

-   **Erstellen Produktion Software Update Sammlungen anhand stabil**: im Allgemeinen stabile Kriterien für Ihre Software Update Inventory und Verteilung erstellen soll alle Phasen des Software Update-Prozesses vereinfachen. Stabilen Kriterien zählen Version des installierten Betriebssystems und Service Packs, Rolle oder Organisation.

-   **Erstellen Präproduktion Sammlungen, die Referenzcomputer enthalten**: Pre-Production-Sammlung gehören repräsentative Konfigurationen die Betriebssystemversionen Zeile Business Software und andere in Ihrem Unternehmen ausgeführt.

Berücksichtigen Sie auch, Software Updateserver befinden, werden in Azure IaaS-Infrastruktur in der Cloud werden oder lokal werden. Dies ist eine wichtige Entscheidung, da Sie die Menge des Datenverkehrs zwischen lokalen Ressourcen und Azure-Infrastruktur auswerten müssen. [Verbinden einer lokalen Netzwerk mit einem virtuellen Microsoft Azure-Netzwerk](https://technet.microsoft.com/library/Dn786406.aspx) weitere lokale Infrastruktur Azure Herstellen einer Verbindung zu lesen.

Designoptionen, die bestimmt, wo der Update-Server befinden hängt auch Ihre aktuelle Infrastruktur und das Software Update-System, das Sie derzeit verwenden. WSUS finden Sie unter [Bereitstellen von Windows Server Update Services in Ihrer Organisation](https://technet.microsoft.com/library/hh852340.aspx) und Lesen Sie für System Center Configuration Manager [Websites und Hierarchien in Configuration Manager planen](https://technet.microsoft.com/library/Gg712681.aspx).

### <a name="backup"></a>Sicherung

Regelmäßige Backups sind nicht nur für die Software Update Management-Plattform selbst aber auch für die Server, die aktualisiert werden. Organisationen, die ein [Änderungsverwaltungsverfahren](https://technet.microsoft.com/library/cc543216.aspx) haben benötigen IT-Warum muss der Server aktualisiert die voraussichtliche Ausfallzeiten und möglichen Gründen zu rechtfertigen. Um sicherzustellen, dass Sie einen Rollback Konfiguration bei eine Aktualisierung fehlschlägt, stellen Sie sicher das System regelmäßig sichern.

Einige backup für Azure IaaS Optionen:

-   [Azure IaaS Arbeitslast Schutz mit Data Protection Manager](https://azure.microsoft.com/blog/2014/09/08/azure-iaas-workload-protection-using-data-protection-manager/)

-   [Azure virtuelle Computer sichern](../backup/backup-azure-vms.md)

### <a name="monitoring"></a>Überwachung

Sie führen regelmäßig die Anzahl fehlender überwachen oder Updates oder Updates mit unvollständiger Status für jedes Softwareupdate autorisiert ist installiert. Ebenso erleichtern reporting für Softwareupdates, die noch nicht autorisiert einfacher Bereitstellung Entscheidung.

Berücksichtigen Sie auch die folgenden Aufgaben:

-   Führen Sie eine Prüfung der anwendbaren und installierten Sicherheitsupdates für alle Computer in Ihrem Unternehmen.

-   Autorisieren und Updates auf den entsprechenden Computern bereitstellen.

-   Den Lagerbestand nachverfolgen und Installationsstatus und Status für alle Computer in Ihrem Unternehmen zu aktualisieren.

Zusätzlich zu Allgemeine Hinweise, die in diesem Artikel beschrieben wurden, berücksichtigen Sie auch jedes Produkt das beste Praxis, zum Beispiel: haben Sie eine VM in Azure mit SQL Server sicherstellen, dass Folgendes Software Updates Empfehlung für dieses Produkt.

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie in diesem Artikel beschriebenen Richtlinien unterstützen Sie beim Bestimmen der besten Optionen für Softwareupdates für virtuelle Computer in Azure IaaS. Gibt es viele Ähnlichkeit zwischen Software Update – best Practices in einem herkömmlichen Datencenter im Vergleich zu Azure IaaS, daher wird empfohlen, bewerten Ihre aktuelle Software Update Richtlinien Azure VMs und die relevanten best Practices aus diesem Artikel in der gesamten Aktualisierung der Software.
