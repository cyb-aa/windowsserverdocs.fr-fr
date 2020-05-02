---
title: break
description: Rubrique de référence pour la commande Break, qui dissocie un volume de clichés instantanés de VSS et le rend accessible comme un volume normal.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: de2b6c95-1c2e-4a43-bec5-341a9014371b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7e8789ab68ecb98d190a79c3f1088aad05b83562
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82707784"
---
# <a name="break"></a>break

Dissocie un volume de clichés instantanés de VSS et le rend accessible comme un volume normal. Le volume est ensuite accessible à l’aide d’une lettre de lecteur (si affectée) ou d’un nom de volume. En cas d’utilisation sans paramètre, **break** affiche l’aide à l’invite de commandes.

> [!NOTE]
> Cette commande s’applique uniquement aux clichés instantanés matériels après l’importation.
>
> Les volumes exposés, comme les clichés instantanés à partir desquels ils proviennent, sont en lecture seule par défaut. L’accès au volume est effectué directement par le fournisseur de matériel sans enregistrement du volume qui a été un cliché instantané.

## <a name="syntax"></a>Syntaxe

```
break [writable] <setid>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| inscriptible | Active l’accès en lecture/écriture sur le volume. |
| \<> SetID | Spécifie l’ID du jeu de clichés instantanés. L’alias de l’ID de cliché instantané, qui est stocké en tant que variable d’environnement par la commande de **chargement des métadonnées** , peut être utilisé dans le paramètre *SetID* . |

## <a name="examples"></a>Exemples

Pour créer un cliché instantané à l’aide du nom d’alias Alias1 accessible en tant que volume accessible en écriture dans le système d’exploitation :

```
break writable %Alias1%
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)