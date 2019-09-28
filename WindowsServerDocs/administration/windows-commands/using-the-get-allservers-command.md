---
title: Utilisation de la commande AllServers
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe2e3c69-8f2e-457d-af55-d249ebf70f53
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8dd7f9917a54a80b3c570b07fe1a87bd3bcbe4d6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363258"
---
# <a name="using-the-get-allservers-command"></a>Utilisation de la commande AllServers



Récupère des informations sur tous les serveurs des services de déploiement Windows.

> [!NOTE]
> L’exécution de cette commande peut prendre un certain temps s’il existe de nombreux serveurs des services de déploiement Windows dans votre environnement ou si la connexion réseau qui relie les serveurs est lente.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL [Options] /Get-AllServers /Show:{Config | Images | All} [/Detailed] [/Forest:{Yes | No}]
```

## <a name="parameters"></a>Paramètres

|   Paramètre   |                                                                                                                 Description                                                                                                                  |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /Show : {config |                                                                                                                    Images                                                                                                                    |
|  /Detailed  | Lorsqu’il est utilisé conjointement avec **/Show : images** ou **/Show : All**, retourne toutes les métadonnées d’image de chaque image. Si l’option **/detailed** n’est pas spécifiée, le comportement par défaut consiste à retourner le nom de l’image, la description et le nom du fichier. |
| [/Forest : {Oui |                                                                                                                     Non}]                                                                                                                     |

## <a name="BKMK_examples"></a>Illustre

Pour afficher des informations sur tous les serveurs, tapez :
```
WDSUTIL /Get-AllServers /Show:Config
```
Pour afficher des informations détaillées sur tous les serveurs, tapez :
```
WDSUTIL /Verbose /Get-AllServers /Show:All /Detailed /Forest:Yes
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)