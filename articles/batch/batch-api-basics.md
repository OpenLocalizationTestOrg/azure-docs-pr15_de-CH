<properties
    pageTitle="Azure Batch Übersicht über die Features für Entwickler | Microsoft Azure"
    description="Lernen Sie die Features Batch und APIs aus Sicht der Entwicklung."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="09/29/2016"
    ms.author="marsma"/>

# <a name="batch-feature-overview-for-developers"></a>Übersicht über die Stapel Features für Entwickler

In diesem Überblick über die Hauptkomponenten des Dienstes Azure Batch besprochen, die hauptsächlichen Funktionen und Ressourcen, mit denen Entwickler Batch erstellen umfangreicher Parallel berechnen Solutions.

Ob eine rechnerische verteilte Anwendung entwickeln oder service gibt direkte [REST API] [ batch_rest_api] aufrufen oder eine [Batch-SDKs](batch-technical-overview.md#batch-development-apis)verwenden, verwenden viele Ressourcen und Funktionen, die in diesem Artikel beschrieben.

> [AZURE.TIP] Eine übergeordnete Einführung in Batch-Dienst finden Sie unter [Grundlagen von Azure Batch](batch-technical-overview.md).

## <a name="batch-service-workflow"></a>Batch-Dienst workflow

Der folgende allgemeine Workflow ist typisch für fast alle Programme und Dienste, die den Batch-Dienst für die Verarbeitung paralleler Arbeitslasten:

1. Die **Dateien** , die zu einer [Azure Storage] [ azure_storage] Konto. Stapel enthält integrierten Unterstützung für den Zugriff auf Azure BLOB-Speicher und Ihre Aufgaben können diese Dateien auf [compute-Knoten](#compute-node) Wenn Aufgaben ausgeführt werden.

2. Hochladen der **Anwendungsdateien** , die Ihre Aufgaben ausgeführt werden. Diese Dateien können Binärdateien oder Skripts und deren abhängigen Dateien und Aufgaben in Ihre Aufträge ausgeführt werden. Aufgaben können diese Dateien aus Ihrem Speicherkonto oder Funktion der [Pakete](#application-packages) des Stapels für Application Management und Bereitstellung.

3. Erstellt einen [Pool](#pool) von Compute-Knoten. Beim Erstellen eines Pools geben Sie die Anzahl von Compute-Knoten für den Pool, ihre Größe und Betriebssystem. Wenn jede Aufgabe in Ihrem Auftrag ausführt, versehen es auf einem der Knoten im Pool ausgeführt werden.

4. Erstellen eines [Auftrags](#job). Ein Auftrag verwaltet eine Auflistung von Aufgaben. Sie ordnen jeder Auftrag an einen bestimmten Pool, wo diese Aufgaben ausgeführt werden.

5. Das Projekt [Vorgänge](#task) hinzufügen. Jeder Task führt die Anwendung oder das Skript, die zum Verarbeiten der Datendateien aus Ihrem Speicherkonto Downloads hochgeladen. Wie jede Aufgabe abgeschlossen ist, können diese Ausgabe in Azure-Speicher hochladen.

6. Überwachung des Projektstatus und taskausgabe von Azure-Speicher abrufen.

Den folgenden Abschnitten werden diese und andere Ressourcen des Stapels, die Ihre verteilte Rechenaufwand ermöglichen.

> [AZURE.NOTE] Sie benötigen ein [Batch-Konto](batch-account-create-portal.md) den Batch-Dienst verwenden. Fast alle Projektmappen verwenden auch eine [Azure Storage] [ azure_storage] Konto für Speicherung und Abruf. Batch derzeit unterstützt nur den **Allgemeine** Speicherung Kontotyp wie in Schritt 5 [erstellen ein Speicherkonto](../storage/storage-create-storage-account.md#create-a-storage-account) in [von Azure-Speicherkonten](../storage/storage-create-storage-account.md)beschrieben.

## <a name="batch-service-resources"></a>Batch-Ressourcen

Die folgenden Ressourcen - Konten compute-Knoten, Pools, Aufträgen und Aufgaben – alle Lösungen erforderlich sind, Batch-Dienst verwenden. Andere zeichnen Auftragszeitpläne und Anwendungspakete, hilfreich, jedoch optional.

- [Konto](#account)
- [Compute-Knoten](#compute-node)
- [Pool](#pool)
- [Auftrag](#job)

  - [Auftragsterminpläne](#scheduled-jobs)

- [Aufgabe](#task)

  - [Aufgabe starten](#start-task)
  - [Manager Projektaufgabe](#job-manager-task)
  - [Aufgaben zur Vorbereitung und Veröffentlichung](#job-preparation-and-release-tasks)
  - [Mehrere Instanzen Aufgabe (MPI)](#multi-instance-tasks)
  - [Verzögert](#task-dependencies)

- [Anwendungspakete](#application-packages)

## <a name="account"></a>Konto

Ein Batch-Konto ist eindeutig bestimmten Entität in der Batch-Dienst. Verarbeitung ist einer Batch-Konto zugeordnet. Wenn Sie mit der Batch-Dienst ausführen, benötigen Sie den Kontonamen und eines seiner Firma. Sie können [ein Azure Batch Konto Azure-Portal erstellen](batch-account-create-portal.md).

## <a name="compute-node"></a>Compute-Knoten

Compute-Knoten ist ein Azure Virtual Machine (VM), die einen Teil der Arbeitslast verarbeiten vorgesehen ist. Die Größe eines Knotens bestimmt die Anzahl der CPU-Kerne, Speicherkapazität und lokale Größe, die dem Knoten zugewiesen wird. Pools mit Windows- oder Linux-Knoten können mithilfe von Azure Cloud Services oder virtuelle Computer Marketplace Bilder. [Pool](#pool) -Abschnitt für Weitere Informationen zu diesen Optionen anzeigen

Knoten können ausführbare oder das Betriebssystem des Knotens unterstützt Skripts ausführen. Dies schließt \*.exe \*cmd, \*shell bat- und Binärdateien zu Windows PowerShell-Skripts und Python-Skripten für Linux.

Alle compute-Knoten im Stapel auch:

- Eine [Ordnerstruktur](#files-and-directories) und [Umgebungsvariablen](#environment-settings-for-tasks) von Aufgaben zur Verfügung.
- **Firewall** -Einstellung, die Zugriffsteuerung konfiguriert.
- [RAS](#connecting-to-compute-nodes) (Remote Desktop Protocol (RDP)) Windows und Linux (Secure Shell (SSH)) Knoten.

## <a name="pool"></a>Pool

Ein Pool ist eine Auflistung von Knoten, die Ihre Anwendung ausgeführt wird. Der Pool kann manuell erstellt werden von Ihnen oder automatisch vom Batch-Dienst beim Festlegen der zu erledigenden Arbeit. Sie können erstellen und verwalten ein Pools, das die Ressourcen der Anwendung erfüllt. Ein Pool kann nur von Batch-Konto verwendet werden, in der es erstellt wurde. Batch-Konto können mehrere Pools.

Azure Batch Pools erstellen auf Core Azure Compute-Plattform. Sie bieten umfangreiche Zuweisung Anwendungsinstallation, Verteilung, Überwachung und flexible Anpassung der Anzahl von Compute-Knoten innerhalb eines Pools ([Skalierung](#scaling-compute-resources)).

Jeder Knoten in einem Pool hinzugefügt wird einen eindeutigen Namen und IP-Adresse zugewiesen. Beim Entfernen eines Knotens aus einem Betriebssystem oder Dateien vorgenommenen Änderungen verloren, und Name und IP-Adresse für die zukünftige Verwendung freigegeben. Ein Knoten einen Pool verlässt, geht ihre Lebensdauer über.

Beim Erstellen eines Pools können Sie die folgenden Attribute:

- Compute Nodes **Betriebssystem** und **version**

    Wenn Sie ein Betriebssystem für die Knoten im Pool auswählen, haben Sie zwei Optionen: **Konfiguration des virtuellen Computers** und **Cloud Dienste**.

    **Konfiguration des virtuellen Computers** bietet Linux- und Windows-Abbilder für compute-Knoten von [Azure VMs Marketplace][vm_marketplace].
    Beim Erstellen eines Pools, das Konfiguration des virtuellen Computers Knoten enthält, müssen Sie nicht nur die Größe der Knoten auch **virtuellen Verweis** und Batch- **Knoten Agent SKU** auf Knoten angeben. Weitere Informationen zum Festlegen dieser Eigenschaften finden Sie unter [Bereitstellung Linux Computeknoten in Azure Batch-Pools](batch-linux-nodes.md).

    **Cloud-Services-Konfiguration** bietet Windows Knoten *nur*berechnen. [Kompatibilitätsmatrix für SDK und Azure Gastbetriebssystem frei](../cloud-services/cloud-services-guestos-update-matrix.md)verfügbaren Betriebssysteme für Cloud-Services-Konfiguration Pools entnehmen. Beim Erstellen eines Pools, das Clouddienste Knoten müssen Sie nur die Knoten und der *Betriebssystemfamilie*angeben. Bei der Erstellung Pools Windows compute-Knoten, am häufigsten Cloud-Dienste verwenden.

    - *BS-Familie* bestimmt auch, welche Versionen von .NET mit dem Betriebssystem installiert werden.
    - Wie mit Worker-Rollen in Cloud-Dienste, eine *Version des Betriebssystems* angeben können (Weitere Informationen zu Worker-Rollen finden Sie im Abschnitt [Weitere Informationen über Cloud-Services](../cloud-services/cloud-services-choose-me.md#tell-me-about-cloud-services) in der [Cloud Services Overview](../cloud-services/cloud-services-choose-me.md)).
    - Als Arbeitskraft Rollen wir empfehlen, dass Sie `*` für die *Version des Betriebssystems* , damit die Knoten automatisch aktualisiert werden und keine erforderlich Arbeit, um neu bieten-Versionen. Der primäre Anwendungsfall für eine bestimmte Betriebssystemversion auswählen soll Anwendungskompatibilität ermöglicht Abwärtskompatibilität Tests ausgeführt werden, ehe die Version aktualisiert werden. Nach der Überprüfung der *Betriebssystemversion* für den Pool aktualisiert werden kann und das neue Betriebssystemabbild kann installiert – alle ausgeführten Vorgänge unterbrochen und Rollback.

- **Größe der Knoten**

    **Cloud Services Configuration** Compute-Knoten Größen sind in [Größen für Cloud-Services](../cloud-services/cloud-services-sizes-specs.md)aufgeführt. Batch unterstützt Clouddienste alle außer `ExtraSmall`.

    **Konfiguration des virtuellen Computers** compute-Knoten angezeigten Größen in [Größen für virtuelle Computer in Azure](../virtual-machines/virtual-machines-linux-sizes.md) (Linux) und [Größen für virtuelle Computer in Azure](../virtual-machines/virtual-machines-windows-sizes.md) (Windows). Batch unterstützt Azure VM-alle außer `STANDARD_A0` und Premium-Speicher (`STANDARD_GS`, `STANDARD_DS`, und `STANDARD_DSV2` Serie).

    Wenn eine Compute-Knoten-Größe sollten Sie die Eigenschaften und der Programme, die Sie auf den Knoten laufen. Aspekte wie ob Multithread-Anwendung handelt und wie viel Speicher verbraucht helfen bestimmen die Knotengröße geeignet und kostengünstige. Es ist üblich, wählen Sie einen Knoten Größe vorausgesetzt, jeweils eine Aufgabe auf einem Knoten ausgeführt wird. Es ist jedoch möglich, mehrere Aufgaben (und daher mehrere Instanzen) [parallel ausgeführt werden](batch-parallel-node-tasks.md) , auf compute-Knoten während der Ausführung des Auftrags. In diesem Fall ist häufig größer Knoten auf die erhöhte Nachfrage parallel Tasks auswählen. Weitere Informationen finden Sie unter [Richtlinien für das Planen von Tasks](#task-scheduling-policy) .

    Alle Knoten in einem Pool haben dieselbe Größe. Wenn ausgeführt Applications unterschiedliche Anforderungen bzw. Ebenen laden wollen wir empfehlen die Verwendung von separaten Pools.

- **Node-Anzahl**

    Dies ist die Anzahl der im Pool bereitzustellende Compute-Knoten. Dies wird als *Ziel* bezeichnet, da in einigen Situationen das Pool die Anzahl Knoten möglicherweise nicht erreicht. Ein Pool kann die gewünschte Anzahl von Knoten nicht erreicht, gelangt [Core Kontingent](batch-quota-limit.md#batch-account-quotas) für die Batch-Konto oder eine automatische Skalierung Formel, die dem Pool anwenden, die die maximale Anzahl von Knoten beschränkt (siehe Abschnitt "Richtlinie Skalierung").

- **Skalierung von Richtlinien**

    Neben einer statischen Anzahl von Knoten, können Sie stattdessen schreiben und eine [Automatische Skalierung Formel](#scaling-compute-resources) an einen Pool. Der Batch-Dienst regelmäßig bewertet die Formel und passt die Anzahl der Knoten im Pool basierend auf verschiedenen Pools, Auftrag und Aufgabenparameter angeben.

- **Richtlinien Planen von Tasks**

    Die Konfigurationsoption [max Aufgaben pro Knoten](batch-parallel-node-tasks.md) bestimmt die maximale Anzahl von Aufgaben, die auf jeder Compute-Knoten innerhalb des Pools parallel ausgeführt werden können.

    Die Konfiguration ist, dass eine Aufgabe auf einem Knoten ausgeführt wird, aber es Szenarien, in denen es vorteilhaft gibt, mehrere Aufgaben gleichzeitig auf einem Knoten ausgeführt. Finden Sie im [Beispielszenario](batch-parallel-node-tasks.md#example-scenario) im Artikel [Aufgaben gleichzeitig Knoten](batch-parallel-node-tasks.md) finden Sie unter Vorteile mehrere Aufgaben pro Knoten.

    Sie können auch einen *Füllungstyp* Stapel verteilt die Aufgaben gleichmäßig über alle Knoten in einem Pool, ob jeder Knoten die maximale Anzahl von Aufgaben vor dem Zuweisen von Aufgaben zu einem anderen Knoten packs bestimmt.

- **Kommunikationsstatus** von Compute-Knoten

    In den meisten Szenarien Aufgaben unabhängig und nicht miteinander kommunizieren müssen. Es gibt jedoch einige Programme in denen Aufgaben wie [MPI Szenarien](batch-mpi.md)kommunizieren müssen.

    Konfigurieren Sie einen Pool zur Kommunikation zwischen den Knoten im It -**Netzwerk-Kommunikation**. Netzwerk-Kommunikation aktiviert, Knoten in Cloud Services Configuration Pools können miteinander Ports 1100 größer und Konfiguration des virtuellen Computers Pools Datenverkehr an allen Ports nicht einschränken.

    Beachten Sie, dass Netzwerk Kommunikation wirkt sich auf die Platzierung der einzelnen Knoten im Cluster auch möglicherweise die maximale Anzahl von Knoten in einem Pool aufgrund Bereitstellung beschränken. Wenn Ihre Anwendung keine Kommunikation zwischen Knoten erforderlich, Batch-Dienst kann eine möglicherweise große Anzahl von Knoten an den Pool aus vielen verschiedenen reservieren und Rechenzentren ermöglicht parallele Verarbeitung erhöht.

- Compute-Knoten **Task starten**

    Optionale *Aufgabe beginnen* führt für jeden Knoten, Knoten Beitritt Pool und jeweils ein Knoten neu gestartet oder versehen. Erste Aufgabe ist besonders nützlich für die Vorbereitung von Compute-Knoten für die Ausführung von Aufgaben wie das Installieren von Applikationen, die Ihre Aufgaben auf den Compute-Knoten ausgeführt.

- **Anwendungspakete**

    Sie können [Pakete](#application-packages) auf den Compute-Knoten im Pool bereitstellen. Anwendungspakete bieten vereinfachte Bereitstellung und Versionskontrolle Anwendungstypen, die Ihre Aufgaben ausgeführt werden. Anwendungspakete für einen Pool angeben werden auf jedem Knoten, die diesem Pool verknüpft installiert und jedes Mal ein Knoten neu gestartet oder versehen. Pakete werden derzeit auf Linux Compute-Knoten nicht unterstützt.

- **Netzwerkkonfiguration**

    Sie können die ID eines Azure [virtual Network (VNet)](../virtual-network/virtual-networks-overview.md) mit des Pools Knoten berechnen erstellt werden soll. Vorschriften für die Angabe einer VNet für der Pool Sie in [einem Pool ein Konto hinzufügen finden] [ vnet] Stapel REST-API-Referenz.

> [AZURE.IMPORTANT] Alle Stapel Konten haben ein **Kontingent** , das die Anzahl der **Kerne** (und folglich Compute-Knoten) in einem Batch-Konto beschränkt. Standardkontingente und Informationen (z. B. die maximale Anzahl der Kerne in Ihrem Konto Batch) [eine Quote](batch-quota-limit.md#increase-a-quota) [Quoten](batch-quota-limit.md)und Grenzwerte für Azure Batch-Dienst gefunden werden. Wenn Sie sich Fragen "Warum wird nicht Pool erreichen mehr als X Knoten?" Dieses Kontingent Core könnte die Ursache sein.

## <a name="job"></a>Auftrag

Ein Auftrag ist eine Sammlung von Aufgaben. Es verwaltet wie Berechnung durch seine Aufgaben auf den Compute-Knoten in einem Pool ausgeführt wird.

- Das Projekt gibt den **Pool** in dem die Arbeit ausgeführt werden. Sie können einen neuen Pool für jeden Auftrag erstellen oder verwenden einen Pool für viele Projekte. Erstellen Sie einen Pool für jedes Projekt, das mit einem Projekt verknüpft ist oder für alle Aufträge, die mit einem Projekt verknüpft sind.

- Sie können eine optionale **Priorität**angeben. Ein Auftrag mit einer höheren Priorität als Aufträge gesendet, die derzeit ausgeführt werden, werden die Aufgaben für den Auftrag mit höherer Priorität in die Warteschlange vor Aufgaben für Einzelvorgänge niedrigerer Priorität eingefügt. Aufgaben in niedrigerer Priorität Aufträge, die bereits ausgeführt werden nicht unterbrochen.

- Job- **Integritätsregeln** können bestimmte Grenzwerte für Ihre Aufträge an:

    **Maximale Wallclock Zeit**lassen, wenn ein Projekt für maximale Wallclock länger, der angegeben wird läuft, der Auftrag und alle Aufgaben beendet werden.

    Batch erkennen und dann wiederholen Fehlgeschlagene Aufgaben. Sie können die **maximale Anzahl der Wiederholungsversuche Aufgabe** als Einschränkung, einschließlich eine Aufgabe *immer* oder *nie* wiederholt. Wiederholen einer Aufgabe bedeutet, dass die Aufgabe Rollback erneut ausgeführt werden.

- Die Clientanwendung können Aufgaben in einem Projekt oder einer [Projektaufgabe Manager](#job-manager-task)angeben. Eine Projektaufgabe Manager enthält Informationen, die die erforderlichen Aufgaben für ein Projekt auf den Compute-Knoten im Pool ausführen mit der Projekt-Manager erstellt werden. Die Projektaufgabe Manager behandelt speziell von Batch - ist in die Warteschlange aufgenommen, sobald der Auftrag erstellt wird und gestartet, schlägt. Ein Projekt-Manager ist *erforderlich* für Aufträge, die vom [Zeitplan](#scheduled-jobs) erstellt werden, ist die einzige Möglichkeit, die Aufgaben definieren, bevor das Projekt instanziiert wird.

- Standardmäßig bleiben Projekte im Zustand Aktiv, wenn alle Vorgänge innerhalb des Projekts abgeschlossen sind. Sie können dieses Verhalten ändern, sodass der Auftrag automatisch beendet, wenn alle Aufgaben des Auftrags abgeschlossen sind. Legen Sie den Auftrag **OnAllTasksComplete** -Eigenschaft ([OnAllTasksComplete] [ net_onalltaskscomplete] in Batch .NET), *Terminatejob* , den Auftrag automatisch beendet, wenn alle Aufgaben im abgeschlossenen Zustand sind.

    Beachten Sie, dass der Batch-Dienst einen Auftrag *ohne* Vorgänge müssen alle Aufgaben abgeschlossen betrachtet. Daher ist diese Option mit einer [Projektaufgabe Manager](#job-manager-task)am häufigsten verwendet. Wenn Sie automatisierte Kündigung Auftrags-Manager verwenden möchten, sollte zunächst legen Sie einen neuen Auftrag **OnAllTasksComplete** -Eigenschaft auf *Noaction*Sie nur nach Hinzufügen von Aufgaben zum Auftrag auf *Terminatejob* festgelegt.

### <a name="job-priority"></a>Priorität

Sie können Aufträge, die im Stapel erstellt eine Priorität zuweisen. Der Batch-Dienst verwendet den Prioritätswert des Auftrags bestimmt die Reihenfolge der Planung von Aufträgen in einem Konto (Dies ist nicht zu verwechseln mit einem [geplanten Auftrag](#scheduled-jobs)). -Werte zwischen-1000 und 1000-1000 die niedrigste Priorität und 1000 die höchste Priorität. Sie können die Priorität eines Auftrags mithilfe [eines Auftrags aktualisieren] [ rest_update_job] Vorgang (Batch REST) oder [CloudJob.Priority] [ net_cloudjob_priority] Eigenschaft (Stapelverarbeitung).

Das gleiche Konto liegen Aufträge mit höherer Priorität Vorrang über niedrigerer Priorität Aufträge planen. Bei höheren Prioritätswert in einem Konto hat keinen Vorrang vor einer anderen Aufgabe mit einem anderen Konto niedrigerer Priorität planen.

Job scheduling in Pools ist unabhängig. Zwischen verschiedenen Pools wird nicht garantiert, dass eine höhere Priorität zuerst Auftrag zugeordnete Pool ist im Leerlauf Knoten. Im gleichen Pool haben Aufträge mit der gleichen Priorität gleich geplant.

### <a name="scheduled-jobs"></a>Geplante Aufträge

[Zeitpläne für Projekt-] [ rest_job_schedules] können Sie wiederkehrende Aufträge innerhalb der Batch-Dienst erstellen. Ein Auftragsterminplan gibt an, wann Aufträge ausgeführt und enthält die Spezifikationen für die Aufträge ausgeführt werden. Sie können die Dauer des Zeitplans – wie lange und Zeitplan - wird und wie oft während dieser Periode Aufträge erstellt werden soll.

## <a name="task"></a>Aufgabe

Eine Aufgabe ist eine Einheit der Berechnung mit einem Projekt verknüpft ist. Es wird ein Knoten ausgeführt. Vorgänge mit einem Knoten ausgeführt werden oder werden zurückgestellt, bis ein Knoten frei wird. Einfach gesagt, führt eine Aufgabe Programme oder Skripts auf Compute-Knoten die Arbeit benötigten ausführen.

Wenn Sie eine Aufgabe erstellen, können Sie:

- Die **Befehlszeile** des Vorgangs. Dies ist die Befehlszeile, die die Anwendung oder das Skript den Compute-Knoten ausgeführt wird.

    Es ist wichtig zu beachten, dass die Befehlszeile nicht tatsächlich unter einem. Daher kann nicht nativ dauert Shell Features wie [Umgebungsvariablen](#environment-settings-for-tasks) Erweiterung (Dies schließt die `PATH`). Um diese Features nutzen zu können, rufen Sie die Shell in der Befehlszeile – z. B. mit `cmd.exe` auf Windows-Knoten oder `/bin/sh` unter Linux:

    `cmd /c MyTaskApplication.exe %MY_ENV_VAR%`

    `/bin/sh -c MyTaskApplication $MY_ENV_VAR`

    Wenn Ihre Aufgaben Ausführen einer Anwendung oder das Skript, die nicht in des Knotens `PATH` oder Umgebungsvariablen verweisen, Shell in der Taskbefehlszeile explizit aufrufen.

- **Ressourcendateien** , die Daten enthalten. Diese Dateien werden vor der Ausführung des Vorgangs Befehlszeile von BLOB-Speicher in einem **allgemeinen** Azure Storage-Konto automatisch zum Knoten kopiert. Weitere Informationen finden Sie in den Abschnitten [Task starten](#start-task) und [Dateien und Verzeichnisse](#files-and-directories).

- Die **Umgebungsvariablen** , die von der Anwendung erforderlich sind. Weitere Informationen finden Sie im Abschnitt [umgebungseinstellungen für Aufgaben](#environment-settings-for-tasks) .

- Die **Nebenbedingungen** , unter dem der Task ausgeführt werden soll. Z. B. die maximale Zeit, die der Task ausgeführt werden darf, die maximale Anzahl, die eine fehlgeschlagene Aufgabe wiederholt werden soll, und die maximale Zeit, die Dateien im Verzeichnis des Vorgangs beibehalten.

- **Pakete** für die Bereitstellung auf den Compute-Knoten auf die Aufgabe ausführen soll. [Anwendungspakete](#application-packages) bieten vereinfachte Bereitstellung und Versionskontrolle Anwendungstypen, die Ihre Aufgaben ausgeführt werden. Task-Pakete sind besonders in freigegebenen Pool Umgebungen, wo andere Aufträge werden in einem Pool ausgeführt und Pool wird nicht gelöscht, wenn ein Auftrag abgeschlossen wurde. Hat Ihre Arbeit weniger Aufgaben als Knoten im Pool, können Aufgabe Anwendungspakete Datenübertragung minimieren, da Bereitstellung der Anwendung auf die Knoten, die Aufgaben ausführen.

Neben Aufgaben definieren so Berechnung auf einem Knoten werden vom Batch-Dienst auch folgenden Aufgaben bereitgestellt:

- [Aufgabe starten](#start-task)
- [Manager Projektaufgabe](#job-manager-task)
- [Aufgaben zur Vorbereitung und Veröffentlichung](#job-preparation-and-release-tasks)
- [Aufgaben mit mehreren Instanzen (MPI)](#multi-instance-tasks)
- [Verzögert](#task-dependencies)

### <a name="start-task"></a>Aufgabe starten

Zuordnen einer **Aufgabe beginnen** mit einem Pool, bereiten Sie das Betriebssystem von Knoten. Beispielsweise können Sie Aktionen wie Installieren von Applikationen, die Ihre Aufgaben ausgeführt und Hintergrundprozesse gestartet. Erste Aufgabe wird jedem Knoten gestartet wird, so im Pool ist - einschließlich der Knoten wird hinzugefügt, Pool und beim Neustart oder verhindert wird.

Ein wesentlicher Vorteil von Start-Aufgabe ist, dass alle Informationen enthalten kann, die erforderlichen Compute-Knoten konfigurieren und installieren die Anwendung für die Ausführung der Aufgabe erforderlich sind. Erhöhen der Anzahl von Knoten in einem Pool also einfach den neuen Ziel Node-Count - Batch bereits Angabe hat neuen Knoten konfigurieren und diese für die Annahme von Aufgaben erforderlichen Informationen.

Wie bei jeder Aufgabe Azure Batch Sie eine Liste der **Dateien** in [Azure Storage]können[azure_storage], zusätzlich über die **Befehlszeile** ausgeführt werden. Stapel kopiert zunächst die Ressourcendateien zum Knoten aus dem Azure-Speicher und führt dann die Befehlszeile. Für einen Pool Start-Aufgabe enthält die Dateiliste normalerweise Aufgabe Anwendung und die zugehörigen Dateien.

Jedoch konnte es auch Daten von allen Aufgaben verwendet werden, die auf den Compute-Knoten ausgeführt werden. Beispielsweise einer Start-Aufgabe-Befehlszeile ausführen konnte ein `robocopy` Vorgang kopieren Anwendungsdateien (die Dateien angegeben und auf dem Knoten heruntergeladen) die Startaufgabe [Arbeitsverzeichnis](#files-and-directories) in den [freigegebenen Ordner](#files-and-directories)und führen Sie dann eine MSI-Datei oder `setup.exe`.

> [AZURE.IMPORTANT] Batch derzeit unterstützt *nur* Speichertyp Konto **Allgemeine** wie in Schritt 5 [erstellen ein Speicherkonto](../storage/storage-create-storage-account.md#create-a-storage-account) in [von Azure-Speicherkonten](../storage/storage-create-storage-account.md)beschrieben. Die Stapelverarbeitungsaufgaben (einschließlich Standardaufgaben, Start Aufgaben Aufgaben zur Vorbereitung und Projektaufgaben Release) geben Ressourcendateien, die *nur* **Allgemeine** Speicherkonten befinden.

Empfiehlt sich in der Regel für den Batch-Dienst warten die Start-Aufgabe abgeschlossen ist, bevor er den Knoten kann Aufgaben zugewiesen werden, aber Sie können dies.

Ausfall eine Start-Aufgabe auf Compute-Knoten Zustand des Knotens wird den Fehler entsprechend aktualisiert und der Knoten wird nicht Aufgaben zugewiesen. Start-Aufgabe kann fehlschlagen, liegt ein Problem mit dem Kopieren der Dateien aus dem Speicher oder von der Befehlszeile ausgeführt Prozess einen Exitcode zurückgibt.

Wenn Sie hinzufügen oder die Start-Aufgabe einem *vorhandenen* Pool aktualisieren müssen die Compute-Knoten für die Start-Aufgabe auf Knoten angewendet werden neu starten.

### <a name="job-manager-task"></a>Manager Projektaufgabe

Sie normalerweise eine **Projektaufgabe Manager** steuern bzw. überwachen Auftragsausführungsergebnisse – beispielsweise zum Erstellen und übermitteln der Aufgaben für ein Projekt, bestimmen zusätzliche Aufgaben ausführen und wann die Arbeit abgeschlossen ist. Eine Projektaufgabe Manager ist jedoch nicht auf diese Aktivitäten. Es ist eine vollständige Aufgabe, die Aktionen ausführen, die für diese Stelle erforderlich sind. Beispielsweise könnte eine Projektaufgabe Manager Downloaden einer Datei, die als Parameter angegeben analysiert den Inhalt der Datei und zusätzliche Aufgaben basierend auf diesen Inhalt senden.

Eine Projektaufgabe Manager wird vor allen anderen Aufgaben gestartet. Es bietet die folgenden Features:

- Es wird automatisch übermittelt als Aufgabe vom Batch-Dienst, wenn der Auftrag erstellt wurde.

- Vor der Aufgaben in einem Projekt ausgeführt werden soll.

- Der zugeordnete Knoten ist der letzte Pool verkleinert wird aus einem Pool entfernt werden.

- Beendigung kann die Beendigung aller Aufgaben im Auftrag verbunden.

- Eine Projektaufgabe Manager erhält die höchste Priorität, wenn neu gestartet werden muss. Wenn ein Knoten im Leerlauf nicht verfügbar ist, kann der Batch-Dienst eines ausgeführten Aufgaben im Pool Platz für die Projektaufgabe Manager ausführen beendet.

- Eine Projektaufgabe Manager in einem Job muss nicht Priorität andere Aufgaben. Für Aufträge werden nur Einzelvorgangsebene Prioritäten beobachtet.

### <a name="job-preparation-and-release-tasks"></a>Aufgaben zur Vorbereitung und Veröffentlichung

Batch bereit vor Auftragsabwicklung Setup Vorbereitungsaufgaben. Release-Aufgaben sind nach Auftrag Wartung oder bereinigen.

- **Projektaufgabe Vorbereitung**: eine Projektaufgabe Vorbereitung auf allen Aufgaben geplante vor alle anderen Projektaufgaben Computeknoten ausgeführt wird. Sie können eine Arbeitsaufgabe zur Vorbereitung von Daten, die alle Vorgänge freigegeben ist, jedoch wird für den Auftrag.
- **Projektaufgabe Version**: nach Abschluss ein Auftrags eine Projektaufgabe Release wird auf jedem Knoten im Pool, die mindestens einen Vorgang ausgeführt. Eine Projektaufgabe Release können Daten löschen, die vom Berichtsvorbereitungstask Auftrag kopiert zu komprimieren und diagnostische Daten, z. B. hochladen.

Beide job Vorbereitung und Version können Sie an einer Befehlszeile ausgeführt, wenn die Aufgabe aufgerufen wird. Sie bieten Funktionen wie Dateidownload, erhöhten Ausführung benutzerdefinierte Variablen, maximale Dauer, Anzahl und Retentionszeit Datei.

Weitere Informationen zu Aufgaben zur Vorbereitung und Veröffentlichung finden Sie unter [Ausführen Vorbereitung und Durchführung Aufgaben in Azure Batch compute-Knoten](batch-job-prep-release.md).

### <a name="multi-instance-task"></a>Aufgabe mit mehreren Instanzen

[Mehrere Instanzen Aufgabe](batch-mpi.md) ist eine Aufgabe, die gleichzeitig auf mehreren Computeknoten ausgeführt. Mit Multi-Instanz können Sie High-Performance computing Szenarien, die eine Gruppe von Compute-Knoten erforderlich, die gemeinsam zugeordnet wurden ist, um eine bestimmte Auslastung (wie Message Passing Interface (MPI)) zu verarbeiten.

Ausführliche Informationen zum Ausführen von Aufträgen MPI im Stapel mithilfe der Stapelverarbeitung .NET Bibliothek checken Sie [verwenden mehrere Instanzen Message Passing Interface (MPI) Anwendung in Azure Batch auszuführenden Aufgaben](batch-mpi.md).

### <a name="task-dependencies"></a>Verzögert

[Verzögert](batch-task-dependencies.md), wie der Name schon sagt, können Sie angeben, dass der Abschluss anderer Aufgaben vor der Ausführung eine Aufgabe abhängig. Dieses Feature bietet Unterstützung für Situationen in denen "downstream" Aufgabe verbraucht die Ausgabe einer Aufgabe "upstream" - oder eine übergeordneten Aufgabe einige Initialisierung ausführt, die von einer untergeordneten Aufgabe erforderlich ist. Um dieses Feature zu verwenden, müssen Sie zuerst verzögert die Stapelverarbeitung aktivieren. Für jede Aufgabe, die von einem anderen abhängt (oder anderem) die Aufgaben Aufgabe abhängig angeben.

Aufgabe Abhängigkeiten können Sie Szenarien wie folgt konfigurieren:

* *TaskB* hängt von *TaskA* (*TaskB* beginnt die Ausführung bis zum Abschluss der *TaskA* ).
* *TaskC* ist *TaskA* und *TaskB*.
* *TaskD* hängt Aufgaben wie Aufgaben *1* bis *10*vor.

Auschecken von [Vorgängern und Nachfolgern in Azure Batch](batch-task-dependencies.md) und [TaskDependencies] [ github_sample_taskdeps] Codebeispiel in [Azure-Batch-Beispiele] [ github_samples] GitHub Repository für weitere ausführlichen Informationen zu dieser Funktion.

## <a name="environment-settings-for-tasks"></a>Umgebungseinstellungen für Aufgaben

Jede Aufgabe ausgeführt, indem der Batch-Dienst hat Zugriff auf Umgebungsvariablen auf Compute-Knoten festgelegt. Hierbei vom Batch-Dienst definierte Umgebungsvariablen ([Dienst definiert][msdn_env_vars]) und benutzerdefinierte Variablen, die Sie für Ihre Aufgaben definieren können. Applikationen und Skripts ausführen Ihrer Aufgaben haben Zugriff auf diese Variablen während der Ausführung.

Auffüllen der *Umgebung* Einstellungseigenschaft für diese Entitäten können Sie benutzerdefinierte Variablen auf Vorgang oder Projekt festlegen. Beispielsweise finden Sie unter [Hinzufügen einer Aufgabe zu einem Auftrag] [ rest_add_task] Vorgang (Batch REST API) oder [CloudTask.EnvironmentSettings] [ net_cloudtask_env] und [CloudJob.CommonEnvironmentSettings] [ net_job_env] Eigenschaften im Batch .NET.

Die Clientanwendung oder ein Dienst erhalten eine Aufgabe Umgebungsvariablen Dienst definiert und benutzerdefinierte [Informationen über eine Aufgabe] mit[ rest_get_task_info] Vorgang (Batch REST) oder [CloudTask.EnvironmentSettings] [ net_cloudtask_env] Eigenschaft (Stapelverarbeitung). Prozesse auf Compute-Knoten können Zugriff auf diese oder andere Umgebungsvariable auf dem Knoten beispielsweise mithilfe der bekanntes `%VARIABLE_NAME%` (Windows) bzw. `$VARIABLE_NAME` (Linux) Syntax.

Finden Sie eine vollständige Liste aller Service definierten Umgebungsvariablen im [Compute-Knoten-Umgebungsvariablen][msdn_env_vars].

## <a name="files-and-directories"></a>Dateien und Verzeichnisse

Jede Aufgabe hat ein *Arbeitsverzeichnis* unter dem NULL oder mehr Dateien und Verzeichnisse erstellt. Dieses Verzeichnis kann zum Speichern des Programms, das die Aufgabe verarbeiteten Daten und die Ausgabe der ausgeführte Verarbeitung ausgeführt wird. Alle Dateien und Verzeichnisse eines Vorgangs Aufgabe Benutzer gehören.

Der Batch-Dienst stellt einen Teil des Dateisystems auf einem Knoten als *Stammverzeichnis*. Aufgaben können Stammverzeichnis zugreifen, indem Sie auf die `AZ_BATCH_NODE_ROOT_DIR` -Umgebungsvariable. Weitere Informationen zur Verwendung von Umgebungsvariablen finden Sie unter [umgebungseinstellungen für Aufgaben](#environment-settings-for-tasks).

Das Stammverzeichnis enthält die folgende Verzeichnisstruktur:

![Compute-Knoten-Verzeichnisstruktur][1]

- **freigegebene**: Dieses Verzeichnis bietet Lese-und Schreibzugriff auf *Alle* Aufgaben, die auf einem Knoten ausgeführt. Jede Aufgabe, die auf dem Knoten kann erstellen, lesen, aktualisieren und Löschen von Dateien in diesem Verzeichnis. Aufgaben können durch Verweisen auf dieses Verzeichnis zugreifen der `AZ_BATCH_NODE_SHARED_DIR` -Umgebungsvariable.

- **Start**: Dieses Verzeichnis wird als Arbeitsverzeichnis von einer Start-Aufgabe verwendet. Alle Dateien, die auf den Knoten vom Start Task heruntergeladen werden werden hier gespeichert. Die Start-Aufgabe kann erstellen, lesen, aktualisieren und Löschen von Dateien in diesem Verzeichnis. Aufgaben können durch Verweisen auf dieses Verzeichnis zugreifen der `AZ_BATCH_NODE_STARTUP_DIR` -Umgebungsvariable.

- **Aufgaben**: ein Verzeichnis erstellt für jeden Vorgang, der auf dem Knoten ausgeführt wird. Erfolgt durch Verweisen auf die `AZ_BATCH_TASK_DIR` -Umgebungsvariable.

    Innerhalb jedes Verzeichnis Aufgabe der Batch-Dienst erstellt ein Arbeitsverzeichnis (`wd`), deren eindeutige Pfad gemäß der `AZ_BATCH_TASK_WORKING_DIR` Umgebungsvariable. Dieses Verzeichnis bietet Lese-/Schreibzugriff auf die Aufgabe. Die Aufgabe kann erstellen, lesen, aktualisieren und Löschen von Dateien in diesem Verzeichnis. Dieses Verzeichnis wird beibehalten, basierend auf der *RetentionTime* -Einschränkung, die für den Vorgang angegeben.

    `stdout.txt`und `stderr.txt`: Diese Dateien werden während der Ausführung der Aufgabe, den Ordner geschrieben.

>[AZURE.IMPORTANT] Wenn ein Knoten aus dem Pool entfernt wird, werden *Alle* Dateien, die auf dem Knoten entfernt.

## <a name="application-packages"></a>Anwendungspakete

[Anwendungspakete](batch-application-packages.md) -Funktion bietet einfaches Management und Bereitstellung von Applikationen auf den Compute-Knoten in Pools. Sie können laden und mehrere Versionen der Anwendung ausführen, indem Sie Ihre Aufgaben, einschließlich Binärdateien und Dateien. Anschließend können Sie eine oder mehrere dieser Anwendung auf den Compute-Knoten im Pool automatisch bereitstellen.

Sie können Pakete Pool und Aufgabe. Wenn Sie Pakete Pool angeben, wird die Anwendung für jeden Knoten im Pool bereitgestellt. Wenn Sie Aufgabenpakete Anwendung angeben, wird die Anwendung nur für geplante Knoten bereitgestellt mindestens den Auftrag Aufgaben vor der Taskbefehlszeile ausgeführt wird.

Batch behandelt die Details mit Azure-Speicher zum Speichern der Anwendungspakete und Ausbringen Computeknoten damit Ihr Code und Management-Overhead vereinfacht werden kann.

Erfahren Sie mehr über die Anwendungsfunktion Paket, checken Sie [Bereitstellung mit Azure Batch-Anwendungspaketen](batch-application-packages.md).

>[AZURE.NOTE] Wenn Sie einem *vorhandenen* Pool Pool Pakete hinzufügen, müssen Sie die Datenverarbeitungsknoten Anwendungspakete Knoten bereitgestellt werden neu starten.

## <a name="pool-and-compute-node-lifetime"></a>Pool und Compute-Knoten-Lebensdauer

Beim Entwerfen Ihrer Azure Batch-Lösung müssen Sie eine Entscheidung darüber, wie und wenn Pools erstellt werden und wie lange compute-Knoten innerhalb dieser Pools verfügbar sind.

Am einen Ende des Spektrums kann Erstellen eines Pools für jeden Auftrag Absenden und Pool löschen, sobald seine Aufgaben Ausführung beenden. Dies maximiert Auslastung, da Knoten nur zugewiesen werden, wenn erforderlich und heruntergefahren, sobald sie inaktiv sind. Das bedeutet, dass der Einzelvorgang warten muss, bis die Knoten zugewiesen werden, ist es wichtig zu beachten, dass Aufgaben zur Ausführung geplant sind, als Knoten sind einzeln erhältlich, die Start-Aufgabe abgeschlossen. Stapel wird *nicht* warten, bis alle Knoten in einem Pool verfügbar sind, bevor der Knoten Aufgaben zuzuweisen. Maximale Nutzung aller verfügbaren Knoten wird sichergestellt.

Am anderen Ende des Spektrums ist sofort Aufträge mit der höchsten Priorität können Sie Erstellen eines Pools voraus und Knoten zur Verfügung stellen, damit Aufträge gesendet werden. In diesem Szenario Aufgaben können sofort, aber Knoten möglicherweise bleiben ungenutzt warten sie zugewiesen werden.

Ein kombinierter Ansatz wird normalerweise für eine Variable jedoch fortlaufend Last verwendet. Einen Pool, der mehrere Aufträge werden jedoch skalierbar ist die Anzahl der Knoten nach oben oder unten entsprechend den Auftrag laden (Siehe im folgenden Abschnitt [Skalierung compute-Ressourcen](#scaling-compute-resources) ). Reaktiv, dabei basierend auf aktuellen oder proaktiv laden vorausgesagt werden kann.

## <a name="scaling-compute-resources"></a>Skalierung von Serverressourcen

Bei der [automatischen Skalierung](batch-automatic-scaling.md)können Sie dynamisch anpassen der Anzahl von Compute-Knoten in einem Pool nach der aktuellen Arbeitslast und Verwendung von Ressourcen des Szenarios Compute Batch-Dienst haben. Dadurch können Sie Niedrigere Gesamtkosten der Ausführung Ihrer Anwendung benötigten Ressourcen verwenden und freigeben, die Sie benötigen.

Sie aktivieren die automatische Skalierung durch eine [Automatische Skalierung Formel](batch-automatic-scaling.md#automatic-scaling-formulas) schreiben und verbinden diese Formel mit. Der Batch-Dienst verwendet die Formel bestimmt die Node-Anzahl im Pool für das Skalieren Intervall (Abstand, die konfiguriert werden können). Sie können die automatischen Skalierung Einstellungen für einen Pool, beim Erstellen oder später in einem Pool Skalierung aktivieren. Sie können auch die Skalierung Einstellungen in einem Pool Skalierung aktiviert aktualisieren.

Beispielsweise muss möglicherweise ein Auftrag senden eine sehr große Anzahl von Aufgaben ausgeführt werden. Sie können Skalierung Formel zum Pool zuweisen, die die Anzahl der Knoten im Pool basiert die aktuelle Anzahl der Aufgaben in der Warteschlange und der Abschluss der Aufgaben im Auftrag passt. Der Batch-Dienst regelmäßig berechnet die Formel und Pool basierend auf Arbeitslast ändert (Knoten für viele Aufgaben in der Warteschlange hinzufügen und entfernen Sie Knoten keine Aufgaben in der Warteschlange oder ausgeführten) und die andere Formel.

Skalierung Formel kann auf folgenden Kriterien basieren:

- **Zeit Metriken** basieren auf Statistiken alle fünf Minuten in die angegebene Anzahl von Stunden.

- **Ressourcenmetriken** basieren auf CPU-Auslastung, Bandbreite Speicherverwendung und Anzahl der Knoten.

- **Aufgabe Metriken** basieren auf Task Status *aktiv* (Warteschlange), *ausgeführt*oder *abgeschlossen*.

Wenn automatische Skalierung die Anzahl der Compute-Knoten in einem Pool sinkt müssen Sie berücksichtigen, wie Aufgaben, die zum Zeitpunkt der Abnahme der Vorgang ausgeführt werden. Dazu bietet Batch *Knoten Freigabe Option* , die in Formeln enthalten. Beispielsweise können Sie angeben, dass ausgeführte Aufgaben sofort beendet, sofort beendet und dann zur Ausführung auf einem anderen Knoten Rollback oder beenden, bevor der Knoten aus dem Pool entfernt wird.

Weitere Informationen zum automatisch Skalieren einer Anwendung finden Sie unter [automatisch skalieren compute-Knoten in einem Pool Azure Batch](batch-automatic-scaling.md).

> [AZURE.TIP] Zur Maximierung der Ressourcenverwendung berechnen Anzahl Ziel der Knoten NULL am Ende eines Auftrags jedoch die Möglichkeit, Aufgaben zu beenden.

## <a name="security-with-certificates"></a>Sicherheit mit Zertifikaten

Normalerweise müssen Zertifikate beim Verschlüsseln oder Entschlüsseln von vertraulichen Informationen für Aufgaben wie der Schlüssel für eine [Azure Storage-Konto]verwendet[azure_storage]. Dafür können Sie Zertifikate für Knoten installieren. Verschlüsselte Kennwörter über Befehlszeilenparameter an Aufgaben übergeben oder in einem Vorgang Ressourcen eingebettet sind und die installierten Zertifikate wird entschlüsselt.

Verwenden Sie das [Zertifikat hinzufügen] [ rest_add_cert] Vorgang (Batch REST) oder [CertificateOperations.CreateCertificate] [ net_create_cert] Methode (Batch .NET) Batch-Konto ein Zertifikat hinzu. Sie können dann das Zertifikat in eine neue oder vorhandene Pool zuordnen. Wenn ein Zertifikat einem Pool zugeordnet ist, installiert der Batch-Dienst das Zertifikat auf jedem Knoten im Pool. Der Batch installiert die entsprechenden Zertifikate beginnt der Knoten, vor dem Starten von Aufgaben (einschließlich Start-Aufgabe und Projektaufgabe Manager).

Wenn Sie einem *vorhandenen* Pool Zertifikate hinzufügen, müssen die Compute-Knoten für die Zertifikate auf Knoten angewendet werden neu starten.

## <a name="error-handling"></a>Fehlerbehandlung

Es müssen sowohl und Fehlerbehandlung in Batch-Lösung möglicherweise.

### <a name="task-failure-handling"></a>Task-Fehlerbehandlung
Taskfehler fallen in die folgenden Kategorien:

- **Planen von Fehlern**

    Wenn die Übertragung der für einen Vorgang angegebenen Dateien aus irgendeinem Grund fehlschlägt, wird "scheduling Error" für den Vorgang festgelegt.

    Planen von Fehler kann auftreten, wenn die Aufgabe Ressourcendateien wurden verschoben, das Speicherkonto ist nicht mehr verfügbar oder ein anderes Problem aufgetreten, die erfolgreich kopiert Dateien auf den Knoten.

- **Anwendungsfehler**

    Über die Befehlszeile die Aufgabe angegebenen Prozess kann auch fehlschlagen. Der Prozess gilt nicht als ein Exitcode vom Prozess zurückgegeben wird, die vom Task ausgeführt wird (siehe *Aufgabe Beendigungscodes* im nächsten Abschnitt).

    Konfigurieren Sie für Anwendungsfehler Batch, um den Vorgang bis zur angegebenen Anzahl automatisch wiederholen.

- **Einschränkung-Fehler**

    Sie können eine Einschränkung festgelegt, die die maximale Dauer für einen Einzelvorgang oder einen Task, der *MaxWallClockTime*angibt. Dies kann beendet "hängt" Aufgaben hilfreich sein.

    Wenn die maximale Zeitdauer überschritten wurde, die Aufgabe als *abgeschlossen*markiert der Exitcode soll `0xC000013A` und das *SchedulingError* -Feld ist als `{ category:"ServerError", code="TaskEnded"}`.

### <a name="debugging-application-failures"></a>Debuggen von Anwendungsfehlern

- `stderr`und`stdout`

    Während der Ausführung kann eine Anwendung diagnostische Ausgabe, mit denen Sie Probleme beheben können. Wie bereits im Abschnitt [Dateien und Verzeichnisse](#files-and-directories)der Batch-Dienst schreibt Standardausgabe und Standardfehlerausgabe `stdout.txt` und `stderr.txt` Dateien im Verzeichnis Aufgabe auf den Compute-Knoten. Azure-Portal oder eine Batch-SDKs können diese Dateien herunterladen. Sie können z. B. abrufen, diese und andere Dateien zur Fehlerbehebung mit [ComputeNode.GetNodeFile] [ net_getfile_node] und [CloudTask.GetNodeFile] [ net_getfile_task] in der Bibliothek Batch .NET.

- **Codes zum Beenden der Aufgabe**

    Wie bereits erwähnt, wird eine Aufgabe als vom Batch-Dienst ausgeführt wird, wenn der Prozess, der die Aufgabe ausgeführt wird, einen Exitcode gibt gekennzeichnet. Wenn eine Aufgabe einen Prozess ausgeführt wird, füllt Batch die Aufgabe beenden Code-Eigenschaft mit *Code des Prozesses zurück*. Es ist wichtig zu beachten, dass eine Aufgabe Exitcode **nicht** anhand der Batch-Dienst festgestellt wird vom Prozess selbst oder das Betriebssystem, auf dem der Prozess ausgeführt.

### <a name="accounting-for-task-failures-or-interruptions"></a>Bilanzierung Taskfehler oder Unterbrechung

Aufgaben gelegentlich fehlschlagen oder unterbrochen werden. Taskanwendung selbst fehlschlagen der Knoten, auf dem der Task ausgeführt wird, möglicherweise neu gestartet oder der Knoten möglicherweise entfernt aus dem Pool in einem Änderungsvorgang Wenn der Pool Freigabe festgelegt ist Knoten sofort entfernen, ohne abgeschlossen. In allen Fällen kann die Aufgabe automatisch Batch zur Ausführung auf einem anderen Knoten hat, weitergeleitet werden.

Es kann auch ein intermittierender Fehler zu einer Aufgabe oder zu lange ausgeführt. Sie können die maximale Ausführungszeit für einen Vorgang festlegen. Überschritten, unterbricht Batch Vorgangs Anwendung.

### <a name="connecting-to-compute-nodes"></a>Herstellen einer Verbindung zum Knoten berechnen

Sie können zusätzliche Fehlerbehebung und das debugging Remote Compute-Knoten Anmelden durchführen. Azure-Portal können Remote Desktop Protocol (RDP) Datei für Windows Knoten herunterladen und Secure Shell (SSH) Verbindungsinformationen für Linux-Knoten. Sie können auch dazu die Batch-APIs – beispielsweise mit [Batch] [ net_rdpfile] oder [Batch Python](batch-linux-nodes.md#connect-to-linux-nodes).

>[AZURE.IMPORTANT] Die Verbindung mit einem Knoten über RDP oder SSH muss einen Benutzer auf dem Knoten erstellen. Dazu können Azure Portal [Hinzufügen eines Benutzerkontos zu einem Knoten] [ rest_create_user] rufen Sie mithilfe der Stapelverarbeitung REST API [ComputeNode.CreateComputeNodeUser] [ net_create_user] -Methode im Stapel .NET oder Aufruf [Add_user] [ py_add_user] Methode im Stapel Python-Modul.

### <a name="troubleshooting-bad-compute-nodes"></a>Problembehandlung bei "schlecht" compute-Knoten

In Situationen, in denen einige Ihrer Aufgaben fehlschlagen, kann die Clientanwendung Batch oder Service Metadaten Fehlgeschlagene Aufgaben einen fehlerhaften Knoten zu untersuchen. Jeder Knoten in einem Pool erhält eine eindeutige ID, und der Knoten, auf dem eine Aufgabe ausgeführt wird, im Task-Metadaten. Nachdem ein Knoten Problem identifiziert haben, nehmen Sie mehrere Aktionen:

- **Starten Sie den Knoten** ([REST][rest_reboot] | [.NET][net_reboot])

    Neustarten des Knotens kann manchmal lose Probleme wie abgestürzt oder fixierten deaktivieren. Beachten Sie, dass das Pool mithilfe einer Start-Aufgabe oder Ihr Projekt verwendet eine Projektaufgabe Vorbereitung ihrer beim Neustart des Knotens Ausführung.

- **Image den Knoten** ([REST][rest_reimage] | [.NET][net_reimage])

    Dies installiert das Betriebssystem auf dem Knoten neu. Wie bei einem Neustart eines Knotens Aufgaben starten und Vorbereitungsaufgaben werden erneut ausführen, nachdem der Knoten versehen wurde.

- **Den Knoten aus dem Pool entfernen** ([REST][rest_remove] | [.NET][net_remove])

    Manchmal ist es notwendig, den Knoten aus dem Pool entfernen.

- **Deaktivieren Sie auf dem Knoten Planen von Tasks** ([REST][rest_offline] | [.NET][net_offline])

    Dies effektiv erhält der Knoten "offline", damit keine weiteren Aufgaben zugewiesen sind, ermöglicht aber Knoten weiterhin ausgeführt und im Pool. Dadurch werden weitere Untersuchung die Ursache der Fehler ohne den fehlgeschlagenen Vorgang Daten verlieren und ohne den Knoten verursacht zusätzliche Taskfehler. Beispielsweise können Sie auf dem Knoten [Remote anmelden](#connecting-to-compute-nodes) , überprüfen Sie den Knoten Ereignisprotokolle oder anderen Problembehandlung Planen von Tasks deaktivieren. Nach Abschluss der Überprüfung Sie können Schalten Sie Knoten wieder online aktivieren Taskplanung ([REST][rest_online] | [.NET][net_online]), oder führen Sie eine der oben beschriebenen Aktionen.

> [AZURE.IMPORTANT] Jede Aktion, die in diesem Abschnitt - beschrieben starten, neu abbilden, entfernen und deaktivieren Task planen – Sie können angeben, wie derzeit auf dem Knoten ausgeführte Aufgaben behandelt werden, wenn die Aktion. Beim Deaktivieren der Taskplanung auf einem Knoten mit Batch .NET-Clientbibliothek Sie können beispielsweise eine [DisableComputeNodeSchedulingOption] [ net_offline_option] Enumerationswert an, ob Aufgaben **Requeue** **Beenden** ausgeführt für Planung auf anderen Knoten oder Aufgaben vor der Aktion (**TaskCompletion**).

## <a name="next-steps"></a>Nächste Schritte

- Beispiel Batch Anwendung in [Azure Batch-Bibliothek für .NET Einstieg](batch-dotnet-get-started.md)schrittweise durchgehen. Zudem ist eine [Python-Version](batch-python-tutorial.md) des Lernprogramms, die eine Arbeitslast Linux Compute-Knoten ausgeführt wird.

- Herunterladen und Erstellen von [Batch-Explorer] [ github_batchexplorer] Beispielprojekt für die Verwendung von Batch-Projektmappen entwickeln. Batch-Explorer können Sie folgende und vieles mehr ausführen:
  - Überwachen und Pools, Aufträgen und Aufgaben in Ihrem Stapel Konto bearbeiten
  - Download `stdout.txt`, `stderr.txt`, und andere Dateien von Knoten
  - Erstellen von Benutzern auf Knoten und RDP-Dateien für Remotebenutzernamen

- Erfahren Sie, wie [Linux Compute-Knoten erstellen](batch-linux-nodes.md).

- Besuchen Sie das [Forum Azure Batch] [ batch_forum] auf MSDN. Das Forum ist ein guter Fragen, ob Sie gerade lernen oder Experte im Stapel.

[1]: ./media/batch-api-basics/node-folder-structure.png

[azure_storage]: https://azure.microsoft.com/services/storage/
[batch_forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurebatch
[cloud_service_sizes]: ../cloud-services/cloud-services-sizes-specs.md
[msmpi]: https://msdn.microsoft.com/library/bb524831.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_sample_taskdeps]:  https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch_net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[msdn_env_vars]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[net_cloudjob_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobmanagertask.aspx
[net_cloudjob_priority]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.priority.aspx
[net_cloudpool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_cloudtask_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.environmentsettings.aspx
[net_create_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.createcertificate.aspx
[net_create_user]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.createcomputenodeuser.aspx
[net_getfile_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getnodefile.aspx
[net_getfile_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.getnodefile.aspx
[net_job_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.commonenvironmentsettings.aspx
[net_multiinstancesettings]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_onalltaskscomplete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.onalltaskscomplete.aspx
[net_rdp]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getrdpfile.aspx
[net_reboot]: https://msdn.microsoft.com/library/azure/mt631495.aspx
[net_reimage]: https://msdn.microsoft.com/library/azure/mt631496.aspx
[net_remove]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.removefrompoolasync.aspx
[net_offline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.disableschedulingasync.aspx
[net_online]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.enableschedulingasync.aspx
[net_offline_option]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.disablecomputenodeschedulingoption.aspx
[net_rdpfile]: https://msdn.microsoft.com/library/azure/Mt272127.aspx
[vnet]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_netconf

[py_add_user]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.ComputeNodeOperations.add_user

[batch_rest_api]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[rest_add_job]: https://msdn.microsoft.com/library/azure/mt282178.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_cert]: https://msdn.microsoft.com/library/azure/dn820169.aspx
[rest_add_task]: https://msdn.microsoft.com/library/azure/dn820105.aspx
[rest_create_user]: https://msdn.microsoft.com/library/azure/dn820137.aspx
[rest_get_task_info]: https://msdn.microsoft.com/library/azure/dn820133.aspx
[rest_job_schedules]: https://msdn.microsoft.com/library/azure/mt282179.aspx
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx
[rest_multiinstancesettings]: https://msdn.microsoft.com/library/azure/dn820105.aspx#multiInstanceSettings
[rest_update_job]: https://msdn.microsoft.com/library/azure/dn820162.aspx
[rest_rdp]: https://msdn.microsoft.com/library/azure/dn820120.aspx
[rest_reboot]: https://msdn.microsoft.com/library/azure/dn820171.aspx
[rest_reimage]: https://msdn.microsoft.com/library/azure/dn820157.aspx
[rest_remove]: https://msdn.microsoft.com/library/azure/dn820194.aspx
[rest_offline]: https://msdn.microsoft.com/library/azure/mt637904.aspx
[rest_online]: https://msdn.microsoft.com/library/azure/mt637907.aspx

[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
