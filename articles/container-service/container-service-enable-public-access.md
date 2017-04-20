<properties
   pageTitle="Öffentlicher Zugriff ACS App | Microsoft Azure"
   description="Zum öffentlichen Zugriff auf Azure Container Service aktivieren."
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
   ms.date="08/26/2016"
   ms.author="timlt"/>

# <a name="enable-public-access-to-an-azure-container-service-application"></a>Öffentlicher Zugriff auf eine Anwendung Azure Container Service

Jedem Container DC-OS ACS [öffentliche Mittel Pool](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) wird automatisch mit dem Internet verbunden. Standardmäßige Ports **80**, **443** **8080** geöffnet und alle (öffentlicher) Container auf diese Ports zugegriffen werden. Dieser Artikel veranschaulicht mehrere Anschlüsse für Ihre Anwendung in Azure Container Service öffnen.

## <a name="open-a-port-portal"></a>Öffnen Sie einen Port (Portal) 

Zunächst müssen wir wollen wir den Port öffnen.

1. Melden Sie sich im Portal.
2. Suchen der Ressourcengruppe Azure Container Service, bereitgestellt.
3. Wählen Sie Agent-Lastenausgleich (benannt wie **XXXX-Agent-lb-XXXX**).

    ![Azure Container Service-Lastenausgleich](media/container-service-dcos-agents/agent-load-balancer.png)

4. Klicken Sie auf **Überprüfen** und dann auf **Hinzufügen**.

    ![Prüfpunkte Azure Container Service-Lastenausgleich](media/container-service-dcos-agents/add-probe.png)

5. Füllen Sie Prüfpunkt aus, und klicken Sie auf **OK**.

  	| Feld | Beschreibung |
  	| ----- | ----------- |
  	| Name  | Ein beschreibender Name des Prüfpunkts. |
  	| Anschluss  | Der Port des Containers zu testen. |
  	| Pfad  | (Wenn im HTTP-Modus) Der Pfad relativ Website gesucht. HTTPS nicht unterstützt. |
  	| Intervall | Die Zeitspanne zwischen Prüfpunkt versucht in Sekunden. |
  	| Fehlerhafte Schwellenwert | Anzahl der aufeinander folgenden Prüfpunkt versucht, bevor Container fehlerhaft in Betracht ziehen. | 
    

6. Klicken Sie in den Eigenschaften des Agenten zum Lastenausgleich auf **Laden Lastenausgleich Regeln** **Hinzufügen**und dann auf.

    ![Azure Container Service Load Balancer Regeln](media/container-service-dcos-agents/add-balancer-rule.png)

7. Load Balancer Formular füllen Sie aus, und klicken Sie auf **OK**.

  	| Feld | Beschreibung |
  	| ----- | ----------- |
  	| Name  | Ein beschreibender Name der Lastenausgleich. |
  	| Anschluss  | Der öffentliche eingehenden Port. |
  	| Back-End-port | Die interne öffentlicher Port des Containers Datenverkehr weiterleiten. |
  	| Back-End-pool | Der Container in diesem Pool werden das Ziel für dieses System zum Lastenausgleich. |
  	| Prüfpunkt | Ermitteln, ob ein Ziel im **Back-End-Pool** Prüfpunkt ist fehlerfrei. |
  	| Sitzung Dauerhaftigkeit | Bestimmt, wie Datenverkehr von einem Client für die Dauer der Sitzung behandelt werden soll.<br><br>**None**: aufeinander folgende Anfragen desselben Clients können von einem beliebigen Container behandelt.<br>**Client-IP**: aufeinander folgende Anfragen vom gleichen Client-IP erfolgt mit demselben Container.<br>**Client-IP und Protokoll**: aufeinander folgende Anfragen die gleiche Kombination von Client-IP- und Protokoll erfolgt mit demselben Container. |
  	| Leerlaufzeitlimit | (Nur TCP) In Minuten, die TCP/HTTP-Client zu öffnen ohne *Keepalive -* Nachrichten. |

## <a name="add-a-security-rule-portal"></a>Hinzufügen einer Regel (Portal)

Als Nächstes müssen wir eine Regel hinzufügen, die Datenverkehr von unserer geöffnete Ports über die Firewall weiterleitet.

1. Melden Sie sich im Portal.
2. Suchen der Ressourcengruppe Azure Container Service, bereitgestellt.
3. Wählen Sie die **Öffentliche** Agent Network Security (benannt wie **XXXX-Agent-Public-Nsg-XXXX**).

    ![Azure Container Service Netzwerk-Sicherheitsgruppe](media/container-service-dcos-agents/agent-nsg.png)

4. Wählen Sie **eingehende Sicherheitsregeln** und **Hinzufügen**.

    ![Azure Container Service Netzwerksicherheitsregeln Gruppe](media/container-service-dcos-agents/add-firewall-rule.png)

5. Füllen Sie die Firewallregel öffentlichen Port und klicken Sie auf **OK**.

  	| Feld | Beschreibung |
  	| ----- | ----------- |
  	| Name  | Ein beschreibender Name der Firewallregel. |
  	| Priorität | Der Prioritätsrang für die Regel. Je niedriger die Zahl desto höher die Priorität. |
  	| Quelle | Beschränken Sie eingehenden IP-Adressbereich zum gewährt oder verweigert die von dieser Regel. Verwenden Sie **** Einschränkung nicht an. |
  	| Dienst | Wählen Sie eine vordefinierte Dienste, denen dieser Regel ist. Erstellen einer eigenen verwenden Sie **benutzerdefinierten** andernfalls. |
  	| Protokoll | Grundlage der **TCP-** oder **UDP-**Datenverkehr einschränken. Verwenden Sie **** Einschränkung nicht an. |
  	| Port-Bereich | Beim ** **Dienst** ist,**gibt die Ports, die diese Regel betrifft. Sie können einen einzelnen Port **80**oder einen Bereich wie **1024 1500**. |
  	| Aktion | Gewähren oder Verweigern von Datenverkehr, der die Kriterien erfüllt. |

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über den Unterschied zwischen [öffentlichen und privaten DC/OS-Agenten](container-service-dcos-agents.md).

Lesen Sie weitere Informationen zum [Verwalten der DC-OS-Container](container-service-mesos-marathon-ui.md).