---
title: Commandes Windows
description: Informations de référence
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c703d07c-8227-4e86-94a6-8ef390f94cdc
author: jasongerend
ms.author: jgerend
manager: dongill
ms.date: 06/26/2019
ms.prod: windows-server
ms.openlocfilehash: 7baec3bbe532bbcedb8c17628fd88d2c8eac34c6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720730"
---
# <a name="windows-commands"></a>Commandes Windows

Toutes les versions prises en charge de Windows (serveur et client) disposent d’un ensemble de commandes de console Win32 intégrées.

Cet ensemble de documentation décrit les commandes Windows que vous pouvez utiliser pour automatiser des tâches à l’aide de scripts ou d’outils de script.

Pour rechercher des informations sur une commande spécifique, dans le menu A-Z suivant, cliquez sur la lettre de départ de la commande, puis cliquez sur le nom de la commande.

[A](#a) |
[B](#b) | 
[M](#m)[D](#d) | 
[V](#v)[C](#c) |
[J](#j) | 
[W](#w)[F](#f) | 
[S](#s)[E](#e) | 
[R](#r)[G](#g) | 
[U](#u)[H](#h) | 
[X](#x) [I](#i) | 
[T](#t)[L](#l)[N](#n)[Q](#q)[O](#o)[P](#p)[K](#k)C D e F G H I J | 
K L M | 
N | 
O | 
P | 
Q R S T U V W X | | 
 | 
 | 
 | 
 | 
 | 
 | 
 | 
 O | Lettre

## <a name="prerequisites"></a>Prérequis

Les informations contenues dans cette rubrique s’appliquent aux éléments suivants :

-   Windows Server 2019
-   Windows Server (canal semi-annuel)
-   Windows Server 2016
-   Windows Server 2012 R2
-   Windows Server 2012 
-   Windows Server 2008 R2
-   Windows Server 2008
-   Windows 10
-   Windows 8.1

### <a name="command-shell-overview"></a>Vue d’ensemble de l’interface de commande

L’interface de commande était le premier Shell intégré à Windows pour automatiser les tâches de routine, telles que la gestion des comptes d’utilisateur ou les sauvegardes nocturnes, avec des fichiers de commandes (. bat). Avec Windows Script Host, vous pouvez exécuter des scripts plus sophistiqués dans l’interface de commande. Pour plus d’informations, consultez [cscript](cscript.md) ou [wscript](wscript.md). Vous pouvez effectuer des opérations plus efficacement à l’aide de scripts que si vous utilisiez l’interface utilisateur. Les scripts acceptent toutes les commandes qui sont disponibles sur la ligne de commande.

Windows dispose de deux shells de commande : l’interface de commande et [PowerShell](https://docs.microsoft.com/powershell/scripting/powershell-scripting?view=powershell-6). Chaque interpréteur de commandes est un programme logiciel qui fournit une communication directe entre vous et le système d’exploitation ou l’application, en fournissant un environnement pour automatiser les opérations informatiques.

PowerShell a été conçu pour étendre les fonctionnalités de l’interface de commande afin d’exécuter des commandes PowerShell appelées cmdlets. Les applets de commande sont similaires aux commandes Windows, mais fournissent un langage de script plus extensible. Vous pouvez exécuter des commandes Windows et des applets de commande PowerShell dans PowerShell, mais l’interface de commande peut uniquement exécuter des commandes Windows et non des applets de commande PowerShell.

Pour l’automatisation Windows à jour la plus robuste, nous vous recommandons d’utiliser PowerShell au lieu des commandes Windows ou Windows Script Host pour Windows Automation. 
> [!NOTE]
>Vous pouvez également télécharger et installer [PowerShell Core](https://docs.microsoft.com/powershell/scripting/whats-new/what-s-new-in-powershell-core-60?view=powershell-6), la version open source de PowerShell. 

> [!CAUTION]
> Une modification incorrecte du Registre peut endommager gravement votre système. Avant d’apporter les modifications suivantes au registre, vous devez sauvegarder toutes les données importantes sur l’ordinateur.

> [!NOTE]
> Pour activer ou désactiver la saisie semi-automatique des noms de fichiers et de répertoires dans l’interface de commande d’un ordinateur ou d’une session utilisateur, exécutez **regedit. exe** et définissez la **valeur de reg_DWOrd**suivante :
> 
> HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\completionChar\ reg_DWOrd
> 
> Pour définir la valeur de **reg_DWOrd** , utilisez la valeur hexadécimale d’un caractère de contrôle pour une fonction particulière (par exemple, **0 9** est Tab et **0 08** est un retour arrière). Les paramètres spécifiés par l’utilisateur sont prioritaires par rapport aux paramètres de l’ordinateur, et les options de ligne de commande sont prioritaires sur les paramètres du Registre.

## <a name="command-line-reference-a-z"></a>Référence de ligne de commande A-Z

Pour rechercher des informations sur une commande Windows spécifique, dans le menu A-Z suivant, cliquez sur la lettre de départ de la commande, puis cliquez sur le nom de la commande.

[A](#a) |
[B](#b) | 
[M](#m)[D](#d) | 
[V](#v)[C](#c) |
[J](#j) | 
[W](#w)[F](#f) | 
[S](#s)[E](#e) | 
[R](#r)[G](#g) | 
[U](#u)[H](#h) | 
[X](#x) [I](#i) | 
[T](#t)[L](#l)[N](#n)[Q](#q)[O](#o)[P](#p)[K](#k)C D e F G H I J | 
K L M | 
N | 
O | 
P | 
Q R S T U V W X | | 
 | 
 | 
 | 
 | 
 | 
 | 
 | 
 O | Lettre

### <a name="a"></a>Un
-   [append](append.md)
-   [arp](arp.md)
-   [assoc](assoc.md)
-   [at](at.md)
-   [atmadm](atmadm.md)
-   [attrib](attrib.md)
-   [auditpol](auditpol.md)
-   [autochk](autochk.md)
-   [autoconv](autoconv.md)
-   [autofmt](autofmt.md)

### <a name="b"></a>B
- [bcdboot](bcdboot.md)
- [bcdedit](bcdedit.md)
- [bdehdcfg](bdehdcfg.md)
- [bitsadmin](bitsadmin.md)
  -   [bitsadmin addfile](bitsadmin-addfile.md)
  -   [bitsadmin addfileset](bitsadmin-addfileset.md)
  -   [bitsadmin addfilewithranges](bitsadmin-addfilewithranges.md)
  -   [bitsadmin cancel](bitsadmin-cancel.md)
  -   [bitsadmin complete](bitsadmin-complete.md)
  -   [bitsadmin create](bitsadmin-create.md)
  -   [bitsadmin getaclflags](bitsadmin-getaclflags.md)
  -   [bitsadmin getbytestotal](bitsadmin-getbytestotal.md)
  -   [bitsadmin getbytestransferred](bitsadmin-getbytestransferred.md)
  -   [bitsadmin getcompletiontime](bitsadmin-getcompletiontime.md)
  -   [bitsadmin getcreationtime](bitsadmin-getcreationtime.md)
  -   [bitsadmin getdescription](bitsadmin-getdescription.md)
  -   [bitsadmin getdisplayname](bitsadmin-getdisplayname.md)
  -   [bitsadmin geterror](bitsadmin-geterror.md)
  -   [bitsadmin geterrorcount](bitsadmin-geterrorcount.md)
  -   [bitsadmin getfilestotal](bitsadmin-getfilestotal.md)
  -   [bitsadmin getfilestransferred](bitsadmin-getfilestransferred.md)
  -   [bitsadmin getminretrydelay](bitsadmin-getminretrydelay.md)
  -   [bitsadmin getmodificationtime](bitsadmin-getmodificationtime.md)
  -   [bitsadmin getnoprogresstimeout](bitsadmin-getnoprogresstimeout.md)
  -   [bitsadmin getnotifycmdline](bitsadmin-getnotifycmdline.md)
  -   [bitsadmin getnotifyflags](bitsadmin-getnotifyflags.md)
  -   [bitsadmin getnotifyinterface](bitsadmin-getnotifyinterface.md)
  -   [bitsadmin getowner](bitsadmin-getowner.md)
  -   [Bitsadmin la priorité](bitsadmin-getpriority.md)
  -   [bitsadmin getproxybypasslist](bitsadmin-getproxybypasslist.md)
  -   [bitsadmin getproxylist](bitsadmin-getproxylist.md)
  -   [bitsadmin getproxyusage](bitsadmin-getproxyusage.md)
  -   [bitsadmin getreplydata](bitsadmin-getreplydata.md)
  -   [bitsadmin getreplyfilename](bitsadmin-getreplyfilename.md)
  -   [bitsadmin getreplyprogress](bitsadmin-getreplyprogress.md)
  -   [bitsadmin getstate](bitsadmin-getstate.md)
  -   [bitsadmin gettype](bitsadmin-gettype.md)
  -   [bitsadmin help](bitsadmin-help.md)
  -   [bitsadmin info](bitsadmin-info.md)
  -   [bitsadmin list](bitsadmin-list.md)
  -   [bitsadmin listfiles](bitsadmin-listfiles.md)
  -   [bitsadmin monitor](bitsadmin-monitor.md)
  -   [bitsadmin nowrap](bitsadmin-nowrap.md)
  -   [bitsadmin rawreturn](bitsadmin-rawreturn.md)
  -   [bitsadmin removecredentials](bitsadmin-removecredentials.md)
  -   [bitsadmin replaceremoteprefix](bitsadmin-replaceremoteprefix.md)
  -   [bitsadmin reset](bitsadmin-reset.md)
  -   [bitsadmin resume](bitsadmin-resume.md)
  -   [bitsadmin setaclflag](bitsadmin-setaclflag.md)
  -   [bitsadmin setcredentials](bitsadmin-setcredentials.md)
  -   [bitsadmin setdescription](bitsadmin-setdescription.md)
  -   [bitsadmin setdisplayname](bitsadmin-setdisplayname.md)
  -   [bitsadmin setminretrydelay](bitsadmin-setminretrydelay.md)
  -   [bitsadmin setnoprogresstimeout](bitsadmin-setnoprogresstimeout.md)
  -   [bitsadmin setnotifycmdline](bitsadmin-setnotifycmdline.md)
  -   [bitsadmin setnotifyflags](bitsadmin-setnotifyflags.md)
  -   [bitsadmin setpriority](bitsadmin-setpriority.md)
  -   [bitsadmin setproxysettings](bitsadmin-setproxysettings.md)
  -   [bitsadmin setreplyfilename](bitsadmin-setreplyfilename.md)
  -   [bitsadmin suspend](bitsadmin-suspend.md)
  -   [bitsadmin takeownership](bitsadmin-takeownership.md)
  -   [Transfert Bitsadmin](bitsadmin-transfer.md)
  -   [bitsadmin util](bitsadmin-util.md)
  -   [bitsadmin wrap](bitsadmin-wrap.md)
- [bootcfg](bootcfg.md)
  -   [bootcfg addsw](bootcfg-addsw.md)
  -   [bootcfg copy](bootcfg-copy.md)
  -   [bootcfg dbg1394](bootcfg-dbg1394.md)
  -   [bootcfg debug](bootcfg-debug.md)  
  -   [bootcfg default](bootcfg-default.md)
  -   [bootcfg delete](bootcfg-delete.md)
  -   [bootcfg ems](bootcfg-ems.md)
  -   [bootcfg query](bootcfg-query.md)
  -   [bootcfg raw](bootcfg-raw.md)
  -   [bootcfg rmsw](bootcfg-rmsw.md)
  -   [bootcfg timeout](bootcfg-timeout.md)
- [break](break_1.md)

### <a name="c"></a>C
- [cacls](cacls_1.md)
- [call](call.md)
- [cd](cd.md)
- [certreq](certreq_1.md)
- [certutil](certutil.md)
- [change](change.md)
  -   [change logon](change-logon.md)
  -   [change port](change-port.md)
  -   [change user](change-user.md)
- [chcp](chcp.md)
- [chdir](chdir_1.md)
- [chglogon](chglogon.md)
- [chgport](chgport.md)
- [chgusr](chgusr.md)
- [chkdsk](chkdsk.md)
- [chkntfs](chkntfs.md)
- [choice](choice.md)
- [cipher](cipher.md)
- [cleanmgr](cleanmgr.md)
- [clip](clip.md)
- [cls](cls.md)
- [Cmd](Cmd.md)
- [cmdkey](cmdkey.md)
- [cmstp](cmstp.md)
- [color](color.md)
- [comp](comp.md)
- [compact](compact.md)
- [convert](convert.md)
- [copy](copy.md)
- [cprofile](cprofile.md)
- [cscript](cscript.md)

### <a name="d"></a>D
-   [date](date.md)
-   [dcgpofix](dcgpofix.md)
-   [defrag](defrag.md)
-   [del](del.md)
-   [dfsrmig](dfsrmig.md)
-   [diantz](diantz.md)
-   [dir](dir.md)
-   [diskcomp](diskcomp.md)
-   [diskcopy](diskcopy.md)
-   [diskpart](diskpart.md)
-   [diskperf](diskperf.md)
-   [diskraid](diskraid.md)
-   [diskshadow](diskshadow.md)
-   [dispdiag](dispdiag.md)
-   [dnscmd](Dnscmd.md)
-   [doskey](doskey.md)
-   [driverquery](driverquery.md)

### <a name="e"></a>E
-   [echo](echo.md)
-   [edit](edit.md)
-   [endlocal](endlocal.md)
-   [erase](erase.md)
-   [eventcreate](eventcreate.md)
-   [eventquery](eventquery.md)
-   [eventtriggers](eventtriggers.md)
-   [evntcmd](Evntcmd.md)
-   [exit](exit_2.md)
-   [expand](expand.md)
-   [extract](extract.md)

### <a name="f"></a>F
- [fc](fc.md)
- [find](find.md)
- [findstr](findstr.md)
- [finger](finger.md)
- [flattemp](flattemp.md)
- [fondue](fondue.md)
- [for](for.md)
- [forfiles](forfiles.md)
- [format](format.md)
- [freedisk](freedisk.md)
- [fsutil](fsutil.md)
  -   [fsutil 8dot3name](fsutil-8dot3name.md) 
  -   [fsutil behavior](fsutil-behavior.md) 
  -   [fsutil file](fsutil-file.md)
  -   [fsutil fsinfo](fsutil-fsinfo.md)
  -   [fsutil hardlink](fsutil-hardlink.md)
  -   [fsutil objectid](fsutil-objectid.md)
  -   [fsutil quota](fsutil-quota.md)
  -   [fsutil repair](fsutil-repair.md)
  -   [fsutil reparsepoint](fsutil-reparsepoint.md)
  -   [fsutil resource](fsutil-resource.md)
  -   [fsutil sparse](fsutil-sparse.md)
  -   [fsutil tiering](fsutil-tiering.md)
  -   [fsutil transaction](fsutil-transaction.md)
  -   [fsutil usn](fsutil-usn.md)
  -   [fsutil volume](fsutil-volume.md)
  -   [fsutil wim](fsutil-wim.md)
- [FTP](ftp.md)
- [ftype](ftype.md)
- [fveupdate](fveupdate.md)

### <a name="g"></a>G
-   [getmac](getmac.md)
-   [gettype](gettype.md)
-   [goto](goto.md)
-   [gpfixup](gpfixup.md)
-   [gpresult](gpresult.md)
-   [gpupdate](gpupdate.md)
-   [graftabl](graftabl.md)

### <a name="h"></a>H
-   [help](help.md)
-   [helpctr](helpctr.md)
-   [hostname](hostname.md)

### <a name="i"></a>I
-   [icacls](icacls.md)
-   [if](if.md)
-   [inuse](inuse.md)
-   [ipconfig](ipconfig.md)
-   [ipxroute](ipxroute.md)
-   [irftp](irftp.md)

### <a name="j"></a>J
-   [jetpack](jetpack.md)

### <a name="k"></a>K
- [klist](klist.md)
- [ksetup](ksetup.md)
  -   [Ksetup : setrealm](ksetup-setrealm.md)
  -   [Ksetup : mapuser](ksetup-mapuser.md)
  -   [Ksetup : addkdc](ksetup-addkdc.md)
  -   [Ksetup : delkdc](ksetup-delkdc.md)
  -   [Ksetup : addkpasswd](ksetup-addkpasswd.md)
  -   [Ksetup : delkpasswd](ksetup-delkpasswd.md)
  -   [Ksetup : serveur](ksetup-server.md)
  -   [Ksetup : setcomputerpassword](ksetup-setcomputerpassword.md)
  -   [Ksetup : removerealm](ksetup-removerealm.md)
  -   [Ksetup : domaine](ksetup-domain.md)
  -   [Ksetup : ChangePassword](ksetup-changepassword.md)
  -   [Ksetup : listrealmflags](ksetup-listrealmflags.md)
  -   [Ksetup : setrealmflags](ksetup-setrealmflags.md)
  -   [Ksetup : addrealmflags](ksetup-addrealmflags.md)
  -   [Ksetup : delrealmflags](ksetup-delrealmflags.md)
  -   [Ksetup : dumpstate](ksetup-dumpstate.md)
  -   [Ksetup : addhosttorealmmap](ksetup-addhosttorealmmap.md)
  -   [Ksetup : delhosttorealmmap](ksetup-delhosttorealmmap.md)
  -   [Ksetup : setenctypeattr](ksetup-setenctypeattr.md)
  -   [Ksetup : getenctypeattr](ksetup-getenctypeattr.md)
  -   [Ksetup : addenctypeattr](ksetup-addenctypeattr.md)
  -   [Ksetup : delenctypeattr](ksetup-delenctypeattr.md) 
- [ktmutil](ktmutil.md)
- [ktpass](ktpass.md)

### <a name="l"></a>L
- [label](label.md)
- [lodctr](lodctr.md)
- [logman](logman.md)
  -   [logman create](logman-create.md)
  -   [logman query](logman-query.md)
  -   [logman start &124 ; erreur](logman-start-stop.md)
  -   [logman delete](logman-delete.md)
  -   [logman update](logman-update.md)
  -   [logman Import &124 ; exporter](logman-import-export.md)
- [logoff](logoff.md)
- [lpq](lpq.md)
- [lpr](lpr.md)

### <a name="m"></a>M
- [macfile](macfile.md)
- [makecab](makecab.md)
- [manage-bde](manage-bde.md)
  -   [Manage-bde : Status](manage-bde-status.md)
  -   [Manage-bde : on](manage-bde-on.md)
  -   [Manage-bde : OFF](manage-bde-off.md)
  -   [Manage-bde : pause](manage-bde-pause.md)
  -   [Manage-bde : Resume](manage-bde-resume.md)
  -   [Manage-bde : Lock](manage-bde-lock.md)
  -   [Manage-bde : Unlock](manage-bde-unlock.md)
  -   [Manage-bde : autounlock](manage-bde-autounlock.md)
  -   [Manage-bde : protecteurs](manage-bde-protectors.md)
  -   [Manage-bde : TPM](manage-bde-tpm.md)
  -   [Manage-bde : SetIdentifier](manage-bde-setidentifier.md)
  -   [Manage-bde : ForceRecovery](manage-bde-forcerecovery.md)
  -   [Manage-bde : ChangePassword](manage-bde-changepassword.md)
  -   [Manage-bde : changepin](manage-bde-changepin.md)
  -   [Manage-bde : ChangeKey](manage-bde-changekey.md)
  -   [Manage-bde : keypackage](manage-bde-keypackage.md)
  -   [Manage-bde : mettre à niveau](manage-bde-upgrade.md)
  -   [Manage-bde : WipeFreeSpace](manage-bde-wipefreespace.md)
- [mapadmin](mapadmin.md)
- [Md](Md.md)
- [mkdir](mkdir.md)
- [mklink](mklink.md)
- [mmc](mmc.md)
- [mode](mode.md)
- [more](more.md)
- [mount](mount.md)
- [mountvol](mountvol.md)
- [move](move.md)
- [mqbkup](mqbkup.md)
- [mqsvc](mqsvc.md)
- [mqtgsvc](mqtgsvc.md)
- [msdt](msdt.md)
- [msg](msg.md)
- [msiexec](msiexec.md)
- [msinfo32](msinfo32.md)
- [mstsc](mstsc.md)

### <a name="n"></a>N
- [nbtstat](nbtstat.md)
- [netcfg](netcfg.md)
- [netsh](netsh.md)
- [netstat](netstat.md)
- [Net print](net-print.md)
- [nfsadmin](nfsadmin.md)
- [nfsshare](nfsshare.md)
- [nfsstat](nfsstat.md)
- [nlbmgr](nlbmgr.md)
- [nslookup](nslookup.md)
  -   [commande nslookup Exit](nslookup-exit-command.md)
  -   [commande nslookup Finger](nslookup-finger-command.md)
  -   [nslookup help](nslookup-help.md)
  -   [nslookup ls](nslookup-ls.md)
  -   [nslookup lserver](nslookup-lserver.md)
  -   [nslookup root](nslookup-root.md)
  -   [nslookup server](nslookup-server.md)
  -   [nslookup set](nslookup-set.md)
  -   [nslookup set all](nslookup-set-all.md)
  -   [nslookup set class](nslookup-set-class.md)
  -   [nslookup set d2](nslookup-set-d2.md)
  -   [nslookup set debug](nslookup-set-debug.md)
  -   [nslookup set domain](nslookup-set-domain.md)
  -   [nslookup set port](nslookup-set-port.md)
  -   [nslookup set querytype](nslookup-set-querytype.md)
  -   [nslookup set recurse](nslookup-set-recurse.md)
  -   [nslookup set retry](nslookup-set-retry.md)
  -   [nslookup set root](nslookup-set-root.md)
  -   [nslookup set search](nslookup-set-search.md)
  -   [nslookup set srchlist](nslookup-set-srchlist.md)
  -   [nslookup set timeout](nslookup-set-timeout.md)
  -   [nslookup set type](nslookup-set-type.md)
  -   [nslookup set vc](nslookup-set-vc.md)
  -   [nslookup view](nslookup-view.md)
- [ntbackup](ntbackup.md)
- [ntcmdprompt](ntcmdprompt.md)
- [ntfrsutl](ntfrsutl.md)

### <a name="o"></a>O
-   [openfiles](openfiles.md)

### <a name="p"></a>P
-   [pagefileconfig](pagefileconfig.md)
-   [path](path.md)
-   [pathping](pathping.md)
-   [pause](pause.md)
-   [pbadmin](pbadmin.md)
-   [pentnt](pentnt.md)
-   [perfmon](perfmon.md)
-   [ping](ping.md)
-   [pnpunattend](pnpunattend.md)
-   [pnputil](pnputil.md)
-   [popd](popd.md)
-   [PowerShell](PowerShell.md)
-   [PowerShell_ise](PowerShell_ise.md)
-   [print](print.md)
-   [prncnfg](prncnfg.md)
-   [prndrvr](prndrvr.md)
-   [prnjobs](prnjobs.md)
-   [prnmngr](prnmngr.md)
-   [prnport](prnport.md)
-   [prnqctl](prnqctl.md)
-   [prompt](prompt.md)
-   [pubprn](pubprn.md)
-   [pushd](pushd.md)
-   [pushprinterconnections](pushprinterconnections.md)

### <a name="q"></a>Q
-   [qappsrv](qappsrv.md)
-   [qprocess](qprocess.md)
-   [requête](query.md)
-   [quser](quser.md)
-   [qwinsta](qwinsta.md)

### <a name="r"></a>R
- [rcp](rcp.md)
- [rd](rd.md)
- [rdpsign](rdpsign.md)
- [recover](recover.md)
- [reg](reg.md)
  -   [Reg ajouter](reg-add.md)
  -   [Comparaison de Reg](reg-compare.md)
  -   [copie reg](reg-copy.md)
  -   [reg supprimer](reg-delete.md)
  -   [Reg Export](reg-export.md)
  -   [Reg Import](reg-import.md)
  -   [charge reg](reg-load.md)
  -   [Reg Query](reg-query.md)
  -   [Reg Restore](reg-restore.md)
  -   [enregistrement reg](reg-save.md)
  -   [reg unload](reg-unload.md)
- [regini](regini.md)
- [regsvr32](regsvr32.md)
- [relog](relog.md)
- [rem](rem.md)
- [ren](ren.md)
- [rename](rename.md)
- [repair-bde](repair-bde.md)
- [replace](replace.md)
- [reset session](reset-session.md)
- [rexec](rexec.md)
- [risetup](risetup.md)
- [rmdir](rmdir.md)
- [robocopy](robocopy.md)
- [route_ws2008](route_ws2008.md)
- [rpcinfo](rpcinfo.md)
- [rpcping](rpcping.md)
- [rsh](rsh.md)
- [rundll32](rundll32.md)
- [rwinsta](rwinsta.md)

### <a name="s"></a>S
- [schtasks](schtasks.md)
- [scwcmd](Scwcmd.md)
  -   [scwcmd : analyser](scwcmd-analyze.md)
  -   [scwcmd : configurer](scwcmd-configure.md)
  -   [scwcmd : Register](scwcmd-register.md) 
  -   [scwcmd : Rollback](scwcmd-rollback.md) 
  -   [scwcmd : transformer](scwcmd-transform.md) 
  -   [scwcmd : afficher](scwcmd-view.md) 
- [secedit](secedit.md)
  -   [secedit : analyser](secedit-analyze.md)
  -   [secedit : configurer](secedit-configure.md)
  -   [secedit : export](secedit-export.md)
  -   [secedit : generaterollback](secedit-generaterollback.md)
  -   [secedit : import](secedit-import.md)
  -   [secedit : valider](secedit-validate.md)
- [serverceipoptin](serverceipoptin.md)
- [Servermanagercmd](Servermanagercmd.md)
- [serverweroptin](serverweroptin.md)
- [set](set_1.md)
- [setlocal](setlocal.md)
- [setx](setx.md)
- [sfc](sfc.md)
- [shadow](shadow.md)
- [shift](shift.md)
- [showmount](showmount.md)
- [shutdown](shutdown.md)
- [sort](sort.md)
- [start](start.md)
- [subst](subst.md)
- [sxstrace](sxstrace.md)
- [sysocmgr](sysocmgr.md)
- [systeminfo](systeminfo.md)

### <a name="t"></a>T
-   [takeown](takeown.md)
-   [tapicfg](tapicfg.md)
-   [taskkill](taskkill.md)
-   [tasklist](tasklist.md)
-   [tcmsetup](tcmsetup.md)
-   [telnet](telnet.md)
-   [tftp](tftp.md)
-   [time](time.md)
-   [timeout](timeout_1.md)
-   [title](title_1.md)
-   [tlntadmn](tlntadmn.md)
-   [tpmvscmgr](tpmvscmgr.md)
-   [tracerpt](tracerpt_1.md)
-   [tracert](tracert.md)
-   [tree](tree.md)
-   [tscon](tscon.md)
-   [tsdiscon](tsdiscon.md)
-   [tsecimp](tsecimp_1.md)
-   [tskill](tskill.md)
-   [tsprof](tsprof.md)
-   [type](type.md)
-   [typeperf](typeperf.md)
-   [tzutil](tzutil.md)

### <a name="u"></a>U
-   [unlodctr](unlodctr_1.md)

### <a name="v"></a>V
-   [ver](ver.md)
-   [verifier](verifier.md)
-   [verify](verify_1.md)
-   [vol](vol.md)
-   [vssadmin](vssadmin.md)- 

### <a name="w"></a>W
- [waitfor](waitfor.md)
- [wbadmin](wbadmin.md)
  -   [Wbadmin activer la sauvegarde](wbadmin-enable-backup.md)
  -   [Wbadmin désactiver la sauvegarde](wbadmin-disable-backup.md)
  -   [Wbadmin start Backup](wbadmin-start-backup.md)
  -   [tâche d’arrêt Wbadmin](wbadmin-stop-job.md)
  -   [versions d’extraction Wbadmin](wbadmin-get-versions.md)
  -   [Wbadmin obtient les éléments](wbadmin-get-items.md)
  -   [Wbadmin start Recovery](wbadmin-start-recovery.md)
  -   [État de l’extraction de Wbadmin](wbadmin-get-status.md)
  -   [optimiser l’accès aux disques](wbadmin-get-disks.md)
  -   [Wbadmin start systemstaterecovery](wbadmin-start-systemstaterecovery.md)
  -   [Wbadmin start systemstatebackup](wbadmin-start-systemstatebackup.md)
  -   [Wbadmin Delete systemstatebackup](wbadmin-delete-systemstatebackup.md)
  -   [Wbadmin start SYSRECOVERY](wbadmin-start-sysrecovery.md)
  -   [WBADMIN RESTORE CATALOG](wbadmin-restore-catalog.md)
  -   [Wbadmin supprimer le catalogue](wbadmin-delete-catalog.md)
- [wdsutil](wdsutil.md)
- [wecutil](wecutil.md)
- [wevtutil](wevtutil.md)
- [where](where_1.md)
- [whoami](whoami.md)
- [winnt](winnt.md)
- [winnt32](winnt32.md)
- [winpop](winpop.md)
- [winrs](winrs.md)
- [wmic](wmic.md)
- [wscript](wscript.md)

### <a name="x"></a>X
-   [xcopy](xcopy.md)
