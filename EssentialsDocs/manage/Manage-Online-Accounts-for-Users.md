---
title: "Gérer les comptes en ligne des utilisateurs WindowsServerEssentials"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c09f4cf6-4d12-49fe-9ae4-e6cb14027b9d
8author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 95f401bbec9bb503d19e2d9918a05851c04f7ef4
ms.sourcegitcommit: 877a50cd8d6e727048cdfac9b614a98ac3220876
ms.translationtype: HT
ms.contentlocale: fr-FR
---
# <a name="manage-online-accounts-for-windows-server-essentials-users"></a>Gérer les comptes en ligne des utilisateurs WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Lorsque vous intégrez votre serveur WindowsServerEssentials à MicrosoftOffice 365, vous pouvez gérer vos comptes en ligne avec les comptes d’utilisateur du tableau de bord. Dans cette rubrique, vous tout savoir ce que vous pouvez obtenir par la gestion de vos utilisateurs de comptes MicrosoftOnline Services à partir du tableau de bord, comment créer et gérer des comptes en ligne à partir du tableau de bord et comment gérer des adresses de messagerie et de groupes de distribution pour Exchange Online à partir du tableau de bord.  

  
> [!NOTE]
>  Pour gérer vos comptes MicrosoftOnline Services dans WindowsServerEssentials, vous devez intégrer votre serveur à Office 365. Pour obtenir des instructions, voir [gérer Office 365](Manage-Office-365-in-Windows-Server-Essentials.md).  
  
> [!IMPORTANT]
>  Si vous la gestion des comptes en ligne dans WindowsServerEssentials, vous l’habitude de voir les comptes MicrosoftOnline Services appelés *comptes Office 365*. Sur le tableau de bord dans WindowsServerEssentials, les étiquettes ont été modifiés pour *comptes MicrosoftOnline Services*, ou *comptes en ligne Microsoft* pour la forme courte. Les comptes et les procédures sont identiques; les étiquettes modifiés. La plupart des procédures décrites dans cette rubrique utilisent le terme *compte en ligne*.  
  
## <a name="in-this-topic"></a>Dans cette rubrique.  
  
