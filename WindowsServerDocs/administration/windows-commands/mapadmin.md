---
title: mapadmin
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b17332c7-8622-4223-9c43-2fb9cf4d992d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 19975f6f4e0e49cf55e4e80f6566f8596d6d33d8
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724023"
---
# <a name="mapadmin"></a>mapadmin



Vous pouvez utiliser **mapadmin** pour gérer les mappage de noms d’utilisateurs pour Microsoft Services for Network File System.

## <a name="syntax"></a>Syntaxe
```
mapadmin [<computer>] [-u <user> [-p <password>]]
mapadmin [<computer>] [-u <user> [-p <password>]] {start | stop}
mapadmin [<computer>] [-u <user> [-p <password>]] config <option[...]>
mapadmin [<computer>] [-u <user> [-p <password>]] add -wu <WindowsUser> -uu <UNIXUser> [-setprimary]
mapadmin [<computer>] [-u <user> [-p <password>]] add -wg <WindowsGroup> -ug <UNIXGroup> [-setprimary]
mapadmin [<computer>] [-u <user> [-p <password>]] setprimary -wu <WindowsUser> [-uu <UNIXUser>]
mapadmin [<computer>] [-u <user> [-p <password>]] setprimary -wg <WindowsGroup> [-ug <UNIXGroup>]
mapadmin [<computer>] [-u <user> [-p <password>]] delete <option[...]>
mapadmin [<computer>] [-u <user> [-p <password>]] list <option[...]>
mapadmin [<computer>] [-u <user> [-p <password>]] backup <filename> 
mapadmin [<computer>] [-u <user> [-p <password>]] restore <filename>
mapadmin [<computer>] [-u <user> [-p <password>]] adddomainmap -d <WindowsDomain> {-y <<NISdomain>> | -f <path>}
mapadmin [<computer>] [-u <user> [-p <password>]] removedomainmap -d <WindowsDomain> -y <<NISdomain>>
mapadmin [<computer>] [-u <user> [-p <password>]] removedomainmap -all
mapadmin [<computer>] [-u <user> [-p <password>]] listdomainmaps
```

## <a name="description"></a>Description
L’utilitaire de ligne de commande **mapadmin** administre les mappage de noms d’utilisateurs sur l’ordinateur local ou distant qui exécute Microsoft Services for Network File System. Si vous êtes connecté avec un compte qui ne dispose pas d’informations d’identification d’administration, vous pouvez spécifier un nom d’utilisateur et un mot de passe d’un compte qui.

En plus des arguments de commande spécifiques, **mapadmin** accepte les arguments et options suivants :

&lt;ordinateur&gt; spécifie l’ordinateur distant qui exécute le service mappage de noms d’utilisateurs que vous souhaitez administrer. Vous pouvez spécifier l’ordinateur à l’aide d’un nom WINS (Windows Internet Name Service) ou d’un nom DNS (Domain Name System), ou d’une adresse IP (Internet Protocol).

-u &lt;user&gt; spécifie le nom d’utilisateur de l’utilisateur dont les informations d’identification doivent être utilisées. Il peut être nécessaire d’ajouter le nom de domaine au nom d’utilisateur sous la forme<em>nom d’utilisateur</em>de <em>domaine</em>**\\**.

-p &lt;password&gt; spécifie le mot de passe de l’utilisateur. Si vous spécifiez l’option **-u** mais omettez l’option **-p** , vous êtes invité à entrer le mot de passe de l’utilisateur.
L’action spécifique effectuée par **mapadmin** dépend de l’argument de commande que vous spécifiez :

### <a name="parameters"></a>Paramètres
### <a name="start"></a>start
démarre le service mappage de noms d’utilisateurs.

### <a name="stop"></a>stop
Arrête le service mappage de noms d’utilisateurs.

### <a name="config"></a>config
Spécifie les paramètres généraux de mappage de noms d’utilisateurs. Les options suivantes sont disponibles avec cet argument de commande **:- &lt;r&gt;dddd&lt;:&gt;HH&lt;:&gt; mm** : spécifie l’intervalle d’actualisation pour la mise à jour des bases de données Windows et NIS en jours, heures et minutes. L’intervalle minimal est de 5 minutes.
**-i {oui | non}** -active le mappage simple (**Oui**) ou désactivé (**non**). Par défaut, le mappage simple est activé.
**Ajouter** -crée un nouveau mappage pour un utilisateur ou un groupe. Les options suivantes sont disponibles avec cet argument de commande :

