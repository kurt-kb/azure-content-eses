<properties
   pageTitle="Uso de SAP en máquinas virtuales de Linux (VM)| Microsoft Azure"
   description="Aprenda a usar SAP en máquinas virtuales de Linux (VM)en Microsoft Azure"
   services="virtual-machines-linux,virtual-network,storage"
   documentationCenter="saponazure"
   authors="MSSedusch"
   manager="juergent"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="campaign-page"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="na"
   ms.date="05/17/2016"
   ms.author="sedusch"/>

# Uso de SAP en máquinas virtuales de Linux (VM)

[767598]: https://service.sap.com/sap/support/notes/767598
[773830]: https://service.sap.com/sap/support/notes/773830
[826037]: https://service.sap.com/sap/support/notes/826037
[965908]: https://service.sap.com/sap/support/notes/965908
[1139904]: https://service.sap.com/sap/support/notes/1139904
[1173395]: https://service.sap.com/sap/support/notes/1173395
[1245200]: https://service.sap.com/sap/support/notes/1245200
[1409604]: https://service.sap.com/sap/support/notes/1409604
[1558958]: https://service.sap.com/sap/support/notes/1558958
[1585981]: https://service.sap.com/sap/support/notes/1585981
[1588316]: https://service.sap.com/sap/support/notes/1588316
[1590719]: https://service.sap.com/sap/support/notes/1590719
[1605680]: https://service.sap.com/sap/support/notes/1605680
[1619720]: https://service.sap.com/sap/support/notes/1619720
[1619726]: https://service.sap.com/sap/support/notes/1619726
[1619967]: https://service.sap.com/sap/support/notes/1619967
[1750510]: https://service.sap.com/sap/support/notes/1750510
[1752266]: https://service.sap.com/sap/support/notes/1752266
[1757924]: https://service.sap.com/sap/support/notes/1757924
[1757928]: https://service.sap.com/sap/support/notes/1757928
[1758182]: https://service.sap.com/sap/support/notes/1758182
[1758496]: https://service.sap.com/sap/support/notes/1758496
[1772688]: https://service.sap.com/sap/support/notes/1772688
[1814258]: https://service.sap.com/sap/support/notes/1814258
[1882376]: https://service.sap.com/sap/support/notes/1882376
[1909114]: https://service.sap.com/sap/support/notes/1909114
[1922555]: https://service.sap.com/sap/support/notes/1922555
[1928533]: https://service.sap.com/sap/support/notes/1928533
[1941500]: https://service.sap.com/sap/support/notes/1941500
[1956005]: https://service.sap.com/sap/support/notes/1956005
[1973241]: https://service.sap.com/sap/support/notes/1973241
[1999351]: https://service.sap.com/sap/support/notes/1999351
[2015553]: https://service.sap.com/sap/support/notes/2015553
[2039619]: https://service.sap.com/sap/support/notes/2039619
[2121797]: https://service.sap.com/sap/support/notes/2121797
[2134316]: https://service.sap.com/sap/support/notes/2134316
[2178632]: https://service.sap.com/sap/support/notes/2178632
[2191498]: https://service.sap.com/sap/support/notes/2191498
[2233094]: https://service.sap.com/sap/support/notes/2233094

[azure-portal]: https://portal.azure.com
[azure-script-ps]: https://go.microsoft.com/fwlink/p/?LinkID=395017

[planning-guide]: virtual-machines-linux-sap-planning-guide.md "SAP NetWeaver en máquinas virtuales (VM) de Linux – Guía de planeación e implementación"
[planning-guide-classic]: virtual-machines-windows-classic-sap-planning-guide.md
[deployment-guide]: virtual-machines-linux-sap-deployment-guide.md "SAP NetWeaver en máquinas virtuales (VM) de Linux – Guía de implementación"
[deployment-guide-classic]: virtual-machines-windows-classic-sap-deployment-guide.md
[dbms-guide]: virtual-machines-linux-sap-dbms-guide.md "SAP NetWeaver en máquinas virtuales (VM) de Linux – Guía de implementación de DBMS"
[dbms-guide-classic]: virtual-machines-windows-classic-sap-dbms-guide.md
[dr-guide-classic]: virtual-machines-windows-classic-sap-dr-guide.md
[ha-guide-classic]: virtual-machines-windows-classic-sap-ha-guide.md

