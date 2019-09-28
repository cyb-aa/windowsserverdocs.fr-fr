---
title: Récupération de la forêt Active Directory-nettoyage des métadonnées des contrôleurs de domaine supprimés
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: e7543381-4081-407f-adad-a9de792c6616
ms.technology: identity-adds
ms.openlocfilehash: cc41170051e55fbaeca048ac587ecd3351cd53ad
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369267"
---
# <a name="ad-forest-recovery---cleaning-metadata-of-removed-writable-domain-controllers"></a>Récupération de la forêt Active Directory-nettoyage des métadonnées des contrôleurs de domaine accessibles en écriture supprimés

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Le nettoyage des métadonnées supprime Active Directory données qui identifient un contrôleur de réseau dans le système de réplication.  

Utilisez la procédure suivante pour supprimer les objets DC pour les contrôleurs de réseau que vous envisagez de rajouter au réseau en réinstallant AD DS.  
  
Si vous utilisez la version de Active Directory utilisateurs et ordinateurs ou Active Directory sites et services inclus Outils d’administration de serveur distant (RSAT), le nettoyage des métadonnées s’effectue automatiquement lorsque vous supprimez un objet DC.  

## <a name="deleting-a-domain-controller-using-active-directory-users-and-computers"></a>Suppression d’un contrôleur de domaine à l’aide d’utilisateurs et d’ordinateurs Active Directory

Lorsque vous utilisez la version de Active Directory utilisateurs et ordinateurs ou Centre d’administration Active Directory dans Outils d’administration de serveur distant (RSAT), le nettoyage des métadonnées s’effectue automatiquement lorsque vous supprimez l’objet DC. L’objet serveur et l’objet ordinateur sont également supprimés automatiquement.  

Vous pouvez également utiliser Active Directory sites et services dans les outils d’aide à la fois pour supprimer un objet DC. Si vous utilisez des sites et des services Active Directory, vous devez supprimer l’objet serveur et les paramètres NTDS associés avant de pouvoir supprimer l’objet DC.  

Pour plus d’informations sur l’installation de RSAT, consultez l’article [Outils d’administration de serveur distant](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools).
  
La procédure suivante est la même pour les contrôleurs de code qui exécutent Windows Server 2016, 2012, 2008 R2 ou 2008. Le contrôleur de service cible de l’opération de nettoyage des métadonnées peut exécuter n’importe quelle version de Windows Server.  
  
### <a name="to-delete-a-domain-controller-object-using-active-directory-users-and-computers-in-rsat"></a>Pour supprimer un objet de contrôleur de domaine à l’aide d’Active Directory utilisateurs et ordinateurs dans RSAT  
  
1. Cliquez sur **Démarrer**, **Outils d'administration**, puis sur **Utilisateurs et ordinateurs Active Directory**.  
2. Dans l’arborescence de la console, double-cliquez sur le conteneur de domaine, puis double-cliquez sur l’unité d’organisation des **contrôleurs de domaine** .  
3. Dans le volet d’informations, cliquez avec le bouton droit sur le contrôleur de périphérique que vous souhaitez supprimer, puis cliquez sur **supprimer**.
   ![Supprimer](media/AD-Forest-Recovery-Cleaning-Metadata/delete1.png) 
4. Cliquez sur **Oui** pour confirmer la suppression. Sélectionnez le **contrôleur de domaine est définitivement hors connexion et ne peut plus être rétrogradé à l’aide de la case à cocher Assistant Installation Active Directory Domain Services (dcpromo)** , puis cliquez sur **supprimer**.  
5. Si le contrôleur de périphérique était un serveur de catalogue global, cliquez sur **Oui** pour confirmer la suppression.  

## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de la forêt Active Directory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt Active Directory : procédures](AD-Forest-Recovery-Procedures.md)
