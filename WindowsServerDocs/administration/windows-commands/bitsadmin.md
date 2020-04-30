---
title: bitsadmin
description: La rubrique commandes Windows pour **Bitsadmin**, qui est un outil de ligne de commande utilisé pour créer, télécharger ou télécharger des travaux et surveiller leur progression.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4853036e-1df8-45ad-8be6-cfb097b8dd27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7a9cbf715474621b7102d0baf0c448e0ee578bf9
ms.sourcegitcommit: 1d83ca198c50eef83d105151551c6be6f308ab94
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82605559"
---
# <a name="bitsadmin"></a>bitsadmin

> **S’applique à**: Windows Server (canal semi-annuel), windows server 2016, windows server 2012 R2, windows server 2012, Windows 10

Bitsadmin est un outil de ligne de commande utilisé pour créer, télécharger ou télécharger des travaux et surveiller leur progression. L’outil Bitsadmin utilise des commutateurs pour identifier le travail à effectuer. Vous pouvez appeler `bitsadmin /?` ou `bitsadmin /help` pour obtenir une liste de commutateurs.

La plupart des commutateurs requièrent un `<job>` paramètre, que vous définissez sur le nom d’affichage du travail ou le GUID. Le nom d’affichage d’un travail n’a pas besoin d’être unique. Les commutateurs **/Create** et **/List** retournent le GUID d’un travail.

Par défaut, vous pouvez accéder à des informations sur vos propres travaux. Pour accéder aux informations des travaux d’un autre utilisateur, vous devez disposer de privilèges d’administrateur. Si le travail a été créé dans un état élevé, vous devez exécuter **Bitsadmin** à partir d’une fenêtre avec élévation de privilèges. dans le cas contraire, vous disposerez d’un accès en lecture seule au travail.

