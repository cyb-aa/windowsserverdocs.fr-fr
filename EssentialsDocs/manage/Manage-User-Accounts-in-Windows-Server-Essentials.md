---
title: "Gérer les comptes d’utilisateur dans Windows Server Essentials"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0d115697-532b-48c2-a659-9f889e235326
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 91175836e4453860b17d2655e6a5a831645de410
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="manage-user-accounts-in-windows-server-essentials"></a>Gérer les comptes d’utilisateur dans Windows Server Essentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

La page utilisateurs du tableau de bord Windows Server Essentials centralise les informations et les tâches qui vous aident à gérer les comptes d’utilisateur sur votre réseau de petite entreprise. Pour une vue d’ensemble du tableau de bord d’utilisateurs, voir [vue d’ensemble du tableau de bord](Overview-of-the-Dashboard-in-Windows-Server-Essentials.md).  
  
  
##  <a name="BKMK_ManageAccounts"></a>La gestion des comptes d’utilisateur  
 Les rubriques suivantes fournissent des informations sur l’utilisation du tableau de bord Windows Server Essentials pour gérer les comptes d’utilisateur sur le serveur:  
  
-   [Ajouter un compte d’utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1)  
  
-   [Supprimer un compte d’utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Remove)  
  
-   [Afficher les comptes utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage3)  
  
-   [Modifier le nom d’affichage pour le compte d’utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage4)  
  
-   [Activer un compte d’utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage5)  
  
-   [Désactiver un compte d’utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage6)  
  
-   [Comprendre les comptes d’utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage7)  
  
-   [Gérer les comptes d’utilisateur à l’aide du tableau de bord](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8)  
  
###  <a name="BKMK_Manage1"></a>Ajouter un compte d’utilisateur  
 Lorsque vous ajoutez un compte d’utilisateur, l’utilisateur affecté peut ouvrir une session sur le réseau, et vous pouvez donner à l’utilisateur l’autorisation d’accéder aux ressources de réseau tels que les dossiers partagés et le site accès Web à distance. Windows Server Essentials comprend l’Assistant Ajouter un utilisateur compte qui vous permet de:  
  
-   Fournir un nom et un mot de passe pour le compte d’utilisateur.  
  
-   Définir le compte soit en tant qu’administrateur ou en tant qu’utilisateur standard.  
  
-   Sélectionnez les le compte d’utilisateur peut accéder à des dossiers partagés.  
  
-   Spécifiez si le compte d’utilisateur a accès à distance au réseau.  
  
-   Sélectionnez options de messagerie, le cas échéant.  
  
-   Affecter un compte Microsoft Online Services (appelé un compte Office 365 dans Windows Server Essentials), le cas échéant.  
  
-   Affecter des groupes d’utilisateurs (Windows Server Essentials uniquement).  
  
> [!NOTE]
>  -   Les caractères non-ASCII ne sont pas pris en charge dans Microsoft Azure Active Directory (AD Azure). N’utilisez pas de caractères non-ASCII dans votre mot de passe, si votre serveur est intégré à Azure AD.  
> -   Les options de messagerie sont uniquement disponibles si vous installez un complément qui fournit le service de messagerie.  
  
##### <a name="to-add-a-user-account"></a>Pour ajouter un compte d’utilisateur  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **utilisateurs**.  
  
3.  Dans le **tâches utilisateurs** volet, cliquez sur **ajouter un compte d’utilisateur**. L’ajout de qu'un Assistant de compte d’utilisateur s’affiche.  
  
4.  Suivez les instructions pour terminer l’Assistant.  
  
###  <a name="BKMK_Remove"></a>Supprimer un compte d’utilisateur  
 Lorsque vous choisissez de supprimer un compte d’utilisateur à partir du serveur, un Assistant supprime le compte sélectionné. Pour cette raison, vous ne pouvez plus utiliser le compte pour se connecter au réseau ou accéder aux ressources réseau. En option, vous pouvez également supprimer les fichiers pour le compte d’utilisateur en même temps que vous supprimez le compte. Si vous ne souhaitez pas supprimer définitivement le compte d’utilisateur, vous pouvez désactiver le compte d’utilisateur à la place pour interrompre l’accès aux ressources réseau.  
  
> [!IMPORTANT]
>  Si un compte d’utilisateur dispose d’un Microsoft en ligne compte affecté, lorsque vous supprimez le compte d’utilisateur, le compte en ligne est également supprimé de Microsoft Online Services, et les données utilisateur s, notamment le courrier électronique, sont soumis à des données dans Microsoft Online Services, les stratégies de rétention. Si vous souhaitez conserver les données d’utilisateur pour le compte en ligne, désactivez le compte d’utilisateur au lieu de supprimer. Pour plus d’informations, voir [gérer les comptes en ligne pour les utilisateurs](Manage-Online-Accounts-for-Users.md).  
  
##### <a name="to-remove-a-user-account"></a>Pour supprimer un compte d’utilisateur  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **utilisateurs**.  
  
3.  Dans la liste des comptes d’utilisateur, sélectionnez le compte d’utilisateur que vous souhaitez supprimer.  
  
4.  Dans le **< utilisateur\ > tâches** volet, cliquez sur **supprimer le compte d’utilisateur**. Supprimer un Assistant de compte d’utilisateur s’affiche.  
  
5.  Sur le **vous souhaitez conserver les fichiers? ** page de l’Assistant, vous pouvez choisir de supprimer les fichiers utilisateur s, y compris les sauvegardes de l’historique des fichiers et le dossier redirigé pour le compte d’utilisateur. Pour empêcher l’utilisateur fichiers s, laissez la case à cocher vide. Après avoir effectué votre sélection, cliquez sur **suivant**.  
  
6.  Cliquez sur **supprimer le compte**.  
  
> [!NOTE]
>  Après avoir supprimé un compte d’utilisateur, le compte n’apparaît plus dans la liste des comptes d’utilisateur. Si vous choisissez de supprimer les fichiers, le serveur supprime définitivement le dossier utilisateur s à partir de la **utilisateurs** dossier serveur et à partir de la **sauvegardes de l’historique des fichiers** dossier serveur.  
>   
>  Si vous avez un fournisseur de messagerie intégrée, le compte de messagerie affecté au compte d’utilisateur sera également être supprimé.  
  
