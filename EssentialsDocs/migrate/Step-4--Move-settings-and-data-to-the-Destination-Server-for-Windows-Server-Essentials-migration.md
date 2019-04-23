---
title: 'Étape 4 : Déplacer les paramètres et données vers le serveur de destination pour la migration vers Windows Server Essentials'
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e143df43-e227-4629-a4ab-9f70d9bf6e84
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e5a8db44f80c333d589e0c1664174c394701f90d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835680"
---
# <a name="step-4-move-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Étape 4 : Déplacer les paramètres et données vers le serveur de destination pour la migration vers Windows Server Essentials

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Cette section fournit des informations sur la migration des paramètres et données à partir du serveur source. Déplacez les paramètres et les données vers le serveur de destination comme suit :  
  
-   [Copier des données vers le serveur de Destination](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_CopyData)  
  
-   [Configurer le réseau](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_Network)  
  
-   [Mapper les ordinateurs autorisés aux comptes d’utilisateurs](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MapPermittedComputers)  
  
##  <a name="BKMK_CopyData"></a> Copier des données vers le serveur de Destination  
 Avant de copier des données du serveur source vers le serveur de destination, effectuez les tâches suivantes :  
  
-   Passez en revue la liste des dossiers partagés sur le serveur source, y compris les autorisations de chaque dossier. Créez ou personnalisez les dossiers sur le serveur de destination afin qu'ils correspondent à la structure de dossiers que vous migrez depuis le serveur source.  
  
-   Passez en revue la taille de chaque dossier et vérifiez que le serveur de destination dispose de suffisamment d'espace de stockage.  
  
-   Mettez en lecture seule les dossiers partagés sur le serveur source pour tous les utilisateurs afin qu'aucune écriture ne puisse être réalisée sur le disque dur pendant que vous copiez les fichiers vers le serveur de destination.  
  
-   Le dossier **Sauvegarde d'un ordinateur client** ne peut pas être migré vers le serveur de destination. Avant la migration du serveur, vérifiez que tous les ordinateurs clients sont intègres. Après la migration du serveur, nous vous recommandons de configurer et de démarrer les sauvegardes de l'ordinateur client pour vérifier que les données sont sauvegardées pour tous vos ordinateurs clients importants.  
  
-   Le **les sauvegardes de l’historique de fichier** dossier ne peut pas être directement migré vers le serveur de Destination en raison des sauvegarde et la structure de métadonnées modifications apportées au dossier dans Windows Server Essentials. Cependant, il est possible de migrer le dossier **Sauvegardes de l'Historique des fichiers** pour un utilisateur précis sur un ordinateur spécifique. Pour ce faire, vous devez rechercher le dossier **Données** dans le dossier **Sauvegardes de l'Historique des fichiers** pour cet utilisateur et cet ordinateur, puis copiez ce dossier **Données** dans le dossier **Sauvegardes de l'Historique des fichiers** sur le serveur de destination.  
  
#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>Pour copier les données du serveur source vers le serveur de destination  
  
1.  Connectez-vous au serveur de destination en tant qu'administrateur de domaine, puis ouvrez une fenêtre d'invite de commandes ou une invite de commandes Windows PowerShell.  
  
2.  Si vous utilisez la fenêtre d'invite de commandes, tapez la commande suivante et appuyez sur ENTRÉE :  
  
    `robocopy \\<SourceServerName>\<SharedSourceFolderName> "<PathOfTheDestination>\<SharedDestinationFolderName>" /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`
  
     Où :  
  
    -   \<Nom_serveur_source\> est le nom du serveur Source  
  
    -   \<Nomdossiersourcepartagé\> est le nom du dossier partagé sur le serveur Source  
  
    -   \<PathOfTheDestination\> est le chemin d’accès absolu où vous souhaitez déplacer le dossier  
  
    -   \<Nomdossierdestinationpartagé\> est le dossier sur le serveur de Destination vers lequel les données seront copiées  
  
     Par exemple,  `robocopy \\sourceserver\MyData "d:\ServerFolders\MyData" /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`.  
  
3.  Si vous utilisez Windows PowerShell, tapez la commande suivante et appuyez sur ENTRÉE.  
  
     `Add-Wssfolder  Path \ -Name  -KeepPermission`  
  
4.  Répétez ce processus pour chaque dossier partagé que vous migrez depuis le serveur source.  
  
##  <a name="BKMK_Network"></a> Configurer le réseau  
  
#### <a name="to-configure-the-network"></a>Pour configurer le réseau  
  
1.  Sur le serveur de destination, ouvrez le tableau de bord.  
  
2.  Dans la page **Accueil** du tableau de bord, cliquez sur **Configuration**, sur **Configurer l'Accès en tout lieu**, puis choisissez l'option **Cliquez pour configurer l'Accès en tout lieu** .  
  
3.  L'Assistant Configuration de l'Accès en tout lieu s'affiche. Suivez les instructions de l'Assistant pour configurer votre routeur et les noms de domaine.  
  
 Si votre routeur ne prend pas en charge l'infrastructure UPnP, ou si l'infrastructure UPnP est désactivée, une icône d'avertissement jaune peut apparaître en regard du nom du routeur. Vérifiez que les ports suivants sont ouverts et qu'ils sont dirigés vers l'adresse IP du serveur de destination :  
  
-   Port 80 : Trafic Web HTTP  
  
-   Port 443 : Trafic Web HTTPS  
  
> [!NOTE]
>  Si vous voulez configurer un nom de domaine public sur le serveur de destination, vous devez libérer le nom de domaine de votre serveur source pour éviter la concurrence de mise à jour DNS dynamique.  
  
##  <a name="BKMK_MapPermittedComputers"></a> Mapper les ordinateurs autorisés aux comptes d’utilisateurs  
 Chaque compte d'utilisateur qui est migré à partir de versions précédentes de Windows Small Business Server ou Windows Server Essentials doit être mappé à un ou plusieurs ordinateurs.  
  
#### <a name="to-map-user-accounts-to-computers"></a>Pour mapper les comptes d'utilisateur aux ordinateurs  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **Utilisateurs**.  
  
3.  Dans la liste des comptes d'utilisateur, cliquez avec le bouton droit sur un compte d'utilisateur, puis cliquez sur **Afficher les propriétés du compte**.  
  
4.  Cliquez sur l'onglet **Accès en tout lieu**, puis sur **Autoriser l'accès web à distance et l'accès aux applications de services web**.  
  
5.  Cliquez sur **Dossiers partagés**,**Ordinateurs**, puis **Liens vers la page d'accueil**et enfin sur **Appliquer**.  
  
6.  Cliquez sur l'onglet **Accès à l'ordinateur**, puis sur le nom de l'ordinateur auquel vous souhaitez autoriser l'accès.  
  
7.  Répétez les étapes 3, 4, 5 et 6 pour chaque compte d'utilisateur.  
  
> [!NOTE]
>  Vous n'avez pas besoin de modifier la configuration de l'ordinateur client. Il est configuré automatiquement.  
  
> [!NOTE]
>  Après avoir effectué la migration, si vous rencontrez un problème quand vous créez le premier compte d'utilisateur sur le serveur de destination, supprimez le compte d'utilisateur que vous avez ajouté, puis recréez-le.  
  
## <a name="next-steps"></a>Étapes suivantes  
 Vous avez déplacé les paramètres et données vers le serveur de destination. Passez maintenant à [étape 5 : Activer la redirection de dossiers sur la migration du serveur de Destination pour Windows Server Essentials](Step-5--Enable-folder-redirection-on-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  
  

Pour afficher toutes les étapes, consultez [migrer vers Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

