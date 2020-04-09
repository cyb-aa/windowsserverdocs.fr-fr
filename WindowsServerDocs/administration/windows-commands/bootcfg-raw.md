---
title: bootcfg raw
description: La rubrique commandes Windows pour bootcfg RAW, qui ajoute des options de chargement du système d’exploitation, spécifiées sous forme de chaîne, à une entrée du système d’exploitation dans la section système d’exploitation du fichier Boot. ini.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e3458749-b0a0-460f-a022-3ff199a71f27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fd5137187c5ba1dc1b410d728f1c2930ddcbc3cc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848512"
---
# <a name="bootcfg-raw"></a>bootcfg raw

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ajoute des options de chargement du système d’exploitation spécifiées sous forme de chaîne à une entrée du système d’exploitation dans la section **[Operating Systems]** du fichier Boot. ini.

## <a name="syntax"></a>Syntaxe
```
bootcfg /raw [/s <computer> [/u <Domain>\<User> /p <Password>]] <OSLoadOptionsString> [/id <OSEntryLineNum>] [/a]
```
### <a name="parameters"></a>Paramètres

|         Terme          |                                                                                                            Définition                                                                                                             |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /s <computer>     |                                                        Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.                                                         |
| /u <Domain> \\<User>  |               Exécute la commande avec les autorisations de compte de l’utilisateur spécifié par <User> ou <Domain>\\<User>. Par défaut, il s’agit des autorisations de l’utilisateur actuellement connecté sur l’ordinateur qui émet la commande.                |
|     /p <Password>     |                                                                       Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** .                                                                       |
| <OSLoadOptionsString> | Spécifie les options de chargement du système d’exploitation à ajouter à l’entrée du système d’exploitation. Ces options de chargement remplacent les options de chargement existantes associées à l’entrée du système d’exploitation. Aucune validation de <OSLoadOptions> n’est effectuée. |
| /ID <OSEntryLineNum>  |                       Spécifie le numéro de ligne d’entrée du système d’exploitation dans la section [Operating Systems] du fichier Boot. ini à mettre à jour. La première ligne après l’en-tête de la section [Operating Systems] est 1.                       |
|          /a           |                                                       Spécifie que les options de système d’exploitation ajoutées doivent être ajoutées aux options existantes du système d’exploitation.                                                        |
|          /?           |                                                                                               Affiche l'aide à l'invite de commandes.                                                                                                |

##### <a name="remarks"></a>Notes
- **bootcfg RAW** est utilisé pour ajouter du texte à la fin d’une entrée de système d’exploitation, en remplaçant toutes les options d’entrée existantes du système d’exploitation. Ce texte doit contenir des options de chargement de système d’exploitation valides telles que **/Debug**, **/fastdetect**, **/nodebug**, **/baudrate**, **/crashdebug**et **/SOS**. Par exemple, la commande suivante ajoute **/debug/fastdetect** à la fin de la première entrée du système d’exploitation, en remplaçant toutes les options d’entrée précédentes du système d’exploitation :
  ```
  bootcfg /raw /debug /fastdetect /id 1
  ```
  ## <a name="examples"></a><a name=BKMK_examples></a>Illustre
  Les exemples suivants illustrent la façon dont vous pouvez utiliser la commande **bootcfg/RAW** :
  ```
  bootcfg /raw /debug /sos /id 2
  bootcfg /raw /s srvmain /u maindom\hiropln /p p@ssW23 /crashdebug  /id 2
  ```
  ## <a name="additional-references"></a>Références supplémentaires
  - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