[getting-started]: virtual-machines-linux-sap-getting-started-arm.md
[getting-started-windows-classic]: virtual-machines-windows-classic-sap-getting-started.md

[getting-started-windows-classic-dbms]: virtual-machines-windows-classic-sap-getting-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-planning]: virtual-machines-windows-classic-sap-getting-started.md#f2a5e9d8-49e4-419e-9900-af783173481c
[getting-started-windows-classic-deployment]: virtual-machines-windows-classic-sap-getting-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]: virtual-machines-windows-classic-sap-getting-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]: virtual-machines-windows-classic-sap-getting-started.md#4bb7512c-0fa0-4227-9853-4004281b1037

[getting-started-planning]: virtual-machines-linux-sap-getting-started-arm.md#3da0389e-708b-4e82-b2a2-e92f132df89c
[getting-started-deployment]: virtual-machines-linux-sap-getting-started-arm.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-dbms]: virtual-machines-linux-sap-getting-started-arm.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82

[deployment-guide-2.2]: virtual-machines-linux-sap-deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 "Recursos SAP"
[deployment-guide-3]: virtual-machines-linux-sap-deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e "Escenarios de implementación de máquinas virtuales para SAP en Microsoft Azure"
[deployment-guide-3.1.2]: virtual-machines-linux-sap-deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab "Implementación de una máquina virtual con una imagen personalizada"
[deployment-guide-3.2]: virtual-machines-linux-sap-deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 "Escenario 1: Implementación de una máquina virtual de Azure Marketplace para SAP"
[deployment-guide-3.3]: virtual-machines-linux-sap-deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 "Escenario 2: Implementación de una máquina virtual con una imagen personalizada para SAP"
[deployment-guide-3.4]: virtual-machines-linux-sap-deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1 "Escenario 3: Mover una máquina virtual desde una ubicación local con un VHD no generalizado de Azure con SAP"
[deployment-guide-4.1]: virtual-machines-linux-sap-deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 "Implementación de cmdlets de Azure PowerShell"
[deployment-guide-4.2]: virtual-machines-linux-sap-deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e "Descargar e importar cmdlets de PowerShell para SAP"
[deployment-guide-4.3]: virtual-machines-linux-sap-deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc "Unir máquinas virtuales en dominio local - solo Windows"
[deployment-guide-4.4]: virtual-machines-linux-sap-deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d "Descargar, instalar y habilitar el agente de máquina virtual de Azure"
[deployment-guide-4.4.2]: virtual-machines-linux-sap-deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 "Linux"
[deployment-guide-4.5]: virtual-machines-linux-sap-deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca "Configurar la extensión de supervisión mejorada de Azure para SAP"
[deployment-guide-4.5.1]: virtual-machines-linux-sap-deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 "Azure PowerShell"
[deployment-guide-4.5.2]: virtual-machines-linux-sap-deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f "Azure CLI"
[deployment-guide-5.1]: virtual-machines-linux-sap-deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 "Comprobación de preparación de la supervisión mejorada de Azure para SAP"
[deployment-guide-5.2]: virtual-machines-linux-sap-deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1 "Comprobación de estado de la configuración de la infraestructura de supervisión de Azure"
[deployment-guide-5.3]: virtual-machines-linux-sap-deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 "Más información sobre solución de problemas de la infraestructura de supervisión de Azure para SAP"
[deployment-guide-install-vm-agent-windows]: virtual-machines-linux-sap-deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-configure-proxy]: virtual-machines-linux-sap-deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d "Configurar el proxy"
[deployment-guide-configure-monitoring-scenario-1]: virtual-machines-linux-sap-deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b "Configurar la supervisión"
[deployment-guide-troubleshooting-chapter]: virtual-machines-linux-sap-deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b "Comprobaciones y solución de problemas de configuración de supervisión de extremo a extremo para SAP en Azure"
[deployment-guide-figure-100]: ./media/virtual-machines-linux-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-300]: ./media/virtual-machines-linux-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]: ./media/virtual-machines-linux-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-500]: ./media/virtual-machines-linux-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-600]: ./media/virtual-machines-linux-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-700]: ./media/virtual-machines-linux-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]: ./media/virtual-machines-linux-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-50]: ./media/virtual-machines-linux-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-900]: ./media/virtual-machines-linux-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-1000]: ./media/virtual-machines-linux-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-1100]: ./media/virtual-machines-linux-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]: ./media/virtual-machines-linux-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]: ./media/virtual-machines-linux-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-1400]: ./media/virtual-machines-linux-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-5]: virtual-machines-linux-sap-deployment-guide.md#figure-5
[deployment-guide-figure-6]: virtual-machines-linux-sap-deployment-guide.md#figure-6
[deployment-guide-figure-7]: virtual-machines-linux-sap-deployment-guide.md#figure-7
[deployment-guide-figure-11]: virtual-machines-linux-sap-deployment-guide.md#figure-11
[deployment-guide-figure-14]: virtual-machines-linux-sap-deployment-guide.md#figure-14
[deployment-guide-figure-azure-cli-installed]: virtual-machines-linux-sap-deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]: virtual-machines-linux-sap-deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda

