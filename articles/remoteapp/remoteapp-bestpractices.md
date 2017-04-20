<properties
    pageTitle="Azure RemoteApp best Practices | Microsoft Azure"
    description="Best Practices für Konfiguration und Verwendung von Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="best-practices-for-configuring-and-using-azure-remoteapp"></a>Best Practices für Konfiguration und Verwendung von Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Die folgende Informationen helfen Sie konfigurieren und Azure RemoteApp produktiv.

## <a name="connectivity"></a>Konnektivität


- Verwenden Sie immer die neueste Clientversion. Ältere Clients möglicherweise Verbindungsprobleme und andere heruntergestuften führen. Aktivieren automatische Updates für Ihr Gerät wird sichergestellt, dass stets der aktuellste Client installiert wird.
- Verwenden Sie immer den stabile und zuverlässige Internetzugang zur Verfügung.  
- Verwenden Sie nur unterstützte Proxyverbindungen für optimale Leistung.  SOCKS-Proxy wird nicht unterstützt.

## <a name="applications"></a>Applikationen


- Speichern Sie und schließen Sie RemoteApp-Programme mit der Anwendung sind. Die Anwendung nicht schließen kann zu Datenverlust führen.
- Validieren Sie benutzerdefinierte Anwendung vor der Verwendung in Azure RemoteApp Dazu gehört sie eine Multi-Session-Plattform arbeiten und unnötige Ressourcen wie Arbeitsspeicher und CPU, die einem anderen Benutzer in derselben Auflistung blockieren möglicherweise nicht verarbeiten. Weitere Informationen download und [Application Compatibility Best Practices für Remotedesktopdienste](http://www.dabcc.com/resources/Application%20Compatibility%20Best%20Practices%20for%20Remote%20Desktop%20Services.pdf).

## <a name="configuration-and-management"></a>Konfiguration und management


- Stand die Vorlage Bilder, Software-Updates und andere wichtige Updates nach Bedarf installieren. Dadurch wie Azure RemoteApp Auto-Skalierung zu Ihrer Kapazität, jeweils korrigiert.  
- Stellen Sie sicher, dass die Active Directory-Verbunddienste (AD FS) Bereitstellung sicherer und zuverlässiger. Andernfalls können Clientauthentifizierungen fehlschlagen, verhindert den Zugriff auf Azure RemoteApp.
- Konfigurieren Sie Vorlage Bilder mit installierter Programme, Rollen oder Features so, dass sie statusfrei sind. Sie sollten nicht auf alle Instanzen virtueller Computer in einem RemoteApp-Dienst in einem permanenten Zustand verlassen.
    - Speichern Sie alle Benutzerdaten in Benutzerprofilen oder andere Speicherorte außerhalb der Dienst wie lokale Freigaben oder OneDrive-Datei.
    - Speicher für freigegebene Daten in Speicherorte außerhalb der Dienst wie lokale Freigaben oder OneDrive-Datei.
    - Konfigurieren einer systemweiten Einstellung das Vorlagenbild nicht auf einzelne virtuelle Maschinen in einem Dienst.
    - Deaktivieren Sie automatische Softwareupdates für veröffentlichte Anträge - stattdessen manuell auf das Vorlagenbild wenden Sie an und Testen Sie, bevor Sie aus der Vorlage bereitstellen.
