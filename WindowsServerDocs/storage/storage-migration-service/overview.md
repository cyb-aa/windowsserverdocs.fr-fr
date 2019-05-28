---
Title: Vue d’ensemble du Service de Migration de stockage
description: Service de Migration de stockage facilite la migration des serveurs vers une version plus récente de Windows Server. Il fournit un outil graphique qui inventorie les données sur les serveurs, puis transfère les données et la configuration sur de nouveaux serveurs, tout cela sans applications ou utilisateurs d’avoir à modifier quoi que ce soit.
author: jasongerend
ms.author: jgerend
manager: elizapo
ms.date: 05/21/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: fd99058036a5b8041e4c65ca120c6a7e68b2df8d
ms.sourcegitcommit: c8cc0b25ba336a2aafaabc92b19fe8faa56be32b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65976647"
---
# <a name="storage-migration-service-overview"></a>Vue d’ensemble du Service de Migration de stockage

>S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server (canal semi-annuel)

Service de Migration de stockage facilite la migration des serveurs vers une version plus récente de Windows Server. Il fournit un outil graphique qui inventorie les données sur les serveurs, puis transfère les données et la configuration sur de nouveaux serveurs, tout cela sans applications ou utilisateurs d’avoir à modifier quoi que ce soit.

Cette rubrique explique pourquoi vous pouvez utiliser le Service de Migration de stockage, comment fonctionne le processus de migration, et quelles sont les exigences pour les serveurs source et de destination.

## <a name="why-use-storage-migration-service"></a>Pourquoi utiliser le Service de Migration de stockage

Utiliser le Service de Migration de stockage, car vous avez un serveur (ou un grand nombre de serveurs) que vous souhaitez migrer vers la plus récente de matériel ou de machines virtuelles. Service de Migration de stockage est conçu pour aider à en procédant comme suit :

- Plusieurs serveurs et leurs données d’inventaire
- Transférer rapidement des fichiers, partages de fichiers et la configuration de sécurité à partir des serveurs source
- Si vous le souhaitez contrôler l’identité des serveurs source (également appelé Couper sur) afin que les utilisateurs et des applications sans avoir à modifier quoi que ce soit pour accéder aux données existantes
- Gérer un ou plusieurs des migrations à partir de l’interface utilisateur de Windows Admin Center

![Diagramme montrant les fichiers de migration de Service de Migration de stockage & configuration à partir des serveurs sources à des serveurs de destination, les machines virtuelles Azure ou Azure File Sync.](media\overview\storage-migration-service-diagram.png)

**Figure 1 : Sources de Service de Migration de stockage et des destinations**

## <a name="how-the-migration-process-works"></a>Comment fonctionne le processus de migration

Migration est un processus en trois étapes :

1. **Inventaire des serveurs** pour recueillir des informations sur leurs fichiers et de la configuration (illustré dans la Figure 2).
2. **Transférer (copier) des données** entre les serveurs de la source et les serveurs de destination.
3. **Basculer vers les nouveaux serveurs** (facultatif).<br>Les serveurs de destination supposent la source d’identités anciens serveurs afin que les utilisateurs et les applications sans avoir à modifier quoi que ce soit. <br>Les serveurs source entrent dans un état de maintenance où ils ont toujours contiennent les mêmes fichiers qu’ils ont toujours (nous jamais supprimer des fichiers à partir des serveurs source), mais ne sont pas disponibles aux utilisateurs et aux applications. Vous pouvez ensuite retirer les serveurs à votre convenance.

![Capture d’écran montrant un serveur prêt à être analysés](media/migrate/inventory.png)
**Figure 2 : Service de Migration de stockage inventaire des serveurs**

## <a name="requirements"></a>Configuration requise

Pour utiliser le Service de Migration de stockage, vous devez les éléments suivants :

- Un **serveur source** pour migrer les fichiers et les données à partir de
- Un **serveur de destination** exécutant Windows Server 2019 pour migrer vers : Windows Server 2016 et Windows Server 2012 R2 fonctionner aussi bien, mais sont d’environ 50 % plus lent
- Un **serveur orchestrator** exécutant Windows Server 2019 pour gérer la migration  <br>Si vous effectuez une migration uniquement quelques serveurs et des serveurs exécutent Windows Server 2019, vous pouvez l’utiliser que comme l’orchestrateur. Si vous effectuez une migration des serveurs, nous recommandons d’utiliser un serveur distinct orchestrator.
- Un **PC ou un serveur exécutant [Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md)**  pour exécuter l’interface utilisateur du Service de Migration de stockage, sauf si vous préférez utiliser PowerShell pour gérer la migration. La version Windows Admin Center et Windows Server 2019 doit tous deux être au moins la version 1809.

