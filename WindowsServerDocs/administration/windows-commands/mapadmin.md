---
title: mapadmin
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b17332c7-8622-4223-9c43-2fb9cf4d992d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fc4b76c1989298ea83c480b9c838ce0fc18fef5f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373756"
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

&lt;computer @ no__t-1 spécifie l’ordinateur distant qui exécute le service mappage de noms d’utilisateurs que vous souhaitez administrer. Vous pouvez spécifier l’ordinateur à l’aide d’un nom WINS (Windows Internet Name Service) ou d’un nom DNS (Domain Name System), ou d’une adresse IP (Internet Protocol).

-u &lt;user @ no__t-1 spécifie le nom d’utilisateur de l’utilisateur dont les informations d’identification doivent être utilisées. Il peut être nécessaire d’ajouter le nom de domaine au nom d’utilisateur sous la forme <em>domaine</em> **\\** <em>nom d’utilisateur</em>.

-p &lt;password @ no__t-1 spécifie le mot de passe de l’utilisateur. Si vous spécifiez l’option **-u** mais omettez l’option **-p** , vous êtes invité à entrer le mot de passe de l’utilisateur.
L’action spécifique effectuée par **mapadmin** dépend de l’argument de commande que vous spécifiez :

## <a name="parameters"></a>Paramètres
### <a name="start"></a>start
démarre le service mappage de noms d’utilisateurs.

### <a name="stop"></a>stop
Arrête le service mappage de noms d’utilisateurs.

### <a name="config"></a>package
Spécifie les paramètres généraux de mappage de noms d’utilisateurs. Les options suivantes sont disponibles avec cet argument de commande : **-r &lt;dddd @ no__t-2 : &lt;HH @ no__t-4 : &lt;mm @ no__t-6** -spécifie l’intervalle d’actualisation pour la mise à jour des bases de données Windows et NIS en jours, heures et minutes. L’intervalle minimal est de 5 minutes.
**-i {oui | non}** -active le mappage simple (**Oui**) ou désactivé (**non**). Par défaut, le mappage simple est activé.
**Ajouter** -crée un nouveau mappage pour un utilisateur ou un groupe. Les options suivantes sont disponibles avec cet argument de commande :

|Option|Définition|
|-----|-------|
|-Wu &lt;name @ no__t-1|Spécifie le nom de l’utilisateur Windows pour lequel un nouveau mappage est créé.|
|-uu &lt;name @ no__t-1|Spécifie le nom de l’utilisateur UNIX pour lequel un nouveau mappage est créé.|
|-WG &lt;group @ no__t-1|Spécifie le nom du groupe Windows pour lequel un nouveau mappage est créé.|
|-UG &lt;group @ no__t-1|Spécifie le nom du groupe UNIX pour lequel un nouveau mappage est créé.|
|-setprimary|Spécifie que le nouveau mappage est le mappage principal.|

**setprimary** : spécifie le mappage principal pour un utilisateur ou un groupe UNIX avec plusieurs mappages. Les options suivantes sont disponibles avec cet argument de commande :

|Option|Définition|
|-----|-------|
|-Wu &lt;name @ no__t-1|Spécifie l’utilisateur Windows du mappage principal. S’il existe plusieurs mappages pour l’utilisateur, utilisez l’option **-uu** pour spécifier le mappage principal.|
|-uu &lt;name @ no__t-1|Spécifie l’utilisateur UNIX du mappage principal.|
|-WG &lt;group @ no__t-1|Spécifie le groupe Windows du mappage principal. S’il existe plusieurs mappages pour le groupe, utilisez l’option **-UG** pour spécifier le mappage principal.|
|-UG &lt;group @ no__t-1|Spécifie le groupe UNIX du mappage principal.|

**supprimer** -supprime le mappage d’un utilisateur ou d’un groupe. Les options suivantes sont disponibles pour cet argument de commande :

|Option|Définition|
|-----|-------|
|-Wu &lt;user @ no__t-1|Utilisateur Windows pour lequel le mappage sera supprimé, spécifié comme &lt;*WindowsDomain @ no__t-2 @ no__t-3 @ no__t-4user Name @ no__t-5*. Vous devez spécifier l’option **-Wu** ou **-uu** , ou les deux. Si vous spécifiez les deux options, le mappage particulier identifié par les deux options sera supprimé. Si vous spécifiez uniquement l’option **-Wu** , tous les mappages de l’utilisateur spécifié seront supprimés.|
|-WG &lt;group @ no__t-1|Groupe Windows pour lequel le mappage sera supprimé, spécifié sous la forme &lt;WindowsDomain @ no__t-1 @ no__t-2 @ no__t-3groupname @ no__t-4. Vous devez spécifier l’option **-WG** ou **-UG** , ou les deux. Si vous spécifiez les deux options, le mappage particulier identifié par les deux options sera supprimé. Si vous spécifiez uniquement l’option **-WG** , tous les mappages du groupe spécifié seront supprimés.|
|-uu &lt;user @ no__t-1|Utilisateur UNIX pour lequel le mappage sera supprimé, spécifié sous la forme @no__t nom-0User @ no__t-1. Vous devez spécifier l’option **-Wu** ou **-uu** , ou les deux. Si vous spécifiez les deux options, le mappage particulier identifié par les deux options sera supprimé. Si vous spécifiez uniquement l’option **-uu** , tous les mappages de l’utilisateur spécifié seront supprimés.|
|-UG &lt;group @ no__t-1|Le groupe UNIX pour lequel le mappage sera supprimé, spécifié sous la forme &lt;groupname @ no__t-1. Vous devez spécifier l’option **-WG** ou **-UG** , ou les deux. Si vous spécifiez les deux options, le mappage particulier identifié par les deux options sera supprimé. Si vous spécifiez uniquement l’option **-UG** , tous les mappages du groupe spécifié seront supprimés.|

