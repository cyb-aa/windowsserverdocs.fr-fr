---
title: cacls
description: La rubrique commandes Windows pour cacls, qui affiche ou modifie les listes de contrôle d’accès discrétionnaire (DACL) sur les fichiers spécifiés.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b5bdbaaa-4557-48b8-80df-e75ee0d2f27d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8cc6300563880a587b6544ec8cb61e9010ad6c2c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848242"
---
# <a name="cacls"></a>cacls

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche ou modifie les listes de contrôle d’accès discrétionnaire (DACL, Discretionary Access Control List) sur les fichiers spécifiés.  

## <a name="syntax"></a>Syntaxe  
```  
cacls <filename> [/t] [/m] [/l] [/s[:sddl]] [/e] [/c] [/g user:<perm>] [/r user [...]] [/p user:<perm> [...]] [/d user [...]]  
```  
#### <a name="parameters"></a>Paramètres  

|        Paramètre        |                                                                                            Description                                                                                             |
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      nom de fichier \<\>       |                                                                            Obligatoire. Affiche les listes de contrôle d’accès des fichiers spécifiés.                                                                             |
|           /t            |                                                          modifie les listes de contrôle d’accès des fichiers spécifiés dans le répertoire actif et tous les sous-répertoires.                                                          |
|           /m            |                                                                          modifie les listes de contrôle d’accès des volumes montés dans un répertoire.                                                                           |
|           /l            |                                                                        Travaillez sur le lien symbolique lui-même et sur la cible.                                                                         |
|         /s : SDDL         |                                       remplace les listes de contrôle d’accès par celles spécifiées dans la chaîne SDDL (non valide avec **/e**, **/g**, **/r**, **/p**ou **/d**).                                        |
|           /e            |                                                                                 Modifiez l’ACL au lieu de le remplacer.                                                                                  |
|           /c            |                                                                                 Continuer en cas d’erreurs de refus d’accès.                                                                                  |
|    /g utilisateur :\<Perm\>     |   Accordez les droits d’accès utilisateur spécifiés.<p>Valeurs valides pour l’autorisation :<p>-n-aucune<br />-r-lecture<br />-w-Write<br />-c-Modifier (écriture)<br />-f-contrôle total   |
|      /r utilisateur [...]      |                                                                  Révoquer les droits d’accès de l’utilisateur spécifié (valide uniquement avec **/e**).                                                                   |
| [/p utilisateur :\<Perm\> [...] | remplacer les droits d’accès de l’utilisateur spécifié.<p>Valeurs valides pour l’autorisation :<p>-n-aucune<br />-r-lecture<br />-w-Write<br />-c-Modifier (écriture)<br />-f-contrôle total |
|     [/d utilisateur [...]      |                                                                                    Refuser l’accès utilisateur spécifié.                                                                                     |
|           /?            |                                                                                Affiche l'aide à l'invite de commandes.                                                                                |

## <a name="remarks"></a>Notes  
- Cette commande est dépréciée. Utilisez [icacls](icacls.md) à la place.  
- Utilisez le tableau suivant pour interpréter les résultats :  


  |      Sortie       |                L’entrée de contrôle d’accès (ACE) s’applique à                |
  |-------------------|---------------------------------------------------------------------|
  |        OI         |               Héritage de l’objet. Ce dossier et ces fichiers.                |
  |        Élément de configuration         |           Conteneur Inherit. Ce dossier et ses sous-dossiers.            |
  |        E/S         | Hériter uniquement. L’entrée du contrôle d’accès ne s’applique pas au fichier/répertoire actif. |
  | Aucun message de sortie |                          Ce dossier uniquement.                          |
  |     OI CI      |                 Ce dossier, ses sous-dossiers et ses fichiers.                 |
  |   OI CI ENTRÉES    |                     Sous-dossiers et fichiers uniquement.                      |
  |     CI ENTRÉES      |                          Sous-dossiers uniquement.                           |
  |     OI ENTRÉES      |                             Fichiers uniquement.                             |


- Vous pouvez utiliser des caractères génériques ( **?** et **\\\*** ) pour spécifier plusieurs fichiers.  
- Vous pouvez spécifier plusieurs utilisateurs.  

## <a name="additional-references"></a>Références supplémentaires  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)   
-   [icacls](icacls.md)  
