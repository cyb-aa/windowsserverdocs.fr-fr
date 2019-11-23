---
ms.assetid: eafdddc3-40d7-4a75-8f4f-a45294aabfc8
title: Implémentation des hôtes d’administration sécurisée
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 56986f2ea9f49bdfc1194ae5342798793524e86c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408618"
---
# <a name="implementing-secure-administrative-hosts"></a>Implémentation des hôtes d’administration sécurisée

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Les hôtes d’administration sécurisés sont des stations de travail ou des serveurs qui ont été configurés spécifiquement pour la création de plateformes sécurisées à partir desquelles les comptes privilégiés peuvent effectuer des tâches d’administration dans Active Directory ou sur les contrôleurs de domaine. systèmes joints à un domaine et applications s’exécutant sur des systèmes joints à un domaine. Dans ce cas, « comptes privilégiés » fait référence non seulement aux comptes membres des groupes les plus privilégiés dans Active Directory, mais aussi à tous les comptes qui ont reçu des droits et des autorisations qui permettent d’effectuer des tâches d’administration.  
  
Ces comptes peuvent être des comptes de support technique qui ont la possibilité de réinitialiser les mots de passe pour la plupart des utilisateurs d’un domaine, les comptes utilisés pour administrer les enregistrements et les zones DNS ou les comptes utilisés pour la gestion de la configuration. Les hôtes d’administration sécurisés sont dédiés aux fonctionnalités administratives et n’exécutent pas de logiciels tels que des applications de messagerie, des navigateurs Web ou des logiciels de productivité tels que des Microsoft Office.  
  
Bien que les comptes et les groupes « les plus privilégiés » doivent en conséquence être la protection la plus rigoureuse, cela ne supprime pas le besoin de protéger les comptes et les groupes auxquels des privilèges supérieurs à ceux des comptes d’utilisateur standard ont été accordés.  
  
Un ordinateur hôte d’administration sécurisé peut être une station de travail dédiée qui est utilisée uniquement pour les tâches d’administration, un serveur membre qui exécute le rôle de serveur de passerelle Bureau à distance et auquel les utilisateurs se connectent pour effectuer l’administration des hôtes de destination, ou un serveur qui exécute le rôle Hyper-V et fournit un ordinateur virtuel unique pour chaque utilisateur informatique à utiliser pour leurs tâches administratives. Dans de nombreux environnements, les combinaisons des trois approches peuvent être implémentées.  
  
L’implémentation d’ordinateurs hôtes d’administration sécurisés requiert une planification et une configuration cohérente avec la taille de votre organisation, les pratiques administratives, l’appétit des risques et le budget. Les considérations et les options d’implémentation d’ordinateurs d’administration sécurisés sont fournies ici pour vous permettre de développer une stratégie administrative adaptée à votre organisation.  
  
## <a name="principles-for-creating-secure-administrative-hosts"></a>Principes de création d’ordinateurs hôtes d’administration sécurisés  
Pour sécuriser efficacement les systèmes contre les attaques, vous devez garder quelques principes généraux à l’esprit :  
  
1.  Vous ne devez jamais administrer un système approuvé (c’est-à-dire, un serveur sécurisé comme un contrôleur de domaine) à partir d’un ordinateur hôte doté d’un niveau de confiance moindre (autrement dit, une station de travail qui n’est pas sécurisée au même niveau que les systèmes qu’il gère).  
  
2.  Vous ne devez pas compter sur un seul facteur d’authentification lors de l’exécution d’activités privilégiées ; autrement dit, les combinaisons nom d’utilisateur/mot de passe ne doivent pas être considérées comme des authentifications acceptables, car seul un facteur unique (un élément que vous connaissez) est représenté. Vous devez tenir compte de l’emplacement où les informations d’identification sont générées et mises en cache ou stockées dans les scénarios d’administration.  
  
3.  Bien que la plupart des attaques dans le paysage des menaces actuelles tirent parti des logiciels malveillants et des attaques malveillantes, n’oubliez pas la sécurité physique lors de la conception et de l’implémentation d’ordinateurs hôtes d’administration sécurisés.  
  
