<properties
    pageTitle="Verbinden mit SQL-Datenbank mithilfe von SQL Server Management Studio in Azure RemoteApp | Microsoft Azure"
    description="Verwenden Sie in diesem Lernprogramm erfahren Sie, wie mit SQL Server Management Studio in Azure RemoteApp für Sicherheit und Leistung beim Verbinden mit SQL-Datenbank"
    services="sql-database"
    documentationCenter=""
    authors="adhurwit"
    manager="jhubbard"/>

<tags
    ms.service="sql-database"
    ms.workload="data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="adhurwit"/>

# <a name="use-sql-server-management-studio-in-azure-remoteapp-to-connect-to-sql-database"></a>Verwenden Sie SQL Server Management Studio in Azure RemoteApp Verbindung zur SQL-Datenbank

## <a name="introduction"></a>Einführung  
In diesem Lernprogramm wird veranschaulicht, wie mit SQL Server Management Studio (SSMS) in Azure RemoteApp SQL-Datenbank herstellen. Er führt Sie durch die Einrichtung von SQL Server Management Studio in Azure RemoteApp erläutert die Vorteile und Sicherheitsfunktionen, mit denen Sie, in Azure Active Directory zeigt.

**Geschätzte Dauer:** 45 Minuten

## <a name="ssms-in-azure-remoteapp"></a>SSMS in Azure RemoteApp

Azure RemoteApp ist ein RDS-Dienst in Azure, die Anträge übermittelt. Sie erfahren sie hier: [Neuigkeiten RemoteApp?](../remoteapp/remoteapp-whatis.md)

Ausführen in Azure RemoteApp SSMS gibt genauso wie SSMS lokal ausgeführt.

![Screenshot zeigt SSMS in Azure RemoteApp ausgeführt][1]



## <a name="benefits"></a>Vorteile

Viele Vorteile mit SSMS in Azure RemoteApp, einschließlich:

- Azure SQL Server Port 1433 keinen extern (außerhalb von Azure) verfügbar gemacht werden.
- Ohne hinzufügen und Entfernen von IP-Adressen in Firewall Azure SQL Server.
- Alle Azure RemoteApp-Verbindungen auftreten, verschlüsselte Remote Desktop Protocol auf Port 443 über HTTPS
- Mehrere Benutzer ist und skalieren können.
- Gibt es eine Leistungssteigerung davor SSMS im Bereich der SQL-Datenbank.
- Überwachen Sie die Benutzerberichte mit Azure RemoteApp Premium Edition von Azure Active Directory.
- Sie können die kombinierte Authentifizierung (MFA).
- Zugriff SSMS überall Verwendung aller unterstützten Azure RemoteApp-Clients umfasst iOS, Android, Mac, Windows Phone und Windows-PC.


## <a name="create-the-azure-remoteapp-collection"></a>Erstellen der Azure RemoteApp-Sammlung

Hier werden die Schritte zum Erstellen Ihrer Azure RemoteApp-Sammlung mit SSMS:


### <a name="1-create-a-new-windows-vm-from-image"></a>1. erstellen Sie 1. eine neue Windows-VM aus Bild
Verwenden Sie "Windows Server Remote Desktop Session Host Windows Server 2012 R2" Bild aus der Galerie zu Ihrer neuen VM.


### <a name="2-install-ssms-from-sql-express"></a>2. installieren Sie SSMS von SQL Express

