---
title: Gérer les vRSS
description: Dans cette rubrique, vous utilisez les commandes Windows PowerShell pour gérer les vRSS dans les machines virtuelles (VM) et sur les hôtes Hyper-V.
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
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133835"
---
# Gérer les vRSS

Dans cette rubrique, vous utilisez les commandes Windows PowerShell pour gérer les vRSS dans les ordinateurs \(VMs\) et sur les hôtes Hyper\-V.

>[!NOTE]
>Pour plus d’informations sur les commandes mentionnés dans cette rubrique, consultez [Les commandes Windows PowerShell pour RSS et vRSS](vrss-wps.md).

## VMQ sur des hôtes Hyper-V

Sur l’hôte Hyper-V, vous devez utiliser les mots clés qui contrôlent les processeurs VMQ.

**Afficher les paramètres actuels:** 

```PowerShell
Get-NetAdapterVmq
```

**Configurez les paramètres VMQ:** 

```PowerShell
Set-NetAdapterVmq
```


## ports de commutateur vRSS sur Hyper-V

Sur l’hôte Hyper-V, vous devez également activer vRSS sur le port de commutateur virtuel Hyper\-V.

**Afficher les paramètres actuels:**

```PowerShell
Get-VMNetworkAdapter <vm-name> | fl

Get-VMNetworkAdapter -ManagementOS | fl
```
    
Les deux des paramètres suivants doivent être **défini sur True**. 

- VrssEnabledRequested: True
- VrssEnabled: True
    
>[!IMPORTANT]
>Dans certaines conditions de limitation de ressource, un port de commutateur virtuel Hyper\-V peut être impossible d’activer cette fonctionnalité. Il s’agit d’une condition temporaire, et la fonctionnalité peut devenir indisponible à un moment ultérieur.
>
>Si **VrssEnabled** a la **valeur True**, la fonctionnalité est activée pour ce port de commutateur virtuel Hyper\-V, autrement dit, pour cette machine virtuelle ou d’une carte réseau virtuelle.

**Configurer les paramètres de vRSS de port de commutateur:**

```PowerShell
Set-VMNetworkAdapter <vm-name> -VrssEnabled $TRUE
    
Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
```

## vRSS dans les machines virtuelles et les cartes réseau virtuelles hôtes

Vous pouvez utiliser les mêmes commandes utilisées pour RSS natives pour configurer les paramètres de vRSS dans des machines virtuelles et les cartes réseau virtuelles hôtes, qui est également la façon d’activer RSS sur les cartes réseau virtuelles hôtes.  

**Afficher les paramètres actuels:**

```PowerShell
Get-NetAdapterRSS
```

**Configurer les paramètres de vRSS:**

```PowerShell
Set-NetAdapterRss
```

>[!NOTE]
> Définir le profil à l’intérieur de l’ordinateur virtuel n’affecte pas la planification du travail. Hyper\-V prend toutes les décisions de planification et ignore le profil à l’intérieur de l’ordinateur virtuel.

## Désactiver vRSS

Vous pouvez désactiver vRSS pour désactiver les paramètres mentionnés précédemment.

- Désactiver VMQ pour la carte réseau physique ou de l’ordinateur virtuel.

  >[!CAUTION]
  >La désactivation de VMQ sur la physique NIC sérieusement a une incidence sur la possibilité de votre hôte Hyper\-V pour gérer les paquets entrants.

- Désactiver vRSS pour une machine virtuelle sur le port de commutateur virtuel Hyper\-V sur l’hôte Hyper\-V.

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled $FALSE
   ```

- Désactivez vRSS pour une carte réseau virtuelle hôte sur le port de commutateur virtuel Hyper\-V sur l’hôte Hyper\-V.

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $FALSE
   ```

- Désactivation de RSS dans la machine virtuelle \(or host vNIC\) à l’intérieur de l’ordinateur virtuel \ (ou sur le host\)

   ```PowerShell
   Disable-NetAdapterRSS *
   ```
