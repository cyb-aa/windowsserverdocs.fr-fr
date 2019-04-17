---
ms.assetid: 864ad4bc-8428-4a8b-8671-cb93b68b0c03
title: "Ce qui réduit la Surface d’attaque ActiveDirectory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b2de254076b10a1a75d658f006c2245d523de6b7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="reducing-the-active-directory-attack-surface"></a>Ce qui réduit la Surface d’attaque ActiveDirectory

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette section se concentre sur les contrôles techniques pour mettre en œuvre pour réduire la surface d’attaque de l’installation d’ActiveDirectory. La section contient les informations suivantes:  
  
-   [Implémentation de modèles d’administration de moindre privilège](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md) se concentre sur l’identification le risque que l’utilisation des comptes dotés de privilèges élevés pour l’administration quotidienne présente, en plus de fournir des recommandations pour mettre en œuvre pour réduire le risque que présente des comptes privilégiés.  
  
-   [Implémentation des hôtes d’administration sécurisés](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md) décrit les principes de déploiement de systèmes d’administration dédiés, en plus des exemples d’approche à un déploiement de l’hôte d’administration sécurisé.  
  
-   [Sécurisation des contrôleurs de domaine contre les attaques](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md) traite les stratégies et les paramètres qui, bien que similaire aux recommandations pour l’implémentation des hôtes d’administration sécurisés, contiennent des recommandations spécifique du contrôleur de domaine pour s’assurer que les contrôleurs de domaine et les systèmes utilisés pour les gérer sont bien sécurisés.  
  
## <a name="privileged-accounts-and-groups-in-active-directory"></a>Comptes privilégiés et groupes dans ActiveDirectory  
Cette section fournit des informations générales sur les comptes privilégiés et groupes dans ActiveDirectory destinée à expliquer les points communs et les différences entre les comptes privilégiés et groupes dans ActiveDirectory. En analysant ces différences, si vous implémentez les recommandations de [implémentation de modèles d’administration de moindre privilège](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md) textuel ou choisir de les adapter à votre organisation, vous avez les outils que vous devez sécuriser chaque groupe et compte de manière appropriée.  
  
### <a name="built-in-privileged-accounts-and-groups"></a>Intégré privilégié des comptes et groupes  
ActiveDirectory facilite la délégation de l’administration et prend en charge le principe du privilège minimum lors de l’attribution des droits et autorisations. «Normal» aux utilisateurs qui disposent de comptes dans un domaine sont, par défaut, en mesure de lire plus grande partie de ce qui est stocké dans le répertoire, mais sont en mesure de modifier uniquement un ensemble très limité de données dans le répertoire. Les utilisateurs qui nécessitent des privilèges supplémentaires peuvent être accordées à l’appartenance à différents groupes «privilégiés» qui sont intégrés dans le répertoire afin qu’ils peuvent effectuer des tâches spécifiques liées à leur rôle, mais ne peut pas effectuer des tâches qui ne sont pas pertinentes pour leurs fonctions. Les organisations peuvent également créer des groupes sont adaptées aux responsabilités spécifiques et bénéficient de droits granulaires et les autorisations qui permettent le personnel informatique effectuer des tâches d’administration quotidiennes sans accorder des droits et autorisations qui dépassent ce qui est requis pour ces fonctions.  
  
Dans ActiveDirectory, trois groupes intégrés sont les groupes de privilèges plus élevés dans le répertoire: administrateurs, Admins du domaine et administrateurs de l’entreprise. La configuration par défaut et les fonctions de chacun de ces groupes sont décrites dans les sections suivantes:  
  
#### <a name="highest-privilege-groups-in-active-directory"></a>Groupes de privilèges plus élevés dans ActiveDirectory  
  
