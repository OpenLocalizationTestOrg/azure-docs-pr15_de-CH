<properties
   pageTitle="Solutions Operations Management Suite (OMS) | Microsoft Azure"
   description="Solutions erweitern die Funktionen von Operations Management Suite (OMS) mit verpackten Szenarien, die Kunden den OMS-Arbeitsbereich hinzufügen können.  Dieser Artikel enthält Informationen wie benutzerdefinierte Lösungen von Kunden und Partnern."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="bwren" />

# <a name="management-solutions-in-operations-management-suite-oms-preview"></a>Management Solutions in Operations Management Suite (OMS) (Vorschau)

>[AZURE.NOTE]Dies ist eine vorläufige Dokumentation für Informationsmanagement in OMS die derzeit in der Vorschau.    

Lösungen erweitern die Funktionen von Operations Management Suite (OMS) mit verpackten Szenarien, die Kunden ihre Umgebung hinzufügen können.  Neben [Solutions bereitgestellt von Microsoft](../log-analytics/log-analytics-add-solutions.md)können Partnern und Kunden Lösungen in ihrer Umgebung verwendet werden oder Kunden durch die Gemeinschaft.

## <a name="finding-and-installing-management-solutions"></a>Suchen und Installieren von Management-Lösung
Es gibt mehrere Methoden zum Suchen und installieren-Management-Lösungen, wie in den folgenden Abschnitten beschrieben.

### <a name="azure-marketplace"></a>Azure Marketplace
Management-Lösung von Microsoft bereitgestellt und Partnern von Azure Marketplace in Azure-Portal installiert werden.

