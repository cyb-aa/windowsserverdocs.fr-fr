---
title: Prise en main du centre d’administration Windows
description: Prise en main du centre d’administration Windows
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 02/15/2019
ms.openlocfilehash: fc8e6ffa39320cfc73bf3f5bd0a5bc765ded24b4
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79322871"
---
# <a name="get-started-with-windows-admin-center"></a>Prise en main du centre d’administration Windows

>S’applique à : Windows Admin Center, Windows Admin Center Preview

> [!Tip]
> Vous débutez dans Windows Admin Center ?
> [Découvrez-en davantage sur Windows Admin Center](../overview.md) ou [téléchargez maintenant](https://aka.ms/windowsadmincenter).

## <a name="windows-admin-center-installed-on-windows-10"></a>Centre d’administration Windows installé sur Windows 10

> [!IMPORTANT]
> Vous devez être membre du groupe Administrateurs local pour utiliser le centre d’administration Windows sur Windows 10

### <a name="selecting-a-client-certificate"></a>Sélection d’un certificat client

La première fois que vous ouvrez le centre d’administration Windows sur Windows 10, veillez à sélectionner le certificat du *client du centre d’administration Windows* (sinon, vous obtiendrez une erreur http 403 indiquant « impossible d’accéder à cette page »).

Dans Microsoft Edge, lorsque vous êtes invité à utiliser cette boîte de dialogue :
 
1. Cliquez sur **plus de choix**

    ![](../media/launch-cert-1.png)

2. Sélectionnez le certificat intitulé **client du centre d’administration Windows** , puis cliquez sur **OK** .

    ![](../media/launch-cert-2.png)

3. Assurez-vous que **l’option toujours autoriser l’accès** est sélectionnée, puis cliquez sur **autoriser**

    ![](../media/launch-cert-3.png)

## <a name="connecting-to-managed-nodes-and-clusters"></a>Connexion à des clusters et des nœuds gérés

Une fois que vous avez terminé l’installation du centre d’administration Windows, vous pouvez ajouter des serveurs ou des clusters à gérer à partir de la page de vue d’ensemble principale.

 **Ajouter un serveur unique ou un cluster en tant que nœud géré**

1. Cliquez sur **+ Ajouter** sous **toutes les connexions**.

   ![](../media/launch/addserver0.png)

2. Choisissez d’ajouter un serveur, un cluster, un PC Windows ou une machine virtuelle Azure :
    
   ![](../media/launch/ChooseConnectionType.png)

3. Tapez le nom du serveur ou du cluster à gérer, puis cliquez sur **Envoyer**. Le serveur ou le cluster sera ajouté à votre liste de connexions sur la page vue d’ensemble.

   ![](../media/launch/addserver2.png)

   **--OU--**

**Importation en bloc de plusieurs serveurs**

 1. Sur la page **Ajouter une connexion au serveur** , choisissez l’onglet importer des **serveurs** .

    ![](../media/launch/import-servers.png)

 2. Cliquez sur **Parcourir** et sélectionnez un fichier texte contenant une virgule, ou une nouvelle ligne séparée, liste des noms de domaine complets pour les serveurs que vous souhaitez ajouter.

> [!Note]
> Le fichier. csv créé en [exportant vos connexions avec PowerShell](#use-powershell-to-import-or-export-your-connections-with-tags) contient des informations supplémentaires au-delà des noms de serveurs et n’est pas compatible avec cette méthode d’importation.

  **--OU--**

**Ajouter des serveurs en effectuant des recherches Active Directory**

 1. Sur la page **Ajouter une connexion au serveur** , choisissez l’onglet Active Directory de **recherche** .

    ![](../media/launch/search-ad.png)

 2. Entrez vos critères de recherche, puis cliquez sur **Rechercher**. Les caractères génériques (*) sont pris en charge.

 3. Une fois la recherche terminée, sélectionnez un ou plusieurs des résultats, ajoutez éventuellement des balises, puis cliquez sur **Ajouter**.

## <a name="authenticate-with-the-managed-node"></a>S’authentifier avec le nœud géré ##

Le centre d’administration Windows prend en charge plusieurs mécanismes d’authentification avec un nœud géré. L’authentification unique est la valeur par défaut.

**Authentification unique**

Vous pouvez utiliser vos informations d’identification Windows actuelles pour vous authentifier auprès du nœud géré. Il s’agit de la valeur par défaut, et le centre d’administration Windows tente d’ouvrir une session lorsque vous ajoutez un serveur. 

**Authentification unique lors d’un déploiement en tant que service sur Windows Server**

Si vous avez installé le centre d’administration Windows sur Windows Server, une configuration supplémentaire est requise pour l’authentification unique.  [Configurer votre environnement pour la délégation](../configure/user-access-control.md)

**--OU--**

**Utiliser *gérer en tant que* pour spécifier les informations d’identification**

Sous **toutes les connexions**, sélectionnez un serveur dans la liste et choisissez **gérer en tant que** pour spécifier les informations d’identification que vous allez utiliser pour vous authentifier auprès du nœud géré :

![](../media/launch-use-6.png)

Si le centre d’administration Windows s’exécute en mode de service sur Windows Server, mais que la délégation Kerberos n’est pas configurée, vous devez entrer à nouveau vos informations d’identification Windows :

![](../media/launch-use-7.png)

Vous pouvez appliquer les informations d’identification à toutes les connexions, ce qui les mettra en cache pour cette session de navigateur spécifique. Si vous rechargez votre navigateur, vous devez entrer à nouveau vos informations d’identification **gérer en tant que** .

**Solution de mot de passe d’administrateur local (LAPS)**

Si votre environnement utilise des [chevauchements](https://technet.microsoft.com/mt227395.aspx)et que vous avez installé le centre d’administration Windows sur votre PC Windows 10, vous pouvez utiliser des informations d’identification pour vous authentifier auprès du nœud géré. **Si vous utilisez ce scénario, veuillez** [fournir vos commentaires](https://aka.ms/WACFeedback).

## <a name="using-tags-to-organize-your-connections"></a>Utilisation de balises pour organiser vos connexions

Vous pouvez utiliser des balises pour identifier et filtrer les serveurs associés dans votre liste de connexions.  Cela vous permet d’afficher un sous-ensemble de vos serveurs dans la liste des connexions.  Cela s’avère particulièrement utile si vous avez de nombreuses connexions.

### <a name="edit-tags"></a>Modifier les balises

* Sélectionner un ou plusieurs serveurs dans la liste toutes les connexions
* Sous **toutes les connexions**, cliquez sur **modifier les balises**

![](../media/launch/tags-5.png)

Le volet **modifier les balises de connexion** vous permet de modifier, d’ajouter ou de supprimer des balises de vos connexions sélectionnées :

* Pour ajouter une nouvelle balise à vos connexions sélectionnées, sélectionnez Ajouter une **étiquette** , puis entrez le nom de la balise que vous souhaitez utiliser.

* Pour baliser les connexions sélectionnées avec un nom de balise existant, cochez la case en regard du nom de balise que vous souhaitez appliquer.

* Pour supprimer une balise de toutes les connexions sélectionnées, décochez la case en regard de la balise que vous souhaitez supprimer.

* Si une étiquette est appliquée à un sous-ensemble des connexions sélectionnées, la case à cocher est affichée dans un état intermédiaire. Vous pouvez cliquer sur la case pour la vérifier et appliquer la balise à toutes les connexions sélectionnées, ou cliquer à nouveau pour la désactiver et supprimer la balise de toutes les connexions sélectionnées.

![](../media/launch/tags-6.png)

### <a name="filter-connections-by-tag"></a>Filtrer les connexions par balise

Une fois que des balises ont été ajoutées à une ou plusieurs connexions au serveur, vous pouvez afficher les balises dans la liste des connexions et filtrer la liste des connexions par balises.

* Pour filtrer par une balise, sélectionnez l’icône de filtre en regard de la zone de recherche.
![](../media/launch/tags-7.png)
* Vous pouvez sélectionner « ou », « et » ou « non » pour modifier le comportement de filtre des balises sélectionnées.
![](../media/launch/tags-8.png)

## <a name="use-powershell-to-import-or-export-your-connections-with-tags"></a>Utiliser PowerShell pour importer ou exporter vos connexions (avec étiquettes)

[!INCLUDE [ps-connections](../includes/ps-connections.md)]

## <a name="view-powershell-scripts-used-in-windows-admin-center"></a>Afficher les scripts PowerShell utilisés dans le centre d’administration Windows

Une fois que vous êtes connecté à un serveur, un cluster ou un PC, vous pouvez consulter les scripts PowerShell qui alimentent les actions de l’interface utilisateur disponibles dans le centre d’administration Windows. Dans un outil, cliquez sur l’icône PowerShell dans la barre d’application supérieure. Sélectionnez une commande d’intérêt dans la liste déroulante pour accéder au script PowerShell correspondant.

![](../media/launch/showscript.png)
