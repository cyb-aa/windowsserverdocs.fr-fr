---
title: Préinstaller des objets ordinateur de cluster dans les Services de domaine Active Directory
description: Comment préconfigurer les objets ordinateur de cluster dans les Services de domaine Active Directory.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/25/2018
ms.localizationpriority: medium
ms.openlocfilehash: 111969b074b33764dbbf72bfb24ad606f8314e41
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/22/2018
ms.locfileid: "2082070"
---
# <a name="prestage-cluster-computer-objects-in-active-directory-domain-services"></a>Préinstaller des objets ordinateur de cluster dans les Services de domaine Active Directory

>S’applique à: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Cette rubrique montre comment préconfigurer les objets ordinateur de cluster dans les Services de domaine Active Directory (AD DS). Vous pouvez utiliser cette procédure pour activer un utilisateur ou un groupe créer un cluster de basculement lorsqu’ils ne disposent pas d’autorisations pour créer des objets ordinateur dans AD DS.

Lorsque vous créez un cluster de basculement à l’aide de l’Assistant Création d’un Cluster ou en utilisant Windows PowerShell, vous devez spécifier un nom pour le cluster. Si vous disposez des autorisations suffisantes lorsque vous créez le cluster, le processus de création de cluster crée automatiquement un objet ordinateur dans AD DS qui correspond au nom de cluster. Cet objet est appelé *l’objet nom de cluster* ou d’un objet réseau de cluster. Par le biais de l’objet réseau de cluster, les objets de l’ordinateur virtuel (VCO) sont automatiquement créés lors de la configuration des rôles en cluster qui utilisent des points d’accès client. Par exemple, si vous créez un serveur de fichiers hautement disponible avec un point d’accès client qui est nommé *FileServer1*, l’objet réseau de cluster créera un VCO correspondant dans AD DS.

>[!NOTE]
>Dans Windows Server 2012 R2, il est l’option permettant de créer un cluster détachée d’Active Directory, où aucun objet réseau de cluster ou le VCO n’est créés dans AD DS. Il est prévu pour des types spécifiques de déploiements de clusters. Pour plus d’informations, voir [déploiement d’un Cluster Active Directory-Detached](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v%3dws.11)>).

Pour créer l’objet réseau de cluster automatiquement, l’utilisateur qui crée le cluster de basculement doit avoir l’autorisation **d’objets ordinateur créer** à l’unité d’organisation (UO) ou le conteneur où résident les serveurs qui seront former le cluster. Pour activer un utilisateur ou un groupe créer un cluster sans avoir cette autorisation, un utilisateur disposant des autorisations appropriées dans AD DS (généralement, un administrateur de domaine) peut préconfigurer l’objet réseau de cluster dans AD DS. Il fournit également l’administrateur de domaine plus contrôler la convention d’affectation de noms qui est utilisée pour le cluster et contrôle sur quelle unité d’organisation, les objets cluster sont créés dans.

## <a name="step-1-prestage-the-cno-in-ad-ds"></a>Étape 1: Préconfigurer l’objet réseau de cluster dans AD DS

Avant de commencer, assurez-vous que vous connaissez les éléments suivants:

- Le nom que vous voulez attribuer le cluster
- Le nom du compte d’utilisateur ou du groupe auquel vous souhaitez accorder des droits pour créer le cluster

Meilleure pratique, nous vous recommandons que vous créez une unité d’organisation pour les objets du cluster. Si une unité d’organisation existe déjà que vous souhaitez utiliser, l’appartenance au groupe **Opérateurs de compte** est le minimum requis pour effectuer cette étape. Si vous avez besoin créer une unité d’organisation pour les objets de cluster, l’appartenance au groupe **Admins du domaine** , ou équivalent, est le minimum requis pour effectuer cette étape.

>[!NOTE]
>Si vous créez l’objet réseau de cluster dans le conteneur ordinateurs par défaut au lieu d’une unité d’organisation, vous n’avez pas effectuer l’étape 3 de cette rubrique. Dans ce scénario, un administrateur de cluster peut créer jusqu'à 10 VCO sans configuration supplémentaire.

### <a name="prestage-the-cno-in-ad-ds"></a>Préconfigurer l’objet réseau de cluster dans AD DS

1. Sur un ordinateur doté des outils AD DS installée dans les outils d’Administration de serveur distant, ou sur un contrôleur de domaine, ouvrez **utilisateurs Active Directory et les ordinateurs**. Pour ce faire, sur un serveur, démarrez le Gestionnaire de serveur, puis, dans le menu **Outils** , sélectionnez **Active Directory utilisateurs et ordinateurs**.
2. Pour créer une unité d’organisation pour le cluster objets ordinateur, avec le bouton droit le nom de domaine ou une unité d’organisation existante, pointez sur **Nouveau**, puis sélectionnez **Unité d’organisation**. Dans la zone **nom** , entrez le nom de l’unité d’organisation, puis sélectionnez **OK**.
3. Dans l’arborescence de la console, cliquez sur l’unité d’organisation où vous souhaitez créer l’objet réseau de cluster, pointez sur **Nouveau**, puis sélectionnez **ordinateur**.
4. Dans la zone **nom de l’ordinateur** , entrez le nom qui sera utilisé pour le cluster de basculement, puis sélectionnez **OK**.

  >[!NOTE]
  >C’est le nom de cluster qui spécifie l’utilisateur qui crée le cluster dans la page de **Point d’accès pour administrer le Cluster** dans l’Assistant créer un Cluster ou en tant que la valeur de la *– nom* paramètre pour le **Nouveau Cluster** Windows PowerShell applet de commande.

