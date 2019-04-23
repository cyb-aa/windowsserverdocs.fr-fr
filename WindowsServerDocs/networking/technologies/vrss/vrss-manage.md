---
title: Gérer vRSS
description: Dans cette rubrique, vous utiliser les commandes Windows PowerShell pour gérer des vRSS dans des machines virtuelles (VM) et sur les ordinateurs hôtes Hyper-V.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0fe5bfc3-591f-4a19-b98a-0668d4c9f93a
ms.localizationpriority: medium
manager: dougkim
ms.date: 09/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8af800608bee7037b48141a7a2edb0c872a7aac0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856190"
---
# <a name="manage-vrss"></a>Gérer vRSS

Dans cette rubrique, vous utiliser les commandes Windows PowerShell pour gérer des vRSS dans des machines virtuelles \(machines virtuelles\) et sur Hyper\-hôtes V.

>[!NOTE]
>Pour plus d’informations sur les commandes mentionnées dans cette rubrique, consultez [commandes PowerShell de Windows pour les flux RSS et vRSS](vrss-wps.md).

## <a name="vmq-on-hyper-v-hosts"></a>VMQ sur les ordinateurs hôtes Hyper-V

Sur l’hôte Hyper-V, vous devez utiliser les mots clés qui contrôlent les processeurs des ordinateurs virtuels.

**Afficher les paramètres actuels :** 

```PowerShell
Get-NetAdapterVmq
```

**Configurez les paramètres de file :** 

```PowerShell
Set-NetAdapterVmq
```


## <a name="vrss-on-hyper-v-switch-ports"></a>ports de commutateur vRSS sur Hyper-V

Sur l’hôte Hyper-V, vous devez également activer vRSS sur Hyper\-port de commutateur virtuel V.

**Afficher les paramètres actuels :**

```PowerShell
Get-VMNetworkAdapter <vm-name> | fl

Get-VMNetworkAdapter -ManagementOS | fl
```
    
Les deux paramètres suivants doivent être **True**. 

- VrssEnabledRequested: True
- VrssEnabled : True
    
>[!IMPORTANT]
>Dans certaines conditions de limitation de ressources, un Hyper\-port de commutateur virtuel V peut être incapable d’activer cette fonctionnalité. Il s’agit d’une condition temporaire, et la fonctionnalité peut devenir disponible à un moment ultérieur.
>
>Si **VrssEnabled** est **True**, puis la fonctionnalité est activée pour cette Hyper\-port de commutateur virtuel V, autrement dit, pour cette machine virtuelle ou d’une carte réseau virtuelle.

**Configurer les paramètres de vRSS de port de commutateur :**

```PowerShell
Set-VMNetworkAdapter <vm-name> -VrssEnabled $TRUE
    
Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
```

## <a name="vrss-in-vms-and-host-vnics"></a>vRSS dans les machines virtuelles et des cartes réseau virtuelles hôtes

Vous pouvez utiliser les mêmes commandes utilisées pour RSS native pour configurer les paramètres de vRSS dans les machines virtuelles et des cartes réseau virtuelles de l’hôte, qui est également la façon d’activer RSS sur les cartes réseau virtuelles hôtes.  

**Afficher les paramètres actuels :**

```PowerShell
Get-NetAdapterRSS
```

**Configurer les paramètres de vRSS :**

```PowerShell
Set-NetAdapterRss
```

>[!NOTE]
> Définition du profil à l’intérieur de la machine virtuelle n’affecte pas la planification du travail. Hyper\-V rend tous les la planification des décisions et ignore le profil à l’intérieur de la machine virtuelle.

## <a name="disable-vrss"></a>Désactiver vRSS

Vous pouvez désactiver vRSS pour désactiver les paramètres mentionnés précédemment.

- Désactiver VMQ pour la carte réseau physique ou de la machine virtuelle.

  >[!CAUTION]
  >La désactivation de VMQ sur physique carte d’interface réseau affecte sérieusement la capacité de votre Hyper\-hôte V pour gérer les paquets entrants.

- Désactiver vRSS pour une machine virtuelle sur Hyper\-port de commutateur virtuel V sur Hyper\-hôte V.

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled $FALSE
   ```

- Désactiver vRSS pour une carte réseau virtuelle hôte sur l’Hyper\-port de commutateur virtuel V sur Hyper\-hôte V.

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $FALSE
   ```

- Désactiver RSS dans la machine virtuelle \(ou carte réseau virtuelle hôte\) à l’intérieur de la machine virtuelle \(ou sur l’ordinateur hôte\)

   ```PowerShell
   Disable-NetAdapterRSS *
   ```
