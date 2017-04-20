<properties
   pageTitle="Abrufen von Daten aus Azure Analysis Services | Microsoft Azure"
   description="Informationen Sie zu verbinden und Daten von einem Analysis Services-Server in Azure."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="get-data-from-azure-analysis-services"></a>Daten von Azure Analysis Services abrufen
Sobald Sie einen Server in Azure erstellt und ein Tabellenmodell bereitgestellt haben, können Benutzer in Ihrer Organisation zu beginnen Daten.

Azure Analysis Services unterstützt Clientverbindungen [aktualisiert Objektmodelle](#client-libraries)verwenden. TOM, AMO, Adomd.Net oder MSOLAP, über Xmla mit dem Server herstellen. Z. B. Power BI, Power BI Desktop Excel oder Clientanwendungen von Drittanbietern unterstützt, die Objektmodelle.

## <a name="server-name"></a>Servername
Wenn Sie Analysis Services-Server in Azure erstellen, geben Sie einen eindeutigen Namen und die Region der Server erstellt werden. Beim Festlegen des Servernamens in einer Verbindung ist das Namensschema Server:
```
<protocol>://<region>/<servername>
```
 Protokoll Zeichenfolge **Asazure**, der Bereich ist der Uri des Bereichs, in dem der Server erstellt wurde (z. B. für Westen der USA, westus.asazure.windows.net) und Servername ist der Name des Servers innerhalb des Bereichs eindeutig.

## <a name="get-the-server-name"></a>Servername abrufen
Bevor Sie eine Verbindung herstellen, müssen Sie den Servernamen erhalten. In **Azure-Portal** > Server > **Übersicht** > **Servername**, kopieren Sie den gesamten Server-Namen. Wenn andere Benutzer in Ihrer Organisation zu diesem Server verbinden, sollten Sie diesen Servernamen freigeben. Wenn Sie Servernamen angeben, muss der gesamte Pfad verwendet werden.

![Servername in Azure abrufen](./media/analysis-services-deploy/aas-deploy-get-server-name.png)


## <a name="connect-in-power-bi-desktop"></a>Power BI Desktop verbinden

> [AZURE.NOTE] Dieses Feature ist die Vorschau.

1. ** [Power BI Desktop](https://powerbi.microsoft.com/desktop/)klicken Sie** > **Datenbanken** > **Azure Analysis Services**.

2. Fügen Sie in **Server**den Servernamen aus der Zwischenablage ein.

3. **Datenbank**sollten Sie den Namen der tabellarischen Modelldatenbank oder Perspektive herstellen möchten, fügen Sie ihn hier. Andernfalls können Sie dieses Feld leer lassen. Sie können eine Datenbank oder eine Perspektive auf dem nächsten Bildschirm auswählen.

4. Lassen Sie die Standardoption **live Connect** ausgewählt und dann **Verbinden**. Wenn Sie aufgefordert werden, geben Sie ein Konto, geben Sie ein Konto Ihrer Organisation.

5. **Navigator**-erweitern Sie den Server und das Modell oder die Perspektive herstellen möchten, klicken Sie auf **Verbinden**. Ein Klick auf ein Modell oder eine Perspektive zeigt alle Objekte für die Ansicht.


## <a name="connect-in-power-bi"></a>Power BI verbinden
1. Erstellen Sie eine Power BI Desktop-Datei hat eine aktive Verbindung zum Modell auf dem Server.

2. ** [Power BI](https://powerbi.microsoft.com)klicken Sie** > **Dateien**. Suchen Sie und wählen Sie die Datei.


## <a name="connect-in-excel"></a>Verbinden in Excel
Verbindung mit Azure Analysis Services-Server in Excel wird mit Daten in Excel 2016 abrufen oder Power Abfrage in früheren Versionen unterstützt. [Msolap.7 Anbieter](https://aka.ms/msolap) ist erforderlich. Verbindung mit der Tabelle importieren in PowerPivot wird nicht unterstützt.

1. Klicken Sie in Excel 2016 auf dem Menüband **Daten** auf **Externe Daten** > **Aus anderen Quellen** > **Von Analysis Services**.

2. Fügen Sie im Datenverbindungs-Assistenten **Servername**den Servernamen aus der Zwischenablage. **Anmeldeinformationen**, wählen **die folgenden Benutzernamen und Kennwort**, und geben Sie den Namen Organisationseinheit beispielsweise nancy@adventureworks.com, und das Kennwort.

    ![In Excel Anmeldung verbinden](./media/analysis-services-connect/aas-connect-excel-logon.png)

4. Wählen Sie in **Datenbank und Tabelle wählen**die Datenbank und Modell oder Perspektive und klicken Sie dann auf **Fertig stellen**.

    ![In Excel wählen Sie Modell verbinden](./media/analysis-services-connect/aas-connect-excel-select.png)

## <a name="connection-string"></a>Verbindungszeichenfolge
Beim Verbinden mit Analysis Services Azure Formate mit tabellarischen Objektmodell verwenden die folgende Verbindungszeichenfolge:

###### <a name="integrated-azure-active-directory-authentication"></a>Integrierte Azure Active Directory-Authentifizierung
```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```
Integrierte Authentifizierung übernehmen der Anmeldeinformationscache Azure Active Directory, falls verfügbar. Wenn dies nicht der Fall, Azure-Anmeldefenster wird angezeigt.

###### <a name="azure-active-directory-authentication-with-username-and-password"></a>Azure Active Directory-Authentifizierung mit Benutzername und Kennwort
```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

## <a name="client-libraries"></a>Client-Bibliotheken
Bei der Verbindung mit Analysis Services Azure aus Excel oder anderen Schnittstellen wie TOM AsCmd, ADOMD.NET müssen Sie die neuesten Clientbibliotheken Anbieter installieren. Die neueste abrufen:  

[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)</br>
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</br>
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</br>
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</br>



## <a name="next-steps"></a>Nächste Schritte
[Verwalten Sie Ihre server](analysis-services-manage.md)
