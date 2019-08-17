---
Title: Vue d’ensemble de Storage migration service
description: Le Service de migration du stockage facilite la migration des serveurs vers une version plus récente de Windows Server. Il fournit un outil graphique qui inventorie les données sur les serveurs, puis transfère les données et la configuration vers de nouveaux serveurs, le tout sans que les applications ou les utilisateurs aient à changer quoi que ce soit.
author: jasongerend
ms.author: jgerend
manager: elizapo
ms.date: 05/21/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: 8118b58268e88a173a6219631e109ed1c436fea0
ms.sourcegitcommit: 23a6e83b688119c9357262b6815c9402c2965472
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69560557"
---
# <a name="storage-migration-service-overview"></a>Vue d’ensemble de Storage migration service

>S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server (canal semi-annuel)

Le Service de migration du stockage facilite la migration des serveurs vers une version plus récente de Windows Server. Il fournit un outil graphique qui inventorie les données sur les serveurs, puis transfère les données et la configuration vers de nouveaux serveurs, le tout sans que les applications ou les utilisateurs aient à changer quoi que ce soit.

Cette rubrique explique pourquoi vous pouvez utiliser le service de migration de stockage, comment fonctionne le processus de migration et quelles sont les exigences pour les serveurs source et de destination.

## <a name="why-use-storage-migration-service"></a>Pourquoi utiliser le service de migration de stockage

Utilisez le service de migration de stockage, car vous disposez d’un serveur (ou de nombreux serveurs) que vous souhaitez migrer vers des machines virtuelles ou matérielles plus récentes. Storage migration service est conçu pour vous aider à effectuer les opérations suivantes:

- Inventorier plusieurs serveurs et leurs données
- Transférer rapidement des fichiers, des partages de fichiers et la configuration de sécurité à partir des serveurs sources
- Éventuellement, vous pouvez utiliser l’identité des serveurs sources (également appelée «découpage») afin que les utilisateurs et les applications n’aient pas à modifier quoi que ce soit pour accéder aux données existantes
- Gérer une ou plusieurs migrations à partir de l’interface utilisateur du centre d’administration Windows

![Diagramme montrant le service de migration de stockage migration des fichiers & Configuration des serveurs source vers les serveurs de destination, les machines virtuelles Azure ou les Azure File Sync.](media/overview/storage-migration-service-diagram.png)

**Figure 1 : Sources et destinations du service de migration du stockage**

## <a name="how-the-migration-process-works"></a>Fonctionnement du processus de migration

La migration est un processus en trois étapes:

1. Les **serveurs d’inventaire** pour collecter des informations sur leurs fichiers et leur configuration (voir figure 2).
2. **Transférer (copier) les données** des serveurs source vers les serveurs de destination.
3. Passer **aux nouveaux serveurs** (facultatif).<br>Les serveurs de destination supposent les anciennes identités des serveurs sources, de sorte que les applications et les utilisateurs n’ont pas à modifier quoi que ce soit. <br>Les serveurs sources entrent dans un état de maintenance où ils contiennent toujours les mêmes fichiers qu’ils ont toujours (nous ne supprimons jamais les fichiers des serveurs source), mais ils ne sont pas disponibles pour les utilisateurs et les applications. Vous pouvez ensuite désactiver les serveurs à votre convenance.

![Capture d’écran montrant un serveur prêt à être](media/migrate/inventory.png)
analysé**figure 2: Serveurs d’inventaire du service de migration du stockage**

## <a name="requirements"></a>Configuration requise

Pour utiliser le service de migration de stockage, vous avez besoin des éléments suivants:

- Un **serveur source** pour migrer les fichiers et les données à partir de
- Un **serveur de destination** exécutant windows server 2019 à migrer vers — windows server 2016 et windows server 2012 R2 fonctionne également, mais est environ 50% plus lent
- Un **serveur Orchestrator** exécutant Windows Server 2019 pour gérer la migration  <br>Si vous ne migrez que quelques serveurs et que l’un des serveurs exécute Windows Server 2019, vous pouvez l’utiliser en tant qu’orchestrateur. Si vous migrez d’autres serveurs, nous vous recommandons d’utiliser un serveur Orchestrator distinct.
- Un **PC ou un serveur exécutant le [Centre d’administration Windows](../../manage/windows-admin-center/understand/windows-admin-center.md)**  pour exécuter l’interface utilisateur du service de migration du stockage, sauf si vous préférez utiliser PowerShell pour gérer la migration. Le centre d’administration Windows et la version 2019 de Windows Server doivent tous deux avoir au moins la version 1809.

