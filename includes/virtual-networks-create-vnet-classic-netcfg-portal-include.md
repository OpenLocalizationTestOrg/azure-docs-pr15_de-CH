## <a name="how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal"></a>Ein VNet mit einer Netzwerk-Konfigurationsdatei im Azure-Portal erstellen

Azure mithilfe eine XML-Datei alle verfügbaren VNets an ein Abonnement definiert. Sie können diese Datei herunterladen bearbeiten zum Ändern oder löschen vorhandene VNets und neue erstellen. In diesem Dokument werden Sie lernen, genannt Network Configuration (oder Netcgf) Datei herunterladen und bearbeiten, um ein neues VNet erstellen. [Azure virtual Network Configuration Schema](https://msdn.microsoft.com/library/azure/jj157100.aspx) Weitere Informationen zu Netzwerk-Konfigurationsdatei zu überprüfen.

Gehen Sie folgendermaßen vor um ein VNet mit einer Datei Netcfg Azure Portal erstellen.

1. Ein Browser, navigieren Sie zu http://manage.windowsazure.com und melden Sie sich ggf. mit Ihrem Azure-Konto.
2. Blättern Sie in der Liste der Dienste, und klicken Sie auf **Netzwerke** wie folgt.

    ![Azure virtuelle Netzwerke](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure1.gif)

3. Am Ende der Seite klicken Sie auf die Schaltfläche **EXPORTIEREN** unten.

    ![Schaltfläche Exportieren](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure2.png)

4. Auf der Seite **Konfiguration exportieren** Abonnement virtuelle Netzwerkkonfiguration aus exportieren möchten und klicken das Häkchen in der unteren linken Ecke der Seite.
5. Anweisungen Sie Ihr Browser zum Speichern der Datei **NetworkConfig.xml** . Vergessen Sie notieren, wo Sie die Datei speichern.
6. Gespeichert in Schritt 5 Anwendung alle XML- oder Text-Editor öffnen und nach der **<VirtualNetworkSites>** Element. Haben Sie alle Netzwerke bereits jedes Netzwerk erscheint als eigene **<VirtualNetworkSite>** Element.
7. Um das virtuelle Netzwerk in diesem Szenario beschriebene erstellen, fügen Sie das folgende XML direkt unter der **<VirtualNetworkSites>** Element:

        <VirtualNetworkSite name="TestVNet" Location="Central US">
          <AddressSpace>
            <AddressPrefix>192.168.0.0/16</AddressPrefix>
          </AddressSpace>
          <Subnets>
            <Subnet name="FrontEnd">
              <AddressPrefix>192.168.1.0/24</AddressPrefix>
            </Subnet>
            <Subnet name="BackEnd">
              <AddressPrefix>192.168.2.0/24</AddressPrefix>
            </Subnet>
          </Subnets>
        </VirtualNetworkSite>

8.  Speichern Sie Konfiguration von Netzwerk.
9.  Klicken Sie in Azure-Portal auf der unteren linken Ecke der Seite auf **neu**, klicken Sie auf **Netzwerkdienste**, und klicken Sie auf **Virtuelles Netzwerk**, und klicken Sie **Konfiguration importieren** , wie in der folgenden Abbildung dargestellt.

    ![Konfiguration importieren](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure3.gif)

10.  Auf der Seite **Netzwerk Konfigurationsdatei importieren** auf **Für Datei suchen...**und navigieren Sie zu dem Ordner, der die Datei in Schritt 8, wählen Sie die Datei und klicken Sie auf **Öffnen**. Die Webseite sollte der folgenden Abbildung ähneln. Klicken Sie in der unteren rechten Ecke der Seite auf die-Taste, um zum nächsten Schritt wechseln.

    ![Netzwerk-Konfigurationsseite](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure4.png)

11.   Beachten Sie den Eintrag für das neue VNet wie in der folgenden Abbildung dargestellt auf **Ihr Netzwerk** .

    ![Die Netzwerk-Seite erstellen](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure5.png)

12.   Klicken Sie auf das Häkchen rechts der unteren Seite das VNet erstellen. Nach wenigen Sekunden wird das VNet in der Liste der verfügbaren VNets angezeigt wie in der folgenden Abbildung dargestellt.

    ![Neues virtuelles Netzwerk](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure6.png)