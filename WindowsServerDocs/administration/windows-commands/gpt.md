---
title: GPT
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1d6f9029-807f-4420-a336-36669b5361bc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1db2a3fe21b0343c23bbc2432d4d07a500b72e8e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842452"
---
# <a name="gpt"></a>GPT

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sur les disques GPT (GUID partition table) de base, attribue le ou les attributs GPT à la partition qui a le focus.  les attributs de partition GPT fournissent des informations supplémentaires sur l’utilisation de la partition. Certains attributs sont spécifiques au GUID du type de partition.

> [!CAUTION]
> La modification des attributs GPT peut entraîner l’échec de l’affectation des lettres de lecteur aux volumes de données de base ou empêcher le montage du système de fichiers. Vous ne devez pas modifier les attributs GPT, sauf si vous êtes un fabricant OEM (Original Equipment Manufacturer) ou un professionnel de l’informatique expérimenté avec les disques GPT.

## <a name="syntax"></a>Syntaxe

```
gpt attributes=<n>
```

### <a name="parameters"></a>Paramètres

|   Paramètre    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|----------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| attributs =<n> | Spécifie la valeur de l’attribut que vous souhaitez appliquer à la partition qui a le focus. Le champ d’attribut GPT est un champ de 64 bits qui contient deux sous-champs. Le champ supérieur est interprété uniquement dans le contexte de l’ID de partition, tandis que le champ inférieur est commun à tous les ID de partition. Les valeurs acceptées sont les suivantes :<p>-   **0x0000000000000001**. Spécifie que la partition est requise par l’ordinateur pour fonctionner correctement.<br />-   **0x8000000000000000**. Spécifie que la partition ne recevra pas de lettre de lecteur par défaut lorsque le disque est déplacé vers un autre ordinateur ou lorsque le disque est visible pour la première fois par un ordinateur.<br />-   **0x4000000000000000**. Masque le volume d’une partition. Autrement dit, la partition n’est pas détectée par le gestionnaire de montage.<br />-   **0x2000000000000000**. Spécifie que la partition est un cliché instantané d’une autre partition.<br />-   **0x1000000000000000**. Spécifie que la partition est en lecture seule. Cet attribut empêche l’écriture du volume dans.<p>Pour plus d’informations sur ces attributs, consultez la section attributs dans [Create_PARTITION_PARAMETERS structure](https://go.microsoft.com/fwlink/?LinkId=203812). |

## <a name="remarks"></a>Notes

- La partition système EFI contient uniquement les fichiers binaires nécessaires au démarrage du système d’exploitation. Cela permet de placer facilement dans d’autres partitions des fichiers binaires ou binaires OEM spécifiques à un système d’exploitation.
- Une partition GPT de base doit être sélectionnée pour que cette opération aboutisse. Utilisez la commande **Sélectionner une partition** pour sélectionner une partition GPT de base et y déplacer le focus.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

  Si vous déplacez un disque GPT vers un nouvel ordinateur et que vous souhaitez empêcher cet ordinateur d’attribuer automatiquement une lettre de lecteur à la partition qui a le focus, tapez :
  ```
  gpt attributes=0x8000000000000000
  ```
