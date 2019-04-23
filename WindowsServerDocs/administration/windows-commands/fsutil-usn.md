---
ms.assetid: faad34aa-4ba1-4129-bc1f-08088399e2fa
title: fsutil usn
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 4bef63f4938b5ce786bbbfdbf3bdd2081280ce95
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829980"
---
# <a name="fsutil-usn"></a>fsutil usn
>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Gère le journal des modifications des nombre (USN) de séquence de mise à jour.

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

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------------|---------------|
|createjournal|Crée un journal des modifications USN.|
|m =\<MaxSize >|Spécifie la taille maximale, en octets, allouée par le journal des modifications NTFS.|
|a=\<AllocationDelta>|Spécifie la taille, en octets, d’allocation de mémoire qui est ajouté à la fin et supprimés à partir du début du journal de modification.|
|\<VolumePath>|Spécifie la lettre de lecteur (suivie d’un signe deux-points).|
|deletejournal|Supprime ou désactive un journal de modification USN actif. **Attention :** Supprimer le journal des modifications a un impact sur le Service de réplication de fichiers (FRS) et le Service d’indexation, car elle vous oblige à ces services pour effectuer une analyse complète (et du temps) du volume. À son tour avoir un impact négatif a un impact sur la réplication FRS SYSVOL et la réplication entre autres de lien DFS tandis que le volume est en cours de nouveau analysé.|
|/d|Désactive un journal de modification USN actif et renvoie le contrôle d’entrée/sortie (e/s) alors que le journal des modifications est désactivé.|
|/n|Désactive un journal de modification USN actif et retourne le contrôle de l’e/s uniquement une fois que le journal des modifications est désactivé.|
|enablerangetracking|Permet la plage d’écriture USN de suivi pour un volume.|
|c=\<chunk-size>|Spécifie la taille de segment pour effectuer le suivi sur un volume.|
|s=\<file-size-threshold>|Spécifie le seuil de taille de fichier pour la plage de suivi.|
|enumdata|Énumère et répertorie les entrées de journal de modification entre deux limites spécifiées.|
|\<FileRef>|Spécifie la position ordinale dans les fichiers sur le volume à partir duquel l’énumération doit commencer.|
|\<UsnInférieur >|Spécifie la limite inférieure des plage de valeurs USN utilisée pour filtrer les enregistrements qui sont retournés. Seuls les enregistrements dont la dernière modification journal USN est entre ou égale à la *UsnInférieur* et *UsnSupérieur* les valeurs de membre sont retournées.|
|\<UsnSupérieur >|Spécifie la limite supérieure de la plage de valeurs USN utilisée pour filtrer les fichiers qui sont retournées.|
|queryjournal|Interroge les données d’USN d’un volume pour collecter des informations sur le journal des modifications en cours, ses enregistrements et sa capacité maximale.|
|readdata|Lit les données USN d’un fichier.|
|\<FileName>|Spécifie le chemin d’accès complet au fichier, y compris le nom de fichier et l’extension par exemple : C:\documents\filename.txt|
|readjournal|Lit les enregistrements USN dans le journal USN.|
|minver=\<number>|Version majeure de la valeur minimale de USN_RECORD à retourner. Par défaut = 2.|
|maxver=\<number>|Version majeure de la valeur maximale de USN_RECORD à retourner. Par défaut = 4.|
|startusn =\<nombre des USN >|USN pour démarrer la lecture du journal USN à partir de. Par défaut = 0.|


## <a name="remarks"></a>Notes

-   Sur le journal USN

    Le journal USN fournit un enregistrement cohérent de toutes les modifications apportées aux fichiers sur le volume. Comme les fichiers, répertoires et autres objets NTFS sont ajoutés, supprimés et modifiés, NTFS entre des enregistrements dans le journal USN, un pour chaque volume sur l’ordinateur. Chaque enregistrement indique le type de modification et l'objet modifié. Nouveaux enregistrements sont ajoutés à la fin du flux.

    Les programmes peuvent consulter le journal des modifications USN pour déterminer toutes les modifications apportées à un ensemble de fichiers. Le journal USN est beaucoup plus efficace que la vérification des horodateurs ou l’inscription aux notifications de fichier. Le journal USN est activé et utilisé par le Service d’indexation, Service de réplication de fichiers (FRS), les Services d’Installation à distance (RIS) et le stockage étendu.

-   À l’aide de **createjournal**

    S’il existe déjà un journal des modifications sur un volume, le **createjournal** paramètre met à jour le journal des modifications *MaxSize* et *DeltaAllocation* paramètres. Cela vous permet d’étendre le nombre d’enregistrements qui tient à jour un journal actif sans avoir à le désactiver.

-   À l’aide de **m**

    Le journal des modifications peut dépasser cette valeur cible, mais le journal des modifications est tronqué au point de contrôle NTFS suivant à moins que cette valeur. NTFS examine le journal des modifications et réduit lorsque sa taille dépasse la valeur de *MaxSize* plus la valeur de *DeltaAllocation*. Aux points de contrôle NTFS, le système d’exploitation écrit des enregistrements dans le fichier journal NTFS qui permettent de NTFS déterminer le traitement nécessaire pour récupérer après une défaillance.

-   À l’aide de **un**

    Le journal des modifications peut croître au-delà de la somme des valeurs de *MaxSize* et *DeltaAllocation* avant d’être réduite.

-   À l’aide de **deletejournal**

    Suppression ou désactivation d’un journal de modification active est beaucoup de temps, car le système doit accéder à tous les enregistrements dans la table de fichiers principale (MFT) et le dernier attribut USN la valeur est 0 (zéro). Ce processus peut prendre plusieurs minutes, et il peut continuer après le redémarrage du système, si un redémarrage est nécessaire. Pendant ce processus, le journal des modifications n’est pas considéré comme actif, ni comme étant désactivé. Alors que le système désactive le journal, il n’est pas accessible, et toutes les opérations de journal retournent des erreurs. Vous devez utiliser la vigilance lors de la désactivation d’un journal actif, car il affecte négativement les autres applications qui sont à l’aide de la feuille.

## <a name="BKMK_examples"></a>Exemples
Pour créer un journal de modification USN sur le lecteur C, tapez :

```
fsutil usn createjournal m=1000 a=100 c:
```

Pour supprimer un actif journal de modification USN sur le lecteur C, tapez :

```
fsutil usn deletejournal /d c:
```

Pour activer la plage avec une taille de segment spécifié et le seuil de taille de fichier de suivi, tapez :

```
fsutil usn enablerangetracking c=16384 s=67108864 C:
```

Pour énumérer et répertorier les entrées de journal de modification entre deux limites spécifiées sur le lecteur C, tapez :

```
fsutil usn enumdata 1 0 1 c:
```

Pour interroger les données USN d’un volume sur le lecteur C, tapez :

```
fsutil usn queryjournal c:
```

Pour lire les données USN d’un fichier dans le dossier \Temp sur le lecteur C, tapez :

```
fsutil usn readdata c:\temp\sample.txt
```

Pour lire le journal USN avec un USN de démarrage spécifique, tapez :

```
fsutil usn readjournal startusn=0xF00
```

#### <a name="additional-references"></a>Références supplémentaires
[Clé de la syntaxe de ligne de commande](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