Gehen Sie auf die neue VM und navigieren Sie zu dieser Downloadseite: [Microsoft® SQL Server® 2014 Express](https://www.microsoft.com/en-us/download/details.aspx?id=42299)

Es steht nur SSMS herunterladen. Nach dem Herunterladen ins Installationsverzeichnis, und führen Sie Setup SSMS installieren.

Sie müssen SQL Server 2014 Service Pack 1 installieren. Hier herunterladen: [Microsoft SQL Server 2014 Service Pack 1 (SP1)](https://www.microsoft.com/en-us/download/details.aspx?id=46694)

SQL Server 2014 Service Pack 1 enthält wichtige Funktionen für Azure SQL-Datenbank.


### <a name="3-run-validate-script-and-sysprep"></a>3. Führen Sie Sysprep und Skript überprüfen

PowerShell-Skript wird auf dem Desktop des virtuellen Computers überprüfen aufgerufen. Durch Doppelklicken auf ausführen. Es überprüft, dass die VM für Remotehosting Anwendungstypen verwendet werden kann. Wenn Prüfung abgeschlossen ist, fragt es führen Sie Sysprep - ausführen wählen.

Wenn Sysprep abgeschlossen ist, wird die VM Herunterfahren.

Weitere Informationen zum Erstellen eines Abbilds Azure RemoteApp finden Sie unter: [ein RemoteApp Vorlagenbild in Azure erstellen](http://blogs.msdn.com/b/rds/archive/2015/03/17/how-to-create-a-remoteapp-template-image-in-azure.aspx)


### <a name="4-capture-image"></a>4. Bild erfassen

Wenn die VM gestoppt wurde, finden sie im aktuellen Portal zu erfassen.

Informationen zum Erstellen eines Abbilds finden Sie unter [Zeichnen ein Abbild eines virtuellen Computers Windows Azure mit klassischen Bereitstellungsmodell erstellt](../virtual-machines/virtual-machines-windows-classic-capture-image.md)


### <a name="5-add-to-azure-remoteapp-template-images"></a>5. Azure RemoteApp Vorlage Bilder hinzufügen

Im aktuellen Portal Azure RemoteApp Teil zur Registerkarte Vorlage Bilder und klicken Sie auf Hinzufügen. Klicken Sie im Popupmenü wählen Sie aus "Ein Bild aus der Bibliothek virtuelle Computer importieren", und wählen Sie dann das Bild, das Sie gerade erstellt haben.



### <a name="6-create-cloud-collection"></a>6. Cloud-Sammlung erstellen

Erstellen Sie im aktuellen Portal eine neue Azure RemoteApp Cloud-Sammlung. Wählen Sie das gerade importiert Vorlagenbild mit SSMS installiert.

![Neue cloudsammlung erstellen][2]


### <a name="7-publish-ssms"></a>7. SSMS veröffentlichen

Die Veröffentlichung Registerkarte die neue cloudsammlung, die select Veröffentlichen einer Anwendung über das Startmenü und wählen Sie dann aus der Liste SSMS.

![App veröffentlichen][5]

### <a name="8-add-users"></a>8 Benutzer hinzufügen

Auf der Registerkarte Benutzerzugriff können Sie die Benutzer, die Zugriff auf diese Auflistung Azure RemoteApp nur einschließlich SSMS auswählen.

![Benutzer hinzufügen][6]


### <a name="9-install-the-azure-remoteapp-client-application"></a>9 installieren Sie 9 Azure RemoteApp-Clientanwendung

Herunterladen und installieren einen Azure RemoteApp-Client: [herunterladen | Azure RemoteApp](https://www.remoteapp.windowsazure.com/en/clients.aspx)



## <a name="configure-azure-sql-server"></a>Azure SQL Server konfigurieren

Die einzige erforderliche Konfiguration wird sichergestellt, dass Azure Services für die Firewall aktiviert ist. Bei Verwendung dieser Lösung müssen Sie keine IP-Adressen öffnen die Firewall hinzufügen. SQL Server kann Netzwerkverkehr wird von anderen Azure Services.


![Azure zulassen][4]



## <a name="multi-factor-authentication-mfa"></a>Mehrstufige Authentifizierung (MFA)

MFA kann speziell für diese Anwendung aktiviert werden. Wechseln Sie zur Registerkarte Applications von Azure Active Directory. Sie finden einen Eintrag für Microsoft Azure RemoteApp. Wenn Sie auf die Anwendung und konfigurieren, sehen Sie die Seite können Sie MFA für diese Anwendung aktivieren.

![MFA aktivieren][3]



## <a name="audit-user-activity-with-azure-active-directory-premium"></a>Überwachen von Benutzeraktivitäten mit Azure Active Directory

Wenn Sie keinen Azure AD Premium müssen Sie aktivieren im Abschnitt Lizenzen des Verzeichnisses. Mit aktiviert können Benutzer auf Premium zuweisen.

Wenn Sie einem Benutzer in Azure Active Directory wechseln, können Sie dann die Aktivität Registerkarte Anmeldeinformationen Azure RemoteApp wechseln.



## <a name="next-steps"></a>Nächste Schritte

Nach Abschluss aller Schritte, werden Sie in Azure RemoteApp-Client ausführen und mit einem zugewiesenen Benutzer anmelden. Wird als eine Anwendung mit SSMS angezeigt werden, und können wie auf Ihrem Computer Zugriff auf Azure SQL Server-Installation ausführen.

Informationen zum Herstellen die Verbindung mit SQL-Datenbank finden Sie unter [mit SQL-Datenbank mit SQL Server Management Studio und eine T-SQL-Abfrage](sql-database-connect-query-ssms.md).


Das ist alles. Viel Spaß!



<!--Image references-->
[1]: ./media/sql-database-ssms-remoteapp/ssms.png
[2]: ./media/sql-database-ssms-remoteapp/newcloudcollection.png
[3]: ./media/sql-database-ssms-remoteapp/mfa.png
[4]: ./media/sql-database-ssms-remoteapp/allowazure.png
[5]: ./media/sql-database-ssms-remoteapp/publish.png
[6]: ./media/sql-database-ssms-remoteapp/user.png
