---
title: bitsadmin
description: Rubrique de commandes de Windows pour **bitsadmin** -bitsadmin est un outil de ligne de commande que vous pouvez utiliser pour créer, télécharger, ou télécharger des travaux et suivre leur progression.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: da0f05ec716cffb7d7532ebac50a091729a6bb18
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821070"
---
# <a name="bitsadmin"></a>bitsadmin

> **S’applique à**: Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10

Bitsadmin est un outil de ligne de commande que vous pouvez utiliser pour créer de téléchargement ou de tâches de téléchargement et de suivre leur progression. L’outil bitsadmin utilise des commutateurs pour identifier le travail à effectuer.  Vous pouvez appeler `bitsadmin /?` ou `bitsadmin /HELP` pour obtenir une liste de commutateurs.

La plupart des commutateurs nécessitent un \<travail\> paramètre que vous définissez pour le nom d’affichage de la tâche, ou GUID. Notez que nom d’affichage d’un travail peuvent ne pas être unique. Le **/ créer** et **/la liste** commutateurs retournent les GUID d’un travail.

Par défaut, vous pouvez accéder aux informations sur vos propres travaux. Pour accéder à des informations sur les travaux d’un autre utilisateur, vous devez disposer des privilèges d’administrateur. Si le travail a été créé dans un état avec élévation de privilèges, puis vous devez exécuter bitsadmin à partir d’une fenêtre avec élévation de privilèges ; dans le cas contraire, avoir un accès en lecture seule au travail.

La plupart des commutateurs correspondent aux méthodes dans le [interfaces BITS](/windows/desktop/bits/bits-interfaces). Pour plus d’informations supplémentaires qui peuvent être pertinentes pour l’utilisation d’un commutateur, consultez la méthode correspondante.

Utilisez les commutateurs suivants pour créer un travail, définir et récupérer les propriétés d’un travail et surveiller l’état d’un travail. Pour obtenir des exemples qui montrent comment utiliser certains de ces commutateurs pour effectuer des tâches, consultez [bitsadmin exemples](bitsadmin-examples.md).

## <a name="switches"></a>Commutateurs