###  <a name="BKMK_Manage3"></a>Afficher les comptes utilisateur  
 Le **utilisateurs** section du tableau de bord Windows Server Essentials affiche une liste des comptes d’utilisateur réseau. La liste fournit également des informations supplémentaires sur chaque compte.  
  
##### <a name="to-view-a-list-of-user-accounts"></a>Pour afficher la liste des comptes d’utilisateur  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **utilisateurs**.  
  
3.  Le tableau de bord affiche la liste actuelle des comptes d’utilisateur.  
  
##### <a name="to-view-or-change-properties-for-a-user-account"></a>Pour afficher ou modifier les propriétés d’un compte d’utilisateur  
  
1.  Dans la liste des comptes d’utilisateur, sélectionnez le compte pour lequel vous souhaitez afficher ou modifier les propriétés.  
  
2.  Dans le **< utilisateur\ > tâches** volet, cliquez sur **permet d’afficher les propriétés du compte**. Le **propriétés** page pour le compte d’utilisateur s’affiche.  
  
3.  Cliquez sur un onglet pour afficher les propriétés de cette fonctionnalité de compte.  
  
4.  Pour enregistrer les modifications que vous apportez aux propriétés de compte d’utilisateur, cliquez sur **appliquer**.  
  
###  <a name="BKMK_Manage4"></a>Modifier le nom d’affichage pour le compte d’utilisateur  
 Le nom d’affichage est le nom qui apparaît dans le **nom** colonne sur le **utilisateurs** page du tableau de bord. Modification du nom complet ne modifie pas le nom d’ouverture de session ou connectez-vous pour un compte d’utilisateur.  
  
##### <a name="to-change-the-display-name-for-a-user-account"></a>Pour modifier le nom complet d’un compte d’utilisateur  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **utilisateurs**.  
  
3.  Dans la liste des comptes d’utilisateur, sélectionnez le compte d’utilisateur que vous souhaitez modifier.  
  
4.  Dans le **< utilisateur\ > tâches** volet, cliquez sur **permet d’afficher les propriétés du compte**. Le **propriétés** page pour le compte d’utilisateur s’affiche.  
  
5.  Sur le **général** , tapez un nouveau **prénom** et **nom** pour le compte d’utilisateur, puis cliquez sur **OK**.  
  
     Le nouveau nom complet s’affiche dans la liste des comptes d’utilisateur.  
  
###  <a name="BKMK_Manage5"></a>Activer un compte d’utilisateur  
 Lorsque vous activez un compte d’utilisateur, l’utilisateur affecté peut se connecter aux ressources réseau réseau et d’accès auquel le compte a l’autorisation, tels que les dossiers partagés et le site accès Web à distance.  
  
> [!NOTE]
>  Vous pouvez uniquement activer un compte d’utilisateur qui est désactivé. Vous ne pouvez pas activer un compte d’utilisateur après avoir supprimé le serveur.  
  
##### <a name="to-activate-a-user-account"></a>Pour activer un compte d’utilisateur  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **utilisateurs**.  
  
3.  Dans la liste, sélectionnez le compte d’utilisateur que vous souhaitez activer.  
  
4.  Dans le **< utilisateur\ > tâches** volet, cliquez sur **activer le compte d’utilisateur**.  
  
5.  Dans la fenêtre de confirmation, cliquez sur **Oui** pour confirmer votre action.  
  
> [!NOTE]
>  Après avoir activé un compte d’utilisateur, l’état du compte affiche **Active**. Le compte d’utilisateur récupère les droits d’accès qui ont été affectés avant la désactivation du compte.  
>   
>  Si vous avez un fournisseur de messagerie intégrée, le compte de messagerie affecté au compte d’utilisateur est également activé.  
  
###  <a name="BKMK_Manage6"></a>Désactiver un compte d’utilisateur  
 Lorsque vous désactivez un compte d’utilisateur, compte d’accès au serveur est temporairement suspendu. Pour cette raison, l’utilisateur affecté ne peut pas utiliser le compte pour accéder aux ressources réseau telles que les dossiers partagés ou le site accès Web à distance jusqu'à ce que vous activiez le compte.  
  
 Si le compte d’utilisateur dispose d’un compte en ligne Microsoft, le compte en ligne est également désactivé. L’utilisateur ne peut pas utiliser les ressources d’Office 365 et d’autres services en ligne que vous vous abonnez à, mais les données utilisateur s, notamment le courrier électronique, sont conservées dans Microsoft Online Services.  
  
> [!NOTE]
>  Vous pouvez uniquement désactiver un compte d’utilisateur qui est actuellement actif.  
  
##### <a name="to-deactivate-a-user-account"></a>Pour désactiver un compte d’utilisateur  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **utilisateurs**.  
  
3.  Dans la liste, sélectionnez le compte d’utilisateur que vous souhaitez désactiver.  
  
4.  Dans le **< utilisateur\ > tâches** volet, cliquez sur **désactiver le compte d’utilisateur**.  
  
5.  Dans la fenêtre de confirmation, cliquez sur **Oui** pour confirmer votre action.  
  
> [!NOTE]
>  Une fois que vous désactivez un compte d’utilisateur, l’état du compte indique **inactif**.  
>   
>  Si vous avez un fournisseur de messagerie intégrée, le compte de messagerie affecté au compte d’utilisateur est également désactivé.  
  
###  <a name="BKMK_Manage7"></a>Comprendre les comptes d’utilisateur  
 Un compte d’utilisateur contient des informations importantes à Windows Server Essentials, ce qui permet d’accéder aux informations stockées sur le serveur et rend possible pour les utilisateurs individuels créer et gérer leurs fichiers et paramètres. Les utilisateurs peuvent se connecter à n’importe quel ordinateur sur le réseau s’ils disposent d’un compte d’utilisateur Windows Server Essentials et ils disposent d’autorisations pour accéder à un ordinateur. Les utilisateurs accéder à leurs comptes d’utilisateur avec un nom d’utilisateur et mot de passe.  
  
 Il existe deux principaux types de comptes d’utilisateur. Chaque type fournit aux utilisateurs un autre niveau de contrôle sur l’ordinateur:  
  
-   **Standard** sont des comptes pour les opérations quotidiennes. Le compte d’utilisateur standard permet de protéger votre réseau en empêchant les utilisateurs d’apporter des modifications qui affectent les autres utilisateurs, telles que la suppression des fichiers ou changer les paramètres réseau.  
  
