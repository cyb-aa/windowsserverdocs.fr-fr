---
ms.assetid: 864ad4bc-8428-4a8b-8671-cb93b68b0c03
title: Réduire la Surface d'attaque Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d692641d316b5fe7206cc3f413bdcfc9b74675b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874150"
---
# <a name="reducing-the-active-directory-attack-surface"></a>Réduire la Surface d'attaque Active Directory

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette section se concentre sur les contrôles techniques à implémenter pour réduire la surface d’attaque de l’installation d’Active Directory. La section contient les informations suivantes :  
  
-   [Implémentation de modèles d’administration de moindre privilège](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md) se concentre sur l’identification des risques qui l’utilisation de privilèges très élevée des comptes pour l’administration quotidienne est présente, en plus de fournir des recommandations visant à implémenter pour réduire le risque Ce privilège comptes présents.  
  
-   [Implémentation des hôtes d’administration sécurisés](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md) décrit les principes de déploiement de systèmes d’administration dédiés et sécurisés, en plus des exemples de méthodes pour un déploiement de l’hôte d’administration sécurisées.  
  
-   [Sécurisation des contrôleurs de domaine contre les attaques](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md) traite des stratégies et des paramètres qui, bien que similaires pour les recommandations pour l’implémentation des hôtes d’administration sécurisés, contiennent certaines recommandations spécifique du contrôleur de domaine pour aider à Assurez-vous que les contrôleurs de domaine et les systèmes utilisés pour les gérer sont bien sécurisées.  
  
## <a name="privileged-accounts-and-groups-in-active-directory"></a>Comptes privilégiés et groupes dans Active Directory  
Cette section fournit des informations générales sur les comptes privilégiés et groupes dans Active Directory pour vocation d’expliquer les points communs et les différences entre les comptes privilégiés et groupes dans Active Directory. En comprenant ces distinctions, si vous implémentez les recommandations de [implémentation de modèles d’administration doté des privilèges minimaux](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md) textuelle ou choisir de les personnaliser pour votre organisation, vous avez les outils nécessaires pour sécuriser correctement chaque groupe et un compte.  
  
### <a name="built-in-privileged-accounts-and-groups"></a>Intégré privilégié des comptes et groupes  
Active Directory facilite la délégation de l’administration et prend en charge le principe du moindre privilège lors de l’attribution des droits et autorisations. « Regular » aux utilisateurs qui disposent de comptes dans un domaine sont, par défaut, en mesure de lire une grande partie de ce qui est stocké dans le répertoire, mais sont en mesure de modifier uniquement un ensemble très limité de données dans le répertoire. Les utilisateurs qui nécessitent des privilèges supplémentaires peuvent être accordées à l’appartenance aux groupes « privilégiés » différents qui sont intégrées à l’annuaire afin qu’ils peuvent effectuer des tâches spécifiques liées à leurs rôles, mais ne peut pas effectuer des tâches qui ne sont pas pertinentes pour leurs tâches. Les organisations peuvent également créer des groupes qui sont adaptées aux responsabilités spécifique et qui disposent des droits granulaires et les autorisations qui permettent le personnel informatique effectuer des fonctions d’administration quotidiennes sans accorder de droits et autorisations qui dépassent ce qui est requis pour ces fonctions.  
  
Dans Active Directory, trois groupes intégrés sont les groupes de privilège le plus élevés dans le répertoire : Administrateurs de l’entreprise, Admins du domaine et administrateurs. La configuration par défaut et les fonctionnalités de chacun de ces groupes sont décrites dans les sections suivantes :  
  
#### <a name="highest-privilege-groups-in-active-directory"></a>Groupes de privilège le plus élevés dans Active Directory  
  
