<properties 
   pageTitle="Wartungsfreundlichkeit und Rentenkonten Policy guide für Azure Gastbetriebssystem | Microsoft Azure" 
   description="Enthält Informationen über was Microsoft angeht, Gastbetriebssystem Azure Cloud Services verwendet unterstützt werden." 
   services="cloud-services" 
   documentationCenter="na" 
   authors="raiye" 
   manager="timlt" 
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd" 
   ms.date="10/24/2016"
   ms.author="raiye"/>

# <a name="azure-guest-os-supportability-and-retirement-policy"></a>Azure Gastbetriebssystem Wartungsfreundlichkeit und Retirement-Richtlinie
Die Informationen auf dieser Seite bezieht sich auf den Azure Gastbetriebssystem ([Gastbetriebssystem](cloud-services-guestos-update-matrix.md)) für Clouddienste Arbeitskraft und Webrollen (PaaS). Es gilt nicht für virtuelle Computer (IaaS). 

Microsoft hat eine veröffentlichte [Unterstützungsrichtlinie für das Gastbetriebssystem](http://support.microsoft.com/gp/azure-cloud-lifecycle-faq). Die Seite, die Sie gerade lesen, beschreibt heute Umsetzung die Richtlinie.

Die Richtlinie ist 

1. Microsoft unterstützt **zumindest die neuesten zwei Familien des Gastbetriebssystems**. Wenn eine Familie deaktiviert ist, haben Kunden 12 Monaten amtliche Ruhestand Aktualisierung auf eine neuere unterstützten-BS-gastbetriebssystemfamilie.
2. Microsoft unterstützt die **mindestens zwei neuesten unterstützten gastbetriebssystemfamilien**. 
3. Microsoft unterstützt die am **wenigsten die zwei neuesten Azure SDK**. Wenn eine Version des SDK deaktiviert wird, haben Kunden 12 Monaten amtliche Ruhestand auf eine neuere Version aktualisieren. 

Möglicherweise mehr als zwei Familien oder Versionen unterstützt. Offizielle Gastbetriebssystem Supportinformationen erscheint in [Azure Gast Betriebssystemversionen und SDK-Kompatibilitätsmatrix](cloud-services-guestos-update-matrix.md).


## <a name="when-a-guest-os-family-or-version-is-retired"></a>Wenn ein Gastbetriebssystem Produktfamilie oder Version zurückgezogen 


Nach der Veröffentlichung einer neuen offiziellen Version des Betriebssystems Windows Server wird eine neue Gast-BS- **Familie** eingeführt. Einführung eine neue Gastbetriebssystem Familie wird Microsoft die älteste Gast-BS-Familie zurückgezogen. 

Neue Gast-BS- **Versionen** werden eingeführt, über jeden Monat die neuesten MSRC-Updates zu integrieren. Durch regelmäßige monatliche Updates ist eine Gastbetriebssystem Version normalerweise deaktiviert 60 Tage nach der Veröffentlichung. Dadurch wird mindestens zwei Gast-BS-Versionen für jede Familie zur Verfügung. 

### <a name="process-during-a-guest-os-family-retirement"></a>Prozess Ruhestand Familie ein Gastbetriebssystem 


Nachdem die Pensionierung angekündigt, haben Kunden eine 12 Monate "" Übergangszeit vor ältere Familie offiziell vom Dienst entfernt wird. Diese Übergangszeit kann nach dem Ermessen von Microsoft verlängert werden. Updates werden auf [Azure Gast Betriebssystemversionen und SDK-Kompatibilitätsmatrix](cloud-services-guestos-update-matrix.md)gebucht.

Allmählichen Deaktivierungsvorgang beginnt sechs Monate in der Übergangsphase. Während dieser Zeit:

1. Microsoft benachrichtigt Kunden in den Ruhestand. 
2. Die neuere Version von Azure SDK wird nicht zurückgezogene Gast-BS-Familie unterstützen.
3. Bereitstellung neuer und Umschichtung von Clouddiensten zurückgezogenen Familie dürfen

Microsoft wird weiterhin einzuführen, neue Gast-BS-Version mit der neuesten MSRC-Updates bis zum letzten Tag der Übergangszeit "Ablaufdatum" genannt. Zu diesem Zeitpunkt wird die Clouddienste ausgeführt unter der Azure-SLA nicht unterstützt. Microsoft hat nach eigenem Ermessen erzwingen, aktualisieren, löschen oder danach diese Dienste beenden.



### <a name="process-during-a-guest-os-version-retirement"></a>Prozess Ruhestand Gast-Betriebssystem 
Wenn Kunden ihre Gastbetriebssystem automatisch festgelegt, müssen sie mit Gast-BS-Versionen kümmern. Sie werden immer die neueste Gastbetriebssystem verwenden.

Gast-BS-Versionen werden jeden Monat veröffentlicht. Aufgrund der regulären Releases verfügt jede Version eine feste Lebensdauer.

60 Tage nach der Lebensdauer ist eine "*deaktiviert*". "Deaktiviert" bedeutet, dass die Version von klassischen Azure-Portal entfernt wird. Es kann auch nicht aus der Konfigurationsdatei mithilfe festgelegt werden. Vorhandene Installationen werden weiterhin ausgeführt, aber Neuinstallationen und Konfigurationsdateien Updates zu vorhandenen Installationen nicht zulässig. 

Später das Gastbetriebssystem Version "*läuft ab*" und alle Installationen laufen noch Version erzwingen aktualisiert und auf dem Gastbetriebssystem in Zukunft automatisch aktualisiert. Ablauf erfolgt in Batches so der Zeitraum von Deaktivierung Ablaufdatum variieren kann. 

Diese Perioden länger Ermessen von Microsoft Kunden Übergänge vereinfachen erfolgen. Änderungen werden auf [Azure Gast Betriebssystemversionen und SDK-Kompatibilitätsmatrix](cloud-services-guestos-update-matrix.md)mitgeteilt.



### <a name="notifications-during-retirement"></a>Benachrichtigung im Ruhestand 

* **Familie Ruhestand** <br>Microsoft verwendet Blogbeiträge und Azure klassischen Portal Benachrichtigung. Kunden, die noch eine zurückgezogene Gastbetriebssystem Familie werden durch direkte Kommunikation (e-Mail, Portal Nachrichten, Telefonanruf) mit zugewiesenen Administratoren benachrichtigt. Alle auf dieser Seite gebucht und RSS-Feeds am Anfang dieser Seite aufgeführt. 


* **Version Ruhestand** <br>Alle auf dieser Seite gebucht und RSS-Feeds am Anfang dieser Seite einschließlich Version deaktiviert und Ablaufdaten aufgeführt. Services-Administratoren erhalten e-Mails haben Bereitstellung auf eine deaktivierte Gastbetriebssystem Version oder Familie. Das Timing dieser e-Mails kann variieren. Im Allgemeinen sind mindestens einen Monat vor Deaktivierung, obwohl dies keine offizielle SLA. 


## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

**Wie kann Einfluss auf die Migration minimieren?**

Verwenden Sie aktuelle Gast-BS-Familie zum Entwerfen von Cloud-Diensten. 

1. Planen der Migrations zu einer neueren Familie früh. 
2. Temporäre Test Bereitstellung Testen des Cloud-Services auf neue Familie einrichten. 
3. Legen Sie Ihr Gastbetriebssystem Version **automatisch** (OsVersion = * in der [.cscfg](cloud-services-model-and-package.md#cscfg) -Datei), die Migration zu neuen Gastbetriebssystem automatisch ausgeführt wird.

**Was geschieht, wenn meine stärkere Integration in das Betriebssystem müssen?**

Erfordert die Anwendungsarchitektur Web tiefer Abhängigkeit vom Betriebssystem unterstützte Plattform [Startaufgaben](cloud-services-startup-tasks.md) oder weitere Erweiterungsmechanismen bestehen in der Zukunft möglicherweise. Alternativ auch können [Azure Virtual Machines](https://azure.microsoft.com/documentation/scenarios/virtual-machines/) (IaaS-Infrastruktur als Dienst), Sie das zugrunde liegende Betriebssystem beibehalten können.
 
## <a name="next-steps"></a>Nächste Schritte
Lesen Sie die neuesten [Gastbetriebssystem frei](cloud-services-guestos-update-matrix.md).
