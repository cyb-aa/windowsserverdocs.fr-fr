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

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Vous pouvez utiliser **nfsadmin** pour gérer le serveur pour NFS et le client pour NFS.  
  
## <a name="syntax"></a>Syntaxe  
**nfsadmin server** `[`*ComputerName*`] [` @ No__t-4u *nom d’utilisateur*`[` @ no__t-7p *mot de passe*`]]` 0L  
  
**nfsadmin server** `[`*ComputerName*`] [` @ No__t-4u *nom d’utilisateur* `[` @ no__t-7p *mot de passe*`]]` 0R 1*client* 3 tout @ no__t-14  
  
**nfsadmin server** `[`*ComputerName*`] [` @ No__t-4u *nom d’utilisateur* `[` @ no__t-7p *mot de passe*`]] {`start 0 Stop @ no__t-11  
  
**nfsadmin server** `[`*ComputerName*`] [` @ no__t-4U *nom_utilisateur* `[` @ no__t-7p *mot de passe*@no__t- *9 config @no__t*-11  
  
**nfsadmin server** `[`*ComputerName*`] [` @ no__t-4U *nom_utilisateur* `[` @ no__t-7p *mot de passe*`]]` CreateGroup *nom*  
  
**nfsadmin server** `[`*ComputerName*`] [` @ no__t-4U *nom_utilisateur* `[` @ no__t-7p *mot de passe*`]]` listgroups  
  
**nfsadmin server** `[`*ComputerName*`] [` @ no__t-4U *nom_utilisateur* `[` @ no__t-7p *mot de passe*`]]` *nom* de l’DeleteGroup  
  
**nfsadmin server** `[`*ComputerName*`] [` @ No__t-4u *nom d’utilisateur* `[` @ no__t-7p *mot de passe*`]]` renamegroup *OldName NouveauNom*  
  
**nfsadmin server** `[`*ComputerName*`] [` @ no__t-4U *nom_utilisateur* `[` @ no__t-7p *Password*`]]` AddMembers *Name Host*1  
  
**nfsadmin server** `[`*ComputerName*`] [` @ no__t-4U *nom_utilisateur* `[` @ no__t-7p *mot de passe*`]]` ListMembers  
  
**nfsadmin server** `[`*ComputerName*`] [` @ no__t-4U *nom_utilisateur* `[` @ no__t-7p *Password*`]]` deletemembers *Group Host*1  
  
**nfsadmin client** `[`*ComputerName*`] [` @ No__t-4u *nom d’utilisateur* `[` @ no__t-7p *mot de passe*`]] {`start 0 Stop @ no__t-11  
  
**nfsadmin client** `[`*ComputerName*`] [` @ no__t-4U *nom_utilisateur* `[` @ no__t-7p *mot de passe*@no__t- *9 config @no__t*-11  
  