### <a name="account-configuration"></a>Configuration du compte  
Même si votre organisation n’utilise pas de cartes à puce pour l’instant, vous devez envisager de les implémenter pour les comptes privilégiés et les hôtes d’administration sécurisés. Les hôtes d’administration doivent être configurés pour exiger l’ouverture de session par carte à puce pour tous les comptes en modifiant le paramètre suivant dans un objet de stratégie de groupe lié aux unités d’organisation contenant des hôtes d’administration :  
  
**Ordinateur \ stratégies \ stratégies \ stratégies d’ouverture de session Options\Interactive de connexion : exiger une carte à puce**  
  
Ce paramètre nécessite que toutes les ouvertures de session interactives utilisent une carte à puce, quelle que soit la configuration d’un compte individuel dans Active Directory.  
  
Vous devez également configurer des hôtes d’administration sécurisés pour autoriser uniquement les ouvertures de session par les comptes autorisés, qui peuvent être configurés dans :  
  
**Ordinateur \ stratégies \ stratégies \ stratégies d’autorisation \ stratégies Locales\attribution des droits d’ordinateur**  
  
Cela octroie des droits d’ouverture de session interactifs (et, le cas échéant, Services Bureau à distance) uniquement aux utilisateurs autorisés de l’hôte d’administration sécurisé.  
  
### <a name="physical-security"></a>Sécurité physique  
Pour que les hôtes d’administration soient considérés comme dignes de confiance, ils doivent être configurés et protégés au même niveau que les systèmes qu’ils gèrent. La plupart des recommandations fournies dans [sécurisation des contrôleurs de domaine contre les attaques](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md) sont également applicables aux ordinateurs hôtes utilisés pour administrer les contrôleurs de domaine et la base de données AD DS. L’une des difficultés liées à l’implémentation de systèmes d’administration sécurisés dans la plupart des environnements est que la sécurité physique peut être plus difficile à implémenter, car ces ordinateurs se trouvent souvent dans des domaines qui ne sont pas aussi sécurisés que des serveurs hébergés dans des centres de informations, tels que postes de travail des utilisateurs administratifs.  
  
La sécurité physique comprend le contrôle de l’accès physique aux hôtes d’administration. Dans une petite organisation, cela peut signifier que vous conservez une station de travail d’administration dédiée qui reste verrouillée dans un bureau ou un tiroir de bureau lorsqu’elle n’est pas utilisée. Cela peut également signifier que lorsque vous devez effectuer l’administration de Active Directory ou de vos contrôleurs de domaine, vous vous connectez directement au contrôleur de domaine.  
  
Dans les organisations de taille moyenne, vous pouvez envisager d’implémenter des « serveurs de saut » d’administration sécurisée qui se trouvent dans un emplacement sécurisé au sein d’un bureau et qui sont utilisés lorsque la gestion de Active Directory ou de contrôleurs de domaine est requise. Vous pouvez également implémenter des postes de travail d’administration qui sont verrouillés dans des emplacements sécurisés quand ils ne sont pas utilisés, avec ou sans serveur de saut.  
  
Dans les grandes organisations, vous pouvez déployer des serveurs de basculement hébergés dans un centre de donnes qui fournissent un accès strictement contrôlé aux Active Directory ; contrôleurs de domaine ; et les serveurs de fichiers, d’impression ou d’applications. L’implémentation d’une architecture de serveur de sauts est plus susceptible d’inclure une combinaison de stations de travail et de serveurs sécurisés dans des environnements de grande taille.  
  
Quelle que soit la taille de votre organisation et la conception de vos hôtes d’administration, vous devez sécuriser les ordinateurs physiques contre tout accès non autorisé ou vol, et utiliser Chiffrement de lecteur BitLocker pour chiffrer et protéger les lecteurs sur les hôtes d’administration. . En implémentant BitLocker sur les hôtes d’administration, même si un ordinateur hôte est volé ou que ses disques sont supprimés, vous pouvez vous assurer que les données sur le lecteur ne sont pas accessibles aux utilisateurs non autorisés.  
  
