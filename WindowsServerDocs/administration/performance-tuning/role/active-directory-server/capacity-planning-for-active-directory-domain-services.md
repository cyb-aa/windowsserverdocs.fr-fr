---
title: Planification de capacité pour les Services de domaine Active Directory
description: Étude détaillée des facteurs à prendre en compte pendant la planification de capacité pour les services AD DS.
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; kenbrunf
author: Teresa-Motiv
ms.date: 7/3/2019
ms.openlocfilehash: 5a9e2d39d4eedd1e8fdb4bfeaf267ad4cb4c596a
ms.sourcegitcommit: be243a92f09048ca80f85d71555ea6ee3751d712
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67799831"
---
# <a name="capacity-planning-for-active-directory-domain-services"></a>Planification de capacité pour les Services de domaine Active Directory

Cette rubrique est écrit à l’origine par Ken Brumfield, ingénieur de terrain principal chez Microsoft et fournit des recommandations pour la planification de capacité pour les Services de domaine Active Directory (AD DS).

## <a name="goals-of-capacity-planning"></a>Objectifs de la planification de capacité

Planification de la capacité n’est pas identique à la résolution des incidents liés aux performances. Ils sont étroitement liées, mais il est tout à fait différent. Les objectifs de la planification de capacité sont :  

- Implémenter et utiliser un environnement correctement 
- Réduire le temps passé à résolution des problèmes de performances.  
  
Dans la planification de la capacité, une entreprise peut avoir une cible de ligne de base de 40 % l’utilisation du processeur pendant les périodes de pointe afin de répondre aux exigences de performances client et de prendre en compte le temps nécessaire pour mettre à niveau le matériel du centre de données. En revanche, pour être averti des incidents liés aux performances anormales, un seuil d’alerte de surveillance peut être défini à 90 % sur un intervalle de 5 minutes.

La différence est que, quand un seuil de gestion de capacité est dépassé en permanence (un événement unique n’est pas un problème), ajout de capacité (qui est, l’ajout de processeurs plus rapides ou plus) serait une solution ou de mise à l’échelle de service sur plusieurs serveurs serait un solution. Seuils de performances indiquent que l’expérience client est actuellement affecté et immédiates étapes sont nécessaires pour résoudre le problème.

Par analogie : gestion de la capacité vise à empêcher un accident de voiture (défensive de conduite, s’assurer que les freins fonctionnent correctement, et ainsi de suite) alors que la résolution des problèmes de performances sont que la police, service incendie et urgence médicales professionnels font après un accident. Il s’agit sur « conduite défensive », Active Directory-style.

Dernières années, des conseils de planification de capacité pour les systèmes de monter en puissance ont considérablement changé. Les modifications suivantes dans les architectures système ont mis au défi d’hypothèses sur la conception et de mise à l’échelle un service :

- plateformes de serveur 64 bits  
- Virtualisation  
- Attention accrue sur la consommation d’énergie  
- Stockage SSD  
- Scénarios cloud  

En outre, l’approche est un décalage à partir d’une capacité basée sur serveur exercice un exercice de planification de capacité de basée sur le service de planification. Active Directory Domain Services (AD DS), un service distribué mature qui nombreux produits Microsoft et tiers utilisent comme serveur principal, devient un les produits les plus critiques pour planifier correctement garantir la capacité nécessaire pour les autres applications de s’exécuter.

### <a name="baseline-requirements-for-capacity-planning-guidance"></a>Spécifications de base pour des conseils de planification de capacité

Tout au long de cet article, les exigences de base suivants sont attendus :

- Lecteurs ai lu et connaissez [performances réglage des instructions pour Windows Server 2012 R2](https://docs.microsoft.com/previous-versions//dn529133(v=vs.85)).
- La plate-forme Windows Server est un x64 architecture basée sur un. Mais même si votre environnement Active Directory est installé sur Windows Server 2003 x86 (désormais au-delà de la fin du cycle de vie de prise en charge) et a une arborescence d’information d’annuaire (DIT) qui est moins 1,5 Go et qui peut facilement être conservées en mémoire, les instructions à partir de ce l’article sont conservés.
- Planification de la capacité est un processus continu, et vous devez vérifier régulièrement la manière dont l’environnement est répondant aux attentes.
- L’optimisation se produit sur plusieurs cycles de vie de matériel en fonction des coûts matériels. Par exemple, mémoire devient moins cher, réduit le coût par cœur, ou le prix du stockage de différentes options de modification.
- Plan pour la période d’activité de pointe de la journée. Il est recommandé d’examiner cela dans les intervalles de 30 minutes ou heures. Toute valeur supérieure peut masquer les pics réelles et toute valeur qu'inférieure peut être déformée par « pics temporaires ».
- Planifier la croissance au cours du cycle de vie de matériel pour l’entreprise. Cela peut inclure une stratégie de mise à niveau ou l’ajout de matériel de manière échelonnée, ou une actualisation complète de tous les trois à cinq ans. Chacune nécessite une « estimation » que la manière dont beaucoup la charge sur Active Directory augmentera. Données d’historique, si collectées, aidera avec cette évaluation. 
- Planifiez une tolérance de panne. Une fois une estimation *N* est dérivé, plan pour les scénarios incluant *N* &ndash; 1, *N* &ndash; 2, *N* &ndash; *x*.
  - Ajouter des serveurs supplémentaires en fonction des besoins d’organisation pour vous assurer que la perte d’un seul ou plusieurs serveurs ne dépasse pas les estimations de capacité maximale.
  - Envisagez également que la croissance et le plan d’erreur tolérance doivent être intégrés. Par exemple, si un contrôleur de domaine est requis pour prendre en charge de la charge, mais l’estimation est que la charge seront doublés dans l’année à venir et nécessitent deux contrôleurs de domaine totales, il y aura pas d’une capacité suffisante pour prendre en charge d’une tolérance de panne. La solution consisterait à démarrer avec trois contrôleurs de domaine. Un peut également prévoyons d’ajouter le contrôleur de domaine de troisième après 3 ou 6 mois si les budgets sont serrés.

    >[!NOTE]
    >Ajout d’Active Directory prenant en charge les applications peut avoir un impact perceptible sur la charge de contrôleur de domaine, si la charge provient les serveurs d’applications ou les clients.

### <a name="three-step-process-for-the-capacity-planning-cycle"></a>Processus en trois étapes pour la cycle de planification de capacité

Planification de la capacité, vous devez d’abord déterminer la qualité de service est nécessaire. Par exemple, un centre de données core prend en charge un niveau plus élevé d’accès concurrentiel et nécessite une certaine expérience plus cohérente pour les utilisateurs et de consommation des applications, ce qui nécessite une attention accrue pour la redondance et en réduisant les goulots d’étranglement système et infrastructure. En revanche, un emplacement de satellites avec une poignée d’utilisateurs ne doit pas le même niveau d’accès concurrentiel ou d’une tolérance de panne. Le bureau satellite peut donc pas accorder autant d’attention pour optimiser le matériel et l’infrastructure, ce qui peut conduire à la réduction des coûts sous-jacents. Toutes les recommandations et des instructions contenues dans cet article sont pour des performances optimales et peuvent être sélectivement souples pour les scénarios avec moins exigeants.

La question suivante est : virtualisé ou physique ? À partir d’un point de vue de planification de capacité, il n’existe pas de bonne ou de mauvaise réponse ; Il est un autre ensemble de variables à utiliser. Scénarios de virtualisation, plus bas à une des deux options :

- « Mappage direct » avec un invité par l’hôte (où la virtualisation existe uniquement pour abstraire le matériel physique à partir du serveur)
- « Hôte partagé »

Scénarios de test et de production indiquent que le scénario de « mappage direct » permettre être traité de manière identique à un hôte physique. « Partagés, hôte, » présente toutefois, tenez compte des points énoncés plus en détail ultérieurement. Le scénario « hôte partagé » signifie que les services AD DS est également en concurrence pour les ressources et les considérations paramétrage pour effectuer cette opération et de sanctions.

Compte tenu de ces considérations, cycle de planification de la capacité est un processus itératif de trois étapes :

1. Mesurer l’environnement existant, déterminer où les goulots d’étranglement du système sont actuellement et obtenir les principes fondamentaux de l’environnement nécessaires pour planifier la capacité nécessaire.
1. Déterminer le matériel nécessaire en fonction des critères décrites à l’étape 1.
1. Surveiller et valider que l’infrastructure, comme implémenté fonctionne selon les spécifications. Certaines données collectées dans cette étape devient la ligne de base pour le prochain cycle de planification de la capacité.

### <a name="applying-the-process"></a>Appliquer le processus

Pour optimiser les performances, vérifiez que ces composants principaux sont correctement sélectionnés et adaptées à l’application est chargée :

1. Mémoire
1. Réseau
1. Stockage
1. Processeur
1. Accès réseau

Les exigences de stockage de base des services AD DS et le comportement général du logiciel client bien écrit autorisent des environnements avec des utilisateurs maximum de 10 000 à 20 000 à renoncer à des investissements massifs dans capacity planning en ce qui concerne le matériel physique, comme presque n’importe quel serveur moderne système de la classe gère la charge. Ceci dit, le tableau suivant résume comment évaluer un environnement existant afin de sélectionner le matériel adapté. Chaque composant est analysé en détail dans les sections suivantes pour aider les administrateurs des services AD DS à évaluer leur infrastructure à l’aide des recommandations de base et les entités de sécurité spécifiques à l’environnement.

En général :

- N’importe quel dimensionnement en fonction des données en cours sera adaptée à l’environnement actuel.
- Pour les estimations, une valeur à la demande à croître au fil du cycle de vie du matériel.
- Déterminer s’il faut P20 dès aujourd'hui et croissance dans l’environnement plus large ou ajouter de la capacité sur le cycle de vie.
- Pour la virtualisation, les mêmes principaux et les méthodologies de planification de capacité s’appliquent, à ceci près que la surcharge de la virtualisation doit être ajouté à tout domaine lié.
- Planification, comme tout ce qui tente de prédire, de la capacité n’est pas une science exacte. Ne prévoyez pas d’éléments à calculer parfaitement et avec une précision de 100 %. Les conseils donnés ici sont la recommandation disposeront ; Ajoutez des capacités pour une sécurité supplémentaire et en permanence valider que l’environnement reste sur la cible.

### <a name="data-collection-summary-tables"></a>Tables de synthèse de collection de données

#### <a name="new-environment"></a>Nouvel environnement

| Composant | Estimations |
|-|-|
|Taille de stockage/base de données|40 Ko et 60 Ko pour chaque utilisateur|
|RAM|Taille de la base de données<br />Recommandations de base système d’exploitation<br />Les applications tierces|
|Réseau|1 Go|
|UC|utilisateurs simultanés de 1000 pour chaque cœur|

#### <a name="high-level-evaluation-criteria"></a>Critères d’évaluation de haut niveau

| Composant | Critères d’évaluation | Considérations de planification |
|-|-|-|
|Taille de stockage/base de données|La section intitulée « pour activer la journalisation de l’espace disque est libérée par la défragmentation » dans [limites de stockage](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10))| |
|Stockage / base de données de performances|<ul><li>« Disque logique ( *\<lecteur de base de données NTDS\>* ) \Avg disque s/lecture, » « disque logique ( *\<lecteur de base de données NTDS\>* ) \Avg disque s/écriture, « » Disque logique ( *\<lecteur de base de données NTDS\>* ) \Avg disque s/transfert »</li><li>« Disque logique ( *\<lecteur de base de données NTDS\>* ) \Reads/sec, » « disque logique ( *\<lecteur de base de données NTDS\>* ) \Writes/sec, » « disque logique (  *\<Lecteur de base de données NTDS\>* ) \Transfers/sec »</li></ul>|<ul><li>Stockage a deux problèmes à l’adresse<ul><li>Espace disponible, avec lequel la taille de pile en fonction d’aujourd'hui et les disques SSD en fonction de stockage n’est pas pertinente pour la plupart des environnements AD.</li> <li>Opérations d’entrée/sortie (e/s) disponibles – dans de nombreux environnements, il s’agit souvent négligé. Mais il est important d’évaluer uniquement les environnements où il n’est pas assez de RAM pour charger la base de données NTDS entière en mémoire.</li></ul><li>Le stockage peut être un sujet complexe et doit impliquer l’expertise de fournisseur de matériel pour le dimensionnement approprié. En particulier avec des scénarios plus complexes telles que les scénarios d’iSCSI SAN et NAS. Toutefois, en général, le coût par gigaoctet de stockage est souvent dans directe opposition au coût d’e/s :<ul><li>RAID 5 a un coût inférieur par gigaoctet que Raid 1, mais Raid 1 a un coût inférieur par e/s</li><li>Basée sur des piles de disques durs ont un coût inférieur par gigaoctet, mais les disques SSD ont un coût inférieur par e/s</li></ul><li>Après un redémarrage de l’ordinateur ou le service Active Directory Domain Services, le cache du moteur ESE (Extensible Storage Engine) est vide et les performances seront disque lié tandis que le cache augmente.</li><li>Dans la plupart des environnements AD est intensif d’e/s dans un modèle aléatoire vers des disques, annulant ainsi une grande partie de l’avantage de la mise en cache de lecture et lire les stratégies d’optimisation.  En outre, AD dispose d’un cache en mémoire supérieur de façon que la plupart des système de stockage met en cache.</li></ul>
|RAM|<ul><li>Taille de la base de données</li><li>Recommandations de base système d’exploitation</li><li>Les applications tierces</li></ul>|<ul><li>Le stockage est le composant le plus lent dans un ordinateur. Plus, qui peut être résident dans la mémoire RAM, moins il est nécessaire accéder au disque.</li><li>Garantir suffisamment de RAM est allouée pour stocker le système d’exploitation, les Agents (antivirus, de sauvegarde, surveillance), croissance au fil du temps et la base de données NTDS.</li><li>Pour les environnements où l’optimisation de la quantité de RAM n'est pas coût effectif (par exemple, des emplacements satellites) ou pas réalisable (DIT est trop grande), de référence de la section de stockage pour vous assurer que le stockage est correctement dimensionnée.</li></ul>|
|Réseau|<ul><li>« Interface réseau (\*) \Bytes reçus/s »</li><li>« Interface réseau (\*) \Bytes envoyés/s »|<ul><li>En règle générale, le trafic envoyé à partir d’un contrôleur de domaine présent dépasse le trafic envoyé vers un contrôleur de domaine.</li><li>Comme une connexion Ethernet commutée est en duplex intégral, le trafic réseau entrant et sortant doivent être dimensionnés de façon indépendante.</li><li>Consolider le nombre de contrôleurs de domaine, augmenteront la quantité de bande passante utilisée pour envoyer des réponses vers les demandes de client pour chaque contrôleur de domaine, mais seront suffisamment linéaire pour le site dans sa globalité.</li><li>Si la suppression de l’emplacement satellite contrôleurs de domaine, n’oubliez pas de les ajouter la bande passante pour le contrôleur de domaine satellite dans le hub de contrôleurs de domaine mais aussi d’utiliser pour évaluer la quantité de trafic WAN sera.</li></ul>|
|UC« Disque logique ( *\<lecteur de base de données NTDS\>* ) \Avg disque s/lecture »descripti« Process(lsass)\\' temps processeur »tors to consider during capacity planning for AD DS.
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; kenbrunf
author: Teresa-Motiv
ms.date: 7/3/2019
---

# Capacity planning for Active Directory Domain Services

This topic is originally written by Ken Brumfield, Senior Premier Field Engineer at Microsoft, and provides recommendations for capacity planning for Active Directory Domain Services (AD DS).

## Goals of capacity planning

Capacity planning is not the same as troubleshooting performance incidents. They are closely related, but quite different. The goals of capacity planning are:  

- Properly implement and operate an environment 
- Minimize the time spent troubleshooting performance issues.  
  
In capacity planning, an organization might have a baseline target of 40% processor utilization during peak periods in order to meet client performance requirements and accommodate the time necessary to upgrade the hardware in the datacenter. Whereas, to be notified of abnormal performance incidents, a monitoring alert threshold might be set at 90% over a 5 minute interval.

The difference is that when a capacity management threshold is continually exceeded (a one-time event is not a concern), adding capacity (that is, adding in more or faster processors) would be a solution or scaling the service across multiple servers would be a solution. Performance alert thresholds indicate that client experience is currently suffering and immediate steps are needed to address the issue.

As an analogy: capacity management is about preventing a car accident (defensive driving, making sure the brakes are working properly, and so on) whereas performance troubleshooting is what the police, fire department, and emergency medical professionals do after an accident. This is about “defensive driving,” Active Directory-style.

Over the last several years, capacity planning guidance for scale-up systems has changed dramatically. The following changes in system architectures have challenged fundamental assumptions about designing and scaling a service:

- 64-bit server platforms  
- Virtualization  
- Increased attention to power consumption  
- SSD storage  
- Cloud scenarios  

Additionally, the approach is shifting from a server-based capacity planning exercise to a service-based capacity planning exercise. Active Directory Domain Services (AD DS), a mature distributed service that many Microsoft and third-party products use as a backend, becomes one the most critical products to plan correctly to ensure the necessary capacity for other applications to run.

### Baseline requirements for capacity planning guidance

Throughout this article, the following baseline requirements are expected:

- Readers have read and are familiar with [Performance Tuning Guidelines for Windows Server 2012 R2](https://docs.microsoft.com/previous-versions//dn529133(v=vs.85)).
- The Windows Server platform is an x64 based architecture. But even if your Active Directory environment is installed on Windows Server 2003 x86 (now beyond the end of the support lifecycle) and has a directory information tree (DIT) that is less 1.5 GB in size and that can easily be held in memory, the guidelines from this article are still applicable.
- Capacity planning is a continuous process and you should regularly review how well the environment is meeting expectations.
- Optimization will occur over multiple hardware lifecycles as hardware costs change. For example, memory becomes cheaper, the cost per core decreases, or the price of different storage options change.
- Plan for the peak busy period of the day. It is recommended to look at this in either 30 minute or hour intervals. Anything greater may hide the actual peaks and anything less may be distorted by “transient spikes.”
- Plan for growth over the course of the hardware lifecycle for the enterprise. This may include a strategy of upgrading or adding hardware in a staggered fashion, or a complete refresh every three to five years. Each will require a “guess” as how much the load on Active Directory will grow. Historical data, if collected, will help with this assessment. 
- Plan for fault tolerance. Once an estimate *N* is derived, plan for scenarios that include *N* &ndash; 1, *N* &ndash; 2, *N* &ndash; *x*.
  - Add in additional servers according to organizational need to ensure that the loss of a single or multiple servers does not exceed maximum peak capacity estimates.
  - Also consider that the growth plan and fault tolerance plan need to be integrated. For example, if one DC is required to support the load, but the estimate is that the load will be doubled in the next year and require two DCs total, there will not be enough capacity to support fault tolerance. The solution would be to start with three DCs. One could also plan to add the third DC after 3 or 6 months if budgets are tight.

    >[!NOTE]
    >Adding Active Directory-aware applications might have a noticeable impact on the DC load, whether the load is coming from the application servers or clients.

### Three-step process for the capacity planning cycle

In capacity planning, first decide what quality of service is needed. For example, a core datacenter supports a higher level of concurrency and requires more consistent experience for users and consuming applications, which requires greater attention to redundancy and minimizing system and infrastructure bottlenecks. In contrast, a satellite location with a handful of users does not need the same level of concurrency or fault tolerance. Thus, the satellite office might not need as much attention to optimizing the underlying hardware and infrastructure, which may lead to cost savings. All recommendations and guidance herein are for optimal performance, and can be selectively relaxed for scenarios with less demanding requirements.

The next question is: virtualized or physical? From a capacity planning perspective, there is no right or wrong answer; there is only a different set of variables to work with. Virtualization scenarios come down to one of two options:

- “Direct mapping” with one guest per host (where virtualization exists solely to abstract the physical hardware from the server)
- “Shared host”

Testing and production scenarios indicate that the “direct mapping” scenario can be treated identically to a physical host. “Shared host,” however, introduces a number of considerations spelled out in more detail later. The “shared host” scenario means that AD DS is also competing for resources, and there are penalties and tuning considerations for doing so.

With these considerations in mind, the capacity planning cycle is an iterative three-step process:

1. Measure the existing environment, determine where the system bottlenecks currently are, and get environmental basics necessary to plan the amount of capacity needed.
1. Determine the hardware needed according to the criteria outlined in step 1.
1. Monitor and validate that the infrastructure as implemented is operating within specifications. Some data collected in this step becomes the baseline for the next cycle of capacity planning.

### Applying the process

To optimize performance, ensure these major components are correctly selected and tuned to the application loads:

1. Memory
1. Network
1. Storage
1. Processor
1. Net Logon

The basic storage requirements of AD DS and the general behavior of well written client software allow environments with as many as 10,000 to 20,000 users to forego heavy investment in capacity planning with regards to physical hardware, as almost any modern server class system will handle the load. That said, the following table summarizes how to evaluate an existing environment in order to select the right hardware. Each component is analyzed in detail in subsequent sections to help AD DS administrators evaluate their infrastructure using baseline recommendations and environment-specific principals.

In general:

- Any sizing based on current data will only be accurate for the current environment.
- For any estimates, expect demand to grow over the lifecycle of the hardware.
- Determine whether to oversize today and grow into the larger environment, or add capacity over the lifecycle.
- For virtualization, all the same capacity planning principals and methodologies apply, except that the overhead of the virtualization needs to be added to anything domain related.
- Capacity planning, like anything that attempts to predict, is NOT an accurate science. Do not expect things to calculate perfectly and with 100% accuracy. The guidance here is the leanest recommendation; add in capacity for additional safety and continuously validate that the environment remains on target.

### Data collection summary tables

#### New environment

| Component | Estimates |
|-|-|
|Storage/Database Size|40 KB to 60 KB for each user|
|RAM|Database Size<br />Base operating system recommendations<br />Third-party applications|
|Network|1 GB|
|CPU|1000 concurrent users for each core|

#### High-level evaluation criteria

| Component | Evaluation criteria | Planning considerations |
|-|-|-|
|Storage/Database size|The section entitled “To activate logging of disk space that is freed by defragmentation” in [Storage Limits](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10))| |
|Storage/ Database performance|<ul><li>"LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read," "LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Write," "LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Transfer"</li><li>"LogicalDisk(*\<NTDS Database Drive\>*)\Reads/sec," "LogicalDisk(*\<NTDS Database Drive\>*)\Writes/sec," "LogicalDisk(*\<NTDS Database Drive\>*)\Transfers/sec"</li></ul>|<ul><li>Storage has two concerns to address<ul><li>Space available, which with the size of today’s spindle based and SSD based storage is irrelevant for most AD environments.</li> <li>Input/Output (IO) Operations available – In many environments, this is often overlooked. But it is important to evaluate only environments where there is not enough RAM to load the entire NTDS Database into memory.</li></ul><li>Storage can be a complex topic and should involve hardware vendor expertise for proper sizing. Particularly with more complex scenarios such as SAN, NAS, and iSCSI scenarios. However, in general, cost per Gigabyte of storage is often in direct opposition to cost per IO:<ul><li>RAID 5 has lower cost per Gigabyte than Raid 1, but Raid 1 has lower cost per IO</li><li>Spindle-based hard drives have lower cost per Gigabyte, but SSDs have a lower cost per IO</li></ul><li>After a restart of the computer or the Active Directory Domain Services service, the Extensible Storage Engine (ESE) cache is empty and performance will be disk bound while the cache warms.</li><li>In most environments AD is read intensive I/O in a random pattern to disks, negating much of the benefit of caching and read optimization strategies.  Plus, AD has a way larger cache in memory than most storage system caches.</li></ul>
|RAM|<ul><li>Database size</li><li>Base operating system recommendations</li><li>Third-party applications</li></ul>|<ul><li>Storage is the slowest component in a computer. The more that can be resident in RAM, the less it is necessary to go to disk.</li><li>Ensure enough RAM is allocated to store the operating system, Agents (antivirus, backup, monitoring), NTDS Database and growth over time.</li><li>For environments where maximizing the amount of RAM is not cost effective (such as a satellite locations) or not feasible (DIT is too large), reference the Storage section to ensure that storage is properly sized.</li></ul>|
|Network|<ul><li>“Network Interface(\*)\Bytes Received/sec”</li><li>“Network Interface(\*)\Bytes Sent/sec”|<ul><li>In general, traffic sent from a DC far exceeds traffic sent to a DC.</li><li>As a switched Ethernet connection is full-duplex, inbound and outbound network traffic need to be sized independently.</li><li>Consolidating the number of DCs will increase the amount of bandwidth used to send responses back to client requests for each DC, but will be close enough to linear for the site as a whole.</li><li>If removing satellite location DCs, don’t forget to add the bandwidth for the satellite DC into the hub DCs as well as use that to evaluate how much WAN traffic there will be.</li></ul>|
|CPU|<ul><li>“Logical Disk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read”</li><li>“Process(lsass)\\% Processor Time”</li></ul>|<ul><li>Après élimination de stockage comme un goulot d’étranglement, répondre à la quantité de puissance de calcul nécessitée.</li><li>Bien que parfaitement linéaire, le nombre de cœurs de processeur consommés sur tous les serveurs dans une étendue spécifique (par exemple, un site) peut être permet de mesurer le nombre de processeurs est nécessaire pour prendre en charge de la charge totale client. Ajoutez au minimum nécessaire pour maintenir le niveau actuel de service sur tous les systèmes dans l’étendue.</li><li>Modifications apportées à la vitesse du processeur, y compris la gestion de l’alimentation liées change, les numéros d’impact dérivés de l’environnement actuel. En règle générale, il est impossible d’évaluer avec précision la comment passer d’un processeur 2,5 GHz à un processeur de 3 GHz réduira le nombre de processeurs nécessité.</li></ul>|
|Accès réseau (Netlogon)|<ul><li>« Netlogon (\*) \Semaphore acquiert »</li><li>« Netlogon (\*) \Semaphore des délais d’expiration »</li><li>« Netlogon (\*) \Average sémaphore contenir heure »</li></ul>|<ul><li>NET d’ouverture de session Secure Channel/MaxConcurrentAPI affecte uniquement les environnements avec les authentifications NTLM et/ou la Validation PAC. La Validation PAC est activée par défaut dans les versions de système d’exploitation antérieure à Windows Server 2008. Il s’agit d’un paramètre client, les contrôleurs de domaine ne sont donc concernées jusqu'à ce qu’elle est désactivée sur tous les systèmes clients.</li><li>Environnements avec une authentification de confiance entre significatives, qui inclut des approbations inter-forêts, ont le plus grand risque si elle n’est pas dimensionnée correctement.</li><li>Consolidation de serveur augmente la concurrence d’authentification d’approbation croisée.</li><li>Pics d’activité doivent être compensé, telles que des basculements de cluster, comme les utilisateurs s’authentifier de nouveau masse sur le nouveau nœud de cluster.</li><li>Les systèmes clients individuels (par exemple, un cluster) peut-être trop de paramétrage.</li></ul>|

## <a name="planning"></a>Planification

Pendant une longue période, la recommandation de la Communauté pour les services AD DS de dimensionnement a été à « put dans autant de RAM en tant que la taille de la base de données ». La plupart du temps, qu’il est recommandé que la plupart des environnements nécessaire se préoccuper. Mais l’écosystème de consommation des services AD DS est devenu beaucoup plus volumineuse, comme les environnements AD DS eux-mêmes, depuis son introduction en 1999. Bien que l’augmentation de puissance de calcul et le passage de x86 architectures à x64 architectures a apporté les aspects plus subtils de dimensionnement pour les performances sans importance pour un ensemble plus important de clients exécutant les services AD DS sur un matériel physique, la croissance de la virtualisation a réintroduit les problèmes de paramétrage à une audience plus large qu’avant.

Les conseils suivants sont donc sur la façon de déterminer et planifier les demandes d’Active Directory en tant que service, quel que soit qu’elle est déployée dans physique, un mélange de physique/virtuel ou un scénario purement virtualisé. Par conséquent, nous cesse de fonctionner l’évaluation à chacun des quatre composants principaux : le stockage, la mémoire, réseau et processeur. En bref, afin d’optimiser les performances sur les services AD DS, vise à rapprocher comme processeur liées en tant que possible.

## <a name="ram"></a>RAM

Simplement, plus qui peuvent être mis en cache dans la mémoire RAM, moins il est nécessaire accéder au disque. Pour optimiser l’évolutivité du serveur de la quantité minimale de RAM est la somme de la taille actuelle de la base de données, la taille totale de SYSVOL, le système d’exploitation recommandée quantité et les recommandations du fournisseur pour les agents (antiviruss, surveillance, sauvegarde et ainsi de suite ). Une quantité supplémentaire doit être ajoutée pour prendre en compte la croissance pendant la durée de vie du serveur. Il s’agit subjective de l’environnement selon les estimations de croissance de base de données en fonction des modifications de l’environnement.

Pour les environnements où l’optimisation de la quantité de RAM n'est pas coût effectif (par exemple, des emplacements satellites) ou pas réalisable (DIT est trop grande), de référence de la section de stockage pour vous assurer que le stockage est correctement conçue.

Dimensionnement corollaire qui s’affiche dans le contexte général dans la taille de mémoire du fichier de page. Dans le même contexte que tous les autres éléments relatifs à la mémoire, l’objectif est de réduire le sera à peu près plus lentement sur disque. Par conséquent la question doit atteindre, « comment le fichier d’échange dimensionner ? » à « La quantité de RAM est nécessaire pour réduire la pagination ? » La réponse à la question de ce dernier est décrite dans le reste de cette section. Cela laisse la plupart de la discussion pour redimensionner le fichier d’échange dans le domaine de recommandations de système d’exploitation général et la nécessité de configurer le système pour les vidages de mémoire, qui ne sont pas liées aux performances des services AD DS.

### <a name="evaluating"></a>L’évaluation

La quantité de RAM qui a besoin d’un contrôleur de domaine (DC) est en fait un exercice complexe pour ces raisons :

