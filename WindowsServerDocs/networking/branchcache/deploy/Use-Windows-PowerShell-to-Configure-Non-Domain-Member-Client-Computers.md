---
title: Utiliser Windows PowerShell pour configurer les ordinateurs clients membres du domaine
description: Cette rubrique fait partie de la BranchCache déploiement Guide pour Windows Server2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé d’optimiser l’utilisation de la bande passante réseau étendu dans les filiales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 1b511e1a-686d-441f-a1c7-d4d029e1a061
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a5415d9fa5a4af806f23a1af9c907b1f02e9627b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="use-windows-powershell-to-configure-non-domain-member-client-computers"></a>Utiliser Windows PowerShell pour configurer les ordinateurs clients membres du domaine

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette procédure pour configurer manuellement un ordinateur client de BranchCache pour le mode de cache distribué ou en mode de cache hébergé.  
  
> [!NOTE]  
> Si vous avez configuré les ordinateurs clients BranchCache à l’aide de la stratégie de groupe, les paramètres de stratégie de groupe remplacent toute configuration manuelle des ordinateurs clients sur lesquels les stratégies sont appliquées.  
  
L’appartenance au groupe **administrateurs**, ou équivalent est le minimum requis pour effectuer cette procédure.  
  
### <a name="to-enable-branchcache-distributed-or-hosted-cache-mode"></a>Pour activer le mode de cache distribué ou hébergé de BranchCache  
  
1.  Sur l’ordinateur client BranchCache que vous souhaitez configurer, exécutez Windows PowerShell en tant qu’administrateur, puis effectuez l’une des opérations suivantes.  
  
    -   Pour configurer l’ordinateur client pour le mode de cache distribué BranchCache, tapez la commande suivante et appuyez sur ENTRÉE.  
  
        `Enable-BCDistributed`  
  
    -   Pour configurer l’ordinateur client pour le mode de cache hébergé de BranchCache, tapez la commande suivante et appuyez sur ENTRÉE.  
  
        `Enable-BCHostedClient`  
  
        > [!TIP]  
        > Si vous souhaitez spécifier les serveurs de cache hébergé disponibles, utilisez la `-ServerNames` liste de vos serveurs de cache hébergé en tant que la valeur du paramètre séparés par le paramètre par une virgule. Par exemple, si vous disposez de deux serveurs de cache hébergé nommés HCS1 et HCS2, vous pouvez configurer l’ordinateur client pour le mode de cache hébergé avec la commande suivante.  
        >   
        > `Enable-BCHostedClient -ServerNames HCS1,HCS2`  
  


