---
title: nfsadmin
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: c4dc49e23d67ae68c598367de5a3fb0d7d6398a8
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437164"
---
# <a name="nfsadmin"></a>nfsadmin

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous pouvez utiliser **nfsadmin** pour gérer le serveur pour NFS et Client pour NFS.  
  
## <a name="syntax"></a>Syntaxe  
**nfsadmin server** `[` *computerName*`] [`\-u *nom d’utilisateur*`[`\-p *mot de passe* `]]` \-l  
  
**nfsadmin server** `[` *computerName*`] [`\-u *nom d’utilisateur* `[` \-p *mot de passe* `]]` \-r `{` *client* `|` toutes les`}`  
  
**nfsadmin server** `[` *computerName*`] [`\-u *nom d’utilisateur* `[` \-p *mot de passe* `]] {`Démarrer `|` arrêter`}`  
  
**nfsadmin server** `[` *computerName*`] [`\-u *nom d’utilisateur* `[` \-p *mot de passe* `]]` config *Option*`[...]`  
  
**nfsadmin server** `[` *computerName*`] [`\-u *nom d’utilisateur* `[` \-p *mot de passe* `]]` creategroup *nom*  
  
**nfsadmin server** `[` *computerName*`] [`\-u *nom d’utilisateur* `[` \-p *mot de passe* `]]` listgroups  
  
**nfsadmin server** `[` *computerName*`] [`\-u *nom d’utilisateur* `[` \-p *mot de passe* `]]` deletegroup *nom*  
  
**nfsadmin server** `[` *computerName*`] [`\-u *nom d’utilisateur* `[` \-p *mot de passe* `]]` renamegroup *OldName NewName*  
  
**nfsadmin server** `[` *computerName*`] [`\-u *nom d’utilisateur* `[` \-p *mot de passe* `]]` addmembers *nom d’hôte*`[...]`  
  
**nfsadmin server** `[` *computerName*`] [`\-u *nom d’utilisateur* `[` \-p *mot de passe* `]]` listmembers  
  
**nfsadmin server** `[` *computerName*`] [`\-u *nom d’utilisateur* `[` \-p *mot de passe* `]]` deletemembers *groupe hôte*`[...]`  
  
**nfsadmin client** `[` *computerName*`] [`\-u *nom d’utilisateur* `[` \-p *mot de passe* `]] {`Démarrer `|` arrêter`}`  
  
**nfsadmin client** `[` *computerName*`] [`\-u *nom d’utilisateur* `[` \-p *mot de passe* `]]` config *Option*`[...]`  
  
## <a name="description"></a>Description  
Le **nfsadmin** commande\-utilitaire de ligne d’administre serveur pour NFS ou Client pour NFS sur l’ordinateur local ou distant exécutant Microsoft Services for Network File System \(NFS\). Si vous êtes connecté avec un compte qui ne dispose pas des privilèges requis, vous pouvez spécifier un nom d’utilisateur et le mot de passe d’un compte qui en bénéficie. L’action réalisée par **nfsadmin** varie selon les arguments de commande que vous fournissez.  
  
En plus du service\-arguments de commande spécifique et les options, **nfsadmin** accepte les éléments suivants :  
  
*computerName*  
Spécifie l’ordinateur distant que vous souhaitez administrer. Vous pouvez spécifier l’ordinateur à l’aide d’un service Windows Internet Name Service \(WINS\) nom ou un système de nom de domaine \(DNS\) nom, ou par le protocole Internet \(IP\) adresse.  
  
**\-u** *UserName*  
Spécifie le nom d’utilisateur de l’utilisateur dont informations d’identification doivent être utilisées. Il peut être nécessaire d’ajouter le nom de domaine pour le nom d’utilisateur sous la forme <em>domaine</em> **\\** <em>nom d’utilisateur</em>  
  
**\-p** *mot de passe*  
Spécifie le mot de passe de l’utilisateur spécifié à l’aide de la  **\-u** option. Si vous spécifiez le  **\-u** option mais omettent le  **\-p** option, vous êtes invité au mot de passe.  
  