##### <a name="enterprise-admins"></a>Administrateurs de l’entreprise  
Administrateurs de l’entreprise (EA) est un groupe qui existe uniquement dans le domaine racine de forêt, et par défaut, il s’agit d’un membre du groupe Administrateurs dans tous les domaines dans la forêt. Le compte administrateur intégré dans le domaine racine de forêt est le seul membre par défaut du groupe EA. EAs bénéficient de droits et autorisations qui leur permettent d’implémenter des modifications à la forêt (autrement dit, les modifications qui affectent tous les domaines de la forêt), comme l’ajout ou suppression de domaines, l’établissement d’approbations de forêt ou augmentation des niveaux fonctionnels de forêt. Dans un modèle de délégation correctement conçu et implémenté, l’appartenance au contrat entreprise est obligatoire uniquement lors de la construction de tout d’abord la forêt ou lorsque vous apportez certaines modifications de la forêt telles que l’établissement d’une approbation de forêt sortante. La plupart des droits et autorisations accordées au groupe de contrat entreprise peut être déléguée à des groupes et utilisateurs de moindre privilège.  
  
##### <a name="domain-admins"></a>Administrateurs du domaine  

Chaque domaine dans une forêt possède son propre groupe Admins du domaine (DA), qui est membre du groupe des administrateurs de ce domaine et un membre du groupe Administrateurs local sur chaque ordinateur est joint au domaine. Le seul membre par défaut du groupe DA pour un domaine est le compte administrateur intégré pour ce domaine. DAs sont « créer » dans leurs domaines, tandis que EAs ont des privilèges de la forêt. Dans un modèle de délégation correctement conçu et implémenté, l’appartenance d’Admins du domaine doit être requise uniquement dans les scénarios de « secours » (par exemple, des situations dans lesquelles un compte avec des privilèges élevés sur chaque ordinateur dans le domaine est nécessaire). Bien que les mécanismes de délégation Active Directory natifs autorisent la délégation dans la mesure où il est possible d’utiliser des comptes de DA uniquement dans les scénarios d’urgence, la construction d’un modèle de délégation effective peut prendre du temps, et tirer parti de nombreuses organisations des outils tiers pour accélérer le processus.  
  
##### <a name="administrators"></a>Administrateurs  
Le troisième groupe est intégrée aux administrateurs (BA) groupe de domaine local dans lequel sont imbriqués DAs et EAs. Ce groupe est accordé à la plupart des droits d’accès directs et des autorisations dans le répertoire et sur les contrôleurs de domaine. Toutefois, le groupe Administrateurs pour un domaine dispose d’aucun privilège sur les serveurs membres ou sur les stations de travail. Il est par le biais de l’appartenance au groupe Administrateurs local des ordinateurs disposant de privilèges locale.  
  
> [!NOTE]  
> Bien que ce sont les configurations par défaut de ces groupes privilégiés, un membre d’un des trois groupes peut manipuler le répertoire pour devenir membre d’aucun des autres groupes. Dans certains cas, il est facile d’obtenir les adhésions aux autres groupes, tandis que dans d’autres cas, il est plus difficile, mais du point de vue du privilège potentiel, tous les trois groupes doivent être considérées comme équivalentes efficacement.  
  
##### <a name="schema-admins"></a>Administrateurs du schéma  

Une quatrième privilégié de groupe, les administrateurs de schéma (SA), existe uniquement dans le domaine racine de forêt et dispose uniquement de ce domaine compte administrateur intégré en tant qu’un membre par défaut, comme le groupe Administrateurs de l’entreprise. Le groupe Administrateurs du schéma est destiné à être rempli uniquement temporairement et occasionnellement (lors de la modification du schéma AD DS est requise).  
  
Bien que le groupe de l’association de sécurité est le seul groupe qui peut modifier le schéma Active Directory (qui l’est., structures de données sous-jacentes de l’annuaire tels que les objets et attributs), la portée de droits et les autorisations du groupe administrateur système est plus limitée que décrit précédemment groupes. Il est également courant de trouver que les organisations ont développé des pratiques appropriées pour la gestion de l’appartenance du groupe administrateur système, car l’appartenance au groupe est généralement rarement nécessaire et uniquement pour de courtes périodes. Cela est techniquement vrai pour les clients EA, DA et BA groupes dans Active Directory, ainsi, mais il est beaucoup moins courant de trouver que les organisations ont implémenté des pratiques similaires pour ces groupes comme pour le groupe SA.  
  