[Bitsadmin addfile](bitsadmin-addfile.md)  
[bitsadmin addfileset](bitsadmin-addfileset.md)  
[Bitsadmin addfilewithranges](bitsadmin-addfilewithranges.md)  
[Bitsadmin cache](bitsadmin-cache.md)  
[Bitsadmin Annuler](bitsadmin-cancel.md)  
[Bitsadmin terminée](bitsadmin-complete.md)  
[Bitsadmin créer](bitsadmin-create.md)  
[Bitsadmin getaclflags](bitsadmin-getaclflags.md)  
[bitsadmin getbytestotal](bitsadmin-getbytestotal.md)  
[bitsadmin getbytestransferred](bitsadmin-getbytestransferred.md)  
[Bitsadmin getclientcertificate](bitsadmin-getclientcertificate.md)  
[Bitsadmin getcompletiontime](bitsadmin-getcompletiontime.md)  
[Bitsadmin getcreationtime](bitsadmin-getcreationtime.md)  
[Bitsadmin getcustomheaders](bitsadmin-getcustomheaders.md)  
[bitsadmin getdescription](bitsadmin-getdescription.md)  
[Bitsadmin getdisplayname](bitsadmin-getdisplayname.md)  
[Bitsadmin geterror](bitsadmin-geterror.md)  
[Bitsadmin geterrorcount](bitsadmin-geterrorcount.md)  
[Bitsadmin getfilestotal](bitsadmin-getfilestotal.md)  
[Bitsadmin getfilestransferred](bitsadmin-getfilestransferred.md)  
[Bitsadmin gethelpertokenflags](bitsadmin-gethelpertokenflags.md)  
[Bitsadmin gethelpertokensid](bitsadmin-gethelpertokensid.md)  
[Bitsadmin gethttpmethod](bitsadmin-gethttpmethod.md)
[bitsadmin getmaxdownloadtime](bitsadmin-getmaxdownloadtime.md)  
[bitsadmin getminretrydelay](bitsadmin-getminretrydelay.md)  
[Bitsadmin getmodificationtime](bitsadmin-getmodificationtime.md)  
[Bitsadmin getnoprogresstimeout](bitsadmin-getnoprogresstimeout.md)  
[Bitsadmin getnotifycmdline](bitsadmin-getnotifycmdline.md)  
[Bitsadmin getnotifyflags](bitsadmin-getnotifyflags.md)  
[Bitsadmin getnotifyinterface](bitsadmin-getnotifyinterface.md)  
[Bitsadmin getowner](bitsadmin-getowner.md)  
[Bitsadmin getpeercachingflags](bitsadmin-getpeercachingflags.md)  
[Bitsadmin getpriority](bitsadmin-getpriority.md)  
[bitsadmin getproxybypasslist](bitsadmin-getproxybypasslist.md)  
[bitsadmin getproxylist](bitsadmin-getproxylist.md)  
[Bitsadmin getproxyusage](bitsadmin-getproxyusage.md)  
[bitsadmin getreplydata](bitsadmin-getreplydata.md)  
[Bitsadmin getreplyfilename](bitsadmin-getreplyfilename.md)  
[Bitsadmin getreplyprogress](bitsadmin-getreplyprogress.md)  
[Bitsadmin getsecurityflags](bitsadmin-getsecurityflags.md)  
[Bitsadmin getstate](bitsadmin-getstate.md)  
[Bitsadmin gettemporaryname](bitsadmin-gettemporaryname.md)  
[Bitsadmin gettype](bitsadmin-gettype.md)  
[bitsadmin getvalidationstate](bitsadmin-getvalidationstate.md)  
[Bitsadmin aide](bitsadmin-help.md)  
[Bitsadmin info](bitsadmin-info.md)  
[liste de bitsadmin](bitsadmin-list.md)  
[Bitsadmin listfiles](bitsadmin-listfiles.md)  
[Bitsadmin makecustomheaderswriteonly](bitsadmin-makecustomheaderswriteonly.md)
[bitsadmin moniteur](bitsadmin-monitor.md)  
[Bitsadmin nowrap](bitsadmin-nowrap.md)  
[mise en cache bitsadmin](bitsadmin-peercaching.md)  
[Bitsadmin homologues](bitsadmin-peers.md)  
[Bitsadmin rawreturn](bitsadmin-rawreturn.md)  
[Bitsadmin removeclientcertificate](bitsadmin-removeclientcertificate.md)  
[Bitsadmin removecredentials](bitsadmin-removecredentials.md)  
[Bitsadmin replaceremoteprefix](bitsadmin-replaceremoteprefix.md)  
[Bitsadmin réinitialisation](bitsadmin-reset.md)  
[Bitsadmin resume](bitsadmin-resume.md)  
[Bitsadmin setaclflag](bitsadmin-setaclflag.md)  
[Bitsadmin setclientcertificatebyid](bitsadmin-setclientcertificatebyid.md)  
[Bitsadmin setclientcertificatebyname](bitsadmin-setclientcertificatebyname.md)  
[Bitsadmin setcredentials](bitsadmin-setcredentials.md)  
[Bitsadmin setcustomheaders](bitsadmin-setcustomheaders.md)  
[Bitsadmin setdescription](bitsadmin-setdescription.md)  
[Bitsadmin setdisplayname](bitsadmin-setdisplayname.md)  
[Bitsadmin sethelpertoken](bitsadmin-sethelpertoken.md)  
[Bitsadmin sethelpertokenflags](bitsadmin-sethelpertokenflags.md)  
[Bitsadmin sethttpmethod](bitsadmin-sethttpmethod.md)
[bitsadmin setmaxdownloadtime](bitsadmin-setmaxdownloadtime.md)  
[Bitsadmin setminretrydelay](bitsadmin-setminretrydelay.md)  
[Bitsadmin setnoprogresstimeout](bitsadmin-setnoprogresstimeout.md)  
[Bitsadmin setnotifycmdline](bitsadmin-setnotifycmdline.md)  
[Bitsadmin setnotifyflags](bitsadmin-setnotifyflags.md)  
[Bitsadmin setpeercachingflags](bitsadmin-setpeercachingflags.md)  
[Bitsadmin setpriority](bitsadmin-setpriority.md)  
[Bitsadmin setproxysettings](bitsadmin-setproxysettings.md)  
[Bitsadmin setreplyfilename](bitsadmin-setreplyfilename.md)  
[Bitsadmin setsecurityflags](bitsadmin-setsecurityflags.md)  
[Bitsadmin setvalidationstate](bitsadmin-setvalidationstate.md)  
[suspendre bitsadmin](bitsadmin-suspend.md)  
[Bitsadmin takeownership](bitsadmin-takeownership.md)  
[transfert de bitsadmin](bitsadmin-transfer.md)  
[Bitsadmin util](bitsadmin-util.md)  
[retour à la ligne bitsadmin](bitsadmin-wrap.md)  
