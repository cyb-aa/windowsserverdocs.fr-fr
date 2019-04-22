---
title: Déployer des serveurs de Cache hébergé (facultatif)
description: Cette rubrique fait partie de BranchCache déploiement Guide pour Windows Server 2016, qui montre comment déployer BranchCache en mode cache distribué et hébergé pour optimiser l’utilisation de la bande passante WAN dans les succursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 96d03b42-6cd9-4905-b6a2-dc36130dd24f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b19680e933e7a33871816578b63c5a141db0ce00
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826210"
---
# <a name="deploy-hosted-cache-servers-optional"></a>Déployer des serveurs de Cache hébergé (facultatif)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour installer et configurer les serveurs de cache hébergé de BranchCache qui sont trouvent dans des succursales où vous souhaitez déployer le mode de cache hébergé de BranchCache. Avec BranchCache dans Windows Server 2016, vous pouvez déployer plusieurs serveurs de cache hébergé dans une filiale.  
  
> [!IMPORTANT]  
> Cette étape est facultative, car le mode de cache distribué ne nécessite pas d’un ordinateur de serveur de cache hébergé dans les succursales. Si vous ne comptez pas sur déploiement hébergé de mode de cache dans les succursales, vous n’avez pas besoin de déployer un serveur de cache hébergé, et vous n’avez pas besoin d’effectuer les étapes de cette procédure.  
  
Vous devez être membre du **administrateurs**, ou équivalent pour effectuer cette procédure.  
  
### <a name="to-install-and-configure-a-hosted-cache-server"></a>Pour installer et configurer un serveur de cache hébergé  
  
1.  Sur l’ordinateur que vous souhaitez configurer en tant que serveur de cache hébergé, exécutez la commande suivante à l’invite de Windows PowerShell pour installer la fonctionnalité BranchCache.  
  
    `Install-WindowsFeature BranchCache -IncludeManagementTools`  
  
2.  Configurez l’ordinateur comme serveur de cache hébergé en utilisant l’une des commandes suivantes :  
  
    -   Pour configurer un ordinateur joint au domaine non comme un serveur de cache hébergé, tapez la commande suivante à l’invite Windows PowerShell, puis appuyez sur ENTRÉE.  
  
        `Enable-BCHostedServer`  
  
    -   Pour configurer un ordinateur joint au domaine en tant que serveur de cache hébergé et pour inscrire un point de connexion de service dans Active Directory pour la découverte de serveur de cache hébergé automatique par les ordinateurs clients, tapez la commande suivante à l’invite Windows PowerShell, puis Appuyez sur ENTRÉE.  
  
        `Enable-BCHostedServer -RegisterSCP`  
  
3.  Pour vérifier la configuration correcte du serveur de cache hébergé, tapez la commande suivante à l’invite Windows PowerShell, puis appuyez sur ENTRÉE.  
  
    `Get-BCStatus`  
  
    > [!NOTE]  
    > Après avoir exécuté cette commande, dans la section **HostedCacheServerConfiguration**, la valeur de **HostedCacheServerIsEnabled** est **True**. Si vous avez configuré un serveur de cache hébergé joint à un domaine pour inscrire un point de connexion de service (SCP) dans Active Directory, la valeur de **HostedCacheScpRegistrationEnabled** est **True**.  
  

