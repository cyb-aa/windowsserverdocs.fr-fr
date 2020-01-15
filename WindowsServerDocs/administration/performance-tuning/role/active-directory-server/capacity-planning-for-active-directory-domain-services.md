---
title: Planification de la capacité pour Active Directory Domain Services
description: Présentation détaillée des facteurs à prendre en compte lors de la planification de la capacité pour AD DS.
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; kenbrunf
author: Teresa-Motiv
ms.date: 7/3/2019
ms.openlocfilehash: c1cad3242d3abf2838a5aaf71d21c68152bc9b7f
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75947278"
---
# <a name="capacity-planning-for-active-directory-domain-services"></a>Planification de la capacité pour Active Directory Domain Services

Cette rubrique a été rédigée à l’origine par Ken Brumfield, ingénieur de terrain principal chez Microsoft, et fournit des recommandations pour la planification de la capacité pour Active Directory Domain Services (AD DS).

## <a name="goals-of-capacity-planning"></a>Objectifs de la planification de la capacité

La planification de la capacité n’est pas identique à la résolution des incidents de performances. Elles sont étroitement liées, mais très différentes. Les objectifs de la planification de la capacité sont les suivants :  

- Implémenter et faire fonctionner correctement un environnement 
- Réduisez le temps consacré à la résolution des problèmes de performances.  
  
Dans la planification de la capacité, une organisation peut avoir une cible de référence de 40% d’utilisation du processeur pendant les périodes de pointe afin de répondre aux exigences de performances du client et de prendre en compte le temps nécessaire pour mettre à niveau le matériel dans le centre de ressources. Tandis qu’un seuil d’alerte de surveillance peut être défini à 90% sur un intervalle de 5 minutes, pour être averti des incidents de performances anormaux.

La différence réside dans le fait que lorsqu’un seuil de gestion de la capacité est continuellement dépassé (un événement à usage unique n’est pas un problème), l’ajout de capacité (c’est-à-dire l’ajout de processeurs plus ou plus rapides) serait une solution ou la mise à l’échelle du service sur plusieurs serveurs serait un elle. Les seuils d’alerte de performances indiquent que l’expérience client est actuellement affectée et que des étapes immédiates sont nécessaires pour résoudre le problème.

En guise d’analogie, la gestion de la capacité consiste à empêcher un accident de la voiture (une conduite défensive, à s’assurer que les freins fonctionnent correctement, etc.), tandis que la résolution des problèmes de performance concerne les opérations de la police, du service incendie et des professionnels de l’médecine d’urgence. après un accident. C’est à propos de la « conduite défensive », Active Directory.

Au cours des dernières années, les recommandations en matière de planification de la capacité pour les systèmes de montée en puissance ont considérablement changé. Les modifications suivantes dans les architectures système ont posé des hypothèses fondamentales sur la conception et la mise à l’échelle d’un service :

- plateformes de serveur 64 bits  
- Virtualisation  
- Plus grande attention à la consommation énergétique  
- Stockage SSD  
- Scénarios cloud  

En outre, l’approche passe d’un exercice de planification de la capacité basé sur un serveur à un exercice de planification de la capacité basé sur les services. Active Directory Domain Services (AD DS), un service distribué mature que de nombreux produits Microsoft et tiers utilisent comme backend, devient l’un des produits les plus importants à planifier correctement pour garantir la capacité nécessaire à l’exécution d’autres applications.

### <a name="baseline-requirements-for-capacity-planning-guidance"></a>Spécifications de base pour l’aide sur la planification de la capacité

Tout au long de cet article, les exigences de base suivantes sont attendues :

- Les lecteurs ont lu et sont familiarisés avec les [instructions de réglage des performances pour Windows Server 2012 R2](https://docs.microsoft.com/previous-versions//dn529133(v=vs.85)).
- La plate-forme Windows Server est une architecture basée sur x64. Toutefois, même si votre environnement de Active Directory est installé sur Windows Server 2003 x86 (maintenant au-delà de la fin du cycle de vie de support) et possède une arborescence d’informations de répertoire (DIT) d’une taille inférieure à 1,5 Go et pouvant être facilement conservées en mémoire, les instructions de cette l’article est toujours applicable.
- La planification de la capacité est un processus continu. vous devez régulièrement examiner la qualité des attentes de l’environnement.
- L’optimisation aura lieu sur plusieurs cycles de vie du matériel à mesure que les coûts matériels évoluent. Par exemple, la mémoire devient moins coûteuse, le coût par cœur diminue ou le prix des différentes options de stockage change.
- Planifiez la période de pic d’activité de la journée. Il est recommandé d’examiner cette valeur en intervalles de 30 minutes ou d’heures. Tout ce qui est plus important peut masquer les pics réels, et tout ce qui est moins peut être déformé en « pics temporaires ».
- Planifiez la croissance au cours du cycle de vie du matériel pour l’entreprise. Cela peut inclure une stratégie de mise à niveau ou d’ajout de matériel de manière échelonnée, ou une actualisation complète tous les trois à cinq ans. Chaque demande nécessitera une « estimation » du volume de la charge sur Active Directory augmentera. Les données d’historique, si elles sont collectées, sont utiles pour cette évaluation. 
- Planifier la tolérance de panne. Une fois qu’une estimation *n* est dérivée, planifiez les scénarios qui incluent *n* &ndash; 1, *n* &ndash; 2, *n* &ndash; *x*.
  - Ajoutez des serveurs supplémentaires selon les besoins de l’Organisation pour vous assurer que la perte d’un seul ou de plusieurs serveurs ne dépasse pas les estimations de capacité maximale.
  - Envisagez également que le plan de croissance et le plan de tolérance de panne doivent être intégrés. Par exemple, si un contrôleur de service est requis pour prendre en charge la charge, mais que la charge sera doublée au cours de l’année suivante et nécessitera deux contrôleurs de service, la capacité est insuffisante pour prendre en charge la tolérance de panne. La solution consisterait à démarrer avec trois contrôleurs de contrôleur. Vous pouvez également envisager d’ajouter le troisième contrôleur de périphérique après 3 ou 6 mois si les budgets sont serrés.

    >[!NOTE]
    >L’ajout d’applications prenant en charge les Active Directory peut avoir un impact notable sur la charge DC, que la charge provient des serveurs d’applications ou des clients.

### <a name="three-step-process-for-the-capacity-planning-cycle"></a>Processus en trois étapes pour le cycle de planification de la capacité

Dans planification de la capacité, commencez par déterminer la qualité de service nécessaire. Par exemple, un centre de données de base prend en charge un niveau plus élevé de simultanéité et nécessite une expérience plus cohérente pour les utilisateurs et les applications consommatrices, ce qui nécessite une attention toute particulière à la redondance et à la minimisation des goulots d’étranglement du système et de l’infrastructure. En revanche, un emplacement satellite avec quelques utilisateurs n’a pas besoin du même niveau de simultanéité d’accès concurrentiel ou de tolérance de panne. Ainsi, il se peut que le bureau satellite n’ait pas besoin d’une attention particulière à l’optimisation du matériel et de l’infrastructure sous-jacents, ce qui peut entraîner des économies. Toutes les recommandations et instructions fournies dans ce document permettent d’obtenir des performances optimales et peuvent être assouplies de manière sélective pour les scénarios avec des exigences moins exigeantes.

La question suivante est : virtualisé ou physique ? Du point de vue de la planification de la capacité, il n’y a aucune réponse correcte ou correcte ; Il n’existe qu’un ensemble différent de variables à utiliser. Les scénarios de virtualisation sont disponibles pour l’une des deux options suivantes :

- « Mappage direct » avec un invité par hôte (où la virtualisation existe uniquement pour extraire le matériel physique du serveur)
- « Hôte partagé »

Les scénarios de test et de production indiquent que le scénario « mappage direct » peut être traité de la même façon qu’un hôte physique. Toutefois, « hôte partagé » introduit un certain nombre de considérations plus en détail par la suite. Le scénario « hôte partagé » signifie que AD DS est également en concurrence pour les ressources, et qu’il existe des pénalités et des considérations relatives au paramétrage.

Avec ces considérations à l’esprit, le cycle de planification de la capacité est un processus itératif en trois étapes :

1. Mesurez l’environnement existant, déterminez où se trouvent les goulots d’étranglement du système et procurez-vous les principes de base nécessaires à la planification de la capacité nécessaire.
1. Déterminez le matériel nécessaire en fonction des critères indiqués à l’étape 1.
1. Surveiller et vérifier que l’infrastructure telle qu’elle est implémentée fonctionne dans des spécifications. Certaines données collectées au cours de cette étape constituent la ligne de base du cycle de planification de la capacité suivant.

### <a name="applying-the-process"></a>Application du processus

Pour optimiser les performances, assurez-vous que ces composants principaux sont correctement sélectionnés et réglés sur les charges de l’application :

1. Memory
1. réseau
1. Stockage
1. Processeur
1. Accès réseau

Les exigences de base en matière de stockage des AD DS et le comportement général d’un logiciel client bien écrit autorisent les environnements comportant jusqu’à 10 000 à 20 000 utilisateurs à renoncer à un investissement important dans la planification de la capacité en ce qui concerne le matériel physique, comme quasiment n’importe quel serveur moderne le système de classe gérera la charge. Cela dit, le tableau suivant résume comment évaluer un environnement existant afin de sélectionner le matériel approprié. Chaque composant est analysé en détail dans les sections suivantes pour aider AD DS administrateurs à évaluer leur infrastructure à l’aide de recommandations de base et de principaux propres à l’environnement.

En général :

- Tout dimensionnement basé sur les données actuelles ne sera exact que pour l’environnement actuel.
- Pour toute estimation, attendez-vous à ce que la demande augmente au-delà du cycle de vie du matériel.
- Déterminez s’il est important de dépasser la taille actuelle et augmentez la taille de l’environnement, ou ajoutez de la capacité sur le cycle de vie.
- Pour la virtualisation, les mêmes principes et méthodologies de planification de la capacité s’appliquent, à ceci près que la surcharge de la virtualisation doit être ajoutée à tout domaine lié.
- La planification de la capacité, comme tout ce qui tente de prédire, n’est pas une science exacte. Ne vous attendez pas à ce que les choses se calculent parfaitement et avec une précision de 100%. L’aide ici est la recommandation la plus épurée. Ajoutez de la capacité pour renforcer la sécurité et vérifiez en permanence que l’environnement reste sur la cible.

### <a name="data-collection-summary-tables"></a>Tables de résumé de la collecte de données

#### <a name="new-environment"></a>Nouvel environnement

| Component | Estimations |
|-|-|
|Taille du stockage/de la base de données|40 Ko à 60 Ko pour chaque utilisateur|
|Mémoire vive (RAM)|Taille de la base de données<br />Recommandations relatives au système d’exploitation de base<br />Applications tierces|
|réseau|1 Go|
|CPU|1000 utilisateurs simultanés pour chaque cœur|

#### <a name="high-level-evaluation-criteria"></a>Critères d’évaluation de haut niveau

| Component | Critères d’évaluation | Considérations de planification |
|-|-|-|
|Taille du stockage/de la base de données|La section intitulée « pour activer la journalisation de l’espace disque libéré par la défragmentation » dans les [limites de stockage](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10))| |
|Performances du stockage/de la base de données|<ul><li>«Disque logique ( *\<disque de\>base de données NTDS*) \Avg disque s/lecture», «disque logique ( *\<lecteur\>de base de données NTDS*) \Avg s/écriture», «disque logique ( *\<lecteur de base de données NTDS) )\>* \Avg disque s/transfert»</li><li>«Disque logique ( *\<lecteur\>de base de données NTDS*) \ lectures/s», «disque logique ( *\<lecteur\>de base de données NTDS*) \ écritures/s», «disque logique ( *\<lecteur\>debasededonnéesNTDS)* ) \ Transferts/s»</li></ul>|<ul><li>Le stockage a deux préoccupations à résoudre<ul><li>L’espace disponible, avec la taille du stockage basé sur les disques et le disque SSD d’aujourd’hui, est sans importance pour la plupart des environnements AD.</li> <li>Opérations d’entrée/sortie (e/s) disponibles : dans de nombreux environnements, cela est souvent négligé. Toutefois, il est important d’évaluer uniquement les environnements où il n’y a pas assez de RAM pour charger l’intégralité de la base de données NTDS en mémoire.</li></ul><li>Le stockage peut être un sujet complexe et doit impliquer l’expertise du fournisseur de matériel pour le dimensionnement approprié. Notamment avec des scénarios plus complexes tels que les scénarios SAN, NAS et iSCSI. Toutefois, en général, le coût par gigaoctet de stockage est souvent en opposition directe au coût par e/s :<ul><li>RAID 5 a un coût inférieur à 1 gigaoctet par rapport à RAID 1, mais RAID 1 a un coût réduit par e/s</li><li>Les disques durs basés sur des piles ont un coût réduit par gigaoctet, mais les disques SSD ont un coût inférieur par e/s</li></ul><li>Après un redémarrage de l’ordinateur ou du service Active Directory Domain Services, le cache ESE (Extensible Storage Engine) est vide et les performances sont limitées sur le disque pendant le préchauffage du cache.</li><li>Dans la plupart des environnements, AD utilise des e/s de lecture intensives dans un modèle aléatoire sur les disques, ce qui inverse la majeure partie des avantages de la mise en cache et des stratégies d’optimisation de lecture.  En outre, Active Directory dispose d’une mémoire cache plus grande que la plupart des caches du système de stockage.</li></ul>
|Mémoire vive (RAM)|<ul><li>Taille de la base de données</li><li>Recommandations relatives au système d’exploitation de base</li><li>Applications tierces</li></ul>|<ul><li>Le stockage est le composant le plus lent d’un ordinateur. Plus il est possible de résider dans la mémoire vive, moins il est nécessaire d’accéder au disque.</li><li>Assurez-vous que la RAM est suffisante pour stocker le système d’exploitation, les agents (antivirus, sauvegarde, surveillance), la base de données NTDS et la croissance dans le temps.</li><li>Pour les environnements où l’optimisation de la quantité de RAM n’est pas rentable (par exemple, les emplacements satellites) ou non faisable (DIT trop grand), référencez la section stockage pour vous assurer que le stockage est correctement dimensionné.</li></ul>|
|réseau|<ul><li>« Interface réseau (\*) \Octets reçus/s »</li><li>« Interface réseau (\*) \Octets envoyés/s »|<ul><li>En général, le trafic envoyé à partir d’un contrôleur de périphérique dépasse le trafic envoyé à un contrôleur de flux de contrôle.</li><li>Comme une connexion Ethernet commutée est en duplex intégral, le trafic réseau entrant et sortant doit être dimensionné indépendamment.</li><li>La consolidation du nombre de contrôleurs de contrôle augmentera la quantité de bande passante utilisée pour renvoyer les réponses aux demandes des clients pour chaque contrôleur de groupe, mais sera suffisamment proche de linéaire pour le site dans son ensemble.</li><li>Si vous supprimez des contrôleurs de l’emplacement satellites, n’oubliez pas d’ajouter la bande passante du contrôleur de réseau satellite dans les contrôleurs de flux de concentrateur, et de l’utiliser pour évaluer la quantité de trafic WAN.</li></ul>|
|CPU|<ul><li>« Disque logique ( *\<lecteur de base de données NTDS\>* ) \Avg disque s/lecture »</li><li>« Processus (LSASS)\\% temps processeur »</li></ul>|<ul><li>Après avoir éliminé le stockage comme un goulot d’étranglement, corrigez la quantité de puissance de calcul nécessaire.</li><li>Bien qu’il ne soit pas parfaitement linéaire, le nombre de cœurs de processeur consommés sur tous les serveurs au sein d’une étendue spécifique (par exemple, un site) peut être utilisé pour évaluer le nombre de processeurs nécessaires pour prendre en charge la charge totale du client. Ajoutez le minimum nécessaire pour maintenir le niveau de service actuel sur tous les systèmes de l’étendue.</li><li>Modifications de la vitesse du processeur, y compris les modifications liées à la gestion de l’alimentation, les numéros d’impact dérivés de l’environnement actuel. En règle générale, il est impossible d’évaluer précisément la manière dont le passage d’un processeur de 2,5 GHz à un processeur de 3 GHz réduira le nombre de processeurs nécessaires.</li></ul>|
|Accès réseau (Netlogon)|<ul><li>« Netlogon (\*) \Semaphore acquiert »</li><li>« Netlogon (\*) \Semaphore délais d’attente »</li><li>« Netlogon (\*) \Latence la durée d’attente du sémaphore »</li></ul>|<ul><li>Le canal sécurisé Net Logon/MaxConcurrentAPI affecte uniquement les environnements avec authentifications NTLM et/ou validation PAC. La validation PAC est activée par défaut dans les versions de système d’exploitation antérieures à Windows Server 2008. Il s’agit d’un paramètre client, de sorte que les contrôleurs de contrôle sont affectés jusqu’à ce que cette option soit désactivée sur tous les systèmes clients.</li><li>Les environnements avec une authentification croisée importante, qui comprend des approbations intra-forêts, présentent un risque plus élevé si leur taille n’est pas correcte.</li><li>Les consolidations de serveur augmentent la concurrence de l’authentification inter-approbation.</li><li>Les pics doivent être pris en charge, tels que les basculements de cluster, à mesure que les utilisateurs réauthentifient en massive sur le nouveau nœud de cluster.</li><li>Les systèmes clients individuels (par exemple, un cluster) peuvent également nécessiter un réglage.</li></ul>|

## <a name="planning"></a>Planification

