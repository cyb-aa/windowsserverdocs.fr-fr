---
title: nfsadmin
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7375b2cf-c6b8-45b5-abf6-6c10e462defd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c8020b028a046ead36b5f95604cd81d679861746
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723772"
---
# <a name="nfsadmin"></a>nfsadmin

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous pouvez utiliser **nfsadmin** pour gérer le serveur pour NFS et le client pour NFS.  
  
## <a name="syntax"></a>Syntaxe  
**nfsadmin server** `[` *nom_ordinateur*`] [``[``]]` *UserName* \-u nom_utilisateur\-p *mot_de_passe* l\-  
  
**nfsadmin server** `[` *nom_ordinateur*`] [` \- `{` *client* *Password* `|` *UserName* `]]` u nom_utilisateur `[`p mot_de_passe \-r client tout\-`}`  
  
**nfsadmin server** `[` *ComputerName*`] [` \- `|` *UserName* `]] {`u nom_utilisateur `[`p *mot_de_passe*Start Stop\-`}`  
  
**nfsadmin server** `[` *ComputerName*`] [` \-`]]` *Option* *Password* *UserName* u nom_utilisateur `[`p Password, option de configuration\-`[...]`  
  
**nfsadmin server** `[` *nom_ordinateur*`] [` `[``]]` *Name* *Password* *UserName* u nom_utilisateur \-p mot_de_passe CreateGroup nom\-  
  
**nfsadmin server** `[` *nom_ordinateur*`] [` `[``]]` *UserName* u nom_utilisateur \-p *mot_de_passe* listgroups\-  
  
**nfsadmin server** `[` *nom_ordinateur*`] [` \-`]]` *UserName* *Name* *Password* u nom_utilisateur `[`p mot de passe DeleteGroup nom\-  
  
**nfsadmin server** `[` *nom_ordinateur*`] [` `[` *Password* *UserName* `]]` *OldName NewName* u nom_utilisateur \-p mot_de_passe renamegroup OldName NewName\-  
  
**nfsadmin server** `[` *nom_ordinateur*`] [` `[` *Password* *UserName* `]]` *Name Host* u nom_utilisateur \-p mot_de_passe AddMembers nom hôte\-`[...]`  
  
**nfsadmin server** `[` *computerName*`] [`nom_ordinateur\-u *UserName* nom_utilisateur `[``]]` p mot *de passe ListMembers* \-  
  
**nfsadmin server** `[` *nom_ordinateur*`] [` `[` *Password* *UserName* `]]` *Group Host* u nom_utilisateur \-p mot_de_passe deletemembers groupe hôte\-`[...]`  
  
**nfsadmin client** `[` *nom_ordinateur*`] [` `[``]] {` `|` *UserName* *Password*u nom_utilisateur \-p mot de passe démarrer arrêter\-`}`  
  
**nfsadmin client** `[` *ComputerName*`] [` \-`]]` *Password* *UserName* u nom_utilisateur p password *(option de configuration)* `[`\-`[...]`  
  
## <a name="description"></a>Description  
L' **nfsadmin** utilitaire de\-ligne de commande NFSADMIN administre Server for NFS ou client for NFS sur l’ordinateur local ou distant exécutant Microsoft Services for Network File \(System\)NFS. Si vous êtes connecté avec un compte qui ne dispose pas des privilèges requis, vous pouvez spécifier un nom d’utilisateur et un mot de passe d’un compte qui exécute. L’action effectuée par **nfsadmin** dépend des arguments de commande que vous fournissez.  
  
En plus des options\-et des arguments de commande spécifiques au service, **nfsadmin** accepte les éléments suivants :  
  
*nomd’ordinateur*  
Spécifie l’ordinateur distant que vous voulez administrer. Vous pouvez spécifier l’ordinateur à l’aide d’un nom \(WINS\) Windows Internet Name Service ou d' \(un\) nom DNS DNS ou de l' \(adresse\) IP du protocole Internet.  
  
u nom *d’utilisateur* ** \-**  
Spécifie le nom d’utilisateur de l’utilisateur dont les informations d’identification doivent être utilisées. Il peut être nécessaire d’ajouter le nom de domaine au nom d’utilisateur sous la forme <em>domaine</em>**\\**<em>nom d'</em> utilisateur.  
  
