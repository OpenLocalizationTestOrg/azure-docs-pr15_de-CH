<properties
   pageTitle="Sapnetweaver auf Windows virtuelle Maschinen (VMs) - Handbuch für hohe Verfügbarkeit | Microsoft Azure"
   description="Hochverfügbarkeits-Leitfaden für SAP NetWeaver virtuelle Windows-Maschinen"
   services="virtual-machines-windows,virtual-network,storage"
   documentationCenter="saponazure"
   authors="goraco"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/18/2016"
   ms.author="goraco"/>

# <a name="sap-netweaver-on-windows-virtual-machines-vms---high-availability-guide"></a>Sapnetweaver auf Windows virtuelle Maschinen (VMs) - Handbuch für hohe Verfügbarkeit

[767598]:https://service.sap.com/sap/support/notes/767598
[773830]:https://service.sap.com/sap/support/notes/773830
[826037]:https://service.sap.com/sap/support/notes/826037
[965908]:https://service.sap.com/sap/support/notes/965908
[1031096]:https://service.sap.com/sap/support/notes/1031096
[1139904]:https://service.sap.com/sap/support/notes/1139904
[1173395]:https://service.sap.com/sap/support/notes/1173395
[1245200]:https://service.sap.com/sap/support/notes/1245200
[1409604]:https://service.sap.com/sap/support/notes/1409604
[1558958]:https://service.sap.com/sap/support/notes/1558958
[1585981]:https://service.sap.com/sap/support/notes/1585981
[1588316]:https://service.sap.com/sap/support/notes/1588316
[1590719]:https://service.sap.com/sap/support/notes/1590719
[1597355]:https://service.sap.com/sap/support/notes/1597355
[1605680]:https://service.sap.com/sap/support/notes/1605680
[1619720]:https://service.sap.com/sap/support/notes/1619720
[1619726]:https://service.sap.com/sap/support/notes/1619726
[1619967]:https://service.sap.com/sap/support/notes/1619967
[1750510]:https://service.sap.com/sap/support/notes/1750510
[1752266]:https://service.sap.com/sap/support/notes/1752266
[1757924]:https://service.sap.com/sap/support/notes/1757924
[1757928]:https://service.sap.com/sap/support/notes/1757928
[1758182]:https://service.sap.com/sap/support/notes/1758182
[1758496]:https://service.sap.com/sap/support/notes/1758496
[1772688]:https://service.sap.com/sap/support/notes/1772688
[1814258]:https://service.sap.com/sap/support/notes/1814258
[1882376]:https://service.sap.com/sap/support/notes/1882376
[1909114]:https://service.sap.com/sap/support/notes/1909114
[1922555]:https://service.sap.com/sap/support/notes/1922555
[1928533]:https://service.sap.com/sap/support/notes/1928533
[1941500]:https://service.sap.com/sap/support/notes/1941500
[1956005]:https://service.sap.com/sap/support/notes/1956005
[1973241]:https://service.sap.com/sap/support/notes/1973241
[1984787]:https://service.sap.com/sap/support/notes/1984787
[1999351]:https://service.sap.com/sap/support/notes/1999351
[2002167]:https://service.sap.com/sap/support/notes/2002167
[2015553]:https://service.sap.com/sap/support/notes/2015553
[2039619]:https://service.sap.com/sap/support/notes/2039619
[2121797]:https://service.sap.com/sap/support/notes/2121797
[2134316]:https://service.sap.com/sap/support/notes/2134316
[2178632]:https://service.sap.com/sap/support/notes/2178632
[2191498]:https://service.sap.com/sap/support/notes/2191498
[2233094]:https://service.sap.com/sap/support/notes/2233094
[2243692]:https://service.sap.com/sap/support/notes/2243692

[sap-installation-guides]:http://service.sap.com/instguides

[azure-cli]:../xplat-cli-install.md
[azure-portal]:https://portal.azure.com
[azure-ps]:../powershell-install-configure.md
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../azure-subscription-service-limits.md#subscription

[Dbms-guide]:virtual-machines-windows-sap-dbms-guide.md (SAP NetWeaver auf Windows virtuelle Maschinen (VMs) – Bereitstellungshandbuch DBMS) [Dbms-guide-2.1]:virtual-machines-windows-sap-dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f (Zwischenspeichern für VMs und VHDs) [Dbms-guide-2.2]:virtual-machines-windows-sap-dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 (Software-RAID) [Dbms-guide-2.3]:virtual-machines-windows-sap-dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 (Microsoft Azure Storage) [Dbms-guide-2]:virtual-machines-windows-sap-dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 (Struktur einer Bereitstellung RDBMS) [Dbms-guide-3]:virtual-machines-windows-sap-dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 (hohe Verfügbarkeit und Disaster Recovery mit Azure VMs) [dbms-guide-5.5.1]:virtual-machines-windows-sap-dbms-guide.md# 0fef0e79-d3fe-4ae2-85AF-73666a6f7268 (SQL Server 2012 SP1 CU4 und höher) [Dbms-guide-5.5.2]:virtual-machines-windows-sap-dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b (SQL Server 2012 SP1 CU3 und früheren Versionen) [Dbms-guide-5.6]:virtual-machines-windows-sap-dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 (mit einer SQL Server-Bilder aus Microsoft Azure Marketplace) [Dbms-guide-5.8]:virtual-machines-windows-sap-dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 (Allgemeine SQL Server für SAP Azure Zusammenfassung) [Dbms-guide-5]:virtual-machines-windows-sap-dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 (Einzelheiten zum SQL Server-RDBMS) [Dbms-guide-8.4.1]:virtual-machines-windows-sap-dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 (Speicherkonfiguration) [dbms-guide-8.4.2]:virtual-machines-windows-sap-dbms-guide.md# 23c78d3b-ca5a-4e72-8a24-645d141a3f5d (Sicherung und Wiederherstellung) [Dbms-guide-8.4.3]:virtual-machines-windows-sap-dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c (Leistungsaspekte für Sicherung und Wiederherstellung) [Dbms-guide-8.4.4]:virtual-machines-windows-sap-dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 (andere) [Dbms-guide-900-sap-cache-server-on-premises]:virtual-machines-windows-sap-dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:./media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:./media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:./media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:./media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:./media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:./media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:./media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:./media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:./media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[Deployment-guide]:virtual-machines-windows-sap-deployment-guide.md (SAP NetWeaver auf Windows virtuelle Maschinen (VMs) – Bereitstellungshandbuch) [Deployment-guide-2.2]:virtual-machines-windows-sap-deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 (SAP Ressourcen) [Deployment-guide-3.1.2]:virtual-machines-windows-sap-deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab (Bereitstellen eines virtuellen Computers durch ein benutzerdefiniertes Bild) [deployment-guide-3.2]:virtual-machines-windows-sap-deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 (Szenario 1: Bereitstellen eines virtuellen Computers Azure Markt für SAP) [deployment-guide-3.3]:virtual-machines-windows-sap-deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 (Szenario 2: Bereitstellen eines virtuellen Computers durch ein benutzerdefiniertes Bild für SAP) [deployment-guide-3.4]:virtual-machines-windows-sap-deployment-guide.md# a9a60133-a763-4de8-8986-ac0fa33aa8c1 (Szenario 3: Verschieben einer VM lokal eine virtuelle Festplatte nicht verallgemeinert Azure mit SAP) [Deployment-guide-3]:virtual-machines-windows-sap-deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e (Bereitstellung Szenarien der VMs für SAP auf Microsoft Azure) [Deployment-guide-4.1]:virtual-machines-windows-sap-deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 (Bereitstellen von Azure PowerShell-Cmdlets) [Deployment-guide-4.2]:virtual-machines-windows-sap-deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e (Download und Import SAP relevanten PowerShell-Cmdlets) [Deployment-guide-4.3]:virtual-machines-windows-sap-deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc (Join VM in der lokalen Domäne - nur Windows) [Deployment-guide-4.4.2]:virtual-machines-windows-sap-deployment-guide.md# 6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 (Linux) [Deployment-guide-4.4]:virtual-machines-windows-sap-deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d (herunterladen, installieren und aktivieren Azure VM Agent) [Deployment-guide-4.5.1]:virtual-machines-windows-sap-deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 (Azure PowerShell) [Deployment-guide-4.5.2]:virtual-machines-windows-sap-deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f (Azure CLI) [Deployment-guide-4.5]:virtual-machines-windows-sap-deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca (Konfigurieren Azure erweiterte Überwachung Erweiterung für SAP) [Deployment-guide-5.1]:virtual-machines-windows-sap-deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 (Readiness überprüfen Azure erweiterte Überwachung von SAP) [Deployment Guide 5.2]: Virtual-Machines-Windows-SAP-Deployment-Guide.MD#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1 (Health Check für Azure Infrastruktur Überwachungskonfiguration) [Deployment-guide-5.3]:virtual-machines-windows-sap-deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 (Weitere Problembehandlung Überwachen der Azure-Infrastruktur für SAP)

[deployment-guide-configure-monitoring-scenario-1]:virtual-machines-windows-sap-deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b (Configure Monitoring)
[deployment-guide-configure-proxy]:virtual-machines-windows-sap-deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d (Configure Proxy)
[deployment-guide-figure-100]:./media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:./media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:virtual-machines-windows-sap-deployment-guide.md#figure-11
[deployment-guide-figure-1100]:./media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:./media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:./media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:virtual-machines-windows-sap-deployment-guide.md#figure-14
[deployment-guide-figure-1400]:./media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:./media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:./media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:virtual-machines-windows-sap-deployment-guide.md#figure-5
[deployment-guide-figure-50]:./media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:./media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:virtual-machines-windows-sap-deployment-guide.md#figure-6
[deployment-guide-figure-600]:./media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:virtual-machines-windows-sap-deployment-guide.md#figure-7
[deployment-guide-figure-700]:./media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:./media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:./media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:virtual-machines-windows-sap-deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:virtual-machines-windows-sap-deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:virtual-machines-windows-sap-deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:virtual-machines-windows-sap-deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b (Checks and Troubleshooting for End-to-End Monitoring Setup for SAP on Azure)

[deploy-template-cli]:../resource-group-template-deploy.md#deploy-with-azure-cli-for-mac-linux-and-windows
[deploy-template-portal]:../resource-group-template-deploy.md#deploy-with-the-preview-portal
[deploy-template-powershell]:../resource-group-template-deploy.md#deploy-with-powershell

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:virtual-machines-windows-sap-get-started.md
[getting-started-dbms]:virtual-machines-windows-sap-get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:virtual-machines-windows-sap-get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:virtual-machines-windows-sap-get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056

[ha-guide]:virtual-machines-windows-sap-high-availability-guide.md

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:./media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:./media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[Planning-guide]:virtual-machines-windows-sap-planning-guide.md (SAP NetWeaver auf Windows virtuelle Maschinen (VMs) – Planung und Implementierungshandbuch) [Planning-guide-1.2]:virtual-machines-windows-sap-planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff (Ressourcen) [Planning-guide-11]:virtual-machines-windows-sap-planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 (High Availability (HA) und Disaster Recovery (DR) für SAP NetWeaver auf Azure-Computer ausgeführt) [Planning-guide-11.4.1]:virtual-machines-windows-sap-planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 (hohe Verfügbarkeit für SAP-Anwendungsserver) [Planning-guide-11.5]:virtual-machines-windows-sap-planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f (Autostart für SAP-Instanzen verwenden) [planning-guide-2.1]:virtual-machines-windows-sap-planning-guide.md# 1625df66-4cc6-4d60-9202-de8a0b77f803 (nur Cloud - VM-Bereitstellung in Azure ohne Abhängigkeit von lokalen Kundennetzwerk) [Planning-guide-2.2]:virtual-machines-windows-sap-planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 (Cross-lokal - Bereitstellung von einem oder mehreren SAP VMs in Azure mit dem lokalen Netzwerk vollständig integriert wird) [Planning-guide-3.1]:virtual-machines-windows-sap-planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a (Azure Regionen) [Planning-guide-3.2.1]:virtual-machines-windows-sap-planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 (Fehlerdomänen) [Planning-guide-3.2.2]:virtual-machines-windows-sap-planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 (Domänen aktualisieren) [Planning-guide-3.2.3]:virtual-machines-windows-sap-planning-guide.md#18810088- f9be - 4c 97-958a - 27996255c 665 (Azure Verfügbarkeit Sets) [Planning-guide-3.2]:virtual-machines-windows-sap-planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 (der Microsoft Azure Virtual Machine-Konzept) [Planning-guide-3.3.2]:virtual-machines-windows-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium Speicher) [Planning-guide-5.1.1]:virtual-machines-windows-sap-planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 (Verschieben einer VM lokal in Azure mit einer Diskette nicht verallgemeinert) [Planning-guide-5.1.2]:virtual-machines-windows-sap-planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c (Bereitstellen eines virtuellen Computers mit einem bestimmten Kunden-Image) [planning-guide-5.2.1]:virtual-machines-windows-sap-planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef (Vorbereitung zum Verschieben einer VM lokal in Azure mit einer Datenträger nicht verallgemeinert) [Planning-guide-5.2.2]:virtual-machines-windows-sap-planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 (Vorbereitung Bereitstellen eines virtuellen Computers mit einer bestimmten Kunden für SAP) [Planning-guide-5.2]:virtual-machines-windows-sap-planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 (VMs mit SAP für Azure vorbereiten) [Planning-guide-5.3.1]:virtual-machines-windows-sap-planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 (Unterschied zwischen einer Azure und Azure Abbildern) [Planning-guide-5.3.2]:virtual-machines-windows-sap-planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a (VHD lokal in Azure hochladen) [Planning-guide-5.4.2]:virtual-machines-windows-sap-planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 (Kopieren von Festplatten zwischen Azure-Speicherkonten) [Planung-Handbuch-5.5.1]: Virtual-Machines-Windows-SAP-Planning-Guide.MD#4efec401-91e0-40c0-8e64-f2dceadff646 (VM-VHD Struktur für SAP-Installationen) [Planning-guide-5.5.3]:virtual-machines-windows-sap-planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d (Einstellung Automount für angeschlossene Laufwerke) [Planning-guide-7.1]:virtual-machines-windows-sap-planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 (einzelne VM mit SAP NetWeaver Demo-Schulung Szenario) [Planning-guide-7]:virtual-machines-windows-sap-planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 (Konzepte Cloud-Only Bereitstellung von SAP-Instanzen) [Planning-guide-9.1]:virtual-machines-windows-sap-planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 (Azure Monitoring-Lösung für SAP) [planning-guide-azure-premium-storage]:virtual-machines-windows-sap-planning-guide.md# ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium-Speicher)

[planning-guide-figure-100]:./media/virtual-machines-shared-sap-planning-guide/100-single-vm-in-azure.png
[planning-guide-figure-1300]:./media/virtual-machines-shared-sap-planning-guide/1300-ref-config-iaas-for-sap.png
[planning-guide-figure-1400]:./media/virtual-machines-shared-sap-planning-guide/1400-attach-detach-disks.png
[planning-guide-figure-1600]:./media/virtual-machines-shared-sap-planning-guide/1600-firewall-port-rule.png
[planning-guide-figure-1700]:./media/virtual-machines-shared-sap-planning-guide/1700-single-vm-demo.png
[planning-guide-figure-1900]:./media/virtual-machines-shared-sap-planning-guide/1900-vm-set-vnet.png
[planning-guide-figure-200]:./media/virtual-machines-shared-sap-planning-guide/200-multiple-vms-in-azure.png
[planning-guide-figure-2100]:./media/virtual-machines-shared-sap-planning-guide/2100-s2s.png
[planning-guide-figure-2200]:./media/virtual-machines-shared-sap-planning-guide/2200-network-printing.png
[planning-guide-figure-2300]:./media/virtual-machines-shared-sap-planning-guide/2300-sapgui-stms.png
[planning-guide-figure-2400]:./media/virtual-machines-shared-sap-planning-guide/2400-vm-extension-overview.png
[planning-guide-figure-2500]:./media/virtual-machines-shared-sap-planning-guide/2500-vm-extension-details.png
[planning-guide-figure-2600]:./media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:./media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:./media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:./media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-300]:./media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:./media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:./media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-400]:./media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:./media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:./media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:./media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:virtual-machines-windows-sap-planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd (Microsoft Azure Networking)
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:virtual-machines-windows-sap-planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f (Storage: Microsoft Azure Storage and Data Disks)

