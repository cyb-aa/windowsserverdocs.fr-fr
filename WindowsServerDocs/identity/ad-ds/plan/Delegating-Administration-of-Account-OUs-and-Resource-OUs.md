---
ms.assetid: 19feca0e-a6d0-4d27-93b0-cb44f8c26484
title: Délégation de l’administration des unités d’organisation de comptes et de ressources
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 70900b44de85774e8e595f9691885d67682fa753
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834260"
---
# <a name="delegating-administration-of-account-ous-and-resource-ous"></a>Délégation de l’administration des unités d’organisation de comptes et de ressources

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Unités de compte d’organisation (UO) contiennent des objets utilisateur, groupe et ordinateur. Unités d’organisation de ressources contiennent des ressources et les comptes qui sont chargés de gérer ces ressources. Le propriétaire de la forêt est chargé pour la création d’une structure d’unité d’organisation pour gérer ces objets et les ressources et de délégation du contrôle de cette structure pour le propriétaire de l’unité d’organisation.  
  
## <a name="delegating-administration-of-account-ous"></a>Délégation de l’administration des unités d’organisation de compte  
Déléguer un structure d’unité d’organisation de compte aux administrateurs de données s’ils ont besoin créer et modifier des objets utilisateur, groupe et ordinateur. La structure d’unité d’organisation de compte est une sous-arborescence d’unités d’organisation pour chaque type de compte qui doit être contrôlé de manière indépendante. Par exemple, le propriétaire de l’unité d’organisation peut déléguer contrôle spécifique aux administrateurs de données différentes unités d’organisation enfants dans un unité d’organisation de compte pour les utilisateurs, les ordinateurs, les groupes et comptes de service.  
  
L’illustration suivante montre un exemple d’un structure d’unité d’organisation de compte.  
  
![délégation de l’administration](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/66d38fbe-e8eb-42d7-abab-9526243bf6d9.gif)  
  
Le tableau suivant répertorie et décrit les unités d’organisation enfants possibles que vous pouvez créer dans une structure d’unité d’organisation de compte.  
  
|OU|Objectif|  
|------|-----------|  
|Utilisateurs|Contient les comptes d’utilisateur pour le personnel non administratives.|  
|Comptes de service|Certains services qui requièrent l’accès aux ressources réseau s’exécutent en tant que comptes d’utilisateur. Cette unité d’organisation est créée pour les comptes d’utilisateur de service distinct à partir des comptes utilisateur contenus dans l’unité d’organisation utilisateurs. Placer les différents types de comptes d’utilisateur dans les unités d’organisation distinctes vous permet également de les gérer en fonction de leurs besoins d’administration.|  
|Computers|Contient les comptes des ordinateurs autres que les contrôleurs de domaine.|  
|Groups|Contient des groupes de tous les types à l’exception des groupes d’administration, qui sont gérées séparément.|  
|Admins|Contient les comptes d’utilisateur et groupe pour les administrateurs de données dans la structure d’unité d’organisation de compte pour leur permettre d’être gérées séparément des utilisateurs standard. Activer l’audit pour cette unité d’organisation afin que vous pouvez suivre les modifications apportées aux groupes et utilisateurs administratifs.|  
  
L’illustration suivante montre un exemple d’une conception de groupe d’administration pour une structure d’unité d’organisation de compte.  
  
![délégation de l’administration](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/be2cd2d2-6956-429c-a53a-369e6fe40b2b.gif)  
  
Groupes qui gèrent les unités d’organisation enfants sont accordées à un contrôle total uniquement sur la classe spécifique d’objets qu’ils sont responsables de la gestion.  
  
