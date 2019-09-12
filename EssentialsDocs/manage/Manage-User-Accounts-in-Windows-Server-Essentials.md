---
title: Gérer les comptes d'utilisateur dans Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
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
ms.openlocfilehash: 069cbbdf499ce86586390b1031b6fea71f4f2b2a
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865224"
---
# <a name="manage-user-accounts-in-windows-server-essentials"></a>Gérer les comptes d'utilisateur dans Windows Server Essentials

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

La page Utilisateurs du tableau de bord Windows Server Essentials centralise les informations et les tâches qui vous aident à gérer les comptes d'utilisateur sur votre réseau de petite entreprise. Pour obtenir une vue d’ensemble du tableau de bord utilisateurs, consultez [vue d’ensemble du tableau de bord](Overview-of-the-Dashboard-in-Windows-Server-Essentials.md).  
  
  
##  <a name="BKMK_ManageAccounts"></a>Gestion des comptes d’utilisateur  
 Les rubriques suivantes fournissent des informations sur l'utilisation du tableau de bord Windows Server Essentials pour gérer les comptes d'utilisateur sur le serveur :  
  
-   [Ajouter un compte d’utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1)  
  
-   [Supprimer un compte d’utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Remove)  
  
-   [Afficher les comptes d’utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage3)  
  
-   [Modifier le nom complet du compte d’utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage4)  
  
-   [Activer un compte d’utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage5)  
  
-   [Désactiver un compte d’utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage6)  
  
-   [Comprendre les comptes d’utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage7)  
  
-   [Gérer les comptes d’utilisateur à l’aide du tableau de bord](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8)  
  
###  <a name="BKMK_Manage1"></a>Ajouter un compte d’utilisateur  
 Quand vous ajoutez un compte d'utilisateur, l'utilisateur affecté peut se connecter au réseau. En outre, vous pouvez donner à cet utilisateur l'autorisation d'accéder à des ressources réseau telles que les dossiers partagés et le site d'accès web à distance. Windows Server Essentials comprend l'Assistant Ajouter un compte d'utilisateur qui vous permet d'effectuer les tâches suivantes :  
  
-   fournir un nom et un mot de passe pour le compte d'utilisateur ;  
  
-   définir le compte en tant que compte d'administrateur ou compte d'utilisateur standard ;  
  
-   sélectionner les dossiers partagés accessibles au compte d'utilisateur ;  
  
-   spécifier si le compte d'utilisateur a accès à distance au réseau ;  
  
-   sélectionner les options de messagerie électronique, le cas échéant ;  
  
-   Affectez un compte Microsoft Online Services (appelé compte Office 365 dans Windows Server Essentials), le cas échéant.  
  
-   Affecter des groupes d’utilisateurs (Windows Server Essentials uniquement).  
  
> [!NOTE]
> - Les caractères non-ASCII ne sont pas pris en charge dans les Microsoft Azure Active Directory (Azure AD). N’utilisez pas de caractères non-ASCII dans votre mot de passe, si votre serveur est intégré à Azure AD.  
>   -   Les options de messagerie sont disponibles uniquement si vous installez un complément qui fournit un service de messagerie.  
  
##### <a name="to-add-a-user-account"></a>Pour ajouter un compte d'utilisateur  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **Utilisateurs**.  
  
3.  Dans le volet  **Tâches utilisateur** , cliquez sur **Ajouter un compte d’utilisateur**. L'Assistant Ajout d'un compte d'utilisateur s'affiche.  
  
4.  Suivez les instructions pour exécuter l'Assistant.  
  
###  <a name="BKMK_Remove"></a>Supprimer un compte d’utilisateur  
 Quand vous choisissez de supprimer un compte d'utilisateur du serveur, un Assistant supprime le compte sélectionné. Ainsi, vous ne pouvez plus utiliser le compte pour vous connecter au réseau ou accéder aux ressources réseau. Le cas échéant, vous pouvez également supprimer les fichiers du compte d'utilisateur en même temps que vous supprimez ce dernier. Si vous ne souhaitez pas supprimer de façon permanente le compte d'utilisateur, vous pouvez le désactiver à la place pour interrompre l'accès aux ressources réseau.  
  
> [!IMPORTANT]
>  Si un compte d’utilisateur dispose d’un compte en ligne Microsoft, lorsque vous supprimez le compte d’utilisateur, le compte en ligne est également supprimé de Microsoft Online Services, et les données de l’utilisateur, y compris les e-mails, sont soumises aux stratégies de rétention des données dans Microsoft Online Services. Si vous souhaitez conserver les données utilisateur du compte en ligne, désactivez le compte d'utilisateur au lieu de le supprimer. Pour plus d’informations, consultez [gérer les comptes en ligne pour les utilisateurs](Manage-Online-Accounts-for-Users.md).  
  
##### <a name="to-remove-a-user-account"></a>Pour supprimer un compte d'utilisateur  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **Utilisateurs**.  
  
3.  Dans la liste des comptes d'utilisateur, sélectionnez le compte d'utilisateur à supprimer.  
  
4.  Dans le volet **tâches du\> compte d’utilisateur <** , cliquez sur **supprimer le compte d’utilisateur**. L'Assistant Suppression d'un compte d'utilisateur s'affiche.  
  
5.  Dans la page **voulez-vous conserver les fichiers ?** de l’Assistant, vous pouvez choisir de supprimer les fichiers de l’utilisateur, y compris les sauvegardes de l’historique des fichiers et le dossier redirigé du compte d’utilisateur. Pour conserver les fichiers de l’utilisateur, laissez la case à cocher vide. Après avoir effectué votre sélection, cliquez sur **Suivant**.  
  
6.  Cliquez sur **Supprimer le compte**.  
  
> [!NOTE]
>  Une fois que vous avez supprimé un compte d'utilisateur, il n'apparaît plus dans la liste des comptes d'utilisateur. Si vous avez choisi de supprimer les fichiers, le serveur supprime définitivement le dossier de l’utilisateur dans le dossier du serveur **utilisateurs** et dans le dossier du serveur **sauvegardes de l’historique des fichiers** .  
>   
>  Si vous avez un fournisseur de messagerie intégrée, le compte de messagerie affecté au compte d'utilisateur est également supprimé.  
  
