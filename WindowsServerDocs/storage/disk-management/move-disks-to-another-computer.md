---
title: Déplacer des disques vers un autre ordinateur
description: Cet article décrit comment déplacer des disques vers un autre ordinateur
ms.date: 10/12/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 6db73f963766e480f8ec478657354a51bcb1f456
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "71385820"
---
# <a name="move-disks-to-another-computer"></a>Déplacer des disques vers un autre ordinateur

> **S’applique à :** Windows 10, Windows 8.1, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette section décrit les étapes à suivre pour déplacer des disques vers un autre ordinateur et les considérations associées à cette opération. Il peut être utile d’imprimer cette procédure ou de noter les étapes avant d’essayer de déplacer les disques d’un ordinateur à un autre.

> [!NOTE]
> Vous devez au moins être membre du groupe **Opérateurs de sauvegarde** ou **Administrateurs** pour effectuer ces étapes.

## <a name="verify-volume-health"></a>Vérifier l’intégrité du volume

Utilisez l’outil Gestion des disques pour vous assurer que l’état des volumes sur les disques est **Sain**. Si l’état n’est pas **Sain**, réparez les volumes avant de déplacer les disques.

Pour vérifier l’état d’un volume, à partir du menu **Affichage**, vérifiez la colonne **État** de la vue **Liste des volumes**, ou sous les informations relatives à la **taille du volume** et au système de fichiers dans la **Représentation graphique**.

## <a name="uninstall-the-disks"></a>Désinstaller les disques

Désinstallez les disques que vous voulez déplacer à l’aide du Gestionnaire de périphériques.

**Pour désinstaller les disques**

1.  Ouvrez le Gestionnaire de périphériques dans Gestion de l’ordinateur.

2.  Dans la liste des périphériques, double-cliquez sur **Lecteurs de disque**.

3.  Cliquez avec le bouton droit sur les disques que vous souhaitez désinstaller, puis cliquez sur **Désinstaller**.

4.  Dans la boîte de dialogue **Confirmation de la suppression du périphérique**, cliquez sur **OK**.

## <a name="remove-dynamic-disks"></a>Supprimer des disques dynamiques

1. Si les disques que vous voulez déplacer sont des disques dynamiques, dans Gestion des disques, cliquez avec le bouton droit sur les disques à déplacer, puis cliquez sur **Supprimer le disque**.

2. Après avoir supprimé les disques dynamiques, ou si vous déplacez des disques de base, vous pouvez maintenant les déconnecter physiquement. S’il s’agit de disques externes, débranchez-les de l’ordinateur. Dans le cas de disques internes, éteignez l’ordinateur, puis supprimez-les physiquement.

## <a name="install-disks-in-the-new-computer"></a>Installer les disques sur le nouvel ordinateur

1. S’il s’agit de disques externes, branchez-les sur l’ordinateur. Dans le cas de disques internes, assurez-vous que l’ordinateur est éteint, puis installez les disques physiquement sur celui-ci.

2. Démarrez l’ordinateur contenant les disques que vous avez déplacés et suivez les instructions de la boîte de dialogue Nouveau matériel détecté.

## <a name="detect-new-disks"></a>Détecter les nouveaux disques

1. Sur le nouvel ordinateur, ouvrez Gestion de l’ordinateur. 
2. Cliquez sur **Action**, puis sur **Analyser les disques de nouveau**.
3. Cliquez avec le bouton droit sur n’importe quel disque marqué **Étranger**. 
4. Cliquez sur **Importer des disques étrangers**, puis suivez les instructions affichées à l’écran.

## <a name="additional-considerations"></a>Considérations supplémentaires

-   Lors du déplacement vers un autre ordinateur, les volumes de base reçoivent la lettre de lecteur disponible suivante sur l’ordinateur. 
-   Les volumes dynamiques conservent la lettre de lecteur qui leur avait été affectée sur l’ordinateur précédent. Si un volume dynamique n’avait pas de lettre de lecteur sur l’ordinateur précédent, aucune lettre de lecteur ne lui est affectée lorsqu’il est déplacé vers un autre ordinateur. Si la lettre de lecteur est déjà utilisée sur l’ordinateur sur lequel le volume est déplacé, il reçoit la lettre de lecteur disponible suivante.

-   Si un administrateur a utilisé la commande **mountvol/n** ou **diskpart automount** pour empêcher l’ajout de nouveaux volumes au système, les volumes déplacés à partir d’un autre ordinateur ne peuvent pas être montés ni affectés d’une lettre de lecteur. Pour utiliser le volume, vous devez le monter manuellement et lui affecter une lettre de lecteur à l’aide de l’outil Gestion des disques ou des commandes **DiskPart** et **mountvol**.

-   Si vous déplacez des volumes fractionnés, agrégés par bande ou en miroir, ou encore des volumes RAID-5, il est fortement recommandé de déplacer tous les disques contenant le volume ensemble. Dans le cas contraire, les volumes se trouvant sur les disques ne peuvent pas être mis en ligne et ne seront pas accessibles sauf pour la suppression.

-   Vous pouvez déplacer plusieurs disques se trouvant sur des ordinateurs différents vers un même ordinateur. Pour ce faire, installez les disques, ouvrez Gestion des disques, cliquez avec le bouton droit sur un des nouveaux disques, puis cliquez sur **Importer des disques étrangers**. Lors de l’importation de plusieurs disques à partir d’ordinateurs différents, importez toujours tous les disques d’un ordinateur à la fois. Par exemple, si vous voulez déplacer les disques de deux ordinateurs, importez tous les disques du premier ordinateur, puis ceux du deuxième ordinateur.

-   L’outil Gestion des disques décrit la condition des volumes sur les disques avant leur importation. Lisez attentivement ces informations. En cas de problèmes, ces informations vous indiquent ce qu’il adviendra à chaque volume des disques une fois que les disques ont été importés.

-   Si vous déplacez un disque de table de partition GUID (GPT) contenant le système d’exploitation Windows vers un ordinateur x86 ou x64, vous pouvez accéder aux données, mais vous ne pouvez pas démarrer à partir de ce système d’exploitation.