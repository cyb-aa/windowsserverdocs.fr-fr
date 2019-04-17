---
title: Commandes PowerShell de Windows pour les flux RSS et vRSS
description: Dans cette rubrique, vous allez apprendre à trouver rapidement les informations techniques de référence sur les commandes Windows PowerShell pour la mise à l’échelle côté réception (RSS) et RSS virtuel (vRSS).
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
ms.sourcegitcommit: 9ed4c9fe04ebf3ef488170503c9a354c992b6fde
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/03/2018
ms.locfileid: "4339267"
---
# Commandes PowerShell de Windows pour les flux RSS et vRSS

>S’applique à: Windows Server (canal semi-annuel), WindowsServer2016

Dans cette rubrique, vous allez apprendre à trouver rapidement les informations techniques de référence sur les commandes Windows PowerShell pour l’évolutivité côté réception \(RSS\) et virtuel \(vRSS\) RSS.

Utilisez les commandes RSS suivantes pour configurer RSS sur un ordinateur physique avec plusieurs processeurs ou plusieurs cœurs. Vous pouvez utiliser les mêmes commandes pour configurer vRSS sur une machine virtuelle \(VM\) qui exécute un système d’exploitation pris en charge. Pour plus d’informations, voir [Les applets de commande de carte réseau dans Windows PowerShell](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps).

## Configurer VMQ

vRSS nécessite que VMQ est activé et configuré. Vous pouvez utiliser les commandes Windows PowerShell suivantes pour gérer les paramètres VMQ.

- [Disable-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/disable-netadaptervmq?view=win10-ps)
- [Enable-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/enable-netadaptervmq?view=win10-ps)
- [Get-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/get-netadaptervmq?view=win10-ps)
- [Set-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/set-netadaptervmq?view=win10-ps)

## Activer et configurer RSS sur un hôte natif

Utilisez les commandes PowerShell suivantes pour configurer RSS sur un hôte natif ainsi que de gérer RSS dans une machine virtuelle ou sur un hôte de carte réseau virtuelle (vNIC). Certains des paramètres de ces commandes peuvent également affecter \(VMQ\) file d’attente de la Machine virtuelle dans l’hôte Hyper-V.  

>[!IMPORTANT]
>L’activation de RSS dans une machine virtuelle ou sur une carte réseau virtuelle hôte est un composant requis pour l’activation et à l’aide de vRSS.

- [Disable-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/disable-netadapterrss?view=win10-ps)
- [Enable-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/enable-netadapterrss?view=win10-ps)
- [Get-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/get-netadapterrss?view=win10-ps)
- [Set-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/Set-NetAdapterRss?view=win10-ps)

## Activer vRSS sur le port de commutateur virtuel Hyper\-V

Outre l’activation de RSS dans la machine virtuelle, vRSS requiert que vous activez vRSS sur le port de commutateur virtuel Hyper\-V. 

Déterminer les paramètres présents pour vRSS et activer ou désactiver la fonctionnalité pour une machine virtuelle.

   **Afficher les paramètres actuels:** 

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | fl
   ```

   **Activé la fonctionnalité:**
   
   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled [$True|$False]
   ```

## Activer ou désactiver vRSS sur une carte réseau virtuelle hôte

Déterminer les paramètres présents pour vRSS et activer ou désactiver la fonctionnalité d’une carte réseau virtuelle hôte.

   **Afficher les paramètres actuels:** 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | fl
   ```

   **Activer ou désactiver la fonctionnalité:** 

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled [$True|$False]
   ```

## Configurer le mode de planification sur le port de commutateur virtuel Hyper-V 
>S’applique à: Windows Server 2019

Dans Windows Server 2019, vRSS peut mettre à jour les processeurs logiques utilisées pour traiter le trafic réseau dynamiquement.  Appareils avec des pilotes pris en charge sont ce mode planification activé par défaut. 

Déterminer le mode de planification présent sur un système ou modifier le mode de planification pour un ordinateur virtuel.

   **Afficher les paramètres actuels:** 

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | Select 'VRSSQueue'
   ```

   **Définir ou modifier le mode de planification:**

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```

## Configurer le mode de planification sur une carte réseau virtuelle hôte
>S’applique à: Windows Server 2019

Pour déterminer le mode de planification présent ou pour modifier le mode de planification pour une carte réseau virtuelle hôte, utilisez les commandes Windows PowerShell suivantes:

   **Afficher les paramètres actuels:** 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | Select 'VRSSQueue'
   ```

   **Définir ou modifier le mode de planification:** 

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssQueueSchedulingMode -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```


## Rubriques associées 
Pour plus d’informations, consultez les rubriques de référence.

- [Get-VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmnetworkadapter)
- [Set-VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/set-vmnetworkadapter)

Pour plus d’informations, voir [Virtuel \(vrss\ ()](vrss-top.md).