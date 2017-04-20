<properties
   pageTitle="Penetration Tests | Microsoft Azure"
   description="Artikel bietet einen Überblick der Penetration Tests (Pentest) Vorgang und Leistung Pentest gegen Ihre apps in Azure Infrastruktur ausgeführt."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="yurid"/>

# <a name="pen-testing"></a>Penetration Tests

Großes Tests und Bereitstellung mit Microsoft Azure gehört, dass Sie zusammen eine lokalen Infrastruktur entwickeln, testen und Bereitstellen der Anwendung benötigen. Die Infrastruktur ist von Microsoft Azure Platform Services übernommen. Sie haben Fertigungszyklus erwerben, und "Racks und Stapeln" eigene lokale Hardware.

Dies ist großartig, aber Sie müssen noch sicherstellen, dass Ihre normale Sicherheit durchführen Sorgfalt. Ist eines der Dinge Sie müssen Eindringen bereitstellen Anwendung in Azure testen.

Möglicherweise wissen Sie bereits, dass Microsoft [Durchdringungstests Umweltschutz Azure](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)ausgeführt wird. Dies hilft uns, unsere Plattform zu verbessern und unsere Handlungen Einführung in neue Sicherheitsfunktionen, Verbesserung der Sicherheit und Verbesserung unserer Sicherheit führt.

Wir Stift nicht Testen Ihrer Anwendung, aber wir verstehen wird und Stift auf Ihrer eigenen Anwendung ausführen müssen. Deshalb gut, wenn die Sicherheit der Anwendung verbessern, helfen Azure gesamte Ökosystem sicherer.

Wenn Sie Stift Testen Ihrer Anwendung könnte dieser Angriff uns aussehen. Wir [Überwachen ständig](http://blogs.msdn.com/b/azuresecurity/archive/2015/07/05/best-practices-to-protect-your-azure-deployment-against-cloud-drive-by-attacks.aspx) Angriffe Muster und wird eine Vorgehensweise bei Vorfällen nach Bedarf. Es hilft Ihnen nicht und es nicht helfen Sie uns, wenn wir durch Ihre due Diligence Stift Tests Zwischenfälle reagiert auslösen.

Was ist zu tun?

Pen nun Azure gehosteten Anwendung, müssen Sie uns mitteilen. Sobald wir wissen, dass Sie bestimmte Tests durchführt, nicht wir versehentlich fahren Sie (wie blockiert die IP-Adresse, der Sie testen), als der Azure Tests entsprechen Stift testen Geschäftsbedingungen.
Standard können durchgeführten Tests umfassen Folgendes:

- Tests auf Ihre Endgeräte [Open Web Application Security Project (OWASP) Top 10 Sicherheitslücken](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project) aufzudecken
- Endpunkte [Fuzzy-Tests](https://blogs.microsoft.com/cybertrust/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/)
- Endpunkte [Portscans](https://en.wikipedia.org/wiki/Port_scanner)

Ein ist Test, die Sie ausführen können jede Art von [Denial of Service (DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack) -Angriffen. Dies schließt einen DoS-Angriff initiieren oder verknüpften Tests, die bestimmen, zeigen simulieren, jede Art von DoS-Angriff durchgeführt.

Testen Sie startbereit mit Microsoft Azure gehostete Anwendung? Wenn Ja, auf die [Penetration Tests Overview](https://security-forms.azure.com/penetration-testing/terms) Seite (und auf die Schaltfläche Testen Anforderung am unteren Rand der Seite erstellen. Außerdem finden weitere Informationen zu den Stift testen Geschäftsbedingungen und hilfreiche Links auf von Sicherheitslücken im Zusammenhang mit Azure oder anderen Microsoft-Dienst Berichte.
