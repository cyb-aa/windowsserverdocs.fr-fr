---
title: Gérer les comptes en ligne des utilisateurs Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: c09f4cf6-4d12-49fe-9ae4-e6cb14027b9d
author: nnamuhcs
ms.author: daveba
ms.openlocfilehash: 302b5e79f4495c792fd0b30392988720f5594c58
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311246"
---
# <a name="manage-online-accounts-for-windows-server-essentials-users"></a>Gérer les comptes en ligne des utilisateurs Windows Server Essentials

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Lorsque vous intégrez votre serveur Windows Server Essentials à Microsoft Office 365, vous pouvez gérer vos comptes en ligne ainsi que les comptes d’utilisateur à partir du tableau de bord. Dans cette rubrique, vous allez découvrir ce que vous pouvez obtenir en gérant vos utilisateurs comptes Microsoft Online Services à partir du tableau de bord, comment créer et gérer des comptes en ligne à partir du tableau de bord, et comment gérer des adresses de messagerie et des groupes de distribution pour Exchange Online à partir du tableau de bord.  

  
> [!NOTE]
>  Pour gérer vos comptes Microsoft Online Services dans Windows Server Essentials, vous devez intégrer votre serveur à Office 365. Pour obtenir des instructions, consultez [gérer Office 365](Manage-Office-365-in-Windows-Server-Essentials.md).  
  
> [!IMPORTANT]
>  Si vous gérez des comptes en ligne dans Windows Server Essentials, vous avez l’habitude de voir les comptes Microsoft Online Services appelés *comptes Office 365*. Dans le tableau de bord de Windows Server Essentials, les étiquettes ont été remplacées par les *comptes Microsoft Online Services*, ou par les *comptes en ligne Microsoft* pour être courts. Les comptes et les procédures sont identiques ; seules les étiquettes ont changé. La plupart des procédures de cette rubrique utilisent le terme *compte en ligne*.  
  
## <a name="in-this-topic"></a>Dans cette rubrique  
  