[planning-guide-1.2]: virtual-machines-linux-sap-planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff "Recursos"
[planning-guide-2.1]: virtual-machines-linux-sap-planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803 "Solo en la nube: implementaciones de máquina Virtual en Azure sin dependencias en la red local del cliente"
[planning-guide-2.2]: virtual-machines-linux-sap-planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 "Entre locales: implementación de una o varias máquinas virtuales de SAP en Azure con el requisito de estar totalmente integradas en la red local"
[planning-guide-3.1]: virtual-machines-linux-sap-planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a "Regiones de Azure"
[planning-guide-3.2]: virtual-machines-linux-sap-planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 "El concepto de máquina virtual de Microsoft Azure"
[planning-guide-3.2.1]: virtual-machines-linux-sap-planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 "Dominios de error"
[planning-guide-3.2.2]: virtual-machines-linux-sap-planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 "Dominios de actualización"
[planning-guide-3.2.3]: virtual-machines-linux-sap-planning-guide.md#18810088-f9be-4c97-958a-27996255c665 "Conjuntos de disponibilidad de Azure"
[planning-guide-3.3.2]: virtual-machines-linux-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 "Almacenamiento Premium de Azure"
[planning-guide-5.1.1]: virtual-machines-linux-sap-planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 "Mover máquinas virtuales del entorno local a Azure con un disco no generalizado"
[planning-guide-5.1.2]: virtual-machines-linux-sap-planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c "Implementación de una máquina virtual con una imagen específica del cliente"
[planning-guide-5.2]: virtual-machines-linux-sap-planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 "Preparación de máquinas virtuales con SAP para Azure"
[planning-guide-5.2.1]: virtual-machines-linux-sap-planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef "Preparación para mover máquinas virtuales del entorno local a Azure con un disco no generalizado"
[planning-guide-5.2.2]: virtual-machines-linux-sap-planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 "Preparación para la implementación de una máquina virtual con una imagen específica del cliente para SAP"
[planning-guide-5.3.1]: virtual-machines-linux-sap-planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 "Diferencia entre un disco y una imagen de Azure"
[planning-guide-5.3.2]: virtual-machines-linux-sap-planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a "Carga de un VHD desde una instalación local a Azure"
[planning-guide-5.4.2]: virtual-machines-linux-sap-planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 "Copiar discos entre cuentas de almacenamiento de Azure"
[planning-guide-5.5.1]: virtual-machines-linux-sap-planning-guide.md#4efec401-91e0-40c0-8e64-f2dceadff646 "Estructura de VM/VHD para implementaciones SAP"
[planning-guide-5.5.3]: virtual-machines-linux-sap-planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d "Configuración de montaje automático para discos conectados"
[planning-guide-7]: virtual-machines-linux-sap-planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 "Conceptos de implementación solo en la nube de instancias de SAP"
[planning-guide-7.1]: virtual-machines-linux-sap-planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 "Escenario de demostración/aprendizaje de máquina virtual única con SAP NetWeaver"
[planning-guide-9.1]: virtual-machines-linux-sap-planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 "Solución de supervisión de Azure para SAP"
[planning-guide-11.4.1]: virtual-machines-linux-sap-planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 "Alta disponibilidad para servidores de aplicaciones SAP"
[planning-guide-11.5]: virtual-machines-linux-sap-planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f "Uso del inicio automático para instancias de SAP"
[planning-guide-microsoft-azure-networking]: virtual-machines-linux-sap-planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd "Redes de Microsoft Azure"
[planning-guide-storage-microsoft-azure-storage-and-data-disks]: virtual-machines-linux-sap-planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f "Almacenamiento: Almacenamiento de Microsoft Azure y discos de datos"
[planning-guide-azure-premium-storage]: virtual-machines-linux-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 "Almacenamiento Premium de Azure"
[planning-guide-figure-100]: ./media/virtual-machines-linux-sap-planning-guide/100-single-vm-in-azure.png
[planning-guide-figure-200]: ./media/virtual-machines-linux-sap-planning-guide/200-multiple-vms-in-azure.png
[planning-guide-figure-300]: ./media/virtual-machines-linux-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-400]: ./media/virtual-machines-linux-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]: ./media/virtual-machines-linux-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]: ./media/virtual-machines-linux-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]: ./media/virtual-machines-linux-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-figure-1300]: ./media/virtual-machines-linux-sap-planning-guide/1300-ref-config-iaas-for-sap.png
[planning-guide-figure-1400]: ./media/virtual-machines-linux-sap-planning-guide/1400-attach-detach-disks.png
[planning-guide-figure-1600]: ./media/virtual-machines-linux-sap-planning-guide/1600-firewall-port-rule.png
[planning-guide-figure-1700]: ./media/virtual-machines-linux-sap-planning-guide/1700-single-vm-demo.png
[planning-guide-figure-1900]: ./media/virtual-machines-linux-sap-planning-guide/1900-vm-set-vnet.png
[planning-guide-figure-2100]: ./media/virtual-machines-linux-sap-planning-guide/2100-s2s.png
[planning-guide-figure-2200]: ./media/virtual-machines-linux-sap-planning-guide/2200-network-printing.png
[planning-guide-figure-2300]: ./media/virtual-machines-linux-sap-planning-guide/2300-sapgui-stms.png
[planning-guide-figure-2400]: ./media/virtual-machines-linux-sap-planning-guide/2400-vm-extension-overview.png
[planning-guide-figure-2500]: ./media/virtual-machines-linux-sap-planning-guide/2500-vm-extension-details.png
[planning-guide-figure-2600]: ./media/virtual-machines-linux-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]: ./media/virtual-machines-linux-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]: ./media/virtual-machines-linux-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]: ./media/virtual-machines-linux-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-3000]: ./media/virtual-machines-linux-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]: ./media/virtual-machines-linux-sap-planning-guide/3200-sap-ha-with-sql.png

