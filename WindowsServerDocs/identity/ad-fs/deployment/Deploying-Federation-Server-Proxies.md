---
ms.assetid: 1b21b0a9-1fe6-4fd1-8a25-92e578d774ed
title: Déploiement de serveurs proxy de fédération
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: c6af319283de72963691ae3e91c3db5992bdec72
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408409"
---
# <a name="deploying-federation-server-proxies"></a>Déploiement de serveurs proxy de fédération

Pour déployer des serveurs proxys de Fédération dans Services ADFS \(AD FS @ no__t-1, effectuez chacune des tâches dans [Checklist : Configuration d’un serveur proxy de Fédération @ no__t-0.  
  
> [!NOTE]  
> Lorsque vous utilisez cette liste de vérification, nous vous recommandons de lire d’abord les références à l’aide de la planification du serveur proxy de Fédération dans le [Guide de conception AD FS de Windows server 2012](https://technet.microsoft.com/library/dd807036.aspx) avant de commencer les procédures de configuration des serveurs. La liste de vérification fournit une meilleure compréhension du processus de conception et de déploiement pour les serveurs proxys de Fédération.  
  
## <a name="about-federation-server-proxies"></a>À propos des serveurs proxys de Fédération  
Les serveurs proxys de Fédération sont des ordinateurs qui exécutent Windows Server® 2012 et AD FS logiciel qui ont été configurés manuellement pour agir dans le rôle proxy. Vous pouvez utiliser des serveurs proxy de fédération dans votre organisation pour fournir des services intermédiaires entre un client Internet et un serveur de fédération qui se trouve derrière un pare-feu sur votre réseau d’entreprise.  
  
> [!NOTE]  
> Bien que les rôles serveur de Fédération et serveur proxy de Fédération ne peuvent pas être installés sur le même ordinateur, un serveur de Fédération peut exécuter les fonctions de serveur proxy de Fédération. Pour plus d'informations, voir [When to Create a Federation Server](https://technet.microsoft.com/library/dd807101.aspx).  
  
Le fait d’installer le logiciel AD FS sur un ordinateur Windows Server® 2012 et de le configurer pour qu’il serve au rôle proxy permet à cet ordinateur d’être un serveur proxy de Fédération.  
  

