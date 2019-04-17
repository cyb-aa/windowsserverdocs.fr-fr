---
ms.assetid: eafdddc3-40d7-4a75-8f4f-a45294aabfc8
title: "Implémentation des hôtes d’administration sécurisés"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 42be89311f11ecc6a967b8b40600b53311253b89
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="implementing-secure-administrative-hosts"></a>Implémentation des hôtes d’administration sécurisés

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Hôtes d’administration sécurisés sont des stations de travail ou des serveurs qui ont été configurés spécifiquement dans le cadre de la création des plateformes sécurisés à partir de laquelle des comptes privilégiés peuvent effectuer des tâches d’administration dans ActiveDirectory ou sur les contrôleurs de domaine, des systèmes joints au domaine et des applications en cours d’exécution sur les systèmes joints au domaine. Dans ce cas, «comptes privilégiés» fait référence aux comptes qui sont membres des groupes privilégiés plus dans ActiveDirectory, mais à tous les comptes qui ont été délégué des droits et autorisations qui permettent d’effectuer des tâches d’administration.  
  
Ces comptes peuvent être des comptes de support technique qui ont la possibilité de réinitialiser les mots de passe pour la plupart des utilisateurs dans un domaine, les comptes qui sont utilisés pour administrer les enregistrements DNS et les zones ou qui sont utilisés pour la gestion de la configuration. Hôtes d’administration sécurisés dédiés à la fonctionnalité d’administration, et ils n’exécutent pas de logiciels tels que les applications de messagerie, les navigateurs web ou les logiciels de productivité tels que MicrosoftOffice.  
  
Bien que les comptes et les groupes «plus privilégié» doivent être en conséquence la plus stricte protégé, cela n’élimine pas la nécessité de protéger tous les comptes et groupes à quels privilèges supérieurs à ceux d’utilisateur standard comptes ont été accordées.  
  
Un hôte d’administration sécurisé peut être une station de travail dédiée qui est utilisée uniquement pour les tâches d’administration, un serveur membre qui exécute le rôle de serveur de passerelle Bureau à distance et à laquelle les utilisateurs se connectent pour effectuer la gestion des hôtes de destination, ou un serveur qui exécute le rôle Hyper-V et fournit un ordinateur virtuel unique pour chaque utilisateur de l’informatique à utiliser pour leurs tâches administratives. Dans de nombreux environnements, les combinaisons de tous les trois approches peuvent être implémentées.  
  
Implémentation des hôtes d’administration sécurisés requiert la planification et configuration qui est cohérente avec la taille de votre organisation, pratiques administratives, goût du risque et du budget. Considérations et les options pour l’implémentation des hôtes d’administration sécurisés sont fournies ici pour vous permettent d’utiliser dans le développement d’une stratégie d’administration appropriée pour votre organisation.  
  
## <a name="principles-for-creating-secure-administrative-hosts"></a>Principes de création d’ordinateurs hôtes d’administration sécurisées  
Pour sécuriser efficacement les systèmes contre les attaques, quelques principes généraux doivent être maintenus à l’esprit:  
  
1.  Vous ne devez jamais administrer un système approuvé (autrement dit, un serveur sécurisé par exemple, un contrôleur de domaine) à partir d’un hôte approuvé moins (autrement dit, une station de travail n’est pas sécurisé pour le même niveau que les systèmes qu’il gère).  
  
2.  Vous ne devez pas compter sur un facteur d’authentification unique lors de l’exécution des activités privilégiées; autrement dit, les combinaisons de nom et mot de passe utilisateur ne doivent pas être considérées authentification acceptable car un facteur unique (quelque chose que vous connaissez) est représenté. Vous devez envisager où informations d’identification sont générées et mis en cache ou stockées dans des scénarios d’administration.  
  
3.  Bien que la plupart des attaques dans le paysage des menaces en cours tirer parti de programmes malveillants et le piratage malveillants, n’oubliez pas de sécurité physique lors de la conception et l’implémentation des hôtes d’administration sécurisés.  
  