###  <a name="BKMK_Manage3"></a>Afficher les comptes d’utilisateur  
 La section **Utilisateurs** du tableau de bord Windows Server Essentials affiche une liste des comptes d'utilisateur réseau. La liste fournit également des informations supplémentaires sur chaque compte.  
  
##### <a name="to-view-a-list-of-user-accounts"></a>Pour afficher une liste des comptes d'utilisateur  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation principale, cliquez sur **Utilisateurs**.  
  
3.  Le tableau de bord affiche la liste actuelle des comptes d'utilisateur.  
  
##### <a name="to-view-or-change-properties-for-a-user-account"></a>Pour afficher ou changer les propriétés d'un compte d'utilisateur  
  
1.  Dans la liste des comptes d'utilisateur, sélectionnez le compte dont vous voulez afficher ou changer les propriétés.  
  
2.  Dans le volet **tâches du\> compte d’utilisateur <** , cliquez sur **afficher les propriétés du compte**. La page **Propriétés** du compte d'utilisateur s'affiche.  
  
3.  Cliquez sur un onglet pour afficher les propriétés de cette fonctionnalité de compte.  
  
4.  Pour enregistrer les changements que vous apportez aux propriétés du compte d'utilisateur, cliquez sur **Appliquer**.  
  
###  <a name="BKMK_Manage4"></a>Modifier le nom complet du compte d’utilisateur  
 Le nom complet est le nom qui apparaît dans la colonne **Nom** de la page **Utilisateurs** du tableau de bord. Le changement du nom complet ne change pas le nom d'ouverture de session ou le nom de connexion d'un compte d'utilisateur.  
  
##### <a name="to-change-the-display-name-for-a-user-account"></a>Pour changer le nom complet d'un compte d'utilisateur  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **Utilisateurs**.  
  
3.  Dans la liste des comptes d'utilisateur, sélectionnez le compte d'utilisateur à modifier.  
  
4.  Dans le volet **tâches du\> compte d’utilisateur <** , cliquez sur **afficher les propriétés du compte**. La page **Propriétés** du compte d'utilisateur s'affiche.  
  
5.  Sous l'onglet **Général**, tapez un nouveau **Prénom** et **Nom** pour le compte d'utilisateur, puis cliquez sur **OK**.  
  
     Le nouveau nom complet s'affiche dans la liste des comptes d'utilisateur.  
  
###  <a name="BKMK_Manage5"></a>Activer un compte d’utilisateur  
 Quand vous activez un compte d'utilisateur, l'utilisateur affecté peut se connecter au réseau et accéder aux ressources réseau pour lesquelles le compte dispose d'une autorisation d'accès, par exemple les dossiers partagés et le site d'accès web à distance.  
  
> [!NOTE]
>  Vous pouvez uniquement activer un compte d'utilisateur désactivé. Vous ne pouvez pas activer un compte d'utilisateur une fois que vous l'avez supprimé du serveur.  
  
##### <a name="to-activate-a-user-account"></a>Pour activer un compte d'utilisateur  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **Utilisateurs**.  
  
3.  En mode Liste, sélectionnez le compte d'utilisateur à activer.  
  
4.  Dans le volet **tâches du\> compte d’utilisateur <** , cliquez sur **activer le compte d’utilisateur**.  
  
5.  Dans la fenêtre de confirmation, cliquez sur **Oui** pour confirmer votre action.  
  
> [!NOTE]
>  Une fois que vous avez activé un compte d'utilisateur, l'état du compte indique **Actif**. Le compte d'utilisateur récupère les droits d'accès qui ont été affectés avant la désactivation du compte.  
>   
>  Si vous avez un fournisseur de messagerie intégrée, le compte de messagerie affecté au compte d'utilisateur est également activé.  
  
###  <a name="BKMK_Manage6"></a>Désactiver un compte d’utilisateur  
 Quand vous désactivez un compte d'utilisateur, l'accès du compte au serveur est temporairement interrompu. Ainsi, l'utilisateur affecté ne peut pas utiliser le compte pour accéder aux ressources réseau telles que les dossiers partagés ou le site d'accès web à distance tant que vous n'avez pas activé le compte.  
  
 Si le compte d'utilisateur dispose d'un compte en ligne Microsoft, ce dernier est également désactivé. L’utilisateur ne peut pas utiliser les ressources dans Office 365 et d’autres services en ligne auxquelles vous êtes abonné, mais les données de l’utilisateur, y compris les e-mails, sont conservées dans Microsoft Online Services.  
  
> [!NOTE]
>  Vous pouvez uniquement désactiver un compte d'utilisateur actif.  
  
##### <a name="to-deactivate-a-user-account"></a>Pour désactiver un compte d'utilisateur  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **Utilisateurs**.  
  
3.  En mode Liste, sélectionnez le compte d'utilisateur à désactiver.  
  
4.  Dans le volet **tâches du\> compte d’utilisateur <** , cliquez sur **désactiver le compte d’utilisateur**.  
  
5.  Dans la fenêtre de confirmation, cliquez sur **Oui** pour confirmer votre action.  
  
> [!NOTE]
>  Une fois que vous avez désactivé un compte d'utilisateur, l'état du compte indique **Inactif**.  
>   
>  Si vous avez un fournisseur de messagerie intégrée, le compte de messagerie affecté au compte d'utilisateur est également désactivé.  
  
###  <a name="BKMK_Manage7"></a>Comprendre les comptes d’utilisateur  
 Un compte d'utilisateur fournit des informations importantes à Windows Server Essentials. Cela permet aux utilisateurs non seulement d'accéder aux informations stockées sur le serveur, mais également de créer et gérer leurs fichiers et paramètres. Les utilisateurs peuvent se connecter à tous les ordinateurs du réseau s'ils ont un compte d'utilisateur Windows Server Essentials et s'ils disposent d'autorisations d'accès à l'ordinateur choisi. Les utilisateurs accèdent à leurs comptes d'utilisateur avec un nom d'utilisateur et un mot de passe.  
  
 Il existe deux principaux types de compte d'utilisateur. Chaque type fournit aux utilisateurs un niveau distinct de contrôle sur l'ordinateur :  
  
-   Les comptes **standard** sont des comptes pour les opérations quotidiennes. Le compte standard permet de protéger votre réseau en empêchant les utilisateurs d'apporter des changements qui affectent les autres utilisateurs, par exemple supprimer des fichiers ou changer les paramètres réseau.  
  
