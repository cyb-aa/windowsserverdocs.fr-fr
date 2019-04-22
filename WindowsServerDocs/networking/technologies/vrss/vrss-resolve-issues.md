---
title: Résolution des problèmes liés à vRSS
description: Résoudre les problèmes de vRSS si vous ne voyez pas vRSS équilibrage du trafic à la machine virtuelle LPs.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/04/2018
ms.openlocfilehash: a2d6eb43149361b4270565b63fc99f483f364f74
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824030"
---
## <a name="resolve-vrss-issues"></a>Résolution des problèmes liés à vRSS

Si vous avez effectué toutes les étapes de préparation et vous ne voyez toujours pas vRSS équilibrage du trafic à la machine virtuelle LPs, il existe différents problèmes possibles.

1. Avant que vous avez effectué les étapes de préparation, vRSS a été désactivé : et maintenant doit être activé. Vous pouvez exécuter **Set-VMNetworkAdapter** pour activer vRSS pour la machine virtuelle.

   ```PowerShell
   Set-VMNetworkAdapter <VMname> -VrssEnabled $TRUE
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
   ```

2. RSS a été désactivé dans la machine virtuelle ou sur la carte réseau virtuelle hôte. Windows Server 2016 Active RSS par défaut ; une personne peut avoir désactivée. 

   - Activé = **True**

   **Afficher les paramètres actuels :** 

   Exécutez l’applet de commande PowerShell suivante dans la machine virtuelle\(pour vRSS dans une machine virtuelle\) ou sur l’ordinateur hôte \(pour vRSS de carte réseau virtuelle hôte\).

   ```PowerShell
   Get-NetAdapterRss
   ```

   **Activer la fonctionnalité :** 

   Pour modifier la valeur de False à True, exécutez l’applet de commande PowerShell suivante.

   ```PowerShell
   Enable-NetAdapterRss *
   ```
   
   Une autre façon de l’échelle du système pour configurer RSS à l’aide de netsh. Utilisez 
   
    ```cmd
   netsh int tcp show global
   ```
   
   Pour vous assurer que RSS n’est pas désactivé globalement. Et l’activer si nécessaire. Ce paramètre n’est pas couvertes par *-NetAdapterRSS.

3. Si vous trouvez que VMMQ n’est pas activé une fois que vous configurez vRSS, vérifiez les paramètres suivants sur chaque carte attachée au commutateur virtuel :

   - VmmqEnabled = **False**
   - VmmqEnabledRequested = **True**

   ![vmmq-enabled](../../media/vmmq-enabled.png)

   **Afficher les paramètres actuels :** 

   ```PowerShell
   Get-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS'
   ```

   **Activer la fonctionnalité :** 

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS' -DisplayValue Enabled”
   ```
 
4. _(Windows Server 2019)_  Vous ne pouvez pas activer VMMQ (VmmqEnabled = False) tandis que le paramètre **VrssQueueSchedulingMode** à **dynamique**. Le VrssQueueSchedulingMode ne change pas dynamique une fois que VMMQ est activé.<p>Le **VrssQueueSchedulingMode** de **dynamique** nécessite la prise en charge de pilote lorsque VMMQ est activé.  VMMQ est un déchargement de l’emplacement de paquets sur les processeurs logiques et nécessite par conséquent, les pilotes pris en charge pour tirer parti de l’algorithme dynamique.  Installez le pilote et le microprogramme qui prend en charge VMMQ dynamiques du fournisseur de carte réseau.



---
