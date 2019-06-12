---
title: gpt
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1d6f9029-807f-4420-a336-36669b5361bc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cba9839f98dfd5a72289838273a057dd0e09a7e5
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438178"
---
# <a name="gpt"></a>gpt

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Disques de base GUID partition gpt (table), affecte l’ou les attributs gpt à la partition qui a le focus.  attributs de partition GPT donnent des informations supplémentaires sur l’utilisation de la partition. Certains attributs sont spécifiques au type de partition GUID.

> [!CAUTION]
> Modifier les attributs gpt risque de vos volumes de données de base à échouer à affecter des lettres de lecteur, ou pour empêcher le système de fichiers de montage. Vous ne devez pas modifier les attributs gpt, sauf si vous êtes un fabricant (OEM) ou un professionnel de l’informatique est familiarisé avec les disques gpt.
> ## <a name="syntax"></a>Syntaxe
> ```
> gpt attributes=<n>
> ```
> ## <a name="parameters"></a>Paramètres
> 
> |   Paramètre    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
> |----------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | attributes=<n> | Spécifie la valeur de l’attribut que vous souhaitez appliquer à la partition qui a le focus. Le champ d’attribut gpt est un champ de 64 bits qui contient deux sous-champs. Le champ de niveau supérieur est interprété uniquement dans le contexte de l’ID de partition, tandis que le champ inférieur est commun à tous les ID de partition. Les valeurs acceptées sont les suivantes :<br /><br />-   **0x0000000000000001**. Spécifie que la partition est requis par l’ordinateur fonctionne correctement.<br />-   **0 x 8000000000000000**. Spécifie que la partition ne recevra une lettre de lecteur par défaut lorsque le disque est déplacé vers un autre ordinateur, ou lorsque le disque est visible pour la première fois par un ordinateur.<br />-   **0x4000000000000000**. Masque les volumes d’une partition. Autrement dit, la partition n’est pas détectée par le Gestionnaire de montage.<br />-   **0x2000000000000000**. Spécifie que la partition est un cliché instantané d’une autre partition.<br />-   **0x1000000000000000**. Spécifie que la partition est en lecture seule. Cet attribut empêche l’écrit dans le volume.<br /><b />Pour plus d’informations sur ces attributs, consultez la section d’attributs à [create_PARTITION_PARAMETERS Structure](https://go.microsoft.com/fwlink/?LinkId=203812) (<https://go.microsoft.com/fwlink/?LinkId=203812>). |
> 
> ## <a name="remarks"></a>Notes
> - La partition système EFI contient uniquement les binaires nécessaires pour démarrer le système d’exploitation. Cela rend plus facile pour les fichiers binaires de l’OEM ou fichiers binaires propres à un système d’exploitation à placer dans les autres partitions.
> - Une partition gpt de base doit être sélectionnée pour cette opération réussisse. Utilisez le **sélectionnez partition** commande pour sélectionner une partition gpt de base et déplacer le focus vers elle.
>   ## <a name="BKMK_examples"></a>Exemples
>   Si vous migrez un disque gpt vers un nouvel ordinateur et que vous souhaitez empêcher l’attribuer automatiquement une lettre de lecteur à la partition avec le focus, le type de cet ordinateur :
>   ```
>   gpt attributes=0x8000000000000000
>   ```

