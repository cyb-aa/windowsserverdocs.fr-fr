---
title: Gérer des machines virtuelles Azure IaaS
description: Gestion des machines virtuelles IaaS Azure à l’aide du centre d’administration Windows (Project Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/07/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 7b85f64d108283d4865b718b565ad3ba40f14f02
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357342"
---
# <a name="manage-azure-iaas-virtual-machines-with-windows-admin-center"></a>Gérer des machines virtuelles Azure IaaS avec le centre d’administration Windows

Vous pouvez utiliser Windows Admin Center pour gérer vos machines virtuelles Azure ainsi que vos ordinateurs locaux. Plusieurs configurations sont possibles : choisissez la configuration la plus appropriée pour votre environnement :
- [Gérer des machines virtuelles Azure à partir d’une passerelle du centre d’administration Windows locale](#manage-with-an-on-premises-windows-admin-center-gateway)
- [Gérer des machines virtuelles Azure à partir d’une passerelle du centre d’administration Windows installée sur une machine virtuelle Azure](#use-a-windows-admin-center-gateway-deployed-in-azure)

## <a name="manage-with-an-on-premises-windows-admin-center-gateway"></a>Gérer avec une passerelle du centre d’administration Windows locale

Si vous avez déjà installé le centre d’administration Windows sur une passerelle locale (sur Windows 10 ou Windows Server 2016), vous pouvez utiliser cette même passerelle pour gérer Windows 10 ou Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2 Machines virtuelles dans Azure. 

### <a name="connecting-to-vms-with-a-public-ip"></a>Connexion à des machines virtuelles avec une adresse IP publique

Si vos machines virtuelles cibles (les machines virtuelles que vous souhaitez gérer avec le centre d’administration Windows) ont des adresses IP publiques, ajoutez-les à votre passerelle du centre d’administration Windows par adresse IP ou par nom de domaine complet. Il existe quelques considérations à prendre en compte :

- Vous devez activer l’accès WinRM à votre machine virtuelle cible en exécutant la commande suivante dans PowerShell ou l’invite de commandes sur la machine virtuelle cible : `winrm quickconfig`
- Si vous n’avez pas joint le domaine à la machine virtuelle Azure, celle-ci se comporte comme un serveur dans le groupe de travail. vous devez donc vous assurer de prendre en compte [l’utilisation du centre d’administration Windows dans un groupe de travail](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup).
- Vous devez également activer les connexions entrantes sur le port 5985 pour WinRM sur HTTP afin que le centre d’administration Windows gère la machine virtuelle cible :
  1. Exécutez le script PowerShell suivant sur la machine virtuelle cible pour activer les connexions entrantes vers le port 5985 sur le système d’exploitation invité :   
     `Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any`

  2. Vous devez également ouvrir le port dans la mise en réseau Azure :

     - Sélectionnez votre machine virtuelle Azure, sélectionnez **mise en réseau**, puis ajouter une règle de **port entrant**. 
     - Vérifiez que l’option de **base** est sélectionnée en haut du volet **Ajouter une règle de sécurité de trafic entrant** .
     - Dans le champ **plages de ports** , entrez **5985**.
    
     Si votre passerelle du centre d’administration Windows possède une adresse IP statique, vous pouvez choisir d’autoriser uniquement l’accès au WinRM entrant à partir de votre passerelle du centre d’administration Windows pour renforcer la sécurité.
     Pour ce faire, sélectionnez **avancé** en haut du volet **Ajouter une règle de sécurité de trafic entrant** .

     Pour **source**, sélectionnez **adresses IP**, puis entrez l’adresse IP source correspondant à votre passerelle du centre d’administration Windows.

     - Pour **protocole** , sélectionnez **TCP**.
     - Le reste peut être laissé par défaut.

> [!NOTE]
> Vous devez créer une règle de port personnalisée. La règle de port WinRM fournie par la mise en réseau Azure utilise le port 5986 (via HTTPs) au lieu de 5985 (via HTTP). 

### <a name="connecting-to-vms-without-a-public-ip"></a>Connexion à des machines virtuelles sans adresse IP publique

Si vos machines virtuelles Azure cibles n’ont pas d’adresses IP publiques et que vous souhaitez gérer ces machines virtuelles à partir d’une passerelle du centre d’administration Windows déployée sur votre réseau local, vous devez configurer votre réseau local pour qu’il dispose d’une connectivité au réseau virtuel sur lequel se trouvent les machines virtuelles cibles. correctement. Vous pouvez procéder de trois façons : ExpressRoute, VPN de site à site ou VPN de point à site. [Découvrez l’option de connectivité la plus appropriée pour votre environnement.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design) 

>[!TIP]
>Si vous souhaitez utiliser un VPN de point à site pour connecter votre passerelle du centre d’administration Windows à un réseau virtuel Azure afin de gérer des machines virtuelles Azure dans ce réseau virtuel, vous pouvez utiliser la fonctionnalité [carte réseau Azure](https://aka.ms/WACNetworkAdapter) dans le centre d’administration Windows. Pour ce faire, connectez-vous au serveur sur lequel le centre d’administration Windows est installé, accédez à l’outil réseau et sélectionnez « Ajouter une carte réseau Azure ». Lorsque vous fournissez les informations nécessaires et cliquez sur « configurer », le centre d’administration Windows configure un VPN de point à site sur le réseau virtuel Azure que vous spécifiez, après quoi, vous pouvez vous connecter et gérer des machines virtuelles Azure à partir de votre passerelle du centre d’administration Windows locale.

Vérifiez que WinRM est en cours d’exécution sur vos machines virtuelles cibles en exécutant la commande suivante dans PowerShell ou l’invite de commandes sur la machine virtuelle cible : `winrm quickconfig`

Si vous n’avez pas joint le domaine à la machine virtuelle Azure, celle-ci se comporte comme un serveur dans le groupe de travail. vous devez donc vous assurer de prendre en compte [l’utilisation du centre d’administration Windows dans un groupe de travail](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup).

Si vous rencontrez des problèmes, consultez [résoudre les problèmes liés au centre d’administration Windows](../support/troubleshooting.md) pour déterminer si des étapes supplémentaires sont requises pour la configuration (par exemple, si vous vous connectez à l’aide d’un compte d’administrateur local ou si vous n’êtes pas joint au domaine).

## <a name="use-a-windows-admin-center-gateway-deployed-in-azure"></a>Utiliser une passerelle du centre d’administration Windows déployée dans Azure

Vous pouvez gérer des machines virtuelles Azure sans aucune dépendance locale en déployant le centre d’administration Windows dans le réseau virtuel où vos machines virtuelles cibles sont connectées. 

Pour gérer des machines virtuelles en dehors du réseau virtuel sur lequel est déployée la passerelle du centre d’administration Windows, vous devez établir une connectivité de réseau virtuel à réseau virtuel entre le réseau virtuel de la passerelle du centre d’administration Windows et le réseau virtuel des serveurs cibles. Vous pouvez établir cette connectivité avec une homologation de réseaux virtuels, une connexion de réseau virtuel à réseau virtuel ou une connexion de site à site. [En savoir plus sur l’option de connectivité de réseau virtuel à réseau virtuel dans votre environnement.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal)

Le centre d’administration Windows peut être installé sur une machine virtuelle existante ou nouvellement déployée dans votre environnement. La machine virtuelle que vous choisissez pour l’installation du centre d’administration Windows doit avoir une adresse IP publique et un nom DNS.

[En savoir plus sur le déploiement d’une passerelle du centre d’administration Windows dans Azure](deploy-wac-in-azure.md)