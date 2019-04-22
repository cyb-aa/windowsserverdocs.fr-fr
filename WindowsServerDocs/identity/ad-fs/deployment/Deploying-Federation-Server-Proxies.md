---
ms.assetid: 1b21b0a9-1fe6-4fd1-8a25-92e578d774ed
title: Déploiement de serveurs proxy de fédération
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: b914141a0445febd3961b688aadc2f444b2eee7b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816260"
---
# <a name="deploying-federation-server-proxies"></a>Déploiement de serveurs proxy de fédération

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Pour déployer des serveurs proxy de fédération dans Active Directory Federation Services \(AD FS\), effectuez toutes les tâches dans [liste de vérification : Configuration d’un serveur Proxy de fédération](Checklist--Setting-Up-a-Federation-Server-Proxy.md).  
  
> [!NOTE]  
> Lorsque vous utilisez cette liste de vérification, nous vous recommandons de commencer par lire les références au serveur proxy de fédération planification dans le [Guide de conception AD FS dans Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) avant de commencer les procédures de configuration des serveurs. Suivant la liste de vérification fournit une meilleure compréhension du processus de conception et de déploiement pour la fédération de serveurs proxy.  
  
## <a name="about-federation-server-proxies"></a>Sur les serveurs proxy de fédération  
Serveurs proxy de fédération est des ordinateurs qui exécutent Windows Server® 2012 et le logiciel AD FS qui ont été configurés manuellement pour jouer le rôle de proxy. Vous pouvez utiliser des serveurs proxy de fédération dans votre organisation pour fournir des services intermédiaires entre un client Internet et un serveur de fédération qui se trouve derrière un pare-feu sur votre réseau d’entreprise.  
  
> [!NOTE]  
> Bien que le serveur de fédération et les rôles de proxy de serveur de fédération ne peut pas être installés sur le même ordinateur, un serveur de fédération peut exécuter des fonctions de proxy de serveur de fédération. Pour plus d'informations, voir [When to Create a Federation Server](https://technet.microsoft.com/library/dd807101.aspx).  
  
Le fait d’installer le logiciel AD FS sur un ordinateur Windows Server® 2012 et sa configuration pour servir dans le rôle de proxy rend cet ordinateur à un serveur proxy de fédération.  
  

