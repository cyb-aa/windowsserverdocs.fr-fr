---
title: bootcfg addsw
description: Rubrique de référence pour la commande bootcfg Addsw, qui ajoute des options de chargement du système d’exploitation pour une entrée de système d’exploitation spécifiée.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d8389293-ecd9-42f0-b84b-b9ead4cf52e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 17abdc1ba28afad173ea6486519277916f08ad3d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82709942"
---
# <a name="bootcfg-addsw"></a>bootcfg addsw

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ajoute des options de chargement du système d’exploitation pour une entrée de système d’exploitation spécifiée.

## <a name="syntax"></a>Syntaxe

```
bootcfg /addsw [/s <computer> [/u <domain>\<user> /p <password>]] [/mm <maximumram>] [/bv] [/so] [/ng] /id <osentrylinenum>
```

### <a name="parameters"></a>Paramètres

| Terme | Définition |
| ---- | ---------- |
| `/s <computer>` | Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local. |
| `/u <domain>\<user>`  | Exécute la commande avec les autorisations de compte de l’utilisateur spécifié `<user>` par `<domain>\<user>`ou. Par défaut, il s’agit des autorisations de l’utilisateur actuellement connecté sur l’ordinateur qui émet la commande. |
| `/p <password>` | Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** . |
| `/mm <maximumram>` | Spécifie la quantité maximale de mémoire vive (en mégaoctets) pouvant être utilisée par le système d’exploitation. La valeur doit être supérieure ou égale à 32 mégaoctets. |
| /bv | Ajoute l’option **/basevideo** au spécifié `<osentrylinenum>`, en dirigeant le système d’exploitation pour qu’il utilise le mode VGA standard pour le pilote vidéo installé. |
| /So | Ajoute l’option **/SOS** au spécifié `<osentrylinenum>`, en dirigeant le système d’exploitation afin d’afficher les noms des pilotes de périphériques pendant leur chargement. |
| /ng | Ajoute l’option **/noguiboot** au spécifié `<osentrylinenum>`, en désactivant la barre de progression qui s’affiche avant l’invite d’ouverture de session Ctrl + Alt + Suppr. |
| `/id <osentrylinenum>` | Spécifie le numéro de ligne d’entrée du système d’exploitation dans la section [Operating Systems] du fichier Boot. ini auquel les options de chargement du système d’exploitation sont ajoutées. La première ligne après l’en-tête de la section [Operating Systems] est 1. |
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="examples"></a>Exemples

Pour utiliser la commande **bootcfg/Addsw** :

```
bootcfg /addsw /mm 64 /id 2
bootcfg /addsw /so /id 3
bootcfg /addsw /so /ng /s srvmain /u hiropln /id 2
bootcfg /addsw /ng /id 2
bootcfg /addsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande bootcfg](bootcfg.md)