Nous recommandons vivement que les ordinateurs Orchestrator et de destination disposent d’au moins deux cœurs ou deux processeurs virtuels et au moins 2 Go de mémoire. Les opérations d’inventaire et de transfert sont beaucoup plus rapides avec davantage de processeurs et de mémoire.

### <a name="security-requirements"></a>Exigences de sécurité

- Un compte de migration qui est administrateur sur les ordinateurs source et l’ordinateur Orchestrator.
- Un compte de migration qui est administrateur sur les ordinateurs de destination et l’ordinateur Orchestrator.
- La règle de pare-feu de partage de fichiers et d’imprimantes (SMB-in)doit être activée sur l’ordinateur Orchestrator.
- Les règles de pare-feu suivantes doivent être activées sur les ordinateurs source et de destination (même si vous les avez peut-être déjà activés):
  - Partage de fichiers et d’imprimantes (SMB-Entrée)
  - Service Netlogon (NP-in)
  - Windows Management Instrumentation (DCOM-in)
  - Windows Management Instrumentation (WMI-In)
  
  > [!TIP]
  > L’installation du service de proxy Storage migration service sur un ordinateur Windows Server 2019 ouvre automatiquement les ports de pare-feu nécessaires sur cet ordinateur.
- Si les ordinateurs appartiennent à un domaine Active Directory Domain Services, ils doivent tous appartenir à la même forêt. Le serveur de destination doit également se trouver dans le même domaine que le serveur source si vous souhaitez transférer le nom de domaine de la source vers la destination lors du découpage. Le basculement fonctionne techniquement sur plusieurs domaines, mais le nom de domaine complet de la destination sera différent de la source...

### <a name="requirements-for-source-servers"></a>Configuration requise pour les serveurs sources

Le serveur source doit exécuter l’un des systèmes d’exploitation suivants:

- Windows Server, canal semi-annuel
- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2
- Windows Server 2008
- Windows Server 2003 R2
- Windows Server 2003
- Windows Small Business Server 2003 R2
- Windows Small Business Server 2008
- Windows Small Business Server 2011
- Windows Server 2012 Essentials
- Windows Server 2012 R2 Essentials
- WindowsServer2016 Essentials
- Windows Server 2019 Essentials

Remarque : Windows Small Business Server et Windows Server Essentials sont des contrôleurs de domaine. Le service de migration du stockage ne peut pas encore couper les contrôleurs de domaine, mais il peut inventorier et transférer des fichiers à partir de ces derniers.   

Si l’orchestrateur exécute Windows Server, version 1903 ou ultérieure, vous pouvez migrer les types de sources supplémentaires suivants:

- Clusters de basculement
- Serveurs Linux qui utilisent samba. Nous avons testé les éléments suivants:
    - RedHat Enterprise Linux 7,6, CentOS 7, Debian 8, Ubuntu 16,04 et 12.04.5, SUSE Linux Enterprise Server (SLES) 11 SP4
    - Samba 4. x, 3.6. x

### <a name="requirements-for-destination-servers"></a>Configuration requise pour les serveurs de destination

Le serveur de destination doit exécuter l’un des systèmes d’exploitation suivants:

- Windows Server, canal semi-annuel
- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2

> [!TIP]
> Les serveurs de destination qui exécutent Windows Server 2019 ou Windows Server, le canal semi-annuel version 1809 ou ultérieure ont un double des performances de transfert des versions antérieures de Windows Server. Cette amélioration des performances est due à l’inclusion d’un service de proxy de service de migration de stockage intégré, qui ouvre également les ports de pare-feu nécessaires s’ils ne sont pas déjà ouverts.

## <a name="whats-new-in-storage-migration-service"></a>Nouveautés du service de migration de stockage

Windows Server, version 1903 ajoute les nouvelles fonctionnalités suivantes, lorsqu’elles sont exécutées sur le serveur Orchestrator:

- Migrer des groupes et utilisateurs locaux vers le nouveau serveur
- Migrer le stockage à partir de clusters de basculement
- Migrer le stockage à partir d’un serveur Linux qui utilise Samba
- Synchroniser plus facilement des partages migrés dans Azure à l’aide d’Azure File Sync
- Migrer vers de nouveaux réseaux comme Azure

## <a name="see-also"></a>Voir aussi

- [Migrer un serveur de fichiers à l’aide du service de migration de stockage](migrate-data.md)
- [Forum aux questions sur Storage migration services (FAQ)](faq.md)
- [Problèmes connus du service de migration du stockage](known-issues.md)