### <a name="operating-system-versions-and-configuration"></a>Versions et configuration du système d’exploitation  
Tous les ordinateurs hôtes d’administration, qu’il s’agisse de serveurs ou de stations de travail, doivent exécuter le système d’exploitation le plus récent utilisé dans votre organisation pour les raisons décrites précédemment dans ce document. En exécutant les systèmes d’exploitation actuels, votre personnel administratif tire parti de nouvelles fonctionnalités de sécurité, d’une prise en charge complète des fournisseurs et de fonctionnalités supplémentaires introduites dans le système d’exploitation. En outre, lorsque vous évaluez un nouveau système d’exploitation, en le déployant d’abord sur les hôtes d’administration, vous devez vous familiariser avec les nouvelles fonctionnalités, les paramètres et les mécanismes de gestion qu’il offre, qui peut ensuite être exploité dans la planification. déploiement plus étendu du système d’exploitation. Ensuite, les utilisateurs les plus sophistiqués de votre organisation seront également les utilisateurs qui sont familiarisés avec le nouveau système d’exploitation et le mieux adapté pour le prendre en charge.  
  
### <a name="microsoft-security-configuration-wizard"></a>Assistant Configuration de la sécurité de Microsoft  
Si vous implémentez des serveurs de saut dans le cadre de votre stratégie d’hôte administratif, vous devez utiliser l’Assistant Configuration de la sécurité intégrée pour configurer les paramètres du service, du Registre, de l’audit et du pare-feu afin de réduire la surface d’attaque du serveur. Lorsque les paramètres de configuration de l’Assistant Configuration de la sécurité ont été collectés et configurés, les paramètres peuvent être convertis en un objet de stratégie de groupe qui est utilisé pour appliquer une configuration de ligne de base cohérente sur tous les serveurs de basculement. Vous pouvez modifier davantage l’objet de stratégie de groupe pour implémenter des paramètres de sécurité spécifiques aux serveurs de saut, et vous pouvez combiner tous les paramètres avec des paramètres de ligne de base supplémentaires extraits de Microsoft Security Compliance Manager.  
  
### <a name="microsoft-security-compliance-manager"></a>Microsoft Security Compliance Manager  
[Microsoft Security Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx) est un outil disponible gratuitement qui intègre les configurations de sécurité recommandées par Microsoft, en fonction de la version du système d’exploitation et de la configuration du rôle, et qui les collecte dans un outil unique et une interface utilisateur qui peut être utilisée pour créer et configurer des paramètres de sécurité de base pour les contrôleurs de domaine. Les modèles Microsoft Security Compliance Manager peuvent être combinés avec les paramètres de l’Assistant Configuration de la sécurité pour produire des lignes de base de configuration complètes pour les serveurs de saut qui sont déployés et appliqués par les objets de stratégie de groupe déployés dans les unités d’organisation dans lesquelles les serveurs de saut sont situé dans Active Directory.  
  
> [!NOTE]  
> À la rédaction de cet article, Microsoft Security Compliance Manager n’inclut pas de paramètres spécifiques aux serveurs de sauts ou à d’autres hôtes d’administration sécurisés, mais Security Compliance Manager (SCM) peut toujours être utilisé pour créer des lignes de base initiales pour votre administration répertorié. Toutefois, pour sécuriser correctement les ordinateurs hôtes, vous devez appliquer des paramètres de sécurité supplémentaires adaptés aux stations de travail et serveurs hautement sécurisés.  
  
### <a name="applocker"></a>AppLocker  
Les hôtes d’administration et les machinesshould virtuels sont configurés avec des listes de scripts, d’outils et d’applications autorisées via AppLocker ou un logiciel de restriction d’application tiers. Les applications ou utilitaires d’administration qui n’adhèrent pas aux paramètres de sécurité doivent être mis à niveau ou remplacés par des outils qui adhèrent à des pratiques de développement et d’administration sécurisées. Lorsque des outils nouveaux ou supplémentaires sont nécessaires sur un hôte d’administration, les applications et les utilitaires doivent être testés minutieusement et, si les outils sont adaptés au déploiement sur les hôtes d’administration, ils peuvent être ajoutés aux listes d’autorisation des systèmes.  
  
