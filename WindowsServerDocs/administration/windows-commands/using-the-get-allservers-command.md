---
title: À l’aide de la commande get-AllServers
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 080836e406b329cf8c15f95ef6afc99973bb3e4d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853090"
---
# <a name="using-the-get-allservers-command"></a>À l’aide de la commande get-AllServers



Récupère des informations sur tous les serveurs de Services de déploiement Windows.

> [!NOTE]
> Cette commande peut prendre un certain temps s’il existe plusieurs serveurs de Services de déploiement Windows dans votre environnement, ou si la connexion réseau que les serveurs de la liaison est lente.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL [Options] /Get-AllServers /Show:{Config | Images | All} [/Detailed] [/Forest:{Yes | No}]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/ Afficher : {Config | Images | All}|Spécifie le type d’informations à retourner.</br>-   **Config** retourne des informations de configuration de serveur.</br>-   **Images** retourne des informations sur les groupes d’images, les images de démarrage et les images d’installation sur le serveur.</br>-   **Tous les** retourne des informations de configuration et l’image du serveur.|
|[/Detailed]|Lorsqu’il est utilisé conjointement avec le **/Show : images** ou **/Show : all**, retourne toutes les métadonnées de chaque image de l’image. Si le **/détaillées** option n’est pas spécifiée, le comportement par défaut consiste à retourner le nom de l’image, la description et le nom de fichier.|
|[/ Forêt : {Oui | No}]|Spécifie s’il faut retourner des informations sur l’ensemble de la forêt ou le domaine local. Si une valeur pour cette option n’est pas spécifiée, le comportement par défaut consiste à retourner les serveurs dans le domaine local.|

## <a name="BKMK_examples"></a>Exemples

Pour afficher des informations sur tous les serveurs, tapez :
```
WDSUTIL /Get-AllServers /Show:Config
```
Pour afficher des informations détaillées sur tous les serveurs, tapez :
```
WDSUTIL /Verbose /Get-AllServers /Show:All /Detailed /Forest:Yes
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)