---
title: Vue d’ensemble de Storage migration service
description: Storage migration service facilite la migration du stockage vers Windows Server ou vers Azure. Il fournit un outil graphique qui inventorit les données sur les serveurs Windows et Linux, puis transfère les données vers des serveurs plus récents ou vers des machines virtuelles Azure. Le service de migration de stockage offre également la possibilité de transférer l’identité d’un serveur vers le serveur de destination afin que les applications et les utilisateurs puissent accéder à leurs données sans modifier les liens ou les chemins d’accès.
author: jasongerend
ms.author: jgerend
manager: elizapo
ms.date: 01/17/2020
ms.topic: article
ms.prod: windows-server
ms.technology: storage
ms.openlocfilehash: 70ce4ebca35e071cf6e27fe429d3c4e6f67d342c
ms.sourcegitcommit: 8b801bd86e2ddf8255899b11f547daa920e5f651
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2020
ms.locfileid: "80110672"
---
# <a name="storage-migration-service-overview"></a>Vue d’ensemble de Storage migration service

>S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server (canal semi-annuel)

Storage migration service facilite la migration du stockage vers Windows Server ou vers Azure. Il fournit un outil graphique qui inventorit les données sur les serveurs Windows et Linux, puis transfère les données vers des serveurs plus récents ou vers des machines virtuelles Azure. Le service de migration de stockage offre également la possibilité de transférer l’identité d’un serveur vers le serveur de destination afin que les applications et les utilisateurs puissent accéder à leurs données sans modifier les liens ou les chemins d’accès.

Cette rubrique explique pourquoi vous pouvez utiliser le service de migration de stockage, comment fonctionne le processus de migration et quelles sont les exigences pour les serveurs source et de destination.

## <a name="why-use-storage-migration-service"></a>Pourquoi utiliser le service de migration de stockage

Utilisez le service de migration de stockage, car vous disposez d’un serveur (ou de nombreux serveurs) que vous souhaitez migrer vers des machines virtuelles ou matérielles plus récentes. Storage migration service est conçu pour vous aider à effectuer les opérations suivantes :

- Inventorier plusieurs serveurs et leurs données
- Transférer rapidement des fichiers, des partages de fichiers et la configuration de sécurité à partir des serveurs sources
- Éventuellement, vous pouvez utiliser l’identité des serveurs sources (également appelée « découpage ») afin que les utilisateurs et les applications n’aient pas à modifier quoi que ce soit pour accéder aux données existantes
- Gérer une ou plusieurs migrations à partir de l’interface utilisateur du centre d’administration Windows

![Diagramme montrant le service de migration de stockage migration des fichiers & Configuration des serveurs source vers les serveurs de destination, les machines virtuelles Azure ou les Azure File Sync.](media/overview/storage-migration-service-diagram.png)

**Figure 1 : sources et destinations du service de migration du stockage**

## <a name="how-the-migration-process-works"></a>Fonctionnement du processus de migration

La migration est un processus en trois étapes :

1. Les **serveurs d’inventaire** pour collecter des informations sur leurs fichiers et leur configuration (voir figure 2).
2. **Transférer (copier) les données** des serveurs source vers les serveurs de destination.
3. **Basculez vers les nouveaux serveurs** (facultatif).<br>Les serveurs de destination supposent les anciennes identités des serveurs sources, de sorte que les applications et les utilisateurs n’ont pas à modifier quoi que ce soit. <br>Les serveurs sources entrent dans un état de maintenance où ils contiennent toujours les mêmes fichiers qu’ils ont toujours (nous ne supprimons jamais les fichiers des serveurs source), mais ils ne sont pas disponibles pour les utilisateurs et les applications. Vous pouvez ensuite désactiver les serveurs à votre convenance.

Capture d’écran ![montrant un serveur prêt à être analysé](media/migrate/inventory.png)
**figure 2 : serveurs d’inventaire du service de migration du stockage**

Voici une vidéo qui montre comment utiliser Storage migration service pour prendre un serveur, tel qu’un serveur Windows Server 2008 R2 qui n’est plus pris en charge, et déplacer le stockage vers un serveur plus récent.

