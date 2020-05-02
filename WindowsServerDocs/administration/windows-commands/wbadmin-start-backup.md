---
title: Wbadmin start Backup
description: Rubrique de référence pour Wbadmin start Backup, qui crée une sauvegarde à l’aide des paramètres spécifiés.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 56f3e752-d99a-4c3d-8e97-10303c37dd78
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4e113d8c69e4e7793f70615d071889e9b9666988
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725910"
---
# <a name="wbadmin-start-backup"></a>Wbadmin start Backup

Crée une sauvegarde à l’aide des paramètres spécifiés. Si aucun paramètre n’est spécifié et que vous avez créé une sauvegarde quotidienne planifiée, cette sous-commande crée la sauvegarde à l’aide des paramètres de la sauvegarde planifiée. Si des paramètres sont spécifiés, il crée une sauvegarde de copie Service VSS (VSS) et ne met pas à jour l’historique des fichiers en cours de sauvegarde.

Pour créer une sauvegarde unique avec cette sous-commande, vous devez être membre du groupe opérateurs de **sauvegarde** ou **administrateurs** , ou les autorisations appropriées doivent vous avoir été déléguées. En outre, vous devez exécuter **Wbadmin** à partir d’une invite de commandes avec élévation de privilèges. (Pour ouvrir une invite de commandes avec élévation de privilèges, cliquez avec le bouton droit sur **invite de commandes** , puis cliquez sur **exécuter en tant qu’administrateur**.)

## <a name="syntax"></a>Syntaxe

