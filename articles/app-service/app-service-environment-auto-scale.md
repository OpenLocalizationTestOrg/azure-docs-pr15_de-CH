<properties
    pageTitle="Automatische Skalierung und App Service-Umgebung | Microsoft Azure"
    description="Automatische Skalierung und App Service-Umgebung"
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="byvinyal"
/>

# <a name="autoscaling-and-app-service-environment"></a>Automatische Skalierung und App Service-Umgebung

Azure App Service-Umgebung unterstützen die *Automatische Skalierung*. Skalierungsgröße Einzelunternehmer Pools basierend auf Kennzahlen oder planen können.

![Optionen für einen Arbeitspool skalieren.][intro]

Automatische Skalierung optimiert die Ressourcenverwendung durch automatisch vergrößern und Verkleinern einer App Service-Umgebung zu Ihrem Budget passt und Profil laden.

## <a name="configure-worker-pool-autoscale"></a>Konfigurieren der Arbeitskraft Pool skalieren

Sie können der Registerkarte **Einstellungen** dem Arbeitspool skalieren-Funktionen zugreifen.

![Registerkarte von dem Arbeitspool.][settings-scale]

Dort ist die Schnittstelle ziemlich vertraut, da sie Erfahrung beim Skalieren eines App Service-Plans angezeigt. 

![Manuelle Einstellungen.][scale-manual]

Sie können auch eine Profil skalieren.

![Skalierungsgröße Settings.][scale-profile]

Skalierungsgröße Profile sind Grenzwerte für die Skalierung festlegen. Auf diese Weise haben Sie eine gleichbleibende Leistung auftreten, indem Sie einen Skalierungswert Untergrenze (1) und eine vorhersagbare Ausgabe Cap durch Festlegen einer Obergrenze (2).

![Einstellungen im Profil.][scale-profile2]

Nachdem Sie ein Profil definieren, können Sie Regeln oder die Anzahl der Instanzen in dem Arbeitspool Profil festgelegten Rahmen skalieren hinzufügen. Skalierungsgröße Regeln basieren auf Metriken.

![Scale-Regel.][scale-rule]

 Arbeitspool oder Front-End-Metriken dienen Autoscale Regeln definieren. Diese Metriken sind die gleichen Metriken überwachen Ressource Blade-Diagramme oder Alarm für festlegen.

## <a name="autoscale-example"></a>Skalieren-Beispiel

Skalieren einer App Service-Umgebung kann am besten anhand eines Szenarios veranschaulicht werden.

Erläutert die erforderlichen Aspekte beim Skalieren einrichten. Der Artikel führt Sie durch Aktivitäten ins Spiel kommen, wenn man Skalierung App Umgebungen, die App Service-Umgebung gehostet werden.

### <a name="scenario-introduction"></a>Szenario-Einführung

Frank ist ein Systemadministrator eines Unternehmens, die einen Teil der Arbeitslast, die er verwaltet eine App Service-Umgebung migriert wurde.

Die App Service-Umgebung wird manuelle Skalierung konfiguriert:

* **Vorne enden:** 3
* **Arbeitspool 1**: 10
* **Arbeitspool 2**: 5
* **Arbeitspool 3**: 5

Arbeitspool 1 Produktions-Workloads dient Arbeitskraft 2 und Arbeitskraft 3 (QA) und Entwicklung Arbeitslasten verwendet werden.

App Service-Pläne für Qualitätskontrolle und Entwicklung sind manuelle Skalierung konfiguriert. Die Produktion App Service-Plan Skalierungsgröße soll mit laden und verarbeiten.

Frank ist mit der Anwendung. Er weiß, dass Spitzenzeiten Auslastung zwischen 9:00 Uhr und 18:00 Uhr ist dies eine Line-of-Business (LOB)-Anwendung, die Mitarbeiter verwenden, während sie im Büro sind. Verwendung löscht, wenn Benutzer für den Tag fertig sind. Außerhalb Spitzenzeiten gibt es noch einige laden, da Benutzer die Anwendung remote zugreifen können mit ihren mobilen Geräten oder Heim-PCs. Die Produktion App Service-Plan ist bereits Skalierungsgröße basierend auf CPU-Auslastung mit folgenden Regeln konfiguriert:

![Einstellungen für LOB-Anwendung.][asp-scale]

