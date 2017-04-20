<properties 
    pageTitle="Windows-Authentifizierung und Azure Multi-Factor Authentication Server"
    description="Dies ist die Azure mehrstufige Authentifizierungsseite, die Bereitstellung von Windows-Authentifizierung und Azure mehrstufige Authentifizierungsserver unterstützen."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="windows-authentication-and-azure-multi-factor-authentication-server"></a>Windows-Authentifizierung und Azure Multi-Factor Authentication Server

Im Abschnitt Windows-Authentifizierung kann der Administrator aktivieren und konfigurieren Windows-Authentifizierung für ein oder mehrere Programme.  Folgendes ist eine Liste der Dinge beachten Sie vor dem Einrichten von Windows-Authentifizierung.

-  Neustart ist erforderlich, bevor Azure mehrstufige Authentifizierung für Terminaldienste aktiviert werden.
-  Wenn 'Azure mehrstufige Authentifizierung müssen Benutzer Match' aktiviert ist und Sie nicht in der Liste, werden Sie nach dem Neustart des Computers Anmelden nicht.
-  Vertrauenswürdige IP-Adressen ist abhängig davon, ob die Anwendung die Client-IP-die Authentifizierung bereitstellen kann. Derzeit wird nur Terminaldienste wird unterstützt.  







>[AZURE.NOTE]Dieses Feature wird nicht sicheren Terminaldienste unter Windows Server 2012 R2 unterstützt.




## <a name="to-secure-an-application-with-windows-authentication-use-the-following-procedure"></a>Gehen Sie folgendermaßen vor, um eine Anwendung mit Windows-Authentifizierung zu sichern.

1. Klicken Sie in Azure mehrstufige Authentifizierungsserver auf Windows-Authentifizierung.
![Windows-Authentifizierung](./media/multi-factor-authentication-get-started-server-windows/windowsauth.png)
2. Kontrollkästchen Sie das Aktivieren von Windows-Authentifizierung. Dieses Kontrollkästchen ist standardmäßig deaktiviert.
3. Die Registerkarte Applications kann der Administrator eine oder mehrere Windows-Authentifizierung konfigurieren.
4. Wählen Sie einen Server oder Anwendung – angeben, ob die Server-Anwendung aktiviert ist. Klicken Sie auf OK.
5. Klicken Sie auf Hinzufügen... Schaltfläche.
6. Die Registerkarte Vertrauenswürdige IP-Adressen können Sie Azure Mehrfaktor-Authentifizierung für Windows-Sitzung aus bestimmten IPs überspringen. Zum Beispiel wenn Mitarbeiter die Anwendung im Büro und zuhause verwenden, könnten Sie ihre Telefone für Azure mehrstufige Authentifizierung im Büro klingeln soll. Dazu würden Sie Office Subnetz vertrauenswürdige IP-Adressen eingeben.
7. Klicken Sie auf Hinzufügen... Schaltfläche.
8. Wählen Sie einzelne IP-Adresse, wenn eine einzelne IP-Adresse zu überspringen.
9. Wählen Sie IP-gesamten IP-Bereich überspringen möchten. Beispiel 10.63.193.1-10.63.193.100.
10. Wählen Sie Subnetz einen Bereich von IP-Adressen Subnetz Schreibweise angeben möchten. Geben Sie das Subnetz Start-IP- und wählen Sie die geeignete Netzmaske aus der Dropdown-Liste aus.
11. Klicken Sie auf die Schaltfläche OK.
