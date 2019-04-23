---
title: Prédéfinir des objets d’ordinateur de cluster dans les Services de domaine Active Directory
description: Comment prédéfinir des objets d’ordinateur de cluster dans les Services de domaine Active Directory.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/25/2018
ms.localizationpriority: medium
ms.openlocfilehash: 111969b074b33764dbbf72bfb24ad606f8314e41
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869720"
---
# <a name="prestage-cluster-computer-objects-in-active-directory-domain-services"></a>Prédéfinir des objets d’ordinateur de cluster dans les Services de domaine Active Directory

>S’applique à : Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Cette rubrique montre comment prédéfinir des objets ordinateur du cluster dans les services de domaine Active Directory (AD DS). Vous pouvez utiliser cette procédure pour permettre à un utilisateur ou un groupe de créer un cluster de basculement lorsqu’il n’a pas l’autorisation de créer des objets ordinateur dans AD DS.

Lorsque vous créez un cluster de basculement à l’aide de l’Assistant Création d’un cluster ou de Windows PowerShell, vous devez indiquer un nom pour le cluster. Si vous disposez d’autorisations suffisantes lorsque vous créez le cluster, le processus de création du cluster crée automatiquement un objet ordinateur dans AD DS qui correspond au nom du cluster. Cet objet est appelé *objet nom de cluster* . À l’aide de l’objet nom de cluster, les objets ordinateur virtuel sont automatiquement créés lorsque vous configurez des rôles en cluster qui utilisent des points d’accès client. Par exemple, si vous créez un serveur de fichiers à haut niveau de disponibilité avec un point d’accès client nommé *FileServer1*, l’objet nom de cluster crée un objet ordinateur virtuel correspondant dans AD DS.

>[!NOTE]
>Dans Windows Server 2012 R2, il est possible de créer un cluster détaché d’Active Directory, où aucun objet nom de cluster ni d’un objet ordinateur virtuel n’est créés dans les services AD DS. Cela s’adresse à des types spécifiques de déploiements de cluster. Pour plus d’informations, voir [Déployer un cluster détaché d’Active Directory](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v%3dws.11)>).

Pour créer automatiquement l’objet nom de cluster, l’utilisateur qui crée le cluster de basculement doit disposer de l’autorisation de **création d’objets ordinateur** sur l’unité d’organisation ou le conteneur où se trouvent les serveurs qui constitueront le cluster. Pour permettre à un utilisateur ou un groupe de créer un cluster sans avoir cette autorisation, un utilisateur avec les autorisations appropriées dans AD DS (en général un administrateur de domaine) peut prédéfinir l’objet nom de cluster dans AD DS. L’administrateur de domaine peut ainsi mieux contrôler la convention d’affectation des noms utilisée pour le cluster ainsi que l’unité d’organisation dans laquelle les objets de cluster sont créés.

## <a name="step-1-prestage-the-cno-in-ad-ds"></a>Étape 1 : Préparer le CNO dans AD DS

Avant de commencer, assurez-vous de connaître les éléments suivants :

- Le nom que vous souhaitez affecter au cluster
- Le nom du compte d’utilisateur ou du groupe auquel vous souhaitez accorder des droits pour créer le cluster

Nous vous recommandons, à titre de meilleure pratique, de créer une unité d’organisation pour les objets de cluster. S’il existe déjà une unité d’organisation que vous voulez utiliser, l’appartenance au groupe **Opérateurs de compte** est la condition minimale requise pour effectuer cette étape. Si vous devez créer une unité d’organisation pour les objets de cluster, l’appartenance au groupe **Admins du domaine**, ou à un groupe équivalent, est la condition minimale requise pour effectuer cette étape.

>[!NOTE]
>Si vous créez l’objet nom de cluster dans le conteneur Ordinateurs par défaut et non dans une unité d’organisation, vous n’avez pas à effectuer l’étape 3 de cette rubrique. Dans ce scénario, un administrateur de cluster peut créer jusqu’à 10 objets ordinateur virtuel sans aucune configuration supplémentaire.

### <a name="prestage-the-cno-in-ad-ds"></a>Préparer le CNO dans AD DS

1. Sur un ordinateur sur lequel sont installés les outils AD DS à partir des outils d’administration de serveur distant, ou sur un contrôleur de domaine, ouvrez **Utilisateurs et ordinateurs Active Directory**. Pour ce faire, sur un serveur, démarrez le Gestionnaire de serveur, puis, dans le **outils** menu, sélectionnez **Active Directory Users and Computers**.
2. Pour créer une unité d’organisation pour le cluster des objets d’ordinateur, cliquez sur le nom de domaine ou une unité d’organisation existante, pointez sur **New**, puis sélectionnez **unité d’organisation**. Dans le **nom** zone, entrez le nom de l’unité d’organisation, puis sélectionnez **OK**.
3. Dans l’arborescence de la console, cliquez sur l’unité d’organisation où vous souhaitez créer le CNO, pointez sur **New**, puis sélectionnez **ordinateur**.
4. Dans le **nom de l’ordinateur** , entrez le nom qui sera utilisé pour le cluster de basculement, puis sélectionnez **OK**.

  >[!NOTE]
  >Il s’agit du nom de cluster que l’utilisateur qui crée le cluster indique dans la page **Point d’accès pour l’administration du cluster** dans l’Assistant Création d’un cluster ou en tant que valeur du paramètre *–Name* pour l’applet de commande Windows PowerShell **New-Cluster** .

