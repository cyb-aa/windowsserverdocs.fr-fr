---
ms.assetid: faad34aa-4ba1-4129-bc1f-08088399e2fa
title: Fsutil USN
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: d17246b03de20d0bc327af801621a28f406edf63
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720079"
---
# <a name="fsutil-usn"></a>Fsutil USN
> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Gère le journal des modifications du numéro de séquence de mise à jour (USN).

## <a name="syntax"></a>Syntaxe

```
fsutil usn [createjournal] m=<MaxSize> a=<AllocationDelta> <VolumePath>
fsutil usn [deletejournal] {/D | /N} <volumepath>
fsutil usn [enablerangetracking] <volumepath> [options]
fsutil usn [enumdata] <FileRef> <LowUSN> <HighUSN> <VolumePath>
fsutil usn [queryjournal] <VolumePath>
fsutil usn [readdata] <FileName>
fsutil usn [readjournal] [c= <chunk-size> s=<file-size-threshold>] <volumepath>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------------|---------------|
|createjournal|Crée un journal des modifications USN.|
|m =\<MaxSize>|Spécifie la taille maximale, en octets, allouée par NTFS au journal des modifications.|
|a =\<AllocationDelta>|Spécifie la taille, en octets, de l’allocation de mémoire qui est ajoutée à la fin et supprimée du début du journal des modifications.|
|\<VolumePath>|Spécifie la lettre de lecteur (suivie d’un signe deux-points).|
|deletejournal|Supprime ou désactive un journal des modifications USN actif. **Attention :** La suppression du journal des modifications a un impact sur le service de réplication de fichiers (FRS) et le service d’indexation, car cela nécessiterait que ces services effectuent une analyse complète (et longue) du volume. Cela a un impact négatif sur la réplication SYSVOL de FRS et la réplication entre les remplaçants DFS et la réanalyse du volume.|
|/d|Désactive le journal des modifications USN actif et retourne le contrôle d’entrée/sortie (e/s) pendant la désactivation du journal des modifications.|
|/n|Désactive un journal des modifications USN actif et retourne le contrôle d’e/s uniquement après la désactivation du journal des modifications.|
|enablerangetracking|Active le suivi des plages d’écriture USN pour un volume.|
|c =\<taille de segment>|Spécifie la taille de segment à suivre sur un volume.|
|s =\<File-Size-Threshold>|Spécifie le seuil de taille de fichier pour le suivi de plage.|
|enumdata|Énumère et répertorie les entrées de journal des modifications entre deux limites spécifiées.|
|\<FileRef>|Spécifie la position ordinale dans les fichiers sur le volume à partir duquel l’énumération doit commencer.|
|\<LowUSN>|Spécifie la limite inférieure de la plage de valeurs USN utilisée pour filtrer les enregistrements retournés. Seuls les enregistrements dont le dernier USN du journal des modifications est compris entre ou sont égaux aux valeurs des membres *LowUSN* et *HighUSN* sont retournés.|
|\<HighUSN>|Spécifie la limite supérieure de la plage de valeurs USN utilisée pour filtrer les fichiers retournés.|
|queryjournal|Interroge les données USN d’un volume pour recueillir des informations sur le journal des modifications actuel, ses enregistrements et sa capacité.|
|ReadData|Lit les données USN d’un fichier.|
|\<Nom de fichier>|Spécifie le chemin d’accès complet au fichier, y compris le nom de fichier et l’extension, par exemple : C:\documents\filename.txt|
|à reajourner|Lit les enregistrements USN dans le journal USN.|
|Minver =\<nombre>|Version principale minimale de USN_RECORD à retourner. Valeur par défaut = 2.|
|MaxVer =\<nombre>|Version principale maximale de USN_RECORD à retourner. Valeur par défaut = 4.|
|startusn =\<numéro USN>|USN à partir duquel commencer la lecture du journal USN. Valeur par défaut = 0.|


## <a name="remarks"></a>Notes 

-   À propos du journal des modifications USN

    Le journal des modifications USN fournit un journal persistant de toutes les modifications apportées aux fichiers sur le volume. Au fur et à mesure de l’ajout, de la suppression et de la modification de fichiers, de répertoires et d’autres objets NTFS, NTFS entre des enregistrements dans le journal des modifications USN, un pour chaque volume sur l’ordinateur. Chaque enregistrement indique le type de modification et l'objet modifié. Les nouveaux enregistrements sont ajoutés à la fin du flux.

    Les programmes peuvent consulter le journal des modifications USN pour déterminer toutes les modifications apportées à un ensemble de fichiers. Le journal des modifications USN est bien plus efficace que la vérification des horodatages ou l’inscription aux notifications de fichiers. Le journal des modifications USN est activé et utilisé par le service d’indexation, le service de réplication de fichiers (FRS), les services d’installation à distance (RIS) et le stockage étendu.

-   Utilisation de **createjournal**

    Si un journal des modifications existe déjà sur un volume, le paramètre **createjournal** met à jour les paramètres *MaxSize* et *AllocationDelta* du journal des modifications. Cela vous permet d’étendre le nombre d’enregistrements qu’un journal actif gère sans avoir à le désactiver.

-   Utilisation de **m**

    Le journal des modifications peut croître plus grand que cette valeur cible, mais le journal des modifications est tronqué au prochain point de contrôle NTFS à une valeur inférieure à cette valeur. NTFS examine le journal des modifications et le supprime lorsque sa taille dépasse la valeur de *MaxSize* plus la valeur de *AllocationDelta*. Au niveau des points de contrôle NTFS, le système d’exploitation écrit des enregistrements dans le fichier journal NTFS qui permet à NTFS de déterminer le traitement requis pour récupérer suite à une défaillance.

-   Utilisation **d’un**

    Le journal des modifications peut croître jusqu’à la somme des valeurs *MaxSize* et *AllocationDelta* avant d’être tronqué.

-   Utilisation de **deletejournal**

    La suppression ou la désactivation d’un journal des modifications actif prend beaucoup de temps, car le système doit accéder à tous les enregistrements de la table de fichiers maîtres (MFT) et affecter la valeur 0 (zéro) au dernier attribut USN. Ce processus peut prendre plusieurs minutes et peut se poursuivre après le redémarrage du système, si un redémarrage est nécessaire. Pendant ce processus, le journal des modifications n’est pas considéré comme actif, ni désactivé. Si le système désactive le journal, il est inaccessible et toutes les opérations du journal retournent des erreurs. Vous devez faire attention à la désactivation d’un journal actif, car cela affecte négativement les autres applications qui utilisent le journal.

## <a name="examples"></a><a name="BKMK_examples"></a>Illustre
Pour créer un journal des modifications USN sur le lecteur C, tapez :

```
fsutil usn createjournal m=1000 a=100 c:
```

Pour supprimer un journal de modification USN actif sur le lecteur C, tapez :

```
fsutil usn deletejournal /d c:
```

Pour activer le suivi de plage avec une taille de segment et un seuil de taille de fichier spécifiés, tapez :

```
fsutil usn enablerangetracking c=16384 s=67108864 C:
```

Pour énumérer et répertorier les entrées du journal des modifications entre deux limites spécifiées sur le lecteur C, tapez :

```
fsutil usn enumdata 1 0 1 c:
```

Pour interroger les données USN d’un volume sur le lecteur C, tapez :

```
fsutil usn queryjournal c:
```

Pour lire les données USN d’un fichier dans le dossier \temp sur le lecteur C, tapez :

```
fsutil usn readdata c:\temp\sample.txt
```

Pour lire le journal USN avec un numéro USN de début spécifique, tapez :

```
fsutil usn readjournal startusn=0xF00
```

## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

[Fsutil](Fsutil.md)


