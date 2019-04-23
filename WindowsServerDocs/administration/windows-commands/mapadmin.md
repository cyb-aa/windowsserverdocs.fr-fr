---
title: mapadmin
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 007256fffde11899d930c9197cade6d3bf9be42c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868780"
---
# <a name="mapadmin"></a>mapadmin



Vous pouvez utiliser **Mapadmin** pour gérer le mappage de nom de l’utilisateur pour Microsoft Services for Network File System.

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
Le **mapadmin** utilitaire de ligne de commande applique le mappage de nom de l’utilisateur sur l’ordinateur local ou distant exécutant Microsoft Services for Network File System. Si vous êtes connecté avec un compte qui n’a pas d’informations d’identification administratives, vous pouvez spécifier un nom d’utilisateur et le mot de passe d’un compte qui en bénéficie.

En plus des arguments de commande spécifiques, **mapadmin** accepte les arguments et options suivants :

&lt;ordinateur&gt; Spécifie l’ordinateur distant exécutant le service de mappage de noms d’utilisateur que vous souhaitez administrer. Vous pouvez spécifier l’ordinateur à l’aide d’un nom de Windows Internet Service WINS (Name) ou un nom de système DNS (Domain Name) ou l’adresse par IP (Internet Protocol).

-u &lt;utilisateur&gt; Spécifie le nom d’utilisateur de l’utilisateur dont informations d’identification doivent être utilisées. Il peut être nécessaire d’ajouter le nom de domaine pour le nom d’utilisateur sous la forme *domaine***\\***nom d’utilisateur*.

p - &lt;mot de passe&gt; Spécifie le mot de passe de l’utilisateur. Si vous spécifiez le **-u** option mais omettent le **-p** option, vous êtes invité au mot de passe.
L’action spécifique qui **mapadmin** effectue dépend de l’argument de commande que vous spécifiez :

## <a name="parameters"></a>Paramètres
### <a name="start"></a>start
démarre le service de mappage de noms d’utilisateur.

### <a name="stop"></a>stop
Arrête le service de mappage de noms d’utilisateur.

### <a name="config"></a>config
Spécifie les paramètres généraux pour le mappage de nom de l’utilisateur. Les options suivantes sont disponibles avec cet argument de commande : **- r &lt;dddd&gt;:&lt;hh&gt;:&lt;mm&gt;**  -Spécifie l’intervalle d’actualisation pour la mise à jour des bases de données Windows et NIS en jours, heures et minutes. L’intervalle minimum est de 5 minutes.
**i-{Oui | aucun}** -Active le mappage simple (**Oui**) ou désactivé (**aucune**). Par défaut, le mappage simple est activé.
**ajouter** -crée un nouveau mappage pour un utilisateur ou un groupe. Les options suivantes sont disponibles avec cet argument de commande :

|Option|Définition|
|-----|-------|
|-wu &lt;name&gt;|Spécifie le nom de l’utilisateur Windows pour lequel un nouveau mappage est créé.|
|-uu &lt;name&gt;|Spécifie le nom de l’utilisateur UNIX pour lequel un nouveau mappage est créé.|
|-wg &lt;group&gt;|Spécifie le nom du groupe Windows pour lequel un nouveau mappage est créé.|
|-ug &lt;groupe&gt;|Spécifie le nom du groupe UNIX pour lequel un nouveau mappage est créé.|
|-setprimary|Spécifie que le nouveau mappage est le mappage principal.|

**setprimary** -Spécifie que le mappage est le mappage principal pour un utilisateur UNIX ou un groupe avec plusieurs mappages. Les options suivantes sont disponibles avec cet argument de commande :

|Option|Définition|
|-----|-------|
|-wu &lt;name&gt;|Spécifie l’utilisateur Windows du mappage principal. Si plusieurs mappages pour l’utilisateur existe, utilisez le **- uu** option pour spécifier le mappage principal.|
|-uu &lt;name&gt;|Spécifie l’utilisateur UNIX du mappage principal.|
|-wg &lt;group&gt;|Spécifie le groupe Windows du mappage principal. Si plusieurs mappages pour le groupe existe, utilisez le **- ug** option pour spécifier le mappage principal.|
|-ug &lt;groupe&gt;|Spécifie le groupe UNIX du mappage principal.|

**supprimer** -supprime le mappage pour un utilisateur ou un groupe. Les options suivantes sont disponibles pour cet argument de commande :

|Option|Définition|
|-----|-------|
|-wu &lt;user&gt;|L’utilisateur Windows pour lequel le mappage est supprimé, spécifié en tant que &lt; *WindowsDomain&gt;\\&lt;nom d’utilisateur&gt;*. Vous devez spécifier soit le **- wu** ou **- uu** option, ou les deux. Si vous spécifiez les deux options, le mappage particulier identifié par les deux options est supprimé. Si vous spécifiez uniquement le **- wu** option, tous les mappages par l’utilisateur spécifié est supprimé.|
|-wg &lt;group&gt;|Le groupe Windows pour lequel le mappage est supprimé, spécifié en tant que &lt;WindowsDomain&gt;\\&lt;groupname&gt;. Vous devez spécifier soit le **- wg** ou **- ug** option, ou les deux. Si vous spécifiez les deux options, le mappage particulier identifié par les deux options est supprimé. Si vous spécifiez uniquement le **- wg** option, tous les mappages pour le groupe spécifié est supprimé.|
|-uu &lt;user&gt;|L’utilisateur UNIX pour lequel le mappage sera supprimé, spécifié en tant que &lt;nom d’utilisateur&gt;. Vous devez spécifier soit le **- wu** ou **- uu** option, ou les deux. Si vous spécifiez les deux options, le mappage particulier identifié par les deux options est supprimé. Si vous spécifiez uniquement le **- uu** option, tous les mappages par l’utilisateur spécifié est supprimé.|
|-ug &lt;groupe&gt;|Le groupe d’UNIX pour lequel le mappage est supprimé, spécifié en tant que &lt;groupname&gt;. Vous devez spécifier soit le **- wg** ou **- ug** option, ou les deux. Si vous spécifiez les deux options, le mappage particulier identifié par les deux options est supprimé. Si vous spécifiez uniquement le **- ug** option, tous les mappages pour le groupe spécifié est supprimé.|