##### <a name="enterprise-admins"></a>Administrateurs de l’entreprise  
Administrateurs de l’entreprise (EA) est un groupe qui existe uniquement dans le domaine racine de forêt, et par défaut, il est membre du groupe Administrateurs dans tous les domaines dans la forêt. Le compte administrateur intégré dans le domaine racine de forêt est le seul membre par défaut du groupe EA. EAs sont droits et autorisations accordés les autoriser à implémenter des changements de forêt (autrement dit, les modifications qui affectent tous les domaines de la forêt), telles que l’ajout ou suppression de domaines, établissement des approbations de forêt ou augmenter les niveaux fonctionnels de forêt. Dans un modèle de délégation correctement conçu et implémenté, l’appartenance EA est requis uniquement lors de la construction tout d’abord la forêt ou lorsque vous effectuez certaines modifications à la forêt telles que l’établissement d’une approbation de forêt sortante. La plupart des droits et autorisations accordées au groupe EA peut être déléguée à des groupes et utilisateurs de moindre privilège.  
  
##### <a name="domain-admins"></a>Admins du domaine  

Chaque domaine dans une forêt possède son propre groupe Admins du domaine (DA), qui est membre du groupe des administrateurs de ce domaine et un membre du groupe Administrateurs local sur chaque ordinateur est joint au domaine. Le seul membre par défaut du groupe DA pour un domaine est le compte administrateur intégré pour ce domaine. DAs sont «si» au sein de leurs domaines, tandis que EAs ont des privilèges de la forêt. Dans un modèle de délégation correctement conçu et implémenté, l’appartenance au domaine Administrateurs convient uniquement dans les scénarios de «coupure de verre» (par exemple, des situations dans lesquelles un compte avec des privilèges élevés sur chaque ordinateur dans le domaine est nécessaire). Bien que les mécanismes de délégation ActiveDirectory natifs autorisent la délégation dans la mesure où il est possible d’utiliser des comptes DA uniquement dans les scénarios d’urgence, créant un modèle de délégation effective peut prendre beaucoup de temps et des outils tiers pour accélérer le processus de tirer parti de nombreuses organisations.  
  
##### <a name="administrators"></a>Administrateurs  
Le groupe tiers est intégré Administrateurs (BA) groupe de domaine local dans lequel sont imbriqués DAs et EAs. Ce groupe dispose des autorisations de nombreuses directs droits et autorisations dans le répertoire et sur les contrôleurs de domaine. Toutefois, le groupe Administrateurs pour un domaine dispose d’aucun privilège sur les serveurs membres ou sur les stations de travail. Il est via l’appartenance au groupe Administrateurs local des ordinateurs disposant de privilèges locaux.  
  
> [!NOTE]  
> Bien que ces conditions sont les configurations par défaut de ces groupes privilégiés, un membre d’un des trois groupes peut manipuler l’active pour adhérer à un des autres groupes. Dans certains cas, il est très facile à obtenir d’autres groupes d’appartenance dans d’autres, il est plus difficile, mais du point de vue de privilège potentiel, tous les trois groupes doivent être considérées comme équivalentes efficacement.  
  
##### <a name="schema-admins"></a>Administrateurs du schéma  

Un quatrième privilégié, groupe d’administrateurs du schéma (SA) existe uniquement dans le domaine racine de forêt et qu’il dispose uniquement de ce domaine compte administrateur intégré en tant que membre par défaut, similaire au groupe Administrateurs de l’entreprise. Le groupe Administrateurs du schéma est destiné à être remplies uniquement temporairement et parfois (lors de la modification du schéma ADDS est requise).  
  
Bien que le groupe de l’association de sécurité est le seul groupe que vous pouvez modifier le schéma ActiveDirectory (ce Salomon, structures de données sous-jacentes du répertoire tels que les objets et attributs), l’étendue des droits et des autorisations du groupe d’association de sécurité est plus limitée que les groupes décrites précédemment. Il est également courant de trouver qu’organisations ont développé des pratiques appropriées pour la gestion de l’appartenance du groupe d’association de sécurité, car l’appartenance au groupe est généralement rarement nécessaire et uniquement pour de courtes périodes. Cela est vrai techniquement des EA et DA BA groupes dans ActiveDirectory, ainsi, mais il est beaucoup moins courant de trouver que les organisations ont implémenté des pratiques similaires pour ces groupes que pour le groupe d’association de sécurité.  
  
