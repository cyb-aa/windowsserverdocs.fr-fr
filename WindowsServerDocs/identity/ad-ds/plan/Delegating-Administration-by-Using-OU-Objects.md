---
ms.assetid: d8e61aa4-8e4b-4097-83ca-70cf61366b75
title: "Délégation de l’Administration à l’aide d’objets de l’unité d’organisation"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 8d0b4765304d8b302fc174c191af2c8e87a25304
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="delegating-administration-by-using-ou-objects"></a>Délégation de l’Administration à l’aide d’objets de l’unité d’organisation

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Vous pouvez utiliser des unités d’organisation (UO) pour déléguer l’administration des objets, tels que des utilisateurs ou des ordinateurs, au sein de l’unité d’organisation à un groupe ou une personne. Pour déléguer l’administration à l’aide d’une unité d’organisation, placez l’individu ou le groupe auquel vous sont délégation de droits d’administration dans un groupe, placez l’ensemble des objets à contrôler dans une unité d’organisation et ensuite déléguer les tâches d’administration pour l’unité d’organisation à ce groupe.  
  
Services de domaine Active Directory (AD DS) vous permet de contrôler les tâches administratives peuvent être déléguées à un niveau très détaillé. Par exemple, vous pouvez affecter un groupe possèdent le contrôle total de tous les objets dans une unité d’organisation; affecter un autre groupe les droits uniquement à créer, supprimer et gérer les comptes d’utilisateur dans l’unité d’organisation; et puis attribuer une troisième groupe le droit uniquement réinitialisation des mots de passe de compte utilisateur. Vous pouvez rendre ces autorisations pouvant être héritées afin qu’ils s’appliquent à toutes les unités d’organisation qui sont placées dans des sous-arborescences de l’unité d’organisation d’origine.  
  
Conteneurs et unités d’organisation par défaut sont créés pendant l’installation des services AD DS et sont contrôlés par les administrateurs de service. Il est préférable si les administrateurs de service continuer à contrôler ces conteneurs. Si vous avez besoin déléguer le contrôle sur les objets dans le répertoire, créez les unités d’organisation supplémentaires et placer les objets dans ces unités d’organisation. Déléguer le contrôle ces unités d’organisation pour les administrateurs de données approprié. Cela rend possible déléguer le contrôle sur les objets dans le répertoire sans modifier le contrôle par défaut donné aux administrateurs de service.  
  
Le propriétaire de la forêt détermine le niveau de l’autorité qui est délégué pour un propriétaire de l’unité d’organisation. Cela peut être comprise entre la possibilité de créer et manipuler des objets au sein de l’unité d’organisation à uniquement pour contrôler un attribut unique d’un seul type d’objet dans l’unité d’organisation. Accorder à un utilisateur la possibilité de créer un objet dans l’unité d’organisation implicitement accorde à cet utilisateur la possibilité de manipuler n’importe quel attribut de n’importe quel objet créé par l’utilisateur. En outre, si l’objet qui est créé est un conteneur, l’utilisateur a implicitement la possibilité de créer et de manipuler les objets qui sont placés dans le conteneur.  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Déléguer l’Administration des unités d’organisation et des conteneurs par défaut](../../ad-ds/plan/Delegating-Administration-of-Default-Containers-and-OUs.md)  
  
-   [Déléguer l’Administration des unités d’organisation de compte et de ressources](../../ad-ds/plan/Delegating-Administration-of-Account-OUs-and-Resource-OUs.md)  
  


