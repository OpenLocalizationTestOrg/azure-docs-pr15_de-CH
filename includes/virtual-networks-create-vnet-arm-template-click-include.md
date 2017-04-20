## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a>Bereitstellen Sie ARM-Vorlage mithilfe von auf Bereitstellung

Sie können vordefinierte ARM Vorlagen Upload zu einem von Microsoft verwalteten Github Repository wiederverwenden und in die Gemeinschaft öffnen. Diese Vorlagen können aus Github bereitgestellt oder gedownloadet und Ihren Bedürfnissen angepasst. Gehen Sie folgendermaßen vor um eine Vorlage bereitzustellen, die ein VNet mit zwei Subnetzen.

1. Navigieren Sie in einem Browser zu [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).
2. Bildlauf in der Liste Vorlagen und **101 Vnet zwei Subnetze**auf. Überprüfen Sie die Datei **README.md** wie folgt.

    ![READEME.md Datei github](./media/virtual-networks-create-vnet-arm-template-click-include/figure1.png)

3. Klicken Sie auf **in Azure bereitstellen**. Geben Sie gegebenenfalls Ihre Azure-Anmeldeinformationen. 
4. Blatt **Parameter** Geben Sie Ihre neue VNet erstellen möchten, und klicken Sie auf **OK**. Die Abbildung zeigt die Werte für das Szenario.

    ![ARM Vorlagenparameter](./media/virtual-networks-create-vnet-arm-template-click-include/figure2.png)

4. Klicken Sie auf **Gruppe** und wählen Sie VNet zum Hinzufügen oder klicken Sie auf **neu erstellen** , um die VNet eine neue Ressourcengruppe hinzufügen. Die nachfolgende Abbildung zeigt die Ressource Einstellungen für neue Ressourcengruppe **TestRG**aufgerufen.

    ![Ressourcengruppe](./media/virtual-networks-create-vnet-arm-template-click-include/figure3.png)

5. Ggf. ändern Sie das **Abonnement** und den **Speicherort** für das VNet.
6. Wenn nicht das VNet als ein Element im **Startmenü**angezeigt werden soll, deaktivieren Sie **an Startmenü anheften**.
5. Auf **legaler Begriffe**, lesen Sie die rechtlichen Hinweise und klicken Sie auf **kaufen** , um zuzustimmen. 
6. Klicken Sie auf **Erstellen** , um die VNet zu erstellen.

    ![Vorschauportal senden Bereitstellung untereinander](./media/virtual-networks-create-vnet-arm-template-click-include/figure4.png)

7. Nachdem die Bereitstellung abgeschlossen ist, klicken Sie auf **TestVNet** > **Alle** > **Subnetze** Subnetzeigenschaften an, wie unten dargestellt.

    ![VNet in vorschauportal erstellen](./media/virtual-networks-create-vnet-arm-template-click-include/figure5.gif)