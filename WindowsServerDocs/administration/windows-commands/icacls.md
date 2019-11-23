---
title: icacls
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 403edfcc-328a-479d-b641-80c290ccf73e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/21/2018
ms.openlocfilehash: 494c87073cfd78c7f5e17c72d4c65bec33a49b98
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375491"
---
# <a name="icacls"></a>icacls

Affiche ou modifie les listes de contrôle d’accès discrétionnaire (DACL) sur les fichiers spécifiés et applique les DACL stockées aux fichiers des répertoires spécifiés.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#examples).

## <a name="syntax"></a>Syntaxe

```
icacls <FileName> [/grant[:r] <Sid>:<Perm>[...]] [/deny <Sid>:<Perm>[...]] [/remove[:g|:d]] <Sid>[...]] [/t] [/c] [/l] [/q] [/setintegritylevel <Level>:<Policy>[...]]
icacls <Directory> [/substitute <SidOld> <SidNew> [...]] [/restore <ACLfile> [/c] [/l] [/q]]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Nom de fichier \<>|Spécifie le fichier pour lequel afficher les DACL.|
|Répertoire \<>|Spécifie le répertoire pour lequel afficher les DACL.|
|commutateur|Exécute l’opération sur tous les fichiers spécifiés dans le répertoire actif et ses sous-répertoires.|
|/c|Poursuit l’opération en dépit des erreurs de fichier. Les messages d’erreur s’affichent toujours.|
|/l|Effectue l’opération sur un lien symbolique par rapport à sa destination.|
|/q|Supprime les messages de réussite.|
|[/Save \<ACLfile > [/t] [/c] [/l] [/q]]|Stocke des DACL pour tous les fichiers correspondants dans *ACLfile* pour une utilisation ultérieure avec **/Restore**.|
|[/SetOwner \<nom d’utilisateur > [/t] [/c] [/l] [/q]]|Modifie le propriétaire de tous les fichiers correspondants à l’utilisateur spécifié.|
|[/findSID \<sid > [/t] [/c] [/l] [/q]]|Recherche tous les fichiers correspondants qui contiennent une liste DACL mentionnant explicitement l’identificateur de sécurité (SID) spécifié.|
|[/verify [/t] [/c] [/l] [/q]]|Recherche tous les fichiers dont les listes de contrôle d’accès ne sont pas canoniques ou qui ont des longueurs incohérentes avec les nombres d’entrées de contrôle d’accès.|
|[/RESET [/t] [/c] [/l] [/q]]|Remplace les ACL par des ACL héritées par défaut pour tous les fichiers correspondants.|
|[/Grant [ : r] \<sid >:<Perm>[...]]|Accorde les droits d’accès utilisateur spécifiés. Les autorisations remplacent les autorisations explicites précédemment accordées.</br>Sans **: r**, les autorisations sont ajoutées à toutes les autorisations explicites accordées précédemment.|
|[/Deny \<sid >:<Perm>[...]]|Refuse explicitement les droits d’accès utilisateur spécifiés. Une entrée de contrôle d’accès Deny explicite est ajoutée pour les autorisations indiquées et les mêmes autorisations dans toute attribution explicite sont supprimées.|
|[/Remove [ : g\|:d]] \<sid > [...]] commutateur commutateur /l /q|Supprime toutes les occurrences du SID spécifié de la liste DACL.</br>**: g** supprime toutes les occurrences des droits accordés au SID spécifié.</br>**:d** supprime toutes les occurrences des droits refusés au SID spécifié.|
|[/setintegritylevel [(CI) (OI)]\<niveau >:<Policy>[...]]|Ajoute explicitement une entrée du contrôle d’intégrité à tous les fichiers correspondants. Le *niveau* est spécifié comme suit :</br>-   **L**[in]</br>-   **M**[edium]</br>-   **H**[MPI]</br>Les options d’héritage pour l’ACE d’intégrité peuvent précéder le niveau et s’appliquent uniquement aux répertoires.|
|[/Substitute \<SidOld > <SidNew> [...]]|Remplace un SID existant (*SidOld*) par un nouveau sid (*SidNew*). Requiert le paramètre *Directory* .|
|/Restore \<ACLfile > [/c] [/l] [/q]|Applique les listes DACL stockées de *ACLfile* aux fichiers dans le répertoire spécifié. Requiert le paramètre *Directory* .|
|/InheritanceLevel : [e\|d\|r]|Définit le niveau d’héritage : <br>  **e** -active enheritance <br>**d** -désactive l’héritage et copie les ACE <br>**r** -supprime toutes les ACE héritées

## <a name="remarks"></a>Notes

-   Les SID peuvent être sous la forme d’un nom numérique ou d’un nom convivial. Si vous utilisez une forme numérique, vous devez apposer **&#42;** le caractère générique au début du SID.
-   **icacls** conserve l’ordre canonique des entrées ACE comme suit :  
    -   Refus explicites
    -   Autorisations explicites
    -   Refus hérité
    -   Autorisations héritées
-   *Perm* est un masque d’autorisation qui peut être spécifié sous l’une des formes suivantes :  
    -   Une séquence de droits simples :

        **F** (accès total)

        **M** (modifier l’accès)

        **RX** (accès en lecture et en exécution)

        **R** (accès en lecture seule)

        **W** (accès en écriture seule)
    -   Liste séparée par des virgules entre parenthèses de droits spécifiques :

        **D** (supprimer)

        **RC** (contrôle de lecture)

        **WDac** (écriture DAC)

        **WO** (écriture propriétaire)

        **S** (synchroniser)

        **As** (accès à la sécurité du système)

        **Ma** (maximum autorisé)

        **GR** (lecture générique)

        **GW** (écriture générique)

        **GE** (exécution générique)

        **GA** (générique tout)

        **Rd** (répertoire de lecture des données/listes)

        **WD** (écrire des données/ajouter un fichier)

        **Ad** (ajouter des données/ajouter un sous-répertoire)

        **Rea** (lire les attributs étendus)

        **WEA** (écrire les attributs étendus)

        **X** (Exécuter/Parcourir)

        **DC** (supprimer l’enfant)

        **RA** (attributs de lecture)

        **Wa** (attributs d’écriture)
-   Les droits d’héritage peuvent précéder une forme *Perm* et s’appliquent uniquement aux répertoires :

    **(OI)** : l’objet hérite

    **(Ci)** : conteneur Inherit

    **(E/s)** : hériter uniquement

    **(NP)** : ne pas propager l’héritage

## <a name="examples"></a>Exemples

Pour enregistrer les listes DACL pour tous les fichiers du répertoire C:\Windows et de ses sous-répertoires dans le fichier ACLFile, tapez :

```
icacls c:\windows\* /save aclfile /t
```

Pour restaurer les listes DACL pour chaque fichier dans ACLFile qui existe dans le répertoire C:\Windows et ses sous-répertoires, tapez :

```
icacls c:\windows\ /restore aclfile
```

Pour accorder à l’utilisateur User1 les autorisations de suppression et d’écriture DAC dans un fichier nommé « Test1 », tapez :

```
icacls test1 /grant User1:(d,wdac)
```

Pour accorder à l’utilisateur défini par le SID S-1-1-0 des autorisations de suppression et d’écriture de DAC dans un fichier nommé « test2 », tapez :

```
icacls test2 /grant *S-1-1-0:(d,wdac)
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
