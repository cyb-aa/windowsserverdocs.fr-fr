---
ms.assetid: bd64a766-5362-4f29-b963-5465c2bb79e7
title: Planification de l’emplacement des rôles de maître d’opérations
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 990f93d44189a6061653d5e190a176b049a280c4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822102"
---
# <a name="planning-operations-master-role-placement"></a>Planification de l’emplacement des rôles de maître d’opérations

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Active Directory Domain Services (AD DS) prend en charge la réplication multimaître des données d’annuaire, ce qui signifie que n’importe quel contrôleur de domaine peut accepter des modifications d’annuaire et répliquer les modifications sur tous les autres contrôleurs de domaine. Toutefois, certaines modifications, telles que les modifications de schéma, sont impossibles à effectuer dans un mode multimaître. Pour cette raison, certains contrôleurs de domaine, connus sous le nom de maîtres d’opérations, détiennent des rôles chargés d’accepter des demandes pour certaines modifications spécifiques.  
  
> [!NOTE]  
> Les détenteurs du rôle de maître d’opérations doivent être en mesure d’écrire des informations dans la base de données Active Directory. En raison de la nature en lecture seule de la base de données Active Directory sur un contrôleur de domaine en lecture seule (RODC), les contrôleurs de domaine en lecture seule **ne peuvent pas jouer le rôle de détenteur du rôle de maître d’opérations**.  
  
Trois rôles de maître d’opérations (également appelés opérations à maître unique flottant ou FSMO) existent dans chaque domaine :  
  
- Le maître d’opérations de l’émulateur de contrôleur de domaine principal traite toutes les mises à jour de mot de passe.  

- Le maître d’opérations RID (relative ID) conserve le pool RID global pour le domaine et alloue des pools RID locaux à tous les contrôleurs de domaine pour s’assurer que tous les principaux de sécurité créés dans le domaine ont un identificateur unique.  
- Le maître d’opérations d’infrastructure pour un domaine donné gère une liste des principaux de sécurité d’autres domaines qui sont membres de groupes au sein de son domaine.  

Outre les trois rôles de maître d’opérations au niveau du domaine, deux rôles de maître d’opérations existent dans chaque forêt :  
  
- Le maître d’opérations de schéma régit les modifications apportées au schéma.  
- Le maître d’opérations d’attribution de noms de domaine ajoute et supprime des domaines et d’autres partitions d’annuaire (par exemple, les partitions d’application DNS (Domain Name System)) vers et depuis la forêt.  
  
Placez les contrôleurs de domaine qui hébergent ces rôles de maître d’opérations dans les domaines où la fiabilité du réseau est élevée, et assurez-vous que l’émulateur de contrôleur de domaine principal et le maître RID sont toujours disponibles.  
  
Les détenteurs du rôle de maître d’opérations sont attribués automatiquement lors de la création du premier contrôleur de domaine dans un domaine donné. Les deux rôles au niveau de la forêt (contrôleur de schéma et maître d’attribution de noms de domaine) sont affectés au premier contrôleur de domaine créé dans une forêt. En outre, les trois rôles au niveau du domaine (maître RID, maître d’infrastructure et émulateur PDC) sont affectés au premier contrôleur de domaine créé dans un domaine.  
  
> [!NOTE]  
> Les attributions de détenteur du rôle de maître d’opérations automatique sont effectuées uniquement lorsqu’un nouveau domaine est créé et lorsqu’un détenteur de rôle actuel est rétrogradé. Toutes les autres modifications apportées aux propriétaires de rôle doivent être initiées par un administrateur.  
  
Ces attributions de rôle de maître d’opérations automatique peuvent entraîner une utilisation très élevée du processeur sur le premier contrôleur de domaine créé dans la forêt ou le domaine. Pour éviter cela, affectez des rôles de maître d’opérations (transfert) à différents contrôleurs de domaine dans votre forêt ou domaine. Placez les contrôleurs de domaine qui hébergent les rôles de maître d’opérations dans les domaines où le réseau est fiable et où les maîtres d’opérations sont accessibles par tous les autres contrôleurs de domaine de la forêt.  
  