#### <a name="protected-accounts-and-groups-in-active-directory"></a>Comptes protégés et groupes dans Active Directory  
Dans Active Directory, un ensemble de comptes privilégiés et groupes par défaut appelée « protégés » des comptes et groupes sont sécurisés différemment des autres objets dans l’annuaire. N’importe quel compte qui a une appartenance directe ou transitive dans n’importe quel groupe protégé (indépendamment de si l’appartenance est dérivée des groupes de sécurité ou de distribution) hérite de cette sécurité restreint.  

  
Par exemple, si un utilisateur est membre d’un groupe de distribution qui est, à son tour, membre d’un groupe dans Active Directory, cet objet utilisateur protégé est marqué comme un compte protégé. Quand un compte est marqué comme un compte protégé, la valeur de l’attribut adminCount sur l’objet est définie sur 1.  
  
> [!NOTE]
> Bien que transitive appartenance à un groupe protégé inclut la distribution imbriquée et des groupes de sécurité imbriqués, les comptes qui sont membres de groupes de distribution imbriqué recevra pas SID protégé du groupe dans leurs jetons d’accès. Toutefois, les groupes de distribution peuvent être convertis à des groupes de sécurité dans Active Directory, c’est pourquoi les groupes de distribution sont inclus dans l’énumération des membres des groupes protégés. Un groupe de distribution imbriqué protégé doit toujours être converti en un groupe de sécurité, les comptes qui sont membres du groupe de distribution précédent reçoit par la suite le parent protégé SID du groupe dans les jetons d’accès à la prochaine connexion.  
  
Le tableau suivant répertorie les comptes par défaut protégée et les groupes dans Active Directory par le niveau du système d’exploitation version et le service pack.  
  
**Comptes protégés et groupes dans Active Directory par le système d’exploitation et Version du Service Pack (SP) par défaut**  
  
|||||  
|-|-|-|-|  
|**Windows 2000 <SP4**|**Windows 2000 SP4-Windows Server 2003**|**Windows Server 2003 SP1 +**|**Windows Server 2008 - Windows Server 2012**|  
|Administrateurs|Opérateurs de compte|Opérateurs de compte|Opérateurs de compte|  
||Administrateur|Administrateur|Administrateur|  
||Administrateurs|Administrateurs|Administrateurs|  
|Administrateurs du domaine|Opérateurs de sauvegarde|Opérateurs de sauvegarde|Opérateurs de sauvegarde|  
||Éditeurs de certificats|||  
||Administrateurs du domaine|Administrateurs du domaine|Administrateurs du domaine|  
|Administrateurs de l’entreprise|Contrôleurs de domaine|Contrôleurs de domaine|Contrôleurs de domaine|  
||Administrateurs de l’entreprise|Administrateurs de l’entreprise|Administrateurs de l’entreprise|  
||Krbtgt|Krbtgt|Krbtgt|  
||Opérateurs d'impression|Opérateurs d'impression|Opérateurs d'impression|  
||||Contrôleurs de domaine en lecture seule|  
||Réplicateur|Réplicateur|Réplicateur|  
|Administrateurs du schéma|Administrateurs du schéma||Administrateurs du schéma|  
  
##### <a name="adminsdholder-and-sdprop"></a>AdminSDHolder et SDProp  
Dans le conteneur système de chaque domaine Active Directory, un objet appelé AdminSDHolder est automatiquement créé. L’objectif de l’objet AdminSDHolder consiste à vérifier que les autorisations sur les comptes protégés et groupes sont appliquées systématiquement, quel que soit l’où les comptes et groupes protégés se trouvent dans le domaine.  

Toutes les 60 minutes (par défaut), un processus appelé propagateur de descripteurs de sécurité (SDProp) s’exécute sur le contrôleur de domaine qui détient le rôle d’émulateur PDC du domaine. SDProp compare les autorisations sur l’objet AdminSDHolder du domaine avec les autorisations sur les comptes protégés et les groupes dans le domaine. Si les autorisations sur un des comptes protégés et des groupes ne correspondent pas les autorisations sur l’objet AdminSDHolder, les autorisations sur les comptes protégés et les groupes sont réinitialisées pour correspondre à celles de l’objet de AdminSDHolder du domaine.  
  
