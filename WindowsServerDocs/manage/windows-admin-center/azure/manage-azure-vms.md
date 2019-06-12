---
title: Gérer des machines virtuelles Azure IaaS
description: La gestion des machines virtuelles IaaS Azure avec Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/07/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ac98f42c4ad5606cc8d2b142f209f9bdb2b9611c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445900"
---
# <a name="manage-azure-iaas-virtual-machines-with-windows-admin-center"></a>Gérer des machines virtuelles de Azure IaaS avec Windows Admin Center

Vous pouvez utiliser Windows Admin Center pour gérer vos machines virtuelles Azure, ainsi que les machines locales. Il existe plusieurs configurations possibles : choisissez la configuration qui a du sens pour votre environnement :
- [Gérer des machines virtuelles Azure à partir d’une passerelle de Windows Admin Center locale](#manage-with-an-on-premises-windows-admin-center-gateway)
- [Gérer des machines virtuelles Azure à partir d’une passerelle Windows Admin Center installée sur une machine virtuelle Azure](#use-a-windows-admin-center-gateway-deployed-in-azure)

## <a name="manage-with-an-on-premises-windows-admin-center-gateway"></a>Gérer avec une passerelle de Windows Admin Center local

Si vous avez déjà installé Windows Admin Center sur une passerelle locale (soit sur Windows 10 ou Windows Server 2016), vous pouvez utiliser cette même passerelle pour gérer Windows 10 ou Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2 Machines virtuelles dans Azure. 

### <a name="connecting-to-vms-with-a-public-ip"></a>Connexion aux machines virtuelles avec une adresse IP publique

Si votre cible de machines virtuelles (les machines virtuelles vous voulez gérer avec Windows Admin Center) ont des adresses IP publiques, ajoutez-les à votre passerelle Windows Admin Center par adresse IP ou nom de domaine complet. Il existe quelques considérations à prendre en compte :

- Vous devez activer l’accès WinRM pour votre machine virtuelle cible en exécutant la commande suivante dans PowerShell ou l’invite de commandes sur la machine virtuelle cible : `winrm quickconfig`
- Si vous n’avez pas joints au domaine la machine virtuelle Azure, la machine virtuelle se comporte comme un serveur dans le groupe de travail, vous devez vous assurer de tenir compte de la [à l’aide de Windows Admin Center dans un groupe de travail](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup).
- Vous devez également activer les connexions entrantes au port 5985 pour WinRM sur HTTP afin que Windows Admin Center gérer la machine virtuelle cible :
  1. Exécutez le script PowerShell suivant sur la machine virtuelle pour activer les connexions entrantes au port 5985 sur le système d’exploitation invité de la cible :   
     `Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any`

  2. Vous devez également ouvrir le port sur le réseau Azure :

     - Sélectionnez votre machine virtuelle Azure, sélectionnez **mise en réseau**, puis **ajouter une règle de port entrant**. 
     - Vérifiez **base** est sélectionné en haut de la **ajouter une règle de sécurité de trafic entrant** volet.
     - Dans le **plages de Port** , entrez **5985**.
    
     Si votre passerelle Windows Admin Center dispose d’une adresse IP statique, vous pouvez sélectionner pour autoriser uniquement WinRM accès entrant à partir de votre passerelle Windows Admin Center pour renforcer la sécurité.
     Pour ce faire, sélectionnez **avancé** en haut de la **ajouter une règle de sécurité de trafic entrant** volet.

     Pour **Source**, sélectionnez **adresses IP**, puis entrez l’adresse IP de Source correspondant à votre passerelle Windows Admin Center.

     - Pour **protocole** sélectionnez **TCP**.
     - Le reste peut être laissé comme valeur par défaut.

> [!NOTE]
> Vous devez créer une règle de port personnalisé. La règle de port WinRM fournie par la mise en réseau Azure utilise le port 5986 (HTTPS) au lieu de 5985 (HTTP). 

### <a name="connecting-to-vms-without-a-public-ip"></a>Connexion aux machines virtuelles sans une adresse IP publique

Si vos machines virtuelles Azure cibles n’ont des adresses IP publiques, et que vous souhaitez gérer ces machines virtuelles à partir d’une passerelle Windows Admin Center déployée dans votre réseau local, vous devez configurer votre réseau local pour disposer d’une connectivité au réseau virtuel sur lequel sont la cible de machines virtuelles connecté. Il existe 3 façons vous pouvez le faire : ExpressRoute, VPN de Site à Site ou Point-to-Site VPN. [Découvrez quelle option de connectivité est pertinent dans votre environnement.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design) 

>[!TIP]
>Si vous souhaitez utiliser un VPN de Point à Site pour connecter votre passerelle de Windows Admin Center à un réseau virtuel Azure pour gérer des machines virtuelles Azure dans ce réseau virtuel, vous pouvez utiliser la [carte de réseau Azure](https://aka.ms/WACNetworkAdapter) fonctionnalité dans Windows Admin Center. Pour ce faire, connectez-vous au serveur sur lequel est installé Windows Admin Center, accédez à l’outil réseau et sélectionnez « Ajouter une carte réseau Azure ». Lorsque vous fournissez les informations nécessaires et cliquez sur « Configurer », Windows Admin Center configurera un VPN de Point-to-Site au réseau virtuel Azure que vous spécifiez, après quoi, vous pouvez vous connecter à et gérer des machines virtuelles Azure à partir de votre passerelle de Windows Admin Center locale.

Vérifiez que WinRM est en cours d’exécution sur vos machines virtuelles cibles en exécutant la commande suivante dans PowerShell ou l’invite de commandes sur la machine virtuelle cible : `winrm quickconfig`

Si vous n’avez pas joints au domaine la machine virtuelle Azure, la machine virtuelle se comporte comme un serveur dans le groupe de travail, vous devez vous assurer de tenir compte de la [à l’aide de Windows Admin Center dans un groupe de travail](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup).

Si vous rencontrez des problèmes, consultez [de résolution des problèmes Windows Admin Center](../support/troubleshooting.md) pour voir si des étapes supplémentaires sont requises pour la configuration (par exemple, si vous vous connectez à l’aide d’un compte d’administrateur local ou ne sont pas joints au domaine).

## <a name="use-a-windows-admin-center-gateway-deployed-in-azure"></a>Utiliser une passerelle Windows Admin Center déployée dans Azure

Vous pouvez gérer des machines virtuelles Azure sans aucune dépendance sur site en déployant Windows Admin Center dans le réseau virtuel où vos machines virtuelles cibles sont connectés. 

Pour gérer des machines virtuelles en dehors du réseau virtuel sur lequel la passerelle Windows Admin Center est déployée, vous devez établir la connectivité de réseau virtuel à réseau virtuel entre le réseau virtuel de la passerelle Windows Admin Center et le réseau virtuel des serveurs cibles. Vous pouvez établir cette connectivité avec l’homologation, connexion de réseau virtuel à réseau virtuel ou d’une connexion de Site à Site. [En savoir plus sur lequel la connectivité réseau virtuel à réseau virtuel option est pertinent dans votre environnement.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal)

Windows Admin Center peut être installé sur une machine virtuelle au existante ou nouvellement déployée dans votre environnement. La machine virtuelle que vous choisissez pour l’installation de Windows Admin Center doit avoir un nom DNS et IP public.

[En savoir plus sur le déploiement d’une passerelle Windows Admin Center dans Azure](deploy-wac-in-azure.md)