Vous devez également désigner des maîtres d’opérations de secours pour tous les rôles de maître d’opérations. Les maîtres d’opérations en attente sont des contrôleurs de domaine vers lesquels vous pouvez transférer les rôles de maître d’opérations en cas d’échec des détenteurs de rôle d’origine. Assurez-vous que les maîtres d’opérations en attente sont des partenaires de réplication directs des maîtres d’opérations réels.  
  
## <a name="planning-the-pdc-emulator-placement"></a>Planification de l’emplacement de l’émulateur de contrôleur de domaine principal

L’émulateur de contrôleur de domaine principal traite les modifications de mot de passe client. Un seul contrôleur de domaine joue le rôle d’émulateur de contrôleur de domaine principal dans chaque domaine de la forêt.  
  
Même si tous les contrôleurs de domaine sont mis à niveau vers Windows 2000, Windows Server 2003 et Windows Server 2008, et que le domaine fonctionne au niveau fonctionnel natif de Windows 2000, l’émulateur de contrôleur de domaine principal reçoit la réplication préférentielle des modifications de mot de passe effectuées par les autres contrôleurs de domaine du domaine. Si un mot de passe a été modifié récemment, la réplication de ce changement prend du temps sur chaque contrôleur de domaine du domaine. Si l’authentification d’ouverture de session échoue sur un autre contrôleur de domaine en raison d’un mot de passe incorrect, ce contrôleur de domaine transfère la demande d’authentification à l’émulateur PDC avant de décider d’accepter ou de refuser la tentative d’ouverture de session.  
  
Placez l’émulateur de contrôleur de domaine principal dans un emplacement qui contient un grand nombre d’utilisateurs de ce domaine pour les opérations de transfert de mot de passe, si nécessaire. En outre, assurez-vous que l’emplacement est bien connecté à d’autres emplacements pour réduire la latence de la réplication.  
  
Pour obtenir une feuille de calcul qui vous aide à documenter les informations relatives à l’emplacement où vous envisagez de placer les émulateurs de contrôleur de domaine principal et le nombre d’utilisateurs pour chaque domaine représenté dans chaque emplacement, consultez outils d’aide pour le kit de déploiement de Windows Server 2003 ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), téléchargez Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip et emplacement du contrôleur de domaine (DSSTOPO_4. doc).  
  
