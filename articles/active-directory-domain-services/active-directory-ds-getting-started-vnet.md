<properties
    pageTitle="Azure Active Directory-Domänendienste: Erstellen, oder wählen Sie ein virtuelles Netzwerk | Microsoft Azure"
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
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="create-or-select-a-virtual-network-for-azure-ad-domain-services"></a>Erstellen Sie oder wählen Sie ein virtuelles Netzwerk für Azure Active Directory-Domänendienste

## <a name="guidelines-to-select-an-azure-virtual-network"></a>Richtlinien für Azure virtuelle Netzwerk auswählen
> [AZURE.NOTE] **Bevor Sie beginnen**: [Aspekte der Azure AD-Domänendienste](active-directory-ds-networking.md)Netzwerk verweisen.


## <a name="task-2-create-an-azure-virtual-network"></a>Aufgabe 2: Erstellen eines virtuellen Azure-Netzwerks
Die nächste Konfigurationsaufgabe ist Azure virtuelles Netzwerk und ein Subnetz erstellen. Azure Active Directory-Domänendienste in diesem Subnet werden innerhalb des virtuellen Netzwerks aktivieren. Wenn Sie bereits ein vorhandenes virtuelles Netzwerk verwenden möchten, können Sie diesen Schritt überspringen.

> [AZURE.NOTE] Stellen Sie sicher, Azure virtuelle Netzwerk erstellen oder mit Azure Active Directory-Domänendienste verwenden eine Azure-Region gehört, die von Azure Active Directory-Domänendienste unterstützt. Siehe [Azure Services nach Region](https://azure.microsoft.com/regions/#services/) kennen Azure Regionen, die Azure AD-Domänendienste verfügbar ist.

Notieren Sie den Namen des virtuellen Netzwerks auswählen im virtuelle Netzwerk bei Azure AD-Domänendienste in nachfolgenden Konfigurationsschritt aktivieren.

Führen Sie die folgenden Konfigurationsschritte zum Azure virtuelles Netzwerk erstellen in dem Azure Active Directory-Domänendiensten aktivieren möchten.

1. Navigieren Sie zu der **Azure-Verwaltungsportal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. Wählen Sie im linken Bereich den Knoten **Netzwerke** .

    ![Knoten Netzwerke](./media/active-directory-domain-services-getting-started/networks-node.png)

3. Klicken Sie im Aufgabenbereich am unteren Rand der Seite auf **neu** .

    ![Virtuelle Netzwerke Knoten](./media/active-directory-domain-services-getting-started/virtual-networks.png)

4. Wählen Sie im Knoten **Netzwerkdienste** **Virtuelle Netzwerk**.

5. Klicken Sie zum Erstellen eines virtuellen Netzwerks **Schnell erstellen** .

    ![Virtuelle Netzwerk - quick](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)

6. Geben Sie einen **Namen** für das virtuelle Netzwerk. Sie können auch den **Adressraum** oder **maximale VM-Anzahl** für dieses Netzwerk konfigurieren. Jetzt können Sie die **DNS** -Einstellung auf 'None' festgelegt lassen. Sie können den DNS-Server nach Ihrem Azure AD-Domäne aktivieren Dienste festlegen aktualisieren.

7. Sicherstellen Sie, dass eine unterstützte Azure Region in der Dropdownliste **Speicherort** auswählen. Siehe [Azure Services nach Region](https://azure.microsoft.com/regions/#services/) kennen Azure Regionen, die Azure AD-Domänendienste verfügbar ist.

8. Um das virtuelle Netzwerk zu erstellen, klicken Sie auf **ein virtuelles Netzwerk erstellen** .

    ![Erstellen Sie ein virtuelles Netzwerk für Azure Active Directory-Domänendienste.](./media/active-directory-domain-services-getting-started/create-vnet.png)

9. Nach dem Erstellen des virtuellen Netzwerks wählen Sie das virtuelle Netzwerk, und klicken Sie auf die Registerkarte **Konfigurieren** .

    ![Erstellen Sie ein Subnetz](./media/active-directory-domain-services-getting-started/create-vnet-properties.png)

10. Navigieren Sie zum Abschnitt **Adressräume virtuelles Netzwerk** . Auf **Subnetz hinzufügen** , und geben Sie ein Subnetz mit dem Namen **AaddsSubnet**. Klicken Sie auf **Speichern** , um das Subnetz erstellen.

    ![Erstellen Sie ein Subnetz für Azure Active Directory-Domänendienste.](./media/active-directory-domain-services-getting-started/create-vnet-add-subnet.png)


<br>

## <a name="task-3---enable-azure-ad-domain-services"></a>Aufgabe 3: Aktivieren Azure Active Directory-Domänendienste
Die nächste Konfigurationsaufgabe ist [Azure AD-Domäne ermöglichen](active-directory-ds-getting-started-enableaadds.md).
