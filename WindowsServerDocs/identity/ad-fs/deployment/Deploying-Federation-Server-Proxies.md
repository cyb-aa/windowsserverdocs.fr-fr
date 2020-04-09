---
ms.assetid: 1b21b0a9-1fe6-4fd1-8a25-92e578d774ed
title: Déploiement de serveurs proxy de fédération
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: f348b7dbc9b786cfe401eb72b82592a51e1e6343
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855552"
---
# <a name="deploying-federation-server-proxies"></a>Déploiement de serveurs proxy de fédération

Pour déployer des serveurs proxys de Fédération dans Services ADFS \(AD FS\), effectuez chacune des tâches de la [liste de vérification : configuration d’un serveur proxy de Fédération](Checklist--Setting-Up-a-Federation-Server-Proxy.md).  
  
> [!NOTE]  
> Lorsque vous utilisez cette liste de vérification, nous vous recommandons de lire d’abord les références à l’aide de la planification du serveur proxy de Fédération dans le [Guide de conception AD FS de Windows server 2012](https://technet.microsoft.com/library/dd807036.aspx) avant de commencer les procédures de configuration des serveurs. La liste de vérification fournit une meilleure compréhension du processus de conception et de déploiement pour les serveurs proxys de Fédération.  
  
## <a name="about-federation-server-proxies"></a>À propos des serveurs proxys de Fédération  
Les serveurs proxys de Fédération sont des ordinateurs qui exécutent Windows Server&reg; 2012 et AD FS logiciel qui ont été configurés manuellement pour agir dans le rôle proxy. Vous pouvez utiliser des serveurs proxy de fédération dans votre organisation pour fournir des services intermédiaires entre un client Internet et un serveur de fédération qui se trouve derrière un pare-feu sur votre réseau d’entreprise.  
  
> [!NOTE]  
> Bien que les rôles serveur de Fédération et serveur proxy de Fédération ne peuvent pas être installés sur le même ordinateur, un serveur de Fédération peut exécuter les fonctions de serveur proxy de Fédération. Pour plus d'informations, voir [When to Create a Federation Server](https://technet.microsoft.com/library/dd807101.aspx).  
  
Le fait d’installer le logiciel AD FS sur un ordinateur Windows Server&reg; 2012 et de le configurer pour qu’il serve au rôle proxy permet à cet ordinateur d’être un serveur proxy de Fédération.  
  

