---
title: Démarrer la sauvegarde WBADMIN
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 56f3e752-d99a-4c3d-8e97-10303c37dd78
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ac602506960b92333750e7a37692c44c92aae22
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440273"
---
# <a name="wbadmin-start-backup"></a>Démarrer la sauvegarde WBADMIN



Crée une sauvegarde à l’aide des paramètres spécifiés. Si aucun paramètre n’est spécifié et que vous avez créé une sauvegarde quotidienne planifiée, la sous-commande crée la sauvegarde en utilisant les paramètres pour la sauvegarde planifiée. Si les paramètres sont spécifiés, il crée une sauvegarde de copie VSS Volume Shadow Copy Service () et ne met pas à jour l’historique des fichiers qui sont en cours de sauvegarde.

Pour créer une sauvegarde ponctuelle avec la sous-commande, vous devez être membre du **opérateurs de sauvegarde** groupe ou le **administrateurs** groupe, ou vous devez vous avoir été délégué des autorisations appropriées. En outre, vous devez exécuter **wbadmin** à partir d’une invite de commandes avec élévation de privilèges. (Pour ouvrir un invite de commandes avec élévation de privilèges de droit **invite de commandes** puis cliquez sur **exécuter en tant qu’administrateur**.)

Pour obtenir des exemples montrant comment utiliser cette sous-commande, consultez [exemples](#BKMK_examples).

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
Syntaxe pour Windows 7 et Windows Server 2008 R2 et versions ultérieures :
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

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|-backupTarget|Spécifie l’emplacement de stockage pour cette sauvegarde. Nécessite une lettre de lecteur de disque dur (f :)), un chemin basée sur GUID du volume dans le format de \\ \\? \Volume{GUID}, ou un chemin d’accès UNC Universal Naming Convention () vers un dossier partagé distant (\\\\\<nom_serveur > \<nom_partage >\). Par défaut, la sauvegarde sera enregistrée à : \\ \\ <servername> \<nom_partage >\** WindowsImageBackup **\\<ComputerBackedUp>\.</br>Important : Si vous enregistrez une sauvegarde vers un dossier partagé distant, cette sauvegarde sera remplacée si vous utilisez le même dossier pour sauvegarder le même ordinateur à nouveau. En outre, si l’opération de sauvegarde échoue, vous pourrez retrouver sans sauvegarde, car la sauvegarde antérieure sera remplacée, mais la plus récente sauvegarde ne sera pas utilisable. Vous pouvez éviter cela en créant des sous-dossiers dans le dossier partagé distant pour organiser vos sauvegardes. Si vous procédez ainsi, les sous-dossiers devez double de l’espace que le dossier parent.|
|-include|Pour Windows ° Vista et Windows Server 2008, spécifie la liste délimitée par des virgules des lettres de lecteur de volume, les points de montage de volume ou les noms de volumes GUID à inclure dans la sauvegarde. Ce paramètre doit être utilisé uniquement lorsque le **- backupTarget** paramètre est utilisé.</br>Pour Windows 7 et Windows Server 2008 R2 et versions ultérieures, spécifie la liste délimitée par des virgules des éléments à inclure dans la sauvegarde. Vous pouvez inclure plusieurs fichiers, dossiers ou volumes. Les chemins d'accès aux volumes peuvent être spécifiés à l'aide de lettres de lecteur de volume, de points de montage de volumes ou de noms de volumes GUID. Si vous utilisez un nom basé sur le GUID de volume, il doit s’arrêter avec une barre oblique inverse (\\). Vous pouvez utiliser le caractère générique (\*) dans le nom de fichier lorsque vous spécifiez un chemin d’accès à un fichier. Doit être utilisé uniquement lorsque le **- backupTarget** paramètre est utilisé.|
|-exclude|Pour Windows 7 et Windows Server 2008 R2 et versions ultérieures, spécifie la liste délimitée par des virgules des éléments à exclure de la sauvegarde. Vous pouvez exclure des fichiers, dossiers ou volumes. Les chemins d'accès aux volumes peuvent être spécifiés à l'aide de lettres de lecteur de volume, de points de montage de volumes ou de noms de volumes GUID. Si vous utilisez un nom basé sur le GUID de volume, il doit s’arrêter avec une barre oblique inverse (\\). Vous pouvez utiliser le caractère générique (\*) dans le nom de fichier lorsque vous spécifiez un chemin d’accès à un fichier. Doit être utilisé uniquement lorsque le **- backupTarget** paramètre est utilisé.|
|-nonRecurseInclude|Pour Windows 7 et Windows Server 2008 R2 et versions ultérieures, spécifie la non récursif, une liste délimitée par des virgules d’éléments à inclure dans la sauvegarde. Vous pouvez inclure plusieurs fichiers, dossiers ou volumes. Les chemins d'accès aux volumes peuvent être spécifiés à l'aide de lettres de lecteur de volume, de points de montage de volumes ou de noms de volumes GUID. Si vous utilisez un nom basé sur le GUID de volume, il doit s’arrêter avec une barre oblique inverse (\\). Vous pouvez utiliser le caractère générique (\*) dans le nom de fichier lorsque vous spécifiez un chemin d’accès à un fichier. Doit être utilisé uniquement lorsque le **- backupTarget** paramètre est utilisé.|
|-nonRecurseExclude|Pour Windows 7 et Windows Server 2008 R2 et versions ultérieures, spécifie la non récursif, une liste délimitée par des virgules d’éléments à exclure de la sauvegarde. Vous pouvez exclure des fichiers, dossiers ou volumes. Les chemins d'accès aux volumes peuvent être spécifiés à l'aide de lettres de lecteur de volume, de points de montage de volumes ou de noms de volumes GUID. Si vous utilisez un nom basé sur le GUID de volume, il doit s’arrêter avec une barre oblique inverse (\\). Vous pouvez utiliser le caractère générique (\*) dans le nom de fichier lorsque vous spécifiez un chemin d’accès à un fichier. Doit être utilisé uniquement lorsque le **- backupTarget** paramètre est utilisé.|
|-allCritical|Spécifie que tous les volumes critiques (volumes qui contiennent l’état du système d’exploitation) inclus dans les sauvegardes. Ce paramètre est utile si vous créez une sauvegarde pour une récupération complète. Il doit être utilisé uniquement lorsque **- backupTarget** est spécifié, sinon la commande échoue. Peut être utilisé avec le **-inclure** option.</br>Astuce : Le volume cible pour une sauvegarde sur le volume critique peut être un lecteur local, mais il ne peut pas être un des volumes qui sont inclus dans la sauvegarde.|
|-systemState|Pour Windows 7 et Windows Server 2008 R2 et versions ultérieures, crée une sauvegarde qui inclut l’état du système en plus de tous les autres éléments que vous avez spécifié avec la **-inclure** paramètre. L’état du système contient les fichiers de démarrage (Boot.ini, NDTLDR, NTDetect.com), le Registre Windows, y compris les paramètres de COM, le dossier SYSVOL (stratégies de groupe et les Scripts d’ouverture de session), Active Directory et NTDS. DIT sur les contrôleurs de domaine et, si le service de certificats est installé, le certificat Store. Si le rôle de serveur Web installé sur votre serveur, le méta-annuaire IIS sera inclus. Si le serveur fait partie d’un cluster, les informations de Service de Cluster seront également être incluses.|
|-noVerify|Spécifie que les sauvegardes enregistrées sur un support amovible (par exemple, un DVD) ne sont pas vérifiées pour les erreurs. Si vous n’utilisez pas ce paramètre, les sauvegardes enregistrées sur un support amovible sont vérifiés pour les erreurs.|
|-utilisateur|Si la sauvegarde est enregistrée dans un dossier partagé distant, spécifie le nom d’utilisateur avec l’autorisation d’écriture dans le dossier.|
|-password|Spécifie le mot de passe pour le nom d’utilisateur qui est fourni par le paramètre **-utilisateur**.|
|-noInheritAcl|Applique les autorisations de liste (ACL) de contrôle d’accès correspondent pour les informations d’identification fournies par le **-utilisateur** et **-mot de passe** paramètres \\ \\ \< nom_serveur >\<nom_partage > \WindowsImageBackup\<ComputerBackedUp > \ (le dossier qui contient la sauvegarde). Pour accéder à la sauvegarde plus tard, vous devez utiliser ces informations d’identification ou être membre du groupe Administrateurs ou du groupe Opérateurs de sauvegarde sur l’ordinateur avec le dossier partagé. Si **- noInheritAcl** est ne pas utilisé, les autorisations ACL à partir du dossier partagé distant sont appliquées à la \<ComputerBackedUp > dossier par défaut, de sorte que toute personne ayant accès au dossier partagé distant peut accéder à la sauvegarde.|
|-vssFull|Effectue une sauvegarde complète à l’aide de Volume Shadow Copy Service (VSS). Tous les fichiers sont sauvegardés, l’historique de chaque fichier est mis à jour qu’elle a été sauvegardée, et les journaux des sauvegardes antérieures peuvent être tronquées. Si ce paramètre n’est pas utilisé **wbadmin start sauvegarde** fait une copie de sauvegarde, mais l’historique des fichiers en cours de sauvegarde n’est pas mis à jour.</br>Avertissement : N’utilisez pas ce paramètre si vous utilisez un produit autre que la sauvegarde Windows Server pour sauvegarder des applications qui se trouvent sur les volumes inclus dans la sauvegarde actuelle. Cette opération donc risque de perturber l’incrémentiel, différentielle ou autres type des sauvegardes qui crée le produit de sauvegarde, car l’historique qui ils estiment que pour déterminer la quantité de données à sauvegarder peut-être être manquante et ils peuvent effectuer complète de sauvegarde inutilement.|
|-vssCopy|Pour Windows 7 et Windows Server 2008 R2 et versions ultérieures, effectue une sauvegarde de copie à l’aide de VSS. Tous les fichiers sont sauvegardés, mais l’historique des fichiers en cours de sauvegarde n’est pas mis à jour afin de conserver toutes les informations sur les fichiers où modifié, supprimé, etc., ainsi que les fichiers journaux d’application. À l’aide de ce type de sauvegarde n’affecte pas la séquence des sauvegardes incrémentielles et différentielles qui peuvent se produire indépendamment de cette sauvegarde de copie. Valeur par défaut.</br>Avertissement : Une sauvegarde de copie ne peut pas être utilisée pour les restaurations ou sauvegardes incrémentielles ou différentielles.|
|-quiet|Exécute la sous-commande sans invite à l’utilisateur.|

