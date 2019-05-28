---
title: icacls
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2639b8bb913bcd604a7c79015545006a23e1d0f2
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222948"
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
|\<FileName>|Spécifie le fichier pour lequel afficher les DACL.|
|\<Directory>|Spécifie le répertoire pour lequel afficher les DACL.|
|/t|Exécute l’opération sur tous les fichiers spécifiés dans le répertoire actif et ses sous-répertoires.|
|/c|Continue malgré les erreurs de fichier. Messages d’erreur seront affiche toujours.|
|/l|Effectue l’opération sur un lien symbolique, par rapport à sa destination.|
|/q|Supprime les messages de réussite.|
|[/ Enregistrer \<ACLfile > [/ t] [/ c] [/ l] [/ q]]|Stocke des DACL pour tous les fichiers correspondants dans *ACLfile* pour une utilisation ultérieure avec **/restauration**.|
|[/ setowner \<nom d’utilisateur > [/ t] [/ c] [/ l] [/ q]]|Modifie le propriétaire de tous les fichiers correspondants à l’utilisateur spécifié.|
|[/findSID \<Sid> [/t] [/c] [/l] [/q]]|Recherche tous les fichiers correspondants qui contiennent une liste DACL mentionner explicitement l’identificateur de sécurité spécifié (SID).|
|[/verify [/t] [/c] [/l] [/q]]|Recherche tous les fichiers avec des ACL qui n’est pas canonique ou qui ont des longueurs incohérents avec les nombres d’ACE (entrée de contrôle d’accès).|
|[/reset [/t] [/c] [/l] [/q]]|Remplace les ACL par défaut héritent des ACL pour tous les fichiers correspondants.|
|[/ accorder [ : r] \<Sid > :<Perm>[...]]|Accorde spécifié des droits d’accès utilisateur. Autorisation remplace les autorisations précédemment accordées explicites.</br>Sans **: r**, les autorisations sont ajoutées à une des autorisations explicites accordées précédemment.|
|[/ refuser \<Sid > :<Perm>[...]]|Refuse explicitement les droits d’accès utilisateur spécifié. Explicite refuser l’ACE est ajoutée pour les autorisations indiquées et les mêmes autorisations dans n’importe quel octroi explicite sont supprimées.|
|[/ supprimer [ : g\|: d]] \<Sid > [...]] / [t] [/ c] / [l] [/ q]|Supprime toutes les occurrences du SID spécifié de la liste DACL.</br>**: g** supprime toutes les occurrences de droits accordés au SID spécifié.</br>**: d** supprime toutes les occurrences de droits refusés au SID spécifié.|
|[/ setintegritylevel [(CI)(OI)]\<niveau > :<Policy>[...]]|Ajoute explicitement une ACE d’intégrité pour tous les fichiers correspondants. *Niveau* est spécifié en tant que :</br>-   **L**[ow]</br>-   **M**[edium]</br>-   **H**[MPI]</br>Options pour l’intégrité ACE d’héritage peuvent précéder le niveau et sont appliquées uniquement aux répertoires.|
|[/substitute \<SidOld> <SidNew> [...]]|Remplace un SID existant (*SidOld*) avec un nouveau SID (*SidNew*). Nécessite le *Directory* paramètre.|
|/ restauration \<ACLfile > [/ c] [/ l] [/ q]|Applique les DACL stockées à partir de *ACLfile* aux fichiers dans le répertoire spécifié. Nécessite le *Directory* paramètre.|
|/InheritanceLevel : [e\|d\|r]|Définit le niveau d’héritage : <br>  **e** -permet d’enheritance <br>**d** - désactive l’héritage et copie les ACE <br>**r** -supprime tous les héritée ACE

## <a name="remarks"></a>Notes

-   Identificateurs de sécurité peut être sous une forme numérique ou nom convivial. Si vous utilisez une forme numérique, apposer le caractère générique **&#42;** au début de l’identificateur de sécurité.
-   **icacls** conserve l’ordre canonique des entrées ACE en tant que :  
    -   Refus explicites
    -   Allocations explicites
    -   Refus hérités
    -   Accorde hérité
-   *Perm* est un masque d’autorisation qui peut être spécifié dans une des formes suivantes :  
    -   Une séquence de droits simples :

        **F** (accès complet)

        **M** (accès en modification)

        **RX** (lecture et exécution des accès)

        **R** (accès en lecture seule)

        **W** (accès en écriture seule)
    -   Une liste séparée par des virgules entre parenthèses des droits spécifiques :

        **D** (supprimer)

        **RC** (contrôle en lecture)

        **WDAC** (écriture DAC)

        **WO** (propriétaire de l’écriture)

        **S** (synchronisation)

        **AS** (accéder à la sécurité du système)

        **MA** (maximum autorisé)

        **GR** (générique lire)

        **GW** (écriture générique)

        **GE** (exécution générique)

        **GA** (générique toutes les)

        **Bureau à distance** (répertoire de données/liste de lecture)

        **WD** (écriture de fichier/ajouter des données)

        **AD** (append sous-répertoire/ajouter des données)

        **REA** (lire les attributs étendus)

        **WEA** (écrire des attributs étendus)

        **X** (exécuter/parcours)

        **Contrôleur de domaine** (supprimer l’enfant)

        **RA** (lire les attributs)

        **WA** (écrire des attributs)
-   Droits de l’héritage peuvent précéder soit *Perm* formulaire et ils sont appliqués uniquement aux répertoires :

    **(OI)** : hériter de l’objet

    **(CI)** : hériter de conteneur

    **(E/S)** : hériter uniquement

    **(NP)** : ne se propagent pas hériter

## <a name="examples"></a>Exemples

Pour enregistrer les DACL pour tous les fichiers dans le répertoire C:\Windows et ses sous-répertoires dans le fichier ACLFile, tapez :
```
icacls c:\windows\* /save aclfile /t
```
Pour restaurer les DACL pour chaque fichier dans ACLFile qui existe dans le répertoire C:\Windows et ses sous-répertoires, tapez :
```
icacls c:\windows\ /restore aclfile
```
Pour accorder des autorisations de User1 supprimer et de DAC d’écriture dans un fichier nommé « Test1 » de l’utilisateur, tapez :
```
icacls test1 /grant User1:(d,wdac)
```
Pour accorder à l’utilisateur défini par les autorisations Delete de SID S-1-1-0 et écrire la DAC dans un fichier nommé « Test2 », tapez :
```
icacls test2 /grant *S-1-1-0:(d,wdac)
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