-   **Administrateur** comptes fournissent le meilleur contrôle sur un réseau d’ordinateur. Vous devez attribuer à l’administrateur du compte de type uniquement lorsque cela est nécessaire.  
  
###  <a name="BKMK_Manage8"></a>Gérer les comptes d’utilisateur à l’aide du tableau de bord  
 Windows Server Essentials permet d’effectuer des tâches d’administration courantes à l’aide du tableau de bord Windows Server Essentials. Par défaut, le **utilisateurs** page du tableau de bord comprend deux onglets **utilisateurs** et **groupes d’utilisateurs**.  
  
> [!NOTE]
>  -   Si vous intégrez votre serveur qui exécute Windows Server Essentials à Office 365, un nouvel onglet appelé **groupes de Distribution** est également ajouté à la **utilisateurs** page du tableau de bord.  
> -   Dans Windows Server Essentials, le **utilisateurs** page du tableau de bord inclut uniquement l’onglet - **utilisateurs**.  
  
 Le **utilisateurs** onglet inclut les éléments suivants:  
  
-   Liste des comptes d’utilisateur, qui indique:  
  
    -   Le nom de l’utilisateur.  
  
    -   Le nom d’ouverture de session pour le compte d’utilisateur.  
  
    -   Indique si le compte d’utilisateur a l’autorisation d’accès en tout lieu. N’importe où l’autorisation d’accès pour un compte d’utilisateur est **autorisé** ou **ne pas autorisé**.  
  
    -   Indique si l’historique des fichiers pour ce compte d’utilisateur est géré par le serveur exécutant Windows Server Essentials. L’état de l’historique des fichiers d’un compte d’utilisateur est soit **géré** ou **ne pas gérés**.  
  
    -   Le niveau d’accès qui est affecté au compte d’utilisateur. Vous pouvez affecter le **utilisateur Standard** accès ou **administrateur** accès pour un compte d’utilisateur.  
  
    -   L’état du compte utilisateur. Un compte d’utilisateur peut être **Active**, **inactif**, ou **incomplet**.  
  
    -   Dans Windows Server Essentials, si le serveur est intégré à Office 365 ou Windows Intune, le compte en ligne Microsoft s’affiche.  
  
    -   Dans Windows Server Essentials, si le serveur est intégré à Microsoft Office 365, l’état du compte Office 365 (appelé Windows Server Essentials en tant que le compte en ligne Microsoft) pour le compte d’utilisateur s’affiche.  
  
-   Un volet d’informations avec des informations supplémentaires sur un compte d’utilisateur sélectionné.  
  
-   Un volet des tâches qui inclut:  
  
    -   Un ensemble de tâches d’administration de compte utilisateur telles que l’affichage et suppression de comptes d’utilisateur et la modification des mots de passe.  
  
    -   Tâches qui vous permettent de définir globalement ou de modifier les paramètres pour tous les comptes d’utilisateur dans le réseau.  
  
 Le tableau suivant décrit les différentes tâches de compte utilisateur qui sont disponibles à partir de la **utilisateurs** onglet. Certaines tâches sont spécifiques aux comptes d’utilisateur, et elles ne sont visibles que lorsque vous sélectionnez un compte d’utilisateur dans la liste.  
  
> [!NOTE]
>  Si vous intégrez Office 365 avec Windows Server Essentials, des tâches supplémentaires sont disponibles. Pour plus d’informations, voir [gérer les comptes en ligne pour les utilisateurs](Manage-Online-Accounts-for-Users.md).  
  
### <a name="user-account-tasks-in-the-dashboard"></a>Tâches de compte d’utilisateur dans le tableau de bord  
  
|Nom de la tâche|Description|  
|---------------|-----------------|  
|Afficher les propriétés du compte|Vous permet d’afficher et modifier les propriétés du compte d’utilisateur sélectionné et pour spécifier des autorisations d’accès de dossier pour le compte.|  
|Désactiver le compte d’utilisateur|Un compte d’utilisateur désactivé ne peut pas ouvrir une session au réseau ou accès ressources réseau telles que les dossiers partagés ou les imprimantes.|  
|Activer le compte d’utilisateur|Un compte d’utilisateur qui est activé peut se connecter au réseau et peut accéder aux ressources réseau, tel que défini par les autorisations du compte.|  
|Supprimer le compte d’utilisateur|Vous permet de supprimer le compte d’utilisateur sélectionné.|  
|Modifier le mot de passe de compte d’utilisateur|Vous permet de réinitialiser le mot de passe réseau pour le compte d’utilisateur sélectionné.|  
|Ajouter un compte d’utilisateur|Démarre l’Assistant Ajouter un utilisateur compte, qui vous permet de créer un nouveau compte d’utilisateur unique disposant d’accès utilisateur standard ou accès administrateur.|  
|Affecter un compte en ligne Microsoft|Ajoute un compte en ligne Microsoft pour le compte d’utilisateur réseau local sélectionné.<br /><br /> Cette tâche s’affiche lorsque votre serveur est intégré à Microsoft online services, comme Office 365.|  
|Ajouter des comptes en ligne Microsoft|Ajoute des comptes en ligne Microsoft et les associe aux comptes d’utilisateur réseau local.<br /><br /> Cette tâche s’affiche lorsque votre serveur est intégré à Microsoft online services, comme Office 365.|  
|Définir la stratégie de mot de passe|Permet de modifier les valeurs du mot de passe des stratégies pour votre réseau.|  
|Importer des comptes en ligne Microsoft|Effectue une importation en bloc des comptes de services en ligne Microsoft dans le réseau local.<br /><br /> Cette tâche s’affiche lorsque votre serveur est intégré à Microsoft online services, comme Office 365.|  
|Actualisation|Actualise l’onglet utilisateurs.<br /><br /> Cette tâche s’applique à Windows Server Essentials.|  
|Modifier les paramètres de l’historique des fichiers|Vous permet de modifier les paramètres de l’historique des fichiers, tels que la fréquence de sauvegarde ou la durée de sauvegarde.<br /><br /> Cette tâche s’applique à Windows Server Essentials.|  
|Exporter toutes les connexions à distance|Crée un. Fichier au format CSV de toutes les connexions à distance au serveur qui se sont produits au cours des 30 derniers jours.|  
  
