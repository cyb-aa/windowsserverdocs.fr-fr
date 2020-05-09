---
title: defrag
description: Rubrique de référence pour la commande Defrag, qui localise et consolide les fichiers fragmentés sur les volumes locaux pour améliorer les performances du système.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aaf1d1ac-996a-4282-9b4d-1e8245ff162c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bf3ca6febfa07c7780b959389ff57fe4f3a0018b
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993150"
---
# <a name="defrag"></a>defrag

> S’applique à : Windows 10, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Localise et consolide les fichiers fragmentés sur les volumes locaux pour améliorer les performances du système.

L’appartenance au groupe **administrateurs** local, ou équivalent, est la condition minimale requise pour exécuter cette commande.

## <a name="syntax"></a>Syntaxe

```
defrag <volumes> | /c | /e <volumes>    [/h] [/m [n]| [/u] [v]]
defrag <volumes> | /c | /e <volumes> /a [/h] [/m [n]| [/u] [v]]
defrag <volumes> | /c | /e <volumes> /x [/h] [/m [n]| [/u] [v]]
defrag <volume> [<parameters>]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `<volume>` | Spécifie la lettre de lecteur ou le chemin d’accès du point de montage du volume à défragmenter ou à analyser. |
| /a | Effectuer une analyse sur les volumes spécifiés. |
| /C | Effectuez l’opération sur tous les volumes. |
| /d | Effectuer une défragmentation traditionnelle (il s’agit de la valeur par défaut). En revanche, sur un volume hiérarchisé, la défragmentation traditionnelle est exécutée uniquement sur le niveau de capacité. |
| /e | Effectuez l’opération sur tous les volumes, à l’exception de ceux spécifiés. |
| /g | Optimisez les niveaux de stockage sur les volumes spécifiés. |
| /h | Exécutez l’opération en priorité normale (la valeur par défaut est Low). |
| /i [n] | L’optimisation du niveau s’exécuterait pendant au plus n secondes sur chaque volume. |
| /k | Effectuez la consolidation des sections sur les volumes spécifiés. |
| /l | Effectuer une réajustement sur les volumes spécifiés. |
| /m [n] | Exécutez l’opération sur chaque volume en parallèle en arrière-plan. Au plus n threads optimisent les niveaux de stockage en parallèle. |
| /o | Effectuez l’optimisation appropriée pour chaque type de média. |
| /t | Effectue le suivi d’une opération déjà en cours sur le volume spécifié. |
| /U | Imprimez la progression de l’opération à l’écran. |
| /v | Affichez la sortie détaillée contenant les statistiques de fragmentation. |
| /x | Effectuez une consolidation de l’espace libre sur les volumes spécifiés. |
| /? | Affiche ces informations d’aide. |

#### <a name="remarks"></a>Notes 

- Vous ne pouvez pas défragmenter des volumes ou des lecteurs de système de fichiers spécifiques, notamment :

  - Volumes verrouillés par le système de fichiers.

  - Volume le système de fichiers marqué comme modifié, indiquant une éventuelle altération.<br>Vous devez exécuter `chkdsk` avant de pouvoir défragmenter ce volume ou ce lecteur. Vous pouvez déterminer si un volume est modifié à l’aide `fsutil dirty` de la commande.

  - Lecteurs réseau.

  - CD-ROM.

  - Volumes de système de fichiers qui ne sont pas **NTFS**, **ReFS**, **FAT** ou **FAT32**.

- Vous ne pouvez pas planifier la défragmentation d’un disque SSD (Solid State Drive) ou d’un volume sur un disque dur virtuel (VHD) qui réside sur un disque SSD.

- Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs sur l'ordinateur local, ou l'autorité appropriée doit vous avoir été déléguée. Si l'ordinateur est joint à un domaine, les membres du groupe Admins du domaine peuvent être en mesure d'effectuer cette procédure. Pour des raisons de sécurité, envisagez d’utiliser **exécuter en tant que** pour effectuer cette procédure.

- Un volume doit disposer d’au moins 15% d’espace **libre pour être** défragmenté complètement et correctement défragmenté. **Defrag** utilise cet espace comme zone de tri pour les fragments de fichier. Si un volume dispose de moins de 15% d’espace libre, **Defrag** ne le défragmentera que partiellement. Pour augmenter l’espace libre sur un volume, supprimez les fichiers superflus ou déplacez-les sur un autre disque.

- Alors que **Defrag** analyse et défragmente un volume, il affiche un curseur clignotant. Lorsque **Defrag** a fini d’analyser et de défragmenter le volume, il affiche le rapport d’analyse, le rapport de défragmentation, ou les deux rapports, puis se ferme à l’invite de commandes.

- Par défaut, **Defrag** affiche un résumé des rapports d’analyse et de défragmentation si vous ne spécifiez pas les paramètres **/a** ou **/v** .

- Vous pouvez envoyer les rapports vers un fichier texte en tapant **>** <em>filename. txt</em>, où *filename. txt* est un nom de fichier que vous spécifiez. Par exemple : `defrag volume /v > FileName.txt`

- Pour interrompre le processus de défragmentation, sur la ligne de commande, appuyez sur **Ctrl + C**.

- L’exécution de la commande **Defrag** et du défragmenteur de disque s’exclut mutuellement. Si vous utilisez le Défragmenteur de disque pour défragmenter un volume et que vous exécutez la commande **Defrag** sur une ligne de commande, la commande **Defrag** échoue. À l’inverse, si vous exécutez la commande **Defrag** et que vous ouvrez le Défragmenteur de disque, les options de défragmentation du défragmenteur de disque ne sont pas disponibles.

## <a name="examples"></a>Exemples

Pour défragmenter le volume sur le lecteur C tout en fournissant la progression et la sortie détaillée, tapez :

```
defrag c: /u /v
```

Pour défragmenter les volumes sur les lecteurs C et D en parallèle en arrière-plan, tapez :

```
defrag c: d: /m
```

Pour effectuer une analyse de fragmentation d’un volume monté sur le lecteur C et indiquer la progression, tapez :

```
defrag c: mountpoint /a /u
```

Pour défragmenter tous les volumes avec une priorité normale et fournir une sortie détaillée, tapez :

```
defrag /c /h /v
```

## <a name="scheduled-task"></a>Tâche planifiée

Le processus de défragmentation exécute la tâche planifiée en tant que tâche de maintenance, qui s’exécute généralement toutes les semaines. En tant qu’administrateur, vous pouvez modifier la fréquence d’exécution de la tâche à l’aide de l’application **optimiser les lecteurs** .

- Lorsqu’elle est exécutée à partir de la tâche planifiée, **Defrag** utilise les instructions de stratégie ci-dessous pour les disques SSD :

  - **Processus d’optimisation traditionnels**. Comprend une **défragmentation traditionnelle**, par exemple le déplacement de fichiers afin de les rendre raisonnablement contigus et de les **réajuster**. Cette opération est effectuée une fois par mois. Toutefois, si la **défragmentation** et la **réajustement** classiques sont ignorés, l' **analyse** n’est pas exécutée.

  - Si vous exécutez manuellement la **défragmentation traditionnelle** sur un disque SSD, entre les exécutions planifiées normalement, l’exécution de la tâche planifiée suivante effectue une **analyse** et une **réajustement**, mais ignore la **défragmentation traditionnelle** sur ce disque SSD.

  - Si vous ignorez l' **analyse**, vous ne verrez pas l’heure de la **dernière exécution** mise à jour dans l’application **optimiser les lecteurs** . C’est la raison pour laquelle l’heure de la **dernière exécution** peut être antérieure à un mois.

  - Vous pouvez constater que la tâche planifiée n’a pas défragmenté tous les volumes. C’est généralement le fait que :

    - Le processus ne sortira pas l’ordinateur pour qu’il s’exécute.

    - L’ordinateur n’est pas branché. Le processus ne s’exécute pas si l’ordinateur fonctionne sur batterie.

    - L’ordinateur a démarré la sauvegarde (reprise à partir de l’inactivité).

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [chkdsk](chkdsk.md)

- [fsutil](fsutil.md)

- [fsutil Dirty](fsutil-dirty.md)

- [Optimiser le volume PowerShell](https://docs.microsoft.com/powershell/module/storage/optimize-volume?view=win10-ps)
