---
title: Optimisation des performances des serveurs Active Directory
description: Optimisation des performances des serveurs Active Directory
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 04c9683c3d14291d5dc2682c6836657313865866
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59891940"
---
# <a name="performance-tuning-active-directory-servers"></a>Optimisation des performances des serveurs Active Directory

L’optimisation des performances des serveurs Active Directory se concentre sur deux objectifs :
- Active Directory est configuré de manière optimale pour traiter la charge de la manière la plus efficace possible
- Les charges de travail soumises à Active Directory doivent être aussi efficaces que possible

Cela nécessite une attention particulière dans trois domaines distincts :
- Planification appropriée de la capacité - s'assurer que suffisamment de matériel est en place pour prendre en charge la charge existante
- Optimisation côté serveur - configuration des contrôleurs de domaine pour gérer la charge aussi efficacement que possible
- Optimisation Active Directory client/application - pour que les clients et les applications utilisent Active Directory de manière optimale

## <a name="start-with-capacity-planning"></a>Commencez avec la planification de la capacité
Il est essentiel de déployer correctement un nombre suffisant de contrôleurs de domaine, dans le bon domaine, dans les bons environnements locaux et de permettre la redondance afin de répondre rapidement aux demandes des clients. C’est un sujet qui peut être discuté de façon approfondie et qui sort du cadre de ce guide. Les lecteurs sont encouragés à commencer à optimiser leurs performances Active Directory en lisant et en comprenant les recommandations et les conseils abordés dans la section [Planification de la capacité pour les services de domaine Active Directory](https://go.microsoft.com/fwlink/?LinkId=324566).

>[!Important]
> Une configuration et un dimensionnement corrects de Active Directory ont un impact potentiel significatif sur les performances du système global et de la charge de travail. Les lecteurs sont vivement encouragés à commencer par lire la section [Planification de la capacité pour les services de domaine Active Directory](https://go.microsoft.com/fwlink/?LinkId=324566).

## <a name="updates-and-evolving-recommendations"></a>Mises à jour et recommandations en constante évolution

Des améliorations significatives dans les optimisations Active Directory et des performances client ont eu lieu au cours des dernières générations du système d'exploitation et ces efforts se poursuivent. Nous recommandons de déployer les versions les plus récentes de la plate-forme pour en tirer les avantages suivants :

- Fiabilité accrue
- Meilleures performances
- Meilleure journalisation et outils de dépannage

Cependant, nous sommes conscients que cela prend du temps et que de nombreux environnements sont déployés dans un scénario où il est impossible d’adopter à 100 % la plate-forme la plus récente. Certaines améliorations ont été ajoutées aux anciennes versions de la plate-forme et nous continuerons d’en ajouter de nouvelles.

Nous vous encourageons à rester au fait des dernières actualités, conseils et meilleures pratiques en matière de gestion d'AD DS en suivant le blog de notre équipe, [« Demander à l’équipe des services d’annuaire »](https://blogs.technet.microsoft.com/askds).

## <a name="see-also"></a>Voir également
- [Considérations matérielles](hardware-considerations.md)
- [Considérations relatives au protocole LDAP](ldap-considerations.md)
- [Positionnement correct des contrôleurs de domaine et considérations relatives au site](site-definition-considerations.md)
- [Résoudre les problèmes de performances d’AD DS](troubleshoot.md) 
- [Planification de la capacité pour les services de domaine Active Directory](https://go.microsoft.com/fwlink/?LinkId=324566)