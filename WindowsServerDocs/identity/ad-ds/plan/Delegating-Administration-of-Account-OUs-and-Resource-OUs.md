---
ms.assetid: 19feca0e-a6d0-4d27-93b0-cb44f8c26484
title: "Déléguer l’Administration des unités d’organisation de compte et de ressources"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 56e69bffdf8ecc0f37f9f5159ae6bce7fcdc49f8
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="delegating-administration-of-account-ous-and-resource-ous"></a>Déléguer l’Administration des unités d’organisation de compte et de ressources

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Unités d’organisation de compte (ou) contiennent des objets utilisateur, groupe et d’ordinateur. Unités d’organisation ressources contiennent des ressources et les comptes qui sont chargés de gérer ces ressources. Le propriétaire de la forêt est responsable de la création d’une structure d’unité d’organisation pour gérer ces objets et les ressources et pour déléguer le contrôle de cette structure au propriétaire de l’unité d’organisation.  
  
## <a name="delegating-administration-of-account-ous"></a>Déléguer l’administration des unités d’organisation de compte  
Déléguer un structure d’unité d’organisation de compte aux administrateurs de données s’ils ont besoin créer et modifier des objets utilisateur, groupe et d’ordinateur. Le compte de structure d’unité d’organisation est une sous-arborescence d’unités d’organisation pour chaque type de compte qui doit être contrôlé de manière indépendante. Par exemple, le propriétaire de l’unité d’organisation permettre déléguer contrôle spécifique aux administrateurs de données différentes unités d’organisation enfants dans un compte d’unité d’organisation pour les utilisateurs, les ordinateurs, groupes et comptes de service.  
  
L’illustration suivante montre un exemple de structure d’unité d’organisation.  
  
![délégation de l’administration](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/66d38fbe-e8eb-42d7-abab-9526243bf6d9.gif)  
  
Le tableau suivant répertorie et décrit les unités d’organisation enfants possibles que vous pouvez créer dans un structure d’unité d’organisation de compte.  
  
|UNITÉ D’ORGANISATION|Objectif|  
|------|-----------|  
|Utilisateurs|Contient des comptes d’utilisateur pour le personnel non administratives.|  
|Comptes de service|Certains services qui nécessitent un accès aux ressources réseau s’exécuter en tant que comptes d’utilisateur. Cette unité d’organisation est créée pour les comptes d’utilisateur de service distinct à partir des comptes d’utilisateur contenus dans l’unité d’organisation utilisateurs. En plaçant les différents types de comptes d’utilisateurs dans les unités d’organisation distinctes vous permet également de les gérer en fonction de leurs besoins d’administration.|  
|Ordinateurs|Contient des comptes pour les ordinateurs autres que des contrôleurs de domaine.|  
|Groupes|Contient des groupes de tous les types à l’exception des groupes d’administration, qui sont gérés séparément.|  
|Administrateurs|Contient des comptes d’utilisateur et groupe pour les administrateurs de données dans la structure d’unité d’organisation de compte pouvoir être gérés séparément à partir des utilisateurs standard. Activer l’audit pour cette unité d’organisation afin que vous pouvez suivre les modifications apportées aux groupes et utilisateurs administratifs.|  
  
L’illustration suivante montre un exemple d’une conception de groupe d’administration pour un compte de structure d’unité d’organisation.  
  
![délégation de l’administration](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/be2cd2d2-6956-429c-a53a-369e6fe40b2b.gif)  
  
Groupes qui gèrent les unités d’organisation enfants autorisation contrôle total sur la classe d’objets qu’ils sont responsables de la gestion spécifique uniquement.  
  