##  <a name="BKMK_ManageAccess"></a>La gestion des mots de passe et d’accès  
 Les rubriques suivantes fournissent des informations sur l’utilisation du tableau de bord Windows Server Essentials pour gérer les mots de passe de compte utilisateur et des accès utilisateur aux dossiers partagés sur le serveur:  
  
-   [Modifier ou réinitialiser le mot de passe pour un compte d’utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access1)  
  
-   [Ce que vous devez savoir sur les stratégies de mot de passe](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access3)  
  
-   [Modifier la stratégie de mot de passe](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access4)  
  
-   [Niveau d’accès aux dossiers partagés](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access5)  
  
-   [Conserver et gérer l’accès aux fichiers pour les comptes d’utilisateur supprimés](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access6)  
  
-   [Synchroniser le mot de passe DSRM avec le mot de passe administrateur réseau](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access7)  
  
-   [Donner des comptes d’utilisateur l’autorisation Bureau à distance](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access8)  
  
-   [Permettre aux utilisateurs d’accéder aux ressources sur le serveur](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access9)  
  
-   [Modifier les autorisations d’accès à distance pour un compte d’utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access10)  
  
-   [Modifier les autorisations de réseau privé virtuel pour un compte d’utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access11)  
  
-   [Modifier l’accès aux dossiers partagés internes pour un compte d’utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access12)  
  
-   [Autoriser les comptes d’utilisateur établir une session Bureau à distance à leur ordinateur](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access13)  
  
###  <a name="BKMK_Access1"></a>Modifier ou réinitialiser le mot de passe pour un compte d’utilisateur  
 Pour modifier ou réinitialiser un mot de passe de compte d’utilisateur, procédez comme suit.  
  
##### <a name="to-reset-the-password-for-a-user-account"></a>Pour réinitialiser le mot de passe pour un compte d’utilisateur  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **utilisateurs**.  
  
3.  Dans la liste des comptes d’utilisateur, sélectionnez le compte d’utilisateur que vous voulez réinitialiser.  
  
4.  Dans le **< utilisateur\ > tâches** volet, cliquez sur **modifier le mot de passe du compte utilisateur**. L’Assistant de mot de passe de compte utilisateur modification s’affiche.  
  
5.  Tapez un nouveau mot de passe pour le compte d’utilisateur, puis tapez le mot de passe pour le confirmer.  
  
6.  Cliquez sur **modification mot de passe**.  
  
7.  Fournir le nouveau mot de passe à l’utilisateur.  
  
    > [!IMPORTANT]
    >  -   Vous ne pourrez pas modifier votre mot de passe si la stratégie de mot de passe pour votre compte a été définie pour **mots de passe n’expirent jamais**.  
    > -   Les caractères non-ASCII ne sont pas pris en charge dans Azure AD. Par conséquent, si votre serveur est intégré à Azure AD, n’utilisez pas les caractères non-ASCII dans votre mot de passe.  
    > -   Si un compte en ligne Microsoft (appelé Windows Server Essentials en tant qu’un compte Office 365) est affecté à l’utilisateur, le mot de passe est synchronisé avec le mot de passe du compte en ligne. L’utilisateur utilise le nouveau mot de passe pour vous connecter sur le serveur ou de se connecter à Office 365. Pour plus d’informations, voir [gérer les comptes en ligne pour les utilisateurs](Manage-Online-Accounts-for-Users.md).  
  
###  <a name="BKMK_Access3"></a>Ce que vous devez savoir sur les stratégies de mot de passe  
 La stratégie de mot de passe est un ensemble de règles qui définissent comment les utilisateurs créent et utilisent des mots de passe. La stratégie contribue à empêcher tout accès non autorisé aux données utilisateur et d’autres informations qui sont stockées sur le serveur. La stratégie de mot de passe est appliquée à tous les comptes d’utilisateurs qui accèdent au réseau.  
  
 La stratégie de mot de passe de Windows Server Essentials se compose de trois éléments principaux comme suit:  
  
-   **Longueur de mot de passe**.  Plus un mot de passe, il est plus sécurisé. Mots de passe vides ne sont pas sécurisés.  
  
