---
ms.assetid: 62708b2e-4090-4cf7-8ae6-a557f31f561f
title: Présentation du modèle logique Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0e909d4e9c1fb26aa0f7cb97a7dc06192db5cc21
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834270"
---
# <a name="understanding-the-active-directory-logical-model"></a>Présentation du modèle logique Active Directory

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Conception de votre structure logique pour les Services de domaine Active Directory (AD DS) implique définissent les relations entre les conteneurs dans votre répertoire. Ces relations peuvent être basées sur des exigences administratives, telles que de la délégation de l’autorité, ou ils peuvent être définis par les exigences opérationnelles, telles que la nécessité de contrôler la réplication.  
  
Avant de concevoir votre structure Active Directory logique, il est important de comprendre le modèle logique Active Directory. Les services AD DS est une base de données distribuée qui stocke et gère les informations sur les ressources réseau, ainsi que les données spécifiques à l’application à partir d’applications d’annuaire. Les services AD DS permet aux administrateurs d’organiser les éléments d’un réseau (par exemple, les utilisateurs, ordinateurs et périphériques) en une structure hiérarchique de relation contenant-contenu. Le conteneur de niveau supérieur est la forêt. Dans les forêts domaines et dans les domaines sont les unités d’organisation (UO). On parle le modèle logique, car elle est indépendante des aspects du déploiement, telles que le nombre de contrôleurs de domaine requis dans chaque domaine et topologie réseau physiques.  
  
## <a name="active-directory-forest"></a>Forêt Active Directory  
Une forêt est une collection d’un ou plusieurs domaines Active Directory qui partagent une structure logique commune, le schéma de répertoire (définitions de classes et d’attributs), la configuration de répertoire (informations de site et de réplication) et (forêt recherche de catalogue global fonctionnalités). Domaines dans la même forêt sont automatiquement liées par les relations d’approbation transitive bidirectionnelle.  
  
## <a name="active-directory-domain"></a>domaine Active Directory,  
Un domaine est une partition dans une forêt Active Directory. Partitionnement des données permet aux organisations de répliquer des données uniquement à là où cela est nécessaire. De cette façon, le répertoire peut mettre à l’échelle dans le monde entier sur un réseau disposant d’une bande passante limitée. En outre, le domaine prend en charge un nombre d’autres fonctions liées à l’administration, y compris :  
  
-   Identité de l’utilisateur de l’échelle du réseau. Domaines permettent aux identités d’utilisateur à créer une seule fois et référencé sur n’importe quel ordinateur joint à la forêt dans laquelle se trouve le domaine. Les contrôleurs de domaine qui composent un domaine sont utilisés pour stocker en toute sécurité des comptes d’utilisateur et les informations d’identification de l’utilisateur (par exemple, les mots de passe ou certificats).  
  
-   authentification. Contrôleurs de domaine fournissent des services d’authentification pour les utilisateurs et fournissent des données d’autorisation supplémentaires telles que les appartenances de groupe de l’utilisateur, qui peuvent être utilisés pour contrôler l’accès aux ressources sur le réseau.  
  
-   Relations d’approbation. Domaines peuvent étendre les services d’authentification aux utilisateurs dans des domaines en dehors de leur propre forêt au moyen d’approbations.  
  
-   Réplication. Le domaine définit une partition du répertoire qui contient des données suffisantes pour fournir des services de domaine et il réplique ensuite entre les contrôleurs de domaine. De cette façon, tous les contrôleurs de domaine sont des homologues dans un domaine et sont gérées en tant qu’unité.  
  
## <a name="active-directory-organizational-units"></a>Unités d’organisation Active Directory  
Unités d’organisation peuvent être utilisées pour former une hiérarchie de conteneurs dans un domaine. Unités d’organisation sont utilisés pour regrouper des objets pour des raisons administratives telles que l’application de stratégie de groupe ou de la délégation de l’autorité. Contrôle (sur une unité d’organisation et les objets qu’il contient) est déterminée par les listes de contrôle accès (ACL) sur l’unité d’organisation et sur les objets dans l’unité d’organisation. Pour faciliter la gestion d’un grand nombre d’objets, les services AD DS prend en charge le concept de délégation de l’autorité. Au moyen de la délégation, propriétaires peuvent transférer un contrôle administratif complet ou limité sur les objets à d’autres utilisateurs ou groupes. La délégation est importante car elle permet de distribuer la gestion d’un grand nombre d’objets sur un nombre de personnes qui sont chargées d’effectuer des tâches de gestion.  
  


