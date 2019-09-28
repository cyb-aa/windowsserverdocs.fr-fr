---
title: monter
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 9a225c847055198a9a48962a3b40969556f10ec1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373586"
---
# <a name="mount"></a>monter



Vous pouvez utiliser le **montage** pour monter des partages réseau NFS (Network File System).

## <a name="syntax"></a>Syntaxe

```
mount [-o <Option>[...]] [-u:<UserName>] [-p:{<Password> | *}] {\\<ComputerName>\<ShareName> | <ComputerName>:/<ShareName>} {<DeviceName> | *}
```

## <a name="description"></a>Description

L’utilitaire de ligne de commande **Mount** monte le système de fichiers identifié par *Nom_Partage* exporté par le serveur NFS identifié par *ComputerName* et l’associe à la lettre de lecteur spécifiée par *DeviceName* ou, si un astérisque ( **&#42;** ) est utilisé, par la première lettre de lecteur disponible. Les utilisateurs peuvent ensuite accéder au système de fichiers exporté comme s’il s’agissait d’un lecteur sur l’ordinateur local. Lorsqu’il est utilisé sans options ou arguments, **Mount** affiche des informations sur tous les systèmes de fichiers NFS montés.

L’utilitaire **Mount** est disponible uniquement si le client pour NFS est installé.

Les options et les arguments suivants peuvent être utilisés avec l’utilitaire **Mount** .


|          Terme          |                                                                                                                                                                                                                                                Définition                                                                                                                                                                                                                                                |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -o rsize = \<buffersize > |                                                                                                                                                                                            Définit la taille en kilo-octets de la mémoire tampon de lecture. Les valeurs acceptables sont 1, 2, 4, 8, 16 et 32 ; la valeur par défaut est 32 Ko.                                                                                                                                                                                            |
| -o wsize = \<buffersize > |                                                                                                                                                                                           Définit la taille en kilo-octets de la mémoire tampon d’écriture. Les valeurs acceptables sont 1, 2, 4, 8, 16 et 32 ; la valeur par défaut est 32 Ko.                                                                                                                                                                                            |
| -o Timeout = \<seconds >  |                                                                                                                                                                       Définit la valeur du délai d’attente en secondes pour un appel de procédure distante (RPC). Les valeurs acceptables sont 0,8, 0,9 et tout entier dans la plage 1-60 ; la valeur par défaut est 0,8.                                                                                                                                                                       |
|   -o Retry = \<number >   |                                                                                                                                                                                             Définit le nombre de nouvelles tentatives pour un montage conditionnel. Les valeurs acceptables sont des entiers dans la plage 1-10 ; la valeur par défaut est 1.                                                                                                                                                                                             |
|     -o mtype = {Soft     |                                                                                                                                                                                                                                                  réels                                                                                                                                                                                                                                                   |
|        -o anon         |                                                                                                                                                                                                                                       Monte en tant qu’utilisateur anonyme.                                                                                                                                                                                                                                       |
|       -o NOLOCK        |                                                                                                                                                                                                                                Désactive le verrouillage ( **activé**par défaut).                                                                                                                                                                                                                                |
|    -o CaseSensitive    |                                                                                                                                                                                                                         Force les recherches de fichiers sur le serveur à respecter la casse.                                                                                                                                                                                                                          |
| -o FileAccess = \<Mode >  | Spécifie le mode d’autorisation par défaut des nouveaux fichiers créés sur le partage NFS. Spécifiez le *mode* sous la forme d’un nombre à trois chiffres sous la forme *ogw*, où *o*, *g*et *w* sont chacun un chiffre représentant l’accès qui a accordé le propriétaire, le groupe et le monde du fichier, respectivement. Les chiffres doivent se trouver dans la plage 0-7 avec la signification suivante :</br>entre Aucun accès</br>-1 : x (exécuter l’accès)</br>-2 : w (accès en écriture)</br>-3 : WX</br>-4 : r (accès en lecture)</br>-5 : RX</br>-6 : RW</br>-7 : rwx |
|    -o lang = {EUC-JP     |                                                                                                                                                                                                                                                  EUC-TW                                                                                                                                                                                                                                                  |
|     -u : \<UserName >     |                                                                                                                                                                             Spécifie le nom d’utilisateur à utiliser pour le montage du partage. Si le *nom d'* utilisateur n’est pas précédé d’une barre oblique inverse ( **\\** ), il est traité comme un nom d’utilisateur UNIX.                                                                                                                                                                             |
|     -p : \<Password >     |                                                                                                                                                                                          Mot de passe à utiliser pour le montage du partage. Si vous utilisez un astérisque ( **&#42;** ), vous êtes invité à entrer le mot de passe.                                                                                                                                                                                          |

> [!NOTE]
