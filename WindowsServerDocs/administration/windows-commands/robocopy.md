---
title: robocopy
description: Découvrez comment utiliser la commande Robocopy dans Windows et Windows Server pour copier des fichiers
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d4c6e8e9-fcb3-4a4a-9d04-2d8c367b6354
author: coreyp-at-msft
ms.author: coreyp
manager: lizapo
ms.date: 07/25/2018
ms.openlocfilehash: b814134dd8ca82a4338f80aba26c5a7dcee3b90a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384499"
---
# <a name="robocopy"></a>robocopy

Copie les données du fichier.

## <a name="syntax"></a>Syntaxe

```
robocopy <Source> <Destination> [<File>[ ...]] [<Options>]
```

## <a name="parameters"></a>Paramètres

|   Paramètre    |                                                                                            Description                                                                                             |
|----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   @no__t 0Source >    |                                                                            Spécifie le chemin d’accès au répertoire source.                                                                             |
| @no__t 0Destination > |                                                                          Spécifie le chemin d’accès au répertoire de destination.                                                                          |
|    @no__t 0File >     | Spécifie le ou les fichiers à copier. Si vous le souhaitez, vous **&#42;** pouvez utiliser des caractères génériques (ou **?** ). Si le paramètre de **fichier** n’est pas spécifié, **\*. \\** \* est utilisé comme valeur par défaut. |
|   @no__t 0Options >   |                                                                    Spécifie les options à utiliser avec la commande **Robocopy** .                                                                     |

### <a name="copy-options"></a>Options de copie

