---
title: 'Guide de laboratoire de test : décrire un déploiement Multisite DirectAccess'
description: 'Cette rubrique fait partie du Guide de laboratoire de Test : illustrer un déploiement Multisite DirectAccess pour Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3c98106c-67cc-406a-810e-f2e09f7e2c5e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 87c1f629d96d247d273d48066797795b9fe724e6
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281379"
---
# <a name="test-lab-guide-demonstrate-a-directaccess-multisite-deployment"></a>Guide du laboratoire de test : Décrire un déploiement Multisite DirectAccess

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Accès à distance est un rôle de serveur dans les systèmes d’exploitation Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012 qui permet aux utilisateurs distants un accès sécurisé aux ressources réseau internes à l’aide de DirectAccess ou VPN RRAS. Ce guide contient des instructions pas à pas permettant d’élargir le [Guide de laboratoire de Test : Démontrer DirectAccess configuration d’un serveur dans un environnement mixte IPv4 et IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004) afin d’illustrer l’accès à distance dans un scénario multisite.  
  
Déploiement de l’accès à distance dans un scénario multisite vous permet de configurer des serveurs d’accès à distance dans différents emplacements géographiques. Auparavant, les utilisateurs distants devaient se connecte toujours au réseau d’entreprise via un serveur DirectAccess particulier. Avec Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 et Windows 10 ou Windows 8, vous pouvez configurer des points d’entrée pour chaque emplacement géographique dans votre déploiement. Chaque point d’entrée peut être un seul serveur d’accès à distance ou d’un cluster de serveurs d’accès à distance. Les utilisateurs distants ont l’option se connecter à aucun des points d’entrée de l’accès à distance de l’organisation. Par exemple, si un utilisateur distant se connecte généralement au point d’entrée de l’accès à distance situé en Asie, mais passe ensuite en voyage d’affaires en Europe, l’ordinateur client se connecte automatiquement au point d’entrée d’accès à distance le plus proche.  
  
## <a name="about-this-guide"></a>À propos de ce guide  
Ce guide contient des instructions pour la configuration et de démontrer l’accès à distance à l’aide de neuf serveurs et les trois ordinateurs clients. Le laboratoire de test multisite de l’accès à distance terminé simule un intranet, Internet et un réseau domestique et illustre la fonctionnalité d’accès à distance dans différents scénarios de connexion Internet.  
  
> [!IMPORTANT]  
> Pour appuyer cette démonstration, il utilise un nombre minimum d’ordinateurs. La configuration détaillée dans ce guide est fournie à des fins de tests en laboratoire seulement et ne doit pas être utilisée dans un environnement de production.  
  


