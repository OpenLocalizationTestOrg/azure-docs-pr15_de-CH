
1. Blatt **hybridverbindungen** hybridverbindung gerade erstellten klicken **Listener einrichten**.
    
    ![Klicken Sie auf Listener einrichten](./media/app-service-hybrid-connections-manager-install/D04ClickListenerSetup.png)
    
4. **Verbindungseigenschaften Hybrid** -Blatt wird geöffnet. Wählen Sie unter **Lokale Hybrid-Verbindungs-Manager** **downloaden und manuell konfigurieren**, speichern Sie des heruntergeladenen Pakets HybridConnectionManager.msi und kopieren Sie Gateway-Verbindungszeichenfolge.
    
    ![Klicken Sie hier, um zu installieren](./media/app-service-hybrid-connections-manager-install/D05ClickToInstallHCM.png)
    
5. Geben Sie den folgenden Befehl zum Starten des Installationsprogramms eine Administratorbefehlszeile:

        start HybridConnectionManager.msi
 
7. Nach der Ausführung des Installationsprogramms klicken Sie auf **nicht**%ProgramFiles%\Microsoft\HybridConnectionManager Ordner, führen Sie HCMConfigWizard.exe und klicken Sie im Dialogfeld **Der Benutzerkontensteuerung** auf **Ja** .
        
7. Fügen Sie der Hybrid-Verbindungszeichenfolge, die Sie zuvor kopiert und klicken Sie auf **OK**. 
    
    ![Installieren](./media/app-service-hybrid-connections-manager-install/D08aHCMInstallManual.png)
    
8. Wenn die Installation abgeschlossen ist, klicken Sie auf **Schließen**.
    
    ![Klicken Sie auf Schließen](./media/app-service-hybrid-connections-manager-install/D09HCMInstallComplete.png)
    
    -Blade **hybridverbindungen** zeigt die Spalte **Status** jetzt **verbunden**. 
    
    ![Status verbunden](./media/app-service-hybrid-connections-manager-install/D10HCStatusConnected.png)