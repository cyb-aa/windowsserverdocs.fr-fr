---
title: CHKDSK
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 62912a3c-d2cc-4ef6-9679-43709a286035
author: coreyp-at-msft
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: 9c5272ba0a5ff7c0a30f61631bb6c8dac6552ef0
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192609"
---
# <a name="chkdsk"></a>CHKDSK



Vérifie le système de fichiers et les métadonnées de système de fichiers d’un volume pour les erreurs logiques et physiques. Si utilisée sans paramètres, **chkdsk** affiche uniquement l’état du volume et de ne pas corriger les éventuelles erreurs. Si utilisé avec le **/f**, **/r**, **/x**, ou **/b** paramètres, il corrige les erreurs sur le volume.

> [!IMPORTANT]
> L’appartenance au local **administrateurs** , ou équivalent, est la condition minimale requise pour exécuter **chkdsk**. Pour ouvrir une fenêtre d’invite de commandes en tant qu’administrateur, avec le bouton droit **invite de commandes** dans le menu Démarrer, puis cliquez sur **exécuter en tant qu’administrateur**.

> [!IMPORTANT]
> Interrompre **chkdsk** n’est pas recommandée. Toutefois, l’annulation ou interrompre **chkdsk** ne doivent pas quitter le volume endommagé de plus qu’auparavant **chkdsk** a été exécuté. Réexécuter **chkdsk** vérifie et répare les éventuelles corruptions présentes restante sur le volume.

> [!IMPORTANT]
> **Remarque :** CHKDSK peut être utilisé uniquement pour les disques locaux. La commande ne peut pas être utilisée par une lettre de lecteur local qui a été redirigée sur le réseau.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#examples).

##<a name="syntax"></a>Syntaxe

```
chkdsk [<Volume>[[<Path>]<FileName>]] [/f] [/v] [/r] [/x] [/i] [/c] [/l[:<Size>]] [/b]  

```

##<a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Volume>|Spécifie la lettre de lecteur (suivie d’un signe deux-points), point de montage ou nom de volume.|
|[\<Path>]<FileName>|Utiliser avec la table d’allocation de fichiers (FAT) et FAT32 uniquement. Spécifie l’emplacement et le nom d’un fichier ou un ensemble de fichiers que vous souhaitez **chkdsk** pour vérifier la fragmentation. Vous pouvez utiliser le **?** et **&#42;** des caractères génériques pour spécifier plusieurs fichiers.|
|/f|Corrige les erreurs sur le disque. Le disque doit être verrouillé. Si **chkdsk** ne peut pas verrouiller le lecteur, un message s’affiche vous demandant si vous souhaitez vérifier le lecteur de la prochaine fois que vous redémarrez l’ordinateur.|
|/v|Affiche le nom de chaque fichier dans chaque répertoire comme le disque est vérifié.|
|/r|Recherche les secteurs défectueux et récupère les informations lisibles. Le disque doit être verrouillé. **/r** inclut les fonctionnalités de **/f**, avec l’analyse supplémentaire des erreurs de disque physique.|
|/x|Force le volume à démonter tout d’abord, si nécessaire. Tous les descripteurs ouverts sur le lecteur sont invalidés. **/x** inclut également les fonctionnalités de **/f**.|
|/i|Utilisez avec NTFS uniquement. Effectue une vérification moins rigoureuse des entrées d’index, ce qui réduit la quantité de temps nécessaire pour exécuter **chkdsk**.|
|/c|Utilisez avec NTFS uniquement. Ne vérifie pas les cycles au sein de la structure de dossiers, ce qui réduit la quantité de temps nécessaire pour exécuter **chkdsk**.|
|/l [ :\<taille >]|Utilisez avec NTFS uniquement. Modifie la taille du fichier journal à la taille que vous tapez. Si vous omettez le paramètre de taille, **/l** affiche la taille actuelle.|
|/b|NTFS uniquement : Efface la liste de clusters défectueux sur le volume et analysera tous les clusters alloués et gratuits pour les erreurs. **/b** inclut les fonctionnalités de **/r**. Utilisez ce paramètre après la création d’images d’un volume sur un nouveau lecteur de disque dur.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Contrôles de volume est ignoré

    Le **/i** ou **/c** commutateur réduit la quantité de temps nécessaire pour exécuter **chkdsk** en ignorant certains volume vérifie.
-   Vérification d’un lecteur verrouillé au redémarrage

    Si vous souhaitez **chkdsk** pour corriger les erreurs de disque, vous ne pouvez pas les fichiers ouverts sur le lecteur. Si les fichiers sont ouverts, le message d’erreur suivant s’affiche :  
    ```
    Chkdsk cannot run because the volume is in use by another process. Would you like to schedule this volume to be checked the next time the system restarts? (Y/N)  
    
    ```  
    Si vous choisissez de vérifier le lecteur lors du prochain redémarrage de l’ordinateur, **chkdsk** vérifie le lecteur et corrige automatiquement les erreurs lorsque vous redémarrez l’ordinateur. Si la partition du lecteur est une partition de démarrage, **chkdsk** redémarre automatiquement l’ordinateur après avoir vérifié le lecteur.

    Vous pouvez également utiliser le **chkntfs /c** commande pour planifier le volume soit vérifié au prochain redémarrage de l’ordinateur. Utilisez le **ensemble dirty fsutil** commande pour définir le volume du bit modifié (indiquant une corruption), afin que Windows s’exécute **chkdsk** lorsque l’ordinateur est redémarré.