### <a name="account-configuration"></a>Configuration du compte  
Même si votre organisation n’utilise pas actuellement de cartes à puce, vous devez envisager de les mettre en œuvre pour les comptes privilégiés et des hôtes d’administration sécurisés. Ordinateurs hôtes d’administration doivent être configurés pour exiger une connexion par carte à puce pour tous les comptes en modifiant le paramètre suivant dans un objet de stratégie de groupe est lié à des unités d’organisation contenant des ordinateurs hôtes d’administration:  
  
**Ordinateur Configuration ordinateur\Stratégies\Paramètres paramètres de sécurité\Stratégies locales\Options de sécurité\Session: carte à puce nécessaire**  
  
Ce paramètre nécessite toutes les ouvertures de session interactives à utiliser une carte à puce, quelle que soit la configuration sur un compte individuel dans ActiveDirectory.  
  
Vous devez également configurer des hôtes d’administration sécurisés pour autoriser les ouvertures de session uniquement par les comptes autorisés, qui peuvent être configurés dans:  
  
**Ordinateur Configuration ordinateur\Stratégies\Paramètres paramètres de sécurité\Stratégies locales\Options sécurité\Stratégies Locales\attribution des droits**  
  
Cela accorde interactif (et, le cas échéant, les Services Bureau à distance) les droits d’ouverture de session uniquement aux utilisateurs autorisés de l’hôte d’administration sécurisée.  
  
### <a name="physical-security"></a>Sécurité physique  
Pour les ordinateurs hôtes d’administration être considéré comme digne de confiance, ils doivent être configurés et protéger sur le même niveau que les systèmes qu’ils gèrent. La plupart des recommandations fournies dans [sécurisation de contrôleurs de domaine contre les attaques](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md) sont également applicables aux ordinateurs hôtes qui sont utilisés pour administrer des contrôleurs de domaine et de la base de données ADDS. Un des défis de la mise en œuvre de la sécurisation des systèmes d’administration dans la plupart des environnements est que la sécurité physique peut être plus difficile à implémenter dans la mesure où ces ordinateurs se trouvent généralement dans les zones qui ne sont pas aussi sécurisés en tant que serveurs hébergés dans des centres de données, tels que des postes de travail des utilisateurs administratifs.  
  
Sécurité physique inclut le contrôle d’accès physique aux ordinateurs hôtes d’administration. Dans une petite entreprise, cela signifie peut-être que vous gérez verrouillé par une station de travail d’administration dédiée qui est conservée dans un bureau ou un tiroir lorsqu’elles pas en cours d’utilisation. Ou bien, cela peut signifier que lorsque vous avez besoin effectuer l’administration d’ActiveDirectory ou de vos contrôleurs de domaine, vous connecter au contrôleur de domaine directement.  
  
Dans les organisations de taille moyenne, vous pouvez envisager la mise en œuvre de la sécurité d’administration «sauter serveurs» qui se trouvent dans un emplacement sécurisé dans un bureau et sont utilisées lors de la gestion des contrôleurs de domaine ActiveDirectory est requise. Vous pouvez également utiliser des stations de travail d’administration qui sont verrouillées dans des emplacements sécurisés lorsque pas utilisez, avec ou sans les serveurs de renvoi.  
  
Dans les grandes entreprises, vous pouvez déployer des serveurs de renvoi hébergées-centre de données qui fournissent l’accès strictement contrôlé à ActiveDirectory. contrôleurs de domaine; et les serveurs de fichiers, d’impression ou application. Implémentation d’une architecture de serveur de renvoi est plus susceptible d’inclure une combinaison de serveurs et stations de travail sécurisées dans les environnements de grande taille.  
  
Quelle que soit la taille de votre organisation et la conception de vos ordinateurs hôtes d’administration, vous devez sécuriser les ordinateurs physiques contre tout accès non autorisé ou de vol et devez utiliser le chiffrement de lecteur BitLocker pour chiffrer et de protéger les lecteurs sur les ordinateurs hôtes d’administration. En mettant en œuvre de BitLocker sur des ordinateurs hôtes d’administration, même si un ordinateur hôte est volé ou ses disques sont supprimés, vous pouvez vous assurer que les données sur le lecteur ne sont pas accessibles aux utilisateurs non autorisés.  
  