|Option|Définition|
|-----|-------|
|-nom &lt;Wu&gt;|Spécifie le nom de l’utilisateur Windows pour lequel un nouveau mappage est créé.|
|-uu &lt;nom&gt;|Spécifie le nom de l’utilisateur UNIX pour lequel un nouveau mappage est créé.|
|-WG &lt;groupe&gt;|Spécifie le nom du groupe Windows pour lequel un nouveau mappage est créé.|
|-UG &lt;groupe&gt;|Spécifie le nom du groupe UNIX pour lequel un nouveau mappage est créé.|
|-setprimary|Spécifie que le nouveau mappage est le mappage principal.|

**setprimary** : spécifie le mappage principal pour un utilisateur ou un groupe UNIX avec plusieurs mappages. Les options suivantes sont disponibles avec cet argument de commande :

|Option|Définition|
|-----|-------|
|-nom &lt;Wu&gt;|Spécifie l’utilisateur Windows du mappage principal. S’il existe plusieurs mappages pour l’utilisateur, utilisez l’option **-uu** pour spécifier le mappage principal.|
|-uu &lt;nom&gt;|Spécifie l’utilisateur UNIX du mappage principal.|
|-WG &lt;groupe&gt;|Spécifie le groupe Windows du mappage principal. S’il existe plusieurs mappages pour le groupe, utilisez l’option **-UG** pour spécifier le mappage principal.|
|-UG &lt;groupe&gt;|Spécifie le groupe UNIX du mappage principal.|

**supprimer** -supprime le mappage d’un utilisateur ou d’un groupe. Les options suivantes sont disponibles pour cet argument de commande :

|Option|Définition|
|-----|-------|
|-utilisateur &lt;Wu&gt;|Utilisateur Windows pour lequel le mappage sera supprimé, spécifié en tant que &lt; *nom&gt;\\&lt;&gt;d’utilisateur WindowsDomain*. Vous devez spécifier l’option **-Wu** ou **-uu** , ou les deux. Si vous spécifiez les deux options, le mappage particulier identifié par les deux options sera supprimé. Si vous spécifiez uniquement l’option **-Wu** , tous les mappages de l’utilisateur spécifié seront supprimés.|
|-WG &lt;groupe&gt;|Groupe Windows pour lequel le mappage sera supprimé, spécifié sous la forme &lt;WindowsDomain&gt;\\&lt;GroupName&gt;. Vous devez spécifier l’option **-WG** ou **-UG** , ou les deux. Si vous spécifiez les deux options, le mappage particulier identifié par les deux options sera supprimé. Si vous spécifiez uniquement l’option **-WG** , tous les mappages du groupe spécifié seront supprimés.|
|-utilisateur &lt;uu&gt;|Utilisateur UNIX pour lequel le mappage sera supprimé, spécifié sous la forme &lt;d’un&gt;nom d’utilisateur. Vous devez spécifier l’option **-Wu** ou **-uu** , ou les deux. Si vous spécifiez les deux options, le mappage particulier identifié par les deux options sera supprimé. Si vous spécifiez uniquement l’option **-uu** , tous les mappages de l’utilisateur spécifié seront supprimés.|
|-UG &lt;groupe&gt;|Le groupe UNIX pour lequel le mappage sera supprimé, spécifié sous la &lt;forme&gt;GroupName. Vous devez spécifier l’option **-WG** ou **-UG** , ou les deux. Si vous spécifiez les deux options, le mappage particulier identifié par les deux options sera supprimé. Si vous spécifiez uniquement l’option **-UG** , tous les mappages du groupe spécifié seront supprimés.|

**liste** : affiche des informations sur les mappages d’utilisateurs et de groupes. Les options suivantes sont disponibles avec cet argument de commande :

|Option|Définition|
|-----|-------|
|-tout|répertorie les mappages simples et avancés pour les utilisateurs et les groupes.|
|-simple|répertorie tous les utilisateurs et groupes mappés simples.|
|-avancé|répertorie tous les utilisateurs et groupes mappés avancés. Les mappages sont répertoriés dans l’ordre dans lequel ils sont évalués. Les cartes principales, marquées d’un astérisque (*), sont répertoriées en premier, suivies des cartes secondaires, qui sont marquées d’un accent circonflexe (^).|
|-nom &lt;Wu&gt;|répertorie le mappage d’un utilisateur Windows spécifié.|
|-WG &lt;groupe&gt;|répertorie le mappage d’un groupe Windows.|
|-uu &lt;nom&gt;|répertorie le mappage d’un utilisateur UNIX.|
|-UG &lt;groupe&gt;|répertorie le mappage d’un groupe UNIX.|

