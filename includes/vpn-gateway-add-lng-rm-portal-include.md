1. Klicken Sie im Portal **alle Ressourcen**auf **+ Hinzufügen**. **Alle** Blade-Suchfeld Geben Sie **lokalen Netzwerk-Gateway**, klicken Sie auf Suchen. Dadurch wird eine Liste zurückgegeben. **LAN Gateway** um das Blatt zu öffnen, klicken Sie auf **Erstellen** , um das **lokale Netzwerk-Gateway erstellen** Blade öffnen.

    ![Lokales Netzwerk-Gateway erstellen](./media/vpn-gateway-add-lng-rm-portal-include/addlng250.png)

2. **Erstellen LAN Gateway Blade**Geben Sie einen **Namen** für das lokale Netzwerk Gateway-Objekt.
 
3. Geben Sie eine gültige öffentliche **IP-Adresse** für den VPN-Gerät oder virtuellen Netzwerk-Gateway, den Sie eine Verbindung herstellen möchten.<br>Diese lokalen Netzwerk einen lokalen Speicherort darstellt, ist die öffentliche IP-Adresse des VPN-Geräts, das Sie möchten. Es kann nicht hinter NAT und Azure erreichbar sein.<br>Steht dieses lokalen Netzwerk ein anderes VNet, geben Sie die öffentliche IP-Adresse, die Gateway des virtuellen Netzwerks für dieses VNet zugewiesen wurde.<br>

4. **Adressraum** bezeichnet die Adressbereiche für das Netzwerk, das lokale Netzwerk darstellt. Sie können mehrere Adressbereiche Speicherplatz hinzufügen. Stellen Sie sicher, dass die Bereiche, die Sie hier nicht mit anderen Netzwerken überschneiden, die Sie herstellen möchten.
 
5. **Abonnement**stellen Sie sicher, dass das richtige Abonnement angezeigt wird.

6. **Ressourcengruppe**wählen Sie die Ressourcengruppe, die Sie verwenden möchten. Sie können eine neue Ressourcengruppe erstellen oder auswählen, die Sie bereits erstellt haben.

7. Wählen Sie als **Speicherort**, die dieses Objekt erstellt werden. Sie möchten den Speicherort wählen, dem in Ihrem VNet befindet, aber Sie nicht tun müssen.

8. Klicken Sie auf **Erstellen** , um das lokale Netzwerk-Gateway erstellen.
