---
title: Nettoyer les métadonnées du serveur AD DS
description: Utiliser les outils intégrés pour nettoyer les métadonnées des contrôleurs de domaine supprimés
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 11/14/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e8adbaf07976569fdea86156e15f246aad2e4fe0
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868241"
---
# <a name="clean-up-active-directory-domain-controller-server-metadata"></a>Nettoyer les métadonnées du serveur de contrôleur domaine Active Directory

S'applique à : Windows Server

Le nettoyage des métadonnées est une procédure obligatoire après une suppression forcée de Active Directory Domain Services (AD DS). Vous effectuez un nettoyage des métadonnées sur un contrôleur de domaine dans le domaine du contrôleur de domaine que vous avez supprimé de force. Le nettoyage des métadonnées supprime les données de AD DS qui identifie un contrôleur de domaine auprès du système de réplication. Le nettoyage des métadonnées supprime également les connexions de réplication du service de réplication de fichiers (FRS) et de système de fichiers DFS (DFS) et tente de transférer ou de prendre des rôles de maître d’opérations (également appelés opérations à maître unique flottant ou FSMO) que le domaine retiré le contrôleur est en bloc.

Il existe trois options pour nettoyer les métadonnées du serveur :

- Nettoyer les métadonnées du serveur à l’aide des outils d’interface utilisateur graphique
- Nettoyer les métadonnées du serveur à l’aide de la ligne de commande
- Nettoyer les métadonnées du serveur à l’aide d’un script

> [!NOTE]
> Si vous recevez une erreur « Accès refusé » lorsque vous utilisez l’une de ces méthodes pour effectuer un nettoyage des métadonnées, assurez-vous que l’objet ordinateur et l’objet Paramètres NTDS pour le contrôleur de domaine ne sont pas protégés contre les suppressions accidentelles. Pour vérifier cela, cliquez avec le bouton droit sur l’objet ordinateur ou sur l’objet Paramètres NTDS, cliquez sur **Propriétés**, sur **objet**, puis désactivez la case à cocher **protéger l’objet des suppressions accidentelles** . Dans Active Directory utilisateurs et ordinateurs, l’onglet **objet** d’un objet s’affiche si vous cliquez sur **affichage** , puis sur **fonctionnalités avancées**.

## <a name="clean-up-server-metadata-using-gui-tools"></a>Nettoyer les métadonnées du serveur à l’aide des outils d’interface utilisateur graphique

Lorsque vous utilisez Outils d’administration de serveur distant (RSAT) ou la console utilisateurs et ordinateurs Active Directory (DSA. msc) qui est incluse dans Windows Server pour supprimer un compte d’ordinateur de contrôleur de domaine de l’unité d’organisation des contrôleurs de domaine, le le nettoyage des métadonnées du serveur s’effectue automatiquement. Avant Windows Server 2008, vous deviez effectuer une procédure distincte de nettoyage des métadonnées.

Vous pouvez également utiliser la console sites et services Active Directory (Dssite. msc) pour supprimer le compte d’ordinateur d’un contrôleur de domaine, ce qui complète automatiquement le nettoyage des métadonnées. Toutefois, Active Directory sites et services ne supprime les métadonnées automatiquement que lorsque vous supprimez d’abord l’objet Paramètres NTDS sous le compte d’ordinateur dans Dssite. msc.

Tant que vous utilisez Windows Server 2008 ou des versions ultérieures de RSAT de DSA. msc ou Dssite. msc, vous pouvez nettoyer automatiquement les métadonnées pour les contrôleurs de domaine exécutant des versions antérieures des systèmes d’exploitation Windows.

Pour effectuer ces procédures, il est nécessaire d’appartenir au minimum au **groupe Admins du domaine**ou à un groupe équivalent.

## <a name="clean-up-server-metadata-using-activedirectory-users-and-computers"></a>Nettoyer les métadonnées du serveur à l’aide d’utilisateurs et d’ordinateurs Active Directory

1. Ouvrez **Utilisateurs et ordinateurs Active Directory**.
2. Si vous avez identifié des partenaires de réplication en préparation pour cette procédure et si vous n’êtes pas connecté à un partenaire de réplication du contrôleur de domaine supprimé dont vous nettoyez les métadonnées, cliquez avec le bouton droit sur **Active Directory nœud utilisateurs et ordinateurs** . , puis cliquez sur **modifier le contrôleur de domaine**. Cliquez sur le nom du contrôleur de domaine à partir duquel vous souhaitez supprimer les métadonnées, puis cliquez sur **OK**.
3. Développez le domaine du contrôleur de domaine qui a été supprimé de force, puis cliquez sur **contrôleurs de domaine**.
4. Dans le volet d’informations, cliquez avec le bouton droit sur l’objet ordinateur du contrôleur de domaine dont vous souhaitez nettoyer les métadonnées, puis cliquez sur **supprimer**.
5. Dans la boîte de dialogue **Active Directory Domain Services** , confirmez que le nom du contrôleur de domaine que vous souhaitez supprimer s’affiche, puis cliquez sur **Oui** pour confirmer la suppression de l’objet ordinateur.
6. Dans la boîte de dialogue suppression d’un **contrôleur de domaine** , sélectionnez **ce contrôleur de domaine est définitivement hors connexion et ne peut plus être rétrogradé à l’aide du Assistant Installation Active Directory Domain Services (dcpromo)** , puis cliquez sur **supprimer**.
7. Si le contrôleur de domaine est un serveur de catalogue global, dans la boîte de dialogue **supprimer le contrôleur de domaine** , cliquez sur **Oui** pour poursuivre la suppression.
8. Si le contrôleur de domaine contient actuellement un ou plusieurs rôles de maître d’opérations, cliquez sur **OK** pour déplacer le ou les rôles vers le contrôleur de domaine indiqué. Vous ne pouvez pas modifier ce contrôleur de domaine. Si vous souhaitez déplacer le rôle vers un autre contrôleur de domaine, vous devez déplacer le rôle après avoir terminé la procédure de nettoyage des métadonnées du serveur.

