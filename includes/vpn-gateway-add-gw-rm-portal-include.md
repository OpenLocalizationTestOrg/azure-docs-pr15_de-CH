1. Gehen Sie im Portal auf **neu**. Geben Sie in Suchen "Virtuelle Netzwerk-Gateway". Suchen Sie **virtuelle Netzwerk-Gateway** zurück suchen, und klicken Sie auf den Eintrag. Das **virtuelle Netzwerk-Gateway erstellen** Blade wird geöffnet.
2. Klicken Sie am unteren Rand das **virtuelle Netzwerk-Gateway** -Blade **Erstellen** . **Erstellen virtuelle Netzwerk-Gateway** -Blatt wird geöffnet. Geben Sie die Werte für Ihr virtuelles Netzwerk-Gateway.

    ![Erstellen virtuelle Netzwerk-Gateway Blade-Felder] (./media/vpn-gateway-add-gw-rm-portal-include/createvnetgw300.png "Erstellen virtuelle Netzwerk-Gateway Blade-Felder")

3. **Name**: Name Ihres Gateways. Dies ist nicht identisch mit Namen ein. Es ist der Name des Gatewayobjekts, die Sie erstellen.

4. **Gateway-Typ**: Wählen Sie **VPN**. VPN-Gateways verwenden virtuelle Netzwerk-Gateway **VPN**. 

5. **VPN-Typ**: Wählen Sie für Ihre Konfiguration angegebenen VPN-Typ. Die meisten Konfigurationen erfordern einen Route-basierten VPN-Typ.

6. **SKU**: Wählen Sie das Gateway SKU aus. In der Dropdown-Liste aufgeführten SKUs hängen die VPN-Typ.

7. **Ort**: Passen Sie das Feld **Speicherort** den Speicherort auf Ihrem virtuelle Netzwerk befindet.
 
8. Wählen Sie das virtuelle Netzwerk dieses Gateway hinzugefügt werden soll. Klicken Sie auf **Virtual Network** Blade **Wählen Sie ein virtuelles Netzwerk** öffnen. Wählen Sie das VNet. Wenn Sie nicht das VNet stellen sicher, dass das Feld **Speicherort** den Bereich zeigt in dem virtuelle Netzwerk befindet.

9. Wählen Sie eine öffentliche IP-Adresse. Klicken Sie auf **öffentliche IP-Adresse** , um das Blade **Wählen Sie öffentliche IP-Adresse** öffnen. Klicken Sie auf **+ neue** **öffentliche IP-Adresse Blade erstellen**zu öffnen. Geben Sie einen Namen für die öffentliche IP-Adresse. Dieses Blatt erstellt eine öffentliche IP-Adressobjekt, eine öffentliche IP-Adresse dynamisch zugewiesen wird.<br>Klicken Sie auf **OK** , um Ihr Blatt speichern.

10. **Abonnement**: Stellen Sie sicher, dass das richtige Abonnement aktiviert ist.

11. **Ressourcengruppe**: Diese Einstellung wird durch das virtuelle Netzwerk, das Sie auswählen bestimmt. 

12. Passen Sie der **Speicherort** nicht nach Angabe die bisherigen an

13. Überprüfen. Wenn Ihr Gateway auf dem Dashboard angezeigt werden soll, können Sie **auf Dashboard** am unteren Rand das Blade auswählen.

14. Klicken Sie auf **Erstellen** , um das Gateway erstellen. Die Einstellung überprüft und Sie sehen "Deploying Virtual Network Gateway" nebeneinander auf dem Dashboard. Erstellen eines Gateways kann bis zu 45 Minuten. Sie müssen der Portalseite um den abgeschlossenen Status aktualisieren.

    ![Bereitstellen von virtuellen Netzwerk-gateway] (./media/vpn-gateway-add-gw-rm-portal-include/deployvnetgw150.png "Bereitstellen von virtuellen Netzwerk-gateway")

11. Nach der Erstellung des Gateways die IP-Adresse angezeigt, die das virtuelle Netzwerk im Portal ansehen, zugewiesen wurde. Das Gateway erscheint als ein verbundenes Gerät. Klicken Sie auf das Gerät (Ihr virtuelles Netzwerk-Gateway) um weitere Informationen anzuzeigen.



