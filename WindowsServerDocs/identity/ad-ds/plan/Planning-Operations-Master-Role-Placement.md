---
ms.assetid: bd64a766-5362-4f29-b963-5465c2bb79e7
title: Planification de l’emplacement des rôles de maître d’opérations
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: a3fbe76302199888dce19f845b1c838c07facb82
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870890"
---
# <a name="planning-operations-master-role-placement"></a>Planification de l’emplacement des rôles de maître d’opérations

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Services de domaine Active Directory (AD DS) prend en charge la réplication multimaître des données d’annuaire, ce qui signifie que tout contrôleur de domaine peut accepter des modifications d’annuaire et réplique les modifications dans tous les autres contrôleurs de domaine. Toutefois, certaines modifications, telles que les modifications de schéma, sont difficile d’effectuer de manière multimaître. C’est pourquoi certains contrôleurs de domaine, appelés maîtres d’opérations, maintenez la touche rôles chargés d’accepter les demandes pour certaines des modifications spécifiques.  
  
> [!NOTE]  
> Détenteurs du rôle de maître d’opérations doivent être en mesure d’écrire des informations à la base de données Active Directory. En raison de la nature en lecture seule de la base de données Active Directory sur un contrôleur de domaine en lecture seule (RODC), **RODC ne peut pas servir de détenteurs du rôle de maître d’opérations**.  
  
Il existe trois rôles (également appelés opérations à maître unique ou FSMO) dans chaque domaine :  
  
- Le maître d’opérations émulateur PDC (contrôleur de domaine principal) traite toutes les mises à jour de mot de passe.  

- Du maître d’opérations RID (ID) gère le pool RID global pour le domaine et alloue des pools RID locales sur tous les contrôleurs de domaine pour vous assurer que tous les principaux de sécurité créés dans le domaine ont un identificateur unique.  
- Un domaine donné dans le maître d’opérations de l’infrastructure gère une liste des principaux de sécurité à partir d’autres domaines qui sont membres des groupes au sein de son domaine.  

Outre les rôles de maître des opérations au niveau du domaine trois, deux rôles existent dans chaque forêt :  
  
- Le maître d’opérations de schéma régit les modifications du schéma.  
- Le maître d’opérations d’affectation de noms de domaine ajoute et supprime des domaines et des autres partitions d’annuaire (par exemple, les partitions application système DNS (Domain Name)) vers et à partir de la forêt.  
  
Placez les contrôleurs de domaine qui héberge ces rôles de maître d’opérations dans les zones où la fiabilité du réseau est élevée et vous assurer que l’émulateur de contrôleur de domaine principal et le maître RID sont constamment disponibles.  
  
Détenteurs du rôle de maître d’opérations sont affectés automatiquement lors de la création du premier contrôleur de domaine dans un domaine donné. Les deux rôles de niveau de la forêt (contrôleur de schéma et maître d’attribution de noms de domaine) sont affectés au premier contrôleur de domaine créé dans une forêt. En outre, les trois rôles au niveau du domaine (maître RID, maître d’infrastructure et d’émulateur PDC) sont affectés au premier contrôleur de domaine créé dans un domaine.  
  
> [!NOTE]  
> Affectations de détenteur du rôle de maître des opérations automatiques sont effectuées uniquement lorsqu’un nouveau domaine est créé lorsqu’un détenteur de rôle actuel est rétrogradé. Toutes les autres modifications apportées aux propriétaires des rôles ont d’être initiées par un administrateur.  
  
Ces affectations de rôle de maître d’opérations automatique peuvent entraîner une utilisation très élevée du processeur sur le premier contrôleur de domaine créé dans la forêt ou le domaine. Pour éviter ce problème, affecter les opérations (transfert) rôles de maître sur différents contrôleurs de domaine dans votre domaine ou forêt. Placer les contrôleurs de domaine que hôte maître d’opérations dans les zones où le réseau est fiable et où les maîtres d’opérations sont accessible par tous les autres contrôleurs de domaine dans la forêt.  
  