### <a name="rdp-restrictions"></a>Restrictions RDP  
Bien que la configuration spécifique varie en fonction de l’architecture de vos systèmes d’administration, vous devez inclure des restrictions sur les comptes et les ordinateurs qui peuvent être utilisés pour établir des connexions protocole RDP (Remote Desktop Protocol) (RDP) aux systèmes gérés. par exemple, l’utilisation de serveurs de saut de passerelle de Bureau à distance (passerelle Bureau à distance) pour contrôler l’accès aux contrôleurs de domaine et autres systèmes gérés à partir des utilisateurs et systèmes autorisés.  
  
Vous devez autoriser les ouvertures de session interactives par les utilisateurs autorisés et supprimer ou même bloquer d’autres types d’ouverture de session qui ne sont pas nécessaires pour l’accès au serveur.  
  
### <a name="patch-and-configuration-management"></a>Gestion des correctifs et des configurations  
Les organisations plus petites peuvent s’appuyer sur des offres telles que Windows Update ou [Windows Server Update Services](https://technet.microsoft.com/windowsserver/bb332157) (WSUS) pour gérer le déploiement des mises à jour sur les systèmes Windows, tandis que les grandes entreprises peuvent implémenter des logiciels de gestion des configurations et des correctifs d’entreprise tels que des System Center Configuration Manager. Quels que soient les mécanismes que vous utilisez pour déployer des mises à jour sur votre serveur général et votre alimentation de station de travail, vous devez envisager des déploiements distincts pour les systèmes hautement sécurisés, tels que les contrôleurs de domaine, les autorités de certification et les hôtes d’administration. En séparant ces systèmes de l’infrastructure de gestion générale, si votre logiciel de gestion ou vos comptes de service sont compromis, la compromission ne peut pas être facilement étendue aux systèmes les plus sécurisés de votre infrastructure.  
  
Même si vous ne devez pas implémenter les processus de mise à jour manuelle pour les systèmes sécurisés, vous devez configurer une infrastructure distincte pour la mise à jour des systèmes sécurisés. Même dans les grandes organisations, cette infrastructure peut généralement être implémentée via des serveurs WSUS dédiés et des objets de stratégie de groupe pour les systèmes sécurisés.  
  
### <a name="blocking-internet-access"></a>Blocage de l’accès à Internet  
Les hôtes d’administration ne doivent pas être autorisés à accéder à Internet, et ils ne doivent pas être en mesure de parcourir l’intranet d’une organisation. Les navigateurs Web et les applications similaires ne doivent pas être autorisés sur les hôtes d’administration. Vous pouvez bloquer l’accès à Internet pour les ordinateurs hôtes sécurisés à l’aide d’une combinaison de paramètres de pare-feu de périmètre, de configuration WFAS et de configuration de proxy « trou noir » sur les hôtes sécurisés. Vous pouvez également utiliser la mise en liste verte d’application pour empêcher l’utilisation de navigateurs Web sur les hôtes d’administration.  
  
### <a name="virtualization"></a>Virtualisation  
Si possible, envisagez d’implémenter des ordinateurs virtuels en tant qu’ordinateurs hôtes d’administration. À l’aide de la virtualisation, vous pouvez créer des systèmes d’administration par utilisateur qui sont stockés et gérés de manière centralisée, et qui peuvent être facilement arrêtés lorsqu’ils ne sont pas utilisés, garantissant ainsi que les informations d’identification ne restent pas actives sur les systèmes d’administration. Vous pouvez également exiger que les hôtes d’administration virtuelle soient réinitialisés à un instantané initial après chaque utilisation, garantissant ainsi que les machines virtuelles restent initiales. Vous trouverez plus d’informations sur les options de virtualisation des ordinateurs hôtes d’administration dans la section suivante.  
  
## <a name="sample-approaches-to-implementing-secure-administrative-hosts"></a>Exemples d’approches de l’implémentation d’ordinateurs hôtes d’administration sécurisés  
Quelle que soit la façon dont vous concevez et déployez votre infrastructure d’hôte d’administration, vous devez garder à l’esprit les instructions fournies dans « principes de création d’ordinateurs hôtes d’administration sécurisés » plus haut dans cette rubrique. Chacune des approches décrites ici fournit des informations générales sur la façon dont vous pouvez séparer les systèmes « d’administration » et de « productivité » utilisés par votre service informatique. Les systèmes de productivité sont des ordinateurs que les administrateurs informatiques utilisent pour vérifier l’adresse de messagerie, naviguer sur Internet et utiliser un logiciel de productivité général, tel que Microsoft Office. Les systèmes d’administration sont des ordinateurs qui sont renforcés et dédiés pour l’administration quotidienne d’un environnement informatique.  
  
La façon la plus simple d’implémenter des hôtes d’administration sécurisés consiste à fournir à votre service informatique des stations de travail sécurisées à partir desquelles ils peuvent effectuer des tâches d’administration. Dans une implémentation de station de travail uniquement, chaque station de travail d’administration est utilisée pour lancer des outils de gestion et des connexions RDP pour gérer des serveurs et d’autres infrastructures. Les implémentations de station de travail uniquement peuvent être efficaces dans les organisations de petite taille, bien que les infrastructures plus grandes et plus complexes puissent bénéficier d’une conception distribuée pour les hôtes administratifs dans lesquels des stations de travail et des serveurs d’administration dédiés sont utilisés, comme décrit dans la section « implémentation de stations de travail d’administration sécurisées et de serveurs de saut » plus loin dans cette rubrique.  
  
### <a name="implementing-separate-physical-workstations"></a>Implémentation de stations de travail physiques distinctes  
L’une des méthodes permettant d’implémenter des hôtes d’administration consiste à émettre chaque utilisateur informatique de deux stations de travail. Une station de travail est utilisée avec un compte d’utilisateur « normal » pour effectuer des activités telles que la vérification de l’e-mail et l’utilisation d’applications de productivité, tandis que la deuxième station de travail est dédiée exclusivement aux fonctions d’administration.  
  
Pour la station de travail de productivité, le personnel informatique peut recevoir des comptes d’utilisateur standard plutôt que d’utiliser des comptes privilégiés pour se connecter à des ordinateurs non sécurisés. La station de travail d’administration doit être configurée avec une configuration rigoureusement contrôlée et le personnel informatique doit utiliser un autre compte pour se connecter à la station de travail d’administration.  
  
Si vous avez implémenté des cartes à puce, les stations de travail d’administration doivent être configurées pour exiger des ouvertures de session par carte à puce, et le personnel informatique doit disposer de comptes distincts pour l’utilisation administrative, également configurés pour exiger des cartes à puce pour une ouverture de session interactive. L’hôte d’administration doit être renforcé comme décrit précédemment, et seuls les utilisateurs désignés doivent être autorisés à se connecter localement à la station de travail d’administration.  
  
#### <a name="pros"></a>Avantages  
En implémentant des systèmes physiques distincts, vous pouvez vous assurer que chaque ordinateur est correctement configuré pour son rôle et qu’il ne peut pas exposer par inadvertance les systèmes d’administration à des risques.  
  
#### <a name="cons"></a>Inconvénients  
  
-   L’implémentation d’ordinateurs physiques distincts augmente les coûts matériels.  
  
-   La connexion à un ordinateur physique à l’aide des informations d’identification utilisées pour administrer des systèmes distants met en cache les informations d’identification en mémoire.  
  
-   Si les stations de travail d’administration ne sont pas stockées de manière sécurisée, elles peuvent être vulnérables à une compromission via des mécanismes tels que des enregistreurs de clés matérielles physiques ou d’autres attaques physiques.  
  
### <a name="implementing-a-secure-physical-workstation-with-a-virtualized-productivity-workstation"></a>Implémentation d’une station de travail physique sécurisée avec une station de travail de productivité virtualisée  
Dans cette approche, les utilisateurs disposent d’une station de travail d’administration sécurisée à partir de laquelle ils peuvent exécuter des fonctions d’administration quotidiennes, à l’aide de Outils d’administration de serveur distant (RSAT) ou de connexions RDP aux serveurs au sein de leur domaine d’application. Quand les utilisateurs doivent effectuer des tâches de productivité, ils peuvent se connecter via RDP à une station de travail de productivité distante s’exécutant en tant que machine virtuelle. Des informations d’identification distinctes doivent être utilisées pour chaque station de travail, et les contrôles tels que les cartes à puce doivent être implémentés.  
  
#### <a name="pros"></a>Avantages  
  
-   Les stations de travail d’administration et les stations de travail de productivité sont séparées.  
  
-   Le personnel informatique qui utilise des postes de travail sécurisés pour se connecter aux stations de travail de productivité peut utiliser des informations d’identification et des cartes à puce distinctes, et les informations d’identification privilégiées ne sont pas déposées sur l’ordinateur moins sécurisé.  
  
#### <a name="cons"></a>Inconvénients  
  
-   L’implémentation de la solution nécessite un travail de conception et d’implémentation et des options de virtualisation robustes.  
  
-   Si les stations de travail physiques ne sont pas stockées en toute sécurité, elles peuvent être vulnérables aux attaques physiques qui compromettent le matériel ou le système d’exploitation et les rendent vulnérables à l’interception des communications.  
  
### <a name="implementing-a-single-secure-workstation-with-connections-to-separate-productivity-and-administrative-virtual-machines"></a>Implémentation d’une station de travail sécurisée unique avec des connexions pour séparer les machines virtuelles « de productivité » et « d’administration »  
Dans cette approche, vous pouvez émettre aux utilisateurs informatiques une seule station de travail physique verrouillée comme décrit précédemment, et sur laquelle les utilisateurs n’ont pas d’accès privilégié. Vous pouvez fournir des connexions Services Bureau à distance aux machines virtuelles hébergées sur des serveurs dédiés, ce qui permet au personnel informatique d’utiliser une machine virtuelle qui exécute la messagerie électronique et d’autres applications de productivité, ainsi qu’une deuxième machine virtuelle configurée en tant qu’utilisateur hôte d’administration dédié.  
  
Vous devez exiger une carte à puce ou une autre ouverture de session multifacteur pour les machines virtuelles, à l’aide de comptes distincts autres que le compte utilisé pour se connecter à l’ordinateur physique. Une fois qu’un utilisateur informatique se connecte à un ordinateur physique, il peut utiliser sa carte à puce de productivité pour se connecter à son ordinateur de productivité distant et un compte distinct et une carte à puce pour se connecter à son ordinateur d’administration à distance.  
  
#### <a name="pros"></a>Avantages  
  
-   Les utilisateurs peuvent utiliser une seule station de travail physique.  
  
-   En exigeant des comptes distincts pour les hôtes virtuels et en utilisant des connexions Services Bureau à distance aux machines virtuelles, les informations d’identification des utilisateurs ne sont pas mises en cache dans la mémoire de l’ordinateur local.  
  
-   L’hôte physique peut être sécurisé au même niveau que les hôtes d’administration, ce qui réduit le risque de compromission de l’ordinateur local.  
  
-   Dans les cas où la machine virtuelle de productivité de l’utilisateur informatique ou sa machine virtuelle d’administration peut avoir été compromise, l’ordinateur virtuel peut être facilement réinitialisé à un état « correct connu ».  
  
-   Si l’ordinateur physique est compromis, aucune information d’identification privilégiée n’est mise en cache en mémoire, et l’utilisation de cartes à puce peut empêcher la compromission des informations d’identification par des enregistreurs de frappe.  
  
#### <a name="cons"></a>Inconvénients  
  
-   L’implémentation de la solution nécessite un travail de conception et d’implémentation et des options de virtualisation robustes.  
  
-   Si les stations de travail physiques ne sont pas stockées en toute sécurité, elles peuvent être vulnérables aux attaques physiques qui compromettent le matériel ou le système d’exploitation et les rendent vulnérables à l’interception des communications.  
  
### <a name="implementing-secure-administrative-workstations-and-jump-servers"></a>Implémentation de stations de travail d’administration sécurisées et de serveurs de basculement  
En guise d’alternative à la sécurisation des stations de travail d’administration, ou en combinaison avec elles, vous pouvez implémenter des serveurs de saut sécurisés, et les utilisateurs administratifs peuvent se connecter aux serveurs de sauts à l’aide du protocole RDP et des cartes à puce pour effectuer des tâches d’administration.  
  
Les serveurs de saut doivent être configurés pour exécuter le rôle de passerelle Bureau à distance pour vous permettre d’implémenter des restrictions sur les connexions au serveur de renvoi et aux serveurs de destination qui seront gérés à partir de ce dernier. Si possible, vous devez également installer le rôle Hyper-V et créer des [bureaux virtuels personnels](https://technet.microsoft.com/library/dd759174.aspx) ou d’autres machines virtuelles par utilisateur pour que les utilisateurs administratifs puissent les utiliser pour leurs tâches sur les serveurs de basculement.  
  
En donnant aux utilisateurs administratifs des machines virtuelles par utilisateur sur le serveur de renvoi, vous fournissez une sécurité physique pour les stations de travail d’administration, et les utilisateurs administratifs peuvent réinitialiser ou arrêter leurs ordinateurs virtuels lorsqu’ils ne sont pas utilisés. Si vous préférez ne pas installer le rôle Hyper-V et le rôle de passerelle Bureau à distance sur le même hôte d’administration, vous pouvez les installer sur des ordinateurs distincts.  
  
Dans la mesure du possible, vous devez utiliser les outils d’administration à distance pour gérer les serveurs. La fonctionnalité de Outils d’administration de serveur distant (RSAT) doit être installée sur les machines virtuelles des utilisateurs (ou sur le serveur de renvoi si vous n’implémentez pas de machines virtuelles par utilisateur pour l’administration) et que le personnel administratif doit se connecter via RDP à son machines virtuelles pour effectuer des tâches d’administration.  
  
Dans les cas où un utilisateur administratif doit se connecter via RDP à un serveur de destination pour le gérer directement, la passerelle des services Bureau à distance doit être configurée pour autoriser la connexion à être établie uniquement si l’utilisateur et l’ordinateur appropriés sont utilisés pour établir la connexion à la destination. serveurs. L’exécution d’outils RSAT (ou des outils similaires) doit être interdite sur les systèmes qui ne sont pas des systèmes de gestion désignés, tels que des stations de travail à usage général et des serveurs membres qui ne sont pas des serveurs de saut.  
  
#### <a name="pros"></a>Avantages  
  
-   La création de serveurs de basculement vous permet de mapper des serveurs spécifiques à des « zones » (ensembles de systèmes présentant des exigences de configuration, de connexion et de sécurité similaires) sur votre réseau et d’exiger que l’administration de chaque zone soit effectuée par le personnel administratif connexion d’ordinateurs hôtes d’administration sécurisés à un serveur « zone » désigné.  
  
-   En mappant les serveurs de saut à des zones, vous pouvez implémenter des contrôles granulaires pour les propriétés de connexion et les exigences de configuration, et vous pouvez facilement identifier les tentatives de connexion à partir de systèmes non autorisés.  
  
-   En implémentant des machines virtuelles par administrateur sur des serveurs de basculement, vous appliquez l’arrêt et la réinitialisation des ordinateurs virtuels à un état propre connu lorsque les tâches d’administration sont terminées. En appliquant l’arrêt (ou le redémarrage) des machines virtuelles lorsque les tâches d’administration sont terminées, les ordinateurs virtuels ne peuvent pas être ciblés par les attaquants et les attaques par vol d’informations d’identification ne sont pas réalisables, car les informations d’identification mises en cache de la mémoire ne sont pas conservées après un redémarrage.  
  
#### <a name="cons"></a>Inconvénients  
  
-   Des serveurs dédiés sont requis pour les serveurs de sauts, qu’ils soient physiques ou virtuels.  
  
-   L’implémentation de serveurs de sauts désignés et de stations de travail d’administration nécessite une planification et une configuration soignées qui mappent à toutes les zones de sécurité configurées dans l’environnement.  
  


