---
ms.assetid: 19feca0e-a6d0-4d27-93b0-cb44f8c26484
title: Délégation de l’administration des unités d’organisation de comptes et de ressources
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 3043aaf79b2c0894fffe2f896a235ad519222e05
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822742"
---
# <a name="delegating-administration-of-account-ous-and-resource-ous"></a>Délégation de l’administration des unités d’organisation de comptes et de ressources

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Les unités d’organisation (UO) de compte contiennent des objets utilisateur, groupe et ordinateur. Les unités d’organisation de ressource contiennent des ressources et les comptes qui sont responsables de la gestion de ces ressources. Le propriétaire de la forêt est chargé de créer une structure d’unité d’organisation pour gérer ces objets et ressources et pour déléguer le contrôle de cette structure au propriétaire de l’unité d’organisation.  
  
## <a name="delegating-administration-of-account-ous"></a>Délégation de l’administration des unités d’organisation de compte  
Déléguez une structure d’unité d’organisation de compte aux administrateurs de données s’ils ont besoin de créer et de modifier des objets d’utilisateur, de groupe et d’ordinateur. La structure d’unité d’organisation de compte est une sous-arborescence d’unités d’organisation pour chaque type de compte qui doit être contrôlé indépendamment. Par exemple, le propriétaire de l’unité d’organisation peut déléguer un contrôle spécifique à différents administrateurs de données sur les unités d’organisation enfants d’une unité d’organisation de compte pour les utilisateurs, les ordinateurs, les groupes et les comptes de service.  
  
L’illustration suivante montre un exemple de structure d’unité d’organisation de compte.  
  
![délégation de l’administration](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/66d38fbe-e8eb-42d7-abab-9526243bf6d9.gif)  
  
Le tableau suivant répertorie et décrit les UO enfants possibles que vous pouvez créer dans une structure d’unité d’organisation de compte.  
  
|OU|Objectif|  
|------|-----------|  
|Utilisateurs|Contient des comptes d’utilisateur pour le personnel non administratif.|  
|Comptes de service|Certains services qui requièrent l’accès aux ressources réseau s’exécutent en tant que comptes d’utilisateur. Cette unité d’organisation est créée pour séparer les comptes d’utilisateur de service des comptes d’utilisateur contenus dans l’unité d’organisation utilisateurs. En outre, le placement des différents types de comptes d’utilisateur dans des unités d’organisation distinctes vous permet de les gérer en fonction de leurs exigences d’administration spécifiques.|  
|Ordinateurs|Contient des comptes pour les ordinateurs autres que les contrôleurs de domaine.|  
|Groups|Contient des groupes de tous les types, à l’exception des groupes d’administration, qui sont gérés séparément.|  
|Admins|Contient des comptes d’utilisateurs et de groupes pour les administrateurs de données dans la structure de l’unité d’organisation de compte pour leur permettre d’être gérés séparément des utilisateurs standard. Activez l’audit pour cette unité d’organisation afin de pouvoir effectuer le suivi des modifications apportées aux utilisateurs et aux groupes administratifs.|  
  
L’illustration suivante montre un exemple de conception de groupe d’administration pour une structure d’unité d’organisation de compte.  
  
![délégation de l’administration](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/be2cd2d2-6956-429c-a53a-369e6fe40b2b.gif)  
  
Les groupes qui gèrent les unités d’organisation enfants disposent uniquement d’un contrôle total sur la classe spécifique d’objets qu’ils sont responsables de la gestion.  
  
Les types de groupes que vous utilisez pour déléguer le contrôle au sein d’une structure d’unité d’organisation sont basés sur l’emplacement des comptes par rapport à la structure d’UO qui doit être gérée. Si les comptes d’utilisateur administrateur et la structure d’unité d’organisation existent tous dans un domaine unique, les groupes que vous créez pour la délégation doivent être des groupes globaux. Si votre organisation dispose d’un service qui gère ses propres comptes d’utilisateur et existe dans plusieurs régions géographiques, vous pouvez avoir un groupe d’administrateurs de données responsable de la gestion des unités d’organisation de compte dans plusieurs domaines. Si les comptes des administrateurs de données existent tous dans un domaine unique et que vous avez des structures d’UO dans plusieurs domaines auxquels vous devez déléguer le contrôle, faites de ces comptes d’administration des membres de groupes globaux et déléguez le contrôle des structures d’UO de chaque domaine à ces groupes globaux. Si les administrateurs de données auxquels vous déléguez le contrôle d’une structure d’UO proviennent de plusieurs domaines, vous devez utiliser un groupe universel. Les groupes universels peuvent contenir des utilisateurs de différents domaines, et par conséquent, ils peuvent être utilisés pour déléguer le contrôle dans plusieurs domaines.  
  
