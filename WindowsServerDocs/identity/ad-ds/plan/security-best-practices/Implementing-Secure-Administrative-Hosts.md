---
ms.assetid: eafdddc3-40d7-4a75-8f4f-a45294aabfc8
title: Implémentation des hôtes d'administration sécurisées
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ed2ff7bfa0cc3b27506b1ca324e819860eef314c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890420"
---
# <a name="implementing-secure-administrative-hosts"></a>Implémentation des hôtes d'administration sécurisées

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Hôtes d’administration sécurisés sont des stations de travail ou les serveurs qui ont été configurés en particulier dans le cadre de la création des plates-formes sécurisées à partir de laquelle des comptes privilégiés peuvent effectuer des tâches administratives dans Active Directory ou les contrôleurs de domaine joints au domaine des systèmes et applications qui s’exécutent sur des systèmes joints à un domaine. Dans ce cas, « comptes privilégiés » fait référence aux comptes qui sont membres des groupes plus privilégiés dans Active Directory, mais à tous les comptes qui ont été délégué des droits et autorisations qui permettent d’effectuer des tâches d’administration.  
  
Ces comptes peuvent être des comptes de support technique qui ont la possibilité de réinitialiser les mots de passe pour la plupart des utilisateurs dans un domaine, les comptes qui sont utilisés pour administrer des enregistrements et zones DNS ou les comptes qui sont utilisés pour la gestion de la configuration. Hôtes d’administration sécurisés sont dédiés aux fonctionnalités d’administration, et ils n’exécutaient pas de logiciels tels que les applications de messagerie, des navigateurs web ou des logiciels de productivité tels que Microsoft Office.  
  
Bien que les comptes et les groupes « plus privilégié » doivent être en conséquence la plus stricte protégé, il n’élimine pas la nécessité de protéger tous les comptes et groupes de quels privilèges au-delà de ceux d’utilisateur standard comptes ont été accordés.  
  
Hôte d’administration sécurisé peut être une station de travail dédiée est utilisée uniquement pour les tâches d’administration, un serveur membre qui exécute le rôle de serveur de passerelle Bureau à distance et les utilisateurs se connectent pour effectuer l’administration des hôtes de destination, ou un serveur qui exécute le rôle Hyper-V et fournit une machine virtuelle unique pour chaque utilisateur de l’informatique à utiliser pour leurs tâches d’administration. Dans de nombreux environnements, les combinaisons de ces trois approches peuvent être implémentés.  
  
Implémentation des hôtes d’administration sécurisées nécessite une planification et configuration qui est cohérente avec la taille de votre organisation, les pratiques d’administration, au goût du risque et budget. Options et considérations pour l’implémentation des hôtes d’administration sécurisés sont fournies ici que vous pouvez utiliser dans le développement d’une stratégie d’administration appropriée pour votre organisation.  
  
## <a name="principles-for-creating-secure-administrative-hosts"></a>Principes de création d’hôtes d’administration sécurisés  
Pour sécuriser efficacement les systèmes contre les attaques, quelques principes généraux doivent être conservés à l’esprit :  
  
1.  Vous ne devez jamais administrer un système approuvé (autrement dit, un serveur sécurisé comme un contrôleur de domaine) à partir d’un hôte de niveau de confiance moindre (autrement dit, une station de travail n’est pas sécurisé pour le même niveau que les systèmes qu’il gère).  
  
2.  Vous ne devez pas compter sur un facteur d’authentification unique lors de l’exécution des activités privilégiées ; Autrement dit, les combinaisons nom et mot de passe utilisateur ne doivent pas être considérées acceptable d’authentification, car un seul facteur (quelque chose que vous connaissez) est représenté. Vous devez envisager où informations d’identification sont générées et mis en cache ou stockées dans les scénarios d’administration.  
  
3.  Bien que la plupart des attaques dans le paysage des menaces actuel tirer parti de logiciels malveillants et le piratage malveillant, n’oubliez pas de sécurité physique lors de la conception et implémentation des hôtes d’administration sécurisés.  
  