L’héritage des autorisations est désactivé sur les groupes protégés et les comptes, ce qui signifie que même si les comptes ou les groupes sont déplacés vers différents emplacements dans le répertoire, ils n’héritent pas des autorisations de leurs objets parents de nouveau. L’héritage est également désactivé sur l’objet AdminSDHolder afin que les modifications apportées aux autorisations pour les objets parents ne modifiez pas les autorisations de AdminSDHolder.  
  
> [!NOTE]
> Lorsqu’un compte est supprimé d’un groupe protégé, il est n’est plus considéré comme un compte protégé, mais son reste d’attribut adminCount défini sur 1 si elle n’est pas modifié manuellement. Le résultat de cette configuration est qu’ACL l’objet est ne sont plus mis à jour par SDProp, mais l’objet toujours n’hérite pas des autorisations de son objet parent. Par conséquent, l’objet peut se trouver dans une unité d’organisation (UO) à laquelle les autorisations ont été déléguées, mais l’objet précédemment protégée n’héritera pas ces autorisations déléguées. Vous trouverez un script pour localiser et de réinitialiser les objets précédemment protégées dans le domaine dans le [article du Support Microsoft 817433](https://support.microsoft.com/?id=817433).  
  
###### <a name="adminsdholder-ownership"></a>Propriété AdminSDHolder  
La plupart des objets dans Active Directory sont détenus par le groupe du domaine BA. Toutefois, l’objet AdminSDHolder est, par défaut, détenu par le groupe du domaine DA. (Il s’agit d’une circonstance dans laquelle DAs ne dérivent pas leurs droits et autorisations par le biais de l’appartenance au groupe Administrateurs du domaine.)  
  
Dans les versions de Windows antérieures à Windows Server 2008, les propriétaires d’un objet peuvent modifier des autorisations de l’objet, y compris s’octroyer des autorisations qu’ils n’ont pas à l’origine. Par conséquent, les autorisations par défaut sur l’objet AdminSDHolder d’un domaine empêchent les utilisateurs qui sont membres de groupes BA ou EA à partir de la modification des autorisations pour l’objet AdminSDHolder d’un domaine. Toutefois, les membres du groupe Administrateurs du domaine peuvent prendre possession de l’objet et s’accorder des autorisations supplémentaires, ce qui signifie que cette protection est rudimentaire protège uniquement l’objet contre les modifications accidentelles par des utilisateurs pas de membres du groupe DA dans le domaine. En outre, le BA et EA (le cas échéant) groupes sont autorisé à modifier les attributs de l’objet AdminSDHolder dans le domaine local (domaine racine pour EA).  
  
> [!NOTE]  
> Un attribut sur l’objet AdminSDHolder, dSHeuristics, permet une personnalisation limitée (suppression) des groupes qui sont considérés comme des groupes protégés et sont affectés par AdminSDHolder et SDProp. Cette personnalisation doit être soigneusement si elle est implémentée, bien qu’il existe des circonstances valides dans lequel la modification de dSHeuristics sur AdminSDHolder est utile. Vous trouverez plus d’informations sur la modification de l’attribut dSHeuristics sur un objet AdminSDHolder dans les articles de Support Microsoft [817433](https://support.microsoft.com/?id=817433) et [973840](https://support.microsoft.com/kb/973840)et dans [annexe c : Comptes et groupes dans Active Directory protégés](Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
Bien que les groupes plus privilégiés dans Active Directory sont décrites ici, il existe un nombre d’autres groupes qui ont été accordées élevés des niveaux de privilège. Pour plus d’informations sur tous les groupes intégrés dans Active Directory et les droits d’utilisateur affectés à chacune et par défaut, consultez [annexe b : Privilégié des comptes et groupes dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md).  
  


