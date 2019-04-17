---
title: Vérifiez les paramètres de l’ordinateur Client
description: Cette rubrique fait partie de la BranchCache déploiement Guide pour Windows Server2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé d’optimiser l’utilisation de la bande passante réseau étendu dans les filiales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 31ea58b0-d407-4f62-8ec6-6a1b19174042
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d507f2097a9349cbefba520ad0dc143e0dd7452e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="verify-client-computer-settings"></a>Vérifiez les paramètres de l’ordinateur Client

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette procédure pour vérifier que l’ordinateur client est configuré correctement pour BranchCache.  
  
> [!NOTE]  
> Cette procédure inclut des étapes pour mettre à jour manuellement la stratégie de groupe et pour le redémarrage du service BranchCache. Vous n’avez pas besoin effectuer ces actions si vous redémarrez l’ordinateur, comme ils seront produira automatiquement dans ce cas.  
  
Vous devez être membre du **administrateurs**, ou équivalent pour effectuer cette procédure.  
  
### <a name="to-verify-branchcache-client-computer-settings"></a>Pour vérifier les paramètres de l’ordinateur client BranchCache  
  
1.  Pour actualiser la stratégie de groupe sur l’ordinateur client dont vous souhaitez vérifier la configuration de BranchCache, exécutez Windows PowerShell en tant qu’administrateur, tapez la commande suivante et appuyez sur ENTRÉE.  
  
    `gpupdate /force`  
  
2.  Pour les ordinateurs clients qui sont configurés en mode de cache hébergé et sont configurés pour détecter automatiquement les serveurs de cache hébergé par point de connexion de service, exécutez les commandes suivantes pour arrêter et redémarrer le service BranchCache.  
  
    `net stop peerdistsvc`  
  
    `net start peerdistsvc`  
  
3.  Vérifiez que le mode de fonctionnement de BranchCache actuel en exécutant la commande suivante.  
  
    `Get-BCStatus`  
  
4.  Dans Windows PowerShell, consultez la sortie de la **Get-BCStatus** commande.  
  
    La valeur de **BranchCacheIsEnabled** doit être **True**.  
  
    Dans **ClientSettings**, la valeur de **CurrentClientMode** doit être **DistributedClient** ou **HostedCacheClient**, selon le mode que vous avez configuré à l’aide de ce guide.  
  
    Dans **ClientSettings**si configuré en mode de cache hébergé et fourni les noms de vos serveurs de cache hébergé lors de la configuration ou si le client a localisé automatiquement hébergé à l’aide de points de connexion de service, des serveurs de cache **HostedCacheServerList** doit avoir une valeur qui est le même que l’ou les noms de vos serveurs de cache hébergé. Par exemple, si votre serveur de cache hébergé est appelée HCS1 et votre domaine est corp.contoso.com, la valeur de **HostedCacheServerList** est **HCS1.corp.contoso.com**.  
  
5.  Si un des paramètres ci-dessus BranchCache n’ont pas les valeurs correctes, utilisez les étapes décrites dans ce guide pour vérifier les paramètres de stratégie de groupe ou stratégie ordinateur Local, ainsi que les exceptions de pare-feu, que vous avez configuré, et assurez-vous que qu’ils sont corrects. En outre, redémarrez l’ordinateur ou suivez les étapes de cette procédure pour actualiser la stratégie de groupe et redémarrez le service BranchCache.  
  


