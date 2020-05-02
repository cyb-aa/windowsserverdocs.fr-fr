---
title: prnmngr
description: Découvrez comment ajouter, supprimer et répertorier des imprimantes et des connexions.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39eee1a8-4b41-4c9f-941e-486495135eb8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 0851e5c24d743a80b7edc611306df4f65338d95d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722822"
---
# <a name="prnmngr"></a>prnmngr

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ajoute, supprime et répertorie des imprimantes ou des connexions d’imprimante, en plus de définir et d’afficher l’imprimante par défaut.

## <a name="syntax"></a>Syntaxe
```
cscript Prnmngr {-a | -d | -x | -g | -t | -l | -?}[c] [-s <ServerName>] 
[-p <printerName>] [-m <printermodel>] [-r <PortName>] [-u <UserName>] 
[-w <Password>]
```

### <a name="parameters"></a>Paramètres

|           Paramètre           |                                                                                                                                                                                        Description                                                                                                                                                                                        |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              -a               |                                                                                                                                                                             Ajoute une connexion d’imprimante locale.                                                                                                                                                                              |
|              -d               |                                                                                                                                                                               supprime une connexion d’imprimante.                                                                                                                                                                               |
|              -X               |                                                                                                               supprime toutes les imprimantes du serveur spécifié avec le paramètre **-s** . Si vous ne spécifiez pas de serveur, Windows supprime toutes les imprimantes sur l’ordinateur local.                                                                                                               |
|              -g               |                                                                                                                                                                               Affiche l’imprimante par défaut.                                                                                                                                                                               |
|              -T               |                                                                                                                                                        Définit l’imprimante par défaut sur l’imprimante spécifiée par le paramètre **-p** .                                                                                                                                                         |
|              -l               |                                                                                                         répertorie toutes les imprimantes installées sur le serveur spécifié par le paramètre **-s** . Si vous ne spécifiez pas de serveur, Windows répertorie les imprimantes installées sur l’ordinateur local.                                                                                                         |
|               c               |                                                                                                                                      Spécifie que le paramètre s’applique aux connexions d’imprimante. Peut être utilisé avec les paramètres **-a** et **-x** .                                                                                                                                      |
|        -s<ServerName>        |                                                                                                                  Spécifie le nom de l’ordinateur distant qui héberge l’imprimante que vous souhaitez gérer. Si vous ne spécifiez pas d’ordinateur, l’ordinateur local est utilisé.                                                                                                                  |
|       -p \<nom_imprimante>       |                                                                                                                                                                Spécifie le nom de l’imprimante que vous souhaitez gérer.                                                                                                                                                                 |
|     -m \<DrivermodelName>     |                                                                                                          Spécifie (par nom) le pilote que vous souhaitez installer. Les pilotes sont souvent nommés pour le modèle d’imprimante pris en charge. Pour plus d’informations, consultez la documentation de l’imprimante.                                                                                                           |
|        -r \<PortName>         |                                                                         Spécifie le port sur lequel l’imprimante est connectée. S’il s’agit d’un port parallèle ou série, utilisez l’ID du port (par exemple, LPT1 : ou COM1 :). S’il s’agit d’un port TCP/IP, utilisez le nom de port qui a été spécifié lors de l’ajout du port.                                                                          |
| -u \<nom_utilisateur>-w \<mot de passe> | Spécifie un compte disposant des autorisations nécessaires pour se connecter à l’ordinateur qui héberge l’imprimante que vous souhaitez gérer. Tous les membres du groupe Administrateurs local de l’ordinateur cible disposent de ces autorisations, mais les autorisations peuvent également être accordées à d’autres utilisateurs. Si vous ne spécifiez pas de compte, vous devez avoir ouvert une session sous un compte disposant de ces autorisations pour que la commande fonctionne. |
|              /?               |                                                                                                                                                                           Affiche l'aide à l'invite de commandes.                                                                                                                                                                            |

## <a name="remarks"></a>Notes 
-   La commande **prndrvr** est un script Visual Basic situé dans le répertoire%windir%\system32\\\ <language> printing_Admin_Scripts. Pour utiliser cette commande, à l’invite de commandes, tapez **cscript** suivi du chemin d’accès complet au fichier **prnmngr** ou accédez au dossier approprié. Par exemple :
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnmngr
    ```
-   Si les informations que vous fournissez contiennent des espaces, utilisez des guillemets autour du texte ( `"computer Name"`par exemple,).

## <a name="examples"></a><a name="BKMK_examples"></a>Illustre
Pour ajouter une imprimante nommée colorprinter_2 connectée à LPT1 sur l’ordinateur local et nécessitant un pilote d’imprimante nommé Color Printer driver1, tapez :
```
cscript prnmngr -a -p colorprinter_2 -m "color printer Driver1" -r lpt1:
```
Pour supprimer l’imprimante nommée colorprinter_2 de l’ordinateur distant nommé HRServer, tapez :
```
cscript prnmngr -d -s HRServer -p colorprinter_2 
```

## <a name="additional-references"></a>Références supplémentaires
- [Référence des](command-line-syntax-key.md)
[commandes d’impression](print-command-reference.md) de la clé de syntaxe de ligne de commande
