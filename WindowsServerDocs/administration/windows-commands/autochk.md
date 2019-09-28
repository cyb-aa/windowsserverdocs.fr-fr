---
title: autochk
description: La rubrique commandes Windows pour **Autochk** -s’exécute lorsque l’ordinateur est démarré et avant que Windows Server ne commence à vérifier l’intégrité logique d’un système de fichiers.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8787e6a3-f023-4ea5-b2d1-61c6876d8aff
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 76e54d14879cefd4661a1ca7f1c3b8ee7ec58de2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382351"
---
# <a name="autochk"></a>autochk



S’exécute lorsque l’ordinateur est démarré et avant Windows Server® 2008 R2, à partir du début de la vérification de l’intégrité logique d’un système de fichiers.

**Autochk. exe** est une version de **chkdsk** qui s’exécute uniquement sur les disques NTFS et uniquement avant le démarrage de Windows Server 2008 R2. **Autochk** ne peut pas être exécuté directement à partir de la ligne de commande. À la place, **Autochk** s’exécute dans les cas suivants :
-   Si vous essayez d’exécuter **chkdsk** sur le volume de démarrage
-   Si **chkdsk** ne peut pas obtenir l’utilisation exclusive du volume
-   Si le volume est marqué comme modifié

## <a name="remarks"></a>Notes

> -   [!WARNING]
>     L’outil de ligne de commande **Autochk** ne peut pas être exécuté directement à partir de la ligne de commande. Utilisez plutôt l’outil en ligne de commande **chkntfs** pour configurer la façon dont vous souhaitez que **Autochk** s’exécute au démarrage.
> -   Vous pouvez utiliser **chkntfs** avec le paramètre **/x** pour empêcher l’exécution de **Autochk** sur un volume spécifique ou sur plusieurs volumes.
> -   Utilisez l’outil en ligne de commande **chkntfs. exe** avec le paramètre **/t** pour modifier le délai d’Autochk de 0 seconde à 3 jours (259 200 secondes). Toutefois, un long délai signifie que l’ordinateur ne démarre pas avant l’expiration du délai ou jusqu’à ce que vous appuyiez sur une touche pour annuler **Autochk**.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

[Chkdsk](chkdsk.md)

[Chkntfs](chkntfs.md)

[Résolution des problèmes liés aux disques et aux systèmes de fichiers](https://go.microsoft.com/fwlink/?LinkId=4527)