### <a name="operating-system-versions-and-configuration"></a>Configuration et les Versions de système d’exploitation  
Tous les ordinateurs hôtes d’administration, si les serveurs ou stations de travail, doivent exécuter le dernier système d’exploitation en cours d’utilisation dans votre organisation pour les raisons décrites précédemment dans ce document. En exécutant des systèmes d’exploitation en cours, votre personnel d’administration bénéficie des nouvelles fonctionnalités de sécurité, la prise en charge complète de fournisseur et des fonctionnalités supplémentaires introduites dans le système d’exploitation. En outre, lorsque vous évaluez un nouveau système d’exploitation, en la déployant tout d’abord sur les ordinateurs hôtes d’administration, vous devez vous familiariser avec les nouvelles fonctionnalités, les paramètres et les mécanismes de gestion proposés, qui peuvent ensuite être exploitées dans la planification d’un déploiement plus large du système d’exploitation. D’ici là, les utilisateurs de votre organisation plus sophistiquées sera également les utilisateurs qui sont familiarisés avec le nouveau système d’exploitation et mieux placé prise en charge.  
  
### <a name="microsoft-security-configuration-wizard"></a>Assistant Configuration de sécurité Microsoft  
Si vous implémentez des serveurs de renvoi dans le cadre de votre stratégie d’ordinateur hôte d’administration, vous devez utiliser l’Assistant Configuration de sécurité intégrée pour configurer le service, le Registre, d’audit et les paramètres de pare-feu pour réduire la surface d’attaque du serveur. Lors de la configuration de l’Assistant Configuration de sécurité ont été collectée et configurée, les paramètres peuvent être convertis en un objet de stratégie de groupe qui est utilisé pour appliquer une configuration de base cohérente sur tous les serveurs de renvoi. Vous pouvez ensuite modifier l’objet de stratégie de groupe pour implémenter des paramètres de sécurité spécifiques pour passer de serveurs et pouvez combiner tous les paramètres avec les paramètres de ligne de base supplémentaires extraits à partir de MicrosoftSecurity Compliance Manager.  
  
### <a name="microsoft-security-compliance-manager"></a>MicrosoftSecurity Compliance Manager  
Le [MicrosoftSecurity Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx) est un outil disponible gratuitement qui intègre des configurations de sécurité qui sont recommandées par Microsoft, selon la configuration de version et le rôle de système d’exploitation, et les regroupe dans un outil unique et l’interface utilisateur qui peut être utilisé pour créer et configurer les paramètres de sécurité de base pour les contrôleurs de domaine. MicrosoftSecurity Compliance Manager modèles peuvent être combinées avec les paramètres de l’Assistant Configuration de sécurité pour produire des lignes de base de configuration complète pour les serveurs de renvoi qui sont déployés et appliquées par la stratégie de groupe déployée sur les unités d’organisation dans les raccourcis des serveurs se trouvent dans ActiveDirectory.  
  
> [!NOTE]  
> À ce jour, MicrosoftSecurity Compliance Manager n’inclut pas de paramètres spécifiques aux serveurs de renvoi ou d’autres hôtes d’administration sécurisés, mais Security Compliance Manager (SCM) peut toujours être utilisé pour créer des lignes de base initiales pour vos ordinateurs hôtes d’administration. Pour sécuriser les ordinateurs hôtes, toutefois, vous devez appliquer des paramètres de sécurité supplémentaires pour les serveurs et stations de travail hautement sécurisées.  
  
### <a name="applocker"></a>AppLocker  
Hôtes d’administration et machinesshould virtuel être configuré avec un script, l’outil et d’autorisation d’applications via AppLocker ou une application tierce restriction de logiciel. Toutes les applications d’administration ou des utilitaires qui n’adhèrent pas pour sécuriser les paramètres doivent être mis à niveau ou remplacés par des outils qui respecte les pratiques d’administration et de développement sécurisé. Lorsque les outils nouveaux ou supplémentaires sont nécessaire sur un ordinateur hôte d’administration, utilitaires et applications doivent être testées, et si les outils convient pour le déploiement sur des ordinateurs hôtes d’administration, il peut être ajouté à un blocage du système.  
  
