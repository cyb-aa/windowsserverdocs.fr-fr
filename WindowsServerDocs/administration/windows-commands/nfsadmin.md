---
title: nfsadmin
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7375b2cf-c6b8-45b5-abf6-6c10e462defd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2658cf610e4328d382b9224f4230d68a022d1cc3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373218"
---
# <a name="nfsadmin"></a>nfsadmin

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous pouvez utiliser **nfsadmin** pour gérer le serveur pour NFS et le client pour NFS.  
  
## <a name="syntax"></a>Syntaxe  
**nfsadmin server** `[`*ComputerName*`] [`\-u *nom_utilisateur*`[`\-p *mot_de_passe*`]]` \-l  
  
**nfsadmin server** `[`*ComputerName*`] [`\-u *nom_utilisateur* `[`\-p *mot_de_passe*`]]` \-r `{`*client* `|` tous`}`  
  
**nfsadmin server** `[`*nom_ordinateur*`] [`\-u *nom d’utilisateur* `[`\-p *mot de passe*`]] {`démarrer `|` arrêter`}`  
  
**nfsadmin server** `[`*ComputerName*`] [`\-u *nom d’utilisateur* `[`\-p *mot de passe*`]]` *option* de configuration`[...]`  
  
**nfsadmin server** `[`*ComputerName*`] [`\-u *nom_utilisateur* `[`\-p *mot_de_passe*`]]` *nom* CreateGroup  
  
**nfsadmin server** `[`*ComputerName*`] [`\-u *nom_utilisateur* `[`\-p *mot_de_passe*`]]` listgroups  
  
**nfsadmin server** `[`*ComputerName*`] [`\-u *nom_utilisateur* `[`\-p *mot_de_passe*`]]` *nom* de DeleteGroup  
  
**nfsadmin server** `[`*ComputerName*`] [`\-u *nom_utilisateur* `[`\-p *mot_de_passe*`]]` renamegroup *OldName NewName*  
  
**nfsadmin server** `[`*nom_ordinateur*`] [`\-u nom *d’utilisateur* `[`\-p *mot de passe*`]]` *hôte de nom* AddMembers`[...]`  
  
**nfsadmin server** `[`*ComputerName*`] [`\-u *nom_utilisateur* `[`\-p *mot_de_passe*`]]` ListMembers  
  
**nfsadmin server** `[`*nom_ordinateur*`] [`\-u *nom d’utilisateur* `[`\-p *mot de passe*`]]` *hôte de groupe* deletemembers`[...]`  
  
**nfsadmin client** `[`*nom_ordinateur*`] [`\-u *nom d’utilisateur* `[`\-p *mot de passe*`]] {`démarrer `|` arrêter`}`  
  
**nfsadmin client** `[`*nom_ordinateur*`] [`\-u *nom d’utilisateur* `[`\-p *mot de passe*`]]` *option* de configuration`[...]`  
  
## <a name="description"></a>Description  
L’utilitaire de ligne de commande **nfsadmin**\-Utility administre Server for NFS ou client for NFS sur l’ordinateur local ou distant exécutant Microsoft Services for Network File System \(NFS\). Si vous êtes connecté avec un compte qui ne dispose pas des privilèges requis, vous pouvez spécifier un nom d’utilisateur et un mot de passe d’un compte qui exécute. L’action effectuée par **nfsadmin** dépend des arguments de commande que vous fournissez.  
  
En plus du service\-des arguments de commande et des options spécifiques, **nfsadmin** accepte les éléments suivants :  
  
