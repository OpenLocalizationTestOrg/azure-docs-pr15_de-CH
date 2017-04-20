<properties
 pageTitle="Problembehandlung für Cloud Service-Bereitstellung | Microsoft Azure"
 description="Es gibt einige allgemeine Probleme, die bei der Bereitstellung eines Cloud-Diensts in Azure auftreten können. Dieser Artikel bietet Lösungen für sie."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="09/02/2016"
   ms.author="v-six" />

# <a name="troubleshoot-cloud-service-deployment-problems"></a>Problembehandlung für Cloud Service-Bereitstellung

Wenn Sie Azure ein Cloud-Anwendungspaket bereitstellen, erhalten Sie Informationen zur Bereitstellung von **Eigenschaftenbereich im Azure-Portal** . Sie können die Details in diesem Bereich Problembehandlung mit Cloud-Dienst und können Sie diese Informationen zur Azure beim Öffnen einer neuen Supportanfrage.

Finden Sie im Bereich **Eigenschaften** wie folgt:

* Klicken Sie in Azure-Portal auf die Bereitstellung von Cloud-Dienst, klicken Sie **Alle**und klicken Sie dann auf **Eigenschaften**.
* Klicken Sie im klassischen Azure-Portal auf die Bereitstellung von Cloud-Dienst, klicken Sie auf **DASHBOARD**auf der unteren rechten Ecke der Seite (unter **Blick**). Beachten Sie, dass keine "Eigenschaften" im Bereich sein.

> [AZURE.NOTE] Auf das Symbol in der oberen rechten Ecke des Bereichs, um den Inhalt des Fensterausschnitts **Eigenschaften** in die Zwischenablage zu kopieren.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="problem-i-cannot-access-my-website-but-my-deployment-is-started-and-all-role-instances-are-ready"></a>Problem: Ich kann nicht meine Website zugreifen aber meine Bereitstellung gestartet und können alle Instanzen

Der URL-Link im Portal angezeigt umfasst nicht den Port. Der Standardport für Websites ist 80. Wenn die Anwendung an einem anderen Port Ausführung konfiguriert, müssen Sie die richtige Portnummer URL hinzufügen beim Zugriff auf die Website.

1. Klicken Sie auf die Bereitstellung von Cloud-Dienst in Azure-Portal.
2. Überprüfen Sie im **Eigenschaftenbereich der Azure-Portal** die Ports für die Rolleninstanzen (unter **Eingabe Endpunkte**).
3. Ist der Anschluss nicht 80 fügen Sie den richtigen Port-Wert der URL beim Zugriff auf die Anwendung hinzu. Um einen nicht standardmäßigen Port anzugeben, geben Sie die URL gefolgt von einem Doppelpunkt, gefolgt von der Port-Nummer keine Leerzeichen.

## <a name="problem-my-role-instances-recycled-without-me-doing-anything"></a>Problem: Meine Rolleninstanzen wiederverwendet werden, ohne dass ich etwas

Reparatur Service tritt automatisch Azure Problem Knoten erkennt und daher Rolleninstanzen neue Knoten verschoben. In diesem Fall sehen Sie Ihre Instanzen automatisch wiederverwendet. Um herauszufinden, ob Service Reparatur ist aufgetreten:

1. Klicken Sie auf die Bereitstellung von Cloud-Dienst in Azure-Portal.
2. Überprüfen Sie im **Eigenschaftenbereich Azure-Portal** Informationen und bestimmen Sie, ob Service Reparatur Zeit aufgetreten, die die Rollen recycling beobachtet.

Rollen werden ungefähr einmal pro Monat im Host-Betriebssystem und Gastbetriebssystem Updates recyceln.  
Weitere Informationen finden Sie im Blogbeitrag [Rolle Instanz Neustart durch BS-Upgrades](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx)

## <a name="problem-i-cannot-do-a-vip-swap-and-receive-an-error"></a>Problem: Ich kann VIP vertauscht und Fehlermeldung

VIP-Swap ist nicht zulässig, wenn eine Bereitstellung aktualisiert wird. Bereitstellungsupdates können automatisch auftreten, wenn:

* Ein neues Gastbetriebssystem ist verfügbar und für automatische Updates konfigurieren.
* Service Reparatur auftritt.

Um herauszufinden, ob eine automatische Aktualisierung verhindert eine VIP-Swap ausführen:

1. Klicken Sie auf die Bereitstellung von Cloud-Dienst in Azure-Portal.
2. Suchen Sie im **Eigenschaftenbereich der Azure-Portal** auf den Wert der **Status**. Wenn es **Fertig**ist, möglicherweise Kontrollkästchen **letzten Arbeitsgang** auf einem kürzlich auftrat VIP-Swap.
3. Wiederholen Sie die Schritte 1 und 2 für den Produktionsbetrieb.
4. Wenn eine automatische Aktualisierung durchgeführt wird, warten sie bevor VIP austauschen wollen.

## <a name="problem-a-role-instance-is-looping-between-started-initializing-busy-and-stopped"></a>Problem: Eine Rolleninstanz gestartet, Initialisierung, gebucht und beendet Nachrichtenschleife

Dies deutet auf ein Problem mit der Anwendung Code, Paket oder Konfiguration. In diesem Fall sollte man den Status alle paar Minuten ändern und Azure-Portal kann etwas wie **Recycling**, **beschäftigt**oder **Initialisierung**sagen. Dies bedeutet, dass etwas mit der Anwendung, die die Rolleninstanz ausgeführt halten.

Weitere Informationen zum Beheben dieses Problems finden Sie im Blogbeitrag [Azure PaaS berechnen-Diagnosedaten](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx) und [Probleme Rollen dadurch wiederverwendet](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md).

## <a name="problem-my-application-stopped-working"></a>Problem: Meine Anwendung funktioniert nicht

1. Klicken Sie auf die Instanz der Rolle in Azure-Portal.
2. Beachten Sie im **Eigenschaftenbereich der Azure-Portal** folgende Lösung des Problems:
   * Wenn die Instanz der Rolle zuletzt beendet wurde (Sie können den Wert Überprüfen der **Anzahl abgebrochen**), kann die Bereitstellung aktualisieren. Warten Sie, ob die Instanz der Rolle im eigenen Betrieb wieder aufgenommen.
   * Die Instanz der Rolle **gebucht**überprüfen Sie den Anwendungscode, um festzustellen, ob das [StatusCheck](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.statuscheck) -Ereignis behandelt wird. Möglicherweise müssen hinzufügen oder Korrigieren von Code, der dieses Ereignis verarbeitet.
   * Die Diagnosedaten und Szenarios im Blogbeitrag [Azure PaaS berechnen-Diagnosedaten](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx)durchgehen.

>[AZURE.WARNING] Wenn Sie den Cloud-Dienst wiederverwenden, setzen Sie die Eigenschaften für die Bereitstellung effektiv löschen die Informationen des ursprünglichen Problems.

## <a name="next-steps"></a>Nächste Schritte

Weitere [Artikel zur Fehlerbehebung](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) für Cloud-Dienste anzeigen

Informationen zum Cloud-Dienst Rolle Problemen mit Azure PaaS-Diagnosedaten [Kevin Williamsons blogreihe](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx)angezeigt.
