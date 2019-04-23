---
ms.assetid: d8e61aa4-8e4b-4097-83ca-70cf61366b75
title: Délégation de l’administration avec des objets d’unité d’organisation
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 39c4278270e4ab4fba9ff1062d2aa043d203a74b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886940"
---
# <a name="delegating-administration-by-using-ou-objects"></a>Délégation de l’administration avec des objets d’unité d’organisation

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous pouvez utiliser des unités d’organisation (UO) pour déléguer l’administration d’objets, tels que les utilisateurs ou ordinateurs, au sein de l’unité d’organisation à un groupe ou personne désignée. Pour déléguer l’administration à l’aide d’une unité d’organisation, placez l’individu ou groupe auquel vous déléguez les droits d’administration dans un groupe, placer le jeu d’objets à contrôler dans une unité d’organisation, puis déléguer des tâches administratives pour l’unité d’organisation à ce groupe.  
  
Services de domaine Active Directory (AD DS) vous permet de contrôler les tâches administratives peuvent être déléguées à un niveau très détaillé. Par exemple, vous pouvez affecter un groupe pour avoir un contrôle total de tous les objets dans une unité d’organisation ; affecter un autre groupe les droits uniquement pour créer, supprimer et gérer des comptes d’utilisateur dans l’unité d’organisation ; et ensuite affecter un troisième groupe le droit uniquement pour réinitialiser les mots de passe de compte utilisateur. Vous pouvez rendre ces autorisations pouvant être héritées afin qu’elles s’appliquent pour les unités d’organisation qui sont placées dans les sous-arborescences de l’unité d’organisation d’origine.  
  
Conteneurs et unités d’organisation par défaut sont créés pendant l’installation des services AD DS et sont contrôlés par les administrateurs de service. Il est préférable si les administrateurs de service continuer à contrôler ces conteneurs. Si vous avez besoin déléguer le contrôle sur les objets dans le répertoire, créez les unités d’organisation supplémentaires et placer les objets dans ces unités d’organisation. Déléguer le contrôle sur ces unités d’organisation pour les administrateurs de données approprié. Cela rend possible de déléguer le contrôle sur les objets dans le répertoire sans modifier le contrôle par défaut donné aux administrateurs de service.  
  
Le propriétaire de la forêt détermine le niveau d’autorité est déléguée à un propriétaire de l’unité d’organisation. Cela peut consister à partir de la possibilité de créer et manipuler des objets dans l’unité d’organisation à admis pour contrôler un seul attribut d’un type d’objet dans l’unité d’organisation unique. Accorder à un utilisateur la possibilité de créer un objet dans l’unité d’organisation implicitement accorde à cet utilisateur la possibilité de manipuler n’importe quel attribut de tout objet créé par l’utilisateur. En outre, si l’objet qui est créé est un conteneur, l’utilisateur a implicitement la possibilité de créer et manipuler des objets qui sont placés dans le conteneur.  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Délégation de l’Administration des unités d’organisation et les conteneurs par défaut](../../ad-ds/plan/Delegating-Administration-of-Default-Containers-and-OUs.md)  
  
-   [Délégation de l’Administration des unités d’organisation de compte et les unités d’organisation de ressource](../../ad-ds/plan/Delegating-Administration-of-Account-OUs-and-Resource-OUs.md)  
  