[sap-ha-guide]:virtual-machines-windows-sap-high-availability-guide.md (High-availability SAP NetWeaver on Windows virtual machines)
[sap-ha-guide-1]:virtual-machines-windows-sap-high-availability-guide.md#217c5479-5595-4cd8-870d-15ab00d4f84c (Prerequisites)
[sap-ha-guide-2]:virtual-machines-windows-sap-high-availability-guide.md#42b8f600-7ba3-4606-b8a5-53c4f026da08 (Resources)
[sap-ha-guide-3]:virtual-machines-windows-sap-high-availability-guide.md#42156640c6-01cf-45a9-b225-4baa678b24f1 (High-availability SAP with Azure Resource Manager vs. the classic deployment model)
[sap-ha-guide-3.1]:virtual-machines-windows-sap-high-availability-guide.md#f76af273-1993-4d83-b12d-65deeae23686 (Resource groups)
[sap-ha-guide-3.2]:virtual-machines-windows-sap-high-availability-guide.md#3e85fbe0-84b1-4892-87af-d9b65ff91860 (Clustering with Azure Resource Manager vs. the classic deployment model)
[sap-ha-guide-4]:virtual-machines-windows-sap-high-availability-guide.md#8ecf3ba0-67c0-4495-9c14-feec1a2255b7 (Windows Server Failover Clustering)
[sap-ha-guide-4.1]:virtual-machines-windows-sap-high-availability-guide.md#1a3c5408-b168-46d6-99f5-4219ad1b1ff2 (Quorum modes)
[sap-ha-guide-5]:virtual-machines-windows-sap-high-availability-guide.md#fdfee875-6e66-483a-a343-14bbaee33275 (Windows Server Failover Clustering on-premises)
[sap-ha-guide-5.1]:virtual-machines-windows-sap-high-availability-guide.md#be21cf3e-fb01-402b-9955-54fbecf66592 (Shared storage)
[sap-ha-guide-5.2]:virtual-machines-windows-sap-high-availability-guide.md#ff7a9a06-2bc5-4b20-860a-46cdb44669cd (Networking and name resolution)
[sap-ha-guide-6]:virtual-machines-windows-sap-high-availability-guide.md#2ddba413-a7f5-4e4e-9a51-87908879c10a (Windows Server Failover Clustering in Azure)
[sap-ha-guide-6.1]:virtual-machines-windows-sap-high-availability-guide.md#1a464091-922b-48d7-9d08-7cecf757f341 (Shared disk in Azure with SIOS DataKeeper)
[sap-ha-guide-6.2]:virtual-machines-windows-sap-high-availability-guide.md#44641e18-a94e-431f-95ff-303ab65e0bcb (Name resolution in Azure)
[Sap-ha-guide-7]:virtual-machines-windows-sap-high-availability-guide.md#2e3fec50-241e-441b-8708-0b1864f66dfa (hohe Verfügbarkeit in Azure Infrastructure-as-a-Service (IaaS) SAP NetWeaver) [Sap-ha-guide-7.1]:virtual-machines-windows-sap-high-availability-guide.md#93faa747-907e-440a-b00a-1ae0a89b1c0e (hoher Verfügbarkeit SAP-Anwendungsserver) [Sap-ha-guide-7.2]:virtual-machines-windows-sap-high-availability-guide.md#f559c285-ee68-4eec-add1-f60fe7b978db (hoher Verfügbarkeit SAP ASC/SCS-Instanz) [Sap-ha-guide-7.2.1]:virtual-machines-windows-sap-high-availability-guide.md#b5b1fd0b-1db4-4d49-9162-de07a0132a51 (hoher Verfügbarkeit SAP ASC/SCS-Instanz mit Windows Server Failover Clustering in Azure) [sap-ha-guide-7.3]:virtual-machines-windows-sap-high-availability-guide.md#ddd878a0-9c2f-4b8e-8968-26ce60be1027 () High-Availability DBMS-Instanz) [Sap-ha-guide-7.4]:virtual-machines-windows-sap-high-availability-guide.md#045252ed-0277-4fc8-8f46-c5a29694a816 (End-to-End-Hochverfügbarkeits-Bereitstellungsszenarien) [Sap-ha-guide-8]:virtual-machines-windows-sap-high-availability-guide.md#78092dbe-165b-454c-92f5-4972bdbef9bf (Vorbereiten der Infrastruktur) [Sap-ha-guide-8.1]:virtual-machines-windows-sap-high-availability-guide.md#c87a8d3f-b1dc-4d2f-b23c-da4b72977489 (Bereitstellen virtueller Maschinen mit Unternehmensnetzwerk (standortübergreifende) in der Produktion verwenden) [Sap-ha-guide-8.2]:virtual-machines-windows-sap-high-availability-guide.md#7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310 (Cloud nur Bereitstellung von SAP-Instanzen für Test und Demo) [sap-ha-guide-8.3]:virtual-machines-windows-sap-high-availability-guide.md#47d5300a-a830-41d4-83dd-1a0d1ffdbe6a () Azure Virtual Network) [Sap-ha-guide-8.4]:virtual-machines-windows-sap-high-availability-guide.md#b22d7b3b-4343-40ff-a319-097e13f62f9e (DNS IP Adressen) [Sap-ha-guide-8.5]:virtual-machines-windows-sap-high-availability-guide.md#9fbd43c0-5850-4965-9726-2a921d85d73f (Hostnamen und statische IP-Adressen für die Clusterinstanz SAP ASC/SCS und Clusterinstanz DBMS) [Sap-ha-guide-8.6]:virtual-machines-windows-sap-high-availability-guide.md#84c019fe-8c58-4dac-9e54-173efd4b2c30 (Set static IP-Adressen für virtuelle SAP-Maschinen) [Sap-ha-guide-8.7]:virtual-machines-windows-sap-high-availability-guide.md#7a8f3e9b-0624-4051-9e41-b73fff816a9e (erstellen Sie eine statische IP-Adresse für den internen Lastenausgleich) [Sap-ha-guide-8.8]:virtual-machines-windows-sap-high-availability-guide.md#f19bd997-154d-4583-a46e-7f5a69d0153c (Default ASC-SCS laden Regeln für interne Lastenausgleich Azure Netzwerklastenausgleich) [Sap-ha-guide-8.9]:virtual-machines-windows-sap-high-availability-guide.md#fe0bd8b5-2b43-45e3-8295-80bee5415716 (ASC-SCS Standard Lastenausgleich für Azure internen Lastenausgleich Regeln ändern) [Sap-ha-guide-8.10]:virtual-machines-windows-sap-high-availability-guide.md#e69e9a34-4601-47a3-a41c-d2e11c626c0c (Hinzufügen Windows virtuelle Maschinen zur Domäne) [Sap-ha-guide-8.11]:virtual-machines-windows-sap-high-availability-guide.md#661035b2-4d0f-4d31-86f8-dc0a50d78158 (Registrierungseinträge hinzufügen auf beiden Clusterknoten SAP ASC/SCS-Instanz) [Sap-ha-guide-8.12]:virtual-machines-windows-sap-high-availability-guide.md#0d67f090-7928-43e0-8772-5ccbf8f59aab (Einrichten eines Clusters Windows Server Failover Clustering für SAP ASC/SCS-Instanz) [Sap-ha-Handbuch-8.12.1] : virtual-machines-windows-sap-high-availability-guide.md#5eecb071-c703-4ccc-ba6d-fe9c6ded9d79 (sammeln Clusterknoten in einer Clusterkonfiguration) [sap-ha-guide-8.12.2]:virtual-machines-windows-sap-high-availability-guide.md#e49a4529-50c9-4dcf-bde7-15a0c21d21ca (Cluster File Share Witness konfigurieren) [Sap-ha-guide-8.12.2.1]:virtual-machines-windows-sap-high-availability-guide.md#06260b30-d697-4c4d-b1c9-d22c0bd64855 (Erstellen einer Dateifreigabe) [Sap-ha-guide-8.12.2.2]:virtual-machines-windows-sap-high-availability-guide.md#4c08c387-78a0-46b1-9d27-b497b08cac3d (Set Datei Share Witness Quorum im Failovercluster-Manager) [Sap-ha-guide-8.12.3]:virtual-machines-windows-sap-high-availability-guide.md#5c8e5482-841e-45e1-a89d-a05c0907c868 (installieren SIOS DataKeeper Cluster Edition for SAP ASC/SCS Cluster freigegebenen Datenträger) [Sap-ha-Handbuch-8.12.3.1] : Virtual-machines-windows-sap-high-availability-guide.md#1c2788c3-3648-4e82-9e0d-e058e475e2a3 (Add.NET Framework 3.5) [Sap-ha-guide-8.12.3.2]:virtual-machines-windows-sap-high-availability-guide.md#dd41d5a2-8083-415b-9878-839652812102 (SIOS-DataKeeper installieren) [Sap-ha-guide-8.12.3.3]:virtual-machines-windows-sap-high-availability-guide.md#d9c1fc8e-8710-4dff-bec2-1f535db7b006 (SIOS DataKeeper einrichten) [Sap-ha-guide-9]:virtual-machines-windows-sap-high-availability-guide.md#a06f0b49-8a7a-42bf-8b0d-c12026c5746b (Installation System SAP NetWeaver) [Sap-ha-guide-9.1]:virtual-machines-windows-sap-high-availability-guide.md#31c6bd4f-51df-4057-9fdf-3fcbc619c170 (SAP eine hohe Verfügbarkeit ASC-SCS-Instanz installieren) [sap-ha-guide-9.1.1]:virtual-machines-windows-sap-high-availability-guide.md# a97ad604-9094-44fe-a364-f89cb39bf097 (erstellen einen virtuellen Hostnamen für SAP ASC/SCS-Clusterinstanz) [Sap-ha-guide-9.1.2]:virtual-machines-windows-sap-high-availability-guide.md#eb5af918-b42f-4803-bb50-eff41f84b0b0 (SAP ersten Cluster-Knoten installieren) [Sap-ha-guide-9.1.3]:virtual-machines-windows-sap-high-availability-guide.md#e4caaab2-e90f-4f2c-bc84-2cd2e12a9556 (Ändern der SAP-Profil ASC-SCS-Instanz) [Sap-ha-guide-9.1.4]:virtual-machines-windows-sap-high-availability-guide.md#10822f4f-32e7-4871-b63a-9b86c76ce761 (Probe Port hinzufügen) [Sap-ha-guide-9.2]:virtual-machines-windows-sap-high-availability-guide.md#85d78414-b21d-4097-92b6-34d8bcb724b7 (Installation der Datenbankinstanz) [Sap-ha-guide-9.3]:virtual-machines-windows-sap-high-availability-guide.md#8a276e16-f507-4071-b829-cdc0a4d36748 (dem zweiten Clusterknoten installieren) [Sap-ha-guide-9.4]:virtual-machines-windows-sap-high-availability-guide.md#094bc895-31d4-4471-91cc-1513b64e406a (ändern den Starttyp der Dienstinstanz SAP ERS Windows) [Sap-ha-guide-9.5]:virtual-machines-windows-sap-high-availability-guide.md#2477e58f-c5a7-4a5d-9ae3-7b91022cafb5 (Installation der primären SAP-Anwendungsserver) [Sap-ha-guide-9.6]:virtual-machines-windows-sap-high-availability-guide.md#0ba4a6c1-cc37-4bcf-a8dc-025de4263772 (Installation zusätzliche SAP-Anwendungsserver) [sap-ha-guide-10]:virtual-machines-windows-sap-high-availability-guide.md#18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9 (Test SAP ASC/SCS Instanz Failover- und SIOS Replikation) [Sap-ha-guide-10.1]:virtual-machines-windows-sap-high-availability-guide.md#65fdef0f-9f94-41f9-b314-ea45bbfea445 (SAP ASC/SCS auf Knoten A ausgeführt) [ SAP-HA-Guide-10.2]:Virtual-Machines-Windows-SAP-High-Availability-Guide.MD#5e959fa9-8fcd-49e5-a12c-37f6ba07b916 (Failover von Knoten A zu Knoten B) [Sap-ha-guide-10.3]:virtual-machines-windows-sap-high-availability-guide.md#755a6b93-0099-4533-9f6d-5c9a613878b5 (SAP ASC/SCS auf Clusterknoten B ausgeführt))


