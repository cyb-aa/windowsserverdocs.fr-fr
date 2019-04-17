---
title: Déployer des serveurs de Cache hébergé (facultatif)
description: Cette rubrique fait partie de la BranchCache déploiement Guide pour Windows Server2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé d’optimiser l’utilisation de la bande passante réseau étendu dans les filiales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 96d03b42-6cd9-4905-b6a2-dc36130dd24f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d7345b9acf5ef5003cc2a811569083d7c12894a1
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-hosted-cache-servers-optional"></a>Déployer des serveurs de Cache hébergé (facultatif)

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette procédure pour installer et configurer les serveurs de cache hébergé de BranchCache qui sont trouvent dans les filiales, où vous souhaitez déployer le mode de cache hébergé de BranchCache. Avec BranchCache dans Windows Server2016, vous pouvez déployer plusieurs serveurs de cache hébergé dans une filiale.  
  
> [!IMPORTANT]  
> Cette étape est facultative, car le mode de cache distribué ne requiert pas d’un ordinateur de serveur de cache hébergé dans les filiales. Si vous n’envisagez pas de déploiement hébergé le mode de cache dans les succursales, vous n’avez pas besoin de déployer un serveur de cache hébergé et vous n’avez pas besoin d’effectuer les étapes de cette procédure.  
  
Vous devez être membre du **administrateurs**, ou équivalent pour effectuer cette procédure.  
  
### <a name="to-install-and-configure-a-hosted-cache-server"></a>Pour installer et configurer un serveur de cache hébergé  
  
1.  Sur l’ordinateur que vous souhaitez configurer en tant que serveur de cache hébergé, exécutez la commande suivante à une invite de commandes Windows PowerShell pour installer la fonctionnalité BranchCache.  
  
    `Install-WindowsFeature BranchCache -IncludeManagementTools`  
  
2.  Configurer l’ordinateur comme serveur de cache hébergé à l’aide d’une des commandes suivantes:  
  
    -   Pour configurer un ordinateur joint au domaine comme serveur de cache hébergé, tapez la commande suivante à l’invite Windows PowerShell et appuyez sur ENTRÉE.  
  
        `Enable-BCHostedServer`  
  
    -   Pour configurer un domaine joint l’ordinateur comme serveur de cache hébergé et pour inscrire un point de connexion de service dans ActiveDirectory pour la découverte de serveur de cache hébergé automatique par les ordinateurs clients, tapez la commande suivante à l’invite Windows PowerShell et appuyez sur ENTRÉE.  
  
        `Enable-BCHostedServer -RegisterSCP`  
  
3.  Pour vérifier la configuration correcte du serveur de cache hébergé, tapez la commande suivante à l’invite Windows PowerShell et appuyez sur ENTRÉE.  
  
    `Get-BCStatus`  
  
    > [!NOTE]  
    > Après avoir exécuté cette commande, dans la section **HostedCacheServerConfiguration**, la valeur de **HostedCacheServerIsEnabled** est **True **. Si vous avez configuré un serveur de cache hébergé joint à un domaine pour enregistrer un point de connexion de service (SCP) dans ActiveDirectory, la valeur de **HostedCacheScpRegistrationEnabled** est **True **.  
  

