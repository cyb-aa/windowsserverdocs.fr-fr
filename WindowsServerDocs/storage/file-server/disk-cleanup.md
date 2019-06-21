---
title: À l’aide de nettoyage de disque sur Windows Server
description: Découvrez comment utiliser les options de ligne de commande pour configurer l’outil de nettoyage de disque (Cleanmgr.exe) pour nettoyer automatiquement les certains fichiers.
ms.prod: windows-server-threshold
ms.reviewer: cosmosdarwin
author: iangpgh
ms.author: jgerend
manager: daveba
ms.technology: storage-spaces
ms.date: 06/20/2019
ms.openlocfilehash: b479697366239144e5ca9d3486b84191eb51dc4d
ms.sourcegitcommit: 078304c4b92bb57eb85ba29634afc92cc028c644
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67301602"
---
# <a name="using-disk-cleanup-on-windows-server"></a>À l’aide de nettoyage de disque sur Windows Server

> S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

L’outil de nettoyage de disque supprime les fichiers inutiles dans un environnement Windows Server. Cet outil est disponible par défaut sur Windows Server 2019 et Windows Server 2016, mais vous devrez peut-être effectuer quelques étapes manuelles pour l’activer sur les versions antérieures de Windows Server.

Pour démarrer l’outil de nettoyage de disque, exécutez la commande Cleanmgr.exe, ou sélectionnez **Démarrer**, sélectionnez **les outils d’administration Windows**, puis sélectionnez **nettoyage de disque**.

Vous pouvez également exécuter le nettoyage de disque à l’aide de la [cleanmgr commande de Windows](../../administration/windows-commands/clean-mgr.md) et utilisez les options de ligne de commande pour spécifier que le nettoyage de disque nettoie certains fichiers.

## <a name="enable-disk-cleanup-on-an-earlier-version-of-windows-server-by-installing-the-desktop-experience"></a>Activer le nettoyage de disque sur une version antérieure de Windows Server en installant la fonctionnalité expérience utilisateur

Suivez ces étapes pour utiliser l’ajout de rôles et fonctionnalités Assistant pour installer la fonctionnalité expérience utilisateur sur un serveur exécutant Windows Server 2012 R2 ou version antérieure, qui installe également le nettoyage de disque.

1. Si le Gestionnaire de serveur est déjà ouvert, passez à l’étape suivante. S’il n’est pas déjà ouvert, ouvrez-le en effectuant l’une des opérations suivantes.

   - Sur le Bureau Windows, démarrez le Gestionnaire de serveur en cliquant sur **Gestionnaire de serveur** dans la barre des tâches Windows.

   - Accédez à **Démarrer** et sélectionnez la vignette du Gestionnaire de serveur.

1. Sur le **gérer** menu, sélectionnez Ajouter **des rôles et fonctionnalités**.

1. Sur le **avant de commencer** page, vérifiez que votre serveur de destination et l’environnement réseau sont préparés pour la fonctionnalité que vous souhaitez installer. Sélectionnez **Suivant**.

1. Sur le **sélectionner type d’installation** , sélectionnez **installation en fonction du rôle ou une fonctionnalité** pour installer toutes les fonctionnalités WebPart sur un seul serveur. Sélectionnez **Suivant**.

1. Dans la page **Sélectionner le serveur de destination**, sélectionnez un serveur dans le pool de serveurs ou sélectionnez un disque dur virtuel hors connexion. Sélectionnez **Suivant**.

1. Sur le **sélectionner des rôles de serveur** , sélectionnez **suivant**.

1. Sur le **sélectionner des fonctionnalités** , sélectionnez **Interface utilisateur et Infrastructure**, puis sélectionnez **expérience**.

1. Dans **ajouter des fonctionnalités qui sont requises pour la fonctionnalité expérience utilisateur ?** , sélectionnez **ajouter des fonctionnalités**.

1. Poursuivre l’installation, puis redémarrez le système.

1. Vérifiez que le **nettoyage de disque** case d’option apparaît dans la boîte de dialogue de propriétés.

   ![Boîte de dialogue de propriétés avec l’option de nettoyage de disque du disque](media/diskpropswcleanup.png)

## <a name="manually-add-disk-cleanup-to-an-earlier-version-of-windows-server"></a>Ajouter manuellement le nettoyage de disque à une version antérieure de Windows Server

L’outil de nettoyage de disque (cleanmgr.exe) n’est pas présent sur Windows Server 2012 R2 ou version antérieure, sauf si vous disposez de la fonctionnalité Expérience Bureau installée.

Pour utiliser cleanmgr.exe, installez la fonctionnalité expérience utilisateur comme décrit précédemment, ou copier deux fichiers sont déjà présents sur le serveur, de cleanmgr.exe et de cleanmgr.exe.mui. Utilisez le tableau suivant pour localiser les fichiers de votre système d’exploitation.

| Système d’exploitation  | Architecture  | Emplacement du fichier  |
| ----------------- | -------------- | --------------- |
| Windows Server 2008 R2 | 64 bits | C:\Windows\winsxs\amd64_microsoft-windows-cleanmgr_31bf3856ad364e35_6.1.7600.16385_none_c9392808773cd7da\cleanmgr.exe 
| Windows Server 2008 R2 | 64 bits | C:\Windows\winsxs\amd64_microsoft-windows-cleanmgr.resources_31bf3856ad364e35_6.1.7600.16385_en-us_b9cb6194b257cc63\cleanmgr.exe.mui |

Recherchez cleanmgr.exe et déplacez le fichier vers **%systemroot%\System32**.

Recherchez cleanmgr.exe.mui et déplacer les fichiers **%systemroot%\System32\en-US**.

Vous pouvez maintenant lancer l’outil de nettoyage de disque en exécutant Cleanmgr.exe à partir de l’invite de commandes, ou en cliquant sur **Démarrer** et en tapant **Cleanmgr** dans la barre de recherche.

Pour que le bouton de nettoyage de disque apparaissent dans la boîte de dialogue Propriétés d’un disque, vous devez également installer la fonctionnalité expérience utilisateur.

## <a name="additional-references"></a>Références supplémentaires

[Libérer de l’espace disque dans Windows 10](https://support.microsoft.com/en-us/help/12425/windows-10-free-up-drive-space)

[cleanmgr](../../administration/windows-commands/clean-mgr.md)