5. Meilleure pratique, avec le bouton droit le compte d’ordinateur que vous venez de créer, sélectionnez **Propriétés**, puis sélectionnez l’onglet **objet** . Sous l’onglet **objet** , activez la case à cocher **objet protéger contre la suppression accidentelle** , puis sélectionnez **OK**.
6. Cliquez sur le compte d’ordinateur que vous venez de créer, puis sélectionnez **Désactiver le compte**. Sélectionnez **Oui** pour confirmer, puis sélectionnez **OK**.

  >[!NOTE]
  >Vous devez désactiver le compte afin que lors de la création du cluster, le processus de création du cluster peut confirmer que le compte n’est pas actuellement en cours d’utilisation par un ordinateur existant ou d’un cluster dans le domaine.

![Objet réseau de cluster désactivé dans l’unité d’organisation des Clusters exemple](media/prestage-cluster-adds/disabled-cno-in-the-example-clusters-ou.png)

**Figure1. Objet réseau de cluster désactivé dans l’unité d’organisation des Clusters exemple**

## <a name="step-2-grant-the-user-permissions-to-create-the-cluster"></a>Étape 2: Accorder les autorisations utilisateur pour créer le cluster

Vous devez configurer des autorisations de sorte que le compte d’utilisateur qui sera utilisé pour créer le cluster de basculement dispose des autorisations Contrôle total sur l’objet réseau de cluster.

L’appartenance au groupe **Opérateurs de compte** est le minimum requis pour effectuer cette étape.

Voici comment accorder les autorisations utilisateur pour créer le cluster:

1. Dans Active Directory utilisateurs et ordinateurs, dans le menu **affichage** , assurez-vous que les **Fonctionnalités avancées** est sélectionnée.
2. Recherchez et puis cliquez sur l’objet réseau de cluster, puis sélectionnez **Propriétés**.
3. Sous l’onglet **sécurité** , sélectionnez **Ajouter**.
4. Dans la boîte de dialogue **Sélectionner des utilisateurs, ordinateurs ou groupes** , spécifiez le compte d’utilisateur ou un groupe que vous souhaitez accorder des autorisations à, puis sélectionnez **OK**.
5. Sélectionnez le compte d’utilisateur ou le groupe que vous venez d’ajouter, puis sélectionnez la case à cocher **Autoriser** en regard de **contrôle total**.
  
  ![L’autorisation contrôle total à l’utilisateur ou du groupe qui permet de créer le cluster](media/prestage-cluster-adds/granting-full-control-to-the-user-create-the-cluster.png)
  
  **Figure2. L’autorisation contrôle total à l’utilisateur ou du groupe qui permet de créer le cluster**
6. Sélectionnez **OK**.

Après avoir terminé cette étape, vous octroyer des autorisations à l’utilisateur peut créer le cluster de basculement. Toutefois, si l’objet réseau de cluster se trouve dans une unité d’organisation, l’utilisateur ne peut pas créer des rôles en cluster qui nécessitent un point d’accès client jusqu'à ce que vous avez terminé l’étape 3.

>[!NOTE]
>Si l’objet réseau de cluster se trouve dans le conteneur ordinateurs par défaut, un administrateur de cluster peut créer jusqu'à 10 VCO sans configuration supplémentaire. Pour ajouter plus de 10 VCO, vous devez explicitement accorder l’autorisation de **l’ordinateur de créer des objets** à l’objet réseau de cluster pour le conteneur ordinateurs.

## <a name="step-3-grant-the-cno-permissions-to-the-ou-or-prestage-vcos-for-clustered-roles"></a>Étape 3: Accorder les autorisations de l’objet réseau de cluster à l’unité d’organisation ou prédéfinir VCO pour les rôles en cluster

Lorsque vous créez un rôle en cluster avec un point d’accès client, le cluster crée un VCO dans la même unité d’organisation en tant que l’objet réseau de cluster. Pour ce faire automatiquement, l’objet réseau de cluster doit avoir des autorisations pour créer des objets ordinateur dans l’unité d’organisation.

Si vous prédéfini de l’objet réseau de cluster dans AD DS, vous pouvez effectuer une des options suivantes pour créer le VCO:

- Option 1: [accorder les autorisations de l’objet réseau de cluster sur l’unité d’organisation](#grant-the-cno-permissions-to-the-ou). Si vous utilisez cette option, le cluster peut créer automatiquement VCO dans AD DS. Par conséquent, un administrateur de cluster de basculement peut créer des rôles en cluster sans avoir à demander que vous prédéfinissez VCO dans AD DS.

>[!NOTE]
>L’appartenance au groupe **Admins du domaine** , ou équivalent, est le minimum requis pour effectuer les étapes de cette option.

- Option 2: [Prestage un VCO pour un rôle en cluster](#prestage-a-vco-for-the-clustered-role). Utilisez cette option si nécessaire préconfigurer les comptes pour les rôles en cluster en raison des exigences de votre organisation. Par exemple, vous souhaiterez peut-être contrôler la convention d’affectation de noms, ou le contrôle les rôles en clusters sont créés.

>[!NOTE]
>L’appartenance au groupe **Opérateurs de compte** est le minimum requis pour effectuer les étapes de cette option.

### <a name="grant-the-cno-permissions-to-the-ou"></a>Accorder les autorisations de l’objet réseau de cluster à l’unité d’organisation

1. Dans Active Directory utilisateurs et ordinateurs, dans le menu **affichage** , assurez-vous que les **Fonctionnalités avancées** est sélectionnée.
2. Avec le bouton droit de l’unité d’organisation où vous avez créé l’objet réseau de cluster dans [étape 1: préconfigurer l’objet réseau de cluster dans AD DS](#step-1:-prestage-the-CNO-in-ad-ds), puis sélectionnez **Propriétés**.
3. Sous l’onglet **sécurité** , cliquez sur **Avancé**.
4. Dans la boîte de dialogue **Paramètres de sécurité avancés** , sélectionnez **Ajouter**.
5. En regard de **Principal**, sélectionnez **Sélectionner un principal**.
6. Dans la boîte de dialogue **Sélectionner un utilisateur, ordinateur, le compte de Service ou des groupes** , sélectionnez les **Types d’objets**, activez la case à cocher **ordinateurs** , puis sélectionnez **OK**.
7. Dans la zone **Entrez les noms des objets à sélectionner**, entrez le nom de l’objet réseau de cluster, sélectionnez **Vérifier les noms**, puis sélectionnez **OK**. En réponse au message d’avertissement indiquant que vous allez ajouter un objet désactivé, sélectionnez **OK**.
8. Dans la boîte de dialogue **Entrée d’autorisation** , assurez-vous que la liste **Type** est définie sur **Autoriser**, et la liste **s’applique à** est définie sur **cet objet et tous les objets descendants**.
9. Sous **autorisations**, activez la case à cocher **objets ordinateur créer** .

  ![Accorder l’autorisation d’objets de créer un ordinateur à l’objet réseau de cluster](media/prestage-cluster-adds/granting-create-computer-objects-permission-to-the-cno.png)

  **Figure3. Accorder l’autorisation d’objets de créer un ordinateur à l’objet réseau de cluster**
10. Sélectionnez **OK** jusqu'à ce que vous revenez à Active Directory utilisateurs et ordinateurs.

Un administrateur sur le cluster de basculement permettre maintenant créer rôles en cluster avec client points d’accès et mettre les ressources en ligne.

### <a name="prestage-a-vco-for-a-clustered-role"></a>Préconfigurer un VCO pour un rôle en cluster

1. Avant de commencer, assurez-vous que vous connaissez le nom du cluster et le nom que le rôle de cluster.
2. Dans Active Directory utilisateurs et ordinateurs, dans le menu **affichage** , assurez-vous que les **Fonctionnalités avancées** est sélectionnée.
3. Dans utilisateurs et ordinateurs, avec le bouton droit de l’unité d’organisation dans laquelle réside l’objet réseau de cluster pour le cluster, pointez sur **Nouveau**, puis sélectionnez **ordinateur**.
4. Dans la zone **nom de l’ordinateur** , entrez le nom que vous utilisez pour le rôle en cluster et puis sélectionnez **OK**.
5. Meilleure pratique, avec le bouton droit le compte d’ordinateur que vous venez de créer, sélectionnez **Propriétés**, puis sélectionnez l’onglet **objet** . Sous l’onglet **objet** , activez la case à cocher **objet protéger contre la suppression accidentelle** , puis sélectionnez **OK**.
6. Cliquez sur le compte d’ordinateur que vous venez de créer, puis sélectionnez **Propriétés**.
7. Sous l’onglet **sécurité** , sélectionnez **Ajouter**.
8. Dans la boîte de dialogue **Sélectionner un utilisateur, ordinateur, le compte de Service ou des groupes** , sélectionnez les **Types d’objets**, activez la case à cocher **ordinateurs** , puis sélectionnez **OK**.
9. Dans la zone **Entrez les noms des objets à sélectionner**, entrez le nom de l’objet réseau de cluster, sélectionnez **Vérifier les noms**, puis sélectionnez **OK**. Si vous recevez un message d’avertissement indiquant que vous êtes sur le point d’ajouter un objet désactivé, sélectionnez **OK**.
10. Assurez-vous que l’objet réseau de cluster est sélectionné, puis activez la case à cocher **Autoriser** en regard de **contrôle total**.
11. Sélectionnez **OK**.

Un administrateur sur le cluster de basculement peut maintenant créer le rôle en cluster avec un point d’accès client qui correspond au nom VCO prédéfini et mettre la ressource en ligne.

## <a name="more-information"></a>Informations supplémentaires

- [Clustering de basculement](failover-clustering.md)