---
ms.assetid: d8e61aa4-8e4b-4097-83ca-70cf61366b75
title: Délégation de l’administration avec des objets d’unité d’organisation
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 31b8ef30cb12903936d00a8ab8fe56de77f8025a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408940"
---
# <a name="delegating-administration-by-using-ou-objects"></a>Délégation de l’administration avec des objets d’unité d’organisation

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous pouvez utiliser des unités d’organisation (UO) pour déléguer l’administration d’objets, tels que des utilisateurs ou des ordinateurs, au sein de l’unité d’organisation à un individu ou un groupe désigné. Pour déléguer l’administration à l’aide d’une unité d’organisation, placez la personne ou le groupe auquel vous déléguez des droits d’administration dans un groupe, placez l’ensemble d’objets à contrôler dans une unité d’organisation, puis déléguez les tâches d’administration de l’unité d’organisation à ce groupe.  
  
Active Directory Domain Services (AD DS) vous permet de contrôler les tâches d’administration qui peuvent être déléguées à un niveau très détaillé. Par exemple, vous pouvez attribuer à un groupe un contrôle total de tous les objets d’une unité d’organisation ; Affectez un autre groupe aux droits uniquement pour créer, supprimer et gérer des comptes d’utilisateur dans l’unité d’organisation. puis, attribuez un troisième groupe uniquement pour réinitialiser les mots de passe des comptes d’utilisateur. Vous pouvez rendre ces autorisations héritables afin qu’elles s’appliquent à toutes les unités d’organisation placées dans des sous-arborescences de l’unité d’organisation d’origine.  
  
Les unités d’organisation et les conteneurs par défaut sont créés lors de l’installation de AD DS et sont contrôlés par les administrateurs de service. Il est préférable que les administrateurs de service continuent à contrôler ces conteneurs. Si vous avez besoin de déléguer le contrôle sur des objets dans l’annuaire, créez des UO supplémentaires et placez les objets dans ces unités d’organisation. Déléguez le contrôle de ces UO aux administrateurs de données appropriés. Cela permet de déléguer le contrôle sur les objets de l’annuaire sans modifier le contrôle par défaut donné aux administrateurs de service.  
  
Le propriétaire de la forêt détermine le niveau d’autorité délégué à un propriétaire de l’unité d’organisation. Cela peut aller de la possibilité de créer et de manipuler des objets dans l’unité d’organisation de sorte qu’ils ne soient autorisés à contrôler qu’un seul attribut d’un seul type d’objet dans l’unité d’organisation. Accorder à un utilisateur la possibilité de créer un objet dans l’unité d’organisation accorde implicitement à cet utilisateur la possibilité de manipuler n’importe quel attribut de n’importe quel objet créé par l’utilisateur. En outre, si l’objet créé est un conteneur, l’utilisateur a implicitement la possibilité de créer et de manipuler des objets placés dans le conteneur.  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Délégation de l’administration des conteneurs et unités d’organisation par défaut](../../ad-ds/plan/Delegating-Administration-of-Default-Containers-and-OUs.md)  
  
-   [Délégation de l’administration des unités d’organisation de comptes et de ressources](../../ad-ds/plan/Delegating-Administration-of-Account-OUs-and-Resource-OUs.md)  
  


