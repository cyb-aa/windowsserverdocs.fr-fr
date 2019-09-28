---
title: exemples Bitsadmin
description: Les exemples suivants montrent comment utiliser l’outil Bitsadmin pour effectuer les tâches les plus courantes.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cb8f8374-ba6e-4a68-85a1-9a95b8215354
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/31/2018
ms.openlocfilehash: c675f08752b3464f7ab1eddd4e9fddf3b16db5f4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381770"
---
# <a name="bitsadmin-examples"></a>exemples Bitsadmin

Les exemples suivants montrent comment utiliser l’outil `bitsadmin` pour effectuer les tâches les plus courantes.

## <a name="transfer-a-file"></a>Transférer un fichier

Le commutateur **/Transfer** est un raccourci permettant d’effectuer les tâches ci-dessous. Ce commutateur crée le travail, ajoute les fichiers au travail, active le travail dans la file d’attente de transfert et termine le travail. BITSAdmin continue à afficher les informations de progression dans la fenêtre MS-DOS jusqu’à ce que le transfert soit terminé ou qu’une erreur se produise.

**Bitsadmin/Transfer myDownloadJob/Download/Priority normal `https://downloadsrv/10mb.zip c:\\10mb.zip`**

## <a name="create-a-download-job"></a>Créer un travail de téléchargement

Utilisez le commutateur **/Create** pour créer une tâche de téléchargement nommée myDownloadJob.

**Bitsadmin/Create myDownloadJob**

BITSAdmin retourne un GUID qui identifie de façon unique le travail. Utilisez le GUID ou le nom de la tâche dans les appels suivants. Le texte suivant est un exemple de sortie.

``` syntax
Created job {C775D194-090F-431F-B5FB-8334D00D1CB6}.
```

Ensuite, utilisez le commutateur **/AddFile** pour ajouter un ou plusieurs fichiers au travail de téléchargement.

## <a name="add-files-to-the-download-job"></a>Ajouter des fichiers au travail de téléchargement

Utilisez le commutateur **/AddFile** pour ajouter un fichier au travail. Répétez cet appel pour chaque fichier que vous souhaitez ajouter. Si plusieurs travaux utilisent myDownloadJob comme nom, vous devez remplacer myDownloadJob par le GUID du travail pour identifier le travail de façon unique.

**Bitsadmin/AddFile myDownloadJob https://downloadsrv/10mb.zip c : @no__t -210mb. zip**

Pour activer le travail dans la file d’attente de transfert, utilisez le commutateur **/Resume**

## <a name="activate-the-download-job"></a>Activer le travail de téléchargement

Lorsque vous créez un nouveau travail, le service BITS interrompt le travail. Pour activer le travail dans la file d’attente de transfert, utilisez le commutateur **/Resume** Si plusieurs travaux utilisent myDownloadJob comme nom, vous devez remplacer myDownloadJob par le GUID du travail pour identifier le travail de façon unique.

**Bitsadmin/Resume myDownloadJob**

Pour déterminer la progression du travail, utilisez le commutateur **/List**, **/info**ou **/Monitor** .

## <a name="determine-the-progress-of-the-download-job"></a>Déterminer la progression du travail de téléchargement

Utilisez le commutateur **/info** pour déterminer la progression d’un travail. Si plusieurs travaux utilisent myDownloadJob comme nom, vous devez remplacer myDownloadJob par le GUID du travail pour identifier le travail de façon unique.

**Bitsadmin/info myDownloadJob/verbose**

Le commutateur **/info** retourne l’état du travail et le nombre de fichiers et d’octets transférés. Lorsque l’État est transféré, le service BITS a transféré avec succès tous les fichiers du travail. L’argument **/Verbose** fournit des détails complets sur le travail. Le texte suivant est un exemple de sortie.

``` syntax
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

Pour recevoir des informations pour tous les travaux de la file d’attente de transfert, utilisez le commutateur **/List** ou **/Monitor**

## <a name="completing-the-download-job"></a>Fin du travail de téléchargement

Lorsque l’état du travail est transféré, le service BITS a transféré tous les fichiers du travail. Toutefois, les fichiers ne sont pas disponibles tant que vous n’utilisez pas le commutateur **/Complete** Si plusieurs travaux utilisent myDownloadJob comme nom, vous devez remplacer myDownloadJob par le GUID du travail pour identifier le travail de façon unique.

**Bitsadmin/Complete myDownloadJob**

## <a name="monitoring-jobs-in-the-transfer-queue"></a>Surveillance des travaux dans la file d’attente de transfert

Utilisez le commutateur **/List**, **/Monitor**ou **/info** pour surveiller les travaux dans la file d’attente de transfert. Le commutateur **/List** fournit des informations pour tous les travaux de la file d’attente.

**bitsadmin/list**

Le commutateur **/List** retourne l’état du travail et le nombre de fichiers et d’octets transférés pour tous les travaux de la file d’attente de transfert. Le texte suivant est un exemple de sortie.

``` syntax
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN

Listed 2 job(s).
```

Utilisez le commutateur **/Monitor** pour surveiller tous les travaux dans la file d’attente. Le commutateur **/Monitor** actualise les données toutes les 5 secondes. Pour arrêter l’actualisation, entrez CTRL + C.

**Bitsadmin/Monitor**

Le commutateur **/Monitor** retourne l’état du travail et le nombre de fichiers et d’octets transférés pour tous les travaux de la file d’attente de transfert. Le texte suivant est un exemple de sortie.

``` syntax
MONITORING BACKGROUND COPY MANAGER(5 second refresh)
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN
{0B138008-304B-4264-B021-FD04455588FF} job3 TRANSFERRED 1 / 1 100379370 / 100379370
```

## <a name="deleting-jobs-from-the-transfer-queue"></a>Suppression de travaux de la file d’attente de transfert

Utilisez le commutateur **/Reset** pour supprimer tous les travaux de la file d’attente de transfert.

**Bitsadmin/Reset**

Le texte suivant est un exemple de sortie.

``` syntax
{DC61A20C-44AB-4768-B175-8000D02545B9} canceled.
{BB6E91F3-6EDA-4BB4-9E01-5C5CBB5411F8} canceled.
2 out of 2 jobs canceled.
```