Pendant une longue période, la recommandation de la Communauté concernant le dimensionnement AD DS a été de « placer dans autant de RAM que la taille de la base de données ». Pour l’essentiel, cette recommandation est tout ce que la plupart des environnements devaient se préoccuper. Toutefois, l’écosystème consommant AD DS a été beaucoup plus grand, comme les environnements AD DS eux-mêmes, depuis son introduction dans 1999. Bien que l’augmentation de la puissance de calcul et le passage des architectures x86 aux architectures x64 aient rendu les aspects plus subtils du dimensionnement des performances sans intérêt pour un plus grand ensemble de clients exécutant AD DS sur le matériel physique, la croissance de la virtualisation a réintroduit les problèmes de paramétrage pour un plus grand public qu’auparavant.

Les instructions suivantes expliquent par conséquent comment déterminer et planifier les demandes de Active Directory en tant que service, qu’elles soient déployées dans un environnement physique, une combinaison virtuelle/physique ou un scénario purement virtualisé. Par conséquent, nous décomposons l’évaluation de chacun des quatre composants principaux : stockage, mémoire, réseau et processeur. En bref, afin d’optimiser les performances sur AD DS, l’objectif est de se rapprocher de la limite du processeur.

## <a name="ram"></a>Mémoire vive (RAM)

Plus simplement, plus il est possible de mettre en cache la RAM, moins il est nécessaire d’accéder au disque. Pour optimiser l’évolutivité du serveur, la quantité minimale de RAM doit être la somme de la taille actuelle de la base de données, de la taille totale de SYSVOL, de la quantité recommandée pour le système d’exploitation et des recommandations des fournisseurs pour les agents (antivirus, surveillance, sauvegarde, etc.). ). Une quantité supplémentaire doit être ajoutée pour tenir compte de la croissance pendant la durée de vie du serveur. Il s’agit d’un environnement subjectiv basé sur les estimations de croissance de la base de données en fonction des modifications environnementales.

Pour les environnements où l’optimisation de la quantité de RAM n’est pas rentable (par exemple, les emplacements satellites) ou non faisable (DIT trop grand), référencez la section stockage pour vous assurer que le stockage est correctement conçu.

Un corollaire qui s’affiche dans le contexte général de la taille de la mémoire est le dimensionnement du fichier d’échange. Dans le même contexte que tout le reste de la mémoire, l’objectif est de réduire le nombre d’allers sur le disque beaucoup plus lent. Par conséquent, la question doit aller de « comment le fichier d’échange est-il dimensionné ? ». « quelle quantité de RAM est nécessaire pour réduire la pagination ? » La réponse à cette dernière question est décrite dans le reste de cette section. Cela laisse la majeure partie de la discussion pour le dimensionnement du fichier d’échange vers le domaine des recommandations générales du système d’exploitation et la nécessité de configurer le système pour les vidages de mémoire, qui ne sont pas liés aux performances de AD DS.

### <a name="evaluating"></a>Évaluation

La quantité de mémoire vive nécessaire à un contrôleur de domaine est en fait un exercice complexe pour les raisons suivantes :

- Risque élevé d’erreur lors de la tentative d’utilisation d’un système existant pour évaluer la quantité de RAM requise, car LSASS se découpera sous des conditions de sollicitation de la mémoire, en déflatant le besoin de manière artificielle.
- Le fait subjective qu’un contrôleur de périphérique individuel a uniquement besoin de mettre en cache ce qui est « intéressant » pour ses clients. Cela signifie que les données qui doivent être mises en cache sur un contrôleur de site d’un site avec un seul serveur Exchange sont très différentes des données qui doivent être mises en cache sur un contrôleur de site qui n’authentifie que les utilisateurs.
- Le travail d’évaluation de la RAM pour chaque contrôleur de périphérique au cas par cas est prohibitif et change au fur et à mesure que l’environnement change.
- Les critères à l’appui de la recommandation vous aideront à prendre des décisions informées : 
- Plus il est possible de mettre en cache dans la mémoire vive, moins il est nécessaire d’accéder au disque. 
- Le stockage est de loin le composant le plus lent d’un ordinateur. L’accès aux données sur les supports de stockage SSD et basés sur les piles se fait de l’ordre de 1 000 000 fois plus lentement que l’accès aux données de la RAM.

Ainsi, afin d’optimiser l’évolutivité du serveur, la quantité minimale de RAM est la somme de la taille de la base de données actuelle, de la taille totale de SYSVOL, de la quantité recommandée du système d’exploitation et des recommandations des fournisseurs pour les agents (antivirus, surveillance, sauvegarde, et ainsi de suite). Ajoutez des montants supplémentaires pour prendre en charge la croissance pendant la durée de vie du serveur. Il s’agit d’un environnement subjectiv basé sur les estimations de croissance de la base de données. Toutefois, pour les emplacements satellites avec un petit ensemble d’utilisateurs finaux, ces exigences peuvent être assouplies, car ces sites n’ont pas besoin de mettre en cache une grande partie des demandes.

Pour les environnements où l’optimisation de la quantité de RAM n’est pas rentable (par exemple, les emplacements satellites) ou non faisable (DIT trop grand), référencez la section stockage pour vous assurer que le stockage est correctement dimensionné.

> [!NOTE]
> Un corollaire pendant la redimensionnement de la mémoire est le dimensionnement du fichier d’échange. Étant donné que l’objectif est de réduire le nombre d’accès au disque beaucoup plus lent, la question est la suivante : « comment le fichier d’échange est-il dimensionné ? ». « quelle quantité de RAM est nécessaire pour réduire la pagination ? » La réponse à cette dernière question est décrite dans le reste de cette section. Cela laisse la majeure partie de la discussion pour le dimensionnement du fichier d’échange vers le domaine des recommandations générales du système d’exploitation et la nécessité de configurer le système pour les vidages de mémoire, qui ne sont pas liés aux performances de AD DS.

### <a name="virtualization-considerations-for-ram"></a>Considérations relatives à la virtualisation pour la RAM

Évitez la validation de la mémoire sur l’hôte. L’objectif fondamental de l’optimisation de la quantité de RAM est de réduire le temps passé sur le disque. Dans les scénarios de virtualisation, le concept de validation sur la mémoire existe où une plus grande RAM est allouée aux invités, alors qu’elle existe sur l’ordinateur physique. Cela ne pose pas de problème. Cela pose un problème lorsque la mémoire totale utilisée activement par tous les invités dépasse la quantité de RAM sur l’ordinateur hôte et que l’hôte sous-jacent démarre la pagination. Les performances deviennent liées au disque dans les cas où le contrôleur de domaine va accéder à NTDS. dit pour récupérer des données, ou le contrôleur de domaine va accéder au fichier d’échange pour récupérer les données, ou l’hôte va sur le disque pour recevoir les données que l’invité pense se trouver dans la mémoire vive.

### <a name="calculation-summary-example"></a>Exemple de résumé du calcul

|Component|Mémoire estimée (exemple)|
|-|-|
|Mémoire vive recommandée pour le système d’exploitation (Windows Server 2008)|2 Go|
|Tâches internes LSASS|200 Mo|
|Agent de surveillance|100 Mo|
|Intégration antivirus|100 Mo|
|Base de données (catalogue global)|8,5 Go êtes-vous sûr ???|
|Coussin pour la sauvegarde à exécuter, les administrateurs qui se connectent sans impact|1 Go|
|Total|12 Go|

**Recommandé : 16 Go**

Au fil du temps, il peut être fait que davantage de données seront ajoutées à la base de données et que le serveur sera probablement en production pendant 3 à 5 ans. Sur la base d’une estimation de la croissance de 33%, 16 Go seraient une quantité raisonnable de RAM à placer sur un serveur physique. Sur une machine virtuelle, compte tenu de la facilité avec laquelle les paramètres peuvent être modifiés et que la RAM peut être ajoutée à la machine virtuelle, en commençant à 12 Go avec le plan à surveiller et à mettre à niveau à l’avenir est raisonnable.

## <a name="network"></a>réseau

### <a name="evaluating"></a>Évaluation
Cette section est moins axée sur l’évaluation des demandes relatives au trafic de réplication, qui se concentre sur le trafic qui traverse le WAN et qui est traitée en détail dans [Active Directory le trafic de réplication](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/bb742457(v=technet.10)), par rapport à l’évaluation de la bande passante totale et de la capacité réseau nécessaire, des requêtes clientes, des applications stratégie de groupe, etc. Pour les environnements existants, vous pouvez les collecter à l’aide des compteurs de performance «\*interface réseau () \Octets reçus/s» et «interface(\*)réseau  \Octets envoyés/s». Intervalles d’échantillonnage des compteurs d’interface réseau en 15, 30 ou 60 minutes. Tout ce qui est moins est généralement trop volatil pour les mesures appropriées ; tout ce qui est plus important lissera les aperçus quotidiens de manière excessive.

> [!NOTE]
> En règle générale, la majeure partie du trafic réseau sur un contrôleur de réseau est sortante, car le contrôleur de réseau répond aux requêtes du client. Il s’agit de la raison pour laquelle le trafic sortant est concentré, bien qu’il soit recommandé d’évaluer chaque environnement pour le trafic entrant également. Les mêmes approches peuvent être utilisées pour résoudre et examiner les exigences en matière de trafic réseau entrant. Pour plus d’informations, consultez l’article 929851 de la base de connaissances [: la plage de ports dynamiques par défaut pour TCP/IP a changé dans Windows Vista et dans Windows Server 2008](https://support.microsoft.com/kb/929851).

### <a name="bandwidth-needs"></a>Besoins en bande passante

La planification de l’extensibilité du réseau couvre deux catégories distinctes : la quantité de trafic et la charge de l’UC à partir du trafic réseau. Chacun de ces scénarios est simple par rapport à certaines des autres rubriques de cet article.

Pour évaluer la quantité de trafic qui doit être prise en charge, il existe deux catégories uniques de planification de la capacité pour AD DS en termes de trafic réseau. Le premier est le trafic de réplication qui traverse les contrôleurs de domaine et qui est traité minutieusement dans la référence [Active Directory le trafic de réplication](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/bb742457(v=technet.10)) et qui est toujours pertinent pour les versions actuelles de AD DS. Le deuxième est le trafic entre le client et le serveur intrasite. L’un des scénarios les plus simples à planifier, le trafic intrasite reçoit principalement de petites requêtes des clients par rapport aux grandes quantités de données renvoyées aux clients. 100 Mo sont généralement appropriés dans les environnements jusqu’à 5 000 utilisateurs par serveur, dans un site. L’utilisation d’une carte réseau de 1 Go et la prise en charge de la mise à l’échelle côté réception (RSS) sont recommandées pour tous les utilisateurs au-delà de 5 000. Pour valider ce scénario, en particulier dans le cas de scénarios de consolidation de serveurs, examinez l’interface réseau (\*) \ octets/s sur tous les contrôleurs de domaine d’un site, ajoutez-les ensemble et divisez par le nombre cible de contrôleurs de domaine pour vous assurer qu’il existe une capacité adéquate. Le moyen le plus simple consiste à utiliser la vue « zone empilée » dans l’analyseur de fiabilité et de performances Windows (anciennement Perfmon), en veillant à ce que tous les compteurs soient mis à l’échelle de la même façon.

Prenons l’exemple suivant (également connu sous le nom de «méthode vraiment très complexe pour valider que la règle générale s’applique à un environnement spécifique). Les hypothèses suivantes sont effectuées :

- L’objectif est de réduire le nombre de serveurs possible. Dans l’idéal, un serveur contiendra la charge et un serveur supplémentaire est déployé pour la redondance (scénario *N* + 1). 
- Dans ce scénario, la carte réseau actuelle prend en charge uniquement 100 Mo et se trouve dans un environnement commuté.  
  L’utilisation maximale de la bande passante du réseau cible est de 60% dans un scénario N (perte d’un contrôleur de réseau).
- Environ 10 000 clients sont connectés à chaque serveur.

Connaissances obtenues à partir des données du graphique (interface réseau (\*) \Octets envoyés/s) :

1. Le jour ouvrable commence à 5:30 et est incrémenté à 7:00 h 00.
1. La période de pointe la plus chargée est comprise entre 8:00 et 8:15 AM, avec plus de 25 octets envoyés/s sur le contrôleur de périphérique le plus occupé.  
   > [!NOTE]
   > Toutes les données de performances sont historiques. Le point de données de pointe à 8:15 indique donc la charge de 8:00 à 8:15.
1. Il y a des pics avant 4:00 AM, avec plus de 20 octets envoyés/s sur le DC le plus occupé, ce qui peut indiquer une charge à partir de différents fuseaux horaires ou d’une activité d’infrastructure d’arrière-plan, tels que des sauvegardes. Étant donné que le pic à 8:00 AM dépasse cette activité, il n’est pas pertinent.
1. Il existe cinq contrôleurs de domaine dans le site.
1. La charge maximale est d’environ 5,5 Mo/s par DC, ce qui représente 44% de la connexion de 100 Mo. À l’aide de ces données, il peut être estimé que la bande passante totale nécessaire entre 8:00 AM et 8:15 AM est de 28 Mo/s.
   >[!NOTE]
   >Soyez attentif au fait que les compteurs d’interface réseau envoyés/reçus sont en octets et que la bande passante réseau est mesurée en bits. 100 Mo &divide; 8 = 12,5 Mo, 1 Go &divide; 8 = 128 Mo.
  
Conclusions

1. Cet environnement actuel est conforme au niveau N + 1 de tolérance de panne à 60% d’utilisation cible. La mise hors connexion d’un système déplace la bande passante par serveur d’environ 5,5 Mo/s (44%) à environ 7 Mo/s (56%).
1. En fonction de l’objectif précédent de la consolidation sur un serveur, les deux dépassent l’utilisation maximale de la cible et, théoriquement, l’utilisation possible d’une connexion de 100 Mo.
1. Avec une connexion de 1 Go, cela représente 22% de la capacité totale.
1. Dans des conditions de fonctionnement normales du scénario *N* + 1, la charge du client sera distribuée de manière proportionnelle à environ 14 Mo/s par serveur ou à 11% de la capacité totale.
1. Pour garantir que la capacité est suffisante lors de l’indisponibilité d’un contrôleur de réseau, les cibles de fonctionnement normales par serveur seraient environ 30% d’utilisation du réseau ou 38 Mo/s par serveur. Les cibles de basculement sont de 60% d’utilisation du réseau ou de 72 Mo/s par serveur.  
  
En bref, le déploiement final de systèmes doit avoir une carte réseau de 1 Go et être connecté à une infrastructure réseau qui prend en charge cette charge. Une autre remarque est que compte tenu de la quantité de trafic réseau générée, la charge du processeur à partir des communications réseau peut avoir un impact significatif et limiter l’extensibilité maximale de AD DS. Ce même processus peut être utilisé pour estimer la quantité de communication entrante vers le contrôleur de l’État. Mais étant donné la prédominance du trafic sortant par rapport au trafic entrant, il s’agit d’un exercice universitaire pour la plupart des environnements. Il est important de s’assurer que la prise en charge du matériel pour RSS est importante dans les environnements comportant plus de 5 000 utilisateurs par serveur. Pour les scénarios avec un trafic réseau élevé, l’équilibrage de la charge d’interruption peut être un goulot d’étranglement. Cela peut être détecté par le processeur (\*)\% temps d’interruption distribué de manière inégale entre les UC. Les cartes réseau activées pour RSS peuvent atténuer cette limitation et accroître l’évolutivité.

> [!NOTE]
> Une approche similaire peut être utilisée pour estimer la capacité supplémentaire nécessaire lors de la consolidation de centres de données ou le retrait d’un contrôleur de domaine dans un emplacement satellite. Il vous suffit de collecter le trafic sortant et entrant vers les clients, qui sera la quantité de trafic qui sera présent sur les liaisons de réseau étendu.  
>  
> Dans certains cas, il se peut que vous rencontriez plus de trafic que prévu, car le trafic est plus lent, par exemple lorsque la vérification du certificat ne répond pas à des délais d’attente agressifs sur le réseau étendu. C’est la raison pour laquelle le dimensionnement et l’utilisation des réseaux étendus doivent être un processus itératif et continu.

### <a name="virtualization-considerations-for-network-bandwidth"></a>Considérations relatives à la virtualisation pour la bande passante réseau

Il est facile de formuler des recommandations pour un serveur physique : 1 Go pour les serveurs prenant en charge plus de 5000 utilisateurs. Une fois que plusieurs invités commencent à partager une infrastructure de commutateur virtuel sous-jacent, une attention supplémentaire est nécessaire pour s’assurer que l’ordinateur hôte dispose de la bande passante réseau adéquate pour prendre en charge tous les invités sur le système, et nécessite donc la rigueur supplémentaire. Il n’y a rien de plus qu’une extension de la garantie de l’infrastructure réseau dans l’ordinateur hôte. Il s’agit du fait que le réseau est inclusif et que le contrôleur de domaine s’exécute en tant qu’invité d’ordinateur virtuel sur un ordinateur hôte dont le trafic réseau passe par un commutateur virtuel, ou s’il est connecté directement à un commutateur physique. Le commutateur virtuel n’est qu’un composant supplémentaire dans lequel la liaison montante doit prendre en charge la quantité de données transmises. Par conséquent, la carte réseau physique de l’hôte physique liée au commutateur doit pouvoir prendre en charge la charge DC, ainsi que tous les autres invités partageant le commutateur virtuel connecté à la carte réseau physique.

### <a name="calculation-summary-example"></a>Exemple de résumé du calcul

|Système|Bande passante maximale|
|-|-|
DC 1|6,5 Mo/s|
DC 2|6,25 Mo/s|
|DC 3|6,25 Mo/s|
|DC 4|5,75 Mo/s|
|DC 5|4,75 Mo/s|
|Total|28,5 Mo/s|

**Recommandé : 72 Mo/s** (28,5 Mo/s divisé par 40%)

|Nombre de systèmes (s) cible (s)|Bande passante totale (ci-dessus)|
|-|-|
|2|28,5 Mo/s|
|Comportement normal résultant|28,5 &divide; 2 = 14,25 Mo/s|

Comme toujours, dans le temps, il est possible de faire en sorte que la charge du client augmente et que cette croissance soit planifiée le mieux possible. La quantité recommandée pour la planification permettrait une croissance estimée du trafic réseau de 50%.

## <a name="storage"></a>Stockage

La planification du stockage est constituée de deux composants :

- Capacité ou taille de stockage
- Performances

Une grande quantité de temps et de la documentation sont consacrés à la planification de la capacité, ce qui rend souvent les performances entièrement négligées. Avec les coûts matériels actuels, la plupart des environnements ne sont pas suffisamment importants pour que l’un d’entre eux soit en fait un problème, et la recommandation de « placer dans autant de RAM que la taille de la base de données » couvre généralement le reste, bien qu’il puisse être excessif pour les emplacements satellites dans une plus grande taille environnements.

### <a name="sizing"></a>Dimensionnement

#### <a name="evaluating-for-storage"></a>Évaluation pour le stockage

Il y a environ 13 ans lorsque Active Directory a été introduite, une fois que les lecteurs de 4 Go et 9 Go étaient les plus courants, le dimensionnement pour les Active Directory n’est même pas une considération pour tous les environnements sauf les plus importants. Avec les plus petites tailles de disque dur disponibles dans la plage de 180 Go, l’ensemble du système d’exploitation, de SYSVOL et de NTDS. dit peut facilement tenir sur un lecteur. Par conséquent, il est recommandé de déprécier les investissements importants dans ce domaine.

La seule recommandation à prendre en compte est de s’assurer que 110% de la taille de NTDS. dit est disponible pour activer la défragmentation. En outre, les logements pour la croissance pendant la durée de vie du matériel doivent être pris en charge.

La première considération et la plus importante consiste à évaluer la taille de NTDS. dit et SYSVOL. Ces mesures vont entraîner le dimensionnement du disque fixe et de l’allocation de RAM. En raison du faible coût (relativement) de ces composants, les mathématiques n’ont pas besoin d’être rigoureuses et précises. Vous trouverez des informations sur la façon d’évaluer cela pour les environnements existants et nouveaux dans la série d’articles sur le [stockage de données](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961771(v=technet.10)) . Plus précisément, reportez-vous aux articles suivants :

- **Pour les environnements existants &ndash;** La section intitulée « pour activer la journalisation de l’espace disque libéré par la défragmentation » dans l’article [limites de stockage](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10)).
- **Pour les nouveaux environnements &ndash;** L’article a intitulé [estimations de croissance pour les utilisateurs Active Directory et les unités d’organisation](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961779(v=technet.10)).

  > [!NOTE]
  > Les articles sont basés sur les estimations de la taille des données effectuées au moment de la publication de Active Directory dans Windows 2000. Utilisez des tailles d’objet qui reflètent la taille réelle des objets dans votre environnement.

