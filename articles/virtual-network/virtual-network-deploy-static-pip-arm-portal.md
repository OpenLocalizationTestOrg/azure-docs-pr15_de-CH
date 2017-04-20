<properties 
   pageTitle="Bereitstellen eine VM mit öffentlichen Adresse mit Azure-Portal im Ressourcenmanager | Microsoft Azure"
   description="Weitere Informationen zum Bereitstellen von VMs mit öffentlichen Adresse über das Portal Zure Ressourcen-Manager"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/04/2016"
   ms.author="jdial" />

# <a name="deploy-a-vm-with-a-static-public-ip-using-the-azure-portal"></a>Bereitstellen einer VM mit öffentlichen Adresse mit Azure-portal

[AZURE.INCLUDE [virtual-network-deploy-static-pip-arm-selectors-include.md](../../includes/virtual-network-deploy-static-pip-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]Klassische Bereitstellungsmodell.

[AZURE.INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="create-a-vm-with-a-static-public-ip"></a>Erstellen einer VM eine statische öffentliche IP-Adresse 

Gehen Sie folgendermaßen vor um einen virtuellen Computer eine statische öffentliche IP-Adresse im Azure-Portal erstellen.

1. In einem Browser navigieren Sie zum [Azure-Portal](https://portal.azure.com) und melden Sie sich ggf. mit Ihrem Azure-Konto.
2. Klicken Sie in der linken oberen Ecke des Portals auf **neu**>>**berechnen**>**Windows Server 2012 R2 Datacenter**.
3. Wählen Sie in der Liste **Wählen Sie ein Bereitstellungsmodell** **Ressourcenmanager** und klicken Sie auf **Erstellen**.
4. Blatt **Grundlagen** Geben Sie die VM Informationen unten und klicken Sie auf **OK**.

    ![Azure-Portal - Grundlagen](./media/virtual-network-deploy-static-pip-arm-portal/figure1.png)

5. Blatt **auswählen eine Größe** klicken Sie auf **Standard A1** unten und klicken Sie auf **auswählen**.

    ![Azure-Portal - Größe](./media/virtual-network-deploy-static-pip-arm-portal/figure2.png)

6. Blatt **Einstellungen** **öffentliche IP-Adresse**klicken Sie Blatt **öffentliche IP-Adresse** unter **Zuweisung** **statischer** wie unten dargestellt. Und klicken Sie dann auf **OK**.

    ![Azure-Portal - öffentliche IP-Adresse erstellen](./media/virtual-network-deploy-static-pip-arm-portal/figure3.png)

7. Blatt **Einstellungen** klicken Sie auf **OK**.
8. **Überprüfen Sie **Zusammenfassung** -Blade wie folgt**

    ![Azure-Portal - öffentliche IP-Adresse erstellen](./media/virtual-network-deploy-static-pip-arm-portal/figure4.png)

9. Beachten Sie das neue Musterelement auf Ihrem Dashboard.

    ![Azure-Portal - öffentliche IP-Adresse erstellen](./media/virtual-network-deploy-static-pip-arm-portal/figure5.png)

10. Nach Erstellung die VM Blatt **Einstellungen** erscheint wie folgt

    ![Azure-Portal - öffentliche IP-Adresse erstellen](./media/virtual-network-deploy-static-pip-arm-portal/figure6.png)