*mot de passe* p ** \-**  
Spécifie le mot de passe de l’utilisateur ** \-** spécifié à l’aide de l’option u. Si vous spécifiez ** \-** l’option u mais que vous omettez l' ** \-option p** , vous êtes invité à entrer le mot de passe de l’utilisateur.  
  
#### <a name="administering-server-for-nfs"></a>Administration de Server pour NFS  
Utilisez la commande **nfsadmin server** pour administrer le serveur pour NFS. L’action spécifique que le **serveur** prend en dépend de l’option de commande ou de l’argument que vous spécifiez :  
  
**\-budget**  
répertorie tous les verrous détenus par les clients.  
  
r {*client* | **tout**} ** \-**  
Libère les verrous détenus par un *client* ou, si **tous** sont spécifiés, par tous les clients.  
  
**start**  
démarre le serveur pour le service NFS.  
  
**stop**  
Arrête le service serveur pour NFS.  
  
**package**  
Spécifie les paramètres généraux pour le serveur pour NFS. Vous devez fournir au moins l’une des options suivantes avec l’argument de commande **config** :  
  
<em>serveur</em> **mapsvr\=**  
Définit *le serveur en* tant que serveur de mappage de noms d’utilisateurs pour le serveur pour NFS. Bien que cette option continue d’être prise en charge pour la compatibilité avec les versions antérieures, vous devez utiliser l’utilitaire **sfuadmin** à la place.  
  
**auditlocation\=**{**eventlog** | **file**fichier | **both**EventLog aucun**none**} |   
Spécifie si les événements seront audités et où les événements seront enregistrés. L’un des arguments suivants est obligatoire.  
  
**événements**  
Spécifie que les événements audités seront enregistrés uniquement dans le journal des applications observateur d’événements.  
  
**txt**  
Spécifie que les événements audités seront enregistrés uniquement dans le fichier spécifié par la **configuration fname**.  
  
**versions**  
Spécifie que les événements audités seront enregistrés dans le journal des applications observateur d’événements ainsi que le fichier spécifié par la **configuration fname**.  
  
**Aucune**  
Spécifie que les événements ne seront pas audités.  
  
<em>fichier</em> **fname\=**  
Définit le fichier *spécifié par le fichier en* tant que fichier d’audit. La valeur par défaut est%\\sfudir\\% journal nfssvr. log  
  
**taille\=fsize**\=*size*  
Définit la *taille* maximale en mégaoctets du fichier d’audit. La taille maximale par défaut est de 7 Mo.  
  