|   **Skalierungsgröße Profil Weekdays-App Service-plan**     |   **Skalierungsgröße Profil Wochenende App Service-plan**     |
|   ----------------------------------------------------    |   ----------------------------------------------------    |
|   **Name:** Wochentag-Profil                               |   **Name:** Wochenende Profil                               |
|   **Skalieren:** Zeitplan und Leistungsregeln            |   **Skalieren:** Zeitplan und Leistungsregeln            |
|   **Profil:** Wochentage                                   |   **Profil:** Am Wochenende                                    |
|   **Typ:** Serie                                    |   **Typ:** Serie                                    |
|   **Zielbereich:** 5 bis 20 Instanzen                     |   **Zielbereich:** 3 bis 10 Instanzen                     |
|   **Tage:** Montag, Dienstag, Mittwoch, Donnerstag, Freitag  |   **Tage:** Samstag, Sonntag                              |
|   **Startzeit:** 9:00 Uhr                                 |   **Startzeit:** 9:00 Uhr                                 |
|   **Zeitzone:** UTC-08                                   |   **Zeitzone:** UTC-08                                   |
|                                                           |                                                           |
|   **Regel skalieren (Scale Up)**                           |   **Regel skalieren (Scale Up)**                           |
|   **Ressource:** Produktion (App Service-Umgebung)      |   **Ressource:** Produktion (App Service-Umgebung)      |
|   **Metrik:** CPU%                                       |   **Metrik:** CPU%                                       |
|   **Vorgang:** Mehr als 60 %                         |   **Vorgang:** Mehr als 80 %                         |
|   **Dauer:** 5 Minuten                                 |   **Dauer:** 10 Minuten                                |
|   **Aggregation Zeit:** Durchschnitt                           |   **Aggregation Zeit:** Durchschnitt                           |
|   **Aktion:** Anzahl von 2 erhöhen                         |   **Aktion:** Anzahl um 1 erhöhen                         |
|   **(Minuten) abkühlen:** 15                             |   **(Minuten) abkühlen:** 20                             |
|                                                           |                                                           |
  	|   **Regel skalieren (Scale nach unten)**                     |   **Regel skalieren (Scale nach unten)**                         |
|   **Ressource:** Produktion (App Service-Umgebung)      |   **Ressource:** Produktion (App Service-Umgebung)      |
|   **Metrik:** CPU%                                       |   **Metrik:** CPU%                                       |
|   **Vorgang:** Weniger als 30 %                            |   **Vorgang:** Weniger als 20 %                            |
|   **Dauer:** 10 Minuten                                |   **Dauer:** 15 Minuten                                |
|   **Aggregation Zeit:** Durchschnitt                           |   **Aggregation Zeit:** Durchschnitt                           |
|   **Aktion:** 1 Anzahl verringern                         |   **Aktion:** 1 Anzahl verringern                         |
|   **(Minuten) abkühlen:** 20                             |   **(Minuten) abkühlen:** 10                             |

### <a name="app-service-plan-inflation-rate"></a>App Service Plan Inflationsrate

App Service-Pläne, die konfiguriert sind Skalierungsgröße tun maximale Rate pro Stunde. Dieser Satz kann basierend auf skalieren bereitgestellten Werte berechnet.

Verstehen und berechnen die *Inflationsrate für App Service-Plan* ist wichtig für App Service-Umgebung skalieren, weil Maßstabs einer Arbeitskraft Pool nicht unmittelbar.

Die Inflationsrate App Service-Plan wird wie folgt berechnet:

![App Service Inflationsrate Berechnung.][ASP-Inflation]

Basierend auf skalieren – skalieren Regel Profil Wochentag Produktion App Service-Plan:

![App Service Plan Inflationsrate Wochentage skalieren – skalieren Regel abhängig.][Equation1]

Bei skalieren – skalieren Regel für das Wochenende Profil der Produktion App Service-Plan würde die Formel folgendermaßen aufgelöst:

![App Service Plan Inflationsrate Wochenenden skalieren – skalieren Regel abhängig.][Equation2]

Dieser Wert kann auch für Herunterskalieren Vorgänge berechnet.

Skalieren, skalieren Sie die Regel für Profil Wochentag Produktion App Serviceplan Grundlage würde dies wie folgt aussehen:

![App Service Plan Inflationsrate Wochentage basierend auf skalieren, skalieren Sie die Regel.][Equation3]

Bei skalieren, skalieren Sie die Regel für das Wochenende Profil der Produktion App Service-Plan würde die Formel folgendermaßen aufgelöst:  