Les types de groupes que vous utilisez pour déléguer le contrôle au sein d’une structure d’unité d’organisation sont basées sur l’emplacement où se trouvent les comptes par rapport à la structure d’unité d’organisation qui doit être géré. Si tous les comptes d’utilisateur administrateur et la structure d’unité d’organisation existent au sein d’un domaine unique, les groupes que vous créez pour l’utiliser pour la délégation doivent être groupes globaux. Si votre organisation dispose d’un service qui gère ses propres comptes d’utilisateur et existe dans plusieurs régions géographiques, vous pouvez avoir un groupe d’administrateurs de données qui sont chargés de la gestion des unités d’organisation dans plusieurs domaines de compte. Si les comptes de tous les administrateurs de données existent dans un seul domaine et que vous disposez des structures de l’unité d’organisation dans plusieurs domaines, auquel vous avez besoin déléguer le contrôle, que les membres de ces comptes d’administration des groupes globaux et déléguer le contrôle des structures de l’unité d’organisation dans chaque domaine pour ces groupes globaux. Si les comptes d’administrateurs de données à laquelle vous déléguez le contrôle d’une structure d’unité d’organisation issus de plusieurs domaines, vous devez utiliser un groupe universel. Groupes universels peuvent contenir des utilisateurs à des domaines différents, et par conséquent, ils peuvent être utilisés pour déléguer le contrôle dans plusieurs domaines.  
  
## <a name="delegating-administration-of-resource-ous"></a>Déléguer l’administration des unités d’organisation de ressource  
Unités d’organisation de ressource sont utilisées pour gérer l’accès aux ressources. Le propriétaire d’unité d’organisation de ressource crée des comptes d’ordinateur pour les serveurs qui sont joints au domaine qui incluent des ressources telles que les partages de fichiers, les bases de données et les imprimantes. Le propriétaire d’unité d’organisation de ressource crée également des groupes pour contrôler l’accès à ces ressources.  
  
L’illustration suivante montre les deux emplacements possibles pour l’unité d’organisation de ressource.  
  
![délégation de l’administration](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/6667a5ce-34d6-48a9-9974-b823ba70e2af.gif)  
  
La ressource unité d’organisation peut se trouve sous la racine du domaine ou une unité d’organisation du compte correspondant unité d’organisation dans la hiérarchie d’administration d’unité d’organisation enfant. Unités d’organisation de ressources n’ont pas les unités d’organisation enfant standard. Les ordinateurs et les groupes sont placés directement dans l’unité d’organisation de ressource.  
  
Le propriétaire d’unité d’organisation de ressource possède les objets au sein de l’unité d’organisation, mais ne possède pas le conteneur d’unité d’organisation proprement dit. Gérer les propriétaires d’unité d’organisation de ressource uniquement des ordinateurs et des objets de groupe; ils ne peut pas créer d’autres classes d’objets au sein de l’unité d’organisation, et ils ne peut pas créer des unités d’organisation enfant.  
  
> [!NOTE]  
> Créateur ou propriétaire d’un objet a la possibilité de définir la liste de contrôle d’accès (ACL) sur l’objet, indépendamment des autorisations qui sont hérités du conteneur parent. Si le propriétaire d’une unité d’organisation de ressource peut réinitialiser l’ACL sur une unité d’organisation, ce propriétaire peut créer une classe d’objets dans l’unité d’organisation, y compris les utilisateurs. Pour cette raison, les propriétaires des ressources d’unité d’organisation ne sont pas autorisés à créer des unités d’organisation.  
  
Pour chaque unité d’organisation dans le domaine de la ressource, créez un groupe global pour représenter les administrateurs de données qui sont chargés de gérer le contenu de l’unité d’organisation. Ce groupe possède le contrôle total sur les objets de groupe et d’ordinateur dans l’unité d’organisation, mais pas sur le conteneur d’unité d’organisation proprement dit.  
  
L’illustration suivante montre la conception de groupe d’administration pour une unité d’organisation de ressource.  
  
![délégation de l’administration](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/8a3f7714-a3bf-43f7-b999-6070543248b0.gif)  
  
Placer les comptes d’ordinateur dans une unité d’organisation de ressource donne à l’unité d’organisation propriétaire de contrôler les objets de compte, mais le propriétaire de l’unité d’organisation n’effectue pas un administrateur des ordinateurs. Dans un domaine Active Directory, le groupe Admins du domaine est, par défaut, placé dans le groupe Administrateurs local sur tous les ordinateurs. Autrement dit, les administrateurs de service possèdent un contrôle sur ces ordinateurs. Si les propriétaires des ressources d’unité d’organisation requièrent un contrôle administratif sur les ordinateurs dans leurs unités d’organisation, le propriétaire de la forêt peut appliquer une stratégie de groupe groupes restreints pour rendre le propriétaire d’unité d’organisation de ressource un membre du groupe Administrateurs sur les ordinateurs de cette unité d’organisation.  
  


