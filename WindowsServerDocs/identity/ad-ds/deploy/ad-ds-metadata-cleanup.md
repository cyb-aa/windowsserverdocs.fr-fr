---
title: Nettoyer les métadonnées de serveur AD DS
description: Utiliser des outils intégrés pour nettoyer les métadonnées à partir de contrôleurs de domaine supprimés
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 11/14/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: fbb6e720c9289c608d71d3c36695ba623a9df5f6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818080"
---
# <a name="clean-up-active-directory-domain-controller-server-metadata"></a>Nettoyer les métadonnées de serveur de contrôleur de domaine Active Directory

S'applique à : Windows Server

Nettoyage des métadonnées est une procédure requise après une suppression forcée des Services de domaine Active Directory (AD DS). Vous effectuez le nettoyage des métadonnées sur un contrôleur de domaine dans le domaine du contrôleur de domaine que vous avez dû être supprimé. Nettoyage des métadonnées supprime les données à partir d’AD DS qui identifie un contrôleur de domaine pour le système de réplication. Nettoyage des métadonnées également supprime les connexions de Service de réplication de fichiers (FRS) et la réplication de système de fichiers distribués (DFS) et tente de transférer ou prendre les rôles de maître (également appelés opérations à maître unique ou FSMO) opérations que le domaine mis hors service contrôleur contient.

Il existe trois options pour nettoyer les métadonnées du serveur :

- Nettoyer les métadonnées de serveur à l’aide des outils de l’interface graphique utilisateur
- Nettoyer les métadonnées de serveur à l’aide de la ligne de commande
- Nettoyer les métadonnées de serveur à l’aide d’un script

> [!NOTE]
> Si vous recevez une erreur « Accès refusé » lorsque vous utilisez une de ces méthodes pour effectuer le nettoyage des métadonnées, assurez-vous que l’objet ordinateur et l’objet Paramètres NTDS pour le contrôleur de domaine ne sont pas protégées contre une suppression accidentelle. Pour vérifier ce avec le bouton droit de l’objet ordinateur ou l’objet Paramètres NTDS, cliquez sur **propriétés**, cliquez sur **objet**, désactivez le **protéger l’objet des suppressions accidentelles** case à cocher. Dans Active Directory utilisateurs et ordinateurs, le **objet** onglet d’un objet s’affiche si vous cliquez sur **vue** puis cliquez sur **fonctionnalités avancées**.

## <a name="clean-up-server-metadata-using-gui-tools"></a>Nettoyer les métadonnées de serveur à l’aide des outils de l’interface graphique utilisateur

Lorsque vous utilisez des outils d’Administration de serveur distant (RSAT) ou l’Active Directory console Utilisateurs et ordinateurs (DSA.msc) qui est inclus avec Windows Server pour supprimer un compte ordinateur de contrôleur de domaine à partir de l’unité d’organisation contrôleurs de domaine (UO), le nettoyage des métadonnées du serveur est effectué automatiquement. Avant Windows Server 2008, vous deviez effectuer une procédure de nettoyage des métadonnées distinctes.

Vous pouvez également utiliser la console (Dssite.msc) pour supprimer le compte d’ordinateur d’un contrôleur de domaine, les Sites Active Directory et les Services qui se termine également le nettoyage des métadonnées automatiquement. Toutefois, les Services et les Sites Active Directory supprime les métadonnées automatiquement uniquement lorsque vous supprimez tout d’abord l’objet Paramètres NTDS sous le compte d’ordinateur dans Dssite.msc.

Tant que vous utilisez le Windows Server 2008 ou versions plus récentes de RSAT de « DSA.msc » ou « dssite.msc », vous pouvez nettoyer les métadonnées automatiquement pour les contrôleurs de domaine exécutant des versions antérieures des systèmes d’exploitation de Windows.

L’appartenance au **Admins du domaine**, ou équivalente, est la condition minimale requise pour réaliser ces procédures.

## <a name="clean-up-server-metadata-using-activedirectory-users-and-computers"></a>Nettoyer les métadonnées de serveur à l’aide d’Active Directory Users and Computers

1. Ouvrez **Utilisateurs et ordinateurs Active Directory**.
2. Si vous avez identifié les partenaires de réplication en préparation de cette procédure et si vous n’êtes pas connecté à un partenaire de réplication du contrôleur de domaine supprimé dont les métadonnées que vous nettoyez, cliquez sur **les utilisateurs Active Directory et Ordinateurs** nœud, puis cliquez sur **modifier le contrôleur de domaine**. Cliquez sur le nom du contrôleur de domaine à partir duquel vous souhaitez supprimer les métadonnées, puis cliquez sur **OK**.
3. Développez le domaine du contrôleur de domaine qui a été conservée, puis cliquez sur **contrôleurs de domaine**.
4. Dans le volet de détails, cliquez sur l’objet ordinateur du contrôleur de domaine dont vous souhaitez nettoyer, puis cliquez sur les métadonnées **supprimer**.
5. Dans le **Active Directory Domain Services** boîte de dialogue zone, vérifiez le nom du contrôleur de domaine que vous souhaitez supprimer est indiqué, cliquez sur **Oui** pour confirmer la suppression de l’objet ordinateur.
6. Dans le **suppression d’un contrôleur de domaine** boîte de dialogue, sélectionnez **ce contrôleur de domaine est définitivement hors connexion et ne peut plus être rétrogradé à l’aide de l’Active Directory domaine Services Assistant Installation (DCPROMO)**, puis cliquez sur **supprimer**.
7. Si le contrôleur de domaine est un serveur de catalogue global, dans le **supprimer un contrôleur de domaine** boîte de dialogue, cliquez sur **Oui** pour poursuivre la suppression.
8. Si le contrôleur de domaine détient actuellement une ou plusieurs opérations des rôles de maître, cliquez sur **OK** pour déplacer l’ou les rôles au contrôleur de domaine qui s’affiche. Vous ne pouvez pas modifier ce contrôleur de domaine. Si vous souhaitez déplacer le rôle vers un autre contrôleur de domaine, vous devez déplacer le rôle après avoir terminé la procédure de nettoyage des métadonnées de serveur.

