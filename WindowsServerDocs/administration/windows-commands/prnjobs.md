---
title: prnjobs
description: Apprenez à gérer les travaux d’impression à partir de la ligne de commande.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5ad34199-7a5a-40c1-8053-bccd5929df43
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 231b8a7a9f4f8623b3d84cc789064d256883a733
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837292"
---
# <a name="prnjobs"></a>prnjobs

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

suspend, reprend, annule et répertorie les travaux d’impression.

## <a name="syntax"></a>Syntaxe
```
cscript Prnjobs {-z | -m | -x | -l | -?} [-s <ServerName>] 
[-p <printerName>] [-j <JobID>] [-u <UserName>] [-w <Password>]
```

### <a name="parameters"></a>Paramètres

|          Paramètre           |                                                                                                                                                                                        Description                                                                                                                                                                                        |
|------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              -z              |                                                                                                                                                                 suspend le travail d’impression spécifié avec le paramètre **-j** .                                                                                                                                                                 |
|              -m              |                                                                                                                                                                Reprend le travail d’impression spécifié avec le paramètre **-j** .                                                                                                                                                                 |
|              -x              |                                                                                                                                                                Annule le travail d’impression spécifié avec le paramètre **-j** .                                                                                                                                                                 |
|              -l              |                                                                                                                                                                        répertorie tous les travaux d’impression dans une file d’attente à l’impression.                                                                                                                                                                         |
|       -s \<ServerName >       |                                                                                                                  Spécifie le nom de l’ordinateur distant qui héberge l’imprimante que vous souhaitez gérer. Si vous ne spécifiez pas d’ordinateur, l’ordinateur local est utilisé.                                                                                                                  |
|      -p \<nom_imprimante >       |                                                                                                                                                           Spécifie le nom de l’imprimante que vous souhaitez gérer. Obligatoire.                                                                                                                                                            |
|         -j \<JobID >          |                                                                                                                                                                Spécifie (par numéro d’identification) le travail d’impression que vous souhaitez annuler.                                                                                                                                                                 |
| -u \<nom_utilisateur >-w <Password> | Spécifie un compte disposant des autorisations nécessaires pour se connecter à l’ordinateur qui héberge l’imprimante que vous souhaitez gérer. Tous les membres du groupe Administrateurs local de l’ordinateur cible disposent de ces autorisations, mais les autorisations peuvent également être accordées à d’autres utilisateurs. Si vous ne spécifiez pas de compte, vous devez avoir ouvert une session sous un compte disposant de ces autorisations pour que la commande fonctionne. |
|              /?              |                                                                                                                                                                           Affiche l'aide à l'invite de commandes.                                                                                                                                                                            |

## <a name="remarks"></a>Notes
-   La commande **prnjobs** est un script Visual Basic situé dans le répertoire%windir%\system32\ printing_Admin_Scripts\\<language>. Pour utiliser cette commande, à l’invite de commandes, tapez **cscript** suivi du chemin d’accès complet au fichier prnjobs ou accédez au dossier approprié. Par exemple :
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnjobs.vbs
    ```
-   Si les informations que vous fournissez contiennent des espaces, utilisez des guillemets autour du texte (par exemple, `"computer Name"`).

## <a name="examples"></a><a name="BKMK_examples"></a>Illustre
Pour suspendre un travail d’impression avec un ID de travail 27 envoyé à l’ordinateur distant nommé HRServer pour l’impression sur l’imprimante nommée colorprinter, tapez :
```
cscript prnjobs.vbs -z -s HRServer -p colorprinter -j 27
```
Pour répertorier tous les travaux d’impression en file d’attente pour l’imprimante locale nommée colorprinter_2, tapez :
```
cscript prnjobs.vbs -l -p colorprinter_2
```

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Référence des commandes d’impression](print-command-reference.md)
