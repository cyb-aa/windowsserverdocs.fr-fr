---
title: defrag
description: La rubrique commandes Windows pour Defrag, qui localise et consolide les fichiers fragmentés sur des volumes locaux pour améliorer les performances du système.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aaf1d1ac-996a-4282-9b4d-1e8245ff162c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8723afc936fa1ea311e275a58a85b20988f92a2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846712"
---
# <a name="defrag"></a>defrag

>S’applique à : Windows 10, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Localise et consolide les fichiers fragmentés sur les volumes locaux pour améliorer les performances du système.

L’appartenance au groupe **administrateurs** local, ou équivalent, est la condition minimale requise pour exécuter cette commande.

## <a name="syntax"></a>Syntaxe
```
defrag <volumes> | /C | /E <volumes>    [/H] [/M [n]| [/U] [/V]]
defrag <volumes> | /C | /E <volumes> /A [/H] [/M [n]| [/U] [/V]]
defrag <volumes> | /C | /E <volumes> /X [/H] [/M [n]| [/U] [/V]]
defrag <volume> [/<Parameter>]*
```
### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------|--------|
|`<volume>`|Spécifie la lettre de lecteur ou le chemin d’accès du point de montage du volume à défragmenter ou à analyser.|
|A|Effectuer une analyse sur les volumes spécifiés.|
|C|Effectuez l’opération sur tous les volumes.|
|D|Effectuer une défragmentation traditionnelle (il s’agit de la valeur par défaut). En revanche, sur un volume hiérarchisé, la défragmentation traditionnelle est exécutée uniquement sur le niveau de capacité.|
|E|Effectuez l’opération sur tous les volumes, à l’exception de ceux spécifiés.|
|I|Optimisez les niveaux de stockage sur les volumes spécifiés.|
|H|Exécutez l’opération en priorité normale (la valeur par défaut est Low).|
|I n|L’optimisation du niveau s’exécuterait pendant au plus n secondes sur chaque volume.|
|K|Effectuez la consolidation des sections sur les volumes spécifiés.|
|L|Effectuer une réajustement sur les volumes spécifiés.|
|M [n]|Exécutez l’opération sur chaque volume en parallèle en arrière-plan. Au plus n threads optimisent les niveaux de stockage en parallèle.|
|O|Effectuez l’optimisation appropriée pour chaque type de média.|
|T|Effectue le suivi d’une opération déjà en cours sur le volume spécifié.|
|U|Imprimez la progression de l’opération à l’écran.|
|V|Affichez la sortie détaillée contenant les statistiques de fragmentation.|
|X|Effectuez une consolidation de l’espace libre sur les volumes spécifiés.|
|?|Affiche ces informations d’aide.|

