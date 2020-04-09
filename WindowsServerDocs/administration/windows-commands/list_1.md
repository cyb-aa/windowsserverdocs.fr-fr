---
title: list
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 69b105a1-9710-4a06-8102-38cc9e475ca5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b36600ff4de16bf35b1c2067efac9b13957cefaa
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841032"
---
# <a name="list"></a>list



Affiche la liste des disques, des partitions sur un disque, des volumes d’un disque ou des disques durs virtuels (VHD).

## <a name="syntax"></a>Syntaxe

```
list { disk | partition | volume | vdisk }
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|disque|Affiche la liste des disques et des informations à leur sujet, tels que leur taille, la quantité d’espace libre disponible, si le disque est un disque de base ou dynamique, et si le disque utilise l’enregistrement de démarrage principal (MBR) ou le style de partition GPT (GUID partition table).|
|partition|Affiche les partitions figurant dans la table de partition du disque actuel.|
|volume|Affiche une liste des volumes de base et dynamiques sur tous les disques.|
|Personal|Affiche la liste des disques durs virtuels attachés et/ou sélectionnés. Cette commande répertorie les VHD détachés s’ils sont actuellement sélectionnés. Toutefois, le type de disque est défini sur inconnu jusqu’à ce que le disque dur virtuel soit attaché. Le disque dur virtuel marqué d’un astérisque (*) a le focus.</br>Remarque : cette commande est disponible uniquement pour Windows 7 et Windows Server 2008 R2.|

## <a name="remarks"></a>Notes

-   Lorsque vous répertoriez des partitions sur un disque dynamique, les partitions peuvent ne pas correspondre aux volumes dynamiques sur le disque. Cette différence est due au fait que les disques dynamiques contiennent des entrées dans la table de partition pour le volume système ou le volume de démarrage (s’il est présent sur le disque). Ils contiennent également une partition qui occupe le reste du disque afin de réserver de l’espace pour une utilisation par les volumes dynamiques.
-   L’objet marqué d’un astérisque (*) a le focus.
-   Lorsque vous répertoriez les disques, si un disque est manquant, son numéro de disque est préfixé avec M. Par exemple, le premier disque manquant est numéroté M0.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

```
list disk
list partition
list volume
list vdisk
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