-   Les comptes**Administrateur** fournissent le contrôle le plus important possible sur un réseau d'ordinateurs. Vous devez affecter le type de compte Administrateur uniquement quand cela est nécessaire.  
  
###  <a name="BKMK_Manage8"></a>Gérer les comptes d’utilisateur à l’aide du tableau de bord  
 Windows Server Essentials vous permet d'effectuer des tâches d'administration courantes à l'aide du tableau de bord Windows Server Essentials. Par défaut, la page **utilisateurs** du tableau de bord comprend deux onglets : **Utilisateurs** et **groupes d’utilisateurs**.  
  
> [!NOTE]
> - Si vous intégrez votre serveur qui exécute Windows Server Essentials à Office 365, un nouvel onglet appelé **groupes de distribution** est également ajouté à la page **utilisateurs** du tableau de bord.  
>   -   Dans Windows Server Essentials, la page **utilisateurs** du tableau de bord comprend uniquement un seul onglet **utilisateurs**.  
  
 L'onglet **Utilisateurs** comprend les éléments suivants :  
  
- Une liste des comptes d'utilisateur, qui indique :  
  
  -   Le nom de l'utilisateur.  
  
  -   Le nom d'ouverture de session du compte d'utilisateur.  
  
  -   Si le compte d'utilisateur dispose de l'autorisation Accès en tout lieu. L'autorisation Accès en tout lieu d'un compte d'utilisateur a la valeur **Autorisé** ou **Non autorisé**.  
  
  -   Si l'historique des fichiers de ce compte d'utilisateur est géré par le serveur exécutant Windows Server Essentials. L'état de l'historique des fichiers d'un compte d'utilisateur est **Géré** ou **Non géré**.  
  
  -   Le niveau d'accès affecté au compte d'utilisateur. Vous pouvez affecter l'accès **Utilisateur standard** ou **Administrateur** à un compte d'utilisateur.  
  
  -   L'état du compte d'utilisateur. Un compte d'utilisateur peut être **Actif**, **Inactif**ou **Incomplet**.  
  
  -   Dans Windows Server Essentials, si le serveur est intégré à Office 365 ou à Windows Intune, le compte en ligne Microsoft s’affiche.  
  
  -   Dans Windows Server Essentials, si le serveur est intégré à Microsoft Office 365, l’état du compte Office 365 (connu sous le nom de compte en ligne Microsoft Windows Server Essentials) pour le compte d’utilisateur est affiché.  
  
- Un volet d'informations avec des informations supplémentaires sur le compte d'utilisateur sélectionné.  
  
- Un volet des tâches qui comprend :  
  
  -   un ensemble de tâches d'administration de compte d'utilisateur, par exemple l'affichage et la suppression de comptes d'utilisateur, ainsi que le changement de mot de passe ;  
  
  -   les tâches qui vous permettent de définir ou changer globalement les paramètres de tous les comptes d'utilisateur du réseau.  
  
  Le tableau suivant décrit les différentes tâches de compte d'utilisateur disponibles sous l'onglet **Utilisateurs** . Certaines tâches sont spécifiques aux comptes d'utilisateur. Elles ne sont visibles que si vous sélectionnez un compte d'utilisateur dans la liste.  
  
> [!NOTE]
>  Si vous intégrez Office 365 à Windows Server Essentials, des tâches supplémentaires sont disponibles. Pour plus d’informations, consultez [gérer les comptes en ligne pour les utilisateurs](Manage-Online-Accounts-for-Users.md).  
  
### <a name="user-account-tasks-in-the-dashboard"></a>Tâches de compte d'utilisateur dans le tableau de bord  
  
|Nom de la tâche|Description|  
|---------------|-----------------|  
|Afficher les propriétés du compte|Vous permet d'afficher et de changer les propriétés du compte d'utilisateur sélectionné, ainsi que de spécifier les autorisations d'accès aux dossiers pour le compte.|  
|Désactiver le compte d’utilisateur|Un compte d'utilisateur désactivé ne peut pas se connecter au réseau, ni accéder aux ressources réseau telles que les dossiers partagés ou les imprimantes.|  
|Activer le compte d’utilisateur|Un compte d'utilisateur activé peut se connecter au réseau et accéder aux ressources réseau, conformément aux autorisations du compte.|  
|Supprimer le compte d’utilisateur|Vous permet de supprimer le compte d'utilisateur sélectionné.|  
|Changer le mot de passe du compte d'utilisateur|Vous permet de réinitialiser le mot de passe réseau du compte d'utilisateur sélectionné.|  
|Ajouter un compte d'utilisateur|Démarre l'Assistant Ajouter un compte d'utilisateur, qui vous permet de créer un compte d'utilisateur unique disposant d'un accès utilisateur standard ou d'un accès administrateur.|  
|Affecter un compte en ligne Microsoft|Ajoute un compte en ligne Microsoft au compte d'utilisateur réseau local sélectionné.<br /><br /> Cette tâche s'affiche quand votre serveur est intégré à Microsoft Online Services, comme Office 365.|  
|Ajouter des comptes en ligne Microsoft|Ajoute des comptes en ligne Microsoft et les associe aux comptes d'utilisateur du réseau local.<br /><br /> Cette tâche s'affiche quand votre serveur est intégré à Microsoft Online Services, comme Office 365.|  
|Définir la stratégie de mot de passe|Vous permet de changer les valeurs des stratégies de mot de passe de votre réseau.|  
|Importer des comptes en ligne Microsoft|Effectue une importation en bloc des comptes de Microsoft Online Services vers le réseau local.<br /><br /> Cette tâche s'affiche quand votre serveur est intégré à Microsoft Online Services, comme Office 365.|  
|Actualiser|Actualise l'onglet Utilisateurs.<br /><br /> Cette tâche s’applique à Windows Server Essentials.|  
|Changer les paramètres de l'historique des fichiers|Vous permet de changer les paramètres de l'historique des fichiers, par exemple la fréquence de la sauvegarde ou sa durée.<br /><br /> Cette tâche s’applique à Windows Server Essentials.|  
|Exporter toutes les connexions à distance|Crée un fichier au format .CSV de toutes les connexions à distance vers le serveur au cours des 30 derniers jours.|  
  
##  <a name="BKMK_ManageAccess"></a>Gestion des mots de passe et de l’accès  
 Les rubriques suivantes fournissent des informations sur l'utilisation du tableau de bord Windows Server Essentials pour gérer les mots de passe des comptes d'utilisateur et l'accès des utilisateurs aux dossiers partagés sur le serveur :  
  