[dbms-guide-2]: virtual-machines-linux-sap-dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 "Estructura de una implementación de RDBMS"
[dbms-guide-2.1]: virtual-machines-linux-sap-dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f "Almacenamiento en caché de máquinas virtuales y VHD"
[dbms-guide-2.2]: virtual-machines-linux-sap-dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 "Software RAID"
[dbms-guide-2.3]: virtual-machines-linux-sap-dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 "Almacenamiento de Microsoft Azure"
[dbms-guide-3]: virtual-machines-linux-sap-dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 "Alta disponibilidad y recuperación ante desastres con máquinas virtuales de Azure"
[dbms-guide-5]: virtual-machines-linux-sap-dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 "Detalles para SQL Server RDBMS"
[dbms-guide-5.5.1]: virtual-machines-linux-sap-dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268 "SQL Server 2012 SP1 CU4 y versiones posteriores"
[dbms-guide-5.5.2]: virtual-machines-linux-sap-dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b "SQL Server 2012 SP1 CU3 y versiones anteriores"
[dbms-guide-5.6]: virtual-machines-linux-sap-dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 "Uso de imágenes de SQL Server fuera de Microsoft Azure Marketplace"
[dbms-guide-5.8]: virtual-machines-linux-sap-dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 "Resumen general de SQL Server para SAP en Azure"
[dbms-guide-8.4.1]: virtual-machines-linux-sap-dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 "Configuración de almacenamiento"
[dbms-guide-8.4.2]: virtual-machines-linux-sap-dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d "Copia de seguridad y restauración"
[dbms-guide-8.4.3]: virtual-machines-linux-sap-dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c "Consideraciones de rendimiento sobre copias de seguridad y restauraciones"
[dbms-guide-8.4.4]: virtual-machines-linux-sap-dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 "Otros"
[dbms-guide-figure-100]: ./media/virtual-machines-linux-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]: ./media/virtual-machines-linux-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]: ./media/virtual-machines-linux-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]: ./media/virtual-machines-linux-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]: ./media/virtual-machines-linux-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]: ./media/virtual-machines-linux-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]: ./media/virtual-machines-linux-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]: ./media/virtual-machines-linux-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]: ./media/virtual-machines-linux-sap-dbms-guide/900-sap-cache-server-on-premises.png