> [!VIDEO https://www.youtube.com/embed/h-Xc9j1w144]

## <a name="requirements"></a>Configuration requise

Pour utiliser le service de migration de stockage, vous avez besoin des éléments suivants :

- Un **serveur source** ou un **cluster de basculement** pour migrer les fichiers et les données à partir de
- Un **serveur de destination** exécutant Windows Server 2019 (cluster ou autonome) à migrer vers. Windows Server 2016 et Windows Server 2012 R2 fonctionnent aussi bien, mais sont environ 50% plus lents
- Un **serveur Orchestrator** exécutant Windows Server 2019 pour gérer la migration  <br>Si vous ne migrez que quelques serveurs et que l’un des serveurs exécute Windows Server 2019, vous pouvez l’utiliser en tant qu’orchestrateur. Si vous migrez d’autres serveurs, nous vous recommandons d’utiliser un serveur Orchestrator distinct.
- Un **PC ou un serveur exécutant le [Centre d’administration Windows](../../manage/windows-admin-center/understand/windows-admin-center.md)**  pour exécuter l’interface utilisateur du service de migration du stockage, sauf si vous préférez utiliser PowerShell pour gérer la migration. Le centre d’administration Windows et la version 2019 de Windows Server doivent tous deux avoir au moins la version 1809.

Nous recommandons vivement que les ordinateurs Orchestrator et de destination disposent d’au moins deux cœurs ou deux processeurs virtuels et au moins 2 Go de mémoire. Les opérations d’inventaire et de transfert sont beaucoup plus rapides avec davantage de processeurs et de mémoire.

### <a name="security-requirements-the-storage-migration-service-proxy-service-and-firewall-ports"></a>Exigences de sécurité, service de proxy de service de migration de stockage et ports de pare-feu

- Un compte de migration qui est administrateur sur les ordinateurs source et l’ordinateur Orchestrator.
- Un compte de migration qui est administrateur sur les ordinateurs de destination et l’ordinateur Orchestrator.
- La règle de pare-feu de partage de fichiers et d’imprimantes (SMB-in) doit *être activée sur*l’ordinateur Orchestrator.
- Les règles de pare-feu suivantes doivent être activées sur les ordinateurs source et de destination (même si vous les avez peut-être *déjà activés* ) :
  - Partage de fichiers et d’imprimantes (SMB-Entrée)
  - Service Netlogon (NP-in)
  - Windows Management Instrumentation (DCOM-In)
  - Windows Management Instrumentation (WMI-In)
  
  > [!TIP]
  > L’installation du service de proxy Storage migration service sur un ordinateur Windows Server 2019 ouvre automatiquement les ports de pare-feu nécessaires sur cet ordinateur. Pour ce faire, connectez-vous au serveur de destination dans le centre d’administration Windows, puis accédez à **Gestionnaire de serveur** (dans le centre d’administration windows) > **rôles et fonctionnalités**, sélectionnez **proxy de service de migration de stockage**, puis sélectionnez **installer**.


- Si les ordinateurs appartiennent à un domaine Active Directory Domain Services, ils doivent tous appartenir à la même forêt. Le serveur de destination doit également se trouver dans le même domaine que le serveur source si vous souhaitez transférer le nom de domaine de la source vers la destination lors du découpage. Le basculement fonctionne techniquement sur plusieurs domaines, mais le nom de domaine complet de la destination sera différent de la source...

### <a name="requirements-for-source-servers"></a>Configuration requise pour les serveurs sources

Le serveur source doit exécuter l’un des systèmes d’exploitation suivants :

- Windows Server, canal semi-annuel
- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2
- Windows Server 2008
- Windows Server 2003 R2
- Windows Server 2003
- Windows Small Business Server 2003 R2
- Windows Small Business Server 2008
- Windows Small Business Server 2011
- Windows Server 2012 Essentials
- Windows Server 2012 R2 Essentials
- WindowsServer2016 Essentials
- Windows Server 2019 Essentials
- Windows Storage Server 2008
- Windows Storage Server 2008 R2
- Windows Storage Server 2012
- Windows Storage Server 2012 R2
- Windows Storage Server 2016

Remarque : Windows Small Business Server et Windows Server Essentials sont des contrôleurs de domaine. Le service de migration du stockage ne peut pas encore couper les contrôleurs de domaine, mais il peut inventorier et transférer des fichiers à partir de ces derniers.   

Vous pouvez migrer les types de sources supplémentaires suivants si Orchestrator exécute Windows Server, version 1903 ou ultérieure, ou si Orchestrator exécute une version antérieure de Windows Server avec [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) installé :

- Clusters de basculement exécutant Windows Server 2012, Windows Server 2012 R2, Windows Server 2016, Windows Server 2019
- Serveurs Linux qui utilisent samba. Nous avons testé les éléments suivants :
    - CentOS 7
    - Debian GNU/Linux 8
    - RedHat Enterprise Linux 7,6
    - SUSE Linux Enterprise Server (SLES) 11 SP4
    - Ubuntu 16,04 LTS et 12.04.5 LTS
    - Samba 4,8, 4,7, 4,3, 4,2 et 3,6

### <a name="requirements-for-destination-servers"></a>Configuration requise pour les serveurs de destination

Le serveur de destination doit exécuter l’un des systèmes d’exploitation suivants :

- Windows Server, canal semi-annuel
- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2

> [!TIP]
> Les serveurs de destination exécutant Windows Server 2019 ou Windows Server, un canal semi-annuel ou une version ultérieure ont doublement les performances de transfert des versions antérieures de Windows Server. Cette amélioration des performances est due à l’inclusion d’un service de proxy de service de migration de stockage intégré, qui ouvre également les ports de pare-feu nécessaires s’ils ne sont pas déjà ouverts.

## <a name="azure-vm-migration"></a>Migration de machines virtuelles Azure

Le centre d’administration Windows version 1910 vous permet de déployer des machines virtuelles Azure. Cela intègre le déploiement de machines virtuelles dans le service de migration de stockage. Au lieu de créer des serveurs et des machines virtuelles dans le portail Azure manuellement avant de déployer votre charge de travail, et éventuellement des étapes et des configurations requises, le centre d’administration Windows peut déployer la machine virtuelle Azure, configurer son stockage, la joindre à votre domaine, installer des rôles et Configurez ensuite votre système distribué. 

## <a name="whats-new-in-storage-migration-service"></a>Nouveautés du service de migration de stockage

Les nouvelles fonctionnalités suivantes sont disponibles lors de l’exécution du serveur de migration de stockage Orchestrator sur Windows Server, version 1903 ou ultérieure, ou d’une version antérieure de Windows Server avec [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) installé :

- Migrer des groupes et utilisateurs locaux vers le nouveau serveur
- Migrer le stockage à partir des clusters de basculement, migrer vers les clusters de basculement et migrer entre les serveurs autonomes et les clusters de basculement
- Migrer le stockage à partir d’un serveur Linux qui utilise Samba
- Synchroniser plus facilement des partages migrés dans Azure à l’aide d’Azure File Sync
- Migrer vers de nouveaux réseaux comme Azure

## <a name="see-also"></a>Voir aussi

- [Migrer un serveur de fichiers à l’aide du service de migration de stockage](migrate-data.md)
- [Forum aux questions sur Storage migration services (FAQ)](faq.md)
- [Problèmes connus du service de migration du stockage](known-issues.md)