## <a name="description"></a>Description  
L’utilitaire de commande **nfsadmin** @ no__t-1Line administre Server for NFS ou client for NFS sur l’ordinateur local ou distant exécutant Microsoft Services for Network File System \(NFS @ no__t-3. Si vous êtes connecté avec un compte qui ne dispose pas des privilèges requis, vous pouvez spécifier un nom d’utilisateur et un mot de passe d’un compte qui exécute. L’action effectuée par **nfsadmin** dépend des arguments de commande que vous fournissez.  
  
En plus des options et des arguments de commande service @ no__t-0specific, **nfsadmin** accepte les éléments suivants :  
  
*nomd’ordinateur*  
Spécifie l’ordinateur distant que vous voulez administrer. Vous pouvez spécifier l’ordinateur à l’aide d’un nom Windows Internet Name Service \(WINS @ no__t-1 ou d’un nom de domaine DNS \(DNS @ no__t-3 ou d’une adresse IP \(IP @ no__t-5.  
  
**@no__t-** *nom d’utilisateur* 1U  
Spécifie le nom d’utilisateur de l’utilisateur dont les informations d’identification doivent être utilisées. Il peut être nécessaire d’ajouter le nom de domaine au nom d’utilisateur sous la forme <em>domaine</em> **\\** nom<em>d’utilisateur</em>  
  
*mot de passe* **\-P**  
Spécifie le mot de passe de l’utilisateur spécifié à l’aide de l’option **\-U** . Si vous spécifiez l’option **\-U** , mais que vous omettez l’option **\-P** , vous êtes invité à entrer le mot de passe de l’utilisateur.  
  
#### <a name="administering-server-for-nfs"></a>Administration de Server pour NFS  
Utilisez la commande **nfsadmin server** pour administrer le serveur pour NFS. L’action spécifique que le **serveur** prend en dépend de l’option de commande ou de l’argument que vous spécifiez :  
  
**@no__t 1L**  
répertorie tous les verrous détenus par les clients.  
  
**\-R** {*client* | **All**}  
Libère les verrous détenus par un *client* ou, si **tous** sont spécifiés, par tous les clients.  
  
**start**  
démarre le serveur pour le service NFS.  
  
**stop**  
Arrête le service serveur pour NFS.  
  
**config**  
Spécifie les paramètres généraux pour le serveur pour NFS. Vous devez fournir au moins l’une des options suivantes avec l’argument de commande **config** :  
  
**mapsvr @ no__t-1**<em>serveur</em>  
Définit *le serveur en* tant que serveur de mappage de noms d’utilisateurs pour le serveur pour NFS. Bien que cette option continue d’être prise en charge pour la compatibilité avec les versions antérieures, vous devez utiliser l’utilitaire **sfuadmin** à la place.  
  
**auditlocation @ no__t-1**{**EventLog** | **file**@no__t **-5 @no__t**-7**None**}  
Spécifie si les événements seront audités et où les événements seront enregistrés. L’un des arguments suivants est obligatoire.  
  
**événements**  
Spécifie que les événements audités seront enregistrés uniquement dans le journal des applications observateur d’événements.  
  
**file**  
Spécifie que les événements audités seront enregistrés uniquement dans le fichier spécifié par la **configuration fname**.  
  
**versions**  
Spécifie que les événements audités seront enregistrés dans le journal des applications observateur d’événements ainsi que le fichier spécifié par la **configuration fname**.  
  
**None**  
Spécifie que les événements ne seront pas audités.  
  
**fname @ no__t-1**<em>fichier</em>  
Définit le fichier *spécifié par le fichier en* tant que fichier d’audit. La valeur par défaut est% sfudir% @no__t -0log\\nfssvr.log  
  
**fsize @ no__t-1**\=*taille*  
Définit la *taille* maximale en mégaoctets du fichier d’audit. La taille maximale par défaut est de 7 Mo.  
  
**audit @ no__t-1**\[ **\+** | **\-** \]**montage** \=0**2**3**5**6 @no__t de**lecture** -18 @no__t-**20**1 **@no__ t-23**4**Write** 6**8**9**1**2**create** 4**6**7**9**0**Delete** 2 **@no__ t-44**5**7**8**verrouillage** 0**2**3**5**6**tout**  
Spécifie les événements à enregistrer. Pour commencer l’enregistrement d’un événement, tapez un signe plus \( **\+** \) avant le nom de l’événement ; pour arrêter l’enregistrement d’un événement, tapez un signe moins \( **\-** \) avant le nom de l’événement. Si le signe est omis, le signe plus est utilisé. N’utilisez pas **All** avec un autre nom d’événement.  
  
**lockperiod @ no__t-1**<em>secondes</em>  
Spécifie le nombre de secondes pendant lequel le service serveur pour NFS attend la récupération des verrous après qu’une connexion au serveur pour NFS a été perdue, puis rétablie ou après le redémarrage du service serveur pour NFS.  
  
Portmapprotocol @ no__t-0 {TCP | UDP | TCP @ no__t-1UDP  
Spécifie les protocoles de transport pris en charge par portmap. Le paramètre par défaut est **TCP @ no__t-1UDP**.  
  
mountprotocol @ no__t-0 {TCP | UDP | TCP @ no__t-1UDP}  
Spécifie le montage pris en charge par les protocoles de transport. Le paramètre par défaut est **TCP @ no__t-1UDP**.  
  
nfsprotocol @ no__t-0 {TCP | UDP | TCP @ no__t-1UDP}  
Spécifie les protocoles de transport que le système de fichiers réseau \(NFS @ no__t-1 prend en charge. Le paramètre par défaut est **TCP @ no__t-1UDP**  
  
nlmprotocol @ no__t-0 {TCP | UDP | TCP @ no__t-1UDP}  
Spécifie les protocoles de transport que le gestionnaire de verrous réseau \(NLM @ no__t-1 prend en charge. Le paramètre par défaut est **TCP @ no__t-1UDP**.  
  
nsmprotocol @ no__t-0 {TCP | UDP | TCP @ no__t-1UDP}  
Spécifie les protocoles de transport que le gestionnaire d’État réseau \(NSM @ no__t-1 prend en charge. Le paramètre par défaut est **TCP @ no__t-1UDP**.  
  
**enableV3 @ no__t-1**{**Oui** | **non**}  
Spécifie si les protocoles NFS version 3 seront pris en charge. La valeur par défaut est **Oui**.  
  
**renewauth @ no__t-1**{**Oui** | **non**}  
Spécifie si les connexions clientes doivent être réauthentifiées après la période spécifiée par la **configuration renewauthinterval**. Le paramètre par défaut est **non**.  
  
**renewauthinterval @ no__t-1**<em>secondes</em>  
Spécifie le nombre de secondes qui s’écoulent avant qu’un client ne soit forcé à être authentifié si la **configuration renewauth** est définie sur **Oui**. La valeur par défaut est 600 secondes.  
  
**dircache @ no__t-1**<em>taille</em>  
Spécifie la taille en kilo-octets du cache de répertoire. Le nombre spécifié comme *Size* doit être un multiple de 4 et 128. La taille par défaut de l’annuaire @ no__t-0cache est de 128 Ko.  
  
**translationfile**\= @ no__t-2file @ no__t-3  
Spécifie un fichier contenant des informations de mappage pour remplacer des caractères dans les noms de fichiers lors de leur déplacement de Windows @ no__t-0based vers des systèmes de fichiers UNIX @ no__t-1based. Si le *fichier* n’est pas spécifié, la traduction des caractères de nom de fichier est désactivée. Si la valeur de **translationfile** est modifiée, vous devez redémarrer le serveur pour que la modification prenne effet.  
  
**dotfileshidden**\= {**Yes** | **non**}  
Spécifie si les fichiers créés avec des noms commençant par un point \(. \) sont marqués comme étant masqués dans le système de fichiers Windows et, par conséquent, masqués par les clients NFS. Le paramètre par défaut est **non**.  
  
**casesensitivelookups @ no__t-1**{**Oui** | **non**}  
Spécifie si les recherches dans l’annuaire respectent la casse \(requiring correspondance exacte de la casse des caractères @ no__t-1.  
  
Vous devez également désactiver le cas du noyau Windows @ no__t-0insensitivity pour que le service serveur pour NFS prenne en charge les noms de fichiers de cas @ no__t-1sensitive. Vous pouvez désactiver le cas du noyau Windows @ no__t-0insensitivity en effaçant la clé de Registre suivante sur 0 :  
  
HKLM @ no__t-0SYSTEM @ no__t-1CurrentControlSet @ no__t-2Control @ no__t-3Session Manager @ no__t-4kernel  
  
DWOrd ObCaseInsensitive   
  
> [!IMPORTANT]  
> Cette section s’applique uniquement à Windows Server 2008 R2, Windows Server 2008 et Windows Server 2003. Cette section ne s’applique pas à Windows Server 2012 R2 ou Windows Server 2012.  
  
**NTFScase @ no__t-1**{**Lower** | **supérieur** | **Preserve**}  
Spécifie si la casse des caractères dans les noms de fichiers dans le système de fichiers NTFS est retournée en minuscules, en majuscules ou sous la forme stockée dans le répertoire. La valeur par défaut est **Preserve**. Ce paramètre ne peut pas être modifié si **casesensitivelookups** est défini sur **Oui**.  
  
*nom* de CreateGroup  
crée un nouveau groupe client, en lui attribuant le *nom*spécifié.  
  
**listgroups**  
Affiche les noms de tous les groupes de clients.  
  
*nom* de DeleteGroup  
supprime le groupe client spécifié par *Name*.  
  
**renamegroup** *OldName NewName*  
modifie le nom du groupe de clients spécifié par *OldName* en *NewName*  
  
**AddMembers** *nom*de l’hôte \[... \]  
Ajoute l' *hôte* au groupe client spécifié par *nom*.  
  
*nom* de la ListMembers  
répertorie les ordinateurs hôtes dans le groupe de clients spécifiés par *nom*.  
  
*hôte de groupe*deletemembers \[... \]  
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
  
<em>mode</em> **de no__t-1**  
-   Spécifie le mode d’autorisation par défaut pour les fichiers créés sur les serveurs Network File System \(NFS @ no__t-1. L’argument *mode* se compose de trois chiffres de 0 à 7 \(inclusive @ no__t-2 représentant les autorisations par défaut accordées à l’utilisateur, au groupe et à d’autres \(respectively @ no__t-4. Les chiffres sont convertis en autorisations UNIX @ no__t-0style comme suit : 0 @ no__t-0none, 1 @ no__t-1x, 2 @ no__t-2W, 3 @ no__t-3wx, 4 @ no__t-4R, 5 @ no__t-5RX, 6 @ no__t-6RW et 7 @ no__t-7rwx. Par exemple, **FileAccess @ no__t-1750** accorde l’autorisation rwx au propriétaire, l’autorisation RX au groupe et aucune autorisation d’accès aux autres utilisateurs.  
  
**mapsvr @ no__t-1**<em>serveur</em>  
Définit le *serveur* en tant que serveur de mappage de noms d’utilisateurs pour le client pour NFS. Bien que cette option continue d’être prise en charge pour la compatibilité avec les versions antérieures, vous devez utiliser l’utilitaire **sfuadmin** à la place.  
  
**mtype @ no__t-1**{**Hard** | **Soft**}  
Spécifie le type de montage par défaut. Pour un montage manuel, le client pour NFS continue à réessayer un RPC qui a échoué jusqu’à ce qu’il aboutisse. Pour un montage logiciel, le client pour NFS retourne un échec à l’application appelante après une nouvelle tentative d’appel, le nombre de fois spécifié par l’option **Retry** .  
  
**nouvelle tentative @ no__t-1**  
Spécifie le nombre de tentatives d’établir une connexion pour un montage conditionnel. Cette valeur doit être comprise entre 1 et 10 inclus. La valeur par défaut est 1.  
  
**délai d’expiration @ no__t-1**<em>seconde</em>  
Spécifie le nombre de secondes à attendre pour une connexion @no__t appel de procédure 0remote-no__t-1. Cette valeur doit être 0,8, 0,9, ou un entier compris entre 1 et 60 inclus. La valeur par défaut est 0,8.  
  
**Protocole @ no__t-1 {TCP | UDP | TCP @ no__t-2UDP}**  
Spécifie les protocoles de transport pris en charge par le client. Le paramètre par défaut est **TCP @ no__t-1UDP**  
  
**rsize @ no__t-1**<em>taille</em>  
Spécifie la taille, en kilo-octets, de la mémoire tampon de lecture. Cette valeur peut être 0,5, 1, 2, 4, 8, 16 ou 32. La valeur par défaut est 32.  
  
**wsize @ no__t-1**<em>taille</em>  
Spécifie la taille, en kilo-octets, de la mémoire tampon d’écriture. Cette valeur peut être 0,5, 1, 2, 4, 8, 16 ou 32. La valeur par défaut est 32.  
  
**Perf @ no__t-1default**  
Restaure les valeurs par défaut des paramètres de performances suivants :  
  
-   **mtype**  
  
-   **réessayez**  
  
-   **timeout**  
  
-   **rsize**  
  
-   **wsize**  
  
<em>mode</em> **de no__t-1**  
Spécifie le mode d’autorisation par défaut pour les fichiers créés sur les serveurs Network File System \(NFS @ no__t-1. L’argument *mode* se compose de trois chiffres de 0 à 7 \(inclusive @ no__t-2 représentant les autorisations par défaut accordées à l’utilisateur, au groupe et à d’autres \(respectively @ no__t-4. Les chiffres sont convertis en autorisations UNIX @ no__t-0style comme suit : 0 @ no__t-0none, 1 @ no__t-1x, 2 @ no__t-2W, 3 @ no__t-3wx, 4 @ no__t-4R, 5 @ no__t-5RX, 6 @ no__t-6RW et 7 @ no__t-7rwx. Par exemple, **FileAccess @ no__t-1750** accorde l’autorisation rwx au propriétaire, l’autorisation RX au groupe et aucune autorisation d’accès aux autres utilisateurs.  
  
Si vous ne spécifiez pas l’option ou l’argument de commande, **nfsadmin client** affiche les paramètres de configuration actuels du client pour NFS.  
  