## <a name="clean-up-server-metadata-using-activedirectory-sites-and-services"></a>Nettoyer les métadonnées de serveur à l’aide des Services et Sites Active Directory

1. Ouvrez le composant Sites et services Active Directory.
2. Si vous avez identifié les partenaires de réplication en préparation de cette procédure et si vous n’êtes pas connecté à un partenaire de réplication du contrôleur de domaine supprimé dont les métadonnées que vous nettoyez, cliquez sur **Sites Active Directory et des Services** , puis cliquez sur **modifier le contrôleur de domaine**. Cliquez sur le nom du contrôleur de domaine à partir duquel vous souhaitez supprimer les métadonnées, puis cliquez sur **OK**.
3. Développez le site du contrôleur de domaine qui a été conservée, **serveurs**, développez le nom du contrôleur de domaine, cliquez sur l’objet Paramètres NTDS, puis cliquez sur **supprimer**.
4. Dans le **Sites Active Directory et Services** boîte de dialogue, cliquez sur **Oui** pour confirmer la suppression de paramètres NTDS.
5. Dans le **suppression d’un contrôleur de domaine** boîte de dialogue, sélectionnez **ce contrôleur de domaine est définitivement hors connexion et ne peut plus être rétrogradé à l’aide de l’Active Directory domaine Services Assistant Installation (DCPROMO)**, puis cliquez sur **supprimer**.
6. Si le contrôleur de domaine est un serveur de catalogue global, dans le **supprimer un contrôleur de domaine** boîte de dialogue, cliquez sur **Oui** pour poursuivre la suppression.
7. Si le contrôleur de domaine détient actuellement une ou plusieurs opérations des rôles de maître, cliquez sur **OK** pour déplacer l’ou les rôles au contrôleur de domaine qui s’affiche.
8. Cliquez sur le contrôleur de domaine qui a été conservée et puis cliquez sur Supprimer.
9. Dans le **Active Directory Domain Services** boîte de dialogue, cliquez sur **Oui** pour confirmer la suppression de contrôleur de domaine.

## <a name="clean-up-server-metadata-using-the-command-line"></a>Nettoyer les métadonnées de serveur à l’aide de la ligne de commande

Comme alternative, vous pouvez nettoyer les métadonnées à l’aide de Ntdsutil.exe, un outil de ligne de commande qui est installé automatiquement sur tous les contrôleurs de domaine et les serveurs disposant d’Active Directory Lightweight Directory Services (AD LDS) installé. Ntdsutil.exe est également disponible sur les ordinateurs qui ont installé de serveur distant.

## <a name="to-clean-up-server-metadata-by-using-ntdsutil"></a>Pour nettoyer les métadonnées du serveur à l’aide de Ntdsutil

1. Ouvrez une invite de commandes en tant qu’administrateur : Sur le **Démarrer** menu, avec le bouton droit **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**. Si le **contrôle de compte d’utilisateur** boîte de dialogue s’affiche, fournissez les informations d’identification d’administrateur d’entreprise si nécessaire, puis cliquez sur **continuer**.
2. À l'invite de commandes, tapez la commande suivante et appuyez sur Entrée :

   `ntdsutil`

3. À l’invite de commandes `ntdsutil:`, tapez la commande suivante, puis appuyez sur Entrée :

   `metadata cleanup`

4. À l’invite de commandes `metadata cleanup:`, tapez la commande suivante, puis appuyez sur Entrée :

   `remove selected server <ServerName>`

5. Dans **boîte de dialogue serveur supprimer Configuration**, passez en revue les informations et un avertissement, puis cliquez sur **Oui** pour supprimer l’objet serveur et les métadonnées.

   À ce stade, Ntdsutil confirme que le contrôleur de domaine a été supprimé avec succès. Si vous recevez un message d’erreur qui indique que l’objet est introuvable, le contrôleur de domaine a peut-être été supprimé précédemment.

6. À la `metadata cleanup:` et `ntdsutil:` invites, tapez `quit`, puis appuyez sur ENTRÉE.

7. Pour confirmer la suppression du contrôleur de domaine :

   Ouvrez Utilisateurs et ordinateurs Active Directory. Dans le domaine du contrôleur de domaine supprimé, cliquez sur **contrôleurs de domaine**. Dans le volet de détails, un objet du contrôleur de domaine que vous avez supprimé ne doit pas apparaître.

   Ouvrez Sites et Services Active Directory. Accédez à la **serveurs** conteneur et vérifiez que l’objet serveur du contrôleur de domaine que vous avez supprimé ne contient pas d’un objet Paramètres NTDS. Si aucun objet enfant n’apparaître sous l’objet serveur, vous pouvez supprimer l’objet serveur. Si un objet enfant s’affiche, ne supprimez pas l’objet serveur, car une autre application à l’aide de l’objet.

## <a name="see-also"></a>Voir aussi

* [Rétrogradation de contrôleurs de domaine](Demoting-Domain-Controllers-and-Domains--Level-200-.md)
* [Référence de commande de Ntdsutil](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753343(v=ws.10))