## <a name="clean-up-server-metadata-using-activedirectory-sites-and-services"></a>Nettoyer les métadonnées du serveur à l’aide de sites et services Active Directory

1. Ouvrez le composant Sites et services Active Directory.
2. Si vous avez identifié des partenaires de réplication en préparation pour cette procédure et si vous n’êtes pas connecté à un partenaire de réplication du contrôleur de domaine supprimé dont vous nettoyez les métadonnées, cliquez avec le bouton droit sur **Active Directory sites et services**, puis Cliquez ensuite sur **modifier le contrôleur de domaine**. Cliquez sur le nom du contrôleur de domaine à partir duquel vous souhaitez supprimer les métadonnées, puis cliquez sur **OK**.
3. Développez le site du contrôleur de domaine qui a été supprimé de force, développez **serveurs**, développez le nom du contrôleur de domaine, cliquez avec le bouton droit sur l’objet Paramètres NTDS, puis cliquez sur **supprimer**.
4. Dans la boîte de dialogue **Active Directory les sites et services** , cliquez sur **Oui** pour confirmer la suppression des paramètres NTDS.
5. Dans la boîte de dialogue suppression d’un **contrôleur de domaine** , sélectionnez **ce contrôleur de domaine est définitivement hors connexion et ne peut plus être rétrogradé à l’aide du Assistant Installation Active Directory Domain Services (dcpromo)** , puis cliquez sur **supprimer**.
6. Si le contrôleur de domaine est un serveur de catalogue global, dans la boîte de dialogue **supprimer le contrôleur de domaine** , cliquez sur **Oui** pour poursuivre la suppression.
7. Si le contrôleur de domaine contient actuellement un ou plusieurs rôles de maître d’opérations, cliquez sur **OK** pour déplacer le ou les rôles vers le contrôleur de domaine indiqué.
8. Cliquez avec le bouton droit sur le contrôleur de domaine qui a été supprimé de force, puis cliquez sur supprimer.
9. Dans la boîte de dialogue **Active Directory Domain Services** , cliquez sur **Oui** pour confirmer la suppression du contrôleur de domaine.

## <a name="clean-up-server-metadata-using-the-command-line"></a>Nettoyer les métadonnées du serveur à l’aide de la ligne de commande

Vous pouvez également nettoyer les métadonnées à l’aide de Ntdsutil. exe, un outil de ligne de commande qui est installé automatiquement sur tous les contrôleurs de domaine et les serveurs sur lesquels services AD LDS (Active Directory Lightweight Directory Services) (AD LDS) est installé. Ntdsutil. exe est également disponible sur les ordinateurs sur lesquels RSAT est installé.

## <a name="to-clean-up-server-metadata-by-using-ntdsutil"></a>Pour nettoyer les métadonnées du serveur à l’aide de Ntdsutil

1. Ouvrez une invite de commandes en tant qu’administrateur : Dans le menu **Démarrer** , cliquez avec le bouton droit sur **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**. Si la boîte de dialogue **contrôle de compte d’utilisateur** s’affiche, fournissez les informations d’identification d’un administrateur d’entreprise si nécessaire, puis cliquez sur **Continuer**.
2. À l'invite de commandes, tapez la commande suivante et appuyez sur Entrée :

   `ntdsutil`

3. À l’invite de commandes `ntdsutil:`, tapez la commande suivante, puis appuyez sur Entrée :

   `metadata cleanup`

4. À l’invite de commandes `metadata cleanup:`, tapez la commande suivante, puis appuyez sur Entrée :

   `remove selected server <ServerName>`

5. Dans la **boîte de dialogue Supprimer la configuration du serveur**, passez en revue les informations et les avertissements, puis cliquez sur **Oui** pour supprimer l’objet serveur et les métadonnées.

   À ce stade, Ntdsutil confirme que le contrôleur de domaine a été correctement supprimé. Si vous recevez un message d’erreur indiquant que l’objet est introuvable, cela signifie peut-être que le contrôleur de domaine a été supprimé précédemment.

6. À l' `metadata cleanup:` invite `ntdsutil:` de commandes et, `quit`tapez, puis appuyez sur entrée.

7. Pour confirmer la suppression du contrôleur de domaine :

   Ouvrez Active Directory utilisateurs et ordinateurs. Dans le domaine du contrôleur de domaine supprimé, cliquez sur **contrôleurs de domaine**. Dans le volet d’informations, un objet pour le contrôleur de domaine que vous avez supprimé ne doit pas apparaître.

   Ouvrez sites et services Active Directory. Accédez au conteneur **serveurs** et vérifiez que l’objet serveur pour le contrôleur de domaine que vous avez supprimé ne contient pas d’objet Paramètres NTDS. Si aucun objet enfant n’apparaît sous l’objet serveur, vous pouvez supprimer l’objet serveur. Si un objet enfant s’affiche, ne supprimez pas l’objet serveur, car une autre application utilise l’objet.

## <a name="see-also"></a>Voir aussi

* [Rétrogradation de contrôleurs de domaine](Demoting-Domain-Controllers-and-Domains--Level-200-.md)
* [Informations de référence sur les commandes Ntdsutil](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753343(v=ws.10))