### <a name="account-configuration"></a>Configuration du compte  
Même si votre organisation n’utilise pas actuellement les cartes à puce, vous devez envisager de les mettre en œuvre pour les comptes privilégiés et des hôtes d’administration sécurisés. Hôtes d’administration doivent être configurés pour exiger l’ouverture de session de carte à puce pour tous les comptes en modifiant le paramètre suivant dans un objet de stratégie de groupe qui est lié aux unités d’organisation contenant des hôtes d’administration :  
  
**Ordinateur Configuration ordinateur\Stratégies\Paramètres Settings\Local Policies\Security sécurité\Session : Carte à puce nécessaire**  
  
Ce paramètre nécessite toutes les ouvertures de session interactives à utiliser une carte à puce, quelle que soit la configuration sur un compte individuel dans Active Directory.  
  
Vous devez également configurer des hôtes d’administration sécurisés pour autoriser les ouvertures de session uniquement par les comptes autorisés, qui peuvent être configurés dans :  
  
**Computer Configuration ordinateur\Stratégies\Paramètres Settings\Local Policies\Security Settings\Local Policies\User Rights Assignment**  
  
Cela accorde interactive (et, le cas échéant, les Services Bureau à distance) les droits d’ouverture de session uniquement aux utilisateurs autorisés de l’hôte d’administration sécurisée.  
  
### <a name="physical-security"></a>Sécurité physique  
Pour les hôtes d’administration être considéré comme digne de confiance, ils doivent être configurés et protégées dans le même niveau que les systèmes qu’ils gèrent. La plupart des recommandations fournies dans [sécurisation de contrôleurs de domaine contre les attaques](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md) sont également applicables aux ordinateurs hôtes qui sont utilisés pour administrer les contrôleurs de domaine et de la base de données AD DS. Un des défis de l’implémentation des systèmes d’administration sécurisés dans la plupart des environnements est que la sécurité physique peut être plus difficile à implémenter, car ces ordinateurs se trouvent souvent dans des zones qui ne sont pas aussi sécurisés que les serveurs hébergés dans des centres de données, tel que ordinateurs de bureau des utilisateurs administratifs.  
  
Sécurité physique inclut le contrôle d’accès physique aux hôtes d’administration. Dans une petite entreprise, cela peut signifier que vous avez verrouillé par une station de travail d’administration dédiée qui est conservée dans un bureau ou un tiroir inutilisée. Ou cela peut signifier que lorsque vous avez besoin exécuter l’administration d’Active Directory ou vos contrôleurs de domaine, vous ouvrez une session le contrôleur de domaine directement.  
  
Dans les entreprises de taille moyenne, vous pouvez envisager d’implémentation sécurisée d’administration « jump serveurs » qui se trouvent dans un emplacement sécurisé dans un bureau et sont utilisées lors de la gestion des contrôleurs de domaine Active Directory est requise. Vous pouvez également implémenter des stations de travail administratives qui sont verrouillées à des emplacements sécurisés inutilisée, avec ou sans serveurs jump.  
  
Dans les grandes organisations, vous pouvez déployer des serveurs jump hébergée de centre de données qui fournissent un accès strictement contrôlé à Active Directory ; contrôleurs de domaine ; et fichier, d’impression ou serveurs d’applications. Implémentation d’une architecture de serveur de renvoi est probablement à inclure une combinaison de serveurs et stations de travail sécurisées dans les environnements de grande taille.  
  
Quelle que soit la taille de votre organisation et la conception de vos hôtes d’administration, vous devez sécuriser les ordinateurs physiques contre tout accès non autorisé ou le vol et devez utiliser le chiffrement de lecteur BitLocker pour chiffrer et protéger les lecteurs sur les ordinateurs hôtes d’administration . En mettant en œuvre de BitLocker sur les ordinateurs hôtes d’administration, même si un ordinateur hôte est volé ou ses disques sont supprimés, vous pouvez vous assurer que les données sur le lecteur ne sont pas accessible aux utilisateurs non autorisés.  
  