-   [Pourquoi dois-je gérer mes comptes en ligne à partir du tableau de bord ?](#BKMK_WhyManageOnlineAccounts)  
  
-   [Créer des comptes en ligne](#BKMK_SECTION_CreateOnlineAccounts)  
  
-   [Gérer les comptes en ligne](#BKMK_SECTION_ManageOnlineAccounts)  
  
-   [Gérer les adresses de messagerie pour Exchange Online](#BKMK_SECTION_ManageEmailAddresses)  
  
-   [Gérer des groupes de distribution pour Exchange Online](#BKMK_SECTION_ManageDistributionGroups)  
  
##  <a name="why-should-i-manage-my-online-accounts-from-the-dashboard"></a><a name="BKMK_WhyManageOnlineAccounts"></a>Pourquoi dois-je gérer mes comptes en ligne à partir du tableau de bord ?  
 Lorsque vous utilisez le tableau de bord pour affecter un compte Microsoft Online Services à un compte d’utilisateur, les mots de passe des comptes sont automatiquement synchronisés, et vous pouvez gérer les deux comptes ensemble tout au long du cycle de vie des comptes d’utilisateur.  
  
 C’est pratique pour l’utilisateur, qui peut utiliser le même mot de passe pour accéder aux ressources sur le serveur et dans Office 365. Vous pouvez également appliquer les mêmes exigences de mot de passe pour accéder aux ressources dans Office 365 dont vous avez besoin pour vos ressources internes.  
  
### <a name="how-does-password-synchronization-work"></a>Fonctionnement de la synchronisation des mots de passe  
 Lorsque vous utilisez le tableau de bord pour affecter un compte Microsoft Online Services à un compte d’utilisateur, le mot de passe du compte d’utilisateur est automatiquement synchronisé avec le compte utilisateur en ligne. Cela signifie qu’un utilisateur n’a besoin que d’un seul mot de passe pour accéder aux ressources sur le serveur et dans Office 365. En outre, vous pouvez utiliser le même nom pour le compte d’utilisateur et l’ID en ligne de l’utilisateur.  
  
 La synchronisation de mot de passe se produit immédiatement et automatiquement quand un utilisateur modifie le mot de passe pour son compte d'utilisateur à partir d'un ordinateur appartenant à un domaine ou à l'aide de l'accès web à distance.  
  
> [!IMPORTANT]
>  Si Office 365 est intégré à Windows Server Essentials, les utilisateurs ne doivent pas modifier le mot de passe de leur compte en ligne Microsoft à partir du portail Office 365. Cette action bloque la synchronisation des mots de passe.  
  
### <a name="simplified-account-creation"></a>Création de compte simplifiée  
 Il y a un autre avantage lorsque vous créez vos comptes en ligne initiaux à partir du tableau de bord. Vous pouvez créer des comptes en ligne pour tous les utilisateurs en une seule opération. En revanche, si vos employés utilisent déjà Office 365 et que vous configurez un nouveau serveur Windows Server Essentials, vous pouvez créer tous vos comptes d’utilisateur à partir des comptes en ligne en une seule action. Pour plus d'informations, voir [Créer des comptes en ligne](#BKMK_SECTION_CreateOnlineAccounts).  
  
### <a name="manage-email-addresses-and-distribution-groups-from-the-dashboard"></a>Gérer des adresses de messagerie et des groupes de distribution à partir du tableau de bord  
 Vous pouvez gérer vos adresses de messagerie et groupes de distribution pour Exchange Online à partir du tableau de bord. Vous pouvez utiliser le domaine Internet de votre organisation dans les adresses de messagerie. Vous pouvez effectuer tout cela à partir du tableau de bord, sans vous connecter à Office 365. (Vous devez utiliser Windows Server Essentials pour gérer les groupes de distribution à partir du tableau de bord. Cette fonctionnalité n’est pas prise en charge dans Windows Server Essentials.) Pour plus d’informations, consultez [gérer les adresses de messagerie pour Exchange Online](#BKMK_SECTION_ManageEmailAddresses) et [gérer les groupes de distribution pour Exchange Online](#BKMK_SECTION_ManageDistributionGroups).  
  
### <a name="manage-the-user-account-and-online-account-together"></a>Gérer ensemble le compte d'utilisateur et le compte en ligne  
 En outre, vous pouvez gérer un compte en ligne avec le compte d’utilisateur tout au long du cycle de vie des comptes. Si vous désactivez le compte d'utilisateur, le compte en ligne est désactivé dans Microsoft Online Services. Si vous supprimez un compte d'utilisateur, le compte en ligne est également supprimé. Pour plus d'informations, voir [Gérer les comptes en ligne](#BKMK_SECTION_ManageOnlineAccounts).  
  
##  <a name="create-online-accounts"></a><a name="BKMK_SECTION_CreateOnlineAccounts"></a>Créer des comptes en ligne  
 Une fois que vous avez intégré votre serveur à Office 365, il vous permet de créer des comptes Microsoft Online Services pour vos utilisateurs à partir du tableau de bord. Vous bénéficiez d’une grande flexibilité pour créer des comptes en ligne. Si vous avez un nouvel abonnement Office 365, vous pouvez créer en bloc des comptes en ligne pour tous vos utilisateurs. Si vous avez déjà créé vos comptes en ligne dans Office 365, ne vous inquiétez pas. Si vous configurez un nouveau serveur, vous pouvez créer vos comptes d’utilisateur sur le serveur en important les comptes en ligne. Vous pouvez aussi affecter un compte en ligne nouveau ou existant lorsque vous créez un compte d’utilisateur individuel ou lorsque vous ajoutez un compte en ligne à un compte d’utilisateur existant.  
  
 **Conditions de licence** Vous aurez besoin d’une licence utilisateur pour chaque compte en ligne que vous créez. Consultez la page **office 365** dans le tableau de bord pour connaître le nombre de licences utilisateur disponibles via votre abonnement Office 365. Si vous devez ajouter d’autres licences utilisateur, vous pouvez ouvrir votre abonnement Office 365 dans Office 365 pour ce faire.  
  
 **Suivi pour les utilisateurs** Une fois que vous avez ajouté un compte en ligne pour un compte d’utilisateur, l’utilisateur doit modifier le mot de passe de son compte d’utilisateur la prochaine fois qu’il se connecte. Le nouveau mot de passe est immédiatement synchronisé avec son compte en ligne. Après cela, ils peuvent utiliser le mot de passe pour se connecter à Office 365 avec leur IDENTIFIant en ligne.  
  
> [!IMPORTANT]
>  Insistez sur le faire pour les utilisateurs qu’ils ne doivent jamais modifier leur mot de passe de compte en ligne dans Office 365. Le mot de passe est modifié automatiquement chaque fois qu'ils changent le mot de passe de leur compte d'utilisateur. S’ils modifient le mot de passe en ligne dans Office 365, la synchronisation de mot de passe est interrompue.  
  
 Utilisez les procédures décrites dans cette section pour effectuer les opérations suivantes :  
  
-   [Créer en bloc des comptes en ligne pour vos comptes d’utilisateur existants](#BKMK_ToBulkCreateOnlineAccounts)  
  
-   [Importer des comptes d’utilisateur à partir de vos comptes Microsoft Online Services](#BKMK_ToImportUserAccounts)  
  
-   [Créer un nouveau compte d’utilisateur avec un compte en ligne qui lui est affecté](#BKMK_ToCreateaNewUserAccount)  
  
-   [Affecter un compte en ligne à un compte d’utilisateur](#BKMK_ToAssignAnOnlineAccount)  
  
> [!NOTE]
>  Si vous utilisez Windows Server Essentials, vous verrez le *compte Office 365* au lieu du *compte Microsoft Online Services* tout au long de ces procédures. Le processus est le même, mais la terminologie a changé dans Windows Server Essentials.  
  
###  <a name="to-bulk-create-online-accounts-for-your-existing-user-accounts"></a><a name="BKMK_ToBulkCreateOnlineAccounts"></a>Pour créer en bloc des comptes en ligne pour vos comptes d’utilisateur existants  
  
1.  Connectez-vous au serveur en tant qu’administrateur, puis ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans le tableau de bord, ouvrez la page **Utilisateurs**.  
  
3.  Dans **Tâches Utilisateurs**, cliquez sur **Ajouter des comptes en ligne Microsoft**.  
  
     La page **Ajouter des comptes Microsoft Online Services** de l'Assistant affiche tous les comptes d'utilisateur qui n'ont pas de compte Microsoft en ligne. Tous les comptes sont sélectionnés par défaut, et le nom d'utilisateur est suggéré pour l'ID en ligne Microsoft. Si vous avez lié un domaine Internet personnalisé à votre abonnement Office 365, ce domaine sera utilisé par défaut.  
  
4.  Dans la page **Ajouter des comptes Microsoft Online Services**, passez en revue les comptes qui seront créés. Par exemple, recherchez les utilisateurs qui disposent déjà d'un compte en ligne avec un autre ID en ligne, et assurez-vous que le domaine que vous souhaitez utiliser dans des adresses de messagerie est sélectionné. Lorsque vous avez terminé d'apporter les modifications appropriées, cliquez sur **Suivant**.  
  
5.  Dans la page **affecter des licences Microsoft Online Services** , sélectionnez les services Office 365 que vos utilisateurs utiliseront. Lorsque vous cliquez sur **Suivant**, la création du compte commence.  
  
    > [!NOTE]
    >  Vous devez disposer d’une licence de service dans Office 365 pour chaque compte en ligne. Vous pouvez vérifier vos licences disponibles et, si nécessaire, ouvrir votre abonnement pour ajouter des licences à partir de la page **Office 365** du tableau de bord.  
  
6.  Informez les utilisateurs qu'ils disposent désormais d'un compte en ligne Microsoft. Ils doivent modifier le mot de passe de leur compte d’utilisateur réseau pour pouvoir se connecter à Office 365. Pour obtenir des instructions, voir [Apprendre à utiliser un nouveau compte en ligne Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
###  <a name="to-begin-using-a-new-microsoft-online-account"></a><a name="BKMK_ToBeginUsingAnOnlineAccount"></a>Pour commencer à utiliser un nouveau compte en ligne Microsoft  
  
1.  Connectez-vous sur votre ordinateur avec votre compte d'utilisateur réseau.  
  
2.  Modifiez le mot de passe de votre compte d'utilisateur. Pour commencer, appuyez sur Ctrl+Alt+Suppr, puis cliquez sur **Modifier un mot de passe**.  
  
     Lorsque vous modifiez votre mot de passe, le mot de passe est synchronisé avec votre nouveau compte en ligne. Vous pouvez maintenant utiliser le même mot de passe pour vous connecter à Office 365.  
  
3.  [Connectez-vous à Office 365](https://login.microsoftonline.com/login.srf?wa=wsignin1.0&rpsnv=3&ct=1398981834&rver=6.1.6206.0&wp=MBI_SSL&wreply=https:%2F%2Foutlook.office365.com%2Fowa%2F&id=260563&CBCXT=out) à l’aide de votre nouvel ID en ligne et de votre mot de passe de compte d’utilisateur.  
  
    > [!IMPORTANT]
    >  Ne modifiez pas le mot de passe de votre compte en ligne dans Office 365. Cela bloque la synchronisation des mots de passe. Votre mot de passe en ligne sera mis à jour chaque fois que vous modifierez le mot de passe pour votre compte d'utilisateur réseau.  
  
###  <a name="to-import-user-accounts-from-your-existing-online-accounts"></a><a name="BKMK_ToImportUserAccounts"></a>Pour importer des comptes d’utilisateur à partir de vos comptes en ligne existants  
  
1.  Dans le tableau de bord, ouvrez la page **Utilisateurs**.  
  
2.  Dans **Tâches Utilisateurs**, cliquez sur **Importer les comptes depuis Office 365**.  
  
     La page suivante affiche tous les comptes en ligne de votre abonnement Office 365 qui n’ont pas de compte d’utilisateur sur le serveur. Tous les comptes sont sélectionnés par défaut, et l'ID en ligne est suggéré pour le nom d'utilisateur.  
  
3.  Pour créer les comptes d'utilisateur :  
  
    1.  Apportez toutes les modifications nécessaires pour les comptes d'utilisateur proposés.  
  
    2.  Si vous le souhaitez, cliquez sur le lien pour afficher les mots de passe temporaires qui seront affectés aux comptes d'utilisateur. Vous devez donner à vos utilisateurs leur mot de passe temporaire avec le nom de leur nouveau compte.  
  
         (Après avoir créé les comptes, vous trouverez les mots de passe suivants dans ce fichier : *lecteur_système*\Utilisateurs\\*Office365admin*\\*NewServerUser*. txt, où *Office365admin* est le compte réseau utilisé pour administrer Office 365 sur le serveur et *NewServerUser* est le nouveau nom de compte d’utilisateur.)  
  
    3.  Cliquez sur **suivant** pour créer les comptes d'utilisateur.  
  
4.  Informez vos utilisateurs de leurs nouveaux comptes d’utilisateur et des mots de passe temporaires qu’ils utiliseront pour se connecter au serveur pour la première fois et qu’ils devront modifier le mot de passe après s’être connecté. Pour obtenir des instructions pour vos utilisateurs, consultez la rubrique [Pour commencer à utiliser un compte en ligne Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
     Assurez-vous qu’ils savent que les mots de passe de leur compte en ligne seront synchronisés avec leur compte d’utilisateur, et qu’ils ne doivent pas modifier leur mot de passe en ligne dans Office 365.  
  
###  <a name="to-create-a-new-user-account-with-an-online-account-assigned-to-it"></a><a name="BKMK_ToCreateaNewUserAccount"></a>Pour créer un nouveau compte d’utilisateur avec un compte en ligne qui lui est affecté  
  
1.  Dans le tableau de bord, cliquez sur **Utilisateurs**.  
  
2.  Dans **Tâches Utilisateurs**, cliquez sur **Ajouter un compte d'utilisateur**. L'Assistant Ajout d'un compte d'utilisateur s'affiche.  
  
3.  Suivez les instructions pour créer le compte d'utilisateur.  
  
4.  Dans la page **Affecter un compte Microsoft Online Services**, créez un compte en ligne pour l'utilisateur ou affectez un compte en ligne existant :  
  
    -   Pour créer un compte en ligne, cliquez sur **Créer un compte Microsoft Online Services et l'affecter à ce compte d'utilisateur**, tapez un nom pour le compte Microsoft Online Services (par défaut, le nom d'utilisateur est utilisé pour l'ID en ligne). Ensuite, cliquez sur **Suivant**.  
  
    -   Pour affecter un compte en ligne Microsoft existant, cliquez sur **Affecter un compte Microsoft Online Services existant à ce compte d'utilisateur**, puis sélectionnez un compte existant dans la liste déroulante. Ensuite, cliquez sur **Suivant**.  
  
    > [!NOTE]
    >  Dans Windows Server Essentials, les comptes Microsoft Online Services sont appelés comptes Office 365 dans les assistants et les étiquettes du tableau de bord.  
  
5.  Suivez les instructions pour exécuter l'Assistant.  
  
6.  Indiquez aux utilisateurs qu'ils devront modifier le mot de passe de leur compte d'utilisateur avant de pouvoir se connecter à Office 365 avec leur nouveau compte en ligne. Pour obtenir des instructions, voir [Apprendre à utiliser un nouveau compte en ligne Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
#### <a name="to-assign-an-online-account-to-a-user-account"></a><a name="BKMK_ToAssignAnOnlineAccount"></a>Pour affecter un compte en ligne à un compte d’utilisateur  
  
1.  Dans le tableau de bord, cliquez sur **Utilisateurs**.  
  
2.  Cliquez avec le bouton droit sur le compte d'utilisateur dans la liste, puis cliquez sur **Affecter un compte en ligne Microsoft**. L'Assistant Affecter un compte Microsoft Online Services s'affiche.  
  
3.  Affectez un compte en ligne existant ou créez-en un pour l'utilisateur. L'ID en ligne par défaut pour un nouveau compte est le nom d'utilisateur. Cliquez ensuite sur **Suivant** pour ajouter le compte en ligne au compte d'utilisateur.  
  
4.  Passez en revue les informations affichez dans la dernière page de l'Assistant, puis cliquez sur **Fermer**.  
  
5.  Indiquez aux utilisateurs qu'ils devront modifier le mot de passe de leur compte d'utilisateur avant de pouvoir se connecter à Office 365 avec leur nouveau compte en ligne. Pour obtenir des instructions, voir [Apprendre à utiliser un nouveau compte en ligne Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
##  <a name="manage-online-accounts"></a><a name="BKMK_SECTION_ManageOnlineAccounts"></a>Gérer les comptes en ligne  
 Lorsque vous ajoutez un compte en ligne à un compte d’utilisateur dans Windows Server Essentials, vous pouvez gérer les deux comptes ensemble tout au long du cycle de vie des comptes.  
  
###  <a name="understanding-the-online-account-status"></a><a name="BKMK_UnderstandingAccountStatus"></a>Compréhension de l’état du compte en ligne  
 Lorsque vous affectez un compte Microsoft Online Services à un compte d'utilisateur, l'adresse de messagerie du compte s'affiche dans la colonne **Compte en ligne Microsoft** de la page **Utilisateurs** du tableau de bord. (Dans Windows Server Essentials, l’étiquette de colonne est **compte Office 365**.)  
  
-   Une icône bleue en regard d'une adresse de messagerie indique que le compte en ligne est actif. Autrement dit, le compte dispose d’une licence Office 365 en cours et l’utilisateur peut utiliser l’ID en ligne pour se connecter à Office 365.  
  
-   Une icône grisée en regard de l’adresse de messagerie indique que le compte en ligne est inactif, soit parce que la licence n’est plus active, soit parce que le compte en ligne n’a pas été attribué. Lorsque vous supprimez l’attribution d’un compte utilisateur en ligne, la licence est supprimée et l’utilisateur est bloqué de se connecter à Office 365 à l’aide du compte. Toutefois, le serveur gère le mappage entre le nom du compte d’utilisateur et l’adresse de messagerie Office 365.  
  
###  <a name="restrict-access-to-an-online-account"></a><a name="BKMK_UnassignOnlineAccount"></a>Restreindre l’accès à un compte en ligne  
 Que faire si un utilisateur quitte votre organisation ou si vous souhaitez limiter l’accès des utilisateurs aux services Office 365 ? Si vous gérez les comptes en ligne de vos utilisateurs avec leurs comptes d’utilisateur dans Windows Server Essentials, vous avez trois options :  
  
-   **Supprimer l’affectation du compte en ligne** ? Si vous souhaitez empêcher un utilisateur d’utiliser Office 365 sans empêcher l’accès aux ressources sur le serveur, vous devez supprimer l’attribution du compte en ligne. La licence Office 365 sera publiée et l’utilisateur ne pourra pas se connecter à Office 365. Toutefois, le serveur gère le mappage entre le nom du compte d’utilisateur et l’adresse de messagerie Office 365. Pour obtenir des instructions, consultez [pour supprimer l’attribution d’un compte en ligne à un compte d’utilisateur](#BKMK_ToUnassignAnOnlineAccount).  
  
-   **Désactiver le compte d’utilisateur** ? Si vous désactivez un compte d’utilisateur car un employé quitte temporairement ou définitivement, le compte utilisateur en ligne est également désactivé. Le compte en ligne ne peut pas être utilisé, mais les données utilisateur, notamment le courrier électronique, sont conservées dans Microsoft Online Services. Pour obtenir des instructions, consultez [désactiver un compte d’utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage6) dans [gérer les comptes d’utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
-   **Supprimer le compte d’utilisateur** ? Si vous supprimez un compte d’utilisateur, le compte en ligne est également supprimé de Microsoft Online Services. Pour obtenir des instructions, consultez [supprimer un compte d’utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Remove) dans [gérer les comptes d’utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
    > [!WARNING]
    >  Notez que, lorsqu'un compte en ligne est supprimé, les données utilisateur sont soumises aux stratégies de conservation des données de Microsoft Online Services. Si vous devez conserver les données utilisateur de la personne après la sortie d’un employé, désactivez le compte d’utilisateur au lieu de le supprimer.  
  
####  <a name="to-unassign-an-online-account-from-a-user-account"></a><a name="BKMK_ToUnassignAnOnlineAccount"></a>Pour supprimer l’attribution d’un compte en ligne à un compte d’utilisateur  
  
1.  Dans le tableau de bord, cliquez sur **Utilisateurs**.  
  
2.  Cliquez avec le bouton droit sur le compte d’utilisateur dans la liste, puis cliquez sur **Supprimer l’attribution d’un compte en ligne Microsoft** (dans Windows Server Essentials, cliquez sur **Supprimer l’attribution d’un compte Office 365**).  
  
3.  À l'invite de confirmation, cliquez sur **Oui**.  
  
##  <a name="manage-email-addresses-for-exchange-online"></a><a name="BKMK_SECTION_ManageEmailAddresses"></a>Gérer les adresses de messagerie pour Exchange Online  
 En ajoutant des adresses de messagerie au compte utilisateur en ligne dans Windows Server Essentials, vous pouvez autoriser l’utilisateur à recevoir des courriers électroniques à plusieurs adresses de messagerie dans Exchange Online.  
  
###  <a name="to-add-additional-email-addresses-to-a-user-s-microsoft-online-account"></a><a name="BKMK_PROC_AddEmailAliases"></a>Pour ajouter des adresses de messagerie supplémentaires à un compte en ligne Microsoft de l’utilisateur  
  
1.  Dans le tableau de bord Windows Server Essentials, cliquez sur **utilisateurs**.  
  
2.  Cliquez avec le bouton droit sur le compte d'utilisateur dans la liste, puis cliquez sur **Afficher les propriétés du compte**.  
  
3.  Sous l’onglet **Microsoft Online** des propriétés du compte (ou l’onglet **Office 365** dans Windows Server Essentials), cliquez sur **Ajouter**.  
  
4.  Tapez le nouvel alias de messagerie électronique, puis sélectionnez le domaine de messagerie.  
  
5.  Cliquez deux fois sur **OK**.  
  
##  <a name="manage-distribution-groups-for-exchange-online-windows-server-essentials-only"></a><a name="BKMK_SECTION_ManageDistributionGroups"></a>Gérer des groupes de distribution pour Exchange Online (Windows Server Essentials uniquement)  
 Après avoir intégré votre serveur Windows Server Essentials à Office 365, vous pouvez créer et gérer des groupes de distribution pour Exchange Online à partir du tableau de bord Windows Server Essentials. Pour ce faire, utilisez l’onglet **groupes de distribution** qui est ajouté à la page **utilisateurs** . Cet onglet s'affiche uniquement si vous disposez d'un abonnement Exchange Online. Cette fonctionnalité n’est pas disponible dans Windows Server Essentials.  
  
 Utilisez les procédures suivantes pour :  
  
-   [Ajouter un groupe de distribution](#BKMK_PROCEDURE_AddDistGroup)  
  
-   [Modifier les membres d’un groupe de distribution](#BKMK_ChangeGroupMembers)  
  
-   [Modifier les appartenances aux groupes de distribution des utilisateurs](#BKMK_EditUserMemberships)  
  
-   [Supprimer un groupe de distribution](#BKMK_RemoveDistributionGroup)  
  
###  <a name="to-add-a-distribution-group"></a><a name="BKMK_PROCEDURE_AddDistGroup"></a>Pour ajouter un groupe de distribution  
  
1.  Dans le tableau de bord de Windows Server Essentials, cliquez sur **utilisateurs**, puis sur l’onglet **groupes de distribution** .  
  
2.  Dans **Tâches Groupe de distribution**, cliquez sur **Ajouter un groupe de distribution**.  
  
     L'Assistant Ajouter un groupe de distribution s'affiche.  
  
3.  Dans la page **Ajouter un groupe de distribution**, entrez les informations suivantes, puis cliquez sur **Suivant** :  
  
    -   Entrez un nom de groupe, une description facultative et alias de messagerie électronique pour le nouveau groupe de distribution.  
  
    -   Par défaut, le groupe de distribution peut recevoir du courrier électronique de personnes extérieures à votre organisation. Si vous ne souhaitez pas l'autoriser, désactivez cette option.  
  
4.  Dans la page **Ajouter des membres du groupe**, utilisez le bouton **Ajouter** pour ajouter au nouveau groupe de distribution des comptes d'utilisateur actifs auxquels un compte en ligne est affecté, ainsi que d'autres groupes de distribution. Ensuite, cliquez sur **Suivant**.  
  
     Le nouveau groupe de distribution est créé dans Exchange Online.  
  
###  <a name="to-change-the-members-of-a-distribution-group"></a><a name="BKMK_ChangeGroupMembers"></a>Pour modifier les membres d’un groupe de distribution  
  
1.  Dans le tableau de bord, cliquez sur **Utilisateurs**, puis cliquez sur l'onglet **Groupes de distribution**.  
  
2.  Cliquez avec le bouton droit sur le groupe de distribution dans la liste, puis cliquez sur **Modifier l'appartenance au groupe**.  
  
3.  Utilisez les boutons **Ajouter** et **Supprimer** pour ajouter ou supprimer des comptes en ligne actifs dans le groupe de distribution. Cliquez ensuite sur **Suivant** pour mettre à jour l'appartenance au groupe de distribution dans Exchange Online.  
  
###  <a name="to-change-a-user-s-distribution-group-memberships"></a><a name="BKMK_EditUserMemberships"></a>Pour modifier les appartenances aux groupes de distribution des utilisateurs  
  
1.  Dans le tableau de bord, cliquez sur **Utilisateurs**.  
  
2.  Cliquez avec le bouton droit sur le compte d'utilisateur dans la liste, puis cliquez sur **Afficher les propriétés du compte**.  
  
3.  Dans les propriétés du compte d'utilisateur, cliquez sur l'onglet **Groupes de distribution**, puis sur **Modifier**.  
  
4.  Dans la zone **Modifier l'appartenance au groupe**, utilisez les boutons **Ajouter** et **Supprimer** pour ajouter ou supprimer des groupes de distribution pour le compte d'utilisateur, puis cliquez sur **Fermer**.  
  
5.  Cliquez sur **OK** pour enregistrer les propriétés du compte d'utilisateur mises à jour.  
  
###  <a name="to-remove-a-distribution-group"></a><a name="BKMK_RemoveDistributionGroup"></a>Pour supprimer un groupe de distribution  
  
1.  Dans le tableau de bord, cliquez sur **Utilisateurs**, puis cliquez sur l'onglet **Groupes de distribution**.  
  
2.  Cliquez avec le bouton droit sur le groupe de distribution dans la liste, puis cliquez sur **Supprimer le groupe**.  
  
3.  À l'invite de confirmation, cliquez sur **Supprimer le groupe**.  
  
     Le groupe de distribution est supprimé d'Exchange Online.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer des comptes d’utilisateurs](Manage-User-Accounts-in-Windows-Server-Essentials.md)  
  
-   [Gérer Office 365](Manage-Office-365-in-Windows-Server-Essentials.md)  
  
-   [Gérer Microsoft Online Services](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)