#### <a name="administering-server-for-nfs"></a>Administration de serveur pour NFS  
Utilisez le **nfsadmin server** commande pour administrer le serveur pour NFS. L’action spécifique qui **nfsadmin server** dépend de l’option de commande ou l’argument que vous spécifiez :  
  
**\-l**  
Répertorie tous les verrous détenus par les clients.  
  
**\-r** {*client* | **all**}  
Libère les verrous maintenus par un *client* ou, si **tous les** est spécifiée, par tous les clients.  
  
**start**  
démarre le service serveur pour NFS.  
  
**stop**  
Arrête le service serveur pour NFS.  
  
**config**  
Spécifie les paramètres généraux pour le serveur pour NFS. Vous devez fournir au moins une des options suivantes avec le **config** argument de commande :  
  
**mapsvr\=** <em>server</em>  
Jeux *server* comme serveur de mappage de noms d’utilisateur pour le serveur pour NFS. Bien que cette option continue d’être pris en charge pour la compatibilité avec les versions précédentes, vous devez utiliser le **sfuadmin** utilitaire au lieu de cela.  
  
**auditlocation\=** {**eventlog** | **file** | **both** | **none**}  
Spécifie si les événements sont audités et où les événements seront enregistrées. Un des arguments suivants est requis.  
  
**eventlog**  
Spécifie que les événements audités seront enregistrés uniquement dans le journal.  
  
**file**  
Spécifie que les événements audités seront enregistrés uniquement dans le fichier spécifié par **config fname**.  
  
**both**  
Spécifie que les événements audités seront enregistrés dans le journal d’application de l’Observateur d’événements, ainsi que le fichier spécifié par **config fname**.  
  
**None**  
Spécifie que les événements ne seront pas auditées.  
  
**fname\=** <em>file</em>  
Définit le fichier spécifié par *fichier* en tant que le fichier d’audit. La valeur par défaut est % sfudir %\\journal\\nfssvr.log  
  
**fsize\=** \=*size*  
Jeux *taille* en tant que la taille maximale en Mo du fichier d’audit. La taille maximale par défaut est 7 Mo.  
  
**audit\=** \[ **\+** | **\-** \]**monter** \[ **\+** | **\-** \] **lire** \[ **\+** | **\-** \] **écrire** \[ **\+** | **\-** \] **créer** \[ **\+** | **\-** \] **supprimer** \[ **\+** | **\-** \] **verrouillage** \[ **\+** | **\-** \] **toutes les**  
Spécifie les événements à consigner. Pour démarrer l’enregistrement d’un événement, tapez un signe \( **\+** \) avant le nom de l’événement ; pour arrêter l’enregistrement d’un événement, tapez un signe moins \( **\-** \) avant le nom de l’événement. Si le signe est omis, le signe est supposé. N’utilisez pas **tous les** avec n’importe quel autre nom de l’événement.  
  
**lockperiod\=** <em>secondes</em>  
Spécifie le nombre de secondes pendant lesquelles le service serveur pour NFS attendra pour libérer des verrous après qu’une connexion au serveur pour NFS a été perdue et puis rétablie ou le service serveur pour NFS a été redémarré.  
  
