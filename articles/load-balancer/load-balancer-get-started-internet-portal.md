<properties
   pageTitle="Ein Lastenausgleich Internetzugriff in Ressourcen-Manager mit der Azure-Portal erstellen | Microsoft Azure"
   description="Erstellen Sie ein Lastenausgleich Internetzugriff in Ressourcen-Manager mit der Azure-portal"
   services="load-balancer"
   documentationCenter="na"
   authors="anavinahar"
   manager="narayan"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="annahar" />

# <a name="creating-an-internet-facing-load-balancer-using-the-azure-portal"></a>Erstellen einer Azure-Portal mit Internetschnittstelle Lastenausgleich

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das Bereitstellungsmodell Ressourcenmanager. Sie können auch [eine klassische Bereitstellung mit Internetzugriff Lastenausgleich erstellen](load-balancer-get-started-internet-classic-portal.md)

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

Dies umfasst die Abfolge der einzelnen Aufgaben hat es getan werden, um einen Lastenausgleich erstellen und detailliert was unternommen wird, um das Ziel zu erreichen.

## <a name="what-is-required-to-create-an-internet-facing-load-balancer"></a>Was ist erforderlich, um einen Lastenausgleich Internetzugriff erstellen?

Sie müssen zum Erstellen und konfigurieren Sie die folgenden Objekte um einen Lastenausgleich bereitzustellen.

- Front-End-IP-Konfiguration – enthält öffentliche IP-Adressen für den eingehenden Netzwerkverkehr.

- Back-End-Adresspool - enthält Netzwerkkarten (NICs) für virtuelle Maschinen von Lastenausgleich der Netzwerkdatenverkehr.

- Regeln des Lastenausgleichs - enthält Regeln Port im Back-End-Adresspool mit einem öffentlichen Port auf dem Lastenausgleich zuordnen.

- Eingehende NAT-Regeln - enthält einen Anschluss für einen bestimmten virtuellen Computer im Back-End-Adresspool mit einem öffentlichen Port auf dem Lastenausgleich zuordnen.

- Prüfpunkte - Gesundheit Prüfpunkte verwendet, um Instanzen von virtuellen Maschinen im Back-End-Adresspool Verfügbarkeit enthält.

Sie erhalten weitere Informationen zum Load Balancer Komponenten mit Azure-Ressourcen-Manager bei [Azure Resource Manager Unterstützung für Lastenausgleich](load-balancer-arm.md).


## <a name="set-up-a-load-balancer-in-azure-portal"></a>Ein Lastenausgleich in Azure-Portal einrichten

> [AZURE.IMPORTANT] Es wird angenommen, dass Sie ein virtuelles Netzwerk **MyVNet**aufgerufen haben. Siehe dazu [virtuelles Netzwerk erstellen](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) . Vorausgesetzt wird auch ein Subnet in **MyVNet** bezeichnet **LB-Subnetz sein** und zwei VMs namens **web1** und **web2** bzw. innerhalb Verfügbarkeit **MyAvailSet** **MyVNet**. Finden Sie [hier](../virtual-machines/virtual-machines-windows-hero-tutorial.md) VMs zu erstellen.


