<properties
   pageTitle="Pfad-basierte Regeln für ein Gateway mit dem Portal erstellen | Microsoft Azure"
   description="Erstellen Sie Pfad-basierte Regel für ein Gateway mithilfe der Portalwebsite"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-a-path-based-rule-for-an-application-gateway-by-using-the-portal"></a>Erstellen Sie Pfad-basierte Regeln für ein Gateway mit dem portal

> [AZURE.SELECTOR]
- [Azure-portal](application-gateway-create-url-route-portal.md)
- [Azure Ressourcenmanager PowerShell](application-gateway-create-url-route-arm-ps.md)

URL-Pfad-basierte routing können Sie basierte auf den URL-Pfad der HTTP-Anforderung Routen zuordnen. Es überprüft, ob eine Route zum Back-End-Pool für die URL-Listen in Application Gateway konfiguriert und definierten Back-End-Pool den Netzwerkverkehr an. Ein URL-basiertem routing wird Saldo Anfragen für verschiedene Inhaltstypen zu anderen Back-End-Server geladen.

URL-basierte routing stellt einen neuen Typ Application Gateway. Application Gateway hat zwei Regeltypen: grundlegende und Pfad basierende Regeln. Grundlegende Regeltyp bietet Round Robin für Back-End-Pools beim Pfad Regeln neben Round-Robin-Verteilung, berücksichtigt Pfad Muster des angeforderten URL beim Back-End-Pool auswählen.

## <a name="scenario"></a>Szenario

Das folgende Szenario durchläuft in ein vorhandenes Gateway Pfad-basierte Regel erstellen.
Das Szenario wird davon ausgegangen, dass Sie bereits die Schritte [Ein Gateway](application-gateway-create-gateway-portal.md)erstellt haben.

![URL-route][scenario]

## <a name="createrule"></a>Die Pfad-basierte Regel erstellen

Pfad-basierte Regel erfordert eigene Listener, vor dem Erstellen der Regel überprüfen Sie verfügbaren Listener verwenden.

### <a name="step-1"></a>Schritt 1

Navigieren Sie zu http://portal.azure.com, und wählen Sie ein vorhandenes Gateway. Klicken Sie auf **Regeln**

![Gateway Anwendungsübersicht][1]

### <a name="step-2"></a>Schritt 2

Klicken Sie **Pfad-basierte** zum Hinzufügen einer neuen Regel Pfad basieren.

### <a name="step-3"></a>Schritt 3

Das **Pfad-basierte Regel hinzufügen** -Blade verfügt über zwei Abschnitte. Im erste Abschnitt ist, in dem Sie den Listener, den Namen der Regel und die Standardeinstellungen Pfad definiert. Der Pfad sind Routen, die nicht unter die benutzerdefinierten Pfad-basierte Route. Der zweite Abschnitt des **Pfad-basierte Regel hinzufügen** Blade ist definieren die Pfad Regeln selbst.

**Standardeinstellungen**

- **Name** – Dies ist ein Anzeigename für die Regel, die im Portal verfügbar ist.
- **Listener** - Dies ist der Listener, die für die Regel verwendet wird.
- **Standard-Back-End-Pool** - diese Einstellung wird die Einstellung, die Back-End für die Standardregel zu verwendende definiert
- **Standard-HTTP-Einstellungen** - diese Einstellung ist die Einstellung, die HTTP-Einstellungen für die Standardregel definiert.

**Pfad-Regeln**

- **Name** – Dies ist ein angezeigter Name, Pfad-basierte Regel.
- **Pfade** - diese Einstellung definiert den Pfad, den die Regel beim Weiterleiten von Datenverkehr sucht
- **Back-End-Pool** - diese Einstellung wird die Einstellung, die Back-End für die Regel zu verwendende definiert
- **Http-Einstellung** - diese Einstellung ist, die HTTP-Einstellungen für die Regel definiert.

>[AZURE.IMPORTANT] Pfade: Die Liste der Pfad Muster übereinstimmen. Jede mit beginnen / und nur einen "\*" darf am Ende. Gültige sind Werte /xyz, /xyz* oder /xyz/*.  

![Fügen Sie Pfad-basierte Blatt Informationen ausgefüllt][2]

Hinzufügen einer Regel Pfad basiert auf einer vorhandenen Anwendungsgateway ist ein einfacher Prozess über das Portal. Erstellte Regel Pfad basieren können bearbeitet werden, fügen Sie zusätzliche Regeln. 

![Hinzufügen von zusätzlichen Pfad Regeln][3]

## <a name="next-steps"></a>Nächste Schritte

So konfigurieren Sie SSL-Abladung Azure Application Gateway finden Sie [Konfigurieren SSL-Verschiebung](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-url-route-portal/figure1.png
[2]: ./media/application-gateway-create-url-route-portal/figure2.png
[3]: ./media/application-gateway-create-url-route-portal/figure3.png
[scenario]: ./media/application-gateway-create-url-route-portal/scenario.png