-   Rapports d’erreurs de disque

    Vous devez utiliser **chkdsk** occasionnellement sur les systèmes de fichiers FAT et NTFS pour rechercher les erreurs de disque. **CHKDSK** examine l’espace disque et l’utilisation de disque et fournit un rapport d’état spécifique à chaque système de fichiers. Le rapport d’état affiche les erreurs trouvées dans le système de fichiers. Si vous exécutez **chkdsk** sans le **/f** paramètre sur une partition active, il peut signaler des erreurs, car il ne peut pas verrouiller le lecteur.
-   Correction des erreurs de disque logique

    **CHKDSK** corrige les erreurs de disque logique uniquement si vous spécifiez le **/f** paramètre. **CHKDSK** doit être en mesure de verrouiller le lecteur pour corriger des erreurs.

    Étant donné que les réparations sur les systèmes de fichiers FAT modifient généralement la table d’allocation de fichier d’un disque et parfois entraîner une perte de données, **chkdsk** peut afficher un message de confirmation semblable à ce qui suit :  
    ```
    10 lost allocation units found in 3 chains.  
    Convert lost chains to files?  
    
    ```  
    Si vous appuyez sur **Y**, Windows enregistre chaque chaîne perdue dans le répertoire racine en tant que fichier avec un nom dans le fichier de format\<nnnn > .chk. Lorsque **chkdsk** se termine, vous pouvez vérifier ces fichiers pour voir s’ils contiennent des données que vous avez besoin. Si vous appuyez sur **N**, Windows résout le disque, mais il n’enregistre pas le contenu des unités d’allocation perdues.

    Si vous n’utilisez pas le **/f** paramètre, **chkdsk** affiche un message indiquant que le fichier doit être corrigé, mais elle ne corrige pas les erreurs.

    Si vous utilisez **chkdsk /f** sur un disque très volumineux ou un disque avec un très grand nombre de fichiers (par exemple, des millions de fichiers), **chkdsk /f** peut prendre beaucoup de temps.
-   Rechercher les erreurs de disque physique

    Utilisez le **/r** paramètre pour rechercher les erreurs de disque physique dans le système de fichiers et de tenter de récupérer des données à partir d’un affectées les secteurs de disque.
-   À l’aide de **chkdsk** avec des fichiers ouverts

    Si vous spécifiez le **/f** paramètre, **chkdsk** affiche un message d’erreur si des fichiers sont ouverts sur le disque. Si vous ne spécifiez pas le **/f** paramètre et ouvrir des fichiers existent, **chkdsk** peut signaler des unités d’allocation perdues sur le disque. Cela peut se produire si elle est ouverte fichiers n’ont pas encore été enregistrés dans la table d’allocation. Si **chkdsk** signale la perte d’un grand nombre d’unités d’allocation, envisagez de réparer le disque.
-   À l’aide de **chkdsk** avec les clichés instantanés pour dossiers partagés

    Étant donné que les clichés instantanés de volume de source de dossiers partagés ne peuvent pas être verrouillés lors de clichés instantanés de dossiers partagés est activée, en cours d’exécution **chkdsk** sur la source de volume peut-être signaler de fausses erreurs ou la **chkdsk** fermeture inattendue. Vous pouvez, toutefois, vérifier les clichés instantanés pour les erreurs en exécutant **chkdsk** en mode en lecture seule (sans paramètres) pour vérifier les clichés instantanés de volume de stockage de dossiers partagés.
-   Description des codes de sortie

    Le tableau suivant répertorie les codes de la sortie qui **chkdsk** signale la fin de son exécution.  
    |Code de sortie|Description|
    |---------|-----------|
    |0|Aucune erreur n’a été trouvé.|
    |1|Erreurs ont été trouvées et fixe.|
    |2|Effectué le nettoyage de disque (par exemple, le garbage collection) ou n’a pas effectué le nettoyage car **/f** n’était pas spécifié.|
    |3|Ne peut pas vérifier le disque, les erreurs ne peuvent pas être résolus ou erreurs n’ont pas été résolus, car **/f** n’était pas spécifié.|
-   Le **chkdsk** commande, avec des paramètres différents, est disponible à partir de la Console de récupération.
-   Sur les serveurs qui sont rarement redémarrés, il pouvez que vous souhaitez utiliser le **chkntfs** ou **requête dirty fsutil** commandes pour déterminer si le volume's erroné bit est déjà définissent avant l’exécution de chkdsk.

## <a name="examples"></a>Exemples

Si vous souhaitez vérifier le disque dans le lecteur D et demander à Windows de corriger les erreurs, tapez :
```
chkdsk d: /f  

```
Si elle rencontre des erreurs, **chkdsk** s’interrompt et affiche des messages. **CHKDSK** se termine en affichant un rapport qui répertorie l’état du disque. Impossible d’ouvrir des fichiers sur le lecteur spécifié jusqu'à ce que **chkdsk** se termine.

Pour vérifier tous les fichiers sur un disque FAT dans le répertoire actif pour les blocs non contigus, tapez :
```
chkdsk *.*  

```
**CHKDSK** affiche un rapport d’état et répertorie ensuite les fichiers qui correspondent aux spécifications de fichiers qui contiennent des blocs non contigus.
#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