[dbms-guide-900-sap-cache-server-on-premises]: virtual-machines-linux-sap-dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[Logo_Windows]: ./media/virtual-machines-linux-sap-shared/Windows.png
[Logo_Linux]: ./media/virtual-machines-linux-sap-shared/Linux.png

[vm-size-specs]: virtual-machines-size-specs.md
[azure-subscription-service-limits-subscription]: azure-subscription-service-limits.md#subscription
[vpn-gateway-create-site-to-site-rm-powershell]: vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-cross-premises-options]: vpn-gateway-cross-premises-options.md
[vpn-gateway-site-to-site-create]: vpn-gateway-site-to-site-create.md
[virtual-machines-deploy-rmtemplates-azure-cli]: virtual-machines-deploy-rmtemplates-azure-cli.md "Implementación y administración de máquinas virtuales con plantillas del Administrador de recursos de Azure y CLI de Azure"
[virtual-machines-deploy-rmtemplates-powershell]: virtual-machines-deploy-rmtemplates-powershell.md "Administración de máquinas virtuales con el Administrador de recursos de Azure y con PowerShell"
[virtual-machines-linux-capture-image-resource-manager]: virtual-machines-linux-capture-image-resource-manager.md
[virtual-machines-manage-availability]: virtual-machines-manage-availability.md
[virtual-machines-linux-how-to-attach-disk]: virtual-machines-linux-how-to-attach-disk.md
[virtual-networks-reserved-private-ip]: virtual-networks-static-private-ip-arm-ps.md
[virtual-machines-sql-server-infrastructure-services]: virtual-machines-sql-server-infrastructure-services.md
[storage-redundancy]: storage-redundancy.md
[storage-scalability-targets]: storage-scalability-targets.md
[virtual-networks-manage-dns-in-vnet]: virtual-networks-manage-dns-in-vnet.md
[resource-groups-networking]: resource-groups-networking.md
[virtual-networks-static-private-ip-arm-pportal]: virtual-networks-static-private-ip-arm-pportal.md
[virtual-networks-multiple-nics]: virtual-networks-multiple-nics.md
[virtual-network-deploy-multinic-arm-template]: virtual-network-deploy-multinic-arm-template.md
[virtual-network-deploy-multinic-arm-ps]: virtual-network-deploy-multinic-arm-ps.md
[virtual-network-deploy-multinic-arm-cli]: virtual-network-deploy-multinic-arm-cli.md
[vpn-gateway-create-site-to-site-rm-powershell]: vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-create-site-to-site-rm-powershell]: vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-about-vpn-devices]: vpn-gateway-about-vpn-devices.md
[vpn-gateway-vpn-faq]: vpn-gateway-vpn-faq.md
[vpn-gateway-create-site-to-site-rm-powershell]: vpn-gateway-create-site-to-site-rm-powershell.md
[powershell-install-configure]: powershell-install-configure.md
[xplat-cli]: xplat-cli.md
[virtual-machines-deploy-rmtemplates-azure-cli]: virtual-machines-deploy-rmtemplates-azure-cli.md
[xplat-cli-azure-resource-manager]: xplat-cli-azure-resource-manager.md
[virtual-machines-linux-create-upload-vhd-suse]: virtual-machines-linux-create-upload-vhd-suse.md
[virtual-machines-linux-capture-image-resource-manager]: virtual-machines-linux-capture-image-resource-manager.md
[storage-use-azcopy]: storage-use-azcopy.md
[virtual-machines-linux-capture-image-resource-manager-capture]: virtual-machines-linux-capture-image-resource-manager.md#capture-the-vm
[storage-azure-cli]: storage-azure-cli.md
[storage-powershell-guide-full-copy-vhd]: storage-powershell-guide-full.md#how-to-copy-blobs-from-one-storage-container-to-another
[storage-azure-cli-copy-blobs]: storage-azure-cli.md#copy-blobs
[virtual-machines-linux-agent-user-guide]: virtual-machines-linux-agent-user-guide.md
[virtual-machines-size-specs]: virtual-machines-size-specs.md
[virtual-machines-sql-server-performance-best-practices]: virtual-machines-sql-server-performance-best-practices.md
[virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions]: virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions.md
[virtual-machines-sql-server-alwayson-availability-groups-powershell]: virtual-machines-sql-server-alwayson-availability-groups-powershell.md
[virtual-machines-sql-server-configure-ilb-alwayson-availability-group-listener]: virtual-machines-sql-server-configure-ilb-alwayson-availability-group-listener.md
[virtual-networks-configure-vnet-to-vnet-connection]: virtual-networks-configure-vnet-to-vnet-connection.md
[azure-subscription-service-limits]: azure-subscription-service-limits.md
[virtual-machines-configuring-oracle-data-guard]: virtual-machines-configuring-oracle-data-guard.md
[virtual-machines-linux-configure-raid]: virtual-machines-linux-configure-raid.md
[virtual-machines-attach-disk-preview]: virtual-machines-attach-disk-preview.md
[virtual-machines-workload-template-sql-alwayson]: virtual-machines-workload-template-sql-alwayson.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]: virtual-machines-linux-how-to-attach-disk.md#how-to-initialize-a-new-data-disk-in-linux
[resource-group-authoring-templates]: resource-group-authoring-templates.md
[virtual-machines-linux-update-agent]: virtual-machines-linux-update-agent.md
[virtual-machines-linux-create-upload-vhd-step-1]: virtual-machines-linux-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[deploy-template-powershell]: resource-group-template-deploy.md#deploy-with-powershell
[deploy-template-cli]: resource-group-template-deploy.md#deploy-with-azure-cli-for-mac-linux-and-windows
[deploy-template-portal]: resource-group-template-deploy.md#deploy-with-the-preview-portal
[virtual-networks-udr-overview]: virtual-networks-udr-overview.md
[resource-group-overview]: resource-group-overview.md
[virtual-machines-linux-agent-user-guide-command-line-options]: virtual-machines-linux-agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]: virtual-machines-linux-capture-image-resource-manager.md
[virtual-networks-udr-overview]: virtual-networks-udr-overview.md
[virtual-networks-nsg]: virtual-networks-nsg.md
[storage-premium-storage-preview-portal]: storage-premium-storage-preview-portal.md
[storage-introduction]: storage-introduction.md
[virtual-machines-upload-image-windows-resource-manager]: virtual-machines-upload-image-windows-resource-manager.md
[virtual-machines-azure-resource-manager-architecture]: virtual-machines-azure-resource-manager-architecture.md
[virtual-machines-windows-tutorial]: virtual-machines-windows-tutorial.md
[virtual-networks-create-vnet-arm-pportal]: virtual-networks-create-vnet-arm-pportal.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]: virtual-machines-ps-create-preconfigure-windows-resource-manager-vms.md
[virtual-machines-linux-tutorial]: virtual-machines-linux-tutorial.md

