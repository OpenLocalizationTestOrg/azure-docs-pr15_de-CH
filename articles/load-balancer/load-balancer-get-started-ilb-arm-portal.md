<properties
   pageTitle="Erste Schritte beim Erstellen einer internen Lastenausgleich in Ressourcen-Manager mit der Azure-Portal | Microsoft Azure"
   description="Erstellen Sie ein interner Lastenausgleich in Ressourcen-Manager mit der Azure-portal"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="create-an-internal-load-balancer-in-the-azure-portal"></a>Erstellen Sie ein interner Lastenausgleich in Azure-portal

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassische Bereitstellungsmodell](load-balancer-get-started-ilb-classic-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="get-started-creating-an-internal-load-balancer-using-azure-portal"></a>Erste Schritte beim Erstellen einer internen Lastenausgleich über Azure-portal

Gehen Sie folgendermaßen vor, einen internen Lastenausgleich von Azure-Portal erstellen.

1. Öffnen Sie einen Browser, navigieren Sie zum [Azure-Portal](http://portal.azure.com)und Azure-Konto anmelden.
2. Klicken Sie in der oberen linken Seite des Bildschirms auf **neu** > **Netzwerk** > **Lastenausgleich**.
3. Blatt **Erstellen-Lastenausgleich** Geben Sie einen **Namen** für Ihr System zum Lastenausgleich.
4. Klicken Sie unter **System** **intern**.
5. Klicken Sie auf **virtuelle Netzwerk**, und wählen Sie das virtuelle Netzwerk Lastenausgleich erstellt werden soll.

    >[AZURE.NOTE] Wenn Sie die gewünschten das virtuelle Netzwerk nicht angezeigt wird, überprüfen Sie den **Speicherort** für Lastenausgleich verwenden und entsprechend ändern.

6. **Subnetz**auf und wählen Sie dann das Subnetz Lastenausgleich erstellt werden soll.
7. **IP-Adresszuweisung**klicken Sie **dynamische** oder **statische**, je nachdem, ob Sie die IP-Adresse des Lastenausgleichs (statisch) behoben werden.

    >[AZURE.NOTE] Wenn Sie eine statische IP-Adresse verwenden, müssen Sie eine Adresse für den Lastenausgleich bereitzustellen.

8. Klicken Sie unter **Ressourcengruppe** entweder Name eine neue Ressourcengruppe für den Lastenausgleich oder **vorhandene auswählen** und wählen Sie eine vorhandene Ressourcengruppe.
9. Klicken Sie auf **Erstellen**.

## <a name="configure-load-balancing-rules"></a>Konfigurieren des Netzwerklastenausgleichs Regeln

Navigieren Sie nach der Erstellung Load Balancer Load Balancer Ressourcen konfigurieren.
Sie müssen zuerst einen Back-End-Adresspool und einen Prüfpunkt vor eine Lastenausgleich Regel konfigurieren.

### <a name="step-1-configure-a-back-end-pool"></a>Schritt 1: Konfigurieren von Back-End-pool

1. Klicken Sie im Azure-Portal auf **Durchsuchen** > **Lastenausgleich**, und klicken Sie auf die oben erstellte Lastenausgleich.
2. Klicken Sie auf **Back-End-Pools**Blatt **Einstellungen** .
3. **Backend-Adresspools** Blatt klicken Sie auf **Hinzufügen**.
4. Blatt **Hinzufügen Back-End-Pool** Geben Sie einen **Namen** für die Back-End-Pool, und klicken Sie auf **OK**.

### <a name="step-2-configure-a-probe"></a>Schritt 2: Konfigurieren einer probe

1. Klicken Sie im Azure-Portal auf **Durchsuchen** > **Lastenausgleich**, und klicken Sie auf die oben erstellte Lastenausgleich.
2. Klicken Sie im Blatt **Einstellungen** **untersucht**.
3. Blade **-Prüfpunkte** klicken Sie auf **Hinzufügen**.
4. Blatt **Hinzufügen Probe** Geben Sie einen **Namen** für den Prüfpunkt.
5. Wählen Sie unter **Protokoll** **HTTP** (Websites) oder **TCP** (andere TCP-basierte Anwendung).
6. Geben Sie unter **Port**den Port für den Prüfpunkt den Zugriff auf.
7. Geben Sie unter **Pfad** (nur HTTP-Prüfpunkte) den Pfad als einen Prüfpunkt.
8. Geben Sie unter **Intervall** an, wie häufig die Anwendung untersuchen.
9. Geben Sie unter **Fehlerhafte Schwellenwert**wie viele Versuche fehlschlagen sollte vor der Back-End-virtuelle Computer als fehlerhaft gekennzeichnet ist.
10. Klicken Sie auf **OK** , um Prüfpunkt.

### <a name="step-3-configure-load-balancing-rules"></a>Schritt 3: Konfigurieren des Netzwerklastenausgleichs Regeln

1. Klicken Sie im Azure-Portal auf **Durchsuchen** > **Lastenausgleich**, und klicken Sie auf die oben erstellte Lastenausgleich.
2. Klicken Sie im Blatt **Einstellungen** **Lastenausgleich Regeln**.
3. Das Blade **Lastenausgleich Regeln** klicken Sie auf **Hinzufügen**.
4. Blatt **Hinzufügen laden Lastenausgleich Regel** Geben Sie einen **Namen** für die Regel.
5. Wählen Sie unter **Protokoll** **HTTP** (Websites) oder **TCP** (andere TCP-basierte Anwendung).
6. Geben Sie unter **Port**Clients Port Verbindung Lastenausgleich.
7. Geben Sie unter **Back-End-Port**den Port im Back-End-Pool verwendet werden (in der Regel Load Balancer Port und den Back-End-Port sind gleich).
8. Wählen Sie unter **Back-End-Pool**-Back-End-Pool, der Sie soeben erstellt haben.
9. Wählen Sie unter **Sitzung Dauerhaftigkeit**wie Sitzungen beibehalten werden soll.
10. Geben Sie unter **Leerlaufzeitlimit (Minuten)**das Leerlauftimeout.
11. **Bewegliche IP (direct Server zurückgeben)**klicken Sie auf **deaktiviert** oder **aktiviert**.
12. Klicken Sie auf **OK**.

## <a name="next-steps"></a>Nächste Schritte

[Einen Load Balancer Verteilung Modus konfigurieren](load-balancer-distribution-mode.md)

[Konfigurieren Sie im Leerlauf TCP-Timeouteinstellungen für die Lastenausgleich](load-balancer-tcp-idle-timeout.md)