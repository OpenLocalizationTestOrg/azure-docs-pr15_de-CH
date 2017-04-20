<properties
   pageTitle="Hybrid Runbook Worker: Ein Runbook Auftrag beendet mit dem Status angehalten | Microsoft Azure"
   description="Symptome Ursachen und Lösungsmöglichkeiten für Hybrid Runbook Worker Beendigung Auftragsfehlers."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/17/2016"
   ms.author="magoedte" />

# <a name="hybrid-runbook-worker-a-runbook-job-terminates-with-a-status-of-suspended"></a>Hybrid Runbook Worker: Ein Runbook Auftrag beendet mit dem Status angehalten

## <a name="summary"></a>Zusammenfassung

Danach versucht dreimal ausgeführt wird Ihr Runbook unterbrochen. Gibt Geschäftsbedingungen Runbook Abschluss unterbrechen können, und die Fehlermeldung, die angibt, warum zusätzliche Informationen nicht enthalten. Dieser Artikel enthält Schritte zur Fehlerbehebung für Probleme mit der Hybrid Runbook Worker Runbook Ausführungsfehler.

Wenn Ihre Azure Problem in diesem Artikel nicht behandelt wird, finden Sie auf [MSDN und den Stapelüberlauf](https://azure.microsoft.com/support/forums/)Azure Foren. Buchen kann das Problem auf diese Foren oder [ @AzureSupport auf Twitter](https://twitter.com/AzureSupport). Auch können Sie durch Auswählen der [Azure-support](https://azure.microsoft.com/support/options/) -Website **erhalten Sie Unterstützung** Azure Supportanfrage einreichen.

## <a name="symptom"></a>Symptom

Runbook Ausführung fehlschlägt und der Fehler ist "der Auftrag Aktion"Activate"ausgeführt werden kann, da der Prozess nicht mehr reagiert. Die Auftragsaktion wurde 3 Mal versucht."


## <a name="cause"></a>Ursache

Es gibt mehrere mögliche Ursachen für diesen Fehler: 

  1. Hybrid-Worker ist hinter einem Proxyserver oder einer firewall
  2. Hybrid-Worker läuft Computer ist weniger Hardware- [Anforderungen](automation-hybrid-runbook-worker.md#hybrid-runbook-worker-requirements) 
  3. Die Runbooks authentifizieren nicht mit lokalen Ressourcen.


## <a name="cause-1-hybrid-runbook-worker-is-behind-proxy-or-firewall"></a>Ursache 1: Hybrid Runbook Worker hinter Proxy oder eine Firewall ist

Der Computer läuft Hybrid Runbook Worker hinter einem Firewall oder Proxy Server und ausgehenden Netzwerkzugriff möglicherweise nicht zulässig oder nicht richtig konfiguriert.

### <a name="solution"></a>Lösung

Überprüfen des Computers hat ausgehenden Zugriff auf *. cloudapp.net auf Port 443, 9354 und 30000 30199. 

## <a name="cause-2-computer-has-less-than-minimum-hardware-requirements"></a>Ursache 2: Computer hat weniger als Mindestanforderungen für Hardware

Hybrid Runbook Worker-Computern sollte die Mindestanforderungen erfüllen, bevor festlegen, damit diese Funktion host. Andernfalls je nach die Ressourcenverwendung anderer Hintergrundprozesse und Konflikte während der Ausführung von Runbooks verursacht, der Computer wird werden über ausgelastet ist und Runbook Projekt verzögert oder Timeouts. 

### <a name="solution"></a>Lösung 

Zuerst bestätigen Sie bezeichneten Hybrid Runbook Worker-Funktion ausführen Computer die Mindestanforderungen erfüllt.  Wenn Ja, überwachen Sie, CPU und RAM-Nutzung um Zusammenhänge zwischen der Leistung der Hybrid Runbook Arbeitsprozesse und Windows zu ermitteln.  Wenn Arbeitsspeicher oder CPU-Überlastung vorhanden ist, kann sein muss aktualisieren oder Hinzufügen zusätzlicher Prozessoren oder Speicher Ressourcenengpass und den Fehler beheben. Alternativ wählen Sie eine andere computeressource, die unterstützt die Mindestanforderungen und Skalierung bei Leistungsdatenanalyse, erforderlich ist.         

## <a name="cause-3-runbooks-cannot-authenticate-with-local-resources"></a>Ursache 3: Runbooks nicht mit lokalen Ressourcen authentifizieren.

### <a name="solution"></a>Lösung

Die **Microsoft-SMA** das Ereignisprotokoll für ein entsprechendes Ereignis mit *Win32-Prozess wurde mit Code [4294967295] beendet*.  Die Ursache dieses Fehlers ist nicht in Ihrem Runbooks Authentifizierung konfiguriert oder Ausführen als Anmeldeinformationen für hybride Arbeitskraftgruppe angegeben.  Überprüfen Sie [Runbook Berechtigungen](automation-hybrid-runbook-worker.md#runbook-permissions) bestätigen, dass Sie Ihre Runbooks korrekt Authentifizierung konfiguriert haben.  


 

