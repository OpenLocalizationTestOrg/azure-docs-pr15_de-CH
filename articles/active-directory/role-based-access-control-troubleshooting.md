<properties
    pageTitle="Rollenbasierte Zugriff Steuerelement Fehlerbehebung | Microsoft Azure"
    description="Hilfe bei Problemen oder Fragen zur Rolle Based Access Control-Ressourcen."
    services="azure-portal"
    documentationCenter="na"
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="kgremban"/>

# <a name="role-based-access-control-troubleshooting"></a>Rollenbasierte Zugriffskontrolle Problembehandlung

## <a name="introduction"></a>Einführung

[Role-Based Access Control](role-based-access-control-configure.md) ist ein leistungsfähiges Feature, das differenzierten Zugriff auf Ressourcen in Azure delegieren können. Dies bedeutet, dass fühlen Vertrauen gewähren einer bestimmten Person an, was sie brauchen, und mehr. Allerdings manchmal das Ressourcenmodell zum Azure-Ressourcen kann kompliziert sein und kann schwer verständlich, genau wie Sie Berechtigungen erteilen.

Dieses Dokument können Sie wissen, was Sie erwartet, wenn die Rollen in der Azure-Portal verwenden. Diese drei Funktionen umfassen alle Ressourcentypen:

- Besitzer  
- Teilnehmer  
- Reader  

Eigentümer und Beitragende haben vollen Zugriff auf die Verwaltungsfunktionen Mitwirkender Zugriff erteilen kann anderen Benutzern oder Gruppen Interessanter etwas mit der Leser-Rolle ist, wo wir einige Zeit verbringen. Finden Sie die [Rollenbasierte Zugriffskontrolle Artikel erste Schritte](role-based-access-control-configure.md) zum gewähren.

## <a name="app-service-workloads"></a>App Service Arbeitslasten

### <a name="write-access-capabilities"></a>Schreibzugriff-Funktionen

Gewährt einem Benutzer Lesezugriff zu einer einzigen Web app sind einige Funktionen deaktiviert, nicht erwarten. Die folgenden Verwaltungsfunktionen erfordern **Schreibzugriff zu Web-app (Beitragender oder Eigentümer)** und nicht in jedem Szenario schreibgeschützt zur Verfügung.

- Befehle (z. B. Start, Stop, usw.)
- Ändern von Einstellungen wie allgemeine Konfiguration, Einstellungen, Sicherungsdateien und Überwachung settings
- Zugriff auf Veröffentlichung Anmeldeinformationen und anderen vertraulichen app-Einstellungen wie Verbindungszeichenfolgen
- Streaming-Protokolle
- Konfiguration von Diagnoseprotokollen
- Konsole (Befehlszeile)
- Aktive und aktuelle Bereitstellung (für lokale Git kontinuierliche Bereitstellung)
- Geschätzte Ausgaben
- Webtests
- Virtuelles Netzwerk (nur sichtbar für einen Leser, wenn ein virtuelles Netzwerk zuvor von einem Benutzer mit Schreibzugriff konfiguriert wurde).

Wenn Sie diese Kacheln zugreifen können, müssen Sie Ihr Systemadministrator Teilnehmer Zugriff auf Web-app.

### <a name="dealing-with-related-resources"></a>Mit Verwandte Ressourcen

Webapps werden durch einige andere Ressourcen erschwert, das Zusammenspiel. Hier ist eine normale Ressourcengruppe mit wenigen Websites:

![Web app Ressourcengruppe](./media/role-based-access-control-troubleshooting/website-resource-model.png)

Als Ergebnis, wenn jemand gewähren Zugriff auf genau die Web app viele Funktionen im Azure-Portal auf der Website deaktiviert.

Diese Elemente erfordern **Schreibzugriff **App Service-Plan** , die Ihre Website entspricht** :  

- Ebene (frei oder Standard) Preise des Web app anzeigen  
- Scale-Konfiguration (Anzahl der Instanzen, Größe des virtuellen Computers, skalieren Einstellungen)  
- Kontingente (Speicher, Bandbreite und CPU)  

Diese Elemente erfordern **Schreibzugriff auf die gesamte **Ressourcengruppe** , die Ihre Website enthält** :  

- SSL-Zertifikate und Bindungen (Dies ist da SSL-Zertifikate zwischen Standorten in derselben Ressourcengruppe und Standort genutzt werden können)  
- Warnregeln  
- Skalierungsgröße settings  
- Anwendungskomponenten Einblicke  
- Webtests  

## <a name="virtual-machine-workloads"></a>Virtual Machine-Arbeitslasten

Viel wie Web Apps einige Funktionen erfordern auf dem virtuellen Computer Zugriff auf den virtuellen Computer oder andere Ressourcen in der Ressourcengruppe.

Virtuelle Computer beziehen sich auf Domain-Namen, virtuelle Netzwerke, Speicherkonten und Warnregeln.

Diese Elemente erfordern **Schreibzugriff auf den **virtuellen Computer**** :

- Endpunkte  
- IP-Adressen  
- Datenträger  
- Extensions  

Diese benötigen Zugriff auf die **virtuellen Computer**und die **Ressourcengruppe** (zusammen mit der Domänenname), die in **Schreiben** :  

- Festlegen der Verfügbarkeit  
- Ausgewogene laden  
- Warnregeln  

Wenn Sie diese Kacheln zugreifen können, müssen Sie der Systemadministrator auf Teilnehmer der Ressourcengruppe.

## <a name="see-more"></a>Weitere
- [Rolle Based Access Control](role-based-access-control-configure.md): Erste Schritte mit RBAC in Azure-Portal.
- [Integrierte Rollen](role-based-access-built-in-roles.md): Details zu den Rollen, die standardmäßig im RBAC.
- [Benutzerdefinierte Rollen in Azure RBAC](role-based-access-control-custom-roles.md): benutzerdefinierte Rollen entsprechend Ihrer Bedürfnisse Zugriff erstellen.
- [Erstellen einer Access Änderungshistorie](role-based-access-control-access-change-history-report.md): Überblick ändern rollenzuweisungen RBAC.
