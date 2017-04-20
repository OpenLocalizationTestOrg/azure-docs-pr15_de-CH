<properties
   pageTitle="Operations Management Suite Sicherheit und Audits Lösung Baseline | Microsoft Azure"
   description="Dieses Dokument beschreibt die OMS Sicherheit und Lösung zu eine Baseline Bewertung aller überwachten Computer für Compliance und Sicherheit führen."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/08/2016"
   ms.author="yurid"/>

# <a name="baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a>Geplante Bewertung Operations Management Suite Sicherheits- und Audit-Lösung

Dieses Dokument hilft Ihnen mit [Operations Management Suite (OMS) Sicherheits- und Audit-Lösung](operations-management-suite-overview.md) Baseline Bewertung Funktionen auf sicheren Zustand der überwachten Ressourcen zugreifen.

## <a name="what-is-baseline-assessment"></a>Was ist die geplante Bewertung?

Microsoft definiert zusammen mit Wirtschaftsregeln und Organisationen eine Windows-Konfiguration, die Bereitstellung sehr sicheren Server darstellt. Diese Konfiguration ist eine Reihe von Registrierungsschlüsseln, Audit-Richtlinien und Sicherheitsrichtlinien sowie von Microsoft empfohlene Wert für diese Einstellung. Dieser Satz von Regeln wird als Sicherheitsgrundlage bezeichnet. OMS Sicherheits- und geplante Bewertung Funktionen kann alle Computer für Compliance nahtlos scannen. 

Es gibt drei Arten von Regeln:

- **Registrierungspfadregeln**: Überprüfen Sie, dass die Registrierungsschlüssel ordnungsgemäß eingestellt sind.
- **Überwachungsrichtlinienregeln**: Regeln für Ihre Überwachungsrichtlinie.
- **Sicherheitsrichtlinien**: Regeln für die Berechtigungen des Benutzers auf dem Computer.