**liste** : affiche des informations sur les mappages d’utilisateurs et de groupes. Les options suivantes sont disponibles avec cet argument de commande :

|Option|Définition|
|-----|-------|
|-tout|répertorie les mappages simples et avancés pour les utilisateurs et les groupes.|
|-simple|répertorie tous les utilisateurs et groupes mappés simples.|
|-avancé|répertorie tous les utilisateurs et groupes mappés avancés. Les mappages sont répertoriés dans l’ordre dans lequel ils sont évalués. Les cartes principales, marquées d’un astérisque (*), sont répertoriées en premier, suivies des cartes secondaires, qui sont marquées d’un accent circonflexe (^).|
|-Wu &lt;name @ no__t-1|répertorie le mappage d’un utilisateur Windows spécifié.|
|-WG &lt;group @ no__t-1|répertorie le mappage d’un groupe Windows.|
|-uu &lt;name @ no__t-1|répertorie le mappage d’un utilisateur UNIX.|
|-UG &lt;group @ no__t-1|répertorie le mappage d’un groupe UNIX.|

**Backup** : enregistre mappage de noms d’utilisateurs configuration et le mappage des données dans le fichier spécifié par &lt;filename @ no__t-2.
**Restore** : remplace les données de configuration et de mappage par les données du fichier (spécifiées par &lt;filename @ no__t-2) qui ont été créées à l’aide de l’argument de commande **Backup** .
**adddomainmap** : ajoute un mappage simple entre un domaine Windows et un domaine NIS ou des fichiers de groupe et de mot de passe. Les options suivantes sont disponibles pour cet argument de commande :

|Option|Définition|
|-----|-------|
|-d &lt;WindowsDomain @ no__t-1|Spécifie le domaine Windows à mapper.|
|-y &lt;NISdomain @ no__t-1|Spécifie le domaine NIS à mapper. &lt;br/&gt; @ no__t-2br/&gt; **-n** &lt;nisServer @ no__t-6 spécifie le serveur NIS pour le domaine NIS spécifié avec l’option **-y** .|
|-f &lt;path @ no__t-1|Spécifie le chemin d’accès complet du répertoire contenant les fichiers de groupe et de mot de passe à mapper. Les fichiers doivent se trouver sur l’ordinateur en cours de gestion et vous ne pouvez pas utiliser **mapadmin** pour gérer un ordinateur distant afin de configurer des mappages basés sur des fichiers de groupe et de mot de passe.|

**removedomainmap** : supprime un mappage simple entre un domaine Windows et un domaine NIS. Les options et les arguments suivants sont disponibles pour cet argument de commande :

|Option|Définition|
|-----|-------|
|-d &lt;WindowsDomain @ no__t-1|Spécifie le domaine Windows du mappage à supprimer.|
|-y &lt;NISdomain @ no__t-1|Spécifie le domaine NIS du mappage à supprimer.|
|-tout|Spécifie que tous les mappages simples entre les domaines Windows et NIS doivent être supprimés. Cela supprimera également tout mappage simple entre un domaine Windows et des fichiers de groupe et de mot de passe.|

**listdomainmaps** : répertorie les domaines Windows mappés à des domaines NIS ou à des fichiers de groupe et de mot de passe.

## <a name="notes"></a>Notes
-   Si vous ne spécifiez pas d’argument de commande, **mapadmin** affiche les paramètres actuels pour mappage de noms d’utilisateurs.
-   pour toutes les options qui spécifient un nom d’utilisateur ou de groupe, vous pouvez utiliser les formats suivants :
-   pour les utilisateurs Windows, utilisez la forme &lt;domain @ no__t-1 @ no__t-2 @ no__t-3User Name @ no__t-4, \\ @ no__t-6 @ no__t-7computer @ no__t-8 @ no__t-9 @ no__t-10user Name @ no__t-11, &gt;2 @ no__t-13computer @ no__t-14 @ no__t-15 @ no__t-16user Name @ no__ t-17 ou 8computer @ no__t-19 @ no__t-20 @ no__t-21user Name @ no__t-22
-   pour les groupes Windows, utilisez la forme &lt;domain @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4groupname @ no__t-5 @ no__t-6, \\ @ no__t-8 @ no__t-9computer @ no__t-10 @ no__t-11 @ no__t-12 @ no__t-13groupname @ no__t-14 @ no__t-15, &gt;6 @ no__t-17computer @ NO_ _ t-18 @ no__t-19 @ no__t-20 @ no__t-21groupname @ no__t-22 @ no__t-23, ou \\4computer @ no__t-25 @ no__t-26 @ no__t-27 @ no__t-28groupname @ no__t-29 @ no__t-30
-   pour les utilisateurs UNIX, utilisez la forme &lt;NISdomain @ no__t-1 @ no__t-2 @ no__t-3User Name @ no__t-4, &lt;user Name @ no__t-6 @ no__t-7 @ no__t-8NISdomain @ no__t-9, User &gt;0name @ no__t-11 @ no__t-12, ou PCNFS @ no__t-13 @ no__t-14user Name @ no__t-15
-   pour les groupes UNIX, utilisez la forme &lt;NISdomain @ no__t-1 @ no__t-2 @ no__t-3groupname @ no__t-4, &lt;groupname @ no__t-6 @ no__t-7 @ no__t-8NISdomain @ no__t-9, &gt;0groupname @ no__t-11 @ no__t-12 ou PCNFS @ no__t-13 @ no__t-14groupname @ no__t-15

## <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
