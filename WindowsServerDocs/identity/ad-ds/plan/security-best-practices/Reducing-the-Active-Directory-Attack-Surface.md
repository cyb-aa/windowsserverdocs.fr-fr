---
ms.assetid: 864ad4bc-8428-4a8b-8671-cb93b68b0c03
title: Réduire la Surface d'attaque Active Directory
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: dcd0b412e7a0005bc6574638e0f6fce4554c6487
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80821052"
---
# <a name="reducing-the-active-directory-attack-surface"></a>Réduire la Surface d'attaque Active Directory

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette section se concentre sur les contrôles techniques à implémenter pour réduire la surface d’attaque de l’installation Active Directory. La section contient les informations suivantes :  
  
-   L' [implémentation de modèles d’administration à privilèges moindres](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md) porte sur l’identification du risque lié à l’utilisation de comptes dotés de privilèges élevés pour une administration quotidienne, en plus de fournir des recommandations pour la mise en œuvre afin de réduire le risque que les comptes privilégiés présentent.  
  
-   [Implémentation d’ordinateurs hôtes d’administration sécurisés](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md) décrit les principes de déploiement de systèmes d’administration sécurisés et dédiés, en plus de quelques exemples d’approche du déploiement d’un hôte d’administration sécurisé.  
  
-   La [sécurisation des contrôleurs de domaine contre les attaques porte sur](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md) les stratégies et les paramètres qui, bien qu’ils soient similaires aux recommandations en matière d’implémentation d’ordinateurs hôtes d’administration sécurisés, contiennent certaines recommandations spécifiques au contrôleur de domaine pour s’assurer que les contrôleurs de domaine et les systèmes utilisés pour les gérer sont bien sécurisés.  
  
## <a name="privileged-accounts-and-groups-in-active-directory"></a>Comptes et groupes privilégiés dans Active Directory  
Cette section fournit des informations générales sur les comptes et les groupes privilégiés dans Active Directory destinés à expliquer les points communs et les différences entre les comptes et les groupes privilégiés dans Active Directory. En comprenant ces distinctions, que vous implémentiez les recommandations en matière d' [implémentation de modèles d’administration à privilèges faibles](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md) textuellement ou que vous choisissiez de les personnaliser pour votre organisation, vous disposez des outils dont vous avez besoin pour sécuriser chaque groupe et compte de manière appropriée.  
  
### <a name="built-in-privileged-accounts-and-groups"></a>Comptes et groupes privilégiés intégrés  
Active Directory facilite la délégation de l’administration et prend en charge le principe des privilèges minimum pour l’attribution de droits et d’autorisations. Les utilisateurs « normaux » qui ont des comptes dans un domaine sont, par défaut, capables de lire une grande partie de ce qui est stocké dans l’annuaire, mais sont en mesure de modifier uniquement un ensemble très limité de données dans l’annuaire. Les utilisateurs qui requièrent des privilèges supplémentaires peuvent se voir accorder l’appartenance à différents groupes « privilégiés » intégrés à l’annuaire pour qu’ils puissent effectuer des tâches spécifiques liées à leurs rôles, mais ne peuvent pas effectuer des tâches qui ne sont pas pertinentes pour leurs tâches. Les organisations peuvent également créer des groupes qui sont adaptés à des responsabilités de travail spécifiques et qui disposent de droits et d’autorisations granulaires qui permettent au personnel informatique d’effectuer des fonctions d’administration quotidiennes sans accorder de droits et d’autorisations qui dépassent ce qui est requis pour ces fonctions.  
  
Dans Active Directory, trois groupes prédéfinis sont les groupes de privilèges les plus élevés dans le répertoire : administrateurs de l’entreprise, admins du domaine et administrateurs. La configuration et les fonctionnalités par défaut de chacun de ces groupes sont décrites dans les sections suivantes :  
  
#### <a name="highest-privilege-groups-in-active-directory"></a>Groupes de privilèges les plus élevés dans Active Directory  
  