-   [Pourquoi dois-je gérer mes comptes en ligne à partir du tableau de bord?](#BKMK_WhyManageOnlineAccounts)  
  
-   [Créer des comptes en ligne](#BKMK_SECTION_CreateOnlineAccounts)  
  
-   [Gérer les comptes en ligne](#BKMK_SECTION_ManageOnlineAccounts)  
  
-   [Gérer les adresses de messagerie pour Exchange Online](#BKMK_SECTION_ManageEmailAddresses)  
  
-   [Gérer des groupes de distribution pour Exchange Online](#BKMK_SECTION_ManageDistributionGroups)  
  
##  <a name="BKMK_WhyManageOnlineAccounts"></a>Pourquoi dois-je gérer mes comptes en ligne à partir du tableau de bord?  
 Lorsque vous utilisez le tableau de bord pour affecter un compte MicrosoftOnline Services à un compte d’utilisateur, les mots de passe sont automatiquement synchronisées, et vous pouvez conserver les deux comptes ensemble tout au long du cycle de vie s de compte utilisateur.  
  
 Il est pratique pour l’utilisateur, ce qui peut utiliser le même mot de passe pour accéder aux ressources sur le serveur et dans Office 365. Et vous pouvez appliquer les mêmes exigences de mot de passe pour l’accès aux ressources dans Office 365 dont vous avez besoin pour vos ressources internes.  
  
### <a name="how-does-password-synchronization-work"></a>Comment fonctionne la synchronisation de mot de passe?  
 Lorsque vous utilisez le tableau de bord pour affecter un compte MicrosoftOnline Services à un compte d’utilisateur, le mot de passe de compte d’utilisateur est automatiquement synchronisé avec le compte utilisateur en ligne. Cela signifie qu’un utilisateur nécessite seulement un mot de passe pour accéder aux ressources sur le serveur et dans Office 365. En outre, vous pouvez utiliser le même nom pour le compte d’utilisateur et l’ID utilisateur s en ligne.  
  
 Synchronisation de mot de passe se produit immédiatement et automatiquement quand un utilisateur modifie le mot de passe pour son compte d’utilisateur à partir d’un ordinateur joint au domaine ou à l’aide de l’accès Web à distance.  
  
> [!IMPORTANT]
>  Si Office 365 est intégré à WindowsServerEssentials, les utilisateurs ne doivent pas modifier le mot de passe de leur compte en ligne Microsoft à partir du portail Office 365. Cela bloque la synchronisation de mot de passe.  
  
### <a name="simplified-account-creation"></a>Création de compte simplifiée  
 Il existe un autre avantage lorsque vous créez vos comptes en ligne initiales à partir du tableau de bord. Vous pouvez créer des comptes en ligne pour tous les utilisateurs en une seule action. En revanche, si vos employés utilisent déjà Office 365 et que vous configurez un nouveau serveur WindowsServerEssentials, vous pouvez créer tous vos comptes d’utilisateur à partir des comptes en ligne en une seule action. Pour plus d’informations, voir [créer des comptes en ligne](#BKMK_SECTION_CreateOnlineAccounts).  
  
### <a name="manage-email-addresses-and-distribution-groups-from-the-dashboard"></a>Gérer les adresses de messagerie et de groupes de distribution à partir du tableau de bord  
 Vous serez en mesure de gérer vos adresses de messagerie et les groupes de distribution pour Exchange Online à partir du tableau de bord. Et vous pouvez utiliser votre domaine d’Internet s organisation dans les adresses de messagerie. Vous pouvez effectuer toutes ces du tableau de bord, sans vous connecter à Office 365. (Vous devez être à l’aide de WindowsServerEssentials pour gérer des groupes de distribution à partir du tableau de bord. Cette fonctionnalité n'est pas prise en charge dans WindowsServerEssentials.) Pour plus d’informations, voir [gérer les adresses de messagerie pour Exchange Online](#BKMK_SECTION_ManageEmailAddresses) et [gérer des groupes de distribution pour Exchange Online](#BKMK_SECTION_ManageDistributionGroups).  
  
### <a name="manage-the-user-account-and-online-account-together"></a>Gérer le compte d’utilisateur et un compte en ligne ensemble  
 Et vous pouvez gérer un compte en ligne, ainsi que le compte d’utilisateur tout au long du cycle de vie du compte s. Si vous désactivez le compte d’utilisateur, le compte en ligne est également désactivé dans MicrosoftOnline Services. Si vous supprimez un compte d’utilisateur, le compte en ligne est également supprimé. Pour plus d’informations, voir [gérer les comptes en ligne](#BKMK_SECTION_ManageOnlineAccounts).  
  
##  <a name="BKMK_SECTION_CreateOnlineAccounts"></a>Créer des comptes en ligne  
 Après avoir intégré votre serveur à Office 365, il est de votre intérêt à créer des comptes MicrosoftOnline Services pour vos utilisateurs à partir du tableau de bord. Vous devez une grande souplesse dans la création de comptes en ligne. Si vous avez un abonnement Office 365, vous pouvez créer en bloc des comptes en ligne pour tous les utilisateurs. Si vous avez déjà créé vos comptes en ligne dans Office 365, ne vous inquiétez pas. Si vous configurez un nouveau serveur, vous pouvez créer vos comptes d’utilisateur sur le serveur en important les comptes en ligne. Et vous pouvez affecter un nouveau ou un compte en ligne existant lorsque vous créez un compte d’utilisateur ou lorsque vous ajoutez un compte en ligne à un compte d’utilisateur existant.  
  
 **Exigences de la licence** vous besoin d’une licence utilisateur pour chaque compte en ligne que vous créez. Vérifiez le **Office 365** page du tableau de bord pour connaître le nombre de licences utilisateur disponible via votre abonnement Office 365. Si vous avez besoin ajouter des licences utilisateur, vous serez en mesure d’ouvrir votre abonnement Office 365dans Office 365 pour effectuer cette opération.  
  
 **Suivi des utilisateurs** après avoir ajouté un compte en ligne pour un compte d’utilisateur, l’utilisateur est nécessaire pour modifier le mot de passe pour son compte d’utilisateur la prochaine fois qu’ils ouvrent une session. Le nouveau mot de passe est immédiatement synchronisé avec son compte en ligne. Après cela, ils peuvent utiliser le mot de passe pour se connecter à Office 365 avec son ID en ligne.  
  
> [!IMPORTANT]
>  Mettre en évidence à vos utilisateurs qu’ils ne doivent jamais changer le mot de passe de leur compte en ligne dans Office 365. Le mot de passe est modifié automatiquement chaque fois qu’ils modifier le mot de passe pour son compte d’utilisateur. S’ils modifient le mot de passe en ligne dans Office 365, la synchronisation de mot de passe sera rompue.  
  
 Utilisez les procédures de cette section pour:  
  
-   [Créer en bloc des comptes en ligne pour vos comptes d’utilisateur existant](#BKMK_ToBulkCreateOnlineAccounts)  
  
-   [Importer des comptes d’utilisateur à partir de vos comptes MicrosoftOnline Services](#BKMK_ToImportUserAccounts)  
  
-   [Créer un nouveau compte d’utilisateur avec un compte en ligne](#BKMK_ToCreateaNewUserAccount)  
  
-   [Affecter un compte en ligne à un compte d’utilisateur](#BKMK_ToAssignAnOnlineAccount)  
  
> [!NOTE]
>  Si vous utilisez WindowsServerEssentials, vous verrez *compte Office 365* au lieu de *compte MicrosoftOnline Services* tout au long de ces procédures. Le processus est le même, mais la terminologie a changé dans WindowsServerEssentials.  
  
###  <a name="BKMK_ToBulkCreateOnlineAccounts"></a>Pour créer en bloc des comptes en ligne pour vos comptes d’utilisateur existants  
  
1.  Ouvrez une session sur le serveur en tant qu’administrateur et ouvrez le tableau de bord WindowsServerEssentials.  
  
2.  Sur le tableau de bord, ouvrez le **utilisateurs** page.  
  
3.  Dans **tâches utilisateurs**, cliquez sur **comptes en ligne Microsoft ajouter**.  
  
     Le **comptes ajouter MicrosoftOnline Services** page de l’Assistant affiche tous les comptes d’utilisateurs qui n’ont pas d’un compte en ligne Microsoft. Tous les comptes sont sélectionnés par défaut, et le nom d’utilisateur est suggéré pour l’ID en ligne de Microsoft. Si vous avez lié un domaine Internet personnalisé à votre abonnement Office 365, ce domaine sera utilisé par défaut.  
  
4.  Sur le **comptes ajouter MicrosoftOnline Services** page, passez en revue les comptes qui seront créés. Par exemple, recherchez les utilisateurs qui disposent d’un compte en ligne avec un autre ID en ligne déjà et vérifiez que le domaine que vous souhaitez utiliser dans les adresses de messagerie est sélectionné. Lorsque vous avez terminé d’apporter des modifications appropriées, cliquez sur **suivant**.  
  
5.  Sur le **licences affecter MicrosoftOnline Services** page, sélectionnez services Office 365vos utilisateurs utiliseront. Lorsque vous cliquez sur **suivant**, la création du compte commence.  
  
    > [!NOTE]
    >  Vous devez posséder une licence de service dans Office 365 pour chaque compte en ligne. Vous pouvez vérifier vos licences disponibles et, si nécessaire, ouvrir votre abonnement pour ajouter des licences à partir de la **Office 365** page du tableau de bord.  
  
6.  Informez les utilisateurs qu’ils disposent désormais d’un compte en ligne Microsoft. Il peut se connecter à Office 365, ils doivent modifier leur mot de passe du compte utilisateur réseau. Pour obtenir des instructions, voir [pour commencer à utiliser un compte en ligne Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
###  <a name="BKMK_ToBeginUsingAnOnlineAccount"></a>Pour commencer à utiliser un compte en ligne Microsoft  
  
1.  Ouvrez une session sur votre ordinateur avec votre compte d’utilisateur réseau.  
  
2.  Modifier le mot de passe pour votre compte d’utilisateur. Pour commencer, appuyez sur Ctrl + Alt + Suppr, puis cliquez sur **modifier un mot de passe**.  
  
     Lorsque vous modifiez votre mot de passe, le mot de passe est synchronisé avec votre nouveau compte en ligne. Vous pouvez maintenant utiliser le même mot de passe pour se connecter à Office 365.  
  
3.  [Connectez-vous à Office 365](https://login.microsoftonline.com/login.srf?wa=wsignin1.0&rpsnv=3&ct=1398981834&rver=6.1.6206.0&wp=MBI_SSL&wreply=https:%2F%2Foutlook.office365.com%2Fowa%2F&id=260563&CBCXT=out) à l’aide de votre nouvel ID en ligne et votre mot de passe de compte d’utilisateur.  
  
    > [!IMPORTANT]
    >  Ne modifiez pas votre mot de passe du compte en ligne dans Office 365. Cela bloque la synchronisation de mot de passe. Votre mot de passe en ligne est mis à jour chaque fois que vous modifiez le mot de passe pour votre compte d’utilisateur réseau.  
  
###  <a name="BKMK_ToImportUserAccounts"></a>Pour importer des comptes d’utilisateur à partir de vos comptes en ligne existants  
  
1.  Sur le tableau de bord, ouvrez le **utilisateurs** page.  
  
2.  Dans **tâches utilisateurs**, cliquez sur **importer les comptes depuis Office 365**.  
  
     La page suivante affiche tous les comptes en ligne pour votre abonnement Office 365 qui n’ont pas un compte d’utilisateur sur le serveur. Tous les comptes sont sélectionnés par défaut, et l’ID en ligne est suggéré pour le nom d’utilisateur.  
  
3.  Pour créer les comptes d’utilisateur:  
  
    1.  Apportez les modifications nécessaires pour les comptes d’utilisateur proposés.  
  
    2.  Si vous le souhaitez, cliquez sur le lien pour afficher les mots de passe temporaires qui seront affectés aux comptes d’utilisateur. Vous devez donner à vos utilisateurs leur mot de passe temporaire, ainsi que leur nouveau nom de compte.  
  
         (Après avoir créé les comptes, tout vous trouver ces mots de passe répertoriés dans ce fichier: *SystemDrive*\Users\\*Office365admin*\\*Nouvel_utilisateur_serveur*.txt où *Office365admin* est le compte réseau qui est utilisé pour gérer Office 365 sur le serveur et *Nouvel_utilisateur_serveur* est le nouveau nom de compte d’utilisateur.)  
  
    3.  Cliquez sur **suivant** pour créer les comptes d’utilisateur.  
  
4.  Informez vos utilisateurs leurs nouveaux comptes d’utilisateur et mots de passe temporaires qu’ils utiliseront pour se connecter sur le serveur pour la première section de temps et qu’ils doivent modifier le mot de passe une fois qu’ils se connectent. Pour obtenir des instructions pour vos utilisateurs, voir [pour commencer à utiliser un compte en ligne Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
     Veillez à ce qu’ils savent que les mots de passe de leur compte en ligne seront synchronisés avec leur compte d’utilisateur à l’avenir, et ils ne doivent pas modifier leur mot de passe en ligne dans Office 365.  
  
###  <a name="BKMK_ToCreateaNewUserAccount"></a>Pour créer un nouveau compte d’utilisateur avec un compte en ligne  
  
1.  Dans le tableau de bord, cliquez sur **utilisateurs**.  
  
2.  Dans **tâches utilisateurs**, cliquez sur **ajouter un compte d’utilisateur**. L’ajout de qu'un Assistant de compte d’utilisateur s’affiche.  
  
3.  Suivez les instructions pour créer le compte d’utilisateur.  
  
4.  Sur le **affecter un compte MicrosoftOnline Services** page, créez un en ligne de compte pour l’utilisateur ou affectez un compte en ligne existant:  
  
    -   Pour créer un nouveau compte en ligne, cliquez sur **créer un compte MicrosoftOnline Services et l’affecter à ce compte d’utilisateur**, tapez un nom pour le compte MicrosoftOnline Services (par défaut, le nom d’utilisateur est utilisé pour l’ID en ligne). Puis cliquez sur **suivant**.  
  
    -   Pour affecter un compte en ligne Microsoft existant, cliquez sur **affecter un compte MicrosoftOnline Services existant à ce compte d’utilisateur**, puis sélectionnez un compte existant dans la liste déroulante. Puis cliquez sur **suivant**.  
  
    > [!NOTE]
    >  Dans WindowsServerEssentials, les comptes MicrosoftOnline Services sont appelés comptes Office 365dans les Assistants et les étiquettes du tableau de bord.  
  
5.  Suivez les instructions pour terminer l’Assistant.  
  
6.  Avertir l’utilisateur qu’ils devront modifier leur mot de passe de compte d’utilisateur avant il peut se connecter à Office 365 avec leur nouveau compte en ligne. Pour obtenir des instructions, voir [pour commencer à utiliser un compte en ligne Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
#### <a name="BKMK_ToAssignAnOnlineAccount"></a>Pour affecter un compte en ligne à un compte d’utilisateur  
  
1.  Dans le tableau de bord, cliquez sur **utilisateurs**.  
  
2.  Cliquez sur le compte d’utilisateur dans la liste, puis cliquez sur **affecter un compte en ligne Microsoft**. L’attribuer un Assistant de compte MicrosoftOnline Services s’affiche.  
  
3.  Affectez un compte en ligne existant ou créer un nouveau pour l’utilisateur. L’ID en ligne par défaut pour un nouveau compte est le nom d’utilisateur. Puis cliquez sur **suivant** pour ajouter le compte en ligne pour le compte d’utilisateur.  
  
4.  Passez en revue les informations sur la dernière page de l’Assistant, puis cliquez sur **fermer**.  
  
5.  Avertir l’utilisateur qu’ils devront modifier leur mot de passe de compte d’utilisateur avant il peut se connecter à Office 365 avec leur nouveau compte en ligne. Pour obtenir des instructions, voir [pour commencer à utiliser un compte en ligne Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
##  <a name="BKMK_SECTION_ManageOnlineAccounts"></a>Gérer les comptes en ligne  
 Lorsque vous ajoutez un compte en ligne à un compte d’utilisateur dans WindowsServerEssentials, vous pouvez gérer les deux comptes ensemble tout au long du cycle de vie du compte s.  
  
###  <a name="BKMK_UnderstandingAccountStatus"></a>Présentation de l’état du compte en ligne  
 Lorsque vous affectez un compte MicrosoftOnline Services à un compte d’utilisateur, l’adresse de messagerie pour le compte s’affiche dans le **compte en ligne Microsoft** colonne sur le **utilisateurs** page du tableau de bord. (Dans WindowsServerEssentials, l’étiquette de colonne est **compte Office 365**.)  
  
-   Une icône bleue en regard d’une adresse de messagerie indique le compte en ligne est actif. Autrement dit, le compte dispose d’une licence Office 365 en cours, et l’utilisateur peut utiliser l’ID en ligne pour se connecter à Office 365.  
  
-   Une icône grisée en regard de l’adresse de messagerie indique le compte en ligne est une section inactive, car la licence n’est plus active ou le compte en ligne a été supprimée. Lorsque vous supprimer l’affectation d’un compte utilisateur en ligne, la licence est supprimée et l’utilisateur est bloqué de se connecter à Office 365 à l’aide du compte. Toutefois, le serveur gère le mappage entre le nom de compte d’utilisateur et l’adresse de messagerie Office 365.  
  
###  <a name="BKMK_UnassignOnlineAccount"></a>Limiter l’accès à un compte en ligne  
 Que faire si un utilisateur quitte votre organisation, ou si vous voulez limiter l’accès des utilisateurs s à vos services Office 365? Si vous la gestion de vos comptes en ligne les utilisateurs avec leurs comptes d’utilisateur dans WindowsServerEssentials, vous disposez de trois options:  
  
-   **Supprimer l’affectation du compte en ligne**? Si vous souhaitez empêcher un utilisateur d’utiliser Office 365sans empêcher l’accès aux ressources sur le serveur, vous devez supprimer l’affectation du compte en ligne. La licence Office 365 est libérée et l’utilisateur est bloqué de se connecter à Office 365. Toutefois, le serveur gère le mappage entre le nom de compte d’utilisateur et l’adresse de messagerie Office 365. Pour obtenir des instructions, voir [pour annuler l’attribution d’un compte en ligne à partir d’un compte d’utilisateur](#BKMK_ToUnassignAnOnlineAccount).  
  
-   **Désactiver le compte d’utilisateur**? Si vous désactivez un compte d’utilisateur, car un employé quitte, temporairement ou définitivement, le compte utilisateur en ligne est également désactivé. Le compte en ligne ne peut pas être utilisé, mais les données utilisateur, notamment le courrier électronique, sont conservées dans MicrosoftOnline Services. Pour obtenir des instructions, voir [désactiver un compte d’utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage6) dans [gérer les comptes d’utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
-   **Supprimer le compte d’utilisateur**? Si vous supprimez un compte d’utilisateur, le compte en ligne est également supprimé de MicrosoftOnline Services. Pour obtenir des instructions, voir [supprimer un compte d’utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Remove) dans [gérer les comptes d’utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
    > [!WARNING]
    >  N’oubliez pas que lorsqu’un compte en ligne est supprimé, les données utilisateur sont soumis à des données de rétention de MicrosoftOnline Services. Si vous avez besoin de conserver les données utilisateur s après qu’un employé quitte, désactivez le compte d’utilisateur au lieu de supprimer.  
  
####  <a name="BKMK_ToUnassignAnOnlineAccount"></a>Pour annuler l’attribution d’un compte en ligne à partir d’un compte d’utilisateur  
  
1.  Dans le tableau de bord, cliquez sur **utilisateurs**.  
  
2.  Cliquez sur le compte d’utilisateur dans la liste, puis cliquez sur **annuler l’attribution d’un compte en ligne Microsoft** (dans WindowsServerEssentials, cliquez sur **annuler l’attribution d’un compte Office 365**).  
  
3.  À l’invite de confirmation, cliquez sur **Oui**.  
  
##  <a name="BKMK_SECTION_ManageEmailAddresses"></a>Gérer les adresses de messagerie pour Exchange Online  
 En ajoutant des adresses de messagerie au compte d’utilisateur s en ligne dans WindowsServerEssentials, vous pouvez autoriser l’utilisateur à recevoir des messages électroniques à plusieurs adresses de messagerie dans Exchange en ligne.  
  
###  <a name="BKMK_PROC_AddEmailAliases"></a>Pour ajouter des adresses de messagerie supplémentaires à un utilisateur s compte en ligne Microsoft  
  
1.  Dans le tableau de bord WindowsServerEssentials, cliquez sur **utilisateurs**.  
  
2.  Cliquez sur le compte d’utilisateur dans la liste, puis cliquez sur **permet d’afficher les propriétés du compte**.  
  
3.  Sur le **Microsoft en ligne** onglet des propriétés du compte (ou le **Office 365** onglet dans WindowsServerEssentials), cliquez sur **ajouter**.  
  
4.  Tapez le nouvel alias de messagerie, puis sélectionnez le domaine de messagerie.  
  
5.  Cliquez sur **OK** deux fois.  
  
##  <a name="BKMK_SECTION_ManageDistributionGroups"></a>Gérer des groupes de distribution pour Exchange Online (WindowsServerEssentials uniquement)  
 Après avoir intégré votre serveur WindowsServerEssentials à Office 365, vous pouvez créer et gérer des groupes de distribution pour Exchange Online à partir du tableau de bord WindowsServerEssentials. Vous tout cela dans le **groupes de Distribution** onglet est ajouté à la **utilisateurs** page. Vous verrez seulement cet onglet si vous disposez d’un abonnement Exchange Online. Cette fonctionnalité n’est pas disponible dans WindowsServerEssentials.  
  
 Utilisez les procédures suivantes pour:  
  
-   [Ajouter un groupe de distribution](#BKMK_PROCEDURE_AddDistGroup)  
  
-   [Modifier les membres d’un groupe de distribution](#BKMK_ChangeGroupMembers)  
  
-   [Modifier un utilisateur s distribution les membres du groupe](#BKMK_EditUserMemberships)  
  
-   [Supprimer un groupe de distribution](#BKMK_RemoveDistributionGroup)  
  
###  <a name="BKMK_PROCEDURE_AddDistGroup"></a>Pour ajouter un groupe de distribution  
  
1.  Dans le tableau de bord dans WindowsServerEssentials, cliquez sur **utilisateurs**, puis cliquez sur le **groupes de Distribution** onglet.  
  
2.  Dans **tâches groupe de Distribution**, cliquez sur **ajouter un groupe de distribution**.  
  
     L’ajout d’un Assistant Nouveau groupe de Distribution s’affiche.  
  
3.  Sur le **ajouter un nouveau groupe de distribution** page, entrez les informations suivantes, puis sur **suivant**:  
  
    -   Entrez un nom de groupe, une description facultative et alias de messagerie électronique pour le nouveau groupe de distribution.  
  
    -   Par défaut, le groupe de distribution peut recevoir des messages électroniques provenant de personnes extérieures à votre organisation. Si vous ne souhaitez pas autoriser, désactivez cette option.  
  
4.  Sur le **ajouter des membres du groupe** page, utilisez le **ajouter** bouton pour ajouter des comptes d’utilisateur actifs qui ont un compte en ligne qui leur sont affecté, et les autres groupes de distribution au nouveau groupe de distribution. Puis cliquez sur **suivant**.  
  
     Le nouveau groupe de distribution est créé dans Exchange Online.  
  
###  <a name="BKMK_ChangeGroupMembers"></a>Pour modifier les membres d’un groupe de distribution  
  
1.  Dans le tableau de bord, cliquez sur **utilisateurs**, puis cliquez sur le **groupes de Distribution** onglet.  
  
2.  Cliquez sur le groupe de distribution dans la liste, puis cliquez sur **modifier l’appartenance au groupe**.  
  
3.  Utilisez le **ajouter** et **supprimer** boutons pour ajouter ou supprimer des comptes en ligne actifs dans le groupe de distribution. Puis cliquez sur **suivant** pour mettre à jour l’appartenance au groupe de distribution dans Exchange Online.  
  
###  <a name="BKMK_EditUserMemberships"></a>Pour modifier un s distribution appartenance au groupe  
  
1.  Dans le tableau de bord, cliquez sur **utilisateurs**.  
  
2.  Cliquez sur le compte d’utilisateur dans la liste, puis cliquez sur **permet d’afficher les propriétés du compte**.  
  
3.  Dans les propriétés du compte utilisateur, cliquez sur le **groupes de Distribution** onglet, puis cliquez sur **modifier**.  
  
4.  Dans le **modifier l’appartenance au groupe**, utilisez le **ajouter** et **supprimer** boutons pour ajouter ou supprimer des groupes de distribution à partir du compte d’utilisateur, puis cliquez sur **fermer**.  
  
5.  Cliquez sur **OK** pour enregistrer des propriétés du compte de l’utilisateur mis à jour.  
  
###  <a name="BKMK_RemoveDistributionGroup"></a>Pour supprimer un groupe de distribution  
  
1.  Dans le tableau de bord, cliquez sur **utilisateurs**, puis cliquez sur le **groupes de Distribution** onglet.  
  
2.  Cliquez sur le groupe de distribution dans la liste, puis cliquez sur **supprimer le groupe**.  
  
3.  À l’invite de confirmation, cliquez sur **supprimer le groupe**.  
  
     Le groupe de distribution est supprimé d’Exchange Online.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer les comptes d’utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md)  
  
-   [Gérer Office 365](Manage-Office-365-in-Windows-Server-Essentials.md)  
  
-   [Gérer les Services en ligne Microsoft](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)
