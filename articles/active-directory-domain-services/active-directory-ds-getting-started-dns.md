<properties
    pageTitle="Azure Active Directory-Domänendienste: Update DNS-Einstellungen für Azure virtuelle Netzwerk | Microsoft Azure"
    description="Erste Schritte mit Azure Active Directory-Domänendienste"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/21/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services---update-dns-settings-for-the-azure-virtual-network"></a>Azure Active Directory-Domänendienste - DNS Update Standardeinstellungen für Azure virtual network

## <a name="task-4-update-dns-settings-for-the-azure-virtual-network"></a>Aufgabe 4: Aktualisieren DNS-Einstellungen für Azure virtual Network
In den vorangegangenen Aufgaben, haben Sie erfolgreich Azure Active Directory-Domänendienste für das Verzeichnis aktiviert. Die nächste Aufgabe ist sicherzustellen, dass Computer innerhalb des virtuellen Netzwerks können und diese Dienste. Aktualisieren Sie die DNS-servereinstellungen für das virtuelle Netzwerk auf zwei IP-Adressen mit Azure Active Directory-Domänendienste virtuellen Netzwerk verfügbar ist.

> [AZURE.NOTE] Notieren Sie die IP-Adressen für Azure AD-Domänendienste Azure Active Directory-Domänendienste für das Verzeichnis konnten im Register **Konfigurieren** Ihres Verzeichnisses angezeigt.

Führen Sie die folgenden Konfigurationsschritte aktualisieren die DNS-Einstellung für das virtuelle Netzwerk, in dem Azure Active Directory-Domänendienste konnten.

1. Navigieren Sie zu der **Azure-Verwaltungsportal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. Wählen Sie im linken Bereich den Knoten **Netzwerke** .

    ![Virtuelle Netzwerke Knoten](./media/active-directory-domain-services-getting-started/virtual-network-select.png)

3. Wählen Sie auf der Registerkarte **Virtuelle Netzwerke** das virtuelle Netzwerk in dem Azure AD-Domänendienste zur Anzeige der Sicherheitseigenschaften aktiviert.

4. Klicken Sie auf die Registerkarte **Konfigurieren** .

    ![Virtuelle Netzwerke Knoten](./media/active-directory-domain-services-getting-started/virtual-network-configure-tab.png)

5. Geben Sie im Abschnitt **DNS-Server** die IP-Adressen von Azure Active Directory-Domänendienste.

6. Stellen Sie sicher, dass Sie die IP-Adressen eingeben, die im Abschnitt **Domänendienste** Register **Konfigurieren** Ihres Verzeichnisses angezeigt wurden.

7. **Um DNS-Server-Einstellungen für diese virtuelle Netzwerk zu speichern, klicken Sie auf den Aufgabenbereich am unteren Rand der Seite.**

   ![Die DNS-Server-Einstellungen für das virtuelle Netzwerk zu aktualisieren.](./media/active-directory-domain-services-getting-started/update-dns.png)

> [AZURE.NOTE] Nach der Aktualisierung der DNS-servereinstellungen für das virtuelle Netzwerk, dauert es eine Weile für virtuelle Computer im Netzwerk die aktualisierten DNS-Konfiguration. Ein virtuellen Computer zur Domäne herstellen kann, können Sie den DNS-Cache (z. b. nicht löschen "Ipconfig/flushdns") auf dem virtuellen Computer. Dieser Befehl erzwingt eine Aktualisierung des DNS-Clienteinstellungen auf dem virtuellen Computer.


## <a name="task-5---enable-password-synchronization-to-azure-ad-domain-services"></a>Aufgabe 5 - Kennwortsynchronisation aktivieren auf Azure Active Directory-Domänendienste
Die nächste Konfigurationsaufgabe ist [in Azure AD-Domänendienste Kennwortsynchronisation aktivieren](active-directory-ds-getting-started-password-sync.md).
