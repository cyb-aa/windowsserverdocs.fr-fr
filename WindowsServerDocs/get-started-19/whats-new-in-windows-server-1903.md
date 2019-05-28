---
title: Nouveautés dans Windows Server, version 1903
description: Cette rubrique décrit certaines des nouvelles fonctionnalités dans Windows Server, version 1903, qui est une version de canal semi-annuel.
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.date: 05/21/2019
ms.openlocfilehash: 2aa84df1ca162cb6e2dfa580b4b0744ff08cc95a
ms.sourcegitcommit: c8cc0b25ba336a2aafaabc92b19fe8faa56be32b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65983439"
---
# <a name="whats-new-in-windows-server-version-1903"></a>Quelles sont les nouveautés dans Windows Server, version 1903

>S’applique à : Windows Server (canal semi-annuel)

Cette rubrique décrit certaines des nouvelles fonctionnalités dans Windows Server, version 1903, qui est une version de canal semi-annuel. Ces fonctionnalités incluent des améliorations pour l’exécution et la gestion des conteneurs, outils pour travailler dans les installations Server Core et la possibilité de migrer le stockage à partir d’appareils de Linux.

Pour rechercher à la place les nouveautés dans la dernière version de canal de maintenance à long terme (LTSC) de Windows Server, consultez Voir [What ' s New in Windows Server 2019](../get-started-19/whats-new-19.md). Consultez également [quelles sont les nouveautés dans Windows 10, le contenu de professionnels de l’informatique de la version 1903](https://docs.microsoft.com/windows/whats-new/whats-new-windows-10-version-1903).

La configuration système requise pour cette version est les mêmes que pour Windows Server 2019 — consultez [requise](../get-started-19/sys-reqs-19.md) pour plus d’informations. Pour voir ce qui a été supprimé récemment, consultez [fonctionnalités supprimées ou planifié pour le remplacement à compter de Windows Server, version 1903](../get-started-19/removed-features-1903.md)

> [!NOTE]
> Les conteneurs Windows doivent utiliser la même version de Windows en tant que le serveur hôte, ou un *antérieures* version. Par exemple, un serveur hôte exécutant la version finale de Windows Server, version 1903 (build 18342) permettre exécuter des conteneurs Windows Server avec une version identique ou antérieure et numéro de build (même si le conteneur utilise une version d’Insider Preview de Windows). Pour plus d’informations, consultez [compatibilité des versions de conteneur Windows](https://docs.microsoft.com/virtualization/windowscontainers/deploy-containers/version-compatibility).

## <a name="enhanced-support-for-non-microsoft-container-services"></a>Prise en charge améliorée pour les services de conteneur non Microsoft

Nous avons amélioré les fonctionnalités de la plateforme pour prendre en charge des services de conteneur Azure et les services de conteneur non Microsoft.

- Nous avons intégré containerd de l’élément de rapport personnalisé avec hôte de calcul Service HCS () pour prendre en charge les pods de conteneurs de Windows et Linux sur Windows (LCOW) sur Azure.
- Nous avons collaboré avec la Communauté Kubernetes pour activer la prise en charge des conteneurs Windows. Avec la version de Kubernetes v1.14, prise en charge de nœud Windows Server a officiellement été diplômé de bêta stable. Pour plus d’informations, consultez [conteneurs Windows désormais pris en charge dans Kubernetes](https://cloudblogs.microsoft.com/opensource/2019/03/25/windows-server-containers-now-supported-kubernetes/).
- Tricolore Tigera pour Windows est désormais disponible en tant que partie d’un abonnement Tigera Essentials et offres interopérable sur stratégie non superposition mise en réseau et de réseau mixte des environnements Linux/Windows.
- Nous avons fourni des améliorations d’évolutivité amélioration de la superposition mise en réseau de la prise en charge des conteneurs Windows, y compris l’intégration avec Kubernetes via la dernière version de v1.14 Flannel et Kubernetes. Pour plus d’informations, consultez [Introduction à la prise en charge de Windows dans Kubernetes](https://kubernetes.io/docs/setup/windows/).

## <a name="directx-hardware-acceleration-in-containers"></a>Accélération matérielle DirectX dans des conteneurs

Nous autorisons prise en charge de l’accélération matérielle pour DirectX APIs dans Windows conteneurs tp prise en charge des scénarios tels que de l’inférence de Machine Learning (ML) à l’aide de matériel graphique (GPU) de traitement graphique local. Pour plus d’informations, consultez [accélération GPU apportant vers les conteneurs Windows](https://techcommunity.microsoft.com/t5/Containers/Bringing-GPU-acceleration-to-Windows-containers/ba-p/393939).

## <a name="updated-container-identity-and-group-managed-service-account-documentation"></a>Identité de conteneur mise à jour et de groupe gérés documentation de compte de service

Nous avons ajouté d’autres exemples et informations de compatibilité à le [groupe comptes de Service administrés](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/manage-serviceaccounts) documentation et effectué le [module PowerShell de spécification d’informations d’identification](https://www.powershellgallery.com/packages/CredentialSpec) disponible dans PowerShell Gallery. Pour plus d’informations, consultez le [quelles sont les nouveautés pour l’identité du conteneur](https://techcommunity.microsoft.com/t5/Containers/What-s-new-for-container-identity/ba-p/389151) billet de blog.

## <a name="add-task-scheduler-and-hyper-v-manager-to-server-core-installations"></a>Ajouter le Planificateur de tâches et le Gestionnaire Hyper-V pour les installations Server Core

Comme vous le savez peut-être, nous recommandons d’utiliser l’option d’installation Server Core lors de l’utilisation de Windows Server, canal semi-annuel en production. Toutefois, Server Core par défaut omet un nombre d’outils de gestion utiles. Vous pouvez ajouter les plus nombreux outils couramment utilisés en installant la fonctionnalité de compatibilité des applications, mais il y a toujours eu de certains outils manquants.

Par conséquent, en fonction des commentaires des clients, nous avons ajouté deux outils plus à la fonctionnalité de compatibilité des applications dans cette version : Le Planificateur de tâches (taskschd.msc) et le Gestionnaire Hyper-V (virtmgmt.msc).

Pour plus d’informations, consultez [fonctionnalité de compatibilité d’application Server Core](../get-started-19/install-fod-19.md).

## <a name="storage-migration-service-now-migrates-local-accounts-clusters-and-linux-servers"></a>Service de Migration de stockage migre désormais des comptes locaux, des clusters et des serveurs Linux

Service de Migration de stockage facilite la migration des serveurs vers une version plus récente de Windows Server. Il fournit un outil graphique qui inventorie les données sur les serveurs, puis transfère les données et la configuration sur de nouveaux serveurs, tout cela sans applications ou utilisateurs d’avoir à modifier quoi que ce soit.

Lorsque vous utilisez cette version de Windows Server pour orchestrer des migrations, nous avons ajouté les possibilités suivantes :

- Migrer des utilisateurs et groupes locaux vers le nouveau serveur
- Migrer le stockage à partir de clusters de basculement
- Migration du stockage d’un serveur Linux qui utilise Samba
- Synchroniser plus facilement les partages migrés dans Azure à l’aide d’Azure File Sync
- Migrer vers les nouveaux réseaux virtuels, tels que Azure

Pour plus d’informations sur le Service de Migration de stockage, consultez [vue d’ensemble du Service de Migration de stockage](../storage/storage-migration-service/overview.md).

## <a name="system-insights-disk-anomaly-detection"></a>Détection d’anomalie disque système Insights

[Système Insights](../manage/system-insights/overview.md) est une fonctionnalité d’analytique prédictive qui analyse les données de système de Windows Server localement et de mieux appréhender le fonctionnement du serveur. Il est fourni avec un nombre de fonctionnalités intégrées, mais nous avons ajouté la possibilité d’installer des fonctionnalités supplémentaires via Windows Admin Center, en commençant par la détection d’anomalie de disque.

Détection d’anomalie de disque est une nouvelle fonctionnalité qui met en surbrillance lorsque les disques sont comportent *différemment* que d’habitude. Si n’est pas différent nécessairement une mauvaise chose, voir ces instants anormales peut être utile lors du dépannage de problèmes sur vos systèmes.

Cette fonctionnalité est également disponible pour les serveurs exécutant Windows Server 2019.

## <a name="windows-admin-center-enhancements"></a>Améliorations de Windows Admin Center

Une nouvelle version de Windows Admin Center est out, ajout de nouvelles fonctionnalités à Windows Server. Pour plus d’informations sur les fonctionnalités les plus récentes, consultez [Windows Admin Center](../manage/windows-admin-center/understand/windows-admin-center.md).

## <a name="security-baseline-for-windows-10-and-windows-server"></a>Sécurité de base pour Windows 10 et Windows Server

La version brouillon de le [paramètres de ligne de base de configuration de sécurité](https://blogs.technet.microsoft.com/secguide/2019/04/24/security-baseline-draft-for-windows-10-v1903-and-windows-server-v1903/) pour Windows 10 version 1903 et pour Windows Server version 1903 est disponible.

## <a name="setupdiag"></a>SetupDiag
[SetupDiag](https://docs.microsoft.com/windows/deployment/upgrade/setupdiag) version 1.4.1 est disponible.

SetupDiag est un outil de ligne de commande qui peut aider à diagnostiquer la raison de l’échec d’une mise à jour de Windows. SetupDiag fonctionne en recherchant les fichiers journaux d’installation de Windows. Lors de la recherche des fichiers journaux, SetupDiag utilise un ensemble de règles pour la correspondance des problèmes connus. Dans la version actuelle de SetupDiag 53 règles contenues dans le fichier rules.xml, qui est extrait lorsque SetupDiag est exécuté. Le fichier rules.xml est mis à jour lorsque de nouvelles versions de SetupDiag sont disponibles.

## <a name="update-rollback-improvements"></a>Améliorations de la restauration de mise à jour

Serveurs peuvent désormais automatiquement récupérer à partir d’échecs de démarrage en supprimant les mises à jour si l’échec de démarrage a été introduite après l’installation de mises à jour de pilote ou qualité récentes. Quand un appareil ne peut pas démarrer correctement après l’installation récente de la qualité des mises à jour de pilote, Windows désinstalle désormais automatiquement les mises à jour pour obtenir le périphérique de sauvegarde et fonctionne normalement.

Cette fonctionnalité nécessite le serveur pour être à l’aide de l’option d’installation Server Core avec un [environnement de récupération Windows](https://docs.microsoft.com/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference) partition.

## <a name="microsoft-defender-advanced-threat-protection-atp-improvements"></a>Améliorations de Microsoft Defender Advanced Threat Protection (ATP)

Windows Server inclut Microsoft Defender Thread-Protection avancée (pour plus d’informations, consultez [Antivirus Windows Defender sur Windows Server](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016)). Cette version inclut les améliorations suivantes :

- [La réduction de la surface d’attaque](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/overview-attack-surface-reduction) – les administrateurs informatiques peuvent configurer des appareils avec protection web avancées qui leur permet de définir et de refus des listes pour les URL spécifique et les adresses IP.
- [Protection de génération suivante](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10) – contrôles ont été étendues à la protection contre les ransomware, une mauvaise utilisation des informations d’identification et attaques qui sont transmises via le stockage amovible.
    - Fonctionnalités de mise en œuvre l’intégrité – activer, exécution à distance d’attestation.
    - Vérification de falsification fonctionnalités – utilise la sécurité basée sur la virtualisation pour isoler les fonctionnalités de sécurité ATP critiques en dehors du système d’exploitation et les pirates.
- Technologies de protection de la nouvelle génération de Microsoft Defender ATP :
    - **Advanced apprentissage**: Amélioré avec l’apprentissage automatique avancée et modèles d’intelligence artificielle qui lui permettent de protéger contre les attaques de sommet (Apex) à l’aide de logiciels malveillants, les outils et techniques d’exploiter une vulnérabilité innovantes.
    - **Protection de l’attaque d’urgence**: Fournit une protection d’urgence apparition d’un programme qui met automatiquement à jour les appareils de nouvelles informations quand une nouvelle attaque a été détectée.
    - **Certifié ISO 27001 conformité**: Garantit que le service cloud a analysé les menaces, failles et impacts, et que les contrôles de sécurité et de gestion des risques sont en place.
    - **Prise en charge de la géolocalisation**: Prend en charge de géolocalisation et la souveraineté des exemples de données, ainsi que des stratégies de rétention configurable.