Lorsque vous examinez des environnements existants avec plusieurs domaines, il peut y avoir des variations de la taille des bases de données. Si c’est le cas, utilisez le catalogue global (GC) le plus petit et les tailles non GC.  

La taille de la base de données peut varier selon les versions de système d’exploitation. Les contrôleurs de réseau qui exécutent des systèmes d’exploitation antérieurs, tels que Windows Server 2003, ont une taille de base de données inférieure à celle d’un contrôleur de réseau qui exécute un système d’exploitation ultérieur, tel que Windows Server 2008 R2, en particulier lorsque des fonctionnalités telles Active Directory la Corbeille ou les informations d’identification itinérantes sont activées.

> [!NOTE]  
  >
>- Pour les nouveaux environnements, Notez que les estimations des estimations de croissance pour Active Directory utilisateurs et les unités d’organisation indiquent que 100 000 utilisateurs (dans le même domaine) consomment environ 450 Mo d’espace. Notez que les attributs remplis peuvent avoir un impact énorme sur la quantité totale. Les attributs seront remplis sur de nombreux objets par des produits tiers et des produits Microsoft, y compris Microsoft Exchange Server et Lync. Une évaluation basée sur le portefeuille des produits de l’environnement est préférable, mais l’exercice de la description des mathématiques et des tests pour obtenir des estimations précises pour tous les environnements, sauf les plus importants, peut ne pas réellement avoir de temps et d’efforts significatifs.
>- Assurez-vous que 110% de la taille de NTDS. dit est disponible en tant qu’espace libre afin d’activer la défragmentation hors connexion et de planifier la croissance sur une durée de vie matérielle de trois à cinq ans. Étant donné le stockage économique, l’estimation du stockage à 300% de la taille de la DIT, car l’allocation de stockage est sécurisée pour prendre en charge la croissance et la nécessité potentielle de défragmentation hors connexion.

#### <a name="virtualization-considerations-for-storage"></a>Considérations relatives à la virtualisation pour le stockage

Dans un scénario où plusieurs fichiers de disque dur virtuel (VHD) sont alloués sur un seul volume, utilisez un disque de taille fixe d’au moins 210% (100% de l’espace libre de DIT + 110%) la taille de la DIT pour s’assurer qu’il y a suffisamment d’espace réservé.  

#### <a name="calculation-summary-example"></a>Exemple de résumé du calcul

|Données collectées à partir de la phase d’évaluation| |
|-|-|
|Taille de NTDS. dit|35 Go|
|Modificateur permettant la défragmentation hors connexion|2.1|
|Stockage total requis|73,5 GO|

> [!NOTE]
> Ce stockage est nécessaire en plus du stockage requis pour SYSVOL, le système d’exploitation, le fichier d’échange, les fichiers temporaires, les données en mémoire cache locale (telles que les fichiers du programme d’installation) et les applications.

### <a name="storage-performance"></a>Performances de stockage

#### <a name="evaluating-performance-of-storage"></a>Évaluation des performances du stockage

En tant que composant le plus lent au sein d’un ordinateur, le stockage peut avoir un impact négatif sur l’expérience du client. Pour les environnements suffisamment grands pour lesquels les recommandations de dimensionnement de la RAM ne sont pas réalisables, les conséquences de la surRecherche de la planification du stockage pour les performances peuvent être dévastatrices.  En outre, les complexités et les variétés de la technologie de stockage augmentent le risque d’échec, car la pertinence des meilleures pratiques les plus longues du « placement du système d’exploitation, des journaux et de la base de données » sur des disques physiques distincts est limitée dans les scénarios utiles.  Cela est dû au fait que la meilleure pratique la plus courante repose sur l’hypothèse qu’un « disque » est une pile dédiée, et que ces e/s autorisées sont isolées.  Les hypothèses qui font ce vrai ne sont plus pertinentes avec l’introduction de :

- Nouveaux types de stockage et scénarios de stockage partagé et virtualisé
- Piles partagées sur un réseau de zone de stockage (SAN)
- Fichier VHD sur un réseau SAN ou un stockage connecté au réseau
- Disques SSD
- Architectures de stockage à plusieurs niveaux (par exemple, la mise en cache du niveau de stockage SSD avec un stockage basé sur un axe plus important)

En bref, l’objectif final de tous les efforts en matière de performances de stockage, indépendamment de l’architecture et de la conception de stockage sous-jacents, est de garantir que la quantité nécessaire d’opérations d’entrée/sortie par seconde (IOPS) est disponible et que ces IOPS se produisent dans un laps de temps acceptable. Cette section explique comment évaluer les demandes AD DS du stockage sous-jacent afin de garantir que les solutions de stockage sont correctement conçues.  Étant donné la variabilité des technologies de stockage d’aujourd’hui, il est préférable de travailler avec les fournisseurs de stockage pour garantir des e/s par seconde adéquates.  Pour les scénarios avec stockage attaché localement, reportez-vous à l’annexe C pour connaître les principes de base de la conception de scénarios de stockage local traditionnels.  Ces principaux s’appliquent généralement à des niveaux de stockage plus complexes et sont également utiles dans la boîte de dialogue avec les fournisseurs qui prennent en charge les solutions de stockage backend.

- Compte tenu de la large éventail d’options de stockage disponibles, il est recommandé de faire appel à l’expertise des équipes de support matériel ou des fournisseurs pour s’assurer que la solution spécifique répond aux besoins de AD DS. Les numéros suivants sont les informations qui seraient fournies aux spécialistes du stockage.

Pour les environnements dans lesquels la base de données est trop volumineuse pour être conservée dans la mémoire RAM, utilisez les compteurs de performance pour déterminer la quantité d’e/s qui doit être prise en charge :

- Disque logique (\*) \Avg disque s/lecture (par exemple, si NTDS. dit est stocké sur le lecteur D:/ le chemin d’accès complet est disque logique (D :) \Avg disque s/lecture)
- Disque logique (\*) \Avg disque s/écriture
- Disque logique (\*) \Avg disque s/transfert
- Disque logique (\*) \ lectures/s
- Disque logique (\*) \ écritures/s
- Disque logique (\*) \ transferts/s

Ils doivent être échantillonnés par intervalles de 15/30/60 minutes pour évaluer les demandes de l’environnement actuel.

#### <a name="evaluating-the-results"></a>Évaluation des résultats

> [!NOTE]
> L’accent est mis sur les lectures de la base de données, car il s’agit généralement du composant le plus exigeant, la même logique peut être appliquée aux écritures dans le fichier journal en remplaçant le disque logique ( *\<NTDS log\>* ) \Avg Disk s/Write and LogicalDisk (*Journal\>NTDS) \ écritures/s): \<*
>  
> - Disque logique *\<(\>NTDS*) \Avg s/lecture indique si le stockage actuel est correctement dimensionné.  Si les résultats sont à peu près égaux au temps d’accès au disque pour le type de disque, le disque logique ( *\<NTDS\>* ) \ lectures/s est une mesure valide.  Vérifiez les spécifications du fabricant pour le stockage sur le back end, mais les plages correctes pour disque logique ( *\<NTDS\>* ) \Avg disque s/lecture seraient à peu près :
>   - 7200 – 9 à 12,5 millisecondes (MS)
>   - 10 000 – 6 à 10 ms
>   - 15 000 – 4 à 6 ms  
>   - SSD – 1 à 3 ms  
>   - >[!NOTE]
>     >Il existe des recommandations indiquant que les performances de stockage sont dégradées de 15ms à 20 ms (selon la source).  La différence entre les valeurs ci-dessus et les autres recommandations est que les valeurs ci-dessus correspondent à la plage de fonctionnement normale.  Les autres recommandations sont des conseils de dépannage permettant d’identifier le moment où l’expérience client se dégrade de manière significative et devient perceptible.  Pour une explication plus détaillée, consultez l’annexe C.
> - Disque logique ( *\<NTDS\>* ) \ lectures/s correspond à la quantité d’e/s en cours d’exécution.
>   - Si le disque logique ( *\<NTDS\>* ) \Avg disque s/lecture se trouve dans la plage optimale pour le stockage principal, le disque logique ( *\<\>NTDS* ) \ lectures/s peut être utilisé directement pour dimensionner le stockage.
>   - Si le disque logique ( *\<NTDS\>* ) \Avg disque s/lecture ne fait pas partie de la plage optimale pour le stockage principal, des e/s supplémentaires sont nécessaires en fonction de la formule suivante :
>     > (Disque logique ( *\<ntds\>* ) \Avg disque s/lecture) &divide; (temps d’accès au disque du média physique) &times; (disque logique ( *\<NTDS\>* ) \Avg disque s/lecture)

Éléments à prendre en considération :

