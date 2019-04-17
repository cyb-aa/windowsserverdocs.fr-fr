---
ms.assetid: bd64a766-5362-4f29-b963-5465c2bb79e7
title: "Planification de l’emplacement du rôle de maître des opérations"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9d271ed3a54e78106aed0d4dbfdb610cc8b9a460
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="planning-operations-master-role-placement"></a>Planification de l’emplacement du rôle de maître des opérations

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Services de domaine ActiveDirectory (ADDS) prend en charge la réplication multimaître des données d’annuaire, ce qui signifie que tout contrôleur de domaine peut accepter les modifications de l’annuaire et répliquer les modifications à tous les autres contrôleurs de domaine. Toutefois, certaines modifications, telles que des modifications de schéma sont difficile d’effectuer de manière multimaître. Pour cette raison certains contrôleurs de domaine, appelés maîtres d’opérations, maintenez rôles responsable pour accepter les demandes de certaines modifications spécifiques.  
  
> [!NOTE]  
> Détenteurs du rôle de maître d’opérations doivent être en mesure d’écrire des informations à la base de données ActiveDirectory. En raison de la nature en lecture seule de la base de données ActiveDirectory sur un contrôleur de domaine en lecture seule (RODC), les RODC ne peut pas agir en tant que détenteurs du rôle de maître d’opérations.  
  
Il existe trois rôles (également appelées opérations à maître unique flottant ou FSMO) dans chaque domaine:  
  
-   Le maître d’opérations émulateur PDC (PDC) traite toutes les mises à jour du mot de passe.  
  
-   Du maître d’opérations RID (ID) gère le pool RID global pour le domaine et alloue des pools RID locales sur tous les contrôleurs de domaine pour vous assurer que toutes les entités de sécurité créées dans le domaine ont un identificateur unique.  
  
-   Le maître d’opérations infrastructure pour un domaine donné conserve une liste des entités de sécurité d’autres domaines qui sont membres de groupes au sein de son domaine.  
  
Outre les rôles de maître des opérations au niveau du domaine trois, les rôles de maître de deux opérations existent dans chaque forêt:  
  
-   Le maître d’opérations de schéma régit les modifications du schéma.  
  
-   Le maître d’opérations d’attribution de noms de domaine ajoute et supprime des domaines et les autres partitions d’annuaire (par exemple, les partitions système DNS (Domain Name) application) vers et à partir de la forêt.  
  
Placer les contrôleurs de domaine qui héberge ces rôles de maître d’opérations dans les zones où la fiabilité du réseau est élevée et vérifiez que l’émulateur de contrôleur de domaine principal et le maître RID sont toujours disponibles.  
  
Détenteurs du rôle de maître d’opérations sont affectés automatiquement lors de la création du premier contrôleur de domaine dans un domaine donné. Les deux rôles au niveau de la forêt (contrôleur de schéma et maître d’attribution de noms de domaine) sont affectés au premier contrôleur de domaine créé dans une forêt. En outre, les trois rôles au niveau du domaine (maître RID, maître d’infrastructure et émulateur PDC) sont affectés au premier contrôleur de domaine créé dans un domaine.  
  
> [!NOTE]  
> Affectations de détenteur du rôle de maître des opérations automatique sont effectuées uniquement lorsqu’un nouveau domaine est créé lorsqu’un détenteur du rôle actuel est rétrogradé. Tous les autres modifications apportées aux propriétaires de rôles doivent être initiées par un administrateur.  
  
Ces affectations de rôle de maître d’opérations automatique peuvent entraîner une utilisation très élevée du processeur sur le premier contrôleur de domaine créé dans la forêt ou du domaine. Pour éviter ce problème, affectez (transferts) rôles de maître d’à différents contrôleurs de domaine dans votre domaine ou forêt. Placez les contrôleurs de domaine que dans les zones où le réseau est fiable des rôles de maître d’opérations de l’ordinateur hôte et où les maîtres d’opérations sont accessibles par tous les autres contrôleurs de domaine dans la forêt.  
  
Vous pouvez également désigner des opérations de mise en veille (remplacement) maîtres pour toutes les opérations de rôles de maître. Les maîtres d’opérations en attente sont des contrôleurs de domaine vers lequel vous pouvez transférer les rôles de maître d’opérations en cas d’échouent les détenteurs du rôle d’origine. Assurez-vous que les maîtres d’opérations en attente sont des partenaires de réplication directe des maîtres d’opérations réelle.  
  
## <a name="planning-the-pdc-emulator-placement"></a>Planification du positionnement d’émulateur de contrôleur de domaine principal  
L’émulateur PDC traite les modifications de mot de passe de client. Contrôleur de domaine qu’un seul agit comme l’émulateur de contrôleur de domaine principal dans chaque domaine dans la forêt.  
  
Même si tous les contrôleurs de domaine sont mis à niveau vers Windows2000, Windows Server2003 et Windows Server2008 et que le domaine est au niveau fonctionnel Windows2000 natif, l’émulateur PDC reçoit préférentielle réplication de mot de passe effectuées par d’autres contrôleurs de domaine dans le domaine. Si un mot de passe a été modifié récemment, cette modification de temps à répliquer sur chaque contrôleur de domaine dans le domaine. Si l’authentification d’ouverture de session échoue sur un autre contrôleur de domaine en raison d’un mot de passe incorrect, ce contrôleur de domaine transmet la demande d’authentification de l’émulateur PDC avant de décider s’il faut accepter ou refuser la tentative d’ouverture de session.  
  
Placez l’émulateur de contrôleur de domaine principal dans un emplacement qui contient un grand nombre d’utilisateurs de ce domaine mot de passe d’opérations de transfert si nécessaire. En outre, assurez-vous que l’emplacement est bien connecté à d’autres emplacements afin de réduire la latence de réplication.  
  
