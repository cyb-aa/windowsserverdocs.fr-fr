---
title: Utilisation de l’outil Nettoyage de disque sur Windows Server
description: Découvrez comment utiliser les options de ligne de commande pour configurer l’outil Nettoyage de disque (Cleanmgr.exe) pour nettoyer automatiquement certains fichiers.
ms.prod: windows-server
ms.reviewer: cosmosdarwin
author: iangpgh
ms.author: jgerend
manager: daveba
ms.technology: storage-spaces
ms.date: 06/20/2019
ms.openlocfilehash: bb93ec15fd138ee65797c9d27413552c3a1759a6
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949670"
---
# <a name="using-disk-cleanup-on-windows-server"></a>Utilisation de l’outil Nettoyage de disque sur Windows Server

> S'applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

L’outil Nettoyage de disque permet d’effacer les fichiers inutiles dans un environnement Windows Server. Cet outil est disponible par défaut sur Windows Server 2019 et Windows Server 2016, mais vous devrez peut-être effectuer quelques étapes manuelles pour l’activer sur les versions antérieures de Windows Server.

Pour démarrer l’outil Nettoyage de disque, exécutez la commande Cleanmgr.exe ou sélectionnez **Démarrer**, **Outils d’administration Windows**, puis **Nettoyage de disque**.

Vous pouvez également exécuter l’outil Nettoyage de disque à l’aide de la [commande Windows cleanmgr](../../administration/windows-commands/cleanmgr.md) et utiliser des options de ligne de commande pour spécifier de nettoyer certains fichiers.

## <a name="enable-disk-cleanup-on-an-earlier-version-of-windows-server-by-installing-the-desktop-experience"></a>Activer l’outil Nettoyage de disque sur une version antérieure de Windows Server en installant la fonctionnalité Expérience utilisateur

Suivez ces étapes pour utiliser l’Assistant Ajout de rôles et fonctionnalités pour installer la fonctionnalité Expérience utilisateur sur un serveur exécutant Windows Server 2012 R2 ou une version antérieure, ce qui permet aussi d’installer l’outil Nettoyage de disque.

1. Si le Gestionnaire de serveur est déjà ouvert, passez à l’étape suivante. S’il n’est pas déjà ouvert, ouvrez-le en effectuant l’une des opérations suivantes.

   - Sur le Bureau Windows, démarrez le Gestionnaire de serveur en cliquant sur **Gestionnaire de serveur** dans la barre des tâches Windows.

   - Accédez à **Démarrer** et sélectionnez la vignette Gestionnaire de serveur.

1. Dans le menu **Gérer**, sélectionnez **Ajouter des rôles et fonctionnalités**.

1. Dans la page **Avant de commencer**, vérifiez que votre serveur de destination et votre environnement réseau sont préparés pour la fonctionnalité que vous voulez installer. Sélectionnez **Suivant**.

1. Dans la page **Sélectionner le type d’installation**, sélectionnez **Installation basée sur un rôle ou une fonctionnalité** pour installer toutes les fonctionnalités sur un seul serveur. Sélectionnez **Suivant**.

1. Dans la page **Sélectionner le serveur de destination** , sélectionnez un serveur dans le pool de serveurs ou sélectionnez un disque dur virtuel hors connexion. Sélectionnez **Suivant**.

1. Dans la page **Sélectionner des rôles serveurs**, sélectionnez **Suivant**.

1. Dans la page **Sélectionner des fonctionnalités**, sélectionnez **Interface utilisateur et infrastructure**, puis sélectionnez **Expérience utilisateur**.

1. Dans **Ajouter des fonctionnalités requises pour l’expérience utilisateur ?** , sélectionnez **Ajouter des fonctionnalités**.

1. Poursuivez l’installation, puis redémarrez le système.

1. Vérifiez que la case d’option **Nettoyage de disque** apparaît dans la boîte de dialogue Propriétés.

   ![Boîte de dialogue Propriétés du disque avec l’option Nettoyage de disque](media/diskpropswcleanup.png)

## <a name="manually-add-disk-cleanup-to-an-earlier-version-of-windows-server"></a>Ajouter manuellement l’outil Nettoyage de disque à une version antérieure de Windows Server

L’outil Nettoyage de disque (cleanmgr.exe) n’est pas présent sur Windows Server 2012 R2 ou version antérieure, sauf si la fonctionnalité Expérience utilisateur est installée.

Pour utiliser cleanmgr.exe, installez la fonctionnalité Expérience utilisateur comme décrit précédemment, ou bien copiez deux fichiers déjà présents sur le serveur, cleanmgr.exe et cleanmgr.exe.mui. Utilisez le tableau suivant pour trouver les fichiers appropriés à votre système d’exploitation.

| Système d'exploitation  | Architecture  | Emplacement du fichier  |
| ----------------- | -------------- | --------------- |
| Windows Server 2008 R2 | 64 bits | C:\Windows\winsxs\amd64_microsoft-windows-cleanmgr_31bf3856ad364e35_6.1.7600.16385_none_c9392808773cd7da\cleanmgr.exe 
| Windows Server 2008 R2 | 64 bits | C:\Windows\winsxs\amd64_microsoft-windows-cleanmgr.resources_31bf3856ad364e35_6.1.7600.16385_en-us_b9cb6194b257cc63\cleanmgr.exe.mui |

Trouvez cleanmgr.exe et déplacez le fichier vers **%systemroot%\System32**.

Trouvez cleanmgr.exe.mui et déplacez les fichiers vers **%systemroot%\System32\en-US**.

Vous pouvez maintenant lancer l’outil Nettoyage de disque en exécutant Cleanmgr.exe à partir de l’invite de commandes ou en cliquant sur **Démarrer** et en tapant **Cleanmgr** dans la barre de recherche.

Pour que le bouton Nettoyage de disque apparaisse dans la boîte de dialogue Propriétés d’un disque, vous devez également installer la fonctionnalité Expérience utilisateur.

## <a name="additional-references"></a>Références supplémentaires

[Libérer de l’espace disque dans Windows 10](https://support.microsoft.com/help/12425/windows-10-free-up-drive-space)

[cleanmgr](../../administration/windows-commands/cleanmgr.md)