## <a name="BKMK_examples"></a>Exemples

Les exemples suivants montrent comment la **wbadmin start sauvegarde** commande peut être utilisée dans différents scénarios de sauvegarde :

Scénario #1
- Créer une sauvegarde de volumes e:, d:\mountpoint, et \\ \\? \Volume{cc566d14-4410-11d9-9d93-806e6f6e6963}
- La sauvegarde vers un volume f:
  ```
  wbadmin start backup -backupTarget:f: -include:e:,d:\mountpoint,\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
  ```
  Scénario #2
- Effectuer une sauvegarde ponctuelle des *f:\folder1* et *h:\folder2* au volume *d:* .
- Sauvegarde l’état du système
- Effectuez une sauvegarde de copie afin que la sauvegarde différentielle normalement planifiée n’est pas affectée.
  ```
  wbadmin start backup –backupTarget:d: -include:g\folder1,h:\folder2 –systemstate -vsscopy
  ```
  Scénario #3
- Effectuer une sauvegarde ponctuelle des *d:\folder1* qui doivent être sauvegardées de manière non récursive.
- Le dossier à l’emplacement réseau de sauvegarde  *\\ \\backupshare\backup1*
- Restreindre l’accès à la sauvegarde aux membres de la **administrateurs** ou **opérateurs de sauvegarde** groupe.
  ```
  wbadmin start backup –backupTarget: \\backupshare\backup1 -noinheritacl -nonrecurseinclude:d:\folder1
  ```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
