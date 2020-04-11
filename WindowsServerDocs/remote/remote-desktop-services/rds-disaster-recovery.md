---
title: Protection de votre déploiement des services Bureau à distance - Récupération d’urgence
description: En savoir plus sur vos options de récupération d’urgence pour les Services Bureau à distance
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 9ff6a3b0-ea14-424e-9524-209252e9f1a8
author: lizap
ms.author: elizapo
ms.date: 06/12/2017
ms.openlocfilehash: 5c3257be5b3e9a53b8aa2400777f0519f8891843
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861292"
---
# <a name="configure-disaster-recovery-for-remote-desktop-services"></a>Configurer la récupération d’urgence pour les Services Bureau à distance

Lorsque vous déployez les Services Bureau à distance dans votre environnement, ils deviennent un élément essentiel de votre infrastructure, en particulier les applications et les ressources que vous partagez avec les utilisateurs. Si le déploiement des services Bureau à distance s’arrête en raison d’une défaillance du réseau ou d’une catastrophe naturelle, les utilisateurs ne peuvent pas accéder aux applications et aux ressources et votre entreprise s’en ressent. Pour éviter ce problème, vous pouvez configurer une solution de récupération d’urgence qui vous permet de basculer votre déploiement : si votre déploiement des services Bureau à distance n’est pas disponible pour une raison ou pour une autre, une sauvegarde est disponible afin de reprendre automatiquement.

Pour que votre déploiement des services Bureau à distance reste en activité en cas de panne d’un ordinateur ou d’un composant, nous vous recommandons de configurer votre déploiement des services Bureau à distance pour la haute disponibilité. Pour ce faire, vous pouvez configurer une [batterie de serveurs RDSH](rds-scale-rdsh-farm.md) et vous assurer que vos [services Broker de connexion sont mis en cluster pour la haute disponibilité](rds-connection-broker-cluster.md). 

Les solutions de récupération d’urgence que nous recommandons ici sont destinées à protéger votre déploiement des catastrophes, qui arrêtent tout votre déploiement des services Bureau à distance (y compris les rôles redondants configurés pour la haute disponibilité). Si un tel incident se produit, disposer d’une solution de récupération d’urgence intégrée à votre déploiement vous permet de basculer la totalité du déploiement et de remettre rapidement en fonctionnement les applications et les ressources pour vos utilisateurs.

Utilisez les informations suivantes pour déployer les solutions de récupération d’urgence dans les services Bureau à distance :

- [Exploitez plusieurs centres de données Azure pour garantir que les utilisateurs puissent accéder à votre déploiement des services Bureau à distance, même si un centre de données Azure tombe en panne (géo-redondance)](rds-multi-datacenter-deployment.md)
- [Déployez Azure Site Recovery pour assurer le basculement pour les composants des services Bureau à distance dans les basculements de site à site ou de site vers Azure](rds-disaster-recovery-with-azure.md)