Portmapprotocol\={TCP | UDP | TCP\+UDP  
Spécifie les protocoles de transport Portmap prend en charge. Le paramètre par défaut est **TCP\+UDP**.  
  
mountprotocol\={TCP | UDP | TCP\+UDP}  
Spécifie le transport prend en charge de protocoles de montage. Le paramètre par défaut est **TCP\+UDP**.  
  
nfsprotocol\={TCP | UDP | TCP\+UDP}  
Spécifie les protocoles de transport Network File System \(NFS\) prend en charge. Le paramètre par défaut est **TCP\+UDP**  
  
nlmprotocol\={TCP | UDP | TCP\+UDP}  
Spécifie le Gestionnaire de verrous de réseau de protocoles de transport \(NLM\) prend en charge. Le paramètre par défaut est **TCP\+UDP**.  
  
nsmprotocol\={TCP | UDP | TCP\+UDP}  
Spécifie les protocoles de transport réseau le Gestionnaire d’état \(NSM\) prend en charge. Le paramètre par défaut est **TCP\+UDP**.  
  
**enableV3\=** {**yes** | **no**}  
Spécifie si les protocoles de version 3 de NFS seront être pris en charge. Le paramètre par défaut est **Oui**.  
  
**renewauth\=** {**yes** | **no**}  
Spécifie si les connexions client devront s’authentifier à nouveau après la période spécifiée par **config renewauthinterval**. Le paramètre par défaut est **aucun**.  
  
**renewauthinterval\=** <em>seconds</em>  
Spécifie le nombre de secondes qui s’écoulent avant un client est forcé à s’authentifier à nouveau si **config renewauth** a la valeur **Oui**. La valeur par défaut est 600 secondes.  
  
**dircache\=** <em>size</em>  
Spécifie la taille en kilo-octets, du cache de répertoire. Le nombre spécifié comme *taille* doit être un multiple de 4 comprise entre 4 et 128. Le répertoire par défaut\-taille du cache est de 128 Ko.  
  
**translationfile**\=\[file\]  
Spécifie un fichier contenant des informations de mappage pour remplacer des caractères dans les noms de fichiers lors de leur déplacement à partir de Windows\-basés sur UNIX\-en fonction des systèmes de fichiers. Si *fichier* n’est pas spécifié, puis de la conversion de caractères de nom de fichier est désactivée. Si la valeur de **translationfile** est modifié, vous devez redémarrer le serveur pour que la modification prenne effet.  
  
**dotfileshidden**\={**yes** | **no**}  
Spécifie si les fichiers qui sont créés avec des noms commençant par une période \(.\) sera marqué comme masqué dans le système de fichiers Windows et par conséquent masqués à partir de clients NFS. Le paramètre par défaut est **aucun**.  
  
**casesensitivelookups\=** {**yes** | **no**}  
Spécifie si les recherches de répertoire seront sensible à la casse \(nécessitant une correspondance exacte de la casse des caractères\).  
  
Vous devez également désactiver les cas du noyau Windows\-majuscules/minuscules dans l’ordre pour le serveur pour NFS prendre en charge les cas\-noms de fichiers sensibles. Vous pouvez désactiver le cas du noyau Windows\-majuscules/minuscules en désactivant la clé de Registre suivante sur 0 :  
  
HKLM\\système\\CurrentControlSet\\contrôle\\Gestionnaire de Session\\noyau  
  
DWOrd  obcaseinsensitive   
  
> [!IMPORTANT]  
> Cette section s’applique uniquement à Windows Server 2008 R2, Windows Server 2008 et Windows Server 2003. Cette section ne s’applique pas à Windows Server 2012 R2 ou Windows Server 2012.  
  
**ntfscase\=** {**lower** | **upper** | **preserve**}  
Spécifie si la casse des caractères dans les noms des fichiers dans le système de fichiers NTFS est retournée en minuscules, majuscules, ou sous la forme stocké dans le répertoire. Le paramètre par défaut est **conserver**. Ce paramètre ne peut pas être modifié si **casesensitivelookups** a la valeur **Oui**.  
  
**creategroup** *name*  
Crée un nouveau groupe de client, en lui attribuant spécifié *nom*.  
  
**listgroups**  
Affiche les noms de tous les groupes de clients.  
  
**deletegroup** *name*  
Supprime le groupe de client spécifié par *nom*.  
  
**renamegroup** *OldName NewName*  
modifie le nom du groupe de client spécifié par *OldName* à *NewName*  
  
**AddMembers** *nom hôte*\[...\]  
Ajoute *hôte* au groupe de client spécifié par *nom*.  
  
**ListMembers** *nom*  
Répertorie les ordinateurs hôtes dans le groupe de client spécifié par *nom*.  
  
**deletemembers** *groupe hôte*\[...\]  
Supprime le client spécifié par *hôte* à partir du groupe de client spécifié par *groupe*.  
  
Si vous ne spécifiez pas une option de commande ou un argument, **nfsadmin server** affiche le serveur actuel pour les paramètres de configuration de NFS.  
  
#### <a name="administering-client-for-nfs"></a>Administration de service Client pour NFS  
Utilisez le **nfsadmin client** commande pour administrer le service Client pour NFS. L’action spécifique qui **nfsadmin client** dépend de l’argument de commande que vous spécifiez :  
  
**start**  
démarre le service Client pour NFS.  
  
**stop**  
Arrête le service Client pour NFS.  
  
**config**  
Spécifie les paramètres généraux pour le Client pour NFS. Vous devez fournir au moins une des options suivantes avec le **config** argument de commande :  
  
**fileaccess\=** <em>mode</em>  
-   Spécifie le mode d’autorisation par défaut pour les fichiers créés sur le système de fichiers réseau \(NFS\) serveurs. Le *mode* argument se compose d’un trois chiffres de 0 à 7 \(inclusif\) représentant les autorisations par défaut accordées à l’utilisateur, groupe et autres \(respectivement\). Les chiffres traduire à UNIX\-style autorisations comme suit : 0\=aucun, 1\=x 2\=w, 3\=wx, 4\=r, 5\=rx, 6\=rw et 7\=rwx. Par exemple, **fileaccess\=750** donne des autorisations rwx au propriétaire, rx autorisation au groupe et aucune autorisation d’accès à d’autres personnes.  
  
**mapsvr\=** <em>server</em>  
Jeux *server* que le serveur de mappage de noms d’utilisateur pour le Client pour NFS. Bien que cette option continue d’être pris en charge pour la compatibilité avec les versions précédentes, vous devez utiliser le **sfuadmin** utilitaire au lieu de cela.  
  
**mtype\=** {**hard** | **soft**}  
Spécifie le type de montage par défaut. Pour un montage inconditionnel, service Client pour NFS renouvelle les tentatives de RPC ayant échoué jusqu'à ce qu’il réussisse. Pour un montage conditionnel, service Client pour NFS échoue à l’application appelante après une nouvelle tentative de l’appel le nombre de fois spécifié par le **de nouvelle tentative** option.  
  
**nouvelle tentative\=** <em>nombre</em>  
Spécifie le nombre d’essais établir une connexion pour un montage conditionnel. Cette valeur doit être comprise entre 1 et 10, inclusif. La valeur par défaut est 1.  
  
**délai d’attente\=** <em>secondes</em>  
Spécifie le nombre de secondes d’attente pour une connexion \(appel de procédure distante\). Cette valeur doit être 0,8, 0,9 ou entier de 1 à 60, inclus. La valeur par défaut est 0,8.  
  
**Protocol\={TCP | UDP | TCP\+UDP}**  
Spécifie que le client prend en charge des protocoles de transport. Le paramètre par défaut est **TCP\+UDP**  
  
**rsize\=** <em>size</em>  
Spécifie la taille, en kilo-octets, de la mémoire tampon de lecture. Cette valeur peut être 0,5, 1, 2, 4, 8, 16 ou 32. La valeur par défaut est 32.  
  
**wsize\=** <em>size</em>  
Spécifie la taille, en kilo-octets, de la mémoire tampon d’écriture. Cette valeur peut être 0,5, 1, 2, 4, 8, 16 ou 32. La valeur par défaut est 32.  
  
**perf\=default**  
Restaure les paramètres de performances suivants pour les valeurs par défaut :  
  
-   **mtype**  
  
-   **retry**  
  
-   **timeout**  
  
-   **rsize**  
  
-   **wsize**  
  
**fileaccess\=** <em>mode</em>  
Spécifie le mode d’autorisation par défaut pour les fichiers créés sur le système de fichiers réseau \(NFS\) serveurs. Le *mode* argument se compose d’un trois chiffres de 0 à 7 \(inclusif\) représentant les autorisations par défaut accordées à l’utilisateur, groupe et autres \(respectivement\). Les chiffres traduire à UNIX\-style autorisations comme suit : 0\=aucun, 1\=x 2\=w, 3\=wx, 4\=r, 5\=rx, 6\=rw et 7\=rwx. Par exemple, **fileaccess\=750** donne des autorisations rwx au propriétaire, rx autorisation au groupe et aucune autorisation d’accès à d’autres personnes.  
  
Si vous ne spécifiez pas une option de commande ou un argument, **nfsadmin client** affiche les paramètres de configuration NFS Client actuel.  
  