### <a name="operating-system-versions-and-configuration"></a>Configuration et les Versions de système d’exploitation  
Tous les hôtes d’administration, si les serveurs ou stations de travail doit s’exécuter le dernier système d’exploitation en cours d’utilisation dans votre organisation pour les raisons décrites précédemment dans ce document. En exécutant des systèmes d’exploitation en cours, votre personnel administratif tire parti de nouvelles fonctionnalités de sécurité, prise en charge complète de fournisseur et une fonctionnalité supplémentaire introduite dans le système d’exploitation. En outre, lorsque vous évaluez un nouveau système d’exploitation, en le déployant hôtes le premier pour l’administration, vous devrez vous familiariser avec les nouvelles fonctionnalités, les paramètres et les mécanismes de gestion qu’il offre, qui peuvent être exploitées par la suite dans la planification déploiement plus large du système d’exploitation. À ce moment-là, les utilisateurs les plus sophistiqués de votre organisation sera également les utilisateurs qui sont familiers avec le nouveau système d’exploitation et mieux placé sa prise en charge.  
  
### <a name="microsoft-security-configuration-wizard"></a>Assistant Configuration de la sécurité de Microsoft  
Si vous implémentez des serveurs jump dans le cadre de votre stratégie d’administration hôte, vous devez utiliser l’Assistant de Configuration de sécurité intégré pour configurer service, le Registre, d’audit et les paramètres de pare-feu pour réduire la surface d’attaque du serveur. Lorsque les paramètres de configuration Assistant Configuration de sécurité ont été collectées et configurés, les paramètres peuvent être convertis en un objet de stratégie de groupe qui est utilisé pour appliquer une configuration de base cohérente sur tous les serveurs de saut. Vous pouvez ensuite modifier l’objet de stratégie de groupe pour implémenter les paramètres de sécurité spécifiques pour atteindre les serveurs et pourrez combiner tous les paramètres avec les paramètres de ligne de base supplémentaires extraites à partir de Microsoft Security Compliance Manager.  
  
### <a name="microsoft-security-compliance-manager"></a>Microsoft Security Compliance Manager  
Le [Microsoft Security Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx) est un outil disponible gratuitement qui intègre les configurations de sécurité recommandés par Microsoft, selon la configuration de version et le rôle de système d’exploitation et les rassemble dans un outil unique et l’interface utilisateur qui peut être utilisé pour créer et configurer les paramètres de sécurité de base pour les contrôleurs de domaine. Modèles de Microsoft Security Compliance Manager peuvent être combinées avec les paramètres de l’Assistant Configuration de sécurité pour produire des lignes de base de configuration complète pour serveurs jump qui sont déployées et appliquées par GPO déployé sur les unités d’organisation dans les raccourcis sont des serveurs situé dans Active Directory.  
  
> [!NOTE]  
> À ce jour, Microsoft Security Compliance Manager n’inclut pas de paramètres spécifiques aux serveurs jump ou d’autres hôtes d’administration sécurisés, mais Security Compliance Manager (SCM) peut toujours être utilisé pour créer des lignes de base initiales pour votre administration ordinateurs hôtes. Toutefois, pour sécuriser les ordinateurs hôtes, vous devez appliquer les paramètres de sécurité supplémentaires appropriées sur les serveurs et stations de travail hautement sécurisées.  
  
### <a name="applocker"></a>AppLocker  
Hôtes d’administration et virtuel machinesshould être configurés avec script, l’outil et d’autorisation d’application via AppLocker ou un logiciel de restriction d’application tierce. Tous les applications administratives ou les utilitaires qui n’adhèrent pas pour sécuriser les paramètres doivent être mis à niveau ou remplacés par des outils qui adhère à la sécurisation du développement et des pratiques d’administration. Outils de nouveau ou supplémentaire est nécessaire sur un ordinateur hôte d’administration, utilitaires et applications doivent être testées, et si les outils sont adaptée pour le déploiement sur des hôtes d’administration, il peut être ajouté aux listes d’autorisation du système.  
  
