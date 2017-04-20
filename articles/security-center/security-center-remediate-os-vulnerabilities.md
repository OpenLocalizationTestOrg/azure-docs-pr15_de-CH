<properties
   pageTitle="Beseitigen Schwachstellen in Azure Security Center OS | Microsoft Azure"
   description="Dieses Dokument veranschaulicht die Azure Security Center Empfehlung **OS beseitigen Schwachstellen**implementieren."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/17/2016"
   ms.author="terrylan"/>

# <a name="remediate-os-vulnerabilities-in-azure-security-center"></a>Behebung von OS-Schwachstellen in Azure Security Center

Azure Security Center analysiert täglich Ihre Virtual Machine (VM) Betriebssystem (OS) für Konfigurationen, die können den virtuellen Computer anfälliger für Angriffe und Neukonfiguration von diesen Schwachstellen empfiehlt. Weitere Informationen über die spezifischen Konfigurationen überwacht finden Sie unter [Regelliste empfohlen](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Sicherheitscenter empfiehlt, Sicherheitslücken beheben, wenn die VM-Betriebssystemkonfiguration nicht die empfohlene Konfigurationsregeln.

> [AZURE.NOTE] Dieses Dokument stellt den Dienst mithilfe einer beispielbereitstellung.  Dies ist nicht Schritt.

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlung

1. Wählen Sie Blatt **Recommendations** **OS beseitigen Schwachstellen**.
![Betriebssystem-Sicherheitslücken beseitigen][1]

    **OS beseitigen Schwachstellen** Blade wird geöffnet und listet die VMs mit OS, die nicht die empfohlene Konfigurationsregeln entsprechen.  Für jeden VM bezeichnet das Blade:

   - **FEHLGESCHLAGENEN Regeln** --die Anzahl der Regeln, die die VM OS Konfiguration ist fehlgeschlagen.
   - **Letzte SCAN-Zeit für** Datum und Uhrzeit, Security Center die VM-Betriebssystemkonfiguration zuletzt gescannt.
   - **Status** - den aktuellen Status des Sicherheitsrisikos:

        - Öffnen: Die Schwachstelle wurde noch nicht behandelt wurde
        - Ausgeführt: Die Schwachstelle derzeit angewendet wird, ist keine Aktion Ihrerseits erforderlich
        - Behoben: Die Schwachstelle wurde bereits (wenn das Problem behoben ist, der Eintrag ist abgeblendet)
  - **Schweregrad** – alle sollen Schweregrad niedrig, d. h. eine Schwachstelle werden jedoch keine unmittelbare Aufmerksamkeit erfordert.

Wählen Sie einen virtuellen Computer. Blade für diese VM wird geöffnet und zeigt die Regeln, die fehlgeschlagen sind.
   ![Variantenregeln fehlgeschlagen][2]

Wählen Sie eine Regel aus. In diesem Beispiel können wählen Sie **Kennwort muss diese Anforderung erfüllen**. Eine Blade öffnet Beschreibung der fehlgeschlagenen Regel und die Auswirkung. Überprüfen Sie die Details und Anwendung Betriebssystemkonfigurationen berücksichtigen.
  ![Beschreibung der fehlgeschlagenen Regel][3]

  Sicherheitscenter verwendet gemeinsame Konfiguration Enumeration (CCE) eindeutige Bezeichner für Regeln zuweisen. Dieses Blatt enthält die folgende Informationen:

  - NAME – Name der Regel
  - Schweregrad – CCE Schweregrad Wert kritisch, wichtig oder Warnung
  - CCIED – CCE Eindeutiger Bezeichner für die Regel
  - Beschreibung – Beschreibung der Regel
  - Sicherheitsrisiko - Erklärung Sicherheitslücken oder Gefahr dar, wenn die Regel nicht angewendet wird
  - Auswirkung - Geschäftlichen Regel angewendet wird
  - : ERWARTET Wert erwartet bei Security Center die VM Betriebssystemkonfiguration gegen
  - -Regel Regel Betrieb während der Analyse der VM OS Konfiguration gegen vom Sicherheitscenter verwendet
  - Istwert - Rückgabewert nach Analyse der VM OS Konfiguration gegen
  - Bewertung –-Ergebnis: Fehler zu übergeben


## <a name="see-also"></a>Siehe auch

Dieser Artikel wurde gezeigt, wie das Sicherheitscenter Empfehlung "Problembehebung OS Vulnerabilities". Überprüfen Sie den Satz von Regeln [hier](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Sicherheitscenter verwendet CCE (Allgemeine Konfiguration Enumeration) Zuweisen von eindeutigen Bezeichnern für Regeln. Der [CCE](http://cce.mitre.org) Website für Weitere Informationen.

Erfahren Sie mehr über das Sicherheitscenter finden Sie hier:

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md) – Informationen zum Konfigurieren von Sicherheitsrichtlinien für Ihren Azure-Abonnements und Ressourcengruppen.
- [Verwalten von Sicherheitsaspekte in Azure Security Center](security-center-recommendations.md) – erfahren Sie, wie Recommendations Azure Ressourcen schützen.
- [Security Health monitoring in Azure Security Center](security-center-monitoring.md) – erfahren Sie, wie Ihre Azure Ressourcen überwachen.
- [Verwalten von und reagieren auf Sicherheitshinweise in Azure Security Center](security-center-managing-and-responding-alerts.md) - Informationen verwalten und Sicherheitshinweise auf.
- [Überwachung Partner Solutions mit Azure Security Center](security-center-partner-solutions.md) erfahren Sie den Status Ihrer Lösungen Partner überwachen.
- [Azure Security Center FAQ](security-center-faq.md) - finden Sie häufig gestellte Fragen zur Verwendung des Dienstes.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – Blog suchen Beiträge über Azure Sicherheit und Compliance.

<!--Image references-->
[1]: ./media/security-center-remediate-os-vulnerabilities/recommendation.png
[2]:./media/security-center-remediate-os-vulnerabilities/vm-remediate-os-vulnerabilities.png
[3]: ./media/security-center-remediate-os-vulnerabilities/vulnerability-details.png
