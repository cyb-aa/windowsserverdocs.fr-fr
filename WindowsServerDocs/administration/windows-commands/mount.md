---
title: mount
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dd9d7ecb-ef00-4aaa-bcd0-423fa636e34a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2e7ef55f5e5b66a0501c62767968a4192880040f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723938"
---
# <a name="mount"></a>mount



Vous pouvez utiliser le **montage** pour monter des partages réseau NFS (Network File System).

## <a name="syntax"></a>Syntaxe

```
mount [-o <Option>[...]] [-u:<UserName>] [-p:{<Password> | *}] {\\<ComputerName>\<ShareName> | <ComputerName>:/<ShareName>} {<DeviceName> | *}
```

## <a name="description"></a>Description

L’utilitaire de ligne de commande **Mount** monte le système de fichiers identifié par *Nom_Partage* exporté par le serveur NFS identifié par *ComputerName* et l’associe à la lettre de lecteur spécifiée par *DeviceName* ou, si un astérisque (**&#42;**) est utilisé, par la première lettre de lecteur disponible. Les utilisateurs peuvent ensuite accéder au système de fichiers exporté comme s’il s’agissait d’un lecteur sur l’ordinateur local. Lorsqu’il est utilisé sans options ou arguments, **Mount** affiche des informations sur tous les systèmes de fichiers NFS montés.

L’utilitaire **Mount** est disponible uniquement si le client pour NFS est installé.

Les options et les arguments suivants peuvent être utilisés avec l’utilitaire **Mount** .


|          Terme          |                                                                                                                                                                                                                                                Définition                                                                                                                                                                                                                                                |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -o rsize =\<bufferSize> |                                                                                                                                                                                            Définit la taille en kilo-octets de la mémoire tampon de lecture. Les valeurs acceptables sont 1, 2, 4, 8, 16 et 32 ; la valeur par défaut est 32 Ko.                                                                                                                                                                                            |
| -o wsize =\<bufferSize> |                                                                                                                                                                                           Définit la taille en kilo-octets de la mémoire tampon d’écriture. Les valeurs acceptables sont 1, 2, 4, 8, 16 et 32 ; la valeur par défaut est 32 Ko.                                                                                                                                                                                            |
| -o Timeout =\<secondes>  |                                                                                                                                                                       Définit la valeur du délai d’attente en secondes pour un appel de procédure distante (RPC). Les valeurs acceptables sont 0,8, 0,9 et tout entier dans la plage 1-60 ; la valeur par défaut est 0,8.                                                                                                                                                                       |
|   -o Retry =\<nombre>   |                                                                                                                                                                                             Définit le nombre de nouvelles tentatives pour un montage conditionnel. Les valeurs acceptables sont des entiers dans la plage 1-10 ; la valeur par défaut est 1.                                                                                                                                                                                             |
|     -o mtype = {Soft     |                                                                                                                                                                                                                                                  réels                                                                                                                                                                                                                                                   |
|        -o anon         |                                                                                                                                                                                                                                       Monte en tant qu’utilisateur anonyme.                                                                                                                                                                                                                                       |
|       -o NOLOCK        |                                                                                                                                                                                                                                Désactive le verrouillage ( **activé**par défaut).                                                                                                                                                                                                                                |
|    -o CaseSensitive    |                                                                                                                                                                                                                         Force les recherches de fichiers sur le serveur à respecter la casse.                                                                                                                                                                                                                          |
| -o FileAccess =\<mode>  | Spécifie le mode d’autorisation par défaut des nouveaux fichiers créés sur le partage NFS. Spécifiez le *mode* sous la forme d’un nombre à trois chiffres sous la forme *ogw*, où *o*, *g*et *w* sont chacun un chiffre représentant l’accès qui a accordé le propriétaire, le groupe et le monde du fichier, respectivement. Les chiffres doivent se trouver dans la plage 0-7 avec la signification suivante :</br>-0 : aucun accès</br>-1 : x (exécuter l’accès)</br>-2 : w (accès en écriture)</br>-3 : WX</br>-4 : r (accès en lecture)</br>-5 : RX</br>-6 : RW</br>-7 : rwx |
|    -o lang = {EUC-JP     |                                                                                                                                                                                                                                                  EUC-TW                                                                                                                                                                                                                                                  |
|     -u :\<nom d’utilisateur>     |                                                                                                                                                                             Spécifie le nom d’utilisateur à utiliser pour le montage du partage. Si *username* n’est pas précédé d’une barre oblique**\\**inverse (), il est traité comme un nom d’utilisateur UNIX.                                                                                                                                                                             |
|     -p :\<mot de passe>     |                                                                                                                                                                                          Mot de passe à utiliser pour le montage du partage. Si vous utilisez un astérisque (**&#42;**), vous êtes invité à entrer le mot de passe.                                                                                                                                                                                          |

> [!NOTE]