*nomd’ordinateur*  
Spécifie l’ordinateur distant que vous voulez administrer. Vous pouvez spécifier l’ordinateur à l’aide d’un service Windows Internet Name Service \(nom du\) WINS ou d’un système de noms de domaine \(nom de l'\) DNS ou de l’adresse IP \(IP\).  
  
**\-u** *nom d’utilisateur*  
Spécifie le nom d’utilisateur de l’utilisateur dont les informations d’identification doivent être utilisées. Il peut être nécessaire d’ajouter le nom de domaine au nom d’utilisateur sous la forme <em>domaine</em> **\\** <em>nom d'</em> utilisateur  
  
*mot de passe* **\-p**  
Spécifie le mot de passe de l’utilisateur spécifié à l’aide de l’option **\-u** . Si vous spécifiez l’option **\-u** mais omettez l’option **\-p** , vous êtes invité à entrer le mot de passe de l’utilisateur.  
  
#### <a name="administering-server-for-nfs"></a>Administration de Server pour NFS  
Utilisez la commande **nfsadmin server** pour administrer le serveur pour NFS. L’action spécifique que le **serveur** prend en dépend de l’option de commande ou de l’argument que vous spécifiez :  
  
**\-l**  
répertorie tous les verrous détenus par les clients.  
  
**\-r** {*client* | **tout**}  
Libère les verrous détenus par un *client* ou, si **tous** sont spécifiés, par tous les clients.  
  
**start**  
démarre le serveur pour le service NFS.  
  
**stop**  
Arrête le service serveur pour NFS.  
  
**config**  
Spécifie les paramètres généraux pour le serveur pour NFS. Vous devez fournir au moins l’une des options suivantes avec l’argument de commande **config** :  
  
<em>serveur</em> de **\=mapsvr**  
Définit *le serveur en* tant que serveur de mappage de noms d’utilisateurs pour le serveur pour NFS. Bien que cette option continue d’être prise en charge pour la compatibilité avec les versions antérieures, vous devez utiliser l’utilitaire **sfuadmin** à la place.  
  
**auditlocation\=** {**eventlog** | **fichier** | **les deux** | **aucun**}  
Spécifie si les événements seront audités et où les événements seront enregistrés. L’un des arguments suivants est obligatoire.  
  
**événements**  
Spécifie que les événements audités seront enregistrés uniquement dans le journal des applications observateur d’événements.  
  
**file**  
Spécifie que les événements audités seront enregistrés uniquement dans le fichier spécifié par la **configuration fname**.  
  
**versions**  
Spécifie que les événements audités seront enregistrés dans le journal des applications observateur d’événements ainsi que le fichier spécifié par la **configuration fname**.  
  
**None**  
Spécifie que les événements ne seront pas audités.  
  
<em>fichier</em> de **\=fname**  
Définit le fichier *spécifié par le fichier en* tant que fichier d’audit. La valeur par défaut est% sfudir%\\journal\\nfssvr. log  
  
**\=fsize**\=*taille*  
Définit la *taille* maximale en mégaoctets du fichier d’audit. La taille maximale par défaut est de 7 Mo.  
  
**audit\=** \[ **\+** | **\-** \]**mount** \[ **\+** | **\-** \]**lecture** \[ **\+|\-** \]**écriture** \[ **\+** | **\-** \]**créer** \[ **\+|\-\]** **Supprimer** \[ **\+** | **\-** \]**verrouillage** \[ **\+** | **\-** \]**tout**  
Spécifie les événements à enregistrer. Pour commencer l’enregistrement d’un événement, tapez un signe plus \( **\+** \) avant le nom de l’événement ; pour arrêter l’enregistrement d’un événement, tapez un signe moins \( **\-** \) avant le nom de l’événement. Si le signe est omis, le signe plus est utilisé. N’utilisez pas **All** avec un autre nom d’événement.  
  
**lockperiod\=** <em>secondes</em>  
Spécifie le nombre de secondes pendant lequel le service serveur pour NFS attend la récupération des verrous après qu’une connexion au serveur pour NFS a été perdue, puis rétablie ou après le redémarrage du service serveur pour NFS.  
  
Portmapprotocol\={TCP | UDP | TCP\+UDP  
Spécifie les protocoles de transport pris en charge par portmap. Le paramètre par défaut est **TCP\+UDP**.  
  
mountprotocol\={TCP | UDP | TCP\+UDP}  
Spécifie le montage pris en charge par les protocoles de transport. Le paramètre par défaut est **TCP\+UDP**.  
  
nfsprotocol\={TCP | UDP | TCP\+UDP}  
Spécifie les protocoles de transport que le système de fichiers réseau \(NFS\) prend en charge. Le paramètre par défaut est **TCP\+UDP**  
  
nlmprotocol\={TCP | UDP | TCP\+UDP}  
Spécifie les protocoles de transport que le gestionnaire de verrous réseau \(\) NLM prend en charge. Le paramètre par défaut est **TCP\+UDP**.  
  
nsmprotocol\={TCP | UDP | TCP\+UDP}  
Spécifie les protocoles de transport que le gestionnaire d’État réseau \(NSM\) prend en charge. Le paramètre par défaut est **TCP\+UDP**.  
  
**enableV3\=** {**Yes** | **no**}  
Spécifie si les protocoles NFS version 3 seront pris en charge. La valeur par défaut est **Oui**.  
  
**renewauth\=** {**Yes** | **no**}  
Spécifie si les connexions clientes doivent être réauthentifiées après la période spécifiée par la **configuration renewauthinterval**. Le paramètre par défaut est **non**.  
  
**renewauthinterval\=** <em>secondes</em>  
Spécifie le nombre de secondes qui s’écoulent avant qu’un client ne soit forcé à être authentifié si la **configuration renewauth** est définie sur **Oui**. La valeur par défaut est 600 secondes.  
  
<em>taille</em> de **\=dircache**  
Spécifie la taille en kilo-octets du cache de répertoire. Le nombre spécifié comme *Size* doit être un multiple de 4 et 128. Le répertoire par défaut\-la taille du cache est de 128 Ko.  
  
fichier **translationfile**\=\[\]  
Spécifie un fichier contenant des informations de mappage pour remplacer des caractères dans les noms de fichiers lors de leur déplacement de Windows\-en fonction des systèmes de fichiers UNIX\-. Si le *fichier* n’est pas spécifié, la traduction des caractères de nom de fichier est désactivée. Si la valeur de **translationfile** est modifiée, vous devez redémarrer le serveur pour que la modification prenne effet.  
  
**dotfileshidden**\={**Yes** | **no**}  
Spécifie si les fichiers créés avec des noms commençant par un point \(.\) sont marqués comme étant masqués dans le système de fichiers Windows et, par conséquent, masqués par les clients NFS. Le paramètre par défaut est **non**.  
  
**casesensitivelookups\=** {**Yes** | **no**}  
Spécifie si les recherches dans les répertoires respectent la casse \(nécessitant une correspondance exacte de la casse des caractères\).  
  
Vous devez également désactiver le non-respect de la casse du noyau Windows\-pour que le service serveur pour NFS prenne en charge la casse\-les noms de fichiers sensibles. Vous pouvez désactiver le non-respect de la casse du noyau Windows\-en effaçant la clé de Registre suivante sur 0 :  
  
HKLM\\système\\CurrentControlSet\\contrôle\\gestionnaire de session\\noyau  
  
DWOrd ObCaseInsensitive   
  
> [!IMPORTANT]  
> Cette section s’applique uniquement à Windows Server 2008 R2, Windows Server 2008 et Windows Server 2003. Cette section ne s’applique pas à Windows Server 2012 R2 ou Windows Server 2012.  
  
**ntfscase\=** {**lower** | **supérieur** | **Preserve**}  
Spécifie si la casse des caractères dans les noms de fichiers dans le système de fichiers NTFS est retournée en minuscules, en majuscules ou sous la forme stockée dans le répertoire. La valeur par défaut est **Preserve**. Ce paramètre ne peut pas être modifié si **casesensitivelookups** est défini sur **Oui**.  
  
*nom* de CreateGroup  
crée un nouveau groupe client, en lui attribuant le *nom*spécifié.  
  
**listgroups**  
Affiche les noms de tous les groupes de clients.  
  
*nom* de DeleteGroup  
supprime le groupe client spécifié par *Name*.  
  
**renamegroup** *OldName NewName*  
modifie le nom du groupe de clients spécifié par *OldName* en *NewName*  
  
**AddMembers** *nom* de l’hôte\[...\]  
Ajoute l' *hôte* au groupe client spécifié par *nom*.  
  
*nom* de la ListMembers  
répertorie les ordinateurs hôtes dans le groupe de clients spécifiés par *nom*.  
  
\[de l' *hôte du groupe* **deletemembers** ...\]  
supprime le client spécifié par l' *hôte* du groupe de clients spécifié par le *groupe*.  
  
Si vous ne spécifiez pas l’option ou l’argument de commande, **nfsadmin server** affiche les paramètres de configuration actuels du serveur pour NFS.  
  
#### <a name="administering-client-for-nfs"></a>Administration du client pour NFS  
Utilisez la commande de **client nfsadmin** pour administrer le client pour NFS. L’action spécifique prise par le **client nfsadmin** dépend de l’argument de commande que vous spécifiez :  
  
**start**  
démarre le client pour le service NFS.  
  
**stop**  
Arrête le service client pour NFS.  
  
**config**  
Spécifie les paramètres généraux du client pour NFS. Vous devez fournir au moins l’une des options suivantes avec l’argument de commande **config** :  
  
<em>mode</em> de **\=FileAccess**  
-   Spécifie le mode d’autorisation par défaut pour les fichiers créés sur le système de fichiers réseau \(les serveurs NFS\). L’argument *mode* se compose de trois chiffres de 0 à 7 \(inclus\) représentant les autorisations par défaut accordées à l’utilisateur, au groupe et à d’autres \(respectivement\). Les chiffres sont convertis en autorisations UNIX\-style comme suit : 0\=aucun, 1\=x, 2\=w, 3\=WX, 4\=r, 5\=RX, 6\=RW et 7\=rwx. Par exemple, **fileaccess\=750** donne l’autorisation rwx au propriétaire, l’autorisation RX sur le groupe et aucune autorisation d’accès aux autres utilisateurs.  
  
<em>serveur</em> de **\=mapsvr**  
Définit le *serveur* en tant que serveur de mappage de noms d’utilisateurs pour le client pour NFS. Bien que cette option continue d’être prise en charge pour la compatibilité avec les versions antérieures, vous devez utiliser l’utilitaire **sfuadmin** à la place.  
  
**mtype\=** {**Hard** | **Soft**}  
Spécifie le type de montage par défaut. Pour un montage manuel, le client pour NFS continue à réessayer un RPC qui a échoué jusqu’à ce qu’il aboutisse. Pour un montage logiciel, le client pour NFS retourne un échec à l’application appelante après une nouvelle tentative d’appel, le nombre de fois spécifié par l’option **Retry** .  
  
<em>nombre</em> de **nouvelles tentatives\=**  
Spécifie le nombre de tentatives d’établir une connexion pour un montage conditionnel. Cette valeur doit être comprise entre 1 et 10 inclus. La valeur par défaut est 1.  
  
**délai d’expiration\=** <em>secondes</em>  
Spécifie le nombre de secondes d’attente d’une connexion \(appel de procédure distante\). Cette valeur doit être 0,8, 0,9, ou un entier compris entre 1 et 60 inclus. La valeur par défaut est 0,8.  
  
**\=de protocole {TCP | UDP | TCP\+UDP}**  
Spécifie les protocoles de transport pris en charge par le client. Le paramètre par défaut est **TCP\+UDP**  
  
<em>taille</em> de **\=rsize**  
Spécifie la taille, en kilo-octets, de la mémoire tampon de lecture. Cette valeur peut être 0,5, 1, 2, 4, 8, 16 ou 32. La valeur par défaut est 32.  
  
<em>taille</em> de **\=wsize**  
Spécifie la taille, en kilo-octets, de la mémoire tampon d’écriture. Cette valeur peut être 0,5, 1, 2, 4, 8, 16 ou 32. La valeur par défaut est 32.  
  
**Perf\=par défaut**  
Restaure les valeurs par défaut des paramètres de performances suivants :  
  
-   **mtype**  
  
-   **réessayez**  
  
-   **timeout**  
  
-   **rsize**  
  
-   **wsize**  
  
<em>mode</em> de **\=FileAccess**  
Spécifie le mode d’autorisation par défaut pour les fichiers créés sur le système de fichiers réseau \(les serveurs NFS\). L’argument *mode* se compose de trois chiffres de 0 à 7 \(inclus\) représentant les autorisations par défaut accordées à l’utilisateur, au groupe et à d’autres \(respectivement\). Les chiffres sont convertis en autorisations UNIX\-style comme suit : 0\=aucun, 1\=x, 2\=w, 3\=WX, 4\=r, 5\=RX, 6\=RW et 7\=rwx. Par exemple, **fileaccess\=750** donne l’autorisation rwx au propriétaire, l’autorisation RX sur le groupe et aucune autorisation d’accès aux autres utilisateurs.  
  
Si vous ne spécifiez pas l’option ou l’argument de commande, **nfsadmin client** affiche les paramètres de configuration actuels du client pour NFS.  
  

