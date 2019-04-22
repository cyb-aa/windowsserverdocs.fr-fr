---
title: Commandes Windows
description: Commandes Windows
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c703d07c-8227-4e86-94a6-8ef390f94cdc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/22/2018
ms.prod: windows-server-threshold
ms.openlocfilehash: 4cc9bc5c288eb063f333fa598dbb3511f7be5966
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820470"
---
# <a name="windows-commands"></a>Commandes Windows

Toutes les versions prises en charge de Windows (serveur et client) possèdent un ensemble de commandes de la console Win32 intégrées.

Cet ensemble de la documentation décrit les commandes de Windows que vous pouvez utiliser pour automatiser les tâches à l’aide de scripts ou outils de script.

Pour trouver des informations sur une commande spécifique, dans le menu suivant A-Z, cliquez sur la lettre qui commence par la commande, puis cliquez sur le nom de commande.

[UN](#BKMK_a) |
[B](#BKMK_b) | 
[C](#BKMK_c) | 
[D](#BKMK_d) | 
[E](#BKMK_e)  | 
 [F](#BKMK_f) | 
[G](#BKMK_g) | 
[H](#BKMK_h) | 
[JE](#BKMK_i)  |
 [J](#BKMK_j) | 
[K](#BKMK_k) | 
[L](#BKMK_l) | 
[M](#BKMK_m) | 
[N](#BKMK_n)  | 
 [O](#BKMK_o) | 
[P](#BKMK_p) | 
[Q](#BKMK_q) | 
[R](#BKMK_r)  | 
 [S](#BKMK_s) | 
[T](#BKMK_t) | 
[U](#BKMK_u) | 
[V](#BKMK_v)  | 
 [W](#BKMK_w) | 
[X](#BKMK_x) | 
[Y](#BKMK_y) | 
[Z](#BKMK_z)

## <a name="BKMK_PREREQ"></a>Conditions préalables
Les informations contenues dans ce fichier PDF s’applique à :

-   Windows Server 2019
-   Windows Server (canal semi-annuel)
-   Windows Server 2016
-   Windows Server 2012 R2
-   Windows Server 2012 
-   Windows Server 2008 R2
-   Windows Server 2008
-   Windows 10
-   Windows 8.1

### <a name="BKMK_OVR"></a>Vue d’ensemble du shell de commande
L’interface de commande a été le premier shell intégré à Windows pour automatiser les tâches de routine, telles que la gestion des comptes utilisateur ou les sauvegardes nocturnes, avec les fichiers de commandes (.bat). Avec Windows Script Host, vous pouvez exécuter des scripts plus sophistiqués dans l’interface de commande. Pour plus d’informations, consultez [cscript](cscript.md) ou [wscript](wscript.md). Vous pouvez effectuer des opérations plus efficacement à l’aide de scripts que vous pouvez le faire à l’aide de l’interface utilisateur. Scripts acceptent toutes les commandes qui sont disponibles en ligne de commande.

Windows a deux interfaces de commande : L’interface de commande et [PowerShell](https://docs.microsoft.com/powershell/scripting/powershell-scripting?view=powershell-6). Chaque shell est un logiciel qui fournit une communication directe entre vous et le système d’exploitation ou l’application, en fournissant un environnement pour automatiser les opérations informatiques.

PowerShell a été conçu pour étendre les fonctionnalités de l’interface de commande pour exécuter des commandes PowerShell appelés applets de commande. Applets de commande sont semblables aux commandes de Windows, mais fournit un langage de script plus extensible. Vous pouvez exécuter des commandes de Windows et les applets de commande PowerShell dans Powershell, mais l’interface de commande peut uniquement exécuter les commandes Windows et pas les applets de commande PowerShell.

Le plus robuste et à jour Windows automation, nous recommandons à l’aide de PowerShell au lieu de commandes de Windows ou Windows Script Host pour Windows automation. 
> [!NOTE]
>Vous pouvez également télécharger et installer [PowerShell Core](https://docs.microsoft.com/powershell/scripting/whats-new/what-s-new-in-powershell-core-60?view=powershell-6), la version open source de PowerShell. 

> [!CAUTION]
> Une modification incorrecte du Registre peut endommager gravement votre système. Avant d’apporter les modifications suivantes au Registre, vous devez sauvegarder toutes vos données importantes sur l’ordinateur.

> [!NOTE]
> Pour activer ou désactiver des fichiers et répertoires saisie semi-automatique du nom dans l’interface de commande sur une session d’ouverture de session utilisateur ou ordinateur, exécutez **regedit.exe** et définissez les éléments suivants **valeur reg_DWOrd**:
> 
> HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\completionChar\reg_DWOrd
> 
> Pour définir le **reg_DWOrd** , utilisez la valeur hexadécimale d’un caractère de contrôle pour une fonction particulière (par exemple, **0 9** est onglet et **0 08** est le retour arrière). Paramètres spécifiés par l’utilisateur sont prioritaires sur les paramètres de l’ordinateur, et les options de ligne de commande sont prioritaires sur les paramètres du Registre.

## <a name="BKMK_CmdRef"></a>Référence de ligne de commande A à Z
Pour trouver des informations sur une commande spécifique de Windows, dans le menu suivant A-Z, cliquez sur la lettre qui commence par la commande, puis cliquez sur le nom de commande.

[UN](#BKMK_a) |
[B](#BKMK_b) | 
[C](#BKMK_c) | 
[D](#BKMK_d) | 
[E](#BKMK_e)  | 
 [F](#BKMK_f) | 
[G](#BKMK_g) | 
[H](#BKMK_h) | 
[JE](#BKMK_i)  |
 [J](#BKMK_j) | 
[K](#BKMK_k) | 
[L](#BKMK_l) | 
[M](#BKMK_m) | 
[N](#BKMK_n)  | 
 [O](#BKMK_o) | 
[P](#BKMK_p) | 
[Q](#BKMK_q) | 
[R](#BKMK_r)  | 
 [S](#BKMK_s) | 
[T](#BKMK_t) | 
[U](#BKMK_u) | 
[V](#BKMK_v)  | 
 [W](#BKMK_w) | 
[X](#BKMK_x) | 
[Y](#BKMK_y) | 
[Z](#BKMK_z)

### <a name="BKMK_a"></a>A
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

### <a name="BKMK_b"></a>B
-   [bcdboot](bcdboot.md)
-   [bcdedit](bcdedit.md)
-   [bdehdcfg](bdehdcfg.md)
-   [bitsadmin](bitsadmin.md)
  -   [Bitsadmin addfile](bitsadmin-addfile.md)
  -   [bitsadmin addfileset](bitsadmin-addfileset.md)
  -   [Bitsadmin addfilewithranges](bitsadmin-addfilewithranges.md)
  -   [Bitsadmin Annuler](bitsadmin-cancel.md)
  -   [Bitsadmin terminée](bitsadmin-complete.md)
  -   [Bitsadmin créer](bitsadmin-create.md)
  -   [Bitsadmin getaclflags](bitsadmin-getaclflags.md)
  -   [bitsadmin getbytestotal](bitsadmin-getbytestotal.md)
  -   [bitsadmin getbytestransferred](bitsadmin-getbytestransferred.md)
  -   [Bitsadmin getcompletiontime](bitsadmin-getcompletiontime.md)
  -   [Bitsadmin getcreationtime](bitsadmin-getcreationtime.md)
  -   [bitsadmin getdescription](bitsadmin-getdescription.md)
  -   [Bitsadmin getdisplayname](bitsadmin-getdisplayname.md)
  -   [Bitsadmin geterror](bitsadmin-geterror.md)
  -   [Bitsadmin geterrorcount](bitsadmin-geterrorcount.md)
  -   [Bitsadmin getfilestotal](bitsadmin-getfilestotal.md)
  -   [Bitsadmin getfilestransferred](bitsadmin-getfilestransferred.md)
  -   [bitsadmin getminretrydelay](bitsadmin-getminretrydelay.md)
  -   [Bitsadmin getmodificationtime](bitsadmin-getmodificationtime.md)
  -   [Bitsadmin getnoprogresstimeout](bitsadmin-getnoprogresstimeout.md)
  -   [Bitsadmin getnotifycmdline](bitsadmin-getnotifycmdline.md)
  -   [Bitsadmin getnotifyflags](bitsadmin-getnotifyflags.md)
  -   [Bitsadmin getnotifyinterface](bitsadmin-getnotifyinterface.md)
  -   [Bitsadmin getowner](bitsadmin-getowner.md)
  -   [priorité de get bitsadmin](bitsadmin-getpriority.md)
  -   [bitsadmin getproxybypasslist](bitsadmin-getproxybypasslist.md)
  -   [bitsadmin getproxylist](bitsadmin-getproxylist.md)
  -   [Bitsadmin getproxyusage](bitsadmin-getproxyusage.md)
  -   [bitsadmin getreplydata](bitsadmin-getreplydata.md)
  -   [Bitsadmin getreplyfilename](bitsadmin-getreplyfilename.md)
  -   [Bitsadmin getreplyprogress](bitsadmin-getreplyprogress.md)
  -   [Bitsadmin getstate](bitsadmin-getstate.md)
  -   [Bitsadmin gettype](bitsadmin-gettype.md)
  -   [Bitsadmin aide](bitsadmin-help.md)
  -   [Bitsadmin info](bitsadmin-info.md)
  -   [liste de bitsadmin](bitsadmin-list.md)
  -   [Bitsadmin listfiles](bitsadmin-listfiles.md)
  -   [Moniteur de bitsadmin](bitsadmin-monitor.md)
  -   [Bitsadmin nowrap](bitsadmin-nowrap.md)
  -   [Bitsadmin rawreturn](bitsadmin-rawreturn.md)
  -   [Bitsadmin removecredentials](bitsadmin-removecredentials.md)
  -   [Bitsadmin replaceremoteprefix](bitsadmin-replaceremoteprefix.md)
  -   [Bitsadmin réinitialisation](bitsadmin-reset.md)
  -   [Bitsadmin resume](bitsadmin-resume.md)
  -   [Bitsadmin setaclflag](bitsadmin-setaclflag.md)
  -   [Bitsadmin setcredentials](bitsadmin-setcredentials.md)
  -   [Bitsadmin setdescription](bitsadmin-setdescription.md)
  -   [Bitsadmin setdisplayname](bitsadmin-setdisplayname.md)
  -   [Bitsadmin setminretrydelay](bitsadmin-setminretrydelay.md)
  -   [Bitsadmin setnoprogresstimeout](bitsadmin-setnoprogresstimeout.md)
  -   [Bitsadmin setnotifycmdline](bitsadmin-setnotifycmdline.md)
  -   [Bitsadmin setnotifyflags](bitsadmin-setnotifyflags.md)
  -   [Bitsadmin setpriority](bitsadmin-setpriority.md)
  -   [Bitsadmin setproxysettings](bitsadmin-setproxysettings.md)
  -   [Bitsadmin setreplyfilename](bitsadmin-setreplyfilename.md)
  -   [suspendre bitsadmin](bitsadmin-suspend.md)
  -   [Bitsadmin takeownership](bitsadmin-takeownership.md)
  -   [Bitsadmin transfert](bitsadmin-transfer.md)
  -   [Bitsadmin util](bitsadmin-util.md)
  -   [retour à la ligne bitsadmin](bitsadmin-wrap.md)
-   [bootcfg](bootcfg.md)
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
-   [break](break_1.md)

### <a name="BKMK_c"></a>C
-   [cacls](cacls_1.md)
-   [call](call.md)
-   [cd](cd.md)
-   [certreq](certreq_1.md)
-   [certutil](certutil.md)
-   [change](change.md)
  -   [changer d’ouverture de session](change-logon.md)
  -   [modifier le port](change-port.md)
  -   [modifier l’utilisateur](change-user.md)
-   [chcp](chcp.md)
-   [chdir](chdir_1.md)
-   [chglogon](chglogon.md)
-   [chgport](chgport.md)
-   [chgusr](chgusr.md)
-   [chkdsk](chkdsk.md)
-   [chkntfs](chkntfs.md)
-   [choice](choice.md)
-   [cipher](cipher.md)
-   [clip](clip.md)
-   [cls](cls.md)
-   [Cmd](Cmd.md)
-   [cmdkey](cmdkey.md)
-   [cmstp](cmstp.md)
-   [color](color.md)
-   [comp](comp.md)
-   [compact](compact.md)
-   [convert](convert.md)
-   [copy](copy.md)
-   [cprofile](cprofile.md)
-   [cscript](cscript.md)

### <a name="BKMK_d"></a>D
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

### <a name="BKMK_e"></a>E
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

### <a name="BKMK_f"></a>F
-   [fc](fc.md)
-   [find](find.md)
-   [findstr](findstr.md)
-   [finger](finger.md)
-   [flattemp](flattemp.md)
-   [fondue](fondue.md)
-   [for](for.md)
-   [forfiles](forfiles.md)
-   [format](format.md)
-   [freedisk](freedisk.md)
-   [fsutil](fsutil.md)
  -   [fsutil 8dot3name](fsutil-8dot3name.md) 
  -   [comportement de fsutil](fsutil-behavior.md) 
  -   [fichier de fsutil](fsutil-file.md)
  -   [fsutil fsinfo](fsutil-fsinfo.md)
  -   [fsutil hardlink](fsutil-hardlink.md)
  -   [fsutil objectid](fsutil-objectid.md)
  -   [fsutil quota](fsutil-quota.md)
  -   [réparation de fsutil](fsutil-repair.md)
  -   [fsutil reparsepoint](fsutil-reparsepoint.md)
  -   [ressources de fsutil](fsutil-resource.md)
  -   [fsutil sparse](fsutil-sparse.md)
  -   [la hiérarchisation fsutil](fsutil-tiering.md)
  -   [transaction de fsutil](fsutil-transaction.md)
  -   [fsutil usn](fsutil-usn.md)
  -   [fsutil volume](fsutil-volume.md)
  -   [fsutil wim](fsutil-wim.md)
-   [ftp](ftp.md)
-   [ftype](ftype.md)
-   [fveupdate](fveupdate.md)

### <a name="BKMK_g"></a>G
-   [getmac](getmac.md)
-   [gettype](gettype.md)
-   [goto](goto.md)
-   [gpfixup](gpfixup.md)
-   [gpresult](gpresult.md)
-   [gpupdate](gpupdate.md)
-   [graftabl](graftabl.md)

### <a name="BKMK_h"></a>H
-   [help](help.md)
-   [helpctr](helpctr.md)
-   [hostname](hostname.md)

### <a name="BKMK_i"></a>I
-   [icacls](icacls.md)
-   [if](if.md)
-   [inuse](inuse.md)
-   [ipconfig](ipconfig.md)
-   [ipxroute](ipxroute.md)
-   [irftp](irftp.md)

### <a name="BKMK_j"></a>J
-   [jetpack](jetpack.md)

### <a name="BKMK_k"></a>K
-   [klist](klist.md)
-   [ksetup](ksetup.md)
  -   [ksetup:setrealm](ksetup-setrealm.md)
  -   [ksetup:mapuser](ksetup-mapuser.md)
  -   [ksetup:addkdc](ksetup-addkdc.md)
  -   [ksetup:delkdc](ksetup-delkdc.md)
  -   [ksetup:addkpasswd](ksetup-addkpasswd.md)
  -   [ksetup:delkpasswd](ksetup-delkpasswd.md)
  -   [ksetup:server](ksetup-server.md)
  -   [ksetup:setcomputerpassword](ksetup-setcomputerpassword.md)
  -   [ksetup:removerealm](ksetup-removerealm.md)
  -   [ksetup:domain](ksetup-domain.md)
  -   [ksetup:changepassword](ksetup-changepassword.md)
  -   [ksetup:listrealmflags](ksetup-listrealmflags.md)
  -   [ksetup:setrealmflags](ksetup-setrealmflags.md)
  -   [ksetup:addrealmflags](ksetup-addrealmflags.md)
  -   [ksetup:delrealmflags](ksetup-delrealmflags.md)
  -   [ksetup:dumpstate](ksetup-dumpstate.md)
  -   [ksetup:addhosttorealmmap](ksetup-addhosttorealmmap.md)
  -   [ksetup:delhosttorealmmap](ksetup-delhosttorealmmap.md)
  -   [ksetup:setenctypeattr](ksetup-setenctypeattr.md)
  -   [ksetup:getenctypeattr](ksetup-getenctypeattr.md)
  -   [ksetup:addenctypeattr](ksetup-addenctypeattr.md)
  -   [ksetup:delenctypeattr](ksetup-delenctypeattr.md) 
-   [ktmutil](ktmutil.md)
-   [ktpass](ktpass.md)

### <a name="BKMK_l"></a>L
-   [label](label.md)
-   [lodctr](lodctr.md)
-   [logman](logman.md)
  -   [créer de logman](logman-create.md)
  -   [requête de logman](logman-query.md)
  -   [logman start & 124 ; arrêter](logman-start-stop.md)
  -   [logman delete](logman-delete.md)
  -   [mise à jour de logman](logman-update.md)
  -   [Logman import & 124 ; exportation](logman-import-export.md)
-   [logoff](logoff.md)
-   [lpq](lpq.md)
-   [lpr](lpr.md)

### <a name="BKMK_m"></a>M
-   [macfile](macfile.md)
-   [makecab](makecab.md)
-   [manage-bde](manage-bde.md)
  -   [gérer-bde : état](manage-bde-status.md)
  -   [gérer-bde : sur](manage-bde-on.md)
  -   [gérer-bde : désactivé](manage-bde-off.md)
  -   [manage-bde: pause](manage-bde-pause.md)
  -   [gérer-bde : reprendre](manage-bde-resume.md)
  -   [gérer-bde : verrouillage](manage-bde-lock.md)
  -   [gérer-bde : déverrouiller](manage-bde-unlock.md)
  -   [manage-bde: autounlock](manage-bde-autounlock.md)
  -   [gérer-bde : protecteurs](manage-bde-protectors.md)
  -   [manage-bde: tpm](manage-bde-tpm.md)
  -   [gérer-bde : setidentifier](manage-bde-setidentifier.md)
  -   [gérer-bde : ForceRecovery](manage-bde-forcerecovery.md)
  -   [gérer-bde : changepassword](manage-bde-changepassword.md)
  -   [gérer-bde : changepin met](manage-bde-changepin.md)
  -   [gérer-bde : changekey](manage-bde-changekey.md)
  -   [gérer-bde : KeyPackage](manage-bde-keypackage.md)
  -   [gérer-bde : mise à niveau](manage-bde-upgrade.md)
  -   [gérer-bde : WipeFreeSpace](manage-bde-wipefreespace.md)
-   [mapadmin](mapadmin.md)
-   [Md](Md.md)
-   [mkdir](mkdir.md)
-   [mklink](mklink.md)
-   [mmc](mmc.md)
-   [mode](mode.md)
-   [plus](more.md)
-   [mount](mount.md)
-   [mountvol](mountvol.md)
-   [move](move.md)
-   [mqbkup](mqbkup.md)
-   [mqsvc](mqsvc.md)
-   [mqtgsvc](mqtgsvc.md)
-   [msdt](msdt.md)
-   [msg](msg.md)
-   [msiexec](msiexec.md)
-   [msinfo32](msinfo32.md)
-   [mstsc](mstsc.md)

### <a name="BKMK_n"></a>N
-   [nbtstat](nbtstat.md)
-   [netcfg](netcfg.md)
-   [netsh](netsh.md)
-   [netstat](netstat.md)
-   [Impression de NET](net-print.md)
-   [nfsadmin](nfsadmin.md)
-   [nfsshare](nfsshare.md)
-   [nfsstat](nfsstat.md)
-   [nlbmgr](nlbmgr.md)
-   [nslookup](nslookup.md)
  -   [commande de sortie de Nslookup](nslookup-exit-command.md)
  -   [commande doigt de Nslookup](nslookup-finger-command.md)
  -   [nslookup help](nslookup-help.md)
  -   [nslookup ls](nslookup-ls.md)
  -   [nslookup lserver](nslookup-lserver.md)
  -   [nslookup root](nslookup-root.md)
  -   [nslookup server](nslookup-server.md)
  -   [nslookup set](nslookup-set.md)
  -   [nslookup définir tout](nslookup-set-all.md)
  -   [nslookup set, classe](nslookup-set-class.md)
  -   [nslookup set d2](nslookup-set-d2.md)
  -   [débogage de jeu de Nslookup](nslookup-set-debug.md)
  -   [domaine de jeu de Nslookup](nslookup-set-domain.md)
  -   [nslookup définie le port](nslookup-set-port.md)
  -   [nslookup set querytype](nslookup-set-querytype.md)
  -   [nslookup set recurse](nslookup-set-recurse.md)
  -   [nouvelle tentative de jeu de Nslookup](nslookup-set-retry.md)
  -   [racine du jeu de Nslookup](nslookup-set-root.md)
  -   [recherche de jeu de Nslookup](nslookup-set-search.md)
  -   [nslookup set srchlist](nslookup-set-srchlist.md)
  -   [délai d’expiration du jeu de Nslookup](nslookup-set-timeout.md)
  -   [définie le type de Nslookup](nslookup-set-type.md)
  -   [nslookup set vc](nslookup-set-vc.md)
  -   [vue de Nslookup](nslookup-view.md)
-   [ntbackup](ntbackup.md)
-   [ntcmdprompt](ntcmdprompt.md)
-   [ntfrsutl](ntfrsutl.md)

### <a name="BKMK_o"></a>O
-   [openfiles](openfiles.md)

### <a name="BKMK_p"></a>P
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

### <a name="BKMK_q"></a>Q
-   [qappsrv](qappsrv.md)
-   [qprocess](qprocess.md)
-   [requête](query.md)
-   [quser](quser.md)
-   [qwinsta](qwinsta.md)

### <a name="BKMK_r"></a>R
-   [rcp](rcp.md)
-   [rd](rd.md)
-   [rdpsign](rdpsign.md)
-   [recover](recover.md)
-   [reg](reg.md)
  -   [Ajout de reg](reg-add.md)
  -   [comparaison de reg](reg-compare.md)
  -   [reg copy](reg-copy.md)
  -   [reg delete](reg-delete.md)
  -   [reg export](reg-export.md)
  -   [reg import](reg-import.md)
  -   [charge de reg](reg-load.md)
  -   [reg query](reg-query.md)
  -   [restauration de reg](reg-restore.md)
  -   [reg enregistrer](reg-save.md)
  -   [reg unload](reg-unload.md)
-   [regini](regini.md)
-   [regsvr32](regsvr32.md)
-   [relog](relog.md)
-   [rem](rem.md)
-   [ren](ren.md)
-   [rename](rename.md)
-   [repair-bde](repair-bde.md)
-   [replace](replace.md)
-   [Réinitialiser la session](reset-session.md)
-   [rexec](rexec.md)
-   [risetup](risetup.md)
-   [rmdir](rmdir.md)
-   [robocopy](robocopy.md)
-   [route_ws2008](route_ws2008.md)
-   [rpcinfo](rpcinfo.md)
-   [rpcping](rpcping.md)
-   [rsh](rsh.md)
-   [rundll32](rundll32.md)
-   [rwinsta](rwinsta.md)

### <a name="BKMK_s"></a>S
-   [schtasks](schtasks.md)
-   [scwcmd](Scwcmd.md)
  -   [scwcmd: analyze](scwcmd-analyze.md)
  -   [Scwcmd : configurer](scwcmd-configure.md)
  -   [scwcmd: register](scwcmd-register.md) 
  -   [scwcmd: rollback](scwcmd-rollback.md) 
  -   [scwcmd: transform](scwcmd-transform.md) 
  -   [Scwcmd : vue](scwcmd-view.md) 
-   [secedit](secedit.md)
  -   [secedit:analyze](secedit-analyze.md)
  -   [secedit:configure](secedit-configure.md)
  -   [secedit:export](secedit-export.md)
  -   [secedit:generaterollback](secedit-generaterollback.md)
  -   [secedit:import](secedit-import.md)
  -   [secedit:validate](secedit-validate.md)
-   [serverceipoptin](serverceipoptin.md)
-   [Servermanagercmd](Servermanagercmd.md)
-   [serverweroptin](serverweroptin.md)
-   [set](set_1.md)
-   [setlocal](setlocal.md)
-   [setx](setx.md)
-   [sfc](sfc.md)
-   [shadow](shadow.md)
-   [shift](shift.md)
-   [showmount](showmount.md)
-   [shutdown](shutdown.md)
-   [sort](sort.md)
-   [start](start.md)
-   [subst](subst.md)
-   [sxstrace](sxstrace.md)
-   [sysocmgr](sysocmgr.md)
-   [systeminfo](systeminfo.md)

### <a name="BKMK_t"></a>T
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

### <a name="BKMK_u"></a>U
-   [unlodctr](unlodctr_1.md)

### <a name="BKMK_v"></a>V
-   [ver](ver.md)
-   [verifier](verifier.md)
-   [verify](verify_1.md)
-   [vol](vol.md)
-   [vssadmin](vssadmin.md)- 

### <a name="BKMK_w"></a>W
-   [waitfor](waitfor.md)
-   [wbadmin](wbadmin.md)
  -   [Activer la sauvegarde WBADMIN](wbadmin-enable-backup.md)
  -   [désactiver la sauvegarde WBADMIN](wbadmin-disable-backup.md)
  -   [Démarrer la sauvegarde WBADMIN](wbadmin-start-backup.md)
  -   [tâche d’arrêt WBADMIN](wbadmin-stop-job.md)
  -   [WBADMIN get versions](wbadmin-get-versions.md)
  -   [WBADMIN get éléments](wbadmin-get-items.md)
  -   [WBADMIN start recovery](wbadmin-start-recovery.md)
  -   [WBADMIN get état](wbadmin-get-status.md)
  -   [WBADMIN get disques](wbadmin-get-disks.md)
  -   [WBADMIN start systemstaterecovery](wbadmin-start-systemstaterecovery.md)
  -   [WBADMIN start systemstatebackup](wbadmin-start-systemstatebackup.md)
  -   [WBADMIN delete systemstatebackup](wbadmin-delete-systemstatebackup.md)
  -   [WBADMIN start sysrecovery](wbadmin-start-sysrecovery.md)
  -   [catalogue de restauration WBADMIN](wbadmin-restore-catalog.md)
  -   [WBADMIN delete catalogue](wbadmin-delete-catalog.md)
-   [wdsutil](wdsutil.md)
-   [wecutil](wecutil.md)
-   [wevtutil](wevtutil.md)
-   [where](where_1.md)
-   [whoami](whoami.md)
-   [winnt](winnt.md)
-   [winnt32](winnt32.md)
-   [winpop](winpop.md)
-   [winrs](winrs.md)
-   [wlbs](wlbs_1.md)
-   [wmic](wmic.md)
-   [wscript](wscript.md)

### <a name="BKMK_x"></a>X
-   [xcopy](xcopy.md)