[msdn-set-azurermvmaemextension]: https://msdn.microsoft.com/library/azure/mt670598.aspx

[virtual-machines-azurerm-versus-azuresm]: virtual-machines-azurerm-versus-azuresm.md

[install-extension-cli]: https://github.com/Azure/azure-linux-extensions/blob/master/AzureEnhancedMonitor/README.md

[azure-quickstart-templates-github]: https://github.com/Azure/azure-quickstart-templates
[sap-templates-2-tier-marketplace-image]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-user-image]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[templates-101-simple-windows-vm]: https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-windows-vm
[templates-101-vm-from-user-image]: https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image
[template-201-vm-from-specialized-vhd]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd

[sap-pam]: https://support.sap.com/pam "Matriz de disponibilidad de productos SAP"

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]modelo de implementación clásica.

La informática en la nube es un término ampliamente usado que está cobrando cada vez más importancia en la industria de TI, desde pequeñas empresas hasta las grandes corporaciones y multinacionales. Microsoft Azure es la plataforma de servicios en la nube de Microsoft que ofrece una amplia gama de nuevas posibilidades. Ahora los clientes pueden aprovisionar y desaprovisionar rápidamente aplicaciones como servicios en la nube, por lo que no se verán limitados por restricciones técnicas o presupuestarias. En lugar de invertir tiempo y presupuesto en infraestructura de hardware, las empresas pueden centrarse en la aplicación, en los procesos empresariales y en sus ventajas para clientes y usuarios.

