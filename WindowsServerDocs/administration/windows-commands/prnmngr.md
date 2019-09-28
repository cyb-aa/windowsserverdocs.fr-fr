---
title: prnmngr
description: Découvrez comment ajouter, supprimer et répertorier des imprimantes et des connexions.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 39eee1a8-4b41-4c9f-941e-486495135eb8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 12981519a1d3bfc079a58e5883bc845955b8a8c6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372071"
---
# <a name="prnmngr"></a>prnmngr

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Ajoute, supprime et répertorie des imprimantes ou des connexions d’imprimante, en plus de définir et d’afficher l’imprimante par défaut.

## <a name="syntax"></a>Syntaxe
```
cscript Prnmngr {-a | -d | -x | -g | -t | -l | -?}[c] [-s <ServerName>] 
[-p <printerName>] [-m <printermodel>] [-r <PortName>] [-u <UserName>] 
[-w <Password>]
```

## <a name="parameters"></a>Paramètres

|           Paramètre           |                                                                                                                                                                                        Description                                                                                                                                                                                        |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              -a               |                                                                                                                                                                             Ajoute une connexion d’imprimante locale.                                                                                                                                                                              |
|              -d               |                                                                                                                                                                               supprime une connexion d’imprimante.                                                                                                                                                                               |
|              -x               |                                                                                                               supprime toutes les imprimantes du serveur spécifié avec le paramètre **-s** . Si vous ne spécifiez pas de serveur, Windows supprime toutes les imprimantes sur l’ordinateur local.                                                                                                               |
|              -g               |                                                                                                                                                                               Affiche l’imprimante par défaut.                                                                                                                                                                               |
|              -t               |                                                                                                                                                        Définit l’imprimante par défaut sur l’imprimante spécifiée par le paramètre **-p** .                                                                                                                                                         |
|              -l               |                                                                                                         répertorie toutes les imprimantes installées sur le serveur spécifié par le paramètre **-s** . Si vous ne spécifiez pas de serveur, Windows répertorie les imprimantes installées sur l’ordinateur local.                                                                                                         |
|               c               |                                                                                                                                      Spécifie que le paramètre s’applique aux connexions d’imprimante. Peut être utilisé avec les paramètres **-a** et **-x** .                                                                                                                                      |
|        -s <ServerName>        |                                                                                                                  Spécifie le nom de l’ordinateur distant qui héberge l’imprimante que vous souhaitez gérer. Si vous ne spécifiez pas d’ordinateur, l’ordinateur local est utilisé.                                                                                                                  |
|       -p @no__t 0printerName >       |                                                                                                                                                                Spécifie le nom de l’imprimante que vous souhaitez gérer.                                                                                                                                                                 |
|     -m \<DrivermodelName >     |                                                                                                          Spécifie (par nom) le pilote que vous souhaitez installer. Les pilotes sont souvent nommés pour le modèle d’imprimante pris en charge. Pour plus d’informations, consultez la documentation de l’imprimante.                                                                                                           |
|        -r @no__t 0PortName >         |                                                                         Spécifie le port sur lequel l’imprimante est connectée. S’il s’agit d’un port parallèle ou série, utilisez l’ID du port (par exemple, LPT1 : ou COM1 :). S’il s’agit d’un port TCP/IP, utilisez le nom de port qui a été spécifié lors de l’ajout du port.                                                                          |
| -u \<UserName >-w \<Password > | Spécifie un compte disposant des autorisations nécessaires pour se connecter à l’ordinateur qui héberge l’imprimante que vous souhaitez gérer. Tous les membres du groupe Administrateurs local de l’ordinateur cible disposent de ces autorisations, mais les autorisations peuvent également être accordées à d’autres utilisateurs. Si vous ne spécifiez pas de compte, vous devez avoir ouvert une session sous un compte disposant de ces autorisations pour que la commande fonctionne. |
|              /?               |                                                                                                                                                                           Affiche l'aide à l'invite de commandes.                                                                                                                                                                            |

## <a name="remarks"></a>Notes
-   La commande **prndrvr** est un script Visual Basic situé dans le répertoire%WINdir%\System32\printing_Admin_Scripts @ no__t-1 @ no__t-2. Pour utiliser cette commande, à l’invite de commandes, tapez **cscript** suivi du chemin d’accès complet au fichier **prnmngr** ou accédez au dossier approprié. Exemple :
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnmngr
    ```
-   Si les informations que vous fournissez contiennent des espaces, utilisez des guillemets autour du texte (par exemple, `"computer Name"`).

## <a name="BKMK_examples"></a>Illustre
Pour ajouter une imprimante nommée colorprinter_2 connectée à LPT1 sur l’ordinateur local et nécessitant un pilote d’imprimante nommé Color Printer driver1, tapez :
```
cscript prnmngr -a -p colorprinter_2 -m "color printer Driver1" -r lpt1:
```
Pour supprimer l’imprimante nommée colorprinter_2 de l’ordinateur distant nommé HRServer, tapez :
```
cscript prnmngr -d -s HRServer -p colorprinter_2 
```

#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[référence de commande d’impression](print-command-reference.md)