-   [Modifier ou réinitialiser le mot de passe d’un compte d’utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access1)  
  
-   [Ce que vous devez savoir sur les stratégies de mot de passe](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access3)  
  
-   [Modifier la stratégie de mot de passe](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access4)  
  
-   [Niveau d’accès aux dossiers partagés](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access5)  
  
-   [Conserver et gérer l’accès aux fichiers pour les comptes d’utilisateur supprimés](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access6)  
  
-   [Synchroniser le mot de passe DSRM avec le mot de passe d’administrateur réseau](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access7)  
  
-   [Accorder à des comptes d’utilisateur l’autorisation Bureau à distance](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access8)  
  
-   [Permettre aux utilisateurs d’accéder aux ressources sur le serveur](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access9)  
  
-   [Modifier les autorisations d’accès à distance pour un compte d’utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access10)  
  
-   [Modifier les autorisations de réseau privé virtuel pour un compte d’utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access11)  
  
-   [Modifier l’accès aux dossiers partagés internes pour un compte d’utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access12)  
  
-   [Autoriser les comptes d’utilisateur à établir une session Bureau à distance sur leur ordinateur](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access13)  
  
###  <a name="BKMK_Access1"></a>Modifier ou réinitialiser le mot de passe d’un compte d’utilisateur  
 Pour changer ou réinitialiser un mot de passe de compte d'utilisateur, procédez comme suit.  
  
##### <a name="to-reset-the-password-for-a-user-account"></a>Pour réinitialiser le mot de passe d'un compte d'utilisateur  
  
1. Ouvrez le tableau de bord Windows Server Essentials.  
  
2. Dans la barre de navigation, cliquez sur **Utilisateurs**.  
  
3. Dans la liste des comptes d'utilisateur, sélectionnez le compte d'utilisateur à réinitialiser.  
  
4. Dans le volet **tâches du\> compte d’utilisateur <** , cliquez sur **modifier le mot de passe du compte d’utilisateur**. L'Assistant permettant de changer le mot de passe du compte d'utilisateur s'affiche.  
  
5. Tapez le nouveau mot de passe du compte d'utilisateur, puis retapez le mot de passe pour le confirmer.  
  
6. Cliquez sur **Modifier le mot de passe**.  
  
7. Indiquez le mot de passe à l'utilisateur.  
  
   > [!IMPORTANT]
   > - Vous risquez de ne pas pouvoir changer votre mot de passe si la stratégie de mot de passe du compte a la valeur **Les mots de passe n'arrivent jamais à expiration**.  
   >   -   Les caractères non-ASCII ne sont pas pris en charge dans Azure AD. Par conséquent, si votre serveur est intégré à Azure AD, n’utilisez pas de caractères non-ASCII dans votre mot de passe.  
   >   -   Si un compte en ligne Microsoft (connu sous le nom de compte Office 365 dans Windows Server Essentials) est affecté à l’utilisateur, le mot de passe est synchronisé avec le mot de passe du compte en ligne. L'utilisateur emploie le nouveau mot de passe pour se connecter au serveur ou à Office 365. Pour plus d’informations, consultez [gérer les comptes en ligne pour les utilisateurs](Manage-Online-Accounts-for-Users.md).  
  
###  <a name="BKMK_Access3"></a>Ce que vous devez savoir sur les stratégies de mot de passe  
 La stratégie de mot de passe est un ensemble de règles qui définissent la façon dont les utilisateurs créent et utilisent des mots de passe. La stratégie contribue à empêcher tout accès non autorisé aux données utilisateur et autres informations stockées sur le serveur. La stratégie de mot de passe est appliquée à tous les comptes d'utilisateur qui accèdent au réseau.  
  
 La stratégie de mot de passe Windows Server Essentials se compose de trois éléments principaux, comme suit :  
  
- **Longueur du mot de passe**.  Plus un mot de passe est long, plus il est sécurisé. Les mots de passe vides ne sont pas sécurisés.  
  
