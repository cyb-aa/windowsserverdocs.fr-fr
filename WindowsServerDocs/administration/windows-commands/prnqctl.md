---
title: prnqctl
description: Imprimer une page de test, suspendre ou reprendre une imprimante.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 1ba58970e76497f6e91c53c73a429eb65a275b2f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442107"
---
# <a name="prnqctl"></a>prnqctl

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imprime une page de test s’interrompt ou reprend une imprimante et efface une file d’attente de l’imprimante.  

## <a name="syntax"></a>Syntaxe  
```  
cscript Prnqctl {-z | -m | -e | -x | -?} [-s <ServerName>]   
[-p <printerName>] [-u <UserName>] [-w <Password>]  
```  
## <a name="parameters"></a>Paramètres  

|Paramètre|Description|  
|-------|--------|  
|-z|Suspend l’impression sur l’imprimante spécifiée avec le **-p** paramètre.|  
|-m|Reprend l’impression sur l’imprimante spécifiée avec le **-p** paramètre.|  
|-e|Imprime une page de test sur l’imprimante spécifiée avec le **-p** paramètre.|  
|-x|Annule tous les travaux d’impression sur l’imprimante spécifiée avec le **-p** paramètre.|  
|s - \<nom_serveur >|Spécifie le nom de l’ordinateur distant qui héberge l’imprimante que vous souhaitez gérer. Si vous ne spécifiez pas un ordinateur, l’ordinateur local est utilisé.|  
|-p \<printerName>|Spécifie le nom de l’imprimante que vous souhaitez gérer. Obligatoire.|  
|-u \<nom d’utilisateur > -w \<mot de passe >|Spécifie un compte disposant d’autorisations pour se connecter à l’ordinateur qui héberge l’imprimante que vous souhaitez gérer. Tous les membres du groupe Administrateurs local de l’ordinateur cible disposent de ces autorisations, mais les autorisations peuvent également être accordées aux autres utilisateurs. Si vous ne spécifiez pas un compte, vous devez être connecté sous un compte disposant de ces autorisations pour la commande fonctionne.|  
|/?|Affiche l'aide à l'invite de commandes.|  

## <a name="remarks"></a>Notes  
- Le **prnqctl** commande est un script Visual Basic situé dans le %WINdir%\System32\printing_Admin_Scripts\\ <language> directory. Pour utiliser cette commande, à une invite de commandes, tapez **cscript** suivie du chemin d’accès complet au fichier de prnqctl, ou modifiez les répertoires vers le dossier approprié. Exemple :  
  ```  
  cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnqctl  
  ```  
- Si les informations que vous fournissez contient des espaces, utilisez des guillemets autour du texte (par exemple, `"computer Name"`).  

## <a name="BKMK_examples"></a>Exemples  
Pour imprimer une page de test sur l’imprimante Laserprinter1 partagé par le \\\Server1 ordinateur, tapez :  
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
[Référence des commandes d’impression](print-command-reference.md)  