- Risque d’erreur lorsque vous tentez d’utiliser un système existant pour la quantité de RAM est nécessaire car LSASS supprime dans des conditions de sollicitation de la mémoire, DÉGONFLAGE artificiellement la nécessité de jauge.
- Le fait de subjectif qu’un contrôleur de domaine individuel doit uniquement mettre en cache ce qui est « intéressant » à ses clients. Cela signifie que les données devant être mis en cache sur un contrôleur de domaine dans un site avec uniquement un serveur Exchange seront très différentes des données qui doit être mis en cache sur un contrôleur de domaine qui authentifie uniquement les utilisateurs.
- Le travail à évaluer de RAM pour chaque contrôleur de domaine sur un cas par cas est excessif et change à mesure que les modifications de l’environnement.
- Les critères de la recommandation vous aide à prendre des décisions avisées : 
- Plus qui peuvent être mis en cache dans la mémoire RAM, moins il est nécessaire accéder au disque. 
- Le stockage est de loin le composant le plus lent d’un ordinateur. Accès aux données sur basée sur la pile et SSD, supports de stockage sont l’ordre de 1 000 000 fois plus lente que l’accès aux données dans la mémoire RAM.

Par conséquent, afin d’optimiser l’évolutivité du serveur, la quantité minimale de RAM est la somme de la taille actuelle de la base de données, la taille totale de SYSVOL, le système d’exploitation recommandée quantité et les recommandations du fournisseur pour les agents (antivirus, surveillance, sauvegarde, et ainsi de suite). Ajouter des montants supplémentaires pour prendre en compte la croissance pendant la durée de vie du serveur. Il s’agit subjective de l’environnement selon les estimations de croissance de la base de données. Toutefois, pour les emplacements satellites avec un petit ensemble d’utilisateurs finaux, ces exigences peuvent être moins stricte que ces sites ne doivent pas cache autant à la plupart des demandes de service.

Pour les environnements où l’optimisation de la quantité de RAM n'est pas coût effectif (par exemple, des emplacements satellites) ou pas réalisable (DIT est trop grande), de référence de la section de stockage pour vous assurer que le stockage est correctement dimensionnée.

> [!NOTE]
> Dimensionnement corollaire lors de la taille de mémoire du fichier de page. Étant donné que l’objectif est de réduire le sera à peu près plus lentement sur disque, la question est insérée à partir de « comment le fichier d’échange dimensionner ? » à « La quantité de RAM est nécessaire pour réduire la pagination ? » La réponse à la question de ce dernier est décrite dans le reste de cette section. Cela laisse la plupart de la discussion pour redimensionner le fichier d’échange dans le domaine de recommandations de système d’exploitation général et la nécessité de configurer le système pour les vidages de mémoire, qui ne sont pas liées aux performances des services AD DS.

### <a name="virtualization-considerations-for-ram"></a>Considérations relatives à la virtualisation de RAM

Évitez de validation excessive de mémoire au niveau de l’hôte. L’objectif fondamental derrière l’optimisation de la quantité de RAM consiste à réduire la quantité de temps passé à passer sur le disque. Dans les scénarios de virtualisation, le concept de validation excessive de mémoire existe où plus de mémoire vive est alloué pour les invités puis existe sur l’ordinateur physique. Cela elle-même n’est pas un problème. Il devient un problème lors de la mémoire totale utilisée activement par tous les invités dépasse la quantité de RAM sur l’ordinateur hôte et l’hôte sous-jacent démarre la pagination. Performances devient dépendant du disque dans les cas où le contrôleur de domaine va NTDS.dit pour obtenir des données, le contrôleur de domaine va le fichier d’échange pour obtenir des données ou l’hôte va sur le disque pour obtenir des données que l’invité considère comme dans la mémoire RAM.

### <a name="calculation-summary-example"></a>Exemple de résumé de calcul

|Composant|Estimation de la mémoire (par exemple)|
|-|-|
|Système d’exploitation base recommandé de RAM (Windows Server 2008)|2 Go|
|Tâches internes LSASS|200 MO|
|L’agent de surveillance|100 Mo|
|Antivirus|100 Mo|
|Base de données (catalogue Global)|8,5 Go êtes-vous ???|
|Tampon pour la sauvegarde à exécuter, les administrateurs de se connecter sans impact|1 Go|
|Total|12 Go|

**Recommandé : 16 GO**

Au fil du temps, l’hypothèse est possible que davantage de données sera ajouté à la base de données et le serveur sera probablement en production pendant 3 à 5 ans. Selon une estimation de croissance de 33 %, 16 Go serait une quantité raisonnable de mémoire vive à placer dans un serveur physique. Dans une machine virtuelle, étant donné la facilité avec laquelle les paramètres peuvent être modifiés et RAM peut être ajoutée à la machine virtuelle, à partir de 12 Go avec le plan pour surveiller et mettre à niveau à l’avenir est raisonnable.

## <a name="network"></a>Réseau

### <a name="evaluating"></a>L’évaluation
Cette section est inférieur sur l’évaluation des exigences concernant le trafic de réplication, ce qui se concentre sur le trafic qui traverse le réseau étendu et est entièrement couverte dans [trafic de réplication Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/bb742457(v=technet.10)), plutôt que sur l’évaluation de total la bande passante et la capacité réseau nécessaire, qui inclut les requêtes des clients, les applications de stratégie de groupe et ainsi de suite. Pour les environnements existants, cela peut être collectée à l’aide des compteurs de performances « Interface réseau (\*) \Bytes reçus/s, » et « Interface réseau (\*) \Bytes envoyés/s. » Intervalles d’échantillonnage des compteurs d’Interface réseau dans 15, 30 ou 60 minutes. Quoi que ce soit inférieure sera généralement trop volatile pour les bonnes mesures ; toute valeur supérieure sera lisser excessivement lit quotidienne.

> [!NOTE]
> En règle générale, la majorité du trafic réseau sur un contrôleur de domaine est sortante car le contrôleur de domaine répond aux requêtes des clients. Il s’agit de la raison pour le focus sur le trafic sortant, s’il est recommandé d’évaluer chaque environnement pour le trafic entrant également. Mêmes approches peuvent être utilisés pour résoudre et passez en revue les exigences de trafic réseau entrant. Pour plus d’informations, consultez l’article de la Base de connaissances [929851 : La plage de ports dynamiques par défaut pour le protocole TCP/IP a changé dans Windows Vista et Windows Server 2008](http://support.microsoft.com/kb/929851).

### <a name="bandwidth-needs"></a>Besoins en bande passante

La planification de l’évolutivité de réseau couvre deux catégories distinctes : la quantité de trafic et de l’UC de charge du trafic réseau. Chacun de ces scénarios est simple par rapport à certaines des autres rubriques dans cet article.

Dans l’évaluation de la quantité de trafic doit être pris en charge, il existe deux catégories uniques de planification de capacité pour les services AD DS en termes de trafic réseau. Le premier est le trafic de réplication qui traverse entre les contrôleurs de domaine et est entièrement couverte dans la référence [trafic de réplication Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/bb742457(v=technet.10)) et sont toujours applicables aux versions actuelles des services AD DS. Le second est le trafic de client à serveur intrasite. L’un des scénarios plus simples pour planifier, intrasite le trafic principalement reçoit des petites demandes des clients par rapport à grandes quantités de données envoyées aux clients. 100 Mo sera généralement suffisante dans les environnements jusqu'à 5 000 utilisateurs par serveur, dans un site. Prise en charge à l’aide d’une carte réseau de 1 Go et la mise à l’échelle côté réception (RSS) est recommandé pour les éléments ci-dessus 5 000 utilisateurs. Pour valider ce scénario, en particulier dans le cas des scénarios de consolidation de serveur, examinez d’Interface réseau (\*) \Bytes/sec sur tous les contrôleurs de domaine dans un site, les additionner et diviser par le nombre cible de contrôleurs de domaine pour vous assurer que est une capacité adéquate. Le plus simple pour ce faire consiste à utiliser la vue « Empilée » dans Windows moniteur de fiabilité et performances (anciennement appelé Perfmon), pour s’assurer que tous les compteurs sont mis à l’échelle le même.

Prenons l’exemple suivant (également appelé une très, très complexe permet de valider que la règle générale s’applique à un environnement spécifique). Les hypothèses suivantes sont effectuées :

- L’objectif est de réduire l’encombrement aux serveurs aussi peu que possible. Dans l’idéal, un seul serveur contiendra la charge et un serveur supplémentaire est déployé pour la redondance (*N* + 1 scénario). 
- Dans ce scénario, la carte réseau en cours prend en charge uniquement les 100 Mo et est dans un environnement commuté.  
  L’utilisation de la bande passante du réseau cible maximale est de 60 % dans un scénario de N (perte d’un contrôleur de domaine).
- Chaque serveur a environ 10 000 clients connectés à ce dernier.

Connaissances acquises à partir des données dans le graphique (Interface réseau (\*) \Bytes envoyés/s) :

1. Le jour ouvré démarre précieuse environ 5 h 30 et les crues vers le bas à 7 h 00.
1. La période de pointe le plus occupée est de 8 h 00 à 8 h 15, avec plus de 25 octets envoyés/s sur le contrôleur de domaine le plus occupé.  
   > [!NOTE]
   > Toutes les données de performances sont historiques. Par conséquent, le point de données de pointe à 8 h 15 indique la charge de 8:00 à 8:15.
1. Il existe des pics avant 4 h 00, avec plus de 20 octets envoyés/s sur le contrôleur de domaine le plus occupé, ce qui peut indiquer un chargement à partir de différents fuseaux horaires ou activité d’infrastructure, telles que les sauvegardes en arrière-plan. Étant donné que le pic à 8 h 00 dépasse cette activité, il n’est pas pertinent.
1. Il existe cinq contrôleurs de domaine dans le site.
1. La charge maximale est d’environ 5,5 Mo/s par le contrôleur de domaine, qui représente de 44 % de la connexion de 100 Mo. À l’aide de ces données, il peut être estimé que la bande passante totale nécessaire entre 8 h 00 et 8:15 AM est 28 Mo/s.
   >[!NOTE]
   >Soyez prudent avec le fait que les compteurs Interface réseau envoyés/reçus sont exprimées en octets et la bande passante réseau est mesurée en bits. 100 MO &divide; 8 = 12,5 MO, 1 GO &divide; 8 = 128 MO.
  
Conclusions :

1. Cet environnement actuel ne répond pas au N + 1 niveau de tolérance de panne à 60 % d’utilisation cible. Mettre un système hors connexion déplace la bande passante par le serveur à partir d’environ 5,5 Mo/s (44 %) environ 7 Mo/s (56 %).
1. L’objectif déclaré précédemment de la consolidation à un seul serveur, cela dépasse l’utilisation de la cible maximale et en théorie l’utilisation possible d’une connexion de 100 Mo.
1. Avec une connexion de 1 Go, ceci représentera 22 % de la capacité totale.
1. Sous normal d’exploitation conditions dans le *N* + 1 scénario, charge client sera relativement répartie à environ 14 Mo/s par serveur ou 11 % de la capacité totale.
1. Pour vous assurer que la capacité est suffisante lors indisponibilité d’un contrôleur de domaine, les cibles de fonctionnement normales par serveur serait environ 30 % réseau utilisation ou 38 Mo/s par serveur. Cibles de basculement est de 60 % d’utilisation réseau soit 72 Mo/s par serveur.  
  
En bref, le déploiement de systèmes final doit avoir une carte réseau de 1 Go et être connecté à une infrastructure réseau qui prendra en charge ladite charge. Une note supplémentaire est qu’étant donné la quantité de trafic réseau généré, la charge du processeur à partir des communications réseau peut avoir un impact significatif et limiter l’extensibilité maximale des services AD DS. Ce même processus peut être utilisé pour estimer la quantité de communications entrantes pour le contrôleur de domaine. Mais étant donné la prédominance de trafic sortant par rapport à trafic entrant, il est un exercice d’apprentissage pour la plupart des environnements. En s’assurant de la prise en charge matérielle pour RSS est important dans les environnements avec plus de 5 000 utilisateurs par serveur. Pour les scénarios avec un trafic réseau élevé, l’équilibrage de charge d’interruption peut être un goulot d’étranglement. Cela peut être détectée par le processeur (\*)\% du temps d’interruption qui est distribué inégalement entre les unités centrales. RSS activé des cartes d’interface réseau peut atténuer cette limitation et augmenter l’évolutivité.

> [!NOTE]
> Une approche similaire peut être utilisée pour estimer la capacité supplémentaire nécessaire lors de la consolidation des centres de données, ou en retirant un contrôleur de domaine dans un emplacement satellite. Simplement collecter le trafic entrant et sortant aux clients et qui sera la quantité de trafic qui sera affiché sur les liaisons réseau étendu.  
>  
> Dans certains cas, vous pouvez rencontrer davantage de trafic que prévu, car le trafic est plus lent, telles que lorsque la vérification de certificats ne respecte pas les délais d’expiration agressives sur le réseau étendu. Pour cette raison, l’utilisation et dimensionnement WAN doivent être un processus continu, itératif.

### <a name="virtualization-considerations-for-network-bandwidth"></a>Considérations relatives à la virtualisation pour la bande passante réseau

Il est facile de faire des recommandations pour un serveur physique : 1 Go pour les serveurs prenant en charge supérieure à 5 000 utilisateurs. Une fois que plusieurs invités partage une infrastructure sous-jacente de commutateur virtuel, une attention supplémentaire est nécessaire pour garantir que l’hôte a de la bande passante réseau suffisante pour prendre en charge tous les invités sur le système et nécessite donc la rigueur supplémentaire. Cela n’est rien de plus qu’une extension de s’assurer de l’infrastructure réseau dans l’ordinateur hôte. Il s’agit peu importe que le réseau qui inclut le contrôleur de domaine en cours d’exécution en tant qu’ordinateur virtuel invité sur un ordinateur hôte avec le trafic réseau via un commutateur virtuel ou si directement connecté à un commutateur physique. Le commutateur virtuel est simplement un composant plus où la liaison montante doit prendre en charge de la quantité de données transmises. Par conséquent, la carte réseau physique hôte physique liée au commutateur doit être en mesure de prendre en charge de la charge de contrôleur de domaine ainsi que tous les autres invités qui partage le commutateur virtuel connecté à la carte réseau physique.

### <a name="calculation-summary-example"></a>Exemple de résumé de calcul

|System|Bande passante maximale|
|-|-|
CONTRÔLEUR DE DOMAINE 1|6.5 Mo/s|
DC 2|6,25 Mo/s|
|CONTRÔLEUR DE DOMAINE 3|6,25 Mo/s|
|CONTRÔLEUR DE DOMAINE 4|5,75 Mo/s|
|CONTRÔLEUR DE DOMAINE 5|4,75 Mo/s|
|Total|28,5 Mo/s|

**Recommandé : Mo/s 72** (28.5 Mo/s divisé par 40 %)

|Nombre de systèmes cible|Bande passante totale (ci-dessus)|
|-|-|
|2|28,5 Mo/s|
|Ce qui entraîne un comportement normal|28,5 &divide; 2 = 14.25 des Mo/s|

Comme toujours, au fil du temps l’hypothèse est possible que la charge client augmente et cette croissance devrait être planifiée en tant que meilleure que possible. La quantité recommandée pour planifier permettrait une croissance estimé dans le trafic réseau de 50 %.

## <a name="storage"></a>Stockage

Planification de stockage constitue deux composants :

- Capacité, ou la taille de stockage
- Performances

Une large part de temps et de la documentation est consacrée à la capacité de planification, laissant les performances souvent complètement négligé. Avec les coûts actuels de matériel, la plupart des environnements ne sont pas volumineux suffisamment qu’une de ces est en fait un critère important et la recommandation pour « put dans autant de RAM en tant que la taille de la base de données » couvre généralement le reste, bien qu’il soit excessif pour les emplacements satellites dans supérieure environnements.

### <a name="sizing"></a>Dimensionnement

#### <a name="evaluating-for-storage"></a>L’évaluation pour le stockage

Par rapport à 13 ans lorsque Active Directory a été introduite, un temps lorsque les lecteurs de 4 Go et de 9 Go ont été les tailles de lecteur courants, dimensionnement d’Active Directory n’est pas même un facteur important pour tous, mais plus grands environnements. Avec le disque dur disponible le plus petit tailles dans la plage de 180 Go, l’ensemble du système d’exploitation, SYSVOL et NTDS.dit peuvent facilement tenir sur un seul lecteur. Par conséquent, il est recommandé de déconseiller investissement lourd dans cette zone.

La seule recommandation à prendre en compte est de garantir que 110 % de la taille de NTDS.dit est disponible pour permettre la défragmentation. En outre, les ajustements de croissance pendant la durée de vie du matériel doivent être effectuées.

L’examen de la première et la plus importante est l’évaluation de la taille NTDS.dit et SYSVOL sera. Ces mesures seront traduit en un disque de taille fixée et allocation de mémoire vive. En raison de () coût relativement faible de ces composants, les calculs n’a pas besoin être rigoureux et précis. Contenu sur l’évaluation cela pour les environnements existants et nouveaux se trouve dans le [stockage de données](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961771(v=technet.10)) série d’articles. En particulier, reportez-vous aux articles suivants :

- **Pour les environnements existants &ndash;**  la section intitulée « pour activer la journalisation de l’espace disque est libérée par la défragmentation » dans l’article [limites de stockage](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10)).
- **Pour les nouveaux environnements &ndash;**  l’article intitulé [des estimations de croissance pour les utilisateurs Active Directory et les unités d’organisation](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc961779(v=technet.10)).

  > [!NOTE]
  > Les articles sont basées sur des estimations de taille de données effectuées au moment de la version d’Active Directory dans Windows 2000. Utiliser des tailles d’objet qui reflètent la taille réelle des objets dans votre environnement.

Lorsque vous parcourez les environnements existants avec plusieurs domaines, il existe peut-être des variations dans les tailles de base de données. Lorsque cela est vrai, utilisez le plus petit catalogue global (GC) et les tailles de non-GC.  

La taille de la base de données peut varier entre les versions de système d’exploitation. Contrôleurs de domaine qu’exécuter des systèmes d’exploitation antérieurs tels que Windows Server 2003 a une taille inférieure de la base de données à un contrôleur de domaine qui exécute un système d’exploitation ultérieur, comme Windows Server 2008 R2, en particulier lors de la Corbeille de ce type Active Directory de fonctionnalités Bin ou informations d’identification itinérantes sont activées.

> [!NOTE]  
  >
>- Pour les nouveaux environnements, notez que les estimations de croissance des estimations pour les utilisateurs Active Directory et les unités d’organisation indiquent que 100 000 utilisateurs (dans le même domaine) consomment environ 450 Mo d’espace. Veuillez noter que les attributs remplis peuvent avoir un impact important sur la quantité totale. Attributs sont renseignés sur de nombreux objets par des tiers et les produits Microsoft, y compris Microsoft Exchange Server et Lync. Une évaluation basée sur le portefeuille de produits dans l’environnement est recommandée, mais l’exercice consistant en détaillant les calculs et de test pour des estimations précises pour tous, mais plus grands environnements est peut-être pas intéressant de beaucoup de temps et d’efforts.
>- Assurez-vous que 110 % de la taille n’est disponible en tant qu’espace libre afin d’activer en mode hors connexion de NTDS.dit defrag et planifier la croissance sur une durée de vie du matériel trois à cinq ans. Faible coût de stockage est, estimation de stockage à la taille de la DIT que l’allocation de stockage est sécurisée prendre en compte la croissance et le besoin potentiel de hors connexion de défragmentation de 300 %.

#### <a name="virtualization-considerations-for-storage"></a>Considérations relatives à la virtualisation pour le stockage

Dans un scénario où plusieurs fichiers de disque dur virtuel (VHD) sont allouées sur un volume unique utilisation fixe de taille disque d’au moins 210 % (100 % de la DIT + 110 % d’espace libre) la taille de la DIT pour vous assurer qu’il existe suffisamment d’espace réservé.  

#### <a name="calculation-summary-example"></a>Exemple de résumé de calcul

|Données collectées à partir de la phase d’évaluation| |
|-|-|
|Taille de NTDS.dit|35 GO|
|La défragmentation de modificateur permet pour hors connexion|2.1|
|Stockage total nécessaire|73,5 GO|

> [!NOTE]
> Ce stockage nécessité s’ajoute le stockage nécessaire pour SYSVOL, système d’exploitation, fichier d’échange, les fichiers temporaires, les données mises en cache locales (par exemple, les fichiers du programme d’installation) et applications.

### <a name="storage-performance"></a>Performances de stockage

#### <a name="evaluating-performance-of-storage"></a>L’évaluation des performances de stockage

En tant que le composant le plus lent dans n’importe quel ordinateur, le stockage peut avoir le plus gros impact négatif sur l’expérience client. Pour ces environnements de grande taille suffisamment pour lequel les recommandations de dimensionnement de RAM ne sont pas envisageables, les conséquences de négliger la planification de stockage pour les performances peuvent être catastrophiques.  En outre, la complexité et les types de technologie de stockage supplémentaire augmentent les risques de défaillance comme la pertinence de longue date avec nous les meilleures pratiques de « put de système d’exploitation, les journaux et les base de données » sur des disques physiques distincts est limitée dans ses scénarios utiles.  Il s’agit comme la meilleure pratique de longue date est basée sur l’hypothèse est que « disque » est une pile dédiée et cela autorisé d’e/s soit isolé.  Cette hypothèses qui rendent cela true ne sont plus appropriés avec l’introduction de :

- Nouveaux types de stockage et les scénarios de stockage virtualisé et partagées
- Partagé des piles de disques sur un réseau SAN (Storage Area)
- Fichier de disque dur virtuel sur un réseau SAN ou un stockage connecté au réseau
- Lecteurs SSD
- Architectures de stockage à plusieurs niveaux (par exemple, les niveaux de stockage SSD sont stockage plus important de pile en fonction de la mise en cache)

En bref, l’objectif final de tous les efforts de performances de stockage, quelle que soit l’architecture de stockage sous-jacent et la conception, consiste à garantir la quantité nécessaire d’opérations d’e/s par seconde (IOPS) sont disponibles et que les e/s se produisent pendant un laps de temps acceptable . Cette section explique comment évaluer les demandes de services AD DS du stockage sous-jacent afin de garantir des solutions de stockage sont correctement conçues.  Étant donné la variabilité des technologies de stockage d’aujourd'hui, il est préférable de travailler avec les fournisseurs de stockage pour garantir des e/s suffisante.  Pour les scénarios avec stockage attaché localement, faire référence à l’annexe C pour les principes de base sur la façon de concevoir des scénarios de stockage local traditionnel.  Cette principaux est généralement applicables aux niveaux de stockage plus complexes et vous aidera également à la boîte de dialogue avec les fournisseurs de prise en charge des solutions de stockage principal.

- Étant donné la large gamme d’options de stockage disponibles, il est recommandé de s’engager l’expertise des équipes de support matériel ou des fournisseurs pour vous assurer que la solution répond aux besoins des services AD DS. Les numéros suivants sont des informations qui seraient fournies pour les spécialistes en stockage.

Pour les environnements où la base de données est trop volumineux pour être chargé dans la mémoire RAM, utilisez les compteurs de performances pour déterminer quelle mesure les e/s doit être pris en charge :

- Disque logique (\*) \Avg disque s/lecture (par exemple, si NTDS.dit est stocké sur le lecteur D: / lecteur, le chemin complet serait \Avg LogicalDisk (d :)) disque s/lecture)
- Disque logique (\*) \Avg disque s/écriture
- Disque logique (\*) \Avg disque s/transfert
- LogicalDisk(\*)\Reads/sec
- LogicalDisk(\*)\Writes/sec
- LogicalDisk(\*)\Transfers/sec

Il doivent être échantillonnés à 30/15/60 minutes d’intervalle pour évaluer les demandes de l’environnement actuel.

#### <a name="evaluating-the-results"></a>Évaluation des résultats

> [!NOTE]
> Le focus est sur les lectures à partir de la base de données, car il s’agit généralement du composant les plus exigeantes, de la même logique peut être appliquée pour les écritures dans le fichier journal en remplaçant le disque logique ( *\<NTDS journal\>* ) \Avg disque s/écriture et le disque logique ( *\<NTDS journal\>* ) \Writes/sec) :
>  
> - Disque logique ( *\<NTDS\>* ) \Avg disque s/lecture indique si le stockage en cours est la taille adéquate.  Si les résultats sont à peu près égales à l’heure d’accès de disque pour le type de disque, le disque logique ( *\<NTDS\>* ) \Reads/sec est une mesure valide.  Vérifier les spécifications du fabricant pour le stockage sur le serveur principal, mais bon plages pour le disque logique ( *\<NTDS\>* ) \Avg disque s/lecture est à peu près :
>   - 7200 – 9 à 12,5 millisecondes (ms)
>   - 10 000 – 6 à 10 ms
>   - 15 000 – 4 à 6 ms  
>   - SSD – 1 à 3 ms  
>   - >[!NOTE]
>     >Recommandations existent indiquant que les performances de stockage sont détérioré à 15 MS à 20 ms (selon la source).  La différence entre les valeurs ci-dessus et les autres instructions est que les valeurs ci-dessus sont la plage de fonctionnement normale.  Les autres recommandations dépannez des conseils pour identifier lors de l’expérience du client se dégrade considérablement et devenir visible.  Référence annexe C pour une explication plus approfondie.
> - Disque logique ( *\<NTDS\>* ) \Reads/sec est la quantité d’e/s qui est en cours d’exécution.
>   - Si disque logique ( *\<NTDS\>* ) \Avg disque s/lecture est dans la plage optimale pour le stockage principal, disque logique ( *\<NTDS\>* ) \Reads/ s peut être utilisé directement pour dimensionner le stockage.
>   - Si disque logique ( *\<NTDS\>* ) \Avg disque s/lecture n’est pas dans la plage optimale pour le stockage principal, les e/s supplémentaire est nécessaire en fonction de la formule suivante :
>     > (Disque logique ( *\<NTDS\>* ) \Avg disque s/lecture) &divide; (temps d’accès de disque de supports physiques) &times; (disque logique ( *\<NTDS\>* ) \Avg disque s/lecture)

Éléments à prendre en considération :