## <a name="delegating-administration-of-resource-ous"></a>Délégation de l’administration des unités d’organisation des ressources  
Les unités d’organisation des ressources sont utilisées pour gérer l’accès aux ressources. Le propriétaire de l’unité d’organisation de ressource crée des comptes d’ordinateurs pour les serveurs qui sont joints au domaine qui incluent des ressources telles que des partages de fichiers, des bases de données et des imprimantes. Le propriétaire de l’unité d’organisation de ressource crée également des groupes pour contrôler l’accès à ces ressources.  
  
L’illustration suivante montre les deux emplacements possibles pour l’unité d’organisation de ressource.  
  
![délégation de l’administration](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/6667a5ce-34d6-48a9-9974-b823ba70e2af.gif)  
  
L’unité d’organisation Resource peut se trouver sous la racine du domaine ou en tant qu’unité d’organisation enfant de l’unité d’organisation de compte correspondante dans la hiérarchie d’administration de l’unité d’organisation. Les unités d’organisation de ressource n’ont pas d’UO enfants standard. Les ordinateurs et les groupes sont placés directement dans l’unité d’organisation de ressource.  
  
Le propriétaire de l’unité d’organisation de ressource possède les objets dans l’unité d’organisation, mais ne possède pas le conteneur d’UO lui-même. Les propriétaires d’UO de ressources ne gèrent que les objets ordinateur et groupe. ils ne peuvent pas créer d’autres classes d’objets dans l’unité d’organisation et ne peuvent pas créer d’unités d’organisation enfants.  
  
> [!NOTE]  
> Le créateur ou le propriétaire d’un objet peut définir la liste de contrôle d’accès (ACL) sur l’objet indépendamment des autorisations qui sont héritées du conteneur parent. Si un propriétaire d’UO de ressources peut réinitialiser la liste de contrôle d’accès (ACL) sur une unité d’organisation, ce propriétaire peut créer n’importe quelle classe d’objet dans l’unité d’organisation, y compris les utilisateurs. Pour cette raison, les propriétaires d’UO de ressources ne sont pas autorisés à créer des unités d’organisation.  
  
Pour chaque unité d’organisation de ressource dans le domaine, créez un groupe global pour représenter les administrateurs de données responsables de la gestion du contenu de l’unité d’organisation. Ce groupe dispose d’un contrôle total sur les objets de groupe et d’ordinateur dans l’unité d’organisation, mais pas sur le conteneur d’UO lui-même.  
  
L’illustration suivante montre la conception d’un groupe d’administration pour une unité d’organisation de ressource.  
  
![délégation de l’administration](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/8a3f7714-a3bf-43f7-b999-6070543248b0.gif)  
  
Le fait de placer les comptes d’ordinateur dans une unité d’organisation de ressource donne au propriétaire de l’unité d’organisation le contrôle des objets de compte, mais ne fait pas de l’administrateur de l’UO un administrateur des ordinateurs. Dans un domaine Active Directory, le groupe Admins du domaine est, par défaut, placé dans le groupe Administrateurs local sur tous les ordinateurs. Autrement dit, les administrateurs de service contrôlent ces ordinateurs. Si les propriétaires d’UO de ressources nécessitent un contrôle administratif sur les ordinateurs de leurs UO, le propriétaire de la forêt peut appliquer des groupes restreints stratégie de groupe pour que le propriétaire de l’unité d’organisation Resource soit membre du groupe Administrateurs sur les ordinateurs de cette unité d’organisation.  
  


