## <a name="scenario"></a>Szenario

Zur Veranschaulichung von NSGs erstellen, wird dieses Dokument folgenden Szenario verwenden.

![VNet-Szenario](./media/virtual-networks-create-nsg-scenario-include/figure1.png)

In diesem Szenario erstellen Sie eine NSG für jedes Subnetz im virtuellen Netzwerk **TestVNet** wie nachfolgend beschrieben: 

- **NSG FrontEnd**. Front-End NSG wird mit dem *Front-End* -Subnetz und zwei Regeln enthalten:  
    - **Rdp-Regel**. Diese Regel wird RDP-Datenverkehr an den *Front-End* -Subnetz zulassen.
    - **Web-Regel**. Diese Regel ermöglicht HTTP-Datenverkehr an den *Front-End* -Subnetz.
- **NSG-BackEnd**. Back-End NSG wird mit dem *Back-End-* Subnetz und zwei Regeln enthalten: 
    - **Sql-Regel**. Diese Regel ermöglicht SQL nur der Datenverkehr vom *Front-End* -Subnetz.
    - **Web-Regel**. Diese Regel verweigert alle Datenverkehr aus dem Subnetz *BackEnd* gebunden.

Die Kombination dieser Regeln Erstellen einer DMZ wie Szenario, Back-End-Subnetz erhalten nur eingehenden Datenverkehr für SQL-front-End-Subnetz und hat keinen Zugriff auf das Internet beim front-End-Subnetz mit dem Internet kommunizieren kann und Empfangen von HTTP-Anfragen nur.
 
