---
ms.assetid: 50bd2566-e03c-4884-b5c4-895c8aab80aa
title: Identification des participants au projet de déploiement
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 516c37165952e46c8e6e76499909e90851e305ce
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822312"
---
# <a name="identifying-the-deployment-project-participants"></a>Identification des participants au projet de déploiement

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La première étape de l’établissement d’un projet de déploiement pour domaine Active Directory Service (AD DS) consiste à établir les équipes de projet de conception et de déploiement qui seront chargées de gérer la phase de conception et la phase de déploiement du cycle de projet Active Directory. En outre, vous devez identifier les individus et les groupes qui seront responsables du propriétaire et de la maintenance de l’annuaire une fois le déploiement terminé.  
  
-   [Définition des rôles spécifiques au projet](#BKMK_1)  
  
-   [Établissement des propriétaires et administrateurs](#BKMK_2)  
  
-   [Génération d’équipes de projet](#BKMK_3)  
  
## <a name="defining-project-specific-roles"></a><a name="BKMK_1"></a>Définition des rôles spécifiques au projet  
L’une des étapes importantes de l’établissement des équipes de projet consiste à identifier les personnes qui doivent détenir des rôles spécifiques à un projet. Il s’agit notamment du sponsor exécutif, de l’architecte de projet et du chef de projet. Ces personnes sont responsables de l’exécution du projet de déploiement Active Directory.  
  
Une fois que vous avez désigné l’architecte de projet et le chef de projet, ces personnes établissent des canaux de communication au sein de l’organisation, créent des planifications de projet et identifient les personnes qui seront membres des équipes de projet, en commençant par les différents propriétaires.  
  
### <a name="executive-sponsor"></a>Sponsor exécutif  
Le déploiement d’une infrastructure comme AD DS peut avoir un impact important sur une organisation. Pour cette raison, il est important de disposer d’un parrain dirigeant qui comprend la valeur métier du déploiement, qui prend en charge le projet au niveau exécutif et peut aider à résoudre les conflits au sein de l’organisation.  
  
### <a name="project-architect"></a>Architecte de projet  
Chaque projet de déploiement Active Directory nécessite un architecte de projet pour gérer le processus de prise de décision de conception et de déploiement Active Directory. L’architecte fournit une expertise technique pour faciliter le processus de conception et de déploiement de AD DS.  
  
> [!NOTE]  
> Si aucun personnel existant de votre organisation ne dispose de l’expérience de conception d’annuaire, vous souhaiterez peut-être embaucher un consultant extérieur qui est un expert en matière de conception et de déploiement de Active Directory.  
  
Les responsabilités de l’architecte de projet Active Directory sont les suivantes :  
  
-   Propriétaire de la conception de la Active Directory  
  
-   Compréhension et enregistrement du raisonnement pour les décisions de conception clés  
  
-   S’assurer que la conception répond aux besoins de l’entreprise  
  
-   Établissement d’un consensus entre les équipes de conception, de déploiement et d’exploitation  
  
-   Comprendre les besoins des applications intégrées à AD DS  
  
La dernière conception de Active Directory doit refléter une combinaison de objectifs commerciaux et de décisions techniques. Par conséquent, l’architecte du projet doit passer en revue les décisions de conception pour s’assurer qu’il s’aligne avec les objectifs de l’entreprise.  
  
### <a name="project-manager"></a>Chef de projet  
Le chef de projet facilite la coopération entre les divisions et entre les groupes de gestion des technologies. Dans l’idéal, le responsable de projet de déploiement Active Directory est un membre de l’organisation qui est familiarisé avec les stratégies opérationnelles du groupe informatique et les exigences de conception pour les groupes qui se préparent à déployer AD DS. Le responsable de projet supervise l’intégralité du projet de déploiement, en commençant par la conception et en poursuivant l’implémentation, et s’assure que le projet reste dans les délais et dans le budget. Les responsabilités du chef de projet sont les suivantes :  
  
-   Mise en place d’une planification de projet de base, telle que la planification et la budgétisation  
  
-   Progression du projet de conception et de déploiement de Active Directory  
  
-   S’assurer que les personnes appropriées sont impliquées dans chaque partie du processus de conception  
  
-   Utilisation d’un point de contact unique pour le projet de déploiement Active Directory  
  
-   Établissement de la communication entre les équipes de conception, de déploiement et d’exploitation  
  
-   Établissement et maintien de la communication avec le sponsor exécutif tout au long du projet de déploiement  
  
## <a name="establishing-owners-and-administrators"></a><a name="BKMK_2"></a>Établissement des propriétaires et administrateurs  
Dans un projet de déploiement Active Directory, les individus propriétaires sont tenus responsables par la direction pour s’assurer que les tâches de déploiement sont terminées et que Active Directory spécifications de conception répondent aux besoins de l’organisation. Les propriétaires n’ont pas nécessairement accès ou manipulent directement l’infrastructure d’annuaire. Les administrateurs sont les personnes responsables de la réalisation des tâches de déploiement requises. Les administrateurs disposent de l’accès réseau et des autorisations nécessaires pour manipuler l’annuaire et son infrastructure.  
  
Le rôle du propriétaire est stratégique et responsable. Les propriétaires sont responsables de la communication des administrateurs aux tâches nécessaires à l’implémentation de la conception de la Active Directory, telles que la création de contrôleurs de domaine dans la forêt. Les administrateurs sont responsables de l’implémentation de la conception sur le réseau conformément aux spécifications de conception.  
  
Dans les grandes organisations, les différents individus remplissent les rôles de propriétaire et d’administrateur ; Toutefois, dans certaines petites organisations, la même personne peut agir à la fois comme propriétaire et comme administrateur.  
  
### <a name="service-and-data-owners"></a>Propriétaires de services et de données  
La gestion quotidienne des AD DS implique deux types de propriétaires :  
  
-   Les propriétaires de service qui sont responsables de la planification et de la maintenance à long terme de l’infrastructure Active Directory et pour s’assurer que le répertoire continue de fonctionner et que les objectifs établis dans les contrats de niveau de service sont conservés.  
  
-   Les propriétaires de données responsables de la maintenance des informations stockées dans l’annuaire. Cela comprend la gestion et la gestion des comptes d’utilisateurs et d’ordinateurs des ressources locales telles que les serveurs membres et les stations de travail.  
  
Il est important d’identifier le service Active Directory et les propriétaires de données tôt afin qu’ils puissent participer à la plus grande partie du processus de conception possible. Étant donné que les propriétaires de services et de données sont responsables de la maintenance à long terme de l’annuaire à la fin du projet de déploiement, il est important pour ces personnes de fournir des informations sur les besoins de l’organisation et de savoir comment et pourquoi certaines décisions de conception ont été prises. Les propriétaires de services incluent le propriétaire de la forêt, le propriétaire du système de nommage des domaine Active Directory (DNS) et le propriétaire de la topologie de site. Les propriétaires de données incluent les propriétaires d’unités d’organisation.  
  
### <a name="service-and-data-administrators"></a>Administrateurs de services et de données  
Le fonctionnement de AD DS implique deux types d’administrateurs : les administrateurs de service et les administrateurs de données. Les administrateurs de service implémentent des décisions de stratégie émises par les propriétaires de services et gèrent les tâches quotidiennes associées à la gestion du service d’annuaire et de l’infrastructure. Cela comprend la gestion des contrôleurs de domaine qui hébergent le service d’annuaire, la gestion d’autres services réseau tels que DNS qui sont requis pour AD DS, le contrôle de la configuration des paramètres à l’ensemble de la forêt et la garantie que l’annuaire est toujours disponible.  
  
Les administrateurs de service sont également chargés d’effectuer les tâches de déploiement Active Directory en cours qui sont nécessaires après la fin du processus de déploiement initial de Windows Server 2008 Active Directory. Par exemple, à mesure que les demandes sur l’annuaire augmentent, les administrateurs de service créent des contrôleurs de domaine supplémentaires et établissent ou suppriment des approbations entre les domaines, si nécessaire. Pour cette raison, l’équipe de déploiement Active Directory doit inclure les administrateurs de service.  
  
Vous devez veiller à affecter des rôles d’administrateur de service uniquement à des personnes de confiance dans l’organisation. Étant donné que ces personnes ont la possibilité de modifier les fichiers système sur les contrôleurs de domaine, elles peuvent modifier le comportement de AD DS. Vous devez vous assurer que les administrateurs de services de votre organisation sont des personnes qui connaissent bien les stratégies de sécurité et opérationnelles qui sont en place sur votre réseau et qui comprennent la nécessité d’appliquer ces stratégies.  
  
Les administrateurs de données sont des utilisateurs d’un domaine qui sont responsables de la gestion des données stockées dans AD DS tels que les comptes d’utilisateurs et de groupes, et pour la maintenance des ordinateurs qui sont membres de leur domaine. Les administrateurs de données contrôlent des sous-ensembles d’objets dans le répertoire et n’ont aucun contrôle sur l’installation ou la configuration du service d’annuaire.  
  
Les comptes d’administrateur de données ne sont pas fournis par défaut. Une fois que l’équipe de conception a déterminé comment les ressources doivent être gérées pour l’organisation, les propriétaires du domaine doivent créer des comptes d’administrateur de données et les déléguer aux autorisations appropriées en fonction de l’ensemble d’objets dont les administrateurs sont responsables.  
  
Il est préférable de limiter le nombre d’administrateurs de service au sein de votre organisation au nombre minimal requis pour s’assurer que l’infrastructure continue à fonctionner. La majorité des tâches administratives peuvent être effectuées par les administrateurs de données. Les administrateurs de service ont besoin d’un ensemble de compétences plus larges, car ils sont responsables de la gestion de l’annuaire et de l’infrastructure qui le prend en charge. Les administrateurs de données n’ont besoin que des compétences nécessaires pour gérer leur partie de l’annuaire. La Division des affectations de travail de cette manière entraîne des économies pour l’organisation, car seul un petit nombre d’administrateurs doivent être formés pour fonctionner et gérer l’ensemble de l’annuaire et de son infrastructure.  
  
Par exemple, un administrateur de service doit comprendre comment ajouter un domaine à une forêt. Cela explique comment installer le logiciel pour convertir un serveur en contrôleur de domaine et comment manipuler l’environnement DNS afin que le contrôleur de domaine puisse être fusionné en toute transparence dans l’environnement de Active Directory. Un administrateur de données a uniquement besoin de savoir comment gérer les données spécifiques dont il est responsable, telles que la création de nouveaux comptes d’utilisateur pour les nouveaux employés de leur service.  
  
Le déploiement de AD DS nécessite la coordination et la communication entre de nombreux groupes différents impliqués dans le fonctionnement de l’infrastructure réseau. Ces groupes doivent désigner les propriétaires de services et de données qui sont responsables de la représentation des différents groupes pendant le processus de conception et de déploiement.  
  
Une fois le projet de déploiement terminé, ces propriétaires de services et de données continuent à être responsables de la partie de l’infrastructure gérée par leur groupe. Dans un environnement Active Directory, ces propriétaires sont le propriétaire de la forêt, le DNS pour AD DS propriétaire, le propriétaire de la topologie de site et le propriétaire de l’unité d’organisation. Les rôles de ces propriétaires de services et de données sont expliqués dans les sections suivantes.  
  
#### <a name="forest-owner"></a>Propriétaire de la forêt  
Le propriétaire de la forêt est généralement responsable informatique au sein de l’organisation qui est responsable de l’Active Directory processus de déploiement et qui est finalement responsable de la maintenance de la livraison de service au sein de la forêt une fois le déploiement terminé. Le propriétaire de la forêt affecte les individus pour remplir les autres rôles de propriété en identifiant le personnel clé au sein de l’organisation qui est en mesure de fournir les informations nécessaires sur l’infrastructure réseau et les besoins administratifs. Le propriétaire de la forêt est responsable des opérations suivantes :  
  
-   Déploiement du domaine racine de la forêt pour créer la forêt  
  
-   Déploiement du premier contrôleur de domaine dans chaque domaine pour créer les domaines requis pour la forêt  
  
-   Appartenances des groupes d’administrateurs de service dans tous les domaines de la forêt  
  
-   Création de la structure de l’unité d’organisation pour chaque domaine de la forêt  
  
-   Délégation de l’autorité administrative aux propriétaires d’UO  
  
-   Modifications apportées au schéma  
  
-   Modifications apportées aux paramètres de configuration à l’ensemble de la forêt  
  
-   Implémentation de certains paramètres de stratégie de stratégie de groupe, y compris les stratégies de compte d’utilisateur de domaine, telles que le mot de passe affiné et la stratégie de verrouillage de compte  
  
-   Paramètres de stratégie d’entreprise qui s’appliquent aux contrôleurs de domaine  
  
-   Tout autre stratégie de groupe paramètres appliqués au niveau du domaine  
  
Le propriétaire de la forêt a l’autorité sur l’ensemble de la forêt. Il incombe au propriétaire de la forêt de définir les stratégie de groupe et les stratégies d’entreprise, et de sélectionner les personnes qui sont des administrateurs de service. Le propriétaire de la forêt est un propriétaire de service.  
  
#### <a name="dns-for-ad-ds-owner"></a>DNS pour AD DS propriétaire  
Le DNS pour AD DS propriétaire est une personne qui a une compréhension approfondie de l’infrastructure DNS existante et de l’espace de noms existant de l’organisation.  
  
Le DNS pour AD DS propriétaire est responsable des opérations suivantes :  
  
-   Faire office de liaison entre l’équipe de conception et le groupe informatique qui possède actuellement l’infrastructure DNS  
  
-   Fournir des informations sur l’espace de noms DNS existant de l’Organisation pour faciliter la création du nouvel espace de noms Active Directory  
  
-   Collaborez avec l’équipe de déploiement pour vous assurer que la nouvelle infrastructure DNS est déployée en fonction des spécifications de l’équipe de conception et qu’elle fonctionne correctement.  
  
-   Gestion du DNS pour l’infrastructure AD DS, y compris le service serveur DNS et les données DNS  
  
Le DNS pour AD DS propriétaire est un propriétaire de service.  
  
#### <a name="site-topology-owner"></a>Propriétaire de la topologie de site  
Le propriétaire de la topologie de site est familiarisé avec la structure physique du réseau de l’organisation, y compris le mappage des sous-réseaux individuels, des routeurs et des zones réseau qui sont connectés par le biais de liaisons lentes. Le propriétaire de la topologie de site est responsable des opérations suivantes :  
  
-   Fonctionnement de la topologie du réseau physique et de son impact sur AD DS  
  
-   Comprendre comment le déploiement de Active Directory a un impact sur le réseau  
  
-   Détermination de la Active Directory les sites logiques qui doivent être créés  
  
-   Mise à jour des objets de site pour les contrôleurs de domaine lors de l’ajout, de la modification ou de la suppression d’un sous-réseau  
  
-   Création de liens de sites, de ponts entre liens de sites et d’objets de connexion manuels  
  
Le propriétaire de la topologie de site est un propriétaire de service.  
  
#### <a name="ou-owner"></a>Propriétaire de l’unité d’organisation  
Le propriétaire de l’unité d’organisation est responsable de la gestion des données stockées dans l’annuaire. Cet individu doit être familiarisé avec les stratégies opérationnelles et de sécurité en place sur le réseau. Les propriétaires d’UO ne peuvent exécuter que les tâches qui leur ont été déléguées par les administrateurs de service, et ils ne peuvent exécuter que les tâches sur les unités d’organisation auxquelles ils sont affectés. Les tâches qui peuvent être assignées au propriétaire de l’unité d’organisation sont les suivantes :  
  
-   Exécution de toutes les tâches de gestion de compte au sein de l’unité d’organisation attribuée  
  
-   Gestion des postes de travail et des serveurs membres membres de l’unité d’organisation attribuée  
  
-   Délégation de l’autorité aux administrateurs locaux au sein de l’unité d’organisation attribuée  
  
Le propriétaire de l’unité d’organisation est un propriétaire de données.  
  
## <a name="building-project-teams"></a><a name="BKMK_3"></a>Génération d’équipes de projet  
Active Directory équipes de projet sont des groupes temporaires chargés d’effectuer Active Directory tâches de conception et de déploiement. Une fois le projet de déploiement de Active Directory terminé, les propriétaires assument la responsabilité de l’annuaire et les équipes de projet peuvent DISBAND.  
  
La taille des équipes de projet varie en fonction de la taille de l’organisation. Dans les petites organisations, une seule personne peut couvrir plusieurs domaines de responsabilité dans une équipe de projet et être impliquée dans plusieurs phases du déploiement. Les grandes organisations peuvent nécessiter des équipes de plus grande taille avec des personnes différentes ou même des équipes différentes couvrant les différents domaines de responsabilité. La taille des équipes n’est pas importante tant que tous les domaines de responsabilité sont affectés, et les objectifs de conception de l’organisation sont atteints.  
  
### <a name="identifying-potential-forest-owners"></a>Identification des propriétaires potentiels de la forêt  
Identifiez les groupes au sein de votre organisation qui possèdent et contrôlent les ressources nécessaires pour fournir des services d’annuaire aux utilisateurs sur le réseau. Ces groupes sont considérés comme des propriétaires de forêts potentiels.  
  
La séparation de l’administration des services et des données dans AD DS permet à l’infrastructure (ou aux groupes) d’une organisation de gérer le service d’annuaire, tandis que les administrateurs locaux de chaque groupe gèrent les données qui appartiennent à leurs propres groupes. Les propriétaires de forêts potentiels disposent de l’autorité requise sur l’infrastructure réseau pour déployer et prendre en charge AD DS.  
  
Pour les organisations qui ont un groupe informatique centralisé, le groupe informatique est généralement le propriétaire de la forêt et, par conséquent, le propriétaire de la forêt potentielle pour tous les déploiements futurs. Les organisations qui incluent un certain nombre de groupes informatiques d’infrastructure indépendants ont un certain nombre de propriétaires de forêts potentiels. Si votre organisation dispose déjà d’une infrastructure Active Directory en place, tous les propriétaires de forêts actuels sont également des propriétaires de forêts potentiels pour les nouveaux déploiements.  
  
Sélectionnez l’un des propriétaires de forêts potentiels comme propriétaire de la forêt pour chaque forêt que vous envisagez de déployer. Ces propriétaires de forêts potentiels sont responsables de l’utilisation de l’équipe de conception pour déterminer si leur forêt est réellement déployée ou si une autre voie d’action (telle que la jonction à une autre forêt existante) est une meilleure utilisation des ressources disponibles, tout en répondant à leurs besoins. Le propriétaire de la forêt (ou les propriétaires) de votre organisation est membre de l’équipe de conception Active Directory.  
  
### <a name="establishing-a-design-team"></a>Établissement d’une équipe de conception  
L’équipe de conception Active Directory est chargée de rassembler toutes les informations nécessaires pour prendre des décisions sur la conception de la structure logique Active Directory.  
  
Les responsabilités de l’équipe de conception sont les suivantes :  
  
-   Détermination du nombre de forêts et de domaines requis et des relations entre les forêts et les domaines  
  
-   Travailler avec des propriétaires de données pour s’assurer que la conception respecte leurs exigences en matière de sécurité et d’administration  
  
-   Travailler avec les administrateurs réseau actuels pour s’assurer que l’infrastructure réseau actuelle prend en charge la conception et que la conception ne risque pas de nuire aux applications existantes déployées sur le réseau  
  
-   Collaboration avec les représentants du groupe de sécurité de l’Organisation pour s’assurer que la conception respecte les stratégies de sécurité établies  
  
-   Conception de structures d’UO qui autorisent des niveaux de protection appropriés et la délégation d’autorité appropriée aux propriétaires de données  
  
-   Collaborez avec l’équipe de déploiement pour tester la conception dans un environnement de laboratoire afin de vous assurer qu’elle fonctionne comme prévu et de modifier la conception si nécessaire pour résoudre les problèmes qui se produisent.  
  
-   Création d’une conception de topologie de site qui répond aux exigences de réplication de la forêt tout en empêchant la surcharge de la bande passante disponible. Pour plus d’informations sur la conception de la topologie de site, voir [conception de la topologie de site pour Windows Server 2008 AD DS](https://technet.microsoft.com/library/cc772013.aspx).  
  
-   Travailler avec l’équipe de déploiement pour s’assurer que la conception est correctement implémentée  
  
L’équipe de conception comprend les membres suivants :  
  
-   Propriétaires potentiels de la forêt  
  
-   Architecte de projet  
  
-   Chef de projet  
  
-   Personnes responsables de l’établissement et de la maintenance des stratégies de sécurité sur le réseau  
  
Pendant le processus de conception de la structure logique, l’équipe de conception identifie les autres propriétaires. Ces personnes doivent commencer à participer au processus de conception dès qu’elles sont identifiées. Une fois le projet de déploiement transmis à l’équipe de déploiement, l’équipe de conception est chargée de surveiller le processus de déploiement pour s’assurer que la conception est correctement implémentée. L’équipe de conception apporte également des modifications à la conception en fonction des commentaires des tests.  
  
### <a name="establishing-a-deployment-team"></a>Établissement d’une équipe de déploiement  
L’équipe de déploiement Active Directory est chargée du test et de l’implémentation de la conception de la structure logique Active Directory. Cela implique les tâches suivantes :  
  
-   Mise en place d’un environnement de test qui émule suffisamment l’environnement de production  
  
-   Test de la conception en implémentant la forêt et la structure de domaine proposées dans un environnement Lab pour vérifier qu’elle répond aux objectifs de chaque propriétaire de rôle  
  
-   Développement et test de tous les scénarios de migration proposés par la conception dans un environnement Lab  
  
-   Vérification que chaque propriétaire se déconnecte du processus de test pour s’assurer que les fonctionnalités de conception correctes sont testées  
  
-   Test de l’opération de déploiement dans un environnement pilote  
  
Une fois les tâches de conception et de test terminées, l’équipe de déploiement effectue les tâches suivantes :  
  
-   Crée les forêts et les domaines selon la conception de la structure logique Active Directory  
  
-   Crée les sites et les objets de lien de sites selon les besoins en fonction de la conception de la topologie de site  
  
-   Garantit que l’infrastructure DNS est configurée pour prendre en charge AD DS et que tous les nouveaux espaces de noms sont intégrés à l’espace de noms existant de l’Organisation  
  
L’équipe de déploiement Active Directory comprend les membres suivants :  
  
-   Propriétaire de la forêt  
  
-   DNS pour AD DS propriétaire  
  
-   Propriétaire de la topologie de site  
  
-   Propriétaires d’UO  
  
L’équipe de déploiement collabore avec le service et les administrateurs de données au cours de la phase de déploiement pour s’assurer que les membres de l’équipe d’exploitation connaissent bien la nouvelle conception. Cela permet d’assurer une transition sans heurts de la propriété lorsque l’opération de déploiement est terminée. À la fin du processus de déploiement, la responsabilité de la gestion du nouvel environnement de Active Directory passe à l’équipe des opérations.  
  
### <a name="documenting-the-design-and-deployment-teams"></a>Documentation des équipes de conception et de déploiement  
Documentez les noms et les informations de contact des personnes qui feront partie de la conception et du déploiement de AD DS. Identifiez qui sera responsable de chaque rôle sur les équipes de conception et de déploiement. Initialement, cette liste comprend les propriétaires de forêts potentiels, le chef de projet et l’architecte de projet. Lorsque vous déterminez le nombre de forêts que vous allez déployer, vous devrez peut-être créer de nouvelles équipes de conception pour des forêts supplémentaires. Notez que vous devez mettre à jour votre documentation au fur et à mesure que vous identifiez les différents propriétaires de Active Directory pendant le processus de conception. Pour obtenir une feuille de travail qui vous aide à documenter les équipes de conception et de déploiement pour chaque forêt, téléchargez Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip à partir des outils d’aide pour le kit de déploiement de Windows Server 2003 ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)) et ouvrez « informations sur l’équipe de conception et de déploiement » (DSSLOGI_1. doc).  
  