Nous recommandons vivement qu’orchestrator et destination ordinateurs ont au moins deux cœurs ou deux processeurs virtuels et au moins 2 Go de mémoire. Opérations d’inventaire et de transfert sont beaucoup plus rapides avec plus de processeurs et de mémoire.

### <a name="security-requirements"></a>Exigences de sécurité

- Un compte de migration qui est administrateur sur les ordinateurs source et l’ordinateur orchestrator.
- Un compte de migration qui est administrateur sur les ordinateurs de destination et l’ordinateur orchestrator.
- L’ordinateur orchestrator doit avoir la règle de pare-feu (SMB-In) partage de fichiers et imprimantes est activée *entrant*.
- Les ordinateurs source et de destination doivent avoir les règles de pare-feu suivantes activés *entrant* (même si vous disposez peut-être déjà les activé) :
  - Partage de fichiers et d’imprimantes (SMB-Entrée)
  - Service accès réseau (NP-Entrée)
  - Windows Management Instrumentation (DCOM-In)
  - Windows Management Instrumentation (WMI-In)
  
  > [!TIP]
  > Installer le service de Proxy de Service de Migration de stockage sur un ordinateur Windows Server 2019 automatiquement ouvre les ports de pare-feu nécessaires sur cet ordinateur.
- Si les ordinateurs appartiennent à un domaine Active Directory Domain Services, ils doivent tous appartenir à la même forêt. Le serveur de destination doit également être dans le même domaine que le serveur source si vous souhaitez transférer le nom de domaine de la source vers la destination lors de la coupe sur. Le basculement techniquement fonctionne sur plusieurs domaines, mais le nom de domaine complet de la destination sera différent de la source...

### <a name="requirements-for-source-servers"></a>Configuration requise pour les serveurs sources

Le serveur source doit exécuter un des systèmes d’exploitation suivants :

- Windows Server, canal semi-annuel
- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2
- Windows Server 2008
- Windows Server 2003 R2
- Windows Server 2003

Si l’orchestrateur exécute Windows Server, version 1903 ou version ultérieure, vous pouvez migrer les types de sources supplémentaires suivants :

- Clusters de basculement
- Serveurs Linux qui utilisent Samba. Nous avons testé ce qui suit :
    - Red Hat Enterprise Linux 7.6, CentOS 7, 8 Debian, Ubuntu 16.04 et 12.04.5, SUSE Linux Enterprise Server (SLES) 11 SP4
    - Samba 4.x, 3.6.x

### <a name="requirements-for-destination-servers"></a>Configuration requise pour les serveurs de destination

Le serveur de destination doit exécuter un des systèmes d’exploitation suivants :

- Windows Server, canal semi-annuel
- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2

> [!TIP]
> Serveurs de destination exécutant Windows Server 2019 ou Windows Server, le canal semi-annuel 1809 ou version ultérieure ont double les performances de transfert des versions antérieures de Windows Server. Cette amélioration des performances est en raison de l’inclusion d’un service de proxy de Service de Migration de stockage intégrée, ce qui ouvre également le ports si elles ne sont pas déjà ouverts de pare-feu nécessaires.

## <a name="whats-new-in-storage-migration-service"></a>Quelles sont les nouveautés dans le Service de Migration de stockage

Windows Server, version 1903 ajoute les nouvelles fonctionnalités suivantes sur le serveur orchestrator :

- Migrer des utilisateurs et groupes locaux vers le nouveau serveur
- Migrer le stockage à partir de clusters de basculement
- Migration du stockage d’un serveur Linux qui utilise Samba
- Synchroniser plus facilement les partages migrés dans Azure à l’aide d’Azure File Sync
- Migrer vers les nouveaux réseaux virtuels, tels que Azure

## <a name="see-also"></a>Voir aussi

- [Migrer un serveur de fichiers à l’aide du Service de Migration de stockage](migrate-data.md)
- [Services de Migration de stockage Forum aux questions (FAQ)](faq.md)
- [Service de Migration de stockage problèmes connus](known-issues.md)
