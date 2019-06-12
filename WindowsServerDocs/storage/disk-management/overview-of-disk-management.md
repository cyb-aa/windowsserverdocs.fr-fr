---
title: Vue d’ensemble de Gestion des disques
description: Gestion des disques est un utilitaire système dans Windows qui vous permet d’effectuer des tâches de stockage avancées, telles que l’initialisation d’un nouveau lecteur, l’extension des volumes, réduction des partitions et modification des lettres de lecteur.
ms.date: 06/07/2019
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: a3885ae6b09ad431fd1ea5e4c593e02c7bb274d9
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812545"
---
# <a name="overview-of-disk-management"></a>Vue d’ensemble de Gestion des disques

> **S’applique à :** Windows 10, Windows 8.1, Windows 7, Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Gestion des disques est un utilitaire système dans Windows qui vous permet d’effectuer des tâches de stockage avancées. Voici quelques-unes des choses que Gestion des disques est valable pour :

- Pour configurer un nouveau lecteur, consultez [l’initialisation d’un nouveau lecteur](initialize-new-disks.md).
- Pour étendre un volume dans un espace qui n’est pas déjà partie d’un volume sur le même lecteur, consultez [étendre un volume de base](extend-a-basic-volume.md).
- Pour réduire une partition, généralement afin que vous pouvez étendre une partition voisine, consultez [réduire un volume de base](shrink-a-basic-volume.md).
- Pour modifier une lettre de lecteur ou attribuer une nouvelle lettre de lecteur, consultez [modifier une lettre de lecteur](change-a-drive-letter.md).

![Gestion des disques montrant un disque classique avec trois partitions - une partition de système de 499 Mo, un plus grand lecteur C pour Windows et une autre partition 499 Mo pour la récupération](media/disk-management.png)

> [!TIP]
>  Si vous obtenez une erreur ou quelque chose ne fonctionne pas lorsque vous suivez ces procédures, prenez un œil sur le [résolution des problèmes de gestion de disque](troubleshooting-disk-management.md) rubrique. Si cela ne suffit pas - ne vous affolez pas ! Il existe une multitude d’informations sur la [Communauté Microsoft](https://answers.microsoft.com/en-us/windows) site - essayez de rechercher le [fichiers, dossiers et stockage](https://answers.microsoft.com/en-us/windows/forum/windows_10-files?sort=lastreplydate&dir=desc&tab=All&status=all&mod=&modAge=&advFil=&postedAfter=&postedBefore=&threadType=all&isFilterExpanded=true&tm=1514405359639) section et si vous avez besoin d’aide, publiez une question et de Microsoft ou d’autres membres de la Communauté va tenter de vous aider à. Si vous avez des commentaires sur la façon d’améliorer ces rubriques, nous aimerions connaître votre opinion ! Répondez simplement le *sert cette page ?* invite et laisser des commentaires de cet emplacement ou dans le thread de commentaires publics en bas de cette rubrique.

Voici certaines tâches courantes que vous pouvez souhaiter faire mais qui utilisent d’autres outils dans Windows :

- Pour libérer l’espace disque, consultez [libérer de l’espace disque dans Windows 10](https://support.microsoft.com/help/12425/windows-10-free-up-drive-space).
- Pour défragmenter les lecteurs, consultez [défragmenter votre PC Windows 10](https://support.microsoft.com/help/4026701/windows-defragment-your-windows-10-pc).
- Pour prendre de plusieurs disques durs et leur ensemble, similaire à un volume RAID du pool, consultez [espaces de stockage](https://support.microsoft.com/help/12438/windows-10-storage-spaces).

## <a name="about-those-extra-recovery-partitions"></a>À propos de ces partitions de récupération supplémentaires

Si vous êtes curieux (nous avons lire vos commentaires !), Windows inclut généralement trois partitions sur votre lecteur principal (généralement C:\ lecteur) :

![Disque 0 montrant trois partitions - une partition système EFI, la partition Windows et une partition de récupération](media/windows-partitions.png)

- **Partition système EFI** -cela est utilisé par les PC modernes pour démarrer votre PC et votre système d’exploitation.
- **Lecteur de système d’exploitation Windows (c)** -il s’agit où Windows est installé, et généralement où vous placez le reste de vos applications et les fichiers.
- **Partition de récupération** -il s’agit d’emplacement des outils spéciaux pour vous aider à récupérer Windows au cas où il a des difficultés à démarrer ou s’exécute sur les autres problèmes graves.

Bien que la gestion des disques peut afficher la partition système EFI et la partition de récupération en tant que 100 % gratuite, il se trouve. Ces partitions sont généralement assez complètes avec des fichiers très importants de que votre PC a besoin pour fonctionner correctement. Il est préférable de simplement de les laisser seul pour effectuer leurs tâches de démarrage de votre PC et vous aider à résoudre des problèmes.

## <a name="see-also"></a>Voir aussi

- [Gérer les disques](manage-disks.md)
- [Gérer les volumes de base](manage-basic-volumes.md)
- [Résolution des problèmes de la Gestion des disques](troubleshooting-disk-management.md)
- [Options de récupération dans Windows 10](https://support.microsoft.com/help/12415/windows-10-recovery-options)
- [Rechercher des fichiers perdus après la mise à jour vers Windows 10](https://support.microsoft.com/help/12386/windows-10-find-lost-files-after-update)
- [Sauvegarder et restaurer vos fichiers](https://support.microsoft.com/help/17143/windows-10-back-up-your-files)
- [Créer un lecteur de récupération](https://support.microsoft.com/help/4026852/windows-create-a-recovery-drive)
- [Créer un point de restauration du système](https://support.microsoft.com/help/4027538/windows-create-a-system-restore-point)
- [Trouver ma clé de récupération BitLocker](https://support.microsoft.com/help/4026181/windows-find-my-bitlocker-recovery-key)
