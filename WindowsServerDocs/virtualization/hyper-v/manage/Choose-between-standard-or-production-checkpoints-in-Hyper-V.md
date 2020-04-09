---
title: Choisir entre des points de contrôle standard ou de production dans Hyper-V
description: Fournit des instructions pour configurer un ordinateur virtuel afin d’utiliser des points de contrôle standard ou de production.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: 92bb573b-03b7-470e-b72e-e35edf52b349
author: kbdazure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 80e26c76e1377c904901f9da10e5fea347e2d333
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852892"
---
# <a name="choose-between-standard-or-production-checkpoints-in-hyper-v"></a>Choisir entre des points de contrôle standard ou de production dans Hyper-V

>S’applique à : Windows 10, Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

  
À compter de Windows Server 2016 et Windows 10, vous pouvez choisir entre les points de contrôle standard et de production pour chaque machine virtuelle. Les points de contrôle de production sont la valeur par défaut pour les nouvelles machines virtuelles.
  
- Les points de contrôle de production sont des images à « point dans le temps » d’une machine virtuelle, qui peuvent être restaurées par la suite d’une manière entièrement prise en charge pour toutes les charges de travail de production. Notez que la création du point de contrôle repose sur la technologie de sauvegarde de l’invité, et non sur la technologie de l’état de mise en mémoire.  
  
- Les points de contrôle standard capturent l’État, les données et la configuration matérielle d’un ordinateur virtuel en cours d’exécution et sont destinés à être utilisés dans les scénarios de développement et de test. Les points de contrôle standard peuvent être utiles si vous devez recréer un État ou une condition spécifique d’une machine virtuelle en cours d’exécution afin de pouvoir résoudre un problème.  
 
  ## <a name="change-checkpoints-to-production-or-standard-checkpoints"></a>Modifier les points de contrôle en production ou points de contrôle standard  
  
1.  Dans le **Gestionnaire Hyper-V**, cliquez avec le bouton droit sur l’ordinateur virtuel, puis cliquez sur **paramètres**.  
  
2.  Sous la section **gestion** , sélectionnez **points de contrôle**.  
  
3.  Sélectionnez les points de contrôle de production ou les points de contrôle standard.  
  
    Si vous choisissez des points de contrôle de production, vous pouvez également spécifier si l’hôte doit prendre un point de contrôle standard si un point de contrôle de production ne peut pas être utilisé. Si vous désactivez cette case à cocher et qu’un point de contrôle de production ne peut pas être utilisé, aucun point de contrôle n’est effectué.  
  
4.  Si vous souhaitez stocker les fichiers de configuration de point de contrôle à un autre emplacement, modifiez-les dans la section **emplacement du fichier de point de contrôle** .  
  
5.  Cliquez sur **appliquer** pour enregistrer vos modifications. Si vous avez terminé, cliquez sur **OK** pour fermer la boîte de dialogue.  
  
> [!NOTE]
> Seuls les **points de contrôle de production** sont pris en charge sur les invités qui exécutent Active Directory Domain Services rôle (contrôleur de domaine) ou services AD LDS (Active Directory Lightweight Directory Services) rôle.

## <a name="see-also"></a>Voir aussi  
  
-   [Points de contrôle de production](../What-s-new-in-Hyper-V-on-Windows.md#production-checkpoints-new)  
  
-   [Activer ou désactiver des points de contrôle](Enable-or-disable-checkpoints-in-Hyper-V.md)  
  