Vous pouvez également désigner des opérations de mise en veille (autre) maîtres pour toutes les opérations des rôles de maître. Les maîtres d’opérations en attente sont des contrôleurs de domaine vers lequel vous pouvez transférer les rôles de maître d’opérations en cas d’échouent les détenteurs du rôle d’origine. Assurez-vous que les maîtres d’opérations en attente sont des partenaires de réplication directe des maîtres d’opérations réelle.  
  
## <a name="planning-the-pdc-emulator-placement"></a>Planification du positionnement d’émulateur de contrôleur de domaine principal

L’émulateur de contrôleur de domaine principal traite les modifications de mot de passe client. L’émulateur de contrôleur de domaine principal dans chaque domaine dans la forêt joue le rôle contrôleur de domaine qu’une seule.  
  
Même si tous les contrôleurs de domaine sont mis à niveau vers Windows 2000, Windows Server 2003 et Windows Server 2008 et que le domaine fonctionne au niveau fonctionnel Windows 2000 natif, l’émulateur PDC reçoit une réplication préférentielle des modifications de mot de passe effectuées en d’autres contrôleurs de domaine dans le domaine. Si un mot de passe a été modifié récemment, cette modification prend de temps pour répliquer sur chaque contrôleur de domaine dans le domaine. Si l’authentification de connexion échoue sur un autre contrôleur de domaine en raison d’un mot de passe incorrect, ce contrôleur de domaine transfère la demande d’authentification à l’émulateur de contrôleur de domaine principal avant de décider d’accepter ou rejeter la tentative d’ouverture de session.  
  
Placez l’émulateur de contrôleur de domaine principal dans un emplacement qui contient un grand nombre d’utilisateurs de ce domaine pour les opérations de transfert si nécessaire un mot de passe. En outre, assurez-vous que l’emplacement est bien connecté à d’autres emplacements pour réduire la latence de réplication.  
  