**Backup** : enregistre mappage de noms d’utilisateurs configuration et le mappage des données dans le fichier &lt;spécifié&gt;par filename.
**Restore** : remplace les données de configuration et de mappage par les données du &lt;fichier&gt;(spécifié par filename) qui ont été créées à l’aide de l’argument de commande **Backup** .
**adddomainmap** : ajoute un mappage simple entre un domaine Windows et un domaine NIS ou des fichiers de groupe et de mot de passe. Les options suivantes sont disponibles pour cet argument de commande :

|Option|Définition|
|-----|-------|
|-d &lt;WindowsDomain&gt;|Spécifie le domaine Windows à mapper.|
|-y &lt;NISdomain&gt;|Spécifie le domaine NIS à mapper. &lt;br/&gt;&lt;br/&gt;**-n** &lt;nisServer&gt; spécifie le serveur NIS pour le domaine NIS spécifié avec l’option **-y** .|
|-f &lt;chemin&gt;|Spécifie le chemin d’accès complet du répertoire contenant les fichiers de groupe et de mot de passe à mapper. Les fichiers doivent se trouver sur l’ordinateur en cours de gestion et vous ne pouvez pas utiliser **mapadmin** pour gérer un ordinateur distant afin de configurer des mappages basés sur des fichiers de groupe et de mot de passe.|

**removedomainmap** : supprime un mappage simple entre un domaine Windows et un domaine NIS. Les options et les arguments suivants sont disponibles pour cet argument de commande :

|Option|Définition|
|-----|-------|
|-d &lt;WindowsDomain&gt;|Spécifie le domaine Windows du mappage à supprimer.|
|-y &lt;NISdomain&gt;|Spécifie le domaine NIS du mappage à supprimer.|
|-tout|Spécifie que tous les mappages simples entre les domaines Windows et NIS doivent être supprimés. Cela supprimera également tout mappage simple entre un domaine Windows et des fichiers de groupe et de mot de passe.|

**listdomainmaps** : répertorie les domaines Windows mappés à des domaines NIS ou à des fichiers de groupe et de mot de passe.

## <a name="notes"></a>Notes
-   Si vous ne spécifiez pas d’argument de commande, **mapadmin** affiche les paramètres actuels pour mappage de noms d’utilisateurs.
-   pour toutes les options qui spécifient un nom d’utilisateur ou de groupe, vous pouvez utiliser les formats suivants :
-   pour les utilisateurs Windows, utilisez la &lt;forme&gt;\\&lt;nom&gt;d’utilisateur \\ \\ &lt;de&gt;\\&lt;domaine,&gt;nom \\ &lt;d'&gt;\\&lt;utilisateur de&gt;l’ordinateur &lt;,&gt;\\&lt;nom d’utilisateur de l’ordinateur ou nom d’utilisateur de l’ordinateur.&gt;
-   pour les groupes Windows, utilisez le &lt;format&gt;\\&lt;&lt;domaine&gt;&gt;GroupName \\ \\ &lt;,&gt;\\&lt;Computer&lt;GroupName \\ &lt;, Computer&gt;GroupName ou Computer&gt;GroupName.\\&lt;&gt;&gt;\\ &lt;&lt;&gt;&gt;&lt;&lt;&gt;&gt;
-   pour les utilisateurs UNIX, utilisez la &lt;forme&gt;\\&lt;NISdomain nom&gt;d' &lt;utilisateur,&gt;@&lt;nom&gt;d’utilisateur &lt;NISdomain&gt;@PCNFS, nom d'\\&lt;utilisateur ou nom d’utilisateur PCNFS.&gt;
-   pour les groupes UNIX, utilisez la &lt;forme&gt;\\&lt;NISdomain&gt;GroupName &lt;,&gt;@&lt;GroupName&gt;NISdomain &lt;,&gt;@PCNFSGroupName ou PCNFS\\&lt;GroupName.&gt;

## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