[sap-ha-guide-figure-1000]:./media/virtual-machines-shared-sap-high-availability-guide/1000-wsfc-for-sap-ascs-on-azure.png
[sap-ha-guide-figure-1001]:./media/virtual-machines-shared-sap-high-availability-guide/1001-wsfc-on-azure-ilb.png
[sap-ha-guide-figure-1002]:./media/virtual-machines-shared-sap-high-availability-guide/1002-wsfc-sios-on-azure-ilb.png
[sap-ha-guide-figure-2000]:./media/virtual-machines-shared-sap-high-availability-guide/2000-wsfc-sap-as-ha-on-azure.png
[sap-ha-guide-figure-2001]:./media/virtual-machines-shared-sap-high-availability-guide/2001-wsfc-sap-ascs-ha-on-azure.png
[sap-ha-guide-figure-2003]:./media/virtual-machines-shared-sap-high-availability-guide/2003-wsfc-sap-dbms-ha-on-azure.png
[sap-ha-guide-figure-2004]:./media/virtual-machines-shared-sap-high-availability-guide/2004-wsfc-sap-ha-e2e-archit-template1-on-azure.png
[sap-ha-guide-figure-3000]:./media/virtual-machines-shared-sap-high-availability-guide/3000-template-parameters-sap-ha-arm-on-azure.png
[sap-ha-guide-figure-3001]:./media/virtual-machines-shared-sap-high-availability-guide/3001-configuring-dns-servers-for-Azure-vnet.png
[sap-ha-guide-figure-3002]:./media/virtual-machines-shared-sap-high-availability-guide/3002-configuring-static-IP-address-for-network-card-of-each-vm.png
[sap-ha-guide-figure-3003]:./media/virtual-machines-shared-sap-high-availability-guide/3003-setup-static-ip-address-ilb-for-ascs-instance.png
[sap-ha-guide-figure-3004]:./media/virtual-machines-shared-sap-high-availability-guide/3004-default-ascs-scs-ilb-balancing-rules-for-azure-ilb.png
[sap-ha-guide-figure-3005]:./media/virtual-machines-shared-sap-high-availability-guide/3005-changing-ascs-scs-default-ilb-rules-for-azure-ilb.png
[sap-ha-guide-figure-3006]:./media/virtual-machines-shared-sap-high-availability-guide/3006-adding-vm-to-domain.png
[sap-ha-guide-figure-3007]:./media/virtual-machines-shared-sap-high-availability-guide/3007-config-wsfc-1.png
[sap-ha-guide-figure-3008]:./media/virtual-machines-shared-sap-high-availability-guide/3008-config-wsfc-2.png
[sap-ha-guide-figure-3009]:./media/virtual-machines-shared-sap-high-availability-guide/3009-config-wsfc-3.png
[sap-ha-guide-figure-3010]:./media/virtual-machines-shared-sap-high-availability-guide/3010-config-wsfc-4.png
[sap-ha-guide-figure-3011]:./media/virtual-machines-shared-sap-high-availability-guide/3011-config-wsfc-5.png
[sap-ha-guide-figure-3012]:./media/virtual-machines-shared-sap-high-availability-guide/3012-config-wsfc-6.png
[sap-ha-guide-figure-3013]:./media/virtual-machines-shared-sap-high-availability-guide/3013-config-wsfc-7.png
[sap-ha-guide-figure-3014]:./media/virtual-machines-shared-sap-high-availability-guide/3014-config-wsfc-8.png
[sap-ha-guide-figure-3015]:./media/virtual-machines-shared-sap-high-availability-guide/3015-config-wsfc-9.png
[sap-ha-guide-figure-3016]:./media/virtual-machines-shared-sap-high-availability-guide/3016-config-wsfc-10.png
[sap-ha-guide-figure-3017]:./media/virtual-machines-shared-sap-high-availability-guide/3017-config-wsfc-11.png
[sap-ha-guide-figure-3018]:./media/virtual-machines-shared-sap-high-availability-guide/3018-config-wsfc-12.png
[sap-ha-guide-figure-3019]:./media/virtual-machines-shared-sap-high-availability-guide/3019-assign-permissions-on-share-for-cluster-name-object.png
[sap-ha-guide-figure-3020]:./media/virtual-machines-shared-sap-high-availability-guide/3020-change-object-type-include-computer-objects.png
[sap-ha-guide-figure-3021]:./media/virtual-machines-shared-sap-high-availability-guide/3021-check-box-for-computer-objects.png
[sap-ha-guide-figure-3022]:./media/virtual-machines-shared-sap-high-availability-guide/3022-set-security-attributes-for-cluster-name-object-on-file-share-quorum.png
[sap-ha-guide-figure-3023]:./media/virtual-machines-shared-sap-high-availability-guide/3023-call-configure-cluster-quorum-setting-wizard.png
[sap-ha-guide-figure-3024]:./media/virtual-machines-shared-sap-high-availability-guide/3024-selection-screen-different-quorum-configurations.png
[sap-ha-guide-figure-3025]:./media/virtual-machines-shared-sap-high-availability-guide/3025-selection-screen-file-share-witness.png
[sap-ha-guide-figure-3026]:./media/virtual-machines-shared-sap-high-availability-guide/3026-define-file-share-location-for-witness-share.png
[sap-ha-guide-figure-3027]:./media/virtual-machines-shared-sap-high-availability-guide/3027-successful-reconfiguration-cluster-file-share-witness.png
[sap-ha-guide-figure-3028]:./media/virtual-machines-shared-sap-high-availability-guide/3028-install-dot-net-framework-35.png
[sap-ha-guide-figure-3029]:./media/virtual-machines-shared-sap-high-availability-guide/3029-install-dot-net-framework-35-progress.png
[sap-ha-guide-figure-3030]:./media/virtual-machines-shared-sap-high-availability-guide/3030-sios-installer.png
[sap-ha-guide-figure-3031]:./media/virtual-machines-shared-sap-high-availability-guide/3031-first-screen-sios-data-keeper-installation.png
[sap-ha-guide-figure-3032]:./media/virtual-machines-shared-sap-high-availability-guide/3032-data-keeper-informs-service-be-disabled.png
[sap-ha-guide-figure-3033]:./media/virtual-machines-shared-sap-high-availability-guide/3033-user-selection-sios-data-keeper.png
[sap-ha-guide-figure-3034]:./media/virtual-machines-shared-sap-high-availability-guide/3034-domain-user-sios-data-keeper.png
[sap-ha-guide-figure-3035]:./media/virtual-machines-shared-sap-high-availability-guide/3035-provide-sios-data-keeper-license.png
[sap-ha-guide-figure-3036]:./media/virtual-machines-shared-sap-high-availability-guide/3036-data-keeper-management-config-tool.png
[sap-ha-guide-figure-3037]:./media/virtual-machines-shared-sap-high-availability-guide/3037-tcp-ip-address-first-node-data-keeper.png
[sap-ha-guide-figure-3038]:./media/virtual-machines-shared-sap-high-availability-guide/3038-create-replication-sios-job.png
[sap-ha-guide-figure-3039]:./media/virtual-machines-shared-sap-high-availability-guide/3039-define-sios-replication-job-name.png
[sap-ha-guide-figure-3040]:./media/virtual-machines-shared-sap-high-availability-guide/3040-define-sios-source-node.png
[sap-ha-guide-figure-3041]:./media/virtual-machines-shared-sap-high-availability-guide/3041-define-sios-target-node.png
[sap-ha-guide-figure-3042]:./media/virtual-machines-shared-sap-high-availability-guide/3042-define-sios-synchronous-replication.png
[sap-ha-guide-figure-3043]:./media/virtual-machines-shared-sap-high-availability-guide/3043-enable-sios-replicated-volume-as-cluster-volume.png
[sap-ha-guide-figure-3044]:./media/virtual-machines-shared-sap-high-availability-guide/3044-data-keeper-synchronous-mirroring-for-SAP-gui.png
[sap-ha-guide-figure-3045]:./media/virtual-machines-shared-sap-high-availability-guide/3045-replicated-disk-by-data-keeper-in-wsfc.png
[sap-ha-guide-figure-3046]:./media/virtual-machines-shared-sap-high-availability-guide/3046-dns-entry-sap-ascs-virtual-name-ip.png
[sap-ha-guide-figure-3047]:./media/virtual-machines-shared-sap-high-availability-guide/3047-dns-manager.png
[sap-ha-guide-figure-3048]:./media/virtual-machines-shared-sap-high-availability-guide/3048-default-cluster-probe-port.png
[sap-ha-guide-figure-3049]:./media/virtual-machines-shared-sap-high-availability-guide/3049-cluster-probe-port-after.png
[sap-ha-guide-figure-3050]:./media/virtual-machines-shared-sap-high-availability-guide/3050-service-type-ers-delayed-automatic.png
[sap-ha-guide-figure-5000]:./media/virtual-machines-shared-sap-high-availability-guide/5000-wsfc-sap-sid-node-a.png
[sap-ha-guide-figure-5001]:./media/virtual-machines-shared-sap-high-availability-guide/5001-sios-replicating-local-volume.png
[sap-ha-guide-figure-5002]:./media/virtual-machines-shared-sap-high-availability-guide/5002-wsfc-sap-sid-node-b.png
[sap-ha-guide-figure-5003]:./media/virtual-machines-shared-sap-high-availability-guide/5003-sios-replicating-local-volume-b-to-a.png


[powershell-install-configure]:../powershell-install-configure.md
[resource-group-authoring-templates]:../resource-group-authoring-templates.md
[resource-group-overview]:../resource-group-overview.md
[resource-groups-networking]:../virtual-network/resource-groups-networking.md
[sap-pam]:https://support.sap.com/pam (SAP Product Availability Matrix)
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[storage-azure-cli]:../storage/storage-azure-cli.md
[storage-azure-cli-copy-blobs]:../storage/storage-azure-cli.md#copy-blobs
[storage-introduction]:../storage/storage-introduction.md
[storage-powershell-guide-full-copy-vhd]:../storage/storage-powershell-guide-full.md#how-to-copy-blobs-from-one-storage-container-to-another
[storage-premium-storage-preview-portal]:../storage/storage-premium-storage.md
[storage-redundancy]:../storage/storage-redundancy.md
[storage-scalability-targets]:../storage/storage-scalability-targets.md
[storage-use-azcopy]:../storage/storage-use-azcopy.md
[template-201-vm-from-specialized-vhd]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd
[templates-101-simple-windows-vm]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-windows-vm
[templates-101-vm-from-user-image]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image
[virtual-machines-linux-attach-disk-portal]:virtual-machines-linux-attach-disk-portal.md
[virtual-machines-windows-attach-disk-portal]:virtual-machines-windows-attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../resource-manager-deployment-model.md
[virtual-machines-azure-resource-manager-architecture-benefits-arm]:../resource-manager-deployment-model.md#benefits-of-using-resource-manager-and-resource-groups
[virtual-machines-azurerm-versus-azuresm]:virtual-machines-windows-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:virtual-machines-linux-cli-deploy-templates.md (Deploy and manage virtual machines by using Azure Resource Manager templates and the Azure CLI)
[virtual-machines-deploy-rmtemplates-powershell]:virtual-machines-windows-ps-manage.md (Manage virtual machines using Azure Resource Manager and PowerShell)
[virtual-machines-linux-agent-user-guide]:virtual-machines-linux-agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:virtual-machines-linux-agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:virtual-machines-linux-capture-image.md
[virtual-machines-linux-capture-image-capture]:virtual-machines-linux-capture-image.md#capture-the-vm
[virtual-machines-windows-capture-image]:virtual-machines-windows-capture-image.md
[virtual-machines-windows-capture-image-capture]:virtual-machines-windows-capture-image.md#capture-the-vm
[virtual-machines-linux-configure-lvm]:virtual-machines-linux-configure-lvm.md
[virtual-machines-linux-configure-raid]:virtual-machines-linux-configure-raid.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:virtual-machines-linux-suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:virtual-machines-linux-redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:virtual-machines-linux-add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:virtual-machines-linux-add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:virtual-machines-linux-quick-create-cli.md
[virtual-machines-linux-update-agent]:virtual-machines-linux-update-agent.md
[virtual-machines-manage-availability]:virtual-machines-windows-manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:virtual-machines-windows-ps-create.md
[virtual-machines-sizes]:virtual-machines-windows-sizes.md
[virtual-machines-windows-classic-ps-sql-alwayson-availability-groups]:virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md
[virtual-machines-windows-classic-ps-sql-int-listener]:virtual-machines-windows-classic-ps-sql-int-listener.md
[virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]:virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md
[virtual-machines-windows-portal-sql-alwayson-int-listener]:virtual-machines-windows-portal-sql-alwayson-int-listener.md
[virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions]:virtual-machines-windows-sql-high-availability-dr.md
[virtual-machines-sql-server-infrastructure-services]:virtual-machines-windows-sql-server-iaas-overview.md
[virtual-machines-sql-server-performance-best-practices]:virtual-machines-windows-sql-performance.md
[virtual-machines-upload-image-windows-resource-manager]:virtual-machines-windows-upload-image.md
[virtual-machines-windows-tutorial]:virtual-machines-windows-hero-tutorial.md
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/documentation/templates/sql-server-2014-alwayson-dsc/
[virtual-network-deploy-multinic-arm-cli]:../virtual-network/virtual-network-deploy-multinic-arm-cli.md
[virtual-network-deploy-multinic-arm-ps]:../virtual-network/virtual-network-deploy-multinic-arm-ps.md
[virtual-network-deploy-multinic-arm-template]:../virtual-network/virtual-network-deploy-multinic-arm-template.md
[virtual-networks-configure-vnet-to-vnet-connection]:../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]:../virtual-network/virtual-networks-create-vnet-arm-pportal.md
[virtual-networks-manage-dns-in-vnet]:../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics]:../virtual-network/virtual-networks-multiple-nics.md
[virtual-networks-nsg]:../virtual-network/virtual-networks-nsg.md
[virtual-networks-reserved-private-ip]:../virtual-network/virtual-networks-static-private-ip-arm-ps.md
[virtual-networks-static-private-ip-arm-pportal]:../virtual-network/virtual-networks-static-private-ip-arm-pportal.md
[virtual-networks-udr-overview]:../virtual-network/virtual-networks-udr-overview.md
[vpn-gateway-about-vpn-devices]:../vpn-gateway/vpn-gateway-about-vpn-devices.md
[vpn-gateway-create-site-to-site-rm-powershell]:../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-cross-premises-options]:../vpn-gateway/vpn-gateway-plan-design.md
[vpn-gateway-site-to-site-create]:../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md
[vpn-gateway-vpn-faq]:../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../xplat-cli-install.md
[xplat-cli-azure-resource-manager]:../xplat-cli-azure-resource-manager.md


Azure Virtual Machines ist die Lösung für Unternehmen, die COMPUTE, Speicher und Netzwerk-Ressourcen in kürzester Zeit und ohne lange beschaffungszyklen benötigen. Azure Virtual Machines können klassische Anwendung wie SAP NetWeaver-basierte ABAP, Java und ABAP + Java-Stack bereitstellen. Zuverlässigkeit und Verfügbarkeit ohne zusätzliche lokale Ressourcen zu erweitern. Da Azure Virtual Machines standortübergreifende Konnektivität unterstützt, kann Ihrer Organisation Azure Virtual Machines Ihre lokalen Domänen privater Clouds und SAP-Systemlandschaft integrieren.

In diesem Artikel untersuchen wir die Schritte ausführen, um SAP-Systeme mit hoher Verfügbarkeit in Azure mithilfe von neuen Bereitstellungsmodell von Azure-Ressourcen-Manager bereitstellen. Wir durchlaufen Sie diese Hauptaufgaben:

- Suchen rechts SAP Installationshandbücher und Notizen aufgeführten [Ressourcen] [ sap-ha-guide-2] Abschnitt. Dieser Artikel ergänzt SAP-Installationsdokumentation und Hinweise, die primären Ressourcen erleichtern installieren und Bereitstellen von SAP-Software auf bestimmten Plattformen.

- Lernen Sie die Unterschiede zwischen der klassischen Azure-Bereitstellung und Azure Ressourcenmanager Bereitstellung.

- Erfahren Sie mehr über Windows Server Failover Clustering-Quorummodi, damit Sie das Modell auswählen können, für Ihre Azure-Bereitstellung.

- Erfahren Sie mehr über Windows Server Failover Clustering freigegebenen Speicher in Azure Services.

- Informationen zu Single Point of Failure Komponenten wie die ABAP SAP zentrale Dienste (ASC) / SAP zentrale Services (SCS) und Datenbankmanagementsysteme (DBMS) und redundante Komponenten wie SAP-Anwendungsservern in Azure.

- Führen Sie ein schrittweises Beispiel eine Installation und Konfiguration eines SAP-Systems hohe Verfügbarkeit in einem Cluster Windows Server Failover Clustering mit Azure als Plattform mit Azure-Ressourcen-Manager.

- Erfahren Sie mehr über weitere Schritte mit der Windows Server Failover Clustering Azure jedoch die lokale Bereitstellung nicht erforderlich sind.

Zur Vereinfachung der Bereitstellung und Konfiguration in diesem Artikel verwenden wir neuen SAP dreistufige Hochverfügbarkeits-Ressourcen-Manager Vorlagen. Die Vorlagen Automatisieren der Bereitstellung der gesamten Infrastruktur für ein SAP-System mit hoher Verfügbarkeit. Die Infrastruktur unterstützt auch SAPS Größe Ihres SAP-Systems.

[AZURE.INCLUDE [windows-warning](../../includes/virtual-machines-linux-sap-warning.md)]

## <a name="217c5479-5595-4cd8-870d-15ab00d4f84c"></a>Erforderliche Komponenten

Bevor Sie beginnen, stellen Sie sicher, dass die erforderlichen Komponenten entsprechen, die in den folgenden Abschnitten beschrieben werden. Auch alle Ressourcen in [Ressourcen] überprüfen[ sap-ha-guide-2] Abschnitt.

In diesem Artikel verwenden wir Azure Resource Manager Vorlagen für [Dreistufige SAP NetWeaver](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image/). Eine hilfreiche Übersicht über Vorlagen finden Sie unter [SAP Azure Resource Manager Vorlagen](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/).

## <a name="42b8f600-7ba3-4606-b8a5-53c4f026da08"></a>Ressourcen

Diese Leitfäden abdecken SAP-Installationen in Azure:

- [Sapnetweaver auf Windows virtuelle Maschinen (VMs) – Planung und Implementierungshandbuch] [Planungshandbuch]
- [Sapnetweaver auf Windows virtuelle Maschinen (VMs) – Bereitstellungshandbuch] [Bereitstellungshandbuch]
- [Sapnetweaver auf Windows virtuelle Maschinen (VMs) – Bereitstellungshandbuch DBMS] [Dbms-Guide]
- [Sapnetweaver auf Windows virtuelle Maschinen (VMs)-Hochverfügbarkeits-Handbuch [dabei]][sap-ha-guide]