|Option|Description|
|------|-----------|
|/s|Copie les sous-répertoires. Notez que cette option exclut les répertoires vides.|
|/e|Copie les sous-répertoires. Notez que cette option comprend des répertoires vides. Pour plus d’informations, consultez la [section Notes](#remarks).|
|/Lev : \<N >|Copie uniquement les *N* premiers niveaux de l’arborescence du répertoire source.|
|z|Copie les fichiers en mode redémarrable.|
|/b|Copie les fichiers en mode de sauvegarde.|
|/ZB|Utilise le mode redémarrable. Si l’accès est refusé, cette option utilise le mode de sauvegarde.|
|/efsraw|Copie tous les fichiers chiffrés en mode EFS RAW.|
|/Copy : \<CopyFlags >|Spécifie les propriétés de fichier à copier. Les valeurs valides pour cette option sont les suivantes :</br>Données **D**</br>**Attributs**</br>Horodatages **T**</br>Liste de contrôle d’accès (ACL **) NTFS**</br>**O** -informations sur le propriétaire</br>Informations d’audit **U**</br>La valeur par défaut de **CopyFlags** est **dat** (données, attributs et horodatages).|
|/DCOPY : \<copyflags @ no__t-1|Définit les éléments à copier pour les répertoires. La valeur par défaut est DA. Les options sont D = Data, A = Attributes et T = timestamps.|
|sec|Copie les fichiers avec sécurité (équivalent à **/Copy : DATS**).|
|/copyall|Copie toutes les informations de fichier (équivalent à **/Copy : DATSOU**).|
|/nocopy|Ne copie aucune information sur le fichier (utile avec **/purge**).|
|/secfix|Résout la sécurité des fichiers sur tous les fichiers, même ignorés.|
|/timfix|Corrige les temps de fichier sur tous les fichiers, même ceux qui ont été ignorés.|
|/purge|Supprime les fichiers et répertoires de destination qui n’existent plus dans la source. Pour plus d’informations, consultez la [section Notes](#remarks).|
|/mir|Reflète une arborescence de répertoires (équivalent à **/e** plus **/purge**). Pour plus d’informations, consultez la [section Notes](#remarks).|
|/mov|Déplace les fichiers et les supprime de la source une fois qu’ils ont été copiés.|
|/Move|Déplace les fichiers et les répertoires et les supprime de la source une fois qu’ils ont été copiés.|
|/a + : [RASHCNET]|Ajoute les attributs spécifiés aux fichiers copiés.|
|/a-( : [RASHCNET]|Supprime les attributs spécifiés des fichiers copiés.|
|/Create|Crée uniquement une arborescence de répertoires et des fichiers de longueur nulle.|
|/fat|Crée des fichiers de destination en utilisant uniquement des noms de fichiers FAT de longueur 8,3 caractères.|
|/256|Désactive la prise en charge des chemins d’accès très longs (plus de 256 caractères).|
|/mon : \<N >|Analyse la source et s’exécute à nouveau lorsque plus de *N* modifications sont détectées.|
|/mot : \<M >|Analyse la source et s’exécute à nouveau dans *M* minutes si des modifications sont détectées.|
|/MT [ : N]|Crée des copies multithread avec *N* threads. *N* doit être un entier compris entre 1 et 128. La valeur par défaut de *N* est 8.</br>Le paramètre **/MT** ne peut pas être utilisé avec les paramètres **/IPG** et **/EFSRAW** .</br>Redirigez la sortie à l’aide de l’option **/log** pour obtenir de meilleures performances.</br>Remarque : Le paramètre/MT s’applique à Windows Server 2008 R2 et Windows 7.|
|/RH : HHMM-HHMM|Spécifie les durées d’exécution lorsque de nouvelles copies peuvent être démarrées.|
|/PF|Vérifie les durées d’exécution sur une base par fichier (et non par passe).|
|/IPG : n|Spécifie l’intervalle entre les paquets pour libérer la bande passante sur les lignes lentes.|
|/sl|Ne suivez pas les liens symboliques et créez plutôt une copie du lien.|

> [!IMPORTANT]
> Lorsque vous utilisez l’option de copie **/secfix** , spécifiez le type d’informations de sécurité que vous souhaitez copier en utilisant également l’une des options de copie supplémentaires suivantes :
>- **/COPYALL**
>- **/COPY : O**
>- **/COPY : S**
>- **/COPY : U**
>- **SEC**

### <a name="file-selection-options"></a>Options de sélection de fichier

|Option|Description|
|------|-----------|
|/a|Copie uniquement les fichiers pour lesquels l’attribut **Archive** est défini.|
|/m|Copie uniquement les fichiers pour lesquels l’attribut **Archive** est défini et réinitialise l’attribut **Archive** .|
|/IA : [RASHCNETO]|Comprend uniquement les fichiers pour lesquels l’un des attributs spécifiés est défini.|
|/xa:[RASHCNETO]|Exclut les fichiers pour lesquels l’un des attributs spécifiés est défini.|
|/XF \<FileName > [...]|Exclut les fichiers qui correspondent aux noms ou chemins d’accès spécifiés. Notez que le *nom de fichier* peut inclure **&#42;** des caractères génériques (et **?** ).|
|/xD \<Directory > [...]|Exclut les répertoires qui correspondent aux noms et chemins d’accès spécifiés.|
|/xc|Exclut les fichiers modifiés.|
|/xn|Exclut les fichiers plus récents.|
|/xo|Exclut les fichiers plus anciens.|
|/xx|Exclut les fichiers et les répertoires supplémentaires.|
|/xl|Exclut les fichiers et répertoires « impropres ».|
|/is|Comprend les mêmes fichiers.|
|/IT|Comprend des fichiers « modifiés ».|
|/Max : \<N >|Spécifie la taille de fichier maximale (pour exclure des fichiers de plus de *N* octets).|
|/min : \<N >|Spécifie la taille de fichier minimale (pour exclure des fichiers de plus de *N* octets).|
|/MAXAGE : \<N >|Spécifie l’âge maximal du fichier (pour exclure des fichiers de plus de *N* jours ou de la date).|
|/minage : \<N >|Spécifie l’ancienneté minimale des fichiers (excluez les fichiers de plus de *N* jours ou date).|
|/MAXLAD : \<N >|Spécifie la date maximale de la dernière accès (exclut les fichiers non utilisés depuis *N*).|
|/MINLAD : \<N >|Spécifie la date de dernier accès minimale (exclut les fichiers utilisés depuis *n*) si *n* est inférieur à 1900, *n* spécifie le nombre de jours. Dans le cas contraire, *N* spécifie une date au format AAAAMMJJ.|
|/xj|Exclut les points de jonction, qui sont normalement inclus par défaut.|
|/fft|Suppose l’heure des fichiers FAT (précision de deux secondes).|
|/DST|Compense les différences d’heure d’été d’une heure.|
|/xjd|Exclut les points de jonction des répertoires.|
|/xjf|Exclut les points de jonction pour les fichiers.|

### <a name="retry-options"></a>Options de nouvelle tentative

|Option|Description|
|------|-----------|
|/r : \<N >|Spécifie le nombre de nouvelles tentatives en cas d’échec de copies. La valeur par défaut de *N* est 1 million (1 million nouvelles tentatives).|
|/w : \<N >|Spécifie le délai d’attente entre les tentatives, en secondes. La valeur par défaut de *N* est 30 (temps d’attente de 30 secondes).|
|/reg|Enregistre les valeurs spécifiées dans les options **/r** et **/w** comme paramètres par défaut dans le registre.|
|/tbd|Spécifie que le système doit attendre la définition des noms de partage (erreur de nouvelle tentative 67).|

### <a name="logging-options"></a>Options de journalisation

|Option|Description|
|------|-----------|
|/l|Spécifie que les fichiers doivent être répertoriés uniquement (et non copiés, supprimés ou horodatés).|
|/x|Signale tous les fichiers supplémentaires, pas seulement ceux qui sont sélectionnés.|
|/v|Produit une sortie détaillée et affiche tous les fichiers ignorés.|
|/ts|Comprend les horodatages de fichier source dans la sortie.|
|/FP|Contient les noms de chemin d’accès complets des fichiers dans la sortie.|
|/bytes|Imprime les tailles sous la forme d’octets.|
|/ns|Spécifie que les tailles de fichiers ne doivent pas être journalisées.|
|/nc|Spécifie que les classes de fichiers ne doivent pas être journalisées.|
|/nfl|Spécifie que les noms de fichiers ne doivent pas être journalisés.|
|/ndl|Spécifie que les noms des répertoires ne doivent pas être journalisés.|
|/np|Spécifie que la progression de l’opération de copie (le nombre de fichiers ou de répertoires copiés jusqu’à présent) ne s’affiche pas.|
|/eta|Affiche l’heure d’arrivée estimée (ETA) des fichiers copiés.|
|/log : \<LogFile >|Écrit la sortie de l’État dans le fichier journal (remplace le fichier journal existant).|
|/log + : \<LogFile >|Écrit la sortie d’État dans le fichier journal (ajoute la sortie au fichier journal existant).|
|/Unicode|Affiche la sortie de l’État sous forme de texte Unicode.|
|/UNILOG : \<LogFile >|Écrit la sortie d’État dans le fichier journal en tant que texte Unicode (remplace le fichier journal existant).|
|/UNILOG + : \<LogFile >|Écrit la sortie d’État dans le fichier journal en tant que texte Unicode (ajoute la sortie au fichier journal existant).|
|/tee|Écrit la sortie d’État dans la fenêtre de console, ainsi que dans le fichier journal.|
|/njh|Spécifie qu’il n’existe aucun en-tête de tâche.|
|/njs|Spécifie qu’il n’existe aucun Résumé de la tâche.|

### <a name="job-options"></a>Options de la tâche

|Option|Description|
|------|-----------|
|/travail : \<JobName >|Spécifie que les paramètres doivent être dérivés du fichier de tâche nommé.|
|/Save : \<JobName >|Spécifie que les paramètres doivent être enregistrés dans le fichier de tâche nommé.|
|/quit|Quitte après le traitement de la ligne de commande (pour afficher les paramètres).|
|/nosd|Indique qu’aucun répertoire source n’est spécifié.|
|/nodd|Indique qu’aucun répertoire de destination n’est spécifié.|
|/If|Contient les fichiers spécifiés.|

### <a name="exit-return-codes"></a>Codes de sortie (retour)

Value | Description
-- | --
0 | Aucun fichier n’a été copié. Aucune erreur n’a été rencontrée.  Aucun fichier n’a été mis en correspondance. Les fichiers existent déjà dans le répertoire de destination ; par conséquent, l’opération de copie a été ignorée.
1 | Tous les fichiers ont été correctement copiés.
2 | Le répertoire de destination contient des fichiers supplémentaires qui ne sont pas présents dans le répertoire source. Aucun fichier n’a été copié.
3 | Certains fichiers ont été copiés. Des fichiers supplémentaires étaient présents. Aucune erreur n’a été rencontrée.
5 | Certains fichiers ont été copiés. Certains fichiers sont incompatibles. Aucune erreur n’a été rencontrée.
6\. | Des fichiers supplémentaires et des fichiers incompatibles existent. Aucun fichier n’a été copié et aucun échec n’a été rencontré. Cela signifie que les fichiers existent déjà dans le répertoire de destination.
7 | Des fichiers ont été copiés, une incompatibilité de fichiers était présente et des fichiers supplémentaires étaient présents.
8 | Plusieurs fichiers n’ont pas été copiés.

> [!NOTE]
> Toute valeur supérieure à 8 indique qu’il y avait au moins un échec au cours de l’opération de copie.

### <a name="remarks"></a>Notes

-   L’option **/Mir** est équivalente aux options **/e** plus **/purge** avec une petite différence de comportement :  
    -   Avec les options **/e** plus **/purge** , si le répertoire de destination existe, les paramètres de sécurité du répertoire de destination ne sont pas remplacés.
    -   Avec l’option **/Mir** , si le répertoire de destination existe, les paramètres de sécurité du répertoire de destination sont remplacés.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