Con los servicios de máquinas virtuales de Microsoft Azure, Microsoft ofrece una completa plataforma de infraestructura como servicio (IaaS). Las aplicaciones de basadas en SAP NetWeaver son compatibles en las máquinas virtuales de Azure (IaaS). Las notas del producto siguientes describen cómo planear e implementar aplicaciones basadas en SAP NetWeaver en Microsoft Azure como plataforma preferida.

[AZURE.INCLUDE [windows-warning](../../includes/virtual-machines-linux-sap-warning.md)]

##  <a name="3da0389e-708b-4e82-b2a2-e92f132df89c"></a>Planeamiento e implementación

Título: SAP NetWeaver en máquinas virtuales (VM) de Linux – Guía de planeación e implementación

Resumen: Este es el documento por donde debe empezar si está pensando en ejecutar SAP NetWeaver en máquinas virtuales de Azure. Esta guía de planeación e implementación le ayudará a evaluar si un sistema basado SAP NetWeaver planeado o existente puede implementarse en un entorno de máquinas virtuales de Azure. Cubre varios escenarios de implementación de SAP NetWeaver e incluye configuraciones SAP específicas para Azure. Este documento enumera y describe toda la información de configuración necesaria que deberá tener al lado de SAP/Azure para ejecutar un entorno SAP híbrido. También cubre las medidas que se pueden tomar para garantizar una alta disponibilidad de los sistemas basados en SAP NetWeaver en IaaS.

Última actualización: marzo de 2016

[Esta guía se puede encontrar aquí][planning-guide]
## <a name="6aadadd2-76b5-46d8-8713-e8d63630e955"></a>Implementación

Título: SAP NetWeaver en máquinas virtuales (VM) de Linux – Guía de implementación

Resumen: Este documento proporciona una guía de los procedimientos necesarios para implementar el software SAP NetWeaver en máquinas virtuales de Azure Este documento se centra en tres escenarios de implementación específicos, con énfasis en la habilitación de las Extensiones de supervisión de Azure para SAP, incluidas las recomendaciones para la solución de problemas de las Extensiones de supervisión de Azure para SAP. Este documento supone que ha leído la guía de planeación e implementación.

Última actualización: marzo de 2016

[Esta guía se puede encontrar aquí][deployment-guide]

## <a name="1343ffe1-8021-4ce6-a08d-3a1553a4db82"></a>Guía de implementación de DBMS

Título: SAP NetWeaver en máquinas virtuales (VM) de Linux – Guía de implementación de DBMS

Resumen: Este documento cubre las consideraciones de planeación e implementación para los sistemas DBMS que deben ejecutarse con SAP. En la primera parte se presentan y se enumeran consideraciones generales. Las siguientes partes del documento están relacionadas con las implementaciones de diferentes DBMS en Azure que admite SAP. Los distintos DBMS que se presentan son SQL Server, SAP ASE y Oracle. En estas partes concretas, se tratan las consideraciones que debe tener en cuenta al ejecutar sistemas SAP en Azure junto con los DBMS. Se incluyen temas como los métodos de copia de seguridad y de alta disponibilidad que admiten los distintos DBMS en Azure para el uso con aplicaciones SAP.

Última actualización: marzo de 2016

[Esta guía se puede encontrar aquí][dbms-guide]

<!---HONumber=AcomDC_0518_2016-->