##### <a name="enterprise-admins"></a>Administrateurs de l'entreprise  
Administrateurs de l’entreprise (EA) est un groupe qui existe uniquement dans le domaine racine de la forêt. par défaut, il est membre du groupe administrateurs dans tous les domaines de la forêt. Le compte administrateur intégré dans le domaine racine de forêt est le seul membre par défaut du groupe EA. EAs disposent de droits et d’autorisations leur permettant d’implémenter des modifications à l’ensemble de la forêt (c’est-à-dire des modifications qui affectent tous les domaines de la forêt), telles que l’ajout ou la suppression de domaines, l’établissement d’approbations de forêt ou le déclenchement de niveaux fonctionnels de forêt. Dans un modèle de délégation correctement conçu et implémenté, l’appartenance EA est requise uniquement lors de la première construction de la forêt ou lors de l’exécution de certaines modifications à l’ensemble de la forêt, telles que l’établissement d’une approbation de forêt sortante. La plupart des droits et autorisations accordés au groupe EA peuvent être délégués à des utilisateurs et des groupes à privilèges moindres.  
  
##### <a name="domain-admins"></a>Admins du domaine  

Chaque domaine d’une forêt possède son propre groupe Admins du domaine (DA), qui est membre du groupe administrateurs de ce domaine et d’un membre du groupe Administrateurs local sur chaque ordinateur joint au domaine. Le seul membre par défaut du groupe DA pour un domaine est le compte administrateur intégré pour ce domaine. Les DAs sont « tout-puissants » dans leurs domaines, tandis que EAs disposent de privilèges à l’ensemble de la forêt. Dans un modèle de délégation correctement conçu et implémenté, l’appartenance aux administrateurs de domaine doit être requise uniquement dans les scénarios de « coupure » (par exemple, dans les situations où un compte avec des niveaux élevés de privilèges sur chaque ordinateur du domaine est nécessaire). Bien que les mécanismes de délégation Active Directory natifs permettent à la délégation dans la mesure où il est possible d’utiliser des comptes DA uniquement dans des scénarios d’urgence, la construction d’un modèle de délégation efficace peut prendre du temps, et de nombreuses organisations tirent parti des outils tiers pour accélérer le processus.  
  
##### <a name="administrators"></a>Administrateurs  
Le troisième groupe est le groupe d’administrateurs de domaine local intégré dans lequel les environnements DAs et EAs sont imbriqués. Ce groupe reçoit de nombreux droits et autorisations directs dans le répertoire et sur les contrôleurs de domaine. Toutefois, le groupe administrateurs d’un domaine ne dispose pas de privilèges sur les serveurs membres ou sur les stations de travail. Elle s’effectue via l’appartenance au groupe Administrateurs local des ordinateurs, auquel le privilège local est accordé.  
  
> [!NOTE]  
> Bien qu’il s’agisse des configurations par défaut de ces groupes privilégiés, un membre de l’un des trois groupes peut manipuler l’annuaire pour être membre de l’un des autres groupes. Dans certains cas, il est facile d’obtenir l’appartenance aux autres groupes, tandis que dans d’autres, il est plus difficile, mais du point de vue des privilèges potentiels, les trois groupes doivent être considérés comme équivalents de manière efficace.  
  
##### <a name="schema-admins"></a>Administrateurs du schéma  

Un quatrième groupe privilégié, administrateurs de schéma (SA), existe uniquement dans le domaine racine de forêt et n’a que le compte administrateur intégré de ce domaine comme membre par défaut, comme le groupe administrateurs de l’entreprise. Le groupe administrateurs du schéma est destiné à être rempli uniquement temporairement et occasionnellement (lorsque la modification du schéma AD DS est requise).  
  
Même si le groupe SA est le seul à pouvoir modifier le schéma Active Directory (c’est-à-dire, les structures de données sous-jacentes de l’annuaire, telles que les objets et les attributs), l’étendue des droits et autorisations du groupe SA est plus limitée que les groupes décrits précédemment. Il est également courant de constater que les organisations ont développé des pratiques appropriées pour la gestion de l’appartenance au groupe SA, car l’appartenance au groupe n’est généralement pas nécessaire, et uniquement pendant de courtes périodes. Cela est techniquement vrai des groupes EA, DA et BA dans Active Directory, mais il est beaucoup moins courant de savoir que les organisations ont implémenté des pratiques similaires pour ces groupes comme pour le groupe SA.  
  
