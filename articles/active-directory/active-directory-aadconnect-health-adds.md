
<properties
    pageTitle="Gesundheit in AD DS verbinden über Azure AD | Microsoft Azure"
    description="Dies ist der Azure AD Connect Health-Seite, die von AD DS überwachen erläutert."
    services="active-directory"
    documentationCenter=""
    authors="arluca"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="arluca"/>

# <a name="using-azure-ad-connect-health-with-ad-ds"></a>Gesundheit in AD DS verbinden über Azure AD
Die folgende Dokumentation ist spezifisch für Active Directory-Domänendienste mit Azure AD Connect Health monitoring. Die unterstützten Versionen von AD DS: Windows Server 2008 R2, Windows Server 2012 und Windows Server 2012 R2.

Weitere Informationen zu AD FS mit Azure AD Connect Health monitoring finden Sie unter [Verwendung von Azure AD Connect Health mit AD FS](active-directory-aadconnect-health-adfs.md). Finden Sie außerdem Informationen zur Überwachung von Azure AD Connect (synchronisiert) mit Azure AD verbinden [Mithilfe von Azure AD Connect Health für die Synchronisierung](active-directory-aadconnect-health-sync.md).

![Azure AD Connect Health für AD DS](./media/active-directory-aadconnect-health/aadconnect-health-adds-entry.png)

## <a name="alerts-for-azure-ad-connect-health-for-ad-ds"></a>Alarme für Azure AD Connect Health für AD DS
Die Alarme in Azure AD Connect Health für AD DS finden Sie einer Liste von aktiven und behobenen Warnungen Bezug auf Domänencontroller. Auswählen einer aktiven oder gelösten Warnung öffnet ein neues Blatt mit zusätzlichen Informationen und Schritte, und links zu unterstützenden Dokumentation. Jeden Benachrichtigungstyp können eine oder mehrere Instanzen dieser spezielle Alarm betroffen Domänencontroller entsprechen. Im unteren Bereich des alert Blades können Sie einen betroffenen Domänencontroller um zusätzliche Blade mit Details zu dieser Warnung doppelklicken.

In diesem Blatt können Sie e-Mail-Benachrichtigungen für Warnungen aktivieren und im Bereich ändern. Erweitern des Zeitraums können Sie vorherige aufgelöst Alerts anzeigen.

![Azure AD Connect Synchronisierungsfehler](./media/active-directory-aadconnect-health/aadconnect-health-adds-alerts.png)

## <a name="domain-controllers-dashboard"></a>Domäne-Controller Dashboard
Dieses Dashboard bietet eine topologische Ansicht Ihrer Umgebung betriebliche Kennzahlen und Zustand der einzelnen überwachten Domänencontroller. Die dargestellten Metriken Hilfe schnell identifizieren alle Domänencontroller, die weitere Untersuchung erfordern. Standardmäßig wird nur eine Teilmenge der Spalten angezeigt. Den gesamten Satz der verfügbaren Spalten finden Sie jedoch den Befehl Spalten doppelklicken. Auswählen der Spalten, die Sie am häufigsten kümmern aktiviert setzen Sie dieses Dashboard in einem einzigen und einfach den Zustand der AD DS-Umgebung anzeigen.

![Domänencontroller](./media/active-directory-aadconnect-health/aadconnect-health-adds-domainsandsites-dashboard.png)

Domänencontroller können durch die jeweilige Domäne oder Website, die zum Verständnis der Umgebung Topologie gruppiert werden. Schließlich Blade-Header doppelklicken, wird das Dashboard nutzen den verfügbaren Bildschirmplatz maximiert. Größere Ansicht ist hilfreich, wenn mehrere Spalten angezeigt werden.

## <a name="replication-status-dashboard"></a>Replikation Status Dashboard
Das Dashboard bietet eine Ansicht des Replikationsstatus und Replikationstopologie überwachten Domänencontroller. Status des letzten Replikationsversuchs ist sowie hilfreiche Dokumentation für alle Fehler aufgelistet, die gefunden wird. Domänencontroller mit einem Fehler mit einen neuen Blade Öffnen doppelklicken: Fehlerdetails, empfohlene Schritte und Links zur Problembehandlung in der Dokumentation.

![Replikationsstatus](./media/active-directory-aadconnect-health/aadconnect-health-adds-replication.png)

## <a name="monitoring"></a>Überwachung
Diese Funktion stellt grafisch Trends verschiedene Leistungsindikatoren, die von überwachten Domänencontrollern kontinuierlich gesammelt werden. Leistung eines Domänencontrollers kann problemlos für alle überwachten Domänencontroller in der Gesamtstruktur verglichen werden. Darüber hinaus können Sie verschiedene Leistungsindikatoren nebeneinander sehen die ist hilfreich bei der Behandlung von Problemen in Ihrer Umgebung.

![Überwachung](./media/active-directory-aadconnect-health/aadconnect-health-adds-monitoring.png)

Standardmäßig haben wir vier Leistungsindikatoren vorausgewählt; Allerdings können Sie andere durch Klicken auf den Filterbefehl und aktivieren bzw. deaktivieren alle gewünschten Leistungsindikatoren enthalten. Darüber hinaus können Sie einen Zähler Leistungsdiagramm öffnen ein neues Blatt mit Daten für jeden überwachten Domänencontroller doppelklicken.

## <a name="related-links"></a>Verwandte links

* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Azure AD Connect Health Agent-Installation](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health-Operationen](active-directory-aadconnect-health-operations.md)
* [Schließen Sie Gesundheit mit Azure AD mit AD FS](active-directory-aadconnect-health-adfs.md)
* [Verwenden von Azure AD Connect Health für Synchronisierung](active-directory-aadconnect-health-sync.md)
* [Azure AD Connect Health FAQ](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect Version Gesundheitsdaten](active-directory-aadconnect-health-version-history.md)
