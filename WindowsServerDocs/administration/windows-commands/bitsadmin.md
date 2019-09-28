---
title: bitsadmin
description: La rubrique commandes Windows pour **Bitsadmin** -Bitsadmin est un outil en ligne de commande que vous pouvez utiliser pour créer, télécharger ou télécharger des travaux et surveiller leur progression.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4853036e-1df8-45ad-8be6-cfb097b8dd27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b53293b28a83ecced34d248741996c958531d517
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380099"
---
# <a name="bitsadmin"></a>bitsadmin

> **S’applique à**: Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10

Bitsadmin est un outil de ligne de commande que vous pouvez utiliser pour créer des travaux de téléchargement ou de téléchargement et pour surveiller leur progression. L’outil Bitsadmin utilise des commutateurs pour identifier le travail à effectuer.  Vous pouvez appeler `bitsadmin /?` ou `bitsadmin /HELP` pour obtenir une liste de commutateurs.

La plupart des commutateurs requièrent un paramètre \<Job @ no__t-1 que vous définissez sur le nom d’affichage ou le GUID du travail. Notez que le nom d’affichage d’un travail n’est peut-être pas unique. Les commutateurs **/Create** et **/List** retournent le GUID d’un travail.

Par défaut, vous pouvez accéder à des informations sur vos propres travaux. Pour accéder aux informations des travaux d’un autre utilisateur, vous devez disposer de privilèges d’administrateur. Si le travail a été créé dans un état élevé, vous devez exécuter bitsadmin à partir d’une fenêtre avec élévation de privilèges. dans le cas contraire, vous aurez un accès en lecture seule au travail.

La plupart des commutateurs correspondent aux méthodes des [interfaces bits](/windows/desktop/bits/bits-interfaces). Pour plus d’informations sur l’utilisation d’un commutateur, consultez la méthode correspondante.

Utilisez les commutateurs suivants pour créer un travail, définir et récupérer les propriétés d’un travail, et surveiller l’état d’un travail. Pour obtenir des exemples qui montrent comment utiliser certains de ces commutateurs pour effectuer des tâches, consultez [exemples Bitsadmin](bitsadmin-examples.md).

## <a name="switches"></a>Active