#### <a name="protected-accounts-and-groups-in-active-directory"></a>Comptes et groupes protégés dans Active Directory  
Dans Active Directory, un ensemble par défaut de comptes et de groupes privilégiés appelés comptes et groupes protégés est sécurisé différemment des autres objets de l’annuaire. Tout compte ayant une appartenance directe ou transitive à un groupe protégé (que l’appartenance soit dérivée de la sécurité ou des groupes de distribution) hérite de cette sécurité restreinte.  

  
Par exemple, si un utilisateur est membre d’un groupe de distribution qui est, à son tour, membre d’un groupe protégé dans Active Directory, cet objet utilisateur est marqué comme étant un compte protégé. Lorsqu’un compte est marqué comme compte protégé, la valeur de l’attribut adminCount sur l’objet est définie sur 1.  
  
> [!NOTE]
> Bien que l’appartenance transitive dans un groupe protégé comprenne des groupes de sécurité imbriqués et imbriqués, les comptes qui sont membres de groupes de distribution imbriqués ne recevront pas le SID du groupe protégé dans leurs jetons d’accès. Toutefois, les groupes de distribution peuvent être convertis en groupes de sécurité dans Active Directory, c’est pourquoi les groupes de distribution sont inclus dans l’énumération des membres du groupe protégé. Si un groupe de distribution imbriqué protégé est toujours converti en groupe de sécurité, les comptes qui sont membres du groupe de distribution précédent recevront par la suite le SID du groupe protégé parent dans leurs jetons d’accès à la prochaine ouverture de session.  
  
Le tableau suivant répertorie les comptes et les groupes protégés par défaut dans Active Directory par version du système d’exploitation et niveau de Service Pack.  
  
**Comptes et groupes protégés par défaut dans Active Directory par le système d’exploitation et la version du Service Pack (SP)**  
  
|||||  
|-|-|-|-|  
|**Windows 2000 < SP4**|**Windows 2000 SP4-Windows Server 2003**|**Windows Server 2003 SP1 +**|**Windows Server 2008-Windows Server 2012**|  
|Administrateurs|Opérateurs de compte|Opérateurs de compte|Opérateurs de compte|  
||Administrateur|Administrateur|Administrateur|  
||Administrateurs|Administrateurs|Administrateurs|  
|Admins du domaine|Opérateurs de sauvegarde|Opérateurs de sauvegarde|Opérateurs de sauvegarde|  
||Éditeurs de certificats|||  
||Admins du domaine|Admins du domaine|Admins du domaine|  
|Administrateurs de l'entreprise|Contrôleurs de domaine|Contrôleurs de domaine|Contrôleurs de domaine|  
||Administrateurs de l'entreprise|Administrateurs de l'entreprise|Administrateurs de l'entreprise|  
||Krbtgt|Krbtgt|Krbtgt|  
||Opérateurs d'impression|Opérateurs d'impression|Opérateurs d'impression|  
||||Contrôleurs de domaine en lecture seule|  
||Réplicateur|Réplicateur|Réplicateur|  
|Administrateurs du schéma|Administrateurs du schéma||Administrateurs du schéma|  
  
##### <a name="adminsdholder-and-sdprop"></a>AdminSDHolder et SDProp  
Dans le conteneur système de chaque domaine de Active Directory, un objet appelé AdminSDHolder est automatiquement créé. L’objectif de l’objet AdminSDHolder est de s’assurer que les autorisations sur les comptes et les groupes protégés sont appliquées de manière cohérente, quel que soit l’emplacement où se trouvent les groupes et les comptes protégés dans le domaine.  

