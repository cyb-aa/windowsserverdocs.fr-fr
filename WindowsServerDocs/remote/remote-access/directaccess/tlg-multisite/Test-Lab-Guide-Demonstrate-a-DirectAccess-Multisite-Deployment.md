---
title: Guide de laboratoire de test-illustrer un déploiement multisite DirectAccess
description: 'Cette rubrique fait partie du Guide de laboratoire de test : illustrer un déploiement multisite DirectAccess pour Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3c98106c-67cc-406a-810e-f2e09f7e2c5e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bb4d69f914331e8fa7a06c6dd77ea342d5eefc6a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388219"
---
# <a name="test-lab-guide-demonstrate-a-directaccess-multisite-deployment"></a>Guide du laboratoire de test : Illustrer un déploiement multisite DirectAccess

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

L’accès à distance est un rôle serveur dans les systèmes d’exploitation Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012 qui permet aux utilisateurs distants d’accéder en toute sécurité aux ressources réseau internes à l’aide de DirectAccess ou du VPN RRAS. Ce guide contient des instructions pas à pas pour l’extension du Guide de laboratoire [Test : Démonstration de la configuration d’un seul serveur DirectAccess avec Mixed IPv4 et IPv6 @ no__t-0 pour illustrer l’accès à distance dans un scénario multisite.  
  
Le déploiement de l’accès à distance dans un scénario multisite vous permet de configurer des serveurs d’accès à distance dans des emplacements géographiquement différents. Auparavant, les utilisateurs distants devaient toujours se connecter au réseau d’entreprise via un serveur DirectAccess particulier. Avec Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 et Windows 10 ou Windows 8, vous pouvez configurer des points d’entrée pour chaque emplacement géographique de votre déploiement. Chaque point d’entrée peut être un serveur d’accès à distance unique ou un cluster de serveurs d’accès à distance. Les utilisateurs distants ont la possibilité de se connecter à n’importe quel point d’entrée d’accès à distance de l’organisation. Par exemple, si un utilisateur distant se connecte généralement au point d’entrée d’accès à distance situé en Asie, mais qu’il passe ensuite à l’Europe, l’ordinateur client se connecte automatiquement au point d’entrée d’accès distant le plus proche.  
  
## <a name="about-this-guide"></a>À propos de ce guide  
Ce guide contient des instructions pour la configuration et la démonstration de l’accès à distance à l’aide de neuf serveurs et de trois ordinateurs clients. Le laboratoire de test multisite d’accès à distance terminé simule un intranet, Internet et un réseau privé, et illustre les fonctionnalités d’accès à distance dans différents scénarios de connexion Internet.  
  
> [!IMPORTANT]  
> Pour appuyer cette démonstration, il utilise un nombre minimum d’ordinateurs. La configuration détaillée dans ce guide est fournie à des fins de tests en laboratoire seulement et ne doit pas être utilisée dans un environnement de production.  
  