Les types de groupes que vous utilisez pour déléguer le contrôle au sein d’une structure d’unité d’organisation sont basées sur où se trouvent les comptes par rapport à la structure d’unité d’organisation qui doit être géré. Si les comptes d’utilisateur administrateur et la structure d’unité d’organisation de toutes les existent dans un seul domaine, les groupes que vous créez à utiliser pour la délégation doivent être des groupes globaux. Si votre organisation dispose d’un service qui gère ses propres comptes d’utilisateur et qui existe dans plusieurs régions géographiques, vous pouvez avoir un groupe d’administrateurs de données qui sont responsables de la gestion des unités d’organisation dans plusieurs domaines de compte. Si les comptes de tous les administrateurs de données existent dans un seul domaine et que vous avez des structures de l’unité d’organisation dans plusieurs domaines sur lesquels vous devez déléguer le contrôle, rendre ces membres de comptes d’administration des groupes globaux et déléguer le contrôle des structures dans chaque unité d’organisation domaine à ces groupes globaux. Si les comptes d’administrateurs de données à laquelle vous déléguer le contrôle d’une structure d’unité d’organisation proviennent de plusieurs domaines, vous devez utiliser un groupe universel. Les groupes universels peuvent contenir des utilisateurs de domaines différents, et par conséquent, ils peuvent être utilisés pour déléguer le contrôle dans plusieurs domaines.  
  
## <a name="delegating-administration-of-resource-ous"></a>Délégation de l’administration des unités d’organisation de ressource  
Unités d’organisation de ressource sont utilisées pour gérer l’accès aux ressources. Le propriétaire d’unité d’organisation de ressources crée des comptes d’ordinateur pour les serveurs qui sont joints au domaine qui incluent des ressources telles que les partages de fichiers, les bases de données et les imprimantes. Le propriétaire d’unité d’organisation de ressources crée également des groupes pour contrôler l’accès à ces ressources.  
  
L’illustration suivante montre les deux emplacements possibles pour l’unité d’organisation de ressource.  
  
![délégation de l’administration](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/6667a5ce-34d6-48a9-9974-b823ba70e2af.gif)  
  
La ressource unité d’organisation peut être située sous la racine du domaine ou en tant qu’enfant d’unités d’organisation de l’unité d’organisation de compte correspondant dans la hiérarchie d’administration d’unité d’organisation. Unités d’organisation de ressources n’ont pas les unités d’organisation enfant standard. Ordinateurs et les groupes sont placés directement dans l’unité d’organisation de ressource.  
  
Le propriétaire d’unité d’organisation de ressource possède les objets au sein de l’unité d’organisation, mais ne possède pas le conteneur d’unité d’organisation elle-même. Gérer les propriétaires d’unité d’organisation de ressources uniquement objets ordinateur et groupe ; Impossible de créer des autres classes d’objets au sein de l’unité d’organisation, et ils ne peuvent pas créer des unités d’organisation enfants.  
  
> [!NOTE]  
> Le créateur ou le propriétaire d’un objet a la possibilité de définir la liste de contrôle d’accès (ACL) sur l’objet indépendamment des autorisations qui sont héritées à partir du conteneur parent. Si le propriétaire d’une unité d’organisation de ressource peut réinitialiser l’ACL sur une unité d’organisation, ce propriétaire peut créer n’importe quelle classe d’objet dans l’unité d’organisation, y compris les utilisateurs. Pour cette raison, les propriétaires d’unité d’organisation de ressources ne sont pas autorisés à créer des unités d’organisation.  
  
Pour chaque unité d’organisation dans le domaine de la ressource, créez un groupe global pour représenter les administrateurs de données qui sont responsables de la gestion du contenu de l’unité d’organisation. Ce groupe dispose d’un contrôle total sur les objets de groupe et d’ordinateur dans l’unité d’organisation, mais pas sur le conteneur d’unité d’organisation elle-même.  
  
L’illustration suivante montre la conception de groupe d’administration pour une unité d’organisation de ressource.  
  
![délégation de l’administration](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/8a3f7714-a3bf-43f7-b999-6070543248b0.gif)  
  
Placer les comptes d’ordinateur dans une unité d’organisation de ressource donne le contrôle de propriétaire d’unité d’organisation sur les objets de compte, mais le propriétaire de l’unité d’organisation ne fait pas un administrateur des ordinateurs. Dans un domaine Active Directory, le groupe Admins du domaine est, par défaut, placé dans le groupe Administrateurs local sur tous les ordinateurs. Autrement dit, les administrateurs de service possèdent un contrôle sur ces ordinateurs. Si les propriétaires d’unité d’organisation de ressources nécessitent un contrôle administratif sur les ordinateurs dans leurs unités d’organisation, le propriétaire de la forêt peut appliquer une stratégie de groupe groupes restreints pour rendre le propriétaire d’unité d’organisation de ressource un membre du groupe Administrateurs sur les ordinateurs de cette unité d’organisation.  
  