Syntaxe pour Windows ° Vista et Windows Server 2008 :
```
wbadmin start backup
[-backupTarget:{<BackupTargetLocation> | <TargetNetworkShare>}]
[-include:<VolumesToInclude>]
[-allCritical]
[-noVerify]
[-user:<UserName>]
[-password:<Password>]
[-noinheritAcl]
[-vssFull]
[-quiet]
```
Syntaxe pour Windows ° 7 et Windows Server 2008 R2 et versions ultérieures :
```
Wbadmin start backup
[-backupTarget:{<BackupTargetLocation> | <TargetNetworkShare>}]
[-include:<ItemsToInclude>]
[-nonRecurseInclude:<ItemsToInclude>]
[-exclude:<ItemsToExclude>]
[-nonRecurseExclude:<ItemsToExclude>]
[-allCritical]
[-systemState]
[-noVerify]
[-user:<UserName>]
[-password:<Password>]
[-noInheritAcl]
[-vssFull | -vssCopy] 
[-quiet]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|-backupTarget|Spécifie l’emplacement de stockage pour cette sauvegarde. Nécessite une lettre de \\ \\lecteur de disque dur (f :), un chemin d’accès basé sur un GUID de\\ volume au format ? Volume {GUID}, ou un chemin d’accès UNC (Universal Naming Convention) à un dossier partagé\\\\\<distant ( \\ \<ServerName>\\nom_partage>). Par défaut, la sauvegarde est enregistrée à l’adresse \\ \\ \<suivante : \\ \<NomServeur>\\nom_partage>**WindowsImageBackup**\\\<ComputerBackedUp>\\.</br>Important : Si vous enregistrez une sauvegarde dans un dossier partagé distant, cette sauvegarde sera remplacée si vous utilisez le même dossier pour sauvegarder à nouveau le même ordinateur. En outre, si l’opération de sauvegarde échoue, vous risquez de vous retrouver sans sauvegarde, car l’ancienne sauvegarde sera remplacée, mais la sauvegarde la plus récente ne sera pas utilisable. Vous pouvez éviter cela en créant des sous-dossiers dans le dossier partagé distant pour organiser vos sauvegardes. Si vous procédez ainsi, les sous-dossiers auront besoin de deux fois plus d’espace que le dossier parent.|
|-inclure|Pour Windows ° Vista et Windows Server 2008, spécifie la liste délimitée par des virgules de lettres de lecteur de volume, de points de montage de volume ou de noms de volumes basés sur le GUID à inclure dans la sauvegarde. Ce paramètre doit être utilisé uniquement lorsque le paramètre **-backupTarget** est utilisé.</br>Pour Windows ° 7 et Windows Server 2008 R2 et versions ultérieures, spécifie la liste délimitée par des virgules des éléments à inclure dans la sauvegarde. Vous pouvez inclure plusieurs fichiers, dossiers ou volumes. Les chemins d'accès aux volumes peuvent être spécifiés à l'aide de lettres de lecteur de volume, de points de montage de volumes ou de noms de volumes GUID. Si vous utilisez un nom de volume basé sur le GUID, il doit se terminer par une barre\\oblique inverse (). Vous pouvez utiliser le caractère générique (\*) dans le nom de fichier lors de la spécification d’un chemin d’accès à un fichier. Doit être utilisé uniquement lorsque le paramètre **-backupTarget** est utilisé.|
|-Exclude|Pour Windows ° 7 et Windows Server 2008 R2 et versions ultérieures, spécifie la liste délimitée par des virgules des éléments à exclure de la sauvegarde. Vous pouvez exclure des fichiers, des dossiers ou des volumes. Les chemins d'accès aux volumes peuvent être spécifiés à l'aide de lettres de lecteur de volume, de points de montage de volumes ou de noms de volumes GUID. Si vous utilisez un nom de volume basé sur le GUID, il doit se terminer par une barre\\oblique inverse (). Vous pouvez utiliser le caractère générique (\*) dans le nom de fichier lors de la spécification d’un chemin d’accès à un fichier. Doit être utilisé uniquement lorsque le paramètre **-backupTarget** est utilisé.|
|-nonRecurseInclude|Pour Windows ° 7 et Windows Server 2008 R2 et versions ultérieures, spécifie la liste non récursive, délimitée par des virgules, des éléments à inclure dans la sauvegarde. Vous pouvez inclure plusieurs fichiers, dossiers ou volumes. Les chemins d'accès aux volumes peuvent être spécifiés à l'aide de lettres de lecteur de volume, de points de montage de volumes ou de noms de volumes GUID. Si vous utilisez un nom de volume basé sur le GUID, il doit se terminer par une barre\\oblique inverse (). Vous pouvez utiliser le caractère générique (\*) dans le nom de fichier lors de la spécification d’un chemin d’accès à un fichier. Doit être utilisé uniquement lorsque le paramètre **-backupTarget** est utilisé.|
|-nonRecurseExclude|Pour Windows ° 7 et Windows Server 2008 R2 et versions ultérieures, spécifie la liste non récursive, délimitée par des virgules, des éléments à exclure de la sauvegarde. Vous pouvez exclure des fichiers, des dossiers ou des volumes. Les chemins d'accès aux volumes peuvent être spécifiés à l'aide de lettres de lecteur de volume, de points de montage de volumes ou de noms de volumes GUID. Si vous utilisez un nom de volume basé sur le GUID, il doit se terminer par une barre\\oblique inverse (). Vous pouvez utiliser le caractère générique (\*) dans le nom de fichier lors de la spécification d’un chemin d’accès à un fichier. Doit être utilisé uniquement lorsque le paramètre **-backupTarget** est utilisé.|
|-allCritical|Spécifie que tous les volumes critiques (volumes qui contiennent l’état du système d’exploitation) sont inclus dans les sauvegardes. Ce paramètre est utile si vous créez une sauvegarde pour une récupération complète. Elle doit être utilisée uniquement lorsque **-backupTarget** est spécifié. sinon, la commande échoue. Peut être utilisé avec l’option **-include** .</br>Conseil : le volume cible d’une sauvegarde de volume critique peut être un lecteur local, mais il ne peut pas être l’un des volumes inclus dans la sauvegarde.|
|-systemState|Pour Windows ° 7 et Windows Server 2008 R2 et versions ultérieures, crée une sauvegarde qui inclut l’état du système en plus de tous les autres éléments que vous avez spécifiés avec le paramètre **-include** . L’état du système contient les fichiers de démarrage (Boot. ini, NDTLDR, NTDetect.com), le Registre Windows, y compris les paramètres COM, le SYSVOL (stratégies de groupe et scripts d’ouverture de session), le Active Directory et NTDS. DIT sur les contrôleurs de domaine et, si le service de certificats est installé, le magasin de certificats. Si le rôle de serveur Web est installé sur votre serveur, le méta-annuaire IIS sera inclus. Si le serveur fait partie d’un cluster, les informations du service de cluster sont également incluses.|
|-noVerify|Spécifie que les sauvegardes enregistrées sur un support amovible (tel qu’un DVD) ne sont pas vérifiées pour les erreurs. Si vous n’utilisez pas ce paramètre, les sauvegardes enregistrées sur des supports amovibles sont vérifiées pour les erreurs.|
|-utilisateur|Si la sauvegarde est enregistrée dans un dossier partagé distant, spécifie le nom d’utilisateur avec une autorisation d’écriture sur le dossier.|
|-password|Spécifie le mot de passe pour le nom d’utilisateur fourni par le paramètre **-User**.|
|-noInheritAcl|Applique les autorisations de liste de contrôle d’accès (ACL) qui correspondent aux informations d’identification fournies par les paramètres **-User** et \\ \\ \< \\ \< **-Password** à ServerName \\>\\\<nom_partage>\\ WindowsImageBackup ComputerBackedUp>(le dossier qui contient la sauvegarde). Pour accéder ultérieurement à la sauvegarde, vous devez utiliser ces informations d’identification ou être membre du groupe administrateurs ou opérateurs de sauvegarde sur l’ordinateur avec le dossier partagé. Si **-noInheritAcl** n’est pas utilisé, les autorisations de liste de contrôle d’accès du dossier partagé \\ \<distant sont appliquées au dossier ComputerBackedUp> par défaut afin que toute personne ayant accès au dossier partagé distant puisse accéder à la sauvegarde.|
|-vssFull|Effectue une sauvegarde complète à l’aide du Service VSS (VSS). Tous les fichiers sont sauvegardés, l’historique de chaque fichier est mis à jour pour refléter la sauvegarde et les journaux des sauvegardes précédentes peuvent être tronqués. Si ce paramètre n’est pas utilisé, **Wbadmin start Backup** effectue une sauvegarde de copie, mais l’historique des fichiers sauvegardés n’est pas mis à jour.</br>ATTENTION : n’utilisez pas ce paramètre si vous utilisez un produit autre que Sauvegarde Windows Server pour sauvegarder des applications qui se trouvent sur les volumes inclus dans la sauvegarde en cours. Cela peut potentiellement rompre le type incrémentiel, différentiel ou autre de sauvegarde créé par l’autre produit de sauvegarde, car l’historique sur lequel il repose pour déterminer la quantité de données à sauvegarder peut être manquant et peut effectuer une sauvegarde complète inutilement.|
|-vssCopy|Pour Windows 7 et Windows Server 2008 R2 et versions ultérieures, effectue une sauvegarde de copie à l’aide de VSS. Tous les fichiers sont sauvegardés, mais l’historique des fichiers en cours de sauvegarde n’est pas mis à jour, ce qui vous permet de conserver toutes les informations sur les fichiers qui ont été modifiés, supprimés, etc., ainsi que tous les fichiers journaux des applications. L’utilisation de ce type de sauvegarde n’affecte pas la séquence de sauvegardes incrémentielles et différentielles qui peuvent se produire indépendamment de cette sauvegarde de copie. Il s’agit de la valeur par défaut.</br>AVERTISSEMENT : une sauvegarde de copie ne peut pas être utilisée pour les sauvegardes ou restaurations incrémentielles ou différentielles.|
|-quiet|Exécute la sous-commande sans invite à l’utilisateur.|

## <a name="examples"></a>Exemples

Les exemples suivants montrent comment la commande **Wbadmin start Backup** peut être utilisée dans différents scénarios de sauvegarde :

#1 de scénario
- Créer une sauvegarde des volumes e :, d :\\mountpoint et \\ \\? \\Volume {cc566d14-4410-11d9-9d93-806e6f6e6963}
- Enregistrez la sauvegarde dans volume f :
  ```
  wbadmin start backup -backupTarget:f: -include:e:,d:\mountpoint,\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
  ```
  #2 de scénario
- Effectuez une sauvegarde unique de *f :\\dossier1* et *h\\: Dossier2* vers le volume *d :*.
- Sauvegarder l’état du système
- Effectuez une sauvegarde de copie afin que la sauvegarde différentielle normalement planifiée ne soit pas affectée.
  ```
  wbadmin start backup –backupTarget:d: -include:g\folder1,h:\folder2 –systemstate -vsscopy
  ```
  #3 de scénario
- Effectuez une sauvegarde unique de *d :\\dossier1* qui doit être sauvegardée de manière non récursive.
- Sauvegardez le dossier à l’emplacement * \\ \\réseau\\partagesauvegarde manuelle1*
- Limitez l’accès à la sauvegarde aux membres du groupe **administrateurs** ou **opérateurs de sauvegarde** .
  ```
  wbadmin start backup –backupTarget: \\backupshare\backup1 -noinheritacl -nonrecurseinclude:d:\folder1
  ```

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
