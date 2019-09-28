---
title: Commandes Windows PowerShell pour RSS et vRSS
description: Dans cette rubrique, vous allez apprendre à trouver rapidement des informations de référence technique sur les commandes Windows PowerShell pour la mise à l’échelle côté réception (RSS) et la fonction RSS virtuelle (vRSS).
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 49e93b9f-46d9-4cee-bcda-1c4634893ddd
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/05/2018
ms.openlocfilehash: bb915f72e53d28c73a9c2e405b3b0edd656953db
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405272"
---
# <a name="windows-powershell-commands-for-rss-and-vrss"></a>Commandes Windows PowerShell pour RSS et vRSS

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Dans cette rubrique, vous allez apprendre à trouver rapidement des informations de référence technique sur les commandes Windows PowerShell pour la mise à l’échelle côté réception \(RSS @ no__t-1 et Virtual RSS \(vRSS @ no__t-3.

Utilisez les commandes RSS suivantes pour configurer RSS sur un ordinateur physique équipé de plusieurs processeurs ou de plusieurs cœurs. Vous pouvez utiliser les mêmes commandes pour configurer vRSS sur une machine virtuelle \(VM @ no__t-1 qui exécute un système d’exploitation pris en charge. Pour plus d’informations, consultez [applets de commande de cartes réseau dans Windows PowerShell](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps).

## <a name="configure-vmq"></a>Configurer l’ordinateurs virtuels

vRSS requiert l’activation et la configuration de la gestion des ordinateurs virtuels. Vous pouvez utiliser les commandes Windows PowerShell suivantes pour gérer les paramètres de l’ordinateurs virtuels.

- [Disable-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/disable-netadaptervmq?view=win10-ps)
- [Enable-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/enable-netadaptervmq?view=win10-ps)
- [NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/get-netadaptervmq?view=win10-ps)
- [Set-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/set-netadaptervmq?view=win10-ps)

## <a name="enable-and-configure-rss-on-a-native-host"></a>Activer et configurer RSS sur un hôte natif

Utilisez les commandes PowerShell suivantes pour configurer RSS sur un hôte natif, ainsi que pour gérer RSS sur une machine virtuelle ou sur une carte réseau virtuelle hôte (carte réseau virtuelle). Certains des paramètres de ces commandes peuvent également affecter File d’attente d’ordinateurs virtuels \(VMQ @ no__t-1 dans l’hôte Hyper-V.  

>[!IMPORTANT]
>L’activation de RSS sur une machine virtuelle ou sur un ordinateur hôte carte réseau virtuelle est une condition préalable à l’activation et à l’utilisation de vRSS.

- [Disable-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/disable-netadapterrss?view=win10-ps)
- [Enable-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/enable-netadapterrss?view=win10-ps)
- [NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/get-netadapterrss?view=win10-ps)
- [Set-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/Set-NetAdapterRss?view=win10-ps)

## <a name="enable-vrss-on-the-hyper-v-virtual-switch-port"></a>Activer vRSS sur le port de commutateur virtuel Hyper @ no__t-0V

En plus de l’activation de RSS dans la machine virtuelle, vRSS requiert l’activation de vRSS sur le port de commutateur virtuel Hyper @ no__t-0V. 

Déterminez les paramètres présents pour vRSS et activez ou désactivez la fonctionnalité pour une machine virtuelle.

   **Affichez les paramètres actuels :** 

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | fl
   ```

   **Activation de la fonctionnalité :**
   
   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled [$True|$False]
   ```

## <a name="enable-or-disable-vrss-on-a-host-vnic"></a>Activer ou désactiver vRSS sur un ordinateur hôte carte réseau virtuelle

Déterminez les paramètres présents pour vRSS, puis activez ou désactivez la fonctionnalité pour un ordinateur hôte carte réseau virtuelle.

   **Affichez les paramètres actuels :** 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | fl
   ```

   **Activez ou désactivez la fonctionnalité :** 

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled [$True|$False]
   ```

## <a name="configure-the-scheduling-mode-on-the-hyper-v-virtual-switch-port"></a>Configurer le mode de planification sur le port de commutateur virtuel Hyper-V 
>S’applique à : Windows Server 2019

Dans Windows Server 2019, vRSS peut mettre à jour les processeurs logiques utilisés pour traiter le trafic réseau dynamiquement.  Ce mode de planification est activé par défaut pour les appareils avec des pilotes pris en charge. 

Déterminez le mode de planification présent sur un système ou modifiez le mode de planification d’une machine virtuelle.

   **Affichez les paramètres actuels :** 

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | Select 'VRSSQueue'
   ```

   **Définir ou modifier le mode de planification :**

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```

## <a name="configure-the-scheduling-mode-on-a-host-vnic"></a>Configurer le mode de planification sur un ordinateur hôte carte réseau virtuelle
>S’applique à : Windows Server 2019

Pour déterminer le mode de planification actuel ou pour modifier le mode de planification d’un carte réseau virtuelle d’ordinateur hôte, utilisez les commandes Windows PowerShell suivantes :

   **Affichez les paramètres actuels :** 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | Select 'VRSSQueue'
   ```

   **Définir ou modifier le mode de planification :** 

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssQueueSchedulingMode -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```


## <a name="related-topics"></a>Rubriques connexes 
Pour plus d’informations, consultez les rubriques de référence suivantes.

- [Récupération-VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmnetworkadapter)
- [Set-VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/set-vmnetworkadapter)

Pour plus d’informations, voir la rubrique relative à [la mise à l’échelle côté réception virtuelle (vRSS)](vrss-top.md).