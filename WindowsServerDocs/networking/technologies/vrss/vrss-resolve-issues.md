---
title: Résolution des problèmes liés à vRSS
description: Résolvez les problèmes vRSS si vous ne voyez pas le trafic d’équilibrage de charge vRSS sur la machine virtuelle.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/04/2018
ms.openlocfilehash: 850aa376e8cd0060992573561a0c32af563b88ad
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405154"
---
## <a name="resolve-vrss-issues"></a>Résolution des problèmes liés à vRSS

Si vous avez effectué toutes les étapes de préparation et que vous ne voyez toujours pas le trafic d’équilibrage de charge vRSS sur la machine virtuelle, il existe différents problèmes possibles.

1. Avant d’effectuer les étapes de préparation, le vRSS a été désactivé et doit être activé. Vous pouvez exécuter **Set-VMNetworkAdapter** pour activer le vRSS pour la machine virtuelle.

   ```PowerShell
   Set-VMNetworkAdapter <VMname> -VrssEnabled $TRUE
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
   ```

2. RSS a été désactivé sur la machine virtuelle ou sur l’ordinateur hôte carte réseau virtuelle. Windows Server 2016 active RSS par défaut ; quelqu’un l’a peut-être désactivée. 

   - Activé = **true**

   **Affichez les paramètres actuels :** 

   Exécutez l’applet de commande PowerShell suivante dans\(la machine virtuelle pour vrss\) dans une machine virtuelle \(ou sur l’ordinateur\)hôte pour l’hôte carte réseau virtuelle vRSS.

   ```PowerShell
   Get-NetAdapterRss
   ```

   **Activez la fonctionnalité :** 

   Pour remplacer la valeur false par true, exécutez l’applet de commande PowerShell suivante.

   ```PowerShell
   Enable-NetAdapterRss *
   ```
   
   Netsh est une autre méthode à l’ensemble du système pour configurer RSS. Utilisez 
   
    ```cmd
   netsh int tcp show global
   ```
   
   pour vous assurer que RSS n’est pas désactivé globalement. Et activez-le si nécessaire. Ce paramètre n’est pas affecté par *-NetAdapterRSS.

3. Si vous trouvez que VMMQ n’est pas activé après avoir configuré vRSS, vérifiez les paramètres suivants sur chaque carte attachée au commutateur virtuel :

   - VmmqEnabled = **false**
   - VmmqEnabledRequested = **true**

   ![vmmq-activé](../../media/vmmq-enabled.png)

   **Affichez les paramètres actuels :** 

   ```PowerShell
   Get-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS'
   ```

   **Activez la fonctionnalité :** 

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS' -DisplayValue Enabled”
   ```
 
4. _(Windows Server 2019)_ Vous ne pouvez pas activer VMMQ (VmmqEnabled = false) lors de la définition de **VrssQueueSchedulingMode** sur **Dynamic**. VrssQueueSchedulingMode ne change pas en dynamique une fois VMMQ est activé.<p>Le **VrssQueueSchedulingMode** de **Dynamic** requiert la prise en charge du pilote lorsque VMMQ est activé.  VMMQ est un déchargement de l’emplacement des paquets sur les processeurs logiques et, par conséquent, nécessite une prise en charge du pilote pour tirer parti de l’algorithme dynamique.  Installez le pilote et le microprogramme du fournisseur de la carte réseau qui prend en charge les VMMQ dynamiques.



---
