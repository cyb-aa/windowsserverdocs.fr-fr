---
title: Considérations relatives au compte d’utilisateur
description: Fournit des considérations sur le compte d’utilisateur, le nom d’utilisateur et le mot de passe pour MultiPoint services
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
ms.openlocfilehash: c4a0355b5e081e0673447fb86f1475d0b34c3792
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871499"
---
# <a name="user-account-considerations"></a>Considérations relatives au compte d’utilisateur
Cette rubrique décrit les problèmes que vous devez prendre en compte, en tant qu’utilisateur administratif, lorsque vous créez et gérez des comptes d’utilisateur. Vous gérez les comptes d’utilisateur sous l’onglet utilisateurs du gestionnaire MultiPoint. Pour plus d’informations, voir la rubrique [Gérer les comptes d’utilisateur](Manage-User-Accounts.md).  
  
## <a name="user-account-types"></a>Types de comptes d’utilisateur  
Un compte d’utilisateur est un ensemble d’informations qui indique à MultiPoint services les fichiers et les dossiers auxquels un utilisateur peut accéder, les modifications qu’il peut apporter au système MultiPoint services et les préférences de chaque utilisateur, telles que l’arrière-plan du bureau. Chaque personne accède à son propre compte d’utilisateur en utilisant un nom d’utilisateur et un mot de passe uniques. MultiPoint Services prend en charge trois types de comptes d’utilisateur :  
  
-   Les **comptes d’utilisateur d’administration** sont destinés aux personnes qui utilisent le gestionnaire multipoint pour utiliser et gérer le système multipoint services. Pour plus d’informations, voir [Créer un compte d’utilisateur administratif](Create-an-Administrative-User-Account.md).  
  
-   Les **comptes d’utilisateur standard** sont pour les personnes qui accèdent régulièrement aux stations, mais qui ne gèrent pas le système. En général, vous devez créer des comptes d’utilisateur standard pour la plupart des utilisateurs du système MultiPoint Services. Pour plus d’informations, voir [Créer un compte d’utilisateur standard](Create-a-Standard-User-Account.md).  
  
-   Les **comptes d’utilisateur du tableau de bord MultiPoint** sont pour les personnes qui utilisent le tableau de bord MultiPoint pour gérer les sessions utilisateur standard et peuvent se connecter de n’importe quelle station. Pour plus d’informations, voir [Créer un compte d’utilisateur du tableau de bord MultiPoint](Create-a-MultiPoint-Dashboard-User-Account.md).  
  
## <a name="user-name-and-password-considerations"></a>Considérations relatives au nom d’utilisateur et au mot de passe  
Les utilisateurs administratifs peuvent exécuter des tâches qui affectent tous les autres utilisateurs du système MultiPoint Services, par exemple l’installation d’un logiciel ou la modification des paramètres de sécurité. C’est pourquoi les utilisateurs administratifs doivent avoir des noms d’utilisateur et des mots de passe uniques qu’ils sont les seuls à connaître.  
  
Il est à noter que chaque compte d’utilisateur est doté d’une bibliothèque **Documents** dans l’Explorateur Windows qui inclut le dossier **Mes Documents**. Si les utilisateurs standard de votre système MultiPoint Services stockent les documents privés dans leur bibliothèque **Documents** dans l’Explorateur Windows, ils doivent également se connecter au système MultiPoint Services avec un nom d’utilisateur et un mot de passe uniques qu’ils sont les seuls à connaître. Pour plus d’informations sur le stockage de documents dans l’Explorateur Windows, voir la rubrique [Gérer les fichiers d’utilisateur](Manage-User-Files.md).  
  
> [!TIP]  
> Pour renforcer la sécurité du système, les mots de passe de tous les utilisateurs doivent être des mots de passe forts. Un mot de passe fort est un mot de passe qui ne peut pas être deviné ou fissuré facilement, contient au moins huit caractères, ne contient pas tout ou partie du nom de compte de l’utilisateur et contient au moins trois des quatre catégories de caractères suivantes : caractères en majuscules, minuscules caractères, chiffres et symboles trouvés sur un clavier (tels que !, @, #).  
  
## <a name="see-also"></a>Voir aussi  
[Créer un compte d’utilisateur administratif](Create-an-Administrative-User-Account.md)  
[Créer un compte d’utilisateur standard](Create-a-Standard-User-Account.md)  
[Gérer les fichiers](Manage-User-Files.md)
utilisateur[gérer les comptes d’utilisateur](Manage-User-Accounts.md)