<properties
    pageTitle="Geschachtelte Traffic Manager Profile | Microsoft Azure"
    description="Erläutert das Feature "Geschachtelt Profile" Azure Traffic Manager"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="nested-traffic-manager-profiles"></a>Geschachtelte Traffic Manager-Profile

Traffic Manager umfasst eine Reihe von Datenverkehr weiterleiten Methoden, mit denen Sie steuern, wie Traffic Manager wählt der Endpunkt Datenverkehr von jeder Endbenutzer erhalten soll. Weitere Informationen finden Sie unter [Traffic Manager Streckenführung Methoden](traffic-manager-routing-methods.md).

Jedes Profil Traffic Manager gibt eine einzelne Methode Datenverkehr weiterleiten. Es gibt jedoch Szenarien, die anspruchsvollere Streckenführung erfordern als Arbeitsgang von einem einzigen Traffic Manager-Profil bereitgestellt. Traffic Manager Profile, um die Vorteile von mehrere Datenverkehr weiterleiten-Methode können geschachtelt werden. Geschachtelte Profile können Sie das Standardverhalten Traffic Manager größere und komplexere anwendungsbereitstellung überschreiben.

Die folgenden Beispiele veranschaulichen, wie geschachtelte Traffic Manager Profile in verschiedenen Szenarien verwenden.

## <a name="example-1-combining-performance-and-weighted-traffic-routing"></a>Beispiel 1: Kombinieren von 'Leistung' und "Weighted" Streckenführung

Angenommen, eine Anwendung in den folgenden Bereichen Azure bereitgestellt: Westen der USA, Europa und Asien. Mithilfe Traffic Manager "Leistung" Streckenführung Methode auf Ihre Region der Benutzer verteilen.

![Einzelne Traffic Manager-Profil][1]

Angenommen Sie möchten ein Update für den Dienst zu testen, bevor Sie weit mehr Rollen. Sie möchten gewichtete"Streckenführung Methode einen kleinen Prozentsatz der Datenverkehr die Testkonfiguration angewiesen. Die Testkonfiguration neben der vorhandenen Bereitstellung in der Produktion in Europa eingerichtet.

Können nicht beide Weighted kombiniert und "Performance Datenverkehrs-routing in einem einzigen Profil. Zur Unterstützung dieses Szenarios wird ein Traffic Manager-Profil mit zwei Westeuropa Endpunkte und gewichtet"Streckenführung Methode erstellen. Fügen Sie dieses Profil 'Kind' als Endpunkt 'Parent' Profil. Übergeordnete Profil noch Leistung Streckenführung Methode verwendet und enthält die globale Bereitstellung als Endpunkte.

Die folgende Abbildung veranschaulicht dieses Beispiel:

![Geschachtelte Traffic Manager-Profile][2]

In dieser Konfiguration verteilt über die übergeordnete Profil gerichtete Datenverkehr Datenverkehr Regionen normalerweise. In Westeuropa verteilt geschachtelte Profil Datenverkehr Produktions- und Endpunkte nach Gewichtung zugeordnet.