> [AZURE.NOTE] Wann immer möglich, geben wir Ihnen einen Link verweisenden SAP-Installationshandbuch ( [SAP Installationshandbücher]Siehe[sap-installation-guides]). Informationen zum Installationsvorgang und erforderliche Komponenten empfiehlt es eine SAP NetWeaver-Handbücher lesen. Dieser Artikel behandelt nur bestimmte Aufgaben für SAP NetWeaver-Systemen, mit denen Sie mit Azure Virtual Machines.

Diese Hinweise beziehen sich auf das Thema der SAP in Azure:

| Hinweisnummer   | Titel                                                    
| ------------- |----------------------------------------------------------
| [1928533]       | SAP-Applikationen in Azure: unterstützte Produkte und Größe
| [2015553]       | SAP auf Microsoft Azure: unterstützt Komponenten         
| [1999351]       | Verbesserte Überwachung Azure für SAP                        
| [2178632]       | Schlüssel überwachen Metriken für SAP auf Microsoft Azure        
| [1999351]       | Virtualisierung unter Windows: Verbesserte Überwachung      


Erfahren Sie mehr über die [Grenzen der Azure-Abonnements][azure-subscription-service-limits-subscription], einschließlich allgemeiner standardmäßig beschränkt und maximalen Grenzen.

## <a name="42156640c6-01cf-45a9-b225-4baa678b24f1"></a>Hohe Verfügbarkeit SAP mit Azure-Ressourcen-Manager und das klassische Bereitstellungsmodell

Azure-Ressourcen-Manager und klassischen Bereitstellungsmodelle unterscheiden sich auf zwei Arten:

- Ressourcengruppen
- Clusterdienstanforderungen