- Notez que si le serveur est configuré avec une quantité optimale de RAM, ces valeurs seront erronés lors de la planification.  Ils seront à tort du côté supérieur et peuvent toujours être utilisées comme pire des cas.
- Ajout/optimisation RAM détermineront spécifiquement une diminution de la lecture d’e/s (disque logique ( *\<NTDS\>* ) \Reads/Sec.  Cela signifie que la solution de stockage n’avez pas à être aussi robuste qu’initialement calculée.  Malheureusement, quoi que ce soit plus spécifique à cette déclaration générale est l’environnement dépende de la charge du client et aides générales ne peut pas être fournie.  La meilleure option consiste à ajuster le dimensionnement de stockage après l’optimisation de mémoire RAM.

#### <a name="virtualization-considerations-for-performance"></a>Considérations relatives à la virtualisation pour des performances

Comme pour toutes les discussions précédentes de la virtualisation, la clé ici est pour vous assurer que l’infrastructure partagée sous-jacent peut prendre en charge la charge de contrôleur de domaine, ainsi que les autres ressources à l’aide de sous-jacent partagé media et toutes les voies à celui-ci. Cela est vrai si un contrôleur de domaine physique partage le même support sous-jacent sur un réseau SAN, NAS ou iSCSI infrastructure en tant que d’autres serveurs ou applications, qu’il s’agisse d’un invité à l’aide de passe via l’accès à une infrastructure de réseau SAN, NAS ou iSCSI qui partage le sous-jacent de média, ou si l’invité utilise un fichier de disque dur virtuel qui réside sur les médias partagés localement ou une infrastructure de réseau SAN, NAS ou iSCSI. L’exercice de planification est une affaire de s’assurer que le média sous-jacent peut prendre en charge la charge totale de tous les consommateurs.

En outre, du point de vue de l’invité, comme il existe des chemins de code supplémentaires qui doivent être parcourus, il existe un impact sur les performances de devoir passer par un ordinateur hôte pour accéder à n’importe quel stockage. Sans surprise, test de performances de stockage indique que la virtualisation a un impact sur le débit est subjectif à l’utilisation du processeur du système hôte (voir l’annexe a : Critère de dimensionnement de processeur), qui est évidemment influencé par les ressources de l’hôte demandées par l’invité. Cela contribue aux considérations concernant les besoins de traitement dans un scénario virtualisé virtualisation (consultez [considérations relatives à la virtualisation pour le traitement](#virtualization-considerations-for-processing)).

Ce qui en fait plus complexe : il existe une variété de différentes options de stockage qui sont disponibles que tous ont des impacts sur les performances différentes. Une estimation sans échec lors de la migration de physique vers virtuel, utiliser un multiplicateur 1,10 pour ajuster les différentes options de stockage pour les invités virtualisées sur Hyper-V, tels que le stockage direct, une carte SCSI ou IDE. Les ajustements qui doivent être apportées lors du transfert entre les scénarios de stockage différents sont sans intérêt si le stockage est local, SAN, NAS, ou iSCSI.

#### <a name="calculation-summary-example"></a>Exemple de résumé de calcul

Détermination de la quantité d’e/s nécessaires pour un système sain dans des conditions normales de fonctionnement :

- Disque logique ( *\<lecteur de base de données NTDS\>* ) \Transfers/sec durant la période de pointe période minute 15 
- Pour déterminer la quantité d’e/s nécessaires pour le stockage où la capacité du stockage sous-jacent est dépassée :
  >*Nécessaire d’e/s* = (disque logique ( *\<lecteur de base de données NTDS\>* ) \Avg disque s/lecture &divide; *\<cibler moyenne disque s/lecture\>* ) &times; LogicalDisk ( *\<lecteur de base de données NTDS\>* ) \Read/sec

|Compteur|Value|
|-|-|
|Disque logique réelle ( *\<lecteur de base de données NTDS\>* ) \Avg disque s/transfert|secondes.02 (20 millisecondes)|
|Cible du disque logique ( *\<lecteur de base de données NTDS\>* ) \Avg disque s/transfert|0,01 seconde|
|Multiplicateur de modification dans les e/s disponibles|0.02 &divide; 0.01 = 2|  
  
|Nom de valeur|Value|
|-|-|
|Disque logique ( *\<lecteur de base de données NTDS\>* ) \Transfers/sec|400|
|Multiplicateur de modification dans les e/s disponibles|2|
|Total des IOPS nécessaires durant la période de pointe|800|

Pour déterminer la vitesse à laquelle le cache doivent être initialisées :

- Déterminer la durée maximale acceptable pour préchauffer le cache. Il est soit la quantité de temps qu’il doit effectuer pour charger la base de données entière à partir du disque, ou pour les scénarios où la base de données ne peut pas être chargé dans la mémoire RAM, il s’agit de la durée maximale pour remplir la mémoire RAM.
- Déterminer la taille de la base de données, à l’exclusion des espaces blancs.  Pour plus d’informations, consultez [l’évaluation pour le stockage](#evaluating-for-storage).  
- Diviser la taille de la base de données de 8 Ko ; Il s’agit du total IOs nécessaires au chargement de la base de données.
- Divisez les total IOs par le nombre de secondes dans le laps de temps défini.

Notez que le taux calculé, tandis que précis, ne sera pas exact, car les pages précédemment chargées sont supprimées si ESE n’est pas configuré pour avoir une taille fixe de cache et les services AD DS par défaut utilise la taille du cache de variable.

|Points de données à collecter|Valeurs
|-|-|
|Durée maximale acceptable pendant préchargées|10 minutes (600 secondes)
|Taille de la base de données|2 Go|  
  
|Étape de calcul|Formule|Résultat|
|-|-|-|
|Calculer la taille de base de données dans les pages|(2 Go &times; 1024 &times; 1024) = *taille de base de données en Ko*|2 097 152 KO|
|Calculer le nombre de pages dans la base de données|2 097 152 Ko &divide; 8 Ko = *nombre de pages*|pages 262 144|
|Calculer les e/s nécessaires pour entièrement préchauffer le cache|pages 262 144 &divide; 600 secondes = *IOPS nécessaires*|437 E/S|

## <a name="processing"></a>Traitement en cours

### <a name="evaluating-active-directory-processor-usage"></a>L’évaluation de l’utilisation du processeur Active Directory

Pour la plupart des environnements, une fois que le stockage, de RAM et de mise en réseau sont correctement paramétrés comme décrit dans la section Planification, la gestion de la quantité de capacité de traitement sera le composant qui mérite le plus d’attention. Il existe deux défis lors de l’évaluation de la capacité du processeur nécessaire :

- Si les applications dans l’environnement sont en cours se comportant bien dans une infrastructure de services partagés et est décrite dans la section intitulée « Suivi coûteux et inefficaces recherches » dans l’article Création plus efficace Microsoft Active Annuaire des Applications ou éloignant SAM de bas niveau des appels à des appels LDAP.  
  
  Dans les environnements de grande taille, la raison à que cela est important est qu’applications mal codées peuvent lecteur volatilité dans la charge de l’UC, « subtiliser » énormément de temps processeur à partir d’autres applications, augmenter artificiellement les besoins en capacité et inégalement répartir la charge sur les contrôleurs de domaine.  
- Comme les services AD DS est un environnement distribué avec une grande variété de clients potentiels, les dépenses liées à un « client unique » de l’estimation sont subjective de l’environnement en raison des modèles d’utilisation et le type ou la quantité d’applications en tirant parti des services AD DS. En bref, comme la section mise en réseau, d’applicabilité large, cela est mieux proche du point de vue de l’évaluation de la capacité totale nécessaire dans l’environnement.

Pour les environnements existants, comme taille de stockage a été indiqué précédemment, il est supposé que le stockage est maintenant correctement dimensionné et par conséquent, les données concernant la charge du processeur sont valides. Pour rappel, il est essentiel pour vous assurer que le goulot d’étranglement dans le système n’est pas les performances du stockage. Quand il existe un goulot d’étranglement et le processeur est en attente, il existe des états inactifs qui disparaissent le goulot d’étranglement est supprimé.  États d’attente de processeur sont supprimées, par définition, l’utilisation du processeur augmente car il n’a plus à attendre les données. Par conséquent, de collecter les compteurs de performances « disque logique ( *\<lecteur de base de données NTDS\>* ) \Avg disque s/lecture » et « Process(lsass)\\% Processor Time ». Les données dans « Process(lsass)\\% temps processeur » sera artificiellement faible si « disque logique ( *\<lecteur de base de données NTDS\>* ) \Avg disque s/lecture » dépasse 10 à 15 ms, ce qui est un seuil général qui Prise en charge de Microsoft utilise pour résoudre les problèmes de performances liés au stockage. Comme auparavant, il est recommandé que les intervalles d’échantillonnage être 15, 30 ou 60 minutes. Quoi que ce soit inférieure sera généralement trop volatile pour les bonnes mesures ; toute valeur supérieure sera lisser excessivement lit quotidienne.

### <a name="introduction"></a>Présentation

Afin de planifier la planification de la capacité pour les contrôleurs de domaine, la puissance de traitement nécessite le plus d’attention et la compréhension. Lors du dimensionnement des systèmes pour garantir des performances optimales, il y a toujours un composant qui est le goulot d’étranglement et d’un contrôleur de domaine correctement dimensionné, ce sera le processeur.

Similaire à la section mise en réseau dans lequel la demande de l’environnement est examinée sur une base site par site, les mêmes doivent être effectuées pour la capacité de calcul demandée. Contrairement à la section mise en réseau, où les technologies de mise en réseau disponibles dépassent largement la normale à la demande, plus particulièrement à dimensionner la capacité de l’UC.  Comme n’importe quel environnement de taille moyenne ; quoi que ce soit sur quelques milliers d’utilisateurs simultanés permettre placer une charge importante sur le processeur.

Malheureusement, en raison de la variabilité énorme des applications clientes qui tirent parti d’Active Directory, l’estimation générale d’utilisateurs par UC est inapplicable confronter à tous les environnements. Plus précisément, les demandes de calcul sont soumises à profil de comportement et l’application utilisateur. Par conséquent, chaque environnement doit être dimensionnée individuellement.

#### <a name="target-site-behavior-profile"></a>Profil de comportement de site cible

Comme mentionné précédemment, lors de la planification de capacité pour un site complet, l’objectif est de cibler une conception avec un *N* + conception 1 capacité, dont la défaillance d’un système pendant la période de pointe permettant de la continuation du service à un raisonnable niveau de qualité. Cela signifie que dans une «*N*« scénario, charge sur toutes les zones doit être inférieure à 100 % (mieux encore, moins de 80 %) pendant les heures creuses.

En outre, si les applications et les clients du site sont à l’aide de meilleures pratiques pour la localisation des contrôleurs de domaine (autrement dit, à l’aide de la [fonction DsGetDcName](http://msdn.microsoft.com/en-us/library/windows/desktop/ms675983(v=vs.85).aspx)), les clients doivent être distribués de façon relativement uniforme avec mineur pics temporaires en raison d’un nombre quelconque de facteurs.

Dans l’exemple suivant, les hypothèses suivantes sont effectuées :

- Chacune des cinq contrôleurs de domaine dans le site a quatre unités centrales.
- Objectif total de l’utilisation du processeur pendant les heures d’ouverture est de 40 % dans des conditions normales («*N* + 1 ») et 60 % sinon («*N*»). Pendant les heures creuses, l’utilisation de l’unité centrale cible est 80 %, car le logiciel de sauvegarde et d’autres tâches de maintenance sont censés consommer toutes les ressources disponibles.

![Graphique d’utilisation du processeur](media/capacity-planning-considerations-cpu-chart.png)

Analyse des données dans le graphique (processeur Information(_Total)\% processeur Utility) pour chacun des contrôleurs de domaine :

- La plupart du temps, la charge est relativement répartie qui est ce que sont susceptibles de lorsque les clients utilisent le localisateur de contrôleur de domaine et ont écrit également les recherches. 
- Il existe un nombre de pics de cinq minutes de 10 %, avec certains aussi grande que 20 %. En règle générale, sauf s’ils entraînent la cible du plan de capacité à être dépassée, examinez ces n’est pas utile.  
- La période de pointe pour tous les systèmes est entre environ 8 h 00 et 9 h 15. Avec la transition en douceur d’environ 5 h 00 environ 5:00 heures, il s’agit généralement un bon indicateur de celui-ci. Que plus aléatoire des pics d’utilisation du processeur sur un scénario de zone par zone entre 17 h 00 et 4 h 00 serait en dehors de la planification de capacité.

  >[!NOTE]
  >Sur un système bien géré, ladite pics sont peut-être en cours d’exécution logiciel de sauvegarde, analyses antivirus complète du système, inventaire logiciel ou matériel, logiciel ou un correctif de déploiement et ainsi de suite. Car elles ne sont pas le cycle d’activité utilisateur pointe, les cibles ne sont pas dépassées.

- Comme chaque système est d’environ 40 %, et tous les systèmes ont les mêmes numéros d’unités centrales, doit un échouer ou être mis hors connexion, les systèmes restants seraient exécutera à environ 53 % (charge de 40 % du système D est uniformément fractionnée et ajoutée à d’un système et de système C existant 40 % de la charge). Pour plusieurs raisons, cette hypothèse linéaire n’est pas parfaitement précis, mais fournit suffisamment de précision pour la jauge.  

  **Autre scénario –** deux contrôleurs de domaine en cours d’exécution à 40 % : Un domaine contrôleur échoue, UC estimé sur celle restante serait environ 80 %. Cela beaucoup de dépasse les seuils présentées ci-dessus pour planifier la capacité et met également à limiter sévèrement la quantité d’espace de tête pour les 10 à 20 % vu dans le profil de charge ci-dessus, ce qui signifie que les pics seraient lecteur le contrôleur de domaine à 90 % à 100 % au cours de la «*N*« scénario et sans aucun doute dégrader la réactivité.

### <a name="calculating-cpu-demands"></a>Calcul des demandes de processeur

Le « processus\\% temps processeur » compteur de performance objet additionne le montant total du temps écoulé que tous les threads d’une application de dépenses sur l’UC et l’heure divise par la quantité totale du système. L’effet est qu’une application multithread sur un système multiprocesseur peut dépasser 100 % du temps processeur et serait interprétée différemment que « informations sur le processeur\\% processeur utilitaire ». Dans la pratique la « Process(lsass)\\% temps processeur » peut être considéré comme le nombre de processeurs à 100 % qui sont nécessaires pour prendre en charge les demandes du processus. Une valeur de 200 % signifie que 2 processeurs, chacun à 100 %, sont nécessaires pour prendre en charge le chargement complet de services AD DS. Bien qu’un processeur à 100 % de leur capacité est le meilleur parti des coûts efficace du point de vue de l’argent au niveau des processeurs et la consommation d’énergie, à un nombre de raisons détaillées dans l’annexe A, une meilleure réactivité sur un système multi-thread se produit lorsque le système est inactif à 100 %.

Pour prendre en compte les pics temporaires dans la charge du client, il est recommandé pour cibler un processeur de période de pointe d’entre 40 et 60 % de la capacité du système. Utilisez l’exemple ci-dessus, cela signifierait qu’entre 3.33 (cible de 60 %) et 5 (cible de 40 %) processeurs seraient nécessaire pour le chargement (processus lsass) d’AD DS. Une capacité supplémentaire doit être ajoutée dans en fonction des exigences du système d’exploitation de base et d’autres agents requis (tels que les antivirus, sauvegarde, l’analyse et ainsi de suite). Bien que l’impact des agents doit être évalué sur une base par environnement, une estimation d’est comprise entre 5 et 10 % d’un processeur unique peut être effectuée. Dans l’exemple actuel, cela suggère qu’entre 3.43 (cible de 60 %) et 5.1 (cible de 40 %) processeurs sont nécessaires pendant les périodes de pointe.

Le plus simple pour ce faire consiste à utiliser la vue « Empilée » dans Windows moniteur de fiabilité et performances (perfmon), pour s’assurer que tous les compteurs sont mis à l’échelle le même.

Hypothèses :

- Vise à réduire l’encombrement aux serveurs aussi peu que possible. Dans l’idéal, un seul serveur exécute la charge et un serveur supplémentaire ajouté pour assurer la redondance (*N* + 1 scénario).

![Graphique du temps processeur pour le processus lsass (sur tous les processeurs)](media/capacity-planning-considerations-proc-time-chart.png)

Connaissances acquises à partir des données dans le graphique (Process(lsass)\\% Processor Time) :

- Le jour ouvré démarre précieuse environ 7:00 et diminue à 17 h 00.
- La période de pointe le plus occupée est de 9 h 30 à 11:00 PST. 
  > [!NOTE]
  > Toutes les données de performances sont historiques. Le point de données de pointe à 9 h 15 indique la charge de 9 h 00 à 9 h 15.
- Il existe des pics avant 7 h 00 qui peut indiquer une charge à partir de différents fuseaux horaires ou activité d’infrastructure en arrière-plan, telles que les sauvegardes. Étant donné que le pic à 9 h 30 dépasse cette activité, elle ne concerne pas.
- Il existe trois contrôleurs de domaine dans le site.

À la charge maximale, lsass consomme environ 485 % d’une UC ou de 4.85 processeurs à 100 %. Selon les calculs précédemment, cela signifie que le site a besoin d’environ 12,25 processeurs pour AD DS. Ajouter dans les suggestions ci-dessus de 5 à 10 % pour les processus en arrière-plan, et cela signifie que devez environ 12 h 30 à 12,35 processeurs pour prendre en charge de la même charge en remplaçant le serveur dès aujourd'hui. Une estimation de l’environnement maintenant une croissance doit être pris en compte.

### <a name="when-to-tune-ldap-weights"></a>Lorsque paramétrer les pondérations LDAP

Il existe plusieurs scénarios où le paramétrage [LdapSrvWeight](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc957291(v=technet.10)) doit être considéré comme. Dans le contexte de planification de la capacité, cela aurait fait lorsque les charges de l’application ou un utilisateur ne sont pas équilibrées, ou systèmes sous-jacents ne sont pas équilibrées en termes de fonctionnalité. Raisons pour ce faire, au-delà de la planification de capacité sont trouvent en dehors de la portée de cet article.

Il existe deux raisons principales pour paramétrer les pondérations de LDAP :

- L’émulateur PDC est un exemple qui affecte chaque environnement pour l’utilisateur ou de l’application qui charge comportement n'est pas répartie. Comme certains outils et les actions ciblent l’émulateur de contrôleur de domaine principal, tels que les outils de gestion de stratégie de groupe, le deuxième tentatives en cas d’échecs d’authentification, établissement de l’approbation et ainsi de suite, les ressources processeur sur l’émulateur PDC peuvent être plus lourdement sollicitées qu’ailleurs dans le site.
  - Il est uniquement utile de paramétrer ce s’il existe une différence notable de l’utilisation du processeur afin de réduire la charge sur l’émulateur PDC et augmenter la charge sur les autres contrôleurs de domaine permettra une meilleure répartition de charge.
  - Dans ce cas, définissez LDAPSrvWeight comprise entre 50 et 75 pour l’émulateur PDC.
- Serveurs avec différents nombres de processeurs (et de vitesses) dans un site.  Par exemple, il existe deux serveurs de huit cœurs et un serveur de quatre cœurs.  Le dernier serveur a la moitié des processeurs des deux autres serveurs.  Cela signifie qu’une charge distribuée bien client augmentera la charge moyenne du processeur dans la boîte de quatre cœurs à environ deux fois celle des cases à huit cœurs.
  - Par exemple, les deux zones de huit cœurs s’exécuterait à 40 % et la zone de quatre noyaux s’exécuterait à 80 %.
  - En outre, envisagez l’impact de la perte d’une zone de huit cœurs dans ce scénario, en particulier le fait que la zone de quatre cœurs est désormais être surchargée.

#### <a name="example-1---pdc"></a>Exemple 1 : contrôleur de domaine principal

| |Utilisation avec les valeurs par défaut|Nouveau LdapSrvWeight|Utilisation du nouveau estimé|
|-|-|-|-|
|1 contrôleur de domaine (émulateur PDC)|53%|57|40%|
|DC 2|33%|100|40%|
|CONTRÔLEUR DE DOMAINE 3|33%|100|40%|

La difficulté est que si le rôle d’émulateur PDC est transféré ou saisi, en particulier à un autre contrôleur de domaine dans le site, il y aura une augmentation considérable sur le nouvel émulateur PDC.

À l’aide de l’exemple à partir de la section [profil de comportement de site cible](#target-site-behavior-profile), une hypothèse a été faite que tous les trois contrôleurs de domaine dans le site avaient quatre unités centrales. Ce qui doit se produire, dans des conditions normales, si un des contrôleurs de domaine a huit UC ? Il y aura deux contrôleurs de domaine à 40 % d’utilisation et l’autre à 20 % d’utilisation. Bien que cela ne soit pas incorrect, il existe une opportunité d’un peu mieux équilibrer la charge. Tirez parti des poids LDAP pour effectuer cette opération.  Un exemple de scénario serait :

#### <a name="example-2---differing-cpu-counts"></a>Exemple 2 - UC qui se différencie compte

| |Informations sur le processeur\\ %&nbsp;Utility(_Total) de processeur<br />Utilisation avec les valeurs par défaut|Nouveau LdapSrvWeight|Utilisation du nouveau estimé|
|-|-|-|-|
|CONTRÔLEUR DE DOMAINE 4 UC 1|40|100|30%|
|CONTRÔLEUR DE DOMAINE 4 UC 2|40|100|30%|
|CONTRÔLEUR DE DOMAINE DE 8 PROCESSEURS 3|20|200|30%|

Soyez très prudent avec ces scénarios. Comme illustré ci-dessus, les calculs recherche vraiment agréable et plus agréable sur papier. Mais tout au long de cet article, la planification d’une «*N* + 1 » scénario est d’une importance capitale. L’impact d’un contrôleur de domaine hors connexion doit être calculée pour chaque scénario. Dans le scénario précédent, où la distribution de charge est pair, afin de garantir un 60 % de charge pendant un «*N*« scénario, avec la charge équilibrée uniformément entre tous les serveurs, la distribution fera l’affaire restez en les proportions cohérent. Recherchez l’émulateur de contrôleur de domaine principal scénario de paramétrage et en général tout scénario où la charge utilisateur ou l’application est déséquilibrée, l’effet est très différent :

| |Utilisation du analysé|Nouveau LdapSrvWeight|Utilisation du nouveau estimé|
|-|-|-|-|
|1 contrôleur de domaine (émulateur PDC)|40%|85|47%|
|DC 2|40%|100|53%|
|CONTRÔLEUR DE DOMAINE 3|40%|100|53%|

### <a name="virtualization-considerations-for-processing"></a>Considérations relatives à la virtualisation pour le traitement

Il existe deux couches de la planification de capacité qui doivent être effectuées dans un environnement virtualisé. Au niveau de l’hôte, similaire à l’identification de la cycle des opérations décrites pour le contrôleur de domaine de traitement précédemment, les seuils pendant la période de pointe doivent être identifiés. Étant donné que les principaux sous-jacent sont les mêmes pour un ordinateur hôte de planification de threads invité sur l’UC que pour l’obtention des threads de AD DS sur le processeur sur un ordinateur physique, le même objectif de 40 à 60 % sur l’hôte sous-jacent sont recommandés. Dans la couche suivante, la couche d’invité, depuis les principaux de la planification des threads n'ont pas changé, l’objectif dans le reste de l’invité dans la plage de 40 à 60 %.

Dans un scénario mappé directe, un invité par hôte, tous les la planification des capacités fait jusqu'à présent doit être ajouté à la configuration requise (RAM, disque, réseau) du système d’exploitation hôte sous-jacent. Dans un scénario d’hébergement partagé, le test indique qu’il existe 10 % impact sur l’efficacité des processeurs sous-jacent. Cela signifie que si un site a besoin de 10 processeurs à une cible de 40 %, la quantité de processeurs virtuels à attribuer au sein de tous les «*N*» invités serait 11. Dans un site avec une distribution mixte de serveurs physiques et des serveurs virtuels, le modificateur s’applique uniquement aux machines virtuelles. Par exemple, si un site comporte un «*N* + 1 » scénario, un serveur physique ou mappé en direct avec 10 unités centrales serait sur équivalent à un invité avec des 11 processeurs sur un ordinateur hôte, avec des 11 processeurs réservés pour le contrôleur de domaine.

Tout au long de l’analyse et de calcul des quantités de processeur nécessaires pour prendre en charge le chargement d’AD DS, les numéros d’unités centrales qui correspondent à ce qui peut être acheté en matériel physique des termes du contrat ne correspondent pas nécessairement proprement. La virtualisation vous évite de devoir est arrondie. La virtualisation réduit les efforts nécessaires pour ajouter la capacité de calcul à un site, étant donné la facilité avec laquelle un processeur peut être ajouté à une machine virtuelle. Il n’élimine pas le besoin pour évaluer avec précision la puissance de calcul nécessaire pour que le matériel sous-jacent est disponible lorsque des processeurs supplémentaires doivent être ajoutés pour les invités.  Comme toujours, n’oubliez pas de planifier et de surveiller la croissance de la demande.

### <a name="calculation-summary-example"></a>Exemple de résumé de calcul

|System|Pic processeur|
|-|-|-|
|CONTRÔLEUR DE DOMAINE 1|120%|
|DC 2|147%|
|Dc 3|218%|
|Temps processeur total utilisé|485%|  
  
|Nombre de systèmes cible|Bande passante totale (ci-dessus)|
|-|-|
|Processeurs nécessaires à la cible de 40 %|4.85 &divide; .4 = 12.25|

Extensible en raison de l’importance de ce point, *n’oubliez pas de planifier la croissance*. En supposant que la croissance de 50 % sur les trois prochaines années, cette environment aura besoin d’unités 18.375 centrales (12,25 &times; 1.5) à la marque de trois ans. Un plan de rechange consisterait à examiner après la première année et ajouter des capacités supplémentaires en fonction des besoins.

### <a name="cross-trust-client-authentication-load-for-ntlm"></a>Charge de l’authentification de client de l’approbation croisée pour NTLM

#### <a name="evaluating-cross-trust-client-authentication-load"></a>Charge de l’authentification de client de niveau de confiance entre l’évaluation

De nombreux environnements peuvent avoir un ou plusieurs domaines reliés par une relation d’approbation. Une demande d’authentification pour une identité dans un autre domaine qui n’utilise pas l’authentification Kerberos doit traverser une approbation à l’aide d’un canal sécurisé du contrôleur de domaine vers un autre contrôleur de domaine dans le domaine de destination ou le domaine suivant dans le chemin d’accès pour le domaine de destination. Le nombre d’appels simultanés en utilisant le canal sécurisé un contrôleur de domaine peut apporter à un contrôleur de domaine dans un domaine approuvé est contrôlé par un paramètre appelé **MaxConcurrentAPI**. Pour les contrôleurs de domaine, en garantissant que le canal sécurisé peut gérer la quantité de charge s’effectue par une des deux approches : réglage **MaxConcurrentAPI** ou, dans une forêt, la création des raccourcis d’approbation. Pour évaluer le volume de trafic via une approbation individuelle, reportez-vous à [comment effectuer le réglage des performances pour l’authentification NTLM en utilisant le paramètre MaxConcurrentApi](https://support.microsoft.com/kb/2688798).

Pendant la collecte de données, comme avec tous les autres scénarios, doit être collectée pendant les périodes occupé de pointe de la journée pour les données être utile.

> [!NOTE]
> Intraforêt et scénarios entre forêts peuvent entraîner l’authentification de traverser plusieurs approbations et chaque étape devra être paramétrées.

#### <a name="planning"></a>Planification

Il existe un nombre d’applications qui utilisent l’authentification NTLM par défaut, ou l’utiliser dans un scénario de configuration certains. Serveurs d’applications augmente capacité et un nombre croissant de clients actifs de service. Il existe également une tendance que les clients ne fermez pas sessions pendant une durée limitée et vous reconnecter au lieu de cela régulièrement (par exemple, la synchronisation du courrier électronique par extraction). Un autre exemple courant pour une charge élevée NTLM est serveurs proxy web qui requièrent une authentification pour accéder à Internet.

Ces applications peuvent entraîner une charge importante pour l’authentification NTLM, qui peut placer une pression importante sur les contrôleurs de domaine, en particulier lorsque les utilisateurs et les ressources sont dans des domaines différents.

Il existe plusieurs approches de gestion de la charge de cross-trust, qui, dans la pratique, sont utilisées conjointement, plutôt que dans un exclusif soit / ou scénario. Les options possibles sont :

- Réduire l’authentification du client de confiance entre en recherchant les services qui consomme d’un utilisateur dans le même domaine que l’utilisateur réside dans.
- Augmenter le nombre de secure-canaux disponibles. Cela s’applique à la forêt et le trafic entre les forêts et sont des raccourcis d’approbation.
- Régler les paramètres par défaut **MaxConcurrentAPI**.

Pour le réglage **MaxConcurrentAPI** sur un serveur existant, l’équation est :

> *New_MaxConcurrentApi_setting* &ge; (*semaphore_acquires* + *semaphore_time-aboutissants*) &times; *average_semaphore_ hold_time* &divide; *time_collection_length*

Pour plus d’informations, consultez [article de la base de connaissances 2688798 : Comment effectuer le réglage des performances pour l’authentification NTLM en utilisant le paramètre MaxConcurrentApi](http://support.microsoft.com/kb/2688798).

## <a name="virtualization-considerations"></a>Éléments à prendre en compte en matière de virtualisation

Aucun, il s’agit du paramètre de réglage de système d’exploitation.

### <a name="calculation-summary-example"></a>Exemple de résumé de calcul

|Type de données|Value|
|-|-|
|Acquiert le sémaphore (Minimum)|6,161|
|Acquiert le sémaphore (Maximum)|6,762|
|Délais d’expiration de sémaphore|0|
|Durée de conservation de sémaphore moyenne|0.012|
|Durée (en secondes) de la collection|1:11 minutes (71 secondes)|
|Formule (de base de connaissances 2688798)|((6762 &ndash; 6161) + 0) &times; 0.012 /|
|Valeur minimale pour **MaxConcurrentAPI**|((6762 &ndash; 6161) + 0) &times; 0.012 &divide; 71 = .101|

Pour ce système pour cette période, les valeurs par défaut sont acceptables.

## <a name="monitoring-for-compliance-with-capacity-planning-goals"></a>Surveillance de la conformité avec les objectifs de planification de capacité

Tout au long de cet article, il a été évoquée que de planification et de mise à l’échelle aller vers les cibles de l’utilisation. Voici un graphique de synthèse des seuils recommandées qui doivent être surveillés pour vérifier que les systèmes sont exécutent dans une limite de capacité adéquate. N’oubliez pas qu’il s’agit pas des seuils de performance, mais que les seuils de planification de capacité. Un serveur d’exploitation qui dépassent ces seuils fonctionnera, mais il est temps de commencer la validation que toutes les applications sont bien au comportement médiocre. Si vous dit que les applications sont au comportement correct, il est temps pour démarrer l’évaluation des mises à niveau matérielles ou autres modifications de configuration.

|Category|Compteur de performances|Intervalle/échantillonnage|Cible|Warning|
|-|-|-|-|-|
|Processeur|Processor Information(_Total)\\% Processor Utility|60 min|40%|60%|
|RAM (Windows Server 2008 R2 or earlier)|Memory\Available MB|< 100 MB|N/A|< 100 MB|
|RAM (Windows Server 2012)|Memory\Long-Term Average Standby Cache Lifetime(s)|30 min|Must be tested|Must be tested|
|Network|Network Interface(\*)\Bytes Sent/sec<br /><br />Network Interface(\*)\Bytes Received/sec|30 min|40%|60%|
|Storage|LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read<br /><br />LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Write|60 min|10 ms|15 ms|
|AD Services|Netlogon(\*)\Average Semaphore Hold Time|60 min|0|1 second|

## Appendix A: CPU sizing criteria

### Definitions

**Processor (microprocessor) –** a component that reads and executes program instructions  

**CPU –** Central Processing Unit  

**Multi-Core processor –** multiple CPUs on the same integrated circuit  

**Multi-CPU –** multiple CPUs, not on the same integrated circuit  

**Logical Processor –** one logical computing engine from the perspective of the operating system  

This includes hyper-threaded, one core on multi-core processor, or a single core processor.  

As today’s server systems have multiple processors, multiple multi-core processors, and hyper-threading, this information is generalized to cover both scenarios. As such, the term logical processor will be used as it represents the operating system and application perspective of the available computing engines.

### Thread-level parallelism

Each thread is an independent task, as each thread has its own stack and instructions. Because AD DS is multi-threaded and the number of available threads can be tuned by using [How to view and set LDAP policy in Active Directory by using Ntdsutil.exe](http://support.microsoft.com/kb/315071), it scales well across multiple logical processors.

### Data-level parallelism

This involves sharing data across multiple threads within one process (in the case of the AD DS process alone) and across multiple threads in multiple processes (in general). With concern to over-simplifying the case, this means that any changes to data are reflected to all running threads in all the various levels of cache (L1, L2, L3) across all cores running said threads as well as updating shared memory. Performance can degrade during write operations while all the various memory locations are brought consistent before instruction processing can continue.

### CPU speed vs. multiple-core considerations

The general rule of thumb is faster logical processors reduce the duration it takes to process a series of instructions, while more logical processors means that more tasks can be run at the same time. These rules of thumb break down as the scenarios become inherently more complex with considerations of fetching data from shared-memory, waiting on data-level parallelism, and the overhead of managing multiple threads. This is also why scalability in multi-core systems is not linear.

Consider the following analogies in these considerations: think of a highway, with each thread being an individual car, each lane being a core, and the speed limit being the clock speed.

1. If there is only one car on the highway, it doesn’t matter if there are two lanes or 12 lanes. That car is only going to go as fast as the speed limit will allow.
1. Assume that the data the thread needs is not immediately available. The analogy would be that a segment of road is shutdown. If there is only one car on the highway, it doesn’t matter what the speed limit is until the lane is reopened (data is fetched from memory).
1. As the number of cars increase, the overhead to manage the number of cars increases. Compare the experience of driving and the amount of attention necessary when the road is practically empty (such as late evening) versus when the traffic is heavy (such as mid-afternoon, but not rush hour). Also, consider the amount of attention necessary when driving on a two-lane highway, where there is only one other lane to worry about what the drivers are doing, versus a six-lane highway where one has to worry about what a lot of other drivers are doing.
   > [!NOTE]
   > The analogy about the rush hour scenario is extended in the next section: Response Time/How the System Busyness Impacts Performance.

As a result, specifics about more or faster processors become highly subjective to application behavior, which in the case of AD DS is very environmentally specific and even varies from server to server within an environment. This is why the references earlier in the article do not invest heavily in being overly precise, and a margin of safety is included in the calculations. When making budget-driven purchasing decisions, it is recommended that optimizing usage of the processors at 40% (or the desired number for the environment) occurs first, before considering buying faster processors. The increased synchronization across more processors reduces the true benefit of more processors from the linear progression (2&times; the number of processors provides less than 2&times; available additional compute power).

> [!NOTE]
> Amdahl’s Law and Gustafson’s Law are the relevant concepts here.

### Response time/How the system busyness impacts performance

Queuing theory is the mathematical study of waiting lines (queues). In queuing theory, the Utilization Law is represented by the equation:

*U* k = *B* &divide; *T*

Where *U* k is the utilization percentage, *B* is the amount of time busy, and *T* is the total time the system was observed. Translated into the context of Windows, this means the number of 100-nanosecond (ns) interval threads that are in a Running state divided by how many 100-ns intervals were available in given time interval. This is exactly the formula for calculating % Processor Utility (reference [Processor Object](https://docs.microsoft.com/previous-versions/ms804036(v=msdn.10)) and [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/previous-versions/windows/embedded/ms901169(v=msdn.10))).

Queuing theory also provides the formula: *N* = *U* k &divide; (1 &ndash; *U* k) to estimate the number of waiting items based on utilization ( *N* is the length of the queue). Charting this over all utilization intervals provides the following estimates to how long the queue to get on the processor is at any given CPU load.

![Queue length](media/capacity-planning-considerations-queue-length.png)

It is observed that after 50% CPU load, on average there is always a wait of one other item in the queue, with a noticeably rapid increase after about 70% CPU utilization.

Returning to the driving analogy used earlier in this section:

- The busy times of “mid-afternoon” would, hypothetically, fall somewhere into the 40% to 70% range. There is enough traffic such that one’s ability to pick any lane is not majorly restricted, and the chance of another driver being in the way, while high, does not require the level of effort to “find” a safe gap between other cars on the road.
- One will notice that as traffic approaches rush hour, the road system approaches 100% capacity. Changing lanes can become very challenging because cars are so close together that increased caution must be exercised to do so.

This is why the long term averages for capacity conservatively estimated at 40% allows for head room for abnormal spikes in load, whether said spikes transitory (such as poorly coded queries that run for a few minutes) or abnormal bursts in general load (the morning of the first day after a long weekend).

The above statement regards % Processor Time calculation being the same as the Utilization Law is a bit of a simplification for the ease of the general reader. For those more mathematically rigorous:  
- Translating the [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10))
  - *B* = The number of 100-ns intervals “Idle” thread spends on the logical processor. The change in the “*X*” variable in the [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10)) calculation
  - *T* = the total number of 100-ns intervals in a given time range. The change in the “*Y*” variable in the [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10)) calculation.
  - *U* k = The utilization percentage of the logical processor by the “Idle Thread” or % Idle Time.  
- Working out the math:
  - *U* k = 1 – %Processor Time
  - %Processor Time = 1 – *U* k
  - %Processor Time = 1 – *B* / *T*
  - %Processor Time = 1 – *X1* – *X0* / *Y1* – *Y0*

### Applying the concepts to capacity planning

The preceding math may make determinations about the number of logical processors needed in a system seem overwhelmingly complex. This is why the approach to sizing the systems is focused on determining maximum target utilization based on current load and calculating the number of logical processors required to get there. Additionally, while logical processor speeds will have a significant impact on performance, cache efficiencies, memory coherence requirements, thread scheduling and synchronization, and imperfectly balanced client load will all have significant impacts on performance that will vary on a server-by-server basis. With the relatively cheap cost of compute power, attempting to analyze and determine the perfect number of CPUs needed becomes more an academic exercise than it does provide business value.

Forty percent is not a hard and fast requirement, it is a reasonable start. Various consumers of Active Directory require various levels of responsiveness. There may be scenarios where environments can run at 80% or 90% utilization as a sustained average, as the increased wait times for access to the processor will not noticeably impact client performance. It is important to re-iterate that there are many areas in the system that are much slower than the logical processor in the system, including access to RAM, access to disk, and transmitting the response over the network. All of these items need to be tuned in conjunction. Examples:

- Adding more processors to a system running 90% that is disk-bound is probably not going to significantly improve performance. Deeper analysis of the system will probably identify that there are a lot of threads that are not even getting on the processor because they are waiting on I/O to complete.
- Resolving the disk-bound issues potentially means that threads that were previously spending a lot of time in a waiting state will no longer be in a waiting state for I/O and there will be more competition for CPU time, meaning that the 90% utilization in the previous example will go to 100% (because it can not go higher). Both components need to be tuned in conjunction.
  > [!NOTE]
  > Processor Information(*)\\% Processor Utility can exceed 100% with systems that have a "Turbo" mode.  This is where the CPU exceeds the rated processor speed for short periods.  Reference CPU manufacturers documentation and description of the counter for greater insight.  

Discussing whole system utilization considerations also brings into the conversation domain controllers as virtualized guests. [Response time/How the system busyness impacts performance](#response-timehow-the-system-busyness-impacts-performance) applies to both the host and the guest in a virtualized scenario. This is why in a host with only one guest, a domain controller (and generally any system) has near the same performance it does on physical hardware. Adding additional guests to the hosts increases the utilization of the underlying host, thereby increasing the wait times to get access to the processors as explained previously. In short, logical processor utilization needs to be managed at both the host and at the guest levels.

Extending the previous analogies, leaving the highway as the physical hardware, the guest VM will be analogized with a bus (an express bus that goes straight to the destination the rider wants). Imagine the following four scenarios:

- It is off hours, a rider gets on a bus that is nearly empty, and the bus gets on a road that is also nearly empty. As there is no traffic to contend with, the rider has a nice easy ride and gets there just as fast as if the rider had driven instead. The rider’s travel times are still constrained by the speed limit.
- It is off hours so the bus is nearly empty but most of the lanes on the road are closed, so the highway is still congested. The rider is on an almost-empty bus on a congested road. While the rider does not have a lot of competition in the bus for where to sit, the total trip time is still dictated by the rest of the traffic outside.
- It is rush hour so the highway and the bus are congested. Not only does the trip take longer, but getting on and off the bus is a nightmare because people are shoulder to shoulder and the highway is not much better. Adding more busses (logical processors to the guest) does not mean they can fit on the road any more easily, or that the trip will be shortened.
- The final scenario, though it may be stretching the analogy a little, is where the bus is full, but the road is not congested. While the rider will still have trouble getting on and off the bus, the trip will be efficient after the bus is on the road. This is the only scenario where adding more busses (logical processors to the guest) will improve guest performance.

From there it is relatively easy to extrapolate that there are a number of scenarios in between the 0%-utilized and the 100%-utilized state of the road and the 0%- and 100%-utilized state of the bus that have varying degrees of impact.

Applying the principals above of 40% CPU as reasonable target for the host as well as the guest is a reasonable start for the same reasoning as above, the amount of queuing.

## Appendix B: Considerations regarding different processor speeds, and the effect of processor power management on processor speeds

Throughout the sections on processor selection the assumption is made that the processor is running at 100% of clock speed the entire time the data is being collected and that the replacement systems will have the same speed processors. Despite both assumptions in practice being false, particularly with Windows Server 2008 R2 and later, where the default power plan is **Balanced**, the methodology still stands as it is the conservative approach. While the potential error rate may increase, it only increases the margin of safety as processor speeds increase.

- For example, in a scenario where 11.25 CPUs are demanded, if the processors were running at half speed when the data was collected, the more accurate estimate might be 5.125 &divide; 2.
- It is impossible to guarantee that doubling the clock speeds would double the amount of processing that happens for a given time period. This is due to the fact the amount of time that the processors spend waiting on RAM or other system components could stay the same. The net effect is that the faster processors might spend a greater percentage of the time idle while waiting on data to be fetched. Again, it is recommended to stick with the lowest common denominator, being conservative, and avoid trying to calculate a potentially false level of accuracy by assuming a linear comparison between processor speeds.

Alternatively, if processor speeds in replacement hardware are lower than current hardware, it would be safe to increase the estimate of processors needed by a proportionate amount. For example, it is calculated that 10 processors are needed to sustain the load in a site, and the current processors are running at 3.3 Ghz and replacement processors will run at 2.6 Ghz, this is a 21% decrease in speed. In this case, 12 processors would be the recommended amount.

That said, this variability would not change the Capacity Management processor utilization targets. As processor clock speeds will be adjusted dynamically based on the load demanded, running the system under higher loads will generate a scenario where the CPU spends more time in a higher clock speed state, making the ultimate goal to be at 40% utilization in a 100% clock speed state at peak. Anything less than that will generate power savings as CPU speeds will be throttled back during off peak scenarios.

> [!NOTE]
> An option would be to turn off power management on the processors (setting the power plan to **High Performance**) while data is collected. That would give a more accurate representation of the CPU consumption on the target server.

To adjust estimates for different processors, it used to be safe, excluding other system bottlenecks outlined above, to assume that doubling processor speeds doubled the amount of processing that could be performed.  Today, the internal architecture of processors is different enough between processors, that a safer way to gauge the effects of using different processors than data was taken from is to leverage the SPECint_rate2006 benchmark from Standard Performance Evaluation Corporation.

1. Find the SPECint_rate2006 scores for the processor that are in use and that plan to be used.
    1. On the website of the Standard Performance Evaluation Corporation, select **Results**, highlight **CPU2006**, and select **Search all SPECint_rate2006 results**.
    1. Under **Simple Request**, enter the search criteria for the target processor, for example **Processor Matches E5-2630 (baselinetarget)** and **Processor Matches E5-2650 (baseline)**.
    1. Find the server and processor configuration to be used (or something close, if an exact match is not available) and note the value in the **Result** and **# Cores** columns.
1. To determine the modifier use the following equation:
   >((*Target platform per-core score value*) &times; (*MHz per-core of baseline platform*)) &divide; ((*Baseline per-core score value*) &times; (*MHz per-core of target platform*))  

    Using the above example:
   >(35.83 &times; 2000) &divide; (33.75 &times; 2300) = 0.92
1. Multiply the estimated number of processors by the modifier.  In the above case to go from the E5-2650 processor to the E5-2630 processor multiply the calculated 11.25 CPUs &times; 0.92 = 10.35 processors needed.

## Appendix C: Fundamentals regarding the operating system interacting with storage

The queuing theory concepts outlined in [Response time/How the system busyness impacts performance](#response-timehow-the-system-busyness-impacts-performance) are also applicable to storage. Having a familiarity of how the operating system handles I/O is necessary to apply these concepts. In the Microsoft Windows operating system, a queue to hold the I/O requests is created for each physical disk. However, a clarification on physical disk needs to be made. Array controllers and SANs present aggregations of spindles to the operating system as single physical disks. Additionally, array controllers and SANs can aggregate multiple disks into one array set and then split this array set into multiple “partitions”, which is in turn presented to the operating system as multiple physical disks (ref. figure).

![Block spindles](media/capacity-planning-considerations-block-spindles.png)  

In this figure the two spindles are mirrored and split into logical areas for data storage (Data 1 and Data 2). These logical areas are viewed by the operating system as separate physical disks.

Although this can be highly confusing, the following terminology is used throughout this appendix to identify the different entities:

- **Spindle –** the device that is physically installed in the server.
- **Array –** a collection of spindles aggregated by controller.
- **Array partition –** a partitioning of the aggregated array
- **LUN –** an array, used when referring to SANs
- **Disk –** What the operating system observes to be a single physical disk.
- **Partition –** a logical partitioning of what the operating system perceives as a physical disk.

### Operating system architecture considerations

The operating system creates a First In/First Out (FIFO) I/O queue for each disk that is observed; this disk may be representing a spindle, an array, or an array partition. From the operating system perspective, with regard to handling I/O, the more active queues the better. As a FIFO queue is serialized, meaning that all I/Os issued to the storage subsystem must be processed in the order the request arrived. By correlating each disk observed by the operating system with a spindle/array, the operating system now maintains an I/O queue for each unique set of disks, thereby eliminating contention for scarce I/O resources across disks and isolating I/O demand to a single disk. As an exception, Windows Server 2008 introduces the concept of I/O prioritization, and applications designed to use the “Low” priority fall out of this normal order and take a back seat. Applications not specifically coded to leverage the “Low” priority default to “Normal.”

### Introducing simple storage subsystems

Starting with a simple example (a single hard drive inside a computer) a component-by-component analysis will be given. Breaking this down into the major storage subsystem components, the system consists of:

- **1 –** 10,000 RPM Ultra Fast SCSI HD (Ultra Fast SCSI has a 20 MB/s transfer rate)
- **1 –** SCSI Bus (the cable)
- **1 –** Ultra Fast SCSI Adapter
- **1 –** 32-bit 33 MHz PCI bus

Once the components are identified, an idea of how much data can transit the system, or how much I/O can be handled, can be calculated. Note that the amount of I/O and quantity of data that can transit the system is correlated, but not the same. This correlation depends on whether the disk I/O is random or sequential and the block size. (All data is written to the disk as a block, but different applications using different block sizes.) On a component-by-component basis:

- **The hard drive –** The average 10,000-RPM hard drive has a 7-millisecond (ms) seek time and a 3 ms access time. Seek time is the average amount of time it takes the read/write head to move to a location on the platter. Access time is the average amount of time it takes to read or write the data to disk, once the head is in the correct location. Thus, the average time for reading a unique block of data in a 10,000-RPM HD constitutes a seek and an access, for a total of approximately 10 ms (or .010 seconds) per block of data.

  When every disk access requires movement of the head to a new location on the disk, the read/write behavior is referred to as “random.” Thus, when all I/O is random, a 10,000-RPM HD can handle approximately 100 I/O per second (IOPS) (the formula is 1000 ms per second divided by 10 ms per I/O or 1000/10=100 IOPS).

  Alternatively, when all I/O occurs from adjacent sectors on the HD, this is referred to as sequential I/O. Sequential I/O has no seek time because when the first I/O is complete, the read/write head is at the start of where the next block of data is stored on the HD. Thus a 10,000-RPM HD is capable of handling approximately 333 I/O per second (1000 ms per second divided by 3 ms per I/O).

  >[!NOTE]
  >This example does not reflect the disk cache, where the data of one cylinder is typically kept. In this case, the 10 ms are needed on the first I/O and the disk reads the whole cylinder. All other sequential I/O is satisfied from the cache. As a result, in-disk caches might improve sequential I/O performance.
  
  So far, the transfer rate of the hard drive has been irrelevant. Whether the hard drive is 20 MB/s Ultra Wide or an Ultra3 160 MB/s, the actual amount of IOPS the can be handled by the 10,000-RPM HD is ~100 random or ~300 sequential I/O. As block sizes change based on the application writing to the drive, the amount of data that is pulled per I/O is different. For example, if the block size is 8 KB, 100 I/O operations will read from or write to the hard drive a total of 800 KB. However, if the block size is 32 KB, 100 I/O will read/write 3,200 KB (3.2 MB) to the hard drive. As long as the SCSI transfer rate is in excess of the total amount of data transferred, getting a “faster” transfer rate drive will gain nothing. See the following tables for comparison.

  | |7200 RPM 9ms seek, 4ms access|10,000 RPM 7ms seek, 3ms access|15,000 RPM 4ms seek, 2ms access
  |-|-|-|-|
  |Random I/O|80|100|150|
  |Sequential I/O|250|300|500|  
  
  |10,000 RPM drive|8 KB block size (Active Directory Jet)|
  |-|-|
  |Random I/O|800 KB/s|
  |Sequential I/O|2400 KB/s|

- **SCSI backplane (bus) –** Understanding how the “SCSI backplane (bus)”, or in this scenario the ribbon cable, impacts throughput of the storage subsystem depends on knowledge of the block size. Essentially the question would be, how much I/O can the bus handle if the I/O is in 8 KB blocks? In this scenario, the SCSI bus is 20 MB/s, or 20480 KB/s. 20480 KB/s divided by 8 KB blocks yields a maximum of approximately 2500 IOPS supported by the SCSI bus.

  >[!NOTE]
  >The figures in the following table represent an example. Most attached storage devices currently use PCI Express, which provides much higher throughput.  
  
  |I/O supported by SCSI bus per block size|2 KB block size|8 KB block size (AD Jet) (SQL Server 7.0/SQL Server 2000)
  |-|-|-|
  |20 MB/s|10,000|2,500|
  |40 MB/s|20,000|5,000|
  |128 MB/s|65,536|16,384|
  |320 MB/s|160,000|40,000|

  As can be determined from this chart, in the scenario presented, no matter what the use, the bus will never be a bottleneck, as the spindle maximum is 100 I/O, well below any of the above thresholds.

  >[!NOTE]
  >This assumes that the SCSI bus is 100% efficient.
  
- **SCSI adapter –** For determining the amount of I/O that this can handle, the manufacturer’s specifications need to be checked. Directing I/O requests to the appropriate device requires processing of some sort, thus the amount of I/O that can be handled is dependent on the SCSI adapter (or array controller) processor.

  In this example, the assumption that 1,000 I/O can be handled will be made.

- **PCI bus –** This is an often overlooked component. In this example, this will not be the bottleneck; however as systems scale up, it can become a bottleneck. For reference, a 32 bit PCI bus operating at 33Mhz can in theory transfer 133 MB/s of data. Following is the equation:  
  > 32 bits &divide; 8 bits per byte &times; 33 MHz = 133 MB/s.  

  Note that is the theoretical limit; in reality only about 50% of the maximum is actually reached, although in certain burst scenarios, 75% efficiency can be obtained for short periods.

  A 66Mhz 64-bit PCI bus can support a theoretical maximum of (64 bits &divide; 8 bits per byte &times; 66 Mhz) = 528 MB/sec. Additionally, any other device (such as the network adapter, second SCSI controller, and so on) will reduce the bandwidth available as the bandwidth is shared and the devices will contend for the limited resources.

After analysis of the components of this storage subsystem, the spindle is the limiting factor in the amount of I/O that can be requested, and consequently the amount of data that can transit the system. Specifically, in an AD DS scenario, this is 100 random I/O per second in 8 KB increments, for a total of 800 KB per second when accessing the Jet database. Alternatively, the maximum throughput for a spindle that is exclusively allocated to log files would suffer the following limitations: 300 sequential I/O per second in 8 KB increments, for a total of 2400 KB (2.4 MB) per second.

Now, having analyzed a simple configuration, the following table demonstrates where the bottleneck will occur as components in the storage subsystem are changed or added.

|Notes|Bottleneck analysis|Disk|Bus|Adapter|PCI bus|
|-|-|-|-|-|-|
|This is the domain controller configuration after adding a second disk. The disk configuration represents the bottleneck at 800 KB/s.|Add 1 disk (Total=2)<br /><br />I/O is random<br /><br />4 KB block size<br /><br />10,000 RPM HD|200 I/Os total<br />800 KB/s total.| | | |
|After adding 7 disks, the disk configuration still represents the bottleneck at 3200 KB/s.|**Add 7 disks (Total=8)**  <br /><br />I/O is random<br /><br />4 KB block size<br /><br />10,000 RPM HD|800 I/Os total.<br />3200 KB/s total| | | |
|After changing I/O to sequential, the network adapter becomes the bottleneck because it is limited to 1000 IOPS.|Add 7 disks (Total=8)<br /><br />**I/O is sequential**<br /><br />4 KB block size<br /><br />10,000 RPM HD| | |2400 I/O sec can be read/written to disk, controller limited to 1000 IOPS| |
|After replacing the network adapter with a SCSI adapter that supports 10,000 IOPS, the bottleneck returns to the disk configuration.|Add 7 disks (Total=8)<br /><br />I/O is random<br /><br />4 KB block size<br /><br />10,000 RPM HD<br /><br />**Upgrade SCSI adapter (now supports 10,000 I/O)**|800 I/Os total.<br />3,200 KB/s total| | | |
|After increasing the block size to 32 KB, the bus becomes the bottleneck because it only supports 20 MB/s.|Add 7 disks (Total=8)<br /><br />I/O is random<br /><br />**32 KB block size**<br /><br />10,000 RPM HD| |800 I/Os total. 25,600 KB/s (25 MB/s) can be read/written to disk.<br /><br />The bus only supports 20 MB/s| | |
|After upgrading the bus and adding more disks, the disk remains the bottleneck.|**Add 13 disks (Total=14)**<br /><br />Add second SCSI adapter with 14 disks<br /><br />I/O is random<br /><br />4 KB block size<br /><br />10,000 RPM HD<br /><br />**Upgrade to 320 MB/s SCSI bus**|2800 I/Os<br /><br />11,200 KB/s (10.9 MB/s)| | | |
|After changing I/O to sequential, the disk remains the bottleneck.|Add 13 disks (Total=14)<br /><br />Add second SCSI Adapter with 14 disks<br /><br />**I/O is sequential**<br /><br />4 KB block size<br /><br />10,000 RPM HD<br /><br />Upgrade to 320 MB/s SCSI bus|8,400 I/Os<br /><br />33,600 KB\s<br /><br />(32.8 MB\s)| | | |
|After adding faster hard drives, the disk remains the bottleneck.|Add 13 disks (Total=14)<br /><br />Add second SCSI adapter with 14 disks<br /><br />I/O is sequential<br /><br />4 KB block size<br /><br />**15,000 RPM HD**<br /><br />Upgrade to 320 MB/s SCSI bus|14,000 I/Os<br /><br />56,000 KB/s<br /><br />(54.7 MB/s)| | | |
|After increasing the block size to 32 KB, the PCI bus becomes the bottleneck.|Add 13 disks (Total=14)<br /><br />Add second SCSI adapter with 14 disks<br /><br />I/O is sequential<br /><br />**32 KB block size**<br /><br />15,000 RPM HD<br /><br />Upgrade to 320 MB/s SCSI bus| | | |14,000 I/Os<br /><br />448,000 KB/s<br /><br />(437 MB/s) is the read/write limit to the spindle.<br /><br />The PCI bus supports a theoretical maximum of 133 MB/s (75% efficient at best).|

### Introducing RAID

The nature of a storage subsystem does not change dramatically when an array controller is introduced; it just replaces the SCSI adapter in the calculations. What does change is the cost of reading and writing data to the disk when using the various array levels (such as RAID 0, RAID 1, or RAID 5).

In RAID 0, the data is striped across all the disks in the RAID set. This means that during a read or a write operation, a portion of the data is pulled from or pushed to each disk, increasing the amount of data that can transit the system during the same time period. Thus, in one second, on each spindle (again assuming 10,000-RPM drives), 100 I/O operations can be performed. The total amount of I/O that can be supported is N spindles times 100 I/O per second per spindle (yields 100*N I/O per second).

![Logical d: drive](media/capacity-planning-considerations-logical-d-drive.png)

In RAID 1, the data is mirrored (duplicated) across a pair of spindles for redundancy. Thus, when a read I/O operation is performed, data can be read from both of the spindles in the set. This effectively makes the I/O capacity from both disks available during a read operation. The caveat is that write operations gain no performance advantage in a RAID 1. This is because the same data needs to be written to both drives for the sake of redundancy. Though it does not take any longer, as the write of data occurs concurrently on both spindles, because both spindles are occupied duplicating the data, a write I/O operation in essence prevents two read operations from occurring. Thus, every write I/O costs two read I/O. A formula can be created from that information to determine the total number of I/O operations that are occurring:  

> *Read I/O* + 2 &times; *Write I/O* = *Total available disk I/O consumed*  

When the ratio of reads to writes and the number of spindles are known, the following equation can be derived from the above equation to identify the maximum I/O that can be supported by the array:  

> *Maximum IOPS per spindle* &times; 2 spindles &times; [(*%Reads* + *%Writes*) &divide; (*%Reads* + 2 &times; *%Writes*)] = *Total IOPS*

RAID 1+ 0, behaves exactly the same as RAID 1 regarding the expense of reading and writing. However, the I/O is now striped across each mirrored set. If  

> *Maximum IOPS per spindle* &times; 2 spindles &times; [(*%Reads* + *%Writes*) &divide; (*%Reads* + 2 &times; *%Writes*)] = *Total I/O*  

in a RAID 1 set, when a multiplicity (*N*) of RAID 1 sets are striped, the Total I/O that can be processed becomes N &times; I/O per RAID 1 set:  

> *N* &times; {*Maximum IOPS per spindle* &times; 2 spindles &times; [(*%Reads* + *%Writes*) &divide; (*%Reads* + 2 &times; *%Writes*)] } = *Total IOPS*

In RAID 5, sometimes referred to as *N* + 1 RAID, the data is striped across *N* spindles and parity information is written to the “+ 1” spindle. However, RAID 5 is much more expensive when performing a write I/O than RAID 1 or 1 + 0. RAID 5 performs the following process every time a write I/O is submitted to the array:

1. Read the old data
1. Read the old parity
1. Write the new data
1. Write the new parity

As every write I/O request that is submitted to the array controller by the operating system requires four I/O operations to complete, write requests submitted take four times as long to complete as a single read I/O. To derive a formula to translate I/O requests from the operating system perspective to that experienced by the spindles:  

> *Read I/O* + 4 &times; *Write I/O* = *Total I/O*  

Similarly in a RAID 1 set, when the ratio of reads to writes and the number of spindles are known, the following equation can be derived from the above equation to identify the maximum I/O that can be supported by the array (Note that total number of spindles does not include the “drive” lost to parity):  

> *IOPS per spindle* &times; (*Spindles* – 1) &times; [(*%Reads* + *%Writes*) &divide; (*%Reads* + 4 &times; *%Writes*)] = *Total IOPS*

### Introducing SANs

Expanding the complexity of the storage subsystem, when a SAN is introduced into the environment, the basic principles outlined do not change, however I/O behavior for all of the systems connected to the SAN needs to be taken into account. As one of the major advantages in using a SAN is an additional amount of redundancy over internally or externally attached storage, capacity planning now needs to take into account fault tolerance needs. Also, more components are introduced that need to be evaluated. Breaking a SAN down into the component parts:

- SCSI or Fibre Channel hard drive
- Storage unit channel backplane
- Storage units
- Storage controller module
- SAN switch(es)
- HBA(s)
- The PCI bus

When designing any system for redundancy, additional components are included to accommodate the potential of failure. It is very important, when capacity planning, to exclude the redundant component from available resources. For example, if the SAN has two controller modules, the I/O capacity of one controller module is all that should be used for total I/O throughput available to the system. This is due to the fact that if one controller fails, the entire I/O load demanded by all connected systems will need to be processed by the remaining controller. As all capacity planning is done for peak usage periods, redundant components should not be factored into the available resources and planned peak utilization should not exceed 80% saturation of the system (in order to accommodate bursts or anomalous system behavior). Similarly, the redundant SAN switch, storage unit, and spindles should not be factored into the I/O calculations.

When analyzing the behavior of the SCSI or Fibre Channel hard drive, the method of analyzing the behavior as outlined previously does not change. Although there are certain advantages and disadvantages to each protocol, the limiting factor on a per disk basis is the mechanical limitation of the hard drive.

Analyzing the channel on the storage unit is exactly the same as calculating the resources available on the SCSI bus, or bandwidth (such as 20 MB/s) divided by block size (such as 8 KB). Where this deviates from the simple previous example is in the aggregation of multiple channels. For example, if there are 6 channels, each supporting 20 MB/s maximum transfer rate, the total amount of I/O and data transfer that is available is 100 MB/s (this is correct, it is not 120 MB/s). Again, fault tolerance is a major player in this calculation, in the event of the loss of an entire channel, the system is only left with 5 functioning channels. Thus, to ensure continuing to meet performance expectations in the event of failure, total throughput for all of the storage channels should not exceed 100 MB/s (this assumes load and fault tolerance is evenly distributed across all channels). Turning this into an I/O profile is dependent on the behavior of the application. In the case of Active Directory Jet I/O, this would correlate to approximately 12,500 I/O per second (100 MB/s &divide; 8 KB per I/O).

Next, obtaining the manufacturer’s specifications for the controller modules is required in order to gain an understanding of the throughput each module can support. In this example, the SAN has two controller modules that support 7,500 I/O each. The total throughput of the system may be 15,000 IOPS if redundancy is not desired. In calculating maximum throughput in the case of failure, the limitation is the throughput of one controller, or 7,500 IOPS. This threshold is well below the 12,500 IOPS (assuming 4 KB block size) maximum that can be supported by all of the storage channels, and thus, is currently the bottleneck in the analysis. Still for planning purposes, the desired maximum I/O to be planned for would be 10,400 I/O.

When the data exits the controller module, it transits a Fibre Channel connection rated at 1 GB/s (or 1 Gigabit per second). To correlate this with the other metrics, 1 GB/s turns into 128 MB/s (1 GB/s &divide; 8 bits/byte). As this is in excess of the total bandwidth across all channels in the storage unit (100 MB/s), this will not bottleneck the system. Additionally, as this is only one of the two channels (the additional 1 GB/s Fibre Channel connection being for redundancy), if one connection fails, the remaining connection still has enough capacity to handle all the data transfer demanded.

En route to the server, the data will most likely transit a SAN switch. As the SAN switch has to process the incoming I/O request and forward it out the appropriate port, the switch will have a limit to the amount of I/O that can be handled, however, manufacturers specifications will be required to determine what that limit is. For example, if there are two switches and each switch can handle 10,000 IOPS, the total throughput will be 20,000 IOPS. Again, fault tolerance being a concern, if one switch fails, the total throughput of the system will be 10,000 IOPS. As it is desired not to exceed 80% utilization in normal operation, using no more than 8000 I/O should be the target.

Finally, the HBA installed in the server would also have a limit to the amount of I/O that it can handle. Usually, a second HBA is installed for redundancy, but just like with the SAN switch, when calculating maximum I/O that can be handled, the total throughput of *N* &ndash; 1 HBAs is what the maximum scalability of the system is.

### Caching considerations

Caches are one of the components that can significantly impact the overall performance at any point in the storage system. Detailed analysis about caching algorithms is beyond the scope of this article; however, some basic statements about caching on disk subsystems are worth illuminating:

- Caching does improved sustained sequential write I/O as it can buffer many smaller write operations into larger I/O blocks and de-stage to storage in fewer, but larger block sizes. This will reduce total random I/O and total sequential I/O, thus providing more resource availability for other I/O.
- Caching does not improve sustained write I/O throughput of the storage subsystem. It only allows for the writes to be buffered until the spindles are available to commit the data. When all the available I/O of the spindles in the storage subsystem is saturated for long periods, the cache will eventually fill up. In order to empty the cache, enough time between bursts, or extra spindles, need to be allotted in order to provide enough I/O to allow the cache to flush.

  Larger caches only allow for more data to be buffered. This means longer periods of saturation can be accommodated.

  In a normally operating storage subsystem, the operating system will experience improved write performance as the data only needs to be written to cache. Once the underlying media is saturated with I/O, the cache will fill and write performance will return to disk speed.

- When caching read I/O, the scenario where the cache is most advantageous is when the data is stored sequentially on the disk, and the cache can read-ahead (it makes the assumption that the next sector contains the data that will be requested next).
- When read I/O is random, caching at the drive controller is unlikely to provide any enhancement to the amount of data that can be read from the disk. Any enhancement is non-existent if the operating system or application-based cache size is greater than the hardware-based cache size.

  In the case of Active Directory, the cache is only limited by the amount of RAM.

### SSD considerations

SSDs are a completely different animal than spindle-based hard disks. Yet the two key criteria remain: “How many IOPS can it handle?” and “What is the latency for those IOPS?” In comparison to spindle-based hard disks, SSDs can handle higher volumes of I/O and can have lower latencies. In general and as of this writing, while SSDs are still expensive in a cost-per-Gigabyte comparison, they are very cheap in terms of cost-per-I/O and deserve significant consideration in terms of storage performance.

Considerations:

- Both IOPS and latencies are very subjective to the manufacturer designs and in some cases have been observed to be poorer performing than spindle based technologies. In short, it is more important to review and validate the manufacturer specs drive by drive and not assume any generalities.
- IOPS types can have very different numbers depending on whether it is read or write. AD DS services, in general, being predominantly read-based, will be less affected than some other application scenarios.
- “Write endurance” – this is the concept that SSD cells will eventually wear out. Various manufacturers deal with this challenge different fashions. At least for the database drive, the predominantly read I/O profile allows for downplaying the significance of this concern as the data is not highly volatile.

### Summary

One way to think about storage is picturing household plumbing. Imagine the IOPS of the media that the data is stored on is the household main drain. When this is clogged (such as roots in the pipe) or limited (it is collapsed or too small), all the sinks in the household back up when too much water is being used (too many guests). This is perfectly analogous to a shared environment where one or more systems are leveraging shared storage on an SAN/NAS/iSCSI with the same underlying media. Different approaches can be taken to resolve the different scenarios:

- A collapsed or undersized drain requires a full scale replacement and fix. This would be similar to adding in new hardware or redistributing the systems using the shared storage throughout the infrastructure.
- A “clogged” pipe usually means identification of one or more offending problems and removal of those problems. In a storage scenario this could be storage or system level backups, synchronized antivirus scans across all servers, and synchronized defragmentation software running during peak periods.

In any plumbing design, multiple drains feed into the main drain. If anything stops up one of those drains or a junction point, only the things behind that junction point back up. In a storage scenario, this could be an overloaded switch (SAN/NAS/iSCSI scenario), driver compatibility issues (wrong driver/HBA Firmware/storport.sys combination), or backup/antivirus/defragmentation. To determine if the storage “pipe” is big enough, IOPS and I/O size needs to be measured. At each joint add them together to ensure adequate “pipe diameter.”

## Appendix D - Discussion on storage troubleshooting - Environments where providing at least as much RAM as the database size is not a viable option

It is helpful to understand why these recommendations exist so that the changes in storage technology can be accommodated. These recommendations exist for two reasons. The first is isolation of IO, such that performance issues (that is, paging) on the operating system spindle do not impact performance of the database and I/O profiles. The second is that log files for AD DS (and most databases) are sequential in nature, and spindle-based hard drives and caches have a huge performance benefit when used with sequential I/O as compared to the more random I/O patterns of the operating system and almost purely random I/O patterns of the AD DS database drive. By isolating the sequential I/O to a separate physical drive, throughput can be increased. The challenge presented by today’s storage options is that the fundamental assumptions behind these recommendations are no longer true. In many virtualized storage scenarios, such as iSCSI, SAN, NAS, and Virtual Disk image files, the underlying storage media is shared across multiple hosts, thus completely negating both the “isolation of IO” and the “sequential I/O optimization” aspects. In fact these scenarios add an additional layer of complexity in that other hosts accessing the shared media can degrade responsiveness to the domain controller.

In planning storage performance, there are three categories to consider: cold cache state, warmed cache state, and backup/restore. The cold cache state occurs in scenarios such as when the domain controller is initially rebooted or the Active Directory service is restarted and there is no Active Directory data in RAM. Warm cache state is where the domain controller is in a steady state and the database is cached. These are important to note as they will drive very different performance profiles, and having enough RAM to cache the entire database does not help performance when the cache is cold. One can consider performance design for these two scenarios with the following analogy, warming the cold cache is a “sprint” and running a server with a warm cache is a “marathon.”

For both the cold cache and warm cache scenario, the question becomes how fast the storage can move the data from disk into memory. Warming the cache is a scenario where, over time, performance improves as more queries reuse data, the cache hit rate increases, and the frequency of needing to go to disk decreases. As a result the adverse performance impact of going to disk decreases. Any degradation in performance is only transient while waiting for the cache to warm and grow to the maximum, system-dependent allowed size. The conversation can be simplified to how quickly the data can be gotten off of disk, and is a simple measure of the IOPS available to Active Directory, which is subjective to IOPS available from the underlying storage. From a planning perspective, because warming the cache and backup/restore scenarios happen on an exceptional basis, normally occur off hours, and are subjective to the load of the DC, general recommendations do not exist except in that these activities be scheduled for non-peak hours.

AD DS, in most scenarios, is predominantly read IO, usually a ratio of 90% read/10% write. Read I/O often tends to be the bottleneck for user experience, and with write IO, causes write performance to degrade. As I/O to the NTDS.dit is predominantly random, caches tend to provide minimal benefit to read IO, making it that much more important to configure the storage for read I/O profile correctly.

For normal operating conditions, the storage planning goal is minimize the wait times for a request from AD DS to be returned from disk. This essentially means that the number of outstanding and pending I/O is less than or equivalent to the number of pathways to the disk. There are a variety of ways to measure this. In a performance monitoring scenario, the general recommendation is that LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read be less than 20 ms. The desired operating threshold must be much lower, preferably as close to the speed of the storage as possible, in the 2 to 6 millisecond (.002 to .006 second) range depending on the type of storage.

Example:

![Storage latency chart](media/capacity-planning-considerations-storage-latency.png)

Analyzing the chart:

- **Green oval on the left –** The latency remains consistent at 10 ms. The load increases from 800 IOPS to 2400 IOPS. This is the absolute floor to how quickly an I/O request can be processed by the underlying storage. This is subject to the specifics of the storage solution.
- **Burgundy oval on the right –** The throughput remains flat from the exit of the green circle through to the end of the data collection while the latency continues to increase. This is demonstrating that when the request volumes exceed the physical limitations of the underlying storage, the longer the requests spend sitting in the queue waiting to be sent out to the storage subsystem.

Applying this knowledge:

- **Impact to a user querying membership of a large group –** Assume this requires reading 1 MB of data from the disk, the amount of I/O and how long it takes can be evaluated as follows:
  - Active Directory database pages are 8 KB in size. 
  - A minimum of 128 pages need to be read in from disk. 
  - Assuming nothing is cached, at the floor (10 ms) this is going to take a minimum 1.28 seconds to load the data from disk in order to return it to the client. At 20 ms, where the throughput on storage has long since maxed out and is also the recommended maximum, it will take 2.5 seconds to get the data from disk in order to return it to the end user.  
- **At what rate will the cache be warmed –** Making the assumption that the client load is going to maximize the throughput on this storage example, the cache will warm at a rate of 2400 IOPS &times; 8 KB per IO. Or, approximately 20 MB/s per second, loading about 1 GB of database into RAM every 53 seconds.

> [!NOTE]
> It is normal for short periods to observe the latencies climb when components aggressively read or write to disk, such as when the system is being backed up or when AD DS is running garbage collection. Additional head room on top of the calculations should be provided to accommodate these periodic events. The goal being to provide enough throughput to accommodate these scenarios without impacting normal function.

As can be seen, there is a physical limit based on the storage design to how quickly the cache can possibly warm. What will warm the cache are incoming client requests up to the rate that the underlying storage can provide. Running scripts to “pre-warm” the cache during peak hours will provide competition to load driven by real client requests. That can adversely affect delivering data that clients need first because, by design, it will generate competition for scarce disk resources as artificial attempts to warm the cache will load data that is not relevant to the clients contacting the DC.Processeur Information(_Total)\\' utilitaire de processeur Domain Services
description: Detailed discussion of the factors to consider during capacity planning for AD DS.
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; kenbrunf
author: Teresa-Motiv
ms.date: 7/3/2019
---

# Capacity planning for Active Directory Domain Services

This topic is originally written by Ken Brumfield, Senior Premier Field Engineer at Microsoft, and provides recommendations for capacity planning for Active Directory Domain Services (AD DS).

## Goals of capacity planning

Capacity planning is not the same as troubleshooting performance incidents. They are closely related, but quite different. The goals of capacity planning are:  

- Properly implement and operate an environment 
- Minimize the time spent troubleshooting performance issues.  
  
In capacity planning, an organization might have a baseline target of 40% processor utilization during peak periods in order to meet client performance requirements and accommodate the time necessary to upgrade the hardware in the datacenter. Whereas, to be notified of abnormal performance incidents, a monitoring alert threshold might be set at 90% over a 5 minute interval.

The difference is that when a capacity management threshold is continually exceeded (a one-time event is not a concern), adding capacity (that is, adding in more or faster processors) would be a solution or scaling the service across multiple servers would be a solution. Performance alert thresholds indicate that client experience is currently suffering and immediate steps are needed to address the issue.

As an analogy: capacity management is about preventing a car accident (defensive driving, making sure the brakes are working properly, and so on) whereas performance troubleshooting is what the police, fire department, and emergency medical professionals do after an accident. This is about “defensive driving,” Active Directory-style.

Over the last several years, capacity planning guidance for scale-up systems has changed dramatically. The following changes in system architectures have challenged fundamental assumptions about designing and scaling a service:

- 64-bit server platforms  
- Virtualization  
- Increased attention to power consumption  
- SSD storage  
- Cloud scenarios  

Additionally, the approach is shifting from a server-based capacity planning exercise to a service-based capacity planning exercise. Active Directory Domain Services (AD DS), a mature distributed service that many Microsoft and third-party products use as a backend, becomes one the most critical products to plan correctly to ensure the necessary capacity for other applications to run.

### Baseline requirements for capacity planning guidance

Throughout this article, the following baseline requirements are expected:

- Readers have read and are familiar with [Performance Tuning Guidelines for Windows Server 2012 R2](https://docs.microsoft.com/previous-versions//dn529133(v=vs.85)).
- The Windows Server platform is an x64 based architecture. But even if your Active Directory environment is installed on Windows Server 2003 x86 (now beyond the end of the support lifecycle) and has a directory information tree (DIT) that is less 1.5 GB in size and that can easily be held in memory, the guidelines from this article are still applicable.
- Capacity planning is a continuous process and you should regularly review how well the environment is meeting expectations.
- Optimization will occur over multiple hardware lifecycles as hardware costs change. For example, memory becomes cheaper, the cost per core decreases, or the price of different storage options change.
- Plan for the peak busy period of the day. It is recommended to look at this in either 30 minute or hour intervals. Anything greater may hide the actual peaks and anything less may be distorted by “transient spikes.”
- Plan for growth over the course of the hardware lifecycle for the enterprise. This may include a strategy of upgrading or adding hardware in a staggered fashion, or a complete refresh every three to five years. Each will require a “guess” as how much the load on Active Directory will grow. Historical data, if collected, will help with this assessment. 
- Plan for fault tolerance. Once an estimate *N* is derived, plan for scenarios that include *N* &ndash; 1, *N* &ndash; 2, *N* &ndash; *x*.
  - Add in additional servers according to organizational need to ensure that the loss of a single or multiple servers does not exceed maximum peak capacity estimates.
  - Also consider that the growth plan and fault tolerance plan need to be integrated. For example, if one DC is required to support the load, but the estimate is that the load will be doubled in the next year and require two DCs total, there will not be enough capacity to support fault tolerance. The solution would be to start with three DCs. One could also plan to add the third DC after 3 or 6 months if budgets are tight.

    >[!NOTE]
    >Adding Active Directory-aware applications might have a noticeable impact on the DC load, whether the load is coming from the application servers or clients.

### Three-step process for the capacity planning cycle

In capacity planning, first decide what quality of service is needed. For example, a core datacenter supports a higher level of concurrency and requires more consistent experience for users and consuming applications, which requires greater attention to redundancy and minimizing system and infrastructure bottlenecks. In contrast, a satellite location with a handful of users does not need the same level of concurrency or fault tolerance. Thus, the satellite office might not need as much attention to optimizing the underlying hardware and infrastructure, which may lead to cost savings. All recommendations and guidance herein are for optimal performance, and can be selectively relaxed for scenarios with less demanding requirements.

The next question is: virtualized or physical? From a capacity planning perspective, there is no right or wrong answer; there is only a different set of variables to work with. Virtualization scenarios come down to one of two options:

- “Direct mapping” with one guest per host (where virtualization exists solely to abstract the physical hardware from the server)
- “Shared host”

Testing and production scenarios indicate that the “direct mapping” scenario can be treated identically to a physical host. “Shared host,” however, introduces a number of considerations spelled out in more detail later. The “shared host” scenario means that AD DS is also competing for resources, and there are penalties and tuning considerations for doing so.

With these considerations in mind, the capacity planning cycle is an iterative three-step process:

1. Measure the existing environment, determine where the system bottlenecks currently are, and get environmental basics necessary to plan the amount of capacity needed.
1. Determine the hardware needed according to the criteria outlined in step 1.
1. Monitor and validate that the infrastructure as implemented is operating within specifications. Some data collected in this step becomes the baseline for the next cycle of capacity planning.

### Applying the process

To optimize performance, ensure these major components are correctly selected and tuned to the application loads:

1. Memory
1. Network
1. Storage
1. Processor
1. Net Logon

The basic storage requirements of AD DS and the general behavior of well written client software allow environments with as many as 10,000 to 20,000 users to forego heavy investment in capacity planning with regards to physical hardware, as almost any modern server class system will handle the load. That said, the following table summarizes how to evaluate an existing environment in order to select the right hardware. Each component is analyzed in detail in subsequent sections to help AD DS administrators evaluate their infrastructure using baseline recommendations and environment-specific principals.

In general:

- Any sizing based on current data will only be accurate for the current environment.
- For any estimates, expect demand to grow over the lifecycle of the hardware.
- Determine whether to oversize today and grow into the larger environment, or add capacity over the lifecycle.
- For virtualization, all the same capacity planning principals and methodologies apply, except that the overhead of the virtualization needs to be added to anything domain related.
- Capacity planning, like anything that attempts to predict, is NOT an accurate science. Do not expect things to calculate perfectly and with 100% accuracy. The guidance here is the leanest recommendation; add in capacity for additional safety and continuously validate that the environment remains on target.

### Data collection summary tables

#### New environment

| Component | Estimates |
|-|-|
|Storage/Database Size|40 KB to 60 KB for each user|
|RAM|Database Size<br />Base operating system recommendations<br />Third-party applications|
|Network|1 GB|
|CPU|1000 concurrent users for each core|

#### High-level evaluation criteria

| Component | Evaluation criteria | Planning considerations |
|-|-|-|
|Storage/Database size|The section entitled “To activate logging of disk space that is freed by defragmentation” in [Storage Limits](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10))| |
|Storage/ Database performance|<ul><li>"LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read," "LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Write," "LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Transfer"</li><li>"LogicalDisk(*\<NTDS Database Drive\>*)\Reads/sec," "LogicalDisk(*\<NTDS Database Drive\>*)\Writes/sec," "LogicalDisk(*\<NTDS Database Drive\>*)\Transfers/sec"</li></ul>|<ul><li>Storage has two concerns to address<ul><li>Space available, which with the size of today’s spindle based and SSD based storage is irrelevant for most AD environments.</li> <li>Input/Output (IO) Operations available – In many environments, this is often overlooked. But it is important to evaluate only environments where there is not enough RAM to load the entire NTDS Database into memory.</li></ul><li>Storage can be a complex topic and should involve hardware vendor expertise for proper sizing. Particularly with more complex scenarios such as SAN, NAS, and iSCSI scenarios. However, in general, cost per Gigabyte of storage is often in direct opposition to cost per IO:<ul><li>RAID 5 has lower cost per Gigabyte than Raid 1, but Raid 1 has lower cost per IO</li><li>Spindle-based hard drives have lower cost per Gigabyte, but SSDs have a lower cost per IO</li></ul><li>After a restart of the computer or the Active Directory Domain Services service, the Extensible Storage Engine (ESE) cache is empty and performance will be disk bound while the cache warms.</li><li>In most environments AD is read intensive I/O in a random pattern to disks, negating much of the benefit of caching and read optimization strategies.  Plus, AD has a way larger cache in memory than most storage system caches.</li></ul>
|RAM|<ul><li>Database size</li><li>Base operating system recommendations</li><li>Third-party applications</li></ul>|<ul><li>Storage is the slowest component in a computer. The more that can be resident in RAM, the less it is necessary to go to disk.</li><li>Ensure enough RAM is allocated to store the operating system, Agents (antivirus, backup, monitoring), NTDS Database and growth over time.</li><li>For environments where maximizing the amount of RAM is not cost effective (such as a satellite locations) or not feasible (DIT is too large), reference the Storage section to ensure that storage is properly sized.</li></ul>|
|Network|<ul><li>“Network Interface(\*)\Bytes Received/sec”</li><li>“Network Interface(\*)\Bytes Sent/sec”|<ul><li>In general, traffic sent from a DC far exceeds traffic sent to a DC.</li><li>As a switched Ethernet connection is full-duplex, inbound and outbound network traffic need to be sized independently.</li><li>Consolidating the number of DCs will increase the amount of bandwidth used to send responses back to client requests for each DC, but will be close enough to linear for the site as a whole.</li><li>If removing satellite location DCs, don’t forget to add the bandwidth for the satellite DC into the hub DCs as well as use that to evaluate how much WAN traffic there will be.</li></ul>|
|CPU|<ul><li>“Logical Disk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read”</li><li>“Process(lsass)\\% Processor Time”</li></ul>|<ul><li>After eliminating storage as a bottleneck, address the amount of compute power needed.</li><li>While not perfectly linear, the number of processor cores consumed across all servers within a specific scope (such as a site) can be used to gauge how many processors are necessary to support the total client load. Add the minimum necessary to maintain the current level of service across all the systems within the scope.</li><li>Changes in processor speed, including power management related changes, impact numbers derived from the current environment. Generally, it is impossible to precisely evaluate how going from a 2.5 GHz processor to a 3 GHz processor will reduce the number of CPUs needed.</li></ul>|
|NetLogon|<ul><li>“Netlogon(\*)\Semaphore Acquires”</li><li>“Netlogon(\*)\Semaphore Timeouts”</li><li>“Netlogon(\*)\Average Semaphore Hold Time”</li></ul>|<ul><li>Net Logon Secure Channel/MaxConcurrentAPI only affects environments with NTLM authentications and/or PAC Validation. PAC Validation is on by default in operating system versions before Windows Server 2008. This is a client setting, so the DCs will be impacted until this is turned off on all client systems.</li><li>Environments with significant cross trust authentication, which includes intra-forest trusts, have greater risk if not sized properly.</li><li>Server consolidations will increase concurrency of cross-trust authentication.</li><li>Surges need to be accommodated, such as cluster fail-overs, as users re-authenticate en masse to the new cluster node.</li><li>Individual client systems (such as a cluster) might need tuning too.</li></ul>|

## Planning

For a long time, the community’s recommendation for sizing AD DS has been to “put in as much RAM as the database size.” For the most part, that recommendation is all that most environments needed to be concerned about. But the ecosystem consuming AD DS has gotten much bigger, as have the AD DS environments themselves, since its introduction in 1999. Although the increase in compute power and the switch from x86 architectures to x64 architectures has made the subtler aspects of sizing for performance irrelevant to a larger set of customers running AD DS on physical hardware, the growth of virtualization has reintroduced the tuning concerns to a larger audience than before.

The following guidance is thus about how to determine and plan for the demands of Active Directory as a service regardless of whether it is deployed in a physical, a virtual/physical mix, or a purely virtualized scenario. As such, we will break down the evaluation to each of the four main components: storage, memory, network, and processor. In short, in order to maximize performance on AD DS, the goal is to get as close to processor bound as possible.

## RAM

Simply, the more that can be cached in RAM, the less it is necessary to go to disk. To maximize the scalability of the server the minimum amount of RAM should be the sum of the current database size, the total SYSVOL size, the operating system recommended amount, and the vendor recommendations for the agents (antivirus, monitoring, backup, and so on). An additional amount should be added to accommodate growth over the lifetime of the server. This will be environmentally subjective based on estimates of database growth based on environmental changes.

For environments where maximizing the amount of RAM is not cost effective (such as a satellite locations) or not feasible (DIT is too large), reference the Storage  section to ensure that storage is properly designed.

A corollary that comes up in the general context in sizing memory is sizing of the page file. In the same context as everything else memory related, the goal is to minimize going to the much slower disk. Thus the question should go from, “how should the page file be sized?” to “how much RAM is needed to minimize paging?” The answer to the latter question is outlined in the rest of this section. This leaves most of the discussion for sizing the page file to the realm of general operating system recommendations and the need to configure the system for memory dumps, which are unrelated to AD DS performance.

### Evaluating

The amount of RAM that a domain controller (DC) needs is actually a complex exercise for these reasons:

- High potential for error when trying to use an existing system to gauge how much RAM is needed as LSASS will trim under memory pressure conditions, artificially deflating the need.
- The subjective fact that an individual DC only needs to cache what is “interesting” to its clients. This means that the data that needs to be cached on a DC in a site with only an Exchange server will be very different than the data that needs to be cached on a DC that only authenticates users.
- The labor to evaluate RAM for each DC on a case-by-case basis is prohibitive and changes as the environment changes.
- The criteria behind the recommendation will help to make informed decisions: 
- The more that can be cached in RAM, the less it is necessary to go to disk. 
- Storage is by far the slowest component of a computer. Access to data on spindle-based and SSD storage media is on the order of 1,000,000x slower than access to data in RAM.

Thus, in order to maximize the scalability of the server, the minimum amount of RAM is the sum of the current database size, the total SYSVOL size, the operating system recommended amount, and the vendor recommendations for the agents (antivirus, monitoring, backup, and so on). Add additional amounts to accommodate growth over the lifetime of the server. This will be environmentally subjective based on estimates of database growth. However, for satellite locations with a small set of end users, these requirements can be relaxed as these sites will not need to cache as much to service most of the requests.

For environments where maximizing the amount of RAM is not cost effective (such as a satellite locations) or not feasible (DIT is too large), reference the Storage section to ensure that storage is properly sized.

> [!NOTE]
> A corollary while sizing memory is sizing of the page file. Because the goal is to minimize going to the much slower disk, the question goes from “how should the page file be sized?” to “how much RAM is needed to minimize paging?” The answer to the latter question is outlined in the rest of this section. This leaves most of the discussion for sizing the page file to the realm of general operating system recommendations and the need to configure the system for memory dumps, which are unrelated to AD DS performance.

### Virtualization considerations for RAM

Avoid memory over-commit at the host. The fundamental goal behind optimizing the amount of RAM is to minimize the amount of time spent going to disk. In virtualization scenarios, the concept of memory over-commit exists where more RAM is allocated to the guests then exists on the physical machine. This in and of itself is not a problem. It becomes a problem when the total memory actively used by all the guests exceeds the amount of RAM on the host and the underlying host starts paging. Performance becomes disk-bound in cases where the domain controller is going to the NTDS.dit to get data, or the domain controller is going to the page file to get data, or the host is going to disk to get data that the guest thinks is in RAM.

### Calculation summary example

|Component|Estimated memory (example)|
|-|-|
|Base operating system recommended RAM (Windows Server 2008)|2 GB|
|LSASS internal tasks|200 MB|
|Monitoring agent|100 MB|
|Antivirus|100 MB|
|Database (Global Catalog)|8.5 GB are you sure ???|
|Cushion for backup to run, administrators to log on without impact|1 GB|
|Total|12 GB|

**Recommended: 16 GB**

Over time, the assumption can be made that more data will be added to the database and the server will probably be in production for 3 to 5 years. Based on an estimate of growth of 33%, 16 GB would be a reasonable amount of RAM to put in a physical server. In a virtual machine, given the ease with which settings can be modified and RAM can be added to the VM, starting at 12 GB with the plan to monitor and upgrade in the future is reasonable.

## Network

### Evaluating
This section is less about evaluating the demands regarding replication traffic, which is focused on traffic traversing the WAN and is thoroughly covered in [Active Directory Replication Traffic](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/bb742457(v=technet.10)), than it is about evaluating total bandwidth and network capacity needed, inclusive of client queries, Group Policy applications, and so on. For existing environments, this can be collected by using performance counters “Network Interface(\*)\Bytes Received/sec,” and “Network Interface(\*)\Bytes Sent/sec.” Sample intervals for Network Interface counters in either 15, 30, or 60 minutes. Anything less will generally be too volatile for good measurements; anything greater will smooth out daily peeks excessively.

> [!NOTE]
> Generally, the majority of network traffic on a DC is outbound as the DC responds to client queries. This is the reason for the focus on outbound traffic, though it is recommended to evaluate each environment for inbound traffic also. The same approaches can be used to address and review inbound network traffic requirements. For more information, see Knowledge Base article [929851: The default dynamic port range for TCP/IP has changed in Windows Vista and in Windows Server 2008](http://support.microsoft.com/kb/929851).

### Bandwidth needs

Planning for network scalability covers two distinct categories: the amount of traffic and the CPU load from the network traffic. Each of these scenarios is straight-forward compared to some of the other topics in this article.

In evaluating how much traffic must be supported, there are two unique categories of capacity planning for AD DS in terms of network traffic. The first is replication traffic that traverses between domain controllers and is covered thoroughly in the reference [Active Directory Replication Traffic](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/bb742457(v=technet.10)) and is still relevant to current versions of AD DS. The second is the intrasite client-to-server traffic. One of the simpler scenarios to plan for, intrasite traffic predominantly receives small requests from clients relative to the large amounts of data sent back to the clients. 100 MB will generally be adequate in environments up to 5,000 users per server, in a site. Using a 1 GB network adapter and Receive Side Scaling (RSS) support is recommended for anything above 5,000 users. To validate this scenario, particularly in the case of server consolidation scenarios, look at Network Interface(\*)\Bytes/sec across all the DCs in a site, add them together, and divide by the target number of domain controllers to ensure that there is adequate capacity. The easiest way to do this is to use the “Stacked Area” view in Windows Reliability and Performance Monitor (formerly known as Perfmon), making sure all of the counters are scaled the same.

Consider the following example (also known as, a really, really complex way to validate that the general rule is applicable to a specific environment). The following assumptions are made:

- The goal is to reduce the footprint to as few servers as possible. Ideally, one server will carry the load and an additional server is deployed for redundancy (*N* + 1 scenario). 
- In this scenario, the current network adapter supports only 100 MB and is in a switched environment.  
  The maximum target network bandwidth utilization is 60% in an N scenario (loss of a DC).
- Each server has about 10,000 clients connected to it.

Knowledge gained from the data in the chart (Network Interface(\*)\Bytes Sent/sec):

1. The business day starts ramping up around 5:30 and winds down at 7:00 PM.
1. The peak busiest period is from 8:00 AM to 8:15 AM, with greater than 25 Bytes sent/sec on the busiest DC.  
   > [!NOTE]
   > All performance data is historical. So the peak data point at 8:15 indicates the load from 8:00 to 8:15.
1. There are spikes before 4:00 AM, with more than 20 Bytes sent/sec on the busiest DC, which could indicate either load from different time zones or background infrastructure activity, such as backups. Since the peak at 8:00 AM exceeds this activity, it is not relevant.
1. There are five Domain Controllers in the site.
1. The max load is about 5.5 MB/s per DC, which represents 44% of the 100 MB connection. Using this data, it can be estimated that the total bandwidth needed between 8:00 AM and 8:15 AM is 28 MB/s.
   >[!NOTE]
   >Be careful with the fact that Network Interface sent/receive counters are in bytes and network bandwidth is measured in bits. 100 MB &divide; 8 = 12.5 MB, 1 GB &divide; 8 = 128 MB.
  
Conclusions:

1. This current environment does meet the N+1 level of fault tolerance at 60% target utilization. Taking one system offline will shift the bandwidth per server from about 5.5 MB/s (44%) to about 7 MB/s (56%).
1. Based on the previously stated goal of consolidating to one server, this both exceeds the maximum target utilization and theoretically the possible utilization of a 100 MB connection.
1. With a 1 GB connection this will represent 22% of the total capacity.
1. Under normal operating conditions in the *N* + 1 scenario, client load will be relatively evenly distributed at about 14 MB/s per server or 11% of total capacity.
1. To ensure that capacity is adequate during unavailability of a DC, the normal operating targets per server would be about 30% network utilization or 38 MB/s per server. Failover targets would be 60% network utilization or 72 MB/s per server.  
  
In short, the final deployment of systems must have a 1 GB network adapter and be connected to a network infrastructure that will support said load. A further note is that given the amount of network traffic generated, the CPU load from network communications can have a significant impact and limit the maximum scalability of AD DS. This same process can be used to estimate the amount of inbound communication to the DC. But given the predominance of outbound traffic relative to inbound traffic, it is an academic exercise for most environments. Ensuring hardware support for RSS is important in environments with greater than 5,000 users per server. For scenarios with high network traffic, balancing of interrupt load can be a bottleneck. This can be detected by Processor(\*)\% Interrupt Time being unevenly distributed across CPUs. RSS enabled NICs can mitigate this limitation and increase scalability.

> [!NOTE]
> A similar approach can be used to estimate the additional capacity necessary when consolidating data centers, or retiring a domain controller in a satellite location. Simply collect the outbound and inbound traffic to clients and that will be the amount of traffic that will now be present on the WAN links.  
>  
> In some cases, you might experience more traffic than expected because traffic is slower, such as when certificate checking fails to meet aggressive time-outs on the WAN. For this reason, WAN sizing and utilization should be an iterative, ongoing process.

### Virtualization considerations for network bandwidth

It is easy to make recommendations for a physical server: 1 GB for servers supporting greater than 5000 users. Once multiple guests start sharing an underlying virtual switch infrastructure, additional attention is necessary to ensure that the host has adequate network bandwidth to support all the guests on the system, and thus requires the additional rigor. This is nothing more than an extension of ensuring the network infrastructure into the host machine. This is regardless whether the network is inclusive of the domain controller running as a virtual machine guest on a host with the network traffic going over a virtual switch, or whether connected directly to a physical switch. The virtual switch is just one more component where the uplink needs to support the amount of data being transmitted. Thus the physical host physical network adapter linked to the switch should be able to support the DC load plus all other guests sharing the virtual switch connected to the physical network adapter.

### Calculation summary example

|System|Peak bandwidth|
|-|-|
DC 1|6.5 MB/s|
DC 2|6.25 MB/s|
|DC 3|6.25 MB/s|
|DC 4|5.75 MB/s|
|DC 5|4.75 MB/s|
|Total|28.5 MB/s|

**Recommended: 72 MB/s** (28.5 MB/s divided by 40%)

|Target system(s) count|Total bandwidth (from above)|
|-|-|
|2|28.5 MB/s|
|Resulting normal behavior|28.5 &divide; 2 = 14.25 MB/s|

As always, over time the assumption can be made that client load will increase and this growth should be planned for as best as possible. The recommended amount to plan for would allow for an estimated growth in network traffic of 50%.

## Storage

Planning storage constitutes two components:

- Capacity, or storage size
- Performance

A great amount of time and documentation is spent on planning capacity, leaving performance often completely overlooked. With current hardware costs, most environments are not large enough that either of these is actually a concern, and the recommendation to “put in as much RAM as the database size” usually covers the rest, though it may be overkill for satellite locations in larger environments.

### Sizing

#### Evaluating for storage

Compared to 13 years ago when Active Directory was introduced, a time when 4 GB and 9 GB drives were the most common drive sizes, sizing for Active Directory is not even a consideration for all but the largest environments. With the smallest available hard drive sizes in the 180 GB range, the entire operating system, SYSVOL, and NTDS.dit can easily fit on one drive. As such, it is recommended to deprecate heavy investment in this area.

The only recommendation for consideration is to ensure that 110% of the NTDS.dit size is available in order to enable defrag. Additionally, accommodations for growth over the life of the hardware should be made.

The first and most important consideration is evaluating how large the NTDS.dit and SYSVOL will be. These measurements will lead into sizing both fixed disk and RAM allocation. Due to the (relatively) low cost of these components, the math does not need to be rigorous and precise. Content about how to evaluate this for both existing and new environments can be found in the [Data Storage](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961771(v=technet.10)) series of articles. Specifically, refer to the following articles:

- **For existing environments &ndash;** The section titled “To activate logging of disk space that is freed by defragmentation” in the article [Storage Limits](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10)).
- **For new environments &ndash;** The article titled [Growth Estimates for Active Directory Users and Organizational Units](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc961779(v=technet.10)).

  > [!NOTE]
  > The articles are based on data size estimates made at the time of the release of Active Directory in Windows 2000. Use object sizes that reflect the actual size of objects in your environment.

When reviewing existing environments with multiple domains, there may be variations in database sizes. Where this is true, use the smallest global catalog (GC) and non-GC sizes.  

The database size can vary between operating system versions. DCs that run earlier operating systems such as Windows Server 2003 has a smaller database size than a DC that runs a later operating system such as Windows Server 2008 R2, especially when features such Active Directory Recycle Bin or Credential Roaming are enabled.

> [!NOTE]  
  >
>- For new environments, notice that the estimates in Growth Estimates for Active Directory Users and Organizational Units indicate that 100,000 users (in the same domain) consume about 450 MB of space. Please note that the attributes populated can have a huge impact on the total amount. Attributes will be populated on many objects by both third-party and Microsoft products, including Microsoft Exchange Server and Lync. An evaluation based on the portfolio of the products in the environment is preferred, but the exercise of detailing out the math and testing for precise estimates for all but the largest environments may not actually be worth significant time and effort.
>- Ensure that 110% of the NTDS.dit size is available as free space in order to enable offline defrag, and plan for growth over a three to five year hardware lifespan. Given how cheap storage is, estimating storage at 300% the size of the DIT as storage allocation is safe to accommodate growth and the potential need for offline defrag.

#### Virtualization considerations for storage

In a scenario where multiple Virtual Hard Disk (VHD) files are being allocated on a single volume use a fixed sized disk of at least 210% (100% of the DIT + 110% free space) the size of the DIT to ensure that there is adequate space reserved.  

#### Calculation summary example

|Data collected from evaluation phase| |
|-|-|
|NTDS.dit size|35 GB|
|Modifier to allow for offline defrag|2.1|
|Total storage needed|73.5 GB|

> [!NOTE]
> This storage needed is in addition to the storage needed for SYSVOL, operating system, page file, temporary files, local cached data (such as installer files), and applications.

### Storage performance

#### Evaluating performance of storage

As the slowest component within any computer, storage can have the biggest adverse impact on client experience. For those environments large enough for which the RAM sizing recommendations are not feasible, the consequences of overlooking planning storage for performance can be devastating.  Also, the complexities and varieties of storage technology further increase the risk of failure as the relevance of long standing best practices of “put operating system, logs, and database” on separate physical disks is limited in it’s useful scenarios.  This is because the long standing best practice is based on the assumption that is that a “disk” is a dedicated spindle and this allowed I/O to be isolated.  This assumptions that make this true are no longer relevant with the introduction of:

- New storage types and virtualized and shared storage scenarios
- Shared spindles on a Storage Area Network (SAN)
- VHD file on a SAN or network-attached storage
- Solid State Drives
- Tiered storage architectures (i.e. SSD storage tier caching larger spindle based storage)

In short, the end goal of all storage performance efforts, regardless of underlying storage architecture and design, is to ensure the needed amount of Input/Output Operations per Second (IOPS) are available and that those IOPS happen within an acceptable time frame. This section covers how to evaluate what AD DS demands of the underlying storage in order to ensure storage solutions are properly designed.  Given the variability of today’s storage technologies, it is best to work with the storage vendors to ensure adequate IOPS.  For those scenarios with locally attached storage, reference the Appendix C for the basics in how to design traditional local storage scenarios.  This principals are generally applicable to more complex storage tiers and will also help in dialog with the vendors supporting backend storage solutions.

- Given the wide breadth of storage options available, it is recommended to engage the expertise of hardware support teams or vendors to ensure that the specific solution meets the needs of AD DS. The following numbers are the information that would be provided to the storage specialists.

For environments where the database is too large to be held in RAM, use the performance counters to determine how much I/O needs to be supported:

- LogicalDisk(\*)\Avg Disk sec/Read (for example, if NTDS.dit is stored on the D:/ drive, the full path would be LogicalDisk(D:)\Avg Disk sec/Read)
- LogicalDisk(\*)\Avg Disk sec/Write
- LogicalDisk(\*)\Avg Disk sec/Transfer
- LogicalDisk(\*)\Reads/sec
- LogicalDisk(\*)\Writes/sec
- LogicalDisk(\*)\Transfers/sec

These should be sampled in 15/30/60 minute intervals to benchmark the demands of the current environment.

#### Evaluating the results

> [!NOTE]
> The focus is on reads from the database as this is usually the most demanding component, the same logic can be applied to writes to the log file by substituting LogicalDisk(*\<NTDS Log\>*)\Avg Disk sec/Write and LogicalDisk(*\<NTDS Log\>*)\Writes/sec):
>  
> - LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read indicates whether or not the current storage is adequately sized.  If the results are roughly equal to the Disk Access Time for the disk type, LogicalDisk(*\<NTDS\>*)\Reads/sec is a valid measure.  Check the manufacturer specifications for the storage on the back end, but good ranges for LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read would roughly be:
>   - 7200 – 9 to 12.5 milliseconds (ms)
>   - 10,000 – 6 to 10 ms
>   - 15,000 – 4 to 6 ms  
>   - SSD – 1 to 3 ms  
>   - >[!NOTE]
>     >Recommendations exist stating that storage performance is degraded at 15ms to 20ms (depending on source).  The difference between the above values and the other guidance is that the above values are the normal operating range.  The other recommendations are troubleshooting guidance to identify when client experience significantly degrades and becomes noticeable.  Reference Appendix C for a deeper explanation.
> - LogicalDisk(*\<NTDS\>*)\Reads/sec is the amount of I/O that is being performed.
>   - If LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read is within the optimal range for the backend storage, LogicalDisk(*\<NTDS\>*)\Reads/sec can be used directly to size the storage.
>   - If LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read is not within the optimal range for the backend storage, additional I/O is needed according to the following formula:
>     > (LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read) &divide; (Physical Media Disk Access Time) &times; (LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read)

Considerations:

- Note that if the server is configured with a sub-optimal amount of RAM, these values will be inaccurate for planning purposes.  They will be erroneously on the high side and can still be used as a worst case scenario.
- Adding/Optimizing RAM specifically will drive a decrease in the amount of read I/O (LogicalDisk(*\<NTDS\>*)\Reads/Sec.  This means the storage solution may not have to be as robust as initially calculated.  Unfortunately, anything more specific than this general statement is environmentally dependent on client load and general guidance cannot be provided.  The best option is to adjust storage sizing after optimizing RAM.

#### Virtualization considerations for performance

Similar to all of the preceding virtualization discussions, the key here is to ensure that the underlying shared infrastructure can support the DC load plus the other resources using the underlying shared media and all pathways to it. This is true whether a physical domain controller is sharing the same underlying media on a SAN, NAS, or iSCSI infrastructure as other servers or applications, whether it is a guest using pass through access to a SAN, NAS, or iSCSI infrastructure that shares the underlying media, or if the guest is using a VHD file that resides on shared media locally or a SAN, NAS, or iSCSI infrastructure. The planning exercise is all about making sure that the underlying media can support the total load of all consumers.

Also, from a guest perspective, as there are additional code paths that must be traversed, there is a performance impact to having to go through a host to access any storage. Not surprisingly, storage performance testing indicates that the virtualizing has an impact on throughput that is subjective to the processor utilization of the host system (see Appendix A: CPU Sizing Criteria), which is obviously influenced by the resources of the host demanded by the guest. This contributes to the virtualization considerations regarding processing needs in a virtualized scenario (see [Virtualization considerations for processing](#virtualization-considerations-for-processing)).

Making this more complex is that there are a variety of different storage options that are available that all have different performance impacts. As a safe estimate when migrating from physical to virtual, use a multiplier of 1.10 to adjust for different storage options for virtualized guests on Hyper-V, such as pass-through storage, SCSI Adapter, or IDE. The adjustments that need to be made when transferring between the different storage scenarios are irrelevant as to whether the storage is local, SAN, NAS, or iSCSI.

#### Calculation summary example

Determining the amount of I/O needed for a healthy system under normal operating conditions:

- LogicalDisk(*\<NTDS Database Drive\>*)\Transfers/sec during the peak period 15 minute period 
- To determine the amount of I/O needed for storage where the capacity of the underlying storage is exceeded:
  >*Needed IOPS* = (LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read &divide; *\<Target Avg Disk sec/Read\>*) &times; LogicalDisk(*\<NTDS Database Drive\>*)\Read/sec

|Counter|Value|
|-|-|
|Actual LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Transfer|.02 seconds (20 milliseconds)|
|Target LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Transfer|.01 seconds|
|Multiplier for change in available IO|0.02 &divide; 0.01 = 2|  
  
|Value name|Value|
|-|-|
|LogicalDisk(*\<NTDS Database Drive\>*)\Transfers/sec|400|
|Multiplier for change in available IO|2|
|Total IOPS needed during peak period|800|

To determine the rate at which the cache is desired to be warmed:

- Determine the maximum acceptable time to warm the cache. It is either the amount of time that it should take to load the entire database from disk, or for scenarios where the entire database cannot be loaded in RAM, this would be the maximum time to fill RAM.
- Determine the size of the database, excluding white space.  For more information, see [Evaluating for storage](#evaluating-for-storage).  
- Divide the database size by 8 KB; that will be the total IOs necessary to load the database.
- Divide the total IOs by the number of seconds in the defined time frame.

Note that the rate calculated, while accurate, will not be exact because previously loaded pages are evicted if ESE is not configured to have a fixed cache size, and AD DS by default uses variable cache size.

|Data points to collect|Values
|-|-|
|Maximum acceptable time to warm|10 minutes (600 seconds)
|Database size|2 GB|  
  
|Calculation step|Formula|Result|
|-|-|-|
|Calculate size of database in pages|(2 GB &times; 1024 &times; 1024) = *Size of database in KB*|2,097,152 KB|
|Calculate number of pages in database|2,097,152 KB &divide; 8 KB = *Number of pages*|262,144 pages|
|Calculate IOPS necessary to fully warm the cache|262,144 pages &divide; 600 seconds = *IOPS needed*|437 IOPS|

## Processing

### Evaluating Active Directory processor usage

For most environments, after storage, RAM, and networking are properly tuned as described in the Planning section, managing the amount of processing capacity will be the component that deserves the most attention. There are two challenges in evaluating CPU capacity needed:

- Whether or not the applications in the environment are being well-behaved in a shared services infrastructure, and is discussed in the section titled “Tracking Expensive and Inefficient Searches” in the article Creating More Efficient Microsoft Active Directory-Enabled Applications or migrating away from down-level SAM calls to LDAP calls.  
  
  In larger environments, the reason this is important is that poorly coded applications can drive volatility in CPU load, “steal” an inordinate amount of CPU time from other applications, artificially drive up capacity needs, and unevenly distribute load against the DCs.  
- As AD DS is a distributed environment with a large variety of potential clients, estimating the expense of a “single client” is environmentally subjective due to usage patterns and the type or quantity of applications leveraging AD DS. In short, much like the networking section, for broad applicability, this is better approached from the perspective of evaluating the total capacity needed in the environment.

For existing environments, as storage sizing was discussed previously, the assumption is made that storage is now properly sized and thus the data regarding processor load is valid. To reiterate, it is critical to ensure that the bottleneck in the system is not the performance of the storage. When a bottleneck exists and the processor is waiting, there are idle states that will go away once the bottleneck is removed.  As processor wait states are removed, by definition, CPU utilization increases as it no longer has to wait on the data. Thus, collect performance counters “Logical Disk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read” and “Process(lsass)\\% Processor Time”. The data in “Process(lsass)\\% Processor Time” will be artificially low if “Logical Disk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read” exceeds 10 to 15 ms, which is a general threshold that Microsoft support uses for troubleshooting storage-related performance issues. As before, it is recommended that sample intervals be either 15, 30, or 60 minutes. Anything less will generally be too volatile for good measurements; anything greater will smooth out daily peeks excessively.

### Introduction

In order to plan capacity planning for domain controllers, processing power requires the most attention and understanding. When sizing systems to ensure maximum performance, there is always a component that is the bottleneck and in a properly sized Domain Controller this will be the processor.

Similar to the networking section where the demand of the environment is reviewed on a site-by-site basis, the same must be done for the compute capacity demanded. Unlike the networking section, where the available networking technologies far exceed the normal demand, pay more attention to sizing CPU capacity.  As any environment of even moderate size; anything over a few thousand concurrent users can put significant load on the CPU.

Unfortunately, due to the huge variability of client applications that leverage AD, a general estimate of users per CPU is woefully inapplicable to all environments. Specifically, the compute demands are subject to user behavior and application profile. Therefore, each environment needs to be individually sized.

#### Target site behavior profile

As mentioned previously, when planning capacity for an entire site, the goal is to target a design with an *N* + 1 capacity design, such that failure of one system during the peak period will allow for continuation of service at a reasonable level of quality. That means that in an “*N*” scenario, load across all the boxes should be less than 100% (better yet, less than 80%) during the peak periods.

Additionally, if the applications and clients in the site are using best practices for locating domain controllers (that is, using the [DsGetDcName function](http://msdn.microsoft.com/en-us/library/windows/desktop/ms675983(v=vs.85).aspx)), the clients should be relatively evenly distributed with minor transient spikes due to any number of factors.

In the next example, the following assumptions are made:

- Each of the five DCs in the site has four of CPUs.
- Total target CPU usage during business hours is 40% under normal operating conditions (“*N* + 1”) and 60 % otherwise (“*N*”). During non-business hours, the target CPU usage is 80% because backup software and other maintenance are expected to consume all available resources.

![CPU usage chart](media/capacity-planning-considerations-cpu-chart.png)

Analyzing the data in the chart (Processor Information(_Total)\% Processor Utility) for each of the DCs:

- For the most part, the load is relatively evenly distributed which is what would be expected when clients use DC locator and have well written searches. 
- There are a number of five-minute spikes of 10%, with some as large as 20%. Generally, unless they cause the capacity plan target to be exceeded, investigating these is not worthwhile.  
- The peak period for all systems is between about 8:00 AM and 9:15 AM. With the smooth transition from about 5:00 AM through about 5:00 PM, this is generally indicative of the business cycle. The more randomized spikes of CPU usage on a box-by-box scenario between 5:00 PM and 4:00 AM would be outside of the capacity planning concerns.

  >[!NOTE]
  >On a well-managed system, said spikes are might be backup software running, full system antivirus scans, hardware or software inventory, software or patch deployment, and so on. Because they fall outside the peak user business cycle, the targets are not exceeded.

- As each system is about 40% and all systems have the same numbers of CPUs, should one fail or be taken offline, the remaining systems would run at an estimated 53% (System D's 40% load is evenly split and added to System A's and System C’s existing 40% load). For a number of reasons, this linear assumption is NOT perfectly accurate, but provides enough accuracy to gauge.  

  **Alternate scenario –** Two domain controllers running at 40%: One domain controller fails, estimated CPU on the remaining one would be an estimated 80%. This far exceeds the thresholds outlined above for capacity plan and also starts to severely limit the amount of head room for the 10% to 20% seen in the load profile above, which means that the spikes would drive the DC to 90% to 100% during the “*N*” scenario and definitely degrade responsiveness.

### Calculating CPU demands

The “Process\\% Processor Time” performance object counter sums the total amount of time that all of the threads of an application spend on the CPU and divides by the total amount of system time that has passed. The effect of this is that a multi-threaded application on a multi-CPU system can exceed 100% CPU time, and would be interpreted VERY differently than “Processor Information\\% Processor Utility”. In practice the “Process(lsass)\\% Processor Time” can be viewed as the count of CPUs running at 100% that are necessary to support the process’s demands. A value of 200% means that 2 CPUs, each at 100%, are needed to support the full AD DS load. Although a CPU running at 100% capacity is the most cost efficient from the perspective of money spent on CPUs and power and energy consumption, for a number of reasons detailed in Appendix A, better responsiveness on a multi-threaded system occurs when the system is not running at 100%.

To accommodate transient spikes in client load, it is recommended to target a peak period CPU of between 40% and 60% of system capacity. Working with the example above, that would mean that between 3.33 (60% target) and 5 (40% target) CPUs would be needed for the AD DS (lsass process) load. Additional capacity should be added in according to the demands of the base operating system and other agents required (such as antivirus, backup, monitoring, and so on). Although the impact of agents needs to be evaluated on a per environment basis, an estimate of between 5% and 10% of a single CPU can be made. In the current example, this would suggest that between 3.43 (60% target) and 5.1 (40% target) CPUs are necessary during peak periods.

The easiest way to do this is to use the “Stacked Area” view in Windows Reliability and Performance Monitor (perfmon), making sure all of the counters are scaled the same.

Assumptions:

- Goal is to reduce footprint to as few servers as possible. Ideally, one server would carry the load and an additional server added for redundancy (*N* + 1 scenario).

![Processor time chart for lsass process (over all processors)](media/capacity-planning-considerations-proc-time-chart.png)

Knowledge gained from the data in the chart (Process(lsass)\\% Processor Time):

- The business day starts ramping up around 7:00 and decreases at 5:00 PM.
- The peak busiest period is from 9:30 AM to 11:00 AM. 
  > [!NOTE]
  > All performance data is historical. The peak data point at 9:15 indicates the load from 9:00 to 9:15.
- There are spikes before 7:00 AM which could indicate either load from different time zones or background infrastructure activity, such as backups. Because the peak at 9:30 AM exceeds this activity, it is not relevant.
- There are three domain controllers in the site.

At maximum load, lsass consumes about 485% of one CPU, or 4.85 CPUs running at 100%. As per the math earlier, this means the site needs about 12.25 CPUs for AD DS. Add in the above suggestions of 5% to 10% for background processes and that means replacing the server today would need approximately 12.30 to 12.35 CPUs to support the same load. An environmental estimate for growth now needs to be factored in.

### When to tune LDAP weights

There are several scenarios where tuning [LdapSrvWeight](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc957291(v=technet.10)) should be considered. Within the context of capacity planning, this would be done when the application or user loads are not evenly balanced, or the underlying systems are not evenly balanced in terms of capability. Reasons to do so beyond capacity planning are outside of the scope of this article.

There are two common reasons to tune LDAP Weights:

- The PDC emulator is an example that affects every environment for which user or application load behavior is not evenly distributed. As certain tools and actions target the PDC emulator, such as the Group Policy management tools, second attempts in the case of authentication failures, trust establishment, and so on, CPU resources on the PDC emulator may be more heavily demanded than elsewhere in the site.
  - It is only useful to tune this if there is a noticeable difference in CPU utilization in order  to reduce the load on the PDC emulator and increase the load on other domain controllers will allow a more even distribution of load.
  - In this case, set LDAPSrvWeight between 50 and 75 for the PDC emulator.
- Servers with differing counts of CPUs (and speeds) in a site.  For example, say there are two eight-core servers and one four-core server.  The last server has half the processors of the other two servers.  This means that a well distributed client load will increase the average CPU load on the four-core box to roughly twice that of the eight-core boxes.
  - For example, the two eight-core boxes would be running at 40% and the four-core box would be running at 80%.
  - Also, consider the impact of loss of one eight-core box in this scenario, specifically the fact that the four-core box would now be overloaded.

#### Example 1 - PDC

| |Utilization with defaults|New LdapSrvWeight|Estimated new utilization|
|-|-|-|-|
|DC 1 (PDC emulator)|53%|57|40%|
|DC 2|33%|100|40%|
|DC 3|33%|100|40%|

The catch here is that if the PDC emulator role is transferred or seized, particularly to another domain controller in the site, there will be a dramatic increase on the new PDC emulator.

Using the example from the section [Target site behavior profile](#target-site-behavior-profile), an assumption was made that all three domain controllers in the site had four CPUs. What should happen, under normal conditions, if one of the domain controllers had eight CPUs? There would be two domain controllers at 40% utilization and one at 20% utilization. While this is not bad, there is an opportunity to balance the load a little bit better. Leverage LDAP weights to accomplish this.  An example scenario would be:

#### Example 2 - Differing CPU counts

| |Processor Information\\ %&nbsp;Processor Utility(_Total)<br />Utilization with defaults|New LdapSrvWeight|Estimated new utilization|
|-|-|-|-|
|4-CPU DC 1|40|100|30%|
|4-CPU DC 2|40|100|30%|
|8-CPU DC 3|20|200|30%|

Be very careful with these scenarios though. As can be seen above, the math looks really nice and pretty on paper. But throughout this article, planning for an “*N* + 1” scenario is of paramount importance. The impact of one DC going offline must be calculated for every scenario. In the immediately preceding scenario where the load distribution is even, in order to ensure a 60% load during an “*N*” scenario, with the load balanced evenly across all servers, the distribution will be fine as the ratios stay consistent. Looking at the PDC emulator tuning scenario, and in general any scenario where user or application load is unbalanced, the effect is very different:

| |Tuned Utilization|New LdapSrvWeight|Estimated New Utilization|
|-|-|-|-|
|DC 1 (PDC emulator)|40%|85|47%|
|DC 2|40%|100|53%|
|DC 3|40%|100|53%|

### Virtualization considerations for processing

There are two layers of capacity planning that need to be done in a virtualized environment. At the host level, similar to the identification of the business cycle outlined for the domain controller processing previously, thresholds during the peak period need to be identified. Because the underlying principals are the same for a host machine scheduling guest threads on the CPU as for getting AD DS threads on the CPU on a physical machine, the same goal of 40% to 60% on the underlying host are recommended. At the next layer, the guest layer, since the principals of thread scheduling have not changed, the goal within the guest remains in the 40% to 60% range.

In a direct mapped scenario, one guest per host, all the capacity planning done to this point needs to be added to the requirements (RAM, disk, network) of the underlying host operating system. In a shared host scenario, testing indicates that there is 10% impact on the efficiency of the underlying processors. That means if a site needs 10 CPUs at a target of 40%, the recommended amount of virtual CPUs to allocate across all the “*N*” guests would be 11. In a site with a mixed distribution of physical servers and virtual servers, the modifier only applies to the VMs. For example, if a site has an “*N* + 1” scenario, one physical or direct-mapped server with 10 CPUs would be about equivalent to one guest with 11 CPUs on a host, with 11 CPUs reserved for the domain controller.

Throughout the analysis and calculation of the CPU quantities necessary to support AD DS load, the numbers of CPUs that map to what can be purchased in terms physical hardware do not necessarily map cleanly. Virtualization eliminates the need to round up. Virtualization decreases the effort necessary to add compute capacity to a site, given the ease with which a CPU can be added to a VM. It does not eliminate the need to accurately evaluate the compute power needed so that the underlying hardware is available when additional CPUs need to be added to the guests.  As always, remember to plan and monitor for growth in demand.

### Calculation summary example

|System|Peak CPU|
|-|-|-|
|DC 1|120%|
|DC 2|147%|
|Dc 3|218%|
|Total CPU being used|485%|  
  
|Target system(s) count|Total bandwidth (from above)|
|-|-|
|CPUs needed at 40% target|4.85 &divide; .4 = 12.25|

Repeating due to the importance of this point, *remember to plan for growth*. Assuming 50% growth over the next three years, this environment will need 18.375 CPUs (12.25 &times; 1.5) at the three-year mark. An alternate plan would be to review after the first year and add in additional capacity as needed.

### Cross-trust client authentication load for NTLM

#### Evaluating cross-trust client authentication load

Many environments may have one or more domains connected by a trust. An authentication request for an identity in another domain that does not use Kerberos authentication needs to traverse a trust using the domain controller’s secure channel to another domain controller either in the destination domain or the next domain in the path to the destination domain. The number of concurrent calls using the secure channel that a domain controller can make to a domain controller in a trusted domain is controlled by a setting known as **MaxConcurrentAPI**. For domain controllers, ensuring that the secure channel can handle the amount of load is accomplished by one of two approaches: tuning **MaxConcurrentAPI** or, within a forest, creating shortcut trusts. To gauge the volume of traffic across an individual trust, refer to [How to do performance tuning for NTLM authentication by using the MaxConcurrentApi setting](https://support.microsoft.com/kb/2688798).

During data collection, this, as with all the other scenarios, must be collected during the peak busy periods of the day for the data to be useful.

> [!NOTE]
> Intraforest and interforest scenarios may cause the authentication to traverse multiple trusts and each stage would need to be tuned.

#### Planning

There are a number of applications that use NTLM authentication by default, or use it in a certain configuration scenario. Application servers grow in capacity and service an increasing number of active clients. There is also a trend that clients keep sessions open for a limited time and rather reconnect on a regular basis (such as email pull sync). Another common example for high NTLM load is web proxy servers that require authentication for Internet access.

These applications can cause a significant load for NTLM authentication, which can put significant stress on the DCs, especially when users and resources are in different domains.

There are multiple approaches to managing cross-trust load, which in practice are used in conjunction rather than in an exclusive either/or scenario. The possible options are:

- Reduce cross-trust client authentication by locating the services that a user consumes in the same domain that the user is resident in.
- Increase the number of secure-channels available. This is relevant to intraforest and cross-forest traffic and are known as shortcut trusts.
- Tune the default settings for **MaxConcurrentAPI**.

For tuning **MaxConcurrentAPI** on an existing server, the equation is:

> *New_MaxConcurrentApi_setting* &ge; (*semaphore_acquires* + *semaphore_time-outs*) &times; *average_semaphore_hold_time* &divide; *time_collection_length*

For more information, see [KB article 2688798: How to do performance tuning for NTLM authentication by using the MaxConcurrentApi setting](http://support.microsoft.com/kb/2688798).

## Virtualization considerations

None, this is an operating system tuning setting.

### Calculation summary example

|Data type|Value|
|-|-|
|Semaphore Acquires (Minimum)|6,161|
|Semaphore Acquires (Maximum)|6,762|
|Semaphore Timeouts|0|
|Average Semaphore Hold Time|0.012|
|Collection Duration (seconds)|1:11 minutes (71 seconds)|
|Formula (from KB 2688798)|((6762 &ndash; 6161) + 0) &times; 0.012 /|
|Minimum value for **MaxConcurrentAPI**|((6762 &ndash; 6161) + 0) &times; 0.012 &divide; 71 = .101|

For this system for this time period, the default values are acceptable.

## Monitoring for compliance with capacity planning goals

Throughout this article, it has been discussed that planning and scaling go towards utilization targets. Here is a summary chart of the recommended thresholds that must be monitored to ensure the systems are operating within adequate capacity thresholds. Keep in mind that these are not performance thresholds, but capacity planning thresholds. A server operating in excess of these thresholds will work, but is time to start validating that all the applications are well behaved. If said applications are well behaved, it is time to start evaluating hardware upgrades or other configuration changes.

|Category|Performance counter|Interval/Sampling|Target|Warning|
|-|-|-|-|-|
|Processor|Processor Information(_Total)\\% Processor Utility|60 min|40%|60%|
|RAM (Windows Server 2008 R2 ou une version antérieure)|Mémoire\Mégaoctets Mo|< 100 MO|N/A|< 100 MO|
|RAM (Windows Server 2012)|Memory\Long-durée moyenne du Cache en attente Lifetime(s)|30 min|Doit être testée.|Doit être testée.|
|Réseau|Interface réseau (\*) \Bytes envoyés/s<br /><br />Interface réseau (\*) \Bytes reçus/s|30 min|40%|60 %|
|Stockage|Disque logique ( *\<lecteur de base de données NTDS\>* ) \Avg disque s/lecture<br /><br />Disque logique ( *\<lecteur de base de données NTDS\>* ) \Avg disque s/écriture|60 min|10 ms|15 ms|
|Services AD|Netlogon (\*) \Average sémaphore contenir temps|60 min|0|1 seconde|

## <a name="appendix-a-cpu-sizing-criteria"></a>Annexe A : Critère de dimensionnement du processeur

### <a name="definitions"></a>DÉFINITIONS

**Processeur (microprocesseur) –** un composant qui lit et exécute les instructions de programme  

**Processeur –** unité centrale  

**Processeur multicœur –** plusieurs processeurs sur le même circuit intégré  

**Multiprocesseur –** plusieurs unités centrales, pas sur le même circuit intégré  

**Processeur logique –** un moteur de calcul logique du point de vue du système d’exploitation  

Cela inclut les multithreads, un seul cœur de processeur multicœur ou un processeur à cœur unique.  

Comme les systèmes de serveur d’aujourd'hui ont plusieurs processeurs, plusieurs processeurs multicœurs et hyper-threading, ces informations sont généralisées pour couvrir les deux scénarios. Par conséquent, le processeur logique du terme servira car il représente le système d’exploitation et la perspective d’application des moteurs de calculs disponibles.

### <a name="thread-level-parallelism"></a>Parallélisme au niveau du thread

Chaque thread est une tâche indépendante, comme chaque thread possède sa propre pile et obtenir des instructions. Étant donné que les services AD DS est multithread et le nombre de threads disponibles peut être ajustée à l’aide de [comment afficher et définir la stratégie LDAP dans Active Directory à l’aide de Ntdsutil.exe](http://support.microsoft.com/kb/315071), il s’adapte bien entre plusieurs processeurs logiques.

### <a name="data-level-parallelism"></a>Parallélisme au niveau des données

Cela implique le partage des données entre plusieurs threads dans un processus (dans le cas le seul le processus de AD DS) et entre plusieurs threads dans plusieurs processus (en général). Inquiétude à excessif simplifiant le cas, cela signifie que les modifications apportées aux données sont répercutées à tous les threads en cours d’exécution dans les différents niveaux de cache (L1, L2, L3) pour tous les cœurs en cours d’exécution des threads ladite, ainsi que la mise à jour de la mémoire partagée. Performances peuvent se dégrader lors des opérations d’écriture, tandis que tous les emplacements de mémoire sont rendues cohérentes que le traitement de l’instruction peut continuer.

### <a name="cpu-speed-vs-multiple-core-considerations"></a>Vitesse et considérations relatives à plusieurs cœurs de processeur

La règle générale est plus rapide des processeurs logiques de réduire la durée nécessaire pour traiter une série d’instructions, lors de la plus logique signifie de processeurs que davantage de tâches peut être exécuté en même temps. Ces règles générales diviser dès les scénarios sont par nature plus complexes avec les considérations d’extraction de données à partir de la mémoire partagée, en attente sur le parallélisme au niveau des données et la surcharge de la gestion de plusieurs threads. C’est également pourquoi l’évolutivité dans des systèmes multicœurs n’est pas linéaire.

Prendre en compte les analogies suivantes dans ces considérations : réflexion d’une autoroute, avec chaque thread en cours d’une voiture individuelle, chaque couloir en cours d’un cœur, ainsi que la limite de vitesse en cours de la vitesse d’horloge.

1. S’il n'existe qu’une seule voiture sur l’autoroute, peu importe s’il existe deux couloirs ou 12 couloirs. Cette voiture ne va opérées aussi rapidement que la limite de vitesse.
1. Supposons que les données que le thread a besoin ne sont pas immédiatement disponibles. L’analogie serait qu’un segment de route est arrêtée. S’il n'existe qu’une seule voiture sur l’autoroute, peu importe ce que la limite de vitesse est jusqu'à ce que la voie est rouvert (les données sont extraites de la mémoire).
1. Comme le nombre de voitures augmente, la surcharge pour gérer le nombre de voitures augmente. Comparer l’expérience de conduite et la quantité d’attention nécessaire lors de la route est pratiquement vide (par exemple, en fin de soirée) et lorsque le trafic est importante (par exemple, l’après-midi intermédiaire, mais pas heures). En outre, prenez en considération l’attention nécessaire lorsque vous menez sur deux AutoRoute, où il y n'a qu’une seule autre couloir à vous soucier de ce que font les pilotes, par rapport à une autoroute six où un a à vous soucier de quelles beaucoup d’autres pilotes font.
   > [!NOTE]
   > L’analogie à propos du scénario d’heures est étendue dans la section suivante : Temps de réponse/comment le système affaires affecte les performances.

Par conséquent, plus de détails sur des processeurs plus rapides ou deviennent très subjectives au comportement d’application, qui dans le cas d’AD DS est très écologique spécifique et varie également de serveur à serveur dans un environnement. C’est pourquoi les références précédemment dans cet article ne pas investir massivement dans étant trop précises et une marge de sécurité est incluse dans les calculs. Lorsque vous élaborez des décisions d’achat en fonction de budget, il est recommandé qu’optimisation de l’utilisation des processeurs à 40 % (ou le nombre souhaité pour l’environnement) se produit en premier, avant d’envisager l’achat de processeurs plus rapides. La synchronisation accrue sur des processeurs plus réduit le véritable avantage de davantage de processeurs à partir de la progression linéaire (2&times; le nombre de processeurs fournit moins de 2&times; la puissance de calcul supplémentaires disponibles).

> [!NOTE]
> D’Amdahl et de Gustafson sont les concepts pertinents ici.

### <a name="response-timehow-the-system-busyness-impacts-performance"></a>Temps de réponse / l’impact des performances par les affaires de système

Théorie de file d’attente est une technique mathématique de lignes en attente (files d’attente). En théorie de file d’attente, la loi de l’utilisation est représentée par l’équation :

*U* k = *B* &divide; *T*

Où *U* k est le pourcentage d’utilisation, *B* est la quantité de temps occupé, et *T* est la durée totale, le système a été observé. Traduit dans le contexte de Windows, cela signifie que le nombre de 100 nanosecondes (ns) threads intervalle qui se trouvent dans un état d’exécution divisé par intervalles de 100 ns combien ont été disponibles dans une donnée de l’intervalle de temps. Il s’agit exactement la formule de calcul de % processeur Utility (référence [objet processeur](https://docs.microsoft.com/previous-versions/ms804036(v=msdn.10)) et [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/previous-versions/windows/embedded/ms901169(v=msdn.10))).

File d’attente théorie fournit également la formule : *N* = *U* k &divide; (1 &ndash; *U* k) pour estimer le nombre d’éléments en attente basées sur l’utilisation ( *N* est le longueur de la file d’attente). Ce graphique tous les intervalles de l’utilisation fournit des estimations suivantes à la durée pendant laquelle la file d’attente pour obtenir sur le processeur est à toute charge de processeur donnée.

![Longueur de file d’attente](media/capacity-planning-considerations-queue-length.png)

Il est constaté qu’après le chargement de 50 % du processeur, en moyenne il y a toujours une attente d’un autre élément dans la file d’attente, avec une rapide sensiblement augmenter après environ 70 % utilisation du processeur.

Retour à l’analogie de conduite utilisé précédemment dans cette section :

- Les heures d’indisponibilité de « après-midi intermédiaire », en théorie, comprises quelque part dans la plage de 40 % à 70 %. Est suffisamment de trafic telle que de possibilité de choisir n’importe quel couloir n’est pas majorly limitée, et le risque d’un autre pilote dans la manière dont, tout en haut, ne nécessite pas le niveau d’effort pour un intervalle sans échec entre autres véhicules sur la route « rechercher ».
- Un remarquerez que comme le trafic est proche heures, le système de route approche la capacité de 100 %. Modification des couloirs peut devenir très difficile, car les voitures sont si proches qu’attention accrue doit être testée pour ce faire.

C’est pourquoi le long terme moyennes pour capacité de manière plus restrictive estimée à 40 % permet de place principal pour les pics d’activité anormales dans charger, si vous dit de pics temporaires (par exemple, les requêtes mal codés qui s’exécutent pendant quelques minutes) ou anormale est effectuée en général charge (le matin de le premier jour après un long week-end).

L’instruction ci-dessus ce qui concerne le calcul du temps processeur % qui est la même que la loi de l’utilisation est un peu d’une simplification de la facilité du lecteur général. Pour ces plus mathématiquement rigoureux :  
- Traduire le [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10))
  - *B* = le nombre d’intervalles de 100 ns « Inactif » thread consacré par le processeur logique. La modification de la «*X*« variable dans le [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10)) calcul
  - *T* = le nombre total d’intervalles de 100 ns dans un intervalle de temps donné. La modification de la «*Y*« variable dans le [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10)) calcul.
  - *U* k = le pourcentage d’utilisation du processeur logique par le « Thread inactif » ou le % de temps d’inactivité.  
- Utilisation de calculs :
  - *U* k = 1 – %Processor Time
  - % Temps processeur = 1 – *U* k
  - % Temps processeur = 1 – *B* / *T*
  - %Processor Time = 1 – *X1* – *X0* / *Y1* – *Y0*

### <a name="applying-the-concepts-to-capacity-planning"></a>L’application des concepts à la planification de capacité

Les calculs ci-dessus peuvent rendre déterminations quant au nombre de processeurs logiques nécessaires dans un système semblent extrêmement complexes. C’est pourquoi l’approche à dimensionner les systèmes se concentre sur la détermination de l’utilisation de cible maximale selon actuel de charge et de calcul du nombre de processeurs logiques nécessaires pour y accéder. En outre, tandis que les vitesses de processeur logique aura un impact significatif sur les performances, l’efficacité du cache, exigences de cohérence de mémoire, planification des threads et la synchronisation et à charge équilibrée de manière imparfaite client charge auront un impact significatif performances varient sur une base de serveur par serveur. Avec le coût relativement bon marché de puissance de calcul, tente d’analyser et de déterminer le nombre idéal de processeurs nécessité devient plus un exercice d’apprentissage qu’il fournit la valeur commerciale.

Quarante pour cent n’est pas obligatoire d’or, il est un départ raisonnable. Divers consommateurs d’Active Directory nécessitent différents niveaux de réactivité. Peut par ailleurs être scénarios, les environnements peuvent s’exécuter à 80 % ou 90 % d’utilisation comme une moyenne soutenue, les temps d’attente accrue pour l’accès au processeur n’affectera pas sensiblement les performances du client. Il est important de réitérer qu’il existe plusieurs domaines dans le système qui sont beaucoup plus lent que le processeur logique dans le système, y compris l’accès à la RAM, l’accès au disque et la transmission de la réponse sur le réseau. Tous ces éléments doivent être ajustés conjointement. Exemples :

- Ajout de plus de processeurs sur un système en cours d’exécution 90 % et qui est liée au disque va probablement pas à améliorer considérablement les performances. Une analyse plus approfondie du système identifiera probablement qu’il existe un grand nombre de threads que vous n’obtenez pas la même sur le processeur, car ils sont en attente d’e/s pour terminer.
- Résoudre les problèmes liés aux disques potentiellement signifie que les threads qui ont été précédemment beaucoup de temps en état d’attente ne seront plus être en état d’attente d’e/s et il y aura plusieurs concurrence pour le temps processeur, ce qui signifie que l’utilisation de 90 % dans le précédent exemple passera à 100 % (car il ne peut pas être supérieur). Les deux composants doivent être ajustés conjointement.
  > [!NOTE]
  > Processeur Information(*)\\% processeur utilitaire peut dépasser 100 % avec les systèmes qui ont un mode « Turbo ».  Il s’agit où l’UC dépasse la vitesse du processeur évalués sur de courtes périodes.  Documentation du fabricant du processeur de référence et la description du compteur pour avoir un meilleur aperçu.  

Considérant les aspects de l’utilisation du système dans son ensemble offre également aux contrôleurs de domaine de conversation en tant qu’invités virtualisées. [Temps de réponse / l’impact des performances par les affaires système](#response-timehow-the-system-busyness-impacts-performance) s’applique à l’hôte et l’invité dans un scénario virtualisé. C’est pourquoi dans un hôte avec seul invité, un contrôleur de domaine (et généralement n’importe quel système) a près les mêmes performances que c’est le cas sur du matériel physique. Ajout d’invités supplémentaires aux hôtes augmente l’utilisation de l’hôte sous-jacent, ce qui augmente les temps d’attente pour accéder aux processeurs comme expliqué précédemment. En bref, l’utilisation du processeur logique doit être géré sur l’hôte et sur les niveaux de l’invité.

Étendre les analogies précédentes, en laissant de l’autoroute en tant que le matériel physique, l’invité machine virtuelle sera analogized avec un bus (un bus d’express qui va directement vers la destination de l’avenant veut). Supposons que les quatre scénarios suivants :

- Il est heures creuses, un avenant Obtient un bus, qui est presque vide et le bus obtient sur une route qui est également presque vide. Car il n’existe aucun trafic à entrer en concurrence avec, le conducteur a un tour facile agréable et il obtient tout aussi rapide que si le conducteur avait fait à la place. Temps de trajet du conducteur sont toujours soumis à la limite de vitesse.
- Il s’agit heures creuses, le bus est presque vide mais la plupart des couloirs de déplacement est fermée, le bus est toujours encombré. L’avenant est sur un bus presque vide sur une route encombré. Tandis que le conducteur n’a pas beaucoup de concurrence dans le bus de l’emplacement où se trouvent, la durée totale est toujours déterminée par le reste du trafic à l’extérieur.
- Il s’agit heures autoroutier et le bus sont encombrés. Non seulement le voyage prend plus de temps, mais et déconnexion le bus est un cauchemar, car les personnes sont épaule pour épaule et le bus n’est pas bien meilleur. Ajout de plusieurs bus (processeurs logiques à l’invité) ne signifie pas qu’ils tiennent sur la route les plus facilement, ou que le voyage va être réduit.
- Le dernier scénario, même si elle peut étirer l’analogie avec un peu, est où le bus est saturée, mais la route n’est pas saturé. Tandis que le conducteur aura toujours des difficultés à l’obtention et de désactiver le bus, le voyage seront efficace une fois que le bus est en déplacement. Il s’agit du seul scénario dans lequel ajouter plus de bus (processeurs logiques à l’invité) améliore les performances de l’invité.

À partir de là, il est relativement facile d’extrapoler qu’il n’y a un nombre de scénarios entre l’utilisation de 0 % et l’état utilisé à 100 % de la route et l’état de 0 % et 100 % utilisés du bus qui ont différents degrés de l’impact.

Appliquer les principaux au-dessus de 40 % UC en tant que cible raisonnable pour l’hôte, ainsi que l’invité est un départ raisonnable pour le même raisonnement comme ci-dessus, la quantité de queuing.

## <a name="appendix-b-considerations-regarding-different-processor-speeds-and-the-effect-of-processor-power-management-on-processor-speeds"></a>Annexe B : Considérations relatives à des vitesses de processeur différentes et l’effet de la gestion de l’alimentation du processeur sur les vitesses de processeur

Dans toutes les sections sur la sélection de processeur l’hypothèse que le processeur fonctionne à 100 % de la vitesse d’horloge pendant tout ce temps que les données sont collectées et que les systèmes de remplacement aura la même vitesse du processeur. Malgré les deux hypothèses dans la pratique est false, en particulier avec Windows Server 2008 R2 et versions ultérieures, où le mode d’alimentation par défaut est **équilibré**, la méthodologie serait encore car il s’agit de l’approche classique. Bien que le taux d’erreur potentielles peut augmenter, il augmente uniquement la marge de sécurité, comme l’augmentation de vitesse du processeur.

- Par exemple, dans un scénario où les 11.25 processeurs sont demandées, si les processeurs étaient en cours d’exécution à demi-vitesse lorsque les données ont été collectées, l’estimation plus précise peut être 5.125 &divide; 2.
- Il est impossible de garantir que le fait de doubler la vitesse d’horloge serait double la quantité de traitement qui se produit pendant une période donnée. Cela est due au fait que la durée pendant laquelle les processeurs passent en attente sur la mémoire RAM ou d’autres composants système pourraient rester le même. L’effet net est que les processeurs plus rapides peuvent passer un pourcentage supérieur de la durée inactif pendant l’attente sur les données à extraire. Là encore, il est recommandé de conserver le plus petit dénominateur commun est conservateur, et d’éviter tente de calculer un niveau potentiellement false de précision en supposant une comparaison linéaire entre les vitesses de processeur.

Ou bien, si les vitesses de processeur dans le matériel de remplacement sont inférieurs à ceux de matériel actuel, il serait possible sans augmenter l’estimation de processeurs requis par un montant proportionnel. Par exemple, il est calculé que 10 processeurs sont nécessaires pour supporter la charge dans un site et les processeurs en cours sont en cours d’exécution 3.3 GHz et processeurs de remplacement seront exécutera à 2,6 Ghz, il s’agit d’une baisse de 21 % de la vitesse. Dans ce cas, 12 processeurs serait la quantité recommandée.

Ceci dit, cette variabilité ne changerait pas les cibles de l’utilisation de processeur de gestion de la capacité. Comme la vitesse d’horloge soient ajustées dynamiquement en fonction de la charge demandée, en exécutant le système sous des charges plus élevées générera un scénario où le processeur consacre plus de temps dans un état de vitesse de l’horloge plus élevé, rendre l’objectif ultime à 40 % d’utilisation de 100 % état de la vitesse de l’horloge à son maximum. Quoi que ce soit inférieure à celle génère des économies d’énergie, car les vitesses de processeur seront peuvent-elles être limitées revenir au cours de scénarios de pointe.

> [!NOTE]
> Une option consisterait à désactiver la gestion de l’alimentation sur les processeurs (définition de la gestion de l’alimentation sur **hautes performances**) tandis que les données sont collectées. Qui donnerait une représentation plus précise de la consommation du processeur sur le serveur cible.

Pour ajuster les estimations pour des processeurs différents, il permet de plus de sécurité, à l’exclusion des autres goulots d’étranglement système décrites ci-dessus, à supposer que le fait de doubler les vitesses de processeur doublé la quantité de traitement qui a pu être effectuée.  Aujourd'hui, l’architecture interne de processeurs est suffisamment différent entre les processeurs, c'est-à-dire un moyen plus sûr pour évaluer les effets de l’utilisation des processeurs différents de données a été effectuées à partir d’exploiter le test d’évaluation SPECint_rate2006 à partir de l’évaluation des performances Standard Corporation.

1. Rechercher les scores SPECint_rate2006 pour le processeur qui sont en cours d’utilisation et qui envisagent à utiliser.
    1. Sur le site Web de l’entreprise d’évaluation de performances Standard, sélectionnez **résultats**, mettez en surbrillance **CPU2006**, puis sélectionnez **rechercher tous les résultats SPECint_rate2006**.
    1. Sous **Simple demande**, entrez les critères de recherche pour le processeur cible, par exemple **correspondances processeur E5-2630 (baselinetarget)** et **correspondances processeur E5-2650(lignedebase)** .
    1. Trouver la configuration de serveur et le processeur à utiliser (ou quelque chose de la fermeture, si une correspondance exacte n’est pas disponible) et notez la valeur dans le **résultat** et **# cœurs** colonnes.
1. Pour déterminer le modificateur d’utiliser l’équation suivante :
   >((*Valeur de score par cœur de plateforme cible*) &times; (*MHz par cœur de la plateforme de la ligne de base*)) &divide; ((*base de référence de valeur de score par cœur*) &times; (*MHz par cœur de la plateforme cible*))  

    À l’aide de l’exemple ci-dessus :
   >(35.83 &times; 2000) &divide; (33.75 &times; 2300) = 0.92
1. Multipliez le nombre estimé de processeurs par le modificateur.  Dans le cas ci-dessus pour accéder à partir de la E5-2650 processeur au processeur E5-2630 multiplier les 11.25 processeurs calculées &times; 0,92 = 10,35 processeurs nécessités.

## <a name="appendix-c-fundamentals-regarding-the-operating-system-interacting-with-storage"></a>Annexe c : Principes de base concernant le système d’exploitation interaction avec le stockage

Les concepts de théorie de file d’attente décrites dans [temps de réponse / l’impact des performances par les affaires système](#response-timehow-the-system-busyness-impacts-performance) sont également applicables au stockage. Avoir une connaissance de la façon dont les handles de système d’exploitation d’e/s est nécessaire pour appliquer ces concepts. Dans le système d’exploitation Microsoft Windows, une file d’attente pour stocker les demandes d’e/s est créé pour chaque disque physique. Toutefois, une clarification sur le disque physique doit être effectuée. SAN et les contrôleurs de baie présentent des agrégations de piles pour le système d’exploitation de disques physiques comme unique. En outre, SAN et les contrôleurs de baie peuvent agréger plusieurs disques dans l’ensemble d’un tableau et puis fractionner ce tableau défini dans plusieurs « partitions », qui est à son tour présentée au système d’exploitation en tant que plusieurs disques physiques (figure Réf.).

![Bloquer les piles](media/capacity-planning-considerations-block-spindles.png)  

Dans cette figure, les deux piles sont mises en miroir et divisés en zones logiques pour le stockage de données (Data 1 et 2 de données). Ces zones logiques sont affichés par le système d’exploitation en tant que disques physiques distincts.

Bien que cela puisse être très déroutant, la terminologie suivante est utilisée tout au long de cette annexe pour identifier les différentes entités :

- **Broche –** l’appareil est physiquement installée sur le serveur.
- **Tableau –** une collection de piles agrégées par contrôleur.
- **Tableau de partition –** un partitionnement du tableau agrégé
- **Numéro d’unité logique –** un tableau, utilisé pour faire référence aux réseaux SAN
- **Disque –** ce que le système d’exploitation observe à être un seul disque physique.
- **Partition –** un partitionnement logique de ce que le système d’exploitation perçoit comme un disque physique.

### <a name="operating-system-architecture-considerations"></a>Considérations d’architecture de système d’exploitation

Le système d’exploitation crée une file d’attente premier In/First sorti (FIFO) d’e/s pour chaque disque est observée ; ce disque peut être représentant un axe, un tableau ou une partition de tableau. Le système d’exploitation du point de vue, en ce qui concerne la gestion des e/s, le plus actif files d’attente améliore leur qualité. Comme une file d’attente FIFO est sérialisé, ce qui signifie que toutes les e/s émis pour le sous-système de stockage doivent être traités dans l’ordre de la demande est parvenue. En mettant en corrélation chaque disque observé par le système d’exploitation avec un axe/tableau, le système d’exploitation gère maintenant une file d’attente d’e/s pour chaque ensemble de disques, ainsi éliminer la contention des ressources d’e/s rares sur disques et isoler à la demande d’e/s pour un seul disque. En tant qu’exception, Windows Server 2008 introduit le concept de définition des priorités d’e/s, et les applications conçues pour utiliser la priorité « Faible » n’entrent pas dans cet ordre normal et heurtent. Les applications ne sont pas spécifiquement codées pour tirer parti de la valeur par défaut de la priorité « Faible » à « Normal ».

### <a name="introducing-simple-storage-subsystems"></a>Présentation des sous-systèmes de stockage simple

En commençant par un exemple simple (un seul disque dur à l’intérieur d’un ordinateur) une analyse de composant par composant recevra. La décomposition dans les composants du sous-système de stockage principales, le système se compose de :

- **1 –** 10 000 tr/min Ultra rapide SCSI HD (Ultra rapide SCSI a un taux de transfert 20 Mo/s)
- **1 –** Bus SCSI (le câble)
- **1 –** rapide carte SCSI ultra
- **1 –** bus PCI de 33 MHz 32 bits

Une fois que les composants sont identifiés, une idée de la quantité de données faire transiter le système, ou la quantité e/s peuvent être gérées, peut être calculée. Notez que la quantité d’e/s et la quantité de données permettant de faire transiter le système sont mis en corrélation, mais pas identiques. Cette corrélation varie selon que les e/s de disque est aléatoires ou séquentielles et de la taille de bloc. (Toutes les données sont écrites sur le disque sous forme de bloc, mais différentes applications à l’aide de différentes tailles de bloc). Chaque composant par composant :

- **Le disque dur –** le disque dur de 10 000 tr/min moyenne a un 7 en millisecondes (ms) recherchent d’heure et une heure d’accès 3 ms. Recherche d’heure est la quantité moyenne de la durée de la tête de lecture/écriture à déplacer vers un emplacement sur le plateau. Temps d’accès est la quantité moyenne de temps que nécessaire pour lire ou écrire les données sur le disque, une fois la tête à l’emplacement approprié. Par conséquent, le temps moyen pour la lecture d’un bloc unique de données dans un disque dur de 10 000 tr/min constitue une recherche et un accès, pour un total d’environ 10 ms (ou.010 secondes) par le bloc de données.

  Lors de chaque accès au disque requiert un mouvement de la tête vers un nouvel emplacement sur le disque, le comportement en lecture/écriture est appelé « aléatoires ». Par conséquent, une fois toutes les e/s aléatoires, un disque dur de 10 000 tr/min peut gérer environ 100 e/s par seconde (IOPS) (la formule est 1 000 ms par seconde divisé par 10 ms par e/s ou 1000/10 = 100 e/s).

  Vous pouvez également lorsque toutes les e/s se produit dans les secteurs adjacents sur le disque dur, il est appelé d’e/s séquentielles. E/s séquentielles n’a aucun temps de recherche lorsque le premier e/s est terminée, la tête de lecture/écriture est au début d’où le bloc de données suivant est stocké sur le disque dur. Par conséquent un HD de 10 000 tr/min est capable de gérer environ 333 e/s par seconde (1 000 ms par seconde divisé par 3 ms par e/s).

  >[!NOTE]
  >Cet exemple ne reflète pas le cache disque, selon lequel les données d’un cylindre sont généralement conservées. Dans ce cas, les 10 ms sont nécessaires sur les e/s de premier et le disque lit le cylindre entière. Tous les autres e/s séquentielles est satisfaite à partir du cache. Par conséquent, les caches de disque peuvent améliorer les performances d’e/s séquentielles.
  
  Jusqu’ici, le taux de transfert du disque dur a été non pertinent. Indique si le disque dur est de 20 Mo/s Ultra large ou un Ultra3 160 Mo/s, la quantité réelle d’e/s le peut être gérée par le disque dur de 10 000 tr/min est environ 100 aléatoire ou e/s séquentielles ~ 300. Modifier les tailles de bloc basé sur l’application d’écriture sur le disque, la quantité de données qui sont extraites par les e/s est différente. Par exemple, si la taille de bloc est de 8 Ko, 100 opérations d’e/s lues ou écrire sur le disque dur, un total de 800 Ko. Toutefois, si la taille de bloc est de 32 Ko, 100 e/s sont en lecture/écriture 3 200 Ko (3,2 Mo) sur le disque dur. Tant que le taux de transfert SCSI est dépassant le montant total des données transférées, l’obtention d’un transfert plus « rapide » lecteur de taux a un accès rien. Consultez les tableaux suivants pour la comparaison.

  | |Accéder à 4ms 7200 tr/min 9ms seek,|10 000 %7 MS RPM seek, 3ms accéder|15 000 tr/min 4ms seek, accéder à 2 MS
  |-|-|-|-|
  |E/s aléatoires|80|100|150|
  |E/s séquentielles|250|300|500|  
  
  |Lecteur de 10 000 tr/min|Taille de bloc de 8 Ko (Active Directory Jet)|
  |-|-|
  |E/s aléatoires|800 Ko/s|
  |E/s séquentielles|2400 Ko/s|

- **Fond de panier SCSI (bus) –** comprendre comment le « SCSI fond de panier (bus) », ou dans ce scénario, le câble du ruban, débit d’impacts du sous-système de stockage dépend des connaissances de la taille du bloc. Essentiellement, la question serait combien les e/s peut le handle de bus si les e/s se trouve dans les blocs de 8 Ko ? Dans ce scénario, le bus SCSI est 20 Mo/s, ou 20 480 octets Ko/s. 20 480 octets Ko/s divisé par blocs de 8 Ko génère un maximum d’environ 2500 e/s pris en charge par le bus SCSI.

  >[!NOTE]
  >Les chiffres dans le tableau suivant représentent un exemple. La plupart des périphériques de stockage attachés utilisent actuellement la norme PCI Express, qui fournit un débit beaucoup plus élevé.  
  
  |E/s pris en charge par le bus SCSI par taille de bloc|Taille de bloc de 2 Ko|Taille de bloc de 8 Ko (AD Jet) (SQL Server 7.0/SQL Server 2000)
  |-|-|-|
  |20 Mo/s|10 000|2 500|
  |40 Mo/s|20 000|5 000|
  |128 Mo/s|65,536|16,384|
  |320 Mo/s|160 000|40 000|

  Que peut être déterminé à partir de ce graphique, dans le scénario présenté, quel que soit l’utilisation de quelles, le bus ne sera jamais un goulot d’étranglement, comme l’axe, valeur maximale est 100 e/s, bien en dessous des seuils ci-dessus.

  >[!NOTE]
  >Cela suppose que le bus SCSI est efficace à 100 %.
  
- **Carte SCSI –** pour déterminer la quantité d’e/s que cela peut gérer, les spécifications du fabricant doivent être vérifiées. Dirige les demandes d’e/s à l’appareil approprié requiert un traitement quelconque, par conséquent, la quantité d’e/s qui peuvent être gérés dépend de la carte SCSI (ou un contrôleur de baie) processeur.

  Dans cet exemple, l’hypothèse capable d’e/s de 1 000 être gérée sera établie.

- **Bus PCI –** il s’agit d’un composant souvent négligé. Dans cet exemple, cela ne sera pas le goulot d’étranglement ; Cependant comme systèmes de monter en puissance, il peut devenir un goulot d’étranglement. Pour référence, un bus PCI de 32 bits à 33Mhz peut dans le transfert de la théorie 133 Mo/s de données. L’équation est le suivant :  
  > 32 bits &divide; 8 bits par octet &times; 33 MHz = 133 Mo/s.  

  Notez que la limite théorique ; en réalité qu’environ 50 % de la valeur maximale est réellement atteint, bien que dans certains scénarios en rafale, l’efficacité de 75 % peut être obtenue sur de courtes périodes.

  Bus PCI 64 bits 66 Mhz peut prendre en charge une valeur théorique maximale de (64 bits &divide; 8 bits par octet &times; 66 Mhz) = 528 Mo/s. en outre, n’importe quel autre appareil (telles que la carte réseau, deuxième contrôleur SCSI et ainsi de suite) réduit la bande passante disponible comme la bande passante est partagée et les appareils entrent en compétition pour les ressources limitées.

Après l’analyse des composants de ce sous-système de stockage, la pile est le facteur limitant quantité d’e/s qui peut être demandée et donc la quantité de données permettant de faire transiter le système. Plus précisément, en cas d’AD DS, il s’agit 100 e/s aléatoires par seconde par incréments de 8 Ko, soit un total de 800 Ko par seconde après l’accès à la base de données Jet. Vous pouvez également le débit maximal pour un axe qui est alloué exclusivement des fichiers journaux risquent d’être affectées les limitations suivantes : 300 e/s séquentielles par seconde par incréments de 8 Ko, soit un total de 2 400 Ko (2,4 Mo) par seconde.

À présent, le tableau suivant après avoir analysé une configuration simple, montre où le goulot d’étranglement se produit comme composants du sous-système de stockage sont modifiés ou ajoutés.

|Notes|Analyse de goulot d’étranglement|Disque|Bus|Carte|Bus PCI|
|-|-|-|-|-|-|
|Il s’agit de la configuration du contrôleur de domaine après l’ajout d’un deuxième disque. La configuration de disque représente le goulot d’étranglement à 800 Ko/s.|Ajouter 1 disque (Total = 2)<br /><br />E/s sont aléatoires<br /><br />Taille de bloc de 4 Ko<br /><br />HD DE 10 000 TR/MIN|200 e/s total<br />Total de 800 Ko/s.| | | |
|Après avoir ajouté les 7 disques, la configuration de disque représente toujours le goulot d’étranglement à 3200 Ko/s.|**Ajouter des 7 disques (Total = 8)**  <br /><br />E/s sont aléatoires<br /><br />Taille de bloc de 4 Ko<br /><br />HD DE 10 000 TR/MIN|800 e/s total.<br />Nombre total de 3200 Ko/s| | | |
|Après avoir modifié les e/s à séquentiel, la carte réseau devient le goulot d’étranglement, car il est limité à 1 000 IOPS.|Ajouter des 7 disques (Total = 8)<br /><br />**E/s est séquentiel**<br /><br />Taille de bloc de 4 Ko<br /><br />HD DE 10 000 TR/MIN| | |2400 d’e/s s peuvent être lues/écrites sur le disque, limité à 1 000 IOPS du contrôleur| |
|Après avoir remplacé la carte réseau avec une carte SCSI qui prend en charge 10 000 e/s, le goulot d’étranglement retourne à la configuration de disque.|Ajouter des 7 disques (Total = 8)<br /><br />E/s sont aléatoires<br /><br />Taille de bloc de 4 Ko<br /><br />HD DE 10 000 TR/MIN<br /><br />**Mise à niveau de la carte SCSI (désormais prend en charge 10 000 e/s)**|800 e/s total.<br />Total de 3 200 Ko/s| | | |
|Après avoir augmenté la taille de bloc à 32 Ko, le bus devient le goulot d’étranglement, car il prend uniquement en charge 20 Mo/s.|Ajouter des 7 disques (Total = 8)<br /><br />E/s sont aléatoires<br /><br />**Taille de bloc de 32 Ko**<br /><br />HD DE 10 000 TR/MIN| |800 e/s total. 25,600 Ko/s (25 Mo/s) peuvent être lues/écrites sur le disque.<br /><br />Le bus prend uniquement en charge 20 Mo/s| | |
|Après la mise à niveau le bus et ajout de disques de plus, le disque reste le goulot d’étranglement.|**Ajouter des 13 disques (Total = 14)**<br /><br />Ajouter la deuxième carte SCSI avec 14 disques<br /><br />E/s sont aléatoires<br /><br />Taille de bloc de 4 Ko<br /><br />HD DE 10 000 TR/MIN<br /><br />**Mise à niveau vers 320 bus SCSI de Mo/s**|E/s 2800<br /><br />11,200 Ko/s (10.9 Mo/s)| | | |
|Après avoir modifié les e/s à séquentielles, le disque reste le goulot d’étranglement.|Ajouter des 13 disques (Total = 14)<br /><br />Ajouter la deuxième carte SCSI avec 14 disques<br /><br />**E/s est séquentiel**<br /><br />Taille de bloc de 4 Ko<br /><br />HD DE 10 000 TR/MIN<br /><br />Mise à niveau vers 320 bus SCSI de Mo/s|8,400 e/s<br /><br />KB\s 33 600<br /><br />(32,8 MB\s)| | | |
|Après l’ajout de disques durs plus rapides, le disque reste le goulot d’étranglement.|Ajouter des 13 disques (Total = 14)<br /><br />Ajouter la deuxième carte SCSI avec 14 disques<br /><br />E/s est séquentiel<br /><br />Taille de bloc de 4 Ko<br /><br />**HD DE 15 000 TR/MIN**<br /><br />Mise à niveau vers 320 bus SCSI de Mo/s|14 000 e/s<br /><br />56 000 Ko/s<br /><br />(54.7 Mo/s)| | | |
|Après avoir augmenté la taille de bloc à 32 Ko, le bus PCI devient le goulot d’étranglement.|Ajouter des 13 disques (Total = 14)<br /><br />Ajouter la deuxième carte SCSI avec 14 disques<br /><br />E/s est séquentiel<br /><br />**Taille de bloc de 32 Ko**<br /><br />HD DE 15 000 TR/MIN<br /><br />Mise à niveau vers 320 bus SCSI de Mo/s| | | |14 000 e/s<br /><br />448,000 Ko/s<br /><br />(437 Mo/s) est la limite en lecture/écriture à l’axe.<br /><br />Le bus PCI prend en charge un maximum théorique de 133 Mo/s (efficace au mieux de 75 %).|

### <a name="introducing-raid"></a>Présentation de RAID

La nature d’un sous-système de stockage ne change pas considérablement lors de l’introduction d’un contrôleur de baie ; Il remplace simplement la carte SCSI dans les calculs. Ce qui change est le coût de lecture et écriture de données sur le disque lors de l’utilisation de différents niveaux de tableau (telles que RAID 0, RAID 1 ou RAID 5).

Avec RAID 0, les données sont réparties sur l’ensemble des disques dans le RAID. Cela signifie que, lors d’une lecture ou une opération d’écriture, une partie des données est extraite ou transmise sur chaque disque, augmenter la quantité de données permettant de faire transiter le système au cours de la même période. Par conséquent, dans une seconde, sur chaque axe (à nouveau en supposant que les lecteurs de 10 000 tr/min), 100 opérations d’e/s peuvent être effectuées. La quantité totale d’e/s pouvant être pris en charge est piles N multiplié par 100 e/s par seconde par pile (rendements 100 * N e/s par seconde).

![Lecteur logique d:](media/capacity-planning-considerations-logical-d-drive.png)

En RAID 1, les données sont mise en miroir (dupliqué) entre une paire de piles pour assurer la redondance. Par conséquent, lorsqu’une opération d’e/s de lecture est effectuée, les données peuvent être lues à partir des deux axes dans le jeu. Cela fait effectivement la capacité d’e/s à partir de deux disques disponibles pendant une opération de lecture. L’inconvénient est que n’écriture gain d’opérations aucun avantage de performances dans un système RAID 1. Il s’agit, car les mêmes données doivent être écrites sur les deux disques pour des raisons de redondance. Même si elle ne prend pas plus long, comme l’écriture de données se produit simultanément sur les deux axes, car les deux axes sont occupés à dupliquer les données, une opération d’e/s d’écriture empêche essentiellement deux opérations de lecture ne se produise. Par conséquent, chaque écriture des coûts d’e/s de deux lecture d’e/s. Une formule peut être créée à partir de ces informations pour déterminer le nombre total d’opérations d’e/s qui se produisent :  

> *E/s en lecture* + 2 &times; *écrire les e/s* = *nombre Total d’e/s disponible sur le disque utilisé*  

Lorsque le ratio lectures et écritures, et le nombre d’axes sont connus, l’équation suivante peut être dérivée de l’équation ci-dessus pour identifier les e/s maximale pouvant être pris en charge par le tableau :  

> *Nombre maximal d’IOPS par pile* &times; 2 piles &times; [( *% des lectures* +  *% d’écritures*) &divide; ( *% des lectures* + 2 &times; *% d’écritures*)] = *nombre Total d’IOPS*

RAID 1 + 0, se comporte exactement comme système RAID 1 concernant les dépenses liées à la lecture et l’écriture. Toutefois, les e/s sont désormais réparties sur chaque jeu de mise en miroir. Si  

> *Nombre maximal d’IOPS par pile* &times; 2 piles &times; [( *% des lectures* +  *% d’écritures*) &divide; ( *% des lectures* + 2 &times; *% d’écritures*)] = *nombre Total d’e/s*  

dans un ensemble RAID 1, quand une multiplicité (*N*) de RAID 1 jeux sont agrégés par bande, les e/s Total qui peuvent être traités devient N &times; e/s par jeu de RAID 1 :  

> *N* &times; {*nombre maximal d’IOPS par pile* &times; 2 piles &times; [( *% des lectures* +  *% d’écritures*) &divide; ( *% Des lectures* + 2 &times; *% d’écritures*)]} = *nombre Total d’IOPS*

En RAID 5, parfois appelé *N* + 1 RAID, les données sont réparties sur *N* piles de disques et la parité informations sont écrites à l’axe « + 1 ». Toutefois, RAID 5 est beaucoup plus onéreuse lors de l’exécution d’une e/s d’écriture que RAID 1 ou 1 + 0. Chaque fois qu’une e/s d’écriture est soumis au tableau de RAID 5 sont le processus suivant :

1. Lire les données anciennes
1. Lire la parité ancien
1. Écrire les nouvelles données
1. Écrire la nouvelle parité

Comme chaque demande d’e/s d’écriture qui est soumis par le système d’exploitation pour le contrôleur de baie requiert quatre opérations d’e/s se termine, les demandes d’écriture soumis prennent quatre fois plus longs pour terminer, comme les lectures d’e/s. Pour dériver une formule pour traduire des requêtes d’e/s du point de vue de système d’exploitation à celle rencontrée par les axes :  

> *Lecture d’e/s* + 4 &times; *écriture d’e/s* = *nombre Total d’e/s*  

De même dans un ensemble RAID 1, lorsque le taux de lectures et écritures et le nombre de piles sont connus, l’équation suivante peut être dérivée de l’équation ci-dessus pour identifier les e/s maximale pouvant être pris en charge par le tableau (Notez que le nombre total d’axes n’inclut pas e la « lecteur » perdu à parité) :  

> *E/s par pile* &times; (*piles* – 1) &times; [( *% des lectures* +  *% d’écritures*) &divide; ( *% Des lectures* + 4 &times; *% d’écritures*)] = *nombre Total d’IOPS*

### <a name="introducing-sans"></a>Présentation des réseaux SAN

En développant la complexité du sous-système de stockage, lorsqu’un réseau SAN est introduit dans l’environnement, les principes de base décrites ne changent pas, cependant le comportement d’e/s pour tous les systèmes connectés au réseau SAN devant prendre en considération. L’un des principaux avantages à l’aide d’un réseau SAN est une quantité supplémentaire de redondance sur stockage attaché de façon interne ou externe, la planification de capacité doit à présent prendre en compte des besoins de tolérance de panne. En outre, plusieurs composants sont introduites qui doivent être évalués. Divise un réseau SAN en composants :

- Disque dur SCSI ou Fibre Channel.
- Unité de stockage panier
- Unités de stockage
- Module de contrôleur de stockage
- Ou les commutateurs de réseau SAN
- HBA(s)
- Le bus PCI

Lorsque vous concevez n’importe quel système pour la redondance, des composants supplémentaires sont inclus pour prendre en compte le risque de défaillance. Il est très important, lorsque Planifier la capacité, pour exclure le composant redondant de ressources disponibles. Par exemple, si le réseau SAN a deux modules de contrôleur, la capacité d’e/s du module d’un seul contrôleur est tout ce qui doit être utilisé pour le débit d’e/s total disponible pour le système. Cela est dû au fait que si un contrôleur échoue, toute la charge d’e/s demandée par tous les systèmes connectés doit être traitée par le contrôleur restant. Comme toute planification de capacité pour les périodes d’utilisation, les composants redondants ne doivent pas être factorisés dans les ressources disponibles et planifié des pics d’utilisation ne doivent pas dépasser 80 % de saturation du système (afin de prendre en charge les pics ou anormal du système comportement). De même, le commutateur, unité de stockage et piles de disques SAN redondant ne doivent pas être factorisés dans les calculs d’e/s.

Lorsque vous analysez le comportement du disque dur SCSI ou Fibre Channel, la méthode d’analyse du comportement, comme indiqué précédemment ne change pas. Bien qu’il existe certains avantages et des inconvénients de chaque protocole, le facteur de limitation sur une base par disque est la limitation mécanique du disque dur.

Analyse le canal sur l’unité de stockage est exactement le même que le calcul des ressources disponibles sur le bus SCSI ou de bande passante (par exemple, 20 Mo/s) divisée par la taille de bloc (par exemple, 8 Ko). Où cela s’éloigne de l’exemple précédent est dans l’agrégation de plusieurs canaux. Par exemple, s’il existe 6 canaux, chacun prenant en charge des taux de transfert maximal 20 Mo/s, la quantité totale de transfert des données et d’e/s qui est disponible est 100 Mo/s (cela est correct, il n’est pas 120 Mo/s). Là encore, une tolérance de panne est un acteur majeur dans ce calcul, en cas de perte d’un canal entier, le système est la seule gauche avec 5 canaux fonctionne. Par conséquent, pour vous assurer continuant à répondre aux attentes de performances en cas de défaillance, le débit total pour tous les canaux de stockage ne doit pas dépasser 100 Mo/s (Cela suppose que charge et tolérance de panne est répartie équitablement entre tous les canaux). L’activation de ce dans un profil d’e/s est dépendante sur le comportement de l’application. Dans le cas d’Active Directory Jet e/s, cela serait mettre en corrélation les environ 12 500 e/s par seconde (100 Mo/s &divide; 8 Ko par e/s).

Ensuite, l’obtention du fabricant pour les modules de contrôleur est requise afin de mieux comprendre le débit de que chaque module peut prendre en charge. Dans cet exemple, le réseau SAN a deux modules de contrôleur que la prise en charge de 7 500 e/s chaque. Le débit total du système peut être de 15 000 e/s si la redondance n’est pas souhaitée. Dans le calcul du débit maximal en cas de défaillance, la limitation est le débit d’un seul contrôleur, ou 7 500 e/s. Ce seuil est largement inférieur au maximum 12 500 e/s (en supposant que la taille de bloc de 4 Ko) pouvant être pris en charge par tous les canaux de stockage et par conséquent, est actuellement le goulot d’étranglement dans l’analyse. Toujours pour la planification, les e/s maximal souhaité planifier serait 10,400 e/s.

Lorsque les données termine le module de contrôleur, il passe d’une connexion Fibre Channel à 1 Go/s (ou 1 Gigabit par seconde). Pour cela mettre en corrélation avec les autres mesures, 1 Go/s transforme en 128 Mo/s (1 Go/s &divide; 8 bits/octet). Comme il s’agit dépassant la bande passante totale sur tous les canaux dans l’unité de stockage (100 Mo/s), cela ne sera pas goulot d’étranglement du système. En outre, comme il s’agit d’un seul des deux canaux (1 GBIT/s Fibre Channel une connexion supplémentaire en cours pour la redondance), si une connexion échoue, la connexion restante a toujours une capacité suffisante pour gérer tous les transferts de données demandées.

Routage vers le serveur, les données de transit sera très probablement un commutateur de réseau SAN. Comme le commutateur SAN doit traiter la requête d’e/s entrante et transférer au port approprié, le commutateur a une limite à la quantité d’e/s qui peut être gérée, toutefois, les spécifications de fabricants sera nécessaire pour déterminer ce qu’est cette limite. Par exemple, s’il existe deux commutateurs et chaque commutateur peut gérer 10 000 e/s, le débit total sera de 20 000 e/s. Là encore, tolérance de panne en cours d’un problème, si un seul commutateur échoue, le débit total du système seront 10 000 e/s. Comme il est nécessaire pour ne pas dépasser 80 % d’utilisation en fonctionnement normal, à l’aide de ne plus de 8 000 e/s doivent être la cible.

Enfin, l’adaptateur HBA installé sur le serveur aurait également une limite à la quantité d’e/s qu’il peut traiter. En règle générale, un deuxième adaptateur HBA est installé pour assurer la redondance, mais comme avec le commutateur de réseau SAN, lors du calcul du nombre maximal d’e/s qui peut être gérée, le débit total de *N* &ndash; HBA 1 est l’optimiser l’évolutivité du système.

### <a name="caching-considerations"></a>Considérations relatives à la mise en cache

Les caches sont un des composants qui peuvent affecter considérablement les performances globales à tout moment dans le système de stockage. Analyse détaillée sur les algorithmes de mise en cache est abordée dans cet article ; Toutefois, certaines instructions de base sur la mise en cache sur des sous-systèmes de disque méritent d’éclairage :

- La mise en cache d’e/s écriture séquentielle soutenu améliorées d’effectue tel qu’il peut nombreuses plus petites opérations d’écriture dans des blocs d’e/s plus importants de la mémoire tampon et annuler la copie intermédiaire vers le stockage dans les tailles de bloc moins, mais il est plus grand. Cela permet de réduire les e/s aléatoires total et total e/s séquentielles, par conséquent une plus grande disponibilité de ressources pour les autres d’e/s.
- La mise en cache n’améliore pas le débit d’e/s écriture soutenue du sous-système de stockage. Il autorise uniquement pour les écritures en mémoire tampon jusqu'à ce que les piles sont disponibles pour valider les données. Lorsque toutes les e/s disponibles des axes dans le sous-système de stockage est saturé pendant de longues périodes, le cache finalement à saturation. Pour vider le cache, suffisamment de temps entre les pics ou les piles de disques supplémentaires, devez être alloué afin de fournir suffisamment d’e/s pour autoriser le cache à vider.

  Caches supérieure n’autorisent davantage de données en mémoire tampon. Cela signifie que de plus longues périodes de saturation pouvant être utilisés.

  Dans un sous-système de stockage normalement d’exploitation, le système d’exploitation subit des meilleures performances d’écriture comme les données ne doivent être écrites dans le cache. Une fois que le média sous-jacent est saturé d’e/s, le cache remplira et performances d’écriture renvoie à la vitesse du disque.

- Lors de la mise en cache de lecture d’e/s, le scénario où le cache est plus avantageux est lorsque les données sont stockées de manière séquentielle sur le disque et le cache peut lecture anticipée (rend l’hypothèse que le secteur suivant contient les données qui seront demandées ensuite).
- Lors de la lecture e/s est aléatoire, la mise en cache au niveau du contrôleur de lecteur est peu probable fournir toute amélioration de la quantité de données qui peuvent être lues à partir du disque. Toute amélioration n’existe pas si le système d’exploitation ou la taille de cache basée sur l’application est supérieure à la taille de cache basée sur le matériel.

  Dans le cas d’Active Directory, le cache est uniquement limité par la quantité de RAM.

### <a name="ssd-considerations"></a>Considérations sur les disques SSD

Disques SSD est un animal complètement différent que basée sur l’axe des disques durs. Tout en laissant les deux critères clés : « Combien d’IOPS peut-elle gérer ? » et « Quelle est la latence pour les e/s ? » Par rapport à basée sur l’axe des disques durs, les disques SSD peut gérer des volumes importants d’e/s, peut avoir des latences réduites. En général à ce jour, tandis que les disques SSD est toujours coûteux dans une comparaison du coût par gigaoctet, ils sont très bon marchés en termes de coût-par-e/S et méritent votre attention significative en termes de performances de stockage.

Éléments à prendre en considération :

- E/s et latences sont très subjectives pour les conceptions de fabricant et dans certains cas ont été observés pour être performante inférieures à celles que les technologies en fonction de la broche. En bref, il est plus important examiner et valider les spécifications du fabricant par lecteur et n’assume pas n’importe quel generalities.
- Types d’e/s peuvent avoir des nombres très différents en fonction de si elle est en lecture ou écriture. En règle générale, les services AD DS, qui est principalement sur des lectures, sera moins touchés que certains autres scénarios d’application.
- « Résistance d’écriture » : il s’agit le concept des cellules SSD seront finalement usure prématurée. Divers fabricants occuper ce défi de différentes manières. Au moins pour le lecteur de base de données, le profil d’e/s majoritairement lecture permet downplaying la signification de ce problème, car les données ne sont pas hautement volatiles.

### <a name="summary"></a>Récapitulatif

Une façon de réfléchir à stockage est pour illustrer plomberie domestique. Imaginez les IOPS du support qui les données sont stockées sur la purge principale foyer. Lorsque cela est envahi (par exemple, les racines dans le canal) ou limited (il est réduit ou trop petit), tous les récepteurs dans la famille sauvegarder quand une trop grande quantité d’eau est en cours utilisé (trop d’invités). Cela est parfaitement analogue à un environnement partagé où un ou plusieurs systèmes tirent parti de stockage partagé sur un SAN/NAS/iSCSI avec le même média sous-jacent. Différentes approches vous peuvent entreprendre pour résoudre les différents scénarios :

- Un drainage réduit ou taille insuffisante nécessite un remplacement de la mise à l’échelle complet et le résoudre. Ceci serait similaire à l’ajout de nouveau matériel ou redistribuer les systèmes en utilisant le stockage partagé dans l’infrastructure.
- Un canal « artères » signifie généralement l’identification d’un ou plusieurs problèmes incriminées et suppression de ces problèmes. Dans un scénario de stockage est peut-être sauvegardes de niveau système ou de stockage, synchronisé des analyses antivirus sur tous les serveurs et en cours d’exécution logiciel défragmentation synchronisés pendant les périodes de pointe.

Dans toute conception de la plomberie, plusieurs diminue alimenter la purge principale. Si quoi que ce soit arrête un de ces diminue ou un point de jonction, les seuls éléments derrière cette jonction point sauvegarder. Dans un scénario de stockage, cela peut être un commutateur surchargé (scénario SAN/NAS/iSCSI), problèmes de compatibilité de pilote (mauvais pilote/HBA Firmware/storport.sys combinaison) ou la sauvegarde/antivirus/défragmentation. Pour déterminer si le « canal » de stockage est assez grand, taille d’e/s et d’e/s doit être mesurée. À chaque joint les ajouter ensemble pour garantir le bon niveau « diamètre de canal. »

## <a name="appendix-d---discussion-on-storage-troubleshooting---environments-where-providing-at-least-as-much-ram-as-the-database-size-is-not-a-viable-option"></a>Annexe D - Discussion sur la résolution des problèmes de stockage - environnements où fournissant au moins autant de mémoire RAM comme la taille de la base de données n’est pas une option viable

Il est utile de comprendre pourquoi ces recommandations existent afin que les modifications apportées à la technologie de stockage peuvent être prise en charge. Ces recommandations existent pour deux raisons. Le premier est l’isolation des e/s, telles que des problèmes de performances (qui est, la pagination) sur l’axe du système d’exploitation n’affectent pas les performances de la base de données et les profils d’e/s. Le deuxième est que les fichiers journaux pour les services AD DS (et la plupart des bases de données) sont séquentiels par nature et met en cache et basée sur des piles de disques durs ont un gain de performances énorme lorsqu’il est utilisé avec des e/s séquentielles par rapport aux schémas d’e/s plus aléatoires du système d’exploitation et presque lecteur de la base de données purement aléatoires modèles d’e/s de l’instance AD DS En isolant les e/s séquentielles vers un autre lecteur physique, le débit peut être augmenté. Le défi présenté par les options de stockage d’aujourd'hui est que les hypothèses fondamentales derrière ces recommandations ne sont plus trues. Dans de nombreux virtualisées stockage scénarios, tels qu’iSCSI, les fichiers d’image de disque virtuel SAN et NAS, media est partagé entre plusieurs hôtes de stockage sous-jacent, par conséquent complètement la négation « isolation d’e/s » et les aspects de « optimisation des e/s séquentielles ». En fait ces scénarios ajoutent une couche supplémentaire de complexité dans la mesure la réactivité au contrôleur de domaine peuvent dégrader les autres hôtes de l’accès au média partagé.

Dans la planification des performances de stockage, il existe trois catégories à prendre en compte : état du cache à froid, l’état du cache chauffés et sauvegarde/restauration. L’état du cache à froid se produit dans des scénarios tels que lorsque le contrôleur de domaine est initialement redémarré ou du redémarrage du service Active Directory et aucune donnée Active Directory dans la mémoire RAM. État du cache à chaud est où le contrôleur de domaine est dans un état stable et la base de données est mis en cache. Il est important de noter qu’ils détermineront les profils de performances très différentes, et avoir suffisamment de RAM pour mettre en cache de la base de données entière ne pas améliorer les performances lorsque le cache est à froid. On peut considérer la conception de performances pour les deux scénarios avec l’exemple suivant, préchauffer le cache à froid est un « sprint » et un serveur en cours d’exécution avec un cache à chaud est « marathon ».

Pour le cache à froid et le scénario de cache rempli, la question devient la vitesse à laquelle le stockage peut déplacer les données à partir du disque dans la mémoire. Préchauffage du cache est un scénario où, au fil du temps, les performances s’améliorent ainsi davantage de requêtes réutiliser les données, le cache atteint le taux augmente, diminue la fréquence de devoir accéder au disque. Par conséquent, l’impact négatif concernant les performances de leur disque diminue. Toute dégradation de performances est uniquement temporaire en attendant que le cache pour préparer et atteindre la taille autorisée maximale, dépendant du système. La conversation peut être simplifiée à la vitesse à laquelle les données peuvent être obtenues sur le disque et est une mesure simple des IOPS disponibles pour Active Directory, lequel doit subjective IOPS disponibles à partir du stockage sous-jacent. Du point de vue de la planification, car préchargement dans les scénarios de cache et de sauvegarde/restauration se produisent à titre exceptionnel, normalement se produire heures creuses et sont subjective à la charge du contrôleur de domaine, les recommandations générales n’existent pas, sauf dans la mesure où ces activités être planifiées pour les heures creuses.

AD DS, dans la plupart des scénarios, est essentiellement des lectures d’e/s, généralement un ratio de 90 % read/10% écriture. E/s en lecture tend souvent à être le goulot d’étranglement pour l’expérience utilisateur, et écriture d’e/s, les causes d’écrire une dégradation des performances. Comme les e/s NTDS.dit est principalement aléatoire, les caches ont tendance à fournir peu d’avantages pour la lecture d’e/s, en rendant qui beaucoup plus important de configurer le stockage pour lire d’e/s profil correctement.

Pour les conditions de fonctionnement normales, l’objectif de planification de stockage est minimiser les temps d’attente pour une demande à partir d’AD DS doit être retourné à partir du disque. Cela signifie essentiellement que le nombre d’en suspens et en attente d’e/s est inférieure ou équivalente au nombre de chemins de communication sur le disque. Il existe plusieurs façons pour mesurer ce. Dans un scénario d’analyse des performances, la recommandation générale consiste ce disque logique ( *\<lecteur de base de données NTDS\>* ) \Avg disque s/lecture est inférieure à 20 ms. Le seuil de fonctionnement souhaité doit être beaucoup plus faible, de préférence comme proche de la vitesse du stockage que possible, dans la plage de milliseconde de 2 à 6 (.002 à.006 seconde) en fonction du type de stockage.

Exemple :

![Graphique de latence de stockage](media/capacity-planning-considerations-storage-latency.png)

Analyse le graphique :

- **Ovale vert sur la gauche –** la latence reste cohérente à 10 ms. La charge augmente à partir de l’e/800 s à 2400 IOPS. Il s’agit de l’étage absolu à la vitesse à laquelle une demande d’e/s peut être traitée par le stockage sous-jacent. Il s’agit susceptibles d’être les spécificités de la solution de stockage.
- **Bandeau ovale sur la droite :** le débit reste plat à partir de la sortie du cercle vert via à la fin de la collecte de données tandis que la latence continue d’augmenter. Cela montre que lorsque les volumes de requêtes dépassent les limites physiques du stockage sous-jacent, plus les demandes passent assis dans la file d’attente d’envoi au sous-système de stockage.

Application de cette base de connaissances :

- **Impact sur un utilisateur interrogation appartenance à un grand groupe –** supposent cela nécessite la lecture de 1 Mo de données à partir du disque, la quantité d’e/s et la durée peut être évaluée comme suit :
  - Taille des pages de base de données actives Directory sont 8 Ko. 
  - Un minimum de 128 pages doivent être lues dans à partir du disque. 
  - En supposant que rien n’est mis en cache, à l’étage (10 ms) cela va prendre minimum 1,28 seconde pour charger les données à partir du disque afin de retourner au client. À 20 ms, où le débit sur le stockage a depuis longtemps atteint sa limite et est également le maximum recommandé, il prendra 2,5 secondes pour obtenir les données à partir du disque afin de renvoyer à l’utilisateur final.  
- **À quelle fréquence le cache être initialisée –** ce qui suppose que la charge client va optimiser le débit sur cet exemple de stockage, le cache sera à chaud à un taux d’e/s 2400 &times; 8 Ko par e/s. Ou bien, environ 20 Mo/s par second, le chargement environ 1 Go de base de données dans la mémoire vive toutes les 53 secondes.

> [!NOTE]
> Il est normal de courtes périodes observer les latences grimper lorsque les composants de manière agressive lire ou écrire sur le disque, telles que lorsque le système est en cours de sauvegarde ou lorsque les services AD DS s’exécute le garbage collection. Salle de tête supplémentaire par-dessus les calculs doit être fournie pour prendre en charge de ces événements périodiques. L’objectif étant de fournir suffisamment de débit pour prendre en charge ces scénarios sans affecter le fonctionnement normal.

Comme vous pouvez le constater, il existe une limite physique en fonction de la conception du stockage pour la vitesse à laquelle le cache peut éventuellement préparer. Ce que sera préchauffer le cache sont entrants demandes du client jusqu'à la vitesse à laquelle le stockage sous-jacent peut fournir. Exécution de scripts pour « préalable préparer » le cache pendant les heures de pointe fournira la compétition pour charger pilotées par les demandes client réel. Qui peut affecter défavorablement la fourniture des données que les clients doivent tout d’abord, car, par sa conception, il générera concurrence pour les ressources de disque rares comme tentatives artificiels pour préchauffer le cache chargera les données qui ne sont pas pertinentes pour les clients de contacter le contrôleur de domaine.
