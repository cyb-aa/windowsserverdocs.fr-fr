---
title: Nouveautés de Windows Server, version 1709
description: Présentation des nouvelles fonctionnalités de calcul, d’identité, de gestion, d’automatisation, de mise en réseau, de sécurité et de stockage.
ms.prod: windows-server
ms.technology: server-general
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
ms.localizationpriority: medium
ms.date: 06/03/2019
ms.openlocfilehash: 07479bc5bd2fdf661db8a30e3a9f20c7cce0513e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80825992"
---
# <a name="whats-new-in-windows-server-version-1709"></a>Nouveautés de Windows Server, version 1709

>S'applique à : Windows Server (canal semi-annuel)

<img src=../media/landing-icons/new.png style='float:left; padding:.5em;' alt=Icon showing a newspaper>&nbsp;Pour découvrir les dernières fonctionnalités de Windows, consultez [Nouveautés de Windows Server](whats-new-in-windows-server.md). Le contenu de cette section décrit les nouveautés et les modifications de Windows Server, version 1709. Les nouvelles fonctionnalités et modifications répertoriées ici sont celles qui sont susceptibles d'avoir l'impact le plus important quand vous utilisez cette version. Voir également [Windows Server, version 1709](https://blogs.technet.microsoft.com/windowsserver/2017/08/24/sneak-peek-1-windows-server-version-1709/).

> [!IMPORTANT]
> Windows Server, version 1709 n’est plus pris en charge à compter du 9 avril 2019.


## <a name="new-cadence-of-releases"></a>Nouvelle cadence de publication

À compter de cette version, il existe deux façons de recevoir les mises à jour des fonctionnalités Windows Server :
- **Canal de maintenance à long terme (LTSC)**  : bénéficie de cinq ans de support standard et cinq ans de support étendu. Vous avez la possibilité dֹ’effectuer une mise à niveau vers la version suivante du canal de maintenance à long terme tous les deux à trois ans, comme c’est le cas depuis 20 ans.
- **Canal semi-annuel** : il s’agit d’un avantage de la Software Assurance, entièrement pris en charge en production. La différence réside dans le fait qu’il est pris en charge pendant 18 mois et qu’une nouvelle version est disponible tous les six mois.

Les canaux de distribution sont résumés dans le tableau suivant.

|   | Canal semi-annuel | Canal de maintenance à long terme |
| ------------- | ------------- | ------------ |
| Cadence de publication  | Deux fois par an (printemps et automne)  | Tous les deux à trois ans |
| Plan de support  | 18 mois de support standard en production  | Cinq ans de support standard + cinq ans de support étendu |
| Disponibilité  | Software Assurance ou Azure (hébergement dans le cloud)  | Tous les canaux |
| Convention d’affectation de noms  | Windows Server, version AAMM  | Windows Server AAAA |

Pour plus d’informations, consultez [Comparaison des canaux de maintenance](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview).

## <a name="application-containers-and-micro-services"></a>Conteneurs d’applications et micro-services

- L’image conteneur Server Core a été optimisée pour les scénarios « lift-and-shift » qui permettent de migrer des bases de code ou des applications existantes dans des conteneurs avec très peu de modifications, et qui plus est une taille inférieure de 60 %. 
- L’image conteneur Nano Server est réduite de près de 80 %.
    - Dans le canal semi-annuel Windows Server, l’image de système d’exploitation de base de conteneur Nano Server est réduite de 390 Mo à 80 Mo.
- Conteneurs Linux avec isolation Hyper-V 

Pour plus d’informations, consultez [Modifications apportées à Nano Server dans la prochaine version de Windows Server](https://docs.microsoft.com/windows-server/get-started/nano-in-semi-annual-channel) et [Windows Server, version 1709 pour les développeurs](https://blogs.technet.microsoft.com/windowsserver/2017/09/13/sneak-peek-3-windows-server-version-1709-for-developers/).

## <a name="modern-management"></a>Gestion moderne

Découvrez le [projet Honolulu](https://docs.microsoft.com/windows-server/manage/honolulu/honolulu) qui offre aux administrateurs informatiques une expérience intégrée, simplifiée et sécurisée facilitant la gestion des principaux scénarios de résolution des problèmes, de configuration et de maintenance.  Le projet Honolulu comprend des outils de nouvelle génération avec une interface simplifiée, intégrée, sécurisée et extensible.
Le projet Honolulu propose une expérience de gestion intuitive entièrement nouvelle pour la gestion des PC, serveurs Windows, clusters de basculement, mais également une infrastructure hyperconvergée basée sur les espaces de stockage direct, ce qui permet de réduire les coûts d’exploitation.

## <a name="compute"></a>Calcul

**Conteneur Nano et conteneur Server Core** : cette version a pour objectif principal de favoriser l’innovation d’application. Nano Server ou Nano en tant qu’hôte est déprécié et remplacé par le conteneur Nano, qui correspond à Nano exécuté en tant qu’image conteneur. 

Pour plus d’informations sur les conteneurs, consultez [Présentation de la mise en réseau de conteneurs](https://docs.microsoft.com/windows-server/networking/sdn/technologies/containers/container-networking-overview).

L’hôte **Server Core en tant que conteneur** (et infrastructure) offre une plus grande flexibilité, davantage de densité et de meilleures performances pour les applications existantes dans le cadre d’un processus de modernisation et personnalise les nouvelles applications déjà développées à l’aide du modèle de cloud.

La fonction **Ordre de démarrage des machines virtuelles** est également améliorée avec la reconnaissance des applications et du système d’exploitation. Elle apporte des déclencheurs améliorés pour déterminer si une machine virtuelle est considérée comme démarrée avant le démarrage de la suivante.

La **prise en charge de la mémoire de classe stockage pour les machines virtuelles** permet la création de volumes NTFS à accès direct sur des éléments DIMM non volatiles et leur exposition à des machines virtuelles Hyper-V. Ainsi, les machines virtuelles Hyper-V peuvent tirer parti des avantages des performances à faible latence des périphériques mémoire de classe stockage.

La **mémoire persistante virtualisée (vPMEM)** est activée en créant un fichier VHD (.vhdpmem) sur un volume à accès direct sur un hôte, en ajoutant un contrôleur vPMEM à une machine virtuelle, puis en ajoutant le périphérique créé (.vhdpmem) à une machine virtuelle. L’utilisation de fichiers vhdpmem sur des volumes à accès direct sur un hôte pour sauvegarder la mémoire persistante virtualisée (vPMEM) offre une plus grande flexibilité d’allocation et tire parti d’un modèle de gestion familier pour l’ajout de disques à des machines virtuelles.

**Stockage de conteneur : volumes de données persistants sur des volumes partagés de cluster (CSV)** . Dans Windows Server version 1709 ainsi que dans Windows Server 2016 avec les dernières mises à jour, nous avons ajouté la prise en charge de l’accès par des conteneurs à des volumes de données persistants situés sur des volumes partagés de cluster, notamment des volumes partagés de cluster dans les espaces de stockage direct. Ainsi, le conteneur d’applications dispose d’un accès persistant au volume, quel que soit le nœud de cluster sur lequel s’exécute l’instance de conteneur. Pour plus d’informations, consultez le billet de blog [Container Storage Support with Cluster Shared Volumes (CSV), Storage Spaces Direct (S2D), SMB Global Mapping](https://blogs.msdn.microsoft.com/clustering/2017/08/10/container-storage-support-with-cluster-shared-volumes-csv-storage-spaces-direct-s2d-smb-global-mapping/).

**Stockage de conteneur : volumes de données persistants avec le mappage global SMB**. Dans Windows Server version 1709, nous avons ajouté la prise en charge du mappage d’un partage de fichiers SMB à une lettre de lecteur à l’intérieur d’un conteneur. Ce mappage est appelé mappage global SMB. Ce lecteur mappé est ensuite accessible à tous les utilisateurs sur le serveur local afin que les E/S de conteneur sur le volume de données puissent transiter par le biais du lecteur monté pour accéder au partage de fichiers sous-jacent. Pour plus d’informations, consultez le billet de blog [Container Storage Support with Cluster Shared Volumes (CSV), Storage Spaces Direct (S2D), SMB Global Mapping](https://blogs.msdn.microsoft.com/clustering/2017/08/10/container-storage-support-with-cluster-shared-volumes-csv-storage-spaces-direct-s2d-smb-global-mapping/).

**Format de fichier de configuration de machine virtuelle (mis à jour)** . Un fichier supplémentaire (.vmgs) a été ajouté pour les machines virtuelles dont la version de configuration est 8.2 ou ultérieure. VMGS signifie « VM guest state » (état de machine virtuelle invitée). Il s’agit d’un nouveau fichier interne qui inclut l’état de l’appareil, lequel faisait précédemment partie du fichier d’état du runtime de la machine virtuelle.

## <a name="security-and-assurance"></a>Sécurité et assurance

Le **chiffrement réseau** vous permet de rapidement chiffrer des segments de réseau sur une infrastructure réseau à définition logicielle afin de répondre aux exigences en matière de sécurité et de conformité.

**Service Guardian hôte (SGH)** en tant que machine virtuelle dotée d’une protection maximale est activé. Avant cette version, il était recommandé de déployer un cluster physique à trois nœuds. Si ce déploiement garantit l’environnement SGH contre toute attaque d’un administrateur, il s’avère souvent très onéreux.

**Linux en tant que machine virtuelle dotée d’une protection maximale** est désormais pris en charge.

Pour plus d’informations, consultez [Présentation de l’infrastructure protégée et des machines virtuelles dotées d’une protection maximale](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms).

**Vulnérabilité SMBLoris** Un problème appelé « SMBLoris », capable de provoquer un déni de service, a été résolu.

## <a name="storage"></a>Stockage

**Réplica de stockage** : la protection par reprise d’activité après sinistre ajoutée par le réplica de stockage dans Windows Server 2016 a été étendue de manière à inclure les éléments suivants :
- **Test de basculement** : le montage du stockage de destination est maintenant possible grâce à la fonctionnalité de test de basculement. Vous pouvez monter une capture instantanée du stockage répliqué sur les nœuds de destination de manière temporaire à des fins de test ou de sauvegarde.  Pour plus d’informations, consultez le [Forum aux questions sur le réplica de stockage](https://aka.ms/srfaq). 
- **Prise en charge du projet Honolulu** : la gestion graphique de la réplication de serveur à serveur est désormais prise en charge dans le projet Honolulu. Cela supprime la nécessité d’utiliser PowerShell pour gérer une charge de travail de protection d’urgence courante.

**SMB** : 
- **SMB1 et suppression de l’authentification d’invité** : Windows Server, version 1709 n’installe plus le client et serveur SMB1 par défaut. En outre, la possibilité d’authentification en tant qu’invité dans SMB2 et versions ultérieures est désactivée par défaut. Pour plus d’informations, consultez [SMBv1 n’est pas installé par défaut dans Windows 10, version 1709 et Windows Server, version 1709](https://support.microsoft.com/help/4034314/smbv1-is-not-installed-by-default-in-windows-10-rs3-and-windows-server). 

- **Sécurité et compatibilité SMB2/SMB3** : des options supplémentaires ont été ajoutées pour la sécurité et la compatibilité des applications, notamment la possibilité de désactiver les verrouillages opportunistes (oplocks) dans SMB2+ pour les applications héritées, ainsi que l’obligation de signature ou de chiffrement par connexion pour un client. Pour plus d’informations, consultez l’aide du module SMBShare PowerShell.

**Déduplication des données** : 
- **La déduplication des données prend désormais en charge ReFS** : il n’est plus nécessaire de choisir entre les avantages d’un système de fichiers moderne avec ReFS et la déduplication des données. Vous pouvez désormais activer la déduplication des données partout où vous pouvez activer ReFS. Augmentez l’efficacité du stockage de plus de 95 % avec ReFS.
- **API DataPort pour des entrées/sorties optimisées pour les volumes dédupliqués** : les développeurs peuvent désormais tirer parti des connaissances de la déduplication des données en matière de stockage efficace des données pour déplacer des données entre les volumes, les serveurs et les clusters de manière optimale.

## <a name="remote-desktop-services-rds"></a>Services Bureau à distance

**Les Services Bureau à distance sont intégrés à Azure AD**, ce qui permet aux clients d’exploiter les stratégies d’accès conditionnel, l’authentification multifacteur, l’authentification intégrée avec d’autres applications SaaS à l’aide d’Azure Active Directory et bien plus encore. Pour plus d’informations, consultez [Intégrer Azure AD Domain Services à votre déploiement RDS](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-azure-adds).

>[!TIP]
>Pour avoir un aperçu des modifications prometteuses à venir pour les services Bureau à distance, consultez [Services Bureau à distance : mises à jour et innovations à venir](https://blogs.technet.microsoft.com/enterprisemobility/2017/09/20/first-look-at-updates-coming-to-remote-desktop-services/)

## <a name="networking"></a>Mise en réseau

**Le maillage de routage de Docker** est pris en charge. Le maillage de routage entrant fait partie du [mode Swarm](https://docs.docker.com/engine/swarm/), la solution d’orchestration intégrée de Docker pour les conteneurs. Pour plus d’informations, consultez le billet de blog [Docker’s routing mesh available with Windows Server version 1709](https://blogs.technet.microsoft.com/virtualization/2017/09/26/dockers-ingress-routing-mesh-available-with-windows-server-version-1709/).

De **nouvelles fonctionnalités de Docker** sont disponibles. Pour plus d’informations, consultez le billet de blog [Exciting new things for Docker with Windows Server 1709](https://blog.docker.com/2017/09/docker-windows-server-1709/).

**Mise en réseau Windows à parité avec Linux pour Kubernetes** : Windows est désormais à parité avec Linux en termes de mise en réseau. Les clients peuvent déployer des clusters Kubernetes sur des systèmes d’exploitation mixtes dans n’importe quel environnement (Azure, environnement local ou piles de cloud tiers) avec les mêmes primitives et topologies de réseau que celles prises en charge sur Linux, sans avoir besoin de solutions de contournement ni d’extensions de commutateur.

**Pile de réseau principal** : plusieurs fonctionnalités de la pile de réseau principal ont été améliorées. Pour plus d’informations sur ces fonctionnalités, consultez le billet de blog [Core Network Stack Features in the Creators Update for Windows 10](https://blogs.technet.microsoft.com/networking/2017/07/13/core-network-stack-features-in-the-creators-update-for-windows-10/).
- **TCP Fast Open (TFO)**  : la prise en charge de TFO a été ajoutée afin d’optimiser le processus de connexion en trois temps TCP. TFO établit un cookie TFO sécurisé dans la première connexion à l’aide d’une connexion en trois temps standard.  Les connexions suivantes au même serveur utilisent le cookie TFO au lieu d’une connexion en trois temps pour se connecter, avec un temps de réponse complet nul.
- **CUBIC** : une implémentation native Windows expérimentale de CUBIC, un algorithme de contrôle de congestion TCP, est disponible. Les commandes suivantes activent ou désactivent CUBIC, respectivement.

    ```
    netsh int tcp set supplemental template=internet congestionprovider=cubic
    netsh int tcp set supplemental template=internet congestionprovider=compound
    ```

- **Receive Window Autotuning** : la logique de réglage automatique TCP calcule le paramètre « fenêtre de réception » d’une connexion TCP.  Les connexions rapides ou avec un long délai d’attente nécessitent cet algorithme pour obtenir des caractéristiques de performances optimales.  Dans cette version, l’algorithme a été modifié de façon à utiliser une fonction intermédiaire afin de converger sur la valeur de fenêtre de réception maximale pour une connexion donnée.
- **API de statistiques TCP** : une nouvelle API appelée SIO_TCP_INFO a été introduite.  SIO_TCP_INFO permet aux développeurs de rechercher des informations détaillées sur des connexions TCP individuelles à l’aide d’une option de socket.
- **IPv6** : plusieurs améliorations ont été apportées à IPv6 dans cette version.
  - Prise en charge de **RFC 6106** : RFC 6106 autorise la configuration DNS par le biais d’annonces de routeur. Vous pouvez utiliser la commande suivante pour activer ou désactiver la prise en charge de RFC 6106 :

    ```
    netsh int ipv6 set interface <ifindex> rabaseddnsconfig=<enabled | disabled>
    ```

  - **Étiquettes de flux** : à compter de la mise à jour Creators Update, ce champ est défini sur un hachage de cinq tuples (Src IP, Dst IP, Src Port, Dst Port) pour les paquets TCP et UDP sortants sur IPv6.  Cela rendra plus efficaces les centres de données IPv6 uniquement effectuant l’équilibrage de charge ou la classification de flux. Pour activer les étiquettes de flux :

    ```
    netsh int ipv6 set flowlabel=[disabled|enabled] (enabled by default)
    netsh int ipv6 set global flowlabel=<enabled | disabled>
    ```

  - **ISATAP et 6to4** : afin de tenir compte de la dépréciation ultérieure, ces technologies seront désactivées par défaut dans la mise à jour Creators Update.
- **Détection de passerelle inactive** : l’algorithme de détection de passerelle inactive fait automatiquement transiter les connexions sur une autre passerelle quand la passerelle actuelle est inaccessible. Dans cette version, l’algorithme a été amélioré afin de revérifier régulièrement l’environnement réseau.
- [Test-NetConnection](https://technet.microsoft.com/itpro/powershell/windows/nettcpip/test-netconnection) est une applet de commande intégrée dans Windows PowerShell qui effectue différents diagnostics du réseau.  Dans cette version, nous avons amélioré l’applet de commande afin de fournir des informations détaillées à la fois sur la sélection de la route et sur la sélection de l’adresse source.

**Mise en réseau SDN (Software Defined Networking)**

- Le **chiffrement de réseau virtuel** est une nouvelle fonctionnalité qui permet de chiffrer le trafic réseau virtuel entre les machines virtuelles qui communiquent entre elles au sein de sous-réseaux marqués comme ayant le chiffrement activé. Cette fonctionnalité utilise le protocole DTLS (Datagram Transport Layer Security) sur le sous-réseau virtuel pour chiffrer les paquets.  DTLS offre une protection contre les écoutes clandestines, l’altération et la falsification par toute personne ayant accès au réseau physique.
 
**VPN Windows 10**

- **Tunnels d’infrastructure avant connexion**. Par défaut, le VPN Windows 10 ne crée pas automatiquement de tunnels d’infrastructure quand les utilisateurs ne sont pas connectés à leur ordinateur ou appareil. Vous pouvez configurer le VPN Windows 10 afin qu’il crée automatiquement des tunnels d’infrastructure avant connexion à l’aide de la fonctionnalité Tunnel de périphérique (avant connexion) dans le profil VPN.
- **Gestion d’ordinateurs et de périphériques distants**.  Vous pouvez gérer les clients VPN Windows 10 en configurant la fonctionnalité Tunnel de périphérique (avant connexion) dans le profil VPN. En outre, vous devez configurer la connexion VPN afin d’enregistrer de manière dynamique les adresses IP qui sont attribuées à l’interface VPN avec les services DNS internes.
- **Spécifiez des passerelles avant connexion**. Vous pouvez spécifier des passerelles avant connexion avec la fonctionnalité Tunnel de périphérique (avant connexion) dans le profil VPN, en les associant à des filtres de trafic pour contrôler les systèmes de gestion accessibles sur le réseau d’entreprise par le biais du tunnel de périphérique.

