---
ms.assetid: 62708b2e-4090-4cf7-8ae6-a557f31f561f
title: "Présentation du modèle logique ActiveDirectory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0f8cdc4789d1b3008f3b53104e5517d4ef1e65b9
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="understanding-the-active-directory-logical-model"></a>Présentation du modèle logique ActiveDirectory

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Conception de votre structure logique pour les Services de domaine ActiveDirectory (ADDS) implique la définition les relations entre les conteneurs dans votre répertoire. Ces relations peuvent être basées sur les exigences d’administration, par exemple, la délégation de l’autorité, ou ils peuvent être définis par des exigences opérationnelles, comme la nécessité pour contrôler la réplication.  
  
Avant de concevoir votre structure logique ActiveDirectory, il est important de comprendre le modèle logique ActiveDirectory. Les services ADDS est une base de données distribuée qui stocke et gère des informations sur les ressources réseau, ainsi que des données spécifiques à l’application à partir d’applications d’annuaire. Les services ADDS permet aux administrateurs organiser les éléments d’un réseau (par exemple, les utilisateurs, ordinateurs et périphériques) en une structure hiérarchique de type contenant-contenu. Le conteneur de niveau supérieur est la forêt. Dans les forêts domaines et dans les domaines sont des unités d’organisation (UO). Il s’agit du modèle logique, car il est indépendant des aspects du déploiement, comme le nombre de contrôleurs de domaine requis dans chaque domaine et topologie réseau physiques.  
  
## <a name="active-directory-forest"></a>Forêt Active Directory  
Une forêt est une collection d’un ou plusieurs domaines ActiveDirectory qui partagent une structure commune logique, le schéma Active (définitions de classes et attributs), la configuration active (informations de site et de réplication) et le catalogue global (des fonctionnalités de recherche de la forêt). Les domaines dans la même forêt sont automatiquement liés à relations d’approbation transitive bidirectionnelle.  
  
## <a name="active-directory-domain"></a>Domaine Active Directory  
Un domaine est une partition dans une forêt ActiveDirectory. Partitionnement des données permet aux organisations de répliquer des données uniquement aux là où cela est nécessaire. De cette façon, le répertoire peut adapter de manière globale sur un réseau disposant d’une bande passante limitée. En outre, le domaine prend en charge un nombre d’autres fonctions liées à l’administration, y compris:  
  
-   Identité de l’utilisateur à l’échelle du réseau. Domaines autorisent les identités des utilisateurs à créer une seule fois et référencés sur n’importe quel ordinateur joint à la forêt dans laquelle se trouve le domaine. Les contrôleurs de domaine qui constituent un domaine sont utilisés pour stocker les informations d’identification utilisateur (par exemple, les mots de passe ou des certificats) et les comptes d’utilisateur en toute sécurité.  
  
-   Authentification. Contrôleurs de domaine fournissent des services d’authentification pour les utilisateurs et fournissant des données d’autorisation supplémentaires tels que les appartenances de groupe utilisateur, qui peuvent être utilisés pour contrôler l’accès aux ressources sur le réseau.  
  
-   Relations d’approbation. Domaines peuvent étendre les services d’authentification pour les utilisateurs dans les domaines situés en dehors de leur propre forêt au moyen d’approbations.  
  
-   Réplication. Le domaine définit une partition du répertoire qui contient des données suffisantes pour fournir des services de domaine et il sont ensuite répliquées entre les contrôleurs de domaine. De cette façon, tous les contrôleurs de domaine sont des homologues dans un domaine et gérés comme une unité.  
  
## <a name="active-directory-organizational-units"></a>Unités d’organisation ActiveDirectory  
Unités d’organisation peuvent servir à forment une hiérarchie de conteneurs dans un domaine. Unités d’organisation sont utilisés pour regrouper des objets tels que l’application de stratégie de groupe ou de la délégation de l’autorité à des fins d’administration. Contrôle (sur une unité d’organisation et les objets au sein de celle-ci) est déterminée par les listes de contrôle accès (ACL) sur l’unité d’organisation et sur les objets dans l’unité d’organisation. Pour faciliter la gestion d’un grand nombre d’objets, les services ADDS prend en charge le concept de délégation de l’autorité. Au moyen de la délégation, propriétaires peuvent transférer un contrôle administratif complet ou limité sur les objets à d’autres utilisateurs ou groupes. La délégation est importante car elle permet de distribuer la gestion d’un grand nombre d’objets sur un certain nombre de personnes chargées d’effectuer des tâches de gestion.  
  


