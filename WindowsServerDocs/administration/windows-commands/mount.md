---
title: monter
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dd9d7ecb-ef00-4aaa-bcd0-423fa636e34a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 65083ce4b7d04129ff630a7f314c458d7ee378f4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437238"
---
# <a name="mount"></a>monter



Vous pouvez utiliser **monter** pour monter les partages de réseau système NFS (Network File).

## <a name="syntax"></a>Syntaxe

```
mount [-o <Option>[...]] [-u:<UserName>] [-p:{<Password> | *}] {\\<ComputerName>\<ShareName> | <ComputerName>:/<ShareName>} {<DeviceName> | *}
```

## <a name="description"></a>Description

Le **monter** utilitaire de ligne de commande monte le système de fichiers identifié par *nom_partage* exporté par le serveur NFS identifié par *ComputerName* et l’associe le lettre de lecteur spécifiée par *DeviceName* ou, si un astérisque ( **&#42;** ) est utilisé par la première lettre de pilotes disponibles. Les utilisateurs peuvent ensuite accéder le système de fichiers exporté comme s’il s’agissait d’un lecteur sur l’ordinateur local. Lorsqu’il est utilisé sans aucune option ou arguments, **monter** affiche des informations sur tous les systèmes de fichiers NFS montés.

Le **monter** utilitaire est disponible uniquement si le Client pour NFS est installé.

Les options et arguments suivants peuvent être utilisés avec le **monter** utilitaire.


|          Terme          |                                                                                                                                                                                                                                                Définition                                                                                                                                                                                                                                                |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -o rsize =\<buffersize > |                                                                                                                                                                                            Définit la taille en kilo-octets, de la mémoire tampon de lecture. Les valeurs acceptables sont 1, 2, 4, 8, 16 et 32 ; la valeur par défaut est 32 Ko.                                                                                                                                                                                            |
| -o wsize =\<buffersize > |                                                                                                                                                                                           Définit la taille en kilo-octets, de la mémoire tampon d’écriture. Les valeurs acceptables sont 1, 2, 4, 8, 16 et 32 ; la valeur par défaut est 32 Ko.                                                                                                                                                                                            |
| -o timeout=\<seconds>  |                                                                                                                                                                       Définit la valeur de délai d’attente en secondes pour un appel de procédure distante (RPC). Les valeurs acceptables sont 0,8, 0,9 et tout entier dans la plage 1-60 ; la valeur par défaut est 0,8.                                                                                                                                                                       |
|   -o retry=\<number>   |                                                                                                                                                                                             Définit le nombre de tentatives pour un montage conditionnel. Les valeurs acceptables sont des entiers dans la plage 1 à 10 ; la valeur par défaut est 1.                                                                                                                                                                                             |
|     -o mtype={soft     |                                                                                                                                                                                                                                                  hard}                                                                                                                                                                                                                                                   |
|        -o anon         |                                                                                                                                                                                                                                       Montages en tant qu’utilisateur anonyme.                                                                                                                                                                                                                                       |
|       -o nolock        |                                                                                                                                                                                                                                Désactive le verrouillage (valeur par défaut est **activé**).                                                                                                                                                                                                                                |
|    -o casesensitive    |                                                                                                                                                                                                                         Force les recherches de fichiers sur le serveur pour respecter la casse.                                                                                                                                                                                                                          |
| -o fileaccess =\<mode >  | Spécifie le mode d’autorisation par défaut des nouveaux fichiers créés sur le partage NFS. Spécifiez *mode* comme un nombre à trois chiffres dans le formulaire *ogw*, où *o*, *g*, et *w* est chacun un chiffre qui représente l’accès accordé le propriétaire du fichier, groupe et le monde, respectivement. Les chiffres doivent être dans la plage 0-7, avec la signification suivante :</br>-   0: Aucun accès</br>-1 : x (accès en exécution)</br>-2 : w (accès en écriture)</br>-3 : wx</br>-4 : r (accès en lecture)</br>-   5: rx</br>-6 : rw</br>-   7: rwx |
|    o - lang = {euc-jp     |                                                                                                                                                                                                                                                  euc-tw                                                                                                                                                                                                                                                  |
|     -u:\<nom d’utilisateur >     |                                                                                                                                                                             Spécifie le nom d’utilisateur à utiliser pour le montage du partage. Si *nom d’utilisateur* n’est pas précédé d’une barre oblique inverse ( **\\** ), il est traité comme un nom d’utilisateur UNIX.                                                                                                                                                                             |
|     -p:\<mot de passe >     |                                                                                                                                                                                          Le mot de passe à utiliser pour le montage du partage. Si vous utilisez un astérisque ( **&#42;** ), vous serez invité pour le mot de passe.                                                                                                                                                                                          |

> [!NOTE]
