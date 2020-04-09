---
title: Créer un compte d’utilisateur administratif
description: Créer un compte avec des privilèges d’administrateur dans MultiPoint services
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 8ce4c5a9-3dec-412f-910b-54a252f8f209
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 72b7517d35d18064806d3df35f2f9ed7b636df8d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859772"
---
# <a name="create-an-administrative-user-account"></a>Créer un compte d’utilisateur administratif
Créez des *comptes d’utilisateur administratif* pour les personnes qui gèrent le système MultiPoint Services. Pour voir qui dispose d’un accès administrateur, dans le gestionnaire MultiPoint, cliquez sur l’onglet **utilisateurs** . les comptes d’utilisateur d’administration s’affichent dans la colonne **type de compte** en tant qu' **administrateur**. Les *utilisateurs administratifs* ont accès à toutes les tâches du gestionnaire multipoint qui modifient les paramètres du bureau et du système, par exemple :  
  
-   Création de comptes  
  
-   Ajout et suppression de programmes  
  
-   Gestion des *bureaux* et du matériel  
  
-   Arrêt des *sessions* d’un autre utilisateur  
  
Les utilisateurs administratifs peuvent exécuter des tâches qui affectent tous les autres utilisateurs du système MultiPoint Services, par exemple l’installation d’un logiciel ou la modification des paramètres de sécurité. C’est pourquoi les utilisateurs administratifs doivent avoir des noms d’utilisateur et des mots de passe uniques qu’ils sont les seuls à connaître.  
  
Pour plus d’informations sur les problèmes qui vous intéressent en tant qu’utilisateur administratif, car vous créez et gérez des comptes d’utilisateur, voir la rubrique [Considérations sur le compte d’utilisateur](User-Account-Considerations.md).  
  
> [!NOTE]  
> Vous voudrez peut-être créer un *compte d’utilisateur standard* pour exécuter des tâches sur le système MultiPoint Services qui ne sont pas liées à la gestion du système MultiPoint Services. Vous vous connectez alors à votre compte d’utilisateur administratif uniquement pour exécuter des tâches de gestion du système.  
  
#### <a name="to-create-an-administrative-user-account"></a>Pour créer un compte d’utilisateur administratif  
  
1.  Dans le gestionnaire MultiPoint, cliquez sur l’onglet **utilisateurs** .  
  
2.  Sous **Tâches utilisateur**, cliquez sur **Ajouter un compte d’utilisateur**. L’Assistant **Ajouter un compte d’utilisateur** s’ouvre.  
  
3.  Dans le champ **Compte d’utilisateur**, tapez un nom de connexion pour l’utilisateur. En général, le nom de connexion de l’utilisateur est le prénom et le nom écrits en un mot sans espace, ou l’initiale du prénom et le nom écrits en un mot sans espace.  
  
4.  Dans le champ **Nom complet**, tapez le nom de l’utilisateur au format de votre choix, par exemple le prénom, le nom complet ou un surnom.  
  
5.  Dans le champ **Mot de passe**, tapez un mot de passe pour l’utilisateur. Seuls vous et l’utilisateur devez connaître le mot de passe, qui doit être stocké en lieu sûr. Le mot de passe peut être modifié uniquement par un utilisateur administratif.  
  
6.  Dans le champ **Confirmer le mot de passe**, retapez le mot de passe, puis cliquez sur **Suivant**.  
  
7.  Dans la page de niveau d’accès, sélectionnez **Utilisateur administratif**, puis cliquez sur **Suivant**.  
  
8.  MultiPoint Services vérifie toutes les informations et affiche un message quand le compte est installé. Quand le texte **Nouveau compte d’utilisateur correctement créé** s’affiche, cliquez sur **Terminer**.  