**audit\=** \] **create** **all** **mount** **read** **write** **locking** **delete** montage lecture écriture créer \[supprimer **\+** \[ **\+** \] | **\-** \[ | **\-** \] **\-** | **\+** \[ \] **\-** | **\+** \[ \] **\-** | **\+** \[\]**\-**|**\+**\[ **\+** | **\-** \]  
Spécifie les événements à enregistrer. Pour commencer l’enregistrement d’un événement, tapez un \( **\+** \) signe plus avant le nom de l’événement ; pour arrêter l’enregistrement d’un événement, tapez le \( **\-** \) signe moins devant le nom de l’événement. Si le signe est omis, le signe plus est utilisé. N’utilisez pas **All** avec un autre nom d’événement.  
  
**lockperiod\=**<em>secondes</em>  
Spécifie le nombre de secondes pendant lequel le service serveur pour NFS attend la récupération des verrous après qu’une connexion au serveur pour NFS a été perdue, puis rétablie ou après le redémarrage du service serveur pour NFS.  
  
Portmapprotocol\={TCP | UDP | UDP\+TCP  
Spécifie les protocoles de transport pris en charge par portmap. Le paramètre par défaut **est\+TCP UDP**.  
  
mountprotocol\={TCP | UDP | UDP\+TCP}  
Spécifie le montage pris en charge par les protocoles de transport. Le paramètre par défaut **est\+TCP UDP**.  
  
nfsprotocol\={TCP | UDP | UDP\+TCP}  
Spécifie les protocoles de transport que \(NFS\) prend en charge. Le paramètre par défaut **est\+TCP UDP**  
  
nlmprotocol\={TCP | UDP | UDP\+TCP}  
Spécifie les protocoles de transport que \(NLM\) gère le gestionnaire de verrous réseau. Le paramètre par défaut **est\+TCP UDP**.  
  
nsmprotocol\={TCP | UDP | UDP\+TCP}  
Spécifie les protocoles de transport pris \(en\) charge par NSM dans le gestionnaire d’État réseau. Le paramètre par défaut **est\+TCP UDP**.  
  
**enableV3\=**{**Oui** | **non**}  
Spécifie si les protocoles NFS version 3 seront pris en charge. La valeur par défaut est **Oui**.  
  
**renewauth\=**{**Oui** | **non**}  
Spécifie si les connexions clientes doivent être réauthentifiées après la période spécifiée par la **configuration renewauthinterval**. Le paramètre par défaut est **non**.  
  
**renewauthinterval\=**<em>secondes</em>  
Spécifie le nombre de secondes qui s’écoulent avant qu’un client ne soit forcé à être authentifié si la **configuration renewauth** est définie sur **Oui**. La valeur par défaut est 600 secondes.  
  
<em>taille</em> de **dircache\=**  
Spécifie la taille en kilo-octets du cache de répertoire. Le nombre spécifié comme *Size* doit être un multiple de 4 et 128. La taille du\-cache de répertoire par défaut est de 128 Ko.  
  
**fichier translationfile**\=\[\]  
Spécifie un fichier contenant des informations de mappage pour remplacer des caractères dans les noms de fichiers lors\-de leur déplacement\-depuis Windows vers des systèmes de fichiers basés sur UNIX. Si le *fichier* n’est pas spécifié, la traduction des caractères de nom de fichier est désactivée. Si la valeur de **translationfile** est modifiée, vous devez redémarrer le serveur pour que la modification prenne effet.  
  
**dotfileshidden**\={**Oui** | **non**}  
Spécifie si les fichiers créés avec des noms commençant par un \(point. \) sera marqué comme masqué dans le système de fichiers Windows et, par conséquent, masqué pour les clients NFS. Le paramètre par défaut est **non**.  
  
**casesensitivelookups\=**{**Oui** | **non**}  
Spécifie si les recherches dans les répertoires \(doivent respecter la casse, ce\)qui nécessite une correspondance exacte de la casse des caractères.  
  
Vous devez également désactiver le non-respect\-de la casse du noyau Windows pour que le service serveur\-pour NFS prenne en charge les noms de fichiers sensibles à la casse. Vous pouvez désactiver le non-\-respect de la casse du noyau Windows en effaçant la clé de Registre suivante sur 0 :  
  
Noyau\\du\\gestionnaire\\\\de\\session du contrôle de CurrentControlSet du système HKLM  
  
DWOrd ObCaseInsensitive   
  
> [!IMPORTANT]  
> Cette section s’applique uniquement à Windows Server 2008 R2, Windows Server 2008 et Windows Server 2003. Cette section ne s’applique pas à Windows Server 2012 R2 ou Windows Server 2012.  
  
**NTFScase\=**{préversion**supérieure** | **preserve****inférieure** | }  
Spécifie si la casse des caractères dans les noms de fichiers dans le système de fichiers NTFS est retournée en minuscules, en majuscules ou sous la forme stockée dans le répertoire. La valeur par défaut est **Preserve**. Ce paramètre ne peut pas être modifié si **casesensitivelookups** est défini sur **Oui**.  
  
**creategroup** *nom* de CreateGroup  
crée un nouveau groupe client, en lui attribuant le *nom*spécifié.  
  
**listgroups**  
Affiche les noms de tous les groupes de clients.  
  
**deletegroup** *nom* de DeleteGroup  
supprime le groupe client spécifié par *Name*.  
  
**renamegroup** *OldName NewName*  
modifie le nom du groupe de clients spécifié par *OldName* en *NewName*  
  
nom de l' *hôte*\[de **AddMembers** ...\]  
Ajoute l' *hôte* au groupe client spécifié par *nom*.  
  
**listmembers** *nom* de la ListMembers  
répertorie les ordinateurs hôtes dans le groupe de clients spécifiés par *nom*.  
  
**deletemembers** *Group Host*hôte\[de groupe deletemembers...\]  
supprime le client spécifié par l' *hôte* du groupe de clients spécifié par le *groupe*.  
  
Si vous ne spécifiez pas l’option ou l’argument de commande, **nfsadmin server** affiche les paramètres de configuration actuels du serveur pour NFS.  
  
#### <a name="administering-client-for-nfs"></a>Administration du client pour NFS  
Utilisez la commande de **client nfsadmin** pour administrer le client pour NFS. L’action spécifique prise par le **client nfsadmin** dépend de l’argument de commande que vous spécifiez :  
  
**start**  
démarre le client pour le service NFS.  
  
**stop**  
Arrête le service client pour NFS.  
  
**package**  
Spécifie les paramètres généraux du client pour NFS. Vous devez fournir au moins l’une des options suivantes avec l’argument de commande **config** :  
  
<em>mode</em> **FileAccess\=**  
-   Spécifie le mode d’autorisation par défaut pour les fichiers créés \(sur\) les serveurs NFS NFS. L’argument *mode* se compose de trois chiffres de 0 à 7 \(inclus\) , représentant les autorisations par défaut accordées à l’utilisateur, \(au\)groupe et à d’autres, respectivement. Les chiffres traduisent les\-autorisations de style Unix comme suit\=: 0 aucun\=, 1 x\=, 2 w\=, 3 wx\=, 4 r\=, 5 RX\=, 6 RW et\=7 rwx. Par exemple, **fileaccess\=750** donne l’autorisation rwx au propriétaire, l’autorisation RX au groupe et aucune autorisation d’accès à d’autres utilisateurs.  
  
<em>serveur</em> **mapsvr\=**  
Définit le *serveur* en tant que serveur de mappage de noms d’utilisateurs pour le client pour NFS. Bien que cette option continue d’être prise en charge pour la compatibilité avec les versions antérieures, vous devez utiliser l’utilitaire **sfuadmin** à la place.  
  
**mtype\=****hard**{ | **Soft Soft**}  
Spécifie le type de montage par défaut. Pour un montage manuel, le client pour NFS continue à réessayer un RPC qui a échoué jusqu’à ce qu’il aboutisse. Pour un montage logiciel, le client pour NFS retourne un échec à l’application appelante après une nouvelle tentative d’appel, le nombre de fois spécifié par l’option **Retry** .  
  
<em>numéro</em> de **nouvelle tentative\=**  
Spécifie le nombre de tentatives d’établir une connexion pour un montage conditionnel. Cette valeur doit être comprise entre 1 et 10 inclus. La valeur par défaut est 1.  
  
**délai\=d’expiration**en<em>secondes</em>  
Spécifie le nombre de secondes d’attente pour un \(appel\)de procédure distante de connexion. Cette valeur doit être 0,8, 0,9, ou un entier compris entre 1 et 60 inclus. La valeur par défaut est 0,8.  
  
**Protocole\={TCP | UDP | UDP\+TCP}**  
Spécifie les protocoles de transport pris en charge par le client. Le paramètre par défaut **est\+TCP UDP**  
  
<em>taille</em> de **rsize\=**  
Spécifie la taille, en kilo-octets, de la mémoire tampon de lecture. Cette valeur peut être 0,5, 1, 2, 4, 8, 16 ou 32. La valeur par défaut est 32.  
  
<em>taille</em> de **wsize\=**  
Spécifie la taille, en kilo-octets, de la mémoire tampon d’écriture. Cette valeur peut être 0,5, 1, 2, 4, 8, 16 ou 32. La valeur par défaut est 32.  
  
**valeur\=par défaut perf**  
Restaure les valeurs par défaut des paramètres de performances suivants :  
  
-   **mtype**  
  
-   **réessayez**  
  
-   **timeout**  
  
-   **rsize**  
  
-   **wsize**  
  
<em>mode</em> **FileAccess\=**  
Spécifie le mode d’autorisation par défaut pour les fichiers créés \(sur\) les serveurs NFS NFS. L’argument *mode* se compose de trois chiffres de 0 à 7 \(inclus\) , représentant les autorisations par défaut accordées à l’utilisateur, \(au\)groupe et à d’autres, respectivement. Les chiffres traduisent les\-autorisations de style Unix comme suit\=: 0 aucun\=, 1 x\=, 2 w\=, 3 wx\=, 4 r\=, 5 RX\=, 6 RW et\=7 rwx. Par exemple, **fileaccess\=750** donne l’autorisation rwx au propriétaire, l’autorisation RX au groupe et aucune autorisation d’accès à d’autres utilisateurs.  
  
Si vous ne spécifiez pas l’option ou l’argument de commande, **nfsadmin client** affiche les paramètres de configuration actuels du client pour NFS.  
  

