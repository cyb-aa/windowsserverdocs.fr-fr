---
title: Vue d’ensemble de Gestion des disques
description: La gestion des disques est un utilitaire système dans Windows qui vous permet d’effectuer des tâches de stockage avancées, comme l’initialisation d’un nouveau lecteur, l’extension des volumes, la réduction des partitions et la modification des lettres de lecteur.
ms.date: 06/07/2019
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: a3885ae6b09ad431fd1ea5e4c593e02c7bb274d9
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "66812545"
---
# <a name="overview-of-disk-management"></a>Vue d’ensemble de Gestion des disques

> **S’applique à :** Windows 10, Windows 8.1, Windows 7, Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La gestion des disques est un utilitaire système dans Windows qui vous permet d’effectuer des tâches de stockage avancées. Voici quelques-unes des actions que Gestion des disques vous permet de faire :

- Pour configurer un nouveau lecteur, consultez [Initialisation d’un nouveau lecteur](initialize-new-disks.md).
- Pour étendre un volume dans un espace qui n’est pas déjà parti d’un volume sur le même lecteur, consultez [Étendre un volume de base](extend-a-basic-volume.md).
- Pour réduire une partition, généralement pour pouvoir étendre une partition voisine, consultez [Réduire un volume de base](shrink-a-basic-volume.md).
- Pour modifier une lettre de lecteur ou attribuer une nouvelle lettre de lecteur, consultez [Modifier une lettre de lecteur](change-a-drive-letter.md).

![Gestion des disques montrant un disque classique avec trois partitions, une partition système de 499 Mo, un plus grand lecteur C pour Windows et une autre partition 499 Mo pour la récupération](media/disk-management.png)

> [!TIP]
>  Si vous obtenez une erreur ou si quelque chose ne fonctionne pas lorsque vous suivez ces procédures, jetez un œil à la [résolution des problèmes de gestion de disque](troubleshooting-disk-management.md). Si cela ne suffit pas, ne vous affolez pas ! Il existe une multitude d’informations sur le site de la [communauté Microsoft](https://answers.microsoft.com/en-us/windows). Essayez de rechercher la section [Fichiers, dossiers et stockage](https://answers.microsoft.com/en-us/windows/forum/windows_10-files?sort=lastreplydate&dir=desc&tab=All&status=all&mod=&modAge=&advFil=&postedAfter=&postedBefore=&threadType=all&isFilterExpanded=true&tm=1514405359639) et si vous avez toujours besoin d’aide, publiez une question ici et Microsoft ou d’autres membres de la communauté essayeront de vous aider. Si vous avez des commentaires sur la façon d’améliorer ces rubriques, nous aimerions les connaître ! Répondez simplement l’invite *Est-ce que cette page est utile ?* et laissez des commentaires ici ou dans la conversation des commentaires publics en bas de cette rubrique.

Voici certaines tâches courantes que vous pouvez vouloir faire mais qui utilisent d’autres outils dans Windows :

- Pour libérer de l’espace disque, consultez [Libérer de l’espace disque dans Windows 10](https://support.microsoft.com/help/12425/windows-10-free-up-drive-space).
- Pour défragmenter vos lecteurs, consultez [Défragmenter votre PC Windows 10](https://support.microsoft.com/help/4026701/windows-defragment-your-windows-10-pc).
- Pour prendre plusieurs disques durs et les rassembler, comme un volume RAID, consultez [Espaces de stockage](https://support.microsoft.com/help/12438/windows-10-storage-spaces).

## <a name="about-those-extra-recovery-partitions"></a>À propos de ces partitions de récupération supplémentaires

Si vous êtes curieux (nous avons lu vos commentaires !), Windows inclut généralement trois partitions sur votre lecteur principal (généralement le lecteur C:\) :

![Disque 0 montrant trois partitions, une partition système EFI, la partition Windows et une partition de récupération](media/windows-partitions.png)

- **Partition système EFI** : utilisée par les PC modernes pour démarrer votre PC et votre système d’exploitation.
- **Lecteur du système d’exploitation Windows (C:)** : emplacement où Windows est installé, généralement l’emplacement où vous placez le reste de vos applications et de vos fichiers.
- **Partition de récupération** : emplacement des outils spéciaux pour vous aider à récupérer Windows au cas où il aurait des difficultés à démarrer ou en cas d’autres problèmes graves.

Bien que la gestion des disques puisse afficher la partition système EFI et la partition de récupération comme ayant 100 % d’espace libre, ce n’est pas la vérité. Ces partitions sont généralement assez pleines, avec des fichiers très importants dont votre PC a besoin pour fonctionner correctement. Il est préférable de ne pas y toucher, afin qu’ils vous aident à démarrer votre PC et à résoudre les problèmes.

## <a name="see-also"></a>Voir également

- [Gérer les disques](manage-disks.md)
- [Gérer les volumes de base](manage-basic-volumes.md)
- [Résolution des problèmes de la Gestion des disques](troubleshooting-disk-management.md)
- [Options de récupération dans Windows 10](https://support.microsoft.com/help/12415/windows-10-recovery-options)
- [Rechercher des fichiers perdus après la mise à jour vers Windows 10](https://support.microsoft.com/help/12386/windows-10-find-lost-files-after-update)
- [Sauvegarder et restaurer vos fichiers](https://support.microsoft.com/help/17143/windows-10-back-up-your-files)
- [Créer un lecteur de récupération](https://support.microsoft.com/help/4026852/windows-create-a-recovery-drive)
- [Créer un point de restauration du système](https://support.microsoft.com/help/4027538/windows-create-a-system-restore-point)
- [Trouver ma clé de récupération BitLocker](https://support.microsoft.com/help/4026181/windows-find-my-bitlocker-recovery-key)