## <a name="remarks"></a>Notes
- Vous ne pouvez pas défragmenter des types spécifiques de volumes ou de lecteurs de système de fichiers :
  -   Vous ne pouvez pas défragmenter des volumes verrouillés par le système de fichiers.
  -   Vous ne pouvez pas défragmenter des volumes marqués comme modifiés par le système de fichiers, ce qui indique une éventuelle altération. Vous devez exécuter **chkdsk** sur un volume endommagé pour pouvoir le défragmenter. Vous pouvez déterminer si un volume est modifié à l’aide de la commande **fsutil** Dirty Query. Pour plus d’informations sur **chkdsk** et sur **fsutil** Dirty, consultez [Références supplémentaires](defrag.md#BKMK_additionalRef).
  -   Vous ne pouvez pas défragmenter les lecteurs réseau.
  -   Vous ne pouvez pas défragmenter les cdROM.
  -   Vous ne pouvez pas défragmenter des volumes de système de fichiers qui ne sont pas **NTFS**, **ReFS**, **FAT** ou **FAT32**.
- Avec Windows Server 2008 R2, Windows Server 2008 et, Windows Vista, vous pouvez planifier la défragmentation d’un volume. Toutefois, vous ne pouvez pas planifier la défragmentation d’un disque SSD (Solid State Drive) ou d’un volume sur un disque dur virtuel (VHD) qui réside sur un disque SSD.
- Pour réaliser cette procédure, vous devez être membre du groupe Administrateurs sur l'ordinateur local ou bien disposer de l'autorité appropriée. Si l'ordinateur est joint à un domaine, les membres du groupe Administrateurs du domaine doivent pouvoir suivre cette procédure. Pour des raisons de sécurité, envisagez d’utiliser **exécuter en tant que** pour effectuer cette procédure.
- Un volume doit disposer d’au moins 15% d’espace **libre pour être** défragmenté complètement et correctement défragmenté. **Defrag** utilise cet espace comme zone de tri pour les fragments de fichier. Si un volume dispose de moins de 15% d’espace libre, **Defrag** ne le défragmentera que partiellement. Pour augmenter l’espace libre sur un volume, supprimez les fichiers superflus ou déplacez-les sur un autre disque.
- Alors que **Defrag** analyse et défragmente un volume, il affiche un curseur clignotant. Lorsque **Defrag** a fini d’analyser et de défragmenter le volume, il affiche le rapport d’analyse, le rapport de défragmentation, ou les deux rapports, puis se ferme à l’invite de commandes.
- Par défaut, **Defrag** affiche un résumé des rapports d’analyse et de défragmentation si vous ne spécifiez pas les paramètres **/a** ou **/v** .
- Vous pouvez envoyer les rapports vers un fichier texte en tapant **>** <em>nom_fichier. txt</em>, où *nom_fichier. txt* est un nom de fichier que vous spécifiez. Par exemple : `defrag volume /v > FileName.txt`
- Pour interrompre le processus de défragmentation, sur la ligne de commande, appuyez sur **Ctrl + C**.
- L’exécution de la commande **Defrag** et du défragmenteur de disque s’exclut mutuellement. Si vous utilisez le Défragmenteur de disque pour défragmenter un volume et que vous exécutez la commande **Defrag** sur une ligne de commande, la commande **Defrag** échoue. À l’inverse, si vous exécutez la commande **Defrag** et que vous ouvrez le Défragmenteur de disque, les options de défragmentation du défragmenteur de disque ne sont pas disponibles.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Pour défragmenter le volume sur le lecteur C tout en fournissant la progression et la sortie détaillée, tapez :
```
defrag C: /U /V
```
Pour défragmenter les volumes sur les lecteurs C et D en parallèle en arrière-plan, tapez :
```
defrag C: D: /M
```
Pour effectuer une analyse de fragmentation d’un volume monté sur le lecteur C et indiquer la progression, tapez :
```
defrag C: mountpoint /A /U
```
Pour défragmenter tous les volumes avec une priorité normale et fournir une sortie détaillée, tapez :
```
defrag /C /H /V
```

## <a name="scheduled-task"></a><a name=BKMK_scheduledTask></a>Tâche planifiée
La tâche planifiée de la défragmentation s’exécute en tant que tâche de maintenance et est généralement planifiée pour s’exécuter toutes les semaines. L’administrateur peut modifier la fréquence à l’aide de l’application **optimiser les lecteurs** .
- Lorsqu’elle est exécutée à partir de la tâche planifiée, **Defrag** possède la stratégie ci-dessous pour SSDS :
   - La **défragmentation traditionnelle** (c’est-à-dire le déplacement de fichiers pour les rendre raisonnablement contigus) et la **réajustement** ne sont exécutées qu’une seule fois par mois.
   - Si la **défragmentation** et la **réajustement** traditionnels sont ignorés, l' **analyse** n’est pas exécutée.
      - Si l’utilisateur a exécuté la **défragmentation traditionnelle** manuellement sur un disque SSD, par exemple 3 semaines après la dernière exécution de la tâche planifiée, la prochaine exécution de la tâche planifiée effectue l' **analyse** et le **Retrim** , mais ignore la **défragmentation traditionnelle** sur ce disque SSD.
   - Si l' **analyse** est ignorée, la dernière heure d' **exécution** dans **optimiser les lecteurs** ne sera pas mise à jour.  Ainsi, pour les disques SSD, l’heure de la **dernière exécution** dans **optimiser les lecteurs** peut être datant d’un mois.
- Cette tâche de maintenance peut ne pas défragmenter tous les volumes, à des moments où cette tâche effectue les opérations suivantes :
   - Ne sort pas l’ordinateur pour exécuter Defrag
   - Démarre uniquement si l’ordinateur est alimenté en secteur et s’arrête si l’ordinateur passe à l’alimentation par batterie
   - S’arrête si l’ordinateur cesse d’être inactif

## <a name="additional-references"></a><a name=BKMK_additionalRef></a>Références supplémentaires
-   [chkdsk](chkdsk.md)
-   [fsutil](fsutil.md)
-   [fsutil Dirty](fsutil-dirty.md)
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