1. Melden Sie sich bei Azure-Portal.
2. Wählen Sie im linken Bereich **Weitere Dienste**.
3. Scrollen Sie **Projektmappen** oder Dialogfeld **Filter** Geben Sie *Solutions ein* .
4. Klicken Sie auf die Schaltfläche **+ Hinzufügen** .
5. Suchen Sie nach Lösungen, die Sie durchsuchen, klicken auf **Filter** oder Eingabe im Feld **Suchen alles** interessiert sind.
6. Klicken Sie auf ein Element Marketplace, um detaillierte Informationen anzuzeigen.
4. Klicken Sie auf **Erstellen** , um das **Solution hinzufügen** zu öffnen.
5. Sie werden aufgefordert, erforderliche Informationen wie [OMS-Arbeitsbereich und automatisierungskonto](#oms-workspace-and-automation-account) zusätzlich Werte für alle Parameter in der Projektmappe.
6. Klicken Sie auf **Erstellen** , um die Projektmappe zu installieren.

### <a name="oms-portal"></a>OMS-Portal
Von Microsoft bereitgestellte Lösungen können von Lösungskatalog OMS-Portal installiert werden.

1. Auf der OMS-Portal anmelden.
2. Klicken Sie auf die Kachel **Lösungskatalog** .
2. Auf der Seite OMS Lösungskatalog enthält Informationen Sie zu jeder Lösung. Klicken Sie auf den Namen der Projektmappe, die Sie in OMS hinzufügen möchten.
3. Detaillierte Informationen zu der Lösung wird auf der Seite für die gewählte Lösung angezeigt. Klicken Sie auf **Hinzufügen**.
4. Eine neue Kachel für die Lösung angezeigt auf der Seite OMS und Sie wieder verwenden kann hinzugefügt, nachdem der OMS-Dienst Daten verarbeitet.

### <a name="azure-quickstart-templates"></a>Azure Schnellstart-Vorlagen
Mitglieder der Community können Lösungen Azure Schnellstart Vorlagen senden.  Diese Vorlagen für eine spätere Installation oder Überprüfen Sie diese Informationen zum [Erstellen eigener Projektmappen](#creating-a-solution).

1. Folgen Sie in [OMS-Arbeitsbereich und automatisierungskonto](#oms-workspace-and-automation-account) einen Arbeitsbereich und Konto beschrieben.
2. Gehen Sie zu [Azure Schnellstart-Vorlagen](https://azure.microsoft.com/documentation/templates/).  
3. Suchen Sie nach einer Lösung, der Sie interessieren.
4. Wählen Sie die Lösung aus, dessen Details anzuzeigen.
5. Klicken Sie **in Azure bereitstellen** .
6. Sie werden aufgefordert, die Informationen wie die Ressourcengruppe Speicherort sowie Werte für alle Parameter in der Projektmappe.
7. Klicken Sie auf **Einkauf** um die Projektmappe zu installieren.

### <a name="deploy-azure-resource-manager-template"></a>Azure-Ressourcen-Manager-Vorlage bereitstellen
Solutions, von der Community erhalten oder als Ressourcen-Manager-Vorlage implementiert sind, Sie [selbst erstellen](#creating-a-solution) , können Sie eine der Standardmethoden [Bereitstellen einer Vorlage](../resource-group-template-deploy-portal.md).  Beachten Sie, dass vor der Installation der Lösung müssen Sie erstellen und Verknüpfen von [OMS-Arbeitsbereich und Automatisierung](#oms-workspace-and-automation-account).

## <a name="oms-workspace-and-automation-account"></a>OMS-Arbeitsbereich und Automation-Konto
Die meisten Management Solutions erfordern eine [OMS Arbeitsbereich](../log-analytics/log-analytics-manage-access.md) Ansichten enthalten und ein [Konto Automatisierung](../automation/automation-security-overview.md#automation-account-overview) Runbooks und zugehörige Ressourcen enthalten. Arbeitsbereich und Konto müssen folgenden Vorschriften entsprechen.

- Eine Lösung können nur ein OMS-Arbeitsbereich und eine automatisierungskonto.  
- OMS-Arbeitsbereich und automatisierungskonto einer Lösung müssen miteinander verknüpft werden. OMS-Arbeitsbereich kann nur ein Automation-Konto verknüpft und Automation-Konto kann nur einem OMS-Arbeitsbereich verknüpft werden.
- Verbunden werden, müssen OMS-Arbeitsbereich und automatisierungskonto in derselben Ressourcengruppe und Region sein.  Die Ausnahme ist ein OMS Arbeitsbereich im südostasiatischen USA und und Automatisierung im südostasiatischen USA 2.

### <a name="creating-a-link-between-an-oms-workspace-and-automation-account"></a>Erstellen einer Verknüpfung zwischen OMS-Arbeitsbereich und Automation-Konto
Wie Geben Sie OMS-Arbeitsbereich und automatisierungskonto hängt die Installationsmethode für die Projektmappe.

- Bei der Installation von Microsoft-Lösung über das Portal OMS OMS-Arbeitsbereichs installiert ist und keine Automation-Konto ist erforderlich.

- Bei der Installation einer Lösung über Azure Marketplace werden für einen OMS-Arbeitsbereich und Automation-Konto, und die Verknüpfung erstellt.  

- Für Lösungen außerhalb der Azure Marketplace müssen Sie vor der Installation der Lösung OMS Arbeitsbereich und Automation-Konto verknüpfen.  Hierzu können Sie eine Lösung auf dem Markt Azure auswählen und OMS-Arbeitsbereich und Automation-Konto auswählen.  Sie müssen die Projektmappe tatsächlich zu installieren, da der Link erstellt wird, wie die OMS-Arbeitsbereich und Automation-Konto ausgewählt.  Nachdem die Verknüpfung erstellt wurde, können Sie, OMS-Arbeitsbereich und automatisierungskonto für jede Lösung verwenden. 

### <a name="verifying-the-link-between-an-oms-workspace-and-automation-account"></a>Überprüfen die Verknüpfung zwischen OMS-Arbeitsbereich und Automation-Konto
Sie können die Verknüpfung zwischen einem OMS-Arbeitsbereich und Automation-Konto mithilfe des folgenden Verfahrens überprüfen.

1. Wählen Sie das Konto Automatisierung im Azure-Portal.
2. Scrollen Sie nach unten **Einstellungsbereich** .
3. Ist ein Abschnitt **OMS-Ressourcen** im Bereich **Einstellungen** , ist dieses Konto ein OMS-Arbeitsbereich zugeordnet.
4. Wählen Sie **Arbeitsbereich** auf die Details des Arbeitsbereichs OMS Automation-Konto verknüpft.


## <a name="listing-management-solutions"></a>Angebot Management solutions
Gehen Sie zu Management Solutions in Azure-Abonnement verknüpft Arbeitsbereiche anzeigen.

1. Melden Sie sich bei Azure-Portal.
2. Wählen Sie im linken Bereich **Weitere Dienste**.
3. Scrollen Sie **Projektmappen** oder Dialogfeld **Filter** Geben Sie *Solutions ein* .
4. Lösungen in Ihren Arbeitsbereichen werden aufgelistet.

Beachten Sie, dass nur die im aktuellen Arbeitsbereich OMS-Portal installiert Microsoft Solutions anzeigen können.

## <a name="removing-a-management-solution"></a>Entfernen einer Lösung
Beim Entfernen einer Lösung werden auch alle Ressourcen in der Projektmappe entfernt.  

1. Suchen Sie die Projektmappe im Azure-Portal mit der Prozedur [Solutions](#listing-solutions)auflisten.
2. Wählen Sie die Lösung, die Sie entfernen möchten.
3. Klicken Sie auf die Schaltfläche **Löschen** .

## <a name="creating-a-management-solution"></a>Erstellen einer Lösung
Vollständige Anleitung zum Erstellen von Lösungen finden Sie unter [Creating Solutions in Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md). 


## <a name="next-steps"></a>Nächste Schritte

- Suche [Azure Schnellstart Vorlagen](https://azure.microsoft.com/documentation/templates) Beispiele für verschiedene Ressourcen-Manager-Vorlagen.
- Erstellen Sie eigener [Lösungen](operations-management-suite-solutions-creating.md).
 