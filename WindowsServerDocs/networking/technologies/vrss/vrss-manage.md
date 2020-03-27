---
title: Gérer vRSS
description: Dans cette rubrique, vous allez utiliser les commandes Windows PowerShell pour gérer vRSS dans les machines virtuelles et sur les hôtes Hyper-V.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 0fe5bfc3-591f-4a19-b98a-0668d4c9f93a
ms.localizationpriority: medium
manager: dougkim
ms.date: 09/05/2018
ms.author: lizross
author: eross-msft
ms.openlocfilehash: fc2feb9101c84566304910b1ee24f13f1ac277f1
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315354"
---
# <a name="manage-vrss"></a>Gérer vRSS

Dans cette rubrique, vous allez utiliser les commandes Windows PowerShell pour gérer vRSS dans les machines virtuelles \(les machines virtuelles\) et sur les hôtes Hyper\-V.

>[!NOTE]
>Pour plus d’informations sur les commandes mentionnées dans cette rubrique, consultez [commandes Windows PowerShell pour RSS et vRSS](vrss-wps.md).

## <a name="vmq-on-hyper-v-hosts"></a>Ordinateurs virtuels sur les hôtes Hyper-V

Sur l’hôte Hyper-V, vous devez utiliser les mots clés qui contrôlent les processeurs de l’ordinateur hôte.

**Affichez les paramètres actuels :** 

```PowerShell
Get-NetAdapterVmq
```

**Configurez les paramètres de l’ordinateurs virtuels :** 

```PowerShell
Set-NetAdapterVmq
```


## <a name="vrss-on-hyper-v-switch-ports"></a>vRSS sur les ports de commutateur Hyper-V

Sur l’hôte Hyper-V, vous devez également activer vRSS sur le port de commutateur virtuel Hyper\-V.

**Affichez les paramètres actuels :**

```PowerShell
Get-VMNetworkAdapter <vm-name> | fl

Get-VMNetworkAdapter -ManagementOS | fl
```
    
Les deux paramètres suivants doivent être **vrais**. 

- VrssEnabledRequested : true
- VrssEnabled : true
    
>[!IMPORTANT]
>Dans certaines conditions de limitation de ressources, il se peut que cette fonctionnalité ne soit pas activée pour un port de commutateur virtuel Hyper\-V. Il s’agit d’une condition temporaire qui peut être disponible à un moment ultérieur.
>
>Si **VrssEnabled** a la **valeur true**, cela signifie que la fonctionnalité est activée pour ce port de commutateur virtuel Hyper\-V, c’est-à-dire pour cette machine virtuelle ou carte réseau virtuelle.

**Configurez les paramètres vRSS du port du commutateur :**

```PowerShell
Set-VMNetworkAdapter <vm-name> -VrssEnabled $TRUE
    
Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
```

## <a name="vrss-in-vms-and-host-vnics"></a>vRSS dans les machines virtuelles et les cartes réseau virtuelles hôtes

Vous pouvez utiliser les mêmes commandes que celles utilisées pour le flux RSS natif afin de configurer les paramètres vRSS dans les machines virtuelles et les cartes réseau virtuelles hôtes, ce qui permet également d’activer RSS sur les cartes réseau virtuelles hôtes.  

**Affichez les paramètres actuels :**

```PowerShell
Get-NetAdapterRSS
```

**Configurer les paramètres vRSS :**

```PowerShell
Set-NetAdapterRss
```

>[!NOTE]
> La définition du profil à l’intérieur de la machine virtuelle n’a aucun impact sur la planification du travail. Hyper\-V prend toutes les décisions de planification et ignore le profil à l’intérieur de la machine virtuelle.

## <a name="disable-vrss"></a>Désactiver vRSS

Vous pouvez désactiver vRSS pour désactiver l’un des paramètres mentionnés précédemment.

- Désactivez l’ordinateur virtuel pour la carte réseau physique ou la machine virtuelle.

  >[!CAUTION]
  >La désactivation des ordinateurs virtuels sur la carte réseau physique a un impact considérable sur la capacité de votre hôte Hyper\-V à gérer les paquets entrants.

- Désactivez vRSS pour une machine virtuelle sur le port de commutateur virtuel Hyper\-V sur l’hôte Hyper\-V.

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled $FALSE
   ```

- Désactivez vRSS pour un ordinateur hôte carte réseau virtuelle sur le port de commutateur virtuel Hyper\-V sur l’hôte Hyper\-V.

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $FALSE
   ```

- Désactivez RSS dans le \(de la machine virtuelle ou l’hôte carte réseau virtuelle\) à l’intérieur de la machine virtuelle \(ou sur l’ordinateur hôte\)

   ```PowerShell
   Disable-NetAdapterRSS *
   ```
