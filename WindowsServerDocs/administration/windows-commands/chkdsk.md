---
title: CHKDSK
description: Rubrique de référence pour la commande CHKDSK, qui vérifie les métadonnées du système de fichiers et du système de fichiers d’un volume pour les erreurs logiques et physiques.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 62912a3c-d2cc-4ef6-9679-43709a286035
author: jasongerend
ms.author: jgerend
manager: lizapo
ms.date: 10/09/2019
ms.openlocfilehash: 4843624337e031f81453def78e4df97bdbfe821e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82714407"
---
# <a name="chkdsk"></a>CHKDSK

Vérifie les métadonnées du système de fichiers et du système de fichiers d’un volume pour les erreurs logiques et physiques. En cas d’utilisation sans paramètre, **chkdsk** affiche uniquement l’état du volume et ne corrige aucune erreur. S’il est utilisé avec les paramètres **/f**, **/r**, **/x**ou **/b** , il corrige les erreurs sur le volume.

> [!IMPORTANT]
> L’appartenance au groupe **administrateurs** local, ou équivalent, est la condition minimale requise pour exécuter **chkdsk**. Pour ouvrir une fenêtre d’invite de commandes en tant qu’administrateur, cliquez avec le bouton droit sur **invite de commandes** dans le menu **Démarrer** , puis cliquez sur **exécuter en tant qu’administrateur**.

> [!IMPORTANT]
> L’interruption de **chkdsk** n’est pas recommandée. Toutefois, l’annulation ou l’interruption de **chkdsk** ne doit pas empêcher le volume d’être endommagé avant l’exécution de **chkdsk** . L’exécution de **chkdsk** vérifie et doit réparer toute altération restante sur le volume.

> [!NOTE]
> Chkdsk ne peut être utilisé que pour les disques locaux. La commande ne peut pas être utilisée avec une lettre de lecteur local qui a été redirigée sur le réseau.

## <a name="syntax"></a>Syntaxe

