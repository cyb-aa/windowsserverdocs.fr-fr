---
title: Résolution des problèmes liés à vRSS
description: Résoudre les problèmes de vRSS si vous ne voyez pas vRSS charger l’équilibrage de charge le trafic vers les PL de machine virtuelle.
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
ms.sourcegitcommit: 515b4fd5c40fcbae0e12a2c30090384533972353
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8232518"
---
## Résolution des problèmes liés à vRSS

Si vous avez terminé toutes les étapes de préparation et que vous ne voyez toujours pas vRSS charger l’équilibrage de charge le trafic vers les PL de machine virtuelle, il existe différents problèmes possibles.

1. Avant que vous avez effectué les étapes de préparation, vRSS a été désactivé - et doit maintenant être activé. Vous pouvez exécuter **Set-VMNetworkAdapter** pour activer vRSS pour la machine virtuelle.

   ```PowerShell
   Set-VMNetworkAdapter <VMname> -VrssEnabled $TRUE
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
   ```

2. RSS a été désactivé dans la machine virtuelle ou sur la carte réseau virtuelle hôte. Windows Server 2016 Active RSS par défaut; un utilisateur peut l’avoir désactivée. 

   - Activé = **True**

   **Afficher les paramètres actuels:** 

   Exécutez l’applet de commande PowerShell suivante dans le VM\ (pour vRSS dans un VM\) ou sur l’ordinateur hôte \ (pour vRSS\ de carte réseau virtuelle hôte).

   ```PowerShell
   Get-NetAdapterRss
   ```

   **Activer la fonctionnalité:** 

   Pour modifier la valeur de False true, exécutez l’applet de commande PowerShell suivante.

   ```PowerShell
   Enable-NetAdapterRss *
   ```
   
   Une autre façon à l’échelle du système de configuration du flux RSS est à l’aide de netsh. Utiliser 
   
    ```cmd
   netsh int tcp show global
   ```
   
   Pour vous assurer que cette RSS n’est pas désactivé globalement. Et l’activer si nécessaire. Ce paramètre n’est pas touché par *-NetAdapterRSS.

3. Si vous trouvez que VMMQ n’est pas activée après avoir configuré vRSS, vérifiez les paramètres suivants sur chaque carte associée au commutateur virtuel:

   - VmmqEnabled = **False**
   - VmmqEnabledRequested = **True**

   ![vmmq compatible](../../media/vmmq-enabled.png)

   **Afficher les paramètres actuels:** 

   ```PowerShell
   Get-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS'
   ```

   **Activer la fonctionnalité:** 

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS' -DisplayValue Enabled”
   ```
 
4. _(Windows Server 2019)_ Vous ne pouvez pas activer VMMQ (VmmqEnabled = False) lors de la définition **VrssQueueSchedulingMode** **dynamique**. Le VrssQueueSchedulingMode ne change pas dynamique une fois que VMMQ est activé.<p>Le **VrssQueueSchedulingMode** de **Dynamic** nécessite prise en charge du pilote lorsque VMMQ est activé.  VMMQ est un déchargement de l’emplacement de paquets sur les processeurs logiques et par conséquent, nécessite prise en charge des pilotes de tirer parti de l’algorithme dynamique.  Installez les pilotes et des microprogrammes qui prend en charge VMMQ dynamique de fournisseur de la carte réseau.



---
