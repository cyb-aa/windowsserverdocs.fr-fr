---
title: exemples bitsadmin
description: Exemples montrant comment utiliser l’outil Bitsadmin pour effectuer les tâches les plus courantes.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cb8f8374-ba6e-4a68-85a1-9a95b8215354
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/31/2018
ms.openlocfilehash: 2fcf7d3716ae45c24510b433ab125551a6d04c85
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718210"
---
# <a name="bitsadmin-examples"></a>exemples bitsadmin

Les exemples suivants montrent comment utiliser l' `bitsadmin` outil pour effectuer les tâches les plus courantes.

## <a name="transfer-a-file"></a>Transférer un fichier

Pour créer un travail, ajouter des fichiers, activer le travail dans la file d’attente de transfert et terminer le travail :

`bitsadmin /transfer myDownloadJob /download /priority normal https://downloadsrv/10mb.zip c:\\10mb.zip`

BITSAdmin continue à afficher les informations de progression dans la fenêtre MS-DOS jusqu’à ce que le transfert soit terminé ou qu’une erreur se produise.

## <a name="create-a-download-job"></a>Créer un travail de téléchargement

Pour créer un travail de téléchargement nommé *myDownloadJob*:

```
bitsadmin /create myDownloadJob
```

BITSAdmin retourne un GUID qui identifie de façon unique le travail. Utilisez le GUID ou le nom de la tâche dans les appels suivants. Le texte suivant est un exemple de sortie.

### <a name="sample-output"></a>Exemple de sortie

`created job {C775D194-090F-431F-B5FB-8334D00D1CB6}`

## <a name="add-files-to-the-download-job"></a>Ajouter des fichiers au travail de téléchargement

Pour ajouter un fichier à la tâche :

```
bitsadmin /addfile myDownloadJob https://downloadsrv/10mb.zip c:\\10mb.zip
```

Répétez cet appel pour chaque fichier que vous souhaitez ajouter. Si plusieurs travaux utilisent *myDownloadJob* comme nom, vous devez utiliser le GUID du travail pour l’identifier de manière unique pour l’achèvement.

## <a name="activate-the-download-job"></a>Activer le travail de téléchargement

Une fois que vous avez créé un nouveau travail, le service BITS interrompt automatiquement le travail. Pour activer le travail dans la file d’attente de transfert :

```
bitsadmin /resume myDownloadJob
```

Si plusieurs travaux utilisent *myDownloadJob* comme nom, vous devez utiliser le GUID du travail pour l’identifier de manière unique pour l’achèvement.

## <a name="determine-the-progress-of-the-download-job"></a>Déterminer la progression du travail de téléchargement

Le commutateur **/info** retourne l’état du travail et le nombre de fichiers et d’octets transférés. Lorsque l’État est indiqué comme `TRANSFERRED`, cela signifie que le service bits a transféré avec succès tous les fichiers du travail. Vous pouvez également ajouter l’argument **/Verbose** pour obtenir des détails complets sur le travail, et **/List** ou **/Monitor** pour obtenir tous les travaux dans la file d’attente de transfert.

Pour retourner l’état de la tâche :

```
bitsadmin /info myDownloadJob /verbose
```

Si plusieurs travaux utilisent *myDownloadJob* comme nom, vous devez utiliser le GUID du travail pour l’identifier de manière unique pour l’achèvement.

## <a name="complete-the-download-job"></a>Terminer le travail de téléchargement

Pour terminer le travail une fois que l’État a `TRANSFERRED`changé :

```
bitsadmin /complete myDownloadJob
```

Vous devez exécuter le `/complete` commutateur pour que les fichiers du travail deviennent disponibles. Si plusieurs travaux utilisent *myDownloadJob* comme nom, vous devez utiliser le GUID du travail pour l’identifier de manière unique pour l’achèvement.

## <a name="monitor-jobs-in-the-transfer-queue-using-the-list-switch"></a>Surveiller les travaux dans la file d’attente de transfert à l’aide du commutateur/List

Pour retourner l’état du travail et le nombre de fichiers et d’octets transférés pour tous les travaux de la file d’attente de transfert :

```
bitsadmin /list
```

### <a name="sample-output"></a>Exemple de sortie

```
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN

Listed 2 job(s).
```

## <a name="monitor-jobs-in-the-transfer-queue-using-the-monitor-switch"></a>Surveiller les travaux dans la file d’attente de transfert à l’aide du commutateur/Monitor

Pour retourner l’état du travail et le nombre de fichiers et d’octets transférés pour tous les travaux dans la file d’attente de transfert, en actualisant les données toutes les 5 secondes :

```
bitsadmin /monitor
```

> [!NOTE]
> Pour arrêter l’actualisation, appuyez sur CTRL + C.

### <a name="sample-output"></a>Exemple de sortie

```
MONITORING BACKGROUND COPY MANAGER(5 second refresh)
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN
{0B138008-304B-4264-B021-FD04455588FF} job3 TRANSFERRED 1 / 1 100379370 / 100379370
```

## <a name="monitor-jobs-in-the-transfer-queue-using-the-info-switch"></a>Surveiller les travaux dans la file d’attente de transfert à l’aide du commutateur/info

Pour retourner l’état du travail et le nombre de fichiers et d’octets transférés :

```
bitsadmin /info
```

### <a name="sample-output"></a>Exemple de sortie

```
GUID: {482FCAF0-74BF-469B-8929-5CCD028C9499} DISPLAY: myDownloadJob
TYPE: DOWNLOAD STATE: TRANSIENT_ERROR OWNER: domain\user
PRIORITY: NORMAL FILES: 0 / 1 BYTES: 0 / UNKNOWN
CREATION TIME: 12/17/2002 1:21:17 PM MODIFICATION TIME: 12/17/2002 1:21:30 PM
COMPLETION TIME: UNKNOWN
NOTIFY INTERFACE: UNREGISTERED NOTIFICATION FLAGS: 3
RETRY DELAY: 600 NO PROGRESS TIMEOUT: 1209600 ERROR COUNT: 0
PROXY USAGE: PRECONFIG PROXY LIST: NULL PROXY BYPASS LIST: NULL
ERROR FILE:    https://downloadsrv/10mb.zip -> c:\10mb.zip
ERROR CODE:    0x80072ee7 - The server name or address could not be resolved
ERROR CONTEXT: 0x00000005 - The error occurred while the remote file was being 
processed.
DESCRIPTION:
JOB FILES:
0 / UNKNOWN WORKING https://downloadsrv/10mb.zip -> c:\10mb.zip
NOTIFICATION COMMAND LINE: none
```

## <a name="delete-jobs-from-the-transfer-queue"></a>Supprimer des travaux de la file d’attente de transfert

Pour supprimer tous les travaux de la file d’attente de transfert, utilisez le commutateur/Reset :

```
bitsadmin /reset
```

### <a name="sample-output"></a>Exemple de sortie

```
{DC61A20C-44AB-4768-B175-8000D02545B9} canceled.
{BB6E91F3-6EDA-4BB4-9E01-5C5CBB5411F8} canceled.
2 out of 2 jobs canceled.
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
