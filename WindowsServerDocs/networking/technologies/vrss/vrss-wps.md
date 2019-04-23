---
title: Commandes PowerShell de Windows pour les flux RSS et vRSS
description: Dans cette rubrique, vous allez apprendre à localiser rapidement les informations de référence technique sur les commandes Windows PowerShell pour la mise à l’échelle côté réception (RSS) et vRSS (RSS virtuel).
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 49e93b9f-46d9-4cee-bcda-1c4634893ddd
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/05/2018
ms.openlocfilehash: 10039388009e32c10d71067b835bad65db5607ef
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833260"
---
# <a name="windows-powershell-commands-for-rss-and-vrss"></a>Commandes PowerShell de Windows pour les flux RSS et vRSS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans cette rubrique, vous allez apprendre à localiser rapidement les informations de référence technique sur les commandes Windows PowerShell pour l’échelle côté réception \(RSS\) et RSS virtuel \(vRSS\).

Utilisez les commandes RSS suivantes pour configurer RSS sur un ordinateur physique disposant de plusieurs processeurs. Vous pouvez utiliser les mêmes commandes pour configurer vRSS sur une machine virtuelle \(machine virtuelle\) qui exécute un système d’exploitation pris en charge. Pour plus d’informations, consultez [les applets de commande de carte réseau dans Windows PowerShell](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps).

## <a name="configure-vmq"></a>Configurer des ordinateurs virtuels

vRSS nécessite que les ordinateurs virtuels est activé et configuré. Vous pouvez utiliser les commandes Windows PowerShell suivantes pour gérer les paramètres de la file.

- [Disable-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/disable-netadaptervmq?view=win10-ps)
- [Enable-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/enable-netadaptervmq?view=win10-ps)
- [Get-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/get-netadaptervmq?view=win10-ps)
- [Set-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/set-netadaptervmq?view=win10-ps)

## <a name="enable-and-configure-rss-on-a-native-host"></a>Activer et configurer RSS sur un hôte natif

Utilisez les commandes PowerShell suivantes pour configurer RSS sur un hôte natif aussi gérer RSS dans une machine virtuelle ou sur un ordinateur hôte carte réseau virtuelle (vNIC). Certains des paramètres de ces commandes peuvent aussi affecter la file d’attente de la Machine virtuelle \(VMQ\) dans l’hôte Hyper-V.  

>[!IMPORTANT]
>L’activation de RSS dans une machine virtuelle ou sur une carte réseau virtuelle hôte est un composant requis pour l’activation et à l’aide de vRSS.

- [Disable-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/disable-netadapterrss?view=win10-ps)
- [Enable-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/enable-netadapterrss?view=win10-ps)
- [Get-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/get-netadapterrss?view=win10-ps)
- [Set-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/Set-NetAdapterRss?view=win10-ps)

## <a name="enable-vrss-on-the-hyper-v-virtual-switch-port"></a>Activez vRSS sur Hyper\-port de commutateur virtuel V

Outre l’activation de RSS dans la machine virtuelle, vRSS nécessite que vous activez vRSS sur Hyper\-port de commutateur virtuel V. 

Déterminer les paramètres présents pour vRSS et activer ou désactiver la fonctionnalité pour une machine virtuelle.

   **Afficher les paramètres actuels :** 

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | fl
   ```

   **Activé la fonctionnalité :**
   
   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled [$True|$False]
   ```

## <a name="enable-or-disable-vrss-on-a-host-vnic"></a>Activer ou désactiver vRSS sur une carte réseau virtuelle hôte

Déterminer les paramètres présents pour vRSS et activer ou désactiver la fonctionnalité pour une carte réseau virtuelle hôte.

   **Afficher les paramètres actuels :** 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | fl
   ```

   **Activer ou désactiver la fonctionnalité :** 

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled [$True|$False]
   ```

## <a name="configure-the-scheduling-mode-on-the-hyper-v-virtual-switch-port"></a>Configurer le mode de planification sur le port de commutateur virtuel Hyper-V 
>S’applique à : Windows Server 2019

Dans Windows Server 2019, vRSS peut mettre à jour les processeurs logiques permettent de traiter le trafic réseau dynamiquement.  Appareils avec les pilotes pris en charge ont ce mode de planification est activé par défaut. 

Déterminer le mode de planification présents sur un système ou modifier le mode de planification pour une machine virtuelle.

   **Afficher les paramètres actuels :** 

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | Select 'VRSSQueue'
   ```

   **Définir ou modifier le mode de planification :**

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```

## <a name="configure-the-scheduling-mode-on-a-host-vnic"></a>Configurer le mode de planification sur une carte réseau virtuelle hôte
>S’applique à : Windows Server 2019

Pour déterminer le mode de planification présent ou pour modifier le mode de planification pour une carte réseau virtuelle hôte, utilisez les commandes Windows PowerShell suivantes :

   **Afficher les paramètres actuels :** 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | Select 'VRSSQueue'
   ```

   **Définir ou modifier le mode de planification :** 

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssQueueSchedulingMode -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```


## <a name="related-topics"></a>Rubriques connexes 
Pour plus d’informations, consultez les rubriques de référence suivantes.

- [Get-VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmnetworkadapter)
- [Set-VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/set-vmnetworkadapter)

Pour plus d’informations, consultez [virtuel l’échelle côté réception (vRSS)](vrss-top.md).