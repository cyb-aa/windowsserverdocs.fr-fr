---
title: autochk
description: Rubrique de référence pour la commande Autochk, qui s’exécute lorsque l’ordinateur est démarré et avant que Windows Server commence à vérifier l’intégrité logique d’un système de fichiers.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8787e6a3-f023-4ea5-b2d1-61c6876d8aff
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a767e8f6cebee9c946f53e0403198384b7c0b790
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718791"
---
# <a name="autochk"></a>autochk

S’exécute lorsque l’ordinateur est démarré et avant que Windows Server ne commence à vérifier l’intégrité logique d’un système de fichiers.

**Autochk. exe** est une version de **chkdsk** qui s’exécute uniquement sur les disques NTFS et uniquement avant le démarrage de Windows Server. **Autochk** ne peut pas être exécuté directement à partir de la ligne de commande. À la place, **Autochk** s’exécute dans les cas suivants :

- Si vous essayez d’exécuter **chkdsk** sur le volume de démarrage.

- Si **chkdsk** ne peut pas obtenir l’utilisation exclusive du volume.

- Si le volume est marqué comme modifié.

## <a name="remarks"></a>Notes 

> [!WARNING]
> L’outil de ligne de commande **Autochk** ne peut pas être exécuté directement à partir de la ligne de commande. Utilisez plutôt l’outil en ligne de commande **chkntfs** pour configurer la façon dont vous souhaitez que **Autochk** s’exécute au démarrage.
>
> - Vous pouvez utiliser **chkntfs** avec le paramètre **/x** pour empêcher l’exécution de **Autochk** sur un volume spécifique ou sur plusieurs volumes.
>
> - Utilisez l’outil en ligne de commande **chkntfs. exe** avec le paramètre **/t** pour modifier le délai d’Autochk de 0 seconde à 3 jours (259 200 secondes). Toutefois, un long délai signifie que l’ordinateur ne démarre pas avant l’expiration du délai ou jusqu’à ce que vous appuyiez sur une touche pour annuler **Autochk**.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande CHKDSK](chkdsk.md)

- [commande chkntfs](chkntfs.md)
