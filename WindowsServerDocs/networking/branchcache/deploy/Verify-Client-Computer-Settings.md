---
title: Vérifier les paramètres de l’ordinateur client
description: Cette rubrique fait partie du Guide de déploiement BranchCache pour Windows Server 2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé pour optimiser l’utilisation de la bande passante WAN dans les filiales.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 31ea58b0-d407-4f62-8ec6-6a1b19174042
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6d0adbf0db2d7888ca12ca49f50fc37baa8cbc16
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356511"
---
# <a name="verify-client-computer-settings"></a>Vérifier les paramètres de l’ordinateur client

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour vérifier que l’ordinateur client est correctement configuré pour BranchCache.  
  
> [!NOTE]  
> Cette procédure comprend les étapes de mise à jour manuelle de stratégie de groupe et de redémarrage du service BranchCache. Vous n’avez pas besoin d’effectuer ces actions si vous redémarrez l’ordinateur, car elles seront exécutées automatiquement dans ce cas.  
  
Vous devez être membre des **administrateurs**ou équivalent pour effectuer cette procédure.  
  
### <a name="to-verify-branchcache-client-computer-settings"></a>Pour vérifier les paramètres de l’ordinateur client BranchCache  
  
1.  Pour actualiser stratégie de groupe sur l’ordinateur client dont vous souhaitez vérifier la configuration BranchCache, exécutez Windows PowerShell en tant qu’administrateur, tapez la commande suivante, puis appuyez sur entrée.  
  
    `gpupdate /force`  
  
2.  Pour les ordinateurs clients configurés en mode de cache hébergé et configurés pour découvrir automatiquement les serveurs de cache hébergé par point de connexion de service, exécutez les commandes suivantes pour arrêter et redémarrer le service BranchCache.  
  
    `net stop peerdistsvc`  
  
    `net start peerdistsvc`  
  
3.  Inspectez le mode opérationnel BranchCache actuel en exécutant la commande suivante.  
  
    `Get-BCStatus`  
  
4.  Dans Windows PowerShell, passez en revue la sortie de la commande **BCStatus** .  
  
    La valeur de **BranchCacheIsEnabled** doit être **true**.  
  
    Dans **ClientSettings**, la valeur de **CurrentClientMode** doit être **DistributedClient** ou **HostedCacheClient**, selon le mode que vous avez configuré à l’aide de ce guide.  
  
    Dans **ClientSettings**, si vous avez configuré le mode de cache hébergé et fourni les noms de vos serveurs de cache hébergé pendant la configuration, ou si le client a localisé automatiquement les serveurs de cache hébergé à l’aide de points de connexion de service,  **HostedCacheServerList** doit avoir une valeur identique au nom ou aux noms de vos serveurs de cache hébergé. Par exemple, si votre serveur de cache hébergé est nommé HCS1 et que votre domaine est corp.contoso.com, la valeur de **HostedCacheServerList** est **HCS1.Corp.contoso.com**.  
  
5.  Si l’un des paramètres BranchCache répertoriés ci-dessus n’a pas les valeurs correctes, suivez les étapes de ce guide pour vérifier les paramètres de stratégie de stratégie de groupe ou de l’ordinateur local, ainsi que les exceptions de pare-feu que vous avez configurées et vérifiez qu’elles sont correctes. En outre, redémarrez l’ordinateur ou suivez les étapes de cette procédure pour actualiser stratégie de groupe et redémarrer le service BranchCache.  
  