```
chkdsk [<volume>[[<path>]<filename>]] [/f] [/v] [/r] [/x] [/i] [/c] [/l[:<size>]] [/b]  
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `<volume>` | Spécifie la lettre de lecteur (suivie d’un signe deux-points), d’un point de montage ou d’un nom de volume. |
| [ `[<path>]<filename>` | À utiliser avec la table d’allocation des fichiers (FAT) et FAT32 uniquement. Spécifie l’emplacement et le nom d’un fichier ou d’un ensemble de fichiers dont vous souhaitez que **chkdsk** vérifie la fragmentation. Vous pouvez utiliser l' **?** et **&#42;** caractères génériques pour spécifier plusieurs fichiers. |
| /f | Corrige les erreurs sur le disque. Le disque doit être verrouillé. Si **chkdsk** ne peut pas verrouiller le lecteur, un message s’affiche vous demandant si vous souhaitez vérifier le lecteur la prochaine fois que vous redémarrez l’ordinateur. |
| /v | Affiche le nom de chaque fichier dans chaque répertoire lors de la vérification du disque. |
| /r | Localise les secteurs défectueux et récupère des informations lisibles. Le disque doit être verrouillé. **/r** comprend les fonctionnalités de l' **/f**, avec l’analyse supplémentaire des erreurs de disque physique. |
| /x | Force le démontage du volume en premier, si nécessaire. Tous les descripteurs ouverts sur le lecteur sont invalidés. **/x** comprend également les fonctionnalités de l' **/f**.  |
| /i | À utiliser avec NTFS uniquement. Effectue une vérification moins énergique des entrées d’index, ce qui réduit le temps nécessaire à l’exécution de **chkdsk**. |
| /C | À utiliser avec NTFS uniquement. Ne vérifie pas les cycles au sein de la structure de dossiers, ce qui réduit le temps nécessaire à l’exécution de **chkdsk**.  |
| /l [ :`<size>`] | À utiliser avec NTFS uniquement. Remplace la taille du fichier journal par la taille que vous avez tapée. Si vous omettez le paramètre de taille, **/l** affiche la taille actuelle. |
| /b | À utiliser avec NTFS uniquement. Efface la liste des clusters défectueux sur le volume et rerecherche les erreurs dans tous les clusters alloués et libres. **/b** comprend les fonctionnalités de **/r**. Utilisez ce paramètre après avoir Imaging un volume sur un nouveau disque dur. |
| /Scan | À utiliser avec NTFS uniquement. Exécute une analyse en ligne sur le volume. |
| /forceofflinefix | Utilisez avec le système de fichiers NTFS uniquement (doit être utilisé avec **/Scan**). Ignorer toute réparation en ligne ; tous les défauts détectés sont mis en file d’attente pour une `chkdsk /spotfix`réparation hors connexion (par exemple,). |
| /perf | Utilisez avec le système de fichiers NTFS uniquement (doit être utilisé avec **/Scan**). Utilise davantage de ressources système pour effectuer une analyse aussi rapidement que possible. Cela peut avoir un impact négatif sur les performances sur les autres tâches qui s’exécutent sur le système. |
| /spotfix | À utiliser avec NTFS uniquement. Exécute la résolution des points sur le volume. |
| /sdcleanup | À utiliser avec NTFS uniquement. Récupérez les données inutiles du descripteur de sécurité (implique **/f**). |
| /offlinescanandfix | Exécute une analyse hors connexion et un correctif sur le volume. |
| /freeorphanedchains | À utiliser avec FAT/FAT32/exFAT uniquement. Libère toutes les chaînes de cluster orphelines au lieu de récupérer leur contenu. |
| /markclean | À utiliser avec FAT/FAT32/exFAT uniquement. Marque le nettoyage du volume si aucune altération n’a été détectée, même si **/f** n’a pas été spécifié. |
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="remarks"></a>Notes 

- Le commutateur **/i** ou **/c** réduit la durée nécessaire à l’exécution de **chkdsk** en ignorant certaines vérifications de volume.

- Si vous souhaitez que **chkdsk** corrige les erreurs de disque, vous ne pouvez pas avoir des fichiers ouverts sur le lecteur. Si des fichiers sont ouverts, le message d’erreur suivant s’affiche :

  ```
  Chkdsk cannot run because the volume is in use by another process. Would you like to schedule this volume to be checked the next time the system restarts? (Y/N)  
  ```

- Si vous choisissez de vérifier le lecteur la prochaine fois que vous redémarrez l’ordinateur, **chkdsk** vérifie le lecteur et corrige les erreurs automatiquement lorsque vous redémarrez l’ordinateur. Si la partition de lecteur est une partition de démarrage, **chkdsk** redémarre automatiquement l’ordinateur après avoir vérifié le lecteur.

- Vous pouvez également utiliser la `chkntfs /c` commande pour planifier le volume à vérifier lors du prochain redémarrage de l’ordinateur. Utilisez la `fsutil dirty set` commande pour définir le bit d’intégrité du volume (indiquant une altération), afin que Windows exécute **chkdsk** lorsque l’ordinateur est redémarré.

- Vous devez parfois utiliser **chkdsk** sur les systèmes de fichiers FAT et NTFS pour rechercher les erreurs de disque. **Chkdsk** examine l’espace disque et l’utilisation du disque et fournit un rapport d’état spécifique à chaque système de fichiers. Le rapport d’état affiche les erreurs trouvées dans le système de fichiers. Si vous exécutez **chkdsk** sans le paramètre **/f** sur une partition active, il peut signaler de fausses erreurs, car il ne peut pas verrouiller le lecteur.

- **Chkdsk** corrige les erreurs de disque logique uniquement si vous spécifiez le paramètre **/f** . **Chkdsk** doit pouvoir verrouiller le lecteur pour corriger les erreurs.

  Étant donné que les réparations sur les systèmes de fichiers FAT modifient généralement la table d’allocation des fichiers d’un disque et provoquent parfois une perte de données, **chkdsk** peut afficher un message de confirmation similaire à ce qui suit :

  ```
  10 lost allocation units found in 3 chains.  
  Convert lost chains to files?  
  ```

    - Si vous appuyez sur **o**, Windows enregistre chaque chaîne perdue dans le répertoire racine sous la forme d’un fichier dont le nom`<nnnn>`est au format fichier. chk. À la fin de l’exécution de **chkdsk** , vous pouvez vérifier ces fichiers pour voir s’ils contiennent des données dont vous avez besoin.

    - Si vous appuyez sur **N**, Windows corrige le disque, mais il n’enregistre pas le contenu des unités d’allocation perdues.

- Si vous n’utilisez pas le paramètre **/f** , **chkdsk** affiche un message indiquant que le fichier doit être corrigé, mais ne corrige aucune erreur.

- Si vous utilisez `chkdsk /f*` sur un disque très volumineux ou sur un disque contenant un très grand nombre de fichiers (par exemple, des millions de fichiers `chkdsk /f` ), l’exécution peut prendre beaucoup de temps.

- Utilisez le paramètre **/r** pour rechercher les erreurs de disque physique dans le système de fichiers et tenter de récupérer des données à partir des secteurs de disque affectés.

- Si vous spécifiez le paramètre **/f** , **chkdsk** affiche un message d’erreur s’il existe des fichiers ouverts sur le disque. Si vous ne spécifiez pas le paramètre **/f** et que les fichiers ouverts existent, **chkdsk** risque de signaler des unités d’allocation perdues sur le disque. Cela peut se produire si des fichiers ouverts n’ont pas encore été enregistrés dans la table d’allocation de fichiers. Si **chkdsk** signale la perte d’un grand nombre d’unités d’allocation, envisagez de réparer le disque.

- Étant donné que le volume source clichés instantanés pour dossiers partagés ne peut pas être verrouillé pendant que **clichés instantanés pour dossiers partagés** est activé, l’exécution de **chkdsk** sur le volume source peut signaler des erreurs erronées ou provoquer la fermeture inattendue de **chkdsk** . Toutefois, vous pouvez vérifier les clichés instantanés pour les erreurs en exécutant **chkdsk** en mode lecture seule (sans paramètres) pour vérifier le volume de stockage clichés instantanés pour dossiers partagés.

- La commande **chkdsk** , avec des paramètres différents, est disponible à partir de la console de récupération.

- Sur les serveurs qui ne sont pas redémarrés fréquemment, vous pouvez utiliser les **chkntfs** `fsutil dirty query` commandes chkntfs ou pour déterminer si le bit d’intégrité du volume est déjà défini avant l’exécution de Chkdsk.

### <a name="understanding-exit-codes"></a>Comprendre les codes de sortie

Le tableau suivant répertorie les codes de sortie que **chkdsk** signale une fois qu’il est terminé.  

  | Code de sortie | Description |
  | --------- | ----------- |
  | 0 | Aucune erreur n’a été trouvée. |
  | 1 | Des erreurs ont été trouvées et corrigées. |
  | 2 | Le nettoyage de disque (par exemple, garbage collection) ou le nettoyage n’a pas été effectué, car **/f** n’a pas été spécifié. |
  | 3 | Impossible de vérifier le disque, des erreurs n’ont pas pu être corrigées ou des erreurs n’ont pas été corrigées, car **/f** n’a pas été spécifié. |

## <a name="examples"></a>Exemples

Pour vérifier le disque dans le lecteur D et résoudre les erreurs de correction de Windows, tapez :

```
chkdsk d: /f  
```

Si elle rencontre des erreurs, **chkdsk** s’interrompt et affiche des messages. **Chkdsk** se termine en affichant un rapport qui indique l’état du disque. Vous ne pouvez pas ouvrir de fichiers sur le lecteur spécifié tant que **chkdsk** n’est pas terminé.

Pour vérifier tous les fichiers sur un disque FAT dans le répertoire actif pour les blocs non contigus, tapez :

```
chkdsk *.*  
```

**Chkdsk** affiche un rapport d’État, puis répertorie les fichiers qui correspondent aux spécifications de fichier qui ont des blocs non contigus.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
