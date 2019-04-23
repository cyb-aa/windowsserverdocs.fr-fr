---
title: Utiliser Windows PowerShell pour configurer les ordinateurs Client membre n’appartenant pas à un domaine
description: Cette rubrique fait partie de BranchCache déploiement Guide pour Windows Server 2016, qui montre comment déployer BranchCache en mode cache distribué et hébergé pour optimiser l’utilisation de la bande passante WAN dans les succursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 1b511e1a-686d-441f-a1c7-d4d029e1a061
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9abb77e573d7b3f144ab831c655c81370a4a6af1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839950"
---
# <a name="use-windows-powershell-to-configure-non-domain-member-client-computers"></a>Utiliser Windows PowerShell pour configurer les ordinateurs Client membre n’appartenant pas à un domaine

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour configurer manuellement un ordinateur de client BranchCache pour le mode cache distribué ou mode de cache hébergé.  
  
> [!NOTE]  
> Si vous avez configuré les ordinateurs clients BranchCache à l’aide de la stratégie de groupe, les paramètres de stratégie de groupe remplacent toute configuration manuelle des ordinateurs clients auxquels les stratégies sont appliquées.  
  
Pour exécuter cette procédure, il est nécessaire d'appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.  
  
### <a name="to-enable-branchcache-distributed-or-hosted-cache-mode"></a>Pour activer le mode de cache distribué ou hébergé de BranchCache  
  
1.  Sur l’ordinateur client BranchCache que vous souhaitez configurer, exécuter Windows PowerShell en tant qu’administrateur, puis effectuez l’une des opérations suivantes.  
  
    -   Pour configurer l’ordinateur client pour le mode de cache distribué de BranchCache, tapez la commande suivante, puis appuyez sur ENTRÉE.  
  
        `Enable-BCDistributed`  
  
    -   Pour configurer l’ordinateur client pour le mode de cache hébergé de BranchCache, tapez la commande suivante, puis appuyez sur ENTRÉE.  
  
        `Enable-BCHostedClient`  
  
        > [!TIP]  
        > Si vous souhaitez spécifier les serveurs de cache hébergé disponibles, utilisez la `-ServerNames` liste de vos serveurs de cache hébergé en tant que la valeur du paramètre séparés par le paramètre avec une virgule. Par exemple, si vous avez deux serveurs de cache hébergé nommés HCS1 et HCS2, configurez l’ordinateur client pour le mode de cache hébergé avec la commande suivante.  
        >   
        > `Enable-BCHostedClient -ServerNames HCS1,HCS2`  
  