Wenn übergeordnete Profil "Leistung" Streckenführung Methode verwendet, muss jeder Endpunkt eine Position zugewiesen. Der Speicherort wird beim Konfigurieren des Endpunkts zugewiesen. Wählen Sie Ihre Azure Region der Bereitstellung. Azure Bereiche sind Positionswerte Internet Wartezeit Tabelle unterstützt. Weitere Informationen finden Sie unter [Traffic Manager "Leistung" Streckenführung Methode](traffic-manager-routing-methods.md#performance-traffic-routing-method).

## <a name="example-2-endpoint-monitoring-in-nested-profiles"></a>Beispiel 2: Endpunktüberwachung geschachtelte Profile

Traffic Manager überwacht aktiv den Zustand jeder Endpunkt. Wenn ein Endpunkt fehlerhaft ist, weist Traffic Manager Benutzer alternative Endpunkte, die Verfügbarkeit des Dienstes zu erhalten. Dieser Endpunkt monitoring und Failover gilt für alle Methoden Datenverkehr weiterleiten. Weitere Informationen finden Sie unter [Traffic Manager Endpunkt überwachen](traffic-manager-monitoring.md). Überwachen der Endpunkt Funktionsweise für geschachtelte Profile. Mit verschachtelten ausführen nicht übergeordneten Profil Gesundheitskontrolle für das untergeordnete Element direkt. Stattdessen dient der Zustand der untergeordneten Profil Endpunkte den Gesamtzustand der untergeordneten Profil berechnet. Diese Gesundheitsinformationen wird Profil geschachtelte Hierarchie weitergegeben. Dieser Zustand zum direkten Verkehr untergeordneten Profil bestimmt aggregiert Parent-Profil Abschnitt der [häufig gestellten Fragen](#faq) Weitere Einzelheiten Gesundheitsberichterstattung geschachtelte Profile.

Nach dem vorherigen Beispiel Produktionsbetrieb in Westeuropa nicht angenommen. Standardmäßig leitet 'Child'-Profil den gesamten Datenverkehr an die Testkonfiguration. Ausfall der Testkonfiguration auch bestimmt Profil übergeordnete, untergeordnete Profil nicht passieren dürfen alle untergeordneten Endpunkte fehlerhaft sind. Übergeordnete Profil dann verteilt Verkehr an andere Regionen.

![Geschachtelte Profil Failover (Standardverhalten)][3]

Sie können mit diesem gerne. Oder Sie befürchtet, dass Datenverkehr für Europa jetzt Testkonfiguration statt einer begrenzten Datenverkehr wird. Unabhängig von der Gesundheit der testbereitstellung möchten Sie anderen Regionen Failover fällt die Bereitstellung in der Produktion in Europa. Zum Aktivieren dieser Failover können beim untergeordneten Profil als Endpunkt im übergeordneten Profil konfigurieren 'MinChildEndpoints'-Parameter angeben. Der Parameter bestimmt die minimale Anzahl von verfügbaren Endpunkte im kinderprofil. Der Standardwert ist "1". In diesem Szenario legen Sie den Wert MinChildEndpoints 2. Unter diesem Schwellenwert übergeordnete Profil betrachtet das gesamte untergeordnete Profil nicht verfügbar und leitet den Datenverkehr mit anderen Endpunkten.

Die folgende Abbildung zeigt diese Konfiguration:

![Geschachtelte Profil Failover mit 'MinChildEndpoints' = 2][4]

>[AZURE.NOTE]
>'Priority' Streckenführung Methode verteilt alle Verkehr zu einem einzigen Endpunkt. Deshalb wenig MinChildEndpoints Einstellung als "1" für ein kinderprofil.

## <a name="example-3-prioritized-failover-regions-in-performance-traffic-routing"></a>Beispiel 3: Priorisierte Failover Regionen Streckenführung "Leistung"

Das Standardverhalten für "Leistung" Streckenführung Methode dient den nächsten Endpunkt zu laden und Kaskadierung mehrerer Fehler verursacht. Fällt ein Endpunkt wird Datenverkehr, der an diesen Endpunkt geleitet wurden würde gleichmäßig an die Endpunkte in allen Regionen verteilt.

!["Leistung" Datenverkehr mit Standard-failover][5]

Aber Angenommen Sie, Sie möchten Westen der USA, Westeuropa Datenverkehr Failover und direkten Datenverkehr an andere Regionen Wenn beide Endpunkte nicht verfügbar sind. Sie können diese Lösung mit einem untergeordneten Profil mit 'Priority' Streckenführung Methode erstellen.

!["Leistung" Datenverkehr mit bevorzugten failover][6]

Seit der Endpunkt Westeuropa höheren Priorität als der Endpunkt Westen der USA, alle Datenverkehr an Westeuropa Endpunkt gesendet, wenn beide Endpunkte online sind. Westeuropa andernfalls richtet der Datenverkehr an Westen der USA. Geschachtelte Profil Datenverkehr Ostasien richtet erst Westeuropa und USA Westen fehl.

Sie können dieses Muster für alle Regionen wiederholen. Ersetzen Sie alle drei Endpunkte im übergeordneten Profil mit drei untergeordneten jeweils nacheinander priorisierte Failover.

## <a name="example-4-controlling-performance-traffic-routing-between-multiple-endpoints-in-the-same-region"></a>Beispiel 4: Steuern des Datenverkehrs-routing zwischen mehreren Endpunkten im selben Bereich "Leistung"

Angenommen Sie "Leistung" Streckenführung Methode in einem Profil verwendet wird, mehrere Endpunkte in einer bestimmten Region wird. Standardmäßig wird dieser Bereich gerichtete Datenverkehr über alle verfügbaren Endpunkte in diesem Bereich verteilt.

!["Leistung" Datenverkehr weiterleiten von im Bereich datenverkehrsverteilung (Standardverhalten)][7]

Anstatt mehrere Endpunkte in Europa werden diese Endpunkte in einem separaten untergeordneten Profil eingeschlossen. Untergeordnete Profil wird das übergeordnete Element als einzige Endpunkt in Westeuropa hinzugefügt. Auf untergeordnete Profil können die datenverkehrsverteilung Westeuropa aktivieren prioritätsbasierte oder gewichtete Streckenführung Region steuern.

!["Leistung" Datenverkehr mit benutzerdefinierten im Bereich Verkehr routing][8]

## <a name="example-5-per-endpoint-monitoring-settings"></a>: 5 pro Endpunkt Überwachung konfigurieren

Nehmen Sie Traffic Manager arbeiten reibungslos migrieren lokalen Datenverkehr aus einer Website auf eine neue Cloud-basierte Version in Azure gehostet. Für legacy-Standort soll mit der Homepage URI Site überwachen. Aber für neue Cloud-basierte Version implementieren Sie eine benutzerdefinierte Seite Überwachung (Pfad "/ monitor.aspx"), enthält zusätzliche Kontrollen.

![Traffic Manager-Endpunkt (Standardverhalten) überwachen][9]

Überwachen Einstellungen im Profil Traffic Manager gelten für alle Endpunkte innerhalb eines Profils. Mit verschachtelten verwenden Sie ein anderes kinderprofil pro Standort unterschiedliche Überwachung definieren.

![Traffic Manager-Endpunkt mit pro Endpunkt überwachen][10]

## <a name="faq"></a>Häufig gestellte Fragen

### <a name="how-do-i-configure-nested-profiles"></a>Wie konfigurieren Sie geschachtelte Profile

Geschachtelte Traffic Manager-Profile können mit der Azure-Ressourcen-Manager und den klassischen Azure REST APIs Azure PowerShell-Cmdlets und plattformübergreifende Azure-CLI-Befehle konfiguriert werden. Sie werden auch neue Azure Portal unterstützt. Sie werden nicht im klassischen Portal unterstützt.

### <a name="how-many-layers-of-nesting-does-traffic-manger-support"></a>Wie viele Ebenen verschachtelt wird Datenverkehr Manager unterstützt?

Sie können Profile bis zu 10 Ebenen verschachteln. 'Schleife' sind nicht zulässig.

### <a name="can-i-mix-other-endpoint-types-with-nested-child-profiles-in-the-same-traffic-manager-profile"></a>Können andere Endpunkt mit geschachtelte untergeordnete in demselben Profil Traffic Manager werden kombiniert?

Ja. Es gibt keine Beschränkungen wie Endpunkte verschiedene innerhalb eines Profils kombinieren.

### <a name="how-does-the-billing-model-apply-for-nested-profiles"></a>Wie gilt das Abrechnung Modell für verschachtelte Profile?

Es gibt keine negative Auswirkung auf geschachtelte Profile Preise.

Traffic Manager Fakturierung besteht aus zwei Komponenten: Endpunkt Healthchecks und Millionen von DNS-Abfragen

- Endpunkt Gesundheitskontrolle: kostenfrei untergeordneten Profil als einen Endpunkt in einem übergeordneten Profil konfiguriert. Überwachung von Endpunkten in untergeordneten Profil werden in der üblichen Weise berechnet.
- DNS-Abfragen: jede Abfrage nur einmal gezählt. Eine Abfrage für ein Parent-Profil, die einen Endpunkt aus einem untergeordneten Profil gibt zählt gegen das übergeordnete-Profil.

Einzelheiten finden Sie unter [Traffic Manager Preisseite](https://azure.microsoft.com/pricing/details/traffic-manager/).

### <a name="is-there-a-performance-impact-for-nested-profiles"></a>Besteht Leistungseinbußen für geschachtelte Profile?

Nein. Gibt es keine Leistungseinbußen, wenn geschachtelte Profile verwenden.

Traffic Manager Namenserver Durchlaufen der Profilhierarchie intern bei der Verarbeitung jeder DNS-Abfrage. Eine DNS-Abfrage zu einem übergeordneten Profil erhalten eine DNS-Antwort mit einem Endpunkt aus einem kinderprofil. Ein CNAME-Eintrag wird verwendet, ob Sie ein einzelnes Profil oder geschachtelte Profile. Gibt es einen CNAME-Eintrag für jedes Profil in der Hierarchie erstellen müssen.

### <a name="how-does-traffic-manager-compute-the-health-of-a-nested-endpoint-in-a-parent-profile"></a>Berechnen Traffic Manager wie den Zustand der geschachtelten Endpunkt in einem übergeordneten Profil?

Übergeordnete Profil ausführen Gesundheitskontrolle für das untergeordnete Element nicht direkt. Stattdessen wird der Zustand der untergeordneten Profil Endpunkte den Gesamtzustand der untergeordneten Profil berechnet Diese Informationen werden geschachtelte Profil Hierarchie bestimmt den Zustand der geschachtelten Endpunkt weitergegeben. Übergeordnete Profil verwendet diese zusammengesetzten Zustand um zu bestimmen, ob der Datenverkehr zum untergeordneten Element weitergeleitet werden kann.

Die folgende Tabelle beschreibt das Verhalten der Traffic Manager für geschachtelte Endpunkt Gesundheitskontrollen.

|Profil-Monitor untergeordneten status|Übergeordnete Endpunkt Monitor status|Notizen|
|---|---|---|
|Deaktiviert. Das kinderprofil wurde deaktiviert.|Beendet|Der übergeordnete Endpunktstatus angehalten nicht deaktiviert. -Status vorbehalten, dass den Endpunkt im übergeordneten Profil deaktiviert haben.|
|Beeinträchtigt. Mindestens ein untergeordnetes Element Profil Endpunkt ist Status Degraded.| Online: die Anzahl der Online-Endpunkte im untergeordneten Profil ist mindestens den Wert MinChildEndpoints.<BR>CheckingEndpoint: die Anzahl der Online plus CheckingEndpoint Endpunkte im untergeordneten Profil ist mindestens der Wert MinChildEndpoints.<BR>Beschädigt: andernfalls.|Datenverkehr wird an einen Endpunkt der Status CheckingEndpoint weitergeleitet. Wenn MinChildEndpoints zu hoch festgelegt ist, wird der Endpunkt immer beeinträchtigt.|
|Online. Mindestens ein untergeordnetes Element Profil Endpunkt ist ein Online-Status. Kein Endpunkt befindet sich im Status Degraded.|Siehe oben.||
|CheckingEndpoints. Mindestens ein untergeordnetes Element Profil Endpunkt ist 'CheckingEndpoint'. Werden keine Endpunkte 'Online' oder 'Heruntergestuft'|Dasselbe wie oben.||
|Inaktiv. Alle untergeordneten Profil Endpunkte sind deaktiviert oder angehalten oder dieses Profil hat keine Endpunkte.|Beendet||


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die [Funktionsweise von Traffic Manager](traffic-manager-how-traffic-manager-works.md)

Informationen zum [Erstellen eines Profils Traffic Manager](traffic-manager-manage-profiles.md)

<!--Image references-->
[1]: ./media/traffic-manager-nested-profiles/figure-1.png
[2]: ./media/traffic-manager-nested-profiles/figure-2.png
[3]: ./media/traffic-manager-nested-profiles/figure-3.png
[4]: ./media/traffic-manager-nested-profiles/figure-4.png
[5]: ./media/traffic-manager-nested-profiles/figure-5.png
[6]: ./media/traffic-manager-nested-profiles/figure-6.png
[7]: ./media/traffic-manager-nested-profiles/figure-7.png
[8]: ./media/traffic-manager-nested-profiles/figure-8.png
[9]: ./media/traffic-manager-nested-profiles/figure-9.png
[10]: ./media/traffic-manager-nested-profiles/figure-10.png

