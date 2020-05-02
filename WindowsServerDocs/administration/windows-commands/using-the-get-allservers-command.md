---
title: AllServers
description: Rubrique de référence pour la fonction AllServers, qui récupère des informations sur tous les serveurs des services de déploiement Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fe2e3c69-8f2e-457d-af55-d249ebf70f53
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b623b5e95e2a57147b7d9d191d42556191dd8e4d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719982"
---
# <a name="get-allservers"></a>AllServers

Récupère des informations sur tous les serveurs des services de déploiement Windows.

> [!NOTE]
> L’exécution de cette commande peut prendre un certain temps s’il existe de nombreux serveurs des services de déploiement Windows dans votre environnement ou si la connexion réseau qui relie les serveurs est lente.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL [Options] /Get-AllServers /Show:{Config | Images | All} [/Detailed] [/Forest:{Yes | No}]
```

### <a name="parameters"></a>Paramètres

|   Paramètre   |                                                                                                                 Description                                                                                                                  |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /Show : {config |                                                                                                                    Images                                                                                                                    |
|  /Detailed  | Lorsqu’il est utilisé conjointement avec **/Show : images** ou **/Show : All**, retourne toutes les métadonnées d’image de chaque image. Si l’option **/detailed** n’est pas spécifiée, le comportement par défaut consiste à retourner le nom de l’image, la description et le nom du fichier. |
| [/Forest : {Oui |                                                                                                                     Non}]                                                                                                                     |

## <a name="examples"></a>Exemples

Pour afficher des informations sur tous les serveurs, tapez :
```
WDSUTIL /Get-AllServers /Show:Config
```
Pour afficher des informations détaillées sur tous les serveurs, tapez :
```
WDSUTIL /Verbose /Get-AllServers /Show:All /Detailed /Forest:Yes
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)