### <a name="rdp-restrictions"></a>Restrictions de RDP  
Bien que la configuration spécifique varie selon l’architecture de vos systèmes d’administration, vous devez inclure des restrictions sur lequel les comptes et des ordinateurs peuvent être utilisés pour établir des connexions de protocole RDP (Remote Desktop) pour les systèmes gérés, par exemple à l’aide de la passerelle des services Bureau à distance (passerelle RD) accède serveurs pour contrôler l’accès aux contrôleurs de domaine et autres systèmes gérés à partir de systèmes et utilisateurs autorisés.  
  
Autoriser les ouvertures de session interactive par les utilisateurs autorisés et doit supprimer ou vous même bloquer d’autres types d’ouverture de session qui ne sont pas nécessaires pour accéder au serveur.  
  
### <a name="patch-and-configuration-management"></a>Gestion de Configuration et des correctifs  
Organisations de petite taille peuvent s’appuyer sur les offres telles que Windows Update ou [Windows Server Update Services](https://technet.microsoft.com/windowsserver/bb332157) (WSUS) pour gérer le déploiement des mises à jour des systèmes Windows, tandis que les grandes entreprises peuvent implémenter des correctifs de l’entreprise et logiciel de gestion de configuration tels que System Center Configuration Manager. Quel que soit les mécanismes que vous utilisez pour déployer des mises à jour à votre serveur générales et des postes de travail, vous devez envisager des déploiements distincts pour des systèmes hautement sécurisés tels que les contrôleurs de domaine, les autorités de certification et les hôtes d’administration. En séparant ces systèmes à partir de l’infrastructure de gestion générale, si vos comptes de logiciel ou du service de gestion sont compromis, la compromission ne peuvent pas facilement être étendue pour les systèmes plus sécurisées dans votre infrastructure.  
  
Bien que vous ne devez pas implémenter des processus de mise à jour manuelle de systèmes sécurisés, vous devez configurer une infrastructure distincte pour la mise à jour des systèmes sécurisés. Même dans les très grandes organisations, cette infrastructure peut généralement être implémentée via des serveurs WSUS dédiés et les objets GPO pour les systèmes sécurisés.  
  
### <a name="blocking-internet-access"></a>Bloque l’accès à Internet  
Hôtes d’administration ne doivent pas être autorisés à accéder à Internet, ni doivent-ils être en mesure de parcourir un intranet d’entreprise. Navigateurs Web et des applications similaires ne doivent pas être autorisées sur les ordinateurs hôtes d’administration. Vous pouvez bloquer l’accès à Internet pour les hôtes sécurisés via une combinaison de paramètres de pare-feu de périmètre, la configuration de WFAS et configuration du proxy « trou noir » sur des hôtes sécurisés. Vous pouvez également utiliser la liste verte d’application pour empêcher les navigateurs web d’être utilisés sur des hôtes d’administration.  
  
### <a name="virtualization"></a>Virtualisation  
Si possible, envisagez d’implémenter des machines virtuelles en tant qu’hôtes d’administration. À l’aide de la virtualisation, vous pouvez créer par l’utilisateur des systèmes d’administration qui sont centralisée stockés et gérés, et qui peut être facilement éteintes lorsqu’elles pas en cours d’utilisation, en garantissant qu’informations d’identification ne restent pas actives sur les systèmes d’administration. Vous pouvez également exiger que les hôtes d’administration virtual sont réinitialisés à un instantané initial après chaque utilisation, en garantissant que les ordinateurs virtuels restent trouvés. Plus d’informations sur les options de la virtualisation d’ordinateurs hôtes d’administration sont fournis dans la section suivante.  
  
## <a name="sample-approaches-to-implementing-secure-administrative-hosts"></a>Sécuriser les approches d’exemple à l’implémentation des hôtes d’administration  
Quelle que soit la façon dont vous concevez et déployez votre infrastructure de l’hôte d’administration, vous gardez à l’esprit les instructions fournies dans « Principes de création Secure hôtes d’administration » plus haut dans cette rubrique. Chacune des approches décrites ici fournit des informations générales sur la façon dont vous pouvez séparer les « administration » et les systèmes de « productivity » utilisés par votre service informatique. Les systèmes de productivité sont des ordinateurs qui les administrateurs informatiques utilisent pour vérifier votre messagerie électronique, naviguer sur Internet et d’utiliser des logiciels de productivité général telles que Microsoft Office. Systèmes d’administration sont des ordinateurs qui sont renforcés et dédiés à utiliser pour l’administration quotidienne d’un environnement informatique.  
  
La façon la plus simple d’implémenter des hôtes d’administration sécurisés consiste à fournir votre service informatique avec les stations de travail sécurisées à partir de laquelle ils peuvent effectuer des tâches administratives. Dans une implémentation de station de travail uniquement, chaque station de travail d’administration est utilisée pour lancer les outils de gestion et les connexions RDP pour gérer les serveurs et autres infrastructures. Les implémentations de station de travail uniquement peuvent être efficaces dans les entreprises plus petites, bien que les infrastructures de plus vastes et plus complexes susceptibles de bénéficier d’une conception distribuée pour les hôtes d’administration dans lequel dédié de serveurs d’administration et de stations de travail sont utilisées, en tant que décrit dans « Implémentation sécurisée d’administration stations de travail et passer serveurs » plus loin dans cette rubrique.  
  
### <a name="implementing-separate-physical-workstations"></a>Implémentation des stations de travail physiques distinctes  
Première que vous pouvez implémenter des hôtes d’administration consiste à émettre des deux stations de travail de chaque utilisateur de l’informatique. Une station de travail est utilisée avec un compte d’utilisateur « standard » pour effectuer des activités telles que consulter ses e-mails et à l’aide d’applications de productivité, tandis que la deuxième station de travail est strictement dédiée aux fonctions d’administration.  
  
Pour la station de travail de productivité, le personnel informatique peut se voir comptes d’utilisateur standard plutôt que d’utiliser des comptes privilégiés pour vous connecter à des ordinateurs non sécurisés. La station de travail d’administration doit être configurée avec une configuration rigoureusement contrôlée et le personnel informatique doit utiliser un autre compte pour vous connecter à la station de travail d’administration.  
  
Si vous avez implémenté des cartes à puce, stations de travail d’administration doivent être configurées pour exiger des ouvertures de session de carte à puce et le personnel informatique doit disposer de comptes distincts pour une utilisation d’administration, également configurée pour exiger des cartes à puce pour l’ouverture de session interactive. L’hôte d’administration doit être renforcé comme décrit précédemment, et seuls les utilisateurs informatiques désignés doivent être autorisés à ouvrir une session localement à la station de travail d’administration.  
  
#### <a name="pros"></a>Avantages  
En implémentant des systèmes physiques distincts, vous pouvez vous assurer que chaque ordinateur est correctement configuré pour son rôle et que les utilisateurs ne peuvent pas exposer par inadvertance des systèmes d’administration à un risque.  
  
#### <a name="cons"></a>Inconvénients  
  
-   L’implémentation des ordinateurs physiques distincts augmente les coûts matériels.  
  
-   Connexion à un ordinateur physique avec informations d’identification utilisées pour gérer des systèmes distants met en cache les informations d’identification en mémoire.  
  
-   Si les stations de travail administratives ne sont pas stockées en toute sécurité, il peuvent être vulnérables par le biais de mécanismes tels que les enregistreurs de frappe de matériel physique ou d’autres attaques physiques.  
  
### <a name="implementing-a-secure-physical-workstation-with-a-virtualized-productivity-workstation"></a>Implémentation d’une station de travail physique sécurisée avec une station de travail virtualisés de productivité  
Dans cette approche, les utilisateurs bénéficient d’une station de travail d’administration sécurisée à partir de laquelle ils peuvent effectuer des fonctions administratives quotidiennes, à l’aide des outils d’Administration de serveur distant (RSAT) ou les connexions RDP aux serveurs dans leur étendue de la responsabilité. Lorsque les utilisateurs de l’informatique doivent effectuer des tâches de productivité, ils peuvent se connecter via RDP à une station de travail de productivité à distance en cours d’exécution comme une machine virtuelle. Informations d’identification distinctes doivent être utilisées pour chaque station de travail, et les contrôles tels que des cartes à puce doivent être implémentés.  
  
#### <a name="pros"></a>Avantages  
  
-   Stations de travail de productivité et de stations de travail d’administration sont séparées.  
  
-   Service informatique à l’aide de stations de travail sécurisées pour se connecter aux postes de travail de productivité pouvez utiliser les informations d’identification distinctes et cartes à puce et informations d’identification privilégiées ne sont pas déposées sur l’ordinateur moins sécurisé.  
  
#### <a name="cons"></a>Inconvénients  
  
-   Implémentation de la solution nécessite la conception et de travail d’implémentation et d’options de virtualisation robuste.  
  
-   Si les stations de travail physiques ne sont pas stockées en toute sécurité, ils peuvent être vulnérables aux attaques physiques qui compromettent le matériel ou le système d’exploitation et les rendre vulnérables à l’interception de communications.  
  
### <a name="implementing-a-single-secure-workstation-with-connections-to-separate-productivity-and-administrative-virtual-machines"></a>Implémentation d’un seul poste de travail sécurisé avec des connexions pour séparer les « Productivity » et « Administration » les Machines virtuelles  
Dans cette approche, vous pouvez émettre des utilisateurs de l’informatique une station de travail physique unique, qui est verrouillé comme décrit précédemment, et sur lequel les utilisateurs n’ont pas d’accès privilégié. Vous pouvez fournir des connexions des Services Bureau à distance aux ordinateurs virtuels hébergés sur des serveurs dédiés, fournissant le service informatique avec une machine virtuelle qui exécute le courrier électronique et autres applications de productivité et une deuxième machine virtuelle qui est configurée en tant que l’utilisateur hôte d’administration dédié.  
  
Vous devez exiger une carte à puce ou autres d’ouverture de session multifacteur pour les machines virtuelles, à l’aide de comptes distincts autre que le compte qui est utilisé pour ouvrir une session l’ordinateur physique. Un utilisateur de l’informatique se connecte à un ordinateur physique, ils peuvent utiliser leur carte à puce de productivité pour vous connecter à leur ordinateur à distance de la productivité et un compte distinct et la carte à puce pour se connecter à leur ordinateur d’administration à distance.  
  
#### <a name="pros"></a>Avantages  
  
-   Utilisateurs de l’informatique peuvent utiliser une station de travail physique unique.  
  
-   En exigeant des comptes distincts pour les ordinateurs virtuels hôtes et à l’aide de connexions des Services Bureau à distance aux ordinateurs virtuels, informations d’identification des utilisateurs de l’informatique ne sont pas mises en cache en mémoire sur l’ordinateur local.  
  
-   L’hôte physique peut être sécurisé pour le même niveau que les hôtes d’administration, réduisant ainsi les risques de compromission de l’ordinateur local.  
  
-   Dans le cas dans lequel leur ordinateur virtuel d’administration ou machine virtuelle de la productivité d’un utilisateur de l’informatique ont peut-être été compromis, la machine virtuelle peut facilement être réinitialisée à un état « vérifié ».  
  
-   Si l’ordinateur physique est compromis, aucune information d’identification privilégiée ne sera mis en cache en mémoire et l’utilisation de cartes à puce peut empêcher la compromission des informations d’identification par les enregistreurs de frappe.  
  
#### <a name="cons"></a>Inconvénients  
  
-   Implémentation de la solution nécessite la conception et de travail d’implémentation et d’options de virtualisation robuste.  
  
-   Si les stations de travail physiques ne sont pas stockées en toute sécurité, ils peuvent être vulnérables aux attaques physiques qui compromettent le matériel ou le système d’exploitation et les rendre vulnérables à l’interception de communications.  
  
### <a name="implementing-secure-administrative-workstations-and-jump-servers"></a>Implémentation de stations de travail d’administration sécurisées et des serveurs Jump  
Comme alternative à sécuriser les stations de travail d’administration, ou en combinaison avec eux, vous pouvez implémenter des serveurs jump sécurisé, et les utilisateurs administratifs peuvent se connecter aux serveurs de saut à l’aide de RDP et les cartes à puce pour effectuer des tâches d’administration.  
  
Serveurs Jump doivent être configurés pour exécuter le rôle de passerelle Bureau à distance pour vous permettent d’implémenter des restrictions sur les connexions vers le serveur de renvoi et aux serveurs de destination qui seront gérés à partir de celui-ci. Si possible, vous devez également installer le rôle Hyper-V et créer [des bureaux virtuels personnels](https://technet.microsoft.com/library/dd759174.aspx) ou d’autres machines virtuelles de chaque utilisateur pour les utilisateurs administratifs à utiliser pour leurs tâches sur les serveurs de saut.  
  
En attribuant les utilisateurs administratifs à des machines virtuelles par utilisateur sur le serveur de renvoi, vous fournissez une sécurité physique pour les stations de travail d’administration, et les utilisateurs administratifs peuvent réinitialiser ou arrêter leurs machines virtuelles inutilisée. Si vous préférez ne pas installer le rôle Hyper-V et le rôle de passerelle Bureau à distance sur le même hôte d’administration, vous pouvez les installer sur des ordinateurs distincts.  
  
Dans la mesure du possible, les outils d’administration à distance doivent servir à gérer les serveurs. La fonctionnalité Outils d’Administration de serveur distant (RSAT) doit être installée sur les machines virtuelles de l’utilisateur (ou si vous n’implémentez pas de machines virtuelles de chaque utilisateur pour l’administration, le serveur de renvoi), et le personnel administratif doit se connecter via RDP à leur machines virtuelles pour effectuer des tâches d’administration.  
  
Dans le cas lorsqu’un utilisateur administratif doit se connecter via RDP à un serveur de destination pour le gérer directement, passerelle Bureau à distance doit être configuré pour autoriser la connexion être effectuée uniquement si l’utilisateur approprié et l’ordinateur sont utilisés pour établir la connexion à la destination serveur. L’exécution de serveur distant (ou similaire), outils doivent être interdit sur les systèmes qui ne sont pas désignés de systèmes de gestion, tels que les stations de travail usage général et les serveurs membres qui sont pas passer de serveurs.  
  
#### <a name="pros"></a>Avantages  
  
-   Création de serveurs jump vous permet de mapper des serveurs spécifiques « zones » (collections de systèmes avec des exigences similaires en termes de configuration, de connexion et de sécurité) dans votre réseau et d’exiger que l’administration de chaque zone est effectuée par le personnel administratif connexion à partir d’hôtes d’administration sécurisés sur un serveur désigné « zone ».  
  
-   En mappant les serveurs jump aux zones, vous pouvez implémenter des contrôles granulaires pour les propriétés de connexion et la configuration requise et pouvez identifier facilement les tentatives de connexion à partir des systèmes non autorisés.  
  
-   En implémentant des machines virtuelles par l’administrateur sur les serveurs jump, vous appliquer arrêt et la réinitialisation des machines virtuelles à un état propre connu lorsque les tâches d’administration sont terminées. En appliquant arrêt (ou redémarrer) des machines virtuelles lorsque les tâches d’administration sont terminées, les machines virtuelles ne peut pas être ciblés par les attaquants, ni les attaques de vol d’informations d’identification possible, car les informations d’identification de la mise en mémoire cache ne persistent pas après un redémarrage.  
  
#### <a name="cons"></a>Inconvénients  
  
-   Des serveurs dédiés sont requis pour les serveurs jump, physique ou virtuel.  
  
-   Implémentation désigné raccourcis des serveurs et stations de travail d’administration requiert une planification et configuration qui mappe à des zones de sécurité configurés dans l’environnement.  
  


