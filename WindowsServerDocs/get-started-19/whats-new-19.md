---
title: Nouveautés de Windows Server 2019
description: Vue d’ensemble des nouvelles fonctionnalités de Windows Server 2019, notamment l’expérience utilisateur, le service de migration du stockage, les insights système, l’adaptateur réseau Azure, les améliorations apportées aux espaces de stockage direct et d’autres modifications.
ms.prod: windows-server
ms.technology: server-general
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: high
ms.date: 06/04/2019
ms.openlocfilehash: 47269fbfac6aea3fe46513ad67d2cfa2f0c9b78e
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80639918"
---
# <a name="whats-new-in-windows-server-2019"></a>Nouveautés de Windows Server 2019

> S'applique à : Windows Server 2019

Cette rubrique décrit certaines des nouvelles fonctionnalités de Windows Server 2019. Windows Server 2019 repose sur les bases solides de Windows Server 2016 et apporte de nombreuses innovations dans quatre thèmes clés : cloud hybride, sécurité, plateforme d’applications et infrastructure hyperconvergée (HCL).

Pour découvrir les nouveautés des versions de canal semi-annuel de Windows Server, consultez [Nouveautés de Windows Server](../get-started/whats-new-in-windows-server.md).

## <a name="general"></a>Général

### <a name="windows-admin-center"></a>Windows Admin Center

Windows Admin Center est une application basée sur navigateur, déployée localement pour la gestion des serveurs, des clusters, de l’infrastructure hyperconvergée et des PC Windows 10. Il est fourni sans coût supplémentaire au-delà de Windows et est prêt à être utilisé en production.

Vous pouvez installer Windows Admin Center sur Windows Server 2019 ainsi que sur Windows 10 et versions antérieures de Windows et Windows Server, et l’utiliser pour gérer des serveurs et des clusters exécutant Windows Server 2008 R2 et ultérieur.

Pour plus d’informations, consultez [Windows Admin Center](../manage/windows-admin-center/understand/windows-admin-center.md).

### <a name="desktop-experience"></a>Expérience utilisateur

Étant donné que Windows Server 2019 est une version de canal de maintenance à long terme (LTSC), il inclut l’<b>expérience utilisateur</b>. (Les versions de canal semi-annuel \(SAC\) n’incluent pas l’expérience utilisateur par défaut ; il s’agit strictement de versions d’image de conteneur Server Core et Nano Server.) Comme avec Windows Server 2016, pendant l’installation du système d’exploitation, vous pouvez choisir entre les installations Server Core ou Server avec Expérience utilisateur.

### <a name="system-insights"></a>Insights système

Insights système est une nouvelle fonctionnalité disponible dans Windows Server 2019 qui apporte des fonctionnalités analytiques prédictives locales en mode natif de Windows Server. Ces fonctionnalités prédictives, chacune soutenue par un modèle d’apprentissage machine, analysent localement les données système de Windows Server, telles que les compteurs de performances et les événements, vous offrent des renseignements sur le fonctionnement de vos serveurs et vous aident à réduire les dépenses d’exploitation associées à la gestion réactive des problèmes dans vos déploiements de Windows Server.

## <a name="hybrid-cloud"></a>Cloud hybride

### <a name="server-core-app-compatibility-feature-on-demand"></a>Fonctionnalité de compatibilité des applications Server Core à la demande (FOD)

