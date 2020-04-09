---
title: append
description: La rubrique commandes Windows pour **Append**, qui permet aux programmes d’ouvrir des fichiers de données dans les répertoires spécifiés, comme s’ils étaient dans le répertoire actif.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9c3fea20-9502-40ad-a442-7a927aad88eb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 95bbc607ef297e7cf67da2e388884882356ef744
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851322"
---
# <a name="append"></a>append

Permet aux programmes d’ouvrir des fichiers de données dans les répertoires spécifiés comme s’ils étaient dans le répertoire actif. En cas d’utilisation sans paramètre, **Append** affiche la liste des répertoires ajoutés.

> [!NOTE]
> Cette commande n’est pas prise en charge dans Windows 10.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
append [[<Drive>:]<Path>[;...]] [/x[:on|:off]] [/path:[:on|:off] [/e] 
append ;
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `[\<Drive>:]<Path>` | Spécifie un lecteur et un répertoire à ajouter. |
| `/x:on` | Applique les répertoires ajoutés aux recherches de fichiers et le lancement d’applications. |
| `/x:off` | Applique les répertoires ajoutés uniquement aux demandes d’ouverture de fichiers. L’option **/x : OFF** est le paramètre par défaut. |
| `/path:on` | Applique des répertoires ajoutés aux demandes de fichier qui spécifient déjà un chemin d’accès. **/Path : on** est le paramètre par défaut. |
| `/path:off` | Désactive l’effet de **/Path : on**. |
| `/e` | Stocke une copie de la liste des répertoires ajoutés dans une variable d’environnement nommée APPEND. **/e** peut être utilisé uniquement la première fois que vous utilisez **Append** après le démarrage de votre système. |
| `;` | Efface la liste des répertoires ajoutés. |
| `/?` | Affiche l'aide à l'invite de commandes. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour effacer la liste des répertoires ajoutés, tapez :

```
append ;
```

Pour stocker une copie du répertoire ajouté à une variable d’environnement nommée APPEND, tapez :

```
append /e
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
