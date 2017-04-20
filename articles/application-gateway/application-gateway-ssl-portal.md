<properties
   pageTitle="Ein Gateway für SSL-Verschiebung mithilfe der Portalwebsite konfigurieren | Microsoft Azure"
   description="Diese Seite bietet Anleitung Erstellen Sie ein Gateway mit SSL Verschiebung mithilfe der Portalwebsite"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/09/2016"
   ms.author="gwallace"/>

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-portal"></a>Konfigurieren Sie ein Gateway für SSL-Verschiebung mithilfe der Portalwebsite

> [AZURE.SELECTOR]
-[Azure-Portal](application-gateway-ssl-portal.md)
-[Azure Ressourcenmanager PowerShell](application-gateway-ssl-arm.md)
-[Azure klassische PowerShell](application-gateway-ssl.md)

Azure Application Gateway kann zum Beenden der Sitzung Secure Sockets Layer (SSL) am Gateway teure SSL-Entschlüsselung Aufgaben zu der Webserverfarm konfiguriert werden. SSL-Verschiebung vereinfacht auch die Front-End-Server-Setup und Verwaltung der Website.

## <a name="scenario"></a>Szenario

Das folgende Szenario durchläuft konfigurieren SSL auf ein vorhandenes Gateway abnimmt. Das Szenario wird davon ausgegangen, dass Sie bereits die Schritte [Ein Gateway](application-gateway-create-gateway-portal.md)erstellt haben.

## <a name="before-you-begin"></a>Bevor Sie beginnen

Zum Konfigurieren von SSL-Verschiebung mit ein Gateway ist ein Zertifikat erforderlich. Dieses Zertifikat auf dem Anwendungsgateway geladen und zum Verschlüsseln und Entschlüsseln von Datenverkehr über SSL verwendet. Das Zertifikat muss im Format Persönlicher Informationsaustausch (Pfx). Dieses Dateiformat kann für den privaten Schlüssel exportieren die vom Application Gateway zu ver- und Entschlüsselung von Datenverkehr führen.

## <a name="add-an-https-listener"></a>Einen HTTPS-Listener hinzufügen

HTTPS-Listener sucht Datenverkehr auf Grundlage der Konfiguration und hilft den Datenverkehr an Back-End-Pools weiterleiten.

### <a name="step-1"></a>Schritt 1

Azure-Portal navigieren Sie, und wählen Sie ein vorhandenes gateway

### <a name="step-2"></a>Schritt 2

Listener auf, und klicken Sie auf Hinzufügen, um einen Abhörer hinzuzufügen.

![App-Gateways Übersicht blade][1]

### <a name="step-3"></a>Schritt 3

Füllen Sie die erforderlichen Informationen für den Listener und Upload .pfx-Zertifikat, wenn Sie fertig sind, klicken Sie auf OK.

**Name** - Wert ist ein angezeigter Name des Listeners.

**Front-End-IP-Konfiguration** – dieser Wert ist der Front-End-IP-Konfiguration, die für den Listener verwendet.

**Frontend-Port (Name/Port)** – einen angezeigten Namen für den Port auf front-End der Anwendungsgateway und dem tatsächlichen Port verwendet.

**Protokoll** - Schalter bestimmt, ob Https oder http für front-End verwendet wird.

**Zertifikat (Name/Kennwort)** - Wenn SSL-Verschiebung verwendet und eine .pfx-Zertifikat für diese Einstellung ist ein angezeigter Name und Kennwort sind erforderlich.

![Listener-Blade hinzufügen][2]

## <a name="create-a-rule-and-associate-it-to-the-listener"></a>Eine Regel erstellen und den Listener zuordnen

Der Listener ist jetzt erstellt. Es ist Zeit für eine Regel behandeln den Datenverkehr vom Listener erstellen.

### <a name="step-1"></a>Schritt 1

Klicken Sie auf die **Regeln** des Gateways Anwendung und dann auf Hinzufügen.

![App-Gateways Regeln blade][3]

### <a name="step-2"></a>Schritt 2

Geben Sie den Namen der Regel Blade **Grundregel hinzufügen** und wählen Sie im vorherigen Schritt erstellten Listener. Wählen Sie die entsprechenden Back-End-Pool und http, und klicken Sie auf **OK**

![HTTPS-Einstellungen][4]

Jetzt werden an das Anwendungsgateway gespeichert. Speichern für diese Einstellungen Anspruch über das Portal oder über PowerShell werden zu verarbeiten. Gespeichertes Application Gateway verarbeitet die Ver- und Entschlüsselung von Datenverkehr. Gesamten Datenverkehr zwischen Application Gateway und der Back-End-Webserver über http erfolgt. Kommunikation an den Client über Https initiiert wird an den Client verschlüsselt zurückgegeben.

## <a name="next-steps"></a>Nächste Schritte

Konfigurieren einer benutzerdefinierten Integritätstest für Azure Application Gateway finden Sie unter [Erstellen einer benutzerdefinierten Integritätstest](application-gateway-create-gateway-portal.md).

[1]: ./media/application-gateway-ssl-portal/figure1.png
[2]: ./media/application-gateway-ssl-portal/figure2.png
[3]: ./media/application-gateway-ssl-portal/figure3.png
[4]: ./media/application-gateway-ssl-portal/figure4.png