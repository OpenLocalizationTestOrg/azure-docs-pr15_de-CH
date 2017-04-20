<properties
   pageTitle="Öffentliche und Private DC/OS Agent ACS | Microsoft Azure"
   description="Funktionsweise von öffentlichen und privaten Mittel Pools mit einer Azure Container Service."
   services="container-service"
   documentationCenter=""
   authors="Thraka"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Andockfenster Container Micro-Services Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="timlt"/>

# <a name="dcos-agent-pools-for-azure-container-service"></a>DC-OS Agent Pools für Azure Containerservice

DC-OS Azure Container Service teilt Agenten in öffentlichen oder privaten Pools. Eine Bereitstellung kann entweder Pool die Eingabehilfen zwischen Computern in Ihrem containerservice erfolgen. Computer ans Internet (öffentlich) oder internen (privaten) gespeichert. Dieser Artikel gibt einen Überblick gibt es ein öffentlicher und einen privater Pool.

### <a name="private-agents"></a>Private agents

Private Agent Knoten über ein nicht routbares Netzwerk ausgeführt werden. Dieses Netzwerk ist nur über die Admin-Zone oder öffentliche Zone Edge-Router verfügbar. Standardmäßig startet DC-OS apps auf private Agent-Knoten. Weitere Informationen über Sicherheit finden Sie in den [DC-Betriebssystem-Dokumentation](https://dcos.io/docs/1.7/administration/securing-your-cluster/) .

### <a name="public-agents"></a>Öffentliche Mittel

Öffentliche Mittel Knoten DC-OS apps und Services in einem öffentlich zugänglichen Netzwerk ausführen. Weitere Informationen über Sicherheit finden Sie in den [DC-Betriebssystem-Dokumentation](https://dcos.io/docs/1.7/administration/securing-your-cluster/) .

## <a name="using-agent-pools"></a>Agent-Pools verwenden

Standardmäßig setzt **Marathon** jeder neue Anwendung *private* Agent-Knoten. Sie müssen explizit die Anwendung *öffentlichen* Knoten während der Erstellung der Anwendung bereitstellen. **Optional** die Registerkarte, und geben Sie **Slave_public** als Wert **Akzeptiert Ressource Rollen** . Dieser Prozess dokumentiert [hier](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) und in der Dokumentation zu [DC\OS](https://dcos.io/docs/1.7/administration/installing/custom/create-public-agent/) .

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie weitere Informationen zum [Verwalten der DC-OS-Container](container-service-mesos-marathon-ui.md).

Informationen zum [Öffnen des Firewalls](container-service-enable-public-access.md) von Azure öffentlichen Zugriff auf den DC-OS-Container bereitgestellt.