- **Complexité du mot de passe**.  Les mots de passe complexes contiennent une combinaison de caractères en majuscules et minuscules (a-z, A-Z), de chiffres (0-9) et de symboles non alphabétiques (à savoir ; !,@,#,_,-). Les mots de passe complexes sont beaucoup moins vulnérables aux accès non autorisés. Les mots de passe qui contiennent des noms d'utilisateurs, des dates de naissance ou d'autres informations personnelles n'offrent pas une sécurité appropriée.  
  
- **Âge du mot de passe**.  Windows Server Essentials demande aux utilisateurs de changer leur mot de passe au moins une fois tous les 180 jours. Éventuellement, vous pouvez choisir d'avoir des mots de passe qui n'expirent jamais.  
  
  Pour faciliter l'implémentation d'une stratégie de mot de passe dans votre réseau informatique, Windows Server Essentials fournit un outil simple qui vous permet de définir ou de changer la stratégie de mot de passe en fonction de l'un des quatre profils de stratégie prédéfinis suivants :  
  
- **Faible**.  Les utilisateurs peuvent spécifier n'importe quel mot de passe non vide.  
  
- **Moyen**.  Ces mots de passe doivent contenir au moins 5 caractères. Il n'est pas obligatoire d'utiliser un mot de passe complexe.  
  
- **Moyen Fort**.  Ces mots de passe doivent contenir au moins 5 caractères et doivent inclure des lettres, des chiffres et des symboles.  
  
- **Fort**.  Ces mots de passe doivent contenir au moins 7 caractères et doivent inclure des lettres, des chiffres et des symboles. Ces mots de passe sont plus sécurisés, mais ils peuvent être plus difficiles à mémoriser.  
  
  > [!NOTE]
  >  Les mots de passe ne peuvent pas contenir le nom d'utilisateur ou l'adresse de messagerie.  
  > 
  >  Si vous effectuez une intégration à Office 365, elle applique la stratégie de mot de passe **Fort** et la met à jour pour inclure les exigences suivantes :  
  > 
  > - Les mots de passe doivent contenir 8 16 caractères.  
  >   -   Les mots de passe ne doivent pas contenir d'espaces, ni le nom du compte de messagerie Office 365.  
  
  Par défaut, l'installation du serveur définit la stratégie de mot de passe en utilisant l'option **Fort**.  
  
###  <a name="BKMK_Access4"></a>Modifier la stratégie de mot de passe  
 Procédez comme suit pour définir ou changer la stratégie de mot de passe en fonction de l'un des quatre profils de stratégie prédéfinis.  
  
##### <a name="to-change-the-password-policy"></a>Pour changer la stratégie de mot de passe  
  
1.  Ouvrez le tableau de bord Windows Server Essentials, puis cliquez sur **Utilisateurs**.  
  
2.  Dans le volet **Tâches** , cliquez sur **Définir la stratégie de mot de passe**.  
  
3.  Dans l'écran **Modifier la stratégie de mot de passe**, définissez le niveau de sécurité du mot de passe en déplaçant le curseur.  
  
     Microsoft recommande de définir le niveau de sécurité du mot de passe à **Fort**.  
  
    > [!NOTE]
    >  Éventuellement, vous pouvez également sélectionner **Les mots de passe n'arrivent jamais à expiration**. Ce paramètre est moins sécurisé et n'est donc pas recommandé.  
  
4.  Cliquez sur **Modifier la stratégie**.  
  
###  <a name="BKMK_Access5"></a>Niveau d’accès aux dossiers partagés  
 Il est recommandé d'affecter aux utilisateurs les autorisations les plus restrictives tout en leur permettant d'effectuer les tâches dont ils ont besoin.  
  
 Vous disposez de trois paramètres d'accès pour les dossiers partagés sur le serveur :  
  
-   **Lecture/écriture**.  Choisissez ce paramètre si vous voulez autoriser le compte d'utilisateur à créer, changer et supprimer des fichiers dans le dossier partagé.  
  
-   **Lecture seule**.  Choisissez ce paramètre si vous voulez uniquement autoriser le compte d'utilisateur à lire des fichiers dans le dossier partagé. Les comptes d'utilisateur avec accès en lecture seule ne peuvent pas créer, changer ou supprimer de fichiers dans le dossier partagé.  
  
-   **Aucun accès**.  Choisissez ce paramètre si vous ne voulez pas que le compte d'utilisateur ait accès aux fichiers dans le dossier partagé.  
  
###  <a name="BKMK_Access6"></a>Conserver et gérer l’accès aux fichiers pour les comptes d’utilisateur supprimés  
 L’administrateur réseau peut supprimer un compte d’utilisateur et choisir de conserver les fichiers de l’utilisateur pour une utilisation ultérieure. Dans ce scénario, le compte d'utilisateur supprimé ne peut plus être utilisé pour la connexion au réseau. Toutefois, les fichiers de l'utilisateur concerné sont enregistrés dans un dossier partagé, qui peut être accessible à un autre utilisateur.  
  
> [!IMPORTANT]
>  Sachez que si vous supprimez un compte d'utilisateur qui dispose d'un compte en ligne Microsoft affecté, le compte en ligne est également supprimé. Par ailleurs, les données utilisateur (y compris le courrier électronique) sont soumises aux stratégies de rétention des données de Microsoft Online Services. Pour conserver les données utilisateur du compte en ligne, désactivez le compte d'utilisateur au lieu de le supprimer. Pour plus d’informations, consultez [gérer les comptes en ligne pour les utilisateurs](Manage-Online-Accounts-for-Users.md).  
  
##### <a name="to-remove-a-user-account-but-retain-access-to-the-users-files"></a>Pour supprimer un compte d’utilisateur tout en conservant l’accès aux fichiers de l’utilisateur  
  
1. Ouvrez le tableau de bord Windows Server Essentials.  
  
2. Dans la barre de navigation, cliquez sur **Utilisateurs**.  
  
3. Dans la liste des comptes d'utilisateur, sélectionnez le compte d'utilisateur à supprimer.  
  
4. Dans le volet **tâches du\> compte d’utilisateur <** , cliquez sur **supprimer le compte d’utilisateur**. L'Assistant Suppression d'un compte d'utilisateur s'affiche.  
  
5. Dans la page **Voulez-vous conserver les fichiers ?** , assurez-vous que la case **Supprimer les fichiers inclus dans les sauvegardes Historique des fichiers et le dossier redirigé pour ce compte d'utilisateur** n'est pas cochée, puis cliquez sur **Suivant**.  
  
    Une page de confirmation s'affiche pour vous avertir que vous supprimez le compte mais que les fichiers sont conservés.  
  
6. Cliquez sur **Supprimer le compte** pour supprimer le compte d'utilisateur.  
  
   Une fois le compte d'utilisateur supprimé, l'administrateur peut donner à un autre compte d'utilisateur l'accès au dossier partagé.  
  
##### <a name="to-give-a-user-account-permission-to-access-a-shared-folder"></a>Pour donner à un compte d'utilisateur l'autorisation d'accéder à un dossier partagé  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **Stockage**, puis sur l'onglet **Dossiers du serveur** .  
  
3.  Dans la liste des dossiers, sélectionnez le dossier **Utilisateurs**.  
  
4.  Dans le volet **Tâches** , cliquez sur **Ouvrir le dossier**. L'Explorateur Windows s'ouvre et affiche le contenu du dossier **Utilisateurs**.  
  
5.  Cliquez avec le bouton droit sur le compte d'utilisateur à partager, puis cliquez sur **Propriétés**.  
  
6.  Dans **< propriétés du\> compte d’utilisateur**, cliquez sur l’onglet **partage** , puis sur **partager**.  
  
7.  Dans la fenêtre **Partage de fichiers**, tapez ou sélectionnez le nom du compte d'utilisateur avec lequel vous souhaitez partager le dossier, puis cliquez sur **Ajouter**.  
  
8.  Choisissez le **Niveau d'autorisation** du compte d'utilisateur, puis cliquez sur **Partager**.  
  
###  <a name="BKMK_Access7"></a>Synchroniser le mot de passe DSRM avec le mot de passe d’administrateur réseau  
 Le mode de restauration des services d'annuaire (DSRM) est un mode de démarrage spécial qui permet de réparer ou de récupérer Active Directory. Le système d'exploitation utilise le mode DSRM pour se connecter à l'ordinateur si Active Directory est défaillant ou doit être restauré. Si votre mot de passe d'administrateur réseau et le mot de passe DSRM sont différents, le mode DSRM ne se charge pas.  
  
 Durant une nouvelle installation (première installation) de Windows Server Essentials, le programme définit le mot de passe DSRM en fonction du mot de passe de compte administrateur réseau spécifié pendant l'installation ou dans le fichier de réponses de migration. Quand vous changez votre mot de passe d'administrateur réseau, (de préférence, tous les 60 jours pour une sécurité accrue du serveur), le changement de mot de passe n'est pas transmis au mode DSRM. Cela entraîne une incompatibilité des mots de passe. Si cela se produit, vous pouvez utiliser les solutions suivantes pour synchroniser manuellement ou automatiquement le mot de passe de votre administrateur réseau avec le mot de passe DSRM.  
  
##### <a name="to-manually-synchronize-the-dsrm-password-to-a-network-administrator-account"></a>Pour synchroniser manuellement le mot de passe DSRM au mot de passe d'un compte d'administrateur réseau  
  
1. À l’invite de commandes, exécutez `ntdsutil.exe` pour ouvrir l’outil ntdsutil.  
  
2. Pour réinitialiser le mot de passe DSRM, tapez **set dsrm password**.  
  
3. Pour synchroniser le mot de passe DSRM sur un contrôleur de domaine avec le compte de l’administrateur réseau actuel, tapez :  
  
    **synchroniser à partir du compte de domaine** *< > current_network_administrator_account*, puis appuyez sur entrée.  
  
   Comme vous allez changer régulièrement le mot de passe du compte d'administrateur réseau, nous vous recommandons de créer une tâche planifiée pour synchroniser automatiquement et quotidiennement le mot de passe DSRM au mot de passe d'administrateur réseau. Cela permet de garantir que le mot de passe DSRM est toujours identique au mot de passe actif de l'administrateur réseau.  
  
##### <a name="to-automatically-synchronize-the-dsrm-password-to-a-network-administrator-account"></a>Pour synchroniser automatiquement le mot de passe DSRM au mot de passe d'un compte d'administrateur réseau  
  
1.  Sur le serveur, ouvrez **Outils d'administration**, puis double-cliquez sur **Planificateur de tâches**.  
  
2.  Dans le volet **Actions** du Planificateur de tâches, cliquez sur **Créer une tâche**.  
  
3.  Dans la zone de texte **Nom** , tapez un nom pour la tâche, par exemple **Synchronisation automatique de mot de passe DSRM**, puis sélectionnez l'option **Exécuter avec les autorisations maximales** .  
  
4.  Définissez le moment où la tâche doit s'exécuter :  
  
    1.  Dans la boîte de dialogue **Créer une tâche**, cliquez sur l'onglet **Déclencheurs**, puis sur **Nouveau**.  
  
    2.  Dans la boîte de dialogue **Nouveau déclencheur** , sélectionnez votre option de périodicité, spécifiez l'intervalle de périodicité, puis choisissez une heure de début.  
  
        > [!NOTE]
        >  Il est recommandé de définir la tâche pour qu'elle s'exécute tous les jours en dehors des horaires de travail.  
  
    3.  Cliquez sur **OK** pour enregistrer vos changements et revenir à la boîte de dialogue **Créer une tâche** .  
  
5.  Définissez les actions de la tâche :  
  
    1.  Cliquez sur l'onglet **Actions**, puis sur **Nouveau**. La boîte de dialogue **Nouvelle action** s'affiche.  
  
    2.  Dans la liste **Action**, cliquez sur **Démarrer un programme**, puis accédez à **C:\WINDOWS\SYSTEM32\ntdsutil.exe**.  
  
    3.  Dans la zone de texte **Ajouter des arguments**(facultatif), tapez ce qui suit (vous devez inclure les guillemets) : **définir la synchronisation du mot de passe DSRM à partir du compte de domaine SBS_network_administrator_account q q** , où *SBS_network_administrator_account* est nom du compte de l’administrateur réseau actuel.  
  
6.  Cliquez sur **OK** à deux reprises pour enregistrer la tâche et fermer la boîte de dialogue **Créer une tâche**. La nouvelle tâche apparaît dans la section **Tâches actives** du **Planificateur de tâches**.  
  
###  <a name="BKMK_Access8"></a>Accorder à des comptes d’utilisateur l’autorisation Bureau à distance  
 Dans l'installation par défaut de Windows Server Essentials, les utilisateurs réseau ne sont pas autorisés à établir de connexion à distance aux ordinateurs ou autres ressources réseau.  
  
 Pour permettre aux utilisateurs réseau d'établir une connexion à distance aux ressources réseau, vous devez d'abord configurer l'Accès en tout lieu. Une fois l'Accès en tout lieu configuré, les utilisateurs peuvent accéder aux fichiers, applications et ordinateurs de votre réseau d'entreprise à partir d'un appareil (quel que soit son emplacement) disposant d'une connexion Internet.  
  
 L'Assistant Configuration de l'Accès en tout lieu vous permet d'activer deux méthodes d'accès à distance :  
  
- réseau privé virtuel (VPN) ;  
  
- Accès web à distance  
  
  Quand vous exécutez l'Assistant, vous pouvez également choisir d'autoriser l'Accès en tout lieu pour tous les comptes d'utilisateur, qu'ils soient actifs ou récemment ajoutés.  
  
  Pour configurer l'Accès en tout lieu, ouvrez la page **Accueil** du tableau de bord, cliquez sur **CONFIGURATION**, puis sur **Configurer l'Accès en tout lieu**.  
  
  Pour plus d’informations sur l’accès en tout lieu, consultez [gérer l’accès en tout lieu](Manage-Anywhere-Access-in-Windows-Server-Essentials.md).  
  
###  <a name="BKMK_Access9"></a>Permettre aux utilisateurs d’accéder aux ressources sur le serveur  
  Cette section s’applique à un serveur exécutant Windows Server Essentials ou Windows Server Essentials, ou à un serveur exécutant Windows Server 2012 R2 Standard ou Windows Server 2012 R2 Datacenter avec le rôle expérience Windows Server Essentials installé.  
  
 Si vous souhaitez que les utilisateurs se servent de l'accès à distance et/ou disposent de comptes d'utilisateur individuels, une fois que vous avez fini de connecter un ordinateur au serveur, créez sur le serveur à l'aide du tableau de bord des comptes d'utilisateur réseau pour les utilisateurs de l'ordinateur en réseau. Pour plus d'informations sur la création d'un compte d'utilisateur, voir [Ajouter un compte d'utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1). Après avoir créé les comptes d'utilisateur, vous devez fournir les informations relatives aux mots de passe et aux noms d'utilisateurs réseau des utilisateurs de l'ordinateur client pour qu'ils puissent accéder aux ressources du serveur via Launchpad.  
  
 Pour chaque compte d'utilisateur créé, vous pouvez définir l'accès aux éléments suivants à l'aide des propriétés de compte d'utilisateur :  
  
-   **Dossiers partagés**.  Par défaut, les comptes administrateur réseau ont une autorisation d'accès en **Lecture/écriture** à tous les dossiers partagés, alors que les comptes d'utilisateur standard ont une autorisation d'accès en **Lecture seule** au dossier Société. Si la diffusion de contenu multimédia est activée, vous pouvez affecter aux comptes d'utilisateur standard des autorisations d'accès aux dossiers partagés suivants : **Musique**, **Images**, **TV enregistrée** et **Vidéos**. Vous pouvez définir les autorisations qui permettent aux comptes d'utilisateur d'accéder aux dossiers partagés sous l'onglet **Dossiers partagés** des propriétés de compte d'utilisateur.  
  
-   **Accès en tout lieu**.  Par défaut, les administrateurs réseau peuvent utiliser le réseau VPN ou l'accès web à distance pour accéder aux ressources du serveur. Pour les comptes d'utilisateur standard, vous devez définir les autorisations du compte d'utilisateur sous l'onglet **Accès en tout lieu**.  
  
-   **Accès à l'ordinateur**.  Par défaut, les administrateurs réseau peuvent accéder à tous les ordinateurs du réseau. Toutefois, pour les comptes d'utilisateur standard, vous pouvez définir des autorisations individuelles d'accès aux ordinateurs du réseau sous l'onglet **Accès à l'ordinateur** des propriétés de compte d'utilisateur.  
  
##### <a name="to-edit-user-account-properties-in-windows-server-essentials-2012-r2"></a>Pour changer les propriétés de compte d'utilisateur dans Windows Server Essentials 2012 R2  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **UTILISATEURS**.  
  
3.  Dans la liste des comptes d'utilisateur, sélectionnez le compte d'utilisateur que vous souhaitez modifier.  
  
4.  Dans le volet **tâches du\> compte d’utilisateur <** , cliquez sur **afficher les propriétés du compte**.  
  
5.  Dans les **Propriétés du compte\> d’utilisateur <** , procédez comme suit :  
  
    1.  Sous l'onglet **Dossiers partagés** , définissez les autorisations de dossier appropriées pour chaque dossier partagé en fonction des besoins.  
  
    2.  Sous l'onglet **Accès en tout lieu** :  
  
        1.  Pour autoriser un utilisateur à se connecter au serveur à l'aide d'un réseau VPN, cochez la case **Autoriser le Réseau privé virtuel (VPN)** .  
  
        2.  Pour autoriser un utilisateur à se connecter au serveur à l'aide de l'accès web à distance, cochez la case **Autoriser l'accès web à distance et l'accès aux applications de services Web**.  
  
    3.  Sous l'onglet **Accès à l'ordinateur** , sélectionnez les ordinateurs réseau auxquels vous voulez que l'utilisateur puisse accéder.  
  
##### <a name="to-edit-user-account-properties-in-windows-server-essentials-2012"></a>Pour modifier les propriétés de compte d'utilisateur dans Windows Server Essentials 2012  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **UTILISATEURS**.  
  
3.  Dans la liste des comptes d'utilisateur, sélectionnez le compte d'utilisateur que vous souhaitez modifier.  
  
4.  Dans le volet **tâches du\> compte d’utilisateur <** , cliquez sur **Propriétés**.  
  
5.  Dans les **Propriétés du compte\> d’utilisateur <** , procédez comme suit :  
  
    1.  Sous l'onglet **Général**, sélectionnez **L'utilisateur peut afficher les alertes de santé du réseau**, si le compte d'utilisateur a besoin d'accéder aux rapports d'intégrité du réseau.  
  
    2.  Sous l'onglet **Dossiers partagés** , définissez les autorisations de dossier appropriées pour chaque dossier partagé en fonction des besoins.  
  
    3.  Sous l'onglet **Accès en tout lieu** :  
  
        1.  Pour autoriser un utilisateur à se connecter au serveur à l'aide d'un réseau VPN, cochez la case **Autoriser le Réseau privé virtuel (VPN)** .  
  
        2.  Pour autoriser un utilisateur à se connecter au serveur à l'aide de l'accès web à distance, cochez la case **Autoriser l'accès web à distance et l'accès aux applications de services Web**.  
  
    4.  Sous l'onglet **Accès à l'ordinateur** , sélectionnez les ordinateurs réseau auxquels vous voulez que l'utilisateur puisse accéder.  
  
###  <a name="BKMK_Access10"></a>Modifier les autorisations d’accès à distance pour un compte d’utilisateur  
 Un utilisateur peut accéder aux ressources situées sur le serveur à partir d'un emplacement distant à l'aide d'un réseau privé virtuel (VPN), de l'accès web à distance ou d'autres applications de services web. Par défaut, les autorisations d'accès à distance sont activées pour les utilisateurs réseau quand vous configurez l'Accès en tout lieu dans Windows Server Essentials à l'aide du tableau de bord.  
  
##### <a name="to-change-remote-access-permissions-for-a-user-account"></a>Pour changer les autorisations d'accès à distance pour un compte d'utilisateur  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **Utilisateurs**.  
  
3.  Dans la liste des comptes d'utilisateur, sélectionnez le compte d'utilisateur à modifier.  
  
4.  Dans le volet **tâches du\> compte d’utilisateur <** , cliquez sur **afficher les propriétés du compte**. La page **Propriétés** du compte d'utilisateur s'affiche.  
  
5.  Sous l'onglet **Accès en tout lieu** , procédez comme suit :  
  
    -   Cochez la case **Autoriser le réseau privé virtuel (VPN)** pour autoriser un utilisateur à se connecter au serveur à l'aide d'un réseau VPN.  
  
    -   Cochez la case **Autoriser l'accès web à distance et l'accès aux applications de services web** pour autoriser un utilisateur à se connecter au serveur à l'aide de l'accès web à distance.  
  
6.  Cliquez sur **Appliquer**, puis sur **OK**.  
  
###  <a name="BKMK_Access11"></a>Modifier les autorisations de réseau privé virtuel pour un compte d’utilisateur  
 Vous pouvez utiliser un réseau privé virtuel (VPN) pour vous connecter à Windows Server Essentials et accéder à toutes les ressources stockées sur le serveur. Cela est particulièrement utile si vous disposez d'un ordinateur client configuré avec des comptes réseau pouvant être utilisés pour se connecter à un serveur Windows Server Essentials hébergé via une connexion VPN. Tous les comptes d'utilisateur récemment créés sur le serveur Windows Server Essentials hébergé doivent utiliser le réseau VPN pour se connecter à l'ordinateur client pour la première fois.  
  
##### <a name="to-change-vpn-permissions-for-network-users"></a>Pour changer les autorisations VPN des utilisateurs réseau  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **UTILISATEURS**.  
  
3.  Dans la liste des comptes d'utilisateur, sélectionnez celui auquel vous voulez accorder des autorisations d'accès au Bureau à distance.  
  
4.  Dans le volet **tâches du\> compte d’utilisateur <** , cliquez sur **Propriétés**.  
  
5.  Dans la **< propriétés du\> compte d’utilisateur**, cliquez sur l’onglet **accès en tout lieu** .  
  
6.  Sous l'onglet **Accès en tout lieu**, pour autoriser un utilisateur à se connecter au serveur à l'aide d'un réseau VPN, cochez la case **Autoriser le Réseau privé virtuel (VPN)** .  
  
7.  Cliquez sur **Appliquer**, puis sur **OK**.  
  
###  <a name="BKMK_Access12"></a>Modifier l’accès aux dossiers partagés internes pour un compte d’utilisateur  
 Vous pouvez gérer l'accès aux dossiers partagés sur le serveur à l'aide des tâches situées sous l'onglet **Dossiers du serveur** du tableau de bord. Par défaut, les dossiers serveur suivants sont créés quand vous installez Windows Server Essentials :  
  
-   **Sauvegardes d'ordinateurs client**.  Permet de stocker les sauvegardes d'ordinateurs clients créées par la fonctionnalité de sauvegarde de Windows Server. Ce dossier serveur n'est pas partagé.  
  
-   **Société**.  Permet aux utilisateurs réseau de stocker des documents liés à votre organisation et d'y accéder.  
  
-   **Sauvegardes de l'Historique des fichiers**.  Par défaut, Windows Server Essentials stocke les sauvegardes de fichiers créées à l'aide de l'historique des fichiers. Ce dossier serveur n'est pas partagé.  
  
-   **Redirection de dossiers**.  Permet aux utilisateurs réseau de stocker des dossiers configurés pour la redirection de dossiers et d'y accéder. Ce dossier serveur n'est pas partagé.  
  
-   **Musique**.  Permet aux utilisateurs réseau de stocker des fichiers de musique et d'y accéder. Ce dossier est créé quand vous activez le partage de fichiers multimédias.  
  
-   **Images**.  Permet aux utilisateurs réseau de stocker des images et d'y accéder. Ce dossier est créé quand vous activez le partage de fichiers multimédias.  
  
-   **TV enregistrée**.  Permet aux utilisateurs réseau de stocker des programmes TV enregistrés et d'y accéder. Ce dossier est créé quand vous activez le partage de fichiers multimédias.  
  
-   **Vidéos**.  Permet aux utilisateurs réseau de stocker des vidéos et d'y accéder. Ce dossier est créé quand vous activez le partage de fichiers multimédias.  
  
-   **Utilisateurs**.  Permet aux utilisateurs réseau de stocker des fichiers et d'y accéder. Un dossier propre à l'utilisateur est généré automatiquement dans le dossier serveur **Utilisateurs** pour chaque compte d'utilisateur réseau que vous créez.  
  
##### <a name="to-change-access-to-a-shared-folder-for-a-user-account"></a>Pour changer l'accès à un dossier partagé pour un compte d'utilisateur  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Cliquez sur **STOCKAGE**, puis sur **Dossiers du serveur**.  
  
3.  Recherchez et sélectionnez le dossier de serveur dont vous souhaitez modifier les autorisations.  
  
4.  Dans le volet des tâches, cliquez sur **Afficher les propriétés du dossier**.  
  
5.  Dans **< propriétés\> NomDossier**, cliquez sur **partage**et sélectionnez le niveau d’accès utilisateur approprié pour les comptes d’utilisateurs listés, puis cliquez sur **appliquer**.  
  
    > [!NOTE]
    >  Vous ne pouvez pas changer les autorisations de partage des dossiers serveur **Sauvegardes de l'Historique des fichiers**, **Redirection de dossiers**et **Utilisateurs** . Il n'y a donc pas d'onglet **Partage** dans les propriétés de dossier de ces dossiers serveur.  
  
###  <a name="BKMK_Access13"></a>Autoriser les comptes d’utilisateur à établir une session Bureau à distance sur leur ordinateur  
  Cette section s’applique à un serveur exécutant Windows Server Essentials ou Windows Server Essentials, ou à un serveur exécutant Windows Server 2012 R2 Standard ou Windows Server 2012 R2 Datacenter avec le rôle expérience Windows Server Essentials installé.  
  
 L'administrateur réseau peut accorder des autorisations aux utilisateurs réseau pour accéder à leurs ordinateurs réseau à partir d'un emplacement distant.  
  
##### <a name="to-enable-users-to-access-their-network-computers-from-a-remote-location"></a>Pour permettre aux utilisateurs d'accéder à leurs ordinateurs réseau à partir d'un emplacement distant  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **UTILISATEURS**.  
  
3.  Dans la liste des comptes d'utilisateur, sélectionnez celui auquel vous voulez accorder des autorisations d'accès au Bureau à distance.  
  
4.  Dans le volet **tâches du\> compte d’utilisateur <** , cliquez sur **Propriétés**.  
  
5.  Dans la **< propriétés du\> compte d’utilisateur**, cliquez sur l’onglet **accès à l’ordinateur** .  
  
6.  Sélectionnez les ordinateurs auxquels ce compte d'utilisateur peut accéder à distance, puis cliquez sur **OK**.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer les comptes en ligne des utilisateurs](Manage-Online-Accounts-for-Users.md)  
  
-   [Connectez-vous](../use/Get-Connected-in-Windows-Server-Essentials.md)  
  
-   [Utiliser Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)  
  
-   [Gérer Windows Server Essentials](Manage-Windows-Server-Essentials.md)