Pour une feuille de calcul pour vous aider à documenter les informations sur où vous prévoyez de placer des émulateurs de contrôleur de domaine principal et le nombre d’utilisateurs pour chaque domaine qui est représenté dans chaque emplacement, consultez le travail aides pour Windows Server 2003 Deployment Kit ([ https://go.microsoft.com/fwlink/?LinkID=102558 ](https://go.microsoft.com/fwlink/?LinkID=102558)), Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip de télécharger et ouvrir le Placement des contrôleurs de domaine (DSSTOPO_4.doc).  
  
Vous avez besoin faire référence aux informations sur les emplacements dans lesquels vous devez placer les émulateurs de contrôleur de domaine principal lors du déploiement de domaines régionaux. Pour plus d’informations sur le déploiement de domaines régionaux, consultez [déploiement de Windows Server 2008 domaines régionaux](https://technet.microsoft.com/library/cc755118.aspx).  
  
## <a name="requirements-for-infrastructure-master-placement"></a>Configuration requise pour la sélection élective maître d’infrastructure  

Le maître d’infrastructure met à jour les noms de principaux de sécurité à partir d’autres domaines sont ajoutés aux groupes dans son propre domaine. Par exemple, si un utilisateur d’un domaine est membre d’un groupe dans un second domaine et le nom d’utilisateur est modifié dans le premier domaine, le deuxième domaine n'est pas averti que le nom d’utilisateur doit être mis à jour dans la liste des membres du groupe. Étant donné que les contrôleurs de domaine dans un domaine ne pas répliquent les principaux de sécurité pour les contrôleurs de domaine dans un autre domaine, le deuxième domaine devient jamais prenant en charge de la modification en l’absence du maître d’infrastructure.  
  
Le maître d’infrastructure constamment moniteurs appartenances au groupe, recherchez les principaux de sécurité à partir d’autres domaines. S’il en trouve, il vérifie avec le domaine de l’entité de sécurité pour vérifier que les informations sont mis à jour. Si les informations sont obsolètes, le maître d’infrastructure effectue la mise à jour et réplique ensuite la modification aux autres contrôleurs de domaine dans son domaine.  
  
Deux exceptions s’appliquent à cette règle. Tout d’abord, si tous les contrôleurs de domaine sont des serveurs de catalogue global, le contrôleur de domaine qui héberge le rôle de maître d’infrastructure est insignifiant, car les catalogues globaux de répliquent les informations mises à jour, quel que soit le domaine auquel ils appartiennent. En second lieu, si la forêt n'a qu’un seul domaine, le contrôleur de domaine qui héberge le rôle de maître d’infrastructure est insignifiant, car les entités de sécurité à partir d’autres domaines n’existent pas.  
  
Ne placez pas le maître d’infrastructure sur un contrôleur de domaine qui est également un serveur de catalogue global. Si le maître d’infrastructure et le catalogue global sont sur le même contrôleur de domaine, le maître d’infrastructure ne fonctionnera pas. Le maître d’infrastructure ne trouvera jamais de données sont obsolètes ; Par conséquent, il répliquera jamais les modifications sur les autres contrôleurs de domaine dans le domaine.  
  
## <a name="operations-master-placement-for-networks-with-limited-connectivity"></a>Sélection élective pour les réseaux de maître d’opérations avec une connectivité limitée

N’oubliez pas que si votre environnement a un emplacement central ou un site hub dans lequel vous pouvez placer les détenteurs du rôle de maître d’opérations, certaines opérations de contrôleur de domaine qui dépendent de la disponibilité de ces opérations rôle du maître détenteurs susceptibles d’être affectés.  
  
Par exemple, supposons qu’une organisation crée des sites A, B, C, et des liens de sites de D. existent entre A et B, entre B et C et entre C et D. la connectivité réseau exactement reflète la connectivité réseau des liens de sites. Dans cet exemple, toutes les opérations de rôles de maître sont placés dans de site une et l’option pour **relier tous les liens de site** n’est pas sélectionnée.  
  
Bien que cette configuration entraîne la réussite de la réplication entre tous les sites, les fonctions de rôle de maître d’opérations présentent les limitations suivantes :  
  
- Contrôleurs de domaine dans les sites C et D ne peut pas accéder à l’émulateur de contrôleur de domaine principal dans le site A pour mettre à jour un mot de passe ou pour le mot de passe qui a été récemment mis à jour.  
- Contrôleurs de domaine dans les sites C et D ne peut pas accéder au maître RID dans le site A pour obtenir un pool RID initial après l’installation d’Active Directory et pour actualiser les pools RID comme ils diminution.  
- Contrôleurs de domaine dans les sites C et D ne peut pas ajouter ou supprimer directory, DNS ou les partitions d’application personnalisée.  
- Contrôleurs de domaine dans les sites C et D ne peut pas modifier le schéma.  
  
Pour une feuille de calcul pour vous aider à planifier le placement de rôle de maître d’opérations, consultez [travail aides pour Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip de télécharger et ouvrir Placement des contrôleurs de domaine (DSSTOPO_4.doc).  
  
Vous devrez consulter ces informations lorsque vous créez le domaine racine de forêt et domaines régionaux. Pour plus d’informations sur le déploiement du domaine racine de forêt, consultez Déploiement un [déploiement d’un domaine racine de forêt Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx). Pour plus d’informations sur le déploiement de domaines régionaux, consultez [déploiement de Windows Server 2008 domaines régionaux](https://technet.microsoft.com/library/cc755118.aspx).  

## <a name="next-steps"></a>Étapes suivantes

Vous trouverez des informations supplémentaires sur le placement des rôles FSMO dans la rubrique prise en charge [FSMO placement et optimisation sur les contrôleurs de domaine Active Directory](https://support.microsoft.com/help/223346)