Pour une feuille de calcul pour vous aider à documenter les informations sur lesquels vous souhaitez placer les émulateurs de contrôleur de domaine principal et le nombre d’utilisateurs pour chaque domaine qui est représenté dans chaque emplacement, voir le travail d’identificateurs d’applet pour Windows Server2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558) ), téléchargez Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip, ouvrez (DSSTOPO_4.doc) la sélection élective du contrôleur de domaine.  
  
Vous devez faire référence aux informations sur les emplacements dans lesquels vous devez placer les émulateurs PDC lors du déploiement de domaines régionaux. Pour plus d’informations sur le déploiement de domaines régionaux, voir [déploiement de Windows Server2008domaines régionaux](https://technet.microsoft.com/library/cc755118.aspx).  
  
## <a name="requirements-for-infrastructure-master-placement"></a>Configuration requise pour la sélection élective maître d’infrastructure  

Le maître d’infrastructure met à jour les noms de principaux de sécurité d’autres domaines qui sont ajoutés aux groupes dans son propre domaine. Par exemple, si un utilisateur d’un domaine est membre d’un groupe dans un second domaine et le nom d’utilisateur est modifié dans le premier domaine, le deuxième domaine n’est pas averti que le nom d’utilisateur doit être mis à jour dans la liste des membres du groupe. Étant donné que les contrôleurs de domaine dans un domaine ne répliquent pas les entités de sécurité pour les contrôleurs de domaine dans un autre domaine, le deuxième domaine devient jamais prenant en charge de la modification en l’absence du maître d’infrastructure.  
  
Le maître d’infrastructure constamment moniteurs appartenances au groupe, recherchez les principaux de sécurité d’autres domaines. S’il trouve un, il vérifie avec le domaine de l’entité de sécurité pour vérifier que les informations sont mis à jour. Si les informations sont obsolètes, le maître d’infrastructure effectue la mise à jour et réplique ensuite la modification aux autres contrôleurs de domaine dans son domaine.  
  
Deux exceptions s’appliquent à cette règle. Tout d’abord, si tous les contrôleurs de domaine sont des serveurs de catalogue global, le contrôleur de domaine qui héberge le rôle de maître d’infrastructure est insignifiant, car les catalogues globaux de répliquent les informations mises à jour, quelle que soit le domaine auquel ils appartiennent. Ensuite, si la forêt n'a qu’un seul domaine, le contrôleur de domaine qui héberge le rôle de maître d’infrastructure est insignifiant car les principaux de sécurité d’autres domaines n’existent pas.  
  
Ne placez pas le maître d’infrastructure sur un contrôleur de domaine qui est également un serveur de catalogue global. Si le maître d’infrastructure et le catalogue global sont du même contrôleur de domaine, le maître d’infrastructure ne fonctionnera pas. Le maître d’infrastructure ne trouvera jamais de données sont obsolètes; par conséquent, il sera répliqué jamais les modifications sur les autres contrôleurs de domaine dans le domaine.  
  
## <a name="operations-master-placement-for-networks-with-limited-connectivity"></a>La sélection élective pour les réseaux de maître d’opérations avec une connectivité limitée  
N’oubliez pas que si votre environnement possède un emplacement central ou un site hub dans laquelle vous pouvez placer les détenteurs du rôle de maître d’opérations, certaines opérations de contrôleur de domaine qui dépendent de la disponibilité de ces opérations rôle du maître des détenteurs peuvent être affectés.  
  
Par exemple, supposons qu’une organisation crée des sites A, B, C, et des liens de sites D. existent entre A et B, entre B et C et entre C et D. la connectivité réseau exactement reflète la connectivité réseau des liens de sites. Dans cet exemple, toutes les opérations de rôles de maître sont placés dans site une et l’option pour **relier tous les liens de sites** n’est pas activée.  
  
Bien que cette configuration entraîne la réplication fonctionne entre tous les sites, les fonctions de rôle de maître d’opérations ont les limitations suivantes:  
  
-   Contrôleurs de domaine dans sites C et D ne peut pas accéder à l’émulateur de contrôleur de domaine principal sur le site A pour mettre à jour d’un mot de passe ou à un mot de passe qui a été récemment mis à jour.  
  
-   Contrôleurs de domaine dans sites C et D ne peut pas accéder le maître RID dans le site A pour obtenir un pool RID initial après l’installation d’ActiveDirectory et pour actualiser les pools RID comme ils deviennent épuisés.  
  
-   Contrôleurs de domaine dans sites C et D ne peut pas ajouter ou supprimer des partitions d’application personnalisée, DNS ou active.  
  
-   Contrôleurs de domaine dans sites C et D ne peuvent pas apporter des modifications de schéma.  
  
Pour plus d’une feuille de calcul pour vous aider à planifier le placement de rôle de maître d’opérations, consultez aide-mémoire pour [Kit de déploiement de Windows Server2003](https://go.microsoft.com/fwlink/?LinkID=102558), Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip de télécharger et d’ouvrir la sélection élective du contrôleur de domaine (DSSTOPO_4.doc).  
  
Vous devrez faire référence à ces informations lorsque vous créez le domaine racine de forêt et les domaines régionaux. Pour plus d’informations sur le déploiement du domaine racine de forêt, consultez Déploiement un [déploiement d’un domaine racine de forêt Windows Server2008](https://technet.microsoft.com/library/cc731174.aspx). Pour plus d’informations sur le déploiement de domaines régionaux, voir [déploiement de Windows Server2008domaines régionaux](https://technet.microsoft.com/library/cc755118.aspx).  
  


