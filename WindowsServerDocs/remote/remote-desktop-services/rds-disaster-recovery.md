---
title: Protéger votre déploiement des services Bureau à distance - récupération d’urgence
description: En savoir plus sur vos options de récupération d’urgence pour les Services Bureau à distance
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9ff6a3b0-ea14-424e-9524-209252e9f1a8
author: lizap
ms.author: elizapo
ms.date: 06/12/2017
ms.openlocfilehash: a6eac3a50999633d15b1b6dc28608f60f6fef6c7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834080"
---
# <a name="configure-disaster-recovery-for-remote-desktop-services"></a>Configurer la récupération d’urgence pour les Services Bureau à distance

Lorsque vous déployez des Services Bureau à distance dans votre environnement, il devient un élément essentiel de votre infrastructure, en particulier les applications et les ressources que vous partagez avec des utilisateurs. Si le déploiement des services Bureau à distance s’arrête en raison de quoi que ce soit à partir d’une défaillance du réseau à une catastrophe naturelle, les utilisateurs ne peuvent pas accéder aux applications et aux ressources et votre entreprise s’en ressentent. Pour éviter ce problème, vous pouvez configurer une solution de récupération d’urgence qui vous permet de basculer votre déploiement - si votre déploiement des services Bureau à distance n’est pas disponible, pour une raison quelconque, il est une sauvegarde disponible pour reprendre automatiquement.

Pour conserver votre déploiement des services Bureau à distance en cours d’exécution dans le cas d’un ordinateur en panne ou un composant unique, nous vous recommandons de configuration de votre déploiement des services Bureau à distance pour la haute disponibilité. Ce faire, vous pouvez configurer un [batterie de serveurs RDSH](rds-scale-rdsh-farm.md) et garantir votre [agents de connexion sont en cluster pour la haute disponibilité](rds-connection-broker-cluster.md). 

Les solutions de récupération d’urgence qu'est recommandé ici sont à protéger votre déploiement à partir de la catastrophe - quelque chose qui retire tout votre déploiement de services Bureau à distance (y compris les rôles redondants configurés pour la haute disponibilité). Si un tel incident est atteint, disposer d’une solution de récupération d’urgence intégrée à votre déploiement vous pour basculer la totalité du déploiement et obtenir rapidement des applications et ressources et en cours d’exécution pour vos utilisateurs.

Utilisez les informations suivantes pour déployer des solutions de récupération d’urgence dans les services Bureau à distance :

- [Tirer parti de plusieurs centres de données Azure pour garantir les utilisateurs peuvent accéder à votre déploiement des services Bureau à distance, même si un centre de données Azure tombe en panne (géo-redondance)](rds-multi-datacenter-deployment.md)
- [Déployer Azure Site Recovery pour assurer le basculement pour les composants des services Bureau à distance dans les basculements de site à site ou site vers Azure](rds-disaster-recovery-with-azure.md)