### <a name="f76af273-1993-4d83-b12d-65deeae23686"></a>Ressourcengruppen
Azure-Ressourcen-Manager Ressourcengruppen können Sie alle Ressourcen der Anwendung in Azure-Abonnement verwalten. Ein integrierter Ansatz haben alle Ressourcen in einer Ressourcengruppe denselben Lebenszyklus. Beispielsweise sind alle Ressourcen zur gleichen Zeit erstellt und gleichzeitig gelöscht. Weitere Informationen zu [Ressourcengruppen](azure-resource-manager/resource-group-overview.md#resource-groups)erhalten.

### <a name="3e85fbe0-84b1-4892-87af-d9b65ff91860"></a>Clustering mit Azure-Ressourcen-Manager und das klassische Bereitstellungsmodell

Im Modell Azure-Ressourcen-Manager benötigen Sie keinen Clouddienst Azure internen Lastenausgleich für hohe Verfügbarkeit verwenden.

Azure Klassisch führen Sie mithilfe die Verfahren [SAP NetWeaver in Azure: Clustering SAP ASC/SCS-Instanzen mithilfe von Windows Server Failover Clustering in Azure mit SIOS DataKeeper](http://go.microsoft.com/fwlink/?LinkId=613056).

> [AZURE.NOTE] Wir empfehlen die Verwendung von Azure-Ressourcen-Manager-Bereitstellungsmodell für Ihre SAP-Installationen. Es bietet viele Vorteile, die nicht in der klassischen Bereitstellungsmodell verfügbar sind. Weitere Informationen zu Azure [Bereitstellungsmodelle][virtual-machines-azure-resource-manager-architecture-benefits-arm].   

## <a name="8ecf3ba0-67c0-4495-9c14-feec1a2255b7"></a>Windows Server Failover Clustering

Windows Server Failover Clustering ist die Grundlage für eine hohe Verfügbarkeit SAP ASC/SCS-Installation und DBMS in Windows.

Ein Failover-Cluster ist eine Gruppe von 1 + n unabhängiger Server (Knoten), die zusammenarbeiten, um die Verfügbarkeit von Programmen und Diensten. Wenn ein Knoten ausfällt, berechnet Windows Server Failover Clustering die Anzahl der Fehler, die auftreten können und fehlerfreien Cluster definierten Programme und Dienste zu verwalten. Von anderen-Quorummodi können dazu.

### <a name="1a3c5408-b168-46d6-99f5-4219ad1b1ff2"></a>Quorummodi

Sie können aus vier-Quorummodi Verwendung Windows Server Failover Clustering:

- **Knotenmehrheit**. Jeder Knoten des Clusters kann abstimmen. Cluster funktioniert nur bei Mehrheit der stimmen, also mehr als die Hälfte der stimmen. Wir empfehlen diese Option für Cluster mit einer ungeraden Anzahl von Knoten. Z. B. drei Knoten in einem Cluster mit sieben Knoten können fehlschlagen und Standbilder Cluster Mehrheit erreicht und wird weiterhin ausgeführt.  

- **Knoten- und Datenträgermehrheit**. Jeder Knoten und einen bestimmten Datenträger (einen Zeugendatenträger) im Clusterspeicher können abstimmen verfügbar und Kommunikation. Cluster funktioniert nur bei Mehrheit der stimmen, also mehr als die Hälfte der stimmen. Dieser Modus ist sinnvoll in einer Clusterumgebung mit einer geraden Anzahl von Knoten. Wenn die Hälfte der Knoten und den Datenträger online sind, bleibt der Cluster in einem ordnungsgemäßen Zustand.

- **Knoten- und Dateifreigabemehrheit**. Jeder Knoten sowie einer bestimmten Dateifreigabe (File Share Witness), die der Administrator erstellt kann stimmen, unabhängig davon, ob sie verfügbar und Kommunikation. Cluster funktioniert nur bei Mehrheit der stimmen, also mehr als die Hälfte der stimmen. Dieser Modus ist sinnvoll in einer Clusterumgebung mit einer geraden Anzahl von Knoten. Knoten- und Datenträgermehrheit Modus ähnelt, aber eine Dateifreigabe Zeuge statt einen Zeugendatenträger verwendet. Dieser Modus ist einfach zu implementieren, aber wenn die Datei selbst ist nicht hoch verfügbar, es kann eine einzelne Fehlerquelle werden.

- **Keine Mehrheit: nur Datenträger**. Der Cluster ist beschlussfähig, wenn ein Knoten verfügbar und Kommunikation mit einem bestimmten Datenträger im Clusterspeicher. Kommunikation mit dem Datenträger sind Knoten können dem Cluster beitreten. Wir empfehlen diesen Modus nicht verwenden.
 
## <a name="fdfee875-6e66-483a-a343-14bbaee33275"></a>Windows Server Failover Clustering lokal
Das Beispiel in Abbildung 1 zeigt einen Cluster mit zwei Knoten. Wenn die Verbindung zwischen den Knoten fehlschlägt und Knoten bleiben und ausgeführt, ein Quorum-Datenträger oder Datei freigeben bestimmt Knoten weiterhin Programme und Dienste des Clusters. Der Knoten, der Zugriff auf oder die Quorumfreigabe ist der Knoten, der sicherstellt, dass Dienste.

Da in diesem Beispiel wird einen Cluster mit zwei Knoten verwendet, verwenden wir den Knoten- und Dateifreigabemehrheit Quorum-Modus. Knoten- und Datenträgermehrheit ist ebenfalls gültig. In einer produktiven Umgebung wird empfohlen, einen Quorumdatenträger zu verwenden. Netzwerk- und System-Technologie können Sie um hoch verfügbar zu machen.

![Abbildung 1: Beispiel für eine Windows Server Failover Clustering-Konfiguration für SAP ASC/SCS in Azure][sap-ha-guide-figure-1000]

_**Abbildung 1:** Beispiel für eine Windows Server Failover Clustering-Konfiguration für SAP ASC/SCS in Azure_

### <a name="be21cf3e-fb01-402b-9955-54fbecf66592"></a>Gemeinsam genutztem Speicher

Abbildung 1 zeigt außerdem zwei Knoten freigegebenen Speicher-Cluster. In einem lokalen freigegebenen speichercluster erkennen alle Knoten im Cluster gemeinsam genutzten Speicher. Einen schützt Daten vor Beschädigung. Alle Knoten können sie erkennen, bei einem anderen Knoten auftritt. Bei Ausfall eines Knotens, der verbleibende Knoten übernimmt den Besitz der Speicherressourcen und gewährleistet die Verfügbarkeit von Diensten.

> [AZURE.NOTE] Sie brauchen nicht freigegebene Datenträgern für hohe Verfügbarkeit mit einigen Programmen DBMS wie mit SQL Server. SQL Server immer auf repliziert DBMS Daten- und Protokolldateien Dateien vom lokalen Datenträger von einem Clusterknoten auf dem lokalen Datenträger von einem anderen Knoten. In diesem Fall benötigen nicht die Windows-Clusterkonfiguration einen freigegebenen Datenträger.

### <a name="ff7a9a06-2bc5-4b20-860a-46cdb44669cd"></a>Netzwerk- und Auflösung

Clientcomputer erreichen Cluster virtuellen IP-Adresse und einen virtuellen Hostnamen, den vom DNS-Server. Die lokalen Knoten und der DNS-Server können mehrere IP-Adressen verarbeiten.

In einer normalen Installation verwenden Sie mindestens zwei Netzwerkschnittstellen:

- Eine dedizierte Verbindung zum Speicher
- Cluster-interne Netzwerkschnittstelle für den Takt
- Ein öffentliches Netzwerk, mit dem Clients herstellen

## <a name="2ddba413-a7f5-4e4e-9a51-87908879c10a"></a>Windows Server Failover Clustering in Azure

Um bare Metal oder private Cloud-Bereitstellungen benötigt Azure Virtual Machines zusätzliche Schritte zur Konfiguration von Windows Server Failover Clustering. Wenn Sie einen freigegebenen Clusterdatenträger erstellen, müssen Sie mehrere IP-Adressen und virtuellen Hostnamen für SAP ASC/SCS-Instanz festgelegt.

In diesem Artikel erläutern wir Schlüsselkonzepte und die zusätzlichen Schritte bei der Erstellung eines Clusters SAP hoher Verfügbarkeit zentraler Dienste in Azure. Wir zeigen Ihnen, wie Drittanbietertool SIOS DataKeeper eingerichtet und Azure internen Lastenausgleich konfigurieren. Diese Tools können einem Windows-Failovercluster mit File Share Witness in Azure erstellen.


![Abbildung 2: Windows Server Failover Clustering-Konfiguration in Azure ohne einen freigegebenen Datenträger][sap-ha-guide-figure-1001]

_**Abbildung 2:** Windows Server Failover Clustering-Konfiguration in Azure ohne einen freigegebenen Datenträger_


### <a name="1a464091-922b-48d7-9d08-7cecf757f341"></a>Freigegebenen Datenträger in Azure mit SIOS DataKeeper

Sie müssen freigegebenen Speicher für eine hohe Verfügbarkeit SAP ASC/SCS-Instanz cluster. Ab September 2016 bietet Azure keine freigegebenen Speicher, mit denen Sie einen gemeinsam genutzten Speicher-Cluster erstellen. Drittanbietersoftware SIOS DataKeeper Cluster Edition können Sie einen gespiegelten Speicher erstellen, der freigegebene Speicher simuliert. SIOS bietet synchrone Datenreplikation in Echtzeit. Dies ist das Erstellen einer freigegebenen Datenträgerressource eines Clusters:

1. Fügen Sie eine zusätzliche Azure virtuelle Festplatte (VHD) aller virtuellen Computer in einer Windows-Clusterkonfiguration.
2. SIOS DataKeeper Cluster Edition auf beide virtuellen Knoten ausführen.
3. Konfigurieren Sie SIOS DataKeeper Cluster Edition, sodass den Inhalt zusätzliche VHD angefügte Volume aus virtuellen Quellcomputers VHD angefügte zusätzliche mit dem virtuellen Zielcomputer spiegelt. SIOS DataKeeper abstrahiert die Quell- und lokalen Volumes und stellt diese auf Windows Server Failover Clustering als eine freigegebene Festplatte.

Erfahren Sie mehr über [SIOS DataKeeper](http://us.sios.com/products/datakeeper-cluster/).

 ![Abbildung 3: Windows Server Failover Clustering-Konfiguration in Azure mit SIOS DataKeeper][sap-ha-guide-figure-1002]

_**Abbildung 3:** Windows Server Failover Clustering-Konfiguration in Azure mit SIOS DataKeeper_

> [AZURE.NOTE] Sie brauchen nicht freigegebene Datenträgern für hohe Verfügbarkeit mit einigen DBMS-Produkte wie SQL Server. SQL Server immer auf repliziert DBMS Daten- und Protokolldateien Dateien vom lokalen Datenträger von einem Clusterknoten auf dem lokalen Datenträger von einem anderen Knoten. In diesem Fall erforderlich nicht Windows-Cluster-Konfiguration eine gemeinsam genutzte Festplatte.

### <a name="44641e18-a94e-431f-95ff-303ab65e0bcb"></a>Auflösung in Azure

 Azure-Cloud-Plattform bietet keine virtuelle IP-Adressen wie schwebende IPs konfigurieren können. Deshalb benötigen Sie eine alternative Lösung eine virtuelle IP-Adresse zu der Clusterressource in der Cloud einrichten.
Azure hat eine interne Lastenausgleich Lastenausgleich Azure Service. Mit einem internen Lastenausgleich erreichen Kunden Cluster Cluster virtuellen IP-Adresse.
Sie müssen interne Lastenausgleich in der Ressourcengruppe bereitstellen, die Cluster-Knoten enthält. Konfigurieren Sie alle erforderlichen Ports Regeln mit den Ports des internen Systems zum Lastenausgleich weiterleiten.
Die Clients können über den virtuellen Hostnamen. Der DNS-Server löst IP-Adresse und interne Load Balancer Handles Port forwarding auf dem aktiven Knoten des Clusters.

## <a name="2e3fec50-241e-441b-8708-0b1864f66dfa"></a>Hohe Verfügbarkeit SAP NetWeaver in Azure Infrastructure-as-a-Service (IaaS)

Zu hohe Verfügbarkeit der SAP-Anwendung, z. B. SAP-Software-Komponenten müssen Sie die folgenden Komponenten schützen. Dies wird ausführlicher in [SAP NetWeaver auf Windows virtuelle Maschinen (VMs) – Planung und Implementierungshandbuch] [Planung-Handbuch-11].

- SAP-Anwendungsserver
- SAP ASC/SCS-Instanz
- DBMS-server

### <a name="93faa747-907e-440a-b00a-1ae0a89b1c0e"></a>Hohe Verfügbarkeit SAP-Anwendungsserver

Nicht benötigen Sie normalerweise eine bestimmte Hochverfügbarkeits-Lösung für SAP-Anwendungsserver und Dialogfeld Instanzen. Hohen Verfügbarkeit durch Redundanz erreichen, und konfigurieren Sie Dialogfeld mehrfach in verschiedenen Instanzen von Azure Virtual Machines. Sie sollten mindestens zwei Instanzen des SAP-Anwendung in zwei Instanzen der Azure-Computer installiert haben.

![Abbildung 4: Hohe Verfügbarkeit SAP-Anwendungsserver][sap-ha-guide-figure-2000]

_**Abbildung 4:** Hohe Verfügbarkeit SAP-Anwendungsserver_

Sie müssen alle virtuellen Computer platzieren, die SAP-Anwendungsservern in der gleichen Azure Host festgelegt. Ein Satz Azure Verfügbarkeit stellt Folgendes sicher:

- Alle virtuellen Maschinen sind Teil der gleichen Domäne aktualisieren. Eine Aktualisierungsdomäne stellt z. B. sicher, dass die virtuellen Computer während der geplanten Wartung Ausfallzeiten gleichzeitig aktualisiert werden.
- Alle virtuellen Maschinen sind Teil der gleichen Fehlerdomäne. Eine Fehlerdomäne stellt z. B. sicher, dass virtuelle Computer bereitgestellt werden, damit keine einzelne Fehlerquelle wirkt sich auf die Verfügbarkeit aller virtuellen Maschinen.

Enthält Informationen zum [Verwalten der Verfügbarkeit virtueller Maschinen][virtual-machines-manage-availability].

Ist das Azure-Speicherkonto mögliche Fehlerquelle unbedingt mindestens zwei Azure Speicherkonten verfügen über mindestens zwei virtuelle Computer verteilt werden. In eine ideale Einrichtung wird jeder virtuelle Computer, die ein SAP-Dialogfeld ausgeführt wird in einem anderen Speicherkonto bereitgestellt werden.

### <a name="f559c285-ee68-4eec-add1-f60fe7b978db"></a>Hohe Verfügbarkeit SAP ASC/SCS-Instanz

![Abbildung 5: Hohe Verfügbarkeit SAP ASC/SCS-Instanz][sap-ha-guide-figure-2001]

_**Abbildung 5:** Hohe Verfügbarkeit SAP ASC/SCS-Instanz_


#### <a name="b5b1fd0b-1db4-4d49-9162-de07a0132a51"></a>Hohe Verfügbarkeit SAP ASC/SCS-Instanz mit Windows Server Failover Clustering in Azure

Um bare Metal oder private Cloud-Bereitstellungen benötigt Azure Virtual Machines zusätzliche Schritte zur Konfiguration von Windows Server Failover Clustering. Um eine Windows-Failovercluster zu erstellen, benötigen Sie einen freigegebenen Clusterdatenträger, mehrere IP-Adressen virtuellen Hostnamen und eine Azure internen Lastenausgleich für clustering SAP ASC/SCS-Instanz.

Wir besprechen später in diesem Artikel ausführlicher.

![Abbildung 6: Windows Server Failover Clustering für eine SAP ASC/SCS-Konfiguration in Azure mit SIOS DataKeeper][sap-ha-guide-figure-1002]

_**Abbildung 6:** Windows Server Failover Clustering für eine SAP ASC/SCS-Konfiguration in Azure mit SIOS DataKeeper_


### <a name="ddd878a0-9c2f-4b8e-8968-26ce60be1027"></a>Hohe Verfügbarkeit DBMS-Instanz

Das DBMS ist auch eine zentrale Anlaufstelle eines SAP-Systems. Sie müssen mit einer Lösung mit hoher Verfügbarkeit zu schützen. Abbildung 7 zeigt ein Beispiel für eine SQL Server immer Hochverfügbarkeits-Lösung mit Windows Server Failover Clustering und Azure internen Lastenausgleich in Azure. SQL Server ständig repliziert DBMS Daten- und Protokolldateien Dateien durch eine eigene DBMS-Replikation. In diesem Fall müssen Sie nicht freigegebene Datenträger cluster die gesamte Einstellung vereinfacht.

![Abbildung 7: Beispiel für eine hohe Verfügbarkeit SAP DBMS: SQL Server immer auf][sap-ha-guide-figure-2003]

_**Abbildung 7:** Beispiel für eine hohe Verfügbarkeit SAP DBMS: SQL Server immer auf_


Weitere Informationen zum clustering in Azure SQL Server mithilfe des Azure-Ressourcen-Manager-Bereitstellungsmodells finden Sie in diesen Artikeln:

- [Konfigurieren Sie Gruppe immer auf Verfügbarkeit in Azure Virtual Machines manuell mithilfe des Ressourcen-Managers][virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]
- [Konfigurieren Sie ein interner Lastenausgleich für eine Gruppe immer auf Verfügbarkeit in Azure][virtual-machines-windows-portal-sql-alwayson-int-listener]

### <a name="045252ed-0277-4fc8-8f46-c5a29694a816"></a>End-to-End-Hochverfügbarkeits-Bereitstellungsszenarien

Abbildung 8 zeigt ein Beispiel für ein SAP NetWeaver-Architektur für hohe Verfügbarkeit in Azure. In diesem Fall verwenden wir ein dedizierten Cluster für SAP ASC/SCS-Instanz und eine andere für das DBMS.

![Abbildung 8: SAP HA Architektur Vorlage 1 mit einem dedizierten Cluster ASC-SCS eines dedizierten Clusters für DBMS-Instanz][sap-ha-guide-figure-2004]

_**Abbildung 8:** SAP HA-Architektur Vorlage 1: Dedizierter Cluster ASC-SCS und DBMS_

## <a name="78092dbe-165b-454c-92f5-4972bdbef9bf"></a>Vorbereiten der Infrastruktur

Azure Ressourcenmanager Vorlagen für SAP vereinfachen Bereitstellung der erforderlichen Ressourcen.

Die dreistufige Vorlagen unterstützen auch Hochverfügbarkeits-Szenarien wie Architektur Vorlage 1 mit zwei Clustern. Jeder Block ist ein SAP Einzelpunktversagen für SAP ASC/SCS und DBMS.

Hier ist Azure Resource Manager Vorlagen für Szenario 1 erhalten Sie:

- [Azure Marketplace-Bild](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image)  
- [Benutzerdefiniertes Bild](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image)

Wenn Sie SAP dreistufige Marketplace-Bild auswählen, zeigt dieser Bildschirm Azure-Portal:

![Abbildung 9: SAP Hochverfügbarkeits-Ressourcenmanager Azure Parameter angeben][sap-ha-guide-figure-3000]

_**Abbildung 9:** SAP Hochverfügbarkeits-Ressourcenmanager Azure Parameter angeben_


Wählen Sie im **SYSTEMAVAILABILITY** **HA**.

Vorlagen erstellen:

- **Virtuelle Computer**:
    - SAP Application Server virtueller Maschinen: <*SAPSystemSID*> - di - <*Zahl*>
    - ASC-SCS cluster virtuellen Maschinen: <*SAPSystemSID*> - ASC - <*Zahl*>
    - Ein DBMS-Cluster: <*SAPSystemSID*> - Db - <*Zahl*>
- **Netzwerkkarten für alle virtuellen Maschinen mit zugeordneten IP-Adressen**:
    - <*SAPSystemSID*> - Nic - di - <*Zahl*>
    - <*SAPSystemSID*> - Nic - ASC - <*Zahl*>
    - <*SAPSystemSID*> - Nic - Db - <*Zahl*>
- **Azure-Speicherkonten**
- **Verfügbarkeit-Gruppen** :
    - SAP Application Server virtueller Maschinen: <*SAPSystemSID*> - Avset - di
    - SAP ASC/SCS cluster virtuellen Maschinen: <*SAPSystemSID*> - Avset - ASC
    - DBMS cluster virtuellen Maschinen: <*SAPSystemSID*> - Avset - Db
- **Interne Azure Lastenausgleich**:
  - Mit allen Ports für die ASC-SCS-Instanz und die IP-Adresse <*SAPSystemSID*> - lb - ASC
  - Mit allen Ports für SQL Server DBMS und IP-Adresse <*SAPSystemSID*> - lb - db
- **Netzwerk-Sicherheitsgruppe**: <*SAPSystemSID*> - Nsg - ASC-0  
    - Mit einem offenen externen Remote Desktop Protocol (RDP) Port <*SAPSystemSID*> - ASC - 0 virtuellen Computer


> [AZURE.NOTE] Alle IP-Adressen der Netzwerkkarten und Azure interne Lastenausgleich sind **dynamische** standardmäßig. In **statischen** IP-Adressen ändern. Wir beschreiben später in diesem Artikel.


### <a name="c87a8d3f-b1dc-4d2f-b23c-da4b72977489"></a>Bereitstellen von virtuellen Computern mit Unternehmensnetzwerk (standortübergreifende) in der Produktion verwenden

Für SAP-Produktionssysteme, Bereitstellen von Azure virtuellen Computern mit [Unternehmensnetzwerk (standortübergreifende)] [Planung-Handbuch-2.2] Azure Standort-zu-Standort-VPN oder Azure ExpressRoute.

> [AZURE.NOTE] Können Sie Ihre Azure Virtual Network-Instanz ein. Virtuelles Netzwerk und Subnetz, haben bereits vorbereitet und wurde erstellt.

Wählen Sie im **NEWOREXISTINGSUBNET** **vorhandene**.

Fügen Sie in **SUBNETID**die vollständige Zeichenfolge vorbereitete Azure Netzwerk SubnetID, wo Sie Ihre Azure virtuellen Computer bereitstellen möchten.

Führen Sie diese PowerShell-Befehl eine Liste aller Azure Netzwerk-Subnetze:

```powershell
(Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets
```

Das **ID** -Feld zeigt **SUBNETID**.

Mit dem folgenden PowerShell-Befehl können Sie eine Liste aller **SUBNETID** Werte abrufen:

```PowerShell
(Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets.Id
```

Der **SUBNETID** sieht folgendermaßen aus:

```
/subscriptions/<SubscriptionId>/resourceGroups/<VPNName>/providers/Microsoft.Network/virtualNetworks/azureVnet/subnets/<SubnetName>
```

### <a name="7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310"></a>Nur-Cloud-Bereitstellung von SAP-Instanzen für Test und demo

Sie können auch Ihr SAP-System hohe Verfügbarkeit in einer nur-Cloud-Bereitstellungsmodell bereitstellen.

Diese Art der Bereitstellung ist hauptsächlich nützlich für Demo oder Test Anwendungsfälle. Es eignet sich nicht für Produktions-Anwendungsfälle.

Wählen Sie in **NEWOREXISTINGSUBNET** **neuen**. Lassen Sie das **SUBNETID** -Feld leer.

Die SAP Azure-Ressourcen-Manager-Vorlage erstellt automatisch Azure virtuelles Netzwerk und Subnetz.

> [AZURE.NOTE] Sie müssen mindestens einen dedizierten virtuellen Computer für Active Directory und DNS in derselben Azure Virtual Network-Instanz bereitstellen. Die Vorlage wird diese virtuellen Computer erstellen.

### <a name="47d5300a-a830-41d4-83dd-1a0d1ffdbe6a"></a>Azure Virtual Network

In unserem Beispiel ist der Adressraum Azure virtual Network 10.0.0.0/16. Es ist ein Subnetz **Subnetz**mit einem Adressbereich des 10.0.0.0/24 aufgerufen. Alle virtuellen Computer und internen Lastenausgleich werden in diesem virtuellen Netzwerk bereitgestellt.

> [AZURE.NOTE] Änderungen Sie keine auf dem Netzwerk innerhalb des Gast-Betriebssystems. Dazu gehören IP-Adressen, DNS-Server und Subnetz. Konfigurieren Sie alle Ihre Netzwerk in Azure. Dynamic Host Configuration Protocol (DHCP) Dienst überträgt Einstellungen.

### <a name="b22d7b3b-4343-40ff-a319-097e13f62f9e"></a>DNS IP Adressen

Stellen Sie sicher, dass virtuelle Netzwerk **DNS-Server** -Option auf **DNS benutzerdefiniert**festgelegt ist.
Wählen Sie Einstellungen basierend auf dem Typ des Netzwerks müssen Sie:

- [Netzwerk-Konnektivität (standortübergreifende)] [Planung-Handbuch-2.2]: die IP-Adressen des lokalen DNS-Server hinzufügen.  

    Lokalen DNS-Server auf den virtuellen Maschinen kann, die in Azure ausgeführt werden. In diesem Szenario können Sie die IP-Adressen der Azure virtuellen Computer hinzufügen, auf denen DNS-Dienst ausgeführt.

-   [Bereitstellung Cloud nur] [Planung-Handbuch-2.1]: Bereitstellung einen weiteren virtuellen Computer im gleichen virtuellen Netzwerk-Instanz, die als DNS-Server dient. Fügen Sie die IP-Adressen Azure virtuelle Computer, die DNS-Dienst ausführen eingerichtet haben.


![Abbildung 10: Konfigurieren von DNS-Servern für Azure Virtual Network][sap-ha-guide-figure-3001]

_**Abbildung 10:** Konfigurieren von DNS-Servern für Azure Virtual Network_

> [AZURE.NOTE] Ändern Sie die IP-Adressen von DNS-Servern müssen Sie starten Azure virtuelle Computer die Änderung und die neuen DNS-Server übertragen.

In unserem Beispiel wird der DNS-Dienst installiert und konfiguriert auf diese virtuelle Windows-Maschinen:

| Virtual Machine-Rolle        | Host-Name des virtuellen Computers | Namen der Netzwerkkarte  | Statische IP-Adresse  
| ---------------|-------------|--------------------|-------------------
| Erste DNS-server | Domcontr 0  | PR1-Nic-Domcontr-0 | 10.0.0.10         
| Zweiten DNS-server | Domcontr-1  | PR1-Nic-Domcontr-1 | 10.0.0.11         


### <a name="9fbd43c0-5850-4965-9726-2a921d85d73f"></a>Hostnamen und statische IP-Adressen für die Clusterinstanz SAP ASC/SCS und DBMS Clusterinstanz

Lokale Bereitstellung erfordert diese reservierten Hostnamen und IP-Adressen:

| Virtueller Host Name Rolle                                                       | Name des virtuellen Hosts | Virtuelle statische IP-Adresse
| ----------------------------------------------------------------------------|------------------|---------------------------
| SAP ASC/SCS erste Cluster virtuellen Hostnamen (für clustermanagement) | PR1 ASC vir     | 10.0.0.42                 
| SAP ASC/SCS VirtualHost Instanznamen                                  | PR1 ASC sap     | 10.0.0.43             
| SAP-DBMS zweiten Cluster virtuellen Hostnamen (Clusterverwaltung)     | PR1 Dbms vir     | 10.0.0.32                 

Beim Erstellen des Clusters erstellen Sie virtueller Host Namen **pr1 ASC Vir** und **pr1 Dbms Vir** und zugeordneten IP-Adressen, die den Cluster verwalten. Wird beschrieben, wie in [sammeln Clusterknoten Clusterkonfiguration] [Sap-ha-Handbuch-8.12.1].

Sie können die beiden virtuellen Hostnamen, **pr1 ASC Sap** und **pr1 Dbms Sap**und zugeordneten IP-Adressen manuell auf dem DNS-Server erstellen. SAP ASC/SCS Clusterinstanz und DBMS Clusterinstanz verwenden diese Ressourcen. Dies wird in [erstellen einen virtuellen Hostnamen für gruppierte SAP ASCS/SCS][sap-ha-guide-9.1.1].

### <a name="84c019fe-8c58-4dac-9e54-173efd4b2c30"></a>Adressen Sie statische IP-für virtuelle SAP-Maschinen

Nach der Bereitstellung der virtuellen Computer im Cluster verwenden müssen Sie statische IP-Adressen für alle virtuellen Maschinen festgelegt. Hierzu in Azure virtuelle Netzwerkkonfiguration und nicht auf dem Gastbetriebssystem.

Eine Möglichkeit, eine statische IP-Adresse ist in der Azure-Portal. **Ressourcengruppe**im Azure-Portal gehen > **Netzwerkkarte** > **Settings** > **IP-Adresse**.

Wählen Sie unter **Zuweisung** **statischer**. Geben Sie im Feld **IP-Adresse** die IP-Adresse, die Sie verwenden möchten.

> [AZURE.NOTE] Ändern die IP-Adresse der Netzwerkkarte müssen Sie Starten der Azure virtuellen Computer die Änderung zu übernehmen.  


![Abbildung 11: Legen Sie statische IP-Adressen für die Netzwerkkarte der einzelnen virtuellen Computer][sap-ha-guide-figure-3002]

_**Abbildung 11:** Adressen Sie statische IP-für die Netzwerkkarte der einzelnen virtuellen Computer_

Wiederholen Sie diesen Schritt für alle Netzwerkschnittstellen, die für alle virtuellen Maschinen virtuelle Computer einschließlich, die Sie für den Active Directory-DNS-Dienst verwenden möchten.

In unserem Beispiel haben wir diese virtuellen Maschinen und statische IP-Adressen:

| Virtual Machine-Rolle                                 | Host-Name des virtuellen Computers  | Namen der Netzwerkkarte  | Statische IP-Adresse  
| ----------------------------------------|--------------|--------------------|-------------------
| Erste SAP-Anwendungsserver              | PR1-di-0     | PR1-Nic-di-0       | 10.0.0.50         
| Zweite SAP-Anwendungsserver              | PR1 di 1     | PR1-Nic-di-1       | 10.0.0.51         
| ...                                     | ...          | ...                | ...               
| Letzte SAP-Anwendungsserver             | PR1 di 5     | PR1-Nic-di-5       | 10.0.0.55         
| Erster Clusterknoten ASC-SCS-Instanz  | PR1 ASC 0   | PR1 Nic ASC 0     | 10.0.0.40         
| Zweiten Clusterknoten ASC-SCS-Instanz  | PR1 ASC 1   | PR1-Nic-ASC-1     | 10.0.0.41         
| Erster Clusterknoten für DBMS-Instanz      | PR1-Db-0     | PR1-Nic-Db-0       | 10.0.0.30         
| Zweiten Clusterknoten für DBMS-Instanz      | PR1-Db-1     | PR1-Nic-Db-1       | 10.0.0.31         

### <a name="7a8f3e9b-0624-4051-9e41-b73fff816a9e"></a>Festlegen einer statischen IP-Adresse für den internen Lastenausgleich

Die Vorlage SAP Azure-Ressourcen-Manager erstellt ein Azure internen Lastenausgleich, die für SAP ASC/SCS Instanz Cluster zu DBMS verwendet wird.

Die erste Bereitstellung wird auf **dynamische**interne Load Balancer IP-Adresse. Es ist wichtig, **statische**IP-Adresse ändern.

In unserem Beispiel haben wir zwei Azure interne Systeme zum Lastenausgleich, die diese Adressen:

| Azure interne Load Balancer Rolle             | Azure interne Load Balancer name | Statische IP-Adresse
| ---------------------------|----------------|-------------------
| SAP ASC/SCS Instanz internen System zum Lastenausgleich  | PR1 lb ASC    | 10.0.0.43         
| Interne SAP DBMS-Lastenausgleich               | PR1 lb dbms    | 10.0.0.33         


> [AZURE.NOTE] Die IP-Adresse des virtuellen Hostnamens der SAP ASC/SCS entspricht der IP-Adresse der SAP ASC/SCS interne Load Balancer pr1-lb-ASC.
Die IP-Adresse des virtuellen Namens des DBMS entspricht die IP-Adresse des DBMS interne Load Balancer pr1-lb-Dbms.

In unserem Beispiel setzen wir die IP-Adresse des internen Load Balancer **pr1 lb ASC** der IP-Adresse des virtuellen Hostnamens SAP ASC/SCS-Instanz (in unserem Beispiel **10.0.0.43**).

![Abbildung 12: Statische IP-Adressen für die internen Lastenausgleich für SAP ASC/SCS-Instanz festgelegt][sap-ha-guide-figure-3003]

_**Abbildung 12:** Festlegen Sie statische IP-Adressen für die internen Lastenausgleich für SAP ASC/SCS-Instanz_

Legen Sie die IP-Adresse des internen Load Balancer **pr1 lb Dbms** der IP-Adresse des virtuellen Hostnamens Instanz DBMS (in unserem Beispiel **10.0.0.33**).

### <a name="f19bd997-154d-4583-a46e-7f5a69d0153c"></a>ASC-SCS Lastenausgleich Regeln für interne Lastenausgleich Azure Standard

SAP Azure Ressourcenmanager Vorlage erstellt die benötigten Ports:

- Eine ABAP ASCS-Instanz die Standardinstanz Nummer **00**
- Eine Java SCS-Instanz die Standardinstanz Nummer **01**

Beim Installieren der SAP ASC/SCS-Instanz müssen Sie die Standard-Instanznummer 00 ABAP ASCS-Instanz und die Instanz Standardanzahl 01 für Java SCS-Instanz verwenden.

Erstellen Sie dann diese erforderlichen internen Lastenausgleich Endpunkte für SAP NetWeaver ABAP ASC-Ports:

| Service-Load balancing Regelname           | Standard-Portnummern | Konkrete Ports (ASC-Instanz mit der Instanznummer 00) (ERS mit 10)  
| ---------------------------------------------|-----------------------|--------------------------------------------------------------------------
| Enqueue-Server / _lbrule3200_                | 32 <*InstanceNumber*>  | 3200                                                                     
| ABAP Messageserver / _lbrule3600_           | 36 <*InstanceNumber*>  | 3600                                                                     
| Interne ABAP Nachricht / _lbrule3900_         | 39 <*InstanceNumber*>  | 3900                                                                     
| Message Server HTTP / _Lbrule8100_           | 81 <*InstanceNumber*>  | 8100                                                                     
| SAP Start Service ASC HTTP / _Lbrule50013_  | 5 <*InstanceNumber*> 13 | 50013                                                                    
| SAP Start Service ASC HTTPS / _Lbrule50014_ | 5 <*InstanceNumber*> 14 | 50014                                                                    
| Enqueue-Replikation / _Lbrule50016_          | 5 <*InstanceNumber*> 16 | 50016                                                                    
| SAP Start Service ERS HTTP- _Lbrule51013_     | 5 <*InstanceNumber*> 13 | 51013                                                                    
| SAP Start Service ERS HTTP- _Lbrule51014_     | 5 <*InstanceNumber*> 14 | 51014                                                                    
| Win RM _Lbrule5985_                          |                       | 5985                                                                     
| Datei-Freigabe _Lbrule445_                       |                       | 445                                                                      

_**Tabelle 1:** Portnummern Instanzen SAP NetWeaver ABAP ASC_


Erstellen Sie diese erforderlichen internen Lastenausgleich Endpunkte für SAP NetWeaver Java SCS-Ports:

| Service-Load balancing Regelname           | Standard-Portnummern | Konkrete Ports (SCS-Instanz mit der Instanznummer 01) (ERS 11)  
| ---------------------------------------------|-----------------------|--------------------------------------------------------------------------
| Enqueue-Server / _lbrule3201_                | 32 <*InstanceNumber*>  | 3201                                                                     
| Gateway-Server / _lbrule3301_                | 33 <*InstanceNumber*>  | 3301                                                                     
| Java Messageserver / _lbrule3900_           | 39 <*InstanceNumber*>  | 3901                                                                     
| Message Server HTTP / _Lbrule8101_           | 81 <*InstanceNumber*>  | 8101                                                                     
| SAP Start Service SCS HTTP / _Lbrule50113_   | 5 <*InstanceNumber*> 13 | 50113                                                                    
| SAP Start Service SCS HTTPS / _Lbrule50114_  | 5 <*InstanceNumber*> 14 | 50114                                                                    
| Enqueue-Replikation / _Lbrule50116_          | 5 <*InstanceNumber*> 16 | 50116                                                                    
| SAP Start Service ERS HTTP- _Lbrule51113_     | 5 <*InstanceNumber*> 13 | 51113                                                                    
| SAP Start Service ERS HTTP- _Lbrule51114_     | 5 <*InstanceNumber*> 14 | 51114                                                                    
| Win RM _Lbrule5985_                          |                       | 5985                                                                     
| Datei-Freigabe _Lbrule445_                       |                       | 445                                                                      

_**Tabelle 2:** Portnummern SAP NetWeaver Java SCS-Instanzen_


![Abbildung 13: Standard ASC/SCS den Lastenausgleich Regeln für Azure internen Lastenausgleich][sap-ha-guide-figure-3004]

_**Abbildung 13:** ASC-SCS Lastenausgleich Regeln für interne Lastenausgleich Azure Standard_

Legen Sie die IP-Adresse des Load Balancer **pr1 lb Dbms** der IP-Adresse des virtuellen Hostnamens Instanz DBMS (in unserem Beispiel **10.0.0.33**).

### <a name="fe0bd8b5-2b43-45e3-8295-80bee5415716"></a>Ändern der ASC-SCS Standard Regeln für Azure internen Lastenausgleich

Wenn Sie unterschiedliche Instanzen SAP ASC oder SCS verwenden möchten, müssen Sie die Namen und Werte dieser Ports aktualisieren.

Variantennummern aktualisiert wird mithilfe des Azure-Portals:

Klicken Sie auf * *<*SID*> ASC - lb - Lastenausgleich** > **laden Netzwerklastenausgleich Regeln **.

Ändern Sie diese Werte für alle Regeln, die die SAP ASC oder SCS-Instanz gehören zur Lastenausgleich:

- Name
- Anschluss
- Back-End-Ports

Wenn die ASC Instanz Standardzahl von 00 bis 31 geändert werden soll, müssen Sie z. B. für alle in Tabelle 1 aufgeführten Ports ändern.

Hier ist ein Beispiel für ein Update für Port _lbrule3200_.

![Abbildung 14: Ändern der ASC-SCS Standard Regeln für Azure internen Lastenausgleich][sap-ha-guide-figure-3005]

_**Abbildung 14:** Ändern der ASC-SCS Standard Regeln für Azure internen Lastenausgleich_

### <a name="e69e9a34-4601-47a3-a41c-d2e11c626c0c"></a>Windows virtuelle Computer der Domäne hinzufügen

Nachdem Sie die virtuellen Computer eine statische IP-Adresse zuweisen, fügen Sie die virtuellen Computer der Domäne hinzu.

![Abbildung 15: Einen virtuellen Computer zu einer Domäne hinzufügen][sap-ha-guide-figure-3006]

_**Abbildung 15:** Einen virtuellen Computer zu einer Domäne hinzufügen_

### <a name="661035b2-4d0f-4d31-86f8-dc0a50d78158"></a>Hinzufügen von Registrierungseinträgen auf beiden Clusterknoten SAP ASC/SCS-Instanz

Azure Lastenausgleich hat eine interne Lastenausgleich schließt Verbindungen, wenn die Verbindung für einen bestimmten Zeitraum im Leerlauf befinden (idle Timeout) Zeit. SAP-Arbeitsprozesse im Dialogfeld Instanzen eine offene Verbindung zu SAP Enqueue verarbeiten als der ersten einreihen/entfernen Anforderung gesendet werden soll. Diese Verbindungen in der Regel bis festgelegten Arbeitsprozess oder Enqueue-Prozess neu gestartet. Die Verbindung für einen Zeitraum im Leerlauf ist, schließt Azure internen Lastenausgleich jedoch die Verbindung. Dies ist kein Problem, da SAP-Arbeitsprozess die Verbindung mit der Warteschlange Prozess wird ist nicht mehr vorhanden. Diese Aktivitäten sind Entwickler Spuren von SAP-Prozesse dokumentiert, aber erstellen sie eine große Menge zusätzlicher Inhalt in diese Spuren. Ist eine gute Idee, TCP/IP ändern `KeepAliveTime` und `KeepAliveInterval` auf beiden Clusterknoten. Kombinieren Sie hierzu in den TCP/IP-Parametern mit SAP-Profilparameter später in diesem Artikel beschrieben.

Diese Windows-Registrierungseinträge auf beiden Clusterknoten Windows für SAP ASC/SCS hinzufügen:

| Pfad                  | HKLM\System\CurrentControlSet\Services\Tcpip\Parameters   
| ----------------------|-----------------------------------------------------------
| Name der Variablen         | `KeepAliveTime`                                              
| Variablentyp         | REG_DWORD (dezimal)                                        
| Wert                 | 120000                                                     
| Link zur Dokumentation | [https://technet.Microsoft.com/en-us/library/cc957549.aspx](https://technet.microsoft.com/en-us/library/cc957549.aspx)


_**Tabelle 3:** Ändern Sie den ersten TCP-parameter_


| Pfad                  | HKLM\System\CurrentControlSet\Services\Tcpip\Parameters   
| ----------------------|-----------------------------------------------------------
| Name der Variablen         | `KeepAliveInterval`                                          
| Variablentyp         | REG_DWORD (dezimal)                                        
| Wert                 | 120000                                                     
| Link zur Dokumentation | [https://technet.Microsoft.com/en-us/library/cc957548.aspx](https://technet.microsoft.com/en-us/library/cc957548.aspx)


_**Tabelle 4:** Ändern Sie den zweiten TCP-parameter_

Um die Änderungen zu übernehmen, starten Sie beide Clusterknoten.

### <a name="0d67f090-7928-43e0-8772-5ccbf8f59aab"></a>Einrichten eines Clusters Windows Server Failover Clustering für SAP ASC/SCS-Instanz

#### <a name="5eecb071-c703-4ccc-ba6d-fe9c6ded9d79"></a>Sammeln Sie die Clusterknoten in einer Clusterkonfiguration

Der erste Schritt ist Failoverclustering beide Clusterknoten hinzufügen. Verwendung der Rollen und Features-Assistenten hinzufügen.

Im zweite Schritt werden den Failovercluster mithilfe des Failovercluster-Manager eingerichtet.

Wählen Sie im Failovercluster-Manager **Cluster erstellen**, und fügen Sie nur den Namen des ersten Cluster-Knoten A. Fügen Sie beispielsweise **pr1 ASC 0**. Fügen Sie den zweiten Knoten noch nicht. Sie fügen den zweiten Knoten in einem späteren Schritt.

![Abbildung 16: Fügen Sie den Server oder virtuellen Namen des ersten Clusterknoten][sap-ha-guide-figure-3007]

_**Abbildung 16:** Fügen Sie den Server oder virtuellen Namen des ersten Clusterknoten_

Als Nächstes werden Sie für den Netzwerknamen (Name des virtuellen Host) des Clusters aufgefordert.

![Abbildung 17: Definieren der Clustername][sap-ha-guide-figure-3008]

_**In Abbildung 17:** Definieren der Clustername_


Führen Sie nach dem Erstellen des Clusters Cluster überprüfen.

![Abbildung 18: Führen Sie die Cluster-Überprüfung][sap-ha-guide-figure-3009]

_**Abbildung 18:** Die Cluster-Überprüfung ausführen_

![Abbildung 19: Kein Quorumdatenträger gefunden][sap-ha-guide-figure-3010]

_**Abbildung 19:** Kein Quorumdatenträger gefunden_

Jede Warnung zu Datenträgern zu diesem Zeitpunkt im Prozess können Sie ignorieren. Sie fügen File Share Witness und die SIOS freigegebenen Datenträger später. In dieser Phase müssen Sie Gedanken ein Quorum.

![Abbildung 20: Core-Cluster-Ressource benötigt eine neue IP-Adresse][sap-ha-guide-figure-3011]

_**Abbildung 20:** Core-Cluster-Ressource benötigt eine neue IP-Adresse_

Cluster kann nicht gestartet werden, da die IP-Adresse des Servers zu einem virtuellen Knoten verweist. Sie müssen die IP-Adresse des Clusterdienstes Core.

Angenommen, möchten wir eine IP-Adresse (in unserem Beispiel **10.0.0.42**) für Cluster virtuellen Host Name **pr1 ASC Vir**. Tun Sie dies auf der Eigenschaftenseite der Clusterdienst Core IP-Ressource, in Abbildung 21 dargestellt.

![Abbildung 21: Die ** ** Eigenschaften-Dialogfeld ändern die IP-Adresse][sap-ha-guide-figure-3012]

_**Abbildung 21:** Ändern Sie im Dialogfeld **Eigenschaften** der IP-Adresse_

![Abbildung 22: Zuweisen der IP-Adresse, die für den Cluster reserviert][sap-ha-guide-figure-3013]

_**Abbildung 22:** Weisen Sie die IP-Adresse, die für den Cluster reserviert_

Schalten Sie nach dem Ändern der IP-Adresse Cluster virtuellen Hostname.

![Abbildung 23: Core-Clusterdienst ist und ausgeführt und die korrekte IP-Adresse][sap-ha-guide-figure-3014]

_**Abbildung 23:** Core-Clusterdienst ist und ausgeführt und die korrekte IP-Adresse_

Core-Clusterdienst ausgeführt wird, können Sie dem zweiten Clusterknoten hinzufügen.

![Abbildung 24: Hinzufügen der zweiten Clusterknoten][sap-ha-guide-figure-3015]

_**Abbildung 24:** Fügen Sie dem zweiten Clusterknoten_

![Abbildung 25: Fügen Sie den zweiten Cluster Knoten Hostnamen, z. B. pr1 ASC 1 hinzu][sap-ha-guide-figure-3016]

_**Abbildung 25:** Fügen Sie den zweiten Cluster Knoten Hostnamen, z. B. **pr1 ASC 1**_

![Abbildung 26: Führen Sie nicht das Kontrollkästchen][sap-ha-guide-figure-3017]

_**In Abbildung 26:** Führen Sie *nicht* das Kontrollkästchen_

> [AZURE.IMPORTANT] Stellen Sie sicher, dass das Kontrollkästchen **Alle beihilfefähigen Speicher zum Cluster hinzufügen** *ausgewählt ist* .  

![Abbildung 27: Ignorieren Sie Warnungen Quorum Datenträger][sap-ha-guide-figure-3018]

_**Abbildung 27:** Ignorieren Sie Warnungen Quorum Datenträger_

Warnhinweise zu Quorum und Festplatten können Sie ignorieren. Sie legen das Quorum und später teilen die Festplatte unter [installieren SIOS DataKeeper Cluster Edition für SAP ASC/SCS Cluster freigegebenen Datenträger] [Sap-ha-Handbuch-8.12.3].

#### <a name="e49a4529-50c9-4dcf-bde7-15a0c21d21ca"></a>Cluster File Share Witness konfigurieren

##### <a name="06260b30-d697-4c4d-b1c9-d22c0bd64855"></a>Erstellen einer Dateifreigabe

File Share Witness statt einem Quorumdatenträger auswählen SIOS DataKeeper unterstützt diese Option.

In den Beispielen in diesem Artikel wird der Dateifreigabenzeuge auf Active Directory-DNS-Server, der in Azure ausgeführt wird. Der Dateifreigabenzeuge heißt **Domcontr 0**. Da eine Verbindung virtuelles privates Netzwerk (VPN) in Azure (per VPN-Standort-zu-Standort-Azure ExpressRoute) Konfiguration würde freigeben Active Directory/DNS Service vor Ort und nicht für eine Datei Zeugen.

> [AZURE.NOTE] Wenn Ihre Active Directory-DNS-Dienst nur lokal ausgeführt wird, konfigurieren Sie nicht der Dateifreigabenzeuge auf Active Directory-DNS-Windows-Betriebssystem, das lokal ausgeführt. Netzwerklatenz zwischen Azure und Active Directory-DNS-lokal auf Clusterknoten möglicherweise zu groß und Verbindungsprobleme verursachen. Achten Sie darauf, dass Dateifreigabenzeuge auf Azure virtuellen Computer konfigurieren, in der Nähe der Clusterknoten ausgeführt wird.  

Das Quorum-Laufwerk benötigt mindestens 1.024 MB freien Speicherplatz. 2.048 MB Speicherplatz empfohlen.

Der erste Schritt ist das Clusterobjekt Namen hinzufügen.

![Abbildung 28: Berechtigungen Sie für die Freigabe für das Clusterobjekt name][sap-ha-guide-figure-3019]

_**Abbildung 28:** Weisen Sie die Berechtigungen für die Freigabe für das Clusterobjekt name_

Werden Sie sicher, dass die Berechtigungen die Berechtigung, Daten in die Freigabe für das Clusterobjekt Name (in unserem Beispiel **pr1 ASC Vir$**) ändern. Um das Clusterobjekt Namen zur Liste hinzuzufügen, wählen Sie **Hinzufügen**. Ändern des Filters für Computerobjekte neben den in Abbildung 29 dargestellten überprüfen:

![Abbildung 29: Ändern Sie den Objekttyp auf Computerobjekte][sap-ha-guide-figure-3020]

_**Abbildung 29:** Ändern Sie den Objekttyp auf Computerobjekte_

![Abbildung 30: Aktivieren Sie das Kontrollkästchen für Computerobjekte][sap-ha-guide-figure-3021]

_**Abbildung 30:** Aktivieren Sie das Kontrollkästchen für Computerobjekte_

Geben Sie das Namen Clusterobjekt wie in Abbildung 29 dargestellt. Da der Datensatz bereits erstellt wurde, können Sie die Berechtigungen ändern, wie in Abbildung 28 dargestellt.

Anschließend wählen Sie die Registerkarte **Sicherheit** der Freigabe und festlegen Sie weitere Berechtigungen für das Clusterobjekt Namen.

![Abbildung 31: Festlegen der Sicherheitsattribute für Cluster Name-Objekt Datei Share Quorum][sap-ha-guide-figure-3022]

_**In Abbildung 31:** Festlegen Sie die Sicherheitsattribute für das Clusterobjekt Namen Datei Share Quorum_

##### <a name="4c08c387-78a0-46b1-9d27-b497b08cac3d"></a>Legen Sie im Failovercluster-Manager Datei Share Witness quorum

Ändern Sie im Failovercluster-Manager, die Clusterkonfiguration in File Share Witness.

![Abbildung 32: Starten Sie den Cluster Quorum Einstellung konfigurieren][sap-ha-guide-figure-3023]

_**In Abbildung 32:** Starten Sie den Cluster Quorum Einstellung konfigurieren_

![Abbildung 33: Quorumkonfigurationen aus wählen][sap-ha-guide-figure-3024]

_**Abbildung 33:** Quorumkonfigurationen aus wählen_

Wählen Sie **das Quorum Witness auswählen**.

![Abbildung 34: Auswählen der Dateifreigabenzeuge][sap-ha-guide-figure-3025]

_**Abbildung 34:** Wählen Sie die Zeugendateifreigabe_

Wählen Sie **konfigurieren eine Datei Zeugen freigeben**.

![Abbildung 35: Definieren Sie Speicherort der Datei für die Freigabe Zeugen][sap-ha-guide-figure-3026]

_**Abbildung 35:** Definieren Sie Speicherort der Datei für die Freigabe Zeugen_

Geben Sie den Quellpfad auf die Dateifreigabe (in unserem Beispiel \\Domcontr 0\FSW).

Wählen Sie **Weiter** eine Liste der Änderungen anzeigen können. Wählen Sie der gewünschten Änderung aus und anschließend auf **Weiter**.

![Abbildung 36: Bestätigen Sie den Cluster neu][sap-ha-guide-figure-3027]

_**Abbildung 36:** Bestätigen Sie neu den cluster_

In diesem letzten Schritt müssen Sie erfolgreich die Clusterkonfiguration, siehe Abbildung 36.  

### <a name="5c8e5482-841e-45e1-a89d-a05c0907c868"></a>Installieren Sie SIOS DataKeeper Cluster Edition für SAP ASC/SCS Cluster freigegebenen Datenträger

Jetzt haben Sie eine funktionierende Konfiguration Windows Server Failover Clustering in Azure. Um eine SAP ASC/SCS-Instanz installieren, Sie benötigen jedoch eine freigegebene Datenträgerressource. Sie können freigegebenen Datenträgerressourcen müssen Sie in Azure erstellen. SIOS DataKeeper Cluster Edition ist eine Drittanbieter-Lösung, mit freigegebenen Datenträgerressourcen erstellen.

#### <a name="1c2788c3-3648-4e82-9e0d-e058e475e2a3"></a>Hinzufügen von.NET Framework 3.5

Microsoft.NET Framework 3.5 nicht automatisch aktiviert oder auf Windows Server 2012 R2 installiert. Allerdings SIOS DataKeeper der.NET Framework, auf allen Knoten, auf dem DataKeeper installiert. Aus diesem Grund müssen Sie.NET Framework 3.5 auf dem Gastbetriebssystem aller virtuellen Maschinen im Cluster installieren.

Gibt es zwei Arten der.NET Framework 3.5 hinzugefügt. Eine Möglichkeit ist die Verwendung von Hinzufügen von Rollen und Features Assistent in Windows Abbildung 37 gezeigt.

![Abbildung 37: Installieren Sie.NET Framework 3.5 mithilfe von Hinzufügen von Rollen und Features Assistent][sap-ha-guide-figure-3028]

_**Abbildung 37:** Hinzufügen von Rollen und Features Assistent mit installieren Sie.NET Framework 3.5_

![Abbildung 38: Installation Statusanzeige Wenn Sie.NET Framework 3.5 installieren Hinzufügen von Rollen und Features Assistent][sap-ha-guide-figure-3029]

_**Abbildung 38:** Installation bei der Installation von.NET Framework 3.5 durch Hinzufügen von Rollen und Features Assistent Statusanzeige_

Die zweite Option für.NET Framework 3.5-Funktion aktivieren ist mit dem Befehlszeilenprogramm dism.exe. Für diese Art der Installation müssen Sie die parallele Verzeichnis auf dem Windows-Installationsmedium. Führen Sie diesen Befehl in ein Eingabeaufforderungsfenster mit erhöhten Rechten:

```
Dism /online /enable-feature /featurename:NetFx3 /All /Source:installation_media_drive:\sources\sxs /LimitAccess
```

#### <a name="dd41d5a2-8083-415b-9878-839652812102"></a>Installieren von SIOS DataKeeper

Installieren Sie SIOS DataKeeper Cluster Edition auf jedem Knoten im Cluster. Erstellen Sie mit SIOS DataKeeper virtuellen freigegebenen Speicher erstellen eine synchronisierte Spiegelung und simulieren Sie freigegebene Speicher.

Erstellen Sie vor der Installation der Software SIOS Domänenbenutzer **DataKeeperSvc**.

> [AZURE.NOTE] **DataKeeperSvc** Benutzer zu der **Lokalen** Administratorgruppe auf beiden Clusterknoten hinzufügen.

Installieren Sie die SIOS-Software auf beiden Clusterknoten.

![SIOS-Installationsprogramm][sap-ha-guide-figure-3030]

![Abbildung 39: Ersten Bildschirm der SIOS DataKeeper installation][sap-ha-guide-figure-3031]

_**Abbildung 39:** Ersten Bildschirm der SIOS DataKeeper installation_

![Abbildung 40: DataKeeper informiert, dass ein Dienst deaktiviert][sap-ha-guide-figure-3032]

_**Abbildung 40:** DataKeeper informiert Sie darüber, dass ein Dienst deaktiviert_

Wählen Sie **Ja**im Dialogfeld in Abbildung 40 angezeigt.

![Abbildung 41: Benutzerauswahl für SIOS DataKeeper][sap-ha-guide-figure-3033]

_**Abbildung 41:** Benutzerauswahl für SIOS DataKeeper_


Abbildung 41 Bildschirm empfiehlt **Domänenkonto oder Server**auswählen.

![Abbildung 42: Geben Sie den Domänenbenutzernamen und das Kennwort für die SIOS DataKeeper installation][sap-ha-guide-figure-3034]

_**Abbildung 42:** Geben Sie den Domänenbenutzernamen und das Kennwort für die SIOS DataKeeper installation_

Geben Sie die Domänenbenutzernamen und Kennwörter für SIOS DataKeeper erstellt.

![Abbildung 43: Geben Sie Ihre SIOS DataKeeper Lizenz][sap-ha-guide-figure-3035]

_**Abbildung 43:** Geben Sie Ihre SIOS DataKeeper Lizenz_

Installieren Sie den Lizenzschlüssel für die Instanz SIOS DataKeeper siehe Abbildung 43. Am Ende der Installation werden Sie aufgefordert, den virtuellen Computer neu zu starten.

#### <a name="d9c1fc8e-8710-4dff-bec2-1f535db7b006"></a>SIOS DataKeeper einrichten

Nachdem Sie SIOS DataKeeper auf beiden Knoten installieren, müssen Sie starten. Das Ziel der Konfiguration soll synchrone Datenreplikation zwischen zusätzlichen Festplatten an jeden der virtuellen Computer. Dies sind die Schritte, die Sie beide Knoten konfigurieren.

![Abbildung 44: SIOS DataKeeper Management und Konfigurationstools][sap-ha-guide-figure-3036]

_**Abbildung 44:** SIOS DataKeeper Management- und Konfigurations-Tools_

Starten Sie das Tool DataKeeper Verwaltung und Konfiguration, und wählen Sie **Server verbinden**. (In Abbildung 44 ist diese Option rot markiert.)

![Abbildung 45: Geben Sie den Namen oder TCP/IP-Adresse des ersten Knotens der Verwaltung und Konfigurationstool sollte auf, und im zweiten Schritt des zweiten Knotens][sap-ha-guide-figure-3037]

_**Abbildung 45:** Legen Sie den Namen oder die TCP/IP-Adresse des ersten Knotens, Management- und Konfigurations-Tool, und im zweiten Schritt den zweiten Knoten verbinden_

Der nächste Schritt ist die Replikation zwischen den beiden Knoten erstellen.

![Abbildung 46: Erstellen einer Replikationsauftrag][sap-ha-guide-figure-3038]

_**Abbildung 46:** Erstellen eines Auftrags Replikation_

Ein Assistent führt Sie durch den Prozess der Erstellung einer Replikationsauftrag.

![Abbildung 47: Definieren Sie der Name des Replikationsjobs][sap-ha-guide-figure-3039]

_**Abbildung 47:** Der Name des Replikationsjobs definieren_

![Abbildung 48: Definieren Sie die Daten für den Knoten und die aktuelle Quellknoten][sap-ha-guide-figure-3040]

_**Abbildung 48:** Definieren Sie die Daten für den Knoten und die aktuelle Quellknoten_

Im ersten Schritt müssen Sie den Namen, die TCP/IP-Adresse und den Datenträger des Quellknotens definieren. Der zweite Schritt ist den Zielknoten definieren. Wie bereits erwähnt, müssen Sie Name, TCP/IP-Adresse und Datenträger des Zielknotens definieren.

![Abbildung 49: Definieren Sie die Daten für den Knoten und die aktuelle Zielknoten][sap-ha-guide-figure-3041]

_**Abbildung 49:** Definieren Sie die Daten für den Knoten und die aktuelle Zielknoten_

Definieren Sie danach die Komprimierungsalgorithmen. In unserem Beispiel wird empfohlen, den Replikationsstream komprimieren. Insbesondere Resynchronisation reduziert die Komprimierung von den Replikationsstream Resynchronisation Zeit. Beachten Sie, dass die Komprimierung mithilfe der CPU und RAM-Ressourcen eines virtuellen Computers. Die Komprimierungsrate zunimmt, wächst das Volumen der verwendeten CPU-Ressourcen. Sie können diese Einstellung später ändern.

Eine andere Einstellung müssen Sie überprüfen ist, ob die Replikation asynchron oder synchron. *Beim Schutz von SAP ASC/SCS Konfigurationen muss synchronen Replikation verwendet*.  

![Abbildung 50: Replikationsdetails definieren][sap-ha-guide-figure-3042]

_**Abbildung 50:** Replikationsdetails definieren_

Im letzte Schritt wird definiert, ob das Volume durch die Replikation repliziert eine Cluster-Konfiguration mit Windows Server Failover Clustering als freigegeben dargestellt werden sollen. Wählen Sie für SAP ASC/SCS-Konfiguration **Ja** , damit Windows Cluster des Replikat-Volumes als freigegeben sieht als Cluster-Datenträger verwenden können.

![Abbildung 51: Wählen Sie ** Ja ** replizierten Volumes als Clustervolume festlegen][sap-ha-guide-figure-3043]

_**Abbildung 51:** Wählen Sie **Ja** replizierten Volumes als Clustervolume festlegen_

Nachdem der Datenträger erstellt wurde, zeigt das Tool DataKeeper Verwaltung und Konfiguration, die Replikation aktiviert ist.

![Abbildung 52: DataKeeper synchrone Spiegelung für SAP ASC/SCS freigegebenen Datenträger ist aktiv][sap-ha-guide-figure-3044]

_**Abbildung 52:** DataKeeper für SAP ASC/SCS freigegebenen Datenträger synchrone Spiegelung ist aktiv_

Zeigt Failovercluster-Manager den Datenträger als Datenträger DataKeeper, siehe Abbildung 53.

![Abbildung 53: Failovercluster-Manager zeigt die Festplatte DataKeeper repliziert][sap-ha-guide-figure-3045]

_**Abbildung 53:** Failovercluster-Manager zeigt der Festplatte DataKeeper repliziert_


## <a name="a06f0b49-8a7a-42bf-8b0d-c12026c5746b"></a>Installieren der SAP NetWeaver

Da das DBMS-System Installationen je nach verwendeten wird nicht das DBMS Setup beschrieben. Allerdings wird angenommen, dass hohe Verfügbarkeit mit dem DBMS mit Funktionen Probleme sind verschiedenen DBMS Kreditoren für Azure unterstützen. Z. B. ständig oder Datenbankspiegeln für SQL Server und Oracle Data Guard für Oracle. In dem Szenario in diesem Artikel verwenden wir haben wir mehr Schutz DBMS hinzufügen.

Diese Art von SAP ASC/SCS-Clusterkonfiguration in Azure verschiedene DBMS Dienste interagieren nicht Aspekte.


> [AZURE.NOTE] Die Installationsprozeduren SAP NetWeaver ABAP Systeme, Java-Systeme und ABAP + Java-Systeme sind nahezu identisch. Der wichtigste Unterschied ist, dass eine System SAP ABAP einmal ASC. SAP-Java-System verfügt über eine SCS-Instanz. SAP ABAP + Java System hat einmal ASC und SCS einmal im gleichen Microsoft Failover Cluster-Gruppe. Installation Unterschiede für SAP NetWeaver Installation Stapel sind explizit. Sie können davon ausgehen, dass alle Teile gleich sind.  

### <a name="31c6bd4f-51df-4057-9fdf-3fcbc619c170"></a>Installieren Sie SAP eine hohe Verfügbarkeit ASC-SCS-Instanz

> [AZURE.IMPORTANT] Achten Sie darauf, nicht die Seite Datei auf DataKeeper gespiegelte Volumes. DataKeeper unterstützt keine gespiegelten Volumes. Lassen Sie die Auslagerungsdatei auf dem temporären Laufwerk D eine Azure virtuellen Computer die Standardeinstellung. Wenn es nicht bereits vorhanden ist, wechseln Sie die Windows-Auslagerungsdatei auf Laufwerk D des virtuellen Computers Azure

#### <a name="a97ad604-9094-44fe-a364-f89cb39bf097"></a>Erstellen Sie einen virtuellen Hostnamen für SAP ASC/SCS Clusterinstanz

Erstellen Sie in Windows DNS-Manager zunächst einen DNS-Eintrag für den virtuellen Hostnamen ASC-SCS-Instanz. Definieren Sie dann die IP-Adresse des virtuellen Hostnamens.

> [AZURE.NOTE]  
Beachten Sie, dass die IP-Adresse, die dem virtuellen Hostnamen ASC-SCS-Instanz zuweisen der IP-Adresse identisch ist, die Lastenausgleich Azure zugewiesen (<*SID*> - lb - ASC).  

Die IP-Adresse des virtuellen SAP ASC/SCS-Hostnamens (pr1 ASC Sap) ist die IP-Adresse des Azure-Lastenausgleich (pr1 lb Ascs) identisch.

Nur ein SAP-Failover Cluster-Rolle kann in einer Windows Server-Failovercluster in Azure ausführen. Beispielsweise bedeutet dies einmal für das System ABAP ASC und eine SCS-Instanz für das Java-System. ABAP + Java wäre es einmal ASC und einer SCS-Instanz.

> [AZURE.NOTE] Derzeit Multi-SID-clustering Siehe Installationshandbücher SAP (siehe [SAP Installationshandbücher][sap-installation-guides]) in Azure funktioniert nicht.

![Abbildung 54: Definieren Sie den DNS-Eintrag für SAP ASC/SCS Cluster virtuellen Namen und TCP/IP-Adresse][sap-ha-guide-figure-3046]

_**Abbildung 54:** Definieren des DNS-Eintrags für SAP ASC/SCS Cluster virtuellen Namen und TCP/IP-Adresse_

Der Eintrag ist unter der Domäne im DNS-Manager, siehe Abbildung 55.

![Abbildung 55: Neuen virtuellen Namen und Serveradresse für SAP ASC/SCS-Clusterkonfiguration][sap-ha-guide-figure-3047]

_**Abbildung 55:** Neuen virtuellen Namen und TCP/IP-Adresse für SAP ASC/SCS-Clusterkonfiguration_

#### <a name="eb5af918-b42f-4803-bb50-eff41f84b0b0"></a>Installieren von SAP ersten Cluster-Knoten

Den ersten SAP-Cluster installieren, führen Sie die erste Cluster-Knoten-Option auf Knoten a Auf Host **pr1 ASC 0** .

Wenn Sie möchten die Standardports für interne Azure Lastenausgleich, wählen Sie:

- **ABAP System**: **ASC** Instanz Nummer **00**
- **Java System**: **SCS** Instanz Nummer **01**
- **ABAP + JAVA System**: **ASC** Instanz Nummer **00** und **SCS** -Instanz Nummer **01**

Wenn Sie Instanz als 00 01 für Java SCS-Instanz zu ABAP ASCS-Instanz verwenden möchten, müssen Sie zunächst den Azure interne Load Balancer Standard Lastenausgleich Regeln gemäß [ASC-SCS Standard Lastenausgleich für Azure internen Lastenausgleich Regeln ändern] ändern [Sap-ha-Handbuch-8,9].

Führen Sie einige Schritte, die im normalen SAP-Installationsdokumentation beschrieben sind nicht.

> [AZURE.NOTE] SAP-Installationsdokumentation beschreibt die ersten ASC-SCS-Clusterknoten installieren.

#### <a name="e4caaab2-e90f-4f2c-bc84-2cd2e12a9556"></a>Ändern Sie das Profil SAP ASC/SCS i'' ' Powershellnstance

Sie müssen einen neues Profilparameter hinzufügen. Der Profilparameter verhindert Verbindungen zwischen SAP Arbeitsverfahren und Enqueue Server geschlossen wird, wenn sie zu lange im Leerlauf befinden. Erwähnt Problemszenario [Registrierungseinträge auf beiden Clusterknoten SAP ASC/SCS-Instanz hinzufügen] [Sap-ha-Handbuch-8.11] in diesem Artikel. In diesem Abschnitt haben wir auch zwei einige grundlegende TCP/IP-Parameter. In einem zweiten Schritt müssen Sie den Enqueue Server **Keep_alive** signalisieren, dass Anschlüsse der Azure interne Load-Balancer Leerlauf Schwellenwert getroffen haben.

SAP ASC/SCS instanzprofil dieses Profilparameter hinzufügen:
```
enque/encni/set_so_keepalive = true
```
In unserem Beispiel lautet der Pfad:

`<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_ASCS00_pr1-ascs-sap`

Auf SAP SCS instanzprofil und entsprechenden Pfad:

`<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_SCS01_pr1-ascs-sap`


#### <a name="10822f4f-32e7-4871-b63a-9b86c76ce761"></a>Fügen Sie einen Prüfpunkt

Verwenden der internen Lastenausgleich Prüfpunkt Funktionalität die gesamte Cluster-Konfiguration mit Lastenausgleich arbeiten. Azure internen Lastenausgleich verteilt normalerweise eingehende Arbeitslast zwischen teilnehmenden virtuellen Maschinen. Jedoch funktioniert dies in einigen Clusterkonfigurationen nicht, da nur eine Instanz aktiv ist. Die Instanz ist Passiv und kann die Arbeitslast nicht annehmen. Probe-Funktionen kann bei Azure internen Lastenausgleich Arbeit nur eine aktive Instanz zugewiesen. Probe-Funktionen kann Instanzen aktiv sind, und diesen dann die Instanz mit der internen Lastenausgleich erkennen.

Überprüfen Sie zunächst die aktuelle Einstellung der **ProbePort** mit dem folgenden PowerShell-Befehl. Führen Sie in einem virtuellen Computer in der Cluster-Konfiguration:

```PowerShell
Get-ClusterResource „SAP PR1 IP" | Get-ClusterParameter
```

![Abbildung 56: Cluster-Konfiguration Prüfpunkt Port ist standardmäßig 0][sap-ha-guide-figure-3048]

_**Abbildung 56:** Der Cluster-Konfiguration Prüfpunkt Anschluss ist standardmäßig 0_

Definieren Sie dann einen Prüfpunkt-Port. Die Standardportnummer Prüfpunkt ist 0. In unserem Beispiel verwenden wir Prüfpunkt port **62300**.

Die Anschlussnummer wird in SAP Azure Ressourcenmanager Vorlagen definiert. Sie können die Portnummer in PowerShell zuweisen.

Zuerst rufen Sie virtuellen Hosts SAP **SAP WAC IP**-Clusterressource.

```PowerShell
$var = Get-ClusterResource | Where-Object {  $_.name -eq "SAP PR1 IP"  }
```

Legen Sie dann den Prüfpunkt-Anschluss **62300**.

```PowerShell
$var | Set-ClusterParameter -Multiple @{"Address"="10.0.0.43";"ProbePort"=62300;"Subnetmask"="255.255.255.0";"Network"="Cluster Network 1";"OverrideAddressMatch"=1;"EnableDhcp"=0}  
```

Um die Änderung zu aktivieren, beenden Sie und starten Sie Clusterrolle **SAP PR1** .

Nachdem Sie mit **SAP PR1** Clusterrolle, überprüfen Sie, ob **ProbePort** auf den neuen Wert:

```PowerShell
Get-ClusterResource „SAP PR1 IP" | Get-ClusterParameter
```

![Abbildung 57: Prüfpunkt clusterport, nachdem Sie den neuen Wert festgelegt][sap-ha-guide-figure-3049]

_**Abbildung 57:** Clusterport Prüfpunkt, nachdem Sie den neuen Wert festgelegt_

Die **ProbePort** wird auf **62300**festgelegt. Nachdem Sie die Dateifreigabe zugreifen können ** \\\ascsha-clsap\sapmnt** von anderen Hosts wie **Ascsha Dbas**.

### <a name="85d78414-b21d-4097-92b6-34d8bcb724b7"></a>Installieren der Datenbankinstanz

Folgen Sie zum Installieren der Datenbankinstanz in der SAP-Installation-Dokumentation beschrieben.

### <a name="8a276e16-f507-4071-b829-cdc0a4d36748"></a>Dem zweiten Clusterknoten zu installieren

Um den zweiten Cluster zu installieren, folgen Sie den Schritten im SAP-Installationshandbuch.

### <a name="094bc895-31d4-4471-91cc-1513b64e406a"></a>Ändern des Starttyps SAP bietet Windows-Dienstinstanz

Ändern Sie den Starttyp des Dienstes SAP Enqueue Replication Server (ERS) Windows automatisch **(Verzögerter Start)** auf beiden Clusterknoten.

![Abbildung 58: Ändern Sie Diensttyp für SAP ERS Instanz verzögerte automatisch][sap-ha-guide-figure-3050]

_**Abbildung 58:** Ändern Sie Diensttyp für SAP ERS Instanz verzögerte automatisch_

### <a name="2477e58f-c5a7-4a5d-9ae3-7b91022cafb5"></a>Installieren Sie den primären SAP-Anwendungsserver

Installieren Sie die primäre Anwendung Server (PAS) Instanz <*SID*> - di - 0 auf dem virtuellen Computer, den Sie zum Hosten der PAS festgelegt haben. Es gibt keine Abhängigkeiten zu Azure oder DataKeeper Besonderheiten.

### <a name="0ba4a6c1-cc37-4bcf-a8dc-025de4263772"></a>Weitere SAP-Anwendungsserver installieren

Installieren Sie einen SAP zusätzliche Application Server (AAS) auf allen virtuellen Computern, die zum Hosten von SAP-Anwendungsserver festgelegt haben. Auf <*SID*> - di - 1 <*SID*> - di -<n>.

## <a name="18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9"></a>Testen von SAP ASC/SCS Instanz Failover- und SIOS Replikation

Es ist einfach zum Testen und Überwachen einer SAP ASC/SCS Instanz Failover- und SIOS Laufwerksreplikation mit Failovercluster-Manager und die SIOS-DataKeeper-Benutzeroberfläche.

### <a name="65fdef0f-9f94-41f9-b314-ea45bbfea445"></a>SAP ASC/SCS wird auf Knoten A ausgeführt

Die Clustergruppe **SAP WAC** läuft auf Knoten a Auf **Ascsha Clna**. Knoten a weisen Sie freigegebene Laufwerk S gehört **SAP WAC** Clustergruppe und die ASC-SCS-Instanz verwendet zu,

![Abbildung 59: Failovercluster-Manager: der SAP < * SID * > Clustergruppe auf Knoten A ausgeführt wird][sap-ha-guide-figure-5000]

_**Abbildung 59:** Failovercluster-Manager: der SAP <*SID*> Clustergruppe auf Clusterknoten ein läuft_

Mithilfe der SIOS-DataKeeper-Benutzeroberfläche sehen Sie, dass die freigegebenen Datenträgerdaten synchron aus Quelle Volume Laufwerk S auf Knoten A Target Volume Laufwerk S auf Knoten b repliziert werden Aus **Ascsha-Clna [10.0.0.41]** , **[10.0.0.42] Ascsha-Clnb**.

![Abbildung 60: In SIOS DataKeeper Replizieren des lokalen Volumes von Clusterknoten ein Cluster-Knoten B][sap-ha-guide-figure-5001]

_**Abbildung 60:** In SIOS DataKeeper Replizieren des lokalen Volumes von Clusterknoten ein Cluster-Knoten B_


### <a name="5e959fa9-8fcd-49e5-a12c-37f6ba07b916"></a>Failover von Knoten A auf Knoten B

Eine der folgenden Optionen können Sie ein Failover der Clustergruppe SAP <*SID*> von Knoten A zu Knoten b zu starten

- Failovercluster-Manager verwenden  
- Failovercluster PowerShell verwenden
  ```powershell
  Move-ClusterGroup -Name "SAP WAC"
  ```
- Starten Sie Cluster Knoten A in die Windows-Gastbetriebssystem (initiiert ein automatisches Failover der Clustergruppe SAP <*SID*> von Knoten A zu Knoten B)  
- Starten Sie Cluster Knoten A von Azure-Portal (initiiert ein automatisches Failover der Clustergruppe SAP <*SID*> von Knoten A zu Knoten B)  
- Starten Sie Cluster Knoten A mithilfe von Azure PowerShell (initiiert ein automatisches Failover der Clustergruppe SAP <*SID*> von Knoten A zu Knoten B)

  ```powershell
  Restart-AzureVM -Name ascsha-clna -ServiceName ascsha-cluster
  ```

Nach einem Failover wird die Clustergruppe SAP <*SID*> auf Knoten b ausgeführt Auf **Ascsha Clnb**.

![Abbildung 61: Im Failovercluster-Manager, die SAP < * SID * > Clustergruppe auf Clusterknoten B ausgeführt][sap-ha-guide-figure-5002]

_**Abbildung 61**: Failover Cluster-Manager die Clustergruppe SAP <*SID*> Clusterknoten B läuft_

Nun der freigegebene Datenträger auf Clusterknoten montiert ist b SIOS DataKeeper Source Volume Laufwerk S auf Clusterknoten B Target Volume Laufwerk S auf Knoten a Replizieren von Daten Aus **Ascsha-Clnb [10.0.0.42]** , **[10.0.0.41] Ascsha-Clna**.


![Abbildung 62: SIOS DataKeeper repliziert lokalen Volumes von Clusterknoten B cluster-Knoten A][sap-ha-guide-figure-5003]

_**Abbildung 62:** SIOS DataKeeper repliziert des lokalen Volumes von Cluster-Knoten B, Knoten A cluster_