> [AZURE.NOTE] Finden Sie eine kurze Übersicht über diese Funktion [OMS-Sicherheit Sicherheit Basiskonfiguration zu verwenden](https://blogs.technet.microsoft.com/msoms/2016/08/12/use-oms-security-to-assess-the-security-configuration-baseline/) .

## <a name="security-baseline-assessment"></a>Geplante Bestandsaufnahme

Überprüfen Sie Ihre aktuelle Baseline-Bestandsaufnahme für alle Computer, die von OMS Sicherheits- und mit dem Dashboard überwacht werden.  Führen Sie Folgendes Security Baseline Bewertung Dashboard zugreifen:

1. Klicken Sie im Schaltpult wichtigsten **Microsoft Operations Management Suite** auf **Sicherheits- und** nebeneinander.
2. Klicken Sie im Schaltpult **Sicherheits- und** **Geplante Bewertung** unter **Sicherheitsdomänen**. **Bestandsaufnahme der Baseline** -Schaltpult wird angezeigt, wie in der folgenden Abbildung dargestellt:
    
    ![OMS-Sicherheit und Audits Baseline Bewertung](./media/oms-security-baseline/oms-security-baseline-fig1.png)

Das Dashboard ist in drei Hauptbereiche unterteilt:

- **Computer im Vergleich zu geplanten**: Dieser Abschnitt enthält eine Übersicht über die Anzahl der Computer, auf die zugegriffen wurde und den Prozentsatz der Computer, auf denen die Beurteilung übergeben. Es gibt auch Top 10 Computer und das Ergebnis in Prozent für die Bewertung.
- **Erforderliche Regeln Status**: in diesem Abschnitt wurde die Absicht zu Bewusstsein der fehlgeschlagenen Regeln nach Schweregrad und fehlgeschlagenen Regeln vom Typ. Anhand der ersten Diagramms können Sie schnell identifizieren die meisten fehlgeschlagenen Regeln wichtig sind. Es gibt auch eine Liste der Top 10 fehlgeschlagenen Regeln und Schweregrad. Das zweite Diagramm zeigt die Regel, die während der Bewertung nicht. 
- **Computer fehlen Baseline Bewertung**: Dieser Abschnitt Liste Computer Betriebssystem Inkompatibilität oder Fehlern nicht zugegriffen wurde. 

### <a name="accessing-computers-compared-to-baseline"></a>Zugriff auf Computer im Vergleich zur baseline

Im Idealfall sein alle Computer mit geplanten Bestandsaufnahme kompatibel. Jedoch ist vorgesehen, daß in einigen Situationen dies nicht geschehen. Als Teil des Sicherheits-Managements ist es wichtig, überprüfen die Computer, die alle Security Assessment Tests nicht bestanden. Schnell visualisieren, die durch die Option im Abschnitt **Computer im Vergleich zu geplanten** **Computer zugreifen** . Zeigt die Liste der Computer im folgenden Bildschirm zeigt Log Ergebnis sollte angezeigt werden:

![Zugriff führt](./media/oms-security-baseline/oms-security-baseline-fig2.png)

Das Suchergebnis wird, bei denen die erste Spalte hat den Computernamen und die zweite Farbe die Anzahl der Regeln, die nicht in einem Tabellenformat angezeigt. Rufen Sie Informationen über den Typ der Regel, die nicht auf die Anzahl der fehlgeschlagenen Regeln neben dem Computernamen klicken. Wie in der folgenden Abbildung gezeigten Ergebnis sollte angezeigt werden:

![Zugriff auf Computer anzeigen](./media/oms-security-baseline/oms-security-baseline-fig3.png)

In das Suchergebnis haben Sie insgesamt zugegriffen Regeln, Anzahl wichtiger Regeln Fehler, Warnung Regeln und die Regeln Informationen ist fehlgeschlagen.

### <a name="accessing-required-rules-status"></a>Zugriff auf erforderliche Regeln-status

Nach Informationen über die Prozentzahl der Computer, die die Bewertung übergeben möchten Sie mehr Informationen über die Regeln nach der Wichtigkeit fehlschlagen. Diese Visualisierung können Sie Prioritäten, welche Computer zunächst richten um sicherzustellen, dass sie in die nächste Bewertung kompatibel sind. Mauszeiger kritischen Teil des Diagramms in der Kachel **fehlgeschlagenen Regeln nach Schweregrad** unter **erforderlichen Regeln Status** befindet, und klicken Sie darauf. Ähnlich wie das folgende Ergebnis sollte angezeigt werden:

![Informationen zum Schweregrad Regeln Fehler](./media/oms-security-baseline/oms-security-baseline-fig4.png) 

In diesem Protokoll Ergebnis Baseline-Regeltyp, der die Beschreibung dieser Regel und die allgemeine Konfiguration Enumeration (CCE) ID dieser Regel nicht angezeigt. Diese Attribute sollten Maßnahme zum Beheben dieses Problems im Zielcomputer ausgeführt werden.

> [AZURE.NOTE] Weitere Informationen zu CCE Zugriff auf der [National Vulnerability Database](https://nvd.nist.gov/cce/index.cfm).

### <a name="accessing-computers-missing-baseline-assessment"></a>Zugriff auf Computer fehlen Baseline-Bewertung

OMS unterstützt das Domänenprofil Member Baseline für Windows Server 2008 R2 bis Windows Server 2012 R2. Windows Server 2016 geplanten noch nicht final und hinzugefügt, sobald es veröffentlicht wird. Alle anderen Betriebssysteme über OMS Sicherheits- und geplante Bewertung gescannt wird unter **Computer geplanten Bewertung fehlen** .

## <a name="see-also"></a>Siehe auch

In diesem Dokument haben Sie zu OMS Sicherheits- und Baseline. Weitere OMS-Sicherheit finden Sie in den folgenden Artikeln:

- [Operations Management Suite (OMS) (Übersicht)](operations-management-suite-overview.md)
- [Überwachen und reagieren auf Sicherheitshinweise Operations Management Suite Sicherheit und Audit-Lösung](oms-security-responding-alerts.md)
- [Überwachen von Ressourcen in Operations Management Suite Sicherheit und Audit-Lösung](oms-security-monitoring-resources.md)

