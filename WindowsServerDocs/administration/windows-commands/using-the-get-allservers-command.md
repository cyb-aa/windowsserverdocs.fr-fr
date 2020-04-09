---
title: AllServers
description: La rubrique commandes Windows pour la commande AllServers, qui récupère des informations sur tous les serveurs des services de déploiement Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fe2e3c69-8f2e-457d-af55-d249ebf70f53
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b400d5a2be69e8e89a05b233cc2e8f29bec848f6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831212"
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

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

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