[bitsadmin addfile](bitsadmin-addfile.md)  
[bitsadmin addfileset](bitsadmin-addfileset.md)  
[bitsadmin addfilewithranges](bitsadmin-addfilewithranges.md)  
[bitsadmin cache](bitsadmin-cache.md)  
[bitsadmin cancel](bitsadmin-cancel.md)  
[bitsadmin complete](bitsadmin-complete.md)  
[bitsadmin create](bitsadmin-create.md)  
[bitsadmin getaclflags](bitsadmin-getaclflags.md)  
[bitsadmin getbytestotal](bitsadmin-getbytestotal.md)  
[bitsadmin getbytestransferred](bitsadmin-getbytestransferred.md)  
[bitsadmin getclientcertificate](bitsadmin-getclientcertificate.md)  
[bitsadmin getcompletiontime](bitsadmin-getcompletiontime.md)  
[bitsadmin getcreationtime](bitsadmin-getcreationtime.md)  
[bitsadmin getcustomheaders](bitsadmin-getcustomheaders.md)  
[bitsadmin getdescription](bitsadmin-getdescription.md)  
[bitsadmin getdisplayname](bitsadmin-getdisplayname.md)  
[bitsadmin geterror](bitsadmin-geterror.md)  
[bitsadmin geterrorcount](bitsadmin-geterrorcount.md)  
[bitsadmin getfilestotal](bitsadmin-getfilestotal.md)  
[bitsadmin getfilestransferred](bitsadmin-getfilestransferred.md)  
[bitsadmin gethelpertokenflags](bitsadmin-gethelpertokenflags.md)  
[bitsadmin gethelpertokensid](bitsadmin-gethelpertokensid.md)  
[Bitsadmin gethttpmethod](bitsadmin-gethttpmethod.md)
[Bitsadmin getmaxdownloadtime](bitsadmin-getmaxdownloadtime.md)  
[bitsadmin getminretrydelay](bitsadmin-getminretrydelay.md)  
[bitsadmin getmodificationtime](bitsadmin-getmodificationtime.md)  
[bitsadmin getnoprogresstimeout](bitsadmin-getnoprogresstimeout.md)  
[bitsadmin getnotifycmdline](bitsadmin-getnotifycmdline.md)  
[bitsadmin getnotifyflags](bitsadmin-getnotifyflags.md)  
[bitsadmin getnotifyinterface](bitsadmin-getnotifyinterface.md)  
[bitsadmin getowner](bitsadmin-getowner.md)  
[bitsadmin getpeercachingflags](bitsadmin-getpeercachingflags.md)  
[bitsadmin getpriority](bitsadmin-getpriority.md)  
[bitsadmin getproxybypasslist](bitsadmin-getproxybypasslist.md)  
[bitsadmin getproxylist](bitsadmin-getproxylist.md)  
[bitsadmin getproxyusage](bitsadmin-getproxyusage.md)  
[bitsadmin getreplydata](bitsadmin-getreplydata.md)  
[bitsadmin getreplyfilename](bitsadmin-getreplyfilename.md)  
[bitsadmin getreplyprogress](bitsadmin-getreplyprogress.md)  
[bitsadmin getsecurityflags](bitsadmin-getsecurityflags.md)  
[bitsadmin getstate](bitsadmin-getstate.md)  
[bitsadmin gettemporaryname](bitsadmin-gettemporaryname.md)  
[bitsadmin gettype](bitsadmin-gettype.md)  
[bitsadmin getvalidationstate](bitsadmin-getvalidationstate.md)  
[bitsadmin help](bitsadmin-help.md)  
[bitsadmin info](bitsadmin-info.md)  
[bitsadmin list](bitsadmin-list.md)  
[bitsadmin listfiles](bitsadmin-listfiles.md)  
moniteur [Bitsadmin makecustomheaderswriteonly](bitsadmin-makecustomheaderswriteonly.md)
[Bitsadmin](bitsadmin-monitor.md)  
[bitsadmin nowrap](bitsadmin-nowrap.md)  
[bitsadmin peercaching](bitsadmin-peercaching.md)  
[bitsadmin peers](bitsadmin-peers.md)  
[bitsadmin rawreturn](bitsadmin-rawreturn.md)  
[bitsadmin removeclientcertificate](bitsadmin-removeclientcertificate.md)  
[bitsadmin removecredentials](bitsadmin-removecredentials.md)  
[bitsadmin replaceremoteprefix](bitsadmin-replaceremoteprefix.md)  
[bitsadmin reset](bitsadmin-reset.md)  
[bitsadmin resume](bitsadmin-resume.md)  
[bitsadmin setaclflag](bitsadmin-setaclflag.md)  
[bitsadmin setclientcertificatebyid](bitsadmin-setclientcertificatebyid.md)  
[bitsadmin setclientcertificatebyname](bitsadmin-setclientcertificatebyname.md)  
[bitsadmin setcredentials](bitsadmin-setcredentials.md)  
[bitsadmin setcustomheaders](bitsadmin-setcustomheaders.md)  
[bitsadmin setdescription](bitsadmin-setdescription.md)  
[bitsadmin setdisplayname](bitsadmin-setdisplayname.md)  
[bitsadmin sethelpertoken](bitsadmin-sethelpertoken.md)  
[bitsadmin sethelpertokenflags](bitsadmin-sethelpertokenflags.md)  
[Bitsadmin sethttpmethod](bitsadmin-sethttpmethod.md)
[Bitsadmin setmaxdownloadtime](bitsadmin-setmaxdownloadtime.md)  
[bitsadmin setminretrydelay](bitsadmin-setminretrydelay.md)  
[bitsadmin setnoprogresstimeout](bitsadmin-setnoprogresstimeout.md)  
[bitsadmin setnotifycmdline](bitsadmin-setnotifycmdline.md)  
[bitsadmin setnotifyflags](bitsadmin-setnotifyflags.md)  
[bitsadmin setpeercachingflags](bitsadmin-setpeercachingflags.md)  
[bitsadmin setpriority](bitsadmin-setpriority.md)  
[bitsadmin setproxysettings](bitsadmin-setproxysettings.md)  
[bitsadmin setreplyfilename](bitsadmin-setreplyfilename.md)  
[bitsadmin setsecurityflags](bitsadmin-setsecurityflags.md)  
[bitsadmin setvalidationstate](bitsadmin-setvalidationstate.md)  
[bitsadmin suspend](bitsadmin-suspend.md)  
[bitsadmin takeownership](bitsadmin-takeownership.md)  
[bitsadmin transfer](bitsadmin-transfer.md)  
[bitsadmin util](bitsadmin-util.md)  
[bitsadmin wrap](bitsadmin-wrap.md)  
