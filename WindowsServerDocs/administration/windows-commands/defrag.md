---
title: defrag
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aaf1d1ac-996a-4282-9b4d-1e8245ff162c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6997e878b2bb7b77a5920ad7398ef7c2301cc8c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813190"
---
# <a name="defrag"></a>defrag

>S'applique à : Windows 10, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Localise et regroupe les fichiers fragmentés sur des volumes locaux pour améliorer les performances du système.
L’appartenance au local **administrateurs** , ou équivalent, est la condition minimale requise pour exécuter cette commande.

## <a name="syntax"></a>Syntaxe
```
defrag <volumes> | /C | /E <volumes>    [/H] [/M [n]| [/U] [/V]]
defrag <volumes> | /C | /E <volumes> /A [/H] [/M [n]| [/U] [/V]]
defrag <volumes> | /C | /E <volumes> /X [/H] [/M [n]| [/U] [/V]]
defrag <volume> [/<Parameter>]*
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|`<volume>`|Spécifie le chemin lecteur du point de montage ou de lettre du volume à être défragmenté ou analysées.|
|A|Effectuer une analyse sur les volumes spécifiés.|
|C|Effectuer l’opération sur tous les volumes.|
|D|Effectuer la défragmentation traditionnelle (il s’agit de la valeur par défaut). Sur un volume à plusieurs niveaux, defrag traditionnel est effectuée uniquement sur le niveau de capacité.|
|E|Effectuer l’opération sur tous les volumes à l’exception de celles spécifiées.|
|G|Optimiser les niveaux de stockage sur les volumes spécifiés.|
|H|Exécuter l’opération avec une priorité normale (valeur par défaut est faible).|
|Je n|Optimisation des niveaux s’exécuterait au plus n secondes sur chaque volume.|
|K|Effectuer la consolidation sur les volumes spécifiés.|
|L|Effectuer retrim sur les volumes spécifiés.|
|M [n]|Exécuter l’opération sur chaque volume en parallèle en arrière-plan. Threads au plus n optimisent les niveaux de stockage en parallèle.|
|O|Effectuer l’optimisation appropriée pour chaque type de média.|
|T|Effectuer le suivi d’une opération déjà en cours d’exécution sur le volume spécifié.|
|U|imprimer la progression de l’opération sur l’écran.|
|V|sortie détaillée contenant les statistiques de fragmentation.|
|X|Effectuer la consolidation de l’espace libre sur les volumes spécifiés.|
|?|Affiche ces informations d’aide.|

## <a name="remarks"></a>Notes
-   Vous ne peuvent pas défragmenter des types de volumes de système de fichiers ou de lecteurs :
    -   Vous ne pouvez pas défragmenter des volumes que le système de fichiers a verrouillé.
    -   Vous ne pouvez pas défragmenter des volumes que le système de fichiers a marqués comme modifié, qui indique un endommagement possible. Vous devez exécuter **chkdsk** sur un volume douteux avant de pouvoir défragmenter. Vous pouvez déterminer si un volume est modifié à l’aide de la **fsutil** incorrectes de commande de requête. Pour plus d’informations sur **chkdsk** et **fsutil** modifiées, consultez [références supplémentaires](defrag.md#BKMK_additionalRef).
    -   Vous ne peuvent pas défragmenter les lecteurs réseau.
    -   Vous ne pouvez pas défragmenter les lecteurs.
    -   Vous ne pouvez pas défragmenter des volumes de système de fichiers qui ne sont pas **NTFS**, **ReFS**, **Fat** ou **Fat32**.
-   Avec Windows Server 2008 R2, Windows Server 2008 et Windows Vista, vous pouvez planifier pour défragmenter un volume. Toutefois, vous ne pouvez pas planifier de défragmenter un lecteur SSD (Solid State) ou un volume situé sur un disque dur virtuel (VHD) qui réside sur un disque SSD.
-   Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs sur l’ordinateur local, ou l’autorité appropriée doit vous avoir été déléguée. Si l'ordinateur est joint à un domaine, les membres du groupe Admins du domaine peuvent être en mesure d'effectuer cette procédure. En tant qu’une sécurité optimale, envisagez d’utiliser **exécuter en tant que** pour effectuer cette procédure.
-   Un volume doit avoir au moins 15 % espace libre pour **defrag** à défragmenter complètement et correctement. **la défragmentation** utilise cet espace comme zone de tri pour les fragments de fichiers. Si un volume possède moins de 15 % d’espace libre, **defrag** défragmentera que partiellement. Pour augmenter l’espace libre sur un volume, supprimez les fichiers superflus ou déplacez-les vers un autre disque.
-   Bien que **defrag** est analyser et de défragmenter un volume, il affiche un curseur clignotant. Lorsque **defrag** est terminé d’analyser et de défragmenter le volume, il affiche le rapport d’analyse, le rapport de défragmentation ou les deux rapports et se termine à l’invite de commandes.
-   Par défaut, **defrag** affiche un résumé des rapports d’analyse et de défragmentation si vous ne spécifiez pas le **/a** ou **/v** paramètres.
-   Vous pouvez envoyer les rapports dans un fichier texte en tapant **>** *FileName.txt*, où *nomfichier.txt* est un nom de fichier que vous spécifiez. Par exemple : `defrag volume /v > FileName.txt`
-   Pour interrompre le processus de défragmentation en ligne de commande, appuyez sur **CTRL + C**.
-   En cours d’exécution le **defrag** commande et Défragmenteur de disque s’excluent mutuellement. Si vous utilisez le Défragmenteur de disque pour défragmenter un volume et que vous exécutez le **defrag** commande sur une ligne de commande, le **defrag** Échec d’une commande. À l’inverse, si vous exécutez le **defrag** commande et ouvrir Défragmenteur de disque, les options de défragmentation Défragmenteur de disque ne sont pas disponibles.

## <a name="BKMK_examples"></a>Exemples
Pour défragmenter le volume sur le lecteur C tout en offrant des cours et une sortie détaillée, tapez :
```
defrag C: /U /V
```
Pour défragmenter les volumes sur les lecteurs C et D en parallèle en arrière-plan, tapez :
```
defrag C: D: /M
```
Pour effectuer une analyse de fragmentation d’un volume monté sur le lecteur C et fournir des cours, tapez :
```
defrag C: mountpoint /A /U
```
Pour défragmenter tous les volumes avec une priorité normale et une sortie détaillée, tapez :
```
defrag /C /H /V
```

## <a name="BKMK_scheduledTask"></a>Tâche planifiée
La défragmentation de tâche planifiée s’exécute comme une tâche de maintenance et généralement devrait s’exécuter chaque semaine. Administrateur peut modifier la fréquence à l’aide **optimiser les lecteurs** application.
- Quand exécuter à partir de la tâche planifiée, **defrag** a stratégie ci-dessous pour les disques SSD :
   - **La défragmentation traditionnel** (par exemple, en déplaçant les fichiers pour les rendre raisonnablement contiguës) et **retrim** est d’exécuter une seule fois par mois.
   - Si les deux **traditionnel defrag** et **retrim** sont ignorées, **analysis** n’est pas exécutée.
      - Si l’utilisateur a exécuté **traditionnel defrag** manuellement sur un disque SSD, par exemple 3 semaines après planifiée de la dernière exécution de tâches, puis exécuter la tâche planifiée suivante effectuera **analysis** et **retrim** mais Ignorer **traditionnel defrag** sur ce disque SSD.
   - Si **analysis** est ignorée, le **de la dernière exécution** temps dans **optimiser les lecteurs** ne sera pas mis à jour.  C’est le cas pour les disques SSD le **de la dernière exécution** temps dans **optimiser les lecteurs** peut être un mois.
- Cette tâche de maintenance peut parfois pas defrag tous les volumes, étant donné que cette tâche effectue les opérations suivantes :
   - Ne sortir les ordinateurs afin d’exécuter la défragmentation
   - Démarre uniquement si l’ordinateur est branché sur secteur et s’arrête si l’ordinateur passe en alimentation par batterie
   - S’arrête si l’ordinateur cesse d’être inactif

## <a name="BKMK_additionalRef"></a>Références supplémentaires
-   [chkdsk](chkdsk.md)
-   [fsutil](fsutil.md)
-   [fsutil dirty](fsutil-dirty.md)
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
