---
ms.assetid: 50bd2566-e03c-4884-b5c4-895c8aab80aa
title: "Identifier les participants au projet de déploiement"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 57c24c2b2b48c712445ef7dca453de586d33a130
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="identifying-the-deployment-project-participants"></a>Identifier les participants au projet de déploiement

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

La première étape de l’établissement d’un projet de déploiement pour les services de domaine Active Directory (AD DS) consiste à établir la conception et du cycle des équipes de projet de déploiement qui seront chargés de gérer la phase de conception et de la phase de déploiement du projet Active Directory. En outre, vous devez identifier les personnes et les groupes qui seront chargés de possession et de maintenance du répertoire après que le déploiement est terminé.  
  
-   [Définition des rôles spécifiques à un projet](#BKMK_1)  
  
-   [Établissement des propriétaires et des administrateurs](#BKMK_2)  
  
-   [Équipes de projet de création](#BKMK_3)  
  
## <a name="BKMK_1"></a>Définition des rôles spécifiques à un projet  
Une étape importante pour établir les associations de projet consiste à identifier les personnes qui conservent des rôles spécifiques du projet. Ceux-ci incluent le soutien de la direction, l’architecte du projet et le chef de projet. Ces personnes sont responsables de l’exécution du projet de déploiement Active Directory.  
  
Une fois que vous désigne l’architecte du projet et le chef de projet, ces personnes établissent des canaux de communication au sein de l’organisation, créer des calendriers de projet et identifient les personnes qui seront membres des équipes de projet, en commençant par les propriétaires différents.  
  
### <a name="executive-sponsor"></a>Dirigeant  
Déploiement d’une infrastructure tels que les services AD DS peut avoir un impact sur une organisation. Pour cette raison, il est important d’avoir un dirigeant qui comprend la valeur métier du déploiement, prend en charge le projet au niveau exécutif et peut aider à résoudre les conflits dans l’organisation.  
  
### <a name="project-architect"></a>Architecte du projet  
Chaque projet de déploiement Active Directory requiert un architecte du projet gérer le processus de prise de décision Active Directory conception et de déploiement. L’architecte fournit une expertise technique pour faciliter le processus de conception et déploiement des services AD DS.  
  
> [!NOTE]  
> Si aucun membre du personnel existant dans votre organisation n’ont conception Active expérience, vous souhaiterez d’engager un consultant externe qui est un expert de la conception d’Active Directory et de déploiement.  
  
Responsabilités de l’architecte du projet Active Directory sont les suivantes:  
  
-   Propriétaire de la conception d’Active Directory  
  
-   Présentation et en enregistrant la logique pour les décisions de conception clés  
  
-   S’assurer que la conception répond aux besoins de l’organisation  
  
-   Établissement de consensus entre les équipes de conception, déploiement et opérations  
  
-   Comprendre les besoins des applications pour AD DS intégré  
  
La conception d’Active Directory finale doit refléter une combinaison d’objectifs commerciaux et techniques décisions. Par conséquent, l’architecte du projet doive passer en revue les décisions de conception pour vous assurer qu’ils s’alignent sur les objectifs professionnels.  
  
### <a name="project-manager"></a>Chef de projet  
Le chef de projet facilite la coopération sur des unités commerciales et entre les groupes d’administration de la technologie. Dans l’idéal, le chef de projet de déploiement Active Directory est une personne à partir de l’organisation qui est familiarisée avec les deux stratégies les fonctionnement du groupe informatique et les exigences de conception pour les groupes qui sont préparent à déployer les services AD DS. Le chef de projet supervise le projet de déploiement tout entier, à partir de conception et poursuivre la mise en œuvre et permet de s’assurer que le projet reste sur la planification et de budget. Les responsabilités le chef de projet sont les suivantes:  
  
-   Projet base planification telles que la planification et la budgétisation  
  
-   Conduite progression sur le projet de conception et de déploiement Active Directory  
  
-   S’assurer que les personnes appropriées sont impliqués dans chaque partie du processus de conception  
  
-   Agissant comme un seul point de contact pour le projet de déploiement Active Directory  
  
-   Établir une communication entre les équipes de conception, déploiement et opérations  
  
-   Établissement et le maintien de la communication avec le soutien de la direction tout au long du projet de déploiement  
  
## <a name="BKMK_2"></a>Établissement des propriétaires et des administrateurs  
Dans un projet de déploiement Active Directory, les personnes qui sont propriétaires sont tenus responsables en gestion pour vous assurer que le déploiement tâches sont terminées et cette conception d’Active Directory spécifications de répondre aux besoins de l’organisation. Propriétaires de ne pas nécessairement avoir accès à ou manipuler directement de l’infrastructure d’annuaire. Les administrateurs sont les personnes responsables pour exécuter les tâches de déploiement requis. Les administrateurs ont l’accès au réseau et les autorisations nécessaires pour manipuler le répertoire et son infrastructure.  
  
Le rôle du propriétaire est stratégique et de gestion. Propriétaires sont chargés de communiquer aux administrateurs les tâches requises pour l’implémentation de la conception d’Active Directory telles que la création de nouveaux contrôleurs de domaine dans la forêt. Les administrateurs sont responsables de l’implémentation de la conception sur le réseau en fonction des spécifications de conception.  
  
Dans les grandes organisations, différentes personnes remplir des rôles d’administrateur; Toutefois, dans certaines entreprises de petite taille, la même personne peut agir en tant que le propriétaire et l’administrateur.  
  
### <a name="service-and-data-owners"></a>Propriétaires de services et données  
La gestion des services AD DS sur une base quotidienne impliquant deux types de propriétaires:  
  
-   Propriétaires de services qui sont responsables de la maintenance à long terme et de planification de l’infrastructure Active Directory et de s’assurer que le répertoire continue de fonctionner et que les objectifs établis dans les contrats de niveau de service sont conservés.  
  
-   Propriétaires de données qui sont chargés de la maintenance des informations stockées dans le répertoire. Cela inclut la gestion des comptes d’utilisateurs et gestion des ressources locales, telles que les serveurs membres et stations de travail.  
  
Il est important d’identifier rapidement les propriétaires de données et le service Active Directory afin qu’ils peuvent participer autant que possible, le processus de conception. Étant donné que les propriétaires de service et les données sont responsables de la maintenance à long terme du répertoire une fois le projet de déploiement est terminé, il est important pour ces personnes pour fournir des informations concernant les besoins de l’organisation et à vous familiariser avec comment et pourquoi certaines décisions de conception sont effectuées. Les propriétaires de services incluent le propriétaire de la forêt, le propriétaire d’Active Directory d’attribution de noms DNS (Domain System) et le propriétaire de topologie de site. Les propriétaires de données comprennent des propriétaires de l’unité d’organisation (UO).  
  
### <a name="service-and-data-administrators"></a>Administrateurs de services et de données  
Le fonctionnement des services AD DS implique deux types d’administrateurs: les administrateurs et les administrateurs de données de service. Les administrateurs de service implémentent des décisions de stratégie effectuées par les propriétaires de service et gérer les tâches quotidiennes liées à la gestion de l’infrastructure et le service d’annuaire. Cela inclut la gestion des contrôleurs de domaine qui hébergent le service d’annuaire, la gestion des autres services réseau tels que DNS qui sont requis pour les services AD DS, contrôle de la configuration des paramètres de la forêt et de s’assurer que le répertoire est toujours disponible.  
  
Les administrateurs de service sont également responsables de l’exécution de tâches de déploiement Active Directory en cours qui sont nécessaires une fois le processus de déploiement de Windows Server 2008 Active Directory initial est terminé. Par exemple, selon les besoins de l’augmentation de l’annuaire, les administrateurs de service créer d’autres contrôleurs de domaine et établissent ou supprimer des approbations entre domaines, en fonction des besoins. Pour cette raison, l’équipe de déploiement Active Directory doit inclure les administrateurs de service.  
  
Vous devez être prudent d’attribuer des rôles d’administrateur de service uniquement aux personnes de confiance dans l’organisation. Étant donné que ces personnes ont la possibilité de modifier les fichiers système sur les contrôleurs de domaine, ils peuvent modifier le comportement des services AD DS. Vous devez vous assurer que les administrateurs de service dans votre organisation sont les personnes qui sont familiarisés avec le fonctionnement et les stratégies de sécurité qui sont en place sur votre réseau et qui comprennent la nécessité d’appliquer ces stratégies.  
  
Les administrateurs de données sont des utilisateurs au sein d’un domaine qui sont chargés de gérer les données stockées dans AD DS, tels que les comptes d’utilisateur et de groupe et de la gestion des ordinateurs qui sont membres de leur domaine. Les administrateurs de données contrôlent les sous-ensembles d’objets dans le répertoire et n’ont aucun contrôle sur l’installation ou la configuration du service d’annuaire.  
  
Comptes d’administrateur de données ne sont pas fournis par défaut. Une fois que l’équipe de conception détermine comment les ressources doivent être gérés de l’organisation, les propriétaires de domaine doivent créer des comptes d’administrateur de données et leur déléguer des autorisations appropriées basées sur l’ensemble des objets pour lesquels les administrateurs doivent être chargés.  
  
Il est préférable de limiter le nombre d’administrateurs de service dans votre organisation pour le nombre minimal requis pour vous assurer que l’infrastructure continue de fonctionner. La majorité des tâches administratives peut être effectuée par les administrateurs de données. Les administrateurs de service nécessitent une quantité plus large ensemble de compétences, car ils sont responsables de la maintenance du répertoire et l’infrastructure qui prend en charge. Les administrateurs de données nécessitent uniquement les compétences nécessaires pour gérer leur partie de l’annuaire. Répartition des affectations de travail de cette façon due à réduire les coûts de l’organisation uniquement un petit nombre d’administrateurs doit être formés à exploiter et maintenir l’intégralité du répertoire et son infrastructure.  
  
Par exemple, un administrateur de service doit comprendre comment ajouter un domaine à une forêt. Cela inclut comment installer le logiciel pour convertir un serveur en contrôleur de domaine et comment manipuler l’environnement DNS afin que le contrôleur de domaine peut être fusionné en toute transparence dans l’environnement Active Directory. Un administrateur de données doit uniquement savoir comment gérer les données qu’ils sont responsables de telles que la création de nouveaux comptes d’utilisateur pour les nouveaux employés dans leur service.  
  
Déploiement des services AD DS nécessite la coordination et la communication entre les différents groupes impliqués dans l’opération de l’infrastructure réseau. Ces groupes doivent désigner des propriétaires de services et de données qui sont chargés de représentant les différents groupes au cours du processus de conception et de déploiement.  
  
Une fois le projet de déploiement est terminé, les propriétaires de ces services et de données continuent à être responsable de la partie de l’infrastructure géré par leur groupe. Dans un environnement Active Directory, ces propriétaires sont le propriétaire de la forêt, le serveur DNS pour le propriétaire du domaine Active Directory, le propriétaire de topologie de site et le propriétaire de l’unité d’organisation. Les rôles de ces services et données propriétaires sont expliquées dans les sections suivantes.  
  
#### <a name="forest-owner"></a>Propriétaire de la forêt  
Le propriétaire de la forêt est généralement un gestionnaire de technologies de l’informatique de l’organisation qui est responsable du processus de déploiement Active Directory et qui est finalement responsable de la gestion de prestation de services au sein de la forêt après que le déploiement est terminé. Le propriétaire de la forêt affecte des personnes pour remplir les autres rôles de possession en identifiant personnel clé au sein de l’organisation qui sont en mesure de fournir les informations nécessaires sur l’infrastructure de réseau et des besoins d’administration. Le propriétaire de la forêt est responsable pour les éléments suivants:  
  
-   Déploiement du domaine racine de forêt pour créer la forêt  
  
-   Déploiement du premier contrôleur de domaine dans chaque domaine pour créer les domaines requis pour la forêt  
  
-   Appartenances aux groupes d’administrateur de service dans tous les domaines de la forêt  
  
-   Création de la conception de la structure d’unité d’organisation pour chaque domaine dans la forêt  
  
-   Délégation de l’autorité administrative pour les propriétaires de l’unité d’organisation  
  
-   Modifications du schéma  
  
-   Modifications apportées aux paramètres de configuration de la forêt  
  
-   Mise en œuvre de certains paramètres de stratégie de stratégie de groupe, y compris les stratégies de compte utilisateur de domaine comme stratégie de verrouillage de compte et de mot de passe affinée  
  
-   Paramètres de stratégie d’entreprise qui s’appliquent aux contrôleurs de domaine  
  
-   Les paramètres de stratégie de groupe sont appliquées au niveau du domaine  
  
Le propriétaire de la forêt a autorité sur l’ensemble de la forêt. Il incombe du propriétaire de la forêt pour définir la stratégie de groupe et les stratégies d’entreprise et de sélectionner les personnes qui sont des administrateurs de service. Le propriétaire de la forêt est un service.  
  
#### <a name="dns-for-ad-ds-owner"></a>DNS pour le propriétaire du domaine Active Directory  
Le serveur DNS pour le propriétaire du domaine Active Directory est une personne qui a une bonne compréhension de l’infrastructure DNS existante et l’espace de noms existant de l’organisation.  
  
Le serveur DNS pour le propriétaire du domaine Active Directory est responsable pour les éléments suivants:  
  
-   Sert à une liaison entre l’équipe de conception et le groupe informatique actuellement propriétaire de l’infrastructure DNS  
  
-   Fournir les informations sur l’espace de noms DNS de l’entreprise pour faciliter la création de l’espace de noms Active Directory  
  
-   Utilisation de l’équipe de déploiement pour vous assurer que la nouvelle infrastructure DNS est déployée selon les spécifications de l’équipe de conception et qu’il fonctionne correctement  
  
-   La gestion du DNS pour une infrastructure AD DS, y compris le service serveur DNS et les données DNS  
  
Le serveur DNS pour le propriétaire du domaine Active Directory est propriétaire d’un service.  
  
#### <a name="site-topology-owner"></a>Propriétaire de topologie de site  
Le propriétaire de topologie de site est familiarisé avec la structure physique du réseau de l’organisation, y compris le mappage des zones du réseau qui sont connectés au moyen de liaisons lentes, les routeurs et sous-réseaux individuels. Le propriétaire de topologie de site est responsable pour les éléments suivants:  
  
-   Présentation de la topologie réseau physique et la façon dont elle affecte les services AD DS  
  
-   Comprendre comment le déploiement Active Directory aura un impact sur le réseau  
  
-   Déterminer les sites Active Directory logiques qui doivent être créés  
  
-   Mise à jour des objets de site pour les contrôleurs de domaine lors de l’ajout d’un sous-réseau, modifiées ou supprimées  
  
-   Création de liens de sites, ponts entre liens de site et les objets de connexion manuelle  
  
Le propriétaire de topologie de site est propriétaire d’un service.  
  
#### <a name="ou-owner"></a>Propriétaire de l’unité d’organisation  
Le propriétaire de l’unité d’organisation est responsable de la gestion des données stockées dans le répertoire. Cette personne doit être familier avec l’exploitation et des stratégies de sécurité qui sont en place sur le réseau. Propriétaires de l’unité d’organisation peuvent effectuer uniquement les tâches qui ont été délégués par les administrateurs de service, et ils peuvent effectuer les tâches sur les unités d’organisation à laquelle ils sont affectés. Les tâches qui peuvent être affectées au propriétaire de l’unité d’organisation sont les suivantes:  
  
-   Effectuer toutes les tâches de gestion de comptes au sein de leur unité d’organisation est attribuée  
  
-   La gestion des stations de travail et les serveurs membres qui sont membres de leur unité d’organisation est attribuée  
  
-   Délégation de l’autorité pour les administrateurs locaux au sein de leur unité d’organisation est attribuée  
  
Le propriétaire de l’unité d’organisation est un propriétaire des données.  
  
## <a name="BKMK_3"></a>Équipes de projet de création  
Les équipes de projet Active Directory sont des groupes temporaires sont responsables de l’exécution des tâches de conception et de déploiement Active Directory. Une fois le projet de déploiement Active Directory terminée, les propriétaires de responsabilité pour le répertoire et les équipes de projet peuvent disperser le.  
  
La taille des équipes projet varie en fonction de la taille de l’organisation. Dans les petites entreprises, une seule personne peut couvrir plusieurs domaines de responsabilité sur une équipe de projet et être impliqués dans plusieurs phases du déploiement. Les grandes entreprises peuvent nécessiter des grandes équipes avec différentes personnes ou équipes même différentes portant sur les différents domaines de responsabilité. La taille des équipes n’est pas importante tant que tous les domaines de responsabilité sont attribués et les objectifs de conception de l’organisation sont remplies.  
  
### <a name="identifying-potential-forest-owners"></a>Identification des propriétaires de forêt potentiels  
Identifiez les groupes au sein de votre organisation qui possèdent et contrôlent les ressources nécessaires pour fournir des services d’annuaire aux utilisateurs sur le réseau. Ces groupes sont considérés comme des propriétaires de forêt potentiels.  
  
La séparation de l’administration de services et de données dans les services AD DS rend possible pour l’infrastructure informatique groupe (ou groupes) d’une organisation pour gérer le service d’annuaire, tandis que les administrateurs locaux dans chaque groupe de gérer les données qui appartiennent à leurs propres groupes. Propriétaires de forêt potentiels ont l’autorité requise sur l’infrastructure réseau pour déployer et prendre en charge les services AD DS.  
  
Pour les organisations qui disposent d’une infrastructure centralisée groupe IT, le groupe informatique est généralement le propriétaire de la forêt et, par conséquent, le propriétaire de la forêt potentiels pour les déploiements futurs. Les organisations qui comprennent un certain nombre d’indépendant infrastructure informatique groupes ont un certain nombre de propriétaires de forêt potentiels. Si votre organisation possède déjà une infrastructure Active Directory en place, les propriétaires de forêt actuels sont également des propriétaires de forêt potentiels pour les nouveaux déploiements.  
  
Sélectionnez un des propriétaires de forêt potentiels à agir en tant que le propriétaire de la forêt pour chaque forêt que vous envisagez de déploiement. Les propriétaires de forêt potentiels sont chargés pour travailler avec l’équipe de conception pour déterminer si leur forêt est déployé ou si un autre plan d’action (par exemple, joindre une autre forêt existante) est une meilleure utilisation des ressources disponibles et toujours répond à leurs besoins. Le propriétaire de la forêt (ou propriétaires) dans votre organisation sont membres de l’équipe de conception d’Active Directory.  
  
### <a name="establishing-a-design-team"></a>Établissement d’une équipe de conception  
L’équipe de conception d’Active Directory est responsable de la collecte de toutes les informations nécessaires pour prendre des décisions concernant la conception de la structure logique Active Directory.  
  
Responsabilités de l’équipe de conception suivantes:  
  
-   Déterminer le nombre de domaines et forêts sont requis et quelles sont les relations entre les forêts et les domaines  
  
-   Fonctionne avec les propriétaires de données pour vous assurer que la conception répond à leurs besoins d’administration et sécurité  
  
-   Fonctionne avec les administrateurs réseau actuel pour vous assurer que l’infrastructure réseau actuelle prend en charge la conception et que la conception ne sera pas de nuire applications existantes déployées sur le réseau  
  
-   Utilisation des représentants du groupe de sécurité de l’organisation pour s’assurer que la conception est conforme aux stratégies de sécurité établie  
  
-   Conception de structures d’unité d’organisation qui autorisent les niveaux appropriés de protection et la délégation de l’autorité appropriée pour les propriétaires de données  
  
-   Travailler avec l’équipe de déploiement pour la conception de test dans un laboratoire environnement pour vous assurer qu’il fonctionne comme prévu et modification de la conception en tant que nécessaire pour résoudre les problèmes qui se produisent  
  
-   Création d’une conception de topologie de site qui répond à la configuration requise pour la réplication de la forêt tout en empêchant la surcharge de la bande passante disponible. Pour plus d’informations sur la conception de la topologie de site, voir [conception du Site topologie pour Windows Server 2008 AD DS](https://technet.microsoft.com/library/cc772013.aspx).  
  
-   Utilisation de l’équipe de déploiement pour vous assurer que la conception est correctement implémentée.  
  
L’équipe de conception inclut les membres suivants:  
  
-   Propriétaires de forêt potentiels  
  
-   Architecte du projet  
  
-   Chef de projet  
  
-   Personnes responsables de l’établissement et la maintenance des stratégies de sécurité sur le réseau  
  
Pendant le processus de conception de structure logique, l’équipe de conception identifie les autres propriétaires. Ces personnes doivent participer au programme dans le processus de conception dès qu’ils sont identifiés. Une fois le projet de déploiement est remis à l’équipe de déploiement, l’équipe de conception est chargé de surveiller le processus de déploiement pour vous assurer que la conception est correctement implémentée. L’équipe de conception modifie également la conception basée sur les commentaires des tests.  
  
### <a name="establishing-a-deployment-team"></a>Établissement d’une équipe de déploiement  
L’équipe de déploiement Active Directory est responsable pour les tests et l’implémentation de la conception de la structure logique Active Directory. Cela implique les tâches suivantes:  
  
-   Établissement d’un environnement de test qui émule suffisamment de l’environnement de production  
  
-   Test de la conception en implémentant la structure de forêt et domaine proposée dans un environnement de laboratoire pour vérifier qu’il répond aux objectifs de chaque propriétaire du rôle  
  
-   Développer et tester les scénarios de migration proposés par la conception dans un environnement de laboratoire  
  
-   S’assurer que chaque propriétaire approuve sur le processus de test pour vous assurer que les fonctionnalités de conception corrects sont en cours de test  
  
-   Test de l’opération de déploiement dans un environnement de pilot  
  
Lors de la conception et les tâches de test sont terminées, l’équipe de déploiement effectue les tâches suivantes:  
  
-   Crée les forêts et les domaines en fonction de la conception de la structure logique Active Directory  
  
-   Crée les sites et les objets de liens de site en fonction des besoins en fonction de la conception de topologie de site  
  
-   Permet de s’assurer que l’infrastructure DNS est configuré pour prendre en charge les services AD DS et que les nouveaux espaces de noms sont intégrés dans l’espace de noms existant de l’organisation  
  
L’équipe de déploiement Active Directory comprend les membres suivants:  
  
-   Propriétaire de la forêt  
  
-   DNS pour le propriétaire du domaine Active Directory  
  
-   Propriétaire de topologie de site  
  
-   Propriétaires de l’unité d’organisation  
  
L’équipe de déploiement fonctionne avec les administrateurs de service et des données pendant la phase de déploiement pour vous assurer que les membres de l’équipe des opérations sont familiarisés avec la nouvelle conception. Cela permet d’assurer une transition harmonieuse de possession lorsque l’opération de déploiement est terminée. À l’issue du processus de déploiement, la responsabilité de la gestion de l’environnement Active Directory passe à l’équipe des opérations.  
  
### <a name="documenting-the-design-and-deployment-teams"></a>Documenter les équipes de conception et de déploiement  
Les noms de documents et les informations de contact pour les personnes qui va participer à la conception et le déploiement des services AD DS. Identifier qui sera chargé pour chaque rôle sur les équipes de conception et de déploiement. Initialement, cette liste inclut les propriétaires de forêt potentiels, le chef de projet et l’architecte du projet. Lorsque vous déterminez le nombre de forêts que vous allez déployer, vous devrez peut-être créer de nouvelles équipes de conception pour d’autres forêts. Notez que vous devez mettre à jour votre documentation que les membres de l’équipe changent et que vous identifiez les propriétaires d’Active Directory différents pendant le processus de conception. Pour une feuille de calcul pour vous aider à documenter les équipes de conception et de déploiement pour chaque forêt, téléchargez Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip à partir de la tâche des identificateurs d’applet pour Windows Server2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)) et ouvrez "Conception et déploiement équipe informations" (DSSLOGI_1.doc).  
  


