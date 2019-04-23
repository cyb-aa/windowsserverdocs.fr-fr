---
title: Considérations relatives au compte d’utilisateur
description: Fournit le compte d’utilisateur, nom d’utilisateur et mot de passe considérations relatives à MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e225900b-cee9-48c9-b21c-394dc5e72b78
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 00fb5e83921ba0b8ad86a6f75bdfd7bf16419b73
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850790"
---
# <a name="user-account-considerations"></a>Considérations relatives au compte d’utilisateur
Cette rubrique décrit les problèmes que vous, en tant qu’administrateur, devez prendre en compte que vous créez et gérez des comptes d’utilisateur. Vous gérez des comptes d’utilisateur dans l’onglet utilisateurs dans le gestionnaire MultiPoint. Pour plus d’informations, voir la rubrique [Gérer les comptes d’utilisateur](Manage-User-Accounts.md).  
  
## <a name="user-account-types"></a>Types de comptes d’utilisateur  
Un compte d’utilisateur regroupe un ensemble d’informations qui indiquent à MultiPoint Services à quels fichiers et à quels dossiers un utilisateur a accès, quelles modifications un utilisateur peut apporter au système MultiPoint Services et les préférences de chaque utilisateur, par exemple l’arrière-plan du Bureau. Chaque personne accède à son propre compte d’utilisateur en utilisant un nom d’utilisateur et un mot de passe uniques. MultiPoint Services prend en charge trois types de comptes d’utilisateur :  
  
-   **Comptes d’utilisateur administratif** sont pour les personnes qui utilisent le gestionnaire MultiPoint pour utiliser et gérer le système MultiPoint Services. Pour plus d’informations, voir [Créer un compte d’utilisateur administratif](Create-an-Administrative-User-Account.md).  
  
-   Les **comptes d’utilisateur standard** sont pour les personnes qui accèdent régulièrement aux stations, mais qui ne gèrent pas le système. En général, vous devez créer des comptes d’utilisateur standard pour la plupart des utilisateurs du système MultiPoint Services. Pour plus d’informations, voir [Créer un compte d’utilisateur standard](Create-a-Standard-User-Account.md).  
  
-   Les **comptes d’utilisateur du tableau de bord MultiPoint** sont pour les personnes qui utilisent le tableau de bord MultiPoint pour gérer les sessions utilisateur standard et peuvent se connecter de n’importe quelle station. Pour plus d’informations, voir [Créer un compte d’utilisateur du tableau de bord MultiPoint](Create-a-MultiPoint-Dashboard-User-Account.md).  
  
## <a name="user-name-and-password-considerations"></a>Considérations relatives au nom d’utilisateur et au mot de passe  
Les utilisateurs administratifs peuvent exécuter des tâches qui affectent tous les autres utilisateurs du système MultiPoint Services, par exemple l’installation d’un logiciel ou la modification des paramètres de sécurité. C’est pourquoi les utilisateurs administratifs doivent avoir des noms d’utilisateur et des mots de passe uniques qu’ils sont les seuls à connaître.  
  
Il est à noter que chaque compte d’utilisateur est doté d’une bibliothèque **Documents** dans l’Explorateur Windows qui inclut le dossier **Mes Documents**. Si les utilisateurs standard de votre système MultiPoint Services stockent les documents privés dans leur bibliothèque **Documents** dans l’Explorateur Windows, ils doivent également se connecter au système MultiPoint Services avec un nom d’utilisateur et un mot de passe uniques qu’ils sont les seuls à connaître. Pour plus d’informations sur le stockage de documents dans l’Explorateur Windows, voir la rubrique [Gérer les fichiers d’utilisateur](Manage-User-Files.md).  
  
> [!TIP]  
> Pour une meilleure sécurité du système, tous les mots de passe des utilisateurs doivent être forts. Un mot de passe fort est celui qui ne peut pas être difficile de deviner ou envahi, est au moins huit caractères, ne contient pas de tout ou partie du nom du compte de l’utilisateur et contient au moins trois des quatre catégories de caractères suivantes : majuscules, minuscules caractères, des chiffres et symboles se trouvant sur un clavier (tels que !, @, #).  
  
## <a name="see-also"></a>Voir aussi  
[Créer un compte d’utilisateur administratif](Create-an-Administrative-User-Account.md)  
[Créer un compte d’utilisateur Standard](Create-a-Standard-User-Account.md)  
[Gérer les fichiers utilisateur](Manage-User-Files.md)
[gérer les comptes d’utilisateur](Manage-User-Accounts.md)