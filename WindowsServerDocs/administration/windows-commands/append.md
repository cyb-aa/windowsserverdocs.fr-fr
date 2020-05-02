---
title: append
description: Rubrique de référence pour la commande Append, qui permet aux programmes d’ouvrir des fichiers de données dans les répertoires spécifiés, comme s’ils se trouvaient dans le répertoire actif.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9c3fea20-9502-40ad-a442-7a927aad88eb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 562a13c6b1a47e43bb66548902f0b8e57e789a34
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718996"
---
# <a name="append"></a>append

Permet aux programmes d’ouvrir des fichiers de données dans les répertoires spécifiés comme s’ils étaient dans le répertoire actif. En cas d’utilisation sans paramètre, **Append** affiche la liste des répertoires ajoutés.

> [!NOTE]
> Cette commande n’est pas prise en charge dans Windows 10.

## <a name="syntax"></a>Syntaxe

```
append [[<drive>:]<path>[;...]] [/x[:on|:off]] [/path:[:on|:off] [/e]
append ;
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `[\<drive>:]<path>` | Spécifie un lecteur et un répertoire à ajouter. |
| /x : activé | Applique les répertoires ajoutés aux recherches de fichiers et le lancement d’applications. |
| /x : désactivé | Applique les répertoires ajoutés uniquement aux demandes d’ouverture de fichiers. L’option **/x : OFF** est le paramètre par défaut. |
| /Path : activé | Applique des répertoires ajoutés aux demandes de fichier qui spécifient déjà un chemin d’accès. **/Path : on** est le paramètre par défaut. |
| /Path : désactivé | Désactive l’effet de **/Path : on**. |
| /e | Stocke une copie de la liste des répertoires ajoutés dans une variable d’environnement nommée APPEND. **/e** peut être utilisé uniquement la première fois que vous utilisez **Append** après le démarrage de votre système. |
| ; | Efface la liste des répertoires ajoutés. |
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="examples"></a>Exemples

Pour effacer la liste des répertoires ajoutés, tapez :

```
append ;
```

Pour stocker une copie du répertoire ajouté à une variable d’environnement nommée *Append*, tapez :

```
append /e
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
