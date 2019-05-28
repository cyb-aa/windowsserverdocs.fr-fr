---
title: Nouveautés de Windows Server, version 1709
description: Présentation des nouvelles fonctionnalités de calcul, d’identité, de gestion, d’automatisation, de mise en réseau, de sécurité et de stockage.
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
ms.localizationpriority: medium
ms.openlocfilehash: 32ce591a8b50c6e35c3fde4fedb177b6d76fccdd
ms.sourcegitcommit: c8cc0b25ba336a2aafaabc92b19fe8faa56be32b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65976729"
---
# <a name="whats-new-in-windows-server-version-1709"></a>Nouveautés de Windows Server, version 1709

>S'applique à : Windows Server (canal semi-annuel)

<img src="../media/landing-icons/new.png" style='float:left; padding:.5em;' alt="Icon showing a newspaper">&nbsp;Pour en savoir plus sur les dernières fonctionnalités dans Windows, consultez [What ' s New in Windows Server](whats-new-in-windows-server.md). Le contenu de cette section décrit les nouveautés et les modifications de Windows Server, version 1709. Les nouvelles fonctionnalités et modifications répertoriées ici sont celles qui sont susceptibles d'avoir l'impact le plus important quand vous utilisez cette version. Voir également [Windows Server, version 1709](https://blogs.technet.microsoft.com/windowsserver/2017/08/24/sneak-peek-1-windows-server-version-1709/).
   

## <a name="new-cadence-of-releases"></a>Nouvelle cadence de versions

À compter de cette version, il existe deux façons de recevoir les mises à jour des fonctionnalités Windows Server :
- **À long terme (LTSC) de canal de maintenance**: Il s’agit d’entreprise comme d’habitude à l’aide de 5 ans de support standard et 5 ans de support étendu. Vous avez la possibilité dֹ’effectuer une mise à niveau vers la version suivante du canal de maintenance à long terme tous les 2 à 3 ans, comme cela est le cas depuis 20 ans.
- **Canal semi-annuel (SAC)** : Ceci est un avantage de Software Assurance et est entièrement pris en charge en production. La différence réside dans le fait qu’il est pris en charge pendant 18 mois et qu’une nouvelle version est disponible tous les six mois.

Les canaux de distribution sont résumés dans le tableau suivant.

|   | Canal semi-annuel | Canal de maintenance à long terme |
| ------------- | ------------- | ------------ |
| Cadence de publication  | Deux fois par an (printemps et automne)  | Tous les 2 ou 3 ans |
| Plan de support  | 18 mois de support standard en production  | 5 ans de support standard + 5 ans de support étendu |
| Disponibilité  | Software Assurance ou Azure (hébergement dans le cloud)  | Tous les canaux |
| Convention d’affectation de noms  | Windows Server, version AAMM  | Windows Server AAAA |

Pour plus d’informations, consultez [comparaison des canaux de maintenance](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview).

## <a name="application-containers-and-micro-services"></a>Conteneurs d’applications et micro-services

- L’image de conteneur Server Core a été optimisée pour les scénarios «lift-and-shift » qui permettent de migrer des bases de code ou des applications existantes dans des conteneurs avec très peu de modifications, et qui plus est une taille inférieure de 60 %. 
- L’image de conteneur Nano Server est réduite de près de 80 %.
    - Dans le canal semi-annuel Windows Server, l’image de système d’exploitation de base de conteneur Nano Server est réduite de 390 Mo à 80 Mo.
- Conteneurs Linux avec isolation Hyper-V 

Pour plus d’informations, voir [Modifications apportées à Nano Server dans la prochaine version de Windows Server](https://docs.microsoft.com/windows-server/get-started/nano-in-semi-annual-channel) et [Windows Server, version 1709 pour les développeurs](https://blogs.technet.microsoft.com/windowsserver/2017/09/13/sneak-peek-3-windows-server-version-1709-for-developers/).

## <a name="modern-management"></a>Gestion moderne

Découvrez le [projet Honolulu](https://docs.microsoft.com/windows-server/manage/honolulu/honolulu) qui offre aux administrateurs informatiques une expérience intégrée, simplifiée et sécurisée, facilitant la gestion des principaux scénarios de résolution des problèmes, de configuration et de maintenance.  Le projet Honolulu inclut la prochaine génération d’outils avec une interface simplifiée, intégrée, sécurisée et extensible.
Le projet Honolulu propose une expérience de gestion intuitive entièrement nouvelle pour la gestion des PC, serveurs Windows, clusters de basculement, mais également une infrastructure hyperconvergée basée sur les espaces de stockage direct, ce qui permet de réduire les coûts d’exploitation.

## <a name="compute"></a>Calcul :

**Conteneur NANO et Server Core conteneur**: D’abord, cette version est sur l’apport d’innovation de l’application. Nano Server ou Nano en tant qu’hôte est déconseillé et remplacé par le conteneur Nano, qui correspond à Nano exécuté en tant qu’image de conteneur. 

Pour plus d’informations sur les conteneurs, consultez [Présentation de la mise en réseau de conteneurs](https://docs.microsoft.com/windows-server/networking/sdn/technologies/containers/container-networking-overview).

L’hôte **Server Core en tant que conteneur** (et infrastructure) offre une plus grande flexibilité, davantage de densité et de meilleures performances pour les applications existantes dans le cadre d’un processus de modernisation et personnalise les nouvelles applications déjà développées à l’aide du modèle de cloud.

La fonction **Ordre de démarrage des ordinateurs virtuels** est également améliorée avec la reconnaissance des applications et du système d’exploitation. Elle apporte des déclencheurs améliorés pour déterminer si un ordinateur virtuel est considéré comme démarré avant le démarrage du suivant.

La **prise en charge de la mémoire de classe stockage pour les ordinateurs virtuels** permet la création de volumes NTFS à accès direct sur des éléments DIMM non volatiles et leur exposition à des ordinateurs virtuels Hyper\-V. Ainsi, les ordinateurs virtuels Hyper-V peuvent tirer parti des avantages des performances à faible latence des périphériques de mémoire de classe stockage.

La **mémoire persistante virtualisée (vPMEM)** est activée en créant un fichier VHD (.vhdpmem) sur un volume à accès direct sur un hôte, en ajoutant un contrôleur vPMEM à un ordinateur virtuel, puis en ajoutant le périphérique créé (.vhdpmem) à un ordinateur virtuel. L’utilisation de fichiers vhdpmem sur des volumes à accès direct sur un hôte pour sauvegarder la mémoire persistante virtualisée (vPMEM) offre une plus grande flexibilité d’allocation et tire parti d’un modèle de gestion familier pour l’ajout de disques à des ordinateurs virtuels.

**Stockage de conteneur : il s’agit de volumes de données persistantes sur des volumes partagés de cluster (CSV)** . Dans Windows Server, version 1709, ainsi que dans Windows Server 2016 disposant des dernières mises à jour, nous avons ajouté la prise en charge de l’accès par des conteneurs à des volumes de données persistantes situés sur des volumes partagés de cluster, y compris des volumes partagés de cluster dans les espaces de stockage direct. Ainsi, le conteneur d’applications dispose d’un accès persistant au volume, quel que soit le nœud de cluster sur lequel s’exécute l’instance de conteneur. Pour plus d’informations, voir le billet de blog [Container Storage Support with Cluster Shared Volumes (CSV), Storage Spaces Direct (S2D), SMB Global Mapping](https://blogs.msdn.microsoft.com/clustering/2017/08/10/container-storage-support-with-cluster-shared-volumes-csv-storage-spaces-direct-s2d-smb-global-mapping/).

**Stockage de conteneur : il s’agit de volumes de données persistants avec le mappage global SMB**. Dans Windows Server, version 1709, nous avons ajouté la prise en charge du mappage d’un partage de fichiers SMB à une lettre de lecteur à l’intérieur d’un conteneur. Ce mappage est appelé mappage global SMB. Ce lecteur mappé est ensuite accessible à tous les utilisateurs sur le serveur local afin que les E/S de conteneur sur le volume de données puissent transiter via le lecteur monté pour accéder au partage de fichiers sous-jacent. Pour plus d’informations, voir le billet de blog [Container Storage Support with Cluster Shared Volumes (CSV), Storage Spaces Direct (S2D), SMB Global Mapping](https://blogs.msdn.microsoft.com/clustering/2017/08/10/container-storage-support-with-cluster-shared-volumes-csv-storage-spaces-direct-s2d-smb-global-mapping/).

**Format de fichier de configuration d’ordinateur virtuel (mis à jour)** . Un fichier supplémentaire (.vmgs) a été ajouté pour les ordinateurs virtuels dont la version de configuration est 8.2 ou ultérieure. VMGS signifie « VM guest state » (état d’ordinateur virtuel invité) et est un nouveau fichier interne qui inclut l’état de l’appareil, lequel faisait précédemment partie du fichier d’état du runtime de l’ordinateur virtuel.

## <a name="security-and-assurance"></a>Sécurité et assurance

Le **chiffrement réseau** vous permet de rapidement chiffrer des segments de réseau sur une infrastructure réseau définie par logiciel afin de répondre aux exigences en matière de sécurité et de conformité.

**Service Guardian hôte (SGH)** en tant qu’ordinateur virtuel protégé est activé. Avant cette version, il était recommandé de déployer un cluster physique à 3 nœuds. Si ce déploiement garantit l’environnement SGH contre toute attaque d’un administrateur, il s’avère souvent très onéreux.

**Linux en tant qu’ordinateur virtuel protégé** est désormais pris en charge.

Pour plus d’informations, consultez [Présentation de l’infrastructure protégée et des ordinateurs virtuels protégés](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms).

**Vulnérabilité SMBLoris** Un problème appelé « SMBLoris », capable de provoquer un déni de service, a été résolu.

## <a name="storage"></a>Stockage

**Le réplica de stockage**: La protection de récupération d’urgence ajoutée par le réplica de stockage dans Windows Server 2016 est maintenant développée pour inclure :
- **Test de basculement** : le montage du stockage de destination est maintenant possible grâce à la fonctionnalité de test de basculement. Vous pouvez monter une capture instantanée du stockage répliqué sur les nœuds de destination de manière temporaire à des fins de test ou de sauvegarde.  Pour plus d'informations, consultez le [Forum Aux Questions sur le réplica de stockage](https://aka.ms/srfaq). 
- **Projet de prise en charge de Honolulu**: Prise en charge pour la gestion graphique de la réplication de serveur à serveur est désormais disponible dans Honolulu du projet. Cela supprime la nécessité d’utiliser PowerShell pour gérer une charge de travail de protection d’urgence courante.

**SMB** : 
- **Suppression de l’authentification SMB1 et invité**: Windows Server, version 1709 n’installe plus le SMB1 client et le serveur par défaut. En outre, la possibilité d’authentification en tant qu’invité dans SMB2 et versions ultérieures est désactivée par défaut. Pour plus d’informations, consultez [SMBv1 n’est pas installé par défaut dans Windows 10, version 1709 et Windows Server, version 1709](https://support.microsoft.com/help/4034314/smbv1-is-not-installed-by-default-in-windows-10-rs3-and-windows-server). 

- **Compatibilité et sécurité de SMB2/SMB3**: Options supplémentaires pour la compatibilité de sécurité et d’application ont été ajoutées, y compris la possibilité de désactiver oplocks dans SMB2 + pour les applications héritées, ainsi que d’exiger la signature ou le chiffrement sur chaque connexion à partir d’un client. Pour plus d’informations, consultez l’aide du module SMBShare PowerShell.

**Déduplication des données** : 
- **Prend en charge de la déduplication des données désormais ReFS**: Vous n’avez plus devez choisir entre les avantages d’un système de fichiers moderne avec ReFS et de la déduplication des données : désormais, vous pouvez activer la déduplication des données chaque fois que vous pouvez activer (Refs). Augmentez l’efficacité du stockage de plus de 95 % avec ReFS.
- **API de port d’entrée/sortie optimisée pour les volumes dédupliqués**: Les développeurs peuvent désormais tirer parti des connaissances de la déduplication des données a sur le stockage de données efficacement pour déplacer des données entre les volumes, serveurs et efficacement des clusters.

## <a name="remote-desktop-services-rds"></a>Services Bureau à distance

**Les Services Bureau à distance sont intégrés à Azure AD**, ce qui permet aux clients d’exploiter les stratégies d’accès conditionnel, l’authentification multifacteur, l’authentification intégrée avec d’autres applications SaaS à l’aide d’Azure Active Directory et bien plus encore. Pour plus d’informations, voir [Intégrer les Services de domaines Azure Active Directory à votre déploiement RDS](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-azure-adds).

>[!TIP]
>Pour plus d’un coup de œil sur les autres modifications intéressantes sera disponible pour les services Bureau à distance, consultez [Services Bureau à distance : Mises à jour et des innovations à venir](https://blogs.technet.microsoft.com/enterprisemobility/2017/09/20/first-look-at-updates-coming-to-remote-desktop-services/)

## <a name="networking"></a>Mise en réseau

**Le maillage de routage de Docker** est pris en charge. Le maillage de routage entrant fait partie du [mode Swarm](https://docs.docker.com/engine/swarm/), la solution d’orchestration intégrée de Docker pour les conteneurs. Pour plus d’informations, voir le billet de blog [Docker’s routing mesh available with Windows Server version 1709](https://blogs.technet.microsoft.com/virtualization/2017/09/26/dockers-ingress-routing-mesh-available-with-windows-server-version-1709/).

De **nouvelles fonctionnalités de Docker** sont disponibles. Pour plus d’informations, voir le billet de blog [Exciting new things for Docker with Windows Server 1709](https://blog.docker.com/2017/09/docker-windows-server-1709/).

**Windows Networking à parité avec Linux pour Kubernetes**: Windows est désormais identiques à celles de Linux en termes de mise en réseau. Les clients peuvent déployer des clusters Kubernetes sur des systèmes d’exploitation mixtes dans n’importe quel environnement (Azure, environnement local ou piles de cloud tiers) avec la même primitives et topologies de réseau que celles prises en charge sur Linux, sans avoir besoin de solutions de contournement ni d’extensions de commutateur.

**Pile de réseau Core**: Plusieurs fonctionnalités de la pile de réseau de base sont améliorées. Pour plus d’informations sur ces fonctionnalités, voir le billet de blog [Core Network Stack Features in the Creators Update for Windows 10](https://blogs.technet.microsoft.com/networking/2017/07/13/core-network-stack-features-in-the-creators-update-for-windows-10/).
- **TCP Fast Open (TFO)** : Prise en charge de TFO a été ajoutée pour optimiser le processus de négociation 3 voies TCP. TFO établit un cookie TFO sécurisé dans la première connexion à l’aide d’une connexion en 3 temps standard.  Les connexions suivantes au même serveur utilisent le cookie TFO au lieu d’une connexion en 3 temps pour se connecter avec un temps de réponse complet nul.
- **CUBIQUE**: Expérimental Windows implémentation native cubique, un algorithme de contrôle de congestion TCP est disponible. Les commandes suivantes activent ou désactivent CUBIC, respectivement.

    ```
    netsh int tcp set supplemental template=internet congestionprovider=cubic
    netsh int tcp set supplemental template=internet congestionprovider=compound
    ```

- **Réglage automatique de fenêtre de réception**: Logique de réglage automatique TCP calcule le paramètre « fenêtre de réception » d’une connexion TCP.  Les connexions rapides ou avec un long délai d’attente requièrent cet algorithme pour obtenir des caractéristiques de performances optimales.  Dans cette version, l’algorithme a été modifié de façon à utiliser une fonction intermédiaire pour converger sur la valeur de fenêtre de réception maximale pour une connexion donnée.
- **Statistiques TCP API**: Introduit une nouvelle API appelée SIO_TCP_INFO.  SIO_TCP_INFO permet aux développeurs de rechercher des informations détaillées sur des connexions TCP individuelles à l’aide d’une option de socket.
- **IPv6**: Il existe plusieurs améliorations de IPv6 dans cette version.
    - **RFC 6106** prennent en charge : 6106 RFC qui autorise une configuration DNS par le biais des annonces de routeur (RAs). Vous pouvez utiliser la commande suivante pour activer ou désactiver la prise en charge de RFC 6106 :

    ```
    netsh int ipv6 set interface <ifindex> rabaseddnsconfig=<enabled | disabled>
    ```

    - **Flux des étiquettes**: À partir de la mise à jour Creators, sortantes TCP et UDP des paquets via IPv6 sont ce champ la valeur est un hachage du 5-tuple (Src IP, IP de l’heure d’été, Src Port, Port de destination).  Cela rendra plus efficaces les centres de données IPv6 uniquement effectuant l’équilibrage de charge ou la classification de flux. Pour activer les étiquettes de flux :

    ```
    netsh int ipv6 set flowlabel=[disabled|enabled] (enabled by default)
    netsh int ipv6 set global flowlabel=<enabled | disabled>
    ```

    - **ISATAP et 6to4**: Comme une étape vers la désapprobation futures, la mise à jour Creators ont ces technologies désactivées par défaut.
- **Détection des passerelles inactives (DGD)** : L’algorithme DGD passe automatiquement les connexions sur vers une autre passerelle lors de la passerelle est inaccessible. Dans cette version, l’algorithme a été amélioré afin de revérifier régulièrement l’environnement réseau.
- [Test-NetConnection](https://technet.microsoft.com/itpro/powershell/windows/nettcpip/test-netconnection) est une applet de commande intégrée dans Windows PowerShell qui effectue différents diagnostics du réseau.  Dans cette version, nous avons amélioré l’applet de commande afin de fournir des informations détaillées à la fois sur la sélection de l’itinéraire et sur la sélection de l’adresse source.

**Mise en réseau SDN (Software Defined Networking)**

- Le **chiffrement de réseau virtuel** est une nouvelle fonctionnalité qui permet de chiffrer le trafic réseau virtuel entre les ordinateurs virtuels qui communiquent entre eux au sein de sous-réseaux marqués comme ayant le « Chiffrement activé ». Cette fonctionnalité utilise le protocole DTLS (Datagram Transport Layer Security) sur le sous-réseau virtuel pour chiffrer les paquets.  DTLS offre une protection contre les écoutes clandestines, l’altération et la falsification par toute personne ayant accès au réseau physique.
 
**Windows 10 VPN**

- **Tunnels d’infrastructure avant connexion**. Par défaut, le VPN Windows 10 ne crée pas automatiquement de tunnels d’infrastructure lorsque les utilisateurs ne sont pas connectés à leur ordinateur ou appareil. Vous pouvez configurer le VPN Windows 10 afin qu’il crée automatiquement des tunnels d’infrastructure avant connexion à l’aide de la fonctionnalité Tunnel de périphérique (avant connexion) dans le profil VPN.
- **Gestion d’ordinateurs et de périphériques distants**.  Vous pouvez gérer les clients VPN Windows 10 en configurant la fonctionnalité Tunnel de périphérique (avant connexion) dans le profil VPN. En outre, vous devez configurer la connexion VPN afin d’enregistrer de manière dynamique les adresses IP qui sont attribuées à l’interface VPN avec les services DNS internes.
- **Spécifiez des passerelles avant connexion**. Vous pouvez spécifier des passerelles avant connexion avec la fonctionnalité Tunnel de périphérique (avant connexion) dans le profil VPN, en les associant à des filtres de trafic pour contrôler les systèmes de gestion accessibles sur le réseau d’entreprise via le tunnel de périphérique.

