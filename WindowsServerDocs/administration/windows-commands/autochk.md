---
title: autochk
description: 'Rubrique de commandes de Windows pour **autochk** : s’exécute lorsque l’ordinateur est démarré et le démarrage d’avant Windows Server vérifier l’intégrité logique d’un système de fichiers.'
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 023bd81b93106a091fb9f26d97cf7eda75f0f633
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888460"
---
# <a name="autochk"></a>autochk



S’exécute lorsque l’ordinateur est démarré et les versions antérieures à Windows Server® 2008 R2 à partir de vérifier l’intégrité logique d’un système de fichiers.

**Autochk.exe** est une version de **Chkdsk** qui s’exécute uniquement sur des disques NTFS et uniquement avant le démarrage de Windows Server 2008 R2. **Autochk** ne peut pas être exécutée directement à partir de la ligne de commande. Au lieu de cela, **Autochk** s’exécute dans les situations suivantes :
-   Si vous essayez d’exécuter **Chkdsk** sur le volume de démarrage
-   Si **Chkdsk** ne peuvent pas accéder à une utilisation exclusive du volume
-   Si le volume est marqué comme modifié

## <a name="remarks"></a>Notes

> -   [!WARNING]
>     Le **Autochk** outil de ligne de commande ne peut pas être exécuté directement à partir de la ligne de commande. Au lieu de cela, utilisez le **Chkntfs** outil de ligne de commande pour configurer la manière dont vous souhaitez **Autochk** à exécuter au démarrage.
-   Vous pouvez utiliser **Chkntfs** avec la **/x** paramètre pour empêcher **Autochk** de s’exécuter sur un volume spécifique ou plusieurs volumes.
-   Utilisez le **Chkntfs.exe** outil de ligne de commande avec le **/t** paramètre pour modifier le délai de Autochk à partir de 0 seconde à 3 jours (à 259 200 secondes). Toutefois, un long délai signifie que l’ordinateur ne démarre pas tant que le temps écoulé ou jusqu'à ce que vous appuyez sur une touche pour annuler **Autochk**.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

[Chkdsk](chkdsk.md)

[Chkntfs](chkntfs.md)

[Résolution des problèmes de disques et les systèmes de fichiers](https://go.microsoft.com/fwlink/?LinkId=4527)