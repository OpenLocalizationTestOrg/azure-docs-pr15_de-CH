## <a name="how-to-create-a-vnet-in-the-azure-portal"></a>Erstellen Sie ein VNet in Azure-portal

Gehen Sie folgendermaßen vor um ein VNet basierend auf dem Szenario zu erstellen.

1. Ein Browser, navigieren Sie zu http://manage.windowsazure.com und melden Sie sich ggf. mit Ihrem Azure-Konto.
2. Klicken Sie auf **neu** > **Netzwerkdienste** > **VIRTUAL NETWORK** > **Benutzerdefinierte** , wie in der folgenden Abbildung dargestellt.

    ![VNet im Portal erstellen](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure1.gif)

3. Auf der Seite **Virtual Network Details** geben den **Namen** der VNet, den **Speicherort**und klicken Sie auf den Pfeil rechts unten der Seite Schritt 2 wechseln. Die nachfolgende Abbildung zeigt die Einstellungen für das Szenario.

    ![Virtuelles Netzwerk Seite](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure2.png)

4. Geben Sie auf **DNS-Server und VPN-Konnektivität** den Namen und die IP-Adresse für bis zu 9 DNS-Server verwenden. Wenn Sie einen DNS-Server nicht angeben, verwendet das VNet interne Benennung Auflösung Auflösung von Azure bereitgestellt. In diesem Szenario wird keine DNS-Server konfiguriert.
5. Wenn Sie Ihr VNet Punkt-zu-Standort-VPN-Zugriff gewähren möchten, aktivieren Sie das Kontrollkästchen **eine Punkt-zu-Standort-VPN konfigurieren** . Wenn Sie eine Punkt-zu-Standort-VPN nicht konfigurieren, können Sie es zu Ihrem VNet jederzeit nach der Erstellung hinzufügen. In diesem Szenario wird keine Punkt-zu-Standort-VPN konfiguriert.
6. Wenn Sie Standort-zu-Standort-VPN-Konnektivität zwischen der VNet und anderen VNet oder lokalen Netzwerk bereitstellen möchten, aktivieren Sie das Kontrollkästchen **Konfigurieren einer Standort-zu-Standort-VPN** und angeben mit **ExpressRoute** oder Hinweis und den Namen des Netzwerks herstellen. Wenn ein Standort-zu-Standort-VPN nicht konfigurieren, können Sie es zu Ihrem VNet jederzeit nach der Erstellung hinzufügen. In diesem Szenario wird ein Standort-zu-Standort-VPN nicht konfiguriert.
7. Klicken Sie auf den Pfeil rechts unten der Seite wechseln Sie zu Schritt 3 zeigt die Abbildung die Einstellungen für das Szenario.

    ![DNS-Server und VPN-Konnektivität-Seite](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure3.png)

8. Auf **Virtuellen Netzwerk Adressräume** unter **Start-IP-** *10.0.0.0* Adressraum VNet ändern auf, und geben Sie Start Adressraum verwenden möchten. Geben Sie für dieses Szenario *192.168.0.0*. 
9. Wählen Sie unter **CIDR (ZÄHLUNG)** die Anzahl der Bits für die Subnetzmaske. Wählen Sie für dieses Szenario *16 (65536)*.
10. Unter **SUBNETZE**auf *Subnetz 1* und Umbenennen Sie Subnetz ggf.. In diesem Szenario umbenennen Sie, *Front-End*.

    >[AZURE.NOTE] Wenn außerhalb das Textfeld Name für ein Subnetz klicken Sie dürfen den Namen bearbeiten, wenn das Subnetz. Beheben, Sie das Subnetz durch Klicken auf die Schaltfläche X rechts entfernen müssen, fügen Sie ein neues Subnetz wie unter Schritt 13 fort.

11. Geben Sie unter **Start-IP-** erste Subnetz IP-Startadresse für das Subnetz. Geben Sie für dieses Szenario *192.168.1.0*.
12. Wählen Sie unter **CIDR (ZÄHLUNG)** die Anzahl der Bits für die Subnetzmaske für das erste Subnetz. Wählen Sie in diesem Szenario *24 (256)*.
13. Klicken Sie ggf. auf **Subnetz hinzufügen** , fügen Sie ein neues Subnetz. In diesem Szenario fügen Sie ein Subnetz hinzu und wiederholen Sie die Schritte 10 bis 12 VNet wie in der folgenden Abbildung dargestellt konfiguriert.

    ![Virtuelles Netzwerk Adressseite Leerzeichen](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure4.png)

14. Klicken Sie auf das Häkchen rechts der unteren Seite das VNet erstellen. Nach wenigen Sekunden wird das VNet in der Liste der verfügbaren VNets angezeigt wie in der folgenden Abbildung dargestellt.

    ![Neues virtuelles Netzwerk](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure5.png)