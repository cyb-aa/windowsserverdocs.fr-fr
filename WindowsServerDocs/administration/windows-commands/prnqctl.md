---
title: prnqctl
description: Imprimez une page de test, interrompez ou reprenez une imprimante.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8df9dfa7-984c-4276-bb7d-e7675e7c399e jpjofre
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 189b344dc0c4f587ba7a6382c481304242e22c74
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372040"
---
# <a name="prnqctl"></a>prnqctl

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imprime une page de test, interrompt ou reprend une imprimante et efface une file d’attente d’impression.  

## <a name="syntax"></a>Syntaxe  
```  
cscript Prnqctl {-z | -m | -e | -x | -?} [-s <ServerName>]   
[-p <printerName>] [-u <UserName>] [-w <Password>]  
```  
## <a name="parameters"></a>Paramètres  

|Paramètre|Description|  
|-------|--------|  
|-z|suspend l’impression sur l’imprimante spécifiée avec le paramètre **-p** .|  
|-m|Reprend l’impression sur l’imprimante spécifiée avec le paramètre **-p** .|  
|-e|imprime une page de test sur l’imprimante spécifiée avec le paramètre **-p** .|  
|-x|Annule tous les travaux d’impression sur l’imprimante spécifiée avec le paramètre **-p** .|  
|-s \<ServerName >|Spécifie le nom de l’ordinateur distant qui héberge l’imprimante que vous souhaitez gérer. Si vous ne spécifiez pas d’ordinateur, l’ordinateur local est utilisé.|  
|-p \<nom_imprimante >|Spécifie le nom de l’imprimante que vous souhaitez gérer. Obligatoire.|  
|-u \<nom_utilisateur >-w \<mot de passe >|Spécifie un compte disposant des autorisations nécessaires pour se connecter à l’ordinateur qui héberge l’imprimante que vous souhaitez gérer. Tous les membres du groupe Administrateurs local de l’ordinateur cible disposent de ces autorisations, mais les autorisations peuvent également être accordées à d’autres utilisateurs. Si vous ne spécifiez pas de compte, vous devez avoir ouvert une session sous un compte disposant de ces autorisations pour que la commande fonctionne.|  
|/?|Affiche l'aide à l'invite de commandes.|  

## <a name="remarks"></a>Notes  
- La commande **prnqctl** est un script Visual Basic situé dans le répertoire%windir%\system32\ printing_Admin_Scripts\\<language>. Pour utiliser cette commande, à l’invite de commandes, tapez **cscript** suivi du chemin d’accès complet au fichier prnqctl ou accédez au dossier approprié. Par exemple :  
  ```  
  cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnqctl  
  ```  
- Si les informations que vous fournissez contiennent des espaces, utilisez des guillemets autour du texte (par exemple, `"computer Name"`).  

## <a name="BKMK_examples"></a>Illustre  
Pour imprimer une page de test sur l’imprimante Laserprinter1 partagée par l’ordinateur \\\Server1, tapez :  
```  
cscript Prnqctl -e -s Server1 -p Laserprinter1  
```  
Pour suspendre l’impression sur l’imprimante Laserprinter1 sur l’ordinateur local, tapez :  
```  
cscript Prnqctl -z -p Laserprinter1  
```  
Pour annuler tous les travaux d’impression sur l’imprimante Laserprinter1 sur l’ordinateur local, tapez :  
```  
cscript Prnqctl -x -p Laserprinter1  
```  

#### <a name="additional-references"></a>Références supplémentaires  
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
[imprimer la référence des commandes](print-command-reference.md)  
