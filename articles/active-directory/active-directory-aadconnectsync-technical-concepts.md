<properties
    pageTitle="Azure AD Verbindung synchronisieren: technische Begriffe | Microsoft Azure"
    description="Erläutert die technischen Konzepte von Azure AD-Verbindung synchronisieren."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-technical-concepts"></a>Azure AD Verbindung synchronisieren: technische Begriffe
Dieser Artikel ist eine Zusammenfassung des Themas [Architektur verstehen](active-directory-aadconnectsync-technical-concepts.md).

Azure AD Connect Sync baut auf eine solide Metaverzeichnis Synchronisierung Plattform.
Die folgenden Abschnitte erläutern die Konzepte für das Metaverzeichnis Synchronisierung.
Aufbauend auf MIIS ILM und FIM, bietet Azure Active Directory Sync Services der nächsten Verbinden mit Datenquellen Synchronisieren von Daten zwischen Datenquellen sowie die Bereitstellung und Entfernung von Identitäten.

![Technische Begriffe](./media/active-directory-aadconnectsync-technical-concepts/scenario.png)

Die folgenden Abschnitte enthalten weitere Informationen zu den folgenden Aspekten der Synchronisierungsdienst FIM:

- Anschluss
- Attributfluss
- Connectorspace
- Metaverse
- Bereitstellung

## <a name="connector"></a>Anschluss

Zur Kommunikation mit einem verbundenen Verzeichnis Codemodule heißen Connectors (ehemals Verwaltungsagenten (MAs)).

Diese werden auf dem Computer Azure AD Connect Sync installiert.
Die Anschlüsse können ohne Agents über Remotesystem Protokolle anstatt auf die Bereitstellung von spezialisierten Agents unterhalten. Dies bedeutet geringere Risiken und die Bereitstellungszeit besonders wenn kritische Applikationen und Systeme.

In der obigen Abbildung der Connector steht im Connectorspace jedoch umfasst die gesamte Kommunikation mit dem externen System.

Der Connector ist verantwortlich für alle importieren und Exportfunktionen für das System und gibt Entwicklern zu verstehen, wie jedes System direkt herstellen, wenn Sie deklarative Bereitstellung Datentransformationen anpassen.

Importe und Exporte treten nur auf, wenn für weitere Isolierung von Änderungen im System, da ändert sich nicht automatisch auf der verbundenen Datenquelle übertragen werden. Darüber hinaus können Entwickler auch eigene Anschlüsse für praktisch alle Datenquellen erstellen.

## <a name="attribute-flow"></a>Attributfluss

Metaverse ist die konsolidierte Ansicht aller verknüpften Identitäten aus Connector Leerzeichen. In der obigen Abbildung zeigt Attributfluss Linien mit Pfeilspitzen für eingehenden und ausgehenden Verkehr. Attributfluss wird kopieren oder Transformieren von Daten von einem System und alle Attribut Abläufe (eingehend oder ausgehend).

Attributfluss tritt zwischen den Connectorspace und das Metaverse bidirektional Synchronisierungsvorgänge (vollständig oder Delta) geplant ist.

Attributfluss tritt nur auf, wenn diese Synchronisationen ausgeführt werden. Attribut Abläufe werden in Regeln definiert. Eingehend (ISR in der obigen Abbildung) oder ausgehend (OSR in der obigen Abbildung) können.

## <a name="connected-system"></a>Verbundenes system

Verbundenen System (aka verbundenen Verzeichnis) ist verweisen auf dem Remotesystem Azure AD Connect Sync verbundenen lesen und Schreiben von Daten in und aus.

## <a name="connector-space"></a>Connectorspace

Jeder verbundenen Datenquelle wird als eine gefilterte Teilmenge der Objekte und Attribute im Connectorspace dargestellt.
Synchronisierungsdienst lokal ohne remote-System an, wenn die Objekte synchronisiert Betrieb ermöglicht Interaktion Einfuhren beschränkt und nur exportiert.

Bei der Datenquelle und der Connector die Möglichkeit, eine Liste ändert (deltaimport) dann erhöht Effizienz erheblich nur geändert seit dem letzten Abfragezyklus ausgetauscht. Connectorspace isoliert die verbundene Datenquelle ändert automatisch übertragen, indem gefordert wird, dass der Connector importiert und exportiert. Diese zusätzlichen Versicherung gewährt beruhigt testen, Vorschau oder das nächste Update bestätigen.

## <a name="metaverse"></a>Metaverse

Metaverse ist die konsolidierte Ansicht aller verknüpften Identitäten aus Connector Leerzeichen.

Identitäten sind miteinander verknüpft und Autorität für verschiedene Attribute durch Import Fluss Mappings zugeordnet ist, beginnt das zentralen Metaverse-Objekt Informationen aus mehreren Systemen. Dieses Attribut Objektfluss tragen Mappings Informationen zu ausgehenden Systemen.

Objekte werden erstellt, wenn ein autorisierende System in Metaverse projiziert. Sobald alle Verbindungen entfernt werden, wird das Metaverse-Objekt gelöscht.

Objekte im Metaverse können nicht direkt bearbeitet werden. Alle Daten im Objekt müssen über Attributfluss beigetragen. Metaverse verwaltet dauerhafte Connectors mit jeder Connector. Diese Connectors erfordern keine erneute für jede Synchronisation ausführen. Dies bedeutet, dass Azure AD verbinden synchronisieren nicht übereinstimmende Remoteobjekt jedem suchen. Dies vermeidet kostspielige Agents ändern Attribute zu verhindern, die normalerweise Objekte korreliert werden.

Neue Datenquellen zu entdecken, die vorhandenen Objekte verfügen, die verwaltet werden müssen, werden mithilfe Azure AD Connect Sync sogenannten Join Regel Kandidaten, verbinden ausgewertet.
Sobald die Verbindung dieser Bewertung erfolgt und normalen Attributfluss zwischen einer verbundenen Datenquelle und Metaverse auftreten kann.

## <a name="provisioning"></a>Bereitstellung

Wenn eine autorisierende Quellprojekte kann ein neues Objekt in das Metaversum eine neue Connectorspace-Objekt in einen anderen Anschluss eine nachgelagerten verbundenen Datenquelle darstellt erstellt.

Dies stellt grundsätzlich eine Verknüpfung her kann, und Attributfluss bidirektional.

Wenn eine Regel fest, dass ein neue Connectorspace-Objekt erstellt werden muss, wird provisioning genannt werden. Da dieser Vorgang nur im Connectorspace erfolgt, ist es nicht in der verbundenen Datenquelle übertragen bis Export durchgeführt wird.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Azure AD Verbindung synchronisieren: Anpassen von Synchronisierungsoptionen](active-directory-aadconnectsync-whatis.md)
* [Integration von lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)

<!--Image references-->
[1]: ./media/active-directory-aadsync-technical-concepts/ic750598.png