1. Navigieren Sie in einem Browser zu Azure-Portal: [http://portal.azure.com](http://portal.azure.com) und melden Sie sich mit Ihrem Azure-Konto.

2. Wählen Sie links auf dem Bildschirm **neu** > **Netzwerk** > **Lastenausgleich.**

3. Geben Sie einen Namen für die Lastenausgleich Blatt **Erstellen-Lastenausgleich** . Hier wird **MyLoadBalancer**genannt.

4. Wählen Sie unter **Typ** **Public**.

5. **Öffentliche IP-Adresse**erstellen Sie eine neue öffentliche IP **MyPublicIP**aufgerufen.

6. Wählen Sie die Ressourcengruppe **MyRG**. Wählen Sie einen geeigneten **Speicherort**, und klicken Sie auf **OK**. Lastenausgleich wird dann bereitstellen und dauert einige Minuten Bereitstellung erfolgreich abgeschlossen.

![Aktualisieren von Ressourcengruppe Lastenausgleich](./media/load-balancer-get-started-internet-portal/1-load-balancer.png)


## <a name="create-a-back-end-address-pool"></a>Einen Back-End-Adresspool erstellen

1. Sobald der Lastenausgleich erfolgreich bereitgestellt wurde, wählen sie innerhalb Ihrer Ressourcen. Wählen Sie unter Settings Back-End-Pools. Geben Sie einen Namen für die Back-End-Pool. Klicken Sie auf die Schaltfläche **Hinzufügen** oben Blatts angezeigt.

2. Klicken Sie auf **einem virtuellen Computer** in Blade **-Back-End-Pool hinzufügen** .  Wählen Sie unter **verfügbarkeitsgruppe** **Wählen Sie eine Verfügbarkeit** und **MyAvailSet**. Anschließend Abschnitt VMs Blatt wählen Sie **Wählen Sie die virtuellen Computer aus** , und klicken Sie auf **web1** und **web2**, zwei VMs für Netzwerklastenausgleich erstellt. Sicherstellen Sie, dass beide blauen Häkchen links wie in der folgenden Abbildung dargestellt. Klicken Sie **Wählen Sie** dieses Blatt gefolgt von OK **Wählen Sie virtuellen Maschinen** Blade und **OK** Blatt **Back-End-Pool hinzufügen** .

    ![Backend-Adresspool hinzufügen- ](./media/load-balancer-get-started-internet-portal/3-load-balancer-backend-02.png)

3. Sicherstellen, dass Benachrichtigungen Dropdownliste hat ein Update zum Load Balancer Back-End-Pool Aktualisierung der Netzwerkschnittstelle für die VMs **web1** und **web2**gespeichert.


## <a name="create-a-probe-lb-rule-and-nat-rules"></a>Erstellen Sie einen Prüfpunkt LB Regel und NAT-Regeln

1. Erstellen Sie einen Integritätstest.

    Wählen Sie unter Settings die Lastenausgleich Prüfpunkte. Klicken Sie am oberen Rand des Blades **Hinzufügen** .

    Gibt es zwei Verfahren zum Konfigurieren einer Probe: HTTP oder TCP. Dieses Beispiel zeigt HTTP aber TCP ähnlich konfiguriert werden kann.
    Aktualisieren Sie die erforderlichen Informationen. Wie bereits erwähnt, lädt **MyLoadBalancer** Saldo Datenverkehr über Port 80. Der ausgewählte Pfad ist HealthProbe.aspx und fehlerhafte Schwellenwert ist 2 Intervall beträgt 15 Sekunden. Nachdem, klicken Sie auf **OK** , um den Prüfpunkt.

    Bewegen Sie den Mauszeiger über das "i" Symbol, um weitere Informationen zu diesen einzelnen Konfigurationen und wie sie geändert werden, um die Anforderungen.

    ![Hinzufügen einer probe](./media/load-balancer-get-started-internet-portal/4-load-balancer-probes.png)

2. Erstellen Sie eine Load Balancer Regel.

    Klicken Sie auf Regeln in den Einstellungen der Lastenausgleich für den Lastenausgleich. Neues Blatt klicken Sie auf **Hinzufügen**. Benennen Sie die Regel. Hier ist HTTP. Wählen Sie Port Front-End und Back-End-Port. Hier wird 80 für beide ausgewählt. **Pfd-Back-End-** Back-End-Pool und der zuvor erstellten **HealthProbe** als der Prüfpunkt auswählen Andere Konfigurationen können entsprechend festgelegt werden. Klicken Sie auf OK, um den Lastenausgleich Regel speichern.

    ![Eine Lastenausgleich Regel hinzufügen](./media/load-balancer-get-started-internet-portal/5-load-balancing-rules.png)

3. Eingehende NAT-Regeln erstellen

    Klicken Sie auf eingehende NAT-Regeln im Einstellungsabschnitt der Lastenausgleich. Neues Blatt, das Klicken auf **Hinzufügen**. Nennen Sie die eingehende NAT-Regel. Hier wird **inboundNATrule1**genannt. Das Ziel sollte die öffentliche IP-Adresse bereits erstellt. Wählen Sie benutzerdefinierte Service und das Protokoll, das Sie verwenden möchten. Hier wird TCP ausgewählt. Geben Sie den Port 3441, und in diesem Fall den Zielport 3389. Klicken Sie auf OK, um die Regel zu speichern.

    Nach Erstellung der erste Regel wiederholen Sie diesen Schritt für die zweite eingehende NAT-Regel Zielport 3389 inboundNATrule2 von Port 3442 aufgefordert.

    ![Eine eingehende NAT-Regel hinzufügen](./media/load-balancer-get-started-internet-portal/6-load-balancer-inbound-nat-rules.png)

## <a name="remove-a-load-balancer"></a>Ein Lastenausgleich entfernen

Um einen Lastenausgleich zu löschen, wählen Sie Lastenausgleich entfernen möchten. *Lastenausgleich* Blatt klicken Sie auf **Löschen** am oberen Rand des Blatts. Wählen Sie dann **Ja** .

## <a name="next-steps"></a>Nächste Schritte

[Beginnen Sie einen internen Lastenausgleich konfigurieren](load-balancer-get-started-ilb-arm-cli.md)

[Einen Load Balancer Verteilung Modus konfigurieren](load-balancer-distribution-mode.md)

[Konfigurieren Sie im Leerlauf TCP-Timeouteinstellungen für die Lastenausgleich](load-balancer-tcp-idle-timeout.md)
