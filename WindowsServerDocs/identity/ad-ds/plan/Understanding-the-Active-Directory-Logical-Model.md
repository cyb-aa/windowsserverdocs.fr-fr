---
ms.assetid: 62708b2e-4090-4cf7-8ae6-a557f31f561f
title: Présentation du modèle logique Active Directory
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: cf2b997d601d42a47282df0ed95382e471233ff6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80821612"
---
# <a name="understanding-the-active-directory-logical-model"></a>Présentation du modèle logique Active Directory

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La conception de votre structure logique pour Active Directory Domain Services (AD DS) implique la définition des relations entre les conteneurs dans votre répertoire. Ces relations peuvent reposer sur des exigences administratives, telles que la délégation d’autorité, ou elles peuvent être définies par des exigences opérationnelles, telles que la nécessité de contrôler la réplication.  
  
Avant de concevoir votre structure logique Active Directory, il est important de comprendre le modèle logique Active Directory. AD DS est une base de données distribuée qui stocke et gère des informations sur les ressources réseau, ainsi que des données spécifiques à l’application à partir d’applications compatibles avec l’annuaire. AD DS permet aux administrateurs d’organiser les éléments d’un réseau (tels que les utilisateurs, les ordinateurs et les périphériques) en une structure hiérarchique de type contenant-contenu. Le conteneur de niveau supérieur est la forêt. Dans, les forêts sont des domaines et les domaines sont des unités d’organisation (UO). C’est ce que l’on appelle le modèle logique, car il est indépendant des aspects physiques du déploiement, tels que le nombre de contrôleurs de domaine requis au sein de chaque topologie de domaine et de réseau.  
  
## <a name="active-directory-forest"></a>Forêt Active Directory  
Une forêt est un regroupement d’un ou de plusieurs domaines Active Directory qui partagent une structure logique commune, un schéma d’annuaire (définitions de classes et d’attributs), une configuration d’annuaire (informations de site et de réplication) et un catalogue global (fonctionnalités de recherche à l’échelle de la forêt). Les domaines de la même forêt sont automatiquement liés avec des relations d’approbation transitive bidirectionnelle.  
  
## <a name="active-directory-domain"></a>Domaine Active Directory  
Un domaine est une partition dans une forêt Active Directory. Le partitionnement des données permet aux organisations de répliquer les données uniquement à l’endroit où elles sont nécessaires. De cette façon, le répertoire peut être mis à l’échelle globalement sur un réseau dont la bande passante est limitée. En outre, le domaine prend en charge un certain nombre d’autres fonctions principales liées à l’administration, notamment :  
  
-   Identité de l’utilisateur au niveau du réseau. Les domaines permettent de créer des identités d’utilisateur une seule fois et de les référencer sur tout ordinateur joint à la forêt dans laquelle se trouve le domaine. Les contrôleurs de domaine qui composent un domaine sont utilisés pour stocker les comptes d’utilisateur et les informations d’identification de l’utilisateur (tels que les mots de passe ou les certificats) en toute sécurité.  
  
-   Authentification. Les contrôleurs de domaine fournissent des services d’authentification pour les utilisateurs et fournissent des données d’autorisation supplémentaires, telles que des appartenances aux groupes d’utilisateurs, qui peuvent être utilisées pour contrôler l’accès aux ressources sur le réseau.  
  
-   Relations d’approbation. Les domaines peuvent étendre les services d’authentification aux utilisateurs dans des domaines situés en dehors de leur propre forêt par le biais d’approbations.  
  
-   Réplication. Le domaine définit une partition de l’annuaire qui contient les données suffisantes pour fournir des services de domaine, puis les réplique entre les contrôleurs de domaine. De cette façon, tous les contrôleurs de domaine sont des homologues d’un domaine et sont gérés en tant qu’unité.  
  
## <a name="active-directory-organizational-units"></a>Active Directory les unités d’organisation  
Les unités d’organisation peuvent être utilisées pour former une hiérarchie de conteneurs au sein d’un domaine. Les unités d’organisation sont utilisées pour regrouper des objets à des fins administratives, telles que l’application d’stratégie de groupe ou la délégation d’autorité. Le contrôle (sur une unité d’organisation et les objets qu’il contient) est déterminé par les listes de contrôle d’accès (ACL) sur l’unité d’organisation et sur les objets de l’unité d’organisation. Pour faciliter la gestion d’un grand nombre d’objets, AD DS prend en charge le concept de délégation d’autorité. Au moyen de la délégation, les propriétaires peuvent transférer un contrôle administratif complet ou limité sur des objets à d’autres utilisateurs ou groupes. La délégation est importante car elle permet de distribuer la gestion d’un grand nombre d’objets sur un certain nombre de personnes approuvées pour effectuer des tâches de gestion.  
  


