<properties
    pageTitle="Oberfläche Hubs mit Protokollanalyse überwachen | Microsoft Azure"
    description="Verwenden Sie Fläche Hub-Lösung zu überwachen des Zustands der Fläche Hubs wie sie verwendet werden."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="monitor-surface-hubs-with-log-analytics"></a>Monitor Fläche Hubs mit Protokollanalyse

Dieser Artikel beschreibt, wie Sie Surface Hub-Lösung in Protokollanalyse Microsoft Surface Hubs mit Microsoft Operations Management Suite (OMS) überwachen. Protokollanalyse können Sie den Zustand der Ihre Fläche nachverfolgen sowie zu verstehen, wie sie verwendet werden.

Jede Fläche Hub ist Microsoft Monitoring Agent installiert. Die durch den Agent, können Sie Daten aus Ihrem Haupt-Oberfläche zu OMS senden. Protokolldateien der Fläche Hubs und lesen und an den OMS-Dienst gesendet werden. Wie Server offline Kalender verwendet, oder wenn das Gerät Konto anmelden kann werden Skype in OMS in Surface Hub Dashboard angezeigt. Anhand der Daten im Dashboard erkennen Sie Geräte, die nicht ausgeführt oder sind andere Probleme, und gelten eventuell erkannten Probleme behoben.


## <a name="installing-and-configuring-the-solution"></a>Installation und Konfiguration der Lösung

Anhand der folgenden Informationen zum Installieren und Konfigurieren der Lösung. Um die Fläche Hubs aus Microsoft Operations Management Suite (OMS) verwalten, benötigen Sie Folgendes:

- Ein gültiges Abonnement für [OMS](http://www.microsoft.com/oms).
- Ein [OMS](https://azure.microsoft.com/pricing/details/log-analytics/) Abonnementstufe, die die Anzahl von Geräten unterstützt, die Sie überwachen möchten. OMS Preise variieren je nach wie viele Geräte registriert sind und wie viele Daten sie Prozesse. Sie sollten dies berücksichtigen bei der Planung Ihrer Implementierung Surface Hub.

Als Nächstes Sie Microsoft Azure-Abonnement ein OMS-Abonnement hinzufügen oder einen neuen Arbeitsbereich direkt über den OMS-Portal erstellen. Ausführliche Informationen zur Verwendung der beiden Methoden ist [Erste Schritte mit Protokollanalyse](log-analytics-get-started.md). Nach des Abonnements OMS einrichten zweierlei Fläche Hub Geräte registrieren:

- Automatisch durch Windows InTune
- Manuell über die **Einstellungen** auf dem Surface Hub-Gerät.

## <a name="set-up-monitoring"></a>Einrichten der Überwachung

Überwachen Sie den Zustand und die Aktivität des in OMS Protokollanalyse mit Haupt-Fläche. Surface Hub in OMS können mittels InTune oder lokal **auf der Fläche Hub** registriert werden.

## <a name="connect-surface-hubs-to-oms-through-intune"></a>OMS über InTune Fläche Hubs anschließen

Die Arbeitsplatz-ID und Arbeitsbereich Schlüssel benötigen für OMS-Arbeitsbereich Sie, die Ihre Hubs Oberfläche verwalten. Die OMS-Portal erhalten.

Windows InTune ist ein Microsoft-Produkt, mit dem Sie die OMS-Konfigurationen zentral verwalten, die auf eine oder mehrere Geräte angewendet werden. Gehen Sie Ihre Geräte InTune konfigurieren

1. Melden Sie sich bei Windows InTune.
2. Navigieren Sie zu **Settings** > **Quellen verbunden**.
3. Erstellen oder Bearbeiten einer richtlinienbasiertes basierend auf der Fläche Hub-Vorlage.
4. Navigieren Sie zum Abschnitt OMS (Azure betriebliche Erkenntnisse) der Richtlinie und der Richtlinie fügen Sie die *Arbeitsplatz-ID* und *Arbeitsbereich Schlüssel hinzu* .
5. Speichern Sie die Richtlinie.
6. Ordnen Sie die Richtlinie der entsprechenden Gruppe von Geräten.

  ![Windows InTune-Richtlinien](./media/log-analytics-surface-hubs/intune.png)

InTune synchronisiert dann OMS-Einstellung mit der Zielgruppe im Arbeitsbereich OMS registrieren.

## <a name="connect-surface-hubs-to-oms-using-the-settings-app"></a>OMS Anwendung Einstellungen schließen Sie Fläche Hubs an

Die Arbeitsplatz-ID und Arbeitsbereich Schlüssel benötigen für OMS-Arbeitsbereich Sie, die Ihre Hubs Oberfläche verwalten. Die OMS-Portal erhalten.

Wenn Sie InTune zur Verwaltung der Umgebung verwenden, können Sie Geräte manuell durch **Einstellung** auf jede Fläche Hub registrieren:

1. Der Hub Fläche öffnen Sie **Einstellungen**.
2. Geben Sie Administratoranmeldeinformationen Gerät Aufforderung.
3. Klicken Sie auf **das Gerät**und unter **Überwachung**klicken **konfiguriert OMS**.
4. **Aktivieren Sie die Überwachung**aktivieren.
6. OMS-Dateispeicherort Geben Sie die **Arbeitsplatz-ID** und der **Workspace-Schlüssel**.  
  ![Einstellungen](./media/log-analytics-surface-hubs/settings.png)
7. Klicken Sie auf **OK** , um die Konfiguration abzuschließen.

Eine Bestätigung wird angezeigt, ob die OMS-Konfiguration auf dem Gerät erfolgreich angewendet wurde. Es wurde eine Meldung angezeigt, dass der Agent an den OMS-Dienst hergestellt. Das Gerät startet das Senden von Daten an OMS können Sie anzeigen und handeln.

## <a name="monitor-surface-hubs"></a>Monitor Fläche Hubs

Überwachung der Fläche Hubs ähnelt OMS mit registrierten Geräte überwachen.

1. Mit OMS-Portal anmelden.
2. Navigieren Sie zu Surface Hub Lösung Pack Dashboard.
3. Der Device-Status wird angezeigt.

  ![Fläche Hub-dashboard](./media/log-analytics-surface-hubs/surface-hub-dashboard.png)

[Alarme](log-analytics-alerts.md) basierend auf vorhandenen oder benutzerdefinierten Protokoll suchen können. Mit den Daten, die von der Fläche der OMS sammelt, können Sie suchen Probleme und Warnung abhängig, die Sie für Ihre Geräte definieren.


## <a name="next-steps"></a>Nächste Schritte

- Verwenden Sie [Log durchsucht Protokollanalyse](log-analytics-log-searches.md) Fläche Hub Detaildaten anzeigen.
- [Alerts](log-analytics-alerts.md) benachrichtigen treten Probleme mit der Fläche zu erstellen.
