---
title: Choisissez entre les points de contrôle standard ou de production dans Hyper-V
description: Fournit des instructions sur la configuration d’une machine virtuelle à utiliser des points de contrôle standard ou de production
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 92bb573b-03b7-470e-b72e-e35edf52b349
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 3591e17c9485fc8f9e365f6322c4f48e783db8ce
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442228"
---
# <a name="choose-between-standard-or-production-checkpoints-in-hyper-v"></a>Choisissez entre les points de contrôle standard ou de production dans Hyper-V

>S'applique à : Windows 10, Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

  
À partir de Windows Server 2016 et Windows 10, vous pouvez choisir entre les points de contrôle standard et de production pour chaque machine virtuelle. Points de contrôle de production sont la valeur par défaut pour les nouvelles machines virtuelles.
  
- Points de contrôle de production sont des images « ponctuelles » d’une machine virtuelle, qui peuvent être restaurés par la suite d’une manière qui est complètement pris en charge pour toutes les charges de travail de production. Notez que la création du point de contrôle repose sur la technologie de sauvegarde de l’invité, et non sur la technologie de l’état de mise en mémoire.  
  
- Points de contrôle standard capturent l’état, données et la configuration matérielle d’un ordinateur virtuel en cours d’exécution et sont destinés à utiliser dans les scénarios de développement et de test. Points de contrôle standard peuvent être utiles si vous avez besoin de recréer un état spécifique ou une condition d’une machine virtuelle en cours d’exécution afin que vous pouvez résoudre un problème.  
 
  ## <a name="change-checkpoints-to-production-or-standard-checkpoints"></a>Modifier les points de contrôle de production ou des points de contrôle standard  
  
1.  Dans **Gestionnaire Hyper-V**, cliquez sur la machine virtuelle et cliquez sur **paramètres**.  
  
2.  Sous le **gestion** section, sélectionnez **points de contrôle**.  
  
3.  Sélectionnez les points de contrôle de production ou les points de contrôle standard.  
  
    Si vous choisissez les points de contrôle de production, vous pouvez également spécifier si l’hôte doit effectuer un point de contrôle standard si un point de contrôle de production ne peut pas être effectuée. Si vous désactivez cette case à cocher et un point de contrôle de production ne peut pas être effectuée, aucun point de contrôle n’est effectuée.  
  
4.  Si vous souhaitez stocker les fichiers de configuration de point de contrôle dans un emplacement différent, modifiez-le dans le **emplacement du fichier de point de contrôle** section.  
  
5.  Cliquez sur **appliquer** pour enregistrer vos modifications. Si vous avez terminé, cliquez sur **OK** pour fermer la boîte de dialogue.  
  
> [!NOTE]
> Uniquement **points de contrôle de Production** sont pris en charge sur des invités qui exécutent le rôle de Services de domaine Active Directory (contrôleur de domaine) ou Active Directory Lightweight Directory Services.

## <a name="see-also"></a>Voir aussi  
  
-   [Points de contrôle de production](../What-s-new-in-Hyper-V-on-Windows.md#BKMK_check)  
  
-   [Activer ou désactiver des points de contrôle](Enable-or-disable-checkpoints-in-Hyper-V.md)  
  