La plupart des commutateurs correspondent aux méthodes des [interfaces bits](https://docs.microsoft.com/windows/win32/bits/bits-interfaces). Pour plus d’informations sur l’utilisation d’un commutateur, consultez la méthode correspondante.

Utilisez les commutateurs suivants pour créer un travail, définir et récupérer les propriétés d’un travail, et surveiller l’état d’un travail. Pour obtenir des exemples qui montrent comment utiliser certains de ces commutateurs pour effectuer des tâches, consultez [exemples Bitsadmin](bitsadmin-examples.md).

## <a name="available-switches"></a>Commutateurs disponibles

- [Bitsadmin/AddFile](bitsadmin-addfile.md)
- [Bitsadmin/ADDFILESET](bitsadmin-addfileset.md)
- [Bitsadmin/ADDFILEWITHRANGES](bitsadmin-addfilewithranges.md)
- [Bitsadmin/cache](bitsadmin-cache.md)
- [Bitsadmin/cache/Delete](bitsadmin-cache-and-delete.md)
- [Bitsadmin/cache/DeleteUrl](bitsadmin-cache-and-deleteurl.md)
- [Bitsadmin/cache/getexpirationtime](bitsadmin-cache-and-getexpirationtime.md)
- [Bitsadmin/cache/getlimit](bitsadmin-cache-and-getlimit.md)
- [Bitsadmin/cache/Help](bitsadmin-cache-and-help.md)
- [Bitsadmin/cache/info](bitsadmin-cache-and-info.md)
- [Bitsadmin/cache/List](bitsadmin-cache-and-list.md)
- [Bitsadmin/cache/setexpirationtime](bitsadmin-cache-and-setexpirationtime.md)
- [Bitsadmin/cache/setLimit](bitsadmin-cache-and-setlimit.md)
- [Bitsadmin/cache/Clear](bitsadmin-cache-clear.md)
- [Bitsadmin/Cancel](bitsadmin-cancel.md)
- [Bitsadmin/Complete](bitsadmin-complete.md)
- [Bitsadmin/Create](bitsadmin-create.md)
- [Bitsadmin/examples](bitsadmin-examples.md)
- [Bitsadmin/GETACLFLAGS](bitsadmin-getaclflags.md)
- [Bitsadmin/getbytestotal](bitsadmin-getbytestotal.md)
- [Bitsadmin/getbytestransferred](bitsadmin-getbytestransferred.md)
- [Bitsadmin/GetClientCertificate](bitsadmin-getclientcertificate.md)
- [Bitsadmin/getcompletiontime](bitsadmin-getcompletiontime.md)
- [Bitsadmin/GetCreationTime](bitsadmin-getcreationtime.md)
- [Bitsadmin/getcustomheaders](bitsadmin-getcustomheaders.md)
- [Bitsadmin/GetDescription](bitsadmin-getdescription.md)
- [Bitsadmin/GetDisplayName](bitsadmin-getdisplayname.md)
- [Bitsadmin/GetError](bitsadmin-geterror.md)
- [Bitsadmin/GetErrorCount](bitsadmin-geterrorcount.md)
- [Bitsadmin/getfilestotal](bitsadmin-getfilestotal.md)
- [Bitsadmin/getfilestransferred](bitsadmin-getfilestransferred.md)
- [Bitsadmin/gethelpertokenflags](bitsadmin-gethelpertokenflags.md)
- [Bitsadmin/gethelpertokensid](bitsadmin-gethelpertokensid.md)
- [Bitsadmin/gethttpmethod](bitsadmin-gethttpmethod.md)
- [Bitsadmin/getmaxdownloadtime](bitsadmin-getmaxdownloadtime.md)
- [Bitsadmin/getminretrydelay](bitsadmin-getminretrydelay.md)
- [Bitsadmin/getmodificationtime](bitsadmin-getmodificationtime.md)
- [Bitsadmin/getnoprogresstimeout](bitsadmin-getnoprogresstimeout.md)
- [Bitsadmin/getnotifycmdline](bitsadmin-getnotifycmdline.md)
- [Bitsadmin/getnotifyflags](bitsadmin-getnotifyflags.md)
- [Bitsadmin/getnotifyinterface](bitsadmin-getnotifyinterface.md)
- [Bitsadmin/GetOwner](bitsadmin-getowner.md)
- [Bitsadmin/getpeercachingflags](bitsadmin-getpeercachingflags.md)
- [Bitsadmin/getPriority](bitsadmin-getpriority.md)
- [Bitsadmin/getproxybypasslist](bitsadmin-getproxybypasslist.md)
- [Bitsadmin/getproxylist](bitsadmin-getproxylist.md)
- [Bitsadmin/getproxyusage](bitsadmin-getproxyusage.md)
- [Bitsadmin/getreplydata](bitsadmin-getreplydata.md)
- [Bitsadmin/getreplyfilename](bitsadmin-getreplyfilename.md)
- [Bitsadmin/getreplyprogress](bitsadmin-getreplyprogress.md)
- [Bitsadmin/getsecurityflags](bitsadmin-getsecurityflags.md)
- [Bitsadmin/GetState](bitsadmin-getstate.md)
- [Bitsadmin/gettemporaryname](bitsadmin-gettemporaryname.md)
- [Bitsadmin/GetType](bitsadmin-gettype.md)
- [Bitsadmin/getvalidationstate](bitsadmin-getvalidationstate.md)
- [Bitsadmin/Help](bitsadmin-help.md)
- [Bitsadmin/info](bitsadmin-info.md)
- [bitsadmin/list](bitsadmin-list.md)
- [Bitsadmin/listfiles](bitsadmin-listfiles.md)
- [Bitsadmin/makecustomheaderswriteonly](bitsadmin-makecustomheaderswriteonly.md)
- [Bitsadmin/Monitor](bitsadmin-monitor.md)
- [Bitsadmin/nowrap](bitsadmin-nowrap.md)
- [Bitsadmin/peercaching](bitsadmin-peercaching.md)
- [Bitsadmin/peercaching/getconfigurationflags](bitsadmin-peercaching-and-getconfigurationflags.md)
- [Bitsadmin/peercaching/Help](bitsadmin-peercaching-and-help.md)
- [Bitsadmin/peercaching/setconfigurationflags](bitsadmin-peercaching-and-setconfigurationflags.md)
- [Bitsadmin/Peers](bitsadmin-peers.md)
- [Bitsadmin/Peers/Clear](bitsadmin-peers-and-clear.md)
- [Bitsadmin/Peers/Discover](bitsadmin-peers-and-discover.md)
- [Bitsadmin/Peers/Help](bitsadmin-peers-and-help.md)
- [Bitsadmin/Peers/List](bitsadmin-peers-and-list.md)
- [Bitsadmin/rawReturn](bitsadmin-rawreturn.md)
- [Bitsadmin/removeclientcertificate](bitsadmin-removeclientcertificate.md)
- [Bitsadmin/removecredentials](bitsadmin-removecredentials.md)
- [Bitsadmin/REPLACEREMOTEPREFIX](bitsadmin-replaceremoteprefix.md)
- [Bitsadmin/Reset](bitsadmin-reset.md)
- [Bitsadmin/Resume](bitsadmin-resume.md)
- [Bitsadmin/setaclflag](bitsadmin-setaclflag.md)
- [Bitsadmin/setclientcertificatebyid](bitsadmin-setclientcertificatebyid.md)
- [Bitsadmin/setclientcertificatebyname](bitsadmin-setclientcertificatebyname.md)
- [Bitsadmin/SetCredentials](bitsadmin-setcredentials.md)
- [Bitsadmin/setcustomheaders](bitsadmin-setcustomheaders.md)
- [Bitsadmin/SetDescription](bitsadmin-setdescription.md)
- [Bitsadmin/SetDisplayName](bitsadmin-setdisplayname.md)
- [Bitsadmin/sethelpertoken](bitsadmin-sethelpertoken.md)
- [Bitsadmin/sethelpertokenflags](bitsadmin-sethelpertokenflags.md)
- [Bitsadmin/sethttpmethod](bitsadmin-sethttpmethod.md)
- [Bitsadmin/setmaxdownloadtime](bitsadmin-setmaxdownloadtime.md)
- [Bitsadmin/setminretrydelay](bitsadmin-setminretrydelay.md)
- [Bitsadmin/setnoprogresstimeout](bitsadmin-setnoprogresstimeout.md)
- [Bitsadmin/setnotifycmdline](bitsadmin-setnotifycmdline.md)
- [Bitsadmin/setnotifyflags](bitsadmin-setnotifyflags.md)
- [Bitsadmin/setpeercachingflags](bitsadmin-setpeercachingflags.md)
- [Bitsadmin/SetPriority](bitsadmin-setpriority.md)
- [Bitsadmin/setproxysettings](bitsadmin-setproxysettings.md)
- [Bitsadmin/setreplyfilename](bitsadmin-setreplyfilename.md)
- [Bitsadmin/setsecurityflags](bitsadmin-setsecurityflags.md)
- [Bitsadmin/setvalidationstate](bitsadmin-setvalidationstate.md)
- [Bitsadmin/suspend](bitsadmin-suspend.md)
- [Bitsadmin/TakeOwnership](bitsadmin-takeownership.md)
- [Bitsadmin/Transfer](bitsadmin-transfer.md)
- [Bitsadmin/util](bitsadmin-util.md)
- [Bitsadmin/util/enableanalyticchannel](bitsadmin-util-and-enableanalyticchannel.md)
- [Bitsadmin/util/GETIEPROXY](bitsadmin-util-and-getieproxy.md)
- [Bitsadmin/util/Help](bitsadmin-util-and-help.md)
- [Bitsadmin/UTIL/REPAIRSERVICE](bitsadmin-util-and-repairservice.md)
- [Bitsadmin/util/SETIEPROXY](bitsadmin-util-and-setieproxy.md)
- [Bitsadmin/util/version](bitsadmin-util-and-version.md)
- [Bitsadmin/Wrap](bitsadmin-wrap.md)
