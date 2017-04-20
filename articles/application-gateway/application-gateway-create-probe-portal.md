<properties
   pageTitle="Erstellen Sie eine benutzerdefinierte Überprüfung für ein Gateway mit dem Portal | Microsoft Azure"
   description="Erstellen Sie benutzerdefinierte Überprüfung für Application Gateway mithilfe der Portalwebsite"
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

# <a name="create-a-custom-probe-for-application-gateway-by-using-the-portal"></a>Erstellen Sie eine benutzerdefinierte Überprüfung für Application Gateway mithilfe der Portalwebsite

> [AZURE.SELECTOR]
- [Azure-portal](application-gateway-create-probe-portal.md)
- [Azure Ressourcenmanager PowerShell](application-gateway-create-probe-ps.md)
- [Klassische Azure PowerShell](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

## <a name="scenario"></a>Szenario

Das folgende Szenario durchläuft ein Gateway der vorhandenen benutzerdefinierten Integritätstest erstellen.
Das Szenario wird davon ausgegangen, dass Sie bereits die Schritte [Ein Gateway](application-gateway-create-gateway-portal.md)erstellt haben.

## <a name="createprobe"></a>Den Prüfpunkt erstellen

Prüfpunkte werden in zwei Schritten über das Portal konfiguriert. Der erste Schritt zum Erstellen des Prüfpunkts ist, fügen Sie den Prüfpunkt auf die Back-End-HTTP-Anwendungsgateway.

### <a name="step-1"></a>Schritt 1

Navigieren Sie zu http://portal.azure.com, und wählen Sie ein vorhandenes Gateway.

![Gateway Anwendungsübersicht][1]

### <a name="step-2"></a>Schritt 2

**Prüfpunkte** auf, und klicken Sie auf **Hinzufügen** , um eine neue Probe hinzufügen.

![Fügen Sie Prüfpunkt Blade Informationen ausgefüllt hinzu][2]

### <a name="step-3"></a>Schritt 3

Füllen Sie die erforderlichen Informationen für den Prüfpunkt aus, und klicken Sie auf **OK**.

- **Name** – Dies ist ein angezeigter Name für den Prüfpunkt im Portal zugegriffen werden kann.
- **Host** - Dies ist der Hostname, der für den Prüfpunkt verwendet wird. Für ist nur, wenn mehrere Standorte auf Application Gateway, andernfalls verwenden "127.0.0.1". Dies unterscheidet sich von VM-Hostnamen.
- **Pfad** – den Rest der vollständige Url für den benutzerdefinierten Prüfpunkt. Ein gültiger Pfad beginnt mit '/'
- **Intervall (Sekunden)** - wie oft der Prüfpunkt ausgeführt wird, Überprüfen des Zustands. Wird nicht empfohlen je niedriger als 30 Sekunden.
- **Zeitlimit (Sek.)** - die Zeitdauer, die der Prüfpunkt Timeout wartet. Das Timeoutintervall muss hoch genug ist, ein HTTP-Aufruf erfolgen kann, um sicherzustellen, dass die Back-End-Seite verfügbar ist.
- **Fehlerhafte Schwelle** - Anzahl fehlgeschlagener Versuche fehlerhaft betrachtet. Ein Schwellenwert von 0 bedeutet, dass wenn ein Health Check fehlschlägt Back-End fehlerhafte sofort bestimmt werden.

> [AZURE.IMPORTANT] der Host-Name ist nicht der Servername. Dies ist der Name des virtuellen Hosts auf dem Anwendungsserver ausgeführt. Der Prüfpunkt wird an Http://(host name):(port from httpsetting)/UrlPath gesendet.

![Prüfpunkt-Konfigurationen][3]

## <a name="add-probe-to-the-gateway"></a>Das Gateway Probe hinzufügen

Der Prüfpunkt erstellt wurde, ist es Zeit, das Gateway hinzufügen. Probe werden auf die Back-End-HTTP-Anwendungsgateway eingestellt.

### <a name="step-1"></a>Schritt 1

Klicken Sie auf **http-Einstellungen** von Application Gateway und klicken Sie dann auf den Back-End-HTTP-Einstellungen im Fenster Konfiguration Blatt zu.

![HTTPS-Einstellungen][4]

### <a name="step-2"></a>Schritt 2

Blatt Einstellungen **AppGatewayBackEndHttp** **verwenden benutzerdefinierte Probe** auf, und wählen Sie im Abschnitt [Erstellen der Prüfpunkt](#createprobe) erstellten Prüfpunkt.
Wenn Sie fertig sind, klicken Sie auf **OK** und die Einstellung angewendet werden.

![Appgatewaybackend Settings blade][5]

Der Standard-Prüfpunkt überprüft standardmäßig Zugriff auf die Anwendung. Benutzerdefinierte Probe erstellt wurde, verwendet das Anwendungsgateway den benutzerdefinierten Pfad definiert, um die Überwachung für das Backend ausgewählt. Basierend auf den Kriterien, die definiert wurde, prüft das Application Gateway Prüfpunkts angegebene Datei. Wenn der Aufruf von Host: Port / Pfad keine Statusantwort Http 200 OK zurück, des Servers wird aus Drehung nach der fehlerhafte Schwellenwert erreicht wird. Suche weiter fehlerhafte Instanzen, sobald wieder fehlerfrei ist. Nachdem die Instanz wieder hinzugefügt zum fehlerfreien Serverpool Datenverkehr beginnt es wieder und Überprüfung der Instanz mit festgelegten Benutzer normal fortgesetzt.


## <a name="next-steps"></a>Nächste Schritte

So konfigurieren Sie SSL-Abladung Azure Application Gateway finden Sie [Konfigurieren SSL-Verschiebung](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-probe-portal/figure1.png
[2]: ./media/application-gateway-create-probe-portal/figure2.png
[3]: ./media/application-gateway-create-probe-portal/figure3.png
[4]: ./media/application-gateway-create-probe-portal/figure4.png
[5]: ./media/application-gateway-create-probe-portal/figure5.png