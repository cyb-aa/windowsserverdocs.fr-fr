---
title: prnmngr
description: Découvrez comment ajouter, supprimer et répertorier les imprimantes et des connexions.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2c0aa44cc6f27e553bf8c1b57356b884bc0cd632
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887200"
---
# <a name="prnmngr"></a>prnmngr

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ajoute, supprime et répertorie les imprimantes ou les connexions d’imprimante, en plus de la configuration et affichage de l’imprimante par défaut.

## <a name="syntax"></a>Syntaxe
```
cscript Prnmngr {-a | -d | -x | -g | -t | -l | -?}[c] [-s <ServerName>] 
[-p <printerName>] [-m <printermodel>] [-r <PortName>] [-u <UserName>] 
[-w <Password>]
```

## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|-a|Ajoute une connexion d’imprimante locale.|
|-d|Supprime une connexion d’imprimante.|
|-x|Supprime toutes les imprimantes à partir du serveur spécifié avec le **-s** paramètre. Si vous ne spécifiez pas un serveur, Windows supprime toutes les imprimantes sur l’ordinateur local.|
|-g|Affiche l’imprimante par défaut.|
|-t|Définit l’imprimante par défaut sur l’imprimante spécifiée par le **-p** paramètre.|
|-l|Répertorie toutes les imprimantes installées sur le serveur spécifié par le **-s** paramètre. Si vous ne spécifiez pas un serveur, Windows répertorie les imprimantes installées sur l’ordinateur local.|
|c|Spécifie que le paramètre s’applique aux connexions d’imprimante. Peut être utilisé avec le **-** et **- x** paramètres.|
|-s <ServerName>|Spécifie le nom de l’ordinateur distant qui héberge l’imprimante que vous souhaitez gérer. Si vous ne spécifiez pas un ordinateur, l’ordinateur local est utilisé.|
|-p \<printerName>|Spécifie le nom de l’imprimante que vous souhaitez gérer.|
|-m \<DrivermodelName>|Spécifie le pilote que vous souhaitez installer (par nom). Pilotes sont souvent nommées pour le modèle d’imprimante que prises en charge. Consultez la documentation de l’imprimante pour plus d’informations.|
|-r \<PortName>|Spécifie le port auquel l’imprimante est connectée. S’il s’agit d’une action parallèle ou un port série, utilisez l’ID du port (par exemple, LPT1 : ou COM1 :). S’il s’agit d’un port TCP/IP, utilisez le nom de port spécifié lors de l’ajout du port.|
|-u \<nom d’utilisateur > -w \<mot de passe >|Spécifie un compte disposant d’autorisations pour se connecter à l’ordinateur qui héberge l’imprimante que vous souhaitez gérer. Tous les membres du groupe Administrateurs local de l’ordinateur cible disposent de ces autorisations, mais les autorisations peuvent également être accordées aux autres utilisateurs. Si vous ne spécifiez pas un compte, vous devez être connecté sous un compte disposant de ces autorisations pour la commande fonctionne.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes
-   Le **prndrvr** commande est un script Visual Basic situé dans le %WINdir%\System32\printing_Admin_Scripts\\ <language> directory. Pour utiliser cette commande, à une invite de commandes, tapez **cscript** suivi par le chemin complet vers le **prnmngr** fichier, ou accédez au dossier approprié. Exemple :
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnmngr
    ```
-   Si les informations que vous fournissez contient des espaces, utilisez des guillemets autour du texte (par exemple, `"computer Name"`).

## <a name="BKMK_examples"></a>Exemples
Pour ajouter une imprimante appelée ImprimanteCouleur_2 qui est connecté à LPT1 sur l’ordinateur local et qui nécessite un pilote d’imprimante appelé color printer Driver1, tapez :
```
cscript prnmngr -a -p colorprinter_2 -m "color printer Driver1" -r lpt1:
```
Pour supprimer l’imprimante appelée ImprimanteCouleur_2 à partir de l’ordinateur distant appelé HRServeur, tapez :
```
cscript prnmngr -d -s HRServer -p colorprinter_2 
```

#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[référence des commandes d’impression](print-command-reference.md)
