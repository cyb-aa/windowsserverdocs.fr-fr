---
title: prnqctl
description: Imprimez une page de test, interrompez ou reprenez une imprimante.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8df9dfa7-984c-4276-bb7d-e7675e7c399e jpjofre
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 377b448eb74f0a27228d8502ce149fb04869895b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722802"
---
# <a name="prnqctl"></a>prnqctl

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imprime une page de test, interrompt ou reprend une imprimante et efface une file d’attente d’impression.  

## <a name="syntax"></a>Syntaxe  
```  
cscript Prnqctl {-z | -m | -e | -x | -?} [-s <ServerName>]   
[-p <printerName>] [-u <UserName>] [-w <Password>]  
```  
### <a name="parameters"></a>Paramètres  

|Paramètre|Description|  
|-------|--------|  
|-Z|suspend l’impression sur l’imprimante spécifiée avec le paramètre **-p** .|  
|-M|Reprend l’impression sur l’imprimante spécifiée avec le paramètre **-p** .|  
|-E|imprime une page de test sur l’imprimante spécifiée avec le paramètre **-p** .|  
|-X|Annule tous les travaux d’impression sur l’imprimante spécifiée avec le paramètre **-p** .|  
|-s \<nom_serveur>|Spécifie le nom de l’ordinateur distant qui héberge l’imprimante que vous souhaitez gérer. Si vous ne spécifiez pas d’ordinateur, l’ordinateur local est utilisé.|  
|-p \<nom_imprimante>|Spécifie le nom de l’imprimante que vous souhaitez gérer. Obligatoire.|  
|-u \<nom_utilisateur>-w \<mot de passe>|Spécifie un compte disposant des autorisations nécessaires pour se connecter à l’ordinateur qui héberge l’imprimante que vous souhaitez gérer. Tous les membres du groupe Administrateurs local de l’ordinateur cible disposent de ces autorisations, mais les autorisations peuvent également être accordées à d’autres utilisateurs. Si vous ne spécifiez pas de compte, vous devez avoir ouvert une session sous un compte disposant de ces autorisations pour que la commande fonctionne.|  
|/?|Affiche l'aide à l'invite de commandes.|  

## <a name="remarks"></a>Notes   
- La commande **prnqctl** est un script Visual Basic situé dans le répertoire%windir%\system32\\\ <language> printing_Admin_Scripts. Pour utiliser cette commande, à l’invite de commandes, tapez **cscript** suivi du chemin d’accès complet au fichier prnqctl ou accédez au dossier approprié. Par exemple :  
  ```  
  cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnqctl  
  ```  
- Si les informations que vous fournissez contiennent des espaces, utilisez des guillemets autour du texte ( `"computer Name"`par exemple,).  

## <a name="examples"></a><a name="BKMK_examples"></a>Illustre  
Pour imprimer une page de test sur l’imprimante Laserprinter1 partagée par \\l’ordinateur \Server1, tapez :  
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

## <a name="additional-references"></a>Références supplémentaires  
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
[imprimer la référence des commandes](print-command-reference.md)  
