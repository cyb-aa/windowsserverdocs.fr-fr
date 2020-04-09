---
title: exemples Bitsadmin
description: Les exemples suivants montrent comment utiliser l’outil Bitsadmin pour effectuer les tâches les plus courantes.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cb8f8374-ba6e-4a68-85a1-9a95b8215354
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/31/2018
ms.openlocfilehash: 96447410f76e4402c456b5ec402cc730480aedaf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850802"
---
# <a name="bitsadmin-examples"></a>exemples Bitsadmin

Les exemples suivants montrent comment utiliser l’outil `bitsadmin` pour effectuer les tâches les plus courantes.

## <a name="transfer-a-file"></a>Transférer un fichier

Le commutateur **/Transfer** est un raccourci permettant d’effectuer les tâches ci-dessous. Ce commutateur crée le travail, ajoute les fichiers au travail, active le travail dans la file d’attente de transfert et termine le travail. BITSAdmin continue à afficher les informations de progression dans la fenêtre MS-DOS jusqu’à ce que le transfert soit terminé ou qu’une erreur se produise.

`bitsadmin /transfer myDownloadJob /download /priority normal https://downloadsrv/10mb.zip c:\\10mb.zip`

## <a name="create-a-download-job"></a>Créer un travail de téléchargement

Utilisez le commutateur **/Create** pour créer une tâche de téléchargement nommée myDownloadJob.

### <a name="syntax"></a>Syntaxe

```
bitsadmin /create myDownloadJob
```

BITSAdmin retourne un GUID qui identifie de façon unique le travail. Utilisez le GUID ou le nom de la tâche dans les appels suivants. Le texte suivant est un exemple de sortie.

#### <a name="sample-output"></a>Exemple de sortie

`created job {C775D194-090F-431F-B5FB-8334D00D1CB6}`

## <a name="add-files-to-the-download-job"></a>Ajouter des fichiers au travail de téléchargement

Utilisez le commutateur **/AddFile** pour ajouter un fichier au travail. Répétez cet appel pour chaque fichier que vous souhaitez ajouter.

Si plusieurs travaux utilisent myDownloadJob comme nom, vous devez remplacer myDownloadJob par le GUID du travail pour identifier le travail de façon unique.

### <a name="syntax"></a>Syntaxe

```
bitsadmin /addfile myDownloadJob https://downloadsrv/10mb.zip c:\\10mb.zip
```

## <a name="activate-the-download-job"></a>Activer le travail de téléchargement

Lorsque vous créez un nouveau travail, le service BITS interrompt le travail. Pour activer le travail dans la file d’attente de transfert, utilisez le commutateur **/Resume**

Si plusieurs travaux utilisent myDownloadJob comme nom, vous devez remplacer myDownloadJob par le GUID du travail pour identifier le travail de façon unique.

### <a name="syntax"></a>Syntaxe

`bitsadmin /resume myDownloadJob`

## <a name="determine-the-progress-of-the-download-job"></a>Déterminer la progression du travail de téléchargement

Utilisez le commutateur **/info** pour retourner l’état du travail et le nombre de fichiers et d’octets transférés. Lorsque l’État est transféré, le service BITS a transféré avec succès tous les fichiers du travail.

- Utilisez l’argument **/Verbose** pour obtenir des détails complets sur le travail.

- Utilisez le commutateur **/List** ou **/Monitor** pour récupérer tous les travaux dans la file d’attente de transfert.

Si plusieurs travaux utilisent myDownloadJob comme nom, vous devez remplacer myDownloadJob par le GUID du travail pour identifier le travail de façon unique.

### <a name="syntax"></a>Syntaxe

`bitsadmin /info myDownloadJob /verbose`

#### <a name="sample-output"></a>Exemple de sortie

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

## <a name="completing-the-download-job"></a>Fin du travail de téléchargement

Lorsque l’état du travail est transféré, le service BITS a transféré tous les fichiers du travail. Toutefois, les fichiers ne sont pas disponibles tant que vous n’utilisez pas le commutateur **/Complete**

Si plusieurs travaux utilisent myDownloadJob comme nom, vous devez remplacer myDownloadJob par le GUID du travail pour identifier le travail de façon unique.

### <a name="syntax"></a>Syntaxe

`bitsadmin /complete myDownloadJob`

## <a name="monitoring-jobs-in-the-transfer-queue"></a>Surveillance des travaux dans la file d’attente de transfert

Utilisez le commutateur **/List**, **/Monitor**ou **/info** pour surveiller les travaux dans la file d’attente de transfert. Le commutateur **/List** fournit des informations pour tous les travaux de la file d’attente.

## <a name="list-switch"></a>commutateur/List

Le commutateur **/List** retourne l’état du travail et le nombre de fichiers et d’octets transférés pour tous les travaux de la file d’attente de transfert.

### <a name="syntax"></a>Syntaxe

`bitsadmin /list`

#### <a name="sample-output-for-the-list-switch"></a>Exemple de sortie pour le commutateur/List

```
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN

Listed 2 job(s).
```

## <a name="monitor-switch"></a>commutateur/Monitor

Le commutateur **/Monitor** retourne l’état du travail et le nombre de fichiers et d’octets transférés pour tous les travaux dans la file d’attente de transfert, en actualisant les données toutes les 5 secondes. Pour arrêter l’actualisation, appuyez sur CTRL + C.

### <a name="syntax"></a>Syntaxe

`bitsadmin /monitor`

#### <a name="sample-output"></a>Exemple de sortie

```
MONITORING BACKGROUND COPY MANAGER(5 second refresh)
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN
{0B138008-304B-4264-B021-FD04455588FF} job3 TRANSFERRED 1 / 1 100379370 / 100379370
```

## <a name="deleting-jobs-from-the-transfer-queue"></a>Suppression de travaux de la file d’attente de transfert

Utilisez le commutateur **/Reset** pour supprimer tous les travaux de la file d’attente de transfert.

### <a name="syntax"></a>Syntaxe

`bitsadmin /reset`

#### <a name="sample-output"></a>Exemple de sortie

```
{DC61A20C-44AB-4768-B175-8000D02545B9} canceled.
{BB6E91F3-6EDA-4BB4-9E01-5C5CBB5411F8} canceled.
2 out of 2 jobs canceled.
```
