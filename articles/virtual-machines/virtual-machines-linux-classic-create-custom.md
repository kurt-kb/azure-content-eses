<properties
	pageTitle="Creación de una máquina virtual Linux | Microsoft Azure"
	description="Obtenga información acerca de cómo crear una máquina virtual personalizada con el modelo de implementación clásica ejecutando el sistema operativo Linux."
	services="virtual-machines-linux"
	documentationCenter=""
	authors="iainfoulds"
	manager="timlt"
	editor="tysonn"
	tags="azure-service-management"/>

<tags
	ms.service="virtual-machines-linux"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/07/2016"
	ms.author="iainfou"/>

# Creación de una máquina virtual con Linux personalizada

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

En este tema se describe cómo crear una máquina virtual(VM) personalizada con la CLI de Azure mediante el modelo de implementación clásica. Usaremos una imagen de Linux de una de las **IMÁGENES** disponibles en Azure. Los comandos de la CLI de Azure ofrecen, entre otras, las siguientes opciones de configuración:

- Conexión de la VM a una red virtual
- Adición de la VM a un servicio en la nube existente
- Incorporación de la máquina virtual a una cuenta de almacenamiento existente
- Incorporación de la máquina virtual a un conjunto de disponibilidad o una ubicación

> [AZURE.IMPORTANT] Si desea que la máquina virtual use una red virtual, con el fin de poder conectarse a ella directamente mediante un nombre de host o configurar conexiones entre locales, asegúrese de que especifica la red virtual al crear la máquina virtual. Puede configurarse una máquina virtual para que se una a una red virtual solo después de crear la máquina virtual. Para obtener detalles acerca de las redes virtuales, consulte [Información general sobre redes virtuales de Azure](http://go.microsoft.com/fwlink/p/?LinkID=294063).


## Creación de una máquina virtual con Linux mediante el modelo de implementación clásica

[AZURE.INCLUDE [virtual-machines-create-LinuxVM](../../includes/virtual-machines-create-linuxvm.md)]

<!---HONumber=AcomDC_0629_2016-->