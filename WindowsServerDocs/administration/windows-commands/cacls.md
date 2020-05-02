---
title: cacls
description: Rubrique de référence pour la commande cacls, qui affiche ou modifie les listes de contrôle d’accès discrétionnaire (DACL) sur les fichiers spécifiés.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b5bdbaaa-4557-48b8-80df-e75ee0d2f27d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d827199ea319bd41511f9abadfde8c6e8949976e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82726021"
---
# <a name="cacls"></a>cacls

>[!IMPORTANT]
> Cette commande est dépréciée. Utilisez [icacls](icacls.md) à la place.  

Affiche ou modifie les listes de contrôle d’accès discrétionnaire (DACL, Discretionary Access Control List) sur les fichiers spécifiés.  

## <a name="syntax"></a>Syntaxe

```  
cacls <filename> [/t] [/m] [/l] [/s[:sddl]] [/e] [/c] [/g user:<perm>] [/r user [...]] [/p user:<perm> [...]] [/d user [...]]  
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `<filename>` | Obligatoire. Affiche les listes de contrôle d’accès des fichiers spécifiés. |
| /t | Modifie les listes de contrôle d’accès des fichiers spécifiés dans le répertoire actif et tous les sous-répertoires. |
| /m | Modifie les listes de contrôle d’accès des volumes montés dans un répertoire. |
| /l | Fonctionne sur le lien symbolique lui-même au lieu de la cible. |
| /s : SDDL | Remplace les listes de contrôle d’accès par celles spécifiées dans la chaîne SDDL. Ce paramètre n’est pas valide pour une utilisation avec les paramètres **/e**, **/g**, **/r**, **/p**ou **/d** . |
| /e | Modifiez une liste de contrôle d’accès au lieu de la remplacer. |
| /C | Continuer après des erreurs d’accès refusé. |
| `/g user:<perm>` | Octroie les droits d’accès utilisateur spécifiés, y compris les valeurs valides pour l’autorisation :<ul><li>**n** -aucune</li><li>**r** -lecture</li><li>**w** -écriture</li><li>**c** -modifier (écriture)</li><li>**f** -contrôle total</li></ul> |
| /r utilisateur [...] | Révoquer les droits d’accès de l’utilisateur spécifié. Valide uniquement lorsqu’il est utilisé avec le paramètre **/e** . |
| `[/p user:<perm> [...]` | Remplacer les droits d’accès de l’utilisateur spécifié, y compris les valeurs valides pour l’autorisation :<ul><li>**n** -aucune</li><li>**r** -lecture</li><li>**w** -écriture</li><li>**c** -modifier (écriture)</li><li>**f** -contrôle total</li></ul> |
| [/d utilisateur [...] | Refuser l’accès utilisateur spécifié. |
| /? | Affiche l'aide à l'invite de commandes. |

#### <a name="sample-output"></a>Exemple de sortie

| Output | L’entrée de contrôle d’accès (ACE) s’applique à |
-------- | ------------------------------------- |
| OI | Héritage de l’objet. Ce dossier et ces fichiers. |
| CI | Conteneur Inherit. Ce dossier et ses sous-dossiers. |
| IO | Hériter uniquement. L’entrée du contrôle d’accès ne s’applique pas au fichier/répertoire actif. |
| Aucun message de sortie | Ce dossier uniquement. |
| OI CI | Ce dossier, ses sous-dossiers et ses fichiers. |
| OI CI ENTRÉES | Sous-dossiers et fichiers uniquement. |
| CI ENTRÉES | Sous-dossiers uniquement. |
| OI ENTRÉES | Fichiers uniquement. |

#### <a name="remarks"></a>Notes 

- Vous pouvez utiliser des caractères génériques (**?** et **&#42;**) pour spécifier plusieurs fichiers.

- Vous pouvez spécifier plusieurs utilisateurs.  

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [icacls](icacls.md)