5. Comme meilleure pratique, cliquez sur le compte d’ordinateur que vous venez de créer, sélectionnez **propriétés**, puis sélectionnez le **objet** onglet. Sur le **objet** onglet, sélectionnez le **protéger l’objet des suppressions accidentelles** case à cocher, puis sélectionnez **OK**.
6. Cliquez sur le compte d’ordinateur que vous venez de créée, puis sélectionnez **désactiver le compte**. Sélectionnez **Oui** pour confirmer, puis sélectionnez **OK**.

  >[!NOTE]
  >Vous devez désactiver le compte afin que, lors de la création du cluster, le processus de création du cluster puisse confirmer que le compte n’est actuellement pas utilisé par un cluster ou ordinateur existant dans le domaine.

![Objet nom de cluster désactivé dans l’exemple d’unité d’organisation Clusters](media/prestage-cluster-adds/disabled-cno-in-the-example-clusters-ou.png)

**Figure 1. Objet nom de cluster désactivé dans l’exemple unité d’organisation Clusters**

## <a name="step-2-grant-the-user-permissions-to-create-the-cluster"></a>Étape 2 : Accordez les autorisations utilisateur pour créer le cluster

Vous devez configurer des autorisations afin que le compte d’utilisateur qui sera utilisé pour créer le cluster de basculement dispose des autorisations Contrôle total sur l’objet nom de cluster.

L’appartenance au groupe **Opérateurs de compte** est la condition minimale requise pour effectuer cette étape.

Voici comment accorder des autorisations d’utilisateur pour créer le cluster :

1. Dans Utilisateurs et ordinateurs Active Directory, dans le menu **Affichage** , vérifiez que l’option **Fonctionnalités avancées** est sélectionnée.
2. Recherchez et cliquez ensuite sur le CNO, puis sélectionnez **propriétés**.
3. Sur le **sécurité** onglet, sélectionnez **ajouter**.
4. Dans le **sélectionnez utilisateurs, ordinateurs ou groupes** boîte de dialogue, spécifiez le compte d’utilisateur ou le groupe que vous souhaitez accorder des autorisations, puis sélectionnez **OK**.
5. Sélectionnez le compte d’utilisateur ou le groupe qui vous venez d’ajouter puis, en regard de **Contrôle total**, activez la case à cocher **Autoriser**.
  
  ![Octroi du contrôle total à l’utilisateur ou au groupe qui va créer le cluster](media/prestage-cluster-adds/granting-full-control-to-the-user-create-the-cluster.png)
  
  **Figure 2. Accorder un contrôle total à l’utilisateur ou le groupe qui va créer le cluster**
6. Sélectionnez **OK**.

Après avoir effectué cette étape, l’utilisateur auquel vous avez accordé les autorisations peut créer le cluster de basculement. Toutefois, si l’objet nom de cluster est situé dans une unité d’organisation, l’utilisateur ne peut pas créer des rôles en cluster qui nécessitent un point d’accès client tant que vous n’avez pas effectué l’étape 3.

>[!NOTE]
>Si l’objet nom de cluster se trouve dans le conteneur Ordinateurs par défaut, un administrateur de cluster peut créer jusqu’à 10 objets ordinateur virtuel sans aucune configuration supplémentaire. Pour ajouter plus de 10 objets ordinateur virtuel, vous devez accorder de façon explicite l’autorisation de **création d’objets ordinateur** à l’objet nom de cluster pour le conteneur Ordinateurs.

## <a name="step-3-grant-the-cno-permissions-to-the-ou-or-prestage-vcos-for-clustered-roles"></a>Étape 3 : Accorder les autorisations de l’objet nom de cluster à l’unité d’organisation ou prédéfinir des objets ordinateur virtuel pour les rôles en cluster

Lorsque vous créez un rôle en cluster avec un point d’accès client, le cluster crée un objet ordinateur virtuel dans la même unité d’organisation que l’objet nom de cluster. Pour que cette opération soit automatique, l’objet nom de cluster doit avoir les autorisations de créer des objets ordinateur dans l’unité d’organisation.

Si vous avez prédéfini l’objet nom de cluster dans AD DS, vous pouvez procéder de l’une des façons suivantes pour créer des objets ordinateur virtuel :

