---
ms.assetid: 50bd2566-e03c-4884-b5c4-895c8aab80aa
title: Identification des participants au projet de déploiement
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 962825494c613dfef808ce54dfba22727abf6878
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834650"
---
# <a name="identifying-the-deployment-project-participants"></a>Identification des participants au projet de déploiement

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La première étape de l’établissement d’un projet de déploiement pour les services de domaine Active Directory (AD DS) consiste à établir la conception et cycle d’équipes de projet de déploiement qui seront responsables de la gestion de la phase de conception et de la phase de déploiement du projet Active Directory. En outre, vous devez identifier les personnes et les groupes qui seront responsables de la possession et de maintenance du répertoire une fois le déploiement terminé.  
  
-   [Définition de rôles spécifiques au projet](#BKMK_1)  
  
-   [L’établissement des propriétaires et administrateurs](#BKMK_2)  
  
-   [Équipes de projet de création](#BKMK_3)  
  
## <a name="BKMK_1"></a>Définition de rôles spécifiques au projet  
Une étape importante à mettre en place les équipes de projet consiste à identifier les personnes qui détiennent les rôles spécifiques au projet. Celles-ci incluent le sponsor exécutif, l’architecte du projet et le responsable de projet. Ces personnes sont responsables de l’exécution du projet de déploiement Active Directory.  
  
Une fois que vous nommez qu’il l’architecte du projet et le chef de projet, ces personnes établissent des canaux de communication dans toute l’organisation, de générer des calendriers de projet et identifier les personnes qui seront membres des équipes de projet, en commençant par le propriétaires différents.  
  
### <a name="executive-sponsor"></a>Sponsor exécutif  
Déploiement d’une infrastructure tels que les services AD DS peut avoir un impact sur une organisation. Pour cette raison, il est important de disposer d’un sponsor exécutif qui comprend la valeur métier du déploiement, prend en charge le projet au niveau exécutif et peut aider à résoudre les conflits dans l’organisation.  
  
### <a name="project-architect"></a>Architecte du projet  
Chaque projet de déploiement Active Directory nécessite un architecte de projet gérer le processus de prise de décision Active Directory conception et de déploiement. L’architecte fournit une expertise technique afin de faciliter le processus de conception et déploiement d’AD DS.  
  
> [!NOTE]  
> Si aucun membre du personnel existant dans votre organisation n’avez conception directory expérience, vous souhaiterez engager un consultant externe qui est un expert de la conception d’Active Directory et le déploiement.  
  
Les responsabilités de l’architecte de projet Active Directory sont les suivantes :  
  
-   Propriétaire de la conception d’Active Directory  
  
-   Compréhension et l’enregistrement de la logique pour les décisions de conception clés  
  
-   S’assurer que la conception répond aux besoins de l’organisation  
  
-   L’établissement d’un consensus entre les équipes de conception, déploiement et opérations  
  
-   Comprendre les besoins des applications d’intégrés au service d’annuaire AD  
  
La conception d’Active Directory finale doit refléter une combinaison d’objectifs de l’entreprise et prendre des décisions techniques. Par conséquent, l’architecte du projet doit examiner les décisions de conception pour vous assurer que leur adéquation aux objectifs de l’entreprise.  
  
### <a name="project-manager"></a>Chef de projet  
Le responsable de projet facilite la coopération entre les unités commerciales et entre les groupes de gestion de la technologie. Dans l’idéal, le chef de projet de déploiement Active Directory est une personne à partir de l’organisation qui est familiarisée avec les deux les stratégies opérationnelles du groupe informatique et les exigences de conception pour les groupes qui sont préparent à déployer les services AD DS. Le chef de projet surveille le projet de déploiement entier commençant par conception et en continuant d’implémentation et permet de s’assurer que le projet reste sur la planification et le budget. Les responsabilités au chef de projet sont les suivantes :  
  
-   Fournissant le projet de base telles que la planification et la budgétisation de planification  
  
-   Progression de la commande sur le projet de conception et de déploiement Active Directory  
  
-   En s’assurant que les personnes appropriées sont impliquées dans chaque partie du processus de conception  
  
-   Sert de point de contact pour le projet de déploiement Active Directory unique  
  
-   Établissement de la communication entre les équipes de conception, déploiement et opérations  
  
-   L’établissement et la maintenance de la communication avec le commanditaire tout au long du projet de déploiement  
  
## <a name="BKMK_2"></a>L’établissement des propriétaires et administrateurs  
Dans un projet de déploiement Active Directory, les personnes qui sont propriétaires sont tenus responsables par management pour vous assurer que ce déploiement de tâches sont terminées cette conception d’Active Directory spécifications selon les besoins de l’organisation. Les propriétaires de ne pas nécessairement avoir accès à ou manipuler directement de l’infrastructure d’annuaire. Les administrateurs sont les personnes responsables pour exécuter les tâches de déploiement requis. Les administrateurs ont l’accès réseau et les autorisations nécessaires pour manipuler le répertoire et son infrastructure.  
  
Le rôle du propriétaire est stratégique et managériale. Il incombe aux propriétaires pour communiquer aux administrateurs les tâches requises pour l’implémentation de la conception d’Active Directory, comme la création de nouveaux contrôleurs de domaine au sein de la forêt. Les administrateurs sont responsables de l’implémentation de la conception du réseau en fonction des spécifications de conception.  
  
Dans les grandes organisations, les différents utilisateurs occupent des rôles d’administrateur ; Toutefois, dans certaines entreprises de petite taille, la même personne peut jouer le propriétaire et l’administrateur.  
  
### <a name="service-and-data-owners"></a>Propriétaires de services et données  
La gestion des services AD DS sur une base quotidienne impliquant deux types de propriétaires :  
  
-   Propriétaires de services qui sont responsables de la maintenance de planification et à long terme de l’infrastructure Active Directory et de s’assurer que le répertoire continue à fonctionner et que les objectifs établis dans les contrats de niveau de service sont conservés  
  
-   Propriétaires de données qui sont responsables de la maintenance des informations stockées dans le répertoire. Cela inclut l’utilisateur et de gestion de compte d’ordinateur et de gestion des ressources locales telles que les serveurs membres et les stations de travail.  
  
Il est important d’identifier rapidement les propriétaires de données et du service Active Directory afin qu’ils peuvent participer autant que possible le processus de conception. Étant donné que les propriétaires de services et de données sont responsables de la maintenance à long terme du répertoire une fois le projet de déploiement est terminé, il est important pour ces personnes à fournir des informations concernant les besoins de l’organisation et de se familiariser avec comment et pourquoi certaines décisions de conception. Les propriétaires de services incluent le propriétaire de la forêt, le propriétaire d’Active Directory d’affectation de noms DNS (Domain System) et le propriétaire de topologie de site. Les propriétaires de données incluent les propriétaires de l’unité d’organisation (UO).  
  
### <a name="service-and-data-administrators"></a>Administrateurs de service et de données  
L’opération d’AD DS implique deux types d’administrateurs : les administrateurs et les administrateurs de données de service. Les administrateurs de service implémentent les décisions de stratégie effectuées par les propriétaires de service et gérer les tâches quotidiennes associées au maintien de l’infrastructure et le service d’annuaire. Cela inclut la gestion des contrôleurs de domaine qui héberge le service d’annuaire, de la gestion des autres services réseau tels que DNS qui sont requis pour les services AD DS, contrôle de la configuration des paramètres de la forêt et s’assurer que le répertoire est toujours disponible.  
  
Les administrateurs de service sont également responsables de l’exécution des tâches de déploiement Active Directory en cours qui sont requises après que le processus de déploiement de Windows Server 2008 Active Directory initial est terminé. Par exemple, que les demandes sur l’augmentation de l’annuaire, les administrateurs de service créer des contrôleurs de domaine supplémentaires et établissent ou supprimer des approbations entre domaines, en fonction des besoins. Pour cette raison, l’équipe de déploiement Active Directory doit inclure les administrateurs de service.  
  
Vous devez veiller à affecter des rôles d’administrateur de service uniquement aux personnes de confiance dans l’organisation. Étant donné que ces personnes ont la possibilité de modifier les fichiers système sur les contrôleurs de domaine, ils peuvent modifier le comportement des services AD DS. Vous devez vous assurer que les administrateurs de service dans votre organisation sont les personnes qui connaissent les opérations et les stratégies de sécurité qui sont en place sur votre réseau et qui comprennent la nécessité d’appliquer ces stratégies.  
  
Les administrateurs de données sont des utilisateurs au sein d’un domaine qui sont chargés à la fois pour la gestion des données qui sont stockées dans AD DS, tels que les comptes d’utilisateur et de groupe et gérer les ordinateurs qui sont membres de leur domaine. Les administrateurs de données contrôlent les sous-ensembles d’objets dans le répertoire et ont aucun contrôle sur l’installation ou la configuration du service d’annuaire.  
  
Comptes d’administrateur de données ne sont pas fournis par défaut. Une fois que l’équipe de conception détermine comment les ressources qui doivent être gérés pour l’organisation, les propriétaires de domaine doivent créer des comptes d’administrateur de données et leur déléguer les autorisations appropriées selon le jeu d’objets pour lesquels les administrateurs doivent être responsable .  
  
Il est préférable de limiter le nombre d’administrateurs de service dans votre organisation pour le nombre minimal requis pour garantir que l’infrastructure continue à fonctionner. La majorité des tâches d’administration peut être effectuée par les administrateurs de données. Les administrateurs de service nécessitent une quantité plus large ensemble de compétences, car ils sont chargés de gérer le répertoire et l’infrastructure qui prend en charge. Les administrateurs de données nécessitent uniquement les compétences nécessaires pour gérer leur partie du répertoire. Division d’affectation de tâches de cette manière entraîne des économies de coût pour l’organisation, car seul un petit nombre d’administrateurs doit être formé à exploiter et maintenir son infrastructure et l’ensemble du répertoire.  
  
Par exemple, un administrateur de service a besoin comprendre comment ajouter un domaine à une forêt. Cela inclut l’installation du logiciel pour convertir un serveur en contrôleur de domaine et la manipulation de l’environnement DNS afin que le contrôleur de domaine peut être fusionné en toute transparence dans l’environnement Active Directory. Un administrateur de données doit uniquement savoir comment gérer les données spécifiques qui sont responsables, comme la création de nouveaux comptes d’utilisateur pour les nouveaux employés de leur département.  
  
Déploiement d’AD DS nécessite la coordination et la communication entre les différents groupes impliqués dans l’opération de l’infrastructure réseau. Ces groupes doivent désigner des propriétaires de services et de données qui sont responsables de représentant les différents groupes au cours du processus de conception et de déploiement.  
  
Une fois que le projet de déploiement est terminé, ces propriétaires de services et données continuent d’être responsable de la partie de l’infrastructure gérée par leur groupe. Dans un environnement Active Directory, ces propriétaires sont le propriétaire de la forêt, le DNS pour le propriétaire de AD DS, le propriétaire de topologie de site et le propriétaire de l’unité d’organisation. Les rôles de ces propriétaires de services et les données sont expliquées dans les sections suivantes.  
  
#### <a name="forest-owner"></a>Propriétaire de la forêt  
Le propriétaire de la forêt est généralement un responsable informatique (IT) senior dans l’organisation qui est responsable du processus de déploiement d’Active Directory et qui est finalement responsable du maintien de prestation de services au sein de la forêt après le déploiement est terminé. Le propriétaire de la forêt attribue des individus pour remplir les autres rôles de la propriété en identifiant les ressources principales au sein de l’organisation qui sont en mesure de contribuer les informations nécessaires sur l’infrastructure réseau et les besoins administratifs. Le propriétaire de la forêt est chargé pour les éléments suivants :  
  
-   Déploiement du domaine racine de forêt pour créer la forêt  
  
-   Déploiement du premier contrôleur de domaine dans chaque domaine pour créer les domaines requis pour la forêt  
  
-   Appartenances aux groupes d’administrateur de service dans tous les domaines de la forêt  
  
-   Création de la conception de la structure d’unité d’organisation pour chaque domaine dans la forêt  
  
-   Délégation de l’autorité administrative aux propriétaires de l’unité d’organisation  
  
-   Modifications du schéma  
  
-   Modifications apportées aux paramètres de configuration de la forêt  
  
-   Implémentation de certains paramètres de stratégie de stratégie de groupe, y compris les stratégies de compte utilisateur de domaine tels que stratégie de verrouillage de compte et mot de passe affinée  
  
-   Paramètres de stratégie d’entreprise qui s’appliquent aux contrôleurs de domaine  
  
-   Tous les autres paramètres de stratégie de groupe qui sont appliqués au niveau du domaine  
  
Le propriétaire de la forêt a autorité sur toute la forêt. Il incombe du propriétaire de la forêt pour définir la stratégie de groupe et les stratégies d’entreprise et de sélectionner les personnes qui sont des administrateurs de service. Le propriétaire de la forêt est un service.  
  
#### <a name="dns-for-ad-ds-owner"></a>DNS pour le propriétaire de AD DS  
Le DNS pour le propriétaire de AD DS est une personne qui a une connaissance approfondie de l’infrastructure DNS existante et l’espace de noms existant de l’organisation.  
  
Le DNS pour le propriétaire de AD DS est responsable de ce qui suit :  
  
-   Agissant comme une liaison entre l’équipe de conception et le groupe IT actuellement propriétaire de l’infrastructure DNS  
  
-   En fournissant les informations sur l’espace de noms DNS existant de l’organisation afin de faciliter la création d’un nouvel espace de noms Active Directory  
  
-   Utilisation de l’équipe de déploiement pour vous assurer que la nouvelle infrastructure DNS est déployée conformément aux spécifications de l’équipe de conception et qu’elle fonctionne correctement  
  
-   Gérez le serveur DNS pour l’infrastructure AD DS, y compris le service serveur DNS et les données DNS  
  
Le DNS pour le propriétaire de AD DS est propriétaire d’un service.  
  
#### <a name="site-topology-owner"></a>Propriétaire de topologie de site  
Le propriétaire de topologie de site est familiarisé avec la structure physique du réseau de l’organisation, notamment du mappage des sous-réseaux individuels, les routeurs et les zones du réseau qui sont connectés au moyen des liaisons lentes. Le propriétaire de topologie de site est chargé pour les éléments suivants :  
  
-   Présentation de la topologie du réseau physique et comment il affecte les services AD DS  
  
-   Comprendre comment le déploiement d’Active Directory aura un impact sur le réseau  
  
-   Déterminer les sites logiques Active Directory qui doivent être créés  
  
-   La mise à jour des objets de site pour les contrôleurs de domaine lorsqu’un sous-réseau est ajouté, modifié ou supprimé  
  
-   Création de liens de sites, ponts entre liens de site et les objets de connexion manuelle  
  
Le propriétaire de topologie de site est un propriétaire du service.  
  
#### <a name="ou-owner"></a>Propriétaire de l’unité d’organisation  
Le propriétaire de l’unité d’organisation est responsable de la gestion des données stockées dans le répertoire. Cette personne doit être familiarisé avec les opérations et les stratégies de sécurité qui sont en place sur le réseau. Propriétaires de l’unité d’organisation peuvent effectuer uniquement les tâches qui ont été déléguées par les administrateurs de service, et ils peuvent effectuer les tâches sur les unités d’organisation à laquelle ils sont attribués. Tâches susceptibles d’être attribués au propriétaire de l’unité d’organisation sont les suivantes :  
  
-   Effectuer toutes les tâches de gestion de compte dans leur UO attribué  
  
-   La gestion des postes de travail et les serveurs membres qui sont membres de leur unité d’organisation affectée  
  
-   Délégation de l’autorité aux administrateurs locaux au sein de leur unité d’organisation affectée  
  
Le propriétaire de l’unité d’organisation est un propriétaire de données.  
  
## <a name="BKMK_3"></a>Équipes de projet de création  
Les équipes de projet Active Directory sont des groupes temporaires qui sont responsables de l’exécution des tâches de conception et de déploiement Active Directory. Lorsque le projet de déploiement Active Directory est terminé, les propriétaires de responsabilité pour le répertoire et les équipes de projet peuvent disperser le.  
  
La taille des équipes de projet varie en fonction de la taille de l’organisation. Dans les petites entreprises, une seule personne peut couvrir plusieurs domaines de responsabilité d’une équipe de projet et être impliqué dans plusieurs phases du déploiement. Les grandes organisations peuvent nécessiter des équipes plus grandes avec des personnes différentes ou même différentes équipes qui couvrent les différentes zones de responsabilité. La taille des équipes n’est pas importante tant que tous les domaines de responsabilité sont affectés, et les objectifs de conception de l’organisation sont remplies.  
  
### <a name="identifying-potential-forest-owners"></a>Identification des propriétaires de forêt potentiels  
Identifier les groupes au sein de votre organisation que vous possèdent et contrôlent les ressources nécessaires pour fournir des services d’annuaire aux utilisateurs sur le réseau. Ces groupes sont considérés comme propriétaires de forêt potentiels.  
  
La séparation de l’administration de service et les données dans les services AD DS rend possible pour l’infrastructure informatique groupe (ou groupes) d’une organisation pour gérer le service d’annuaire, tandis que les administrateurs locaux de chaque groupe gèrent les données qui appartiennent à leurs propres groupes. Les propriétaires de forêt potentiels ont l’autorité requise sur l’infrastructure réseau pour déployer et prendre en charge les services AD DS.  
  
Pour les organisations qui ont une seule infrastructure centralisée groupe informatique, le groupe informatique est généralement le propriétaire de la forêt et, par conséquent, le propriétaire de la forêt potentiels pour tous les futurs déploiements. Les organisations qui incluent un certain nombre de d’infrastructure indépendant groupes ont un nombre de propriétaires de forêt potentiels. Si votre organisation possède déjà une infrastructure Active Directory en place, les propriétaires actuels de la forêt sont également des propriétaires de forêt potentiels pour les nouveaux déploiements.  
  
Sélectionnez un des propriétaires de forêt potentiels à agir en tant que le propriétaire de la forêt pour chaque forêt que vous envisagez de déploiement. Ces propriétaires de forêt potentiels sont responsables de travailler avec l’équipe de conception pour déterminer si leur forêt est déployé ou si une autre (par exemple, la jointure d’une autre forêt existante) est une meilleure utilisation des ressources disponibles et toujours répond à leurs besoins. Le propriétaire de la forêt (ou les propriétaires) dans votre organisation sont membres de l’équipe de conception d’Active Directory.  
  
### <a name="establishing-a-design-team"></a>L’établissement d’une équipe de conception  
L’équipe de conception d’Active Directory est responsable de la collecte de toutes les informations nécessaires pour prendre des décisions sur la conception de la structure logique Active Directory.  
  
Les responsabilités de l’équipe de conception sont les suivantes :  
  
-   Déterminer le nombre de forêts et domaines sont requis et quelles sont les relations entre les domaines et forêts  
  
-   Collaboration avec les propriétaires de données pour vous assurer que la conception répond à leurs besoins d’administration et de sécurité  
  
-   Travailler avec les administrateurs réseau actuel pour vous assurer que l’infrastructure réseau actuelle prend en charge la conception et que la conception n’affecte pas les applications existantes déployées sur le réseau  
  
-   Utilisation des représentants du groupe de sécurité de l’organisation pour vous assurer que la conception répond aux stratégies de sécurité établi  
  
-   Conception de structures d’unité d’organisation qui permettent à des niveaux appropriés de protection et la délégation de l’autorité appropriée pour les propriétaires des données  
  
-   Utilisation de l’équipe de déploiement pour tester la conception dans un laboratoire environnement pour vous assurer qu’il fonctionne comme prévu et modifier la conception en tant que nécessaire pour traiter tous les problèmes qui se produisent  
  
-   Création d’une conception de topologie de site qui répond aux besoins de réplication de la forêt tout en empêchant la surcharge de la bande passante disponible. Pour plus d’informations sur la conception de la topologie de site, consultez [conception du Site topologie pour Windows Server 2008 AD DS](https://technet.microsoft.com/library/cc772013.aspx).  
  
-   Utilisation de l’équipe de déploiement pour vous assurer que la conception est implémentée correctement  
  
L’équipe de conception inclut les membres suivants :  
  
-   Propriétaires de forêt potentiels  
  
-   Architecte du projet  
  
-   Chef de projet  
  
-   Personnes responsables de l’établissement et la gestion des stratégies de sécurité sur le réseau  
  
Pendant le processus de conception de structure logique, l’équipe de conception identifie les autres propriétaires. Ces personnes doivent participer au programme dans le processus de conception, dès qu’ils sont identifiés. Une fois le projet de déploiement est remis à l’équipe de déploiement, l’équipe de conception est chargé de superviser le processus de déploiement pour vous assurer que la conception est implémentée correctement. L’équipe de conception apporte également des modifications à la conception en fonction des commentaires à partir de test.  
  
### <a name="establishing-a-deployment-team"></a>L’établissement d’une équipe de déploiement  
Il incombe à l’équipe de déploiement Active Directory pour le test et l’implémentation de la conception de la structure logique Active Directory. Cela implique les tâches suivantes :  
  
-   L’établissement d’un environnement de test qui émule suffisamment de l’environnement de production  
  
-   Test de la conception en implémentant la structure proposée de forêt et le domaine dans un environnement de laboratoire pour vérifier qu’il répond aux objectifs de chaque propriétaire du rôle  
  
-   Développer et tester les scénarios de migration proposées par la conception dans un environnement lab  
  
-   S’assurer que chaque propriétaire déconnecte sur le processus de test pour vous assurer que les fonctionnalités de conception corrects sont en cours de test  
  
-   Test de l’opération de déploiement dans un environnement pilote  
  
Lors de la conception et les tâches de test sont terminées, l’équipe de déploiement effectue les tâches suivantes :  
  
-   Crée les forêts et les domaines en fonction de la conception de la structure logique Active Directory  
  
-   Crée les sites et les objets de lien de site si nécessaire en fonction de la conception de topologie de site  
  
-   Garantit que l’infrastructure DNS est configuré pour prendre en charge les services AD DS et que les nouveaux espaces de noms sont intégrés dans l’espace de noms existant de l’organisation  
  
L’équipe de déploiement Active Directory comprend les membres suivants :  
  
-   Propriétaire de la forêt  
  
-   DNS pour le propriétaire de AD DS  
  
-   Propriétaire de topologie de site  
  
-   Propriétaires de l’unité d’organisation  
  
L’équipe de déploiement fonctionne avec les administrateurs de service et des données pendant la phase de déploiement pour vous assurer que les membres de l’équipe des opérations sont familiers avec la nouvelle conception. Cela permet de garantir une transition en douceur de la propriété lorsque l’opération de déploiement est terminée. À l’issue du processus de déploiement, la responsabilité de la maintenance de l’environnement Active Directory nouvelle passe à l’équipe des opérations.  
  
### <a name="documenting-the-design-and-deployment-teams"></a>Documenter les équipes de conception et de déploiement  
Documenter les noms et les informations de contact pour les personnes qui participeront à la conception et le déploiement des services AD DS. Identifier qui sera chargé pour chaque rôle sur les équipes de conception et de déploiement. Initialement, cette liste inclut les propriétaires de forêt potentiels, le chef de projet et l’architecte du projet. Lorsque vous déterminez le nombre de forêts que vous allez déployer, vous devrez peut-être créer de nouvelles équipes de conception pour d’autres forêts. Notez que vous devez mettre à jour votre documentation comme membres d’équipe changent et que vous identifiez les différents propriétaires d’Active Directory pendant le processus de conception. Pour une feuille de calcul pour vous aider à documenter les équipes de conception et de déploiement pour chaque forêt, téléchargez Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip à partir de la tâche aides pour Windows Server 2003 Deployment Kit ([ https://go.microsoft.com/fwlink/?LinkID=102558 ](https://go.microsoft.com/fwlink/?LinkID=102558)) et ouvrez « Conception et déploiement Team informations » (DSSLOGI_1.doc).  
  