Vous devez faire référence aux informations sur les emplacements dans lesquels vous devez placer des émulateurs de contrôleur de domaine principal lorsque vous déployez des domaines régionaux. Pour plus d’informations sur le déploiement de domaines régionaux, voir [déploiement de domaines régionaux Windows Server 2008](https://technet.microsoft.com/library/cc755118.aspx).  
  
## <a name="requirements-for-infrastructure-master-placement"></a>Configuration requise pour le placement du maître d’infrastructure  

Le maître d’infrastructure met à jour les noms des principaux de sécurité à partir d’autres domaines ajoutés aux groupes dans son propre domaine. Par exemple, si un utilisateur d’un domaine est membre d’un groupe dans un deuxième domaine et que le nom de l’utilisateur est modifié dans le premier domaine, le deuxième domaine n’est pas notifié que le nom de l’utilisateur doit être mis à jour dans la liste d’appartenance du groupe. Étant donné que les contrôleurs de domaine dans un domaine ne répliquent pas les principaux de sécurité sur les contrôleurs de domaine d’un autre domaine, le deuxième domaine ne prend jamais connaissance de la modification en l’absence du maître d’infrastructure.  
  
Le maître d’infrastructure surveille constamment les appartenances aux groupes, en recherchant les principaux de sécurité des autres domaines. S’il en trouve un, il consulte le domaine de l’entité de sécurité pour vérifier que les informations sont mises à jour. Si les informations sont obsolètes, le maître d’infrastructure effectue la mise à jour, puis réplique la modification sur les autres contrôleurs de domaine de son domaine.  
  
Deux exceptions s’appliquent à cette règle. Premièrement, si tous les contrôleurs de domaine sont des serveurs de catalogue global, le contrôleur de domaine qui héberge le rôle de maître d’infrastructure n’est pas significatif, car les catalogues globaux répliquent les informations mises à jour quel que soit le domaine auquel ils appartiennent. Deuxièmement, si la forêt n’a qu’un seul domaine, le contrôleur de domaine qui héberge le rôle de maître d’infrastructure n’est pas significatif, car les principaux de sécurité des autres domaines n’existent pas.  
  
Ne placez pas le maître d’infrastructure sur un contrôleur de domaine qui est également un serveur de catalogue global. Si le maître d’infrastructure et le catalogue global se trouvent sur le même contrôleur de domaine, le maître d’infrastructure ne fonctionnera pas. Le maître d’infrastructure ne trouvera jamais de données obsolètes. par conséquent, il ne répliquera jamais les modifications apportées aux autres contrôleurs de domaine du domaine.  
  
## <a name="operations-master-placement-for-networks-with-limited-connectivity"></a>Emplacement du maître d’opérations pour les réseaux avec une connectivité limitée

Sachez que si votre environnement dispose d’un emplacement central ou d’un site hub dans lequel vous pouvez placer des détenteurs du rôle de maître d’opérations, certaines opérations de contrôleur de domaine qui dépendent de la disponibilité de ces conteneurs de rôles de maître d’opérations peuvent être affectées.  
  
Supposons, par exemple, qu’une organisation crée des sites A, B, C et D. des liens de sites existent entre A et B, entre B et C, et entre C et D. la connectivité réseau reflète exactement la connectivité réseau des liens de sites. Dans cet exemple, tous les rôles de maître d’opérations sont placés dans le site A et l’option permettant de **relier tous les liens de sites** n’est pas sélectionnée.  
  
Bien que cette configuration entraîne la réussite de la réplication entre tous les sites, les fonctions du rôle de maître d’opérations présentent les limitations suivantes :  
  
- Les contrôleurs de domaine dans les sites C et D ne peuvent pas accéder à l’émulateur de contrôleur de domaine principal dans le site A pour mettre à jour un mot de passe ou le vérifier pour un mot de passe qui a été récemment mis à jour.  
- Les contrôleurs de domaine dans les sites C et D ne peuvent pas accéder au maître RID du site A pour obtenir un pool RID initial après l’installation du Active Directory et actualiser les pools RID à mesure qu’ils sont épuisés.  
- Les contrôleurs de domaine dans les sites C et D ne peuvent pas ajouter ou supprimer des partitions d’applications d’annuaire, DNS ou personnalisées.  
- Les contrôleurs de domaine dans les sites C et D ne peuvent pas apporter de modifications au schéma.  
  
Pour obtenir une feuille de calcul pour vous aider à planifier le placement du rôle de maître d’opérations, consultez [les outils d’aide pour le kit de déploiement Windows Server 2003](https://go.microsoft.com/fwlink/?LinkID=102558), téléchargez Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip et le placement du contrôleur de domaine (DSSTOPO_4. doc).  
  
Vous devrez vous référer à ces informations lorsque vous créerez le domaine racine de forêt et les domaines régionaux. Pour plus d’informations sur le déploiement du domaine racine de la forêt, voir déploiement d’un [domaine racine de forêt Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx). Pour plus d’informations sur le déploiement de domaines régionaux, voir [déploiement de domaines régionaux Windows Server 2008](https://technet.microsoft.com/library/cc755118.aspx).  

## <a name="next-steps"></a>Étapes suivantes :

Vous trouverez des informations supplémentaires sur l’emplacement des rôles FSMO dans la rubrique de support [placement et optimisation du rôle FSMO sur les contrôleurs de domaine Active Directory](https://support.microsoft.com/help/223346)
