---
title: robocopy
description: Découvrez comment utiliser la commande robocopy dans Windows et Windows Server pour copier des fichiers
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: c4218331f716dc530e01f28a295cebd83c0e6616
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818110"
---
# <a name="robocopy"></a>robocopy



Copie des données de fichier.

## <a name="syntax"></a>Syntaxe

```
robocopy <Source> <Destination> [<File>[ ...]] [<Options>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Source>|Spécifie le chemin d’accès au répertoire source.|
|\<Destination>|Spécifie le chemin d’accès au répertoire de destination.|
|\<File>|Spécifie l’ou les fichiers à copier. Vous pouvez utiliser des caractères génériques (**&#42;** ou **?**), si vous le souhaitez. Si le **fichier** paramètre n’est pas spécifié, **\*.\*** est utilisé comme valeur par défaut.|
|\<Options>|Spécifie les options à utiliser avec le **robocopy** commande.|

### <a name="copy-options"></a>Options de copie

|Option|Description|
|------|-----------|
|/s|Sous-répertoires de copies. Notez que cette option exclut les répertoires vides.|
|/e|Sous-répertoires de copies. Notez que cette option inclut les répertoires vides. Pour plus d’informations, consultez [notes](#BKMK_remarks).|
|/Lev :\<N >|Copie uniquement la partie supérieure *N* niveaux de l’arborescence du répertoire source.|
|/z|Copie les fichiers en mode de redémarrage.|
|/b|Copie les fichiers en mode de sauvegarde.|
|/zb|Utilise le mode de redémarrage. Si l’accès est refusé, cette option utilise le mode de sauvegarde.|
|/EFSRAW|Copie tous les fichiers chiffrés en mode EFS RAW.|
|/ copier :\<indicateurscopie >|Spécifie les propriétés de fichier à copier. Voici les valeurs valides pour cette option :</br>**D** données</br>**Un** attributs</br>**T** horodatages</br>**S** liste de contrôle d’accès (ACL) NTFS</br>**O** informations sur le propriétaire</br>**U** informations d’audit</br>La valeur par défaut **indicateurscopie** est **DAT** (données, attributs et horodatages).|
|/dcopy:\<copyflags\>|Définit les éléments à copier des répertoires. Valeur par défaut est DA. Les options sont D = données, A = attributs et T = horodatages.|
|/sec|Copie des fichiers avec la sécurité (équivalent à **/Copy : DATs**).|
|/copyall|Copie toutes les informations de fichier (équivalent à **datsou**).|
|/NOCOPY|Ne copie aucune information de fichier (utile avec **/purge**).|
|/secfix|Sécurité des fichiers de correctifs sur tous les fichiers, même celles ignoré.|
|/timfix|Temps de fichiers de correctifs sur tous les fichiers, même celles ignoré.|
|/ Purge|Supprime les fichiers de destination et de répertoires qui n’existent plus dans la source. Pour plus d’informations, consultez [notes](#BKMK_remarks).|
|/mir|Reflète une arborescence de répertoires (équivalent à **/e** plus **/purge**). Pour plus d’informations, consultez [notes](#BKMK_remarks).|
|/mov|Déplace les fichiers et les supprime de la source une fois qu’ils sont copiés.|
|/ Move|Déplace les fichiers et répertoires et les supprime de la source une fois qu’ils sont copiés.|
|/ a + : [RASHCNET]|Ajoute les attributs spécifiés pour les fichiers copiés.|
|/ a- : [RASHCNET]|Supprime les attributs spécifiés à partir de fichiers copiés.|
|/ créer|Crée une arborescence de répertoires et fichiers de longueur nulle uniquement.|
|/fat|Crée des fichiers de destination en utilisant uniquement les noms de fichiers FAT de longueur en caractères 8.3.|
|/256|Désactive la prise en charge des chemins d’accès très longs (plus de 256 caractères).|
|/ mois :\<N >|Surveille la source et s’exécute à nouveau lorsque plus de *N* modifications sont détectées.|
|/mot :\<M >|Surveille la source et s’exécute à nouveau en *M* minutes si des modifications sont détectées.|
|/MT[:N]|Crée des copies multithreads avec *N* threads. *N* doit être un entier compris entre 1 et 128. La valeur par défaut *N* est 8.</br>Le **/MT** paramètre ne peut pas être utilisé avec le **/IPG** et **/EFSRAW** paramètres.</br>Redirige la sortie à l’aide de **/LOG** option pour optimiser les performances.</br>Remarque: Le paramètre/MT s’applique à Windows Server 2008 R2 et Windows 7.|
|/rh:hhmm-hhmm|Spécifie le temps d’exécution lorsque les nouvelles copies peuvent être démarrés.|
|/pf|Vérifications exécutées fois sur une base par fichier (et pas par pass).|
|/IPG:n|Spécifie l’intervalle entre les paquets pour libérer de la bande passante sur les lignes lentes.|
|/sl|Suit le lien symbolique et copie de la cible.|

> [!IMPORTANT]
> Lorsque vous utilisez le **/SECFIX** copier option, spécifiez le type d’informations de sécurité à copier également l’une de ces options de copie supplémentaire :</br>> -   **/COPYALL**</br>&GT;- **/COPY:O**</br>&GT;- **/COPY:S**</br>&GT;- **/COPY:U**</br>> -   **/SEC**

### <a name="file-selection-options"></a>Options de sélection de fichier

|Option|Description|
|------|-----------|
|/a|Copie uniquement les fichiers pour lesquels le **Archive** attribut est défini.|
|/m|Copie uniquement les fichiers pour lesquels le **Archive** attribut est défini et réinitialise le **Archive** attribut.|
|/IA : [RASHCNETO]|Inclut uniquement les fichiers pour lequel un des attributs spécifiés sont défini.|
|/XA : [RASHCNETO]|Exclut les fichiers pour lequel un des attributs spécifiés sont défini.|
|/xf \<FileName>[ ...]|Exclut les fichiers qui correspondent aux noms spécifiés ou des chemins d’accès. Notez que *FileName* peut inclure des caractères génériques (**&#42;** et **?**).|
|/XD \<répertoire > [...]|Exclut les répertoires qui correspondent aux noms spécifiés et aux chemins d’accès.|
|/xc|Exclut les fichiers modifiés.|
|/xn|Exclut les fichiers les plus récents.|
|/xo|Exclut les fichiers plus anciens.|
|/xx|Exclut les répertoires et fichiers supplémentaires.|
|/xl|Exclut les répertoires et fichiers « vide ».|
|/is|Inclut les mêmes fichiers.|
|/it|Inclut les fichiers « modifiés ».|
|/ max. :\<N >|Spécifie la taille de fichier maximale (pour exclure des fichiers supérieure *N* octets).|
|/min :\<N >|Spécifie la taille de fichier minimale (pour exclure les fichiers inférieurs à *N* octets).|
|/MaxAge :\<N >|Spécifie l’âge de fichier maximale (pour exclure les fichiers antérieurs à *N* jours ou date).|
|/MINAGE :\<N >|Spécifie l’âge minimal du fichier (exclut les fichiers plus récent que *N* jours ou date).|
|/MAXLAD :\<N >|Spécifie la date du dernier accès au maximum (exclut les fichiers inutilisés depuis *N*).|
|/MINLAD :\<N >|Spécifie la date du dernier accès au minimum (exclut les fichiers utilisés depuis *N*) si *N* est inférieur à 1900, *N* Spécifie le nombre de jours. Sinon, *N* spécifie une date au format AAAAMMJJ.|
|/xj|Exclut les points de jonction, qui sont normalement inclus par défaut.|
|/fft|Suppose que les fichiers FAT heures (précision de deux secondes).|
|/dst|Compense les différences d’heure de l’heure d’été d’une heure.|
|/xjd|Exclut les points de jonction pour les répertoires.|
|/xjf|Exclut les points de jonction pour les fichiers.|

### <a name="retry-options"></a>Options de nouvelle tentative

|Option|Description|
|------|-----------|
|/ r:\<N >|Spécifie le nombre de tentatives sur des copies ayant échoués. La valeur par défaut *N* est 1 000 000 (un million de tentatives).|
|/ w:\<N >|Spécifie le délai d’attente entre les tentatives, en secondes. La valeur par défaut *N* est 30 (30 secondes de délai d’attente).|
|/reg|Enregistre les valeurs spécifiées dans le **/r** et **/w** options en tant que paramètres par défaut dans le Registre.|
|/tbd|Spécifie que le système attendra pour définir les noms de partage (erreur 67 de nouvelle tentative).|

### <a name="logging-options"></a>Options de journalisation

|Option|Description|
|------|-----------|
|/l|Spécifie que les fichiers doivent être répertoriés uniquement (et pas copiées, supprimées, ou horodatés).|
|/x|Signale tous les fichiers supplémentaires, pas uniquement celles qui sont sélectionnées.|
|/v|Produit une sortie détaillée et affiche tous les fichiers ignorés.|
|/ts|Inclut les horodatages de fichier source dans la sortie.|
|/fp|Inclut les noms de chemin d’accès complet des fichiers dans la sortie.|
|/bytes|Imprime les tailles, en octets.|
|/ns|Spécifie que les tailles de fichier ne doivent ne pas être connecté.|
|/nc|Spécifie que les classes de fichier ne doivent ne pas être connecté.|
|/nfl|Spécifie que les noms de fichiers ne doivent ne pas être connecté.|
|/ndl|Spécifie que les noms de répertoire ne doivent ne pas être connecté.|
|/np|Spécifie que la progression de l’opération de copie (le nombre de fichiers ou répertoires copiés jusqu’ici) n’est pas affichée.|
|/eta|Affiche la durée estimée d’arrivée (ETA) les fichiers copiés.|
|/ log :\<LogFile >|Écrit la sortie de l’état dans le fichier journal (écrase le fichier journal existant).|
|/log+ :\<LogFile >|Écrit la sortie de l’état dans le fichier journal (ajoute la sortie au fichier journal existant).|
|/unicode|Affiche la sortie de l’état en tant que texte Unicode.|
|/UNILOG :\<LogFile >|Écrit l’état dans le fichier journal de sortie en tant que texte Unicode (écrase le fichier journal existant).|
|/UNILOG+ :\<LogFile >|Écrit l’état dans le fichier journal de sortie en tant que texte Unicode (ajoute la sortie au fichier journal existant).|
|/tee|Écrit la sortie de l’état dans la fenêtre de console, ainsi que dans le fichier journal.|
|/njh|Spécifie qu’il n’existe aucun en-tête de travail.|
|/njs|Spécifie qu’il n’existe aucun résumé du travail.|

### <a name="job-options"></a>Options de tâche

|Option|Description|
|------|-----------|
|/job:\<JobName>|Spécifie que les paramètres sont à dériver à partir du fichier de travail nommé.|
|/ Enregistrer :\<JobName >|Spécifie que les paramètres sont à enregistrer dans le fichier nommé.|
|/quit|Quitte la ligne de commande de traitement (pour afficher les paramètres).|
|/nosd|Indique qu’aucun répertoire source n’est spécifié.|
|/NODD|Indique qu’aucun répertoire de destination est spécifié.|
|/if|Inclut les fichiers spécifiés.|

### <a name="exit-return-codes"></a>Codes de sortie (retour)
Value | Description
-- | --
0 | Aucun fichier ont été copiés. Aucune défaillance a été rencontrée.  Aucun fichier ont été ne correspondent pas. Les fichiers existent déjà dans le répertoire de destination ; Par conséquent, l’opération de copie a été ignorée.
1 | Tous les fichiers ont été copiés avec succès.
2 | Il existe des fichiers supplémentaires dans le répertoire de destination qui ne figurent pas dans le répertoire source. Aucun fichier ont été copiés.
3 | Certains fichiers ont été copiés. Fichiers supplémentaires étaient présents. Aucune défaillance a été rencontrée.
5 | Certains fichiers ont été copiés. Certains fichiers ont été incompatibles. Aucune défaillance a été rencontrée.
6 | Fichiers supplémentaires et des fichiers qui ne correspondent pas existent. Aucun fichier ont été copiés et sans erreurs se sont produites. Cela signifie que les fichiers existent déjà dans le répertoire de destination.
7 | Fichiers ont été copiés, il existait une incompatibilité de fichier et des fichiers supplémentaires étaient présents.
8 | Plusieurs fichiers n’a pas été copié.

> [!NOTE]
> Toute valeur supérieure à 8 indique qu’il existe au moins un échec pendant l’opération de copie.

### <a name="BKMK_remarks"></a>Remarques

-   Le **/mir** option est équivalente à la **/e** plus **/purge** options avec une petite différence de comportement :  
    -   Avec le **/e** plus **/purge** options, si le répertoire de destination existe, les paramètres de sécurité de répertoire de destination ne sont pas remplacés.
    -   Avec le **/mir** option, si le répertoire de destination existe, les paramètres de sécurité de répertoire de destination sont remplacés.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
