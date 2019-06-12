---
title: bootcfg raw
description: Rubrique de commandes de Windows pour **bootcfg brutes** -ajoute des options de chargement du système d’exploitation spécifiées sous forme de chaîne à une entrée de système d’exploitation dans le **[systèmes d’exploitation]** section du fichier Boot.ini.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e3458749-b0a0-460f-a022-3ff199a71f27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5334ff8a1c5d15343b4a48814b52012c641016a4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434694"
---
# <a name="bootcfg-raw"></a>bootcfg raw

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ajoute les options de chargement du système d’exploitation spécifiées sous forme de chaîne à une entrée de système d’exploitation dans le **[systèmes d’exploitation]** section du fichier Boot.ini.

## <a name="syntax"></a>Syntaxe
```
bootcfg /raw [/s <computer> [/u <Domain>\<User> /p <Password>]] <OSLoadOptionsString> [/id <OSEntryLineNum>] [/a]
```
## <a name="parameters"></a>Paramètres

|         Terme          |                                                                                                            Définition                                                                                                             |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /s <computer>     |                                                        Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.                                                         |
| /u <Domain> \\<User>  |               Exécute la commande avec les autorisations de compte de l’utilisateur spécifié par <User> ou <Domain> \\ <User>. La valeur par défaut est les autorisations de l’utilisateur actuellement connecté sur l’ordinateur exécutant la commande.                |
|     /p <Password>     |                                                                       Spécifie le mot de passe du compte d’utilisateur qui est spécifié dans le **/u** paramètre.                                                                       |
| <OSLoadOptionsString> | Spécifie les options de chargement de système d’exploitation à ajouter à l’entrée de système d’exploitation. Ces options de chargement remplacent les options de chargement existant associées à l’entrée de système d’exploitation. Aucune validation de <OSLoadOptions> est effectuée. |
| /id <OSEntryLineNum>  |                       Spécifie le numéro de ligne d’entrée système d’exploitation dans la section [operating systems] du fichier Boot.ini à mettre à jour. La première ligne après l’en-tête de section de la section [operating systems] est 1.                       |
|          /a           |                                                       Spécifie que les options de système d’exploitation en cours d’ajout doivent être ajoutées à des options de système d’exploitation existant.                                                        |
|          /?           |                                                                                               Affiche l'aide à l'invite de commandes.                                                                                                |

##### <a name="remarks"></a>Notes
- **BOOTCFG brutes** est utilisé pour ajouter du texte à la fin d’une entrée de système d’exploitation, en remplaçant tout options d’entrée de système d’exploitation existant. Ce texte doit contenir des Options de chargement du système d’exploitation valide tel que **/debug**, **/fastdetect**, **/nodebug.** , **/baudrate**, **/ /crashdebug**, et **/SOS**. Par exemple, la commande suivante ajoute « **/debug /fastdetect**» à la fin de la première entrée de système d’exploitation, en remplaçant tout options d’entrée de système d’exploitation précédent :
  ```
  bootcfg /raw "/debug /fastdetect" /id 1
  ```
  ## <a name="BKMK_examples"></a>Exemples
  Les exemples suivants montrent comment vous pouvez utiliser la **bootcfg / brutes** commande :
  ```
  bootcfg /raw "/debug /sos" /id 2
  bootcfg /raw /s srvmain /u maindom\hiropln /p p@ssW23 "/crashdebug " /id 2
  ```
  #### <a name="additional-references"></a>Références supplémentaires
  [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