#### <a name="protected-accounts-and-groups-in-active-directory"></a>Comptes protégés et groupes dans ActiveDirectory  
Dans ActiveDirectory, un ensemble de comptes privilégiés et groupes par défaut appelé «protégés» des comptes et groupes sont sécurisés différemment des autres objets dans l’annuaire. Les comptes dont l’appartenance directe ou transitive dans n’importe quel groupe protégé (quelles que soient indique si l’appartenance est dérivé de groupes de sécurité ou de distribution) hérite de cette sécurité restreint.  

  
Par exemple, si un utilisateur est membre d’un groupe de distribution, à son tour, un membre d’un groupe dans ActiveDirectory, cet objet utilisateur protégé est marqué comme un compte protégé. Lorsqu’un compte est marqué comme un compte protégé, la valeur de l’attribut adminCount sur l’objet est définie sur 1.  
  
> [!NOTE]
> Bien que transitive appartenance à un groupe protégé inclut distribution imbriqué et des groupes de sécurité imbriqués, les comptes qui sont membres de groupes de distribution imbriqués recevra pas SID protégé du groupe dans leur jeton d’accès. Toutefois, les groupes de distribution peuvent être converties aux groupes de sécurité dans ActiveDirectory, c’est pourquoi les groupes de distribution sont inclus dans l’énumération des membres des groupes protégés. Un groupe de distribution imbriquées protégé doit jamais être converti en groupe de sécurité, les comptes qui sont membres du groupe de distribution ancien par la suite recevront le parent protégé SID de groupe dans leur jeton d’accès à la prochaine ouverture de session.  
  
Le tableau suivant répertorie les comptes par défaut protégé et les groupes dans ActiveDirectory par le niveau du système d’exploitation version et le service pack.  
  
**Comptes protégés et groupes dans ActiveDirectory par le système d’exploitation et Version du Service Pack (SP) par défaut**  
  
|||||  
|-|-|-|-|  
|**Windows 2000 < SP4**|**Windows2000SP4-Windows Server2003**|**Windows Server 2003 SP1 +**|**Windows Server2008 - Windows Server2012**|  
|Administrateurs|Opérateurs de compte|Opérateurs de compte|Opérateurs de compte|  
||Administrateur|Administrateur|Administrateur|  
||Administrateurs|Administrateurs|Administrateurs|  
|Admins du domaine|Opérateurs de sauvegarde|Opérateurs de sauvegarde|Opérateurs de sauvegarde|  
||Éditeurs de certificats|||  
||Admins du domaine|Admins du domaine|Admins du domaine|  
|Administrateurs de l’entreprise|Contrôleurs de domaine|Contrôleurs de domaine|Contrôleurs de domaine|  
||Administrateurs de l’entreprise|Administrateurs de l’entreprise|Administrateurs de l’entreprise|  
||Krbtgt|Krbtgt|Krbtgt|  
||Opérateurs d’impression|Opérateurs d’impression|Opérateurs d’impression|  
||||Contrôleurs de domaine en lecture seule|  
||Duplicateurs|Duplicateurs|Duplicateurs|  
|Administrateurs du schéma|Administrateurs du schéma||Administrateurs du schéma|  
  
##### <a name="adminsdholder-and-sdprop"></a>AdminSDHolder et SDProp  
Dans le conteneur système de chaque domaine ActiveDirectory, un objet appelé AdminSDHolder est créé automatiquement. L’objectif de l’objet AdminSDHolder est pour vous assurer que les autorisations sur les comptes protégés et les groupes sont appliquées cohérente, quelle que soit l’où les comptes et groupes protégés sont trouvent dans le domaine.  

Toutes les 60minutes (par défaut), un processus appelé propagateur de descripteurs de sécurité (SDProp) s’exécute sur le contrôleur de domaine qui détient le rôle d’émulateur de contrôleur de domaine principal du domaine. SDProp compare les autorisations sur l’objet AdminSDHolder du domaine avec les autorisations sur les comptes protégés et les groupes dans le domaine. Si les autorisations sur un des comptes protégés et des groupes ne correspondent pas les autorisations sur l’objet AdminSDHolder, les autorisations sur les comptes protégés et les groupes sont rétablies pour correspondre à celles de l’objet AdminSDHolder du domaine.  
  