Toutes les 60 minutes (par défaut), un processus appelé propagateur descripteur de sécurité (SDProp) s’exécute sur le contrôleur de domaine qui détient le rôle d’émulateur de contrôleur de domaine principal du domaine. SDProp compare les autorisations sur l’objet AdminSDHolder du domaine avec les autorisations sur les comptes et les groupes protégés dans le domaine. Si les autorisations sur l’un des comptes et groupes protégés ne correspondent pas aux autorisations sur l’objet AdminSDHolder, les autorisations sur les comptes et les groupes protégés sont réinitialisées pour correspondre à celles de l’objet AdminSDHolder du domaine.  
  
L’héritage des autorisations est désactivé sur les groupes et les comptes protégés, ce qui signifie que même si les comptes ou les groupes sont déplacés vers des emplacements différents dans l’annuaire, ils n’héritent pas des autorisations de leurs nouveaux objets parents. L’héritage est également désactivé sur l’objet AdminSDHolder afin que les modifications d’autorisations apportées aux objets parents ne modifient pas les autorisations de AdminSDHolder.  
  
> [!NOTE]
> Lorsqu’un compte est supprimé d’un groupe protégé, il n’est plus considéré comme un compte protégé, mais son attribut adminCount conserve la valeur 1 s’il n’est pas modifié manuellement. Le résultat de cette configuration est que les listes de contrôle d’accès de l’objet ne sont plus mises à jour par SDProp, mais l’objet n’hérite toujours pas des autorisations de son objet parent. Par conséquent, l’objet peut résider dans une unité d’organisation (UO) à laquelle les autorisations ont été déléguées, mais l’objet précédemment protégé n’héritera pas de ces autorisations déléguées. Un script pour rechercher et réinitialiser les objets précédemment protégés dans le domaine se trouve dans le [support Microsoft l’article 817433](https://support.microsoft.com/?id=817433).  
  
###### <a name="adminsdholder-ownership"></a>Propriété AdminSDHolder  
La plupart des objets de Active Directory sont détenus par le groupe BA du domaine. Toutefois, l’objet AdminSDHolder est, par défaut, détenu par le groupe DA du domaine. (Il s’agit d’une circonstance dans laquelle DAs ne dérive pas ses droits et autorisations via l’appartenance au groupe administrateurs du domaine.)  
  
Dans les versions de Windows antérieures à Windows Server 2008, les propriétaires d’un objet peuvent modifier les autorisations de l’objet, notamment accorder les autorisations qu’ils n’avaient pas à l’origine. Par conséquent, les autorisations par défaut sur l’objet AdminSDHolder d’un domaine empêchent les utilisateurs qui sont membres de groupes BA ou EA de modifier les autorisations pour l’objet AdminSDHolder d’un domaine. Toutefois, les membres du groupe administrateurs du domaine peuvent prendre possession de l’objet et accorder des autorisations supplémentaires, ce qui signifie que cette protection est rudimentaire et protège uniquement l’objet contre les modifications accidentelles par les utilisateurs qui ne sont pas membres du groupe DA dans le domaine. En outre, les groupes BA et EA (le cas échéant) ont l’autorisation de modifier les attributs de l’objet AdminSDHolder dans le domaine local (domaine racine pour EA).  
  
> [!NOTE]  
> Un attribut sur l’objet AdminSDHolder, dSHeuristics, permet une personnalisation (suppression) limitée des groupes qui sont considérés comme des groupes protégés et sont affectés par AdminSDHolder et SDProp. Cette personnalisation doit être examinée avec soin si elle est implémentée, bien qu’il existe des circonstances valides dans lesquelles la modification de dSHeuristics sur AdminSDHolder est utile. Pour plus d’informations sur la modification de l’attribut dSHeuristics sur un objet AdminSDHolder, consultez les articles [817433](https://support.microsoft.com/?id=817433) et [973840](https://support.microsoft.com/kb/973840)de la support Microsoft, ainsi que l' [annexe C : comptes et groupes protégés dans Active Directory](Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
Bien que les groupes les plus privilégiés dans Active Directory soient décrits ici, il existe un certain nombre d’autres groupes auxquels ont été accordés des niveaux élevés de privilèges. Pour plus d’informations sur tous les groupes par défaut et prédéfinis dans Active Directory et les droits d’utilisateur attribués à chacun, consultez [l’annexe B : comptes et groupes privilégiés dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md).  
  


