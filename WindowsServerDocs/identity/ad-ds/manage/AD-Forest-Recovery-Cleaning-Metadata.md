---
title: Récupération de forêt AD - nettoyage des métadonnées des contrôleurs de domaine supprimé
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: e7543381-4081-407f-adad-a9de792c6616
ms.technology: identity-adds
ms.openlocfilehash: b71cab51a362a96ab6071e5eed3cf31c4421041c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843040"
---
# <a name="ad-forest-recovery---cleaning-metadata-of-removed-writable-domain-controllers"></a>Récupération de forêt AD - nettoyage des métadonnées des contrôleurs de domaine accessible en écriture supprimés

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Nettoyage des métadonnées supprime les données Active Directory identifiant un contrôleur de domaine pour le système de réplication.  

Utilisez la procédure suivante pour supprimer les objets de contrôleur de domaine pour les contrôleurs de domaine que vous souhaitez ajouter au réseau en réinstallant les services AD DS.  
  
Si vous utilisez la version d’Active Directory Users and Computers, ou les Sites Active Directory et les Services qui est inclus les outils d’Administration de serveur distant (RSAT), le nettoyage des métadonnées est effectué automatiquement lorsque vous supprimez un objet de contrôleur de domaine.  

## <a name="deleting-a-domain-controller-using-active-directory-users-and-computers"></a>Suppression d’un contrôleur de domaine à l’aide d’Active Directory Users and Computers

Lorsque vous utilisez la version d’Active Directory utilisateurs et ordinateurs ou centre d’administration Active Directory dans Server Administration outils distant (RSAT), le nettoyage des métadonnées est effectué automatiquement lorsque vous supprimez l’objet contrôleur de domaine. L’objet serveur et l’objet ordinateur sont également supprimés automatiquement.  

Comme alternative, vous pouvez également utiliser les Services et les Sites Active Directory dans RSAT pour supprimer un objet de contrôleur de domaine. Si vous utilisez des Services et Sites Active Directory, vous devez supprimer l’objet de serveur associé et d’un objet Paramètres NTDS avant de pouvoir supprimer l’objet contrôleur de domaine.  

Pour plus d’informations sur l’installation de serveur distant, consultez l’article [outils d’Administration de serveur distant](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools).
  
La procédure suivante est la même pour les contrôleurs de domaine qui exécutent soit Windows Server 2016, 2012, 2008 R2 ou 2008. La contrôleur de domaine cible de l’opération de nettoyage des métadonnées peut exécuter n’importe quelle version de Windows Server.  
  
### <a name="to-delete-a-domain-controller-object-using-active-directory-users-and-computers-in-rsat"></a>Pour supprimer un objet de contrôleur de domaine à l’aide d’Active Directory Users and Computers dans RSAT  
  
1. Cliquez sur **Démarrer**, **Outils d'administration**, puis sur **Utilisateurs et ordinateurs Active Directory**.  
2. Dans l’arborescence de la console, double-cliquez sur le conteneur de domaine, puis double-cliquez sur le **contrôleurs de domaine** unité d’organisation (UO).  
3. Dans le volet de détails, cliquez sur le contrôleur de domaine que vous souhaitez supprimer, puis cliquez sur **supprimer**.
   ![Supprimer](media/AD-Forest-Recovery-Cleaning-Metadata/delete1.png) 
4. Cliquez sur **Oui** pour confirmer la suppression. Sélectionnez le **ce contrôleur de domaine est définitivement hors connexion et ne peut plus être rétrogradé à l’aide de l’Assistant Installation Services Directory domaine (DCPROMO) Active** case à cocher et cliquez sur **supprimer**.  
5. Si le contrôleur de domaine était un serveur de catalogue global, cliquez sur **Oui** Vérifiez que la suppression.  

## <a name="next-steps"></a>Étapes suivantes

- [Guide de la forêt Active Directory](AD-Forest-Recovery-Guide.md)
- [Récupération de forêt AD - procédures](AD-Forest-Recovery-Procedures.md)
