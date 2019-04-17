---
title: Gérer des machines virtuelles de Azure IaaS
description: La gestion des machines virtuelles Azure IaaS avec Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/07/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 6f162294076d7e12df09f31b0bbd7d9244abe679
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296930"
---
# Gérer des machines virtuelles de Azure IaaS avec Windows Admin Center

Vous pouvez utiliser Windows Admin Center pour gérer vos machines virtuelles Azure, ainsi que les ordinateurs locaux. Il existe différentes configurations possibles: choisissez la configuration appropriée pour votre environnement:
- [Gérer les machines virtuelles Azure à partir d’une passerelle de Windows Admin Center sur site](#manage-with-an-on-premises-windows-admin-center-gateway)
- [Gérer les machines virtuelles Azure à partir d’une passerelle Windows Admin Center installée sur une machine virtuelle Azure](#use-a-windows-admin-center-gateway-deployed-in-azure)

## Gérer avec une passerelle de Windows Admin Center sur site

Si vous avez déjà installé Windows Admin Center sur une passerelle sur site (soit sur Windows 10 ou Windows Server 2016), vous pouvez utiliser cette même passerelle pour gérer Windows 10 ou Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2 Ordinateurs virtuels dans Azure. 

### Connexion aux machines virtuelles avec une adresse IP publique

Si votre cible de machines virtuelles (les ordinateurs virtuels à gérer avec Windows Admin Center) possède des adresses IP publique, ajoutez-les à votre passerelle Windows Admin Center par adresse IP ou par nom de domaine complet. Il existe deux éléments à prendre en compte:

- Vous devez activer WinRM accès à votre ordinateur virtuel cible en exécutant la commande suivante dans PowerShell ou l’invite de commandes sur la cible de machine virtuelle: `winrm quickconfig`
- Si vous n’avez pas encore joint au domaine la machine virtuelle Azure, l’ordinateur virtuel se comporte comme un serveur de groupe de travail, vous devez donc pour vous assurer que vous prendre en compte pour [l’utilisation de Windows Admin Center dans un groupe de travail](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup).
- Vous devez également activer les connexions entrantes sur le port 5985 pour WinRM sur HTTP afin que Windows Admin Center gérer la cible de machine virtuelle:
   1. Exécutez le script PowerShell suivant sur la cible ordinateur virtuel pour activer les connexions entrantes sur le port 5985 sur le système d’exploitation invité:   
`Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any`

   2. Vous devez également ouvrir le port de mise en réseau Azure:

    - Sélectionnez votre machine virtuelle Azure, sélectionnez la **mise en réseau**, puis **Ajouter une règle port d’entrée**. 
    - Assurez-vous de **que base** est sélectionné en haut du volet **Ajouter une règle sécurité entrante** .
    - Dans le champ de **plages de ports** , entrez **5985**.
    
    Si votre passerelle Windows Admin Center a une adresse IP statique, vous pouvez sélectionner pour autoriser uniquement WinRM l’accès entrant à partir de votre passerelle Windows Admin Center pour une sécurité accrue.
    Pour ce faire, sélectionnez **Avancé** en haut du volet **Ajouter une règle sécurité entrante** .

    Pour la **Source**, sélectionnez **Les adresses IP**, puis entrez l’adresse IP Source correspondant à votre passerelle Windows Admin Center.

    - Pour le **protocole** , sélectionnez **TCP**.
    - Le reste peut être laissé par défaut.

> [!NOTE]
> Vous devez créer une règle de port personnalisé. La règle de port WinRM fournie par la mise en réseau Azure utilise le port 5986 (via HTTPS) au lieu de 5985 (sur HTTP). 

### Connexion aux ordinateurs virtuels sans une adresse IP publique

Si votre cible de machines virtuelles Azure n’avez adresses IP publique, et que vous souhaitez gérer ces machines virtuelles à partir d’une passerelle Windows Admin Center déployée dans votre réseau local, vous devez configurer votre réseau local pour disposer d’une connectivité à la basculée sur lequel la cible de machines virtuelles sont connecté. Il existe 3 moyens, vous pouvez le faire: ExpressRoute, VPN Site à Site ou Point-to-Site VPN. [Découvrez l’option de connectivité logique dans votre environnement.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design) 

>[!TIP]
>Si vous souhaitez utiliser un VPN de Point-à-Site pour connecter votre passerelle Windows Admin Center à une basculée Azure pour gérer les machines virtuelles Azure dans ce basculée, vous pouvez utiliser la fonctionnalité de [L’adaptateur réseau Azure](https://aka.ms/WACNetworkAdapter) dans Windows Admin Center. Pour ce faire, connectez-vous au serveur sur lequel Windows Admin Center est installé, accédez à l’outil réseau et sélectionnez «Ajouter une carte réseau Azure». Lorsque vous fournissez les informations nécessaires, puis cliquez sur «Configurer», Windows Admin Center allez configurer un réseau privé virtuel de Point-à-Site pour le basculée Azure que vous spécifiez, après quoi, vous pouvez vous connecter à et gérer des machines virtuelles Azure à partir de votre passerelle Windows Admin Center de locaux.

Vérifiez que WinRM s’exécute sur vos ordinateurs virtuels cible en exécutant la commande suivante dans PowerShell ou l’invite de commandes sur la cible de machine virtuelle: `winrm quickconfig`

Si vous n’avez pas encore joint au domaine la machine virtuelle Azure, l’ordinateur virtuel se comporte comme un serveur de groupe de travail, vous devez donc pour vous assurer que vous prendre en compte pour [l’utilisation de Windows Admin Center dans un groupe de travail](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup).

Si vous rencontrez des problèmes, consultez la [Résolution des problèmes Windows Admin Center](../support/troubleshooting.md) pour voir si des étapes supplémentaires sont nécessaires pour la configuration (par exemple, si vous êtes connecté en utilisant un compte administrateur local ou ne sont pas joints au domaine).

## Utiliser une passerelle Windows Admin Center déployée dans Azure

Vous pouvez gérer des machines virtuelles Azure sans une dépendance sur site en déployant Windows Admin Center dans la basculée où vos machines virtuelles cible sont connectés. 

Pour gérer les machines virtuelles en dehors de la basculée sur lequel la passerelle Windows Admin Center est déployée, vous devez établir une connectivité basculée-à-basculée entre la basculée de la passerelle Windows Admin Center et la basculée des serveurs cible. Vous pouvez établir cette connectivité avec l’homologation basculée, connexion basculée-à-basculée ou une connexion de Site à Site. [En savoir plus sur la connectivité qui basculée-à-basculée option logique dans votre environnement.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal)

Windows Admin Center peut être installé sur une machine virtuelle au existante ou qui vient d’être déployée dans votre environnement. L’ordinateur virtuel que vous choisissez pour l’installation de Windows Admin Center doit avoir un nom d’adresse IP et DNS public.

[En savoir plus sur le déploiement d’une passerelle Windows Admin Center dans Azure](deploy-wac-in-azure.md)