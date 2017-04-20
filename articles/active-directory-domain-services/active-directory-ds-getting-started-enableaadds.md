<properties
    pageTitle="Azure Active Directory-Domänendienste: Azure Active Directory-Domänendiensten aktivieren | Microsoft Azure"
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
    ms.date="10/19/2016"
    ms.author="maheshu"/>

# <a name="enable-azure-ad-domain-services"></a>Aktivieren von Azure Active Directory-Domänendienste

## <a name="task-3-enable-azure-ad-domain-services"></a>Aufgabe 3: Azure Active Directory-Domänendiensten aktivieren
In dieser Aufgabe können Sie Azure Active Directory-Domänendienste für das Verzeichnis. Die folgenden Schritte Konfiguration Azure Active Directory-Domänendienste für das Verzeichnis aktiviert.

1. Navigieren Sie zu der **Azure-Verwaltungsportal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. **Active Directory** -Knoten im linken Bereich auswählen.

3. Wählen Sie den Azure AD-Mandanten (Verzeichnis) für den Azure Active Directory-Domänendiensten aktivieren möchten.

    ![Wählen Sie Azure AD-Verzeichnis](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. Klicken Sie auf die Registerkarte **Konfigurieren** .

    ![Registerkarte Verzeichnis konfigurieren](./media/active-directory-domain-services-getting-started/configure-tab.png)

5. Blättern Sie zum Abschnitt **Domänendienste**.

    ![Domain Services Konfigurationsabschnitt](./media/active-directory-domain-services-getting-started/domain-services-configuration.png)

6. Schaltet die Option **Domäne dieses Verzeichnis aktiviert** auf **Ja**. Beachten Sie einige weitere Konfigurationsoptionen für Azure Active Directory-Domänendiensten auf der Seite angezeigt.

    ![Domänendienste aktivieren](./media/active-directory-domain-services-getting-started/enable-domain-services.png)

    > [AZURE.NOTE] Beim Aktivieren von Azure Active Directory-Domänendienste für Ihren Mandanten Azure AD generiert und speichert die Kerberos und NTLM-Anmeldeinformationen Hashes, die zum Authentifizieren von Benutzern erforderlich sind.

7. Geben Sie den **DNS-Domänennamen der Domänendienste**.

   - Der Standard-Domänenname des Verzeichnisses (d. h. bis die **. onmicrosoft.com** Domänensuffix) ist standardmäßig aktiviert.

   - Die Liste enthält alle Domänen einschließlich überprüft für Ihre Azure AD-Verzeichnis konfiguriert wurde als auch nicht überprüften Domänen, die in der Registerkarte 'Domänen' konfiguriert.

   - Darüber hinaus können Sie auch einen benutzerdefinierten Domänennamen eingeben. In diesem Beispiel haben wir einen benutzerdefinierten Domänennamen 'contoso100.com' eingegeben.

     > [AZURE.WARNING] Sicherstellen Sie, dass das Präfix Domäne den Domänennamen (z. B. 'contoso100', 'contoso100.com'-Domänenname angeben) weniger als 15 Zeichen. Sie können keine Domäne Azure Active Directory Domain Services mit mehr als 15 Zeichen Domäne erstellen.

8. Stellen Sie sicher, dass der DNS-Domänenname der verwalteten Domäne gewählten virtuellen Netzwerks nicht bereits vorhanden ist. Überprüfen Sie insbesondere:

   - Sie haben bereits eine Domäne mit demselben DNS-Domänennamen für das virtuelle Netzwerk.

   - das virtuelle Netzwerk ausgewählten hat eine VPN-Verbindung mit dem lokalen Netzwerk und Sie einer Domäne mit dem DNS-Domänennamen im lokalen Netzwerk.

   - Sie haben einen vorhandenen Cloud-Dienst mit diesem Namen für das virtuelle Netzwerk.

9. Im nächste Schritt wird ein virtuelles Netzwerk wählen Sie in dem Azure AD-Domänendienste verfügbar sein sollen. Wählen Sie virtuellen Netzwerk und spezielle Subnetz erstellt in der Dropdown-Liste mit dem Titel **Domänendienste in diesem virtuellen Netzwerk verbinden**.

   - Sicherstellen Sie, dass das virtuelle Netzwerk angegebene einer Azure-Region von Azure Active Directory-Domänendienste unterstützt gehört. Verweisen Sie auf die [Region Azure Services](https://azure.microsoft.com/regions/#services/) kennen Azure Regionen, die Azure AD-Domänendienste verfügbar ist.

   - Virtuelle Netzwerke in einer Region, Azure Active Directory-Domänendienste nicht unterstützt, werden nicht in der Liste angezeigt.
   
   - Verwenden Sie einen dedizierten Subnetz innerhalb des virtuellen Netzwerks für Azure Active Directory-Domänendienste. Sicherstellen Sie, dass das gatewaysubnetz nicht aktivieren. Finden Sie unter [networking Considerations](active-directory-ds-networking.md). 

   - Virtuelle Netzwerke mit Azure-Ressourcen-Manager erstellt wurden, erscheinen auch nicht in der Dropdown-Liste. Ressourcenmanager-basierte virtuelle Netzwerke werden von Azure Active Directory-Domänendienste derzeit nicht unterstützt.

10. **Um Azure Active Directory-Domänendienste zu aktivieren, klicken Sie im Aufgabenbereich am unteren Rand der Seite.**

11. Die Seite zeigt eine ' Ausstehend... " Zustand während Azure Active Directory-Domänendienste für das Verzeichnis aktiviert ist.

    ![Aktivieren der Domänendienste - aussteht](./media/active-directory-domain-services-getting-started/enable-domain-services-pendingstate.png)

    > [AZURE.NOTE] Azure Active Directory-Domänendienste bietet hohe Verfügbarkeit für Ihre verwalteten Domäne. Nach dem Aktivieren von Azure Active Directory-Domänendienste Beachten Sie IP-Adressen mit denen Domänendienste nacheinander auf das virtuelle Netzwerk verfügbar sind. Sobald der Dienst hohen Verfügbarkeit für Ihre Domäne ermöglicht wird, die zweite IP-Adresse angezeigt. Wenn hoher Verfügbarkeit konfiguriert und für Ihre Domäne aktiv ist, erhalten Sie zwei IP-Adressen im Abschnitt **Domänendienste** Registerkarte **Konfigurieren** .

12. Nach ca. 20-30 Minuten sehen Sie die erste IP-Adresse mit der Domänendienste im virtuellen Netzwerk im Feld **IP-Adresse** auf der Seite **Konfigurieren** .

    ![Aktiviert - Domänendienste ersten IP-Adresse bereitgestellt](./media/active-directory-domain-services-getting-started/domain-services-enabled-firstdc-available.png)

13. Wenn hohe Verfügbarkeit für Ihre Domäne ist, sehen Sie zwei IP-Adressen auf der Seite angezeigt. Der verwalteten Domäne ist im ausgewählten virtuellen Netzwerk auf diese zwei IP-Adressen verfügbar. Notieren Sie die IP-Adressen aktualisieren können die DNS-Einstellungen für das virtuelle Netzwerk. Hierdurch können die virtuellen Computer im virtuellen Netzwerk Verbindung zu der Domäne wie Domänenbeitritt.

    ![Domänendienste aktiviert – beide IP-Adressen bereitgestellt](./media/active-directory-domain-services-getting-started/domain-services-enabled-bothdcs-available.png)

> [AZURE.NOTE] Je nach Ihrem Azure AD-Mandanten (Anzahl der Benutzer, Gruppen usw.), Synchronisierung der verwalteten Domäne dauert eine Weile. Diese Synchronisation erfolgt im Hintergrund. Für große Mieter mit Zehntausenden von Objekten dauert es einen Tag oder für alle Benutzer, Gruppenmitgliedschaften und Anmeldeinformationen synchronisiert werden.

<br>

## <a name="task-4---update-dns-settings-for-the-azure-virtual-network"></a>Aufgabe 4 - Update DNS-Einstellungen für Azure virtual Network
Die nächste Konfigurationsaufgabe ist [die DNS-Clienteinstellungen für Azure virtuelle Netzwerk aktualisieren](active-directory-ds-getting-started-dns.md).
