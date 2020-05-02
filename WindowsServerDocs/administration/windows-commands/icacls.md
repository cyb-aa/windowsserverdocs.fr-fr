---
title: icacls
description: Rubrique de référence pour la commande icacls, qui affiche ou modifie les listes de contrôle d’accès discrétionnaire (DACL, Discretionary Access Control Lists) sur les fichiers spécifiés et applique les listes DACL stockées aux fichiers dans les répertoires spécifiés.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 403edfcc-328a-479d-b641-80c290ccf73e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/21/2018
ms.openlocfilehash: dcf4fa9fa9205a762ead99ac4a8486ac04c23514
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724872"
---
# <a name="icacls"></a>icacls

Affiche ou modifie les listes de contrôle d’accès discrétionnaire (DACL) sur les fichiers spécifiés et applique les DACL stockées aux fichiers des répertoires spécifiés.

> [!NOTE]
> Cette commande remplace la [commande cacls](cacls.md)déconseillée.

## <a name="syntax"></a>Syntaxe

```
icacls <filename> [/grant[:r] <sid>:<perm>[...]] [/deny <sid>:<perm>[...]] [/remove[:g|:d]] <sid>[...]] [/t] [/c] [/l] [/q] [/setintegritylevel <Level>:<policy>[...]]
icacls <directory> [/substitute <sidold> <sidnew> [...]] [/restore <aclfile> [/c] [/l] [/q]]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `<filename>` | Spécifie le fichier pour lequel afficher les DACL. |
| `<directory>` | Spécifie le répertoire pour lequel afficher les DACL. |
| /t | Exécute l’opération sur tous les fichiers spécifiés dans le répertoire actif et ses sous-répertoires. |
| /C | Poursuit l’opération en dépit des erreurs de fichier. Les messages d’erreur s’affichent toujours. |
| /l | Exécute l’opération sur un lien symbolique au lieu de sa destination. |
| /q | Supprime les messages de réussite. |
| [/Save `<ACLfile>` [/t] [/c] [/l] [/q]] | Stocke des DACL pour tous les fichiers correspondants dans *ACLfile* pour une utilisation ultérieure avec **/Restore**. |
| [/SetOwner `<username>` [/t] [/c] [/l] [/q]] | Modifie le propriétaire de tous les fichiers correspondants à l’utilisateur spécifié. |
| [/findsid `<sid>` [/t] [/c] [/l] [/q]] | Recherche tous les fichiers correspondants qui contiennent une liste DACL mentionnant explicitement l’identificateur de sécurité (SID) spécifié. |
| [/verify [/t] [/c] [/l] [/q]] | Recherche tous les fichiers dont les listes de contrôle d’accès ne sont pas canoniques ou qui ont des longueurs incohérentes avec les nombres d’entrées de contrôle d’accès. |
| [/RESET [/t] [/c] [/l] [/q]] | Remplace les ACL par des ACL héritées par défaut pour tous les fichiers correspondants. |
| [/Grant [ : r] \<sid> :<perm>[...]] | Accorde les droits d’accès utilisateur spécifiés. Les autorisations remplacent les autorisations explicites précédemment accordées.<p>Ne pas ajouter le **: r**signifie que les autorisations sont ajoutées à toutes les autorisations explicites accordées précédemment. |
| [> \<sid/deny :<perm>[...]] | Refuse explicitement les droits d’accès utilisateur spécifiés. Une entrée de contrôle d’accès Deny explicite est ajoutée pour les autorisations indiquées et les mêmes autorisations dans toute attribution explicite sont supprimées. |
| [/Remove`[:g | :d]]` `<sid>`[...] commutateur commutateur /l /q | Supprime toutes les occurrences du SID spécifié de la liste DACL. Cette commande peut également utiliser :<ul><li>**: g** -supprime toutes les occurrences des droits accordés au SID spécifié.</li><li>**:d** -supprime toutes les occurrences des droits refusés au SID spécifié. |
| [/setintegritylevel [(CI) (OI)] `<Level>:<Policy>`[...]] | Ajoute explicitement une entrée du contrôle d’intégrité à tous les fichiers correspondants. Le niveau peut être spécifié comme suit :<ul><li>**l** -bas</li><li>**m**-moyen</li><li>**h** -élevé</li></ul>Les options d’héritage pour l’ACE d’intégrité peuvent précéder le niveau et s’appliquent uniquement aux répertoires. |
| [/Substitute `<sidold> <sidnew>` [...]] | Remplace un SID existant (*sidold*) par un nouveau sid (*sidnew*). Requiert l’utilisation de `<directory>` avec le paramètre. |
| /Restore `<ACLfile>` [/c] [/l] [/q] | Applique les listes DACL `<ACLfile>` stockées à partir de à des fichiers dans le répertoire spécifié. Requiert l’utilisation de `<directory>` avec le paramètre. |
| /inheritancelevel:`[e | d | r]` | Définit le niveau d’héritage, qui peut être :<ul><li>**e** -active l’héritage</li><li>**d** -désactive l’héritage et copie les ACE</li><li>**r** -supprime toutes les ACE héritées</li></ul> |

## <a name="remarks"></a>Notes 

- Les SID peuvent être sous la forme d’un nom numérique ou d’un nom convivial. Si vous utilisez une forme numérique, vous devez apposer le caractère générique **&#42;** au début du SID.

- Cette commande conserve l’ordre canonique des entrées ACE comme suit :  

    - Refus explicites

    -  Autorisations explicites

    - Refus hérité

    - Autorisations héritées

- L' `<perm>` option est un masque d’autorisation qui peut être spécifié sous l’une des formes suivantes :

    - Une séquence de droits simples :

      - **F** -accès complet

      - **M**-modifier l’accès

      - **RX** -lecture et exécution d’accès

      - **R** -accès en lecture seule

      - **W** -accès en écriture seule

    - Liste séparée par des virgules entre parenthèses de droits spécifiques :

      - **D** -supprimer

      - **RC** -contrôle de lecture

      - **WDac** -écriture DAC

      - Propriétaire de l’écriture **WO**

      - **S** -synchroniser

      - Sécurité du système d’accès **en tant que**

      - **Ma** -maximum autorisé

      - **GR** -lecture générique

      - **GW** -écriture générique

      - **GE** -exécution générique

      - **GA** -générique tout

      - **Bureau à distance** -lire le répertoire de données/liste

      - **WD** -écrire des données/ajouter un fichier

      - **Ad** -ajouter des données/ajouter un sous-répertoire

      - **Rea** -lire les attributs étendus

      - **WEA** -écrire des attributs étendus

      - **X** -exécuter/parcourir

      - **DC** -supprimer un enfant

      - **RA** -attributs de lecture

      - **Wa** -attributs d’écriture

  - Les droits d’héritage peuvent `<perm>` précéder l’une ou l’autre des formes, et elles s’appliquent uniquement aux répertoires :

      - **(OI)** -l’objet hérite

      - **(Ci)** -conteneur Inherit

      - **(E/s)** -hériter uniquement

      - **(NP)** -ne pas propager l’héritage

## <a name="examples"></a>Exemples

Pour enregistrer les listes DACL pour tous les fichiers du répertoire C:\Windows et de ses sous-répertoires dans le fichier ACLFile, tapez :

```
icacls c:\windows\* /save aclfile /t
```

Pour restaurer les listes DACL pour chaque fichier dans ACLFile qui existe dans le répertoire C:\Windows et ses sous-répertoires, tapez :

```
icacls c:\windows\ /restore aclfile
```

Pour accorder à l’utilisateur User1 les autorisations de suppression et d’écriture DAC dans un fichier nommé Test1, tapez :

```
icacls test1 /grant User1:(d,wdac)
```

Pour accorder à l’utilisateur les autorisations S-1-1-0 supprimer et écrire DAC dans un fichier nommé test2, tapez :

```
icacls test2 /grant *S-1-1-0:(d,wdac)
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