- Option 1 : [Accorder les autorisations de l’objet nom de cluster à l’unité d’organisation](#grant-the-cno-permissions-to-the-ou). Si vous utilisez cette option, le cluster peut automatiquement créer des objets ordinateur virtuel dans AD DS. Par conséquent, un administrateur pour le cluster de basculement peut créer des rôles en cluster sans avoir à vous demander de prédéfinir des objets ordinateur virtuel dans AD DS.

>[!NOTE]
>L’appartenance au groupe **Admins du domaine**, ou à un groupe équivalent, est la condition minimale requise pour effectuer les étapes relatives à cette option.

- Option 2 : [Préparer un objet ordinateur virtuel pour un rôle en cluster](#prestage-a-vco-for-the-clustered-role). Utilisez cette option s’il est nécessaire de prédéfinir des comptes pour les rôles en cluster en raison des exigences de votre organisation. Par exemple, il peut être souhaitable de contrôler la convention d’affectation des noms ou les rôles en cluster qui sont créés.

>[!NOTE]
>L’appartenance au groupe **Opérateurs de compte** est la condition minimale requise pour effectuer les étapes relatives à cette option.

### <a name="grant-the-cno-permissions-to-the-ou"></a>Accorder les autorisations de l’objet nom de cluster à l’unité d’organisation

1. Dans Utilisateurs et ordinateurs Active Directory, dans le menu **Affichage** , vérifiez que l’option **Fonctionnalités avancées** est sélectionnée.
2. Avec le bouton droit de l’unité d’organisation où vous avez créé le CNO dans [étape 1 : Préparer le CNO dans AD DS](#step-1:-prestage-the-CNO-in-ad-ds), puis sélectionnez **propriétés**.
3. Sur le **sécurité** onglet, sélectionnez **avancé**.
4. Dans le **paramètres de sécurité avancés** boîte de dialogue, sélectionnez **ajouter**.
5. Regard **Principal**, sélectionnez **sélectionner un principal**.
6. Dans le **sélectionner un utilisateur, ordinateur, compte de Service ou groupes** boîte de dialogue, sélectionnez **Types d’objets**, sélectionnez le **ordinateurs** case à cocher, puis sélectionnez **OK** .
7. Sous **Entrez les noms des objets à sélectionner**, entrez le nom de l’objet nom de cluster, sélectionnez **vérifier les noms**, puis sélectionnez **OK**. En réponse au message d’avertissement qui indique que vous allez ajouter un objet désactivé, sélectionnez **OK**.
8. Dans la boîte de dialogue **Entrée d’autorisation** , vérifiez que la liste **Type** a la valeur **Autoriser**et que la liste **S’applique à** a la valeur **cet objet et tous ceux descendants**.
9. Sous **Autorisations**, activez la case à cocher **Créer Objets ordinateur** .

  ![Octroi de l’autorisation de création d’objets ordinateur à l’objet nom de cluster](media/prestage-cluster-adds/granting-create-computer-objects-permission-to-the-cno.png)

  **Figure 3. L’octroi de l’autorisation d’objets de créer un ordinateur pour le CNO**
10. Sélectionnez **OK** jusqu'à ce que vous reveniez aux utilisateurs Active Directory et les ordinateurs d’un composant logiciel enfichable.

Un administrateur sur le cluster de basculement peut maintenant créer des rôles en cluster avec des points d’accès client, puis mettre les ressources en ligne.

### <a name="prestage-a-vco-for-a-clustered-role"></a>Préparer un objet ordinateur virtuel pour un rôle en cluster

1. Avant de commencer, assurez-vous de connaître le nom du cluster et celui que le rôle en cluster portera.
2. Dans Utilisateurs et ordinateurs Active Directory, dans le menu **Affichage** , vérifiez que l’option **Fonctionnalités avancées** est sélectionnée.
3. Dans Active Directory utilisateurs et ordinateurs, cliquez sur l’unité d’organisation où le CNO pour réside le cluster, pointez sur **New**, puis sélectionnez **ordinateur**.
4. Dans le **nom de l’ordinateur** , entrez le nom que vous allez utiliser pour le rôle en cluster et puis sélectionnez **OK**.
5. Comme meilleure pratique, cliquez sur le compte d’ordinateur que vous venez de créer, sélectionnez **propriétés**, puis sélectionnez le **objet** onglet. Sur le **objet** onglet, sélectionnez le **protéger l’objet des suppressions accidentelles** case à cocher, puis sélectionnez **OK**.
6. Cliquez sur le compte d’ordinateur que vous venez de créée, puis sélectionnez **propriétés**.
7. Sur le **sécurité** onglet, sélectionnez **ajouter**.
8. Dans le **sélectionner un utilisateur, ordinateur, compte de Service ou groupes** boîte de dialogue, sélectionnez **Types d’objets**, sélectionnez le **ordinateurs** case à cocher, puis sélectionnez **OK** .
9. Sous **Entrez les noms des objets à sélectionner**, entrez le nom de l’objet nom de cluster, sélectionnez **vérifier les noms**, puis sélectionnez **OK**. Si vous recevez un message d’avertissement indiquant que vous êtes sur le point d’ajouter un objet désactivé, sélectionnez **OK**.
10. Assurez-vous que l’objet nom de cluster est sélectionné puis, en regard de **Contrôle total**, activez la case à cocher **Autoriser** .
11. Sélectionnez **OK**.

Un administrateur sur le cluster de basculement peut maintenant créer le rôle en cluster avec un point d’accès client qui correspond au nom de l’objet ordinateur virtuel prédéfini, puis mettre la ressource en ligne.

## <a name="more-information"></a>Informations supplémentaires

- [Clustering de basculement](failover-clustering.md)