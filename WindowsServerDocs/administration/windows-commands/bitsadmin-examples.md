---
title: exemples de bitsadmin
description: Les exemples suivants montrent comment utiliser l’outil bitsadmin pour effectuer les tâches les plus courantes.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: a98e1a876c972b0f146ff37aff0a77399b684e99
ms.sourcegitcommit: 8eea7aadbe94f5d4635c4ffedc6a831558733cc0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66308558"
---
# <a name="bitsadmin-examples"></a>exemples de bitsadmin

Les exemples suivants montrent comment utiliser le `bitsadmin` outil pour effectuer les tâches les plus courantes.

## <a name="transfer-a-file"></a>Transférer un fichier

Le **/transfert** commutateur est un raccourci pour effectuer les tâches répertoriées ci-dessous. Ce commutateur crée la tâche ajoute les fichiers au travail, Active le travail dans la file d’attente de transfert et termine la tâche. BITSAdmin continue d’afficher des informations sur la progression dans la fenêtre MS-DOS jusqu'à ce que le transfert se termine ou une erreur se produit.

**Bitsadmin /transfer myDownloadJob /download /priority normal `https://downloadsrv/10mb.zip c:\\10mb.zip`**

## <a name="create-a-download-job"></a>Créer une tâche de téléchargement

Utilisez le **/ créer** commutateur pour créer une tâche de téléchargement nommée myDownloadJob.

**Bitsadmin / créer myDownloadJob**

BITSAdmin retourne un GUID qui identifie de façon unique le travail. Utilisez le nom de travail ou le GUID dans les appels suivants. Le texte suivant est un exemple de sortie.

``` syntax
Created job {C775D194-090F-431F-B5FB-8334D00D1CB6}.
```

Ensuite, utilisez le **/AddFile.** commutateur à ajouter un ou plusieurs fichiers à la tâche de téléchargement.

## <a name="add-files-to-the-download-job"></a>Ajouter des fichiers à la tâche de téléchargement

Utilisez le **/AddFile.** commutateur pour ajouter un fichier à la tâche. Répétez cet appel pour chaque fichier que vous souhaitez ajouter. Si plusieurs travaux utilisent myDownloadJob comme leur nom, vous devez remplacer myDownloadJob avec le GUID du travail pour identifier de façon unique le travail.

**Bitsadmin /AddFile. myDownloadJob https://downloadsrv/10mb.zip c:\\10mb.zip**

Pour activer la tâche dans la file d’attente de transfert, utilisez le **/reprendre** basculer.

## <a name="activate-the-download-job"></a>Activer la tâche de téléchargement

Lorsque vous créez une nouvelle tâche, le service BITS interrompt le travail. Pour activer la tâche dans la file d’attente de transfert, utilisez le **/reprendre** basculer. Si plusieurs travaux utilisent myDownloadJob comme leur nom, vous devez remplacer myDownloadJob avec le GUID du travail pour identifier de façon unique le travail.

**Bitsadmin /resume myDownloadJob**

Pour déterminer la progression du travail, utilisez le **/la liste**, **/info**, ou **/surveiller** basculer.

## <a name="determine-the-progress-of-the-download-job"></a>Déterminer la progression de la tâche de téléchargement

Utilisez le **/info** commutateur pour déterminer la progression d’une tâche. Si plusieurs travaux utilisent myDownloadJob comme leur nom, vous devez remplacer myDownloadJob avec le GUID du travail pour identifier de façon unique le travail.

**bitsadmin /info myDownloadJob /verbose**

Le **/info** switch renvoie l’état du travail et le nombre de fichiers et d’octets transférés. Lorsque l’état est transféré, BITS a transféré tous les fichiers dans le travail. Le **/verbose** argument fournit des détails complets de la tâche. Le texte suivant est un exemple de sortie.

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

Pour recevoir des informations pour tous les travaux dans la file d’attente de transfert, utilisez le **/la liste** ou **/surveiller** basculer.

## <a name="completing-the-download-job"></a>Fin de la tâche de téléchargement

Lorsque l’état du travail est transféré, BITS a transféré tous les fichiers dans le travail. Toutefois, les fichiers ne sont pas disponibles tant que vous utilisez le **/ complète** basculer. Si plusieurs travaux utilisent myDownloadJob comme leur nom, vous devez remplacer myDownloadJob avec le GUID du travail pour identifier de façon unique le travail.

**Bitsadmin / complete myDownloadJob**

## <a name="monitoring-jobs-in-the-transfer-queue"></a>Surveillance des travaux dans la file d’attente de transfert

Utilisez le **/la liste**, **/surveiller**, ou **/info** commutateur pour surveiller les travaux dans la file d’attente de transfert. Le **/la liste** commutateur fournit des informations pour tous les travaux dans la file d’attente.

**bitsadmin /list**

Le **/la liste** switch renvoie l’état du travail et le nombre de fichiers et d’octets transférés pour tous les travaux dans la file d’attente de transfert. Le texte suivant est un exemple de sortie.

``` syntax
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN

Listed 2 job(s).
```

Utilisez le **/surveiller** commutateur pour surveiller tous les travaux dans la file d’attente. Le **/surveiller** commutateur actualise les données toutes les 5 secondes. Pour arrêter l’actualisation, entrez CTRL + C.

**bitsadmin /monitor**

Le **/surveiller** switch renvoie l’état du travail et le nombre de fichiers et d’octets transférés pour tous les travaux dans la file d’attente de transfert. Le texte suivant est un exemple de sortie.

``` syntax
MONITORING BACKGROUND COPY MANAGER(5 second refresh)
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN
{0B138008-304B-4264-B021-FD04455588FF} job3 TRANSFERRED 1 / 1 100379370 / 100379370
```

## <a name="deleting-jobs-from-the-transfer-queue"></a>Suppression de travaux à partir de la file d’attente de transfert

Utilisez le **/réinitialisation** commutateur pour supprimer tous les travaux de la file d’attente de transfert.

**bitsadmin /reset**

Le texte suivant est un exemple de sortie.

``` syntax
{DC61A20C-44AB-4768-B175-8000D02545B9} canceled.
{BB6E91F3-6EDA-4BB4-9E01-5C5CBB5411F8} canceled.
2 out of 2 jobs canceled.
```