L’héritage des autorisations est désactivé sur les groupes protégés et les comptes, ce qui signifie que même si les comptes ou les groupes sont déplacés vers différents emplacements dans le répertoire, ils n’héritent pas des autorisations de leurs nouveaux objets parents. L’héritage est également désactivé sur l’objet AdminSDHolder afin que les modifications apportées aux autorisations pour les objets parents ne modifiez pas les autorisations de AdminSDHolder.  
  
> [!NOTE]
> Lorsqu’un compte est supprimé d’un groupe protégé, il est n’est plus considéré comme un compte protégé, mais son reste d’attribut adminCount définie sur 1 s’il n’est pas modifié manuellement. Le résultat de cette configuration est que les ACL de l’objet est n’est plus mis à jour SDProp, mais l’objet toujours n’hérite pas des autorisations de son objet parent. Par conséquent, l’objet peut se trouver dans une unité d’organisation (UO) à laquelle les autorisations ont été déléguées, mais l’objet protégé anciennement hérite ces autorisations déléguées. Un script pour localiser et réinitialiser des objets précédemment protégés dans le domaine se trouve dans le [article du Support Microsoft817433](https://support.microsoft.com/?id=817433).  
  
###### <a name="adminsdholder-ownership"></a>Propriété AdminSDHolder  
La plupart des objets dans ActiveDirectory sont détenus par le groupe du domaine BA. Toutefois, l’objet AdminSDHolder est, par défaut, détenu par le groupe du domaine DA. (Il s’agit d’une situation dans laquelle DAs ne dérivent pas leurs droits et autorisations via l’appartenance au groupe Administrateurs du domaine.)  
  
Dans les versions de Windows antérieures à Windows Server2008, les propriétaires d’un objet peuvent modifier les autorisations de l’objet, y compris eux-mêmes accorder des autorisations dont ils n’ont pas à l’origine. Par conséquent, les autorisations par défaut sur l’objet AdminSDHolder d’un domaine empêchent les utilisateurs qui sont membres de groupes BA ou EA de modifier les autorisations pour l’objet AdminSDHolder d’un domaine. Toutefois, les membres du groupe Administrateurs du domaine peuvent prendre possession de l’objet et s’accorder des autorisations supplémentaires, ce qui signifie que cette protection est fondamentale et protège uniquement l’objet contre toute modification accidentelle par les utilisateurs qui ne sont pas membres du groupe DA dans le domaine. En outre, le BA et EA (le cas échéant) groupes sont autorisés à modifier les attributs de l’objet AdminSDHolder dans le domaine local (le domaine racine pour EA).  
  
> [!NOTE]  
> Un attribut sur l’objet AdminSDHolder, dSHeuristics, permet une personnalisation limitée (suppression) des groupes qui sont considérés comme des groupes protégés et sont affectés par AdminSDHolder et SDProp. Cette personnalisation doit être soigneusement si elle est implémentée, bien qu’il existe des circonstances valides dans laquelle la modification de dSHeuristics sur AdminSDHolder est utile. Vous trouverez plus d’informations sur la modification de l’attribut dSHeuristics sur un objet AdminSDHolder dans les articles du Support Microsoft [817433](https://support.microsoft.com/?id=817433) et [973840](https://support.microsoft.com/kb/973840)et dans [annexe c: des comptes protégés et groupes dans ActiveDirectory](Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
Bien que les groupes plus privilégiés dans ActiveDirectory sont décrites ici, il existe un nombre d’autres groupes qui ont été accordées élevés des niveaux de privilèges. Pour plus d’informations sur tous les groupes intégrés dans ActiveDirectory et les droits d’utilisateur attribués à chaque et par défaut, voir [annexe b: des comptes privilégiés et groupes dans ActiveDirectory](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md).  
  