![App Service Plan Inflationsrate Wochenenden basierend auf skalieren, skalieren Sie die Regel.][Equation4]

Die Produktion App Service-Plan kann eine maximal acht Instanzen pro Stunde in der Woche und vier Instanzen/Stunde am Wochenende wachsen. Sie können Instanzen mit maximal vier Instanzen pro Stunde in der Woche und sechs Instanzen/Stunde am Wochenende freigeben.

Mehrere App Service-Pläne in einer Arbeitskraft gehostet werden, haben Sie zum Berechnen der *Inflationsrate* als Summe der Inflationsrate für die App Service-Pläne, die im Pool Arbeitskraft.

![Inflationsrate Berechnung für mehrere App Service-Pläne in einer Arbeitskraft gehostet.][ASP-Total-Inflation]

### <a name="use-the-app-service-plan-inflation-rate-to-define-worker-pool-autoscale-rules"></a>Mit der Arbeitskraft Pool Autoscale Regeln die Inflationsrate App Service-plan

Arbeitskraft-pools, Host App Service-Pläne, die konfiguriert sind Skalierungsgröße Puffer Kapazität zugewiesen werden müssen. Der Puffer ermöglicht Autoscale Vorgänge vergrößern und Verkleinern des App Service-Plans nach Bedarf. Die Mindestgröße des Puffers wäre berechneten App Service planen Inflationsrate.

Da App Umgebung skalieren Dienstvorgänge gelten einige Zeit dauern, sollten Änderungen bedarfsänderungen berücksichtigt, die auftreten kann, während eine Skalierung ausgeführt wird. Um diese Wartezeit zu unterstützen, wird empfohlen, verwenden berechnete App Service planen Inflationsrate als die minimale Anzahl von Instanzen hinzugefügt werden für jeden Vorgang automatisch skalieren.

Mit diesen Informationen können Frank folgende Autoscale Profil und Regeln:

![Skalierungsgröße Profil Regeln für LOB-Beispiel.][Worker-Pool-Scale]

|   **Skalierungsgröße Profil Wochentage**                        |   **Skalierungsgröße Profil Wochenenden**                |
|   ----------------------------------------------------    |   --------------------------------------------    |
|   **Name:** Wochentag-Profil                               |   **Name:** Wochenende Profil                       |
|   **Skalieren:** Zeitplan und Leistungsregeln            |   **Skalieren:** Zeitplan und Leistungsregeln    |
|   **Profil:** Wochentage                                   |   **Profil:** Am Wochenende                            |
|   **Typ:** Serie                                    |   **Typ:** Serie                            |
|   **Zielbereich:** 13: 25 Instanzen                    |   **Zielbereich:** 6 bis 15 Instanzen             |
|   **Tage:** Montag, Dienstag, Mittwoch, Donnerstag, Freitag  |   **Tage:** Samstag, Sonntag                      |
|   **Startzeit:** 7:00 Uhr                                 |   **Startzeit:** 9:00 Uhr                         |
|   **Zeitzone:** UTC-08                                   |   **Zeitzone:** UTC-08                           |
|                                                           |                                                   |
|   **Regel skalieren (Scale Up)**                           |   **Regel skalieren (Scale Up)**                   |
|   **Ressource:** Arbeitspool 1                             |   **Ressource:** Arbeitspool 1                     |
|   **Metrik:** WorkersAvailable                            |   **Metrik:** WorkersAvailable                    |
|   **Vorgang:** Weniger als 8                              |   **Vorgang:** Weniger als 3                      |
|   **Dauer:** 20 Minuten                                |   **Dauer:** 30 Minuten                        |
|   **Aggregation Zeit:** Durchschnitt                           |   **Aggregation Zeit:** Durchschnitt                   |
|   **Aktion:** Erhöhen Sie die Anzahl von 8                         |   **Aktion:** Erhöhen Sie die Anzahl von 3                 |
|   **(Minuten) abkühlen:** 180                            |   **(Minuten) abkühlen:** 180                    |
|                                                           |                                                   |
|   **Regel skalieren (Scale nach unten)**                         |   **Regel skalieren (Scale nach unten)**                 |
|   **Ressource:** Arbeitspool 1                             |   **Ressource:** Arbeitspool 1                     |
|   **Metrik:** WorkersAvailable                            |   **Metrik:** WorkersAvailable                    |
|   **Vorgang:** Größer als 8                           |   **Vorgang:** Größer als 3                   |
|   **Dauer:** 20 Minuten                                |   **Dauer:** 15 Minuten                        |
|   **Aggregation Zeit:** Durchschnitt                           |   **Aggregation Zeit:** Durchschnitt                   |
|   **Aktion:** Verringern der Anzahl von 2                         |   **Aktion:** Verringern der Anzahl 3                 |
|   **(Minuten) abkühlen:** 120                            |   **(Minuten) abkühlen:** 120                    |

