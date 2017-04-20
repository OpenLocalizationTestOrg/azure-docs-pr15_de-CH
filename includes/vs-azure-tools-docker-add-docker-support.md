1. Im **Projektmappen-Explorer**von Visual Studio mit der rechten Maustaste des Projekts, und wählen Sie **Hinzufügen > Andockfenster Unterstützung** aus dem Kontextmenü.

    ![Andockfenster Support-Kontextmenü hinzufügen](media/vs-azure-tools-docker-add-docker-support/docker-support-context-menu.png)

1. Eine ASP.NET Core Andockfenster Unterstützung hinzufügen führt Webprojekt zusätzlich mehrere Andockfenster-Dateien des Projekts, einschließlich Andockfenster erstellen Dateien, Windows PowerShell Bereitstellungsskripts und Andockfenster Dateien hinzugefügt wird. 

    ![Andockfenster Dateien hinzu](media/vs-azure-tools-docker-add-docker-support/docker-files-added.png)
    
> [AZURE.NOTE]Verwenden [Windows Beta Andockfenster](https://beta.docker.com)öffnen Sie Properties\Docker.props entfernen Sie Standardwert und starten Sie Visual Studio für den Wert zu übernehmen.
> 
> ```
> <DockerMachineName Condition="'$(DockerMachineName)'=='' "></DockerMachineName>
> ```