- Notez que, si le serveur est configuré avec une quantité de RAM inférieure, ces valeurs ne sont pas exactes à des fins de planification.  Elles seront erronées sur le haut et peuvent toujours être utilisées dans le pire des cas.
- L’ajout ou l’optimisation de la mémoire RAM entraînera une baisse de la quantité d’e/s de lecture (disque logique ( *\<NTDS\>* ) \ lectures/s.  Cela signifie que la solution de stockage ne doit pas nécessairement être aussi robuste que celle initialement calculée.  Malheureusement, tout ce qui est plus spécifique que cette déclaration générale dépend de l’environnement de la charge du client et les recommandations générales ne peuvent pas être fournies.  La meilleure option consiste à ajuster le dimensionnement du stockage après l’optimisation de la RAM.

#### <a name="virtualization-considerations-for-performance"></a>Considérations relatives à la virtualisation pour les performances

À l’instar de toutes les discussions de virtualisation précédentes, la clé ici consiste à s’assurer que l’infrastructure partagée sous-jacente peut prendre en charge la charge DC et les autres ressources à l’aide du support partagé sous-jacent et de tous les chemins vers celui-ci. C’est le cas, qu’un contrôleur de domaine physique partage le même média sous-jacent sur une infrastructure SAN, NAS ou iSCSI comme d’autres serveurs ou applications, qu’il s’agisse d’un invité utilisant un accès direct à une infrastructure SAN, NAS ou iSCSI qui partage le média sous-jacent, ou si l’invité utilise un fichier VHD qui réside sur un support partagé localement ou sur une infrastructure SAN, NAS ou iSCSI. L’exercice de planification consiste à s’assurer que le support sous-jacent peut prendre en charge la charge totale de tous les consommateurs.

De même, du point de vue de l’invité, comme il existe des chemins de code supplémentaires qui doivent être parcourus, il y a un impact sur les performances de l’utilisation d’un hôte pour accéder à n’importe quel stockage. Le test des performances de stockage indique que la virtualisation a un impact sur le débit de façon subjective de l’utilisation du processeur du système hôte (voir annexe A : critères de dimensionnement de l’UC), qui est évidemment influencée par les ressources de hôte demandé par l’invité. Cela contribue aux considérations relatives à la virtualisation en ce qui concerne le traitement des besoins dans un scénario virtualisé (consultez [Considérations sur la virtualisation pour le traitement](#virtualization-considerations-for-processing)).

Ce qui est plus complexe, c’est qu’il existe une variété d’options de stockage qui sont disponibles et qui ont un impact différent sur les performances. En guise d’estimation sécurisée lors de la migration d’un physique vers un autre, utilisez un multiplicateur de 1,10 pour ajuster les différentes options de stockage pour les invités virtualisés sur Hyper-V, tels que le stockage direct, la carte SCSI ou l’IDE. Les ajustements qui doivent être effectués lors du transfert entre les différents scénarios de stockage ne sont pas pertinents pour déterminer si le stockage est local, SAN, NAS ou iSCSI.

#### <a name="calculation-summary-example"></a>Exemple de résumé du calcul

Détermination de la quantité d’e/s nécessaires pour un système sain dans des conditions de fonctionnement normales :

- Disque logique ( *\<\>de lecteur de base de données NTDS* ) \ transferts/s pendant la période de pointe de 15 minutes 
- Pour déterminer la quantité d’e/s nécessaire pour le stockage où la capacité du stockage sous-jacent est dépassée :
  >Opérations d' *e/s par seconde nécessaires* = (disque logique ( *\<disque\>de base de données*) \Avg disque s &times; / &divide; *\<lecture cible moyenne disque s/lecture\>* ) ( *\< Lecteur\>de base de données NTDS*) \ lecture/s

|Counter|Value|
|-|-|
|Disque logique réel ( *\<lecteur de base de données NTDS\>* ) \Avg disque s/transfert|.02 secondes (20 millisecondes)|
|Disque logique cible ( *\<\>lecteur de base de données NTDS* ) \Avg disque s/transfert|.01 secondes|
|Multiplicateur pour la modification des e/s disponibles|0,02 &divide; 0,01 = 2|  
  
|Nom de valeur|Value|
|-|-|
|Disque logique ( *\<\>de lecteur de base de données NTDS* ) \ transferts/s|400|
|Multiplicateur pour la modification des e/s disponibles|2|
|Nombre total d’e/s par seconde nécessaires pendant la période de pointe|800|

Pour déterminer la fréquence à laquelle le cache doit être chauffé :

- Déterminez le délai maximal acceptable pour le préchauffage du cache. Il s’agit de la durée nécessaire pour charger l’intégralité de la base de données à partir du disque, ou pour les scénarios où la totalité de la base de données ne peut pas être chargée dans la mémoire RAM, il s’agit de la durée maximale de remplissage de la mémoire RAM.
- Déterminez la taille de la base de données, à l’exception des espaces blancs.  Pour plus d’informations, consultez [évaluation du stockage](#evaluating-for-storage).  
- Divise la taille de la base de données par 8 Ko ; Il s’agit de la valeur IOs totale nécessaire au chargement de la base de données.
- Divise le nombre total d’IOs par le nombre de secondes dans le laps de temps défini.

Notez que le taux calculé, bien que exact, ne sera pas exact, car les pages précédemment chargées sont supprimées si ESE n’est pas configuré pour avoir une taille de cache fixe, et AD DS par défaut utilise la taille de cache variable.

|Points de données à collecter|Valeurs
|-|-|
|Durée maximale acceptable à chaud|10 minutes (600 secondes)
|Taille de la base de données|2 Go|  
  
|Étape de calcul|Formule|Résultat|
|-|-|-|
|Calculer la taille de la base de données dans les pages|(2 Go &times; 1024 &times; 1024) = *taille de la base de données en Ko*|2 097 152 KO|
|Calculer le nombre de pages dans la base de données|2 097 152 Ko &divide; 8 Ko = *nombre de pages*|262 144 pages|
|Calculer les e/s par seconde nécessaires pour le démarrage complet du cache|262 144 pages &divide; 600 secondes = *IOPS nécessaires*|437 E/S PAR SECONDE|

## <a name="processing"></a>Processing

### <a name="evaluating-active-directory-processor-usage"></a>Évaluation de l’utilisation du processeur Active Directory

Pour la plupart des environnements, une fois que le stockage, la RAM et la mise en réseau sont correctement réglés comme décrit dans la section de planification, la gestion de la quantité de capacité de traitement est le composant qui mérite le plus d’attention. L’évaluation de la capacité de l’UC nécessite deux défis :

- Que les applications de l’environnement soient ou non correctement commises dans une infrastructure de services partagés et qu’elles soient abordées dans la section intitulée « suivi des recherches coûteuses et inefficaces » dans l’article création d’une application Microsoft Active plus efficace Applications compatibles avec l’annuaire ou migration à partir d’appels SAM de niveau supérieur vers des appels LDAP.  
  
  Dans les environnements plus grands, la raison en est que les applications mal codées peuvent conduire à la volatilité de la charge de l’UC, « voler » une quantité inégale de temps processeur à partir d’autres applications, à créer des besoins de capacité de manière artificielle et à répartir la charge de manière inégale les contrôleurs de contrôle.  
- Comme AD DS est un environnement distribué avec un grand nombre de clients potentiels, l’estimation des dépenses d’un « client unique » est environnementalement subjective en raison des modèles d’utilisation et du type ou de la quantité d’applications tirant parti de AD DS. En bref, comme la section mise en réseau, pour une mise en application étendue, cette approche est mieux adaptée du point de vue de l’évaluation de la capacité totale nécessaire dans l’environnement.

Pour les environnements existants, au fur et à mesure que le dimensionnement du stockage a été abordé précédemment, l’hypothèse est que le stockage est maintenant dimensionné correctement et que les données relatives à la charge du processeur sont donc valides. Pour réitérer, il est essentiel de s’assurer que le goulot d’étranglement dans le système n’est pas le niveau de performance du stockage. Lorsqu’il existe un goulot d’étranglement et que le processeur attend, des États inactifs disparaissent une fois le goulot d’étranglement supprimé.  Lorsque les États d’attente du processeur sont supprimés, par définition, l’utilisation de l’UC augmente, car elle n’a plus à attendre les données. Par conséquent, collectez les compteurs de performances « disque logique ( *\<lecteur de base de données NTDS\>* ) \Avg disque s/lecture » et « processus (lsass)\\% temps processeur ». Les données de « processus (LSASS)\\% temps processeur » sont artificiellement basses si « disque logique ( *\<lecteur de base de données NTDS\>* ) \Avg disque s/lecture » dépasse 10 à 15 ms, ce qui correspond à un seuil général que le support technique de Microsoft utilise pour résoudre les problèmes de performances liés au stockage. Comme précédemment, il est recommandé d’utiliser des intervalles d’échantillonnage de 15, 30 ou 60 minutes. Tout ce qui est moins est généralement trop volatil pour les mesures appropriées ; tout ce qui est plus important lissera les aperçus quotidiens de manière excessive.

### <a name="introduction"></a>Introduction

Pour planifier la planification de la capacité des contrôleurs de domaine, la puissance de traitement nécessite la plus grande attention et compréhension. Lorsque vous redimensionnez des systèmes pour garantir des performances optimales, il y a toujours un composant qui est le goulet d’étranglement et dans un contrôleur de domaine correctement dimensionné, il s’agit du processeur.

À l’instar de la section mise en réseau dans laquelle la demande de l’environnement est examinée au niveau de chaque site, vous devez effectuer la même opération pour la capacité de calcul demandée. Contrairement à la section mise en réseau, où les technologies de mise en réseau disponibles dépassent la demande normale, payez plus en plus à la capacité de dimensionnement de l’UC.  Comme n’importe quel environnement de taille moyenne égale ; tout ce qui se passe de quelques milliers d’utilisateurs simultanés peut mettre en place une charge importante sur le processeur.

Malheureusement, en raison de l’énorme variabilité des applications clientes qui exploitent Active Directory, une estimation générale des utilisateurs par UC est résolument inapplicable à tous les environnements. Plus précisément, les demandes de calcul sont soumises au comportement utilisateur et au profil d’application. Par conséquent, chaque environnement doit être dimensionné individuellement.

#### <a name="target-site-behavior-profile"></a>Profil de comportement du site cible

Comme mentionné précédemment, lors de la planification de la capacité d’un site entier, l’objectif est de cibler une conception avec une conception de capacité *N* + 1, de telle sorte que la défaillance d’un système pendant la période de pointe permettra la poursuite du service à un niveau de qualité raisonnable. Cela signifie que dans un scénario «*N*», la charge dans toutes les zones doit être inférieure à 100% (mieux encore, moins de 80%) pendant les pics d’activité.

En outre, si les applications et les clients du site utilisent les meilleures pratiques pour localiser les contrôleurs de domaine (autrement dit, à l’aide de la [fonction DsGetDcName](https://msdn.microsoft.com/library/windows/desktop/ms675983(v=vs.85).aspx)), les clients doivent être distribués de manière relativement équitable avec des pics temporaires mineurs en raison d’un nombre quelconque de facteurs.

Dans l’exemple suivant, les hypothèses suivantes sont effectuées :

- Chacun des cinq contrôleurs de l’un des deux contrôleurs de site dispose de quatre processeurs.
- L’utilisation totale du processeur cible pendant les heures de bureau est de 40% dans des conditions de fonctionnement normales («*n* + 1 ») et de 60% sinon («*n*»). En dehors des heures de bureau, l’utilisation de l’UC cible est de 80%, car les logiciels de sauvegarde et autres opérations de maintenance sont censés utiliser toutes les ressources disponibles.

![Graphique d’utilisation de l’UC](media/capacity-planning-considerations-cpu-chart.png)

Analyse des données du graphique (informations sur le processeur (_Total)\% utilitaire processeur) pour chacun des contrôleurs de contrôle :

- Dans la plupart des cas, la charge est distribuée de manière relativement uniforme, ce qui est attendu lorsque les clients utilisent le localisateur de contrôleur de site et ont des recherches bien écrites. 
- Il y a un certain nombre de pics de 5 minutes de 10%, dont certains peuvent atteindre 20%. En règle générale, à moins qu’ils ne provoquent le dépassement de la cible du plan de capacité, l’examen de ces dernières n’est pas utile.  
- La période de pointe pour tous les systèmes est comprise entre environ 8:00 AM et 9:15 AM. Avec la transition en douceur d’environ 5:00 à environ 5:00 PM, il s’agit généralement du cycle d’entreprise. Les pics les plus aléatoires de l’utilisation du processeur dans un scénario par boîte entre 5:00 h 00 et 4:00 AM se situent en dehors des préoccupations de planification de la capacité.

  >[!NOTE]
  >Sur un système bien géré, il peut s’agir d’un logiciel de sauvegarde en cours d’exécution, d’analyses antivirus complètes du système, d’inventaire matériel ou logiciel, d’un déploiement logiciel ou de correctifs, etc. Les cibles ne sont pas dépassées car elles ne sont pas dépassées dans le cycle d’activité des utilisateurs de pointe.

- Étant donné que chaque système est d’environ 40% et que tous les systèmes ont le même nombre de processeurs, en cas d’échec ou de mise hors connexion, les systèmes restants s’exécutaient à une valeur estimée de 53% (le chargement de 40% du système D est uniformément fractionné et ajouté à la charge de 40% du système A et du système C). Pour plusieurs raisons, cette hypothèse linéaire n’est pas parfaitement exacte, mais elle offre suffisamment de précision pour la jauge.  

  **Autre scénario :** Deux contrôleurs de domaine en cours d’exécution à 40% : un contrôleur de domaine échoue, l’UC estimée sur le restant est une estimation de 80%. Cette limite dépasse les seuils définis ci-dessus pour le plan de capacité et commence à limiter gravement la quantité de la salle d’absence pour les 10 à 20% visibles dans le profil de charge ci-dessus, ce qui signifie que les pics pilotent le DC à 90% à 100% au cours du scénario «*N*» et dégradent nettement la réactivité.

### <a name="calculating-cpu-demands"></a>Calcul des demandes de l’UC

Le compteur d’objets de performance « processus\\% temps processeur » totalise la durée totale pendant laquelle tous les threads d’une application sont répartis sur le processeur et se divise par la quantité totale de temps système écoulée. Cela est lié au fait qu’une application multithread sur un système multiprocesseur peut dépasser 100% du temps processeur, et être interprétée de manière très différente de « l’utilitaire processeur-informations\\% ». Dans la pratique, le « processus (LSASS)\\% temps processeur » peut être considéré comme le nombre d’UC s’exécutant à 100% qui sont nécessaires pour prendre en charge les demandes du processus. Une valeur de 200% signifie que 2 processeurs, chacun à 100%, sont nécessaires pour prendre en charge la charge de AD DS complète. Bien qu’un processeur s’exécutant à 100% de la capacité soit le plus rentable du point de vue de l’argent passé sur les processeurs et la consommation d’énergie et d’énergie, pour plusieurs raisons détaillées dans l’annexe A, une meilleure réactivité sur un système multithread se produit lorsque le système est ne s’exécute pas à 100%.

Pour tenir compte des pics transitoires dans la charge du client, il est recommandé de cibler une période de pointe de temps processeur de entre 40% et 60% de la capacité du système. À l’aide de l’exemple ci-dessus, cela signifie qu’entre 3,33 (60% cible) et 5 (40% de la cible) UC est nécessaire pour la charge du AD DS (processus LSASS). Une capacité supplémentaire doit être ajoutée dans en fonction des besoins du système d’exploitation de base et des autres agents requis (tels que les antivirus, la sauvegarde, la surveillance, etc.). Bien que l’impact des agents doive être évalué en fonction de l’environnement, une estimation de 5% et 10% d’un processeur unique peut être effectuée. Dans l’exemple actuel, cela suggère qu’entre 3,43 (60% cible) et 5,1 (40% cible) UC sont nécessaires pendant les périodes de pointe.

Le moyen le plus simple consiste à utiliser la vue « zone empilée » dans l’analyseur de fiabilité et de performances Windows (Perfmon), en veillant à ce que tous les compteurs soient mis à l’échelle de la même façon.

Hypothèses :

- L’objectif est de réduire l’encombrement au moins de serveurs possible. Dans l’idéal, un serveur contiendra la charge et un serveur supplémentaire ajouté pour la redondance (scénario*N* + 1).

![Graphique de temps processeur pour le processus LSASS (sur tous les processeurs)](media/capacity-planning-considerations-proc-time-chart.png)

Connaissances obtenues à partir des données du graphique (processus (LSASS)\\% de temps processeur) :

- Le jour ouvrable commence à 7:00 et diminue à 5:00 PM.
- La période de pointe la plus chargée est comprise entre 9:30 et 11:00 AM. 
  > [!NOTE]
  > Toutes les données de performances sont historiques. Le point de données de pointe à 9:15 indique la charge de 9:00 à 9:15.
- Il y a des pics avant 7:00, qui peuvent indiquer une charge à partir de différents fuseaux horaires ou une activité d’infrastructure en arrière-plan, telle que des sauvegardes. Étant donné que le pic à 9:30 AM dépasse cette activité, il n’est pas pertinent.
- Il existe trois contrôleurs de domaine dans le site.

Au niveau de la charge maximale, Lsass consomme environ 485% d’un processeur, ou 4,85 UC s’exécutant à 100%. Comme c’est le cas pour les mathématiques précédentes, cela signifie que le site a besoin d’environ 12,25 UC pour AD DS. Ajoutez les suggestions ci-dessus de 5 à 10% pour les processus d’arrière-plan et cela nécessitera environ 12,30 à 12,35 processeurs pour prendre en charge la même charge. Une estimation environnementale de la croissance doit maintenant être factorisée.

### <a name="when-to-tune-ldap-weights"></a>Quand régler les poids LDAP

Il existe plusieurs scénarios dans lesquels le paramétrage de [LdapSrvWeight](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc957291(v=technet.10)) doit être pris en compte. Dans le cadre de la planification de la capacité, cela se fait lorsque l’application ou le chargement de l’utilisateur ne sont pas équilibrés uniformément, ou que les systèmes sous-jacents ne sont pas équilibrés uniformément en termes de capacité. La planification de la capacité n’est pas abordée dans le cadre de cet article.

Il existe deux raisons courantes pour régler les poids LDAP :

- L’émulateur de contrôleur de domaine principal est un exemple qui affecte tous les environnements pour lesquels le comportement de chargement d’un utilisateur ou d’une application n’est pas distribué uniformément. Étant donné que certains outils et actions ciblent l’émulateur de contrôleur de domaine principal, par exemple les outils de gestion stratégie de groupe, les autres tentatives en cas d’échec de l’authentification, l’établissement d’une approbation, etc., les ressources du processeur sur l’émulateur de contrôleur de domaine principal peuvent être de plus en plus sollicitées par ailleurs dans le site.
  - Il est utile de régler ce paramètre uniquement s’il existe une différence notable dans l’utilisation de l’UC afin de réduire la charge sur l’émulateur de contrôleur de domaine principal et d’augmenter la charge sur les autres contrôleurs de domaine, ce qui permet une distribution plus équilibrée de la charge.
  - Dans ce cas, définissez LDAPSrvWeight entre 50 et 75 pour l’émulateur de contrôleur de domaine principal.
- Serveurs avec différents nombres de processeurs (et vitesses) dans un site.  Par exemple, imaginons qu’il y ait des serveurs 2 8-Core et un serveur 1 4-Core.  Le dernier serveur a la moitié des processeurs des deux autres serveurs.  Cela signifie qu’une charge de client bien distribuée augmente la charge moyenne de l’UC sur la zone à quatre cœurs à deux fois plus près que des zones à huit cœurs.
  - Par exemple, les zones 2 8-Core s’exécutent à 40% et la zone à quatre cœurs s’exécute à 80%.
  - En outre, envisagez l’impact de la perte de la boîte de 1 8-Core dans ce scénario, en particulier le fait que la zone à quatre cœurs serait maintenant surchargée.

#### <a name="example-1---pdc"></a>Exemple 1-PDC

| |Utilisation avec valeurs par défaut|Nouveau LdapSrvWeight|Estimation de la nouvelle utilisation|
|-|-|-|-|
|DC 1 (émulateur PDC)|53%|57|40 %|
|DC 2|33%|100|40 %|
|DC 3|33%|100|40 %|

Le problème est que si le rôle d’émulateur de contrôleur de domaine principal est transféré ou pris, en particulier pour un autre contrôleur de domaine dans le site, une augmentation spectaculaire du nouvel émulateur de contrôleur de domaine principal se produira.

À l’aide de l’exemple de la section [profil de comportement de site cible](#target-site-behavior-profile), on suppose que les trois contrôleurs de domaine du site comportent quatre processeurs. Que se passe-t-il, dans des conditions normales, si l’un des contrôleurs de domaine a huit processeurs ? Il y aurait deux contrôleurs de domaine à 40% d’utilisation et une à 20% d’utilisation. Bien que cela ne soit pas mauvais, il est possible d’équilibrer la charge un peu plus. Tirez parti des pondérations LDAP pour y parvenir.  Voici un exemple de scénario :

#### <a name="example-2---differing-cpu-counts"></a>Exemple 2-nombre de PROCESSEURs différents

| |Informations sur le processeur\\ %utilitaire processeur &nbsp;(_Total)<br />Utilisation avec valeurs par défaut|Nouveau LdapSrvWeight|Estimation de la nouvelle utilisation|
|-|-|-|-|
|4 PROCESSEURS DC 1|40|100|30 %|
|4 PROCESSEURS DC 2|40|100|30 %|
|8 PROCESSEURS DC 3|20|200|30 %|

Toutefois, soyez très vigilant avec ces scénarios. Comme vous pouvez le voir ci-dessus, la mathématique semble très intéressante et assez belle sur le papier. Mais tout au long de cet article, la planification d’un scénario «*N* + 1 » revêt une importance primordiale. L’impact de la mise hors connexion d’un contrôleur de groupe doit être calculé pour chaque scénario. Dans le scénario précédent où la distribution de la charge est même, afin de garantir une charge de 60% au cours d’un scénario «*N*», la charge étant équilibrée uniformément entre tous les serveurs, la distribution est parfaite, car les ratios restent cohérents. En examinant le scénario de paramétrage de l’émulateur de contrôleur de domaine principal et, en général, tout scénario dans lequel la charge utilisateur ou d’application est déséquilibrée, l’effet est très différent :

| |Utilisation optimisée|Nouveau LdapSrvWeight|Estimation de la nouvelle utilisation|
|-|-|-|-|
|DC 1 (émulateur PDC)|40 %|85|47 %|
|DC 2|40 %|100|53%|
|DC 3|40 %|100|53%|

### <a name="virtualization-considerations-for-processing"></a>Considérations relatives à la virtualisation pour le traitement

Il existe deux couches de planification de la capacité qui doivent être effectuées dans un environnement virtualisé. Au niveau de l’hôte, à l’instar de l’identification du cycle d’activité du traitement du contrôleur de domaine précédemment, les seuils au cours de la période de pointe doivent être identifiés. Étant donné que les principaux sous-jacents sont les mêmes pour un ordinateur hôte qui planifie les threads invités sur le processeur que pour l’obtention de AD DS threads sur l’UC sur un ordinateur physique, le même objectif de 40% à 60% sur l’hôte sous-jacent est recommandé. À la couche suivante, la couche invitée, étant donné que les principaux de la planification des threads n’ont pas changé, l’objectif au sein de l’invité reste dans la plage de 40 à 60%.

Dans un scénario mappé direct, un invité par hôte, toute la planification de la capacité effectuée jusqu’à ce point doit être ajoutée aux exigences (RAM, disque, réseau) du système d’exploitation hôte sous-jacent. Dans un scénario d’hôte partagé, le test indique qu’il y a 10% d’impact sur l’efficacité des processeurs sous-jacents. Cela signifie que si un site a besoin de 10 processeurs à une cible de 40%, la quantité recommandée de processeurs virtuels à *N* allouer sur tous les invités est de 11. Sur un site avec une distribution mixte des serveurs physiques et des serveurs virtuels, le modificateur s’applique uniquement aux machines virtuelles. Par exemple, si un site a un scénario «*N* + 1 », un serveur physique ou mappé en direct avec 10 processeurs est équivalent à un invité avec 11 UC sur un ordinateur hôte, avec 11 UC réservées pour le contrôleur de domaine.

Tout au long de l’analyse et du calcul des quantités d’UC nécessaires à la prise en charge de la charge AD DS, le nombre de processeurs mappés sur ce qui peut être acheté en termes de matériel physique n’est pas nécessairement correctement mappé. La virtualisation élimine la nécessité d’arrondir. La virtualisation réduit les efforts nécessaires pour ajouter de la capacité de calcul à un site, en fonction de la facilité avec laquelle un processeur peut être ajouté à une machine virtuelle. Il n’élimine pas la nécessité d’évaluer avec précision la puissance de calcul nécessaire pour que le matériel sous-jacent soit disponible lorsque des UC supplémentaires doivent être ajoutées aux invités.  Comme toujours, n’oubliez pas de planifier et de surveiller la croissance de la demande.

### <a name="calculation-summary-example"></a>Exemple de résumé du calcul

|Système|PIC d’utilisation de l’UC|
|-|-|-|
|DC 1|120%|
|DC 2|147%|
|DC 3|218%|
|PROCESSEUR total utilisé|485%|  
  
|Nombre de systèmes (s) cible (s)|Bande passante totale (ci-dessus)|
|-|-|
|UC nécessaires à 40% de la cible|4,85 &divide;. 4 = 12,25|

En raison de l’importance de ce point, *pensez à planifier la croissance*. En supposant une croissance de 50% au cours des trois prochaines années, cet environnement nécessitera 18,375 UC (12,25 &times; 1,5) sur la marque de trois ans. Un autre plan consiste à passer en revue après la première année et à ajouter de la capacité supplémentaire en fonction des besoins.

### <a name="cross-trust-client-authentication-load-for-ntlm"></a>Charge d’authentification du client de confiance croisée pour NTLM

#### <a name="evaluating-cross-trust-client-authentication-load"></a>Évaluation de la charge d’authentification du client de confiance croisée

De nombreux environnements peuvent avoir un ou plusieurs domaines connectés par une approbation. Une demande d’authentification pour une identité dans un autre domaine qui n’utilise pas l’authentification Kerberos doit traverser une approbation à l’aide du canal sécurisé du contrôleur de domaine vers un autre contrôleur de domaine dans le domaine de destination ou le domaine suivant dans le chemin d’accès au domaine de destination. Le nombre d’appels simultanés à l’aide du canal sécurisé qu’un contrôleur de domaine peut apporter à un contrôleur de domaine dans un domaine approuvé est contrôlé par un paramètre appelé **MaxConcurrentApi**. Pour les contrôleurs de domaine, le fait de s’assurer que le canal sécurisé peut gérer la quantité de charge est accompli par l’une des deux approches suivantes : paramétrage de **MaxConcurrentApi** ou, au sein d’une forêt, création d’approbations de raccourci. Pour évaluer le volume de trafic au sein d’une approbation individuelle, consultez [Comment effectuer le réglage des performances pour l’authentification NTLM à l’aide du paramètre MaxConcurrentApi](https://support.microsoft.com/kb/2688798).

Au cours de la collecte des données, cela, comme avec tous les autres scénarios, doit être collecté au cours des périodes de pic d’activité du jour pour que les données soient utiles.

> [!NOTE]
> Dans le cas d’une forêt ou d’une forêt, l’authentification peut traverser plusieurs approbations et chaque étape doit être paramétrée.

#### <a name="planning"></a>Planification

Il existe un certain nombre d’applications qui utilisent l’authentification NTLM par défaut, ou l’utilisent dans un certain scénario de configuration. Les serveurs d’applications augmentent en capacité et en service un nombre grandissant de clients actifs. Il existe également une tendance selon laquelle les clients conservent les sessions ouvertes pendant une période limitée et se reconnectent régulièrement (par exemple, la synchronisation par courrier électronique). Les serveurs proxy Web qui requièrent une authentification pour l’accès à Internet constituent un autre exemple courant de charge NTLM élevée.

Ces applications peuvent entraîner une charge importante pour l’authentification NTLM, ce qui peut entraîner une contrainte importante sur les contrôleurs de domaine, en particulier lorsque les utilisateurs et les ressources se trouvent dans des domaines différents.

Il existe plusieurs approches de gestion de la charge de confiance croisée, qui sont utilisées en pratique conjointement plutôt que dans un scénario exclusif/ou. Les options possibles sont les suivantes :

- Réduire l’authentification du client de confiance croisée en localisant les services qu’un utilisateur consomme dans le même domaine que celui dans lequel il réside.
- Augmentez le nombre de canaux sécurisés disponibles. Cela s’applique au trafic à forêt et aux forêts et est connu sous le nom d’approbations raccourcies.
- Réglez les paramètres par défaut de **MaxConcurrentApi**.

Pour le paramétrage de **MaxConcurrentApi** sur un serveur existant, l’équation est la suivante :

> *New_MaxConcurrentApi_setting* &ge; *(semaphore_acquires*  +  *semaphore_time*) &times; *average_semaphore_hold_time* &divide; *time_collection_length*

Pour plus d’informations, consultez [l’article 2688798 de la base de connaissances : comment effectuer le réglage des performances pour l’authentification NTLM à l’aide du paramètre MaxConcurrentApi](https://support.microsoft.com/kb/2688798).

## <a name="virtualization-considerations"></a>Éléments à prendre en compte en matière de virtualisation

Aucun, il s’agit d’un paramètre de réglage du système d’exploitation.

### <a name="calculation-summary-example"></a>Exemple de résumé du calcul

|Type de données|Value|
|-|-|
|Acquisitions de sémaphore (minimum)|6 161|
|Acquisitions de sémaphore (maximum)|6 762|
|Délais d’expiration des sémaphores|0|
|Durée moyenne de maintien du sémaphore|0,012|
|Durée de la collection (secondes)|1:11 minutes (71 secondes)|
|Formule (de l’article KB 2688798)|((6762 &ndash; 6161) + 0) &times; 0,012/|
|Valeur minimale pour **MaxConcurrentApi**|((6762 &ndash; 6161) + 0) &times; 0,012 &divide; 71 =. 101|

Pour ce système pour cette période, les valeurs par défaut sont acceptables.

## <a name="monitoring-for-compliance-with-capacity-planning-goals"></a>Surveillance de la conformité avec les objectifs de planification de capacité

Tout au long de cet article, nous avons vu que la planification et la mise à l’échelle vont vers les objectifs d’utilisation. Voici un graphique récapitulatif des seuils recommandés qui doivent être analysés pour s’assurer que les systèmes fonctionnent dans des seuils de capacité appropriés. Gardez à l’esprit qu’il ne s’agit pas de seuils de performances, mais de seuils de planification de la capacité. Un serveur fonctionnant au-delà de ces seuils fonctionnera, mais il est temps de commencer à valider que toutes les applications fonctionnent correctement. Si ces applications sont bien connues, il est temps de commencer à évaluer les mises à niveau matérielles ou d’autres modifications de configuration.

|Catégorie|Compteur de performance|Intervalle/échantillonnage|Target|Warning|
|-|-|-|-|-|
|Processeur|Informations sur le processeur (_Total)\\% Processor Utility|60 min|40 %|60 %|
|RAM (Windows Server 2008 R2 ou version antérieure)|Mémoire \ Mo|< 100 Mo|NON APPLICABLE|< 100 Mo|
|RAM (Windows Server 2012)|Durée de vie moyenne du cache en attente Memory\Long-Term|30 min|Doit être testé|Doit être testé|
|réseau|Interface réseau (\*) \Octets envoyés/s<br /><br />Interface réseau (\*) \Octets reçus/s|30 min|40 %|60 %|
|Stockage|Disque logique ( *\<lecteur de base de données NTDS\>* ) \Avg disque s/lecture<br /><br />Disque logique ( *\<\>de lecteur de base de données NTDS* ) \Avg disque s/écriture|60 min|10 ms|15 ms|
|Services AD|Le \Latence Netlogon (\*) a conservé le temps de blocage|60 min|0|1 seconde|

## <a name="appendix-a-cpu-sizing-criteria"></a>Annexe A : critères de dimensionnement de l’UC

### <a name="definitions"></a>DÉFINITIONS

**Processeur (microprocesseur) :** composant qui lit et exécute des instructions de programme.  

**Processeur –** Unité centrale de traitement  

**Processeur à plusieurs cœurs :** plusieurs processeurs sur le même circuit intégré  

**Multi-UC :** plusieurs UC, et non sur le même circuit intégré  

**Processeur logique :** un moteur de calcul logique du point de vue du système d’exploitation  

Cela comprend l’Hyper-Threading, un cœur sur un processeur multicœur ou un processeur à noyau unique.  

À mesure que les systèmes serveurs d’aujourd’hui disposent de plusieurs processeurs, de plusieurs processeurs multicœurs et de l’Hyper-Threading, ces informations sont généralisées pour couvrir les deux scénarios. Par conséquent, le terme processeur logique sera utilisé, car il représente le point de vue du système d’exploitation et de l’application des moteurs informatiques disponibles.

### <a name="thread-level-parallelism"></a>Parallélisme au niveau du thread

Chaque thread est une tâche indépendante, car chaque thread possède sa propre pile et ses instructions. Étant donné que AD DS est multithread et que le nombre de threads disponibles peut être réglé à l’aide [de la procédure d’affichage et de définition d’une stratégie LDAP dans Active Directory à l’aide de Ntdsutil. exe](https://support.microsoft.com/kb/315071), il évolue bien sur plusieurs processeurs logiques.

### <a name="data-level-parallelism"></a>Parallélisme au niveau des données

Cela implique le partage des données entre plusieurs threads au sein d’un processus (dans le cas du processus AD DS seul) et sur plusieurs threads dans plusieurs processus (en général). Avec la préoccupation de sursimplifier le cas, cela signifie que toutes les modifications apportées aux données sont répercutées sur tous les threads en cours d’exécution dans les différents niveaux de cache (L1, L2, L3) sur tous les cœurs qui exécutent ces threads, ainsi que la mise à jour de la mémoire partagée. Les performances peuvent se dégrader pendant les opérations d’écriture, tandis que tous les différents emplacements de mémoire sont mis en cohérence avant que le traitement des instructions puisse continuer.

### <a name="cpu-speed-vs-multiple-core-considerations"></a>Vitesse du processeur et considérations à plusieurs cœurs

En règle générale, les processeurs logiques plus rapides réduisent la durée nécessaire au traitement d’une série d’instructions, tandis que davantage de processeurs logiques signifient que davantage de tâches peuvent être exécutées en même temps. Ces règles de curseur détaillent les scénarios qui deviennent intrinsèquement plus complexes avec les considérations relatives à la récupération de données à partir de la mémoire partagée, à l’attente du parallélisme au niveau des données et à la surcharge liée à la gestion de plusieurs threads. C’est également la raison pour laquelle l’extensibilité des systèmes multicœurs n’est pas linéaire.

Considérez les analogies suivantes dans ces considérations : Imaginez une autoroute, chaque thread étant une voiture individuelle, chaque voie est un cœur et la limite de vitesse étant la vitesse de l’horloge.

1. S’il n’y a qu’une seule voiture sur l’autoroute, peu importe s’il y a deux couloirs ou 12 couloirs. Cette voiture ne va pas être aussi rapide que la limite de vitesse autorisée.
1. Supposons que les données requises par le thread ne sont pas immédiatement disponibles. L’analogie est qu’un segment de route est arrêté. S’il n’y a qu’une seule voiture sur l’autoroute, peu importe la limite de vitesse jusqu’à la réouverture de la voie (les données sont extraites de la mémoire).
1. À mesure que le nombre de voitures augmente, la surcharge de gestion du nombre de voitures augmente. Comparez l’expérience de la conduite et la quantité d’attention nécessaire lorsque la route est pratiquement vide (par exemple, la fin de la soirée) par rapport à la surcharge du trafic (par exemple, la mi-après-midi, mais pas l’heure de démarrage). En outre, tenez compte de la quantité de vigilance nécessaire lorsque vous conduisez sur une autoroute à deux voies, où il n’y a qu’une autre voie à se soucier de ce que font les pilotes, par opposition à une autoroute à six voies, où l’un d’eux doit se préoccuper de la réalisation d’un grand nombre d’autres pilotes.
   > [!NOTE]
   > L’analogie du scénario d’heure de Rush est étendue dans la section suivante : le temps de réponse/la manière dont l’occupation du système influe sur les performances.

Par conséquent, les spécificités des processeurs plus ou plus rapides deviennent très subjectives pour le comportement des applications, ce qui, dans le cas de AD DS, est très spécifique à l’environnement et même varie d’un serveur à l’autre au sein d’un environnement. C’est la raison pour laquelle les références mentionnées plus haut dans l’article ne sont pas beaucoup trop précises et une marge de sécurité est incluse dans les calculs. Lorsque vous prenez des décisions d’achat basées sur le budget, il est recommandé d’utiliser d’abord l’optimisation de l’utilisation des processeurs à 40% (ou le nombre souhaité pour l’environnement) avant d’acheter des processeurs plus rapides. L’augmentation de la synchronisation sur davantage de processeurs réduit l’avantage réel d’un plus grand nombre de processeurs par rapport à la progression linéaire (2&times; le nombre de processeurs fournit moins de 2&times; de puissance de calcul supplémentaire disponible).

> [!NOTE]
> La Loi de Amdahl et de Gustafson est ici les concepts pertinents.

### <a name="response-timehow-the-system-busyness-impacts-performance"></a>Temps de réponse/impact de l’occupation du système sur les performances

La théorie de la mise en file d’attente est l’étude mathématique des lignes en attente (files d’attente). En matière de mise en file d’attente, la Loi d’utilisation est représentée par l’équation suivante :

*U* k = *B* &divide; *t*

Où *U* k est le pourcentage d’utilisation, *B* est la durée occupée et *T* est la durée totale pendant laquelle le système a été observé. Traduit dans le contexte de Windows, ce qui correspond au nombre de threads d’intervalle de 100 nanosecondes (NS) qui sont dans un État en cours d’exécution divisée par le nombre d’intervalles de 100-ns disponibles dans un intervalle de temps donné. Il s’agit exactement de la formule permettant de calculer l’utilitaire% Processor ( [objet du processeur](https://docs.microsoft.com/previous-versions/ms804036(v=msdn.10)) de référence et [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/previous-versions/windows/embedded/ms901169(v=msdn.10))).

La théorie de la mise en file d’attente fournit également la formule suivante : *N* = *u* k &divide; (1 &ndash; *u* k) pour estimer le nombre d’éléments en attente en fonction de l’utilisation ( *N* est la longueur de la file d’attente). La représentation sous forme de graphique de tous les intervalles d’utilisation fournit les estimations suivantes sur la durée pendant laquelle la file d’attente doit être obtenue sur le processeur à une charge de processeur donnée.

![Longueur de la file d’attente](media/capacity-planning-considerations-queue-length.png)

Il est observé qu’après 50% de la charge de l’UC, en moyenne, il y a toujours une attente d’un autre élément dans la file d’attente, avec une augmentation notable de l’utilisation du processeur de 70%.

Retour à l’analogie de conduite utilisée plus haut dans cette section :

- Les heures occupées de « mi-après-midi » se situent dans la plage de 40% à 70%. Il y a suffisamment de trafic, de sorte que l’une des capacités de sélection d’une voie n’est pas très restreinte, et que le risque d’un autre pilote, bien que élevé, n’exige pas le niveau d’effort pour « trouver » un écart de sécurité entre les autres voitures en déplacement.
- L’un d’entre eux remarquera que comme le trafic approche l’heure de Rush, le système routier s’approche de la capacité de 100%. La modification des couloirs peut devenir très complexe, car les voitures sont tellement proches, ce qui nécessite une attention accrue.

C’est la raison pour laquelle les moyennes à long terme de la capacité estimée de façon figée à 40% autorisent la chambre de tête pour les pics de charge anormaux, qu’il s’agisse de pics de charge (par exemple des requêtes mal codées qui s’exécutent pendant quelques minutes) ou de pics anormaux en charge générale (le matin de premier jour après un week-end long).

L’instruction ci-dessus considère que le calcul du temps processeur est identique à celui de la Loi d’utilisation. il s’agit d’une simplification de la facilité du lecteur général. Pour ceux qui sont plus mathématiquement rigoureux :  
- Traduction du [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/previous-versions/windows/embedded/ms901169(v=msdn.10))
  - *B* = le nombre de threads « inactifs » de 100-ns passe sur le processeur logique. Modification de la variable «*X*» dans le calcul de la [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/previous-versions/windows/embedded/ms901169(v=msdn.10))
  - *T* = nombre total d’intervalles de 100-ns dans un intervalle de temps donné. Modification de la variable «*Y*» dans le calcul de la [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/previous-versions/windows/embedded/ms901169(v=msdn.10)) .
  - *U* k = pourcentage d’utilisation du processeur logique par le « thread inactif » ou% de la durée d’inactivité.  
- Fonctionnement de la Math :
  - *U* k = 1 –% de temps processeur
  - % Temps processeur = 1 – *U* k
  - % Temps processeur = 1 – *B* / *t*
  - % Temps processeur = 1 – *x1* – *x0* / *Y1* – *Y0*

### <a name="applying-the-concepts-to-capacity-planning"></a>Application des concepts à la planification de la capacité

Les mathématiques précédentes peuvent déterminer si le nombre de processeurs logiques nécessaires dans un système semble très complexe. C’est la raison pour laquelle l’approche de dimensionnement des systèmes est axée sur la détermination de l’utilisation maximale de la cible en fonction de la charge actuelle et du calcul du nombre de processeurs logiques requis pour y parvenir. En outre, bien que les vitesses de processeur logique aient un impact significatif sur les performances, l’efficacité du cache, les exigences de cohérence de la mémoire, la planification et la synchronisation des threads, et la charge de client imparfaitement équilibrée aura un impact significatif sur les performances varient d’un serveur à l’autre. Avec un coût relativement bon marché de la puissance de calcul, toute tentative d’analyse et de détermination du nombre de processeurs nécessaires devient plus un exercice universitaire qu’il ne fournit de valeur commerciale.

40 pour cent n’est pas une exigence absolue et rapide, il s’agit d’un début raisonnable. Différents consommateurs de Active Directory requièrent différents niveaux de réactivité. Il peut y avoir des scénarios dans lesquels les environnements peuvent s’exécuter à 80% ou à 90% d’utilisation comme moyenne soutenue, car les temps d’attente accrus pour l’accès au processeur n’ont pas un impact notable sur les performances du client. Il est important de réitérer le fait qu’il existe de nombreuses zones dans le système qui sont beaucoup plus lentes que le processeur logique dans le système, y compris l’accès à la RAM, l’accès au disque et la transmission de la réponse sur le réseau. Tous ces éléments doivent être réglés conjointement. Exemples :

- L’ajout de processeurs à un système exécutant 90% qui est lié au disque ne va probablement pas améliorer de manière significative les performances. Une analyse plus poussée du système permettra probablement d’identifier un grand nombre de threads qui ne se trouvent même pas sur le processeur, car ils sont en attente de la fin des e/s.
- La résolution des problèmes liés aux disques signifie que les threads qui ont déjà passé beaucoup de temps dans un état d’attente ne seront plus dans un état d’attente pour les e/s et qu’il y aura plus de concurrence pour le temps processeur, ce qui signifie que l’utilisation de 90% dans l’exemple précédent passera à 100% (car il ne peut pas aller plus haut). Les deux composants doivent être paramétrés conjointement.
  > [!NOTE]
  > Les informations sur le processeur (*)\\% Utility Utility peuvent dépasser 100% avec les systèmes qui ont un mode « Turbo ».  C’est dans ce cas que le processeur dépasse la vitesse nominale du processeur pendant de courtes périodes.  Pour plus d’informations, consultez documentation des fabricants de PROCESSEURs et description du compteur.  

Les considérations relatives à l’utilisation de l’ensemble du système intègrent également les contrôleurs de domaine de conversation en tant qu’invités virtualisés. Le [temps de réponse/la manière dont l’occupation du système influe](#response-timehow-the-system-busyness-impacts-performance) sur les performances s’applique à la fois à l’hôte et à l’invité dans un scénario virtualisé. C’est pourquoi, dans un hôte avec un seul invité, un contrôleur de domaine (et généralement tout système) a presque les mêmes performances que le matériel physique. L’ajout d’invités supplémentaires aux hôtes augmente l’utilisation de l’hôte sous-jacent, ce qui augmente les temps d’attente pour accéder aux processeurs comme expliqué précédemment. En résumé, l’utilisation du processeur logique doit être gérée au niveau de l’hôte et au niveau de l’invité.

En étendant les analogies précédentes, en laissant l’autoroute en tant que matériel physique, la machine virtuelle invitée est analogue à un bus (un bus Express qui est directement dirigé vers la destination souhaitée). Imaginez les quatre scénarios suivants :

- En dehors des heures de bureau, un conducteur se trouve sur un bus presque vide et le bus sur une route qui est également presque vide. Comme il n’y a aucun trafic à rivaliser avec, le conducteur a une bonne chose, et il se place aussi rapidement que si le conducteur avait à la place. Les durées de trajet du conducteur sont toujours limitées par la limite de vitesse.
- En dehors des heures, le bus est presque vide, mais la plupart des couloirs de la route sont fermés, de sorte que l’autoroute est toujours encombré. Le conducteur se trouve sur un bus presque vide sur une route encombrée. Si le conducteur n’a pas beaucoup de concurrence dans le bus pour l’endroit où il se trouve, la durée totale du trajet est toujours dictée par le reste du trafic en dehors de.
- L’heure et le bus sont encombrés. Non seulement le voyage prend plus de temps, mais la mise sous tension et la désactivation du bus est un véritable cauchemar, car les gens sont à l’épaule et l’autoroute n’est pas bien plus efficace. L’ajout de bus (processeurs logiques à l’invité) ne signifie pas qu’ils peuvent tenir sur la route plus facilement, ou que le trajet sera raccourci.
- Le dernier scénario, bien qu’il puisse être un peu étiré, est l’endroit où le bus est plein, mais la route n’est pas encombrée. Alors que le conducteur aura toujours des difficultés à se connecter au bus, le voyage sera efficace une fois que le bus sera en déplacement. Il s’agit du seul scénario dans lequel l’ajout de bus supplémentaires (processeurs logiques à l’invité) améliore les performances de l’invité.

À partir de là, il est relativement facile d’extrapoler qu’il y a un certain nombre de scénarios entre l’utilisation à 0% et l’État utilisé à 100% de la route et l’État utilisé à 0% et 100% du bus qui a des degrés d’impact différents.

L’application des principaux ci-dessus de 40% de l’UC en tant que cible raisonnable pour l’hôte et l’invité est un démarrage raisonnable pour le même raisonnement que ci-dessus, la quantité de mise en file d’attente.

## <a name="appendix-b-considerations-regarding-different-processor-speeds-and-the-effect-of-processor-power-management-on-processor-speeds"></a>Annexe B : considérations relatives aux différentes vitesses de processeur et effet de la gestion de l’alimentation du processeur sur les vitesses de processeur

Dans les sections de la sélection du processeur, l’hypothèse est que le processeur s’exécute à 100% de la fréquence d’horloge pendant toute la durée de collecte des données et que les systèmes de remplacement auront les mêmes processeurs de vitesse. Bien que les deux hypothèses en pratique soient fausses, en particulier avec Windows Server 2008 R2 et versions ultérieures, où le mode de gestion de l’alimentation par défaut est **équilibré**, la méthodologie représente toujours l’approche conservatrice. Bien que le taux d’erreur potentiel puisse augmenter, il augmente uniquement la marge de sécurité en cas d’augmentation de la vitesse du processeur.

- Par exemple, dans un scénario où 11,25 processeurs sont demandés, si les processeurs étaient exécutés à mi-vitesse lors de la collecte des données, l’estimation la plus précise peut être 5,125 &divide; 2.
- Il est impossible de garantir que le doublement des vitesses d’horloge serait le double de la quantité de traitement qui se produit pendant une période donnée. Cela est dû au fait que le temps passé par les processeurs à attendre la RAM ou d’autres composants système peut rester le même. L’effet net est que les processeurs plus rapides peuvent passer un pourcentage plus élevé du temps d’inactivité lors de l’attente de l’extraction des données. Là encore, il est recommandé de respecter le dénominateur commun le plus bas, en étant conservateur, et d’éviter d’essayer de calculer un niveau de précision potentiellement faux en supposant une comparaison linéaire entre les vitesses de processeur.

En guise d’alternative, si les vitesses du processeur dans le matériel de remplacement sont inférieures à celles du matériel actuel, il est recommandé d’augmenter l’estimation des processeurs nécessaires à un montant proportionnel. Par exemple, il est calculé que 10 processeurs sont nécessaires pour supporter la charge dans un site, et les processeurs actuels s’exécutent à 3,3 GHz et les processeurs de remplacement s’exécutent à 2,6 GHz, ce qui représente une baisse de 21% de la vitesse. Dans ce cas, 12 processeurs seraient la quantité recommandée.

Cela dit, cette variabilité ne changeait pas les objectifs d’utilisation du processeur de gestion de la capacité. À mesure que les vitesses d’horloge du processeur seront ajustées dynamiquement en fonction de la charge demandée, l’exécution du système sous des charges plus élevées générera un scénario dans lequel le processeur consacre plus de temps à un état de fréquence d’horloge plus élevé, ce qui rend l’objectif ultime d’une utilisation de 40% dans un délai de 100%. État de la fréquence d’horloge au pic. Toute valeur inférieure à celle-ci génère des économies d’énergie, car les vitesses du processeur sont limitées pendant les périodes creuses.

> [!NOTE]
> Une option consiste à désactiver la gestion de l’alimentation sur les processeurs (en définissant le mode de gestion de l’alimentation sur **hautes performances**) pendant que les données sont collectées. Cela donne une représentation plus précise de la consommation du processeur sur le serveur cible.

Pour ajuster les estimations pour différents processeurs, il s’agissait d’une utilisation sécurisée, à l’exception des autres goulots d’étranglement du système décrits ci-dessus, afin de supposer que la vitesse du processeur doubler la quantité de traitement pouvant être effectuée.  À l’heure actuelle, l’architecture interne des processeurs est suffisamment différente entre les processeurs, ce qui constitue un moyen plus sûr de mesurer les effets de l’utilisation de différents processeurs que les données à partir de, afin de tirer parti de l’évaluation de la SPECint_rate2006 de l’évaluation des performances standard. Corporation.

1. Recherchez les scores d’SPECint_rate2006 pour le processeur utilisé et ce plan à utiliser.
    1. Sur le site Web de l’évaluation de performance standard Corporation, sélectionnez **résultats**, mettez **CPU2006**en surbrillance, puis sélectionnez **Rechercher toutes les SPECint_rate2006 résultats**.
    1. Sous **requête simple**, entrez les critères de recherche pour le processeur cible, par exemple le **processeur correspond à E5-2630 (baselinetarget)** et le **processeur correspond à E5-2650 (ligne de base)** .
    1. Recherchez la configuration du serveur et du processeur à utiliser (ou une fermeture, si une correspondance exacte n’est pas disponible) et notez la valeur dans les colonnes **result** et **# cores** .
1. Pour déterminer le modificateur, utilisez l’équation suivante :
   >((*Valeur du score par cœur de la plateforme cible*) &times; (*MHz par cœur de plateforme de référence*)) &divide; ((*valeur du score de base par*cœur) &times; (*MHz par cœur de plateforme cible*))  

    À l’aide de l’exemple ci-dessus :
   >(35,83 &times; 2000) &divide; (33,75 &times; 2300) = 0,92
1. Multipliez le nombre estimé de processeurs par le modificateur.  Dans le cas ci-dessus, pour passer du processeur E5-2650 au processeur E5-2630, multipliez les 11,25 UC calculées &times; 0,92 = 10,35 processeurs nécessaires.

## <a name="appendix-c-fundamentals-regarding-the-operating-system-interacting-with-storage"></a>Annexe C : notions de base concernant le système d’exploitation qui interagit avec le stockage

Les concepts de la théorie de la file d’attente définis dans [le temps de réponse/la manière dont l’occupation du système a un impact sur les performances](#response-timehow-the-system-busyness-impacts-performance) sont également applicables au stockage. Il est nécessaire d’avoir une bonne connaissance de la façon dont le système d’exploitation gère les e/s pour appliquer ces concepts. Dans le système d’exploitation Microsoft Windows, une file d’attente de stockage des demandes d’e/s est créée pour chaque disque physique. Toutefois, une clarification sur le disque physique doit être effectuée. Les contrôleurs de groupe et les réseaux San présentent des agrégations de piles de disques au système d’exploitation en tant que disques physiques uniques. En outre, les contrôleurs de groupe et les réseaux SAN peuvent regrouper plusieurs disques en un seul ensemble de baies, puis fractionner ce groupe en plusieurs « partitions », qui sont ensuite présentées au système d’exploitation sous la forme de plusieurs disques physiques (Ref. figures).

![Bloquer les piles](media/capacity-planning-considerations-block-spindles.png)  

Dans cette figure, les deux axes sont mis en miroir et répartis en zones logiques pour le stockage de données (Data 1 et Data 2). Ces zones logiques sont affichées par le système d’exploitation en tant que disques physiques distincts.

Bien que cela puisse être très confus, la terminologie suivante est utilisée dans cette annexe pour identifier les différentes entités :

- **Broche :** appareil physiquement installé sur le serveur.
- **Tableau :** collection de piles agrégées par contrôleur.
- **Partition de tableau :** partitionnement du tableau agrégé
- **Lun :** tableau utilisé pour la référence à des réseaux San
- **Disque –** Ce que le système d’exploitation observe comme un seul disque physique.
- **Partition :** partitionnement logique de ce que le système d’exploitation perçoit en tant que disque physique.

### <a name="operating-system-architecture-considerations"></a>Considérations relatives à l’architecture du système d’exploitation

Le système d’exploitation crée une file d’attente d’e/s FIFO (premier entré/premier sorti) pour chaque disque observé ; ce disque peut représenter un axe, un tableau ou une partition de tableau. Du point de vue du système d’exploitation, en ce qui concerne la gestion des e/s, plus la file d’attente est active, plus les files d’attente sont performantes. Une file d’attente FIFO est sérialisée, ce qui signifie que toutes les e/s émises vers le sous-système de stockage doivent être traitées dans l’ordre d’arrivée de la demande. En mettant en corrélation chaque disque observé par le système d’exploitation avec un arbre/matrice, le système d’exploitation gère maintenant une file d’attente d’e/s pour chaque ensemble unique de disques, ce qui élimine la contention des ressources d’e/s rares sur les disques et isole les demandes d’e/s sur un seul disque. En guise d’exception, Windows Server 2008 introduit le concept de hiérarchisation des e/s, et les applications conçues pour utiliser la priorité « basse » sont classées dans cet ordre normal et reprennent un siège. Les applications qui ne sont pas spécifiquement codées pour tirer parti de la priorité « basse » par défaut « normal ».

### <a name="introducing-simple-storage-subsystems"></a>Présentation des sous-systèmes de stockage simples

En commençant par un exemple simple (un seul disque dur à l’intérieur d’un ordinateur), une analyse composant par composant est donnée. En décomposant les principaux composants du sous-système de stockage, le système se compose des éléments suivants :

- **1-** 10 000 rpm Ultra Fast SCSI HD (Ultra Fast SCSI avec un taux de transfert de 20 Mo/s)
- **1 –** Bus SCSI (câble)
- **1 –** Carte Ultra-Fast SCSI
- bus PCI **1** -32 bits 33 MHz

Une fois les composants identifiés, une idée de la quantité de données pouvant transiter par le système, ou de la quantité d’e/s pouvant être gérées, peut être calculée. Notez que la quantité d’e/s et la quantité de données qui peuvent transiter par le système sont corrélées, mais pas les mêmes. Cette corrélation varie selon que les e/s disque sont aléatoires ou séquentielles et la taille de bloc. (Toutes les données sont écrites sur le disque sous la forme d’un bloc, mais différentes applications utilisent des tailles de bloc différentes.) Sur une base composant par composant :

- **Le disque dur :** Le disque dur 10 000-RPM moyen a une durée de recherche de 7 millisecondes (MS) et un temps d’accès de 3 ms. Le temps de recherche est la durée moyenne pendant laquelle la tête de lecture/écriture est déplacée vers un emplacement sur le plateau. Le temps d’accès est la durée moyenne nécessaire pour lire ou écrire les données sur le disque, une fois que la tête se trouve à l’emplacement approprié. Ainsi, la durée moyenne de lecture d’un bloc de données unique dans une HD de 10 000 RPM constitue une recherche et un accès, pour un total d’environ 10 ms (ou .010 secondes) par bloc de données.

  Lorsque chaque accès au disque nécessite un déplacement du début vers un nouvel emplacement sur le disque, le comportement de lecture/écriture est appelé « aléatoire ». Ainsi, lorsque toutes les e/s sont aléatoires, une HD de 10 000 RPM peut gérer environ 100 e/s par seconde (IOPS) (la formule est de 1000 ms par seconde divisé par 10 ms par e/s ou 1 000/10 = 100 IOPS).

  En guise d’alternative, lorsque toutes les e/s se produisent à partir de secteurs adjacents sur la HD, on parle d’e/s séquentielles. Les e/s séquentielles n’ont pas de temps de recherche car, lorsque la première e/s est terminée, la tête de lecture/écriture se trouve au début de l’emplacement où le bloc de données suivant est stocké sur le disque dur. Ainsi, un disque dur de 10 000 RPM peut gérer environ 333 e/s par seconde (1000 ms par seconde divisé par 3 ms par e/s).

  >[!NOTE]
  >Cet exemple ne reflète pas le cache disque, où les données d’un cylindre sont généralement conservées. Dans ce cas, les 10 ms sont nécessaires sur la première e/s et le disque lit l’ensemble du cylindre. Toutes les autres e/s séquentielles sont satisfaites à partir du cache. Par conséquent, les caches dans le disque peuvent améliorer les performances d’e/s séquentielles.
  
  Jusqu’à présent, le taux de transfert du disque dur n’a pas d’importance. Qu’il s’agisse d’un disque dur de 20 Mo/s ou d’un disque Ultra3 160 Mo/s, le volume d’e/s par seconde réel pouvant être géré par la HD 10 000 RPM est de ~ 100 ou ~ 300 e/s séquentielles. Lorsque la taille des blocs change en fonction de l’écriture de l’application sur le lecteur, la quantité de données extraites par e/s est différente. Par exemple, si la taille de bloc est de 8 Ko, 100 opérations d’e/s sont lues ou écrites sur le disque dur un total de 800 Ko. Toutefois, si la taille de bloc est de 32 Ko, 100 e/s lit/écrit 3 200 Ko (3,2 Mo) sur le disque dur. Tant que le taux de transfert SCSI dépasse la quantité totale de données transférées, l’obtention d’un taux de transfert « plus rapide » n’aura aucun effet. Consultez les tableaux suivants pour la comparaison.

  | |7200 RPM 9ms Seek, 4 MS Access|10 000 RPM 7 ms Seek, 3 MS Access|15 000 RPM 4 ms Seek, 2 ms accès
  |-|-|-|-|
  |E/s aléatoires|80|100|150|
  |E/s séquentielles|250|300|500|  
  
  |Lecteur RPM 10 000|taille de bloc de 8 Ko (Active Directory jet)|
  |-|-|
  |E/s aléatoires|800 Ko/s|
  |E/s séquentielles|2400 Ko/s|

- **Fond de panier SCSI (bus) :** La compréhension de la façon dont le « fond de panier SCSI (bus) », ou dans ce scénario, le câble ruban, influe sur le débit du sous-système de stockage dépend de la connaissance de la taille de bloc. La question est essentiellement la suivante : quelle quantité d’e/s peut être gérée par le bus si les e/s sont des blocs de 8 Ko ? Dans ce scénario, le bus SCSI est de 20 Mo/s, soit 20480 Ko/s. 20480 Ko/s divisées par blocs de 8 Ko entraînent un maximum de 2500 IOPS environ pris en charge par le bus SCSI.

  >[!NOTE]
  >Les figures du tableau suivant représentent un exemple. La plupart des périphériques de stockage attachés utilisent actuellement PCI Express, qui offre un débit nettement plus élevé.  
  
  |E/s prises en charge par le bus SCSI par taille de bloc|taille de bloc de 2 Ko|taille de bloc de 8 Ko (AD jet) (SQL Server 7.0/SQL Server 2000)
  |-|-|-|
  |20 Mo/s|10 000|2 500|
  |40 Mo/s|20 000|5 000|
  |128 Mo/s|65 536|16 384|
  |320 Mo/s|160 000|40 000|

  Comme vous pouvez le déterminer à partir de ce graphique, dans le scénario présenté, quelle que soit l’utilisation, le bus ne sera jamais un goulot d’étranglement, car le maximum de l’axe est de 100 e/s, bien en dessous de l’un des seuils ci-dessus.

  >[!NOTE]
  >Cela suppose que le bus SCSI est 100% efficace.
  
- **Carte SCSI :** Pour déterminer la quantité d’e/s que cela peut traiter, les spécifications du fabricant doivent être vérifiées. Le fait de diriger les demandes d’e/s vers l’appareil approprié nécessite le traitement d’un certain type de tri, de sorte que la quantité d’e/s pouvant être gérées dépend du processeur de la carte SCSI (ou du contrôleur de groupe).

  Dans cet exemple, l’hypothèse selon laquelle les e/s 1 000 peuvent être gérées est effectuée.

- **Bus PCI –** Il s’agit d’un composant souvent négligé. Dans cet exemple, il ne s’agit pas du goulot d’étranglement ; Cependant, à mesure que les systèmes évoluent, cela peut devenir un goulet d’étranglement. À des fins de référence, un bus PCI 32 bits fonctionnant sur 33Mhz peut, en théorie, transférer 133 Mo/s de données. Voici l’équation :  
  > 32 bits &divide; 8 bits par octet &times; 33 MHz = 133 Mo/s.  

  Notez que est la limite théorique ; en réalité, seulement environ 50% du maximum sont atteints, bien que dans certains scénarios de rafales, il est possible d’obtenir une efficacité de 75% pendant de courtes périodes.

  Un bus PCI 64 bits de 66 MHz peut prendre en charge un maximum théorique de (64 bits &divide; 8 bits par octet &times; 66 MHz) = 528 Mo/s. en outre, tout autre périphérique (tel que la carte réseau, le deuxième contrôleur SCSI, etc.) réduira la bande passante disponible lorsque la bande passante est partagée et que les appareils sont en concurrence pour les ressources limitées

Après l’analyse des composants de ce sous-système de stockage, l’axe est le facteur limitatif de la quantité d’e/s qui peut être demandée et, par conséquent, la quantité de données qui peuvent transiter par le système. Plus précisément, dans un scénario de AD DS, il s’agit de 100 e/s aléatoires par seconde par incréments de 8 Ko, soit un total de 800 Ko par seconde lors de l’accès à la base de données Jet. En guise d’alternative, le débit maximal pour une pile qui est exclusivement allouée aux fichiers journaux peut présenter les limitations suivantes : 300 e/s séquentielles par seconde par incréments de 8 Ko, soit un total de 2400 Ko (2,4 Mo) par seconde.

Maintenant, après avoir analysé une configuration simple, le tableau suivant montre où le goulot d’étranglement se produira lorsque les composants du sous-système de stockage seront modifiés ou ajoutés.

|Remarques|Analyse des goulots d’étranglement|Disque|Bus|Carte|Bus PCI|
|-|-|-|-|-|-|
|Il s’agit de la configuration du contrôleur de domaine après l’ajout d’un deuxième disque. La configuration du disque représente le goulot d’étranglement à 800 Ko/s.|Ajouter 1 disque (total = 2)<br /><br />Les e/s sont aléatoires<br /><br />taille de bloc de 4 Ko<br /><br />HD 10 000 RPM|200 e/s au total<br />800 Ko/s au total.| | | |
|Après l’ajout de 7 disques, la configuration du disque représente toujours le goulot d’étranglement à 3200 Ko/s.|**Ajouter 7 disques (total = 8)**  <br /><br />Les e/s sont aléatoires<br /><br />taille de bloc de 4 Ko<br /><br />HD 10 000 RPM|800 e/s au total.<br />3200 Ko/s au total| | | |
|Après la modification des e/s en séquence séquentielle, la carte réseau devient le goulot d’étranglement, car elle est limitée à 1000 e/s par seconde.|Ajouter 7 disques (total = 8)<br /><br />**Les e/s sont séquentielles**<br /><br />taille de bloc de 4 Ko<br /><br />HD 10 000 RPM| | |2400 e/s par seconde peuvent être lues/écrites sur le disque, contrôleur limité à 1000 IOPS| |
|Après avoir remplacé la carte réseau par une carte SCSI qui prend en charge 10 000 e/s par seconde, le goulot d’étranglement revient à la configuration du disque.|Ajouter 7 disques (total = 8)<br /><br />Les e/s sont aléatoires<br /><br />taille de bloc de 4 Ko<br /><br />HD 10 000 RPM<br /><br />**Mettre à niveau la carte SCSI (prend désormais en charge les e/s 10 000)**|800 e/s au total.<br />3 200 Ko/s au total| | | |
|Après avoir augmenté la taille de bloc à 32 Ko, le bus devient le goulot d’étranglement, car il ne prend en charge que 20 Mo/s.|Ajouter 7 disques (total = 8)<br /><br />Les e/s sont aléatoires<br /><br />**taille de bloc de 32 Ko**<br /><br />HD 10 000 RPM| |800 e/s au total. 25 600 Ko/s (25 Mo/s) peuvent être lus/écrits sur le disque.<br /><br />Le bus prend uniquement en charge 20 Mo/s| | |
|Après la mise à niveau du bus et l’ajout de disques supplémentaires, le disque reste le goulot d’étranglement.|**Ajouter 13 disques (total = 14)**<br /><br />Ajouter une deuxième carte SCSI avec 14 disques<br /><br />Les e/s sont aléatoires<br /><br />taille de bloc de 4 Ko<br /><br />HD 10 000 RPM<br /><br />**Mise à niveau vers le bus SCSI 320 Mo/s**|2800 e/s<br /><br />11 200 Ko/s (10,9 Mo/s)| | | |
|Après la modification des e/s en séquence séquentielle, le disque reste le goulot d’étranglement.|Ajouter 13 disques (total = 14)<br /><br />Ajouter une deuxième carte SCSI avec 14 disques<br /><br />**Les e/s sont séquentielles**<br /><br />taille de bloc de 4 Ko<br /><br />HD 10 000 RPM<br /><br />Mise à niveau vers le bus SCSI 320 Mo/s|8 400 e/s<br /><br />33 600 KB\s<br /><br />(32,8 MB\s)| | | |
|Après l’ajout de disques durs plus rapides, le disque reste le goulot d’étranglement.|Ajouter 13 disques (total = 14)<br /><br />Ajouter une deuxième carte SCSI avec 14 disques<br /><br />Les e/s sont séquentielles<br /><br />taille de bloc de 4 Ko<br /><br />**HD 15 000 RPM**<br /><br />Mise à niveau vers le bus SCSI 320 Mo/s|14 000 e/s<br /><br />56 000 Ko/s<br /><br />(54,7 Mo/s)| | | |
|Après avoir augmenté la taille de bloc à 32 Ko, le bus PCI devient le goulot d’étranglement.|Ajouter 13 disques (total = 14)<br /><br />Ajouter une deuxième carte SCSI avec 14 disques<br /><br />Les e/s sont séquentielles<br /><br />**taille de bloc de 32 Ko**<br /><br />HD 15 000 RPM<br /><br />Mise à niveau vers le bus SCSI 320 Mo/s| | | |14 000 e/s<br /><br />448 000 Ko/s<br /><br />(437 Mo/s) est la limite de lecture/écriture à l’axe.<br /><br />Le bus PCI prend en charge un maximum théorique de 133 Mo/s (75% efficace au mieux).|

### <a name="introducing-raid"></a>Présentation de RAID

La nature d’un sous-système de stockage ne change pas de façon spectaculaire lorsqu’un contrôleur de baie est introduit ; Il remplace simplement la carte SCSI dans les calculs. Ce qui change est le coût de la lecture et de l’écriture des données sur le disque lors de l’utilisation des différents niveaux de groupe (tels que RAID 0, RAID 1 ou RAID 5).

En RAID 0, les données sont agrégées par bandes sur tous les disques de l’ensemble RAID. Cela signifie qu’au cours d’une opération de lecture ou d’écriture, une partie des données est extraite ou envoyée à chaque disque, ce qui permet d’améliorer la quantité de données qui peuvent transiter le système pendant la même période. Ainsi, en une seconde, sur chaque pile (à nouveau en supposant que les lecteurs 10 000-RPM), 100 opérations d’e/s peuvent être effectuées. La quantité totale d’e/s pouvant être prises en charge est de N piles de 1 à 100 e/s par seconde par pile (produit 100 * N e/s par seconde).

![Lecteur logique d :](media/capacity-planning-considerations-logical-d-drive.png)

En RAID 1, les données sont mises en miroir (dupliquées) sur une paire de piles de disques à des fins de redondance. Ainsi, lorsqu’une opération d’e/s de lecture est effectuée, les données peuvent être lues à partir des deux piles de l’ensemble. Cela rend la capacité d’e/s des deux disques disponible au cours d’une opération de lecture. L’inconvénient est que les opérations d’écriture n’offrent aucun avantage en matière de performances dans un RAID 1. Cela est dû au fait que les mêmes données doivent être écrites sur les deux disques pour des raisons de redondance. Bien qu’il ne prenne plus de temps, car l’écriture de données se produit simultanément sur les deux axes, étant donné que les deux axes sont occupés à dupliquer les données, une opération d’e/s en écriture empêche deux opérations de lecture de se produire. Ainsi, chaque e/s d’écriture coûte deux e/s en lecture. Une formule peut être créée à partir de ces informations pour déterminer le nombre total d’opérations d’e/s qui se produisent :  

> E/s *en lecture* + 2 &times; *e/s d’écriture* = *e/s disque disponibles totales consommées*  

Lorsque le rapport entre les lectures et le nombre d’axes est connu, l’équation suivante peut être dérivée de l’équation ci-dessus pour identifier les e/s maximales qui peuvent être prises en charge par le tableau :  

> *Nombre maximal d’e/s par seconde par fuseau horaire* &times; 2 piles de disques&times; [( *%lectures*  +  *%écritures*) &divide;( *%lectures* + 2&times; *%d’écritures* )]= *nombretotald’e/sparseconde*

RAID 1 + 0, se comporte exactement de la même façon que RAID 1 en ce qui concerne les dépenses de lecture et d’écriture. Toutefois, l’e/s est maintenant entrelacée sur chaque jeu en miroir. Si l’option  

> *Nombre maximal d’e/s par seconde par fuseau horaire* &times; 2 piles de disques&times; [( *%lectures*  +  *%écritures*) &divide;( *%lectures* + 2&times; *%d’écritures* )]= *nombretotald’e/sparseconde*  

dans un jeu RAID 1, lorsqu’une multiplicité (*N*) d’ensembles RAID 1 est agrégée, les e/s totales qui peuvent être traitées deviennent N &times; e/s par RAID 1 défini :  

> *N* &times;{ *nombre maximal d’e/s par seconde par axe de broche* &times; 2 &times;[( *% lectures* +  *%écritures*) &divide; ( *%de lectures* + 2 &times; *% d’écritures* )]} = *Nombre total d’IOPS*

En RAID 5, parfois appelés RAID *n* + 1, les données sont agrégées par bandes sur *n* piles et les informations de parité sont écrites sur l’axe « + 1 ». Toutefois, le RAID 5 est beaucoup plus onéreux lors de l’exécution d’e/s d’écriture que RAID 1 ou 1 + 0. RAID 5 effectue le processus suivant chaque fois qu’une e/s d’écriture est soumise au tableau :

1. Lire les anciennes données
1. Lire l’ancienne parité
1. Écrire les nouvelles données
1. Écrire la nouvelle parité

Étant donné que chaque demande d’e/s d’écriture soumise au contrôleur de groupe par le système d’exploitation nécessite l’exécution de quatre opérations d’e/s, les demandes d’écriture envoyées prennent quatre fois plus de temps que l’exécution d’une seule e/s de lecture. Pour dériver une formule afin de traduire les demandes d’e/s du point de vue du système d’exploitation vers celles rencontrées par les piles :  

> E/s *en lecture* + 4 &times; *e/s d’écriture* = *e/s totales*  

De même, dans un jeu RAID 1, lorsque le rapport entre les lectures et les écritures et le nombre de piles est connu, l’équation suivante peut être dérivée de l’équation ci-dessus pour identifier les e/s maximales pouvant être prises en charge par le tableau (Notez que le nombre total de piles n’inclut pas le « lecteur » perdu à la parité) :  

> *IOPS par fuseau horaire* &times; (*Piles* – 1) &times;[( *% lectures* +  *% écritures*) &divide; ( *%lectures* + 4 &times; *% d’écritures*)] = *nombre total d’e/s par seconde*

### <a name="introducing-sans"></a>Présentation de San

En développant la complexité du sous-système de stockage, lorsqu’un réseau SAN est introduit dans l’environnement, les principes de base décrits ne changent pas, mais le comportement des e/s pour tous les systèmes connectés au réseau SAN doit être pris en compte. L’un des principaux avantages de l’utilisation d’un réseau SAN est une quantité supplémentaire de redondance sur un stockage attaché en interne ou en externe, et la planification de la capacité doit maintenant prendre en compte les besoins de tolérance de panne. En outre, d’autres composants ont été introduits et doivent être évalués. Fractionnement d’un réseau SAN en plusieurs composants :

- Disque dur SCSI ou Fibre Channel
- Fond de panier du canal d’unité de stockage
- Unités de stockage
- Module contrôleur de stockage
- Commutateurs SAN
- HBA (s)
- Bus PCI

Lors de la conception d’un système de redondance, des composants supplémentaires sont inclus pour s’adapter au potentiel de défaillance. Il est très important, lors de la planification de la capacité, d’exclure le composant redondant des ressources disponibles. Par exemple, si le réseau SAN a deux modules de contrôleur, la capacité d’e/s d’un module de contrôleur est tout ce qui doit être utilisé pour le débit d’e/s total disponible pour le système. Cela est dû au fait que si un contrôleur tombe en panne, l’ensemble de la charge d’e/s exigée par tous les systèmes connectés devra être traité par le contrôleur restant. Étant donné que toutes les planifications de capacité sont effectuées pour les périodes d’utilisation maximale, les composants redondants ne doivent pas être pris en compte dans les ressources disponibles et les pics d’utilisation planifiés ne doivent pas dépasser 80% de la saturation du système (afin de prendre en charge les pics ou le système anormal comportement). De même, le commutateur SAN, l’unité de stockage et les piles de disques redondants ne doivent pas être factorisés dans les calculs d’e/s.

Lors de l’analyse du comportement du disque dur SCSI ou Fibre Channel, la méthode d’analyse du comportement comme indiqué précédemment ne change pas. Bien qu’il y ait certains avantages et inconvénients pour chaque protocole, le facteur de limitation sur une base par disque est la limitation mécanique du disque dur.

L’analyse du canal sur l’unité de stockage est exactement la même que le calcul des ressources disponibles sur le bus SCSI, ou la bande passante (par exemple, 20 Mo/s) divisée par la taille de bloc (8 Ko, par exemple). Dans ce cas, l’agrégation de plusieurs canaux s’écarte de l’exemple simple précédent. Par exemple, s’il existe 6 canaux, chacun prenant en charge un taux de transfert maximal de 20 Mo/s, la quantité totale d’e/s et de transfert de données disponible est de 100 Mo/s (cette valeur est correcte, il ne s’agit pas de 120 Mo/s). Là encore, la tolérance de panne est un acteur majeur dans ce calcul, en cas de perte d’un canal entier, le système n’est laissé qu’avec 5 canaux opérationnels. Ainsi, pour s’assurer que les performances sont satisfaisantes en cas de défaillance, le débit total de tous les canaux de stockage ne doit pas dépasser 100 Mo/s (cela suppose que la charge et la tolérance de pannes sont réparties uniformément sur tous les canaux). L’activation d’un profil d’e/s dépend du comportement de l’application. Dans le cas des e/s de Active Directory jet, cela se traduirait par une mise en corrélation d’environ 12 500 e/s par seconde (100 Mo/s &divide; 8 Ko par e/s).

Ensuite, vous devez obtenir les spécifications du fabricant pour les modules de contrôleur afin d’avoir une bonne compréhension du débit que chaque module peut prendre en charge. Dans cet exemple, le réseau SAN a deux modules de contrôleur qui prennent en charge les e/s 7 500 chacune. Le débit total du système peut être de 15 000 e/s par seconde si la redondance n’est pas souhaitée. Dans le calcul du débit maximal en cas d’échec, la limitation correspond au débit d’un contrôleur, ou à 7 500 e/s par seconde. Ce seuil est bien inférieur au nombre maximal de 12 500 d’e/s par seconde (en supposant une taille de bloc de 4 Ko) qui peut être pris en charge par tous les canaux de stockage et, par conséquent, est actuellement le goulot d’étranglement dans l’analyse. Toujours à des fins de planification, les e/s maximales souhaitées doivent être de 10 400 e/s.

Lorsque les données quittent le module de contrôleur, il transite une connexion Fibre Channel évaluée à 1 Go/s (ou 1 Gigabit par seconde). Pour corréler cela avec les autres métriques, 1 Go/s se transforme en 128 Mo/s (1 Go/s &divide; 8 bits/octet). Comme cela dépasse la bande passante totale sur tous les canaux de l’unité de stockage (100 Mo/s), cela n’engorge pas le système. En outre, comme il ne s’agit que d’un des deux canaux (la connexion supplémentaire de 1 Go/s Fibre Channel pour la redondance), si une connexion échoue, la connexion restante a toujours une capacité suffisante pour gérer tous les transferts de données demandés.

En route vers le serveur, les données transitent probablement un commutateur SAN. Étant donné que le commutateur SAN doit traiter la demande d’e/s entrante et le transférer vers le port approprié, le commutateur aura une limite à la quantité d’e/s pouvant être gérées. Toutefois, les spécifications du fabricant seront requises pour déterminer la limite. Par exemple, s’il existe deux commutateurs et que chaque commutateur peut traiter 10 000 e/s par seconde, le débit total sera de 20 000 IOPS. Là encore, la tolérance de panne est un problème, en cas de défaillance d’un commutateur, le débit total du système sera de 10 000 e/s par seconde. Comme il est souhaitable de ne pas dépasser 80% d’utilisation en fonctionnement normal, l’utilisation de 8000 e/s au maximum doit être la cible.

Enfin, le HBA installé sur le serveur est également limité au nombre d’e/s qu’il peut traiter. En règle générale, un deuxième adaptateur HBA est installé pour la redondance, mais comme avec le commutateur SAN, lorsque vous calculez des e/s maximales pouvant être traitées, le débit total de *N* &ndash; 1 HBA est l’évolutivité maximale du système.

### <a name="caching-considerations"></a>Considérations relatives à la mise en cache

Les caches sont l’un des composants qui peuvent avoir un impact significatif sur les performances globales à tout moment dans le système de stockage. L’analyse détaillée des algorithmes de mise en cache dépasse le cadre de cet article. Toutefois, certaines instructions de base sur la mise en cache sur les sous-systèmes de disque doivent éclairer :

- La mise en cache permet d’améliorer les e/s d’écriture séquentielles, car elle peut mettre en mémoire tampon de nombreuses opérations d’écriture plus petites dans des blocs d’e/s plus volumineux et la déphase vers le stockage en moins de tailles de blocs plus volumineuses. Cela permet de réduire le nombre total d’e/s aléatoires et d’e/s séquentielles, ce qui permet d’augmenter la disponibilité des ressources pour les autres e/s.
- La mise en cache n’améliore pas le débit d’e/s d’écriture soutenu du sous-système de stockage. Elle permet uniquement aux écritures d’être mises en mémoire tampon jusqu’à ce que les piles soient disponibles pour valider les données. Lorsque toutes les e/s disponibles des piles dans le sous-système de stockage sont saturées pendant de longues périodes, le cache finit par se remplir. Pour vider le cache, il est nécessaire d’allouer suffisamment de temps entre les rafales, ou les piles supplémentaires, afin de fournir suffisamment d’e/s pour permettre le vidage du cache.

  Les caches plus grands autorisent uniquement la mise en mémoire tampon de données. Cela signifie que les périodes de saturation plus longues peuvent être prises en charge.

  Dans un sous-système de stockage de fonctionnement normal, le système d’exploitation présente des performances d’écriture améliorées, car les données doivent uniquement être écrites dans le cache. Une fois que le média sous-jacent est saturé avec des e/s, le cache est rempli et les performances en écriture sont retournées à la vitesse du disque.

- Lors de la mise en cache des e/s de lecture, le scénario dans lequel le cache est le plus avantageux est lorsque les données sont stockées de façon séquentielle sur le disque, et le cache peut lire en continu (cela fait supposer que le secteur suivant contient les données qui seront demandées ensuite).
- Lorsque les e/s de lecture sont aléatoires, il est peu probable que la mise en cache sur le contrôleur de disque offre une amélioration de la quantité de données pouvant être lues à partir du disque. Toute amélioration est inexistante si la taille du cache basé sur le système d’exploitation ou l’application est supérieure à la taille du cache matériel.

  Dans le cas de Active Directory, le cache n’est limité que par la quantité de RAM.

### <a name="ssd-considerations"></a>Considérations sur les disques SSD

Les disques SSD sont un animal complètement différent des disques durs basés sur des piles. Toutefois, les deux critères clés restent : « combien d’e/s par seconde peut-il gérer ? ». et « quelle est la latence pour ces e/s par seconde ? » En comparaison avec les disques durs basés sur des piles, les disques SSD peuvent gérer des volumes d’e/s plus élevés et peuvent avoir des latences inférieures. En général et au moment de la rédaction de cet article, tandis que les disques SSD sont toujours onéreux dans une comparaison coût par gigaoctet, ils sont très bon marché en termes de coût par e/s et méritent une attention particulière en termes de performances de stockage.

Éléments à prendre en considération :

- Les e/s par seconde et les latences sont très subjectives pour les conceptions du fabricant et, dans certains cas, ont été observées comme moins performantes que les technologies basées sur les piles. En bref, il est plus important de passer en revue et de valider le lecteur de spécifications du fabricant par lecteur et de ne pas supposer les généralistes.
- Les types d’e/s par seconde peuvent avoir des nombres très différents selon qu’ils sont en lecture ou en écriture. AD DS services, en général, principalement basés sur la lecture, seront moins affectés que d’autres scénarios d’application.
- « Endurance d’écriture » : il s’agit du concept que les cellules SSD finiront par s’occuper. Différents fabricants traitent ce défi de manière différente. Au moins pour le lecteur de base de données, le profil d’e/s de lecture prédominante permet à downplaying de déterminer la signification de ce problème, car les données ne sont pas très volatiles.

### <a name="summary"></a>Récapitulatif

L’une des façons de penser au stockage est le illustrer domestique. Imaginez que les e/s par seconde du support sur lequel les données sont stockées sont le principal drain du ménage. Lorsque cette opération est obstruée (par exemple, les racines dans le canal) ou limitée (elle est réduite ou trop petite), tous les récepteurs dans le ménage sont sauvegardés quand un trop grand volume d’eau est utilisé (trop d’invités). Cela est parfaitement similaire à un environnement partagé dans lequel un ou plusieurs systèmes tirent parti du stockage partagé sur un réseau SAN/NAS/iSCSI avec le même support sous-jacent. Différentes approches peuvent être prises pour résoudre les différents scénarios :

- Une vidange réduite ou sous-dimensionnée nécessite un remplacement et une correction à l’échelle complète. Cela serait similaire à l’ajout d’un nouveau matériel ou à la redistribution des systèmes à l’aide du stockage partagé dans toute l’infrastructure.
- Un canal « obstrué » signifie généralement l’identification d’un ou de plusieurs problèmes incriminés et la suppression de ces problèmes. Dans un scénario de stockage, il peut s’agir de sauvegardes au niveau du stockage ou du système, d’analyses antivirus synchronisées sur tous les serveurs et de logiciels de défragmentation synchronisés qui s’exécutent pendant les périodes de pointe.

Dans toute conception de plomberie, plusieurs drains alimentent le principal drain. Si quelque chose arrête l’un de ces drains ou un point de jonction, seuls les éléments qui se trouvent derrière ce point de jonction sont sauvegardés. Dans un scénario de stockage, il peut s’agir d’un commutateur surchargé (scénario SAN/NAS/iSCSI), de problèmes de compatibilité de pilote (problème de pilote/microprogramme HBA/combinaison Storport. sys) ou de sauvegarde/antivirus/défragmentation. Pour déterminer si le « canal » de stockage est suffisamment grand, les e/s par seconde et la taille des e/s doivent être mesurées. À chaque jointure, ajoutez-les ensemble pour garantir un diamètre de tuyau adéquat.

## <a name="appendix-d---discussion-on-storage-troubleshooting---environments-where-providing-at-least-as-much-ram-as-the-database-size-is-not-a-viable-option"></a>Annexe D-discussion sur la résolution des problèmes de stockage-environnements où fournir au moins autant de RAM que la taille de la base de données n’est pas une option viable

Il est utile de comprendre pourquoi ces recommandations existent pour que les modifications apportées à la technologie de stockage puissent être prises en charge. Ces recommandations existent pour deux raisons. La première est l’isolation des e/s, de telle sorte que les problèmes de performances (c’est-à-dire la pagination) sur l’axe du système d’exploitation n’affectent pas les performances de la base de données et des profils d’e/s. Le deuxième est que les fichiers journaux de AD DS (et de la plupart des bases de données) sont séquentiels par nature, et les disques durs et caches basés sur des piles présentent un avantage en matière de performances lorsqu’ils sont utilisés avec des e/s séquentielles, par rapport aux modèles d’e/s aléatoires du système d’exploitation et aux modèles d’e/s de AD DS. En isolant les e/s séquentielles sur un lecteur physique distinct, le débit peut être augmenté. Le défi présenté par les options de stockage actuelles est que les hypothèses fondamentales sous-jacentes à ces recommandations ne sont plus vraies. Dans de nombreux scénarios de stockage virtualisés, tels que les fichiers d’image iSCSI, SAN, NAS et de disque virtuel, le support de stockage sous-jacent est partagé entre plusieurs hôtes, ce qui inverse complètement les aspects « isolation des e/s » et « optimisation des e/s séquentielles ». En fait, ces scénarios ajoutent une couche de complexité supplémentaire dans le fait que les autres hôtes qui accèdent au support partagé peuvent nuire à la réactivité du contrôleur de domaine.

Pour la planification des performances de stockage, vous devez prendre en compte trois catégories : état du cache froid, état du cache chauffé et sauvegarde/restauration. L’état du cache à froid se produit dans des scénarios tels que lorsque le contrôleur de domaine est redémarré pour la première fois ou que le service Active Directory est redémarré et qu’il n’y a aucune donnée Active Directory dans la mémoire vive. L’état du cache à chaud est l’endroit où le contrôleur de domaine se trouve dans un état stable et où la base de données est mise en cache. Il est important de noter qu’ils vont générer des profils de performances très différents et disposer d’une mémoire RAM suffisante pour mettre en cache la totalité de la base de données, ce qui n’aide pas les performances lorsque le cache est froid. Vous pouvez envisager une conception des performances pour ces deux scénarios avec l’analogie suivante, le préchauffage du cache à froid étant un « sprint » et l’exécution d’un serveur avec un cache à chaud est un « marathon ».

Pour le scénario du cache à froid et du cache à chaud, la question devient la vitesse à laquelle le stockage peut déplacer les données du disque en mémoire. Le préchauffage du cache est un scénario dans lequel, au fil du temps, les performances s’améliorent à mesure que le nombre de requêtes réutilise les données, que le taux d’accès au cache augmente et que la fréquence d’accès au disque diminue. En conséquence, l’impact négatif sur les performances du disque diminue. Toute dégradation des performances est uniquement temporaire en attendant que le cache chauffe et augmente jusqu’à la taille maximale autorisée dépendante du système. La conversation peut être simplifiée en fonction de la vitesse à laquelle les données peuvent être extraites du disque. il s’agit d’une mesure simple des e/s par seconde disponibles pour Active Directory, qui est subjective pour les e/s par seconde du stockage sous-jacent. Du point de vue de la planification, dans la mesure où le préchauffage du cache et les scénarios de sauvegarde/restauration se produisent de façon exceptionnelle, se produisent normalement en dehors des heures et sont subjectifs pour la charge du contrôleur de l’État, les recommandations générales n’existent pas, sauf dans le sens où ces activités sont planifiées pour des heures creuses.

AD DS, dans la plupart des scénarios, est principalement en lecture e/s, ce qui correspond généralement à un ratio de 90% de lecture/10% d’écriture. Les e/s de lecture ont souvent tendance à être le goulot d’étranglement de l’expérience utilisateur, et les e/s en écriture entraînent une dégradation des performances d’écriture. Étant donné que les e/s à NTDS. dit sont principalement aléatoires, les caches ont tendance à offrir un avantage minimal pour la lecture des e/s, ce qui complique la configuration du stockage pour le profil d’e/s de lecture correctement.

Pour les conditions de fonctionnement normales, l’objectif de la planification du stockage est de réduire le temps d’attente pendant lequel une demande de AD DS être retournée à partir du disque. Cela signifie essentiellement que le nombre d’e/s en suspens et en attente est inférieur ou égal au nombre de chemins d’entrée vers le disque. Il existe plusieurs façons de mesurer cela. Dans un scénario d’analyse des performances, la recommandation générale est que le disque logique ( *\<\>lecteur de base de données NTDS* ) \Avg s/lecture est inférieur à 20 ms. Le seuil de fonctionnement souhaité doit être nettement plus faible, de préférence aussi proche que possible de la vitesse du stockage, dans la plage de 2 à 6 millisecondes (. 002 à .006 seconde) en fonction du type de stockage.

Exemple :

![Graphique de latence de stockage](media/capacity-planning-considerations-storage-latency.png)

Analyse du graphique :

- **Ovale vert à gauche :** La latence reste cohérente à 10 ms. La charge augmente de 800 e/s par seconde à 2400 IOPS. Il s’agit de l’étage absolu de la vitesse à laquelle une demande d’e/s peut être traitée par le stockage sous-jacent. Cela est soumis aux spécificités de la solution de stockage.
- **Bandeau ovale situé à droite –** Le débit reste plat depuis la sortie du cercle vert jusqu’à la fin de la collecte de données, tandis que la latence continue d’augmenter. Cela démontre que lorsque les volumes de la demande dépassent les limites physiques du stockage sous-jacent, plus les demandes passent dans la file d’attente en attente d’envoi au sous-système de stockage.

Application de ces connaissances :

- **Impact sur l’interrogation d’un utilisateur d’un groupe important :** Supposons que vous devez lire 1 Mo de données sur le disque, la quantité d’e/s et le temps qu’il prend peut être évalué comme suit :
  - Active Directory pages de la base de données ont une taille de 8 Ko. 
  - Au moins 128 pages doivent être lues à partir du disque. 
  - En supposant que rien n’est mis en cache, à l’étage (10 ms), le chargement des données à partir du disque dure au moins 1,28 secondes afin de les renvoyer au client. À 20 ms, où le débit sur le stockage est long depuis la fin du délai d’expiration et est également la valeur maximale recommandée, il faut 2,5 secondes pour obtenir les données du disque afin de les renvoyer à l’utilisateur final.  
- **À quel taux le cache doit-il être chaud ?** En partant du principe que la charge du client va maximiser le débit sur cet exemple de stockage, le cache sera chaud à un débit de 2400 e/s par seconde &times; 8 Ko par e/s. Ou environ 20 Mo/s par seconde, en chargeant environ 1 Go de base de données en mémoire vive toutes les 53 secondes.

> [!NOTE]
> Il est normal, pendant de courtes périodes, d’observer les latences lorsque les composants lisent ou écrivent de manière agressive sur le disque, par exemple lorsque le système est sauvegardé ou lorsque AD DS exécute garbage collection. Une salle de tête supplémentaire en plus des calculs doit être fournie pour tenir compte de ces événements périodiques. L’objectif est de fournir un débit suffisant pour prendre en charge ces scénarios sans affecter la fonction normale.

Comme vous pouvez le voir, il existe une limite physique basée sur la conception du stockage jusqu’à la vitesse à laquelle le cache peut être chaud. La vitesse à laquelle le cache est chargé est celle des demandes client entrantes jusqu’à la fréquence que le stockage sous-jacent peut fournir. L’exécution de scripts pour « préchauffer » le cache pendant les heures de pointe fournira la concurrence pour la charge pilotée par les requêtes client réelles. Cela peut nuire à la transmission de données dont les clients ont besoin en premier lieu car, par conception, il génère la concurrence pour les ressources disque rares, car les tentatives artificielles de mise en mémoire cache chargent les données qui ne sont pas pertinentes pour les clients qui contactent le contrôleur de l’État.
