---
title: bitsadmin setnotifyflags
description: La rubrique commandes Windows pour Bitsadmin setnotifyflags, qui définit les indicateurs de notification d’événement pour le travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d5763d95-94a6-45ca-9e03-891c20047e06
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fd3001fa4ae7f51cab92556f4f2f498511cca5ae
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849282"
---
# <a name="bitsadmin-setnotifyflags"></a>bitsadmin setnotifyflags

Définit les indicateurs de notification d’événement pour le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetNotifyFlags <Job> <NotifyFlags>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|NotifyFlags|Voir les notes|

## <a name="remarks"></a>Notes

Le paramètre **NotifyFlags** peut contenir un ou plusieurs des indicateurs de notification suivants.

|-----|-----| | 1 | Générez un événement lorsque tous les fichiers du travail ont été transférés. | | 2 | Génère un événement lorsqu’une erreur se produit. | | 4 | Désactiver les notifications. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant définit le travail indicateurs de notification pour les événements de transfert et d’erreur pour le travail nommé *myDownloadJob*.
```
C:\>bitsadmin /SetNotifyFlags myDownloadJob 3
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)