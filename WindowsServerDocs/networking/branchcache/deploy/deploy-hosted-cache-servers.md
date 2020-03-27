---
title: Déployer des serveurs de Cache hébergé (facultatif)
description: Cette rubrique fait partie du Guide de déploiement BranchCache pour Windows Server 2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé pour optimiser l’utilisation de la bande passante WAN dans les filiales.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 96d03b42-6cd9-4905-b6a2-dc36130dd24f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f66e7e728f8e5ca657d002c3c5afcd2d9d23e9ff
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319149"
---
# <a name="deploy-hosted-cache-servers-optional"></a>Déployer des serveurs de Cache hébergé (facultatif)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour installer et configurer les serveurs de cache hébergé de BranchCache qui se trouvent dans les filiales où vous souhaitez déployer le mode de cache hébergé de BranchCache. Avec BranchCache dans Windows Server 2016, vous pouvez déployer plusieurs serveurs de cache hébergé dans une filiale.  
  
> [!IMPORTANT]  
> Cette étape est facultative, car le mode de cache distribué ne nécessite pas un ordinateur de serveur de cache hébergé dans les filiales. Si vous n’envisagez pas de déployer le mode de cache hébergé dans des succursales, vous n’avez pas besoin de déployer un serveur de cache hébergé et vous n’avez pas besoin d’effectuer les étapes de cette procédure.  
  
Vous devez être membre des **administrateurs**ou équivalent pour effectuer cette procédure.  
  
### <a name="to-install-and-configure-a-hosted-cache-server"></a>Pour installer et configurer un serveur de cache hébergé  
  
1.  Sur l’ordinateur que vous souhaitez configurer en tant que serveur de cache hébergé, exécutez la commande suivante à une invite de commandes Windows PowerShell pour installer la fonctionnalité BranchCache.  
  
    `Install-WindowsFeature BranchCache -IncludeManagementTools`  
  
2.  Configurez l’ordinateur en tant que serveur de cache hébergé à l’aide de l’une des commandes suivantes :  
  
    -   Pour configurer un ordinateur qui n’est pas joint à un domaine en tant que serveur de cache hébergé, tapez la commande suivante à l’invite de commandes Windows PowerShell, puis appuyez sur entrée.  
  
        `Enable-BCHostedServer`  
  
    -   Pour configurer un ordinateur joint à un domaine en tant que serveur de cache hébergé, et pour inscrire un point de connexion de service dans Active Directory pour la découverte automatique de serveurs de cache hébergé par les ordinateurs clients, tapez la commande suivante à l’invite de commandes Windows PowerShell, puis Appuyez sur entrée.  
  
        `Enable-BCHostedServer -RegisterSCP`  
  
3.  Pour vérifier la configuration correcte du serveur de cache hébergé, tapez la commande suivante à l’invite de commandes Windows PowerShell, puis appuyez sur entrée.  
  
    `Get-BCStatus`  
  
    > [!NOTE]  
    > Après avoir exécuté cette commande, dans la section **HostedCacheServerConfiguration**, la valeur de **HostedCacheServerIsEnabled** est **true**. Si vous avez configuré un serveur de cache hébergé joint à un domaine pour inscrire un point de connexion de service (SCP) dans Active Directory, la valeur de **HostedCacheScpRegistrationEnabled** est **true**.  
  

