---
title: prnjobs
description: Découvrez comment gérer des travaux d’impression à partir de la ligne de commande.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5ad34199-7a5a-40c1-8053-bccd5929df43
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 03a7f0cf36539272140ea39903bab585b4bc5c72
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889580"
---
# <a name="prnjobs"></a>prnjobs

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

met en pause reprend, annule et répertorie les travaux d’impression.

## <a name="syntax"></a>Syntaxe
```
cscript Prnjobs {-z | -m | -x | -l | -?} [-s <ServerName>] 
[-p <printerName>] [-j <JobID>] [-u <UserName>] [-w <Password>]
```

## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|-z|Suspend le travail d’impression spécifié avec le **-j** paramètre.|
|-m|Reprend le travail d’impression spécifié avec le **-j** paramètre.|
|-x|Annule le travail d’impression spécifié avec le **-j** paramètre.|
|-l|Répertorie tous les travaux d’impression dans une file d’attente à l’impression.|
|s - \<nom_serveur >|Spécifie le nom de l’ordinateur distant qui héberge l’imprimante que vous souhaitez gérer. Si vous ne spécifiez pas un ordinateur, l’ordinateur local est utilisé.|
|-p \<printerName>|Spécifie le nom de l’imprimante que vous souhaitez gérer. Obligatoire.|
|-j \<JobID>|Spécifie (par numéro d’identification) pour annuler le travail d’impression.|
|-u \<nom d’utilisateur > -w <Password>|Spécifie un compte disposant d’autorisations pour se connecter à l’ordinateur qui héberge l’imprimante que vous souhaitez gérer. Tous les membres du groupe Administrateurs local de l’ordinateur cible disposent de ces autorisations, mais les autorisations peuvent également être accordées aux autres utilisateurs. Si vous ne spécifiez pas un compte, vous devez être connecté sous un compte disposant de ces autorisations pour la commande fonctionne.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes
-   Le **prnjobs** commande est un script Visual Basic situé dans le %WINdir%\System32\printing_Admin_Scripts\\ <language> directory. Pour utiliser cette commande, à une invite de commandes, tapez **cscript** suivie du chemin d’accès complet au fichier de prnjobs, ou modifiez les répertoires vers le dossier approprié. Exemple :
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnjobs
    ```
-   Si les informations que vous fournissez contient des espaces, utilisez des guillemets autour du texte (par exemple, `"computer Name"`).

## <a name="BKMK_examples"></a>Exemples
Pour suspendre un travail d’impression avec un ID de tâche de 27 envoyé à l’ordinateur distant appelé HRServeur pour l’impression sur l’imprimante nommée ImprimanteCouleur, tapez :
```
cscript prnjobs -z -s HRServer -p colorprinter -j 27
```
Pour répertorier tous les travaux d’impression en cours dans la file d’attente pour l’imprimante locale appelée ImprimanteCouleur_2, tapez :
```
cscript prnjobs -l -p colorprinter_2
```

#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[référence des commandes d’impression](print-command-reference.md)