-   **Complexité du mot de passe**.  Mots de passe complexes contiennent une combinaison de majuscules et minuscules (a-z, A-Z), de base chiffres (0-9) et symboles non alphabétiques (tels que;!, @, #, _,-). Mots de passe complexes sont beaucoup moins vulnérables aux accès non autorisés. Mots de passe qui contiennent des noms d’utilisateur, les dates de naissance ou autres informations personnelles n’offrent pas de sécurité adéquat.  
  
-   **Âge du mot de passe**.  Windows Server Essentials demande aux utilisateurs de changer leur mot de passe au moins une fois tous les 180 jours. En option, vous pouvez choisir d’avoir des mots de passe n’expire jamais.  
  
 Pour rendre plus facile à implémenter une stratégie de mot de passe sur votre réseau d’ordinateur, Windows Server Essentials fournit un outil simple qui vous permet de définir ou modifier la stratégie de mot de passe à l’un des quatre profils de stratégie prédéfinis suivants:  
  
-   **Faible**.  Les utilisateurs peuvent spécifier n’importe quel mot de passe qui n’est pas vide.  
  
-   **Moyenne**.  Ces mots de passe doivent contenir au moins 5 caractères. Un mot de passe complexe n’est pas nécessaire.  
  
-   **Moyen fort**.  Ces mots de passe doivent contenir au moins 5 caractères et doivent inclure des lettres, des chiffres et des symboles.  
  
-   **Fort**.  Ces mots de passe doivent contenir au moins 7 caractères et doivent inclure des lettres, des chiffres et des symboles. Ces mots de passe sont plus sécurisées, mais peuvent être plus difficiles à mémoriser.  
  
    > [!NOTE]
    >  Mots de passe ne peut pas contenir l’utilisateur nom ou adresse e-mail.  
    >   
    >  Si vous effectuez une intégration avec Office 365, elle applique la **fort** stratégie de mot de passe et met à jour pour inclure les exigences suivantes:  
    >   
    >  -   Mots de passe doivent contenir 8 16 caractères.  
    > -   Mots de passe ne peut pas contenir d’espaces, ni le nom de messagerie Office 365.  
  
 Par défaut, installation du serveur définit la stratégie de mot de passe par défaut le **fort** option.  
  
###  <a name="BKMK_Access4"></a>Modifier la stratégie de mot de passe  
 Utilisez la procédure suivante pour définir ou modifier la stratégie de mot de passe à l’un des quatre profils de stratégie prédéfinis.  
  
##### <a name="to-change-the-password-policy"></a>Pour modifier la stratégie de mot de passe  
  
1.  Ouvrez le tableau de bord Windows Server Essentials, puis cliquez sur **utilisateurs**.  
  
2.  Dans le **tâches utilisateurs** volet, cliquez sur **définir la stratégie de mot de passe**.  
  
3.  Sur le **modifier la stratégie de mot de passe** écran, définissez le niveau de mot de passe en déplaçant le curseur.  
  
     Microsoft recommande de définir le niveau de mot de passe **fort**.  
  
    > [!NOTE]
    >  En option, vous pouvez également sélectionner **mots de passe n’expirent jamais**. Ce paramètre est moins sécurisé, et par conséquent, il n’est pas recommandé.  
  
4.  Cliquez sur **modifier la stratégie**.  
  
###  <a name="BKMK_Access5"></a>Niveau d’accès aux dossiers partagés  
 Comme meilleure pratique, vous devez attribuer les autorisations plus restrictives disponibles permettant toujours aux utilisateurs d’effectuer les tâches requises.  
  
 Vous disposez de trois paramètres d’accès pour les dossiers partagés sur le serveur:  
  
-   **En lecture/écriture**.  Choisissez ce paramètre si vous souhaitez autoriser le compte d’utilisateur à créer, modifier et supprimer des fichiers dans le dossier partagé.  
  
-   **En lecture seule**.  Choisissez ce paramètre si vous souhaitez autoriser le compte d’utilisateur à lire uniquement les fichiers dans le dossier partagé. Comptes d’utilisateurs avec accès en lecture seule ne peut pas créer, modifier ou supprimer des fichiers dans le dossier partagé.  
  
-   **Aucun accès**.  Choisissez ce paramètre si vous ne souhaitez pas que le compte d’utilisateur pour accéder aux fichiers dans le dossier partagé.  
  
###  <a name="BKMK_Access6"></a>Conserver et gérer l’accès aux fichiers pour les comptes d’utilisateur supprimés  
 L’administrateur réseau peut supprimer un compte d’utilisateur et choisissez de conserver l’utilisateur fichiers s pour une utilisation ultérieure. Dans ce scénario, le compte d’utilisateur supprimé ne peut plus être utilisé pour se connecter au réseau; Toutefois, les fichiers de cet utilisateur seront être enregistrés dans un dossier partagé, ce qui peut être partagé avec un autre utilisateur.  
  
> [!IMPORTANT]
>  N’oubliez pas que si vous supprimez un compte d’utilisateur qui dispose d’un compte en ligne Microsoft, le compte en ligne est également supprimé, et les données utilisateur, notamment le courrier électronique, sont soumise aux politiques de rétention de données dans Microsoft Online Services. Pour conserver les données utilisateur pour le compte en ligne, désactivez le compte d’utilisateur au lieu de supprimer. Pour plus d’informations, voir [gérer les comptes en ligne pour les utilisateurs](Manage-Online-Accounts-for-Users.md).  
  
##### <a name="to-remove-a-user-account-but-retain-access-to-the-user-s-files"></a>Pour supprimer un compte d’utilisateur tout en conservant l’accès aux fichiers utilisateur s  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **utilisateurs**.  
  
3.  Dans la liste des comptes d’utilisateur, sélectionnez le compte d’utilisateur que vous souhaitez supprimer.  
  
4.  Dans le **< utilisateur\ > tâches** volet, cliquez sur **supprimer le compte d’utilisateur**. Supprimer un Assistant de compte d’utilisateur s’affiche.  
  
5.  Sur le **vous souhaitez conserver les fichiers? ** page, assurez-vous que le **supprimer les fichiers, y compris les sauvegardes de l’historique des fichiers et dossier redirigé pour ce compte d’utilisateur** vérifier la case est désactivée, puis cliquez sur **suivant**.  
  
     Une page de confirmation s’affiche vous avertir que vous supprimez le compte mais gardez les fichiers.  
  
6.  Cliquez sur **supprimer le compte** pour supprimer le compte d’utilisateur.  
  
 Une fois le compte d’utilisateur est supprimé, l’administrateur peut donner à un autre compte d’utilisateur accès au dossier partagé.  
  
##### <a name="to-give-a-user-account-permission-to-access-a-shared-folder"></a>Pour autoriser un compte d’utilisateur à accéder à un dossier partagé  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **stockage**, puis cliquez sur le **dossiers du serveur** onglet.  
  
3.  Dans la liste des dossiers, sélectionnez le **utilisateurs** dossier.  
  
4.  Dans le **tâches utilisateurs** volet, cliquez sur **ouvrir le dossier**. L’Explorateur Windows s’ouvre et affiche le contenu de la **utilisateurs** dossier.  
  
5.  Cliquez sur le dossier pour le compte d’utilisateur que vous souhaitez partager, puis cliquez sur **propriétés**.  
  
6.  Dans **< utilisateur\ > Propriétés**, cliquez sur le **partage** onglet, puis cliquez sur **partage**.  
  
7.  Dans le **le partage de fichiers** fenêtre, tapez ou sélectionnez le nom de compte d’utilisateur avec lequel vous souhaitez partager le dossier, puis cliquez sur **ajouter**.  
  
8.  Choisissez le **niveau d’autorisation** que vous souhaitez que le compte d’utilisateur, et puis cliquez sur **partage**.  
  
###  <a name="BKMK_Access7"></a>Synchroniser le mot de passe DSRM avec le mot de passe administrateur réseau  
 Mode de restauration des Services annuaire (DSRM) est un mode spécial de démarrage pour la réparation ou la récupération d’Active Directory. Le système d’exploitation utilise le mode DSRM pour se connecter à l’ordinateur si Active Directory est défaillant ou doit être restauré. Si votre mot de passe administrateur réseau et le mot de passe DSRM sont différents, DSRM ne se charge pas.  
  
 Lors d’une installation propre, première de Windows Server Essentials, le programme définit le mot de passe DSRM sur le réseau mot de passe que vous spécifiez pendant l’installation ou dans le fichier de réponses de migration. Lorsque vous modifiez votre mot de passe réseau administrateur, (de préférence, tous les 60 jours pour une sécurité accrue du serveur), la modification de mot de passe n’est pas transmise au mode DSRM. Cela entraîne une incompatibilité de mot de passe. Si cela se produit, vous pouvez utiliser les solutions suivantes pour manuellement ou synchroniser automatiquement votre mot de passe réseau administrateur s avec le mot de passe DSRM.  
  
##### <a name="to-manually-synchronize-the-dsrm-password-to-a-network-administrator-account"></a>Pour synchroniser manuellement le mot de passe DSRM sur un compte d’administrateur réseau  
  
1.  À l’invite de commandes, exécutez `ntdsutil.exe` pour ouvrir l’outil ntdsutil.  
  
2.  Pour réinitialiser le mot de passe DSRM, tapez **mot de passe dsrm ensemble**.  
  
3.  Pour synchroniser le mot de passe DSRM sur un contrôleur de domaine avec le compte s d’administrateur réseau actuel, tapez:  
  
     **synchroniser à partir du compte de domaine** *< compte_administrateur_réseau_actuel >*, puis appuyez sur ENTRÉE.  
  
 Étant donné que vous allez changer régulièrement le mot de passe pour le compte d’administrateur réseau pour vous assurer que le mot de passe DSRM est toujours le même que le mot de passe actuel de l’administrateur réseau, nous vous recommandons de créer automatiquement une tâche planifiée pour synchroniser le mot de passe DSRM au mot de passe administrateur réseau tous les jours.  
  
##### <a name="to-automatically-synchronize-the-dsrm-password-to-a-network-administrator-account"></a>Pour synchroniser automatiquement le mot de passe DSRM sur un compte d’administrateur réseau  
  
1.  À partir du serveur, ouvrez **outils d’administration**, puis double-cliquez sur **le Planificateur de tâches**.  
  
2.  Dans le Planificateur de tâches **Actions** volet, cliquez sur **créer une tâche**.  
  
3.  Dans le **nom** zone de texte, tapez un nom pour la tâche, par exemple **mot de passe DSRM synchronisation automatique**, puis sélectionnez le **exécuter avec les autorisations maximales** option.  
  
4.  Définir lorsque la tâche doit s’exécuter:  
  
    1.  Dans le **créer une tâche** boîte de dialogue, cliquez sur le **déclencheurs** onglet, puis cliquez sur **New**.  
  
    2.  Dans le **nouveau déclencheur** boîte de dialogue, sélectionnez votre option de périodicité, spécifiez l’intervalle de récurrence et choisissez une heure de début.  
  
        > [!NOTE]
        >  Comme meilleure pratique, vous devez définir la tâche s’exécute tous les jours pendant les heures creuses.  
  
    3.  Cliquez sur **OK** pour enregistrer vos modifications et revenir à la **créer une tâche** boîte de dialogue.  
  
5.  Définir les actions de la tâche:  
  
    1.  Cliquez sur le **Actions** onglet, puis cliquez sur **New**. Le **nouvelle Action** boîte de dialogue s’affiche.  
  
    2.  Dans le **Action** , cliquez sur **démarrer un programme**, puis accédez à **C:\WINDOWS\SYSTEM32\ntdsutil.exe**.  
  
    3.  Dans le **ajouter des arguments**texte (facultatif), tapez les éléments suivants (vous devez inclure les guillemets): **définir la synchronisation de mot de passe dsrm à partir du compte de domaine SBS_network_administrator_account q q** où *SBS_network_administrator_account* est le nom du compte administrateur s réseau actuel.  
  
6.  Cliquez sur **OK** deux fois pour enregistrer la tâche et fermer le **créer une tâche** boîte de dialogue. La nouvelle tâche apparaît dans le **tâches actives** section **planification de la tâche**.  
  
###  <a name="BKMK_Access8"></a>Donner des comptes d’utilisateur l’autorisation Bureau à distance  
 Dans l’installation par défaut de Windows Server Essentials, les utilisateurs du réseau n’êtes pas autorisé à établir une connexion à distance aux ordinateurs ou autres ressources sur le réseau.  
  
 Avant que les utilisateurs du réseau peuvent établir une connexion à distance aux ressources réseau, vous devez d’abord définir un accès en tout lieu. Après avoir configuré l’accès en tout lieu, les utilisateurs peuvent accéder les fichiers, applications et des ordinateurs de votre réseau d’entreprise à partir d’un appareil dans n’importe quel emplacement avec une connexion Internet.  
  
 L’Assistant accès en tout lieu configurer vous permet d’activer deux méthodes d’accès à distance:  
  
-   Réseau privé virtuel (VPN)  
  
-   Accès Web à distance  
  
 Lorsque vous exécutez l’Assistant, vous pouvez également choisir d’autoriser l’accès en tout lieu pour tous les comptes d’utilisateur actifs ou récemment ajoutés.  
  
 Pour configurer l’accès en tout lieu, ouvrez le tableau de bord **accueil** , cliquez sur **le programme d’installation**, puis cliquez sur **configurer l’accès en tout lieu**.  
  
 Pour plus d’informations sur l’accès en tout lieu, consultez [Manage Anywhere Access](Manage-Anywhere-Access-in-Windows-Server-Essentials.md).  
  
###  <a name="BKMK_Access9"></a>Permettre aux utilisateurs d’accéder aux ressources sur le serveur  
  Cette section s’applique à un serveur exécutant Windows Server Essentials ou Windows Server Essentials, ou à un serveur exécutant Windows Server 2012 R2 Standard ou Windows Server 2012 R2 Datacenter avec le rôle expérience Windows Server Essentials installé.  
  
 Si vous souhaitez que les utilisateurs à utiliser l’accès à distance et/ou disposent de comptes d’utilisateur individuels, après avoir terminé la connexion d’un ordinateur au serveur, vous pouvez créer nouveau réseau comptes d’utilisateur pour les utilisateurs de l’ordinateur en réseau sur le serveur à l’aide du tableau de bord. Pour plus d’informations sur la création d’un compte d’utilisateur, voir [ajouter un compte d’utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1). Après avoir créé les comptes d’utilisateur, vous devez fournir les informations de nom et mot de passe utilisateur de réseau pour les utilisateurs de l’ordinateur client afin qu’ils peuvent accéder aux ressources sur le serveur à l’aide du Launchpad.  
  
 Pour chaque compte d’utilisateur que vous créez vous pouvez définir des propriétés du compte d’accès pour les éléments suivants par le biais de l’utilisateur:  
  
-   **Dossiers partagés**.  Par défaut, les administrateurs réseau ont **en lecture/écriture** autorisation tous les dossiers partagés, les comptes d’utilisateurs standard ont **en lecture seule** autorisations au dossier société. Si la diffusion multimédia en continu est activée, vous pouvez affecter des autorisations d’accès aux comptes d’utilisateur standard pour les dossiers partagés suivants: **musique**, **images**, **TV enregistrée**, et **vidéos**. Vous pouvez définir des autorisations pour les comptes d’utilisateur accéder aux dossiers partagés sur le **dossiers partagés** onglet des propriétés du compte d’utilisateur.  
  
-   **Accès en tout lieu**.  Par défaut, permet aux administrateurs réseau VPN ou accès Web à distance pour accéder aux ressources de serveur. Pour les comptes d’utilisateur standard, vous devez définir les autorisations du compte d’utilisateur sur le **accès en tout lieu** onglet.  
  
-   **Accès à l’ordinateur**.  Par défaut, les administrateurs réseau peuvent accéder à tous les ordinateurs du réseau. Toutefois, pour les comptes d’utilisateur standard, vous pouvez définir des autorisations individuelles pour l’accès aux ordinateurs du réseau sur le **accès à l’ordinateur** onglet des propriétés du compte d’utilisateur.  
  
##### <a name="to-edit-user-account-properties-in-windows-server-essentials-2012-r2"></a>Pour modifier les propriétés du compte d’utilisateur dans Windows Server Essentials 2012 R2  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **utilisateurs**.  
  
3.  Dans la liste des comptes d’utilisateur, sélectionnez le compte d’utilisateur que vous souhaitez modifier.  
  
4.  Dans le **< utilisateur\ > tâches** volet, cliquez sur **permet d’afficher les propriétés du compte**.  
  
5.  Dans le **< utilisateur\ > Propriétés**, procédez comme suit:  
  
    1.  Sur le **dossiers partagés** onglet, définissez les autorisations de dossier approprié pour chaque dossier partagé en fonction des besoins.  
  
    2.  Sur le **accès en tout lieu** onglet:  
  
        1.  Pour autoriser un utilisateur à se connecter au serveur à l’aide de VPN, sélectionnez le **autoriser privé réseau virtuel (VPN)** case à cocher.  
  
        2.  Pour autoriser un utilisateur à se connecter au serveur à l’aide de l’accès Web à distance, sélectionnez le **autoriser l’accès Web à distance et l’accès aux applications de services web** case à cocher.  
  
    3.  Sur le **accès à l’ordinateur** , sélectionnez les ordinateurs du réseau que vous souhaitez que l’utilisateur d’accéder à.  
  
##### <a name="to-edit-user-account-properties-in-windows-server-essentials-2012"></a>Pour modifier les propriétés du compte d’utilisateur dans Windows Server 2012 Essentials  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **utilisateurs**.  
  
3.  Dans la liste des comptes d’utilisateur, sélectionnez le compte d’utilisateur que vous souhaitez modifier.  
  
4.  Dans le **< utilisateur\ > tâches** volet, cliquez sur **propriétés**.  
  
5.  Dans le **< utilisateur\ > Propriétés**, procédez comme suit:  
  
    1.  Sur le **général** onglet, sélectionnez **utilisateur peut afficher les alertes d’intégrité réseau** si le compte d’utilisateur a besoin d’accéder aux rapports d’intégrité du réseau.  
  
    2.  Sur le **dossiers partagés** onglet, définissez les autorisations de dossier approprié pour chaque dossier partagé en fonction des besoins.  
  
    3.  Sur le **accès en tout lieu** onglet:  
  
        1.  Pour autoriser un utilisateur à se connecter au serveur à l’aide de VPN, sélectionnez le **autoriser privé réseau virtuel (VPN)** case à cocher.  
  
        2.  Pour autoriser un utilisateur à se connecter au serveur à l’aide de l’accès Web à distance, sélectionnez le **autoriser l’accès Web à distance et l’accès aux applications de services web** case à cocher.  
  
    4.  Sur le **accès à l’ordinateur** , sélectionnez les ordinateurs du réseau que vous souhaitez que l’utilisateur d’accéder à.  
  
###  <a name="BKMK_Access10"></a>Modifier les autorisations d’accès à distance pour un compte d’utilisateur  
 Un utilisateur peut accéder aux ressources situées sur le serveur à partir d’un emplacement distant à l’aide d’un réseau privé virtuel (VPN), accès Web à distance ou autres applications de services web. Par défaut, les autorisations d’accès à distance sont activées pour les utilisateurs du réseau lorsque vous configurez l’accès en tout lieu dans Windows Server Essentials à l’aide du tableau de bord.  
  
##### <a name="to-change-remote-access-permissions-for-a-user-account"></a>Pour modifier les autorisations d’accès à distance pour un compte d’utilisateur  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **utilisateurs**.  
  
3.  Dans la liste des comptes d’utilisateur, sélectionnez le compte d’utilisateur que vous souhaitez modifier.  
  
4.  Dans le **< utilisateur\ > tâches** volet, cliquez sur **permet d’afficher les propriétés du compte**. Le **propriétés** page pour le compte d’utilisateur s’affiche.  
  
5.  Sur le **accès en tout lieu** onglet, procédez comme suit:  
  
    -   Sélectionnez le **autoriser privé réseau virtuel (VPN)** case à cocher pour autoriser un utilisateur à se connecter au serveur à l’aide de VPN.  
  
    -   Sélectionnez le **autoriser l’accès Web à distance et l’accès aux applications de services web** case à cocher pour autoriser un utilisateur à se connecter au serveur à l’aide de l’accès Web à distance.  
  
6.  Cliquez sur **appliquer**, puis cliquez sur **OK**.  
  
###  <a name="BKMK_Access11"></a>Modifier les autorisations de réseau privé virtuel pour un compte d’utilisateur  
 Vous pouvez utiliser un réseau privé virtuel (VPN) pour se connecter à Windows Server Essentials et accéder à toutes les ressources qui sont stockés sur le serveur. Cela est particulièrement utile si vous disposez d’un ordinateur client qui est configuré avec des comptes de réseau qui peuvent être utilisés pour se connecter à un serveur Windows Server Essentials hébergé via une connexion VPN. Tous les comptes d’utilisateur récemment créé sur le serveur Windows Server Essentials hébergé doivent utiliser VPN pour se connecter à l’ordinateur client pour la première fois.  
  
##### <a name="to-change-vpn-permissions-for-network-users"></a>Pour modifier les autorisations VPN des utilisateurs du réseau  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **utilisateurs**.  
  
3.  Dans la liste des comptes d’utilisateur, sélectionnez le compte d’utilisateur auquel vous voulez accorder des autorisations pour accéder au Bureau à distance.  
  
4.  Dans le **< utilisateur\ > tâches** volet, cliquez sur **propriétés**.  
  
5.  Dans le **< utilisateur\ > Propriétés**, cliquez sur le **accès en tout lieu** onglet.  
  
6.  Sur le **accès en tout lieu** onglet, pour autoriser un utilisateur à se connecter au serveur à l’aide de VPN, sélectionnez le **autoriser privé réseau virtuel (VPN)** case à cocher.  
  
7.  Cliquez sur **appliquer**, puis cliquez sur **OK**.  
  
###  <a name="BKMK_Access12"></a>Modifier l’accès aux dossiers partagés internes pour un compte d’utilisateur  
 Vous pouvez gérer l’accès à des dossiers partagés sur le serveur à l’aide des tâches sur le **dossiers du serveur** onglet du tableau de bord. Par défaut, les dossiers serveur suivants sont créés lorsque vous installez Windows Server Essentials:  
  
-   **Sauvegardes de l’ordinateur client**.  Utilisé pour stocker les sauvegardes de l’ordinateur client créés par la sauvegarde de Windows Server. Ce dossier serveur n’est pas partagé.  
  
-   **Société**.  Utilisé pour stocker et accéder aux documents liés à votre organisation par les utilisateurs du réseau.  
  
-   **Sauvegardes de l’historique des fichiers**.  Par défaut, Windows Server Essentials stocke les sauvegardes de fichiers créées à l’aide de l’historique des fichiers. Ce dossier serveur n’est pas partagé.  
  
-   **La Redirection de dossiers**.  Permet de stocker et d’accéder aux dossiers qui sont configurés pour la redirection de dossiers par les utilisateurs du réseau. Ce dossier serveur n’est pas partagé.  
  
-   **Musique**.  Utilisé pour stocker et accéder aux fichiers de musique aux utilisateurs du réseau. Ce dossier est créé lorsque vous activez le partage de médias.  
  
-   **Images**.  Utilisé pour stocker et accéder aux images par les utilisateurs du réseau. Ce dossier est créé lorsque vous activez le partage de médias.  
  
-   **TV enregistrée**.  Utilisé pour stocker et accéder à des programmes TV enregistrés par les utilisateurs du réseau. Ce dossier est créé lorsque vous activez le partage de médias.  
  
-   **Vidéos**.  Utilisé pour stocker et accéder à vos vidéos par les utilisateurs du réseau. Ce dossier est créé lorsque vous activez le partage de médias.  
  
-   **Les utilisateurs**.  Utilisé pour stocker les fichiers et y accéder aux utilisateurs du réseau. Un dossier spécifique à l’utilisateur est généré automatiquement dans le **utilisateurs** dossier serveur pour chaque compte d’utilisateur réseau que vous créez.  
  
##### <a name="to-change-access-to-a-shared-folder-for-a-user-account"></a>Pour modifier l’accès à un dossier partagé pour un compte d’utilisateur  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Cliquez sur **stockage**, puis cliquez sur **dossiers du serveur**.  
  
3.  Atteindre et sélectionner le dossier de serveur pour lequel vous souhaitez modifier les autorisations.  
  
4.  Dans le volet des tâches, cliquez sur **permet d’afficher les propriétés du dossier**.  
  
5.  Dans **< FolderName\ > Propriétés**, cliquez sur **partage**et sélectionnez le niveau d’accès utilisateur approprié pour les comptes d’utilisateur de la liste, puis cliquez sur **appliquer**.  
  
    > [!NOTE]
    >  Vous ne pouvez pas modifier les autorisations de partage pour **sauvegardes de l’historique des fichiers**, **la Redirection de dossiers**, et **utilisateurs** dossiers du serveur. Par conséquent, les propriétés du dossier de ces dossiers serveur n’incluent pas un **partage** onglet.  
  
###  <a name="BKMK_Access13"></a>Autoriser les comptes d’utilisateur établir une session Bureau à distance à leur ordinateur  
  Cette section s’applique à un serveur exécutant Windows Server Essentials ou Windows Server Essentials, ou à un serveur exécutant Windows Server 2012 R2 Standard ou Windows Server 2012 R2 Datacenter avec le rôle expérience Windows Server Essentials installé.  
  
 L’administrateur réseau peut accorder des autorisations aux utilisateurs du réseau les autoriser à accéder à leurs ordinateurs réseau à partir d’un emplacement distant.  
  
##### <a name="to-enable-users-to-access-their-network-computers-from-a-remote-location"></a>Pour permettre aux utilisateurs d’accéder à leurs ordinateurs réseau à partir d’un emplacement distant  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **utilisateurs**.  
  
3.  Dans la liste des comptes d’utilisateur, sélectionnez le compte d’utilisateur que vous voulez accorder des autorisations d’accès au Bureau à distance.  
  
4.  Dans le **< utilisateur\ > tâches** volet, cliquez sur **propriétés**.  
  
5.  Dans le **< utilisateur\ > Propriétés**, cliquez sur le **accès à l’ordinateur** onglet.  
  
6.  Sélectionnez les ordinateurs que vous souhaitez être en mesure d’accéder à distance, puis cliquez sur ce compte d’utilisateur **OK**.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer les comptes en ligne des utilisateurs](Manage-Online-Accounts-for-Users.md)  
  
-   [Se connecter](../use/Get-Connected-in-Windows-Server-Essentials.md)  
  
-   [Utiliser Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)  
  
-   [Gérer WindowsServerEssentials](Manage-Windows-Server-Essentials.md)