La [fonctionnalité de compatibilité des applications Server Core à la demande (FOD)](https://docs.microsoft.com/windows-server/get-started-19/install-fod-19) améliore considérablement la compatibilité de l’application de l’option d’installation Windows Server Core en incluant un sous-ensemble des fichiers binaires et les composants de Windows Server avec Expérience utilisateur, sans ajouter l’environnement graphique de l’Expérience utilisateur de Windows Server elle-même.  Cela a pour but d’augmenter la fonctionnalité et la compatibilité de Server Core tout en le conservant aussi simple que possible.  

Cette fonctionnalité facultative à la demande est disponible sur un fichier ISO distinct et peut être ajoutée à des installations et des images Windows Server Core uniquement, à l’aide de DISM. 

## <a name="security"></a>Sécurité

### <a name="windows-defender-advanced-threat-protection-atp"></a>Protection avancée contre les menaces Windows Defender

Les capteurs profonds de plate-forme et les actions de réponse d’ATP exposent des attaques au niveau de la mémoire et du noyau et y répondent par la suppression des fichiers malveillants et l’arrêt des processus malveillants.

-   Pour plus d’informations sur Windows Defender ATP, consultez [Présentation des capacités de Windows Defender ATP](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/overview).

-   Pour plus d’informations sur l’intégration des serveurs, consultez [Intégrer des serveurs au service Windows Defender ATP](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/configure-server-endpoints-windows-defender-advanced-threat-protection).

**Windows Defender ATP Exploit Guard** est un nouvel ensemble de fonctionnalités de prévention des intrusions dans l’hôte. Les quatre composants de Windows Defender Exploit Guard sont conçus pour verrouiller l’appareil contre un grand nombre de vecteurs d’attaque et pour bloquer les comportements couramment utilisés dans les attaques par programmes malveillants, tout en vous permettant d’équilibrer les risques de sécurité et les exigences de productivité.

-   [Attack Surface Reduction (ASR)](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-exploit-guard/attack-surface-reduction-exploit-guard?ocid=cx-blog-mmpc) est un jeu de contrôles permettant aux entreprises d’empêcher les programmes malveillants d’accéder aux ordinateurs en bloquant les fichiers soupçonnés d’être malveillants (par exemple, des fichiers Office), les scripts, les mouvement latéraux, les comportements de rançongiciel et les menaces par e-mail.

-   [Protection réseau](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/network-protection) protège le point de terminaison contre les menaces web en bloquant n’importe quel processus sortant sur l’appareil vers des adresses d’hôtes/IP non approuvées à travers Windows Defender SmartScreen.

-   [L’accès contrôlé aux dossiers](https://cloudblogs.microsoft.com/microsoftsecure/2017/10/23/stopping-ransomware-where-it-counts-protecting-your-data-with-controlled-folder-access/?ocid=cx-blog-mmpc?source=mmpc) protège les données sensibles contre les rançongiciels en empêchant les processus non approuvés d’accéder à vos dossiers protégés.

-   [Exploit Protection](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-exploit-guard/exploit-protection-exploit-guard) est un ensemble de mesures d’atténuation des attaques de vulnérabilité (remplaçant EMET) qui peuvent être facilement configurées pour protéger votre système et vos applications.

[Contrôle d’application Windows Defender](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control) (également connu sous le nom de stratégie d’intégrité du code [CI]) a été publié dans Windows Server 2016.
Les commentaires des clients ont indiqué qu’il s’agissait d’un excellent concept, mais difficile à déployer.
Pour résoudre ce problème, nous avons créé des stratégies CI par défaut, qui autorisent tous les fichiers intégrés dans Windows, ainsi que les applications de Microsoft, telles que SQL Server, et bloquent les fichiers exécutables connus qui peuvent contourner CI. 

### <a name="security-with-software-defined-networking-sdn"></a>Sécurité avec SDN (Software Defined Networking)

[Sécurité avec SDN](https://docs.microsoft.com/windows-server/networking/sdn/security/sdn-security-top) offre de nombreuses fonctionnalités destinées à renforcer la confiance des clients exécutant des charges de travail, soit en local, soit comme fournisseurs de services dans le cloud. 

Ces améliorations de sécurité sont intégrées dans la plateforme SDN complète introduite dans Windows Server 2016.

Pour connaître la liste complète des nouveautés de SDN, consultez [Nouveautés SDN pour Windows Server 2019](https://docs.microsoft.com/windows-server/networking/sdn/sdn-whats-new).

### <a name="shielded-virtual-machines-improvements"></a>Améliorations apportées aux machines virtuelles dotées d’une protection maximale

- **Améliorations pour les filiales**

    Vous pouvez maintenant exécuter des machines virtuelles dotées d’une protection maximale sur des ordinateurs ayant une connectivité intermittente au Service Guardian hôte en tirant parti du nouveau [SGH de secours](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office#fallback-configuration) et des fonctionnalités du [mode hors connexion](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office#offline-mode). Le SGH de secours vous permet de configurer un deuxième ensemble d’URL qu’Hyper-V peut essayer s’il ne peut pas atteindre votre serveur SGH principal.

    Le mode hors connexion vous permet de continuer à démarrer vos machines virtuelles protégés même si le SGH n’est pas accessible, tant que la machine virtuelle a démarré avec succès une fois et que la configuration de sécurité de l’hôte n’a pas changé.

- **Améliorations de la résolution des problèmes**

    Nous avons également simplifié le [dépannage de vos machines virtuelles](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-shielded-vms) en autorisant la prise en charge du Mode de Session étendu VMConnect et de PowerShell Direct. Ces outils sont particulièrement utiles si vous avez perdu la connectivité réseau à votre machine virtuelle et que vous devez mettre à jour sa configuration pour restaurer l’accès. 

    Ces fonctionnalités n’ont pas besoin d’être configurées et deviennent automatiquement disponibles quand une machine virtuelle protégé est placée sur un ordinateur hôte Hyper-V exécutant Windows Server version 1803 ou ultérieur.

- **Prise en charge de Linux**

    Si vous exécutez des environnements à systèmes d’exploitation mixtes, Windows Server 2019 prend désormais en charge l’exécution d’Ubuntu, Red Hat Enterprise Linux et SUSE Linux Enterprise Server dans les machines virtuelles.

### <a name="http2-for-a-faster-and-safer-web"></a>HTTP/2 pour un Internet plus rapide et plus sûr

- Amélioration de la fusion de connexions pour offrir une expérience de navigation ininterrompue et correctement chiffrée.

- Mise à niveau de la négociation de suite de chiffrement côté serveur HTTP/2 pour l’atténuation automatique des échecs de connexion et la simplicité de déploiement.

- Modification de notre fournisseur de congestion TCP par défaut en faveur de Cubic pour vous offrir davantage de débit !

## <a name="storage"></a>Stockage

Voici quelques-unes des modifications apportées au stockage dans Windows Server 2019. Pour plus d’informations, consultez [Nouveautés du stockage](../storage/whats-new-in-storage.md).

### <a name="storage-migration-service"></a>Service de migration du stockage

Le Service de migration du stockage est une nouvelle technologie qui facilite la migration des serveurs vers une version plus récente de Windows Server. Il offre un outil graphique qui répertorie les données sur les serveurs, transfère les données et la configuration vers les nouveaux serveurs puis, de manière facultative, fait passer les identités des anciens serveurs sur les nouveaux serveurs afin que les applications et les utilisateurs n’aient rien à modifier. Pour plus d’informations, consultez [Service de migration du stockage](../storage/storage-migration-service/overview.md).

### <a name="storage-spaces-direct"></a>Espaces de stockage directs

Voici une liste des nouveautés introduites dans les espaces de stockage direct. Pour plus d’informations, consultez [Nouveautés des espaces de stockage direct](../storage/whats-new-in-storage.md#storage-spaces-direct). Consultez également [Azure Stack HCL](https://docs.microsoft.com/azure-stack/operator/azure-stack-hci-overview) pour plus d’informations sur l’acquisition de systèmes d’espaces de stockage direct validés.

- **Déduplication et compression des volumes ReFS**
- **Prise en charge native de la mémoire persistante**
- **Résilience imbriquée d’une infrastructure hyperconvergée à deux nœuds au niveau du bord**
- **Clusters de deux serveurs utilisant une clé USB comme témoin**
- **Prise en charge de Windows Admin Center**
- **Historique des performances**
- **Mettre à l’échelle jusqu’à 4 Po par cluster**
- **La parité accélérée par la mise en miroir est 2 fois plus rapide**
- **Détection d’anomalie de latence de lecteur**
- **Délimiter manuellement la répartition des volumes pour renforcer la tolérance de pannes**

### <a name="storage-replica"></a>Réplica de stockage

Voici les nouveautés du réplica de stockage. Pour plus d’informations, consultez [Nouveautés du réplica de stockage](../storage/whats-new-in-storage.md#storage-replica).

- Le réplica de stockage est désormais disponible dans Windows Server 2019 Standard Edition.
- Le test de basculement est une nouvelle fonctionnalité qui permet de monter le stockage de destination pour valider les données de réplication ou de sauvegarde. Pour plus d’informations, consultez le [Forum aux questions sur le réplica de stockage](../storage/storage-replica/storage-replica-frequently-asked-questions.md).
- Améliorations des performances du journal de réplica de stockage
- Prise en charge de Windows Admin Center

## <a name="failover-clustering"></a>Clustering de basculement

Voici une liste des nouveautés du clustering de basculement. Pour plus d’informations, consultez [Nouveautés du clustering de basculement](../failover-clustering/whats-new-in-failover-clustering.md).

- **Jeux de clusters**
- **Clusters adaptés à Azure**
- **Migration de cluster entre domaines**
- **Témoin USB**
- **Améliorations de l’infrastructure de cluster**
- **Mise à jour adaptée aux clusters qui prend en charge les espaces de stockage direct**
- **Améliorations du témoin de partage de fichiers**
- **Renforcement de cluster**
- **Le cluster de basculement n’utilise plus l’authentification NTLM**

## <a name="application-platform"></a>Plateforme d’applications

### <a name="linux-containers-on-windows"></a>Conteneurs Linux sur Windows

Il est désormais possible d’exécuter des conteneurs Windows et Linux sur le même hôte de conteneurs à l’aide du même démon docker. Ceci permet d’avoir un environnement d’hôte de conteneurs hétérogène tout en offrant davantage de flexibilité aux développeurs d’applications.

### <a name="built-in-support-for-kubernetes"></a>Prise en charge intégrée de Kubernetes

Windows Server 2019 poursuit les améliorations apportées aux capacités de calcul, de réseau et de stockage à partir des versions de canal semi-annuel nécessaires pour prendre en charge Kubernetes sur Windows. Plus de détails sont disponibles dans les prochaines versions de Kubernetes.

- Les [réseaux de conteneurs](https://docs.microsoft.com/windows-server/networking/sdn/technologies/containers/container-networking-overview) dans Windows Server 2019 améliorent considérablement la facilité d’utilisation de Kubernetes sur Windows en améliorant la résilience de la mise en réseau des plateformes et la prise en charge des plug-ins de mise en réseau de conteneurs.

- Les charges de travail déployées sur Kubernetes peuvent utiliser la sécurité du réseau pour protéger les services Windows et Linux à l’aide d’outils intégrés.

### <a name="container-improvements"></a>Améliorations apportées aux conteneurs
    
- **Identité intégrée améliorée**

    Nous avons simplifié l’authentification Windows intégrée dans les conteneurs et l’avons rendue plus fiable en résolvant plusieurs limitations des versions antérieures de Windows Server.

- **Meilleure compatibilité des applications**

    La mise en conteneur des applications basées sur Windows est plus facile que jamais : La compatibilité des applications pour l’image *windowsservercore* existante a été augmentée. Pour les applications avec des dépendances d’API supplémentaires, il existe désormais une image de base tierce : *windows*.

- **Taille réduite et performances supérieures**

    La taille du téléchargement des images de conteneur de base, la taille sur disque et le temps de démarrage ont été améliorés. Cela accélère le workflow des conteneurs.

- **Expérience de gestion à l’aide de Windows Admin Center \(préversion\)**

    Il est maintenant plus facile que jamais de voir quels conteneurs sont en cours d’exécution sur votre ordinateur et de gérer des conteneurs individuels avec une nouvelle extension pour Windows Admin Center. Recherchez l’extension « Conteneurs » dans le [flux public du Windows Admin Center](https://docs.microsoft.com/windows-server/manage/windows-admin-center/configure/using-extensions).

### <a name="encrypted-networks"></a>Réseaux chiffrés

[Réseaux chiffrés](https://docs.microsoft.com/windows-server/networking/sdn/sdn-whats-new) - Le chiffrement des réseaux virtuels permet de chiffrer le trafic de réseau virtuel entre des machines virtuelles qui communiquent entre eux au sein des sous-réseaux marqués comme **Chiffrement activé**. Il utilise aussi le protocole DTLS (Datagram Transport Layer Security) sur le sous-réseau virtuel pour chiffrer les paquets. DTLS protège contre les écoutes clandestines, l’altération et la falsification par toute personne ayant accès au réseau physique.

### <a name="network-performance-improvements-for-virtual-workloads"></a>Améliorations des performances réseau pour les charges de travail virtuelles

[Améliorations des performances réseau pour les charges de travail virtuelles](https://docs.microsoft.com/windows-server/networking/technologies/hpn/hpn-insider-preview) optimise le débit du réseau pour les machines virtuelles sans avoir à constamment ajuster ou surdimensionner votre hôte. Cela réduit les coûts d’exploitation et de maintenance tout en augmentant la densité disponible de vos ordinateurs hôtes. Ces nouvelles fonctionnalités sont les suivantes :

* Receive Segment Coalescing dans le commutateur virtuel

* Dynamic Virtual Machine Multi-Queue (d.VMMQ)

### <a name="low-extra-delay-background-transport"></a>Low Extra Delay Background Transport

Low Extra Delay Background Transport (LEDBAT) est un fournisseur de contrôle de congestion du réseau à latence optimisée, conçu pour allouer automatiquement de la bande passante aux utilisateurs et aux applications, tout en consommant toute la bande passante disponible quand le réseau n’est pas en cours d’utilisation.   
Cette technologie est destinée à déployer des mises à jour volumineuses et critiques dans un environnement informatique sans impact sur les services côté client ni sur la bande passante associée.

### <a name="windows-time-service"></a>Service de temps Windows

Le [Service de temps Windows](https://docs.microsoft.com/windows-server/networking/windows-time-service/insider-preview) comprend une véritable prise en charge de la de seconde intercalaire conforme à l’UTC, un nouveau protocole d’horaire appelé Precision Time Protocol et une traçabilité de bout en bout.


### <a name="high-performance-sdn-gateways"></a>Passerelles SDN hautes performances

[Les passerelles SDN hautes performances](https://docs.microsoft.com/windows-server/networking/sdn/gateway-performance) dans Windows Server 2019 améliorent considérablement les performances pour les connexions IPsec et GRE, fournissant un débit ultra hautes performances en utilisant beaucoup moins l’UC.
<br/>

### <a name="new-deployment-ui-and-windows-admin-center-extension-for-sdn"></a>Nouvelle interface utilisateur de déploiement et extension du Windows Admin Center pour SDN

Désormais, avec Windows Server 2019, il est facile de déployer et de gérer par le biais d’une nouvelle interface utilisateur de déploiement et l’extension Windows Admin Center qui permettra à tout le monde de tirer parti de la puissance du SDN. 

### <a name="persistent-memory-support-for-hyper-v-vms"></a>Prise en charge de la mémoire persistante pour les machines virtuelles Hyper-V

Pour tirer parti du débit élevé et de la faible latence de la mémoire persistante (également appelée mémoire de classe stockage) dans des machines virtuelles, celle-ci peut désormais être projetée directement dans des machines virtuelles. Cela peut aider à réduire considérablement la latence des transactions de base de données ou à réduire les temps de récupération de bases de données à faible latence en mémoire en cas d’échec.