Zielbereich im Profil definierte errechnet die minimalen Instanzen im Profil für die App Service-Plan + Puffer definiert.

Der maximale Bereich wäre die Summe der maximalen Bereich für alle App Service-Pläne in dem Arbeitspool gehostet.

Anzahl der Erhöhung für die Skalierung der Regeln sollte mindestens 1 X die Inflationsrate Skalierung App Service Plan eingerichtet.

Abnahme der Anzahl können Sie etwas zwischen 1/2 X 1 X App Service Plan Inflationsrate Skalierung angepasst werden.

### <a name="autoscale-for-front-end-pool"></a>Automatische Skalierung für Front-End-pool

Regeln für Front-End-skalieren sind einfacher für Arbeitskraft Pools. Vor allem sollten Sie  
Stellen Sie sicher, dass die Dauer der Messung und die Abklingzeit Zeitgeber halte Skalierung Operationen für eine App Service-Plan nicht unmittelbar.

In diesem Szenario erkennt Frank, dass die Fehlerrate erhöht front-Ends CPU-Auslastung 80 % erreichen und skalieren Regel Instanzen wie folgt zu:

![Skalierungsgröße Einstellungen für Front-End-Pool.][Front-End-Scale]

|   **Skalierungsgröße Profil Frontends**              |
|   --------------------------------------------    |
|   **Name:** Skalieren – Front-ends                |
|   **Skalieren:** Zeitplan und Leistungsregeln    |
|   **Profil:** Täglich                           |
|   **Typ:** Serie                            |
|   **Zielbereich:** 3 bis 10 Instanzen             |
|   **Tage:** Täglich                              |
|   **Startzeit:** 9:00 Uhr                         |
|   **Zeitzone:** UTC-08                           |
|                                                   |
|   **Regel skalieren (Scale Up)**                   |
|   **Ressource:** Front-End-pool                    |
|   **Metrik:** CPU%                               |
|   **Vorgang:** Mehr als 60 %                 |
|   **Dauer:** 20 Minuten                        |
|   **Aggregation Zeit:** Durchschnitt                   |
|   **Aktion:** Erhöhen Sie die Anzahl von 3                 |
|   **(Minuten) abkühlen:** 120                    |
|                                                   |
|   **Regel skalieren (Scale nach unten)**                 |
|   **Ressource:** Arbeitspool 1                     |
|   **Metrik:** CPU%                               |
|   **Vorgang:** Weniger als 30 %                    |
|   **Dauer:** 20 Minuten                        |
|   **Aggregation Zeit:** Durchschnitt                   |
|   **Aktion:** Verringern der Anzahl 3                 |
|   **(Minuten) abkühlen:** 120                    |

<!-- IMAGES -->
[intro]: ./media/app-service-environment-auto-scale/introduction.png
[settings-scale]: ./media/app-service-environment-auto-scale/settings-scale.png
[scale-manual]: ./media/app-service-environment-auto-scale/scale-manual.png
[scale-profile]: ./media/app-service-environment-auto-scale/scale-profile.png
[scale-profile2]: ./media/app-service-environment-auto-scale/scale-profile-2.png
[scale-rule]: ./media/app-service-environment-auto-scale/scale-rule.png
[asp-scale]: ./media/app-service-environment-auto-scale/asp-scale.png
[ASP-Inflation]: ./media/app-service-environment-auto-scale/asp-inflation-rate.png
[Equation1]: ./media/app-service-environment-auto-scale/equation1.png
[Equation2]: ./media/app-service-environment-auto-scale/equation2.png
[Equation3]: ./media/app-service-environment-auto-scale/equation3.png
[Equation4]: ./media/app-service-environment-auto-scale/equation4.png
[ASP-Total-Inflation]: ./media/app-service-environment-auto-scale/asp-total-inflation-rate.png
[Worker-Pool-Scale]: ./media/app-service-environment-auto-scale/wp-scale.png
[Front-End-Scale]: ./media/app-service-environment-auto-scale/fe-scale.png
