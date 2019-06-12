---
title: cacls
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5bdbaaa-4557-48b8-80df-e75ee0d2f27d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 42f620a417f9d7bd06f779802e684e0196efc6a7
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434588"
---
# <a name="cacls"></a>cacls

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche ou modifie les listes de contrôle d’accès discrétionnaire (DACL) sur les fichiers spécifiés.  
## <a name="syntax"></a>Syntaxe  
```  
cacls <filename> [/t] [/m] [/l] [/s[:sddl]] [/e] [/c] [/g user:<perm>] [/r user [...]] [/p user:<perm> [...]] [/d user [...]]  
```  
### <a name="parameters"></a>Paramètres  

|        Paramètre        |                                                                                            Description                                                                                             |
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      \<filename\>       |                                                                            Obligatoire. Affiche les ACL des fichiers spécifiés.                                                                             |
|           /t            |                                                          Modifie les ACL des fichiers spécifiés dans le répertoire actif et tous les sous-répertoires.                                                          |
|           /m            |                                                                          modifications ACL des volumes montés dans un répertoire.                                                                           |
|           /l            |                                                                        Travailler sur le lien symbolique par rapport à la cible.                                                                         |
|         /s:sddl         |                                       remplace les ACL par celles spécifiées dans la chaîne SDDL (non valide avec **/e**, **/g**, **/r**, **/p**, ou **/d**).                                        |
|           /e            |                                                                                 modifier l’ACL au lieu de remplacer.                                                                                  |
|           /c            |                                                                                 Continuer en cas d’erreurs d’accès refusé.                                                                                  |
|    /g utilisateur :\<perm\>     |   Grant spécifié des droits d’accès utilisateur.<br /><br />Valeurs valides pour l’autorisation :<br /><br />-n - aucun<br />-r - lecture<br />-   w - write<br />modification - c - (écriture)<br />contrôle total - f-   |
|      utilisateur /r [...]      |                                                                  Révoquer les droits d’accès de l’utilisateur spécifié (valide uniquement avec **/e**).                                                                   |
| [/p utilisateur :\<perm\> [...] | Remplacez les droits d’accès de l’utilisateur spécifié.<br /><br />Valeurs valides pour l’autorisation :<br /><br />-n - aucun<br />-r - lecture<br />-   w - write<br />modification - c - (écriture)<br />contrôle total - f- |
|     /d [utilisateur [...]      |                                                                                    Refuser l’accès de l’utilisateur spécifié.                                                                                     |
|           /?            |                                                                                Affiche l'aide à l'invite de commandes.                                                                                |

## <a name="remarks"></a>Notes  
- Cette commande a été déconseillée. Utilisez [icacls](icacls.md) à la place.  
- Utilisez le tableau suivant pour interpréter les résultats :  


  |      Sortie       |                Entrée de contrôle d’accès (ACE) s’applique à                |
  |-------------------|---------------------------------------------------------------------|
  |        OI         |               Hériter de l’objet. Ce dossier et les fichiers.                |
  |        CI         |           Conteneur héritent. Ce dossier et les sous-dossiers.            |
  |        E/S         | Hériter uniquement. L’entrée du contrôle ne s’applique pas dans le fichier/répertoire actif. |
  | Aucun message de sortie |                          Ce dossier uniquement.                          |
  |     (OI)(CI)      |                 Ce dossier, sous-dossiers et fichiers.                 |
  |   (OI)(CI)(IO)    |                     Sous-dossiers et fichiers uniquement.                      |
  |     (CI)(IO)      |                          Sous-dossiers.                           |
  |     (OI)(IO)      |                             Fichiers uniquement.                             |


- Vous pouvez utiliser des caractères génériques ( **?** et **\\***) pour spécifier plusieurs fichiers.  
- Vous pouvez spécifier plusieurs utilisateurs.  

#### <a name="additional-references"></a>Références supplémentaires  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)   
-   [icacls](icacls.md)  