**liste** -affiche des informations sur les mappages d’utilisateur et de groupe. Les options suivantes sont disponibles avec cet argument de commande :

|Option|Définition|
|-----|-------|
|-toutes les|Répertorie les mappages simples et avancés pour les utilisateurs et groupes.|
|-simple|Répertorie tous les utilisateurs mappés simples et les groupes.|
|-avancée|Répertorie tous les utilisateurs mappés avancés et des groupes. Mappages sont répertoriés dans l’ordre dans lequel ils sont évalués. Cartes principales, marqués d’un astérisque (*) sont répertoriés en premier, suivie de cartes secondaire, qui sont marqués avec un accent circonflexe (^).|
|-wu &lt;name&gt;|répertorie le mappage d’un utilisateur Windows spécifié.|
|-wg &lt;group&gt;|répertorie le mappage pour un groupe Windows.|
|-uu &lt;name&gt;|répertorie le mappage pour un utilisateur UNIX.|
|-ug &lt;groupe&gt;|répertorie le mappage pour un groupe d’UNIX.|

**sauvegarde** - nom d’utilisateur enregistre la configuration du mappage et de mappage des données dans le fichier spécifié par &lt;filename&gt;.
**restaurer** -remplace les données de configuration et de mappage avec des données à partir du fichier (spécifié par &lt;filename&gt;) qui a été créé à l’aide de la **sauvegarde** argument de commande.
**adddomainmap** -ajoute un mappage simple entre un domaine Windows et un domaine ou de mot de passe et de groupe des fichiers NIS. Les options suivantes sont disponibles pour cet argument de commande :

|Option|Définition|
|-----|-------|
|-d &lt;WindowsDomain&gt;|Spécifie le domaine Windows à mapper.|
|-y &lt;NISdomain&gt;|Spécifie le domaine NIS à mapper. &lt;br /&gt;&lt;br /&gt;**- n** &lt;nisServer&gt; Spécifie le serveur NIS pour le domaine NIS spécifié avec le **- y**option.|
|-f &lt;path&gt;|Spécifie le chemin d’accès qualifié complet du répertoire contenant les fichiers de mot de passe et de groupe à mapper. Les fichiers doivent se trouver sur l’ordinateur géré, et vous ne pouvez pas utiliser **mapadmin** pour gérer un ordinateur distant pour configurer les mappages basés sur mot de passe et le groupe de fichiers.|

**removedomainmap** -supprime un mappage simple entre un domaine Windows et un domaine NIS. Les options et arguments suivants sont disponibles pour cet argument de commande :

|Option|Définition|
|-----|-------|
|-d &lt;WindowsDomain&gt;|Spécifie le domaine Windows de la carte à supprimer.|
|-y &lt;NISdomain&gt;|Spécifie le domaine NIS de la carte à supprimer.|
|-toutes les|Spécifie que tous les mappages simples entre les domaines Windows et NIS doivent être supprimés. Cela supprimera également tout mappage simple entre un domaine et mot de passe et groupe les fichiers Windows.|

**listdomainmaps** -répertorie les domaines Windows qui sont mappées aux domaines NIS ou mot de passe et le groupe de fichiers.

## <a name="notes"></a>Notes
-   Si vous ne spécifiez pas un argument de commande, **mapadmin** affiche les paramètres actuels pour le mappage de nom de l’utilisateur.
-   pour toutes les options qui spécifient un nom d’utilisateur ou groupe, vous peuvent utiliser les formats suivants :
-   pour les utilisateurs de Windows, utilisez la forme &lt;domaine&gt;\\&lt;nom d’utilisateur&gt;, \\ \\ &lt;ordinateur&gt;\\&lt;utilisateur nom&gt;, \\ &lt;ordinateur&gt;\\&lt;nom d’utilisateur&gt;, ou &lt;ordinateur&gt;\\&lt;utilisateur nom&gt;
-   pour les groupes de Windows, utilisez le formulaire &lt;domaine&gt;\\&lt;&lt;groupname&gt;&gt;, \\ \\ &lt;ordinateur&gt; \\ &lt; &lt;groupname&gt;&gt;, \\ &lt;ordinateur&gt;\\&lt;&lt;groupname&gt; &gt;, ou &lt;ordinateur&gt;\\&lt;&lt;groupname&gt;&gt;
-   pour les utilisateurs UNIX, utilisez la forme &lt;NISdomain&gt;\\&lt;nom d’utilisateur&gt;, &lt;nom d’utilisateur&gt;@&lt;NISdomain&gt;, utilisateur &lt;nom&gt;@PCNFS, ou PCNFS\\&lt;nom d’utilisateur&gt;
-   pour les groupes de UNIX, utilisez le formulaire &lt;NISdomain&gt;\\&lt;groupname&gt;, &lt;groupname&gt;@&lt;NISdomain&gt;, &lt;groupname&gt;@PCNFS, ou PCNFS\\&lt;groupname&gt;

## <a name="additional-references"></a>Références supplémentaires
[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
