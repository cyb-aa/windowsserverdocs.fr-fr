---
title: Nouveautés de Windows Server, version 1903
description: Cette rubrique décrit quelques-unes des nouvelles fonctionnalités de Windows Server, version 1903, qui est une publication du canal semi-annuel.
ms.prod: windows-server
ms.technology: server-general
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.date: 05/21/2019
ms.openlocfilehash: b8ef24aa62df0ea538a6ee34da17714ac9b4107f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71391898"
---
# <a name="whats-new-in-windows-server-version-1903"></a>Nouveautés de Windows Server, version 1903

>S’applique à : Windows Server (canal semi-annuel)

Cette rubrique décrit quelques-unes des nouvelles fonctionnalités de Windows Server, version 1903, qui est une publication du canal semi-annuel. Ces fonctionnalités incluent des améliorations de l’exécution et de la gestion des conteneurs, outils destinés à travailler dans des installations Server Core, et la possibilité de migrer le stockage à partir d’appareils Linux.

Pour rechercher plutôt les nouveautés de la dernière version du canal de maintenance à long terme (LTSC) de Windows Server, consultez [Nouveautés de Windows Server 2019](../get-started-19/whats-new-19.md). Consultez également [Nouveautés de Windows 10 version 1903 concernant les contenus destinés aux professionnels de l’informatique](https://docs.microsoft.com/windows/whats-new/whats-new-windows-10-version-1903).

La configuration requise pour cette version est la même que pour Windows Server 2019. Pour plus d’informations, consultez [Configuration système requise](../get-started-19/sys-reqs-19.md). Pour voir ce qui a été supprimé récemment, consultez [Fonctionnalités supprimées ou dont le remplacement est prévu à compter de Windows Server, version 1903](../get-started-19/removed-features-1903.md).

> [!NOTE]
> Les conteneurs Windows doivent utiliser la même version de Windows que le serveur hôte, ou une version *antérieure*. Par exemple, un serveur hôte exécutant la version finale de Windows Server, version 1903 (build 18342), peut exécuter des conteneurs Windows Server de version identique ou antérieure et de numéro de build identique ou antérieur (même si le conteneur utilise une version Insider Preview de Windows). Pour plus d’informations, consultez [Compatibilité des versions des conteneurs Windows](https://docs.microsoft.com/virtualization/windowscontainers/deploy-containers/version-compatibility).

## <a name="enhanced-support-for-non-microsoft-container-services"></a>Amélioration de la prise en charge des services de conteneur non-Microsoft

Nous avons amélioré les fonctionnalités de la plateforme pour prendre en charge les services de conteneur Azure et les services de conteneur non-Microsoft.

- Nous avons intégré des conteneurs CRI au Service de calcul hôte (HCS) pour prendre en charge les pods de conteneurs Windows et de conteneurs Linux sur Windows (LCOW) sur Azure.
- Nous avons collaboré avec la Communauté Kubernetes pour activer la prise en charge des conteneurs Windows. Avec la version de Kubernetes v1.14, la prise en charge de nœuds Windows Server est officiellement passée de bêta à stable. Pour plus d’informations, consultez [Conteneurs Windows désormais pris en charge dans Kubernetes](https://cloudblogs.microsoft.com/opensource/2019/03/25/windows-server-containers-now-supported-kubernetes/).
- Tigera Calico pour Windows est maintenant en disponibilité générale dans le cadre de l’abonnement à Tigera Essentials. Il offre à la fois un réseau sans superposition et une stratégie réseau qui peuvent interagir dans des environnements Linux/Windows mixtes.
- Nous avons fourni des améliorations de la scalabilité qui optimisent la prise en charge du réseau de superposition pour les conteneurs Windows, notamment l’intégration à Kubernetes par le biais de la dernière version de Flannel et de Kubernetes v1.14. Pour plus d’informations, consultez [Introduction à la prise en charge de Windows dans Kubernetes](https://kubernetes.io/docs/setup/windows/).

## <a name="directx-hardware-acceleration-in-containers"></a>Accélération matérielle DirectX dans les conteneurs

Nous activons la prise en charge de l’accélération matérielle des API DirectX dans les conteneurs Windows pour prendre en charge des scénarios comme l’inférence du Machine Learning (ML) à l’aide de matériel d’unité centrale graphique (GPU). Pour plus d’informations, consultez [Intégration de l’accélération GPU dans les conteneurs Windows](https://techcommunity.microsoft.com/t5/Containers/Bringing-GPU-acceleration-to-Windows-containers/ba-p/393939).

## <a name="updated-container-identity-and-group-managed-service-account-documentation"></a>Mise à jour de la documentation sur l’identité de conteneur et les comptes de service administrés de groupe

Nous avons ajouté d’autres exemples et informations de compatibilité à la documentation [Comptes de service administrés de groupe](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/manage-serviceaccounts) et rendu le [module PowerShell de spécification des informations d’identification](https://www.powershellgallery.com/packages/CredentialSpec) disponible dans PowerShell Gallery. Pour plus d’informations, consultez le billet de blog [Nouveautés de l’identité de conteneur](https://techcommunity.microsoft.com/t5/Containers/What-s-new-for-container-identity/ba-p/389151).

## <a name="add-task-scheduler-and-hyper-v-manager-to-server-core-installations"></a>Ajout du Planificateur de tâches et du Gestionnaire Hyper-V aux installations Server Core

Comme vous le savez peut-être, nous recommandons d’utiliser l’option d’installation Server Core quand vous utilisez le canal semi-annuel de Windows Server en production. Toutefois, Server Core omet par défaut un certain nombre d’outils de gestion utiles. Vous pouvez ajouter un grand nombre des outils les plus couramment utilisés en installant la fonctionnalité de compatibilité des applications, mais certains outils sont toujours manquants.

Par conséquent, sur la base des commentaires des clients, nous avons ajouté deux outils supplémentaires à la fonctionnalité de compatibilité des applications dans cette version : le Planificateur de tâches (taskschd.msc) et le Gestionnaire Hyper-V (virtmgmt.msc).

Pour plus d’informations, consultez [Fonctionnalité de compatibilité des applications Server Core](../get-started-19/install-fod-19.md).

## <a name="storage-migration-service-now-migrates-local-accounts-clusters-and-linux-servers"></a>À présent, le Service de migration de stockage migre des comptes locaux, des clusters et des serveurs Linux

Le Service de migration du stockage facilite la migration des serveurs vers une version plus récente de Windows Server. Il fournit un outil graphique qui inventorie les données sur les serveurs, puis transfère les données et la configuration vers de nouveaux serveurs, le tout sans que les applications ou les utilisateurs aient à changer quoi que ce soit.

Nous avons ajouté les options suivantes à utiliser quand vous vous servez de cette version de Windows Server pour orchestrer des migrations :

- Migrer des groupes et utilisateurs locaux vers le nouveau serveur
- Migrer le stockage à partir de clusters de basculement
- Migrer le stockage à partir d’un serveur Linux qui utilise Samba
- Synchroniser plus facilement des partages migrés dans Azure à l’aide d’Azure File Sync
- Migrer vers de nouveaux réseaux comme Azure

Pour plus d’informations sur le Service de migration de stockage, consultez [Vue d’ensemble du Service de migration de stockage](../storage/storage-migration-service/overview.md).

## <a name="system-insights-disk-anomaly-detection"></a>Détection des anomalies de disque d’Insights système

[Insights système](../manage/system-insights/overview.md) est une fonctionnalité d’analyse prédictive qui analyse localement les données système de Windows Server et fournit des insights sur le fonctionnement du serveur. Elle est fournie avec un certain nombre de fonctionnalités intégrées, mais nous avons ajouté la possibilité d’installer des fonctionnalités supplémentaires par le biais de Windows Admin Center, en commençant par la détection des anomalies de disque.

La détection des anomalies de disque est une nouvelle fonctionnalité qui signale quand les disques se comportent de manière *inhabituelle*. Si ce qui est inhabituel n’est pas nécessairement une mauvaise chose, il peut être utile de voir ces situations anormales lors de la résolution de problèmes sur vos systèmes.

Cette fonctionnalité est également disponible pour les serveurs exécutant Windows Server 2019.

## <a name="windows-admin-center-enhancements"></a>Améliorations apportées à Windows Admin Center

Une nouvelle version de Windows Admin Center est sortie, ce qui ajoute de nouvelles fonctionnalités à Windows Server. Pour plus d’informations sur les dernières fonctionnalités, consultez [Windows Admin Center](../manage/windows-admin-center/understand/windows-admin-center.md).

## <a name="security-baseline-for-windows-10-and-windows-server"></a>Base de référence de sécurité de Windows 10 et Windows Server

La version à l’état de brouillon des [paramètres de base de référence de configuration de la sécurité](https://blogs.technet.microsoft.com/secguide/2019/04/24/security-baseline-draft-for-windows-10-v1903-and-windows-server-v1903/) pour Windows 10 version 1903 et Windows Server version 1903 est disponible.

## <a name="setupdiag"></a>SetupDiag
[SetupDiag](https://docs.microsoft.com/windows/deployment/upgrade/setupdiag) version 1.4.1 est disponible.

SetupDiag est un outil en ligne de commande qui permet de diagnostiquer les raisons pour lesquelles une mise à jour Windows a échoué. SetupDiag fonctionne en faisant des recherches dans les fichiers journaux d’installation de Windows. Lorsqu’il recherche dans les fichiers journaux, SetupDiag utilise un ensemble de règles pour répondre à des problèmes connus. Dans la version actuelle de SetupDiag, il y a 53 règles contenues dans le fichier rules.xml, lequel est extrait quand SetupDiag est exécuté. Le fichier rules.xml est mis à jour quand de nouvelles versions de SetupDiag deviennent disponibles.

## <a name="update-rollback-improvements"></a>Améliorations de la restauration des mises à jour

Les serveurs peuvent désormais reprendre automatiquement leur activité après des échecs du démarrage en supprimant les mises à jour si cet échec s’est manifesté après l’installation de mises à jour qualité ou de pilote récentes. Quand un appareil ne peut pas démarrer correctement après l’installation récente de mises à jour qualité ou de pilote, Windows désinstalle désormais automatiquement ces mises à jour pour rétablir l’appareil dans un état opérationnel normal.

Cette fonctionnalité nécessite que le serveur utilise l’option d’installation Server Core avec une partition d’[environnement de récupération Windows](https://docs.microsoft.com/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference).

## <a name="microsoft-defender-advanced-threat-protection-atp-improvements"></a>Améliorations de Microsoft Defender Advanced Threat Protection (ATP)

Windows Server inclut Microsoft Defender Advanced Thread Protection (pour plus d’informations, consultez [Antivirus Windows Defender sur Windows Server](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016)). Cette version présente les améliorations suivantes :

- [Réduction de la surface d’attaque](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/overview-attack-surface-reduction)Les administrateurs informatiques peuvent configurer des appareils avec une protection web avancée qui leur permet de définir et d’exclure des listes pour des URL et des adresses IP spécifiques.
- [Protection génération suivante](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10)Des contrôles ont été étendus à la protection contre les ransomware, une mauvaise utilisation des informations d’identification et les attaques qui sont transmises par le biais d’un stockage amovible.
    - Fonctionnalités de mise en œuvre de l’intégrité : activez l’attestation de runtime à distance.
    - Fonctionnalités anti-falsification : utilise la sécurité basée sur la virtualisation pour isoler les fonctionnalités de sécurité ATP critiques du système d’exploitation et des attaquants.
- Technologies de protection nouvelle génération de Microsoft Defender ATP :
    - **Machine Learning avancé** : amélioration avec le Machine Learning avancé et les modèles d’intelligence artificielle (IA) qui offrent une protection contre les attaquants apex utilisant des programmes malveillants, des outils et des techniques d’attaque de vulnérabilité innovants.
    - **Protection d’urgence contre les attaques** : fournit une protection d’urgence contre les attaques qui met automatiquement à jour les appareils avec de nouvelles informations quand une nouvelle attaque a été détectée.
    - **Conformité à la norme ISO 27001 certifiée** : Garantit que le service cloud a effectué une analyse des menaces, des vulnérabilités et des impacts, et qu’une gestion des risques et des contrôles de sécurité sont en place.
    - **Prise en charge de la géolocalisation** : Prend en charge la géolocalisation et la souveraineté des exemples de données, ainsi que les stratégies de conservation configurables.