### <a name="rdp-restrictions"></a>Restrictions de RDP  
Bien que la configuration spécifique varie en fonction de l’architecture de vos systèmes d’administration, vous devez inclure des restrictions sur lequel les comptes et des ordinateurs peuvent être utilisés pour établir des connexions de protocole RDP (Remote Desktop) pour les systèmes gérés, par exemple, à l’aide de la passerelle des services Bureau à distance (RD Gateway) raccourcis des serveurs pour contrôler l’accès aux contrôleurs de domaine et autres gérés systèmes à partir de systèmes et utilisateurs autorisés.  
  
Autoriser les ouvertures de session interactive par les utilisateurs autorisés et doit supprimer ou si vous bloquez des autres types d’ouverture de session qui ne sont pas nécessaires pour accéder au serveur.  
  
### <a name="patch-and-configuration-management"></a>Correctifs et gestion de la Configuration  
Les petites entreprises peuvent reposer sur les offres tels que Windows Update ou [WindowsServerUpdateServices](https://technet.microsoft.com/windowsserver/bb332157) (WSUS) pour gérer le déploiement des mises à jour pour les systèmes Windows, tandis que les grandes entreprises peuvent mettre en œuvre entreprise configuration et le correctif logiciel de gestion tels que SystemCenter ConfigurationManager. Quel que soit les mécanismes que vous utilisez pour déployer des mises à jour sur votre serveur général et de la population de station de travail, vous devez envisager déploiements distincts pour des systèmes hautement sécurisés tels que les contrôleurs de domaine, les autorités de certification et les ordinateurs hôtes d’administration. En séparant ces systèmes de l’infrastructure de gestion générales, si vos comptes de logiciel ou le service de gestion sont compromis, la compromission ne peut pas être facilement étendue pour les systèmes plus sécurisées dans votre infrastructure.  
  
Bien que vous ne devez pas implémenter les processus de mise à jour manuelle de systèmes sécurisés, vous devez configurer une infrastructure de mise à jour de la sécurisation des systèmes distincte. Même dans les organisations de très grande taille, cette infrastructure peut être implémentée généralement via des serveurs WSUS dédiés et stratégie de groupe pour les systèmes sécurisés.  
  
### <a name="blocking-internet-access"></a>Blocage de l’accès Internet  
Hôtes d’administration ne doivent pas être autorisés à accéder à Internet, ni doit pouvoir parcourir intranet d’une organisation. Navigateurs Web et applications similaires ne doivent être autorisées sur les ordinateurs hôtes d’administration. Vous pouvez bloquer l’accès à Internet pour les ordinateurs hôtes sécurisés via une combinaison de paramètres de pare-feu de périmètre, la configuration WFAS et configuration du serveur proxy sur des ordinateurs hôtes sécurisés «trou noir». Vous pouvez également utiliser la liste verte d’applications pour empêcher l’utilisation sur des ordinateurs hôtes d’administration des navigateurs web.  
  
### <a name="virtualization"></a>Virtualisation  
Si possible, envisagez d’implémenter des machines virtuelles en tant qu’ordinateurs hôtes d’administration. À l’aide de la virtualisation, vous pouvez créer par utilisateur des systèmes d’administration qui sont stockés et gérés de manière centralisée, et qui peut être facilement arrêté lorsque pas en cours d’utilisation, garantissant qu’informations d’identification ne sont pas laissées actives sur les systèmes d’administration. Vous pouvez également exiger que les ordinateurs hôtes d’administration virtual sont rétablies à une capture instantanée initiale après chaque utilisation, garantissant que les ordinateurs virtuels demeurent inchangés. Plus d’informations sur les options pour la virtualisation d’ordinateurs hôtes d’administration sont fournies dans la section suivante.  
  
## <a name="sample-approaches-to-implementing-secure-administrative-hosts"></a>Exemple des approches pour implémenter des sécuriser les ordinateurs hôtes d’administration  
Quelle que soit la conception et le déploiement de votre infrastructure de l’hôte d’administration, vous devez garder à l’esprit les recommandations fournies dans «Principes de création sécurisé hôtes d’administration» plus haut dans cette rubrique. Chacune des méthodes décrites ici fournit des informations générales sur la façon dont vous pouvez séparer «administration» et les systèmes de «productivité» utilisés par votre personnel informatique. Les systèmes de productivité sont des ordinateurs qui les administrateurs informatiques utilisent pour vérifier votre messagerie électronique, naviguer sur Internet et à utiliser des logiciels de productivité générales telles que MicrosoftOffice. Systèmes d’administration sont des ordinateurs qui sont renforcés et dédiés à utiliser pour l’administration quotidienne d’un environnement informatique.  
  
Le moyen le plus simple à implémenter des ordinateurs hôtes d’administration sécurisées consiste à fournir à votre personnel informatique sécurisées stations de travail à partir duquel ils peuvent effectuer des tâches d’administration. Dans une implémentation de station de travail uniquement, chaque station de travail d’administration est utilisée pour lancer les outils de gestion et les connexions RDP pour gérer les serveurs et toute autre infrastructure. Les implémentations de station de travail uniquement peuvent être efficaces dans les organisations de petite taille, bien que les infrastructures de plus grande et plus complexes susceptibles de bénéficier d’une conception distribuée pour les ordinateurs hôtes d’administration dans lequel les stations de travail et des serveurs d’administration dédiés sont utilisées, comme décrit dans «Implémentation sécurisé d’administration stations de travail et raccourcis serveurs» plus loin dans cette rubrique.  
  
### <a name="implementing-separate-physical-workstations"></a>Implémentation des postes de travail physiques distincts  
Une façon que vous pouvez implémenter des hôtes d’administration consiste à émettre des deux stations de travail de chaque utilisateur de l’informatique. Une station de travail est utilisée avec un compte d’utilisateur «normal» pour effectuer des activités telles que la vérification de la messagerie et à l’aide des applications de productivité, tandis que la deuxième station de travail est strictement dédiée aux fonctions d’administration.  
  
Pour la station de travail de productivité, le personnel informatique peut recevoir les comptes d’utilisateur standard au lieu d’utiliser des comptes privilégiés pour vous connecter à des ordinateurs non sécurisés. La station de travail d’administration doit être configurée avec une configuration strictement contrôlée et le personnel informatique doit utiliser un autre compte pour vous connecter à la station de travail d’administration.  
  
Si vous avez implémenté les cartes à puce, stations de travail d’administration doivent être configurées pour exiger des ouvertures de session de carte à puce, et le personnel informatique doit disposer de comptes distincts pour une utilisation d’administration, également configurée pour exiger des cartes à puce pour l’ouverture de session interactive. L’hôte d’administration doit être renforcée comme décrit précédemment, et seuls les utilisateurs désignés informatique doivent être autorisées à se connecter localement à la station de travail d’administration.  
  
#### <a name="pros"></a>Professionnels de  
En implémentant des systèmes physiques distincts, vous pouvez vous assurer que chaque ordinateur est configuré de manière appropriée pour son rôle, et que les utilisateurs ne peuvent pas exposer par inadvertance des systèmes d’administration à des risques.  
  
#### <a name="cons"></a>Inconvénients  
  
-   Implémentation des ordinateurs physiques distincts augmente les coûts du matériel.  
  
-   Ouverture de session un ordinateur physique avec les informations d’identification qui sont utilisés pour administrer des systèmes distants met en cache les informations d’identification dans la mémoire.  
  
-   Si les stations de travail d’administration ne sont pas stockées en toute sécurité, il peuvent être vulnérables via des mécanismes tels que les enregistreurs de matériel physique ou d’autres attaques physiques.  
  
### <a name="implementing-a-secure-physical-workstation-with-a-virtualized-productivity-workstation"></a>Implémentation d’une station de travail physique sécurisée avec une station de travail de productivité virtualisé  
Dans cette approche, les utilisateurs reçoivent une station de travail d’administration sécurisée à partir duquel ils peuvent effectuer des tâches d’administration quotidiennes, à l’aide des outils d’Administration de serveur distant (RSAT) ou des connexions RDP à des serveurs au sein de leur étendue de responsabilité. Lorsque les utilisateurs doivent effectuer les tâches de productivité, ils peuvent se connecter via RDP à une station de travail de productivité à distance en cours d’exécution en tant qu’un ordinateur virtuel. Les informations d’identification distinctes doivent être utilisées pour chaque station de travail, et les contrôles tels que des cartes à puce doivent être implémentés.  
  
#### <a name="pros"></a>Professionnels de  
  
-   Stations de travail de productivité et de stations de travail d’administration sont séparées.  
  
-   LE personnel informatique à l’aide de stations de travail sécurisées pour se connecter à des stations de travail de productivité peut utiliser les informations d’identification distinctes et les cartes à puce et les informations d’identification privilégiées ne sont pas déposées sur l’ordinateur moins sécurisé.  
  
#### <a name="cons"></a>Inconvénients  
  
-   Implémentation de la solution requiert les options de conception et de mise en œuvre et la virtualisation fiable.  
  
-   Si les stations de travail physiques ne sont pas stockées en toute sécurité, il peuvent être vulnérables aux attaques physiques qui compromettre le matériel ou le système d’exploitation et de les rendre peuvent être interceptées de communications.  
  
### <a name="implementing-a-single-secure-workstation-with-connections-to-separate-productivity-and-administrative-virtual-machines"></a>Implémentation d’une station de travail sécurisée unique avec des connexions pour séparer les «Productivité» et «Administration» les ordinateurs virtuels  
Dans cette approche, vous pouvez émettre aux utilisateurs une station de travail physique unique, qui est verrouillé comme décrit précédemment, et sur lequel les utilisateurs n’ont pas accès privilégié. Vous pouvez fournir des connexions des Services Bureau à distance aux ordinateurs virtuels hébergés sur des serveurs dédiés, en fournissant le personnel informatique avec un ordinateur virtuel qui s’exécute par courrier électronique et autres applications de productivité et une deuxième machine virtuelle qui est configurée en tant qu’hôte de l’utilisateur d’administration dédiée.  
  
Vous devez exiger carte à puce ou autres multifacteur d’ouverture de session pour les ordinateurs virtuels, à l’aide des comptes distincts autre que le compte qui est utilisé pour ouvrir une session sur l’ordinateur physique. Un utilisateur de l’informatique se connecte à un ordinateur physique, qu’ils peuvent utiliser leur carte à puce de productivité pour se connecter à leur ordinateur de productivité à distance et un compte distinct et la carte à puce pour se connecter à leur ordinateur d’administration à distance.  
  
#### <a name="pros"></a>Professionnels de  
  
-   Utilisateurs de l’informatique peuvent utiliser une station de travail physique unique.  
  
-   En exigeant des comptes distincts pour les ordinateurs virtuels hôtes et à l’aide de connexions des Services Bureau à distance aux ordinateurs virtuels, informations d’identification des utilisateurs de l’informatique ne sont pas mises en cache dans la mémoire sur l’ordinateur local.  
  
-   L’hôte physique peut être sécurisé au même degré en tant qu’ordinateurs hôtes d’administration, ce qui réduit le risque de compromission de l’ordinateur local.  
  
-   Dans les cas dans lesquels machine virtuelle de la productivité de l’utilisateur d’une informatique ou leur ordinateur virtuel d’administration a peut-être été compromis, l’ordinateur virtuel peut facilement être réinitialisé à un état «bien connu».  
  
-   Si l’ordinateur physique est compromis, aucune information d’identification privilégiée n’est mises en cache dans la mémoire et l’utilisation de cartes à puce peut empêcher la compromission des informations d’identification par l’enregistreur de frappe.  
  
#### <a name="cons"></a>Inconvénients  
  
-   Implémentation de la solution requiert les options de conception et de mise en œuvre et la virtualisation fiable.  
  
-   Si les stations de travail physiques ne sont pas stockées en toute sécurité, il peuvent être vulnérables aux attaques physiques qui compromettre le matériel ou le système d’exploitation et de les rendre peuvent être interceptées de communications.  
  
### <a name="implementing-secure-administrative-workstations-and-jump-servers"></a>Implémentation de stations de travail d’administration sécurisées et de serveurs de renvoi  
Comme alternative à sécuriser les stations de travail d’administration, ou en combinaison avec eux, vous pouvez implémenter des serveurs de renvoi sécurisé, et les utilisateurs administratifs peuvent se connecter aux serveurs de renvoi à l’aide de RDP et les cartes à puce pour effectuer des tâches d’administration.  
  
Serveurs de renvoi doivent être configurés pour exécuter le rôle de passerelle Bureau à distance pour vous permettre d’implémenter des restrictions sur les connexions pour le serveur de renvoi et les serveurs de destination qui seront gérés à partir de celui-ci. Si possible, vous devez également installer le rôle Hyper-V et créer [des bureaux virtuels personnels](https://technet.microsoft.com/library/dd759174.aspx) ou d’autres ordinateurs virtuels par l’utilisateur pour les utilisateurs administratifs à utiliser pour leurs tâches sur les serveurs de renvoi.  
  
En donnant les utilisateurs administratifs par utilisateur, les ordinateurs virtuels sur le serveur de renvoi, vous fournissez une sécurité physique pour les stations de travail d’administration et les utilisateurs administratifs peuvent réinitialiser ou arrêter leurs ordinateurs virtuels quand elles pas en cours d’utilisation. Si vous préférez ne pas installer le rôle Hyper-V et le rôle de passerelle Bureau à distance sur le même hôte d’administration, vous pouvez les installer sur des ordinateurs distincts.  
  
Dès que possible, outils d’administration à distance doivent servir à gérer les serveurs. La fonctionnalité Outils d’Administration de serveur distant (RSAT) doit être installée sur les ordinateurs virtuels des utilisateurs (ou le serveur de renvoi si vous implémentez pas les ordinateurs virtuels par l’utilisateur pour l’administration), et le personnel administratif doit se connecter à leurs ordinateurs virtuels pour effectuer des tâches d’administration via RDP.  
  
Dans le cas lorsqu’un utilisateur administratif doit se connecter à un serveur de destination pour le gérer directement via RDP passerelle Bureau à distance doit être configuré pour autoriser la connexion à être effectuée uniquement si l’utilisateur approprié et l’ordinateur sont utilisés pour établir la connexion au serveur de destination. L’exécution de serveur distant (ou similaire) outils doivent être interdit sur les systèmes qui ne sont pas désignées systèmes de gestion, tels que les stations de travail à usage général et les serveurs membres qui sont pas passer de serveurs.  
  
#### <a name="pros"></a>Professionnels de  
  
-   Création de serveurs de renvoi vous permet de mapper des serveurs spécifiques à des «zones» (collections de systèmes ayant les mêmes exigences de configuration, de connexion et de sécurité) dans votre réseau et qu’ils exigent que l’administration de chaque zone est obtenue par le personnel d’administration se connectant à partir des hôtes d’administration sécurisées à un serveur désigné «zone».  
  
-   En mappant les serveurs de renvoi aux zones, vous pouvez implémenter des contrôles granulaires des propriétés de connexion et la configuration requise et pouvez facilement identifier les tentatives de connexion à partir des systèmes non autorisés.  
  
-   En mettant en œuvre par l’administrateur des ordinateurs virtuels sur les serveurs de renvoi, appliquer arrêt et réinitialiser les ordinateurs virtuels dans un état cohérent connu lors de la réalisation des tâches d’administration. En appliquant arrêt (ou redémarrer) des ordinateurs virtuels lors de la réalisation des tâches d’administration, les ordinateurs virtuels ne peuvent pas être ciblés par les personnes malveillantes, ni sont des attaques par vol d’informations d’identification possible, car la mise en mémoire cache les informations d’identification ne sont pas conservés au-delà d’un redémarrage.  
  
#### <a name="cons"></a>Inconvénients  
  
-   Des serveurs dédiés sont requis pour les serveurs de renvoi physique ou virtuel.  
  
-   Implémentation désigné raccourcis des serveurs et stations de travail d’administration requiert une planification et configuration qui mappe à toutes les zones de sécurité configurés dans l’environnement.  
  


